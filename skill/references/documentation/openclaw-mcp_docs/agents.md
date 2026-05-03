# Openclaw-Mcp_Docs - Agents

**Pages:** 41

---

## OpenClaw

**URL:** https://docs.openclaw.ai/

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Get Started
- Run Onboarding
- Open the Control UI
- ​What is OpenClaw?
- ​How it works
- ​Key capabilities
- Multi-channel gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Any OS gateway for AI agents across Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, and more. Send a message, get an agent response from your pocket. Run one Gateway across built-in channels, bundled channel plugins, WebChat, and mobile nodes.

Onboard and install the service

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## API usage and costs

**URL:** https://docs.openclaw.ai/reference/api-usage-costs

**Contents:**
- API usage and costs
- Documentation Index
- ​API usage & costs
- ​Where costs show up (chat + CLI)
- ​How keys are discovered
- ​Features that can spend keys
  - ​1) Core model responses (chat + tools)
  - ​2) Media understanding (audio/image/video)
  - ​3) Image and video generation
  - ​4) Memory embeddings + semantic search

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## AGENTS.md template

**URL:** https://docs.openclaw.ai/reference/templates/AGENTS

**Contents:**
- AGENTS.md template
- Documentation Index
- ​AGENTS.md - Your Workspace
- ​First Run
- ​Session Startup
- ​Memory
  - ​🧠 MEMORY.md - Your Long-Term Memory
  - ​📝 Write It Down - No “Mental Notes”!
- ​Red Lines
- ​External vs Internal

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "lastChecks": {
    "email": 1703275200,
    "calendar": 1703260800,
    "weather": null
  }
}
```

---

## GitHub Copilot

**URL:** https://docs.openclaw.ai/providers/github-copilot

**Contents:**
- GitHub Copilot
- Documentation Index
- ​Two ways to use Copilot in OpenClaw
- ​Optional flags
- ​Non-interactive onboarding
- ​Memory search embeddings
  - ​Auto-detection
  - ​Explicit config
  - ​How it works
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Run the login command

Interactive TTY required

Model availability depends on your plan

Request compatibility

Environment variable resolution order

**Examples:**

Example 1 (unknown):
```unknown
openclaw models auth login-github-copilot
```

Example 2 (unknown):
```unknown
openclaw models set github-copilot/claude-opus-4.7
```

Example 3 (json):
```json
{
  agents: {
    defaults: { model: { primary: "github-copilot/claude-opus-4.7" } },
  },
}
```

Example 4 (markdown):
```markdown
# Skip confirmation
openclaw models auth login-github-copilot --yes

