---
source_url: https://docs.openclaw.ai/reference/AGENTS.default
title: "Default AGENTS.md - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/reference/AGENTS.default#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Templates

Default AGENTS.md

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [AGENTS.md - OpenClaw Personal Assistant (default)](https://docs.openclaw.ai/reference/AGENTS.default#agents-md-openclaw-personal-assistant-default)
- [First run (recommended)](https://docs.openclaw.ai/reference/AGENTS.default#first-run-recommended)
- [Safety defaults](https://docs.openclaw.ai/reference/AGENTS.default#safety-defaults)
- [Session start (required)](https://docs.openclaw.ai/reference/AGENTS.default#session-start-required)
- [Soul (required)](https://docs.openclaw.ai/reference/AGENTS.default#soul-required)
- [Shared spaces (recommended)](https://docs.openclaw.ai/reference/AGENTS.default#shared-spaces-recommended)
- [Memory system (recommended)](https://docs.openclaw.ai/reference/AGENTS.default#memory-system-recommended)
- [Tools & skills](https://docs.openclaw.ai/reference/AGENTS.default#tools-%26-skills)
- [Backup tip (recommended)](https://docs.openclaw.ai/reference/AGENTS.default#backup-tip-recommended)
- [What OpenClaw does](https://docs.openclaw.ai/reference/AGENTS.default#what-openclaw-does)
- [Core skills (enable in Settings → Skills)](https://docs.openclaw.ai/reference/AGENTS.default#core-skills-enable-in-settings-%E2%86%92-skills)
- [Usage notes](https://docs.openclaw.ai/reference/AGENTS.default#usage-notes)
- [Related](https://docs.openclaw.ai/reference/AGENTS.default#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/reference/AGENTS.default\#agents-md-openclaw-personal-assistant-default)  AGENTS.md - OpenClaw Personal Assistant (default)

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#first-run-recommended)  First run (recommended)

OpenClaw uses a dedicated workspace directory for the agent. Default: `~/.openclaw/workspace` (configurable via `agents.defaults.workspace`).

1. Create the workspace (if it doesn’t already exist):

```
mkdir -p ~/.openclaw/workspace
```

2. Copy the default workspace templates into the workspace:

```
cp docs/reference/templates/AGENTS.md ~/.openclaw/workspace/AGENTS.md
cp docs/reference/templates/SOUL.md ~/.openclaw/workspace/SOUL.md
cp docs/reference/templates/TOOLS.md ~/.openclaw/workspace/TOOLS.md
```

3. Optional: if you want the personal assistant skill roster, replace AGENTS.md with this file:

```
cp docs/reference/AGENTS.default.md ~/.openclaw/workspace/AGENTS.md
```

4. Optional: choose a different workspace by setting `agents.defaults.workspace` (supports `~`):

```
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
}
```

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#safety-defaults)  Safety defaults

- Don’t dump directories or secrets into chat.
- Don’t run destructive commands unless explicitly asked.
- Don’t send partial/streaming replies to external messaging surfaces (only final replies).

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#session-start-required)  Session start (required)

- Read `SOUL.md`, `USER.md`, and today+yesterday in `memory/`.
- Read `MEMORY.md` when present.
- Do it before responding.

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#soul-required)  Soul (required)

- `SOUL.md` defines identity, tone, and boundaries. Keep it current.
- If you change `SOUL.md`, tell the user.
- You are a fresh instance each session; continuity lives in these files.

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#shared-spaces-recommended)  Shared spaces (recommended)

- You’re not the user’s voice; be careful in group chats or public channels.
- Don’t share private data, contact info, or internal notes.

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#memory-system-recommended)  Memory system (recommended)

- Daily log: `memory/YYYY-MM-DD.md` (create `memory/` if needed).
- Long-term memory: `MEMORY.md` for durable facts, preferences, and decisions.
- Lowercase `memory.md` is legacy repair input only; do not keep both root files on purpose.
- On session start, read today + yesterday + `MEMORY.md` when present.
- Capture: decisions, preferences, constraints, open loops.
- Avoid secrets unless explicitly requested.

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#tools-&-skills)  Tools & skills

- Tools live in skills; follow each skill’s `SKILL.md` when you need it.
- Keep environment-specific notes in `TOOLS.md` (Notes for Skills).

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#backup-tip-recommended)  Backup tip (recommended)

If you treat this workspace as Clawd’s “memory”, make it a git repo (ideally private) so `AGENTS.md` and your memory files are backed up.

```
cd ~/.openclaw/workspace
git init
git add AGENTS.md
git commit -m "Add Clawd workspace"
# Optional: add a private remote + push
```

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#what-openclaw-does)  What OpenClaw does

- Runs WhatsApp gateway + Pi coding agent so the assistant can read/write chats, fetch context, and run skills via the host Mac.
- macOS app manages permissions (screen recording, notifications, microphone) and exposes the `openclaw` CLI via its bundled binary.
- Direct chats collapse into the agent’s `main` session by default; groups stay isolated as `agent:<agentId>:<channel>:group:<id>` (rooms/channels: `agent:<agentId>:<channel>:channel:<id>`); heartbeats keep background tasks alive.

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#core-skills-enable-in-settings-%E2%86%92-skills)  Core skills (enable in Settings → Skills)

- **mcporter** — Tool server runtime/CLI for managing external skill backends.
- **Peekaboo** — Fast macOS screenshots with optional AI vision analysis.
- **camsnap** — Capture frames, clips, or motion alerts from RTSP/ONVIF security cams.
- **oracle** — OpenAI-ready agent CLI with session replay and browser control.
- **eightctl** — Control your sleep, from the terminal.
- **imsg** — Send, read, stream iMessage & SMS.
- **wacli** — WhatsApp CLI: sync, search, send.
- **discord** — Discord actions: react, stickers, polls. Use `user:<id>` or `channel:<id>` targets (bare numeric ids are ambiguous).
- **gog** — Google Suite CLI: Gmail, Calendar, Drive, Contacts.
- **spotify-player** — Terminal Spotify client to search/queue/control playback.
- **sag** — ElevenLabs speech with mac-style say UX; streams to speakers by default.
- **Sonos CLI** — Control Sonos speakers (discover/status/playback/volume/grouping) from scripts.
- **blucli** — Play, group, and automate BluOS players from scripts.
- **OpenHue CLI** — Philips Hue lighting control for scenes and automations.
- **OpenAI Whisper** — Local speech-to-text for quick dictation and voicemail transcripts.
- **Gemini CLI** — Google Gemini models from the terminal for fast Q&A.
- **agent-tools** — Utility toolkit for automations and helper scripts.

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#usage-notes)  Usage notes

- Prefer the `openclaw` CLI for scripting; mac app handles permissions.
- Run installs from the Skills tab; it hides the button if a binary is already present.
- Keep heartbeats enabled so the assistant can schedule reminders, monitor inboxes, and trigger camera captures.
- Canvas UI runs full-screen with native overlays. Avoid placing critical controls in the top-left/top-right/bottom edges; add explicit gutters in the layout and don’t rely on safe-area insets.
- For browser-driven verification, use `openclaw browser` (tabs/status/screenshot) with the OpenClaw-managed Chrome profile.
- For DOM inspection, use `openclaw browser eval|query|dom|snapshot` (and `--json`/`--out` when you need machine output).
- For interactions, use `openclaw browser click|type|hover|drag|select|upload|press|wait|navigate|back|evaluate|run` (click/type require snapshot refs; use `evaluate` for CSS selectors).

## [​](https://docs.openclaw.ai/reference/AGENTS.default\#related)  Related

- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)
- [Agent runtime](https://docs.openclaw.ai/concepts/agent)

[Device model database](https://docs.openclaw.ai/reference/device-models) [AGENTS.md template](https://docs.openclaw.ai/reference/templates/AGENTS)

Ctrl+I