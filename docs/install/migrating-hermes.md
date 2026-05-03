---
source_url: https://docs.openclaw.ai/install/migrating-hermes
title: "Migrating from Hermes - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/migrating-hermes#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Migrating

Migrating from Hermes

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Two ways to import](https://docs.openclaw.ai/install/migrating-hermes#two-ways-to-import)
- [What gets imported](https://docs.openclaw.ai/install/migrating-hermes#what-gets-imported)
- [What stays archive-only](https://docs.openclaw.ai/install/migrating-hermes#what-stays-archive-only)
- [Recommended flow](https://docs.openclaw.ai/install/migrating-hermes#recommended-flow)
- [Conflict handling](https://docs.openclaw.ai/install/migrating-hermes#conflict-handling)
- [Secrets](https://docs.openclaw.ai/install/migrating-hermes#secrets)
- [JSON output for automation](https://docs.openclaw.ai/install/migrating-hermes#json-output-for-automation)
- [Troubleshooting](https://docs.openclaw.ai/install/migrating-hermes#troubleshooting)
- [Related](https://docs.openclaw.ai/install/migrating-hermes#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw imports Hermes state through a bundled migration provider. The provider previews everything before changing state, redacts secrets in plans and reports, and creates a verified backup before apply.

Imports require a fresh OpenClaw setup. If you already have local OpenClaw state, reset config, credentials, sessions, and the workspace first, or use `openclaw migrate` directly with `--overwrite` after reviewing the plan.

## [​](https://docs.openclaw.ai/install/migrating-hermes\#two-ways-to-import)  Two ways to import

- Onboarding wizard

- CLI


The fastest path. The wizard detects Hermes at `~/.hermes` and shows a preview before applying.

```
openclaw onboard --flow import
```

Or point at a specific source:

```
openclaw onboard --import-from hermes --import-source ~/.hermes
```

Use `openclaw migrate` for scripted or repeatable runs. See [`openclaw migrate`](https://docs.openclaw.ai/cli/migrate) for the full reference.

```
openclaw migrate hermes --dry-run    # preview only
openclaw migrate apply hermes --yes  # apply with confirmation skipped
```

Add `--from <path>` when Hermes lives outside `~/.hermes`.

## [​](https://docs.openclaw.ai/install/migrating-hermes\#what-gets-imported)  What gets imported

Model configuration

- Default model selection from Hermes `config.yaml`.
- Configured model providers and custom OpenAI-compatible endpoints from `providers` and `custom_providers`.

MCP servers

MCP server definitions from `mcp_servers` or `mcp.servers`.

Workspace files

- `SOUL.md` and `AGENTS.md` are copied into the OpenClaw agent workspace.
- `memories/MEMORY.md` and `memories/USER.md` are **appended** to the matching OpenClaw memory files instead of overwriting them.

Memory configuration

Memory config defaults for OpenClaw file memory. External memory providers such as Honcho are recorded as archive or manual-review items so you can move them deliberately.

Skills

Skills with a `SKILL.md` file under `skills/<name>/` are copied, along with per-skill config values from `skills.config`.

API keys (opt-in)

Set `--include-secrets` to import supported `.env` keys: `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, `OPENROUTER_API_KEY`, `GOOGLE_API_KEY`, `GEMINI_API_KEY`, `GROQ_API_KEY`, `XAI_API_KEY`, `MISTRAL_API_KEY`, `DEEPSEEK_API_KEY`. Without the flag, secrets are never copied.

## [​](https://docs.openclaw.ai/install/migrating-hermes\#what-stays-archive-only)  What stays archive-only

The provider copies these into the migration report directory for manual review, but does **not** load them into live OpenClaw config or credentials:

- `plugins/`
- `sessions/`
- `logs/`
- `cron/`
- `mcp-tokens/`
- `auth.json`
- `state.db`

OpenClaw refuses to execute or trust this state automatically because the formats and trust assumptions can drift between systems. Move what you need by hand after reviewing the archive.

## [​](https://docs.openclaw.ai/install/migrating-hermes\#recommended-flow)  Recommended flow

1

[Navigate to header](https://docs.openclaw.ai/install/migrating-hermes#)

Preview the plan

```
openclaw migrate hermes --dry-run
```

The plan lists everything that will change, including conflicts, skipped items, and any sensitive items. Plan output redacts nested secret-looking keys.

2

[Navigate to header](https://docs.openclaw.ai/install/migrating-hermes#)

Apply with backup

```
openclaw migrate apply hermes --yes
```

OpenClaw creates and verifies a backup before applying. If you need API keys imported, add `--include-secrets`.

3

[Navigate to header](https://docs.openclaw.ai/install/migrating-hermes#)

Run doctor

```
openclaw doctor
```

[Doctor](https://docs.openclaw.ai/gateway/doctor) reapplies any pending config migrations and checks for issues introduced during the import.

4

[Navigate to header](https://docs.openclaw.ai/install/migrating-hermes#)

Restart and verify

```
openclaw gateway restart
openclaw status
```

Confirm the gateway is healthy and your imported model, memory, and skills are loaded.

## [​](https://docs.openclaw.ai/install/migrating-hermes\#conflict-handling)  Conflict handling

Apply refuses to continue when the plan reports conflicts (a file or config value already exists at the target).

Rerun with `--overwrite` only when replacing the existing target is intentional. Providers may still write item-level backups for overwritten files in the migration report directory.

For a fresh OpenClaw install, conflicts are unusual. They typically appear when you re-run the import on a setup that already has user edits.If a conflict surfaces mid-apply (for example, an unexpected race on a config file), Hermes marks remaining dependent config items as `skipped` with reason `blocked by earlier apply conflict` instead of writing them partially. The migration report records each blocked item so you can resolve the original conflict and rerun the import.

## [​](https://docs.openclaw.ai/install/migrating-hermes\#secrets)  Secrets

Secrets are never imported by default.

- Run `openclaw migrate apply hermes --yes` first to import non-secret state.
- If you also want supported `.env` keys copied across, rerun with `--include-secrets`.
- For SecretRef-managed credentials, configure the SecretRef source after the import completes.

## [​](https://docs.openclaw.ai/install/migrating-hermes\#json-output-for-automation)  JSON output for automation

```
openclaw migrate hermes --dry-run --json
openclaw migrate apply hermes --json --yes
```

With `--json` and no `--yes`, apply prints the plan and does not mutate state. This is the safest mode for CI and shared scripts.

## [​](https://docs.openclaw.ai/install/migrating-hermes\#troubleshooting)  Troubleshooting

Apply refuses with conflicts

Inspect the plan output. Each conflict identifies the source path and the existing target. Decide per item whether to skip, edit the target, or rerun with `--overwrite`.

Hermes lives outside ~/.hermes

Pass `--from /actual/path` (CLI) or `--import-source /actual/path` (onboarding).

Onboarding refuses to import on an existing setup

Onboarding imports require a fresh setup. Either reset state and re-onboard, or use `openclaw migrate apply hermes` directly, which supports `--overwrite` and explicit backup control.

API keys did not import

`--include-secrets` is required, and only the keys listed above are recognized. Other variables in `.env` are ignored.

## [​](https://docs.openclaw.ai/install/migrating-hermes\#related)  Related

- [`openclaw migrate`](https://docs.openclaw.ai/cli/migrate): full CLI reference, plugin contract, and JSON shapes.
- [Onboarding](https://docs.openclaw.ai/cli/onboard): wizard flow and non-interactive flags.
- [Migrating](https://docs.openclaw.ai/install/migrating): move an OpenClaw install between machines.
- [Doctor](https://docs.openclaw.ai/gateway/doctor): post-migration health check.
- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace): where `SOUL.md`, `AGENTS.md`, and memory files live.

[Migrating from Claude](https://docs.openclaw.ai/install/migrating-claude) [Uninstall](https://docs.openclaw.ai/install/uninstall)

Ctrl+I