# Gateway Ops

_38 pages from docs.openclaw.ai_


---

## Doctor - OpenClaw

_Source: <https://docs.openclaw.ai/cli/doctor>_

# `openclaw doctor`

Health checks + quick fixes for the gateway and channels.Related:

- Troubleshooting: [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)
- Security audit: [Security](https://docs.openclaw.ai/gateway/security)

## Examples

```
openclaw doctor
openclaw doctor --repair
openclaw doctor --deep
openclaw doctor --repair --non-interactive
openclaw doctor --generate-gateway-token
```

## Options

- `--no-workspace-suggestions`: disable workspace memory/search suggestions
- `--yes`: accept defaults without prompting
- `--repair`: apply recommended non-service repairs without prompting; gateway service installs and rewrites still require interactive confirmation or explicit gateway commands
- `--fix`: alias for `--repair`
- `--force`: apply aggressive repairs, including overwriting custom service config when needed
- `--non-interactive`: run without prompts; safe migrations and non-service repairs only
- `--generate-gateway-token`: generate and configure a gateway token
- `--deep`: scan system services for extra gateway installs

Notes:

- Interactive prompts (like keychain/OAuth fixes) only run when stdin is a TTY and `--non-interactive` is **not** set. Headless runs (cron, Telegram, no terminal) will skip prompts.
- Performance: non-interactive `doctor` runs skip eager plugin loading so headless health checks stay fast. Interactive sessions still fully load plugins when a check needs their contribution.
- `--fix` (alias for `--repair`) writes a backup to `~/.openclaw/openclaw.json.bak` and drops unknown config keys, listing each removal.
- `doctor --fix --non-interactive` reports missing or stale gateway service definitions but does not install or rewrite them outside update repair mode. Run `openclaw gateway install` for a missing service, or `openclaw gateway install --force` when you intentionally want to replace the launcher.
- State integrity checks now detect orphan transcript files in the sessions directory. Archiving them as `.deleted.<timestamp>` requires an interactive confirmation; `--fix`, `--yes`, and headless runs leave them in place.
- Doctor also scans `~/.openclaw/cron/jobs.json` (or `cron.store`) for legacy cron job shapes and can rewrite them in place before the scheduler has to auto-normalize them at runtime.
- On Linux, doctor warns when the user’s crontab still runs legacy `~/.openclaw/bin/ensure-whatsapp.sh`; that script is no longer maintained and can log false WhatsApp gateway outages when cron lacks the systemd user-bus environment.
- Doctor cleans legacy plugin dependency staging state created by older OpenClaw versions. It also repairs missing configured downloadable plugins when the registry can resolve them, and the 2026.5.2 doctor pass automatically installs downloadable plugins that an older config already uses before marking the config touched for that release.
- Doctor repairs stale plugin config by removing missing plugin ids from `plugins.allow`/`plugins.entries`, plus matching dangling channel config, heartbeat targets, and channel model overrides when plugin discovery is healthy.
- Doctor quarantines invalid plugin config by disabling the affected `plugins.entries.<id>` entry and removing its invalid `config` payload. Gateway startup already skips only that bad plugin so other plugins and channels can keep running.
- Set `OPENCLAW_SERVICE_REPAIR_POLICY=external` when another supervisor owns the gateway lifecycle. Doctor still reports gateway/service health and applies non-service repairs, but skips service install/start/restart/bootstrap and legacy service cleanup.
- On Linux, doctor ignores inactive extra gateway-like systemd units and does not rewrite command/entrypoint metadata for a running systemd gateway service during repair. Stop the service first or use `openclaw gateway install --force` when you intentionally want to replace the active launcher.
- Doctor auto-migrates legacy flat Talk config (`talk.voiceId`, `talk.modelId`, and friends) into `talk.pr

_… [truncated; see https://docs.openclaw.ai/cli/doctor for full content]_


---

## Gateway - OpenClaw

_Source: <https://docs.openclaw.ai/cli/gateway>_

[OpenClaw home page](https://docs.openclaw.ai/)

Gateway and service

Gateway

The Gateway is OpenClaw’s WebSocket server (channels, nodes, sessions, hooks). Subcommands in this page live under `openclaw gateway …`.

[**Bonjour discovery** \\
\\
Local mDNS + wide-area DNS-SD setup.](https://docs.openclaw.ai/gateway/bonjour)

[**Discovery overview** \\
\\
How OpenClaw advertises and finds gateways.](https://docs.openclaw.ai/gateway/discovery)

[**Configuration** \\
\\
Top-level gateway config keys.](https://docs.openclaw.ai/gateway/configuration)

## Run the Gateway

Run a local Gateway process:

```
openclaw gateway
```

Foreground alias:

```
openclaw gateway run
```

Startup behavior

- By default, the Gateway refuses to start unless `gateway.mode=local` is set in `~/.openclaw/openclaw.json`. Use `--allow-unconfigured` for ad-hoc/dev runs.
- `openclaw onboard --mode local` and `openclaw setup` are expected to write `gateway.mode=local`. If the file exists but `gateway.mode` is missing, treat that as a broken or clobbered config and repair it instead of assuming local mode implicitly.
- If the file exists and `gateway.mode` is missing, the Gateway treats that as suspicious config damage and refuses to “guess local” for you.
- Binding beyond loopback without auth is blocked (safety guardrail).
- `SIGUSR1` triggers an in-process restart when authorized (`commands.restart` is enabled by default; set `commands.restart: false` to block manual restart, while gateway tool/config apply/update remain allowed).
- `SIGINT`/`SIGTERM` handlers stop the gateway process, but they don’t restore any custom terminal state. If you wrap the CLI with a TUI or raw-mode input, restore the terminal before exit.

### Options

[​](https://docs.openclaw.ai/cli/gateway#param-port-port)

--port <port>

number

WebSocket port (default comes from config/env; usually `18789`).

[​](https://docs.openclaw.ai/cli/gateway#param-bind-loopback-lan-tailnet-auto-custom)

--bind <loopback\|lan\|tailnet\|auto\|custom>

string

Listener bind mode.

[​](https://docs.openclaw.ai/cli/gateway#param-auth-token-password)

--auth <token\|password>

string

Auth mode override.

[​](https://docs.openclaw.ai/cli/gateway#param-token-token)

--token <token>

string

Token override (also sets `OPENCLAW_GATEWAY_TOKEN` for the process).

[​](https://docs.openclaw.ai/cli/gateway#param-password-password)

--password <password>

string

Password override.

[​](https://docs.openclaw.ai/cli/gateway#param-password-file-path)

--password-file <path>

string

Read the gateway password from a file.

[​](https://docs.openclaw.ai/cli/gateway#param-tailscale-off-serve-funnel)

--tailscale <off\|serve\|funnel>

string

Expose the Gateway via Tailscale.

[​](https://docs.openclaw.ai/cli/gateway#param-tailscale-reset-on-exit)

--tailscale-reset-on-exit

boolean

Reset Tailscale serve/funnel config on shutdown.

[​](https://docs.openclaw.ai/cli/gateway#param-allow-unconfigured)

--allow-unconfigured

boolean

Allow gateway start without `gateway.mode=local` in config. Bypasses the startup guard for ad-hoc/dev bootstrap only; does not write or repair the config file.

[​](https://docs.openclaw.ai/cli/gateway#param-dev)

--dev

boolean

Create a dev config + workspace if missing (skips BOOTSTRAP.md).

[​](https://docs.openclaw.ai/cli/gateway#param-reset)

--reset

boolean

Reset dev config + credentials + sessions + workspace (requires `--dev`).

[​](https://docs.openclaw.ai/cli/gateway#param-force)

--force

boolean

Kill any existing listener on the selected port before starting.

[​](https://docs.openclaw.ai/cli/gateway#param-verbose)

--verbose

boolean

Verbose logs.

[​](https://docs.openclaw.ai/cli/gateway#param-cli-backend-logs)

--cli-backend-logs

boolean

Only show CLI backend logs in the console (and enable stdout/stderr).

[​](https://docs.openclaw.ai/cli/gateway#param-ws-log-auto-full-compact)

--ws-log <auto\|full\|compact>

string

default:"auto"

Websocket log style.

[​](https://doc

_… [truncated; see https://docs.openclaw.ai/cli/gateway for full content]_


---

## Status - OpenClaw

_Source: <https://docs.openclaw.ai/cli/status>_

# `openclaw status`

Diagnostics for channels + sessions.

```
openclaw status
openclaw status --all
openclaw status --deep
openclaw status --usage
```

Notes:

- `--deep` runs live probes (WhatsApp Web + Telegram + Discord + Slack + Signal).
- Plain `openclaw status` stays on the fast read-only path and marks memory as `not checked` instead of unavailable when it skips memory inspection. Heavy security audit, plugin compatibility, and memory-vector probes are left to `openclaw status --all`, `openclaw status --deep`, `openclaw security audit`, and `openclaw memory status --deep`.
- `status --json --all` reports memory details from the active memory plugin runtime selected by `plugins.slots.memory`. Custom memory plugins can leave built-in `agents.defaults.memorySearch.enabled` disabled and still report their own files, chunks, vector, and FTS state.
- `--usage` prints normalized provider usage windows as `X% left`.
- Session status output separates `Execution:` from `Runtime:`. `Execution` is the sandbox path (`direct`, `docker/*`), while `Runtime` tells you whether the session is using `OpenClaw Pi Default`, `OpenAI Codex`, a CLI backend, or an ACP backend such as `codex (acp/acpx)`. See [Agent runtimes](https://docs.openclaw.ai/concepts/agent-runtimes) for the provider/model/runtime distinction.
- MiniMax’s raw `usage_percent` / `usagePercent` fields are remaining quota, so OpenClaw inverts them before display; count-based fields win when present. `model_remains` responses prefer the chat-model entry, derive the window label from timestamps when needed, and include the model name in the plan label.
- When the current session snapshot is sparse, `/status` can backfill token and cache counters from the most recent transcript usage log. Existing nonzero live values still win over transcript fallback values.
- Transcript fallback can also recover the active runtime model label when the live session entry is missing it. If that transcript model differs from the selected model, status resolves the context window against the recovered runtime model instead of the selected one.
- For prompt-size accounting, transcript fallback prefers the larger prompt-oriented total when session metadata is missing or smaller, so custom-provider sessions do not collapse to `0` token displays.
- Output includes per-agent session stores when multiple agents are configured.
- Overview includes Gateway + node host service install/runtime status when available.
- Overview includes update channel + git SHA (for source checkouts).
- Update info surfaces in the Overview; if an update is available, status prints a hint to run `openclaw update` (see [Updating](https://docs.openclaw.ai/install/updating)).
- Read-only status surfaces (`status`, `status --json`, `status --all`) resolve supported SecretRefs for their targeted config paths when possible.
- If a supported channel SecretRef is configured but unavailable in the current command path, status stays read-only and reports degraded output instead of crashing. Human output shows warnings such as “configured token unavailable in this command path”, and JSON output includes `secretDiagnostics`.
- When command-local SecretRef resolution succeeds, status prefers the resolved snapshot and clears transient “secret unavailable” channel markers from the final output.
- `status --all` includes a Secrets overview row and a diagnosis section that summarizes secret diagnostics (truncated for readability) without stopping report generation.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Doctor](https://docs.openclaw.ai/gateway/doctor)

[Setup](https://docs.openclaw.ai/cli/setup) [Uninstall](https://docs.openclaw.ai/cli/uninstall)

Ctrl+I


---

## Diagnostics flags - OpenClaw

_Source: <https://docs.openclaw.ai/diagnostics/flags>_

[OpenClaw home page](https://docs.openclaw.ai/)

Diagnostics

Diagnostics flags

Diagnostics flags let you enable targeted debug logs without turning on verbose logging everywhere. Flags are opt-in and have no effect unless a subsystem checks them.

## How it works

- Flags are strings (case-insensitive).
- You can enable flags in config or via an env override.
- Wildcards are supported:
  - `telegram.*` matches `telegram.http`
  - `*` enables all flags

## Enable via config

```
{
  "diagnostics": {
    "flags": ["telegram.http"]
  }
}
```

Multiple flags:

```
{
  "diagnostics": {
    "flags": ["telegram.http", "brave.http", "gateway.*"]
  }
}
```

Restart the gateway after changing flags.

## Env override (one-off)

```
OPENCLAW_DIAGNOSTICS=telegram.http,telegram.payload
```

Disable all flags:

```
OPENCLAW_DIAGNOSTICS=0
```

## Timeline artifacts

The `timeline` flag writes structured startup and runtime timing events for
external QA harnesses:

```
OPENCLAW_DIAGNOSTICS=timeline \
OPENCLAW_DIAGNOSTICS_TIMELINE_PATH=/tmp/openclaw-timeline.jsonl \
openclaw gateway run
```

You can also enable it in config:

```
{
  "diagnostics": {
    "flags": ["timeline"]
  }
}
```

The timeline file path still comes from
`OPENCLAW_DIAGNOSTICS_TIMELINE_PATH`. When `timeline` is enabled only from
config, the earliest config-loading spans are not emitted because OpenClaw has
not read config yet; subsequent startup spans use the config flag.`OPENCLAW_DIAGNOSTICS=1`, `OPENCLAW_DIAGNOSTICS=all`, and
`OPENCLAW_DIAGNOSTICS=*` also enable the timeline because they enable every
diagnostics flag. Prefer `timeline` when you only want the JSONL timing
artifact.Timeline records use the `openclaw.diagnostics.v1` envelope. Events can include
process ids, phase names, span names, durations, plugin ids, dependency counts,
event-loop delay samples, provider operation names, child-process exit state,
and startup error names/messages. Treat timeline files as local diagnostics
artifacts; review them before sharing outside your machine.

## Where logs go

Flags emit logs into the standard diagnostics log file. By default:

```
/tmp/openclaw/openclaw-YYYY-MM-DD.log
```

If you set `logging.file`, use that path instead. Logs are JSONL (one JSON object per line). Redaction still applies based on `logging.redactSensitive`.

## Extract logs

Pick the latest log file:

```
ls -t /tmp/openclaw/openclaw-*.log | head -n 1
```

Filter for Telegram HTTP diagnostics:

```
rg "telegram http error" /tmp/openclaw/openclaw-*.log
```

Filter for Brave Search HTTP diagnostics:

```
rg "brave http" /tmp/openclaw/openclaw-*.log
```

Or tail while reproducing:

```
tail -f /tmp/openclaw/openclaw-$(date +%F).log | rg "telegram http error"
```

For remote gateways, you can also use `openclaw logs --follow` (see [/cli/logs](https://docs.openclaw.ai/cli/logs)).

## Notes

- If `logging.level` is set higher than `warn`, these logs may be suppressed. Default `info` is fine.
- `brave.http` logs Brave Search request URLs/query params, response status/timing, and cache hit/miss/write events. It does not log API keys or response bodies, but search queries can be sensitive.
- Flags are safe to leave enabled; they only affect log volume for the specific subsystem.
- Use [/logging](https://docs.openclaw.ai/logging) to change log destinations, levels, and redaction.

## Related

- [Gateway diagnostics](https://docs.openclaw.ai/gateway/diagnostics)
- [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)

[Environment variables](https://docs.openclaw.ai/help/environment) [Node + tsx crash](https://docs.openclaw.ai/debug/node-issue)

Ctrl+I


---

## Gateway runbook - OpenClaw

_Source: <https://docs.openclaw.ai/gateway>_

# debug/trace mirrored to stdio
openclaw gateway --port 18789 --verbose
# force-kill listener on selected port, then start
openclaw gateway --force
```

2

[Navigate to header](https://docs.openclaw.ai/gateway#)

Verify service health

```
openclaw gateway status
openclaw status
openclaw logs --follow
```

Healthy baseline: `Runtime: running`, `Connectivity probe: ok`, and `Capability: ...` that matches what you expect. Use `openclaw gateway status --require-rpc` when you need read-scope RPC proof, not just reachability.

3

[Navigate to header](https://docs.openclaw.ai/gateway#)

Validate channel readiness

```
openclaw channels status --probe
```

With a reachable gateway this runs live per-account channel probes and optional audits.
If the gateway is unreachable, the CLI falls back to config-only channel summaries instead
of live probe output.

Gateway config reload watches the active config file path (resolved from profile/state defaults, or `OPENCLAW_CONFIG_PATH` when set).
Default mode is `gateway.reload.mode="hybrid"`.
After the first successful load, the running process serves the active in-memory config snapshot; successful reload swaps that snapshot atomically.

## Runtime model

- One always-on process for routing, control plane, and channel connections.
- Single multiplexed port for:
  - WebSocket control/RPC
  - HTTP APIs, OpenAI compatible (`/v1/models`, `/v1/embeddings`, `/v1/chat/completions`, `/v1/responses`, `/tools/invoke`)
  - Control UI and hooks
- Default bind mode: `loopback`.
- Auth is required by default. Shared-secret setups use
`gateway.auth.token` / `gateway.auth.password` (or
`OPENCLAW_GATEWAY_TOKEN` / `OPENCLAW_GATEWAY_PASSWORD`), and non-loopback
reverse-proxy setups can use `gateway.auth.mode: "trusted-proxy"`.

## OpenAI-compatible endpoints

OpenClaw’s highest-leverage compatibility surface is now:

- `GET /v1/models`
- `GET /v1/models/{id}`
- `POST /v1/embeddings`
- `POST /v1/chat/completions`
- `POST /v1/responses`

Why this set matters:

- Most Open WebUI, LobeChat, and LibreChat integrations probe `/v1/models` first.
- Many RAG and memory pipelines expect `/v1/embeddings`.
- Agent-native clients increasingly prefer `/v1/responses`.

Planning note:

- `/v1/models` is agent-first: it returns `openclaw`, `openclaw/default`, and `openclaw/<agentId>`.
- `openclaw/default` is the stable alias that always maps to the configured default agent.
- Use `x-openclaw-model` when you want a backend provider/model override; otherwise the selected agent’s normal model and embedding setup stays in control.

All of these run on the main Gateway port and use the same trusted operator auth boundary as the rest of the Gateway HTTP API.

### Port and bind precedence

| Setting | Resolution order |
| --- | --- |
| Gateway port | `--port` → `OPENCLAW_GATEWAY_PORT` → `gateway.port` → `18789` |
| Bind mode | CLI/override → `gateway.bind` → `loopback` |

Installed gateway services record the resolved `--port` in supervisor metadata. After changing `gateway.port`, run `openclaw doctor --fix` or `openclaw gateway install --force` so launchd/systemd/schtasks starts the process on the new port.Gateway startup uses the same effective port and bind when it seeds local
Control UI origins for non-loopback binds. For example, `--bind lan --port 3000`
seeds `http://localhost:3000` and `http://127.0.0.1:3000` before runtime
validation runs. Add any remote browser origins, such as HTTPS proxy URLs, to
`gateway.controlUi.allowedOrigins` explicitly.

### Hot reload modes

| `gateway.reload.mode` | Behavior |
| --- | --- |
| `off` | No config reload |
| `hot` | Apply only hot-safe changes |
| `restart` | Restart on reload-required changes |
| `hybrid` (default) | Hot-apply when safe, restart when required |

## Operator command set

```
openclaw gateway status
openclaw gateway status --deep   # adds a system-level service scan
openclaw gateway status --json
openclaw gateway install
openclaw gateway restart
openclaw gateway stop
ope

_… [truncated; see https://docs.openclaw.ai/gateway for full content]_


---

## Authentication - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/authentication>_

# Run on the gateway host
claude auth login
claude auth status --text
openclaw models auth login --provider anthropic --method cli --set-default
```

This is a two-step setup:

1. Log Claude Code itself into Anthropic on the gateway host.
2. Tell OpenClaw to switch Anthropic model selection to the local `claude-cli`
backend and store the matching OpenClaw auth profile.

If `claude` is not on `PATH`, either install Claude Code first or set
`agents.defaults.cliBackends.claude-cli.command` to the real binary path.Manual token entry (any provider; writes `auth-profiles.json` \+ updates config):

```
openclaw models auth paste-token --provider openrouter
```

`auth-profiles.json` stores credentials only. The canonical shape is:

```
{
  "version": 1,
  "profiles": {
    "openrouter:default": {
      "type": "api_key",
      "provider": "openrouter",
      "key": "OPENROUTER_API_KEY"
    }
  }
}
```

OpenClaw expects the canonical `version` \+ `profiles` shape at runtime. If an older install still has a flat file such as `{ "openrouter": { "apiKey": "..." } }`, run `openclaw doctor --fix` to rewrite it as an `openrouter:default` API-key profile; doctor keeps a `.legacy-flat.*.bak` copy beside the original. Endpoint details such as `baseUrl`, `api`, model ids, headers, and timeouts belong under `models.providers.<id>` in `openclaw.json` or `models.json`, not in `auth-profiles.json`.Auth profile refs are also supported for static credentials:

- `api_key` credentials can use `keyRef: { source, provider, id }`
- `token` credentials can use `tokenRef: { source, provider, id }`
- OAuth-mode profiles do not support SecretRef credentials; if `auth.profiles.<id>.mode` is set to `"oauth"`, SecretRef-backed `keyRef`/`tokenRef` input for that profile is rejected.

Automation-friendly check (exit `1` when expired/missing, `2` when expiring):

```
openclaw models status --check
```

Live auth probes:

```
openclaw models status --probe
```

Notes:

- Probe rows can come from auth profiles, env credentials, or `models.json`.
- If explicit `auth.order.<provider>` omits a stored profile, probe reports
`excluded_by_auth_order` for that profile instead of trying it.
- If auth exists but OpenClaw cannot resolve a probeable model candidate for
that provider, probe reports `status: no_model`.
- Rate-limit cooldowns can be model-scoped. A profile cooling down for one
model can still be usable for a sibling model on the same provider.

Optional ops scripts (systemd/Termux) are documented here:
[Auth monitoring scripts](https://docs.openclaw.ai/help/scripts#auth-monitoring-scripts)

## Anthropic note

The Anthropic `claude-cli` backend is supported again.

- Anthropic staff told us this OpenClaw integration path is allowed again.
- OpenClaw therefore treats Claude CLI reuse and `claude -p` usage as sanctioned
for Anthropic-backed runs unless Anthropic publishes a new policy.
- Anthropic API keys remain the most predictable choice for long-lived gateway
hosts and explicit server-side billing control.

## Checking model auth status

```
openclaw models status
openclaw doctor
```

## API key rotation behavior (gateway)

Some providers support retrying a request with alternative keys when an API call
hits a provider rate limit.

- Priority order:
  - `OPENCLAW_LIVE_<PROVIDER>_KEY` (single override)
  - `<PROVIDER>_API_KEYS`
  - `<PROVIDER>_API_KEY`
  - `<PROVIDER>_API_KEY_*`
- Google providers also include `GOOGLE_API_KEY` as an additional fallback.
- The same key list is deduplicated before use.
- OpenClaw retries with the next key only for rate-limit errors (for example
`429`, `rate_limit`, `quota`, `resource exhausted`, `Too many concurrent requests`, `ThrottlingException`, `concurrency limit reached`, or
`workers_ai ... quota limit exceeded`).
- Non-rate-limit errors are not retried with alternate keys.
- If all keys fail, the final error from the last attempt is returned.

## Controlling which credential is used

### Per-session (chat command)

Use `/mode

_… [truncated; see https://docs.openclaw.ai/gateway/authentication for full content]_


---

## Background exec and process tool - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/background-process>_

# Background Exec + Process Tool

OpenClaw runs shell commands through the `exec` tool and keeps long‑running tasks in memory. The `process` tool manages those background sessions.

## exec tool

Key parameters:

- `command` (required)
- `yieldMs` (default 10000): auto‑background after this delay
- `background` (bool): background immediately
- `timeout` (seconds, default `tools.exec.timeoutSec`): kill the process after this timeout; set `timeout: 0` only to disable the exec process timeout for that call
- `elevated` (bool): run outside the sandbox if elevated mode is enabled/allowed (`gateway` by default, or `node` when the exec target is `node`)
- Need a real TTY? Set `pty: true`.
- `workdir`, `env`

Behavior:

- Foreground runs return output directly.
- When backgrounded (explicit or timeout), the tool returns `status: "running"` \+ `sessionId` and a short tail.
- Background and `yieldMs` runs inherit `tools.exec.timeoutSec` unless the call provides an explicit `timeout`.
- Output is kept in memory until the session is polled or cleared.
- If the `process` tool is disallowed, `exec` runs synchronously and ignores `yieldMs`/`background`.
- Spawned exec commands receive `OPENCLAW_SHELL=exec` for context-aware shell/profile rules.
- For long-running work that starts now, start it once and rely on automatic
completion wake when it is enabled and the command emits output or fails.
- If automatic completion wake is unavailable, or you need quiet-success
confirmation for a command that exited cleanly without output, use `process`
to confirm completion.
- Do not emulate reminders or delayed follow-ups with `sleep` loops or repeated
polling; use cron for future work.

## Child process bridging

When spawning long-running child processes outside the exec/process tools (for example, CLI respawns or gateway helpers), attach the child-process bridge helper so termination signals are forwarded and listeners are detached on exit/error. This avoids orphaned processes on systemd and keeps shutdown behavior consistent across platforms.Environment overrides:

- `PI_BASH_YIELD_MS`: default yield (ms)
- `PI_BASH_MAX_OUTPUT_CHARS`: in‑memory output cap (chars)
- `OPENCLAW_BASH_PENDING_MAX_OUTPUT_CHARS`: pending stdout/stderr cap per stream (chars)
- `PI_BASH_JOB_TTL_MS`: TTL for finished sessions (ms, bounded to 1m–3h)

Config (preferred):

- `tools.exec.backgroundMs` (default 10000)
- `tools.exec.timeoutSec` (default 1800)
- `tools.exec.cleanupMs` (default 1800000)
- `tools.exec.notifyOnExit` (default true): enqueue a system event + request heartbeat when a backgrounded exec exits.
- `tools.exec.notifyOnExitEmptySuccess` (default false): when true, also enqueue completion events for successful backgrounded runs that produced no output.

## process tool

Actions:

- `list`: running + finished sessions
- `poll`: drain new output for a session (also reports exit status)
- `log`: read the aggregated output (supports `offset` \+ `limit`)
- `write`: send stdin (`data`, optional `eof`)
- `send-keys`: send explicit key tokens or bytes to a PTY-backed session
- `submit`: send Enter / carriage return to a PTY-backed session
- `paste`: send literal text, optionally wrapped in bracketed paste mode
- `kill`: terminate a background session
- `clear`: remove a finished session from memory
- `remove`: kill if running, otherwise clear if finished

Notes:

- Only backgrounded sessions are listed/persisted in memory.
- Sessions are lost on process restart (no disk persistence).
- Session logs are only saved to chat history if you run `process poll/log` and the tool result is recorded.
- `process` is scoped per agent; it only sees sessions started by that agent.
- Use `poll` / `log` for status, logs, quiet-success confirmation, or
completion confirmation when automatic completion wake is unavailable.
- Use `write` / `send-keys` / `submit` / `paste` / `kill` when you need input
or intervention.
- `process list` includes a derived `name` (command verb + target) for q

_… [truncated; see https://docs.openclaw.ai/gateway/background-process for full content]_


---

## Bonjour discovery - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/bonjour>_

# Bonjour / mDNS discovery

OpenClaw uses Bonjour (mDNS / DNS‑SD) to discover an active Gateway (WebSocket endpoint).
Multicast `local.` browsing is a **LAN-only convenience**. The bundled `bonjour`
plugin owns LAN advertising and is enabled by default. For cross-network discovery,
the same beacon can also be published through a configured wide-area DNS-SD domain.
Discovery is still best-effort and does **not** replace SSH or Tailnet-based connectivity.

## Wide-area Bonjour (Unicast DNS-SD) over Tailscale

If the node and gateway are on different networks, multicast mDNS won’t cross the
boundary. You can keep the same discovery UX by switching to **unicast DNS‑SD**
(“Wide‑Area Bonjour”) over Tailscale.High‑level steps:

1. Run a DNS server on the gateway host (reachable over Tailnet).
2. Publish DNS‑SD records for `_openclaw-gw._tcp` under a dedicated zone
(example: `openclaw.internal.`).
3. Configure Tailscale **split DNS** so your chosen domain resolves via that
DNS server for clients (including iOS).

OpenClaw supports any discovery domain; `openclaw.internal.` is just an example.
iOS/Android nodes browse both `local.` and your configured wide‑area domain.

### Gateway config (recommended)

```
{
  gateway: { bind: "tailnet" }, // tailnet-only (recommended)
  discovery: { wideArea: { enabled: true } }, // enables wide-area DNS-SD publishing
}
```

### One-time DNS server setup (gateway host)

```
openclaw dns setup --apply
```

This installs CoreDNS and configures it to:

- listen on port 53 only on the gateway’s Tailscale interfaces
- serve your chosen domain (example: `openclaw.internal.`) from `~/.openclaw/dns/<domain>.db`

Validate from a tailnet‑connected machine:

```
dns-sd -B _openclaw-gw._tcp openclaw.internal.
dig @<TAILNET_IPV4> -p 53 _openclaw-gw._tcp.openclaw.internal PTR +short
```

### Tailscale DNS settings

In the Tailscale admin console:

- Add a nameserver pointing at the gateway’s tailnet IP (UDP/TCP 53).
- Add split DNS so your discovery domain uses that nameserver.

Once clients accept tailnet DNS, iOS nodes and CLI discovery can browse
`_openclaw-gw._tcp` in your discovery domain without multicast.

### Gateway listener security (recommended)

The Gateway WS port (default `18789`) binds to loopback by default. For LAN/tailnet
access, bind explicitly and keep auth enabled.For tailnet‑only setups:

- Set `gateway.bind: "tailnet"` in `~/.openclaw/openclaw.json`.
- Restart the Gateway (or restart the macOS menubar app).

## What advertises

Only the Gateway advertises `_openclaw-gw._tcp`. LAN multicast advertising is
provided by the bundled `bonjour` plugin; wide-area DNS-SD publishing remains
Gateway-owned.

## Service types

- `_openclaw-gw._tcp` — gateway transport beacon (used by macOS/iOS/Android nodes).

## TXT keys (non-secret hints)

The Gateway advertises small non‑secret hints to make UI flows convenient:

- `role=gateway`
- `displayName=<friendly name>`
- `lanHost=<hostname>.local`
- `gatewayPort=<port>` (Gateway WS + HTTP)
- `gatewayTls=1` (only when TLS is enabled)
- `gatewayTlsSha256=<sha256>` (only when TLS is enabled and fingerprint is available)
- `canvasPort=<port>` (only when the canvas host is enabled; currently the same as `gatewayPort`)
- `transport=gateway`
- `tailnetDns=<magicdns>` (mDNS full mode only, optional hint when Tailnet is available)
- `sshPort=<port>` (mDNS full mode only; wide-area DNS-SD may omit it)
- `cliPath=<path>` (mDNS full mode only; wide-area DNS-SD still writes it as a remote-install hint)

Security notes:

- Bonjour/mDNS TXT records are **unauthenticated**. Clients must not treat TXT as authoritative routing.
- Clients should route using the resolved service endpoint (SRV + A/AAAA). Treat `lanHost`, `tailnetDns`, `gatewayPort`, and `gatewayTlsSha256` as hints only.
- SSH auto-targeting should likewise use the resolved service host, not TXT-only hints.
- TLS pinning must never allow an advertised `gatewayTlsSha256` to override a previously stored pin.
- iOS/A

_… [truncated; see https://docs.openclaw.ai/gateway/bonjour for full content]_


---

## Configuration — agents - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/config-agents>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Configuration — agents

- [agents.list\[\].contextLimits](https://docs.openclaw.ai/gateway/config-agents#agents-list-contextlimits)
- [skills.limits.maxSkillsPromptChars](https://docs.openclaw.ai/gateway/config-agents#skills-limits-maxskillspromptchars)
- [agents.list\[\].skillsLimits.maxSkillsPromptChars](https://docs.openclaw.ai/gateway/config-agents#agents-list-skillslimits-maxskillspromptchars)
- [agents.defaults.imageMaxDimensionPx](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-imagemaxdimensionpx)
- [agents.defaults.userTimezone](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-usertimezone)
- [agents.defaults.timeFormat](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-timeformat)
- [agents.defaults.model](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-model)
- [agents.defaults.agentRuntime](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-agentruntime)
- [agents.defaults.cliBackends](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-clibackends)
- [agents.defaults.systemPromptOverride](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-systempromptoverride)
- [agents.defaults.promptOverlays](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-promptoverlays)
- [agents.defaults.heartbeat](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-heartbeat)
- [agents.defaults.compaction](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-compaction)
- [agents.defaults.contextPruning](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-contextpruning)
- [Block streaming](https://docs.openclaw.ai/gateway/config-agents#block-streaming)
- [Typing indicators](https://docs.openclaw.ai/gateway/config-agents#typing-indicators)
- [agents.defaults.sandbox](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-sandbox)
- [agents.list (per-agent overrides)](https://docs.openclaw.ai/gateway/config-agents#agents-list-per-agent-overrides)
- [Multi-agent routing](https://docs.openclaw.ai/gateway/config-agents#multi-agent-routing)
- [Binding match fields](https://docs.openclaw.ai/gateway/config-agents#binding-match-fields)
- [Per-agent access profiles](https://docs.openclaw.ai/gateway/config-agents#per-agent-access-profiles)
- [Session](https://docs.openclaw.ai/gateway/config-agents#session)
- [Messages](https://docs.openclaw.ai/gateway/config-agents#messages)
- [Response prefix](https://docs.openclaw.ai/gateway/config-agents#response-prefix)
- [Ack reaction](https://docs.openclaw.ai/gateway/config-agents#ack-reaction)
- [Inbound debounce](https://docs.openclaw.ai/gateway/config-agents#inbound-debounce)
- [TTS (text-to-speech)](https://docs.openclaw.ai/gateway/config-agents#tts-text-to-speech)
- [Talk](https://docs.openclaw.ai/gateway/config-agents#talk)
- [Related](https://docs.openclaw.ai/gateway/config-agents#related)

Agent-scoped configuration keys under `agents.*`, `multiAgent.*`, `session.*`,
`messages.*`, and `talk.*`. For channels, tools, gateway runtime, and other
top-level keys, see [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference).

## Agent defaults

### `agents.defaults.workspace`

Default: `~/.openclaw/workspace`.

```
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
}
```

### `agents.defaults.repoRoot`

Optional repository root shown in the system prompt’s Runtime line. If unset, OpenClaw auto-detects by walking upward from the workspace.

```
{
  agents: { defaults: { repoRoot: "~/Projects/openclaw" } },
}
```

### `agents.defaults.skills`

Optional default skill allowlist for agents that do not set
`agents.list[].skills`.

```
{
  agents: {
    defaults: { skills: ["github", "weather"] },
    list: [\
      { id: "writer" }, // inherits github, weather\
      { id: "docs", skills: ["docs-search"] }, // replaces defaults\
      { id: "locked-down", skills: [] },

_… [truncated; see https://docs.openclaw.ai/gateway/config-agents for full content]_


---

## Configuration — channels - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/config-channels>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Configuration — channels

Per-channel configuration keys under `channels.*`. Covers DM and group access,
multi-account setups, mention gating, and per-channel keys for Slack, Discord,
Telegram, WhatsApp, Matrix, iMessage, and the other bundled channel plugins.For agents, tools, gateway runtime, and other top-level keys, see
[Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference).

## Channels

Each channel starts automatically when its config section exists (unless `enabled: false`).

### DM and group access

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

### Channel model overrides

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

### Channel defaults and heartbeat

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

### WhatsApp

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

_… [truncated; see https://docs.openclaw.ai/gateway/config-channels for full content]_


---

## Configuration — tools and custom providers - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/config-tools>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Configuration — tools and custom providers

`tools.*` config keys and custom provider / base-URL setup. For agents, channels, and other top-level config keys, see [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference).

## Tools

### Tool profiles

`tools.profile` sets a base allowlist before `tools.allow`/`tools.deny`:

Local onboarding defaults new local configs to `tools.profile: "coding"` when unset (existing explicit profiles are preserved).

| Profile | Includes |
| --- | --- |
| `minimal` | `session_status` only |
| `coding` | `group:fs`, `group:runtime`, `group:web`, `group:sessions`, `group:memory`, `cron`, `image`, `image_generate`, `video_generate` |
| `messaging` | `group:messaging`, `sessions_list`, `sessions_history`, `sessions_send`, `session_status` |
| `full` | No restriction (same as unset) |

### Tool groups

| Group | Tools |
| --- | --- |
| `group:runtime` | `exec`, `process`, `code_execution` (`bash` is accepted as an alias for `exec`) |
| `group:fs` | `read`, `write`, `edit`, `apply_patch` |
| `group:sessions` | `sessions_list`, `sessions_history`, `sessions_send`, `sessions_spawn`, `sessions_yield`, `subagents`, `session_status` |
| `group:memory` | `memory_search`, `memory_get` |
| `group:web` | `web_search`, `x_search`, `web_fetch` |
| `group:ui` | `browser`, `canvas` |
| `group:automation` | `cron`, `gateway` |
| `group:messaging` | `message` |
| `group:nodes` | `nodes` |
| `group:agents` | `agents_list` |
| `group:media` | `image`, `image_generate`, `video_generate`, `tts` |
| `group:openclaw` | All built-in tools (excludes provider plugins) |

### `tools.allow` / `tools.deny`

Global tool allow/deny policy (deny wins). Case-insensitive, supports `*` wildcards. Applied even when Docker sandbox is off.

```
{
  tools: { deny: ["browser", "canvas"] },
}
```

### `tools.byProvider`

Further restrict tools for specific providers or models. Order: base profile → provider profile → allow/deny.

```
{
  tools: {
    profile: "coding",
    byProvider: {
      "google-antigravity": { profile: "minimal" },
      "openai/gpt-5.4": { allow: ["group:fs", "sessions_list"] },
    },
  },
}
```

### `tools.elevated`

Controls elevated exec access outside the sandbox:

```
{
  tools: {
    elevated: {
      enabled: true,
      allowFrom: {
        whatsapp: ["+15555550123"],
        discord: ["1234567890123", "987654321098765432"],
      },
    },
  },
}
```

- Per-agent override (`agents.list[].tools.elevated`) can only further restrict.
- `/elevated on|off|ask|full` stores state per session; inline directives apply to single message.
- Elevated `exec` bypasses sandboxing and uses the configured escape path (`gateway` by default, or `node` when the exec target is `node`).

### `tools.exec`

```
{
  tools: {
    exec: {
      backgroundMs: 10000,
      timeoutSec: 1800,
      cleanupMs: 1800000,
      notifyOnExit: true,
      notifyOnExitEmptySuccess: false,
      applyPatch: {
        enabled: false,
        allowModels: ["gpt-5.5"],
      },
    },
  },
}
```

### `tools.loopDetection`

Tool-loop safety checks are **disabled by default**. Set `enabled: true` to activate detection. Settings can be defined globally in `tools.loopDetection` and overridden per-agent at `agents.list[].tools.loopDetection`.

```
{
  tools: {
    loopDetection: {
      enabled: true,
      historySize: 30,
      warningThreshold: 10,
      criticalThreshold: 20,
      globalCircuitBreakerThreshold: 30,
      detectors: {
        genericRepeat: true,
        knownPollNoProgress: true,
        pingPong: true,
      },
    },
  },
}
```

[​](https://docs.openclaw.ai/gateway/config-tools#param-history-size)

historySize

number

Max tool-call history retained for loop analysis.

[​](https://docs.openclaw.ai/gateway/config-tools#param-warning-threshold)

warningThreshold

number

Repeating no-progress pattern threshold for warnings.

[​](https://doc

_… [truncated; see https://docs.openclaw.ai/gateway/config-tools for full content]_


---

## Configuration - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/configuration>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Configuration

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

## Minimal config

```
// ~/.openclaw/openclaw.json
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

## Editing config

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

## Strict validation

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
is skipped when a candidate contains redacted secret placeholders

_… [truncated; see https://docs.openclaw.ai/gateway/configuration for full content]_


---

## Configuration examples - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/configuration-examples>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Configuration examples

Examples below are aligned with the current config schema. For the exhaustive reference and per-field notes, see [Configuration](https://docs.openclaw.ai/gateway/configuration).

## Quick start

### Absolute minimum

```
{
  agent: { workspace: "~/.openclaw/workspace" },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

Save to `~/.openclaw/openclaw.json` and you can DM the bot from that number.

### Recommended starter

```
{
  identity: {
    name: "Clawd",
    theme: "helpful assistant",
    emoji: "🦞",
  },
  agent: {
    workspace: "~/.openclaw/workspace",
    model: { primary: "anthropic/claude-sonnet-4-6" },
  },
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: {
    visibleReplies: "automatic",
    groupChat: {
      visibleReplies: "message_tool", // default; use "automatic" for legacy room replies
    },
  },
}
```

## Expanded example (major options)

> JSON5 lets you use comments and trailing commas. Regular JSON works too.

```
{
  // Environment + shell
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: {
      GROQ_API_KEY: "gsk-...",
    },
    shellEnv: {
      enabled: true,
      timeoutMs: 15000,
    },
  },

  // Auth profile metadata (secrets live in auth-profiles.json)
  auth: {
    profiles: {
      "anthropic:default": { provider: "anthropic", mode: "api_key" },
      "anthropic:work": { provider: "anthropic", mode: "api_key" },
      "openai:default": { provider: "openai", mode: "api_key" },
      "openai-codex:personal": { provider: "openai-codex", mode: "oauth" },
    },
    order: {
      anthropic: ["anthropic:default", "anthropic:work"],
      openai: ["openai:default"],
      "openai-codex": ["openai-codex:personal"],
    },
  },

  // Identity
  identity: {
    name: "Samantha",
    theme: "helpful sloth",
    emoji: "🦥",
  },

  // Logging
  logging: {
    level: "info",
    file: "/tmp/openclaw/openclaw.log",
    consoleLevel: "info",
    consoleStyle: "pretty",
    redactSensitive: "tools",
  },

  // Message formatting
  messages: {
    messagePrefix: "[openclaw]",
    visibleReplies: "automatic",
    responsePrefix: ">",
    ackReaction: "👀",
    ackReactionScope: "group-mentions",
    groupChat: {
      historyLimit: 50,
      visibleReplies: "message_tool", // normal final replies stay private in groups/channels
    },
    queue: {
      mode: "steer",
      debounceMs: 500,
      cap: 20,
      drop: "summarize",
      byChannel: {
        whatsapp: "steer",
        telegram: "steer",
        discord: "steer",
        slack: "steer",
        signal: "steer",
        imessage: "steer",
        webchat: "steer",
      },
    },
  },

  // Tooling
  tools: {
    media: {
      audio: {
        enabled: true,
        maxBytes: 20971520,
        models: [\
          { provider: "openai", model: "gpt-4o-mini-transcribe" },\
          // Optional CLI fallback (Whisper binary):\
          // { type: "cli", command: "whisper", args: ["--model", "base", "{{MediaPath}}"] }\
        ],
        timeoutSeconds: 120,
      },
      video: {
        enabled: true,
        maxBytes: 52428800,
        models: [{ provider: "google", model: "gemini-3-flash-preview" }],
      },
    },
  },

  // Session behavior
  session: {
    scope: "per-sender",
    dmScope: "per-channel-peer", // recommended for multi-user inboxes
    reset: {
      mode: "daily",
      atHour: 4,
      idleMinutes: 60,
    },
    resetByChannel: {
      discord: { mode: "idle", idleMinutes: 10080 },
    },
    resetTriggers: ["/new", "/reset"],
    store: "~/.openclaw/agents/default/sessions/sessions.json",
    maintenance: {
      mode: "warn",
      pruneAfter: "30d",
      maxEntries: 500,
      resetArchiveRetention: "30d", // duration or false
      maxDiskBytes: "500mb", // optional
      highWaterBytes: "400mb", // optional (default

_… [truncated; see https://docs.openclaw.ai/gateway/configuration-examples for full content]_


---

## Configuration reference - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/configuration-reference>_

[OpenClaw home page](https://docs.openclaw.ai/)

Configuration

Configuration reference

Core config reference for `~/.openclaw/openclaw.json`. For a task-oriented overview, see [Configuration](https://docs.openclaw.ai/gateway/configuration).Covers the main OpenClaw config surfaces and links out when a subsystem has its own deeper reference. Channel- and plugin-owned command catalogs and deep memory/QMD knobs live on their own pages rather than on this one.Code truth:

- `openclaw config schema` prints the live JSON Schema used for validation and Control UI, with bundled/plugin/channel metadata merged in when available
- `config.schema.lookup` returns one path-scoped schema node for drill-down tooling
- `pnpm config:docs:check` / `pnpm config:docs:gen` validate the config-doc baseline hash against the current schema surface

Agent lookup path: use the `gateway` tool action `config.schema.lookup` for
exact field-level docs and constraints before edits. Use
[Configuration](https://docs.openclaw.ai/gateway/configuration) for task-oriented guidance and this page
for the broader field map, defaults, and links to subsystem references.Dedicated deep references:

- [Memory configuration reference](https://docs.openclaw.ai/reference/memory-config) for `agents.defaults.memorySearch.*`, `memory.qmd.*`, `memory.citations`, and dreaming config under `plugins.entries.memory-core.config.dreaming`
- [Slash commands](https://docs.openclaw.ai/tools/slash-commands) for the current built-in + bundled command catalog
- owning channel/plugin pages for channel-specific command surfaces

Config format is **JSON5** (comments + trailing commas allowed). All fields are optional — OpenClaw uses safe defaults when omitted.

* * *

## Channels

Per-channel config keys moved to a dedicated page — see
[Configuration — channels](https://docs.openclaw.ai/gateway/config-channels) for `channels.*`,
including Slack, Discord, Telegram, WhatsApp, Matrix, iMessage, and other
bundled channels (auth, access control, multi-account, mention gating).

## Agent defaults, multi-agent, sessions, and messages

Moved to a dedicated page — see
[Configuration — agents](https://docs.openclaw.ai/gateway/config-agents) for:

- `agents.defaults.*` (workspace, model, thinking, heartbeat, memory, media, skills, sandbox)
- `multiAgent.*` (multi-agent routing and bindings)
- `session.*` (session lifecycle, compaction, pruning)
- `messages.*` (message delivery, TTS, markdown rendering)
- `talk.*`(Talk mode)

  - `talk.speechLocale`: optional BCP 47 locale id for Talk speech recognition on iOS/macOS
  - `talk.silenceTimeoutMs`: when unset, Talk keeps the platform default pause window before sending the transcript (`700 ms on macOS and Android, 900 ms on iOS`)

## Tools and custom providers

Tool policy, experimental toggles, provider-backed tool config, and custom
provider / base-URL setup moved to a dedicated page — see
[Configuration — tools and custom providers](https://docs.openclaw.ai/gateway/config-tools).

## Models

Provider definitions, model allowlists, and custom provider setup live in
[Configuration — tools and custom providers](https://docs.openclaw.ai/gateway/config-tools#custom-providers-and-base-urls).
The `models` root also owns global model-catalog behavior.

```
{
  models: {
    // Optional. Default: true. Requires a Gateway restart when changed.
    pricing: { enabled: false },
  },
}
```

- `models.mode`: provider catalog behavior (`merge` or `replace`).
- `models.providers`: custom provider map keyed by provider id.
- `models.pricing.enabled`: controls the background pricing bootstrap that
starts after sidecars and channels reach the Gateway ready path. When `false`,
the Gateway skips OpenRouter and LiteLLM pricing-catalog fetches; configured
`models.providers.*.models[].cost` values still work for local cost estimates.

## MCP

OpenClaw-managed MCP server definitions live under `mcp.servers` and are
consumed by embedded Pi and other runtime adapters. The `openclaw

_… [truncated; see https://docs.openclaw.ai/gateway/configuration-reference for full content]_


---

## https://docs.openclaw.ai/gateway/configuration-reference.md

_Source: <https://docs.openclaw.ai/gateway/configuration-reference.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Configuration reference

Core config reference for \`~/.openclaw/openclaw.json\`. For a task-oriented overview, see \[Configuration\](/gateway/configuration).

Covers the main OpenClaw config surfaces and links out when a subsystem has its own deeper reference. Channel- and plugin-owned command catalogs and deep memory/QMD knobs live on their own pages rather than on this one.

Code truth:

\\* \`openclaw config schema\` prints the live JSON Schema used for validation and Control UI, with bundled/plugin/channel metadata merged in when available
\\* \`config.schema.lookup\` returns one path-scoped schema node for drill-down tooling
\\* \`pnpm config:docs:check\` / \`pnpm config:docs:gen\` validate the config-doc baseline hash against the current schema surface

Agent lookup path: use the \`gateway\` tool action \`config.schema.lookup\` for
exact field-level docs and constraints before edits. Use
\[Configuration\](/gateway/configuration) for task-oriented guidance and this page
for the broader field map, defaults, and links to subsystem references.

Dedicated deep references:

\\* \[Memory configuration reference\](/reference/memory-config) for \`agents.defaults.memorySearch.\*\`, \`memory.qmd.\*\`, \`memory.citations\`, and dreaming config under \`plugins.entries.memory-core.config.dreaming\`
\\* \[Slash commands\](/tools/slash-commands) for the current built-in + bundled command catalog
\\* owning channel/plugin pages for channel-specific command surfaces

Config format is \*\*JSON5\*\* (comments + trailing commas allowed). All fields are optional — OpenClaw uses safe defaults when omitted.

\\*\\*\\*

\## Channels

Per-channel config keys moved to a dedicated page — see
\[Configuration — channels\](/gateway/config-channels) for \`channels.\*\`,
including Slack, Discord, Telegram, WhatsApp, Matrix, iMessage, and other
bundled channels (auth, access control, multi-account, mention gating).

\## Agent defaults, multi-agent, sessions, and messages

Moved to a dedicated page — see
\[Configuration — agents\](/gateway/config-agents) for:

\\* \`agents.defaults.\*\` (workspace, model, thinking, heartbeat, memory, media, skills, sandbox)
\\* \`multiAgent.\*\` (multi-agent routing and bindings)
\\* \`session.\*\` (session lifecycle, compaction, pruning)
\\* \`messages.\*\` (message delivery, TTS, markdown rendering)
\\* \`talk.\*\` (Talk mode)
 \\* \`talk.speechLocale\`: optional BCP 47 locale id for Talk speech recognition on iOS/macOS
 \\* \`talk.silenceTimeoutMs\`: when unset, Talk keeps the platform default pause window before sending the transcript (\`700 ms on macOS and Android, 900 ms on iOS\`)

\## Tools and custom providers

Tool policy, experimental toggles, provider-backed tool config, and custom
provider / base-URL setup moved to a dedicated page — see
\[Configuration — tools and custom providers\](/gateway/config-tools).

\## Models

Provider definitions, model allowlists, and custom provider setup live in
\[Configuration — tools and custom providers\](/gateway/config-tools#custom-providers-and-base-urls).
The \`models\` root also owns global model-catalog behavior.

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 models: {
 // Optional. Default: true. Requires a Gateway restart when changed.
 pricing: { enabled: false },
 },
}
\`\`\`

\\* \`models.mode\`: provider catalog behavior (\`merge\` or \`replace\`).
\\* \`models.providers\`: custom provider map keyed by provider id.
\\* \`models.pricing.enabled\`: controls the background pricing bootstrap that
 starts after sidecars and channels reach the Gateway ready path. When \`false\`,
 the Gateway skips OpenRouter and LiteLLM pricing-catalog fetches; configured
 \`models.providers.\*.models\[\].cost\` values still work for local cost estimates.

\## MCP

OpenClaw-managed MCP server definitions

_… [truncated; see https://docs.openclaw.ai/gateway/configuration-reference.md for full content]_


---

## https://docs.openclaw.ai/gateway/configuration.md

_Source: <https://docs.openclaw.ai/gateway/configuration.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Configuration

OpenClaw reads an optional \*\*JSON5\*\* config from \`~/.openclaw/openclaw.json\`.
The active config path must be a regular file. Symlinked \`openclaw.json\`
layouts are unsupported for OpenClaw-owned writes; an atomic write may replace
the path instead of preserving the symlink. If you keep config outside the
default state directory, point \`OPENCLAW\_CONFIG\_PATH\` directly at the real file.

If the file is missing, OpenClaw uses safe defaults. Common reasons to add a config:

\\* Connect channels and control who can message the bot
\\* Set models, tools, sandboxing, or automation (cron, hooks)
\\* Tune sessions, media, networking, or UI

See the \[full reference\](/gateway/configuration-reference) for every available field.

Agents and automation should use \`config.schema.lookup\` for exact field-level
docs before editing config. Use this page for task-oriented guidance and
\[Configuration reference\](/gateway/configuration-reference) for the broader
field map and defaults.

 \*\*New to configuration?\*\* Start with \`openclaw onboard\` for interactive setup, or check out the \[Configuration Examples\](/gateway/configuration-examples) guide for complete copy-paste configs.

\## Minimal config

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
// ~/.openclaw/openclaw.json
{
 agents: { defaults: { workspace: "~/.openclaw/workspace" } },
 channels: { whatsapp: { allowFrom: \["+15555550123"\] } },
}
\`\`\`

\## Editing config

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard # full onboarding flow
 openclaw configure # config wizard
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw config get agents.defaults.workspace
 openclaw config set agents.defaults.heartbeat.every "2h"
 openclaw config unset plugins.entries.brave.config.webSearch.apiKey
 \`\`\`

 Open \[http://127.0.0.1:18789\](http://127.0.0.1:18789) and use the \*\*Config\*\* tab.
 The Control UI renders a form from the live config schema, including field
 \`title\` / \`description\` docs metadata plus plugin and channel schemas when
 available, with a \*\*Raw JSON\*\* editor as an escape hatch. For drill-down
 UIs and other tooling, the gateway also exposes \`config.schema.lookup\` to
 fetch one path-scoped schema node plus immediate child summaries.

 Edit \`~/.openclaw/openclaw.json\` directly. The Gateway watches the file and applies changes automatically (see \[hot reload\](#config-hot-reload)).

\## Strict validation

 OpenClaw only accepts configurations that fully match the schema. Unknown keys, malformed types, or invalid values cause the Gateway to \*\*refuse to start\*\*. The only root-level exception is \`$schema\` (string), so editors can attach JSON Schema metadata.

\`openclaw config schema\` prints the canonical JSON Schema used by Control UI
and validation. \`config.schema.lookup\` fetches a single path-scoped node plus
child summaries for drill-down tooling. Field \`title\`/\`description\` docs metadata
carries through nested objects, wildcard (\`\*\`), array-item (\`\[\]\`), and \`anyOf\`/
\`oneOf\`/\`allOf\` branches. Runtime plugin and channel schemas merge in when the
manifest registry is loaded.

When validation fails:

\\* The Gateway does not boot
\\* Only diagnostic commands work (\`openclaw doctor\`, \`openclaw logs\`, \`openclaw health\`, \`openclaw status\`)
\\* Run \`openclaw doctor\` to see exact issues
\\* Run \`openclaw doctor --fix\` (or \`--yes\`) to apply repairs

The Gateway keeps a trusted last-known-good copy after each successful startup.
If \`openclaw.json\` later fails validation (or drops \`gateway.mode\`, shrinks
sharply, or has a stray log line prepended), OpenClaw preserves the broken file
as \`.clobbered.\*\`, restores the last-known-good copy, and logs the r

_… [truncated; see https://docs.openclaw.ai/gateway/configuration.md for full content]_


---

## Diagnostics export - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/diagnostics>_

[OpenClaw home page](https://docs.openclaw.ai/)

Health and diagnostics

Diagnostics export

OpenClaw can create a local diagnostics zip for bug reports. It combines
sanitized Gateway status, health, logs, config shape, and recent payload-free
stability events.Treat diagnostics bundles like secrets until you have reviewed them. They are
designed to omit or redact payloads and credentials, but they still summarize
local Gateway logs and host-level runtime state.

## Quick start

```
openclaw gateway diagnostics export
```

The command prints the written zip path. To choose a path:

```
openclaw gateway diagnostics export --output openclaw-diagnostics.zip
```

For automation:

```
openclaw gateway diagnostics export --json
```

## Chat command

Owners can use `/diagnostics [note]` in chat to request a local Gateway export.
Use this when the bug happened in a real conversation and you want one
copy-pasteable report for support:

1. Send `/diagnostics` in the conversation where you noticed the problem. Add a
short note if it helps, for example `/diagnostics bad tool choice`.
2. OpenClaw sends the diagnostics preamble and asks for one explicit exec
approval. The approval runs `openclaw gateway diagnostics export --json`.
Do not approve diagnostics through an allow-all rule.
3. After approval, OpenClaw replies with a pasteable report containing the local
bundle path, manifest summary, privacy notes, and relevant session ids.

In group chats, an owner can still run `/diagnostics`, but OpenClaw does not
post the diagnostic details back into the shared chat. It sends the preamble,
approval prompts, Gateway export result, and Codex session/thread breakdown to
the owner through the private approval route. The group only gets a short notice
that the diagnostics flow was sent privately. If OpenClaw cannot find a private
owner route, the command fails closed and asks the owner to run it from a DM.When the active OpenClaw session is using the native OpenAI Codex harness,
the same exec approval also covers an OpenAI feedback upload for the Codex
runtime threads OpenClaw knows about. That upload is separate from the local
Gateway zip and appears only for Codex harness sessions. Before approval, the
prompt explains that approving diagnostics will also send Codex feedback, but it
does not list Codex session or thread ids. After approval, the chat reply lists
the channels, OpenClaw session ids, Codex thread ids, and local resume commands
for the threads that were sent to OpenAI servers. If you deny or ignore the
approval, OpenClaw does not run the export, does not send Codex feedback, and
does not print the Codex ids.That makes the common Codex debugging loop short: notice the bad behavior in
Telegram, Discord, or another channel, run `/diagnostics`, approve once, share
the report with support, then run the printed `codex resume <thread-id>` command
locally if you want to inspect the native Codex thread yourself. See
[Codex harness](https://docs.openclaw.ai/plugins/codex-harness#inspect-a-codex-thread-from-the-cli) for
that inspection workflow.

## What the export contains

The zip includes:

- `summary.md`: human-readable overview for support.
- `diagnostics.json`: machine-readable summary of config, logs, status, health,
and stability data.
- `manifest.json`: export metadata and file list.
- Sanitized config shape and non-secret config details.
- Sanitized log summaries and recent redacted log lines.
- Best-effort Gateway status and health snapshots.
- `stability/latest.json`: newest persisted stability bundle, when available.

The export is useful even when the Gateway is unhealthy. If the Gateway cannot
answer status or health requests, the local logs, config shape, and latest
stability bundle are still collected when available.

## Privacy model

Diagnostics are designed to be shareable. The export keeps operational data
that helps debugging, such as:

- subsystem names, plugin ids, provider ids, channel ids, and configured modes
- status co

_… [truncated; see https://docs.openclaw.ai/gateway/diagnostics for full content]_


---

## Doctor - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/doctor>_

[OpenClaw home page](https://docs.openclaw.ai/)

Health and diagnostics

Doctor

`openclaw doctor` is the repair + migration tool for OpenClaw. It fixes stale config/state, checks health, and provides actionable repair steps.

## Quick start

```
openclaw doctor
```

### Headless and automation modes

- --yes

- --repair

- --repair --force

- --non-interactive

- --deep

```
openclaw doctor --yes
```

Accept defaults without prompting (including restart/service/sandbox repair steps when applicable).

```
openclaw doctor --repair
```

Apply recommended repairs without prompting (repairs + restarts where safe).

```
openclaw doctor --repair --force
```

Apply aggressive repairs too (overwrites custom supervisor configs).

```
openclaw doctor --non-interactive
```

Run without prompts and only apply safe migrations (config normalization + on-disk state moves). Skips restart/service/sandbox actions that require human confirmation. Legacy state migrations run automatically when detected.

```
openclaw doctor --deep
```

Scan system services for extra gateway installs (launchd/systemd/schtasks).

If you want to review changes before writing, open the config file first:

```
cat ~/.openclaw/openclaw.json
```

## What it does (summary)

Health, UI, and updates

- Optional pre-flight update for git installs (interactive only).
- UI protocol freshness check (rebuilds Control UI when the protocol schema is newer).
- Health check + restart prompt.
- Skills status summary (eligible/missing/blocked) and plugin status.

Config and migrations

- Config normalization for legacy values.
- Talk config migration from legacy flat `talk.*` fields into `talk.provider` \+ `talk.providers.<provider>`.
- Browser migration checks for legacy Chrome extension configs and Chrome MCP readiness.
- OpenCode provider override warnings (`models.providers.opencode` / `models.providers.opencode-go`).
- Codex OAuth shadowing warnings (`models.providers.openai-codex`).
- OAuth TLS prerequisites check for OpenAI Codex OAuth profiles.
- Plugin/tool allowlist warnings when `plugins.allow` is restrictive but tool policy still asks for wildcard or plugin-owned tools.
- Legacy on-disk state migration (sessions/agent dir/WhatsApp auth).
- Legacy plugin manifest contract key migration (`speechProviders`, `realtimeTranscriptionProviders`, `realtimeVoiceProviders`, `mediaUnderstandingProviders`, `imageGenerationProviders`, `videoGenerationProviders`, `webFetchProviders`, `webSearchProviders` → `contracts`).
- Legacy cron store migration (`jobId`, `schedule.cron`, top-level delivery/payload fields, payload `provider`, simple `notify: true` webhook fallback jobs).
- Legacy agent runtime-policy migration to `agents.defaults.agentRuntime` and `agents.list[].agentRuntime`.
- Stale plugin config cleanup when plugins are enabled; when `plugins.enabled=false`, stale plugin references are treated as inert containment config and are preserved.

State and integrity

- Session lock file inspection and stale lock cleanup.
- Session transcript repair for duplicated prompt-rewrite branches created by affected 2026.4.24 builds.
- Wedged subagent restart-recovery tombstone detection, with `--fix` support for clearing stale aborted recovery flags so startup does not keep treating the child as restart-aborted.
- State integrity and permissions checks (sessions, transcripts, state dir).
- Config file permission checks (chmod 600) when running locally.
- Model auth health: checks OAuth expiry, can refresh expiring tokens, and reports auth-profile cooldown/disabled states.
- Extra workspace dir detection (`~/openclaw`).

Gateway, services, and supervisors

- Sandbox image repair when sandboxing is enabled.
- Legacy service migration and extra gateway detection.
- Matrix channel legacy state migration (in `--fix` / `--repair` mode).
- Gateway runtime checks (service installed but not running; cached launchd label).
- Channel status warnings (probed from the running gateway).
- Supervisor config a

_… [truncated; see https://docs.openclaw.ai/gateway/doctor for full content]_


---

## Health checks - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/health>_

[OpenClaw home page](https://docs.openclaw.ai/)

Health and diagnostics

Health checks

Short guide to verify channel connectivity without guessing.

## Quick checks

- `openclaw status` — local summary: gateway reachability/mode, update hint, linked channel auth age, sessions + recent activity.
- `openclaw status --all` — full local diagnosis (read-only, color, safe to paste for debugging).
- `openclaw status --deep` — asks the running gateway for a live health probe (`health` with `probe:true`), including per-account channel probes when supported.
- `openclaw health` — asks the running gateway for its health snapshot (WS-only; no direct channel sockets from the CLI).
- `openclaw health --verbose` — forces a live health probe and prints gateway connection details.
- `openclaw health --json` — machine-readable health snapshot output.
- Send `/status` as a standalone message in WhatsApp/WebChat to get a status reply without invoking the agent.
- Logs: tail `/tmp/openclaw/openclaw-*.log` and filter for `web-heartbeat`, `web-reconnect`, `web-auto-reply`, `web-inbound`.

For Discord and other chat providers, session rows are not socket liveness.
`openclaw sessions`, Gateway `sessions.list`, and the agent `sessions_list` tool
read stored conversation state. A provider can reconnect and show healthy channel
status before any new session row is materialized. Use the channel status and
health commands above for live connectivity checks.

## Deep diagnostics

- Creds on disk: `ls -l ~/.openclaw/credentials/whatsapp/<accountId>/creds.json` (mtime should be recent).
- Session store: `ls -l ~/.openclaw/agents/<agentId>/sessions/sessions.json` (path can be overridden in config). Count and recent recipients are surfaced via `status`.
- Relink flow: `openclaw channels logout && openclaw channels login --verbose` when status codes 409–515 or `loggedOut` appear in logs. (Note: the QR login flow auto-restarts once for status 515 after pairing.)
- Diagnostics are enabled by default. The gateway records operational facts unless `diagnostics.enabled: false` is set. Memory events record RSS/heap byte counts, threshold pressure, and growth pressure. Liveness warnings record event-loop delay, event-loop utilization, CPU-core ratio, and active/waiting/queued session counts when the process is running but saturated. Oversized-payload events record what was rejected, truncated, or chunked, plus sizes and limits when available. They do not record the message text, attachment contents, webhook body, raw request or response body, tokens, cookies, or secret values. The same heartbeat starts the bounded stability recorder, which is available through `openclaw gateway stability` or the `diagnostics.stability` Gateway RPC. Fatal Gateway exits, shutdown timeouts, and restart startup failures persist the latest recorder snapshot under `~/.openclaw/logs/stability/` when events exist; inspect the newest saved bundle with `openclaw gateway stability --bundle latest`.
- For bug reports, run `openclaw gateway diagnostics export` and attach the generated zip. The export combines a Markdown summary, the newest stability bundle, sanitized log metadata, sanitized Gateway status/health snapshots, and config shape. It is meant to be shared: chat text, webhook bodies, tool outputs, credentials, cookies, account/message identifiers, and secret values are omitted or redacted. See [Diagnostics Export](https://docs.openclaw.ai/gateway/diagnostics).

## Health monitor config

- `gateway.channelHealthCheckMinutes`: how often the gateway checks channel health. Default: `5`. Set `0` to disable health-monitor restarts globally.
- `gateway.channelStaleEventThresholdMinutes`: how long a connected channel can stay idle before the health monitor treats it as stale and restarts it. Default: `30`. Keep this greater than or equal to `gateway.channelHealthCheckMinutes`.
- `gateway.channelMaxRestartsPerHour`: rolling one-hour cap for health-monitor restarts per channel/account. Default: `10`

_… [truncated; see https://docs.openclaw.ai/gateway/health for full content]_


---

## Heartbeat - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/heartbeat>_

# Heartbeat checklist

- Quick scan: anything urgent in inboxes?
- If it's daytime, do a lightweight check-in if nothing else is pending.
- If a task is blocked, write down _what is missing_ and ask Peter next time.
```

### `tasks:` blocks

`HEARTBEAT.md` also supports a small structured `tasks:` block for interval-based checks inside heartbeat itself.Example:

```
tasks:

- name: inbox-triage
  interval: 30m
  prompt: "Check for urgent unread emails and flag anything time sensitive."
- name: calendar-scan
  interval: 2h
  prompt: "Check for upcoming meetings that need prep or follow-up."

# Additional instructions

- Keep alerts short.
- If nothing needs attention after all due tasks, reply HEARTBEAT_OK.
```

Behavior

- OpenClaw parses the `tasks:` block and checks each task against its own `interval`.
- Only **due** tasks are included in the heartbeat prompt for that tick.
- If no tasks are due, the heartbeat is skipped entirely (`reason=no-tasks-due`) to avoid a wasted model call.
- Non-task content in `HEARTBEAT.md` is preserved and appended as additional context after the due-task list.
- Task last-run timestamps are stored in session state (`heartbeatTaskState`), so intervals survive normal restarts.
- Task timestamps are only advanced after a heartbeat run completes its normal reply path. Skipped `empty-heartbeat-file` / `no-tasks-due` runs do not mark tasks as completed.

Task mode is useful when you want one heartbeat file to hold several periodic checks without paying for all of them every tick.

### Can the agent update HEARTBEAT.md?

Yes — if you ask it to.`HEARTBEAT.md` is just a normal file in the agent workspace, so you can tell the agent (in a normal chat) something like:

- “Update `HEARTBEAT.md` to add a daily calendar check.”
- “Rewrite `HEARTBEAT.md` so it’s shorter and focused on inbox follow-ups.”

If you want this to happen proactively, you can also include an explicit line in your heartbeat prompt like: “If the checklist becomes stale, update HEARTBEAT.md with a better one.”

Don’t put secrets (API keys, phone numbers, private tokens) into `HEARTBEAT.md` — it becomes part of the prompt context.

## Manual wake (on-demand)

You can enqueue a system event and trigger an immediate heartbeat with:

```
openclaw system event --text "Check for urgent follow-ups" --mode now
```

If multiple agents have `heartbeat` configured, a manual wake runs each of those agent heartbeats immediately.Use `--mode next-heartbeat` to wait for the next scheduled tick.

## Reasoning delivery (optional)

By default, heartbeats deliver only the final “answer” payload.If you want transparency, enable:

- `agents.defaults.heartbeat.includeReasoning: true`

When enabled, heartbeats will also deliver a separate message prefixed `Reasoning:` (same shape as `/reasoning on`). This can be useful when the agent is managing multiple sessions/codexes and you want to see why it decided to ping you — but it can also leak more internal detail than you want. Prefer keeping it off in group chats.

## Cost awareness

Heartbeats run full agent turns. Shorter intervals burn more tokens. To reduce cost:

- Use `isolatedSession: true` to avoid sending full conversation history (~100K tokens down to ~2-5K per run).
- Use `lightContext: true` to limit bootstrap files to just `HEARTBEAT.md`.
- Set a cheaper `model` (e.g. `ollama/llama3.2:1b`).
- Keep `HEARTBEAT.md` small.
- Use `target: "none"` if you only want internal state updates.

## Context overflow after heartbeat

If a heartbeat uses a smaller local model, for example an Ollama model with a 32k window, and the next main-session turn reports context overflow, check whether the previous heartbeat left the session on the heartbeat model. OpenClaw’s reset message calls this out when the last runtime model matches configured `heartbeat.model`.Use `isolatedSession: true` to run heartbeats in a fresh session, combine it with `lightContext: true` for the smallest prompt, or choose a heartbeat model wit

_… [truncated; see https://docs.openclaw.ai/gateway/heartbeat for full content]_


---

## https://docs.openclaw.ai/gateway/heartbeat.md

_Source: <https://docs.openclaw.ai/gateway/heartbeat.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Heartbeat

 \*\*Heartbeat vs cron?\*\* See \[Automation & Tasks\](/automation) for guidance on when to use each.

Heartbeat runs \*\*periodic agent turns\*\* in the main session so the model can surface anything that needs attention without spamming you.

Heartbeat is a scheduled main-session turn — it does \*\*not\*\* create \[background task\](/automation/tasks) records. Task records are for detached work (ACP runs, subagents, isolated cron jobs).

Troubleshooting: \[Scheduled Tasks\](/automation/cron-jobs#troubleshooting)

\## Quick start (beginner)

 Leave heartbeats enabled (default is \`30m\`, or \`1h\` for Anthropic OAuth/token auth, including Claude CLI reuse) or set your own cadence.

 Create a tiny \`HEARTBEAT.md\` checklist or \`tasks:\` block in the agent workspace.

 \`target: "none"\` is the default; set \`target: "last"\` to route to the last contact.

 \\* Enable heartbeat reasoning delivery for transparency.
 \\* Use lightweight bootstrap context if heartbeat runs only need \`HEARTBEAT.md\`.
 \\* Enable isolated sessions to avoid sending full conversation history each heartbeat.
 \\* Restrict heartbeats to active hours (local time).

Example config:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 heartbeat: {
 every: "30m",
 target: "last", // explicit delivery to last contact (default is "none")
 directPolicy: "allow", // default: allow direct/DM targets; set "block" to suppress
 lightContext: true, // optional: only inject HEARTBEAT.md from bootstrap files
 isolatedSession: true, // optional: fresh session each run (no conversation history)
 skipWhenBusy: true, // optional: also defer when subagent or nested lanes are busy
 // activeHours: { start: "08:00", end: "24:00" },
 // includeReasoning: true, // optional: send separate \`Reasoning:\` message too
 },
 },
 },
}
\`\`\`

\## Defaults

\\* Interval: \`30m\` (or \`1h\` when Anthropic OAuth/token auth is the detected auth mode, including Claude CLI reuse). Set \`agents.defaults.heartbeat.every\` or per-agent \`agents.list\[\].heartbeat.every\`; use \`0m\` to disable.
\\* Prompt body (configurable via \`agents.defaults.heartbeat.prompt\`): \`Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT\_OK.\`
\\* The heartbeat prompt is sent \*\*verbatim\*\* as the user message. The system prompt includes a "Heartbeat" section only when heartbeats are enabled for the default agent, and the run is flagged internally.
\\* When heartbeats are disabled with \`0m\`, normal runs also omit \`HEARTBEAT.md\` from bootstrap context so the model does not see heartbeat-only instructions.
\\* Active hours (\`heartbeat.activeHours\`) are checked in the configured timezone. Outside the window, heartbeats are skipped until the next tick inside the window.
\\* Heartbeats automatically defer while cron work is active or queued. Set \`heartbeat.skipWhenBusy: true\` to defer on extra busy lanes (subagent or nested command work) as well; this is useful for local Ollama and other constrained single-runtime hosts.

\## What the heartbeat prompt is for

The default prompt is intentionally broad:

\\* \*\*Background tasks\*\*: "Consider outstanding tasks" nudges the agent to review follow-ups (inbox, calendar, reminders, queued work) and surface anything urgent.
\\* \*\*Human check-in\*\*: "Checkup sometimes on your human during day time" nudges an occasional lightweight "anything you need?" message, but avoids night-time spam by using your configured local timezone (see \[Timezone\](/concepts/timezone)).

Heartbeat can react to completed \[background tasks\](/automation/tasks), but a heartbeat run itself does not create a task record.

If you want a heartbeat to do somethin

_… [truncated; see https://docs.openclaw.ai/gateway/heartbeat.md for full content]_


---

## Gateway logging - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/logging>_

# Logging

For a user-facing overview (CLI + Control UI + config), see [/logging](https://docs.openclaw.ai/logging).OpenClaw has two log “surfaces”:

- **Console output** (what you see in the terminal / Debug UI).
- **File logs** (JSON lines) written by the gateway logger.

## File-based logger

- Default rolling log file is under `/tmp/openclaw/` (one file per day): `openclaw-YYYY-MM-DD.log`
  - Date uses the gateway host’s local timezone.
- Active log files rotate at `logging.maxFileBytes` (default: 100 MB), keeping
up to five numbered archives and continuing to write a fresh active file.
- The log file path and level can be configured via `~/.openclaw/openclaw.json`:

  - `logging.file`
  - `logging.level`

The file format is one JSON object per line.The Control UI Logs tab tails this file via the gateway (`logs.tail`).
CLI can do the same:

```
openclaw logs --follow
```

**Verbose vs. log levels**

- **File logs** are controlled exclusively by `logging.level`.
- `--verbose` only affects **console verbosity** (and WS log style); it does **not**
raise the file log level.
- To capture verbose-only details in file logs, set `logging.level` to `debug` or
`trace`.
- Trace logging also includes diagnostic timing summaries for selected hot paths,
such as plugin tool factory preparation. See
[/tools/plugin#slow-plugin-tool-setup](https://docs.openclaw.ai/tools/plugin#slow-plugin-tool-setup).

## Console capture

The CLI captures `console.log/info/warn/error/debug/trace` and writes them to file logs,
while still printing to stdout/stderr.You can tune console verbosity independently via:

- `logging.consoleLevel` (default `info`)
- `logging.consoleStyle` (`pretty` \| `compact` \| `json`)

## Redaction

OpenClaw can mask sensitive tokens before log or transcript output leaves the
process. This logging redaction policy is applied at console, file-log, OTLP
log-record, and session transcript text sinks, so matching secret values are
masked before JSONL lines or messages are written to disk.

- `logging.redactSensitive`: `off` \| `tools` (default: `tools`)
- `logging.redactPatterns`: array of regex strings (overrides defaults)

  - Use raw regex strings (auto `gi`), or `/pattern/flags` if you need custom flags.
  - Matches are masked by keeping the first 6 + last 4 chars (length >= 18), otherwise `***`.
  - Defaults cover common key assignments, CLI flags, JSON fields, bearer headers, PEM blocks, popular token prefixes, and payment credential field names such as card number, CVC/CVV, shared payment token, and payment credential.

Some safety boundaries always redact regardless of `logging.redactSensitive`.
That includes Control UI tool-call events, `sessions_history` tool output,
diagnostics support exports, provider error observations, exec approval command
display, and Gateway WebSocket protocol logs. These surfaces may still use
`logging.redactPatterns` as additional patterns, but `redactSensitive: "off"`
does not make them emit raw secrets.

## Gateway WebSocket logs

The gateway prints WebSocket protocol logs in two modes:

- **Normal mode (no `--verbose`)**: only “interesting” RPC results are printed:

  - errors (`ok=false`)
  - slow calls (default threshold: `>= 50ms`)
  - parse errors
- **Verbose mode (`--verbose`)**: prints all WS request/response traffic.

### WS log style

`openclaw gateway` supports a per-gateway style switch:

- `--ws-log auto` (default): normal mode is optimized; verbose mode uses compact output
- `--ws-log compact`: compact output (paired request/response) when verbose
- `--ws-log full`: full per-frame output when verbose
- `--compact`: alias for `--ws-log compact`

Examples:

```
# optimized (only errors/slow)
openclaw gateway

# show all WS traffic (paired)
openclaw gateway --verbose --ws-log compact

# show all WS traffic (full meta)
openclaw gateway --verbose --ws-log full
```

## Console formatting (subsystem logging)

The console formatter is **TTY-aware** and prints consistent, prefixed lines.
Sub

_… [truncated; see https://docs.openclaw.ai/gateway/logging for full content]_


---

## OpenAI chat completions - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/openai-http-api>_

[OpenClaw home page](https://docs.openclaw.ai/)

Protocols and APIs

OpenAI chat completions

OpenClaw’s Gateway can serve a small OpenAI-compatible Chat Completions endpoint.This endpoint is **disabled by default**. Enable it in config first.

- `POST /v1/chat/completions`
- Same port as the Gateway (WS + HTTP multiplex): `http://<gateway-host>:<port>/v1/chat/completions`

When the Gateway’s OpenAI-compatible HTTP surface is enabled, it also serves:

- `GET /v1/models`
- `GET /v1/models/{id}`
- `POST /v1/embeddings`
- `POST /v1/responses`

Under the hood, requests are executed as a normal Gateway agent run (same codepath as `openclaw agent`), so routing/permissions/config match your Gateway.

## Authentication

Uses the Gateway auth configuration.Common HTTP auth paths:

- shared-secret auth (`gateway.auth.mode="token"` or `"password"`):
`Authorization: Bearer <token-or-password>`
- trusted identity-bearing HTTP auth (`gateway.auth.mode="trusted-proxy"`):
route through the configured identity-aware proxy and let it inject the
required identity headers
- private-ingress open auth (`gateway.auth.mode="none"`):
no auth header required

Notes:

- When `gateway.auth.mode="token"`, use `gateway.auth.token` (or `OPENCLAW_GATEWAY_TOKEN`).
- When `gateway.auth.mode="password"`, use `gateway.auth.password` (or `OPENCLAW_GATEWAY_PASSWORD`).
- When `gateway.auth.mode="trusted-proxy"`, the HTTP request must come from a
configured trusted proxy source; same-host loopback proxies require explicit
`gateway.auth.trustedProxy.allowLoopback = true`.
- If `gateway.auth.rateLimit` is configured and too many auth failures occur, the endpoint returns `429` with `Retry-After`.

## Security boundary (important)

Treat this endpoint as a **full operator-access** surface for the gateway instance.

- HTTP bearer auth here is not a narrow per-user scope model.
- A valid Gateway token/password for this endpoint should be treated like an owner/operator credential.
- Requests run through the same control-plane agent path as trusted operator actions.
- There is no separate non-owner/per-user tool boundary on this endpoint; once a caller passes Gateway auth here, OpenClaw treats that caller as a trusted operator for this gateway.
- For shared-secret auth modes (`token` and `password`), the endpoint restores the normal full operator defaults even if the caller sends a narrower `x-openclaw-scopes` header.
- Trusted identity-bearing HTTP modes (for example trusted proxy auth or `gateway.auth.mode="none"`) honor `x-openclaw-scopes` when present and otherwise fall back to the normal operator default scope set.
- If the target agent policy allows sensitive tools, this endpoint can use them.
- Keep this endpoint on loopback/tailnet/private ingress only; do not expose it directly to the public internet.

Auth matrix:

- `gateway.auth.mode="token"` or `"password"` \+ `Authorization: Bearer ...`
  - proves possession of the shared gateway operator secret
  - ignores narrower `x-openclaw-scopes`
  - restores the full default operator scope set:
    `operator.admin`, `operator.approvals`, `operator.pairing`,
    `operator.read`, `operator.talk.secrets`, `operator.write`
  - treats chat turns on this endpoint as owner-sender turns
- trusted identity-bearing HTTP modes (for example trusted proxy auth, or `gateway.auth.mode="none"` on private ingress)

  - authenticate some outer trusted identity or deployment boundary
  - honor `x-openclaw-scopes` when the header is present
  - fall back to the normal operator default scope set when the header is absent
  - only lose owner semantics when the caller explicitly narrows scopes and omits `operator.admin`

See [Security](https://docs.openclaw.ai/gateway/security) and [Remote access](https://docs.openclaw.ai/gateway/remote).

## Agent-first model contract

OpenClaw treats the OpenAI `model` field as an **agent target**, not a raw provider model id.

- `model: "openclaw"` routes to the configured default agent.
- `model: "opencla

_… [truncated; see https://docs.openclaw.ai/gateway/openai-http-api for full content]_


---

## OpenResponses API - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/openresponses-http-api>_

[OpenClaw home page](https://docs.openclaw.ai/)

Protocols and APIs

OpenResponses API

OpenClaw’s Gateway can serve an OpenResponses-compatible `POST /v1/responses` endpoint.This endpoint is **disabled by default**. Enable it in config first.

- `POST /v1/responses`
- Same port as the Gateway (WS + HTTP multiplex): `http://<gateway-host>:<port>/v1/responses`

Under the hood, requests are executed as a normal Gateway agent run (same codepath as
`openclaw agent`), so routing/permissions/config match your Gateway.

## Authentication, security, and routing

Operational behavior matches [OpenAI Chat Completions](https://docs.openclaw.ai/gateway/openai-http-api):

- use the matching Gateway HTTP auth path:
  - shared-secret auth (`gateway.auth.mode="token"` or `"password"`): `Authorization: Bearer <token-or-password>`
  - trusted-proxy auth (`gateway.auth.mode="trusted-proxy"`): identity-aware proxy headers from a configured trusted proxy source; same-host loopback proxies require explicit `gateway.auth.trustedProxy.allowLoopback = true`
  - private-ingress open auth (`gateway.auth.mode="none"`): no auth header
- treat the endpoint as full operator access for the gateway instance
- for shared-secret auth modes (`token` and `password`), ignore narrower bearer-declared `x-openclaw-scopes` values and restore the normal full operator defaults
- for trusted identity-bearing HTTP modes (for example trusted proxy auth or `gateway.auth.mode="none"`), honor `x-openclaw-scopes` when present and otherwise fall back to the normal operator default scope set
- select agents with `model: "openclaw"`, `model: "openclaw/default"`, `model: "openclaw/<agentId>"`, or `x-openclaw-agent-id`
- use `x-openclaw-model` when you want to override the selected agent’s backend model
- use `x-openclaw-session-key` for explicit session routing
- use `x-openclaw-message-channel` when you want a non-default synthetic ingress channel context

Auth matrix:

- `gateway.auth.mode="token"` or `"password"` \+ `Authorization: Bearer ...`
  - proves possession of the shared gateway operator secret
  - ignores narrower `x-openclaw-scopes`
  - restores the full default operator scope set:
    `operator.admin`, `operator.approvals`, `operator.pairing`,
    `operator.read`, `operator.talk.secrets`, `operator.write`
  - treats chat turns on this endpoint as owner-sender turns
- trusted identity-bearing HTTP modes (for example trusted proxy auth, or `gateway.auth.mode="none"` on private ingress)

  - honor `x-openclaw-scopes` when the header is present
  - fall back to the normal operator default scope set when the header is absent
  - only lose owner semantics when the caller explicitly narrows scopes and omits `operator.admin`

Enable or disable this endpoint with `gateway.http.endpoints.responses.enabled`.The same compatibility surface also includes:

- `GET /v1/models`
- `GET /v1/models/{id}`
- `POST /v1/embeddings`
- `POST /v1/chat/completions`

For the canonical explanation of how agent-target models, `openclaw/default`, embeddings pass-through, and backend model overrides fit together, see [OpenAI Chat Completions](https://docs.openclaw.ai/gateway/openai-http-api#agent-first-model-contract) and [Model list and agent routing](https://docs.openclaw.ai/gateway/openai-http-api#model-list-and-agent-routing).

## Session behavior

By default the endpoint is **stateless per request** (a new session key is generated each call).If the request includes an OpenResponses `user` string, the Gateway derives a stable session key
from it, so repeated calls can share an agent session.

## Request shape (supported)

The request follows the OpenResponses API with item-based input. Current support:

- `input`: string or array of item objects.
- `instructions`: merged into the system prompt.
- `tools`: client tool definitions (function tools).
- `tool_choice`: filter or require client tools.
- `stream`: enables SSE streaming.
- `max_output_tokens`: best-effort output limit (provider dependent

_… [truncated; see https://docs.openclaw.ai/gateway/openresponses-http-api for full content]_


---

## OpenTelemetry export - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/opentelemetry>_

[OpenClaw home page](https://docs.openclaw.ai/)

Health and diagnostics

OpenTelemetry export

OpenClaw exports diagnostics through the official `diagnostics-otel` plugin
using **OTLP/HTTP (protobuf)**. Any collector or backend that accepts OTLP/HTTP
works without code changes. For local file logs and how to read them, see
[Logging](https://docs.openclaw.ai/logging).

## How it fits together

- **Diagnostics events** are structured, in-process records emitted by the
Gateway and bundled plugins for model runs, message flow, sessions, queues,
and exec.
- **`diagnostics-otel` plugin** subscribes to those events and exports them as
OpenTelemetry **metrics**, **traces**, and **logs** over OTLP/HTTP.
- **Provider calls** receive a W3C `traceparent` header from OpenClaw’s
trusted model-call span context when the provider transport accepts custom
headers. Plugin-emitted trace context is not propagated.
- Exporters only attach when both the diagnostics surface and the plugin are
enabled, so the in-process cost stays near zero by default.

## Quick start

For packaged installs, install the plugin first:

```
openclaw plugins install @openclaw/diagnostics-otel
```

```
{
  plugins: {
    allow: ["diagnostics-otel"],
    entries: {
      "diagnostics-otel": { enabled: true },
    },
  },
  diagnostics: {
    enabled: true,
    otel: {
      enabled: true,
      endpoint: "http://otel-collector:4318",
      protocol: "http/protobuf",
      serviceName: "openclaw-gateway",
      traces: true,
      metrics: true,
      logs: true,
      sampleRate: 0.2,
      flushIntervalMs: 60000,
    },
  },
}
```

You can also enable the plugin from the CLI:

```
openclaw plugins enable diagnostics-otel
```

`protocol` currently supports `http/protobuf` only. `grpc` is ignored.

## Signals exported

| Signal | What goes in it |
| --- | --- |
| **Metrics** | Counters and histograms for token usage, cost, run duration, message flow, queue lanes, session state, exec, and memory pressure. |
| **Traces** | Spans for model usage, model calls, harness lifecycle, tool execution, exec, webhook/message processing, context assembly, and tool loops. |
| **Logs** | Structured `logging.file` records exported over OTLP when `diagnostics.otel.logs` is enabled. |

Toggle `traces`, `metrics`, and `logs` independently. All three default to on
when `diagnostics.otel.enabled` is true.

## Configuration reference

```
{
  diagnostics: {
    enabled: true,
    otel: {
      enabled: true,
      endpoint: "http://otel-collector:4318",
      tracesEndpoint: "http://otel-collector:4318/v1/traces",
      metricsEndpoint: "http://otel-collector:4318/v1/metrics",
      logsEndpoint: "http://otel-collector:4318/v1/logs",
      protocol: "http/protobuf", // grpc is ignored
      serviceName: "openclaw-gateway",
      headers: { "x-collector-token": "..." },
      traces: true,
      metrics: true,
      logs: true,
      sampleRate: 0.2, // root-span sampler, 0.0..1.0
      flushIntervalMs: 60000, // metric export interval (min 1000ms)
      captureContent: {
        enabled: false,
        inputMessages: false,
        outputMessages: false,
        toolInputs: false,
        toolOutputs: false,
        systemPrompt: false,
      },
    },
  },
}
```

### Environment variables

| Variable | Purpose |
| --- | --- |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | Override `diagnostics.otel.endpoint`. If the value already contains `/v1/traces`, `/v1/metrics`, or `/v1/logs`, it is used as-is. |
| `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT` / `OTEL_EXPORTER_OTLP_METRICS_ENDPOINT` / `OTEL_EXPORTER_OTLP_LOGS_ENDPOINT` | Signal-specific endpoint overrides used when the matching `diagnostics.otel.*Endpoint` config key is unset. Signal-specific config wins over signal-specific env, which wins over the shared endpoint. |
| `OTEL_SERVICE_NAME` | Override `diagnostics.otel.serviceName`. |
| `OTEL_EXPORTER_OTLP_PROTOCOL` | Override the wire protocol (only `http/protobuf` is honored today). |
| `OTEL_SEMCONV_STABILITY

_… [truncated; see https://docs.openclaw.ai/gateway/opentelemetry for full content]_


---

## Operator scopes - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/operator-scopes>_

[OpenClaw home page](https://docs.openclaw.ai/)

Security and sandboxing

Operator scopes

Operator scopes define what a Gateway client may do after it authenticates.
They are a control-plane guardrail inside one trusted Gateway operator domain,
not hostile multi-tenant isolation. If you need strong separation between
people, teams, or machines, run separate Gateways under separate OS users or
hosts.Related: [Security](https://docs.openclaw.ai/gateway/security), [Gateway protocol](https://docs.openclaw.ai/gateway/protocol),
[Gateway pairing](https://docs.openclaw.ai/gateway/pairing), [Devices CLI](https://docs.openclaw.ai/cli/devices).

## Roles

Gateway WebSocket clients connect with one role:

- `operator`: control-plane clients such as CLI, Control UI, automation, and
trusted helper processes.
- `node`: capability hosts such as macOS, iOS, Android, or headless nodes that
expose commands through `node.invoke`.

Operator RPC methods require the `operator` role. Node-originated methods
require the `node` role.

## Scope levels

| Scope | Meaning |
| --- | --- |
| `operator.read` | Read-only status, lists, catalog, logs, session reads, and other non-mutating control-plane calls. |
| `operator.write` | Normal mutating operator actions such as sending messages, invoking tools, updating talk/voice settings, and node command relay. Also satisfies `operator.read`. |
| `operator.admin` | Administrative control-plane access. Satisfies every `operator.*` scope. Required for config mutation, updates, native hooks, sensitive reserved namespaces, and high-risk approvals. |
| `operator.pairing` | Device and node pairing management, including listing, approving, rejecting, removing, rotating, and revoking pairing records or device tokens. |
| `operator.approvals` | Exec and plugin approval APIs. |
| `operator.talk.secrets` | Reading Talk configuration with secrets included. |

Unknown future `operator.*` scopes require an exact match unless the caller has
`operator.admin`.

## Method scope is only the first gate

Each Gateway RPC has a least-privilege method scope. That method scope decides
whether the request can reach the handler. Some handlers then apply stricter
approval-time checks based on the concrete thing being approved or mutated.Examples:

- `device.pair.approve` is reachable with `operator.pairing`, but approving an
operator device can only mint or preserve scopes the caller already holds.
- `node.pair.approve` is reachable with `operator.pairing`, then derives extra
approval scopes from the pending node command list.
- `chat.send` is normally a write-scoped method, but persistent `/config set`
and `/config unset` require `operator.admin` at command level.

This lets lower-scope operators perform low-risk pairing actions without making
all pairing approval admin-only.

## Device pairing approvals

Device pairing records are the durable source of approved roles and scopes.
Already paired devices do not get broader access silently: reconnects that ask
for a broader role or broader scopes create a new pending upgrade request.When approving a device request:

- A request with no operator role does not need operator token scope approval.
- A request for `operator.read`, `operator.write`, `operator.approvals`,
`operator.pairing`, or `operator.talk.secrets` requires the caller to hold
those scopes, or `operator.admin`.
- A request for `operator.admin` requires `operator.admin`.
- A repair request with no explicit scopes can inherit the existing operator
token scopes. If that existing token is admin-scoped, approval still requires
`operator.admin`.

For paired-device token sessions, management is self-scoped unless the caller
also has `operator.admin`: non-admin callers can rotate, revoke, or remove only
their own device entry.

## Node pairing approvals

Legacy `node.pair.*` uses a separate Gateway-owned node pairing store. WS nodes
use device pairing with `role: node`, but the same approval-level vocabulary
applies.`node.pair.approve`

_… [truncated; see https://docs.openclaw.ai/gateway/operator-scopes for full content]_


---

## Prometheus metrics - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/prometheus>_

# prometheus.yml
scrape_configs:
  - job_name: openclaw
    scrape_interval: 30s
    metrics_path: /api/diagnostics/prometheus
    authorization:
      credentials_file: /etc/prometheus/openclaw-gateway-token
    static_configs:
      - targets: ["openclaw-gateway:18789"]
```

`diagnostics.enabled: true` is required. Without it, the plugin still registers the HTTP route but no diagnostic events flow into the exporter, so the response is empty.

## Metrics exported

| Metric | Type | Labels |
| --- | --- | --- |
| `openclaw_run_completed_total` | counter | `channel`, `model`, `outcome`, `provider`, `trigger` |
| `openclaw_run_duration_seconds` | histogram | `channel`, `model`, `outcome`, `provider`, `trigger` |
| `openclaw_model_call_total` | counter | `api`, `error_category`, `model`, `outcome`, `provider`, `transport` |
| `openclaw_model_call_duration_seconds` | histogram | `api`, `error_category`, `model`, `outcome`, `provider`, `transport` |
| `openclaw_model_tokens_total` | counter | `agent`, `channel`, `model`, `provider`, `token_type` |
| `openclaw_gen_ai_client_token_usage` | histogram | `model`, `provider`, `token_type` |
| `openclaw_model_cost_usd_total` | counter | `agent`, `channel`, `model`, `provider` |
| `openclaw_tool_execution_total` | counter | `error_category`, `outcome`, `params_kind`, `tool` |
| `openclaw_tool_execution_duration_seconds` | histogram | `error_category`, `outcome`, `params_kind`, `tool` |
| `openclaw_harness_run_total` | counter | `channel`, `error_category`, `harness`, `model`, `outcome`, `phase`, `plugin`, `provider` |
| `openclaw_harness_run_duration_seconds` | histogram | `channel`, `error_category`, `harness`, `model`, `outcome`, `phase`, `plugin`, `provider` |
| `openclaw_message_processed_total` | counter | `channel`, `outcome`, `reason` |
| `openclaw_message_processed_duration_seconds` | histogram | `channel`, `outcome`, `reason` |
| `openclaw_message_delivery_total` | counter | `channel`, `delivery_kind`, `error_category`, `outcome` |
| `openclaw_message_delivery_duration_seconds` | histogram | `channel`, `delivery_kind`, `error_category`, `outcome` |
| `openclaw_queue_lane_size` | gauge | `lane` |
| `openclaw_queue_lane_wait_seconds` | histogram | `lane` |
| `openclaw_session_state_total` | counter | `reason`, `state` |
| `openclaw_session_queue_depth` | gauge | `state` |
| `openclaw_memory_bytes` | gauge | `kind` |
| `openclaw_memory_rss_bytes` | histogram | none |
| `openclaw_memory_pressure_total` | counter | `level`, `reason` |
| `openclaw_telemetry_exporter_total` | counter | `exporter`, `reason`, `signal`, `status` |
| `openclaw_prometheus_series_dropped_total` | counter | none |

## Label policy

Bounded, low-cardinality labels

Prometheus labels stay bounded and low-cardinality. The exporter does not emit raw diagnostic identifiers such as `runId`, `sessionKey`, `sessionId`, `callId`, `toolCallId`, message IDs, chat IDs, or provider request IDs.Label values are redacted and must match OpenClaw’s low-cardinality character policy. Values that fail the policy are replaced with `unknown`, `other`, or `none`, depending on the metric.

Series cap and overflow accounting

The exporter caps retained time series in memory at **2048** series across counters, gauges, and histograms combined. New series beyond that cap are dropped, and `openclaw_prometheus_series_dropped_total` increments by one each time.Watch this counter as a hard signal that an attribute upstream is leaking high-cardinality values. The exporter never lifts the cap automatically; if it climbs, fix the source rather than disabling the cap.

What never appears in Prometheus output

- prompt text, response text, tool inputs, tool outputs, system prompts
- raw provider request IDs (only bounded hashes, where applicable, on spans — never on metrics)
- session keys and session IDs
- hostnames, file paths, secret values

## PromQL recipes

```
# Tokens per minute, split by provider
sum by (provider) (rate(openclaw_model_tok

_… [truncated; see https://docs.openclaw.ai/gateway/prometheus for full content]_


---

## Gateway protocol - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/protocol>_

[OpenClaw home page](https://docs.openclaw.ai/)

Protocols and APIs

Gateway protocol

The Gateway WS protocol is the **single control plane + node transport** for
OpenClaw. All clients (CLI, web UI, macOS app, iOS/Android nodes, headless
nodes) connect over WebSocket and declare their **role** \+ **scope** at
handshake time.

## Transport

- WebSocket, text frames with JSON payloads.
- First frame **must** be a `connect` request.
- Pre-connect frames are capped at 64 KiB. After a successful handshake, clients
should follow the `hello-ok.policy.maxPayload` and
`hello-ok.policy.maxBufferedBytes` limits. With diagnostics enabled,
oversized inbound frames and slow outbound buffers emit `payload.large` events
before the gateway closes or drops the affected frame. These events keep
sizes, limits, surfaces, and safe reason codes. They do not keep the message
body, attachment contents, raw frame body, tokens, cookies, or secret values.

## Handshake (connect)

Gateway → Client (pre-connect challenge):

```
{
  "type": "event",
  "event": "connect.challenge",
  "payload": { "nonce": "…", "ts": 1737264000000 }
}
```

Client → Gateway:

```
{
  "type": "req",
  "id": "…",
  "method": "connect",
  "params": {
    "minProtocol": 3,
    "maxProtocol": 3,
    "client": {
      "id": "cli",
      "version": "1.2.3",
      "platform": "macos",
      "mode": "operator"
    },
    "role": "operator",
    "scopes": ["operator.read", "operator.write"],
    "caps": [],
    "commands": [],
    "permissions": {},
    "auth": { "token": "…" },
    "locale": "en-US",
    "userAgent": "openclaw-cli/1.2.3",
    "device": {
      "id": "device_fingerprint",
      "publicKey": "…",
      "signature": "…",
      "signedAt": 1737264000000,
      "nonce": "…"
    }
  }
}
```

Gateway → Client:

```
{
  "type": "res",
  "id": "…",
  "ok": true,
  "payload": {
    "type": "hello-ok",
    "protocol": 3,
    "server": { "version": "…", "connId": "…" },
    "features": { "methods": ["…"], "events": ["…"] },
    "snapshot": { "…": "…" },
    "auth": {
      "role": "operator",
      "scopes": ["operator.read", "operator.write"]
    },
    "policy": {
      "maxPayload": 26214400,
      "maxBufferedBytes": 52428800,
      "tickIntervalMs": 15000
    }
  }
}
```

While the Gateway is still finishing startup sidecars, the `connect` request can
return a retryable `UNAVAILABLE` error with `details.reason` set to
`"startup-sidecars"` and `retryAfterMs`. Clients should retry that response
within their overall connection budget instead of surfacing it as a terminal
handshake failure.`server`, `features`, `snapshot`, and `policy` are all required by the schema
(`src/gateway/protocol/schema/frames.ts`). `auth` is also required and reports
the negotiated role/scopes. `canvasHostUrl` is optional.When no device token is issued, `hello-ok.auth` reports the negotiated
permissions without token fields:

```
{
  "auth": {
    "role": "operator",
    "scopes": ["operator.read", "operator.write"]
  }
}
```

Trusted same-process backend clients (`client.id: "gateway-client"`,
`client.mode: "backend"`) may omit `device` on direct loopback connections when
they authenticate with the shared gateway token/password. This path is reserved
for internal control-plane RPCs and keeps stale CLI/device pairing baselines from
blocking local backend work such as subagent session updates. Remote clients,
browser-origin clients, node clients, and explicit device-token/device-identity
clients still use the normal pairing and scope-upgrade checks.When a device token is issued, `hello-ok` also includes:

```
{
  "auth": {
    "deviceToken": "…",
    "role": "operator",
    "scopes": ["operator.read", "operator.write"]
  }
}
```

During trusted bootstrap handoff, `hello-ok.auth` may also include additional
bounded role entries in `deviceTokens`:

```
{
  "auth": {
    "deviceToken": "…",
    "role": "node",
    "scopes": [],
    "deviceTokens": [\
      {\
        "deviceToken": "…",\
        "role": "o

_… [truncated; see https://docs.openclaw.ai/gateway/protocol for full content]_


---

## Remote access - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/remote>_

[OpenClaw home page](https://docs.openclaw.ai/)

Remote access

Remote access

This repo supports “remote over SSH” by keeping a single Gateway (the master) running on a dedicated host (desktop/server) and connecting clients to it.

- For **operators (you / the macOS app)**: SSH tunneling is the universal fallback.
- For **nodes (iOS/Android and future devices)**: connect to the Gateway **WebSocket** (LAN/tailnet or SSH tunnel as needed).

## The core idea

- The Gateway WebSocket binds to **loopback** on your configured port (defaults to 18789).
- For remote use, you forward that loopback port over SSH (or use a tailnet/VPN and tunnel less).

## Common VPN and tailnet setups

Think of the **Gateway host** as where the agent lives. It owns sessions, auth profiles, channels, and state. Your laptop, desktop, and nodes connect to that host.

### Always-on Gateway in your tailnet

Run the Gateway on a persistent host (VPS or home server) and reach it via **Tailscale** or SSH.

- **Best UX:** keep `gateway.bind: "loopback"` and use **Tailscale Serve** for the Control UI.
- **Fallback:** keep loopback plus SSH tunnel from any machine that needs access.
- **Examples:** [exe.dev](https://docs.openclaw.ai/install/exe-dev) (easy VM) or [Hetzner](https://docs.openclaw.ai/install/hetzner) (production VPS).

Ideal when your laptop sleeps often but you want the agent always-on.

### Home desktop runs the Gateway

The laptop does **not** run the agent. It connects remotely:

- Use the macOS app’s **Remote over SSH** mode (Settings → General → OpenClaw runs).
- The app opens and manages the tunnel, so WebChat and health checks just work.

Runbook: [macOS remote access](https://docs.openclaw.ai/platforms/mac/remote).

### Laptop runs the Gateway

Keep the Gateway local but expose it safely:

- SSH tunnel to the laptop from other machines, or
- Tailscale Serve the Control UI and keep the Gateway loopback-only.

Guides: [Tailscale](https://docs.openclaw.ai/gateway/tailscale) and [Web overview](https://docs.openclaw.ai/web).

## Command flow (what runs where)

One gateway service owns state + channels. Nodes are peripherals.Flow example (Telegram → node):

- Telegram message arrives at the **Gateway**.
- Gateway runs the **agent** and decides whether to call a node tool.
- Gateway calls the **node** over the Gateway WebSocket (`node.*` RPC).
- Node returns the result; Gateway replies back out to Telegram.

Notes:

- **Nodes do not run the gateway service.** Only one gateway should run per host unless you intentionally run isolated profiles (see [Multiple gateways](https://docs.openclaw.ai/gateway/multiple-gateways)).
- macOS app “node mode” is just a node client over the Gateway WebSocket.

## SSH tunnel (CLI + tools)

Create a local tunnel to the remote Gateway WS:

```
ssh -N -L 18789:127.0.0.1:18789 user@host
```

With the tunnel up:

- `openclaw health` and `openclaw status --deep` now reach the remote gateway via `ws://127.0.0.1:18789`.
- `openclaw gateway status`, `openclaw gateway health`, `openclaw gateway probe`, and `openclaw gateway call` can also target the forwarded URL via `--url` when needed.

Replace `18789` with your configured `gateway.port` (or `--port` or `OPENCLAW_GATEWAY_PORT`).

When you pass `--url`, the CLI does not fall back to config or environment credentials. Include `--token` or `--password` explicitly. Missing explicit credentials is an error.

## CLI remote defaults

You can persist a remote target so CLI commands use it by default:

```
{
  gateway: {
    mode: "remote",
    remote: {
      url: "ws://127.0.0.1:18789",
      token: "your-token",
    },
  },
}
```

When the gateway is loopback-only, keep the URL at `ws://127.0.0.1:18789` and open the SSH tunnel first.
In the macOS app’s SSH tunnel transport, discovered gateway hostnames belong in
`gateway.remote.sshTarget`; `gateway.remote.url` remains the local tunnel URL.

## Credential precedence

Gateway credential resolution follows one shared contract across

_… [truncated; see https://docs.openclaw.ai/gateway/remote for full content]_


---

## Sandboxing - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/sandboxing>_

[OpenClaw home page](https://docs.openclaw.ai/)

Security and sandboxing

Sandboxing

OpenClaw can run **tools inside sandbox backends** to reduce blast radius. This is **optional** and controlled by configuration (`agents.defaults.sandbox` or `agents.list[].sandbox`). If sandboxing is off, tools run on the host. The Gateway stays on the host; tool execution runs in an isolated sandbox when enabled.

This is not a perfect security boundary, but it materially limits filesystem and process access when the model does something dumb.

## What gets sandboxed

- Tool execution (`exec`, `read`, `write`, `edit`, `apply_patch`, `process`, etc.).
- Optional sandboxed browser (`agents.defaults.sandbox.browser`).

Sandboxed browser details

- By default, the sandbox browser auto-starts (ensures CDP is reachable) when the browser tool needs it. Configure via `agents.defaults.sandbox.browser.autoStart` and `agents.defaults.sandbox.browser.autoStartTimeoutMs`.
- By default, sandbox browser containers use a dedicated Docker network (`openclaw-sandbox-browser`) instead of the global `bridge` network. Configure with `agents.defaults.sandbox.browser.network`.
- Optional `agents.defaults.sandbox.browser.cdpSourceRange` restricts container-edge CDP ingress with a CIDR allowlist (for example `172.21.0.1/32`).
- noVNC observer access is password-protected by default; OpenClaw emits a short-lived token URL that serves a local bootstrap page and opens noVNC with password in URL fragment (not query/header logs).
- `agents.defaults.sandbox.browser.allowHostControl` lets sandboxed sessions target the host browser explicitly.
- Optional allowlists gate `target: "custom"`: `allowedControlUrls`, `allowedControlHosts`, `allowedControlPorts`.

Not sandboxed:

- The Gateway process itself.
- Any tool explicitly allowed to run outside the sandbox (e.g. `tools.elevated`).

  - **Elevated exec bypasses sandboxing and uses the configured escape path (`gateway` by default, or `node` when the exec target is `node`).**
  - If sandboxing is off, `tools.elevated` does not change execution (already on host). See [Elevated Mode](https://docs.openclaw.ai/tools/elevated).

## Modes

`agents.defaults.sandbox.mode` controls **when** sandboxing is used:

- off

- non-main

- all

No sandboxing.

Sandbox only **non-main** sessions (default if you want normal chats on host).`"non-main"` is based on `session.mainKey` (default `"main"`), not agent id. Group/channel sessions use their own keys, so they count as non-main and will be sandboxed.

Every session runs in a sandbox.

## Scope

`agents.defaults.sandbox.scope` controls **how many containers** are created:

- `"agent"` (default): one container per agent.
- `"session"`: one container per session.
- `"shared"`: one container shared by all sandboxed sessions.

## Backend

`agents.defaults.sandbox.backend` controls **which runtime** provides the sandbox:

- `"docker"` (default when sandboxing is enabled): local Docker-backed sandbox runtime.
- `"ssh"`: generic SSH-backed remote sandbox runtime.
- `"openshell"`: OpenShell-backed sandbox runtime.

SSH-specific config lives under `agents.defaults.sandbox.ssh`. OpenShell-specific config lives under `plugins.entries.openshell.config`.

### Choosing a backend

|  | Docker | SSH | OpenShell |
| --- | --- | --- | --- |
| **Where it runs** | Local container | Any SSH-accessible host | OpenShell managed sandbox |
| **Setup** | `scripts/sandbox-setup.sh` | SSH key + target host | OpenShell plugin enabled |
| **Workspace model** | Bind-mount or copy | Remote-canonical (seed once) | `mirror` or `remote` |
| **Network control** | `docker.network` (default: none) | Depends on remote host | Depends on OpenShell |
| **Browser sandbox** | Supported | Not supported | Not supported yet |
| **Bind mounts** | `docker.binds` | N/A | N/A |
| **Best for** | Local dev, full isolation | Offloading to a remote machine | Managed remote sandboxes with optional two-way sync |

### Docker backend

Sandboxing

_… [truncated; see https://docs.openclaw.ai/gateway/sandboxing for full content]_


---

## Secrets management - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/secrets>_

[OpenClaw home page](https://docs.openclaw.ai/)

Authentication and secrets

Secrets management

OpenClaw supports additive SecretRefs so supported credentials do not need to be stored as plaintext in configuration.

Plaintext still works. SecretRefs are opt-in per credential.

## Goals and runtime model

Secrets are resolved into an in-memory runtime snapshot.

- Resolution is eager during activation, not lazy on request paths.
- Startup fails fast when an effectively active SecretRef cannot be resolved.
- Reload uses atomic swap: full success, or keep the last-known-good snapshot.
- SecretRef policy violations (for example OAuth-mode auth profiles combined with SecretRef input) fail activation before runtime swap.
- Runtime requests read from the active in-memory snapshot only.
- After the first successful config activation/load, runtime code paths keep reading that active in-memory snapshot until a successful reload swaps it.
- Outbound delivery paths also read from that active snapshot (for example Discord reply/thread delivery and Telegram action sends); they do not re-resolve SecretRefs on each send.

This keeps secret-provider outages off hot request paths.

## Active-surface filtering

SecretRefs are validated only on effectively active surfaces.

- Enabled surfaces: unresolved refs block startup/reload.
- Inactive surfaces: unresolved refs do not block startup/reload.
- Inactive refs emit non-fatal diagnostics with code `SECRETS_REF_IGNORED_INACTIVE_SURFACE`.

Examples of inactive surfaces

- Disabled channel/account entries.
- Top-level channel credentials that no enabled account inherits.
- Disabled tool/feature surfaces.
- Web search provider-specific keys that are not selected by `tools.web.search.provider`. In auto mode (provider unset), keys are consulted by precedence for provider auto-detection until one resolves. After selection, non-selected provider keys are treated as inactive until selected.
- Sandbox SSH auth material (`agents.defaults.sandbox.ssh.identityData`, `certificateData`, `knownHostsData`, plus per-agent overrides) is active only when the effective sandbox backend is `ssh` for the default agent or an enabled agent.
- `gateway.remote.token` / `gateway.remote.password` SecretRefs are active if one of these is true:

  - `gateway.mode=remote`
  - `gateway.remote.url` is configured
  - `gateway.tailscale.mode` is `serve` or `funnel`
  - In local mode without those remote surfaces:
    - `gateway.remote.token` is active when token auth can win and no env/auth token is configured.
    - `gateway.remote.password` is active only when password auth can win and no env/auth password is configured.
- `gateway.auth.token` SecretRef is inactive for startup auth resolution when `OPENCLAW_GATEWAY_TOKEN` is set, because env token input wins for that runtime.

## Gateway auth surface diagnostics

When a SecretRef is configured on `gateway.auth.token`, `gateway.auth.password`, `gateway.remote.token`, or `gateway.remote.password`, gateway startup/reload logs the surface state explicitly:

- `active`: the SecretRef is part of the effective auth surface and must resolve.
- `inactive`: the SecretRef is ignored for this runtime because another auth surface wins, or because remote auth is disabled/not active.

These entries are logged with `SECRETS_GATEWAY_AUTH_SURFACE` and include the reason used by the active-surface policy, so you can see why a credential was treated as active or inactive.

## Onboarding reference preflight

When onboarding runs in interactive mode and you choose SecretRef storage, OpenClaw runs preflight validation before saving:

- Env refs: validates env var name and confirms a non-empty value is visible during setup.
- Provider refs (`file` or `exec`): validates provider selection, resolves `id`, and checks resolved value type.
- Quickstart reuse path: when `gateway.auth.token` is already a SecretRef, onboarding resolves it before probe/dashboard bootstrap (for `env`, `file`, and `exec` refs) usi

_… [truncated; see https://docs.openclaw.ai/gateway/secrets for full content]_


---

## Security - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/security>_

# Optional. Default false.
  # Only enable if your proxy cannot provide X-Forwarded-For.
  allowRealIpFallback: false
  auth:
    mode: password
    password: ${OPENCLAW_GATEWAY_PASSWORD}
```

When `trustedProxies` is configured, the Gateway uses `X-Forwarded-For` to determine the client IP. `X-Real-IP` is ignored by default unless `gateway.allowRealIpFallback: true` is explicitly set.Trusted proxy headers do not make node device pairing automatically trusted.
`gateway.nodes.pairing.autoApproveCidrs` is a separate, disabled-by-default
operator policy. Even when enabled, loopback-source trusted-proxy header paths
are excluded from node auto-approval because local callers can forge those
headers, including when loopback trusted-proxy auth is explicitly enabled.Good reverse proxy behavior (overwrite incoming forwarding headers):

```
proxy_set_header X-Forwarded-For $remote_addr;
proxy_set_header X-Real-IP $remote_addr;
```

Bad reverse proxy behavior (append/preserve untrusted forwarding headers):

```
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

## HSTS and origin notes

- OpenClaw gateway is local/loopback first. If you terminate TLS at a reverse proxy, set HSTS on the proxy-facing HTTPS domain there.
- If the gateway itself terminates HTTPS, you can set `gateway.http.securityHeaders.strictTransportSecurity` to emit the HSTS header from OpenClaw responses.
- Detailed deployment guidance is in [Trusted Proxy Auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth#tls-termination-and-hsts).
- For non-loopback Control UI deployments, `gateway.controlUi.allowedOrigins` is required by default.
- `gateway.controlUi.allowedOrigins: ["*"]` is an explicit allow-all browser-origin policy, not a hardened default. Avoid it outside tightly controlled local testing.
- Browser-origin auth failures on loopback are still rate-limited even when the
general loopback exemption is enabled, but the lockout key is scoped per
normalized `Origin` value instead of one shared localhost bucket.
- `gateway.controlUi.dangerouslyAllowHostHeaderOriginFallback=true` enables Host-header origin fallback mode; treat it as a dangerous operator-selected policy.
- Treat DNS rebinding and proxy-host header behavior as deployment hardening concerns; keep `trustedProxies` tight and avoid exposing the gateway directly to the public internet.

## Local session logs live on disk

OpenClaw stores session transcripts on disk under `~/.openclaw/agents/<agentId>/sessions/*.jsonl`.
This is required for session continuity and (optionally) session memory indexing, but it also means
**any process/user with filesystem access can read those logs**. Treat disk access as the trust
boundary and lock down permissions on `~/.openclaw` (see the audit section below). If you need
stronger isolation between agents, run them under separate OS users or separate hosts.

## Node execution (system.run)

If a macOS node is paired, the Gateway can invoke `system.run` on that node. This is **remote code execution** on the Mac:

- Requires node pairing (approval + token).
- Gateway node pairing is not a per-command approval surface. It establishes node identity/trust and token issuance.
- The Gateway applies a coarse global node command policy via `gateway.nodes.allowCommands` / `denyCommands`.
- Controlled on the Mac via **Settings → Exec approvals** (security + ask + allowlist).
- The per-node `system.run` policy is the node’s own exec approvals file (`exec.approvals.node.*`), which can be stricter or looser than the gateway’s global command-ID policy.
- A node running with `security="full"` and `ask="off"` is following the default trusted-operator model. Treat that as expected behavior unless your deployment explicitly requires a tighter approval or allowlist stance.
- Approval mode binds exact request context and, when possible, one concrete local script/file operand. If OpenClaw cannot identify exactly one direct local file for an interpreter/runtime command, approval-ba

_… [truncated; see https://docs.openclaw.ai/gateway/security for full content]_


---

## Tailscale - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/tailscale>_

[OpenClaw home page](https://docs.openclaw.ai/)

Remote access

Tailscale

OpenClaw can auto-configure Tailscale **Serve** (tailnet) or **Funnel** (public) for the
Gateway dashboard and WebSocket port. This keeps the Gateway bound to loopback while
Tailscale provides HTTPS, routing, and (for Serve) identity headers.

## Modes

- `serve`: Tailnet-only Serve via `tailscale serve`. The gateway stays on `127.0.0.1`.
- `funnel`: Public HTTPS via `tailscale funnel`. OpenClaw requires a shared password.
- `off`: Default (no Tailscale automation).

Status and audit output use **Tailscale exposure** for this OpenClaw Serve/Funnel
mode. `off` means OpenClaw is not managing Serve or Funnel; it does not mean the
local Tailscale daemon is stopped or logged out.

## Auth

Set `gateway.auth.mode` to control the handshake:

- `none` (private ingress only)
- `token` (default when `OPENCLAW_GATEWAY_TOKEN` is set)
- `password` (shared secret via `OPENCLAW_GATEWAY_PASSWORD` or config)
- `trusted-proxy` (identity-aware reverse proxy; see [Trusted Proxy Auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth))

When `tailscale.mode = "serve"` and `gateway.auth.allowTailscale` is `true`,
Control UI/WebSocket auth can use Tailscale identity headers
(`tailscale-user-login`) without supplying a token/password. OpenClaw verifies
the identity by resolving the `x-forwarded-for` address via the local Tailscale
daemon (`tailscale whois`) and matching it to the header before accepting it.
OpenClaw only treats a request as Serve when it arrives from loopback with
Tailscale’s `x-forwarded-for`, `x-forwarded-proto`, and `x-forwarded-host`
headers.
For Control UI operator sessions that include browser device identity, this
verified Serve path also skips the device-pairing round trip. It does not bypass
browser device identity: device-less clients are still rejected, and node-role
or non-Control UI WebSocket connections still follow the normal pairing and
auth checks.
HTTP API endpoints (for example `/v1/*`, `/tools/invoke`, and `/api/channels/*`)
do **not** use Tailscale identity-header auth. They still follow the gateway’s
normal HTTP auth mode: shared-secret auth by default, or an intentionally
configured trusted-proxy / private-ingress `none` setup.
This tokenless flow assumes the gateway host is trusted. If untrusted local code
may run on the same host, disable `gateway.auth.allowTailscale` and require
token/password auth instead.
To require explicit shared-secret credentials, set `gateway.auth.allowTailscale: false`
and use `gateway.auth.mode: "token"` or `"password"`.

## Config examples

### Tailnet-only (Serve)

```
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "serve" },
  },
}
```

Open: `https://<magicdns>/` (or your configured `gateway.controlUi.basePath`)

### Tailnet-only (bind to Tailnet IP)

Use this when you want the Gateway to listen directly on the Tailnet IP (no Serve/Funnel).

```
{
  gateway: {
    bind: "tailnet",
    auth: { mode: "token", token: "your-token" },
  },
}
```

Connect from another Tailnet device:

- Control UI: `http://<tailscale-ip>:18789/`
- WebSocket: `ws://<tailscale-ip>:18789`

Loopback (`http://127.0.0.1:18789`) will **not** work in this mode.

### Public internet (Funnel + shared password)

```
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "funnel" },
    auth: { mode: "password", password: "replace-me" },
  },
}
```

Prefer `OPENCLAW_GATEWAY_PASSWORD` over committing a password to disk.

## CLI examples

```
openclaw gateway --tailscale serve
openclaw gateway --tailscale funnel --auth password
```

## Notes

- Tailscale Serve/Funnel requires the `tailscale` CLI to be installed and logged in.
- `tailscale.mode: "funnel"` refuses to start unless auth mode is `password` to avoid public exposure.
- Set `gateway.tailscale.resetOnExit` if you want OpenClaw to undo `tailscale serve`
or `tailscale funnel` configuration on shutdown.
- `gateway.bind: "tailnet"` is a direct Tailnet bind (no HTTPS,

_… [truncated; see https://docs.openclaw.ai/gateway/tailscale for full content]_


---

## Troubleshooting - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/troubleshooting>_

[OpenClaw home page](https://docs.openclaw.ai/)

Health and diagnostics

Troubleshooting

This page is the deep runbook. Start at [/help/troubleshooting](https://docs.openclaw.ai/help/troubleshooting) if you want the fast triage flow first.

## Command ladder

Run these first, in this order:

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

Expected healthy signals:

- `openclaw gateway status` shows `Runtime: running`, `Connectivity probe: ok`, and a `Capability: ...` line.
- `openclaw doctor` reports no blocking config/service issues.
- `openclaw channels status --probe` shows live per-account transport status and, where supported, probe/audit results such as `works` or `audit ok`.

## Split brain installs and newer config guard

Use this when a gateway service unexpectedly stops after an update, or logs show that one `openclaw` binary is older than the version that last wrote `openclaw.json`.OpenClaw stamps config writes with `meta.lastTouchedVersion`. Read-only commands can still inspect a config written by a newer OpenClaw, but process and service mutations refuse to continue from an older binary. Blocked actions include gateway service start, stop, restart, uninstall, forced service reinstall, service-mode gateway startup, and `gateway --force` port cleanup.

```
which openclaw
openclaw --version
openclaw gateway status --deep
openclaw config get meta.lastTouchedVersion
```

1

[Navigate to header](https://docs.openclaw.ai/gateway/troubleshooting#)

Fix PATH

Fix `PATH` so `openclaw` resolves to the newer install, then rerun the action.

2

[Navigate to header](https://docs.openclaw.ai/gateway/troubleshooting#)

Reinstall the gateway service

Reinstall the intended gateway service from the newer install:

```
openclaw gateway install --force
openclaw gateway restart
```

3

[Navigate to header](https://docs.openclaw.ai/gateway/troubleshooting#)

Remove stale wrappers

Remove stale system package or old wrapper entries that still point at an old `openclaw` binary.

For intentional downgrade or emergency recovery only, set `OPENCLAW_ALLOW_OLDER_BINARY_DESTRUCTIVE_ACTIONS=1` for the single command. Leave it unset for normal operation.

## Anthropic 429 extra usage required for long context

Use this when logs/errors include: `HTTP 429: rate_limit_error: Extra usage is required for long context requests`.

```
openclaw logs --follow
openclaw models status
openclaw config get agents.defaults.models
```

Look for:

- Selected Anthropic Opus/Sonnet model has `params.context1m: true`.
- Current Anthropic credential is not eligible for long-context usage.
- Requests fail only on long sessions/model runs that need the 1M beta path.

Fix options:

1

[Navigate to header](https://docs.openclaw.ai/gateway/troubleshooting#)

Disable context1m

Disable `context1m` for that model to fall back to the normal context window.

2

[Navigate to header](https://docs.openclaw.ai/gateway/troubleshooting#)

Use an eligible credential

Use an Anthropic credential that is eligible for long-context requests, or switch to an Anthropic API key.

3

[Navigate to header](https://docs.openclaw.ai/gateway/troubleshooting#)

Configure fallback models

Configure fallback models so runs continue when Anthropic long-context requests are rejected.

Related:

- [Anthropic](https://docs.openclaw.ai/providers/anthropic)
- [Token use and costs](https://docs.openclaw.ai/reference/token-use)
- [Why am I seeing HTTP 429 from Anthropic?](https://docs.openclaw.ai/help/faq-first-run#why-am-i-seeing-http-429-ratelimiterror-from-anthropic)

## Local OpenAI-compatible backend passes direct probes but agent runs fail

Use this when:

- `curl ... /v1/models` works
- tiny direct `/v1/chat/completions` calls work
- OpenClaw model runs fail only on normal agent turns

```
curl http://127.0.0.1:1234/v1/models
curl http://127.0.0.1:1234/v1/chat/completions \
  -H 'content-type: application/json' \
  -d '{"mod

_… [truncated; see https://docs.openclaw.ai/gateway/troubleshooting for full content]_


---

## Trusted proxy auth - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/trusted-proxy-auth>_

[OpenClaw home page](https://docs.openclaw.ai/)

Authentication and secrets

Trusted proxy auth

**Security-sensitive feature.** This mode delegates authentication entirely to your reverse proxy. Misconfiguration can expose your Gateway to unauthorized access. Read this page carefully before enabling.

## When to use

Use `trusted-proxy` auth mode when:

- You run OpenClaw behind an **identity-aware proxy** (Pomerium, Caddy + OAuth, nginx + oauth2-proxy, Traefik + forward auth).
- Your proxy handles all authentication and passes user identity via headers.
- You’re in a Kubernetes or container environment where the proxy is the only path to the Gateway.
- You’re hitting WebSocket `1008 unauthorized` errors because browsers can’t pass tokens in WS payloads.

## When NOT to use

- If your proxy doesn’t authenticate users (just a TLS terminator or load balancer).
- If there’s any path to the Gateway that bypasses the proxy (firewall holes, internal network access).
- If you’re unsure whether your proxy correctly strips/overwrites forwarded headers.
- If you only need personal single-user access (consider Tailscale Serve + loopback for simpler setup).

## How it works

1

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Proxy authenticates the user

Your reverse proxy authenticates users (OAuth, OIDC, SAML, etc.).

2

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Proxy adds an identity header

Proxy adds a header with the authenticated user identity (e.g., `x-forwarded-user: nick@example.com`).

3

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Gateway verifies trusted source

OpenClaw checks that the request came from a **trusted proxy IP** (configured in `gateway.trustedProxies`).

4

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Gateway extracts identity

OpenClaw extracts the user identity from the configured header.

5

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Authorize

If everything checks out, the request is authorized.

## Control UI pairing behavior

When `gateway.auth.mode = "trusted-proxy"` is active and the request passes trusted-proxy checks, Control UI WebSocket sessions can connect without device pairing identity.Implications:

- Pairing is no longer the primary gate for Control UI access in this mode.
- Your reverse proxy auth policy and `allowUsers` become the effective access control.
- Keep gateway ingress locked to trusted proxy IPs only (`gateway.trustedProxies` \+ firewall).

## Configuration

```
{
  gateway: {
    // Trusted-proxy auth expects requests from a non-loopback trusted proxy source by default
    bind: "lan",

    // CRITICAL: Only add your proxy's IP(s) here
    trustedProxies: ["10.0.0.1", "172.17.0.1"],

    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        // Header containing authenticated user identity (required)
        userHeader: "x-forwarded-user",

        // Optional: headers that MUST be present (proxy verification)
        requiredHeaders: ["x-forwarded-proto", "x-forwarded-host"],

        // Optional: restrict to specific users (empty = allow all)
        allowUsers: ["nick@example.com", "admin@company.org"],

        // Optional: allow a same-host loopback proxy after explicit opt-in
        allowLoopback: false,
      },
    },
  },
}
```

**Important runtime rules**

- Trusted-proxy auth rejects loopback-source requests (`127.0.0.1`, `::1`, loopback CIDRs) by default.
- Same-host loopback reverse proxies do **not** satisfy trusted-proxy auth unless you explicitly set `gateway.auth.trustedProxy.allowLoopback = true` and include the loopback address in `gateway.trustedProxies`.
- `allowLoopback` trusts local processes on the Gateway host to the same degree as the reverse proxy. Enable it only when the Gateway is still firewalled from direct remote access and the local proxy strips or overwrites client-supplied iden

_… [truncated; see https://docs.openclaw.ai/gateway/trusted-proxy-auth for full content]_


---

## Logging - OpenClaw

_Source: <https://docs.openclaw.ai/logging>_

[OpenClaw home page](https://docs.openclaw.ai/)

Health and diagnostics

Logging

OpenClaw has two main log surfaces:

- **File logs** (JSON lines) written by the Gateway.
- **Console output** shown in terminals and the Gateway Debug UI.

The Control UI **Logs** tab tails the gateway file log. This page explains where
logs live, how to read them, and how to configure log levels and formats.

## Where logs live

By default, the Gateway writes a rolling log file under:`/tmp/openclaw/openclaw-YYYY-MM-DD.log`The date uses the gateway host’s local timezone.Each file rotates when it reaches `logging.maxFileBytes` (default: 100 MB).
OpenClaw keeps up to five numbered archives beside the active file, such as
`openclaw-YYYY-MM-DD.1.log`, and keeps writing to a fresh active log instead of
suppressing diagnostics.You can override this in `~/.openclaw/openclaw.json`:

```
{
  "logging": {
    "file": "/path/to/openclaw.log"
  }
}
```

## How to read logs

### CLI: live tail (recommended)

Use the CLI to tail the gateway log file via RPC:

```
openclaw logs --follow
```

Useful current options:

- `--local-time`: render timestamps in your local timezone
- `--url <url>` / `--token <token>` / `--timeout <ms>`: standard Gateway RPC flags
- `--expect-final`: agent-backed RPC final-response wait flag (accepted here via the shared client layer)

Output modes:

- **TTY sessions**: pretty, colorized, structured log lines.
- **Non-TTY sessions**: plain text.
- `--json`: line-delimited JSON (one log event per line).
- `--plain`: force plain text in TTY sessions.
- `--no-color`: disable ANSI colors.

When you pass an explicit `--url`, the CLI does not auto-apply config or
environment credentials; include `--token` yourself if the target Gateway
requires auth.In JSON mode, the CLI emits `type`-tagged objects:

- `meta`: stream metadata (file, cursor, size)
- `log`: parsed log entry
- `notice`: truncation / rotation hints
- `raw`: unparsed log line

If the implicit local loopback Gateway asks for pairing, closes during connect,
or times out before `logs.tail` answers, `openclaw logs` falls back to the
configured Gateway file log automatically. Explicit `--url` targets do not use
this fallback.If the Gateway is unreachable, the CLI prints a short hint to run:

```
openclaw doctor
```

### Control UI (web)

The Control UI’s **Logs** tab tails the same file using `logs.tail`.
See [/web/control-ui](https://docs.openclaw.ai/web/control-ui) for how to open it.

### Channel-only logs

To filter channel activity (WhatsApp/Telegram/etc), use:

```
openclaw channels logs --channel whatsapp
```

## Log formats

### File logs (JSONL)

Each line in the log file is a JSON object. The CLI and Control UI parse these
entries to render structured output (time, level, subsystem, message).File-log JSONL records also include machine-filterable top-level fields when
available:

- `hostname`: gateway host name.
- `message`: flattened log message text for full-text search.
- `agent_id`: active agent id when the log call carries agent context.
- `session_id`: active session id/key when the log call carries session context.
- `channel`: active channel when the log call carries channel context.

OpenClaw preserves the original structured log arguments alongside these fields
so existing parsers that read numbered tslog argument keys keep working.

### Console output

Console logs are **TTY-aware** and formatted for readability:

- Subsystem prefixes (e.g. `gateway/channels/whatsapp`)
- Level coloring (info/warn/error)
- Optional compact or JSON mode

Console formatting is controlled by `logging.consoleStyle`.

### Gateway WebSocket logs

`openclaw gateway` also has WebSocket protocol logging for RPC traffic:

- normal mode: only interesting results (errors, parse errors, slow calls)
- `--verbose`: all request/response traffic
- `--ws-log auto|compact|full`: pick the verbose rendering style
- `--compact`: alias for `--ws-log compact`

Examples:

```
openclaw gateway
openclaw gatew

_… [truncated; see https://docs.openclaw.ai/logging for full content]_


---

_2 additional pages omitted to keep this file ≤ 146 KB. See https://docs.openclaw.ai for full content._
