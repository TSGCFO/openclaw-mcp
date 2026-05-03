---
source_url: https://docs.openclaw.ai/cli/directory
title: "Directory - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/directory#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Channels and messaging

Directory

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw directory](https://docs.openclaw.ai/cli/directory#openclaw-directory)
- [Common flags](https://docs.openclaw.ai/cli/directory#common-flags)
- [Notes](https://docs.openclaw.ai/cli/directory#notes)
- [Using results with message send](https://docs.openclaw.ai/cli/directory#using-results-with-message-send)
- [ID formats (by channel)](https://docs.openclaw.ai/cli/directory#id-formats-by-channel)
- [Self (“me”)](https://docs.openclaw.ai/cli/directory#self-%E2%80%9Cme%E2%80%9D)
- [Peers (contacts/users)](https://docs.openclaw.ai/cli/directory#peers-contacts%2Fusers)
- [Groups](https://docs.openclaw.ai/cli/directory#groups)
- [Related](https://docs.openclaw.ai/cli/directory#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/directory\#openclaw-directory)  `openclaw directory`

Directory lookups for channels that support it (contacts/peers, groups, and “me”).

## [​](https://docs.openclaw.ai/cli/directory\#common-flags)  Common flags

- `--channel <name>`: channel id/alias (required when multiple channels are configured; auto when only one is configured)
- `--account <id>`: account id (default: channel default)
- `--json`: output JSON

## [​](https://docs.openclaw.ai/cli/directory\#notes)  Notes

- `directory` is meant to help you find IDs you can paste into other commands (especially `openclaw message send --target ...`).
- For many channels, results are config-backed (allowlists / configured groups) rather than a live provider directory.
- Installed channel plugins can still omit directory support; in that case the command reports the unsupported directory operation instead of reinstalling the plugin.
- Default output is `id` (and sometimes `name`) separated by a tab; use `--json` for scripting.

## [​](https://docs.openclaw.ai/cli/directory\#using-results-with-message-send)  Using results with `message send`

```
openclaw directory peers list --channel slack --query "U0"
openclaw message send --channel slack --target user:U012ABCDEF --message "hello"
```

## [​](https://docs.openclaw.ai/cli/directory\#id-formats-by-channel)  ID formats (by channel)

- WhatsApp: `+15551234567` (DM), `1234567890-1234567890@g.us` (group), `120363123456789@newsletter` (Channel/Newsletter outbound target)
- Telegram: `@username` or numeric chat id; groups are numeric ids
- Slack: `user:U…` and `channel:C…`
- Discord: `user:<id>` and `channel:<id>`
- Matrix (plugin): `user:@user:server`, `room:!roomId:server`, or `#alias:server`
- Microsoft Teams (plugin): `user:<id>` and `conversation:<id>`
- Zalo (plugin): user id (Bot API)
- Zalo Personal / `zalouser` (plugin): thread id (DM/group) from `zca` (`me`, `friend list`, `group list`)

## [​](https://docs.openclaw.ai/cli/directory\#self-%E2%80%9Cme%E2%80%9D)  Self (“me”)

```
openclaw directory self --channel zalouser
```

## [​](https://docs.openclaw.ai/cli/directory\#peers-contacts/users)  Peers (contacts/users)

```
openclaw directory peers list --channel zalouser
openclaw directory peers list --channel zalouser --query "name"
openclaw directory peers list --channel zalouser --limit 50
```

## [​](https://docs.openclaw.ai/cli/directory\#groups)  Groups

```
openclaw directory groups list --channel zalouser
openclaw directory groups list --channel zalouser --query "work"
openclaw directory groups members --channel zalouser --group-id <id>
```

## [​](https://docs.openclaw.ai/cli/directory\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)

[Devices](https://docs.openclaw.ai/cli/devices) [Pairing](https://docs.openclaw.ai/cli/pairing)

Ctrl+I