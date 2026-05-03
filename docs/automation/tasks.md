---
source_url: https://docs.openclaw.ai/automation/tasks
title: "Background tasks - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/automation/tasks#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Automation and tasks

Background tasks

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [TL;DR](https://docs.openclaw.ai/automation/tasks#tldr)
- [Quick start](https://docs.openclaw.ai/automation/tasks#quick-start)
- [What creates a task](https://docs.openclaw.ai/automation/tasks#what-creates-a-task)
- [Task lifecycle](https://docs.openclaw.ai/automation/tasks#task-lifecycle)
- [Delivery and notifications](https://docs.openclaw.ai/automation/tasks#delivery-and-notifications)
- [Notification policies](https://docs.openclaw.ai/automation/tasks#notification-policies)
- [CLI reference](https://docs.openclaw.ai/automation/tasks#cli-reference)
- [Chat task board (/tasks)](https://docs.openclaw.ai/automation/tasks#chat-task-board-%2Ftasks)
- [Status integration (task pressure)](https://docs.openclaw.ai/automation/tasks#status-integration-task-pressure)
- [Storage and maintenance](https://docs.openclaw.ai/automation/tasks#storage-and-maintenance)
- [Where tasks live](https://docs.openclaw.ai/automation/tasks#where-tasks-live)
- [Automatic maintenance](https://docs.openclaw.ai/automation/tasks#automatic-maintenance)
- [How tasks relate to other systems](https://docs.openclaw.ai/automation/tasks#how-tasks-relate-to-other-systems)
- [Related](https://docs.openclaw.ai/automation/tasks#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Looking for scheduling? See [Automation and tasks](https://docs.openclaw.ai/automation) for choosing the right mechanism. This page is the activity ledger for background work, not the scheduler.

Background tasks track work that runs **outside your main conversation session**: ACP runs, subagent spawns, isolated cron job executions, and CLI-initiated operations.Tasks do **not** replace sessions, cron jobs, or heartbeats — they are the **activity ledger** that records what detached work happened, when, and whether it succeeded.

Not every agent run creates a task. Heartbeat turns and normal interactive chat do not. All cron executions, ACP spawns, subagent spawns, and CLI agent commands do.

## [​](https://docs.openclaw.ai/automation/tasks\#tldr)  TL;DR

- Tasks are **records**, not schedulers — cron and heartbeat decide _when_ work runs, tasks track _what happened_.
- ACP, subagents, all cron jobs, and CLI operations create tasks. Heartbeat turns do not.
- Each task moves through `queued → running → terminal` (succeeded, failed, timed\_out, cancelled, or lost).
- Cron tasks stay live while the cron runtime still owns the job; if the
in-memory runtime state is gone, task maintenance first checks durable cron
run history before marking a task lost.
- Completion is push-driven: detached work can notify directly or wake the
requester session/heartbeat when it finishes, so status polling loops are
usually the wrong shape.
- Isolated cron runs and subagent completions best-effort clean up tracked browser tabs/processes for their child session before final cleanup bookkeeping.
- Isolated cron delivery suppresses stale interim parent replies while descendant subagent work is still draining, and it prefers final descendant output when that arrives before delivery.
- Completion notifications are delivered directly to a channel or queued for the next heartbeat.
- `openclaw tasks list` shows all tasks; `openclaw tasks audit` surfaces issues.
- Terminal records are kept for 7 days, then automatically pruned.

## [​](https://docs.openclaw.ai/automation/tasks\#quick-start)  Quick start

- List and filter

- Inspect

- Cancel and notify

- Audit and maintenance

- Task flow


```
# List all tasks (newest first)
openclaw tasks list

# Filter by runtime or status
openclaw tasks list --runtime acp
openclaw tasks list --status running
```

```
# Show details for a specific task (by ID, run ID, or session key)
openclaw tasks show <lookup>
```

```
# Cancel a running task (kills the child session)
openclaw tasks cancel <lookup>

# Change notification policy for a task
openclaw tasks notify <lookup> state_changes
```

```
# Run a health audit
openclaw tasks audit

# Preview or apply maintenance
openclaw tasks maintenance
openclaw tasks maintenance --apply
```

```
# Inspect TaskFlow state
openclaw tasks flow list
openclaw tasks flow show <lookup>
openclaw tasks flow cancel <lookup>
```

## [​](https://docs.openclaw.ai/automation/tasks\#what-creates-a-task)  What creates a task

| Source | Runtime type | When a task record is created | Default notify policy |
| --- | --- | --- | --- |
| ACP background runs | `acp` | Spawning a child ACP session | `done_only` |
| Subagent orchestration | `subagent` | Spawning a subagent via `sessions_spawn` | `done_only` |
| Cron jobs (all types) | `cron` | Every cron execution (main-session and isolated) | `silent` |
| CLI operations | `cli` | `openclaw agent` commands that run through the gateway | `silent` |
| Agent media jobs | `cli` | Session-backed `music_generate`/`video_generate` runs | `silent` |

Notify defaults for cron and media

Main-session cron tasks use `silent` notify policy by default — they create records for tracking but do not generate notifications. Isolated cron tasks also default to `silent` but are more visible because they run in their own session.Session-backed `music_generate` and `video_generate` runs also use `silent` notify policy. They still create task records, but completion is handed back to the original agent session as an internal wake so the agent can write the follow-up message and attach the finished media itself. If you opt into `tools.media.asyncCompletion.directSend`, async `video_generate` completions can try direct channel delivery first; async `music_generate` completions stay on the requester-session wake path.

Concurrent video\_generate guardrail

While a session-backed `video_generate` task is still active, the tool also acts as a guardrail: repeated `video_generate` calls in that same session return the active task status instead of starting a second concurrent generation. Use `action: "status"` when you want an explicit progress/status lookup from the agent side.

What does not create tasks

- Heartbeat turns — main-session; see [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat)
- Normal interactive chat turns
- Direct `/command` responses

## [​](https://docs.openclaw.ai/automation/tasks\#task-lifecycle)  Task lifecycle

agent starts

completes ok

error

timeout exceeded

operator cancels

session gone > 5 min

session gone > 5 min

queued

running

succeeded

failed

timed\_out

cancelled

lost

| Status | What it means |
| --- | --- |
| `queued` | Created, waiting for the agent to start |
| `running` | Agent turn is actively executing |
| `succeeded` | Completed successfully |
| `failed` | Completed with an error |
| `timed_out` | Exceeded the configured timeout |
| `cancelled` | Stopped by the operator via `openclaw tasks cancel` |
| `lost` | The runtime lost authoritative backing state after a 5-minute grace period |

Transitions happen automatically — when the associated agent run ends, the task status updates to match.Agent run completion is authoritative for active task records. A successful detached run finalizes as `succeeded`, ordinary run errors finalize as `failed`, and timeout or abort outcomes finalize as `timed_out`. If an operator already cancelled the task, or the runtime already recorded a stronger terminal state such as `failed`, `timed_out`, or `lost`, a later success signal does not downgrade that terminal status.`lost` is runtime-aware:

- ACP tasks: backing ACP child session metadata disappeared.
- Subagent tasks: backing child session disappeared from the target agent store.
- Cron tasks: the cron runtime no longer tracks the job as active and durable
cron run history does not show a terminal result for that run. Offline CLI
audit does not treat its own empty in-process cron runtime state as authority.
- CLI tasks: isolated child-session tasks use the child session; chat-backed
CLI tasks use the live run context instead, so lingering
channel/group/direct session rows do not keep them alive. Gateway-backed
`openclaw agent` runs also finalize from their run result, so completed runs
do not sit active until the sweeper marks them `lost`.

## [​](https://docs.openclaw.ai/automation/tasks\#delivery-and-notifications)  Delivery and notifications

When a task reaches a terminal state, OpenClaw notifies you. There are two delivery paths:**Direct delivery** — if the task has a channel target (the `requesterOrigin`), the completion message goes straight to that channel (Telegram, Discord, Slack, etc.). For subagent completions, OpenClaw also preserves bound thread/topic routing when available and can fill a missing `to` / account from the requester session’s stored route (`lastChannel` / `lastTo` / `lastAccountId`) before giving up on direct delivery.**Session-queued delivery** — if direct delivery fails or no origin is set, the update is queued as a system event in the requester’s session and surfaces on the next heartbeat.

Task completion triggers an immediate heartbeat wake so you see the result quickly — you do not have to wait for the next scheduled heartbeat tick.

That means the usual workflow is push-based: start detached work once, then let the runtime wake or notify you on completion. Poll task state only when you need debugging, intervention, or an explicit audit.

### [​](https://docs.openclaw.ai/automation/tasks\#notification-policies)  Notification policies

Control how much you hear about each task:

| Policy | What is delivered |
| --- | --- |
| `done_only` (default) | Only terminal state (succeeded, failed, etc.) — **this is the default** |
| `state_changes` | Every state transition and progress update |
| `silent` | Nothing at all |

Change the policy while a task is running:

```
openclaw tasks notify <lookup> state_changes
```

## [​](https://docs.openclaw.ai/automation/tasks\#cli-reference)  CLI reference

tasks list

```
openclaw tasks list [--runtime <acp|subagent|cron|cli>] [--status <status>] [--json]
```

Output columns: Task ID, Kind, Status, Delivery, Run ID, Child Session, Summary.

tasks show

```
openclaw tasks show <lookup>
```

The lookup token accepts a task ID, run ID, or session key. Shows the full record including timing, delivery state, error, and terminal summary.

tasks cancel

```
openclaw tasks cancel <lookup>
```

For ACP and subagent tasks, this kills the child session. For CLI-tracked tasks, cancellation is recorded in the task registry (there is no separate child runtime handle). Status transitions to `cancelled` and a delivery notification is sent when applicable.

tasks notify

```
openclaw tasks notify <lookup> <done_only|state_changes|silent>
```

tasks audit

```
openclaw tasks audit [--json]
```

Surfaces operational issues. Findings also appear in `openclaw status` when issues are detected.

| Finding | Severity | Trigger |
| --- | --- | --- |
| `stale_queued` | warn | Queued for more than 10 minutes |
| `stale_running` | error | Running for more than 30 minutes |
| `lost` | warn/error | Runtime-backed task ownership disappeared; retained lost tasks warn until `cleanupAfter`, then become errors |
| `delivery_failed` | warn | Delivery failed and notify policy is not `silent` |
| `missing_cleanup` | warn | Terminal task with no cleanup timestamp |
| `inconsistent_timestamps` | warn | Timeline violation (for example ended before started) |

tasks maintenance

```
openclaw tasks maintenance [--json]
openclaw tasks maintenance --apply [--json]
```

Use this to preview or apply reconciliation, cleanup stamping, and pruning for tasks and Task Flow state.Reconciliation is runtime-aware:

- ACP/subagent tasks check their backing child session.
- Subagent tasks whose child session has a restart-recovery tombstone are marked lost instead of being treated as recoverable backing sessions.
- Cron tasks check whether the cron runtime still owns the job, then recover terminal status from persisted cron run logs/job state before falling back to `lost`. Only the Gateway process is authoritative for the in-memory cron active-job set; offline CLI audit uses durable history but does not mark a cron task lost solely because that local Set is empty.
- Chat-backed CLI tasks check the owning live run context, not just the chat session row.

Completion cleanup is also runtime-aware:

- Subagent completion best-effort closes tracked browser tabs/processes for the child session before announce cleanup continues.
- Isolated cron completion best-effort closes tracked browser tabs/processes for the cron session before the run fully tears down.
- Isolated cron delivery waits out descendant subagent follow-up when needed and suppresses stale parent acknowledgement text instead of announcing it.
- Subagent completion delivery prefers the latest visible assistant text; if that is empty it falls back to sanitized latest tool/toolResult text, and timeout-only tool-call runs can collapse to a short partial-progress summary. Terminal failed runs announce failure status without replaying captured reply text.
- Cleanup failures do not mask the real task outcome.

tasks flow list \| show \| cancel

```
openclaw tasks flow list [--status <status>] [--json]
openclaw tasks flow show <lookup> [--json]
openclaw tasks flow cancel <lookup>
```

Use these when the orchestrating Task Flow is the thing you care about rather than one individual background task record.

## [​](https://docs.openclaw.ai/automation/tasks\#chat-task-board-/tasks)  Chat task board (`/tasks`)

Use `/tasks` in any chat session to see background tasks linked to that session. The board shows active and recently completed tasks with runtime, status, timing, and progress or error detail.When the current session has no visible linked tasks, `/tasks` falls back to agent-local task counts so you still get an overview without leaking other-session details.For the full operator ledger, use the CLI: `openclaw tasks list`.

## [​](https://docs.openclaw.ai/automation/tasks\#status-integration-task-pressure)  Status integration (task pressure)

`openclaw status` includes an at-a-glance task summary:

```
Tasks: 3 queued · 2 running · 1 issues
```

The summary reports:

- **active** — count of `queued` \+ `running`
- **failures** — count of `failed` \+ `timed_out` \+ `lost`
- **byRuntime** — breakdown by `acp`, `subagent`, `cron`, `cli`

Both `/status` and the `session_status` tool use a cleanup-aware task snapshot: active tasks are preferred, stale completed rows are hidden, and recent failures only surface when no active work remains. This keeps the status card focused on what matters right now.

## [​](https://docs.openclaw.ai/automation/tasks\#storage-and-maintenance)  Storage and maintenance

### [​](https://docs.openclaw.ai/automation/tasks\#where-tasks-live)  Where tasks live

Task records persist in SQLite at:

```
$OPENCLAW_STATE_DIR/tasks/runs.sqlite
```

The registry loads into memory at gateway start and syncs writes to SQLite for durability across restarts.
The Gateway keeps the SQLite write-ahead log bounded by using SQLite’s default
autocheckpoint threshold plus periodic and shutdown `TRUNCATE` checkpoints.

### [​](https://docs.openclaw.ai/automation/tasks\#automatic-maintenance)  Automatic maintenance

A sweeper runs every **60 seconds** and handles four things:

1

[Navigate to header](https://docs.openclaw.ai/automation/tasks#)

Reconciliation

Checks whether active tasks still have authoritative runtime backing. ACP/subagent tasks use child-session state, cron tasks use active-job ownership, and chat-backed CLI tasks use the owning run context. If that backing state is gone for more than 5 minutes, the task is marked `lost`.

2

[Navigate to header](https://docs.openclaw.ai/automation/tasks#)

ACP session repair

Closes terminal or orphaned parent-owned one-shot ACP sessions, and closes stale terminal or orphaned persistent ACP sessions only when no active conversation binding remains.

3

[Navigate to header](https://docs.openclaw.ai/automation/tasks#)

Cleanup stamping

Sets a `cleanupAfter` timestamp on terminal tasks (endedAt + 7 days). During retention, lost tasks still appear in audit as warnings; after `cleanupAfter` expires or when cleanup metadata is missing, they are errors.

4

[Navigate to header](https://docs.openclaw.ai/automation/tasks#)

Pruning

Deletes records past their `cleanupAfter` date.

**Retention:** terminal task records are kept for **7 days**, then automatically pruned. No configuration needed.

## [​](https://docs.openclaw.ai/automation/tasks\#how-tasks-relate-to-other-systems)  How tasks relate to other systems

Tasks and Task Flow

[Task Flow](https://docs.openclaw.ai/automation/taskflow) is the flow orchestration layer above background tasks. A single flow may coordinate multiple tasks over its lifetime using managed or mirrored sync modes. Use `openclaw tasks` to inspect individual task records and `openclaw tasks flow` to inspect the orchestrating flow.See [Task Flow](https://docs.openclaw.ai/automation/taskflow) for details.

Tasks and cron

A cron job **definition** lives in `~/.openclaw/cron/jobs.json`; runtime execution state lives beside it in `~/.openclaw/cron/jobs-state.json`. **Every** cron execution creates a task record — both main-session and isolated. Main-session cron tasks default to `silent` notify policy so they track without generating notifications.See [Cron Jobs](https://docs.openclaw.ai/automation/cron-jobs).

Tasks and heartbeat

Heartbeat runs are main-session turns — they do not create task records. When a task completes, it can trigger a heartbeat wake so you see the result promptly.See [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat).

Tasks and sessions

A task may reference a `childSessionKey` (where work runs) and a `requesterSessionKey` (who started it). Sessions are conversation context; tasks are activity tracking on top of that.

Tasks and agent runs

A task’s `runId` links to the agent run doing the work. Agent lifecycle events (start, end, error) automatically update the task status — you do not need to manage the lifecycle manually.

## [​](https://docs.openclaw.ai/automation/tasks\#related)  Related

- [Automation & Tasks](https://docs.openclaw.ai/automation) — all automation mechanisms at a glance
- [CLI: Tasks](https://docs.openclaw.ai/cli/tasks) — CLI command reference
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) — periodic main-session turns
- [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs) — scheduling background work
- [Task Flow](https://docs.openclaw.ai/automation/taskflow) — flow orchestration above tasks

[Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs) [Task flow](https://docs.openclaw.ai/automation/taskflow)

Ctrl+I