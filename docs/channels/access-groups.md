---
source_url: https://docs.openclaw.ai/channels/access-groups
title: "Access groups - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels/access-groups#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Configuration

Access groups

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Static message sender groups](https://docs.openclaw.ai/channels/access-groups#static-message-sender-groups)
- [Reference groups from allowlists](https://docs.openclaw.ai/channels/access-groups#reference-groups-from-allowlists)
- [Supported message-channel paths](https://docs.openclaw.ai/channels/access-groups#supported-message-channel-paths)
- [Discord channel audiences](https://docs.openclaw.ai/channels/access-groups#discord-channel-audiences)
- [Security notes](https://docs.openclaw.ai/channels/access-groups#security-notes)
- [Troubleshooting](https://docs.openclaw.ai/channels/access-groups#troubleshooting)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Access groups are named sender lists you define once and reference from channel allowlists with `accessGroup:<name>`.Use them when the same people should be allowed across several message channels, or when one trusted set should apply to both DMs and group sender authorization.Access groups do not grant access by themselves. A group only matters when an allowlist field references it.

## [​](https://docs.openclaw.ai/channels/access-groups\#static-message-sender-groups)  Static message sender groups

Static sender groups use `type: "message.senders"`.

```
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        "*": ["global-owner-id"],
        discord: ["discord:123456789012345678"],
        telegram: ["987654321"],
        whatsapp: ["+15551234567"],
      },
    },
  },
}
```

Member lists are keyed by message-channel id:

| Key | Meaning |
| --- | --- |
| `"*"` | Shared entries checked for every message channel that references group. |
| `discord` | Entries checked only for Discord allowlist matching. |
| `telegram` | Entries checked only for Telegram allowlist matching. |
| `whatsapp` | Entries checked only for WhatsApp allowlist matching. |

Entries are matched with the destination channel’s normal `allowFrom` rules. OpenClaw does not translate sender ids between channels. If Alice has a Telegram id and a Discord id, list both ids under the appropriate keys.

## [​](https://docs.openclaw.ai/channels/access-groups\#reference-groups-from-allowlists)  Reference groups from allowlists

Reference a group with `accessGroup:<name>` anywhere the message channel path supports sender allowlists.DM allowlist example:

```
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        discord: ["discord:123456789012345678"],
        telegram: ["987654321"],
      },
    },
  },
  channels: {
    discord: {
      dmPolicy: "allowlist",
      allowFrom: ["accessGroup:operators"],
    },
    telegram: {
      dmPolicy: "allowlist",
      allowFrom: ["accessGroup:operators"],
    },
  },
}
```

Group sender allowlist example:

```
{
  accessGroups: {
    oncall: {
      type: "message.senders",
      members: {
        whatsapp: ["+15551234567"],
        googlechat: ["users/1234567890"],
      },
    },
  },
  channels: {
    whatsapp: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["accessGroup:oncall"],
    },
    googlechat: {
      spaces: {
        "spaces/AAA": {
          users: ["accessGroup:oncall"],
        },
      },
    },
  },
}
```

You can mix groups and direct entries:

```
{
  channels: {
    discord: {
      dmPolicy: "allowlist",
      allowFrom: ["accessGroup:operators", "discord:123456789012345678"],
    },
  },
}
```

## [​](https://docs.openclaw.ai/channels/access-groups\#supported-message-channel-paths)  Supported message-channel paths

Access groups are available in shared message-channel authorization paths, including:

- DM sender allowlists such as `channels.<channel>.allowFrom`
- group sender allowlists such as `channels.<channel>.groupAllowFrom`
- channel-specific per-room sender allowlists that use the same sender matching rules
- command authorization paths that reuse message-channel sender allowlists

Channel support depends on whether that channel is wired through the shared OpenClaw sender-authorization helpers. Current bundled support includes Discord, Google Chat, Nostr, WhatsApp, Zalo, and Zalo Personal. Static `message.senders` groups are designed to be channel-agnostic, so new message channels should support them by using the shared plugin SDK helpers instead of custom allowlist expansion.

## [​](https://docs.openclaw.ai/channels/access-groups\#discord-channel-audiences)  Discord channel audiences

Discord also supports a dynamic access group type:

```
{
  accessGroups: {
    maintainers: {
      type: "discord.channelAudience",
      guildId: "1456350064065904867",
      channelId: "1456744319972282449",
      membership: "canViewChannel",
    },
  },
  channels: {
    discord: {
      dmPolicy: "allowlist",
      allowFrom: ["accessGroup:maintainers"],
    },
  },
}
```

`discord.channelAudience` means “allow Discord DM senders who can currently view this guild channel.” OpenClaw resolves the sender through Discord at authorization time and applies Discord `ViewChannel` permission rules.Use this when a Discord channel is already the source of truth for a team, such as `#maintainers` or `#on-call`.Requirements and failure behavior:

- The bot needs access to the guild and channel.
- The bot needs the Discord Developer Portal **Server Members Intent**.
- The access group fails closed when Discord returns `Missing Access`, the sender cannot be resolved as a guild member, or the channel belongs to another guild.

More Discord-specific examples: [Discord access control](https://docs.openclaw.ai/channels/discord#access-control-and-routing)

## [​](https://docs.openclaw.ai/channels/access-groups\#security-notes)  Security notes

- Access groups are allowlist aliases, not roles. They do not create owners, approve pairing requests, or grant tool permissions by themselves.
- `dmPolicy: "open"` still requires `"*"` in the effective DM allowlist. Referencing an access group is not the same as public access.
- Missing group names fail closed. If `allowFrom` contains `accessGroup:operators` and `accessGroups.operators` is absent, that entry authorizes nobody.
- Keep channel ids stable. Prefer numeric/user ids over display names when the channel supports both.

## [​](https://docs.openclaw.ai/channels/access-groups\#troubleshooting)  Troubleshooting

If a sender should match but is blocked:

1. Confirm the allowlist field contains the exact `accessGroup:<name>` reference.
2. Confirm `accessGroups.<name>.type` is correct.
3. Confirm the sender id is listed under the matching channel key, or under `"*"`.
4. Confirm the entry uses that channel’s normal allowlist syntax.
5. For Discord channel audiences, confirm the bot can see the guild channel and has Server Members Intent enabled.

Run `openclaw doctor` after editing access-control config. It catches many invalid allowlist and policy combinations before runtime.

[Pairing](https://docs.openclaw.ai/channels/pairing) [Group messages](https://docs.openclaw.ai/channels/group-messages)

Ctrl+I