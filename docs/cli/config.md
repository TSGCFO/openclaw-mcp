---
source_url: https://docs.openclaw.ai/cli/config
title: "Config - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/config#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Configuration

Config

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Root options](https://docs.openclaw.ai/cli/config#root-options)
- [Examples](https://docs.openclaw.ai/cli/config#examples)
- [config schema](https://docs.openclaw.ai/cli/config#config-schema)
- [Paths](https://docs.openclaw.ai/cli/config#paths)
- [Values](https://docs.openclaw.ai/cli/config#values)
- [config set modes](https://docs.openclaw.ai/cli/config#config-set-modes)
- [config patch](https://docs.openclaw.ai/cli/config#config-patch)
- [Provider builder flags](https://docs.openclaw.ai/cli/config#provider-builder-flags)
- [Dry run](https://docs.openclaw.ai/cli/config#dry-run)
- [JSON output shape](https://docs.openclaw.ai/cli/config#json-output-shape)
- [Write safety](https://docs.openclaw.ai/cli/config#write-safety)
- [Subcommands](https://docs.openclaw.ai/cli/config#subcommands)
- [Validate](https://docs.openclaw.ai/cli/config#validate)
- [Related](https://docs.openclaw.ai/cli/config#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Config helpers for non-interactive edits in `openclaw.json`: get/set/patch/unset/file/schema/validate values by path and print the active config file. Run without a subcommand to open the configure wizard (same as `openclaw configure`).

## [​](https://docs.openclaw.ai/cli/config\#root-options)  Root options

[​](https://docs.openclaw.ai/cli/config#param-section-section)

--section <section>

string

Repeatable guided-setup section filter when you run `openclaw config` without a subcommand.

Supported guided sections: `workspace`, `model`, `web`, `gateway`, `daemon`, `channels`, `plugins`, `skills`, `health`.

## [​](https://docs.openclaw.ai/cli/config\#examples)  Examples

```
openclaw config file
openclaw config --section model
openclaw config --section gateway --section daemon
openclaw config schema
openclaw config get browser.executablePath
openclaw config set browser.executablePath "/usr/bin/google-chrome"
openclaw config set browser.profiles.work.executablePath "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
openclaw config set agents.defaults.heartbeat.every "2h"
openclaw config set agents.list[0].tools.exec.node "node-id-or-name"
openclaw config set agents.defaults.models '{"openai/gpt-5.4":{}}' --strict-json --merge
openclaw config set channels.discord.token --ref-provider default --ref-source env --ref-id DISCORD_BOT_TOKEN
openclaw config set secrets.providers.vaultfile --provider-source file --provider-path /etc/openclaw/secrets.json --provider-mode json
openclaw config patch --file ./openclaw.patch.json5 --dry-run
openclaw config unset plugins.entries.brave.config.webSearch.apiKey
openclaw config set channels.discord.token --ref-provider default --ref-source env --ref-id DISCORD_BOT_TOKEN --dry-run
openclaw config validate
openclaw config validate --json
```

### [​](https://docs.openclaw.ai/cli/config\#config-schema)  `config schema`

Print the generated JSON schema for `openclaw.json` to stdout as JSON.

What it includes

- The current root config schema, plus a root `$schema` string field for editor tooling.
- Field `title` and `description` docs metadata used by the Control UI.
- Nested object, wildcard (`*`), and array-item (`[]`) nodes inherit the same `title` / `description` metadata when matching field documentation exists.
- `anyOf` / `oneOf` / `allOf` branches inherit the same docs metadata too when matching field documentation exists.
- Best-effort live plugin + channel schema metadata when runtime manifests can be loaded.
- A clean fallback schema even when the current config is invalid.

Related runtime RPC

`config.schema.lookup` returns one normalized config path with a shallow schema node (`title`, `description`, `type`, `enum`, `const`, common bounds), matched UI hint metadata, and immediate child summaries. Use it for path-scoped drill-down in Control UI or custom clients.

```
openclaw config schema
```

Pipe it into a file when you want to inspect or validate it with other tools:

```
openclaw config schema > openclaw.schema.json
```

### [​](https://docs.openclaw.ai/cli/config\#paths)  Paths

Paths use dot or bracket notation:

```
openclaw config get agents.defaults.workspace
openclaw config get agents.list[0].id
```

Use the agent list index to target a specific agent:

```
openclaw config get agents.list
openclaw config set agents.list[1].tools.exec.node "node-id-or-name"
```

## [​](https://docs.openclaw.ai/cli/config\#values)  Values

Values are parsed as JSON5 when possible; otherwise they are treated as strings. Use `--strict-json` to require JSON5 parsing. `--json` remains supported as a legacy alias.

```
openclaw config set agents.defaults.heartbeat.every "0m"
openclaw config set gateway.port 19001 --strict-json
openclaw config set channels.whatsapp.groups '["*"]' --strict-json
```

`config get <path> --json` prints the raw value as JSON instead of terminal-formatted text.

Object assignment replaces the target path by default. Protected map/list paths that commonly hold user-added entries, such as `agents.defaults.models`, `models.providers`, `models.providers.<id>.models`, `plugins.entries`, and `auth.profiles`, refuse replacements that would remove existing entries unless you pass `--replace`.

Use `--merge` when adding entries to those maps:

```
openclaw config set agents.defaults.models '{"openai/gpt-5.4":{}}' --strict-json --merge
openclaw config set models.providers.ollama.models '[{"id":"llama3.2","name":"Llama 3.2"}]' --strict-json --merge
```

Use `--replace` only when you intentionally want the provided value to become the complete target value.

## [​](https://docs.openclaw.ai/cli/config\#config-set-modes)  `config set` modes

`openclaw config set` supports four assignment styles:

- Value mode

- SecretRef builder mode

- Provider builder mode

- Batch mode


```
openclaw config set <path> <value>
```

```
openclaw config set channels.discord.token \
  --ref-provider default \
  --ref-source env \
  --ref-id DISCORD_BOT_TOKEN
```

Provider builder mode targets `secrets.providers.<alias>` paths only:

```
openclaw config set secrets.providers.vault \
  --provider-source exec \
  --provider-command /usr/local/bin/openclaw-vault \
  --provider-arg read \
  --provider-arg openai/api-key \
  --provider-timeout-ms 5000
```

```
openclaw config set --batch-json '[\
  {\
    "path": "secrets.providers.default",\
    "provider": { "source": "env" }\
  },\
  {\
    "path": "channels.discord.token",\
    "ref": { "source": "env", "provider": "default", "id": "DISCORD_BOT_TOKEN" }\
  }\
]'
```

```
openclaw config set --batch-file ./config-set.batch.json --dry-run
```

SecretRef assignments are rejected on unsupported runtime-mutable surfaces (for example `hooks.token`, `commands.ownerDisplaySecret`, Discord thread-binding webhook tokens, and WhatsApp creds JSON). See [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface).

Batch parsing always uses the batch payload (`--batch-json`/`--batch-file`) as the source of truth. `--strict-json` / `--json` do not change batch parsing behavior.

## [​](https://docs.openclaw.ai/cli/config\#config-patch)  `config patch`

Use `config patch` when you want to paste or pipe a config-shaped patch instead of running many path-based `config set` commands. The input is a JSON5 object. Objects merge recursively, arrays and scalar values replace the target value, and `null` deletes the target path.

```
openclaw config patch --file ./openclaw.patch.json5 --dry-run
openclaw config patch --file ./openclaw.patch.json5
```

You can also pipe a patch over stdin, which is useful for remote setup scripts:

```
ssh openclaw-host 'openclaw config patch --stdin --dry-run' < ./openclaw.patch.json5
ssh openclaw-host 'openclaw config patch --stdin' < ./openclaw.patch.json5
```

Example patch:

```
{
  channels: {
    slack: {
      enabled: true,
      mode: "socket",
      botToken: { source: "env", provider: "default", id: "SLACK_BOT_TOKEN" },
      appToken: { source: "env", provider: "default", id: "SLACK_APP_TOKEN" },
      groupPolicy: "open",
      requireMention: false,
    },
    discord: {
      enabled: true,
      token: { source: "env", provider: "default", id: "DISCORD_BOT_TOKEN" },
      dmPolicy: "disabled",
      dm: { enabled: false },
      groupPolicy: "allowlist",
    },
  },
  agents: {
    defaults: {
      model: { primary: "openai/gpt-5.5" },
      models: {
        "openai/gpt-5.5": { params: { fastMode: true } },
      },
    },
  },
}
```

Use `--replace-path <path>` when one object or array must become exactly the provided value instead of being recursively patched:

```
openclaw config patch --file ./discord.patch.json5 --replace-path 'channels.discord.guilds["123"].channels'
```

`--dry-run` runs schema and SecretRef resolvability checks without writing. Exec-backed SecretRefs are skipped by default during dry-run; add `--allow-exec` when you intentionally want dry-run to execute provider commands.JSON path/value mode remains supported for both SecretRefs and providers:

```
openclaw config set channels.discord.token \
  '{"source":"env","provider":"default","id":"DISCORD_BOT_TOKEN"}' \
  --strict-json

openclaw config set secrets.providers.vaultfile \
  '{"source":"file","path":"/etc/openclaw/secrets.json","mode":"json"}' \
  --strict-json
```

## [​](https://docs.openclaw.ai/cli/config\#provider-builder-flags)  Provider builder flags

Provider builder targets must use `secrets.providers.<alias>` as the path.

Common flags

- `--provider-source <env|file|exec>`
- `--provider-timeout-ms <ms>` (`file`, `exec`)

Env provider (--provider-source env)

- `--provider-allowlist <ENV_VAR>` (repeatable)

File provider (--provider-source file)

- `--provider-path <path>` (required)
- `--provider-mode <singleValue|json>`
- `--provider-max-bytes <bytes>`
- `--provider-allow-insecure-path`

Exec provider (--provider-source exec)

- `--provider-command <path>` (required)
- `--provider-arg <arg>` (repeatable)
- `--provider-no-output-timeout-ms <ms>`
- `--provider-max-output-bytes <bytes>`
- `--provider-json-only`
- `--provider-env <KEY=VALUE>` (repeatable)
- `--provider-pass-env <ENV_VAR>` (repeatable)
- `--provider-trusted-dir <path>` (repeatable)
- `--provider-allow-insecure-path`
- `--provider-allow-symlink-command`

Hardened exec provider example:

```
openclaw config set secrets.providers.vault \
  --provider-source exec \
  --provider-command /usr/local/bin/openclaw-vault \
  --provider-arg read \
  --provider-arg openai/api-key \
  --provider-json-only \
  --provider-pass-env VAULT_TOKEN \
  --provider-trusted-dir /usr/local/bin \
  --provider-timeout-ms 5000
```

## [​](https://docs.openclaw.ai/cli/config\#dry-run)  Dry run

Use `--dry-run` to validate changes without writing `openclaw.json`.

```
openclaw config set channels.discord.token \
  --ref-provider default \
  --ref-source env \
  --ref-id DISCORD_BOT_TOKEN \
  --dry-run

openclaw config set channels.discord.token \
  --ref-provider default \
  --ref-source env \
  --ref-id DISCORD_BOT_TOKEN \
  --dry-run \
  --json

openclaw config set channels.discord.token \
  --ref-provider vault \
  --ref-source exec \
  --ref-id discord/token \
  --dry-run \
  --allow-exec
```

Dry-run behavior

- Builder mode: runs SecretRef resolvability checks for changed refs/providers.
- JSON mode (`--strict-json`, `--json`, or batch mode): runs schema validation plus SecretRef resolvability checks.
- Policy validation also runs for known unsupported SecretRef target surfaces.
- Policy checks evaluate the full post-change config, so parent-object writes (for example setting `hooks` as an object) cannot bypass unsupported-surface validation.
- Exec SecretRef checks are skipped by default during dry-run to avoid command side effects.
- Use `--allow-exec` with `--dry-run` to opt in to exec SecretRef checks (this may execute provider commands).
- `--allow-exec` is dry-run only and errors if used without `--dry-run`.

--dry-run --json fields

`--dry-run --json` prints a machine-readable report:

- `ok`: whether dry-run passed
- `operations`: number of assignments evaluated
- `checks`: whether schema/resolvability checks ran
- `checks.resolvabilityComplete`: whether resolvability checks ran to completion (false when exec refs are skipped)
- `refsChecked`: number of refs actually resolved during dry-run
- `skippedExecRefs`: number of exec refs skipped because `--allow-exec` was not set
- `errors`: structured schema/resolvability failures when `ok=false`

### [​](https://docs.openclaw.ai/cli/config\#json-output-shape)  JSON output shape

```
{
  ok: boolean,
  operations: number,
  configPath: string,
  inputModes: ["value" | "json" | "builder", ...],
  checks: {
    schema: boolean,
    resolvability: boolean,
    resolvabilityComplete: boolean,
  },
  refsChecked: number,
  skippedExecRefs: number,
  errors?: [\
    {\
      kind: "schema" | "resolvability",\
      message: string,\
      ref?: string, // present for resolvability errors\
    },\
  ],
}
```

- Success example

- Failure example


```
{
  "ok": true,
  "operations": 1,
  "configPath": "~/.openclaw/openclaw.json",
  "inputModes": ["builder"],
  "checks": {
    "schema": false,
    "resolvability": true,
    "resolvabilityComplete": true
  },
  "refsChecked": 1,
  "skippedExecRefs": 0
}
```

```
{
  "ok": false,
  "operations": 1,
  "configPath": "~/.openclaw/openclaw.json",
  "inputModes": ["builder"],
  "checks": {
    "schema": false,
    "resolvability": true,
    "resolvabilityComplete": true
  },
  "refsChecked": 1,
  "skippedExecRefs": 0,
  "errors": [\
    {\
      "kind": "resolvability",\
      "message": "Error: Environment variable \"MISSING_TEST_SECRET\" is not set.",\
      "ref": "env:default:MISSING_TEST_SECRET"\
    }\
  ]
}
```

If dry-run fails

- `config schema validation failed`: your post-change config shape is invalid; fix path/value or provider/ref object shape.
- `Config policy validation failed: unsupported SecretRef usage`: move that credential back to plaintext/string input and keep SecretRefs on supported surfaces only.
- `SecretRef assignment(s) could not be resolved`: referenced provider/ref currently cannot resolve (missing env var, invalid file pointer, exec provider failure, or provider/source mismatch).
- `Dry run note: skipped <n> exec SecretRef resolvability check(s)`: dry-run skipped exec refs; rerun with `--allow-exec` if you need exec resolvability validation.
- For batch mode, fix failing entries and rerun `--dry-run` before writing.

## [​](https://docs.openclaw.ai/cli/config\#write-safety)  Write safety

`openclaw config set` and other OpenClaw-owned config writers validate the full post-change config before committing it to disk. If the new payload fails schema validation or looks like a destructive clobber, the active config is left alone and the rejected payload is saved beside it as `openclaw.json.rejected.*`.

The active config path must be a regular file. Symlinked `openclaw.json` layouts are unsupported for writes; use `OPENCLAW_CONFIG_PATH` to point directly at the real file instead.

Prefer CLI writes for small edits:

```
openclaw config set gateway.reload.mode hybrid --dry-run
openclaw config set gateway.reload.mode hybrid
openclaw config validate
```

If a write is rejected, inspect the saved payload and fix the full config shape:

```
CONFIG="$(openclaw config file)"
ls -lt "$CONFIG".rejected.* 2>/dev/null | head
openclaw config validate
```

Direct editor writes are still allowed, but the running Gateway treats them as untrusted until they validate. Invalid direct edits can be restored from the last-known-good backup during startup or hot reload. See [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting#gateway-restored-last-known-good-config).Whole-file recovery is reserved for globally broken config, such as parse errors, root-level schema failures, legacy migration failures, or mixed plugin and root failures. If validation fails only under `plugins.entries.<id>...`, OpenClaw keeps the active `openclaw.json` in place and reports the plugin-local issue instead of restoring `.last-good`. This prevents plugin schema changes or `minHostVersion` skew from rolling back unrelated user settings such as models, providers, auth profiles, channels, gateway exposure, tools, memory, browser, or cron config.

## [​](https://docs.openclaw.ai/cli/config\#subcommands)  Subcommands

- `config file`: Print the active config file path (resolved from `OPENCLAW_CONFIG_PATH` or default location). The path should name a regular file, not a symlink.

Restart the gateway after edits.

## [​](https://docs.openclaw.ai/cli/config\#validate)  Validate

Validate the current config against the active schema without starting the gateway.

```
openclaw config validate
openclaw config validate --json
```

After `openclaw config validate` is passing, you can use the local TUI to have an embedded agent compare the active config against the docs while you validate each change from the same terminal:

If validation is already failing, start with `openclaw configure` or `openclaw doctor --fix`. `openclaw chat` does not bypass the invalid-config guard.

```
openclaw chat
```

Then inside the TUI:

```
!openclaw config file
!openclaw docs gateway auth token secretref
!openclaw config validate
!openclaw doctor
```

Typical repair loop:

1

[Navigate to header](https://docs.openclaw.ai/cli/config#)

Compare with docs

Ask the agent to compare your current config with the relevant docs page and suggest the smallest fix.

2

[Navigate to header](https://docs.openclaw.ai/cli/config#)

Apply targeted edits

Apply targeted edits with `openclaw config set` or `openclaw configure`.

3

[Navigate to header](https://docs.openclaw.ai/cli/config#)

Re-validate

Rerun `openclaw config validate` after each change.

4

[Navigate to header](https://docs.openclaw.ai/cli/config#)

Doctor for runtime issues

If validation passes but the runtime is still unhealthy, run `openclaw doctor` or `openclaw doctor --fix` for migration and repair help.

## [​](https://docs.openclaw.ai/cli/config\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)

[Sandbox CLI](https://docs.openclaw.ai/cli/sandbox) [Configure](https://docs.openclaw.ai/cli/configure)

Ctrl+I