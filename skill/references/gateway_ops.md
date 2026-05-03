# Gateway Ops

_40 pages from docs.openclaw.ai — full content preserved._

## Contents

- [Diagnostics flags - OpenClaw](#diagnostics-flags---openclaw)
- [Gateway runbook - OpenClaw](#gateway-runbook---openclaw)
- [Authentication - OpenClaw](#authentication---openclaw)
- [Background exec and process tool - OpenClaw](#background-exec-and-process-tool---openclaw)
- [Bonjour discovery - OpenClaw](#bonjour-discovery---openclaw)
- [Configuration — agents - OpenClaw](#configuration-agents---openclaw)
- [Configuration — channels - OpenClaw](#configuration-channels---openclaw)
- [Configuration — tools and custom providers - OpenClaw](#configuration-tools-and-custom-providers---openclaw)
- [Configuration - OpenClaw](#configuration---openclaw)
- [Configuration examples - OpenClaw](#configuration-examples---openclaw)
- [Configuration reference - OpenClaw](#configuration-reference---openclaw)
- [https://docs.openclaw.ai/gateway/configuration-reference.md](#httpsdocsopenclawaigatewayconfiguration-referencemd)
- [https://docs.openclaw.ai/gateway/configuration.md](#httpsdocsopenclawaigatewayconfigurationmd)
- [Diagnostics export - OpenClaw](#diagnostics-export---openclaw)
- [Doctor - OpenClaw](#doctor---openclaw)
- [Health checks - OpenClaw](#health-checks---openclaw)
- [Heartbeat - OpenClaw](#heartbeat---openclaw)
- [https://docs.openclaw.ai/gateway/heartbeat.md](#httpsdocsopenclawaigatewayheartbeatmd)
- [Local models - OpenClaw](#local-models---openclaw)
- [Gateway logging - OpenClaw](#gateway-logging---openclaw)
- [OpenAI chat completions - OpenClaw](#openai-chat-completions---openclaw)
- [OpenResponses API - OpenClaw](#openresponses-api---openclaw)
- [OpenTelemetry export - OpenClaw](#opentelemetry-export---openclaw)
- [Operator scopes - OpenClaw](#operator-scopes---openclaw)
- [Gateway-owned pairing - OpenClaw](#gateway-owned-pairing---openclaw)
- [Prometheus metrics - OpenClaw](#prometheus-metrics---openclaw)
- [Gateway protocol - OpenClaw](#gateway-protocol---openclaw)
- [Remote access - OpenClaw](#remote-access---openclaw)
- [Sandboxing - OpenClaw](#sandboxing---openclaw)
- [Secrets management - OpenClaw](#secrets-management---openclaw)
- [Security - OpenClaw](#security---openclaw)
- [Tailscale - OpenClaw](#tailscale---openclaw)
- [https://docs.openclaw.ai/gateway/tools-invoke-http-api.md](#httpsdocsopenclawaigatewaytools-invoke-http-apimd)
- [Troubleshooting - OpenClaw](#troubleshooting---openclaw)
- [Trusted proxy auth - OpenClaw](#trusted-proxy-auth---openclaw)
- [Logging - OpenClaw](#logging---openclaw)
- [Formal verification (security models) - OpenClaw](#formal-verification-security-models---openclaw)
- [Network proxy - OpenClaw](#network-proxy---openclaw)
- [https://docs.openclaw.ai/zh-CN/gateway/background-process.md](#httpsdocsopenclawaizh-cngatewaybackground-processmd)
- [远程 Gateway 网关设置 - OpenClaw](#gateway---openclaw)

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
openclaw secrets reload
openclaw logs --follow
openclaw doctor
```

`gateway status --deep` is for extra service discovery (LaunchDaemons/systemd system
units/schtasks), not a deeper RPC health probe.

## Multiple gateways (same host)

Most installs should run one gateway per machine. A single gateway can host multiple
agents and channels.You only need multiple gateways when you intentionally want isolation or a rescue bot.Useful checks:

```
openclaw gateway status --deep
openclaw gateway probe
```

What to expect:

- `gateway status --deep` can report `Other gateway-like services detected (best effort)`
and print cleanup hints when stale launchd/systemd/schtasks installs are still around.
- `gateway probe` can warn about `multiple reachable gateways` when more than one target
answers.
- If that is intentional, isolate ports, config/state, and workspace roots per gateway.

Checklist per instance:

- Unique `gateway.port`
- Unique `OPENCLAW_CONFIG_PATH`
- Unique `OPENCLAW_STATE_DIR`
- Unique `agents.defaults.workspace`

Example:

```
OPENCLAW_CONFIG_PATH=~/.openclaw/a.json OPENCLAW_STATE_DIR=~/.openclaw-a openclaw gateway --port 19001
OPENCLAW_CONFIG_PATH=~/.openclaw/b.json OPENCLAW_STATE_DIR=~/.openclaw-b openclaw gateway --port 19002
```

Detailed setup: [/gateway/multiple-gateways](https://docs.openclaw.ai/gateway/multiple-gateways).

## VoiceClaw real-time brain endpoint

OpenClaw exposes a VoiceClaw-compatible real-time WebSocket endpoint at
`/voiceclaw/realtime`. Use it when a VoiceClaw desktop client should talk
directly to a real-time OpenClaw brain instead of going through a separate relay
process.The endpoint uses Gemini Live for real-time audio and calls OpenClaw as the
brain by exposing OpenClaw tools directly to Gemini Live. Tool calls return an
immediate `working` result to keep the voice turn responsive, then OpenClaw
executes the actual tool asynchronously and injects the result back into the
live session. Set `GEMINI_API_KEY` in the gateway process environment. If
gateway auth is enabled, the desktop client sends the gateway token or password
in its first `session.config` message.Real-time brain access runs owner-authorized OpenClaw agent commands. Keep
`gateway.auth.mode: "none"` limited to loopback-only test instances. Non-local
real-time brain connections require gateway auth.For an isolated test gateway, run a separate instance with its own port, config,
and state:

```
OPENCLAW_CONFIG_PATH=/path/to/openclaw-realtime/openclaw.json \
OPENCLAW_STATE_DIR=/path/to/openclaw-realtime/state \
OPENCLAW_SKIP_CHANNELS=1 \
GEMINI_API_KEY=... \
openclaw gateway --port 19789
```

Then configure VoiceClaw to use:

```
ws://127.0.0.1:19789/voiceclaw/realtime
```

## Remote access

Preferred: Tailscale/VPN.
Fallback: SSH tunnel.

```
ssh -N -L 18789:127.0.0.1:18789 user@host
```

Then connect clients locally to `ws://127.0.0.1:18789`.

SSH tunnels do not bypass gateway auth. For shared-secret auth, clients still
must send `token`/`password` even over the tunnel. For identity-bearing modes,
the request still has to satisfy that auth path.

See: [Remote Gateway](https://docs.openclaw.ai/gateway/remote), [Authentication](https://docs.openclaw.ai/gateway/authentication), [Tailscale](https://docs.openclaw.ai/gateway/tailscale).

## Supervision and service lifecycle

Use supervised runs for production-like reliability.

- macOS (launchd)

- Linux (systemd user)

- Windows (native)

- Linux (system service)

```
openclaw gateway install
openclaw gateway status
openclaw gateway restart
openclaw gateway stop
```

Use `openclaw gateway restart` for restarts. Do not chain `openclaw gateway stop` and `openclaw gateway start`; on macOS, `gateway stop` intentionally disables the LaunchAgent before stopping it.LaunchAgent labels are `ai.openclaw.gateway` (default) or `ai.openclaw.<profile>` (named profile). `openclaw doctor` audits and repairs service config drift.

```
openclaw gateway install
systemctl --user enable --now openclaw-gateway[-<profile>].service
openclaw gateway status
```

For persistence after logout, enable lingering:

```
sudo loginctl enable-linger <user>
```

Manual user-unit example when you need a custom install path:

```
[Unit]
Description=OpenClaw Gateway
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/local/bin/openclaw gateway --port 18789
Restart=always
RestartSec=5
TimeoutStopSec=30
TimeoutStartSec=30
SuccessExitStatus=0 143
KillMode=control-group

[Install]
WantedBy=default.target
```

```
openclaw gateway install
openclaw gateway status --json
openclaw gateway restart
openclaw gateway stop
```

Native Windows managed startup uses a Scheduled Task named `OpenClaw Gateway`
(or `OpenClaw Gateway (<profile>)` for named profiles). If Scheduled Task
creation is denied, OpenClaw falls back to a per-user Startup-folder launcher
that points at `gateway.cmd` inside the state directory.

Use a system unit for multi-user/always-on hosts.

```
sudo systemctl daemon-reload
sudo systemctl enable --now openclaw-gateway[-<profile>].service
```

Use the same service body as the user unit, but install it under
`/etc/systemd/system/openclaw-gateway[-<profile>].service` and adjust
`ExecStart=` if your `openclaw` binary lives elsewhere.Do not also let `openclaw doctor --fix` install a user-level gateway service for the same profile/port. Doctor refuses that automatic install when it finds a system-level OpenClaw gateway service; use `OPENCLAW_SERVICE_REPAIR_POLICY=external` when the system unit owns the lifecycle.

## Dev profile quick path

```
openclaw --dev setup
openclaw --dev gateway --allow-unconfigured
openclaw --dev status
```

Defaults include isolated state/config and base gateway port `19001`.

## Protocol quick reference (operator view)

- First client frame must be `connect`.
- Gateway returns `hello-ok` snapshot (`presence`, `health`, `stateVersion`, `uptimeMs`, limits/policy).
- `hello-ok.features.methods` / `events` are a conservative discovery list, not
a generated dump of every callable helper route.
- Requests: `req(method, params)` → `res(ok/payload|error)`.
- Common events include `connect.challenge`, `agent`, `chat`,
`session.message`, `session.tool`, `sessions.changed`, `presence`, `tick`,
`health`, `heartbeat`, pairing/approval lifecycle events, and `shutdown`.

Agent runs are two-stage:

1. Immediate accepted ack (`status:"accepted"`)
2. Final completion response (`status:"ok"|"error"`), with streamed `agent` events in between.

See full protocol docs: [Gateway Protocol](https://docs.openclaw.ai/gateway/protocol).

## Operational checks

### Liveness

- Open WS and send `connect`.
- Expect `hello-ok` response with snapshot.

### Readiness

```
openclaw gateway status
openclaw channels status --probe
openclaw health
```

### Gap recovery

Events are not replayed. On sequence gaps, refresh state (`health`, `system-presence`) before continuing.

## Common failure signatures

| Signature | Likely issue |
| --- | --- |
| `refusing to bind gateway ... without auth` | Non-loopback bind without a valid gateway auth path |
| `another gateway instance is already listening` / `EADDRINUSE` | Port conflict |
| `Gateway start blocked: set gateway.mode=local` | Config set to remote mode, or local-mode stamp is missing from a damaged config |
| `unauthorized` during connect | Auth mismatch between client and gateway |

For full diagnosis ladders, use [Gateway Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting).

## Safety guarantees

- Gateway protocol clients fail fast when Gateway is unavailable (no implicit direct-channel fallback).
- Invalid/non-connect first frames are rejected and closed.
- Graceful shutdown emits `shutdown` event before socket close.

* * *

Related:

- [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)
- [Background Process](https://docs.openclaw.ai/gateway/background-process)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Health](https://docs.openclaw.ai/gateway/health)
- [Doctor](https://docs.openclaw.ai/gateway/doctor)
- [Authentication](https://docs.openclaw.ai/gateway/authentication)

## Related

- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)
- [Remote access](https://docs.openclaw.ai/gateway/remote)
- [Secrets management](https://docs.openclaw.ai/gateway/secrets)

[Configuration](https://docs.openclaw.ai/gateway/configuration)

Ctrl+I

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

Use `/model <alias-or-id>@<profileId>` to pin a specific provider credential for the current session (example profile ids: `anthropic:default`, `anthropic:work`).Use `/model` (or `/model list`) for a compact picker; use `/model status` for the full view (candidates + next auth profile, plus provider endpoint details when configured).

### Per-agent (CLI override)

Set an explicit auth profile order override for an agent (stored in that agent’s `auth-state.json`):

```
openclaw models auth order get --provider anthropic
openclaw models auth order set --provider anthropic anthropic:default
openclaw models auth order clear --provider anthropic
```

Use `--agent <id>` to target a specific agent; omit it to use the configured default agent.
When you debug order issues, `openclaw models status --probe` shows omitted
stored profiles as `excluded_by_auth_order` instead of silently skipping them.
When you debug cooldown issues, remember that rate-limit cooldowns can be tied
to one model id rather than the whole provider profile.

## Troubleshooting

### ”No credentials found”

If the Anthropic profile is missing, configure an Anthropic API key on the
**gateway host** or set up the Anthropic setup-token path, then re-check:

```
openclaw models status
```

### Token expiring/expired

Run `openclaw models status` to confirm which profile is expiring. If an
Anthropic token profile is missing or expired, refresh that setup via
setup-token or migrate to an Anthropic API key.

## Related

- [Secrets management](https://docs.openclaw.ai/gateway/secrets)
- [Remote access](https://docs.openclaw.ai/gateway/remote)
- [Auth storage](https://docs.openclaw.ai/concepts/oauth)

[Configuration examples](https://docs.openclaw.ai/gateway/configuration-examples) [Auth credential semantics](https://docs.openclaw.ai/auth-credential-semantics)

Ctrl+I

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
- `process list` includes a derived `name` (command verb + target) for quick scans.
- `process log` uses line-based `offset`/`limit`.
- When both `offset` and `limit` are omitted, it returns the last 200 lines and includes a paging hint.
- When `offset` is provided and `limit` is omitted, it returns from `offset` to the end (not capped to 200).
- Polling is for on-demand status, not wait-loop scheduling. If the work should
happen later, use cron instead.

## Examples

Run a long task and poll later:

```
{ "tool": "exec", "command": "sleep 5 && echo done", "yieldMs": 1000 }
```

```
{ "tool": "process", "action": "poll", "sessionId": "<id>" }
```

Start immediately in background:

```
{ "tool": "exec", "command": "npm run build", "background": true }
```

Send stdin:

```
{ "tool": "process", "action": "write", "sessionId": "<id>", "data": "y\n" }
```

Send PTY keys:

```
{ "tool": "process", "action": "send-keys", "sessionId": "<id>", "keys": ["C-c"] }
```

Submit current line:

```
{ "tool": "process", "action": "submit", "sessionId": "<id>" }
```

Paste literal text:

```
{ "tool": "process", "action": "paste", "sessionId": "<id>", "text": "line1\nline2\n" }
```

## Related

- [Exec tool](https://docs.openclaw.ai/tools/exec)
- [Exec approvals](https://docs.openclaw.ai/tools/exec-approvals)

[Gateway lock](https://docs.openclaw.ai/gateway/gateway-lock) [Multiple gateways](https://docs.openclaw.ai/gateway/multiple-gateways)

Ctrl+I

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
- iOS/Android nodes should treat discovery-based direct connects as **TLS-only** and require explicit user confirmation before trusting a first-time fingerprint.

## Debugging on macOS

Useful built‑in tools:

- Browse instances:

```
dns-sd -B _openclaw-gw._tcp local.
```

- Resolve one instance (replace `<instance>`):

```
dns-sd -L "<instance>" _openclaw-gw._tcp local.
```

If browsing works but resolving fails, you’re usually hitting a LAN policy or
mDNS resolver issue.

## Debugging in Gateway logs

The Gateway writes a rolling log file (printed on startup as
`gateway log file: ...`). Look for `bonjour:` lines, especially:

- `bonjour: advertise failed ...`
- `bonjour: suppressing ciao cancellation ...`
- `bonjour: ... name conflict resolved` / `hostname conflict resolved`
- `bonjour: watchdog detected non-announced service ...`
- `bonjour: disabling advertiser after ... failed restarts ...`

Bonjour uses the system hostname for the advertised `.local` host when it is a
valid DNS label. If the system hostname contains spaces, underscores, or another
invalid DNS-label character, OpenClaw falls back to `openclaw.local`. Set
`OPENCLAW_MDNS_HOSTNAME=<name>` before starting the Gateway when you need an
explicit host label.

## Debugging on iOS node

The iOS node uses `NWBrowser` to discover `_openclaw-gw._tcp`.To capture logs:

- Settings → Gateway → Advanced → **Discovery Debug Logs**
- Settings → Gateway → Advanced → **Discovery Logs** → reproduce → **Copy**

The log includes browser state transitions and result‑set changes.

## When to disable Bonjour

Disable Bonjour only when LAN multicast advertising is unavailable or harmful.
The common case is a Gateway running behind Docker bridge networking, WSL, or a
network policy that drops mDNS multicast. In those environments the Gateway is
still reachable through its published URL, SSH, Tailnet, or wide-area DNS-SD,
but LAN auto-discovery is not reliable.Prefer the existing environment override when the problem is deployment-scoped:

```
OPENCLAW_DISABLE_BONJOUR=1
```

That disables LAN multicast advertising without changing plugin configuration.
It is safe for Docker images, service files, launch scripts, and one-off
debugging because the setting disappears when the environment does.Use plugin configuration only when you intentionally want to turn off the
bundled LAN discovery plugin for that OpenClaw config:

```
openclaw plugins disable bonjour
```

## Docker gotchas

The bundled Bonjour plugin auto-disables LAN multicast advertising in detected
containers when `OPENCLAW_DISABLE_BONJOUR` is unset. Docker bridge networks
usually do not forward mDNS multicast (`224.0.0.251:5353`) between the container
and the LAN, so advertising from the container rarely makes discovery work.Important gotchas:

- Disabling Bonjour does not stop the Gateway. It only stops LAN multicast
advertising.
- Disabling Bonjour does not change `gateway.bind`; Docker still defaults to
`OPENCLAW_GATEWAY_BIND=lan` so the published host port can work.
- Disabling Bonjour does not disable wide-area DNS-SD. Use wide-area discovery
or Tailnet when the Gateway and node are not on the same LAN.
- Reusing the same `OPENCLAW_CONFIG_DIR` outside Docker does not persist the
container auto-disable policy.
- Set `OPENCLAW_DISABLE_BONJOUR=0` only for host networking, macvlan, or another
network where mDNS multicast is known to pass; set it to `1` to force-disable.

## Troubleshooting disabled Bonjour

If a node no longer auto-discovers the Gateway after Docker setup:

1. Confirm whether the Gateway is running in auto, forced-on, or forced-off mode:

```
docker compose config | grep OPENCLAW_DISABLE_BONJOUR
```

2. Confirm the Gateway itself is reachable through the published port:

```
curl -fsS http://127.0.0.1:18789/healthz
```

3. Use a direct target when Bonjour is disabled:   - Control UI or local tools: `http://127.0.0.1:18789`
   - LAN clients: `http://<gateway-host>:18789`
   - Cross-network clients: Tailnet MagicDNS, Tailnet IP, SSH tunnel, or
     wide-area DNS-SD
4. If you deliberately enabled Bonjour in Docker with
`OPENCLAW_DISABLE_BONJOUR=0`, test multicast from the host:

```
dns-sd -B _openclaw-gw._tcp local.
```

If browsing is empty or the Gateway logs show repeated ciao watchdog
cancellations, restore `OPENCLAW_DISABLE_BONJOUR=1` and use a direct or
Tailnet route.

## Common failure modes

- **Bonjour doesn’t cross networks**: use Tailnet or SSH.
- **Multicast blocked**: some Wi‑Fi networks disable mDNS.
- **Advertiser stuck in probing/announcing**: hosts with blocked multicast,
container bridges, WSL, or interface churn can leave the ciao advertiser in a
non-announced state. OpenClaw retries a few times and then disables Bonjour
for the current Gateway process instead of restarting the advertiser forever.
- **Docker bridge networking**: Bonjour auto-disables in detected containers.
Set `OPENCLAW_DISABLE_BONJOUR=0` only for host, macvlan, or another
mDNS-capable network.
- **Sleep / interface churn**: macOS may temporarily drop mDNS results; retry.
- **Browse works but resolve fails**: keep machine names simple (avoid emojis or
punctuation), then restart the Gateway. The service instance name derives from
the host name, so overly complex names can confuse some resolvers.

## Escaped instance names (`\032`)

Bonjour/DNS‑SD often escapes bytes in service instance names as decimal `\DDD`
sequences (e.g. spaces become `\032`).

- This is normal at the protocol level.
- UIs should decode for display (iOS uses `BonjourEscapes.decode`).

## Disabling / configuration

- `openclaw plugins disable bonjour` disables LAN multicast advertising by disabling the bundled plugin.
- `openclaw plugins enable bonjour` restores the default LAN discovery plugin.
- `OPENCLAW_DISABLE_BONJOUR=1` disables LAN multicast advertising without changing plugin config; accepted truthy values are `1`, `true`, `yes`, and `on` (legacy: `OPENCLAW_DISABLE_BONJOUR`).
- `OPENCLAW_DISABLE_BONJOUR=0` forces LAN multicast advertising on, including inside detected containers; accepted falsy values are `0`, `false`, `no`, and `off`.
- When `OPENCLAW_DISABLE_BONJOUR` is unset, Bonjour advertises on normal hosts and auto-disables inside detected containers.
- `gateway.bind` in `~/.openclaw/openclaw.json` controls the Gateway bind mode.
- `OPENCLAW_SSH_PORT` overrides the SSH port when `sshPort` is advertised (legacy: `OPENCLAW_SSH_PORT`).
- `OPENCLAW_TAILNET_DNS` publishes a MagicDNS hint in TXT when mDNS full mode is enabled (legacy: `OPENCLAW_TAILNET_DNS`).
- `OPENCLAW_CLI_PATH` overrides the advertised CLI path (legacy: `OPENCLAW_CLI_PATH`).

## Related docs

- Discovery policy and transport selection: [Discovery](https://docs.openclaw.ai/gateway/discovery)
- Node pairing + approvals: [Gateway pairing](https://docs.openclaw.ai/gateway/pairing)

[Discovery and transports](https://docs.openclaw.ai/gateway/discovery) [Remote access](https://docs.openclaw.ai/gateway/remote)

Ctrl+I

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
      { id: "locked-down", skills: [] }, // no skills\
    ],
  },
}
```

- Omit `agents.defaults.skills` for unrestricted skills by default.
- Omit `agents.list[].skills` to inherit the defaults.
- Set `agents.list[].skills: []` for no skills.
- A non-empty `agents.list[].skills` list is the final set for that agent; it
does not merge with defaults.

### `agents.defaults.skipBootstrap`

Disables automatic creation of workspace bootstrap files (`AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`, `BOOTSTRAP.md`).

```
{
  agents: { defaults: { skipBootstrap: true } },
}
```

### `agents.defaults.skipOptionalBootstrapFiles`

Skips creation of selected optional workspace files while still writing required bootstrap files. Valid values: `SOUL.md`, `USER.md`, `HEARTBEAT.md`, and `IDENTITY.md`.

```
{
  agents: {
    defaults: {
      skipOptionalBootstrapFiles: ["SOUL.md", "USER.md"],
    },
  },
}
```

### `agents.defaults.contextInjection`

Controls when workspace bootstrap files are injected into the system prompt. Default: `"always"`.

- `"continuation-skip"`: safe continuation turns (after a completed assistant response) skip workspace bootstrap re-injection, reducing prompt size. Heartbeat runs and post-compaction retries still rebuild context.
- `"never"`: disable workspace bootstrap and context-file injection on every turn. Use this only for agents that fully own their prompt lifecycle (custom context engines, native runtimes that build their own context, or specialized bootstrap-free workflows). Heartbeat and compaction-recovery turns also skip injection.

```
{
  agents: { defaults: { contextInjection: "continuation-skip" } },
}
```

### `agents.defaults.bootstrapMaxChars`

Max characters per workspace bootstrap file before truncation. Default: `12000`.

```
{
  agents: { defaults: { bootstrapMaxChars: 12000 } },
}
```

### `agents.defaults.bootstrapTotalMaxChars`

Max total characters injected across all workspace bootstrap files. Default: `60000`.

```
{
  agents: { defaults: { bootstrapTotalMaxChars: 60000 } },
}
```

### `agents.defaults.bootstrapPromptTruncationWarning`

Controls agent-visible warning text when bootstrap context is truncated.
Default: `"once"`.

- `"off"`: never inject warning text into the system prompt.
- `"once"`: inject warning once per unique truncation signature (recommended).
- `"always"`: inject warning on every run when truncation exists.

```
{
  agents: { defaults: { bootstrapPromptTruncationWarning: "once" } }, // off | once | always
}
```

### Context budget ownership map

OpenClaw has multiple high-volume prompt/context budgets, and they are
intentionally split by subsystem instead of all flowing through one generic
knob.

- `agents.defaults.bootstrapMaxChars` /
`agents.defaults.bootstrapTotalMaxChars`:
normal workspace bootstrap injection.
- `agents.defaults.startupContext.*`:
one-shot reset/startup model-run prelude, including recent daily
`memory/*.md` files. Bare chat `/new` and `/reset` commands are
acknowledged without invoking the model.
- `skills.limits.*`:
the compact skills list injected into the system prompt.
- `agents.defaults.contextLimits.*`:
bounded runtime excerpts and injected runtime-owned blocks.
- `memory.qmd.limits.*`:
indexed memory-search snippet and injection sizing.

Use the matching per-agent override only when one agent needs a different
budget:

- `agents.list[].skillsLimits.maxSkillsPromptChars`
- `agents.list[].contextLimits.*`

#### `agents.defaults.startupContext`

Controls the first-turn startup prelude injected on reset/startup model runs.
Bare chat `/new` and `/reset` commands acknowledge the reset without invoking
the model, so they do not load this prelude.

```
{
  agents: {
    defaults: {
      startupContext: {
        enabled: true,
        applyOn: ["new", "reset"],
        dailyMemoryDays: 2,
        maxFileBytes: 16384,
        maxFileChars: 1200,
        maxTotalChars: 2800,
      },
    },
  },
}
```

#### `agents.defaults.contextLimits`

Shared defaults for bounded runtime context surfaces.

```
{
  agents: {
    defaults: {
      contextLimits: {
        memoryGetMaxChars: 12000,
        memoryGetDefaultLines: 120,
        toolResultMaxChars: 16000,
        postCompactionMaxChars: 1800,
      },
    },
  },
}
```

- `memoryGetMaxChars`: default `memory_get` excerpt cap before truncation
metadata and continuation notice are added.
- `memoryGetDefaultLines`: default `memory_get` line window when `lines` is
omitted.
- `toolResultMaxChars`: live tool-result cap used for persisted results and
overflow recovery.
- `postCompactionMaxChars`: AGENTS.md excerpt cap used during post-compaction
refresh injection.

#### `agents.list[].contextLimits`

Per-agent override for the shared `contextLimits` knobs. Omitted fields inherit
from `agents.defaults.contextLimits`.

```
{
  agents: {
    defaults: {
      contextLimits: {
        memoryGetMaxChars: 12000,
        toolResultMaxChars: 16000,
      },
    },
    list: [\
      {\
        id: "tiny-local",\
        contextLimits: {\
          memoryGetMaxChars: 6000,\
          toolResultMaxChars: 8000,\
        },\
      },\
    ],
  },
}
```

#### `skills.limits.maxSkillsPromptChars`

Global cap for the compact skills list injected into the system prompt. This
does not affect reading `SKILL.md` files on demand.

```
{
  skills: {
    limits: {
      maxSkillsPromptChars: 18000,
    },
  },
}
```

#### `agents.list[].skillsLimits.maxSkillsPromptChars`

Per-agent override for the skills prompt budget.

```
{
  agents: {
    list: [\
      {\
        id: "tiny-local",\
        skillsLimits: {\
          maxSkillsPromptChars: 6000,\
        },\
      },\
    ],
  },
}
```

### `agents.defaults.imageMaxDimensionPx`

Max pixel size for the longest image side in transcript/tool image blocks before provider calls.
Default: `1200`.Lower values usually reduce vision-token usage and request payload size for screenshot-heavy runs.
Higher values preserve more visual detail.

```
{
  agents: { defaults: { imageMaxDimensionPx: 1200 } },
}
```

### `agents.defaults.userTimezone`

Timezone for system prompt context (not message timestamps). Falls back to host timezone.

```
{
  agents: { defaults: { userTimezone: "America/Chicago" } },
}
```

### `agents.defaults.timeFormat`

Time format in system prompt. Default: `auto` (OS preference).

```
{
  agents: { defaults: { timeFormat: "auto" } }, // auto | 12 | 24
}
```

### `agents.defaults.model`

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": { alias: "opus" },
        "minimax/MiniMax-M2.7": { alias: "minimax" },
      },
      model: {
        primary: "anthropic/claude-opus-4-6",
        fallbacks: ["minimax/MiniMax-M2.7"],
      },
      imageModel: {
        primary: "openrouter/qwen/qwen-2.5-vl-72b-instruct:free",
        fallbacks: ["openrouter/google/gemini-2.0-flash-vision:free"],
      },
      imageGenerationModel: {
        primary: "openai/gpt-image-2",
        fallbacks: ["google/gemini-3.1-flash-image-preview"],
      },
      videoGenerationModel: {
        primary: "qwen/wan2.6-t2v",
        fallbacks: ["qwen/wan2.6-i2v"],
      },
      pdfModel: {
        primary: "anthropic/claude-opus-4-6",
        fallbacks: ["openai/gpt-5.4-mini"],
      },
      params: { cacheRetention: "long" }, // global default provider params
      agentRuntime: {
        id: "pi", // pi | auto | registered harness id, e.g. codex
      },
      pdfMaxBytesMb: 10,
      pdfMaxPages: 20,
      thinkingDefault: "low",
      verboseDefault: "off",
      reasoningDefault: "off",
      elevatedDefault: "on",
      timeoutSeconds: 600,
      mediaMaxMb: 5,
      contextTokens: 200000,
      maxConcurrent: 3,
    },
  },
}
```

- `model`: accepts either a string (`"provider/model"`) or an object (`{ primary, fallbacks }`).

  - String form sets only the primary model.
  - Object form sets primary plus ordered failover models.
- `imageModel`: accepts either a string (`"provider/model"`) or an object (`{ primary, fallbacks }`).

  - Used by the `image` tool path as its vision-model config.
  - Also used as fallback routing when the selected/default model cannot accept image input.
  - Prefer explicit `provider/model` refs. Bare IDs are accepted for compatibility; if a bare ID uniquely matches a configured image-capable entry in `models.providers.*.models`, OpenClaw qualifies it to that provider. Ambiguous configured matches require an explicit provider prefix.
- `imageGenerationModel`: accepts either a string (`"provider/model"`) or an object (`{ primary, fallbacks }`).

  - Used by the shared image-generation capability and any future tool/plugin surface that generates images.
  - Typical values: `google/gemini-3.1-flash-image-preview` for native Gemini image generation, `fal/fal-ai/flux/dev` for fal, `openai/gpt-image-2` for OpenAI Images, or `openai/gpt-image-1.5` for transparent-background OpenAI PNG/WebP output.
  - If you select a provider/model directly, configure matching provider auth too (for example `GEMINI_API_KEY` or `GOOGLE_API_KEY` for `google/*`, `OPENAI_API_KEY` or OpenAI Codex OAuth for `openai/gpt-image-2` / `openai/gpt-image-1.5`, `FAL_KEY` for `fal/*`).
  - If omitted, `image_generate` can still infer an auth-backed provider default. It tries the current default provider first, then the remaining registered image-generation providers in provider-id order.
- `musicGenerationModel`: accepts either a string (`"provider/model"`) or an object (`{ primary, fallbacks }`).

  - Used by the shared music-generation capability and the built-in `music_generate` tool.
  - Typical values: `google/lyria-3-clip-preview`, `google/lyria-3-pro-preview`, or `minimax/music-2.6`.
  - If omitted, `music_generate` can still infer an auth-backed provider default. It tries the current default provider first, then the remaining registered music-generation providers in provider-id order.
  - If you select a provider/model directly, configure the matching provider auth/API key too.
- `videoGenerationModel`: accepts either a string (`"provider/model"`) or an object (`{ primary, fallbacks }`).

  - Used by the shared video-generation capability and the built-in `video_generate` tool.
  - Typical values: `qwen/wan2.6-t2v`, `qwen/wan2.6-i2v`, `qwen/wan2.6-r2v`, `qwen/wan2.6-r2v-flash`, or `qwen/wan2.7-r2v`.
  - If omitted, `video_generate` can still infer an auth-backed provider default. It tries the current default provider first, then the remaining registered video-generation providers in provider-id order.
  - If you select a provider/model directly, configure the matching provider auth/API key too.
  - The bundled Qwen video-generation provider supports up to 1 output video, 1 input image, 4 input videos, 10 seconds duration, and provider-level `size`, `aspectRatio`, `resolution`, `audio`, and `watermark` options.
- `pdfModel`: accepts either a string (`"provider/model"`) or an object (`{ primary, fallbacks }`).

  - Used by the `pdf` tool for model routing.
  - If omitted, the PDF tool falls back to `imageModel`, then to the resolved session/default model.
- `pdfMaxBytesMb`: default PDF size limit for the `pdf` tool when `maxBytesMb` is not passed at call time.
- `pdfMaxPages`: default maximum pages considered by extraction fallback mode in the `pdf` tool.
- `verboseDefault`: default verbose level for agents. Values: `"off"`, `"on"`, `"full"`. Default: `"off"`.
- `reasoningDefault`: default reasoning visibility for agents. Values: `"off"`, `"on"`, `"stream"`. Per-agent `agents.list[].reasoningDefault` overrides this default. Configured reasoning defaults are only applied for owners, authorized senders, or operator-admin gateway contexts when no per-message or session reasoning override is set.
- `elevatedDefault`: default elevated-output level for agents. Values: `"off"`, `"on"`, `"ask"`, `"full"`. Default: `"on"`.
- `model.primary`: format `provider/model` (e.g. `openai/gpt-5.5` for API-key access or `openai-codex/gpt-5.5` for Codex OAuth). If you omit the provider, OpenClaw tries an alias first, then a unique configured-provider match for that exact model id, and only then falls back to the configured default provider (deprecated compatibility behavior, so prefer explicit `provider/model`). If that provider no longer exposes the configured default model, OpenClaw falls back to the first configured provider/model instead of surfacing a stale removed-provider default.
- `models`: the configured model catalog and allowlist for `/model`. Each entry can include `alias` (shortcut) and `params` (provider-specific, for example `temperature`, `maxTokens`, `cacheRetention`, `context1m`, `responsesServerCompaction`, `responsesCompactThreshold`, `chat_template_kwargs`, `extra_body`/`extraBody`).

  - Safe edits: use `openclaw config set agents.defaults.models '<json>' --strict-json --merge` to add entries. `config set` refuses replacements that would remove existing allowlist entries unless you pass `--replace`.
  - Provider-scoped configure/onboarding flows merge selected provider models into this map and preserve unrelated providers already configured.
  - For direct OpenAI Responses models, server-side compaction is enabled automatically. Use `params.responsesServerCompaction: false` to stop injecting `context_management`, or `params.responsesCompactThreshold` to override the threshold. See [OpenAI server-side compaction](https://docs.openclaw.ai/providers/openai#server-side-compaction-responses-api).
- `params`: global default provider parameters applied to all models. Set at `agents.defaults.params` (e.g. `{ cacheRetention: "long" }`).
- `params` merge precedence (config): `agents.defaults.params` (global base) is overridden by `agents.defaults.models["provider/model"].params` (per-model), then `agents.list[].params` (matching agent id) overrides by key. See [Prompt Caching](https://docs.openclaw.ai/reference/prompt-caching) for details.
- `params.extra_body`/`params.extraBody`: advanced pass-through JSON merged into `api: "openai-completions"` request bodies for OpenAI-compatible proxies. If it collides with generated request keys, the extra body wins; non-native completions routes still strip OpenAI-only `store` afterward.
- `params.chat_template_kwargs`: vLLM/OpenAI-compatible chat-template arguments merged into top-level `api: "openai-completions"` request bodies. For `vllm/nemotron-3-*` with thinking off, the bundled vLLM plugin automatically sends `enable_thinking: false` and `force_nonempty_content: true`; explicit `chat_template_kwargs` override generated defaults, and `extra_body.chat_template_kwargs` still has final precedence. For vLLM Qwen thinking controls, set `params.qwenThinkingFormat` to `"chat-template"` or `"top-level"` on that model entry.
- `compat.supportedReasoningEfforts`: per-model OpenAI-compatible reasoning effort list. Include `"xhigh"` for custom endpoints that truly accept it; OpenClaw then exposes `/think xhigh` in command menus, Gateway session rows, session patch validation, agent CLI validation, and `llm-task` validation for that configured provider/model. Use `compat.reasoningEffortMap` when the backend wants a provider-specific value for a canonical level.
- `params.preserveThinking`: Z.AI-only opt-in for preserved thinking. When enabled and thinking is on, OpenClaw sends `thinking.clear_thinking: false` and replays prior `reasoning_content`; see [Z.AI thinking and preserved thinking](https://docs.openclaw.ai/providers/zai#thinking-and-preserved-thinking).
- `agentRuntime`: default low-level agent runtime policy. Omitted id defaults to OpenClaw Pi. Use `id: "pi"` to force the built-in PI harness, `id: "auto"` to let registered plugin harnesses claim supported models and use PI when none match, a registered harness id such as `id: "codex"` to require that harness, or a supported CLI backend alias such as `id: "claude-cli"`. Explicit plugin runtimes fail closed when the harness is unavailable or fails. Keep model refs canonical as `provider/model`; select Codex, Claude CLI, Gemini CLI, and other execution backends through runtime config instead of legacy runtime provider prefixes. See [Agent runtimes](https://docs.openclaw.ai/concepts/agent-runtimes) for how this differs from provider/model selection.
- Config writers that mutate these fields (for example `/models set`, `/models set-image`, and fallback add/remove commands) save canonical object form and preserve existing fallback lists when possible.
- `maxConcurrent`: max parallel agent runs across sessions (each session still serialized). Default: 4.

### `agents.defaults.agentRuntime`

`agentRuntime` controls which low-level executor runs agent turns. Most
deployments should keep the default OpenClaw Pi runtime. Use it when a trusted
plugin provides a native harness, such as the bundled Codex app-server harness,
or when you want a supported CLI backend such as Claude CLI. For the mental
model, see [Agent runtimes](https://docs.openclaw.ai/concepts/agent-runtimes).

```
{
  agents: {
    defaults: {
      model: "openai/gpt-5.5",
      agentRuntime: {
        id: "codex",
      },
    },
  },
}
```

- `id`: `"auto"`, `"pi"`, a registered plugin harness id, or a supported CLI backend alias. The bundled Codex plugin registers `codex`; the bundled Anthropic plugin provides the `claude-cli` CLI backend.
- `id: "auto"` lets registered plugin harnesses claim supported turns and uses PI when no harness matches. An explicit plugin runtime such as `id: "codex"` requires that harness and fails closed if it is unavailable or fails.
- Environment override: `OPENCLAW_AGENT_RUNTIME=<id|auto|pi>` overrides `id` for that process.
- For Codex-only deployments, set `model: "openai/gpt-5.5"` and `agentRuntime.id: "codex"`.
- For Claude CLI deployments, prefer `model: "anthropic/claude-opus-4-7"` plus `agentRuntime.id: "claude-cli"`. Legacy `claude-cli/claude-opus-4-7` model refs still work for compatibility, but new config should keep provider/model selection canonical and put the execution backend in `agentRuntime.id`.
- Older runtime-policy keys are rewritten to `agentRuntime` by `openclaw doctor --fix`.
- Harness choice is pinned per session id after the first embedded run. Config/env changes affect new or reset sessions, not an existing transcript. Legacy sessions with transcript history but no recorded pin are treated as PI-pinned. `/status` reports the effective runtime, for example `Runtime: OpenClaw Pi Default` or `Runtime: OpenAI Codex`.
- This only controls text agent-turn execution. Media generation, vision, PDF, music, video, and TTS still use their provider/model settings.

**Built-in alias shorthands** (only apply when the model is in `agents.defaults.models`):

| Alias | Model |
| --- | --- |
| `opus` | `anthropic/claude-opus-4-6` |
| `sonnet` | `anthropic/claude-sonnet-4-6` |
| `gpt` | `openai/gpt-5.5` or `openai-codex/gpt-5.5` |
| `gpt-mini` | `openai/gpt-5.4-mini` |
| `gpt-nano` | `openai/gpt-5.4-nano` |
| `gemini` | `google/gemini-3.1-pro-preview` |
| `gemini-flash` | `google/gemini-3-flash-preview` |
| `gemini-flash-lite` | `google/gemini-3.1-flash-lite-preview` |

Your configured aliases always win over defaults.Z.AI GLM-4.x models automatically enable thinking mode unless you set `--thinking off` or define `agents.defaults.models["zai/<model>"].params.thinking` yourself.
Z.AI models enable `tool_stream` by default for tool call streaming. Set `agents.defaults.models["zai/<model>"].params.tool_stream` to `false` to disable it.
Anthropic Claude 4.6 models default to `adaptive` thinking when no explicit thinking level is set.

### `agents.defaults.cliBackends`

Optional CLI backends for text-only fallback runs (no tool calls). Useful as a backup when API providers fail.

```
{
  agents: {
    defaults: {
      cliBackends: {
        "codex-cli": {
          command: "/opt/homebrew/bin/codex",
        },
        "my-cli": {
          command: "my-cli",
          args: ["--json"],
          output: "json",
          modelArg: "--model",
          sessionArg: "--session",
          sessionMode: "existing",
          systemPromptArg: "--system",
          // Or use systemPromptFileArg when the CLI accepts a prompt file flag.
          systemPromptWhen: "first",
          imageArg: "--image",
          imageMode: "repeat",
        },
      },
    },
  },
}
```

- CLI backends are text-first; tools are always disabled.
- Sessions supported when `sessionArg` is set.
- Image pass-through supported when `imageArg` accepts file paths.

### `agents.defaults.systemPromptOverride`

Replace the entire OpenClaw-assembled system prompt with a fixed string. Set at the default level (`agents.defaults.systemPromptOverride`) or per agent (`agents.list[].systemPromptOverride`). Per-agent values take precedence; an empty or whitespace-only value is ignored. Useful for controlled prompt experiments.

```
{
  agents: {
    defaults: {
      systemPromptOverride: "You are a helpful assistant.",
    },
  },
}
```

### `agents.defaults.promptOverlays`

Provider-independent prompt overlays applied by model family. GPT-5-family model ids receive the shared behavior contract across providers; `personality` controls only the friendly interaction-style layer.

```
{
  agents: {
    defaults: {
      promptOverlays: {
        gpt5: {
          personality: "friendly", // friendly | on | off
        },
      },
    },
  },
}
```

- `"friendly"` (default) and `"on"` enable the friendly interaction-style layer.
- `"off"` disables only the friendly layer; the tagged GPT-5 behavior contract remains enabled.
- Legacy `plugins.entries.openai.config.personality` is still read when this shared setting is unset.

### `agents.defaults.heartbeat`

Periodic heartbeat runs.

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m", // 0m disables
        model: "openai/gpt-5.4-mini",
        includeReasoning: false,
        includeSystemPromptSection: true, // default: true; false omits the Heartbeat section from the system prompt
        lightContext: false, // default: false; true keeps only HEARTBEAT.md from workspace bootstrap files
        isolatedSession: false, // default: false; true runs each heartbeat in a fresh session (no conversation history)
        skipWhenBusy: false, // default: false; true also waits for subagent/nested lanes
        session: "main",
        to: "+15555550123",
        directPolicy: "allow", // allow (default) | block
        target: "none", // default: none | options: last | whatsapp | telegram | discord | ...
        prompt: "Read HEARTBEAT.md if it exists...",
        ackMaxChars: 300,
        suppressToolErrorWarnings: false,
        timeoutSeconds: 45,
      },
    },
  },
}
```

- `every`: duration string (ms/s/m/h). Default: `30m` (API-key auth) or `1h` (OAuth auth). Set to `0m` to disable.
- `includeSystemPromptSection`: when false, omits the Heartbeat section from the system prompt and skips `HEARTBEAT.md` injection into bootstrap context. Default: `true`.
- `suppressToolErrorWarnings`: when true, suppresses tool error warning payloads during heartbeat runs.
- `timeoutSeconds`: maximum time in seconds allowed for a heartbeat agent turn before it is aborted. Leave unset to use `agents.defaults.timeoutSeconds`.
- `directPolicy`: direct/DM delivery policy. `allow` (default) permits direct-target delivery. `block` suppresses direct-target delivery and emits `reason=dm-blocked`.
- `lightContext`: when true, heartbeat runs use lightweight bootstrap context and keep only `HEARTBEAT.md` from workspace bootstrap files.
- `isolatedSession`: when true, each heartbeat runs in a fresh session with no prior conversation history. Same isolation pattern as cron `sessionTarget: "isolated"`. Reduces per-heartbeat token cost from ~100K to ~2-5K tokens.
- `skipWhenBusy`: when true, heartbeat runs defer on extra busy lanes: subagent or nested command work. Cron lanes always defer heartbeats, even without this flag.
- Per-agent: set `agents.list[].heartbeat`. When any agent defines `heartbeat`, **only those agents** run heartbeats.
- Heartbeats run full agent turns — shorter intervals burn more tokens.

### `agents.defaults.compaction`

```
{
  agents: {
    defaults: {
      compaction: {
        mode: "safeguard", // default | safeguard
        provider: "my-provider", // id of a registered compaction provider plugin (optional)
        timeoutSeconds: 900,
        reserveTokensFloor: 24000,
        keepRecentTokens: 50000,
        identifierPolicy: "strict", // strict | off | custom
        identifierInstructions: "Preserve deployment IDs, ticket IDs, and host:port pairs exactly.", // used when identifierPolicy=custom
        qualityGuard: { enabled: true, maxRetries: 1 },
        midTurnPrecheck: { enabled: false }, // optional Pi tool-loop pressure check
        postCompactionSections: ["Session Startup", "Red Lines"], // [] disables reinjection
        model: "openrouter/anthropic/claude-sonnet-4-6", // optional compaction-only model override
        truncateAfterCompaction: true, // rotate to a smaller successor JSONL after compaction
        maxActiveTranscriptBytes: "20mb", // optional preflight local compaction trigger
        notifyUser: true, // send brief notices when compaction starts and completes (default: false)
        memoryFlush: {
          enabled: true,
          model: "ollama/qwen3:8b", // optional memory-flush-only model override
          softThresholdTokens: 6000,
          systemPrompt: "Session nearing compaction. Store durable memories now.",
          prompt: "Write any lasting notes to memory/YYYY-MM-DD.md; reply with the exact silent token NO_REPLY if nothing to store.",
        },
      },
    },
  },
}
```

- `mode`: `default` or `safeguard` (chunked summarization for long histories). See [Compaction](https://docs.openclaw.ai/concepts/compaction).
- `provider`: id of a registered compaction provider plugin. When set, the provider’s `summarize()` is called instead of built-in LLM summarization. Falls back to built-in on failure. Setting a provider forces `mode: "safeguard"`. See [Compaction](https://docs.openclaw.ai/concepts/compaction).
- `timeoutSeconds`: maximum seconds allowed for a single compaction operation before OpenClaw aborts it. Default: `900`.
- `keepRecentTokens`: Pi cut-point budget for keeping the most recent transcript tail verbatim. Manual `/compact` honors this when explicitly set; otherwise manual compaction is a hard checkpoint.
- `identifierPolicy`: `strict` (default), `off`, or `custom`. `strict` prepends built-in opaque identifier retention guidance during compaction summarization.
- `identifierInstructions`: optional custom identifier-preservation text used when `identifierPolicy=custom`.
- `qualityGuard`: retry-on-malformed-output checks for safeguard summaries. Enabled by default in safeguard mode; set `enabled: false` to skip the audit.
- `midTurnPrecheck`: optional Pi tool-loop pressure check. When `enabled: true`, OpenClaw checks context pressure after tool results are appended and before the next model call. If the context no longer fits, it aborts the current attempt before submitting the prompt and reuses the existing precheck recovery path to truncate tool results or compact and retry. Works with both `default` and `safeguard` compaction modes. Default: disabled.
- `postCompactionSections`: optional AGENTS.md H2/H3 section names to re-inject after compaction. Defaults to `["Session Startup", "Red Lines"]`; set `[]` to disable reinjection. When unset or explicitly set to that default pair, older `Every Session`/`Safety` headings are also accepted as a legacy fallback.
- `model`: optional `provider/model-id` override for compaction summarization only. Use this when the main session should keep one model but compaction summaries should run on another; when unset, compaction uses the session’s primary model.
- `maxActiveTranscriptBytes`: optional byte threshold (`number` or strings like `"20mb"`) that triggers normal local compaction before a run when the active JSONL grows past the threshold. Requires `truncateAfterCompaction` so successful compaction can rotate to a smaller successor transcript. Disabled when unset or `0`.
- `notifyUser`: when `true`, sends brief notices to the user when compaction starts and when it completes (for example, “Compacting context…” and “Compaction complete”). Disabled by default to keep compaction silent.
- `memoryFlush`: silent agentic turn before auto-compaction to store durable memories. Set `model` to an exact provider/model such as `ollama/qwen3:8b` when this housekeeping turn should stay on a local model; the override does not inherit the active session fallback chain. Skipped when workspace is read-only.

### `agents.defaults.contextPruning`

Prunes **old tool results** from in-memory context before sending to the LLM. Does **not** modify session history on disk.

```
{
  agents: {
    defaults: {
      contextPruning: {
        mode: "cache-ttl", // off | cache-ttl
        ttl: "1h", // duration (ms/s/m/h), default unit: minutes
        keepLastAssistants: 3,
        softTrimRatio: 0.3,
        hardClearRatio: 0.5,
        minPrunableToolChars: 50000,
        softTrim: { maxChars: 4000, headChars: 1500, tailChars: 1500 },
        hardClear: { enabled: true, placeholder: "[Old tool result content cleared]" },
        tools: { deny: ["browser", "canvas"] },
      },
    },
  },
}
```

cache-ttl mode behavior

- `mode: "cache-ttl"` enables pruning passes.
- `ttl` controls how often pruning can run again (after the last cache touch).
- Pruning soft-trims oversized tool results first, then hard-clears older tool results if needed.

**Soft-trim** keeps beginning + end and inserts `...` in the middle.**Hard-clear** replaces the entire tool result with the placeholder.Notes:

- Image blocks are never trimmed/cleared.
- Ratios are character-based (approximate), not exact token counts.
- If fewer than `keepLastAssistants` assistant messages exist, pruning is skipped.

See [Session Pruning](https://docs.openclaw.ai/concepts/session-pruning) for behavior details.

### Block streaming

```
{
  agents: {
    defaults: {
      blockStreamingDefault: "off", // on | off
      blockStreamingBreak: "text_end", // text_end | message_end
      blockStreamingChunk: { minChars: 800, maxChars: 1200 },
      blockStreamingCoalesce: { idleMs: 1000 },
      humanDelay: { mode: "natural" }, // off | natural | custom (use minMs/maxMs)
    },
  },
}
```

- Non-Telegram channels require explicit `*.blockStreaming: true` to enable block replies.
- Channel overrides: `channels.<channel>.blockStreamingCoalesce` (and per-account variants). Signal/Slack/Discord/Google Chat default `minChars: 1500`.
- `humanDelay`: randomized pause between block replies. `natural` = 800–2500ms. Per-agent override: `agents.list[].humanDelay`.

See [Streaming](https://docs.openclaw.ai/concepts/streaming) for behavior + chunking details.

### Typing indicators

```
{
  agents: {
    defaults: {
      typingMode: "instant", // never | instant | thinking | message
      typingIntervalSeconds: 6,
    },
  },
}
```

- Defaults: `instant` for direct chats/mentions, `message` for unmentioned group chats.
- Per-session overrides: `session.typingMode`, `session.typingIntervalSeconds`.

See [Typing Indicators](https://docs.openclaw.ai/concepts/typing-indicators).

### `agents.defaults.sandbox`

Optional sandboxing for the embedded agent. See [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing) for the full guide.

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main", // off | non-main | all
        backend: "docker", // docker | ssh | openshell
        scope: "agent", // session | agent | shared
        workspaceAccess: "none", // none | ro | rw
        workspaceRoot: "~/.openclaw/sandboxes",
        docker: {
          image: "openclaw-sandbox:bookworm-slim",
          containerPrefix: "openclaw-sbx-",
          workdir: "/workspace",
          readOnlyRoot: true,
          tmpfs: ["/tmp", "/var/tmp", "/run"],
          network: "none",
          user: "1000:1000",
          capDrop: ["ALL"],
          env: { LANG: "C.UTF-8" },
          setupCommand: "apt-get update && apt-get install -y git curl jq",
          pidsLimit: 256,
          memory: "1g",
          memorySwap: "2g",
          cpus: 1,
          ulimits: {
            nofile: { soft: 1024, hard: 2048 },
            nproc: 256,
          },
          seccompProfile: "/path/to/seccomp.json",
          apparmorProfile: "openclaw-sandbox",
          dns: ["1.1.1.1", "8.8.8.8"],
          extraHosts: ["internal.service:10.0.0.5"],
          binds: ["/home/user/source:/source:rw"],
        },
        ssh: {
          target: "user@gateway-host:22",
          command: "ssh",
          workspaceRoot: "/tmp/openclaw-sandboxes",
          strictHostKeyChecking: true,
          updateHostKeys: true,
          identityFile: "~/.ssh/id_ed25519",
          certificateFile: "~/.ssh/id_ed25519-cert.pub",
          knownHostsFile: "~/.ssh/known_hosts",
          // SecretRefs / inline contents also supported:
          // identityData: { source: "env", provider: "default", id: "SSH_IDENTITY" },
          // certificateData: { source: "env", provider: "default", id: "SSH_CERTIFICATE" },
          // knownHostsData: { source: "env", provider: "default", id: "SSH_KNOWN_HOSTS" },
        },
        browser: {
          enabled: false,
          image: "openclaw-sandbox-browser:bookworm-slim",
          network: "openclaw-sandbox-browser",
          cdpPort: 9222,
          cdpSourceRange: "172.21.0.1/32",
          vncPort: 5900,
          noVncPort: 6080,
          headless: false,
          enableNoVnc: true,
          allowHostControl: false,
          autoStart: true,
          autoStartTimeoutMs: 12000,
        },
        prune: {
          idleHours: 24,
          maxAgeDays: 7,
        },
      },
    },
  },
  tools: {
    sandbox: {
      tools: {
        allow: [\
          "exec",\
          "process",\
          "read",\
          "write",\
          "edit",\
          "apply_patch",\
          "sessions_list",\
          "sessions_history",\
          "sessions_send",\
          "sessions_spawn",\
          "session_status",\
        ],
        deny: ["browser", "canvas", "nodes", "cron", "discord", "gateway"],
      },
    },
  },
}
```

Sandbox details

**Backend:**

- `docker`: local Docker runtime (default)
- `ssh`: generic SSH-backed remote runtime
- `openshell`: OpenShell runtime

When `backend: "openshell"` is selected, runtime-specific settings move to
`plugins.entries.openshell.config`.**SSH backend config:**

- `target`: SSH target in `user@host[:port]` form
- `command`: SSH client command (default: `ssh`)
- `workspaceRoot`: absolute remote root used for per-scope workspaces
- `identityFile` / `certificateFile` / `knownHostsFile`: existing local files passed to OpenSSH
- `identityData` / `certificateData` / `knownHostsData`: inline contents or SecretRefs that OpenClaw materializes into temp files at runtime
- `strictHostKeyChecking` / `updateHostKeys`: OpenSSH host-key policy knobs

**SSH auth precedence:**

- `identityData` wins over `identityFile`
- `certificateData` wins over `certificateFile`
- `knownHostsData` wins over `knownHostsFile`
- SecretRef-backed `*Data` values are resolved from the active secrets runtime snapshot before the sandbox session starts

**SSH backend behavior:**

- seeds the remote workspace once after create or recreate
- then keeps the remote SSH workspace canonical
- routes `exec`, file tools, and media paths over SSH
- does not sync remote changes back to the host automatically
- does not support sandbox browser containers

**Workspace access:**

- `none`: per-scope sandbox workspace under `~/.openclaw/sandboxes`
- `ro`: sandbox workspace at `/workspace`, agent workspace mounted read-only at `/agent`
- `rw`: agent workspace mounted read/write at `/workspace`

**Scope:**

- `session`: per-session container + workspace
- `agent`: one container + workspace per agent (default)
- `shared`: shared container and workspace (no cross-session isolation)

**OpenShell plugin config:**

```
{
  plugins: {
    entries: {
      openshell: {
        enabled: true,
        config: {
          mode: "mirror", // mirror | remote
          from: "openclaw",
          remoteWorkspaceDir: "/sandbox",
          remoteAgentWorkspaceDir: "/agent",
          gateway: "lab", // optional
          gatewayEndpoint: "https://lab.example", // optional
          policy: "strict", // optional OpenShell policy id
          providers: ["openai"], // optional
          autoProviders: true,
          timeoutSeconds: 120,
        },
      },
    },
  },
}
```

**OpenShell mode:**

- `mirror`: seed remote from local before exec, sync back after exec; local workspace stays canonical
- `remote`: seed remote once when the sandbox is created, then keep the remote workspace canonical

In `remote` mode, host-local edits made outside OpenClaw are not synced into the sandbox automatically after the seed step.
Transport is SSH into the OpenShell sandbox, but the plugin owns sandbox lifecycle and optional mirror sync.**`setupCommand`** runs once after container creation (via `sh -lc`). Needs network egress, writable root, root user.**Containers default to `network: "none"`** — set to `"bridge"` (or a custom bridge network) if the agent needs outbound access.
`"host"` is blocked. `"container:<id>"` is blocked by default unless you explicitly set
`sandbox.docker.dangerouslyAllowContainerNamespaceJoin: true` (break-glass).**Inbound attachments** are staged into `media/inbound/*` in the active workspace.**`docker.binds`** mounts additional host directories; global and per-agent binds are merged.**Sandboxed browser** (`sandbox.browser.enabled`): Chromium + CDP in a container. noVNC URL injected into system prompt. Does not require `browser.enabled` in `openclaw.json`.
noVNC observer access uses VNC auth by default and OpenClaw emits a short-lived token URL (instead of exposing the password in the shared URL).

- `allowHostControl: false` (default) blocks sandboxed sessions from targeting the host browser.
- `network` defaults to `openclaw-sandbox-browser` (dedicated bridge network). Set to `bridge` only when you explicitly want global bridge connectivity.
- `cdpSourceRange` optionally restricts CDP ingress at the container edge to a CIDR range (for example `172.21.0.1/32`).
- `sandbox.browser.binds` mounts additional host directories into the sandbox browser container only. When set (including `[]`), it replaces `docker.binds` for the browser container.
- Launch defaults are defined in `scripts/sandbox-browser-entrypoint.sh` and tuned for container hosts:

  - `--remote-debugging-address=127.0.0.1`
  - `--remote-debugging-port=<derived from OPENCLAW_BROWSER_CDP_PORT>`
  - `--user-data-dir=${HOME}/.chrome`
  - `--no-first-run`
  - `--no-default-browser-check`
  - `--disable-3d-apis`
  - `--disable-gpu`
  - `--disable-software-rasterizer`
  - `--disable-dev-shm-usage`
  - `--disable-background-networking`
  - `--disable-features=TranslateUI`
  - `--disable-breakpad`
  - `--disable-crash-reporter`
  - `--renderer-process-limit=2`
  - `--no-zygote`
  - `--metrics-recording-only`
  - `--disable-extensions` (default enabled)
  - `--disable-3d-apis`, `--disable-software-rasterizer`, and `--disable-gpu` are
    enabled by default and can be disabled with
    `OPENCLAW_BROWSER_DISABLE_GRAPHICS_FLAGS=0` if WebGL/3D usage requires it.
  - `OPENCLAW_BROWSER_DISABLE_EXTENSIONS=0` re-enables extensions if your workflow
    depends on them.
  - `--renderer-process-limit=2` can be changed with
    `OPENCLAW_BROWSER_RENDERER_PROCESS_LIMIT=<N>`; set `0` to use Chromium’s
    default process limit.
  - plus `--no-sandbox` when `noSandbox` is enabled.
  - Defaults are the container image baseline; use a custom browser image with a custom
    entrypoint to change container defaults.

Browser sandboxing and `sandbox.docker.binds` are Docker-only.Build images (from a source checkout):

```
scripts/sandbox-setup.sh           # main sandbox image
scripts/sandbox-browser-setup.sh   # optional browser image
```

For npm installs without a source checkout, see [Sandboxing § Images and setup](https://docs.openclaw.ai/gateway/sandboxing#images-and-setup) for inline `docker build` commands.

### `agents.list` (per-agent overrides)

Use `agents.list[].tts` to give an agent its own TTS provider, voice, model,
style, or auto-TTS mode. The agent block deep-merges over global
`messages.tts`, so shared credentials can stay in one place while individual
agents override only the voice or provider fields they need. The active agent’s
override applies to automatic spoken replies, `/tts audio`, `/tts status`, and
the `tts` agent tool. See [Text-to-speech](https://docs.openclaw.ai/tools/tts#per-agent-voice-overrides)
for provider examples and precedence.

```
{
  agents: {
    list: [\
      {\
        id: "main",\
        default: true,\
        name: "Main Agent",\
        workspace: "~/.openclaw/workspace",\
        agentDir: "~/.openclaw/agents/main/agent",\
        model: "anthropic/claude-opus-4-6", // or { primary, fallbacks }\
        thinkingDefault: "high", // per-agent thinking level override\
        reasoningDefault: "on", // per-agent reasoning visibility override\
        fastModeDefault: false, // per-agent fast mode override\
        agentRuntime: { id: "auto" },\
        params: { cacheRetention: "none" }, // overrides matching defaults.models params by key\
        tts: {\
          providers: {\
            elevenlabs: { voiceId: "EXAVITQu4vr4xnSDxMaL" },\
          },\
        },\
        skills: ["docs-search"], // replaces agents.defaults.skills when set\
        identity: {\
          name: "Samantha",\
          theme: "helpful sloth",\
          emoji: "🦥",\
          avatar: "avatars/samantha.png",\
        },\
        groupChat: { mentionPatterns: ["@openclaw"] },\
        sandbox: { mode: "off" },\
        runtime: {\
          type: "acp",\
          acp: {\
            agent: "codex",\
            backend: "acpx",\
            mode: "persistent",\
            cwd: "/workspace/openclaw",\
          },\
        },\
        subagents: { allowAgents: ["*"] },\
        tools: {\
          profile: "coding",\
          allow: ["browser"],\
          deny: ["canvas"],\
          elevated: { enabled: true },\
        },\
      },\
    ],
  },
}
```

- `id`: stable agent id (required).
- `default`: when multiple are set, first wins (warning logged). If none set, first list entry is default.
- `model`: string form sets a strict per-agent primary with no model fallback; object form `{ primary }` is also strict unless you add `fallbacks`. Use `{ primary, fallbacks: [...] }` to opt that agent into fallback, or `{ primary, fallbacks: [] }` to make strict behavior explicit. Cron jobs that only override `primary` still inherit default fallbacks unless you set `fallbacks: []`.
- `params`: per-agent stream params merged over the selected model entry in `agents.defaults.models`. Use this for agent-specific overrides like `cacheRetention`, `temperature`, or `maxTokens` without duplicating the whole model catalog.
- `tts`: optional per-agent text-to-speech overrides. The block deep-merges over `messages.tts`, so keep shared provider credentials and fallback policy in `messages.tts` and set only persona-specific values such as provider, voice, model, style, or auto mode here.
- `skills`: optional per-agent skill allowlist. If omitted, the agent inherits `agents.defaults.skills` when set; an explicit list replaces defaults instead of merging, and `[]` means no skills.
- `thinkingDefault`: optional per-agent default thinking level (`off | minimal | low | medium | high | xhigh | adaptive | max`). Overrides `agents.defaults.thinkingDefault` for this agent when no per-message or session override is set. The selected provider/model profile controls which values are valid; for Google Gemini, `adaptive` keeps provider-owned dynamic thinking (`thinkingLevel` omitted on Gemini 3/3.1, `thinkingBudget: -1` on Gemini 2.5).
- `reasoningDefault`: optional per-agent default reasoning visibility (`on | off | stream`). Overrides `agents.defaults.reasoningDefault` for this agent when no per-message or session reasoning override is set.
- `fastModeDefault`: optional per-agent default for fast mode (`true | false`). Applies when no per-message or session fast-mode override is set.
- `agentRuntime`: optional per-agent low-level runtime policy override. Use `{ id: "codex" }` to make one agent Codex-only while other agents keep the default PI fallback in `auto` mode.
- `runtime`: optional per-agent runtime descriptor. Use `type: "acp"` with `runtime.acp` defaults (`agent`, `backend`, `mode`, `cwd`) when the agent should default to ACP harness sessions.
- `identity.avatar`: workspace-relative path, `http(s)` URL, or `data:` URI.
- `identity` derives defaults: `ackReaction` from `emoji`, `mentionPatterns` from `name`/`emoji`.
- `subagents.allowAgents`: allowlist of agent ids for explicit `sessions_spawn.agentId` targets (`["*"]` = any; default: same agent only). Include the requester id when self-targeted `agentId` calls should be allowed.
- Sandbox inheritance guard: if the requester session is sandboxed, `sessions_spawn` rejects targets that would run unsandboxed.
- `subagents.requireAgentId`: when true, block `sessions_spawn` calls that omit `agentId` (forces explicit profile selection; default: false).

* * *

## Multi-agent routing

Run multiple isolated agents inside one Gateway. See [Multi-Agent](https://docs.openclaw.ai/concepts/multi-agent).

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

### Binding match fields

- `type` (optional): `route` for normal routing (missing type defaults to route), `acp` for persistent ACP conversation bindings.
- `match.channel` (required)
- `match.accountId` (optional; `*` = any account; omitted = default account)
- `match.peer` (optional; `{ kind: direct|group|channel, id }`)
- `match.guildId` / `match.teamId` (optional; channel-specific)
- `acp` (optional; only for `type: "acp"`): `{ mode, label, cwd, backend }`

**Deterministic match order:**

1. `match.peer`
2. `match.guildId`
3. `match.teamId`
4. `match.accountId` (exact, no peer/guild/team)
5. `match.accountId: "*"` (channel-wide)
6. Default agent

Within each tier, the first matching `bindings` entry wins.For `type: "acp"` entries, OpenClaw resolves by exact conversation identity (`match.channel` \+ account + `match.peer.id`) and does not use the route binding tier order above.

### Per-agent access profiles

Full access (no sandbox)

```
{
  agents: {
    list: [\
      {\
        id: "personal",\
        workspace: "~/.openclaw/workspace-personal",\
        sandbox: { mode: "off" },\
      },\
    ],
  },
}
```

Read-only tools + workspace

```
{
  agents: {
    list: [\
      {\
        id: "family",\
        workspace: "~/.openclaw/workspace-family",\
        sandbox: { mode: "all", scope: "agent", workspaceAccess: "ro" },\
        tools: {\
          allow: [\
            "read",\
            "sessions_list",\
            "sessions_history",\
            "sessions_send",\
            "sessions_spawn",\
            "session_status",\
          ],\
          deny: ["write", "edit", "apply_patch", "exec", "process", "browser"],\
        },\
      },\
    ],
  },
}
```

No filesystem access (messaging only)

```
{
  agents: {
    list: [\
      {\
        id: "public",\
        workspace: "~/.openclaw/workspace-public",\
        sandbox: { mode: "all", scope: "agent", workspaceAccess: "none" },\
        tools: {\
          allow: [\
            "sessions_list",\
            "sessions_history",\
            "sessions_send",\
            "sessions_spawn",\
            "session_status",\
            "whatsapp",\
            "telegram",\
            "slack",\
            "discord",\
            "gateway",\
          ],\
          deny: [\
            "read",\
            "write",\
            "edit",\
            "apply_patch",\
            "exec",\
            "process",\
            "browser",\
            "canvas",\
            "nodes",\
            "cron",\
            "gateway",\
            "image",\
          ],\
        },\
      },\
    ],
  },
}
```

See [Multi-Agent Sandbox & Tools](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools) for precedence details.

* * *

## Session

```
{
  session: {
    scope: "per-sender",
    dmScope: "main", // main | per-peer | per-channel-peer | per-account-channel-peer
    identityLinks: {
      alice: ["telegram:123456789", "discord:987654321012345678"],
    },
    reset: {
      mode: "daily", // daily | idle
      atHour: 4,
      idleMinutes: 60,
    },
    resetByType: {
      thread: { mode: "daily", atHour: 4 },
      direct: { mode: "idle", idleMinutes: 240 },
      group: { mode: "idle", idleMinutes: 120 },
    },
    resetTriggers: ["/new", "/reset"],
    store: "~/.openclaw/agents/{agentId}/sessions/sessions.json",
    maintenance: {
      mode: "warn", // warn | enforce
      pruneAfter: "30d",
      maxEntries: 500,
      resetArchiveRetention: "30d", // duration or false
      maxDiskBytes: "500mb", // optional hard budget
      highWaterBytes: "400mb", // optional cleanup target
    },
    threadBindings: {
      enabled: true,
      idleHours: 24, // default inactivity auto-unfocus in hours (`0` disables)
      maxAgeHours: 0, // default hard max age in hours (`0` disables)
    },
    mainKey: "main", // legacy (runtime always uses "main")
    agentToAgent: { maxPingPongTurns: 5 },
    sendPolicy: {
      rules: [{ action: "deny", match: { channel: "discord", chatType: "group" } }],
      default: "allow",
    },
  },
}
```

Session field details

- **`scope`**: base session grouping strategy for group-chat contexts.

  - `per-sender` (default): each sender gets an isolated session within a channel context.
  - `global`: all participants in a channel context share a single session (use only when shared context is intended).
- **`dmScope`**: how DMs are grouped.

  - `main`: all DMs share the main session.
  - `per-peer`: isolate by sender id across channels.
  - `per-channel-peer`: isolate per channel + sender (recommended for multi-user inboxes).
  - `per-account-channel-peer`: isolate per account + channel + sender (recommended for multi-account).
- **`identityLinks`**: map canonical ids to provider-prefixed peers for cross-channel session sharing. Dock commands such as `/dock_discord` use the same map to switch the active session’s reply route to another linked channel peer; see [Channel docking](https://docs.openclaw.ai/concepts/channel-docking).
- **`reset`**: primary reset policy. `daily` resets at `atHour` local time; `idle` resets after `idleMinutes`. When both configured, whichever expires first wins. Daily reset freshness uses the session row’s `sessionStartedAt`; idle reset freshness uses `lastInteractionAt`. Background/system-event writes such as heartbeat, cron wakeups, exec notifications, and gateway bookkeeping can update `updatedAt`, but they do not keep daily/idle sessions fresh.
- **`resetByType`**: per-type overrides (`direct`, `group`, `thread`). Legacy `dm` accepted as alias for `direct`.
- **`mainKey`**: legacy field. Runtime always uses `"main"` for the main direct-chat bucket.
- **`agentToAgent.maxPingPongTurns`**: maximum reply-back turns between agents during agent-to-agent exchanges (integer, range: `0`–`5`). `0` disables ping-pong chaining.
- **`sendPolicy`**: match by `channel`, `chatType` (`direct|group|channel`, with legacy `dm` alias), `keyPrefix`, or `rawKeyPrefix`. First deny wins.
- **`maintenance`**: session-store cleanup + retention controls.

  - `mode`: `warn` emits warnings only; `enforce` applies cleanup.
  - `pruneAfter`: age cutoff for stale entries (default `30d`).
  - `maxEntries`: maximum number of entries in `sessions.json` (default `500`). Runtime writes batch cleanup with a small high-water buffer for production-sized caps; `openclaw sessions cleanup --enforce` applies the cap immediately.
  - `rotateBytes`: deprecated and ignored; `openclaw doctor --fix` removes it from older configs.
  - `resetArchiveRetention`: retention for `*.reset.<timestamp>` transcript archives. Defaults to `pruneAfter`; set `false` to disable.
  - `maxDiskBytes`: optional sessions-directory disk budget. In `warn` mode it logs warnings; in `enforce` mode it removes oldest artifacts/sessions first.
  - `highWaterBytes`: optional target after budget cleanup. Defaults to `80%` of `maxDiskBytes`.
- **`threadBindings`**: global defaults for thread-bound session features.

  - `enabled`: master default switch (providers can override; Discord uses `channels.discord.threadBindings.enabled`)
  - `idleHours`: default inactivity auto-unfocus in hours (`0` disables; providers can override)
  - `maxAgeHours`: default hard max age in hours (`0` disables; providers can override)
  - `spawnSessions`: default gate for creating thread-bound work sessions from `sessions_spawn` and ACP thread spawns. Defaults to `true` when thread bindings are enabled; providers/accounts can override.
  - `defaultSpawnContext`: default native subagent context for thread-bound spawns (`"fork"` or `"isolated"`). Defaults to `"fork"`.

* * *

## Messages

```
{
  messages: {
    responsePrefix: "🦞", // or "auto"
    ackReaction: "👀",
    ackReactionScope: "group-mentions", // group-mentions | group-all | direct | all
    removeAckAfterReply: false,
    queue: {
      mode: "steer", // steer | queue (legacy one-at-a-time) | followup | collect | steer-backlog | steer+backlog | interrupt
      debounceMs: 500,
      cap: 20,
      drop: "summarize", // old | new | summarize
      byChannel: {
        whatsapp: "steer",
        telegram: "steer",
      },
    },
    inbound: {
      debounceMs: 2000, // 0 disables
      byChannel: {
        whatsapp: 5000,
        slack: 1500,
      },
    },
  },
}
```

### Response prefix

Per-channel/account overrides: `channels.<channel>.responsePrefix`, `channels.<channel>.accounts.<id>.responsePrefix`.Resolution (most specific wins): account → channel → global. `""` disables and stops cascade. `"auto"` derives `[{identity.name}]`.**Template variables:**

| Variable | Description | Example |
| --- | --- | --- |
| `{model}` | Short model name | `claude-opus-4-6` |
| `{modelFull}` | Full model identifier | `anthropic/claude-opus-4-6` |
| `{provider}` | Provider name | `anthropic` |
| `{thinkingLevel}` | Current thinking level | `high`, `low`, `off` |
| `{identity.name}` | Agent identity name | (same as `"auto"`) |

Variables are case-insensitive. `{think}` is an alias for `{thinkingLevel}`.

### Ack reaction

- Defaults to active agent’s `identity.emoji`, otherwise `"👀"`. Set `""` to disable.
- Per-channel overrides: `channels.<channel>.ackReaction`, `channels.<channel>.accounts.<id>.ackReaction`.
- Resolution order: account → channel → `messages.ackReaction` → identity fallback.
- Scope: `group-mentions` (default), `group-all`, `direct`, `all`.
- `removeAckAfterReply`: removes ack after reply on reaction-capable channels such as Slack, Discord, Telegram, WhatsApp, and BlueBubbles.
- `messages.statusReactions.enabled`: enables lifecycle status reactions on Slack, Discord, and Telegram.
On Slack and Discord, unset keeps status reactions enabled when ack reactions are active.
On Telegram, set it explicitly to `true` to enable lifecycle status reactions.

### Inbound debounce

Batches rapid text-only messages from the same sender into a single agent turn. Media/attachments flush immediately. Control commands bypass debouncing.

### TTS (text-to-speech)

```
{
  messages: {
    tts: {
      auto: "always", // off | always | inbound | tagged
      mode: "final", // final | all
      provider: "elevenlabs",
      summaryModel: "openai/gpt-4.1-mini",
      modelOverrides: { enabled: true },
      maxTextLength: 4000,
      timeoutMs: 30000,
      prefsPath: "~/.openclaw/settings/tts.json",
      providers: {
        elevenlabs: {
          apiKey: "elevenlabs_api_key",
          baseUrl: "https://api.elevenlabs.io",
          voiceId: "voice_id",
          modelId: "eleven_multilingual_v2",
          seed: 42,
          applyTextNormalization: "auto",
          languageCode: "en",
          voiceSettings: {
            stability: 0.5,
            similarityBoost: 0.75,
            style: 0.0,
            useSpeakerBoost: true,
            speed: 1.0,
          },
        },
        microsoft: {
          voice: "en-US-AvaMultilingualNeural",
          lang: "en-US",
          outputFormat: "audio-24khz-48kbitrate-mono-mp3",
        },
        openai: {
          apiKey: "openai_api_key",
          baseUrl: "https://api.openai.com/v1",
          model: "gpt-4o-mini-tts",
          voice: "alloy",
        },
      },
    },
  },
}
```

- `auto` controls the default auto-TTS mode: `off`, `always`, `inbound`, or `tagged`. `/tts on|off` can override local prefs, and `/tts status` shows the effective state.
- `summaryModel` overrides `agents.defaults.model.primary` for auto-summary.
- `modelOverrides` is enabled by default; `modelOverrides.allowProvider` defaults to `false` (opt-in).
- API keys fall back to `ELEVENLABS_API_KEY`/`XI_API_KEY` and `OPENAI_API_KEY`.
- Bundled speech providers are plugin-owned. If `plugins.allow` is set, include each TTS provider plugin you want to use, for example `microsoft` for Edge TTS. The legacy `edge` provider id is accepted as an alias for `microsoft`.
- `providers.openai.baseUrl` overrides the OpenAI TTS endpoint. Resolution order is config, then `OPENAI_TTS_BASE_URL`, then `https://api.openai.com/v1`.
- When `providers.openai.baseUrl` points to a non-OpenAI endpoint, OpenClaw treats it as an OpenAI-compatible TTS server and relaxes model/voice validation.

* * *

## Talk

Defaults for Talk mode (macOS/iOS/Android).

```
{
  talk: {
    provider: "elevenlabs",
    providers: {
      elevenlabs: {
        voiceId: "elevenlabs_voice_id",
        voiceAliases: {
          Clawd: "EXAVITQu4vr4xnSDxMaL",
          Roger: "CwhRBWXzGAHq8TQ4Fs17",
        },
        modelId: "eleven_v3",
        outputFormat: "mp3_44100_128",
        apiKey: "elevenlabs_api_key",
      },
      mlx: {
        modelId: "mlx-community/Soprano-80M-bf16",
      },
      system: {},
    },
    speechLocale: "ru-RU",
    silenceTimeoutMs: 1500,
    interruptOnSpeech: true,
  },
}
```

- `talk.provider` must match a key in `talk.providers` when multiple Talk providers are configured.
- Legacy flat Talk keys (`talk.voiceId`, `talk.voiceAliases`, `talk.modelId`, `talk.outputFormat`, `talk.apiKey`) are compatibility-only and are auto-migrated into `talk.providers.<provider>`.
- Voice IDs fall back to `ELEVENLABS_VOICE_ID` or `SAG_VOICE_ID`.
- `providers.*.apiKey` accepts plaintext strings or SecretRef objects.
- `ELEVENLABS_API_KEY` fallback applies only when no Talk API key is configured.
- `providers.*.voiceAliases` lets Talk directives use friendly names.
- `providers.mlx.modelId` selects the Hugging Face repo used by the macOS local MLX helper. If omitted, macOS uses `mlx-community/Soprano-80M-bf16`.
- macOS MLX playback runs through the bundled `openclaw-mlx-tts` helper when present, or an executable on `PATH`; `OPENCLAW_MLX_TTS_BIN` overrides the helper path for development.
- `speechLocale` sets the BCP 47 locale id used by iOS/macOS Talk speech recognition. Leave unset to use the device default.
- `silenceTimeoutMs` controls how long Talk mode waits after user silence before it sends the transcript. Unset keeps the platform default pause window (`700 ms on macOS and Android, 900 ms on iOS`).

* * *

## Related

- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference) — all other config keys
- [Configuration](https://docs.openclaw.ai/gateway/configuration) — common tasks and quick setup
- [Configuration examples](https://docs.openclaw.ai/gateway/configuration-examples)

[Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference) [Configuration — channels](https://docs.openclaw.ai/gateway/config-channels)

Ctrl+I

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
      },
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15551234567"],
    },
  },
  web: {
    enabled: true,
    heartbeatSeconds: 60,
    reconnect: {
      initialMs: 2000,
      maxMs: 120000,
      factor: 1.4,
      jitter: 0.2,
      maxAttempts: 0,
    },
  },
}
```

Multi-account WhatsApp

```
{
  channels: {
    whatsapp: {
      accounts: {
        default: {},
        personal: {},
        biz: {
          // authDir: "~/.openclaw/credentials/whatsapp/biz",
        },
      },
    },
  },
}
```

- Outbound commands default to account `default` if present; otherwise the first configured account id (sorted).
- Optional `channels.whatsapp.defaultAccount` overrides that fallback default account selection when it matches a configured account id.
- Legacy single-account Baileys auth dir is migrated by `openclaw doctor` into `whatsapp/default`.
- Per-account overrides: `channels.whatsapp.accounts.<id>.sendReadReceipts`, `channels.whatsapp.accounts.<id>.dmPolicy`, `channels.whatsapp.accounts.<id>.allowFrom`.

### Telegram

```
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "your-bot-token",
      dmPolicy: "pairing",
      allowFrom: ["tg:123456789"],
      groups: {
        "*": { requireMention: true },
        "-1001234567890": {
          allowFrom: ["@admin"],
          systemPrompt: "Keep answers brief.",
          topics: {
            "99": {
              requireMention: false,
              skills: ["search"],
              systemPrompt: "Stay on topic.",
            },
          },
        },
      },
      customCommands: [\
        { command: "backup", description: "Git backup" },\
        { command: "generate", description: "Create an image" },\
      ],
      historyLimit: 50,
      replyToMode: "first", // off | first | all | batched
      linkPreview: true,
      streaming: "partial", // off | partial | block | progress (default: off; opt in explicitly to avoid preview-edit rate limits)
      actions: { reactions: true, sendMessage: true },
      reactionNotifications: "own", // off | own | all
      mediaMaxMb: 100,
      retry: {
        attempts: 3,
        minDelayMs: 400,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
      network: {
        autoSelectFamily: true,
        dnsResultOrder: "ipv4first",
      },
      apiRoot: "https://api.telegram.org",
      proxy: "socks5://localhost:9050",
      webhookUrl: "https://example.com/telegram-webhook",
      webhookSecret: "secret",
      webhookPath: "/telegram-webhook",
    },
  },
}
```

- Bot token: `channels.telegram.botToken` or `channels.telegram.tokenFile` (regular file only; symlinks rejected), with `TELEGRAM_BOT_TOKEN` as fallback for the default account.
- `apiRoot` is the Telegram Bot API root only. Use `https://api.telegram.org` or your self-hosted/proxy root, not `https://api.telegram.org/bot<TOKEN>`; `openclaw doctor --fix` removes an accidental trailing `/bot<TOKEN>` suffix.
- Optional `channels.telegram.defaultAccount` overrides default account selection when it matches a configured account id.
- In multi-account setups (2+ account ids), set an explicit default (`channels.telegram.defaultAccount` or `channels.telegram.accounts.default`) to avoid fallback routing; `openclaw doctor` warns when this is missing or invalid.
- `configWrites: false` blocks Telegram-initiated config writes (supergroup ID migrations, `/config set|unset`).
- Top-level `bindings[]` entries with `type: "acp"` configure persistent ACP bindings for forum topics (use canonical `chatId:topic:topicId` in `match.peer.id`). Field semantics are shared in [ACP Agents](https://docs.openclaw.ai/tools/acp-agents#persistent-channel-bindings).
- Telegram stream previews use `sendMessage` \+ `editMessageText` (works in direct and group chats).
- Retry policy: see [Retry policy](https://docs.openclaw.ai/concepts/retry).

### Discord

```
{
  channels: {
    discord: {
      enabled: true,
      token: "your-bot-token",
      mediaMaxMb: 100,
      allowBots: false,
      actions: {
        reactions: true,
        stickers: true,
        polls: true,
        permissions: true,
        messages: true,
        threads: true,
        pins: true,
        search: true,
        memberInfo: true,
        roleInfo: true,
        roles: false,
        channelInfo: true,
        voiceStatus: true,
        events: true,
        moderation: false,
      },
      replyToMode: "off", // off | first | all | batched
      dmPolicy: "pairing",
      allowFrom: ["1234567890", "123456789012345678"],
      dm: { enabled: true, groupEnabled: false, groupChannels: ["openclaw-dm"] },
      guilds: {
        "123456789012345678": {
          slug: "friends-of-openclaw",
          requireMention: false,
          ignoreOtherMentions: true,
          reactionNotifications: "own",
          users: ["987654321098765432"],
          channels: {
            general: { allow: true },
            help: {
              allow: true,
              requireMention: true,
              users: ["987654321098765432"],
              skills: ["docs"],
              systemPrompt: "Short answers only.",
            },
          },
        },
      },
      historyLimit: 20,
      textChunkLimit: 2000,
      chunkMode: "length", // length | newline
      streaming: "off", // off | partial | block | progress (progress maps to partial on Discord)
      maxLinesPerMessage: 17,
      ui: {
        components: {
          accentColor: "#5865F2",
        },
      },
      threadBindings: {
        enabled: true,
        idleHours: 24,
        maxAgeHours: 0,
        spawnSessions: true,
        defaultSpawnContext: "fork",
      },
      voice: {
        enabled: true,
        autoJoin: [\
          {\
            guildId: "123456789012345678",\
            channelId: "234567890123456789",\
          },\
        ],
        daveEncryption: true,
        decryptionFailureTolerance: 24,
        connectTimeoutMs: 30000,
        reconnectGraceMs: 15000,
        tts: {
          provider: "openai",
          openai: { voice: "alloy" },
        },
      },
      execApprovals: {
        enabled: "auto", // true | false | "auto"
        approvers: ["987654321098765432"],
        agentFilter: ["default"],
        sessionFilter: ["discord:"],
        target: "dm", // dm | channel | both
        cleanupAfterResolve: false,
      },
      retry: {
        attempts: 3,
        minDelayMs: 500,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
    },
  },
}
```

- Token: `channels.discord.token`, with `DISCORD_BOT_TOKEN` as fallback for the default account.
- Direct outbound calls that provide an explicit Discord `token` use that token for the call; account retry/policy settings still come from the selected account in the active runtime snapshot.
- Optional `channels.discord.defaultAccount` overrides default account selection when it matches a configured account id.
- Use `user:<id>` (DM) or `channel:<id>` (guild channel) for delivery targets; bare numeric IDs are rejected.
- Guild slugs are lowercase with spaces replaced by `-`; channel keys use the slugged name (no `#`). Prefer guild IDs.
- Bot-authored messages are ignored by default. `allowBots: true` enables them; use `allowBots: "mentions"` to only accept bot messages that mention the bot (own messages still filtered).
- `channels.discord.guilds.<id>.ignoreOtherMentions` (and channel overrides) drops messages that mention another user or role but not the bot (excluding @everyone/@here).
- `channels.discord.mentionAliases` maps stable outbound `@handle` text to Discord user IDs before sending, so known teammates can be mentioned deterministically even when the transient directory cache is empty. Per-account overrides live under `channels.discord.accounts.<accountId>.mentionAliases`.
- `maxLinesPerMessage` (default 17) splits tall messages even when under 2000 chars.
- `channels.discord.threadBindings`controls Discord thread-bound routing:

  - `enabled`: Discord override for thread-bound session features (`/focus`, `/unfocus`, `/agents`, `/session idle`, `/session max-age`, and bound delivery/routing)
  - `idleHours`: Discord override for inactivity auto-unfocus in hours (`0` disables)
  - `maxAgeHours`: Discord override for hard max age in hours (`0` disables)
  - `spawnSessions`: switch for `sessions_spawn({ thread: true })` and ACP thread-spawn auto thread creation/binding (default: `true`)
  - `defaultSpawnContext`: native subagent context for thread-bound spawns (`"fork"` by default)
- Top-level `bindings[]` entries with `type: "acp"` configure persistent ACP bindings for channels and threads (use channel/thread id in `match.peer.id`). Field semantics are shared in [ACP Agents](https://docs.openclaw.ai/tools/acp-agents#persistent-channel-bindings).
- `channels.discord.ui.components.accentColor` sets the accent color for Discord components v2 containers.
- `channels.discord.voice` enables Discord voice channel conversations and optional auto-join + LLM + TTS overrides. Text-only Discord configs leave voice off by default; set `channels.discord.voice.enabled=true` to opt in.
- `channels.discord.voice.model` optionally overrides the LLM model used for Discord voice channel responses.
- `channels.discord.voice.daveEncryption` and `channels.discord.voice.decryptionFailureTolerance` pass through to `@discordjs/voice` DAVE options (`true` and `24` by default).
- `channels.discord.voice.connectTimeoutMs` controls the initial `@discordjs/voice` Ready wait for `/vc join` and auto-join attempts (`30000` by default).
- `channels.discord.voice.reconnectGraceMs` controls how long a disconnected voice session may take to enter reconnect signalling before OpenClaw destroys it (`15000` by default).
- OpenClaw additionally attempts voice receive recovery by leaving/rejoining a voice session after repeated decrypt failures.
- `channels.discord.streaming` is the canonical stream mode key. Legacy `streamMode` and boolean `streaming` values are auto-migrated.
- `channels.discord.autoPresence` maps runtime availability to bot presence (healthy => online, degraded => idle, exhausted => dnd) and allows optional status text overrides.
- `channels.discord.dangerouslyAllowNameMatching` re-enables mutable name/tag matching (break-glass compatibility mode).
- `channels.discord.execApprovals`: Discord-native exec approval delivery and approver authorization.

  - `enabled`: `true`, `false`, or `"auto"` (default). In auto mode, exec approvals activate when approvers can be resolved from `approvers` or `commands.ownerAllowFrom`.
  - `approvers`: Discord user IDs allowed to approve exec requests. Falls back to `commands.ownerAllowFrom` when omitted.
  - `agentFilter`: optional agent ID allowlist. Omit to forward approvals for all agents.
  - `sessionFilter`: optional session key patterns (substring or regex).
  - `target`: where to send approval prompts. `"dm"` (default) sends to approver DMs, `"channel"` sends to the originating channel, `"both"` sends to both. When target includes `"channel"`, buttons are only usable by resolved approvers.
  - `cleanupAfterResolve`: when `true`, deletes approval DMs after approval, denial, or timeout.

**Reaction notification modes:**`off` (none), `own` (bot’s messages, default), `all` (all messages), `allowlist` (from `guilds.<id>.users` on all messages).

### Google Chat

```
{
  channels: {
    googlechat: {
      enabled: true,
      serviceAccountFile: "/path/to/service-account.json",
      audienceType: "app-url", // app-url | project-number
      audience: "https://gateway.example.com/googlechat",
      webhookPath: "/googlechat",
      botUser: "users/1234567890",
      dm: {
        enabled: true,
        policy: "pairing",
        allowFrom: ["users/1234567890"],
      },
      groupPolicy: "allowlist",
      groups: {
        "spaces/AAAA": { allow: true, requireMention: true },
      },
      actions: { reactions: true },
      typingIndicator: "message",
      mediaMaxMb: 20,
    },
  },
}
```

- Service account JSON: inline (`serviceAccount`) or file-based (`serviceAccountFile`).
- Service account SecretRef is also supported (`serviceAccountRef`).
- Env fallbacks: `GOOGLE_CHAT_SERVICE_ACCOUNT` or `GOOGLE_CHAT_SERVICE_ACCOUNT_FILE`.
- Use `spaces/<spaceId>` or `users/<userId>` for delivery targets.
- `channels.googlechat.dangerouslyAllowNameMatching` re-enables mutable email principal matching (break-glass compatibility mode).

### Slack

```
{
  channels: {
    slack: {
      enabled: true,
      botToken: "xoxb-...",
      appToken: "xapp-...",
      socketMode: {
        clientPingTimeout: 15000,
        serverPingTimeout: 30000,
        pingPongLoggingEnabled: false,
      },
      dmPolicy: "pairing",
      allowFrom: ["U123", "U456", "*"],
      dm: { enabled: true, groupEnabled: false, groupChannels: ["G123"] },
      channels: {
        C123: { allow: true, requireMention: true, allowBots: false },
        "#general": {
          allow: true,
          requireMention: true,
          allowBots: false,
          users: ["U123"],
          skills: ["docs"],
          systemPrompt: "Short answers only.",
        },
      },
      historyLimit: 50,
      allowBots: false,
      reactionNotifications: "own",
      reactionAllowlist: ["U123"],
      replyToMode: "off", // off | first | all | batched
      thread: {
        historyScope: "thread", // thread | channel
        inheritParent: false,
      },
      actions: {
        reactions: true,
        messages: true,
        pins: true,
        memberInfo: true,
        emojiList: true,
      },
      slashCommand: {
        enabled: true,
        name: "openclaw",
        sessionPrefix: "slack:slash",
        ephemeral: true,
      },
      typingReaction: "hourglass_flowing_sand",
      textChunkLimit: 4000,
      chunkMode: "length",
      streaming: {
        mode: "partial", // off | partial | block | progress
        nativeTransport: true, // use Slack native streaming API when mode=partial
      },
      mediaMaxMb: 20,
      execApprovals: {
        enabled: "auto", // true | false | "auto"
        approvers: ["U123"],
        agentFilter: ["default"],
        sessionFilter: ["slack:"],
        target: "dm", // dm | channel | both
      },
    },
  },
}
```

- **Socket mode** requires both `botToken` and `appToken` (`SLACK_BOT_TOKEN` \+ `SLACK_APP_TOKEN` for default account env fallback).
- **HTTP mode** requires `botToken` plus `signingSecret` (at root or per-account).
- `socketMode` passes Slack SDK Socket Mode transport tuning through to the public Bolt receiver API. Use it only when investigating ping/pong timeout or stale websocket behavior.
- `botToken`, `appToken`, `signingSecret`, and `userToken` accept plaintext
strings or SecretRef objects.
- Slack account snapshots expose per-credential source/status fields such as
`botTokenSource`, `botTokenStatus`, `appTokenStatus`, and, in HTTP mode,
`signingSecretStatus`. `configured_unavailable` means the account is
configured through SecretRef but the current command/runtime path could not
resolve the secret value.
- `configWrites: false` blocks Slack-initiated config writes.
- Optional `channels.slack.defaultAccount` overrides default account selection when it matches a configured account id.
- `channels.slack.streaming.mode` is the canonical Slack stream mode key. `channels.slack.streaming.nativeTransport` controls Slack’s native streaming transport. Legacy `streamMode`, boolean `streaming`, and `nativeStreaming` values are auto-migrated.
- Use `user:<id>` (DM) or `channel:<id>` for delivery targets.

**Reaction notification modes:**`off`, `own` (default), `all`, `allowlist` (from `reactionAllowlist`).**Thread session isolation:**`thread.historyScope` is per-thread (default) or shared across channel. `thread.inheritParent` copies parent channel transcript to new threads.

- Slack native streaming plus the Slack assistant-style “is typing…” thread status require a reply thread target. Top-level DMs stay off-thread by default, so they can still stream through Slack draft post-and-edit previews instead of showing the thread-style native stream/status preview.
- `typingReaction` adds a temporary reaction to the inbound Slack message while a reply is running, then removes it on completion. Use a Slack emoji shortcode such as `"hourglass_flowing_sand"`.
- `channels.slack.execApprovals`: Slack-native exec approval delivery and approver authorization. Same schema as Discord: `enabled` (`true`/`false`/`"auto"`), `approvers` (Slack user IDs), `agentFilter`, `sessionFilter`, and `target` (`"dm"`, `"channel"`, or `"both"`).

| Action group | Default | Notes |
| --- | --- | --- |
| reactions | enabled | React + list reactions |
| messages | enabled | Read/send/edit/delete |
| pins | enabled | Pin/unpin/list |
| memberInfo | enabled | Member info |
| emojiList | enabled | Custom emoji list |

### Mattermost

Mattermost ships as a bundled plugin in current OpenClaw releases. Older or
custom builds can install a current npm package with
`openclaw plugins install @openclaw/mattermost`. Check
[npmjs.com/package/@openclaw/mattermost](https://www.npmjs.com/package/@openclaw/mattermost)
for the current dist-tags before pinning a version.

```
{
  channels: {
    mattermost: {
      enabled: true,
      botToken: "mm-token",
      baseUrl: "https://chat.example.com",
      dmPolicy: "pairing",
      chatmode: "oncall", // oncall | onmessage | onchar
      oncharPrefixes: [">", "!"],
      groups: {
        "*": { requireMention: true },
        "team-channel-id": { requireMention: false },
      },
      commands: {
        native: true, // opt-in
        nativeSkills: true,
        callbackPath: "/api/channels/mattermost/command",
        // Optional explicit URL for reverse-proxy/public deployments
        callbackUrl: "https://gateway.example.com/api/channels/mattermost/command",
      },
      textChunkLimit: 4000,
      chunkMode: "length",
    },
  },
}
```

Chat modes: `oncall` (respond on @-mention, default), `onmessage` (every message), `onchar` (messages starting with trigger prefix).When Mattermost native commands are enabled:

- `commands.callbackPath` must be a path (for example `/api/channels/mattermost/command`), not a full URL.
- `commands.callbackUrl` must resolve to the OpenClaw gateway endpoint and be reachable from the Mattermost server.
- Native slash callbacks are authenticated with the per-command tokens returned
by Mattermost during slash command registration. If registration fails or no
commands are activated, OpenClaw rejects callbacks with
`Unauthorized: invalid command token.`
- For private/tailnet/internal callback hosts, Mattermost may require
`ServiceSettings.AllowedUntrustedInternalConnections` to include the callback host/domain.
Use host/domain values, not full URLs.
- `channels.mattermost.configWrites`: allow or deny Mattermost-initiated config writes.
- `channels.mattermost.requireMention`: require `@mention` before replying in channels.
- `channels.mattermost.groups.<channelId>.requireMention`: per-channel mention-gating override (`"*"` for default).
- Optional `channels.mattermost.defaultAccount` overrides default account selection when it matches a configured account id.

### Signal

```
{
  channels: {
    signal: {
      enabled: true,
      account: "+15555550123", // optional account binding
      dmPolicy: "pairing",
      allowFrom: ["+15551234567", "uuid:123e4567-e89b-12d3-a456-426614174000"],
      configWrites: true,
      reactionNotifications: "own", // off | own | all | allowlist
      reactionAllowlist: ["+15551234567", "uuid:123e4567-e89b-12d3-a456-426614174000"],
      historyLimit: 50,
    },
  },
}
```

**Reaction notification modes:**`off`, `own` (default), `all`, `allowlist` (from `reactionAllowlist`).

- `channels.signal.account`: pin channel startup to a specific Signal account identity.
- `channels.signal.configWrites`: allow or deny Signal-initiated config writes.
- Optional `channels.signal.defaultAccount` overrides default account selection when it matches a configured account id.

### BlueBubbles

BlueBubbles is the recommended iMessage path (plugin-backed, configured under `channels.bluebubbles`).

```
{
  channels: {
    bluebubbles: {
      enabled: true,
      dmPolicy: "pairing",
      // serverUrl, password, webhookPath, group controls, and advanced actions:
      // see /channels/bluebubbles
    },
  },
}
```

- Core key paths covered here: `channels.bluebubbles`, `channels.bluebubbles.dmPolicy`.
- Optional `channels.bluebubbles.defaultAccount` overrides default account selection when it matches a configured account id.
- Top-level `bindings[]` entries with `type: "acp"` can bind BlueBubbles conversations to persistent ACP sessions. Use a BlueBubbles handle or target string (`chat_id:*`, `chat_guid:*`, `chat_identifier:*`) in `match.peer.id`. Shared field semantics: [ACP Agents](https://docs.openclaw.ai/tools/acp-agents#persistent-channel-bindings).
- Full BlueBubbles channel configuration is documented in [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles).

### iMessage

OpenClaw spawns `imsg rpc` (JSON-RPC over stdio). No daemon or port required.

```
{
  channels: {
    imessage: {
      enabled: true,
      cliPath: "imsg",
      dbPath: "~/Library/Messages/chat.db",
      remoteHost: "user@gateway-host",
      dmPolicy: "pairing",
      allowFrom: ["+15555550123", "user@example.com", "chat_id:123"],
      historyLimit: 50,
      includeAttachments: false,
      attachmentRoots: ["/Users/*/Library/Messages/Attachments"],
      remoteAttachmentRoots: ["/Users/*/Library/Messages/Attachments"],
      mediaMaxMb: 16,
      service: "auto",
      region: "US",
    },
  },
}
```

- Optional `channels.imessage.defaultAccount` overrides default account selection when it matches a configured account id.
- Requires Full Disk Access to the Messages DB.
- Prefer `chat_id:<id>` targets. Use `imsg chats --limit 20` to list chats.
- `cliPath` can point to an SSH wrapper; set `remoteHost` (`host` or `user@host`) for SCP attachment fetching.
- `attachmentRoots` and `remoteAttachmentRoots` restrict inbound attachment paths (default: `/Users/*/Library/Messages/Attachments`).
- SCP uses strict host-key checking, so ensure the relay host key already exists in `~/.ssh/known_hosts`.
- `channels.imessage.configWrites`: allow or deny iMessage-initiated config writes.
- Top-level `bindings[]` entries with `type: "acp"` can bind iMessage conversations to persistent ACP sessions. Use a normalized handle or explicit chat target (`chat_id:*`, `chat_guid:*`, `chat_identifier:*`) in `match.peer.id`. Shared field semantics: [ACP Agents](https://docs.openclaw.ai/tools/acp-agents#persistent-channel-bindings).

iMessage SSH wrapper example

```
#!/usr/bin/env bash
exec ssh -T gateway-host imsg "$@"
```

### Matrix

Matrix is plugin-backed and configured under `channels.matrix`.

```
{
  channels: {
    matrix: {
      enabled: true,
      homeserver: "https://matrix.example.org",
      accessToken: "syt_bot_xxx",
      proxy: "http://127.0.0.1:7890",
      encryption: true,
      initialSyncLimit: 20,
      defaultAccount: "ops",
      accounts: {
        ops: {
          name: "Ops",
          userId: "@ops:example.org",
          accessToken: "syt_ops_xxx",
        },
        alerts: {
          userId: "@alerts:example.org",
          password: "secret",
          proxy: "http://127.0.0.1:7891",
        },
      },
    },
  },
}
```

- Token auth uses `accessToken`; password auth uses `userId` \+ `password`.
- `channels.matrix.proxy` routes Matrix HTTP traffic through an explicit HTTP(S) proxy. Named accounts can override it with `channels.matrix.accounts.<id>.proxy`.
- `channels.matrix.network.dangerouslyAllowPrivateNetwork` allows private/internal homeservers. `proxy` and this network opt-in are independent controls.
- `channels.matrix.defaultAccount` selects the preferred account in multi-account setups.
- `channels.matrix.autoJoin` defaults to `off`, so invited rooms and fresh DM-style invites are ignored until you set `autoJoin: "allowlist"` with `autoJoinAllowlist` or `autoJoin: "always"`.
- `channels.matrix.execApprovals`: Matrix-native exec approval delivery and approver authorization.

  - `enabled`: `true`, `false`, or `"auto"` (default). In auto mode, exec approvals activate when approvers can be resolved from `approvers` or `commands.ownerAllowFrom`.
  - `approvers`: Matrix user IDs (e.g. `@owner:example.org`) allowed to approve exec requests.
  - `agentFilter`: optional agent ID allowlist. Omit to forward approvals for all agents.
  - `sessionFilter`: optional session key patterns (substring or regex).
  - `target`: where to send approval prompts. `"dm"` (default), `"channel"` (originating room), or `"both"`.
  - Per-account overrides: `channels.matrix.accounts.<id>.execApprovals`.
- `channels.matrix.dm.sessionScope` controls how Matrix DMs group into sessions: `per-user` (default) shares by routed peer, while `per-room` isolates each DM room.
- Matrix status probes and live directory lookups use the same proxy policy as runtime traffic.
- Full Matrix configuration, targeting rules, and setup examples are documented in [Matrix](https://docs.openclaw.ai/channels/matrix).

### Microsoft Teams

Microsoft Teams is plugin-backed and configured under `channels.msteams`.

```
{
  channels: {
    msteams: {
      enabled: true,
      configWrites: true,
      // appId, appPassword, tenantId, webhook, team/channel policies:
      // see /channels/msteams
    },
  },
}
```

- Core key paths covered here: `channels.msteams`, `channels.msteams.configWrites`.
- Full Teams config (credentials, webhook, DM/group policy, per-team/per-channel overrides) is documented in [Microsoft Teams](https://docs.openclaw.ai/channels/msteams).

### IRC

IRC is plugin-backed and configured under `channels.irc`.

```
{
  channels: {
    irc: {
      enabled: true,
      dmPolicy: "pairing",
      configWrites: true,
      nickserv: {
        enabled: true,
        service: "NickServ",
        password: "${IRC_NICKSERV_PASSWORD}",
        register: false,
        registerEmail: "bot@example.com",
      },
    },
  },
}
```

- Core key paths covered here: `channels.irc`, `channels.irc.dmPolicy`, `channels.irc.configWrites`, `channels.irc.nickserv.*`.
- Optional `channels.irc.defaultAccount` overrides default account selection when it matches a configured account id.
- Full IRC channel configuration (host/port/TLS/channels/allowlists/mention gating) is documented in [IRC](https://docs.openclaw.ai/channels/irc).

### Multi-account (all channels)

Run multiple accounts per channel (each with its own `accountId`):

```
{
  channels: {
    telegram: {
      accounts: {
        default: {
          name: "Primary bot",
          botToken: "123456:ABC...",
        },
        alerts: {
          name: "Alerts bot",
          botToken: "987654:XYZ...",
        },
      },
    },
  },
}
```

- `default` is used when `accountId` is omitted (CLI + routing).
- Env tokens only apply to the **default** account.
- Base channel settings apply to all accounts unless overridden per account.
- Use `bindings[].match.accountId` to route each account to a different agent.
- If you add a non-default account via `openclaw channels add` (or channel onboarding) while still on a single-account top-level channel config, OpenClaw promotes account-scoped top-level single-account values into the channel account map first so the original account keeps working. Most channels move them into `channels.<channel>.accounts.default`; Matrix can preserve an existing matching named/default target instead.
- Existing channel-only bindings (no `accountId`) keep matching the default account; account-scoped bindings remain optional.
- `openclaw doctor --fix` also repairs mixed shapes by moving account-scoped top-level single-account values into the promoted account chosen for that channel. Most channels use `accounts.default`; Matrix can preserve an existing matching named/default target instead.

### Other plugin channels

Many plugin channels are configured as `channels.<id>` and documented in their dedicated channel pages (for example Feishu, Matrix, LINE, Nostr, Zalo, Nextcloud Talk, Synology Chat, and Twitch).
See the full channel index: [Channels](https://docs.openclaw.ai/channels).

### Group chat mention gating

Group messages default to **require mention** (metadata mention or safe regex patterns). Applies to WhatsApp, Telegram, Discord, Google Chat, and iMessage group chats.Visible replies are controlled separately. Group/channel rooms default to `messages.groupChat.visibleReplies: "message_tool"`: OpenClaw still processes the turn, but normal final replies stay private and visible room output requires `message(action=send)`. Set `"automatic"` only when you want the legacy behavior where normal replies are posted back to the room. To apply the same tool-only visible-reply behavior to direct chats too, set `messages.visibleReplies: "message_tool"`; the Codex harness also uses that tool-only behavior as its unset direct-chat default.If the message tool is unavailable under the active tool policy, OpenClaw falls back to automatic visible replies instead of silently suppressing the response. `openclaw doctor` warns about this mismatch.The gateway hot-reloads `messages` config after the file is saved. Restart only when file watching or config reload is disabled in the deployment.**Mention types:**

- **Metadata mentions**: Native platform @-mentions. Ignored in WhatsApp self-chat mode.
- **Text patterns**: Safe regex patterns in `agents.list[].groupChat.mentionPatterns`. Invalid patterns and unsafe nested repetition are ignored.
- Mention gating is enforced only when detection is possible (native mentions or at least one pattern).

```
{
  messages: {
    visibleReplies: "automatic", // global default for direct/source chats; Codex harness defaults unset direct chats to message_tool
    groupChat: {
      historyLimit: 50,
      visibleReplies: "message_tool", // default; use "automatic" for legacy final replies
    },
  },
  agents: {
    list: [{ id: "main", groupChat: { mentionPatterns: ["@openclaw", "openclaw"] } }],
  },
}
```

`messages.groupChat.historyLimit` sets the global default. Channels can override with `channels.<channel>.historyLimit` (or per-account). Set `0` to disable.`messages.visibleReplies` is the global source-turn default; `messages.groupChat.visibleReplies` overrides it for group/channel source turns. When `messages.visibleReplies` is unset, a harness can provide its own direct/source default; the Codex harness defaults to `message_tool`. Channel allowlists and mention gating still decide whether a turn is processed.

#### DM history limits

```
{
  channels: {
    telegram: {
      dmHistoryLimit: 30,
      dms: {
        "123456789": { historyLimit: 50 },
      },
    },
  },
}
```

Resolution: per-DM override → provider default → no limit (all retained).Supported: `telegram`, `whatsapp`, `discord`, `slack`, `signal`, `imessage`, `msteams`.

#### Self-chat mode

Include your own number in `allowFrom` to enable self-chat mode (ignores native @-mentions, only responds to text patterns):

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  agents: {
    list: [\
      {\
        id: "main",\
        groupChat: { mentionPatterns: ["reisponde", "@openclaw"] },\
      },\
    ],
  },
}
```

### Commands (chat command handling)

```
{
  commands: {
    native: "auto", // register native commands when supported
    nativeSkills: "auto", // register native skill commands when supported
    text: true, // parse /commands in chat messages
    bash: false, // allow ! (alias: /bash)
    bashForegroundMs: 2000,
    config: false, // allow /config
    mcp: false, // allow /mcp
    plugins: false, // allow /plugins
    debug: false, // allow /debug
    restart: true, // allow /restart + gateway restart tool
    ownerAllowFrom: ["discord:123456789012345678"],
    ownerDisplay: "raw", // raw | hash
    ownerDisplaySecret: "${OWNER_ID_HASH_SECRET}",
    allowFrom: {
      "*": ["user1"],
      discord: ["user:123"],
    },
    useAccessGroups: true,
  },
}
```

Command details

- This block configures command surfaces. For the current built-in + bundled command catalog, see [Slash Commands](https://docs.openclaw.ai/tools/slash-commands).
- This page is a **config-key reference**, not the full command catalog. Channel/plugin-owned commands such as QQ Bot `/bot-ping``/bot-help``/bot-logs`, LINE `/card`, device-pair `/pair`, memory `/dreaming`, phone-control `/phone`, and Talk `/voice` are documented in their channel/plugin pages plus [Slash Commands](https://docs.openclaw.ai/tools/slash-commands).
- Text commands must be **standalone** messages with leading `/`.
- `native: "auto"` turns on native commands for Discord/Telegram, leaves Slack off.
- `nativeSkills: "auto"` turns on native skill commands for Discord/Telegram, leaves Slack off.
- Override per channel: `channels.discord.commands.native` (bool or `"auto"`). `false` clears previously registered commands.
- Override native skill registration per channel with `channels.<provider>.commands.nativeSkills`.
- `channels.telegram.customCommands` adds extra Telegram bot menu entries.
- `bash: true` enables `! <cmd>` for host shell. Requires `tools.elevated.enabled` and sender in `tools.elevated.allowFrom.<channel>`.
- `config: true` enables `/config` (reads/writes `openclaw.json`). For gateway `chat.send` clients, persistent `/config set|unset` writes also require `operator.admin`; read-only `/config show` stays available to normal write-scoped operator clients.
- `mcp: true` enables `/mcp` for OpenClaw-managed MCP server config under `mcp.servers`.
- `plugins: true` enables `/plugins` for plugin discovery, install, and enable/disable controls.
- `channels.<provider>.configWrites` gates config mutations per channel (default: true).
- For multi-account channels, `channels.<provider>.accounts.<id>.configWrites` also gates writes that target that account (for example `/allowlist --config --account <id>` or `/config set channels.<provider>.accounts.<id>...`).
- `restart: false` disables `/restart` and gateway restart tool actions. Default: `true`.
- `ownerAllowFrom` is the explicit owner allowlist for owner-only commands/tools. It is separate from `allowFrom`.
- `ownerDisplay: "hash"` hashes owner ids in the system prompt. Set `ownerDisplaySecret` to control hashing.
- `allowFrom` is per-provider. When set, it is the **only** authorization source (channel allowlists/pairing and `useAccessGroups` are ignored).
- `useAccessGroups: false` allows commands to bypass access-group policies when `allowFrom` is not set.
- Command docs map:
  - built-in + bundled catalog: [Slash Commands](https://docs.openclaw.ai/tools/slash-commands)
  - channel-specific command surfaces: [Channels](https://docs.openclaw.ai/channels)
  - QQ Bot commands: [QQ Bot](https://docs.openclaw.ai/channels/qqbot)
  - pairing commands: [Pairing](https://docs.openclaw.ai/channels/pairing)
  - LINE card command: [LINE](https://docs.openclaw.ai/channels/line)
  - memory dreaming: [Dreaming](https://docs.openclaw.ai/concepts/dreaming)

* * *

## Related

- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference) — top-level keys
- [Configuration — agents](https://docs.openclaw.ai/gateway/config-agents)
- [Channels overview](https://docs.openclaw.ai/channels)

[Configuration — agents](https://docs.openclaw.ai/gateway/config-agents) [Tools and custom providers](https://docs.openclaw.ai/gateway/config-tools)

Ctrl+I

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

[​](https://docs.openclaw.ai/gateway/config-tools#param-critical-threshold)

criticalThreshold

number

Higher repeating threshold for blocking critical loops.

[​](https://docs.openclaw.ai/gateway/config-tools#param-global-circuit-breaker-threshold)

globalCircuitBreakerThreshold

number

Hard stop threshold for any no-progress run.

[​](https://docs.openclaw.ai/gateway/config-tools#param-detectors-generic-repeat)

detectors.genericRepeat

boolean

Warn on repeated same-tool/same-args calls.

[​](https://docs.openclaw.ai/gateway/config-tools#param-detectors-known-poll-no-progress)

detectors.knownPollNoProgress

boolean

Warn/block on known poll tools (`process.poll`, `command_status`, etc.).

[​](https://docs.openclaw.ai/gateway/config-tools#param-detectors-ping-pong)

detectors.pingPong

boolean

Warn/block on alternating no-progress pair patterns.

If `warningThreshold >= criticalThreshold` or `criticalThreshold >= globalCircuitBreakerThreshold`, validation fails.

### `tools.web`

```
{
  tools: {
    web: {
      search: {
        enabled: true,
        apiKey: "brave_api_key", // or BRAVE_API_KEY env
        maxResults: 5,
        timeoutSeconds: 30,
        cacheTtlMinutes: 15,
      },
      fetch: {
        enabled: true,
        provider: "firecrawl", // optional; omit for auto-detect
        maxChars: 50000,
        maxCharsCap: 50000,
        maxResponseBytes: 2000000,
        timeoutSeconds: 30,
        cacheTtlMinutes: 15,
        maxRedirects: 3,
        readability: true,
        userAgent: "custom-ua",
      },
    },
  },
}
```

### `tools.media`

Configures inbound media understanding (image/audio/video):

```
{
  tools: {
    media: {
      concurrency: 2,
      asyncCompletion: {
        directSend: false, // opt-in: send finished async video directly to the channel
      },
      audio: {
        enabled: true,
        maxBytes: 20971520,
        scope: {
          default: "deny",
          rules: [{ action: "allow", match: { chatType: "direct" } }],
        },
        models: [\
          { provider: "openai", model: "gpt-4o-mini-transcribe" },\
          { type: "cli", command: "whisper", args: ["--model", "base", "{{MediaPath}}"] },\
        ],
      },
      image: {
        enabled: true,
        timeoutSeconds: 180,
        models: [{ provider: "ollama", model: "gemma4:26b", timeoutSeconds: 300 }],
      },
      video: {
        enabled: true,
        maxBytes: 52428800,
        models: [{ provider: "google", model: "gemini-3-flash-preview" }],
      },
    },
  },
}
```

Media model entry fields

**Provider entry** (`type: "provider"` or omitted):

- `provider`: API provider id (`openai`, `anthropic`, `google`/`gemini`, `groq`, etc.)
- `model`: model id override
- `profile` / `preferredProfile`: `auth-profiles.json` profile selection

**CLI entry** (`type: "cli"`):

- `command`: executable to run
- `args`: templated args (supports `{{MediaPath}}`, `{{Prompt}}`, `{{MaxChars}}`, etc.; `openclaw doctor --fix` migrates deprecated `{input}` placeholders to `{{MediaPath}}`)

**Common fields:**

- `capabilities`: optional list (`image`, `audio`, `video`). Defaults: `openai`/`anthropic`/`minimax` → image, `google` → image+audio+video, `groq` → audio.
- `prompt`, `maxChars`, `maxBytes`, `timeoutSeconds`, `language`: per-entry overrides.
- `tools.media.image.timeoutSeconds` and matching image model `timeoutSeconds` entries also apply when the agent calls the explicit `image` tool.
- Failures fall back to the next entry.

Provider auth follows standard order: `auth-profiles.json` → env vars → `models.providers.*.apiKey`.**Async completion fields:**

- `asyncCompletion.directSend`: when `true`, completed async media tasks that support direct completion delivery try direct channel delivery first. Default: `false` (requester-session wake/model-delivery path). Today this applies to async `video_generate`; async `music_generate` completions stay requester-session mediated even when this is enabled.

### `tools.agentToAgent`

```
{
  tools: {
    agentToAgent: {
      enabled: false,
      allow: ["home", "work"],
    },
  },
}
```

### `tools.sessions`

Controls which sessions can be targeted by the session tools (`sessions_list`, `sessions_history`, `sessions_send`).Default: `tree` (current session + sessions spawned by it, such as subagents).

```
{
  tools: {
    sessions: {
      // "self" | "tree" | "agent" | "all"
      visibility: "tree",
    },
  },
}
```

Visibility scopes

- `self`: only the current session key.
- `tree`: current session + sessions spawned by the current session (subagents).
- `agent`: any session belonging to the current agent id (can include other users if you run per-sender sessions under the same agent id).
- `all`: any session. Cross-agent targeting still requires `tools.agentToAgent`.
- Sandbox clamp: when the current session is sandboxed and `agents.defaults.sandbox.sessionToolsVisibility="spawned"`, visibility is forced to `tree` even if `tools.sessions.visibility="all"`.

### `tools.sessions_spawn`

Controls inline attachment support for `sessions_spawn`.

```
{
  tools: {
    sessions_spawn: {
      attachments: {
        enabled: false, // opt-in: set true to allow inline file attachments
        maxTotalBytes: 5242880, // 5 MB total across all files
        maxFiles: 50,
        maxFileBytes: 1048576, // 1 MB per file
        retainOnSessionKeep: false, // keep attachments when cleanup="keep"
      },
    },
  },
}
```

Attachment notes

- Attachments are only supported for `runtime: "subagent"`. ACP runtime rejects them.
- Files are materialized into the child workspace at `.openclaw/attachments/<uuid>/` with a `.manifest.json`.
- Attachment content is automatically redacted from transcript persistence.
- Base64 inputs are validated with strict alphabet/padding checks and a pre-decode size guard.
- File permissions are `0700` for directories and `0600` for files.
- Cleanup follows the `cleanup` policy: `delete` always removes attachments; `keep` retains them only when `retainOnSessionKeep: true`.

### `tools.experimental`

Experimental built-in tool flags. Default off unless a strict-agentic GPT-5 auto-enable rule applies.

```
{
  tools: {
    experimental: {
      planTool: true, // enable experimental update_plan
    },
  },
}
```

- `planTool`: enables the structured `update_plan` tool for non-trivial multi-step work tracking.
- Default: `false` unless `agents.defaults.embeddedPi.executionContract` (or a per-agent override) is set to `"strict-agentic"` for an OpenAI or OpenAI Codex GPT-5-family run. Set `true` to force the tool on outside that scope, or `false` to keep it off even for strict-agentic GPT-5 runs.
- When enabled, the system prompt also adds usage guidance so the model only uses it for substantial work and keeps at most one step `in_progress`.

### `agents.defaults.subagents`

```
{
  agents: {
    defaults: {
      subagents: {
        allowAgents: ["research"],
        model: "minimax/MiniMax-M2.7",
        maxConcurrent: 8,
        runTimeoutSeconds: 900,
        archiveAfterMinutes: 60,
      },
    },
  },
}
```

- `model`: default model for spawned sub-agents. If omitted, sub-agents inherit the caller’s model.
- `allowAgents`: default allowlist of target agent ids for `sessions_spawn` when the requester agent does not set its own `subagents.allowAgents` (`["*"]` = any; default: same agent only).
- `runTimeoutSeconds`: default timeout (seconds) for `sessions_spawn` when the tool call omits `runTimeoutSeconds`. `0` means no timeout.
- Per-subagent tool policy: `tools.subagents.tools.allow` / `tools.subagents.tools.deny`.

* * *

## Custom providers and base URLs

OpenClaw uses the built-in model catalog. Add custom providers via `models.providers` in config or `~/.openclaw/agents/<agentId>/agent/models.json`.

```
{
  models: {
    mode: "merge", // merge (default) | replace
    providers: {
      "custom-proxy": {
        baseUrl: "http://localhost:4000/v1",
        apiKey: "LITELLM_KEY",
        api: "openai-completions", // openai-completions | openai-responses | anthropic-messages | google-generative-ai
        models: [\
          {\
            id: "llama-3.1-8b",\
            name: "Llama 3.1 8B",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 128000,\
            contextTokens: 96000,\
            maxTokens: 32000,\
          },\
        ],
      },
    },
  },
}
```

Auth and merge precedence

- Use `authHeader: true` \+ `headers` for custom auth needs.
- Override agent config root with `OPENCLAW_AGENT_DIR` (or `PI_CODING_AGENT_DIR`, a legacy environment variable alias).
- Merge precedence for matching provider IDs:
  - Non-empty agent `models.json``baseUrl` values win.
  - Non-empty agent `apiKey` values win only when that provider is not SecretRef-managed in current config/auth-profile context.
  - SecretRef-managed provider `apiKey` values are refreshed from source markers (`ENV_VAR_NAME` for env refs, `secretref-managed` for file/exec refs) instead of persisting resolved secrets.
  - SecretRef-managed provider header values are refreshed from source markers (`secretref-env:ENV_VAR_NAME` for env refs, `secretref-managed` for file/exec refs).
  - Empty or missing agent `apiKey`/`baseUrl` fall back to `models.providers` in config.
  - Matching model `contextWindow`/`maxTokens` use the higher value between explicit config and implicit catalog values.
  - Matching model `contextTokens` preserves an explicit runtime cap when present; use it to limit effective context without changing native model metadata.
  - Use `models.mode: "replace"` when you want config to fully rewrite `models.json`.
  - Marker persistence is source-authoritative: markers are written from the active source config snapshot (pre-resolution), not from resolved runtime secret values.

### Provider field details

Top-level catalog

- `models.mode`: provider catalog behavior (`merge` or `replace`).
- `models.providers`: custom provider map keyed by provider id.

  - Safe edits: use `openclaw config set models.providers.<id> '<json>' --strict-json --merge` or `openclaw config set models.providers.<id>.models '<json-array>' --strict-json --merge` for additive updates. `config set` refuses destructive replacements unless you pass `--replace`.

Provider connection and auth

- `models.providers.*.api`: request adapter (`openai-completions`, `openai-responses`, `anthropic-messages`, `google-generative-ai`, etc). For self-hosted `/v1/chat/completions` backends such as MLX, vLLM, SGLang, and most OpenAI-compatible local servers, use `openai-completions`. A custom provider with `baseUrl` but no `api` defaults to `openai-completions`; set `openai-responses` only when the backend supports `/v1/responses`.
- `models.providers.*.apiKey`: provider credential (prefer SecretRef/env substitution).
- `models.providers.*.auth`: auth strategy (`api-key`, `token`, `oauth`, `aws-sdk`).
- `models.providers.*.contextWindow`: default native context window for models under this provider when the model entry does not set `contextWindow`.
- `models.providers.*.contextTokens`: default effective runtime context cap for models under this provider when the model entry does not set `contextTokens`.
- `models.providers.*.maxTokens`: default output-token cap for models under this provider when the model entry does not set `maxTokens`.
- `models.providers.*.timeoutSeconds`: optional per-provider model HTTP request timeout in seconds, including connect, headers, body, and total request abort handling.
- `models.providers.*.injectNumCtxForOpenAICompat`: for Ollama + `openai-completions`, inject `options.num_ctx` into requests (default: `true`).
- `models.providers.*.authHeader`: force credential transport in the `Authorization` header when required.
- `models.providers.*.baseUrl`: upstream API base URL.
- `models.providers.*.headers`: extra static headers for proxy/tenant routing.

Request transport overrides

`models.providers.*.request`: transport overrides for model-provider HTTP requests.

- `request.headers`: extra headers (merged with provider defaults). Values accept SecretRef.
- `request.auth`: auth strategy override. Modes: `"provider-default"` (use provider’s built-in auth), `"authorization-bearer"` (with `token`), `"header"` (with `headerName`, `value`, optional `prefix`).
- `request.proxy`: HTTP proxy override. Modes: `"env-proxy"` (use `HTTP_PROXY`/`HTTPS_PROXY` env vars), `"explicit-proxy"` (with `url`). Both modes accept an optional `tls` sub-object.
- `request.tls`: TLS override for direct connections. Fields: `ca`, `cert`, `key`, `passphrase` (all accept SecretRef), `serverName`, `insecureSkipVerify`.
- `request.allowPrivateNetwork`: when `true`, allow HTTPS to `baseUrl` when DNS resolves to private, CGNAT, or similar ranges, via the provider HTTP fetch guard (operator opt-in for trusted self-hosted OpenAI-compatible endpoints). Loopback model-provider stream URLs such as `localhost`, `127.0.0.1`, and `[::1]` are allowed automatically unless this is explicitly set to `false`; LAN, tailnet, and private DNS hosts still require opt-in. WebSocket uses the same `request` for headers/TLS but not that fetch SSRF gate. Default `false`.

Model catalog entries

- `models.providers.*.models`: explicit provider model catalog entries.
- `models.providers.*.models.*.input`: model input modalities. Use `["text"]` for text-only models and `["text", "image"]` for native image/vision models. Image attachments are only injected into agent turns when the selected model is marked image-capable.
- `models.providers.*.models.*.contextWindow`: native model context window metadata. This overrides provider-level `contextWindow` for that model.
- `models.providers.*.models.*.contextTokens`: optional runtime context cap. This overrides provider-level `contextTokens`; use it when you want a smaller effective context budget than the model’s native `contextWindow`; `openclaw models list` shows both values when they differ.
- `models.providers.*.models.*.compat.supportsDeveloperRole`: optional compatibility hint. For `api: "openai-completions"` with a non-empty non-native `baseUrl` (host not `api.openai.com`), OpenClaw forces this to `false` at runtime. Empty/omitted `baseUrl` keeps default OpenAI behavior.
- `models.providers.*.models.*.compat.requiresStringContent`: optional compatibility hint for string-only OpenAI-compatible chat endpoints. When `true`, OpenClaw flattens pure text `messages[].content` arrays into plain strings before sending the request.

Amazon Bedrock discovery

- `plugins.entries.amazon-bedrock.config.discovery`: Bedrock auto-discovery settings root.
- `plugins.entries.amazon-bedrock.config.discovery.enabled`: turn implicit discovery on/off.
- `plugins.entries.amazon-bedrock.config.discovery.region`: AWS region for discovery.
- `plugins.entries.amazon-bedrock.config.discovery.providerFilter`: optional provider-id filter for targeted discovery.
- `plugins.entries.amazon-bedrock.config.discovery.refreshInterval`: polling interval for discovery refresh.
- `plugins.entries.amazon-bedrock.config.discovery.defaultContextWindow`: fallback context window for discovered models.
- `plugins.entries.amazon-bedrock.config.discovery.defaultMaxTokens`: fallback max output tokens for discovered models.

Interactive custom-provider onboarding infers image input for common vision model IDs such as GPT-4o, Claude, Gemini, Qwen-VL, LLaVA, Pixtral, InternVL, Mllama, MiniCPM-V, and GLM-4V, and skips the extra question for known text-only families. Unknown model IDs still prompt for image support. Non-interactive onboarding uses the same inference; pass `--custom-image-input` to force image-capable metadata or `--custom-text-input` to force text-only metadata.

### Provider examples

Cerebras (GLM 4.7 / GPT OSS)

The bundled `cerebras` provider plugin can configure this via `openclaw onboard --auth-choice cerebras-api-key`. Use explicit provider config only when overriding defaults.

```
{
  env: { CEREBRAS_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: {
        primary: "cerebras/zai-glm-4.7",
        fallbacks: ["cerebras/gpt-oss-120b"],
      },
      models: {
        "cerebras/zai-glm-4.7": { alias: "GLM 4.7 (Cerebras)" },
        "cerebras/gpt-oss-120b": { alias: "GPT OSS 120B (Cerebras)" },
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      cerebras: {
        baseUrl: "https://api.cerebras.ai/v1",
        apiKey: "${CEREBRAS_API_KEY}",
        api: "openai-completions",
        models: [\
          { id: "zai-glm-4.7", name: "GLM 4.7 (Cerebras)" },\
          { id: "gpt-oss-120b", name: "GPT OSS 120B (Cerebras)" },\
        ],
      },
    },
  },
}
```

Use `cerebras/zai-glm-4.7` for Cerebras; `zai/glm-4.7` for Z.AI direct.

Kimi Coding

```
{
  env: { KIMI_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "kimi/kimi-code" },
      models: { "kimi/kimi-code": { alias: "Kimi Code" } },
    },
  },
}
```

Anthropic-compatible, built-in provider. Shortcut: `openclaw onboard --auth-choice kimi-code-api-key`.

Local models (LM Studio)

See [Local Models](https://docs.openclaw.ai/gateway/local-models). TL;DR: run a large local model via LM Studio Responses API on serious hardware; keep hosted models merged for fallback.

MiniMax M2.7 (direct)

```
{
  agents: {
    defaults: {
      model: { primary: "minimax/MiniMax-M2.7" },
      models: {
        "minimax/MiniMax-M2.7": { alias: "Minimax" },
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      minimax: {
        baseUrl: "https://api.minimax.io/anthropic",
        apiKey: "${MINIMAX_API_KEY}",
        api: "anthropic-messages",
        models: [\
          {\
            id: "MiniMax-M2.7",\
            name: "MiniMax M2.7",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0.3, output: 1.2, cacheRead: 0.06, cacheWrite: 0.375 },\
            contextWindow: 204800,\
            maxTokens: 131072,\
          },\
        ],
      },
    },
  },
}
```

Set `MINIMAX_API_KEY`. Shortcuts: `openclaw onboard --auth-choice minimax-global-api` or `openclaw onboard --auth-choice minimax-cn-api`. The model catalog defaults to M2.7 only. On the Anthropic-compatible streaming path, OpenClaw disables MiniMax thinking by default unless you explicitly set `thinking` yourself. `/fast on` or `params.fastMode: true` rewrites `MiniMax-M2.7` to `MiniMax-M2.7-highspeed`.

Moonshot AI (Kimi)

```
{
  env: { MOONSHOT_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "moonshot/kimi-k2.6" },
      models: { "moonshot/kimi-k2.6": { alias: "Kimi K2.6" } },
    },
  },
  models: {
    mode: "merge",
    providers: {
      moonshot: {
        baseUrl: "https://api.moonshot.ai/v1",
        apiKey: "${MOONSHOT_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "kimi-k2.6",\
            name: "Kimi K2.6",\
            reasoning: false,\
            input: ["text", "image"],\
            cost: { input: 0.95, output: 4, cacheRead: 0.16, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 262144,\
          },\
        ],
      },
    },
  },
}
```

For the China endpoint: `baseUrl: "https://api.moonshot.cn/v1"` or `openclaw onboard --auth-choice moonshot-api-key-cn`.Native Moonshot endpoints advertise streaming usage compatibility on the shared `openai-completions` transport, and OpenClaw keys that off endpoint capabilities rather than the built-in provider id alone.

OpenCode

```
{
  agents: {
    defaults: {
      model: { primary: "opencode/claude-opus-4-6" },
      models: { "opencode/claude-opus-4-6": { alias: "Opus" } },
    },
  },
}
```

Set `OPENCODE_API_KEY` (or `OPENCODE_ZEN_API_KEY`). Use `opencode/...` refs for the Zen catalog or `opencode-go/...` refs for the Go catalog. Shortcut: `openclaw onboard --auth-choice opencode-zen` or `openclaw onboard --auth-choice opencode-go`.

Synthetic (Anthropic-compatible)

```
{
  env: { SYNTHETIC_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "synthetic/hf:MiniMaxAI/MiniMax-M2.5" },
      models: { "synthetic/hf:MiniMaxAI/MiniMax-M2.5": { alias: "MiniMax M2.5" } },
    },
  },
  models: {
    mode: "merge",
    providers: {
      synthetic: {
        baseUrl: "https://api.synthetic.new/anthropic",
        apiKey: "${SYNTHETIC_API_KEY}",
        api: "anthropic-messages",
        models: [\
          {\
            id: "hf:MiniMaxAI/MiniMax-M2.5",\
            name: "MiniMax M2.5",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 192000,\
            maxTokens: 65536,\
          },\
        ],
      },
    },
  },
}
```

Base URL should omit `/v1` (Anthropic client appends it). Shortcut: `openclaw onboard --auth-choice synthetic-api-key`.

Z.AI (GLM-4.7)

```
{
  agents: {
    defaults: {
      model: { primary: "zai/glm-4.7" },
      models: { "zai/glm-4.7": {} },
    },
  },
}
```

Set `ZAI_API_KEY`. `z.ai/*` and `z-ai/*` are accepted aliases. Shortcut: `openclaw onboard --auth-choice zai-api-key`.

- General endpoint: `https://api.z.ai/api/paas/v4`
- Coding endpoint (default): `https://api.z.ai/api/coding/paas/v4`
- For the general endpoint, define a custom provider with the base URL override.

* * *

## Related

- [Configuration — agents](https://docs.openclaw.ai/gateway/config-agents)
- [Configuration — channels](https://docs.openclaw.ai/gateway/config-channels)
- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference) — other top-level keys
- [Tools and plugins](https://docs.openclaw.ai/tools)

[Configuration — channels](https://docs.openclaw.ai/gateway/config-channels) [Configuration examples](https://docs.openclaw.ai/gateway/configuration-examples)

Ctrl+I

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
is skipped when a candidate contains redacted secret placeholders such as `***`.
When every validation issue is scoped to `plugins.entries.<id>...`, OpenClaw
does not perform whole-file recovery. It keeps the current config active and
surfaces the plugin-local failure so a plugin schema or host-version mismatch
cannot roll back unrelated user settings.

## Common tasks

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

## Config hot reload

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

### Reload modes

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

### What hot-applies vs what needs a restart

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

### Reload planning

When you edit a source file that is referenced through `$include`, OpenClaw plans
the reload from the source-authored layout, not the flattened in-memory view.
That keeps hot-reload decisions (hot-apply vs restart) predictable even when a
single top-level section lives in its own included file such as
`plugins: { $include: "./plugins.json5" }`. Reload planning fails closed if the
source layout is ambiguous.

## Config RPC (programmatic updates)

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

## Environment variables

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

## Full reference

For the complete field-by-field reference, see **[Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference)**.

* * *

_Related: [Configuration Examples](https://docs.openclaw.ai/gateway/configuration-examples) · [Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference) · [Doctor](https://docs.openclaw.ai/gateway/doctor)_

## Related

- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference)
- [Configuration examples](https://docs.openclaw.ai/gateway/configuration-examples)
- [Gateway runbook](https://docs.openclaw.ai/gateway)

[Gateway runbook](https://docs.openclaw.ai/gateway) [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference)

Ctrl+I

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
      highWaterBytes: "400mb", // optional (defaults to 80% of maxDiskBytes)
    },
    typingIntervalSeconds: 5,
    sendPolicy: {
      default: "allow",
      rules: [{ action: "deny", match: { channel: "discord", chatType: "group" } }],
    },
  },

  // Channels
  channels: {
    whatsapp: {
      dmPolicy: "pairing",
      allowFrom: ["+15555550123"],
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },

    telegram: {
      enabled: true,
      botToken: "YOUR_TELEGRAM_BOT_TOKEN",
      allowFrom: ["123456789"],
      groupPolicy: "allowlist",
      groupAllowFrom: ["123456789"],
      groups: { "*": { requireMention: true } },
    },

    discord: {
      enabled: true,
      token: "YOUR_DISCORD_BOT_TOKEN",
      dm: { enabled: true, allowFrom: ["123456789012345678"] },
      guilds: {
        "123456789012345678": {
          slug: "friends-of-openclaw",
          requireMention: false,
          channels: {
            general: { allow: true },
            help: { allow: true, requireMention: true },
          },
        },
      },
    },

    slack: {
      enabled: true,
      botToken: "xoxb-REPLACE_ME",
      appToken: "xapp-REPLACE_ME",
      channels: {
        "#general": { allow: true, requireMention: true },
      },
      dm: { enabled: true, allowFrom: ["U123"] },
      slashCommand: {
        enabled: true,
        name: "openclaw",
        sessionPrefix: "slack:slash",
        ephemeral: true,
      },
    },
  },

  // Agent runtime
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      userTimezone: "America/Chicago",
      model: {
        primary: "anthropic/claude-sonnet-4-6",
        fallbacks: ["anthropic/claude-opus-4-6", "openai/gpt-5.4"],
      },
      imageModel: {
        primary: "openrouter/anthropic/claude-sonnet-4-6",
      },
      models: {
        "anthropic/claude-opus-4-6": { alias: "opus" },
        "anthropic/claude-sonnet-4-6": { alias: "sonnet" },
        "openai/gpt-5.4": { alias: "gpt" },
      },
      skills: ["github", "weather"], // inherited by agents that omit list[].skills
      thinkingDefault: "low",
      verboseDefault: "off",
      reasoningDefault: "off",
      elevatedDefault: "on",
      blockStreamingDefault: "off",
      blockStreamingBreak: "text_end",
      blockStreamingChunk: {
        minChars: 800,
        maxChars: 1200,
        breakPreference: "paragraph",
      },
      blockStreamingCoalesce: {
        idleMs: 1000,
      },
      humanDelay: {
        mode: "natural",
      },
      timeoutSeconds: 600,
      mediaMaxMb: 5,
      typingIntervalSeconds: 5,
      maxConcurrent: 3,
      heartbeat: {
        every: "30m",
        model: "anthropic/claude-sonnet-4-6",
        target: "last",
        directPolicy: "allow", // allow (default) | block
        to: "+15555550123",
        prompt: "HEARTBEAT",
        ackMaxChars: 300,
      },
      memorySearch: {
        provider: "gemini",
        model: "gemini-embedding-001",
        remote: {
          apiKey: "${GEMINI_API_KEY}",
        },
        extraPaths: ["../team-docs", "/srv/shared-notes"],
      },
      sandbox: {
        mode: "non-main",
        scope: "session", // preferred over legacy perSession: true
        workspaceRoot: "~/.openclaw/sandboxes",
        docker: {
          image: "openclaw-sandbox:bookworm-slim",
          workdir: "/workspace",
          readOnlyRoot: true,
          tmpfs: ["/tmp", "/var/tmp", "/run"],
          network: "none",
          user: "1000:1000",
        },
        browser: {
          enabled: false,
        },
      },
    },
    list: [\
      {\
        id: "main",\
        default: true,\
        // inherits defaults.skills -> github, weather\
        groupChat: {\
          mentionPatterns: ["@openclaw", "openclaw"],\
        },\
        thinkingDefault: "high", // per-agent thinking override\
        reasoningDefault: "on", // per-agent reasoning visibility\
        fastModeDefault: false, // per-agent fast mode\
      },\
      {\
        id: "quick",\
        skills: [], // no skills for this agent\
        fastModeDefault: true, // this agent always runs fast\
        thinkingDefault: "off",\
      },\
    ],
  },

  tools: {
    allow: ["exec", "process", "read", "write", "edit", "apply_patch"],
    deny: ["browser", "canvas"],
    exec: {
      backgroundMs: 10000,
      timeoutSec: 1800,
      cleanupMs: 1800000,
    },
    elevated: {
      enabled: true,
      allowFrom: {
        whatsapp: ["+15555550123"],
        telegram: ["123456789"],
        discord: ["123456789012345678"],
        slack: ["U123"],
        signal: ["+15555550123"],
        imessage: ["user@example.com"],
        webchat: ["session:demo"],
      },
    },
  },

  // Custom model providers
  models: {
    mode: "merge",
    providers: {
      "custom-proxy": {
        baseUrl: "http://localhost:4000/v1",
        apiKey: "LITELLM_KEY",
        api: "openai-responses",
        authHeader: true,
        headers: { "X-Proxy-Region": "us-west" },
        models: [\
          {\
            id: "llama-3.1-8b",\
            name: "Llama 3.1 8B",\
            api: "openai-responses",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 128000,\
            maxTokens: 32000,\
          },\
        ],
      },
    },
  },

  // Cron jobs
  cron: {
    enabled: true,
    store: "~/.openclaw/cron/cron.json",
    maxConcurrentRuns: 2, // cron dispatch + isolated cron agent-turn execution
    sessionRetention: "24h",
    runLog: {
      maxBytes: "2mb",
      keepLines: 2000,
    },
  },

  // Webhooks
  hooks: {
    enabled: true,
    path: "/hooks",
    token: "shared-secret",
    presets: ["gmail"],
    transformsDir: "~/.openclaw/hooks/transforms",
    mappings: [\
      {\
        id: "gmail-hook",\
        match: { path: "gmail" },\
        action: "agent",\
        wakeMode: "now",\
        name: "Gmail",\
        sessionKey: "hook:gmail:{{messages[0].id}}",\
        messageTemplate: "From: {{messages[0].from}}\nSubject: {{messages[0].subject}}",\
        textTemplate: "{{messages[0].snippet}}",\
        deliver: true,\
        channel: "last",\
        to: "+15555550123",\
        thinking: "low",\
        timeoutSeconds: 300,\
        transform: {\
          module: "gmail.js",\
          export: "transformGmail",\
        },\
      },\
    ],
    gmail: {
      account: "openclaw@gmail.com",
      label: "INBOX",
      topic: "projects/<project-id>/topics/gog-gmail-watch",
      subscription: "gog-gmail-watch-push",
      pushToken: "shared-push-token",
      hookUrl: "http://127.0.0.1:18789/hooks/gmail",
      includeBody: true,
      maxBytes: 20000,
      renewEveryMinutes: 720,
      serve: { bind: "127.0.0.1", port: 8788, path: "/" },
      tailscale: { mode: "funnel", path: "/gmail-pubsub" },
    },
  },

  // Gateway + networking
  gateway: {
    mode: "local",
    port: 18789,
    bind: "loopback",
    controlUi: { enabled: true, basePath: "/openclaw" },
    auth: {
      mode: "token",
      token: "gateway-token",
      allowTailscale: true,
    },
    tailscale: { mode: "serve", resetOnExit: false },
    remote: { url: "ws://gateway.tailnet:18789", token: "remote-token" },
    reload: { mode: "hybrid", debounceMs: 300 },
  },

  skills: {
    allowBundled: ["gemini", "peekaboo"],
    load: {
      extraDirs: ["~/Projects/agent-scripts/skills"],
    },
    install: {
      preferBrew: true,
      nodeManager: "npm", // npm | pnpm | yarn | bun
    },
    entries: {
      "image-lab": {
        enabled: true,
        apiKey: "GEMINI_KEY_HERE",
        env: { GEMINI_API_KEY: "GEMINI_KEY_HERE" },
      },
      peekaboo: { enabled: true },
    },
  },
}
```

## Common patterns

### Shared skill baseline with one override

```
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      skills: ["github", "weather"],
    },
    list: [\
      { id: "main", default: true },\
      { id: "docs", workspace: "~/.openclaw/workspace-docs", skills: ["docs-search"] },\
    ],
  },
}
```

- `agents.defaults.skills` is the shared baseline.
- `agents.list[].skills` replaces that baseline for one agent.
- Use `skills: []` when an agent should see no skills.

### Multi-platform setup

```
{
  agent: { workspace: "~/.openclaw/workspace" },
  channels: {
    whatsapp: { allowFrom: ["+15555550123"] },
    telegram: {
      enabled: true,
      botToken: "YOUR_TOKEN",
      allowFrom: ["123456789"],
    },
    discord: {
      enabled: true,
      token: "YOUR_TOKEN",
      dm: { allowFrom: ["123456789012345678"] },
    },
  },
}
```

### Trusted node network auto-approval

Keep device pairing manual unless you control the network path. For a dedicated
lab or tailnet subnet, you can opt in to first-time node device auto-approval
with exact CIDRs or IPs:

```
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24", "fd00:1234:5678::/64"],
      },
    },
  },
}
```

This remains off when unset. It only applies to fresh `role: node` pairing with
no requested scopes. Operator/browser clients and role, scope, metadata, or
public-key upgrades still require manual approval.

### Secure DM mode (shared inbox / multi-user DMs)

If more than one person can DM your bot (multiple entries in `allowFrom`, pairing approvals for multiple people, or `dmPolicy: "open"`), enable **secure DM mode** so DMs from different senders don’t share one context by default:

```
{
  // Secure DM mode (recommended for multi-user or sensitive DM agents)
  session: { dmScope: "per-channel-peer" },

  channels: {
    // Example: WhatsApp multi-user inbox
    whatsapp: {
      dmPolicy: "allowlist",
      allowFrom: ["+15555550123", "+15555550124"],
    },

    // Example: Discord multi-user inbox
    discord: {
      enabled: true,
      token: "YOUR_DISCORD_BOT_TOKEN",
      dm: { enabled: true, allowFrom: ["123456789012345678", "987654321098765432"] },
    },
  },
}
```

For Discord/Slack/Google Chat/Microsoft Teams/Mattermost/IRC, sender authorization is ID-first by default.
Only enable direct mutable name/email/nick matching with each channel’s `dangerouslyAllowNameMatching: true` if you explicitly accept that risk.

### Anthropic API key + MiniMax fallback

```
{
  auth: {
    profiles: {
      "anthropic:api": {
        provider: "anthropic",
        mode: "api_key",
      },
    },
    order: {
      anthropic: ["anthropic:api"],
    },
  },
  models: {
    providers: {
      minimax: {
        baseUrl: "https://api.minimax.io/anthropic",
        api: "anthropic-messages",
        apiKey: "${MINIMAX_API_KEY}",
      },
    },
  },
  agent: {
    workspace: "~/.openclaw/workspace",
    model: {
      primary: "anthropic/claude-opus-4-6",
      fallbacks: ["minimax/MiniMax-M2.7"],
    },
  },
}
```

### Work bot (restricted access)

```
{
  identity: {
    name: "WorkBot",
    theme: "professional assistant",
  },
  agent: {
    workspace: "~/work-openclaw",
    elevated: { enabled: false },
  },
  channels: {
    slack: {
      enabled: true,
      botToken: "xoxb-...",
      channels: {
        "#engineering": { allow: true, requireMention: true },
        "#general": { allow: true, requireMention: true },
      },
    },
  },
}
```

### Local models only

```
{
  agent: {
    workspace: "~/.openclaw/workspace",
    model: { primary: "lmstudio/my-local-model" },
  },
  models: {
    mode: "merge",
    providers: {
      lmstudio: {
        baseUrl: "http://127.0.0.1:1234/v1",
        apiKey: "lmstudio",
        api: "openai-responses",
        models: [\
          {\
            id: "my-local-model",\
            name: "Local Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 196608,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

## Tips

- If you set `dmPolicy: "open"`, the matching `allowFrom` list must include `"*"`.
- Provider IDs differ (phone numbers, user IDs, channel IDs). Use the provider docs to confirm the format.
- Optional sections to add later: `web`, `browser`, `ui`, `discovery`, `canvasHost`, `talk`, `signal`, `imessage`.
- See [Providers](https://docs.openclaw.ai/providers) and [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting) for deeper setup notes.

## Related

- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)

[Tools and custom providers](https://docs.openclaw.ai/gateway/config-tools) [Authentication](https://docs.openclaw.ai/gateway/authentication)

Ctrl+I

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
consumed by embedded Pi and other runtime adapters. The `openclaw mcp list`,
`show`, `set`, and `unset` commands manage this block without connecting to the
target server during config edits.

```
{
  mcp: {
    // Optional. Default: 600000 ms (10 minutes). Set 0 to disable idle eviction.
    sessionIdleTtlMs: 600000,
    servers: {
      docs: {
        command: "npx",
        args: ["-y", "@modelcontextprotocol/server-fetch"],
      },
      remote: {
        url: "https://example.com/mcp",
        transport: "streamable-http", // streamable-http | sse
        headers: {
          Authorization: "Bearer ${MCP_REMOTE_TOKEN}",
        },
      },
    },
  },
}
```

- `mcp.servers`: named stdio or remote MCP server definitions for runtimes that
expose configured MCP tools.
Remote entries use `transport: "streamable-http"` or `transport: "sse"`;
`type: "http"` is a CLI-native alias that `openclaw mcp set` and
`openclaw doctor --fix` normalize into the canonical `transport` field.
- `mcp.sessionIdleTtlMs`: idle TTL for session-scoped bundled MCP runtimes.
One-shot embedded runs request run-end cleanup; this TTL is the backstop for
long-lived sessions and future callers.
- Changes under `mcp.*` hot-apply by disposing cached session MCP runtimes.
The next tool discovery/use recreates them from the new config, so removed
`mcp.servers` entries are reaped immediately instead of waiting for idle TTL.

See [MCP](https://docs.openclaw.ai/cli/mcp#openclaw-as-an-mcp-client-registry) and
[CLI backends](https://docs.openclaw.ai/gateway/cli-backends#bundle-mcp-overlays) for runtime behavior.

## Skills

```
{
  skills: {
    allowBundled: ["gemini", "peekaboo"],
    load: {
      extraDirs: ["~/Projects/agent-scripts/skills"],
    },
    install: {
      preferBrew: true,
      nodeManager: "npm", // npm | pnpm | yarn | bun
    },
    entries: {
      "image-lab": {
        apiKey: { source: "env", provider: "default", id: "GEMINI_API_KEY" }, // or plaintext string
        env: { GEMINI_API_KEY: "GEMINI_KEY_HERE" },
      },
      peekaboo: { enabled: true },
      sag: { enabled: false },
    },
  },
}
```

- `allowBundled`: optional allowlist for bundled skills only (managed/workspace skills unaffected).
- `load.extraDirs`: extra shared skill roots (lowest precedence).
- `install.preferBrew`: when true, prefer Homebrew installers when `brew` is
available before falling back to other installer kinds.
- `install.nodeManager`: node installer preference for `metadata.openclaw.install`
specs (`npm` \| `pnpm` \| `yarn` \| `bun`).
- `entries.<skillKey>.enabled: false` disables a skill even if bundled/installed.
- `entries.<skillKey>.apiKey`: convenience for skills declaring a primary env var (plaintext string or SecretRef object).

* * *

## Plugins

```
{
  plugins: {
    enabled: true,
    allow: ["voice-call"],
    deny: [],
    load: {
      paths: ["~/Projects/oss/voice-call-plugin"],
    },
    entries: {
      "voice-call": {
        enabled: true,
        hooks: {
          allowPromptInjection: false,
        },
        config: { provider: "twilio" },
      },
    },
  },
}
```

- Loaded from `~/.openclaw/extensions`, `<workspace>/.openclaw/extensions`, plus `plugins.load.paths`.
- Discovery accepts native OpenClaw plugins plus compatible Codex bundles and Claude bundles, including manifestless Claude default-layout bundles.
- **Config changes require a gateway restart.**
- `allow`: optional allowlist (only listed plugins load). `deny` wins.
- `plugins.entries.<id>.apiKey`: plugin-level API key convenience field (when supported by the plugin).
- `plugins.entries.<id>.env`: plugin-scoped env var map.
- `plugins.entries.<id>.hooks.allowPromptInjection`: when `false`, core blocks `before_prompt_build` and ignores prompt-mutating fields from legacy `before_agent_start`, while preserving legacy `modelOverride` and `providerOverride`. Applies to native plugin hooks and supported bundle-provided hook directories.
- `plugins.entries.<id>.hooks.allowConversationAccess`: when `true`, trusted non-bundled plugins may read raw conversation content from typed hooks such as `llm_input`, `llm_output`, `before_agent_finalize`, and `agent_end`.
- `plugins.entries.<id>.subagent.allowModelOverride`: explicitly trust this plugin to request per-run `provider` and `model` overrides for background subagent runs.
- `plugins.entries.<id>.subagent.allowedModels`: optional allowlist of canonical `provider/model` targets for trusted subagent overrides. Use `"*"` only when you intentionally want to allow any model.
- `plugins.entries.<id>.config`: plugin-defined config object (validated by native OpenClaw plugin schema when available).
- Channel plugin account/runtime settings live under `channels.<id>` and should be described by the owning plugin’s manifest `channelConfigs` metadata, not by a central OpenClaw option registry.
- `plugins.entries.firecrawl.config.webFetch`: Firecrawl web-fetch provider settings.

  - `apiKey`: Firecrawl API key (accepts SecretRef). Falls back to `plugins.entries.firecrawl.config.webSearch.apiKey`, legacy `tools.web.fetch.firecrawl.apiKey`, or `FIRECRAWL_API_KEY` env var.
  - `baseUrl`: Firecrawl API base URL (default: `https://api.firecrawl.dev`; self-hosted overrides must target private/internal endpoints).
  - `onlyMainContent`: extract only the main content from pages (default: `true`).
  - `maxAgeMs`: maximum cache age in milliseconds (default: `172800000` / 2 days).
  - `timeoutSeconds`: scrape request timeout in seconds (default: `60`).
- `plugins.entries.xai.config.xSearch`: xAI X Search (Grok web search) settings.

  - `enabled`: enable the X Search provider.
  - `model`: Grok model to use for search (e.g. `"grok-4-1-fast"`).
- `plugins.entries.memory-core.config.dreaming`: memory dreaming settings. See [Dreaming](https://docs.openclaw.ai/concepts/dreaming) for phases and thresholds.

  - `enabled`: master dreaming switch (default `false`).
  - `frequency`: cron cadence for each full dreaming sweep (`"0 3 * * *"` by default).
  - `model`: optional Dream Diary subagent model override. Requires `plugins.entries.memory-core.subagent.allowModelOverride: true`; pair with `allowedModels` to restrict targets. Model-unavailable errors retry once with the session default model; trust or allowlist failures do not fall back silently.
  - phase policy and thresholds are implementation details (not user-facing config keys).
- Full memory config lives in [Memory configuration reference](https://docs.openclaw.ai/reference/memory-config):

  - `agents.defaults.memorySearch.*`
  - `memory.backend`
  - `memory.citations`
  - `memory.qmd.*`
  - `plugins.entries.memory-core.config.dreaming`
- Enabled Claude bundle plugins can also contribute embedded Pi defaults from `settings.json`; OpenClaw applies those as sanitized agent settings, not as raw OpenClaw config patches.
- `plugins.slots.memory`: pick the active memory plugin id, or `"none"` to disable memory plugins.
- `plugins.slots.contextEngine`: pick the active context engine plugin id; defaults to `"legacy"` unless you install and select another engine.

See [Plugins](https://docs.openclaw.ai/tools/plugin).

* * *

## Commitments

`commitments` controls inferred follow-up memory: OpenClaw can detect check-ins from conversation turns and deliver them through heartbeat runs.

- `commitments.enabled`: enable hidden LLM extraction, storage, and heartbeat delivery for inferred follow-up commitments. Default: `false`.
- `commitments.maxPerDay`: maximum inferred follow-up commitments delivered per agent session in a rolling day. Default: `3`.

See [Inferred commitments](https://docs.openclaw.ai/concepts/commitments).

* * *

## Browser

```
{
  browser: {
    enabled: true,
    evaluateEnabled: true,
    defaultProfile: "user",
    ssrfPolicy: {
      // dangerouslyAllowPrivateNetwork: true, // opt in only for trusted private-network access
      // allowPrivateNetwork: true, // legacy alias
      // hostnameAllowlist: ["*.example.com", "example.com"],
      // allowedHostnames: ["localhost"],
    },
    tabCleanup: {
      enabled: true,
      idleMinutes: 120,
      maxTabsPerSession: 8,
      sweepMinutes: 5,
    },
    profiles: {
      openclaw: { cdpPort: 18800, color: "#FF4500" },
      work: {
        cdpPort: 18801,
        color: "#0066CC",
        executablePath: "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",
      },
      user: { driver: "existing-session", attachOnly: true, color: "#00AA00" },
      brave: {
        driver: "existing-session",
        attachOnly: true,
        userDataDir: "~/Library/Application Support/BraveSoftware/Brave-Browser",
        color: "#FB542B",
      },
      remote: { cdpUrl: "http://10.0.0.42:9222", color: "#00AA00" },
    },
    color: "#FF4500",
    // headless: false,
    // noSandbox: false,
    // extraArgs: [],
    // executablePath: "/Applications/Brave Browser.app/Contents/MacOS/Brave Browser",
    // attachOnly: false,
  },
}
```

- `evaluateEnabled: false` disables `act:evaluate` and `wait --fn`.
- `tabCleanup` reclaims tracked primary-agent tabs after idle time or when a
session exceeds its cap. Set `idleMinutes: 0` or `maxTabsPerSession: 0` to
disable those individual cleanup modes.
- `ssrfPolicy.dangerouslyAllowPrivateNetwork` is disabled when unset, so browser navigation stays strict by default.
- Set `ssrfPolicy.dangerouslyAllowPrivateNetwork: true` only when you intentionally trust private-network browser navigation.
- In strict mode, remote CDP profile endpoints (`profiles.*.cdpUrl`) are subject to the same private-network blocking during reachability/discovery checks.
- `ssrfPolicy.allowPrivateNetwork` remains supported as a legacy alias.
- In strict mode, use `ssrfPolicy.hostnameAllowlist` and `ssrfPolicy.allowedHostnames` for explicit exceptions.
- Remote profiles are attach-only (start/stop/reset disabled).
- `profiles.*.cdpUrl` accepts `http://`, `https://`, `ws://`, and `wss://`.
Use HTTP(S) when you want OpenClaw to discover `/json/version`; use WS(S)
when your provider gives you a direct DevTools WebSocket URL.
- `remoteCdpTimeoutMs` and `remoteCdpHandshakeTimeoutMs` apply to remote and
`attachOnly` CDP reachability plus tab-opening requests. Managed loopback
profiles keep local CDP defaults.
- If an externally managed CDP service is reachable through loopback, set that
profile’s `attachOnly: true`; otherwise OpenClaw treats the loopback port as a
local managed browser profile and may report local port ownership errors.
- `existing-session` profiles use Chrome MCP instead of CDP and can attach on
the selected host or through a connected browser node.
- `existing-session` profiles can set `userDataDir` to target a specific
Chromium-based browser profile such as Brave or Edge.
- `existing-session` profiles keep the current Chrome MCP route limits:
snapshot/ref-driven actions instead of CSS-selector targeting, one-file upload
hooks, no dialog timeout overrides, no `wait --load networkidle`, and no
`responsebody`, PDF export, download interception, or batch actions.
- Local managed `openclaw` profiles auto-assign `cdpPort` and `cdpUrl`; only
set `cdpUrl` explicitly for remote CDP.
- Local managed profiles can set `executablePath` to override the global
`browser.executablePath` for that profile. Use this to run one profile in
Chrome and another in Brave.
- Local managed profiles use `browser.localLaunchTimeoutMs` for Chrome CDP HTTP
discovery after process start and `browser.localCdpReadyTimeoutMs` for
post-launch CDP websocket readiness. Raise them on slower hosts where Chrome
starts successfully but readiness checks race startup. Both values must be
positive integers up to `120000` ms; invalid config values are rejected.
- Auto-detect order: default browser if Chromium-based → Chrome → Brave → Edge → Chromium → Chrome Canary.
- `browser.executablePath` and `browser.profiles.<name>.executablePath` both
accept `~` and `~/...` for your OS home directory before Chromium launch.
Per-profile `userDataDir` on `existing-session` profiles is also tilde-expanded.
- Control service: loopback only (port derived from `gateway.port`, default `18791`).
- `extraArgs` appends extra launch flags to local Chromium startup (for example
`--disable-gpu`, window sizing, or debug flags).

* * *

## UI

```
{
  ui: {
    seamColor: "#FF4500",
    assistant: {
      name: "OpenClaw",
      avatar: "CB", // emoji, short text, image URL, or data URI
    },
  },
}
```

- `seamColor`: accent color for native app UI chrome (Talk Mode bubble tint, etc.).
- `assistant`: Control UI identity override. Falls back to active agent identity.

* * *

## Gateway

```
{
  gateway: {
    mode: "local", // local | remote
    port: 18789,
    bind: "loopback",
    auth: {
      mode: "token", // none | token | password | trusted-proxy
      token: "your-token",
      // password: "your-password", // or OPENCLAW_GATEWAY_PASSWORD
      // trustedProxy: { userHeader: "x-forwarded-user" }, // for mode=trusted-proxy; see /gateway/trusted-proxy-auth
      allowTailscale: true,
      rateLimit: {
        maxAttempts: 10,
        windowMs: 60000,
        lockoutMs: 300000,
        exemptLoopback: true,
      },
    },
    tailscale: {
      mode: "off", // off | serve | funnel
      resetOnExit: false,
    },
    controlUi: {
      enabled: true,
      basePath: "/openclaw",
      // root: "dist/control-ui",
      // embedSandbox: "scripts", // strict | scripts | trusted
      // allowExternalEmbedUrls: false, // dangerous: allow absolute external http(s) embed URLs
      // chatMessageMaxWidth: "min(1280px, 82%)", // optional grouped chat message max-width
      // allowedOrigins: ["https://control.example.com"], // required for non-loopback Control UI
      // dangerouslyAllowHostHeaderOriginFallback: false, // dangerous Host-header origin fallback mode
      // allowInsecureAuth: false,
      // dangerouslyDisableDeviceAuth: false,
    },
    remote: {
      url: "ws://gateway.tailnet:18789",
      transport: "ssh", // ssh | direct
      token: "your-token",
      // password: "your-password",
    },
    trustedProxies: ["10.0.0.1"],
    // Optional. Default false.
    allowRealIpFallback: false,
    nodes: {
      pairing: {
        // Optional. Default unset/disabled.
        autoApproveCidrs: ["192.168.1.0/24", "fd00:1234:5678::/64"],
      },
      allowCommands: ["canvas.navigate"],
      denyCommands: ["system.run"],
    },
    tools: {
      // Additional /tools/invoke HTTP denies
      deny: ["browser"],
      // Remove tools from the default HTTP deny list
      allow: ["gateway"],
    },
    push: {
      apns: {
        relay: {
          baseUrl: "https://relay.example.com",
          timeoutMs: 10000,
        },
      },
    },
  },
}
```

Gateway field details

- `mode`: `local` (run gateway) or `remote` (connect to remote gateway). Gateway refuses to start unless `local`.
- `port`: single multiplexed port for WS + HTTP. Precedence: `--port` \> `OPENCLAW_GATEWAY_PORT` \> `gateway.port` \> `18789`.
- `bind`: `auto`, `loopback` (default), `lan` (`0.0.0.0`), `tailnet` (Tailscale IP only), or `custom`.
- **Legacy bind aliases**: use bind mode values in `gateway.bind` (`auto`, `loopback`, `lan`, `tailnet`, `custom`), not host aliases (`0.0.0.0`, `127.0.0.1`, `localhost`, `::`, `::1`).
- **Docker note**: the default `loopback` bind listens on `127.0.0.1` inside the container. With Docker bridge networking (`-p 18789:18789`), traffic arrives on `eth0`, so the gateway is unreachable. Use `--network host`, or set `bind: "lan"` (or `bind: "custom"` with `customBindHost: "0.0.0.0"`) to listen on all interfaces.
- **Auth**: required by default. Non-loopback binds require gateway auth. In practice that means a shared token/password or an identity-aware reverse proxy with `gateway.auth.mode: "trusted-proxy"`. Onboarding wizard generates a token by default.
- If both `gateway.auth.token` and `gateway.auth.password` are configured (including SecretRefs), set `gateway.auth.mode` explicitly to `token` or `password`. Startup and service install/repair flows fail when both are configured and mode is unset.
- `gateway.auth.mode: "none"`: explicit no-auth mode. Use only for trusted local loopback setups; this is intentionally not offered by onboarding prompts.
- `gateway.auth.mode: "trusted-proxy"`: delegate browser/user auth to an identity-aware reverse proxy and trust identity headers from `gateway.trustedProxies` (see [Trusted Proxy Auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth)). This mode expects a **non-loopback** proxy source by default; same-host loopback reverse proxies require explicit `gateway.auth.trustedProxy.allowLoopback = true`. Internal same-host callers can use `gateway.auth.password` as a local direct fallback; `gateway.auth.token` remains mutually exclusive with trusted-proxy mode.
- `gateway.auth.allowTailscale`: when `true`, Tailscale Serve identity headers can satisfy Control UI/WebSocket auth (verified via `tailscale whois`). HTTP API endpoints do **not** use that Tailscale header auth; they follow the gateway’s normal HTTP auth mode instead. This tokenless flow assumes the gateway host is trusted. Defaults to `true` when `tailscale.mode = "serve"`.
- `gateway.auth.rateLimit`: optional failed-auth limiter. Applies per client IP and per auth scope (shared-secret and device-token are tracked independently). Blocked attempts return `429` \+ `Retry-After`.

  - On the async Tailscale Serve Control UI path, failed attempts for the same `{scope, clientIp}` are serialized before the failure write. Concurrent bad attempts from the same client can therefore trip the limiter on the second request instead of both racing through as plain mismatches.
  - `gateway.auth.rateLimit.exemptLoopback` defaults to `true`; set `false` when you intentionally want localhost traffic rate-limited too (for test setups or strict proxy deployments).
- Browser-origin WS auth attempts are always throttled with loopback exemption disabled (defense-in-depth against browser-based localhost brute force).
- On loopback, those browser-origin lockouts are isolated per normalized `Origin`
value, so repeated failures from one localhost origin do not automatically
lock out a different origin.
- `tailscale.mode`: `serve` (tailnet only, loopback bind) or `funnel` (public, requires auth).
- `controlUi.allowedOrigins`: explicit browser-origin allowlist for Gateway WebSocket connects. Required when browser clients are expected from non-loopback origins.
- `controlUi.chatMessageMaxWidth`: optional max-width for grouped Control UI chat messages. Accepts constrained CSS width values such as `960px`, `82%`, `min(1280px, 82%)`, and `calc(100% - 2rem)`.
- `controlUi.dangerouslyAllowHostHeaderOriginFallback`: dangerous mode that enables Host-header origin fallback for deployments that intentionally rely on Host-header origin policy.
- `remote.transport`: `ssh` (default) or `direct` (ws/wss). For `direct`, `remote.url` must be `ws://` or `wss://`.
- `OPENCLAW_ALLOW_INSECURE_PRIVATE_WS=1`: client-side process-environment
break-glass override that allows plaintext `ws://` to trusted private-network
IPs; default remains loopback-only for plaintext. There is no `openclaw.json`
equivalent, and browser private-network config such as
`browser.ssrfPolicy.dangerouslyAllowPrivateNetwork` does not affect Gateway
WebSocket clients.
- `gateway.remote.token` / `.password` are remote-client credential fields. They do not configure gateway auth by themselves.
- `gateway.push.apns.relay.baseUrl`: base HTTPS URL for the external APNs relay used by official/TestFlight iOS builds after they publish relay-backed registrations to the gateway. This URL must match the relay URL compiled into the iOS build.
- `gateway.push.apns.relay.timeoutMs`: gateway-to-relay send timeout in milliseconds. Defaults to `10000`.
- Relay-backed registrations are delegated to a specific gateway identity. The paired iOS app fetches `gateway.identity.get`, includes that identity in the relay registration, and forwards a registration-scoped send grant to the gateway. Another gateway cannot reuse that stored registration.
- `OPENCLAW_APNS_RELAY_BASE_URL` / `OPENCLAW_APNS_RELAY_TIMEOUT_MS`: temporary env overrides for the relay config above.
- `OPENCLAW_APNS_RELAY_ALLOW_HTTP=true`: development-only escape hatch for loopback HTTP relay URLs. Production relay URLs should stay on HTTPS.
- `gateway.handshakeTimeoutMs`: pre-auth Gateway WebSocket handshake timeout in milliseconds. Default: `15000`. `OPENCLAW_HANDSHAKE_TIMEOUT_MS` takes precedence when set. Increase this on loaded or low-powered hosts where local clients can connect while startup warmup is still settling.
- `gateway.channelHealthCheckMinutes`: channel health-monitor interval in minutes. Set `0` to disable health-monitor restarts globally. Default: `5`.
- `gateway.channelStaleEventThresholdMinutes`: stale-socket threshold in minutes. Keep this greater than or equal to `gateway.channelHealthCheckMinutes`. Default: `30`.
- `gateway.channelMaxRestartsPerHour`: maximum health-monitor restarts per channel/account in a rolling hour. Default: `10`.
- `channels.<provider>.healthMonitor.enabled`: per-channel opt-out for health-monitor restarts while keeping the global monitor enabled.
- `channels.<provider>.accounts.<accountId>.healthMonitor.enabled`: per-account override for multi-account channels. When set, it takes precedence over the channel-level override.
- Local gateway call paths can use `gateway.remote.*` as fallback only when `gateway.auth.*` is unset.
- If `gateway.auth.token` / `gateway.auth.password` is explicitly configured via SecretRef and unresolved, resolution fails closed (no remote fallback masking).
- `trustedProxies`: reverse proxy IPs that terminate TLS or inject forwarded-client headers. Only list proxies you control. Loopback entries are still valid for same-host proxy/local-detection setups (for example Tailscale Serve or a local reverse proxy), but they do **not** make loopback requests eligible for `gateway.auth.mode: "trusted-proxy"`.
- `allowRealIpFallback`: when `true`, the gateway accepts `X-Real-IP` if `X-Forwarded-For` is missing. Default `false` for fail-closed behavior.
- `gateway.nodes.pairing.autoApproveCidrs`: optional CIDR/IP allowlist for auto-approving first-time node device pairing with no requested scopes. It is disabled when unset. This does not auto-approve operator/browser/Control UI/WebChat pairing, and it does not auto-approve role, scope, metadata, or public-key upgrades.
- `gateway.nodes.allowCommands` / `gateway.nodes.denyCommands`: global allow/deny shaping for declared node commands after pairing and platform allowlist evaluation. Use `allowCommands` to opt into dangerous node commands such as `camera.snap`, `camera.clip`, and `screen.record`; `denyCommands` removes a command even if a platform default or explicit allow would otherwise include it. After a node changes its declared command list, reject and re-approve that device pairing so the gateway stores the updated command snapshot.
- `gateway.tools.deny`: extra tool names blocked for HTTP `POST /tools/invoke` (extends default deny list).
- `gateway.tools.allow`: remove tool names from the default HTTP deny list.

### OpenAI-compatible endpoints

- Chat Completions: disabled by default. Enable with `gateway.http.endpoints.chatCompletions.enabled: true`.
- Responses API: `gateway.http.endpoints.responses.enabled`.
- Responses URL-input hardening:
  - `gateway.http.endpoints.responses.maxUrlParts`
  - `gateway.http.endpoints.responses.files.urlAllowlist`
  - `gateway.http.endpoints.responses.images.urlAllowlist`
    Empty allowlists are treated as unset; use `gateway.http.endpoints.responses.files.allowUrl=false`
    and/or `gateway.http.endpoints.responses.images.allowUrl=false` to disable URL fetching.
- Optional response hardening header:
  - `gateway.http.securityHeaders.strictTransportSecurity` (set only for HTTPS origins you control; see [Trusted Proxy Auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth#tls-termination-and-hsts))

### Multi-instance isolation

Run multiple gateways on one host with unique ports and state dirs:

```
OPENCLAW_CONFIG_PATH=~/.openclaw/a.json \
OPENCLAW_STATE_DIR=~/.openclaw-a \
openclaw gateway --port 19001
```

Convenience flags: `--dev` (uses `~/.openclaw-dev` \+ port `19001`), `--profile <name>` (uses `~/.openclaw-<name>`).See [Multiple Gateways](https://docs.openclaw.ai/gateway/multiple-gateways).

### `gateway.tls`

```
{
  gateway: {
    tls: {
      enabled: false,
      autoGenerate: false,
      certPath: "/etc/openclaw/tls/server.crt",
      keyPath: "/etc/openclaw/tls/server.key",
      caPath: "/etc/openclaw/tls/ca-bundle.crt",
    },
  },
}
```

- `enabled`: enables TLS termination at the gateway listener (HTTPS/WSS) (default: `false`).
- `autoGenerate`: auto-generates a local self-signed cert/key pair when explicit files are not configured; for local/dev use only.
- `certPath`: filesystem path to the TLS certificate file.
- `keyPath`: filesystem path to the TLS private key file; keep permission-restricted.
- `caPath`: optional CA bundle path for client verification or custom trust chains.

### `gateway.reload`

```
{
  gateway: {
    reload: {
      mode: "hybrid", // off | restart | hot | hybrid
      debounceMs: 500,
      deferralTimeoutMs: 300000,
    },
  },
}
```

- `mode`: controls how config edits are applied at runtime.

  - `"off"`: ignore live edits; changes require an explicit restart.
  - `"restart"`: always restart the gateway process on config change.
  - `"hot"`: apply changes in-process without restarting.
  - `"hybrid"` (default): try hot reload first; fall back to restart if required.
- `debounceMs`: debounce window in ms before config changes are applied (non-negative integer).
- `deferralTimeoutMs`: optional maximum time in ms to wait for in-flight operations before forcing a restart. Omit it to use the default bounded wait (`300000`); set `0` to wait indefinitely and log periodic still-pending warnings.

* * *

## Hooks

```
{
  hooks: {
    enabled: true,
    token: "shared-secret",
    path: "/hooks",
    maxBodyBytes: 262144,
    defaultSessionKey: "hook:ingress",
    allowRequestSessionKey: true,
    allowedSessionKeyPrefixes: ["hook:", "hook:gmail:"],
    allowedAgentIds: ["hooks", "main"],
    presets: ["gmail"],
    transformsDir: "~/.openclaw/hooks/transforms",
    mappings: [\
      {\
        match: { path: "gmail" },\
        action: "agent",\
        agentId: "hooks",\
        wakeMode: "now",\
        name: "Gmail",\
        sessionKey: "hook:gmail:{{messages[0].id}}",\
        messageTemplate: "From: {{messages[0].from}}\nSubject: {{messages[0].subject}}\n{{messages[0].snippet}}",\
        deliver: true,\
        channel: "last",\
        model: "openai/gpt-5.4-mini",\
      },\
    ],
  },
}
```

Auth: `Authorization: Bearer <token>` or `x-openclaw-token: <token>`.
Query-string hook tokens are rejected.Validation and safety notes:

- `hooks.enabled=true` requires a non-empty `hooks.token`.
- `hooks.token` must be **distinct** from `gateway.auth.token`; reusing the Gateway token is rejected.
- `hooks.path` cannot be `/`; use a dedicated subpath such as `/hooks`.
- If `hooks.allowRequestSessionKey=true`, constrain `hooks.allowedSessionKeyPrefixes` (for example `["hook:"]`).
- If a mapping or preset uses a templated `sessionKey`, set `hooks.allowedSessionKeyPrefixes` and `hooks.allowRequestSessionKey=true`. Static mapping keys do not require that opt-in.

**Endpoints:**

- `POST /hooks/wake` → `{ text, mode?: "now"|"next-heartbeat" }`
- `POST /hooks/agent` → `{ message, name?, agentId?, sessionKey?, wakeMode?, deliver?, channel?, to?, model?, thinking?, timeoutSeconds? }`
  - `sessionKey` from request payload is accepted only when `hooks.allowRequestSessionKey=true` (default: `false`).
- `POST /hooks/<name>` → resolved via `hooks.mappings`
  - Template-rendered mapping `sessionKey` values are treated as externally supplied and also require `hooks.allowRequestSessionKey=true`.

Mapping details

- `match.path` matches sub-path after `/hooks` (e.g. `/hooks/gmail` → `gmail`).
- `match.source` matches a payload field for generic paths.
- Templates like `{{messages[0].subject}}` read from the payload.
- `transform`can point to a JS/TS module returning a hook action.

  - `transform.module` must be a relative path and stays within `hooks.transformsDir` (absolute paths and traversal are rejected).
  - Keep `hooks.transformsDir` under `~/.openclaw/hooks/transforms`; workspace skill directories are rejected. If `openclaw doctor` reports this path as invalid, move the transform module into the hooks transforms directory or remove `hooks.transformsDir`.
- `agentId` routes to a specific agent; unknown IDs fall back to default.
- `allowedAgentIds`: restricts explicit routing (`*` or omitted = allow all, `[]` = deny all).
- `defaultSessionKey`: optional fixed session key for hook agent runs without explicit `sessionKey`.
- `allowRequestSessionKey`: allow `/hooks/agent` callers and template-driven mapping session keys to set `sessionKey` (default: `false`).
- `allowedSessionKeyPrefixes`: optional prefix allowlist for explicit `sessionKey` values (request + mapping), e.g. `["hook:"]`. It becomes required when any mapping or preset uses a templated `sessionKey`.
- `deliver: true` sends final reply to a channel; `channel` defaults to `last`.
- `model` overrides LLM for this hook run (must be allowed if model catalog is set).

### Gmail integration

- The built-in Gmail preset uses `sessionKey: "hook:gmail:{{messages[0].id}}"`.
- If you keep that per-message routing, set `hooks.allowRequestSessionKey: true` and constrain `hooks.allowedSessionKeyPrefixes` to match the Gmail namespace, for example `["hook:", "hook:gmail:"]`.
- If you need `hooks.allowRequestSessionKey: false`, override the preset with a static `sessionKey` instead of the templated default.

```
{
  hooks: {
    gmail: {
      account: "openclaw@gmail.com",
      topic: "projects/<project-id>/topics/gog-gmail-watch",
      subscription: "gog-gmail-watch-push",
      pushToken: "shared-push-token",
      hookUrl: "http://127.0.0.1:18789/hooks/gmail",
      includeBody: true,
      maxBytes: 20000,
      renewEveryMinutes: 720,
      serve: { bind: "127.0.0.1", port: 8788, path: "/" },
      tailscale: { mode: "funnel", path: "/gmail-pubsub" },
      model: "openrouter/meta-llama/llama-3.3-70b-instruct:free",
      thinking: "off",
    },
  },
}
```

- Gateway auto-starts `gog gmail watch serve` on boot when configured. Set `OPENCLAW_SKIP_GMAIL_WATCHER=1` to disable.
- Don’t run a separate `gog gmail watch serve` alongside the Gateway.

* * *

## Canvas host

```
{
  canvasHost: {
    root: "~/.openclaw/workspace/canvas",
    liveReload: true,
    // enabled: false, // or OPENCLAW_SKIP_CANVAS_HOST=1
  },
}
```

- Serves agent-editable HTML/CSS/JS and A2UI over HTTP under the Gateway port:
  - `http://<gateway-host>:<gateway.port>/__openclaw__/canvas/`
  - `http://<gateway-host>:<gateway.port>/__openclaw__/a2ui/`
- Local-only: keep `gateway.bind: "loopback"` (default).
- Non-loopback binds: canvas routes require Gateway auth (token/password/trusted-proxy), same as other Gateway HTTP surfaces.
- Node WebViews typically don’t send auth headers; after a node is paired and connected, the Gateway advertises node-scoped capability URLs for canvas/A2UI access.
- Capability URLs are bound to the active node WS session and expire quickly. IP-based fallback is not used.
- Injects live-reload client into served HTML.
- Auto-creates starter `index.html` when empty.
- Also serves A2UI at `/__openclaw__/a2ui/`.
- Changes require a gateway restart.
- Disable live reload for large directories or `EMFILE` errors.

* * *

## Discovery

### mDNS (Bonjour)

```
{
  discovery: {
    mdns: {
      mode: "minimal", // minimal | full | off
    },
  },
}
```

- `minimal` (default): omit `cliPath` \+ `sshPort` from TXT records.
- `full`: include `cliPath` \+ `sshPort`.
- Hostname defaults to the system hostname when it is a valid DNS label, falling back to `openclaw`. Override with `OPENCLAW_MDNS_HOSTNAME`.

### Wide-area (DNS-SD)

```
{
  discovery: {
    wideArea: { enabled: true },
  },
}
```

Writes a unicast DNS-SD zone under `~/.openclaw/dns/`. For cross-network discovery, pair with a DNS server (CoreDNS recommended) + Tailscale split DNS.Setup: `openclaw dns setup --apply`.

* * *

## Environment

### `env` (inline env vars)

```
{
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
}
```

- Inline env vars are only applied if the process env is missing the key.
- `.env` files: CWD `.env` \+ `~/.openclaw/.env` (neither overrides existing vars).
- `shellEnv`: imports missing expected keys from your login shell profile.
- See [Environment](https://docs.openclaw.ai/help/environment) for full precedence.

### Env var substitution

Reference env vars in any config string with `${VAR_NAME}`:

```
{
  gateway: {
    auth: { token: "${OPENCLAW_GATEWAY_TOKEN}" },
  },
}
```

- Only uppercase names matched: `[A-Z_][A-Z0-9_]*`.
- Missing/empty vars throw an error at config load.
- Escape with `$${VAR}` for a literal `${VAR}`.
- Works with `$include`.

* * *

## Secrets

Secret refs are additive: plaintext values still work.

### `SecretRef`

Use one object shape:

```
{ source: "env" | "file" | "exec", provider: "default", id: "..." }
```

Validation:

- `provider` pattern: `^[a-z][a-z0-9_-]{0,63}$`
- `source: "env"` id pattern: `^[A-Z][A-Z0-9_]{0,127}$`
- `source: "file"` id: absolute JSON pointer (for example `"/providers/openai/apiKey"`)
- `source: "exec"` id pattern: `^[A-Za-z0-9][A-Za-z0-9._:/-]{0,255}$`
- `source: "exec"` ids must not contain `.` or `..` slash-delimited path segments (for example `a/../b` is rejected)

### Supported credential surface

- Canonical matrix: [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface)
- `secrets apply` targets supported `openclaw.json` credential paths.
- `auth-profiles.json` refs are included in runtime resolution and audit coverage.

### Secret providers config

```
{
  secrets: {
    providers: {
      default: { source: "env" }, // optional explicit env provider
      filemain: {
        source: "file",
        path: "~/.openclaw/secrets.json",
        mode: "json",
        timeoutMs: 5000,
      },
      vault: {
        source: "exec",
        command: "/usr/local/bin/openclaw-vault-resolver",
        passEnv: ["PATH", "VAULT_ADDR"],
      },
    },
    defaults: {
      env: "default",
      file: "filemain",
      exec: "vault",
    },
  },
}
```

Notes:

- `file` provider supports `mode: "json"` and `mode: "singleValue"` (`id` must be `"value"` in singleValue mode).
- File and exec provider paths fail closed when Windows ACL verification is unavailable. Set `allowInsecurePath: true` only for trusted paths that cannot be verified.
- `exec` provider requires an absolute `command` path and uses protocol payloads on stdin/stdout.
- By default, symlink command paths are rejected. Set `allowSymlinkCommand: true` to allow symlink paths while validating the resolved target path.
- If `trustedDirs` is configured, the trusted-dir check applies to the resolved target path.
- `exec` child environment is minimal by default; pass required variables explicitly with `passEnv`.
- Secret refs are resolved at activation time into an in-memory snapshot, then request paths read the snapshot only.
- Active-surface filtering applies during activation: unresolved refs on enabled surfaces fail startup/reload, while inactive surfaces are skipped with diagnostics.

* * *

## Auth storage

```
{
  auth: {
    profiles: {
      "anthropic:default": { provider: "anthropic", mode: "api_key" },
      "anthropic:work": { provider: "anthropic", mode: "api_key" },
      "openai-codex:personal": { provider: "openai-codex", mode: "oauth" },
    },
    order: {
      anthropic: ["anthropic:default", "anthropic:work"],
      "openai-codex": ["openai-codex:personal"],
    },
  },
}
```

- Per-agent profiles are stored at `<agentDir>/auth-profiles.json`.
- `auth-profiles.json` supports value-level refs (`keyRef` for `api_key`, `tokenRef` for `token`) for static credential modes.
- Legacy flat `auth-profiles.json` maps such as `{ "provider": { "apiKey": "..." } }` are not a runtime format; `openclaw doctor --fix` rewrites them to canonical `provider:default` API-key profiles with a `.legacy-flat.*.bak` backup.
- OAuth-mode profiles (`auth.profiles.<id>.mode = "oauth"`) do not support SecretRef-backed auth-profile credentials.
- Static runtime credentials come from in-memory resolved snapshots; legacy static `auth.json` entries are scrubbed when discovered.
- Legacy OAuth imports from `~/.openclaw/credentials/oauth.json`.
- See [OAuth](https://docs.openclaw.ai/concepts/oauth).
- Secrets runtime behavior and `audit/configure/apply` tooling: [Secrets Management](https://docs.openclaw.ai/gateway/secrets).

### `auth.cooldowns`

```
{
  auth: {
    cooldowns: {
      billingBackoffHours: 5,
      billingBackoffHoursByProvider: { anthropic: 3, openai: 8 },
      billingMaxHours: 24,
      authPermanentBackoffMinutes: 10,
      authPermanentMaxMinutes: 60,
      failureWindowHours: 24,
      overloadedProfileRotations: 1,
      overloadedBackoffMs: 0,
      rateLimitedProfileRotations: 1,
    },
  },
}
```

- `billingBackoffHours`: base backoff in hours when a profile fails due to true
billing/insufficient-credit errors (default: `5`). Explicit billing text can
still land here even on `401`/`403` responses, but provider-specific text
matchers stay scoped to the provider that owns them (for example OpenRouter
`Key limit exceeded`). Retryable HTTP `402` usage-window or
organization/workspace spend-limit messages stay in the `rate_limit` path
instead.
- `billingBackoffHoursByProvider`: optional per-provider overrides for billing backoff hours.
- `billingMaxHours`: cap in hours for billing backoff exponential growth (default: `24`).
- `authPermanentBackoffMinutes`: base backoff in minutes for high-confidence `auth_permanent` failures (default: `10`).
- `authPermanentMaxMinutes`: cap in minutes for `auth_permanent` backoff growth (default: `60`).
- `failureWindowHours`: rolling window in hours used for backoff counters (default: `24`).
- `overloadedProfileRotations`: maximum same-provider auth-profile rotations for overloaded errors before switching to model fallback (default: `1`). Provider-busy shapes such as `ModelNotReadyException` land here.
- `overloadedBackoffMs`: fixed delay before retrying an overloaded provider/profile rotation (default: `0`).
- `rateLimitedProfileRotations`: maximum same-provider auth-profile rotations for rate-limit errors before switching to model fallback (default: `1`). That rate-limit bucket includes provider-shaped text such as `Too many concurrent requests`, `ThrottlingException`, `concurrency limit reached`, `workers_ai ... quota limit exceeded`, and `resource exhausted`.

* * *

## Logging

```
{
  logging: {
    level: "info",
    file: "/tmp/openclaw/openclaw.log",
    consoleLevel: "info",
    consoleStyle: "pretty", // pretty | compact | json
    redactSensitive: "tools", // off | tools
    redactPatterns: ["\\bTOKEN\\b\\s*[=:]\\s*([\"']?)([^\\s\"']+)\\1"],
  },
}
```

- Default log file: `/tmp/openclaw/openclaw-YYYY-MM-DD.log`.
- Set `logging.file` for a stable path.
- `consoleLevel` bumps to `debug` when `--verbose`.
- `maxFileBytes`: maximum active log file size in bytes before rotation (positive integer; default: `104857600` = 100 MB). OpenClaw keeps up to five numbered archives beside the active file.
- `redactSensitive` / `redactPatterns`: best-effort masking for console output, file logs, OTLP log records, and persisted session transcript text. `redactSensitive: "off"` only disables this general log/transcript policy; UI/tool/diagnostic safety surfaces still redact secrets before emission.

* * *

## Diagnostics

```
{
  diagnostics: {
    enabled: true,
    flags: ["telegram.*"],
    stuckSessionWarnMs: 30000,

    otel: {
      enabled: false,
      endpoint: "https://otel-collector.example.com:4318",
      tracesEndpoint: "https://traces.example.com/v1/traces",
      metricsEndpoint: "https://metrics.example.com/v1/metrics",
      logsEndpoint: "https://logs.example.com/v1/logs",
      protocol: "http/protobuf", // http/protobuf | grpc
      headers: { "x-tenant-id": "my-org" },
      serviceName: "openclaw-gateway",
      traces: true,
      metrics: true,
      logs: false,
      sampleRate: 1.0,
      flushIntervalMs: 5000,
      captureContent: {
        enabled: false,
        inputMessages: false,
        outputMessages: false,
        toolInputs: false,
        toolOutputs: false,
        systemPrompt: false,
      },
    },

    cacheTrace: {
      enabled: false,
      filePath: "~/.openclaw/logs/cache-trace.jsonl",
      includeMessages: true,
      includePrompt: true,
      includeSystem: true,
    },
  },
}
```

- `enabled`: master toggle for instrumentation output (default: `true`).
- `flags`: array of flag strings enabling targeted log output (supports wildcards like `"telegram.*"` or `"*"`).
- `stuckSessionWarnMs`: no-progress age threshold in ms for classifying long-running processing sessions as `session.long_running`, `session.stalled`, or `session.stuck`. Reply, tool, status, block, and ACP progress reset the timer; repeated `session.stuck` diagnostics back off while unchanged.
- `otel.enabled`: enables the OpenTelemetry export pipeline (default: `false`). For the full configuration, signal catalog, and privacy model, see [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry).
- `otel.endpoint`: collector URL for OTel export.
- `otel.tracesEndpoint` / `otel.metricsEndpoint` / `otel.logsEndpoint`: optional signal-specific OTLP endpoints. When set, they override `otel.endpoint` for that signal only.
- `otel.protocol`: `"http/protobuf"` (default) or `"grpc"`.
- `otel.headers`: extra HTTP/gRPC metadata headers sent with OTel export requests.
- `otel.serviceName`: service name for resource attributes.
- `otel.traces` / `otel.metrics` / `otel.logs`: enable trace, metrics, or log export.
- `otel.sampleRate`: trace sampling rate `0`–`1`.
- `otel.flushIntervalMs`: periodic telemetry flush interval in ms.
- `otel.captureContent`: opt-in raw content capture for OTEL span attributes. Defaults to off. Boolean `true` captures non-system message/tool content; the object form lets you enable `inputMessages`, `outputMessages`, `toolInputs`, `toolOutputs`, and `systemPrompt` explicitly.
- `OTEL_SEMCONV_STABILITY_OPT_IN=gen_ai_latest_experimental`: environment toggle for latest experimental GenAI span provider attributes. By default spans keep the legacy `gen_ai.system` attribute for compatibility; GenAI metrics use bounded semantic attributes.
- `OPENCLAW_OTEL_PRELOADED=1`: environment toggle for hosts that already registered a global OpenTelemetry SDK. OpenClaw then skips plugin-owned SDK startup/shutdown while keeping diagnostic listeners active.
- `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT`, `OTEL_EXPORTER_OTLP_METRICS_ENDPOINT`, and `OTEL_EXPORTER_OTLP_LOGS_ENDPOINT`: signal-specific endpoint env vars used when the matching config key is unset.
- `cacheTrace.enabled`: log cache trace snapshots for embedded runs (default: `false`).
- `cacheTrace.filePath`: output path for cache trace JSONL (default: `$OPENCLAW_STATE_DIR/logs/cache-trace.jsonl`).
- `cacheTrace.includeMessages` / `includePrompt` / `includeSystem`: control what is included in cache trace output (all default: `true`).

* * *

## Update

```
{
  update: {
    channel: "stable", // stable | beta | dev
    checkOnStart: true,

    auto: {
      enabled: false,
      stableDelayHours: 6,
      stableJitterHours: 12,
      betaCheckIntervalHours: 1,
    },
  },
}
```

- `channel`: release channel for npm/git installs — `"stable"`, `"beta"`, or `"dev"`.
- `checkOnStart`: check for npm updates when the gateway starts (default: `true`).
- `auto.enabled`: enable background auto-update for package installs (default: `false`).
- `auto.stableDelayHours`: minimum delay in hours before stable-channel auto-apply (default: `6`; max: `168`).
- `auto.stableJitterHours`: extra stable-channel rollout spread window in hours (default: `12`; max: `168`).
- `auto.betaCheckIntervalHours`: how often beta-channel checks run in hours (default: `1`; max: `24`).

* * *

## ACP

```
{
  acp: {
    enabled: true,
    dispatch: { enabled: true },
    backend: "acpx",
    defaultAgent: "main",
    allowedAgents: ["main", "ops"],
    maxConcurrentSessions: 10,

    stream: {
      coalesceIdleMs: 50,
      maxChunkChars: 1000,
      repeatSuppression: true,
      deliveryMode: "live", // live | final_only
      hiddenBoundarySeparator: "paragraph", // none | space | newline | paragraph
      maxOutputChars: 50000,
      maxSessionUpdateChars: 500,
    },

    runtime: {
      ttlMinutes: 30,
    },
  },
}
```

- `enabled`: global ACP feature gate (default: `true`; set `false` to hide ACP dispatch and spawn affordances).
- `dispatch.enabled`: independent gate for ACP session turn dispatch (default: `true`). Set `false` to keep ACP commands available while blocking execution.
- `backend`: default ACP runtime backend id (must match a registered ACP runtime plugin).
Install the backend plugin first, and if `plugins.allow` is set, include the backend plugin id (for example `acpx`) or the ACP backend will not load.
- `defaultAgent`: fallback ACP target agent id when spawns do not specify an explicit target.
- `allowedAgents`: allowlist of agent ids permitted for ACP runtime sessions; empty means no additional restriction.
- `maxConcurrentSessions`: maximum concurrently active ACP sessions.
- `stream.coalesceIdleMs`: idle flush window in ms for streamed text.
- `stream.maxChunkChars`: maximum chunk size before splitting streamed block projection.
- `stream.repeatSuppression`: suppress repeated status/tool lines per turn (default: `true`).
- `stream.deliveryMode`: `"live"` streams incrementally; `"final_only"` buffers until turn terminal events.
- `stream.hiddenBoundarySeparator`: separator before visible text after hidden tool events (default: `"paragraph"`).
- `stream.maxOutputChars`: maximum assistant output characters projected per ACP turn.
- `stream.maxSessionUpdateChars`: maximum characters for projected ACP status/update lines.
- `stream.tagVisibility`: record of tag names to boolean visibility overrides for streamed events.
- `runtime.ttlMinutes`: idle TTL in minutes for ACP session workers before eligible cleanup.
- `runtime.installCommand`: optional install command to run when bootstrapping an ACP runtime environment.

* * *

## CLI

```
{
  cli: {
    banner: {
      taglineMode: "off", // random | default | off
    },
  },
}
```

- `cli.banner.taglineMode`controls banner tagline style:

  - `"random"` (default): rotating funny/seasonal taglines.
  - `"default"`: fixed neutral tagline (`All your chats, one OpenClaw.`).
  - `"off"`: no tagline text (banner title/version still shown).
- To hide the entire banner (not just taglines), set env `OPENCLAW_HIDE_BANNER=1`.

* * *

## Wizard

Metadata written by CLI guided setup flows (`onboard`, `configure`, `doctor`):

```
{
  wizard: {
    lastRunAt: "2026-01-01T00:00:00.000Z",
    lastRunVersion: "2026.1.4",
    lastRunCommit: "abc1234",
    lastRunCommand: "configure",
    lastRunMode: "local",
  },
}
```

* * *

## Identity

See `agents.list` identity fields under [Agent defaults](https://docs.openclaw.ai/gateway/config-agents#agent-defaults).

* * *

## Bridge (legacy, removed)

Current builds no longer include the TCP bridge. Nodes connect over the Gateway WebSocket. `bridge.*` keys are no longer part of the config schema (validation fails until removed; `openclaw doctor --fix` can strip unknown keys).

Legacy bridge config (historical reference)

```
{
  "bridge": {
    "enabled": true,
    "port": 18790,
    "bind": "tailnet",
    "tls": {
      "enabled": true,
      "autoGenerate": true
    }
  }
}
```

* * *

## Cron

```
{
  cron: {
    enabled: true,
    maxConcurrentRuns: 2, // cron dispatch + isolated cron agent-turn execution
    webhook: "https://example.invalid/legacy", // deprecated fallback for stored notify:true jobs
    webhookToken: "replace-with-dedicated-token", // optional bearer token for outbound webhook auth
    sessionRetention: "24h", // duration string or false
    runLog: {
      maxBytes: "2mb", // default 2_000_000 bytes
      keepLines: 2000, // default 2000
    },
  },
}
```

- `sessionRetention`: how long to keep completed isolated cron run sessions before pruning from `sessions.json`. Also controls cleanup of archived deleted cron transcripts. Default: `24h`; set `false` to disable.
- `runLog.maxBytes`: max size per run log file (`cron/runs/<jobId>.jsonl`) before pruning. Default: `2_000_000` bytes.
- `runLog.keepLines`: newest lines retained when run-log pruning is triggered. Default: `2000`.
- `webhookToken`: bearer token used for cron webhook POST delivery (`delivery.mode = "webhook"`), if omitted no auth header is sent.
- `webhook`: deprecated legacy fallback webhook URL (http/https) used only for stored jobs that still have `notify: true`.

### `cron.retry`

```
{
  cron: {
    retry: {
      maxAttempts: 3,
      backoffMs: [30000, 60000, 300000],
      retryOn: ["rate_limit", "overloaded", "network", "timeout", "server_error"],
    },
  },
}
```

- `maxAttempts`: maximum retries for one-shot jobs on transient errors (default: `3`; range: `0`–`10`).
- `backoffMs`: array of backoff delays in ms for each retry attempt (default: `[30000, 60000, 300000]`; 1–10 entries).
- `retryOn`: error types that trigger retries — `"rate_limit"`, `"overloaded"`, `"network"`, `"timeout"`, `"server_error"`. Omit to retry all transient types.

Applies only to one-shot cron jobs. Recurring jobs use separate failure handling.

### `cron.failureAlert`

```
{
  cron: {
    failureAlert: {
      enabled: false,
      after: 3,
      cooldownMs: 3600000,
      includeSkipped: false,
      mode: "announce",
      accountId: "main",
    },
  },
}
```

- `enabled`: enable failure alerts for cron jobs (default: `false`).
- `after`: consecutive failures before an alert fires (positive integer, min: `1`).
- `cooldownMs`: minimum milliseconds between repeated alerts for the same job (non-negative integer).
- `includeSkipped`: count consecutive skipped runs toward the alert threshold (default: `false`). Skipped runs are tracked separately and do not affect execution-error backoff.
- `mode`: delivery mode — `"announce"` sends via a channel message; `"webhook"` posts to the configured webhook.
- `accountId`: optional account or channel id to scope alert delivery.

### `cron.failureDestination`

```
{
  cron: {
    failureDestination: {
      mode: "announce",
      channel: "last",
      to: "channel:C1234567890",
      accountId: "main",
    },
  },
}
```

- Default destination for cron failure notifications across all jobs.
- `mode`: `"announce"` or `"webhook"`; defaults to `"announce"` when enough target data exists.
- `channel`: channel override for announce delivery. `"last"` reuses the last known delivery channel.
- `to`: explicit announce target or webhook URL. Required for webhook mode.
- `accountId`: optional account override for delivery.
- Per-job `delivery.failureDestination` overrides this global default.
- When neither global nor per-job failure destination is set, jobs that already deliver via `announce` fall back to that primary announce target on failure.
- `delivery.failureDestination` is only supported for `sessionTarget="isolated"` jobs unless the job’s primary `delivery.mode` is `"webhook"`.

See [Cron Jobs](https://docs.openclaw.ai/automation/cron-jobs). Isolated cron executions are tracked as [background tasks](https://docs.openclaw.ai/automation/tasks).

* * *

## Media model template variables

Template placeholders expanded in `tools.media.models[].args`:

| Variable | Description |
| --- | --- |
| `{{Body}}` | Full inbound message body |
| `{{RawBody}}` | Raw body (no history/sender wrappers) |
| `{{BodyStripped}}` | Body with group mentions stripped |
| `{{From}}` | Sender identifier |
| `{{To}}` | Destination identifier |
| `{{MessageSid}}` | Channel message id |
| `{{SessionId}}` | Current session UUID |
| `{{IsNewSession}}` | `"true"` when new session created |
| `{{MediaUrl}}` | Inbound media pseudo-URL |
| `{{MediaPath}}` | Local media path |
| `{{MediaType}}` | Media type (image/audio/document/…) |
| `{{Transcript}}` | Audio transcript |
| `{{Prompt}}` | Resolved media prompt for CLI entries |
| `{{MaxChars}}` | Resolved max output chars for CLI entries |
| `{{ChatType}}` | `"direct"` or `"group"` |
| `{{GroupSubject}}` | Group subject (best effort) |
| `{{GroupMembers}}` | Group members preview (best effort) |
| `{{SenderName}}` | Sender display name (best effort) |
| `{{SenderE164}}` | Sender phone number (best effort) |
| `{{Provider}}` | Provider hint (whatsapp, telegram, discord, etc.) |

* * *

## Config includes (`$include`)

Split config into multiple files:

```
// ~/.openclaw/openclaw.json
{
  gateway: { port: 18789 },
  agents: { $include: "./agents.json5" },
  broadcast: {
    $include: ["./clients/mueller.json5", "./clients/schmidt.json5"],
  },
}
```

**Merge behavior:**

- Single file: replaces the containing object.
- Array of files: deep-merged in order (later overrides earlier).
- Sibling keys: merged after includes (override included values).
- Nested includes: up to 10 levels deep.
- Paths: resolved relative to the including file, but must stay inside the top-level config directory (`dirname` of `openclaw.json`). Absolute/`../` forms are allowed only when they still resolve inside that boundary.
- OpenClaw-owned writes that change only one top-level section backed by a single-file include write through to that included file. For example, `plugins install` updates `plugins: { $include: "./plugins.json5" }` in `plugins.json5` and leaves `openclaw.json` intact.
- Root includes, include arrays, and includes with sibling overrides are read-only for OpenClaw-owned writes; those writes fail closed instead of flattening the config.
- Errors: clear messages for missing files, parse errors, and circular includes.

* * *

_Related: [Configuration](https://docs.openclaw.ai/gateway/configuration) · [Configuration Examples](https://docs.openclaw.ai/gateway/configuration-examples) · [Doctor](https://docs.openclaw.ai/gateway/doctor)_

## Related

- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Configuration examples](https://docs.openclaw.ai/gateway/configuration-examples)

[Configuration](https://docs.openclaw.ai/gateway/configuration) [Configuration — agents](https://docs.openclaw.ai/gateway/config-agents)

Ctrl+I

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

OpenClaw-managed MCP server definitions live under \`mcp.servers\` and are
consumed by embedded Pi and other runtime adapters. The \`openclaw mcp list\`,
\`show\`, \`set\`, and \`unset\` commands manage this block without connecting to the
target server during config edits.

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 mcp: {
 // Optional. Default: 600000 ms (10 minutes). Set 0 to disable idle eviction.
 sessionIdleTtlMs: 600000,
 servers: {
 docs: {
 command: "npx",
 args: \["-y", "@modelcontextprotocol/server-fetch"\],
 },
 remote: {
 url: "https://example.com/mcp",
 transport: "streamable-http", // streamable-http \| sse
 headers: {
 Authorization: "Bearer ${MCP\_REMOTE\_TOKEN}",
 },
 },
 },
 },
}
\`\`\`

\\* \`mcp.servers\`: named stdio or remote MCP server definitions for runtimes that
 expose configured MCP tools.
 Remote entries use \`transport: "streamable-http"\` or \`transport: "sse"\`;
 \`type: "http"\` is a CLI-native alias that \`openclaw mcp set\` and
 \`openclaw doctor --fix\` normalize into the canonical \`transport\` field.
\\* \`mcp.sessionIdleTtlMs\`: idle TTL for session-scoped bundled MCP runtimes.
 One-shot embedded runs request run-end cleanup; this TTL is the backstop for
 long-lived sessions and future callers.
\\* Changes under \`mcp.\*\` hot-apply by disposing cached session MCP runtimes.
 The next tool discovery/use recreates them from the new config, so removed
 \`mcp.servers\` entries are reaped immediately instead of waiting for idle TTL.

See \[MCP\](/cli/mcp#openclaw-as-an-mcp-client-registry) and
\[CLI backends\](/gateway/cli-backends#bundle-mcp-overlays) for runtime behavior.

\## Skills

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 skills: {
 allowBundled: \["gemini", "peekaboo"\],
 load: {
 extraDirs: \["~/Projects/agent-scripts/skills"\],
 },
 install: {
 preferBrew: true,
 nodeManager: "npm", // npm \| pnpm \| yarn \| bun
 },
 entries: {
 "image-lab": {
 apiKey: { source: "env", provider: "default", id: "GEMINI\_API\_KEY" }, // or plaintext string
 env: { GEMINI\_API\_KEY: "GEMINI\_KEY\_HERE" },
 },
 peekaboo: { enabled: true },
 sag: { enabled: false },
 },
 },
}
\`\`\`

\\* \`allowBundled\`: optional allowlist for bundled skills only (managed/workspace skills unaffected).
\\* \`load.extraDirs\`: extra shared skill roots (lowest precedence).
\\* \`install.preferBrew\`: when true, prefer Homebrew installers when \`brew\` is
 available before falling back to other installer kinds.
\\* \`install.nodeManager\`: node installer preference for \`metadata.openclaw.install\`
 specs (\`npm\` \| \`pnpm\` \| \`yarn\` \| \`bun\`).
\\* \`entries..enabled: false\` disables a skill even if bundled/installed.
\\* \`entries..apiKey\`: convenience for skills declaring a primary env var (plaintext string or SecretRef object).

\\*\\*\\*

\## Plugins

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 plugins: {
 enabled: true,
 allow: \["voice-call"\],
 deny: \[\],
 load: {
 paths: \["~/Projects/oss/voice-call-plugin"\],
 },
 entries: {
 "voice-call": {
 enabled: true,
 hooks: {
 allowPromptInjection: false,
 },
 config: { provider: "twilio" },
 },
 },
 },
}
\`\`\`

\\* Loaded from \`~/.openclaw/extensions\`, \`/.openclaw/extensions\`, plus \`plugins.load.paths\`.
\\* Discovery accepts native OpenClaw plugins plus compatible Codex bundles and Claude bundles, including manifestless Claude default-layout bundles.
\\* \*\*Config changes require a gateway restart.\*\*
\\* \`allow\`: optional allowlist (only listed plugins load). \`deny\` wins.
\\* \`plugins.entries..apiKey\`: plugin-level API key convenience field (when supported by the plugin).
\\* \`plugins.entries..env\`: plugin-scoped env var map.
\\* \`plugins.entries..hooks.allowPromptInjection\`: when \`false\`, core blocks \`before\_prompt\_build\` and ignores prompt-mutating fields from legacy \`before\_agent\_start\`, while preserving legacy \`modelOverride\` and \`providerOverride\`. Applies to native plugin hooks and supported bundle-provided hook directories.
\\* \`plugins.entries..hooks.allowConversationAccess\`: when \`true\`, trusted non-bundled plugins may read raw conversation content from typed hooks such as \`llm\_input\`, \`llm\_output\`, \`before\_agent\_finalize\`, and \`agent\_end\`.
\\* \`plugins.entries..subagent.allowModelOverride\`: explicitly trust this plugin to request per-run \`provider\` and \`model\` overrides for background subagent runs.
\\* \`plugins.entries..subagent.allowedModels\`: optional allowlist of canonical \`provider/model\` targets for trusted subagent overrides. Use \`"\*"\` only when you intentionally want to allow any model.
\\* \`plugins.entries..config\`: plugin-defined config object (validated by native OpenClaw plugin schema when available).
\\* Channel plugin account/runtime settings live under \`channels.\` and should be described by the owning plugin's manifest \`channelConfigs\` metadata, not by a central OpenClaw option registry.
\\* \`plugins.entries.firecrawl.config.webFetch\`: Firecrawl web-fetch provider settings.
 \\* \`apiKey\`: Firecrawl API key (accepts SecretRef). Falls back to \`plugins.entries.firecrawl.config.webSearch.apiKey\`, legacy \`tools.web.fetch.firecrawl.apiKey\`, or \`FIRECRAWL\_API\_KEY\` env var.
 \\* \`baseUrl\`: Firecrawl API base URL (default: \`https://api.firecrawl.dev\`; self-hosted overrides must target private/internal endpoints).
 \\* \`onlyMainContent\`: extract only the main content from pages (default: \`true\`).
 \\* \`maxAgeMs\`: maximum cache age in milliseconds (default: \`172800000\` / 2 days).
 \\* \`timeoutSeconds\`: scrape request timeout in seconds (default: \`60\`).
\\* \`plugins.entries.xai.config.xSearch\`: xAI X Search (Grok web search) settings.
 \\* \`enabled\`: enable the X Search provider.
 \\* \`model\`: Grok model to use for search (e.g. \`"grok-4-1-fast"\`).
\\* \`plugins.entries.memory-core.config.dreaming\`: memory dreaming settings. See \[Dreaming\](/concepts/dreaming) for phases and thresholds.
 \\* \`enabled\`: master dreaming switch (default \`false\`).
 \\* \`frequency\`: cron cadence for each full dreaming sweep (\`"0 3 \* \* \*"\` by default).
 \\* \`model\`: optional Dream Diary subagent model override. Requires \`plugins.entries.memory-core.subagent.allowModelOverride: true\`; pair with \`allowedModels\` to restrict targets. Model-unavailable errors retry once with the session default model; trust or allowlist failures do not fall back silently.
 \\* phase policy and thresholds are implementation details (not user-facing config keys).
\\* Full memory config lives in \[Memory configuration reference\](/reference/memory-config):
 \\* \`agents.defaults.memorySearch.\*\`
 \\* \`memory.backend\`
 \\* \`memory.citations\`
 \\* \`memory.qmd.\*\`
 \\* \`plugins.entries.memory-core.config.dreaming\`
\\* Enabled Claude bundle plugins can also contribute embedded Pi defaults from \`settings.json\`; OpenClaw applies those as sanitized agent settings, not as raw OpenClaw config patches.
\\* \`plugins.slots.memory\`: pick the active memory plugin id, or \`"none"\` to disable memory plugins.
\\* \`plugins.slots.contextEngine\`: pick the active context engine plugin id; defaults to \`"legacy"\` unless you install and select another engine.

See \[Plugins\](/tools/plugin).

\\*\\*\\*

\## Browser

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 browser: {
 enabled: true,
 evaluateEnabled: true,
 defaultProfile: "user",
 ssrfPolicy: {
 // dangerouslyAllowPrivateNetwork: true, // opt in only for trusted private-network access
 // allowPrivateNetwork: true, // legacy alias
 // hostnameAllowlist: \["\*.example.com", "example.com"\],
 // allowedHostnames: \["localhost"\],
 },
 tabCleanup: {
 enabled: true,
 idleMinutes: 120,
 maxTabsPerSession: 8,
 sweepMinutes: 5,
 },
 profiles: {
 openclaw: { cdpPort: 18800, color: "#FF4500" },
 work: {
 cdpPort: 18801,
 color: "#0066CC",
 executablePath: "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",
 },
 user: { driver: "existing-session", attachOnly: true, color: "#00AA00" },
 brave: {
 driver: "existing-session",
 attachOnly: true,
 userDataDir: "~/Library/Application Support/BraveSoftware/Brave-Browser",
 color: "#FB542B",
 },
 remote: { cdpUrl: "http://10.0.0.42:9222", color: "#00AA00" },
 },
 color: "#FF4500",
 // headless: false,
 // noSandbox: false,
 // extraArgs: \[\],
 // executablePath: "/Applications/Brave Browser.app/Contents/MacOS/Brave Browser",
 // attachOnly: false,
 },
}
\`\`\`

\\* \`evaluateEnabled: false\` disables \`act:evaluate\` and \`wait --fn\`.
\\* \`tabCleanup\` reclaims tracked primary-agent tabs after idle time or when a
 session exceeds its cap. Set \`idleMinutes: 0\` or \`maxTabsPerSession: 0\` to
 disable those individual cleanup modes.
\\* \`ssrfPolicy.dangerouslyAllowPrivateNetwork\` is disabled when unset, so browser navigation stays strict by default.
\\* Set \`ssrfPolicy.dangerouslyAllowPrivateNetwork: true\` only when you intentionally trust private-network browser navigation.
\\* In strict mode, remote CDP profile endpoints (\`profiles.\*.cdpUrl\`) are subject to the same private-network blocking during reachability/discovery checks.
\\* \`ssrfPolicy.allowPrivateNetwork\` remains supported as a legacy alias.
\\* In strict mode, use \`ssrfPolicy.hostnameAllowlist\` and \`ssrfPolicy.allowedHostnames\` for explicit exceptions.
\\* Remote profiles are attach-only (start/stop/reset disabled).
\\* \`profiles.\*.cdpUrl\` accepts \`http://\`, \`https://\`, \`ws://\`, and \`wss://\`.
 Use HTTP(S) when you want OpenClaw to discover \`/json/version\`; use WS(S)
 when your provider gives you a direct DevTools WebSocket URL.
\\* \`remoteCdpTimeoutMs\` and \`remoteCdpHandshakeTimeoutMs\` apply to remote and
 \`attachOnly\` CDP reachability plus tab-opening requests. Managed loopback
 profiles keep local CDP defaults.
\\* If an externally managed CDP service is reachable through loopback, set that
 profile's \`attachOnly: true\`; otherwise OpenClaw treats the loopback port as a
 local managed browser profile and may report local port ownership errors.
\\* \`existing-session\` profiles use Chrome MCP instead of CDP and can attach on
 the selected host or through a connected browser node.
\\* \`existing-session\` profiles can set \`userDataDir\` to target a specific
 Chromium-based browser profile such as Brave or Edge.
\\* \`existing-session\` profiles keep the current Chrome MCP route limits:
 snapshot/ref-driven actions instead of CSS-selector targeting, one-file upload
 hooks, no dialog timeout overrides, no \`wait --load networkidle\`, and no
 \`responsebody\`, PDF export, download interception, or batch actions.
\\* Local managed \`openclaw\` profiles auto-assign \`cdpPort\` and \`cdpUrl\`; only
 set \`cdpUrl\` explicitly for remote CDP.
\\* Local managed profiles can set \`executablePath\` to override the global
 \`browser.executablePath\` for that profile. Use this to run one profile in
 Chrome and another in Brave.
\\* Local managed profiles use \`browser.localLaunchTimeoutMs\` for Chrome CDP HTTP
 discovery after process start and \`browser.localCdpReadyTimeoutMs\` for
 post-launch CDP websocket readiness. Raise them on slower hosts where Chrome
 starts successfully but readiness checks race startup. Both values must be
 positive integers up to \`120000\` ms; invalid config values are rejected.
\\* Auto-detect order: default browser if Chromium-based → Chrome → Brave → Edge → Chromium → Chrome Canary.
\\* \`browser.executablePath\` and \`browser.profiles..executablePath\` both
 accept \`~\` and \`~/...\` for your OS home directory before Chromium launch.
 Per-profile \`userDataDir\` on \`existing-session\` profiles is also tilde-expanded.
\\* Control service: loopback only (port derived from \`gateway.port\`, default \`18791\`).
\\* \`extraArgs\` appends extra launch flags to local Chromium startup (for example
 \`--disable-gpu\`, window sizing, or debug flags).

\\*\\*\\*

\## UI

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 ui: {
 seamColor: "#FF4500",
 assistant: {
 name: "OpenClaw",
 avatar: "CB", // emoji, short text, image URL, or data URI
 },
 },
}
\`\`\`

\\* \`seamColor\`: accent color for native app UI chrome (Talk Mode bubble tint, etc.).
\\* \`assistant\`: Control UI identity override. Falls back to active agent identity.

\\*\\*\\*

\## Gateway

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 gateway: {
 mode: "local", // local \| remote
 port: 18789,
 bind: "loopback",
 auth: {
 mode: "token", // none \| token \| password \| trusted-proxy
 token: "your-token",
 // password: "your-password", // or OPENCLAW\_GATEWAY\_PASSWORD
 // trustedProxy: { userHeader: "x-forwarded-user" }, // for mode=trusted-proxy; see /gateway/trusted-proxy-auth
 allowTailscale: true,
 rateLimit: {
 maxAttempts: 10,
 windowMs: 60000,
 lockoutMs: 300000,
 exemptLoopback: true,
 },
 },
 tailscale: {
 mode: "off", // off \| serve \| funnel
 resetOnExit: false,
 },
 controlUi: {
 enabled: true,
 basePath: "/openclaw",
 // root: "dist/control-ui",
 // embedSandbox: "scripts", // strict \| scripts \| trusted
 // allowExternalEmbedUrls: false, // dangerous: allow absolute external http(s) embed URLs
 // allowedOrigins: \["https://control.example.com"\], // required for non-loopback Control UI
 // dangerouslyAllowHostHeaderOriginFallback: false, // dangerous Host-header origin fallback mode
 // allowInsecureAuth: false,
 // dangerouslyDisableDeviceAuth: false,
 },
 remote: {
 url: "ws://gateway.tailnet:18789",
 transport: "ssh", // ssh \| direct
 token: "your-token",
 // password: "your-password",
 },
 trustedProxies: \["10.0.0.1"\],
 // Optional. Default false.
 allowRealIpFallback: false,
 nodes: {
 pairing: {
 // Optional. Default unset/disabled.
 autoApproveCidrs: \["192.168.1.0/24", "fd00:1234:5678::/64"\],
 },
 allowCommands: \["canvas.navigate"\],
 denyCommands: \["system.run"\],
 },
 tools: {
 // Additional /tools/invoke HTTP denies
 deny: \["browser"\],
 // Remove tools from the default HTTP deny list
 allow: \["gateway"\],
 },
 push: {
 apns: {
 relay: {
 baseUrl: "https://relay.example.com",
 timeoutMs: 10000,
 },
 },
 },
 },
}
\`\`\`

 \\* \`mode\`: \`local\` (run gateway) or \`remote\` (connect to remote gateway). Gateway refuses to start unless \`local\`.
 \\* \`port\`: single multiplexed port for WS + HTTP. Precedence: \`--port\` > \`OPENCLAW\_GATEWAY\_PORT\` > \`gateway.port\` > \`18789\`.
 \\* \`bind\`: \`auto\`, \`loopback\` (default), \`lan\` (\`0.0.0.0\`), \`tailnet\` (Tailscale IP only), or \`custom\`.
 \\* \*\*Legacy bind aliases\*\*: use bind mode values in \`gateway.bind\` (\`auto\`, \`loopback\`, \`lan\`, \`tailnet\`, \`custom\`), not host aliases (\`0.0.0.0\`, \`127.0.0.1\`, \`localhost\`, \`::\`, \`::1\`).
 \\* \*\*Docker note\*\*: the default \`loopback\` bind listens on \`127.0.0.1\` inside the container. With Docker bridge networking (\`-p 18789:18789\`), traffic arrives on \`eth0\`, so the gateway is unreachable. Use \`--network host\`, or set \`bind: "lan"\` (or \`bind: "custom"\` with \`customBindHost: "0.0.0.0"\`) to listen on all interfaces.
 \\* \*\*Auth\*\*: required by default. Non-loopback binds require gateway auth. In practice that means a shared token/password or an identity-aware reverse proxy with \`gateway.auth.mode: "trusted-proxy"\`. Onboarding wizard generates a token by default.
 \\* If both \`gateway.auth.token\` and \`gateway.auth.password\` are configured (including SecretRefs), set \`gateway.auth.mode\` explicitly to \`token\` or \`password\`. Startup and service install/repair flows fail when both are configured and mode is unset.
 \\* \`gateway.auth.mode: "none"\`: explicit no-auth mode. Use only for trusted local loopback setups; this is intentionally not offered by onboarding prompts.
 \\* \`gateway.auth.mode: "trusted-proxy"\`: delegate browser/user auth to an identity-aware reverse proxy and trust identity headers from \`gateway.trustedProxies\` (see \[Trusted Proxy Auth\](/gateway/trusted-proxy-auth)). This mode expects a \*\*non-loopback\*\* proxy source by default; same-host loopback reverse proxies require explicit \`gateway.auth.trustedProxy.allowLoopback = true\`. Internal same-host callers can use \`gateway.auth.password\` as a local direct fallback; \`gateway.auth.token\` remains mutually exclusive with trusted-proxy mode.
 \\* \`gateway.auth.allowTailscale\`: when \`true\`, Tailscale Serve identity headers can satisfy Control UI/WebSocket auth (verified via \`tailscale whois\`). HTTP API endpoints do \*\*not\*\* use that Tailscale header auth; they follow the gateway's normal HTTP auth mode instead. This tokenless flow assumes the gateway host is trusted. Defaults to \`true\` when \`tailscale.mode = "serve"\`.
 \\* \`gateway.auth.rateLimit\`: optional failed-auth limiter. Applies per client IP and per auth scope (shared-secret and device-token are tracked independently). Blocked attempts return \`429\` + \`Retry-After\`.
 \\* On the async Tailscale Serve Control UI path, failed attempts for the same \`{scope, clientIp}\` are serialized before the failure write. Concurrent bad attempts from the same client can therefore trip the limiter on the second request instead of both racing through as plain mismatches.
 \\* \`gateway.auth.rateLimit.exemptLoopback\` defaults to \`true\`; set \`false\` when you intentionally want localhost traffic rate-limited too (for test setups or strict proxy deployments).
 \\* Browser-origin WS auth attempts are always throttled with loopback exemption disabled (defense-in-depth against browser-based localhost brute force).
 \\* On loopback, those browser-origin lockouts are isolated per normalized \`Origin\`
 value, so repeated failures from one localhost origin do not automatically
 lock out a different origin.
 \\* \`tailscale.mode\`: \`serve\` (tailnet only, loopback bind) or \`funnel\` (public, requires auth).
 \\* \`controlUi.allowedOrigins\`: explicit browser-origin allowlist for Gateway WebSocket connects. Required when browser clients are expected from non-loopback origins.
 \\* \`controlUi.dangerouslyAllowHostHeaderOriginFallback\`: dangerous mode that enables Host-header origin fallback for deployments that intentionally rely on Host-header origin policy.
 \\* \`remote.transport\`: \`ssh\` (default) or \`direct\` (ws/wss). For \`direct\`, \`remote.url\` must be \`ws://\` or \`wss://\`.
 \\* \`OPENCLAW\_ALLOW\_INSECURE\_PRIVATE\_WS=1\`: client-side process-environment
 break-glass override that allows plaintext \`ws://\` to trusted private-network
 IPs; default remains loopback-only for plaintext. There is no \`openclaw.json\`
 equivalent, and browser private-network config such as
 \`browser.ssrfPolicy.dangerouslyAllowPrivateNetwork\` does not affect Gateway
 WebSocket clients.
 \\* \`gateway.remote.token\` / \`.password\` are remote-client credential fields. They do not configure gateway auth by themselves.
 \\* \`gateway.push.apns.relay.baseUrl\`: base HTTPS URL for the external APNs relay used by official/TestFlight iOS builds after they publish relay-backed registrations to the gateway. This URL must match the relay URL compiled into the iOS build.
 \\* \`gateway.push.apns.relay.timeoutMs\`: gateway-to-relay send timeout in milliseconds. Defaults to \`10000\`.
 \\* Relay-backed registrations are delegated to a specific gateway identity. The paired iOS app fetches \`gateway.identity.get\`, includes that identity in the relay registration, and forwards a registration-scoped send grant to the gateway. Another gateway cannot reuse that stored registration.
 \\* \`OPENCLAW\_APNS\_RELAY\_BASE\_URL\` / \`OPENCLAW\_APNS\_RELAY\_TIMEOUT\_MS\`: temporary env overrides for the relay config above.
 \\* \`OPENCLAW\_APNS\_RELAY\_ALLOW\_HTTP=true\`: development-only escape hatch for loopback HTTP relay URLs. Production relay URLs should stay on HTTPS.
 \\* \`gateway.handshakeTimeoutMs\`: pre-auth Gateway WebSocket handshake timeout in milliseconds. Default: \`15000\`. \`OPENCLAW\_HANDSHAKE\_TIMEOUT\_MS\` takes precedence when set. Increase this on loaded or low-powered hosts where local clients can connect while startup warmup is still settling.
 \\* \`gateway.channelHealthCheckMinutes\`: channel health-monitor interval in minutes. Set \`0\` to disable health-monitor restarts globally. Default: \`5\`.
 \\* \`gateway.channelStaleEventThresholdMinutes\`: stale-socket threshold in minutes. Keep this greater than or equal to \`gateway.channelHealthCheckMinutes\`. Default: \`30\`.
 \\* \`gateway.channelMaxRestartsPerHour\`: maximum health-monitor restarts per channel/account in a rolling hour. Default: \`10\`.
 \\* \`channels..healthMonitor.enabled\`: per-channel opt-out for health-monitor restarts while keeping the global monitor enabled.
 \\* \`channels..accounts..healthMonitor.enabled\`: per-account override for multi-account channels. When set, it takes precedence over the channel-level override.
 \\* Local gateway call paths can use \`gateway.remote.\*\` as fallback only when \`gateway.auth.\*\` is unset.
 \\* If \`gateway.auth.token\` / \`gateway.auth.password\` is explicitly configured via SecretRef and unresolved, resolution fails closed (no remote fallback masking).
 \\* \`trustedProxies\`: reverse proxy IPs that terminate TLS or inject forwarded-client headers. Only list proxies you control. Loopback entries are still valid for same-host proxy/local-detection setups (for example Tailscale Serve or a local reverse proxy), but they do \*\*not\*\* make loopback requests eligible for \`gateway.auth.mode: "trusted-proxy"\`.
 \\* \`allowRealIpFallback\`: when \`true\`, the gateway accepts \`X-Real-IP\` if \`X-Forwarded-For\` is missing. Default \`false\` for fail-closed behavior.
 \\* \`gateway.nodes.pairing.autoApproveCidrs\`: optional CIDR/IP allowlist for auto-approving first-time node device pairing with no requested scopes. It is disabled when unset. This does not auto-approve operator/browser/Control UI/WebChat pairing, and it does not auto-approve role, scope, metadata, or public-key upgrades.
 \\* \`gateway.nodes.allowCommands\` / \`gateway.nodes.denyCommands\`: global allow/deny shaping for declared node commands after pairing and platform allowlist evaluation. Use \`allowCommands\` to opt into dangerous node commands such as \`camera.snap\`, \`camera.clip\`, and \`screen.record\`; \`denyCommands\` removes a command even if a platform default or explicit allow would otherwise include it. After a node changes its declared command list, reject and re-approve that device pairing so the gateway stores the updated command snapshot.
 \\* \`gateway.tools.deny\`: extra tool names blocked for HTTP \`POST /tools/invoke\` (extends default deny list).
 \\* \`gateway.tools.allow\`: remove tool names from the default HTTP deny list.

\### OpenAI-compatible endpoints

\\* Chat Completions: disabled by default. Enable with \`gateway.http.endpoints.chatCompletions.enabled: true\`.
\\* Responses API: \`gateway.http.endpoints.responses.enabled\`.
\\* Responses URL-input hardening:
 \\* \`gateway.http.endpoints.responses.maxUrlParts\`
 \\* \`gateway.http.endpoints.responses.files.urlAllowlist\`
 \\* \`gateway.http.endpoints.responses.images.urlAllowlist\`
 Empty allowlists are treated as unset; use \`gateway.http.endpoints.responses.files.allowUrl=false\`
 and/or \`gateway.http.endpoints.responses.images.allowUrl=false\` to disable URL fetching.
\\* Optional response hardening header:
 \\* \`gateway.http.securityHeaders.strictTransportSecurity\` (set only for HTTPS origins you control; see \[Trusted Proxy Auth\](/gateway/trusted-proxy-auth#tls-termination-and-hsts))

\### Multi-instance isolation

Run multiple gateways on one host with unique ports and state dirs:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
OPENCLAW\_CONFIG\_PATH=~/.openclaw/a.json \
OPENCLAW\_STATE\_DIR=~/.openclaw-a \
openclaw gateway --port 19001
\`\`\`

Convenience flags: \`--dev\` (uses \`~/.openclaw-dev\` + port \`19001\`), \`--profile \` (uses \`~/.openclaw-\`).

See \[Multiple Gateways\](/gateway/multiple-gateways).

\### \`gateway.tls\`

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 gateway: {
 tls: {
 enabled: false,
 autoGenerate: false,
 certPath: "/etc/openclaw/tls/server.crt",
 keyPath: "/etc/openclaw/tls/server.key",
 caPath: "/etc/openclaw/tls/ca-bundle.crt",
 },
 },
}
\`\`\`

\\* \`enabled\`: enables TLS termination at the gateway listener (HTTPS/WSS) (default: \`false\`).
\\* \`autoGenerate\`: auto-generates a local self-signed cert/key pair when explicit files are not configured; for local/dev use only.
\\* \`certPath\`: filesystem path to the TLS certificate file.
\\* \`keyPath\`: filesystem path to the TLS private key file; keep permission-restricted.
\\* \`caPath\`: optional CA bundle path for client verification or custom trust chains.

\### \`gateway.reload\`

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 gateway: {
 reload: {
 mode: "hybrid", // off \| restart \| hot \| hybrid
 debounceMs: 500,
 deferralTimeoutMs: 300000,
 },
 },
}
\`\`\`

\\* \`mode\`: controls how config edits are applied at runtime.
 \\* \`"off"\`: ignore live edits; changes require an explicit restart.
 \\* \`"restart"\`: always restart the gateway process on config change.
 \\* \`"hot"\`: apply changes in-process without restarting.
 \\* \`"hybrid"\` (default): try hot reload first; fall back to restart if required.
\\* \`debounceMs\`: debounce window in ms before config changes are applied (non-negative integer).
\\* \`deferralTimeoutMs\`: optional maximum time in ms to wait for in-flight operations before forcing a restart. Omit it to use the default bounded wait (\`300000\`); set \`0\` to wait indefinitely and log periodic still-pending warnings.

\\*\\*\\*

\## Hooks

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 hooks: {
 enabled: true,
 token: "shared-secret",
 path: "/hooks",
 maxBodyBytes: 262144,
 defaultSessionKey: "hook:ingress",
 allowRequestSessionKey: true,
 allowedSessionKeyPrefixes: \["hook:", "hook:gmail:"\],
 allowedAgentIds: \["hooks", "main"\],
 presets: \["gmail"\],
 transformsDir: "~/.openclaw/hooks/transforms",
 mappings: \[\
 {\
 match: { path: "gmail" },\
 action: "agent",\
 agentId: "hooks",\
 wakeMode: "now",\
 name: "Gmail",\
 sessionKey: "hook:gmail:{{messages\[0\].id}}",\
 messageTemplate: "From: {{messages\[0\].from}}\\nSubject: {{messages\[0\].subject}}\\n{{messages\[0\].snippet}}",\
 deliver: true,\
 channel: "last",\
 model: "openai/gpt-5.4-mini",\
 },\
 \],
 },
}
\`\`\`

Auth: \`Authorization: Bearer \` or \`x-openclaw-token: \`.
Query-string hook tokens are rejected.

Validation and safety notes:

\\* \`hooks.enabled=true\` requires a non-empty \`hooks.token\`.
\\* \`hooks.token\` must be \*\*distinct\*\* from \`gateway.auth.token\`; reusing the Gateway token is rejected.
\\* \`hooks.path\` cannot be \`/\`; use a dedicated subpath such as \`/hooks\`.
\\* If \`hooks.allowRequestSessionKey=true\`, constrain \`hooks.allowedSessionKeyPrefixes\` (for example \`\["hook:"\]\`).
\\* If a mapping or preset uses a templated \`sessionKey\`, set \`hooks.allowedSessionKeyPrefixes\` and \`hooks.allowRequestSessionKey=true\`. Static mapping keys do not require that opt-in.

\*\*Endpoints:\*\*

\\* \`POST /hooks/wake\` → \`{ text, mode?: "now"\|"next-heartbeat" }\`
\\* \`POST /hooks/agent\` → \`{ message, name?, agentId?, sessionKey?, wakeMode?, deliver?, channel?, to?, model?, thinking?, timeoutSeconds? }\`
 \\* \`sessionKey\` from request payload is accepted only when \`hooks.allowRequestSessionKey=true\` (default: \`false\`).
\\* \`POST /hooks/\` → resolved via \`hooks.mappings\`
 \\* Template-rendered mapping \`sessionKey\` values are treated as externally supplied and also require \`hooks.allowRequestSessionKey=true\`.

 \- \`match.path\` matches sub-path after \`/hooks\` (e.g. \`/hooks/gmail\` → \`gmail\`).
 \- \`match.source\` matches a payload field for generic paths.
 \- Templates like \`{{messages\[0\].subject}}\` read from the payload.
 \- \`transform\` can point to a JS/TS module returning a hook action.
 \\* \`transform.module\` must be a relative path and stays within \`hooks.transformsDir\` (absolute paths and traversal are rejected).
 \\* Keep \`hooks.transformsDir\` under \`~/.openclaw/hooks/transforms\`; workspace skill directories are rejected. If \`openclaw doctor\` reports this path as invalid, move the transform module into the hooks transforms directory or remove \`hooks.transformsDir\`.
 \- \`agentId\` routes to a specific agent; unknown IDs fall back to default.
 \- \`allowedAgentIds\`: restricts explicit routing (\`\*\` or omitted = allow all, \`\[\]\` = deny all).
 \- \`defaultSessionKey\`: optional fixed session key for hook agent runs without explicit \`sessionKey\`.
 \- \`allowRequestSessionKey\`: allow \`/hooks/agent\` callers and template-driven mapping session keys to set \`sessionKey\` (default: \`false\`).
 \- \`allowedSessionKeyPrefixes\`: optional prefix allowlist for explicit \`sessionKey\` values (request + mapping), e.g. \`\["hook:"\]\`. It becomes required when any mapping or preset uses a templated \`sessionKey\`.
 \- \`deliver: true\` sends final reply to a channel; \`channel\` defaults to \`last\`.
 \- \`model\` overrides LLM for this hook run (must be allowed if model catalog is set).

\### Gmail integration

\\* The built-in Gmail preset uses \`sessionKey: "hook:gmail:{{messages\[0\].id}}"\`.
\\* If you keep that per-message routing, set \`hooks.allowRequestSessionKey: true\` and constrain \`hooks.allowedSessionKeyPrefixes\` to match the Gmail namespace, for example \`\["hook:", "hook:gmail:"\]\`.
\\* If you need \`hooks.allowRequestSessionKey: false\`, override the preset with a static \`sessionKey\` instead of the templated default.

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 hooks: {
 gmail: {
 account: "openclaw@gmail.com",
 topic: "projects//topics/gog-gmail-watch",
 subscription: "gog-gmail-watch-push",
 pushToken: "shared-push-token",
 hookUrl: "http://127.0.0.1:18789/hooks/gmail",
 includeBody: true,
 maxBytes: 20000,
 renewEveryMinutes: 720,
 serve: { bind: "127.0.0.1", port: 8788, path: "/" },
 tailscale: { mode: "funnel", path: "/gmail-pubsub" },
 model: "openrouter/meta-llama/llama-3.3-70b-instruct:free",
 thinking: "off",
 },
 },
}
\`\`\`

\\* Gateway auto-starts \`gog gmail watch serve\` on boot when configured. Set \`OPENCLAW\_SKIP\_GMAIL\_WATCHER=1\` to disable.
\\* Don't run a separate \`gog gmail watch serve\` alongside the Gateway.

\\*\\*\\*

\## Canvas host

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 canvasHost: {
 root: "~/.openclaw/workspace/canvas",
 liveReload: true,
 // enabled: false, // or OPENCLAW\_SKIP\_CANVAS\_HOST=1
 },
}
\`\`\`

\\* Serves agent-editable HTML/CSS/JS and A2UI over HTTP under the Gateway port:
 \\* \`http://:/\_\_openclaw\_\_/canvas/\`
 \\* \`http://:/\_\_openclaw\_\_/a2ui/\`
\\* Local-only: keep \`gateway.bind: "loopback"\` (default).
\\* Non-loopback binds: canvas routes require Gateway auth (token/password/trusted-proxy), same as other Gateway HTTP surfaces.
\\* Node WebViews typically don't send auth headers; after a node is paired and connected, the Gateway advertises node-scoped capability URLs for canvas/A2UI access.
\\* Capability URLs are bound to the active node WS session and expire quickly. IP-based fallback is not used.
\\* Injects live-reload client into served HTML.
\\* Auto-creates starter \`index.html\` when empty.
\\* Also serves A2UI at \`/\_\_openclaw\_\_/a2ui/\`.
\\* Changes require a gateway restart.
\\* Disable live reload for large directories or \`EMFILE\` errors.

\\*\\*\\*

\## Discovery

\### mDNS (Bonjour)

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 discovery: {
 mdns: {
 mode: "minimal", // minimal \| full \| off
 },
 },
}
\`\`\`

\\* \`minimal\` (default): omit \`cliPath\` + \`sshPort\` from TXT records.
\\* \`full\`: include \`cliPath\` + \`sshPort\`.
\\* Hostname defaults to the system hostname when it is a valid DNS label, falling back to \`openclaw\`. Override with \`OPENCLAW\_MDNS\_HOSTNAME\`.

\### Wide-area (DNS-SD)

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 discovery: {
 wideArea: { enabled: true },
 },
}
\`\`\`

Writes a unicast DNS-SD zone under \`~/.openclaw/dns/\`. For cross-network discovery, pair with a DNS server (CoreDNS recommended) + Tailscale split DNS.

Setup: \`openclaw dns setup --apply\`.

\\*\\*\\*

\## Environment

\### \`env\` (inline env vars)

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 env: {
 OPENROUTER\_API\_KEY: "sk-or-...",
 vars: {
 GROQ\_API\_KEY: "gsk-...",
 },
 shellEnv: {
 enabled: true,
 timeoutMs: 15000,
 },
 },
}
\`\`\`

\\* Inline env vars are only applied if the process env is missing the key.
\\* \`.env\` files: CWD \`.env\` + \`~/.openclaw/.env\` (neither overrides existing vars).
\\* \`shellEnv\`: imports missing expected keys from your login shell profile.
\\* See \[Environment\](/help/environment) for full precedence.

\### Env var substitution

Reference env vars in any config string with \`${VAR\_NAME}\`:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 gateway: {
 auth: { token: "${OPENCLAW\_GATEWAY\_TOKEN}" },
 },
}
\`\`\`

\\* Only uppercase names matched: \`\[A-Z\_\]\[A-Z0-9\_\]\*\`.
\\* Missing/empty vars throw an error at config load.
\\* Escape with \`$${VAR}\` for a literal \`${VAR}\`.
\\* Works with \`$include\`.

\\*\\*\\*

\## Secrets

Secret refs are additive: plaintext values still work.

\### \`SecretRef\`

Use one object shape:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ source: "env" \| "file" \| "exec", provider: "default", id: "..." }
\`\`\`

Validation:

\\* \`provider\` pattern: \`^\[a-z\]\[a-z0-9\_-\]{0,63}$\`
\\* \`source: "env"\` id pattern: \`^\[A-Z\]\[A-Z0-9\_\]{0,127}$\`
\\* \`source: "file"\` id: absolute JSON pointer (for example \`"/providers/openai/apiKey"\`)
\\* \`source: "exec"\` id pattern: \`^\[A-Za-z0-9\]\[A-Za-z0-9.\_:/-\]{0,255}$\`
\\* \`source: "exec"\` ids must not contain \`.\` or \`..\` slash-delimited path segments (for example \`a/../b\` is rejected)

\### Supported credential surface

\\* Canonical matrix: \[SecretRef Credential Surface\](/reference/secretref-credential-surface)
\\* \`secrets apply\` targets supported \`openclaw.json\` credential paths.
\\* \`auth-profiles.json\` refs are included in runtime resolution and audit coverage.

\### Secret providers config

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 secrets: {
 providers: {
 default: { source: "env" }, // optional explicit env provider
 filemain: {
 source: "file",
 path: "~/.openclaw/secrets.json",
 mode: "json",
 timeoutMs: 5000,
 },
 vault: {
 source: "exec",
 command: "/usr/local/bin/openclaw-vault-resolver",
 passEnv: \["PATH", "VAULT\_ADDR"\],
 },
 },
 defaults: {
 env: "default",
 file: "filemain",
 exec: "vault",
 },
 },
}
\`\`\`

Notes:

\\* \`file\` provider supports \`mode: "json"\` and \`mode: "singleValue"\` (\`id\` must be \`"value"\` in singleValue mode).
\\* File and exec provider paths fail closed when Windows ACL verification is unavailable. Set \`allowInsecurePath: true\` only for trusted paths that cannot be verified.
\\* \`exec\` provider requires an absolute \`command\` path and uses protocol payloads on stdin/stdout.
\\* By default, symlink command paths are rejected. Set \`allowSymlinkCommand: true\` to allow symlink paths while validating the resolved target path.
\\* If \`trustedDirs\` is configured, the trusted-dir check applies to the resolved target path.
\\* \`exec\` child environment is minimal by default; pass required variables explicitly with \`passEnv\`.
\\* Secret refs are resolved at activation time into an in-memory snapshot, then request paths read the snapshot only.
\\* Active-surface filtering applies during activation: unresolved refs on enabled surfaces fail startup/reload, while inactive surfaces are skipped with diagnostics.

\\*\\*\\*

\## Auth storage

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 auth: {
 profiles: {
 "anthropic:default": { provider: "anthropic", mode: "api\_key" },
 "anthropic:work": { provider: "anthropic", mode: "api\_key" },
 "openai-codex:personal": { provider: "openai-codex", mode: "oauth" },
 },
 order: {
 anthropic: \["anthropic:default", "anthropic:work"\],
 "openai-codex": \["openai-codex:personal"\],
 },
 },
}
\`\`\`

\\* Per-agent profiles are stored at \`/auth-profiles.json\`.
\\* \`auth-profiles.json\` supports value-level refs (\`keyRef\` for \`api\_key\`, \`tokenRef\` for \`token\`) for static credential modes.
\\* Legacy flat \`auth-profiles.json\` maps such as \`{ "provider": { "apiKey": "..." } }\` are not a runtime format; \`openclaw doctor --fix\` rewrites them to canonical \`provider:default\` API-key profiles with a \`.legacy-flat.\*.bak\` backup.
\\* OAuth-mode profiles (\`auth.profiles..mode = "oauth"\`) do not support SecretRef-backed auth-profile credentials.
\\* Static runtime credentials come from in-memory resolved snapshots; legacy static \`auth.json\` entries are scrubbed when discovered.
\\* Legacy OAuth imports from \`~/.openclaw/credentials/oauth.json\`.
\\* See \[OAuth\](/concepts/oauth).
\\* Secrets runtime behavior and \`audit/configure/apply\` tooling: \[Secrets Management\](/gateway/secrets).

\### \`auth.cooldowns\`

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 auth: {
 cooldowns: {
 billingBackoffHours: 5,
 billingBackoffHoursByProvider: { anthropic: 3, openai: 8 },
 billingMaxHours: 24,
 authPermanentBackoffMinutes: 10,
 authPermanentMaxMinutes: 60,
 failureWindowHours: 24,
 overloadedProfileRotations: 1,
 overloadedBackoffMs: 0,
 rateLimitedProfileRotations: 1,
 },
 },
}
\`\`\`

\\* \`billingBackoffHours\`: base backoff in hours when a profile fails due to true
 billing/insufficient-credit errors (default: \`5\`). Explicit billing text can
 still land here even on \`401\`/\`403\` responses, but provider-specific text
 matchers stay scoped to the provider that owns them (for example OpenRouter
 \`Key limit exceeded\`). Retryable HTTP \`402\` usage-window or
 organization/workspace spend-limit messages stay in the \`rate\_limit\` path
 instead.
\\* \`billingBackoffHoursByProvider\`: optional per-provider overrides for billing backoff hours.
\\* \`billingMaxHours\`: cap in hours for billing backoff exponential growth (default: \`24\`).
\\* \`authPermanentBackoffMinutes\`: base backoff in minutes for high-confidence \`auth\_permanent\` failures (default: \`10\`).
\\* \`authPermanentMaxMinutes\`: cap in minutes for \`auth\_permanent\` backoff growth (default: \`60\`).
\\* \`failureWindowHours\`: rolling window in hours used for backoff counters (default: \`24\`).
\\* \`overloadedProfileRotations\`: maximum same-provider auth-profile rotations for overloaded errors before switching to model fallback (default: \`1\`). Provider-busy shapes such as \`ModelNotReadyException\` land here.
\\* \`overloadedBackoffMs\`: fixed delay before retrying an overloaded provider/profile rotation (default: \`0\`).
\\* \`rateLimitedProfileRotations\`: maximum same-provider auth-profile rotations for rate-limit errors before switching to model fallback (default: \`1\`). That rate-limit bucket includes provider-shaped text such as \`Too many concurrent requests\`, \`ThrottlingException\`, \`concurrency limit reached\`, \`workers\_ai ... quota limit exceeded\`, and \`resource exhausted\`.

\\*\\*\\*

\## Logging

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 logging: {
 level: "info",
 file: "/tmp/openclaw/openclaw.log",
 consoleLevel: "info",
 consoleStyle: "pretty", // pretty \| compact \| json
 redactSensitive: "tools", // off \| tools
 redactPatterns: \["\\\bTOKEN\\\b\\\s\*\[=:\]\\\s\*(\[\\"'\]?)(\[^\\\s\\"'\]+)\\\1"\],
 },
}
\`\`\`

\\* Default log file: \`/tmp/openclaw/openclaw-YYYY-MM-DD.log\`.
\\* Set \`logging.file\` for a stable path.
\\* \`consoleLevel\` bumps to \`debug\` when \`--verbose\`.
\\* \`maxFileBytes\`: maximum active log file size in bytes before rotation (positive integer; default: \`104857600\` = 100 MB). OpenClaw keeps up to five numbered archives beside the active file.
\\* \`redactSensitive\` / \`redactPatterns\`: best-effort masking for console output, file logs, OTLP log records, and persisted session transcript text. \`redactSensitive: "off"\` only disables this general log/transcript policy; UI/tool/diagnostic safety surfaces still redact secrets before emission.

\\*\\*\\*

\## Diagnostics

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 diagnostics: {
 enabled: true,
 flags: \["telegram.\*"\],
 stuckSessionWarnMs: 30000,

 otel: {
 enabled: false,
 endpoint: "https://otel-collector.example.com:4318",
 tracesEndpoint: "https://traces.example.com/v1/traces",
 metricsEndpoint: "https://metrics.example.com/v1/metrics",
 logsEndpoint: "https://logs.example.com/v1/logs",
 protocol: "http/protobuf", // http/protobuf \| grpc
 headers: { "x-tenant-id": "my-org" },
 serviceName: "openclaw-gateway",
 traces: true,
 metrics: true,
 logs: false,
 sampleRate: 1.0,
 flushIntervalMs: 5000,
 captureContent: {
 enabled: false,
 inputMessages: false,
 outputMessages: false,
 toolInputs: false,
 toolOutputs: false,
 systemPrompt: false,
 },
 },

 cacheTrace: {
 enabled: false,
 filePath: "~/.openclaw/logs/cache-trace.jsonl",
 includeMessages: true,
 includePrompt: true,
 includeSystem: true,
 },
 },
}
\`\`\`

\\* \`enabled\`: master toggle for instrumentation output (default: \`true\`).
\\* \`flags\`: array of flag strings enabling targeted log output (supports wildcards like \`"telegram.\*"\` or \`"\*"\`).
\\* \`stuckSessionWarnMs\`: no-progress age threshold in ms for classifying long-running processing sessions as \`session.long\_running\`, \`session.stalled\`, or \`session.stuck\`. Reply, tool, status, block, and ACP progress reset the timer; repeated \`session.stuck\` diagnostics back off while unchanged.
\\* \`otel.enabled\`: enables the OpenTelemetry export pipeline (default: \`false\`). For the full configuration, signal catalog, and privacy model, see \[OpenTelemetry export\](/gateway/opentelemetry).
\\* \`otel.endpoint\`: collector URL for OTel export.
\\* \`otel.tracesEndpoint\` / \`otel.metricsEndpoint\` / \`otel.logsEndpoint\`: optional signal-specific OTLP endpoints. When set, they override \`otel.endpoint\` for that signal only.
\\* \`otel.protocol\`: \`"http/protobuf"\` (default) or \`"grpc"\`.
\\* \`otel.headers\`: extra HTTP/gRPC metadata headers sent with OTel export requests.
\\* \`otel.serviceName\`: service name for resource attributes.
\\* \`otel.traces\` / \`otel.metrics\` / \`otel.logs\`: enable trace, metrics, or log export.
\\* \`otel.sampleRate\`: trace sampling rate \`0\`–\`1\`.
\\* \`otel.flushIntervalMs\`: periodic telemetry flush interval in ms.
\\* \`otel.captureContent\`: opt-in raw content capture for OTEL span attributes. Defaults to off. Boolean \`true\` captures non-system message/tool content; the object form lets you enable \`inputMessages\`, \`outputMessages\`, \`toolInputs\`, \`toolOutputs\`, and \`systemPrompt\` explicitly.
\\* \`OTEL\_SEMCONV\_STABILITY\_OPT\_IN=gen\_ai\_latest\_experimental\`: environment toggle for latest experimental GenAI span provider attributes. By default spans keep the legacy \`gen\_ai.system\` attribute for compatibility; GenAI metrics use bounded semantic attributes.
\\* \`OPENCLAW\_OTEL\_PRELOADED=1\`: environment toggle for hosts that already registered a global OpenTelemetry SDK. OpenClaw then skips plugin-owned SDK startup/shutdown while keeping diagnostic listeners active.
\\* \`OTEL\_EXPORTER\_OTLP\_TRACES\_ENDPOINT\`, \`OTEL\_EXPORTER\_OTLP\_METRICS\_ENDPOINT\`, and \`OTEL\_EXPORTER\_OTLP\_LOGS\_ENDPOINT\`: signal-specific endpoint env vars used when the matching config key is unset.
\\* \`cacheTrace.enabled\`: log cache trace snapshots for embedded runs (default: \`false\`).
\\* \`cacheTrace.filePath\`: output path for cache trace JSONL (default: \`$OPENCLAW\_STATE\_DIR/logs/cache-trace.jsonl\`).
\\* \`cacheTrace.includeMessages\` / \`includePrompt\` / \`includeSystem\`: control what is included in cache trace output (all default: \`true\`).

\\*\\*\\*

\## Update

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 update: {
 channel: "stable", // stable \| beta \| dev
 checkOnStart: true,

 auto: {
 enabled: false,
 stableDelayHours: 6,
 stableJitterHours: 12,
 betaCheckIntervalHours: 1,
 },
 },
}
\`\`\`

\\* \`channel\`: release channel for npm/git installs — \`"stable"\`, \`"beta"\`, or \`"dev"\`.
\\* \`checkOnStart\`: check for npm updates when the gateway starts (default: \`true\`).
\\* \`auto.enabled\`: enable background auto-update for package installs (default: \`false\`).
\\* \`auto.stableDelayHours\`: minimum delay in hours before stable-channel auto-apply (default: \`6\`; max: \`168\`).
\\* \`auto.stableJitterHours\`: extra stable-channel rollout spread window in hours (default: \`12\`; max: \`168\`).
\\* \`auto.betaCheckIntervalHours\`: how often beta-channel checks run in hours (default: \`1\`; max: \`24\`).

\\*\\*\\*

\## ACP

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 acp: {
 enabled: true,
 dispatch: { enabled: true },
 backend: "acpx",
 defaultAgent: "main",
 allowedAgents: \["main", "ops"\],
 maxConcurrentSessions: 10,

 stream: {
 coalesceIdleMs: 50,
 maxChunkChars: 1000,
 repeatSuppression: true,
 deliveryMode: "live", // live \| final\_only
 hiddenBoundarySeparator: "paragraph", // none \| space \| newline \| paragraph
 maxOutputChars: 50000,
 maxSessionUpdateChars: 500,
 },

 runtime: {
 ttlMinutes: 30,
 },
 },
}
\`\`\`

\\* \`enabled\`: global ACP feature gate (default: \`true\`; set \`false\` to hide ACP dispatch and spawn affordances).
\\* \`dispatch.enabled\`: independent gate for ACP session turn dispatch (default: \`true\`). Set \`false\` to keep ACP commands available while blocking execution.
\\* \`backend\`: default ACP runtime backend id (must match a registered ACP runtime plugin).
 Install the backend plugin first, and if \`plugins.allow\` is set, include the backend plugin id (for example \`acpx\`) or the ACP backend will not load.
\\* \`defaultAgent\`: fallback ACP target agent id when spawns do not specify an explicit target.
\\* \`allowedAgents\`: allowlist of agent ids permitted for ACP runtime sessions; empty means no additional restriction.
\\* \`maxConcurrentSessions\`: maximum concurrently active ACP sessions.
\\* \`stream.coalesceIdleMs\`: idle flush window in ms for streamed text.
\\* \`stream.maxChunkChars\`: maximum chunk size before splitting streamed block projection.
\\* \`stream.repeatSuppression\`: suppress repeated status/tool lines per turn (default: \`true\`).
\\* \`stream.deliveryMode\`: \`"live"\` streams incrementally; \`"final\_only"\` buffers until turn terminal events.
\\* \`stream.hiddenBoundarySeparator\`: separator before visible text after hidden tool events (default: \`"paragraph"\`).
\\* \`stream.maxOutputChars\`: maximum assistant output characters projected per ACP turn.
\\* \`stream.maxSessionUpdateChars\`: maximum characters for projected ACP status/update lines.
\\* \`stream.tagVisibility\`: record of tag names to boolean visibility overrides for streamed events.
\\* \`runtime.ttlMinutes\`: idle TTL in minutes for ACP session workers before eligible cleanup.
\\* \`runtime.installCommand\`: optional install command to run when bootstrapping an ACP runtime environment.

\\*\\*\\*

\## CLI

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 cli: {
 banner: {
 taglineMode: "off", // random \| default \| off
 },
 },
}
\`\`\`

\\* \`cli.banner.taglineMode\` controls banner tagline style:
 \\* \`"random"\` (default): rotating funny/seasonal taglines.
 \\* \`"default"\`: fixed neutral tagline (\`All your chats, one OpenClaw.\`).
 \\* \`"off"\`: no tagline text (banner title/version still shown).
\\* To hide the entire banner (not just taglines), set env \`OPENCLAW\_HIDE\_BANNER=1\`.

\\*\\*\\*

\## Wizard

Metadata written by CLI guided setup flows (\`onboard\`, \`configure\`, \`doctor\`):

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 wizard: {
 lastRunAt: "2026-01-01T00:00:00.000Z",
 lastRunVersion: "2026.1.4",
 lastRunCommit: "abc1234",
 lastRunCommand: "configure",
 lastRunMode: "local",
 },
}
\`\`\`

\\*\\*\\*

\## Identity

See \`agents.list\` identity fields under \[Agent defaults\](/gateway/config-agents#agent-defaults).

\\*\\*\\*

\## Bridge (legacy, removed)

Current builds no longer include the TCP bridge. Nodes connect over the Gateway WebSocket. \`bridge.\*\` keys are no longer part of the config schema (validation fails until removed; \`openclaw doctor --fix\` can strip unknown keys).

 \`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 "bridge": {
 "enabled": true,
 "port": 18790,
 "bind": "tailnet",
 "tls": {
 "enabled": true,
 "autoGenerate": true
 }
 }
 }
 \`\`\`

\\*\\*\\*

\## Cron

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 cron: {
 enabled: true,
 maxConcurrentRuns: 2, // cron dispatch + isolated cron agent-turn execution
 webhook: "https://example.invalid/legacy", // deprecated fallback for stored notify:true jobs
 webhookToken: "replace-with-dedicated-token", // optional bearer token for outbound webhook auth
 sessionRetention: "24h", // duration string or false
 runLog: {
 maxBytes: "2mb", // default 2\_000\_000 bytes
 keepLines: 2000, // default 2000
 },
 },
}
\`\`\`

\\* \`sessionRetention\`: how long to keep completed isolated cron run sessions before pruning from \`sessions.json\`. Also controls cleanup of archived deleted cron transcripts. Default: \`24h\`; set \`false\` to disable.
\\* \`runLog.maxBytes\`: max size per run log file (\`cron/runs/.jsonl\`) before pruning. Default: \`2\_000\_000\` bytes.
\\* \`runLog.keepLines\`: newest lines retained when run-log pruning is triggered. Default: \`2000\`.
\\* \`webhookToken\`: bearer token used for cron webhook POST delivery (\`delivery.mode = "webhook"\`), if omitted no auth header is sent.
\\* \`webhook\`: deprecated legacy fallback webhook URL (http/https) used only for stored jobs that still have \`notify: true\`.

\### \`cron.retry\`

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 cron: {
 retry: {
 maxAttempts: 3,
 backoffMs: \[30000, 60000, 300000\],
 retryOn: \["rate\_limit", "overloaded", "network", "timeout", "server\_error"\],
 },
 },
}
\`\`\`

\\* \`maxAttempts\`: maximum retries for one-shot jobs on transient errors (default: \`3\`; range: \`0\`–\`10\`).
\\* \`backoffMs\`: array of backoff delays in ms for each retry attempt (default: \`\[30000, 60000, 300000\]\`; 1–10 entries).
\\* \`retryOn\`: error types that trigger retries — \`"rate\_limit"\`, \`"overloaded"\`, \`"network"\`, \`"timeout"\`, \`"server\_error"\`. Omit to retry all transient types.

Applies only to one-shot cron jobs. Recurring jobs use separate failure handling.

\### \`cron.failureAlert\`

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 cron: {
 failureAlert: {
 enabled: false,
 after: 3,
 cooldownMs: 3600000,
 includeSkipped: false,
 mode: "announce",
 accountId: "main",
 },
 },
}
\`\`\`

\\* \`enabled\`: enable failure alerts for cron jobs (default: \`false\`).
\\* \`after\`: consecutive failures before an alert fires (positive integer, min: \`1\`).
\\* \`cooldownMs\`: minimum milliseconds between repeated alerts for the same job (non-negative integer).
\\* \`includeSkipped\`: count consecutive skipped runs toward the alert threshold (default: \`false\`). Skipped runs are tracked separately and do not affect execution-error backoff.
\\* \`mode\`: delivery mode — \`"announce"\` sends via a channel message; \`"webhook"\` posts to the configured webhook.
\\* \`accountId\`: optional account or channel id to scope alert delivery.

\### \`cron.failureDestination\`

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 cron: {
 failureDestination: {
 mode: "announce",
 channel: "last",
 to: "channel:C1234567890",
 accountId: "main",
 },
 },
}
\`\`\`

\\* Default destination for cron failure notifications across all jobs.
\\* \`mode\`: \`"announce"\` or \`"webhook"\`; defaults to \`"announce"\` when enough target data exists.
\\* \`channel\`: channel override for announce delivery. \`"last"\` reuses the last known delivery channel.
\\* \`to\`: explicit announce target or webhook URL. Required for webhook mode.
\\* \`accountId\`: optional account override for delivery.
\\* Per-job \`delivery.failureDestination\` overrides this global default.
\\* When neither global nor per-job failure destination is set, jobs that already deliver via \`announce\` fall back to that primary announce target on failure.
\\* \`delivery.failureDestination\` is only supported for \`sessionTarget="isolated"\` jobs unless the job's primary \`delivery.mode\` is \`"webhook"\`.

See \[Cron Jobs\](/automation/cron-jobs). Isolated cron executions are tracked as \[background tasks\](/automation/tasks).

\\*\\*\\*

\## Media model template variables

Template placeholders expanded in \`tools.media.models\[\].args\`:

\| Variable \| Description \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`{{Body}}\` \| Full inbound message body \|
\| \`{{RawBody}}\` \| Raw body (no history/sender wrappers) \|
\| \`{{BodyStripped}}\` \| Body with group mentions stripped \|
\| \`{{From}}\` \| Sender identifier \|
\| \`{{To}}\` \| Destination identifier \|
\| \`{{MessageSid}}\` \| Channel message id \|
\| \`{{SessionId}}\` \| Current session UUID \|
\| \`{{IsNewSession}}\` \| \`"true"\` when new session created \|
\| \`{{MediaUrl}}\` \| Inbound media pseudo-URL \|
\| \`{{MediaPath}}\` \| Local media path \|
\| \`{{MediaType}}\` \| Media type (image/audio/document/…) \|
\| \`{{Transcript}}\` \| Audio transcript \|
\| \`{{Prompt}}\` \| Resolved media prompt for CLI entries \|
\| \`{{MaxChars}}\` \| Resolved max output chars for CLI entries \|
\| \`{{ChatType}}\` \| \`"direct"\` or \`"group"\` \|
\| \`{{GroupSubject}}\` \| Group subject (best effort) \|
\| \`{{GroupMembers}}\` \| Group members preview (best effort) \|
\| \`{{SenderName}}\` \| Sender display name (best effort) \|
\| \`{{SenderE164}}\` \| Sender phone number (best effort) \|
\| \`{{Provider}}\` \| Provider hint (whatsapp, telegram, discord, etc.) \|

\\*\\*\\*

\## Config includes (\`$include\`)

Split config into multiple files:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
// ~/.openclaw/openclaw.json
{
 gateway: { port: 18789 },
 agents: { $include: "./agents.json5" },
 broadcast: {
 $include: \["./clients/mueller.json5", "./clients/schmidt.json5"\],
 },
}
\`\`\`

\*\*Merge behavior:\*\*

\\* Single file: replaces the containing object.
\\* Array of files: deep-merged in order (later overrides earlier).
\\* Sibling keys: merged after includes (override included values).
\\* Nested includes: up to 10 levels deep.
\\* Paths: resolved relative to the including file, but must stay inside the top-level config directory (\`dirname\` of \`openclaw.json\`). Absolute/\`../\` forms are allowed only when they still resolve inside that boundary.
\\* OpenClaw-owned writes that change only one top-level section backed by a single-file include write through to that included file. For example, \`plugins install\` updates \`plugins: { $include: "./plugins.json5" }\` in \`plugins.json5\` and leaves \`openclaw.json\` intact.
\\* Root includes, include arrays, and includes with sibling overrides are read-only for OpenClaw-owned writes; those writes fail closed instead of flattening the config.
\\* Errors: clear messages for missing files, parse errors, and circular includes.

\\*\\*\\*

\*Related: \[Configuration\](/gateway/configuration) · \[Configuration Examples\](/gateway/configuration-examples) · \[Doctor\](/gateway/doctor)\*

\## Related

\\* \[Configuration\](/gateway/configuration)
\\* \[Configuration examples\](/gateway/configuration-examples)

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
as \`.clobbered.\*\`, restores the last-known-good copy, and logs the recovery
reason. The next agent turn also receives a system-event warning so the main
agent does not blindly rewrite the restored config. Promotion to last-known-good
is skipped when a candidate contains redacted secret placeholders such as \`\*\*\*\`.
When every validation issue is scoped to \`plugins.entries....\`, OpenClaw
does not perform whole-file recovery. It keeps the current config active and
surfaces the plugin-local failure so a plugin schema or host-version mismatch
cannot roll back unrelated user settings.

\## Common tasks

 Each channel has its own config section under \`channels.\`. See the dedicated channel page for setup steps:

 \\* \[WhatsApp\](/channels/whatsapp) — \`channels.whatsapp\`
 \\* \[Telegram\](/channels/telegram) — \`channels.telegram\`
 \\* \[Discord\](/channels/discord) — \`channels.discord\`
 \\* \[Feishu\](/channels/feishu) — \`channels.feishu\`
 \\* \[Google Chat\](/channels/googlechat) — \`channels.googlechat\`
 \\* \[Microsoft Teams\](/channels/msteams) — \`channels.msteams\`
 \\* \[Slack\](/channels/slack) — \`channels.slack\`
 \\* \[Signal\](/channels/signal) — \`channels.signal\`
 \\* \[iMessage\](/channels/imessage) — \`channels.imessage\`
 \\* \[Mattermost\](/channels/mattermost) — \`channels.mattermost\`

 All channels share the same DM policy pattern:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 channels: {
 telegram: {
 enabled: true,
 botToken: "123:abc",
 dmPolicy: "pairing", // pairing \| allowlist \| open \| disabled
 allowFrom: \["tg:123"\], // only for allowlist/open
 },
 },
 }
 \`\`\`

 Set the primary model and optional fallbacks:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 agents: {
 defaults: {
 model: {
 primary: "anthropic/claude-sonnet-4-6",
 fallbacks: \["openai/gpt-5.4"\],
 },
 models: {
 "anthropic/claude-sonnet-4-6": { alias: "Sonnet" },
 "openai/gpt-5.4": { alias: "GPT" },
 },
 },
 },
 }
 \`\`\`

 \\* \`agents.defaults.models\` defines the model catalog and acts as the allowlist for \`/model\`.
 \\* Use \`openclaw config set agents.defaults.models '' --strict-json --merge\` to add allowlist entries without removing existing models. Plain replacements that would remove entries are rejected unless you pass \`--replace\`.
 \\* Model refs use \`provider/model\` format (e.g. \`anthropic/claude-opus-4-6\`).
 \\* \`agents.defaults.imageMaxDimensionPx\` controls transcript/tool image downscaling (default \`1200\`); lower values usually reduce vision-token usage on screenshot-heavy runs.
 \\* See \[Models CLI\](/concepts/models) for switching models in chat and \[Model Failover\](/concepts/model-failover) for auth rotation and fallback behavior.
 \\* For custom/self-hosted providers, see \[Custom providers\](/gateway/config-tools#custom-providers-and-base-urls) in the reference.

 DM access is controlled per channel via \`dmPolicy\`:

 \\* \`"pairing"\` (default): unknown senders get a one-time pairing code to approve
 \\* \`"allowlist"\`: only senders in \`allowFrom\` (or the paired allow store)
 \\* \`"open"\`: allow all inbound DMs (requires \`allowFrom: \["\*"\]\`)
 \\* \`"disabled"\`: ignore all DMs

 For groups, use \`groupPolicy\` + \`groupAllowFrom\` or channel-specific allowlists.

 See the \[full reference\](/gateway/config-channels#dm-and-group-access) for per-channel details.

 Group messages default to \*\*require mention\*\*. Configure trigger patterns per agent, and keep visible room replies on the default message-tool path unless you intentionally want legacy automatic final replies:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 messages: {
 visibleReplies: "automatic", // set "message\_tool" to require message-tool sends everywhere
 groupChat: {
 visibleReplies: "message\_tool", // default; use "automatic" for legacy room replies
 },
 },
 agents: {
 list: \[\
 {\
 id: "main",\
 groupChat: {\
 mentionPatterns: \["@openclaw", "openclaw"\],\
 },\
 },\
 \],
 },
 channels: {
 whatsapp: {
 groups: { "\*": { requireMention: true } },
 },
 },
 }
 \`\`\`

 \\* \*\*Metadata mentions\*\*: native @-mentions (WhatsApp tap-to-mention, Telegram @bot, etc.)
 \\* \*\*Text patterns\*\*: safe regex patterns in \`mentionPatterns\`
 \\* \*\*Visible replies\*\*: \`messages.visibleReplies\` can require message-tool sends globally; \`messages.groupChat.visibleReplies\` overrides that for groups/channels.
 \\* See \[full reference\](/gateway/config-channels#group-chat-mention-gating) for visible reply modes, per-channel overrides, and self-chat mode.

 Use \`agents.defaults.skills\` for a shared baseline, then override specific
 agents with \`agents.list\[\].skills\`:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 agents: {
 defaults: {
 skills: \["github", "weather"\],
 },
 list: \[\
 { id: "writer" }, // inherits github, weather\
 { id: "docs", skills: \["docs-search"\] }, // replaces defaults\
 { id: "locked-down", skills: \[\] }, // no skills\
 \],
 },
 }
 \`\`\`

 \\* Omit \`agents.defaults.skills\` for unrestricted skills by default.
 \\* Omit \`agents.list\[\].skills\` to inherit the defaults.
 \\* Set \`agents.list\[\].skills: \[\]\` for no skills.
 \\* See \[Skills\](/tools/skills), \[Skills config\](/tools/skills-config), and
 the \[Configuration Reference\](/gateway/config-agents#agents-defaults-skills).

 Control how aggressively the gateway restarts channels that look stale:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
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
 \`\`\`

 \\* Set \`gateway.channelHealthCheckMinutes: 0\` to disable health-monitor restarts globally.
 \\* \`channelStaleEventThresholdMinutes\` should be greater than or equal to the check interval.
 \\* Use \`channels..healthMonitor.enabled\` or \`channels..accounts..healthMonitor.enabled\` to disable auto-restarts for one channel or account without disabling the global monitor.
 \\* See \[Health Checks\](/gateway/health) for operational debugging and the \[full reference\](/gateway/configuration-reference#gateway) for all fields.

 Give local clients more time to complete the pre-auth WebSocket handshake on
 loaded or low-powered hosts:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 gateway: {
 handshakeTimeoutMs: 30000,
 },
 }
 \`\`\`

 \\* Default is \`15000\` milliseconds.
 \\* \`OPENCLAW\_HANDSHAKE\_TIMEOUT\_MS\` still takes precedence for one-off service or shell overrides.
 \\* Prefer fixing startup/event-loop stalls first; this knob is for hosts that are healthy but slow during warmup.

 Sessions control conversation continuity and isolation:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 session: {
 dmScope: "per-channel-peer", // recommended for multi-user
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
 \`\`\`

 \\* \`dmScope\`: \`main\` (shared) \| \`per-peer\` \| \`per-channel-peer\` \| \`per-account-channel-peer\`
 \\* \`threadBindings\`: global defaults for thread-bound session routing (Discord supports \`/focus\`, \`/unfocus\`, \`/agents\`, \`/session idle\`, and \`/session max-age\`).
 \\* See \[Session Management\](/concepts/session) for scoping, identity links, and send policy.
 \\* See \[full reference\](/gateway/config-agents#session) for all fields.

 Run agent sessions in isolated sandbox runtimes:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 agents: {
 defaults: {
 sandbox: {
 mode: "non-main", // off \| non-main \| all
 scope: "agent", // session \| agent \| shared
 },
 },
 },
 }
 \`\`\`

 Build the image first — from a source checkout run \`scripts/sandbox-setup.sh\`, or from an npm install see the inline \`docker build\` command in \[Sandboxing § Images and setup\](/gateway/sandboxing#images-and-setup).

 See \[Sandboxing\](/gateway/sandboxing) for the full guide and \[full reference\](/gateway/config-agents#agentsdefaultssandbox) for all options.

 Relay-backed push is configured in \`openclaw.json\`.

 Set this in gateway config:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
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
 \`\`\`

 CLI equivalent:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw config set gateway.push.apns.relay.baseUrl https://relay.example.com
 \`\`\`

 What this does:

 \\* Lets the gateway send \`push.test\`, wake nudges, and reconnect wakes through the external relay.
 \\* Uses a registration-scoped send grant forwarded by the paired iOS app. The gateway does not need a deployment-wide relay token.
 \\* Binds each relay-backed registration to the gateway identity that the iOS app paired with, so another gateway cannot reuse the stored registration.
 \\* Keeps local/manual iOS builds on direct APNs. Relay-backed sends apply only to official distributed builds that registered through the relay.
 \\* Must match the relay base URL baked into the official/TestFlight iOS build, so registration and send traffic reach the same relay deployment.

 End-to-end flow:

 1\. Install an official/TestFlight iOS build that was compiled with the same relay base URL.
 2\. Configure \`gateway.push.apns.relay.baseUrl\` on the gateway.
 3\. Pair the iOS app to the gateway and let both node and operator sessions connect.
 4\. The iOS app fetches the gateway identity, registers with the relay using App Attest plus the app receipt, and then publishes the relay-backed \`push.apns.register\` payload to the paired gateway.
 5\. The gateway stores the relay handle and send grant, then uses them for \`push.test\`, wake nudges, and reconnect wakes.

 Operational notes:

 \\* If you switch the iOS app to a different gateway, reconnect the app so it can publish a new relay registration bound to that gateway.
 \\* If you ship a new iOS build that points at a different relay deployment, the app refreshes its cached relay registration instead of reusing the old relay origin.

 Compatibility note:

 \\* \`OPENCLAW\_APNS\_RELAY\_BASE\_URL\` and \`OPENCLAW\_APNS\_RELAY\_TIMEOUT\_MS\` still work as temporary env overrides.
 \\* \`OPENCLAW\_APNS\_RELAY\_ALLOW\_HTTP=true\` remains a loopback-only development escape hatch; do not persist HTTP relay URLs in config.

 See \[iOS App\](/platforms/ios#relay-backed-push-for-official-builds) for the end-to-end flow and \[Authentication and trust flow\](/platforms/ios#authentication-and-trust-flow) for the relay security model.

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
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
 \`\`\`

 \\* \`every\`: duration string (\`30m\`, \`2h\`). Set \`0m\` to disable.
 \\* \`target\`: \`last\` \| \`none\` \| \`\` (for example \`discord\`, \`matrix\`, \`telegram\`, or \`whatsapp\`)
 \\* \`directPolicy\`: \`allow\` (default) or \`block\` for DM-style heartbeat targets
 \\* See \[Heartbeat\](/gateway/heartbeat) for the full guide.

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
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
 \`\`\`

 \\* \`sessionRetention\`: prune completed isolated run sessions from \`sessions.json\` (default \`24h\`; set \`false\` to disable).
 \\* \`runLog\`: prune \`cron/runs/.jsonl\` by size and retained lines.
 \\* See \[Cron jobs\](/automation/cron-jobs) for feature overview and CLI examples.

 Enable HTTP webhook endpoints on the Gateway:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 hooks: {
 enabled: true,
 token: "shared-secret",
 path: "/hooks",
 defaultSessionKey: "hook:ingress",
 allowRequestSessionKey: false,
 allowedSessionKeyPrefixes: \["hook:"\],
 mappings: \[\
 {\
 match: { path: "gmail" },\
 action: "agent",\
 agentId: "main",\
 deliver: true,\
 },\
 \],
 },
 }
 \`\`\`

 Security note:

 \\* Treat all hook/webhook payload content as untrusted input.
 \\* Use a dedicated \`hooks.token\`; do not reuse the shared Gateway token.
 \\* Hook auth is header-only (\`Authorization: Bearer ...\` or \`x-openclaw-token\`); query-string tokens are rejected.
 \\* \`hooks.path\` cannot be \`/\`; keep webhook ingress on a dedicated subpath such as \`/hooks\`.
 \\* Keep unsafe-content bypass flags disabled (\`hooks.gmail.allowUnsafeExternalContent\`, \`hooks.mappings\[\].allowUnsafeExternalContent\`) unless doing tightly scoped debugging.
 \\* If you enable \`hooks.allowRequestSessionKey\`, also set \`hooks.allowedSessionKeyPrefixes\` to bound caller-selected session keys.
 \\* For hook-driven agents, prefer strong modern model tiers and strict tool policy (for example messaging-only plus sandboxing where possible).

 See \[full reference\](/gateway/configuration-reference#hooks) for all mapping options and Gmail integration.

 Run multiple isolated agents with separate workspaces and sessions:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 agents: {
 list: \[\
 { id: "home", default: true, workspace: "~/.openclaw/workspace-home" },\
 { id: "work", workspace: "~/.openclaw/workspace-work" },\
 \],
 },
 bindings: \[\
 { agentId: "home", match: { channel: "whatsapp", accountId: "personal" } },\
 { agentId: "work", match: { channel: "whatsapp", accountId: "biz" } },\
 \],
 }
 \`\`\`

 See \[Multi-Agent\](/concepts/multi-agent) and \[full reference\](/gateway/config-agents#multi-agent-routing) for binding rules and per-agent access profiles.

 Use \`$include\` to organize large configs:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 // ~/.openclaw/openclaw.json
 {
 gateway: { port: 18789 },
 agents: { $include: "./agents.json5" },
 broadcast: {
 $include: \["./clients/a.json5", "./clients/b.json5"\],
 },
 }
 \`\`\`

 \\* \*\*Single file\*\*: replaces the containing object
 \\* \*\*Array of files\*\*: deep-merged in order (later wins)
 \\* \*\*Sibling keys\*\*: merged after includes (override included values)
 \\* \*\*Nested includes\*\*: supported up to 10 levels deep
 \\* \*\*Relative paths\*\*: resolved relative to the including file
 \\* \*\*OpenClaw-owned writes\*\*: when a write changes only one top-level section
 backed by a single-file include such as \`plugins: { $include: "./plugins.json5" }\`,
 OpenClaw updates that included file and leaves \`openclaw.json\` intact
 \\* \*\*Unsupported write-through\*\*: root includes, include arrays, and includes
 with sibling overrides fail closed for OpenClaw-owned writes instead of
 flattening the config
 \\* \*\*Confinement\*\*: \`$include\` paths must resolve under the directory holding
 \`openclaw.json\`. To share a tree across machines or users, set
 \`OPENCLAW\_INCLUDE\_ROOTS\` to a path-list (\`:\` on POSIX, \`;\` on Windows) of
 additional directories that includes may reference. Symlinks are resolved
 and re-checked, so a path that lexically lives in a config dir but whose
 real target escapes every allowed root is still rejected.
 \\* \*\*Error handling\*\*: clear errors for missing files, parse errors, and circular includes

\## Config hot reload

The Gateway watches \`~/.openclaw/openclaw.json\` and applies changes automatically — no manual restart needed for most settings.

Direct file edits are treated as untrusted until they validate. The watcher waits
for editor temp-write/rename churn to settle, reads the final file, and rejects
invalid external edits by restoring the last-known-good config. OpenClaw-owned
config writes use the same schema gate before writing; destructive clobbers such
as dropping \`gateway.mode\` or shrinking the file by more than half are rejected
and saved as \`.rejected.\*\` for inspection.

Plugin-local validation failures are the exception: if all issues are under
\`plugins.entries....\`, reload keeps the current config and reports the plugin
issue instead of restoring \`.last-good\`.

If you see \`Config auto-restored from last-known-good\` or
\`config reload restored last-known-good config\` in logs, inspect the matching
\`.clobbered.\*\` file next to \`openclaw.json\`, fix the rejected payload, then run
\`openclaw config validate\`. See \[Gateway troubleshooting\](/gateway/troubleshooting#gateway-restored-last-known-good-config)
for the recovery checklist.

\### Reload modes

\| Mode \| Behavior \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \*\*\`hybrid\`\*\* (default) \| Hot-applies safe changes instantly. Automatically restarts for critical ones. \|
\| \*\*\`hot\`\*\* \| Hot-applies safe changes only. Logs a warning when a restart is needed — you handle it. \|
\| \*\*\`restart\`\*\* \| Restarts the Gateway on any config change, safe or not. \|
\| \*\*\`off\`\*\* \| Disables file watching. Changes take effect on the next manual restart. \|

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 gateway: {
 reload: { mode: "hybrid", debounceMs: 300 },
 },
}
\`\`\`

\### What hot-applies vs what needs a restart

Most fields hot-apply without downtime. In \`hybrid\` mode, restart-required changes are handled automatically.

\| Category \| Fields \| Restart needed? \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| Channels \| \`channels.\*\`, \`web\` (WhatsApp) — all built-in and plugin channels \| No \|
\| Agent & models \| \`agent\`, \`agents\`, \`models\`, \`routing\` \| No \|
\| Automation \| \`hooks\`, \`cron\`, \`agent.heartbeat\` \| No \|
\| Sessions & messages \| \`session\`, \`messages\` \| No \|
\| Tools & media \| \`tools\`, \`browser\`, \`skills\`, \`mcp\`, \`audio\`, \`talk\` \| No \|
\| UI & misc \| \`ui\`, \`logging\`, \`identity\`, \`bindings\` \| No \|
\| Gateway server \| \`gateway.\*\` (port, bind, auth, tailscale, TLS, HTTP) \| \*\*Yes\*\* \|
\| Infrastructure \| \`discovery\`, \`canvasHost\`, \`plugins\` \| \*\*Yes\*\* \|

 \`gateway.reload\` and \`gateway.remote\` are exceptions — changing them does \*\*not\*\* trigger a restart.

\### Reload planning

When you edit a source file that is referenced through \`$include\`, OpenClaw plans
the reload from the source-authored layout, not the flattened in-memory view.
That keeps hot-reload decisions (hot-apply vs restart) predictable even when a
single top-level section lives in its own included file such as
\`plugins: { $include: "./plugins.json5" }\`. Reload planning fails closed if the
source layout is ambiguous.

\## Config RPC (programmatic updates)

For tooling that writes config over the gateway API, prefer this flow:

\\* \`config.schema.lookup\` to inspect one subtree (shallow schema node + child
 summaries)
\\* \`config.get\` to fetch the current snapshot plus \`hash\`
\\* \`config.patch\` for partial updates (JSON merge patch: objects merge, \`null\`
 deletes, arrays replace)
\\* \`config.apply\` only when you intend to replace the entire config
\\* \`update.run\` for explicit self-update plus restart
\\* \`update.status\` to inspect the latest update restart sentinel and verify the running version after a restart

Agents should treat \`config.schema.lookup\` as the first stop for exact
field-level docs and constraints. Use \[Configuration reference\](/gateway/configuration-reference)
when they need the broader config map, defaults, or links to dedicated
subsystem references.

 Control-plane writes (\`config.apply\`, \`config.patch\`, \`update.run\`) are
 rate-limited to 3 requests per 60 seconds per \`deviceId+clientIp\`. Restart
 requests coalesce and then enforce a 30-second cooldown between restart cycles.
 \`update.status\` is read-only but admin-scoped because the restart sentinel can
 include update step summaries and command output tails.

Example partial patch:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw gateway call config.get --params '{}' # capture payload.hash
openclaw gateway call config.patch --params '{
 "raw": "{ channels: { telegram: { groups: { \\"\*\\": { requireMention: false } } } } }",
 "baseHash": ""
}'
\`\`\`

Both \`config.apply\` and \`config.patch\` accept \`raw\`, \`baseHash\`, \`sessionKey\`,
\`note\`, and \`restartDelayMs\`. \`baseHash\` is required for both methods when a
config already exists.

\## Environment variables

OpenClaw reads env vars from the parent process plus:

\\* \`.env\` from the current working directory (if present)
\\* \`~/.openclaw/.env\` (global fallback)

Neither file overrides existing env vars. You can also set inline env vars in config:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 env: {
 OPENROUTER\_API\_KEY: "sk-or-...",
 vars: { GROQ\_API\_KEY: "gsk-..." },
 },
}
\`\`\`

 If enabled and expected keys aren't set, OpenClaw runs your login shell and imports only the missing keys:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 env: {
 shellEnv: { enabled: true, timeoutMs: 15000 },
 },
 }
 \`\`\`

 Env var equivalent: \`OPENCLAW\_LOAD\_SHELL\_ENV=1\`

 Reference env vars in any config string value with \`${VAR\_NAME}\`:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 gateway: { auth: { token: "${OPENCLAW\_GATEWAY\_TOKEN}" } },
 models: { providers: { custom: { apiKey: "${CUSTOM\_API\_KEY}" } } },
 }
 \`\`\`

 Rules:

 \\* Only uppercase names matched: \`\[A-Z\_\]\[A-Z0-9\_\]\*\`
 \\* Missing/empty vars throw an error at load time
 \\* Escape with \`$${VAR}\` for literal output
 \\* Works inside \`$include\` files
 \\* Inline substitution: \`"${BASE}/v1"\` → \`"https://api.example.com/v1"\`

 For fields that support SecretRef objects, you can use:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 models: {
 providers: {
 openai: { apiKey: { source: "env", provider: "default", id: "OPENAI\_API\_KEY" } },
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
 \`\`\`

 SecretRef details (including \`secrets.providers\` for \`env\`/\`file\`/\`exec\`) are in \[Secrets Management\](/gateway/secrets).
 Supported credential paths are listed in \[SecretRef Credential Surface\](/reference/secretref-credential-surface).

See \[Environment\](/help/environment) for full precedence and sources.

\## Full reference

For the complete field-by-field reference, see \*\*\[Configuration Reference\](/gateway/configuration-reference)\*\*.

\\*\\*\\*

\*Related: \[Configuration Examples\](/gateway/configuration-examples) · \[Configuration Reference\](/gateway/configuration-reference) · \[Doctor\](/gateway/doctor)\*

\## Related

\\* \[Configuration reference\](/gateway/configuration-reference)
\\* \[Configuration examples\](/gateway/configuration-examples)
\\* \[Gateway runbook\](/gateway)

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
- status codes, durations, byte counts, queue state, and memory readings
- sanitized log metadata and redacted operational messages
- config shape and non-secret feature settings

The export omits or redacts:

- chat text, prompts, instructions, webhook bodies, and tool outputs
- credentials, API keys, tokens, cookies, and secret values
- raw request or response bodies
- account ids, message ids, raw session ids, hostnames, and local usernames

When a log message looks like user, chat, prompt, or tool payload text, the
export keeps only that a message was omitted and the byte count.

## Stability recorder

The Gateway records a bounded, payload-free stability stream by default when
diagnostics are enabled. It is for operational facts, not content.The same diagnostic heartbeat records liveness samples when the Gateway keeps
running but the Node.js event loop or CPU looks saturated. These
`diagnostic.liveness.warning` events include event-loop delay, event-loop
utilization, CPU-core ratio, and active/waiting/queued session counts. Idle
samples stay in telemetry at `info` level; they are only logged as Gateway
warnings when diagnostic work is active, waiting, or queued. They do not
restart the Gateway by themselves.Inspect the live recorder:

```
openclaw gateway stability
openclaw gateway stability --type payload.large
openclaw gateway stability --json
```

Inspect the newest persisted stability bundle after a fatal exit, shutdown
timeout, or restart startup failure:

```
openclaw gateway stability --bundle latest
```

Create a diagnostics zip from the newest persisted bundle:

```
openclaw gateway stability --bundle latest --export
```

Persisted bundles live under `~/.openclaw/logs/stability/` when events exist.

## Useful options

```
openclaw gateway diagnostics export \
  --output openclaw-diagnostics.zip \
  --log-lines 5000 \
  --log-bytes 1000000
```

- `--output <path>`: write to a specific zip path.
- `--log-lines <count>`: maximum sanitized log lines to include.
- `--log-bytes <bytes>`: maximum log bytes to inspect.
- `--url <url>`: Gateway WebSocket URL for status and health snapshots.
- `--token <token>`: Gateway token for status and health snapshots.
- `--password <password>`: Gateway password for status and health snapshots.
- `--timeout <ms>`: status and health snapshot timeout.
- `--no-stability-bundle`: skip persisted stability bundle lookup.
- `--json`: print machine-readable export metadata.

## Disable diagnostics

Diagnostics are enabled by default. To disable the stability recorder and
diagnostic event collection:

```
{
  diagnostics: {
    enabled: false,
  },
}
```

Disabling diagnostics reduces bug-report detail. It does not affect normal
Gateway logging.

## Related

- [Health checks](https://docs.openclaw.ai/gateway/health)
- [Gateway CLI](https://docs.openclaw.ai/cli/gateway#gateway-diagnostics-export)
- [Gateway protocol](https://docs.openclaw.ai/gateway/protocol#system-and-identity)
- [Logging](https://docs.openclaw.ai/logging)
- [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) — separate flow for streaming diagnostics to a collector

[Gateway logging](https://docs.openclaw.ai/gateway/logging) [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)

Ctrl+I

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
- Supervisor config audit (launchd/systemd/schtasks) with optional repair.
- Embedded proxy environment cleanup for gateway services that captured shell `HTTP_PROXY` / `HTTPS_PROXY` / `NO_PROXY` values during install or update.
- Gateway runtime best-practice checks (Node vs Bun, version-manager paths).
- Gateway port collision diagnostics (default `18789`).

Auth, security, and pairing

- Security warnings for open DM policies.
- Gateway auth checks for local token mode (offers token generation when no token source exists; does not overwrite token SecretRef configs).
- Device pairing trouble detection (pending first-time pair requests, pending role/scope upgrades, stale local device-token cache drift, and paired-record auth drift).

Workspace and shell

- systemd linger check on Linux.
- Workspace bootstrap file size check (truncation/near-limit warnings for context files).
- Skills readiness check for the default agent; reports allowed skills with missing bins, env, config, or OS requirements, and `--fix` can disable unavailable skills in `skills.entries`.
- Shell completion status check and auto-install/upgrade.
- Memory search embedding provider readiness check (local model, remote API key, or QMD binary).
- Source install checks (pnpm workspace mismatch, missing UI assets, missing tsx binary).
- Writes updated config + wizard metadata.

## Dreams UI backfill and reset

The Control UI Dreams scene includes **Backfill**, **Reset**, and **Clear Grounded** actions for the grounded dreaming workflow. These actions use gateway doctor-style RPC methods, but they are **not** part of `openclaw doctor` CLI repair/migration.What they do:

- **Backfill** scans historical `memory/YYYY-MM-DD.md` files in the active workspace, runs the grounded REM diary pass, and writes reversible backfill entries into `DREAMS.md`.
- **Reset** removes only those marked backfill diary entries from `DREAMS.md`.
- **Clear Grounded** removes only staged grounded-only short-term entries that came from historical replay and have not accumulated live recall or daily support yet.

What they do **not** do by themselves:

- they do not edit `MEMORY.md`
- they do not run full doctor migrations
- they do not automatically stage grounded candidates into the live short-term promotion store unless you explicitly run the staged CLI path first

If you want grounded historical replay to influence the normal deep promotion lane, use the CLI flow instead:

```
openclaw memory rem-backfill --path ./memory --stage-short-term
```

That stages grounded durable candidates into the short-term dreaming store while keeping `DREAMS.md` as the review surface.

## Detailed behavior and rationale

0\. Optional update (git installs)

If this is a git checkout and doctor is running interactively, it offers to update (fetch/rebase/build) before running doctor.

1\. Config normalization

If the config contains legacy value shapes (for example `messages.ackReaction` without a channel-specific override), doctor normalizes them into the current schema.That includes legacy Talk flat fields. Current public Talk config is `talk.provider` \+ `talk.providers.<provider>`. Doctor rewrites old `talk.voiceId` / `talk.voiceAliases` / `talk.modelId` / `talk.outputFormat` / `talk.apiKey` shapes into the provider map.Doctor also warns when `plugins.allow` is non-empty and tool policy uses
wildcard or plugin-owned tool entries. `tools.allow: ["*"]` only matches tools
from plugins that actually load; it does not bypass the exclusive plugin
allowlist.

2\. Legacy config key migrations

When the config contains deprecated keys, other commands refuse to run and ask you to run `openclaw doctor`.Doctor will:

- Explain which legacy keys were found.
- Show the migration it applied.
- Rewrite `~/.openclaw/openclaw.json` with the updated schema.

The Gateway also auto-runs doctor migrations on startup when it detects a legacy config format, so stale configs are repaired without manual intervention. Cron job store migrations are handled by `openclaw doctor --fix`.Current migrations:

- `routing.allowFrom` → `channels.whatsapp.allowFrom`
- `routing.groupChat.requireMention` → `channels.whatsapp/telegram/imessage.groups."*".requireMention`
- `routing.groupChat.historyLimit` → `messages.groupChat.historyLimit`
- `routing.groupChat.mentionPatterns` → `messages.groupChat.mentionPatterns`
- `routing.queue` → `messages.queue`
- `routing.bindings` → top-level `bindings`
- `routing.agents`/`routing.defaultAgentId` → `agents.list` \+ `agents.list[].default`
- legacy `talk.voiceId`/`talk.voiceAliases`/`talk.modelId`/`talk.outputFormat`/`talk.apiKey` → `talk.provider` \+ `talk.providers.<provider>`
- `routing.agentToAgent` → `tools.agentToAgent`
- `routing.transcribeAudio` → `tools.media.audio.models`
- `messages.tts.<provider>` (`openai`/`elevenlabs`/`microsoft`/`edge`) → `messages.tts.providers.<provider>`
- `messages.tts.provider: "edge"` and `messages.tts.providers.edge` → `messages.tts.provider: "microsoft"` and `messages.tts.providers.microsoft`
- `channels.discord.voice.tts.<provider>` (`openai`/`elevenlabs`/`microsoft`/`edge`) → `channels.discord.voice.tts.providers.<provider>`
- `channels.discord.accounts.<id>.voice.tts.<provider>` (`openai`/`elevenlabs`/`microsoft`/`edge`) → `channels.discord.accounts.<id>.voice.tts.providers.<provider>`
- `plugins.entries.voice-call.config.tts.<provider>` (`openai`/`elevenlabs`/`microsoft`/`edge`) → `plugins.entries.voice-call.config.tts.providers.<provider>`
- `plugins.entries.voice-call.config.tts.provider: "edge"` and `plugins.entries.voice-call.config.tts.providers.edge` → `provider: "microsoft"` and `providers.microsoft`
- `plugins.entries.voice-call.config.provider: "log"` → `"mock"`
- `plugins.entries.voice-call.config.twilio.from` → `plugins.entries.voice-call.config.fromNumber`
- `plugins.entries.voice-call.config.streaming.sttProvider` → `plugins.entries.voice-call.config.streaming.provider`
- `plugins.entries.voice-call.config.streaming.openaiApiKey|sttModel|silenceDurationMs|vadThreshold` → `plugins.entries.voice-call.config.streaming.providers.openai.*`
- `bindings[].match.accountID` → `bindings[].match.accountId`
- For channels with named `accounts` but lingering single-account top-level channel values, move those account-scoped values into the promoted account chosen for that channel (`accounts.default` for most channels; Matrix can preserve an existing matching named/default target)
- `identity` → `agents.list[].identity`
- `agent.*` → `agents.defaults` \+ `tools.*` (tools/elevated/exec/sandbox/subagents)
- `agent.model`/`allowedModels`/`modelAliases`/`modelFallbacks`/`imageModelFallbacks` → `agents.defaults.models` \+ `agents.defaults.model.primary/fallbacks` \+ `agents.defaults.imageModel.primary/fallbacks`
- remove `agents.defaults.llm`; use `models.providers.<id>.timeoutSeconds` for slow provider/model timeouts
- `browser.ssrfPolicy.allowPrivateNetwork` → `browser.ssrfPolicy.dangerouslyAllowPrivateNetwork`
- `browser.profiles.*.driver: "extension"` → `"existing-session"`
- remove `browser.relayBindHost` (legacy extension relay setting)
- legacy `models.providers.*.api: "openai"` → `"openai-completions"` (gateway startup also skips providers whose `api` is set to a future or unknown enum value rather than failing closed)

Doctor warnings also include account-default guidance for multi-account channels:

- If two or more `channels.<channel>.accounts` entries are configured without `channels.<channel>.defaultAccount` or `accounts.default`, doctor warns that fallback routing can pick an unexpected account.
- If `channels.<channel>.defaultAccount` is set to an unknown account ID, doctor warns and lists configured account IDs.

2b. OpenCode provider overrides

If you’ve added `models.providers.opencode`, `opencode-zen`, or `opencode-go` manually, it overrides the built-in OpenCode catalog from `@mariozechner/pi-ai`. That can force models onto the wrong API or zero out costs. Doctor warns so you can remove the override and restore per-model API routing + costs.

2c. Browser migration and Chrome MCP readiness

If your browser config still points at the removed Chrome extension path, doctor normalizes it to the current host-local Chrome MCP attach model:

- `browser.profiles.*.driver: "extension"` becomes `"existing-session"`
- `browser.relayBindHost` is removed

Doctor also audits the host-local Chrome MCP path when you use `defaultProfile: "user"` or a configured `existing-session` profile:

- checks whether Google Chrome is installed on the same host for default auto-connect profiles
- checks the detected Chrome version and warns when it is below Chrome 144
- reminds you to enable remote debugging in the browser inspect page (for example `chrome://inspect/#remote-debugging`, `brave://inspect/#remote-debugging`, or `edge://inspect/#remote-debugging`)

Doctor cannot enable the Chrome-side setting for you. Host-local Chrome MCP still requires:

- a Chromium-based browser 144+ on the gateway/node host
- the browser running locally
- remote debugging enabled in that browser
- approving the first attach consent prompt in the browser

Readiness here is only about local attach prerequisites. Existing-session keeps the current Chrome MCP route limits; advanced routes like `responsebody`, PDF export, download interception, and batch actions still require a managed browser or raw CDP profile.This check does **not** apply to Docker, sandbox, remote-browser, or other headless flows. Those continue to use raw CDP.

2d. OAuth TLS prerequisites

When an OpenAI Codex OAuth profile is configured, doctor probes the OpenAI authorization endpoint to verify that the local Node/OpenSSL TLS stack can validate the certificate chain. If the probe fails with a certificate error (for example `UNABLE_TO_GET_ISSUER_CERT_LOCALLY`, expired cert, or self-signed cert), doctor prints platform-specific fix guidance. On macOS with a Homebrew Node, the fix is usually `brew postinstall ca-certificates`. With `--deep`, the probe runs even if the gateway is healthy.

2e. Codex OAuth provider overrides

If you previously added legacy OpenAI transport settings under `models.providers.openai-codex`, they can shadow the built-in Codex OAuth provider path that newer releases use automatically. Doctor warns when it sees those old transport settings alongside Codex OAuth so you can remove or rewrite the stale transport override and get the built-in routing/fallback behavior back. Custom proxies and header-only overrides are still supported and do not trigger this warning.

2f. Codex plugin route warnings

When the bundled Codex plugin is enabled, doctor also checks whether `openai-codex/*` primary model refs still resolve through the default PI runner. That combination is valid when you want Codex OAuth/subscription auth through PI, but it is easy to confuse with the native Codex app-server harness. Doctor warns and points to the explicit app-server shape: `openai/*` plus `agentRuntime.id: "codex"` or `OPENCLAW_AGENT_RUNTIME=codex`.Doctor does not repair this automatically because both routes are valid:

- `openai-codex/*` \+ PI means “use Codex OAuth/subscription auth through the normal OpenClaw runner.”
- `openai/*` \+ `agentRuntime.id: "codex"` means “run the embedded turn through native Codex app-server.”
- `/codex ...` means “control or bind a native Codex conversation from chat.”
- `/acp ...` or `runtime: "acp"` means “use the external ACP/acpx adapter.”

If the warning appears, choose the route you intended and edit config manually. Keep the warning as-is when PI Codex OAuth is intentional.

3\. Legacy state migrations (disk layout)

Doctor can migrate older on-disk layouts into the current structure:

- Sessions store + transcripts:
  - from `~/.openclaw/sessions/` to `~/.openclaw/agents/<agentId>/sessions/`
- Agent dir:
  - from `~/.openclaw/agent/` to `~/.openclaw/agents/<agentId>/agent/`
- WhatsApp auth state (Baileys):
  - from legacy `~/.openclaw/credentials/*.json` (except `oauth.json`)
  - to `~/.openclaw/credentials/whatsapp/<accountId>/...` (default account id: `default`)

These migrations are best-effort and idempotent; doctor will emit warnings when it leaves any legacy folders behind as backups. The Gateway/CLI also auto-migrates the legacy sessions + agent dir on startup so history/auth/models land in the per-agent path without a manual doctor run. WhatsApp auth is intentionally only migrated via `openclaw doctor`. Talk provider/provider-map normalization now compares by structural equality, so key-order-only diffs no longer trigger repeat no-op `doctor --fix` changes.

3a. Legacy plugin manifest migrations

Doctor scans all installed plugin manifests for deprecated top-level capability keys (`speechProviders`, `realtimeTranscriptionProviders`, `realtimeVoiceProviders`, `mediaUnderstandingProviders`, `imageGenerationProviders`, `videoGenerationProviders`, `webFetchProviders`, `webSearchProviders`). When found, it offers to move them into the `contracts` object and rewrite the manifest file in-place. This migration is idempotent; if the `contracts` key already has the same values, the legacy key is removed without duplicating the data.

3b. Legacy cron store migrations

Doctor also checks the cron job store (`~/.openclaw/cron/jobs.json` by default, or `cron.store` when overridden) for old job shapes that the scheduler still accepts for compatibility.Current cron cleanups include:

- `jobId` → `id`
- `schedule.cron` → `schedule.expr`
- top-level payload fields (`message`, `model`, `thinking`, …) → `payload`
- top-level delivery fields (`deliver`, `channel`, `to`, `provider`, …) → `delivery`
- payload `provider` delivery aliases → explicit `delivery.channel`
- simple legacy `notify: true` webhook fallback jobs → explicit `delivery.mode="webhook"` with `delivery.to=cron.webhook`

Doctor only auto-migrates `notify: true` jobs when it can do so without changing behavior. If a job combines legacy notify fallback with an existing non-webhook delivery mode, doctor warns and leaves that job for manual review.On Linux, doctor also warns when the user’s crontab still invokes legacy `~/.openclaw/bin/ensure-whatsapp.sh`. That host-local script is not maintained by current OpenClaw and can write false `Gateway inactive` messages to `~/.openclaw/logs/whatsapp-health.log` when cron cannot reach the systemd user bus. Remove the stale crontab entry with `crontab -e`; use `openclaw channels status --probe`, `openclaw doctor`, and `openclaw gateway status` for current health checks.

3c. Session lock cleanup

Doctor scans every agent session directory for stale write-lock files — files left behind when a session exited abnormally. For each lock file found it reports: the path, PID, whether the PID is still alive, lock age, and whether it is considered stale (dead PID or older than 30 minutes). In `--fix` / `--repair` mode it removes stale lock files automatically; otherwise it prints a note and instructs you to rerun with `--fix`.

3d. Session transcript branch repair

Doctor scans agent session JSONL files for the duplicated branch shape created by the 2026.4.24 prompt transcript rewrite bug: an abandoned user turn with OpenClaw internal runtime context plus an active sibling containing the same visible user prompt. In `--fix` / `--repair` mode, doctor backs up each affected file next to the original and rewrites the transcript to the active branch so gateway history and memory readers no longer see duplicate turns.

4\. State integrity checks (session persistence, routing, and safety)

The state directory is the operational brainstem. If it vanishes, you lose sessions, credentials, logs, and config (unless you have backups elsewhere).Doctor checks:

- **State dir missing**: warns about catastrophic state loss, prompts to recreate the directory, and reminds you that it cannot recover missing data.
- **State dir permissions**: verifies writability; offers to repair permissions (and emits a `chown` hint when owner/group mismatch is detected).
- **macOS cloud-synced state dir**: warns when state resolves under iCloud Drive (`~/Library/Mobile Documents/com~apple~CloudDocs/...`) or `~/Library/CloudStorage/...` because sync-backed paths can cause slower I/O and lock/sync races.
- **Linux SD or eMMC state dir**: warns when state resolves to an `mmcblk*` mount source, because SD or eMMC-backed random I/O can be slower and wear faster under session and credential writes.
- **Session dirs missing**: `sessions/` and the session store directory are required to persist history and avoid `ENOENT` crashes.
- **Transcript mismatch**: warns when recent session entries have missing transcript files.
- **Main session “1-line JSONL”**: flags when the main transcript has only one line (history is not accumulating).
- **Multiple state dirs**: warns when multiple `~/.openclaw` folders exist across home directories or when `OPENCLAW_STATE_DIR` points elsewhere (history can split between installs).
- **Remote mode reminder**: if `gateway.mode=remote`, doctor reminds you to run it on the remote host (the state lives there).
- **Config file permissions**: warns if `~/.openclaw/openclaw.json` is group/world readable and offers to tighten to `600`.

5\. Model auth health (OAuth expiry)

Doctor inspects OAuth profiles in the auth store, warns when tokens are expiring/expired, and can refresh them when safe. If the Anthropic OAuth/token profile is stale, it suggests an Anthropic API key or the Anthropic setup-token path. Refresh prompts only appear when running interactively (TTY); `--non-interactive` skips refresh attempts.When an OAuth refresh fails permanently (for example `refresh_token_reused`, `invalid_grant`, or a provider telling you to sign in again), doctor reports that re-auth is required and prints the exact `openclaw models auth login --provider ...` command to run.Doctor also reports auth profiles that are temporarily unusable due to:

- short cooldowns (rate limits/timeouts/auth failures)
- longer disables (billing/credit failures)

6\. Hooks model validation

If `hooks.gmail.model` is set, doctor validates the model reference against the catalog and allowlist and warns when it won’t resolve or is disallowed.

7\. Sandbox image repair

When sandboxing is enabled, doctor checks Docker images and offers to build or switch to legacy names if the current image is missing.

7b. Plugin install cleanup

Doctor removes legacy OpenClaw-generated plugin dependency staging state in `openclaw doctor --fix` / `openclaw doctor --repair` mode. This covers stale generated dependency roots, old install-stage directories, and package-local debris from earlier bundled-plugin dependency repair code.Doctor can also reinstall configured downloadable plugins when the config references them but the local plugin registry cannot find them. For the 2026.5.2 bundled-plugin externalization, doctor automatically installs downloadable plugins that the existing config already uses and then relies on `meta.lastTouchedVersion` to run that release pass only once. Gateway startup and config reload do not run package managers; plugin installs remain explicit doctor/install/update work.

8\. Gateway service migrations and cleanup hints

Doctor detects legacy gateway services (launchd/systemd/schtasks) and offers to remove them and install the OpenClaw service using the current gateway port. It can also scan for extra gateway-like services and print cleanup hints. Profile-named OpenClaw gateway services are considered first-class and are not flagged as “extra.”On Linux, if the user-level gateway service is missing but a system-level OpenClaw gateway service exists, doctor does not install a second user-level service automatically. Inspect with `openclaw gateway status --deep` or `openclaw doctor --deep`, then remove the duplicate or set `OPENCLAW_SERVICE_REPAIR_POLICY=external` when a system supervisor owns the gateway lifecycle.

8b. Startup Matrix migration

When a Matrix channel account has a pending or actionable legacy state migration, doctor (in `--fix` / `--repair` mode) creates a pre-migration snapshot and then runs the best-effort migration steps: legacy Matrix state migration and legacy encrypted-state preparation. Both steps are non-fatal; errors are logged and startup continues. In read-only mode (`openclaw doctor` without `--fix`) this check is skipped entirely.

8c. Device pairing and auth drift

Doctor now inspects device-pairing state as part of the normal health pass.What it reports:

- pending first-time pairing requests
- pending role upgrades for already paired devices
- pending scope upgrades for already paired devices
- public-key mismatch repairs where the device id still matches but the device identity no longer matches the approved record
- paired records missing an active token for an approved role
- paired tokens whose scopes drift outside the approved pairing baseline
- local cached device-token entries for the current machine that predate a gateway-side token rotation or carry stale scope metadata

Doctor does not auto-approve pair requests or auto-rotate device tokens. It prints the exact next steps instead:

- inspect pending requests with `openclaw devices list`
- approve the exact request with `openclaw devices approve <requestId>`
- rotate a fresh token with `openclaw devices rotate --device <deviceId> --role <role>`
- remove and re-approve a stale record with `openclaw devices remove <deviceId>`

This closes the common “already paired but still getting pairing required” hole: doctor now distinguishes first-time pairing from pending role/scope upgrades and from stale token/device-identity drift.

9\. Security warnings

Doctor emits warnings when a provider is open to DMs without an allowlist, or when a policy is configured in a dangerous way.

10\. systemd linger (Linux)

If running as a systemd user service, doctor ensures lingering is enabled so the gateway stays alive after logout.

11\. Workspace status (skills, plugins, and legacy dirs)

Doctor prints a summary of the workspace state for the default agent:

- **Skills status**: counts eligible, missing-requirements, and allowlist-blocked skills.
- **Legacy workspace dirs**: warns when `~/openclaw` or other legacy workspace directories exist alongside the current workspace.
- **Plugin status**: counts enabled/disabled/errored plugins; lists plugin IDs for any errors; reports bundle plugin capabilities.
- **Plugin compatibility warnings**: flags plugins that have compatibility issues with the current runtime.
- **Plugin diagnostics**: surfaces any load-time warnings or errors emitted by the plugin registry.

11b. Bootstrap file size

Doctor checks whether workspace bootstrap files (for example `AGENTS.md`, `CLAUDE.md`, or other injected context files) are near or over the configured character budget. It reports per-file raw vs. injected character counts, truncation percentage, truncation cause (`max/file` or `max/total`), and total injected characters as a fraction of the total budget. When files are truncated or near the limit, doctor prints tips for tuning `agents.defaults.bootstrapMaxChars` and `agents.defaults.bootstrapTotalMaxChars`.

11d. Stale channel plugin cleanup

When `openclaw doctor --fix` removes a missing channel plugin, it also removes the dangling channel-scoped config that referenced that plugin: `channels.<id>` entries, heartbeat targets that named the channel, and `agents.*.models["<channel>/*"]` overrides. This prevents Gateway boot loops where the channel runtime is gone but config still asks the gateway to bind to it.

11c. Shell completion

Doctor checks whether tab completion is installed for the current shell (zsh, bash, fish, or PowerShell):

- If the shell profile uses a slow dynamic completion pattern (`source <(openclaw completion ...)`), doctor upgrades it to the faster cached file variant.
- If completion is configured in the profile but the cache file is missing, doctor regenerates the cache automatically.
- If no completion is configured at all, doctor prompts to install it (interactive mode only; skipped with `--non-interactive`).

Run `openclaw completion --write-state` to regenerate the cache manually.

12\. Gateway auth checks (local token)

Doctor checks local gateway token auth readiness.

- If token mode needs a token and no token source exists, doctor offers to generate one.
- If `gateway.auth.token` is SecretRef-managed but unavailable, doctor warns and does not overwrite it with plaintext.
- `openclaw doctor --generate-gateway-token` forces generation only when no token SecretRef is configured.

12b. Read-only SecretRef-aware repairs

Some repair flows need to inspect configured credentials without weakening runtime fail-fast behavior.

- `openclaw doctor --fix` now uses the same read-only SecretRef summary model as status-family commands for targeted config repairs.
- Example: Telegram `allowFrom` / `groupAllowFrom``@username` repair tries to use configured bot credentials when available.
- If the Telegram bot token is configured via SecretRef but unavailable in the current command path, doctor reports that the credential is configured-but-unavailable and skips auto-resolution instead of crashing or misreporting the token as missing.

13\. Gateway health check + restart

Doctor runs a health check and offers to restart the gateway when it looks unhealthy.

13b. Memory search readiness

Doctor checks whether the configured memory search embedding provider is ready for the default agent. The behavior depends on the configured backend and provider:

- **QMD backend**: probes whether the `qmd` binary is available and startable. If not, prints fix guidance including the npm package and a manual binary path option.
- **Explicit local provider**: checks for a local model file or a recognized remote/downloadable model URL. If missing, suggests switching to a remote provider.
- **Explicit remote provider** (`openai`, `voyage`, etc.): verifies an API key is present in the environment or auth store. Prints actionable fix hints if missing.
- **Auto provider**: checks local model availability first, then tries each remote provider in auto-selection order.

When a cached gateway probe result is available (gateway was healthy at the time of the check), doctor cross-references its result with the CLI-visible config and notes any discrepancy. Doctor does not start a fresh embedding ping on the default path; use the deep memory status command when you want a live provider check.Use `openclaw memory status --deep` to verify embedding readiness at runtime.

14\. Channel status warnings

If the gateway is healthy, doctor runs a channel status probe and reports warnings with suggested fixes.

15\. Supervisor config audit + repair

Doctor checks the installed supervisor config (launchd/systemd/schtasks) for missing or outdated defaults (e.g., systemd network-online dependencies and restart delay). When it finds a mismatch, it recommends an update and can rewrite the service file/task to the current defaults.Notes:

- `openclaw doctor` prompts before rewriting supervisor config.
- `openclaw doctor --yes` accepts the default repair prompts.
- `openclaw doctor --repair` applies recommended fixes without prompts.
- `openclaw doctor --repair --force` overwrites custom supervisor configs.
- `OPENCLAW_SERVICE_REPAIR_POLICY=external` keeps doctor read-only for gateway service lifecycle. It still reports service health and runs non-service repairs, but skips service install/start/restart/bootstrap, supervisor config rewrites, and legacy service cleanup because an external supervisor owns that lifecycle.
- On Linux, doctor does not rewrite command/entrypoint metadata while the matching systemd gateway unit is active. It also ignores inactive non-legacy extra gateway-like units during the duplicate-service scan so companion service files do not create cleanup noise.
- If token auth requires a token and `gateway.auth.token` is SecretRef-managed, doctor service install/repair validates the SecretRef but does not persist resolved plaintext token values into supervisor service environment metadata.
- Doctor detects managed `.env`/SecretRef-backed service environment values that older LaunchAgent, systemd, or Windows Scheduled Task installs embedded inline and rewrites the service metadata so those values load from the runtime source instead of the supervisor definition.
- Doctor detects when the service command still pins an old `--port` after `gateway.port` changes and rewrites the service metadata to the current port.
- If token auth requires a token and the configured token SecretRef is unresolved, doctor blocks the install/repair path with actionable guidance.
- If both `gateway.auth.token` and `gateway.auth.password` are configured and `gateway.auth.mode` is unset, doctor blocks install/repair until mode is set explicitly.
- For Linux user-systemd units, doctor token drift checks now include both `Environment=` and `EnvironmentFile=` sources when comparing service auth metadata.
- Doctor service repairs refuse to rewrite, stop, or restart a gateway service from an older OpenClaw binary when the config was last written by a newer version. See [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting#split-brain-installs-and-newer-config-guard).
- You can always force a full rewrite via `openclaw gateway install --force`.

16\. Gateway runtime + port diagnostics

Doctor inspects the service runtime (PID, last exit status) and warns when the service is installed but not actually running. It also checks for port collisions on the gateway port (default `18789`) and reports likely causes (gateway already running, SSH tunnel).

17\. Gateway runtime best practices

Doctor warns when the gateway service runs on Bun or a version-managed Node path (`nvm`, `fnm`, `volta`, `asdf`, etc.). WhatsApp + Telegram channels require Node, and version-manager paths can break after upgrades because the service does not load your shell init. Doctor offers to migrate to a system Node install when available (Homebrew/apt/choco).Newly installed or repaired macOS LaunchAgents use a canonical system PATH (`/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin`) instead of copying the interactive shell PATH, so Volta, asdf, fnm, pnpm, and other version-manager directories do not change which Node child processes resolve. Linux services still keep explicit environment roots (`NVM_DIR`, `FNM_DIR`, `VOLTA_HOME`, `ASDF_DATA_DIR`, `BUN_INSTALL`, `PNPM_HOME`) and stable user-bin directories, but guessed version-manager fallback directories are only written to the service PATH when those directories exist on disk.

18\. Config write + wizard metadata

Doctor persists any config changes and stamps wizard metadata to record the doctor run.

19\. Workspace tips (backup + memory system)

Doctor suggests a workspace memory system when missing and prints a backup tip if the workspace is not already under git.See [/concepts/agent-workspace](https://docs.openclaw.ai/concepts/agent-workspace) for a full guide to workspace structure and git backup (recommended private GitHub or GitLab).

## Related

- [Gateway runbook](https://docs.openclaw.ai/gateway)
- [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)

[Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) [Logging](https://docs.openclaw.ai/logging)

Ctrl+I

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
- `gateway.channelMaxRestartsPerHour`: rolling one-hour cap for health-monitor restarts per channel/account. Default: `10`.
- `channels.<provider>.healthMonitor.enabled`: disable health-monitor restarts for a specific channel while leaving global monitoring enabled.
- `channels.<provider>.accounts.<accountId>.healthMonitor.enabled`: multi-account override that wins over the channel-level setting.
- These per-channel overrides apply to the built-in channel monitors that expose them today: Discord, Google Chat, iMessage, Microsoft Teams, Signal, Slack, Telegram, and WhatsApp.

## When something fails

- `logged out` or status 409–515 → relink with `openclaw channels logout` then `openclaw channels login`.
- Gateway unreachable → start it: `openclaw gateway --port 18789` (use `--force` if the port is busy).
- No inbound messages → confirm linked phone is online and the sender is allowed (`channels.whatsapp.allowFrom`); for group chats, ensure allowlist + mention rules match (`channels.whatsapp.groups`, `agents.list[].groupChat.mentionPatterns`).

## Dedicated “health” command

`openclaw health` asks the running gateway for its health snapshot (no direct channel
sockets from the CLI). By default it can return a fresh cached gateway snapshot; the
gateway then refreshes that cache in the background. `openclaw health --verbose` forces
a live probe instead. The command reports linked creds/auth age when available,
per-channel probe summaries, session-store summary, and a probe duration. It exits
non-zero if the gateway is unreachable or the probe fails/timeouts.Options:

- `--json`: machine-readable JSON output
- `--timeout <ms>`: override the default 10s probe timeout
- `--verbose`: force a live probe and print gateway connection details
- `--debug`: alias for `--verbose`

The health snapshot includes: `ok` (boolean), `ts` (timestamp), `durationMs` (probe time), per-channel status, agent availability, and session-store summary.

## Related

- [Gateway runbook](https://docs.openclaw.ai/gateway)
- [Diagnostics export](https://docs.openclaw.ai/gateway/diagnostics)
- [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)

[Trusted proxy auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth) [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat)

Ctrl+I

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

If a heartbeat uses a smaller local model, for example an Ollama model with a 32k window, and the next main-session turn reports context overflow, check whether the previous heartbeat left the session on the heartbeat model. OpenClaw’s reset message calls this out when the last runtime model matches configured `heartbeat.model`.Use `isolatedSession: true` to run heartbeats in a fresh session, combine it with `lightContext: true` for the smallest prompt, or choose a heartbeat model with a context window large enough for the shared session.

## Related

- [Automation & Tasks](https://docs.openclaw.ai/automation) — all automation mechanisms at a glance
- [Background Tasks](https://docs.openclaw.ai/automation/tasks) — how detached work is tracked
- [Timezone](https://docs.openclaw.ai/concepts/timezone) — how timezone affects heartbeat scheduling
- [Troubleshooting](https://docs.openclaw.ai/automation/cron-jobs#troubleshooting) — debugging automation issues

[Health checks](https://docs.openclaw.ai/gateway/health) [Doctor](https://docs.openclaw.ai/gateway/doctor)

Ctrl+I

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

If you want a heartbeat to do something very specific (e.g. "check Gmail PubSub stats" or "verify gateway health"), set \`agents.defaults.heartbeat.prompt\` (or \`agents.list\[\].heartbeat.prompt\`) to a custom body (sent verbatim).

\## Response contract

\\* If nothing needs attention, reply with \*\*\`HEARTBEAT\_OK\`\*\*.
\\* Tool-capable heartbeat runs may instead call \`heartbeat\_respond\` with \`notify: false\` for no visible update, or \`notify: true\` plus \`notificationText\` for an alert. When present, the structured tool response takes precedence over the text fallback.
\\* During heartbeat runs, OpenClaw treats \`HEARTBEAT\_OK\` as an ack when it appears at the \*\*start or end\*\* of the reply. The token is stripped and the reply is dropped if the remaining content is \*\*≤ \`ackMaxChars\`\*\* (default: 300).
\\* If \`HEARTBEAT\_OK\` appears in the \*\*middle\*\* of a reply, it is not treated specially.
\\* For alerts, \*\*do not\*\* include \`HEARTBEAT\_OK\`; return only the alert text.

Outside heartbeats, stray \`HEARTBEAT\_OK\` at the start/end of a message is stripped and logged; a message that is only \`HEARTBEAT\_OK\` is dropped.

\## Config

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 heartbeat: {
 every: "30m", // default: 30m (0m disables)
 model: "anthropic/claude-opus-4-6",
 includeReasoning: false, // default: false (deliver separate Reasoning: message when available)
 lightContext: false, // default: false; true keeps only HEARTBEAT.md from workspace bootstrap files
 isolatedSession: false, // default: false; true runs each heartbeat in a fresh session (no conversation history)
 skipWhenBusy: false, // default: false; true also waits for subagent/nested lanes
 target: "last", // default: none \| options: last \| none \|  (core or plugin, e.g. "bluebubbles")
 to: "+15551234567", // optional channel-specific override
 accountId: "ops-bot", // optional multi-account channel id
 prompt: "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT\_OK.",
 ackMaxChars: 300, // max chars allowed after HEARTBEAT\_OK
 },
 },
 },
}
\`\`\`

\### Scope and precedence

\\* \`agents.defaults.heartbeat\` sets global heartbeat behavior.
\\* \`agents.list\[\].heartbeat\` merges on top; if any agent has a \`heartbeat\` block, \*\*only those agents\*\* run heartbeats.
\\* \`channels.defaults.heartbeat\` sets visibility defaults for all channels.
\\* \`channels..heartbeat\` overrides channel defaults.
\\* \`channels..accounts..heartbeat\` (multi-account channels) overrides per-channel settings.

\### Per-agent heartbeats

If any \`agents.list\[\]\` entry includes a \`heartbeat\` block, \*\*only those agents\*\* run heartbeats. The per-agent block merges on top of \`agents.defaults.heartbeat\` (so you can set shared defaults once and override per agent).

Example: two agents, only the second agent runs heartbeats.

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 heartbeat: {
 every: "30m",
 target: "last", // explicit delivery to last contact (default is "none")
 },
 },
 list: \[\
 { id: "main", default: true },\
 {\
 id: "ops",\
 heartbeat: {\
 every: "1h",\
 target: "whatsapp",\
 to: "+15551234567",\
 timeoutSeconds: 45,\
 prompt: "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT\_OK.",\
 },\
 },\
 \],
 },
}
\`\`\`

\### Active hours example

Restrict heartbeats to business hours in a specific timezone:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 heartbeat: {
 every: "30m",
 target: "last", // explicit delivery to last contact (default is "none")
 activeHours: {
 start: "09:00",
 end: "22:00",
 timezone: "America/New\_York", // optional; uses your userTimezone if set, otherwise host tz
 },
 },
 },
 },
}
\`\`\`

Outside this window (before 9am or after 10pm Eastern), heartbeats are skipped. The next scheduled tick inside the window will run normally.

\### 24/7 setup

If you want heartbeats to run all day, use one of these patterns:

\\* Omit \`activeHours\` entirely (no time-window restriction; this is the default behavior).
\\* Set a full-day window: \`activeHours: { start: "00:00", end: "24:00" }\`.

 Do not set the same \`start\` and \`end\` time (for example \`08:00\` to \`08:00\`). That is treated as a zero-width window, so heartbeats are always skipped.

\### Multi-account example

Use \`accountId\` to target a specific account on multi-account channels like Telegram:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 list: \[\
 {\
 id: "ops",\
 heartbeat: {\
 every: "1h",\
 target: "telegram",\
 to: "12345678:topic:42", // optional: route to a specific topic/thread\
 accountId: "ops-bot",\
 },\
 },\
 \],
 },
 channels: {
 telegram: {
 accounts: {
 "ops-bot": { botToken: "YOUR\_TELEGRAM\_BOT\_TOKEN" },
 },
 },
 },
}
\`\`\`

\### Field notes

 Heartbeat interval (duration string; default unit = minutes).

 Optional model override for heartbeat runs (\`provider/model\`).

 When enabled, also deliver the separate \`Reasoning:\` message when available (same shape as \`/reasoning on\`).

 When true, heartbeat runs use lightweight bootstrap context and keep only \`HEARTBEAT.md\` from workspace bootstrap files.

 When true, each heartbeat runs in a fresh session with no prior conversation history. Uses the same isolation pattern as cron \`sessionTarget: "isolated"\`. Dramatically reduces per-heartbeat token cost. Combine with \`lightContext: true\` for maximum savings. Delivery routing still uses the main session context.

 When true, heartbeat runs defer on extra busy lanes: subagent or nested command work. Cron lanes always defer heartbeats, even without this flag, so local-model hosts do not run cron and heartbeat prompts at the same time.

 Optional session key for heartbeat runs.

 \\* \`main\` (default): agent main session.
 \\* Explicit session key (copy from \`openclaw sessions --json\` or the \[sessions CLI\](/cli/sessions)).
 \\* Session key formats: see \[Sessions\](/concepts/session) and \[Groups\](/channels/groups).

 \\* \`last\`: deliver to the last used external channel.
 \\* explicit channel: any configured channel or plugin id, for example \`discord\`, \`matrix\`, \`telegram\`, or \`whatsapp\`.
 \\* \`none\` (default): run the heartbeat but \*\*do not deliver\*\* externally.

 Controls direct/DM delivery behavior. \`allow\`: allow direct/DM heartbeat delivery. \`block\`: suppress direct/DM delivery (\`reason=dm-blocked\`).

 Optional recipient override (channel-specific id, e.g. E.164 for WhatsApp or a Telegram chat id). For Telegram topics/threads, use \`:topic:\`.

 Optional account id for multi-account channels. When \`target: "last"\`, the account id applies to the resolved last channel if it supports accounts; otherwise it is ignored. If the account id does not match a configured account for the resolved channel, delivery is skipped.

 Overrides the default prompt body (not merged).

 Max chars allowed after \`HEARTBEAT\_OK\` before delivery.

 When true, suppresses tool error warning payloads during heartbeat runs.

 Restricts heartbeat runs to a time window. Object with \`start\` (HH:MM, inclusive; use \`00:00\` for start-of-day), \`end\` (HH:MM exclusive; \`24:00\` allowed for end-of-day), and optional \`timezone\`.

 \\* Omitted or \`"user"\`: uses your \`agents.defaults.userTimezone\` if set, otherwise falls back to the host system timezone.
 \\* \`"local"\`: always uses the host system timezone.
 \\* Any IANA identifier (e.g. \`America/New\_York\`): used directly; if invalid, falls back to the \`"user"\` behavior above.
 \\* \`start\` and \`end\` must not be equal for an active window; equal values are treated as zero-width (always outside the window).
 \\* Outside the active window, heartbeats are skipped until the next tick inside the window.

\## Delivery behavior

 \\* Heartbeats run in the agent's main session by default (\`agent::\`), or \`global\` when \`session.scope = "global"\`. Set \`session\` to override to a specific channel session (Discord/WhatsApp/etc.).
 \\* \`session\` only affects the run context; delivery is controlled by \`target\` and \`to\`.
 \\* To deliver to a specific channel/recipient, set \`target\` + \`to\`. With \`target: "last"\`, delivery uses the last external channel for that session.
 \\* Heartbeat deliveries allow direct/DM targets by default. Set \`directPolicy: "block"\` to suppress direct-target sends while still running the heartbeat turn.
 \\* If the main queue, target session lane, cron lane, or an active cron job is busy, the heartbeat is skipped and retried later.
 \\* If \`skipWhenBusy: true\`, subagent and nested lanes also defer heartbeat runs.
 \\* If \`target\` resolves to no external destination, the run still happens but no outbound message is sent.

 \\* If \`showOk\`, \`showAlerts\`, and \`useIndicator\` are all disabled, the run is skipped up front as \`reason=alerts-disabled\`.
 \\* If only alert delivery is disabled, OpenClaw can still run the heartbeat, update due-task timestamps, restore the session idle timestamp, and suppress the outward alert payload.
 \\* If the resolved heartbeat target supports typing, OpenClaw shows typing while the heartbeat run is active. This uses the same target the heartbeat would send chat output to, and it is disabled by \`typingMode: "never"\`.

 \\* Heartbeat-only replies do \*\*not\*\* keep the session alive. Heartbeat metadata may update the session row, but idle expiry uses \`lastInteractionAt\` from the last real user/channel message, and daily expiry uses \`sessionStartedAt\`.
 \\* Control UI and WebChat history hide heartbeat prompts and OK-only acknowledgments. The underlying session transcript can still contain those turns for audit/replay.
 \\* Detached \[background tasks\](/automation/tasks) can enqueue a system event and wake heartbeat when the main session should notice something quickly. That wake does not make the heartbeat run a background task.

\## Visibility controls

By default, \`HEARTBEAT\_OK\` acknowledgments are suppressed while alert content is delivered. You can adjust this per channel or per account:

\`\`\`yaml theme={"theme":{"light":"min-light","dark":"min-dark"}}
channels:
 defaults:
 heartbeat:
 showOk: false # Hide HEARTBEAT\_OK (default)
 showAlerts: true # Show alert messages (default)
 useIndicator: true # Emit indicator events (default)
 telegram:
 heartbeat:
 showOk: true # Show OK acknowledgments on Telegram
 whatsapp:
 accounts:
 work:
 heartbeat:
 showAlerts: false # Suppress alert delivery for this account
\`\`\`

Precedence: per-account → per-channel → channel defaults → built-in defaults.

\### What each flag does

\\* \`showOk\`: sends a \`HEARTBEAT\_OK\` acknowledgment when the model returns an OK-only reply.
\\* \`showAlerts\`: sends the alert content when the model returns a non-OK reply.
\\* \`useIndicator\`: emits indicator events for UI status surfaces.

If \*\*all three\*\* are false, OpenClaw skips the heartbeat run entirely (no model call).

\### Per-channel vs per-account examples

\`\`\`yaml theme={"theme":{"light":"min-light","dark":"min-dark"}}
channels:
 defaults:
 heartbeat:
 showOk: false
 showAlerts: true
 useIndicator: true
 slack:
 heartbeat:
 showOk: true # all Slack accounts
 accounts:
 ops:
 heartbeat:
 showAlerts: false # suppress alerts for the ops account only
 telegram:
 heartbeat:
 showOk: true
\`\`\`

\### Common patterns

\| Goal \| Config \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| Default behavior (silent OKs, alerts on) \| \*(no config needed)\* \|
\| Fully silent (no messages, no indicator) \| \`channels.defaults.heartbeat: { showOk: false, showAlerts: false, useIndicator: false }\` \|
\| Indicator-only (no messages) \| \`channels.defaults.heartbeat: { showOk: false, showAlerts: false, useIndicator: true }\` \|
\| OKs in one channel only \| \`channels.telegram.heartbeat: { showOk: true }\` \|

\## HEARTBEAT.md (optional)

If a \`HEARTBEAT.md\` file exists in the workspace, the default prompt tells the agent to read it. Think of it as your "heartbeat checklist": small, stable, and safe to include every 30 minutes.

On normal runs, \`HEARTBEAT.md\` is only injected when heartbeat guidance is enabled for the default agent. Disabling the heartbeat cadence with \`0m\` or setting \`includeSystemPromptSection: false\` omits it from normal bootstrap context.

If \`HEARTBEAT.md\` exists but is effectively empty (only blank lines and markdown headers like \`# Heading\`), OpenClaw skips the heartbeat run to save API calls. That skip is reported as \`reason=empty-heartbeat-file\`. If the file is missing, the heartbeat still runs and the model decides what to do.

Keep it tiny (short checklist or reminders) to avoid prompt bloat.

Example \`HEARTBEAT.md\`:

\`\`\`md theme={"theme":{"light":"min-light","dark":"min-dark"}}
\# Heartbeat checklist

\- Quick scan: anything urgent in inboxes?
\- If it's daytime, do a lightweight check-in if nothing else is pending.
\- If a task is blocked, write down \_what is missing\_ and ask Peter next time.
\`\`\`

\### \`tasks:\` blocks

\`HEARTBEAT.md\` also supports a small structured \`tasks:\` block for interval-based checks inside heartbeat itself.

Example:

\`\`\`md theme={"theme":{"light":"min-light","dark":"min-dark"}}
tasks:

\- name: inbox-triage
 interval: 30m
 prompt: "Check for urgent unread emails and flag anything time sensitive."
\- name: calendar-scan
 interval: 2h
 prompt: "Check for upcoming meetings that need prep or follow-up."

\# Additional instructions

\- Keep alerts short.
\- If nothing needs attention after all due tasks, reply HEARTBEAT\_OK.
\`\`\`

 \\* OpenClaw parses the \`tasks:\` block and checks each task against its own \`interval\`.
 \\* Only \*\*due\*\* tasks are included in the heartbeat prompt for that tick.
 \\* If no tasks are due, the heartbeat is skipped entirely (\`reason=no-tasks-due\`) to avoid a wasted model call.
 \\* Non-task content in \`HEARTBEAT.md\` is preserved and appended as additional context after the due-task list.
 \\* Task last-run timestamps are stored in session state (\`heartbeatTaskState\`), so intervals survive normal restarts.
 \\* Task timestamps are only advanced after a heartbeat run completes its normal reply path. Skipped \`empty-heartbeat-file\` / \`no-tasks-due\` runs do not mark tasks as completed.

Task mode is useful when you want one heartbeat file to hold several periodic checks without paying for all of them every tick.

\### Can the agent update HEARTBEAT.md?

Yes — if you ask it to.

\`HEARTBEAT.md\` is just a normal file in the agent workspace, so you can tell the agent (in a normal chat) something like:

\\* "Update \`HEARTBEAT.md\` to add a daily calendar check."
\\* "Rewrite \`HEARTBEAT.md\` so it's shorter and focused on inbox follow-ups."

If you want this to happen proactively, you can also include an explicit line in your heartbeat prompt like: "If the checklist becomes stale, update HEARTBEAT.md with a better one."

 Don't put secrets (API keys, phone numbers, private tokens) into \`HEARTBEAT.md\` — it becomes part of the prompt context.

\## Manual wake (on-demand)

You can enqueue a system event and trigger an immediate heartbeat with:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw system event --text "Check for urgent follow-ups" --mode now
\`\`\`

If multiple agents have \`heartbeat\` configured, a manual wake runs each of those agent heartbeats immediately.

Use \`--mode next-heartbeat\` to wait for the next scheduled tick.

\## Reasoning delivery (optional)

By default, heartbeats deliver only the final "answer" payload.

If you want transparency, enable:

\\* \`agents.defaults.heartbeat.includeReasoning: true\`

When enabled, heartbeats will also deliver a separate message prefixed \`Reasoning:\` (same shape as \`/reasoning on\`). This can be useful when the agent is managing multiple sessions/codexes and you want to see why it decided to ping you — but it can also leak more internal detail than you want. Prefer keeping it off in group chats.

\## Cost awareness

Heartbeats run full agent turns. Shorter intervals burn more tokens. To reduce cost:

\\* Use \`isolatedSession: true\` to avoid sending full conversation history (\\~100K tokens down to \\~2-5K per run).
\\* Use \`lightContext: true\` to limit bootstrap files to just \`HEARTBEAT.md\`.
\\* Set a cheaper \`model\` (e.g. \`ollama/llama3.2:1b\`).
\\* Keep \`HEARTBEAT.md\` small.
\\* Use \`target: "none"\` if you only want internal state updates.

\## Context overflow after heartbeat

If a heartbeat uses a smaller local model, for example an Ollama model with a 32k window, and the next main-session turn reports context overflow, check whether the previous heartbeat left the session on the heartbeat model. OpenClaw's reset message calls this out when the last runtime model matches configured \`heartbeat.model\`.

Use \`isolatedSession: true\` to run heartbeats in a fresh session, combine it with \`lightContext: true\` for the smallest prompt, or choose a heartbeat model with a context window large enough for the shared session.

\## Related

\\* \[Automation & Tasks\](/automation) — all automation mechanisms at a glance
\\* \[Background Tasks\](/automation/tasks) — how detached work is tracked
\\* \[Timezone\](/concepts/timezone) — how timezone affects heartbeat scheduling
\\* \[Troubleshooting\](/automation/cron-jobs#troubleshooting) — debugging automation issues

---

## Local models - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/local-models>_

[OpenClaw home page](https://docs.openclaw.ai/)

Protocols and APIs

Local models

Local is doable, but OpenClaw expects large context + strong defenses against prompt injection. Small cards truncate context and leak safety. Aim high: **≥2 maxed-out Mac Studios or equivalent GPU rig (~$30k+)**. A single **24 GB** GPU works only for lighter prompts with higher latency. Use the **largest / full-size model variant you can run**; aggressively quantized or “small” checkpoints raise prompt-injection risk (see [Security](https://docs.openclaw.ai/gateway/security)).If you want the lowest-friction local setup, start with [LM Studio](https://docs.openclaw.ai/providers/lmstudio) or [Ollama](https://docs.openclaw.ai/providers/ollama) and `openclaw onboard`. This page is the opinionated guide for higher-end local stacks and custom OpenAI-compatible local servers.

**WSL2 + Ollama + NVIDIA/CUDA users:** The official Ollama Linux installer enables a systemd service with `Restart=always`. On WSL2 GPU setups, autostart can reload the last model during boot and pin host memory. If your WSL2 VM repeatedly restarts after enabling Ollama, see [WSL2 crash loop](https://docs.openclaw.ai/providers/ollama#wsl2-crash-loop-repeated-reboots).

## Recommended: LM Studio + large local model (Responses API)

Best current local stack. Load a large model in LM Studio (for example, a full-size Qwen, DeepSeek, or Llama build), enable the local server (default `http://127.0.0.1:1234`), and use Responses API to keep reasoning separate from final text.

```
{
  agents: {
    defaults: {
      model: { primary: "lmstudio/my-local-model" },
      models: {
        "anthropic/claude-opus-4-6": { alias: "Opus" },
        "lmstudio/my-local-model": { alias: "Local" },
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      lmstudio: {
        baseUrl: "http://127.0.0.1:1234/v1",
        apiKey: "lmstudio",
        api: "openai-responses",
        models: [\
          {\
            id: "my-local-model",\
            name: "Local Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 196608,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

**Setup checklist**

- Install LM Studio: [https://lmstudio.ai](https://lmstudio.ai/)
- In LM Studio, download the **largest model build available** (avoid “small”/heavily quantized variants), start the server, confirm `http://127.0.0.1:1234/v1/models` lists it.
- Replace `my-local-model` with the actual model ID shown in LM Studio.
- Keep the model loaded; cold-load adds startup latency.
- Adjust `contextWindow`/`maxTokens` if your LM Studio build differs.
- For WhatsApp, stick to Responses API so only final text is sent.

Keep hosted models configured even when running local; use `models.mode: "merge"` so fallbacks stay available.

### Hybrid config: hosted primary, local fallback

```
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-6",
        fallbacks: ["lmstudio/my-local-model", "anthropic/claude-opus-4-6"],
      },
      models: {
        "anthropic/claude-sonnet-4-6": { alias: "Sonnet" },
        "lmstudio/my-local-model": { alias: "Local" },
        "anthropic/claude-opus-4-6": { alias: "Opus" },
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      lmstudio: {
        baseUrl: "http://127.0.0.1:1234/v1",
        apiKey: "lmstudio",
        api: "openai-responses",
        models: [\
          {\
            id: "my-local-model",\
            name: "Local Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 196608,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

### Local-first with hosted safety net

Swap the primary and fallback order; keep the same providers block and `models.mode: "merge"` so you can fall back to Sonnet or Opus when the local box is down.

### Regional hosting / data routing

- Hosted MiniMax/Kimi/GLM variants also exist on OpenRouter with region-pinned endpoints (e.g., US-hosted). Pick the regional variant there to keep traffic in your chosen jurisdiction while still using `models.mode: "merge"` for Anthropic/OpenAI fallbacks.
- Local-only remains the strongest privacy path; hosted regional routing is the middle ground when you need provider features but want control over data flow.

## Other OpenAI-compatible local proxies

MLX (`mlx_lm.server`), vLLM, SGLang, LiteLLM, OAI-proxy, or custom
gateways work if they expose an OpenAI-style `/v1/chat/completions`
endpoint. Use the Chat Completions adapter unless the backend explicitly
documents `/v1/responses` support. Replace the provider block above with your
endpoint and model ID:

```
{
  agents: {
    defaults: {
      model: { primary: "local/my-local-model" },
    },
  },
  models: {
    mode: "merge",
    providers: {
      local: {
        baseUrl: "http://127.0.0.1:8000/v1",
        apiKey: "sk-local",
        api: "openai-completions",
        timeoutSeconds: 300,
        models: [\
          {\
            id: "my-local-model",\
            name: "Local Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 120000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

If `api` is omitted on a custom provider with a `baseUrl`, OpenClaw defaults to
`openai-completions`. Loopback endpoints such as `127.0.0.1` are trusted
automatically; LAN, tailnet, and private DNS endpoints still need
`request.allowPrivateNetwork: true`.The `models.providers.<id>.models[].id` value is provider-local. Do not
include the provider prefix there. For example, an MLX server started with
`mlx_lm.server --model mlx-community/Qwen3-30B-A3B-6bit` should use this
catalog id and model ref:

- `models.providers.mlx.models[].id: "mlx-community/Qwen3-30B-A3B-6bit"`
- `agents.defaults.model.primary: "mlx/mlx-community/Qwen3-30B-A3B-6bit"`

Set `input: ["text", "image"]` on local or proxied vision models so image
attachments are injected into agent turns. Interactive custom-provider
onboarding infers common vision model IDs and asks only for unknown names.
Non-interactive onboarding uses the same inference; use `--custom-image-input`
for unknown vision IDs or `--custom-text-input` when a known-looking model is
text-only behind your endpoint.Keep `models.mode: "merge"` so hosted models stay available as fallbacks.
Use `models.providers.<id>.timeoutSeconds` for slow local or remote model
servers before raising `agents.defaults.timeoutSeconds`. The provider timeout
applies only to model HTTP requests, including connect, headers, body streaming,
and the total guarded-fetch abort.

For custom OpenAI-compatible providers, persisting a non-secret local marker such as `apiKey: "ollama-local"` is accepted when `baseUrl` resolves to loopback, a private LAN, `.local`, or a bare hostname. OpenClaw treats it as a valid local credential instead of reporting a missing key. Use a real value for any provider that accepts a public hostname.

Behavior note for local/proxied `/v1` backends:

- OpenClaw treats these as proxy-style OpenAI-compatible routes, not native
OpenAI endpoints
- native OpenAI-only request shaping does not apply here: no
`service_tier`, no Responses `store`, no OpenAI reasoning-compat payload
shaping, and no prompt-cache hints
- hidden OpenClaw attribution headers (`originator`, `version`, `User-Agent`)
are not injected on these custom proxy URLs

Compatibility notes for stricter OpenAI-compatible backends:

- Some servers accept only string `messages[].content` on Chat Completions, not
structured content-part arrays. Set
`models.providers.<provider>.models[].compat.requiresStringContent: true` for
those endpoints.
- Some local models emit standalone bracketed tool requests as text, such as
`[tool_name]` followed by JSON and `[END_TOOL_REQUEST]`. OpenClaw promotes
those into real tool calls only when the name exactly matches a registered
tool for the turn; otherwise the block is treated as unsupported text and is
hidden from user-visible replies.
- If a model emits JSON, XML, or ReAct-style text that looks like a tool call
but the provider did not emit a structured invocation, OpenClaw leaves it as
text and logs a warning with the run id, provider/model, detected pattern, and
tool name when available. Treat that as provider/model tool-call
incompatibility, not a completed tool run.
- If tools appear as assistant text instead of running, for example raw JSON,
XML, ReAct syntax, or an empty `tool_calls` array in the provider response,
first verify the server is using a tool-call-capable chat template/parser. For
OpenAI-compatible Chat Completions backends whose parser works only when tool
use is forced, set a per-model request override instead of relying on text
parsing:

```
{
    agents: {
      defaults: {
        models: {
          "local/my-local-model": {
            params: {
              extra_body: {
                tool_choice: "required",
              },
            },
          },
        },
      },
    },
}
```

Use this only for models/sessions where every normal turn should call a tool.
It overrides OpenClaw’s default proxy value of `tool_choice: "auto"`.
Replace `local/my-local-model` with the exact provider/model ref shown by
`openclaw models list`.

```
openclaw config set agents.defaults.models '{"local/my-local-model":{"params":{"extra_body":{"tool_choice":"required"}}}}' --strict-json --merge
```

- If a custom OpenAI-compatible model accepts OpenAI reasoning efforts beyond
the built-in profile, declare them on the model compat block. Adding `"xhigh"`
here makes `/think xhigh`, session pickers, Gateway validation, and `llm-task`
validation expose the level for that configured provider/model ref:

```
{
    models: {
      providers: {
        local: {
          baseUrl: "http://127.0.0.1:8000/v1",
          apiKey: "sk-local",
          api: "openai-responses",
          models: [\
            {\
              id: "gpt-5.4",\
              name: "GPT 5.4 via local proxy",\
              reasoning: true,\
              input: ["text"],\
              cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
              contextWindow: 196608,\
              maxTokens: 8192,\
              compat: {\
                supportedReasoningEfforts: ["low", "medium", "high", "xhigh"],\
                reasoningEffortMap: { xhigh: "xhigh" },\
              },\
            },\
          ],
        },
      },
    },
}
```

- Some smaller or stricter local backends are unstable with OpenClaw’s full
agent-runtime prompt shape, especially when tool schemas are included. First
verify the provider path with the lean local probe:

```
openclaw infer model run --local --model <provider/model> --prompt "Reply with exactly: pong" --json
```

To verify the Gateway route without the full agent prompt shape, use the
Gateway model probe instead:

```
openclaw infer model run --gateway --model <provider/model> --prompt "Reply with exactly: pong" --json
```

Both local and Gateway model probes send only the supplied prompt. The
Gateway probe still validates Gateway routing, auth, and provider selection,
but it intentionally skips prior session transcript, AGENTS/bootstrap context,
context-engine assembly, tools, and bundled MCP servers.If that succeeds but normal OpenClaw agent turns fail, first try
`agents.defaults.experimental.localModelLean: true` to drop heavyweight
default tools like `browser`, `cron`, and `message`; this is an experimental
flag, not a stable default-mode setting. See
[Experimental Features](https://docs.openclaw.ai/concepts/experimental-features). If that still fails, try
`models.providers.<provider>.models[].compat.supportsTools: false`.
- If the backend still fails only on larger OpenClaw runs, the remaining issue
is usually upstream model/server capacity or a backend bug, not OpenClaw’s
transport layer.

## Troubleshooting

- Gateway can reach the proxy? `curl http://127.0.0.1:1234/v1/models`.
- LM Studio model unloaded? Reload; cold start is a common “hanging” cause.
- Local server says `terminated`, `ECONNRESET`, or closes the stream mid-turn?
OpenClaw records a low-cardinality `model.call.error.failureKind` plus the
OpenClaw process RSS/heap snapshot in diagnostics. For LM Studio/Ollama
memory pressure, match that timestamp against the server log or macOS crash /
jetsam log to confirm whether the model server was killed.
- OpenClaw derives context-window preflight thresholds from the detected model window, or from the uncapped model window when `agents.defaults.contextTokens` lowers the effective window. It warns below 20% with an **8k** floor. Hard blocks use the 10% threshold with a **4k** floor, capped to the effective context window so oversized model metadata cannot reject an otherwise valid user cap. If you hit that preflight, raise the server/model context limit or choose a larger model.
- Context errors? Lower `contextWindow` or raise your server limit.
- OpenAI-compatible server returns `messages[].content ... expected a string`?
Add `compat.requiresStringContent: true` on that model entry.
- Direct tiny `/v1/chat/completions` calls work, but `openclaw infer model run --local`
fails on Gemma or another local model? Check the provider URL, model ref, auth
marker, and server logs first; local `model run` does not include agent tools.
If local `model run` succeeds but larger agent turns fail, reduce the agent
tool surface with `localModelLean` or `compat.supportsTools: false`.
- Tool calls show up as raw JSON/XML/ReAct text, or the provider returns an
empty `tool_calls` array? Do not add a proxy that blindly converts assistant
text into tool execution. Fix the server chat template/parser first. If the
model only works when tool use is forced, add the per-model
`params.extra_body.tool_choice: "required"` override above and use that model
entry only for sessions where a tool call is expected on every turn.
- Safety: local models skip provider-side filters; keep agents narrow and compaction on to limit prompt injection blast radius.

## Related

- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference)
- [Model failover](https://docs.openclaw.ai/concepts/model-failover)

[CLI backends](https://docs.openclaw.ai/gateway/cli-backends) [Network](https://docs.openclaw.ai/network)

Ctrl+I

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
Subsystem loggers keep output grouped and scannable.Behavior:

- **Subsystem prefixes** on every line (e.g. `[gateway]`, `[canvas]`, `[tailscale]`)
- **Subsystem colors** (stable per subsystem) plus level coloring
- **Color when output is a TTY or the environment looks like a rich terminal** (`TERM`/`COLORTERM`/`TERM_PROGRAM`), respects `NO_COLOR`
- **Shortened subsystem prefixes**: drops leading `gateway/` \+ `channels/`, keeps last 2 segments (e.g. `whatsapp/outbound`)
- **Sub-loggers by subsystem** (auto prefix + structured field `{ subsystem }`)
- **`logRaw()`** for QR/UX output (no prefix, no formatting)
- **Console styles** (e.g. `pretty | compact | json`)
- **Console log level** separate from file log level (file keeps full detail when `logging.level` is set to `debug`/`trace`)
- **WhatsApp message bodies** are logged at `debug` (use `--verbose` to see them)

This keeps existing file logs stable while making interactive output scannable.

## Related

- [Logging](https://docs.openclaw.ai/logging)
- [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry)
- [Diagnostics export](https://docs.openclaw.ai/gateway/diagnostics)

[Prometheus](https://docs.openclaw.ai/gateway/prometheus) [Diagnostics export](https://docs.openclaw.ai/gateway/diagnostics)

Ctrl+I

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
- `model: "openclaw/default"` also routes to the configured default agent.
- `model: "openclaw/<agentId>"` routes to a specific agent.

Optional request headers:

- `x-openclaw-model: <provider/model-or-bare-id>` overrides the backend model for the selected agent.
- `x-openclaw-agent-id: <agentId>` remains supported as a compatibility override.
- `x-openclaw-session-key: <sessionKey>` fully controls session routing.
- `x-openclaw-message-channel: <channel>` sets the synthetic ingress channel context for channel-aware prompts and policies.

Compatibility aliases still accepted:

- `model: "openclaw:<agentId>"`
- `model: "agent:<agentId>"`

## Enabling the endpoint

Set `gateway.http.endpoints.chatCompletions.enabled` to `true`:

```
{
  gateway: {
    http: {
      endpoints: {
        chatCompletions: { enabled: true },
      },
    },
  },
}
```

## Disabling the endpoint

Set `gateway.http.endpoints.chatCompletions.enabled` to `false`:

```
{
  gateway: {
    http: {
      endpoints: {
        chatCompletions: { enabled: false },
      },
    },
  },
}
```

## Session behavior

By default the endpoint is **stateless per request** (a new session key is generated each call).If the request includes an OpenAI `user` string, the Gateway derives a stable session key from it, so repeated calls can share an agent session.

## Why this surface matters

This is the highest-leverage compatibility set for self-hosted frontends and tooling:

- Most Open WebUI, LobeChat, and LibreChat setups expect `/v1/models`.
- Many RAG systems expect `/v1/embeddings`.
- Existing OpenAI chat clients can usually start with `/v1/chat/completions`.
- More agent-native clients increasingly prefer `/v1/responses`.

## Model list and agent routing

What does \`/v1/models\` return?

An OpenClaw agent-target list.The returned ids are `openclaw`, `openclaw/default`, and `openclaw/<agentId>` entries.
Use them directly as OpenAI `model` values.

Does \`/v1/models\` list agents or sub-agents?

It lists top-level agent targets, not backend provider models and not sub-agents.Sub-agents remain internal execution topology. They do not appear as pseudo-models.

Why is \`openclaw/default\` included?

`openclaw/default` is the stable alias for the configured default agent.That means clients can keep using one predictable id even if the real default agent id changes between environments.

How do I override the backend model?

Use `x-openclaw-model`.Examples:
`x-openclaw-model: openai/gpt-5.4``x-openclaw-model: gpt-5.5`If you omit it, the selected agent runs with its normal configured model choice.

How do embeddings fit this contract?

`/v1/embeddings` uses the same agent-target `model` ids.Use `model: "openclaw/default"` or `model: "openclaw/<agentId>"`.
When you need a specific embedding model, send it in `x-openclaw-model`.
Without that header, the request passes through to the selected agent’s normal embedding setup.

## Streaming (SSE)

Set `stream: true` to receive Server-Sent Events (SSE):

- `Content-Type: text/event-stream`
- Each event line is `data: <json>`
- Stream ends with `data: [DONE]`

## Open WebUI quick setup

For a basic Open WebUI connection:

- Base URL: `http://127.0.0.1:18789/v1`
- Docker on macOS base URL: `http://host.docker.internal:18789/v1`
- API key: your Gateway bearer token
- Model: `openclaw/default`

Expected behavior:

- `GET /v1/models` should list `openclaw/default`
- Open WebUI should use `openclaw/default` as the chat model id
- If you want a specific backend provider/model for that agent, set the agent’s normal default model or send `x-openclaw-model`

Quick smoke:

```
curl -sS http://127.0.0.1:18789/v1/models \
  -H 'Authorization: Bearer YOUR_TOKEN'
```

If that returns `openclaw/default`, most Open WebUI setups can connect with the same base URL and token.

## Examples

Non-streaming:

```
curl -sS http://127.0.0.1:18789/v1/chat/completions \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "openclaw/default",
    "messages": [{"role":"user","content":"hi"}]
  }'
```

Streaming:

```
curl -N http://127.0.0.1:18789/v1/chat/completions \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-openclaw-model: openai/gpt-5.4' \
  -d '{
    "model": "openclaw/research",
    "stream": true,
    "messages": [{"role":"user","content":"hi"}]
  }'
```

List models:

```
curl -sS http://127.0.0.1:18789/v1/models \
  -H 'Authorization: Bearer YOUR_TOKEN'
```

Fetch one model:

```
curl -sS http://127.0.0.1:18789/v1/models/openclaw%2Fdefault \
  -H 'Authorization: Bearer YOUR_TOKEN'
```

Create embeddings:

```
curl -sS http://127.0.0.1:18789/v1/embeddings \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-openclaw-model: openai/text-embedding-3-small' \
  -d '{
    "model": "openclaw/default",
    "input": ["alpha", "beta"]
  }'
```

Notes:

- `/v1/models` returns OpenClaw agent targets, not raw provider catalogs.
- `openclaw/default` is always present so one stable id works across environments.
- Backend provider/model overrides belong in `x-openclaw-model`, not the OpenAI `model` field.
- `/v1/embeddings` supports `input` as a string or array of strings.

## Related

- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference)
- [OpenAI](https://docs.openclaw.ai/providers/openai)

[Bridge protocol](https://docs.openclaw.ai/gateway/bridge-protocol) [OpenResponses API](https://docs.openclaw.ai/gateway/openresponses-http-api)

Ctrl+I

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
- `max_output_tokens`: best-effort output limit (provider dependent).
- `user`: stable session routing.

Accepted but **currently ignored**:

- `max_tool_calls`
- `reasoning`
- `metadata`
- `store`
- `truncation`

Supported:

- `previous_response_id`: OpenClaw reuses the earlier response session when the request stays within the same agent/user/requested-session scope.

## Items (input)

### `message`

Roles: `system`, `developer`, `user`, `assistant`.

- `system` and `developer` are appended to the system prompt.
- The most recent `user` or `function_call_output` item becomes the “current message.”
- Earlier user/assistant messages are included as history for context.

### `function_call_output` (turn-based tools)

Send tool results back to the model:

```
{
  "type": "function_call_output",
  "call_id": "call_123",
  "output": "{\"temperature\": \"72F\"}"
}
```

### `reasoning` and `item_reference`

Accepted for schema compatibility but ignored when building the prompt.

## Tools (client-side function tools)

Provide tools with `tools: [{ type: "function", function: { name, description?, parameters? } }]`.If the agent decides to call a tool, the response returns a `function_call` output item.
You then send a follow-up request with `function_call_output` to continue the turn.

## Images (`input_image`)

Supports base64 or URL sources:

```
{
  "type": "input_image",
  "source": { "type": "url", "url": "https://example.com/image.png" }
}
```

Allowed MIME types (current): `image/jpeg`, `image/png`, `image/gif`, `image/webp`, `image/heic`, `image/heif`.
Max size (current): 10MB.

## Files (`input_file`)

Supports base64 or URL sources:

```
{
  "type": "input_file",
  "source": {
    "type": "base64",
    "media_type": "text/plain",
    "data": "SGVsbG8gV29ybGQh",
    "filename": "hello.txt"
  }
}
```

Allowed MIME types (current): `text/plain`, `text/markdown`, `text/html`, `text/csv`,
`application/json`, `application/pdf`.Max size (current): 5MB.Current behavior:

- File content is decoded and added to the **system prompt**, not the user message,
so it stays ephemeral (not persisted in session history).
- Decoded file text is wrapped as **untrusted external content** before it is added,
so file bytes are treated as data, not trusted instructions.
- The injected block uses explicit boundary markers like
`<<<EXTERNAL_UNTRUSTED_CONTENT id="...">>>` /
`<<<END_EXTERNAL_UNTRUSTED_CONTENT id="...">>>` and includes a
`Source: External` metadata line.
- This file-input path intentionally omits the long `SECURITY NOTICE:` banner to
preserve prompt budget; the boundary markers and metadata still stay in place.
- PDFs are parsed for text first. If little text is found, the first pages are
rasterized into images and passed to the model, and the injected file block uses
the placeholder `[PDF content rendered to images]`.

PDF parsing is provided by the bundled `document-extract` plugin, which uses the
Node-friendly `pdfjs-dist` legacy build (no worker). The modern PDF.js build
expects browser workers/DOM globals, so it is not used in the Gateway.URL fetch defaults:

- `files.allowUrl`: `true`
- `images.allowUrl`: `true`
- `maxUrlParts`: `8` (total URL-based `input_file` \+ `input_image` parts per request)
- Requests are guarded (DNS resolution, private IP blocking, redirect caps, timeouts).
- Optional hostname allowlists are supported per input type (`files.urlAllowlist`, `images.urlAllowlist`).

  - Exact host: `"cdn.example.com"`
  - Wildcard subdomains: `"*.assets.example.com"` (does not match apex)
  - Empty or omitted allowlists mean no hostname allowlist restriction.
- To disable URL-based fetches entirely, set `files.allowUrl: false` and/or `images.allowUrl: false`.

## File + image limits (config)

Defaults can be tuned under `gateway.http.endpoints.responses`:

```
{
  gateway: {
    http: {
      endpoints: {
        responses: {
          enabled: true,
          maxBodyBytes: 20000000,
          maxUrlParts: 8,
          files: {
            allowUrl: true,
            urlAllowlist: ["cdn.example.com", "*.assets.example.com"],
            allowedMimes: [\
              "text/plain",\
              "text/markdown",\
              "text/html",\
              "text/csv",\
              "application/json",\
              "application/pdf",\
            ],
            maxBytes: 5242880,
            maxChars: 200000,
            maxRedirects: 3,
            timeoutMs: 10000,
            pdf: {
              maxPages: 4,
              maxPixels: 4000000,
              minTextChars: 200,
            },
          },
          images: {
            allowUrl: true,
            urlAllowlist: ["images.example.com"],
            allowedMimes: [\
              "image/jpeg",\
              "image/png",\
              "image/gif",\
              "image/webp",\
              "image/heic",\
              "image/heif",\
            ],
            maxBytes: 10485760,
            maxRedirects: 3,
            timeoutMs: 10000,
          },
        },
      },
    },
  },
}
```

Defaults when omitted:

- `maxBodyBytes`: 20MB
- `maxUrlParts`: 8
- `files.maxBytes`: 5MB
- `files.maxChars`: 200k
- `files.maxRedirects`: 3
- `files.timeoutMs`: 10s
- `files.pdf.maxPages`: 4
- `files.pdf.maxPixels`: 4,000,000
- `files.pdf.minTextChars`: 200
- `images.maxBytes`: 10MB
- `images.maxRedirects`: 3
- `images.timeoutMs`: 10s
- HEIC/HEIF `input_image` sources are accepted and normalized to JPEG before provider delivery.

Security note:

- URL allowlists are enforced before fetch and on redirect hops.
- Allowlisting a hostname does not bypass private/internal IP blocking.
- For internet-exposed gateways, apply network egress controls in addition to app-level guards.
See [Security](https://docs.openclaw.ai/gateway/security).

## Streaming (SSE)

Set `stream: true` to receive Server-Sent Events (SSE):

- `Content-Type: text/event-stream`
- Each event line is `event: <type>` and `data: <json>`
- Stream ends with `data: [DONE]`

Event types currently emitted:

- `response.created`
- `response.in_progress`
- `response.output_item.added`
- `response.content_part.added`
- `response.output_text.delta`
- `response.output_text.done`
- `response.content_part.done`
- `response.output_item.done`
- `response.completed`
- `response.failed` (on error)

## Usage

`usage` is populated when the underlying provider reports token counts.
OpenClaw normalizes common OpenAI-style aliases before those counters reach
downstream status/session surfaces, including `input_tokens` / `output_tokens`
and `prompt_tokens` / `completion_tokens`.

## Errors

Errors use a JSON object like:

```
{ "error": { "message": "...", "type": "invalid_request_error" } }
```

Common cases:

- `401` missing/invalid auth
- `400` invalid request body
- `405` wrong method

## Examples

Non-streaming:

```
curl -sS http://127.0.0.1:18789/v1/responses \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-openclaw-agent-id: main' \
  -d '{
    "model": "openclaw",
    "input": "hi"
  }'
```

Streaming:

```
curl -N http://127.0.0.1:18789/v1/responses \
  -H 'Authorization: Bearer YOUR_TOKEN' \
  -H 'Content-Type: application/json' \
  -H 'x-openclaw-agent-id: main' \
  -d '{
    "model": "openclaw",
    "stream": true,
    "input": "hi"
  }'
```

## Related

- [OpenAI chat completions](https://docs.openclaw.ai/gateway/openai-http-api)
- [OpenAI](https://docs.openclaw.ai/providers/openai)

[OpenAI chat completions](https://docs.openclaw.ai/gateway/openai-http-api) [Tools invoke API](https://docs.openclaw.ai/gateway/tools-invoke-http-api)

Ctrl+I

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
| `OTEL_SEMCONV_STABILITY_OPT_IN` | Set to `gen_ai_latest_experimental` to emit the latest experimental GenAI span attribute (`gen_ai.provider.name`) instead of the legacy `gen_ai.system`. GenAI metrics always use bounded, low-cardinality semantic attributes regardless. |
| `OPENCLAW_OTEL_PRELOADED` | Set to `1` when another preload or host process already registered the global OpenTelemetry SDK. The plugin then skips its own NodeSDK lifecycle but still wires diagnostic listeners and honors `traces`/`metrics`/`logs`. |

## Privacy and content capture

Raw model/tool content is **not** exported by default. Spans carry bounded
identifiers (channel, provider, model, error category, hash-only request ids)
and never include prompt text, response text, tool inputs, tool outputs, or
session keys.Outbound model requests may include a W3C `traceparent` header. That header is
generated only from OpenClaw-owned diagnostic trace context for the active model
call. Existing caller-supplied `traceparent` headers are replaced, so plugins or
custom provider options cannot spoof cross-service trace ancestry.Set `diagnostics.otel.captureContent.*` to `true` only when your collector and
retention policy are approved for prompt, response, tool, or system-prompt
text. Each subkey is opt-in independently:

- `inputMessages` — user prompt content.
- `outputMessages` — model response content.
- `toolInputs` — tool argument payloads.
- `toolOutputs` — tool result payloads.
- `systemPrompt` — assembled system/developer prompt.

When any subkey is enabled, model and tool spans get bounded, redacted
`openclaw.content.*` attributes for that class only.

## Sampling and flushing

- **Traces:**`diagnostics.otel.sampleRate` (root-span only, `0.0` drops all,
`1.0` keeps all).
- **Metrics:**`diagnostics.otel.flushIntervalMs` (minimum `1000`).
- **Logs:** OTLP logs respect `logging.level` (file log level). They use the
diagnostic log-record redaction path, not console formatting. High-volume
installs should prefer OTLP collector sampling/filtering over local sampling.
- **File-log correlation:** JSONL file logs include top-level `traceId`,
`spanId`, `parentSpanId`, and `traceFlags` when the log call carries a valid
diagnostic trace context, which lets log processors join local log lines with
exported spans.
- **Request correlation:** Gateway HTTP requests and WebSocket frames create an
internal request trace scope. Logs and diagnostic events inside that scope
inherit the request trace by default, while agent run and model-call spans are
created as children so provider `traceparent` headers stay on the same trace.

## Exported metrics

### Model usage

- `openclaw.tokens` (counter, attrs: `openclaw.token`, `openclaw.channel`, `openclaw.provider`, `openclaw.model`, `openclaw.agent`)
- `openclaw.cost.usd` (counter, attrs: `openclaw.channel`, `openclaw.provider`, `openclaw.model`)
- `openclaw.run.duration_ms` (histogram, attrs: `openclaw.channel`, `openclaw.provider`, `openclaw.model`)
- `openclaw.context.tokens` (histogram, attrs: `openclaw.context`, `openclaw.channel`, `openclaw.provider`, `openclaw.model`)
- `gen_ai.client.token.usage` (histogram, GenAI semantic-conventions metric, attrs: `gen_ai.token.type` = `input`/`output`, `gen_ai.provider.name`, `gen_ai.operation.name`, `gen_ai.request.model`)
- `gen_ai.client.operation.duration` (histogram, seconds, GenAI semantic-conventions metric, attrs: `gen_ai.provider.name`, `gen_ai.operation.name`, `gen_ai.request.model`, optional `error.type`)
- `openclaw.model_call.duration_ms` (histogram, attrs: `openclaw.provider`, `openclaw.model`, `openclaw.api`, `openclaw.transport`, plus `openclaw.errorCategory` and `openclaw.failureKind` on classified errors)
- `openclaw.model_call.request_bytes` (histogram, UTF-8 byte size of the final model request payload; no raw payload content)
- `openclaw.model_call.response_bytes` (histogram, UTF-8 byte size of streamed model response events; no raw response content)
- `openclaw.model_call.time_to_first_byte_ms` (histogram, elapsed time before the first streamed response event)

### Message flow

- `openclaw.webhook.received` (counter, attrs: `openclaw.channel`, `openclaw.webhook`)
- `openclaw.webhook.error` (counter, attrs: `openclaw.channel`, `openclaw.webhook`)
- `openclaw.webhook.duration_ms` (histogram, attrs: `openclaw.channel`, `openclaw.webhook`)
- `openclaw.message.queued` (counter, attrs: `openclaw.channel`, `openclaw.source`)
- `openclaw.message.processed` (counter, attrs: `openclaw.channel`, `openclaw.outcome`)
- `openclaw.message.duration_ms` (histogram, attrs: `openclaw.channel`, `openclaw.outcome`)
- `openclaw.message.delivery.started` (counter, attrs: `openclaw.channel`, `openclaw.delivery.kind`)
- `openclaw.message.delivery.duration_ms` (histogram, attrs: `openclaw.channel`, `openclaw.delivery.kind`, `openclaw.outcome`, `openclaw.errorCategory`)

### Queues and sessions

- `openclaw.queue.lane.enqueue` (counter, attrs: `openclaw.lane`)
- `openclaw.queue.lane.dequeue` (counter, attrs: `openclaw.lane`)
- `openclaw.queue.depth` (histogram, attrs: `openclaw.lane` or `openclaw.channel=heartbeat`)
- `openclaw.queue.wait_ms` (histogram, attrs: `openclaw.lane`)
- `openclaw.session.state` (counter, attrs: `openclaw.state`, `openclaw.reason`)
- `openclaw.session.stuck` (counter, attrs: `openclaw.state`; emitted only for stale session bookkeeping with no active work)
- `openclaw.session.stuck_age_ms` (histogram, attrs: `openclaw.state`; emitted only for stale session bookkeeping with no active work)
- `openclaw.run.attempt` (counter, attrs: `openclaw.attempt`)

### Session liveness telemetry

`diagnostics.stuckSessionWarnMs` is the no-progress age threshold for session
liveness diagnostics. A `processing` session does not age toward this threshold
while OpenClaw observes reply, tool, status, block, or ACP runtime progress.
Typing keepalives are not counted as progress, so a silent model or harness can
still be detected.OpenClaw classifies sessions by the work it can still observe:

- `session.long_running`: active embedded work, model calls, or tool calls are
still making progress.
- `session.stalled`: active work exists, but the active run has not reported
recent progress.
- `session.stuck`: stale session bookkeeping with no active work. This is the
only liveness classification that releases the affected session lane.

Only `session.stuck` emits the `openclaw.session.stuck` counter, the
`openclaw.session.stuck_age_ms` histogram, and the `openclaw.session.stuck`
span. Repeated `session.stuck` diagnostics back off while the session remains
unchanged, so dashboards should alert on sustained increases rather than every
heartbeat tick. For the config knob and defaults, see
[Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference#diagnostics).

### Harness lifecycle

- `openclaw.harness.duration_ms` (histogram, attrs: `openclaw.harness.id`, `openclaw.harness.plugin`, `openclaw.outcome`, `openclaw.harness.phase` on errors)

### Exec

- `openclaw.exec.duration_ms` (histogram, attrs: `openclaw.exec.target`, `openclaw.exec.mode`, `openclaw.outcome`, `openclaw.failureKind`)

### Diagnostics internals (memory and tool loop)

- `openclaw.memory.heap_used_bytes` (histogram, attrs: `openclaw.memory.kind`)
- `openclaw.memory.rss_bytes` (histogram)
- `openclaw.memory.pressure` (counter, attrs: `openclaw.memory.level`)
- `openclaw.tool.loop.iterations` (counter, attrs: `openclaw.toolName`, `openclaw.outcome`)
- `openclaw.tool.loop.duration_ms` (histogram, attrs: `openclaw.toolName`, `openclaw.outcome`)

## Exported spans

- `openclaw.model.usage`
  - `openclaw.channel`, `openclaw.provider`, `openclaw.model`
  - `openclaw.tokens.*` (input/output/cache\_read/cache\_write/total)
  - `gen_ai.system` by default, or `gen_ai.provider.name` when the latest GenAI semantic conventions are opted in
  - `gen_ai.request.model`, `gen_ai.operation.name`, `gen_ai.usage.*`
- `openclaw.run`
  - `openclaw.outcome`, `openclaw.channel`, `openclaw.provider`, `openclaw.model`, `openclaw.errorCategory`
- `openclaw.model.call`
  - `gen_ai.system` by default, or `gen_ai.provider.name` when the latest GenAI semantic conventions are opted in
  - `gen_ai.request.model`, `gen_ai.operation.name`, `openclaw.provider`, `openclaw.model`, `openclaw.api`, `openclaw.transport`
  - `openclaw.errorCategory` and optional `openclaw.failureKind` on errors
  - `openclaw.model_call.request_bytes`, `openclaw.model_call.response_bytes`, `openclaw.model_call.time_to_first_byte_ms`
  - `openclaw.provider.request_id_hash` (bounded SHA-based hash of the upstream provider request id; raw ids are not exported)
- `openclaw.harness.run`
  - `openclaw.harness.id`, `openclaw.harness.plugin`, `openclaw.outcome`, `openclaw.provider`, `openclaw.model`, `openclaw.channel`
  - On completion: `openclaw.harness.result_classification`, `openclaw.harness.yield_detected`, `openclaw.harness.items.started`, `openclaw.harness.items.completed`, `openclaw.harness.items.active`
  - On error: `openclaw.harness.phase`, `openclaw.errorCategory`, optional `openclaw.harness.cleanup_failed`
- `openclaw.tool.execution`
  - `gen_ai.tool.name`, `openclaw.toolName`, `openclaw.errorCategory`, `openclaw.tool.params.*`
- `openclaw.exec`
  - `openclaw.exec.target`, `openclaw.exec.mode`, `openclaw.outcome`, `openclaw.failureKind`, `openclaw.exec.command_length`, `openclaw.exec.exit_code`, `openclaw.exec.timed_out`
- `openclaw.webhook.processed`
  - `openclaw.channel`, `openclaw.webhook`, `openclaw.chatId`
- `openclaw.webhook.error`
  - `openclaw.channel`, `openclaw.webhook`, `openclaw.chatId`, `openclaw.error`
- `openclaw.message.processed`
  - `openclaw.channel`, `openclaw.outcome`, `openclaw.chatId`, `openclaw.messageId`, `openclaw.reason`
- `openclaw.message.delivery`
  - `openclaw.channel`, `openclaw.delivery.kind`, `openclaw.outcome`, `openclaw.errorCategory`, `openclaw.delivery.result_count`
- `openclaw.session.stuck`
  - `openclaw.state`, `openclaw.ageMs`, `openclaw.queueDepth`
- `openclaw.context.assembled`
  - `openclaw.prompt.size`, `openclaw.history.size`, `openclaw.context.tokens`, `openclaw.errorCategory` (no prompt, history, response, or session-key content)
- `openclaw.tool.loop`
  - `openclaw.toolName`, `openclaw.outcome`, `openclaw.iterations`, `openclaw.errorCategory` (no loop messages, params, or tool output)
- `openclaw.memory.pressure`
  - `openclaw.memory.level`, `openclaw.memory.heap_used_bytes`, `openclaw.memory.rss_bytes`

When content capture is explicitly enabled, model and tool spans can also
include bounded, redacted `openclaw.content.*` attributes for the specific
content classes you opted into.

## Diagnostic event catalog

The events below back the metrics and spans above. Plugins can also subscribe
to them directly without OTLP export.**Model usage**

- `model.usage` — tokens, cost, duration, context, provider/model/channel,
session ids. `usage` is provider/turn accounting for cost and telemetry;
`context.used` is the current prompt/context snapshot and can be lower than
provider `usage.total` when cached input or tool-loop calls are involved.

**Message flow**

- `webhook.received` / `webhook.processed` / `webhook.error`
- `message.queued` / `message.processed`
- `message.delivery.started` / `message.delivery.completed` / `message.delivery.error`

**Queue and session**

- `queue.lane.enqueue` / `queue.lane.dequeue`
- `session.state` / `session.long_running` / `session.stalled` / `session.stuck`
- `run.attempt` / `run.progress`
- `diagnostic.heartbeat` (aggregate counters: webhooks/queue/session)

**Harness lifecycle**

- `harness.run.started` / `harness.run.completed` / `harness.run.error` —
per-run lifecycle for the agent harness. Includes `harnessId`, optional
`pluginId`, provider/model/channel, and run id. Completion adds
`durationMs`, `outcome`, optional `resultClassification`, `yieldDetected`,
and `itemLifecycle` counts. Errors add `phase`
(`prepare`/`start`/`send`/`resolve`/`cleanup`), `errorCategory`, and
optional `cleanupFailed`.

**Exec**

- `exec.process.completed` — terminal outcome, duration, target, mode, exit
code, and failure kind. Command text and working directories are not
included.

## Without an exporter

You can keep diagnostics events available to plugins or custom sinks without
running `diagnostics-otel`:

```
{
  diagnostics: { enabled: true },
}
```

For targeted debug output without raising `logging.level`, use diagnostics
flags. Flags are case-insensitive and support wildcards (e.g. `telegram.*` or
`*`):

```
{
  diagnostics: { flags: ["telegram.http"] },
}
```

Or as a one-off env override:

```
OPENCLAW_DIAGNOSTICS=telegram.http,telegram.payload openclaw gateway
```

Flag output goes to the standard log file (`logging.file`) and is still
redacted by `logging.redactSensitive`. Full guide:
[Diagnostics flags](https://docs.openclaw.ai/diagnostics/flags).

## Disable

```
{
  diagnostics: { otel: { enabled: false } },
}
```

You can also leave `diagnostics-otel` out of `plugins.allow`, or run
`openclaw plugins disable diagnostics-otel`.

## Related

- [Logging](https://docs.openclaw.ai/logging) — file logs, console output, CLI tailing, and the Control UI Logs tab
- [Gateway logging internals](https://docs.openclaw.ai/gateway/logging) — WS log styles, subsystem prefixes, and console capture
- [Diagnostics flags](https://docs.openclaw.ai/diagnostics/flags) — targeted debug-log flags
- [Diagnostics export](https://docs.openclaw.ai/gateway/diagnostics) — operator support-bundle tool (separate from OTEL export)
- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference#diagnostics) — full `diagnostics.*` field reference

[Logging](https://docs.openclaw.ai/logging) [Prometheus](https://docs.openclaw.ai/gateway/prometheus)

Ctrl+I

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
applies.`node.pair.approve` uses the pending request command list to derive additional
required scopes:

- Commandless request: `operator.pairing`
- Non-exec node commands: `operator.pairing` \+ `operator.write`
- `system.run`, `system.run.prepare`, or `system.which`:
`operator.pairing` \+ `operator.admin`

Node pairing establishes identity and trust. It does not replace the node’s
own `system.run` exec approval policy.

## Shared-secret auth

Shared gateway token/password auth is treated as trusted operator access for
that Gateway. OpenAI-compatible HTTP surfaces and `/tools/invoke` restore the
normal full operator default scope set for shared-secret bearer auth, even if a
caller sends narrower declared scopes.Identity-bearing modes, such as trusted proxy auth or private-ingress `none`,
can still honor explicit declared scopes. Use separate Gateways for real trust
boundary separation.

[Security audit checks](https://docs.openclaw.ai/gateway/security/audit-checks) [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing)

Ctrl+I

---

## Gateway-owned pairing - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/pairing>_

[OpenClaw home page](https://docs.openclaw.ai/)

Networking and discovery

Gateway-owned pairing

In Gateway-owned pairing, the **Gateway** is the source of truth for which nodes
are allowed to join. UIs (macOS app, future clients) are just frontends that
approve or reject pending requests.**Important:** WS nodes use **device pairing** (role `node`) during `connect`.
`node.pair.*` is a separate pairing store and does **not** gate the WS handshake.
Only clients that explicitly call `node.pair.*` use this flow.

## Concepts

- **Pending request**: a node asked to join; requires approval.
- **Paired node**: approved node with an issued auth token.
- **Transport**: the Gateway WS endpoint forwards requests but does not decide
membership. (Legacy TCP bridge support has been removed.)

## How pairing works

1. A node connects to the Gateway WS and requests pairing.
2. The Gateway stores a **pending request** and emits `node.pair.requested`.
3. You approve or reject the request (CLI or UI).
4. On approval, the Gateway issues a **new token** (tokens are rotated on re‑pair).
5. The node reconnects using the token and is now “paired”.

Pending requests expire automatically after **5 minutes**.

## CLI workflow (headless friendly)

```
openclaw nodes pending
openclaw nodes approve <requestId>
openclaw nodes reject <requestId>
openclaw nodes status
openclaw nodes remove --node <id|name|ip>
openclaw nodes rename --node <id|name|ip> --name "Living Room iPad"
```

`nodes status` shows paired/connected nodes and their capabilities.

## API surface (gateway protocol)

Events:

- `node.pair.requested` — emitted when a new pending request is created.
- `node.pair.resolved` — emitted when a request is approved/rejected/expired.

Methods:

- `node.pair.request` — create or reuse a pending request.
- `node.pair.list` — list pending + paired nodes (`operator.pairing`).
- `node.pair.approve` — approve a pending request (issues token).
- `node.pair.reject` — reject a pending request.
- `node.pair.remove` — remove a stale paired node entry.
- `node.pair.verify` — verify `{ nodeId, token }`.

Notes:

- `node.pair.request` is idempotent per node: repeated calls return the same
pending request.
- Repeated requests for the same pending node also refresh the stored node
metadata and the latest allowlisted declared command snapshot for operator visibility.
- Approval **always** generates a fresh token; no token is ever returned from
`node.pair.request`.
- Requests may include `silent: true` as a hint for auto-approval flows.
- `node.pair.approve`uses the pending request’s declared commands to enforce
extra approval scopes:

  - commandless request: `operator.pairing`
  - non-exec command request: `operator.pairing` \+ `operator.write`
  - `system.run` / `system.run.prepare` / `system.which` request:
    `operator.pairing` \+ `operator.admin`

Node pairing is a trust and identity flow plus token issuance. It does **not** pin the live node command surface per node.

- Live node commands come from what the node declares on connect after the gateway’s global node command policy (`gateway.nodes.allowCommands` and `denyCommands`) is applied.
- Per-node `system.run` allow and ask policy lives on the node in `exec.approvals.node.*`, not in the pairing record.

## Node command gating (2026.3.31+)

**Breaking change:** Starting with `2026.3.31`, node commands are disabled until node pairing is approved. Device pairing alone is no longer enough to expose declared node commands.

When a node connects for the first time, pairing is requested automatically. Until the pairing request is approved, all pending node commands from that node are filtered and will not execute. Once trust is established through pairing approval, the node’s declared commands become available subject to the normal command policy.This means:

- Nodes that were previously relying on device pairing alone to expose commands must now complete node pairing.
- Commands queued before pairing approval are dropped, not deferred.

## Node event trust boundaries (2026.3.31+)

**Breaking change:** Node-originated runs now stay on a reduced trusted surface.

Node-originated summaries and related session events are restricted to the intended trusted surface. Notification-driven or node-triggered flows that previously relied on broader host or session tool access may need adjustment. This hardening ensures that node events cannot escalate into host-level tool access beyond what the node’s trust boundary permits.Durable node presence updates follow the same identity boundary. The `node.presence.alive` event is
accepted only from authenticated node device sessions and updates pairing metadata only when the
device/node identity is already paired. Self-declared `client.id` values are not enough to write
last-seen state.

## Auto-approval (macOS app)

The macOS app can optionally attempt a **silent approval** when:

- the request is marked `silent`, and
- the app can verify an SSH connection to the gateway host using the same user.

If silent approval fails, it falls back to the normal “Approve/Reject” prompt.

## Trusted-CIDR device auto-approval

WS device pairing for `role: node` remains manual by default. For private
node networks where the Gateway already trusts the network path, operators can
opt in with explicit CIDRs or exact IPs:

```
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24"],
      },
    },
  },
}
```

Security boundary:

- Disabled when `gateway.nodes.pairing.autoApproveCidrs` is unset.
- No blanket LAN or private-network auto-approve mode exists.
- Only fresh `role: node` device pairing with no requested scopes is eligible.
- Operator, browser, Control UI, and WebChat clients stay manual.
- Role, scope, metadata, and public-key upgrades stay manual.
- Same-host loopback trusted-proxy header paths are not eligible because that
path can be spoofed by local callers.

## Metadata-upgrade auto-approval

When an already paired device reconnects with only non-sensitive metadata
changes (for example, display name or client platform hints), OpenClaw treats
that as a `metadata-upgrade`. Silent auto-approval is narrow: it applies only
to trusted non-browser local reconnects that already proved possession of local
or shared credentials, including same-host native app reconnects after OS
version metadata changes. Browser/Control UI clients and remote clients still
use the explicit re-approval flow. Scope upgrades (read to write/admin) and
public key changes are **not** eligible for metadata-upgrade auto-approval —
they stay as explicit re-approval requests.

## QR pairing helpers

`/pair qr` renders the pairing payload as structured media so mobile and
browser clients can scan it directly.Deleting a device also sweeps any stale pending pairing requests for that
device id, so `nodes pending` does not show orphaned rows after a revoke.

## Locality and forwarded headers

Gateway pairing treats a connection as loopback only when both the raw socket
and any upstream proxy evidence agree. If a request arrives on loopback but
carries `X-Forwarded-For` / `X-Forwarded-Host` / `X-Forwarded-Proto` headers
that point at a non-local origin, that forwarded-header evidence disqualifies
the loopback locality claim. The pairing path then requires explicit approval
instead of silently treating the request as a same-host connect. See
[Trusted Proxy Auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth) for the equivalent rule on
operator auth.

## Storage (local, private)

Pairing state is stored under the Gateway state directory (default `~/.openclaw`):

- `~/.openclaw/nodes/paired.json`
- `~/.openclaw/nodes/pending.json`

If you override `OPENCLAW_STATE_DIR`, the `nodes/` folder moves with it.Security notes:

- Tokens are secrets; treat `paired.json` as sensitive.
- Rotating a token requires re-approval (or deleting the node entry).

## Transport behavior

- The transport is **stateless**; it does not store membership.
- If the Gateway is offline or pairing is disabled, nodes cannot pair.
- If the Gateway is in remote mode, pairing still happens against the remote Gateway’s store.

## Related

- [Channel pairing](https://docs.openclaw.ai/channels/pairing)
- [Nodes](https://docs.openclaw.ai/nodes)
- [Devices CLI](https://docs.openclaw.ai/cli/devices)

[Network model](https://docs.openclaw.ai/gateway/network-model) [Discovery and transports](https://docs.openclaw.ai/gateway/discovery)

Ctrl+I

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
sum by (provider) (rate(openclaw_model_tokens_total[1m]))

# Spend (USD) over the last hour, by model
sum by (model) (increase(openclaw_model_cost_usd_total[1h]))

# 95th percentile model run duration
histogram_quantile(
  0.95,
  sum by (le, provider, model)
    (rate(openclaw_run_duration_seconds_bucket[5m]))
)

# Queue wait time SLO (95p under 2s)
histogram_quantile(
  0.95,
  sum by (le, lane) (rate(openclaw_queue_lane_wait_seconds_bucket[5m]))
) < 2

# Dropped Prometheus series (cardinality alarm)
increase(openclaw_prometheus_series_dropped_total[15m]) > 0
```

Prefer `gen_ai_client_token_usage` for cross-provider dashboards: it follows the OpenTelemetry GenAI semantic conventions and is consistent with metrics from non-OpenClaw GenAI services.

## Choosing between Prometheus and OpenTelemetry export

OpenClaw supports both surfaces independently. You can run either, both, or neither.

- diagnostics-prometheus

- diagnostics-otel

- **Pull** model: Prometheus scrapes `/api/diagnostics/prometheus`.
- No external collector required.
- Authenticated through normal Gateway auth.
- Surface is metrics only (no traces or logs).
- Best for stacks already standardized on Prometheus + Grafana.

- **Push** model: OpenClaw sends OTLP/HTTP to a collector or OTLP-compatible backend.
- Surface includes metrics, traces, and logs.
- Bridges to Prometheus through an OpenTelemetry Collector (`prometheus` or `prometheusremotewrite` exporter) when you need both.
- See [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) for the full catalog.

## Troubleshooting

Empty response body

- Check `diagnostics.enabled: true` in config.
- Confirm the plugin is enabled and loaded with `openclaw plugins list --enabled`.
- Generate some traffic; counters and histograms only emit lines after at least one event.

401 / unauthorized

The endpoint requires the Gateway operator scope (`auth: "gateway"` with `gatewayRuntimeScopeSurface: "trusted-operator"`). Use the same token or password Prometheus uses for any other Gateway operator route. There is no public unauthenticated mode.

\`openclaw\_prometheus\_series\_dropped\_total\` is climbing

A new attribute is exceeding the **2048**-series cap. Inspect recent metrics for an unexpectedly high-cardinality label and fix it at the source. The exporter intentionally drops new series instead of silently rewriting labels.

Prometheus shows stale series after a restart

The plugin keeps state in memory only. After a Gateway restart, counters reset to zero and gauges restart at their next reported value. Use PromQL `rate()` and `increase()` to handle resets cleanly.

## Related

- [Diagnostics export](https://docs.openclaw.ai/gateway/diagnostics) — local diagnostics zip for support bundles
- [Health and readiness](https://docs.openclaw.ai/gateway/health) — `/healthz` and `/readyz` probes
- [Logging](https://docs.openclaw.ai/logging) — file-based logging
- [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) — OTLP push for traces, metrics, and logs

[OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) [Gateway logging](https://docs.openclaw.ai/gateway/logging)

Ctrl+I

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
        "role": "operator",\
        "scopes": ["operator.approvals", "operator.read", "operator.talk.secrets", "operator.write"]\
      }\
    ]
  }
}
```

For the built-in node/operator bootstrap flow, the primary node token stays
`scopes: []` and any handed-off operator token stays bounded to the bootstrap
operator allowlist (`operator.approvals`, `operator.read`,
`operator.talk.secrets`, `operator.write`). Bootstrap scope checks stay
role-prefixed: operator entries only satisfy operator requests, and non-operator
roles still need scopes under their own role prefix.

### Node example

```
{
  "type": "req",
  "id": "…",
  "method": "connect",
  "params": {
    "minProtocol": 3,
    "maxProtocol": 3,
    "client": {
      "id": "ios-node",
      "version": "1.2.3",
      "platform": "ios",
      "mode": "node"
    },
    "role": "node",
    "scopes": [],
    "caps": ["camera", "canvas", "screen", "location", "voice"],
    "commands": ["camera.snap", "canvas.navigate", "screen.record", "location.get"],
    "permissions": { "camera.capture": true, "screen.record": false },
    "auth": { "token": "…" },
    "locale": "en-US",
    "userAgent": "openclaw-ios/1.2.3",
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

## Framing

- **Request**: `{type:"req", id, method, params}`
- **Response**: `{type:"res", id, ok, payload|error}`
- **Event**: `{type:"event", event, payload, seq?, stateVersion?}`

Side-effecting methods require **idempotency keys** (see schema).

## Roles + scopes

### Roles

- `operator` = control plane client (CLI/UI/automation).
- `node` = capability host (camera/screen/canvas/system.run).

### Scopes (operator)

Common scopes:

- `operator.read`
- `operator.write`
- `operator.admin`
- `operator.approvals`
- `operator.pairing`
- `operator.talk.secrets`

`talk.config` with `includeSecrets: true` requires `operator.talk.secrets`
(or `operator.admin`).Plugin-registered gateway RPC methods may request their own operator scope, but
reserved core admin prefixes (`config.*`, `exec.approvals.*`, `wizard.*`,
`update.*`) always resolve to `operator.admin`.Method scope is only the first gate. Some slash commands reached through
`chat.send` apply stricter command-level checks on top. For example, persistent
`/config set` and `/config unset` writes require `operator.admin`.`node.pair.approve` also has an extra approval-time scope check on top of the
base method scope:

- commandless requests: `operator.pairing`
- requests with non-exec node commands: `operator.pairing` \+ `operator.write`
- requests that include `system.run`, `system.run.prepare`, or `system.which`:
`operator.pairing` \+ `operator.admin`

### Caps/commands/permissions (node)

Nodes declare capability claims at connect time:

- `caps`: high-level capability categories.
- `commands`: command allowlist for invoke.
- `permissions`: granular toggles (e.g. `screen.record`, `camera.capture`).

The Gateway treats these as **claims** and enforces server-side allowlists.

## Presence

- `system-presence` returns entries keyed by device identity.
- Presence entries include `deviceId`, `roles`, and `scopes` so UIs can show a single row per device
even when it connects as both **operator** and **node**.
- `node.list` includes optional `lastSeenAtMs` and `lastSeenReason` fields. Connected nodes report
their current connection time as `lastSeenAtMs` with reason `connect`; paired nodes can also report
durable background presence when a trusted node event updates their pairing metadata.

### Node background alive event

Nodes may call `node.event` with `event: "node.presence.alive"` to record that a paired node was
alive during a background wake without marking it connected.

```
{
  "event": "node.presence.alive",
  "payloadJSON": "{\"trigger\":\"silent_push\",\"sentAtMs\":1737264000000,\"displayName\":\"Peter's iPhone\",\"version\":\"2026.4.28\",\"platform\":\"iOS 18.4.0\",\"deviceFamily\":\"iPhone\",\"modelIdentifier\":\"iPhone17,1\",\"pushTransport\":\"relay\"}"
}
```

`trigger` is a closed enum: `background`, `silent_push`, `bg_app_refresh`,
`significant_location`, `manual`, or `connect`. Unknown trigger strings are normalized to
`background` by the gateway before persistence. The event is durable only for authenticated node
device sessions; device-less or unpaired sessions return `handled: false`.Successful gateways return a structured result:

```
{
  "ok": true,
  "event": "node.presence.alive",
  "handled": true,
  "reason": "persisted"
}
```

Older gateways may still return `{ "ok": true }` for `node.event`; clients should treat that as an
acknowledged RPC, not as durable presence persistence.

## Broadcast event scoping

Server-pushed WebSocket broadcast events are scope-gated so that pairing-scoped or node-only sessions do not passively receive session content.

- **Chat, agent, and tool-result frames** (including streamed `agent` events and tool call results) require at least `operator.read`. Sessions without `operator.read` skip these frames entirely.
- **Plugin-defined `plugin.*` broadcasts** are gated to `operator.write` or `operator.admin`, depending on how the plugin registered them.
- **Status and transport events** (`heartbeat`, `presence`, `tick`, connect/disconnect lifecycle, etc.) remain unrestricted so transport health stays observable to every authenticated session.
- **Unknown broadcast event families** are scope-gated by default (fail-closed) unless a registered handler explicitly relaxes them.

Each client connection keeps its own per-client sequence number so broadcasts preserve monotonic ordering on that socket even when different clients see different scope-filtered subsets of the event stream.

## Common RPC method families

The public WS surface is broader than the handshake/auth examples above. This
is not a generated dump — `hello-ok.features.methods` is a conservative
discovery list built from `src/gateway/server-methods-list.ts` plus loaded
plugin/channel method exports. Treat it as feature discovery, not a full
enumeration of `src/gateway/server-methods/*.ts`.

System and identity

- `health` returns the cached or freshly probed gateway health snapshot.
- `diagnostics.stability` returns the recent bounded diagnostic stability recorder. It keeps operational metadata such as event names, counts, byte sizes, memory readings, queue/session state, channel/plugin names, and session ids. It does not keep chat text, webhook bodies, tool outputs, raw request or response bodies, tokens, cookies, or secret values. Operator read scope is required.
- `status` returns the `/status`-style gateway summary; sensitive fields are included only for admin-scoped operator clients.
- `gateway.identity.get` returns the gateway device identity used by relay and pairing flows.
- `system-presence` returns the current presence snapshot for connected operator/node devices.
- `system-event` appends a system event and can update/broadcast presence context.
- `last-heartbeat` returns the latest persisted heartbeat event.
- `set-heartbeats` toggles heartbeat processing on the gateway.

Models and usage

- `models.list` returns the runtime-allowed model catalog. Pass `{ "view": "configured" }` for picker-sized configured models (`agents.defaults.models` first, then `models.providers.*.models`), or `{ "view": "all" }` for the full catalog.
- `usage.status` returns provider usage windows/remaining quota summaries.
- `usage.cost` returns aggregated cost usage summaries for a date range.
- `doctor.memory.status` returns vector-memory / cached embedding readiness for the active default agent workspace. Pass `{ "probe": true }` or `{ "deep": true }` only when the caller explicitly wants a live embedding provider ping.
- `doctor.memory.remHarness` returns a bounded, read-only REM harness preview for remote control-plane clients. It can include workspace paths, memory snippets, rendered grounded markdown, and deep promotion candidates, so callers need `operator.read`.
- `sessions.usage` returns per-session usage summaries.
- `sessions.usage.timeseries` returns timeseries usage for one session.
- `sessions.usage.logs` returns usage log entries for one session.

Channels and login helpers

- `channels.status` returns built-in + bundled channel/plugin status summaries.
- `channels.logout` logs out a specific channel/account where the channel supports logout.
- `web.login.start` starts a QR/web login flow for the current QR-capable web channel provider.
- `web.login.wait` waits for that QR/web login flow to complete and starts the channel on success.
- `push.test` sends a test APNs push to a registered iOS node.
- `voicewake.get` returns the stored wake-word triggers.
- `voicewake.set` updates wake-word triggers and broadcasts the change.

Messaging and logs

- `send` is the direct outbound-delivery RPC for channel/account/thread-targeted sends outside the chat runner.
- `logs.tail` returns the configured gateway file-log tail with cursor/limit and max-byte controls.

Talk and TTS

- `talk.config` returns the effective Talk config payload; `includeSecrets` requires `operator.talk.secrets` (or `operator.admin`).
- `talk.mode` sets/broadcasts the current Talk mode state for WebChat/Control UI clients.
- `talk.speak` synthesizes speech through the active Talk speech provider.
- `tts.status` returns TTS enabled state, active provider, fallback providers, and provider config state.
- `tts.providers` returns the visible TTS provider inventory.
- `tts.enable` and `tts.disable` toggle TTS prefs state.
- `tts.setProvider` updates the preferred TTS provider.
- `tts.convert` runs one-shot text-to-speech conversion.

Secrets, config, update, and wizard

- `secrets.reload` re-resolves active SecretRefs and swaps runtime secret state only on full success.
- `secrets.resolve` resolves command-target secret assignments for a specific command/target set.
- `config.get` returns the current config snapshot and hash.
- `config.set` writes a validated config payload.
- `config.patch` merges a partial config update.
- `config.apply` validates + replaces the full config payload.
- `config.schema` returns the live config schema payload used by Control UI and CLI tooling: schema, `uiHints`, version, and generation metadata, including plugin + channel schema metadata when the runtime can load it. The schema includes field `title` / `description` metadata derived from the same labels and help text used by the UI, including nested object, wildcard, array-item, and `anyOf` / `oneOf` / `allOf` composition branches when matching field documentation exists.
- `config.schema.lookup` returns a path-scoped lookup payload for one config path: normalized path, a shallow schema node, matched hint + `hintPath`, and immediate child summaries for UI/CLI drill-down. Lookup schema nodes keep the user-facing docs and common validation fields (`title`, `description`, `type`, `enum`, `const`, `format`, `pattern`, numeric/string/array/object bounds, and flags like `additionalProperties`, `deprecated`, `readOnly`, `writeOnly`). Child summaries expose `key`, normalized `path`, `type`, `required`, `hasChildren`, plus the matched `hint` / `hintPath`.
- `update.run` runs the gateway update flow and schedules a restart only when the update itself succeeded. Package-manager updates force a non-deferred, no-cooldown update restart after the package swap so the old Gateway process does not keep lazy-loading from a replaced `dist` tree.
- `update.status` returns the latest cached update restart sentinel, including the post-restart running version when available.
- `wizard.start`, `wizard.next`, `wizard.status`, and `wizard.cancel` expose the onboarding wizard over WS RPC.

Agent and workspace helpers

- `agents.list` returns configured agent entries, including effective model and runtime metadata.
- `agents.create`, `agents.update`, and `agents.delete` manage agent records and workspace wiring.
- `agents.files.list`, `agents.files.get`, and `agents.files.set` manage the bootstrap workspace files exposed for an agent.
- `artifacts.list`, `artifacts.get`, and `artifacts.download` expose transcript-derived artifact summaries and downloads for an explicit `sessionKey`, `runId`, or `taskId` scope. Run and task queries resolve the owning session server-side and only return transcript media with matching provenance; unsafe or local URL sources return unsupported downloads instead of fetching server-side.
- `agent.identity.get` returns the effective assistant identity for an agent or session.
- `agent.wait` waits for a run to finish and returns the terminal snapshot when available.

Session control

- `sessions.list` returns the current session index, including per-row `agentRuntime` metadata when an agent runtime backend is configured.
- `sessions.subscribe` and `sessions.unsubscribe` toggle session change event subscriptions for the current WS client.
- `sessions.messages.subscribe` and `sessions.messages.unsubscribe` toggle transcript/message event subscriptions for one session.
- `sessions.preview` returns bounded transcript previews for specific session keys.
- `sessions.resolve` resolves or canonicalizes a session target.
- `sessions.create` creates a new session entry.
- `sessions.send` sends a message into an existing session.
- `sessions.steer` is the interrupt-and-steer variant for an active session.
- `sessions.abort` aborts active work for a session. A caller may pass `key` plus optional `runId`, or pass `runId` alone for active runs the Gateway can resolve to a session.
- `sessions.patch` updates session metadata/overrides and reports the resolved canonical model plus effective `agentRuntime`.
- `sessions.reset`, `sessions.delete`, and `sessions.compact` perform session maintenance.
- `sessions.get` returns the full stored session row.
- Chat execution still uses `chat.history`, `chat.send`, `chat.abort`, and `chat.inject`. `chat.history` is display-normalized for UI clients: inline directive tags are stripped from visible text, plain-text tool-call XML payloads (including `<tool_call>...</tool_call>`, `<function_call>...</function_call>`, `<tool_calls>...</tool_calls>`, `<function_calls>...</function_calls>`, and truncated tool-call blocks) and leaked ASCII/full-width model control tokens are stripped, pure silent-token assistant rows such as exact `NO_REPLY` / `no_reply` are omitted, and oversized rows can be replaced with placeholders.

Device pairing and device tokens

- `device.pair.list` returns pending and approved paired devices.
- `device.pair.approve`, `device.pair.reject`, and `device.pair.remove` manage device-pairing records.
- `device.token.rotate` rotates a paired device token within its approved role and caller scope bounds.
- `device.token.revoke` revokes a paired device token within its approved role and caller scope bounds.

Node pairing, invoke, and pending work

- `node.pair.request`, `node.pair.list`, `node.pair.approve`, `node.pair.reject`, `node.pair.remove`, and `node.pair.verify` cover node pairing and bootstrap verification.
- `node.list` and `node.describe` return known/connected node state.
- `node.rename` updates a paired node label.
- `node.invoke` forwards a command to a connected node.
- `node.invoke.result` returns the result for an invoke request.
- `node.event` carries node-originated events back into the gateway.
- `node.canvas.capability.refresh` refreshes scoped canvas-capability tokens.
- `node.pending.pull` and `node.pending.ack` are the connected-node queue APIs.
- `node.pending.enqueue` and `node.pending.drain` manage durable pending work for offline/disconnected nodes.

Approval families

- `exec.approval.request`, `exec.approval.get`, `exec.approval.list`, and `exec.approval.resolve` cover one-shot exec approval requests plus pending approval lookup/replay.
- `exec.approval.waitDecision` waits on one pending exec approval and returns the final decision (or `null` on timeout).
- `exec.approvals.get` and `exec.approvals.set` manage gateway exec approval policy snapshots.
- `exec.approvals.node.get` and `exec.approvals.node.set` manage node-local exec approval policy via node relay commands.
- `plugin.approval.request`, `plugin.approval.list`, `plugin.approval.waitDecision`, and `plugin.approval.resolve` cover plugin-defined approval flows.

Automation, skills, and tools

- Automation: `wake` schedules an immediate or next-heartbeat wake text injection; `cron.list`, `cron.status`, `cron.add`, `cron.update`, `cron.remove`, `cron.run`, `cron.runs` manage scheduled work.
- Skills and tools: `commands.list`, `skills.*`, `tools.catalog`, `tools.effective`, `tools.invoke`.

### Common event families

- `chat`: UI chat updates such as `chat.inject` and other transcript-only chat
events.
- `session.message` and `session.tool`: transcript/event-stream updates for a
subscribed session.
- `sessions.changed`: session index or metadata changed.
- `presence`: system presence snapshot updates.
- `tick`: periodic keepalive / liveness event.
- `health`: gateway health snapshot update.
- `heartbeat`: heartbeat event stream update.
- `cron`: cron run/job change event.
- `shutdown`: gateway shutdown notification.
- `node.pair.requested` / `node.pair.resolved`: node pairing lifecycle.
- `node.invoke.request`: node invoke request broadcast.
- `device.pair.requested` / `device.pair.resolved`: paired-device lifecycle.
- `voicewake.changed`: wake-word trigger config changed.
- `exec.approval.requested` / `exec.approval.resolved`: exec approval
lifecycle.
- `plugin.approval.requested` / `plugin.approval.resolved`: plugin approval
lifecycle.

### Node helper methods

- Nodes may call `skills.bins` to fetch the current list of skill executables
for auto-allow checks.

### Operator helper methods

- Operators may call `commands.list` (`operator.read`) to fetch the runtime
command inventory for an agent.

  - `agentId` is optional; omit it to read the default agent workspace.
  - `scope` controls which surface the primary `name` targets:

    - `text` returns the primary text command token without the leading `/`
    - `native` and the default `both` path return provider-aware native names
      when available
  - `textAliases` carries exact slash aliases such as `/model` and `/m`.
  - `nativeName` carries the provider-aware native command name when one exists.
  - `provider` is optional and only affects native naming plus native plugin
    command availability.
  - `includeArgs=false` omits serialized argument metadata from the response.
- Operators may call `tools.catalog` (`operator.read`) to fetch the runtime tool catalog for an
agent. The response includes grouped tools and provenance metadata:

  - `source`: `core` or `plugin`
  - `pluginId`: plugin owner when `source="plugin"`
  - `optional`: whether a plugin tool is optional
- Operators may call `tools.effective` (`operator.read`) to fetch the runtime-effective tool
inventory for a session.

  - `sessionKey` is required.
  - The gateway derives trusted runtime context from the session server-side instead of accepting
    caller-supplied auth or delivery context.
  - The response is session-scoped and reflects what the active conversation can use right now,
    including core, plugin, and channel tools.
- Operators may call `tools.invoke` (`operator.write`) to invoke one available tool through the
same gateway policy path as `/tools/invoke`.

  - `name` is required. `args`, `sessionKey`, `agentId`, `confirm`, and
    `idempotencyKey` are optional.
  - If both `sessionKey` and `agentId` are present, the resolved session agent must match
    `agentId`.
  - The response is an SDK-facing envelope with `ok`, `toolName`, optional `output`, and typed
    `error` fields. Approval or policy refusals return `ok:false` in the payload rather than
    bypassing the gateway tool policy pipeline.
- Operators may call `skills.status` (`operator.read`) to fetch the visible
skill inventory for an agent.

  - `agentId` is optional; omit it to read the default agent workspace.
  - The response includes eligibility, missing requirements, config checks, and
    sanitized install options without exposing raw secret values.
- Operators may call `skills.search` and `skills.detail` (`operator.read`) for
ClawHub discovery metadata.
- Operators may call `skills.install` (`operator.admin`) in two modes:

  - ClawHub mode: `{ source: "clawhub", slug, version?, force? }` installs a
    skill folder into the default agent workspace `skills/` directory.
  - Gateway installer mode: `{ name, installId, dangerouslyForceUnsafeInstall?, timeoutMs? }`
    runs a declared `metadata.openclaw.install` action on the gateway host.
- Operators may call `skills.update` (`operator.admin`) in two modes:

  - ClawHub mode updates one tracked slug or all tracked ClawHub installs in
    the default agent workspace.
  - Config mode patches `skills.entries.<skillKey>` values such as `enabled`,
    `apiKey`, and `env`.

### `models.list` views

`models.list` accepts an optional `view` parameter:

- Omitted or `"default"`: current runtime behavior. If `agents.defaults.models` is configured, the response is the allowed catalog; otherwise the response is the full Gateway catalog.
- `"configured"`: picker-sized behavior. If `agents.defaults.models` is configured, it still wins. Otherwise the response uses explicit `models.providers.*.models` entries, falling back to the full catalog only when no configured model rows exist.
- `"all"`: full Gateway catalog, bypassing `agents.defaults.models`. Use this for diagnostics and discovery UIs, not normal model pickers.

## Exec approvals

- When an exec request needs approval, the gateway broadcasts `exec.approval.requested`.
- Operator clients resolve by calling `exec.approval.resolve` (requires `operator.approvals` scope).
- For `host=node`, `exec.approval.request` must include `systemRunPlan` (canonical `argv`/`cwd`/`rawCommand`/session metadata). Requests missing `systemRunPlan` are rejected.
- After approval, forwarded `node.invoke system.run` calls reuse that canonical
`systemRunPlan` as the authoritative command/cwd/session context.
- If a caller mutates `command`, `rawCommand`, `cwd`, `agentId`, or
`sessionKey` between prepare and the final approved `system.run` forward, the
gateway rejects the run instead of trusting the mutated payload.

## Agent delivery fallback

- `agent` requests can include `deliver=true` to request outbound delivery.
- `bestEffortDeliver=false` keeps strict behavior: unresolved or internal-only delivery targets return `INVALID_REQUEST`.
- `bestEffortDeliver=true` allows fallback to session-only execution when no external deliverable route can be resolved (for example internal/webchat sessions or ambiguous multi-channel configs).

## Versioning

- `PROTOCOL_VERSION` lives in `src/gateway/protocol/schema/protocol-schemas.ts`.
- Clients send `minProtocol` \+ `maxProtocol`; the server rejects mismatches.
- Schemas + models are generated from TypeBox definitions:
  - `pnpm protocol:gen`
  - `pnpm protocol:gen:swift`
  - `pnpm protocol:check`

### Client constants

The reference client in `src/gateway/client.ts` uses these defaults. Values are
stable across protocol v3 and are the expected baseline for third-party clients.

| Constant | Default | Source |
| --- | --- | --- |
| `PROTOCOL_VERSION` | `3` | `src/gateway/protocol/schema/protocol-schemas.ts` |
| Request timeout (per RPC) | `30_000` ms | `src/gateway/client.ts` (`requestTimeoutMs`) |
| Preauth / connect-challenge timeout | `15_000` ms | `src/gateway/handshake-timeouts.ts` (config/env can raise the paired server/client budget) |
| Initial reconnect backoff | `1_000` ms | `src/gateway/client.ts` (`backoffMs`) |
| Max reconnect backoff | `30_000` ms | `src/gateway/client.ts` (`scheduleReconnect`) |
| Fast-retry clamp after device-token close | `250` ms | `src/gateway/client.ts` |
| Force-stop grace before `terminate()` | `250` ms | `FORCE_STOP_TERMINATE_GRACE_MS` |
| `stopAndWait()` default timeout | `1_000` ms | `STOP_AND_WAIT_TIMEOUT_MS` |
| Default tick interval (pre `hello-ok`) | `30_000` ms | `src/gateway/client.ts` |
| Tick-timeout close | code `4000` when silence exceeds `tickIntervalMs * 2` | `src/gateway/client.ts` |
| `MAX_PAYLOAD_BYTES` | `25 * 1024 * 1024` (25 MB) | `src/gateway/server-constants.ts` |

The server advertises the effective `policy.tickIntervalMs`, `policy.maxPayload`,
and `policy.maxBufferedBytes` in `hello-ok`; clients should honor those values
rather than the pre-handshake defaults.

## Auth

- Shared-secret gateway auth uses `connect.params.auth.token` or
`connect.params.auth.password`, depending on the configured auth mode.
- Identity-bearing modes such as Tailscale Serve
(`gateway.auth.allowTailscale: true`) or non-loopback
`gateway.auth.mode: "trusted-proxy"` satisfy the connect auth check from
request headers instead of `connect.params.auth.*`.
- Private-ingress `gateway.auth.mode: "none"` skips shared-secret connect auth
entirely; do not expose that mode on public/untrusted ingress.
- After pairing, the Gateway issues a **device token** scoped to the connection
role + scopes. It is returned in `hello-ok.auth.deviceToken` and should be
persisted by the client for future connects.
- Clients should persist the primary `hello-ok.auth.deviceToken` after any
successful connect.
- Reconnecting with that **stored** device token should also reuse the stored
approved scope set for that token. This preserves read/probe/status access
that was already granted and avoids silently collapsing reconnects to a
narrower implicit admin-only scope.
- Client-side connect auth assembly (`selectConnectAuth` in
`src/gateway/client.ts`):

  - `auth.password` is orthogonal and is always forwarded when set.
  - `auth.token` is populated in priority order: explicit shared token first,
    then an explicit `deviceToken`, then a stored per-device token (keyed by
    `deviceId` \+ `role`).
  - `auth.bootstrapToken` is sent only when none of the above resolved an
    `auth.token`. A shared token or any resolved device token suppresses it.
  - Auto-promotion of a stored device token on the one-shot
    `AUTH_TOKEN_MISMATCH` retry is gated to **trusted endpoints only** —
    loopback, or `wss://` with a pinned `tlsFingerprint`. Public `wss://`
    without pinning does not qualify.
- Additional `hello-ok.auth.deviceTokens` entries are bootstrap handoff tokens.
Persist them only when the connect used bootstrap auth on a trusted transport
such as `wss://` or loopback/local pairing.
- If a client supplies an **explicit**`deviceToken` or explicit `scopes`, that
caller-requested scope set remains authoritative; cached scopes are only
reused when the client is reusing the stored per-device token.
- Device tokens can be rotated/revoked via `device.token.rotate` and
`device.token.revoke` (requires `operator.pairing` scope).
- `device.token.rotate` returns rotation metadata. It echoes the replacement
bearer token only for same-device calls that are already authenticated with
that device token, so token-only clients can persist their replacement before
reconnecting. Shared/admin rotations do not echo the bearer token.
- Token issuance, rotation, and revocation stay bounded to the approved role set
recorded in that device’s pairing entry; token mutation cannot expand or
target a device role that pairing approval never granted.
- For paired-device token sessions, device management is self-scoped unless the
caller also has `operator.admin`: non-admin callers can remove/revoke/rotate
only their **own** device entry.
- `device.token.rotate` and `device.token.revoke` also check the target operator
token scope set against the caller’s current session scopes. Non-admin callers
cannot rotate or revoke a broader operator token than they already hold.
- Auth failures include `error.details.code` plus recovery hints:

  - `error.details.canRetryWithDeviceToken` (boolean)
  - `error.details.recommendedNextStep` (`retry_with_device_token`, `update_auth_configuration`, `update_auth_credentials`, `wait_then_retry`, `review_auth_configuration`)
- Client behavior for `AUTH_TOKEN_MISMATCH`:

  - Trusted clients may attempt one bounded retry with a cached per-device token.
  - If that retry fails, clients should stop automatic reconnect loops and surface operator action guidance.

## Device identity + pairing

- Nodes should include a stable device identity (`device.id`) derived from a
keypair fingerprint.
- Gateways issue tokens per device + role.
- Pairing approvals are required for new device IDs unless local auto-approval
is enabled.
- Pairing auto-approval is centered on direct local loopback connects.
- OpenClaw also has a narrow backend/container-local self-connect path for
trusted shared-secret helper flows.
- Same-host tailnet or LAN connects are still treated as remote for pairing and
require approval.
- WS clients normally include `device` identity during `connect` (operator +
node). The only device-less operator exceptions are explicit trust paths:

  - `gateway.controlUi.allowInsecureAuth=true` for localhost-only insecure HTTP compatibility.
  - successful `gateway.auth.mode: "trusted-proxy"` operator Control UI auth.
  - `gateway.controlUi.dangerouslyDisableDeviceAuth=true` (break-glass, severe security downgrade).
  - direct-loopback `gateway-client` backend RPCs authenticated with the shared
    gateway token/password.
- All connections must sign the server-provided `connect.challenge` nonce.

### Device auth migration diagnostics

For legacy clients that still use pre-challenge signing behavior, `connect` now returns
`DEVICE_AUTH_*` detail codes under `error.details.code` with a stable `error.details.reason`.Common migration failures:

| Message | details.code | details.reason | Meaning |
| --- | --- | --- | --- |
| `device nonce required` | `DEVICE_AUTH_NONCE_REQUIRED` | `device-nonce-missing` | Client omitted `device.nonce` (or sent blank). |
| `device nonce mismatch` | `DEVICE_AUTH_NONCE_MISMATCH` | `device-nonce-mismatch` | Client signed with a stale/wrong nonce. |
| `device signature invalid` | `DEVICE_AUTH_SIGNATURE_INVALID` | `device-signature` | Signature payload does not match v2 payload. |
| `device signature expired` | `DEVICE_AUTH_SIGNATURE_EXPIRED` | `device-signature-stale` | Signed timestamp is outside allowed skew. |
| `device identity mismatch` | `DEVICE_AUTH_DEVICE_ID_MISMATCH` | `device-id-mismatch` | `device.id` does not match public key fingerprint. |
| `device public key invalid` | `DEVICE_AUTH_PUBLIC_KEY_INVALID` | `device-public-key` | Public key format/canonicalization failed. |

Migration target:

- Always wait for `connect.challenge`.
- Sign the v2 payload that includes the server nonce.
- Send the same nonce in `connect.params.device.nonce`.
- Preferred signature payload is `v3`, which binds `platform` and `deviceFamily`
in addition to device/client/role/scopes/token/nonce fields.
- Legacy `v2` signatures remain accepted for compatibility, but paired-device
metadata pinning still controls command policy on reconnect.

## TLS + pinning

- TLS is supported for WS connections.
- Clients may optionally pin the gateway cert fingerprint (see `gateway.tls`
config plus `gateway.remote.tlsFingerprint` or CLI `--tls-fingerprint`).

## Scope

This protocol exposes the **full gateway API** (status, channels, models, chat,
agent, sessions, nodes, approvals, etc.). The exact surface is defined by the
TypeBox schemas in `src/gateway/protocol/schema.ts`.

## Related

- [Bridge protocol](https://docs.openclaw.ai/gateway/bridge-protocol)
- [Gateway runbook](https://docs.openclaw.ai/gateway)

[Sandbox vs tool policy vs elevated](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated) [Bridge protocol](https://docs.openclaw.ai/gateway/bridge-protocol)

Ctrl+I

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

Gateway credential resolution follows one shared contract across call/probe/status paths and Discord exec-approval monitoring. Node-host uses the same base contract with one local-mode exception (it intentionally ignores `gateway.remote.*`):

- Explicit credentials (`--token`, `--password`, or tool `gatewayToken`) always win on call paths that accept explicit auth.
- URL override safety:
  - CLI URL overrides (`--url`) never reuse implicit config/env credentials.
  - Env URL overrides (`OPENCLAW_GATEWAY_URL`) may use env credentials only (`OPENCLAW_GATEWAY_TOKEN` / `OPENCLAW_GATEWAY_PASSWORD`).
- Local mode defaults:
  - token: `OPENCLAW_GATEWAY_TOKEN` -\> `gateway.auth.token` -\> `gateway.remote.token` (remote fallback applies only when local auth token input is unset)
  - password: `OPENCLAW_GATEWAY_PASSWORD` -\> `gateway.auth.password` -\> `gateway.remote.password` (remote fallback applies only when local auth password input is unset)
- Remote mode defaults:
  - token: `gateway.remote.token` -\> `OPENCLAW_GATEWAY_TOKEN` -\> `gateway.auth.token`
  - password: `OPENCLAW_GATEWAY_PASSWORD` -\> `gateway.remote.password` -\> `gateway.auth.password`
- Node-host local-mode exception: `gateway.remote.token` / `gateway.remote.password` are ignored.
- Remote probe/status token checks are strict by default: they use `gateway.remote.token` only (no local token fallback) when targeting remote mode.
- Gateway env overrides use `OPENCLAW_GATEWAY_*` only.

## Chat UI over SSH

WebChat no longer uses a separate HTTP port. The SwiftUI chat UI connects directly to the Gateway WebSocket.

- Forward `18789` over SSH (see above), then connect clients to `ws://127.0.0.1:18789`.
- On macOS, prefer the app’s “Remote over SSH” mode, which manages the tunnel automatically.

## macOS app Remote over SSH

The macOS menu bar app can drive the same setup end-to-end (remote status checks, WebChat, and Voice Wake forwarding).Runbook: [macOS remote access](https://docs.openclaw.ai/platforms/mac/remote).

## Security rules (remote/VPN)

Short version: **keep the Gateway loopback-only** unless you’re sure you need a bind.

- **Loopback + SSH/Tailscale Serve** is the safest default (no public exposure).
- Plaintext `ws://` is loopback-only by default. For trusted private networks,
set `OPENCLAW_ALLOW_INSECURE_PRIVATE_WS=1` on the client process as
break-glass. There is no `openclaw.json` equivalent; this must be process
environment for the client making the WebSocket connection.
- **Non-loopback binds** (`lan`/`tailnet`/`custom`, or `auto` when loopback is unavailable) must use gateway auth: token, password, or an identity-aware reverse proxy with `gateway.auth.mode: "trusted-proxy"`.
- `gateway.remote.token` / `.password` are client credential sources. They do **not** configure server auth by themselves.
- Local call paths can use `gateway.remote.*` as fallback only when `gateway.auth.*` is unset.
- If `gateway.auth.token` / `gateway.auth.password` is explicitly configured via SecretRef and unresolved, resolution fails closed (no remote fallback masking).
- `gateway.remote.tlsFingerprint` pins the remote TLS cert when using `wss://`.
- **Tailscale Serve** can authenticate Control UI/WebSocket traffic via identity
headers when `gateway.auth.allowTailscale: true`; HTTP API endpoints do not
use that Tailscale header auth and instead follow the gateway’s normal HTTP
auth mode. This tokenless flow assumes the gateway host is trusted. Set it to
`false` if you want shared-secret auth everywhere.
- **Trusted-proxy** auth expects non-loopback identity-aware proxy setups by default.
Same-host loopback reverse proxies require explicit `gateway.auth.trustedProxy.allowLoopback = true`.
- Treat browser control like operator access: tailnet-only + deliberate node pairing.

Deep dive: [Security](https://docs.openclaw.ai/gateway/security).

### macOS: persistent SSH tunnel via LaunchAgent

For macOS clients connecting to a remote gateway, the easiest persistent setup uses an SSH `LocalForward` config entry plus a LaunchAgent to keep the tunnel alive across reboots and crashes.

#### Step 1: add SSH config

Edit `~/.ssh/config`:

```
Host remote-gateway
    HostName <REMOTE_IP>
    User <REMOTE_USER>
    LocalForward 18789 127.0.0.1:18789
    IdentityFile ~/.ssh/id_rsa
```

Replace `<REMOTE_IP>` and `<REMOTE_USER>` with your values.

#### Step 2: copy SSH key (one-time)

```
ssh-copy-id -i ~/.ssh/id_rsa <REMOTE_USER>@<REMOTE_IP>
```

#### Step 3: configure the gateway token

Store the token in config so it persists across restarts:

```
openclaw config set gateway.remote.token "<your-token>"
```

#### Step 4: create the LaunchAgent

Save this as `~/Library/LaunchAgents/ai.openclaw.ssh-tunnel.plist`:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>ai.openclaw.ssh-tunnel</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/ssh</string>
        <string>-N</string>
        <string>remote-gateway</string>
    </array>
    <key>KeepAlive</key>
    <true/>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

#### Step 5: load the LaunchAgent

```
launchctl bootstrap gui/$UID ~/Library/LaunchAgents/ai.openclaw.ssh-tunnel.plist
```

The tunnel will start automatically at login, restart on crash, and keep the forwarded port live.

If you have a leftover `com.openclaw.ssh-tunnel` LaunchAgent from an older setup, unload and delete it.

#### Troubleshooting

Check if the tunnel is running:

```
ps aux | grep "ssh -N remote-gateway" | grep -v grep
lsof -i :18789
```

Restart the tunnel:

```
launchctl kickstart -k gui/$UID/ai.openclaw.ssh-tunnel
```

Stop the tunnel:

```
launchctl bootout gui/$UID/ai.openclaw.ssh-tunnel
```

| Config entry | What it does |
| --- | --- |
| `LocalForward 18789 127.0.0.1:18789` | Forwards local port 18789 to remote port 18789 |
| `ssh -N` | SSH without executing remote commands (port-forwarding only) |
| `KeepAlive` | Automatically restarts the tunnel if it crashes |
| `RunAtLoad` | Starts the tunnel when the LaunchAgent loads at login |

## Related

- [Tailscale](https://docs.openclaw.ai/gateway/tailscale)
- [Authentication](https://docs.openclaw.ai/gateway/authentication)
- [Remote gateway setup](https://docs.openclaw.ai/gateway/remote-gateway-readme)

[Bonjour discovery](https://docs.openclaw.ai/gateway/bonjour) [Remote gateway setup](https://docs.openclaw.ai/gateway/remote-gateway-readme)

Ctrl+I

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

Sandboxing is off by default. If you enable sandboxing and do not choose a backend, OpenClaw uses the Docker backend. It executes tools and sandbox browsers locally via the Docker daemon socket (`/var/run/docker.sock`). Sandbox container isolation is determined by Docker namespaces.To expose host GPUs to Docker sandboxes, set `agents.defaults.sandbox.docker.gpus` or the per-agent `agents.list[].sandbox.docker.gpus` override. The value is passed to Docker’s `--gpus` flag as a separate argument, for example `"all"` or `"device=GPU-uuid"`, and requires a compatible host runtime such as NVIDIA Container Toolkit.

**Docker-out-of-Docker (DooD) constraints**If you deploy the OpenClaw Gateway itself as a Docker container, it orchestrates sibling sandbox containers using the host’s Docker socket (DooD). This introduces a specific path mapping constraint:

- **Config requires host paths**: The `openclaw.json``workspace` configuration MUST contain the **Host’s absolute path** (e.g. `/home/user/.openclaw/workspaces`), not the internal Gateway container path. When OpenClaw asks the Docker daemon to spawn a sandbox, the daemon evaluates paths relative to the Host OS namespace, not the Gateway namespace.
- **FS bridge parity (identical volume map)**: The OpenClaw Gateway native process also writes heartbeat and bridge files to the `workspace` directory. Because the Gateway evaluates the exact same string (the host path) from within its own containerized environment, the Gateway deployment MUST include an identical volume map linking the host namespace natively (`-v /home/user/.openclaw:/home/user/.openclaw`).

If you map paths internally without absolute host parity, OpenClaw natively throws an `EACCES` permission error attempting to write its heartbeat inside the container environment because the fully qualified path string doesn’t exist natively.

### SSH backend

Use `backend: "ssh"` when you want OpenClaw to sandbox `exec`, file tools, and media reads on an arbitrary SSH-accessible machine.

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "all",
        backend: "ssh",
        scope: "session",
        workspaceAccess: "rw",
        ssh: {
          target: "user@gateway-host:22",
          workspaceRoot: "/tmp/openclaw-sandboxes",
          strictHostKeyChecking: true,
          updateHostKeys: true,
          identityFile: "~/.ssh/id_ed25519",
          certificateFile: "~/.ssh/id_ed25519-cert.pub",
          knownHostsFile: "~/.ssh/known_hosts",
          // Or use SecretRefs / inline contents instead of local files:
          // identityData: { source: "env", provider: "default", id: "SSH_IDENTITY" },
          // certificateData: { source: "env", provider: "default", id: "SSH_CERTIFICATE" },
          // knownHostsData: { source: "env", provider: "default", id: "SSH_KNOWN_HOSTS" },
        },
      },
    },
  },
}
```

How it works

- OpenClaw creates a per-scope remote root under `sandbox.ssh.workspaceRoot`.
- On first use after create or recreate, OpenClaw seeds that remote workspace from the local workspace once.
- After that, `exec`, `read`, `write`, `edit`, `apply_patch`, prompt media reads, and inbound media staging run directly against the remote workspace over SSH.
- OpenClaw does not sync remote changes back to the local workspace automatically.

Authentication material

- `identityFile`, `certificateFile`, `knownHostsFile`: use existing local files and pass them through OpenSSH config.
- `identityData`, `certificateData`, `knownHostsData`: use inline strings or SecretRefs. OpenClaw resolves them through the normal secrets runtime snapshot, writes them to temp files with `0600`, and deletes them when the SSH session ends.
- If both `*File` and `*Data` are set for the same item, `*Data` wins for that SSH session.

Remote-canonical consequences

This is a **remote-canonical** model. The remote SSH workspace becomes the real sandbox state after the initial seed.

- Host-local edits made outside OpenClaw after the seed step are not visible remotely until you recreate the sandbox.
- `openclaw sandbox recreate` deletes the per-scope remote root and seeds again from local on next use.
- Browser sandboxing is not supported on the SSH backend.
- `sandbox.docker.*` settings do not apply to the SSH backend.

### OpenShell backend

Use `backend: "openshell"` when you want OpenClaw to sandbox tools in an OpenShell-managed remote environment. For the full setup guide, configuration reference, and workspace mode comparison, see the dedicated [OpenShell page](https://docs.openclaw.ai/gateway/openshell).OpenShell reuses the same core SSH transport and remote filesystem bridge as the generic SSH backend, and adds OpenShell-specific lifecycle (`sandbox create/get/delete`, `sandbox ssh-config`) plus the optional `mirror` workspace mode.

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "all",
        backend: "openshell",
        scope: "session",
        workspaceAccess: "rw",
      },
    },
  },
  plugins: {
    entries: {
      openshell: {
        enabled: true,
        config: {
          from: "openclaw",
          mode: "remote", // mirror | remote
          remoteWorkspaceDir: "/sandbox",
          remoteAgentWorkspaceDir: "/agent",
        },
      },
    },
  },
}
```

OpenShell modes:

- `mirror` (default): local workspace stays canonical. OpenClaw syncs local files into OpenShell before exec and syncs the remote workspace back after exec.
- `remote`: OpenShell workspace is canonical after the sandbox is created. OpenClaw seeds the remote workspace once from the local workspace, then file tools and exec run directly against the remote sandbox without syncing changes back.

Remote transport details

- OpenClaw asks OpenShell for sandbox-specific SSH config via `openshell sandbox ssh-config <name>`.
- Core writes that SSH config to a temp file, opens the SSH session, and reuses the same remote filesystem bridge used by `backend: "ssh"`.
- In `mirror` mode only the lifecycle differs: sync local to remote before exec, then sync back after exec.

Current OpenShell limitations

- sandbox browser is not supported yet
- `sandbox.docker.binds` is not supported on the OpenShell backend
- Docker-specific runtime knobs under `sandbox.docker.*` still apply only to the Docker backend

#### Workspace modes

OpenShell has two workspace models. This is the part that matters most in practice.

- mirror (local canonical)

- remote (OpenShell canonical)

Use `plugins.entries.openshell.config.mode: "mirror"` when you want the **local workspace to stay canonical**.Behavior:

- Before `exec`, OpenClaw syncs the local workspace into the OpenShell sandbox.
- After `exec`, OpenClaw syncs the remote workspace back to the local workspace.
- File tools still operate through the sandbox bridge, but the local workspace remains the source of truth between turns.

Use this when:

- you edit files locally outside OpenClaw and want those changes to show up in the sandbox automatically
- you want the OpenShell sandbox to behave as much like the Docker backend as possible
- you want the host workspace to reflect sandbox writes after each exec turn

Tradeoff: extra sync cost before and after exec.

Use `plugins.entries.openshell.config.mode: "remote"` when you want the **OpenShell workspace to become canonical**.Behavior:

- When the sandbox is first created, OpenClaw seeds the remote workspace from the local workspace once.
- After that, `exec`, `read`, `write`, `edit`, and `apply_patch` operate directly against the remote OpenShell workspace.
- OpenClaw does **not** sync remote changes back into the local workspace after exec.
- Prompt-time media reads still work because file and media tools read through the sandbox bridge instead of assuming a local host path.
- Transport is SSH into the OpenShell sandbox returned by `openshell sandbox ssh-config`.

Important consequences:

- If you edit files on the host outside OpenClaw after the seed step, the remote sandbox will **not** see those changes automatically.
- If the sandbox is recreated, the remote workspace is seeded from the local workspace again.
- With `scope: "agent"` or `scope: "shared"`, that remote workspace is shared at that same scope.

Use this when:

- the sandbox should live primarily on the remote OpenShell side
- you want lower per-turn sync overhead
- you do not want host-local edits to silently overwrite remote sandbox state

Choose `mirror` if you think of the sandbox as a temporary execution environment. Choose `remote` if you think of the sandbox as the real workspace.

#### OpenShell lifecycle

OpenShell sandboxes are still managed through the normal sandbox lifecycle:

- `openclaw sandbox list` shows OpenShell runtimes as well as Docker runtimes
- `openclaw sandbox recreate` deletes the current runtime and lets OpenClaw recreate it on next use
- prune logic is backend-aware too

For `remote` mode, recreate is especially important:

- recreate deletes the canonical remote workspace for that scope
- the next use seeds a fresh remote workspace from the local workspace

For `mirror` mode, recreate mainly resets the remote execution environment because the local workspace remains canonical anyway.

## Workspace access

`agents.defaults.sandbox.workspaceAccess` controls **what the sandbox can see**:

- none (default)

- ro

- rw

Tools see a sandbox workspace under `~/.openclaw/sandboxes`.

Mounts the agent workspace read-only at `/agent` (disables `write`/`edit`/`apply_patch`).

Mounts the agent workspace read/write at `/workspace`.

With the OpenShell backend:

- `mirror` mode still uses the local workspace as the canonical source between exec turns
- `remote` mode uses the remote OpenShell workspace as the canonical source after the initial seed
- `workspaceAccess: "ro"` and `"none"` still restrict write behavior the same way

Inbound media is copied into the active sandbox workspace (`media/inbound/*`).

**Skills note:** the `read` tool is sandbox-rooted. With `workspaceAccess: "none"`, OpenClaw mirrors eligible skills into the sandbox workspace (`.../skills`) so they can be read. With `"rw"`, workspace skills are readable from `/workspace/skills`.

## Custom bind mounts

`agents.defaults.sandbox.docker.binds` mounts additional host directories into the container. Format: `host:container:mode` (e.g., `"/home/user/source:/source:rw"`).Global and per-agent binds are **merged** (not replaced). Under `scope: "shared"`, per-agent binds are ignored.`agents.defaults.sandbox.browser.binds` mounts additional host directories into the **sandbox browser** container only.

- When set (including `[]`), it replaces `agents.defaults.sandbox.docker.binds` for the browser container.
- When omitted, the browser container falls back to `agents.defaults.sandbox.docker.binds` (backwards compatible).

Example (read-only source + an extra data directory):

```
{
  agents: {
    defaults: {
      sandbox: {
        docker: {
          binds: ["/home/user/source:/source:ro", "/var/data/myapp:/data:ro"],
        },
      },
    },
    list: [\
      {\
        id: "build",\
        sandbox: {\
          docker: {\
            binds: ["/mnt/cache:/cache:rw"],\
          },\
        },\
      },\
    ],
  },
}
```

**Bind security**

- Binds bypass the sandbox filesystem: they expose host paths with whatever mode you set (`:ro` or `:rw`).
- OpenClaw blocks dangerous bind sources (for example: `docker.sock`, `/etc`, `/proc`, `/sys`, `/dev`, and parent mounts that would expose them).
- OpenClaw also blocks common home-directory credential roots such as `~/.aws`, `~/.cargo`, `~/.config`, `~/.docker`, `~/.gnupg`, `~/.netrc`, `~/.npm`, and `~/.ssh`.
- Bind validation is not just string matching. OpenClaw normalizes the source path, then resolves it again through the deepest existing ancestor before re-checking blocked paths and allowed roots.
- That means symlink-parent escapes still fail closed even when the final leaf does not exist yet. Example: `/workspace/run-link/new-file` still resolves as `/var/run/...` if `run-link` points there.
- Allowed source roots are canonicalized the same way, so a path that only looks inside the allowlist before symlink resolution is still rejected as `outside allowed roots`.
- Sensitive mounts (secrets, SSH keys, service credentials) should be `:ro` unless absolutely required.
- Combine with `workspaceAccess: "ro"` if you only need read access to the workspace; bind modes stay independent.
- See [Sandbox vs Tool Policy vs Elevated](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated) for how binds interact with tool policy and elevated exec.

## Images and setup

Default Docker image: `openclaw-sandbox:bookworm-slim`

**Source checkout vs npm install**The `scripts/sandbox-setup.sh`, `scripts/sandbox-common-setup.sh`, and `scripts/sandbox-browser-setup.sh` helper scripts are only available when running from a [source checkout](https://github.com/openclaw/openclaw). They are not included in the npm package.If you installed OpenClaw via `npm install -g openclaw`, use the inline `docker build` commands shown below instead.

1

[Navigate to header](https://docs.openclaw.ai/gateway/sandboxing#)

Build the default image

From a source checkout:

```
scripts/sandbox-setup.sh
```

From an npm install (no source checkout needed):

```
docker build -t openclaw-sandbox:bookworm-slim - <<'DOCKERFILE'
FROM debian:bookworm-slim
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
  bash ca-certificates curl git jq python3 ripgrep \
  && rm -rf /var/lib/apt/lists/*
RUN useradd --create-home --shell /bin/bash sandbox
USER sandbox
WORKDIR /home/sandbox
CMD ["sleep", "infinity"]
DOCKERFILE
```

The default image does **not** include Node. If a skill needs Node (or other runtimes), either bake a custom image or install via `sandbox.docker.setupCommand` (requires network egress + writable root + root user).OpenClaw does not silently substitute plain `debian:bookworm-slim` when `openclaw-sandbox:bookworm-slim` is missing. Sandbox runs that target the default image fail fast with a build instruction until you build it, because the bundled image carries `python3` for sandbox write/edit helpers.

2

[Navigate to header](https://docs.openclaw.ai/gateway/sandboxing#)

Optional: build the common image

For a more functional sandbox image with common tooling (for example `curl`, `jq`, `nodejs`, `python3`, `git`):From a source checkout:

```
scripts/sandbox-common-setup.sh
```

From an npm install, build the default image first (see above), then build the common image on top using the [`Dockerfile.sandbox-common`](https://github.com/openclaw/openclaw/blob/main/Dockerfile.sandbox-common) from the repository.Then set `agents.defaults.sandbox.docker.image` to `openclaw-sandbox-common:bookworm-slim`.

3

[Navigate to header](https://docs.openclaw.ai/gateway/sandboxing#)

Optional: build the sandbox browser image

From a source checkout:

```
scripts/sandbox-browser-setup.sh
```

From an npm install, build using the [`Dockerfile.sandbox-browser`](https://github.com/openclaw/openclaw/blob/main/Dockerfile.sandbox-browser) from the repository.

By default, Docker sandbox containers run with **no network**. Override with `agents.defaults.sandbox.docker.network`.

Sandbox browser Chromium defaults

The bundled sandbox browser image also applies conservative Chromium startup defaults for containerized workloads. Current container defaults include:

- `--remote-debugging-address=127.0.0.1`
- `--remote-debugging-port=<derived from OPENCLAW_BROWSER_CDP_PORT>`
- `--user-data-dir=${HOME}/.chrome`
- `--no-first-run`
- `--no-default-browser-check`
- `--disable-3d-apis`
- `--disable-gpu`
- `--disable-dev-shm-usage`
- `--disable-background-networking`
- `--disable-extensions`
- `--disable-features=TranslateUI`
- `--disable-breakpad`
- `--disable-crash-reporter`
- `--disable-software-rasterizer`
- `--no-zygote`
- `--metrics-recording-only`
- `--renderer-process-limit=2`
- `--no-sandbox` when `noSandbox` is enabled.
- The three graphics hardening flags (`--disable-3d-apis`, `--disable-software-rasterizer`, `--disable-gpu`) are optional and are useful when containers lack GPU support. Set `OPENCLAW_BROWSER_DISABLE_GRAPHICS_FLAGS=0` if your workload requires WebGL or other 3D/browser features.
- `--disable-extensions` is enabled by default and can be disabled with `OPENCLAW_BROWSER_DISABLE_EXTENSIONS=0` for extension-reliant flows.
- `--renderer-process-limit=2` is controlled by `OPENCLAW_BROWSER_RENDERER_PROCESS_LIMIT=<N>`, where `0` keeps Chromium’s default.

If you need a different runtime profile, use a custom browser image and provide your own entrypoint. For local (non-container) Chromium profiles, use `browser.extraArgs` to append additional startup flags.

Network security defaults

- `network: "host"` is blocked.
- `network: "container:<id>"` is blocked by default (namespace join bypass risk).
- Break-glass override: `agents.defaults.sandbox.docker.dangerouslyAllowContainerNamespaceJoin: true`.

Docker installs and the containerized gateway live here: [Docker](https://docs.openclaw.ai/install/docker)For Docker gateway deployments, `scripts/docker/setup.sh` can bootstrap sandbox config. Set `OPENCLAW_SANDBOX=1` (or `true`/`yes`/`on`) to enable that path. You can override socket location with `OPENCLAW_DOCKER_SOCKET`. Full setup and env reference: [Docker](https://docs.openclaw.ai/install/docker#agent-sandbox).

## setupCommand (one-time container setup)

`setupCommand` runs **once** after the sandbox container is created (not on every run). It executes inside the container via `sh -lc`.Paths:

- Global: `agents.defaults.sandbox.docker.setupCommand`
- Per-agent: `agents.list[].sandbox.docker.setupCommand`

Common pitfalls

- Default `docker.network` is `"none"` (no egress), so package installs will fail.
- `docker.network: "container:<id>"` requires `dangerouslyAllowContainerNamespaceJoin: true` and is break-glass only.
- `readOnlyRoot: true` prevents writes; set `readOnlyRoot: false` or bake a custom image.
- `user` must be root for package installs (omit `user` or set `user: "0:0"`).
- Sandbox exec does **not** inherit host `process.env`. Use `agents.defaults.sandbox.docker.env` (or a custom image) for skill API keys.

## Tool policy and escape hatches

Tool allow/deny policies still apply before sandbox rules. If a tool is denied globally or per-agent, sandboxing doesn’t bring it back.`tools.elevated` is an explicit escape hatch that runs `exec` outside the sandbox (`gateway` by default, or `node` when the exec target is `node`). `/exec` directives only apply for authorized senders and persist per session; to hard-disable `exec`, use tool policy deny (see [Sandbox vs Tool Policy vs Elevated](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated)).Debugging:

- Use `openclaw sandbox explain` to inspect effective sandbox mode, tool policy, and fix-it config keys.
- See [Sandbox vs Tool Policy vs Elevated](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated) for the “why is this blocked?” mental model.

Keep it locked down.

## Multi-agent overrides

Each agent can override sandbox + tools: `agents.list[].sandbox` and `agents.list[].tools` (plus `agents.list[].tools.sandbox.tools` for sandbox tool policy). See [Multi-Agent Sandbox & Tools](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools) for precedence.

## Minimal enable example

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",
        scope: "session",
        workspaceAccess: "none",
      },
    },
  },
}
```

## Related

- [Multi-Agent Sandbox & Tools](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools) — per-agent overrides and precedence
- [OpenShell](https://docs.openclaw.ai/gateway/openshell) — managed sandbox backend setup, workspace modes, and config reference
- [Sandbox configuration](https://docs.openclaw.ai/gateway/config-agents#agentsdefaultssandbox)
- [Sandbox vs Tool Policy vs Elevated](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated) — debugging “why is this blocked?”
- [Security](https://docs.openclaw.ai/gateway/security)

[Security audit checks](https://docs.openclaw.ai/gateway/security/audit-checks) [OpenShell](https://docs.openclaw.ai/gateway/openshell)

Ctrl+I

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
- Quickstart reuse path: when `gateway.auth.token` is already a SecretRef, onboarding resolves it before probe/dashboard bootstrap (for `env`, `file`, and `exec` refs) using the same fail-fast gate.

If validation fails, onboarding shows the error and lets you retry.

## SecretRef contract

Use one object shape everywhere:

```
{ source: "env" | "file" | "exec", provider: "default", id: "..." }
```

- env

- file

- exec

```
{ source: "env", provider: "default", id: "OPENAI_API_KEY" }
```

Validation:

- `provider` must match `^[a-z][a-z0-9_-]{0,63}$`
- `id` must match `^[A-Z][A-Z0-9_]{0,127}$`

```
{ source: "file", provider: "filemain", id: "/providers/openai/apiKey" }
```

Validation:

- `provider` must match `^[a-z][a-z0-9_-]{0,63}$`
- `id` must be an absolute JSON pointer (`/...`)
- RFC6901 escaping in segments: `~` =\> `~0`, `/` =\> `~1`

```
{ source: "exec", provider: "vault", id: "providers/openai/apiKey" }
```

Validation:

- `provider` must match `^[a-z][a-z0-9_-]{0,63}$`
- `id` must match `^[A-Za-z0-9][A-Za-z0-9._:/-]{0,255}$`
- `id` must not contain `.` or `..` as slash-delimited path segments (for example `a/../b` is rejected)

## Provider config

Define providers under `secrets.providers`:

```
{
  secrets: {
    providers: {
      default: { source: "env" },
      filemain: {
        source: "file",
        path: "~/.openclaw/secrets.json",
        mode: "json", // or "singleValue"
      },
      vault: {
        source: "exec",
        command: "/usr/local/bin/openclaw-vault-resolver",
        args: ["--profile", "prod"],
        passEnv: ["PATH", "VAULT_ADDR"],
        jsonOnly: true,
      },
    },
    defaults: {
      env: "default",
      file: "filemain",
      exec: "vault",
    },
    resolution: {
      maxProviderConcurrency: 4,
      maxRefsPerProvider: 512,
      maxBatchBytes: 262144,
    },
  },
}
```

Env provider

- Optional allowlist via `allowlist`.
- Missing/empty env values fail resolution.

File provider

- Reads local file from `path`.
- `mode: "json"` expects JSON object payload and resolves `id` as pointer.
- `mode: "singleValue"` expects ref id `"value"` and returns file contents.
- Path must pass ownership/permission checks.
- Windows fail-closed note: if ACL verification is unavailable for a path, resolution fails. For trusted paths only, set `allowInsecurePath: true` on that provider to bypass path security checks.

Exec provider

- Runs configured absolute binary path, no shell.
- By default, `command` must point to a regular file (not a symlink).
- Set `allowSymlinkCommand: true` to allow symlink command paths (for example Homebrew shims). OpenClaw validates the resolved target path.
- Pair `allowSymlinkCommand` with `trustedDirs` for package-manager paths (for example `["/opt/homebrew"]`).
- Supports timeout, no-output timeout, output byte limits, env allowlist, and trusted dirs.
- Windows fail-closed note: if ACL verification is unavailable for the command path, resolution fails. For trusted paths only, set `allowInsecurePath: true` on that provider to bypass path security checks.

Request payload (stdin):

```
{ "protocolVersion": 1, "provider": "vault", "ids": ["providers/openai/apiKey"] }
```

Response payload (stdout):

```
{ "protocolVersion": 1, "values": { "providers/openai/apiKey": "<openai-api-key>" } } // pragma: allowlist secret
```

Optional per-id errors:

```
{
  "protocolVersion": 1,
  "values": {},
  "errors": { "providers/openai/apiKey": { "message": "not found" } }
}
```

## Exec integration examples

1Password CLI

```
{
  secrets: {
    providers: {
      onepassword_openai: {
        source: "exec",
        command: "/opt/homebrew/bin/op",
        allowSymlinkCommand: true, // required for Homebrew symlinked binaries
        trustedDirs: ["/opt/homebrew"],
        args: ["read", "op://Personal/OpenClaw QA API Key/password"],
        passEnv: ["HOME"],
        jsonOnly: false,
      },
    },
  },
  models: {
    providers: {
      openai: {
        baseUrl: "https://api.openai.com/v1",
        models: [{ id: "gpt-5", name: "gpt-5" }],
        apiKey: { source: "exec", provider: "onepassword_openai", id: "value" },
      },
    },
  },
}
```

HashiCorp Vault CLI

```
{
  secrets: {
    providers: {
      vault_openai: {
        source: "exec",
        command: "/opt/homebrew/bin/vault",
        allowSymlinkCommand: true, // required for Homebrew symlinked binaries
        trustedDirs: ["/opt/homebrew"],
        args: ["kv", "get", "-field=OPENAI_API_KEY", "secret/openclaw"],
        passEnv: ["VAULT_ADDR", "VAULT_TOKEN"],
        jsonOnly: false,
      },
    },
  },
  models: {
    providers: {
      openai: {
        baseUrl: "https://api.openai.com/v1",
        models: [{ id: "gpt-5", name: "gpt-5" }],
        apiKey: { source: "exec", provider: "vault_openai", id: "value" },
      },
    },
  },
}
```

sops

```
{
  secrets: {
    providers: {
      sops_openai: {
        source: "exec",
        command: "/opt/homebrew/bin/sops",
        allowSymlinkCommand: true, // required for Homebrew symlinked binaries
        trustedDirs: ["/opt/homebrew"],
        args: ["-d", "--extract", '["providers"]["openai"]["apiKey"]', "/path/to/secrets.enc.json"],
        passEnv: ["SOPS_AGE_KEY_FILE"],
        jsonOnly: false,
      },
    },
  },
  models: {
    providers: {
      openai: {
        baseUrl: "https://api.openai.com/v1",
        models: [{ id: "gpt-5", name: "gpt-5" }],
        apiKey: { source: "exec", provider: "sops_openai", id: "value" },
      },
    },
  },
}
```

## MCP server environment variables

MCP server env vars configured via `plugins.entries.acpx.config.mcpServers` support SecretInput. This keeps API keys and tokens out of plaintext config:

```
{
  plugins: {
    entries: {
      acpx: {
        enabled: true,
        config: {
          mcpServers: {
            github: {
              command: "npx",
              args: ["-y", "@modelcontextprotocol/server-github"],
              env: {
                GITHUB_PERSONAL_ACCESS_TOKEN: {
                  source: "env",
                  provider: "default",
                  id: "MCP_GITHUB_PAT",
                },
              },
            },
          },
        },
      },
    },
  },
}
```

Plaintext string values still work. Env-template refs like `${MCP_SERVER_API_KEY}` and SecretRef objects are resolved during gateway activation before the MCP server process is spawned. As with other SecretRef surfaces, unresolved refs only block activation when the `acpx` plugin is effectively active.

## Sandbox SSH auth material

The core `ssh` sandbox backend also supports SecretRefs for SSH auth material:

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "all",
        backend: "ssh",
        ssh: {
          target: "user@gateway-host:22",
          identityData: { source: "env", provider: "default", id: "SSH_IDENTITY" },
          certificateData: { source: "env", provider: "default", id: "SSH_CERTIFICATE" },
          knownHostsData: { source: "env", provider: "default", id: "SSH_KNOWN_HOSTS" },
        },
      },
    },
  },
}
```

Runtime behavior:

- OpenClaw resolves these refs during sandbox activation, not lazily during each SSH call.
- Resolved values are written to temp files with restrictive permissions and used in generated SSH config.
- If the effective sandbox backend is not `ssh`, these refs stay inactive and do not block startup.

## Supported credential surface

Canonical supported and unsupported credentials are listed in:

- [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface)

Runtime-minted or rotating credentials and OAuth refresh material are intentionally excluded from read-only SecretRef resolution.

## Required behavior and precedence

- Field without a ref: unchanged.
- Field with a ref: required on active surfaces during activation.
- If both plaintext and ref are present, ref takes precedence on supported precedence paths.
- The redaction sentinel `__OPENCLAW_REDACTED__` is reserved for internal config redaction/restore and is rejected as literal submitted config data.

Warning and audit signals:

- `SECRETS_REF_OVERRIDES_PLAINTEXT` (runtime warning)
- `REF_SHADOWED` (audit finding when `auth-profiles.json` credentials take precedence over `openclaw.json` refs)

Google Chat compatibility behavior:

- `serviceAccountRef` takes precedence over plaintext `serviceAccount`.
- Plaintext value is ignored when sibling ref is set.

## Activation triggers

Secret activation runs on:

- Startup (preflight plus final activation)
- Config reload hot-apply path
- Config reload restart-check path
- Manual reload via `secrets.reload`
- Gateway config write RPC preflight (`config.set` / `config.apply` / `config.patch`) for active-surface SecretRef resolvability within the submitted config payload before persisting edits

Activation contract:

- Success swaps the snapshot atomically.
- Startup failure aborts gateway startup.
- Runtime reload failure keeps the last-known-good snapshot.
- Write-RPC preflight failure rejects the submitted config and keeps both disk config and active runtime snapshot unchanged.
- Providing an explicit per-call channel token to an outbound helper/tool call does not trigger SecretRef activation; activation points remain startup, reload, and explicit `secrets.reload`.

## Degraded and recovered signals

When reload-time activation fails after a healthy state, OpenClaw enters degraded secrets state.One-shot system event and log codes:

- `SECRETS_RELOADER_DEGRADED`
- `SECRETS_RELOADER_RECOVERED`

Behavior:

- Degraded: runtime keeps last-known-good snapshot.
- Recovered: emitted once after the next successful activation.
- Repeated failures while already degraded log warnings but do not spam events.
- Startup fail-fast does not emit degraded events because runtime never became active.

## Command-path resolution

Command paths can opt into supported SecretRef resolution via gateway snapshot RPC.There are two broad behaviors:

- Strict command paths

- Read-only command paths

For example `openclaw memory` remote-memory paths and `openclaw qr --remote` when it needs remote shared-secret refs. They read from the active snapshot and fail fast when a required SecretRef is unavailable.

For example `openclaw status`, `openclaw status --all`, `openclaw channels status`, `openclaw channels resolve`, `openclaw security audit`, and read-only doctor/config repair flows. They also prefer the active snapshot, but degrade instead of aborting when a targeted SecretRef is unavailable in that command path.Read-only behavior:

- When the gateway is running, these commands read from the active snapshot first.
- If gateway resolution is incomplete or the gateway is unavailable, they attempt targeted local fallback for the specific command surface.
- If a targeted SecretRef is still unavailable, the command continues with degraded read-only output and explicit diagnostics such as “configured but unavailable in this command path”.
- This degraded behavior is command-local only. It does not weaken runtime startup, reload, or send/auth paths.

Other notes:

- Snapshot refresh after backend secret rotation is handled by `openclaw secrets reload`.
- Gateway RPC method used by these command paths: `secrets.resolve`.

## Audit and configure workflow

Default operator flow:

1

[Navigate to header](https://docs.openclaw.ai/gateway/secrets#)

Audit current state

```
openclaw secrets audit --check
```

2

[Navigate to header](https://docs.openclaw.ai/gateway/secrets#)

Configure SecretRefs

```
openclaw secrets configure
```

3

[Navigate to header](https://docs.openclaw.ai/gateway/secrets#)

Re-audit

```
openclaw secrets audit --check
```

secrets audit

Findings include:

- plaintext values at rest (`openclaw.json`, `auth-profiles.json`, `.env`, and generated `agents/*/agent/models.json`)
- plaintext sensitive provider header residues in generated `models.json` entries
- unresolved refs
- precedence shadowing (`auth-profiles.json` taking priority over `openclaw.json` refs)
- legacy residues (`auth.json`, OAuth reminders)

Exec note:

- By default, audit skips exec SecretRef resolvability checks to avoid command side effects.
- Use `openclaw secrets audit --allow-exec` to execute exec providers during audit.

Header residue note:

- Sensitive provider header detection is name-heuristic based (common auth/credential header names and fragments such as `authorization`, `x-api-key`, `token`, `secret`, `password`, and `credential`).

secrets configure

Interactive helper that:

- configures `secrets.providers` first (`env`/`file`/`exec`, add/edit/remove)
- lets you select supported secret-bearing fields in `openclaw.json` plus `auth-profiles.json` for one agent scope
- can create a new `auth-profiles.json` mapping directly in the target picker
- captures SecretRef details (`source`, `provider`, `id`)
- runs preflight resolution
- can apply immediately

Exec note:

- Preflight skips exec SecretRef checks unless `--allow-exec` is set.
- If you apply directly from `configure --apply` and the plan includes exec refs/providers, keep `--allow-exec` set for the apply step too.

Helpful modes:

- `openclaw secrets configure --providers-only`
- `openclaw secrets configure --skip-provider-setup`
- `openclaw secrets configure --agent <id>`

`configure` apply defaults:

- scrub matching static credentials from `auth-profiles.json` for targeted providers
- scrub legacy static `api_key` entries from `auth.json`
- scrub matching known secret lines from `<config-dir>/.env`

secrets apply

Apply a saved plan:

```
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --allow-exec
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run --allow-exec
```

Exec note:

- dry-run skips exec checks unless `--allow-exec` is set.
- write mode rejects plans containing exec SecretRefs/providers unless `--allow-exec` is set.

For strict target/path contract details and exact rejection rules, see [Secrets Apply Plan Contract](https://docs.openclaw.ai/gateway/secrets-plan-contract).

## One-way safety policy

OpenClaw intentionally does not write rollback backups containing historical plaintext secret values.

Safety model:

- preflight must succeed before write mode
- runtime activation is validated before commit
- apply updates files using atomic file replacement and best-effort restore on failure

## Legacy auth compatibility notes

For static credentials, runtime no longer depends on plaintext legacy auth storage.

- Runtime credential source is the resolved in-memory snapshot.
- Legacy static `api_key` entries are scrubbed when discovered.
- OAuth-related compatibility behavior remains separate.

## Web UI note

Some SecretInput unions are easier to configure in raw editor mode than in form mode.

## Related

- [Authentication](https://docs.openclaw.ai/gateway/authentication) — auth setup
- [CLI: secrets](https://docs.openclaw.ai/cli/secrets) — CLI commands
- [Environment Variables](https://docs.openclaw.ai/help/environment) — environment precedence
- [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface) — credential surface
- [Secrets Apply Plan Contract](https://docs.openclaw.ai/gateway/secrets-plan-contract) — plan contract details
- [Security](https://docs.openclaw.ai/gateway/security) — security posture

[Auth credential semantics](https://docs.openclaw.ai/auth-credential-semantics) [Secrets apply plan contract](https://docs.openclaw.ai/gateway/secrets-plan-contract)

Ctrl+I

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
- Approval mode binds exact request context and, when possible, one concrete local script/file operand. If OpenClaw cannot identify exactly one direct local file for an interpreter/runtime command, approval-backed execution is denied rather than promising full semantic coverage.
- For `host=node`, approval-backed runs also store a canonical prepared
`systemRunPlan`; later approved forwards reuse that stored plan, and gateway
validation rejects caller edits to command/cwd/session context after the
approval request was created.
- If you don’t want remote execution, set security to **deny** and remove node pairing for that Mac.

This distinction matters for triage:

- A reconnecting paired node advertising a different command list is not, by itself, a vulnerability if the Gateway global policy and the node’s local exec approvals still enforce the actual execution boundary.
- Reports that treat node pairing metadata as a second hidden per-command approval layer are usually policy/UX confusion, not a security boundary bypass.

## Dynamic skills (watcher / remote nodes)

OpenClaw can refresh the skills list mid-session:

- **Skills watcher**: changes to `SKILL.md` can update the skills snapshot on the next agent turn.
- **Remote nodes**: connecting a macOS node can make macOS-only skills eligible (based on bin probing).

Treat skill folders as **trusted code** and restrict who can modify them.

## The threat model

Your AI assistant can:

- Execute arbitrary shell commands
- Read/write files
- Access network services
- Send messages to anyone (if you give it WhatsApp access)

People who message you can:

- Try to trick your AI into doing bad things
- Social engineer access to your data
- Probe for infrastructure details

## Core concept: access control before intelligence

Most failures here are not fancy exploits — they’re “someone messaged the bot and the bot did what they asked.”OpenClaw’s stance:

- **Identity first:** decide who can talk to the bot (DM pairing / allowlists / explicit “open”).
- **Scope next:** decide where the bot is allowed to act (group allowlists + mention gating, tools, sandboxing, device permissions).
- **Model last:** assume the model can be manipulated; design so manipulation has limited blast radius.

## Command authorization model

Slash commands and directives are only honored for **authorized senders**. Authorization is derived from
channel allowlists/pairing plus `commands.useAccessGroups` (see [Configuration](https://docs.openclaw.ai/gateway/configuration)
and [Slash commands](https://docs.openclaw.ai/tools/slash-commands)). If a channel allowlist is empty or includes `"*"`,
commands are effectively open for that channel.`/exec` is a session-only convenience for authorized operators. It does **not** write config or
change other sessions.

## Control plane tools risk

Two built-in tools can make persistent control-plane changes:

- `gateway` can inspect config with `config.schema.lookup` / `config.get`, and can make persistent changes with `config.apply`, `config.patch`, and `update.run`.
- `cron` can create scheduled jobs that keep running after the original chat/task ends.

The owner-only `gateway` runtime tool still refuses to rewrite
`tools.exec.ask` or `tools.exec.security`; legacy `tools.bash.*` aliases are
normalized to the same protected exec paths before the write.
Agent-driven `gateway config.apply` and `gateway config.patch` edits are
fail-closed by default: only a narrow set of prompt, model, and mention-gating
paths are agent-tunable. New sensitive config trees are therefore protected
unless they are deliberately added to the allowlist.For any agent/surface that handles untrusted content, deny these by default:

```
{
  tools: {
    deny: ["gateway", "cron", "sessions_spawn", "sessions_send"],
  },
}
```

`commands.restart=false` only blocks restart actions. It does not disable `gateway` config/update actions.

## Plugins

Plugins run **in-process** with the Gateway. Treat them as trusted code:

- Only install plugins from sources you trust.
- Prefer explicit `plugins.allow` allowlists.
- Review plugin config before enabling.
- Restart the Gateway after plugin changes.
- If you install or update plugins (`openclaw plugins install <package>`, `openclaw plugins update <id>`), treat it like running untrusted code:

  - The install path is the per-plugin directory under the active plugin install root.
  - OpenClaw runs a built-in dangerous-code scan before install/update. `critical` findings block by default.
  - npm and git plugin installs run package-manager dependency convergence only during the explicit install/update flow. Local paths and archives are treated as self-contained plugin packages; OpenClaw copies/references them without running `npm install`.
  - Prefer pinned, exact versions (`@scope/pkg@1.2.3`), and inspect the unpacked code on disk before enabling.
  - `--dangerously-force-unsafe-install` is break-glass only for built-in scan false positives on plugin install/update flows. It does not bypass plugin `before_install` hook policy blocks and does not bypass scan failures.
  - Gateway-backed skill dependency installs follow the same dangerous/suspicious split: built-in `critical` findings block unless the caller explicitly sets `dangerouslyForceUnsafeInstall`, while suspicious findings still warn only. `openclaw skills install` remains the separate ClawHub skill download/install flow.

Details: [Plugins](https://docs.openclaw.ai/tools/plugin)

## DM access model: pairing, allowlist, open, disabled

All current DM-capable channels support a DM policy (`dmPolicy` or `*.dm.policy`) that gates inbound DMs **before** the message is processed:

- `pairing` (default): unknown senders receive a short pairing code and the bot ignores their message until approved. Codes expire after 1 hour; repeated DMs won’t resend a code until a new request is created. Pending requests are capped at **3 per channel** by default.
- `allowlist`: unknown senders are blocked (no pairing handshake).
- `open`: allow anyone to DM (public). **Requires** the channel allowlist to include `"*"` (explicit opt-in).
- `disabled`: ignore inbound DMs entirely.

Approve via CLI:

```
openclaw pairing list <channel>
openclaw pairing approve <channel> <code>
```

Details + files on disk: [Pairing](https://docs.openclaw.ai/channels/pairing)

## DM session isolation (multi-user mode)

By default, OpenClaw routes **all DMs into the main session** so your assistant has continuity across devices and channels. If **multiple people** can DM the bot (open DMs or a multi-person allowlist), consider isolating DM sessions:

```
{
  session: { dmScope: "per-channel-peer" },
}
```

This prevents cross-user context leakage while keeping group chats isolated.This is a messaging-context boundary, not a host-admin boundary. If users are mutually adversarial and share the same Gateway host/config, run separate gateways per trust boundary instead.

### Secure DM mode (recommended)

Treat the snippet above as **secure DM mode**:

- Default: `session.dmScope: "main"` (all DMs share one session for continuity).
- Local CLI onboarding default: writes `session.dmScope: "per-channel-peer"` when unset (keeps existing explicit values).
- Secure DM mode: `session.dmScope: "per-channel-peer"` (each channel+sender pair gets an isolated DM context).
- Cross-channel peer isolation: `session.dmScope: "per-peer"` (each sender gets one session across all channels of the same type).

If you run multiple accounts on the same channel, use `per-account-channel-peer` instead. If the same person contacts you on multiple channels, use `session.identityLinks` to collapse those DM sessions into one canonical identity. See [Session Management](https://docs.openclaw.ai/concepts/session) and [Configuration](https://docs.openclaw.ai/gateway/configuration).

## Allowlists for DMs and groups

OpenClaw has two separate “who can trigger me?” layers:

- **DM allowlist** (`allowFrom` / `channels.discord.allowFrom` / `channels.slack.allowFrom`; legacy: `channels.discord.dm.allowFrom`, `channels.slack.dm.allowFrom`): who is allowed to talk to the bot in direct messages.

  - When `dmPolicy="pairing"`, approvals are written to the account-scoped pairing allowlist store under `~/.openclaw/credentials/` (`<channel>-allowFrom.json` for default account, `<channel>-<accountId>-allowFrom.json` for non-default accounts), merged with config allowlists.
- **Group allowlist**(channel-specific): which groups/channels/guilds the bot will accept messages from at all.

  - Common patterns:
    - `channels.whatsapp.groups`, `channels.telegram.groups`, `channels.imessage.groups`: per-group defaults like `requireMention`; when set, it also acts as a group allowlist (include `"*"` to keep allow-all behavior).
    - `groupPolicy="allowlist"` \+ `groupAllowFrom`: restrict who can trigger the bot _inside_ a group session (WhatsApp/Telegram/Signal/iMessage/Microsoft Teams).
    - `channels.discord.guilds` / `channels.slack.channels`: per-surface allowlists + mention defaults.
  - Group checks run in this order: `groupPolicy`/group allowlists first, mention/reply activation second.
  - Replying to a bot message (implicit mention) does **not** bypass sender allowlists like `groupAllowFrom`.
  - **Security note:** treat `dmPolicy="open"` and `groupPolicy="open"` as last-resort settings. They should be barely used; prefer pairing + allowlists unless you fully trust every member of the room.

Details: [Configuration](https://docs.openclaw.ai/gateway/configuration) and [Groups](https://docs.openclaw.ai/channels/groups)

## Prompt injection (what it is, why it matters)

Prompt injection is when an attacker crafts a message that manipulates the model into doing something unsafe (“ignore your instructions”, “dump your filesystem”, “follow this link and run commands”, etc.).Even with strong system prompts, **prompt injection is not solved**. System prompt guardrails are soft guidance only; hard enforcement comes from tool policy, exec approvals, sandboxing, and channel allowlists (and operators can disable these by design). What helps in practice:

- Keep inbound DMs locked down (pairing/allowlists).
- Prefer mention gating in groups; avoid “always-on” bots in public rooms.
- Treat links, attachments, and pasted instructions as hostile by default.
- Run sensitive tool execution in a sandbox; keep secrets out of the agent’s reachable filesystem.
- Note: sandboxing is opt-in. If sandbox mode is off, implicit `host=auto` resolves to the gateway host. Explicit `host=sandbox` still fails closed because no sandbox runtime is available. Set `host=gateway` if you want that behavior to be explicit in config.
- Limit high-risk tools (`exec`, `browser`, `web_fetch`, `web_search`) to trusted agents or explicit allowlists.
- If you allowlist interpreters (`python`, `node`, `ruby`, `perl`, `php`, `lua`, `osascript`), enable `tools.exec.strictInlineEval` so inline eval forms still need explicit approval.
- Shell approval analysis also rejects POSIX parameter-expansion forms (`$VAR`, `$?`, `$$`, `$1`, `$@`, `${…}`) inside **unquoted heredocs**, so an allowlisted heredoc body cannot sneak shell expansion past allowlist review as plain text. Quote the heredoc terminator (for example `<<'EOF'`) to opt into literal body semantics; unquoted heredocs that would have expanded variables are rejected.
- **Model choice matters:** older/smaller/legacy models are significantly less robust against prompt injection and tool misuse. For tool-enabled agents, use the strongest latest-generation, instruction-hardened model available.

Red flags to treat as untrusted:

- “Read this file/URL and do exactly what it says.”
- “Ignore your system prompt or safety rules.”
- “Reveal your hidden instructions or tool outputs.”
- “Paste the full contents of ~/.openclaw or your logs.”

## External content special-token sanitization

OpenClaw strips common self-hosted LLM chat-template special-token literals from wrapped external content and metadata before they reach the model. Covered marker families include Qwen/ChatML, Llama, Gemma, Mistral, Phi, and GPT-OSS role/turn tokens.Why:

- OpenAI-compatible backends that front self-hosted models sometimes preserve special tokens that appear in user text, instead of masking them. An attacker who can write into inbound external content (a fetched page, an email body, a file contents tool output) could otherwise inject a synthetic `assistant` or `system` role boundary and escape the wrapped-content guardrails.
- Sanitization happens at the external-content wrapping layer, so it applies uniformly across fetch/read tools and inbound channel content rather than being per-provider.
- Outbound model responses already have a separate sanitizer that strips leaked `<tool_call>`, `<function_calls>`, `<system-reminder>`, `<previous_response>`, and similar internal runtime scaffolding from user-visible replies at the final channel delivery boundary. The external-content sanitizer is the inbound counterpart.

This does not replace the other hardening on this page — `dmPolicy`, allowlists, exec approvals, sandboxing, and `contextVisibility` still do the primary work. It closes one specific tokenizer-layer bypass against self-hosted stacks that forward user text with special tokens intact.

## Unsafe external content bypass flags

OpenClaw includes explicit bypass flags that disable external-content safety wrapping:

- `hooks.mappings[].allowUnsafeExternalContent`
- `hooks.gmail.allowUnsafeExternalContent`
- Cron payload field `allowUnsafeExternalContent`

Guidance:

- Keep these unset/false in production.
- Only enable temporarily for tightly scoped debugging.
- If enabled, isolate that agent (sandbox + minimal tools + dedicated session namespace).

Hooks risk note:

- Hook payloads are untrusted content, even when delivery comes from systems you control (mail/docs/web content can carry prompt injection).
- Weak model tiers increase this risk. For hook-driven automation, prefer strong modern model tiers and keep tool policy tight (`tools.profile: "messaging"` or stricter), plus sandboxing where possible.

### Prompt injection does not require public DMs

Even if **only you** can message the bot, prompt injection can still happen via
any **untrusted content** the bot reads (web search/fetch results, browser pages,
emails, docs, attachments, pasted logs/code). In other words: the sender is not
the only threat surface; the **content itself** can carry adversarial instructions.When tools are enabled, the typical risk is exfiltrating context or triggering
tool calls. Reduce the blast radius by:

- Using a read-only or tool-disabled **reader agent** to summarize untrusted content,
then pass the summary to your main agent.
- Keeping `web_search` / `web_fetch` / `browser` off for tool-enabled agents unless needed.
- For OpenResponses URL inputs (`input_file` / `input_image`), set tight
`gateway.http.endpoints.responses.files.urlAllowlist` and
`gateway.http.endpoints.responses.images.urlAllowlist`, and keep `maxUrlParts` low.
Empty allowlists are treated as unset; use `files.allowUrl: false` / `images.allowUrl: false`
if you want to disable URL fetching entirely.
- For OpenResponses file inputs, decoded `input_file` text is still injected as
**untrusted external content**. Do not rely on file text being trusted just because
the Gateway decoded it locally. The injected block still carries explicit
`<<<EXTERNAL_UNTRUSTED_CONTENT ...>>>` boundary markers plus `Source: External`
metadata, even though this path omits the longer `SECURITY NOTICE:` banner.
- The same marker-based wrapping is applied when media-understanding extracts text
from attached documents before appending that text to the media prompt.
- Enabling sandboxing and strict tool allowlists for any agent that touches untrusted input.
- Keeping secrets out of prompts; pass them via env/config on the gateway host instead.

### Self-hosted LLM backends

OpenAI-compatible self-hosted backends such as vLLM, SGLang, TGI, LM Studio,
or custom Hugging Face tokenizer stacks can differ from hosted providers in how
chat-template special tokens are handled. If a backend tokenizes literal strings
such as `<|im_start|>`, `<|start_header_id|>`, or `<start_of_turn>` as
structural chat-template tokens inside user content, untrusted text can try to
forge role boundaries at the tokenizer layer.OpenClaw strips common model-family special-token literals from wrapped
external content before dispatching it to the model. Keep external-content
wrapping enabled, and prefer backend settings that split or escape special
tokens in user-provided content when available. Hosted providers such as OpenAI
and Anthropic already apply their own request-side sanitization.

### Model strength (security note)

Prompt injection resistance is **not** uniform across model tiers. Smaller/cheaper models are generally more susceptible to tool misuse and instruction hijacking, especially under adversarial prompts.

For tool-enabled agents or agents that read untrusted content, prompt-injection risk with older/smaller models is often too high. Do not run those workloads on weak model tiers.

Recommendations:

- **Use the latest generation, best-tier model** for any bot that can run tools or touch files/networks.
- **Do not use older/weaker/smaller tiers** for tool-enabled agents or untrusted inboxes; the prompt-injection risk is too high.
- If you must use a smaller model, **reduce blast radius** (read-only tools, strong sandboxing, minimal filesystem access, strict allowlists).
- When running small models, **enable sandboxing for all sessions** and **disable web\_search/web\_fetch/browser** unless inputs are tightly controlled.
- For chat-only personal assistants with trusted input and no tools, smaller models are usually fine.

## Reasoning and verbose output in groups

`/reasoning`, `/verbose`, and `/trace` can expose internal reasoning, tool
output, or plugin diagnostics that
was not meant for a public channel. In group settings, treat them as **debug**
**only** and keep them off unless you explicitly need them.Guidance:

- Keep `/reasoning`, `/verbose`, and `/trace` disabled in public rooms.
- If you enable them, do so only in trusted DMs or tightly controlled rooms.
- Remember: verbose and trace output can include tool args, URLs, plugin diagnostics, and data the model saw.

## Configuration hardening examples

### File permissions

Keep config + state private on the gateway host:

- `~/.openclaw/openclaw.json`: `600` (user read/write only)
- `~/.openclaw`: `700` (user only)

`openclaw doctor` can warn and offer to tighten these permissions.

### Network exposure (bind, port, firewall)

The Gateway multiplexes **WebSocket + HTTP** on a single port:

- Default: `18789`
- Config/flags/env: `gateway.port`, `--port`, `OPENCLAW_GATEWAY_PORT`

This HTTP surface includes the Control UI and the canvas host:

- Control UI (SPA assets) (default base path `/`)
- Canvas host: `/__openclaw__/canvas/` and `/__openclaw__/a2ui/` (arbitrary HTML/JS; treat as untrusted content)

If you load canvas content in a normal browser, treat it like any other untrusted web page:

- Don’t expose the canvas host to untrusted networks/users.
- Don’t make canvas content share the same origin as privileged web surfaces unless you fully understand the implications.

Bind mode controls where the Gateway listens:

- `gateway.bind: "loopback"` (default): only local clients can connect.
- Non-loopback binds (`"lan"`, `"tailnet"`, `"custom"`) expand the attack surface. Only use them with gateway auth (shared token/password or a correctly configured trusted proxy) and a real firewall.

Rules of thumb:

- Prefer Tailscale Serve over LAN binds (Serve keeps the Gateway on loopback, and Tailscale handles access).
- If you must bind to LAN, firewall the port to a tight allowlist of source IPs; do not port-forward it broadly.
- Never expose the Gateway unauthenticated on `0.0.0.0`.

### Docker port publishing with UFW

If you run OpenClaw with Docker on a VPS, remember that published container ports
(`-p HOST:CONTAINER` or Compose `ports:`) are routed through Docker’s forwarding
chains, not only host `INPUT` rules.To keep Docker traffic aligned with your firewall policy, enforce rules in
`DOCKER-USER` (this chain is evaluated before Docker’s own accept rules).
On many modern distros, `iptables`/`ip6tables` use the `iptables-nft` frontend
and still apply these rules to the nftables backend.Minimal allowlist example (IPv4):

```
# /etc/ufw/after.rules (append as its own *filter section)
*filter
:DOCKER-USER - [0:0]
-A DOCKER-USER -m conntrack --ctstate ESTABLISHED,RELATED -j RETURN
-A DOCKER-USER -s 127.0.0.0/8 -j RETURN
-A DOCKER-USER -s 10.0.0.0/8 -j RETURN
-A DOCKER-USER -s 172.16.0.0/12 -j RETURN
-A DOCKER-USER -s 192.168.0.0/16 -j RETURN
-A DOCKER-USER -s 100.64.0.0/10 -j RETURN
-A DOCKER-USER -p tcp --dport 80 -j RETURN
-A DOCKER-USER -p tcp --dport 443 -j RETURN
-A DOCKER-USER -m conntrack --ctstate NEW -j DROP
-A DOCKER-USER -j RETURN
COMMIT
```

IPv6 has separate tables. Add a matching policy in `/etc/ufw/after6.rules` if
Docker IPv6 is enabled.Avoid hardcoding interface names like `eth0` in docs snippets. Interface names
vary across VPS images (`ens3`, `enp*`, etc.) and mismatches can accidentally
skip your deny rule.Quick validation after reload:

```
ufw reload
iptables -S DOCKER-USER
ip6tables -S DOCKER-USER
nmap -sT -p 1-65535 <public-ip> --open
```

Expected external ports should be only what you intentionally expose (for most
setups: SSH + your reverse proxy ports).

### mDNS/Bonjour discovery

The Gateway broadcasts its presence via mDNS (`_openclaw-gw._tcp` on port 5353) for local device discovery. In full mode, this includes TXT records that may expose operational details:

- `cliPath`: full filesystem path to the CLI binary (reveals username and install location)
- `sshPort`: advertises SSH availability on the host
- `displayName`, `lanHost`: hostname information

**Operational security consideration:** Broadcasting infrastructure details makes reconnaissance easier for anyone on the local network. Even “harmless” info like filesystem paths and SSH availability helps attackers map your environment.**Recommendations:**

1. **Minimal mode** (default, recommended for exposed gateways): omit sensitive fields from mDNS broadcasts:

```
{
     discovery: {
       mdns: { mode: "minimal" },
     },
}
```

2. **Disable entirely** if you don’t need local device discovery:

```
{
     discovery: {
       mdns: { mode: "off" },
     },
}
```

3. **Full mode** (opt-in): include `cliPath` \+ `sshPort` in TXT records:

```
{
     discovery: {
       mdns: { mode: "full" },
     },
}
```

4. **Environment variable** (alternative): set `OPENCLAW_DISABLE_BONJOUR=1` to disable mDNS without config changes.

In minimal mode, the Gateway still broadcasts enough for device discovery (`role`, `gatewayPort`, `transport`) but omits `cliPath` and `sshPort`. Apps that need CLI path information can fetch it via the authenticated WebSocket connection instead.

### Lock down the Gateway WebSocket (local auth)

Gateway auth is **required by default**. If no valid gateway auth path is configured,
the Gateway refuses WebSocket connections (fail‑closed).Onboarding generates a token by default (even for loopback) so
local clients must authenticate.Set a token so **all** WS clients must authenticate:

```
{
  gateway: {
    auth: { mode: "token", token: "your-token" },
  },
}
```

Doctor can generate one for you: `openclaw doctor --generate-gateway-token`.

`gateway.remote.token` and `gateway.remote.password` are client credential sources. They do **not** protect local WS access by themselves. Local call paths can use `gateway.remote.*` as fallback only when `gateway.auth.*` is unset. If `gateway.auth.token` or `gateway.auth.password` is explicitly configured via SecretRef and unresolved, resolution fails closed (no remote fallback masking).

Optional: pin remote TLS with `gateway.remote.tlsFingerprint` when using `wss://`.
Plaintext `ws://` is loopback-only by default. For trusted private-network
paths, set `OPENCLAW_ALLOW_INSECURE_PRIVATE_WS=1` on the client process as
break-glass. This is intentionally process environment only, not an
`openclaw.json` config key.
Mobile pairing and Android manual or scanned gateway routes are stricter:
cleartext is accepted for loopback, but private-LAN, link-local, `.local`, and
dotless hostnames must use TLS unless you explicitly opt into the trusted
private-network cleartext path.Local device pairing:

- Device pairing is auto-approved for direct local loopback connects to keep
same-host clients smooth.
- OpenClaw also has a narrow backend/container-local self-connect path for
trusted shared-secret helper flows.
- Tailnet and LAN connects, including same-host tailnet binds, are treated as
remote for pairing and still need approval.
- Forwarded-header evidence on a loopback request disqualifies loopback
locality. Metadata-upgrade auto-approval is scoped narrowly. See
[Gateway pairing](https://docs.openclaw.ai/gateway/pairing) for both rules.

Auth modes:

- `gateway.auth.mode: "token"`: shared bearer token (recommended for most setups).
- `gateway.auth.mode: "password"`: password auth (prefer setting via env: `OPENCLAW_GATEWAY_PASSWORD`).
- `gateway.auth.mode: "trusted-proxy"`: trust an identity-aware reverse proxy to authenticate users and pass identity via headers (see [Trusted Proxy Auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth)).

Rotation checklist (token/password):

1. Generate/set a new secret (`gateway.auth.token` or `OPENCLAW_GATEWAY_PASSWORD`).
2. Restart the Gateway (or restart the macOS app if it supervises the Gateway).
3. Update any remote clients (`gateway.remote.token` / `.password` on machines that call into the Gateway).
4. Verify you can no longer connect with the old credentials.

### Tailscale Serve identity headers

When `gateway.auth.allowTailscale` is `true` (default for Serve), OpenClaw
accepts Tailscale Serve identity headers (`tailscale-user-login`) for Control
UI/WebSocket authentication. OpenClaw verifies the identity by resolving the
`x-forwarded-for` address through the local Tailscale daemon (`tailscale whois`)
and matching it to the header. This only triggers for requests that hit loopback
and include `x-forwarded-for`, `x-forwarded-proto`, and `x-forwarded-host` as
injected by Tailscale.
For this async identity check path, failed attempts for the same `{scope, ip}`
are serialized before the limiter records the failure. Concurrent bad retries
from one Serve client can therefore lock out the second attempt immediately
instead of racing through as two plain mismatches.
HTTP API endpoints (for example `/v1/*`, `/tools/invoke`, and `/api/channels/*`)
do **not** use Tailscale identity-header auth. They still follow the gateway’s
configured HTTP auth mode.Important boundary note:

- Gateway HTTP bearer auth is effectively all-or-nothing operator access.
- Treat credentials that can call `/v1/chat/completions`, `/v1/responses`, or `/api/channels/*` as full-access operator secrets for that gateway.
- On the OpenAI-compatible HTTP surface, shared-secret bearer auth restores the full default operator scopes (`operator.admin`, `operator.approvals`, `operator.pairing`, `operator.read`, `operator.talk.secrets`, `operator.write`) and owner semantics for agent turns; narrower `x-openclaw-scopes` values do not reduce that shared-secret path.
- Per-request scope semantics on HTTP only apply when the request comes from an identity-bearing mode such as trusted proxy auth or `gateway.auth.mode="none"` on a private ingress.
- In those identity-bearing modes, omitting `x-openclaw-scopes` falls back to the normal operator default scope set; send the header explicitly when you want a narrower scope set.
- `/tools/invoke` follows the same shared-secret rule: token/password bearer auth is treated as full operator access there too, while identity-bearing modes still honor declared scopes.
- Do not share these credentials with untrusted callers; prefer separate gateways per trust boundary.

**Trust assumption:** tokenless Serve auth assumes the gateway host is trusted.
Do not treat this as protection against hostile same-host processes. If untrusted
local code may run on the gateway host, disable `gateway.auth.allowTailscale`
and require explicit shared-secret auth with `gateway.auth.mode: "token"` or
`"password"`.**Security rule:** do not forward these headers from your own reverse proxy. If
you terminate TLS or proxy in front of the gateway, disable
`gateway.auth.allowTailscale` and use shared-secret auth (`gateway.auth.mode: "token"` or `"password"`) or [Trusted Proxy Auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth)
instead.Trusted proxies:

- If you terminate TLS in front of the Gateway, set `gateway.trustedProxies` to your proxy IPs.
- OpenClaw will trust `x-forwarded-for` (or `x-real-ip`) from those IPs to determine the client IP for local pairing checks and HTTP auth/local checks.
- Ensure your proxy **overwrites**`x-forwarded-for` and blocks direct access to the Gateway port.

See [Tailscale](https://docs.openclaw.ai/gateway/tailscale) and [Web overview](https://docs.openclaw.ai/web).

### Browser control via node host (recommended)

If your Gateway is remote but the browser runs on another machine, run a **node host**
on the browser machine and let the Gateway proxy browser actions (see [Browser tool](https://docs.openclaw.ai/tools/browser)).
Treat node pairing like admin access.Recommended pattern:

- Keep the Gateway and node host on the same tailnet (Tailscale).
- Pair the node intentionally; disable browser proxy routing if you don’t need it.

Avoid:

- Exposing relay/control ports over LAN or public Internet.
- Tailscale Funnel for browser control endpoints (public exposure).

### Secrets on disk

Assume anything under `~/.openclaw/` (or `$OPENCLAW_STATE_DIR/`) may contain secrets or private data:

- `openclaw.json`: config may include tokens (gateway, remote gateway), provider settings, and allowlists.
- `credentials/**`: channel credentials (example: WhatsApp creds), pairing allowlists, legacy OAuth imports.
- `agents/<agentId>/agent/auth-profiles.json`: API keys, token profiles, OAuth tokens, and optional `keyRef`/`tokenRef`.
- `agents/<agentId>/agent/codex-home/**`: per-agent Codex app-server account, config, skills, plugins, native thread state, and diagnostics.
- `secrets.json` (optional): file-backed secret payload used by `file` SecretRef providers (`secrets.providers`).
- `agents/<agentId>/agent/auth.json`: legacy compatibility file. Static `api_key` entries are scrubbed when discovered.
- `agents/<agentId>/sessions/**`: session transcripts (`*.jsonl`) \+ routing metadata (`sessions.json`) that can contain private messages and tool output.
- bundled plugin packages: installed plugins (plus their `node_modules/`).
- `sandboxes/**`: tool sandbox workspaces; can accumulate copies of files you read/write inside the sandbox.

Hardening tips:

- Keep permissions tight (`700` on dirs, `600` on files).
- Use full-disk encryption on the gateway host.
- Prefer a dedicated OS user account for the Gateway if the host is shared.

### Workspace `.env` files

OpenClaw loads workspace-local `.env` files for agents and tools, but never lets those files silently override gateway runtime controls.

- Any key that starts with `OPENCLAW_*` is blocked from untrusted workspace `.env` files.
- Channel endpoint settings for Matrix, Mattermost, IRC, and Synology Chat are also blocked from workspace `.env` overrides, so cloned workspaces cannot redirect bundled connector traffic through local endpoint config. Endpoint env keys (such as `MATRIX_HOMESERVER`, `MATTERMOST_URL`, `IRC_HOST`, `SYNOLOGY_CHAT_INCOMING_URL`) must come from the gateway process environment or `env.shellEnv`, not from a workspace-loaded `.env`.
- The block is fail-closed: a new runtime-control variable added in a future release cannot be inherited from a checked-in or attacker-supplied `.env`; the key is ignored and the gateway keeps its own value.
- Trusted process/OS environment variables (the gateway’s own shell, launchd/systemd unit, app bundle) still apply — this only constrains `.env` file loading.

Why: workspace `.env` files frequently live next to agent code, get committed by accident, or get written by tools. Blocking the whole `OPENCLAW_*` prefix means adding a new `OPENCLAW_*` flag later can never regress into silent inheritance from workspace state.

### Logs and transcripts (redaction and retention)

Logs and transcripts can leak sensitive info even when access controls are correct:

- Gateway logs may include tool summaries, errors, and URLs.
- Session transcripts can include pasted secrets, file contents, command output, and links.

Recommendations:

- Keep log and transcript redaction on (`logging.redactSensitive: "tools"`; default).
- Add custom patterns for your environment via `logging.redactPatterns` (tokens, hostnames, internal URLs).
- When sharing diagnostics, prefer `openclaw status --all` (pasteable, secrets redacted) over raw logs.
- Prune old session transcripts and log files if you don’t need long retention.

Details: [Logging](https://docs.openclaw.ai/gateway/logging)

### DMs: pairing by default

```
{
  channels: { whatsapp: { dmPolicy: "pairing" } },
}
```

### Groups: require mention everywhere

```
{
  "channels": {
    "whatsapp": {
      "groups": {
        "*": { "requireMention": true }
      }
    }
  },
  "agents": {
    "list": [\
      {\
        "id": "main",\
        "groupChat": { "mentionPatterns": ["@openclaw", "@mybot"] }\
      }\
    ]
  }
}
```

In group chats, only respond when explicitly mentioned.

### Separate numbers (WhatsApp, Signal, Telegram)

For phone-number-based channels, consider running your AI on a separate phone number from your personal one:

- Personal number: Your conversations stay private
- Bot number: AI handles these, with appropriate boundaries

### Read-only mode (via sandbox and tools)

You can build a read-only profile by combining:

- `agents.defaults.sandbox.workspaceAccess: "ro"` (or `"none"` for no workspace access)
- tool allow/deny lists that block `write`, `edit`, `apply_patch`, `exec`, `process`, etc.

Additional hardening options:

- `tools.exec.applyPatch.workspaceOnly: true` (default): ensures `apply_patch` cannot write/delete outside the workspace directory even when sandboxing is off. Set to `false` only if you intentionally want `apply_patch` to touch files outside the workspace.
- `tools.fs.workspaceOnly: true` (optional): restricts `read`/`write`/`edit`/`apply_patch` paths and native prompt image auto-load paths to the workspace directory (useful if you allow absolute paths today and want a single guardrail).
- Keep filesystem roots narrow: avoid broad roots like your home directory for agent workspaces/sandbox workspaces. Broad roots can expose sensitive local files (for example state/config under `~/.openclaw`) to filesystem tools.

### Secure baseline (copy/paste)

One “safe default” config that keeps the Gateway private, requires DM pairing, and avoids always-on group bots:

```
{
  gateway: {
    mode: "local",
    bind: "loopback",
    port: 18789,
    auth: { mode: "token", token: "your-long-random-token" },
  },
  channels: {
    whatsapp: {
      dmPolicy: "pairing",
      groups: { "*": { requireMention: true } },
    },
  },
}
```

If you want “safer by default” tool execution too, add a sandbox + deny dangerous tools for any non-owner agent (example below under “Per-agent access profiles”).Built-in baseline for chat-driven agent turns: non-owner senders cannot use the `cron` or `gateway` tools.

## Sandboxing (recommended)

Dedicated doc: [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing)Two complementary approaches:

- **Run the full Gateway in Docker** (container boundary): [Docker](https://docs.openclaw.ai/install/docker)
- **Tool sandbox** (`agents.defaults.sandbox`, host gateway + sandbox-isolated tools; Docker is the default backend): [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing)

To prevent cross-agent access, keep `agents.defaults.sandbox.scope` at `"agent"` (default) or `"session"` for stricter per-session isolation. `scope: "shared"` uses a single container or workspace.

Also consider agent workspace access inside the sandbox:

- `agents.defaults.sandbox.workspaceAccess: "none"` (default) keeps the agent workspace off-limits; tools run against a sandbox workspace under `~/.openclaw/sandboxes`
- `agents.defaults.sandbox.workspaceAccess: "ro"` mounts the agent workspace read-only at `/agent` (disables `write`/`edit`/`apply_patch`)
- `agents.defaults.sandbox.workspaceAccess: "rw"` mounts the agent workspace read/write at `/workspace`
- Extra `sandbox.docker.binds` are validated against normalized and canonicalized source paths. Parent-symlink tricks and canonical home aliases still fail closed if they resolve into blocked roots such as `/etc`, `/var/run`, or credential directories under the OS home.

`tools.elevated` is the global baseline escape hatch that runs exec outside the sandbox. The effective host is `gateway` by default, or `node` when the exec target is configured to `node`. Keep `tools.elevated.allowFrom` tight and do not enable it for strangers. You can further restrict elevated per agent via `agents.list[].tools.elevated`. See [Elevated mode](https://docs.openclaw.ai/tools/elevated).

### Sub-agent delegation guardrail

If you allow session tools, treat delegated sub-agent runs as another boundary decision:

- Deny `sessions_spawn` unless the agent truly needs delegation.
- Keep `agents.defaults.subagents.allowAgents` and any per-agent `agents.list[].subagents.allowAgents` overrides restricted to known-safe target agents.
- For any workflow that must remain sandboxed, call `sessions_spawn` with `sandbox: "require"` (default is `inherit`).
- `sandbox: "require"` fails fast when the target child runtime is not sandboxed.

## Browser control risks

Enabling browser control gives the model the ability to drive a real browser.
If that browser profile already contains logged-in sessions, the model can
access those accounts and data. Treat browser profiles as **sensitive state**:

- Prefer a dedicated profile for the agent (the default `openclaw` profile).
- Avoid pointing the agent at your personal daily-driver profile.
- Keep host browser control disabled for sandboxed agents unless you trust them.
- The standalone loopback browser control API only honors shared-secret auth
(gateway token bearer auth or gateway password). It does not consume
trusted-proxy or Tailscale Serve identity headers.
- Treat browser downloads as untrusted input; prefer an isolated downloads directory.
- Disable browser sync/password managers in the agent profile if possible (reduces blast radius).
- For remote gateways, assume “browser control” is equivalent to “operator access” to whatever that profile can reach.
- Keep the Gateway and node hosts tailnet-only; avoid exposing browser control ports to LAN or public Internet.
- Disable browser proxy routing when you don’t need it (`gateway.nodes.browser.mode="off"`).
- Chrome MCP existing-session mode is **not** “safer”; it can act as you in whatever that host Chrome profile can reach.

### Browser SSRF policy (strict by default)

OpenClaw’s browser navigation policy is strict by default: private/internal destinations stay blocked unless you explicitly opt in.

- Default: `browser.ssrfPolicy.dangerouslyAllowPrivateNetwork` is unset, so browser navigation keeps private/internal/special-use destinations blocked.
- Legacy alias: `browser.ssrfPolicy.allowPrivateNetwork` is still accepted for compatibility.
- Opt-in mode: set `browser.ssrfPolicy.dangerouslyAllowPrivateNetwork: true` to allow private/internal/special-use destinations.
- In strict mode, use `hostnameAllowlist` (patterns like `*.example.com`) and `allowedHostnames` (exact host exceptions, including blocked names like `localhost`) for explicit exceptions.
- Navigation is checked before request and best-effort re-checked on the final `http(s)` URL after navigation to reduce redirect-based pivots.

Example strict policy:

```
{
  browser: {
    ssrfPolicy: {
      dangerouslyAllowPrivateNetwork: false,
      hostnameAllowlist: ["*.example.com", "example.com"],
      allowedHostnames: ["localhost"],
    },
  },
}
```

## Per-agent access profiles (multi-agent)

With multi-agent routing, each agent can have its own sandbox + tool policy:
use this to give **full access**, **read-only**, or **no access** per agent.
See [Multi-Agent Sandbox & Tools](https://docs.openclaw.ai/tools/multi-agent-sandbox-tools) for full details
and precedence rules.Common use cases:

- Personal agent: full access, no sandbox
- Family/work agent: sandboxed + read-only tools
- Public agent: sandboxed + no filesystem/shell tools

### Example: full access (no sandbox)

```
{
  agents: {
    list: [\
      {\
        id: "personal",\
        workspace: "~/.openclaw/workspace-personal",\
        sandbox: { mode: "off" },\
      },\
    ],
  },
}
```

### Example: read-only tools + read-only workspace

```
{
  agents: {
    list: [\
      {\
        id: "family",\
        workspace: "~/.openclaw/workspace-family",\
        sandbox: {\
          mode: "all",\
          scope: "agent",\
          workspaceAccess: "ro",\
        },\
        tools: {\
          allow: ["read"],\
          deny: ["write", "edit", "apply_patch", "exec", "process", "browser"],\
        },\
      },\
    ],
  },
}
```

### Example: no filesystem/shell access (provider messaging allowed)

```
{
  agents: {
    list: [\
      {\
        id: "public",\
        workspace: "~/.openclaw/workspace-public",\
        sandbox: {\
          mode: "all",\
          scope: "agent",\
          workspaceAccess: "none",\
        },\
        // Session tools can reveal sensitive data from transcripts. By default OpenClaw limits these tools\
        // to the current session + spawned subagent sessions, but you can clamp further if needed.\
        // See `tools.sessions.visibility` in the configuration reference.\
        tools: {\
          sessions: { visibility: "tree" }, // self | tree | agent | all\
          allow: [\
            "sessions_list",\
            "sessions_history",\
            "sessions_send",\
            "sessions_spawn",\
            "session_status",\
            "whatsapp",\
            "telegram",\
            "slack",\
            "discord",\
          ],\
          deny: [\
            "read",\
            "write",\
            "edit",\
            "apply_patch",\
            "exec",\
            "process",\
            "browser",\
            "canvas",\
            "nodes",\
            "cron",\
            "gateway",\
            "image",\
          ],\
        },\
      },\
    ],
  },
}
```

## Incident response

If your AI does something bad:

### Contain

1. **Stop it:** stop the macOS app (if it supervises the Gateway) or terminate your `openclaw gateway` process.
2. **Close exposure:** set `gateway.bind: "loopback"` (or disable Tailscale Funnel/Serve) until you understand what happened.
3. **Freeze access:** switch risky DMs/groups to `dmPolicy: "disabled"` / require mentions, and remove `"*"` allow-all entries if you had them.

### Rotate (assume compromise if secrets leaked)

1. Rotate Gateway auth (`gateway.auth.token` / `OPENCLAW_GATEWAY_PASSWORD`) and restart.
2. Rotate remote client secrets (`gateway.remote.token` / `.password`) on any machine that can call the Gateway.
3. Rotate provider/API credentials (WhatsApp creds, Slack/Discord tokens, model/API keys in `auth-profiles.json`, and encrypted secrets payload values when used).

### Audit

1. Check Gateway logs: `/tmp/openclaw/openclaw-YYYY-MM-DD.log` (or `logging.file`).
2. Review the relevant transcript(s): `~/.openclaw/agents/<agentId>/sessions/*.jsonl`.
3. Review recent config changes (anything that could have widened access: `gateway.bind`, `gateway.auth`, dm/group policies, `tools.elevated`, plugin changes).
4. Re-run `openclaw security audit --deep` and confirm critical findings are resolved.

### Collect for a report

- Timestamp, gateway host OS + OpenClaw version
- The session transcript(s) + a short log tail (after redacting)
- What the attacker sent + what the agent did
- Whether the Gateway was exposed beyond loopback (LAN/Tailscale Funnel/Serve)

## Secret scanning

CI runs the pre-commit `detect-private-key` hook over the repository. If it
fails, remove or rotate the committed key material, then reproduce locally:

```
pre-commit run --all-files detect-private-key
```

## Reporting security issues

Found a vulnerability in OpenClaw? Please report responsibly:

1. Email: [security@openclaw.ai](mailto:security@openclaw.ai)
2. Don’t post publicly until fixed
3. We’ll credit you (unless you prefer anonymity)

[Multiple gateways](https://docs.openclaw.ai/gateway/multiple-gateways) [Security audit checks](https://docs.openclaw.ai/gateway/security/audit-checks)

Ctrl+I

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
- `gateway.bind: "tailnet"` is a direct Tailnet bind (no HTTPS, no Serve/Funnel).
- `gateway.bind: "auto"` prefers loopback; use `tailnet` if you want Tailnet-only.
- Serve/Funnel only expose the **Gateway control UI + WS**. Nodes connect over
the same Gateway WS endpoint, so Serve can work for node access.

## Browser control (remote Gateway + local browser)

If you run the Gateway on one machine but want to drive a browser on another machine,
run a **node host** on the browser machine and keep both on the same tailnet.
The Gateway will proxy browser actions to the node; no separate control server or Serve URL needed.Avoid Funnel for browser control; treat node pairing like operator access.

## Tailscale prerequisites + limits

- Serve requires HTTPS enabled for your tailnet; the CLI prompts if it is missing.
- Serve injects Tailscale identity headers; Funnel does not.
- Funnel requires Tailscale v1.38.3+, MagicDNS, HTTPS enabled, and a funnel node attribute.
- Funnel only supports ports `443`, `8443`, and `10000` over TLS.
- Funnel on macOS requires the open-source Tailscale app variant.

## Learn more

- Tailscale Serve overview: [https://tailscale.com/kb/1312/serve](https://tailscale.com/kb/1312/serve)
- `tailscale serve` command: [https://tailscale.com/kb/1242/tailscale-serve](https://tailscale.com/kb/1242/tailscale-serve)
- Tailscale Funnel overview: [https://tailscale.com/kb/1223/tailscale-funnel](https://tailscale.com/kb/1223/tailscale-funnel)
- `tailscale funnel` command: [https://tailscale.com/kb/1311/tailscale-funnel](https://tailscale.com/kb/1311/tailscale-funnel)

## Related

- [Remote access](https://docs.openclaw.ai/gateway/remote)
- [Discovery](https://docs.openclaw.ai/gateway/discovery)
- [Authentication](https://docs.openclaw.ai/gateway/authentication)

[Remote gateway setup](https://docs.openclaw.ai/gateway/remote-gateway-readme) [Network proxy](https://docs.openclaw.ai/security/network-proxy)

Ctrl+I

---

## https://docs.openclaw.ai/gateway/tools-invoke-http-api.md

_Source: <https://docs.openclaw.ai/gateway/tools-invoke-http-api.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Tools invoke API

\# Tools Invoke (HTTP)

OpenClaw’s Gateway exposes a simple HTTP endpoint for invoking a single tool directly. It is always enabled and uses Gateway auth plus tool policy. Like the OpenAI-compatible \`/v1/\*\` surface, shared-secret bearer auth is treated as trusted operator access for the whole gateway.

\\* \`POST /tools/invoke\`
\\* Same port as the Gateway (WS + HTTP multiplex): \`http://:/tools/invoke\`

Default max payload size is 2 MB.

\## Authentication

Uses the Gateway auth configuration.

Common HTTP auth paths:

\\* shared-secret auth (\`gateway.auth.mode="token"\` or \`"password"\`):
 \`Authorization: Bearer \`
\\* trusted identity-bearing HTTP auth (\`gateway.auth.mode="trusted-proxy"\`):
 route through the configured identity-aware proxy and let it inject the
 required identity headers
\\* private-ingress open auth (\`gateway.auth.mode="none"\`):
 no auth header required

Notes:

\\* When \`gateway.auth.mode="token"\`, use \`gateway.auth.token\` (or \`OPENCLAW\_GATEWAY\_TOKEN\`).
\\* When \`gateway.auth.mode="password"\`, use \`gateway.auth.password\` (or \`OPENCLAW\_GATEWAY\_PASSWORD\`).
\\* When \`gateway.auth.mode="trusted-proxy"\`, the HTTP request must come from a
 configured trusted proxy source; same-host loopback proxies require explicit
 \`gateway.auth.trustedProxy.allowLoopback = true\`.
\\* If \`gateway.auth.rateLimit\` is configured and too many auth failures occur, the endpoint returns \`429\` with \`Retry-After\`.

\## Security boundary (important)

Treat this endpoint as a \*\*full operator-access\*\* surface for the gateway instance.

\\* HTTP bearer auth here is not a narrow per-user scope model.
\\* A valid Gateway token/password for this endpoint should be treated like an owner/operator credential.
\\* For shared-secret auth modes (\`token\` and \`password\`), the endpoint restores the normal full operator defaults even if the caller sends a narrower \`x-openclaw-scopes\` header.
\\* Shared-secret auth also treats direct tool invokes on this endpoint as owner-sender turns.
\\* Trusted identity-bearing HTTP modes (for example trusted proxy auth or \`gateway.auth.mode="none"\` on a private ingress) honor \`x-openclaw-scopes\` when present and otherwise fall back to the normal operator default scope set.
\\* Keep this endpoint on loopback/tailnet/private ingress only; do not expose it directly to the public internet.

Auth matrix:

\\* \`gateway.auth.mode="token"\` or \`"password"\` + \`Authorization: Bearer ...\`
 \\* proves possession of the shared gateway operator secret
 \\* ignores narrower \`x-openclaw-scopes\`
 \\* restores the full default operator scope set:
 \`operator.admin\`, \`operator.approvals\`, \`operator.pairing\`,
 \`operator.read\`, \`operator.talk.secrets\`, \`operator.write\`
 \\* treats direct tool invokes on this endpoint as owner-sender turns
\\* trusted identity-bearing HTTP modes (for example trusted proxy auth, or \`gateway.auth.mode="none"\` on private ingress)
 \\* authenticate some outer trusted identity or deployment boundary
 \\* honor \`x-openclaw-scopes\` when the header is present
 \\* fall back to the normal operator default scope set when the header is absent
 \\* only lose owner semantics when the caller explicitly narrows scopes and omits \`operator.admin\`

\## Request body

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "tool": "sessions\_list",
 "action": "json",
 "args": {},
 "sessionKey": "main",
 "dryRun": false
}
\`\`\`

Fields:

\\* \`tool\` (string, required): tool name to invoke.
\\* \`action\` (string, optional): mapped into args if the tool schema supports \`action\` and the args payload omitted it.
\\* \`args\` (object, optional): tool-specific arguments.
\\* \`sessionKey\` (string, optional): target session key. If omitted or \`"main"\`, the Gateway uses the configured main session key (honors \`session.mainKey\` and default agent, or \`global\` in global scope).
\\* \`dryRun\` (boolean, optional): reserved for future use; currently ignored.

\## Policy + routing behavior

Tool availability is filtered through the same policy chain used by Gateway agents:

\\* \`tools.profile\` / \`tools.byProvider.profile\`
\\* \`tools.allow\` / \`tools.byProvider.allow\`
\\* \`agents..tools.allow\` / \`agents..tools.byProvider.allow\`
\\* group policies (if the session key maps to a group or channel)
\\* subagent policy (when invoking with a subagent session key)

If a tool is not allowed by policy, the endpoint returns \*\*404\*\*.

Important boundary notes:

\\* Exec approvals are operator guardrails, not a separate authorization boundary for this HTTP endpoint. If a tool is reachable here via Gateway auth + tool policy, \`/tools/invoke\` does not add an extra per-call approval prompt.
\\* Do not share Gateway bearer credentials with untrusted callers. If you need separation across trust boundaries, run separate gateways (and ideally separate OS users/hosts).

Gateway HTTP also applies a hard deny list by default (even if session policy allows the tool):

\\* \`exec\` — direct command execution (RCE surface)
\\* \`spawn\` — arbitrary child process creation (RCE surface)
\\* \`shell\` — shell command execution (RCE surface)
\\* \`fs\_write\` — arbitrary file mutation on the host
\\* \`fs\_delete\` — arbitrary file deletion on the host
\\* \`fs\_move\` — arbitrary file move/rename on the host
\\* \`apply\_patch\` — patch application can rewrite arbitrary files
\\* \`sessions\_spawn\` — session orchestration; spawning agents remotely is RCE
\\* \`sessions\_send\` — cross-session message injection
\\* \`cron\` — persistent automation control plane
\\* \`gateway\` — gateway control plane; prevents reconfiguration via HTTP
\\* \`nodes\` — node command relay can reach system.run on paired hosts
\\* \`whatsapp\_login\` — interactive setup requiring terminal QR scan; hangs on HTTP

You can customize this deny list via \`gateway.tools\`:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 gateway: {
 tools: {
 // Additional tools to block over HTTP /tools/invoke
 deny: \["browser"\],
 // Remove tools from the default deny list
 allow: \["gateway"\],
 },
 },
}
\`\`\`

To help group policies resolve context, you can optionally set:

\\* \`x-openclaw-message-channel: \` (example: \`slack\`, \`telegram\`)
\\* \`x-openclaw-account-id: \` (when multiple accounts exist)

\## Responses

\\* \`200\` → \`{ ok: true, result }\`
\\* \`400\` → \`{ ok: false, error: { type, message } }\` (invalid request or tool input error)
\\* \`401\` → unauthorized
\\* \`429\` → auth rate-limited (\`Retry-After\` set)
\\* \`404\` → tool not available (not found or not allowlisted)
\\* \`405\` → method not allowed
\\* \`500\` → \`{ ok: false, error: { type, message } }\` (unexpected tool execution error; sanitized message)

\## Example

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
curl -sS http://127.0.0.1:18789/tools/invoke \
 -H 'Authorization: Bearer secret' \
 -H 'Content-Type: application/json' \
 -d '{
 "tool": "sessions\_list",
 "action": "json",
 "args": {}
 }'
\`\`\`

\## Related

\\* \[Gateway protocol\](/gateway/protocol)
\\* \[Tools and plugins\](/tools)

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
  -d '{"model":"<id>","messages":[{"role":"user","content":"hi"}],"stream":false}'
openclaw infer model run --model <provider/model> --prompt "hi" --json
openclaw logs --follow
```

Look for:

- direct tiny calls succeed, but OpenClaw runs fail only on larger prompts
- `model_not_found` or 404 errors even though direct `/v1/chat/completions`
works with the same bare model id
- backend errors about `messages[].content` expecting a string
- intermittent `incomplete turn detected ... stopReason=stop payloads=0` warnings with an OpenAI-compatible local backend
- backend crashes that appear only with larger prompt-token counts or full agent runtime prompts

Common signatures

- `model_not_found` with a local MLX/vLLM-style server → verify `baseUrl` includes `/v1`, `api` is `"openai-completions"` for `/v1/chat/completions` backends, and `models.providers.<provider>.models[].id` is the bare provider-local id. Select it with the provider prefix once, for example `mlx/mlx-community/Qwen3-30B-A3B-6bit`; keep the catalog entry as `mlx-community/Qwen3-30B-A3B-6bit`.
- `messages[...].content: invalid type: sequence, expected a string` → backend rejects structured Chat Completions content parts. Fix: set `models.providers.<provider>.models[].compat.requiresStringContent: true`.
- `incomplete turn detected ... stopReason=stop payloads=0` → the backend completed the Chat Completions request but returned no user-visible assistant text for that turn. OpenClaw retries replay-safe empty OpenAI-compatible turns once; persistent failures usually mean the backend is emitting empty/non-text content or suppressing final-answer text.
- direct tiny requests succeed, but OpenClaw agent runs fail with backend/model crashes (for example Gemma on some `inferrs` builds) → OpenClaw transport is likely already correct; the backend is failing on the larger agent-runtime prompt shape.
- failures shrink after disabling tools but do not disappear → tool schemas were part of the pressure, but the remaining issue is still upstream model/server capacity or a backend bug.

Fix options

1. Set `compat.requiresStringContent: true` for string-only Chat Completions backends.
2. Set `compat.supportsTools: false` for models/backends that cannot handle OpenClaw’s tool schema surface reliably.
3. Lower prompt pressure where possible: smaller workspace bootstrap, shorter session history, lighter local model, or a backend with stronger long-context support.
4. If tiny direct requests keep passing while OpenClaw agent turns still crash inside the backend, treat it as an upstream server/model limitation and file a repro there with the accepted payload shape.

Related:

- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Local models](https://docs.openclaw.ai/gateway/local-models)
- [OpenAI-compatible endpoints](https://docs.openclaw.ai/gateway/configuration-reference#openai-compatible-endpoints)

## No replies

If channels are up but nothing answers, check routing and policy before reconnecting anything.

```
openclaw status
openclaw channels status --probe
openclaw pairing list --channel <channel> [--account <id>]
openclaw config get channels
openclaw logs --follow
```

Look for:

- Pairing pending for DM senders.
- Group mention gating (`requireMention`, `mentionPatterns`).
- Channel/group allowlist mismatches.

Common signatures:

- `drop guild message (mention required` → group message ignored until mention.
- `pairing request` → sender needs approval.
- `blocked` / `allowlist` → sender/channel was filtered by policy.

Related:

- [Channel troubleshooting](https://docs.openclaw.ai/channels/troubleshooting)
- [Groups](https://docs.openclaw.ai/channels/groups)
- [Pairing](https://docs.openclaw.ai/channels/pairing)

## Dashboard control UI connectivity

When dashboard/control UI will not connect, validate URL, auth mode, and secure context assumptions.

```
openclaw gateway status
openclaw status
openclaw logs --follow
openclaw doctor
openclaw gateway status --json
```

Look for:

- Correct probe URL and dashboard URL.
- Auth mode/token mismatch between client and gateway.
- HTTP usage where device identity is required.

Connect / auth signatures

- `device identity required` → non-secure context or missing device auth.
- `origin not allowed` → browser `Origin` is not in `gateway.controlUi.allowedOrigins` (or you are connecting from a non-loopback browser origin without an explicit allowlist).
- `device nonce required` / `device nonce mismatch` → client is not completing the challenge-based device auth flow (`connect.challenge` \+ `device.nonce`).
- `device signature invalid` / `device signature expired` → client signed the wrong payload (or stale timestamp) for the current handshake.
- `AUTH_TOKEN_MISMATCH` with `canRetryWithDeviceToken=true` → client can do one trusted retry with cached device token.
- That cached-token retry reuses the cached scope set stored with the paired device token. Explicit `deviceToken` / explicit `scopes` callers keep their requested scope set instead.
- Outside that retry path, connect auth precedence is explicit shared token/password first, then explicit `deviceToken`, then stored device token, then bootstrap token.
- On the async Tailscale Serve Control UI path, failed attempts for the same `{scope, ip}` are serialized before the limiter records the failure. Two bad concurrent retries from the same client can therefore surface `retry later` on the second attempt instead of two plain mismatches.
- `too many failed authentication attempts (retry later)` from a browser-origin loopback client → repeated failures from that same normalized `Origin` are locked out temporarily; another localhost origin uses a separate bucket.
- repeated `unauthorized` after that retry → shared token/device token drift; refresh token config and re-approve/rotate device token if needed.
- `gateway connect failed:` → wrong host/port/url target.

### Auth detail codes quick map

Use `error.details.code` from the failed `connect` response to pick the next action:

| Detail code | Meaning | Recommended action |
| --- | --- | --- |
| `AUTH_TOKEN_MISSING` | Client did not send a required shared token. | Paste/set token in the client and retry. For dashboard paths: `openclaw config get gateway.auth.token` then paste into Control UI settings. |
| `AUTH_TOKEN_MISMATCH` | Shared token did not match gateway auth token. | If `canRetryWithDeviceToken=true`, allow one trusted retry. Cached-token retries reuse stored approved scopes; explicit `deviceToken` / `scopes` callers keep requested scopes. If still failing, run the [token drift recovery checklist](https://docs.openclaw.ai/cli/devices#token-drift-recovery-checklist). |
| `AUTH_DEVICE_TOKEN_MISMATCH` | Cached per-device token is stale or revoked. | Rotate/re-approve device token using [devices CLI](https://docs.openclaw.ai/cli/devices), then reconnect. |
| `PAIRING_REQUIRED` | Device identity needs approval. Check `error.details.reason` for `not-paired`, `scope-upgrade`, `role-upgrade`, or `metadata-upgrade`, and use `requestId` / `remediationHint` when present. | Approve pending request: `openclaw devices list` then `openclaw devices approve <requestId>`. Scope/role upgrades use the same flow after you review the requested access. |

Direct loopback backend RPCs authenticated with the shared gateway token/password should not depend on the CLI’s paired-device scope baseline. If subagents or other internal calls still fail with `scope-upgrade`, verify the caller is using `client.id: "gateway-client"` and `client.mode: "backend"` and is not forcing an explicit `deviceIdentity` or device token.

Device auth v2 migration check:

```
openclaw --version
openclaw doctor
openclaw gateway status
```

If logs show nonce/signature errors, update the connecting client and verify it:

1

[Navigate to header](https://docs.openclaw.ai/gateway/troubleshooting#)

Wait for connect.challenge

Client waits for the gateway-issued `connect.challenge`.

2

[Navigate to header](https://docs.openclaw.ai/gateway/troubleshooting#)

Sign the payload

Client signs the challenge-bound payload.

3

[Navigate to header](https://docs.openclaw.ai/gateway/troubleshooting#)

Send the device nonce

Client sends `connect.params.device.nonce` with the same challenge nonce.

If `openclaw devices rotate` / `revoke` / `remove` is denied unexpectedly:

- paired-device token sessions can manage only **their own** device unless the caller also has `operator.admin`
- `openclaw devices rotate --scope ...` can only request operator scopes that the caller session already holds

Related:

- [Configuration](https://docs.openclaw.ai/gateway/configuration) (gateway auth modes)
- [Control UI](https://docs.openclaw.ai/web/control-ui)
- [Devices](https://docs.openclaw.ai/cli/devices)
- [Remote access](https://docs.openclaw.ai/gateway/remote)
- [Trusted proxy auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth)

## Gateway service not running

Use this when service is installed but process does not stay up.

```
openclaw gateway status
openclaw status
openclaw logs --follow
openclaw doctor
openclaw gateway status --deep   # also scan system-level services
```

Look for:

- `Runtime: stopped` with exit hints.
- Service config mismatch (`Config (cli)` vs `Config (service)`).
- Port/listener conflicts.
- Extra launchd/systemd/schtasks installs when `--deep` is used.
- `Other gateway-like services detected (best effort)` cleanup hints.

Common signatures

- `Gateway start blocked: set gateway.mode=local` or `existing config is missing gateway.mode` → local gateway mode is not enabled, or the config file was clobbered and lost `gateway.mode`. Fix: set `gateway.mode="local"` in your config, or re-run `openclaw onboard --mode local` / `openclaw setup` to restamp the expected local-mode config. If you are running OpenClaw via Podman, the default config path is `~/.openclaw/openclaw.json`.
- `refusing to bind gateway ... without auth` → non-loopback bind without a valid gateway auth path (token/password, or trusted-proxy where configured).
- `another gateway instance is already listening` / `EADDRINUSE` → port conflict.
- `Other gateway-like services detected (best effort)` → stale or parallel launchd/systemd/schtasks units exist. Most setups should keep one gateway per machine; if you do need more than one, isolate ports + config/state/workspace. See [/gateway#multiple-gateways-same-host](https://docs.openclaw.ai/gateway#multiple-gateways-same-host).
- `System-level OpenClaw gateway service detected` from doctor → a systemd system unit exists while the user-level service is missing. Remove or disable the duplicate before allowing doctor to install a user service, or set `OPENCLAW_SERVICE_REPAIR_POLICY=external` if the system unit is the intended supervisor.
- `Gateway service port does not match current gateway config` → the installed supervisor still pins the old `--port`. Run `openclaw doctor --fix` or `openclaw gateway install --force`, then restart the gateway service.

Related:

- [Background exec and process tool](https://docs.openclaw.ai/gateway/background-process)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Doctor](https://docs.openclaw.ai/gateway/doctor)

## Gateway restored last-known-good config

Use this when the Gateway starts, but logs say it restored `openclaw.json`.

```
openclaw logs --follow
openclaw config file
openclaw config validate
openclaw doctor
```

Look for:

- `Config auto-restored from last-known-good`
- `gateway: invalid config was restored from last-known-good backup`
- `config reload restored last-known-good config after invalid-config`
- A timestamped `openclaw.json.clobbered.*` file beside the active config
- A main-agent system event that starts with `Config recovery warning`

What happened

- The rejected config did not validate during startup or hot reload.
- OpenClaw preserved the rejected payload as `.clobbered.*`.
- The active config was restored from the last validated last-known-good copy.
- The next main-agent turn is warned not to blindly rewrite the rejected config.
- If all validation issues were under `plugins.entries.<id>...`, OpenClaw would not restore the whole file. Plugin-local failures stay loud while unrelated user settings remain in the active config.

Inspect and repair

```
CONFIG="$(openclaw config file)"
ls -lt "$CONFIG".clobbered.* "$CONFIG".rejected.* 2>/dev/null | head
diff -u "$CONFIG" "$(ls -t "$CONFIG".clobbered.* 2>/dev/null | head -n 1)"
openclaw config validate
openclaw doctor
```

Common signatures

- `.clobbered.*` exists → an external direct edit or startup read was restored.
- `.rejected.*` exists → an OpenClaw-owned config write failed schema or clobber checks before commit.
- `Config write rejected:` → the write tried to drop required shape, shrink the file sharply, or persist invalid config.
- `Rejected validation details:` → the recovery log or main-agent notice includes the schema path that caused the restore, such as `agents.defaults.execution` or `gateway.auth.password.source`.
- `missing-meta-vs-last-good`, `gateway-mode-missing-vs-last-good`, or `size-drop-vs-last-good:*` → startup treated the current file as clobbered because it lost fields or size compared with the last-known-good backup.
- `Config last-known-good promotion skipped` → the candidate contained redacted secret placeholders such as `***`.

Fix options

1. Keep the restored active config if it is correct.
2. Copy only the intended keys from `.clobbered.*` or `.rejected.*`, then apply them with `openclaw config set` or `config.patch`.
3. Run `openclaw config validate` before restarting.
4. If you edit by hand, keep the full JSON5 config, not just the partial object you wanted to change.

Related:

- [Config](https://docs.openclaw.ai/cli/config)
- [Configuration: hot reload](https://docs.openclaw.ai/gateway/configuration#config-hot-reload)
- [Configuration: strict validation](https://docs.openclaw.ai/gateway/configuration#strict-validation)
- [Doctor](https://docs.openclaw.ai/gateway/doctor)

## Gateway probe warnings

Use this when `openclaw gateway probe` reaches something, but still prints a warning block.

```
openclaw gateway probe
openclaw gateway probe --json
openclaw gateway probe --ssh user@gateway-host
```

Look for:

- `warnings[].code` and `primaryTargetId` in JSON output.
- Whether the warning is about SSH fallback, multiple gateways, missing scopes, or unresolved auth refs.

Common signatures:

- `SSH tunnel failed to start; falling back to direct probes.` → SSH setup failed, but the command still tried direct configured/loopback targets.
- `multiple reachable gateways detected` → more than one target answered. Usually this means an intentional multi-gateway setup or stale/duplicate listeners.
- `Read-probe diagnostics are limited by gateway scopes (missing operator.read)` → connect worked, but detail RPC is scope-limited; pair device identity or use credentials with `operator.read`.
- `Gateway accepted the WebSocket connection, but follow-up read diagnostics failed` → connect worked, but the full diagnostic RPC set timed out or failed. Treat this as a reachable Gateway with degraded diagnostics; compare `connect.ok` and `connect.rpcOk` in `--json` output.
- `Capability: pairing-pending` or `gateway closed (1008): pairing required` → the gateway answered, but this client still needs pairing/approval before normal operator access.
- unresolved `gateway.auth.*` / `gateway.remote.*` SecretRef warning text → auth material was unavailable in this command path for the failed target.

Related:

- [Gateway](https://docs.openclaw.ai/cli/gateway)
- [Multiple gateways on the same host](https://docs.openclaw.ai/gateway#multiple-gateways-same-host)
- [Remote access](https://docs.openclaw.ai/gateway/remote)

## Channel connected, messages not flowing

If channel state is connected but message flow is dead, focus on policy, permissions, and channel specific delivery rules.

```
openclaw channels status --probe
openclaw pairing list --channel <channel> [--account <id>]
openclaw status --deep
openclaw logs --follow
openclaw config get channels
```

Look for:

- DM policy (`pairing`, `allowlist`, `open`, `disabled`).
- Group allowlist and mention requirements.
- Missing channel API permissions/scopes.

Common signatures:

- `mention required` → message ignored by group mention policy.
- `pairing` / pending approval traces → sender is not approved.
- `missing_scope`, `not_in_channel`, `Forbidden`, `401/403` → channel auth/permissions issue.

Related:

- [Channel troubleshooting](https://docs.openclaw.ai/channels/troubleshooting)
- [Discord](https://docs.openclaw.ai/channels/discord)
- [Telegram](https://docs.openclaw.ai/channels/telegram)
- [WhatsApp](https://docs.openclaw.ai/channels/whatsapp)

## Cron and heartbeat delivery

If cron or heartbeat did not run or did not deliver, verify scheduler state first, then delivery target.

```
openclaw cron status
openclaw cron list
openclaw cron runs --id <jobId> --limit 20
openclaw system heartbeat last
openclaw logs --follow
```

Look for:

- Cron enabled and next wake present.
- Job run history status (`ok`, `skipped`, `error`).
- Heartbeat skip reasons (`quiet-hours`, `requests-in-flight`, `cron-in-progress`, `lanes-busy`, `alerts-disabled`, `empty-heartbeat-file`, `no-tasks-due`).

Common signatures

- `cron: scheduler disabled; jobs will not run automatically` → cron disabled.
- `cron: timer tick failed` → scheduler tick failed; check file/log/runtime errors.
- `heartbeat skipped` with `reason=quiet-hours` → outside active hours window.
- `heartbeat skipped` with `reason=empty-heartbeat-file` → `HEARTBEAT.md` exists but only contains blank lines / markdown headers, so OpenClaw skips the model call.
- `heartbeat skipped` with `reason=no-tasks-due` → `HEARTBEAT.md` contains a `tasks:` block, but none of the tasks are due on this tick.
- `heartbeat: unknown accountId` → invalid account id for heartbeat delivery target.
- `heartbeat skipped` with `reason=dm-blocked` → heartbeat target resolved to a DM-style destination while `agents.defaults.heartbeat.directPolicy` (or per-agent override) is set to `block`.

Related:

- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat)
- [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs)
- [Scheduled tasks: troubleshooting](https://docs.openclaw.ai/automation/cron-jobs#troubleshooting)

## Node paired, tool fails

If a node is paired but tools fail, isolate foreground, permission, and approval state.

```
openclaw nodes status
openclaw nodes describe --node <idOrNameOrIp>
openclaw approvals get --node <idOrNameOrIp>
openclaw logs --follow
openclaw status
```

Look for:

- Node online with expected capabilities.
- OS permission grants for camera/mic/location/screen.
- Exec approvals and allowlist state.

Common signatures:

- `NODE_BACKGROUND_UNAVAILABLE` → node app must be in foreground.
- `*_PERMISSION_REQUIRED` / `LOCATION_PERMISSION_REQUIRED` → missing OS permission.
- `SYSTEM_RUN_DENIED: approval required` → exec approval pending.
- `SYSTEM_RUN_DENIED: allowlist miss` → command blocked by allowlist.

Related:

- [Exec approvals](https://docs.openclaw.ai/tools/exec-approvals)
- [Node troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting)
- [Nodes](https://docs.openclaw.ai/nodes/index)

## Browser tool fails

Use this when browser tool actions fail even though the gateway itself is healthy.

```
openclaw browser status
openclaw browser start --browser-profile openclaw
openclaw browser profiles
openclaw logs --follow
openclaw doctor
```

Look for:

- Whether `plugins.allow` is set and includes `browser`.
- Valid browser executable path.
- CDP profile reachability.
- Local Chrome availability for `existing-session` / `user` profiles.

Plugin / executable signatures

- `unknown command "browser"` or `unknown command 'browser'` → the bundled browser plugin is excluded by `plugins.allow`.
- browser tool missing / unavailable while `browser.enabled=true` → `plugins.allow` excludes `browser`, so the plugin never loaded.
- `Failed to start Chrome CDP on port` → browser process failed to launch.
- `browser.executablePath not found` → configured path is invalid.
- `browser.cdpUrl must be http(s) or ws(s)` → the configured CDP URL uses an unsupported scheme such as `file:` or `ftp:`.
- `browser.cdpUrl has invalid port` → the configured CDP URL has a bad or out-of-range port.
- `Playwright is not available in this gateway build; '<feature>' is unsupported.` → the current gateway install lacks the core browser runtime dependency; reinstall or update OpenClaw, then restart the gateway. ARIA snapshots and basic page screenshots can still work, but navigation, AI snapshots, CSS-selector element screenshots, and PDF export stay unavailable.

Chrome MCP / existing-session signatures

- `Could not find DevToolsActivePort for chrome` → Chrome MCP existing-session could not attach to the selected browser data dir yet. Open the browser inspect page, enable remote debugging, keep the browser open, approve the first attach prompt, then retry. If signed-in state is not required, prefer the managed `openclaw` profile.
- `No Chrome tabs found for profile="user"` → the Chrome MCP attach profile has no open local Chrome tabs.
- `Remote CDP for profile "<name>" is not reachable` → the configured remote CDP endpoint is not reachable from the gateway host.
- `Browser attachOnly is enabled ... not reachable` or `Browser attachOnly is enabled and CDP websocket ... is not reachable` → attach-only profile has no reachable target, or the HTTP endpoint answered but the CDP WebSocket still could not be opened.

Element / screenshot / upload signatures

- `fullPage is not supported for element screenshots` → screenshot request mixed `--full-page` with `--ref` or `--element`.
- `element screenshots are not supported for existing-session profiles; use ref from snapshot.` → Chrome MCP / `existing-session` screenshot calls must use page capture or a snapshot `--ref`, not CSS `--element`.
- `existing-session file uploads do not support element selectors; use ref/inputRef.` → Chrome MCP upload hooks need snapshot refs, not CSS selectors.
- `existing-session file uploads currently support one file at a time.` → send one upload per call on Chrome MCP profiles.
- `existing-session dialog handling does not support timeoutMs.` → dialog hooks on Chrome MCP profiles do not support timeout overrides.
- `existing-session type does not support timeoutMs overrides.` → omit `timeoutMs` for `act:type` on `profile="user"` / Chrome MCP existing-session profiles, or use a managed/CDP browser profile when a custom timeout is required.
- `existing-session evaluate does not support timeoutMs overrides.` → omit `timeoutMs` for `act:evaluate` on `profile="user"` / Chrome MCP existing-session profiles, or use a managed/CDP browser profile when a custom timeout is required.
- `response body is not supported for existing-session profiles yet.` → `responsebody` still requires a managed browser or raw CDP profile.
- stale viewport / dark-mode / locale / offline overrides on attach-only or remote CDP profiles → run `openclaw browser stop --browser-profile <name>` to close the active control session and release Playwright/CDP emulation state without restarting the whole gateway.

Related:

- [Browser (OpenClaw-managed)](https://docs.openclaw.ai/tools/browser)
- [Browser troubleshooting](https://docs.openclaw.ai/tools/browser-linux-troubleshooting)

## If you upgraded and something suddenly broke

Most post-upgrade breakage is config drift or stricter defaults now being enforced.

1\. Auth and URL override behavior changed

```
openclaw gateway status
openclaw config get gateway.mode
openclaw config get gateway.remote.url
openclaw config get gateway.auth.mode
```

What to check:

- If `gateway.mode=remote`, CLI calls may be targeting remote while your local service is fine.
- Explicit `--url` calls do not fall back to stored credentials.

Common signatures:

- `gateway connect failed:` → wrong URL target.
- `unauthorized` → endpoint reachable but wrong auth.

2\. Bind and auth guardrails are stricter

```
openclaw config get gateway.bind
openclaw config get gateway.auth.mode
openclaw config get gateway.auth.token
openclaw gateway status
openclaw logs --follow
```

What to check:

- Non-loopback binds (`lan`, `tailnet`, `custom`) need a valid gateway auth path: shared token/password auth, or a correctly configured non-loopback `trusted-proxy` deployment.
- Old keys like `gateway.token` do not replace `gateway.auth.token`.

Common signatures:

- `refusing to bind gateway ... without auth` → non-loopback bind without a valid gateway auth path.
- `Connectivity probe: failed` while runtime is running → gateway alive but inaccessible with current auth/url.

3\. Pairing and device identity state changed

```
openclaw devices list
openclaw pairing list --channel <channel> [--account <id>]
openclaw logs --follow
openclaw doctor
```

What to check:

- Pending device approvals for dashboard/nodes.
- Pending DM pairing approvals after policy or identity changes.

Common signatures:

- `device identity required` → device auth not satisfied.
- `pairing required` → sender/device must be approved.

If the service config and runtime still disagree after checks, reinstall service metadata from the same profile/state directory:

```
openclaw gateway install --force
openclaw gateway restart
```

Related:

- [Authentication](https://docs.openclaw.ai/gateway/authentication)
- [Background exec and process tool](https://docs.openclaw.ai/gateway/background-process)
- [Gateway-owned pairing](https://docs.openclaw.ai/gateway/pairing)

## Related

- [Doctor](https://docs.openclaw.ai/gateway/doctor)
- [FAQ](https://docs.openclaw.ai/help/faq)
- [Gateway runbook](https://docs.openclaw.ai/gateway)

[Diagnostics export](https://docs.openclaw.ai/gateway/diagnostics) [Gateway lock](https://docs.openclaw.ai/gateway/gateway-lock)

Ctrl+I

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
- `allowLoopback` trusts local processes on the Gateway host to the same degree as the reverse proxy. Enable it only when the Gateway is still firewalled from direct remote access and the local proxy strips or overwrites client-supplied identity headers.
- Internal Gateway clients that do not travel through the reverse proxy should use `gateway.auth.password` / `OPENCLAW_GATEWAY_PASSWORD`, not trusted-proxy identity headers.
- Non-loopback Control UI deployments still need explicit `gateway.controlUi.allowedOrigins`.
- **Forwarded-header evidence overrides loopback locality for local direct fallback.** If a request arrives on loopback but carries `X-Forwarded-For` / `X-Forwarded-Host` / `X-Forwarded-Proto` headers pointing at a non-local origin, that evidence disqualifies local-direct password fallback and device-identity gating. With `allowLoopback: true`, trusted-proxy auth can still accept the request as a same-host proxy request, while `requiredHeaders` and `allowUsers` continue to apply.

### Configuration reference

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-trusted-proxies)

gateway.trustedProxies

string\[\]

required

Array of proxy IP addresses to trust. Requests from other IPs are rejected.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-mode)

gateway.auth.mode

string

required

Must be `"trusted-proxy"`.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-trusted-proxy-user-header)

gateway.auth.trustedProxy.userHeader

string

required

Header name containing the authenticated user identity.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-trusted-proxy-required-headers)

gateway.auth.trustedProxy.requiredHeaders

string\[\]

Additional headers that must be present for the request to be trusted.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-trusted-proxy-allow-users)

gateway.auth.trustedProxy.allowUsers

string\[\]

Allowlist of user identities. Empty means allow all authenticated users.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-trusted-proxy-allow-loopback)

gateway.auth.trustedProxy.allowLoopback

boolean

Opt-in support for same-host loopback reverse proxies. Defaults to `false`.

Only enable `allowLoopback` when the local reverse proxy is the intended trust boundary. Any local process that can connect to the Gateway can try to send proxy identity headers, so keep direct Gateway access private to the host and require proxy-owned headers such as `x-forwarded-proto` or a signed assertion header where your proxy supports one.

## TLS termination and HSTS

Use one TLS termination point and apply HSTS there.

- Proxy TLS termination (recommended)

- Gateway TLS termination

When your reverse proxy handles HTTPS for `https://control.example.com`, set `Strict-Transport-Security` at the proxy for that domain.

- Good fit for internet-facing deployments.
- Keeps certificate + HTTP hardening policy in one place.
- OpenClaw can stay on loopback HTTP behind the proxy.

Example header value:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

If OpenClaw itself serves HTTPS directly (no TLS-terminating proxy), set:

```
{
  gateway: {
    tls: { enabled: true },
    http: {
      securityHeaders: {
        strictTransportSecurity: "max-age=31536000; includeSubDomains",
      },
    },
  },
}
```

`strictTransportSecurity` accepts a string header value, or `false` to disable explicitly.

### Rollout guidance

- Start with a short max age first (for example `max-age=300`) while validating traffic.
- Increase to long-lived values (for example `max-age=31536000`) only after confidence is high.
- Add `includeSubDomains` only if every subdomain is HTTPS-ready.
- Use preload only if you intentionally meet preload requirements for your full domain set.
- Loopback-only local development does not benefit from HSTS.

## Proxy setup examples

Pomerium

Pomerium passes identity in `x-pomerium-claim-email` (or other claim headers) and a JWT in `x-pomerium-jwt-assertion`.

```
{
  gateway: {
    bind: "lan",
    trustedProxies: ["10.0.0.1"], // Pomerium's IP
    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        userHeader: "x-pomerium-claim-email",
        requiredHeaders: ["x-pomerium-jwt-assertion"],
      },
    },
  },
}
```

Pomerium config snippet:

```
routes:
  - from: https://openclaw.example.com
    to: http://openclaw-gateway:18789
    policy:
      - allow:
          or:
            - email:
                is: nick@example.com
    pass_identity_headers: true
```

Caddy with OAuth

Caddy with the `caddy-security` plugin can authenticate users and pass identity headers.

```
{
  gateway: {
    bind: "lan",
    trustedProxies: ["10.0.0.1"], // Caddy/sidecar proxy IP
    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        userHeader: "x-forwarded-user",
      },
    },
  },
}
```

Caddyfile snippet:

```
openclaw.example.com {
    authenticate with oauth2_provider
    authorize with policy1

    reverse_proxy openclaw:18789 {
        header_up X-Forwarded-User {http.auth.user.email}
    }
}
```

nginx + oauth2-proxy

oauth2-proxy authenticates users and passes identity in `x-auth-request-email`.

```
{
  gateway: {
    bind: "lan",
    trustedProxies: ["10.0.0.1"], // nginx/oauth2-proxy IP
    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        userHeader: "x-auth-request-email",
      },
    },
  },
}
```

nginx config snippet:

```
location / {
    auth_request /oauth2/auth;
    auth_request_set $user $upstream_http_x_auth_request_email;

    proxy_pass http://openclaw:18789;
    proxy_set_header X-Auth-Request-Email $user;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
}
```

Traefik with forward auth

```
{
  gateway: {
    bind: "lan",
    trustedProxies: ["172.17.0.1"], // Traefik container IP
    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        userHeader: "x-forwarded-user",
      },
    },
  },
}
```

## Mixed token configuration

OpenClaw rejects ambiguous configurations where both a `gateway.auth.token` (or `OPENCLAW_GATEWAY_TOKEN`) and `trusted-proxy` mode are active at the same time. Mixed token configs can cause loopback requests to silently authenticate on the wrong auth path.If you see a `mixed_trusted_proxy_token` error on startup:

- Remove the shared token when using trusted-proxy mode, or
- Switch `gateway.auth.mode` to `"token"` if you intend token-based auth.

Loopback trusted-proxy identity headers still fail closed: same-host callers are not silently authenticated as proxy users. Internal OpenClaw callers that bypass the proxy may authenticate with `gateway.auth.password` / `OPENCLAW_GATEWAY_PASSWORD` instead. Token fallback remains intentionally unsupported in trusted-proxy mode.

## Operator scopes header

Trusted-proxy auth is an **identity-bearing** HTTP mode, so callers may optionally declare operator scopes with `x-openclaw-scopes`.Examples:

- `x-openclaw-scopes: operator.read`
- `x-openclaw-scopes: operator.read,operator.write`
- `x-openclaw-scopes: operator.admin,operator.write`

Behavior:

- When the header is present, OpenClaw honors the declared scope set.
- When the header is present but empty, the request declares **no** operator scopes.
- When the header is absent, normal identity-bearing HTTP APIs fall back to the standard operator default scope set.
- Gateway-auth **plugin HTTP routes** are narrower by default: when `x-openclaw-scopes` is absent, their runtime scope falls back to `operator.write`.
- Browser-origin HTTP requests still have to pass `gateway.controlUi.allowedOrigins` (or deliberate Host-header fallback mode) even after trusted-proxy auth succeeds.

Practical rule: send `x-openclaw-scopes` explicitly when you want a trusted-proxy request to be narrower than the defaults, or when a gateway-auth plugin route needs something stronger than write scope.

## Security checklist

Before enabling trusted-proxy auth, verify:

- [ ] **Proxy is the only path**: The Gateway port is firewalled from everything except your proxy.
- [ ] **trustedProxies is minimal**: Only your actual proxy IPs, not entire subnets.
- [ ] **Loopback proxy source is deliberate**: trusted-proxy auth fails closed for loopback-source requests unless `gateway.auth.trustedProxy.allowLoopback` is explicitly enabled for a same-host proxy.
- [ ] **Proxy strips headers**: Your proxy overwrites (not appends) `x-forwarded-*` headers from clients.
- [ ] **TLS termination**: Your proxy handles TLS; users connect via HTTPS.
- [ ] **allowedOrigins is explicit**: Non-loopback Control UI uses explicit `gateway.controlUi.allowedOrigins`.
- [ ] **allowUsers is set** (recommended): Restrict to known users rather than allowing anyone authenticated.
- [ ] **No mixed token config**: Do not set both `gateway.auth.token` and `gateway.auth.mode: "trusted-proxy"`.
- [ ] **Local password fallback is private**: If you configure `gateway.auth.password` for internal direct callers, keep the Gateway port firewalled so non-proxy remote clients cannot reach it directly.

## Security audit

`openclaw security audit` will flag trusted-proxy auth with a **critical** severity finding. This is intentional — it’s a reminder that you’re delegating security to your proxy setup.The audit checks for:

- Base `gateway.trusted_proxy_auth` warning/critical reminder
- Missing `trustedProxies` configuration
- Missing `userHeader` configuration
- Empty `allowUsers` (allows any authenticated user)
- Enabled `allowLoopback` for same-host proxy sources
- Wildcard or missing browser-origin policy on exposed Control UI surfaces

## Troubleshooting

trusted\_proxy\_untrusted\_source

The request didn’t come from an IP in `gateway.trustedProxies`. Check:

- Is the proxy IP correct? (Docker container IPs can change.)
- Is there a load balancer in front of your proxy?
- Use `docker inspect` or `kubectl get pods -o wide` to find actual IPs.

trusted\_proxy\_loopback\_source

OpenClaw rejected a loopback-source trusted-proxy request.Check:

- Is the proxy connecting from `127.0.0.1` / `::1`?
- Are you trying to use trusted-proxy auth with a same-host loopback reverse proxy?

Fix:

- Prefer token/password auth for internal same-host clients that do not go through the proxy, or
- Route through a non-loopback trusted proxy address and keep that IP in `gateway.trustedProxies`, or
- For a deliberate same-host reverse proxy, set `gateway.auth.trustedProxy.allowLoopback = true`, keep the loopback address in `gateway.trustedProxies`, and make sure the proxy strips or overwrites identity headers.

trusted\_proxy\_user\_missing

The user header was empty or missing. Check:

- Is your proxy configured to pass identity headers?
- Is the header name correct? (case-insensitive, but spelling matters)
- Is the user actually authenticated at the proxy?

trusted\_proxy\_missing\_header\_\*

A required header wasn’t present. Check:

- Your proxy configuration for those specific headers.
- Whether headers are being stripped somewhere in the chain.

trusted\_proxy\_user\_not\_allowed

The user is authenticated but not in `allowUsers`. Either add them or remove the allowlist.

trusted\_proxy\_origin\_not\_allowed

Trusted-proxy auth succeeded, but the browser `Origin` header did not pass Control UI origin checks.Check:

- `gateway.controlUi.allowedOrigins` includes the exact browser origin.
- You are not relying on wildcard origins unless you intentionally want allow-all behavior.
- If you intentionally use Host-header fallback mode, `gateway.controlUi.dangerouslyAllowHostHeaderOriginFallback=true` is set deliberately.

WebSocket still failing

Make sure your proxy:

- Supports WebSocket upgrades (`Upgrade: websocket`, `Connection: upgrade`).
- Passes the identity headers on WebSocket upgrade requests (not just HTTP).
- Doesn’t have a separate auth path for WebSocket connections.

## Migration from token auth

If you’re moving from token auth to trusted-proxy:

1

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Configure the proxy

Configure your proxy to authenticate users and pass headers.

2

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Test the proxy independently

Test the proxy setup independently (curl with headers).

3

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Update OpenClaw config

Update OpenClaw config with trusted-proxy auth.

4

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Restart the Gateway

Restart the Gateway.

5

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Test WebSocket

Test WebSocket connections from the Control UI.

6

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Audit

Run `openclaw security audit` and review findings.

## Related

- [Configuration](https://docs.openclaw.ai/gateway/configuration) — config reference
- [Remote access](https://docs.openclaw.ai/gateway/remote) — other remote access patterns
- [Security](https://docs.openclaw.ai/gateway/security) — full security guide
- [Tailscale](https://docs.openclaw.ai/gateway/tailscale) — simpler alternative for tailnet-only access

[Secrets apply plan contract](https://docs.openclaw.ai/gateway/secrets-plan-contract) [Health checks](https://docs.openclaw.ai/gateway/health)

Ctrl+I

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
openclaw gateway --verbose --ws-log compact
openclaw gateway --verbose --ws-log full
```

## Configuring logging

All logging configuration lives under `logging` in `~/.openclaw/openclaw.json`.

```
{
  "logging": {
    "level": "info",
    "file": "/tmp/openclaw/openclaw-YYYY-MM-DD.log",
    "consoleLevel": "info",
    "consoleStyle": "pretty",
    "redactSensitive": "tools",
    "redactPatterns": ["sk-.*"]
  }
}
```

### Log levels

- `logging.level`: **file logs** (JSONL) level.
- `logging.consoleLevel`: **console** verbosity level.

You can override both via the **`OPENCLAW_LOG_LEVEL`** environment variable (e.g. `OPENCLAW_LOG_LEVEL=debug`). The env var takes precedence over the config file, so you can raise verbosity for a single run without editing `openclaw.json`. You can also pass the global CLI option **`--log-level <level>`** (for example, `openclaw --log-level debug gateway run`), which overrides the environment variable for that command.`--verbose` only affects console output and WS log verbosity; it does not change
file log levels.

### Trace correlation

File logs are JSONL. When a log call carries a valid diagnostic trace context,
OpenClaw writes the trace fields as top-level JSON keys (`traceId`, `spanId`,
`parentSpanId`, `traceFlags`) so external log processors can correlate the line
with OTEL spans and provider `traceparent` propagation.Gateway HTTP requests and Gateway WebSocket frames establish an internal request
trace scope. Logs and diagnostic events emitted inside that async scope inherit
the request trace when they do not pass an explicit trace context. Agent run and
model-call traces become children of the active request trace, so local logs,
diagnostic snapshots, OTEL spans, and trusted provider `traceparent` headers can
be joined by `traceId` without logging raw request or model content.

### Model call size and timing

Model-call diagnostics record bounded request/response measurements without
capturing raw prompt or response content:

- `requestPayloadBytes`: UTF-8 byte size of the final model request payload
- `responseStreamBytes`: UTF-8 byte size of streamed model response events
- `timeToFirstByteMs`: elapsed time before the first streamed response event
- `durationMs`: total model-call duration

These fields are available to diagnostic snapshots, model-call plugin hooks, and
OTEL model-call spans/metrics when diagnostics export is enabled.

### Console styles

`logging.consoleStyle`:

- `pretty`: human-friendly, colored, with timestamps.
- `compact`: tighter output (best for long sessions).
- `json`: JSON per line (for log processors).

### Redaction

OpenClaw can redact sensitive tokens before they hit console output, file logs,
OTLP log records, persisted session transcript text, or Control UI tool
event payloads (tool start args, partial/final result payloads, derived
exec output, and patch summaries):

- `logging.redactSensitive`: `off` \| `tools` (default: `tools`)
- `logging.redactPatterns`: list of regex strings to override the default set. Custom patterns apply on top of the built-in defaults for Control UI tool payloads, so adding a pattern never weakens redaction of values already caught by the defaults.

File logs and session transcripts stay JSONL, but matching secret values are
masked before the line or message is written to disk. Redaction is best-effort:
it applies to text-bearing message content and log strings, not every
identifier or binary payload field.The built-in defaults cover common API credentials and payment-credential field
names such as card number, CVC/CVV, shared payment token, and payment credential
when they appear as JSON fields, URL parameters, CLI flags, or assignments.`logging.redactSensitive: "off"` only disables this general log/transcript
policy. OpenClaw still redacts safety-boundary payloads that can be shown to UI
clients, support bundles, diagnostics observers, approval prompts, or agent
tools. Examples include Control UI tool-call events, `sessions_history` output,
diagnostics support exports, provider error observations, exec approval command
display, and Gateway WebSocket protocol logs. Custom `logging.redactPatterns`
can still add project-specific patterns on those surfaces.

## Diagnostics and OpenTelemetry

Diagnostics are structured, machine-readable events for model runs and
message-flow telemetry (webhooks, queueing, session state). They do **not**
replace logs — they feed metrics, traces, and exporters. Events are emitted
in-process whether or not you export them.Two adjacent surfaces:

- **OpenTelemetry export** — send metrics, traces, and logs over OTLP/HTTP to
any OpenTelemetry-compatible collector or backend (Grafana, Datadog,
Honeycomb, New Relic, Tempo, etc.). Full configuration, signal catalog,
metric/span names, env vars, and privacy model live on a dedicated page:
[OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry).
- **Diagnostics flags** — targeted debug-log flags that route extra logs to
`logging.file` without raising `logging.level`. Flags are case-insensitive
and support wildcards (`telegram.*`, `*`). Configure under `diagnostics.flags`
or via the `OPENCLAW_DIAGNOSTICS=...` env override. Full guide:
[Diagnostics flags](https://docs.openclaw.ai/diagnostics/flags).

To enable diagnostics events for plugins or custom sinks without OTLP export:

```
{
  diagnostics: { enabled: true },
}
```

For OTLP export to a collector, see [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry).

## Troubleshooting tips

- **Gateway not reachable?** Run `openclaw doctor` first.
- **Logs empty?** Check that the Gateway is running and writing to the file path
in `logging.file`.
- **Need more detail?** Set `logging.level` to `debug` or `trace` and retry.

## Related

- [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) — OTLP/HTTP export, metric/span catalog, privacy model
- [Diagnostics flags](https://docs.openclaw.ai/diagnostics/flags) — targeted debug-log flags
- [Gateway logging internals](https://docs.openclaw.ai/gateway/logging) — WS log styles, subsystem prefixes, and console capture
- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference#diagnostics) — full `diagnostics.*` field reference

[Doctor](https://docs.openclaw.ai/gateway/doctor) [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry)

Ctrl+I

---

## Formal verification (security models) - OpenClaw

_Source: <https://docs.openclaw.ai/security/formal-verification>_

# Java 11+ required (TLC runs on the JVM).
# The repo vendors a pinned `tla2tools.jar` (TLA+ tools) and provides `bin/tlc` + Make targets.

make <target>
```

### Gateway exposure and open gateway misconfiguration

**Claim:** binding beyond loopback without auth can make remote compromise possible / increases exposure; token/password blocks unauth attackers (per the model assumptions).

- Green runs:
  - `make gateway-exposure-v2`
  - `make gateway-exposure-v2-protected`
- Red (expected):
  - `make gateway-exposure-v2-negative`

See also: `docs/gateway-exposure-matrix.md` in the models repo.

### Node exec pipeline (highest-risk capability)

**Claim:**`exec host=node` requires (a) node command allowlist plus declared commands and (b) live approval when configured; approvals are tokenized to prevent replay (in the model).

- Green runs:
  - `make nodes-pipeline`
  - `make approvals-token`
- Red (expected):
  - `make nodes-pipeline-negative`
  - `make approvals-token-negative`

### Pairing store (DM gating)

**Claim:** pairing requests respect TTL and pending-request caps.

- Green runs:
  - `make pairing`
  - `make pairing-cap`
- Red (expected):
  - `make pairing-negative`
  - `make pairing-cap-negative`

### Ingress gating (mentions + control-command bypass)

**Claim:** in group contexts requiring mention, an unauthorized “control command” cannot bypass mention gating.

- Green:
  - `make ingress-gating`
- Red (expected):
  - `make ingress-gating-negative`

### Routing/session-key isolation

**Claim:** DMs from distinct peers do not collapse into the same session unless explicitly linked/configured.

- Green:
  - `make routing-isolation`
- Red (expected):
  - `make routing-isolation-negative`

## v1++: additional bounded models (concurrency, retries, trace correctness)

These are follow-on models that tighten fidelity around real-world failure modes (non-atomic updates, retries, and message fan-out).

### Pairing store concurrency / idempotency

**Claim:** a pairing store should enforce `MaxPending` and idempotency even under interleavings (i.e., “check-then-write” must be atomic / locked; refresh shouldn’t create duplicates).What it means:

- Under concurrent requests, you can’t exceed `MaxPending` for a channel.
- Repeated requests/refreshes for the same `(channel, sender)` should not create duplicate live pending rows.
- Green runs:  - `make pairing-race` (atomic/locked cap check)
  - `make pairing-idempotency`
  - `make pairing-refresh`
  - `make pairing-refresh-race`
- Red (expected):  - `make pairing-race-negative` (non-atomic begin/commit cap race)
  - `make pairing-idempotency-negative`
  - `make pairing-refresh-negative`
  - `make pairing-refresh-race-negative`

### Ingress trace correlation / idempotency

**Claim:** ingestion should preserve trace correlation across fan-out and be idempotent under provider retries.What it means:

- When one external event becomes multiple internal messages, every part keeps the same trace/event identity.
- Retries do not result in double-processing.
- If provider event IDs are missing, dedupe falls back to a safe key (e.g., trace ID) to avoid dropping distinct events.
- Green:  - `make ingress-trace`
  - `make ingress-trace2`
  - `make ingress-idempotency`
  - `make ingress-dedupe-fallback`
- Red (expected):  - `make ingress-trace-negative`
  - `make ingress-trace2-negative`
  - `make ingress-idempotency-negative`
  - `make ingress-dedupe-fallback-negative`

### Routing dmScope precedence + identityLinks

**Claim:** routing must keep DM sessions isolated by default, and only collapse sessions when explicitly configured (channel precedence + identity links).What it means:

- Channel-specific dmScope overrides must win over global defaults.
- identityLinks should collapse only within explicit linked groups, not across unrelated peers.
- Green:  - `make routing-precedence`
  - `make routing-identitylinks`
- Red (expected):  - `make routing-precedence-negative`
  - `make routing-identitylinks-negative`

## Related

- [Threat model](https://docs.openclaw.ai/security/THREAT-MODEL-ATLAS)
- [Contributing to the threat model](https://docs.openclaw.ai/security/CONTRIBUTING-THREAT-MODEL)

[Network proxy](https://docs.openclaw.ai/security/network-proxy) [Threat model (MITRE ATLAS)](https://docs.openclaw.ai/security/THREAT-MODEL-ATLAS)

Ctrl+I

---

## Network proxy - OpenClaw

_Source: <https://docs.openclaw.ai/security/network-proxy>_

# Network Proxy

OpenClaw can route runtime HTTP and WebSocket traffic through an operator-managed forward proxy. This is optional defense in depth for deployments that want central egress control, stronger SSRF protection, and better network auditability.OpenClaw does not ship, download, start, configure, or certify a proxy. You run the proxy technology that fits your environment, and OpenClaw routes normal process-local HTTP and WebSocket clients through it.

## Why Use a Proxy?

A proxy gives operators one network control point for outbound HTTP and WebSocket traffic. That can be useful even outside SSRF hardening:

- Central policy: maintain one egress policy instead of relying on every application HTTP call site to get network rules right.
- Connect-time checks: evaluate the destination after DNS resolution and immediately before the proxy opens the upstream connection.
- DNS rebinding defense: reduce the gap between an application-level DNS check and the actual outbound connection.
- Broader JavaScript coverage: route ordinary `fetch`, `node:http`, `node:https`, WebSocket, axios, got, node-fetch, and similar clients through the same path.
- Auditability: log allowed and denied destinations at the egress boundary.
- Operational control: enforce destination rules, network segmentation, rate limits, or outbound allowlists without rebuilding OpenClaw.

Proxy routing is a process-level guardrail for normal HTTP and WebSocket egress. It gives operators a fail-closed path for routing supported JavaScript HTTP clients through their own filtering proxy, but it is not an OS-level network sandbox and does not make OpenClaw certify the proxy’s destination policy.

## How OpenClaw Routes Traffic

When `proxy.enabled=true` and a proxy URL is configured, protected runtime processes such as `openclaw gateway run`, `openclaw node run`, and `openclaw agent --local` route normal HTTP and WebSocket egress through the configured proxy:

```
OpenClaw process
  fetch                  -> operator-managed filtering proxy -> public internet
  node:http and https    -> operator-managed filtering proxy -> public internet
  WebSocket clients      -> operator-managed filtering proxy -> public internet
```

The public contract is the routing behavior, not the internal Node hooks used to implement it. OpenClaw Gateway control-plane WebSocket clients use a narrow direct path for local loopback Gateway RPC traffic when the Gateway URL uses `localhost` or a literal loopback IP such as `127.0.0.1` or `[::1]`. That control-plane path must be able to reach loopback Gateways even when the operator proxy blocks loopback destinations. Normal runtime HTTP and WebSocket requests still use the configured proxy.Internally, OpenClaw uses two process-level routing hooks for this feature:

- Undici dispatcher routing covers `fetch`, undici-backed clients, and transports that provide their own undici dispatcher.
- `global-agent` routing covers Node core `node:http` and `node:https` callers, including many libraries layered on `http.request`, `https.request`, `http.get`, and `https.get`. Managed proxy mode forces that global agent so explicit Node HTTP agents do not accidentally bypass the operator proxy.

Some plugins own custom transports that need explicit proxy wiring even when process-level routing exists. For example, Telegram’s Bot API transport uses its own HTTP/1 undici dispatcher and therefore honors process proxy env plus the managed `OPENCLAW_PROXY_URL` fallback in that owner-specific transport path.The proxy URL itself must use `http://`. HTTPS destinations are still supported through the proxy with HTTP `CONNECT`; this only means OpenClaw expects a plain HTTP forward-proxy listener such as `http://127.0.0.1:3128`.While the proxy is active, OpenClaw clears `no_proxy`, `NO_PROXY`, and `GLOBAL_AGENT_NO_PROXY`. Those bypass lists are destination-based, so leaving `localhost` or `127.0.0.1` there would let high-risk SSRF targets skip the filtering proxy.On shutdown, OpenClaw restores the previous proxy environment and resets cached process routing state.

## Related Proxy Terms

- `proxy.enabled` / `proxy.proxyUrl`: outbound forward-proxy routing for OpenClaw runtime egress. This page documents that feature.
- `gateway.auth.mode: "trusted-proxy"`: inbound identity-aware reverse-proxy authentication for Gateway access. See [Trusted proxy auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth).
- `openclaw proxy`: local debug proxy and capture inspector for development and support. See [openclaw proxy](https://docs.openclaw.ai/cli/proxy).
- Channel or provider-specific proxy settings: owner-specific overrides for a particular transport. Prefer the managed network proxy when the goal is central egress control across the runtime.

## Configuration

```
proxy:
  enabled: true
  proxyUrl: http://127.0.0.1:3128
```

You can also provide the URL through the environment, while keeping `proxy.enabled=true` in config:

```
OPENCLAW_PROXY_URL=http://127.0.0.1:3128 openclaw gateway run
```

`proxy.proxyUrl` takes precedence over `OPENCLAW_PROXY_URL`.If `enabled=true` but no valid proxy URL is configured, protected commands fail startup instead of falling back to direct network access.For managed gateway services started with `openclaw gateway start`, prefer storing the URL in config:

```
openclaw config set proxy.enabled true
openclaw config set proxy.proxyUrl http://127.0.0.1:3128
openclaw gateway install --force
openclaw gateway start
```

The environment fallback is best for foreground runs. If you use it with an installed service, put `OPENCLAW_PROXY_URL` in the service durable environment, such as `$OPENCLAW_STATE_DIR/.env` or `~/.openclaw/.env`, then reinstall the service so launchd, systemd, or Scheduled Tasks starts the gateway with that value.For `openclaw --container ...` commands, OpenClaw forwards `OPENCLAW_PROXY_URL` into the container-targeted child CLI when it is set. The URL must be reachable from inside the container; `127.0.0.1` refers to the container itself, not the host. OpenClaw rejects loopback proxy URLs for container-targeted commands unless you explicitly override that safety check.

## Proxy Requirements

The proxy policy is the security boundary. OpenClaw cannot verify that the proxy blocks the right targets.Configure the proxy to:

- Bind only to loopback or a private trusted interface.
- Restrict access so only the OpenClaw process, host, container, or service account can use it.
- Resolve destinations itself and block destination IPs after DNS resolution.
- Apply policy at connect time for both plain HTTP requests and HTTPS `CONNECT` tunnels.
- Reject destination-based bypasses for loopback, private, link-local, metadata, multicast, reserved, or documentation ranges.
- Avoid hostname allowlists unless you fully trust the DNS resolution path.
- Log destination, decision, status, and reason without logging request bodies, authorization headers, cookies, or other secrets.
- Keep proxy policy under version control and review changes like security-sensitive configuration.

## Recommended Blocked Destinations

Use this denylist as the starting point for any forward proxy, firewall, or egress policy.OpenClaw application-level classifier logic lives in `src/infra/net/ssrf.ts` and `src/shared/net/ip.ts`. The relevant parity hooks are `BLOCKED_HOSTNAMES`, `BLOCKED_IPV4_SPECIAL_USE_RANGES`, `BLOCKED_IPV6_SPECIAL_USE_RANGES`, `RFC2544_BENCHMARK_PREFIX`, and the embedded IPv4 sentinel handling for NAT64, 6to4, Teredo, ISATAP, and IPv4-mapped forms. Those files are useful references when maintaining an external proxy policy, but OpenClaw does not automatically export or enforce those rules in your proxy.

| Range or host | Why to block |
| --- | --- |
| `127.0.0.0/8`, `localhost`, `localhost.localdomain` | IPv4 loopback |
| `::1/128` | IPv6 loopback |
| `0.0.0.0/8`, `::/128` | Unspecified and this-network addresses |
| `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` | RFC1918 private networks |
| `169.254.0.0/16`, `fe80::/10` | Link-local addresses and common cloud metadata paths |
| `169.254.169.254`, `metadata.google.internal` | Cloud metadata services |
| `100.64.0.0/10` | Carrier-grade NAT shared address space |
| `198.18.0.0/15`, `2001:2::/48` | Benchmarking ranges |
| `192.0.0.0/24`, `192.0.2.0/24`, `198.51.100.0/24`, `203.0.113.0/24`, `2001:db8::/32` | Special-use and documentation ranges |
| `224.0.0.0/4`, `ff00::/8` | Multicast |
| `240.0.0.0/4` | Reserved IPv4 |
| `fc00::/7`, `fec0::/10` | IPv6 local/private ranges |
| `100::/64`, `2001:20::/28` | IPv6 discard and ORCHIDv2 ranges |
| `64:ff9b::/96`, `64:ff9b:1::/48` | NAT64 prefixes with embedded IPv4 |
| `2002::/16`, `2001::/32` | 6to4 and Teredo with embedded IPv4 |
| `::/96`, `::ffff:0:0/96` | IPv4-compatible and IPv4-mapped IPv6 |

If your cloud provider or network platform documents additional metadata hosts or reserved ranges, add those too.

## Validation

Validate the proxy from the same host, container, or service account that runs OpenClaw:

```
openclaw proxy validate --proxy-url http://127.0.0.1:3128
```

By default, when no custom destinations are provided, the command checks that `https://example.com/` succeeds and starts a temporary loopback canary that the proxy must not reach. The default denied check passes when the proxy returns a non-2xx denial response or blocks the canary with a transport failure; it fails if a successful response reaches the canary. If no proxy is enabled and configured, validation reports a config problem; use `--proxy-url` for a one-off preflight before changing config. Use `--allowed-url` and `--denied-url` to test deployment-specific expectations. Custom denied destinations are fail-closed: any HTTP response means the destination was reachable through the proxy, and any transport error is reported as inconclusive because OpenClaw cannot prove the proxy blocked a reachable origin. On validation failure, the command exits with code 1.Use `--json` for automation. The JSON output contains the overall result, the effective proxy config source, any config errors, and each destination check. Proxy URL credentials are redacted in text and JSON output:

```
{
  "ok": true,
  "config": {
    "enabled": true,
    "proxyUrl": "http://127.0.0.1:3128/",
    "source": "override",
    "errors": []
  },
  "checks": [\
    {\
      "kind": "allowed",\
      "url": "https://example.com/",\
      "ok": true,\
      "status": 200\
    }\
  ]
}
```

You can also validate manually with `curl`:

```
curl -x http://127.0.0.1:3128 https://example.com/
curl -x http://127.0.0.1:3128 http://127.0.0.1/
curl -x http://127.0.0.1:3128 http://169.254.169.254/
```

The public request should succeed. The loopback and metadata requests should be blocked by the proxy. For `openclaw proxy validate`, the built-in loopback canary can distinguish a proxy denial from a reachable origin. Custom `--denied-url` checks do not have that canary, so treat both HTTP responses and ambiguous transport failures as validation failures unless your proxy exposes a deployment-specific denial signal you can verify separately.Then enable OpenClaw proxy routing:

```
openclaw config set proxy.enabled true
openclaw config set proxy.proxyUrl http://127.0.0.1:3128
openclaw gateway run
```

or set:

```
proxy:
  enabled: true
  proxyUrl: http://127.0.0.1:3128
```

## Limits

- The proxy improves coverage for process-local JavaScript HTTP and WebSocket clients, but it is not an OS-level network sandbox.
- Raw `net`, `tls`, and `http2` sockets, native addons, and child processes may bypass Node-level proxy routing unless they inherit and respect proxy environment variables.
- User local WebUIs and local model servers should be allowlisted in the operator proxy policy when needed; OpenClaw does not expose a general local-network bypass for them.
- Gateway control-plane proxy bypass is intentionally limited to `localhost` and literal loopback IP URLs. Use `ws://127.0.0.1:18789`, `ws://[::1]:18789`, or `ws://localhost:18789` for local direct Gateway control-plane connections; other hostnames route like ordinary hostname-based traffic.
- OpenClaw does not inspect, test, or certify your proxy policy.
- Treat proxy policy changes as security-sensitive operational changes.

[Tailscale](https://docs.openclaw.ai/gateway/tailscale) [Formal verification (security models)](https://docs.openclaw.ai/security/formal-verification)

Ctrl+I

---

## https://docs.openclaw.ai/zh-CN/gateway/background-process.md

_Source: <https://docs.openclaw.ai/zh-CN/gateway/background-process.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# 后台 exec 和进程工具

\# 后台 Exec + 进程工具

OpenClaw 通过 \`exec\` 工具运行 shell 命令，并将长时间运行的任务保存在内存中。\`process\` 工具用于管理这些后台会话。

\## exec 工具

关键参数：

\\* \`command\`（必填）
\\* \`yieldMs\`（默认 10000）：超过此延迟后自动转入后台
\\* \`background\`（布尔值）：立即转入后台
\\* \`timeout\`（秒，默认 \`tools.exec.timeoutSec\`）：在此超时后终止进程；仅当你想为该次调用禁用 exec 进程超时时，才设置 \`timeout: 0\`
\\* \`elevated\`（布尔值）：如果已启用/允许提升模式，则在沙箱外运行（默认为 \`gateway\`，当 exec 目标是 \`node\` 时则为 \`node\`）
\\* 需要真实的 TTY？设置 \`pty: true\`。
\\* \`workdir\`、\`env\`

行为：

\\* 前台运行会直接返回输出。
\\* 当转入后台时（显式指定或因超时），工具会返回 \`status: "running"\`、\`sessionId\` 和一小段尾部输出。
\\* 后台运行和 \`yieldMs\` 运行会继承 \`tools.exec.timeoutSec\`，除非调用提供了显式的 \`timeout\`。
\\* 输出会保留在内存中，直到该会话被轮询或清除。
\\* 如果 \`process\` 工具不被允许，\`exec\` 会同步运行，并忽略 \`yieldMs\`/\`background\`。
\\* 已生成的 exec 命令会收到 \`OPENCLAW\_SHELL=exec\`，用于支持具备上下文感知能力的 shell/profile 规则。
\\* 对于现在启动的长时间运行任务，只启动一次，然后依赖自动完成唤醒；当该功能已启用且命令产生输出或失败时，它会生效。
\\* 如果自动完成唤醒不可用，或者你需要确认某个无输出但已成功退出的命令确实已完成，请使用 \`process\` 来确认完成状态。
\\* 不要用 \`sleep\` 循环或重复轮询来模拟提醒或延迟跟进；未来任务请使用 cron。

\## 子进程桥接

当你在 exec/process 工具之外生成长时间运行的子进程时（例如 CLI 重启生成的进程或 Gateway 网关辅助进程），请挂载子进程桥接辅助工具，以便转发终止信号，并在退出/出错时移除监听器。这样可以避免在 systemd 下出现孤儿进程，并在各个平台上保持一致的关闭行为。

环境变量覆盖：

\\* \`PI\_BASH\_YIELD\_MS\`：默认 yield（毫秒）
\\* \`PI\_BASH\_MAX\_OUTPUT\_CHARS\`：内存中输出上限（字符）
\\* \`OPENCLAW\_BASH\_PENDING\_MAX\_OUTPUT\_CHARS\`：每个流待处理 stdout/stderr 的上限（字符）
\\* \`PI\_BASH\_JOB\_TTL\_MS\`：已完成会话的 TTL（毫秒，限制在 1 分钟到 3 小时之间）

配置（推荐）：

\\* \`tools.exec.backgroundMs\`（默认 10000）
\\* \`tools.exec.timeoutSec\`（默认 1800）
\\* \`tools.exec.cleanupMs\`（默认 1800000）
\\* \`tools.exec.notifyOnExit\`（默认 true）：当后台 exec 退出时，加入一个系统事件并请求心跳。
\\* \`tools.exec.notifyOnExitEmptySuccess\`（默认 false）：为 true 时，也会为那些成功完成但没有产生输出的后台运行加入完成事件。

\## process 工具

操作：

\\* \`list\`：运行中 + 已完成的会话
\\* \`poll\`：提取某个会话的新输出（也会报告退出状态）
\\* \`log\`：读取聚合输出（支持 \`offset\` + \`limit\`）
\\* \`write\`：发送 stdin（\`data\`，可选 \`eof\`）
\\* \`send-keys\`：向 PTY 支持的会话发送显式按键令牌或字节
\\* \`submit\`：向 PTY 支持的会话发送 Enter / 回车
\\* \`paste\`：发送字面文本，可选择使用 bracketed paste mode 包裹
\\* \`kill\`：终止后台会话
\\* \`clear\`：从内存中移除已完成的会话
\\* \`remove\`：如果在运行则终止，否则如果已完成则清除

说明：

\\* 只有后台会话会在内存中列出/持久保存。
\\* 进程重启后会话会丢失（不会持久化到磁盘）。
\\* 只有当你运行 \`process poll/log\` 且工具结果被记录时，会话日志才会保存到聊天历史中。
\\* \`process\` 按智能体划分作用域；它只能看到由该智能体启动的会话。
\\* 当你需要状态、日志、安静成功确认，或在自动完成唤醒不可用时确认完成状态，请使用 \`poll\` / \`log\`。
\\* 当你需要输入或人工干预时，请使用 \`write\` / \`send-keys\` / \`submit\` / \`paste\` / \`kill\`。
\\* \`process list\` 包含一个派生的 \`name\`（命令动词 + 目标），便于快速查看。
\\* \`process log\` 使用基于行的 \`offset\`/\`limit\`。
\\* 当 \`offset\` 和 \`limit\` 都省略时，它会返回最后 200 行，并包含分页提示。
\\* 当提供了 \`offset\` 但省略 \`limit\` 时，它会返回从 \`offset\` 到末尾的内容（不会限制为 200 行）。
\\* 轮询用于按需查看状态，不用于等待循环调度。如果任务应该稍后发生，请改用 cron。

\## 示例

运行一个长任务，稍后轮询：

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ "tool": "exec", "command": "sleep 5 && echo done", "yieldMs": 1000 }
\`\`\`

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ "tool": "process", "action": "poll", "sessionId": "" }
\`\`\`

立即在后台启动：

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ "tool": "exec", "command": "npm run build", "background": true }
\`\`\`

发送 stdin：

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ "tool": "process", "action": "write", "sessionId": "", "data": "y\\n" }
\`\`\`

发送 PTY 按键：

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ "tool": "process", "action": "send-keys", "sessionId": "", "keys": \["C-c"\] }
\`\`\`

提交当前行：

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ "tool": "process", "action": "submit", "sessionId": "" }
\`\`\`

粘贴字面文本：

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ "tool": "process", "action": "paste", "sessionId": "", "text": "line1\\nline2\\n" }
\`\`\`

\## 相关

\\* \[Exec 工具\](/zh-CN/tools/exec)
\\* \[Exec 审批\](/zh-CN/tools/exec-approvals)

---

## 远程 Gateway 网关设置 - OpenClaw

_Source: <https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme>_

# 使用远程 Gateway 网关运行 OpenClaw.app

OpenClaw.app 使用 SSH 隧道连接到远程 Gateway 网关。本指南将向你展示如何进行设置。

## 概览

远程机器

客户端机器

OpenClaw.app

ws://127.0.0.1:18789

（本地端口）

SSH 隧道

Gateway 网关 WebSocket

ws://127.0.0.1:18789

## 快速开始

### 第 1 步：添加 SSH 配置

编辑 `~/.ssh/config` 并添加：

```
Host remote-gateway
    HostName <REMOTE_IP>          # 例如：172.27.187.184
    User <REMOTE_USER>            # 例如：jefferson
    LocalForward 18789 127.0.0.1:18789
    IdentityFile ~/.ssh/id_rsa
```

将 `<REMOTE_IP>` 和 `<REMOTE_USER>` 替换为你的实际值。

### 第 2 步：复制 SSH 密钥

将你的公钥复制到远程机器（输入一次密码）：

```
ssh-copy-id -i ~/.ssh/id_rsa <REMOTE_USER>@<REMOTE_IP>
```

### 第 3 步：配置远程 Gateway 网关认证

```
openclaw config set gateway.remote.token "<your-token>"
```

如果你的远程 Gateway 网关使用密码认证，请改用 `gateway.remote.password`。
`OPENCLAW_GATEWAY_TOKEN` 仍然可作为 shell 级覆盖使用，但持久化的远程客户端设置应使用 `gateway.remote.token` / `gateway.remote.password`。

### 第 4 步：启动 SSH 隧道

```
ssh -N remote-gateway &
```

### 第 5 步：重启 OpenClaw.app

```
# 退出 OpenClaw.app（⌘Q），然后重新打开：
open /path/to/OpenClaw.app
```

应用现在将通过 SSH 隧道连接到远程 Gateway 网关。

* * *

## 登录时自动启动隧道

如果你希望 SSH 隧道在登录时自动启动，可以创建一个 Launch Agent。

### 创建 PLIST 文件

将以下内容保存为 `~/Library/LaunchAgents/ai.openclaw.ssh-tunnel.plist`：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>ai.openclaw.ssh-tunnel</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/ssh</string>
        <string>-N</string>
        <string>remote-gateway</string>
    </array>
    <key>KeepAlive</key>
    <true/>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

### 加载 Launch Agent

```
launchctl bootstrap gui/$UID ~/Library/LaunchAgents/ai.openclaw.ssh-tunnel.plist
```

现在，该隧道将会：

- 在你登录时自动启动
- 如果崩溃会自动重启
- 在后台持续运行

旧版说明：如果存在残留的 `com.openclaw.ssh-tunnel` LaunchAgent，请将其移除。

* * *

## 故障排除

**检查隧道是否正在运行：**

```
ps aux | grep "ssh -N remote-gateway" | grep -v grep
lsof -i :18789
```

**重启隧道：**

```
launchctl kickstart -k gui/$UID/ai.openclaw.ssh-tunnel
```

**停止隧道：**

```
launchctl bootout gui/$UID/ai.openclaw.ssh-tunnel
```

* * *

## 工作原理

| 组件 | 作用 |
| --- | --- |
| `LocalForward 18789 127.0.0.1:18789` | 将本地端口 18789 转发到远程端口 18789 |
| `ssh -N` | SSH 连接但不执行远程命令（仅进行端口转发） |
| `KeepAlive` | 如果隧道崩溃则自动重启 |
| `RunAtLoad` | 在代理加载时启动隧道 |

OpenClaw.app 会连接到你客户端机器上的 `ws://127.0.0.1:18789`。SSH 隧道会将该连接转发到远程机器上的 18789 端口，也就是 Gateway 网关运行的端口。

## 相关内容

- [远程访问](https://docs.openclaw.ai/zh-CN/gateway/remote)
- [Tailscale](https://docs.openclaw.ai/zh-CN/gateway/tailscale)

[远程访问](https://docs.openclaw.ai/zh-CN/gateway/remote) [Tailscale](https://docs.openclaw.ai/zh-CN/gateway/tailscale)

Ctrl+I

---
