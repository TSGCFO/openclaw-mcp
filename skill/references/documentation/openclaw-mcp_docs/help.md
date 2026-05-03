# Openclaw-Mcp_Docs - Help

**Pages:** 7

---

## Scripts

**URL:** https://docs.openclaw.ai/help/scripts

**Contents:**
- Scripts
- Documentation Index
- ​Conventions
- ​Auth monitoring scripts
- ​GitHub read helper
- ​When adding scripts
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Environment variables

**URL:** https://docs.openclaw.ai/help/environment

**Contents:**
- Environment variables
- Documentation Index
- ​Precedence (highest → lowest)
- ​Config env block
- ​Shell env import
- ​Runtime-injected env vars
- ​UI env vars
- ​Env var substitution in config
- ​Secret refs vs ${ENV} strings
- ​Path-related env vars

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
{
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: {
      GROQ_API_KEY: "gsk-...",
    },
  },
}
```

Example 2 (json):
```json
{
  env: {
    shellEnv: {
      enabled: true,
      timeoutMs: 15000,
    },
  },
}
```

Example 3 (json):
```json
{
  models: {
    providers: {
      "vercel-gateway": {
        apiKey: "${VERCEL_GATEWAY_API_KEY}",
      },
    },
  },
}
```

Example 4 (typescript):
```typescript
<key>EnvironmentVariables</key>
<dict>
  <key>OPENCLAW_HOME</key>
  <string>/Users/user</string>
</dict>
```

---

## Testing

**URL:** https://docs.openclaw.ai/help/testing

**Contents:**
- Testing
- Documentation Index
- ​Quick start
- ​QA-specific runners
  - ​Shared Telegram credentials via Convex (v1)
  - ​Adding a channel to QA
- ​Test suites (what runs where)
  - ​Unit / integration (default)
  - ​Stability (gateway)
  - ​E2E (gateway smoke)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Projects, shards, and scoped lanes

Embedded runner coverage

Vitest pool and isolation defaults

**Examples:**

Example 1 (sass):
```sass
gh workflow run package-acceptance.yml --ref main \
  -f source=npm \
  -f package_spec=openclaw@beta \
  -f suite_profile=product \
  -f telegram_mode=mock-openai
```

Example 2 (sass):
```sass
gh workflow run package-acceptance.yml --ref main \
  -f source=url \
  -f package_url=https://registry.npmjs.org/openclaw/-/openclaw-VERSION.tgz \
  -f package_sha256=<sha256> \
  -f suite_profile=package
```

Example 3 (sass):
```sass
gh workflow run package-acceptance.yml --ref main \
  -f source=artifact \
  -f artifact_run_id=<run-id> \
  -f artifact_name=<artifact-name> \
  -f suite_profile=smoke
```

Example 4 (sql):
```sql
timeout --foreground 150m pnpm test:parallels:npm-update -- --json
timeout --foreground 90m pnpm test:parallels:npm-update -- --platform windows --json
```

---

## Testing: live suites

**URL:** https://docs.openclaw.ai/help/testing-live

**Contents:**
- Testing: live suites
- Documentation Index
- ​Live: local profile smoke commands
- ​Live: Android node capability sweep
- ​Live: model smoke (profile keys)
  - ​Layer 1: Direct model completion (no gateway)
  - ​Layer 2: Gateway + dev agent smoke (what “@openclaw” actually does)
- ​Live: CLI backend smoke (Claude, Codex, Gemini, or other local CLIs)
- ​Live: ACP bind smoke (/acp spawn ... --bind here)
- ​Live: Codex app-server harness smoke

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
source ~/.profile
```

Example 2 (unknown):
```unknown
pnpm openclaw infer tts convert --local --json \
  --text "OpenClaw live smoke." \
  --output /tmp/openclaw-live-smoke.mp3
```

Example 3 (sass):
```sass
pnpm openclaw voicecall setup --json
pnpm openclaw voicecall smoke --to "+15555550123"
```

Example 4 (unknown):
```unknown
openclaw models list
openclaw models list --json
```

---

## Help

**URL:** https://docs.openclaw.ai/help

**Contents:**
- Help
- Documentation Index
- ​FAQ
- ​Diagnostics
- ​Testing
- ​Community and meta

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## FAQ

**URL:** https://docs.openclaw.ai/help/faq

**Contents:**
- FAQ
- Documentation Index
- ​First 60 seconds if something is broken
- ​Quick start and first-run setup
- ​What is OpenClaw?
- ​Skills and automation
- ​Sandboxing and memory
- ​Where things live on disk
- ​Config basics
- ​Remote gateways and nodes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

What is OpenClaw, in one paragraph?

I just set it up - what should I do first?

What are the top five everyday use cases for OpenClaw?

Can OpenClaw help with lead gen, outreach, ads, and blogs for a SaaS?

What are the advantages vs Claude Code for web development?

How do I customize skills without keeping the repo dirty?

Can I load skills from a custom folder?

How can I use different models for different tasks?

The bot freezes while doing heavy work. How do I offload that?

How do thread-bound subagent sessions work on Discord?

A subagent finished, but the completion update went to the wrong place or never posted. What should I check?

Cron or reminders do not fire. What should I check?

Cron fired, but nothing was sent to the channel. Why?

Why did an isolated cron run switch models or retry once?

How do I install skills on Linux?

Can OpenClaw run tasks on a schedule or continuously in the background?

Can I run Apple macOS-only skills from Linux?

Do you have a Notion or HeyGen integration?

How do I use my existing signed-in Chrome with OpenClaw?

Is there a dedicated sandboxing doc?

Docker feels limited - how do I enable full features?

