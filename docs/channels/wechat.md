---
source_url: https://docs.openclaw.ai/channels/wechat
title: "WeChat - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels/wechat#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Regional platforms

WeChat

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Naming](https://docs.openclaw.ai/channels/wechat#naming)
- [How it works](https://docs.openclaw.ai/channels/wechat#how-it-works)
- [Install](https://docs.openclaw.ai/channels/wechat#install)
- [Login](https://docs.openclaw.ai/channels/wechat#login)
- [Access control](https://docs.openclaw.ai/channels/wechat#access-control)
- [Compatibility](https://docs.openclaw.ai/channels/wechat#compatibility)
- [Sidecar process](https://docs.openclaw.ai/channels/wechat#sidecar-process)
- [Troubleshooting](https://docs.openclaw.ai/channels/wechat#troubleshooting)
- [Related docs](https://docs.openclaw.ai/channels/wechat#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw connects to WeChat through Tencent’s external
`@tencent-weixin/openclaw-weixin` channel plugin.Status: external plugin. Direct chats and media are supported. Group chats are not
advertised by the current plugin capability metadata.

## [​](https://docs.openclaw.ai/channels/wechat\#naming)  Naming

- **WeChat** is the user-facing name in these docs.
- **Weixin** is the name used by Tencent’s package and by the plugin id.
- `openclaw-weixin` is the OpenClaw channel id.
- `@tencent-weixin/openclaw-weixin` is the npm package.

Use `openclaw-weixin` in CLI commands and config paths.

## [​](https://docs.openclaw.ai/channels/wechat\#how-it-works)  How it works

The WeChat code does not live in the OpenClaw core repo. OpenClaw provides the
generic channel plugin contract, and the external plugin provides the
WeChat-specific runtime:

1. `openclaw plugins install` installs `@tencent-weixin/openclaw-weixin`.
2. The Gateway discovers the plugin manifest and loads the plugin entrypoint.
3. The plugin registers channel id `openclaw-weixin`.
4. `openclaw channels login --channel openclaw-weixin` starts QR login.
5. The plugin stores account credentials under the OpenClaw state directory.
6. When the Gateway starts, the plugin starts its Weixin monitor for each
configured account.
7. Inbound WeChat messages are normalized through the channel contract, routed to
the selected OpenClaw agent, and sent back through the plugin outbound path.

That separation matters: OpenClaw core should stay channel-agnostic. WeChat login,
Tencent iLink API calls, media upload/download, context tokens, and account
monitoring are owned by the external plugin.

## [​](https://docs.openclaw.ai/channels/wechat\#install)  Install

Quick install:

```
npx -y @tencent-weixin/openclaw-weixin-cli install
```

Manual install:

```
openclaw plugins install "@tencent-weixin/openclaw-weixin"
openclaw config set plugins.entries.openclaw-weixin.enabled true
```

Restart the Gateway after install:

```
openclaw gateway restart
```

## [​](https://docs.openclaw.ai/channels/wechat\#login)  Login

Run QR login on the same machine that runs the Gateway:

```
openclaw channels login --channel openclaw-weixin
```

Scan the QR code with WeChat on your phone and confirm the login. The plugin saves
the account token locally after a successful scan.To add another WeChat account, run the same login command again. For multiple
accounts, isolate direct-message sessions by account, channel, and sender:

```
openclaw config set session.dmScope per-account-channel-peer
```

## [​](https://docs.openclaw.ai/channels/wechat\#access-control)  Access control

Direct messages use the normal OpenClaw pairing and allowlist model for channel
plugins.Approve new senders:

```
openclaw pairing list openclaw-weixin
openclaw pairing approve openclaw-weixin <CODE>
```

For the full access-control model, see [Pairing](https://docs.openclaw.ai/channels/pairing).

## [​](https://docs.openclaw.ai/channels/wechat\#compatibility)  Compatibility

The plugin checks the host OpenClaw version at startup.

| Plugin line | OpenClaw version | npm tag |
| --- | --- | --- |
| `2.x` | `>=2026.3.22` | `latest` |
| `1.x` | `>=2026.1.0 <2026.3.22` | `legacy` |

If the plugin reports that your OpenClaw version is too old, either update
OpenClaw or install the legacy plugin line:

```
openclaw plugins install @tencent-weixin/openclaw-weixin@legacy
```

## [​](https://docs.openclaw.ai/channels/wechat\#sidecar-process)  Sidecar process

The WeChat plugin can run helper work beside the Gateway while it monitors the
Tencent iLink API. In issue #68451, that helper path exposed a bug in OpenClaw’s
generic stale-Gateway cleanup: a child process could try to clean up the parent
Gateway process, causing restart loops under process managers such as systemd.Current OpenClaw startup cleanup excludes the current process and its ancestors,
so a channel helper must not kill the Gateway that launched it. This fix is
generic; it is not a WeChat-specific path in core.

## [​](https://docs.openclaw.ai/channels/wechat\#troubleshooting)  Troubleshooting

Check install and status:

```
openclaw plugins list
openclaw channels status --probe
openclaw --version
```

If the channel shows as installed but does not connect, confirm that the plugin is
enabled and restart:

```
openclaw config set plugins.entries.openclaw-weixin.enabled true
openclaw gateway restart
```

If the Gateway restarts repeatedly after enabling WeChat, update both OpenClaw and
the plugin:

```
npm view @tencent-weixin/openclaw-weixin version
openclaw plugins install "@tencent-weixin/openclaw-weixin" --force
openclaw gateway restart
```

Temporary disable:

```
openclaw config set plugins.entries.openclaw-weixin.enabled false
openclaw gateway restart
```

## [​](https://docs.openclaw.ai/channels/wechat\#related-docs)  Related docs

- Channel overview: [Chat Channels](https://docs.openclaw.ai/channels)
- Pairing: [Pairing](https://docs.openclaw.ai/channels/pairing)
- Channel routing: [Channel Routing](https://docs.openclaw.ai/channels/channel-routing)
- Plugin architecture: [Plugin Architecture](https://docs.openclaw.ai/plugins/architecture)
- Channel plugin SDK: [Channel Plugin SDK](https://docs.openclaw.ai/plugins/sdk-channel-plugins)
- External package: [@tencent-weixin/openclaw-weixin](https://www.npmjs.com/package/@tencent-weixin/openclaw-weixin)

[LINE](https://docs.openclaw.ai/channels/line) [QQ bot](https://docs.openclaw.ai/channels/qqbot)

Ctrl+I