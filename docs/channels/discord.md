---
source_url: https://docs.openclaw.ai/channels/discord
title: "Discord - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels/discord#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Mainstream messaging

Discord

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick setup](https://docs.openclaw.ai/channels/discord#quick-setup)
- [Recommended: Set up a guild workspace](https://docs.openclaw.ai/channels/discord#recommended-set-up-a-guild-workspace)
- [Runtime model](https://docs.openclaw.ai/channels/discord#runtime-model)
- [Forum channels](https://docs.openclaw.ai/channels/discord#forum-channels)
- [Interactive components](https://docs.openclaw.ai/channels/discord#interactive-components)
- [Access control and routing](https://docs.openclaw.ai/channels/discord#access-control-and-routing)
- [Role-based agent routing](https://docs.openclaw.ai/channels/discord#role-based-agent-routing)
- [Native commands and command auth](https://docs.openclaw.ai/channels/discord#native-commands-and-command-auth)
- [Feature details](https://docs.openclaw.ai/channels/discord#feature-details)
- [Tools and action gates](https://docs.openclaw.ai/channels/discord#tools-and-action-gates)
- [Components v2 UI](https://docs.openclaw.ai/channels/discord#components-v2-ui)
- [Voice](https://docs.openclaw.ai/channels/discord#voice)
- [Voice channels](https://docs.openclaw.ai/channels/discord#voice-channels)
- [Voice messages](https://docs.openclaw.ai/channels/discord#voice-messages)
- [Troubleshooting](https://docs.openclaw.ai/channels/discord#troubleshooting)
- [Configuration reference](https://docs.openclaw.ai/channels/discord#configuration-reference)
- [Safety and operations](https://docs.openclaw.ai/channels/discord#safety-and-operations)
- [Related](https://docs.openclaw.ai/channels/discord#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

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

## [​](https://docs.openclaw.ai/channels/discord\#quick-setup)  Quick setup

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

Your Discord bot token is a secret (like a password). Set it on the machine running OpenClaw before messaging your agent.

```
export DISCORD_BOT_TOKEN="YOUR_BOT_TOKEN"
cat > discord.patch.json5 <<'JSON5'
{
  channels: {
    discord: {
      enabled: true,
      token: { source: "env", provider: "default", id: "DISCORD_BOT_TOKEN" },
    },
  },
}
JSON5
openclaw config patch --file ./discord.patch.json5 --dry-run
openclaw config patch --file ./discord.patch.json5
openclaw gateway
```

If OpenClaw is already running as a background service, restart it via the OpenClaw Mac app or by stopping and restarting the `openclaw gateway run` process.
For managed service installs, run `openclaw gateway install` from a shell where `DISCORD_BOT_TOKEN` is present, or store the variable in `~/.openclaw/.env`, so the service can resolve the env SecretRef after restart.
If your host is blocked or rate-limited by Discord’s startup application lookup, set the Discord application/client ID from the Developer Portal so startup can skip that REST call. Use `channels.discord.applicationId` for the default account, or `channels.discord.accounts.<accountId>.applicationId` when you run multiple Discord bots.

8

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Configure OpenClaw and pair

- Ask your agent

- CLI / config


Chat with your OpenClaw agent on any existing channel (e.g. Telegram) and tell it. If Discord is your first channel, use the CLI / config tab instead.

> “I already set my Discord bot token in config. Please finish Discord setup with User ID `<user_id>` and Server ID `<server_id>`.”

If you prefer file-based config, set:

```
{
  channels: {
    discord: {
      enabled: true,
      token: {
        source: "env",
        provider: "default",
        id: "DISCORD_BOT_TOKEN",
      },
    },
  },
}
```

Env fallback for the default account:

```
DISCORD_BOT_TOKEN=...
```

For scripted or remote setup, write the same JSON5 block with `openclaw config patch --file ./discord.patch.json5 --dry-run` and then rerun without `--dry-run`. Plaintext `token` values are supported. SecretRef values are also supported for `channels.discord.token` across env/file/exec providers. See [Secrets Management](https://docs.openclaw.ai/gateway/secrets).For multiple Discord bots, keep each bot token and application ID under its account. A top-level `channels.discord.applicationId` is inherited by accounts, so only set it there when every account should use the same application ID.

```
{
  channels: {
    discord: {
      enabled: true,
      accounts: {
        personal: {
          token: { source: "env", provider: "default", id: "DISCORD_PERSONAL_TOKEN" },
          applicationId: "111111111111111111",
        },
        work: {
          token: { source: "env", provider: "default", id: "DISCORD_WORK_TOKEN" },
          applicationId: "222222222222222222",
        },
      },
    },
  },
}
```

9

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Approve first DM pairing

Wait until the gateway is running, then DM your bot in Discord. It will respond with a pairing code.

- Ask your agent

- CLI


Send the pairing code to your agent on your existing channel:

> “Approve this Discord pairing code: `<CODE>`”

```
openclaw pairing list discord
openclaw pairing approve discord <CODE>
```

Pairing codes expire after 1 hour.You should now be able to chat with your agent in Discord via DM.

Token resolution is account-aware. Config token values win over env fallback. `DISCORD_BOT_TOKEN` is only used for the default account.
If two enabled Discord accounts resolve to the same bot token, OpenClaw starts only one gateway monitor for that token. A config-sourced token wins over the default env fallback; otherwise the first enabled account wins and the duplicate account is reported disabled.
For advanced outbound calls (message tool/channel actions), an explicit per-call `token` is used for that call. This applies to send and read/probe-style actions (for example read/search/fetch/thread/pins/permissions). Account policy/retry settings still come from the selected account in the active runtime snapshot.

## [​](https://docs.openclaw.ai/channels/discord\#recommended-set-up-a-guild-workspace)  Recommended: Set up a guild workspace

Once DMs are working, you can set up your Discord server as a full workspace where each channel gets its own agent session with its own context. This is recommended for private servers where it’s just you and your bot.

1

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Add your server to the guild allowlist

This enables your agent to respond in any channel on your server, not just DMs.

- Ask your agent

- Config


> “Add my Discord Server ID `<server_id>` to the guild allowlist”

```
{
  channels: {
    discord: {
      groupPolicy: "allowlist",
      guilds: {
        YOUR_SERVER_ID: {
          requireMention: true,
          users: ["YOUR_USER_ID"],
        },
      },
    },
  },
}
```

2

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Allow responses without @mention

By default, your agent only responds in guild channels when @mentioned. For a private server, you probably want it to respond to every message.In guild channels, normal assistant final replies stay private by default. Visible Discord output must be sent explicitly with the `message` tool, so the agent can lurk by default and only post when it decides a channel reply is useful.

- Ask your agent

- Config


> “Allow my agent to respond on this server without having to be @mentioned”

Set `requireMention: false` in your guild config:

```
{
  channels: {
    discord: {
      guilds: {
        YOUR_SERVER_ID: {
          requireMention: false,
        },
      },
    },
  },
}
```

To restore legacy automatic final replies for group/channel rooms, set `messages.groupChat.visibleReplies: "automatic"`.

3

[Navigate to header](https://docs.openclaw.ai/channels/discord#)

Plan for memory in guild channels

By default, long-term memory (MEMORY.md) only loads in DM sessions. Guild channels do not auto-load MEMORY.md.

- Ask your agent

- Manual


> “When I ask questions in Discord channels, use memory\_search or memory\_get if you need long-term context from MEMORY.md.”

If you need shared context in every channel, put the stable instructions in `AGENTS.md` or `USER.md` (they are injected for every session). Keep long-term notes in `MEMORY.md` and access them on demand with memory tools.

Now create some channels on your Discord server and start chatting. Your agent can see the channel name, and each channel gets its own isolated session — so you can set up `#coding`, `#home`, `#research`, or whatever fits your workflow.

## [​](https://docs.openclaw.ai/channels/discord\#runtime-model)  Runtime model

- Gateway owns the Discord connection.
- Reply routing is deterministic: Discord inbound replies back to Discord.
- Discord guild/channel metadata is added to the model prompt as untrusted
context, not as a user-visible reply prefix. If a model copies that envelope
back, OpenClaw strips the copied metadata from outbound replies and from
future replay context.
- By default (`session.dmScope=main`), direct chats share the agent main session (`agent:main:main`).
- Guild channels are isolated session keys (`agent:<agentId>:discord:channel:<channelId>`).
- Group DMs are ignored by default (`channels.discord.dm.groupEnabled=false`).
- Native slash commands run in isolated command sessions (`agent:<agentId>:discord:slash:<userId>`), while still carrying `CommandTargetSessionKey` to the routed conversation session.
- Text-only cron/heartbeat announce delivery to Discord uses the final
assistant-visible answer once. Media and structured component payloads remain
multi-message when the agent emits multiple deliverable payloads.

## [​](https://docs.openclaw.ai/channels/discord\#forum-channels)  Forum channels

Discord forum and media channels only accept thread posts. OpenClaw supports two ways to create them:

- Send a message to the forum parent (`channel:<forumId>`) to auto-create a thread. The thread title uses the first non-empty line of your message.
- Use `openclaw message thread create` to create a thread directly. Do not pass `--message-id` for forum channels.

Example: send to forum parent to create a thread

```
openclaw message send --channel discord --target channel:<forumId> \
  --message "Topic title\nBody of the post"
```

Example: create a forum thread explicitly

```
openclaw message thread create --channel discord --target channel:<forumId> \
  --thread-name "Topic title" --message "Body of the post"
```

Forum parents do not accept Discord components. If you need components, send to the thread itself (`channel:<threadId>`).

## [​](https://docs.openclaw.ai/channels/discord\#interactive-components)  Interactive components

OpenClaw supports Discord components v2 containers for agent messages. Use the message tool with a `components` payload. Interaction results are routed back to the agent as normal inbound messages and follow the existing Discord `replyToMode` settings.Supported blocks:

- `text`, `section`, `separator`, `actions`, `media-gallery`, `file`
- Action rows allow up to 5 buttons or a single select menu
- Select types: `string`, `user`, `role`, `mentionable`, `channel`

By default, components are single use. Set `components.reusable=true` to allow buttons, selects, and forms to be used multiple times until they expire.To restrict who can click a button, set `allowedUsers` on that button (Discord user IDs, tags, or `*`). When configured, unmatched users receive an ephemeral denial.The `/model` and `/models` slash commands open an interactive model picker with provider, model, and compatible runtime dropdowns plus a Submit step. `/models add` is deprecated and now returns a deprecation message instead of registering models from chat. The picker reply is ephemeral and only the invoking user can use it.File attachments:

- `file` blocks must point to an attachment reference (`attachment://<filename>`)
- Provide the attachment via `media`/`path`/`filePath` (single file); use `media-gallery` for multiple files
- Use `filename` to override the upload name when it should match the attachment reference

Modal forms:

- Add `components.modal` with up to 5 fields
- Field types: `text`, `checkbox`, `radio`, `select`, `role-select`, `user-select`
- OpenClaw adds a trigger button automatically

Example:

```
{
  channel: "discord",
  action: "send",
  to: "channel:123456789012345678",
  message: "Optional fallback text",
  components: {
    reusable: true,
    text: "Choose a path",
    blocks: [\
      {\
        type: "actions",\
        buttons: [\
          {\
            label: "Approve",\
            style: "success",\
            allowedUsers: ["123456789012345678"],\
          },\
          { label: "Decline", style: "danger" },\
        ],\
      },\
      {\
        type: "actions",\
        select: {\
          type: "string",\
          placeholder: "Pick an option",\
          options: [\
            { label: "Option A", value: "a" },\
            { label: "Option B", value: "b" },\
          ],\
        },\
      },\
    ],
    modal: {
      title: "Details",
      triggerLabel: "Open form",
      fields: [\
        { type: "text", label: "Requester" },\
        {\
          type: "select",\
          label: "Priority",\
          options: [\
            { label: "Low", value: "low" },\
            { label: "High", value: "high" },\
          ],\
        },\
      ],
    },
  },
}
```

## [​](https://docs.openclaw.ai/channels/discord\#access-control-and-routing)  Access control and routing

- DM policy

- DM access groups

- Guild policy

- Mentions and group DMs


`channels.discord.dmPolicy` controls DM access. `channels.discord.allowFrom` is the canonical DM allowlist.

- `pairing` (default)
- `allowlist`
- `open` (requires `channels.discord.allowFrom` to include `"*"`)
- `disabled`

If DM policy is not open, unknown users are blocked (or prompted for pairing in `pairing` mode).Multi-account precedence:

- `channels.discord.accounts.default.allowFrom` applies only to the `default` account.
- For one account, `allowFrom` takes precedence over legacy `dm.allowFrom`.
- Named accounts inherit `channels.discord.allowFrom` when their own `allowFrom` and legacy `dm.allowFrom` are unset.
- Named accounts do not inherit `channels.discord.accounts.default.allowFrom`.

Legacy `channels.discord.dm.policy` and `channels.discord.dm.allowFrom` still read for compatibility. `openclaw doctor --fix` migrates them to `dmPolicy` and `allowFrom` when it can do so without changing access.DM target format for delivery:

- `user:<id>`
- `<@id>` mention

Bare numeric IDs normally resolve as channel IDs when a channel default is active, but IDs listed in the account’s effective DM `allowFrom` are treated as user DM targets for compatibility.

Discord DMs can use dynamic `accessGroup:<name>` entries in `channels.discord.allowFrom`.Access group names are shared across message channels. Use `type: "message.senders"` for a static group whose members are expressed in each channel’s normal `allowFrom` syntax, or `type: "discord.channelAudience"` when a Discord channel’s current `ViewChannel` audience should define membership dynamically. Shared access-group behavior is documented here: [Access groups](https://docs.openclaw.ai/channels/access-groups).

```
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        "*": ["global-owner-id"],
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
  },
}
```

A Discord text channel has no separate member list. `type: "discord.channelAudience"` models membership as: the DM sender is a member of the configured guild and currently has effective `ViewChannel` permission on the configured channel after role and channel overwrites are applied.Example: allow anyone who can see `#maintainers` to DM the bot, while keeping DMs closed to everyone else.

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

You can mix dynamic and static entries:

```
{
  accessGroups: {
    maintainers: {
      type: "discord.channelAudience",
      guildId: "1456350064065904867",
      channelId: "1456744319972282449",
    },
  },
  channels: {
    discord: {
      dmPolicy: "allowlist",
      allowFrom: ["accessGroup:maintainers", "discord:123456789012345678"],
    },
  },
}
```

Lookups fail closed. If Discord returns `Missing Access`, the member lookup fails, or the channel belongs to a different guild, the DM sender is treated as unauthorized.Enable the Discord Developer Portal **Server Members Intent** for the bot when using channel-audience access groups. DMs do not include guild member state, so OpenClaw resolves the member through Discord REST at authorization time.

Guild handling is controlled by `channels.discord.groupPolicy`:

- `open`
- `allowlist`
- `disabled`

Secure baseline when `channels.discord` exists is `allowlist`.`allowlist` behavior:

- guild must match `channels.discord.guilds` (`id` preferred, slug accepted)
- optional sender allowlists: `users` (stable IDs recommended) and `roles` (role IDs only); if either is configured, senders are allowed when they match `users` OR `roles`
- direct name/tag matching is disabled by default; enable `channels.discord.dangerouslyAllowNameMatching: true` only as break-glass compatibility mode
- names/tags are supported for `users`, but IDs are safer; `openclaw security audit` warns when name/tag entries are used
- if a guild has `channels` configured, non-listed channels are denied
- if a guild has no `channels` block, all channels in that allowlisted guild are allowed

Example:

```
{
  channels: {
    discord: {
      groupPolicy: "allowlist",
      guilds: {
        "123456789012345678": {
          requireMention: true,
          ignoreOtherMentions: true,
          users: ["987654321098765432"],
          roles: ["123456789012345678"],
          channels: {
            general: { allow: true },
            help: { allow: true, requireMention: true },
          },
        },
      },
    },
  },
}
```

If you only set `DISCORD_BOT_TOKEN` and do not create a `channels.discord` block, runtime fallback is `groupPolicy="allowlist"` (with a warning in logs), even if `channels.defaults.groupPolicy` is `open`.

Guild messages are mention-gated by default.Mention detection includes:

- explicit bot mention
- configured mention patterns (`agents.list[].groupChat.mentionPatterns`, fallback `messages.groupChat.mentionPatterns`)
- implicit reply-to-bot behavior in supported cases

When writing outbound Discord messages, use canonical mention syntax: `<@USER_ID>` for users, `<#CHANNEL_ID>` for channels, and `<@&ROLE_ID>` for roles. Do not use the legacy `<@!USER_ID>` nickname mention form.`requireMention` is configured per guild/channel (`channels.discord.guilds...`).
`ignoreOtherMentions` optionally drops messages that mention another user/role but not the bot (excluding @everyone/@here).Group DMs:

- default: ignored (`dm.groupEnabled=false`)
- optional allowlist via `dm.groupChannels` (channel IDs or slugs)

### [​](https://docs.openclaw.ai/channels/discord\#role-based-agent-routing)  Role-based agent routing

Use `bindings[].match.roles` to route Discord guild members to different agents by role ID. Role-based bindings accept role IDs only and are evaluated after peer or parent-peer bindings and before guild-only bindings. If a binding also sets other match fields (for example `peer` \+ `guildId` \+ `roles`), all configured fields must match.

```
{
  bindings: [\
    {\
      agentId: "opus",\
      match: {\
        channel: "discord",\
        guildId: "123456789012345678",\
        roles: ["111111111111111111"],\
      },\
    },\
    {\
      agentId: "sonnet",\
      match: {\
        channel: "discord",\
        guildId: "123456789012345678",\
      },\
    },\
  ],
}
```

## [​](https://docs.openclaw.ai/channels/discord\#native-commands-and-command-auth)  Native commands and command auth

- `commands.native` defaults to `"auto"` and is enabled for Discord.
- Per-channel override: `channels.discord.commands.native`.
- `commands.native=false` explicitly clears previously registered Discord native commands.
- Native command auth uses the same Discord allowlists/policies as normal message handling.
- Commands may still be visible in Discord UI for users who are not authorized; execution still enforces OpenClaw auth and returns “not authorized”.

See [Slash commands](https://docs.openclaw.ai/tools/slash-commands) for command catalog and behavior.Default slash command settings:

- `ephemeral: true`

## [​](https://docs.openclaw.ai/channels/discord\#feature-details)  Feature details

Reply tags and native replies

Discord supports reply tags in agent output:

- `[[reply_to_current]]`
- `[[reply_to:<id>]]`

Controlled by `channels.discord.replyToMode`:

- `off` (default)
- `first`
- `all`
- `batched`

Note: `off` disables implicit reply threading. Explicit `[[reply_to_*]]` tags are still honored.
`first` always attaches the implicit native reply reference to the first outbound Discord message for the turn.
`batched` only attaches Discord’s implicit native reply reference when the
inbound turn was a debounced batch of multiple messages. This is useful
when you want native replies mainly for ambiguous bursty chats, not every
single-message turn.Message IDs are surfaced in context/history so agents can target specific messages.

Live stream preview

OpenClaw can stream draft replies by sending a temporary message and editing it as text arrives. `channels.discord.streaming` takes `off` (default) \| `partial` \| `block` \| `progress`. `progress` maps to `partial` on Discord; `streamMode` is a legacy alias and is auto-migrated.Default stays `off` because Discord preview edits hit rate limits quickly when multiple bots or gateways share an account.

```
{
  channels: {
    discord: {
      streaming: "block",
      draftChunk: {
        minChars: 200,
        maxChars: 800,
        breakPreference: "paragraph",
      },
    },
  },
}
```

- `partial` edits a single preview message as tokens arrive.
- `block` emits draft-sized chunks (use `draftChunk` to tune size and breakpoints, clamped to `textChunkLimit`).
- Media, error, and explicit-reply finals cancel pending preview edits.
- `streaming.preview.toolProgress` (default `true`) controls whether tool/progress updates reuse the preview message.

Preview streaming is text-only; media replies fall back to normal delivery. When `block` streaming is explicitly enabled, OpenClaw skips the preview stream to avoid double-streaming.

History, context, and thread behavior

Guild history context:

- `channels.discord.historyLimit` default `20`
- fallback: `messages.groupChat.historyLimit`
- `0` disables

DM history controls:

- `channels.discord.dmHistoryLimit`
- `channels.discord.dms["<user_id>"].historyLimit`

Thread behavior:

- Discord threads route as channel sessions and inherit parent channel config unless overridden.
- Thread sessions inherit the parent channel’s session-level `/model` selection as a model-only fallback; thread-local `/model` selections still take precedence and parent transcript history is not copied unless transcript inheritance is enabled.
- `channels.discord.thread.inheritParent` (default `false`) opts new auto-threads into seeding from the parent transcript. Per-account overrides live under `channels.discord.accounts.<id>.thread.inheritParent`.
- Message-tool reactions can resolve `user:<id>` DM targets.
- `guilds.<guild>.channels.<channel>.requireMention: false` is preserved during reply-stage activation fallback.

Channel topics are injected as **untrusted** context. Allowlists gate who can trigger the agent, not a full supplemental-context redaction boundary.

Thread-bound sessions for subagents

Discord can bind a thread to a session target so follow-up messages in that thread keep routing to the same session (including subagent sessions).Commands:

- `/focus <target>` bind current/new thread to a subagent/session target
- `/unfocus` remove current thread binding
- `/agents` show active runs and binding state
- `/session idle <duration|off>` inspect/update inactivity auto-unfocus for focused bindings
- `/session max-age <duration|off>` inspect/update hard max age for focused bindings

Config:

```
{
  session: {
    threadBindings: {
      enabled: true,
      idleHours: 24,
      maxAgeHours: 0,
    },
  },
  channels: {
    discord: {
      threadBindings: {
        enabled: true,
        idleHours: 24,
        maxAgeHours: 0,
        spawnSessions: true,
        defaultSpawnContext: "fork",
      },
    },
  },
}
```

Notes:

- `session.threadBindings.*` sets global defaults.
- `channels.discord.threadBindings.*` overrides Discord behavior.
- `spawnSessions` controls auto-create/bind threads for `sessions_spawn({ thread: true })` and ACP thread spawns. Default: `true`.
- `defaultSpawnContext` controls native subagent context for thread-bound spawns. Default: `"fork"`.
- Deprecated `spawnSubagentSessions`/`spawnAcpSessions` keys are migrated by `openclaw doctor --fix`.
- If thread bindings are disabled for an account, `/focus` and related thread binding operations are unavailable.

See [Sub-agents](https://docs.openclaw.ai/tools/subagents), [ACP Agents](https://docs.openclaw.ai/tools/acp-agents), and [Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference).

Persistent ACP channel bindings

For stable “always-on” ACP workspaces, configure top-level typed ACP bindings targeting Discord conversations.Config path:

- `bindings[]` with `type: "acp"` and `match.channel: "discord"`

Example:

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
        channel: "discord",\
        accountId: "default",\
        peer: { kind: "channel", id: "222222222222222222" },\
      },\
      acp: { label: "codex-main" },\
    },\
  ],
  channels: {
    discord: {
      guilds: {
        "111111111111111111": {
          channels: {
            "222222222222222222": {
              requireMention: false,
            },
          },
        },
      },
    },
  },
}
```

Notes:

- `/acp spawn codex --bind here` binds the current channel or thread in place and keeps future messages on the same ACP session. Thread messages inherit the parent channel binding.
- In a bound channel or thread, `/new` and `/reset` reset the same ACP session in place. Temporary thread bindings can override target resolution while active.
- `spawnSessions` gates child thread creation/binding via `--thread auto|here`.

See [ACP Agents](https://docs.openclaw.ai/tools/acp-agents) for binding behavior details.

Reaction notifications

Per-guild reaction notification mode:

- `off`
- `own` (default)
- `all`
- `allowlist` (uses `guilds.<id>.users`)

Reaction events are turned into system events and attached to the routed Discord session.

Ack reactions

`ackReaction` sends an acknowledgement emoji while OpenClaw is processing an inbound message.Resolution order:

- `channels.discord.accounts.<accountId>.ackReaction`
- `channels.discord.ackReaction`
- `messages.ackReaction`
- agent identity emoji fallback (`agents.list[].identity.emoji`, else ”👀”)

Notes:

- Discord accepts unicode emoji or custom emoji names.
- Use `""` to disable the reaction for a channel or account.

Config writes

Channel-initiated config writes are enabled by default.This affects `/config set|unset` flows (when command features are enabled).Disable:

```
{
  channels: {
    discord: {
      configWrites: false,
    },
  },
}
```

Gateway proxy

Route Discord gateway WebSocket traffic and startup REST lookups (application ID + allowlist resolution) through an HTTP(S) proxy with `channels.discord.proxy`.

```
{
  channels: {
    discord: {
      proxy: "http://proxy.example:8080",
    },
  },
}
```

Per-account override:

```
{
  channels: {
    discord: {
      accounts: {
        primary: {
          proxy: "http://proxy.example:8080",
        },
      },
    },
  },
}
```

PluralKit support

Enable PluralKit resolution to map proxied messages to system member identity:

```
{
  channels: {
    discord: {
      pluralkit: {
        enabled: true,
        token: "pk_live_...", // optional; needed for private systems
      },
    },
  },
}
```

Notes:

- allowlists can use `pk:<memberId>`
- member display names are matched by name/slug only when `channels.discord.dangerouslyAllowNameMatching: true`
- lookups use original message ID and are time-window constrained
- if lookup fails, proxied messages are treated as bot messages and dropped unless `allowBots=true`

Outbound mention aliases

Use `mentionAliases` when agents need deterministic outbound mentions for known Discord users. Keys are handles without the leading `@`; values are Discord user IDs. Unknown handles, `@everyone`, `@here`, and mentions inside Markdown code spans are left unchanged.

```
{
  channels: {
    discord: {
      mentionAliases: {
        Vladislava: "123456789012345678",
      },
      accounts: {
        ops: {
          mentionAliases: {
            OpsLead: "234567890123456789",
          },
        },
      },
    },
  },
}
```

Presence configuration

Presence updates are applied when you set a status or activity field, or when you enable auto presence.Status only example:

```
{
  channels: {
    discord: {
      status: "idle",
    },
  },
}
```

Activity example (custom status is the default activity type):

```
{
  channels: {
    discord: {
      activity: "Focus time",
      activityType: 4,
    },
  },
}
```

Streaming example:

```
{
  channels: {
    discord: {
      activity: "Live coding",
      activityType: 1,
      activityUrl: "https://twitch.tv/openclaw",
    },
  },
}
```

Activity type map:

- 0: Playing
- 1: Streaming (requires `activityUrl`)
- 2: Listening
- 3: Watching
- 4: Custom (uses the activity text as the status state; emoji is optional)
- 5: Competing

Auto presence example (runtime health signal):

```
{
  channels: {
    discord: {
      autoPresence: {
        enabled: true,
        intervalMs: 30000,
        minUpdateIntervalMs: 15000,
        exhaustedText: "token exhausted",
      },
    },
  },
}
```

Auto presence maps runtime availability to Discord status: healthy => online, degraded or unknown => idle, exhausted or unavailable => dnd. Optional text overrides:

- `autoPresence.healthyText`
- `autoPresence.degradedText`
- `autoPresence.exhaustedText` (supports `{reason}` placeholder)

Approvals in Discord

Discord supports button-based approval handling in DMs and can optionally post approval prompts in the originating channel.Config path:

- `channels.discord.execApprovals.enabled`
- `channels.discord.execApprovals.approvers` (optional; falls back to `commands.ownerAllowFrom` when possible)
- `channels.discord.execApprovals.target` (`dm` \| `channel` \| `both`, default: `dm`)
- `agentFilter`, `sessionFilter`, `cleanupAfterResolve`

Discord auto-enables native exec approvals when `enabled` is unset or `"auto"` and at least one approver can be resolved, either from `execApprovals.approvers` or from `commands.ownerAllowFrom`. Discord does not infer exec approvers from channel `allowFrom`, legacy `dm.allowFrom`, or direct-message `defaultTo`. Set `enabled: false` to disable Discord as a native approval client explicitly.For sensitive owner-only group commands such as `/diagnostics` and `/export-trajectory`, OpenClaw sends approval prompts and final results privately. It tries Discord DM first when the invoking owner has a Discord owner route; if that is not available, it falls back to the first available owner route from `commands.ownerAllowFrom`, such as Telegram.When `target` is `channel` or `both`, the approval prompt is visible in the channel. Only resolved approvers can use the buttons; other users receive an ephemeral denial. Approval prompts include the command text, so only enable channel delivery in trusted channels. If the channel ID cannot be derived from the session key, OpenClaw falls back to DM delivery.Discord also renders the shared approval buttons used by other chat channels. The native Discord adapter mainly adds approver DM routing and channel fanout.
When those buttons are present, they are the primary approval UX; OpenClaw
should only include a manual `/approve` command when the tool result says
chat approvals are unavailable or manual approval is the only path.
If the Discord native approval runtime is not active, OpenClaw keeps the
local deterministic `/approve <id> <decision>` prompt visible. If the
runtime is active but a native card cannot be delivered to any target,
OpenClaw sends a same-chat fallback notice with the exact `/approve`
command from the pending approval.Gateway auth and approval resolution follow the shared Gateway client contract (`plugin:` IDs resolve through `plugin.approval.resolve`; other IDs through `exec.approval.resolve`). Approvals expire after 30 minutes by default.See [Exec approvals](https://docs.openclaw.ai/tools/exec-approvals).

## [​](https://docs.openclaw.ai/channels/discord\#tools-and-action-gates)  Tools and action gates

Discord message actions include messaging, channel admin, moderation, presence, and metadata actions.Core examples:

- messaging: `sendMessage`, `readMessages`, `editMessage`, `deleteMessage`, `threadReply`
- reactions: `react`, `reactions`, `emojiList`
- moderation: `timeout`, `kick`, `ban`
- presence: `setPresence`

The `event-create` action accepts an optional `image` parameter (URL or local file path) to set the scheduled event cover image.Action gates live under `channels.discord.actions.*`.Default gate behavior:

| Action group | Default |
| --- | --- |
| reactions, messages, threads, pins, polls, search, memberInfo, roleInfo, channelInfo, channels, voiceStatus, events, stickers, emojiUploads, stickerUploads, permissions | enabled |
| roles | disabled |
| moderation | disabled |
| presence | disabled |

## [​](https://docs.openclaw.ai/channels/discord\#components-v2-ui)  Components v2 UI

OpenClaw uses Discord components v2 for exec approvals and cross-context markers. Discord message actions can also accept `components` for custom UI (advanced; requires constructing a component payload via the discord tool), while legacy `embeds` remain available but are not recommended.

- `channels.discord.ui.components.accentColor` sets the accent color used by Discord component containers (hex).
- Set per account with `channels.discord.accounts.<id>.ui.components.accentColor`.
- `embeds` are ignored when components v2 are present.

Example:

```
{
  channels: {
    discord: {
      ui: {
        components: {
          accentColor: "#5865F2",
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/channels/discord\#voice)  Voice

Discord has two distinct voice surfaces: realtime **voice channels** (continuous conversations) and **voice message attachments** (the waveform preview format). The gateway supports both.

### [​](https://docs.openclaw.ai/channels/discord\#voice-channels)  Voice channels

Setup checklist:

1. Enable Message Content Intent in the Discord Developer Portal.
2. Enable Server Members Intent when role/user allowlists are used.
3. Invite the bot with `bot` and `applications.commands` scopes.
4. Grant Connect, Speak, Send Messages, and Read Message History in the target voice channel.
5. Enable native commands (`commands.native` or `channels.discord.commands.native`).
6. Configure `channels.discord.voice`.

Use `/vc join|leave|status` to control sessions. The command uses the account default agent and follows the same allowlist and group policy rules as other Discord commands.

```
/vc join channel:<voice-channel-id>
/vc status
/vc leave
```

Auto-join example:

```
{
  channels: {
    discord: {
      voice: {
        enabled: true,
        model: "openai/gpt-5.4-mini",
        autoJoin: [\
          {\
            guildId: "123456789012345678",\
            channelId: "234567890123456789",\
          },\
        ],
        daveEncryption: true,
        decryptionFailureTolerance: 24,
        connectTimeoutMs: 30000,
        reconnectGraceMs: 15000,
        tts: {
          provider: "openai",
          openai: { voice: "onyx" },
        },
      },
    },
  },
}
```

Notes:

- `voice.tts` overrides `messages.tts` for voice playback only.
- `voice.model` overrides the LLM used for Discord voice channel responses only. Leave it unset to inherit the routed agent model.
- STT uses `tools.media.audio`; `voice.model` does not affect transcription.
- Per-channel Discord `systemPrompt` overrides apply to voice transcript turns for that voice channel.
- Voice transcript turns derive owner status from Discord `allowFrom` (or `dm.allowFrom`); non-owner speakers cannot access owner-only tools (for example `gateway` and `cron`).
- Discord voice is opt-in for text-only configs; set `channels.discord.voice.enabled=true` (or keep an existing `channels.discord.voice` block) to enable `/vc` commands, the voice runtime, and the `GuildVoiceStates` gateway intent.
- `channels.discord.intents.voiceStates` can explicitly override voice-state intent subscription. Leave it unset for the intent to follow effective voice enablement.
- `voice.daveEncryption` and `voice.decryptionFailureTolerance` pass through to `@discordjs/voice` join options.
- `@discordjs/voice` defaults are `daveEncryption=true` and `decryptionFailureTolerance=24` if unset.
- `voice.connectTimeoutMs` controls the initial `@discordjs/voice` Ready wait for `/vc join` and auto-join attempts. Default: `30000`.
- `voice.reconnectGraceMs` controls how long OpenClaw waits for a disconnected voice session to begin reconnecting before destroying it. Default: `15000`.
- OpenClaw also watches receive decrypt failures and auto-recovers by leaving/rejoining the voice channel after repeated failures in a short window.
- If receive logs repeatedly show `DecryptionFailed(UnencryptedWhenPassthroughDisabled)` after updating, collect a dependency report and logs. The bundled `@discordjs/voice` line includes the upstream padding fix from discord.js PR #11449, which closed discord.js issue #11419.

Voice channel pipeline:

- Discord PCM capture is converted to a WAV temp file.
- `tools.media.audio` handles STT, for example `openai/gpt-4o-mini-transcribe`.
- The transcript is sent through Discord ingress and routing while the response LLM runs with a voice-output policy that hides the agent `tts` tool and asks for returned text, because Discord voice owns final TTS playback.
- `voice.model`, when set, overrides only the response LLM for this voice-channel turn.
- `voice.tts` is merged over `messages.tts`; the resulting audio is played in the joined channel.

Credentials are resolved per component: LLM route auth for `voice.model`, STT auth for `tools.media.audio`, and TTS auth for `messages.tts`/`voice.tts`.

### [​](https://docs.openclaw.ai/channels/discord\#voice-messages)  Voice messages

Discord voice messages show a waveform preview and require OGG/Opus audio. OpenClaw generates the waveform automatically, but needs `ffmpeg` and `ffprobe` on the gateway host to inspect and convert.

- Provide a **local file path** (URLs are rejected).
- Omit text content (Discord rejects text + voice message in the same payload).
- Any audio format is accepted; OpenClaw converts to OGG/Opus as needed.

```
message(action="send", channel="discord", target="channel:123", path="/path/to/audio.mp3", asVoice=true)
```

## [​](https://docs.openclaw.ai/channels/discord\#troubleshooting)  Troubleshooting

Used disallowed intents or bot sees no guild messages

- enable Message Content Intent
- enable Server Members Intent when you depend on user/member resolution
- restart gateway after changing intents

Guild messages blocked unexpectedly

- verify `groupPolicy`
- verify guild allowlist under `channels.discord.guilds`
- if guild `channels` map exists, only listed channels are allowed
- verify `requireMention` behavior and mention patterns

Useful checks:

```
openclaw doctor
openclaw channels status --probe
openclaw logs --follow
```

Require mention false but still blocked

Common causes:

- `groupPolicy="allowlist"` without matching guild/channel allowlist
- `requireMention` configured in the wrong place (must be under `channels.discord.guilds` or channel entry)
- sender blocked by guild/channel `users` allowlist

Long-running Discord turns or duplicate replies

Typical logs:

- `Slow listener detected ...`
- `stuck session: sessionKey=agent:...:discord:... state=processing ...`

Discord gateway queue knobs:

- single-account: `channels.discord.eventQueue.listenerTimeout`
- multi-account: `channels.discord.accounts.<accountId>.eventQueue.listenerTimeout`
- this only controls Discord gateway listener work, not agent turn lifetime

Discord does not apply a channel-owned timeout to queued agent turns. Message listeners hand off immediately, and queued Discord runs preserve per-session ordering until the session/tool/runtime lifecycle completes or aborts the work.

```
{
  channels: {
    discord: {
      accounts: {
        default: {
          eventQueue: {
            listenerTimeout: 120000,
          },
        },
      },
    },
  },
}
```

Gateway metadata lookup timeout warnings

OpenClaw fetches Discord `/gateway/bot` metadata before connecting. Transient failures fall back to Discord’s default gateway URL and are rate-limited in logs.Metadata timeout knobs:

- single-account: `channels.discord.gatewayInfoTimeoutMs`
- multi-account: `channels.discord.accounts.<accountId>.gatewayInfoTimeoutMs`
- env fallback when config is unset: `OPENCLAW_DISCORD_GATEWAY_INFO_TIMEOUT_MS`
- default: `30000` (30 seconds), max: `120000`

Gateway READY timeout restarts

OpenClaw waits for Discord’s gateway `READY` event during startup and after runtime reconnects. Multi-account setups with startup staggering can need a longer startup READY window than the default.READY timeout knobs:

- startup single-account: `channels.discord.gatewayReadyTimeoutMs`
- startup multi-account: `channels.discord.accounts.<accountId>.gatewayReadyTimeoutMs`
- startup env fallback when config is unset: `OPENCLAW_DISCORD_READY_TIMEOUT_MS`
- startup default: `15000` (15 seconds), max: `120000`
- runtime single-account: `channels.discord.gatewayRuntimeReadyTimeoutMs`
- runtime multi-account: `channels.discord.accounts.<accountId>.gatewayRuntimeReadyTimeoutMs`
- runtime env fallback when config is unset: `OPENCLAW_DISCORD_RUNTIME_READY_TIMEOUT_MS`
- runtime default: `30000` (30 seconds), max: `120000`

Permissions audit mismatches

`channels status --probe` permission checks only work for numeric channel IDs.If you use slug keys, runtime matching can still work, but probe cannot fully verify permissions.

DM and pairing issues

- DM disabled: `channels.discord.dm.enabled=false`
- DM policy disabled: `channels.discord.dmPolicy="disabled"` (legacy: `channels.discord.dm.policy`)
- awaiting pairing approval in `pairing` mode

Bot to bot loops

By default bot-authored messages are ignored.If you set `channels.discord.allowBots=true`, use strict mention and allowlist rules to avoid loop behavior.
Prefer `channels.discord.allowBots="mentions"` to only accept bot messages that mention the bot.

Voice STT drops with DecryptionFailed(...)

- keep OpenClaw current (`openclaw update`) so the Discord voice receive recovery logic is present
- confirm `channels.discord.voice.daveEncryption=true` (default)
- start from `channels.discord.voice.decryptionFailureTolerance=24` (upstream default) and tune only if needed
- watch logs for:
  - `discord voice: DAVE decrypt failures detected`
  - `discord voice: repeated decrypt failures; attempting rejoin`
- if failures continue after automatic rejoin, collect logs and compare against the upstream DAVE receive history in [discord.js #11419](https://github.com/discordjs/discord.js/issues/11419) and [discord.js #11449](https://github.com/discordjs/discord.js/pull/11449)

## [​](https://docs.openclaw.ai/channels/discord\#configuration-reference)  Configuration reference

Primary reference: [Configuration reference - Discord](https://docs.openclaw.ai/gateway/config-channels#discord).

High-signal Discord fields

- startup/auth: `enabled`, `token`, `accounts.*`, `allowBots`
- policy: `groupPolicy`, `dm.*`, `guilds.*`, `guilds.*.channels.*`
- command: `commands.native`, `commands.useAccessGroups`, `configWrites`, `slashCommand.*`
- event queue: `eventQueue.listenerTimeout` (listener budget), `eventQueue.maxQueueSize`, `eventQueue.maxConcurrency`
- gateway: `gatewayInfoTimeoutMs`, `gatewayReadyTimeoutMs`, `gatewayRuntimeReadyTimeoutMs`
- reply/history: `replyToMode`, `historyLimit`, `dmHistoryLimit`, `dms.*.historyLimit`
- delivery: `textChunkLimit`, `chunkMode`, `maxLinesPerMessage`
- streaming: `streaming` (legacy alias: `streamMode`), `streaming.preview.toolProgress`, `draftChunk`, `blockStreaming`, `blockStreamingCoalesce`
- media/retry: `mediaMaxMb` (caps outbound Discord uploads, default `100MB`), `retry`
- actions: `actions.*`
- presence: `activity`, `status`, `activityType`, `activityUrl`
- UI: `ui.components.accentColor`
- features: `threadBindings`, top-level `bindings[]` (`type: "acp"`), `pluralkit`, `execApprovals`, `intents`, `agentComponents`, `heartbeat`, `responsePrefix`

## [​](https://docs.openclaw.ai/channels/discord\#safety-and-operations)  Safety and operations

- Treat bot tokens as secrets (`DISCORD_BOT_TOKEN` preferred in supervised environments).
- Grant least-privilege Discord permissions.
- If command deploy/state is stale, restart gateway and re-check with `openclaw channels status --probe`.

## [​](https://docs.openclaw.ai/channels/discord\#related)  Related

[**Pairing** \\
\\
Pair a Discord user to the gateway.](https://docs.openclaw.ai/channels/pairing)

[**Groups** \\
\\
Group chat and allowlist behavior.](https://docs.openclaw.ai/channels/groups)

[**Channel routing** \\
\\
Route inbound messages to agents.](https://docs.openclaw.ai/channels/channel-routing)

[**Security** \\
\\
Threat model and hardening.](https://docs.openclaw.ai/gateway/security)

[**Multi-agent routing** \\
\\
Map guilds and channels to agents.](https://docs.openclaw.ai/concepts/multi-agent)

[**Slash commands** \\
\\
Native command behavior.](https://docs.openclaw.ai/tools/slash-commands)

[Chat channels](https://docs.openclaw.ai/channels) [Slack](https://docs.openclaw.ai/channels/slack)

Ctrl+I