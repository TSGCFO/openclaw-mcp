---
source_url: https://docs.openclaw.ai/concepts/session
title: "Session management - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/session#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Sessions and memory

Session management

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [How messages are routed](https://docs.openclaw.ai/concepts/session#how-messages-are-routed)
- [DM isolation](https://docs.openclaw.ai/concepts/session#dm-isolation)
- [Dock linked channels](https://docs.openclaw.ai/concepts/session#dock-linked-channels)
- [Session lifecycle](https://docs.openclaw.ai/concepts/session#session-lifecycle)
- [Where state lives](https://docs.openclaw.ai/concepts/session#where-state-lives)
- [Session maintenance](https://docs.openclaw.ai/concepts/session#session-maintenance)
- [Inspecting sessions](https://docs.openclaw.ai/concepts/session#inspecting-sessions)
- [Further reading](https://docs.openclaw.ai/concepts/session#further-reading)
- [Related](https://docs.openclaw.ai/concepts/session#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw organizes conversations into **sessions**. Each message is routed to a
session based on where it came from — DMs, group chats, cron jobs, etc.

## [​](https://docs.openclaw.ai/concepts/session\#how-messages-are-routed)  How messages are routed

| Source | Behavior |
| --- | --- |
| Direct messages | Shared session by default |
| Group chats | Isolated per group |
| Rooms/channels | Isolated per room |
| Cron jobs | Fresh session per run |
| Webhooks | Isolated per hook |

## [​](https://docs.openclaw.ai/concepts/session\#dm-isolation)  DM isolation

By default, all DMs share one session for continuity. This is fine for
single-user setups.

If multiple people can message your agent, enable DM isolation. Without it, all
users share the same conversation context — Alice’s private messages would be
visible to Bob.

**The fix:**

```
{
  session: {
    dmScope: "per-channel-peer", // isolate by channel + sender
  },
}
```

Other options:

- `main` (default) — all DMs share one session.
- `per-peer` — isolate by sender (across channels).
- `per-channel-peer` — isolate by channel + sender (recommended).
- `per-account-channel-peer` — isolate by account + channel + sender.

If the same person contacts you from multiple channels, use
`session.identityLinks` to link their identities so they share one session.

### [​](https://docs.openclaw.ai/concepts/session\#dock-linked-channels)  Dock linked channels

Dock commands let a user move the current direct-chat session’s reply route to
another linked channel without starting a new session. See
[Channel docking](https://docs.openclaw.ai/concepts/channel-docking) for examples, config, and
troubleshooting.Verify your setup with `openclaw security audit`.

## [​](https://docs.openclaw.ai/concepts/session\#session-lifecycle)  Session lifecycle

Sessions are reused until they expire:

- **Daily reset** (default) — new session at 4:00 AM local time on the gateway
host. Daily freshness is based on when the current `sessionId` started, not
on later metadata writes.
- **Idle reset** (optional) — new session after a period of inactivity. Set
`session.reset.idleMinutes`. Idle freshness is based on the last real
user/channel interaction, so heartbeat, cron, and exec system events do not
keep the session alive.
- **Manual reset** — type `/new` or `/reset` in chat. `/new <model>` also
switches the model.

When both daily and idle resets are configured, whichever expires first wins.
Heartbeat, cron, exec, and other system-event turns may write session metadata,
but those writes do not extend daily or idle reset freshness. When a reset
rolls the session, queued system-event notices for the old session are
discarded so stale background updates are not prepended to the first prompt in
the new session.Sessions with an active provider-owned CLI session are not cut by the implicit
daily default. Use `/reset` or configure `session.reset` explicitly when those
sessions should expire on a timer.

## [​](https://docs.openclaw.ai/concepts/session\#where-state-lives)  Where state lives

All session state is owned by the **gateway**. UI clients query the gateway for
session data.

- **Store:**`~/.openclaw/agents/<agentId>/sessions/sessions.json`
- **Transcripts:**`~/.openclaw/agents/<agentId>/sessions/<sessionId>.jsonl`

`sessions.json` keeps separate lifecycle timestamps:

- `sessionStartedAt`: when the current `sessionId` began; daily reset uses this.
- `lastInteractionAt`: last user/channel interaction that extends idle lifetime.
- `updatedAt`: last store-row mutation; useful for listing and pruning, but not
authoritative for daily/idle reset freshness.

Older rows without `sessionStartedAt` are resolved from the transcript JSONL
session header when available. If an older row also lacks `lastInteractionAt`,
idle freshness falls back to that session start time, not to later bookkeeping
writes.

## [​](https://docs.openclaw.ai/concepts/session\#session-maintenance)  Session maintenance

OpenClaw automatically bounds session storage over time. By default, it runs
in `warn` mode (reports what would be cleaned). Set `session.maintenance.mode`
to `"enforce"` for automatic cleanup:

```
{
  session: {
    maintenance: {
      mode: "enforce",
      pruneAfter: "30d",
      maxEntries: 500,
    },
  },
}
```

For production-sized `maxEntries` limits, Gateway runtime writes use a small high-water buffer and clean back down to the configured cap in batches. Session store reads do not prune or cap entries during Gateway startup. This avoids running full store cleanup on every startup or isolated cron session. `openclaw sessions cleanup --enforce` applies the cap immediately.Maintenance preserves durable external conversation pointers, including group
sessions and thread-scoped chat sessions, while still allowing synthetic cron,
hook, heartbeat, ACP, and sub-agent entries to age out.Preview with `openclaw sessions cleanup --dry-run`.

## [​](https://docs.openclaw.ai/concepts/session\#inspecting-sessions)  Inspecting sessions

- `openclaw status` — session store path and recent activity.
- `openclaw sessions --json` — all sessions (filter with `--active <minutes>`).
- `/status` in chat — context usage, model, and toggles.
- `/context list` — what is in the system prompt.

## [​](https://docs.openclaw.ai/concepts/session\#further-reading)  Further reading

- [Session Pruning](https://docs.openclaw.ai/concepts/session-pruning) — trimming tool results
- [Compaction](https://docs.openclaw.ai/concepts/compaction) — summarizing long conversations
- [Session Tools](https://docs.openclaw.ai/concepts/session-tool) — agent tools for cross-session work
- [Session Management Deep Dive](https://docs.openclaw.ai/reference/session-management-compaction) —
store schema, transcripts, send policy, origin metadata, and advanced config
- [Multi-Agent](https://docs.openclaw.ai/concepts/multi-agent) — routing and session isolation across agents
- [Background Tasks](https://docs.openclaw.ai/automation/tasks) — how detached work creates task records with session references
- [Channel Routing](https://docs.openclaw.ai/channels/channel-routing) — how inbound messages are routed to sessions

## [​](https://docs.openclaw.ai/concepts/session\#related)  Related

- [Session pruning](https://docs.openclaw.ai/concepts/session-pruning)
- [Session tools](https://docs.openclaw.ai/concepts/session-tool)
- [Command queue](https://docs.openclaw.ai/concepts/queue)

[Matrix QA](https://docs.openclaw.ai/concepts/qa-matrix) [Channel docking](https://docs.openclaw.ai/concepts/channel-docking)

Ctrl+I