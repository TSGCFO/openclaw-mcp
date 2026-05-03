---
source_url: https://docs.openclaw.ai/start/openclaw
title: "Personal assistant setup - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/start/openclaw#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Guides

Personal assistant setup

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Building a personal assistant with OpenClaw](https://docs.openclaw.ai/start/openclaw#building-a-personal-assistant-with-openclaw)
- [⚠️ Safety first](https://docs.openclaw.ai/start/openclaw#-safety-first)
- [Prerequisites](https://docs.openclaw.ai/start/openclaw#prerequisites)
- [The two-phone setup (recommended)](https://docs.openclaw.ai/start/openclaw#the-two-phone-setup-recommended)
- [5-minute quick start](https://docs.openclaw.ai/start/openclaw#5-minute-quick-start)
- [Give the agent a workspace (AGENTS)](https://docs.openclaw.ai/start/openclaw#give-the-agent-a-workspace-agents)
- [The config that turns it into “an assistant”](https://docs.openclaw.ai/start/openclaw#the-config-that-turns-it-into-%E2%80%9Can-assistant%E2%80%9D)
- [Sessions and memory](https://docs.openclaw.ai/start/openclaw#sessions-and-memory)
- [Heartbeats (proactive mode)](https://docs.openclaw.ai/start/openclaw#heartbeats-proactive-mode)
- [Media in and out](https://docs.openclaw.ai/start/openclaw#media-in-and-out)
- [Operations checklist](https://docs.openclaw.ai/start/openclaw#operations-checklist)
- [Next steps](https://docs.openclaw.ai/start/openclaw#next-steps)
- [Related](https://docs.openclaw.ai/start/openclaw#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/start/openclaw\#building-a-personal-assistant-with-openclaw)  Building a personal assistant with OpenClaw

OpenClaw is a self-hosted gateway that connects Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, and more to AI agents. This guide covers the “personal assistant” setup: a dedicated WhatsApp number that behaves like your always-on AI assistant.

## [​](https://docs.openclaw.ai/start/openclaw\#-safety-first)  ⚠️ Safety first

You’re putting an agent in a position to:

- run commands on your machine (depending on your tool policy)
- read/write files in your workspace
- send messages back out via WhatsApp/Telegram/Discord/Mattermost and other bundled channels

Start conservative:

- Always set `channels.whatsapp.allowFrom` (never run open-to-the-world on your personal Mac).
- Use a dedicated WhatsApp number for the assistant.
- Heartbeats now default to every 30 minutes. Disable until you trust the setup by setting `agents.defaults.heartbeat.every: "0m"`.

## [​](https://docs.openclaw.ai/start/openclaw\#prerequisites)  Prerequisites

- OpenClaw installed and onboarded — see [Getting Started](https://docs.openclaw.ai/start/getting-started) if you haven’t done this yet
- A second phone number (SIM/eSIM/prepaid) for the assistant

## [​](https://docs.openclaw.ai/start/openclaw\#the-two-phone-setup-recommended)  The two-phone setup (recommended)

You want this:

If you link your personal WhatsApp to OpenClaw, every message to you becomes “agent input”. That’s rarely what you want.

## [​](https://docs.openclaw.ai/start/openclaw\#5-minute-quick-start)  5-minute quick start

1. Pair WhatsApp Web (shows QR; scan with the assistant phone):

```
openclaw channels login
```

2. Start the Gateway (leave it running):

```
openclaw gateway --port 18789
```

3. Put a minimal config in `~/.openclaw/openclaw.json`:

```
{
  gateway: { mode: "local" },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

Now message the assistant number from your allowlisted phone.When onboarding finishes, OpenClaw auto-opens the dashboard and prints a clean (non-tokenized) link. If the dashboard prompts for auth, paste the configured shared secret into Control UI settings. Onboarding uses a token by default (`gateway.auth.token`), but password auth works too if you switched `gateway.auth.mode` to `password`. To reopen later: `openclaw dashboard`.

## [​](https://docs.openclaw.ai/start/openclaw\#give-the-agent-a-workspace-agents)  Give the agent a workspace (AGENTS)

OpenClaw reads operating instructions and “memory” from its workspace directory.By default, OpenClaw uses `~/.openclaw/workspace` as the agent workspace, and will create it (plus starter `AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`) automatically on setup/first agent run. `BOOTSTRAP.md` is only created when the workspace is brand new (it should not come back after you delete it). `MEMORY.md` is optional (not auto-created); when present, it is loaded for normal sessions. Subagent sessions only inject `AGENTS.md` and `TOOLS.md`.

Treat this folder like OpenClaw’s memory and make it a git repo (ideally private) so your `AGENTS.md` and memory files are backed up. If git is installed, brand-new workspaces are auto-initialized.

```
openclaw setup
```

Full workspace layout + backup guide: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)
Memory workflow: [Memory](https://docs.openclaw.ai/concepts/memory)Optional: choose a different workspace with `agents.defaults.workspace` (supports `~`).

```
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
    },
  },
}
```

If you already ship your own workspace files from a repo, you can disable bootstrap file creation entirely:

```
{
  agents: {
    defaults: {
      skipBootstrap: true,
    },
  },
}
```

## [​](https://docs.openclaw.ai/start/openclaw\#the-config-that-turns-it-into-%E2%80%9Can-assistant%E2%80%9D)  The config that turns it into “an assistant”

OpenClaw defaults to a good assistant setup, but you’ll usually want to tune:

- persona/instructions in [`SOUL.md`](https://docs.openclaw.ai/concepts/soul)
- thinking defaults (if desired)
- heartbeats (once you trust it)

Example:

```
{
  logging: { level: "info" },
  agent: {
    model: "anthropic/claude-opus-4-6",
    workspace: "~/.openclaw/workspace",
    thinkingDefault: "high",
    timeoutSeconds: 1800,
    // Start with 0; enable later.
    heartbeat: { every: "0m" },
  },
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: {
        "*": { requireMention: true },
      },
    },
  },
  routing: {
    groupChat: {
      mentionPatterns: ["@openclaw", "openclaw"],
    },
  },
  session: {
    scope: "per-sender",
    resetTriggers: ["/new", "/reset"],
    reset: {
      mode: "daily",
      atHour: 4,
      idleMinutes: 10080,
    },
  },
}
```

## [​](https://docs.openclaw.ai/start/openclaw\#sessions-and-memory)  Sessions and memory

- Session files: `~/.openclaw/agents/<agentId>/sessions/{{SessionId}}.jsonl`
- Session metadata (token usage, last route, etc): `~/.openclaw/agents/<agentId>/sessions/sessions.json` (legacy: `~/.openclaw/sessions/sessions.json`)
- `/new` or `/reset` starts a fresh session for that chat (configurable via `resetTriggers`). If sent alone, OpenClaw acknowledges the reset without invoking the model.
- `/compact [instructions]` compacts the session context and reports the remaining context budget.

## [​](https://docs.openclaw.ai/start/openclaw\#heartbeats-proactive-mode)  Heartbeats (proactive mode)

By default, OpenClaw runs a heartbeat every 30 minutes with the prompt:
`Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.`
Set `agents.defaults.heartbeat.every: "0m"` to disable.

- If `HEARTBEAT.md` exists but is effectively empty (only blank lines and markdown headers like `# Heading`), OpenClaw skips the heartbeat run to save API calls.
- If the file is missing, the heartbeat still runs and the model decides what to do.
- If the agent replies with `HEARTBEAT_OK` (optionally with short padding; see `agents.defaults.heartbeat.ackMaxChars`), OpenClaw suppresses outbound delivery for that heartbeat.
- By default, heartbeat delivery to DM-style `user:<id>` targets is allowed. Set `agents.defaults.heartbeat.directPolicy: "block"` to suppress direct-target delivery while keeping heartbeat runs active.
- Heartbeats run full agent turns — shorter intervals burn more tokens.

```
{
  agent: {
    heartbeat: { every: "30m" },
  },
}
```

## [​](https://docs.openclaw.ai/start/openclaw\#media-in-and-out)  Media in and out

Inbound attachments (images/audio/docs) can be surfaced to your command via templates:

- `{{MediaPath}}` (local temp file path)
- `{{MediaUrl}}` (pseudo-URL)
- `{{Transcript}}` (if audio transcription is enabled)

Outbound attachments from the agent: include `MEDIA:<path-or-url>` on its own line (no spaces). Example:

```
Here’s the screenshot.
MEDIA:https://example.com/screenshot.png
```

OpenClaw extracts these and sends them as media alongside the text.Local-path behavior follows the same file-read trust model as the agent:

- If `tools.fs.workspaceOnly` is `true`, outbound `MEDIA:` local paths stay restricted to the OpenClaw temp root, the media cache, agent workspace paths, and sandbox-generated files.
- If `tools.fs.workspaceOnly` is `false`, outbound `MEDIA:` can use host-local files the agent is already allowed to read.
- Host-local sends still only allow media and safe document types (images, audio, video, PDF, and Office documents). Plain text and secret-like files are not treated as sendable media.

That means generated images/files outside the workspace can now send when your fs policy already allows those reads, without reopening arbitrary host-text attachment exfiltration.

## [​](https://docs.openclaw.ai/start/openclaw\#operations-checklist)  Operations checklist

```
openclaw status          # local status (creds, sessions, queued events)
openclaw status --all    # full diagnosis (read-only, pasteable)
openclaw status --deep   # asks the gateway for a live health probe with channel probes when supported
openclaw health --json   # gateway health snapshot (WS; default can return a fresh cached snapshot)
```

Logs live under `/tmp/openclaw/` (default: `openclaw-YYYY-MM-DD.log`).

## [​](https://docs.openclaw.ai/start/openclaw\#next-steps)  Next steps

- WebChat: [WebChat](https://docs.openclaw.ai/web/webchat)
- Gateway ops: [Gateway runbook](https://docs.openclaw.ai/gateway)
- Cron + wakeups: [Cron jobs](https://docs.openclaw.ai/automation/cron-jobs)
- macOS menu bar companion: [OpenClaw macOS app](https://docs.openclaw.ai/platforms/macos)
- iOS node app: [iOS app](https://docs.openclaw.ai/platforms/ios)
- Android node app: [Android app](https://docs.openclaw.ai/platforms/android)
- Windows status: [Windows (WSL2)](https://docs.openclaw.ai/platforms/windows)
- Linux status: [Linux app](https://docs.openclaw.ai/platforms/linux)
- Security: [Security](https://docs.openclaw.ai/gateway/security)

## [​](https://docs.openclaw.ai/start/openclaw\#related)  Related

- [Getting started](https://docs.openclaw.ai/start/getting-started)
- [Setup](https://docs.openclaw.ai/start/setup)
- [Channels overview](https://docs.openclaw.ai/channels)

[Onboarding: macOS App](https://docs.openclaw.ai/start/onboarding) [CLI reference](https://docs.openclaw.ai/start/wizard-cli-reference)

Ctrl+I