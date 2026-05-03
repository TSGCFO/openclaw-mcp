---
source_url: https://docs.openclaw.ai/automation/index
title: "Automation & tasks - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/automation/index#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Automation and tasks

Automation & tasks

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick decision guide](https://docs.openclaw.ai/automation/index#quick-decision-guide)
- [Scheduled Tasks (Cron) vs Heartbeat](https://docs.openclaw.ai/automation/index#scheduled-tasks-cron-vs-heartbeat)
- [Core concepts](https://docs.openclaw.ai/automation/index#core-concepts)
- [Scheduled tasks (cron)](https://docs.openclaw.ai/automation/index#scheduled-tasks-cron)
- [Tasks](https://docs.openclaw.ai/automation/index#tasks)
- [Inferred commitments](https://docs.openclaw.ai/automation/index#inferred-commitments)
- [Task Flow](https://docs.openclaw.ai/automation/index#task-flow)
- [Standing orders](https://docs.openclaw.ai/automation/index#standing-orders)
- [Hooks](https://docs.openclaw.ai/automation/index#hooks)
- [Heartbeat](https://docs.openclaw.ai/automation/index#heartbeat)
- [How they work together](https://docs.openclaw.ai/automation/index#how-they-work-together)
- [Related](https://docs.openclaw.ai/automation/index#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw runs work in the background through tasks, scheduled jobs, inferred
commitments, event hooks, and standing instructions. This page helps you choose
the right mechanism and understand how they fit together.

## [​](https://docs.openclaw.ai/automation/index\#quick-decision-guide)  Quick decision guide

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

### [​](https://docs.openclaw.ai/automation/index\#scheduled-tasks-cron-vs-heartbeat)  Scheduled Tasks (Cron) vs Heartbeat

| Dimension | Scheduled Tasks (Cron) | Heartbeat |
| --- | --- | --- |
| Timing | Exact (cron expressions, one-shot) | Approximate (default every 30 min) |
| Session context | Fresh (isolated) or shared | Full main-session context |
| Task records | Always created | Never created |
| Delivery | Channel, webhook, or silent | Inline in main session |
| Best for | Reports, reminders, background jobs | Inbox checks, calendar, notifications |

Use Scheduled Tasks (Cron) when you need precise timing or isolated execution. Use Heartbeat when the work benefits from full session context and approximate timing is fine.

## [​](https://docs.openclaw.ai/automation/index\#core-concepts)  Core concepts

### [​](https://docs.openclaw.ai/automation/index\#scheduled-tasks-cron)  Scheduled tasks (cron)

Cron is the Gateway’s built-in scheduler for precise timing. It persists jobs, wakes the agent at the right time, and can deliver output to a chat channel or webhook endpoint. Supports one-shot reminders, recurring expressions, and inbound webhook triggers.See [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs).

### [​](https://docs.openclaw.ai/automation/index\#tasks)  Tasks

The background task ledger tracks all detached work: ACP runs, subagent spawns, isolated cron executions, and CLI operations. Tasks are records, not schedulers. Use `openclaw tasks list` and `openclaw tasks audit` to inspect them.See [Background Tasks](https://docs.openclaw.ai/automation/tasks).

### [​](https://docs.openclaw.ai/automation/index\#inferred-commitments)  Inferred commitments

Commitments are opt-in, short-lived follow-up memories. OpenClaw infers them
from normal conversations, scopes them to the same agent and channel, and
delivers due check-ins through heartbeat. Exact user-requested reminders still
belong to cron.See [Inferred Commitments](https://docs.openclaw.ai/concepts/commitments).

### [​](https://docs.openclaw.ai/automation/index\#task-flow)  Task Flow

Task Flow is the flow orchestration substrate above background tasks. It manages durable multi-step flows with managed and mirrored sync modes, revision tracking, and `openclaw tasks flow list|show|cancel` for inspection.See [Task Flow](https://docs.openclaw.ai/automation/taskflow).

### [​](https://docs.openclaw.ai/automation/index\#standing-orders)  Standing orders

Standing orders grant the agent permanent operating authority for defined programs. They live in workspace files (typically `AGENTS.md`) and are injected into every session. Combine with cron for time-based enforcement.See [Standing Orders](https://docs.openclaw.ai/automation/standing-orders).

### [​](https://docs.openclaw.ai/automation/index\#hooks)  Hooks

Internal hooks are event-driven scripts triggered by agent lifecycle events
(`/new`, `/reset`, `/stop`), session compaction, gateway startup, and message
flow. They are automatically discovered from directories and can be managed
with `openclaw hooks`. For in-process tool-call interception, use
[Plugin hooks](https://docs.openclaw.ai/plugins/hooks).See [Hooks](https://docs.openclaw.ai/automation/hooks).

### [​](https://docs.openclaw.ai/automation/index\#heartbeat)  Heartbeat

Heartbeat is a periodic main-session turn (default every 30 minutes). It batches multiple checks (inbox, calendar, notifications) in one agent turn with full session context. Heartbeat turns do not create task records and do not extend daily/idle session reset freshness. Use `HEARTBEAT.md` for a small checklist, or a `tasks:` block when you want due-only periodic checks inside heartbeat itself. Empty heartbeat files skip as `empty-heartbeat-file`; due-only task mode skips as `no-tasks-due`. Heartbeats defer while cron work is active or queued, and `heartbeat.skipWhenBusy` can also defer them while subagent or nested lanes are busy.See [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat).

## [​](https://docs.openclaw.ai/automation/index\#how-they-work-together)  How they work together

- **Cron** handles precise schedules (daily reports, weekly reviews) and one-shot reminders. All cron executions create task records.
- **Heartbeat** handles routine monitoring (inbox, calendar, notifications) in one batched turn every 30 minutes.
- **Hooks** react to specific events (session resets, compaction, message flow) with custom scripts. Plugin hooks cover tool calls.
- **Standing orders** give the agent persistent context and authority boundaries.
- **Task Flow** coordinates multi-step flows above individual tasks.
- **Tasks** automatically track all detached work so you can inspect and audit it.

## [​](https://docs.openclaw.ai/automation/index\#related)  Related

- [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs) — precise scheduling and one-shot reminders
- [Inferred Commitments](https://docs.openclaw.ai/concepts/commitments) — memory-like follow-up check-ins
- [Background Tasks](https://docs.openclaw.ai/automation/tasks) — task ledger for all detached work
- [Task Flow](https://docs.openclaw.ai/automation/taskflow) — durable multi-step flow orchestration
- [Hooks](https://docs.openclaw.ai/automation/hooks) — event-driven lifecycle scripts
- [Plugin hooks](https://docs.openclaw.ai/plugins/hooks) — in-process tool, prompt, message, and lifecycle hooks
- [Standing Orders](https://docs.openclaw.ai/automation/standing-orders) — persistent agent instructions
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) — periodic main-session turns
- [Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference) — all config keys

[OpenProse](https://docs.openclaw.ai/prose) [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs)

Ctrl+I