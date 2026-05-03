---
source_url: https://docs.openclaw.ai/cli/pairing
title: "Pairing - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/pairing#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Channels and messaging

Pairing

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw pairing](https://docs.openclaw.ai/cli/pairing#openclaw-pairing)
- [Commands](https://docs.openclaw.ai/cli/pairing#commands)
- [pairing list](https://docs.openclaw.ai/cli/pairing#pairing-list)
- [pairing approve](https://docs.openclaw.ai/cli/pairing#pairing-approve)
- [Notes](https://docs.openclaw.ai/cli/pairing#notes)
- [Related](https://docs.openclaw.ai/cli/pairing#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/pairing\#openclaw-pairing)  `openclaw pairing`

Approve or inspect DM pairing requests (for channels that support pairing).Related:

- Pairing flow: [Pairing](https://docs.openclaw.ai/channels/pairing)

## [​](https://docs.openclaw.ai/cli/pairing\#commands)  Commands

```
openclaw pairing list telegram
openclaw pairing list --channel telegram --account work
openclaw pairing list telegram --json

openclaw pairing approve <code>
openclaw pairing approve telegram <code>
openclaw pairing approve --channel telegram --account work <code> --notify
```

## [​](https://docs.openclaw.ai/cli/pairing\#pairing-list)  `pairing list`

List pending pairing requests for one channel.Options:

- `[channel]`: positional channel id
- `--channel <channel>`: explicit channel id
- `--account <accountId>`: account id for multi-account channels
- `--json`: machine-readable output

Notes:

- If multiple pairing-capable channels are configured, you must provide a channel either positionally or with `--channel`.
- Extension channels are allowed as long as the channel id is valid.

## [​](https://docs.openclaw.ai/cli/pairing\#pairing-approve)  `pairing approve`

Approve a pending pairing code and allow that sender.Usage:

- `openclaw pairing approve <channel> <code>`
- `openclaw pairing approve --channel <channel> <code>`
- `openclaw pairing approve <code>` when exactly one pairing-capable channel is configured

Options:

- `--channel <channel>`: explicit channel id
- `--account <accountId>`: account id for multi-account channels
- `--notify`: send a confirmation back to the requester on the same channel

Owner bootstrap:

- If `commands.ownerAllowFrom` is empty when you approve a pairing code, OpenClaw also records the approved sender as the command owner, using a channel-scoped entry such as `telegram:123456789`.
- This only bootstraps the first owner. Later pairing approvals do not replace or expand `commands.ownerAllowFrom`.
- The command owner is the human operator account allowed to run owner-only commands and approve dangerous actions such as `/diagnostics`, `/export-trajectory`, `/config`, and exec approvals.

## [​](https://docs.openclaw.ai/cli/pairing\#notes)  Notes

- Channel input: pass it positionally (`pairing list telegram`) or with `--channel <channel>`.
- `pairing list` supports `--account <accountId>` for multi-account channels.
- `pairing approve` supports `--account <accountId>` and `--notify`.
- If only one pairing-capable channel is configured, `pairing approve <code>` is allowed.
- If you approved a sender before this bootstrap existed, run `openclaw doctor`; it warns when no command owner is configured and shows the `openclaw config set commands.ownerAllowFrom ...` command to fix it.

## [​](https://docs.openclaw.ai/cli/pairing\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Channel pairing](https://docs.openclaw.ai/channels/pairing)

[Directory](https://docs.openclaw.ai/cli/directory) [QR](https://docs.openclaw.ai/cli/qr)

Ctrl+I