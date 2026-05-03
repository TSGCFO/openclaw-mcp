---
source_url: https://docs.openclaw.ai/tools/acp-agents-setup
title: "ACP agents \u2014 setup - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/acp-agents-setup#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Agent coordination

ACP agents — setup

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [acpx harness support (current)](https://docs.openclaw.ai/tools/acp-agents-setup#acpx-harness-support-current)
- [Required config](https://docs.openclaw.ai/tools/acp-agents-setup#required-config)
- [Plugin setup for acpx backend](https://docs.openclaw.ai/tools/acp-agents-setup#plugin-setup-for-acpx-backend)
- [acpx command and version configuration](https://docs.openclaw.ai/tools/acp-agents-setup#acpx-command-and-version-configuration)
- [Automatic dependency install](https://docs.openclaw.ai/tools/acp-agents-setup#automatic-dependency-install)
- [Plugin tools MCP bridge](https://docs.openclaw.ai/tools/acp-agents-setup#plugin-tools-mcp-bridge)
- [OpenClaw tools MCP bridge](https://docs.openclaw.ai/tools/acp-agents-setup#openclaw-tools-mcp-bridge)
- [Runtime timeout configuration](https://docs.openclaw.ai/tools/acp-agents-setup#runtime-timeout-configuration)
- [Health probe agent configuration](https://docs.openclaw.ai/tools/acp-agents-setup#health-probe-agent-configuration)
- [Permission configuration](https://docs.openclaw.ai/tools/acp-agents-setup#permission-configuration)
- [permissionMode](https://docs.openclaw.ai/tools/acp-agents-setup#permissionmode)
- [nonInteractivePermissions](https://docs.openclaw.ai/tools/acp-agents-setup#noninteractivepermissions)
- [Configuration](https://docs.openclaw.ai/tools/acp-agents-setup#configuration)
- [Related](https://docs.openclaw.ai/tools/acp-agents-setup#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

For the overview, operator runbook, and concepts, see [ACP agents](https://docs.openclaw.ai/tools/acp-agents).The sections below cover acpx harness config, plugin setup for the MCP bridges, and permission configuration.Use this page only when you are setting up the ACP/acpx route. For native Codex
app-server runtime config, use [Codex harness](https://docs.openclaw.ai/plugins/codex-harness). For
OpenAI API keys or Codex OAuth model-provider config, use
[OpenAI](https://docs.openclaw.ai/providers/openai).Codex has two OpenClaw routes:

| Route | Config/command | Setup page |
| --- | --- | --- |
| Native Codex app-server | `/codex ...`, `agentRuntime.id: "codex"` | [Codex harness](https://docs.openclaw.ai/plugins/codex-harness) |
| Explicit Codex ACP adapter | `/acp spawn codex`, `runtime: "acp", agentId: "codex"` | This page |

Prefer the native route unless you explicitly need ACP/acpx behavior.

## [​](https://docs.openclaw.ai/tools/acp-agents-setup\#acpx-harness-support-current)  acpx harness support (current)

Current acpx built-in harness aliases:

- `claude`
- `codex`
- `copilot`
- `cursor` (Cursor CLI: `cursor-agent acp`)
- `droid`
- `gemini`
- `iflow`
- `kilocode`
- `kimi`
- `kiro`
- `openclaw`
- `opencode`
- `pi`
- `qwen`

When OpenClaw uses the acpx backend, prefer these values for `agentId` unless your acpx config defines custom agent aliases.
If your local Cursor install still exposes ACP as `agent acp`, override the `cursor` agent command in your acpx config instead of changing the built-in default.Direct acpx CLI usage can also target arbitrary adapters via `--agent <command>`, but that raw escape hatch is an acpx CLI feature (not the normal OpenClaw `agentId` path).Model control is adapter-capability dependent. Codex ACP model refs are
normalized by OpenClaw before startup. Other harnesses need ACP `models` plus
`session/set_model` support; if a harness exposes neither that ACP capability
nor its own startup model flag, OpenClaw/acpx cannot force a model selection.

## [​](https://docs.openclaw.ai/tools/acp-agents-setup\#required-config)  Required config

Core ACP baseline:

```
{
  acp: {
    enabled: true,
    // Optional. Default is true; set false to pause ACP dispatch while keeping /acp controls.
    dispatch: { enabled: true },
    backend: "acpx",
    defaultAgent: "codex",
    allowedAgents: [\
      "claude",\
      "codex",\
      "copilot",\
      "cursor",\
      "droid",\
      "gemini",\
      "iflow",\
      "kilocode",\
      "kimi",\
      "kiro",\
      "openclaw",\
      "opencode",\
      "pi",\
      "qwen",\
    ],
    maxConcurrentSessions: 8,
    stream: {
      coalesceIdleMs: 300,
      maxChunkChars: 1200,
    },
    runtime: {
      ttlMinutes: 120,
    },
  },
}
```

Thread binding config is channel-adapter specific. Example for Discord:

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
        spawnAcpSessions: true,
      },
    },
  },
}
```

If thread-bound ACP spawn does not work, verify the adapter feature flag first:

- Discord: `channels.discord.threadBindings.spawnAcpSessions=true`

Current-conversation binds do not require child-thread creation. They require an active conversation context and a channel adapter that exposes ACP conversation bindings.See [Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference).

## [​](https://docs.openclaw.ai/tools/acp-agents-setup\#plugin-setup-for-acpx-backend)  Plugin setup for acpx backend

Fresh installs ship the bundled `acpx` runtime plugin enabled by default, so ACP
usually works without a manual plugin install step.Start with:

```
/acp doctor
```

If you disabled `acpx`, denied it via `plugins.allow` / `plugins.deny`, or want
to switch to a local development checkout, use the explicit plugin path:

```
openclaw plugins install acpx
openclaw config set plugins.entries.acpx.enabled true
```

Local workspace install during development:

```
openclaw plugins install ./path/to/local/acpx-plugin
```

Then verify backend health:

```
/acp doctor
```

### [​](https://docs.openclaw.ai/tools/acp-agents-setup\#acpx-command-and-version-configuration)  acpx command and version configuration

By default, the bundled `acpx` plugin registers the embedded ACP backend without
spawning an ACP agent during Gateway startup. Run `/acp doctor` for an explicit
live probe. Set `OPENCLAW_ACPX_RUNTIME_STARTUP_PROBE=1` only when you need the
Gateway to probe the configured agent at startup.Override the command or version in plugin config:

```
{
  "plugins": {
    "entries": {
      "acpx": {
        "enabled": true,
        "config": {
          "command": "../acpx/dist/cli.js",
          "expectedVersion": "any"
        }
      }
    }
  }
}
```

- `command` accepts an absolute path, relative path (resolved from the OpenClaw workspace), or command name.
- `expectedVersion: "any"` disables strict version matching.
- Custom `command` paths disable plugin-local auto-install.

See [Plugins](https://docs.openclaw.ai/tools/plugin).

### [​](https://docs.openclaw.ai/tools/acp-agents-setup\#automatic-dependency-install)  Automatic dependency install

When you install OpenClaw globally with `npm install -g openclaw`, the acpx
runtime dependencies (platform-specific binaries) are installed automatically
via a postinstall hook. If the automatic install fails, the gateway still starts
normally and reports the missing dependency through `openclaw acp doctor`.

### [​](https://docs.openclaw.ai/tools/acp-agents-setup\#plugin-tools-mcp-bridge)  Plugin tools MCP bridge

By default, ACPX sessions do **not** expose OpenClaw plugin-registered tools to
the ACP harness.If you want ACP agents such as Codex or Claude Code to call installed
OpenClaw plugin tools such as memory recall/store, enable the dedicated bridge:

```
openclaw config set plugins.entries.acpx.config.pluginToolsMcpBridge true
```

What this does:

- Injects a built-in MCP server named `openclaw-plugin-tools` into ACPX session
bootstrap.
- Exposes plugin tools already registered by installed and enabled OpenClaw
plugins.
- Keeps the feature explicit and default-off.

Security and trust notes:

- This expands the ACP harness tool surface.
- ACP agents get access only to plugin tools already active in the gateway.
- Treat this as the same trust boundary as letting those plugins execute in
OpenClaw itself.
- Review installed plugins before enabling it.

Custom `mcpServers` still work as before. The built-in plugin-tools bridge is an
additional opt-in convenience, not a replacement for generic MCP server config.

### [​](https://docs.openclaw.ai/tools/acp-agents-setup\#openclaw-tools-mcp-bridge)  OpenClaw tools MCP bridge

By default, ACPX sessions also do **not** expose built-in OpenClaw tools through
MCP. Enable the separate core-tools bridge when an ACP agent needs selected
built-in tools such as `cron`:

```
openclaw config set plugins.entries.acpx.config.openClawToolsMcpBridge true
```

What this does:

- Injects a built-in MCP server named `openclaw-tools` into ACPX session
bootstrap.
- Exposes selected built-in OpenClaw tools. The initial server exposes `cron`.
- Keeps core-tool exposure explicit and default-off.

### [​](https://docs.openclaw.ai/tools/acp-agents-setup\#runtime-timeout-configuration)  Runtime timeout configuration

The bundled `acpx` plugin defaults embedded runtime turns to a 120-second
timeout. This gives slower harnesses such as Gemini CLI enough time to complete
ACP startup and initialization. Override it if your host needs a different
runtime limit:

```
openclaw config set plugins.entries.acpx.config.timeoutSeconds 180
```

Restart the gateway after changing this value.

### [​](https://docs.openclaw.ai/tools/acp-agents-setup\#health-probe-agent-configuration)  Health probe agent configuration

When `/acp doctor` or the opt-in startup probe checks the backend, the bundled
`acpx` plugin probes one harness agent. If `acp.allowedAgents` is set, it
defaults to the first allowed agent; otherwise it defaults to `codex`. If your
deployment needs a different ACP agent for health checks, set the probe agent
explicitly:

```
openclaw config set plugins.entries.acpx.config.probeAgent claude
```

Restart the gateway after changing this value.

## [​](https://docs.openclaw.ai/tools/acp-agents-setup\#permission-configuration)  Permission configuration

ACP sessions run non-interactively — there is no TTY to approve or deny file-write and shell-exec permission prompts. The acpx plugin provides two config keys that control how permissions are handled:These ACPX harness permissions are separate from OpenClaw exec approvals and separate from CLI-backend vendor bypass flags such as Claude CLI `--permission-mode bypassPermissions`. ACPX `approve-all` is the harness-level break-glass switch for ACP sessions.

### [​](https://docs.openclaw.ai/tools/acp-agents-setup\#permissionmode)  `permissionMode`

Controls which operations the harness agent can perform without prompting.

| Value | Behavior |
| --- | --- |
| `approve-all` | Auto-approve all file writes and shell commands. |
| `approve-reads` | Auto-approve reads only; writes and exec require prompts. |
| `deny-all` | Deny all permission prompts. |

### [​](https://docs.openclaw.ai/tools/acp-agents-setup\#noninteractivepermissions)  `nonInteractivePermissions`

Controls what happens when a permission prompt would be shown but no interactive TTY is available (which is always the case for ACP sessions).

| Value | Behavior |
| --- | --- |
| `fail` | Abort the session with `AcpRuntimeError`. **(default)** |
| `deny` | Silently deny the permission and continue (graceful degradation). |

### [​](https://docs.openclaw.ai/tools/acp-agents-setup\#configuration)  Configuration

Set via plugin config:

```
openclaw config set plugins.entries.acpx.config.permissionMode approve-all
openclaw config set plugins.entries.acpx.config.nonInteractivePermissions fail
```

Restart the gateway after changing these values.

OpenClaw defaults to `permissionMode=approve-reads` and `nonInteractivePermissions=fail`. In non-interactive ACP sessions, any write or exec that triggers a permission prompt can fail with `AcpRuntimeError: Permission prompt unavailable in non-interactive mode`.If you need to restrict permissions, set `nonInteractivePermissions` to `deny` so sessions degrade gracefully instead of crashing.

## [​](https://docs.openclaw.ai/tools/acp-agents-setup\#related)  Related

- [ACP agents](https://docs.openclaw.ai/tools/acp-agents) — overview, operator runbook, concepts
- [Sub-agents](https://docs.openclaw.ai/tools/subagents)
- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)

[ACP agents](https://docs.openclaw.ai/tools/acp-agents) [Multi-agent sandbox and tools](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools)

Ctrl+I