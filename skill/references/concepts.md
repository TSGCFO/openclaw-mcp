# Concepts

_41 pages from docs.openclaw.ai_


---

## `openclaw commitments` - OpenClaw

_Source: <https://docs.openclaw.ai/cli/commitments>_

[OpenClaw home page](https://docs.openclaw.ai/)

Agents and sessions

\`openclaw commitments\`

List and manage inferred follow-up commitments.Commitments are opt-in, short-lived follow-up memories created from
conversation context. See [Inferred commitments](https://docs.openclaw.ai/concepts/commitments) for the
conceptual guide.With no subcommand, `openclaw commitments` lists pending commitments.

## Usage

```
openclaw commitments [--all] [--agent <id>] [--status <status>] [--json]
openclaw commitments list [--all] [--agent <id>] [--status <status>] [--json]
openclaw commitments dismiss <id...> [--json]
```

## Options

- `--all`: show all statuses instead of only pending commitments.
- `--agent <id>`: filter to one agent id.
- `--status <status>`: filter by status. Values: `pending`, `sent`,
`dismissed`, `snoozed`, or `expired`.
- `--json`: output machine-readable JSON.

## Examples

List pending commitments:

```
openclaw commitments
```

List every stored commitment:

```
openclaw commitments --all
```

Filter to one agent:

```
openclaw commitments --agent main
```

Find snoozed commitments:

```
openclaw commitments --status snoozed
```

Dismiss one or more commitments:

```
openclaw commitments dismiss cm_abc123 cm_def456
```

Export as JSON:

```
openclaw commitments --all --json
```

## Output

Text output includes:

- commitment id
- status
- kind
- earliest due time
- scope
- suggested check-in text

JSON output also includes the commitment store path and full stored records.

## Related

- [Inferred commitments](https://docs.openclaw.ai/concepts/commitments)
- [Memory overview](https://docs.openclaw.ai/concepts/memory)
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat)
- [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs)

[Memory](https://docs.openclaw.ai/cli/memory) [Message](https://docs.openclaw.ai/cli/message)

Ctrl+I


---

## Active memory - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/active-memory>_

[OpenClaw home page](https://docs.openclaw.ai/)

Memory

Active memory

Active memory is an optional plugin-owned blocking memory sub-agent that runs
before the main reply for eligible conversational sessions.It exists because most memory systems are capable but reactive. They rely on
the main agent to decide when to search memory, or on the user to say things
like “remember this” or “search memory.” By then, the moment where memory would
have made the reply feel natural has already passed.Active memory gives the system one bounded chance to surface relevant memory
before the main reply is generated.

## Quick start

Paste this into `openclaw.json` for a safe-default setup — plugin on, scoped to
the `main` agent, direct-message sessions only, inherits the session model
when available:

```
{
  plugins: {
    entries: {
      "active-memory": {
        enabled: true,
        config: {
          enabled: true,
          agents: ["main"],
          allowedChatTypes: ["direct"],
          modelFallback: "google/gemini-3-flash",
          queryMode: "recent",
          promptStyle: "balanced",
          timeoutMs: 15000,
          maxSummaryChars: 220,
          persistTranscripts: false,
          logging: true,
        },
      },
    },
  },
}
```

Then restart the gateway:

```
openclaw gateway
```

To inspect it live in a conversation:

```
/verbose on
/trace on
```

What the key fields do:

- `plugins.entries.active-memory.enabled: true` turns the plugin on
- `config.agents: ["main"]` opts only the `main` agent into active memory
- `config.allowedChatTypes: ["direct"]` scopes it to direct-message sessions (opt in groups/channels explicitly)
- `config.model` (optional) pins a dedicated recall model; unset inherits the current session model
- `config.modelFallback` is used only when no explicit or inherited model resolves
- `config.promptStyle: "balanced"` is the default for `recent` mode
- Active memory still runs only for eligible interactive persistent chat sessions

## Speed recommendations

The simplest setup is to leave `config.model` unset and let Active Memory use
the same model you already use for normal replies. That is the safest default
because it follows your existing provider, auth, and model preferences.If you want Active Memory to feel faster, use a dedicated inference model
instead of borrowing the main chat model. Recall quality matters, but latency
matters more than for the main answer path, and Active Memory’s tool surface
is narrow (it only calls available memory recall tools).Good fast-model options:

- `cerebras/gpt-oss-120b` for a dedicated low-latency recall model
- `google/gemini-3-flash` as a low-latency fallback without changing your primary chat model
- your normal session model, by leaving `config.model` unset

### Cerebras setup

Add a Cerebras provider and point Active Memory at it:

```
{
  models: {
    providers: {
      cerebras: {
        baseUrl: "https://api.cerebras.ai/v1",
        apiKey: "${CEREBRAS_API_KEY}",
        api: "openai-completions",
        models: [{ id: "gpt-oss-120b", name: "GPT OSS 120B (Cerebras)" }],
      },
    },
  },
  plugins: {
    entries: {
      "active-memory": {
        enabled: true,
        config: { model: "cerebras/gpt-oss-120b" },
      },
    },
  },
}
```

Make sure the Cerebras API key actually has `chat/completions` access for the
chosen model — `/v1/models` visibility alone does not guarantee it.

## How to see it

Active memory injects a hidden untrusted prompt prefix for the model. It does
not expose raw `<active_memory_plugin>...</active_memory_plugin>` tags in the
normal client-visible reply.

## Session toggle

Use the plugin command when you want to pause or resume active memory for the
current chat session without editing config:

```
/active-memory status
/active-memory off
/active-memory on
```

This is session-scoped. It does not change
`plugins.entries.active-memory.enabled`, agent targeting, or other global
configuration.If you want the

_… [truncated; see https://docs.openclaw.ai/concepts/active-memory for full content]_


---

## Agent runtime - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/agent>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

Agent runtime

OpenClaw runs a **single embedded agent runtime** — one agent process per
Gateway, with its own workspace, bootstrap files, and session store. This page
covers that runtime contract: what the workspace must contain, which files get
injected, and how sessions bootstrap against it.

## Workspace (required)

OpenClaw uses a single agent workspace directory (`agents.defaults.workspace`) as the agent’s **only** working directory (`cwd`) for tools and context.Recommended: use `openclaw setup` to create `~/.openclaw/openclaw.json` if missing and initialize the workspace files.Full workspace layout + backup guide: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)If `agents.defaults.sandbox` is enabled, non-main sessions can override this with
per-session workspaces under `agents.defaults.sandbox.workspaceRoot` (see
[Gateway configuration](https://docs.openclaw.ai/gateway/configuration)).

## Bootstrap files (injected)

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

## Built-in tools

Core tools (read/exec/edit/write and related system tools) are always available,
subject to tool policy. `apply_patch` is optional and gated by
`tools.exec.applyPatch`. `TOOLS.md` does **not** control which tools exist; it’s
guidance for how _you_ want them used.

## Skills

OpenClaw loads skills from these locations (highest precedence first):

- Workspace: `<workspace>/skills`
- Project agent skills: `<workspace>/.agents/skills`
- Personal agent skills: `~/.agents/skills`
- Managed/local: `~/.openclaw/skills`
- Bundled (shipped with the install)
- Extra skill folders: `skills.load.extraDirs`

Skills can be gated by config/env (see `skills` in [Gateway configuration](https://docs.openclaw.ai/gateway/configuration)).

## Runtime boundaries

The embedded agent runtime is built on the Pi agent core (models, tools, and
prompt pipeline). Session management, discovery, tool wiring, and channel
delivery are OpenClaw-owned layers on top of that core.

## Sessions

Session transcripts are stored as JSONL at:

- `~/.openclaw/agents/<agentId>/sessions/<SessionId>.jsonl`

The session ID is stable and chosen by OpenClaw.
Legacy session folders from other tools are not read.

## Steering while streaming

When queue mode is `steer`, inbound messages are injected into the current run.
Queued steering is delivered **after the current assistant turn finishes**
**executing its tool calls**, before the next LLM call. Pi drains all pending
steering messages together for `steer`; legacy `queue` drains one message per
model boundary. Steering no longer skips remaining tool calls from the current
assistant message.When queue mode is `followup` or `collect`, inbound messages are held until the
current turn ends, then a new agent turn starts with the queued payloads. See
[Queue](https://docs.openclaw.ai/concepts/queue) and [Steering queue](h

_… [truncated; see https://docs.openclaw.ai/concepts/agent for full content]_


---

## Agent loop - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/agent-loop>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

Agent loop

An agentic loop is the full “real” run of an agent: intake → context assembly → model inference →
tool execution → streaming replies → persistence. It’s the authoritative path that turns a message
into actions and a final reply, while keeping session state consistent.In OpenClaw, a loop is a single, serialized run per session that emits lifecycle and stream events
as the model thinks, calls tools, and streams output. This doc explains how that authentic loop is
wired end-to-end.

## Entry points

- Gateway RPC: `agent` and `agent.wait`.
- CLI: `agent` command.

## How it works (high-level)

1. `agent` RPC validates params, resolves session (sessionKey/sessionId), persists session metadata, returns `{ runId, acceptedAt }` immediately.
2. `agentCommand`runs the agent:

   - resolves model + thinking/verbose/trace defaults
   - loads skills snapshot
   - calls `runEmbeddedPiAgent` (pi-agent-core runtime)
   - emits **lifecycle end/error** if the embedded loop does not emit one
3. `runEmbeddedPiAgent`:

   - serializes runs via per-session + global queues
   - resolves model + auth profile and builds the pi session
   - subscribes to pi events and streams assistant/tool deltas
   - enforces timeout -> aborts run if exceeded
   - for Codex app-server turns, aborts an accepted turn that stops producing app-server progress before a terminal event
   - returns payloads + usage metadata
4. `subscribeEmbeddedPiSession` bridges pi-agent-core events to OpenClaw `agent` stream:

   - tool events => `stream: "tool"`
   - assistant deltas => `stream: "assistant"`
   - lifecycle events => `stream: "lifecycle"` (`phase: "start" | "end" | "error"`)
5. `agent.wait` uses `waitForAgentRun`:

   - waits for **lifecycle end/error** for `runId`
   - returns `{ status: ok|error|timeout, startedAt, endedAt, error? }`

## Queueing + concurrency

- Runs are serialized per session key (session lane) and optionally through a global lane.
- This prevents tool/session races and keeps session history consistent.
- Messaging channels can choose queue modes (collect/steer/followup) that feed this lane system.
See [Command Queue](https://docs.openclaw.ai/concepts/queue).
- Transcript writes are also protected by a session write lock on the session file. The lock is
process-aware and file-based, so it catches writers that bypass the in-process queue or come from
another process.
- Session write locks are non-reentrant by default. If a helper intentionally nests acquisition of
the same lock while preserving one logical writer, it must opt in explicitly with
`allowReentrant: true`.

## Session + workspace preparation

- Workspace is resolved and created; sandboxed runs may redirect to a sandbox workspace root.
- Skills are loaded (or reused from a snapshot) and injected into env and prompt.
- Bootstrap/context files are resolved and injected into the system prompt report.
- A session write lock is acquired; `SessionManager` is opened and prepared before streaming. Any
later transcript rewrite, compaction, or truncation path must take the same lock before opening or
mutating the transcript file.

## Prompt assembly + system prompt

- System prompt is built from OpenClaw’s base prompt, skills prompt, bootstrap context, and per-run overrides.
- Model-specific limits and compaction reserve tokens are enforced.
- See [System prompt](https://docs.openclaw.ai/concepts/system-prompt) for what the model sees.

## Hook points (where you can intercept)

OpenClaw has two hook systems:

- **Internal hooks** (Gateway hooks): event-driven scripts for commands and lifecycle events.
- **Plugin hooks**: extension points inside the agent/tool lifecycle and gateway pipeline.

### Internal hooks (Gateway hooks)

- **`agent:bootstrap`**: runs while building bootstrap files before the system prompt is finalized.
Use this to add/remove bootstrap context files.
- **Command hooks**: `/new`, `/reset`, `/stop`, and oth

_… [truncated; see https://docs.openclaw.ai/concepts/agent-loop for full content]_


---

## Agent runtimes - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/agent-runtimes>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

Agent runtimes

An **agent runtime** is the component that owns one prepared model loop: it
receives the prompt, drives model output, handles native tool calls, and returns
the finished turn to OpenClaw.Runtimes are easy to confuse with providers because both show up near model
configuration. They are different layers:

| Layer | Examples | What it means |
| --- | --- | --- |
| Provider | `openai`, `anthropic`, `openai-codex` | How OpenClaw authenticates, discovers models, and names model refs. |
| Model | `gpt-5.5`, `claude-opus-4-6` | The model selected for the agent turn. |
| Agent runtime | `pi`, `codex`, `claude-cli` | The low level loop or backend that executes the prepared turn. |
| Channel | Telegram, Discord, Slack, WhatsApp | Where messages enter and leave OpenClaw. |

You will also see the word **harness** in code. A harness is the implementation
that provides an agent runtime. For example, the bundled Codex harness
implements the `codex` runtime. Public config uses `agentRuntime.id`; `openclaw doctor --fix` rewrites older runtime-policy keys to that shape.There are two runtime families:

- **Embedded harnesses** run inside OpenClaw’s prepared agent loop. Today this
is the built-in `pi` runtime plus registered plugin harnesses such as
`codex`.
- **CLI backends** run a local CLI process while keeping the model ref
canonical. For example, `anthropic/claude-opus-4-7` with
`agentRuntime.id: "claude-cli"` means “select the Anthropic model, execute
through Claude CLI.” `claude-cli` is not an embedded harness id and must not
be passed to AgentHarness selection.

## Codex surfaces

Most confusion comes from several different surfaces sharing the Codex name:

| Surface | OpenClaw name/config | What it does |
| --- | --- | --- |
| Native Codex app-server runtime | `openai/*` plus `agentRuntime.id: "codex"` | Runs the embedded agent turn through Codex app-server. This is the usual ChatGPT/Codex subscription setup. |
| Codex OAuth provider route | `openai-codex/*` model refs | Uses ChatGPT/Codex subscription OAuth through the normal OpenClaw PI runner. |
| Codex ACP adapter | `runtime: "acp"`, `agentId: "codex"` | Runs Codex through the external ACP/acpx control plane. Use only when ACP/acpx is explicitly asked. |
| Native Codex chat-control command set | `/codex ...` | Binds, resumes, steers, stops, and inspects Codex app-server threads from chat. |
| OpenAI Platform API route for GPT/Codex-style models | `openai/*` model refs | Uses OpenAI API-key auth unless a runtime override, such as `agentRuntime.id: "codex"`, runs the turn. |

Those surfaces are intentionally independent. Enabling the `codex` plugin makes
the native app-server features available; it does not rewrite
`openai-codex/*` into `openai/*`, does not change existing sessions, and does
not make ACP the Codex default. Selecting `openai-codex/*` means “use the Codex
OAuth provider route” unless you separately force a runtime.The common ChatGPT/Codex subscription setup uses Codex OAuth for auth, but keeps
the model ref as `openai/*` and selects the `codex` runtime:

```
{
  agents: {
    defaults: {
      model: "openai/gpt-5.5",
      agentRuntime: {
        id: "codex",
      },
    },
  },
}
```

That means OpenClaw selects an OpenAI model ref, then asks the Codex app-server
runtime to run the embedded agent turn. It does not mean “use API billing,” and
it does not mean the channel, model provider catalog, or OpenClaw session store
becomes Codex.When the bundled `codex` plugin is enabled, natural-language Codex control
should use the native `/codex` command surface (`/codex bind`, `/codex threads`,
`/codex resume`, `/codex steer`, `/codex stop`) instead of ACP. Use ACP for
Codex only when the user explicitly asks for ACP/acpx or is testing the ACP
adapter path. Claude Code, Gemini CLI, OpenCode, Cursor, and similar external
harnesses still use ACP.This is the agent-facing decision tree:

1. If the user

_… [truncated; see https://docs.openclaw.ai/concepts/agent-runtimes for full content]_


---

## Agent workspace - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/agent-workspace>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

Agent workspace

The workspace is the agent’s home. It is the only working directory used for file tools and for workspace context. Keep it private and treat it as memory.This is separate from `~/.openclaw/`, which stores config, credentials, and sessions.

The workspace is the **default cwd**, not a hard sandbox. Tools resolve relative paths against the workspace, but absolute paths can still reach elsewhere on the host unless sandboxing is enabled. If you need isolation, use [`agents.defaults.sandbox`](https://docs.openclaw.ai/gateway/sandboxing) (and/or per-agent sandbox config).When sandboxing is enabled and `workspaceAccess` is not `"rw"`, tools operate inside a sandbox workspace under `~/.openclaw/sandboxes`, not your host workspace.

## Default location

- Default: `~/.openclaw/workspace`
- If `OPENCLAW_PROFILE` is set and not `"default"`, the default becomes `~/.openclaw/workspace-<profile>`.
- Override in `~/.openclaw/openclaw.json`:

```
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
    },
  },
}
```

`openclaw onboard`, `openclaw configure`, or `openclaw setup` will create the workspace and seed the bootstrap files if they are missing.

Sandbox seed copies only accept regular in-workspace files; symlink/hardlink aliases that resolve outside the source workspace are ignored.

If you already manage the workspace files yourself, you can disable bootstrap file creation:

```
{ agents: { defaults: { skipBootstrap: true } } }
```

## Extra workspace folders

Older installs may have created `~/openclaw`. Keeping multiple workspace directories around can cause confusing auth or state drift, because only one workspace is active at a time.

**Recommendation:** keep a single active workspace. If you no longer use the extra folders, archive or move them to Trash (for example `trash ~/openclaw`). If you intentionally keep multiple workspaces, make sure `agents.defaults.workspace` points to the active one.`openclaw doctor` warns when it detects extra workspace directories.

## Workspace file map

These are the standard files OpenClaw expects inside the workspace:

AGENTS.md — operating instructions

Operating instructions for the agent and how it should use memory. Loaded at the start of every session. Good place for rules, priorities, and “how to behave” details.

SOUL.md — persona and tone

Persona, tone, and boundaries. Loaded every session. Guide: [SOUL.md personality guide](https://docs.openclaw.ai/concepts/soul).

USER.md — who the user is

Who the user is and how to address them. Loaded every session.

IDENTITY.md — name, vibe, emoji

The agent’s name, vibe, and emoji. Created/updated during the bootstrap ritual.

TOOLS.md — local tool conventions

Notes about your local tools and conventions. Does not control tool availability; it is only guidance.

HEARTBEAT.md — heartbeat checklist

Optional tiny checklist for heartbeat runs. Keep it short to avoid token burn.

BOOT.md — startup checklist

Optional startup checklist run automatically on gateway restart (when [internal hooks](https://docs.openclaw.ai/automation/hooks) are enabled). Keep it short; use the message tool for outbound sends.

BOOTSTRAP.md — first-run ritual

One-time first-run ritual. Only created for a brand-new workspace. Delete it after the ritual is complete.

memory/YYYY-MM-DD.md — daily memory log

Daily memory log (one file per day). Recommended to read today + yesterday on session start.

MEMORY.md — curated long-term memory (optional)

Curated long-term memory. Only load in the main, private session (not shared/group contexts). See [Memory](https://docs.openclaw.ai/concepts/memory) for the workflow and automatic memory flush.

skills/ — workspace skills (optional)

Workspace-specific skills. Highest-precedence skill location for that workspace. Overrides project agent skills, personal agent skills, managed skills, bundled skills, and `skills.load.extraDirs` when

_… [truncated; see https://docs.openclaw.ai/concepts/agent-workspace for full content]_


---

## https://docs.openclaw.ai/concepts/agent-workspace.md

_Source: <https://docs.openclaw.ai/concepts/agent-workspace.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Agent workspace

The workspace is the agent's home. It is the only working directory used for file tools and for workspace context. Keep it private and treat it as memory.

This is separate from \`~/.openclaw/\`, which stores config, credentials, and sessions.

 The workspace is the \*\*default cwd\*\*, not a hard sandbox. Tools resolve relative paths against the workspace, but absolute paths can still reach elsewhere on the host unless sandboxing is enabled. If you need isolation, use \[\`agents.defaults.sandbox\`\](/gateway/sandboxing) (and/or per-agent sandbox config).

 When sandboxing is enabled and \`workspaceAccess\` is not \`"rw"\`, tools operate inside a sandbox workspace under \`~/.openclaw/sandboxes\`, not your host workspace.

\## Default location

\\* Default: \`~/.openclaw/workspace\`
\\* If \`OPENCLAW\_PROFILE\` is set and not \`"default"\`, the default becomes \`~/.openclaw/workspace-\`.
\\* Override in \`~/.openclaw/openclaw.json\`:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 workspace: "~/.openclaw/workspace",
 },
 },
}
\`\`\`

\`openclaw onboard\`, \`openclaw configure\`, or \`openclaw setup\` will create the workspace and seed the bootstrap files if they are missing.

 Sandbox seed copies only accept regular in-workspace files; symlink/hardlink aliases that resolve outside the source workspace are ignored.

If you already manage the workspace files yourself, you can disable bootstrap file creation:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ agents: { defaults: { skipBootstrap: true } } }
\`\`\`

\## Extra workspace folders

Older installs may have created \`~/openclaw\`. Keeping multiple workspace directories around can cause confusing auth or state drift, because only one workspace is active at a time.

 \*\*Recommendation:\*\* keep a single active workspace. If you no longer use the extra folders, archive or move them to Trash (for example \`trash ~/openclaw\`). If you intentionally keep multiple workspaces, make sure \`agents.defaults.workspace\` points to the active one.

 \`openclaw doctor\` warns when it detects extra workspace directories.

\## Workspace file map

These are the standard files OpenClaw expects inside the workspace:

 Operating instructions for the agent and how it should use memory. Loaded at the start of every session. Good place for rules, priorities, and "how to behave" details.

 Persona, tone, and boundaries. Loaded every session. Guide: \[SOUL.md personality guide\](/concepts/soul).

 Who the user is and how to address them. Loaded every session.

 The agent's name, vibe, and emoji. Created/updated during the bootstrap ritual.

 Notes about your local tools and conventions. Does not control tool availability; it is only guidance.

 Optional tiny checklist for heartbeat runs. Keep it short to avoid token burn.

 Optional startup checklist run automatically on gateway restart (when \[internal hooks\](/automation/hooks) are enabled). Keep it short; use the message tool for outbound sends.

 One-time first-run ritual. Only created for a brand-new workspace. Delete it after the ritual is complete.

 Daily memory log (one file per day). Recommended to read today + yesterday on session start.

 Curated long-term memory. Only load in the main, private session (not shared/group contexts). See \[Memory\](/concepts/memory) for the workflow and automatic memory flush.

 Workspace-specific skills. Highest-precedence skill location for that workspace. Overrides project agent skills, personal agent skills, managed skills, bundled skills, and \`skills.load.extraDirs\` when names collide.

 Canvas UI files for node displays (for example \`canvas/index.html\`).

 If any bootstrap file is missing, OpenClaw injects a "missing file" marker into the session and co

_… [truncated; see https://docs.openclaw.ai/concepts/agent-workspace.md for full content]_


---

## Gateway architecture - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/architecture>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

Gateway architecture

## Overview

- A single long‑lived **Gateway** owns all messaging surfaces (WhatsApp via
Baileys, Telegram via grammY, Slack, Discord, Signal, iMessage, WebChat).
- Control-plane clients (macOS app, CLI, web UI, automations) connect to the
Gateway over **WebSocket** on the configured bind host (default
`127.0.0.1:18789`).
- **Nodes** (macOS/iOS/Android/headless) also connect over **WebSocket**, but
declare `role: node` with explicit caps/commands.
- One Gateway per host; it is the only place that opens a WhatsApp session.
- The **canvas host** is served by the Gateway HTTP server under:

  - `/__openclaw__/canvas/` (agent-editable HTML/CSS/JS)
  - `/__openclaw__/a2ui/` (A2UI host)
    It uses the same port as the Gateway (default `18789`).

## Components and flows

### Gateway (daemon)

- Maintains provider connections.
- Exposes a typed WS API (requests, responses, server‑push events).
- Validates inbound frames against JSON Schema.
- Emits events like `agent`, `chat`, `presence`, `health`, `heartbeat`, `cron`.

### Clients (mac app / CLI / web admin)

- One WS connection per client.
- Send requests (`health`, `status`, `send`, `agent`, `system-presence`).
- Subscribe to events (`tick`, `agent`, `presence`, `shutdown`).

### Nodes (macOS / iOS / Android / headless)

- Connect to the **same WS server** with `role: node`.
- Provide a device identity in `connect`; pairing is **device‑based** (role `node`) and
approval lives in the device pairing store.
- Expose commands like `canvas.*`, `camera.*`, `screen.record`, `location.get`.

Protocol details:

- [Gateway protocol](https://docs.openclaw.ai/gateway/protocol)

### WebChat

- Static UI that uses the Gateway WS API for chat history and sends.
- In remote setups, connects through the same SSH/Tailscale tunnel as other
clients.

## Connection lifecycle (single client)

GatewayClientGatewayClientor res error + closepayload=hello-oksnapshot: presence + healthreq:connectres (ok)event:presenceevent:tickreq:agentres:agentack {runId, status:"accepted"}event:agent(streaming)res:agentfinal {runId, status, summary}

## Wire protocol (summary)

- Transport: WebSocket, text frames with JSON payloads.
- First frame **must** be `connect`.
- After handshake:
  - Requests: `{type:"req", id, method, params}` → `{type:"res", id, ok, payload|error}`
  - Events: `{type:"event", event, payload, seq?, stateVersion?}`
- `hello-ok.features.methods` / `events` are discovery metadata, not a
generated dump of every callable helper route.
- Shared-secret auth uses `connect.params.auth.token` or
`connect.params.auth.password`, depending on the configured gateway auth mode.
- Identity-bearing modes such as Tailscale Serve
(`gateway.auth.allowTailscale: true`) or non-loopback
`gateway.auth.mode: "trusted-proxy"` satisfy auth from request headers
instead of `connect.params.auth.*`.
- Private-ingress `gateway.auth.mode: "none"` disables shared-secret auth
entirely; keep that mode off public/untrusted ingress.
- Idempotency keys are required for side‑effecting methods (`send`, `agent`) to
safely retry; the server keeps a short‑lived dedupe cache.
- Nodes must include `role: "node"` plus caps/commands/permissions in `connect`.

## Pairing + local trust

- All WS clients (operators + nodes) include a **device identity** on `connect`.
- New device IDs require pairing approval; the Gateway issues a **device token**
for subsequent connects.
- Direct local loopback connects can be auto-approved to keep same-host UX
smooth.
- OpenClaw also has a narrow backend/container-local self-connect path for
trusted shared-secret helper flows.
- Tailnet and LAN connects, including same-host tailnet binds, still require
explicit pairing approval.
- All connects must sign the `connect.challenge` nonce.
- Signature payload `v3` also binds `platform` \+ `deviceFamily`; the gateway
pins paired metadata on reconnect and requires repair pairing for met

_… [truncated; see https://docs.openclaw.ai/concepts/architecture for full content]_


---

## Inferred commitments - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/commitments>_

[OpenClaw home page](https://docs.openclaw.ai/)

Memory

Inferred commitments

Commitments are short-lived follow-up memories. When enabled, OpenClaw can
notice that a conversation created a future check-in opportunity and remember
to bring it back later.Examples:

- You mention an interview tomorrow. OpenClaw may check in afterward.
- You say you are exhausted. OpenClaw may ask later whether you slept.
- The agent says it will follow up after something changes. OpenClaw may track
that open loop.

Commitments are not durable facts like `MEMORY.md`, and they are not exact
reminders. They sit between memory and automation: OpenClaw remembers a
conversation-bound obligation, then heartbeat delivers it when it is due.

## Enable commitments

Commitments are off by default. Enable them in config:

```
openclaw config set commitments.enabled true
openclaw config set commitments.maxPerDay 3
```

Equivalent `openclaw.json`:

```
{
  "commitments": {
    "enabled": true,
    "maxPerDay": 3
  }
}
```

`commitments.maxPerDay` limits how many inferred follow-ups can be delivered
per agent session in a rolling day. The default is `3`.

## How it works

After an agent reply, OpenClaw may run a hidden background extraction pass in a
separate context. That pass looks only for inferred follow-up commitments. It
does not write into the visible conversation and it does not ask the main agent
to reason about the extraction.When it finds a high-confidence candidate, OpenClaw stores a commitment with:

- the agent id
- the session key
- the original channel and delivery target
- a due window
- a short suggested check-in
- non-instructional metadata for heartbeat to decide whether to send it

Delivery happens through heartbeat. When a commitment becomes due, heartbeat
adds the commitment to the heartbeat turn for the same agent and channel scope.
The model can send one natural check-in or reply `HEARTBEAT_OK` to dismiss it.
If heartbeat is configured with `target: "none"`, due commitments remain
internal and do not send external check-ins. Commitment delivery prompts do not
replay the original conversation text, and due commitment heartbeat turns run
without OpenClaw tools.OpenClaw never delivers an inferred commitment immediately after writing it.
The due time is clamped to at least one heartbeat interval after the commitment
is created, so the follow-up cannot echo back in the same moment it was
inferred.

## Scope

Commitments are scoped to the exact agent and channel context where they were
created. A follow-up inferred while talking to one agent in Discord is not
delivered by another agent, another channel, or an unrelated session.This scope is part of the feature. Natural check-ins should feel like the same
conversation continuing, not like a global reminder system.

## Commitments vs reminders

| Need | Use |
| --- | --- |
| ”Remind me at 3 PM” | [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs) |
| ”Ping me in 20 minutes” | [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs) |
| ”Run this report every weekday” | [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs) |
| ”I have an interview tomorrow” | Commitments |
| ”I was up all night” | Commitments |
| ”Follow up if I do not answer this open thread” | Commitments |

Exact user requests already belong to the scheduler path. Commitments are only
for inferred follow-ups: the moments where the user did not ask for a reminder,
but the conversation clearly created a useful future check-in.

## Manage commitments

Use the CLI to inspect and clear stored commitments:

```
openclaw commitments
openclaw commitments --all
openclaw commitments --agent main
openclaw commitments --status snoozed
openclaw commitments dismiss cm_abc123
```

See [`openclaw commitments`](https://docs.openclaw.ai/cli/commitments) for the command reference.

## Privacy and cost

Commitment extraction uses an LLM pass, so enabling it adds background model
usage after eligible turns. The

_… [truncated; see https://docs.openclaw.ai/concepts/commitments for full content]_


---

## Compaction - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/compaction>_

[OpenClaw home page](https://docs.openclaw.ai/)

Sessions and memory

Compaction

Every model has a context window: the maximum number of tokens it can process. When a conversation approaches that limit, OpenClaw **compacts** older messages into a summary so the chat can continue.

## How it works

1. Older conversation turns are summarized into a compact entry.
2. The summary is saved in the session transcript.
3. Recent messages are kept intact.

When OpenClaw splits history into compaction chunks, it keeps assistant tool calls paired with their matching `toolResult` entries. If a split point lands inside a tool block, OpenClaw moves the boundary so the pair stays together and the current unsummarized tail is preserved.The full conversation history stays on disk. Compaction only changes what the model sees on the next turn.

## Auto-compaction

Auto-compaction is on by default. It runs when the session nears the context limit, or when the model returns a context-overflow error (in which case OpenClaw compacts and retries).You will see:

- `🧹 Auto-compaction complete` in verbose mode.
- `/status` showing `🧹 Compactions: <count>`.

Before compacting, OpenClaw automatically reminds the agent to save important notes to [memory](https://docs.openclaw.ai/concepts/memory) files. This prevents context loss.

Recognized overflow signatures

OpenClaw detects context overflow from these provider error patterns:

- `request_too_large`
- `context length exceeded`
- `input exceeds the maximum number of tokens`
- `input token count exceeds the maximum number of input tokens`
- `input is too long for the model`
- `ollama error: context length exceeded`

## Manual compaction

Type `/compact` in any chat to force a compaction. Add instructions to guide the summary:

```
/compact Focus on the API design decisions
```

When `agents.defaults.compaction.keepRecentTokens` is set, manual compaction honors that Pi cut-point and keeps the recent tail in rebuilt context. Without an explicit keep budget, manual compaction behaves as a hard checkpoint and continues from the new summary alone.

## Configuration

Configure compaction under `agents.defaults.compaction` in your `openclaw.json`. The most common knobs are listed below; for the full reference, see [Session management deep dive](https://docs.openclaw.ai/reference/session-management-compaction).

### Using a different model

By default, compaction uses the agent’s primary model. Set `agents.defaults.compaction.model` to delegate summarization to a more capable or specialized model. The override accepts any `provider/model-id` string:

```
{
  "agents": {
    "defaults": {
      "compaction": {
        "model": "openrouter/anthropic/claude-sonnet-4-6"
      }
    }
  }
}
```

This works with local models too, for example a second Ollama model dedicated to summarization:

```
{
  "agents": {
    "defaults": {
      "compaction": {
        "model": "ollama/llama3.1:8b"
      }
    }
  }
}
```

When unset, compaction starts with the active session model. If summarization fails with a model-fallback-eligible provider error, OpenClaw retries that compaction attempt through the session’s existing model fallback chain. The fallback choice is temporary and is not written back to session state. An explicit `agents.defaults.compaction.model` override remains exact and does not inherit the session fallback chain.

### Identifier preservation

Compaction summarization preserves opaque identifiers by default (`identifierPolicy: "strict"`). Override with `identifierPolicy: "off"` to disable, or `identifierPolicy: "custom"` plus `identifierInstructions` for custom guidance.

### Active transcript byte guard

When `agents.defaults.compaction.maxActiveTranscriptBytes` is set, OpenClaw triggers normal local compaction before a run if the active JSONL reaches that size. This is useful for long-running sessions where provider-side context management may keep model context healthy while the local transcript keeps gro

_… [truncated; see https://docs.openclaw.ai/concepts/compaction for full content]_


---

## Context - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/context>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

Context

“Context” is **everything OpenClaw sends to the model for a run**. It is bounded by the model’s **context window** (token limit).Beginner mental model:

- **System prompt** (OpenClaw-built): rules, tools, skills list, time/runtime, and injected workspace files.
- **Conversation history**: your messages + the assistant’s messages for this session.
- **Tool calls/results + attachments**: command output, file reads, images/audio, etc.

Context is _not the same thing_ as “memory”: memory can be stored on disk and reloaded later; context is what’s inside the model’s current window.

## Quick start (inspect context)

- `/status` → quick “how full is my window?” view + session settings.
- `/context list` → what’s injected + rough sizes (per file + totals).
- `/context detail` → deeper breakdown: per-file, per-tool schema sizes, per-skill entry sizes, and system prompt size.
- `/usage tokens` → append per-reply usage footer to normal replies.
- `/compact` → summarize older history into a compact entry to free window space.

See also: [Slash commands](https://docs.openclaw.ai/tools/slash-commands), [Token use & costs](https://docs.openclaw.ai/reference/token-use), [Compaction](https://docs.openclaw.ai/concepts/compaction).

## Example output

Values vary by model, provider, tool policy, and what’s in your workspace.

### `/context list`

```
🧠 Context breakdown
Workspace: <workspaceDir>
Bootstrap max/file: 12,000 chars
Sandbox: mode=non-main sandboxed=false
System prompt (run): 38,412 chars (~9,603 tok) (Project Context 23,901 chars (~5,976 tok))

Injected workspace files:
- AGENTS.md: OK | raw 1,742 chars (~436 tok) | injected 1,742 chars (~436 tok)
- SOUL.md: OK | raw 912 chars (~228 tok) | injected 912 chars (~228 tok)
- TOOLS.md: TRUNCATED | raw 54,210 chars (~13,553 tok) | injected 20,962 chars (~5,241 tok)
- IDENTITY.md: OK | raw 211 chars (~53 tok) | injected 211 chars (~53 tok)
- USER.md: OK | raw 388 chars (~97 tok) | injected 388 chars (~97 tok)
- HEARTBEAT.md: MISSING | raw 0 | injected 0
- BOOTSTRAP.md: OK | raw 0 chars (~0 tok) | injected 0 chars (~0 tok)

Skills list (system prompt text): 2,184 chars (~546 tok) (12 skills)
Tools: read, edit, write, exec, process, browser, message, sessions_send, …
Tool list (system prompt text): 1,032 chars (~258 tok)
Tool schemas (JSON): 31,988 chars (~7,997 tok) (counts toward context; not shown as text)
Tools: (same as above)

Session tokens (cached): 14,250 total / ctx=32,000
```

### `/context detail`

```
🧠 Context breakdown (detailed)
…
Top skills (prompt entry size):
- frontend-design: 412 chars (~103 tok)
- oracle: 401 chars (~101 tok)
… (+10 more skills)

Top tools (schema size):
- browser: 9,812 chars (~2,453 tok)
- exec: 6,240 chars (~1,560 tok)
… (+N more tools)
```

## What counts toward the context window

Everything the model receives counts, including:

- System prompt (all sections).
- Conversation history.
- Tool calls + tool results.
- Attachments/transcripts (images/audio/files).
- Compaction summaries and pruning artifacts.
- Provider “wrappers” or hidden headers (not visible, still counted).

## How OpenClaw builds the system prompt

The system prompt is **OpenClaw-owned** and rebuilt each run. It includes:

- Tool list + short descriptions.
- Skills list (metadata only; see below).
- Workspace location.
- Time (UTC + converted user time if configured).
- Runtime metadata (host/OS/model/thinking).
- Injected workspace bootstrap files under **Project Context**.

Full breakdown: [System Prompt](https://docs.openclaw.ai/concepts/system-prompt).

## Injected workspace files (Project Context)

By default, OpenClaw injects a fixed set of workspace files (if present):

- `AGENTS.md`
- `SOUL.md`
- `TOOLS.md`
- `IDENTITY.md`
- `USER.md`
- `HEARTBEAT.md`
- `BOOTSTRAP.md` (first-run only)

Large files are truncated per-file using `agents.defaults.bootstrapMaxChars` (default `12000` chars). OpenClaw also en

_… [truncated; see https://docs.openclaw.ai/concepts/context for full content]_


---

## Context engine - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/context-engine>_

# or inspect config directly:
cat ~/.openclaw/openclaw.json | jq '.plugins.slots.contextEngine'
```

2

[Navigate to header](https://docs.openclaw.ai/concepts/context-engine#)

Install a plugin engine

Context engine plugins are installed like any other OpenClaw plugin.

- From npm

- From a local path

```
openclaw plugins install @martian-engineering/lossless-claw
```

```
openclaw plugins install -l ./my-context-engine
```

3

[Navigate to header](https://docs.openclaw.ai/concepts/context-engine#)

Enable and select the engine

```
// openclaw.json
{
  plugins: {
    slots: {
      contextEngine: "lossless-claw", // must match the plugin's registered engine id
    },
    entries: {
      "lossless-claw": {
        enabled: true,
        // Plugin-specific config goes here (see the plugin's docs)
      },
    },
  },
}
```

Restart the gateway after installing and configuring.

4

[Navigate to header](https://docs.openclaw.ai/concepts/context-engine#)

Switch back to legacy (optional)

Set `contextEngine` to `"legacy"` (or remove the key entirely — `"legacy"` is the default).

## How it works

Every time OpenClaw runs a model prompt, the context engine participates at four lifecycle points:

1\. Ingest

Called when a new message is added to the session. The engine can store or index the message in its own data store.

2\. Assemble

Called before each model run. The engine returns an ordered set of messages (and an optional `systemPromptAddition`) that fit within the token budget.

3\. Compact

Called when the context window is full, or when the user runs `/compact`. The engine summarizes older history to free space.

4\. After turn

Called after a run completes. The engine can persist state, trigger background compaction, or update indexes.

For the bundled non-ACP Codex harness, OpenClaw applies the same lifecycle by projecting assembled context into Codex developer instructions and the current turn prompt. Codex still owns its native thread history and native compactor.

### Subagent lifecycle (optional)

OpenClaw calls two optional subagent lifecycle hooks:

[​](https://docs.openclaw.ai/concepts/context-engine#param-prepare-subagent-spawn)

prepareSubagentSpawn

method

Prepare shared context state before a child run starts. The hook receives parent/child session keys, `contextMode` (`isolated` or `fork`), available transcript ids/files, and optional TTL. If it returns a rollback handle, OpenClaw calls it when spawn fails after preparation succeeds.

[​](https://docs.openclaw.ai/concepts/context-engine#param-on-subagent-ended)

onSubagentEnded

method

Clean up when a subagent session completes or is swept.

### System prompt addition

The `assemble` method can return a `systemPromptAddition` string. OpenClaw prepends this to the system prompt for the run. This lets engines inject dynamic recall guidance, retrieval instructions, or context-aware hints without requiring static workspace files.

## The legacy engine

The built-in `legacy` engine preserves OpenClaw’s original behavior:

- **Ingest**: no-op (the session manager handles message persistence directly).
- **Assemble**: pass-through (the existing sanitize → validate → limit pipeline in the runtime handles context assembly).
- **Compact**: delegates to the built-in summarization compaction, which creates a single summary of older messages and keeps recent messages intact.
- **After turn**: no-op.

The legacy engine does not register tools or provide a `systemPromptAddition`.When no `plugins.slots.contextEngine` is set (or it’s set to `"legacy"`), this engine is used automatically.

## Plugin engines

A plugin can register a context engine using the plugin API:

```
import { buildMemorySystemPromptAddition } from "openclaw/plugin-sdk/core";

export default function register(api) {
  api.registerContextEngine("my-engine", (ctx) => ({
    info: {
      id: "my-engine",
      name: "My Context Engine",
      ownsCompaction: true,
    },

    async ingest({ sessionId, m

_… [truncated; see https://docs.openclaw.ai/concepts/context-engine for full content]_


---

## Delegate architecture - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/delegate-architecture>_

# Exchange Online PowerShell
Set-Mailbox -Identity "principal@[organization].org" `
  -GrantSendOnBehalfTo "delegate@[organization].org"
```

**Read access** (Graph API with application permissions):Register an Azure AD application with `Mail.Read` and `Calendars.Read` application permissions. **Before using the application**, scope access with an [application access policy](https://learn.microsoft.com/graph/auth-limit-mailbox-access) to restrict the app to only the delegate and principal mailboxes:

```
New-ApplicationAccessPolicy `
  -AppId "<app-client-id>" `
  -PolicyScopeGroupId "<mail-enabled-security-group>" `
  -AccessRight RestrictAccess
```

Without an application access policy, `Mail.Read` application permission grants access to **every mailbox in the tenant**. Always create the access policy before the application reads any mail. Test by confirming the app returns `403` for mailboxes outside the security group.

#### Google Workspace

Create a service account and enable domain-wide delegation in the Admin Console.Delegate only the scopes you need:

```
https://www.googleapis.com/auth/gmail.readonly    # Tier 1
https://www.googleapis.com/auth/gmail.send         # Tier 2
https://www.googleapis.com/auth/calendar           # Tier 2
```

The service account impersonates the delegate user (not the principal), preserving the “on behalf of” model.

Domain-wide delegation allows the service account to impersonate **any user in the entire domain**. Restrict the scopes to the minimum required, and limit the service account’s client ID to only the scopes listed above in the Admin Console (Security > API controls > Domain-wide delegation). A leaked service account key with broad scopes grants full access to every mailbox and calendar in the organization. Rotate keys on a schedule and monitor the Admin Console audit log for unexpected impersonation events.

### 3\. Bind the delegate to channels

Route inbound messages to the delegate agent using [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent) bindings:

```
{
  agents: {
    list: [\
      { id: "main", workspace: "~/.openclaw/workspace" },\
      {\
        id: "delegate",\
        workspace: "~/.openclaw/workspace-delegate",\
        tools: {\
          deny: ["browser", "canvas"],\
        },\
      },\
    ],
  },
  bindings: [\
    // Route a specific channel account to the delegate\
    {\
      agentId: "delegate",\
      match: { channel: "whatsapp", accountId: "org" },\
    },\
    // Route a Discord guild to the delegate\
    {\
      agentId: "delegate",\
      match: { channel: "discord", guildId: "123456789012345678" },\
    },\
    // Everything else goes to the main personal agent\
    { agentId: "main", match: { channel: "whatsapp" } },\
  ],
}
```

### 4\. Add credentials to the delegate agent

Copy or create auth profiles for the delegate’s `agentDir`:

```
# Delegate reads from its own auth store
~/.openclaw/agents/delegate/agent/auth-profiles.json
```

Never share the main agent’s `agentDir` with the delegate. See [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent) for auth isolation details.

## Example: organizational assistant

A complete delegate configuration for an organizational assistant that handles email, calendar, and social media:

```
{
  agents: {
    list: [\
      { id: "main", default: true, workspace: "~/.openclaw/workspace" },\
      {\
        id: "org-assistant",\
        name: "[Organization] Assistant",\
        workspace: "~/.openclaw/workspace-org",\
        agentDir: "~/.openclaw/agents/org-assistant/agent",\
        identity: { name: "[Organization] Assistant" },\
        tools: {\
          allow: ["read", "exec", "message", "cron", "sessions_list", "sessions_history"],\
          deny: ["write", "edit", "apply_patch", "browser", "canvas"],\
        },\
      },\
    ],
  },
  bindings: [\
    {\
      agentId: "org-assistant",\
      match: { channel: "signal", peer: { kind: "group", id: "[gro

_… [truncated; see https://docs.openclaw.ai/concepts/delegate-architecture for full content]_


---

## Dreaming - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/dreaming>_

[OpenClaw home page](https://docs.openclaw.ai/)

Memory

Dreaming

Dreaming is the background memory consolidation system in `memory-core`. It helps OpenClaw move strong short-term signals into durable memory while keeping the process explainable and reviewable.

Dreaming is **opt-in** and disabled by default.

## What dreaming writes

Dreaming keeps two kinds of output:

- **Machine state** in `memory/.dreams/` (recall store, phase signals, ingestion checkpoints, locks).
- **Human-readable output** in `DREAMS.md` (or existing `dreams.md`) and optional phase report files under `memory/dreaming/<phase>/YYYY-MM-DD.md`.

Long-term promotion still writes only to `MEMORY.md`.

## Phase model

Dreaming uses three cooperative phases:

| Phase | Purpose | Durable write |
| --- | --- | --- |
| Light | Sort and stage recent short-term material | No |
| Deep | Score and promote durable candidates | Yes (`MEMORY.md`) |
| REM | Reflect on themes and recurring ideas | No |

These phases are internal implementation details, not separate user-configured “modes.”

Light phase

Light phase ingests recent daily memory signals and recall traces, dedupes them, and stages candidate lines.

- Reads from short-term recall state, recent daily memory files, and redacted session transcripts when available.
- Writes a managed `## Light Sleep` block when storage includes inline output.
- Records reinforcement signals for later deep ranking.
- Never writes to `MEMORY.md`.

Deep phase

Deep phase decides what becomes long-term memory.

- Ranks candidates using weighted scoring and threshold gates.
- Requires `minScore`, `minRecallCount`, and `minUniqueQueries` to pass.
- Rehydrates snippets from live daily files before writing, so stale/deleted snippets are skipped.
- Appends promoted entries to `MEMORY.md`.
- Writes a `## Deep Sleep` summary into `DREAMS.md` and optionally writes `memory/dreaming/deep/YYYY-MM-DD.md`.

REM phase

REM phase extracts patterns and reflective signals.

- Builds theme and reflection summaries from recent short-term traces.
- Writes a managed `## REM Sleep` block when storage includes inline output.
- Records REM reinforcement signals used by deep ranking.
- Never writes to `MEMORY.md`.

## Session transcript ingestion

Dreaming can ingest redacted session transcripts into the dreaming corpus. When transcripts are available, they are fed into the light phase alongside daily memory signals and recall traces. Personal and sensitive content is redacted before ingestion.

## Dream Diary

Dreaming also keeps a narrative **Dream Diary** in `DREAMS.md`. After each phase has enough material, `memory-core` runs a best-effort background subagent turn and appends a short diary entry. It uses the default runtime model unless `dreaming.model` is configured. If the configured model is unavailable, Dream Diary retries once with the session default model.

This diary is for human reading in the Dreams UI, not a promotion source. Dreaming-generated diary/report artifacts are excluded from short-term promotion. Only grounded memory snippets are eligible to promote into `MEMORY.md`.

There is also a grounded historical backfill lane for review and recovery work:

Backfill commands

- `memory rem-harness --path ... --grounded` previews grounded diary output from historical `YYYY-MM-DD.md` notes.
- `memory rem-backfill --path ...` writes reversible grounded diary entries into `DREAMS.md`.
- `memory rem-backfill --path ... --stage-short-term` stages grounded durable candidates into the same short-term evidence store the normal deep phase already uses.
- `memory rem-backfill --rollback` and `--rollback-short-term` remove those staged backfill artifacts without touching ordinary diary entries or live short-term recall.

The Control UI exposes the same diary backfill/reset flow so you can inspect results in the Dreams scene before deciding whether the grounded candidates deserve promotion. The Scene also shows a distinct grounded lane so you can see whic

_… [truncated; see https://docs.openclaw.ai/concepts/dreaming for full content]_


---

## Features - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/features>_

[OpenClaw home page](https://docs.openclaw.ai/)

Overview

Features

## Highlights

[**Channels** \\
\\
Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat, and more with a single Gateway.](https://docs.openclaw.ai/channels)

[**Plugins** \\
\\
Bundled plugins add Matrix, Nextcloud Talk, Nostr, Twitch, Zalo, and more without separate installs in normal current releases.](https://docs.openclaw.ai/tools/plugin)

[**Routing** \\
\\
Multi-agent routing with isolated sessions.](https://docs.openclaw.ai/concepts/multi-agent)

[**Media** \\
\\
Images, audio, video, documents, and image/video generation.](https://docs.openclaw.ai/nodes/images)

[**Apps and UI** \\
\\
Web Control UI and macOS companion app.](https://docs.openclaw.ai/web/control-ui)

[**Mobile nodes** \\
\\
iOS and Android nodes with pairing, voice/chat, and rich device commands.](https://docs.openclaw.ai/nodes)

## Full list

**Channels:**

- Built-in channels include Discord, Google Chat, iMessage (legacy), IRC, Signal, Slack, Telegram, WebChat, and WhatsApp
- Bundled plugin channels include BlueBubbles for iMessage, Feishu, LINE, Matrix, Mattermost, Microsoft Teams, Nextcloud Talk, Nostr, QQ Bot, Synology Chat, Tlon, Twitch, Zalo, and Zalo Personal
- Optional separately installed channel plugins include Voice Call and third-party packages such as WeChat
- Third-party channel plugins can extend the Gateway further, such as WeChat
- Group chat support with mention-based activation
- DM safety with allowlists and pairing

**Agent:**

- Embedded agent runtime with tool streaming
- Multi-agent routing with isolated sessions per workspace or sender
- Sessions: direct chats collapse into shared `main`; groups are isolated
- Streaming and chunking for long responses

**Auth and providers:**

- 35+ model providers (Anthropic, OpenAI, Google, and more)
- Subscription auth via OAuth (e.g. OpenAI Codex)
- Custom and self-hosted provider support (vLLM, SGLang, Ollama, and any OpenAI-compatible or Anthropic-compatible endpoint)

**Media:**

- Images, audio, video, and documents in and out
- Shared image generation and video generation capability surfaces
- Voice note transcription
- Text-to-speech with multiple providers

**Apps and interfaces:**

- WebChat and browser Control UI
- macOS menu bar companion app
- iOS node with pairing, Canvas, camera, screen recording, location, and voice
- Android node with pairing, chat, voice, Canvas, camera, and device commands

**Tools and automation:**

- Browser automation, exec, sandboxing
- Web search (Brave, DuckDuckGo, Exa, Firecrawl, Gemini, Grok, Kimi, MiniMax Search, Ollama Web Search, Perplexity, SearXNG, Tavily)
- Cron jobs and heartbeat scheduling
- Skills, plugins, and workflow pipelines (Lobster)

## Related

- [Experimental features](https://docs.openclaw.ai/concepts/experimental-features)
- [Agent runtime](https://docs.openclaw.ai/concepts/agent)

[Showcase](https://docs.openclaw.ai/start/showcase) [Getting started](https://docs.openclaw.ai/start/getting-started)

Ctrl+I


---

## https://docs.openclaw.ai/concepts/features.md

_Source: <https://docs.openclaw.ai/concepts/features.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Features

\## Highlights

 Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat, and more with a single Gateway.

 Bundled plugins add Matrix, Nextcloud Talk, Nostr, Twitch, Zalo, and more without separate installs in normal current releases.

 Multi-agent routing with isolated sessions.

 Images, audio, video, documents, and image/video generation.

 Web Control UI and macOS companion app.

 iOS and Android nodes with pairing, voice/chat, and rich device commands.

\## Full list

\*\*Channels:\*\*

\\* Built-in channels include Discord, Google Chat, iMessage (legacy), IRC, Signal, Slack, Telegram, WebChat, and WhatsApp
\\* Bundled plugin channels include BlueBubbles for iMessage, Feishu, LINE, Matrix, Mattermost, Microsoft Teams, Nextcloud Talk, Nostr, QQ Bot, Synology Chat, Tlon, Twitch, Zalo, and Zalo Personal
\\* Optional separately installed channel plugins include Voice Call and third-party packages such as WeChat
\\* Third-party channel plugins can extend the Gateway further, such as WeChat
\\* Group chat support with mention-based activation
\\* DM safety with allowlists and pairing

\*\*Agent:\*\*

\\* Embedded agent runtime with tool streaming
\\* Multi-agent routing with isolated sessions per workspace or sender
\\* Sessions: direct chats collapse into shared \`main\`; groups are isolated
\\* Streaming and chunking for long responses

\*\*Auth and providers:\*\*

\\* 35+ model providers (Anthropic, OpenAI, Google, and more)
\\* Subscription auth via OAuth (e.g. OpenAI Codex)
\\* Custom and self-hosted provider support (vLLM, SGLang, Ollama, and any OpenAI-compatible or Anthropic-compatible endpoint)

\*\*Media:\*\*

\\* Images, audio, video, and documents in and out
\\* Shared image generation and video generation capability surfaces
\\* Voice note transcription
\\* Text-to-speech with multiple providers

\*\*Apps and interfaces:\*\*

\\* WebChat and browser Control UI
\\* macOS menu bar companion app
\\* iOS node with pairing, Canvas, camera, screen recording, location, and voice
\\* Android node with pairing, chat, voice, Canvas, camera, and device commands

\*\*Tools and automation:\*\*

\\* Browser automation, exec, sandboxing
\\* Web search (Brave, DuckDuckGo, Exa, Firecrawl, Gemini, Grok, Kimi, MiniMax Search, Ollama Web Search, Perplexity, SearXNG, Tavily)
\\* Cron jobs and heartbeat scheduling
\\* Skills, plugins, and workflow pipelines (Lobster)

\## Related

\\* \[Experimental features\](/concepts/experimental-features)
\\* \[Agent runtime\](/concepts/agent)


---

## Memory overview - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/memory>_

[OpenClaw home page](https://docs.openclaw.ai/)

⌘K

Memory

Memory overview

OpenClaw remembers things by writing **plain Markdown files** in your agent’s
workspace. The model only “remembers” what gets saved to disk — there is no
hidden state.

## How it works

Your agent has three memory-related files:

- **`MEMORY.md`** — long-term memory. Durable facts, preferences, and
decisions. Loaded at the start of every DM session.
- **`memory/YYYY-MM-DD.md`** — daily notes. Running context and observations.
Today and yesterday’s notes are loaded automatically.
- **`DREAMS.md`** (optional) — Dream Diary and dreaming sweep
summaries for human review, including grounded historical backfill entries.

These files live in the agent workspace (default `~/.openclaw/workspace`).

If you want your agent to remember something, just ask it: “Remember that I
prefer TypeScript.” It will write it to the appropriate file.

## Inferred commitments

Some future follow-ups are not durable facts. If you mention an interview
tomorrow, the useful memory may be “check in after the interview,” not “store
this forever in `MEMORY.md`.”[Commitments](https://docs.openclaw.ai/concepts/commitments) are opt-in, short-lived follow-up memories
for that case. OpenClaw infers them in a hidden background pass, scopes them to
the same agent and channel, and delivers due check-ins through heartbeat.
Explicit reminders still use [scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs).

## Memory tools

The agent has two tools for working with memory:

- **`memory_search`** — finds relevant notes using semantic search, even when
the wording differs from the original.
- **`memory_get`** — reads a specific memory file or line range.

Both tools are provided by the active memory plugin (default: `memory-core`).

## Memory Wiki companion plugin

If you want durable memory to behave more like a maintained knowledge base than
just raw notes, use the bundled `memory-wiki` plugin.`memory-wiki` compiles durable knowledge into a wiki vault with:

- deterministic page structure
- structured claims and evidence
- contradiction and freshness tracking
- generated dashboards
- compiled digests for agent/runtime consumers
- wiki-native tools like `wiki_search`, `wiki_get`, `wiki_apply`, and `wiki_lint`

It does not replace the active memory plugin. The active memory plugin still
owns recall, promotion, and dreaming. `memory-wiki` adds a provenance-rich
knowledge layer beside it.See [Memory Wiki](https://docs.openclaw.ai/plugins/memory-wiki).

## Memory search

When an embedding provider is configured, `memory_search` uses **hybrid**
**search** — combining vector similarity (semantic meaning) with keyword matching
(exact terms like IDs and code symbols). This works out of the box once you have
an API key for any supported provider.

OpenClaw auto-detects your embedding provider from available API keys. If you
have an OpenAI, Gemini, Voyage, or Mistral key configured, memory search is
enabled automatically.

For details on how search works, tuning options, and provider setup, see
[Memory Search](https://docs.openclaw.ai/concepts/memory-search).

## Memory backends

## Builtin (default)

SQLite-based. Works out of the box with keyword search, vector similarity, and
hybrid search. No extra dependencies.

## QMD

Local-first sidecar with reranking, query expansion, and the ability to index
directories outside the workspace.

## Honcho

AI-native cross-session memory with user modeling, semantic search, and
multi-agent awareness. Plugin install.

## LanceDB

Bundled LanceDB-backed memory with OpenAI-compatible embeddings, auto-recall,
auto-capture, and local Ollama embedding support.

## Knowledge wiki layer

## Memory Wiki

Compiles durable memory into a provenance-rich wiki vault with claims,
dashboards, bridge mode, and Obsidian-friendly workflows.

## Automatic memory flush

Before [compaction](https://docs.openclaw.ai/concepts/compaction) summarizes your conversation, OpenClaw
r

_… [truncated; see https://docs.openclaw.ai/concepts/memory for full content]_


---

## Builtin memory engine - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/memory-builtin>_

[OpenClaw home page](https://docs.openclaw.ai/)

Memory

Builtin memory engine

The builtin engine is the default memory backend. It stores your memory index in
a per-agent SQLite database and needs no extra dependencies to get started.

## What it provides

- **Keyword search** via FTS5 full-text indexing (BM25 scoring).
- **Vector search** via embeddings from any supported provider.
- **Hybrid search** that combines both for best results.
- **CJK support** via trigram tokenization for Chinese, Japanese, and Korean.
- **sqlite-vec acceleration** for in-database vector queries (optional).

## Getting started

If you have an API key for OpenAI, Gemini, Voyage, Mistral, or DeepInfra, the builtin
engine auto-detects it and enables vector search. No config needed.To set a provider explicitly:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
      },
    },
  },
}
```

Without an embedding provider, only keyword search is available.To force the built-in local embedding provider, install the optional
`node-llama-cpp` runtime package next to OpenClaw, then point `local.modelPath`
at a GGUF file:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "local",
        fallback: "none",
        local: {
          modelPath: "~/.node-llama-cpp/models/embeddinggemma-300m-qat-Q8_0.gguf",
        },
      },
    },
  },
}
```

## Supported embedding providers

| Provider | ID | Auto-detected | Notes |
| --- | --- | --- | --- |
| OpenAI | `openai` | Yes | Default: `text-embedding-3-small` |
| Gemini | `gemini` | Yes | Supports multimodal (image + audio) |
| Voyage | `voyage` | Yes |  |
| Mistral | `mistral` | Yes |  |
| DeepInfra | `deepinfra` | Yes | Default: `BAAI/bge-m3` |
| Ollama | `ollama` | No | Local, set explicitly |
| Local | `local` | Yes (first) | Optional `node-llama-cpp` runtime |

Auto-detection picks the first provider whose API key can be resolved, in the
order shown. Set `memorySearch.provider` to override.

## How indexing works

OpenClaw indexes `MEMORY.md` and `memory/*.md` into chunks (~400 tokens with
80-token overlap) and stores them in a per-agent SQLite database.

- **Index location:**`~/.openclaw/memory/<agentId>.sqlite`
- **Storage maintenance:** SQLite WAL sidecars are bounded with periodic and
shutdown checkpoints.
- **File watching:** changes to memory files trigger a debounced reindex (1.5s).
- **Auto-reindex:** when the embedding provider, model, or chunking config
changes, the entire index is rebuilt automatically.
- **Reindex on demand:**`openclaw memory index --force`

You can also index Markdown files outside the workspace with
`memorySearch.extraPaths`. See the
[configuration reference](https://docs.openclaw.ai/reference/memory-config#additional-memory-paths).

## When to use

The builtin engine is the right choice for most users:

- Works out of the box with no extra dependencies.
- Handles keyword and vector search well.
- Supports all embedding providers.
- Hybrid search combines the best of both retrieval approaches.

Consider switching to [QMD](https://docs.openclaw.ai/concepts/memory-qmd) if you need reranking, query
expansion, or want to index directories outside the workspace.Consider [Honcho](https://docs.openclaw.ai/concepts/memory-honcho) if you want cross-session memory with
automatic user modeling.

## Troubleshooting

**Memory search disabled?** Check `openclaw memory status`. If no provider is
detected, set one explicitly or add an API key.**Local provider not detected?** Confirm the local path exists and run:

```
openclaw memory status --deep --agent main
openclaw memory index --force --agent main
```

Both standalone CLI commands and the Gateway use the same `local` provider id.
If the provider is set to `auto`, local embeddings are considered first only
when `memorySearch.local.modelPath` points to an existing local file.**Stale results?** Run `openclaw memory index --force` to rebuild. The watcher
may miss changes in rare edge ca

_… [truncated; see https://docs.openclaw.ai/concepts/memory-builtin for full content]_


---

## Honcho memory - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/memory-honcho>_

[OpenClaw home page](https://docs.openclaw.ai/)

Memory

Honcho memory

[Honcho](https://honcho.dev/) adds AI-native memory to OpenClaw. It persists
conversations to a dedicated service and builds user and agent models over time,
giving your agent cross-session context that goes beyond workspace Markdown
files.

## What it provides

- **Cross-session memory** — conversations are persisted after every turn, so
context carries across session resets, compaction, and channel switches.
- **User modeling** — Honcho maintains a profile for each user (preferences,
facts, communication style) and for the agent (personality, learned
behaviors).
- **Semantic search** — search over observations from past conversations, not
just the current session.
- **Multi-agent awareness** — parent agents automatically track spawned
sub-agents, with parents added as observers in child sessions.

## Available tools

Honcho registers tools that the agent can use during conversation:**Data retrieval (fast, no LLM call):**

| Tool | What it does |
| --- | --- |
| `honcho_context` | Full user representation across sessions |
| `honcho_search_conclusions` | Semantic search over stored conclusions |
| `honcho_search_messages` | Find messages across sessions (filter by sender, date) |
| `honcho_session` | Current session history and summary |

**Q&A (LLM-powered):**

| Tool | What it does |
| --- | --- |
| `honcho_ask` | Ask about the user. `depth='quick'` for facts, `'thorough'` for synthesis |

## Getting started

Install the plugin and run setup:

```
openclaw plugins install @honcho-ai/openclaw-honcho
openclaw honcho setup
openclaw gateway --force
```

The setup command prompts for your API credentials, writes the config, and
optionally migrates existing workspace memory files.

Honcho can run entirely locally (self-hosted) or via the managed API at
`api.honcho.dev`. No external dependencies are required for the self-hosted
option.

## Configuration

Settings live under `plugins.entries["openclaw-honcho"].config`:

```
{
  plugins: {
    entries: {
      "openclaw-honcho": {
        config: {
          apiKey: "your-api-key", // omit for self-hosted
          workspaceId: "openclaw", // memory isolation
          baseUrl: "https://api.honcho.dev",
        },
      },
    },
  },
}
```

For self-hosted instances, point `baseUrl` to your local server (for example
`http://localhost:8000`) and omit the API key.

## Migrating existing memory

If you have existing workspace memory files (`USER.md`, `MEMORY.md`,
`IDENTITY.md`, `memory/`, `canvas/`), `openclaw honcho setup` detects and
offers to migrate them.

Migration is non-destructive — files are uploaded to Honcho. Originals are
never deleted or moved.

## How it works

After every AI turn, the conversation is persisted to Honcho. Both user and
agent messages are observed, allowing Honcho to build and refine its models over
time.During conversation, Honcho tools query the service in the `before_prompt_build`
phase, injecting relevant context before the model sees the prompt. This ensures
accurate turn boundaries and relevant recall.

## Honcho vs builtin memory

|  | Builtin / QMD | Honcho |
| --- | --- | --- |
| **Storage** | Workspace Markdown files | Dedicated service (local or hosted) |
| **Cross-session** | Via memory files | Automatic, built-in |
| **User modeling** | Manual (write to MEMORY.md) | Automatic profiles |
| **Search** | Vector + keyword (hybrid) | Semantic over observations |
| **Multi-agent** | Not tracked | Parent/child awareness |
| **Dependencies** | None (builtin) or QMD binary | Plugin install |

Honcho and the builtin memory system can work together. When QMD is configured,
additional tools become available for searching local Markdown files alongside
Honcho’s cross-session memory.

## CLI commands

```
openclaw honcho setup                        # Configure API key and migrate files
openclaw honcho status                       # Check connection status
openclaw honcho ask <question

_… [truncated; see https://docs.openclaw.ai/concepts/memory-honcho for full content]_


---

## QMD memory engine - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/memory-qmd>_

[OpenClaw home page](https://docs.openclaw.ai/)

Memory

QMD memory engine

[QMD](https://github.com/tobi/qmd) is a local-first search sidecar that runs
alongside OpenClaw. It combines BM25, vector search, and reranking in a single
binary, and can index content beyond your workspace memory files.

## What it adds over builtin

- **Reranking and query expansion** for better recall.
- **Index extra directories** — project docs, team notes, anything on disk.
- **Index session transcripts** — recall earlier conversations.
- **Fully local** — runs with the optional node-llama-cpp runtime package and
auto-downloads GGUF models.
- **Automatic fallback** — if QMD is unavailable, OpenClaw falls back to the
builtin engine seamlessly.

## Getting started

### Prerequisites

- Install QMD: `npm install -g @tobilu/qmd` or `bun install -g @tobilu/qmd`
- SQLite build that allows extensions (`brew install sqlite` on macOS).
- QMD must be on the gateway’s `PATH`.
- macOS and Linux work out of the box. Windows is best supported via WSL2.

### Enable

```
{
  memory: {
    backend: "qmd",
  },
}
```

OpenClaw creates a self-contained QMD home under
`~/.openclaw/agents/<agentId>/qmd/` and manages the sidecar lifecycle
automatically — collections, updates, and embedding runs are handled for you.
It prefers current QMD collection and MCP query shapes, but still falls back to
alternate collection pattern flags and older MCP tool names when needed.
Boot-time reconciliation also recreates stale managed collections back to their
canonical patterns when an older QMD collection with the same name is still
present.

## How the sidecar works

- OpenClaw creates collections from your workspace memory files and any
configured `memory.qmd.paths`, then runs `qmd update` when the QMD manager is
opened and periodically afterward (default every 5 minutes). These refreshes
run through QMD subprocesses, not an in-process filesystem crawl. Semantic
modes also run `qmd embed`.
- The default workspace collection tracks `MEMORY.md` plus the `memory/`
tree. Lowercase `memory.md` is not indexed as a root memory file.
- QMD’s own scanner ignores hidden paths and common dependency/build
directories such as `.git`, `.cache`, `node_modules`, `vendor`, `dist`, and
`build`. Gateway startup does not initialize QMD by default, so cold boot
avoids importing the memory runtime or creating the long-lived watcher before
memory is first used.
- If you want a gateway-start refresh anyway, set
`memory.qmd.update.startup` to `idle` or `immediate`. The opt-in startup
refresh uses a one-shot QMD subprocess path instead of creating the full
long-lived in-process watcher.
- Searches use the configured `searchMode` (default: `search`; also supports
`vsearch` and `query`). `search` is BM25-only, so OpenClaw skips semantic
vector readiness probes and embedding maintenance in that mode. If a mode
fails, OpenClaw retries with `qmd query`.
- With QMD releases that advertise multi-collection filters, OpenClaw groups
same-source collections into one QMD search invocation. Older QMD releases
keep the compatible per-collection fallback.
- If QMD fails entirely, OpenClaw falls back to the builtin SQLite engine.
Repeated chat-turn attempts back off briefly after an open failure so a
missing binary or broken sidecar dependency does not create a retry storm;
`openclaw memory status` and one-shot CLI probes still recheck QMD directly.

The first search may be slow — QMD auto-downloads GGUF models (~2 GB) for
reranking and query expansion on the first `qmd query` run.

## Search performance and compatibility

OpenClaw keeps the QMD search path compatible with both current and older QMD
installs.On startup, OpenClaw checks the installed QMD help text once per manager. If the
binary advertises support for multiple collection filters, OpenClaw searches all
same-source collections with one command:

```
qmd search "router notes" --json -n 10 -c memory-root-main -c memory-dir-main
```

This avoids starting one

_… [truncated; see https://docs.openclaw.ai/concepts/memory-qmd for full content]_


---

## Memory search - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/memory-search>_

[OpenClaw home page](https://docs.openclaw.ai/)

Memory

Memory search

`memory_search` finds relevant notes from your memory files, even when the
wording differs from the original text. It works by indexing memory into small
chunks and searching them using embeddings, keywords, or both.

## Quick start

If you have a GitHub Copilot subscription, OpenAI, Gemini, Voyage, or Mistral
API key configured, memory search works automatically. To set a provider
explicitly:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai", // or "gemini", "local", "ollama", etc.
      },
    },
  },
}
```

For multi-endpoint setups, `provider` can also be a custom
`models.providers.<id>` entry, such as `ollama-5080`, when that provider sets
`api: "ollama"` or another embedding adapter owner.For local embeddings with no API key, set `provider: "local"`. Source checkouts
may still require native build approval: `pnpm approve-builds` then
`pnpm rebuild node-llama-cpp`.Some OpenAI-compatible embedding endpoints require asymmetric labels such as
`input_type: "query"` for searches and `input_type: "document"` or `"passage"`
for indexed chunks. Configure those with `memorySearch.queryInputType` and
`memorySearch.documentInputType`; see the [Memory configuration reference](https://docs.openclaw.ai/reference/memory-config#provider-specific-config).

## Supported providers

| Provider | ID | Needs API key | Notes |
| --- | --- | --- | --- |
| Bedrock | `bedrock` | No | Auto-detected when the AWS credential chain resolves |
| Gemini | `gemini` | Yes | Supports image/audio indexing |
| GitHub Copilot | `github-copilot` | No | Auto-detected, uses Copilot subscription |
| Local | `local` | No | GGUF model, ~0.6 GB download |
| Mistral | `mistral` | Yes | Auto-detected |
| Ollama | `ollama` | No | Local, must set explicitly |
| OpenAI | `openai` | Yes | Auto-detected, fast |
| Voyage | `voyage` | Yes | Auto-detected |

## How search works

OpenClaw runs two retrieval paths in parallel and merges the results:

Query

Embedding

Tokenize

Vector Search

BM25 Search

Weighted Merge

Top Results

- **Vector search** finds notes with similar meaning (“gateway host” matches
“the machine running OpenClaw”).
- **BM25 keyword search** finds exact matches (IDs, error strings, config
keys).

If only one path is available (no embeddings or no FTS), the other runs alone.When embeddings are unavailable, OpenClaw still uses lexical ranking over FTS results instead of falling back to raw exact-match ordering only. That degraded mode boosts chunks with stronger query-term coverage and relevant file paths, which keeps recall useful even without `sqlite-vec` or an embedding provider.

## Improving search quality

Two optional features help when you have a large note history:

### Temporal decay

Old notes gradually lose ranking weight so recent information surfaces first.
With the default half-life of 30 days, a note from last month scores at 50% of
its original weight. Evergreen files like `MEMORY.md` are never decayed.

Enable temporal decay if your agent has months of daily notes and stale
information keeps outranking recent context.

### MMR (diversity)

Reduces redundant results. If five notes all mention the same router config, MMR
ensures the top results cover different topics instead of repeating.

Enable MMR if `memory_search` keeps returning near-duplicate snippets from
different daily notes.

### Enable both

```
{
  agents: {
    defaults: {
      memorySearch: {
        query: {
          hybrid: {
            mmr: { enabled: true },
            temporalDecay: { enabled: true },
          },
        },
      },
    },
  },
}
```

## Multimodal memory

With Gemini Embedding 2, you can index images and audio files alongside
Markdown. Search queries remain text, but they match against visual and audio
content. See the [Memory configuration reference](https://docs.openclaw.ai/reference/memory-config) for
setup.

## Session memory search

You c

_… [truncated; see https://docs.openclaw.ai/concepts/memory-search for full content]_


---

## https://docs.openclaw.ai/concepts/memory.md

_Source: <https://docs.openclaw.ai/concepts/memory.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Memory overview

OpenClaw remembers things by writing \*\*plain Markdown files\*\* in your agent's
workspace. The model only "remembers" what gets saved to disk — there is no
hidden state.

\## How it works

Your agent has three memory-related files:

\\* \*\*\`MEMORY.md\`\*\* — long-term memory. Durable facts, preferences, and
 decisions. Loaded at the start of every DM session.
\\* \*\*\`memory/YYYY-MM-DD.md\`\*\* — daily notes. Running context and observations.
 Today and yesterday's notes are loaded automatically.
\\* \*\*\`DREAMS.md\`\*\* (optional) — Dream Diary and dreaming sweep
 summaries for human review, including grounded historical backfill entries.

These files live in the agent workspace (default \`~/.openclaw/workspace\`).

 If you want your agent to remember something, just ask it: "Remember that I
 prefer TypeScript." It will write it to the appropriate file.

\## Inferred commitments

Some future follow-ups are not durable facts. If you mention an interview
tomorrow, the useful memory may be "check in after the interview," not "store
this forever in \`MEMORY.md\`."

\[Commitments\](/concepts/commitments) are opt-in, short-lived follow-up memories
for that case. OpenClaw infers them in a hidden background pass, scopes them to
the same agent and channel, and delivers due check-ins through heartbeat.
Explicit reminders still use \[scheduled tasks\](/automation/cron-jobs).

\## Memory tools

The agent has two tools for working with memory:

\\* \*\*\`memory\_search\`\*\* — finds relevant notes using semantic search, even when
 the wording differs from the original.
\\* \*\*\`memory\_get\`\*\* — reads a specific memory file or line range.

Both tools are provided by the active memory plugin (default: \`memory-core\`).

\## Memory Wiki companion plugin

If you want durable memory to behave more like a maintained knowledge base than
just raw notes, use the bundled \`memory-wiki\` plugin.

\`memory-wiki\` compiles durable knowledge into a wiki vault with:

\\* deterministic page structure
\\* structured claims and evidence
\\* contradiction and freshness tracking
\\* generated dashboards
\\* compiled digests for agent/runtime consumers
\\* wiki-native tools like \`wiki\_search\`, \`wiki\_get\`, \`wiki\_apply\`, and \`wiki\_lint\`

It does not replace the active memory plugin. The active memory plugin still
owns recall, promotion, and dreaming. \`memory-wiki\` adds a provenance-rich
knowledge layer beside it.

See \[Memory Wiki\](/plugins/memory-wiki).

\## Memory search

When an embedding provider is configured, \`memory\_search\` uses \*\*hybrid
search\*\* — combining vector similarity (semantic meaning) with keyword matching
(exact terms like IDs and code symbols). This works out of the box once you have
an API key for any supported provider.

 OpenClaw auto-detects your embedding provider from available API keys. If you
 have an OpenAI, Gemini, Voyage, or Mistral key configured, memory search is
 enabled automatically.

For details on how search works, tuning options, and provider setup, see
\[Memory Search\](/concepts/memory-search).

\## Memory backends

 SQLite-based. Works out of the box with keyword search, vector similarity, and
 hybrid search. No extra dependencies.

 Local-first sidecar with reranking, query expansion, and the ability to index
 directories outside the workspace.

 AI-native cross-session memory with user modeling, semantic search, and
 multi-agent awareness. Plugin install.

 Bundled LanceDB-backed memory with OpenAI-compatible embeddings, auto-recall,
 auto-capture, and local Ollama embedding support.

\## Knowledge wiki layer

 Compiles durable memory into a provenance-rich wiki vault with claims,
 dashboards, bridge mode, and Obsidian-friendly workflows.

\## Automatic memory flush

Before \[compaction\](/concepts/compaction) s

_… [truncated; see https://docs.openclaw.ai/concepts/memory.md for full content]_


---

## Messages - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/messages>_

[OpenClaw home page](https://docs.openclaw.ai/)

Messages and delivery

Messages

OpenClaw handles inbound messages through a pipeline of session resolution, queueing, streaming, tool execution, and reasoning visibility. This page maps the path from inbound message to reply.

## Message flow (high level)

```
Inbound message
  -> routing/bindings -> session key
  -> queue (if a run is active)
  -> agent run (streaming + tools)
  -> outbound replies (channel limits + chunking)
```

Key knobs live in configuration:

- `messages.*` for prefixes, queueing, and group behavior.
- `agents.defaults.*` for block streaming and chunking defaults.
- Channel overrides (`channels.whatsapp.*`, `channels.telegram.*`, etc.) for caps and streaming toggles.

See [Configuration](https://docs.openclaw.ai/gateway/configuration) for full schema.

## Inbound dedupe

Channels can redeliver the same message after reconnects. OpenClaw keeps a
short-lived cache keyed by channel/account/peer/session/message id so duplicate
deliveries do not trigger another agent run.

## Inbound debouncing

Rapid consecutive messages from the **same sender** can be batched into a single
agent turn via `messages.inbound`. Debouncing is scoped per channel + conversation
and uses the most recent message for reply threading/IDs.Config (global default + per-channel overrides):

```
{
  messages: {
    inbound: {
      debounceMs: 2000,
      byChannel: {
        whatsapp: 5000,
        slack: 1500,
        discord: 1500,
      },
    },
  },
}
```

Notes:

- Debounce applies to **text-only** messages; media/attachments flush immediately.
- Control commands bypass debouncing so they remain standalone — **except** when a channel explicitly opts in to same-sender DM coalescing (e.g. [BlueBubbles `coalesceSameSenderDms`](https://docs.openclaw.ai/channels/bluebubbles#coalescing-split-send-dms-command--url-in-one-composition)), where DM commands wait inside the debounce window so a split-send payload can join the same agent turn.

## Sessions and devices

Sessions are owned by the gateway, not by clients.

- Direct chats collapse into the agent main session key.
- Groups/channels get their own session keys.
- The session store and transcripts live on the gateway host.

Multiple devices/channels can map to the same session, but history is not fully
synced back to every client. Recommendation: use one primary device for long
conversations to avoid divergent context. The Control UI and TUI always show the
gateway-backed session transcript, so they are the source of truth.Details: [Session management](https://docs.openclaw.ai/concepts/session).

## Tool result metadata

Tool result `content` is the model-visible result. Tool result `details` is
runtime metadata for UI rendering, diagnostics, media delivery, and plugins.OpenClaw keeps that boundary explicit:

- `toolResult.details` is stripped before provider replay and compaction input.
- Persisted session transcripts keep only bounded `details`; oversized metadata
is replaced with a compact summary marked `persistedDetailsTruncated: true`.
- Plugins and tools should put text the model must read in `content`, not only
in `details`.

## Inbound bodies and history context

OpenClaw separates the **prompt body** from the **command body**:

- `BodyForAgent`: primary model-facing text for the current message. Channel
plugins should keep this focused on the sender’s current prompt-bearing text.
- `Body`: legacy prompt fallback. This may include channel envelopes and
optional history wrappers, but current channels should not rely on it as the
primary model input when `BodyForAgent` is available.
- `CommandBody`: raw user text for directive/command parsing.
- `RawBody`: legacy alias for `CommandBody` (kept for compatibility).

When a channel supplies history, it uses a shared wrapper:

- `[Chat messages since your last reply - for context]`
- `[Current message - respond to this]`

For **non-direct chats** (groups/channels/rooms), the **current

_… [truncated; see https://docs.openclaw.ai/concepts/messages for full content]_


---

## Model failover - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/model-failover>_

[OpenClaw home page](https://docs.openclaw.ai/)

Concepts and configuration

Model failover

OpenClaw handles failures in two stages:

1. **Auth profile rotation** within the current provider.
2. **Model fallback** to the next model in `agents.defaults.model.fallbacks`.

This doc explains the runtime rules and the data that backs them.

## Runtime flow

For a normal text run, OpenClaw evaluates candidates in this order:

1

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Resolve session state

Resolve the active session model and auth-profile preference.

2

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Build candidate chain

Build the model candidate chain from the current model selection and the fallback policy for that selection source. Configured defaults, cron job primaries, and auto-selected fallback models can use configured fallbacks; explicit user session selections are strict.

3

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Try the current provider

Try the current provider with auth-profile rotation/cooldown rules.

4

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Advance on failover-worthy errors

If that provider is exhausted with a failover-worthy error, move to the next model candidate.

5

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Persist fallback override

Persist the selected fallback override before the retry starts so other session readers see the same provider/model the runner is about to use. The persisted model override is marked `modelOverrideSource: "auto"`.

6

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Roll back narrowly on failure

If the fallback candidate fails, roll back only the fallback-owned session override fields when they still match that failed candidate.

7

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Throw FallbackSummaryError if exhausted

If every candidate fails, throw a `FallbackSummaryError` with per-attempt detail and the soonest cooldown expiry when one is known.

This is intentionally narrower than “save and restore the whole session”. The reply runner only persists the model-selection fields it owns for fallback:

- `providerOverride`
- `modelOverride`
- `modelOverrideSource`
- `authProfileOverride`
- `authProfileOverrideSource`
- `authProfileOverrideCompactionCount`

That prevents a failed fallback retry from overwriting newer unrelated session mutations such as manual `/model` changes or session rotation updates that happened while the attempt was running.

## Selection source policy

OpenClaw separates the selected provider/model from why it was selected. That source controls whether the fallback chain is allowed:

- **Configured default**: `agents.defaults.model.primary` uses `agents.defaults.model.fallbacks`.
- **Agent primary**: `agents.list[].model` is strict unless that agent model object includes its own `fallbacks`. Use `fallbacks: []` to make the strict behavior explicit, or provide a non-empty list to opt that agent into model fallback.
- **Auto fallback override**: a runtime fallback writes `providerOverride`, `modelOverride`, and `modelOverrideSource: "auto"` before retrying. That auto override can keep walking the configured fallback chain and is cleared by `/new`, `/reset`, and `sessions.reset`.
- **User session override**: `/model`, the model picker, `session_status(model=...)`, and `sessions.patch` write `modelOverrideSource: "user"`. That is an exact session selection. If the selected provider/model fails before producing a reply, OpenClaw reports the failure instead of answering from an unrelated configured fallback.
- **Legacy session override**: older session entries may have `modelOverride` without `modelOverrideSource`. OpenClaw treats those as user overrides so an explicit old selection is not silently converted into fallback behavior.
- **Cron payload model**

_… [truncated; see https://docs.openclaw.ai/concepts/model-failover for full content]_


---

## Model providers - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/model-providers>_

# Install Ollama, then pull a model:
ollama pull llama3.3
```

```
{
  agents: {
    defaults: { model: { primary: "ollama/llama3.3" } },
  },
}
```

Ollama is detected locally at `http://127.0.0.1:11434` when you opt in with `OLLAMA_API_KEY`, and the bundled provider plugin adds Ollama directly to `openclaw onboard` and the model picker. See [/providers/ollama](https://docs.openclaw.ai/providers/ollama) for onboarding, cloud/local mode, and custom configuration.

### vLLM

vLLM ships as a bundled provider plugin for local/self-hosted OpenAI-compatible servers:

- Provider: `vllm`
- Auth: Optional (depends on your server)
- Default base URL: `http://127.0.0.1:8000/v1`

To opt in to auto-discovery locally (any value works if your server doesn’t enforce auth):

```
export VLLM_API_KEY="vllm-local"
```

Then set a model (replace with one of the IDs returned by `/v1/models`):

```
{
  agents: {
    defaults: { model: { primary: "vllm/your-model-id" } },
  },
}
```

See [/providers/vllm](https://docs.openclaw.ai/providers/vllm) for details.

### SGLang

SGLang ships as a bundled provider plugin for fast self-hosted OpenAI-compatible servers:

- Provider: `sglang`
- Auth: Optional (depends on your server)
- Default base URL: `http://127.0.0.1:30000/v1`

To opt in to auto-discovery locally (any value works if your server does not enforce auth):

```
export SGLANG_API_KEY="sglang-local"
```

Then set a model (replace with one of the IDs returned by `/v1/models`):

```
{
  agents: {
    defaults: { model: { primary: "sglang/your-model-id" } },
  },
}
```

See [/providers/sglang](https://docs.openclaw.ai/providers/sglang) for details.

### Local proxies (LM Studio, vLLM, LiteLLM, etc.)

Example (OpenAI‑compatible):

```
{
  agents: {
    defaults: {
      model: { primary: "lmstudio/my-local-model" },
      models: { "lmstudio/my-local-model": { alias: "Local" } },
    },
  },
  models: {
    providers: {
      lmstudio: {
        baseUrl: "http://localhost:1234/v1",
        apiKey: "${LM_API_TOKEN}",
        api: "openai-completions",
        timeoutSeconds: 300,
        models: [\
          {\
            id: "my-local-model",\
            name: "Local Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 200000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

Default optional fields

For custom providers, `reasoning`, `input`, `cost`, `contextWindow`, and `maxTokens` are optional. When omitted, OpenClaw defaults to:

- `reasoning: false`
- `input: ["text"]`
- `cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 }`
- `contextWindow: 200000`
- `maxTokens: 8192`

Recommended: set explicit values that match your proxy/model limits.

Proxy-route shaping rules

- For `api: "openai-completions"` on non-native endpoints (any non-empty `baseUrl` whose host is not `api.openai.com`), OpenClaw forces `compat.supportsDeveloperRole: false` to avoid provider 400 errors for unsupported `developer` roles.
- Proxy-style OpenAI-compatible routes also skip native OpenAI-only request shaping: no `service_tier`, no Responses `store`, no Completions `store`, no prompt-cache hints, no OpenAI reasoning-compat payload shaping, and no hidden OpenClaw attribution headers.
- For OpenAI-compatible Completions proxies that need vendor-specific fields, set `agents.defaults.models["provider/model"].params.extra_body` (or `extraBody`) to merge extra JSON into the outbound request body.
- For vLLM chat-template controls, set `agents.defaults.models["provider/model"].params.chat_template_kwargs`. The bundled vLLM plugin automatically sends `enable_thinking: false` and `force_nonempty_content: true` for `vllm/nemotron-3-*` when the session thinking level is off.
- For slow local models or remote LAN/tailnet hosts, set `models.providers.<id>.timeoutSeconds`. This extends provider model HTTP request handling, includin

_… [truncated; see https://docs.openclaw.ai/concepts/model-providers for full content]_


---

## https://docs.openclaw.ai/concepts/model-providers.md

_Source: <https://docs.openclaw.ai/concepts/model-providers.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Model providers

Reference for \*\*LLM/model providers\*\* (not chat channels like WhatsApp/Telegram). For model selection rules, see \[Models\](/concepts/models).

\## Quick rules

 \\* Model refs use \`provider/model\` (example: \`opencode/claude-opus-4-6\`).
 \\* \`agents.defaults.models\` acts as an allowlist when set.
 \\* CLI helpers: \`openclaw onboard\`, \`openclaw models list\`, \`openclaw models set \`.
 \\* \`models.providers.\*.contextWindow\` / \`contextTokens\` / \`maxTokens\` set provider-level defaults; \`models.providers.\*.models\[\].contextWindow\` / \`contextTokens\` / \`maxTokens\` override them per model.
 \\* Fallback rules, cooldown probes, and session-override persistence: \[Model failover\](/concepts/model-failover).

 \`openclaw configure\` preserves an existing \`agents.defaults.model.primary\` when you add or reauth a provider. Provider plugins may still return a recommended default model in their auth config patch, but configure treats that as "make this model available" when a primary model already exists, not "replace the current primary model."

 To intentionally switch the default model, use \`openclaw models set \` or \`openclaw models auth login --provider  --set-default\`.

 OpenAI-family routes are prefix-specific:

 \\* \`openai/\` plus \`agents.defaults.agentRuntime.id: "codex"\` uses the native Codex app-server harness. This is the usual ChatGPT/Codex subscription setup.
 \\* \`openai-codex/\` uses Codex OAuth in PI.
 \\* \`openai/\` without a Codex runtime override uses the direct OpenAI API-key provider in PI.

 See \[OpenAI\](/providers/openai) and \[Codex harness\](/plugins/codex-harness). If the provider/runtime split is confusing, read \[Agent runtimes\](/concepts/agent-runtimes) first.

 Plugin auto-enable follows the same boundary: \`openai-codex/\` belongs to the OpenAI plugin, while the Codex plugin is enabled by \`agentRuntime.id: "codex"\` or legacy \`codex/\` refs.

 GPT-5.5 is available through the native Codex app-server harness when \`agentRuntime.id: "codex"\` is set, through \`openai-codex/gpt-5.5\` in PI for Codex OAuth, and through \`openai/gpt-5.5\` in PI for direct API-key traffic when your account exposes it.

 CLI runtimes use the same split: choose canonical model refs such as \`anthropic/claude-\*\`, \`google/gemini-\*\`, or \`openai/gpt-\*\`, then set \`agents.defaults.agentRuntime.id\` to \`claude-cli\`, \`google-gemini-cli\`, or \`codex-cli\` when you want a local CLI backend.

 Legacy \`claude-cli/\*\`, \`google-gemini-cli/\*\`, and \`codex-cli/\*\` refs migrate back to canonical provider refs with the runtime recorded separately.

\## Plugin-owned provider behavior

Most provider-specific logic lives in provider plugins (\`registerProvider(...)\`) while OpenClaw keeps the generic inference loop. Plugins own onboarding, model catalogs, auth env-var mapping, transport/config normalization, tool-schema cleanup, failover classification, OAuth refresh, usage reporting, thinking/reasoning profiles, and more.

The full list of provider-SDK hooks and bundled-plugin examples lives in \[Provider plugins\](/plugins/sdk-provider-plugins). A provider that needs a totally custom request executor is a separate, deeper extension surface.

 Provider-owned runner behavior lives on explicit provider hooks such as replay policy, tool-schema normalization, stream wrapping, and transport/request helpers. The legacy \`ProviderPlugin.capabilities\` static bag is compatibility-only and is no longer read by shared runner logic.

\## API key rotation

 Configure multiple keys via:

 \\* \`OPENCLAW\_LIVE\_\_KEY\` (single live override, highest priority)
 \\* \`\_API\_KEYS\` (comma or semicolon list)
 \\* \`\_API\_KEY\` (primary key)
 \\* \`\_API\_KEY\_\*\` (numbered list, e.g. \`\_API\_KEY\_1\`)

 For Google providers, \`GOOG

_… [truncated; see https://docs.openclaw.ai/concepts/model-providers.md for full content]_


---

## Models CLI - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/models>_

[OpenClaw home page](https://docs.openclaw.ai/)

Concepts and configuration

Models CLI

[**Model failover** \\
\\
Auth profile rotation, cooldowns, and how that interacts with fallbacks.](https://docs.openclaw.ai/concepts/model-failover)

[**Model providers** \\
\\
Quick provider overview and examples.](https://docs.openclaw.ai/concepts/model-providers)

[**Agent runtimes** \\
\\
PI, Codex, and other agent loop runtimes.](https://docs.openclaw.ai/concepts/agent-runtimes)

[**Configuration reference** \\
\\
Model config keys.](https://docs.openclaw.ai/gateway/config-agents#agent-defaults)

Model refs choose a provider and model. They do not usually choose the low-level agent runtime. For example, `openai/gpt-5.5` can run through the normal OpenAI provider path or through the Codex app-server runtime, depending on `agents.defaults.agentRuntime.id`. In Codex runtime mode, the `openai/gpt-*` ref does not imply API-key billing; auth can come from a Codex account or `openai-codex` auth profile. See [Agent runtimes](https://docs.openclaw.ai/concepts/agent-runtimes).

## How model selection works

OpenClaw selects models in this order:

1

[Navigate to header](https://docs.openclaw.ai/concepts/models#)

Primary model

`agents.defaults.model.primary` (or `agents.defaults.model`).

2

[Navigate to header](https://docs.openclaw.ai/concepts/models#)

Fallbacks

`agents.defaults.model.fallbacks` (in order).

3

[Navigate to header](https://docs.openclaw.ai/concepts/models#)

Provider auth failover

Auth failover happens inside a provider before moving to the next model.

Related model surfaces

- `agents.defaults.models` is the allowlist/catalog of models OpenClaw can use (plus aliases).
- `agents.defaults.imageModel` is used **only when** the primary model can’t accept images.
- `agents.defaults.pdfModel` is used by the `pdf` tool. If omitted, the tool falls back to `agents.defaults.imageModel`, then the resolved session/default model.
- `agents.defaults.imageGenerationModel` is used by the shared image-generation capability. If omitted, `image_generate` can still infer an auth-backed provider default. It tries the current default provider first, then the remaining registered image-generation providers in provider-id order. If you set a specific provider/model, also configure that provider’s auth/API key.
- `agents.defaults.musicGenerationModel` is used by the shared music-generation capability. If omitted, `music_generate` can still infer an auth-backed provider default. It tries the current default provider first, then the remaining registered music-generation providers in provider-id order. If you set a specific provider/model, also configure that provider’s auth/API key.
- `agents.defaults.videoGenerationModel` is used by the shared video-generation capability. If omitted, `video_generate` can still infer an auth-backed provider default. It tries the current default provider first, then the remaining registered video-generation providers in provider-id order. If you set a specific provider/model, also configure that provider’s auth/API key.
- Per-agent defaults can override `agents.defaults.model` via `agents.list[].model` plus bindings (see [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)).

## Selection source and fallback behavior

The same `provider/model` can mean different things depending on where it came from:

- Configured defaults (`agents.defaults.model.primary` and agent-specific primaries) are the normal starting point and use `agents.defaults.model.fallbacks`.
- Auto fallback selections are temporary recovery state. They are stored with `modelOverrideSource: "auto"` so later turns can keep using the fallback chain without probing a known-bad primary first.
- User session selections are exact. `/model`, the model picker, `session_status(model=...)`, and `sessions.patch` store `modelOverrideSource: "user"`; if that selected provider/model is unreachable, OpenClaw fails visibly instead of falling throug

_… [truncated; see https://docs.openclaw.ai/concepts/models for full content]_


---

## Multi-agent routing - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/multi-agent>_

[OpenClaw home page](https://docs.openclaw.ai/)

Multi-agent

Multi-agent routing

Run multiple _isolated_ agents — each with its own workspace, state directory (`agentDir`), and session history — plus multiple channel accounts (e.g. two WhatsApps) in one running Gateway. Inbound messages are routed to the right agent through bindings.An **agent** here is the full per-persona scope: workspace files, auth profiles, model registry, and session store. `agentDir` is the on-disk state directory that holds this per-agent config at `~/.openclaw/agents/<agentId>/`. A **binding** maps a channel account (e.g. a Slack workspace or a WhatsApp number) to one of those agents.

## What is “one agent”?

An **agent** is a fully scoped brain with its own:

- **Workspace** (files, AGENTS.md/SOUL.md/USER.md, local notes, persona rules).
- **State directory** (`agentDir`) for auth profiles, model registry, and per-agent config.
- **Session store** (chat history + routing state) under `~/.openclaw/agents/<agentId>/sessions`.

Auth profiles are **per-agent**. Each agent reads from its own:

```
~/.openclaw/agents/<agentId>/agent/auth-profiles.json
```

`sessions_history` is the safer cross-session recall path here too: it returns a bounded, sanitized view, not a raw transcript dump. Assistant recall strips thinking tags, `<relevant-memories>` scaffolding, plain-text tool-call XML payloads (including `<tool_call>...</tool_call>`, `<function_call>...</function_call>`, `<tool_calls>...</tool_calls>`, `<function_calls>...</function_calls>`, and truncated tool-call blocks), downgraded tool-call scaffolding, leaked ASCII/full-width model control tokens, and malformed MiniMax tool-call XML before redaction/truncation.

Never reuse `agentDir` across agents (it causes auth/session collisions). Agents
can read through to the default/main agent’s auth profiles when they do not have
a local profile, but OpenClaw does not clone OAuth refresh tokens into the
secondary agent store. If you want an independent OAuth account, sign in from
that agent; if you copy credentials manually, copy only portable static
`api_key` or `token` profiles.

Skills are loaded from each agent workspace plus shared roots such as `~/.openclaw/skills`, then filtered by the effective agent skill allowlist when configured. Use `agents.defaults.skills` for a shared baseline and `agents.list[].skills` for per-agent replacement. See [Skills: per-agent vs shared](https://docs.openclaw.ai/tools/skills#per-agent-vs-shared-skills) and [Skills: agent skill allowlists](https://docs.openclaw.ai/tools/skills#agent-skill-allowlists).The Gateway can host **one agent** (default) or **many agents** side-by-side.

**Workspace note:** each agent’s workspace is the **default cwd**, not a hard sandbox. Relative paths resolve inside the workspace, but absolute paths can reach other host locations unless sandboxing is enabled. See [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing).

## Paths (quick map)

- Config: `~/.openclaw/openclaw.json` (or `OPENCLAW_CONFIG_PATH`)
- State dir: `~/.openclaw` (or `OPENCLAW_STATE_DIR`)
- Workspace: `~/.openclaw/workspace` (or `~/.openclaw/workspace-<agentId>`)
- Agent dir: `~/.openclaw/agents/<agentId>/agent` (or `agents.list[].agentDir`)
- Sessions: `~/.openclaw/agents/<agentId>/sessions`

### Single-agent mode (default)

If you do nothing, OpenClaw runs a single agent:

- `agentId` defaults to **`main`**.
- Sessions are keyed as `agent:main:<mainKey>`.
- Workspace defaults to `~/.openclaw/workspace` (or `~/.openclaw/workspace-<profile>` when `OPENCLAW_PROFILE` is set).
- State defaults to `~/.openclaw/agents/main/agent`.

## Agent helper

Use the agent wizard to add a new isolated agent:

```
openclaw agents add work
```

Then add `bindings` (or let the wizard do it) to route inbound messages.Verify with:

```
openclaw agents list --bindings
```

## Quick start

1

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

Create each agent workspace

Use

_… [truncated; see https://docs.openclaw.ai/concepts/multi-agent for full content]_


---

## OAuth - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/oauth>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

OAuth

OpenClaw supports “subscription auth” via OAuth for providers that offer it
(notably **OpenAI Codex (ChatGPT OAuth)**). For Anthropic, the practical split
is now:

- **Anthropic API key**: normal Anthropic API billing
- **Anthropic Claude CLI / subscription auth inside OpenClaw**: Anthropic staff
told us this usage is allowed again

OpenAI Codex OAuth is explicitly supported for use in external tools like
OpenClaw. This page explains:For Anthropic in production, API key auth is the safer recommended path.

- how the OAuth **token exchange** works (PKCE)
- where tokens are **stored** (and why)
- how to handle **multiple accounts** (profiles + per-session overrides)

OpenClaw also supports **provider plugins** that ship their own OAuth or API‑key
flows. Run them via:

```
openclaw models auth login --provider <id>
```

## The token sink (why it exists)

OAuth providers commonly mint a **new refresh token** during login/refresh flows. Some providers (or OAuth clients) can invalidate older refresh tokens when a new one is issued for the same user/app.Practical symptom:

- you log in via OpenClaw _and_ via Claude Code / Codex CLI → one of them randomly gets “logged out” later

To reduce that, OpenClaw treats `auth-profiles.json` as a **token sink**:

- the runtime reads credentials from **one place**
- we can keep multiple profiles and route them deterministically
- external CLI reuse is provider-specific: Codex CLI can bootstrap an empty
`openai-codex:default` profile, but once OpenClaw has a local OAuth profile,
the local refresh token is canonical; other integrations can remain
externally managed and re-read their CLI auth store
- status and startup paths that already know the configured provider set scope
external CLI discovery to that set, so an unrelated CLI login store is not
probed for a single-provider setup

## Storage (where tokens live)

Secrets are stored in agent auth stores:

- Auth profiles (OAuth + API keys + optional value-level refs): `~/.openclaw/agents/<agentId>/agent/auth-profiles.json`
- Legacy compatibility file: `~/.openclaw/agents/<agentId>/agent/auth.json`
(static `api_key` entries are scrubbed when discovered)

Legacy import-only file (still supported, but not the main store):

- `~/.openclaw/credentials/oauth.json` (imported into `auth-profiles.json` on first use)

All of the above also respect `$OPENCLAW_STATE_DIR` (state dir override). Full reference: [/gateway/configuration](https://docs.openclaw.ai/gateway/configuration-reference#auth-storage)For static secret refs and runtime snapshot activation behavior, see [Secrets Management](https://docs.openclaw.ai/gateway/secrets).When a secondary agent has no local auth profile, OpenClaw uses read-through
inheritance from the default/main agent store. It does not clone the main
agent’s `auth-profiles.json` on read. OAuth refresh tokens are especially
sensitive: normal copy flows skip them by default because some providers rotate
or invalidate refresh tokens after use. Configure a separate OAuth login for an
agent when it needs an independent account.

## Anthropic legacy token compatibility

Anthropic’s public Claude Code docs say direct Claude Code use stays within
Claude subscription limits, and Anthropic staff told us OpenClaw-style Claude
CLI usage is allowed again. OpenClaw therefore treats Claude CLI reuse and
`claude -p` usage as sanctioned for this integration unless Anthropic
publishes a new policy.For Anthropic’s current direct-Claude-Code plan docs, see [Using Claude Code\\
with your Pro or Max\\
plan](https://support.claude.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan)
and [Using Claude Code with your Team or Enterprise\\
plan](https://support.anthropic.com/en/articles/11845131-using-claude-code-with-your-team-or-enterprise-plan/).If you want other subscription-style options in OpenClaw, see [OpenAI\\
Codex](https://docs.openclaw.ai/providers/openai), [

_… [truncated; see https://docs.openclaw.ai/concepts/oauth for full content]_


---

## Parallel specialist lanes - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/parallel-specialist-lanes>_

# Lane contract

## Owns

- <job this lane is responsible for>

## Does not own

- <work to hand off>

## Chat budget

- Answer quick questions directly.
- For multi-step, slow, or tool-heavy work: acknowledge briefly, spawn/background
  the work, then return the result when complete.

## Handoff

If another lane owns the request, reply with:

- target lane
- objective
- relevant context
- exact next action

## Tool posture

Use the smallest tool surface that can complete the task. Avoid broad shell or
network work unless this lane explicitly owns it.
```

## Related

- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)
- [Command queue](https://docs.openclaw.ai/concepts/queue)
- [Sub-agents](https://docs.openclaw.ai/tools/subagents)

[Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent) [Presence](https://docs.openclaw.ai/concepts/presence)

Ctrl+I


---

## Command queue - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/queue>_

[OpenClaw home page](https://docs.openclaw.ai/)

Messages and delivery

Command queue

We serialize inbound auto-reply runs (all channels) through a tiny in-process queue to prevent multiple agent runs from colliding, while still allowing safe parallelism across sessions.

## Why

- Auto-reply runs can be expensive (LLM calls) and can collide when multiple inbound messages arrive close together.
- Serializing avoids competing for shared resources (session files, logs, CLI stdin) and reduces the chance of upstream rate limits.

## How it works

- A lane-aware FIFO queue drains each lane with a configurable concurrency cap (default 1 for unconfigured lanes; main defaults to 4, subagent to 8).
- `runEmbeddedPiAgent` enqueues by **session key** (lane `session:<key>`) to guarantee only one active run per session.
- Each session run is then queued into a **global lane** (`main` by default) so overall parallelism is capped by `agents.defaults.maxConcurrent`.
- When verbose logging is enabled, queued runs emit a short notice if they waited more than ~2s before starting.
- Typing indicators still fire immediately on enqueue (when supported by the channel) so user experience is unchanged while we wait our turn.

## Defaults

When unset, all inbound channel surfaces use:

- `mode: "steer"`
- `debounceMs: 500`
- `cap: 20`
- `drop: "summarize"`

`steer` is the default because it keeps the active model turn responsive without
starting a second session run. It drains all steering messages that arrived
before the next model boundary. If the current run cannot accept steering,
OpenClaw falls back to a followup queue entry.

## Queue modes

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

## Queue options

Options apply to `followup`, `collect`, and `steer-backlog` (and to `steer` or legacy `queue` when steering falls back to followup):

- `debounceMs`: quiet window before draining queued followups. Bare numbers are milliseconds; units `ms`, `s`, `m`, `h`, and `d` are accepted by `/queue` options.
- `cap`: max queued messages per session. Values below `1` are ignored.
- `drop: "summarize"`: default. Drop the oldest queued entries as needed, keep compact summaries, and inject them as a synthetic followup prompt.
- `drop: "old"`: drop the oldest queued entries as needed, without preserving summaries.
- `drop: "new"`: reject

_… [truncated; see https://docs.openclaw.ai/concepts/queue for full content]_


---

## Retry policy - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/retry>_

[OpenClaw home page](https://docs.openclaw.ai/)

Messages and delivery

Retry policy

## Goals

- Retry per HTTP request, not per multi-step flow.
- Preserve ordering by retrying only the current step.
- Avoid duplicating non-idempotent operations.

## Defaults

- Attempts: 3
- Max delay cap: 30000 ms
- Jitter: 0.1 (10 percent)
- Provider defaults:
  - Telegram min delay: 400 ms
  - Discord min delay: 500 ms

## Behavior

### Model providers

- OpenClaw lets provider SDKs handle normal short retries.
- For Stainless-based SDKs such as Anthropic and OpenAI, retryable responses
(`408`, `409`, `429`, and `5xx`) can include `retry-after-ms` or
`retry-after`. When that wait is longer than 60 seconds, OpenClaw injects
`x-should-retry: false` so the SDK surfaces the error immediately and model
failover can rotate to another auth profile or fallback model.
- Override the cap with `OPENCLAW_SDK_RETRY_MAX_WAIT_SECONDS=<seconds>`.
Set it to `0`, `false`, `off`, `none`, or `disabled` to let SDKs honor long
`Retry-After` sleeps internally.

### Discord

- Retries on rate-limit errors (HTTP 429), request timeouts, HTTP 5xx responses,
and transient transport failures such as DNS lookup failures, connection
resets, socket closes, and fetch failures.
- Uses Discord `retry_after` when available, otherwise exponential backoff.

### Telegram

- Retries on transient errors (429, timeout, connect/reset/closed, temporarily unavailable).
- Uses `retry_after` when available, otherwise exponential backoff.
- Markdown parse errors are not retried; they fall back to plain text.

## Configuration

Set retry policy per provider in `~/.openclaw/openclaw.json`:

```
{
  channels: {
    telegram: {
      retry: {
        attempts: 3,
        minDelayMs: 400,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
    },
    discord: {
      retry: {
        attempts: 3,
        minDelayMs: 500,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
    },
  },
}
```

## Notes

- Retries apply per request (message send, media upload, reaction, poll, sticker).
- Composite flows do not retry completed steps.

## Related

- [Model failover](https://docs.openclaw.ai/concepts/model-failover)
- [Command queue](https://docs.openclaw.ai/concepts/queue)

[Streaming and chunking](https://docs.openclaw.ai/concepts/streaming) [Command queue](https://docs.openclaw.ai/concepts/queue)

Ctrl+I


---

## Session management - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/session>_

[OpenClaw home page](https://docs.openclaw.ai/)

Sessions and memory

Session management

OpenClaw organizes conversations into **sessions**. Each message is routed to a
session based on where it came from — DMs, group chats, cron jobs, etc.

## How messages are routed

| Source | Behavior |
| --- | --- |
| Direct messages | Shared session by default |
| Group chats | Isolated per group |
| Rooms/channels | Isolated per room |
| Cron jobs | Fresh session per run |
| Webhooks | Isolated per hook |

## DM isolation

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

### Dock linked channels

Dock commands let a user move the current direct-chat session’s reply route to
another linked channel without starting a new session. See
[Channel docking](https://docs.openclaw.ai/concepts/channel-docking) for examples, config, and
troubleshooting.Verify your setup with `openclaw security audit`.

## Session lifecycle

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

## Where state lives

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

## Session maintenance

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

For production-si

_… [truncated; see https://docs.openclaw.ai/concepts/session for full content]_


---

## Session tools - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/session-tool>_

[OpenClaw home page](https://docs.openclaw.ai/)

Sessions and memory

Session tools

OpenClaw gives agents tools to work across sessions, inspect status, and
orchestrate sub-agents.

## Available tools

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

## Listing and reading sessions

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

## Sending cross-session messages

`sessions_send` delivers a message to another session and optionally waits for
the response:

- **Fire-and-forget:** set `timeoutSeconds: 0` to

_… [truncated; see https://docs.openclaw.ai/concepts/session-tool for full content]_


---

## SOUL.md personality guide - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/soul>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

SOUL.md personality guide

`SOUL.md` is where your agent’s voice lives.OpenClaw injects it on normal sessions, so it has real weight. If your agent
sounds bland, hedgy, or weirdly corporate, this is usually the file to fix.

## What belongs in SOUL.md

Put the stuff that changes how the agent feels to talk to:

- tone
- opinions
- brevity
- humor
- boundaries
- default level of bluntness

Do **not** turn it into:

- a life story
- a changelog
- a security policy dump
- a giant wall of vibes with no behavioral effect

Short beats long. Sharp beats vague.

## Why this works

This lines up with OpenAI’s prompt guidance:

- The prompt engineering guide says high-level behavior, tone, goals, and
examples belong in the high-priority instruction layer, not buried in the
user turn.
- The same guide recommends treating prompts like something you iterate on,
pin, and evaluate, not magical prose you write once and forget.

For OpenClaw, `SOUL.md` is that layer.If you want better personality, write stronger instructions. If you want stable
personality, keep them concise and versioned.OpenAI refs:

- [Prompt engineering](https://developers.openai.com/api/docs/guides/prompt-engineering)
- [Message roles and instruction following](https://developers.openai.com/api/docs/guides/prompt-engineering#message-roles-and-instruction-following)

## The Molty prompt

Paste this into your agent and let it rewrite `SOUL.md`.Path fixed for OpenClaw workspaces: use `SOUL.md`, not `http://SOUL.md`.

```
Read your `SOUL.md`. Now rewrite it with these changes:

1. You have opinions now. Strong ones. Stop hedging everything with "it depends" - commit to a take.
2. Delete every rule that sounds corporate. If it could appear in an employee handbook, it doesn't belong here.
3. Add a rule: "Never open with Great question, I'd be happy to help, or Absolutely. Just answer."
4. Brevity is mandatory. If the answer fits in one sentence, one sentence is what I get.
5. Humor is allowed. Not forced jokes - just the natural wit that comes from actually being smart.
6. You can call things out. If I'm about to do something dumb, say so. Charm over cruelty, but don't sugarcoat.
7. Swearing is allowed when it lands. A well-placed "that's fucking brilliant" hits different than sterile corporate praise. Don't force it. Don't overdo it. But if a situation calls for a "holy shit" - say holy shit.
8. Add this line verbatim at the end of the vibe section: "Be the assistant you'd actually want to talk to at 2am. Not a corporate drone. Not a sycophant. Just... good."

Save the new `SOUL.md`. Welcome to having a personality.
```

## What good looks like

Good `SOUL.md` rules sound like this:

- have a take
- skip filler
- be funny when it fits
- call out bad ideas early
- stay concise unless depth is actually useful

Bad `SOUL.md` rules sound like this:

- maintain professionalism at all times
- provide comprehensive and thoughtful assistance
- ensure a positive and supportive experience

That second list is how you get mush.

## One warning

Personality is not permission to be sloppy.Keep `AGENTS.md` for operating rules. Keep `SOUL.md` for voice, stance, and
style. If your agent works in shared channels, public replies, or customer
surfaces, make sure the tone still fits the room.Sharp is good. Annoying is not.

## Related docs

- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)
- [System prompt](https://docs.openclaw.ai/concepts/system-prompt)
- [SOUL.md template](https://docs.openclaw.ai/reference/templates/SOUL)

[Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace) [OAuth](https://docs.openclaw.ai/concepts/oauth)

Ctrl+I


---

## Streaming and chunking - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/streaming>_

[OpenClaw home page](https://docs.openclaw.ai/)

Messages and delivery

Streaming and chunking

OpenClaw has two separate streaming layers:

- **Block streaming (channels):** emit completed **blocks** as the assistant writes. These are normal channel messages (not token deltas).
- **Preview streaming (Telegram/Discord/Slack):** update a temporary **preview message** while generating.

There is **no true token-delta streaming** to channel messages today. Preview streaming is message-based (send + edits/appends).

## Block streaming (channel messages)

Block streaming sends assistant output in coarse chunks as it becomes available.

```
Model output
  └─ text_delta/events
       ├─ (blockStreamingBreak=text_end)
       │    └─ chunker emits blocks as buffer grows
       └─ (blockStreamingBreak=message_end)
            └─ chunker flushes at message_end
                   └─ channel send (block replies)
```

Legend:

- `text_delta/events`: model stream events (may be sparse for non-streaming models).
- `chunker`: `EmbeddedBlockChunker` applying min/max bounds + break preference.
- `channel send`: actual outbound messages (block replies).

**Controls:**

- `agents.defaults.blockStreamingDefault`: `"on"`/`"off"` (default off).
- Channel overrides: `*.blockStreaming` (and per-account variants) to force `"on"`/`"off"` per channel.
- `agents.defaults.blockStreamingBreak`: `"text_end"` or `"message_end"`.
- `agents.defaults.blockStreamingChunk`: `{ minChars, maxChars, breakPreference? }`.
- `agents.defaults.blockStreamingCoalesce`: `{ minChars?, maxChars?, idleMs? }` (merge streamed blocks before send).
- Channel hard cap: `*.textChunkLimit` (e.g., `channels.whatsapp.textChunkLimit`).
- Channel chunk mode: `*.chunkMode` (`length` default, `newline` splits on blank lines (paragraph boundaries) before length chunking).
- Discord soft cap: `channels.discord.maxLinesPerMessage` (default 17) splits tall replies to avoid UI clipping.

**Boundary semantics:**

- `text_end`: stream blocks as soon as chunker emits; flush on each `text_end`.
- `message_end`: wait until assistant message finishes, then flush buffered output.

`message_end` still uses the chunker if the buffered text exceeds `maxChars`, so it can emit multiple chunks at the end.

### Media delivery with block streaming

`MEDIA:` directives are normal delivery metadata. When block streaming sends a
media block early, OpenClaw remembers that delivery for the turn. If the final
assistant payload repeats the same media URL, the final delivery strips the
duplicate media instead of sending the attachment again.Exact duplicate final payloads are suppressed. If the final payload adds
distinct text around media that was already streamed, OpenClaw still sends the
new text while keeping the media single-delivery. This prevents duplicate voice
notes or files on channels such as Telegram when an agent emits `MEDIA:` during
streaming and the provider also includes it in the completed reply.

## Chunking algorithm (low/high bounds)

Block chunking is implemented by `EmbeddedBlockChunker`:

- **Low bound:** don’t emit until buffer >= `minChars` (unless forced).
- **High bound:** prefer splits before `maxChars`; if forced, split at `maxChars`.
- **Break preference:**`paragraph` → `newline` → `sentence` → `whitespace` → hard break.
- **Code fences:** never split inside fences; when forced at `maxChars`, close + reopen the fence to keep Markdown valid.

`maxChars` is clamped to the channel `textChunkLimit`, so you can’t exceed per-channel caps.

## Coalescing (merge streamed blocks)

When block streaming is enabled, OpenClaw can **merge consecutive block chunks**
before sending them out. This reduces “single-line spam” while still providing
progressive output.

- Coalescing waits for **idle gaps** (`idleMs`) before flushing.
- Buffers are capped by `maxChars` and will flush if they exceed it.
- `minChars` prevents tiny fragments from sending until enough text accumulates
(final flush always sends rem

_… [truncated; see https://docs.openclaw.ai/concepts/streaming for full content]_


---

## System prompt - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/system-prompt>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

System prompt

OpenClaw builds a custom system prompt for every agent run. The prompt is **OpenClaw-owned** and does not use the pi-coding-agent default prompt.The prompt is assembled by OpenClaw and injected into each agent run.Provider plugins can contribute cache-aware prompt guidance without replacing
the full OpenClaw-owned prompt. The provider runtime can:

- replace a small set of named core sections (`interaction_style`,
`tool_call_style`, `execution_bias`)
- inject a **stable prefix** above the prompt cache boundary
- inject a **dynamic suffix** below the prompt cache boundary

Use provider-owned contributions for model-family-specific tuning. Keep legacy
`before_prompt_build` prompt mutation for compatibility or truly global prompt
changes, not normal provider behavior.The OpenAI GPT-5 family overlay keeps the core execution rule small and adds
model-specific guidance for persona latching, concise output, tool discipline,
parallel lookup, deliverable coverage, verification, missing context, and
terminal-tool hygiene.

## Structure

The prompt is intentionally compact and uses fixed sections:

- **Tooling**: structured-tool source-of-truth reminder plus runtime tool-use guidance.
- **Execution Bias**: compact follow-through guidance: act in-turn on
actionable requests, continue until done or blocked, recover from weak tool
results, check mutable state live, and verify before finalizing.
- **Safety**: short guardrail reminder to avoid power-seeking behavior or bypassing oversight.
- **Skills** (when available): tells the model how to load skill instructions on demand.
- **OpenClaw Self-Update**: how to inspect config safely with
`config.schema.lookup`, patch config with `config.patch`, replace the full
config with `config.apply`, and run `update.run` only on explicit user
request. The owner-only `gateway` tool also refuses to rewrite
`tools.exec.ask` / `tools.exec.security`, including legacy `tools.bash.*`
aliases that normalize to those protected exec paths.
- **Workspace**: working directory (`agents.defaults.workspace`).
- **Documentation**: local path to OpenClaw docs (repo or npm package) and when to read them.
- **Workspace Files (injected)**: indicates bootstrap files are included below.
- **Sandbox** (when enabled): indicates sandboxed runtime, sandbox paths, and whether elevated exec is available.
- **Current Date & Time**: user-local time, timezone, and time format.
- **Reply Tags**: optional reply tag syntax for supported providers.
- **Heartbeats**: heartbeat prompt and ack behavior, when heartbeats are enabled for the default agent.
- **Runtime**: host, OS, node, model, repo root (when detected), thinking level (one line).
- **Reasoning**: current visibility level + /reasoning toggle hint.

OpenClaw keeps large stable content, including **Project Context**, above the
internal prompt cache boundary. Volatile channel/session sections such as
Control UI embed guidance, **Messaging**, **Voice**, **Group Chat Context**,
**Reactions**, **Heartbeats**, and **Runtime** are appended below that boundary
so local backends with prefix caches can reuse the stable workspace prefix
across channel turns. Tool descriptions should likewise avoid embedding current
channel names when the accepted schema already carries that runtime detail.The Tooling section also includes runtime guidance for long-running work:

- use cron for future follow-up (`check back later`, reminders, recurring work)
instead of `exec` sleep loops, `yieldMs` delay tricks, or repeated `process`
polling
- use `exec` / `process` only for commands that start now and continue running
in the background
- when automatic completion wake is enabled, start the command once and rely on
the push-based wake path when it emits output or fails
- use `process` for logs, status, input, or intervention when you need to
inspect a running command
- if the task is larger, prefer `sessions_spawn`; sub-agent compl

_… [truncated; see https://docs.openclaw.ai/concepts/system-prompt for full content]_


---

## Usage tracking - OpenClaw

_Source: <https://docs.openclaw.ai/concepts/usage-tracking>_

[OpenClaw home page](https://docs.openclaw.ai/)

Concept internals

Usage tracking

## What it is

- Pulls provider usage/quota directly from their usage endpoints.
- No estimated costs; only the provider-reported windows.
- Human-readable status output is normalized to `X% left`, even when an
upstream API reports consumed quota, remaining quota, or only raw counts.
- Session-level `/status` and `session_status` can fall back to the latest
transcript usage entry when the live session snapshot is sparse. That
fallback fills missing token/cache counters, can recover the active runtime
model label, and prefers the larger prompt-oriented total when session
metadata is missing or smaller. Existing nonzero live values still win.

## Where it shows up

- `/status` in chats: emoji‑rich status card with session tokens + estimated cost (API key only). Provider usage shows for the **current model provider** when available as a normalized `X% left` window.
- `/usage off|tokens|full` in chats: per-response usage footer (OAuth shows tokens only).
- `/usage cost` in chats: local cost summary aggregated from OpenClaw session logs.
- CLI: `openclaw status --usage` prints a full per-provider breakdown.
- CLI: `openclaw channels list` prints the same usage snapshot alongside provider config (use `--no-usage` to skip).
- macOS menu bar: “Usage” section under Context (only if available).

## Providers + credentials

- **Anthropic (Claude)**: OAuth tokens in auth profiles.
- **GitHub Copilot**: OAuth tokens in auth profiles.
- **Gemini CLI**: OAuth tokens in auth profiles.

  - JSON usage falls back to `stats`; `stats.cached` is normalized into
    `cacheRead`.
- **OpenAI Codex**: OAuth tokens in auth profiles (accountId used when present).
- **MiniMax**: API key or MiniMax OAuth auth profile. OpenClaw treats
`minimax`, `minimax-cn`, and `minimax-portal` as the same MiniMax quota
surface, prefers stored MiniMax OAuth when present, and otherwise falls back
to `MINIMAX_CODE_PLAN_KEY`, `MINIMAX_CODING_API_KEY`, or `MINIMAX_API_KEY`.
Usage polling derives the Coding Plan host from `models.providers.minimax-portal.baseUrl`
or `models.providers.minimax.baseUrl` when configured, and otherwise uses the
MiniMax CN host.
MiniMax’s raw `usage_percent` / `usagePercent` fields mean **remaining**
quota, so OpenClaw inverts them before display; count-based fields win when
present.

  - Coding-plan window labels come from provider hours/minutes fields when
    present, then fall back to the `start_time` / `end_time` span.
  - If the coding-plan endpoint returns `model_remains`, OpenClaw prefers the
    chat-model entry, derives the window label from timestamps when explicit
    `window_hours` / `window_minutes` fields are absent, and includes the model
    name in the plan label.
- **Xiaomi MiMo**: API key via env/config/auth store (`XIAOMI_API_KEY`).
- **z.ai**: API key via env/config/auth store.

Usage is hidden when no usable provider usage auth can be resolved. Providers
can supply plugin-specific usage auth logic; otherwise OpenClaw falls back to
matching OAuth/API-key credentials from auth profiles, environment variables,
or config.

## Related

- [Token use and costs](https://docs.openclaw.ai/reference/token-use)
- [API usage and costs](https://docs.openclaw.ai/reference/api-usage-costs)
- [Prompt caching](https://docs.openclaw.ai/reference/prompt-caching)

[Typing indicators](https://docs.openclaw.ai/concepts/typing-indicators) [Timezones](https://docs.openclaw.ai/concepts/timezone)

Ctrl+I


---

_3 additional pages omitted to keep this file ≤ 146 KB. See https://docs.openclaw.ai for full content._