# Login and set the default model in one step
openclaw models auth login --provider github-copilot --method device --set-default
```

---

## Anthropic

**URL:** https://docs.openclaw.ai/providers/anthropic

**Contents:**
- Anthropic
- Documentation Index
- ​Getting started
  - ​Config example
  - ​Config example
- ​Thinking defaults (Claude 4.6)
- ​Prompt caching
- ​Advanced configuration
- ​Troubleshooting
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Ensure Claude CLI is installed and logged in

Verify the model is available

Per-agent cache overrides

Media understanding (image and PDF)

1M context window (beta)

Claude Opus 4.7 1M context

401 errors / token suddenly invalid

No API key found for provider "anthropic"

No credentials found for profile "anthropic:default"

No available auth profile (all in cooldown)

**Examples:**

Example 1 (markdown):
```markdown
openclaw onboard
# choose: Anthropic API key
```

Example 2 (bash):
```bash
openclaw onboard --anthropic-api-key "$ANTHROPIC_API_KEY"
```

Example 3 (unknown):
```unknown
openclaw models list --provider anthropic
```

Example 4 (lua):
```lua
{
  env: { ANTHROPIC_API_KEY: "sk-ant-..." },
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/fr

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Commencer
- Lancer l’intégration initiale
- Ouvrir la Control UI
- ​Qu’est-ce qu’OpenClaw ?
- ​Fonctionnement
- ​Capacités clés
- Gateway multi-canal

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway multi-OS pour agents IA sur Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, et plus encore. Envoyez un message, obtenez une réponse d’agent depuis votre poche. Exécutez une seule Gateway sur les canaux intégrés, les plugins de canal inclus, WebChat et les nœuds mobiles.

Lancer l’intégration initiale et installer le service

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## ACP agents

**URL:** https://docs.openclaw.ai/tools/acp-agents

**Contents:**
- ACP agents
- Documentation Index
- ​Which page do I want?
- ​Does this work out of the box?
- ​Supported harness targets
- ​Operator runbook
- ​ACP versus sub-agents
- ​How ACP runs Claude Code
- ​Bound sessions
  - ​Mental model

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Runtime prerequisites

Native Codex routing rules

Model / provider / runtime selection cheat sheet

ACP-routing natural-language triggers

Binding rules and exclusivity

Thread-bound sessions

Thread-supporting channels

Interactive ACP sessions

Parent-owned one-shot ACP sessions

sessions_send and A2A delivery

Resume an existing session

Post-deploy smoke test

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/acpx
openclaw config set plugins.entries.acpx.enabled true
```

Example 2 (unknown):
```unknown
/codex bind                                              # native Codex bind, route future messages here
/codex model gpt-5.4                                     # tune the bound native Codex thread
/codex stop                                              # control the active native Codex turn
/acp spawn codex --bind here                             # explicit ACP fallback for Codex
/acp spawn codex --thread auto                           # may create a child thread/topic and bind there
/acp spawn codex --bind here --cwd /workspace/repo       # same chat binding, Codex runs in /workspace/repo
```

Example 3 (json):
```json
{
  agents: {
    list: [
      {
        id: "codex",
        runtime: {
          type: "acp",
          acp: {
            agent: "codex",
            backend: "acpx",
            mode: "persistent",
            cwd: "/workspace/openclaw",
          },
        },
      },
      {
        id: "claude",
        runtime: {
          type: "acp",
          acp: { agent: "claude", backend: "acpx", mode: "persistent" },
        },
      },
    ],
  },
  bindings: [
    {
      type: "acp",
      agentId: "codex",
      match: {
        channel: "discord",
        accountId: "default",
        peer: { kind: "channel", id: "222222222222222222" },
      },
      acp: { label: "codex-main" },
    },
    {
      type: "acp",
      agentId: "claude",
      match: {
        channel: "telegram",
        accountId: "default",
        peer: { kind: "group", id: "-1001234567890:topic:42" },
      },
      acp: { cwd: "/workspace/repo-b" },
    },
    {
      type: "route",
      agentId: "main",
      match: { channel: "discord", accountId: "default" },
    },
    {
      type: "route",
      agentId: "main",
      match: { channel: "telegram", accountId: "default" },
    },
  ],
  channels: {
    discord: {
      guilds: {
        "111111111111111111": {
          channels: {
            "222222222222222222": { requireMention: false },
          },
        },
      },
    },
    telegram: {
      groups: {
        "-1001234567890": {
          topics: { "42": { requireMention: false } },
        },
      },
    },
  },
}
```

Example 4 (json):
```json
{
  "task": "Open the repo and summarize failing tests",
  "runtime": "acp",
  "agentId": "codex",
  "thread": true,
  "mode": "session"
}
```

---

## Agent

**URL:** https://docs.openclaw.ai/cli/agent

**Contents:**
- Agent
- Documentation Index
- ​openclaw agent
- ​Options
- ​Examples
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
openclaw agent --to +15555550123 --message "status update" --deliver
openclaw agent --agent ops --message "Summarize logs"
openclaw agent --agent ops --model openai/gpt-5.4 --message "Summarize logs"
openclaw agent --session-id 1234 --message "Summarize inbox" --thinking medium
openclaw agent --to +15555550123 --message "Trace logs" --verbose on --json
openclaw agent --agent ops --message "Generate report" --deliver --reply-channel slack --reply-to "#reports"
openclaw agent --agent ops --message "Run locally" --local
```

---

## Configuration — agents

**URL:** https://docs.openclaw.ai/gateway/config-agents

**Contents:**
- Configuration — agents
- Documentation Index
- ​Agent defaults
  - ​agents.defaults.workspace
  - ​agents.defaults.repoRoot
  - ​agents.defaults.skills
  - ​agents.defaults.skipBootstrap
  - ​agents.defaults.skipOptionalBootstrapFiles
  - ​agents.defaults.contextInjection
  - ​agents.defaults.bootstrapMaxChars

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

cache-ttl mode behavior

Full access (no sandbox)

Read-only tools + workspace

No filesystem access (messaging only)

Session field details

**Examples:**

Example 1 (json):
```json
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
}
```

Example 2 (json):
```json
{
  agents: { defaults: { repoRoot: "~/Projects/openclaw" } },
}
```

Example 3 (json):
```json
{
  agents: {
    defaults: { skills: ["github", "weather"] },
    list: [
      { id: "writer" }, // inherits github, weather
      { id: "docs", skills: ["docs-search"] }, // replaces defaults
      { id: "locked-down", skills: [] }, // no skills
    ],
  },
}
```

Example 4 (json):
```json
{
  agents: { defaults: { skipBootstrap: true } },
}
```

---

## Media understanding

**URL:** https://docs.openclaw.ai/nodes/media-understanding

**Contents:**
- Media understanding
- Documentation Index
- ​Goals
- ​High-level behavior
- ​Config overview
  - ​Model entries
- ​Defaults and limits
  - ​Auto-detect media understanding (default)
  - ​Proxy environment support (provider models)
- ​Capabilities (optional)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Select per-capability

agents.defaults.imageModel

Local CLIs (audio only)

File-attachment extraction behavior

**Examples:**

Example 1 (json):
```json
{
  tools: {
    media: {
      models: [
        /* shared list */
      ],
      image: {
        /* optional overrides */
      },
      audio: {
        /* optional overrides */
        echoTranscript: true,
        echoFormat: '📝 "{transcript}"',
      },
      video: {
        /* optional overrides */
      },
    },
  },
}
```

Example 2 (json):
```json
{
  type: "provider", // default if omitted
  provider: "openai",
  model: "gpt-5.5",
  prompt: "Describe the image in <= 500 chars.",
  maxChars: 500,
  maxBytes: 10485760,
  timeoutSeconds: 60,
  capabilities: ["image"], // optional, used for multi-modal entries
  profile: "vision-profile",
  preferredProfile: "vision-fallback",
}
```

Example 3 (json):
```json
{
  type: "cli",
  command: "gemini",
  args: [
    "-m",
    "gemini-3-flash",
    "--allowed-tools",
    "read_file",
    "Read the media at {{MediaPath}} and describe it in <= {{MaxChars}} characters.",
  ],
  maxChars: 500,
  maxBytes: 52428800,
  timeoutSeconds: 120,
  capabilities: ["video", "image"],
}
```

Example 4 (json):
```json
{
  tools: {
    media: {
      audio: {
        enabled: false,
      },
    },
  },
}
```

---

## Claude Max API proxy

**URL:** https://docs.openclaw.ai/providers/claude-max-api-proxy

**Contents:**
- Claude Max API proxy
- Documentation Index
- ​Why use this?
- ​How it works
- ​Getting started
- ​Built-in catalog
- ​Advanced configuration
- ​Links
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Proxy-style OpenAI-compatible notes

Auto-start on macOS with LaunchAgent

**Examples:**

Example 1 (unknown):
```unknown
Your App → claude-max-api-proxy → Claude Code CLI → Anthropic (via subscription)
     (OpenAI format)              (converts format)      (uses your login)
```

Example 2 (markdown):
```markdown
npm install -g claude-max-api-proxy

# Verify Claude CLI is authenticated
claude --version
```

Example 3 (markdown):
```markdown
claude-max-api
# Server runs at http://localhost:3456
```

Example 4 (json):
```json
# Health check
curl http://localhost:3456/health

# List models
curl http://localhost:3456/v1/models

# Chat completion
curl http://localhost:3456/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-opus-4",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

---

## Telegram

**URL:** https://docs.openclaw.ai/channels/telegram

**Contents:**
- Telegram
- Documentation Index
- Pairing
- Channel troubleshooting
- Gateway configuration
- ​Quick setup
- ​Telegram side settings
- ​Access control and activation
  - ​Finding your Telegram user ID
- ​Runtime behavior

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create the bot token in BotFather

Configure token and DM policy

Start gateway and approve first DM

Add the bot to a group

Privacy mode and group visibility

Helpful BotFather toggles

Live stream preview (message edits)

Formatting and HTML fallback

Native commands and custom commands

Telegram message actions for agents and automation

Forum topics and thread behavior

Audio, video, and stickers

Reaction notifications

Config writes from Telegram events and commands

Long polling vs webhook

Limits, retry, and CLI targets

Exec approvals in Telegram

Bot does not respond to non mention group messages

Bot not seeing group messages at all

Commands work partially or not at all

Startup reports unauthorized token

Polling or network instability

High-signal Telegram fields

**Examples:**

Example 1 (json):
```json
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",
      groups: { "*": { requireMention: true } },
    },
  },
}
```

Example 2 (typescript):
```typescript
openclaw gateway
openclaw pairing list telegram
openclaw pairing approve telegram <CODE>
```

Example 3 (typescript):
```typescript
curl "https://api.telegram.org/bot<bot_token>/getUpdates"
```

Example 4 (json):
```json
{
  channels: {
    telegram: {
      groups: {
        "-1001234567890": {
          groupPolicy: "open",
          requireMention: false,
        },
      },
    },
  },
}
```

---

## Scheduled tasks

**URL:** https://docs.openclaw.ai/automation/cron-jobs

**Contents:**
- Scheduled tasks
- Documentation Index
- ​Quick start
- ​How cron works
- ​Schedule types
  - ​Day-of-month and day-of-week use OR logic
- ​Execution styles
  - ​Payload options for isolated jobs
- ​Delivery and output
- ​CLI examples

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Add a one-shot reminder

Main session vs isolated vs custom

What 'fresh session' means for isolated jobs

Subagent and Discord delivery

Mapped hooks (POST /hooks/<name>)

Select the GCP project

Create topic and grant Gmail push access

Cron fired but no delivery

Cron or heartbeat appears to prevent /new-style rollover

**Examples:**

Example 1 (json):
```json
openclaw cron add \
  --name "Reminder" \
  --at "2026-02-01T16:00:00Z" \
  --session main \
  --system-event "Reminder: check the cron docs draft" \
  --wake now \
  --delete-after-run
