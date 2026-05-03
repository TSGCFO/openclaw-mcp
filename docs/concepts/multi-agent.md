---
source_url: https://docs.openclaw.ai/concepts/multi-agent
title: "Multi-agent routing - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/multi-agent#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Multi-agent

Multi-agent routing

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What is “one agent”?](https://docs.openclaw.ai/concepts/multi-agent#what-is-%E2%80%9Cone-agent%E2%80%9D)
- [Paths (quick map)](https://docs.openclaw.ai/concepts/multi-agent#paths-quick-map)
- [Single-agent mode (default)](https://docs.openclaw.ai/concepts/multi-agent#single-agent-mode-default)
- [Agent helper](https://docs.openclaw.ai/concepts/multi-agent#agent-helper)
- [Quick start](https://docs.openclaw.ai/concepts/multi-agent#quick-start)
- [Multiple agents = multiple people, multiple personalities](https://docs.openclaw.ai/concepts/multi-agent#multiple-agents-%3D-multiple-people-multiple-personalities)
- [Cross-agent QMD memory search](https://docs.openclaw.ai/concepts/multi-agent#cross-agent-qmd-memory-search)
- [One WhatsApp number, multiple people (DM split)](https://docs.openclaw.ai/concepts/multi-agent#one-whatsapp-number-multiple-people-dm-split)
- [Routing rules (how messages pick an agent)](https://docs.openclaw.ai/concepts/multi-agent#routing-rules-how-messages-pick-an-agent)
- [Multiple accounts / phone numbers](https://docs.openclaw.ai/concepts/multi-agent#multiple-accounts-%2F-phone-numbers)
- [Concepts](https://docs.openclaw.ai/concepts/multi-agent#concepts)
- [Platform examples](https://docs.openclaw.ai/concepts/multi-agent#platform-examples)
- [Common patterns](https://docs.openclaw.ai/concepts/multi-agent#common-patterns)
- [Per-agent sandbox and tool configuration](https://docs.openclaw.ai/concepts/multi-agent#per-agent-sandbox-and-tool-configuration)
- [Related](https://docs.openclaw.ai/concepts/multi-agent#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Run multiple _isolated_ agents — each with its own workspace, state directory (`agentDir`), and session history — plus multiple channel accounts (e.g. two WhatsApps) in one running Gateway. Inbound messages are routed to the right agent through bindings.An **agent** here is the full per-persona scope: workspace files, auth profiles, model registry, and session store. `agentDir` is the on-disk state directory that holds this per-agent config at `~/.openclaw/agents/<agentId>/`. A **binding** maps a channel account (e.g. a Slack workspace or a WhatsApp number) to one of those agents.

## [​](https://docs.openclaw.ai/concepts/multi-agent\#what-is-%E2%80%9Cone-agent%E2%80%9D)  What is “one agent”?

An **agent** is a fully scoped brain with its own:

- **Workspace** (files, AGENTS.md/SOUL.md/USER.md, local notes, persona rules).
- **State directory** (`agentDir`) for auth profiles, model registry, and per-agent config.
- **Session store** (chat history + routing state) under `~/.openclaw/agents/<agentId>/sessions`.

Auth profiles are **per-agent**. Each agent reads from its own:

```
~/.openclaw/agents/<agentId>/agent/auth-profiles.json
```

`sessions_history` is the safer cross-session recall path here too: it returns a bounded, sanitized view, not a raw transcript dump. Assistant recall strips thinking tags, `<relevant-memories>` scaffolding, plain-text tool-call XML payloads (including `<tool_call>...</tool_call>`, `<function_call>...</function_call>`, `<tool_calls>...</tool_calls>`, `<function_calls>...</function_calls>`, and truncated tool-call blocks), downgraded tool-call scaffolding, leaked ASCII/full-width model control tokens, and malformed MiniMax tool-call XML before redaction/truncation.

Never reuse `agentDir` across agents (it causes auth/session collisions). Agents
can read through to the default/main agent’s auth profiles when they do not have
a local profile, but OpenClaw does not clone OAuth refresh tokens into the
secondary agent store. If you want an independent OAuth account, sign in from
that agent; if you copy credentials manually, copy only portable static
`api_key` or `token` profiles.

Skills are loaded from each agent workspace plus shared roots such as `~/.openclaw/skills`, then filtered by the effective agent skill allowlist when configured. Use `agents.defaults.skills` for a shared baseline and `agents.list[].skills` for per-agent replacement. See [Skills: per-agent vs shared](https://docs.openclaw.ai/tools/skills#per-agent-vs-shared-skills) and [Skills: agent skill allowlists](https://docs.openclaw.ai/tools/skills#agent-skill-allowlists).The Gateway can host **one agent** (default) or **many agents** side-by-side.

**Workspace note:** each agent’s workspace is the **default cwd**, not a hard sandbox. Relative paths resolve inside the workspace, but absolute paths can reach other host locations unless sandboxing is enabled. See [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing).

## [​](https://docs.openclaw.ai/concepts/multi-agent\#paths-quick-map)  Paths (quick map)

- Config: `~/.openclaw/openclaw.json` (or `OPENCLAW_CONFIG_PATH`)
- State dir: `~/.openclaw` (or `OPENCLAW_STATE_DIR`)
- Workspace: `~/.openclaw/workspace` (or `~/.openclaw/workspace-<agentId>`)
- Agent dir: `~/.openclaw/agents/<agentId>/agent` (or `agents.list[].agentDir`)
- Sessions: `~/.openclaw/agents/<agentId>/sessions`

### [​](https://docs.openclaw.ai/concepts/multi-agent\#single-agent-mode-default)  Single-agent mode (default)

If you do nothing, OpenClaw runs a single agent:

- `agentId` defaults to **`main`**.
- Sessions are keyed as `agent:main:<mainKey>`.
- Workspace defaults to `~/.openclaw/workspace` (or `~/.openclaw/workspace-<profile>` when `OPENCLAW_PROFILE` is set).
- State defaults to `~/.openclaw/agents/main/agent`.

## [​](https://docs.openclaw.ai/concepts/multi-agent\#agent-helper)  Agent helper

Use the agent wizard to add a new isolated agent:

```
openclaw agents add work
```

Then add `bindings` (or let the wizard do it) to route inbound messages.Verify with:

```
openclaw agents list --bindings
```

## [​](https://docs.openclaw.ai/concepts/multi-agent\#quick-start)  Quick start

1

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

Create each agent workspace

Use the wizard or create workspaces manually:

```
openclaw agents add coding
openclaw agents add social
```

Each agent gets its own workspace with `SOUL.md`, `AGENTS.md`, and optional `USER.md`, plus a dedicated `agentDir` and session store under `~/.openclaw/agents/<agentId>`.

2

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

Create channel accounts

Create one account per agent on your preferred channels:

- Discord: one bot per agent, enable Message Content Intent, copy each token.
- Telegram: one bot per agent via BotFather, copy each token.
- WhatsApp: link each phone number per account.

```
openclaw channels login --channel whatsapp --account work
```

See channel guides: [Discord](https://docs.openclaw.ai/channels/discord), [Telegram](https://docs.openclaw.ai/channels/telegram), [WhatsApp](https://docs.openclaw.ai/channels/whatsapp).

3

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

Add agents, accounts, and bindings

Add agents under `agents.list`, channel accounts under `channels.<channel>.accounts`, and connect them with `bindings` (examples below).

4

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

Restart and verify

```
openclaw gateway restart
openclaw agents list --bindings
openclaw channels status --probe
```

## [​](https://docs.openclaw.ai/concepts/multi-agent\#multiple-agents-=-multiple-people-multiple-personalities)  Multiple agents = multiple people, multiple personalities

With **multiple agents**, each `agentId` becomes a **fully isolated persona**:

- **Different phone numbers/accounts** (per channel `accountId`).
- **Different personalities** (per-agent workspace files like `AGENTS.md` and `SOUL.md`).
- **Separate auth + sessions** (no cross-talk unless explicitly enabled).

This lets **multiple people** share one Gateway server while keeping their AI “brains” and data isolated.

## [​](https://docs.openclaw.ai/concepts/multi-agent\#cross-agent-qmd-memory-search)  Cross-agent QMD memory search

If one agent should search another agent’s QMD session transcripts, add extra collections under `agents.list[].memorySearch.qmd.extraCollections`. Use `agents.defaults.memorySearch.qmd.extraCollections` only when every agent should inherit the same shared transcript collections.

```
{
  agents: {
    defaults: {
      workspace: "~/workspaces/main",
      memorySearch: {
        qmd: {
          extraCollections: [{ path: "~/agents/family/sessions", name: "family-sessions" }],
        },
      },
    },
    list: [\
      {\
        id: "main",\
        workspace: "~/workspaces/main",\
        memorySearch: {\
          qmd: {\
            extraCollections: [{ path: "notes" }], // resolves inside workspace -> collection named "notes-main"\
          },\
        },\
      },\
      { id: "family", workspace: "~/workspaces/family" },\
    ],
  },
  memory: {
    backend: "qmd",
    qmd: { includeDefaultMemory: false },
  },
}
```

The extra collection path can be shared across agents, but the collection name stays explicit when the path is outside the agent workspace. Paths inside the workspace remain agent-scoped so each agent keeps its own transcript search set.

## [​](https://docs.openclaw.ai/concepts/multi-agent\#one-whatsapp-number-multiple-people-dm-split)  One WhatsApp number, multiple people (DM split)

You can route **different WhatsApp DMs** to different agents while staying on **one WhatsApp account**. Match on sender E.164 (like `+15551234567`) with `peer.kind: "direct"`. Replies still come from the same WhatsApp number (no per-agent sender identity).

Direct chats collapse to the agent’s **main session key**, so true isolation requires **one agent per person**.

Example:

```
{
  agents: {
    list: [\
      { id: "alex", workspace: "~/.openclaw/workspace-alex" },\
      { id: "mia", workspace: "~/.openclaw/workspace-mia" },\
    ],
  },
  bindings: [\
    {\
      agentId: "alex",\
      match: { channel: "whatsapp", peer: { kind: "direct", id: "+15551230001" } },\
    },\
    {\
      agentId: "mia",\
      match: { channel: "whatsapp", peer: { kind: "direct", id: "+15551230002" } },\
    },\
  ],
  channels: {
    whatsapp: {
      dmPolicy: "allowlist",
      allowFrom: ["+15551230001", "+15551230002"],
    },
  },
}
```

Notes:

- DM access control is **global per WhatsApp account** (pairing/allowlist), not per agent.
- For shared groups, bind the group to one agent or use [Broadcast groups](https://docs.openclaw.ai/channels/broadcast-groups).

## [​](https://docs.openclaw.ai/concepts/multi-agent\#routing-rules-how-messages-pick-an-agent)  Routing rules (how messages pick an agent)

Bindings are **deterministic** and **most-specific wins**:

1

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

peer match

Exact DM/group/channel id.

2

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

parentPeer match

Thread inheritance.

3

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

guildId + roles

Discord role routing.

4

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

guildId

Discord.

5

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

teamId

Slack.

6

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

accountId match for a channel

Per-account fallback.

7

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

Channel-level match

`accountId: "*"`.

8

[Navigate to header](https://docs.openclaw.ai/concepts/multi-agent#)

Default agent

Fallback to `agents.list[].default`, else first list entry, default: `main`.

Tie-breaking and AND semantics

- If multiple bindings match in the same tier, the first one in config order wins.
- If a binding sets multiple match fields (for example `peer` \+ `guildId`), all specified fields are required (`AND` semantics).

Account-scope detail

- A binding that omits `accountId` matches the default account only.
- Use `accountId: "*"` for a channel-wide fallback across all accounts.
- If you later add the same binding for the same agent with an explicit account id, OpenClaw upgrades the existing channel-only binding to account-scoped instead of duplicating it.

## [​](https://docs.openclaw.ai/concepts/multi-agent\#multiple-accounts-/-phone-numbers)  Multiple accounts / phone numbers

Channels that support **multiple accounts** (e.g. WhatsApp) use `accountId` to identify each login. Each `accountId` can be routed to a different agent, so one server can host multiple phone numbers without mixing sessions.If you want a channel-wide default account when `accountId` is omitted, set `channels.<channel>.defaultAccount` (optional). When unset, OpenClaw falls back to `default` if present, otherwise the first configured account id (sorted).Common channels supporting this pattern include:

- `whatsapp`, `telegram`, `discord`, `slack`, `signal`, `imessage`
- `irc`, `line`, `googlechat`, `mattermost`, `matrix`, `nextcloud-talk`
- `bluebubbles`, `zalo`, `zalouser`, `nostr`, `feishu`

## [​](https://docs.openclaw.ai/concepts/multi-agent\#concepts)  Concepts

- `agentId`: one “brain” (workspace, per-agent auth, per-agent session store).
- `accountId`: one channel account instance (e.g. WhatsApp account `"personal"` vs `"biz"`).
- `binding`: routes inbound messages to an `agentId` by `(channel, accountId, peer)` and optionally guild/team ids.
- Direct chats collapse to `agent:<agentId>:<mainKey>` (per-agent “main”; `session.mainKey`).

## [​](https://docs.openclaw.ai/concepts/multi-agent\#platform-examples)  Platform examples

Discord bots per agent

Each Discord bot account maps to a unique `accountId`. Bind each account to an agent and keep allowlists per bot.

```
{
  agents: {
    list: [\
      { id: "main", workspace: "~/.openclaw/workspace-main" },\
      { id: "coding", workspace: "~/.openclaw/workspace-coding" },\
    ],
  },
  bindings: [\
    { agentId: "main", match: { channel: "discord", accountId: "default" } },\
    { agentId: "coding", match: { channel: "discord", accountId: "coding" } },\
  ],
  channels: {
    discord: {
      groupPolicy: "allowlist",
      accounts: {
        default: {
          token: "DISCORD_BOT_TOKEN_MAIN",
          guilds: {
            "123456789012345678": {
              channels: {
                "222222222222222222": { allow: true, requireMention: false },
              },
            },
          },
        },
        coding: {
          token: "DISCORD_BOT_TOKEN_CODING",
          guilds: {
            "123456789012345678": {
              channels: {
                "333333333333333333": { allow: true, requireMention: false },
              },
            },
          },
        },
      },
    },
  },
}
```

- Invite each bot to the guild and enable Message Content Intent.
- Tokens live in `channels.discord.accounts.<id>.token` (default account can use `DISCORD_BOT_TOKEN`).

Telegram bots per agent

```
{
  agents: {
    list: [\
      { id: "main", workspace: "~/.openclaw/workspace-main" },\
      { id: "alerts", workspace: "~/.openclaw/workspace-alerts" },\
    ],
  },
  bindings: [\
    { agentId: "main", match: { channel: "telegram", accountId: "default" } },\
    { agentId: "alerts", match: { channel: "telegram", accountId: "alerts" } },\
  ],
  channels: {
    telegram: {
      accounts: {
        default: {
          botToken: "123456:ABC...",
          dmPolicy: "pairing",
        },
        alerts: {
          botToken: "987654:XYZ...",
          dmPolicy: "allowlist",
          allowFrom: ["tg:123456789"],
        },
      },
    },
  },
}
```

- Create one bot per agent with BotFather and copy each token.
- Tokens live in `channels.telegram.accounts.<id>.botToken` (default account can use `TELEGRAM_BOT_TOKEN`).

WhatsApp numbers per agent

Link each account before starting the gateway:

```
openclaw channels login --channel whatsapp --account personal
openclaw channels login --channel whatsapp --account biz
```

`~/.openclaw/openclaw.json` (JSON5):

```
{
  agents: {
    list: [\
      {\
        id: "home",\
        default: true,\
        name: "Home",\
        workspace: "~/.openclaw/workspace-home",\
        agentDir: "~/.openclaw/agents/home/agent",\
      },\
      {\
        id: "work",\
        name: "Work",\
        workspace: "~/.openclaw/workspace-work",\
        agentDir: "~/.openclaw/agents/work/agent",\
      },\
    ],
  },

  // Deterministic routing: first match wins (most-specific first).
  bindings: [\
    { agentId: "home", match: { channel: "whatsapp", accountId: "personal" } },\
    { agentId: "work", match: { channel: "whatsapp", accountId: "biz" } },\
\
    // Optional per-peer override (example: send a specific group to work agent).\
    {\
      agentId: "work",\
      match: {\
        channel: "whatsapp",\
        accountId: "personal",\
        peer: { kind: "group", id: "1203630...@g.us" },\
      },\
    },\
  ],

  // Off by default: agent-to-agent messaging must be explicitly enabled + allowlisted.
  tools: {
    agentToAgent: {
      enabled: false,
      allow: ["home", "work"],
    },
  },

  channels: {
    whatsapp: {
      accounts: {
        personal: {
          // Optional override. Default: ~/.openclaw/credentials/whatsapp/personal
          // authDir: "~/.openclaw/credentials/whatsapp/personal",
        },
        biz: {
          // Optional override. Default: ~/.openclaw/credentials/whatsapp/biz
          // authDir: "~/.openclaw/credentials/whatsapp/biz",
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/concepts/multi-agent\#common-patterns)  Common patterns

- WhatsApp daily + Telegram deep work

- Same channel, one peer to Opus

- Family agent bound to a WhatsApp group


Split by channel: route WhatsApp to a fast everyday agent and Telegram to an Opus agent.

```
{
  agents: {
    list: [\
      {\
        id: "chat",\
        name: "Everyday",\
        workspace: "~/.openclaw/workspace-chat",\
        model: "anthropic/claude-sonnet-4-6",\
      },\
      {\
        id: "opus",\
        name: "Deep Work",\
        workspace: "~/.openclaw/workspace-opus",\
        model: "anthropic/claude-opus-4-6",\
      },\
    ],
  },
  bindings: [\
    { agentId: "chat", match: { channel: "whatsapp" } },\
    { agentId: "opus", match: { channel: "telegram" } },\
  ],
}
```

Notes:

- If you have multiple accounts for a channel, add `accountId` to the binding (for example `{ channel: "whatsapp", accountId: "personal" }`).
- To route a single DM/group to Opus while keeping the rest on chat, add a `match.peer` binding for that peer; peer matches always win over channel-wide rules.

Keep WhatsApp on the fast agent, but route one DM to Opus:

```
{
  agents: {
    list: [\
      {\
        id: "chat",\
        name: "Everyday",\
        workspace: "~/.openclaw/workspace-chat",\
        model: "anthropic/claude-sonnet-4-6",\
      },\
      {\
        id: "opus",\
        name: "Deep Work",\
        workspace: "~/.openclaw/workspace-opus",\
        model: "anthropic/claude-opus-4-6",\
      },\
    ],
  },
  bindings: [\
    {\
      agentId: "opus",\
      match: { channel: "whatsapp", peer: { kind: "direct", id: "+15551234567" } },\
    },\
    { agentId: "chat", match: { channel: "whatsapp" } },\
  ],
}
```

Peer bindings always win, so keep them above the channel-wide rule.

Bind a dedicated family agent to a single WhatsApp group, with mention gating and a tighter tool policy:

```
{
  agents: {
    list: [\
      {\
        id: "family",\
        name: "Family",\
        workspace: "~/.openclaw/workspace-family",\
        identity: { name: "Family Bot" },\
        groupChat: {\
          mentionPatterns: ["@family", "@familybot", "@Family Bot"],\
        },\
        sandbox: {\
          mode: "all",\
          scope: "agent",\
        },\
        tools: {\
          allow: [\
            "exec",\
            "read",\
            "sessions_list",\
            "sessions_history",\
            "sessions_send",\
            "sessions_spawn",\
            "session_status",\
          ],\
          deny: ["write", "edit", "apply_patch", "browser", "canvas", "nodes", "cron"],\
        },\
      },\
    ],
  },
  bindings: [\
    {\
      agentId: "family",\
      match: {\
        channel: "whatsapp",\
        peer: { kind: "group", id: "120363999999999999@g.us" },\
      },\
    },\
  ],
}
```

Notes:

- Tool allow/deny lists are **tools**, not skills. If a skill needs to run a binary, ensure `exec` is allowed and the binary exists in the sandbox.
- For stricter gating, set `agents.list[].groupChat.mentionPatterns` and keep group allowlists enabled for the channel.

## [​](https://docs.openclaw.ai/concepts/multi-agent\#per-agent-sandbox-and-tool-configuration)  Per-agent sandbox and tool configuration

Each agent can have its own sandbox and tool restrictions:

```
{
  agents: {
    list: [\
      {\
        id: "personal",\
        workspace: "~/.openclaw/workspace-personal",\
        sandbox: {\
          mode: "off",  // No sandbox for personal agent\
        },\
        // No tool restrictions - all tools available\
      },\
      {\
        id: "family",\
        workspace: "~/.openclaw/workspace-family",\
        sandbox: {\
          mode: "all",     // Always sandboxed\
          scope: "agent",  // One container per agent\
          docker: {\
            // Optional one-time setup after container creation\
            setupCommand: "apt-get update && apt-get install -y git curl",\
          },\
        },\
        tools: {\
          allow: ["read"],                    // Only read tool\
          deny: ["exec", "write", "edit", "apply_patch"],    // Deny others\
        },\
      },\
    ],
  },
}
```

`setupCommand` lives under `sandbox.docker` and runs once on container creation. Per-agent `sandbox.docker.*` overrides are ignored when the resolved scope is `"shared"`.

**Benefits:**

- **Security isolation**: restrict tools for untrusted agents.
- **Resource control**: sandbox specific agents while keeping others on host.
- **Flexible policies**: different permissions per agent.

`tools.elevated` is **global** and sender-based; it is not configurable per agent. If you need per-agent boundaries, use `agents.list[].tools` to deny `exec`. For group targeting, use `agents.list[].groupChat.mentionPatterns` so @mentions map cleanly to the intended agent.

See [Multi-agent sandbox and tools](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools) for detailed examples.

## [​](https://docs.openclaw.ai/concepts/multi-agent\#related)  Related

- [ACP agents](https://docs.openclaw.ai/tools/acp-agents) — running external coding harnesses
- [Channel routing](https://docs.openclaw.ai/channels/channel-routing) — how messages route to agents
- [Presence](https://docs.openclaw.ai/concepts/presence) — agent presence and availability
- [Session](https://docs.openclaw.ai/concepts/session) — session isolation and routing
- [Sub-agents](https://docs.openclaw.ai/tools/subagents) — spawning background agent runs

[Compaction](https://docs.openclaw.ai/concepts/compaction) [Specialist lanes](https://docs.openclaw.ai/concepts/parallel-specialist-lanes)

Ctrl+I