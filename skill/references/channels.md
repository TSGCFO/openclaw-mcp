# Channels

_27 pages from docs.openclaw.ai_


---

## Chat channels - OpenClaw

_Source: <https://docs.openclaw.ai/channels>_

[OpenClaw home page](https://docs.openclaw.ai/)

Overview

Chat channels

OpenClaw can talk to you on any chat app you already use. Each channel connects via the Gateway.
Text is supported everywhere; media and reactions vary by channel.

## Delivery notes

- Telegram replies that contain markdown image syntax, such as `![alt](url)`,
are converted into media replies on the final outbound path when possible.
- Slack multi-person DMs route as group chats, so group policy, mention
behavior, and group-session rules apply to MPIM conversations.
- WhatsApp setup is install-on-demand: onboarding can show the setup flow before
the plugin package is installed, and the Gateway loads the WhatsApp runtime
only when the channel is actually active.

## Supported channels

- [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles) — **Recommended for iMessage**; uses the BlueBubbles macOS server REST API with full feature support (bundled plugin; edit, unsend, effects, reactions, group management — edit currently broken on macOS 26 Tahoe).
- [Discord](https://docs.openclaw.ai/channels/discord) — Discord Bot API + Gateway; supports servers, channels, and DMs.
- [Feishu](https://docs.openclaw.ai/channels/feishu) — Feishu/Lark bot via WebSocket (bundled plugin).
- [Google Chat](https://docs.openclaw.ai/channels/googlechat) — Google Chat API app via HTTP webhook (downloadable plugin).
- [iMessage (legacy)](https://docs.openclaw.ai/channels/imessage) — Legacy macOS integration via imsg CLI (deprecated, use BlueBubbles for new setups).
- [IRC](https://docs.openclaw.ai/channels/irc) — Classic IRC servers; channels + DMs with pairing/allowlist controls.
- [LINE](https://docs.openclaw.ai/channels/line) — LINE Messaging API bot (downloadable plugin).
- [Matrix](https://docs.openclaw.ai/channels/matrix) — Matrix protocol (downloadable plugin).
- [Mattermost](https://docs.openclaw.ai/channels/mattermost) — Bot API + WebSocket; channels, groups, DMs (downloadable plugin).
- [Microsoft Teams](https://docs.openclaw.ai/channels/msteams) — Bot Framework; enterprise support (bundled plugin).
- [Nextcloud Talk](https://docs.openclaw.ai/channels/nextcloud-talk) — Self-hosted chat via Nextcloud Talk (bundled plugin).
- [Nostr](https://docs.openclaw.ai/channels/nostr) — Decentralized DMs via NIP-04 (bundled plugin).
- [QQ Bot](https://docs.openclaw.ai/channels/qqbot) — QQ Bot API; private chat, group chat, and rich media (bundled plugin).
- [Signal](https://docs.openclaw.ai/channels/signal) — signal-cli; privacy-focused.
- [Slack](https://docs.openclaw.ai/channels/slack) — Bolt SDK; workspace apps.
- [Synology Chat](https://docs.openclaw.ai/channels/synology-chat) — Synology NAS Chat via outgoing+incoming webhooks (bundled plugin).
- [Telegram](https://docs.openclaw.ai/channels/telegram) — Bot API via grammY; supports groups.
- [Tlon](https://docs.openclaw.ai/channels/tlon) — Urbit-based messenger (bundled plugin).
- [Twitch](https://docs.openclaw.ai/channels/twitch) — Twitch chat via IRC connection (bundled plugin).
- [Voice Call](https://docs.openclaw.ai/plugins/voice-call) — Telephony via Plivo or Twilio (plugin, installed separately).
- [WebChat](https://docs.openclaw.ai/web/webchat) — Gateway WebChat UI over WebSocket.
- [WeChat](https://docs.openclaw.ai/channels/wechat) — Tencent iLink Bot plugin via QR login; private chats only (external plugin).
- [WhatsApp](https://docs.openclaw.ai/channels/whatsapp) — Most popular; uses Baileys and requires QR pairing.
- [Yuanbao](https://docs.openclaw.ai/channels/yuanbao) — Tencent Yuanbao bot (external plugin).
- [Zalo](https://docs.openclaw.ai/channels/zalo) — Zalo Bot API; Vietnam’s popular messenger (bundled plugin).
- [Zalo Personal](https://docs.openclaw.ai/channels/zalouser) — Zalo personal account via QR login (bundled plugin).

## Notes

- Channels can run simultaneously; configure multiple and OpenClaw will route per chat.
- Fastest setup is usually **Telegram** (simple bot token). WhatsApp requires QR pa

_… [truncated; see https://docs.openclaw.ai/channels for full content]_


---

## Access groups - OpenClaw

_Source: <https://docs.openclaw.ai/channels/access-groups>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Access groups

Access groups are named sender lists you define once and reference from channel allowlists with `accessGroup:<name>`.Use them when the same people should be allowed across several message channels, or when one trusted set should apply to both DMs and group sender authorization.Access groups do not grant access by themselves. A group only matters when an allowlist field references it.

## Static message sender groups

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

## Reference groups from allowlists

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

## Supported message-channel paths

Access groups are available in shared message-channel authorization paths, including:

- DM sender allowlists such as `channels.<channel>.allowFrom`
- group sender allowlists such as `channels.<channel>.groupAllowFrom`
- channel-specific per-room sender allowlists that use the same sender matching rules
- command authorization paths that reuse message-channel sender allowlists

Channel support depends on whether that channel is wired through the shared OpenClaw sender-authorization helpers. Current bundled support includes Discord, Google Chat, Nostr, WhatsApp, Zalo, and Zalo Personal. Static `message.senders` groups are designed to be channel-agnostic, so new message channels should support them by using the shared plugin SDK helpers instead of custom allowlist expansion.

## Discord channel audiences

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

`discord.channelAudience` means “allow Discord DM senders who can currently view this guild channel.” Ope

_… [truncated; see https://docs.openclaw.ai/channels/access-groups for full content]_


---

## Broadcast groups - OpenClaw

_Source: <https://docs.openclaw.ai/channels/broadcast-groups>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Broadcast groups

**Status:** Experimental. Added in 2026.1.9.

## Overview

Broadcast Groups enable multiple agents to process and respond to the same message simultaneously. This allows you to create specialized agent teams that work together in a single WhatsApp group or DM — all using one phone number.Current scope: **WhatsApp only** (web channel).Broadcast groups are evaluated after channel allowlists and group activation rules. In WhatsApp groups, this means broadcasts happen when OpenClaw would normally reply (for example: on mention, depending on your group settings).

## Use cases

1\. Specialized agent teams

Deploy multiple agents with atomic, focused responsibilities:

```
Group: "Development Team"
Agents:
  - CodeReviewer (reviews code snippets)
  - DocumentationBot (generates docs)
  - SecurityAuditor (checks for vulnerabilities)
  - TestGenerator (suggests test cases)
```

Each agent processes the same message and provides its specialized perspective.

2\. Multi-language support

```
Group: "International Support"
Agents:
  - Agent_EN (responds in English)
  - Agent_DE (responds in German)
  - Agent_ES (responds in Spanish)
```

3\. Quality assurance workflows

```
Group: "Customer Support"
Agents:
  - SupportAgent (provides answer)
  - QAAgent (reviews quality, only responds if issues found)
```

4\. Task automation

```
Group: "Project Management"
Agents:
  - TaskTracker (updates task database)
  - TimeLogger (logs time spent)
  - ReportGenerator (creates summaries)
```

## Configuration

### Basic setup

Add a top-level `broadcast` section (next to `bindings`). Keys are WhatsApp peer ids:

- group chats: group JID (e.g. `120363403215116621@g.us`)
- DMs: E.164 phone number (e.g. `+15551234567`)

```
{
  "broadcast": {
    "120363403215116621@g.us": ["alfred", "baerbel", "assistant3"]
  }
}
```

**Result:** When OpenClaw would reply in this chat, it will run all three agents.

### Processing strategy

Control how agents process messages:

- parallel (default)

- sequential

All agents process simultaneously:

```
{
  "broadcast": {
    "strategy": "parallel",
    "120363403215116621@g.us": ["alfred", "baerbel"]
  }
}
```

Agents process in order (one waits for previous to finish):

```
{
  "broadcast": {
    "strategy": "sequential",
    "120363403215116621@g.us": ["alfred", "baerbel"]
  }
}
```

### Complete example

```
{
  "agents": {
    "list": [\
      {\
        "id": "code-reviewer",\
        "name": "Code Reviewer",\
        "workspace": "/path/to/code-reviewer",\
        "sandbox": { "mode": "all" }\
      },\
      {\
        "id": "security-auditor",\
        "name": "Security Auditor",\
        "workspace": "/path/to/security-auditor",\
        "sandbox": { "mode": "all" }\
      },\
      {\
        "id": "docs-generator",\
        "name": "Documentation Generator",\
        "workspace": "/path/to/docs-generator",\
        "sandbox": { "mode": "all" }\
      }\
    ]
  },
  "broadcast": {
    "strategy": "parallel",
    "120363403215116621@g.us": ["code-reviewer", "security-auditor", "docs-generator"],
    "120363424282127706@g.us": ["support-en", "support-de"],
    "+15555550123": ["assistant", "logger"]
  }
}
```

## How it works

### Message flow

1

[Navigate to header](https://docs.openclaw.ai/channels/broadcast-groups#)

Incoming message arrives

A WhatsApp group or DM message arrives.

2

[Navigate to header](https://docs.openclaw.ai/channels/broadcast-groups#)

Broadcast check

System checks if peer ID is in `broadcast`.

3

[Navigate to header](https://docs.openclaw.ai/channels/broadcast-groups#)

If in broadcast list

- All listed agents process the message.
- Each agent has its own session key and isolated context.
- Agents process in parallel (default) or sequentially.

4

[Navigate to header](https://docs.openclaw.ai/channels/broadcast-groups#)

If not in broadcast list

Normal routing applies (first matching binding).

B

_… [truncated; see https://docs.openclaw.ai/channels/broadcast-groups for full content]_


---

## Channel routing - OpenClaw

_Source: <https://docs.openclaw.ai/channels/channel-routing>_

# Channels & routing

OpenClaw routes replies **back to the channel where a message came from**. The
model does not choose a channel; routing is deterministic and controlled by the
host configuration.

## Key terms

- **Channel**: `telegram`, `whatsapp`, `discord`, `irc`, `googlechat`, `slack`, `signal`, `imessage`, `line`, plus plugin channels. `webchat` is the internal WebChat UI channel and is not a configurable outbound channel.
- **AccountId**: per‑channel account instance (when supported).
- Optional channel default account: `channels.<channel>.defaultAccount` chooses
which account is used when an outbound path does not specify `accountId`.

  - In multi-account setups, set an explicit default (`defaultAccount` or `accounts.default`) when two or more accounts are configured. Without it, fallback routing may pick the first normalized account ID.
- **AgentId**: an isolated workspace + session store (“brain”).
- **SessionKey**: the bucket key used to store context and control concurrency.

## Outbound target prefixes

Explicit outbound targets may include a provider prefix, such as `telegram:123` or `tg:123`. Core treats that prefix as a channel-selection hint only when the selected channel is `last` or otherwise unresolved, and only when the loaded plugin advertises that prefix. If the caller already selected an explicit channel, the provider prefix must match that channel; cross-channel combinations such as WhatsApp delivery to `telegram:123` fail before plugin-specific target normalization.Target-kind and service prefixes such as `channel:<id>`, `user:<id>`, `room:<id>`, `thread:<id>`, `imessage:<handle>`, and `sms:<number>` stay inside the selected channel’s grammar. They do not select the provider by themselves.

## Session key shapes (examples)

Direct messages collapse to the agent’s **main** session by default:

- `agent:<agentId>:<mainKey>` (default: `agent:main:main`)

Even when direct-message conversation history is shared with main, sandbox and
tool policy use a derived per-account direct-chat runtime key for external DMs
so channel-originated messages are not treated like local main-session runs.Groups and channels remain isolated per channel:

- Groups: `agent:<agentId>:<channel>:group:<id>`
- Channels/rooms: `agent:<agentId>:<channel>:channel:<id>`

Threads:

- Slack/Discord threads append `:thread:<threadId>` to the base key.
- Telegram forum topics embed `:topic:<topicId>` in the group key.

Examples:

- `agent:main:telegram:group:-1001234567890:topic:42`
- `agent:main:discord:channel:123456:thread:987654`

## Main DM route pinning

When `session.dmScope` is `main`, direct messages may share one main session.
To prevent the session’s `lastRoute` from being overwritten by non-owner DMs,
OpenClaw infers a pinned owner from `allowFrom` when all of these are true:

- `allowFrom` has exactly one non-wildcard entry.
- The entry can be normalized to a concrete sender ID for that channel.
- The inbound DM sender does not match that pinned owner.

In that mismatch case, OpenClaw still records inbound session metadata, but it
skips updating the main session `lastRoute`.

## Guarded inbound recording

Channel plugins can mark an inbound session record as `createIfMissing: false`
when a guarded path must not create a new OpenClaw session. In that mode,
OpenClaw may update metadata and `lastRoute` for an existing session, but it
does not create a route-only session entry just because a message was observed.

## Routing rules (how an agent is chosen)

Routing picks **one agent** for each inbound message:

1. **Exact peer match** (`bindings` with `peer.kind` \+ `peer.id`).
2. **Parent peer match** (thread inheritance).
3. **Guild + roles match** (Discord) via `guildId` \+ `roles`.
4. **Guild match** (Discord) via `guildId`.
5. **Team match** (Slack) via `teamId`.
6. **Account match** (`accountId` on the channel).
7. **Channel match** (any account on that channel, `accountId: "*"`).
8. **Default agent** (`agents.list[].def

_… [truncated; see https://docs.openclaw.ai/channels/channel-routing for full content]_


---

## Discord - OpenClaw

_Source: <https://docs.openclaw.ai/channels/discord>_

[OpenClaw home page](https://docs.openclaw.ai/)

Mainstream messaging

Discord

Ready for DMs and guild channels via the official Discord gateway.

[**Pairing** \\
\\
Discord DMs default to pairing mode.](https://docs.openclaw.ai/channels/pairing)

[**Slash commands** \\
\\
Native command behavior and command catalog.](https://docs.openclaw.ai/tools/slash-commands)

[**Channel troubleshooting** \\
\\
Cross-channel diagnostics and repair flow.](https://docs.openclaw.ai/channels/troubleshooting)

## Quick setup

You will need to create a new application with a bot, add the bot to your server, and pair it to OpenClaw. We recommend adding your bot to your own private server. If you don’t have one yet, [create one first](https://support.discord.com/hc/en-us/articles/204849977-How-do-I-create-a-server) (choose **Create My Own > For me and my friends**).

1

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Create a Discord application and bot

Go to the [Discord Developer Portal](https://discord.com/developers/applications) and click **New Application**. Name it something like “OpenClaw”.Click **Bot** on the sidebar. Set the **Username** to whatever you call your OpenClaw agent.

2

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Enable privileged intents

Still on the **Bot** page, scroll down to **Privileged Gateway Intents** and enable:

- **Message Content Intent** (required)
- **Server Members Intent** (recommended; required for role allowlists and name-to-ID matching)
- **Presence Intent** (optional; only needed for presence updates)

3

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Copy your bot token

Scroll back up on the **Bot** page and click **Reset Token**.

Despite the name, this generates your first token — nothing is being “reset.”

Copy the token and save it somewhere. This is your **Bot Token** and you will need it shortly.

4

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Generate an invite URL and add the bot to your server

Click **OAuth2** on the sidebar. You’ll generate an invite URL with the right permissions to add the bot to your server.Scroll down to **OAuth2 URL Generator** and enable:

- `bot`
- `applications.commands`

A **Bot Permissions** section will appear below. Enable at least:**General Permissions**

- View Channels
**Text Permissions**
- Send Messages
- Read Message History
- Embed Links
- Attach Files
- Add Reactions (optional)

This is the baseline set for normal text channels. If you plan to post in Discord threads, including forum or media channel workflows that create or continue a thread, also enable **Send Messages in Threads**.
Copy the generated URL at the bottom, paste it into your browser, select your server, and click **Continue** to connect. You should now see your bot in the Discord server.

5

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Enable Developer Mode and collect your IDs

Back in the Discord app, you need to enable Developer Mode so you can copy internal IDs.

1. Click **User Settings** (gear icon next to your avatar) → **Advanced** → toggle on **Developer Mode**
2. Right-click your **server icon** in the sidebar → **Copy Server ID**
3. Right-click your **own avatar** → **Copy User ID**

Save your **Server ID** and **User ID** alongside your Bot Token — you’ll send all three to OpenClaw in the next step.

6

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Allow DMs from server members

For pairing to work, Discord needs to allow your bot to DM you. Right-click your **server icon** → **Privacy Settings** → toggle on **Direct Messages**.This lets server members (including bots) send you DMs. Keep this enabled if you want to use Discord DMs with OpenClaw. If you only plan to use guild channels, you can disable DMs after pairing.

7

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Set your bot token securely (do not send it in chat)

Your Discord

_… [truncated; see https://docs.openclaw.ai/channels/discord for full content]_


---

## Recommended: Set up a guild workspace

_Source: <https://docs.openclaw.ai/channels/discord.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Discord

Ready for DMs and guild channels via the official Discord gateway.

 Discord DMs default to pairing mode.

 Native command behavior and command catalog.

 Cross-channel diagnostics and repair flow.

\## Quick setup

You will need to create a new application with a bot, add the bot to your server, and pair it to OpenClaw. We recommend adding your bot to your own private server. If you don't have one yet, \[create one first\](https://support.discord.com/hc/en-us/articles/204849977-How-do-I-create-a-server) (choose \*\*Create My Own > For me and my friends\*\*).

 Go to the \[Discord Developer Portal\](https://discord.com/developers/applications) and click \*\*New Application\*\*. Name it something like "OpenClaw".

 Click \*\*Bot\*\* on the sidebar. Set the \*\*Username\*\* to whatever you call your OpenClaw agent.

 Still on the \*\*Bot\*\* page, scroll down to \*\*Privileged Gateway Intents\*\* and enable:

 \\* \*\*Message Content Intent\*\* (required)
 \\* \*\*Server Members Intent\*\* (recommended; required for role allowlists and name-to-ID matching)
 \\* \*\*Presence Intent\*\* (optional; only needed for presence updates)

 Scroll back up on the \*\*Bot\*\* page and click \*\*Reset Token\*\*.

 Despite the name, this generates your first token — nothing is being "reset."

 Copy the token and save it somewhere. This is your \*\*Bot Token\*\* and you will need it shortly.

 Click \*\*OAuth2\*\* on the sidebar. You'll generate an invite URL with the right permissions to add the bot to your server.

 Scroll down to \*\*OAuth2 URL Generator\*\* and enable:

 \\* \`bot\`
 \\* \`applications.commands\`

 A \*\*Bot Permissions\*\* section will appear below. Enable at least:

 \*\*General Permissions\*\*

 \\* View Channels
 \*\*Text Permissions\*\*
 \\* Send Messages
 \\* Read Message History
 \\* Embed Links
 \\* Attach Files
 \\* Add Reactions (optional)

 This is the baseline set for normal text channels. If you plan to post in Discord threads, including forum or media channel workflows that create or continue a thread, also enable \*\*Send Messages in Threads\*\*.
 Copy the generated URL at the bottom, paste it into your browser, select your server, and click \*\*Continue\*\* to connect. You should now see your bot in the Discord server.

 Back in the Discord app, you need to enable Developer Mode so you can copy internal IDs.

 1\. Click \*\*User Settings\*\* (gear icon next to your avatar) → \*\*Advanced\*\* → toggle on \*\*Developer Mode\*\*
 2\. Right-click your \*\*server icon\*\* in the sidebar → \*\*Copy Server ID\*\*
 3\. Right-click your \*\*own avatar\*\* → \*\*Copy User ID\*\*

 Save your \*\*Server ID\*\* and \*\*User ID\*\* alongside your Bot Token — you'll send all three to OpenClaw in the next step.

 For pairing to work, Discord needs to allow your bot to DM you. Right-click your \*\*server icon\*\* → \*\*Privacy Settings\*\* → toggle on \*\*Direct Messages\*\*.

 This lets server members (including bots) send you DMs. Keep this enabled if you want to use Discord DMs with OpenClaw. If you only plan to use guild channels, you can disable DMs after pairing.

 Your Discord bot token is a secret (like a password). Set it on the machine running OpenClaw before messaging your agent.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 export DISCORD\_BOT\_TOKEN="YOUR\_BOT\_TOKEN"
 cat > discord.patch.json5 <<'JSON5'
 {
 channels: {
 discord: {
 enabled: true,
 token: { source: "env", provider: "default", id: "DISCORD\_BOT\_TOKEN" },
 },
 },
 }
 JSON5
 openclaw config patch --file ./discord.patch.json5 --dry-run
 openclaw config patch --file ./discord.patch.json5
 openclaw gateway
 \`\`\`

 If OpenClaw is already running as a background service, restart it via the OpenClaw Mac app or by stopping and restarting the \`openclaw gateway

_… [truncated; see https://docs.openclaw.ai/channels/discord.md for full content]_


---

## Feishu - OpenClaw

_Source: <https://docs.openclaw.ai/channels/feishu>_

# Feishu / Lark

Feishu/Lark is an all-in-one collaboration platform where teams chat, share documents, manage calendars, and get work done together.**Status:** production-ready for bot DMs + group chats. WebSocket is the default mode; webhook mode is optional.

* * *

## Quick start

Requires OpenClaw 2026.4.25 or above. Run `openclaw --version` to check. Upgrade with `openclaw update`.

1

[Navigate to header](https://docs.openclaw.ai/channels/feishu#)

Run the channel setup wizard

```
openclaw channels login --channel feishu
```

Scan the QR code with your Feishu/Lark mobile app to create a Feishu/Lark bot automatically.

2

[Navigate to header](https://docs.openclaw.ai/channels/feishu#)

After setup completes, restart the gateway to apply the changes

```
openclaw gateway restart
```

* * *

## Access control

### Direct messages

Configure `dmPolicy` to control who can DM the bot:

- `"pairing"` — unknown users receive a pairing code; approve via CLI
- `"allowlist"` — only users listed in `allowFrom` can chat (default: bot owner only)
- `"open"` — allow public DMs only when `allowFrom` includes `"*"`; with restrictive entries, only matching users can chat
- `"disabled"` — disable all DMs

**Approve a pairing request:**

```
openclaw pairing list feishu
openclaw pairing approve feishu <CODE>
```

### Group chats

**Group policy** (`channels.feishu.groupPolicy`):

| Value | Behavior |
| --- | --- |
| `"open"` | Respond to all messages in groups |
| `"allowlist"` | Only respond to groups in `groupAllowFrom` or explicitly configured under `groups.<chat_id>` |
| `"disabled"` | Disable all group messages; explicit `groups.<chat_id>` entries do not override this |

Default: `allowlist`**Mention requirement** (`channels.feishu.requireMention`):

- `true` — require @mention (default)
- `false` — respond without @mention
- Per-group override: `channels.feishu.groups.<chat_id>.requireMention`
- Broadcast-only `@all` and `@_all` are not treated as bot mentions. A message that mentions both `@all` and the bot directly still counts as a bot mention.

* * *

## Group configuration examples

### Allow all groups, no @mention required

```
{
  channels: {
    feishu: {
      groupPolicy: "open",
    },
  },
}
```

### Allow all groups, still require @mention

```
{
  channels: {
    feishu: {
      groupPolicy: "open",
      requireMention: true,
    },
  },
}
```

### Allow specific groups only

```
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      // Group IDs look like: oc_xxx
      groupAllowFrom: ["oc_xxx", "oc_yyy"],
    },
  },
}
```

In `allowlist` mode, you can also admit a group by adding an explicit `groups.<chat_id>` entry. Explicit entries do not override `groupPolicy: "disabled"`. Wildcard defaults under `groups.*` configure matching groups, but they do not admit groups by themselves.

```
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      groups: {
        oc_xxx: {
          requireMention: false,
        },
      },
    },
  },
}
```

### Restrict senders within a group

```
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["oc_xxx"],
      groups: {
        oc_xxx: {
          // User open_ids look like: ou_xxx
          allowFrom: ["ou_user1", "ou_user2"],
        },
      },
    },
  },
}
```

* * *

## Get group/user IDs

### Group IDs (`chat_id`, format: `oc_xxx`)

Open the group in Feishu/Lark, click the menu icon in the top-right corner, and go to **Settings**. The group ID (`chat_id`) is listed on the settings page.

### User IDs (`open_id`, format: `ou_xxx`)

Start the gateway, send a DM to the bot, then check the logs:

```
openclaw logs --follow
```

Look for `open_id` in the log output. You can also check pending pairing requests:

```
openclaw pairing list feishu
```

* * *

## Common commands

| Command | Description |
| --- | --- |
| `/status` | Show bot status |
| `/reset` | Reset the current session |
| `/model` | Show or switch the AI

_… [truncated; see https://docs.openclaw.ai/channels/feishu for full content]_


---

## Google Chat - OpenClaw

_Source: <https://docs.openclaw.ai/channels/googlechat>_

# If bound to localhost (127.0.0.1 or 0.0.0.0):
tailscale serve --bg --https 8443 http://127.0.0.1:18789

# If bound to Tailscale IP only (e.g., 100.106.161.80):
tailscale serve --bg --https 8443 http://100.106.161.80:18789
```

3. **Expose only the webhook path publicly:**

```
# If bound to localhost (127.0.0.1 or 0.0.0.0):
tailscale funnel --bg --set-path /googlechat http://127.0.0.1:18789/googlechat

# If bound to Tailscale IP only (e.g., 100.106.161.80):
tailscale funnel --bg --set-path /googlechat http://100.106.161.80:18789/googlechat
```

4. **Authorize the node for Funnel access:**
If prompted, visit the authorization URL shown in the output to enable Funnel for this node in your tailnet policy.
5. **Verify the configuration:**

```
tailscale serve status
tailscale funnel status
```

Your public webhook URL will be:
`https://<node-name>.<tailnet>.ts.net/googlechat`Your private dashboard stays tailnet-only:
`https://<node-name>.<tailnet>.ts.net:8443/`Use the public URL (without `:8443`) in the Google Chat app config.

> Note: This configuration persists across reboots. To remove it later, run `tailscale funnel reset` and `tailscale serve reset`.

### Option B: Reverse Proxy (Caddy)

If you use a reverse proxy like Caddy, only proxy the specific path:

```
your-domain.com {
    reverse_proxy /googlechat* localhost:18789
}
```

With this config, any request to `your-domain.com/` will be ignored or returned as 404, while `your-domain.com/googlechat` is safely routed to OpenClaw.

### Option C: Cloudflare Tunnel

Configure your tunnel’s ingress rules to only route the webhook path:

- **Path**: `/googlechat` -\> `http://localhost:18789/googlechat`
- **Default Rule**: HTTP 404 (Not Found)

## How it works

1. Google Chat sends webhook POSTs to the gateway. Each request includes an `Authorization: Bearer <token>` header.

   - OpenClaw verifies bearer auth before reading/parsing full webhook bodies when the header is present.
   - Google Workspace Add-on requests that carry `authorizationEventObject.systemIdToken` in the body are supported via a stricter pre-auth body budget.
2. OpenClaw verifies the token against the configured `audienceType` \+ `audience`:

   - `audienceType: "app-url"` → audience is your HTTPS webhook URL.
   - `audienceType: "project-number"` → audience is the Cloud project number.
3. Messages are routed by space:
   - DMs use session key `agent:<agentId>:googlechat:direct:<spaceId>`.
   - Spaces use session key `agent:<agentId>:googlechat:group:<spaceId>`.
4. DM access is pairing by default. Unknown senders receive a pairing code; approve with:
   - `openclaw pairing approve googlechat <code>`
5. Group spaces require @-mention by default. Use `botUser` if mention detection needs the app’s user name.

## Targets

Use these identifiers for delivery and allowlists:

- Direct messages: `users/<userId>` (recommended).
- Raw email `name@example.com` is mutable and only used for direct allowlist matching when `channels.googlechat.dangerouslyAllowNameMatching: true`.
- Deprecated: `users/<email>` is treated as a user id, not an email allowlist.
- Spaces: `spaces/<spaceId>`.

## Config highlights

```
{
  channels: {
    googlechat: {
      enabled: true,
      serviceAccountFile: "/path/to/service-account.json",
      // or serviceAccountRef: { source: "file", provider: "filemain", id: "/channels/googlechat/serviceAccount" }
      audienceType: "app-url",
      audience: "https://gateway.example.com/googlechat",
      webhookPath: "/googlechat",
      botUser: "users/1234567890", // optional; helps mention detection
      dm: {
        policy: "pairing",
        allowFrom: ["users/1234567890"],
      },
      groupPolicy: "allowlist",
      groups: {
        "spaces/AAAA": {
          allow: true,
          requireMention: true,
          users: ["users/1234567890"],
          systemPrompt: "Short answers only.",
        },
      },
      actions: { reactions: true },
      typingIndicator: "message",
      me

_… [truncated; see https://docs.openclaw.ai/channels/googlechat for full content]_


---

## Group messages - OpenClaw

_Source: <https://docs.openclaw.ai/channels/group-messages>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Group messages

Goal: let Clawd sit in WhatsApp groups, wake up only when pinged, and keep that thread separate from the personal DM session.

`agents.list[].groupChat.mentionPatterns` is also used by Telegram, Discord, Slack, and iMessage. This doc focuses on WhatsApp-specific behavior. For multi-agent setups, set `agents.list[].groupChat.mentionPatterns` per agent, or use `messages.groupChat.mentionPatterns` as a global fallback.

## Current implementation (2025-12-03)

- Activation modes: `mention` (default) or `always`. `mention` requires a ping (real WhatsApp @-mentions via `mentionedJids`, safe regex patterns, or the bot’s E.164 anywhere in the text). `always` wakes the agent on every message but it should reply only when it can add meaningful value; otherwise it returns the exact silent token `NO_REPLY` / `no_reply`. Defaults can be set in config (`channels.whatsapp.groups`) and overridden per group via `/activation`. When `channels.whatsapp.groups` is set, it also acts as a group allowlist (include `"*"` to allow all).
- Group policy: `channels.whatsapp.groupPolicy` controls whether group messages are accepted (`open|disabled|allowlist`). `allowlist` uses `channels.whatsapp.groupAllowFrom` (fallback: explicit `channels.whatsapp.allowFrom`). Default is `allowlist` (blocked until you add senders).
- Per-group sessions: session keys look like `agent:<agentId>:whatsapp:group:<jid>` so commands such as `/verbose on`, `/trace on`, or `/think high` (sent as standalone messages) are scoped to that group; personal DM state is untouched. Heartbeats are skipped for group threads.
- Context injection: **pending-only** group messages (default 50) that _did not_ trigger a run are prefixed under `[Chat messages since your last reply - for context]`, with the triggering line under `[Current message - respond to this]`. Messages already in the session are not re-injected.
- Sender surfacing: every group batch now ends with `[from: Sender Name (+E164)]` so Pi knows who is speaking.
- Ephemeral/view-once: we unwrap those before extracting text/mentions, so pings inside them still trigger.
- Group system prompt: on the first turn of a group session (and whenever `/activation` changes the mode) we inject a short blurb into the system prompt like `You are replying inside the WhatsApp group "<subject>". Group members: Alice (+44...), Bob (+43...), … Activation: trigger-only … Address the specific sender noted in the message context.` If metadata isn’t available we still tell the agent it’s a group chat.

## Config example (WhatsApp)

Add a `groupChat` block to `~/.openclaw/openclaw.json` so display-name pings work even when WhatsApp strips the visual `@` in the text body:

```
{
  channels: {
    whatsapp: {
      groups: {
        "*": { requireMention: true },
      },
    },
  },
  agents: {
    list: [\
      {\
        id: "main",\
        groupChat: {\
          historyLimit: 50,\
          mentionPatterns: ["@?openclaw", "\\+?15555550123"],\
        },\
      },\
    ],
  },
}
```

Notes:

- The regexes are case-insensitive and use the same safe-regex guardrails as other config regex surfaces; invalid patterns and unsafe nested repetition are ignored.
- WhatsApp still sends canonical mentions via `mentionedJids` when someone taps the contact, so the number fallback is rarely needed but is a useful safety net.

### Activation command (owner-only)

Use the group chat command:

- `/activation mention`
- `/activation always`

Only the owner number (from `channels.whatsapp.allowFrom`, or the bot’s own E.164 when unset) can change this. Send `/status` as a standalone message in the group to see the current activation mode.

## How to use

1. Add your WhatsApp account (the one running OpenClaw) to the group.
2. Say `@openclaw …` (or include the number). Only allowlisted senders can trigger it unless you set `groupPolicy: "open"`.
3. The agent prompt will include recent group con

_… [truncated; see https://docs.openclaw.ai/channels/group-messages for full content]_


---

## Groups - OpenClaw

_Source: <https://docs.openclaw.ai/channels/groups>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Groups

OpenClaw treats group chats consistently across surfaces: Discord, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo.

## Beginner intro (2 minutes)

OpenClaw “lives” on your own messaging accounts. There is no separate WhatsApp bot user. If **you** are in a group, OpenClaw can see that group and respond there.Default behavior:

- Groups are restricted (`groupPolicy: "allowlist"`).
- Replies require a mention unless you explicitly disable mention gating.
- Normal final replies in groups/channels are private by default. Visible room output uses the `message` tool.

Translation: allowlisted senders can trigger OpenClaw by mentioning it.

**TL;DR**

- **DM access** is controlled by `*.allowFrom`.
- **Group access** is controlled by `*.groupPolicy` \+ allowlists (`*.groups`, `*.groupAllowFrom`).
- **Reply triggering** is controlled by mention gating (`requireMention`, `/activation`).

Quick flow (what happens to a group message):

```
groupPolicy? disabled -> drop
groupPolicy? allowlist -> group allowed? no -> drop
requireMention? yes -> mentioned? no -> store for context only
otherwise -> reply
```

## Visible replies

For group/channel rooms, OpenClaw defaults to `messages.groupChat.visibleReplies: "message_tool"`.
`openclaw doctor --fix` writes this default into configured-channel configs that omit it.
That means the agent still processes the turn and can update memory/session state, but its normal final answer is not automatically posted back into the room. To speak visibly, the agent uses `message(action=send)`.If the message tool is unavailable under the active tool policy, OpenClaw falls
back to automatic visible replies instead of silently suppressing the response.
`openclaw doctor` warns about this mismatch.For direct chats and any other source turn, use `messages.visibleReplies: "message_tool"` to apply the same tool-only visible-reply behavior globally. Harnesses can also choose this as their unset default; the Codex harness does this for Codex-mode direct chats. `messages.groupChat.visibleReplies` remains the more specific override for group/channel rooms.This replaces the old pattern of forcing the model to answer `NO_REPLY` for most lurk-mode turns. In tool-only mode, doing nothing visible simply means not calling the message tool.Typing indicators are still sent while the agent works in tool-only mode. The default group typing mode is upgraded from “message” to “instant” for these turns because there may never be normal assistant message text before the agent decides whether to call the message tool. Explicit typing-mode config still wins.To restore legacy automatic final replies for group/channel rooms:

```
{
  messages: {
    groupChat: {
      visibleReplies: "automatic",
    },
  },
}
```

The gateway hot-reloads `messages` config after the file is saved. Restart only
when file watching or config reload is disabled in the deployment.To require visible output to go through the message tool for every source chat:

```
{
  messages: {
    visibleReplies: "message_tool",
  },
}
```

Native slash commands (Discord, Telegram, and other surfaces with native command support) bypass `visibleReplies: "message_tool"` and always reply visibly so the channel-native command UI gets the response it expects. This applies to validated native command turns only; text-typed `/...` commands and ordinary chat turns still follow the configured group default.

## Context visibility and allowlists

Two different controls are involved in group safety:

- **Trigger authorization**: who can trigger the agent (`groupPolicy`, `groups`, `groupAllowFrom`, channel-specific allowlists).
- **Context visibility**: what supplemental context is injected into the model (reply text, quotes, thread history, forwarded metadata).

By default, OpenClaw prioritizes normal chat behavior and keeps context mostly as received. This means allowlists primarily deci

_… [truncated; see https://docs.openclaw.ai/channels/groups for full content]_


---

## iMessage - OpenClaw

_Source: <https://docs.openclaw.ai/channels/imessage>_

# or
imsg send <handle> "test"
```

## Access control and routing

- DM policy

- Group policy + mentions

- Sessions and deterministic replies

`channels.imessage.dmPolicy` controls direct messages:

- `pairing` (default)
- `allowlist`
- `open` (requires `allowFrom` to include `"*"`)
- `disabled`

Allowlist field: `channels.imessage.allowFrom`.Allowlist entries can be handles or chat targets (`chat_id:*`, `chat_guid:*`, `chat_identifier:*`).

`channels.imessage.groupPolicy` controls group handling:

- `allowlist` (default when configured)
- `open`
- `disabled`

Group sender allowlist: `channels.imessage.groupAllowFrom`.Runtime fallback: if `groupAllowFrom` is unset, iMessage group sender checks fall back to `allowFrom` when available.
Runtime note: if `channels.imessage` is completely missing, runtime falls back to `groupPolicy="allowlist"` and logs a warning (even if `channels.defaults.groupPolicy` is set).Mention gating for groups:

- iMessage has no native mention metadata
- mention detection uses regex patterns (`agents.list[].groupChat.mentionPatterns`, fallback `messages.groupChat.mentionPatterns`)
- with no configured patterns, mention gating cannot be enforced

Control commands from authorized senders can bypass mention gating in groups.

- DMs use direct routing; groups use group routing.
- With default `session.dmScope=main`, iMessage DMs collapse into the agent main session.
- Group sessions are isolated (`agent:<agentId>:imessage:group:<chat_id>`).
- Replies route back to iMessage using originating channel/target metadata.

Group-ish thread behavior:Some multi-participant iMessage threads can arrive with `is_group=false`.
If that `chat_id` is explicitly configured under `channels.imessage.groups`, OpenClaw treats it as group traffic (group gating + group session isolation).

## ACP conversation bindings

Legacy iMessage chats can also be bound to ACP sessions.Fast operator flow:

- Run `/acp spawn codex --bind here` inside the DM or allowed group chat.
- Future messages in that same iMessage conversation route to the spawned ACP session.
- `/new` and `/reset` reset the same bound ACP session in place.
- `/acp close` closes the ACP session and removes the binding.

Configured persistent bindings are supported through top-level `bindings[]` entries with `type: "acp"` and `match.channel: "imessage"`.`match.peer.id` can use:

- normalized DM handle such as `+15555550123` or `user@example.com`
- `chat_id:<id>` (recommended for stable group bindings)
- `chat_guid:<guid>`
- `chat_identifier:<identifier>`

Example:

```
{
  agents: {
    list: [\
      {\
        id: "codex",\
        runtime: {\
          type: "acp",\
          acp: { agent: "codex", backend: "acpx", mode: "persistent" },\
        },\
      },\
    ],
  },
  bindings: [\
    {\
      type: "acp",\
      agentId: "codex",\
      match: {\
        channel: "imessage",\
        accountId: "default",\
        peer: { kind: "group", id: "chat_id:123" },\
      },\
      acp: { label: "codex-group" },\
    },\
  ],
}
```

See [ACP Agents](https://docs.openclaw.ai/tools/acp-agents) for shared ACP binding behavior.

## Deployment patterns

Dedicated bot macOS user (separate iMessage identity)

Use a dedicated Apple ID and macOS user so bot traffic is isolated from your personal Messages profile.Typical flow:

1. Create/sign in a dedicated macOS user.
2. Sign into Messages with the bot Apple ID in that user.
3. Install `imsg` in that user.
4. Create SSH wrapper so OpenClaw can run `imsg` in that user context.
5. Point `channels.imessage.accounts.<id>.cliPath` and `.dbPath` to that user profile.

First run may require GUI approvals (Automation + Full Disk Access) in that bot user session.

Remote Mac over Tailscale (example)

Common topology:

- gateway runs on Linux/VM
- iMessage + `imsg` runs on a Mac in your tailnet
- `cliPath` wrapper uses SSH to run `imsg`
- `remoteHost` enables SCP attachment fetches

Example:

```
{
  channels: {
    imessage: {
      enab

_… [truncated; see https://docs.openclaw.ai/channels/imessage for full content]_


---

## Nextcloud Talk - OpenClaw

_Source: <https://docs.openclaw.ai/channels/nextcloud-talk>_

[OpenClaw home page](https://docs.openclaw.ai/)

Developer and self-hosted

Nextcloud Talk

Status: bundled plugin (webhook bot). Direct messages, rooms, reactions, and markdown messages are supported.

## Bundled plugin

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

## Quick setup (beginner)

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

## Notes

- Bots cannot initiate DMs. The user must message the bot first.
- Webhook URL must be reachable by the Gateway; set `webhookPublicUrl` if behind a proxy.
- Media uploads are not supported by the bot API; media is sent as URLs.
- The webhook payload does not distinguish DMs vs rooms; set `apiUser` \+ `apiPassword` to enable room-type lookups (otherwise DMs are treated as rooms).

## Access control (DMs)

- Default: `channels.nextcloud-talk.dmPolicy = "pairing"`. Unknown senders get a pairing code.
- Approve via:
  - `openclaw pairing list nextcloud-talk`
  - `openclaw pairing approve nextcloud-talk <CODE>`
- Public DMs: `channels.nextcloud-talk.dmPolicy="open"` plus `channels.nextcloud-talk.allowFrom=["*"]`.
- `allowFrom` matches Nextcloud user IDs only; display names are ignored.

## Rooms (groups)

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

## Capabilities

| Feature | Status |
| --- | --- |
| Direct messages | Supported |
| Rooms | Supported |
| Threads | Not supported |
| Media | URL-only |
| Reactions | Supported |
| Native commands | Not supported |

## Configuration reference (Nextcloud Talk)

Full configuration: [Configuration](https://docs.openclaw.ai/gateway/configuration)Provider options:

- `channels.nextcloud-talk.enabled`: enable/disable channel startup.
- `channels.nextcloud-talk.baseUrl`: Nextcloud instance URL.
- `channels.nextcloud-talk.botSecret`: bot shared secret.
- `channels.nextcloud-talk.bo

_… [truncated; see https://docs.openclaw.ai/channels/nextcloud-talk for full content]_


---

## Pairing - OpenClaw

_Source: <https://docs.openclaw.ai/channels/pairing>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Pairing

“Pairing” is OpenClaw’s explicit access approval step.
It is used in two places:

1. **DM pairing** (who is allowed to talk to the bot)
2. **Node pairing** (which devices/nodes are allowed to join the gateway network)

Security context: [Security](https://docs.openclaw.ai/gateway/security)

## 1) DM pairing (inbound chat access)

When a channel is configured with DM policy `pairing`, unknown senders get a short code and their message is **not processed** until you approve.Default DM policies are documented in: [Security](https://docs.openclaw.ai/gateway/security)`dmPolicy: "open"` is public only when the effective DM allowlist includes `"*"`.
Setup and validation require that wildcard for public-open configs. If existing
state contains `open` with concrete `allowFrom` entries, runtime still admits
only those senders, and pairing-store approvals do not widen `open` access.Pairing codes:

- 8 characters, uppercase, no ambiguous chars (`0O1I`).
- **Expire after 1 hour**. The bot only sends the pairing message when a new request is created (roughly once per hour per sender).
- Pending DM pairing requests are capped at **3 per channel** by default; additional requests are ignored until one expires or is approved.

### Approve a sender

```
openclaw pairing list telegram
openclaw pairing approve telegram <CODE>
```

If no command owner is configured yet, approving a DM pairing code also bootstraps
`commands.ownerAllowFrom` to the approved sender, such as `telegram:123456789`.
That gives first-time setups an explicit owner for privileged commands and exec
approval prompts. After an owner exists, later pairing approvals only grant DM
access; they do not add more owners.Supported channels: `bluebubbles`, `discord`, `feishu`, `googlechat`, `imessage`, `irc`, `line`, `matrix`, `mattermost`, `msteams`, `nextcloud-talk`, `nostr`, `openclaw-weixin`, `signal`, `slack`, `synology-chat`, `telegram`, `twitch`, `whatsapp`, `zalo`, `zalouser`.

### Reusable sender groups

Use top-level `accessGroups` when the same trusted sender set should apply to
multiple message channels or to both DM and group allowlists.Static groups use `type: "message.senders"` and are referenced with
`accessGroup:<name>` from channel allowlists:

```
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        discord: ["discord:123456789012345678"],
        telegram: ["987654321"],
        whatsapp: ["+15551234567"],
      },
    },
  },
  channels: {
    telegram: { dmPolicy: "allowlist", allowFrom: ["accessGroup:operators"] },
    whatsapp: { groupPolicy: "allowlist", groupAllowFrom: ["accessGroup:operators"] },
  },
}
```

Access groups are documented in detail here: [Access groups](https://docs.openclaw.ai/channels/access-groups)

### Where the state lives

Stored under `~/.openclaw/credentials/`:

- Pending requests: `<channel>-pairing.json`
- Approved allowlist store:
  - Default account: `<channel>-allowFrom.json`
  - Non-default account: `<channel>-<accountId>-allowFrom.json`

Account scoping behavior:

- Non-default accounts read/write only their scoped allowlist file.
- Default account uses the channel-scoped unscoped allowlist file.

Treat these as sensitive (they gate access to your assistant).

The pairing allowlist store is for DM access. Group authorization is separate.
Approving a DM pairing code does not automatically allow that sender to run group
commands or control the bot in groups. First-owner bootstrap is separate config
state in `commands.ownerAllowFrom`, and group chat delivery still follows the
channel’s group allowlists (for example `groupAllowFrom`, `groups`, or per-group
or per-topic overrides depending on the channel).

## 2) Node device pairing (iOS/Android/macOS/headless nodes)

Nodes connect to the Gateway as **devices** with `role: node`. The Gateway
creates a device pairing request that must be approved.

### Pair via Telegram (recom

_… [truncated; see https://docs.openclaw.ai/channels/pairing for full content]_


---

## Reusable sender groups

_Source: <https://docs.openclaw.ai/channels/pairing.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Pairing

“Pairing” is OpenClaw’s explicit access approval step.
It is used in two places:

1\. \*\*DM pairing\*\* (who is allowed to talk to the bot)
2\. \*\*Node pairing\*\* (which devices/nodes are allowed to join the gateway network)

Security context: \[Security\](/gateway/security)

\## 1) DM pairing (inbound chat access)

When a channel is configured with DM policy \`pairing\`, unknown senders get a short code and their message is \*\*not processed\*\* until you approve.

Default DM policies are documented in: \[Security\](/gateway/security)

\`dmPolicy: "open"\` is public only when the effective DM allowlist includes \`"\*"\`.
Setup and validation require that wildcard for public-open configs. If existing
state contains \`open\` with concrete \`allowFrom\` entries, runtime still admits
only those senders, and pairing-store approvals do not widen \`open\` access.

Pairing codes:

\\* 8 characters, uppercase, no ambiguous chars (\`0O1I\`).
\\* \*\*Expire after 1 hour\*\*. The bot only sends the pairing message when a new request is created (roughly once per hour per sender).
\\* Pending DM pairing requests are capped at \*\*3 per channel\*\* by default; additional requests are ignored until one expires or is approved.

\### Approve a sender

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw pairing list telegram
openclaw pairing approve telegram ```
```

If no command owner is configured yet, approving a DM pairing code also bootstraps
`commands.ownerAllowFrom` to the approved sender, such as `telegram:123456789`.
That gives first-time setups an explicit owner for privileged commands and exec
approval prompts. After an owner exists, later pairing approvals only grant DM
access; they do not add more owners.

Supported channels: `bluebubbles`, `discord`, `feishu`, `googlechat`, `imessage`, `irc`, `line`, `matrix`, `mattermost`, `msteams`, `nextcloud-talk`, `nostr`, `openclaw-weixin`, `signal`, `slack`, `synology-chat`, `telegram`, `twitch`, `whatsapp`, `zalo`, `zalouser`.

### Reusable sender groups

Use top-level `accessGroups` when the same trusted sender set should apply to
multiple message channels or to both DM and group allowlists.

Static groups use `type: "message.senders"` and are referenced with
`accessGroup:` from channel allowlists:

```json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        discord: ["discord:123456789012345678"],
        telegram: ["987654321"],
        whatsapp: ["+15551234567"],
      },
    },
  },
  channels: {
    telegram: { dmPolicy: "allowlist", allowFrom: ["accessGroup:operators"] },
    whatsapp: { groupPolicy: "allowlist", groupAllowFrom: ["accessGroup:operators"] },
  },
}
```

Access groups are documented in detail here: [Access groups](/channels/access-groups)

### Where the state lives

Stored under `~/.openclaw/credentials/`:

* Pending requests: `-pairing.json`
* Approved allowlist store:
  * Default account: `-allowFrom.json`
  * Non-default account: `--allowFrom.json`

Account scoping behavior:

* Non-default accounts read/write only their scoped allowlist file.
* Default account uses the channel-scoped unscoped allowlist file.

Treat these as sensitive (they gate access to your assistant).

  The pairing allowlist store is for DM access. Group authorization is separate.
  Approving a DM pairing code does not automatically allow that sender to run group
  commands or control the bot in groups. First-owner bootstrap is separate config
  state in `commands.ownerAllowFrom`, and group chat delivery still follows the
  channel's group allowlists (for example `groupAllowFrom`, `groups`, or per-group
  or per-topic overrides depending on the channel).

## 2) Node device pairing (iOS/Android/macOS/head

_… [truncated; see https://docs.openclaw.ai/channels/pairing.md for full content]_


---

## Slack - OpenClaw

_Source: <https://docs.openclaw.ai/channels/slack>_

[OpenClaw home page](https://docs.openclaw.ai/)

Mainstream messaging

Slack

Production-ready for DMs and channels via Slack app integrations. Default mode is Socket Mode; HTTP Request URLs are also supported.

[**Pairing** \\
\\
Slack DMs default to pairing mode.](https://docs.openclaw.ai/channels/pairing)

[**Slash commands** \\
\\
Native command behavior and command catalog.](https://docs.openclaw.ai/tools/slash-commands)

[**Channel troubleshooting** \\
\\
Cross-channel diagnostics and repair playbooks.](https://docs.openclaw.ai/channels/troubleshooting)

## Quick setup

- Socket Mode (default)

- HTTP Request URLs

1

[Navigate to header](https://docs.openclaw.ai/channels/slack#)

Create a new Slack app

In Slack app settings press the **[Create New App](https://api.slack.com/apps/new)** button:

- choose **from a manifest** and select a workspace for your app
- paste the [example manifest](https://docs.openclaw.ai/channels/slack#manifest-and-scope-checklist) from below and continue to create
- generate an **App-Level Token** (`xapp-...`) with `connections:write`
- install app and copy the **Bot Token** (`xoxb-...`) shown

2

[Navigate to header](https://docs.openclaw.ai/channels/slack#)

Configure OpenClaw

Recommended SecretRef setup:

```
export SLACK_APP_TOKEN=xapp-...
export SLACK_BOT_TOKEN=xoxb-...
cat > slack.socket.patch.json5 <<'JSON5'
{
  channels: {
    slack: {
      enabled: true,
      mode: "socket",
      appToken: { source: "env", provider: "default", id: "SLACK_APP_TOKEN" },
      botToken: { source: "env", provider: "default", id: "SLACK_BOT_TOKEN" },
    },
  },
}
JSON5
openclaw config patch --file ./slack.socket.patch.json5 --dry-run
openclaw config patch --file ./slack.socket.patch.json5
```

Env fallback (default account only):

```
SLACK_APP_TOKEN=xapp-...
SLACK_BOT_TOKEN=xoxb-...
```

3

[Navigate to header](https://docs.openclaw.ai/channels/slack#)

Start gateway

```
openclaw gateway
```

1

[Navigate to header](https://docs.openclaw.ai/channels/slack#)

Create a new Slack app

In Slack app settings press the **[Create New App](https://api.slack.com/apps/new)** button:

- choose **from a manifest** and select a workspace for your app
- paste the [example manifest](https://docs.openclaw.ai/channels/slack#manifest-and-scope-checklist) and update the URLs before create
- save the **Signing Secret** for request verification
- install app and copy the **Bot Token** (`xoxb-...`) shown

2

[Navigate to header](https://docs.openclaw.ai/channels/slack#)

Configure OpenClaw

Recommended SecretRef setup:

```
export SLACK_BOT_TOKEN=xoxb-...
export SLACK_SIGNING_SECRET=...
cat > slack.http.patch.json5 <<'JSON5'
{
  channels: {
    slack: {
      enabled: true,
      mode: "http",
      botToken: { source: "env", provider: "default", id: "SLACK_BOT_TOKEN" },
      signingSecret: { source: "env", provider: "default", id: "SLACK_SIGNING_SECRET" },
      webhookPath: "/slack/events",
    },
  },
}
JSON5
openclaw config patch --file ./slack.http.patch.json5 --dry-run
openclaw config patch --file ./slack.http.patch.json5
```

Use unique webhook paths for multi-account HTTPGive each account a distinct `webhookPath` (default `/slack/events`) so registrations do not collide.

3

[Navigate to header](https://docs.openclaw.ai/channels/slack#)

Start gateway

```
openclaw gateway
```

## Socket Mode transport tuning

OpenClaw sets the Slack SDK client pong timeout to 15 seconds by default for Socket Mode. Override the transport settings only when you need workspace- or host-specific tuning:

```
{
  channels: {
    slack: {
      mode: "socket",
      socketMode: {
        clientPingTimeout: 20000,
        serverPingTimeout: 30000,
        pingPongLoggingEnabled: false,
      },
    },
  },
}
```

Use this only for Socket Mode workspaces that log Slack websocket pong/server-ping timeouts or run on hosts with known event-loop starvation. `clientPingTimeout` is the pong wait after the SDK sends a client ping; `serverPin

_… [truncated; see https://docs.openclaw.ai/channels/slack for full content]_


---

## Threading, sessions, and reply tags

_Source: <https://docs.openclaw.ai/channels/slack.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Slack

Production-ready for DMs and channels via Slack app integrations. Default mode is Socket Mode; HTTP Request URLs are also supported.

 Slack DMs default to pairing mode.

 Native command behavior and command catalog.

 Cross-channel diagnostics and repair playbooks.

\## Quick setup

 In Slack app settings press the \*\*\[Create New App\](https://api.slack.com/apps/new)\*\* button:

 \\* choose \*\*from a manifest\*\* and select a workspace for your app
 \\* paste the \[example manifest\](#manifest-and-scope-checklist) from below and continue to create
 \\* generate an \*\*App-Level Token\*\* (\`xapp-...\`) with \`connections:write\`
 \\* install app and copy the \*\*Bot Token\*\* (\`xoxb-...\`) shown

 Recommended SecretRef setup:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 export SLACK\_APP\_TOKEN=xapp-...
 export SLACK\_BOT\_TOKEN=xoxb-...
 cat > slack.socket.patch.json5 <<'JSON5'
 {
 channels: {
 slack: {
 enabled: true,
 mode: "socket",
 appToken: { source: "env", provider: "default", id: "SLACK\_APP\_TOKEN" },
 botToken: { source: "env", provider: "default", id: "SLACK\_BOT\_TOKEN" },
 },
 },
 }
 JSON5
 openclaw config patch --file ./slack.socket.patch.json5 --dry-run
 openclaw config patch --file ./slack.socket.patch.json5
 \`\`\`

 Env fallback (default account only):

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 SLACK\_APP\_TOKEN=xapp-...
 SLACK\_BOT\_TOKEN=xoxb-...
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway
 \`\`\`

 In Slack app settings press the \*\*\[Create New App\](https://api.slack.com/apps/new)\*\* button:

 \\* choose \*\*from a manifest\*\* and select a workspace for your app
 \\* paste the \[example manifest\](#manifest-and-scope-checklist) and update the URLs before create
 \\* save the \*\*Signing Secret\*\* for request verification
 \\* install app and copy the \*\*Bot Token\*\* (\`xoxb-...\`) shown

 Recommended SecretRef setup:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 export SLACK\_BOT\_TOKEN=xoxb-...
 export SLACK\_SIGNING\_SECRET=...
 cat > slack.http.patch.json5 <<'JSON5'
 {
 channels: {
 slack: {
 enabled: true,
 mode: "http",
 botToken: { source: "env", provider: "default", id: "SLACK\_BOT\_TOKEN" },
 signingSecret: { source: "env", provider: "default", id: "SLACK\_SIGNING\_SECRET" },
 webhookPath: "/slack/events",
 },
 },
 }
 JSON5
 openclaw config patch --file ./slack.http.patch.json5 --dry-run
 openclaw config patch --file ./slack.http.patch.json5
 \`\`\`

 Use unique webhook paths for multi-account HTTP

 Give each account a distinct \`webhookPath\` (default \`/slack/events\`) so registrations do not collide.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway
 \`\`\`

\## Socket Mode transport tuning

OpenClaw sets the Slack SDK client pong timeout to 15 seconds by default for Socket Mode. Override the transport settings only when you need workspace- or host-specific tuning:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 channels: {
 slack: {
 mode: "socket",
 socketMode: {
 clientPingTimeout: 20000,
 serverPingTimeout: 30000,
 pingPongLoggingEnabled: false,
 },
 },
 },
}
\`\`\`

Use this only for Socket Mode workspaces that log Slack websocket pong/server-ping timeouts or run on hosts with known event-loop starvation. \`clientPingTimeout\` is the pong wait after the SDK sends a client ping; \`serverPingTimeout\` is the wait for Slack server pings. App messages and events remain application state, not transport liveness signals.

\## Manifest and scope checklist

The base Slack app manifest is the same for Socket Mode and HTTP Request URLs. Only the \`settings\` block (and the slash command \`url\`) differs.

Base manifest (Socket Mode def

_… [truncated; see https://docs.openclaw.ai/channels/slack.md for full content]_


---

## Telegram - OpenClaw

_Source: <https://docs.openclaw.ai/channels/telegram>_

[OpenClaw home page](https://docs.openclaw.ai/)

Mainstream messaging

Telegram

Production-ready for bot DMs and groups via grammY. Long polling is the default mode; webhook mode is optional.

[**Pairing** \\
\\
Default DM policy for Telegram is pairing.](https://docs.openclaw.ai/channels/pairing)

[**Channel troubleshooting** \\
\\
Cross-channel diagnostics and repair playbooks.](https://docs.openclaw.ai/channels/troubleshooting)

[**Gateway configuration** \\
\\
Full channel config patterns and examples.](https://docs.openclaw.ai/gateway/configuration)

## Quick setup

1

[Navigate to header](https://docs.openclaw.ai/channels/telegram#)

Create the bot token in BotFather

Open Telegram and chat with **@BotFather** (confirm the handle is exactly `@BotFather`).Run `/newbot`, follow prompts, and save the token.

2

[Navigate to header](https://docs.openclaw.ai/channels/telegram#)

Configure token and DM policy

```
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",
      groups: { "*": { requireMention: true } },
    },
  },
}
```

Env fallback: `TELEGRAM_BOT_TOKEN=...` (default account only).
Telegram does **not** use `openclaw channels login telegram`; configure token in config/env, then start gateway.

3

[Navigate to header](https://docs.openclaw.ai/channels/telegram#)

Start gateway and approve first DM

```
openclaw gateway
openclaw pairing list telegram
openclaw pairing approve telegram <CODE>
```

Pairing codes expire after 1 hour.

4

[Navigate to header](https://docs.openclaw.ai/channels/telegram#)

Add the bot to a group

Add the bot to your group, then set `channels.telegram.groups` and `groupPolicy` to match your access model.

Token resolution order is account-aware. In practice, config values win over env fallback, and `TELEGRAM_BOT_TOKEN` only applies to the default account.

## Telegram side settings

Privacy mode and group visibility

Telegram bots default to **Privacy Mode**, which limits what group messages they receive.If the bot must see all group messages, either:

- disable privacy mode via `/setprivacy`, or
- make the bot a group admin.

When toggling privacy mode, remove + re-add the bot in each group so Telegram applies the change.

Group permissions

Admin status is controlled in Telegram group settings.Admin bots receive all group messages, which is useful for always-on group behavior.

Helpful BotFather toggles

- `/setjoingroups` to allow/deny group adds
- `/setprivacy` for group visibility behavior

## Access control and activation

- DM policy

- Group policy and allowlists

- Mention behavior

`channels.telegram.dmPolicy` controls direct message access:

- `pairing` (default)
- `allowlist` (requires at least one sender ID in `allowFrom`)
- `open` (requires `allowFrom` to include `"*"`)
- `disabled`

`dmPolicy: "open"` with `allowFrom: ["*"]` lets any Telegram account that finds or guesses the bot username command the bot. Use it only for intentionally public bots with tightly restricted tools; one-owner bots should use `allowlist` with numeric user IDs.`channels.telegram.allowFrom` accepts numeric Telegram user IDs. `telegram:` / `tg:` prefixes are accepted and normalized.
In multi-account configs, a restrictive top-level `channels.telegram.allowFrom` is treated as a safety boundary: account-level `allowFrom: ["*"]` entries do not make that account public unless the effective account allowlist still contains an explicit wildcard after merging.
`dmPolicy: "allowlist"` with empty `allowFrom` blocks all DMs and is rejected by config validation.
Setup asks for numeric user IDs only.
If you upgraded and your config contains `@username` allowlist entries, run `openclaw doctor --fix` to resolve them (best-effort; requires a Telegram bot token).
If you previously relied on pairing-store allowlist files, `openclaw doctor --fix` can recover entries into `channels.telegram.allowFrom` in allowlist flows (for example when `dmPolicy: "allowlist"` has n

_… [truncated; see https://docs.openclaw.ai/channels/telegram for full content]_


---

## Channel troubleshooting - OpenClaw

_Source: <https://docs.openclaw.ai/channels/troubleshooting>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Channel troubleshooting

Use this page when a channel connects but behavior is wrong.

## Command ladder

Run these in order first:

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

Healthy baseline:

- `Runtime: running`
- `Connectivity probe: ok`
- `Capability: read-only`, `write-capable`, or `admin-capable`
- Channel probe shows transport connected and, where supported, `works` or `audit ok`

## WhatsApp

### WhatsApp failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| Connected but no DM replies | `openclaw pairing list whatsapp` | Approve sender or switch DM policy/allowlist. |
| Group messages ignored | Check `requireMention` \+ mention patterns in config | Mention the bot or relax mention policy for that group. |
| QR login times out with 408 | Check gateway `HTTPS_PROXY` / `HTTP_PROXY` env | Set a reachable proxy; use `NO_PROXY` only for bypasses. |
| Random disconnect/relogin loops | `openclaw channels status --probe` \+ logs | Recent reconnects are flagged even when currently connected; watch logs, restart the gateway, then relink if flapping continues. |

Full troubleshooting: [WhatsApp troubleshooting](https://docs.openclaw.ai/channels/whatsapp#troubleshooting)

## Telegram

### Telegram failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| `/start` but no usable reply flow | `openclaw pairing list telegram` | Approve pairing or change DM policy. |
| Bot online but group stays silent | Verify mention requirement and bot privacy mode | Disable privacy mode for group visibility or mention bot. |
| Send failures with network errors | Inspect logs for Telegram API call failures | Fix DNS/IPv6/proxy routing to `api.telegram.org`. |
| Startup reports `getMe returned 401` | Check configured token source | Re-copy or regenerate the BotFather token and update `botToken`, `tokenFile`, or default-account `TELEGRAM_BOT_TOKEN`. |
| Polling stalls or reconnects slowly | `openclaw logs --follow` for polling diagnostics | Upgrade; if restarts are false positives, tune `pollingStallThresholdMs`. Persistent stalls still point to proxy/DNS/IPv6. |
| `setMyCommands` rejected at startup | Inspect logs for `BOT_COMMANDS_TOO_MUCH` | Reduce plugin/skill/custom Telegram commands or disable native menus. |
| Upgraded and allowlist blocks you | `openclaw security audit` and config allowlists | Run `openclaw doctor --fix` or replace `@username` with numeric sender IDs. |

Full troubleshooting: [Telegram troubleshooting](https://docs.openclaw.ai/channels/telegram#troubleshooting)

## Discord

### Discord failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| Bot online but no guild replies | `openclaw channels status --probe` | Allow guild/channel and verify message content intent. |
| Group messages ignored | Check logs for mention gating drops | Mention bot or set guild/channel `requireMention: false`. |
| DM replies missing | `openclaw pairing list discord` | Approve DM pairing or adjust DM policy. |

Full troubleshooting: [Discord troubleshooting](https://docs.openclaw.ai/channels/discord#troubleshooting)

## Slack

### Slack failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| Socket mode connected but no responses | `openclaw channels status --probe` | Verify app token + bot token and required scopes; watch for `botTokenStatus` / `appTokenStatus = configured_unavailable` on SecretRef-backed setups. |
| DMs blocked | `openclaw pairing list slack` | Approve pairing or relax DM policy. |
| Channel message ignored | Check `groupPolicy` and channel allowlist | Allow the channel or switch policy to `open`. |

Full troubleshooting: [Slack troubleshooting](https://docs.openclaw.ai/channels/slack#troubleshooting)

## iMessage and BlueBubbles

### iMessage and BlueBubbles failure signatures

| Symptom | Fastest check | Fix |

_… [truncated; see https://docs.openclaw.ai/channels/troubleshooting for full content]_


---

## https://docs.openclaw.ai/channels/troubleshooting.md

_Source: <https://docs.openclaw.ai/channels/troubleshooting.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Channel troubleshooting

Use this page when a channel connects but behavior is wrong.

\## Command ladder

Run these in order first:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
\`\`\`

Healthy baseline:

\\* \`Runtime: running\`
\\* \`Connectivity probe: ok\`
\\* \`Capability: read-only\`, \`write-capable\`, or \`admin-capable\`
\\* Channel probe shows transport connected and, where supported, \`works\` or \`audit ok\`

\## WhatsApp

\### WhatsApp failure signatures

\| Symptom \| Fastest check \| Fix \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| Connected but no DM replies \| \`openclaw pairing list whatsapp\` \| Approve sender or switch DM policy/allowlist. \|
\| Group messages ignored \| Check \`requireMention\` + mention patterns in config \| Mention the bot or relax mention policy for that group. \|
\| QR login times out with 408 \| Check gateway \`HTTPS\_PROXY\` / \`HTTP\_PROXY\` env \| Set a reachable proxy; use \`NO\_PROXY\` only for bypasses. \|
\| Random disconnect/relogin loops \| \`openclaw channels status --probe\` + logs \| Recent reconnects are flagged even when currently connected; watch logs, restart the gateway, then relink if flapping continues. \|

Full troubleshooting: \[WhatsApp troubleshooting\](/channels/whatsapp#troubleshooting)

\## Telegram

\### Telegram failure signatures

\| Symptom \| Fastest check \| Fix \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`/start\` but no usable reply flow \| \`openclaw pairing list telegram\` \| Approve pairing or change DM policy. \|
\| Bot online but group stays silent \| Verify mention requirement and bot privacy mode \| Disable privacy mode for group visibility or mention bot. \|
\| Send failures with network errors \| Inspect logs for Telegram API call failures \| Fix DNS/IPv6/proxy routing to \`api.telegram.org\`. \|
\| Startup reports \`getMe returned 401\` \| Check configured token source \| Re-copy or regenerate the BotFather token and update \`botToken\`, \`tokenFile\`, or default-account \`TELEGRAM\_BOT\_TOKEN\`. \|
\| Polling stalls or reconnects slowly \| \`openclaw logs --follow\` for polling diagnostics \| Upgrade; if restarts are false positives, tune \`pollingStallThresholdMs\`. Persistent stalls still point to proxy/DNS/IPv6. \|
\| \`setMyCommands\` rejected at startup \| Inspect logs for \`BOT\_COMMANDS\_TOO\_MUCH\` \| Reduce plugin/skill/custom Telegram commands or disable native menus. \|
\| Upgraded and allowlist blocks you \| \`openclaw security audit\` and config allowlists \| Run \`openclaw doctor --fix\` or replace \`@username\` with numeric sender IDs. \|

Full troubleshooting: \[Telegram troubleshooting\](/channels/telegram#troubleshooting)

\## Discord

\### Discord failure signatures

\| Symptom \| Fastest check \| Fix \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-

_… [truncated; see https://docs.openclaw.ai/channels/troubleshooting.md for full content]_


---

## WeChat - OpenClaw

_Source: <https://docs.openclaw.ai/channels/wechat>_

[OpenClaw home page](https://docs.openclaw.ai/)

Regional platforms

WeChat

OpenClaw connects to WeChat through Tencent’s external
`@tencent-weixin/openclaw-weixin` channel plugin.Status: external plugin. Direct chats and media are supported. Group chats are not
advertised by the current plugin capability metadata.

## Naming

- **WeChat** is the user-facing name in these docs.
- **Weixin** is the name used by Tencent’s package and by the plugin id.
- `openclaw-weixin` is the OpenClaw channel id.
- `@tencent-weixin/openclaw-weixin` is the npm package.

Use `openclaw-weixin` in CLI commands and config paths.

## How it works

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

## Install

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

## Login

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

## Access control

Direct messages use the normal OpenClaw pairing and allowlist model for channel
plugins.Approve new senders:

```
openclaw pairing list openclaw-weixin
openclaw pairing approve openclaw-weixin <CODE>
```

For the full access-control model, see [Pairing](https://docs.openclaw.ai/channels/pairing).

## Compatibility

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

## Sidecar process

The WeChat plugin can run helper work beside the Gateway while it monitors the
Tencent iLink API. In issue #68451, that helper path exposed a bug in OpenClaw’s
generic stale-Gateway cleanup: a child process could try to clean up the parent
Gateway process, causing restart loops under process managers such as systemd.Current OpenClaw startup cleanup excludes the current process and its ancestors,
so a channel helper must not kill the Gateway that launched it. This fix is
generic; it is not a WeChat-specific path in core.

## Troubleshooting

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

_… [truncated; see https://docs.openclaw.ai/channels/wechat for full content]_


---

## WhatsApp - OpenClaw

_Source: <https://docs.openclaw.ai/channels/whatsapp>_

[OpenClaw home page](https://docs.openclaw.ai/)

Mainstream messaging

WhatsApp

Status: production-ready via WhatsApp Web (Baileys). Gateway owns linked session(s).

## Install (on demand)

- Onboarding (`openclaw onboard`) and `openclaw channels add --channel whatsapp`
prompt to install the WhatsApp plugin the first time you select it.
- `openclaw channels login --channel whatsapp` also offers the install flow when
the plugin is not present yet.
- Dev channel + git checkout: defaults to the local plugin path.
- Stable/Beta: uses the npm package `@openclaw/whatsapp` on the current official
release tag.

Manual install stays available:

```
openclaw plugins install @openclaw/whatsapp
```

Use the bare package to follow the current official release tag. Pin an exact
version only when you need a reproducible install.

[**Pairing** \\
\\
Default DM policy is pairing for unknown senders.](https://docs.openclaw.ai/channels/pairing)

[**Channel troubleshooting** \\
\\
Cross-channel diagnostics and repair playbooks.](https://docs.openclaw.ai/channels/troubleshooting)

[**Gateway configuration** \\
\\
Full channel config patterns and examples.](https://docs.openclaw.ai/gateway/configuration)

## Quick setup

1

[Navigate to header](https://docs.openclaw.ai/channels/whatsapp#)

Configure WhatsApp access policy

```
{
  channels: {
    whatsapp: {
      dmPolicy: "pairing",
      allowFrom: ["+15551234567"],
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15551234567"],
    },
  },
}
```

2

[Navigate to header](https://docs.openclaw.ai/channels/whatsapp#)

Link WhatsApp (QR)

```
openclaw channels login --channel whatsapp
```

For a specific account:

```
openclaw channels login --channel whatsapp --account work
```

To attach an existing/custom WhatsApp Web auth directory before login:

```
openclaw channels add --channel whatsapp --account work --auth-dir /path/to/wa-auth
openclaw channels login --channel whatsapp --account work
```

3

[Navigate to header](https://docs.openclaw.ai/channels/whatsapp#)

Start the gateway

```
openclaw gateway
```

4

[Navigate to header](https://docs.openclaw.ai/channels/whatsapp#)

Approve first pairing request (if using pairing mode)

```
openclaw pairing list whatsapp
openclaw pairing approve whatsapp <CODE>
```

Pairing requests expire after 1 hour. Pending requests are capped at 3 per channel.

OpenClaw recommends running WhatsApp on a separate number when possible. (The channel metadata and setup flow are optimized for that setup, but personal-number setups are also supported.)

## Deployment patterns

Dedicated number (recommended)

This is the cleanest operational mode:

- separate WhatsApp identity for OpenClaw
- clearer DM allowlists and routing boundaries
- lower chance of self-chat confusion

Minimal policy pattern:

```
{
  channels: {
    whatsapp: {
      dmPolicy: "allowlist",
      allowFrom: ["+15551234567"],
    },
  },
}
```

Personal-number fallback

Onboarding supports personal-number mode and writes a self-chat-friendly baseline:

- `dmPolicy: "allowlist"`
- `allowFrom` includes your personal number
- `selfChatMode: true`

In runtime, self-chat protections key off the linked self number and `allowFrom`.

WhatsApp Web-only channel scope

The messaging platform channel is WhatsApp Web-based (`Baileys`) in current OpenClaw channel architecture.There is no separate Twilio WhatsApp messaging channel in the built-in chat-channel registry.

## Runtime model

- Gateway owns the WhatsApp socket and reconnect loop.
- The reconnect watchdog uses WhatsApp Web transport activity, not only inbound app-message volume, so a quiet linked-device session is not restarted solely because nobody has sent a message recently. A longer application-silence cap still forces a reconnect if transport frames keep arriving but no application messages are handled for the watchdog window; after a transient reconnect for a recently active session, that application-silence check uses the normal message t

_… [truncated; see https://docs.openclaw.ai/channels/whatsapp for full content]_


---

## Deployment patterns

_Source: <https://docs.openclaw.ai/channels/whatsapp.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# WhatsApp

Status: production-ready via WhatsApp Web (Baileys). Gateway owns linked session(s).

\## Install (on demand)

\\* Onboarding (\`openclaw onboard\`) and \`openclaw channels add --channel whatsapp\`
 prompt to install the WhatsApp plugin the first time you select it.
\\* \`openclaw channels login --channel whatsapp\` also offers the install flow when
 the plugin is not present yet.
\\* Dev channel + git checkout: defaults to the local plugin path.
\\* Stable/Beta: uses the npm package \`@openclaw/whatsapp\` on the current official
 release tag.

Manual install stays available:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw plugins install @openclaw/whatsapp
\`\`\`

Use the bare package to follow the current official release tag. Pin an exact
version only when you need a reproducible install.

 Default DM policy is pairing for unknown senders.

 Cross-channel diagnostics and repair playbooks.

 Full channel config patterns and examples.

\## Quick setup

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 channels: {
 whatsapp: {
 dmPolicy: "pairing",
 allowFrom: \["+15551234567"\],
 groupPolicy: "allowlist",
 groupAllowFrom: \["+15551234567"\],
 },
 },
 }
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw channels login --channel whatsapp
 \`\`\`

 For a specific account:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw channels login --channel whatsapp --account work
 \`\`\`

 To attach an existing/custom WhatsApp Web auth directory before login:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw channels add --channel whatsapp --account work --auth-dir /path/to/wa-auth
 openclaw channels login --channel whatsapp --account work
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw pairing list whatsapp
 openclaw pairing approve whatsapp ```
    ```

    Pairing requests expire after 1 hour. Pending requests are capped at 3 per channel.
````
````

  OpenClaw recommends running WhatsApp on a separate number when possible. (The channel metadata and setup flow are optimized for that setup, but personal-number setups are also supported.)

## Deployment patterns

    This is the cleanest operational mode:

    * separate WhatsApp identity for OpenClaw
    * clearer DM allowlists and routing boundaries
    * lower chance of self-chat confusion

    Minimal policy pattern:

    ```json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
    {
      channels: {
        whatsapp: {
          dmPolicy: "allowlist",
          allowFrom: ["+15551234567"],
        },
      },
    }
    ```

    Onboarding supports personal-number mode and writes a self-chat-friendly baseline:

    * `dmPolicy: "allowlist"`
    * `allowFrom` includes your personal number
    * `selfChatMode: true`

    In runtime, self-chat protections key off the linked self number and `allowFrom`.

    The messaging platform channel is WhatsApp Web-based (`Baileys`) in current OpenClaw channel architecture.

    There is no separate Twilio WhatsApp messaging channel in the built-in chat-channel registry.

## Runtime model

* Gateway owns the WhatsApp socket and reconnect loop.
* The reconnect watchdog uses WhatsApp Web transport activity, not only inbound app-message volume, so a quiet linked-device session is not restarted solely because nobody has sent a message recently. A longer application-silence cap still forces a reconnect if transport frames keep arriving but no application messages are handled for the watchdog window; after a transient reconnect for a recently active session, that application-silence check uses the normal

_… [truncated; see https://docs.openclaw.ai/channels/whatsapp.md for full content]_


---

## Channels - OpenClaw

_Source: <https://docs.openclaw.ai/cli/channels>_

# `openclaw channels`

Manage chat channel accounts and their runtime status on the Gateway.Related docs:

- Channel guides: [Channels](https://docs.openclaw.ai/channels)
- Gateway configuration: [Configuration](https://docs.openclaw.ai/gateway/configuration)

## Common commands

```
openclaw channels list
openclaw channels status
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
openclaw channels resolve --channel slack "#general" "@jane"
openclaw channels logs --channel all
```

## Status / capabilities / resolve / logs

- `channels status`: `--probe`, `--timeout <ms>`, `--json`
- `channels capabilities`: `--channel <name>`, `--account <id>` (only with `--channel`), `--target <dest>`, `--timeout <ms>`, `--json`
- `channels resolve`: `<entries...>`, `--channel <name>`, `--account <id>`, `--kind <auto|user|group>`, `--json`
- `channels logs`: `--channel <name|all>`, `--lines <n>`, `--json`

`channels status --probe` is the live path: on a reachable gateway it runs per-account
`probeAccount` and optional `auditAccount` checks, so output can include transport
state plus probe results such as `works`, `probe failed`, `audit ok`, or `audit failed`.
If the gateway is unreachable, `channels status` falls back to config-only summaries
instead of live probe output.Do not use `openclaw sessions`, Gateway `sessions.list`, or the agent
`sessions_list` tool as a channel socket-health signal. Those surfaces report
stored conversation rows, not provider runtime state. After a Discord provider
restart, a connected but quiet account may be healthy while no Discord session
row appears until the next inbound or outbound conversation event.

## Add / remove accounts

```
openclaw channels add --channel telegram --token <bot-token>
openclaw channels add --channel nostr --private-key "$NOSTR_PRIVATE_KEY"
openclaw channels remove --channel telegram --delete
```

`openclaw channels add --help` shows per-channel flags (token, private key, app token, signal-cli paths, etc).

`channels remove` only operates on installed/configured channel plugins. Use `channels add` first for installable catalog channels.
For runtime-backed channel plugins, `channels remove` also asks the running Gateway to stop the selected account before it updates config, so disabling or deleting an account does not leave the old listener active until restart.Common non-interactive add surfaces include:

- bot-token channels: `--token`, `--bot-token`, `--app-token`, `--token-file`
- Signal/iMessage transport fields: `--signal-number`, `--cli-path`, `--http-url`, `--http-host`, `--http-port`, `--db-path`, `--service`, `--region`
- Google Chat fields: `--webhook-path`, `--webhook-url`, `--audience-type`, `--audience`
- Matrix fields: `--homeserver`, `--user-id`, `--access-token`, `--password`, `--device-name`, `--initial-sync-limit`
- Nostr fields: `--private-key`, `--relay-urls`
- Tlon fields: `--ship`, `--url`, `--code`, `--group-channels`, `--dm-allowlist`, `--auto-discover-channels`
- `--use-env` for default-account env-backed auth where supported

If a channel plugin needs to be installed during a flag-driven add command, OpenClaw uses the channel’s default install source without opening the interactive plugin install prompt.When you run `openclaw channels add` without flags, the interactive wizard can prompt:

- account ids per selected channel
- optional display names for those accounts
- `Bind configured channel accounts to agents now?`

If you confirm bind now, the wizard asks which agent should own each configured channel account and writes account-scoped routing bindings.You can also manage the same routing rules later with `openclaw agents bindings`, `openclaw agents bind`, and `openclaw agents unbind` (see [agents](https://docs.openclaw.ai/cli/agents)).When you add a non-default account to a channel that is still using single-account top-level settings, OpenClaw promotes account-scoped top-level values into the channel’s

_… [truncated; see https://docs.openclaw.ai/cli/channels for full content]_


---

## https://docs.openclaw.ai/cli/channels.md

_Source: <https://docs.openclaw.ai/cli/channels.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Channels

\# \`openclaw channels\`

Manage chat channel accounts and their runtime status on the Gateway.

Related docs:

\\* Channel guides: \[Channels\](/channels)
\\* Gateway configuration: \[Configuration\](/gateway/configuration)

\## Common commands

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw channels list
openclaw channels status
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
openclaw channels resolve --channel slack "#general" "@jane"
openclaw channels logs --channel all
\`\`\`

\## Status / capabilities / resolve / logs

\\* \`channels status\`: \`--probe\`, \`--timeout \`, \`--json\`
\\* \`channels capabilities\`: \`--channel \`, \`--account \` (only with \`--channel\`), \`--target \`, \`--timeout \`, \`--json\`
\\* \`channels resolve\`: \`\`, \`--channel \`, \`--account \`, \`--kind \`, \`--json\`
\\* \`channels logs\`: \`--channel \`, \`--lines \`, \`--json\`

\`channels status --probe\` is the live path: on a reachable gateway it runs per-account
\`probeAccount\` and optional \`auditAccount\` checks, so output can include transport
state plus probe results such as \`works\`, \`probe failed\`, \`audit ok\`, or \`audit failed\`.
If the gateway is unreachable, \`channels status\` falls back to config-only summaries
instead of live probe output.

Do not use \`openclaw sessions\`, Gateway \`sessions.list\`, or the agent
\`sessions\_list\` tool as a channel socket-health signal. Those surfaces report
stored conversation rows, not provider runtime state. After a Discord provider
restart, a connected but quiet account may be healthy while no Discord session
row appears until the next inbound or outbound conversation event.

\## Add / remove accounts

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw channels add --channel telegram --token
openclaw channels add --channel nostr --private-key "$NOSTR\_PRIVATE\_KEY"
openclaw channels remove --channel telegram --delete
\`\`\`

 \`openclaw channels add --help\` shows per-channel flags (token, private key, app token, signal-cli paths, etc).

\`channels remove\` only operates on installed/configured channel plugins. Use \`channels add\` first for installable catalog channels.
For runtime-backed channel plugins, \`channels remove\` also asks the running Gateway to stop the selected account before it updates config, so disabling or deleting an account does not leave the old listener active until restart.

Common non-interactive add surfaces include:

\\* bot-token channels: \`--token\`, \`--bot-token\`, \`--app-token\`, \`--token-file\`
\\* Signal/iMessage transport fields: \`--signal-number\`, \`--cli-path\`, \`--http-url\`, \`--http-host\`, \`--http-port\`, \`--db-path\`, \`--service\`, \`--region\`
\\* Google Chat fields: \`--webhook-path\`, \`--webhook-url\`, \`--audience-type\`, \`--audience\`
\\* Matrix fields: \`--homeserver\`, \`--user-id\`, \`--access-token\`, \`--password\`, \`--device-name\`, \`--initial-sync-limit\`
\\* Nostr fields: \`--private-key\`, \`--relay-urls\`
\\* Tlon fields: \`--ship\`, \`--url\`, \`--code\`, \`--group-channels\`, \`--dm-allowlist\`, \`--auto-discover-channels\`
\\* \`--use-env\` for default-account env-backed auth where supported

If a channel plugin needs to be installed during a flag-driven add command, OpenClaw uses the channel's default install source without opening the interactive plugin install prompt.

When you run \`openclaw channels add\` without flags, the interactive wizard can prompt:

\\* account ids per selected channel
\\* optional display names for those accounts
\\* \`Bind configured channel accounts to agents now?\`

If you confirm bind now, the wizard asks which agent should own each configured channel account and writes account-scoped routing bindings.

_… [truncated; see https://docs.openclaw.ai/cli/channels.md for full content]_


---

## Pairing - OpenClaw

_Source: <https://docs.openclaw.ai/cli/pairing>_

# `openclaw pairing`

Approve or inspect DM pairing requests (for channels that support pairing).Related:

- Pairing flow: [Pairing](https://docs.openclaw.ai/channels/pairing)

## Commands

```
openclaw pairing list telegram
openclaw pairing list --channel telegram --account work
openclaw pairing list telegram --json

openclaw pairing approve <code>
openclaw pairing approve telegram <code>
openclaw pairing approve --channel telegram --account work <code> --notify
```

## `pairing list`

List pending pairing requests for one channel.Options:

- `[channel]`: positional channel id
- `--channel <channel>`: explicit channel id
- `--account <accountId>`: account id for multi-account channels
- `--json`: machine-readable output

Notes:

- If multiple pairing-capable channels are configured, you must provide a channel either positionally or with `--channel`.
- Extension channels are allowed as long as the channel id is valid.

## `pairing approve`

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

## Notes

- Channel input: pass it positionally (`pairing list telegram`) or with `--channel <channel>`.
- `pairing list` supports `--account <accountId>` for multi-account channels.
- `pairing approve` supports `--account <accountId>` and `--notify`.
- If only one pairing-capable channel is configured, `pairing approve <code>` is allowed.
- If you approved a sender before this bootstrap existed, run `openclaw doctor`; it warns when no command owner is configured and shows the `openclaw config set commands.ownerAllowFrom ...` command to fix it.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Channel pairing](https://docs.openclaw.ai/channels/pairing)

[Directory](https://docs.openclaw.ai/cli/directory) [QR](https://docs.openclaw.ai/cli/qr)

Ctrl+I


---

## `pairing list`

_Source: <https://docs.openclaw.ai/cli/pairing.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Pairing

\# \`openclaw pairing\`

Approve or inspect DM pairing requests (for channels that support pairing).

Related:

\\* Pairing flow: \[Pairing\](/channels/pairing)

\## Commands

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw pairing list telegram
openclaw pairing list --channel telegram --account work
openclaw pairing list telegram --json

openclaw pairing approve ```
openclaw pairing approve telegram
openclaw pairing approve --channel telegram --account work  --notify
```

## `pairing list`

List pending pairing requests for one channel.

Options:

* `[channel]`: positional channel id
* `--channel `: explicit channel id
* `--account `: account id for multi-account channels
* `--json`: machine-readable output

Notes:

* If multiple pairing-capable channels are configured, you must provide a channel either positionally or with `--channel`.
* Extension channels are allowed as long as the channel id is valid.

## `pairing approve`

Approve a pending pairing code and allow that sender.

Usage:

* `openclaw pairing approve  `
* `openclaw pairing approve --channel  `
* `openclaw pairing approve ` when exactly one pairing-capable channel is configured

Options:

* `--channel `: explicit channel id
* `--account `: account id for multi-account channels
* `--notify`: send a confirmation back to the requester on the same channel

Owner bootstrap:

* If `commands.ownerAllowFrom` is empty when you approve a pairing code, OpenClaw also records the approved sender as the command owner, using a channel-scoped entry such as `telegram:123456789`.
* This only bootstraps the first owner. Later pairing approvals do not replace or expand `commands.ownerAllowFrom`.
* The command owner is the human operator account allowed to run owner-only commands and approve dangerous actions such as `/diagnostics`, `/export-trajectory`, `/config`, and exec approvals.

## Notes

* Channel input: pass it positionally (`pairing list telegram`) or with `--channel `.
* `pairing list` supports `--account ` for multi-account channels.
* `pairing approve` supports `--account ` and `--notify`.
* If only one pairing-capable channel is configured, `pairing approve ` is allowed.
* If you approved a sender before this bootstrap existed, run `openclaw doctor`; it warns when no command owner is configured and shows the `openclaw config set commands.ownerAllowFrom ...` command to fix it.

## Related

* [CLI reference](/cli)
* [Channel pairing](/channels/pairing)
```


---

## Gateway-owned pairing - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/pairing>_

[OpenClaw home page](https://docs.openclaw.ai/)

Networking and discovery

Gateway-owned pairing

In Gateway-owned pairing, the **Gateway** is the source of truth for which nodes
are allowed to join. UIs (macOS app, future clients) are just frontends that
approve or reject pending requests.**Important:** WS nodes use **device pairing** (role `node`) during `connect`.
`node.pair.*` is a separate pairing store and does **not** gate the WS handshake.
Only clients that explicitly call `node.pair.*` use this flow.

## Concepts

- **Pending request**: a node asked to join; requires approval.
- **Paired node**: approved node with an issued auth token.
- **Transport**: the Gateway WS endpoint forwards requests but does not decide
membership. (Legacy TCP bridge support has been removed.)

## How pairing works

1. A node connects to the Gateway WS and requests pairing.
2. The Gateway stores a **pending request** and emits `node.pair.requested`.
3. You approve or reject the request (CLI or UI).
4. On approval, the Gateway issues a **new token** (tokens are rotated on re‑pair).
5. The node reconnects using the token and is now “paired”.

Pending requests expire automatically after **5 minutes**.

## CLI workflow (headless friendly)

```
openclaw nodes pending
openclaw nodes approve <requestId>
openclaw nodes reject <requestId>
openclaw nodes status
openclaw nodes remove --node <id|name|ip>
openclaw nodes rename --node <id|name|ip> --name "Living Room iPad"
```

`nodes status` shows paired/connected nodes and their capabilities.

## API surface (gateway protocol)

Events:

- `node.pair.requested` — emitted when a new pending request is created.
- `node.pair.resolved` — emitted when a request is approved/rejected/expired.

Methods:

- `node.pair.request` — create or reuse a pending request.
- `node.pair.list` — list pending + paired nodes (`operator.pairing`).
- `node.pair.approve` — approve a pending request (issues token).
- `node.pair.reject` — reject a pending request.
- `node.pair.remove` — remove a stale paired node entry.
- `node.pair.verify` — verify `{ nodeId, token }`.

Notes:

- `node.pair.request` is idempotent per node: repeated calls return the same
pending request.
- Repeated requests for the same pending node also refresh the stored node
metadata and the latest allowlisted declared command snapshot for operator visibility.
- Approval **always** generates a fresh token; no token is ever returned from
`node.pair.request`.
- Requests may include `silent: true` as a hint for auto-approval flows.
- `node.pair.approve`uses the pending request’s declared commands to enforce
extra approval scopes:

  - commandless request: `operator.pairing`
  - non-exec command request: `operator.pairing` \+ `operator.write`
  - `system.run` / `system.run.prepare` / `system.which` request:
    `operator.pairing` \+ `operator.admin`

Node pairing is a trust and identity flow plus token issuance. It does **not** pin the live node command surface per node.

- Live node commands come from what the node declares on connect after the gateway’s global node command policy (`gateway.nodes.allowCommands` and `denyCommands`) is applied.
- Per-node `system.run` allow and ask policy lives on the node in `exec.approvals.node.*`, not in the pairing record.

## Node command gating (2026.3.31+)

**Breaking change:** Starting with `2026.3.31`, node commands are disabled until node pairing is approved. Device pairing alone is no longer enough to expose declared node commands.

When a node connects for the first time, pairing is requested automatically. Until the pairing request is approved, all pending node commands from that node are filtered and will not execute. Once trust is established through pairing approval, the node’s declared commands become available subject to the normal command policy.This means:

- Nodes that were previously relying on device pairing alone to expose commands must now complete node pairing.
- Commands queued before pairing approval are d

_… [truncated; see https://docs.openclaw.ai/gateway/pairing for full content]_
