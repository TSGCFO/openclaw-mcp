---
source_url: https://docs.openclaw.ai/gateway/config-channels
title: "Configuration \u2014 channels - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/config-channels#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Configuration

Configuration — channels

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Channels](https://docs.openclaw.ai/gateway/config-channels#channels)
- [DM and group access](https://docs.openclaw.ai/gateway/config-channels#dm-and-group-access)
- [Channel model overrides](https://docs.openclaw.ai/gateway/config-channels#channel-model-overrides)
- [Channel defaults and heartbeat](https://docs.openclaw.ai/gateway/config-channels#channel-defaults-and-heartbeat)
- [WhatsApp](https://docs.openclaw.ai/gateway/config-channels#whatsapp)
- [Telegram](https://docs.openclaw.ai/gateway/config-channels#telegram)
- [Discord](https://docs.openclaw.ai/gateway/config-channels#discord)
- [Google Chat](https://docs.openclaw.ai/gateway/config-channels#google-chat)
- [Slack](https://docs.openclaw.ai/gateway/config-channels#slack)
- [Mattermost](https://docs.openclaw.ai/gateway/config-channels#mattermost)
- [Signal](https://docs.openclaw.ai/gateway/config-channels#signal)
- [BlueBubbles](https://docs.openclaw.ai/gateway/config-channels#bluebubbles)
- [iMessage](https://docs.openclaw.ai/gateway/config-channels#imessage)
- [Matrix](https://docs.openclaw.ai/gateway/config-channels#matrix)
- [Microsoft Teams](https://docs.openclaw.ai/gateway/config-channels#microsoft-teams)
- [IRC](https://docs.openclaw.ai/gateway/config-channels#irc)
- [Multi-account (all channels)](https://docs.openclaw.ai/gateway/config-channels#multi-account-all-channels)
- [Other plugin channels](https://docs.openclaw.ai/gateway/config-channels#other-plugin-channels)
- [Group chat mention gating](https://docs.openclaw.ai/gateway/config-channels#group-chat-mention-gating)
- [DM history limits](https://docs.openclaw.ai/gateway/config-channels#dm-history-limits)
- [Self-chat mode](https://docs.openclaw.ai/gateway/config-channels#self-chat-mode)
- [Commands (chat command handling)](https://docs.openclaw.ai/gateway/config-channels#commands-chat-command-handling)
- [Related](https://docs.openclaw.ai/gateway/config-channels#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Per-channel configuration keys under `channels.*`. Covers DM and group access,
multi-account setups, mention gating, and per-channel keys for Slack, Discord,
Telegram, WhatsApp, Matrix, iMessage, and the other bundled channel plugins.For agents, tools, gateway runtime, and other top-level keys, see
[Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference).

## [​](https://docs.openclaw.ai/gateway/config-channels\#channels)  Channels

Each channel starts automatically when its config section exists (unless `enabled: false`).

### [​](https://docs.openclaw.ai/gateway/config-channels\#dm-and-group-access)  DM and group access

All channels support DM policies and group policies:

| DM policy | Behavior |
| --- | --- |
| `pairing` (default) | Unknown senders get a one-time pairing code; owner must approve |
| `allowlist` | Only senders in `allowFrom` (or paired allow store) |
| `open` | Allow all inbound DMs (requires `allowFrom: ["*"]`) |
| `disabled` | Ignore all inbound DMs |

| Group policy | Behavior |
| --- | --- |
| `allowlist` (default) | Only groups matching the configured allowlist |
| `open` | Bypass group allowlists (mention-gating still applies) |
| `disabled` | Block all group/room messages |

`channels.defaults.groupPolicy` sets the default when a provider’s `groupPolicy` is unset.
Pairing codes expire after 1 hour. Pending DM pairing requests are capped at **3 per channel**.
If a provider block is missing entirely (`channels.<provider>` absent), runtime group policy falls back to `allowlist` (fail-closed) with a startup warning.

### [​](https://docs.openclaw.ai/gateway/config-channels\#channel-model-overrides)  Channel model overrides

Use `channels.modelByChannel` to pin specific channel IDs to a model. Values accept `provider/model` or configured model aliases. The channel mapping applies when a session does not already have a model override (for example, set via `/model`).

```
{
  channels: {
    modelByChannel: {
      discord: {
        "123456789012345678": "anthropic/claude-opus-4-6",
      },
      slack: {
        C1234567890: "openai/gpt-4.1",
      },
      telegram: {
        "-1001234567890": "openai/gpt-4.1-mini",
        "-1001234567890:topic:99": "anthropic/claude-sonnet-4-6",
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/gateway/config-channels\#channel-defaults-and-heartbeat)  Channel defaults and heartbeat

Use `channels.defaults` for shared group-policy and heartbeat behavior across providers:

```
{
  channels: {
    defaults: {
      groupPolicy: "allowlist", // open | allowlist | disabled
      contextVisibility: "all", // all | allowlist | allowlist_quote
      heartbeat: {
        showOk: false,
        showAlerts: true,
        useIndicator: true,
      },
    },
  },
}
```

- `channels.defaults.groupPolicy`: fallback group policy when a provider-level `groupPolicy` is unset.
- `channels.defaults.contextVisibility`: default supplemental context visibility mode for all channels. Values: `all` (default, include all quoted/thread/history context), `allowlist` (only include context from allowlisted senders), `allowlist_quote` (same as allowlist but keep explicit quote/reply context). Per-channel override: `channels.<channel>.contextVisibility`.
- `channels.defaults.heartbeat.showOk`: include healthy channel statuses in heartbeat output.
- `channels.defaults.heartbeat.showAlerts`: include degraded/error statuses in heartbeat output.
- `channels.defaults.heartbeat.useIndicator`: render compact indicator-style heartbeat output.

### [​](https://docs.openclaw.ai/gateway/config-channels\#whatsapp)  WhatsApp

WhatsApp runs through the gateway’s web channel (Baileys Web). It starts automatically when a linked session exists.

```
{
  web: {
    whatsapp: {
      keepAliveIntervalMs: 25000,
      connectTimeoutMs: 60000,
      defaultQueryTimeoutMs: 60000,
    },
  },
  channels: {
    whatsapp: {
      dmPolicy: "pairing", // pairing | allowlist | open | disabled
      allowFrom: ["+15555550123", "+447700900123"],
      textChunkLimit: 4000,
      chunkMode: "length", // length | newline
      mediaMaxMb: 50,
      sendReadReceipts: true, // blue ticks (false in self-chat mode)
      groups: {
        "*": { requireMention: true },
      },
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15551234567"],
    },
  },
  web: {
    enabled: true,
    heartbeatSeconds: 60,
    reconnect: {
      initialMs: 2000,
      maxMs: 120000,
      factor: 1.4,
      jitter: 0.2,
      maxAttempts: 0,
    },
  },
}
```

Multi-account WhatsApp

```
{
  channels: {
    whatsapp: {
      accounts: {
        default: {},
        personal: {},
        biz: {
          // authDir: "~/.openclaw/credentials/whatsapp/biz",
        },
      },
    },
  },
}
```

- Outbound commands default to account `default` if present; otherwise the first configured account id (sorted).
- Optional `channels.whatsapp.defaultAccount` overrides that fallback default account selection when it matches a configured account id.
- Legacy single-account Baileys auth dir is migrated by `openclaw doctor` into `whatsapp/default`.
- Per-account overrides: `channels.whatsapp.accounts.<id>.sendReadReceipts`, `channels.whatsapp.accounts.<id>.dmPolicy`, `channels.whatsapp.accounts.<id>.allowFrom`.

### [​](https://docs.openclaw.ai/gateway/config-channels\#telegram)  Telegram

```
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "your-bot-token",
      dmPolicy: "pairing",
      allowFrom: ["tg:123456789"],
      groups: {
        "*": { requireMention: true },
        "-1001234567890": {
          allowFrom: ["@admin"],
          systemPrompt: "Keep answers brief.",
          topics: {
            "99": {
              requireMention: false,
              skills: ["search"],
              systemPrompt: "Stay on topic.",
            },
          },
        },
      },
      customCommands: [\
        { command: "backup", description: "Git backup" },\
        { command: "generate", description: "Create an image" },\
      ],
      historyLimit: 50,
      replyToMode: "first", // off | first | all | batched
      linkPreview: true,
      streaming: "partial", // off | partial | block | progress (default: off; opt in explicitly to avoid preview-edit rate limits)
      actions: { reactions: true, sendMessage: true },
      reactionNotifications: "own", // off | own | all
      mediaMaxMb: 100,
      retry: {
        attempts: 3,
        minDelayMs: 400,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
      network: {
        autoSelectFamily: true,
        dnsResultOrder: "ipv4first",
      },
      apiRoot: "https://api.telegram.org",
      proxy: "socks5://localhost:9050",
      webhookUrl: "https://example.com/telegram-webhook",
      webhookSecret: "secret",
      webhookPath: "/telegram-webhook",
    },
  },
}
```

- Bot token: `channels.telegram.botToken` or `channels.telegram.tokenFile` (regular file only; symlinks rejected), with `TELEGRAM_BOT_TOKEN` as fallback for the default account.
- `apiRoot` is the Telegram Bot API root only. Use `https://api.telegram.org` or your self-hosted/proxy root, not `https://api.telegram.org/bot<TOKEN>`; `openclaw doctor --fix` removes an accidental trailing `/bot<TOKEN>` suffix.
- Optional `channels.telegram.defaultAccount` overrides default account selection when it matches a configured account id.
- In multi-account setups (2+ account ids), set an explicit default (`channels.telegram.defaultAccount` or `channels.telegram.accounts.default`) to avoid fallback routing; `openclaw doctor` warns when this is missing or invalid.
- `configWrites: false` blocks Telegram-initiated config writes (supergroup ID migrations, `/config set|unset`).
- Top-level `bindings[]` entries with `type: "acp"` configure persistent ACP bindings for forum topics (use canonical `chatId:topic:topicId` in `match.peer.id`). Field semantics are shared in [ACP Agents](https://docs.openclaw.ai/tools/acp-agents#persistent-channel-bindings).
- Telegram stream previews use `sendMessage` \+ `editMessageText` (works in direct and group chats).
- Retry policy: see [Retry policy](https://docs.openclaw.ai/concepts/retry).

### [​](https://docs.openclaw.ai/gateway/config-channels\#discord)  Discord

```
{
  channels: {
    discord: {
      enabled: true,
      token: "your-bot-token",
      mediaMaxMb: 100,
      allowBots: false,
      actions: {
        reactions: true,
        stickers: true,
        polls: true,
        permissions: true,
        messages: true,
        threads: true,
        pins: true,
        search: true,
        memberInfo: true,
        roleInfo: true,
        roles: false,
        channelInfo: true,
        voiceStatus: true,
        events: true,
        moderation: false,
      },
      replyToMode: "off", // off | first | all | batched
      dmPolicy: "pairing",
      allowFrom: ["1234567890", "123456789012345678"],
      dm: { enabled: true, groupEnabled: false, groupChannels: ["openclaw-dm"] },
      guilds: {
        "123456789012345678": {
          slug: "friends-of-openclaw",
          requireMention: false,
          ignoreOtherMentions: true,
          reactionNotifications: "own",
          users: ["987654321098765432"],
          channels: {
            general: { allow: true },
            help: {
              allow: true,
              requireMention: true,
              users: ["987654321098765432"],
              skills: ["docs"],
              systemPrompt: "Short answers only.",
            },
          },
        },
      },
      historyLimit: 20,
      textChunkLimit: 2000,
      chunkMode: "length", // length | newline
      streaming: "off", // off | partial | block | progress (progress maps to partial on Discord)
      maxLinesPerMessage: 17,
      ui: {
        components: {
          accentColor: "#5865F2",
        },
      },
      threadBindings: {
        enabled: true,
        idleHours: 24,
        maxAgeHours: 0,
        spawnSessions: true,
        defaultSpawnContext: "fork",
      },
      voice: {
        enabled: true,
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
          openai: { voice: "alloy" },
        },
      },
      execApprovals: {
        enabled: "auto", // true | false | "auto"
        approvers: ["987654321098765432"],
        agentFilter: ["default"],
        sessionFilter: ["discord:"],
        target: "dm", // dm | channel | both
        cleanupAfterResolve: false,
      },
      retry: {
        attempts: 3,
        minDelayMs: 500,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
    },
  },
}
```

- Token: `channels.discord.token`, with `DISCORD_BOT_TOKEN` as fallback for the default account.
- Direct outbound calls that provide an explicit Discord `token` use that token for the call; account retry/policy settings still come from the selected account in the active runtime snapshot.
- Optional `channels.discord.defaultAccount` overrides default account selection when it matches a configured account id.
- Use `user:<id>` (DM) or `channel:<id>` (guild channel) for delivery targets; bare numeric IDs are rejected.
- Guild slugs are lowercase with spaces replaced by `-`; channel keys use the slugged name (no `#`). Prefer guild IDs.
- Bot-authored messages are ignored by default. `allowBots: true` enables them; use `allowBots: "mentions"` to only accept bot messages that mention the bot (own messages still filtered).
- `channels.discord.guilds.<id>.ignoreOtherMentions` (and channel overrides) drops messages that mention another user or role but not the bot (excluding @everyone/@here).
- `channels.discord.mentionAliases` maps stable outbound `@handle` text to Discord user IDs before sending, so known teammates can be mentioned deterministically even when the transient directory cache is empty. Per-account overrides live under `channels.discord.accounts.<accountId>.mentionAliases`.
- `maxLinesPerMessage` (default 17) splits tall messages even when under 2000 chars.
- `channels.discord.threadBindings`controls Discord thread-bound routing:

  - `enabled`: Discord override for thread-bound session features (`/focus`, `/unfocus`, `/agents`, `/session idle`, `/session max-age`, and bound delivery/routing)
  - `idleHours`: Discord override for inactivity auto-unfocus in hours (`0` disables)
  - `maxAgeHours`: Discord override for hard max age in hours (`0` disables)
  - `spawnSessions`: switch for `sessions_spawn({ thread: true })` and ACP thread-spawn auto thread creation/binding (default: `true`)
  - `defaultSpawnContext`: native subagent context for thread-bound spawns (`"fork"` by default)
- Top-level `bindings[]` entries with `type: "acp"` configure persistent ACP bindings for channels and threads (use channel/thread id in `match.peer.id`). Field semantics are shared in [ACP Agents](https://docs.openclaw.ai/tools/acp-agents#persistent-channel-bindings).
- `channels.discord.ui.components.accentColor` sets the accent color for Discord components v2 containers.
- `channels.discord.voice` enables Discord voice channel conversations and optional auto-join + LLM + TTS overrides. Text-only Discord configs leave voice off by default; set `channels.discord.voice.enabled=true` to opt in.
- `channels.discord.voice.model` optionally overrides the LLM model used for Discord voice channel responses.
- `channels.discord.voice.daveEncryption` and `channels.discord.voice.decryptionFailureTolerance` pass through to `@discordjs/voice` DAVE options (`true` and `24` by default).
- `channels.discord.voice.connectTimeoutMs` controls the initial `@discordjs/voice` Ready wait for `/vc join` and auto-join attempts (`30000` by default).
- `channels.discord.voice.reconnectGraceMs` controls how long a disconnected voice session may take to enter reconnect signalling before OpenClaw destroys it (`15000` by default).
- OpenClaw additionally attempts voice receive recovery by leaving/rejoining a voice session after repeated decrypt failures.
- `channels.discord.streaming` is the canonical stream mode key. Legacy `streamMode` and boolean `streaming` values are auto-migrated.
- `channels.discord.autoPresence` maps runtime availability to bot presence (healthy => online, degraded => idle, exhausted => dnd) and allows optional status text overrides.
- `channels.discord.dangerouslyAllowNameMatching` re-enables mutable name/tag matching (break-glass compatibility mode).
- `channels.discord.execApprovals`: Discord-native exec approval delivery and approver authorization.

  - `enabled`: `true`, `false`, or `"auto"` (default). In auto mode, exec approvals activate when approvers can be resolved from `approvers` or `commands.ownerAllowFrom`.
  - `approvers`: Discord user IDs allowed to approve exec requests. Falls back to `commands.ownerAllowFrom` when omitted.
  - `agentFilter`: optional agent ID allowlist. Omit to forward approvals for all agents.
  - `sessionFilter`: optional session key patterns (substring or regex).
  - `target`: where to send approval prompts. `"dm"` (default) sends to approver DMs, `"channel"` sends to the originating channel, `"both"` sends to both. When target includes `"channel"`, buttons are only usable by resolved approvers.
  - `cleanupAfterResolve`: when `true`, deletes approval DMs after approval, denial, or timeout.

**Reaction notification modes:**`off` (none), `own` (bot’s messages, default), `all` (all messages), `allowlist` (from `guilds.<id>.users` on all messages).

### [​](https://docs.openclaw.ai/gateway/config-channels\#google-chat)  Google Chat

```
{
  channels: {
    googlechat: {
      enabled: true,
      serviceAccountFile: "/path/to/service-account.json",
      audienceType: "app-url", // app-url | project-number
      audience: "https://gateway.example.com/googlechat",
      webhookPath: "/googlechat",
      botUser: "users/1234567890",
      dm: {
        enabled: true,
        policy: "pairing",
        allowFrom: ["users/1234567890"],
      },
      groupPolicy: "allowlist",
      groups: {
        "spaces/AAAA": { allow: true, requireMention: true },
      },
      actions: { reactions: true },
      typingIndicator: "message",
      mediaMaxMb: 20,
    },
  },
}
```

- Service account JSON: inline (`serviceAccount`) or file-based (`serviceAccountFile`).
- Service account SecretRef is also supported (`serviceAccountRef`).
- Env fallbacks: `GOOGLE_CHAT_SERVICE_ACCOUNT` or `GOOGLE_CHAT_SERVICE_ACCOUNT_FILE`.
- Use `spaces/<spaceId>` or `users/<userId>` for delivery targets.
- `channels.googlechat.dangerouslyAllowNameMatching` re-enables mutable email principal matching (break-glass compatibility mode).

### [​](https://docs.openclaw.ai/gateway/config-channels\#slack)  Slack

```
{
  channels: {
    slack: {
      enabled: true,
      botToken: "xoxb-...",
      appToken: "xapp-...",
      socketMode: {
        clientPingTimeout: 15000,
        serverPingTimeout: 30000,
        pingPongLoggingEnabled: false,
      },
      dmPolicy: "pairing",
      allowFrom: ["U123", "U456", "*"],
      dm: { enabled: true, groupEnabled: false, groupChannels: ["G123"] },
      channels: {
        C123: { allow: true, requireMention: true, allowBots: false },
        "#general": {
          allow: true,
          requireMention: true,
          allowBots: false,
          users: ["U123"],
          skills: ["docs"],
          systemPrompt: "Short answers only.",
        },
      },
      historyLimit: 50,
      allowBots: false,
      reactionNotifications: "own",
      reactionAllowlist: ["U123"],
      replyToMode: "off", // off | first | all | batched
      thread: {
        historyScope: "thread", // thread | channel
        inheritParent: false,
      },
      actions: {
        reactions: true,
        messages: true,
        pins: true,
        memberInfo: true,
        emojiList: true,
      },
      slashCommand: {
        enabled: true,
        name: "openclaw",
        sessionPrefix: "slack:slash",
        ephemeral: true,
      },
      typingReaction: "hourglass_flowing_sand",
      textChunkLimit: 4000,
      chunkMode: "length",
      streaming: {
        mode: "partial", // off | partial | block | progress
        nativeTransport: true, // use Slack native streaming API when mode=partial
      },
      mediaMaxMb: 20,
      execApprovals: {
        enabled: "auto", // true | false | "auto"
        approvers: ["U123"],
        agentFilter: ["default"],
        sessionFilter: ["slack:"],
        target: "dm", // dm | channel | both
      },
    },
  },
}
```

- **Socket mode** requires both `botToken` and `appToken` (`SLACK_BOT_TOKEN` \+ `SLACK_APP_TOKEN` for default account env fallback).
- **HTTP mode** requires `botToken` plus `signingSecret` (at root or per-account).
- `socketMode` passes Slack SDK Socket Mode transport tuning through to the public Bolt receiver API. Use it only when investigating ping/pong timeout or stale websocket behavior.
- `botToken`, `appToken`, `signingSecret`, and `userToken` accept plaintext
strings or SecretRef objects.
- Slack account snapshots expose per-credential source/status fields such as
`botTokenSource`, `botTokenStatus`, `appTokenStatus`, and, in HTTP mode,
`signingSecretStatus`. `configured_unavailable` means the account is
configured through SecretRef but the current command/runtime path could not
resolve the secret value.
- `configWrites: false` blocks Slack-initiated config writes.
- Optional `channels.slack.defaultAccount` overrides default account selection when it matches a configured account id.
- `channels.slack.streaming.mode` is the canonical Slack stream mode key. `channels.slack.streaming.nativeTransport` controls Slack’s native streaming transport. Legacy `streamMode`, boolean `streaming`, and `nativeStreaming` values are auto-migrated.
- Use `user:<id>` (DM) or `channel:<id>` for delivery targets.

**Reaction notification modes:**`off`, `own` (default), `all`, `allowlist` (from `reactionAllowlist`).**Thread session isolation:**`thread.historyScope` is per-thread (default) or shared across channel. `thread.inheritParent` copies parent channel transcript to new threads.

- Slack native streaming plus the Slack assistant-style “is typing…” thread status require a reply thread target. Top-level DMs stay off-thread by default, so they can still stream through Slack draft post-and-edit previews instead of showing the thread-style native stream/status preview.
- `typingReaction` adds a temporary reaction to the inbound Slack message while a reply is running, then removes it on completion. Use a Slack emoji shortcode such as `"hourglass_flowing_sand"`.
- `channels.slack.execApprovals`: Slack-native exec approval delivery and approver authorization. Same schema as Discord: `enabled` (`true`/`false`/`"auto"`), `approvers` (Slack user IDs), `agentFilter`, `sessionFilter`, and `target` (`"dm"`, `"channel"`, or `"both"`).

| Action group | Default | Notes |
| --- | --- | --- |
| reactions | enabled | React + list reactions |
| messages | enabled | Read/send/edit/delete |
| pins | enabled | Pin/unpin/list |
| memberInfo | enabled | Member info |
| emojiList | enabled | Custom emoji list |

### [​](https://docs.openclaw.ai/gateway/config-channels\#mattermost)  Mattermost

Mattermost ships as a bundled plugin in current OpenClaw releases. Older or
custom builds can install a current npm package with
`openclaw plugins install @openclaw/mattermost`. Check
[npmjs.com/package/@openclaw/mattermost](https://www.npmjs.com/package/@openclaw/mattermost)
for the current dist-tags before pinning a version.

```
{
  channels: {
    mattermost: {
      enabled: true,
      botToken: "mm-token",
      baseUrl: "https://chat.example.com",
      dmPolicy: "pairing",
      chatmode: "oncall", // oncall | onmessage | onchar
      oncharPrefixes: [">", "!"],
      groups: {
        "*": { requireMention: true },
        "team-channel-id": { requireMention: false },
      },
      commands: {
        native: true, // opt-in
        nativeSkills: true,
        callbackPath: "/api/channels/mattermost/command",
        // Optional explicit URL for reverse-proxy/public deployments
        callbackUrl: "https://gateway.example.com/api/channels/mattermost/command",
      },
      textChunkLimit: 4000,
      chunkMode: "length",
    },
  },
}
```

Chat modes: `oncall` (respond on @-mention, default), `onmessage` (every message), `onchar` (messages starting with trigger prefix).When Mattermost native commands are enabled:

- `commands.callbackPath` must be a path (for example `/api/channels/mattermost/command`), not a full URL.
- `commands.callbackUrl` must resolve to the OpenClaw gateway endpoint and be reachable from the Mattermost server.
- Native slash callbacks are authenticated with the per-command tokens returned
by Mattermost during slash command registration. If registration fails or no
commands are activated, OpenClaw rejects callbacks with
`Unauthorized: invalid command token.`
- For private/tailnet/internal callback hosts, Mattermost may require
`ServiceSettings.AllowedUntrustedInternalConnections` to include the callback host/domain.
Use host/domain values, not full URLs.
- `channels.mattermost.configWrites`: allow or deny Mattermost-initiated config writes.
- `channels.mattermost.requireMention`: require `@mention` before replying in channels.
- `channels.mattermost.groups.<channelId>.requireMention`: per-channel mention-gating override (`"*"` for default).
- Optional `channels.mattermost.defaultAccount` overrides default account selection when it matches a configured account id.

### [​](https://docs.openclaw.ai/gateway/config-channels\#signal)  Signal

```
{
  channels: {
    signal: {
      enabled: true,
      account: "+15555550123", // optional account binding
      dmPolicy: "pairing",
      allowFrom: ["+15551234567", "uuid:123e4567-e89b-12d3-a456-426614174000"],
      configWrites: true,
      reactionNotifications: "own", // off | own | all | allowlist
      reactionAllowlist: ["+15551234567", "uuid:123e4567-e89b-12d3-a456-426614174000"],
      historyLimit: 50,
    },
  },
}
```

**Reaction notification modes:**`off`, `own` (default), `all`, `allowlist` (from `reactionAllowlist`).

- `channels.signal.account`: pin channel startup to a specific Signal account identity.
- `channels.signal.configWrites`: allow or deny Signal-initiated config writes.
- Optional `channels.signal.defaultAccount` overrides default account selection when it matches a configured account id.

### [​](https://docs.openclaw.ai/gateway/config-channels\#bluebubbles)  BlueBubbles

BlueBubbles is the recommended iMessage path (plugin-backed, configured under `channels.bluebubbles`).

```
{
  channels: {
    bluebubbles: {
      enabled: true,
      dmPolicy: "pairing",
      // serverUrl, password, webhookPath, group controls, and advanced actions:
      // see /channels/bluebubbles
    },
  },
}
```

- Core key paths covered here: `channels.bluebubbles`, `channels.bluebubbles.dmPolicy`.
- Optional `channels.bluebubbles.defaultAccount` overrides default account selection when it matches a configured account id.
- Top-level `bindings[]` entries with `type: "acp"` can bind BlueBubbles conversations to persistent ACP sessions. Use a BlueBubbles handle or target string (`chat_id:*`, `chat_guid:*`, `chat_identifier:*`) in `match.peer.id`. Shared field semantics: [ACP Agents](https://docs.openclaw.ai/tools/acp-agents#persistent-channel-bindings).
- Full BlueBubbles channel configuration is documented in [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles).

### [​](https://docs.openclaw.ai/gateway/config-channels\#imessage)  iMessage

OpenClaw spawns `imsg rpc` (JSON-RPC over stdio). No daemon or port required.

```
{
  channels: {
    imessage: {
      enabled: true,
      cliPath: "imsg",
      dbPath: "~/Library/Messages/chat.db",
      remoteHost: "user@gateway-host",
      dmPolicy: "pairing",
      allowFrom: ["+15555550123", "user@example.com", "chat_id:123"],
      historyLimit: 50,
      includeAttachments: false,
      attachmentRoots: ["/Users/*/Library/Messages/Attachments"],
      remoteAttachmentRoots: ["/Users/*/Library/Messages/Attachments"],
      mediaMaxMb: 16,
      service: "auto",
      region: "US",
    },
  },
}
```

- Optional `channels.imessage.defaultAccount` overrides default account selection when it matches a configured account id.
- Requires Full Disk Access to the Messages DB.
- Prefer `chat_id:<id>` targets. Use `imsg chats --limit 20` to list chats.
- `cliPath` can point to an SSH wrapper; set `remoteHost` (`host` or `user@host`) for SCP attachment fetching.
- `attachmentRoots` and `remoteAttachmentRoots` restrict inbound attachment paths (default: `/Users/*/Library/Messages/Attachments`).
- SCP uses strict host-key checking, so ensure the relay host key already exists in `~/.ssh/known_hosts`.
- `channels.imessage.configWrites`: allow or deny iMessage-initiated config writes.
- Top-level `bindings[]` entries with `type: "acp"` can bind iMessage conversations to persistent ACP sessions. Use a normalized handle or explicit chat target (`chat_id:*`, `chat_guid:*`, `chat_identifier:*`) in `match.peer.id`. Shared field semantics: [ACP Agents](https://docs.openclaw.ai/tools/acp-agents#persistent-channel-bindings).

iMessage SSH wrapper example

```
#!/usr/bin/env bash
exec ssh -T gateway-host imsg "$@"
```

### [​](https://docs.openclaw.ai/gateway/config-channels\#matrix)  Matrix

Matrix is plugin-backed and configured under `channels.matrix`.

```
{
  channels: {
    matrix: {
      enabled: true,
      homeserver: "https://matrix.example.org",
      accessToken: "syt_bot_xxx",
      proxy: "http://127.0.0.1:7890",
      encryption: true,
      initialSyncLimit: 20,
      defaultAccount: "ops",
      accounts: {
        ops: {
          name: "Ops",
          userId: "@ops:example.org",
          accessToken: "syt_ops_xxx",
        },
        alerts: {
          userId: "@alerts:example.org",
          password: "secret",
          proxy: "http://127.0.0.1:7891",
        },
      },
    },
  },
}
```

- Token auth uses `accessToken`; password auth uses `userId` \+ `password`.
- `channels.matrix.proxy` routes Matrix HTTP traffic through an explicit HTTP(S) proxy. Named accounts can override it with `channels.matrix.accounts.<id>.proxy`.
- `channels.matrix.network.dangerouslyAllowPrivateNetwork` allows private/internal homeservers. `proxy` and this network opt-in are independent controls.
- `channels.matrix.defaultAccount` selects the preferred account in multi-account setups.
- `channels.matrix.autoJoin` defaults to `off`, so invited rooms and fresh DM-style invites are ignored until you set `autoJoin: "allowlist"` with `autoJoinAllowlist` or `autoJoin: "always"`.
- `channels.matrix.execApprovals`: Matrix-native exec approval delivery and approver authorization.

  - `enabled`: `true`, `false`, or `"auto"` (default). In auto mode, exec approvals activate when approvers can be resolved from `approvers` or `commands.ownerAllowFrom`.
  - `approvers`: Matrix user IDs (e.g. `@owner:example.org`) allowed to approve exec requests.
  - `agentFilter`: optional agent ID allowlist. Omit to forward approvals for all agents.
  - `sessionFilter`: optional session key patterns (substring or regex).
  - `target`: where to send approval prompts. `"dm"` (default), `"channel"` (originating room), or `"both"`.
  - Per-account overrides: `channels.matrix.accounts.<id>.execApprovals`.
- `channels.matrix.dm.sessionScope` controls how Matrix DMs group into sessions: `per-user` (default) shares by routed peer, while `per-room` isolates each DM room.
- Matrix status probes and live directory lookups use the same proxy policy as runtime traffic.
- Full Matrix configuration, targeting rules, and setup examples are documented in [Matrix](https://docs.openclaw.ai/channels/matrix).

### [​](https://docs.openclaw.ai/gateway/config-channels\#microsoft-teams)  Microsoft Teams

Microsoft Teams is plugin-backed and configured under `channels.msteams`.

```
{
  channels: {
    msteams: {
      enabled: true,
      configWrites: true,
      // appId, appPassword, tenantId, webhook, team/channel policies:
      // see /channels/msteams
    },
  },
}
```

- Core key paths covered here: `channels.msteams`, `channels.msteams.configWrites`.
- Full Teams config (credentials, webhook, DM/group policy, per-team/per-channel overrides) is documented in [Microsoft Teams](https://docs.openclaw.ai/channels/msteams).

### [​](https://docs.openclaw.ai/gateway/config-channels\#irc)  IRC

IRC is plugin-backed and configured under `channels.irc`.

```
{
  channels: {
    irc: {
      enabled: true,
      dmPolicy: "pairing",
      configWrites: true,
      nickserv: {
        enabled: true,
        service: "NickServ",
        password: "${IRC_NICKSERV_PASSWORD}",
        register: false,
        registerEmail: "bot@example.com",
      },
    },
  },
}
```

- Core key paths covered here: `channels.irc`, `channels.irc.dmPolicy`, `channels.irc.configWrites`, `channels.irc.nickserv.*`.
- Optional `channels.irc.defaultAccount` overrides default account selection when it matches a configured account id.
- Full IRC channel configuration (host/port/TLS/channels/allowlists/mention gating) is documented in [IRC](https://docs.openclaw.ai/channels/irc).

### [​](https://docs.openclaw.ai/gateway/config-channels\#multi-account-all-channels)  Multi-account (all channels)

Run multiple accounts per channel (each with its own `accountId`):

```
{
  channels: {
    telegram: {
      accounts: {
        default: {
          name: "Primary bot",
          botToken: "123456:ABC...",
        },
        alerts: {
          name: "Alerts bot",
          botToken: "987654:XYZ...",
        },
      },
    },
  },
}
```

- `default` is used when `accountId` is omitted (CLI + routing).
- Env tokens only apply to the **default** account.
- Base channel settings apply to all accounts unless overridden per account.
- Use `bindings[].match.accountId` to route each account to a different agent.
- If you add a non-default account via `openclaw channels add` (or channel onboarding) while still on a single-account top-level channel config, OpenClaw promotes account-scoped top-level single-account values into the channel account map first so the original account keeps working. Most channels move them into `channels.<channel>.accounts.default`; Matrix can preserve an existing matching named/default target instead.
- Existing channel-only bindings (no `accountId`) keep matching the default account; account-scoped bindings remain optional.
- `openclaw doctor --fix` also repairs mixed shapes by moving account-scoped top-level single-account values into the promoted account chosen for that channel. Most channels use `accounts.default`; Matrix can preserve an existing matching named/default target instead.

### [​](https://docs.openclaw.ai/gateway/config-channels\#other-plugin-channels)  Other plugin channels

Many plugin channels are configured as `channels.<id>` and documented in their dedicated channel pages (for example Feishu, Matrix, LINE, Nostr, Zalo, Nextcloud Talk, Synology Chat, and Twitch).
See the full channel index: [Channels](https://docs.openclaw.ai/channels).

### [​](https://docs.openclaw.ai/gateway/config-channels\#group-chat-mention-gating)  Group chat mention gating

Group messages default to **require mention** (metadata mention or safe regex patterns). Applies to WhatsApp, Telegram, Discord, Google Chat, and iMessage group chats.Visible replies are controlled separately. Group/channel rooms default to `messages.groupChat.visibleReplies: "message_tool"`: OpenClaw still processes the turn, but normal final replies stay private and visible room output requires `message(action=send)`. Set `"automatic"` only when you want the legacy behavior where normal replies are posted back to the room. To apply the same tool-only visible-reply behavior to direct chats too, set `messages.visibleReplies: "message_tool"`; the Codex harness also uses that tool-only behavior as its unset direct-chat default.If the message tool is unavailable under the active tool policy, OpenClaw falls back to automatic visible replies instead of silently suppressing the response. `openclaw doctor` warns about this mismatch.The gateway hot-reloads `messages` config after the file is saved. Restart only when file watching or config reload is disabled in the deployment.**Mention types:**

- **Metadata mentions**: Native platform @-mentions. Ignored in WhatsApp self-chat mode.
- **Text patterns**: Safe regex patterns in `agents.list[].groupChat.mentionPatterns`. Invalid patterns and unsafe nested repetition are ignored.
- Mention gating is enforced only when detection is possible (native mentions or at least one pattern).

```
{
  messages: {
    visibleReplies: "automatic", // global default for direct/source chats; Codex harness defaults unset direct chats to message_tool
    groupChat: {
      historyLimit: 50,
      visibleReplies: "message_tool", // default; use "automatic" for legacy final replies
    },
  },
  agents: {
    list: [{ id: "main", groupChat: { mentionPatterns: ["@openclaw", "openclaw"] } }],
  },
}
```

`messages.groupChat.historyLimit` sets the global default. Channels can override with `channels.<channel>.historyLimit` (or per-account). Set `0` to disable.`messages.visibleReplies` is the global source-turn default; `messages.groupChat.visibleReplies` overrides it for group/channel source turns. When `messages.visibleReplies` is unset, a harness can provide its own direct/source default; the Codex harness defaults to `message_tool`. Channel allowlists and mention gating still decide whether a turn is processed.

#### [​](https://docs.openclaw.ai/gateway/config-channels\#dm-history-limits)  DM history limits

```
{
  channels: {
    telegram: {
      dmHistoryLimit: 30,
      dms: {
        "123456789": { historyLimit: 50 },
      },
    },
  },
}
```

Resolution: per-DM override → provider default → no limit (all retained).Supported: `telegram`, `whatsapp`, `discord`, `slack`, `signal`, `imessage`, `msteams`.

#### [​](https://docs.openclaw.ai/gateway/config-channels\#self-chat-mode)  Self-chat mode

Include your own number in `allowFrom` to enable self-chat mode (ignores native @-mentions, only responds to text patterns):

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  agents: {
    list: [\
      {\
        id: "main",\
        groupChat: { mentionPatterns: ["reisponde", "@openclaw"] },\
      },\
    ],
  },
}
```

### [​](https://docs.openclaw.ai/gateway/config-channels\#commands-chat-command-handling)  Commands (chat command handling)

```
{
  commands: {
    native: "auto", // register native commands when supported
    nativeSkills: "auto", // register native skill commands when supported
    text: true, // parse /commands in chat messages
    bash: false, // allow ! (alias: /bash)
    bashForegroundMs: 2000,
    config: false, // allow /config
    mcp: false, // allow /mcp
    plugins: false, // allow /plugins
    debug: false, // allow /debug
    restart: true, // allow /restart + gateway restart tool
    ownerAllowFrom: ["discord:123456789012345678"],
    ownerDisplay: "raw", // raw | hash
    ownerDisplaySecret: "${OWNER_ID_HASH_SECRET}",
    allowFrom: {
      "*": ["user1"],
      discord: ["user:123"],
    },
    useAccessGroups: true,
  },
}
```

Command details

- This block configures command surfaces. For the current built-in + bundled command catalog, see [Slash Commands](https://docs.openclaw.ai/tools/slash-commands).
- This page is a **config-key reference**, not the full command catalog. Channel/plugin-owned commands such as QQ Bot `/bot-ping``/bot-help``/bot-logs`, LINE `/card`, device-pair `/pair`, memory `/dreaming`, phone-control `/phone`, and Talk `/voice` are documented in their channel/plugin pages plus [Slash Commands](https://docs.openclaw.ai/tools/slash-commands).
- Text commands must be **standalone** messages with leading `/`.
- `native: "auto"` turns on native commands for Discord/Telegram, leaves Slack off.
- `nativeSkills: "auto"` turns on native skill commands for Discord/Telegram, leaves Slack off.
- Override per channel: `channels.discord.commands.native` (bool or `"auto"`). `false` clears previously registered commands.
- Override native skill registration per channel with `channels.<provider>.commands.nativeSkills`.
- `channels.telegram.customCommands` adds extra Telegram bot menu entries.
- `bash: true` enables `! <cmd>` for host shell. Requires `tools.elevated.enabled` and sender in `tools.elevated.allowFrom.<channel>`.
- `config: true` enables `/config` (reads/writes `openclaw.json`). For gateway `chat.send` clients, persistent `/config set|unset` writes also require `operator.admin`; read-only `/config show` stays available to normal write-scoped operator clients.
- `mcp: true` enables `/mcp` for OpenClaw-managed MCP server config under `mcp.servers`.
- `plugins: true` enables `/plugins` for plugin discovery, install, and enable/disable controls.
- `channels.<provider>.configWrites` gates config mutations per channel (default: true).
- For multi-account channels, `channels.<provider>.accounts.<id>.configWrites` also gates writes that target that account (for example `/allowlist --config --account <id>` or `/config set channels.<provider>.accounts.<id>...`).
- `restart: false` disables `/restart` and gateway restart tool actions. Default: `true`.
- `ownerAllowFrom` is the explicit owner allowlist for owner-only commands/tools. It is separate from `allowFrom`.
- `ownerDisplay: "hash"` hashes owner ids in the system prompt. Set `ownerDisplaySecret` to control hashing.
- `allowFrom` is per-provider. When set, it is the **only** authorization source (channel allowlists/pairing and `useAccessGroups` are ignored).
- `useAccessGroups: false` allows commands to bypass access-group policies when `allowFrom` is not set.
- Command docs map:
  - built-in + bundled catalog: [Slash Commands](https://docs.openclaw.ai/tools/slash-commands)
  - channel-specific command surfaces: [Channels](https://docs.openclaw.ai/channels)
  - QQ Bot commands: [QQ Bot](https://docs.openclaw.ai/channels/qqbot)
  - pairing commands: [Pairing](https://docs.openclaw.ai/channels/pairing)
  - LINE card command: [LINE](https://docs.openclaw.ai/channels/line)
  - memory dreaming: [Dreaming](https://docs.openclaw.ai/concepts/dreaming)

* * *

## [​](https://docs.openclaw.ai/gateway/config-channels\#related)  Related

- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference) — top-level keys
- [Configuration — agents](https://docs.openclaw.ai/gateway/config-agents)
- [Channels overview](https://docs.openclaw.ai/channels)

[Configuration — agents](https://docs.openclaw.ai/gateway/config-agents) [Tools and custom providers](https://docs.openclaw.ai/gateway/config-tools)

Ctrl+I