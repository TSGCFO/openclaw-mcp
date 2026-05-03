---
source_url: https://docs.openclaw.ai/channels/broadcast-groups
title: "Broadcast groups - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels/broadcast-groups#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Configuration

Broadcast groups

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Overview](https://docs.openclaw.ai/channels/broadcast-groups#overview)
- [Use cases](https://docs.openclaw.ai/channels/broadcast-groups#use-cases)
- [Configuration](https://docs.openclaw.ai/channels/broadcast-groups#configuration)
- [Basic setup](https://docs.openclaw.ai/channels/broadcast-groups#basic-setup)
- [Processing strategy](https://docs.openclaw.ai/channels/broadcast-groups#processing-strategy)
- [Complete example](https://docs.openclaw.ai/channels/broadcast-groups#complete-example)
- [How it works](https://docs.openclaw.ai/channels/broadcast-groups#how-it-works)
- [Message flow](https://docs.openclaw.ai/channels/broadcast-groups#message-flow)
- [Session isolation](https://docs.openclaw.ai/channels/broadcast-groups#session-isolation)
- [Example: isolated sessions](https://docs.openclaw.ai/channels/broadcast-groups#example-isolated-sessions)
- [Best practices](https://docs.openclaw.ai/channels/broadcast-groups#best-practices)
- [Compatibility](https://docs.openclaw.ai/channels/broadcast-groups#compatibility)
- [Providers](https://docs.openclaw.ai/channels/broadcast-groups#providers)
- [Routing](https://docs.openclaw.ai/channels/broadcast-groups#routing)
- [Troubleshooting](https://docs.openclaw.ai/channels/broadcast-groups#troubleshooting)
- [Examples](https://docs.openclaw.ai/channels/broadcast-groups#examples)
- [API reference](https://docs.openclaw.ai/channels/broadcast-groups#api-reference)
- [Config schema](https://docs.openclaw.ai/channels/broadcast-groups#config-schema)
- [Fields](https://docs.openclaw.ai/channels/broadcast-groups#fields)
- [Limitations](https://docs.openclaw.ai/channels/broadcast-groups#limitations)
- [Future enhancements](https://docs.openclaw.ai/channels/broadcast-groups#future-enhancements)
- [Related](https://docs.openclaw.ai/channels/broadcast-groups#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

**Status:** Experimental. Added in 2026.1.9.

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#overview)  Overview

Broadcast Groups enable multiple agents to process and respond to the same message simultaneously. This allows you to create specialized agent teams that work together in a single WhatsApp group or DM — all using one phone number.Current scope: **WhatsApp only** (web channel).Broadcast groups are evaluated after channel allowlists and group activation rules. In WhatsApp groups, this means broadcasts happen when OpenClaw would normally reply (for example: on mention, depending on your group settings).

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#use-cases)  Use cases

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

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#configuration)  Configuration

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#basic-setup)  Basic setup

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

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#processing-strategy)  Processing strategy

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

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#complete-example)  Complete example

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

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#how-it-works)  How it works

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#message-flow)  Message flow

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

Broadcast groups do not bypass channel allowlists or group activation rules (mentions/commands/etc). They only change _which agents run_ when a message is eligible for processing.

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#session-isolation)  Session isolation

Each agent in a broadcast group maintains completely separate:

- **Session keys** (`agent:alfred:whatsapp:group:120363...` vs `agent:baerbel:whatsapp:group:120363...`)
- **Conversation history** (agent doesn’t see other agents’ messages)
- **Workspace** (separate sandboxes if configured)
- **Tool access** (different allow/deny lists)
- **Memory/context** (separate IDENTITY.md, SOUL.md, etc.)
- **Group context buffer** (recent group messages used for context) is shared per peer, so all broadcast agents see the same context when triggered

This allows each agent to have:

- Different personalities
- Different tool access (e.g., read-only vs. read-write)
- Different models (e.g., opus vs. sonnet)
- Different skills installed

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#example-isolated-sessions)  Example: isolated sessions

In group `120363403215116621@g.us` with agents `["alfred", "baerbel"]`:

- Alfred's context

- Bärbel's context


```
Session: agent:alfred:whatsapp:group:120363403215116621@g.us
History: [user message, alfred's previous responses]
Workspace: /Users/user/openclaw-alfred/
Tools: read, write, exec
```

```
Session: agent:baerbel:whatsapp:group:120363403215116621@g.us
History: [user message, baerbel's previous responses]
Workspace: /Users/user/openclaw-baerbel/
Tools: read only
```

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#best-practices)  Best practices

1\. Keep agents focused

Design each agent with a single, clear responsibility:

```
{
  "broadcast": {
    "DEV_GROUP": ["formatter", "linter", "tester"]
  }
}
```

✅ **Good:** Each agent has one job. ❌ **Bad:** One generic “dev-helper” agent.

2\. Use descriptive names

Make it clear what each agent does:

```
{
  "agents": {
    "security-scanner": { "name": "Security Scanner" },
    "code-formatter": { "name": "Code Formatter" },
    "test-generator": { "name": "Test Generator" }
  }
}
```

3\. Configure different tool access

Give agents only the tools they need:

```
{
  "agents": {
    "reviewer": {
      "tools": { "allow": ["read", "exec"] } // Read-only
    },
    "fixer": {
      "tools": { "allow": ["read", "write", "edit", "exec"] } // Read-write
    }
  }
}
```

4\. Monitor performance

With many agents, consider:

- Using `"strategy": "parallel"` (default) for speed
- Limiting broadcast groups to 5-10 agents
- Using faster models for simpler agents

5\. Handle failures gracefully

Agents fail independently. One agent’s error doesn’t block others:

```
Message → [Agent A ✓, Agent B ✗ error, Agent C ✓]
Result: Agent A and C respond, Agent B logs error
```

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#compatibility)  Compatibility

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#providers)  Providers

Broadcast groups currently work with:

- ✅ WhatsApp (implemented)
- 🚧 Telegram (planned)
- 🚧 Discord (planned)
- 🚧 Slack (planned)

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#routing)  Routing

Broadcast groups work alongside existing routing:

```
{
  "bindings": [\
    {\
      "match": { "channel": "whatsapp", "peer": { "kind": "group", "id": "GROUP_A" } },\
      "agentId": "alfred"\
    }\
  ],
  "broadcast": {
    "GROUP_B": ["agent1", "agent2"]
  }
}
```

- `GROUP_A`: Only alfred responds (normal routing).
- `GROUP_B`: agent1 AND agent2 respond (broadcast).

**Precedence:**`broadcast` takes priority over `bindings`.

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#troubleshooting)  Troubleshooting

Agents not responding

**Check:**

1. Agent IDs exist in `agents.list`.
2. Peer ID format is correct (e.g., `120363403215116621@g.us`).
3. Agents are not in deny lists.

**Debug:**

```
tail -f ~/.openclaw/logs/gateway.log | grep broadcast
```

Only one agent responding

**Cause:** Peer ID might be in `bindings` but not `broadcast`.**Fix:** Add to broadcast config or remove from bindings.

Performance issues

If slow with many agents:

- Reduce number of agents per group.
- Use lighter models (sonnet instead of opus).
- Check sandbox startup time.

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#examples)  Examples

Example 1: Code review team

```
{
  "broadcast": {
    "strategy": "parallel",
    "120363403215116621@g.us": [\
      "code-formatter",\
      "security-scanner",\
      "test-coverage",\
      "docs-checker"\
    ]
  },
  "agents": {
    "list": [\
      {\
        "id": "code-formatter",\
        "workspace": "~/agents/formatter",\
        "tools": { "allow": ["read", "write"] }\
      },\
      {\
        "id": "security-scanner",\
        "workspace": "~/agents/security",\
        "tools": { "allow": ["read", "exec"] }\
      },\
      {\
        "id": "test-coverage",\
        "workspace": "~/agents/testing",\
        "tools": { "allow": ["read", "exec"] }\
      },\
      { "id": "docs-checker", "workspace": "~/agents/docs", "tools": { "allow": ["read"] } }\
    ]
  }
}
```

**User sends:** Code snippet.**Responses:**

- code-formatter: “Fixed indentation and added type hints”
- security-scanner: “⚠️ SQL injection vulnerability in line 12”
- test-coverage: “Coverage is 45%, missing tests for error cases”
- docs-checker: “Missing docstring for function `process_data`”

Example 2: Multi-language support

```
{
  "broadcast": {
    "strategy": "sequential",
    "+15555550123": ["detect-language", "translator-en", "translator-de"]
  },
  "agents": {
    "list": [\
      { "id": "detect-language", "workspace": "~/agents/lang-detect" },\
      { "id": "translator-en", "workspace": "~/agents/translate-en" },\
      { "id": "translator-de", "workspace": "~/agents/translate-de" }\
    ]
  }
}
```

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#api-reference)  API reference

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#config-schema)  Config schema

```
interface OpenClawConfig {
  broadcast?: {
    strategy?: "parallel" | "sequential";
    [peerId: string]: string[];
  };
}
```

### [​](https://docs.openclaw.ai/channels/broadcast-groups\#fields)  Fields

[​](https://docs.openclaw.ai/channels/broadcast-groups#param-strategy)

strategy

"parallel" \| "sequential"

default:"\\"parallel\\""

How to process agents. `parallel` runs all agents simultaneously; `sequential` runs them in array order.

[​](https://docs.openclaw.ai/channels/broadcast-groups#param-peer-id)

\[peerId\]

string\[\]

WhatsApp group JID, E.164 number, or other peer ID. Value is the array of agent IDs that should process messages.

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#limitations)  Limitations

1. **Max agents:** No hard limit, but 10+ agents may be slow.
2. **Shared context:** Agents don’t see each other’s responses (by design).
3. **Message ordering:** Parallel responses may arrive in any order.
4. **Rate limits:** All agents count toward WhatsApp rate limits.

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#future-enhancements)  Future enhancements

Planned features:

- [ ]  Shared context mode (agents see each other’s responses)
- [ ]  Agent coordination (agents can signal each other)
- [ ]  Dynamic agent selection (choose agents based on message content)
- [ ]  Agent priorities (some agents respond before others)

## [​](https://docs.openclaw.ai/channels/broadcast-groups\#related)  Related

- [Channel routing](https://docs.openclaw.ai/channels/channel-routing)
- [Groups](https://docs.openclaw.ai/channels/groups)
- [Multi-agent sandbox tools](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools)
- [Pairing](https://docs.openclaw.ai/channels/pairing)
- [Session management](https://docs.openclaw.ai/concepts/session)

[Groups](https://docs.openclaw.ai/channels/groups) [Channel routing](https://docs.openclaw.ai/channels/channel-routing)

Ctrl+I