```

Example 2 (unknown):
```unknown
openclaw cron list
openclaw cron show <job-id>
```

Example 3 (unknown):
```unknown
openclaw cron runs --id <job-id>
```

Example 4 (markdown):
```markdown
# Intended: "9 AM on the 15th, only if it's a Monday"
# Actual:   "9 AM on every 15th, AND 9 AM on every Monday"
0 9 15 * 1
```

---

## Tools invoke API

**URL:** https://docs.openclaw.ai/gateway/tools-invoke-http-api

**Contents:**
- Tools invoke API
- Documentation Index
- ​Tools Invoke (HTTP)
- ​Authentication
- ​Security boundary (important)
- ​Request body
- ​Policy + routing behavior
- ​Responses
- ​Example
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "tool": "sessions_list",
  "action": "json",
  "args": {},
  "sessionKey": "main",
  "dryRun": false
}
```

Example 2 (json):
```json
{
  gateway: {
    tools: {
      // Additional tools to block over HTTP /tools/invoke
      deny: ["browser"],
      // Remove tools from the default deny list
      allow: ["gateway"],
    },
  },
}
```

Example 3 (json):
```json
curl -sS http://127.0.0.1:18789/tools/invoke \
  -H 'Authorization: Bearer secret' \
  -H 'Content-Type: application/json' \
  -d '{
    "tool": "sessions_list",
    "action": "json",
    "args": {}
  }'
```

---

## Browser control API

**URL:** https://docs.openclaw.ai/tools/browser-control

**Contents:**
- Browser control API
- Documentation Index
- ​Control API (optional)
  - ​/act error contract
  - ​Playwright requirement
    - ​Docker Playwright install
- ​How it works (internal)
- ​CLI quick reference
- ​Snapshots and refs
- ​Wait power-ups

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Basics: status, tabs, open/focus/close

Inspection: screenshot, snapshot, console, errors, requests

Actions: navigate, click, type, drag, wait, evaluate

State: cookies, storage, offline, headers, geo, device

**Examples:**

Example 1 (json):
```json
{ "error": "<message>", "code": "ACT_*" }
```

Example 2 (unknown):
```unknown
docker compose run --rm openclaw-cli \
  node /app/node_modules/playwright-core/cli.js install chromium
```

Example 3 (sql):
```sql
openclaw browser status
openclaw browser start
openclaw browser start --headless # one-shot local managed headless launch
openclaw browser stop            # also clears emulation on attach-only/remote CDP
openclaw browser tabs
openclaw browser tab             # shortcut for current tab
openclaw browser tab new
openclaw browser tab select 2
openclaw browser tab close 2
openclaw browser open https://example.com
openclaw browser focus abcd1234
openclaw browser close abcd1234
```

Example 4 (unknown):
```unknown
openclaw browser screenshot
openclaw browser screenshot --full-page
openclaw browser screenshot --ref 12        # or --ref e12
openclaw browser screenshot --labels
openclaw browser snapshot
openclaw browser snapshot --format aria --limit 200
openclaw browser snapshot --interactive --compact --depth 6
openclaw browser snapshot --efficient
openclaw browser snapshot --labels
openclaw browser snapshot --urls
openclaw browser snapshot --selector "#main" --interactive
openclaw browser snapshot --frame "iframe#main" --interactive
openclaw browser console --level error
openclaw browser errors --clear
openclaw browser requests --filter api --clear
openclaw browser pdf
openclaw browser responsebody "**/api" --max-chars 5000
```

---

## Pi development workflow

**URL:** https://docs.openclaw.ai/pi-dev

**Contents:**
- Pi development workflow
- Documentation Index
- ​Type checking and linting
- ​Running Pi tests
- ​Manual testing
- ​Clean slate reset
- ​References
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm test \
  "src/agents/pi-*.test.ts" \
  "src/agents/pi-embedded-*.test.ts" \
  "src/agents/pi-tools*.test.ts" \
  "src/agents/pi-settings.test.ts" \
  "src/agents/pi-tool-definition-adapter*.test.ts" \
  "src/agents/pi-hooks/**/*.test.ts"
```

Example 2 (sass):
```sass
OPENCLAW_LIVE_TEST=1 pnpm test src/agents/pi-embedded-runner-extraparams.live.test.ts
```

---

## Default AGENTS.md

**URL:** https://docs.openclaw.ai/reference/AGENTS.default

**Contents:**
- Default AGENTS.md
- Documentation Index
- ​AGENTS.md - OpenClaw Personal Assistant (default)
- ​First run (recommended)
- ​Safety defaults
- ​Session start (required)
- ​Soul (required)
- ​Shared spaces (recommended)
- ​Memory system (recommended)
- ​Tools & skills

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
mkdir -p ~/.openclaw/workspace
```

