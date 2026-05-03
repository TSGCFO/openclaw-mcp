---
source_url: https://docs.openclaw.ai/platforms
title: "Platforms - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/platforms#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Platforms overview

Platforms

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Choose your OS](https://docs.openclaw.ai/platforms#choose-your-os)
- [VPS & hosting](https://docs.openclaw.ai/platforms#vps-%26-hosting)
- [Common links](https://docs.openclaw.ai/platforms#common-links)
- [Gateway service install (CLI)](https://docs.openclaw.ai/platforms#gateway-service-install-cli)
- [Related](https://docs.openclaw.ai/platforms#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw core is written in TypeScript. **Node is the recommended runtime**.
Bun is not recommended for the Gateway — known issues with WhatsApp and
Telegram channels; see [Bun (experimental)](https://docs.openclaw.ai/install/bun) for details.Companion apps exist for macOS (menu bar app) and mobile nodes (iOS/Android). Windows and
Linux companion apps are planned, but the Gateway is fully supported today.
Native companion apps for Windows are also planned; the Gateway is recommended via WSL2.

## [​](https://docs.openclaw.ai/platforms\#choose-your-os)  Choose your OS

- macOS: [macOS](https://docs.openclaw.ai/platforms/macos)
- iOS: [iOS](https://docs.openclaw.ai/platforms/ios)
- Android: [Android](https://docs.openclaw.ai/platforms/android)
- Windows: [Windows](https://docs.openclaw.ai/platforms/windows)
- Linux: [Linux](https://docs.openclaw.ai/platforms/linux)

## [​](https://docs.openclaw.ai/platforms\#vps-&-hosting)  VPS & hosting

- VPS hub: [VPS hosting](https://docs.openclaw.ai/vps)
- Fly.io: [Fly.io](https://docs.openclaw.ai/install/fly)
- Hetzner (Docker): [Hetzner](https://docs.openclaw.ai/install/hetzner)
- GCP (Compute Engine): [GCP](https://docs.openclaw.ai/install/gcp)
- Azure (Linux VM): [Azure](https://docs.openclaw.ai/install/azure)
- exe.dev (VM + HTTPS proxy): [exe.dev](https://docs.openclaw.ai/install/exe-dev)

## [​](https://docs.openclaw.ai/platforms\#common-links)  Common links

- Install guide: [Getting Started](https://docs.openclaw.ai/start/getting-started)
- Gateway runbook: [Gateway](https://docs.openclaw.ai/gateway)
- Gateway configuration: [Configuration](https://docs.openclaw.ai/gateway/configuration)
- Service status: `openclaw gateway status`

## [​](https://docs.openclaw.ai/platforms\#gateway-service-install-cli)  Gateway service install (CLI)

Use one of these (all supported):

- Wizard (recommended): `openclaw onboard --install-daemon`
- Direct: `openclaw gateway install`
- Configure flow: `openclaw configure` → select **Gateway service**
- Repair/migrate: `openclaw doctor` (offers to install or fix the service)

The service target depends on OS:

- macOS: LaunchAgent (`ai.openclaw.gateway` or `ai.openclaw.<profile>`; legacy `com.openclaw.*`)
- Linux/WSL2: systemd user service (`openclaw-gateway[-<profile>].service`)
- Native Windows: Scheduled Task (`OpenClaw Gateway` or `OpenClaw Gateway (<profile>)`), with a per-user Startup-folder login item fallback if task creation is denied

## [​](https://docs.openclaw.ai/platforms\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [macOS app](https://docs.openclaw.ai/platforms/macos)
- [iOS app](https://docs.openclaw.ai/platforms/ios)

[macOS app](https://docs.openclaw.ai/platforms/macos)

Ctrl+I