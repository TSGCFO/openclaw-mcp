---
source_url: https://docs.openclaw.ai/gateway
title: "Gateway runbook - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Gateway

Gateway runbook

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [5-minute local startup](https://docs.openclaw.ai/gateway#5-minute-local-startup)
- [Runtime model](https://docs.openclaw.ai/gateway#runtime-model)
- [OpenAI-compatible endpoints](https://docs.openclaw.ai/gateway#openai-compatible-endpoints)
- [Port and bind precedence](https://docs.openclaw.ai/gateway#port-and-bind-precedence)
- [Hot reload modes](https://docs.openclaw.ai/gateway#hot-reload-modes)
- [Operator command set](https://docs.openclaw.ai/gateway#operator-command-set)
- [Multiple gateways (same host)](https://docs.openclaw.ai/gateway#multiple-gateways-same-host)
- [VoiceClaw real-time brain endpoint](https://docs.openclaw.ai/gateway#voiceclaw-real-time-brain-endpoint)
- [Remote access](https://docs.openclaw.ai/gateway#remote-access)
- [Supervision and service lifecycle](https://docs.openclaw.ai/gateway#supervision-and-service-lifecycle)
- [Dev profile quick path](https://docs.openclaw.ai/gateway#dev-profile-quick-path)
- [Protocol quick reference (operator view)](https://docs.openclaw.ai/gateway#protocol-quick-reference-operator-view)
- [Operational checks](https://docs.openclaw.ai/gateway#operational-checks)
- [Liveness](https://docs.openclaw.ai/gateway#liveness)
- [Readiness](https://docs.openclaw.ai/gateway#readiness)
- [Gap recovery](https://docs.openclaw.ai/gateway#gap-recovery)
- [Common failure signatures](https://docs.openclaw.ai/gateway#common-failure-signatures)
- [Safety guarantees](https://docs.openclaw.ai/gateway#safety-guarantees)
- [Related](https://docs.openclaw.ai/gateway#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Use this page for day-1 startup and day-2 operations of the Gateway service.

[**Deep troubleshooting** \\
\\
Symptom-first diagnostics with exact command ladders and log signatures.](https://docs.openclaw.ai/gateway/troubleshooting)

[**Configuration** \\
\\
Task-oriented setup guide + full configuration reference.](https://docs.openclaw.ai/gateway/configuration)

[**Secrets management** \\
\\
SecretRef contract, runtime snapshot behavior, and migrate/reload operations.](https://docs.openclaw.ai/gateway/secrets)

[**Secrets plan contract** \\
\\
Exact `secrets apply` target/path rules and ref-only auth-profile behavior.](https://docs.openclaw.ai/gateway/secrets-plan-contract)

## [​](https://docs.openclaw.ai/gateway\#5-minute-local-startup)  5-minute local startup

1

[Navigate to header](https://docs.openclaw.ai/gateway#)

Start the Gateway

```
openclaw gateway --port 18789
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

## [​](https://docs.openclaw.ai/gateway\#runtime-model)  Runtime model

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

## [​](https://docs.openclaw.ai/gateway\#openai-compatible-endpoints)  OpenAI-compatible endpoints

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

### [​](https://docs.openclaw.ai/gateway\#port-and-bind-precedence)  Port and bind precedence

| Setting | Resolution order |
| --- | --- |
| Gateway port | `--port` → `OPENCLAW_GATEWAY_PORT` → `gateway.port` → `18789` |
| Bind mode | CLI/override → `gateway.bind` → `loopback` |

Installed gateway services record the resolved `--port` in supervisor metadata. After changing `gateway.port`, run `openclaw doctor --fix` or `openclaw gateway install --force` so launchd/systemd/schtasks starts the process on the new port.Gateway startup uses the same effective port and bind when it seeds local
Control UI origins for non-loopback binds. For example, `--bind lan --port 3000`
seeds `http://localhost:3000` and `http://127.0.0.1:3000` before runtime
validation runs. Add any remote browser origins, such as HTTPS proxy URLs, to
`gateway.controlUi.allowedOrigins` explicitly.

### [​](https://docs.openclaw.ai/gateway\#hot-reload-modes)  Hot reload modes

| `gateway.reload.mode` | Behavior |
| --- | --- |
| `off` | No config reload |
| `hot` | Apply only hot-safe changes |
| `restart` | Restart on reload-required changes |
| `hybrid` (default) | Hot-apply when safe, restart when required |

## [​](https://docs.openclaw.ai/gateway\#operator-command-set)  Operator command set

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

## [​](https://docs.openclaw.ai/gateway\#multiple-gateways-same-host)  Multiple gateways (same host)

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

## [​](https://docs.openclaw.ai/gateway\#voiceclaw-real-time-brain-endpoint)  VoiceClaw real-time brain endpoint

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

## [​](https://docs.openclaw.ai/gateway\#remote-access)  Remote access

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

## [​](https://docs.openclaw.ai/gateway\#supervision-and-service-lifecycle)  Supervision and service lifecycle

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

## [​](https://docs.openclaw.ai/gateway\#dev-profile-quick-path)  Dev profile quick path

```
openclaw --dev setup
openclaw --dev gateway --allow-unconfigured
openclaw --dev status
```

Defaults include isolated state/config and base gateway port `19001`.

## [​](https://docs.openclaw.ai/gateway\#protocol-quick-reference-operator-view)  Protocol quick reference (operator view)

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

## [​](https://docs.openclaw.ai/gateway\#operational-checks)  Operational checks

### [​](https://docs.openclaw.ai/gateway\#liveness)  Liveness

- Open WS and send `connect`.
- Expect `hello-ok` response with snapshot.

### [​](https://docs.openclaw.ai/gateway\#readiness)  Readiness

```
openclaw gateway status
openclaw channels status --probe
openclaw health
```

### [​](https://docs.openclaw.ai/gateway\#gap-recovery)  Gap recovery

Events are not replayed. On sequence gaps, refresh state (`health`, `system-presence`) before continuing.

## [​](https://docs.openclaw.ai/gateway\#common-failure-signatures)  Common failure signatures

| Signature | Likely issue |
| --- | --- |
| `refusing to bind gateway ... without auth` | Non-loopback bind without a valid gateway auth path |
| `another gateway instance is already listening` / `EADDRINUSE` | Port conflict |
| `Gateway start blocked: set gateway.mode=local` | Config set to remote mode, or local-mode stamp is missing from a damaged config |
| `unauthorized` during connect | Auth mismatch between client and gateway |

For full diagnosis ladders, use [Gateway Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting).

## [​](https://docs.openclaw.ai/gateway\#safety-guarantees)  Safety guarantees

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

## [​](https://docs.openclaw.ai/gateway\#related)  Related

- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)
- [Remote access](https://docs.openclaw.ai/gateway/remote)
- [Secrets management](https://docs.openclaw.ai/gateway/secrets)

[Configuration](https://docs.openclaw.ai/gateway/configuration)

Ctrl+I