Example 2 (unknown):
```unknown
cp docs/reference/templates/AGENTS.md ~/.openclaw/workspace/AGENTS.md
cp docs/reference/templates/SOUL.md ~/.openclaw/workspace/SOUL.md
cp docs/reference/templates/TOOLS.md ~/.openclaw/workspace/TOOLS.md
```

Example 3 (unknown):
```unknown
cp docs/reference/AGENTS.default.md ~/.openclaw/workspace/AGENTS.md
```

Example 4 (json):
```json
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
}
```

---

## ACP agents — setup

**URL:** https://docs.openclaw.ai/tools/acp-agents-setup

**Contents:**
- ACP agents — setup
- Documentation Index
- ​acpx harness support (current)
- ​Required config
- ​Plugin setup for acpx backend
  - ​acpx command and version configuration
  - ​Automatic dependency install
  - ​Plugin tools MCP bridge
  - ​OpenClaw tools MCP bridge
  - ​Runtime timeout configuration

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  acp: {
    enabled: true,
    // Optional. Default is true; set false to pause ACP dispatch while keeping /acp controls.
    dispatch: { enabled: true },
    backend: "acpx",
    defaultAgent: "codex",
    allowedAgents: [
      "claude",
      "codex",
      "copilot",
      "cursor",
      "droid",
      "gemini",
      "iflow",
      "kilocode",
      "kimi",
      "kiro",
      "openclaw",
      "opencode",
      "pi",
      "qwen",
    ],
    maxConcurrentSessions: 8,
    stream: {
      coalesceIdleMs: 300,
      maxChunkChars: 1200,
    },
    runtime: {
      ttlMinutes: 120,
    },
  },
}
```

Example 2 (json):
```json
{
  session: {
    threadBindings: {
      enabled: true,
      idleHours: 24,
      maxAgeHours: 0,
    },
  },
  channels: {
    discord: {
      threadBindings: {
        enabled: true,
        spawnSessions: true,
      },
    },
  },
}
```

Example 3 (elixir):
```elixir
openclaw plugins install @openclaw/acpx
openclaw config set plugins.entries.acpx.enabled true
```

Example 4 (unknown):
```unknown
/acp doctor
```

---

## OpenResponses API

**URL:** https://docs.openclaw.ai/gateway/openresponses-http-api

**Contents:**
- OpenResponses API
- Documentation Index
- ​Authentication, security, and routing
- ​Session behavior
- ​Request shape (supported)
- ​Items (input)
  - ​message
  - ​function_call_output (turn-based tools)
  - ​reasoning and item_reference
- ​Tools (client-side function tools)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "type": "function_call_output",
  "call_id": "call_123",
  "output": "{\"temperature\": \"72F\"}"
}
```

Example 2 (json):
```json
{
  "type": "input_image",
  "source": { "type": "url", "url": "https://example.com/image.png" }
}
```

Example 3 (json):
```json
{
  "type": "input_file",
  "source": {
    "type": "base64",
    "media_type": "text/plain",
    "data": "SGVsbG8gV29ybGQh",
    "filename": "hello.txt"
  }
}
```

Example 4 (json):
```json
{
  gateway: {
    http: {
      endpoints: {
        responses: {
          enabled: true,
          maxBodyBytes: 20000000,
          maxUrlParts: 8,
          files: {
            allowUrl: true,
            urlAllowlist: ["cdn.example.com", "*.assets.example.com"],
            allowedMimes: [
              "text/plain",
              "text/markdown",
              "text/html",
              "text/csv",
              "application/json",
              "application/pdf",
            ],
            maxBytes: 5242880,
            maxChars: 200000,
            maxRedirects: 3,
            timeoutMs: 10000,
            pdf: {
              maxPages: 4,
              maxPixels: 4000000,
              minTextChars: 200,
            },
          },
          images: {
            allowUrl: true,
            urlAllowlist: ["images.example.com"],
            allowedMimes: [
              "image/jpeg",
              "image/png",
              "image/gif",
              "image/webp",
              "image/heic",
              "image/heif",
            ],
            maxBytes: 10485760,
            maxRedirects: 3,
            timeoutMs: 10000,
          },
        },
      },
    },
  },
}
```

---

## Sub-agents

**URL:** https://docs.openclaw.ai/tools/subagents

**Contents:**
- Sub-agents
- Documentation Index
- ​Slash command
  - ​Thread binding controls
  - ​Spawn behavior
- ​Context modes
- ​Tool: sessions_spawn
  - ​Tool parameters
- ​Thread-bound sessions
  - ​Thread supporting channels

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Non-blocking, push-based completion

Manual-spawn delivery resilience

Completion handoff metadata

Modes and ACP runtime

**Examples:**

Example 1 (typescript):
```typescript
/subagents list
/subagents kill <id|#|all>
/subagents log <id|#> [limit] [tools]
/subagents info <id|#>
/subagents send <id|#> <message>
/subagents steer <id|#> <message>
/subagents spawn <agentId> <task> [--model <model>] [--thinking <level>]
```

Example 2 (unknown):
```unknown
/focus <subagent-label|session-key|session-id|session-label>
/unfocus
/agents
/session idle <duration|off>
/session max-age <duration|off>
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      subagents: {
        maxSpawnDepth: 2, // allow sub-agents to spawn children (default: 1)
        maxChildrenPerAgent: 5, // max active children per agent session (default: 5)
        maxConcurrent: 8, // global concurrency lane cap (default: 8)
        runTimeoutSeconds: 900, // default timeout for sessions_spawn when omitted (0 = no timeout)
      },
    },
  },
}
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      subagents: {
        maxConcurrent: 1,
      },
    },
  },
  tools: {
    subagents: {
      tools: {
        // deny wins
        deny: ["gateway", "cron"],
        // if allow is set, it becomes allow-only (deny still wins)
        // allow: ["read", "exec", "process"]
      },
    },
  },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/nl

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Aan de slag
- Onboarding uitvoeren
- De Control UI openen
- ​Wat is OpenClaw?
- ​Hoe het werkt
- ​Belangrijkste mogelijkheden
- Multikanaal-Gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway voor elk besturingssysteem voor AI-agents via Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo en meer. Stuur een bericht en ontvang een agentreactie vanuit je broekzak. Draai één Gateway voor ingebouwde kanalen, meegeleverde kanaal-plugins, WebChat en mobiele nodes.

Onboarden en de service installeren

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## GPT-5.5 / Codex parity maintainer notes

**URL:** https://docs.openclaw.ai/help/gpt55-codex-agentic-parity-maintainers

**Contents:**
- GPT-5.5 / Codex parity maintainer notes
- Documentation Index
- ​Merge units
  - ​PR A: strict-agentic execution
  - ​PR B: runtime truthfulness
  - ​PR C: execution correctness
  - ​PR D: parity harness
- ​Mapping back to the original six contracts
- ​Review order
- ​What to look for

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OpenAI chat completions

**URL:** https://docs.openclaw.ai/gateway/openai-http-api

**Contents:**
- OpenAI chat completions
- Documentation Index
- ​Authentication
- ​Security boundary (important)
- ​Agent-first model contract
- ​Enabling the endpoint
- ​Disabling the endpoint
- ​Session behavior
- ​Why this surface matters
- ​Model list and agent routing

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

What does `/v1/models` return?

Does `/v1/models` list agents or sub-agents?

Why is `openclaw/default` included?

How do I override the backend model?

How do embeddings fit this contract?

**Examples:**

Example 1 (json):
```json
{
  gateway: {
    http: {
      endpoints: {
        chatCompletions: { enabled: true },
      },
    },
  },
}
```

Example 2 (json):
```json
{
  gateway: {
    http: {
      endpoints: {
        chatCompletions: { enabled: false },
      },
    },
  },
}
```

Example 3 (json):
```json
curl -sS http://127.0.0.1:18789/v1/models \
  -H 'Authorization: Bearer YOUR_TOKEN'
