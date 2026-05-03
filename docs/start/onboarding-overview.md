---
source_url: https://docs.openclaw.ai/start/onboarding-overview
title: "Onboarding overview - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/start/onboarding-overview#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

First steps

Onboarding overview

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Which path should I use?](https://docs.openclaw.ai/start/onboarding-overview#which-path-should-i-use)
- [What onboarding configures](https://docs.openclaw.ai/start/onboarding-overview#what-onboarding-configures)
- [CLI onboarding](https://docs.openclaw.ai/start/onboarding-overview#cli-onboarding)
- [macOS app onboarding](https://docs.openclaw.ai/start/onboarding-overview#macos-app-onboarding)
- [Custom or unlisted providers](https://docs.openclaw.ai/start/onboarding-overview#custom-or-unlisted-providers)
- [Related](https://docs.openclaw.ai/start/onboarding-overview#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw has two onboarding paths. Both configure auth, the Gateway, and
optional chat channels — they just differ in how you interact with the setup.

## [​](https://docs.openclaw.ai/start/onboarding-overview\#which-path-should-i-use)  Which path should I use?

|  | CLI onboarding | macOS app onboarding |
| --- | --- | --- |
| **Platforms** | macOS, Linux, Windows (native or WSL2) | macOS only |
| **Interface** | Terminal wizard | Guided UI in the app |
| **Best for** | Servers, headless, full control | Desktop Mac, visual setup |
| **Automation** | `--non-interactive` for scripts | Manual only |
| **Command** | `openclaw onboard` | Launch the app |

Most users should start with **CLI onboarding** — it works everywhere and gives
you the most control.

## [​](https://docs.openclaw.ai/start/onboarding-overview\#what-onboarding-configures)  What onboarding configures

Regardless of which path you choose, onboarding sets up:

1. **Model provider and auth** — API key, OAuth, or setup token for your chosen provider
2. **Workspace** — directory for agent files, bootstrap templates, and memory
3. **Gateway** — port, bind address, auth mode
4. **Channels** (optional) — built-in and bundled chat channels such as
BlueBubbles, Discord, Feishu, Google Chat, Mattermost, Microsoft Teams,
Telegram, WhatsApp, and more
5. **Daemon** (optional) — background service so the Gateway starts automatically

## [​](https://docs.openclaw.ai/start/onboarding-overview\#cli-onboarding)  CLI onboarding

Run in any terminal:

```
openclaw onboard
```

Add `--install-daemon` to also install the background service in one step.Full reference: [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard)
CLI command docs: [`openclaw onboard`](https://docs.openclaw.ai/cli/onboard)

## [​](https://docs.openclaw.ai/start/onboarding-overview\#macos-app-onboarding)  macOS app onboarding

Open the OpenClaw app. The first-run wizard walks you through the same steps
with a visual interface.Full reference: [Onboarding (macOS App)](https://docs.openclaw.ai/start/onboarding)

## [​](https://docs.openclaw.ai/start/onboarding-overview\#custom-or-unlisted-providers)  Custom or unlisted providers

If your provider is not listed in onboarding, choose **Custom Provider** and
enter:

- API compatibility mode (OpenAI-compatible, Anthropic-compatible, or auto-detect)
- Base URL and API key
- Model ID and optional alias

Multiple custom endpoints can coexist — each gets its own endpoint ID.

## [​](https://docs.openclaw.ai/start/onboarding-overview\#related)  Related

- [Getting started](https://docs.openclaw.ai/start/getting-started)
- [CLI setup reference](https://docs.openclaw.ai/start/wizard-cli-reference)

[Getting started](https://docs.openclaw.ai/start/getting-started) [Onboarding: CLI](https://docs.openclaw.ai/start/wizard)

Ctrl+I