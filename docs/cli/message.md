---
source_url: https://docs.openclaw.ai/cli/message
title: "Message - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/message#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Agents and sessions

Message

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw message](https://docs.openclaw.ai/cli/message#openclaw-message)
- [Usage](https://docs.openclaw.ai/cli/message#usage)
- [Common flags](https://docs.openclaw.ai/cli/message#common-flags)
- [SecretRef behavior](https://docs.openclaw.ai/cli/message#secretref-behavior)
- [Actions](https://docs.openclaw.ai/cli/message#actions)
- [Core](https://docs.openclaw.ai/cli/message#core)
- [Threads](https://docs.openclaw.ai/cli/message#threads)
- [Emojis](https://docs.openclaw.ai/cli/message#emojis)
- [Stickers](https://docs.openclaw.ai/cli/message#stickers)
- [Roles / Channels / Members / Voice](https://docs.openclaw.ai/cli/message#roles-%2F-channels-%2F-members-%2F-voice)
- [Events](https://docs.openclaw.ai/cli/message#events)
- [Moderation (Discord)](https://docs.openclaw.ai/cli/message#moderation-discord)
- [Broadcast](https://docs.openclaw.ai/cli/message#broadcast)
- [Examples](https://docs.openclaw.ai/cli/message#examples)
- [Related](https://docs.openclaw.ai/cli/message#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/message\#openclaw-message)  `openclaw message`

Single outbound command for sending messages and channel actions
(Discord/Google Chat/iMessage/Matrix/Mattermost (plugin)/Microsoft Teams/Signal/Slack/Telegram/WhatsApp).

## [​](https://docs.openclaw.ai/cli/message\#usage)  Usage

```
openclaw message <subcommand> [flags]
```

Channel selection:

- `--channel` required if more than one channel is configured.
- If exactly one channel is configured, it becomes the default.
- Values: `discord|googlechat|imessage|matrix|mattermost|msteams|signal|slack|telegram|whatsapp` (Mattermost requires plugin)
- `openclaw message` resolves the selected channel to its owning plugin when `--channel` or a channel-prefixed target is present; otherwise it loads configured channel plugins for default-channel inference.

Target formats (`--target`):

- WhatsApp: E.164, group JID, or WhatsApp Channel/Newsletter JID (`...@newsletter`)
- Telegram: chat id or `@username`
- Discord: `channel:<id>` or `user:<id>` (or `<@id>` mention; raw numeric ids are treated as channels)
- Google Chat: `spaces/<spaceId>` or `users/<userId>`
- Slack: `channel:<id>` or `user:<id>` (raw channel id is accepted)
- Mattermost (plugin): `channel:<id>`, `user:<id>`, or `@username` (bare ids are treated as channels)
- Signal: `+E.164`, `group:<id>`, `signal:+E.164`, `signal:group:<id>`, or `username:<name>`/`u:<name>`
- iMessage: handle, `chat_id:<id>`, `chat_guid:<guid>`, or `chat_identifier:<id>`
- Matrix: `@user:server`, `!room:server`, or `#alias:server`
- Microsoft Teams: conversation id (`19:...@thread.tacv2`) or `conversation:<id>` or `user:<aad-object-id>`

Name lookup:

- For supported providers (Discord/Slack/etc), channel names like `Help` or `#help` are resolved via the directory cache.
- On cache miss, OpenClaw will attempt a live directory lookup when the provider supports it.

## [​](https://docs.openclaw.ai/cli/message\#common-flags)  Common flags

- `--channel <name>`
- `--account <id>`
- `--target <dest>` (target channel or user for send/poll/read/etc)
- `--targets <name>` (repeat; broadcast only)
- `--json`
- `--dry-run`
- `--verbose`

## [​](https://docs.openclaw.ai/cli/message\#secretref-behavior)  SecretRef behavior

- `openclaw message` resolves supported channel SecretRefs before running the selected action.
- Resolution is scoped to the active action target when possible:
  - channel-scoped when `--channel` is set (or inferred from prefixed targets like `discord:...`)
  - account-scoped when `--account` is set (channel globals + selected account surfaces)
  - when `--account` is omitted, OpenClaw does not force a `default` account SecretRef scope
- Unresolved SecretRefs on unrelated channels do not block a targeted message action.
- If the selected channel/account SecretRef is unresolved, the command fails closed for that action.

## [​](https://docs.openclaw.ai/cli/message\#actions)  Actions

### [​](https://docs.openclaw.ai/cli/message\#core)  Core

- `send`  - Channels: WhatsApp/Telegram/Discord/Google Chat/Slack/Mattermost (plugin)/Signal/iMessage/Matrix/Microsoft Teams
  - Required: `--target`, plus `--message`, `--media`, or `--presentation`
  - Optional: `--media`, `--presentation`, `--delivery`, `--pin`, `--reply-to`, `--thread-id`, `--gif-playback`, `--force-document`, `--silent`
  - Shared presentation payloads: `--presentation` sends semantic blocks (`text`, `context`, `divider`, `buttons`, `select`) that core renders through the selected channel’s declared capabilities. See [Message Presentation](https://docs.openclaw.ai/plugins/message-presentation).
  - Generic delivery preferences: `--delivery` accepts delivery hints such as `{ "pin": true }`; `--pin` is shorthand for pinned delivery when the channel supports it.
  - Telegram only: `--force-document` (send images and GIFs as documents to avoid Telegram compression)
  - Telegram only: `--thread-id` (forum topic id)
  - Slack only: `--thread-id` (thread timestamp; `--reply-to` uses the same field)
  - Telegram + Discord: `--silent`
  - WhatsApp only: `--gif-playback`; WhatsApp Channels/Newsletters are addressed with their native `@newsletter` JID.
- `poll`  - Channels: WhatsApp/Telegram/Discord/Matrix/Microsoft Teams
  - Required: `--target`, `--poll-question`, `--poll-option` (repeat)
  - Optional: `--poll-multi`
  - Discord only: `--poll-duration-hours`, `--silent`, `--message`
  - Telegram only: `--poll-duration-seconds` (5-600), `--silent`, `--poll-anonymous` / `--poll-public`, `--thread-id`
- `react`  - Channels: Discord/Google Chat/Slack/Telegram/WhatsApp/Signal/Matrix
  - Required: `--message-id`, `--target`
  - Optional: `--emoji`, `--remove`, `--participant`, `--from-me`, `--target-author`, `--target-author-uuid`
  - Note: `--remove` requires `--emoji` (omit `--emoji` to clear own reactions where supported; see /tools/reactions)
  - WhatsApp only: `--participant`, `--from-me`
  - Signal group reactions: `--target-author` or `--target-author-uuid` required
- `reactions`  - Channels: Discord/Google Chat/Slack/Matrix
  - Required: `--message-id`, `--target`
  - Optional: `--limit`
- `read`  - Channels: Discord/Slack/Matrix
  - Required: `--target`
  - Optional: `--limit`, `--message-id`, `--before`, `--after`
  - Slack only: `--message-id` reads a specific Slack message timestamp; combine with `--thread-id` to read an exact thread reply.
  - Discord only: `--around`
- `edit`  - Channels: Discord/Slack/Matrix
  - Required: `--message-id`, `--message`, `--target`
- `delete`  - Channels: Discord/Slack/Telegram/Matrix
  - Required: `--message-id`, `--target`
- `pin` / `unpin`  - Channels: Discord/Slack/Matrix
  - Required: `--message-id`, `--target`
- `pins` (list)  - Channels: Discord/Slack/Matrix
  - Required: `--target`
- `permissions`  - Channels: Discord/Matrix
  - Required: `--target`
  - Matrix only: available when Matrix encryption is enabled and verification actions are allowed
- `search`  - Channels: Discord
  - Required: `--guild-id`, `--query`
  - Optional: `--channel-id`, `--channel-ids` (repeat), `--author-id`, `--author-ids` (repeat), `--limit`

### [​](https://docs.openclaw.ai/cli/message\#threads)  Threads

- `thread create`  - Channels: Discord
  - Required: `--thread-name`, `--target` (channel id)
  - Optional: `--message-id`, `--message`, `--auto-archive-min`
- `thread list`  - Channels: Discord
  - Required: `--guild-id`
  - Optional: `--channel-id`, `--include-archived`, `--before`, `--limit`
- `thread reply`  - Channels: Discord
  - Required: `--target` (thread id), `--message`
  - Optional: `--media`, `--reply-to`

### [​](https://docs.openclaw.ai/cli/message\#emojis)  Emojis

- `emoji list`  - Discord: `--guild-id`
  - Slack: no extra flags
- `emoji upload`  - Channels: Discord
  - Required: `--guild-id`, `--emoji-name`, `--media`
  - Optional: `--role-ids` (repeat)

### [​](https://docs.openclaw.ai/cli/message\#stickers)  Stickers

- `sticker send`  - Channels: Discord
  - Required: `--target`, `--sticker-id` (repeat)
  - Optional: `--message`
- `sticker upload`  - Channels: Discord
  - Required: `--guild-id`, `--sticker-name`, `--sticker-desc`, `--sticker-tags`, `--media`

### [​](https://docs.openclaw.ai/cli/message\#roles-/-channels-/-members-/-voice)  Roles / Channels / Members / Voice

- `role info` (Discord): `--guild-id`
- `role add` / `role remove` (Discord): `--guild-id`, `--user-id`, `--role-id`
- `channel info` (Discord): `--target`
- `channel list` (Discord): `--guild-id`
- `member info` (Discord/Slack): `--user-id` (\+ `--guild-id` for Discord)
- `voice status` (Discord): `--guild-id`, `--user-id`

### [​](https://docs.openclaw.ai/cli/message\#events)  Events

- `event list` (Discord): `--guild-id`
- `event create` (Discord): `--guild-id`, `--event-name`, `--start-time`
  - Optional: `--end-time`, `--desc`, `--channel-id`, `--location`, `--event-type`

### [​](https://docs.openclaw.ai/cli/message\#moderation-discord)  Moderation (Discord)

- `timeout`: `--guild-id`, `--user-id` (optional `--duration-min` or `--until`; omit both to clear timeout)
- `kick`: `--guild-id`, `--user-id` (\+ `--reason`)
- `ban`: `--guild-id`, `--user-id` (\+ `--delete-days`, `--reason`)

  - `timeout` also supports `--reason`

### [​](https://docs.openclaw.ai/cli/message\#broadcast)  Broadcast

- `broadcast`
  - Channels: any configured channel; use `--channel all` to target all providers
  - Required: `--targets <target...>`
  - Optional: `--message`, `--media`, `--dry-run`

## [​](https://docs.openclaw.ai/cli/message\#examples)  Examples

Send a Discord reply:

```
openclaw message send --channel discord \
  --target channel:123 --message "hi" --reply-to 456
```

Send a message with semantic buttons:

```
openclaw message send --channel discord \
  --target channel:123 --message "Choose:" \
  --presentation '{"blocks":[{"type":"buttons","buttons":[{"label":"Approve","value":"approve","style":"success"},{"label":"Decline","value":"decline","style":"danger"}]}]}'
```

Core renders the same `presentation` payload into Discord components, Slack blocks, Telegram inline buttons, Mattermost props, or Teams/Feishu cards depending on channel capability. See [Message Presentation](https://docs.openclaw.ai/plugins/message-presentation) for the full contract and fallback rules.Send a richer presentation payload:

```
openclaw message send --channel googlechat --target spaces/AAA... \
  --message "Choose:" \
  --presentation '{"title":"Deploy approval","tone":"warning","blocks":[{"type":"text","text":"Choose a path"},{"type":"buttons","buttons":[{"label":"Approve","value":"approve"},{"label":"Decline","value":"decline"}]}]}'
```

Create a Discord poll:

```
openclaw message poll --channel discord \
  --target channel:123 \
  --poll-question "Snack?" \
  --poll-option Pizza --poll-option Sushi \
  --poll-multi --poll-duration-hours 48
```

Create a Telegram poll (auto-close in 2 minutes):

```
openclaw message poll --channel telegram \
  --target @mychat \
  --poll-question "Lunch?" \
  --poll-option Pizza --poll-option Sushi \
  --poll-duration-seconds 120 --silent
```

Send a Teams proactive message:

```
openclaw message send --channel msteams \
  --target conversation:19:abc@thread.tacv2 --message "hi"
```

Create a Teams poll:

```
openclaw message poll --channel msteams \
  --target conversation:19:abc@thread.tacv2 \
  --poll-question "Lunch?" \
  --poll-option Pizza --poll-option Sushi
```

React in Slack:

```
openclaw message react --channel slack \
  --target C123 --message-id 456 --emoji "✅"
```

React in a Signal group:

```
openclaw message react --channel signal \
  --target signal:group:abc123 --message-id 1737630212345 \
  --emoji "✅" --target-author-uuid 123e4567-e89b-12d3-a456-426614174000
```

Send Telegram inline buttons through generic presentation:

```
openclaw message send --channel telegram --target @mychat --message "Choose:" \
  --presentation '{"blocks":[{"type":"buttons","buttons":[{"label":"Yes","value":"cmd:yes"},{"label":"No","value":"cmd:no"}]}]}'
```

Send a Teams card through generic presentation:

```
openclaw message send --channel msteams \
  --target conversation:19:abc@thread.tacv2 \
  --presentation '{"title":"Status update","blocks":[{"type":"text","text":"Build completed"}]}'
```

Send a Telegram image as a document to avoid compression:

```
openclaw message send --channel telegram --target @mychat \
  --media ./diagram.png --force-document
```

## [​](https://docs.openclaw.ai/cli/message\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Agent send](https://docs.openclaw.ai/tools/agent-send)

[\`openclaw commitments\`](https://docs.openclaw.ai/cli/commitments) [Models](https://docs.openclaw.ai/cli/models)

Ctrl+I