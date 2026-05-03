---
source_url: https://docs.openclaw.ai/channels/feishu
title: "Feishu - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels/feishu#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Regional platforms

Feishu

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Feishu / Lark](https://docs.openclaw.ai/channels/feishu#feishu-%2F-lark)
- [Quick start](https://docs.openclaw.ai/channels/feishu#quick-start)
- [Access control](https://docs.openclaw.ai/channels/feishu#access-control)
- [Direct messages](https://docs.openclaw.ai/channels/feishu#direct-messages)
- [Group chats](https://docs.openclaw.ai/channels/feishu#group-chats)
- [Group configuration examples](https://docs.openclaw.ai/channels/feishu#group-configuration-examples)
- [Allow all groups, no @mention required](https://docs.openclaw.ai/channels/feishu#allow-all-groups-no-%40mention-required)
- [Allow all groups, still require @mention](https://docs.openclaw.ai/channels/feishu#allow-all-groups-still-require-%40mention)
- [Allow specific groups only](https://docs.openclaw.ai/channels/feishu#allow-specific-groups-only)
- [Restrict senders within a group](https://docs.openclaw.ai/channels/feishu#restrict-senders-within-a-group)
- [Get group/user IDs](https://docs.openclaw.ai/channels/feishu#get-group%2Fuser-ids)
- [Group IDs (chat\_id, format: oc\_xxx)](https://docs.openclaw.ai/channels/feishu#group-ids-chat_id-format-oc_xxx)
- [User IDs (open\_id, format: ou\_xxx)](https://docs.openclaw.ai/channels/feishu#user-ids-open_id-format-ou_xxx)
- [Common commands](https://docs.openclaw.ai/channels/feishu#common-commands)
- [Troubleshooting](https://docs.openclaw.ai/channels/feishu#troubleshooting)
- [Bot does not respond in group chats](https://docs.openclaw.ai/channels/feishu#bot-does-not-respond-in-group-chats)
- [Bot does not receive messages](https://docs.openclaw.ai/channels/feishu#bot-does-not-receive-messages)
- [App Secret leaked](https://docs.openclaw.ai/channels/feishu#app-secret-leaked)
- [Advanced configuration](https://docs.openclaw.ai/channels/feishu#advanced-configuration)
- [Multiple accounts](https://docs.openclaw.ai/channels/feishu#multiple-accounts)
- [Message limits](https://docs.openclaw.ai/channels/feishu#message-limits)
- [Streaming](https://docs.openclaw.ai/channels/feishu#streaming)
- [Quota optimization](https://docs.openclaw.ai/channels/feishu#quota-optimization)
- [ACP sessions](https://docs.openclaw.ai/channels/feishu#acp-sessions)
- [Persistent ACP binding](https://docs.openclaw.ai/channels/feishu#persistent-acp-binding)
- [Spawn ACP from chat](https://docs.openclaw.ai/channels/feishu#spawn-acp-from-chat)
- [Multi-agent routing](https://docs.openclaw.ai/channels/feishu#multi-agent-routing)
- [Configuration reference](https://docs.openclaw.ai/channels/feishu#configuration-reference)
- [Supported message types](https://docs.openclaw.ai/channels/feishu#supported-message-types)
- [Receive](https://docs.openclaw.ai/channels/feishu#receive)
- [Send](https://docs.openclaw.ai/channels/feishu#send)
- [Threads and replies](https://docs.openclaw.ai/channels/feishu#threads-and-replies)
- [Related](https://docs.openclaw.ai/channels/feishu#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/channels/feishu\#feishu-/-lark)  Feishu / Lark

Feishu/Lark is an all-in-one collaboration platform where teams chat, share documents, manage calendars, and get work done together.**Status:** production-ready for bot DMs + group chats. WebSocket is the default mode; webhook mode is optional.

* * *

## [​](https://docs.openclaw.ai/channels/feishu\#quick-start)  Quick start

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

## [​](https://docs.openclaw.ai/channels/feishu\#access-control)  Access control

### [​](https://docs.openclaw.ai/channels/feishu\#direct-messages)  Direct messages

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

### [​](https://docs.openclaw.ai/channels/feishu\#group-chats)  Group chats

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

## [​](https://docs.openclaw.ai/channels/feishu\#group-configuration-examples)  Group configuration examples

### [​](https://docs.openclaw.ai/channels/feishu\#allow-all-groups-no-@mention-required)  Allow all groups, no @mention required

```
{
  channels: {
    feishu: {
      groupPolicy: "open",
    },
  },
}
```

### [​](https://docs.openclaw.ai/channels/feishu\#allow-all-groups-still-require-@mention)  Allow all groups, still require @mention

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

### [​](https://docs.openclaw.ai/channels/feishu\#allow-specific-groups-only)  Allow specific groups only

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

### [​](https://docs.openclaw.ai/channels/feishu\#restrict-senders-within-a-group)  Restrict senders within a group

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

## [​](https://docs.openclaw.ai/channels/feishu\#get-group/user-ids)  Get group/user IDs

### [​](https://docs.openclaw.ai/channels/feishu\#group-ids-chat_id-format-oc_xxx)  Group IDs (`chat_id`, format: `oc_xxx`)

Open the group in Feishu/Lark, click the menu icon in the top-right corner, and go to **Settings**. The group ID (`chat_id`) is listed on the settings page.![Get Group ID](https://mintcdn.com/clawdhub/0NpU6wNaI7exeaOE/images/feishu-get-group-id.png?fit=max&auto=format&n=0NpU6wNaI7exeaOE&q=85&s=1c9b41e1f9743621dfdd3abf7e952405)

### [​](https://docs.openclaw.ai/channels/feishu\#user-ids-open_id-format-ou_xxx)  User IDs (`open_id`, format: `ou_xxx`)

Start the gateway, send a DM to the bot, then check the logs:

```
openclaw logs --follow
```

Look for `open_id` in the log output. You can also check pending pairing requests:

```
openclaw pairing list feishu
```

* * *

## [​](https://docs.openclaw.ai/channels/feishu\#common-commands)  Common commands

| Command | Description |
| --- | --- |
| `/status` | Show bot status |
| `/reset` | Reset the current session |
| `/model` | Show or switch the AI model |

Feishu/Lark does not support native slash-command menus, so send these as plain text messages.

* * *

## [​](https://docs.openclaw.ai/channels/feishu\#troubleshooting)  Troubleshooting

### [​](https://docs.openclaw.ai/channels/feishu\#bot-does-not-respond-in-group-chats)  Bot does not respond in group chats

1. Ensure the bot is added to the group
2. Ensure you @mention the bot (required by default)
3. Verify `groupPolicy` is not `"disabled"`
4. Check logs: `openclaw logs --follow`

### [​](https://docs.openclaw.ai/channels/feishu\#bot-does-not-receive-messages)  Bot does not receive messages

1. Ensure the bot is published and approved in Feishu Open Platform / Lark Developer
2. Ensure event subscription includes `im.message.receive_v1`
3. Ensure **persistent connection** (WebSocket) is selected
4. Ensure all required permission scopes are granted
5. Ensure the gateway is running: `openclaw gateway status`
6. Check logs: `openclaw logs --follow`

### [​](https://docs.openclaw.ai/channels/feishu\#app-secret-leaked)  App Secret leaked

1. Reset the App Secret in Feishu Open Platform / Lark Developer
2. Update the value in your config
3. Restart the gateway: `openclaw gateway restart`

* * *

## [​](https://docs.openclaw.ai/channels/feishu\#advanced-configuration)  Advanced configuration

### [​](https://docs.openclaw.ai/channels/feishu\#multiple-accounts)  Multiple accounts

```
{
  channels: {
    feishu: {
      defaultAccount: "main",
      accounts: {
        main: {
          appId: "cli_xxx",
          appSecret: "xxx",
          name: "Primary bot",
          tts: {
            providers: {
              openai: { voice: "shimmer" },
            },
          },
        },
        backup: {
          appId: "cli_yyy",
          appSecret: "yyy",
          name: "Backup bot",
          enabled: false,
        },
      },
    },
  },
}
```

`defaultAccount` controls which account is used when outbound APIs do not specify an `accountId`.
`accounts.<id>.tts` uses the same shape as `messages.tts` and deep-merges over
global TTS config, so multi-bot Feishu setups can keep shared provider
credentials globally while overriding only voice, model, persona, or auto mode
per account.

### [​](https://docs.openclaw.ai/channels/feishu\#message-limits)  Message limits

- `textChunkLimit` — outbound text chunk size (default: `2000` chars)
- `mediaMaxMb` — media upload/download limit (default: `30` MB)

### [​](https://docs.openclaw.ai/channels/feishu\#streaming)  Streaming

Feishu/Lark supports streaming replies via interactive cards. When enabled, the bot updates the card in real time as it generates text.

```
{
  channels: {
    feishu: {
      streaming: true, // enable streaming card output (default: true)
      blockStreaming: true, // enable block-level streaming (default: true)
    },
  },
}
```

Set `streaming: false` to send the complete reply in one message.

### [​](https://docs.openclaw.ai/channels/feishu\#quota-optimization)  Quota optimization

Reduce the number of Feishu/Lark API calls with two optional flags:

- `typingIndicator` (default `true`): set `false` to skip typing reaction calls
- `resolveSenderNames` (default `true`): set `false` to skip sender profile lookups

```
{
  channels: {
    feishu: {
      typingIndicator: false,
      resolveSenderNames: false,
    },
  },
}
```

### [​](https://docs.openclaw.ai/channels/feishu\#acp-sessions)  ACP sessions

Feishu/Lark supports ACP for DMs and group thread messages. Feishu/Lark ACP is text-command driven — there are no native slash-command menus, so use `/acp ...` messages directly in the conversation.

#### [​](https://docs.openclaw.ai/channels/feishu\#persistent-acp-binding)  Persistent ACP binding

```
{
  agents: {
    list: [\
      {\
        id: "codex",\
        runtime: {\
          type: "acp",\
          acp: {\
            agent: "codex",\
            backend: "acpx",\
            mode: "persistent",\
            cwd: "/workspace/openclaw",\
          },\
        },\
      },\
    ],
  },
  bindings: [\
    {\
      type: "acp",\
      agentId: "codex",\
      match: {\
        channel: "feishu",\
        accountId: "default",\
        peer: { kind: "direct", id: "ou_1234567890" },\
      },\
    },\
    {\
      type: "acp",\
      agentId: "codex",\
      match: {\
        channel: "feishu",\
        accountId: "default",\
        peer: { kind: "group", id: "oc_group_chat:topic:om_topic_root" },\
      },\
      acp: { label: "codex-feishu-topic" },\
    },\
  ],
}
```

#### [​](https://docs.openclaw.ai/channels/feishu\#spawn-acp-from-chat)  Spawn ACP from chat

In a Feishu/Lark DM or thread:

```
/acp spawn codex --thread here
```

`--thread here` works for DMs and Feishu/Lark thread messages. Follow-up messages in the bound conversation route directly to that ACP session.

### [​](https://docs.openclaw.ai/channels/feishu\#multi-agent-routing)  Multi-agent routing

Use `bindings` to route Feishu/Lark DMs or groups to different agents.

```
{
  agents: {
    list: [\
      { id: "main" },\
      { id: "agent-a", workspace: "/home/user/agent-a" },\
      { id: "agent-b", workspace: "/home/user/agent-b" },\
    ],
  },
  bindings: [\
    {\
      agentId: "agent-a",\
      match: {\
        channel: "feishu",\
        peer: { kind: "direct", id: "ou_xxx" },\
      },\
    },\
    {\
      agentId: "agent-b",\
      match: {\
        channel: "feishu",\
        peer: { kind: "group", id: "oc_zzz" },\
      },\
    },\
  ],
}
```

Routing fields:

- `match.channel`: `"feishu"`
- `match.peer.kind`: `"direct"` (DM) or `"group"` (group chat)
- `match.peer.id`: user Open ID (`ou_xxx`) or group ID (`oc_xxx`)

See [Get group/user IDs](https://docs.openclaw.ai/channels/feishu#get-groupuser-ids) for lookup tips.

* * *

## [​](https://docs.openclaw.ai/channels/feishu\#configuration-reference)  Configuration reference

Full configuration: [Gateway configuration](https://docs.openclaw.ai/gateway/configuration)

| Setting | Description | Default |
| --- | --- | --- |
| `channels.feishu.enabled` | Enable/disable the channel | `true` |
| `channels.feishu.domain` | API domain (`feishu` or `lark`) | `feishu` |
| `channels.feishu.connectionMode` | Event transport (`websocket` or `webhook`) | `websocket` |
| `channels.feishu.defaultAccount` | Default account for outbound routing | `default` |
| `channels.feishu.verificationToken` | Required for webhook mode | — |
| `channels.feishu.encryptKey` | Required for webhook mode | — |
| `channels.feishu.webhookPath` | Webhook route path | `/feishu/events` |
| `channels.feishu.webhookHost` | Webhook bind host | `127.0.0.1` |
| `channels.feishu.webhookPort` | Webhook bind port | `3000` |
| `channels.feishu.accounts.<id>.appId` | App ID | — |
| `channels.feishu.accounts.<id>.appSecret` | App Secret | — |
| `channels.feishu.accounts.<id>.domain` | Per-account domain override | `feishu` |
| `channels.feishu.accounts.<id>.tts` | Per-account TTS override | `messages.tts` |
| `channels.feishu.dmPolicy` | DM policy | `allowlist` |
| `channels.feishu.allowFrom` | DM allowlist (open\_id list) | \[BotOwnerId\] |
| `channels.feishu.groupPolicy` | Group policy | `allowlist` |
| `channels.feishu.groupAllowFrom` | Group allowlist | — |
| `channels.feishu.requireMention` | Require @mention in groups | `true` |
| `channels.feishu.groups.<chat_id>.requireMention` | Per-group @mention override; explicit IDs also admit the group in allowlist mode | inherited |
| `channels.feishu.groups.<chat_id>.enabled` | Enable/disable a specific group | `true` |
| `channels.feishu.textChunkLimit` | Message chunk size | `2000` |
| `channels.feishu.mediaMaxMb` | Media size limit | `30` |
| `channels.feishu.streaming` | Streaming card output | `true` |
| `channels.feishu.blockStreaming` | Block-level streaming | `true` |
| `channels.feishu.typingIndicator` | Send typing reactions | `true` |
| `channels.feishu.resolveSenderNames` | Resolve sender display names | `true` |

* * *

## [​](https://docs.openclaw.ai/channels/feishu\#supported-message-types)  Supported message types

### [​](https://docs.openclaw.ai/channels/feishu\#receive)  Receive

- ✅ Text
- ✅ Rich text (post)
- ✅ Images
- ✅ Files
- ✅ Audio
- ✅ Video/media
- ✅ Stickers

Inbound Feishu/Lark audio messages are normalized as media placeholders instead
of raw `file_key` JSON. When `tools.media.audio` is configured, OpenClaw
downloads the voice-note resource and runs shared audio transcription before the
agent turn, so the agent receives the spoken transcript. If Feishu includes
transcript text directly in the audio payload, that text is used without another
ASR call. Without an audio transcription provider, the agent still receives a
`<media:audio>` placeholder plus the saved attachment, not the raw Feishu
resource payload.

### [​](https://docs.openclaw.ai/channels/feishu\#send)  Send

- ✅ Text
- ✅ Images
- ✅ Files
- ✅ Audio
- ✅ Video/media
- ✅ Interactive cards (including streaming updates)
- ⚠️ Rich text (post-style formatting; doesn’t support full Feishu/Lark authoring capabilities)

Native Feishu/Lark audio bubbles use the Feishu `audio` message type and require
Ogg/Opus upload media (`file_type: "opus"`). Existing `.opus` and `.ogg` media
is sent directly as native audio. MP3/WAV/M4A and other likely audio formats are
transcoded to 48kHz Ogg/Opus with `ffmpeg` only when the reply requests voice
delivery (`audioAsVoice` / message tool `asVoice`, including TTS voice-note
replies). Ordinary MP3 attachments stay regular files. If `ffmpeg` is missing or
conversion fails, OpenClaw falls back to a file attachment and logs the reason.

### [​](https://docs.openclaw.ai/channels/feishu\#threads-and-replies)  Threads and replies

- ✅ Inline replies
- ✅ Thread replies
- ✅ Media replies stay thread-aware when replying to a thread message

For `groupSessionScope: "group_topic"` and `"group_topic_sender"`, native
Feishu/Lark topic groups use the event `thread_id` (`omt_*`) as the canonical
topic session key. Normal group replies that OpenClaw turns into threads keep
using the reply root message ID (`om_*`) so the first turn and follow-up turn
stay in the same session.

* * *

## [​](https://docs.openclaw.ai/channels/feishu\#related)  Related

- [Channels Overview](https://docs.openclaw.ai/channels) — all supported channels
- [Pairing](https://docs.openclaw.ai/channels/pairing) — DM authentication and pairing flow
- [Groups](https://docs.openclaw.ai/channels/groups) — group chat behavior and mention gating
- [Channel Routing](https://docs.openclaw.ai/channels/channel-routing) — session routing for messages
- [Security](https://docs.openclaw.ai/gateway/security) — access model and hardening

[QQ bot](https://docs.openclaw.ai/channels/qqbot) [Yuanbao](https://docs.openclaw.ai/channels/yuanbao)

Ctrl+I

![Get Group ID](https://mintcdn.com/clawdhub/0NpU6wNaI7exeaOE/images/feishu-get-group-id.png?w=1100&fit=max&auto=format&n=0NpU6wNaI7exeaOE&q=85&s=36df634e2caf2690c29c722f5068b77b)