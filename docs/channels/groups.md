---
source_url: https://docs.openclaw.ai/channels/groups
title: "Groups - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels/groups#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Configuration

Groups

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Beginner intro (2 minutes)](https://docs.openclaw.ai/channels/groups#beginner-intro-2-minutes)
- [Visible replies](https://docs.openclaw.ai/channels/groups#visible-replies)
- [Context visibility and allowlists](https://docs.openclaw.ai/channels/groups#context-visibility-and-allowlists)
- [Session keys](https://docs.openclaw.ai/channels/groups#session-keys)
- [Pattern: personal DMs + public groups (single agent)](https://docs.openclaw.ai/channels/groups#pattern-personal-dms-%2B-public-groups-single-agent)
- [Display labels](https://docs.openclaw.ai/channels/groups#display-labels)
- [Group policy](https://docs.openclaw.ai/channels/groups#group-policy)
- [Mention gating (default)](https://docs.openclaw.ai/channels/groups#mention-gating-default)
- [Group/channel tool restrictions (optional)](https://docs.openclaw.ai/channels/groups#group%2Fchannel-tool-restrictions-optional)
- [Group allowlists](https://docs.openclaw.ai/channels/groups#group-allowlists)
- [Activation (owner-only)](https://docs.openclaw.ai/channels/groups#activation-owner-only)
- [Context fields](https://docs.openclaw.ai/channels/groups#context-fields)
- [iMessage specifics](https://docs.openclaw.ai/channels/groups#imessage-specifics)
- [WhatsApp system prompts](https://docs.openclaw.ai/channels/groups#whatsapp-system-prompts)
- [WhatsApp specifics](https://docs.openclaw.ai/channels/groups#whatsapp-specifics)
- [Related](https://docs.openclaw.ai/channels/groups#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw treats group chats consistently across surfaces: Discord, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo.

## [​](https://docs.openclaw.ai/channels/groups\#beginner-intro-2-minutes)  Beginner intro (2 minutes)

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

## [​](https://docs.openclaw.ai/channels/groups\#visible-replies)  Visible replies

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

## [​](https://docs.openclaw.ai/channels/groups\#context-visibility-and-allowlists)  Context visibility and allowlists

Two different controls are involved in group safety:

- **Trigger authorization**: who can trigger the agent (`groupPolicy`, `groups`, `groupAllowFrom`, channel-specific allowlists).
- **Context visibility**: what supplemental context is injected into the model (reply text, quotes, thread history, forwarded metadata).

By default, OpenClaw prioritizes normal chat behavior and keeps context mostly as received. This means allowlists primarily decide who can trigger actions, not a universal redaction boundary for every quoted or historical snippet.

Current behavior is channel-specific

- Some channels already apply sender-based filtering for supplemental context in specific paths (for example Slack thread seeding, Matrix reply/thread lookups).
- Other channels still pass quote/reply/forward context through as received.

Hardening direction (planned)

- `contextVisibility: "all"` (default) keeps current as-received behavior.
- `contextVisibility: "allowlist"` filters supplemental context to allowlisted senders.
- `contextVisibility: "allowlist_quote"` is `allowlist` plus one explicit quote/reply exception.

Until this hardening model is implemented consistently across channels, expect differences by surface.

![Group message flow](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/images/groups-flow.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=eeb387df91a967fbbe8bf8f80ae41dd7)If you want…

| Goal | What to set |
| --- | --- |
| Allow all groups but only reply on @mentions | `groups: { "*": { requireMention: true } }` |
| Disable all group replies | `groupPolicy: "disabled"` |
| Only specific groups | `groups: { "<group-id>": { ... } }` (no `"*"` key) |
| Only you can trigger in groups | `groupPolicy: "allowlist"`, `groupAllowFrom: ["+1555..."]` |
| Reuse one trusted sender set across channels | `groupAllowFrom: ["accessGroup:operators"]` |

For reusable sender allowlists, see [Access groups](https://docs.openclaw.ai/channels/access-groups).

## [​](https://docs.openclaw.ai/channels/groups\#session-keys)  Session keys

- Group sessions use `agent:<agentId>:<channel>:group:<id>` session keys (rooms/channels use `agent:<agentId>:<channel>:channel:<id>`).
- Telegram forum topics add `:topic:<threadId>` to the group id so each topic has its own session.
- Direct chats use the main session (or per-sender if configured).
- Heartbeats are skipped for group sessions.

## [​](https://docs.openclaw.ai/channels/groups\#pattern-personal-dms-+-public-groups-single-agent)  Pattern: personal DMs + public groups (single agent)

Yes — this works well if your “personal” traffic is **DMs** and your “public” traffic is **groups**.Why: in single-agent mode, DMs typically land in the **main** session key (`agent:main:main`), while groups always use **non-main** session keys (`agent:main:<channel>:group:<id>`). If you enable sandboxing with `mode: "non-main"`, those group sessions run in the configured sandbox backend while your main DM session stays on-host. Docker is the default backend if you do not choose one.This gives you one agent “brain” (shared workspace + memory), but two execution postures:

- **DMs**: full tools (host)
- **Groups**: sandbox + restricted tools

If you need truly separate workspaces/personas (“personal” and “public” must never mix), use a second agent + bindings. See [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent).

- DMs on host, groups sandboxed

- Groups see only an allowlisted folder


```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main", // groups/channels are non-main -> sandboxed
        scope: "session", // strongest isolation (one container per group/channel)
        workspaceAccess: "none",
      },
    },
  },
  tools: {
    sandbox: {
      tools: {
        // If allow is non-empty, everything else is blocked (deny still wins).
        allow: ["group:messaging", "group:sessions"],
        deny: ["group:runtime", "group:fs", "group:ui", "nodes", "cron", "gateway"],
      },
    },
  },
}
```

Want “groups can only see folder X” instead of “no host access”? Keep `workspaceAccess: "none"` and mount only allowlisted paths into the sandbox:

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",
        scope: "session",
        workspaceAccess: "none",
        docker: {
          binds: [\
            // hostPath:containerPath:mode\
            "/home/user/FriendsShared:/data:ro",\
          ],
        },
      },
    },
  },
}
```

Related:

- Configuration keys and defaults: [Gateway configuration](https://docs.openclaw.ai/gateway/config-agents#agentsdefaultssandbox)
- Debugging why a tool is blocked: [Sandbox vs Tool Policy vs Elevated](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated)
- Bind mounts details: [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing#custom-bind-mounts)

## [​](https://docs.openclaw.ai/channels/groups\#display-labels)  Display labels

- UI labels use `displayName` when available, formatted as `<channel>:<token>`.
- `#room` is reserved for rooms/channels; group chats use `g-<slug>` (lowercase, spaces -> `-`, keep `#@+._-`).

## [​](https://docs.openclaw.ai/channels/groups\#group-policy)  Group policy

Control how group/room messages are handled per channel:

```
{
  channels: {
    whatsapp: {
      groupPolicy: "disabled", // "open" | "disabled" | "allowlist"
      groupAllowFrom: ["+15551234567"],
    },
    telegram: {
      groupPolicy: "disabled",
      groupAllowFrom: ["123456789"], // numeric Telegram user id (wizard can resolve @username)
    },
    signal: {
      groupPolicy: "disabled",
      groupAllowFrom: ["+15551234567"],
    },
    imessage: {
      groupPolicy: "disabled",
      groupAllowFrom: ["chat_id:123"],
    },
    msteams: {
      groupPolicy: "disabled",
      groupAllowFrom: ["user@org.com"],
    },
    discord: {
      groupPolicy: "allowlist",
      guilds: {
        GUILD_ID: { channels: { help: { allow: true } } },
      },
    },
    slack: {
      groupPolicy: "allowlist",
      channels: { "#general": { allow: true } },
    },
    matrix: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["@owner:example.org"],
      groups: {
        "!roomId:example.org": { enabled: true },
        "#alias:example.org": { enabled: true },
      },
    },
  },
}
```

| Policy | Behavior |
| --- | --- |
| `"open"` | Groups bypass allowlists; mention-gating still applies. |
| `"disabled"` | Block all group messages entirely. |
| `"allowlist"` | Only allow groups/rooms that match the configured allowlist. |

Per-channel notes

- `groupPolicy` is separate from mention-gating (which requires @mentions).
- WhatsApp/Telegram/Signal/iMessage/Microsoft Teams/Zalo: use `groupAllowFrom` (fallback: explicit `allowFrom`).
- Signal: `groupAllowFrom` can match either the inbound Signal group id or the sender phone/UUID.
- DM pairing approvals (`*-allowFrom` store entries) apply to DM access only; group sender authorization stays explicit to group allowlists.
- Discord: allowlist uses `channels.discord.guilds.<id>.channels`.
- Slack: allowlist uses `channels.slack.channels`.
- Matrix: allowlist uses `channels.matrix.groups`. Prefer room IDs or aliases; joined-room name lookup is best-effort, and unresolved names are ignored at runtime. Use `channels.matrix.groupAllowFrom` to restrict senders; per-room `users` allowlists are also supported.
- Group DMs are controlled separately (`channels.discord.dm.*`, `channels.slack.dm.*`).
- Telegram allowlist can match user IDs (`"123456789"`, `"telegram:123456789"`, `"tg:123456789"`) or usernames (`"@alice"` or `"alice"`); prefixes are case-insensitive.
- Default is `groupPolicy: "allowlist"`; if your group allowlist is empty, group messages are blocked.
- Runtime safety: when a provider block is completely missing (`channels.<provider>` absent), group policy falls back to a fail-closed mode (typically `allowlist`) instead of inheriting `channels.defaults.groupPolicy`.

Quick mental model (evaluation order for group messages):

1

[Navigate to header](https://docs.openclaw.ai/channels/groups#)

groupPolicy

`groupPolicy` (open/disabled/allowlist).

2

[Navigate to header](https://docs.openclaw.ai/channels/groups#)

Group allowlists

Group allowlists (`*.groups`, `*.groupAllowFrom`, channel-specific allowlist).

3

[Navigate to header](https://docs.openclaw.ai/channels/groups#)

Mention gating

Mention gating (`requireMention`, `/activation`).

## [​](https://docs.openclaw.ai/channels/groups\#mention-gating-default)  Mention gating (default)

Group messages require a mention unless overridden per group. Defaults live per subsystem under `*.groups."*"`.Replying to a bot message counts as an implicit mention when the channel supports reply metadata. Quoting a bot message can also count as an implicit mention on channels that expose quote metadata. Current built-in cases include Telegram, WhatsApp, Slack, Discord, Microsoft Teams, and ZaloUser.

```
{
  channels: {
    whatsapp: {
      groups: {
        "*": { requireMention: true },
        "123@g.us": { requireMention: false },
      },
    },
    telegram: {
      groups: {
        "*": { requireMention: true },
        "123456789": { requireMention: false },
      },
    },
    imessage: {
      groups: {
        "*": { requireMention: true },
        "123": { requireMention: false },
      },
    },
  },
  agents: {
    list: [\
      {\
        id: "main",\
        groupChat: {\
          mentionPatterns: ["@openclaw", "openclaw", "\\+15555550123"],\
          historyLimit: 50,\
        },\
      },\
    ],
  },
}
```

Mention gating notes

- `mentionPatterns` are case-insensitive safe regex patterns; invalid patterns and unsafe nested-repetition forms are ignored.
- Surfaces that provide explicit mentions still pass; patterns are a fallback.
- Per-agent override: `agents.list[].groupChat.mentionPatterns` (useful when multiple agents share a group).
- Mention gating is only enforced when mention detection is possible (native mentions or `mentionPatterns` are configured).
- Allowlisting a group or sender does not disable mention gating; set that group’s `requireMention` to `false` when all messages should trigger.
- Group chat prompt context carries the resolved silent-reply instruction every turn; workspace files should not duplicate `NO_REPLY` mechanics.
- Groups where silent replies are allowed treat clean empty or reasoning-only model turns as silent, equivalent to `NO_REPLY`. Direct chats do the same only when direct silent replies are explicitly allowed; otherwise empty replies remain failed agent turns.
- Discord defaults live in `channels.discord.guilds."*"` (overridable per guild/channel).
- Group history context is wrapped uniformly across channels and is **pending-only** (messages skipped due to mention gating); use `messages.groupChat.historyLimit` for the global default and `channels.<channel>.historyLimit` (or `channels.<channel>.accounts.*.historyLimit`) for overrides. Set `0` to disable.

## [​](https://docs.openclaw.ai/channels/groups\#group/channel-tool-restrictions-optional)  Group/channel tool restrictions (optional)

Some channel configs support restricting which tools are available **inside a specific group/room/channel**.

- `tools`: allow/deny tools for the whole group.
- `toolsBySender`: per-sender overrides within the group. Use explicit key prefixes: `id:<senderId>`, `e164:<phone>`, `username:<handle>`, `name:<displayName>`, and `"*"` wildcard. Legacy unprefixed keys are still accepted and matched as `id:` only.

Resolution order (most specific wins):

1

[Navigate to header](https://docs.openclaw.ai/channels/groups#)

Group toolsBySender

Group/channel `toolsBySender` match.

2

[Navigate to header](https://docs.openclaw.ai/channels/groups#)

Group tools

Group/channel `tools`.

3

[Navigate to header](https://docs.openclaw.ai/channels/groups#)

Default toolsBySender

Default (`"*"`) `toolsBySender` match.

4

[Navigate to header](https://docs.openclaw.ai/channels/groups#)

Default tools

Default (`"*"`) `tools`.

Example (Telegram):

```
{
  channels: {
    telegram: {
      groups: {
        "*": { tools: { deny: ["exec"] } },
        "-1001234567890": {
          tools: { deny: ["exec", "read", "write"] },
          toolsBySender: {
            "id:123456789": { alsoAllow: ["exec"] },
          },
        },
      },
    },
  },
}
```

Group/channel tool restrictions are applied in addition to global/agent tool policy (deny still wins). Some channels use different nesting for rooms/channels (e.g., Discord `guilds.*.channels.*`, Slack `channels.*`, Microsoft Teams `teams.*.channels.*`).

## [​](https://docs.openclaw.ai/channels/groups\#group-allowlists)  Group allowlists

When `channels.whatsapp.groups`, `channels.telegram.groups`, or `channels.imessage.groups` is configured, the keys act as a group allowlist. Use `"*"` to allow all groups while still setting default mention behavior.

Common confusion: DM pairing approval is not the same as group authorization. For channels that support DM pairing, the pairing store unlocks DMs only. Group commands still require explicit group sender authorization from config allowlists such as `groupAllowFrom` or the documented config fallback for that channel.

Common intents (copy/paste):

- Disable all group replies

- Allow only specific groups (WhatsApp)

- Allow all groups but require mention

- Owner-only triggers (WhatsApp)


```
{
  channels: { whatsapp: { groupPolicy: "disabled" } },
}
```

```
{
  channels: {
    whatsapp: {
      groups: {
        "123@g.us": { requireMention: true },
        "456@g.us": { requireMention: false },
      },
    },
  },
}
```

```
{
  channels: {
    whatsapp: {
      groups: { "*": { requireMention: true } },
    },
  },
}
```

```
{
  channels: {
    whatsapp: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15551234567"],
      groups: { "*": { requireMention: true } },
    },
  },
}
```

## [​](https://docs.openclaw.ai/channels/groups\#activation-owner-only)  Activation (owner-only)

Group owners can toggle per-group activation:

- `/activation mention`
- `/activation always`

Owner is determined by `channels.whatsapp.allowFrom` (or the bot’s self E.164 when unset). Send the command as a standalone message. Other surfaces currently ignore `/activation`.

## [​](https://docs.openclaw.ai/channels/groups\#context-fields)  Context fields

Group inbound payloads set:

- `ChatType=group`
- `GroupSubject` (if known)
- `GroupMembers` (if known)
- `WasMentioned` (mention gating result)
- Telegram forum topics also include `MessageThreadId` and `IsForum`.

Channel-specific notes:

- BlueBubbles can optionally enrich unnamed macOS group participants from the local Contacts database before populating `GroupMembers`. This is off by default and only runs after normal group gating passes.

The agent system prompt includes a group intro on the first turn of a new group session. It reminds the model to respond like a human, avoid Markdown tables, minimize empty lines and follow normal chat spacing, and avoid typing literal `\n` sequences. Channel-sourced group names and participant labels are rendered as fenced untrusted metadata, not inline system instructions.

## [​](https://docs.openclaw.ai/channels/groups\#imessage-specifics)  iMessage specifics

- Prefer `chat_id:<id>` when routing or allowlisting.
- List chats: `imsg chats --limit 20`.
- Group replies always go back to the same `chat_id`.

## [​](https://docs.openclaw.ai/channels/groups\#whatsapp-system-prompts)  WhatsApp system prompts

See [WhatsApp](https://docs.openclaw.ai/channels/whatsapp#system-prompts) for the canonical WhatsApp system prompt rules, including group and direct prompt resolution, wildcard behavior, and account override semantics.

## [​](https://docs.openclaw.ai/channels/groups\#whatsapp-specifics)  WhatsApp specifics

See [Group messages](https://docs.openclaw.ai/channels/group-messages) for WhatsApp-only behavior (history injection, mention handling details).

## [​](https://docs.openclaw.ai/channels/groups\#related)  Related

- [Broadcast groups](https://docs.openclaw.ai/channels/broadcast-groups)
- [Channel routing](https://docs.openclaw.ai/channels/channel-routing)
- [Group messages](https://docs.openclaw.ai/channels/group-messages)
- [Pairing](https://docs.openclaw.ai/channels/pairing)

[Group messages](https://docs.openclaw.ai/channels/group-messages) [Broadcast groups](https://docs.openclaw.ai/channels/broadcast-groups)

Ctrl+I

![Group message flow](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/images/groups-flow.svg?w=1100&fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=90eb00ad3584e64ef4e66078f7375dc6)