---
source_url: https://docs.openclaw.ai/providers/anthropic
title: "Anthropic - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/anthropic#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Anthropic

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/anthropic#getting-started)
- [Config example](https://docs.openclaw.ai/providers/anthropic#config-example)
- [Thinking defaults (Claude 4.6)](https://docs.openclaw.ai/providers/anthropic#thinking-defaults-claude-4-6)
- [Prompt caching](https://docs.openclaw.ai/providers/anthropic#prompt-caching)
- [Advanced configuration](https://docs.openclaw.ai/providers/anthropic#advanced-configuration)
- [Troubleshooting](https://docs.openclaw.ai/providers/anthropic#troubleshooting)
- [Related](https://docs.openclaw.ai/providers/anthropic#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Anthropic builds the **Claude** model family. OpenClaw supports two auth routes:

- **API key** — direct Anthropic API access with usage-based billing (`anthropic/*` models)
- **Claude CLI** — reuse an existing Claude CLI login on the same host

Anthropic staff told us OpenClaw-style Claude CLI usage is allowed again, so
OpenClaw treats Claude CLI reuse and `claude -p` usage as sanctioned unless
Anthropic publishes a new policy.For long-lived gateway hosts, Anthropic API keys are still the clearest and
most predictable production path.Anthropic’s current public docs:

- [Claude Code CLI reference](https://code.claude.com/docs/en/cli-reference)
- [Claude Agent SDK overview](https://platform.claude.com/docs/en/agent-sdk/overview)
- [Using Claude Code with your Pro or Max plan](https://support.claude.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan)
- [Using Claude Code with your Team or Enterprise plan](https://support.anthropic.com/en/articles/11845131-using-claude-code-with-your-team-or-enterprise-plan/)

## [​](https://docs.openclaw.ai/providers/anthropic\#getting-started)  Getting started

- API key

- Claude CLI


**Best for:** standard API access and usage-based billing.

1

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Get your API key

Create an API key in the [Anthropic Console](https://console.anthropic.com/).

2

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Run onboarding

```
openclaw onboard
# choose: Anthropic API key
```

Or pass the key directly:

```
openclaw onboard --anthropic-api-key "$ANTHROPIC_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Verify the model is available

```
openclaw models list --provider anthropic
```

### [​](https://docs.openclaw.ai/providers/anthropic\#config-example)  Config example

```
{
  env: { ANTHROPIC_API_KEY: "sk-ant-..." },
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

**Best for:** reusing an existing Claude CLI login without a separate API key.

1

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Ensure Claude CLI is installed and logged in

Verify with:

```
claude --version
```

2

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Run onboarding

```
openclaw onboard
# choose: Claude CLI
```

OpenClaw detects and reuses the existing Claude CLI credentials.

3

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Verify the model is available

```
openclaw models list --provider anthropic
```

Setup and runtime details for the Claude CLI backend are in [CLI Backends](https://docs.openclaw.ai/gateway/cli-backends).

### [​](https://docs.openclaw.ai/providers/anthropic\#config-example-2)  Config example

Prefer the canonical Anthropic model ref plus a CLI runtime override:

```
{
  agents: {
    defaults: {
      model: { primary: "anthropic/claude-opus-4-7" },
      agentRuntime: { id: "claude-cli" },
    },
  },
}
```

Legacy `claude-cli/claude-opus-4-7` model refs still work for
compatibility, but new config should keep provider/model selection as
`anthropic/*` and put the execution backend in `agentRuntime.id`.

If you want the clearest billing path, use an Anthropic API key instead. OpenClaw also supports subscription-style options from [OpenAI Codex](https://docs.openclaw.ai/providers/openai), [Qwen Cloud](https://docs.openclaw.ai/providers/qwen), [MiniMax](https://docs.openclaw.ai/providers/minimax), and [Z.AI / GLM](https://docs.openclaw.ai/providers/glm).

## [​](https://docs.openclaw.ai/providers/anthropic\#thinking-defaults-claude-4-6)  Thinking defaults (Claude 4.6)

Claude 4.6 models default to `adaptive` thinking in OpenClaw when no explicit thinking level is set.Override per-message with `/think:<level>` or in model params:

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": {
          params: { thinking: "adaptive" },
        },
      },
    },
  },
}
```

Related Anthropic docs:

- [Adaptive thinking](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking)
- [Extended thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking)

## [​](https://docs.openclaw.ai/providers/anthropic\#prompt-caching)  Prompt caching

OpenClaw supports Anthropic’s prompt caching feature for API-key auth.

| Value | Cache duration | Description |
| --- | --- | --- |
| `"short"` (default) | 5 minutes | Applied automatically for API-key auth |
| `"long"` | 1 hour | Extended cache |
| `"none"` | No caching | Disable prompt caching |

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": {
          params: { cacheRetention: "long" },
        },
      },
    },
  },
}
```

Per-agent cache overrides

Use model-level params as your baseline, then override specific agents via `agents.list[].params`:

```
{
  agents: {
    defaults: {
      model: { primary: "anthropic/claude-opus-4-6" },
      models: {
        "anthropic/claude-opus-4-6": {
          params: { cacheRetention: "long" },
        },
      },
    },
    list: [\
      { id: "research", default: true },\
      { id: "alerts", params: { cacheRetention: "none" } },\
    ],
  },
}
```

Config merge order:

1. `agents.defaults.models["provider/model"].params`
2. `agents.list[].params` (matching `id`, overrides by key)

This lets one agent keep a long-lived cache while another agent on the same model disables caching for bursty/low-reuse traffic.

Bedrock Claude notes

- Anthropic Claude models on Bedrock (`amazon-bedrock/*anthropic.claude*`) accept `cacheRetention` pass-through when configured.
- Non-Anthropic Bedrock models are forced to `cacheRetention: "none"` at runtime.
- API-key smart defaults also seed `cacheRetention: "short"` for Claude-on-Bedrock refs when no explicit value is set.

## [​](https://docs.openclaw.ai/providers/anthropic\#advanced-configuration)  Advanced configuration

Fast mode

OpenClaw’s shared `/fast` toggle supports direct Anthropic traffic (API-key and OAuth to `api.anthropic.com`).

| Command | Maps to |
| --- | --- |
| `/fast on` | `service_tier: "auto"` |
| `/fast off` | `service_tier: "standard_only"` |

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-sonnet-4-6": {
          params: { fastMode: true },
        },
      },
    },
  },
}
```

- Only injected for direct `api.anthropic.com` requests. Proxy routes leave `service_tier` untouched.
- Explicit `serviceTier` or `service_tier` params override `/fast` when both are set.
- On accounts without Priority Tier capacity, `service_tier: "auto"` may resolve to `standard`.

Media understanding (image and PDF)

The bundled Anthropic plugin registers image and PDF understanding. OpenClaw
auto-resolves media capabilities from the configured Anthropic auth — no
additional config is needed.

| Property | Value |
| --- | --- |
| Default model | `claude-opus-4-6` |
| Supported input | Images, PDF documents |

When an image or PDF is attached to a conversation, OpenClaw automatically
routes it through the Anthropic media understanding provider.

1M context window (beta)

Anthropic’s 1M context window is beta-gated. Enable it per model:

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": {
          params: { context1m: true },
        },
      },
    },
  },
}
```

OpenClaw maps this to `anthropic-beta: context-1m-2025-08-07` on requests.`params.context1m: true` also applies to the Claude CLI backend
(`claude-cli/*`) for eligible Opus and Sonnet models, expanding the runtime
context window for those CLI sessions to match the direct-API behavior.

Requires long-context access on your Anthropic credential. Legacy token auth (`sk-ant-oat-*`) is rejected for 1M context requests — OpenClaw logs a warning and falls back to the standard context window.

Claude Opus 4.7 1M context

`anthropic/claude-opus-4.7` and its `claude-cli` variant have a 1M context
window by default — no `params.context1m: true` needed.

## [​](https://docs.openclaw.ai/providers/anthropic\#troubleshooting)  Troubleshooting

401 errors / token suddenly invalid

Anthropic token auth expires and can be revoked. For new setups, use an Anthropic API key instead.

No API key found for provider "anthropic"

Anthropic auth is **per agent** — new agents do not inherit the main agent’s keys. Re-run onboarding for that agent (or configure an API key on the gateway host), then verify with `openclaw models status`.

No credentials found for profile "anthropic:default"

Run `openclaw models status` to see which auth profile is active. Re-run onboarding, or configure an API key for that profile path.

No available auth profile (all in cooldown)

Check `openclaw models status --json` for `auth.unusableProfiles`. Anthropic rate-limit cooldowns can be model-scoped, so a sibling Anthropic model may still be usable. Add another Anthropic profile or wait for cooldown.

More help: [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) and [FAQ](https://docs.openclaw.ai/help/faq).

## [​](https://docs.openclaw.ai/providers/anthropic\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**CLI backends** \\
\\
Claude CLI backend setup and runtime details.](https://docs.openclaw.ai/gateway/cli-backends)

[**Prompt caching** \\
\\
How prompt caching works across providers.](https://docs.openclaw.ai/reference/prompt-caching)

[**OAuth and auth** \\
\\
Auth details and credential reuse rules.](https://docs.openclaw.ai/gateway/authentication)

[Amazon Bedrock Mantle](https://docs.openclaw.ai/providers/bedrock-mantle) [Arcee AI](https://docs.openclaw.ai/providers/arcee)

Ctrl+I