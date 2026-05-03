---
source_url: https://docs.openclaw.ai/install/migrating
title: "Migration guide - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/migrating#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Migrating

Migration guide

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Import from another agent system](https://docs.openclaw.ai/install/migrating#import-from-another-agent-system)
- [Move OpenClaw to a new machine](https://docs.openclaw.ai/install/migrating#move-openclaw-to-a-new-machine)
- [Migration steps](https://docs.openclaw.ai/install/migrating#migration-steps)
- [Common pitfalls](https://docs.openclaw.ai/install/migrating#common-pitfalls)
- [Verification checklist](https://docs.openclaw.ai/install/migrating#verification-checklist)
- [Upgrade a plugin in place](https://docs.openclaw.ai/install/migrating#upgrade-a-plugin-in-place)
- [Related](https://docs.openclaw.ai/install/migrating#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw supports three migration paths: importing from another agent system, moving an existing install to a new machine, and upgrading a plugin in place.

## [​](https://docs.openclaw.ai/install/migrating\#import-from-another-agent-system)  Import from another agent system

Use the bundled migration providers to bring instructions, MCP servers, skills, model config, and (opt-in) API keys into OpenClaw. Plans are previewed before any change, secrets are redacted in reports, and apply is backed by a verified backup.

[**Migrating from Claude** \\
\\
Import Claude Code and Claude Desktop state, including `CLAUDE.md`, MCP servers, skills, and project commands.](https://docs.openclaw.ai/install/migrating-claude)

[**Migrating from Hermes** \\
\\
Import Hermes config, providers, MCP servers, memory, skills, and supported `.env` keys.](https://docs.openclaw.ai/install/migrating-hermes)

The CLI entry point is [`openclaw migrate`](https://docs.openclaw.ai/cli/migrate). Onboarding can also offer migration when it detects a known source (`openclaw onboard --flow import`).

## [​](https://docs.openclaw.ai/install/migrating\#move-openclaw-to-a-new-machine)  Move OpenClaw to a new machine

Copy the **state directory** (`~/.openclaw/` by default) and your **workspace** to preserve:

- **Config** — `openclaw.json` and all gateway settings.
- **Auth** — per-agent `auth-profiles.json` (API keys plus OAuth), plus any channel or provider state under `credentials/`.
- **Sessions** — conversation history and agent state.
- **Channel state** — WhatsApp login, Telegram session, and similar.
- **Workspace files** — `MEMORY.md`, `USER.md`, skills, and prompts.

Run `openclaw status` on the old machine to confirm your state directory path. Custom profiles use `~/.openclaw-<profile>/` or a path set via `OPENCLAW_STATE_DIR`.

### [​](https://docs.openclaw.ai/install/migrating\#migration-steps)  Migration steps

1

[Navigate to header](https://docs.openclaw.ai/install/migrating#)

Stop the gateway and back up

On the **old** machine, stop the gateway so files are not changing mid-copy, then archive:

```
openclaw gateway stop
cd ~
tar -czf openclaw-state.tgz .openclaw
```

If you use multiple profiles (for example `~/.openclaw-work`), archive each separately.

2

[Navigate to header](https://docs.openclaw.ai/install/migrating#)

Install OpenClaw on the new machine

[Install](https://docs.openclaw.ai/install) the CLI (and Node if needed) on the new machine. It is fine if onboarding creates a fresh `~/.openclaw/`. You will overwrite it next.

3

[Navigate to header](https://docs.openclaw.ai/install/migrating#)

Copy state directory and workspace

Transfer the archive via `scp`, `rsync -a`, or an external drive, then extract:

```
cd ~
tar -xzf openclaw-state.tgz
```

Ensure hidden directories were included and file ownership matches the user that will run the gateway.

4

[Navigate to header](https://docs.openclaw.ai/install/migrating#)

Run doctor and verify

On the new machine, run [Doctor](https://docs.openclaw.ai/gateway/doctor) to apply config migrations and repair services:

```
openclaw doctor
openclaw gateway restart
openclaw status
```

If Telegram or Discord uses the default env fallback (`TELEGRAM_BOT_TOKEN` or `DISCORD_BOT_TOKEN`), verify the migrated state-dir `.env` contains those keys without printing the secret values:

```
awk -F= '/^(TELEGRAM_BOT_TOKEN|DISCORD_BOT_TOKEN)=/ { print $1 "=present" }' ~/.openclaw/.env
```

`openclaw doctor` also warns when an enabled default Telegram or Discord account has no configured token and the matching env variable is unavailable to the doctor process.

### [​](https://docs.openclaw.ai/install/migrating\#common-pitfalls)  Common pitfalls

Profile or state-dir mismatch

If the old gateway used `--profile` or `OPENCLAW_STATE_DIR` and the new one does not, channels will appear logged out and sessions will be empty. Launch the gateway with the **same** profile or state-dir you migrated, then rerun `openclaw doctor`.

Copying only openclaw.json

The config file alone is not enough. Model auth profiles live under `agents/<agentId>/agent/auth-profiles.json`, and channel and provider state lives under `credentials/`. Always migrate the **entire** state directory.

Permissions and ownership

If you copied as root or switched users, the gateway may fail to read credentials. Ensure the state directory and workspace are owned by the user running the gateway.

Remote mode

If your UI points at a **remote** gateway, the remote host owns sessions and workspace. Migrate the gateway host itself, not your local laptop. See [FAQ](https://docs.openclaw.ai/help/faq#where-things-live-on-disk).

Secrets in backups

The state directory contains auth profiles, channel credentials, and other provider state. Store backups encrypted, avoid insecure transfer channels, and rotate keys if you suspect exposure.

### [​](https://docs.openclaw.ai/install/migrating\#verification-checklist)  Verification checklist

On the new machine, confirm:

- [ ] `openclaw status` shows the gateway running.
- [ ]  Channels are still connected (no re-pairing needed).
- [ ]  The dashboard opens and shows existing sessions.
- [ ]  Workspace files (memory, configs) are present.

## [​](https://docs.openclaw.ai/install/migrating\#upgrade-a-plugin-in-place)  Upgrade a plugin in place

In-place plugin upgrades preserve the same plugin id and config keys but may move on-disk state into the current layout. Plugin-specific upgrade guides live alongside their channels:

- [Matrix migration](https://docs.openclaw.ai/channels/matrix-migration): encrypted-state recovery limits, automatic snapshot behavior, and manual recovery commands.

## [​](https://docs.openclaw.ai/install/migrating\#related)  Related

- [`openclaw migrate`](https://docs.openclaw.ai/cli/migrate): CLI reference for cross-system imports.
- [Install overview](https://docs.openclaw.ai/install): all installation methods.
- [Doctor](https://docs.openclaw.ai/gateway/doctor): post-migration health check.
- [Uninstall](https://docs.openclaw.ai/install/uninstall): removing OpenClaw cleanly.

[Updating](https://docs.openclaw.ai/install/updating) [Migrating from Claude](https://docs.openclaw.ai/install/migrating-claude)

Ctrl+I