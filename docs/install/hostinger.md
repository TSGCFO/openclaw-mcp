---
source_url: https://docs.openclaw.ai/install/hostinger
title: "Hostinger - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/hostinger#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Hosting

Hostinger

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Prerequisites](https://docs.openclaw.ai/install/hostinger#prerequisites)
- [Option A: 1-Click OpenClaw](https://docs.openclaw.ai/install/hostinger#option-a-1-click-openclaw)
- [Option B: OpenClaw on VPS](https://docs.openclaw.ai/install/hostinger#option-b-openclaw-on-vps)
- [Verify your setup](https://docs.openclaw.ai/install/hostinger#verify-your-setup)
- [Troubleshooting](https://docs.openclaw.ai/install/hostinger#troubleshooting)
- [Next steps](https://docs.openclaw.ai/install/hostinger#next-steps)
- [Related](https://docs.openclaw.ai/install/hostinger#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Run a persistent OpenClaw Gateway on [Hostinger](https://www.hostinger.com/openclaw) via a **1-Click** managed deployment or a **VPS** install.

## [​](https://docs.openclaw.ai/install/hostinger\#prerequisites)  Prerequisites

- Hostinger account ( [signup](https://www.hostinger.com/openclaw))
- About 5-10 minutes

## [​](https://docs.openclaw.ai/install/hostinger\#option-a-1-click-openclaw)  Option A: 1-Click OpenClaw

The fastest way to get started. Hostinger handles infrastructure, Docker, and automatic updates.

1

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Purchase and launch

1. From the [Hostinger OpenClaw page](https://www.hostinger.com/openclaw), choose a Managed OpenClaw plan and complete checkout.

During checkout you can select **Ready-to-Use AI** credits that are pre-purchased and integrated instantly inside OpenClaw — no external accounts or API keys from other providers needed. You can start chatting right away. Alternatively, provide your own key from Anthropic, OpenAI, Google Gemini, or xAI during setup.

2

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Select a messaging channel

Choose one or more channels to connect:

- **WhatsApp** — scan the QR code shown in the setup wizard.
- **Telegram** — paste the bot token from [BotFather](https://t.me/BotFather).

3

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Complete installation

Click **Finish** to deploy the instance. Once ready, access the OpenClaw dashboard from **OpenClaw Overview** in hPanel.

## [​](https://docs.openclaw.ai/install/hostinger\#option-b-openclaw-on-vps)  Option B: OpenClaw on VPS

More control over your server. Hostinger deploys OpenClaw via Docker on your VPS and you manage it through the **Docker Manager** in hPanel.

1

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Purchase a VPS

1. From the [Hostinger OpenClaw page](https://www.hostinger.com/openclaw), choose an OpenClaw on VPS plan and complete checkout.

You can select **Ready-to-Use AI** credits during checkout — these are pre-purchased and integrated instantly inside OpenClaw, so you can start chatting without any external accounts or API keys from other providers.

2

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Configure OpenClaw

Once the VPS is provisioned, fill in the configuration fields:

- **Gateway token** — auto-generated; save it for later use.
- **WhatsApp number** — your number with country code (optional).
- **Telegram bot token** — from [BotFather](https://t.me/BotFather) (optional).
- **API keys** — only needed if you did not select Ready-to-Use AI credits during checkout.

3

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Start OpenClaw

Click **Deploy**. Once running, open the OpenClaw dashboard from the hPanel by clicking on **Open**.

Logs, restarts, and updates are managed directly from the Docker Manager interface in hPanel. To update, press on **Update** in Docker Manager and that will pull the latest image.

## [​](https://docs.openclaw.ai/install/hostinger\#verify-your-setup)  Verify your setup

Send “Hi” to your assistant on the channel you connected. OpenClaw will reply and walk you through initial preferences.

## [​](https://docs.openclaw.ai/install/hostinger\#troubleshooting)  Troubleshooting

**Dashboard not loading** — Wait a few minutes for the container to finish provisioning. Check the Docker Manager logs in hPanel.**Docker container keeps restarting** — Open Docker Manager logs and look for configuration errors (missing tokens, invalid API keys).**Telegram bot not responding** — Send your pairing code message from Telegram directly as a message inside your OpenClaw chat to complete the connection.

## [​](https://docs.openclaw.ai/install/hostinger\#next-steps)  Next steps

- [Channels](https://docs.openclaw.ai/channels) — connect Telegram, WhatsApp, Discord, and more
- [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) — all config options

## [​](https://docs.openclaw.ai/install/hostinger\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [VPS hosting](https://docs.openclaw.ai/vps)
- [DigitalOcean](https://docs.openclaw.ai/install/digitalocean)

[Hetzner](https://docs.openclaw.ai/install/hetzner) [Kubernetes](https://docs.openclaw.ai/install/kubernetes)

Ctrl+I