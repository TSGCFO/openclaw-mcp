---
source_url: https://docs.openclaw.ai/tools/multi-agent-sandbox-tools
title: "Multi-agent sandbox and tools - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Agent coordination

Multi-agent sandbox and tools

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Configuration examples](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#configuration-examples)
- [Configuration precedence](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#configuration-precedence)
- [Sandbox config](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#sandbox-config)
- [Tool restrictions](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#tool-restrictions)
- [Migration from single agent](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#migration-from-single-agent)
- [Tool restriction examples](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#tool-restriction-examples)
- [Common pitfall: “non-main”](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#common-pitfall-%E2%80%9Cnon-main%E2%80%9D)
- [Testing](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#testing)
- [Troubleshooting](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#troubleshooting)
- [Related](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Each agent in a multi-agent setup can override the global sandbox and tool policy. This page covers per-agent configuration, precedence rules, and examples.

[**Sandboxing** \\
\\
Backends and modes — full sandbox reference.](https://docs.openclaw.ai/gateway/sandboxing)

[**Sandbox vs tool policy vs elevated** \\
\\
Debug “why is this blocked?”](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated)

[**Elevated mode** \\
\\
Elevated exec for trusted senders.](https://docs.openclaw.ai/tools/elevated)

Auth is scoped by agent: each agent has its own `agentDir` auth store at `~/.openclaw/agents/<agentId>/agent/auth-profiles.json`. Never reuse `agentDir` across agents. Agents can read through to the default/main agent’s auth profiles when they do not have a local profile, but OAuth refresh tokens are not cloned into secondary agent stores. If you copy credentials manually, copy only portable static `api_key` or `token` profiles.

* * *

## [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#configuration-examples)  Configuration examples

Example 1: Personal + restricted family agent

```
{
  "agents": {
    "list": [\
      {\
        "id": "main",\
        "default": true,\
        "name": "Personal Assistant",\
        "workspace": "~/.openclaw/workspace",\
        "sandbox": { "mode": "off" }\
      },\
      {\
        "id": "family",\
        "name": "Family Bot",\
        "workspace": "~/.openclaw/workspace-family",\
        "sandbox": {\
          "mode": "all",\
          "scope": "agent"\
        },\
        "tools": {\
          "allow": ["read"],\
          "deny": ["exec", "write", "edit", "apply_patch", "process", "browser"]\
        }\
      }\
    ]
  },
  "bindings": [\
    {\
      "agentId": "family",\
      "match": {\
        "provider": "whatsapp",\
        "accountId": "*",\
        "peer": {\
          "kind": "group",\
          "id": "120363424282127706@g.us"\
        }\
      }\
    }\
  ]
}
```

**Result:**

- `main` agent: runs on host, full tool access.
- `family` agent: runs in Docker (one container per agent), only `read` tool.

Example 2: Work agent with shared sandbox

```
{
  "agents": {
    "list": [\
      {\
        "id": "personal",\
        "workspace": "~/.openclaw/workspace-personal",\
        "sandbox": { "mode": "off" }\
      },\
      {\
        "id": "work",\
        "workspace": "~/.openclaw/workspace-work",\
        "sandbox": {\
          "mode": "all",\
          "scope": "shared",\
          "workspaceRoot": "/tmp/work-sandboxes"\
        },\
        "tools": {\
          "allow": ["read", "write", "apply_patch", "exec"],\
          "deny": ["browser", "gateway", "discord"]\
        }\
      }\
    ]
  }
}
```

Example 2b: Global coding profile + messaging-only agent

```
{
  "tools": { "profile": "coding" },
  "agents": {
    "list": [\
      {\
        "id": "support",\
        "tools": { "profile": "messaging", "allow": ["slack"] }\
      }\
    ]
  }
}
```

**Result:**

- default agents get coding tools.
- `support` agent is messaging-only (+ Slack tool).

Example 3: Different sandbox modes per agent

```
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "non-main",
        "scope": "session"
      }
    },
    "list": [\
      {\
        "id": "main",\
        "workspace": "~/.openclaw/workspace",\
        "sandbox": {\
          "mode": "off"\
        }\
      },\
      {\
        "id": "public",\
        "workspace": "~/.openclaw/workspace-public",\
        "sandbox": {\
          "mode": "all",\
          "scope": "agent"\
        },\
        "tools": {\
          "allow": ["read"],\
          "deny": ["exec", "write", "edit", "apply_patch"]\
        }\
      }\
    ]
  }
}
```

* * *

## [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#configuration-precedence)  Configuration precedence

When both global (`agents.defaults.*`) and agent-specific (`agents.list[].*`) configs exist:

### [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#sandbox-config)  Sandbox config

Agent-specific settings override global:

```
agents.list[].sandbox.mode > agents.defaults.sandbox.mode
agents.list[].sandbox.scope > agents.defaults.sandbox.scope
agents.list[].sandbox.workspaceRoot > agents.defaults.sandbox.workspaceRoot
agents.list[].sandbox.workspaceAccess > agents.defaults.sandbox.workspaceAccess
agents.list[].sandbox.docker.* > agents.defaults.sandbox.docker.*
agents.list[].sandbox.browser.* > agents.defaults.sandbox.browser.*
agents.list[].sandbox.prune.* > agents.defaults.sandbox.prune.*
```

`agents.list[].sandbox.{docker,browser,prune}.*` overrides `agents.defaults.sandbox.{docker,browser,prune}.*` for that agent (ignored when sandbox scope resolves to `"shared"`).

### [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#tool-restrictions)  Tool restrictions

The filtering order is:

1

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Tool profile

`tools.profile` or `agents.list[].tools.profile`.

2

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Provider tool profile

`tools.byProvider[provider].profile` or `agents.list[].tools.byProvider[provider].profile`.

3

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Global tool policy

`tools.allow` / `tools.deny`.

4

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Provider tool policy

`tools.byProvider[provider].allow/deny`.

5

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Agent-specific tool policy

`agents.list[].tools.allow/deny`.

6

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Agent provider policy

`agents.list[].tools.byProvider[provider].allow/deny`.

7

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Sandbox tool policy

`tools.sandbox.tools` or `agents.list[].tools.sandbox.tools`.

8

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Subagent tool policy

`tools.subagents.tools`, if applicable.

Precedence rules

- Each level can further restrict tools, but cannot grant back denied tools from earlier levels.
- If `agents.list[].tools.sandbox.tools` is set, it replaces `tools.sandbox.tools` for that agent.
- If `agents.list[].tools.profile` is set, it overrides `tools.profile` for that agent.
- Provider tool keys accept either `provider` (e.g. `google-antigravity`) or `provider/model` (e.g. `openai/gpt-5.4`).

Empty allowlist behavior

If any explicit allowlist in that chain leaves the run with no callable tools, OpenClaw stops before submitting the prompt to the model. This is intentional: an agent configured with a missing tool such as `agents.list[].tools.allow: ["query_db"]` should fail loudly until the plugin that registers `query_db` is enabled, not continue as a text-only agent.

Tool policies support `group:*` shorthands that expand to multiple tools. See [Tool groups](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated#tool-groups-shorthands) for the full list.Per-agent elevated overrides (`agents.list[].tools.elevated`) can further restrict elevated exec for specific agents. See [Elevated mode](https://docs.openclaw.ai/tools/elevated) for details.

* * *

## [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#migration-from-single-agent)  Migration from single agent

- Before (single agent)

- After (multi-agent)


```
{
  "agents": {
    "defaults": {
      "workspace": "~/.openclaw/workspace",
      "sandbox": {
        "mode": "non-main"
      }
    }
  },
  "tools": {
    "sandbox": {
      "tools": {
        "allow": ["read", "write", "apply_patch", "exec"],
        "deny": []
      }
    }
  }
}
```

```
{
  "agents": {
    "list": [\
      {\
        "id": "main",\
        "default": true,\
        "workspace": "~/.openclaw/workspace",\
        "sandbox": { "mode": "off" }\
      }\
    ]
  }
}
```

Legacy `agent.*` configs are migrated by `openclaw doctor`; prefer `agents.defaults` \+ `agents.list` going forward.

* * *

## [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#tool-restriction-examples)  Tool restriction examples

- Read-only agent

- Safe execution (no file modifications)

- Communication-only


```
{
  "tools": {
    "allow": ["read"],
    "deny": ["exec", "write", "edit", "apply_patch", "process"]
  }
}
```

```
{
  "tools": {
    "allow": ["read", "exec", "process"],
    "deny": ["write", "edit", "apply_patch", "browser", "gateway"]
  }
}
```

```
{
  "tools": {
    "sessions": { "visibility": "tree" },
    "allow": ["sessions_list", "sessions_send", "sessions_history", "session_status"],
    "deny": ["exec", "write", "edit", "apply_patch", "read", "browser"]
  }
}
```

`sessions_history` in this profile still returns a bounded, sanitized recall view rather than a raw transcript dump. Assistant recall strips thinking tags, `<relevant-memories>` scaffolding, plain-text tool-call XML payloads (including `<tool_call>...</tool_call>`, `<function_call>...</function_call>`, `<tool_calls>...</tool_calls>`, `<function_calls>...</function_calls>`, and truncated tool-call blocks), downgraded tool-call scaffolding, leaked ASCII/full-width model control tokens, and malformed MiniMax tool-call XML before redaction/truncation.

* * *

## [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#common-pitfall-%E2%80%9Cnon-main%E2%80%9D)  Common pitfall: “non-main”

`agents.defaults.sandbox.mode: "non-main"` is based on `session.mainKey` (default `"main"`), not the agent id. Group/channel sessions always get their own keys, so they are treated as non-main and will be sandboxed. If you want an agent to never sandbox, set `agents.list[].sandbox.mode: "off"`.

* * *

## [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#testing)  Testing

After configuring multi-agent sandbox and tools:

1

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Check agent resolution

```
openclaw agents list --bindings
```

2

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Verify sandbox containers

```
docker ps --filter "name=openclaw-sbx-"
```

3

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Test tool restrictions

- Send a message requiring restricted tools.
- Verify the agent cannot use denied tools.

4

[Navigate to header](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools#)

Monitor logs

```
tail -f "${OPENCLAW_STATE_DIR:-$HOME/.openclaw}/logs/gateway.log" | grep -E "routing|sandbox|tools"
```

* * *

## [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#troubleshooting)  Troubleshooting

Agent not sandboxed despite \`mode: 'all'\`

- Check if there’s a global `agents.defaults.sandbox.mode` that overrides it.
- Agent-specific config takes precedence, so set `agents.list[].sandbox.mode: "all"`.

Tools still available despite deny list

- Check tool filtering order: global → agent → sandbox → subagent.
- Each level can only further restrict, not grant back.
- Verify with logs: `[tools] filtering tools for agent:${agentId}`.

Container not isolated per agent

- Set `scope: "agent"` in agent-specific sandbox config.
- Default is `"session"` which creates one container per session.

* * *

## [​](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools\#related)  Related

- [Elevated mode](https://docs.openclaw.ai/tools/elevated)
- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)
- [Sandbox configuration](https://docs.openclaw.ai/gateway/config-agents#agentsdefaultssandbox)
- [Sandbox vs tool policy vs elevated](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated) — debugging “why is this blocked?”
- [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing) — full sandbox reference (modes, scopes, backends, images)
- [Session management](https://docs.openclaw.ai/concepts/session)

[ACP agents — setup](https://docs.openclaw.ai/tools/acp-agents-setup)

Ctrl+I