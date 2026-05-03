---
source_url: https://docs.openclaw.ai/start/wizard-cli-reference
title: "CLI setup reference - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/start/wizard-cli-reference#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Guides

CLI setup reference

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What the wizard does](https://docs.openclaw.ai/start/wizard-cli-reference#what-the-wizard-does)
- [Local flow details](https://docs.openclaw.ai/start/wizard-cli-reference#local-flow-details)
- [Remote mode details](https://docs.openclaw.ai/start/wizard-cli-reference#remote-mode-details)
- [Auth and model options](https://docs.openclaw.ai/start/wizard-cli-reference#auth-and-model-options)
- [Outputs and internals](https://docs.openclaw.ai/start/wizard-cli-reference#outputs-and-internals)
- [Related docs](https://docs.openclaw.ai/start/wizard-cli-reference#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

This page is the full reference for `openclaw onboard`.
For the short guide, see [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard).

## [​](https://docs.openclaw.ai/start/wizard-cli-reference\#what-the-wizard-does)  What the wizard does

Local mode (default) walks you through:

- Model and auth setup (OpenAI Code subscription OAuth, Anthropic Claude CLI or API key, plus MiniMax, GLM, Ollama, Moonshot, StepFun, and AI Gateway options)
- Workspace location and bootstrap files
- Gateway settings (port, bind, auth, tailscale)
- Channels and providers (Telegram, WhatsApp, Discord, Google Chat, Mattermost, Signal, BlueBubbles, and other bundled channel plugins)
- Daemon install (LaunchAgent, systemd user unit, or native Windows Scheduled Task with Startup-folder fallback)
- Health check
- Skills setup

Remote mode configures this machine to connect to a gateway elsewhere.
It does not install or modify anything on the remote host.

## [​](https://docs.openclaw.ai/start/wizard-cli-reference\#local-flow-details)  Local flow details

1

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Existing config detection

- If `~/.openclaw/openclaw.json` exists, choose Keep, Modify, or Reset.
- Re-running the wizard does not wipe anything unless you explicitly choose Reset (or pass `--reset`).
- CLI `--reset` defaults to `config+creds+sessions`; use `--reset-scope full` to also remove workspace.
- If config is invalid or contains legacy keys, the wizard stops and asks you to run `openclaw doctor` before continuing.
- Reset uses `trash` and offers scopes:

  - Config only
  - Config + credentials + sessions
  - Full reset (also removes workspace)

2

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Model and auth

- Full option matrix is in [Auth and model options](https://docs.openclaw.ai/start/wizard-cli-reference#auth-and-model-options).

3

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Workspace

- Default `~/.openclaw/workspace` (configurable).
- Seeds workspace files needed for first-run bootstrap ritual.
- Workspace layout: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace).

4

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Gateway

- Prompts for port, bind, auth mode, and tailscale exposure.
- Recommended: keep token auth enabled even for loopback so local WS clients must authenticate.
- In token mode, interactive setup offers:
  - **Generate/store plaintext token** (default)
  - **Use SecretRef** (opt-in)
- In password mode, interactive setup also supports plaintext or SecretRef storage.
- Non-interactive token SecretRef path: `--gateway-token-ref-env <ENV_VAR>`.

  - Requires a non-empty env var in the onboarding process environment.
  - Cannot be combined with `--gateway-token`.
- Disable auth only if you fully trust every local process.
- Non-loopback binds still require auth.

5

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Channels

- [WhatsApp](https://docs.openclaw.ai/channels/whatsapp): optional QR login
- [Telegram](https://docs.openclaw.ai/channels/telegram): bot token
- [Discord](https://docs.openclaw.ai/channels/discord): bot token
- [Google Chat](https://docs.openclaw.ai/channels/googlechat): service account JSON + webhook audience
- [Mattermost](https://docs.openclaw.ai/channels/mattermost): bot token + base URL
- [Signal](https://docs.openclaw.ai/channels/signal): optional `signal-cli` install + account config
- [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles): recommended for iMessage; server URL + password + webhook
- [iMessage](https://docs.openclaw.ai/channels/imessage): legacy `imsg` CLI path + DB access
- DM security: default is pairing. First DM sends a code; approve via
`openclaw pairing approve <channel> <code>` or use allowlists.

6

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Daemon install

- macOS: LaunchAgent
  - Requires logged-in user session; for headless, use a custom LaunchDaemon (not shipped).
- Linux and Windows via WSL2: systemd user unit
  - Wizard attempts `loginctl enable-linger <user>` so gateway stays up after logout.
  - May prompt for sudo (writes `/var/lib/systemd/linger`); it tries without sudo first.
- Native Windows: Scheduled Task first
  - If task creation is denied, OpenClaw falls back to a per-user Startup-folder login item and starts the gateway immediately.
  - Scheduled Tasks remain preferred because they provide better supervisor status.
- Runtime selection: Node (recommended; required for WhatsApp and Telegram). Bun is not recommended.

7

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Health check

- Starts gateway (if needed) and runs `openclaw health`.
- `openclaw status --deep` adds the live gateway health probe to status output, including channel probes when supported.

8

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Skills

- Reads available skills and checks requirements.
- Lets you choose node manager: npm, pnpm, or bun.
- Installs optional dependencies (some use Homebrew on macOS).

9

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Finish

- Summary and next steps, including iOS, Android, and macOS app options.

If no GUI is detected, the wizard prints SSH port-forward instructions for the Control UI instead of opening a browser.
If Control UI assets are missing, the wizard attempts to build them; fallback is `pnpm ui:build` (auto-installs UI deps).

## [​](https://docs.openclaw.ai/start/wizard-cli-reference\#remote-mode-details)  Remote mode details

Remote mode configures this machine to connect to a gateway elsewhere.

Remote mode does not install or modify anything on the remote host.

What you set:

- Remote gateway URL (`ws://...`)
- Token if remote gateway auth is required (recommended)

- If gateway is loopback-only, use SSH tunneling or a tailnet.
- Discovery hints:
  - macOS: Bonjour (`dns-sd`)
  - Linux: Avahi (`avahi-browse`)

## [​](https://docs.openclaw.ai/start/wizard-cli-reference\#auth-and-model-options)  Auth and model options

Anthropic API key

Uses `ANTHROPIC_API_KEY` if present or prompts for a key, then saves it for daemon use.

OpenAI Code subscription (OAuth)

Browser flow; paste `code#state`.Sets `agents.defaults.model` to `openai-codex/gpt-5.5` when model is unset or already OpenAI-family.

OpenAI Code subscription (device pairing)

Browser pairing flow with a short-lived device code.Sets `agents.defaults.model` to `openai-codex/gpt-5.5` when model is unset or already OpenAI-family.

OpenAI API key

Uses `OPENAI_API_KEY` if present or prompts for a key, then stores the credential in auth profiles.Sets `agents.defaults.model` to `openai/gpt-5.5` when model is unset, `openai/*`, or `openai-codex/*`.

xAI (Grok) API key

Prompts for `XAI_API_KEY` and configures xAI as a model provider.

OpenCode

Prompts for `OPENCODE_API_KEY` (or `OPENCODE_ZEN_API_KEY`) and lets you choose the Zen or Go catalog.
Setup URL: [opencode.ai/auth](https://opencode.ai/auth).

API key (generic)

Stores the key for you.

Vercel AI Gateway

Prompts for `AI_GATEWAY_API_KEY`.
More detail: [Vercel AI Gateway](https://docs.openclaw.ai/providers/vercel-ai-gateway).

Cloudflare AI Gateway

Prompts for account ID, gateway ID, and `CLOUDFLARE_AI_GATEWAY_API_KEY`.
More detail: [Cloudflare AI Gateway](https://docs.openclaw.ai/providers/cloudflare-ai-gateway).

MiniMax

Config is auto-written. Hosted default is `MiniMax-M2.7`; API-key setup uses
`minimax/...`, and OAuth setup uses `minimax-portal/...`.
More detail: [MiniMax](https://docs.openclaw.ai/providers/minimax).

StepFun

Config is auto-written for StepFun standard or Step Plan on China or global endpoints.
Standard currently includes `step-3.5-flash`, and Step Plan also includes `step-3.5-flash-2603`.
More detail: [StepFun](https://docs.openclaw.ai/providers/stepfun).

Synthetic (Anthropic-compatible)

Prompts for `SYNTHETIC_API_KEY`.
More detail: [Synthetic](https://docs.openclaw.ai/providers/synthetic).

Ollama (Cloud and local open models)

Prompts for `Cloud + Local`, `Cloud only`, or `Local only` first.
`Cloud only` uses `OLLAMA_API_KEY` with `https://ollama.com`.
The host-backed modes prompt for base URL (default `http://127.0.0.1:11434`), discover available models, and suggest defaults.
`Cloud + Local` also checks whether that Ollama host is signed in for cloud access.
More detail: [Ollama](https://docs.openclaw.ai/providers/ollama).

Moonshot and Kimi Coding

Moonshot (Kimi K2) and Kimi Coding configs are auto-written.
More detail: [Moonshot AI (Kimi + Kimi Coding)](https://docs.openclaw.ai/providers/moonshot).

Custom provider

Works with OpenAI-compatible and Anthropic-compatible endpoints.Interactive onboarding supports the same API key storage choices as other provider API key flows:

- **Paste API key now** (plaintext)
- **Use secret reference** (env ref or configured provider ref, with preflight validation)

Non-interactive flags:

- `--auth-choice custom-api-key`
- `--custom-base-url`
- `--custom-model-id`
- `--custom-api-key` (optional; falls back to `CUSTOM_API_KEY`)
- `--custom-provider-id` (optional)
- `--custom-compatibility <openai|anthropic>` (optional; default `openai`)
- `--custom-image-input` / `--custom-text-input` (optional; override inferred model input capability)

Skip

Leaves auth unconfigured.

Model behavior:

- Pick default model from detected options, or enter provider and model manually.
- Custom-provider onboarding infers image support for common model IDs and asks only when the model name is unknown.
- When onboarding starts from a provider auth choice, the model picker prefers
that provider automatically. For Volcengine and BytePlus, the same preference
also matches their coding-plan variants (`volcengine-plan/*`,
`byteplus-plan/*`).
- If that preferred-provider filter would be empty, the picker falls back to
the full catalog instead of showing no models.
- Wizard runs a model check and warns if the configured model is unknown or missing auth.

Credential and profile paths:

- Auth profiles (API keys + OAuth): `~/.openclaw/agents/<agentId>/agent/auth-profiles.json`
- Legacy OAuth import: `~/.openclaw/credentials/oauth.json`

Credential storage mode:

- Default onboarding behavior persists API keys as plaintext values in auth profiles.
- `--secret-input-mode ref`enables reference mode instead of plaintext key storage.
In interactive setup, you can choose either:

  - environment variable ref (for example `keyRef: { source: "env", provider: "default", id: "OPENAI_API_KEY" }`)
  - configured provider ref (`file` or `exec`) with provider alias + id
- Interactive reference mode runs a fast preflight validation before saving.
  - Env refs: validates variable name + non-empty value in the current onboarding environment.
  - Provider refs: validates provider config and resolves the requested id.
  - If preflight fails, onboarding shows the error and lets you retry.
- In non-interactive mode, `--secret-input-mode ref` is env-backed only.

  - Set the provider env var in the onboarding process environment.
  - Inline key flags (for example `--openai-api-key`) require that env var to be set; otherwise onboarding fails fast.
  - For custom providers, non-interactive `ref` mode stores `models.providers.<id>.apiKey` as `{ source: "env", provider: "default", id: "CUSTOM_API_KEY" }`.
  - In that custom-provider case, `--custom-api-key` requires `CUSTOM_API_KEY` to be set; otherwise onboarding fails fast.
- Gateway auth credentials support plaintext and SecretRef choices in interactive setup:
  - Token mode: **Generate/store plaintext token** (default) or **Use SecretRef**.
  - Password mode: plaintext or SecretRef.
- Non-interactive token SecretRef path: `--gateway-token-ref-env <ENV_VAR>`.
- Existing plaintext setups continue to work unchanged.

Headless and server tip: complete OAuth on a machine with a browser, then copy
that agent’s `auth-profiles.json` (for example
`~/.openclaw/agents/<agentId>/agent/auth-profiles.json`, or the matching
`$OPENCLAW_STATE_DIR/...` path) to the gateway host. `credentials/oauth.json`
is only a legacy import source.

## [​](https://docs.openclaw.ai/start/wizard-cli-reference\#outputs-and-internals)  Outputs and internals

Typical fields in `~/.openclaw/openclaw.json`:

- `agents.defaults.workspace`
- `agents.defaults.skipBootstrap` when `--skip-bootstrap` is passed
- `agents.defaults.model` / `models.providers` (if Minimax chosen)
- `tools.profile` (local onboarding defaults to `"coding"` when unset; existing explicit values are preserved)
- `gateway.*` (mode, bind, auth, tailscale)
- `session.dmScope` (local onboarding defaults this to `per-channel-peer` when unset; existing explicit values are preserved)
- `channels.telegram.botToken`, `channels.discord.token`, `channels.matrix.*`, `channels.signal.*`, `channels.imessage.*`
- Channel allowlists (Slack, Discord, Matrix, Microsoft Teams) when you opt in during prompts (names resolve to IDs when possible)
- `skills.install.nodeManager`
  - The `setup --node-manager` flag accepts `npm`, `pnpm`, or `bun`.
  - Manual config can still set `skills.install.nodeManager: "yarn"` later.
- `wizard.lastRunAt`
- `wizard.lastRunVersion`
- `wizard.lastRunCommit`
- `wizard.lastRunCommand`
- `wizard.lastRunMode`

`openclaw agents add` writes `agents.list[]` and optional `bindings`.WhatsApp credentials go under `~/.openclaw/credentials/whatsapp/<accountId>/`.
Sessions are stored under `~/.openclaw/agents/<agentId>/sessions/`.

Some channels are delivered as plugins. When selected during setup, the wizard
prompts to install the plugin (npm or local path) before channel configuration.

Gateway wizard RPC:

- `wizard.start`
- `wizard.next`
- `wizard.cancel`
- `wizard.status`

Clients (macOS app and Control UI) can render steps without re-implementing onboarding logic.Signal setup behavior:

- Downloads the appropriate release asset
- Stores it under `~/.openclaw/tools/signal-cli/<version>/`
- Writes `channels.signal.cliPath` in config
- JVM builds require Java 21
- Native builds are used when available
- Windows uses WSL2 and follows Linux signal-cli flow inside WSL

## [​](https://docs.openclaw.ai/start/wizard-cli-reference\#related-docs)  Related docs

- Onboarding hub: [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard)
- Automation and scripts: [CLI Automation](https://docs.openclaw.ai/start/wizard-cli-automation)
- Command reference: [`openclaw onboard`](https://docs.openclaw.ai/cli/onboard)

[Personal assistant setup](https://docs.openclaw.ai/start/openclaw) [CLI automation](https://docs.openclaw.ai/start/wizard-cli-automation)

Ctrl+I