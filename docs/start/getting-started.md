---
source_url: https://docs.openclaw.ai/start/getting-started
title: "Getting started - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/start/getting-started#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

First steps

Getting started

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What you need](https://docs.openclaw.ai/start/getting-started#what-you-need)
- [Quick setup](https://docs.openclaw.ai/start/getting-started#quick-setup)
- [What to do next](https://docs.openclaw.ai/start/getting-started#what-to-do-next)
- [Related](https://docs.openclaw.ai/start/getting-started#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Install OpenClaw, run onboarding, and chat with your AI assistant — all in
about 5 minutes. By the end you will have a running Gateway, configured auth,
and a working chat session.

## [​](https://docs.openclaw.ai/start/getting-started\#what-you-need)  What you need

- **Node.js** — Node 24 recommended (Node 22.14+ also supported)
- **An API key** from a model provider (Anthropic, OpenAI, Google, etc.) — onboarding will prompt you

Check your Node version with `node --version`.
**Windows users:** both native Windows and WSL2 are supported. WSL2 is more
stable and recommended for the full experience. See [Windows](https://docs.openclaw.ai/platforms/windows).
Need to install Node? See [Node setup](https://docs.openclaw.ai/install/node).

## [​](https://docs.openclaw.ai/start/getting-started\#quick-setup)  Quick setup

1

[Navigate to header](https://docs.openclaw.ai/start/getting-started#)

Install OpenClaw

- macOS / Linux

- Windows (PowerShell)


```
curl -fsSL https://openclaw.ai/install.sh | bash
```

![Install Script Process](https://mintcdn.com/clawdhub/U8jr7qEbUc9OU9YR/assets/install-script.svg?fit=max&auto=format&n=U8jr7qEbUc9OU9YR&q=85&s=50706f81e3210a610262f14facb11f65)

```
iwr -useb https://openclaw.ai/install.ps1 | iex
```

Other install methods (Docker, Nix, npm): [Install](https://docs.openclaw.ai/install).

2

[Navigate to header](https://docs.openclaw.ai/start/getting-started#)

Run onboarding

```
openclaw onboard --install-daemon
```

The wizard walks you through choosing a model provider, setting an API key,
and configuring the Gateway. It takes about 2 minutes.See [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard) for the full reference.

3

[Navigate to header](https://docs.openclaw.ai/start/getting-started#)

Verify the Gateway is running

```
openclaw gateway status
```

You should see the Gateway listening on port 18789.

4

[Navigate to header](https://docs.openclaw.ai/start/getting-started#)

Open the dashboard

```
openclaw dashboard
```

This opens the Control UI in your browser. If it loads, everything is working.

5

[Navigate to header](https://docs.openclaw.ai/start/getting-started#)

Send your first message

Type a message in the Control UI chat and you should get an AI reply.Want to chat from your phone instead? The fastest channel to set up is
[Telegram](https://docs.openclaw.ai/channels/telegram) (just a bot token). See [Channels](https://docs.openclaw.ai/channels)
for all options.

Advanced: mount a custom Control UI build

If you maintain a localized or customized dashboard build, point
`gateway.controlUi.root` to a directory that contains your built static
assets and `index.html`.

```
mkdir -p "$HOME/.openclaw/control-ui-custom"
# Copy your built static files into that directory.
```

Then set:

```
{
  "gateway": {
    "controlUi": {
      "enabled": true,
      "root": "$HOME/.openclaw/control-ui-custom"
    }
  }
}
```

Restart the gateway and reopen the dashboard:

```
openclaw gateway restart
openclaw dashboard
```

## [​](https://docs.openclaw.ai/start/getting-started\#what-to-do-next)  What to do next

[**Connect a channel** \\
\\
Discord, Feishu, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, and more.](https://docs.openclaw.ai/channels)

[**Pairing and safety** \\
\\
Control who can message your agent.](https://docs.openclaw.ai/channels/pairing)

[**Configure the Gateway** \\
\\
Models, tools, sandbox, and advanced settings.](https://docs.openclaw.ai/gateway/configuration)

[**Browse tools** \\
\\
Browser, exec, web search, skills, and plugins.](https://docs.openclaw.ai/tools)

Advanced: environment variables

If you run OpenClaw as a service account or want custom paths:

- `OPENCLAW_HOME` — home directory for internal path resolution
- `OPENCLAW_STATE_DIR` — override the state directory
- `OPENCLAW_CONFIG_PATH` — override the config file path

Full reference: [Environment variables](https://docs.openclaw.ai/help/environment).

## [​](https://docs.openclaw.ai/start/getting-started\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [Channels overview](https://docs.openclaw.ai/channels)
- [Setup](https://docs.openclaw.ai/start/setup)

[Features](https://docs.openclaw.ai/concepts/features) [Onboarding Overview](https://docs.openclaw.ai/start/onboarding-overview)

Ctrl+I

![Install Script Process](https://mintcdn.com/clawdhub/U8jr7qEbUc9OU9YR/assets/install-script.svg?w=1100&fit=max&auto=format&n=U8jr7qEbUc9OU9YR&q=85&s=b5bc84222a0a894ebf01ab00b70c9ec4)