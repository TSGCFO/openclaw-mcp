# Cli

_29 pages from docs.openclaw.ai_


---

## CLI reference - OpenClaw

_Source: <https://docs.openclaw.ai/cli>_

[OpenClaw home page](https://docs.openclaw.ai/)

CLI commands

CLI reference

`openclaw` is the main CLI entry point. Each core command has either a
dedicated reference page or is documented with the command it aliases; this
index lists the commands, the global flags, and the output styling rules that
apply across the CLI.

## Command pages

| Area | Commands |
| --- | --- |
| Setup and onboarding | [`crestodian`](https://docs.openclaw.ai/cli/crestodian) · [`setup`](https://docs.openclaw.ai/cli/setup) · [`onboard`](https://docs.openclaw.ai/cli/onboard) · [`configure`](https://docs.openclaw.ai/cli/configure) · [`config`](https://docs.openclaw.ai/cli/config) · [`completion`](https://docs.openclaw.ai/cli/completion) · [`doctor`](https://docs.openclaw.ai/cli/doctor) · [`dashboard`](https://docs.openclaw.ai/cli/dashboard) |
| Reset and uninstall | [`backup`](https://docs.openclaw.ai/cli/backup) · [`reset`](https://docs.openclaw.ai/cli/reset) · [`uninstall`](https://docs.openclaw.ai/cli/uninstall) · [`update`](https://docs.openclaw.ai/cli/update) |
| Messaging and agents | [`message`](https://docs.openclaw.ai/cli/message) · [`agent`](https://docs.openclaw.ai/cli/agent) · [`agents`](https://docs.openclaw.ai/cli/agents) · [`acp`](https://docs.openclaw.ai/cli/acp) · [`mcp`](https://docs.openclaw.ai/cli/mcp) |
| Health and sessions | [`status`](https://docs.openclaw.ai/cli/status) · [`health`](https://docs.openclaw.ai/cli/health) · [`sessions`](https://docs.openclaw.ai/cli/sessions) |
| Gateway and logs | [`gateway`](https://docs.openclaw.ai/cli/gateway) · [`logs`](https://docs.openclaw.ai/cli/logs) · [`system`](https://docs.openclaw.ai/cli/system) |
| Models and inference | [`models`](https://docs.openclaw.ai/cli/models) · [`infer`](https://docs.openclaw.ai/cli/infer) · `capability` (alias for [`infer`](https://docs.openclaw.ai/cli/infer)) · [`memory`](https://docs.openclaw.ai/cli/memory) · [`commitments`](https://docs.openclaw.ai/cli/commitments) · [`wiki`](https://docs.openclaw.ai/cli/wiki) |
| Network and nodes | [`directory`](https://docs.openclaw.ai/cli/directory) · [`nodes`](https://docs.openclaw.ai/cli/nodes) · [`devices`](https://docs.openclaw.ai/cli/devices) · [`node`](https://docs.openclaw.ai/cli/node) |
| Runtime and sandbox | [`approvals`](https://docs.openclaw.ai/cli/approvals) · `exec-policy` (see [`approvals`](https://docs.openclaw.ai/cli/approvals)) · [`sandbox`](https://docs.openclaw.ai/cli/sandbox) · [`tui`](https://docs.openclaw.ai/cli/tui) · `chat`/`terminal` (aliases for [`tui --local`](https://docs.openclaw.ai/cli/tui)) · [`browser`](https://docs.openclaw.ai/cli/browser) |
| Automation | [`cron`](https://docs.openclaw.ai/cli/cron) · [`tasks`](https://docs.openclaw.ai/cli/tasks) · [`hooks`](https://docs.openclaw.ai/cli/hooks) · [`webhooks`](https://docs.openclaw.ai/cli/webhooks) |
| Discovery and docs | [`dns`](https://docs.openclaw.ai/cli/dns) · [`docs`](https://docs.openclaw.ai/cli/docs) |
| Pairing and channels | [`pairing`](https://docs.openclaw.ai/cli/pairing) · [`qr`](https://docs.openclaw.ai/cli/qr) · [`channels`](https://docs.openclaw.ai/cli/channels) |
| Security and plugins | [`security`](https://docs.openclaw.ai/cli/security) · [`secrets`](https://docs.openclaw.ai/cli/secrets) · [`skills`](https://docs.openclaw.ai/cli/skills) · [`plugins`](https://docs.openclaw.ai/cli/plugins) · [`proxy`](https://docs.openclaw.ai/cli/proxy) |
| Legacy aliases | [`daemon`](https://docs.openclaw.ai/cli/daemon) (gateway service) · [`clawbot`](https://docs.openclaw.ai/cli/clawbot) (namespace) |
| Plugins (optional) | [`voicecall`](https://docs.openclaw.ai/cli/voicecall) (if installed) |

## Global flags

| Flag | Purpose |
| --- | --- |
| `--dev` | Isolate state under `~/.openclaw-dev` and shift default ports |
| `--profile <name>` | Isolate state under `~/.openclaw-<name>` |
| `--container <name>` | Target a named container for execution |
| `--no-color` | Disable ANSI colors (`NO_COLOR=1` is also respected) |
| `--update`

_… [truncated; see https://docs.openclaw.ai/cli for full content]_


---

## ACP - OpenClaw

_Source: <https://docs.openclaw.ai/cli/acp>_

# Remote Gateway
openclaw acp --url wss://gateway-host:18789 --token <token>

# Remote Gateway (token from file)
openclaw acp --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token

# Attach to an existing session key
openclaw acp --session agent:main:main

# Attach by label (must already exist)
openclaw acp --session-label "support inbox"

# Reset the session key before the first prompt
openclaw acp --session agent:main:main --reset-session
```

## ACP client (debug)

Use the built-in ACP client to sanity-check the bridge without an IDE.
It spawns the ACP bridge and lets you type prompts interactively.

```
openclaw acp client

# Point the spawned bridge at a remote Gateway
openclaw acp client --server-args --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token

# Override the server command (default: openclaw)
openclaw acp client --server "node" --server-args openclaw.mjs acp --url ws://127.0.0.1:19001
```

Permission model (client debug mode):

- Auto-approval is allowlist-based and only applies to trusted core tool IDs.
- `read` auto-approval is scoped to the current working directory (`--cwd` when set).
- ACP only auto-approves narrow readonly classes: scoped `read` calls under the active cwd plus readonly search tools (`search`, `web_search`, `memory_search`). Unknown/non-core tools, out-of-scope reads, exec-capable tools, control-plane tools, mutating tools, and interactive flows always require explicit prompt approval.
- Server-provided `toolCall.kind` is treated as untrusted metadata (not an authorization source).
- This ACP bridge policy is separate from ACPX harness permissions. If you run OpenClaw through the `acpx` backend, `plugins.entries.acpx.config.permissionMode=approve-all` is the break-glass “yolo” switch for that harness session.

## How to use this

Use ACP when an IDE (or other client) speaks Agent Client Protocol and you want
it to drive an OpenClaw Gateway session.

1. Ensure the Gateway is running (local or remote).
2. Configure the Gateway target (config or flags).
3. Point your IDE to run `openclaw acp` over stdio.

Example config (persisted):

```
openclaw config set gateway.remote.url wss://gateway-host:18789
openclaw config set gateway.remote.token <token>
```

Example direct run (no config write):

```
openclaw acp --url wss://gateway-host:18789 --token <token>
# preferred for local process safety
openclaw acp --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token
```

## Selecting agents

ACP does not pick agents directly. It routes by the Gateway session key.Use agent-scoped session keys to target a specific agent:

```
openclaw acp --session agent:main:main
openclaw acp --session agent:design:main
openclaw acp --session agent:qa:bug-123
```

Each ACP session maps to a single Gateway session key. One agent can have many
sessions; ACP defaults to an isolated `acp:<uuid>` session unless you override
the key or label.Per-session `mcpServers` are not supported in bridge mode. If an ACP client
sends them during `newSession` or `loadSession`, the bridge returns a clear
error instead of silently ignoring them.If you want ACPX-backed sessions to see OpenClaw plugin tools or selected
built-in tools such as `cron`, enable the gateway-side ACPX MCP bridges instead
of trying to pass per-session `mcpServers`. See
[ACP Agents](https://docs.openclaw.ai/tools/acp-agents-setup#plugin-tools-mcp-bridge) and
[OpenClaw tools MCP bridge](https://docs.openclaw.ai/tools/acp-agents-setup#openclaw-tools-mcp-bridge).

## Use from `acpx` (Codex, Claude, other ACP clients)

If you want a coding agent such as Codex or Claude Code to talk to your
OpenClaw bot over ACP, use `acpx` with its built-in `openclaw` target.Typical flow:

1. Run the Gateway and make sure the ACP bridge can reach it.
2. Point `acpx openclaw` at `openclaw acp`.
3. Target the OpenClaw session key you want the coding agent to use.

Examples:

```
# One-shot request into your default OpenClaw ACP session
acpx

_… [truncated; see https://docs.openclaw.ai/cli/acp for full content]_


---

## https://docs.openclaw.ai/cli/acp.md

_Source: <https://docs.openclaw.ai/cli/acp.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# ACP

Run the \[Agent Client Protocol (ACP)\](https://agentclientprotocol.com/) bridge that talks to an OpenClaw Gateway.

This command speaks ACP over stdio for IDEs and forwards prompts to the Gateway
over WebSocket. It keeps ACP sessions mapped to Gateway session keys.

\`openclaw acp\` is a Gateway-backed ACP bridge, not a full ACP-native editor
runtime. It focuses on session routing, prompt delivery, and basic streaming
updates.

If you want an external MCP client to talk directly to OpenClaw channel
conversations instead of hosting an ACP harness session, use
\[\`openclaw mcp serve\`\](/cli/mcp) instead.

\## What this is not

This page is often confused with ACP harness sessions.

\`openclaw acp\` means:

\\* OpenClaw acts as an ACP server
\\* an IDE or ACP client connects to OpenClaw
\\* OpenClaw forwards that work into a Gateway session

This is different from \[ACP Agents\](/tools/acp-agents), where OpenClaw runs an
external harness such as Codex or Claude Code through \`acpx\`.

Quick rule:

\\* editor/client wants to talk ACP to OpenClaw: use \`openclaw acp\`
\\* OpenClaw should launch Codex/Claude/Gemini as an ACP harness: use \`/acp spawn\` and \[ACP Agents\](/tools/acp-agents)

\## Compatibility Matrix

\| ACP area \| Status \| Notes \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`initialize\`, \`newSession\`, \`prompt\`, \`cancel\` \| Implemented \| Core bridge flow over stdio to Gateway chat/send + abort. \|
\| \`listSessions\`, slash commands \| Implemented \| Session list works against Gateway session state; commands are advertised via \`available\_commands\_update\`. \|
\| \`loadSession\` \| Partial \| Rebinds the ACP session to a Gateway session key and replays stored user/assistant text history. Tool/system history is not reconstructed yet. \|
\| Prompt content (\`text\`, embedded \`resource\`, images) \| Partial \| Text/resources are flattened into chat input; images become Gateway attachments. \|
\| Session modes \| Partial \| \`session/set\_mode\` is supported and the bridge exposes initial Gateway-backed session controls for thought level, tool verbosity, reasoning, usage detail, and elevated actions. Broader ACP-native mode/config surfaces are still out of scope. \|
\| Session info and usage updates \| Partial \| The bridge emits \`session\_info\_update\` and best-effort \`usage\_update\` notifications from cached Gateway session snapshots. Usage is approximate and only sent when Gateway token totals are marked fresh. \|
\| Tool streaming \| Partial \| \`tool\_call\` / \`tool\_call\_update\` events include raw I/O, text content, and best-effort file locations when Gateway tool args/results expose them. Embedded terminals and richer diff-native output are still not exposed. \|
\| Per-session MCP servers (\`mcpServers\`) \| Unsupported \| Bridge mode rejects per-session MCP server requests. Configure MCP on the OpenClaw gateway or agent instead. \|
\| Client filesystem methods (\`fs/read\_text\_file\`, \`fs/write\_text\_file\`) \| Unsupported \| The bridge does not call ACP client filesystem methods. \|
\| Client terminal methods (\`terminal/\*\`) \| Unsupported \| The bridge does not create ACP client terminals or stream terminal ids through

_… [truncated; see https://docs.openclaw.ai/cli/acp.md for full content]_


---

## Backup - OpenClaw

_Source: <https://docs.openclaw.ai/cli/backup>_

# `openclaw backup`

Create a local backup archive for OpenClaw state, config, auth profiles, channel/provider credentials, sessions, and optionally workspaces.

```
openclaw backup create
openclaw backup create --output ~/Backups
openclaw backup create --dry-run --json
openclaw backup create --verify
openclaw backup create --no-include-workspace
openclaw backup create --only-config
openclaw backup verify ./2026-03-09T00-00-00.000Z-openclaw-backup.tar.gz
```

## Notes

- The archive includes a `manifest.json` file with the resolved source paths and archive layout.
- Default output is a timestamped `.tar.gz` archive in the current working directory.
- If the current working directory is inside a backed-up source tree, OpenClaw falls back to your home directory for the default archive location.
- Existing archive files are never overwritten.
- Output paths inside the source state/workspace trees are rejected to avoid self-inclusion.
- `openclaw backup verify <archive>` validates that the archive contains exactly one root manifest, rejects traversal-style archive paths, and checks that every manifest-declared payload exists in the tarball.
- `openclaw backup create --verify` runs that validation immediately after writing the archive.
- `openclaw backup create --only-config` backs up just the active JSON config file.

## What gets backed up

`openclaw backup create` plans backup sources from your local OpenClaw install:

- The state directory returned by OpenClaw’s local state resolver, usually `~/.openclaw`
- The active config file path
- The resolved `credentials/` directory when it exists outside the state directory
- Workspace directories discovered from the current config, unless you pass `--no-include-workspace`

Model auth profiles are already part of the state directory under
`agents/<agentId>/agent/auth-profiles.json`, so they are normally covered by the
state backup entry.If you use `--only-config`, OpenClaw skips state, credentials-directory, and workspace discovery and archives only the active config file path.OpenClaw canonicalizes paths before building the archive. If config, the
credentials directory, or a workspace already live inside the state directory,
they are not duplicated as separate top-level backup sources. Missing paths are
skipped.The archive payload stores file contents from those source trees, and the embedded `manifest.json` records the resolved absolute source paths plus the archive layout used for each asset.Installed plugin source and manifest files under the state directory’s
`extensions/` tree are included, but their nested `node_modules/` dependency
trees are skipped. Those dependencies are rebuildable install artifacts; after
restoring an archive, use `openclaw plugins update <id>` or reinstall the plugin
with `openclaw plugins install <spec> --force` when a restored plugin reports
missing dependencies.

## Invalid config behavior

`openclaw backup` intentionally bypasses the normal config preflight so it can still help during recovery. Because workspace discovery depends on a valid config, `openclaw backup create` now fails fast when the config file exists but is invalid and workspace backup is still enabled.If you still want a partial backup in that situation, rerun:

```
openclaw backup create --no-include-workspace
```

That keeps state, config, and the external credentials directory in scope while
skipping workspace discovery entirely.If you only need a copy of the config file itself, `--only-config` also works when the config is malformed because it does not rely on parsing the config for workspace discovery.

## Size and performance

OpenClaw does not enforce a built-in maximum backup size or per-file size limit.Practical limits come from the local machine and destination filesystem:

- Available space for the temporary archive write plus the final archive
- Time to walk large workspace trees and compress them into a `.tar.gz`
- Time to rescan the archive if you use `openclaw backup crea

_… [truncated; see https://docs.openclaw.ai/cli/backup for full content]_


---

## Backup - OpenClaw

_Source: <https://docs.openclaw.ai/cli/backup#backup>_

# `openclaw backup`

Create a local backup archive for OpenClaw state, config, auth profiles, channel/provider credentials, sessions, and optionally workspaces.

```
openclaw backup create
openclaw backup create --output ~/Backups
openclaw backup create --dry-run --json
openclaw backup create --verify
openclaw backup create --no-include-workspace
openclaw backup create --only-config
openclaw backup verify ./2026-03-09T00-00-00.000Z-openclaw-backup.tar.gz
```

## Notes

- The archive includes a `manifest.json` file with the resolved source paths and archive layout.
- Default output is a timestamped `.tar.gz` archive in the current working directory.
- If the current working directory is inside a backed-up source tree, OpenClaw falls back to your home directory for the default archive location.
- Existing archive files are never overwritten.
- Output paths inside the source state/workspace trees are rejected to avoid self-inclusion.
- `openclaw backup verify <archive>` validates that the archive contains exactly one root manifest, rejects traversal-style archive paths, and checks that every manifest-declared payload exists in the tarball.
- `openclaw backup create --verify` runs that validation immediately after writing the archive.
- `openclaw backup create --only-config` backs up just the active JSON config file.

## What gets backed up

`openclaw backup create` plans backup sources from your local OpenClaw install:

- The state directory returned by OpenClaw’s local state resolver, usually `~/.openclaw`
- The active config file path
- The resolved `credentials/` directory when it exists outside the state directory
- Workspace directories discovered from the current config, unless you pass `--no-include-workspace`

Model auth profiles are already part of the state directory under
`agents/<agentId>/agent/auth-profiles.json`, so they are normally covered by the
state backup entry.If you use `--only-config`, OpenClaw skips state, credentials-directory, and workspace discovery and archives only the active config file path.OpenClaw canonicalizes paths before building the archive. If config, the
credentials directory, or a workspace already live inside the state directory,
they are not duplicated as separate top-level backup sources. Missing paths are
skipped.The archive payload stores file contents from those source trees, and the embedded `manifest.json` records the resolved absolute source paths plus the archive layout used for each asset.Installed plugin source and manifest files under the state directory’s
`extensions/` tree are included, but their nested `node_modules/` dependency
trees are skipped. Those dependencies are rebuildable install artifacts; after
restoring an archive, use `openclaw plugins update <id>` or reinstall the plugin
with `openclaw plugins install <spec> --force` when a restored plugin reports
missing dependencies.

## Invalid config behavior

`openclaw backup` intentionally bypasses the normal config preflight so it can still help during recovery. Because workspace discovery depends on a valid config, `openclaw backup create` now fails fast when the config file exists but is invalid and workspace backup is still enabled.If you still want a partial backup in that situation, rerun:

```
openclaw backup create --no-include-workspace
```

That keeps state, config, and the external credentials directory in scope while
skipping workspace discovery entirely.If you only need a copy of the config file itself, `--only-config` also works when the config is malformed because it does not rely on parsing the config for workspace discovery.

## Size and performance

OpenClaw does not enforce a built-in maximum backup size or per-file size limit.Practical limits come from the local machine and destination filesystem:

- Available space for the temporary archive write plus the final archive
- Time to walk large workspace trees and compress them into a `.tar.gz`
- Time to rescan the archive if you use `openclaw backup crea

_… [truncated; see https://docs.openclaw.ai/cli/backup#backup for full content]_


---

## https://docs.openclaw.ai/cli/backup.md

_Source: <https://docs.openclaw.ai/cli/backup.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Backup

\# \`openclaw backup\`

Create a local backup archive for OpenClaw state, config, auth profiles, channel/provider credentials, sessions, and optionally workspaces.

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw backup create
openclaw backup create --output ~/Backups
openclaw backup create --dry-run --json
openclaw backup create --verify
openclaw backup create --no-include-workspace
openclaw backup create --only-config
openclaw backup verify ./2026-03-09T00-00-00.000Z-openclaw-backup.tar.gz
\`\`\`

\## Notes

\\* The archive includes a \`manifest.json\` file with the resolved source paths and archive layout.
\\* Default output is a timestamped \`.tar.gz\` archive in the current working directory.
\\* If the current working directory is inside a backed-up source tree, OpenClaw falls back to your home directory for the default archive location.
\\* Existing archive files are never overwritten.
\\* Output paths inside the source state/workspace trees are rejected to avoid self-inclusion.
\\* \`openclaw backup verify \` validates that the archive contains exactly one root manifest, rejects traversal-style archive paths, and checks that every manifest-declared payload exists in the tarball.
\\* \`openclaw backup create --verify\` runs that validation immediately after writing the archive.
\\* \`openclaw backup create --only-config\` backs up just the active JSON config file.

\## What gets backed up

\`openclaw backup create\` plans backup sources from your local OpenClaw install:

\\* The state directory returned by OpenClaw's local state resolver, usually \`~/.openclaw\`
\\* The active config file path
\\* The resolved \`credentials/\` directory when it exists outside the state directory
\\* Workspace directories discovered from the current config, unless you pass \`--no-include-workspace\`

Model auth profiles are already part of the state directory under
\`agents//agent/auth-profiles.json\`, so they are normally covered by the
state backup entry.

If you use \`--only-config\`, OpenClaw skips state, credentials-directory, and workspace discovery and archives only the active config file path.

OpenClaw canonicalizes paths before building the archive. If config, the
credentials directory, or a workspace already live inside the state directory,
they are not duplicated as separate top-level backup sources. Missing paths are
skipped.

The archive payload stores file contents from those source trees, and the embedded \`manifest.json\` records the resolved absolute source paths plus the archive layout used for each asset.

Installed plugin source and manifest files under the state directory's
\`extensions/\` tree are included, but their nested \`node\_modules/\` dependency
trees are skipped. Those dependencies are rebuildable install artifacts; after
restoring an archive, use \`openclaw plugins update \` or reinstall the plugin
with \`openclaw plugins install  --force\` when a restored plugin reports
missing dependencies.

\## Invalid config behavior

\`openclaw backup\` intentionally bypasses the normal config preflight so it can still help during recovery. Because workspace discovery depends on a valid config, \`openclaw backup create\` now fails fast when the config file exists but is invalid and workspace backup is still enabled.

If you still want a partial backup in that situation, rerun:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw backup create --no-include-workspace
\`\`\`

That keeps state, config, and the external credentials directory in scope while
skipping workspace discovery entirely.

If you only need a copy of the config file itself, \`--only-config\` also works when the config is malformed because it does not rely on parsing the config for workspace discovery.

\## Size and performance

OpenC

_… [truncated; see https://docs.openclaw.ai/cli/backup.md for full content]_


---

## Config - OpenClaw

_Source: <https://docs.openclaw.ai/cli/config>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Config

Config helpers for non-interactive edits in `openclaw.json`: get/set/patch/unset/file/schema/validate values by path and print the active config file. Run without a subcommand to open the configure wizard (same as `openclaw configure`).

## Root options

[​](https://docs.openclaw.ai/cli/config#param-section-section)

--section <section>

string

Repeatable guided-setup section filter when you run `openclaw config` without a subcommand.

Supported guided sections: `workspace`, `model`, `web`, `gateway`, `daemon`, `channels`, `plugins`, `skills`, `health`.

## Examples

```
openclaw config file
openclaw config --section model
openclaw config --section gateway --section daemon
openclaw config schema
openclaw config get browser.executablePath
openclaw config set browser.executablePath "/usr/bin/google-chrome"
openclaw config set browser.profiles.work.executablePath "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
openclaw config set agents.defaults.heartbeat.every "2h"
openclaw config set agents.list[0].tools.exec.node "node-id-or-name"
openclaw config set agents.defaults.models '{"openai/gpt-5.4":{}}' --strict-json --merge
openclaw config set channels.discord.token --ref-provider default --ref-source env --ref-id DISCORD_BOT_TOKEN
openclaw config set secrets.providers.vaultfile --provider-source file --provider-path /etc/openclaw/secrets.json --provider-mode json
openclaw config patch --file ./openclaw.patch.json5 --dry-run
openclaw config unset plugins.entries.brave.config.webSearch.apiKey
openclaw config set channels.discord.token --ref-provider default --ref-source env --ref-id DISCORD_BOT_TOKEN --dry-run
openclaw config validate
openclaw config validate --json
```

### `config schema`

Print the generated JSON schema for `openclaw.json` to stdout as JSON.

What it includes

- The current root config schema, plus a root `$schema` string field for editor tooling.
- Field `title` and `description` docs metadata used by the Control UI.
- Nested object, wildcard (`*`), and array-item (`[]`) nodes inherit the same `title` / `description` metadata when matching field documentation exists.
- `anyOf` / `oneOf` / `allOf` branches inherit the same docs metadata too when matching field documentation exists.
- Best-effort live plugin + channel schema metadata when runtime manifests can be loaded.
- A clean fallback schema even when the current config is invalid.

Related runtime RPC

`config.schema.lookup` returns one normalized config path with a shallow schema node (`title`, `description`, `type`, `enum`, `const`, common bounds), matched UI hint metadata, and immediate child summaries. Use it for path-scoped drill-down in Control UI or custom clients.

```
openclaw config schema
```

Pipe it into a file when you want to inspect or validate it with other tools:

```
openclaw config schema > openclaw.schema.json
```

### Paths

Paths use dot or bracket notation:

```
openclaw config get agents.defaults.workspace
openclaw config get agents.list[0].id
```

Use the agent list index to target a specific agent:

```
openclaw config get agents.list
openclaw config set agents.list[1].tools.exec.node "node-id-or-name"
```

## Values

Values are parsed as JSON5 when possible; otherwise they are treated as strings. Use `--strict-json` to require JSON5 parsing. `--json` remains supported as a legacy alias.

```
openclaw config set agents.defaults.heartbeat.every "0m"
openclaw config set gateway.port 19001 --strict-json
openclaw config set channels.whatsapp.groups '["*"]' --strict-json
```

`config get <path> --json` prints the raw value as JSON instead of terminal-formatted text.

Object assignment replaces the target path by default. Protected map/list paths that commonly hold user-added entries, such as `agents.defaults.models`, `models.providers`, `models.providers.<id>.models`, `plugins.entries`, and `auth.profiles`, refuse replacements that would remove existi

_… [truncated; see https://docs.openclaw.ai/cli/config for full content]_


---

## Configure - OpenClaw

_Source: <https://docs.openclaw.ai/cli/configure>_

# `openclaw configure`

Interactive prompt to set up credentials, devices, and agent defaults.

The **Model** section includes a multi-select for the `agents.defaults.models` allowlist (what shows up in `/model` and the model picker). Provider-scoped setup choices merge their selected models into the existing allowlist instead of replacing unrelated providers already in the config.Re-running provider auth from configure preserves an existing `agents.defaults.model.primary`, even when the provider’s auth step returns a config patch with its own recommended default model. That means adding or reauthing xAI, OpenRouter, or another provider should make the new model available without taking over from your current primary model. Use `openclaw models auth login --provider <id> --set-default` or `openclaw models set <model>` when you intentionally want to change the default model.

When configure starts from a provider auth choice, the default-model and allowlist pickers prefer that provider automatically. For paired providers such as Volcengine and BytePlus, the same preference also matches their coding-plan variants (`volcengine-plan/*`, `byteplus-plan/*`). If the preferred-provider filter would produce an empty list, configure falls back to the unfiltered catalog instead of showing a blank picker.

`openclaw config` without a subcommand opens the same wizard. Use `openclaw config get|set|unset` for non-interactive edits.

For web search, `openclaw configure --section web` lets you choose a provider
and configure its credentials. Some providers also show provider-specific
follow-up prompts:

- **Grok** can offer optional `x_search` setup with the same `XAI_API_KEY` and
let you pick an `x_search` model.
- **Kimi** can ask for the Moonshot API region (`api.moonshot.ai` vs
`api.moonshot.cn`) and the default Kimi web-search model.

Related:

- Gateway configuration reference: [Configuration](https://docs.openclaw.ai/gateway/configuration)
- Config CLI: [Config](https://docs.openclaw.ai/cli/config)

## Options

- `--section <section>`: repeatable section filter

Available sections:

- `workspace`
- `model`
- `web`
- `gateway`
- `daemon`
- `channels`
- `plugins`
- `skills`
- `health`

Notes:

- Choosing where the Gateway runs always updates `gateway.mode`. You can select “Continue” without other sections if that is all you need.
- After local config writes, configure installs selected downloadable plugins when the chosen setup path requires them. Remote gateway config does not install local plugin packages.
- Channel-oriented services (Slack/Discord/Matrix/Microsoft Teams) prompt for channel/room allowlists during setup. You can enter names or IDs; the wizard resolves names to IDs when possible.
- If you run the daemon install step, token auth requires a token, and `gateway.auth.token` is SecretRef-managed, configure validates the SecretRef but does not persist resolved plaintext token values into supervisor service environment metadata.
- If token auth requires a token and the configured token SecretRef is unresolved, configure blocks daemon install with actionable remediation guidance.
- If both `gateway.auth.token` and `gateway.auth.password` are configured and `gateway.auth.mode` is unset, configure blocks daemon install until mode is set explicitly.

## Examples

```
openclaw configure
openclaw configure --section web
openclaw configure --section model --section channels
openclaw configure --section gateway --section daemon
```

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)

[Config](https://docs.openclaw.ai/cli/config) [Webhooks](https://docs.openclaw.ai/cli/webhooks)

Ctrl+I


---

## Crestodian - OpenClaw

_Source: <https://docs.openclaw.ai/cli/crestodian>_

# `openclaw crestodian`

Crestodian is OpenClaw’s local setup, repair, and configuration helper. It is
designed to stay reachable when the normal agent path is broken.Running `openclaw` with no command starts Crestodian in an interactive terminal.
Running `openclaw crestodian` starts the same helper explicitly.

## What Crestodian shows

On startup, interactive Crestodian opens the same TUI shell used by
`openclaw tui`, with a Crestodian chat backend. The chat log starts with a short
greeting:

- when to start Crestodian
- the model or deterministic planner path Crestodian is actually using
- config validity and the default agent
- Gateway reachability from the first startup probe
- the next debug action Crestodian can take

It does not dump secrets or load plugin CLI commands just to start. The TUI
still provides the normal header, chat log, status line, footer, autocomplete,
and editor controls.Use `status` for the detailed inventory with config path, docs/source paths,
local CLI probes, API-key presence, agents, model, and Gateway details.Crestodian uses the same OpenClaw reference discovery as regular agents. In a Git checkout,
it points itself at local `docs/` and the local source tree. In an npm package install, it
uses the bundled package docs and links to
[https://github.com/openclaw/openclaw](https://github.com/openclaw/openclaw), with explicit
guidance to review source whenever the docs are not enough.

## Examples

```
openclaw
openclaw crestodian
openclaw crestodian --json
openclaw crestodian --message "models"
openclaw crestodian --message "validate config"
openclaw crestodian --message "setup workspace ~/Projects/work model openai/gpt-5.5" --yes
openclaw crestodian --message "set default model openai/gpt-5.5" --yes
openclaw onboard --modern
```

Inside the Crestodian TUI:

```
status
health
doctor
doctor fix
validate config
setup
setup workspace ~/Projects/work model openai/gpt-5.5
config set gateway.port 19001
config set-ref gateway.auth.token env OPENCLAW_GATEWAY_TOKEN
gateway status
restart gateway
agents
create agent work workspace ~/Projects/work
models
set default model openai/gpt-5.5
plugins list
plugins search slack
plugin install clawhub:openclaw-codex-app-server
plugin uninstall openclaw-codex-app-server
talk to work agent
talk to agent for ~/Projects/work
audit
quit
```

## Safe startup

Crestodian’s startup path is deliberately small. It can run when:

- `openclaw.json` is missing
- `openclaw.json` is invalid
- the Gateway is down
- plugin command registration is unavailable
- no agent has been configured yet

`openclaw --help` and `openclaw --version` still use the normal fast paths.
Noninteractive `openclaw` exits with a short message instead of printing root
help, because the no-command product is Crestodian.

## Operations and approval

Crestodian uses typed operations instead of editing config ad hoc.Read-only operations can run immediately:

- show overview
- list agents
- list installed plugins
- search ClawHub plugins
- show model/backend status
- run status or health checks
- check Gateway reachability
- run doctor without interactive fixes
- validate config
- show the audit-log path

Persistent operations require conversational approval in interactive mode unless
you pass `--yes` for a direct command:

- write config
- run `config set`
- set supported SecretRef values through `config set-ref`
- run setup/onboarding bootstrap
- change the default model
- start, stop, or restart the Gateway
- create agents
- install plugins from ClawHub or npm
- uninstall plugins
- run doctor repairs that rewrite config or state

Applied writes are recorded in:

```
~/.openclaw/audit/crestodian.jsonl
```

Discovery is not audited. Only applied operations and writes are logged.`openclaw onboard --modern` starts Crestodian as the modern onboarding preview.
Plain `openclaw onboard` still runs classic onboarding.

## Setup bootstrap

`setup` is the chat-first onboarding bootstrap. It writes only through typed
conf

_… [truncated; see https://docs.openclaw.ai/cli/crestodian for full content]_


---

## Daemon - OpenClaw

_Source: <https://docs.openclaw.ai/cli/daemon>_

# `openclaw daemon`

Legacy alias for Gateway service management commands.`openclaw daemon ...` maps to the same service control surface as `openclaw gateway ...` service commands.

## Usage

```
openclaw daemon status
openclaw daemon install
openclaw daemon start
openclaw daemon stop
openclaw daemon restart
openclaw daemon uninstall
```

## Subcommands

- `status`: show service install state and probe Gateway health
- `install`: install service (`launchd`/`systemd`/`schtasks`)
- `uninstall`: remove service
- `start`: start service
- `stop`: stop service
- `restart`: restart service

## Common options

- `status`: `--url`, `--token`, `--password`, `--timeout`, `--no-probe`, `--require-rpc`, `--deep`, `--json`
- `install`: `--port`, `--runtime <node|bun>`, `--token`, `--force`, `--json`
- `restart`: `--force`, `--wait <duration>`, `--json`
- lifecycle (`uninstall|start|stop`): `--json`

Notes:

- `status` resolves configured auth SecretRefs for probe auth when possible.
- If a required auth SecretRef is unresolved in this command path, `daemon status --json` reports `rpc.authWarning` when probe connectivity/auth fails; pass `--token`/`--password` explicitly or resolve the secret source first.
- If the probe succeeds, unresolved auth-ref warnings are suppressed to avoid false positives.
- `status --deep` adds a best-effort system-level service scan. When it finds other gateway-like services, human output prints cleanup hints and warns that one gateway per machine is still the normal recommendation.
- On Linux systemd installs, `status` token-drift checks include both `Environment=` and `EnvironmentFile=` unit sources.
- Drift checks resolve `gateway.auth.token` SecretRefs using merged runtime env (service command env first, then process env fallback).
- If token auth is not effectively active (explicit `gateway.auth.mode` of `password`/`none`/`trusted-proxy`, or mode unset where password can win and no token candidate can win), token-drift checks skip config token resolution.
- When token auth requires a token and `gateway.auth.token` is SecretRef-managed, `install` validates that the SecretRef is resolvable but does not persist the resolved token into service environment metadata.
- If token auth requires a token and the configured token SecretRef is unresolved, install fails closed.
- If both `gateway.auth.token` and `gateway.auth.password` are configured and `gateway.auth.mode` is unset, install is blocked until mode is set explicitly.
- On macOS, `install` keeps LaunchAgent plists owner-only and loads managed service environment values through an owner-only file and wrapper instead of serializing API keys or auth-profile env refs into `EnvironmentVariables`.
- If you intentionally run multiple gateways on one host, isolate ports, config/state, and workspaces; see [/gateway#multiple-gateways-same-host](https://docs.openclaw.ai/gateway#multiple-gateways-same-host).

## Prefer

Use [`openclaw gateway`](https://docs.openclaw.ai/cli/gateway) for current docs and examples.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Gateway runbook](https://docs.openclaw.ai/gateway)

[Crestodian](https://docs.openclaw.ai/cli/crestodian) [Doctor](https://docs.openclaw.ai/cli/doctor)

Ctrl+I


---

## Dashboard - OpenClaw

_Source: <https://docs.openclaw.ai/cli/dashboard>_

# `openclaw dashboard`

Open the Control UI using your current auth.

```
openclaw dashboard
openclaw dashboard --no-open
```

Notes:

- `dashboard` resolves configured `gateway.auth.token` SecretRefs when possible.
- `dashboard` follows `gateway.tls.enabled`: TLS-enabled gateways print/open
`https://` Control UI URLs and connect over `wss://`.
- For SecretRef-managed tokens (resolved or unresolved), `dashboard` prints/copies/opens a non-tokenized URL to avoid exposing external secrets in terminal output, clipboard history, or browser-launch arguments.
- If `gateway.auth.token` is SecretRef-managed but unresolved in this command path, the command prints a non-tokenized URL and explicit remediation guidance instead of embedding an invalid token placeholder.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Dashboard](https://docs.openclaw.ai/web/dashboard)

[Skills](https://docs.openclaw.ai/cli/skills) [TUI](https://docs.openclaw.ai/cli/tui)

Ctrl+I


---

## Devices - OpenClaw

_Source: <https://docs.openclaw.ai/cli/devices>_

# `openclaw devices`

Manage device pairing requests and device-scoped tokens.

## Commands

### `openclaw devices list`

List pending pairing requests and paired devices.

```
openclaw devices list
openclaw devices list --json
```

Pending request output shows the requested access next to the device’s current
approved access when the device is already paired. This makes scope/role
upgrades explicit instead of looking like the pairing was lost.

### `openclaw devices remove <deviceId>`

Remove one paired device entry.When you are authenticated with a paired device token, non-admin callers can
remove only **their own** device entry. Removing some other device requires
`operator.admin`.

```
openclaw devices remove <deviceId>
openclaw devices remove <deviceId> --json
```

### `openclaw devices clear --yes [--pending]`

Clear paired devices in bulk.

```
openclaw devices clear --yes
openclaw devices clear --yes --pending
openclaw devices clear --yes --pending --json
```

### `openclaw devices approve [requestId] [--latest]`

Approve a pending device pairing request by exact `requestId`. If `requestId`
is omitted or `--latest` is passed, OpenClaw only prints the selected pending
request and exits; rerun approval with the exact request ID after verifying
the details.

If a device retries pairing with changed auth details (role, scopes, or public key), OpenClaw supersedes the previous pending entry and issues a new `requestId`. Run `openclaw devices list` right before approval to use the current ID.

If the device is already paired and asks for broader scopes or a broader role,
OpenClaw keeps the existing approval in place and creates a new pending upgrade
request. Review the `Requested` vs `Approved` columns in `openclaw devices list`
or use `openclaw devices approve --latest` to preview the exact upgrade before
approving it.If the Gateway is explicitly configured with
`gateway.nodes.pairing.autoApproveCidrs`, first-time `role: node` requests from
matching client IPs can be approved before they appear in this list. That policy
is disabled by default and never applies to operator/browser clients or upgrade
requests.

```
openclaw devices approve
openclaw devices approve <requestId>
openclaw devices approve --latest
```

### `openclaw devices reject <requestId>`

Reject a pending device pairing request.

```
openclaw devices reject <requestId>
```

### `openclaw devices rotate --device <id> --role <role> [--scope <scope...>]`

Rotate a device token for a specific role (optionally updating scopes).
The target role must already exist in that device’s approved pairing contract;
rotation cannot mint a new unapproved role.
If you omit `--scope`, later reconnects with the stored rotated token reuse that
token’s cached approved scopes. If you pass explicit `--scope` values, those
become the stored scope set for future cached-token reconnects.
Non-admin paired-device callers can rotate only their **own** device token.
The target token scope set must stay within the caller session’s own operator
scopes; rotation cannot mint or preserve a broader operator token than the
caller already has.

```
openclaw devices rotate --device <deviceId> --role operator --scope operator.read --scope operator.write
```

Returns rotation metadata as JSON. If the caller is rotating its own token while
authenticated with that device token, the response also includes the replacement
token so the client can persist it before reconnecting. Shared/admin rotations
do not echo the bearer token.

### `openclaw devices revoke --device <id> --role <role>`

Revoke a device token for a specific role.Non-admin paired-device callers can revoke only their **own** device token.
Revoking some other device’s token requires `operator.admin`.
The target token scope set must also fit within the caller session’s own
operator scopes; pairing-only callers cannot revoke admin/write operator tokens.

```
openclaw devices revoke --device <deviceId> --role node
```

Returns the revoke resul

_… [truncated; see https://docs.openclaw.ai/cli/devices for full content]_


---

## Directory - OpenClaw

_Source: <https://docs.openclaw.ai/cli/directory>_

# `openclaw directory`

Directory lookups for channels that support it (contacts/peers, groups, and “me”).

## Common flags

- `--channel <name>`: channel id/alias (required when multiple channels are configured; auto when only one is configured)
- `--account <id>`: account id (default: channel default)
- `--json`: output JSON

## Notes

- `directory` is meant to help you find IDs you can paste into other commands (especially `openclaw message send --target ...`).
- For many channels, results are config-backed (allowlists / configured groups) rather than a live provider directory.
- Installed channel plugins can still omit directory support; in that case the command reports the unsupported directory operation instead of reinstalling the plugin.
- Default output is `id` (and sometimes `name`) separated by a tab; use `--json` for scripting.

## Using results with `message send`

```
openclaw directory peers list --channel slack --query "U0"
openclaw message send --channel slack --target user:U012ABCDEF --message "hello"
```

## ID formats (by channel)

- WhatsApp: `+15551234567` (DM), `1234567890-1234567890@g.us` (group), `120363123456789@newsletter` (Channel/Newsletter outbound target)
- Telegram: `@username` or numeric chat id; groups are numeric ids
- Slack: `user:U…` and `channel:C…`
- Discord: `user:<id>` and `channel:<id>`
- Matrix (plugin): `user:@user:server`, `room:!roomId:server`, or `#alias:server`
- Microsoft Teams (plugin): `user:<id>` and `conversation:<id>`
- Zalo (plugin): user id (Bot API)
- Zalo Personal / `zalouser` (plugin): thread id (DM/group) from `zca` (`me`, `friend list`, `group list`)

## Self (“me”)

```
openclaw directory self --channel zalouser
```

## Peers (contacts/users)

```
openclaw directory peers list --channel zalouser
openclaw directory peers list --channel zalouser --query "name"
openclaw directory peers list --channel zalouser --limit 50
```

## Groups

```
openclaw directory groups list --channel zalouser
openclaw directory groups list --channel zalouser --query "work"
openclaw directory groups members --channel zalouser --group-id <id>
```

## Related

- [CLI reference](https://docs.openclaw.ai/cli)

[Devices](https://docs.openclaw.ai/cli/devices) [Pairing](https://docs.openclaw.ai/cli/pairing)

Ctrl+I


---

## Docs - OpenClaw

_Source: <https://docs.openclaw.ai/cli/docs>_

# `openclaw docs`

Search the live docs index.Arguments:

- `[query...]`: search terms to send to the live docs index

Examples:

```
openclaw docs
openclaw docs browser existing-session
openclaw docs sandbox allowHostControl
openclaw docs gateway token secretref
```

Notes:

- With no query, `openclaw docs` opens the live docs search entrypoint.
- Multi-word queries are passed through as one search request.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)

[DNS](https://docs.openclaw.ai/cli/dns) [MCP](https://docs.openclaw.ai/cli/mcp)

Ctrl+I


---

## Health - OpenClaw

_Source: <https://docs.openclaw.ai/cli/health>_

# `openclaw health`

Fetch health from the running Gateway.Options:

- `--json`: machine-readable output
- `--timeout <ms>`: connection timeout in milliseconds (default `10000`)
- `--verbose`: verbose logging
- `--debug`: alias for `--verbose`

Examples:

```
openclaw health
openclaw health --json
openclaw health --timeout 2500
openclaw health --verbose
openclaw health --debug
```

Notes:

- Default `openclaw health` asks the running gateway for its health snapshot. When the
gateway already has a fresh cached snapshot, it can return that cached payload and
refresh in the background.
- `--verbose` forces a live probe, prints gateway connection details, and expands the
human-readable output across all configured accounts and agents.
- Output includes per-agent session stores when multiple agents are configured.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Gateway health](https://docs.openclaw.ai/gateway/health)

[Gateway](https://docs.openclaw.ai/cli/gateway) [Logs](https://docs.openclaw.ai/cli/logs)

Ctrl+I


---

## MCP - OpenClaw

_Source: <https://docs.openclaw.ai/cli/mcp>_

[OpenClaw home page](https://docs.openclaw.ai/)

Utility

MCP

`openclaw mcp` has two jobs:

- run OpenClaw as an MCP server with `openclaw mcp serve`
- manage OpenClaw-owned outbound MCP server definitions with `list`, `show`, `set`, and `unset`

In other words:

- `serve` is OpenClaw acting as an MCP server
- `list` / `show` / `set` / `unset` is OpenClaw acting as an MCP client-side registry for other MCP servers its runtimes may consume later

Use [`openclaw acp`](https://docs.openclaw.ai/cli/acp) when OpenClaw should host a coding harness session itself and route that runtime through ACP.

## OpenClaw as an MCP server

This is the `openclaw mcp serve` path.

### When to use `serve`

Use `openclaw mcp serve` when:

- Codex, Claude Code, or another MCP client should talk directly to OpenClaw-backed channel conversations
- you already have a local or remote OpenClaw Gateway with routed sessions
- you want one MCP server that works across OpenClaw’s channel backends instead of running separate per-channel bridges

Use [`openclaw acp`](https://docs.openclaw.ai/cli/acp) instead when OpenClaw should host the coding runtime itself and keep the agent session inside OpenClaw.

### How it works

`openclaw mcp serve` starts a stdio MCP server. The MCP client owns that process. While the client keeps the stdio session open, the bridge connects to a local or remote OpenClaw Gateway over WebSocket and exposes routed channel conversations over MCP.

1

[Navigate to header](https://docs.openclaw.ai/cli/mcp#)

Client spawns the bridge

The MCP client spawns `openclaw mcp serve`.

2

[Navigate to header](https://docs.openclaw.ai/cli/mcp#)

Bridge connects to Gateway

The bridge connects to the OpenClaw Gateway over WebSocket.

3

[Navigate to header](https://docs.openclaw.ai/cli/mcp#)

Sessions become MCP conversations

Routed sessions become MCP conversations and transcript/history tools.

4

[Navigate to header](https://docs.openclaw.ai/cli/mcp#)

Live events queue

Live events are queued in memory while the bridge is connected.

5

[Navigate to header](https://docs.openclaw.ai/cli/mcp#)

Optional Claude push

If Claude channel mode is enabled, the same session can also receive Claude-specific push notifications.

Important behavior

- live queue state starts when the bridge connects
- older transcript history is read with `messages_read`
- Claude push notifications only exist while the MCP session is alive
- when the client disconnects, the bridge exits and the live queue is gone
- one-shot agent entry points such as `openclaw agent` and `openclaw infer model run` retire any bundled MCP runtimes they open when the reply completes, so repeated scripted runs do not accumulate stdio MCP child processes
- stdio MCP servers launched by OpenClaw (bundled or user-configured) are torn down as a process tree on shutdown, so child subprocesses started by the server do not survive after the parent stdio client exits
- deleting or resetting a session disposes that session’s MCP clients through the shared runtime cleanup path, so there are no lingering stdio connections tied to a removed session

### Choose a client mode

Use the same bridge in two different ways:

- Generic MCP clients

- Claude Code

Standard MCP tools only. Use `conversations_list`, `messages_read`, `events_poll`, `events_wait`, `messages_send`, and the approval tools.

Standard MCP tools plus the Claude-specific channel adapter. Enable `--claude-channel-mode on` or leave the default `auto`.

Today, `auto` behaves the same as `on`. There is no client capability detection yet.

### What `serve` exposes

The bridge uses existing Gateway session route metadata to expose channel-backed conversations. A conversation appears when OpenClaw already has session state with a known route such as:

- `channel`
- recipient or destination metadata
- optional `accountId`
- optional `threadId`

This gives MCP clients one place to:

- list recent routed conversations
- read recent transcript histor

_… [truncated; see https://docs.openclaw.ai/cli/mcp for full content]_


---

## https://docs.openclaw.ai/cli/mcp.md

_Source: <https://docs.openclaw.ai/cli/mcp.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# MCP

\`openclaw mcp\` has two jobs:

\\* run OpenClaw as an MCP server with \`openclaw mcp serve\`
\\* manage OpenClaw-owned outbound MCP server definitions with \`list\`, \`show\`, \`set\`, and \`unset\`

In other words:

\\* \`serve\` is OpenClaw acting as an MCP server
\\* \`list\` / \`show\` / \`set\` / \`unset\` is OpenClaw acting as an MCP client-side registry for other MCP servers its runtimes may consume later

Use \[\`openclaw acp\`\](/cli/acp) when OpenClaw should host a coding harness session itself and route that runtime through ACP.

\## OpenClaw as an MCP server

This is the \`openclaw mcp serve\` path.

\### When to use \`serve\`

Use \`openclaw mcp serve\` when:

\\* Codex, Claude Code, or another MCP client should talk directly to OpenClaw-backed channel conversations
\\* you already have a local or remote OpenClaw Gateway with routed sessions
\\* you want one MCP server that works across OpenClaw's channel backends instead of running separate per-channel bridges

Use \[\`openclaw acp\`\](/cli/acp) instead when OpenClaw should host the coding runtime itself and keep the agent session inside OpenClaw.

\### How it works

\`openclaw mcp serve\` starts a stdio MCP server. The MCP client owns that process. While the client keeps the stdio session open, the bridge connects to a local or remote OpenClaw Gateway over WebSocket and exposes routed channel conversations over MCP.

 The MCP client spawns \`openclaw mcp serve\`.

 The bridge connects to the OpenClaw Gateway over WebSocket.

 Routed sessions become MCP conversations and transcript/history tools.

 Live events are queued in memory while the bridge is connected.

 If Claude channel mode is enabled, the same session can also receive Claude-specific push notifications.

 \\* live queue state starts when the bridge connects
 \\* older transcript history is read with \`messages\_read\`
 \\* Claude push notifications only exist while the MCP session is alive
 \\* when the client disconnects, the bridge exits and the live queue is gone
 \\* one-shot agent entry points such as \`openclaw agent\` and \`openclaw infer model run\` retire any bundled MCP runtimes they open when the reply completes, so repeated scripted runs do not accumulate stdio MCP child processes
 \\* stdio MCP servers launched by OpenClaw (bundled or user-configured) are torn down as a process tree on shutdown, so child subprocesses started by the server do not survive after the parent stdio client exits
 \\* deleting or resetting a session disposes that session's MCP clients through the shared runtime cleanup path, so there are no lingering stdio connections tied to a removed session

\### Choose a client mode

Use the same bridge in two different ways:

 Standard MCP tools only. Use \`conversations\_list\`, \`messages\_read\`, \`events\_poll\`, \`events\_wait\`, \`messages\_send\`, and the approval tools.

 Standard MCP tools plus the Claude-specific channel adapter. Enable \`--claude-channel-mode on\` or leave the default \`auto\`.

 Today, \`auto\` behaves the same as \`on\`. There is no client capability detection yet.

\### What \`serve\` exposes

The bridge uses existing Gateway session route metadata to expose channel-backed conversations. A conversation appears when OpenClaw already has session state with a known route such as:

\\* \`channel\`
\\* recipient or destination metadata
\\* optional \`accountId\`
\\* optional \`threadId\`

This gives MCP clients one place to:

\\* list recent routed conversations
\\* read recent transcript history
\\* wait for new inbound events
\\* send a reply back through the same route
\\* see approval requests that arrive while the bridge is connected

\### Usage

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw mcp serve
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"m

_… [truncated; see https://docs.openclaw.ai/cli/mcp.md for full content]_


---

## Memory - OpenClaw

_Source: <https://docs.openclaw.ai/cli/memory>_

# `openclaw memory`

Manage semantic memory indexing and search.
Provided by the active memory plugin (default: `memory-core`; set `plugins.slots.memory = "none"` to disable).Related:

- Memory concept: [Memory](https://docs.openclaw.ai/concepts/memory)
- Memory wiki: [Memory Wiki](https://docs.openclaw.ai/plugins/memory-wiki)
- Wiki CLI: [wiki](https://docs.openclaw.ai/cli/wiki)
- Plugins: [Plugins](https://docs.openclaw.ai/tools/plugin)

## Examples

```
openclaw memory status
openclaw memory status --deep
openclaw memory status --fix
openclaw memory index --force
openclaw memory search "meeting notes"
openclaw memory search --query "deployment" --max-results 20
openclaw memory promote --limit 10 --min-score 0.75
openclaw memory promote --apply
openclaw memory promote --json --min-recall-count 0 --min-unique-queries 0
openclaw memory promote-explain "router vlan"
openclaw memory promote-explain "router vlan" --json
openclaw memory rem-harness
openclaw memory rem-harness --json
openclaw memory status --json
openclaw memory status --deep --index
openclaw memory status --deep --index --verbose
openclaw memory status --agent main
openclaw memory index --agent main --verbose
```

## Options

`memory status` and `memory index`:

- `--agent <id>`: scope to a single agent. Without it, these commands run for each configured agent; if no agent list is configured, they fall back to the default agent.
- `--verbose`: emit detailed logs during probes and indexing.

`memory status`:

- `--deep`: probe vector + embedding availability. Plain `memory status` stays fast and does not run a live embedding ping. QMD lexical `searchMode: "search"` skips semantic vector probes and embedding maintenance even with `--deep`.
- `--index`: run a reindex if the store is dirty (implies `--deep`).
- `--fix`: repair stale recall locks and normalize promotion metadata.
- `--json`: print JSON output.

If `memory status` shows `Dreaming status: blocked`, the managed dreaming cron is enabled but the heartbeat that drives it is not firing for the default agent. See [Dreaming never runs](https://docs.openclaw.ai/concepts/dreaming#dreaming-never-runs-status-shows-blocked) for the two common causes.`memory index`:

- `--force`: force a full reindex.

`memory search`:

- Query input: pass either positional `[query]` or `--query <text>`.
- If both are provided, `--query` wins.
- If neither is provided, the command exits with an error.
- `--agent <id>`: scope to a single agent (default: the default agent).
- `--max-results <n>`: limit the number of results returned.
- `--min-score <n>`: filter out low-score matches.
- `--json`: print JSON results.

`memory promote`:Preview and apply short-term memory promotions.

```
openclaw memory promote [--apply] [--limit <n>] [--include-promoted]
```

- `--apply` — write promotions to `MEMORY.md` (default: preview only).
- `--limit <n>` — cap the number of candidates shown.
- `--include-promoted` — include entries already promoted in previous cycles.

Full options:

- Ranks short-term candidates from `memory/YYYY-MM-DD.md` using weighted promotion signals (`frequency`, `relevance`, `query diversity`, `recency`, `consolidation`, `conceptual richness`).
- Uses short-term signals from both memory recalls and daily-ingestion passes, plus light/REM phase reinforcement signals.
- When dreaming is enabled, `memory-core` auto-manages one cron job that runs a full sweep (`light -> REM -> deep`) in the background (no manual `openclaw cron add` required).
- `--agent <id>`: scope to a single agent (default: the default agent).
- `--limit <n>`: max candidates to return/apply.
- `--min-score <n>`: minimum weighted promotion score.
- `--min-recall-count <n>`: minimum recall count required for a candidate.
- `--min-unique-queries <n>`: minimum distinct query count required for a candidate.
- `--apply`: append selected candidates into `MEMORY.md` and mark them promoted.
- `--include-promoted`: include already promoted candidates in output.
- `--j

_… [truncated; see https://docs.openclaw.ai/cli/memory for full content]_


---

## Message - OpenClaw

_Source: <https://docs.openclaw.ai/cli/message>_

# `openclaw message`

Single outbound command for sending messages and channel actions
(Discord/Google Chat/iMessage/Matrix/Mattermost (plugin)/Microsoft Teams/Signal/Slack/Telegram/WhatsApp).

## Usage

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

## Common flags

- `--channel <name>`
- `--account <id>`
- `--target <dest>` (target channel or user for send/poll/read/etc)
- `--targets <name>` (repeat; broadcast only)
- `--json`
- `--dry-run`
- `--verbose`

## SecretRef behavior

- `openclaw message` resolves supported channel SecretRefs before running the selected action.
- Resolution is scoped to the active action target when possible:
  - channel-scoped when `--channel` is set (or inferred from prefixed targets like `discord:...`)
  - account-scoped when `--account` is set (channel globals + selected account surfaces)
  - when `--account` is omitted, OpenClaw does not force a `default` account SecretRef scope
- Unresolved SecretRefs on unrelated channels do not block a targeted message action.
- If the selected channel/account SecretRef is unresolved, the command fails closed for that action.

## Actions

### Core

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
  - Required: `--target`, `--poll-question`, `--poll-option` (repea

_… [truncated; see https://docs.openclaw.ai/cli/message for full content]_


---

## Nodes - OpenClaw

_Source: <https://docs.openclaw.ai/cli/nodes>_

# `openclaw nodes`

Manage paired nodes (devices) and invoke node capabilities.Related:

- Nodes overview: [Nodes](https://docs.openclaw.ai/nodes)
- Camera: [Camera nodes](https://docs.openclaw.ai/nodes/camera)
- Images: [Image nodes](https://docs.openclaw.ai/nodes/images)

Common options:

- `--url`, `--token`, `--timeout`, `--json`

## Common commands

```
openclaw nodes list
openclaw nodes list --connected
openclaw nodes list --last-connected 24h
openclaw nodes pending
openclaw nodes approve <requestId>
openclaw nodes reject <requestId>
openclaw nodes remove --node <id|name|ip>
openclaw nodes rename --node <id|name|ip> --name <displayName>
openclaw nodes status
openclaw nodes status --connected
openclaw nodes status --last-connected 24h
```

`nodes list` prints pending/paired tables. Paired rows include the most recent connect age (Last Connect).
Use `--connected` to only show currently-connected nodes. Use `--last-connected <duration>` to
filter to nodes that connected within a duration (e.g. `24h`, `7d`).
Use `nodes remove --node <id|name|ip>` to delete a stale gateway-owned node pairing record.Approval note:

- `openclaw nodes pending` only needs pairing scope.
- `gateway.nodes.pairing.autoApproveCidrs` can skip the pending step only for
explicitly trusted, first-time `role: node` device pairing. It is off by
default and does not approve upgrades.
- `openclaw nodes approve <requestId>`inherits extra scope requirements from the
pending request:

  - commandless request: pairing only
  - non-exec node commands: pairing + write
  - `system.run` / `system.run.prepare` / `system.which`: pairing + admin

## Invoke

```
openclaw nodes invoke --node <id|name|ip> --command <command> --params <json>
```

Invoke flags:

- `--params <json>`: JSON object string (default `{}`).
- `--invoke-timeout <ms>`: node invoke timeout (default `15000`).
- `--idempotency-key <key>`: optional idempotency key.
- `system.run` and `system.run.prepare` are blocked here; use the `exec` tool with `host=node` for shell execution.

For shell execution on a node, use the `exec` tool with `host=node` instead of `openclaw nodes run`.
The `nodes` CLI is now capability-focused: direct RPC via `nodes invoke`, plus pairing, camera,
screen, location, canvas, and notifications.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Nodes](https://docs.openclaw.ai/nodes)

[Node](https://docs.openclaw.ai/cli/node) [Sandbox CLI](https://docs.openclaw.ai/cli/sandbox)

Ctrl+I


---

## Proxy - OpenClaw

_Source: <https://docs.openclaw.ai/cli/proxy>_

# `openclaw proxy`

Validate operator-managed proxy routing, or run the local explicit debug proxy
and inspect captured traffic.Use `validate` to preflight an operator-managed forward proxy before enabling
OpenClaw proxy routing. The other commands are debugging tools for
transport-level investigation: they can start a local proxy, run a child command
with capture enabled, list capture sessions, query common traffic patterns, read
captured blobs, and purge local capture data.

## Commands

```
openclaw proxy start [--host <host>] [--port <port>]
openclaw proxy run [--host <host>] [--port <port>] -- <cmd...>
openclaw proxy validate [--json] [--proxy-url <url>] [--allowed-url <url>] [--denied-url <url>] [--timeout-ms <ms>]
openclaw proxy coverage
openclaw proxy sessions [--limit <count>]
openclaw proxy query --preset <name> [--session <id>]
openclaw proxy blob --id <blobId>
openclaw proxy purge
```

## Validate

`openclaw proxy validate` checks the effective operator-managed proxy URL from
`--proxy-url`, config, or `OPENCLAW_PROXY_URL`. It reports a config problem when
no proxy is enabled and configured; use `--proxy-url` for a one-off preflight
before changing config. By default it verifies that a public destination succeeds
through the proxy and that the proxy cannot reach a temporary loopback canary.
Custom denied destinations are fail-closed: HTTP responses and ambiguous
transport failures both fail unless you can verify a deployment-specific denial
signal separately.Options:

- `--json`: print machine-readable JSON.
- `--proxy-url <url>`: validate this proxy URL instead of config or env.
- `--allowed-url <url>`: add a destination expected to succeed through the proxy. Repeat to check multiple destinations.
- `--denied-url <url>`: add a destination expected to be blocked by the proxy. Repeat to check multiple destinations.
- `--timeout-ms <ms>`: per-request timeout in milliseconds.

See [Network Proxy](https://docs.openclaw.ai/security/network-proxy) for deployment guidance and denial
semantics.

## Query presets

`openclaw proxy query --preset <name>` accepts:

- `double-sends`
- `retry-storms`
- `cache-busting`
- `ws-duplicate-frames`
- `missing-ack`
- `error-bursts`

## Notes

- `start` defaults to `127.0.0.1` unless `--host` is set.
- `run` starts a local debug proxy and then runs the command after `--`.
- `validate` exits with code 1 when proxy config or destination checks fail.
- Captures are local debugging data; use `openclaw proxy purge` when finished.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Network Proxy](https://docs.openclaw.ai/security/network-proxy)
- [Trusted proxy auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth)

[MCP](https://docs.openclaw.ai/cli/mcp) [Wiki](https://docs.openclaw.ai/cli/wiki)

Ctrl+I


---

## https://docs.openclaw.ai/cli/qr.md

_Source: <https://docs.openclaw.ai/cli/qr.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# QR

\# \`openclaw qr\`

Generate a mobile pairing QR and setup code from your current Gateway configuration.

\## Usage

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw qr
openclaw qr --setup-code-only
openclaw qr --json
openclaw qr --remote
openclaw qr --url wss://gateway.example/ws
\`\`\`

\## Options

\\* \`--remote\`: prefer \`gateway.remote.url\`; if it is unset, \`gateway.tailscale.mode=serve\|funnel\` can still provide the remote public URL
\\* \`--url \`: override gateway URL used in payload
\\* \`--public-url \`: override public URL used in payload
\\* \`--token \`: override which gateway token the bootstrap flow authenticates against
\\* \`--password \`: override which gateway password the bootstrap flow authenticates against
\\* \`--setup-code-only\`: print only setup code
\\* \`--no-ascii\`: skip ASCII QR rendering
\\* \`--json\`: emit JSON (\`setupCode\`, \`gatewayUrl\`, \`auth\`, \`urlSource\`)

\## Notes

\\* \`--token\` and \`--password\` are mutually exclusive.
\\* The setup code itself now carries an opaque short-lived \`bootstrapToken\`, not the shared gateway token/password.
\\* In the built-in node/operator bootstrap flow, the primary node token still lands with \`scopes: \[\]\`.
\\* If bootstrap handoff also issues an operator token, it stays bounded to the bootstrap allowlist: \`operator.approvals\`, \`operator.read\`, \`operator.talk.secrets\`, \`operator.write\`.
\\* Bootstrap scope checks are role-prefixed. That operator allowlist only satisfies operator requests; non-operator roles still need scopes under their own role prefix.
\\* Mobile pairing fails closed for Tailscale/public \`ws://\` gateway URLs. Private LAN \`ws://\` remains supported, but Tailscale/public mobile routes should use Tailscale Serve/Funnel or a \`wss://\` gateway URL.
\\* With \`--remote\`, OpenClaw requires either \`gateway.remote.url\` or
 \`gateway.tailscale.mode=serve\|funnel\`.
\\* With \`--remote\`, if effectively active remote credentials are configured as SecretRefs and you do not pass \`--token\` or \`--password\`, the command resolves them from the active gateway snapshot. If gateway is unavailable, the command fails fast.
\\* Without \`--remote\`, local gateway auth SecretRefs are resolved when no CLI auth override is passed:
 \\* \`gateway.auth.token\` resolves when token auth can win (explicit \`gateway.auth.mode="token"\` or inferred mode where no password source wins).
 \\* \`gateway.auth.password\` resolves when password auth can win (explicit \`gateway.auth.mode="password"\` or inferred mode with no winning token from auth/env).
\\* If both \`gateway.auth.token\` and \`gateway.auth.password\` are configured (including SecretRefs) and \`gateway.auth.mode\` is unset, setup-code resolution fails until mode is set explicitly.
\\* Gateway version skew note: this command path requires a gateway that supports \`secrets.resolve\`; older gateways return an unknown-method error.
\\* After scanning, approve device pairing with:
 \\* \`openclaw devices list\`
 \\* \`openclaw devices approve \`

\## Related

\\* \[CLI reference\](/cli)
\\* \[Pairing\](/cli/pairing)


---

## Reset - OpenClaw

_Source: <https://docs.openclaw.ai/cli/reset>_

# `openclaw reset`

Reset local config/state (keeps the CLI installed).Options:

- `--scope <scope>`: `config`, `config+creds+sessions`, or `full`
- `--yes`: skip confirmation prompts
- `--non-interactive`: disable prompts; requires `--scope` and `--yes`
- `--dry-run`: print actions without removing files

Examples:

```
openclaw backup create
openclaw reset
openclaw reset --dry-run
openclaw reset --scope config --yes --non-interactive
openclaw reset --scope config+creds+sessions --yes --non-interactive
openclaw reset --scope full --yes --non-interactive
```

Notes:

- Run `openclaw backup create` first if you want a restorable snapshot before removing local state.
- If you omit `--scope`, `openclaw reset` uses an interactive prompt to choose what to remove.
- `--non-interactive` is only valid when both `--scope` and `--yes` are set.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)

[Onboard](https://docs.openclaw.ai/cli/onboard) [Secrets](https://docs.openclaw.ai/cli/secrets)

Ctrl+I


---

## Secrets - OpenClaw

_Source: <https://docs.openclaw.ai/cli/secrets>_

# `openclaw secrets`

Use `openclaw secrets` to manage SecretRefs and keep the active runtime snapshot healthy.Command roles:

- `reload`: gateway RPC (`secrets.reload`) that re-resolves refs and swaps runtime snapshot only on full success (no config writes).
- `audit`: read-only scan of configuration/auth/generated-model stores and legacy residues for plaintext, unresolved refs, and precedence drift (exec refs are skipped unless `--allow-exec` is set).
- `configure`: interactive planner for provider setup, target mapping, and preflight (TTY required).
- `apply`: execute a saved plan (`--dry-run` for validation only; dry-run skips exec checks by default, and write mode rejects exec-containing plans unless `--allow-exec` is set), then scrub targeted plaintext residues.

Recommended operator loop:

```
openclaw secrets audit --check
openclaw secrets configure
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json
openclaw secrets audit --check
openclaw secrets reload
```

If your plan includes `exec` SecretRefs/providers, pass `--allow-exec` on both dry-run and write apply commands.Exit code note for CI/gates:

- `audit --check` returns `1` on findings.
- unresolved refs return `2`.

Related:

- Secrets guide: [Secrets Management](https://docs.openclaw.ai/gateway/secrets)
- Credential surface: [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface)
- Security guide: [Security](https://docs.openclaw.ai/gateway/security)

## Reload runtime snapshot

Re-resolve secret refs and atomically swap runtime snapshot.

```
openclaw secrets reload
openclaw secrets reload --json
openclaw secrets reload --url ws://127.0.0.1:18789 --token <token>
```

Notes:

- Uses gateway RPC method `secrets.reload`.
- If resolution fails, gateway keeps last-known-good snapshot and returns an error (no partial activation).
- JSON response includes `warningCount`.

Options:

- `--url <url>`
- `--token <token>`
- `--timeout <ms>`
- `--json`

## Audit

Scan OpenClaw state for:

- plaintext secret storage
- unresolved refs
- precedence drift (`auth-profiles.json` credentials shadowing `openclaw.json` refs)
- generated `agents/*/agent/models.json` residues (provider `apiKey` values and sensitive provider headers)
- legacy residues (legacy auth store entries, OAuth reminders)

Header residue note:

- Sensitive provider header detection is name-heuristic based (common auth/credential header names and fragments such as `authorization`, `x-api-key`, `token`, `secret`, `password`, and `credential`).

```
openclaw secrets audit
openclaw secrets audit --check
openclaw secrets audit --json
openclaw secrets audit --allow-exec
```

Exit behavior:

- `--check` exits non-zero on findings.
- unresolved refs exit with higher-priority non-zero code.

Report shape highlights:

- `status`: `clean | findings | unresolved`
- `resolution`: `refsChecked`, `skippedExecRefs`, `resolvabilityComplete`
- `summary`: `plaintextCount`, `unresolvedRefCount`, `shadowedRefCount`, `legacyResidueCount`
- finding codes:
  - `PLAINTEXT_FOUND`
  - `REF_UNRESOLVED`
  - `REF_SHADOWED`
  - `LEGACY_RESIDUE`

## Configure (interactive helper)

Build provider and SecretRef changes interactively, run preflight, and optionally apply:

```
openclaw secrets configure
openclaw secrets configure --plan-out /tmp/openclaw-secrets-plan.json
openclaw secrets configure --apply --yes
openclaw secrets configure --providers-only
openclaw secrets configure --skip-provider-setup
openclaw secrets configure --agent ops
openclaw secrets configure --json
```

Flow:

- Provider setup first (`add/edit/remove` for `secrets.providers` aliases).
- Credential mapping second (select fields and assign `{source, provider, id}` refs).
- Preflight and optional apply last.

Flags:

- `--providers-only`: configure `secrets.providers` only, skip credential mapping.
- `--skip-provider-setup`: skip provider setup and map cr

_… [truncated; see https://docs.openclaw.ai/cli/secrets for full content]_


---

## Sessions - OpenClaw

_Source: <https://docs.openclaw.ai/cli/sessions>_

# `openclaw sessions`

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

## Cleanup maintenance

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
- `--store <path>`: run against a specific `se

_… [truncated; see https://docs.openclaw.ai/cli/sessions for full content]_


---

## Setup - OpenClaw

_Source: <https://docs.openclaw.ai/cli/setup>_

# `openclaw setup`

Initialize `~/.openclaw/openclaw.json` and the agent workspace.Related:

- Getting started: [Getting started](https://docs.openclaw.ai/start/getting-started)
- CLI onboarding: [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard)

## Examples

```
openclaw setup
openclaw setup --workspace ~/.openclaw/workspace
openclaw setup --wizard
openclaw setup --wizard --import-from hermes --import-source ~/.hermes
openclaw setup --non-interactive --mode remote --remote-url wss://gateway-host:18789 --remote-token <token>
```

## Options

- `--workspace <dir>`: agent workspace directory (stored as `agents.defaults.workspace`)
- `--wizard`: run onboarding
- `--non-interactive`: run onboarding without prompts
- `--mode <local|remote>`: onboarding mode
- `--import-from <provider>`: migration provider to run during onboarding
- `--import-source <path>`: source agent home for `--import-from`
- `--import-secrets`: import supported secrets during onboarding migration
- `--remote-url <url>`: remote Gateway WebSocket URL
- `--remote-token <token>`: remote Gateway token

To run onboarding via setup:

```
openclaw setup --wizard
```

Notes:

- Plain `openclaw setup` initializes config + workspace without the full onboarding flow.
- After plain setup, run `openclaw configure` to choose models, channels, Gateway, plugins, skills, or health checks.
- Onboarding auto-runs when any onboarding flags are present (`--wizard`, `--non-interactive`, `--mode`, `--import-from`, `--import-source`, `--import-secrets`, `--remote-url`, `--remote-token`).
- If Hermes state is detected, interactive onboarding can offer migration automatically. Import onboarding requires a fresh setup; use [Migrate](https://docs.openclaw.ai/cli/migrate) for dry-run plans, backups, and overwrite mode outside onboarding.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Install overview](https://docs.openclaw.ai/install)

[Security](https://docs.openclaw.ai/cli/security) [Status](https://docs.openclaw.ai/cli/status)

Ctrl+I


---

## TUI - OpenClaw

_Source: <https://docs.openclaw.ai/cli/tui>_

# `openclaw tui`

Open the terminal UI connected to the Gateway, or run it in local embedded
mode.Related:

- TUI guide: [TUI](https://docs.openclaw.ai/web/tui)

Notes:

- `chat` and `terminal` are aliases for `openclaw tui --local`.
- `--local` cannot be combined with `--url`, `--token`, or `--password`.
- `tui` resolves configured gateway auth SecretRefs for token/password auth when possible (`env`/`file`/`exec` providers).
- When launched from inside a configured agent workspace directory, TUI auto-selects that agent for the session key default (unless `--session` is explicitly `agent:<id>:...`).
- Local mode uses the embedded agent runtime directly. Most local tools work, but Gateway-only features are unavailable.
- Local mode adds `/auth [provider]` inside the TUI command surface.
- Plugin approval gates still apply in local mode. Tools that require approval prompt for a decision in the terminal; nothing is silently auto-approved because the Gateway is not involved.

## Examples

```
openclaw chat
openclaw tui --local
openclaw tui
openclaw tui --url ws://127.0.0.1:18789 --token <token>
openclaw tui --session main --deliver
openclaw chat --message "Compare my config to the docs and tell me what to fix"
# when run inside an agent workspace, infers that agent automatically
openclaw tui --session bugfix
```

## Config repair loop

Use local mode when the current config already validates and you want the
embedded agent to inspect it, compare it against the docs, and help repair it
from the same terminal:If `openclaw config validate` is already failing, use `openclaw configure` or
`openclaw doctor --fix` first. `openclaw chat` does not bypass the invalid-
config guard.

```
openclaw chat
```

Then inside the TUI:

```
!openclaw config file
!openclaw docs gateway auth token secretref
!openclaw config validate
!openclaw doctor
```

Apply targeted fixes with `openclaw config set` or `openclaw configure`, then
rerun `openclaw config validate`. See [TUI](https://docs.openclaw.ai/web/tui) and [Config](https://docs.openclaw.ai/cli/config).

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [TUI](https://docs.openclaw.ai/web/tui)

[Dashboard](https://docs.openclaw.ai/cli/dashboard) [ACP](https://docs.openclaw.ai/cli/acp)

Ctrl+I


---

## Update - OpenClaw

_Source: <https://docs.openclaw.ai/cli/update>_

# `openclaw update`

Safely update OpenClaw and switch between stable/beta/dev channels.If you installed via **npm/pnpm/bun** (global install, no git metadata),
updates happen via the package-manager flow in [Updating](https://docs.openclaw.ai/install/updating).

## Usage

```
openclaw update
openclaw update status
openclaw update wizard
openclaw update --channel beta
openclaw update --channel dev
openclaw update --tag beta
openclaw update --tag main
openclaw update --dry-run
openclaw update --no-restart
openclaw update --yes
openclaw update --json
openclaw --update
```

## Options

- `--no-restart`: skip restarting the Gateway service after a successful update. Package-manager updates that do restart the Gateway verify the restarted service reports the expected updated version before the command succeeds.
- `--channel <stable|beta|dev>`: set the update channel (git + npm; persisted in config).
- `--tag <dist-tag|version|spec>`: override the package target for this update only. For package installs, `main` maps to `github:openclaw/openclaw#main`.
- `--dry-run`: preview planned update actions (channel/tag/target/restart flow) without writing config, installing, syncing plugins, or restarting.
- `--json`: print machine-readable `UpdateRunResult` JSON, including
`postUpdate.plugins.integrityDrifts` when npm plugin artifact drift is
detected during post-update plugin sync.
- `--timeout <seconds>`: per-step timeout (default is 1800s).
- `--yes`: skip confirmation prompts (for example downgrade confirmation).

Downgrades require confirmation because older versions can break configuration.

## `update status`

Show the active update channel + git tag/branch/SHA (for source checkouts), plus update availability.

```
openclaw update status
openclaw update status --json
openclaw update status --timeout 10
```

Options:

- `--json`: print machine-readable status JSON.
- `--timeout <seconds>`: timeout for checks (default is 3s).

## `update wizard`

Interactive flow to pick an update channel and confirm whether to restart the Gateway
after updating (default is to restart). If you select `dev` without a git checkout, it
offers to create one.Options:

- `--timeout <seconds>`: timeout for each update step (default `1800`)

## What it does

When you switch channels explicitly (`--channel ...`), OpenClaw also keeps the
install method aligned:

- `dev` → ensures a git checkout (default: `~/openclaw`, override with `OPENCLAW_GIT_DIR`),
updates it, and installs the global CLI from that checkout.
- `stable` → installs from npm using `latest`.
- `beta` → prefers npm dist-tag `beta`, but falls back to `latest` when beta is
missing or older than the current stable release.

The Gateway core auto-updater (when enabled via config) launches the CLI update path
outside the live Gateway request handler. Control-plane `update.run` package-manager
updates force a non-deferred, no-cooldown update restart after the package swap,
because the old Gateway process may still have in-memory chunks that point at
files removed by the new package.For package-manager installs, `openclaw update` resolves the target package
version before invoking the package manager. npm global installs use a staged
install: OpenClaw installs the new package into a temporary npm prefix, verifies
the packaged `dist` inventory there, then swaps that clean package tree into the
real global prefix. If verification fails, post-update doctor, plugin sync, and
restart work do not run from the suspect tree. Even when the installed version
already matches the target, the command refreshes the global package install,
then runs plugin sync, a core-command completion refresh, and restart work. This
keeps packaged sidecars and channel-owned plugin records aligned with the
installed OpenClaw build while leaving full plugin-command completion rebuilds to
explicit `openclaw completion --write-state` runs.When a local managed Gateway service is installed and restart is enabled,
package-manager updates s

_… [truncated; see https://docs.openclaw.ai/cli/update for full content]_


---

## Wiki - OpenClaw

_Source: <https://docs.openclaw.ai/cli/wiki>_

# `openclaw wiki`

Inspect and maintain the `memory-wiki` vault.Provided by the bundled `memory-wiki` plugin.Related:

- [Memory Wiki plugin](https://docs.openclaw.ai/plugins/memory-wiki)
- [Memory Overview](https://docs.openclaw.ai/concepts/memory)
- [CLI: memory](https://docs.openclaw.ai/cli/memory)

## What it is for

Use `openclaw wiki` when you want a compiled knowledge vault with:

- wiki-native search and page reads
- provenance-rich syntheses
- contradiction and freshness reports
- bridge imports from the active memory plugin
- optional Obsidian CLI helpers

## Common commands

```
openclaw wiki status
openclaw wiki doctor
openclaw wiki init
openclaw wiki ingest ./notes/alpha.md
openclaw wiki compile
openclaw wiki lint
openclaw wiki search "alpha"
openclaw wiki search "who should I ask about Teams?" --mode route-question
openclaw wiki get entity.alpha --from 1 --lines 80

openclaw wiki apply synthesis "Alpha Summary" \
  --body "Short synthesis body" \
  --source-id source.alpha

openclaw wiki apply metadata entity.alpha \
  --source-id source.alpha \
  --status review \
  --question "Still active?"

openclaw wiki bridge import
openclaw wiki unsafe-local import

openclaw wiki obsidian status
openclaw wiki obsidian search "alpha"
openclaw wiki obsidian open syntheses/alpha-summary.md
openclaw wiki obsidian command workspace:quick-switcher
openclaw wiki obsidian daily
```

## Commands

### `wiki status`

Inspect current vault mode, health, and Obsidian CLI availability.Use this first when you are unsure whether the vault is initialized, bridge mode
is healthy, or Obsidian integration is available.When bridge mode is active and configured to read memory artifacts, this command
queries the running Gateway so it sees the same active memory plugin context as
agent/runtime memory.

### `wiki doctor`

Run wiki health checks and surface configuration or vault problems.When bridge mode is active and configured to read memory artifacts, this command
queries the running Gateway before building the report. Disabled bridge imports
and bridge configs that do not read memory artifacts remain local/offline.Typical issues include:

- bridge mode enabled without public memory artifacts
- invalid or missing vault layout
- missing external Obsidian CLI when Obsidian mode is expected

### `wiki init`

Create the wiki vault layout and starter pages.This initializes the root structure, including top-level indexes and cache
directories.

### `wiki ingest <path-or-url>`

Import content into the wiki source layer.Notes:

- URL ingest is controlled by `ingest.allowUrlIngest`
- imported source pages keep provenance in frontmatter
- auto-compile can run after ingest when enabled

### `wiki compile`

Rebuild indexes, related blocks, dashboards, and compiled digests.This writes stable machine-facing artifacts under:

- `.openclaw-wiki/cache/agent-digest.json`
- `.openclaw-wiki/cache/claims.jsonl`

If `render.createDashboards` is enabled, compile also refreshes report pages.

### `wiki lint`

Lint the vault and report:

- structural issues
- provenance gaps
- contradictions
- open questions
- low-confidence pages/claims
- stale pages/claims

Run this after meaningful wiki updates.

### `wiki search <query>`

Search wiki content.Behavior depends on config:

- `search.backend`: `shared` or `local`
- `search.corpus`: `wiki`, `memory`, or `all`
- `--mode`: `auto`, `find-person`, `route-question`, `source-evidence`, or
`raw-claim`

Use `wiki search` when you want wiki-specific ranking or provenance details.
For one broad shared recall pass, prefer `openclaw memory search` when the
active memory plugin exposes shared search.Search modes help the agent choose the right surface:

- `find-person`: aliases, handles, socials, canonical IDs, and person pages
- `route-question`: ask-for/best-used-for hints and relationship context
- `source-evidence`: source pages and structured evidence fields
- `raw-claim`: structured claim text with claim/evidence metadata

Exa

_… [truncated; see https://docs.openclaw.ai/cli/wiki for full content]_
