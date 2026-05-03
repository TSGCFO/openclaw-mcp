---
source_url: https://docs.openclaw.ai/concepts/agent
title: "Agent runtime - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/agent#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Fundamentals

Agent runtime

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Workspace (required)](https://docs.openclaw.ai/concepts/agent#workspace-required)
- [Bootstrap files (injected)](https://docs.openclaw.ai/concepts/agent#bootstrap-files-injected)
- [Built-in tools](https://docs.openclaw.ai/concepts/agent#built-in-tools)
- [Skills](https://docs.openclaw.ai/concepts/agent#skills)
- [Runtime boundaries](https://docs.openclaw.ai/concepts/agent#runtime-boundaries)
- [Sessions](https://docs.openclaw.ai/concepts/agent#sessions)
- [Steering while streaming](https://docs.openclaw.ai/concepts/agent#steering-while-streaming)
- [Model refs](https://docs.openclaw.ai/concepts/agent#model-refs)
- [Configuration (minimal)](https://docs.openclaw.ai/concepts/agent#configuration-minimal)
- [Related](https://docs.openclaw.ai/concepts/agent#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw runs a **single embedded agent runtime** — one agent process per
Gateway, with its own workspace, bootstrap files, and session store. This page
covers that runtime contract: what the workspace must contain, which files get
injected, and how sessions bootstrap against it.

## [​](https://docs.openclaw.ai/concepts/agent\#workspace-required)  Workspace (required)

OpenClaw uses a single agent workspace directory (`agents.defaults.workspace`) as the agent’s **only** working directory (`cwd`) for tools and context.Recommended: use `openclaw setup` to create `~/.openclaw/openclaw.json` if missing and initialize the workspace files.Full workspace layout + backup guide: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)If `agents.defaults.sandbox` is enabled, non-main sessions can override this with
per-session workspaces under `agents.defaults.sandbox.workspaceRoot` (see
[Gateway configuration](https://docs.openclaw.ai/gateway/configuration)).

## [​](https://docs.openclaw.ai/concepts/agent\#bootstrap-files-injected)  Bootstrap files (injected)

Inside `agents.defaults.workspace`, OpenClaw expects these user-editable files:

- `AGENTS.md` — operating instructions + “memory”
- `SOUL.md` — persona, boundaries, tone
- `TOOLS.md` — user-maintained tool notes (e.g. `imsg`, `sag`, conventions)
- `BOOTSTRAP.md` — one-time first-run ritual (deleted after completion)
- `IDENTITY.md` — agent name/vibe/emoji
- `USER.md` — user profile + preferred address

On the first turn of a new session, OpenClaw injects the contents of these files directly into the agent context.Blank files are skipped. Large files are trimmed and truncated with a marker so prompts stay lean (read the file for full content).If a file is missing, OpenClaw injects a single “missing file” marker line (and `openclaw setup` will create a safe default template).`BOOTSTRAP.md` is only created for a **brand new workspace** (no other bootstrap files present). If you delete it after completing the ritual, it should not be recreated on later restarts.To disable bootstrap file creation entirely (for pre-seeded workspaces), set:

```
{ agents: { defaults: { skipBootstrap: true } } }
```

## [​](https://docs.openclaw.ai/concepts/agent\#built-in-tools)  Built-in tools

Core tools (read/exec/edit/write and related system tools) are always available,
subject to tool policy. `apply_patch` is optional and gated by
`tools.exec.applyPatch`. `TOOLS.md` does **not** control which tools exist; it’s
guidance for how _you_ want them used.

## [​](https://docs.openclaw.ai/concepts/agent\#skills)  Skills

OpenClaw loads skills from these locations (highest precedence first):

- Workspace: `<workspace>/skills`
- Project agent skills: `<workspace>/.agents/skills`
- Personal agent skills: `~/.agents/skills`
- Managed/local: `~/.openclaw/skills`
- Bundled (shipped with the install)
- Extra skill folders: `skills.load.extraDirs`

Skills can be gated by config/env (see `skills` in [Gateway configuration](https://docs.openclaw.ai/gateway/configuration)).

## [​](https://docs.openclaw.ai/concepts/agent\#runtime-boundaries)  Runtime boundaries

The embedded agent runtime is built on the Pi agent core (models, tools, and
prompt pipeline). Session management, discovery, tool wiring, and channel
delivery are OpenClaw-owned layers on top of that core.

## [​](https://docs.openclaw.ai/concepts/agent\#sessions)  Sessions

Session transcripts are stored as JSONL at:

- `~/.openclaw/agents/<agentId>/sessions/<SessionId>.jsonl`

The session ID is stable and chosen by OpenClaw.
Legacy session folders from other tools are not read.

## [​](https://docs.openclaw.ai/concepts/agent\#steering-while-streaming)  Steering while streaming

When queue mode is `steer`, inbound messages are injected into the current run.
Queued steering is delivered **after the current assistant turn finishes**
**executing its tool calls**, before the next LLM call. Pi drains all pending
steering messages together for `steer`; legacy `queue` drains one message per
model boundary. Steering no longer skips remaining tool calls from the current
assistant message.When queue mode is `followup` or `collect`, inbound messages are held until the
current turn ends, then a new agent turn starts with the queued payloads. See
[Queue](https://docs.openclaw.ai/concepts/queue) and [Steering queue](https://docs.openclaw.ai/concepts/queue-steering) for mode
and boundary behavior.Block streaming sends completed assistant blocks as soon as they finish; it is
**off by default** (`agents.defaults.blockStreamingDefault: "off"`).
Tune the boundary via `agents.defaults.blockStreamingBreak` (`text_end` vs `message_end`; defaults to text\_end).
Control soft block chunking with `agents.defaults.blockStreamingChunk` (defaults to
800–1200 chars; prefers paragraph breaks, then newlines; sentences last).
Coalesce streamed chunks with `agents.defaults.blockStreamingCoalesce` to reduce
single-line spam (idle-based merging before send). Non-Telegram channels require
explicit `*.blockStreaming: true` to enable block replies.
Verbose tool summaries are emitted at tool start (no debounce); Control UI
streams tool output via agent events when available.
More details: [Streaming + chunking](https://docs.openclaw.ai/concepts/streaming).

## [​](https://docs.openclaw.ai/concepts/agent\#model-refs)  Model refs

Model refs in config (for example `agents.defaults.model` and `agents.defaults.models`) are parsed by splitting on the **first**`/`.

- Use `provider/model` when configuring models.
- If the model ID itself contains `/` (OpenRouter-style), include the provider prefix (example: `openrouter/moonshotai/kimi-k2`).
- If you omit the provider, OpenClaw tries an alias first, then a unique
configured-provider match for that exact model id, and only then falls back
to the configured default provider. If that provider no longer exposes the
configured default model, OpenClaw falls back to the first configured
provider/model instead of surfacing a stale removed-provider default.

## [​](https://docs.openclaw.ai/concepts/agent\#configuration-minimal)  Configuration (minimal)

At minimum, set:

- `agents.defaults.workspace`
- `channels.whatsapp.allowFrom` (strongly recommended)

* * *

_Next: [Group Chats](https://docs.openclaw.ai/channels/group-messages)_ 🦞

## [​](https://docs.openclaw.ai/concepts/agent\#related)  Related

- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)
- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)
- [Session management](https://docs.openclaw.ai/concepts/session)

[Gateway architecture](https://docs.openclaw.ai/concepts/architecture) [Agent loop](https://docs.openclaw.ai/concepts/agent-loop)

Ctrl+I