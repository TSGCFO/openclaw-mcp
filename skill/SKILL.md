---
name: openclaw
description: "Comprehensive OpenClaw 🦞 expertise built from the official docs.openclaw.ai. OpenClaw is a self-hosted gateway that connects chat apps (Discord, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, WebChat, and more) to AI coding agents like Pi/Codex/Claude. Use when installing or upgrading OpenClaw, running and operating the Gateway daemon, configuring agents/providers/models/runtimes, wiring channels and pairing devices, building or invoking tools/plugins/skills, managing sessions and credentials/SecretRef, deploying via Docker/Kubernetes/systemd/launchd, exposing OpenAI-compatible endpoints, integrating MCP servers, troubleshooting `openclaw doctor`/logs, or scripting via the `openclaw` CLI."
license: MIT
metadata:
  version: '1.0.0'
  source: https://docs.openclaw.ai
  source_pages: 344
  generated_with: Skill Seekers v3.6.0
---

# OpenClaw Skill

## When to Use This Skill

Use this skill whenever the user is working with **OpenClaw** — the self-hosted, multi-channel AI agent gateway. Trigger conditions include:

- User mentions: OpenClaw, `openclaw` CLI, ClawHub, Pi runtime, "Gateway", or any of `openclaw onboard`, `openclaw gateway`, `openclaw doctor`, `openclaw dashboard`, `openclaw agents`, `openclaw channels`, `openclaw mcp`, `openclaw secrets`.
- Installing, upgrading, uninstalling, or migrating OpenClaw on macOS, Linux, WSL2, Windows, Docker, or Kubernetes.
- Bringing up the Gateway daemon, binding ports, configuring `gateway.auth`, `gateway.bind`, hot-reload, or trusted-proxy mode.
- Configuring **agents** (`agents.defaults.workspace`, sandboxing, sessions, model refs) and **runtimes** (`pi`, `codex`, `claude-cli`).
- Wiring **channels** — Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, WebChat, Zalo — and pairing flows.
- Configuring **providers** (Anthropic, OpenAI, OpenAI-Codex, Bedrock, Cerebras, DeepSeek, GLM/Z.AI, Google, Groq, HuggingFace, Kilocode, LiteLLM, LM Studio, MiniMax, Mistral, Moonshot, NVIDIA, Ollama, OpenRouter, Qianfan, Qwen, plus Claude Max API proxy).
- Building or invoking **tools and plugins** — browser, exec, web search (Brave/DuckDuckGo/Perplexity/SearXNG/Tavily/MiniMax/Ollama), Firecrawl, image/video/TTS generation, Lobster, ClawHub skills, ACP, subagents, multi-agent sandboxes.
- Wiring **MCP servers** (`openclaw mcp`) into the agent loop.
- Managing **sessions, memory, commitments, hooks, secrets/SecretRef, backups, cron, dashboard, devices, nodes (mobile/headless)**.
- Exposing **OpenAI-compatible endpoints** (`/v1/models`, `/v1/embeddings`, `/v1/chat/completions`, `/v1/responses`, `/tools/invoke`) for Open WebUI, LobeChat, LibreChat, RAG/memory pipelines.
- Troubleshooting via `openclaw doctor`, `openclaw logs --follow`, `openclaw gateway status --require-rpc`, `openclaw channels status --probe`.
- Deploying via systemd/launchd/schtasks; running multiple gateways on one host; remote access.

