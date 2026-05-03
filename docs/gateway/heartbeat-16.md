---
source_url: https://docs.openclaw.ai/gateway/heartbeat
title: "Heartbeat - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/heartbeat#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Health and diagnostics

Heartbeat

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start (beginner)](https://docs.openclaw.ai/gateway/heartbeat#quick-start-beginner)
- [Defaults](https://docs.openclaw.ai/gateway/heartbeat#defaults)
- [What the heartbeat prompt is for](https://docs.openclaw.ai/gateway/heartbeat#what-the-heartbeat-prompt-is-for)
- [Response contract](https://docs.openclaw.ai/gateway/heartbeat#response-contract)
- [Config](https://docs.openclaw.ai/gateway/heartbeat#config)
- [Scope and precedence](https://docs.openclaw.ai/gateway/heartbeat#scope-and-precedence)
- [Per-agent heartbeats](https://docs.openclaw.ai/gateway/heartbeat#per-agent-heartbeats)
- [Active hours example](https://docs.openclaw.ai/gateway/heartbeat#active-hours-example)
- [24/7 setup](https://docs.openclaw.ai/gateway/heartbeat#24%2F7-setup)
- [Multi-account example](https://docs.openclaw.ai/gateway/heartbeat#multi-account-example)
- [Field notes](https://docs.openclaw.ai/gateway/heartbeat#field-notes)
- [Delivery behavior](https://docs.openclaw.ai/gateway/heartbeat#delivery-behavior)
- [Visibility controls](https://docs.openclaw.ai/gateway/heartbeat#visibility-controls)
- [What each flag does](https://docs.openclaw.ai/gateway/heartbeat#what-each-flag-does)
- [Per-channel vs per-account examples](https://docs.openclaw.ai/gateway/heartbeat#per-channel-vs-per-account-examples)
- [Common patterns](https://docs.openclaw.ai/gateway/heartbeat#common-patterns)
- [HEARTBEAT.md (optional)](https://docs.openclaw.ai/gateway/heartbeat#heartbeat-md-optional)
- [tasks: blocks](https://docs.openclaw.ai/gateway/heartbeat#tasks-blocks)
- [Can the agent update HEARTBEAT.md?](https://docs.openclaw.ai/gateway/heartbeat#can-the-agent-update-heartbeat-md)
- [Manual wake (on-demand)](https://docs.openclaw.ai/gateway/heartbeat#manual-wake-on-demand)
- [Reasoning delivery (optional)](https://docs.openclaw.ai/gateway/heartbeat#reasoning-delivery-optional)
- [Cost awareness](https://docs.openclaw.ai/gateway/heartbeat#cost-awareness)
- [Context overflow after heartbeat](https://docs.openclaw.ai/gateway/heartbeat#context-overflow-after-heartbeat)
- [Related](https://docs.openclaw.ai/gateway/heartbeat#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

**Heartbeat vs cron?** See [Automation & Tasks](https://docs.openclaw.ai/automation) for guidance on when to use each.

Heartbeat runs **periodic agent turns** in the main session so the model can surface anything that needs attention without spamming you.Heartbeat is a scheduled main-session turn — it does **not** create [background task](https://docs.openclaw.ai/automation/tasks) records. Task records are for detached work (ACP runs, subagents, isolated cron jobs).Troubleshooting: [Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs#troubleshooting)

## [​](https://docs.openclaw.ai/gateway/heartbeat\#quick-start-beginner)  Quick start (beginner)

1

[Navigate to header](https://docs.openclaw.ai/gateway/heartbeat#)

Pick a cadence

Leave heartbeats enabled (default is `30m`, or `1h` for Anthropic OAuth/token auth, including Claude CLI reuse) or set your own cadence.

2

[Navigate to header](https://docs.openclaw.ai/gateway/heartbeat#)

Add HEARTBEAT.md (optional)

Create a tiny `HEARTBEAT.md` checklist or `tasks:` block in the agent workspace.

3

[Navigate to header](https://docs.openclaw.ai/gateway/heartbeat#)

Decide where heartbeat messages should go

`target: "none"` is the default; set `target: "last"` to route to the last contact.

4

[Navigate to header](https://docs.openclaw.ai/gateway/heartbeat#)

Optional tuning

- Enable heartbeat reasoning delivery for transparency.
- Use lightweight bootstrap context if heartbeat runs only need `HEARTBEAT.md`.
- Enable isolated sessions to avoid sending full conversation history each heartbeat.
- Restrict heartbeats to active hours (local time).

Example config:

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last", // explicit delivery to last contact (default is "none")
        directPolicy: "allow", // default: allow direct/DM targets; set "block" to suppress
        lightContext: true, // optional: only inject HEARTBEAT.md from bootstrap files
        isolatedSession: true, // optional: fresh session each run (no conversation history)
        skipWhenBusy: true, // optional: also defer when subagent or nested lanes are busy
        // activeHours: { start: "08:00", end: "24:00" },
        // includeReasoning: true, // optional: send separate `Reasoning:` message too
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/gateway/heartbeat\#defaults)  Defaults

- Interval: `30m` (or `1h` when Anthropic OAuth/token auth is the detected auth mode, including Claude CLI reuse). Set `agents.defaults.heartbeat.every` or per-agent `agents.list[].heartbeat.every`; use `0m` to disable.
- Prompt body (configurable via `agents.defaults.heartbeat.prompt`): `Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.`
- The heartbeat prompt is sent **verbatim** as the user message. The system prompt includes a “Heartbeat” section only when heartbeats are enabled for the default agent, and the run is flagged internally.
- When heartbeats are disabled with `0m`, normal runs also omit `HEARTBEAT.md` from bootstrap context so the model does not see heartbeat-only instructions.
- Active hours (`heartbeat.activeHours`) are checked in the configured timezone. Outside the window, heartbeats are skipped until the next tick inside the window.
- Heartbeats automatically defer while cron work is active or queued. Set `heartbeat.skipWhenBusy: true` to defer on extra busy lanes (subagent or nested command work) as well; this is useful for local Ollama and other constrained single-runtime hosts.

## [​](https://docs.openclaw.ai/gateway/heartbeat\#what-the-heartbeat-prompt-is-for)  What the heartbeat prompt is for

The default prompt is intentionally broad:

- **Background tasks**: “Consider outstanding tasks” nudges the agent to review follow-ups (inbox, calendar, reminders, queued work) and surface anything urgent.
- **Human check-in**: “Checkup sometimes on your human during day time” nudges an occasional lightweight “anything you need?” message, but avoids night-time spam by using your configured local timezone (see [Timezone](https://docs.openclaw.ai/concepts/timezone)).

Heartbeat can react to completed [background tasks](https://docs.openclaw.ai/automation/tasks), but a heartbeat run itself does not create a task record.If you want a heartbeat to do something very specific (e.g. “check Gmail PubSub stats” or “verify gateway health”), set `agents.defaults.heartbeat.prompt` (or `agents.list[].heartbeat.prompt`) to a custom body (sent verbatim).

## [​](https://docs.openclaw.ai/gateway/heartbeat\#response-contract)  Response contract

- If nothing needs attention, reply with **`HEARTBEAT_OK`**.
- Tool-capable heartbeat runs may instead call `heartbeat_respond` with `notify: false` for no visible update, or `notify: true` plus `notificationText` for an alert. When present, the structured tool response takes precedence over the text fallback.
- During heartbeat runs, OpenClaw treats `HEARTBEAT_OK` as an ack when it appears at the **start or end** of the reply. The token is stripped and the reply is dropped if the remaining content is **≤ `ackMaxChars`** (default: 300).
- If `HEARTBEAT_OK` appears in the **middle** of a reply, it is not treated specially.
- For alerts, **do not** include `HEARTBEAT_OK`; return only the alert text.

Outside heartbeats, stray `HEARTBEAT_OK` at the start/end of a message is stripped and logged; a message that is only `HEARTBEAT_OK` is dropped.

## [​](https://docs.openclaw.ai/gateway/heartbeat\#config)  Config

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m", // default: 30m (0m disables)
        model: "anthropic/claude-opus-4-6",
        includeReasoning: false, // default: false (deliver separate Reasoning: message when available)
        lightContext: false, // default: false; true keeps only HEARTBEAT.md from workspace bootstrap files
        isolatedSession: false, // default: false; true runs each heartbeat in a fresh session (no conversation history)
        skipWhenBusy: false, // default: false; true also waits for subagent/nested lanes
        target: "last", // default: none | options: last | none | <channel id> (core or plugin, e.g. "bluebubbles")
        to: "+15551234567", // optional channel-specific override
        accountId: "ops-bot", // optional multi-account channel id
        prompt: "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.",
        ackMaxChars: 300, // max chars allowed after HEARTBEAT_OK
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/gateway/heartbeat\#scope-and-precedence)  Scope and precedence

- `agents.defaults.heartbeat` sets global heartbeat behavior.
- `agents.list[].heartbeat` merges on top; if any agent has a `heartbeat` block, **only those agents** run heartbeats.
- `channels.defaults.heartbeat` sets visibility defaults for all channels.
- `channels.<channel>.heartbeat` overrides channel defaults.
- `channels.<channel>.accounts.<id>.heartbeat` (multi-account channels) overrides per-channel settings.

### [​](https://docs.openclaw.ai/gateway/heartbeat\#per-agent-heartbeats)  Per-agent heartbeats

If any `agents.list[]` entry includes a `heartbeat` block, **only those agents** run heartbeats. The per-agent block merges on top of `agents.defaults.heartbeat` (so you can set shared defaults once and override per agent).Example: two agents, only the second agent runs heartbeats.

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last", // explicit delivery to last contact (default is "none")
      },
    },
    list: [\
      { id: "main", default: true },\
      {\
        id: "ops",\
        heartbeat: {\
          every: "1h",\
          target: "whatsapp",\
          to: "+15551234567",\
          timeoutSeconds: 45,\
          prompt: "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.",\
        },\
      },\
    ],
  },
}
```

### [​](https://docs.openclaw.ai/gateway/heartbeat\#active-hours-example)  Active hours example

Restrict heartbeats to business hours in a specific timezone:

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last", // explicit delivery to last contact (default is "none")
        activeHours: {
          start: "09:00",
          end: "22:00",
          timezone: "America/New_York", // optional; uses your userTimezone if set, otherwise host tz
        },
      },
    },
  },
}
```

Outside this window (before 9am or after 10pm Eastern), heartbeats are skipped. The next scheduled tick inside the window will run normally.

### [​](https://docs.openclaw.ai/gateway/heartbeat\#24/7-setup)  24/7 setup

If you want heartbeats to run all day, use one of these patterns:

- Omit `activeHours` entirely (no time-window restriction; this is the default behavior).
- Set a full-day window: `activeHours: { start: "00:00", end: "24:00" }`.

Do not set the same `start` and `end` time (for example `08:00` to `08:00`). That is treated as a zero-width window, so heartbeats are always skipped.

### [​](https://docs.openclaw.ai/gateway/heartbeat\#multi-account-example)  Multi-account example

Use `accountId` to target a specific account on multi-account channels like Telegram:

```
{
  agents: {
    list: [\
      {\
        id: "ops",\
        heartbeat: {\
          every: "1h",\
          target: "telegram",\
          to: "12345678:topic:42", // optional: route to a specific topic/thread\
          accountId: "ops-bot",\
        },\
      },\
    ],
  },
  channels: {
    telegram: {
      accounts: {
        "ops-bot": { botToken: "YOUR_TELEGRAM_BOT_TOKEN" },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/gateway/heartbeat\#field-notes)  Field notes

[​](https://docs.openclaw.ai/gateway/heartbeat#param-every)

every

string

Heartbeat interval (duration string; default unit = minutes).

[​](https://docs.openclaw.ai/gateway/heartbeat#param-model)

model

string

Optional model override for heartbeat runs (`provider/model`).

[​](https://docs.openclaw.ai/gateway/heartbeat#param-include-reasoning)

includeReasoning

boolean

default:"false"

When enabled, also deliver the separate `Reasoning:` message when available (same shape as `/reasoning on`).

[​](https://docs.openclaw.ai/gateway/heartbeat#param-light-context)

lightContext

boolean

default:"false"

When true, heartbeat runs use lightweight bootstrap context and keep only `HEARTBEAT.md` from workspace bootstrap files.

[​](https://docs.openclaw.ai/gateway/heartbeat#param-isolated-session)

isolatedSession

boolean

default:"false"

When true, each heartbeat runs in a fresh session with no prior conversation history. Uses the same isolation pattern as cron `sessionTarget: "isolated"`. Dramatically reduces per-heartbeat token cost. Combine with `lightContext: true` for maximum savings. Delivery routing still uses the main session context.

[​](https://docs.openclaw.ai/gateway/heartbeat#param-skip-when-busy)

skipWhenBusy

boolean

default:"false"

When true, heartbeat runs defer on extra busy lanes: subagent or nested command work. Cron lanes always defer heartbeats, even without this flag, so local-model hosts do not run cron and heartbeat prompts at the same time.

[​](https://docs.openclaw.ai/gateway/heartbeat#param-session)

session

string

Optional session key for heartbeat runs.

- `main` (default): agent main session.
- Explicit session key (copy from `openclaw sessions --json` or the [sessions CLI](https://docs.openclaw.ai/cli/sessions)).
- Session key formats: see [Sessions](https://docs.openclaw.ai/concepts/session) and [Groups](https://docs.openclaw.ai/channels/groups).

[​](https://docs.openclaw.ai/gateway/heartbeat#param-target)

target

string

- `last`: deliver to the last used external channel.
- explicit channel: any configured channel or plugin id, for example `discord`, `matrix`, `telegram`, or `whatsapp`.
- `none` (default): run the heartbeat but **do not deliver** externally.

[​](https://docs.openclaw.ai/gateway/heartbeat#param-direct-policy)

directPolicy

"allow" \| "block"

default:"allow"

Controls direct/DM delivery behavior. `allow`: allow direct/DM heartbeat delivery. `block`: suppress direct/DM delivery (`reason=dm-blocked`).

[​](https://docs.openclaw.ai/gateway/heartbeat#param-to)

to

string

Optional recipient override (channel-specific id, e.g. E.164 for WhatsApp or a Telegram chat id). For Telegram topics/threads, use `<chatId>:topic:<messageThreadId>`.

[​](https://docs.openclaw.ai/gateway/heartbeat#param-account-id)

accountId

string

Optional account id for multi-account channels. When `target: "last"`, the account id applies to the resolved last channel if it supports accounts; otherwise it is ignored. If the account id does not match a configured account for the resolved channel, delivery is skipped.

[​](https://docs.openclaw.ai/gateway/heartbeat#param-prompt)

prompt

string

Overrides the default prompt body (not merged).

[​](https://docs.openclaw.ai/gateway/heartbeat#param-ack-max-chars)

ackMaxChars

number

default:"300"

Max chars allowed after `HEARTBEAT_OK` before delivery.

[​](https://docs.openclaw.ai/gateway/heartbeat#param-suppress-tool-error-warnings)

suppressToolErrorWarnings

boolean

When true, suppresses tool error warning payloads during heartbeat runs.

[​](https://docs.openclaw.ai/gateway/heartbeat#param-active-hours)

activeHours

object

Restricts heartbeat runs to a time window. Object with `start` (HH:MM, inclusive; use `00:00` for start-of-day), `end` (HH:MM exclusive; `24:00` allowed for end-of-day), and optional `timezone`.

- Omitted or `"user"`: uses your `agents.defaults.userTimezone` if set, otherwise falls back to the host system timezone.
- `"local"`: always uses the host system timezone.
- Any IANA identifier (e.g. `America/New_York`): used directly; if invalid, falls back to the `"user"` behavior above.
- `start` and `end` must not be equal for an active window; equal values are treated as zero-width (always outside the window).
- Outside the active window, heartbeats are skipped until the next tick inside the window.

## [​](https://docs.openclaw.ai/gateway/heartbeat\#delivery-behavior)  Delivery behavior

Session and target routing

- Heartbeats run in the agent’s main session by default (`agent:<id>:<mainKey>`), or `global` when `session.scope = "global"`. Set `session` to override to a specific channel session (Discord/WhatsApp/etc.).
- `session` only affects the run context; delivery is controlled by `target` and `to`.
- To deliver to a specific channel/recipient, set `target` \+ `to`. With `target: "last"`, delivery uses the last external channel for that session.
- Heartbeat deliveries allow direct/DM targets by default. Set `directPolicy: "block"` to suppress direct-target sends while still running the heartbeat turn.
- If the main queue, target session lane, cron lane, or an active cron job is busy, the heartbeat is skipped and retried later.
- If `skipWhenBusy: true`, subagent and nested lanes also defer heartbeat runs.
- If `target` resolves to no external destination, the run still happens but no outbound message is sent.

Visibility and skip behavior

- If `showOk`, `showAlerts`, and `useIndicator` are all disabled, the run is skipped up front as `reason=alerts-disabled`.
- If only alert delivery is disabled, OpenClaw can still run the heartbeat, update due-task timestamps, restore the session idle timestamp, and suppress the outward alert payload.
- If the resolved heartbeat target supports typing, OpenClaw shows typing while the heartbeat run is active. This uses the same target the heartbeat would send chat output to, and it is disabled by `typingMode: "never"`.

Session lifecycle and audit

- Heartbeat-only replies do **not** keep the session alive. Heartbeat metadata may update the session row, but idle expiry uses `lastInteractionAt` from the last real user/channel message, and daily expiry uses `sessionStartedAt`.
- Control UI and WebChat history hide heartbeat prompts and OK-only acknowledgments. The underlying session transcript can still contain those turns for audit/replay.
- Detached [background tasks](https://docs.openclaw.ai/automation/tasks) can enqueue a system event and wake heartbeat when the main session should notice something quickly. That wake does not make the heartbeat run a background task.

## [​](https://docs.openclaw.ai/gateway/heartbeat\#visibility-controls)  Visibility controls

By default, `HEARTBEAT_OK` acknowledgments are suppressed while alert content is delivered. You can adjust this per channel or per account:

```
channels:
  defaults:
    heartbeat:
      showOk: false # Hide HEARTBEAT_OK (default)
      showAlerts: true # Show alert messages (default)
      useIndicator: true # Emit indicator events (default)
  telegram:
    heartbeat:
      showOk: true # Show OK acknowledgments on Telegram
  whatsapp:
    accounts:
      work:
        heartbeat:
          showAlerts: false # Suppress alert delivery for this account
```

Precedence: per-account → per-channel → channel defaults → built-in defaults.

### [​](https://docs.openclaw.ai/gateway/heartbeat\#what-each-flag-does)  What each flag does

- `showOk`: sends a `HEARTBEAT_OK` acknowledgment when the model returns an OK-only reply.
- `showAlerts`: sends the alert content when the model returns a non-OK reply.
- `useIndicator`: emits indicator events for UI status surfaces.

If **all three** are false, OpenClaw skips the heartbeat run entirely (no model call).

### [​](https://docs.openclaw.ai/gateway/heartbeat\#per-channel-vs-per-account-examples)  Per-channel vs per-account examples

```
channels:
  defaults:
    heartbeat:
      showOk: false
      showAlerts: true
      useIndicator: true
  slack:
    heartbeat:
      showOk: true # all Slack accounts
    accounts:
      ops:
        heartbeat:
          showAlerts: false # suppress alerts for the ops account only
  telegram:
    heartbeat:
      showOk: true
```

### [​](https://docs.openclaw.ai/gateway/heartbeat\#common-patterns)  Common patterns

| Goal | Config |
| --- | --- |
| Default behavior (silent OKs, alerts on) | _(no config needed)_ |
| Fully silent (no messages, no indicator) | `channels.defaults.heartbeat: { showOk: false, showAlerts: false, useIndicator: false }` |
| Indicator-only (no messages) | `channels.defaults.heartbeat: { showOk: false, showAlerts: false, useIndicator: true }` |
| OKs in one channel only | `channels.telegram.heartbeat: { showOk: true }` |

## [​](https://docs.openclaw.ai/gateway/heartbeat\#heartbeat-md-optional)  HEARTBEAT.md (optional)

If a `HEARTBEAT.md` file exists in the workspace, the default prompt tells the agent to read it. Think of it as your “heartbeat checklist”: small, stable, and safe to include every 30 minutes.On normal runs, `HEARTBEAT.md` is only injected when heartbeat guidance is enabled for the default agent. Disabling the heartbeat cadence with `0m` or setting `includeSystemPromptSection: false` omits it from normal bootstrap context.If `HEARTBEAT.md` exists but is effectively empty (only blank lines and markdown headers like `# Heading`), OpenClaw skips the heartbeat run to save API calls. That skip is reported as `reason=empty-heartbeat-file`. If the file is missing, the heartbeat still runs and the model decides what to do.Keep it tiny (short checklist or reminders) to avoid prompt bloat.Example `HEARTBEAT.md`:

```
# Heartbeat checklist

- Quick scan: anything urgent in inboxes?
- If it's daytime, do a lightweight check-in if nothing else is pending.
- If a task is blocked, write down _what is missing_ and ask Peter next time.
```

### [​](https://docs.openclaw.ai/gateway/heartbeat\#tasks-blocks)  `tasks:` blocks

`HEARTBEAT.md` also supports a small structured `tasks:` block for interval-based checks inside heartbeat itself.Example:

```
tasks:

- name: inbox-triage
  interval: 30m
  prompt: "Check for urgent unread emails and flag anything time sensitive."
- name: calendar-scan
  interval: 2h
  prompt: "Check for upcoming meetings that need prep or follow-up."

# Additional instructions

- Keep alerts short.
- If nothing needs attention after all due tasks, reply HEARTBEAT_OK.
```

Behavior

- OpenClaw parses the `tasks:` block and checks each task against its own `interval`.
- Only **due** tasks are included in the heartbeat prompt for that tick.
- If no tasks are due, the heartbeat is skipped entirely (`reason=no-tasks-due`) to avoid a wasted model call.
- Non-task content in `HEARTBEAT.md` is preserved and appended as additional context after the due-task list.
- Task last-run timestamps are stored in session state (`heartbeatTaskState`), so intervals survive normal restarts.
- Task timestamps are only advanced after a heartbeat run completes its normal reply path. Skipped `empty-heartbeat-file` / `no-tasks-due` runs do not mark tasks as completed.

Task mode is useful when you want one heartbeat file to hold several periodic checks without paying for all of them every tick.

### [​](https://docs.openclaw.ai/gateway/heartbeat\#can-the-agent-update-heartbeat-md)  Can the agent update HEARTBEAT.md?

Yes — if you ask it to.`HEARTBEAT.md` is just a normal file in the agent workspace, so you can tell the agent (in a normal chat) something like:

- “Update `HEARTBEAT.md` to add a daily calendar check.”
- “Rewrite `HEARTBEAT.md` so it’s shorter and focused on inbox follow-ups.”

If you want this to happen proactively, you can also include an explicit line in your heartbeat prompt like: “If the checklist becomes stale, update HEARTBEAT.md with a better one.”

Don’t put secrets (API keys, phone numbers, private tokens) into `HEARTBEAT.md` — it becomes part of the prompt context.

## [​](https://docs.openclaw.ai/gateway/heartbeat\#manual-wake-on-demand)  Manual wake (on-demand)

You can enqueue a system event and trigger an immediate heartbeat with:

```
openclaw system event --text "Check for urgent follow-ups" --mode now
```

If multiple agents have `heartbeat` configured, a manual wake runs each of those agent heartbeats immediately.Use `--mode next-heartbeat` to wait for the next scheduled tick.

## [​](https://docs.openclaw.ai/gateway/heartbeat\#reasoning-delivery-optional)  Reasoning delivery (optional)

By default, heartbeats deliver only the final “answer” payload.If you want transparency, enable:

- `agents.defaults.heartbeat.includeReasoning: true`

When enabled, heartbeats will also deliver a separate message prefixed `Reasoning:` (same shape as `/reasoning on`). This can be useful when the agent is managing multiple sessions/codexes and you want to see why it decided to ping you — but it can also leak more internal detail than you want. Prefer keeping it off in group chats.

## [​](https://docs.openclaw.ai/gateway/heartbeat\#cost-awareness)  Cost awareness

Heartbeats run full agent turns. Shorter intervals burn more tokens. To reduce cost:

- Use `isolatedSession: true` to avoid sending full conversation history (~100K tokens down to ~2-5K per run).
- Use `lightContext: true` to limit bootstrap files to just `HEARTBEAT.md`.
- Set a cheaper `model` (e.g. `ollama/llama3.2:1b`).
- Keep `HEARTBEAT.md` small.
- Use `target: "none"` if you only want internal state updates.

## [​](https://docs.openclaw.ai/gateway/heartbeat\#context-overflow-after-heartbeat)  Context overflow after heartbeat

If a heartbeat uses a smaller local model, for example an Ollama model with a 32k window, and the next main-session turn reports context overflow, check whether the previous heartbeat left the session on the heartbeat model. OpenClaw’s reset message calls this out when the last runtime model matches configured `heartbeat.model`.Use `isolatedSession: true` to run heartbeats in a fresh session, combine it with `lightContext: true` for the smallest prompt, or choose a heartbeat model with a context window large enough for the shared session.

## [​](https://docs.openclaw.ai/gateway/heartbeat\#related)  Related

- [Automation & Tasks](https://docs.openclaw.ai/automation) — all automation mechanisms at a glance
- [Background Tasks](https://docs.openclaw.ai/automation/tasks) — how detached work is tracked
- [Timezone](https://docs.openclaw.ai/concepts/timezone) — how timezone affects heartbeat scheduling
- [Troubleshooting](https://docs.openclaw.ai/automation/cron-jobs#troubleshooting) — debugging automation issues

[Health checks](https://docs.openclaw.ai/gateway/health) [Doctor](https://docs.openclaw.ai/gateway/doctor)

Ctrl+I