```

Example 4 (json):
```json
curl -sS http://127.0.0.1:18789/v1/chat/completions \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "openclaw/default",
    "messages": [{"role":"user","content":"hi"}]
  }'
```

---

## OpenAI

**URL:** https://docs.openclaw.ai/providers/openai

**Contents:**
- OpenAI
- Documentation Index
- ​Quick choice
- ​Naming map
- ​OpenClaw feature coverage
- ​Memory embeddings
- ​Getting started
  - ​Route summary
  - ​Config example
  - ​Route summary

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Use the native Codex runtime

Verify Codex auth is available

Speech synthesis (TTS)

Realtime transcription

Transport (WebSocket vs SSE)

Priority processing (service_tier)

Server-side compaction (Responses API)

Strict-agentic GPT mode

Native vs OpenAI-compatible routes

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
        model: "text-embedding-3-small",
      },
    },
  },
}
```

Example 2 (unknown):
```unknown
openclaw onboard --auth-choice openai-api-key
```

Example 3 (bash):
```bash
openclaw onboard --openai-api-key "$OPENAI_API_KEY"
```

Example 4 (unknown):
```unknown
openclaw models list --provider openai
```

---

## Broadcast groups

**URL:** https://docs.openclaw.ai/channels/broadcast-groups

**Contents:**
- Broadcast groups
- Documentation Index
- ​Overview
- ​Use cases
- ​Configuration
  - ​Basic setup
  - ​Processing strategy
  - ​Complete example
- ​How it works
  - ​Message flow

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

1. Specialized agent teams

2. Multi-language support

3. Quality assurance workflows

Incoming message arrives

If not in broadcast list

1. Keep agents focused

2. Use descriptive names

3. Configure different tool access

4. Monitor performance

5. Handle failures gracefully

Agents not responding

Only one agent responding

Example 1: Code review team

Example 2: Multi-language support

**Examples:**

Example 1 (yaml):
```yaml
Group: "Development Team"
Agents:
  - CodeReviewer (reviews code snippets)
  - DocumentationBot (generates docs)
  - SecurityAuditor (checks for vulnerabilities)
  - TestGenerator (suggests test cases)
```

Example 2 (yaml):
```yaml
Group: "International Support"
Agents:
  - Agent_EN (responds in English)
  - Agent_DE (responds in German)
  - Agent_ES (responds in Spanish)
```

Example 3 (yaml):
```yaml
Group: "Customer Support"
Agents:
  - SupportAgent (provides answer)
  - QAAgent (reviews quality, only responds if issues found)
```

Example 4 (yaml):
```yaml
Group: "Project Management"
Agents:
  - TaskTracker (updates task database)
  - TimeLogger (logs time spent)
  - ReportGenerator (creates summaries)
```

---

## Agents

**URL:** https://docs.openclaw.ai/cli/agents

**Contents:**
- Agents
- Documentation Index
- ​openclaw agents
- ​Examples
- ​Routing bindings
  - ​Binding scope behavior
- ​Command surface
  - ​agents
  - ​agents list
  - ​agents add [name]

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (powershell):
```powershell
openclaw agents list
openclaw agents list --bindings
openclaw agents add work --workspace ~/.openclaw/workspace-work
openclaw agents add ops --workspace ~/.openclaw/workspace-ops --bind telegram:ops --non-interactive
openclaw agents bindings
openclaw agents bind --agent work --bind telegram:ops
openclaw agents unbind --agent work --bind telegram:ops
openclaw agents set-identity --workspace ~/.openclaw/workspace --from-identity
openclaw agents set-identity --agent main --avatar avatars/openclaw.png
openclaw agents delete work
```

Example 2 (unknown):
```unknown
openclaw agents bindings
openclaw agents bindings --agent work
openclaw agents bindings --json
```

Example 3 (unknown):
```unknown
openclaw agents bind --agent work --bind telegram:ops --bind discord:guild-a
```

Example 4 (markdown):
```markdown
# initial channel-only binding
openclaw agents bind --agent work --bind telegram

# later upgrade to account-scoped binding
openclaw agents bind --agent work --bind telegram:ops
```

---

## Agent send

**URL:** https://docs.openclaw.ai/tools/agent-send

**Contents:**
- Agent send
- Documentation Index
- ​Quick start
- ​Flags
- ​Behavior
- ​Examples
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Run a simple agent turn

Target a specific agent or session

Deliver the reply to a channel

**Examples:**

Example 1 (unknown):
```unknown
openclaw agent --message "What is the weather today?"
```

Example 2 (sass):
```sass
# Target a specific agent
openclaw agent --agent ops --message "Summarize logs"

# Target a phone number (derives session key)
openclaw agent --to +15555550123 --message "Status update"

# Reuse an existing session
openclaw agent --session-id abc123 --message "Continue the task"
```

Example 3 (sass):
```sass
# Deliver to WhatsApp (default channel)
openclaw agent --to +15555550123 --message "Report ready" --deliver

# Deliver to Slack
openclaw agent --agent ops --message "Generate report" \
  --deliver --reply-channel slack --reply-to "#reports"
```