If the user is asking about Kimi Claw (Moonshot's hosted OpenClaw deployment), use this skill plus the channel/provider sections.

## Architecture in 60 Seconds

OpenClaw is **one always-on Gateway process** that owns:

- **Routing** — message intake from any channel/surface to the agent runtime.
- **Sessions** — isolated per agent/workspace/sender, with active memory and compaction.
- **Channels** — built-in (Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat) plus bundled/external **channel plugins** (Matrix, Microsoft Teams, Nostr, Twitch, Zalo, Google Chat, Feishu, etc.).
- **A single embedded agent runtime** — one runtime per Gateway, owning the workspace, bootstrap files, tool execution, and session store. The active runtime is selected via the agent config (`pi`, `codex`, `claude-cli`).
- **A multiplexed control port** (default `18789`, loopback by default) serving WebSocket RPC, HTTP APIs, OpenAI-compatible endpoints, the Control UI, and webhooks.
- **Nodes** — paired iOS/Android/macOS clients for Canvas, camera, voice, and offline workflows.

Layer separation (don't conflate these):

| Layer | Examples | Meaning |
| --- | --- | --- |
| Provider | `openai`, `anthropic`, `openai-codex` | How OpenClaw authenticates and discovers models. |
| Model | `gpt-5.5`, `claude-opus-4-6` | The model selected for the agent turn. |
| Agent runtime | `pi`, `codex`, `claude-cli` | The low-level loop executing the prepared turn. |
| Channel | Telegram, Discord, Slack, WhatsApp | Where messages enter and leave. |

See `references/concepts.md` for the full architecture, agent loop, runtimes, workspaces, sessions, active memory, compaction, hooks, and commitments.

## Quick Reference

### Install (5 minutes)

```bash
# macOS / Linux / WSL2 — one-liner installer (handles Node 24)
curl -fsSL https://openclaw.ai/install.sh | bash

# Windows PowerShell
iwr -useb https://openclaw.ai/install.ps1 | iex

# Install without running onboarding
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard

# Or via npm/pnpm/bun (Node 24 recommended; Node 22.14+ minimum)
npm install -g openclaw@latest

# Verify
openclaw --version
openclaw doctor
```

### Onboard + start the daemon

```bash
# Guided setup (writes ~/.openclaw/openclaw.json, picks provider/model, installs the service)
openclaw onboard --install-daemon

# Open the browser dashboard
openclaw dashboard

# Or run the gateway in the foreground
openclaw gateway --port 18789
openclaw gateway --port 18789 --verbose       # debug/trace mirrored to stdio
openclaw gateway --force                       # force-kill listener on port, then start
```

### Gateway operations

```bash
# Health checks
openclaw gateway status
openclaw gateway status --require-rpc          # require read-scope RPC proof, not just reachability
openclaw status
openclaw logs --follow

# Validate channel readiness (per-account live probes)
openclaw channels status --probe

# Lifecycle
openclaw gateway restart
openclaw gateway install --force               # reinstall the launchd/systemd/schtasks unit
openclaw doctor --fix                          # auto-remediate common issues
```

### Agents and sessions

```bash
# List & inspect agents (an agent = workspace + bootstrap files + runtime + model)
openclaw agents list
openclaw agent show <agentId>

# Sessions (one per agent/workspace/sender)
openclaw sessions list
openclaw sessions show main
openclaw sessions reset main                   # clear stuck session state

# Set a binding (channel-only → account-scoped)
openclaw agents bind --channel telegram --agent main
openclaw agents bind --account "telegram:+15551234" --agent main
```

### Channels (connect a chat app in minutes)

```bash
# Telegram is the fastest first channel
openclaw channels add telegram
openclaw pairing start --channel telegram      # generates pairing code/QR

# Other built-ins (each has its own auth flow — see references/channels.md)
openclaw channels add discord
openclaw channels add slack
openclaw channels add signal
openclaw channels add whatsapp                 # via Baileys
openclaw channels add imessage                 # macOS only
openclaw channels add webchat
```

### Providers and models

```bash
# Add credentials (uses SecretRef under the hood — never plain text in config)
openclaw secrets set OPENAI_API_KEY
openclaw secrets set ANTHROPIC_API_KEY

# Configure a provider
openclaw providers add anthropic
openclaw providers add openai
openclaw providers add openai-codex            # Codex runtime / responses API

# Pick the agent's model
openclaw agents set main --model anthropic/claude-opus-4-6
openclaw agents set main --model openai/gpt-5.5
```

### OpenAI-compatible endpoints (for Open WebUI / LibreChat / RAG)

The Gateway exposes these on the same port as the rest of the API:

```
GET  /v1/models                    -> returns openclaw, openclaw/default, openclaw/<agentId>
GET  /v1/models/{id}
POST /v1/embeddings
POST /v1/chat/completions
POST /v1/responses                 -- agent-native preferred
POST /tools/invoke
```

Override the backend per-request with the `x-openclaw-model` header. Use `openclaw/default` as the stable alias for the configured default agent.

```bash
# Quick smoke test against a locally running Gateway
curl -s http://127.0.0.1:18789/v1/models \
  -H "Authorization: Bearer $OPENCLAW_GATEWAY_TOKEN" | jq

curl -s http://127.0.0.1:18789/v1/chat/completions \
  -H "Authorization: Bearer $OPENCLAW_GATEWAY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"model":"openclaw/default","messages":[{"role":"user","content":"hello"}]}'
```

### Tools, plugins, MCP, skills

```bash
# List/enable built-in tools (browser, exec, web search, image/video/TTS, etc.)
openclaw tools list
openclaw tools enable browser
openclaw tools enable exec --approval prompt   # require approval per call
openclaw tools enable web                       # default search provider

# Channel/tool plugins
openclaw plugins list
openclaw plugins install <plugin>

# MCP servers (give the agent access to MCP tool catalogues)
openclaw mcp list
openclaw mcp add <name> --command "..."

# ClawHub / local skills bundled into the agent
openclaw skills list
openclaw skills install <skill>
```

### Secrets, backups, cron, hooks

```bash
# Secrets — ref-only auth profiles via SecretRef
openclaw secrets list
openclaw secrets apply secrets-plan.yaml
openclaw secrets reload

# Backups (workspace + config + sessions)
openclaw backup create
openclaw backup restore <backup-id>

# Scheduled jobs (recurring agent runs)
openclaw cron list
openclaw cron add "0 9 * * *" --agent main --prompt "Daily standup"

# Hooks — intercept the agent loop (intake, prompt assembly, tool calls, replies, errors)
openclaw hooks list
```

## Configuration File Cheat Sheet

`~/.openclaw/openclaw.json`:

```json
{
  "gateway": {
    "port": 18789,
    "bind": "loopback",
    "auth": { "mode": "shared-secret", "token": "secretref:OPENCLAW_GATEWAY_TOKEN" },
    "reload": { "mode": "hybrid" },
    "controlUi": { "enabled": true, "allowedOrigins": ["http://localhost:18789"] }
  },
  "agents": {
    "defaults": {
      "workspace": "~/.openclaw/workspace",
      "sandbox": true,
      "runtime": "pi",
      "model": "anthropic/claude-opus-4-6"
    }
  },
  "channels": { "telegram": { "enabled": true, "token": "secretref:TELEGRAM_BOT_TOKEN" } }
}
```

| Setting | Resolution order |
| --- | --- |
| Gateway port | `--port` → `OPENCLAW_GATEWAY_PORT` → `gateway.port` → `18789` |
| Bind mode | CLI/override → `gateway.bind` → `loopback` |
| Reload mode | `gateway.reload.mode` (`off` / `hot` / `restart` / `hybrid` default) |
| Auth | `gateway.auth.token` / `gateway.auth.password` or `OPENCLAW_GATEWAY_TOKEN` / `OPENCLAW_GATEWAY_PASSWORD` (or `mode: trusted-proxy` behind a reverse proxy) |

Useful environment variables (full list in `references/help.md`):

- `OPENCLAW_HOME` — internal path resolution root
- `OPENCLAW_STATE_DIR` — override the state directory
- `OPENCLAW_CONFIG_PATH` — override the config file path
- `OPENCLAW_GATEWAY_PORT`, `OPENCLAW_GATEWAY_TOKEN`, `OPENCLAW_GATEWAY_PASSWORD`

## Reference Files (load on demand)

The full corpus (344 cleaned doc pages) is split into categorized references — read only what you need:

| File | When to read |
| --- | --- |
| `references/getting_started.md` | Install, onboarding wizard, first-run setup, environment vars, doctor. |
| `references/concepts.md` | Architecture, agent loop, agent runtimes, workspaces, sessions, active memory, compaction, commitments, context engine. |
| `references/agents.md` | Agent CLI surface, identities, multi-agent routing, runtime selection, model refs, identity files. |
| `references/channels.md` | Per-channel setup: Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, WebChat, Zalo, plus pairing/safety. |
| `references/tools_plugins.md` | Built-in tools (browser, exec, web search, image/video/TTS), plugins, ClawHub skills, ACP, subagents, multi-agent sandboxes. |
| `references/providers_models.md` | Provider configs (OpenAI, Anthropic, Bedrock, Codex, Google, Groq, Ollama, etc.), model listings, OpenAI-compatible API. |
| `references/gateway_ops.md` | Gateway runbook, runtime model, ports/bind, hot reload, OpenAI-compatible endpoints, multi-gateway, troubleshooting, supervision (systemd/launchd/schtasks). |
| `references/cli.md` | Every `openclaw <verb>` subcommand: agent, agents, backup, browser, channels, config, cron, daemon, dashboard, devices, doctor, gateway, hooks, mcp, memory, message, models, nodes, onboard, pairing, plugins, proxy, qr, reset, secrets, sessions, setup, skills, status, tui, uninstall, update, wiki. |
| `references/automation.md` | Cron, hooks, scheduled prompts, tool approvals, automation patterns. |
| `references/mcp.md` | MCP server integration. |
| `references/nodes.md` | iOS/Android/macOS pairing, Canvas, camera, voice (VoiceClaw), media flows. |
| `references/web_console.md` | Browser dashboard / Control UI. |
| `references/platforms.md` | Platform-specific notes (Windows, WSL2, macOS, Docker, K8s). |
| `references/reference.md` | Auth/Credential surface, SecretRef contract, secrets plan contract. |
| `references/help.md` | Environment variables, FAQ, support. |
| `references/other.md` | Pages that don't fit the categories above. |

## Working with This Skill — Recipes

### Recipe 1: Bring up a fresh OpenClaw on a Linux VPS

1. Install: `curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard`
2. Run `openclaw onboard --install-daemon` and pick provider + model (uses SecretRef).
3. Bind to loopback only (default), front with a reverse proxy (`gateway.auth.mode: "trusted-proxy"`) or expose via Tailscale Serve.
4. Verify with `openclaw doctor`, `openclaw gateway status --require-rpc`, `openclaw logs --follow`.
5. Add a first channel (`openclaw channels add telegram`), pair with `openclaw pairing start`.

### Recipe 2: Use OpenClaw as a drop-in OpenAI server

1. Confirm `openclaw/default` resolves: `openclaw agents list`.
2. Point Open WebUI / LibreChat / LangChain at `http://gateway:18789/v1` with `OPENCLAW_GATEWAY_TOKEN` as the bearer.
3. Use the `x-openclaw-model` header when you need to pin a specific upstream provider/model.
4. For RAG/memory pipelines, call `/v1/embeddings` with `model: "openclaw/default"`.

### Recipe 3: Diagnose "agent not responding" / stuck billing errors

1. `openclaw logs --follow | tail -200` — look for "extra usage", auth, or rate-limit lines.
2. `openclaw sessions list` then `openclaw sessions reset <id>` to clear stuck session state.
3. `claude --version` / `codex --version` directly on the host to confirm the runtime CLI is healthy.
4. If the runtime is healthy but the gateway isn't, `systemctl --user restart openclaw-gateway` (or the platform equivalent) and re-run `openclaw gateway status --require-rpc`.
5. If the gateway can't see the runtime: `openclaw doctor --fix`, then check `openclaw mcp list` and `openclaw plugins list` for stale/corrupt entries.

### Recipe 4: Multiple gateways on the same host

- Each gateway gets its own `gateway.port`, state dir (`OPENCLAW_STATE_DIR`), and config (`OPENCLAW_CONFIG_PATH`).
- Re-run `openclaw gateway install --force` for each one to register a separate launchd/systemd/schtasks unit.
- See `references/gateway_ops.md` § "Multiple gateways (same host)".

### Recipe 5: Wire an MCP server into an agent

1. `openclaw mcp add <name> --command "<launch cmd>"` (stdio or HTTP transport).
2. `openclaw mcp list` to confirm registration; tools from the MCP server appear under the agent's tool catalogue.
3. Restart the gateway (`openclaw gateway restart`) and verify with `openclaw status`.
4. To gate execution, use `openclaw tools enable <tool> --approval prompt`.

## Notes and Invariants

- **Loopback by default.** Never expose the gateway port to a public IP without `gateway.auth` set or a `trusted-proxy` reverse proxy in front.
- **Workspace is the single cwd.** All tools and context resolve against `agents.defaults.workspace`. If `agents.defaults.sandbox` is enabled, non-main sessions can override.
- **One agent runtime per Gateway.** Multi-agent routing happens *above* the runtime — multiple agents share one runtime process.
- **SecretRef for credentials.** Configs reference secrets by name (`secretref:NAME`); plain-text tokens in `openclaw.json` are an anti-pattern.
- **Hot reload is hybrid by default.** Edits to the active config file are hot-applied when safe; restart-required changes trigger a restart.
- **Capability probes vs. reachability.** `openclaw gateway status` shows reachability; add `--require-rpc` to actually exercise read-scope RPC.
- **Channels are independent.** A pairing/auth break on Telegram does not affect Slack. Use `openclaw channels status --probe` to validate per-account.

## When You Don't Know — Discovery

1. `openclaw <verb> --help` — every subcommand documents its flags and examples.
2. `openclaw doctor` — environment + config + connectivity audit.
3. The full doc index is at https://docs.openclaw.ai/llms.txt; full text at https://docs.openclaw.ai/llms-full.txt.
4. The 344 reference pages in `references/` are categorized to match the docs nav (Get Started, Concepts, Channels, Tools & Plugins, Providers & Models, Platforms, Gateway & Ops, Reference, Help).
