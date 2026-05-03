---
source_url: https://docs.openclaw.ai/cli/gateway
title: "Gateway - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/gateway#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Gateway and service

Gateway

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Run the Gateway](https://docs.openclaw.ai/cli/gateway#run-the-gateway)
- [Options](https://docs.openclaw.ai/cli/gateway#options)
- [Startup profiling](https://docs.openclaw.ai/cli/gateway#startup-profiling)
- [Query a running Gateway](https://docs.openclaw.ai/cli/gateway#query-a-running-gateway)
- [gateway health](https://docs.openclaw.ai/cli/gateway#gateway-health)
- [gateway usage-cost](https://docs.openclaw.ai/cli/gateway#gateway-usage-cost)
- [gateway stability](https://docs.openclaw.ai/cli/gateway#gateway-stability)
- [gateway diagnostics export](https://docs.openclaw.ai/cli/gateway#gateway-diagnostics-export)
- [gateway status](https://docs.openclaw.ai/cli/gateway#gateway-status)
- [gateway probe](https://docs.openclaw.ai/cli/gateway#gateway-probe)
- [Remote over SSH (Mac app parity)](https://docs.openclaw.ai/cli/gateway#remote-over-ssh-mac-app-parity)
- [gateway call <method>](https://docs.openclaw.ai/cli/gateway#gateway-call-%3Cmethod%3E)
- [Manage the Gateway service](https://docs.openclaw.ai/cli/gateway#manage-the-gateway-service)
- [Install with a wrapper](https://docs.openclaw.ai/cli/gateway#install-with-a-wrapper)
- [Discover gateways (Bonjour)](https://docs.openclaw.ai/cli/gateway#discover-gateways-bonjour)
- [gateway discover](https://docs.openclaw.ai/cli/gateway#gateway-discover)
- [Related](https://docs.openclaw.ai/cli/gateway#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

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

## [​](https://docs.openclaw.ai/cli/gateway\#run-the-gateway)  Run the Gateway

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

### [​](https://docs.openclaw.ai/cli/gateway\#options)  Options

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

[​](https://docs.openclaw.ai/cli/gateway#param-compact)

--compact

boolean

Alias for `--ws-log compact`.

[​](https://docs.openclaw.ai/cli/gateway#param-raw-stream)

--raw-stream

boolean

Log raw model stream events to jsonl.

[​](https://docs.openclaw.ai/cli/gateway#param-raw-stream-path-path)

--raw-stream-path <path>

string

Raw stream jsonl path.

Inline `--password` can be exposed in local process listings. Prefer `--password-file`, env, or a SecretRef-backed `gateway.auth.password`.

### [​](https://docs.openclaw.ai/cli/gateway\#startup-profiling)  Startup profiling

- Set `OPENCLAW_GATEWAY_STARTUP_TRACE=1` to log phase timings during Gateway startup, including per-phase `eventLoopMax` delay and plugin lookup-table timings for installed-index, manifest registry, startup planning, and owner-map work.
- Set `OPENCLAW_DIAGNOSTICS=timeline` with `OPENCLAW_DIAGNOSTICS_TIMELINE_PATH=<path>` to write a best-effort JSONL startup diagnostics timeline for external QA harnesses. You can also enable the flag with `diagnostics.flags: ["timeline"]` in config; the path is still env-provided. Add `OPENCLAW_DIAGNOSTICS_EVENT_LOOP=1` to include event-loop samples.
- Run `pnpm test:startup:gateway -- --runs 5 --warmup 1` to benchmark Gateway startup. The benchmark records first process output, `/healthz`, `/readyz`, startup trace timings, event-loop delay, and plugin lookup-table timing details.

## [​](https://docs.openclaw.ai/cli/gateway\#query-a-running-gateway)  Query a running Gateway

All query commands use WebSocket RPC.

- Output modes

- Shared options


- Default: human-readable (colored in TTY).
- `--json`: machine-readable JSON (no styling/spinner).
- `--no-color` (or `NO_COLOR=1`): disable ANSI while keeping human layout.

- `--url <url>`: Gateway WebSocket URL.
- `--token <token>`: Gateway token.
- `--password <password>`: Gateway password.
- `--timeout <ms>`: timeout/budget (varies per command).
- `--expect-final`: wait for a “final” response (agent calls).

When you set `--url`, the CLI does not fall back to config or environment credentials. Pass `--token` or `--password` explicitly. Missing explicit credentials is an error.

### [​](https://docs.openclaw.ai/cli/gateway\#gateway-health)  `gateway health`

```
openclaw gateway health --url ws://127.0.0.1:18789
```

The HTTP `/healthz` endpoint is a liveness probe: it returns once the server can answer HTTP. The HTTP `/readyz` endpoint is stricter and stays red while startup plugin sidecars, channels, or configured hooks are still settling. Local or authenticated detailed readiness responses include an `eventLoop` diagnostic block with event-loop delay, event-loop utilization, CPU core ratio, and a `degraded` flag.

### [​](https://docs.openclaw.ai/cli/gateway\#gateway-usage-cost)  `gateway usage-cost`

Fetch usage-cost summaries from session logs.

```
openclaw gateway usage-cost
openclaw gateway usage-cost --days 7
openclaw gateway usage-cost --json
```

[​](https://docs.openclaw.ai/cli/gateway#param-days-days)

--days <days>

number

default:"30"

Number of days to include.

### [​](https://docs.openclaw.ai/cli/gateway\#gateway-stability)  `gateway stability`

Fetch the recent diagnostic stability recorder from a running Gateway.

```
openclaw gateway stability
openclaw gateway stability --type payload.large
openclaw gateway stability --bundle latest
openclaw gateway stability --bundle latest --export
openclaw gateway stability --json
```

[​](https://docs.openclaw.ai/cli/gateway#param-limit-limit)

--limit <limit>

number

default:"25"

Maximum number of recent events to include (max `1000`).

[​](https://docs.openclaw.ai/cli/gateway#param-type-type)

--type <type>

string

Filter by diagnostic event type, such as `payload.large` or `diagnostic.memory.pressure`.

[​](https://docs.openclaw.ai/cli/gateway#param-since-seq-seq)

--since-seq <seq>

number

Include only events after a diagnostic sequence number.

[​](https://docs.openclaw.ai/cli/gateway#param-bundle-path)

--bundle \[path\]

string

Read a persisted stability bundle instead of calling the running Gateway. Use `--bundle latest` (or just `--bundle`) for the newest bundle under the state directory, or pass a bundle JSON path directly.

[​](https://docs.openclaw.ai/cli/gateway#param-export)

--export

boolean

Write a shareable support diagnostics zip instead of printing stability details.

[​](https://docs.openclaw.ai/cli/gateway#param-output-path)

--output <path>

string

Output path for `--export`.

Privacy and bundle behavior

- Records keep operational metadata: event names, counts, byte sizes, memory readings, queue/session state, channel/plugin names, and redacted session summaries. They do not keep chat text, webhook bodies, tool outputs, raw request or response bodies, tokens, cookies, secret values, hostnames, or raw session ids. Set `diagnostics.enabled: false` to disable the recorder entirely.
- On fatal Gateway exits, shutdown timeouts, and restart startup failures, OpenClaw writes the same diagnostic snapshot to `~/.openclaw/logs/stability/openclaw-stability-*.json` when the recorder has events. Inspect the newest bundle with `openclaw gateway stability --bundle latest`; `--limit`, `--type`, and `--since-seq` also apply to bundle output.

### [​](https://docs.openclaw.ai/cli/gateway\#gateway-diagnostics-export)  `gateway diagnostics export`

Write a local diagnostics zip that is designed to attach to bug reports. For the privacy model and bundle contents, see [Diagnostics Export](https://docs.openclaw.ai/gateway/diagnostics).

```
openclaw gateway diagnostics export
openclaw gateway diagnostics export --output openclaw-diagnostics.zip
openclaw gateway diagnostics export --json
```

[​](https://docs.openclaw.ai/cli/gateway#param-output-path-1)

--output <path>

string

Output zip path. Defaults to a support export under the state directory.

[​](https://docs.openclaw.ai/cli/gateway#param-log-lines-count)

--log-lines <count>

number

default:"5000"

Maximum sanitized log lines to include.

[​](https://docs.openclaw.ai/cli/gateway#param-log-bytes-bytes)

--log-bytes <bytes>

number

default:"1000000"

Maximum log bytes to inspect.

[​](https://docs.openclaw.ai/cli/gateway#param-url-url)

--url <url>

string

Gateway WebSocket URL for the health snapshot.

[​](https://docs.openclaw.ai/cli/gateway#param-token-token-1)

--token <token>

string

Gateway token for the health snapshot.

[​](https://docs.openclaw.ai/cli/gateway#param-password-password-1)

--password <password>

string

Gateway password for the health snapshot.

[​](https://docs.openclaw.ai/cli/gateway#param-timeout-ms)

--timeout <ms>

number

default:"3000"

Status/health snapshot timeout.

[​](https://docs.openclaw.ai/cli/gateway#param-no-stability-bundle)

--no-stability-bundle

boolean

Skip persisted stability bundle lookup.

[​](https://docs.openclaw.ai/cli/gateway#param-json)

--json

boolean

Print the written path, size, and manifest as JSON.

The export contains a manifest, a Markdown summary, config shape, sanitized config details, sanitized log summaries, sanitized Gateway status/health snapshots, and the newest stability bundle when one exists.It is meant to be shared. It keeps operational details that help debugging, such as safe OpenClaw log fields, subsystem names, status codes, durations, configured modes, ports, plugin ids, provider ids, non-secret feature settings, and redacted operational log messages. It omits or redacts chat text, webhook bodies, tool outputs, credentials, cookies, account/message identifiers, prompt/instruction text, hostnames, and secret values. When a LogTape-style message looks like user/chat/tool payload text, the export keeps only that a message was omitted plus its byte count.

### [​](https://docs.openclaw.ai/cli/gateway\#gateway-status)  `gateway status`

`gateway status` shows the Gateway service (launchd/systemd/schtasks) plus an optional probe of connectivity/auth capability.

```
openclaw gateway status
openclaw gateway status --json
openclaw gateway status --require-rpc
```

[​](https://docs.openclaw.ai/cli/gateway#param-url-url-1)

--url <url>

string

Add an explicit probe target. Configured remote + localhost are still probed.

[​](https://docs.openclaw.ai/cli/gateway#param-token-token-2)

--token <token>

string

Token auth for the probe.

[​](https://docs.openclaw.ai/cli/gateway#param-password-password-2)

--password <password>

string

Password auth for the probe.

[​](https://docs.openclaw.ai/cli/gateway#param-timeout-ms-1)

--timeout <ms>

number

default:"10000"

Probe timeout.

[​](https://docs.openclaw.ai/cli/gateway#param-no-probe)

--no-probe

boolean

Skip the connectivity probe (service-only view).

[​](https://docs.openclaw.ai/cli/gateway#param-deep)

--deep

boolean

Scan system-level services too.

[​](https://docs.openclaw.ai/cli/gateway#param-require-rpc)

--require-rpc

boolean

Upgrade the default connectivity probe to a read probe and exit non-zero when that read probe fails. Cannot be combined with `--no-probe`.

Status semantics

- `gateway status` stays available for diagnostics even when the local CLI config is missing or invalid.
- Default `gateway status` proves service state, WebSocket connect, and the auth capability visible at handshake time. It does not prove read/write/admin operations.
- Diagnostic probes are non-mutating for first-time device auth: they reuse an existing cached device token when one exists, but they do not create a new CLI device identity or read-only device pairing record just to check status.
- `gateway status` resolves configured auth SecretRefs for probe auth when possible.
- If a required auth SecretRef is unresolved in this command path, `gateway status --json` reports `rpc.authWarning` when probe connectivity/auth fails; pass `--token`/`--password` explicitly or resolve the secret source first.
- If the probe succeeds, unresolved auth-ref warnings are suppressed to avoid false positives.
- Use `--require-rpc` in scripts and automation when a listening service is not enough and you need read-scope RPC calls to be healthy too.
- `--deep` adds a best-effort scan for extra launchd/systemd/schtasks installs. When multiple gateway-like services are detected, human output prints cleanup hints and warns that most setups should run one gateway per machine.
- Human output includes the resolved file log path plus the CLI-vs-service config paths/validity snapshot to help diagnose profile or state-dir drift.

Linux systemd auth-drift checks

- On Linux systemd installs, service auth drift checks read both `Environment=` and `EnvironmentFile=` values from the unit (including `%h`, quoted paths, multiple files, and optional `-` files).
- Drift checks resolve `gateway.auth.token` SecretRefs using merged runtime env (service command env first, then process env fallback).
- If token auth is not effectively active (explicit `gateway.auth.mode` of `password`/`none`/`trusted-proxy`, or mode unset where password can win and no token candidate can win), token-drift checks skip config token resolution.

### [​](https://docs.openclaw.ai/cli/gateway\#gateway-probe)  `gateway probe`

`gateway probe` is the “debug everything” command. It always probes:

- your configured remote gateway (if set), and
- localhost (loopback) **even if remote is configured**.

If you pass `--url`, that explicit target is added ahead of both. Human output labels the targets as:

- `URL (explicit)`
- `Remote (configured)` or `Remote (configured, inactive)`
- `Local loopback`

If multiple gateways are reachable, it prints all of them. Multiple gateways are supported when you use isolated profiles/ports (e.g., a rescue bot), but most installs still run a single gateway.

```
openclaw gateway probe
openclaw gateway probe --json
```

Interpretation

- `Reachable: yes` means at least one target accepted a WebSocket connect.
- `Capability: read-only|write-capable|admin-capable|pairing-pending|connect-only` reports what the probe could prove about auth. It is separate from reachability.
- `Read probe: ok` means read-scope detail RPC calls (`health`/`status`/`system-presence`/`config.get`) also succeeded.
- `Read probe: limited - missing scope: operator.read` means connect succeeded but read-scope RPC is limited. This is reported as **degraded** reachability, not full failure.
- `Read probe: failed` after `Connect: ok` means the Gateway accepted the WebSocket connection, but follow-up read diagnostics timed out or failed. This is also **degraded** reachability, not an unreachable Gateway.
- Like `gateway status`, probe reuses existing cached device auth but does not create first-time device identity or pairing state.
- Exit code is non-zero only when no probed target is reachable.

JSON output

Top level:

- `ok`: at least one target is reachable.
- `degraded`: at least one target accepted a connection but did not complete full detail RPC diagnostics.
- `capability`: best capability seen across reachable targets (`read_only`, `write_capable`, `admin_capable`, `pairing_pending`, `connected_no_operator_scope`, or `unknown`).
- `primaryTargetId`: best target to treat as the active winner in this order: explicit URL, SSH tunnel, configured remote, then local loopback.
- `warnings[]`: best-effort warning records with `code`, `message`, and optional `targetIds`.
- `network`: local loopback/tailnet URL hints derived from current config and host networking.
- `discovery.timeoutMs` and `discovery.count`: the actual discovery budget/result count used for this probe pass.

Per target (`targets[].connect`):

- `ok`: reachability after connect + degraded classification.
- `rpcOk`: full detail RPC success.
- `scopeLimited`: detail RPC failed due to missing operator scope.

Per target (`targets[].auth`):

- `role`: auth role reported in `hello-ok` when available.
- `scopes`: granted scopes reported in `hello-ok` when available.
- `capability`: the surfaced auth capability classification for that target.

Common warning codes

- `ssh_tunnel_failed`: SSH tunnel setup failed; the command fell back to direct probes.
- `multiple_gateways`: more than one target was reachable; this is unusual unless you intentionally run isolated profiles, such as a rescue bot.
- `auth_secretref_unresolved`: a configured auth SecretRef could not be resolved for a failed target.
- `probe_scope_limited`: WebSocket connect succeeded, but the read probe was limited by missing `operator.read`.

#### [​](https://docs.openclaw.ai/cli/gateway\#remote-over-ssh-mac-app-parity)  Remote over SSH (Mac app parity)

The macOS app “Remote over SSH” mode uses a local port-forward so the remote gateway (which may be bound to loopback only) becomes reachable at `ws://127.0.0.1:<port>`.CLI equivalent:

```
openclaw gateway probe --ssh user@gateway-host
```

[​](https://docs.openclaw.ai/cli/gateway#param-ssh-target)

--ssh <target>

string

`user@host` or `user@host:port` (port defaults to `22`).

[​](https://docs.openclaw.ai/cli/gateway#param-ssh-identity-path)

--ssh-identity <path>

string

Identity file.

[​](https://docs.openclaw.ai/cli/gateway#param-ssh-auto)

--ssh-auto

boolean

Pick the first discovered gateway host as SSH target from the resolved discovery endpoint (`local.` plus the configured wide-area domain, if any). TXT-only hints are ignored.

Config (optional, used as defaults):

- `gateway.remote.sshTarget`
- `gateway.remote.sshIdentity`

### [​](https://docs.openclaw.ai/cli/gateway\#gateway-call-%3Cmethod%3E)  `gateway call <method>`

Low-level RPC helper.

```
openclaw gateway call status
openclaw gateway call logs.tail --params '{"sinceMs": 60000}'
```

[​](https://docs.openclaw.ai/cli/gateway#param-params-json)

--params <json>

string

default:"{}"

JSON object string for params.

[​](https://docs.openclaw.ai/cli/gateway#param-url-url-2)

--url <url>

string

Gateway WebSocket URL.

[​](https://docs.openclaw.ai/cli/gateway#param-token-token-3)

--token <token>

string

Gateway token.

[​](https://docs.openclaw.ai/cli/gateway#param-password-password-3)

--password <password>

string

Gateway password.

[​](https://docs.openclaw.ai/cli/gateway#param-timeout-ms-2)

--timeout <ms>

number

Timeout budget.

[​](https://docs.openclaw.ai/cli/gateway#param-expect-final)

--expect-final

boolean

Mainly for agent-style RPCs that stream intermediate events before a final payload.

[​](https://docs.openclaw.ai/cli/gateway#param-json-1)

--json

boolean

Machine-readable JSON output.

`--params` must be valid JSON.

## [​](https://docs.openclaw.ai/cli/gateway\#manage-the-gateway-service)  Manage the Gateway service

```
openclaw gateway install
openclaw gateway start
openclaw gateway stop
openclaw gateway restart
openclaw gateway uninstall
```

### [​](https://docs.openclaw.ai/cli/gateway\#install-with-a-wrapper)  Install with a wrapper

Use `--wrapper` when the managed service must start through another executable, for example a
secrets manager shim or a run-as helper. The wrapper receives the normal Gateway args and is
responsible for eventually exec’ing `openclaw` or Node with those args.

```
cat > ~/.local/bin/openclaw-doppler <<'EOF'
#!/usr/bin/env bash
set -euo pipefail
exec doppler run --project my-project --config production -- openclaw "$@"
EOF
chmod +x ~/.local/bin/openclaw-doppler

openclaw gateway install --wrapper ~/.local/bin/openclaw-doppler --force
openclaw gateway restart
```

You can also set the wrapper through the environment. `gateway install` validates that the path is
an executable file, writes the wrapper into service `ProgramArguments`, and persists
`OPENCLAW_WRAPPER` in the service environment for later forced reinstalls, updates, and doctor
repairs.

```
OPENCLAW_WRAPPER="$HOME/.local/bin/openclaw-doppler" openclaw gateway install --force
openclaw doctor
```

To remove a persisted wrapper, clear `OPENCLAW_WRAPPER` while reinstalling:

```
OPENCLAW_WRAPPER= openclaw gateway install --force
openclaw gateway restart
```

Command options

- `gateway status`: `--url`, `--token`, `--password`, `--timeout`, `--no-probe`, `--require-rpc`, `--deep`, `--json`
- `gateway install`: `--port`, `--runtime <node|bun>`, `--token`, `--wrapper <path>`, `--force`, `--json`
- `gateway restart`: `--force`, `--wait <duration>`, `--json`
- `gateway uninstall|start|stop`: `--json`

Lifecycle behavior

- Use `gateway restart` to restart a managed service. Do not chain `gateway stop` and `gateway start` as a restart substitute; on macOS, `gateway stop` intentionally disables the LaunchAgent before stopping it.
- `gateway restart --wait 30s` overrides the configured restart drain budget for that restart. Bare numbers are milliseconds; units such as `s`, `m`, and `h` are accepted. `--wait 0` waits indefinitely.
- `gateway restart --force` skips the active-work drain and restarts immediately. Use it when an operator has already inspected the listed task blockers and wants the gateway back now.
- Lifecycle commands accept `--json` for scripting.

Auth and SecretRefs at install time

- When token auth requires a token and `gateway.auth.token` is SecretRef-managed, `gateway install` validates that the SecretRef is resolvable but does not persist the resolved token into service environment metadata.
- If token auth requires a token and the configured token SecretRef is unresolved, install fails closed instead of persisting fallback plaintext.
- For password auth on `gateway run`, prefer `OPENCLAW_GATEWAY_PASSWORD`, `--password-file`, or a SecretRef-backed `gateway.auth.password` over inline `--password`.
- In inferred auth mode, shell-only `OPENCLAW_GATEWAY_PASSWORD` does not relax install token requirements; use durable config (`gateway.auth.password` or config `env`) when installing a managed service.
- If both `gateway.auth.token` and `gateway.auth.password` are configured and `gateway.auth.mode` is unset, install is blocked until mode is set explicitly.

## [​](https://docs.openclaw.ai/cli/gateway\#discover-gateways-bonjour)  Discover gateways (Bonjour)

`gateway discover` scans for Gateway beacons (`_openclaw-gw._tcp`).

- Multicast DNS-SD: `local.`
- Unicast DNS-SD (Wide-Area Bonjour): choose a domain (example: `openclaw.internal.`) and set up split DNS + a DNS server; see [Bonjour](https://docs.openclaw.ai/gateway/bonjour).

Only gateways with Bonjour discovery enabled (default) advertise the beacon.Wide-Area discovery records include (TXT):

- `role` (gateway role hint)
- `transport` (transport hint, e.g. `gateway`)
- `gatewayPort` (WebSocket port, usually `18789`)
- `sshPort` (optional; clients default SSH targets to `22` when it is absent)
- `tailnetDns` (MagicDNS hostname, when available)
- `gatewayTls` / `gatewayTlsSha256` (TLS enabled + cert fingerprint)
- `cliPath` (remote-install hint written to the wide-area zone)

### [​](https://docs.openclaw.ai/cli/gateway\#gateway-discover)  `gateway discover`

```
openclaw gateway discover
```

[​](https://docs.openclaw.ai/cli/gateway#param-timeout-ms-3)

--timeout <ms>

number

default:"2000"

Per-command timeout (browse/resolve).

[​](https://docs.openclaw.ai/cli/gateway#param-json-2)

--json

boolean

Machine-readable output (also disables styling/spinner).

Examples:

```
openclaw gateway discover --timeout 4000
openclaw gateway discover --json | jq '.beacons[].wsUrl'
```

- The CLI scans `local.` plus the configured wide-area domain when one is enabled.
- `wsUrl` in JSON output is derived from the resolved service endpoint, not from TXT-only hints such as `lanHost` or `tailnetDns`.
- On `local.` mDNS, `sshPort` and `cliPath` are only broadcast when `discovery.mdns.mode` is `full`. Wide-area DNS-SD still writes `cliPath`; `sshPort` stays optional there too.

## [​](https://docs.openclaw.ai/cli/gateway\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Gateway runbook](https://docs.openclaw.ai/gateway)

[Doctor](https://docs.openclaw.ai/cli/doctor) [Health](https://docs.openclaw.ai/cli/health)

Ctrl+I