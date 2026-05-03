---
source_url: https://docs.openclaw.ai/start/wizard-cli-automation
title: "CLI automation - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/start/wizard-cli-automation#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Guides

CLI automation

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Baseline non-interactive example](https://docs.openclaw.ai/start/wizard-cli-automation#baseline-non-interactive-example)
- [Provider-specific examples](https://docs.openclaw.ai/start/wizard-cli-automation#provider-specific-examples)
- [Add another agent](https://docs.openclaw.ai/start/wizard-cli-automation#add-another-agent)
- [Related docs](https://docs.openclaw.ai/start/wizard-cli-automation#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Use `--non-interactive` to automate `openclaw onboard`.

`--json` does not imply non-interactive mode. Use `--non-interactive` (and `--workspace`) for scripts.

## [​](https://docs.openclaw.ai/start/wizard-cli-automation\#baseline-non-interactive-example)  Baseline non-interactive example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice apiKey \
  --anthropic-api-key "$ANTHROPIC_API_KEY" \
  --secret-input-mode plaintext \
  --gateway-port 18789 \
  --gateway-bind loopback \
  --install-daemon \
  --daemon-runtime node \
  --skip-bootstrap \
  --skip-skills
```

Add `--json` for a machine-readable summary.Use `--skip-bootstrap` when your automation pre-seeds workspace files and does not want onboarding to create the default bootstrap files.Use `--secret-input-mode ref` to store env-backed refs in auth profiles instead of plaintext values.
Interactive selection between env refs and configured provider refs (`file` or `exec`) is available in the onboarding flow.In non-interactive `ref` mode, provider env vars must be set in the process environment.
Passing inline key flags without the matching env var now fails fast.Example:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice openai-api-key \
  --secret-input-mode ref \
  --accept-risk
```

## [​](https://docs.openclaw.ai/start/wizard-cli-automation\#provider-specific-examples)  Provider-specific examples

Anthropic API key example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice apiKey \
  --anthropic-api-key "$ANTHROPIC_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Gemini example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice gemini-api-key \
  --gemini-api-key "$GEMINI_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Z.AI example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice zai-api-key \
  --zai-api-key "$ZAI_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Vercel AI Gateway example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice ai-gateway-api-key \
  --ai-gateway-api-key "$AI_GATEWAY_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Cloudflare AI Gateway example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cloudflare-ai-gateway-api-key \
  --cloudflare-ai-gateway-account-id "your-account-id" \
  --cloudflare-ai-gateway-gateway-id "your-gateway-id" \
  --cloudflare-ai-gateway-api-key "$CLOUDFLARE_AI_GATEWAY_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Moonshot example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice moonshot-api-key \
  --moonshot-api-key "$MOONSHOT_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Mistral example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice mistral-api-key \
  --mistral-api-key "$MISTRAL_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Synthetic example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice synthetic-api-key \
  --synthetic-api-key "$SYNTHETIC_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

OpenCode example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice opencode-zen \
  --opencode-zen-api-key "$OPENCODE_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Swap to `--auth-choice opencode-go --opencode-go-api-key "$OPENCODE_API_KEY"` for the Go catalog.

Ollama example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice ollama \
  --custom-model-id "qwen3.5:27b" \
  --accept-risk \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Custom provider example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice custom-api-key \
  --custom-base-url "https://llm.example.com/v1" \
  --custom-model-id "foo-large" \
  --custom-api-key "$CUSTOM_API_KEY" \
  --custom-provider-id "my-custom" \
  --custom-compatibility anthropic \
  --custom-image-input \
  --gateway-port 18789 \
  --gateway-bind loopback
```

`--custom-api-key` is optional. If omitted, onboarding checks `CUSTOM_API_KEY`.
OpenClaw marks common vision model IDs as image-capable automatically. Add `--custom-image-input` for unknown custom vision IDs, or `--custom-text-input` to force text-only metadata.Ref-mode variant:

```
export CUSTOM_API_KEY="your-key"
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice custom-api-key \
  --custom-base-url "https://llm.example.com/v1" \
  --custom-model-id "foo-large" \
  --secret-input-mode ref \
  --custom-provider-id "my-custom" \
  --custom-compatibility anthropic \
  --custom-image-input \
  --gateway-port 18789 \
  --gateway-bind loopback
```

In this mode, onboarding stores `apiKey` as `{ source: "env", provider: "default", id: "CUSTOM_API_KEY" }`.

Anthropic setup-token remains available as a supported onboarding token path, but OpenClaw now prefers Claude CLI reuse when available.
For production, prefer an Anthropic API key.

## [​](https://docs.openclaw.ai/start/wizard-cli-automation\#add-another-agent)  Add another agent

Use `openclaw agents add <name>` to create a separate agent with its own workspace,
sessions, and auth profiles. Running without `--workspace` launches the wizard.

```
openclaw agents add work \
  --workspace ~/.openclaw/workspace-work \
  --model openai/gpt-5.5 \
  --bind whatsapp:biz \
  --non-interactive \
  --json
```

What it sets:

- `agents.list[].name`
- `agents.list[].workspace`
- `agents.list[].agentDir`

Notes:

- Default workspaces follow `~/.openclaw/workspace-<agentId>`.
- Add `bindings` to route inbound messages (the wizard can do this).
- Non-interactive flags: `--model`, `--agent-dir`, `--bind`, `--non-interactive`.

## [​](https://docs.openclaw.ai/start/wizard-cli-automation\#related-docs)  Related docs

- Onboarding hub: [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard)
- Full reference: [CLI Setup Reference](https://docs.openclaw.ai/start/wizard-cli-reference)
- Command reference: [`openclaw onboard`](https://docs.openclaw.ai/cli/onboard)

[CLI reference](https://docs.openclaw.ai/start/wizard-cli-reference)

Ctrl+I