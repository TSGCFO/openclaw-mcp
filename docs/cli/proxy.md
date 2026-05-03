---
source_url: https://docs.openclaw.ai/cli/proxy
title: "Proxy - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/proxy#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Utility

Proxy

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw proxy](https://docs.openclaw.ai/cli/proxy#openclaw-proxy)
- [Commands](https://docs.openclaw.ai/cli/proxy#commands)
- [Validate](https://docs.openclaw.ai/cli/proxy#validate)
- [Query presets](https://docs.openclaw.ai/cli/proxy#query-presets)
- [Notes](https://docs.openclaw.ai/cli/proxy#notes)
- [Related](https://docs.openclaw.ai/cli/proxy#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/proxy\#openclaw-proxy)  `openclaw proxy`

Validate operator-managed proxy routing, or run the local explicit debug proxy
and inspect captured traffic.Use `validate` to preflight an operator-managed forward proxy before enabling
OpenClaw proxy routing. The other commands are debugging tools for
transport-level investigation: they can start a local proxy, run a child command
with capture enabled, list capture sessions, query common traffic patterns, read
captured blobs, and purge local capture data.

## [​](https://docs.openclaw.ai/cli/proxy\#commands)  Commands

```
openclaw proxy start [--host <host>] [--port <port>]
openclaw proxy run [--host <host>] [--port <port>] -- <cmd...>
openclaw proxy validate [--json] [--proxy-url <url>] [--allowed-url <url>] [--denied-url <url>] [--timeout-ms <ms>]
openclaw proxy coverage
openclaw proxy sessions [--limit <count>]
openclaw proxy query --preset <name> [--session <id>]
openclaw proxy blob --id <blobId>
openclaw proxy purge
```

## [​](https://docs.openclaw.ai/cli/proxy\#validate)  Validate

`openclaw proxy validate` checks the effective operator-managed proxy URL from
`--proxy-url`, config, or `OPENCLAW_PROXY_URL`. It reports a config problem when
no proxy is enabled and configured; use `--proxy-url` for a one-off preflight
before changing config. By default it verifies that a public destination succeeds
through the proxy and that the proxy cannot reach a temporary loopback canary.
Custom denied destinations are fail-closed: HTTP responses and ambiguous
transport failures both fail unless you can verify a deployment-specific denial
signal separately.Options:

- `--json`: print machine-readable JSON.
- `--proxy-url <url>`: validate this proxy URL instead of config or env.
- `--allowed-url <url>`: add a destination expected to succeed through the proxy. Repeat to check multiple destinations.
- `--denied-url <url>`: add a destination expected to be blocked by the proxy. Repeat to check multiple destinations.
- `--timeout-ms <ms>`: per-request timeout in milliseconds.

See [Network Proxy](https://docs.openclaw.ai/security/network-proxy) for deployment guidance and denial
semantics.

## [​](https://docs.openclaw.ai/cli/proxy\#query-presets)  Query presets

`openclaw proxy query --preset <name>` accepts:

- `double-sends`
- `retry-storms`
- `cache-busting`
- `ws-duplicate-frames`
- `missing-ack`
- `error-bursts`

## [​](https://docs.openclaw.ai/cli/proxy\#notes)  Notes

- `start` defaults to `127.0.0.1` unless `--host` is set.
- `run` starts a local debug proxy and then runs the command after `--`.
- `validate` exits with code 1 when proxy config or destination checks fail.
- Captures are local debugging data; use `openclaw proxy purge` when finished.

## [​](https://docs.openclaw.ai/cli/proxy\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Network Proxy](https://docs.openclaw.ai/security/network-proxy)
- [Trusted proxy auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth)

[MCP](https://docs.openclaw.ai/cli/mcp) [Wiki](https://docs.openclaw.ai/cli/wiki)

Ctrl+I