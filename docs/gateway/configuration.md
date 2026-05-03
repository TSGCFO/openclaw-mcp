---
source_url: https://docs.openclaw.ai/gateway/configuration
title: "Configuration - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/configuration#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Configuration

Configuration

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Minimal config](https://docs.openclaw.ai/gateway/configuration#minimal-config)
- [Editing config](https://docs.openclaw.ai/gateway/configuration#editing-config)
- [Strict validation](https://docs.openclaw.ai/gateway/configuration#strict-validation)
- [Common tasks](https://docs.openclaw.ai/gateway/configuration#common-tasks)
- [Config hot reload](https://docs.openclaw.ai/gateway/configuration#config-hot-reload)
- [Reload modes](https://docs.openclaw.ai/gateway/configuration#reload-modes)
- [What hot-applies vs what needs a restart](https://docs.openclaw.ai/gateway/configuration#what-hot-applies-vs-what-needs-a-restart)
- [Reload planning](https://docs.openclaw.ai/gateway/configuration#reload-planning)
- [Config RPC (programmatic updates)](https://docs.openclaw.ai/gateway/configuration#config-rpc-programmatic-updates)
- [Environment variables](https://docs.openclaw.ai/gateway/configuration#environment-variables)
- [Full reference](https://docs.openclaw.ai/gateway/configuration#full-reference)
- [Related](https://docs.openclaw.ai/gateway/configuration#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw reads an optional **JSON5** config from `~/.openclaw/openclaw.json`.
The active config path must be a regular file. Symlinked `openclaw.json`
layouts are unsupported for OpenClaw-owned writes; an atomic write may replace
the path instead of preserving the symlink. If you keep config outside the
default state directory, point `OPENCLAW_CONFIG_PATH` directly at the real file.If the file is missing, OpenClaw uses safe defaults. Common reasons to add a config:

- Connect channels and control who can message the bot
- Set models, tools, sandboxing, or automation (cron, hooks)
- Tune sessions, media, networking, or UI

See the [full reference](https://docs.openclaw.ai/gateway/configuration-reference) for every available field.Agents and automation should use `config.schema.lookup` for exact field-level
docs before editing config. Use this page for task-oriented guidance and
[Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference) for the broader
field map and defaults.

**New to configuration?** Start with `openclaw onboard` for interactive setup, or check out the [Configuration Examples](https://docs.openclaw.ai/gateway/configuration-examples) guide for complete copy-paste configs.

## [​](https://docs.openclaw.ai/gateway/configuration\#minimal-config)  Minimal config

```
// ~/.openclaw/openclaw.json
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

## [​](https://docs.openclaw.ai/gateway/configuration\#editing-config)  Editing config

- Interactive wizard

- CLI (one-liners)

- Control UI

- Direct edit


```
openclaw onboard       # full onboarding flow
openclaw configure     # config wizard
```

```
openclaw config get agents.defaults.workspace
openclaw config set agents.defaults.heartbeat.every "2h"
openclaw config unset plugins.entries.brave.config.webSearch.apiKey
```

Open [http://127.0.0.1:18789](http://127.0.0.1:18789/) and use the **Config** tab.
The Control UI renders a form from the live config schema, including field
`title` / `description` docs metadata plus plugin and channel schemas when
available, with a **Raw JSON** editor as an escape hatch. For drill-down
UIs and other tooling, the gateway also exposes `config.schema.lookup` to
fetch one path-scoped schema node plus immediate child summaries.

Edit `~/.openclaw/openclaw.json` directly. The Gateway watches the file and applies changes automatically (see [hot reload](https://docs.openclaw.ai/gateway/configuration#config-hot-reload)).

## [​](https://docs.openclaw.ai/gateway/configuration\#strict-validation)  Strict validation

OpenClaw only accepts configurations that fully match the schema. Unknown keys, malformed types, or invalid values cause the Gateway to **refuse to start**. The only root-level exception is `$schema` (string), so editors can attach JSON Schema metadata.

`openclaw config schema` prints the canonical JSON Schema used by Control UI
and validation. `config.schema.lookup` fetches a single path-scoped node plus
child summaries for drill-down tooling. Field `title`/`description` docs metadata
carries through nested objects, wildcard (`*`), array-item (`[]`), and `anyOf`/
`oneOf`/`allOf` branches. Runtime plugin and channel schemas merge in when the
manifest registry is loaded.When validation fails:

- The Gateway does not boot
- Only diagnostic commands work (`openclaw doctor`, `openclaw logs`, `openclaw health`, `openclaw status`)
- Run `openclaw doctor` to see exact issues
- Run `openclaw doctor --fix` (or `--yes`) to apply repairs

The Gateway keeps a trusted last-known-good copy after each successful startup.
If `openclaw.json` later fails validation (or drops `gateway.mode`, shrinks
sharply, or has a stray log line prepended), OpenClaw preserves the broken file
as `.clobbered.*`, restores the last-known-good copy, and logs the recovery
reason. The next agent turn also receives a system-event warning so the main
agent does not blindly rewrite the restored config. Promotion to last-known-good
is skipped when a candidate contains redacted secret placeholders such as `***`.
When every validation issue is scoped to `plugins.entries.<id>...`, OpenClaw
does not perform whole-file recovery. It keeps the current config active and
surfaces the plugin-local failure so a plugin schema or host-version mismatch
cannot roll back unrelated user settings.

## [​](https://docs.openclaw.ai/gateway/configuration\#common-tasks)  Common tasks

Set up a channel (WhatsApp, Telegram, Discord, etc.)

Each channel has its own config section under `channels.<provider>`. See the dedicated channel page for setup steps:

- [WhatsApp](https://docs.openclaw.ai/channels/whatsapp) — `channels.whatsapp`
- [Telegram](https://docs.openclaw.ai/channels/telegram) — `channels.telegram`
- [Discord](https://docs.openclaw.ai/channels/discord) — `channels.discord`
- [Feishu](https://docs.openclaw.ai/channels/feishu) — `channels.feishu`
- [Google Chat](https://docs.openclaw.ai/channels/googlechat) — `channels.googlechat`
- [Microsoft Teams](https://docs.openclaw.ai/channels/msteams) — `channels.msteams`
- [Slack](https://docs.openclaw.ai/channels/slack) — `channels.slack`
- [Signal](https://docs.openclaw.ai/channels/signal) — `channels.signal`
- [iMessage](https://docs.openclaw.ai/channels/imessage) — `channels.imessage`
- [Mattermost](https://docs.openclaw.ai/channels/mattermost) — `channels.mattermost`

All channels share the same DM policy pattern:

```
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",   // pairing | allowlist | open | disabled
      allowFrom: ["tg:123"], // only for allowlist/open
    },
  },
}
```

Choose and configure models

Set the primary model and optional fallbacks:

```
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-6",
        fallbacks: ["openai/gpt-5.4"],
      },
      models: {
        "anthropic/claude-sonnet-4-6": { alias: "Sonnet" },
        "openai/gpt-5.4": { alias: "GPT" },
      },
    },
  },
}
```

- `agents.defaults.models` defines the model catalog and acts as the allowlist for `/model`.
- Use `openclaw config set agents.defaults.models '<json>' --strict-json --merge` to add allowlist entries without removing existing models. Plain replacements that would remove entries are rejected unless you pass `--replace`.
- Model refs use `provider/model` format (e.g. `anthropic/claude-opus-4-6`).
- `agents.defaults.imageMaxDimensionPx` controls transcript/tool image downscaling (default `1200`); lower values usually reduce vision-token usage on screenshot-heavy runs.
- See [Models CLI](https://docs.openclaw.ai/concepts/models) for switching models in chat and [Model Failover](https://docs.openclaw.ai/concepts/model-failover) for auth rotation and fallback behavior.
- For custom/self-hosted providers, see [Custom providers](https://docs.openclaw.ai/gateway/config-tools#custom-providers-and-base-urls) in the reference.

Control who can message the bot

DM access is controlled per channel via `dmPolicy`:

- `"pairing"` (default): unknown senders get a one-time pairing code to approve
- `"allowlist"`: only senders in `allowFrom` (or the paired allow store)
- `"open"`: allow all inbound DMs (requires `allowFrom: ["*"]`)
- `"disabled"`: ignore all DMs

For groups, use `groupPolicy` \+ `groupAllowFrom` or channel-specific allowlists.See the [full reference](https://docs.openclaw.ai/gateway/config-channels#dm-and-group-access) for per-channel details.

Set up group chat mention gating

Group messages default to **require mention**. Configure trigger patterns per agent, and keep visible room replies on the default message-tool path unless you intentionally want legacy automatic final replies:

```
{
  messages: {
    visibleReplies: "automatic", // set "message_tool" to require message-tool sends everywhere
    groupChat: {
      visibleReplies: "message_tool", // default; use "automatic" for legacy room replies
    },
  },
  agents: {
    list: [\
      {\
        id: "main",\
        groupChat: {\
          mentionPatterns: ["@openclaw", "openclaw"],\
        },\
      },\
    ],
  },
  channels: {
    whatsapp: {
      groups: { "*": { requireMention: true } },
    },
  },
}
```

- **Metadata mentions**: native @-mentions (WhatsApp tap-to-mention, Telegram @bot, etc.)
- **Text patterns**: safe regex patterns in `mentionPatterns`
- **Visible replies**: `messages.visibleReplies` can require message-tool sends globally; `messages.groupChat.visibleReplies` overrides that for groups/channels.
- See [full reference](https://docs.openclaw.ai/gateway/config-channels#group-chat-mention-gating) for visible reply modes, per-channel overrides, and self-chat mode.

Restrict skills per agent

Use `agents.defaults.skills` for a shared baseline, then override specific
agents with `agents.list[].skills`:

```
{
  agents: {
    defaults: {
      skills: ["github", "weather"],
    },
    list: [\
      { id: "writer" }, // inherits github, weather\
      { id: "docs", skills: ["docs-search"] }, // replaces defaults\
      { id: "locked-down", skills: [] }, // no skills\
    ],
  },
}
```

- Omit `agents.defaults.skills` for unrestricted skills by default.
- Omit `agents.list[].skills` to inherit the defaults.
- Set `agents.list[].skills: []` for no skills.
- See [Skills](https://docs.openclaw.ai/tools/skills), [Skills config](https://docs.openclaw.ai/tools/skills-config), and
the [Configuration Reference](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-skills).

Tune gateway channel health monitoring

Control how aggressively the gateway restarts channels that look stale:

```
{
  gateway: {
    channelHealthCheckMinutes: 5,
    channelStaleEventThresholdMinutes: 30,
    channelMaxRestartsPerHour: 10,
  },
  channels: {
    telegram: {
      healthMonitor: { enabled: false },
      accounts: {
        alerts: {
          healthMonitor: { enabled: true },
        },
      },
    },
  },
}
```

- Set `gateway.channelHealthCheckMinutes: 0` to disable health-monitor restarts globally.
- `channelStaleEventThresholdMinutes` should be greater than or equal to the check interval.
- Use `channels.<provider>.healthMonitor.enabled` or `channels.<provider>.accounts.<id>.healthMonitor.enabled` to disable auto-restarts for one channel or account without disabling the global monitor.
- See [Health Checks](https://docs.openclaw.ai/gateway/health) for operational debugging and the [full reference](https://docs.openclaw.ai/gateway/configuration-reference#gateway) for all fields.

Tune gateway WebSocket handshake timeout

Give local clients more time to complete the pre-auth WebSocket handshake on
loaded or low-powered hosts:

```
{
  gateway: {
    handshakeTimeoutMs: 30000,
  },
}
```

- Default is `15000` milliseconds.
- `OPENCLAW_HANDSHAKE_TIMEOUT_MS` still takes precedence for one-off service or shell overrides.
- Prefer fixing startup/event-loop stalls first; this knob is for hosts that are healthy but slow during warmup.

Configure sessions and resets

Sessions control conversation continuity and isolation:

```
{
  session: {
    dmScope: "per-channel-peer",  // recommended for multi-user
    threadBindings: {
      enabled: true,
      idleHours: 24,
      maxAgeHours: 0,
    },
    reset: {
      mode: "daily",
      atHour: 4,
      idleMinutes: 120,
    },
  },
}
```

- `dmScope`: `main` (shared) \| `per-peer` \| `per-channel-peer` \| `per-account-channel-peer`
- `threadBindings`: global defaults for thread-bound session routing (Discord supports `/focus`, `/unfocus`, `/agents`, `/session idle`, and `/session max-age`).
- See [Session Management](https://docs.openclaw.ai/concepts/session) for scoping, identity links, and send policy.
- See [full reference](https://docs.openclaw.ai/gateway/config-agents#session) for all fields.

Enable sandboxing

Run agent sessions in isolated sandbox runtimes:

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",  // off | non-main | all
        scope: "agent",    // session | agent | shared
      },
    },
  },
}
```

Build the image first — from a source checkout run `scripts/sandbox-setup.sh`, or from an npm install see the inline `docker build` command in [Sandboxing § Images and setup](https://docs.openclaw.ai/gateway/sandboxing#images-and-setup).See [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing) for the full guide and [full reference](https://docs.openclaw.ai/gateway/config-agents#agentsdefaultssandbox) for all options.

Enable relay-backed push for official iOS builds

Relay-backed push is configured in `openclaw.json`.Set this in gateway config:

```
{
  gateway: {
    push: {
      apns: {
        relay: {
          baseUrl: "https://relay.example.com",
          // Optional. Default: 10000
          timeoutMs: 10000,
        },
      },
    },
  },
}
```

CLI equivalent:

```
openclaw config set gateway.push.apns.relay.baseUrl https://relay.example.com
```

What this does:

- Lets the gateway send `push.test`, wake nudges, and reconnect wakes through the external relay.
- Uses a registration-scoped send grant forwarded by the paired iOS app. The gateway does not need a deployment-wide relay token.
- Binds each relay-backed registration to the gateway identity that the iOS app paired with, so another gateway cannot reuse the stored registration.
- Keeps local/manual iOS builds on direct APNs. Relay-backed sends apply only to official distributed builds that registered through the relay.
- Must match the relay base URL baked into the official/TestFlight iOS build, so registration and send traffic reach the same relay deployment.

End-to-end flow:

1. Install an official/TestFlight iOS build that was compiled with the same relay base URL.
2. Configure `gateway.push.apns.relay.baseUrl` on the gateway.
3. Pair the iOS app to the gateway and let both node and operator sessions connect.
4. The iOS app fetches the gateway identity, registers with the relay using App Attest plus the app receipt, and then publishes the relay-backed `push.apns.register` payload to the paired gateway.
5. The gateway stores the relay handle and send grant, then uses them for `push.test`, wake nudges, and reconnect wakes.

Operational notes:

- If you switch the iOS app to a different gateway, reconnect the app so it can publish a new relay registration bound to that gateway.
- If you ship a new iOS build that points at a different relay deployment, the app refreshes its cached relay registration instead of reusing the old relay origin.

Compatibility note:

- `OPENCLAW_APNS_RELAY_BASE_URL` and `OPENCLAW_APNS_RELAY_TIMEOUT_MS` still work as temporary env overrides.
- `OPENCLAW_APNS_RELAY_ALLOW_HTTP=true` remains a loopback-only development escape hatch; do not persist HTTP relay URLs in config.

See [iOS App](https://docs.openclaw.ai/platforms/ios#relay-backed-push-for-official-builds) for the end-to-end flow and [Authentication and trust flow](https://docs.openclaw.ai/platforms/ios#authentication-and-trust-flow) for the relay security model.

Set up heartbeat (periodic check-ins)

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last",
      },
    },
  },
}
```

- `every`: duration string (`30m`, `2h`). Set `0m` to disable.
- `target`: `last` \| `none` \| `<channel-id>` (for example `discord`, `matrix`, `telegram`, or `whatsapp`)
- `directPolicy`: `allow` (default) or `block` for DM-style heartbeat targets
- See [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) for the full guide.

Configure cron jobs

```
{
  cron: {
    enabled: true,
    maxConcurrentRuns: 2, // cron dispatch + isolated cron agent-turn execution
    sessionRetention: "24h",
    runLog: {
      maxBytes: "2mb",
      keepLines: 2000,
    },
  },
}
```

- `sessionRetention`: prune completed isolated run sessions from `sessions.json` (default `24h`; set `false` to disable).
- `runLog`: prune `cron/runs/<jobId>.jsonl` by size and retained lines.
- See [Cron jobs](https://docs.openclaw.ai/automation/cron-jobs) for feature overview and CLI examples.

Set up webhooks (hooks)

Enable HTTP webhook endpoints on the Gateway:

```
{
  hooks: {
    enabled: true,
    token: "shared-secret",
    path: "/hooks",
    defaultSessionKey: "hook:ingress",
    allowRequestSessionKey: false,
    allowedSessionKeyPrefixes: ["hook:"],
    mappings: [\
      {\
        match: { path: "gmail" },\
        action: "agent",\
        agentId: "main",\
        deliver: true,\
      },\
    ],
  },
}
```

Security note:

- Treat all hook/webhook payload content as untrusted input.
- Use a dedicated `hooks.token`; do not reuse the shared Gateway token.
- Hook auth is header-only (`Authorization: Bearer ...` or `x-openclaw-token`); query-string tokens are rejected.
- `hooks.path` cannot be `/`; keep webhook ingress on a dedicated subpath such as `/hooks`.
- Keep unsafe-content bypass flags disabled (`hooks.gmail.allowUnsafeExternalContent`, `hooks.mappings[].allowUnsafeExternalContent`) unless doing tightly scoped debugging.
- If you enable `hooks.allowRequestSessionKey`, also set `hooks.allowedSessionKeyPrefixes` to bound caller-selected session keys.
- For hook-driven agents, prefer strong modern model tiers and strict tool policy (for example messaging-only plus sandboxing where possible).

See [full reference](https://docs.openclaw.ai/gateway/configuration-reference#hooks) for all mapping options and Gmail integration.

Configure multi-agent routing

Run multiple isolated agents with separate workspaces and sessions:

```
{
  agents: {
    list: [\
      { id: "home", default: true, workspace: "~/.openclaw/workspace-home" },\
      { id: "work", workspace: "~/.openclaw/workspace-work" },\
    ],
  },
  bindings: [\
    { agentId: "home", match: { channel: "whatsapp", accountId: "personal" } },\
    { agentId: "work", match: { channel: "whatsapp", accountId: "biz" } },\
  ],
}
```

See [Multi-Agent](https://docs.openclaw.ai/concepts/multi-agent) and [full reference](https://docs.openclaw.ai/gateway/config-agents#multi-agent-routing) for binding rules and per-agent access profiles.

Split config into multiple files ($include)

Use `$include` to organize large configs:

```
// ~/.openclaw/openclaw.json
{
  gateway: { port: 18789 },
  agents: { $include: "./agents.json5" },
  broadcast: {
    $include: ["./clients/a.json5", "./clients/b.json5"],
  },
}
```

- **Single file**: replaces the containing object
- **Array of files**: deep-merged in order (later wins)
- **Sibling keys**: merged after includes (override included values)
- **Nested includes**: supported up to 10 levels deep
- **Relative paths**: resolved relative to the including file
- **OpenClaw-owned writes**: when a write changes only one top-level section
backed by a single-file include such as `plugins: { $include: "./plugins.json5" }`,
OpenClaw updates that included file and leaves `openclaw.json` intact
- **Unsupported write-through**: root includes, include arrays, and includes
with sibling overrides fail closed for OpenClaw-owned writes instead of
flattening the config
- **Confinement**: `$include` paths must resolve under the directory holding
`openclaw.json`. To share a tree across machines or users, set
`OPENCLAW_INCLUDE_ROOTS` to a path-list (`:` on POSIX, `;` on Windows) of
additional directories that includes may reference. Symlinks are resolved
and re-checked, so a path that lexically lives in a config dir but whose
real target escapes every allowed root is still rejected.
- **Error handling**: clear errors for missing files, parse errors, and circular includes

## [​](https://docs.openclaw.ai/gateway/configuration\#config-hot-reload)  Config hot reload

The Gateway watches `~/.openclaw/openclaw.json` and applies changes automatically — no manual restart needed for most settings.Direct file edits are treated as untrusted until they validate. The watcher waits
for editor temp-write/rename churn to settle, reads the final file, and rejects
invalid external edits by restoring the last-known-good config. OpenClaw-owned
config writes use the same schema gate before writing; destructive clobbers such
as dropping `gateway.mode` or shrinking the file by more than half are rejected
and saved as `.rejected.*` for inspection.Plugin-local validation failures are the exception: if all issues are under
`plugins.entries.<id>...`, reload keeps the current config and reports the plugin
issue instead of restoring `.last-good`.If you see `Config auto-restored from last-known-good` or
`config reload restored last-known-good config` in logs, inspect the matching
`.clobbered.*` file next to `openclaw.json`, fix the rejected payload, then run
`openclaw config validate`. See [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting#gateway-restored-last-known-good-config)
for the recovery checklist.

### [​](https://docs.openclaw.ai/gateway/configuration\#reload-modes)  Reload modes

| Mode | Behavior |
| --- | --- |
| **`hybrid`** (default) | Hot-applies safe changes instantly. Automatically restarts for critical ones. |
| **`hot`** | Hot-applies safe changes only. Logs a warning when a restart is needed — you handle it. |
| **`restart`** | Restarts the Gateway on any config change, safe or not. |
| **`off`** | Disables file watching. Changes take effect on the next manual restart. |

```
{
  gateway: {
    reload: { mode: "hybrid", debounceMs: 300 },
  },
}
```

### [​](https://docs.openclaw.ai/gateway/configuration\#what-hot-applies-vs-what-needs-a-restart)  What hot-applies vs what needs a restart

Most fields hot-apply without downtime. In `hybrid` mode, restart-required changes are handled automatically.

| Category | Fields | Restart needed? |
| --- | --- | --- |
| Channels | `channels.*`, `web` (WhatsApp) — all built-in and plugin channels | No |
| Agent & models | `agent`, `agents`, `models`, `routing` | No |
| Automation | `hooks`, `cron`, `agent.heartbeat` | No |
| Sessions & messages | `session`, `messages` | No |
| Tools & media | `tools`, `browser`, `skills`, `mcp`, `audio`, `talk` | No |
| UI & misc | `ui`, `logging`, `identity`, `bindings` | No |
| Gateway server | `gateway.*` (port, bind, auth, tailscale, TLS, HTTP) | **Yes** |
| Infrastructure | `discovery`, `canvasHost`, `plugins` | **Yes** |

`gateway.reload` and `gateway.remote` are exceptions — changing them does **not** trigger a restart.

### [​](https://docs.openclaw.ai/gateway/configuration\#reload-planning)  Reload planning

When you edit a source file that is referenced through `$include`, OpenClaw plans
the reload from the source-authored layout, not the flattened in-memory view.
That keeps hot-reload decisions (hot-apply vs restart) predictable even when a
single top-level section lives in its own included file such as
`plugins: { $include: "./plugins.json5" }`. Reload planning fails closed if the
source layout is ambiguous.

## [​](https://docs.openclaw.ai/gateway/configuration\#config-rpc-programmatic-updates)  Config RPC (programmatic updates)

For tooling that writes config over the gateway API, prefer this flow:

- `config.schema.lookup` to inspect one subtree (shallow schema node + child
summaries)
- `config.get` to fetch the current snapshot plus `hash`
- `config.patch` for partial updates (JSON merge patch: objects merge, `null`
deletes, arrays replace)
- `config.apply` only when you intend to replace the entire config
- `update.run` for explicit self-update plus restart
- `update.status` to inspect the latest update restart sentinel and verify the running version after a restart

Agents should treat `config.schema.lookup` as the first stop for exact
field-level docs and constraints. Use [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference)
when they need the broader config map, defaults, or links to dedicated
subsystem references.

Control-plane writes (`config.apply`, `config.patch`, `update.run`) are
rate-limited to 3 requests per 60 seconds per `deviceId+clientIp`. Restart
requests coalesce and then enforce a 30-second cooldown between restart cycles.
`update.status` is read-only but admin-scoped because the restart sentinel can
include update step summaries and command output tails.

Example partial patch:

```
openclaw gateway call config.get --params '{}'  # capture payload.hash
openclaw gateway call config.patch --params '{
  "raw": "{ channels: { telegram: { groups: { \"*\": { requireMention: false } } } } }",
  "baseHash": "<hash>"
}'
```

Both `config.apply` and `config.patch` accept `raw`, `baseHash`, `sessionKey`,
`note`, and `restartDelayMs`. `baseHash` is required for both methods when a
config already exists.

## [​](https://docs.openclaw.ai/gateway/configuration\#environment-variables)  Environment variables

OpenClaw reads env vars from the parent process plus:

- `.env` from the current working directory (if present)
- `~/.openclaw/.env` (global fallback)

Neither file overrides existing env vars. You can also set inline env vars in config:

```
{
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: { GROQ_API_KEY: "gsk-..." },
  },
}
```

Shell env import (optional)

If enabled and expected keys aren’t set, OpenClaw runs your login shell and imports only the missing keys:

```
{
  env: {
    shellEnv: { enabled: true, timeoutMs: 15000 },
  },
}
```

Env var equivalent: `OPENCLAW_LOAD_SHELL_ENV=1`

Env var substitution in config values

Reference env vars in any config string value with `${VAR_NAME}`:

```
{
  gateway: { auth: { token: "${OPENCLAW_GATEWAY_TOKEN}" } },
  models: { providers: { custom: { apiKey: "${CUSTOM_API_KEY}" } } },
}
```

Rules:

- Only uppercase names matched: `[A-Z_][A-Z0-9_]*`
- Missing/empty vars throw an error at load time
- Escape with `$${VAR}` for literal output
- Works inside `$include` files
- Inline substitution: `"${BASE}/v1"` → `"https://api.example.com/v1"`

Secret refs (env, file, exec)

For fields that support SecretRef objects, you can use:

```
{
  models: {
    providers: {
      openai: { apiKey: { source: "env", provider: "default", id: "OPENAI_API_KEY" } },
    },
  },
  skills: {
    entries: {
      "image-lab": {
        apiKey: {
          source: "file",
          provider: "filemain",
          id: "/skills/entries/image-lab/apiKey",
        },
      },
    },
  },
  channels: {
    googlechat: {
      serviceAccountRef: {
        source: "exec",
        provider: "vault",
        id: "channels/googlechat/serviceAccount",
      },
    },
  },
}
```

SecretRef details (including `secrets.providers` for `env`/`file`/`exec`) are in [Secrets Management](https://docs.openclaw.ai/gateway/secrets).
Supported credential paths are listed in [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface).

See [Environment](https://docs.openclaw.ai/help/environment) for full precedence and sources.

## [​](https://docs.openclaw.ai/gateway/configuration\#full-reference)  Full reference

For the complete field-by-field reference, see **[Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference)**.

* * *

_Related: [Configuration Examples](https://docs.openclaw.ai/gateway/configuration-examples) · [Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference) · [Doctor](https://docs.openclaw.ai/gateway/doctor)_

## [​](https://docs.openclaw.ai/gateway/configuration\#related)  Related

- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference)
- [Configuration examples](https://docs.openclaw.ai/gateway/configuration-examples)
- [Gateway runbook](https://docs.openclaw.ai/gateway)

[Gateway runbook](https://docs.openclaw.ai/gateway) [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference)

Ctrl+I