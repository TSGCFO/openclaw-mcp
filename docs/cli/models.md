---
source_url: https://docs.openclaw.ai/cli/models
title: "Models - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/models#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Agents and sessions

Models

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw models](https://docs.openclaw.ai/cli/models#openclaw-models)
- [Common commands](https://docs.openclaw.ai/cli/models#common-commands)
- [Models scan](https://docs.openclaw.ai/cli/models#models-scan)
- [Models status](https://docs.openclaw.ai/cli/models#models-status)
- [Aliases + fallbacks](https://docs.openclaw.ai/cli/models#aliases-%2B-fallbacks)
- [Auth profiles](https://docs.openclaw.ai/cli/models#auth-profiles)
- [Related](https://docs.openclaw.ai/cli/models#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/models\#openclaw-models)  `openclaw models`

Model discovery, scanning, and configuration (default model, fallbacks, auth profiles).Related:

- Providers + models: [Models](https://docs.openclaw.ai/providers/models)
- Model selection concepts + `/models` slash command: [Models concept](https://docs.openclaw.ai/concepts/models)
- Provider auth setup: [Getting started](https://docs.openclaw.ai/start/getting-started)

## [​](https://docs.openclaw.ai/cli/models\#common-commands)  Common commands

```
openclaw models status
openclaw models list
openclaw models set <model-or-alias>
openclaw models scan
```

`openclaw models status` shows the resolved default/fallbacks plus an auth overview.
When provider usage snapshots are available, the OAuth/API-key status section includes
provider usage windows and quota snapshots.
Current usage-window providers: Anthropic, GitHub Copilot, Gemini CLI, OpenAI
Codex, MiniMax, Xiaomi, and z.ai. Usage auth comes from provider-specific hooks
when available; otherwise OpenClaw falls back to matching OAuth/API-key
credentials from auth profiles, env, or config.
In `--json` output, `auth.providers` is the env/config/store-aware provider
overview, while `auth.oauth` is auth-store profile health only.
Add `--probe` to run live auth probes against each configured provider profile.
Probes are real requests (may consume tokens and trigger rate limits).
Use `--agent <id>` to inspect a configured agent’s model/auth state. When omitted,
the command uses `OPENCLAW_AGENT_DIR`/`PI_CODING_AGENT_DIR` if set, otherwise the
configured default agent.
Probe rows can come from auth profiles, env credentials, or `models.json`.Notes:

- `models set <model-or-alias>` accepts `provider/model` or an alias.
- `models list` is read-only: it reads config, auth profiles, existing catalog
state, and provider-owned catalog rows, but it does not rewrite
`models.json`.
- The `Auth` column is provider-level and read-only. It is computed from local
auth profile metadata, env markers, configured provider keys, local-provider
markers, AWS Bedrock env/profile markers, and plugin synthetic-auth metadata;
it does not load provider runtime, read keychain secrets, call provider
APIs, or prove exact per-model execution readiness.
- `models list --all --provider <id>` can include provider-owned static catalog
rows from plugin manifests or bundled provider catalog metadata even when you
have not authenticated with that provider yet. Those rows still show as
unavailable until matching auth is configured.
- `models list` keeps the control plane responsive while provider catalog
discovery is slow. The default and configured views fall back to configured or
synthetic model rows after a short wait and let discovery finish in the
background. Use `--all` when you need the exact full discovered catalog and
are willing to wait for provider discovery.
- Broad `models list --all` merges manifest catalog rows over registry rows
without loading provider runtime supplement hooks. Provider-filtered manifest
fast paths use only providers marked `static`; providers marked `refreshable`
stay registry/cache-backed and append manifest rows as supplements, while
providers marked `runtime` stay on registry/runtime discovery.
- `models list` keeps native model metadata and runtime caps distinct. In table
output, `Ctx` shows `contextTokens/contextWindow` when an effective runtime
cap differs from the native context window; JSON rows include `contextTokens`
when a provider exposes that cap.
- `models list --provider <id>` filters by provider id, such as `moonshot` or
`openai-codex`. It does not accept display labels from interactive provider
pickers, such as `Moonshot AI`.
- Model refs are parsed by splitting on the **first**`/`. If the model ID includes `/` (OpenRouter-style), include the provider prefix (example: `openrouter/moonshotai/kimi-k2`).
- If you omit the provider, OpenClaw resolves the input as an alias first, then
as a unique configured-provider match for that exact model id, and only then
falls back to the configured default provider with a deprecation warning.
If that provider no longer exposes the configured default model, OpenClaw
falls back to the first configured provider/model instead of surfacing a
stale removed-provider default.
- `models status` may show `marker(<value>)` in auth output for non-secret placeholders (for example `OPENAI_API_KEY`, `secretref-managed`, `minimax-oauth`, `oauth:chutes`, `ollama-local`) instead of masking them as secrets.

### [​](https://docs.openclaw.ai/cli/models\#models-scan)  Models scan

`models scan` reads OpenRouter’s public `:free` catalog and ranks candidates for
fallback use. The catalog itself is public, so metadata-only scans do not need
an OpenRouter key.By default OpenClaw tries to probe tool and image support with live model calls.
If no OpenRouter key is configured, the command falls back to metadata-only
output and explains that `:free` models still require `OPENROUTER_API_KEY` for
probes and inference.Options:

- `--no-probe` (metadata only; no config/secrets lookup)
- `--min-params <b>`
- `--max-age-days <days>`
- `--provider <name>`
- `--max-candidates <n>`
- `--timeout <ms>` (catalog request and per-probe timeout)
- `--concurrency <n>`
- `--yes`
- `--no-input`
- `--set-default`
- `--set-image`
- `--json`

`--set-default` and `--set-image` require live probes; metadata-only scan
results are informational and are not applied to config.

### [​](https://docs.openclaw.ai/cli/models\#models-status)  Models status

Options:

- `--json`
- `--plain`
- `--check` (exit 1=expired/missing, 2=expiring)
- `--probe` (live probe of configured auth profiles)
- `--probe-provider <name>` (probe one provider)
- `--probe-profile <id>` (repeat or comma-separated profile ids)
- `--probe-timeout <ms>`
- `--probe-concurrency <n>`
- `--probe-max-tokens <n>`
- `--agent <id>` (configured agent id; overrides `OPENCLAW_AGENT_DIR`/`PI_CODING_AGENT_DIR`)

`--json` keeps stdout reserved for the JSON payload. Auth-profile, provider,
and startup diagnostics are routed to stderr so scripts can pipe stdout directly
into tools such as `jq`.Probe status buckets:

- `ok`
- `auth`
- `rate_limit`
- `billing`
- `timeout`
- `format`
- `unknown`
- `no_model`

Probe detail/reason-code cases to expect:

- `excluded_by_auth_order`: a stored profile exists, but explicit
`auth.order.<provider>` omitted it, so probe reports the exclusion instead of
trying it.
- `missing_credential`, `invalid_expires`, `expired`, `unresolved_ref`:
profile is present but not eligible/resolvable.
- `no_model`: provider auth exists, but OpenClaw could not resolve a probeable
model candidate for that provider.

## [​](https://docs.openclaw.ai/cli/models\#aliases-+-fallbacks)  Aliases + fallbacks

```
openclaw models aliases list
openclaw models fallbacks list
```

## [​](https://docs.openclaw.ai/cli/models\#auth-profiles)  Auth profiles

```
openclaw models auth add
openclaw models auth login --provider <id>
openclaw models auth setup-token --provider <id>
openclaw models auth paste-token
```

`models auth add` is the interactive auth helper. It can launch a provider auth
flow (OAuth/API key) or guide you into manual token paste, depending on the
provider you choose.`models auth login` runs a provider plugin’s auth flow (OAuth/API key). Use
`openclaw plugins list` to see which providers are installed.
Use `openclaw models auth --agent <id> <subcommand>` to write auth results to a
specific configured agent store. The parent `--agent` flag is honored by
`add`, `login`, `setup-token`, `paste-token`, and `login-github-copilot`.Examples:

```
openclaw models auth login --provider openai-codex --set-default
```

Notes:

- `setup-token` and `paste-token` remain generic token commands for providers
that expose token auth methods.
- `setup-token` requires an interactive TTY and runs the provider’s token-auth
method (defaulting to that provider’s `setup-token` method when it exposes
one).
- `paste-token` accepts a token string generated elsewhere or from automation.
- `paste-token` requires `--provider`, prompts for the token value, and writes
it to the default profile id `<provider>:manual` unless you pass
`--profile-id`.
- `paste-token --expires-in <duration>` stores an absolute token expiry from a
relative duration such as `365d` or `12h`.
- Anthropic note: Anthropic staff told us OpenClaw-style Claude CLI usage is allowed again, so OpenClaw treats Claude CLI reuse and `claude -p` usage as sanctioned for this integration unless Anthropic publishes a new policy.
- Anthropic `setup-token` / `paste-token` remain available as a supported OpenClaw token path, but OpenClaw now prefers Claude CLI reuse and `claude -p` when available.

## [​](https://docs.openclaw.ai/cli/models\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Model selection](https://docs.openclaw.ai/concepts/model-providers)
- [Model failover](https://docs.openclaw.ai/concepts/model-failover)

[Message](https://docs.openclaw.ai/cli/message) [Sessions](https://docs.openclaw.ai/cli/sessions)

Ctrl+I