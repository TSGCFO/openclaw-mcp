---
source_url: https://docs.openclaw.ai/channels/nextcloud-talk
title: "Nextcloud Talk - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels/nextcloud-talk#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Developer and self-hosted

Nextcloud Talk

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Bundled plugin](https://docs.openclaw.ai/channels/nextcloud-talk#bundled-plugin)
- [Quick setup (beginner)](https://docs.openclaw.ai/channels/nextcloud-talk#quick-setup-beginner)
- [Notes](https://docs.openclaw.ai/channels/nextcloud-talk#notes)
- [Access control (DMs)](https://docs.openclaw.ai/channels/nextcloud-talk#access-control-dms)
- [Rooms (groups)](https://docs.openclaw.ai/channels/nextcloud-talk#rooms-groups)
- [Capabilities](https://docs.openclaw.ai/channels/nextcloud-talk#capabilities)
- [Configuration reference (Nextcloud Talk)](https://docs.openclaw.ai/channels/nextcloud-talk#configuration-reference-nextcloud-talk)
- [Related](https://docs.openclaw.ai/channels/nextcloud-talk#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Status: bundled plugin (webhook bot). Direct messages, rooms, reactions, and markdown messages are supported.

## [​](https://docs.openclaw.ai/channels/nextcloud-talk\#bundled-plugin)  Bundled plugin

Nextcloud Talk ships as a bundled plugin in current OpenClaw releases, so
normal packaged builds do not need a separate install.If you are on an older build or a custom install that excludes Nextcloud Talk,
install a current npm package when one is published:Install via CLI (npm registry, when a current package exists):

```
openclaw plugins install @openclaw/nextcloud-talk
```

If npm reports the OpenClaw-owned package as deprecated, use a current packaged
OpenClaw build or the local checkout path until a newer npm package is
published.Local checkout (when running from a git repo):

```
openclaw plugins install ./path/to/local/nextcloud-talk-plugin
```

Details: [Plugins](https://docs.openclaw.ai/tools/plugin)

## [​](https://docs.openclaw.ai/channels/nextcloud-talk\#quick-setup-beginner)  Quick setup (beginner)

1. Ensure the Nextcloud Talk plugin is available.   - Current packaged OpenClaw releases already bundle it.
   - Older/custom installs can add it manually with the commands above.
2. On your Nextcloud server, create a bot:














```
./occ talk:bot:install "OpenClaw" "<shared-secret>" "<webhook-url>" --feature reaction
```

3. Enable the bot in the target room settings.
4. Configure OpenClaw:

   - Config: `channels.nextcloud-talk.baseUrl` \+ `channels.nextcloud-talk.botSecret`
   - Or env: `NEXTCLOUD_TALK_BOT_SECRET` (default account only)

CLI setup:

```
openclaw channels add --channel nextcloud-talk \
  --url https://cloud.example.com \
  --token "<shared-secret>"
```

Equivalent explicit fields:

```
openclaw channels add --channel nextcloud-talk \
  --base-url https://cloud.example.com \
  --secret "<shared-secret>"
```

File-backed secret:

```
openclaw channels add --channel nextcloud-talk \
  --base-url https://cloud.example.com \
  --secret-file /path/to/nextcloud-talk-secret
```

5. Restart the gateway (or finish setup).

Minimal config:

```
{
  channels: {
    "nextcloud-talk": {
      enabled: true,
      baseUrl: "https://cloud.example.com",
      botSecret: "shared-secret",
      dmPolicy: "pairing",
    },
  },
}
```

## [​](https://docs.openclaw.ai/channels/nextcloud-talk\#notes)  Notes

- Bots cannot initiate DMs. The user must message the bot first.
- Webhook URL must be reachable by the Gateway; set `webhookPublicUrl` if behind a proxy.
- Media uploads are not supported by the bot API; media is sent as URLs.
- The webhook payload does not distinguish DMs vs rooms; set `apiUser` \+ `apiPassword` to enable room-type lookups (otherwise DMs are treated as rooms).

## [​](https://docs.openclaw.ai/channels/nextcloud-talk\#access-control-dms)  Access control (DMs)

- Default: `channels.nextcloud-talk.dmPolicy = "pairing"`. Unknown senders get a pairing code.
- Approve via:
  - `openclaw pairing list nextcloud-talk`
  - `openclaw pairing approve nextcloud-talk <CODE>`
- Public DMs: `channels.nextcloud-talk.dmPolicy="open"` plus `channels.nextcloud-talk.allowFrom=["*"]`.
- `allowFrom` matches Nextcloud user IDs only; display names are ignored.

## [​](https://docs.openclaw.ai/channels/nextcloud-talk\#rooms-groups)  Rooms (groups)

- Default: `channels.nextcloud-talk.groupPolicy = "allowlist"` (mention-gated).
- Allowlist rooms with `channels.nextcloud-talk.rooms`:

```
{
  channels: {
    "nextcloud-talk": {
      rooms: {
        "room-token": { requireMention: true },
      },
    },
  },
}
```

- To allow no rooms, keep the allowlist empty or set `channels.nextcloud-talk.groupPolicy="disabled"`.

## [​](https://docs.openclaw.ai/channels/nextcloud-talk\#capabilities)  Capabilities

| Feature | Status |
| --- | --- |
| Direct messages | Supported |
| Rooms | Supported |
| Threads | Not supported |
| Media | URL-only |
| Reactions | Supported |
| Native commands | Not supported |

## [​](https://docs.openclaw.ai/channels/nextcloud-talk\#configuration-reference-nextcloud-talk)  Configuration reference (Nextcloud Talk)

Full configuration: [Configuration](https://docs.openclaw.ai/gateway/configuration)Provider options:

- `channels.nextcloud-talk.enabled`: enable/disable channel startup.
- `channels.nextcloud-talk.baseUrl`: Nextcloud instance URL.
- `channels.nextcloud-talk.botSecret`: bot shared secret.
- `channels.nextcloud-talk.botSecretFile`: regular-file secret path. Symlinks are rejected.
- `channels.nextcloud-talk.apiUser`: API user for room lookups (DM detection).
- `channels.nextcloud-talk.apiPassword`: API/app password for room lookups.
- `channels.nextcloud-talk.apiPasswordFile`: API password file path.
- `channels.nextcloud-talk.webhookPort`: webhook listener port (default: 8788).
- `channels.nextcloud-talk.webhookHost`: webhook host (default: 0.0.0.0).
- `channels.nextcloud-talk.webhookPath`: webhook path (default: /nextcloud-talk-webhook).
- `channels.nextcloud-talk.webhookPublicUrl`: externally reachable webhook URL.
- `channels.nextcloud-talk.dmPolicy`: `pairing | allowlist | open | disabled`.
- `channels.nextcloud-talk.allowFrom`: DM allowlist (user IDs). `open` requires `"*"`.
- `channels.nextcloud-talk.groupPolicy`: `allowlist | open | disabled`.
- `channels.nextcloud-talk.groupAllowFrom`: group allowlist (user IDs).
- `channels.nextcloud-talk.rooms`: per-room settings and allowlist.
- `channels.nextcloud-talk.historyLimit`: group history limit (0 disables).
- `channels.nextcloud-talk.dmHistoryLimit`: DM history limit (0 disables).
- `channels.nextcloud-talk.dms`: per-DM overrides (historyLimit).
- `channels.nextcloud-talk.textChunkLimit`: outbound text chunk size (chars).
- `channels.nextcloud-talk.chunkMode`: `length` (default) or `newline` to split on blank lines (paragraph boundaries) before length chunking.
- `channels.nextcloud-talk.blockStreaming`: disable block streaming for this channel.
- `channels.nextcloud-talk.blockStreamingCoalesce`: block streaming coalesce tuning.
- `channels.nextcloud-talk.mediaMaxMb`: inbound media cap (MB).

## [​](https://docs.openclaw.ai/channels/nextcloud-talk\#related)  Related

- [Channels Overview](https://docs.openclaw.ai/channels) — all supported channels
- [Pairing](https://docs.openclaw.ai/channels/pairing) — DM authentication and pairing flow
- [Groups](https://docs.openclaw.ai/channels/groups) — group chat behavior and mention gating
- [Channel Routing](https://docs.openclaw.ai/channels/channel-routing) — session routing for messages
- [Security](https://docs.openclaw.ai/gateway/security) — access model and hardening

[Mattermost](https://docs.openclaw.ai/channels/mattermost) [Nostr](https://docs.openclaw.ai/channels/nostr)

Ctrl+I