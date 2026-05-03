---
source_url: https://docs.openclaw.ai/start/wizard
title: "Onboarding (CLI) - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/start/wizard#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

First steps

Onboarding (CLI)

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [QuickStart vs Advanced](https://docs.openclaw.ai/start/wizard#quickstart-vs-advanced)
- [What onboarding configures](https://docs.openclaw.ai/start/wizard#what-onboarding-configures)
- [Add another agent](https://docs.openclaw.ai/start/wizard#add-another-agent)
- [Full reference](https://docs.openclaw.ai/start/wizard#full-reference)
- [Related docs](https://docs.openclaw.ai/start/wizard#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

CLI onboarding is the **recommended** way to set up OpenClaw on macOS,
Linux, or Windows (via WSL2; strongly recommended).
It configures a local Gateway or a remote Gateway connection, plus channels, skills,
and workspace defaults in one guided flow.

```
openclaw onboard
```

Fastest first chat: open the Control UI (no channel setup needed). Run
`openclaw dashboard` and chat in the browser. Docs: [Dashboard](https://docs.openclaw.ai/web/dashboard).

To reconfigure later:

```
openclaw configure
openclaw agents add <name>
```

`--json` does not imply non-interactive mode. For scripts, use `--non-interactive`.

CLI onboarding includes a web search step where you can pick a provider
such as Brave, DuckDuckGo, Exa, Firecrawl, Gemini, Grok, Kimi, MiniMax Search,
Ollama Web Search, Perplexity, SearXNG, or Tavily. Some providers require an
API key, while others are key-free. You can also configure this later with
`openclaw configure --section web`. Docs: [Web tools](https://docs.openclaw.ai/tools/web).

## [​](https://docs.openclaw.ai/start/wizard\#quickstart-vs-advanced)  QuickStart vs Advanced

Onboarding starts with **QuickStart** (defaults) vs **Advanced** (full control).

- QuickStart (defaults)

- Advanced (full control)


- Local gateway (loopback)
- Workspace default (or existing workspace)
- Gateway port **18789**
- Gateway auth **Token** (auto‑generated, even on loopback)
- Tool policy default for new local setups: `tools.profile: "coding"` (existing explicit profile is preserved)
- DM isolation default: local onboarding writes `session.dmScope: "per-channel-peer"` when unset. Details: [CLI Setup Reference](https://docs.openclaw.ai/start/wizard-cli-reference#outputs-and-internals)
- Tailscale exposure **Off**
- Telegram + WhatsApp DMs default to **allowlist** (you’ll be prompted for your phone number)

- Exposes every step (mode, workspace, gateway, channels, daemon, skills).

## [​](https://docs.openclaw.ai/start/wizard\#what-onboarding-configures)  What onboarding configures

**Local mode (default)** walks you through these steps:

1. **Model/Auth** — choose any supported provider/auth flow (API key, OAuth, or provider-specific manual auth), including Custom Provider
(OpenAI-compatible, Anthropic-compatible, or Unknown auto-detect). Pick a default model.
Security note: if this agent will run tools or process webhook/hooks content, prefer the strongest latest-generation model available and keep tool policy strict. Weaker/older tiers are easier to prompt-inject.
For non-interactive runs, `--secret-input-mode ref` stores env-backed refs in auth profiles instead of plaintext API key values.
In non-interactive `ref` mode, the provider env var must be set; passing inline key flags without that env var fails fast.
In interactive runs, choosing secret reference mode lets you point at either an environment variable or a configured provider ref (`file` or `exec`), with a fast preflight validation before saving.
For Anthropic, interactive onboarding/configure offers **Anthropic Claude CLI** as the preferred local path and **Anthropic API key** as the recommended production path. Anthropic setup-token also remains available as a supported token-auth path.
2. **Workspace** — Location for agent files (default `~/.openclaw/workspace`). Seeds bootstrap files.
3. **Gateway** — Port, bind address, auth mode, Tailscale exposure.
In interactive token mode, choose default plaintext token storage or opt into SecretRef.
Non-interactive token SecretRef path: `--gateway-token-ref-env <ENV_VAR>`.
4. **Channels** — built-in and bundled chat channels such as BlueBubbles, Discord, Feishu, Google Chat, Mattermost, Microsoft Teams, QQ Bot, Signal, Slack, Telegram, WhatsApp, and more.
5. **Daemon** — Installs a LaunchAgent (macOS), systemd user unit (Linux/WSL2), or native Windows Scheduled Task with per-user Startup-folder fallback.
If token auth requires a token and `gateway.auth.token` is SecretRef-managed, daemon install validates it but does not persist the resolved token into supervisor service environment metadata.
If token auth requires a token and the configured token SecretRef is unresolved, daemon install is blocked with actionable guidance.
If both `gateway.auth.token` and `gateway.auth.password` are configured and `gateway.auth.mode` is unset, daemon install is blocked until mode is set explicitly.
6. **Health check** — Starts the Gateway and verifies it’s running.
7. **Skills** — Installs recommended skills and optional dependencies.

Re-running onboarding does **not** wipe anything unless you explicitly choose **Reset** (or pass `--reset`).
CLI `--reset` defaults to config, credentials, and sessions; use `--reset-scope full` to include workspace.
If the config is invalid or contains legacy keys, onboarding asks you to run `openclaw doctor` first.

**Remote mode** only configures the local client to connect to a Gateway elsewhere.
It does **not** install or change anything on the remote host.

## [​](https://docs.openclaw.ai/start/wizard\#add-another-agent)  Add another agent

Use `openclaw agents add <name>` to create a separate agent with its own workspace,
sessions, and auth profiles. Running without `--workspace` launches onboarding.What it sets:

- `agents.list[].name`
- `agents.list[].workspace`
- `agents.list[].agentDir`

Notes:

- Default workspaces follow `~/.openclaw/workspace-<agentId>`.
- Add `bindings` to route inbound messages (onboarding can do this).
- Non-interactive flags: `--model`, `--agent-dir`, `--bind`, `--non-interactive`.

## [​](https://docs.openclaw.ai/start/wizard\#full-reference)  Full reference

For detailed step-by-step breakdowns and config outputs, see
[CLI Setup Reference](https://docs.openclaw.ai/start/wizard-cli-reference).
For non-interactive examples, see [CLI Automation](https://docs.openclaw.ai/start/wizard-cli-automation).
For the deeper technical reference, including RPC details, see
[Onboarding Reference](https://docs.openclaw.ai/reference/wizard).

## [​](https://docs.openclaw.ai/start/wizard\#related-docs)  Related docs

- CLI command reference: [`openclaw onboard`](https://docs.openclaw.ai/cli/onboard)
- Onboarding overview: [Onboarding Overview](https://docs.openclaw.ai/start/onboarding-overview)
- macOS app onboarding: [Onboarding](https://docs.openclaw.ai/start/onboarding)
- Agent first-run ritual: [Agent Bootstrapping](https://docs.openclaw.ai/start/bootstrapping)

[Onboarding Overview](https://docs.openclaw.ai/start/onboarding-overview) [Onboarding: macOS App](https://docs.openclaw.ai/start/onboarding)

Ctrl+I