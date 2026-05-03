# Automation

_8 pages from docs.openclaw.ai_


---

## Automation & tasks - OpenClaw

_Source: <https://docs.openclaw.ai/automation>_

[OpenClaw home page](https://docs.openclaw.ai/)

Automation and tasks

Automation & tasks

OpenClaw runs work in the background through tasks, scheduled jobs, inferred
commitments, event hooks, and standing instructions. This page helps you choose
the right mechanism and understand how they fit together.

## Quick decision guide

Yes

Exact

Flexible

Yes

Yes

Yes

Yes

Yes

What do you need?

Schedule work?

Track detached work?

Orchestrate multi-step flows?

React to lifecycle events?

Give the agent persistent instructions?

Remember a natural follow-up?

Exact timing or flexible?

Scheduled Tasks (Cron)

Heartbeat

Background Tasks

Task Flow

Hooks

Standing Orders

Inferred Commitments

| Use case | Recommended | Why |
| --- | --- | --- |
| Send daily report at 9 AM sharp | Scheduled Tasks (Cron) | Exact timing, isolated execution |
| Remind me in 20 minutes | Scheduled Tasks (Cron) | One-shot with precise timing (`--at`) |
| Run weekly deep analysis | Scheduled Tasks (Cron) | Standalone task, can use different model |
| Check inbox every 30 min | Heartbeat | Batches with other checks, context-aware |
| Monitor calendar for upcoming events | Heartbeat | Natural fit for periodic awareness |
| Check in after a mentioned interview | Inferred Commitments | Memory-like follow-up, no exact reminder request |
| Gentle care check-in after user context | Inferred Commitments | Scoped to the same agent and channel |
| Inspect status of a subagent or ACP run | Background Tasks | Tasks ledger tracks all detached work |
| Audit what ran and when | Background Tasks | `openclaw tasks list` and `openclaw tasks audit` |
| Multi-step research then summarize | Task Flow | Durable orchestration with revision tracking |
| Run a script on session reset | Hooks | Event-driven, fires on lifecycle events |
| Execute code on every tool call | Plugin hooks | In-process hooks can intercept tool calls |
| Always check compliance before replying | Standing Orders | Injected into every session automatically |

### Scheduled Tasks (Cron) vs Heartbeat

| Dimension | Scheduled Tasks (Cron) | Heartbeat |
| --- | --- | --- |
| Timing | Exact (cron expressions, one-shot) | Approximate (default every 30 min) |
| Session context | Fresh (isolated) or shared | Full main-session context |
| Task records | Always created | Never created |
| Delivery | Channel, webhook, or silent | Inline in main session |
| Best for | Reports, reminders, background jobs | Inbox checks, calendar, notifications |

Use Scheduled Tasks (Cron) when you need precise timing or isolated execution. Use Heartbeat when the work benefits from full session context and approximate timing is fine.

## Core concepts

### Scheduled tasks (cron)

Cron is the Gateway’s built-in scheduler for precise timing. It persists jobs, wakes the agent at the right time, and can deliver output to a chat channel or webhook endpoint. Supports one-shot reminders, recurring expressions, and inbound webhook triggers.See [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs).

### Tasks

The background task ledger tracks all detached work: ACP runs, subagent spawns, isolated cron executions, and CLI operations. Tasks are records, not schedulers. Use `openclaw tasks list` and `openclaw tasks audit` to inspect them.See [Background Tasks](https://docs.openclaw.ai/automation/tasks).

### Inferred commitments

Commitments are opt-in, short-lived follow-up memories. OpenClaw infers them
from normal conversations, scopes them to the same agent and channel, and
delivers due check-ins through heartbeat. Exact user-requested reminders still
belong to cron.See [Inferred Commitments](https://docs.openclaw.ai/concepts/commitments).

### Task Flow

Task Flow is the flow orchestration substrate above background tasks. It manages durable multi-step flows with managed and mirrored sync modes, revision tracking, and `openclaw tasks flow list|show|cancel` for inspection.See [Task Flow](https://docs.openclaw.ai/automation/

_… [truncated; see https://docs.openclaw.ai/automation for full content]_


---

## Scheduled tasks - OpenClaw

_Source: <https://docs.openclaw.ai/automation/cron-jobs>_

# Intended: "9 AM on the 15th, only if it's a Monday"
# Actual:   "9 AM on every 15th, AND 9 AM on every Monday"
0 9 15 * 1
```

This fires ~5–6 times per month instead of 0–1 times per month. OpenClaw uses Croner’s default OR behavior here. To require both conditions, use Croner’s `+` day-of-week modifier (`0 9 15 * +1`) or schedule on one field and guard the other in your job’s prompt or command.

## Execution styles

| Style | `--session` value | Runs in | Best for |
| --- | --- | --- | --- |
| Main session | `main` | Next heartbeat turn | Reminders, system events |
| Isolated | `isolated` | Dedicated `cron:<jobId>` | Reports, background chores |
| Current session | `current` | Bound at creation time | Context-aware recurring work |
| Custom session | `session:custom-id` | Persistent named session | Workflows that build on history |

Main session vs isolated vs custom

**Main session** jobs enqueue a system event and optionally wake the heartbeat (`--wake now` or `--wake next-heartbeat`). Those system events do not extend daily/idle reset freshness for the target session. **Isolated** jobs run a dedicated agent turn with a fresh session. **Custom sessions** (`session:xxx`) persist context across runs, enabling workflows like daily standups that build on previous summaries.

What 'fresh session' means for isolated jobs

For isolated jobs, “fresh session” means a new transcript/session id for each run. OpenClaw may carry safe preferences such as thinking/fast/verbose settings, labels, and explicit user-selected model/auth overrides, but it does not inherit ambient conversation context from an older cron row: channel/group routing, send or queue policy, elevation, origin, or ACP runtime binding. Use `current` or `session:<id>` when a recurring job should deliberately build on the same conversation context.

Runtime cleanup

For isolated jobs, runtime teardown now includes best-effort browser cleanup for that cron session. Cleanup failures are ignored so the actual cron result still wins.Isolated cron runs also dispose any bundled MCP runtime instances created for the job through the shared runtime-cleanup path. This matches how main-session and custom-session MCP clients are torn down, so isolated cron jobs do not leak stdio child processes or long-lived MCP connections across runs.

Subagent and Discord delivery

When isolated cron runs orchestrate subagents, delivery also prefers the final descendant output over stale parent interim text. If descendants are still running, OpenClaw suppresses that partial parent update instead of announcing it.For text-only Discord announce targets, OpenClaw sends the canonical final assistant text once instead of replaying both streamed/intermediate text payloads and the final answer. Media and structured Discord payloads are still delivered as separate payloads so attachments and components are not dropped.

### Payload options for isolated jobs

[​](https://docs.openclaw.ai/automation/cron-jobs#param-message)

--message

string

required

Prompt text (required for isolated).

[​](https://docs.openclaw.ai/automation/cron-jobs#param-model)

--model

string

Model override; uses the selected allowed model for the job.

[​](https://docs.openclaw.ai/automation/cron-jobs#param-thinking)

--thinking

string

Thinking level override.

[​](https://docs.openclaw.ai/automation/cron-jobs#param-light-context)

--light-context

boolean

Skip workspace bootstrap file injection.

[​](https://docs.openclaw.ai/automation/cron-jobs#param-tools)

--tools

string

Restrict which tools the job can use, for example `--tools exec,read`.

`--model` uses the selected allowed model as that job’s primary model. It is not the same as a chat-session `/model` override: configured fallback chains still apply when the job primary fails. If the requested model is not allowed or cannot be resolved, cron fails the run with an explicit validation error instead of silently falling back to the job’s agent/default model selectio

_… [truncated; see https://docs.openclaw.ai/automation/cron-jobs for full content]_


---

## https://docs.openclaw.ai/automation/cron-jobs.md

_Source: <https://docs.openclaw.ai/automation/cron-jobs.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Scheduled tasks

Cron is the Gateway's built-in scheduler. It persists jobs, wakes the agent at the right time, and can deliver output back to a chat channel or webhook endpoint.

\## Quick start

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron add \
 --name "Reminder" \
 --at "2026-02-01T16:00:00Z" \
 --session main \
 --system-event "Reminder: check the cron docs draft" \
 --wake now \
 --delete-after-run
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron list
 openclaw cron show
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron runs --id
 \`\`\`

\## How cron works

\\* Cron runs \*\*inside the Gateway\*\* process (not inside the model).
\\* Job definitions persist at \`~/.openclaw/cron/jobs.json\` so restarts do not lose schedules.
\\* Runtime execution state persists next to it in \`~/.openclaw/cron/jobs-state.json\`. If you track cron definitions in git, track \`jobs.json\` and gitignore \`jobs-state.json\`.
\\* After the split, older OpenClaw versions can read \`jobs.json\` but may treat jobs as fresh because runtime fields now live in \`jobs-state.json\`.
\\* When \`jobs.json\` is edited while the Gateway is running or stopped, OpenClaw compares the changed schedule fields with pending runtime slot metadata and clears stale \`nextRunAtMs\` values. Pure formatting or key-order-only rewrites preserve the pending slot.
\\* All cron executions create \[background task\](/automation/tasks) records.
\\* On Gateway startup, overdue isolated agent-turn jobs are rescheduled out of the channel-connect window instead of replaying immediately, so Discord/Telegram startup and native-command setup stay responsive after restarts.
\\* One-shot jobs (\`--at\`) auto-delete after success by default.
\\* Isolated cron runs best-effort close tracked browser tabs/processes for their \`cron:\` session when the run completes, so detached browser automation does not leave orphaned processes behind.
\\* Isolated cron runs also guard against stale acknowledgement replies. If the first result is just an interim status update (\`on it\`, \`pulling everything together\`, and similar hints) and no descendant subagent run is still responsible for the final answer, OpenClaw re-prompts once for the actual result before delivery.
\\* Isolated cron runs prefer structured execution-denial metadata from the embedded run, then fall back to known final summary/output markers such as \`SYSTEM\_RUN\_DENIED\` and \`INVALID\_REQUEST\`, so a blocked command is not reported as a green run.
\\* Isolated cron runs also treat run-level agent failures as job errors even when no reply payload is produced, so model/provider failures increment error counters and trigger failure notifications instead of clearing the job as successful.
\\* When an isolated agent-turn job reaches \`timeoutSeconds\`, cron aborts the underlying agent run and gives it a short cleanup window. If the run does not drain, Gateway-owned cleanup force-clears that run's session ownership before cron records the timeout, so queued chat work is not left behind a stale processing session.

 Task reconciliation for cron is runtime-owned first, durable-history-backed second: an active cron task stays live while the cron runtime still tracks that job as running, even if an old child session row still exists. Once the runtime stops owning the job and the 5-minute grace window expires, maintenance checks persisted run logs and job state for the matching \`cron::\` run. If that durable history shows a terminal result, the task ledger is finalized from it; otherwise Gateway-owned maintenance can mark the task \`lost\`. Offline CLI audit can recover from durable history, but it does not treat its own empty in-process active-job set as pr

_… [truncated; see https://docs.openclaw.ai/automation/cron-jobs.md for full content]_


---

## Hooks - OpenClaw

_Source: <https://docs.openclaw.ai/automation/hooks>_

# List available hooks
openclaw hooks list

# Enable a hook
openclaw hooks enable session-memory

# Check hook status
openclaw hooks check

# Get detailed information
openclaw hooks info session-memory
```

## Event types

| Event | When it fires |
| --- | --- |
| `command:new` | `/new` command issued |
| `command:reset` | `/reset` command issued |
| `command:stop` | `/stop` command issued |
| `command` | Any command event (general listener) |
| `session:compact:before` | Before compaction summarizes history |
| `session:compact:after` | After compaction completes |
| `session:patch` | When session properties are modified |
| `agent:bootstrap` | Before workspace bootstrap files are injected |
| `gateway:startup` | After channels start and hooks are loaded |
| `gateway:shutdown` | When gateway shutdown begins |
| `gateway:pre-restart` | Before an expected gateway restart |
| `message:received` | Inbound message from any channel |
| `message:transcribed` | After audio transcription completes |
| `message:preprocessed` | After media and link preprocessing completes or is skipped |
| `message:sent` | Outbound message delivered |

## Writing hooks

### Hook structure

Each hook is a directory containing two files:

```
my-hook/
├── HOOK.md          # Metadata + documentation
└── handler.ts       # Handler implementation
```

### HOOK.md format

```
---
name: my-hook
description: "Short description of what this hook does"
metadata:
  { "openclaw": { "emoji": "🔗", "events": ["command:new"], "requires": { "bins": ["node"] } } }
---

# My Hook

Detailed documentation goes here.
```

**Metadata fields** (`metadata.openclaw`):

| Field | Description |
| --- | --- |
| `emoji` | Display emoji for CLI |
| `events` | Array of events to listen for |
| `export` | Named export to use (defaults to `"default"`) |
| `os` | Required platforms (e.g., `["darwin", "linux"]`) |
| `requires` | Required `bins`, `anyBins`, `env`, or `config` paths |
| `always` | Bypass eligibility checks (boolean) |
| `install` | Installation methods |

### Handler implementation

```
const handler = async (event) => {
  if (event.type !== "command" || event.action !== "new") {
    return;
  }

  console.log(`[my-hook] New command triggered`);
  // Your logic here

  // Optionally send message to user
  event.messages.push("Hook executed!");
};

export default handler;
```

Each event includes: `type`, `action`, `sessionKey`, `timestamp`, `messages` (push to send to user), and `context` (event-specific data). Agent and tool plugin hook contexts can also include `trace`, a read-only W3C-compatible diagnostic trace context that plugins may pass into structured logs for OTEL correlation.

### Event context highlights

**Command events** (`command:new`, `command:reset`): `context.sessionEntry`, `context.previousSessionEntry`, `context.commandSource`, `context.workspaceDir`, `context.cfg`.**Message events** (`message:received`): `context.from`, `context.content`, `context.channelId`, `context.metadata` (provider-specific data including `senderId`, `senderName`, `guildId`).**Message events** (`message:sent`): `context.to`, `context.content`, `context.success`, `context.channelId`.**Message events** (`message:transcribed`): `context.transcript`, `context.from`, `context.channelId`, `context.mediaPath`.**Message events** (`message:preprocessed`): `context.bodyForAgent` (final enriched body), `context.from`, `context.channelId`.**Bootstrap events** (`agent:bootstrap`): `context.bootstrapFiles` (mutable array), `context.agentId`.**Session patch events** (`session:patch`): `context.sessionEntry`, `context.patch` (only changed fields), `context.cfg`. Only privileged clients can trigger patch events.**Compaction events**: `session:compact:before` includes `messageCount`, `tokenCount`. `session:compact:after` adds `compactedCount`, `summaryLength`, `tokensBefore`, `tokensAfter`.`command:stop` observes the user issuing `/stop`; it is cancellation/command
lifecycle, not an agent-finaliza

_… [truncated; see https://docs.openclaw.ai/automation/hooks for full content]_


---

## Automation & tasks - OpenClaw

_Source: <https://docs.openclaw.ai/automation/index>_

[OpenClaw home page](https://docs.openclaw.ai/)

Automation and tasks

Automation & tasks

OpenClaw runs work in the background through tasks, scheduled jobs, inferred
commitments, event hooks, and standing instructions. This page helps you choose
the right mechanism and understand how they fit together.

## Quick decision guide

Yes

Exact

Flexible

Yes

Yes

Yes

Yes

Yes

What do you need?

Schedule work?

Track detached work?

Orchestrate multi-step flows?

React to lifecycle events?

Give the agent persistent instructions?

Remember a natural follow-up?

Exact timing or flexible?

Scheduled Tasks (Cron)

Heartbeat

Background Tasks

Task Flow

Hooks

Standing Orders

Inferred Commitments

| Use case | Recommended | Why |
| --- | --- | --- |
| Send daily report at 9 AM sharp | Scheduled Tasks (Cron) | Exact timing, isolated execution |
| Remind me in 20 minutes | Scheduled Tasks (Cron) | One-shot with precise timing (`--at`) |
| Run weekly deep analysis | Scheduled Tasks (Cron) | Standalone task, can use different model |
| Check inbox every 30 min | Heartbeat | Batches with other checks, context-aware |
| Monitor calendar for upcoming events | Heartbeat | Natural fit for periodic awareness |
| Check in after a mentioned interview | Inferred Commitments | Memory-like follow-up, no exact reminder request |
| Gentle care check-in after user context | Inferred Commitments | Scoped to the same agent and channel |
| Inspect status of a subagent or ACP run | Background Tasks | Tasks ledger tracks all detached work |
| Audit what ran and when | Background Tasks | `openclaw tasks list` and `openclaw tasks audit` |
| Multi-step research then summarize | Task Flow | Durable orchestration with revision tracking |
| Run a script on session reset | Hooks | Event-driven, fires on lifecycle events |
| Execute code on every tool call | Plugin hooks | In-process hooks can intercept tool calls |
| Always check compliance before replying | Standing Orders | Injected into every session automatically |

### Scheduled Tasks (Cron) vs Heartbeat

| Dimension | Scheduled Tasks (Cron) | Heartbeat |
| --- | --- | --- |
| Timing | Exact (cron expressions, one-shot) | Approximate (default every 30 min) |
| Session context | Fresh (isolated) or shared | Full main-session context |
| Task records | Always created | Never created |
| Delivery | Channel, webhook, or silent | Inline in main session |
| Best for | Reports, reminders, background jobs | Inbox checks, calendar, notifications |

Use Scheduled Tasks (Cron) when you need precise timing or isolated execution. Use Heartbeat when the work benefits from full session context and approximate timing is fine.

## Core concepts

### Scheduled tasks (cron)

Cron is the Gateway’s built-in scheduler for precise timing. It persists jobs, wakes the agent at the right time, and can deliver output to a chat channel or webhook endpoint. Supports one-shot reminders, recurring expressions, and inbound webhook triggers.See [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs).

### Tasks

The background task ledger tracks all detached work: ACP runs, subagent spawns, isolated cron executions, and CLI operations. Tasks are records, not schedulers. Use `openclaw tasks list` and `openclaw tasks audit` to inspect them.See [Background Tasks](https://docs.openclaw.ai/automation/tasks).

### Inferred commitments

Commitments are opt-in, short-lived follow-up memories. OpenClaw infers them
from normal conversations, scopes them to the same agent and channel, and
delivers due check-ins through heartbeat. Exact user-requested reminders still
belong to cron.See [Inferred Commitments](https://docs.openclaw.ai/concepts/commitments).

### Task Flow

Task Flow is the flow orchestration substrate above background tasks. It manages durable multi-step flows with managed and mirrored sync modes, revision tracking, and `openclaw tasks flow list|show|cancel` for inspection.See [Task Flow](https://docs.openclaw.ai/automation/

_… [truncated; see https://docs.openclaw.ai/automation/index for full content]_


---

## Background tasks - OpenClaw

_Source: <https://docs.openclaw.ai/automation/tasks>_

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

## What creates a task

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

## Task lifecycle

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
- Cron tasks: the cron runtime no longer tracks the

_… [truncated; see https://docs.openclaw.ai/automation/tasks for full content]_


---

## Cron - OpenClaw

_Source: <https://docs.openclaw.ai/cli/cron>_

# `openclaw cron`

Manage cron jobs for the Gateway scheduler.

Run `openclaw cron --help` for the full command surface. See [Cron jobs](https://docs.openclaw.ai/automation/cron-jobs) for the conceptual guide.

## Sessions

`--session` accepts `main`, `isolated`, `current`, or `session:<id>`.

Session keys

- `main` binds to the agent’s main session.
- `isolated` creates a fresh transcript and session id for each run.
- `current` binds to the active session at creation time.
- `session:<id>` pins to an explicit persistent session key.

Isolated session semantics

Isolated runs reset ambient conversation context. Channel and group routing, send/queue policy, elevation, origin, and ACP runtime binding are reset for the new run. Safe preferences and explicit user-selected model or auth overrides can carry across runs.

## Delivery

`openclaw cron list` and `openclaw cron show <job-id>` preview the resolved delivery route. For `channel: "last"`, the preview shows whether the route resolved from the main or current session, or will fail closed.Provider-prefixed targets can disambiguate unresolved announce channels. For example, `to: "telegram:123"` selects Telegram when `delivery.channel` is omitted or `last`. Only prefixes advertised by the loaded plugin are provider selectors. If `delivery.channel` is explicit, the prefix must match that channel; `channel: "whatsapp"` with `to: "telegram:123"` is rejected. Service prefixes such as `imessage:` and `sms:` remain channel-owned target syntax.

Isolated `cron add` jobs default to `--announce` delivery. Use `--no-deliver` to keep output internal. `--deliver` remains as a deprecated alias for `--announce`.

### Delivery ownership

Isolated cron chat delivery is shared between the agent and the runner:

- The agent can send directly using the `message` tool when a chat route is available.
- `announce` fallback-delivers the final reply only when the agent did not send directly to the resolved target.
- `webhook` posts the finished payload to a URL.
- `none` disables runner fallback delivery.

`--announce` is runner fallback delivery for the final reply. `--no-deliver` disables that fallback but does not remove the agent’s `message` tool when a chat route is available.Reminders created from an active chat preserve the live chat delivery target for fallback announce delivery. Internal session keys may be lowercase; do not use them as a source of truth for case-sensitive provider IDs such as Matrix room IDs.

### Failure delivery

Failure notifications resolve in this order:

1. `delivery.failureDestination` on the job.
2. Global `cron.failureDestination`.
3. The job’s primary announce target (when no explicit failure destination is set).

Main-session jobs may only use `delivery.failureDestination` when primary delivery mode is `webhook`. Isolated jobs accept it in all modes.

Note: isolated cron runs treat run-level agent failures as job errors even when
no reply payload is produced, so model/provider failures still increment error
counters and trigger failure notifications.

## Scheduling

### One-shot jobs

`--at <datetime>` schedules a one-shot run. Offset-less datetimes are treated as UTC unless you also pass `--tz <iana>`, which interprets the wall-clock time in the given timezone.

One-shot jobs delete after success by default. Use `--keep-after-run` to preserve them.

### Recurring jobs

Recurring jobs use exponential retry backoff after consecutive errors: 30s, 1m, 5m, 15m, 60m. The schedule returns to normal after the next successful run.Skipped runs are tracked separately from execution errors. They do not affect retry backoff, but `openclaw cron edit <job-id> --failure-alert-include-skipped` can opt failure alerts into repeated skipped-run notifications.For isolated jobs that target a local configured model provider, cron runs a lightweight provider preflight before starting the agent turn. Loopback, private-network, and `.local``api: "ollama"` providers are probed at `/api/tags

_… [truncated; see https://docs.openclaw.ai/cli/cron for full content]_


---

## Hooks - OpenClaw

_Source: <https://docs.openclaw.ai/cli/hooks>_

# `openclaw hooks`

Manage agent hooks (event-driven automations for commands like `/new`, `/reset`, and gateway startup).Running `openclaw hooks` with no subcommand is equivalent to `openclaw hooks list`.Related:

- Hooks: [Hooks](https://docs.openclaw.ai/automation/hooks)
- Plugin hooks: [Plugin hooks](https://docs.openclaw.ai/plugins/hooks)

## List all hooks

```
openclaw hooks list
```

List all discovered hooks from workspace, managed, extra, and bundled directories.
Gateway startup does not load internal hook handlers until at least one internal hook is configured.**Options:**

- `--eligible`: Show only eligible hooks (requirements met)
- `--json`: Output as JSON
- `-v, --verbose`: Show detailed information including missing requirements

**Example output:**

```
Hooks (4/4 ready)

Ready:
  🚀 boot-md ✓ - Run BOOT.md on gateway startup
  📎 bootstrap-extra-files ✓ - Inject extra workspace bootstrap files during agent bootstrap
  📝 command-logger ✓ - Log all command events to a centralized audit file
  💾 session-memory ✓ - Save session context to memory when /new or /reset command is issued
```

**Example (verbose):**

```
openclaw hooks list --verbose
```

Shows missing requirements for ineligible hooks.**Example (JSON):**

```
openclaw hooks list --json
```

Returns structured JSON for programmatic use.

## Get hook information

```
openclaw hooks info <name>
```

Show detailed information about a specific hook.**Arguments:**

- `<name>`: Hook name or hook key (e.g., `session-memory`)

**Options:**

- `--json`: Output as JSON

**Example:**

```
openclaw hooks info session-memory
```

**Output:**

```
💾 session-memory ✓ Ready

Save session context to memory when /new or /reset command is issued

Details:
  Source: openclaw-bundled
  Path: /path/to/openclaw/hooks/bundled/session-memory/HOOK.md
  Handler: /path/to/openclaw/hooks/bundled/session-memory/handler.ts
  Homepage: https://docs.openclaw.ai/automation/hooks#session-memory
  Events: command:new, command:reset

Requirements:
  Config: ✓ workspace.dir
```

## Check hooks eligibility

```
openclaw hooks check
```

Show summary of hook eligibility status (how many are ready vs. not ready).**Options:**

- `--json`: Output as JSON

**Example output:**

```
Hooks Status

Total hooks: 4
Ready: 4
Not ready: 0
```

## Enable a Hook

```
openclaw hooks enable <name>
```

Enable a specific hook by adding it to your config (`~/.openclaw/openclaw.json` by default).**Note:** Workspace hooks are disabled by default until enabled here or in config. Hooks managed by plugins show `plugin:<id>` in `openclaw hooks list` and can’t be enabled/disabled here. Enable/disable the plugin instead.**Arguments:**

- `<name>`: Hook name (e.g., `session-memory`)

**Example:**

```
openclaw hooks enable session-memory
```

**Output:**

```
✓ Enabled hook: 💾 session-memory
```

**What it does:**

- Checks if hook exists and is eligible
- Updates `hooks.internal.entries.<name>.enabled = true` in your config
- Saves config to disk

If the hook came from `<workspace>/hooks/`, this opt-in step is required before
the Gateway will load it.**After enabling:**

- Restart the gateway so hooks reload (menu bar app restart on macOS, or restart your gateway process in dev).

## Disable a Hook

```
openclaw hooks disable <name>
```

Disable a specific hook by updating your config.**Arguments:**

- `<name>`: Hook name (e.g., `command-logger`)

**Example:**

```
openclaw hooks disable command-logger
```

**Output:**

```
⏸ Disabled hook: 📝 command-logger
```

**After disabling:**

- Restart the gateway so hooks reload

## Notes

- `openclaw hooks list --json`, `info --json`, and `check --json` write structured JSON directly to stdout.
- Plugin-managed hooks cannot be enabled or disabled here; enable or disable the owning plugin instead.

## Install hook packs

```
openclaw plugins install <package>        # ClawHub first, then npm
openclaw plugins install npm:<package>    # npm only
openclaw plugins install <package

_… [truncated; see https://docs.openclaw.ai/cli/hooks for full content]_
