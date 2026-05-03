---
source_url: https://docs.openclaw.ai/cli/configure
title: "Configure - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/configure#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Configuration

Configure

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw configure](https://docs.openclaw.ai/cli/configure#openclaw-configure)
- [Options](https://docs.openclaw.ai/cli/configure#options)
- [Examples](https://docs.openclaw.ai/cli/configure#examples)
- [Related](https://docs.openclaw.ai/cli/configure#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/configure\#openclaw-configure)  `openclaw configure`

Interactive prompt to set up credentials, devices, and agent defaults.

The **Model** section includes a multi-select for the `agents.defaults.models` allowlist (what shows up in `/model` and the model picker). Provider-scoped setup choices merge their selected models into the existing allowlist instead of replacing unrelated providers already in the config.Re-running provider auth from configure preserves an existing `agents.defaults.model.primary`, even when the provider’s auth step returns a config patch with its own recommended default model. That means adding or reauthing xAI, OpenRouter, or another provider should make the new model available without taking over from your current primary model. Use `openclaw models auth login --provider <id> --set-default` or `openclaw models set <model>` when you intentionally want to change the default model.

When configure starts from a provider auth choice, the default-model and allowlist pickers prefer that provider automatically. For paired providers such as Volcengine and BytePlus, the same preference also matches their coding-plan variants (`volcengine-plan/*`, `byteplus-plan/*`). If the preferred-provider filter would produce an empty list, configure falls back to the unfiltered catalog instead of showing a blank picker.

`openclaw config` without a subcommand opens the same wizard. Use `openclaw config get|set|unset` for non-interactive edits.

For web search, `openclaw configure --section web` lets you choose a provider
and configure its credentials. Some providers also show provider-specific
follow-up prompts:

- **Grok** can offer optional `x_search` setup with the same `XAI_API_KEY` and
let you pick an `x_search` model.
- **Kimi** can ask for the Moonshot API region (`api.moonshot.ai` vs
`api.moonshot.cn`) and the default Kimi web-search model.

Related:

- Gateway configuration reference: [Configuration](https://docs.openclaw.ai/gateway/configuration)
- Config CLI: [Config](https://docs.openclaw.ai/cli/config)

## [​](https://docs.openclaw.ai/cli/configure\#options)  Options

- `--section <section>`: repeatable section filter

Available sections:

- `workspace`
- `model`
- `web`
- `gateway`
- `daemon`
- `channels`
- `plugins`
- `skills`
- `health`

Notes:

- Choosing where the Gateway runs always updates `gateway.mode`. You can select “Continue” without other sections if that is all you need.
- After local config writes, configure installs selected downloadable plugins when the chosen setup path requires them. Remote gateway config does not install local plugin packages.
- Channel-oriented services (Slack/Discord/Matrix/Microsoft Teams) prompt for channel/room allowlists during setup. You can enter names or IDs; the wizard resolves names to IDs when possible.
- If you run the daemon install step, token auth requires a token, and `gateway.auth.token` is SecretRef-managed, configure validates the SecretRef but does not persist resolved plaintext token values into supervisor service environment metadata.
- If token auth requires a token and the configured token SecretRef is unresolved, configure blocks daemon install with actionable remediation guidance.
- If both `gateway.auth.token` and `gateway.auth.password` are configured and `gateway.auth.mode` is unset, configure blocks daemon install until mode is set explicitly.

## [​](https://docs.openclaw.ai/cli/configure\#examples)  Examples

```
openclaw configure
openclaw configure --section web
openclaw configure --section model --section channels
openclaw configure --section gateway --section daemon
```

## [​](https://docs.openclaw.ai/cli/configure\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)

[Config](https://docs.openclaw.ai/cli/config) [Webhooks](https://docs.openclaw.ai/cli/webhooks)

Ctrl+I