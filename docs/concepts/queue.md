---
source_url: https://docs.openclaw.ai/concepts/queue
title: "Command queue - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/queue#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Messages and delivery

Command queue

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Why](https://docs.openclaw.ai/concepts/queue#why)
- [How it works](https://docs.openclaw.ai/concepts/queue#how-it-works)
- [Defaults](https://docs.openclaw.ai/concepts/queue#defaults)
- [Queue modes](https://docs.openclaw.ai/concepts/queue#queue-modes)
- [Queue options](https://docs.openclaw.ai/concepts/queue#queue-options)
- [Precedence](https://docs.openclaw.ai/concepts/queue#precedence)
- [Per-session overrides](https://docs.openclaw.ai/concepts/queue#per-session-overrides)
- [Scope and guarantees](https://docs.openclaw.ai/concepts/queue#scope-and-guarantees)
- [Troubleshooting](https://docs.openclaw.ai/concepts/queue#troubleshooting)
- [Related](https://docs.openclaw.ai/concepts/queue#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

We serialize inbound auto-reply runs (all channels) through a tiny in-process queue to prevent multiple agent runs from colliding, while still allowing safe parallelism across sessions.

## [​](https://docs.openclaw.ai/concepts/queue\#why)  Why

- Auto-reply runs can be expensive (LLM calls) and can collide when multiple inbound messages arrive close together.
- Serializing avoids competing for shared resources (session files, logs, CLI stdin) and reduces the chance of upstream rate limits.

## [​](https://docs.openclaw.ai/concepts/queue\#how-it-works)  How it works

- A lane-aware FIFO queue drains each lane with a configurable concurrency cap (default 1 for unconfigured lanes; main defaults to 4, subagent to 8).
- `runEmbeddedPiAgent` enqueues by **session key** (lane `session:<key>`) to guarantee only one active run per session.
- Each session run is then queued into a **global lane** (`main` by default) so overall parallelism is capped by `agents.defaults.maxConcurrent`.
- When verbose logging is enabled, queued runs emit a short notice if they waited more than ~2s before starting.
- Typing indicators still fire immediately on enqueue (when supported by the channel) so user experience is unchanged while we wait our turn.

## [​](https://docs.openclaw.ai/concepts/queue\#defaults)  Defaults

When unset, all inbound channel surfaces use:

- `mode: "steer"`
- `debounceMs: 500`
- `cap: 20`
- `drop: "summarize"`

`steer` is the default because it keeps the active model turn responsive without
starting a second session run. It drains all steering messages that arrived
before the next model boundary. If the current run cannot accept steering,
OpenClaw falls back to a followup queue entry.

## [​](https://docs.openclaw.ai/concepts/queue\#queue-modes)  Queue modes

Inbound messages can steer the current run, wait for a followup turn, or do both:

- `steer`: queue steering messages into the active runtime. Pi delivers all pending steering messages **after the current assistant turn finishes executing its tool calls**, before the next LLM call; Codex app-server receives one batched `turn/steer`. If the run is not actively streaming or steering is unavailable, OpenClaw falls back to a followup queue entry.
- `queue` (legacy): old one-at-a-time steering. Pi delivers one queued steering message at each model boundary; Codex app-server receives separate `turn/steer` requests. Prefer `steer` unless you need the previous serialized behavior.
- `followup`: enqueue each message for a later agent turn after the current run ends.
- `collect`: coalesce queued messages into a **single** followup turn after the quiet window. If messages target different channels/threads, they drain individually to preserve routing.
- `steer-backlog` (aka `steer+backlog`): steer now **and** preserve the same message for a followup turn.
- `interrupt` (legacy): abort the active run for that session, then run the newest message.

Steer-backlog means you can get a followup response after the steered run, so
streaming surfaces can look like duplicates. Prefer `collect`/`steer` if you want
one response per inbound message.For runtime-specific timing and dependency behavior, see
[Steering queue](https://docs.openclaw.ai/concepts/queue-steering).Configure globally or per channel via `messages.queue`:

```
{
  messages: {
    queue: {
      mode: "steer",
      debounceMs: 500,
      cap: 20,
      drop: "summarize",
      byChannel: { discord: "collect" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/concepts/queue\#queue-options)  Queue options

Options apply to `followup`, `collect`, and `steer-backlog` (and to `steer` or legacy `queue` when steering falls back to followup):

- `debounceMs`: quiet window before draining queued followups. Bare numbers are milliseconds; units `ms`, `s`, `m`, `h`, and `d` are accepted by `/queue` options.
- `cap`: max queued messages per session. Values below `1` are ignored.
- `drop: "summarize"`: default. Drop the oldest queued entries as needed, keep compact summaries, and inject them as a synthetic followup prompt.
- `drop: "old"`: drop the oldest queued entries as needed, without preserving summaries.
- `drop: "new"`: reject the newest message when the queue is already full.

Defaults: `debounceMs: 500`, `cap: 20`, `drop: summarize`.

## [​](https://docs.openclaw.ai/concepts/queue\#precedence)  Precedence

For mode selection, OpenClaw resolves:

1. Inline or stored per-session `/queue` override.
2. `messages.queue.byChannel.<channel>`.
3. `messages.queue.mode`.
4. Default `steer`.

For options, inline or stored `/queue` options win over config. Then
channel-specific debounce (`messages.queue.debounceMsByChannel`), plugin
debounce defaults, global `messages.queue` options, and built-in defaults are
applied. `cap` and `drop` are global/session options, not per-channel config
keys.

## [​](https://docs.openclaw.ai/concepts/queue\#per-session-overrides)  Per-session overrides

- Send `/queue <mode>` as a standalone command to store the mode for the current session.
- Options can be combined: `/queue collect debounce:0.5s cap:25 drop:summarize`
- `/queue default` or `/queue reset` clears the session override.

## [​](https://docs.openclaw.ai/concepts/queue\#scope-and-guarantees)  Scope and guarantees

- Applies to auto-reply agent runs across all inbound channels that use the gateway reply pipeline (WhatsApp web, Telegram, Slack, Discord, Signal, iMessage, webchat, etc.).
- Default lane (`main`) is process-wide for inbound + main heartbeats; set `agents.defaults.maxConcurrent` to allow multiple sessions in parallel.
- Additional lanes may exist (e.g. `cron`, `cron-nested`, `nested`, `subagent`) so background jobs can run in parallel without blocking inbound replies. Isolated cron agent turns hold a `cron` slot while their inner agent execution uses `cron-nested`; both use `cron.maxConcurrentRuns`. Shared non-cron `nested` flows keep their own lane behavior. These detached runs are tracked as [background tasks](https://docs.openclaw.ai/automation/tasks).
- Per-session lanes guarantee that only one agent run touches a given session at a time.
- No external dependencies or background worker threads; pure TypeScript + promises.

## [​](https://docs.openclaw.ai/concepts/queue\#troubleshooting)  Troubleshooting

- If commands seem stuck, enable verbose logs and look for “queued for …ms” lines to confirm the queue is draining.
- If you need queue depth, enable verbose logs and watch for queue timing lines.
- Codex app-server runs that accept a turn and then stop emitting progress are interrupted by the Codex adapter so the active session lane can release instead of waiting for the outer run timeout.
- When diagnostics are enabled, sessions that remain in `processing` past `diagnostics.stuckSessionWarnMs` with no observed reply, tool, status, block, or ACP progress are classified by current activity. Active work logs as `session.long_running`; active work with no recent progress logs as `session.stalled`; `session.stuck` is reserved for stale session bookkeeping with no active work, and only that path can release the affected session lane so queued work drains. Repeated `session.stuck` diagnostics back off while the session remains unchanged.

## [​](https://docs.openclaw.ai/concepts/queue\#related)  Related

- [Session management](https://docs.openclaw.ai/concepts/session)
- [Steering queue](https://docs.openclaw.ai/concepts/queue-steering)
- [Retry policy](https://docs.openclaw.ai/concepts/retry)

[Retry policy](https://docs.openclaw.ai/concepts/retry) [Steering queue](https://docs.openclaw.ai/concepts/queue-steering)

Ctrl+I