Example 4 (sass):
```sass
# Simple turn with JSON output
openclaw agent --to +15555550123 --message "Trace logs" --verbose on --json

# Turn with thinking level
openclaw agent --session-id 1234 --message "Summarize inbox" --thinking medium

# Deliver to a different channel than the session
openclaw agent --agent ops --message "Alert" --deliver --reply-channel telegram --reply-to "@admin"
```

---

## OpenClaw App SDK API design

**URL:** https://docs.openclaw.ai/reference/openclaw-sdk-api-design

**Contents:**
- OpenClaw App SDK API design
- Documentation Index
- ​Namespace design
- ​Event contract
- ​Result contract
- ​Approvals and questions
- ​ToolSpace model
- ​Artifact model
- ​Security model
- ​Managed environment provider

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
oc.agents.list();
oc.agents.get("main");
oc.agents.create(...);
oc.agents.update(...);

oc.sessions.list();
oc.sessions.create(...);
oc.sessions.resolve(...);
oc.sessions.send(...);
oc.sessions.messages(...);
oc.sessions.fork(...);
oc.sessions.compact(...);
oc.sessions.abort(...);

oc.runs.create(...);
oc.runs.get(runId);
oc.runs.events(runId, { after });
oc.runs.wait(runId);
oc.runs.cancel(runId);

oc.tasks.list(); // future API: current SDK throws unsupported
oc.tasks.get(taskId); // future API: current SDK throws unsupported
oc.tasks.cancel(taskId); // future API: current SDK throws unsupported
oc.tasks.events(taskId, { after }); // future API

oc.models.list();
oc.models.status(); // Gateway models.authStatus

oc.tools.list();
oc.tools.invoke(...); // future API: current SDK throws unsupported

oc.artifacts.list({ runId }); // future API: current SDK throws unsupported
oc.artifacts.get(artifactId); // future API: current SDK throws unsupported
oc.artifacts.download(artifactId); // future API: current SDK throws unsupported

oc.approvals.list();
oc.approvals.respond(approvalId, ...);

oc.environments.list(); // future API: current SDK throws unsupported
oc.environments.create(...); // future API: current SDK throws unsupported
oc.environments.status(environmentId); // future API: current SDK throws unsupported
oc.environments.delete(environmentId); // future API: current SDK throws unsupported
```

Example 2 (swift):
```swift
const run = await agent.run(inputOrParams);
await run.cancel();
await run.wait();

for await (const event of run.events()) {
  // normalized event stream
}

const artifacts = await run.artifacts.list();
const session = await run.session();
```

Example 3 (typescript):
```typescript
type OpenClawEvent = {
  version: 1;
  id: string;
  ts: number;
  type: OpenClawEventType;
  runId?: string;
  sessionId?: string;
  sessionKey?: string;
  taskId?: string;
  agentId?: string;
  data: unknown;
  raw?: unknown;
};
```

Example 4 (typescript):
```typescript
type RunResult = {
  runId: string;
  status: "accepted" | "completed" | "failed" | "cancelled" | "timed_out";
  sessionId?: string;
  sessionKey?: string;
  taskId?: string;
  startedAt?: string | number;
  endedAt?: string | number;
  output?: {
    text?: string;
    messages?: SDKMessage[];
  };
  usage?: {
    inputTokens?: number;
    outputTokens?: number;
    totalTokens?: number;
    costUsd?: number;
  };
  artifacts?: ArtifactSummary[];
  error?: SDKError;
};
```

---

## Deepinfra

**URL:** https://docs.openclaw.ai/providers/deepinfra

**Contents:**
- Deepinfra
- Documentation Index
- ​DeepInfra
- ​Getting an API key
- ​CLI setup
- ​Config snippet
- ​Supported OpenClaw surfaces
- ​Available models
- ​Notes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw onboard --deepinfra-api-key <key>
```

Example 2 (unknown):
```unknown
export DEEPINFRA_API_KEY="<your-deepinfra-api-key>" # pragma: allowlist secret
```

Example 3 (json):
```json
{
  env: { DEEPINFRA_API_KEY: "<your-deepinfra-api-key>" }, // pragma: allowlist secret
  agents: {
    defaults: {
      model: { primary: "deepinfra/deepseek-ai/DeepSeek-V3.2" },
    },
  },
}
```

Example 4 (lua):
```lua
deepinfra/MiniMaxAI/MiniMax-M2.5
deepinfra/deepseek-ai/DeepSeek-V3.2
deepinfra/moonshotai/Kimi-K2.5
deepinfra/zai-org/GLM-5.1
...and many more
```

---

## Multi-agent sandbox and tools

**URL:** https://docs.openclaw.ai/tools/multi-agent-sandbox-tools

**Contents:**
- Multi-agent sandbox and tools
- Documentation Index
- Sandboxing
- Sandbox vs tool policy vs elevated
- Elevated mode
- ​Configuration examples
- ​Configuration precedence
  - ​Sandbox config
  - ​Tool restrictions
- ​Migration from single agent

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Example 1: Personal + restricted family agent

Example 2: Work agent with shared sandbox

Example 2b: Global coding profile + messaging-only agent

Example 3: Different sandbox modes per agent

Provider tool profile

Agent-specific tool policy

Agent provider policy

Empty allowlist behavior

Check agent resolution

Verify sandbox containers

Test tool restrictions

Agent not sandboxed despite `mode: 'all'`

Tools still available despite deny list

Container not isolated per agent

**Examples:**

Example 1 (json):
```json
{
  "agents": {
    "list": [
      {
        "id": "main",
        "default": true,
        "name": "Personal Assistant",
        "workspace": "~/.openclaw/workspace",
        "sandbox": { "mode": "off" }
      },
      {
        "id": "family",
        "name": "Family Bot",
        "workspace": "~/.openclaw/workspace-family",
        "sandbox": {
          "mode": "all",
          "scope": "agent"
        },
        "tools": {
          "allow": ["read"],
          "deny": ["exec", "write", "edit", "apply_patch", "process", "browser"]
        }
      }
    ]
  },
  "bindings": [
    {
      "agentId": "family",
      "match": {
        "provider": "whatsapp",
        "accountId": "*",
        "peer": {
          "kind": "group",
          "id": "120363424282127706@g.us"
        }
      }
    }
  ]
}
```

Example 2 (json):
```json
{
  "agents": {
    "list": [
      {
        "id": "personal",
        "workspace": "~/.openclaw/workspace-personal",
        "sandbox": { "mode": "off" }
      },
      {
        "id": "work",
        "workspace": "~/.openclaw/workspace-work",
        "sandbox": {
          "mode": "all",
          "scope": "shared",
          "workspaceRoot": "/tmp/work-sandboxes"
        },
        "tools": {
          "allow": ["read", "write", "apply_patch", "exec"],
          "deny": ["browser", "gateway", "discord"]
        }
      }
    ]
  }
}
```

