---
source_url: https://docs.openclaw.ai/install/railway
title: "Railway - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/railway#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Hosting

Railway

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Railway](https://docs.openclaw.ai/install/railway#railway)
- [Quick checklist (new users)](https://docs.openclaw.ai/install/railway#quick-checklist-new-users)
- [One-click deploy](https://docs.openclaw.ai/install/railway#one-click-deploy)
- [What you get](https://docs.openclaw.ai/install/railway#what-you-get)
- [Required Railway settings](https://docs.openclaw.ai/install/railway#required-railway-settings)
- [Public Networking](https://docs.openclaw.ai/install/railway#public-networking)
- [Volume (required)](https://docs.openclaw.ai/install/railway#volume-required)
- [Variables](https://docs.openclaw.ai/install/railway#variables)
- [Connect a channel](https://docs.openclaw.ai/install/railway#connect-a-channel)
- [Backups & migration](https://docs.openclaw.ai/install/railway#backups-%26-migration)
- [Next steps](https://docs.openclaw.ai/install/railway#next-steps)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/install/railway\#railway)  Railway

Deploy OpenClaw on Railway with a one-click template and access it through the web Control UI.
This is the easiest “no terminal on the server” path: Railway runs the Gateway for you.

## [​](https://docs.openclaw.ai/install/railway\#quick-checklist-new-users)  Quick checklist (new users)

1. Click **Deploy on Railway** (below).
2. Add a **Volume** mounted at `/data`.
3. Set the required **Variables** (at least `OPENCLAW_GATEWAY_PORT` and `OPENCLAW_GATEWAY_TOKEN`).
4. Enable **HTTP Proxy** on port `8080`.
5. Open `https://<your-railway-domain>/openclaw` and connect using the configured shared secret. This template uses `OPENCLAW_GATEWAY_TOKEN` by default; if you replace it with password auth, use that password instead.

## [​](https://docs.openclaw.ai/install/railway\#one-click-deploy)  One-click deploy

[Deploy on Railway](https://railway.com/deploy/clawdbot-railway-template) After deploy, find your public URL in **Railway → your service → Settings → Domains**.Railway will either:

- give you a generated domain (often `https://<something>.up.railway.app`), or
- use your custom domain if you attached one.

Then open:

- `https://<your-railway-domain>/openclaw` — Control UI

## [​](https://docs.openclaw.ai/install/railway\#what-you-get)  What you get

- Hosted OpenClaw Gateway + Control UI
- Persistent storage via Railway Volume (`/data`) so `openclaw.json`,
per-agent `auth-profiles.json`, channel/provider state, sessions, and
workspace survive redeploys

## [​](https://docs.openclaw.ai/install/railway\#required-railway-settings)  Required Railway settings

### [​](https://docs.openclaw.ai/install/railway\#public-networking)  Public Networking

Enable **HTTP Proxy** for the service.

- Port: `8080`

### [​](https://docs.openclaw.ai/install/railway\#volume-required)  Volume (required)

Attach a volume mounted at:

- `/data`

### [​](https://docs.openclaw.ai/install/railway\#variables)  Variables

Set these variables on the service:

- `OPENCLAW_GATEWAY_PORT=8080` (required — must match the port in Public Networking)
- `OPENCLAW_GATEWAY_TOKEN` (required; treat as an admin secret)
- `OPENCLAW_STATE_DIR=/data/.openclaw` (recommended)
- `OPENCLAW_WORKSPACE_DIR=/data/workspace` (recommended)

## [​](https://docs.openclaw.ai/install/railway\#connect-a-channel)  Connect a channel

Use the Control UI at `/openclaw` or run `openclaw onboard` via Railway’s shell for channel setup instructions:

- [Telegram](https://docs.openclaw.ai/channels/telegram) (fastest — just a bot token)
- [Discord](https://docs.openclaw.ai/channels/discord)
- [All channels](https://docs.openclaw.ai/channels)

## [​](https://docs.openclaw.ai/install/railway\#backups-&-migration)  Backups & migration

Export your state, config, auth profiles, and workspace:

```
openclaw backup create
```

This creates a portable backup archive with OpenClaw state plus any configured
workspace. See [Backup](https://docs.openclaw.ai/cli/backup) for details.

## [​](https://docs.openclaw.ai/install/railway\#next-steps)  Next steps

- Set up messaging channels: [Channels](https://docs.openclaw.ai/channels)
- Configure the Gateway: [Gateway configuration](https://docs.openclaw.ai/gateway/configuration)
- Keep OpenClaw up to date: [Updating](https://docs.openclaw.ai/install/updating)

[Oracle Cloud](https://docs.openclaw.ai/install/oracle) [Raspberry Pi](https://docs.openclaw.ai/install/raspberry-pi)

Ctrl+I