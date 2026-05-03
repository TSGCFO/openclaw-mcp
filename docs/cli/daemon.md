---
source_url: https://docs.openclaw.ai/cli/daemon
title: "Daemon - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/daemon#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Gateway and service

Daemon

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw daemon](https://docs.openclaw.ai/cli/daemon#openclaw-daemon)
- [Usage](https://docs.openclaw.ai/cli/daemon#usage)
- [Subcommands](https://docs.openclaw.ai/cli/daemon#subcommands)
- [Common options](https://docs.openclaw.ai/cli/daemon#common-options)
- [Prefer](https://docs.openclaw.ai/cli/daemon#prefer)
- [Related](https://docs.openclaw.ai/cli/daemon#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/daemon\#openclaw-daemon)  `openclaw daemon`

Legacy alias for Gateway service management commands.`openclaw daemon ...` maps to the same service control surface as `openclaw gateway ...` service commands.

## [​](https://docs.openclaw.ai/cli/daemon\#usage)  Usage

```
openclaw daemon status
openclaw daemon install
openclaw daemon start
openclaw daemon stop
openclaw daemon restart
openclaw daemon uninstall
```

## [​](https://docs.openclaw.ai/cli/daemon\#subcommands)  Subcommands

- `status`: show service install state and probe Gateway health
- `install`: install service (`launchd`/`systemd`/`schtasks`)
- `uninstall`: remove service
- `start`: start service
- `stop`: stop service
- `restart`: restart service

## [​](https://docs.openclaw.ai/cli/daemon\#common-options)  Common options

- `status`: `--url`, `--token`, `--password`, `--timeout`, `--no-probe`, `--require-rpc`, `--deep`, `--json`
- `install`: `--port`, `--runtime <node|bun>`, `--token`, `--force`, `--json`
- `restart`: `--force`, `--wait <duration>`, `--json`
- lifecycle (`uninstall|start|stop`): `--json`

Notes:

- `status` resolves configured auth SecretRefs for probe auth when possible.
- If a required auth SecretRef is unresolved in this command path, `daemon status --json` reports `rpc.authWarning` when probe connectivity/auth fails; pass `--token`/`--password` explicitly or resolve the secret source first.
- If the probe succeeds, unresolved auth-ref warnings are suppressed to avoid false positives.
- `status --deep` adds a best-effort system-level service scan. When it finds other gateway-like services, human output prints cleanup hints and warns that one gateway per machine is still the normal recommendation.
- On Linux systemd installs, `status` token-drift checks include both `Environment=` and `EnvironmentFile=` unit sources.
- Drift checks resolve `gateway.auth.token` SecretRefs using merged runtime env (service command env first, then process env fallback).
- If token auth is not effectively active (explicit `gateway.auth.mode` of `password`/`none`/`trusted-proxy`, or mode unset where password can win and no token candidate can win), token-drift checks skip config token resolution.
- When token auth requires a token and `gateway.auth.token` is SecretRef-managed, `install` validates that the SecretRef is resolvable but does not persist the resolved token into service environment metadata.
- If token auth requires a token and the configured token SecretRef is unresolved, install fails closed.
- If both `gateway.auth.token` and `gateway.auth.password` are configured and `gateway.auth.mode` is unset, install is blocked until mode is set explicitly.
- On macOS, `install` keeps LaunchAgent plists owner-only and loads managed service environment values through an owner-only file and wrapper instead of serializing API keys or auth-profile env refs into `EnvironmentVariables`.
- If you intentionally run multiple gateways on one host, isolate ports, config/state, and workspaces; see [/gateway#multiple-gateways-same-host](https://docs.openclaw.ai/gateway#multiple-gateways-same-host).

## [​](https://docs.openclaw.ai/cli/daemon\#prefer)  Prefer

Use [`openclaw gateway`](https://docs.openclaw.ai/cli/gateway) for current docs and examples.

## [​](https://docs.openclaw.ai/cli/daemon\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Gateway runbook](https://docs.openclaw.ai/gateway)

[Crestodian](https://docs.openclaw.ai/cli/crestodian) [Doctor](https://docs.openclaw.ai/cli/doctor)

Ctrl+I