Can I keep DMs personal but make groups public/sandboxed with one agent?

How do I bind a host folder into the sandbox?

How does memory work?

Memory keeps forgetting things. How do I make it stick?

Does memory persist forever? What are the limits?

Does semantic memory search require an OpenAI API key?

Is all data used with OpenClaw saved locally?

Where does OpenClaw store its data?

Where should AGENTS.md / SOUL.md / USER.md / MEMORY.md live?

Recommended backup strategy

How do I completely uninstall OpenClaw?

Can agents work outside the workspace?

Remote mode: where is the session store?

What format is the config? Where is it?

I set gateway.bind: "lan" (or "tailnet") and now nothing listens / the UI says unauthorized

Why do I need a token on localhost now?

Do I have to restart after changing config?

How do I disable funny CLI taglines?

How do I enable web search (and web fetch)?

config.apply wiped my config. How do I recover and avoid this?

How do I run a central Gateway with specialized workers across devices?

Can the OpenClaw browser run headless?

How do I use Brave for browser control?

How do commands propagate between Telegram, the gateway, and nodes?

How can my agent access my computer if the Gateway is hosted remotely?

Tailscale is connected but I get no replies. What now?

Can two OpenClaw instances talk to each other (local + VPS)?

Do I need separate VPSes for multiple agents?

Is there a benefit to using a node on my personal laptop instead of SSH from a VPS?

Do nodes run a gateway service?

Is there an API / RPC way to apply config?

Minimal sane config for a first install

How do I set up Tailscale on a VPS and connect from my Mac?

How do I connect a Mac node to a remote Gateway (Tailscale Serve)?

Should I install on a second laptop or just add a node?

How does OpenClaw load environment variables?

I started the Gateway via the service and my env vars disappeared. What now?

I set COPILOT_GITHUB_TOKEN, but models status shows "Shell env: off." Why?

How do I start a fresh conversation?

Do sessions reset automatically if I never send /new?

Is there a way to make a team of OpenClaw instances (one CEO and many agents)?

Why did context get truncated mid-task? How do I prevent it?

How do I completely reset OpenClaw but keep it installed?

I am getting "context too large" errors - how do I reset or compact?

Why am I seeing "LLM request rejected: messages.content.tool_use.input field required"?

Why am I getting heartbeat messages every 30 minutes?

Do I need to add a "bot account" to a WhatsApp group?

How do I get the JID of a WhatsApp group?

Why does OpenClaw not reply in a group?

Do groups/threads share context with DMs?

How many workspaces and agents can I create?

Can I run multiple bots or chats at the same time (Slack), and how should I set that up?

What port does the Gateway use?

Why does openclaw gateway status say "Runtime: running" but "Connectivity probe: failed"?

Why does openclaw gateway status show "Config (cli)" and "Config (service)" different?

What does "another gateway instance is already listening" mean?

How do I run OpenClaw in remote mode (client connects to a Gateway elsewhere)?

The Control UI says "unauthorized" (or keeps reconnecting). What now?

I set gateway.bind tailnet but it cannot bind and nothing listens

Can I run multiple Gateways on the same host?

What does "invalid handshake" / code 1008 mean?

How do I start/stop/restart the Gateway service?

I closed my terminal on Windows - how do I restart OpenClaw?

The Gateway is up but replies never arrive. What should I check?

"Disconnected from gateway: no reason" - what now?

Telegram setMyCommands fails. What should I check?

TUI shows no output. What should I check?

How do I completely stop then start the Gateway?

ELI5: openclaw gateway restart vs openclaw gateway

Fastest way to get more details when something fails

My skill generated an image/PDF, but nothing was sent

Is it safe to expose OpenClaw to inbound DMs?

Is prompt injection only a concern for public bots?

Should my bot have its own email, GitHub account, or phone number?

Can I give it autonomy over my text messages and is that safe?

Can I use cheaper models for personal assistant tasks?

I ran /start in Telegram but did not get a pairing code

WhatsApp: will it message my contacts? How does pairing work?

How do I stop internal system messages from showing in chat?

How do I stop/cancel a running task?

How do I send a Discord message from Telegram? ("Cross-context messaging denied")

Why does it feel like the bot "ignores" rapid-fire messages?

What is the default model for Anthropic with an API key?

**Examples:**

Example 1 (unknown):
```unknown
openclaw status
```

Example 2 (unknown):
```unknown
openclaw status --all
```

Example 3 (unknown):
```unknown
openclaw gateway status
```

Example 4 (unknown):
```unknown
openclaw status --deep
```

---

## Debugging

**URL:** https://docs.openclaw.ai/help/debugging

**Contents:**
- Debugging
- Documentation Index
- ​Runtime debug overrides
- ​Session trace output
- ​Plugin lifecycle trace
- ​CLI startup and command profiling
- ​Gateway watch mode
- ​Dev profile + dev gateway (—dev)
- ​Raw stream logging (OpenClaw)
- ​Raw chunk logging (pi-mono)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
/debug show
/debug set messages.responsePrefix="[openclaw]"
/debug unset messages.responsePrefix
/debug reset
```

Example 2 (unknown):
```unknown
/trace
/trace on
/trace off
```

Example 3 (sass):
```sass
OPENCLAW_PLUGIN_LIFECYCLE_TRACE=1 openclaw plugins install tokenjuice --force
```

Example 4 (sass):
```sass
[plugins:lifecycle] phase="config read" ms=6.83 status=ok command="install"
[plugins:lifecycle] phase="slot selection" ms=94.31 status=ok command="install" pluginId="tokenjuice"
[plugins:lifecycle] phase="registry refresh" ms=51.56 status=ok command="install" reason="source-changed"
```

---
