---
source_url: https://docs.openclaw.ai/cli/sessions
title: "Sessions - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/sessions#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Agents and sessions

Sessions

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw sessions](https://docs.openclaw.ai/cli/sessions#openclaw-sessions)
- [Cleanup maintenance](https://docs.openclaw.ai/cli/sessions#cleanup-maintenance)
- [Related](https://docs.openclaw.ai/cli/sessions#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/sessions\#openclaw-sessions)  `openclaw sessions`

List stored conversation sessions.Session lists are not channel/provider liveness checks. They show persisted
conversation rows from session stores. A quiet Discord, Slack, Telegram, or
other channel can reconnect successfully without creating a new session row
until a message is processed. Use `openclaw channels status --probe`,
`openclaw status --deep`, or `openclaw health --verbose` when you need live
channel connectivity.

```
openclaw sessions
openclaw sessions --agent work
openclaw sessions --all-agents
openclaw sessions --active 120
openclaw sessions --verbose
openclaw sessions --json
```

Scope selection:

- default: configured default agent store
- `--verbose`: verbose logging
- `--agent <id>`: one configured agent store
- `--all-agents`: aggregate all configured agent stores
- `--store <path>`: explicit store path (cannot be combined with `--agent` or `--all-agents`)

Export a trajectory bundle for a stored session:

```
openclaw sessions export-trajectory --session-key "agent:main:telegram:direct:123" --workspace .
openclaw sessions export-trajectory --session-key "agent:main:telegram:direct:123" --output bug-123 --json
```

This is the command path used by the `/export-trajectory` slash command after
the owner approves the exec request. The output directory is always resolved
inside `.openclaw/trajectory-exports/` under the selected workspace.`openclaw sessions --all-agents` reads configured agent stores. Gateway and ACP
session discovery are broader: they also include disk-only stores found under
the default `agents/` root or a templated `session.store` root. Those
discovered stores must resolve to regular `sessions.json` files inside the
agent root; symlinks and out-of-root paths are skipped.JSON examples:`openclaw sessions --all-agents --json`:

```
{
  "path": null,
  "stores": [\
    { "agentId": "main", "path": "/home/user/.openclaw/agents/main/sessions/sessions.json" },\
    { "agentId": "work", "path": "/home/user/.openclaw/agents/work/sessions/sessions.json" }\
  ],
  "allAgents": true,
  "count": 2,
  "activeMinutes": null,
  "sessions": [\
    { "agentId": "main", "key": "agent:main:main", "model": "gpt-5" },\
    { "agentId": "work", "key": "agent:work:main", "model": "claude-opus-4-6" }\
  ]
}
```

## [​](https://docs.openclaw.ai/cli/sessions\#cleanup-maintenance)  Cleanup maintenance

Run maintenance now (instead of waiting for the next write cycle):

```
openclaw sessions cleanup --dry-run
openclaw sessions cleanup --agent work --dry-run
openclaw sessions cleanup --all-agents --dry-run
openclaw sessions cleanup --enforce
openclaw sessions cleanup --enforce --active-key "agent:main:telegram:direct:123"
openclaw sessions cleanup --json
```

`openclaw sessions cleanup` uses `session.maintenance` settings from config:

- Scope note: `openclaw sessions cleanup` maintains session stores, transcripts, and trajectory sidecars. It does not prune cron run logs (`cron/runs/<jobId>.jsonl`), which are managed by `cron.runLog.maxBytes` and `cron.runLog.keepLines` in [Cron configuration](https://docs.openclaw.ai/automation/cron-jobs#configuration) and explained in [Cron maintenance](https://docs.openclaw.ai/automation/cron-jobs#maintenance).
- `--dry-run`: preview how many entries would be pruned/capped without writing.  - In text mode, dry-run prints a per-session action table (`Action`, `Key`, `Age`, `Model`, `Flags`) so you can see what would be kept vs removed.
- `--enforce`: apply maintenance even when `session.maintenance.mode` is `warn`.
- `--fix-missing`: remove entries whose transcript files are missing, even if they would not normally age/count out yet.
- `--active-key <key>`: protect a specific active key from disk-budget eviction. Durable external conversation pointers, such as group sessions and thread-scoped chat sessions, are also kept by age/count/disk-budget maintenance.
- `--agent <id>`: run cleanup for one configured agent store.
- `--all-agents`: run cleanup for all configured agent stores.
- `--store <path>`: run against a specific `sessions.json` file.
- `--json`: print a JSON summary. With `--all-agents`, output includes one summary per store.

`openclaw sessions cleanup --all-agents --dry-run --json`:

```
{
  "allAgents": true,
  "mode": "warn",
  "dryRun": true,
  "stores": [\
    {\
      "agentId": "main",\
      "storePath": "/home/user/.openclaw/agents/main/sessions/sessions.json",\
      "beforeCount": 120,\
      "afterCount": 80,\
      "pruned": 40,\
      "capped": 0\
    },\
    {\
      "agentId": "work",\
      "storePath": "/home/user/.openclaw/agents/work/sessions/sessions.json",\
      "beforeCount": 18,\
      "afterCount": 18,\
      "pruned": 0,\
      "capped": 0\
    }\
  ]
}
```

Related:

- Session config: [Configuration reference](https://docs.openclaw.ai/gateway/config-agents#session)

## [​](https://docs.openclaw.ai/cli/sessions\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Session management](https://docs.openclaw.ai/concepts/session)

[Models](https://docs.openclaw.ai/cli/models) [System](https://docs.openclaw.ai/cli/system)

Ctrl+I