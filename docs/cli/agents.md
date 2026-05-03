---
source_url: https://docs.openclaw.ai/cli/agents
title: "Agents - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/agents#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Agents and sessions

Agents

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw agents](https://docs.openclaw.ai/cli/agents#openclaw-agents)
- [Examples](https://docs.openclaw.ai/cli/agents#examples)
- [Routing bindings](https://docs.openclaw.ai/cli/agents#routing-bindings)
- [Binding scope behavior](https://docs.openclaw.ai/cli/agents#binding-scope-behavior)
- [Command surface](https://docs.openclaw.ai/cli/agents#command-surface)
- [agents](https://docs.openclaw.ai/cli/agents#agents)
- [agents list](https://docs.openclaw.ai/cli/agents#agents-list)
- [agents add \[name\]](https://docs.openclaw.ai/cli/agents#agents-add-name)
- [agents bindings](https://docs.openclaw.ai/cli/agents#agents-bindings)
- [agents bind](https://docs.openclaw.ai/cli/agents#agents-bind)
- [agents unbind](https://docs.openclaw.ai/cli/agents#agents-unbind)
- [agents delete <id>](https://docs.openclaw.ai/cli/agents#agents-delete-%3Cid%3E)
- [Identity files](https://docs.openclaw.ai/cli/agents#identity-files)
- [Set identity](https://docs.openclaw.ai/cli/agents#set-identity)
- [Related](https://docs.openclaw.ai/cli/agents#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/agents\#openclaw-agents)  `openclaw agents`

Manage isolated agents (workspaces + auth + routing).Related:

- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)
- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)
- [Skills config](https://docs.openclaw.ai/tools/skills-config): skill visibility configuration.

## [​](https://docs.openclaw.ai/cli/agents\#examples)  Examples

```
openclaw agents list
openclaw agents list --bindings
openclaw agents add work --workspace ~/.openclaw/workspace-work
openclaw agents add ops --workspace ~/.openclaw/workspace-ops --bind telegram:ops --non-interactive
openclaw agents bindings
openclaw agents bind --agent work --bind telegram:ops
openclaw agents unbind --agent work --bind telegram:ops
openclaw agents set-identity --workspace ~/.openclaw/workspace --from-identity
openclaw agents set-identity --agent main --avatar avatars/openclaw.png
openclaw agents delete work
```

## [​](https://docs.openclaw.ai/cli/agents\#routing-bindings)  Routing bindings

Use routing bindings to pin inbound channel traffic to a specific agent.If you also want different visible skills per agent, configure `agents.defaults.skills` and `agents.list[].skills` in `openclaw.json`. See [Skills config](https://docs.openclaw.ai/tools/skills-config) and [Configuration reference](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-skills).List bindings:

```
openclaw agents bindings
openclaw agents bindings --agent work
openclaw agents bindings --json
```

Add bindings:

```
openclaw agents bind --agent work --bind telegram:ops --bind discord:guild-a
```

If you omit `accountId` (`--bind <channel>`), OpenClaw resolves it from channel defaults and plugin setup hooks when available.If you omit `--agent` for `bind` or `unbind`, OpenClaw targets the current default agent.

### [​](https://docs.openclaw.ai/cli/agents\#binding-scope-behavior)  Binding scope behavior

- A binding without `accountId` matches the channel default account only.
- `accountId: "*"` is the channel-wide fallback (all accounts) and is less specific than an explicit account binding.
- If the same agent already has a matching channel binding without `accountId`, and you later bind with an explicit or resolved `accountId`, OpenClaw upgrades that existing binding in place instead of adding a duplicate.

Example:

```
# initial channel-only binding
openclaw agents bind --agent work --bind telegram

# later upgrade to account-scoped binding
openclaw agents bind --agent work --bind telegram:ops
```

After the upgrade, routing for that binding is scoped to `telegram:ops`. If you also want default-account routing, add it explicitly (for example `--bind telegram:default`).Remove bindings:

```
openclaw agents unbind --agent work --bind telegram:ops
openclaw agents unbind --agent work --all
```

`unbind` accepts either `--all` or one or more `--bind` values, not both.

## [​](https://docs.openclaw.ai/cli/agents\#command-surface)  Command surface

### [​](https://docs.openclaw.ai/cli/agents\#agents)  `agents`

Running `openclaw agents` with no subcommand is equivalent to `openclaw agents list`.

### [​](https://docs.openclaw.ai/cli/agents\#agents-list)  `agents list`

Options:

- `--json`
- `--bindings`: include full routing rules, not only per-agent counts/summaries

### [​](https://docs.openclaw.ai/cli/agents\#agents-add-name)  `agents add [name]`

Options:

- `--workspace <dir>`
- `--model <id>`
- `--agent-dir <dir>`
- `--bind <channel[:accountId]>` (repeatable)
- `--non-interactive`
- `--json`

Notes:

- Passing any explicit add flags switches the command into the non-interactive path.
- Non-interactive mode requires both an agent name and `--workspace`.
- `main` is reserved and cannot be used as the new agent id.
- In interactive mode, auth seeding copies only portable static profiles
(`api_key` and static `token` by default). OAuth refresh-token profiles remain
available only by read-through inheritance from the real `main` agent store.
If the configured default agent is not `main`, sign in separately for OAuth
profiles on the new agent.

### [​](https://docs.openclaw.ai/cli/agents\#agents-bindings)  `agents bindings`

Options:

- `--agent <id>`
- `--json`

### [​](https://docs.openclaw.ai/cli/agents\#agents-bind)  `agents bind`

Options:

- `--agent <id>` (defaults to the current default agent)
- `--bind <channel[:accountId]>` (repeatable)
- `--json`

### [​](https://docs.openclaw.ai/cli/agents\#agents-unbind)  `agents unbind`

Options:

- `--agent <id>` (defaults to the current default agent)
- `--bind <channel[:accountId]>` (repeatable)
- `--all`
- `--json`

### [​](https://docs.openclaw.ai/cli/agents\#agents-delete-%3Cid%3E)  `agents delete <id>`

Options:

- `--force`
- `--json`

Notes:

- `main` cannot be deleted.
- Without `--force`, interactive confirmation is required.
- Workspace, agent state, and session transcript directories are moved to Trash, not hard-deleted.
- When the Gateway is reachable, deletion is sent through the Gateway so config and session-store cleanup share the same writer as runtime traffic. If the Gateway cannot be reached, the CLI falls back to the offline local path.
- If another agent’s workspace is the same path, inside this workspace, or contains this workspace,
the workspace is retained and `--json` reports `workspaceRetained`,
`workspaceRetainedReason`, and `workspaceSharedWith`.

## [​](https://docs.openclaw.ai/cli/agents\#identity-files)  Identity files

Each agent workspace can include an `IDENTITY.md` at the workspace root:

- Example path: `~/.openclaw/workspace/IDENTITY.md`
- `set-identity --from-identity` reads from the workspace root (or an explicit `--identity-file`)

Avatar paths resolve relative to the workspace root.

## [​](https://docs.openclaw.ai/cli/agents\#set-identity)  Set identity

`set-identity` writes fields into `agents.list[].identity`:

- `name`
- `theme`
- `emoji`
- `avatar` (workspace-relative path, http(s) URL, or data URI)

Options:

- `--agent <id>`
- `--workspace <dir>`
- `--identity-file <path>`
- `--from-identity`
- `--name <name>`
- `--theme <theme>`
- `--emoji <emoji>`
- `--avatar <value>`
- `--json`

Notes:

- `--agent` or `--workspace` can be used to select the target agent.
- If you rely on `--workspace` and multiple agents share that workspace, the command fails and asks you to pass `--agent`.
- When no explicit identity fields are provided, the command reads identity data from `IDENTITY.md`.

Load from `IDENTITY.md`:

```
openclaw agents set-identity --workspace ~/.openclaw/workspace --from-identity
```

Override fields explicitly:

```
openclaw agents set-identity --agent main --name "OpenClaw" --emoji "🦞" --avatar avatars/openclaw.png
```

Config sample:

```
{
  agents: {
    list: [\
      {\
        id: "main",\
        identity: {\
          name: "OpenClaw",\
          theme: "space lobster",\
          emoji: "🦞",\
          avatar: "avatars/openclaw.png",\
        },\
      },\
    ],
  },
}
```

## [​](https://docs.openclaw.ai/cli/agents\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)
- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)

[Agent](https://docs.openclaw.ai/cli/agent) [Hooks](https://docs.openclaw.ai/cli/hooks)

Ctrl+I