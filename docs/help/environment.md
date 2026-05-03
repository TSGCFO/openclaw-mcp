---
source_url: https://docs.openclaw.ai/help/environment
title: "Environment variables - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/help/environment#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Diagnostics

Environment variables

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Precedence (highest → lowest)](https://docs.openclaw.ai/help/environment#precedence-highest-%E2%86%92-lowest)
- [Config env block](https://docs.openclaw.ai/help/environment#config-env-block)
- [Shell env import](https://docs.openclaw.ai/help/environment#shell-env-import)
- [Runtime-injected env vars](https://docs.openclaw.ai/help/environment#runtime-injected-env-vars)
- [UI env vars](https://docs.openclaw.ai/help/environment#ui-env-vars)
- [Env var substitution in config](https://docs.openclaw.ai/help/environment#env-var-substitution-in-config)
- [Secret refs vs ${ENV} strings](https://docs.openclaw.ai/help/environment#secret-refs-vs-%24env-strings)
- [Path-related env vars](https://docs.openclaw.ai/help/environment#path-related-env-vars)
- [Logging](https://docs.openclaw.ai/help/environment#logging)
- [OPENCLAW\_HOME](https://docs.openclaw.ai/help/environment#openclaw_home)
- [nvm users: web\_fetch TLS failures](https://docs.openclaw.ai/help/environment#nvm-users-web_fetch-tls-failures)
- [Legacy environment variables](https://docs.openclaw.ai/help/environment#legacy-environment-variables)
- [Related](https://docs.openclaw.ai/help/environment#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw pulls environment variables from multiple sources. The rule is **never override existing values**.

## [​](https://docs.openclaw.ai/help/environment\#precedence-highest-%E2%86%92-lowest)  Precedence (highest → lowest)

1. **Process environment** (what the Gateway process already has from the parent shell/daemon).
2. **`.env` in the current working directory** (dotenv default; does not override).
3. **Global `.env`** at `~/.openclaw/.env` (aka `$OPENCLAW_STATE_DIR/.env`; does not override).
4. **Config `env` block** in `~/.openclaw/openclaw.json` (applied only if missing).
5. **Optional login-shell import** (`env.shellEnv.enabled` or `OPENCLAW_LOAD_SHELL_ENV=1`), applied only for missing expected keys.

On Ubuntu fresh installs that use the default state dir, OpenClaw also treats `~/.config/openclaw/gateway.env` as a compatibility fallback after the global `.env`. If both files exist and disagree, OpenClaw keeps `~/.openclaw/.env` and prints a warning.If the config file is missing entirely, step 4 is skipped; shell import still runs if enabled.

## [​](https://docs.openclaw.ai/help/environment\#config-env-block)  Config `env` block

Two equivalent ways to set inline env vars (both are non-overriding):

```
{
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: {
      GROQ_API_KEY: "gsk-...",
    },
  },
}
```

## [​](https://docs.openclaw.ai/help/environment\#shell-env-import)  Shell env import

`env.shellEnv` runs your login shell and imports only **missing** expected keys:

```
{
  env: {
    shellEnv: {
      enabled: true,
      timeoutMs: 15000,
    },
  },
}
```

Env var equivalents:

- `OPENCLAW_LOAD_SHELL_ENV=1`
- `OPENCLAW_SHELL_ENV_TIMEOUT_MS=15000`

## [​](https://docs.openclaw.ai/help/environment\#runtime-injected-env-vars)  Runtime-injected env vars

OpenClaw also injects context markers into spawned child processes:

- `OPENCLAW_SHELL=exec`: set for commands run through the `exec` tool.
- `OPENCLAW_SHELL=acp`: set for ACP runtime backend process spawns (for example `acpx`).
- `OPENCLAW_SHELL=acp-client`: set for `openclaw acp client` when it spawns the ACP bridge process.
- `OPENCLAW_SHELL=tui-local`: set for local TUI `!` shell commands.

These are runtime markers (not required user config). They can be used in shell/profile logic
to apply context-specific rules.

## [​](https://docs.openclaw.ai/help/environment\#ui-env-vars)  UI env vars

- `OPENCLAW_THEME=light`: force the light TUI palette when your terminal has a light background.
- `OPENCLAW_THEME=dark`: force the dark TUI palette.
- `COLORFGBG`: if your terminal exports it, OpenClaw uses the background color hint to auto-pick the TUI palette.

## [​](https://docs.openclaw.ai/help/environment\#env-var-substitution-in-config)  Env var substitution in config

You can reference env vars directly in config string values using `${VAR_NAME}` syntax:

```
{
  models: {
    providers: {
      "vercel-gateway": {
        apiKey: "${VERCEL_GATEWAY_API_KEY}",
      },
    },
  },
}
```

See [Configuration: Env var substitution](https://docs.openclaw.ai/gateway/configuration-reference#env-var-substitution) for full details.

## [​](https://docs.openclaw.ai/help/environment\#secret-refs-vs-$env-strings)  Secret refs vs `${ENV}` strings

OpenClaw supports two env-driven patterns:

- `${VAR}` string substitution in config values.
- SecretRef objects (`{ source: "env", provider: "default", id: "VAR" }`) for fields that support secrets references.

Both resolve from process env at activation time. SecretRef details are documented in [Secrets Management](https://docs.openclaw.ai/gateway/secrets).

## [​](https://docs.openclaw.ai/help/environment\#path-related-env-vars)  Path-related env vars

| Variable | Purpose |
| --- | --- |
| `OPENCLAW_HOME` | Override the home directory used for all internal path resolution (`~/.openclaw/`, agent dirs, sessions, credentials). Useful when running OpenClaw as a dedicated service user. |
| `OPENCLAW_STATE_DIR` | Override the state directory (default `~/.openclaw`). |
| `OPENCLAW_CONFIG_PATH` | Override the config file path (default `~/.openclaw/openclaw.json`). |
| `OPENCLAW_INCLUDE_ROOTS` | Path-list of directories where `$include` directives may resolve files outside the config directory (default: none — `$include` is confined to the config dir). Tilde-expanded. |

## [​](https://docs.openclaw.ai/help/environment\#logging)  Logging

| Variable | Purpose |
| --- | --- |
| `OPENCLAW_LOG_LEVEL` | Override log level for both file and console (e.g. `debug`, `trace`). Takes precedence over `logging.level` and `logging.consoleLevel` in config. Invalid values are ignored with a warning. |

### [​](https://docs.openclaw.ai/help/environment\#openclaw_home)  `OPENCLAW_HOME`

When set, `OPENCLAW_HOME` replaces the system home directory (`$HOME` / `os.homedir()`) for all internal path resolution. This enables full filesystem isolation for headless service accounts.**Precedence:**`OPENCLAW_HOME` \> `$HOME` \> `USERPROFILE` \> `os.homedir()`**Example** (macOS LaunchDaemon):

```
<key>EnvironmentVariables</key>
<dict>
  <key>OPENCLAW_HOME</key>
  <string>/Users/user</string>
</dict>
```

`OPENCLAW_HOME` can also be set to a tilde path (e.g. `~/svc`), which gets expanded using `$HOME` before use.

## [​](https://docs.openclaw.ai/help/environment\#nvm-users-web_fetch-tls-failures)  nvm users: web\_fetch TLS failures

If Node.js was installed via **nvm** (not the system package manager), the built-in `fetch()` uses
nvm’s bundled CA store, which may be missing modern root CAs (ISRG Root X1/X2 for Let’s Encrypt,
DigiCert Global Root G2, etc.). This causes `web_fetch` to fail with `"fetch failed"` on most HTTPS sites.On Linux, OpenClaw automatically detects nvm and applies the fix in the actual startup environment:

- `openclaw gateway install` writes `NODE_EXTRA_CA_CERTS` into the systemd service environment
- the `openclaw` CLI entrypoint re-execs itself with `NODE_EXTRA_CA_CERTS` set before Node startup

**Manual fix (for older versions or direct `node ...` launches):**Export the variable before starting OpenClaw:

```
export NODE_EXTRA_CA_CERTS=/etc/ssl/certs/ca-certificates.crt
openclaw gateway run
```

Do not rely on writing only to `~/.openclaw/.env` for this variable; Node reads
`NODE_EXTRA_CA_CERTS` at process startup.

## [​](https://docs.openclaw.ai/help/environment\#legacy-environment-variables)  Legacy environment variables

OpenClaw only reads `OPENCLAW_*` environment variables. The legacy
`CLAWDBOT_*` and `MOLTBOT_*` prefixes from earlier releases are silently
ignored.If any are still set on the Gateway process at startup, OpenClaw emits a
single Node deprecation warning (`OPENCLAW_LEGACY_ENV_VARS`) listing the
detected prefixes and the total count. Rename each value by replacing the
legacy prefix with `OPENCLAW_` (for example `CLAWDBOT_GATEWAY_TOKEN` →
`OPENCLAW_GATEWAY_TOKEN`); the old names take no effect.

## [​](https://docs.openclaw.ai/help/environment\#related)  Related

- [Gateway configuration](https://docs.openclaw.ai/gateway/configuration)
- [FAQ: env vars and .env loading](https://docs.openclaw.ai/help/faq#env-vars-and-env-loading)
- [Models overview](https://docs.openclaw.ai/concepts/models)

[Live tests](https://docs.openclaw.ai/help/testing-live) [Diagnostics flags](https://docs.openclaw.ai/diagnostics/flags)

Ctrl+I