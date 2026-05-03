---
source_url: https://docs.openclaw.ai/install/uninstall
title: "Uninstall - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/uninstall#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Maintenance

Uninstall

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Easy path (CLI still installed)](https://docs.openclaw.ai/install/uninstall#easy-path-cli-still-installed)
- [Manual service removal (CLI not installed)](https://docs.openclaw.ai/install/uninstall#manual-service-removal-cli-not-installed)
- [macOS (launchd)](https://docs.openclaw.ai/install/uninstall#macos-launchd)
- [Linux (systemd user unit)](https://docs.openclaw.ai/install/uninstall#linux-systemd-user-unit)
- [Windows (Scheduled Task)](https://docs.openclaw.ai/install/uninstall#windows-scheduled-task)
- [Normal install vs source checkout](https://docs.openclaw.ai/install/uninstall#normal-install-vs-source-checkout)
- [Normal install (install.sh / npm / pnpm / bun)](https://docs.openclaw.ai/install/uninstall#normal-install-install-sh-%2F-npm-%2F-pnpm-%2F-bun)
- [Source checkout (git clone)](https://docs.openclaw.ai/install/uninstall#source-checkout-git-clone)
- [Related](https://docs.openclaw.ai/install/uninstall#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Two paths:

- **Easy path** if `openclaw` is still installed.
- **Manual service removal** if the CLI is gone but the service is still running.

## [​](https://docs.openclaw.ai/install/uninstall\#easy-path-cli-still-installed)  Easy path (CLI still installed)

Recommended: use the built-in uninstaller:

```
openclaw uninstall
```

Non-interactive (automation / npx):

```
openclaw uninstall --all --yes --non-interactive
npx -y openclaw uninstall --all --yes --non-interactive
```

Manual steps (same result):

1. Stop the gateway service:

```
openclaw gateway stop
```

2. Uninstall the gateway service (launchd/systemd/schtasks):

```
openclaw gateway uninstall
```

3. Delete state + config:

```
rm -rf "${OPENCLAW_STATE_DIR:-$HOME/.openclaw}"
```

If you set `OPENCLAW_CONFIG_PATH` to a custom location outside the state dir, delete that file too.

4. Delete your workspace (optional, removes agent files):

```
rm -rf ~/.openclaw/workspace
```

5. Remove the CLI install (pick the one you used):

```
npm rm -g openclaw
pnpm remove -g openclaw
bun remove -g openclaw
```

6. If you installed the macOS app:

```
rm -rf /Applications/OpenClaw.app
```

Notes:

- If you used profiles (`--profile` / `OPENCLAW_PROFILE`), repeat step 3 for each state dir (defaults are `~/.openclaw-<profile>`).
- In remote mode, the state dir lives on the **gateway host**, so run steps 1-4 there too.

## [​](https://docs.openclaw.ai/install/uninstall\#manual-service-removal-cli-not-installed)  Manual service removal (CLI not installed)

Use this if the gateway service keeps running but `openclaw` is missing.

### [​](https://docs.openclaw.ai/install/uninstall\#macos-launchd)  macOS (launchd)

Default label is `ai.openclaw.gateway` (or `ai.openclaw.<profile>`; legacy `com.openclaw.*` may still exist):

```
launchctl bootout gui/$UID/ai.openclaw.gateway
rm -f ~/Library/LaunchAgents/ai.openclaw.gateway.plist
```

If you used a profile, replace the label and plist name with `ai.openclaw.<profile>`. Remove any legacy `com.openclaw.*` plists if present.

### [​](https://docs.openclaw.ai/install/uninstall\#linux-systemd-user-unit)  Linux (systemd user unit)

Default unit name is `openclaw-gateway.service` (or `openclaw-gateway-<profile>.service`):

```
systemctl --user disable --now openclaw-gateway.service
rm -f ~/.config/systemd/user/openclaw-gateway.service
systemctl --user daemon-reload
```

### [​](https://docs.openclaw.ai/install/uninstall\#windows-scheduled-task)  Windows (Scheduled Task)

Default task name is `OpenClaw Gateway` (or `OpenClaw Gateway (<profile>)`).
The task script lives under your state dir.

```
schtasks /Delete /F /TN "OpenClaw Gateway"
Remove-Item -Force "$env:USERPROFILE\.openclaw\gateway.cmd"
```

If you used a profile, delete the matching task name and `~\.openclaw-<profile>\gateway.cmd`.

## [​](https://docs.openclaw.ai/install/uninstall\#normal-install-vs-source-checkout)  Normal install vs source checkout

### [​](https://docs.openclaw.ai/install/uninstall\#normal-install-install-sh-/-npm-/-pnpm-/-bun)  Normal install (install.sh / npm / pnpm / bun)

If you used `https://openclaw.ai/install.sh` or `install.ps1`, the CLI was installed with `npm install -g openclaw@latest`.
Remove it with `npm rm -g openclaw` (or `pnpm remove -g` / `bun remove -g` if you installed that way).

### [​](https://docs.openclaw.ai/install/uninstall\#source-checkout-git-clone)  Source checkout (git clone)

If you run from a repo checkout (`git clone` \+ `openclaw ...` / `bun run openclaw ...`):

1. Uninstall the gateway service **before** deleting the repo (use the easy path above or manual service removal).
2. Delete the repo directory.
3. Remove state + workspace as shown above.

## [​](https://docs.openclaw.ai/install/uninstall\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [Migration guide](https://docs.openclaw.ai/install/migrating)

[Migrating from Hermes](https://docs.openclaw.ai/install/migrating-hermes) [Release Channels](https://docs.openclaw.ai/install/development-channels)

Ctrl+I