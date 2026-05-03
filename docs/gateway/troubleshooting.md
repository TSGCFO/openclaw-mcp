---
source_url: https://docs.openclaw.ai/gateway/troubleshooting
title: "Troubleshooting - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/troubleshooting#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Health and diagnostics

Troubleshooting

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Command ladder](https://docs.openclaw.ai/gateway/troubleshooting#command-ladder)
- [Split brain installs and newer config guard](https://docs.openclaw.ai/gateway/troubleshooting#split-brain-installs-and-newer-config-guard)
- [Anthropic 429 extra usage required for long context](https://docs.openclaw.ai/gateway/troubleshooting#anthropic-429-extra-usage-required-for-long-context)
- [Local OpenAI-compatible backend passes direct probes but agent runs fail](https://docs.openclaw.ai/gateway/troubleshooting#local-openai-compatible-backend-passes-direct-probes-but-agent-runs-fail)
- [No replies](https://docs.openclaw.ai/gateway/troubleshooting#no-replies)
- [Dashboard control UI connectivity](https://docs.openclaw.ai/gateway/troubleshooting#dashboard-control-ui-connectivity)
- [Auth detail codes quick map](https://docs.openclaw.ai/gateway/troubleshooting#auth-detail-codes-quick-map)
- [Gateway service not running](https://docs.openclaw.ai/gateway/troubleshooting#gateway-service-not-running)
- [Gateway restored last-known-good config](https://docs.openclaw.ai/gateway/troubleshooting#gateway-restored-last-known-good-config)
- [Gateway probe warnings](https://docs.openclaw.ai/gateway/troubleshooting#gateway-probe-warnings)
- [Channel connected, messages not flowing](https://docs.openclaw.ai/gateway/troubleshooting#channel-connected-messages-not-flowing)
- [Cron and heartbeat delivery](https://docs.openclaw.ai/gateway/troubleshooting#cron-and-heartbeat-delivery)
- [Node paired, tool fails](https://docs.openclaw.ai/gateway/troubleshooting#node-paired-tool-fails)
- [Browser tool fails](https://docs.openclaw.ai/gateway/troubleshooting#browser-tool-fails)
- [If you upgraded and something suddenly broke](https://docs.openclaw.ai/gateway/troubleshooting#if-you-upgraded-and-something-suddenly-broke)
- [Related](https://docs.openclaw.ai/gateway/troubleshooting#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

This page is the deep runbook. Start at [/help/troubleshooting](https://docs.openclaw.ai/help/troubleshooting) if you want the fast triage flow first.

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#command-ladder)  Command ladder

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#split-brain-installs-and-newer-config-guard)  Split brain installs and newer config guard

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#anthropic-429-extra-usage-required-for-long-context)  Anthropic 429 extra usage required for long context

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#local-openai-compatible-backend-passes-direct-probes-but-agent-runs-fail)  Local OpenAI-compatible backend passes direct probes but agent runs fail

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#no-replies)  No replies

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#dashboard-control-ui-connectivity)  Dashboard control UI connectivity

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

### [​](https://docs.openclaw.ai/gateway/troubleshooting\#auth-detail-codes-quick-map)  Auth detail codes quick map

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#gateway-service-not-running)  Gateway service not running

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#gateway-restored-last-known-good-config)  Gateway restored last-known-good config

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#gateway-probe-warnings)  Gateway probe warnings

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#channel-connected-messages-not-flowing)  Channel connected, messages not flowing

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#cron-and-heartbeat-delivery)  Cron and heartbeat delivery

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#node-paired-tool-fails)  Node paired, tool fails

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#browser-tool-fails)  Browser tool fails

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#if-you-upgraded-and-something-suddenly-broke)  If you upgraded and something suddenly broke

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

## [​](https://docs.openclaw.ai/gateway/troubleshooting\#related)  Related

- [Doctor](https://docs.openclaw.ai/gateway/doctor)
- [FAQ](https://docs.openclaw.ai/help/faq)
- [Gateway runbook](https://docs.openclaw.ai/gateway)

[Diagnostics export](https://docs.openclaw.ai/gateway/diagnostics) [Gateway lock](https://docs.openclaw.ai/gateway/gateway-lock)

Ctrl+I