Example 3 (json):
```json
{
  "tools": { "profile": "coding" },
  "agents": {
    "list": [
      {
        "id": "support",
        "tools": { "profile": "messaging", "allow": ["slack"] }
      }
    ]
  }
}
```

Example 4 (json):
```json
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "non-main",
        "scope": "session"
      }
    },
    "list": [
      {
        "id": "main",
        "workspace": "~/.openclaw/workspace",
        "sandbox": {
          "mode": "off"
        }
      },
      {
        "id": "public",
        "workspace": "~/.openclaw/workspace-public",
        "sandbox": {
          "mode": "all",
          "scope": "agent"
        },
        "tools": {
          "allow": ["read"],
          "deny": ["exec", "write", "edit", "apply_patch"]
        }
      }
    ]
  }
}
```

---

## CI pipeline

**URL:** https://docs.openclaw.ai/ci

**Contents:**
- CI pipeline
- Documentation Index
- ​Pipeline overview
- ​Fail-fast order
- ​Scope and routing
- ​ClawSweeper activity forwarding
- ​Manual dispatches
- ​Runners
- ​Local equivalents
- ​OpenClaw Performance

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
gh workflow run ci.yml --ref release/YYYY.M.D
gh workflow run ci.yml --ref main -f target_ref=<branch-or-sha> -f include_android=true
gh workflow run full-release-validation.yml --ref main -f ref=<branch-or-sha>
```

Example 2 (lua):
```lua
pnpm changed:lanes                            # inspect the local changed-lane classifier for origin/main...HEAD
pnpm check:changed                            # smart local check gate: changed typecheck/lint/guards by boundary lane
pnpm check                                    # fast local gate: prod tsgo + sharded lint + parallel fast guards
pnpm check:test-types
pnpm check:timed                              # same gate with per-stage timings
pnpm build:strict-smoke
pnpm check:architecture
pnpm test:gateway:watch-regression
pnpm test                                     # vitest tests
pnpm test:changed                             # cheap smart changed Vitest targets
pnpm test:channels
pnpm test:contracts:channels
pnpm check:docs                               # docs format + lint + broken links
pnpm build                                    # build dist when CI artifact/build-smoke lanes matter
pnpm ci:timings                               # summarize the latest origin/main push CI run
pnpm ci:timings:recent                        # compare recent successful main CI runs
node scripts/ci-run-timings.mjs <run-id>      # summarize wall time, queue time, and slowest jobs
node scripts/ci-run-timings.mjs --latest-main # ignore issue/comment noise and choose origin/main push CI
node scripts/ci-run-timings.mjs --recent 10   # compare recent successful main CI runs
pnpm test:perf:groups --full-suite --allow-failures --output .artifacts/test-perf/baseline-before.json
pnpm test:perf:groups:compare .artifacts/test-perf/baseline-before.json .artifacts/test-perf/after-agent.json
pnpm perf:kova:summary --report .artifacts/kova/reports/mock-provider/report.json --output .artifacts/kova/summary.md
```

Example 3 (sass):
```sass
gh workflow run openclaw-performance.yml --ref main -f profile=diagnostic -f repeat=3
gh workflow run openclaw-performance.yml --ref main -f profile=smoke -f repeat=1 -f deep_profile=true -f live_gpt54=true
gh workflow run openclaw-performance.yml --ref main -f target_ref=v2026.5.2 -f profile=diagnostic -f repeat=3
```

Example 4 (sass):
```sass
gh workflow run openclaw-release-publish.yml \
  --ref release/YYYY.M.D \
  -f tag=vYYYY.M.D-beta.N \
  -f preflight_run_id=<successful-openclaw-npm-preflight-run-id> \
  -f npm_dist_tag=beta
```

---

## Agent harness plugins

**URL:** https://docs.openclaw.ai/plugins/sdk-agent-harness

**Contents:**
- Agent harness plugins
- Documentation Index
- ​When to use a harness
- ​What core still owns
- ​Register a harness
- ​Selection policy
- ​Provider plus harness pairing
  - ​Tool-result middleware
  - ​Terminal outcome classification
  - ​Native Codex harness mode

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (swift):
```swift
import type { AgentHarness } from "openclaw/plugin-sdk/agent-harness";
import { definePluginEntry } from "openclaw/plugin-sdk/plugin-entry";

const myHarness: AgentHarness = {
  id: "my-harness",
  label: "My native agent harness",

  supports(ctx) {
    return ctx.provider === "my-provider"
      ? { supported: true, priority: 100 }
      : { supported: false };
  },

  async runAttempt(params) {
    // Start or resume your native thread.
    // Use params.prompt, params.tools, params.images, params.onPartialReply,
    // params.onAgentEvent, and the other prepared attempt fields.
    return await runMyNativeTurn(params);
  },
};

export default definePluginEntry({
  id: "my-native-agent",
  name: "My Native Agent",
  description: "Runs selected models through a native agent daemon.",
  register(api) {
    api.registerAgentHarness(myHarness);
  },
});
```

Example 2 (json):
```json
{
  "agents": {
    "defaults": {
      "model": "openai/gpt-5.5",
      "agentRuntime": {
        "id": "codex"
      }
    }
  }
}
```

Example 3 (json):
```json
{
  "agents": {
    "defaults": {
      "agentRuntime": {
        "id": "auto"
      }
    }
  }
}
```

Example 4 (json):
```json
{
  "agents": {
    "defaults": {
      "agentRuntime": { "id": "auto" }
    },
    "list": [
      {
        "id": "codex-only",
        "model": "openai/gpt-5.5",
        "agentRuntime": { "id": "codex" }
      }
    ]
  }
}
```

---

## Plugin runtime helpers

**URL:** https://docs.openclaw.ai/plugins/sdk-runtime

**Contents:**
- Plugin runtime helpers
- Documentation Index
- Channel plugins
- Provider plugins
- ​Config Loading And Writes
- ​Runtime namespaces
- ​Storing runtime references
- ​Other top-level api fields
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

api.runtime.agent.defaults

api.runtime.tasks.managedFlows

api.runtime.mediaUnderstanding

api.runtime.imageGeneration

api.runtime.webSearch

api.runtime.modelAuth

Wire into the entry point

Access from other files

**Examples:**

Example 1 (javascript):
```javascript
register(api) {
  const runtime = api.runtime;
}
```

Example 2 (javascript):
```javascript
// Resolve the agent's working directory
const agentDir = api.runtime.agent.resolveAgentDir(cfg);

