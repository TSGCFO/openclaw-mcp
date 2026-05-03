---
source_url: https://docs.openclaw.ai/concepts/session-tool
title: "Session tools - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/session-tool#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Sessions and memory

Session tools

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Available tools](https://docs.openclaw.ai/concepts/session-tool#available-tools)
- [Listing and reading sessions](https://docs.openclaw.ai/concepts/session-tool#listing-and-reading-sessions)
- [Sending cross-session messages](https://docs.openclaw.ai/concepts/session-tool#sending-cross-session-messages)
- [Status and orchestration helpers](https://docs.openclaw.ai/concepts/session-tool#status-and-orchestration-helpers)
- [Spawning sub-agents](https://docs.openclaw.ai/concepts/session-tool#spawning-sub-agents)
- [Visibility](https://docs.openclaw.ai/concepts/session-tool#visibility)
- [Further reading](https://docs.openclaw.ai/concepts/session-tool#further-reading)
- [Related](https://docs.openclaw.ai/concepts/session-tool#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw gives agents tools to work across sessions, inspect status, and
orchestrate sub-agents.

## [​](https://docs.openclaw.ai/concepts/session-tool\#available-tools)  Available tools

| Tool | What it does |
| --- | --- |
| `sessions_list` | List sessions with optional filters (kind, label, agent, recency, preview) |
| `sessions_history` | Read the transcript of a specific session |
| `sessions_send` | Send a message to another session and optionally wait |
| `sessions_spawn` | Spawn an isolated sub-agent session for background work |
| `sessions_yield` | End the current turn and wait for follow-up sub-agent results |
| `subagents` | List, steer, or kill spawned sub-agents for this session |
| `session_status` | Show a `/status`-style card and optionally set a per-session model override |

These tools are still subject to the active tool profile and allow/deny
policy. `tools.profile: "coding"` includes the full session orchestration
set, including `sessions_spawn`, `sessions_yield`, and `subagents`.
`tools.profile: "messaging"` includes cross-session messaging tools
(`sessions_list`, `sessions_history`, `sessions_send`, `session_status`) but
does not include sub-agent spawning. To keep a messaging profile and still
allow native delegation, add:

```
{
  tools: {
    profile: "messaging",
    alsoAllow: ["sessions_spawn", "sessions_yield", "subagents"],
  },
}
```

Group, provider, sandbox, and per-agent policies can still remove those tools
after the profile stage. Use `/tools` from the affected session to inspect the
effective tool list.

## [​](https://docs.openclaw.ai/concepts/session-tool\#listing-and-reading-sessions)  Listing and reading sessions

`sessions_list` returns sessions with their key, agentId, kind, channel, model,
token counts, and timestamps. Filter by kind (`main`, `group`, `cron`, `hook`,
`node`), exact `label`, exact `agentId`, search text, or recency
(`activeMinutes`). When you need mailbox-style triage, it can also ask for a
visibility-scoped derived title, a last-message preview snippet, or bounded
recent messages on each row. Derived titles and previews are produced only for
sessions the caller can already see under the configured session tool
visibility policy, so unrelated sessions stay hidden.`sessions_history` fetches the conversation transcript for a specific session.
By default, tool results are excluded — pass `includeTools: true` to see them.
The returned view is intentionally bounded and safety-filtered:

- assistant text is normalized before recall:
  - thinking tags are stripped
  - `<relevant-memories>` / `<relevant_memories>` scaffolding blocks are stripped
  - plain-text tool-call XML payload blocks such as `<tool_call>...</tool_call>`,
    `<function_call>...</function_call>`, `<tool_calls>...</tool_calls>`, and
    `<function_calls>...</function_calls>` are stripped, including truncated
    payloads that never close cleanly
  - downgraded tool-call/result scaffolding such as `[Tool Call: ...]`,
    `[Tool Result ...]`, and `[Historical context ...]` is stripped
  - leaked model control tokens such as `<|assistant|>`, other ASCII
    `<|...|>` tokens, and full-width `<｜...｜>` variants are stripped
  - malformed MiniMax tool-call XML such as `<invoke ...>` /
    `</minimax:tool_call>` is stripped
- credential/token-like text is redacted before it is returned
- long text blocks are truncated
- very large histories can drop older rows or replace an oversized row with
`[sessions_history omitted: message too large]`
- the tool reports summary flags such as `truncated`, `droppedMessages`,
`contentTruncated`, `contentRedacted`, and `bytes`

Both tools accept either a **session key** (like `"main"`) or a **session ID**
from a previous list call.If you need the exact byte-for-byte transcript, inspect the transcript file on
disk instead of treating `sessions_history` as a raw dump.

## [​](https://docs.openclaw.ai/concepts/session-tool\#sending-cross-session-messages)  Sending cross-session messages

`sessions_send` delivers a message to another session and optionally waits for
the response:

- **Fire-and-forget:** set `timeoutSeconds: 0` to enqueue and return
immediately.
- **Wait for reply:** set a timeout and get the response inline.

Thread-scoped chat sessions, such as Slack or Discord keys ending in
`:thread:<id>`, are not valid `sessions_send` targets. Use the parent channel
session key for inter-agent coordination so tool-routed messages do not appear
inside an active human-facing thread.Messages and A2A follow-up replies are marked as inter-session data in the
receiving prompt (`[Inter-session message ... isUser=false]`) and in transcript
provenance. The receiving agent should treat them as tool-routed data, not as a
direct end-user-authored instruction.After the target responds, OpenClaw can run a **reply-back loop** where the
agents alternate messages (up to 5 turns). The target agent can reply
`REPLY_SKIP` to stop early.

## [​](https://docs.openclaw.ai/concepts/session-tool\#status-and-orchestration-helpers)  Status and orchestration helpers

`session_status` is the lightweight `/status`-equivalent tool for the current
or another visible session. It reports usage, time, model/runtime state, and
linked background-task context when present. Like `/status`, it can backfill
sparse token/cache counters from the latest transcript usage entry, and
`model=default` clears a per-session override. Use `sessionKey="current"` for
the caller’s current session; visible client labels such as `openclaw-tui` are
not session keys.`sessions_yield` intentionally ends the current turn so the next message can be
the follow-up event you are waiting for. Use it after spawning sub-agents when
you want completion results to arrive as the next message instead of building
poll loops.`subagents` is the control-plane helper for already spawned OpenClaw
sub-agents. It supports:

- `action: "list"` to inspect active/recent runs
- `action: "steer"` to send follow-up guidance to a running child
- `action: "kill"` to stop one child or `all`

## [​](https://docs.openclaw.ai/concepts/session-tool\#spawning-sub-agents)  Spawning sub-agents

`sessions_spawn` creates an isolated session for a background task by default.
It is always non-blocking — it returns immediately with a `runId` and
`childSessionKey`.Key options:

- `runtime: "subagent"` (default) or `"acp"` for external harness agents.
- `model` and `thinking` overrides for the child session.
- `thread: true` to bind the spawn to a chat thread (Discord, Slack, etc.).
- `sandbox: "require"` to enforce sandboxing on the child.
- `context: "fork"` for native sub-agents when the child needs the current
requester transcript; omit it or use `context: "isolated"` for a clean child.
Thread-bound native sub-agents default to `context: "fork"` unless
`threadBindings.defaultSpawnContext` says otherwise.

Default leaf sub-agents do not get session tools. When
`maxSpawnDepth >= 2`, depth-1 orchestrator sub-agents additionally receive
`sessions_spawn`, `subagents`, `sessions_list`, and `sessions_history` so they
can manage their own children. Leaf runs still do not get recursive
orchestration tools.After completion, an announce step posts the result to the requester’s channel.
Completion delivery preserves bound thread/topic routing when available, and if
the completion origin only identifies a channel OpenClaw can still reuse the
requester session’s stored route (`lastChannel` / `lastTo`) for direct
delivery.For ACP-specific behavior, see [ACP Agents](https://docs.openclaw.ai/tools/acp-agents).

## [​](https://docs.openclaw.ai/concepts/session-tool\#visibility)  Visibility

Session tools are scoped to limit what the agent can see:

| Level | Scope |
| --- | --- |
| `self` | Only the current session |
| `tree` | Current session + spawned sub-agents |
| `agent` | All sessions for this agent |
| `all` | All sessions (cross-agent if configured) |

Default is `tree`. Sandboxed sessions are clamped to `tree` regardless of
config.

## [​](https://docs.openclaw.ai/concepts/session-tool\#further-reading)  Further reading

- [Session Management](https://docs.openclaw.ai/concepts/session) — routing, lifecycle, maintenance
- [ACP Agents](https://docs.openclaw.ai/tools/acp-agents) — external harness spawning
- [Multi-agent](https://docs.openclaw.ai/concepts/multi-agent) — multi-agent architecture
- [Gateway Configuration](https://docs.openclaw.ai/gateway/configuration) — session tool config knobs

## [​](https://docs.openclaw.ai/concepts/session-tool\#related)  Related

- [Session management](https://docs.openclaw.ai/concepts/session)
- [Session pruning](https://docs.openclaw.ai/concepts/session-pruning)

[Session pruning](https://docs.openclaw.ai/concepts/session-pruning) [Memory overview](https://docs.openclaw.ai/concepts/memory)

Ctrl+I