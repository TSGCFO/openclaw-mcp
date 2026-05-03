---
source_url: https://docs.openclaw.ai/providers/claude-max-api-proxy
title: "Claude Max API proxy - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/claude-max-api-proxy#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Claude Max API proxy

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Why use this?](https://docs.openclaw.ai/providers/claude-max-api-proxy#why-use-this)
- [How it works](https://docs.openclaw.ai/providers/claude-max-api-proxy#how-it-works)
- [Getting started](https://docs.openclaw.ai/providers/claude-max-api-proxy#getting-started)
- [Built-in catalog](https://docs.openclaw.ai/providers/claude-max-api-proxy#built-in-catalog)
- [Advanced configuration](https://docs.openclaw.ai/providers/claude-max-api-proxy#advanced-configuration)
- [Links](https://docs.openclaw.ai/providers/claude-max-api-proxy#links)
- [Notes](https://docs.openclaw.ai/providers/claude-max-api-proxy#notes)
- [Related](https://docs.openclaw.ai/providers/claude-max-api-proxy#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

**claude-max-api-proxy** is a community tool that exposes your Claude Max/Pro subscription as an OpenAI-compatible API endpoint. This allows you to use your subscription with any tool that supports the OpenAI API format.

This path is technical compatibility only. Anthropic has blocked some subscription
usage outside Claude Code in the past. You must decide for yourself whether to use
it and verify Anthropic’s current terms before relying on it.

## [​](https://docs.openclaw.ai/providers/claude-max-api-proxy\#why-use-this)  Why use this?

| Approach | Cost | Best For |
| --- | --- | --- |
| Anthropic API | Pay per token (~15/Minput,15/M input, 15/Minput,75/M output for Opus) | Production apps, high volume |
| Claude Max subscription | $200/month flat | Personal use, development, unlimited usage |

If you have a Claude Max subscription and want to use it with OpenAI-compatible tools, this proxy may reduce cost for some workflows. API keys remain the clearer policy path for production use.

## [​](https://docs.openclaw.ai/providers/claude-max-api-proxy\#how-it-works)  How it works

```
Your App → claude-max-api-proxy → Claude Code CLI → Anthropic (via subscription)
     (OpenAI format)              (converts format)      (uses your login)
```

The proxy:

1. Accepts OpenAI-format requests at `http://localhost:3456/v1/chat/completions`
2. Converts them to Claude Code CLI commands
3. Returns responses in OpenAI format (streaming supported)

## [​](https://docs.openclaw.ai/providers/claude-max-api-proxy\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Install the proxy

Requires Node.js 20+ and Claude Code CLI.

```
npm install -g claude-max-api-proxy

# Verify Claude CLI is authenticated
claude --version
```

2

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Start the server

```
claude-max-api
# Server runs at http://localhost:3456
```

3

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Test the proxy

```
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

4

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Configure OpenClaw

Point OpenClaw at the proxy as a custom OpenAI-compatible endpoint:

```
{
  env: {
    OPENAI_API_KEY: "not-needed",
    OPENAI_BASE_URL: "http://localhost:3456/v1",
  },
  agents: {
    defaults: {
      model: { primary: "openai/claude-opus-4" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/claude-max-api-proxy\#built-in-catalog)  Built-in catalog

| Model ID | Maps To |
| --- | --- |
| `claude-opus-4` | Claude Opus 4 |
| `claude-sonnet-4` | Claude Sonnet 4 |
| `claude-haiku-4` | Claude Haiku 4 |

## [​](https://docs.openclaw.ai/providers/claude-max-api-proxy\#advanced-configuration)  Advanced configuration

Proxy-style OpenAI-compatible notes

This path uses the same proxy-style OpenAI-compatible route as other custom
`/v1` backends:

- Native OpenAI-only request shaping does not apply
- No `service_tier`, no Responses `store`, no prompt-cache hints, and no
OpenAI reasoning-compat payload shaping
- Hidden OpenClaw attribution headers (`originator`, `version`, `User-Agent`)
are not injected on the proxy URL

Auto-start on macOS with LaunchAgent

Create a LaunchAgent to run the proxy automatically:

```
cat > ~/Library/LaunchAgents/com.claude-max-api.plist << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.claude-max-api</string>
  <key>RunAtLoad</key>
  <true/>
  <key>KeepAlive</key>
  <true/>
  <key>ProgramArguments</key>
  <array>
    <string>/usr/local/bin/node</string>
    <string>/usr/local/lib/node_modules/claude-max-api-proxy/dist/server/standalone.js</string>
  </array>
  <key>EnvironmentVariables</key>
  <dict>
    <key>PATH</key>
    <string>/usr/local/bin:/opt/homebrew/bin:~/.local/bin:/usr/bin:/bin</string>
  </dict>
</dict>
</plist>
EOF

launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.claude-max-api.plist
```

## [​](https://docs.openclaw.ai/providers/claude-max-api-proxy\#links)  Links

- **npm:** [https://www.npmjs.com/package/claude-max-api-proxy](https://www.npmjs.com/package/claude-max-api-proxy)
- **GitHub:** [https://github.com/atalovesyou/claude-max-api-proxy](https://github.com/atalovesyou/claude-max-api-proxy)
- **Issues:** [https://github.com/atalovesyou/claude-max-api-proxy/issues](https://github.com/atalovesyou/claude-max-api-proxy/issues)

## [​](https://docs.openclaw.ai/providers/claude-max-api-proxy\#notes)  Notes

- This is a **community tool**, not officially supported by Anthropic or OpenClaw
- Requires an active Claude Max/Pro subscription with Claude Code CLI authenticated
- The proxy runs locally and does not send data to any third-party servers
- Streaming responses are fully supported

For native Anthropic integration with Claude CLI or API keys, see [Anthropic provider](https://docs.openclaw.ai/providers/anthropic). For OpenAI/Codex subscriptions, see [OpenAI provider](https://docs.openclaw.ai/providers/openai).

## [​](https://docs.openclaw.ai/providers/claude-max-api-proxy\#related)  Related

[**Anthropic provider** \\
\\
Native OpenClaw integration with Claude CLI or API keys.](https://docs.openclaw.ai/providers/anthropic)

[**OpenAI provider** \\
\\
For OpenAI/Codex subscriptions.](https://docs.openclaw.ai/providers/openai)

[**Model selection** \\
\\
Overview of all providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration** \\
\\
Full config reference.](https://docs.openclaw.ai/gateway/configuration)

[Chutes](https://docs.openclaw.ai/providers/chutes) [Cloudflare AI gateway](https://docs.openclaw.ai/providers/cloudflare-ai-gateway)

Ctrl+I