// Resolve agent workspace
const workspaceDir = api.runtime.agent.resolveAgentWorkspaceDir(cfg);

// Get agent identity
const identity = api.runtime.agent.resolveAgentIdentity(cfg);

// Get default thinking level
const thinking = api.runtime.agent.resolveThinkingDefault({
  cfg,
  provider,
  model,
});

// Validate a user-provided thinking level against the active provider profile
const policy = api.runtime.agent.resolveThinkingPolicy({ provider, model });
const level = api.runtime.agent.normalizeThinkingLevel("extra high");
if (level && policy.levels.some((entry) => entry.id === level)) {
  // pass level to an embedded run
}

// Get agent timeout
const timeoutMs = api.runtime.agent.resolveAgentTimeoutMs(cfg);

// Ensure workspace exists
await api.runtime.agent.ensureAgentWorkspace(cfg);

// Run an embedded agent turn
const agentDir = api.runtime.agent.resolveAgentDir(cfg);
const result = await api.runtime.agent.runEmbeddedAgent({
  sessionId: "my-plugin:task-1",
  runId: crypto.randomUUID(),
  sessionFile: path.join(agentDir, "sessions", "my-plugin-task-1.jsonl"),
  workspaceDir: api.runtime.agent.resolveAgentWorkspaceDir(cfg),
  prompt: "Summarize the latest changes",
  timeoutMs: api.runtime.agent.resolveAgentTimeoutMs(cfg),
});
```

Example 3 (javascript):
```javascript
const storePath = api.runtime.agent.session.resolveStorePath(cfg);
const store = api.runtime.agent.session.loadSessionStore(storePath);
await api.runtime.agent.session.updateSessionStore(storePath, (nextStore) => {
  // Patch one entry without replacing the whole file from stale state.
  nextStore[sessionKey] = { ...nextStore[sessionKey], thinkingLevel: "high" };
});
const filePath = api.runtime.agent.session.resolveSessionFilePath(cfg, sessionId);
```

Example 4 (javascript):
```javascript
const model = api.runtime.agent.defaults.model; // e.g. "anthropic/claude-sonnet-4-6"
const provider = api.runtime.agent.defaults.provider; // e.g. "anthropic"
```

---

## GPT-5.5 / Codex agentic parity

**URL:** https://docs.openclaw.ai/help/gpt55-codex-agentic-parity

**Contents:**
- GPT-5.5 / Codex agentic parity
- Documentation Index
- ​GPT-5.5 / Codex Agentic Parity in OpenClaw
- ​What changed
  - ​PR A: strict-agentic execution
  - ​PR B: runtime truthfulness
  - ​PR C: execution correctness
  - ​PR D: parity harness
- ​Why this improves GPT-5.5 in practice
- ​Before vs after for GPT-5.5 users

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm openclaw qa parity-report \
  --repo-root . \
  --candidate-summary .artifacts/qa-e2e/gpt55/qa-suite-summary.json \
  --baseline-summary .artifacts/qa-e2e/opus46/qa-suite-summary.json \
  --output-dir .artifacts/qa-e2e/parity
```

---

## Scheduled tasks

**URL:** https://docs.openclaw.ai/automation/webhook

**Contents:**
- Scheduled tasks
- Documentation Index
- ​Quick start
- ​How cron works
- ​Schedule types
  - ​Day-of-month and day-of-week use OR logic
- ​Execution styles
  - ​Payload options for isolated jobs
- ​Delivery and output
- ​CLI examples

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Add a one-shot reminder

Main session vs isolated vs custom

What 'fresh session' means for isolated jobs

Subagent and Discord delivery

Mapped hooks (POST /hooks/<name>)

Select the GCP project

Create topic and grant Gmail push access

Cron fired but no delivery

Cron or heartbeat appears to prevent /new-style rollover

**Examples:**

Example 1 (json):
```json
openclaw cron add \
  --name "Reminder" \
  --at "2026-02-01T16:00:00Z" \
  --session main \
  --system-event "Reminder: check the cron docs draft" \
  --wake now \
  --delete-after-run
```

Example 2 (unknown):
```unknown
openclaw cron list
openclaw cron show <job-id>
```

Example 3 (unknown):
```unknown
openclaw cron runs --id <job-id>
```

Example 4 (markdown):
```markdown
# Intended: "9 AM on the 15th, only if it's a Monday"
# Actual:   "9 AM on every 15th, AND 9 AM on every Monday"
0 9 15 * 1
```

---

## Anthropic plugin

**URL:** https://docs.openclaw.ai/plugins/reference/anthropic

**Contents:**
- Anthropic plugin
- Documentation Index
- ​Anthropic plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Anthropic Vertex plugin

**URL:** https://docs.openclaw.ai/plugins/reference/anthropic-vertex

**Contents:**
- Anthropic Vertex plugin
- Documentation Index
- ​Anthropic Vertex plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Copilot Proxy plugin

**URL:** https://docs.openclaw.ai/plugins/reference/copilot-proxy

**Contents:**
- Copilot Proxy plugin
- Documentation Index
- ​Copilot Proxy plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## DeepInfra plugin

**URL:** https://docs.openclaw.ai/plugins/reference/deepinfra

**Contents:**
- DeepInfra plugin
- Documentation Index
- ​DeepInfra plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## GitHub Copilot plugin

**URL:** https://docs.openclaw.ai/plugins/reference/github-copilot

**Contents:**
- GitHub Copilot plugin
- Documentation Index
- ​GitHub Copilot plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Raspberry Pi

**URL:** https://docs.openclaw.ai/platforms/raspberry-pi

**Contents:**
- Raspberry Pi
- Documentation Index
- ​Prerequisites
- ​Setup
- ​Performance tips
- ​Troubleshooting
- ​Next steps
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Add swap (important for 2 GB or less)

Access the Control UI

**Examples:**

Example 1 (elixir):
```elixir
ssh user@gateway-host
```

Example 2 (sql):
```sql
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl build-essential

# Set timezone (important for cron and reminders)
sudo timedatectl set-timezone America/Chicago
```

Example 3 (unknown):
```unknown
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install -y nodejs
node --version
```

Example 4 (sass):
```sass
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Reduce swappiness for low-RAM devices
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---
