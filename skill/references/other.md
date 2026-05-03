# Other

_6 pages from docs.openclaw.ai_


---

## OpenClaw - OpenClaw

_Source: <https://docs.openclaw.ai>_

# OpenClaw 🦞

> _“EXFOLIATE! EXFOLIATE!”_ — A space lobster, probably

**Any OS gateway for AI agents across Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, and more.**

Send a message, get an agent response from your pocket. Run one Gateway across built-in channels, bundled channel plugins, WebChat, and mobile nodes.

[**Get Started** \\
\\
Install OpenClaw and bring up the Gateway in minutes.](https://docs.openclaw.ai/start/getting-started)

[**Run Onboarding** \\
\\
Guided setup with `openclaw onboard` and pairing flows.](https://docs.openclaw.ai/start/wizard)

[**Open the Control UI** \\
\\
Launch the browser dashboard for chat, config, and sessions.](https://docs.openclaw.ai/web/control-ui)

## What is OpenClaw?

OpenClaw is a **self-hosted gateway** that connects your favorite chat apps and channel surfaces — built-in channels plus bundled or external channel plugins such as Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, and more — to AI coding agents like Pi. You run a single Gateway process on your own machine (or a server), and it becomes the bridge between your messaging apps and an always-available AI assistant.**Who is it for?** Developers and power users who want a personal AI assistant they can message from anywhere — without giving up control of their data or relying on a hosted service.**What makes it different?**

- **Self-hosted**: runs on your hardware, your rules
- **Multi-channel**: one Gateway serves built-in channels plus bundled or external channel plugins simultaneously
- **Agent-native**: built for coding agents with tool use, sessions, memory, and multi-agent routing
- **Open source**: MIT licensed, community-driven

**What do you need?** Node 24 (recommended), or Node 22 LTS (`22.14+`) for compatibility, an API key from your chosen provider, and 5 minutes. For best quality and security, use the strongest latest-generation model available.

## How it works

The Gateway is the single source of truth for sessions, routing, and channel connections.

## Key capabilities

[**Multi-channel gateway** \\
\\
Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat, and more with a single Gateway process.](https://docs.openclaw.ai/channels)

[**Plugin channels** \\
\\
Bundled plugins add Matrix, Nostr, Twitch, Zalo, and more in normal current releases.](https://docs.openclaw.ai/tools/plugin)

[**Multi-agent routing** \\
\\
Isolated sessions per agent, workspace, or sender.](https://docs.openclaw.ai/concepts/multi-agent)

[**Media support** \\
\\
Send and receive images, audio, and documents.](https://docs.openclaw.ai/nodes/images)

[**Web Control UI** \\
\\
Browser dashboard for chat, config, sessions, and nodes.](https://docs.openclaw.ai/web/control-ui)

[**Mobile nodes** \\
\\
Pair iOS and Android nodes for Canvas, camera, and voice-enabled workflows.](https://docs.openclaw.ai/nodes)

## Quick start

1

[Navigate to header](https://docs.openclaw.ai/#)

Install OpenClaw

```
npm install -g openclaw@latest
```

2

[Navigate to header](https://docs.openclaw.ai/#)

Onboard and install the service

```
openclaw onboard --install-daemon
```

3

[Navigate to header](https://docs.openclaw.ai/#)

Chat

Open the Control UI in your browser and send a message:

```
openclaw dashboard
```

Or connect a channel ( [Telegram](https://docs.openclaw.ai/channels/telegram) is fastest) and chat from your phone.

Need the full install and dev setup? See [Getting Started](https://docs.openclaw.ai/start/getting-started).

## Dashboard

Open the browser Control UI after the Gateway starts.

- Local default: [http://127.0.0.1:18789/](http://127.0.0.1:18789/)
- Remote access: [Web surfaces](https://docs.openclaw.ai/web) and [Tailscale](https://docs.openclaw.ai/gateway/tailscale)

## Configuration (optional)

Config lives at `~/.openclaw/openclaw.json`.

- If you **do nothing**, OpenClaw uses the bundled Pi binary in RPC mode with

_… [truncated; see https://docs.openclaw.ai for full content]_


---

## Pi integration architecture - OpenClaw

_Source: <https://docs.openclaw.ai/pi>_

[OpenClaw home page](https://docs.openclaw.ai/)

Technical reference

Pi integration architecture

OpenClaw integrates with [pi-coding-agent](https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent) and its sibling packages (`pi-ai`, `pi-agent-core`, `pi-tui`) to power its AI agent capabilities.

## Overview

OpenClaw uses the pi SDK to embed an AI coding agent into its messaging gateway architecture. Instead of spawning pi as a subprocess or using RPC mode, OpenClaw directly imports and instantiates pi’s `AgentSession` via `createAgentSession()`. This embedded approach provides:

- Full control over session lifecycle and event handling
- Custom tool injection (messaging, sandbox, channel-specific actions)
- System prompt customization per channel/context
- Session persistence with branching/compaction support
- Multi-account auth profile rotation with failover
- Provider-agnostic model switching

## Package dependencies

```
{
  "@mariozechner/pi-agent-core": "0.70.2",
  "@mariozechner/pi-ai": "0.70.2",
  "@mariozechner/pi-coding-agent": "0.70.2",
  "@mariozechner/pi-tui": "0.70.2"
}
```

| Package | Purpose |
| --- | --- |
| `pi-ai` | Core LLM abstractions: `Model`, `streamSimple`, message types, provider APIs |
| `pi-agent-core` | Agent loop, tool execution, `AgentMessage` types |
| `pi-coding-agent` | High-level SDK: `createAgentSession`, `SessionManager`, `AuthStorage`, `ModelRegistry`, built-in tools |
| `pi-tui` | Terminal UI components (used in OpenClaw’s local TUI mode) |

## File structure

```
src/agents/
├── pi-embedded-runner.ts          # Re-exports from pi-embedded-runner/
├── pi-embedded-runner/
│   ├── run.ts                     # Main entry: runEmbeddedPiAgent()
│   ├── run/
│   │   ├── attempt.ts             # Single attempt logic with session setup
│   │   ├── params.ts              # RunEmbeddedPiAgentParams type
│   │   ├── payloads.ts            # Build response payloads from run results
│   │   ├── images.ts              # Vision model image injection
│   │   └── types.ts               # EmbeddedRunAttemptResult
│   ├── abort.ts                   # Abort error detection
│   ├── cache-ttl.ts               # Cache TTL tracking for context pruning
│   ├── compact.ts                 # Manual/auto compaction logic
│   ├── extensions.ts              # Load pi extensions for embedded runs
│   ├── extra-params.ts            # Provider-specific stream params
│   ├── google.ts                  # Google/Gemini turn ordering fixes
│   ├── history.ts                 # History limiting (DM vs group)
│   ├── lanes.ts                   # Session/global command lanes
│   ├── logger.ts                  # Subsystem logger
│   ├── model.ts                   # Model resolution via ModelRegistry
│   ├── runs.ts                    # Active run tracking, abort, queue
│   ├── sandbox-info.ts            # Sandbox info for system prompt
│   ├── session-manager-cache.ts   # SessionManager instance caching
│   ├── session-manager-init.ts    # Session file initialization
│   ├── system-prompt.ts           # System prompt builder
│   ├── tool-split.ts              # Split tools into builtIn vs custom
│   ├── types.ts                   # EmbeddedPiAgentMeta, EmbeddedPiRunResult
│   └── utils.ts                   # ThinkLevel mapping, error description
├── pi-embedded-subscribe.ts       # Session event subscription/dispatch
├── pi-embedded-subscribe.types.ts # SubscribeEmbeddedPiSessionParams
├── pi-embedded-subscribe.handlers.ts # Event handler factory
├── pi-embedded-subscribe.handlers.lifecycle.ts
├── pi-embedded-subscribe.handlers.types.ts
├── pi-embedded-block-chunker.ts   # Streaming block reply chunking
├── pi-embedded-messaging.ts       # Messaging tool sent tracking
├── pi-embedded-helpers.ts         # Error classification, turn validation
├── pi-embedded-helpers/           # Helper modules
├── pi-embedded-utils.ts           # Formatting utilities
├── pi-tools.ts                    # createOpenClawCodingTools()
├──

_… [truncated; see https://docs.openclaw.ai/pi for full content]_


---

## Pi development workflow - OpenClaw

_Source: <https://docs.openclaw.ai/pi-dev>_

[OpenClaw home page](https://docs.openclaw.ai/)

Advanced setup

Pi development workflow

A sane workflow for working on the Pi integration in OpenClaw.

## Type checking and linting

- Default local gate: `pnpm check`
- Build gate: `pnpm build` when the change can affect build output, packaging, or lazy-loading/module boundaries
- Full landing gate for Pi-heavy changes: `pnpm check && pnpm test`

## Running Pi tests

Run the Pi-focused test set directly with Vitest:

```
pnpm test \
  "src/agents/pi-*.test.ts" \
  "src/agents/pi-embedded-*.test.ts" \
  "src/agents/pi-tools*.test.ts" \
  "src/agents/pi-settings.test.ts" \
  "src/agents/pi-tool-definition-adapter*.test.ts" \
  "src/agents/pi-hooks/**/*.test.ts"
```

To include the live provider exercise:

```
OPENCLAW_LIVE_TEST=1 pnpm test src/agents/pi-embedded-runner-extraparams.live.test.ts
```

This covers the main Pi unit suites:

- `src/agents/pi-*.test.ts`
- `src/agents/pi-embedded-*.test.ts`
- `src/agents/pi-tools*.test.ts`
- `src/agents/pi-settings.test.ts`
- `src/agents/pi-tool-definition-adapter.test.ts`
- `src/agents/pi-hooks/*.test.ts`

## Manual testing

Recommended flow:

- Run the gateway in dev mode:
  - `pnpm gateway:dev`
- Trigger the agent directly:
  - `pnpm openclaw agent --message "Hello" --thinking low`
- Use the TUI for interactive debugging:
  - `pnpm tui`

For tool call behavior, prompt for a `read` or `exec` action so you can see tool streaming and payload handling.

## Clean slate reset

State lives under the OpenClaw state directory. Default is `~/.openclaw`. If `OPENCLAW_STATE_DIR` is set, use that directory instead.To reset everything:

- `openclaw.json` for config
- `agents/<agentId>/agent/auth-profiles.json` for model auth profiles (API keys + OAuth)
- `credentials/` for provider/channel state that still lives outside the auth profile store
- `agents/<agentId>/sessions/` for agent session history
- `agents/<agentId>/sessions/sessions.json` for the session index
- `sessions/` if legacy paths exist
- `workspace/` if you want a blank workspace

If you only want to reset sessions, delete `agents/<agentId>/sessions/` for that agent. If you want to keep auth, leave `agents/<agentId>/agent/auth-profiles.json` and any provider state under `credentials/` in place.

## References

- [Testing](https://docs.openclaw.ai/help/testing)
- [Getting Started](https://docs.openclaw.ai/start/getting-started)

## Related

- [Pi integration architecture](https://docs.openclaw.ai/pi)

[Setup](https://docs.openclaw.ai/start/setup)

Ctrl+I


---

## https://docs.openclaw.ai/sitemap.xml

_Source: <https://docs.openclaw.ai/sitemap.xml>_

https://docs.openclaw.ai/ar/auth-credential-semantics2026-05-01T07:59:26.586Zhttps://docs.openclaw.ai/ar/automation/cron-jobs2026-05-01T07:59:26.576Zhttps://docs.openclaw.ai/ar/automation/hooks2026-05-01T07:59:26.610Zhttps://docs.openclaw.ai/ar/automation2026-05-01T07:59:26.589Zhttps://docs.openclaw.ai/ar/automation/standing-orders2026-05-01T07:59:26.973Zhttps://docs.openclaw.ai/ar/automation/taskflow2026-05-01T07:59:26.979Zhttps://docs.openclaw.ai/ar/automation/tasks2026-05-01T07:59:27.034Zhttps://docs.openclaw.ai/ar/channels/bluebubbles2026-05-01T07:59:26.960Zhttps://docs.openclaw.ai/ar/channels/broadcast-groups2026-05-01T07:59:26.963Zhttps://docs.openclaw.ai/ar/channels/channel-routing2026-05-01T07:59:27.051Zhttps://docs.openclaw.ai/ar/channels/discord2026-05-01T07:59:27.733Zhttps://docs.openclaw.ai/ar/channels/feishu2026-05-01T07:59:27.728Zhttps://docs.openclaw.ai/ar/channels/googlechat2026-05-01T07:59:27.703Zhttps://docs.openclaw.ai/ar/channels/group-messages2026-05-01T07:59:27.707Zhttps://docs.openclaw.ai/ar/channels/groups2026-05-01T07:59:27.700Zhttps://docs.openclaw.ai/ar/channels/imessage2026-05-01T07:59:27.692Zhttps://docs.openclaw.ai/ar/channels2026-05-01T07:59:27.710Zhttps://docs.openclaw.ai/ar/channels/irc2026-05-01T07:59:27.696Zhttps://docs.openclaw.ai/ar/channels/line2026-05-01T07:59:27.689Zhttps://docs.openclaw.ai/ar/channels/location2026-05-01T07:59:27.736Zhttps://docs.openclaw.ai/ar/channels/matrix2026-05-01T07:59:28.191Zhttps://docs.openclaw.ai/ar/channels/matrix-migration2026-05-01T07:59:29.416Zhttps://docs.openclaw.ai/ar/channels/matrix-push-rules2026-05-01T07:59:28.970Zhttps://docs.openclaw.ai/ar/channels/mattermost2026-05-01T07:59:28.152Zhttps://docs.openclaw.ai/ar/channels/msteams2026-05-01T07:59:28.179Zhttps://docs.openclaw.ai/ar/channels/nextcloud-talk2026-05-01T07:59:28.182Zhttps://docs.openclaw.ai/ar/channels/nostr2026-05-01T07:59:28.155Zhttps://docs.openclaw.ai/ar/channels/pairing2026-05-01T07:59:28.138Zhttps://docs.openclaw.ai/ar/channels/qa-channel2026-05-01T07:59:28.149Zhttps://docs.openclaw.ai/ar/channels/qqbot2026-05-01T07:59:28.125Zhttps://docs.openclaw.ai/ar/channels/signal2026-05-01T07:59:29.598Zhttps://docs.openclaw.ai/ar/channels/slack2026-05-01T07:59:29.597Zhttps://docs.openclaw.ai/ar/channels/synology-chat2026-05-01T07:59:29.541Zhttps://docs.openclaw.ai/ar/channels/telegram2026-05-01T07:59:29.588Zhttps://docs.openclaw.ai/ar/channels/tlon2026-05-01T07:59:29.514Zhttps://docs.openclaw.ai/ar/channels/troubleshooting2026-05-01T07:59:29.587Zhttps://docs.openclaw.ai/ar/channels/twitch2026-05-01T07:59:29.513Zhttps://docs.openclaw.ai/ar/channels/wechat2026-05-01T07:59:29.512Zhttps://docs.openclaw.ai/ar/channels/whatsapp2026-05-01T07:59:29.582Zhttps://docs.openclaw.ai/ar/channels/yuanbao2026-05-01T07:59:29.585Zhttps://docs.openclaw.ai/ar/channels/zalo2026-05-01T07:59:29.823Zhttps://docs.openclaw.ai/ar/channels/zalouser2026-05-01T07:59:29.816Zhttps://docs.openclaw.ai/ar/ci2026-05-01T07:59:29.760Zhttps://docs.openclaw.ai/ar/cli/acp2026-05-01T07:59:29.758Zhttps://docs.openclaw.ai/ar/cli/agent2026-05-01T07:59:29.755Zhttps://docs.openclaw.ai/ar/cli/agents2026-05-01T07:59:29.756Zhttps://docs.openclaw.ai/ar/cli/approvals2026-05-01T07:59:29.822Zhttps://docs.openclaw.ai/ar/cli/backup2026-05-01T07:59:29.761Zhttps://docs.openclaw.ai/ar/cli/browser2026-05-01T07:59:29.759Zhttps://docs.openclaw.ai/ar/cli/channels2026-05-01T07:59:29.757Zhttps://docs.openclaw.ai/ar/cli/clawbot2026-05-01T07:59:29.957Zhttps://docs.openclaw.ai/ar/cli/commitments2026-05-01T07:59:29.937Zhttps://docs.openclaw.ai/ar/cli/completion2026-05-01T07:59:29.945Zhttps://docs.openclaw.ai/ar/cli/config2026-05-01T07:59:29.943Zhttps://docs.openclaw.ai/ar/cli/configure2026-05-01T07:59:29.935Zhttps://docs.openclaw.ai/ar/cli/cron2026-05-01T07:59:29.913Zhttps://docs.openclaw.ai/ar/cli/daemon2026-05-01T07:59:29.912Zhttps://docs.openclaw.ai/ar/cli/dashboard2026-05-01T07:59:29.911Zhttps://docs.openclaw.ai/ar/cli/devices2026-05-01T07:59:29.910Zhttps://docs

_… [truncated; see https://docs.openclaw.ai/sitemap.xml for full content]_


---

## Linux server - OpenClaw

_Source: <https://docs.openclaw.ai/vps>_

[OpenClaw home page](https://docs.openclaw.ai/)

Hosting

Linux server

Run the OpenClaw Gateway on any Linux server or cloud VPS. This page helps you
pick a provider, explains how cloud deployments work, and covers generic Linux
tuning that applies everywhere.

## Pick a provider

[**Railway** \\
\\
One-click, browser setup](https://docs.openclaw.ai/install/railway)

[**Northflank** \\
\\
One-click, browser setup](https://docs.openclaw.ai/install/northflank)

[**DigitalOcean** \\
\\
Simple paid VPS](https://docs.openclaw.ai/install/digitalocean)

[**Oracle Cloud** \\
\\
Always Free ARM tier](https://docs.openclaw.ai/install/oracle)

[**Fly.io** \\
\\
Fly Machines](https://docs.openclaw.ai/install/fly)

[**Hetzner** \\
\\
Docker on Hetzner VPS](https://docs.openclaw.ai/install/hetzner)

[**Hostinger** \\
\\
VPS with one-click setup](https://docs.openclaw.ai/install/hostinger)

[**GCP** \\
\\
Compute Engine](https://docs.openclaw.ai/install/gcp)

[**Azure** \\
\\
Linux VM](https://docs.openclaw.ai/install/azure)

[**exe.dev** \\
\\
VM with HTTPS proxy](https://docs.openclaw.ai/install/exe-dev)

[**Raspberry Pi** \\
\\
ARM self-hosted](https://docs.openclaw.ai/install/raspberry-pi)

**AWS (EC2 / Lightsail / free tier)** also works well.
A community video walkthrough is available at
[x.com/techfrenAJ/status/2014934471095812547](https://x.com/techfrenAJ/status/2014934471095812547)
(community resource — may become unavailable).

## How cloud setups work

- The **Gateway runs on the VPS** and owns state + workspace.
- You connect from your laptop or phone via the **Control UI** or **Tailscale/SSH**.
- Treat the VPS as the source of truth and **back up** the state + workspace regularly.
- Secure default: keep the Gateway on loopback and access it via SSH tunnel or Tailscale Serve.
If you bind to `lan` or `tailnet`, require `gateway.auth.token` or `gateway.auth.password`.

Related pages: [Gateway remote access](https://docs.openclaw.ai/gateway/remote), [Platforms hub](https://docs.openclaw.ai/platforms).

## Harden admin access first

Before you install OpenClaw on a public VPS, decide how you want to administer
the box itself.

- If you want Tailnet-only admin access, install Tailscale first, join the VPS
to your tailnet, verify a second SSH session over the Tailscale IP or
MagicDNS name, then restrict public SSH.
- If you are not using Tailscale, apply the equivalent hardening for your SSH
path before exposing more services.
- This is separate from Gateway access. You can still keep OpenClaw bound to
loopback and use an SSH tunnel or Tailscale Serve for the dashboard.

Tailscale-specific Gateway options live in [Tailscale](https://docs.openclaw.ai/gateway/tailscale).

## Shared company agent on a VPS

Running a single agent for a team is a valid setup when every user is in the same trust boundary and the agent is business-only.

- Keep it on a dedicated runtime (VPS/VM/container + dedicated OS user/accounts).
- Do not sign that runtime into personal Apple/Google accounts or personal browser/password-manager profiles.
- If users are adversarial to each other, split by gateway/host/OS user.

Security model details: [Security](https://docs.openclaw.ai/gateway/security).

## Using nodes with a VPS

You can keep the Gateway in the cloud and pair **nodes** on your local devices
(Mac/iOS/Android/headless). Nodes provide local screen/camera/canvas and `system.run`
capabilities while the Gateway stays in the cloud.Docs: [Nodes](https://docs.openclaw.ai/nodes), [Nodes CLI](https://docs.openclaw.ai/cli/nodes).

## Startup tuning for small VMs and ARM hosts

If CLI commands feel slow on low-power VMs (or ARM hosts), enable Node’s module compile cache:

```
grep -q 'NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache' ~/.bashrc || cat >> ~/.bashrc <<'EOF'
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1
EOF
source ~/.bashrc
```

- `NODE_COMPILE_CACHE` improves repeated

_… [truncated; see https://docs.openclaw.ai/vps for full content]_


---

## https://docs.openclaw.ai/vps.md

_Source: <https://docs.openclaw.ai/vps.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Linux server

Run the OpenClaw Gateway on any Linux server or cloud VPS. This page helps you
pick a provider, explains how cloud deployments work, and covers generic Linux
tuning that applies everywhere.

\## Pick a provider

One-click, browser setupOne-click, browser setupSimple paid VPSAlways Free ARM tierFly MachinesDocker on Hetzner VPSVPS with one-click setupCompute EngineLinux VMVM with HTTPS proxyARM self-hosted

\*\*AWS (EC2 / Lightsail / free tier)\*\* also works well.
A community video walkthrough is available at
\[x.com/techfrenAJ/status/2014934471095812547\](https://x.com/techfrenAJ/status/2014934471095812547)
(community resource -- may become unavailable).

\## How cloud setups work

\\* The \*\*Gateway runs on the VPS\*\* and owns state + workspace.
\\* You connect from your laptop or phone via the \*\*Control UI\*\* or \*\*Tailscale/SSH\*\*.
\\* Treat the VPS as the source of truth and \*\*back up\*\* the state + workspace regularly.
\\* Secure default: keep the Gateway on loopback and access it via SSH tunnel or Tailscale Serve.
 If you bind to \`lan\` or \`tailnet\`, require \`gateway.auth.token\` or \`gateway.auth.password\`.

Related pages: \[Gateway remote access\](/gateway/remote), \[Platforms hub\](/platforms).

\## Harden admin access first

Before you install OpenClaw on a public VPS, decide how you want to administer
the box itself.

\\* If you want Tailnet-only admin access, install Tailscale first, join the VPS
 to your tailnet, verify a second SSH session over the Tailscale IP or
 MagicDNS name, then restrict public SSH.
\\* If you are not using Tailscale, apply the equivalent hardening for your SSH
 path before exposing more services.
\\* This is separate from Gateway access. You can still keep OpenClaw bound to
 loopback and use an SSH tunnel or Tailscale Serve for the dashboard.

Tailscale-specific Gateway options live in \[Tailscale\](/gateway/tailscale).

\## Shared company agent on a VPS

Running a single agent for a team is a valid setup when every user is in the same trust boundary and the agent is business-only.

\\* Keep it on a dedicated runtime (VPS/VM/container + dedicated OS user/accounts).
\\* Do not sign that runtime into personal Apple/Google accounts or personal browser/password-manager profiles.
\\* If users are adversarial to each other, split by gateway/host/OS user.

Security model details: \[Security\](/gateway/security).

\## Using nodes with a VPS

You can keep the Gateway in the cloud and pair \*\*nodes\*\* on your local devices
(Mac/iOS/Android/headless). Nodes provide local screen/camera/canvas and \`system.run\`
capabilities while the Gateway stays in the cloud.

Docs: \[Nodes\](/nodes), \[Nodes CLI\](/cli/nodes).

\## Startup tuning for small VMs and ARM hosts

If CLI commands feel slow on low-power VMs (or ARM hosts), enable Node's module compile cache:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
grep -q 'NODE\_COMPILE\_CACHE=/var/tmp/openclaw-compile-cache' ~/.bashrc \|\| cat >> ~/.bashrc <<'EOF'
export NODE\_COMPILE\_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW\_NO\_RESPAWN=1
EOF
source ~/.bashrc
\`\`\`

\\* \`NODE\_COMPILE\_CACHE\` improves repeated command startup times.
\\* \`OPENCLAW\_NO\_RESPAWN=1\` avoids extra startup overhead from a self-respawn path.
\\* First command run warms the cache; subsequent runs are faster.
\\* For Raspberry Pi specifics, see \[Raspberry Pi\](/install/raspberry-pi).

\### systemd tuning checklist (optional)

For VM hosts using \`systemd\`, consider:

\\* Add service env for a stable startup path:
 \\* \`OPENCLAW\_NO\_RESPAWN=1\`
 \\* \`NODE\_COMPILE\_CACHE=/var/tmp/openclaw-compile-cache\`
\\* Keep restart behavior explicit:
 \\* \`Restart=always\`
 \\* \`RestartSec=2\`
 \\* \`TimeoutStartSec=90\`
\\*

_… [truncated; see https://docs.openclaw.ai/vps.md for full content]_
