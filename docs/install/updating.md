---
source_url: https://docs.openclaw.ai/install/updating
title: "Updating - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/updating#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Maintenance

Updating

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Recommended: openclaw update](https://docs.openclaw.ai/install/updating#recommended-openclaw-update)
- [Switch between npm and git installs](https://docs.openclaw.ai/install/updating#switch-between-npm-and-git-installs)
- [Alternative: re-run the installer](https://docs.openclaw.ai/install/updating#alternative-re-run-the-installer)
- [Alternative: manual npm, pnpm, or bun](https://docs.openclaw.ai/install/updating#alternative-manual-npm-pnpm-or-bun)
- [Advanced npm install topics](https://docs.openclaw.ai/install/updating#advanced-npm-install-topics)
- [Auto-updater](https://docs.openclaw.ai/install/updating#auto-updater)
- [After updating](https://docs.openclaw.ai/install/updating#after-updating)
- [Run doctor](https://docs.openclaw.ai/install/updating#run-doctor)
- [Restart the gateway](https://docs.openclaw.ai/install/updating#restart-the-gateway)
- [Verify](https://docs.openclaw.ai/install/updating#verify)
- [Rollback](https://docs.openclaw.ai/install/updating#rollback)
- [Pin a version (npm)](https://docs.openclaw.ai/install/updating#pin-a-version-npm)
- [Pin a commit (source)](https://docs.openclaw.ai/install/updating#pin-a-commit-source)
- [If you are stuck](https://docs.openclaw.ai/install/updating#if-you-are-stuck)
- [Related](https://docs.openclaw.ai/install/updating#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Keep OpenClaw up to date.

## [​](https://docs.openclaw.ai/install/updating\#recommended-openclaw-update)  Recommended: `openclaw update`

The fastest way to update. It detects your install type (npm or git), fetches the latest version, runs `openclaw doctor`, and restarts the gateway.

```
openclaw update
```

To switch channels or target a specific version:

```
openclaw update --channel beta
openclaw update --channel dev
openclaw update --tag main
openclaw update --dry-run   # preview without applying
```

`--channel beta` prefers beta, but the runtime falls back to stable/latest when
the beta tag is missing or older than the latest stable release. Use `--tag beta`
if you want the raw npm beta dist-tag for a one-off package update.See [Development channels](https://docs.openclaw.ai/install/development-channels) for channel semantics.

## [​](https://docs.openclaw.ai/install/updating\#switch-between-npm-and-git-installs)  Switch between npm and git installs

Use channels when you want to change the install type. The updater keeps your
state, config, credentials, and workspace in `~/.openclaw`; it only changes
which OpenClaw code install the CLI and gateway use.

```
# npm package install -> editable git checkout
openclaw update --channel dev

# git checkout -> npm package install
openclaw update --channel stable
```

Run with `--dry-run` first to preview the exact install-mode switch:

```
openclaw update --channel dev --dry-run
openclaw update --channel stable --dry-run
```

The `dev` channel ensures a git checkout, builds it, and installs the global CLI
from that checkout. The `stable` and `beta` channels use package installs. If the
gateway is already installed, `openclaw update` refreshes the service metadata
and restarts it unless you pass `--no-restart`.

## [​](https://docs.openclaw.ai/install/updating\#alternative-re-run-the-installer)  Alternative: re-run the installer

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

Add `--no-onboard` to skip onboarding. To force a specific install type through
the installer, pass `--install-method git --no-onboard` or
`--install-method npm --no-onboard`.If `openclaw update` fails after the npm package install phase, re-run the
installer. The installer does not call the old updater; it runs the global
package install directly and can recover a partially updated npm install.

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method npm
```

To pin the recovery to a specific version or dist-tag, add `--version`:

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method npm --version <version-or-dist-tag>
```

## [​](https://docs.openclaw.ai/install/updating\#alternative-manual-npm-pnpm-or-bun)  Alternative: manual npm, pnpm, or bun

```
npm i -g openclaw@latest
```

When `openclaw update` manages a global npm install, it installs the target into
a temporary npm prefix first, verifies the packaged `dist` inventory, then swaps
the clean package tree into the real global prefix. That avoids npm overlaying a
new package onto stale files from the old package. If the install command fails,
OpenClaw retries once with `--omit=optional`. That retry helps hosts where native
optional dependencies cannot compile, while keeping the original failure visible
if the fallback also fails.

```
pnpm add -g openclaw@latest
```

```
bun add -g openclaw@latest
```

### [​](https://docs.openclaw.ai/install/updating\#advanced-npm-install-topics)  Advanced npm install topics

Read-only package tree

OpenClaw treats packaged global installs as read-only at runtime, even when the global package directory is writable by the current user. Plugin package installs live in OpenClaw-owned npm/git roots under the user config directory, and Gateway startup does not mutate the OpenClaw package tree.Some Linux npm setups install global packages under root-owned directories such as `/usr/lib/node_modules/openclaw`. OpenClaw supports that layout because plugin install/update commands write outside that global package directory.

Hardened systemd units

Give OpenClaw write access to its config/state roots so explicit plugin installs, plugin updates, and doctor cleanup can persist their changes:

```
ReadWritePaths=/var/lib/openclaw /home/openclaw/.openclaw /tmp
```

Disk-space preflight

Before package updates and explicit plugin installs, OpenClaw tries a best-effort disk-space check for the target volume. Low space produces a warning with the checked path, but does not block the update because filesystem quotas, snapshots, and network volumes can change after the check. The actual package-manager install and post-install verification remain authoritative.

## [​](https://docs.openclaw.ai/install/updating\#auto-updater)  Auto-updater

The auto-updater is off by default. Enable it in `~/.openclaw/openclaw.json`:

```
{
  update: {
    channel: "stable",
    auto: {
      enabled: true,
      stableDelayHours: 6,
      stableJitterHours: 12,
      betaCheckIntervalHours: 1,
    },
  },
}
```

| Channel | Behavior |
| --- | --- |
| `stable` | Waits `stableDelayHours`, then applies with deterministic jitter across `stableJitterHours` (spread rollout). |
| `beta` | Checks every `betaCheckIntervalHours` (default: hourly) and applies immediately. |
| `dev` | No automatic apply. Use `openclaw update` manually. |

The gateway also logs an update hint on startup (disable with `update.checkOnStart: false`).
For downgrade or incident recovery, set `OPENCLAW_NO_AUTO_UPDATE=1` in the gateway environment to block automatic applies even when `update.auto.enabled` is configured. Startup update hints can still run unless `update.checkOnStart` is also disabled.Package-manager updates requested through the live Gateway control-plane handler
force a non-deferred, no-cooldown update restart after the package swap. That
avoids leaving an old in-memory process around long enough to lazy-load chunks
from a package tree that has already been replaced. Shell `openclaw update`
remains the preferred path for supervised installs because it can stop and
restart the service around the update.

## [​](https://docs.openclaw.ai/install/updating\#after-updating)  After updating

1

[Navigate to header](https://docs.openclaw.ai/install/updating#run-doctor)

Run doctor

2

[Navigate to header](https://docs.openclaw.ai/install/updating#)

```
openclaw doctor
```

3

[Navigate to header](https://docs.openclaw.ai/install/updating#)

Migrates config, audits DM policies, and checks gateway health. Details: [Doctor](https://docs.openclaw.ai/gateway/doctor)

4

[Navigate to header](https://docs.openclaw.ai/install/updating#restart-the-gateway)

Restart the gateway

5

[Navigate to header](https://docs.openclaw.ai/install/updating#)

```
openclaw gateway restart
```

6

[Navigate to header](https://docs.openclaw.ai/install/updating#verify)

Verify

7

[Navigate to header](https://docs.openclaw.ai/install/updating#)

```
openclaw health
```

## [​](https://docs.openclaw.ai/install/updating\#rollback)  Rollback

### [​](https://docs.openclaw.ai/install/updating\#pin-a-version-npm)  Pin a version (npm)

```
npm i -g openclaw@<version>
openclaw doctor
openclaw gateway restart
```

`npm view openclaw version` shows the current published version.

### [​](https://docs.openclaw.ai/install/updating\#pin-a-commit-source)  Pin a commit (source)

```
git fetch origin
git checkout "$(git rev-list -n 1 --before=\"2026-01-01\" origin/main)"
pnpm install && pnpm build
openclaw gateway restart
```

To return to latest: `git checkout main && git pull`.

## [​](https://docs.openclaw.ai/install/updating\#if-you-are-stuck)  If you are stuck

- Run `openclaw doctor` again and read the output carefully.
- For `openclaw update --channel dev` on source checkouts, the updater auto-bootstraps `pnpm` when needed. If you see a pnpm/corepack bootstrap error, install `pnpm` manually (or re-enable `corepack`) and rerun the update.
- Check: [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)
- Ask in Discord: [https://discord.gg/clawd](https://discord.gg/clawd)

## [​](https://docs.openclaw.ai/install/updating\#related)  Related

- [Install overview](https://docs.openclaw.ai/install): all installation methods.
- [Doctor](https://docs.openclaw.ai/gateway/doctor): health checks after updates.
- [Migrating](https://docs.openclaw.ai/install/migrating): major version migration guides.

[Node.js](https://docs.openclaw.ai/install/node) [Migration guide](https://docs.openclaw.ai/install/migrating)

Ctrl+I