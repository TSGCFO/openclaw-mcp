---
source_url: https://docs.openclaw.ai/platforms/linux
title: "Linux app - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/platforms/linux#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Platforms overview

Linux app

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Beginner quick path (VPS)](https://docs.openclaw.ai/platforms/linux#beginner-quick-path-vps)
- [Install](https://docs.openclaw.ai/platforms/linux#install)
- [Gateway](https://docs.openclaw.ai/platforms/linux#gateway)
- [Gateway service install (CLI)](https://docs.openclaw.ai/platforms/linux#gateway-service-install-cli)
- [System control (systemd user unit)](https://docs.openclaw.ai/platforms/linux#system-control-systemd-user-unit)
- [Memory pressure and OOM kills](https://docs.openclaw.ai/platforms/linux#memory-pressure-and-oom-kills)
- [Related](https://docs.openclaw.ai/platforms/linux#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The Gateway is fully supported on Linux. **Node is the recommended runtime**.
Bun is not recommended for the Gateway (WhatsApp/Telegram bugs).Native Linux companion apps are planned. Contributions are welcome if you want to help build one.

## [​](https://docs.openclaw.ai/platforms/linux\#beginner-quick-path-vps)  Beginner quick path (VPS)

1. Install Node 24 (recommended; Node 22 LTS, currently `22.14+`, still works for compatibility)
2. `npm i -g openclaw@latest`
3. `openclaw onboard --install-daemon`
4. From your laptop: `ssh -N -L 18789:127.0.0.1:18789 <user>@<host>`
5. Open `http://127.0.0.1:18789/` and authenticate with the configured shared secret (token by default; password if you set `gateway.auth.mode: "password"`)

Full Linux server guide: [Linux Server](https://docs.openclaw.ai/vps). Step-by-step VPS example: [exe.dev](https://docs.openclaw.ai/install/exe-dev)

## [​](https://docs.openclaw.ai/platforms/linux\#install)  Install

- [Getting Started](https://docs.openclaw.ai/start/getting-started)
- [Install & updates](https://docs.openclaw.ai/install/updating)
- Optional flows: [Bun (experimental)](https://docs.openclaw.ai/install/bun), [Nix](https://docs.openclaw.ai/install/nix), [Docker](https://docs.openclaw.ai/install/docker)

## [​](https://docs.openclaw.ai/platforms/linux\#gateway)  Gateway

- [Gateway runbook](https://docs.openclaw.ai/gateway)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)

## [​](https://docs.openclaw.ai/platforms/linux\#gateway-service-install-cli)  Gateway service install (CLI)

Use one of these:

```
openclaw onboard --install-daemon
```

Or:

```
openclaw gateway install
```

Or:

```
openclaw configure
```

Select **Gateway service** when prompted.Repair/migrate:

```
openclaw doctor
```

## [​](https://docs.openclaw.ai/platforms/linux\#system-control-systemd-user-unit)  System control (systemd user unit)

OpenClaw installs a systemd **user** service by default. Use a **system**
service for shared or always-on servers. `openclaw gateway install` and
`openclaw onboard --install-daemon` already render the current canonical unit
for you; write one by hand only when you need a custom system/service-manager
setup. The full service guidance lives in the [Gateway runbook](https://docs.openclaw.ai/gateway).Minimal setup:Create `~/.config/systemd/user/openclaw-gateway[-<profile>].service`:

```
[Unit]
Description=OpenClaw Gateway (profile: <profile>, v<version>)
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/local/bin/openclaw gateway --port 18789
Restart=always
RestartSec=5
TimeoutStopSec=30
TimeoutStartSec=30
SuccessExitStatus=0 143
KillMode=control-group

[Install]
WantedBy=default.target
```

Enable it:

```
systemctl --user enable --now openclaw-gateway[-<profile>].service
```

## [​](https://docs.openclaw.ai/platforms/linux\#memory-pressure-and-oom-kills)  Memory pressure and OOM kills

On Linux, the kernel chooses an OOM victim when a host, VM, or container cgroup
runs out of memory. The Gateway can be a poor victim because it owns long-lived
sessions and channel connections. OpenClaw therefore biases transient child
processes to be killed before the Gateway when possible.For eligible Linux child spawns, OpenClaw starts the child through a short
`/bin/sh` wrapper that raises the child’s own `oom_score_adj` to `1000`, then
`exec`s the real command. This is an unprivileged operation because the child is
only increasing its own OOM kill likelihood.Covered child process surfaces include:

- supervisor-managed command children,
- PTY shell children,
- MCP stdio server children,
- OpenClaw-launched browser/Chrome processes.

The wrapper is Linux-only and is skipped when `/bin/sh` is unavailable. It is
also skipped if the child env sets `OPENCLAW_CHILD_OOM_SCORE_ADJ=0`, `false`,
`no`, or `off`.To verify a child process:

```
cat /proc/<child-pid>/oom_score_adj
```

Expected value for covered children is `1000`. The Gateway process should keep
its normal score, usually `0`.This does not replace normal memory tuning. If a VPS or container repeatedly
kills children, increase the memory limit, reduce concurrency, or add stronger
resource controls such as systemd `MemoryMax=` or container-level memory limits.

## [​](https://docs.openclaw.ai/platforms/linux\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [Linux server](https://docs.openclaw.ai/vps)
- [Raspberry Pi](https://docs.openclaw.ai/platforms/raspberry-pi)

[macOS app](https://docs.openclaw.ai/platforms/macos) [Windows](https://docs.openclaw.ai/platforms/windows)

Ctrl+I