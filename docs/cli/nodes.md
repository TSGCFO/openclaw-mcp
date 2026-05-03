---
source_url: https://docs.openclaw.ai/cli/nodes
title: "Nodes - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/nodes#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Tools and execution

Nodes

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw nodes](https://docs.openclaw.ai/cli/nodes#openclaw-nodes)
- [Common commands](https://docs.openclaw.ai/cli/nodes#common-commands)
- [Invoke](https://docs.openclaw.ai/cli/nodes#invoke)
- [Related](https://docs.openclaw.ai/cli/nodes#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/nodes\#openclaw-nodes)  `openclaw nodes`

Manage paired nodes (devices) and invoke node capabilities.Related:

- Nodes overview: [Nodes](https://docs.openclaw.ai/nodes)
- Camera: [Camera nodes](https://docs.openclaw.ai/nodes/camera)
- Images: [Image nodes](https://docs.openclaw.ai/nodes/images)

Common options:

- `--url`, `--token`, `--timeout`, `--json`

## [​](https://docs.openclaw.ai/cli/nodes\#common-commands)  Common commands

```
openclaw nodes list
openclaw nodes list --connected
openclaw nodes list --last-connected 24h
openclaw nodes pending
openclaw nodes approve <requestId>
openclaw nodes reject <requestId>
openclaw nodes remove --node <id|name|ip>
openclaw nodes rename --node <id|name|ip> --name <displayName>
openclaw nodes status
openclaw nodes status --connected
openclaw nodes status --last-connected 24h
```

`nodes list` prints pending/paired tables. Paired rows include the most recent connect age (Last Connect).
Use `--connected` to only show currently-connected nodes. Use `--last-connected <duration>` to
filter to nodes that connected within a duration (e.g. `24h`, `7d`).
Use `nodes remove --node <id|name|ip>` to delete a stale gateway-owned node pairing record.Approval note:

- `openclaw nodes pending` only needs pairing scope.
- `gateway.nodes.pairing.autoApproveCidrs` can skip the pending step only for
explicitly trusted, first-time `role: node` device pairing. It is off by
default and does not approve upgrades.
- `openclaw nodes approve <requestId>`inherits extra scope requirements from the
pending request:

  - commandless request: pairing only
  - non-exec node commands: pairing + write
  - `system.run` / `system.run.prepare` / `system.which`: pairing + admin

## [​](https://docs.openclaw.ai/cli/nodes\#invoke)  Invoke

```
openclaw nodes invoke --node <id|name|ip> --command <command> --params <json>
```

Invoke flags:

- `--params <json>`: JSON object string (default `{}`).
- `--invoke-timeout <ms>`: node invoke timeout (default `15000`).
- `--idempotency-key <key>`: optional idempotency key.
- `system.run` and `system.run.prepare` are blocked here; use the `exec` tool with `host=node` for shell execution.

For shell execution on a node, use the `exec` tool with `host=node` instead of `openclaw nodes run`.
The `nodes` CLI is now capability-focused: direct RPC via `nodes invoke`, plus pairing, camera,
screen, location, canvas, and notifications.

## [​](https://docs.openclaw.ai/cli/nodes\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Nodes](https://docs.openclaw.ai/nodes)

[Node](https://docs.openclaw.ai/cli/node) [Sandbox CLI](https://docs.openclaw.ai/cli/sandbox)

Ctrl+I