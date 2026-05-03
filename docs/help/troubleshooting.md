---
source_url: https://docs.openclaw.ai/help/troubleshooting
title: "General troubleshooting - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/help/troubleshooting#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Start here

General troubleshooting

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [First 60 seconds](https://docs.openclaw.ai/help/troubleshooting#first-60-seconds)
- [Anthropic long context 429](https://docs.openclaw.ai/help/troubleshooting#anthropic-long-context-429)
- [Local OpenAI-compatible backend works directly but fails in OpenClaw](https://docs.openclaw.ai/help/troubleshooting#local-openai-compatible-backend-works-directly-but-fails-in-openclaw)
- [Plugin install fails with missing openclaw extensions](https://docs.openclaw.ai/help/troubleshooting#plugin-install-fails-with-missing-openclaw-extensions)
- [Decision tree](https://docs.openclaw.ai/help/troubleshooting#decision-tree)
- [Related](https://docs.openclaw.ai/help/troubleshooting#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

If you only have 2 minutes, use this page as a triage front door.

## [​](https://docs.openclaw.ai/help/troubleshooting\#first-60-seconds)  First 60 seconds

Run this exact ladder in order:

```
openclaw status
openclaw status --all
openclaw gateway probe
openclaw gateway status
openclaw doctor
openclaw channels status --probe
openclaw logs --follow
```

Good output in one line:

- `openclaw status` → shows configured channels and no obvious auth errors.
- `openclaw status --all` → full report is present and shareable.
- `openclaw gateway probe` → expected gateway target is reachable (`Reachable: yes`). `Capability: ...` tells you what auth level the probe could prove, and `Read probe: limited - missing scope: operator.read` is degraded diagnostics, not a connect failure.
- `openclaw gateway status` → `Runtime: running`, `Connectivity probe: ok`, and a plausible `Capability: ...` line. Use `--require-rpc` if you need read-scope RPC proof too.
- `openclaw doctor` → no blocking config/service errors.
- `openclaw channels status --probe` → reachable gateway returns live per-account
transport state plus probe/audit results such as `works` or `audit ok`; if the
gateway is unreachable, the command falls back to config-only summaries.
- `openclaw logs --follow` → steady activity, no repeating fatal errors.

## [​](https://docs.openclaw.ai/help/troubleshooting\#anthropic-long-context-429)  Anthropic long context 429

If you see:
`HTTP 429: rate_limit_error: Extra usage is required for long context requests`,
go to [/gateway/troubleshooting#anthropic-429-extra-usage-required-for-long-context](https://docs.openclaw.ai/gateway/troubleshooting#anthropic-429-extra-usage-required-for-long-context).

## [​](https://docs.openclaw.ai/help/troubleshooting\#local-openai-compatible-backend-works-directly-but-fails-in-openclaw)  Local OpenAI-compatible backend works directly but fails in OpenClaw

If your local or self-hosted `/v1` backend answers small direct
`/v1/chat/completions` probes but fails on `openclaw infer model run` or normal
agent turns:

1. If the error mentions `messages[].content` expecting a string, set
`models.providers.<provider>.models[].compat.requiresStringContent: true`.
2. If the backend still fails only on OpenClaw agent turns, set
`models.providers.<provider>.models[].compat.supportsTools: false` and retry.
3. If tiny direct calls still work but larger OpenClaw prompts crash the
backend, treat the remaining issue as an upstream model/server limitation and
continue in the deep runbook:
[/gateway/troubleshooting#local-openai-compatible-backend-passes-direct-probes-but-agent-runs-fail](https://docs.openclaw.ai/gateway/troubleshooting#local-openai-compatible-backend-passes-direct-probes-but-agent-runs-fail)

## [​](https://docs.openclaw.ai/help/troubleshooting\#plugin-install-fails-with-missing-openclaw-extensions)  Plugin install fails with missing openclaw extensions

If install fails with `package.json missing openclaw.extensions`, the plugin package
is using an old shape that OpenClaw no longer accepts.Fix in the plugin package:

1. Add `openclaw.extensions` to `package.json`.
2. Point entries at built runtime files (usually `./dist/index.js`).
3. Republish the plugin and run `openclaw plugins install <package>` again.

Example:

```
{
  "name": "@openclaw/my-plugin",
  "version": "1.2.3",
  "openclaw": {
    "extensions": ["./dist/index.js"]
  }
}
```

Reference: [Plugin architecture](https://docs.openclaw.ai/plugins/architecture)

## [​](https://docs.openclaw.ai/help/troubleshooting\#decision-tree)  Decision tree

OpenClaw is not working

What breaks first

No replies

Dashboard or Control UI will not connect

Gateway will not start or service not running

Channel connects but messages do not flow

Cron or heartbeat did not fire or did not deliver

Node is paired but camera canvas screen exec fails

Browser tool fails

No replies section

Control UI section

Gateway section

Channel flow section

Automation section

Node tools section

Browser section

No replies

```
openclaw status
openclaw gateway status
openclaw channels status --probe
openclaw pairing list --channel <channel> [--account <id>]
openclaw logs --follow
```

Good output looks like:

- `Runtime: running`
- `Connectivity probe: ok`
- `Capability: read-only`, `write-capable`, or `admin-capable`
- Your channel shows transport connected and, where supported, `works` or `audit ok` in `channels status --probe`
- Sender appears approved (or DM policy is open/allowlist)

Common log signatures:

- `drop guild message (mention required` → mention gating blocked the message in Discord.
- `pairing request` → sender is unapproved and waiting for DM pairing approval.
- `blocked` / `allowlist` in channel logs → sender, room, or group is filtered.

Deep pages:

- [/gateway/troubleshooting#no-replies](https://docs.openclaw.ai/gateway/troubleshooting#no-replies)
- [/channels/troubleshooting](https://docs.openclaw.ai/channels/troubleshooting)
- [/channels/pairing](https://docs.openclaw.ai/channels/pairing)

Dashboard or Control UI will not connect

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

Good output looks like:

- `Dashboard: http://...` is shown in `openclaw gateway status`
- `Connectivity probe: ok`
- `Capability: read-only`, `write-capable`, or `admin-capable`
- No auth loop in logs

Common log signatures:

- `device identity required` → HTTP/non-secure context cannot complete device auth.
- `origin not allowed` → browser `Origin` is not allowed for the Control UI
gateway target.
- `AUTH_TOKEN_MISMATCH` with retry hints (`canRetryWithDeviceToken=true`) → one trusted device-token retry may occur automatically.
- That cached-token retry reuses the cached scope set stored with the paired
device token. Explicit `deviceToken` / explicit `scopes` callers keep
their requested scope set instead.
- On the async Tailscale Serve Control UI path, failed attempts for the same
`{scope, ip}` are serialized before the limiter records the failure, so a
second concurrent bad retry can already show `retry later`.
- `too many failed authentication attempts (retry later)` from a localhost
browser origin → repeated failures from that same `Origin` are temporarily
locked out; another localhost origin uses a separate bucket.
- repeated `unauthorized` after that retry → wrong token/password, auth mode mismatch, or stale paired device token.
- `gateway connect failed:` → UI is targeting the wrong URL/port or unreachable gateway.

Deep pages:

- [/gateway/troubleshooting#dashboard-control-ui-connectivity](https://docs.openclaw.ai/gateway/troubleshooting#dashboard-control-ui-connectivity)
- [/web/control-ui](https://docs.openclaw.ai/web/control-ui)
- [/gateway/authentication](https://docs.openclaw.ai/gateway/authentication)

Gateway will not start or service installed but not running

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

Good output looks like:

- `Service: ... (loaded)`
- `Runtime: running`
- `Connectivity probe: ok`
- `Capability: read-only`, `write-capable`, or `admin-capable`

Common log signatures:

- `Gateway start blocked: set gateway.mode=local` or `existing config is missing gateway.mode` → gateway mode is remote, or the config file is missing the local-mode stamp and should be repaired.
- `refusing to bind gateway ... without auth` → non-loopback bind without a valid gateway auth path (token/password, or trusted-proxy where configured).
- `another gateway instance is already listening` or `EADDRINUSE` → port already taken.

Deep pages:

- [/gateway/troubleshooting#gateway-service-not-running](https://docs.openclaw.ai/gateway/troubleshooting#gateway-service-not-running)
- [/gateway/background-process](https://docs.openclaw.ai/gateway/background-process)
- [/gateway/configuration](https://docs.openclaw.ai/gateway/configuration)

Channel connects but messages do not flow

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

Good output looks like:

- Channel transport is connected.
- Pairing/allowlist checks pass.
- Mentions are detected where required.

Common log signatures:

- `mention required` → group mention gating blocked processing.
- `pairing` / `pending` → DM sender is not approved yet.
- `not_in_channel`, `missing_scope`, `Forbidden`, `401/403` → channel permission token issue.

Deep pages:

- [/gateway/troubleshooting#channel-connected-messages-not-flowing](https://docs.openclaw.ai/gateway/troubleshooting#channel-connected-messages-not-flowing)
- [/channels/troubleshooting](https://docs.openclaw.ai/channels/troubleshooting)

Cron or heartbeat did not fire or did not deliver

```
openclaw status
openclaw gateway status
openclaw cron status
openclaw cron list
openclaw cron runs --id <jobId> --limit 20
openclaw logs --follow
```

Good output looks like:

- `cron.status` shows enabled with a next wake.
- `cron runs` shows recent `ok` entries.
- Heartbeat is enabled and not outside active hours.

Common log signatures:

- `cron: scheduler disabled; jobs will not run automatically` → cron is disabled.
- `heartbeat skipped` with `reason=quiet-hours` → outside configured active hours.
- `heartbeat skipped` with `reason=empty-heartbeat-file` → `HEARTBEAT.md` exists but only contains blank/header-only scaffolding.
- `heartbeat skipped` with `reason=no-tasks-due` → `HEARTBEAT.md` task mode is active but none of the task intervals are due yet.
- `heartbeat skipped` with `reason=alerts-disabled` → all heartbeat visibility is disabled (`showOk`, `showAlerts`, and `useIndicator` are all off).
- `requests-in-flight` → main lane busy; heartbeat wake was deferred.
- `unknown accountId` → heartbeat delivery target account does not exist.

Deep pages:

- [/gateway/troubleshooting#cron-and-heartbeat-delivery](https://docs.openclaw.ai/gateway/troubleshooting#cron-and-heartbeat-delivery)
- [/automation/cron-jobs#troubleshooting](https://docs.openclaw.ai/automation/cron-jobs#troubleshooting)
- [/gateway/heartbeat](https://docs.openclaw.ai/gateway/heartbeat)

Node is paired but tool fails camera canvas screen exec

```
openclaw status
openclaw gateway status
openclaw nodes status
openclaw nodes describe --node <idOrNameOrIp>
openclaw logs --follow
```

Good output looks like:

- Node is listed as connected and paired for role `node`.
- Capability exists for the command you are invoking.
- Permission state is granted for the tool.

Common log signatures:

- `NODE_BACKGROUND_UNAVAILABLE` → bring node app to foreground.
- `*_PERMISSION_REQUIRED` → OS permission was denied/missing.
- `SYSTEM_RUN_DENIED: approval required` → exec approval is pending.
- `SYSTEM_RUN_DENIED: allowlist miss` → command not on exec allowlist.

Deep pages:

- [/gateway/troubleshooting#node-paired-tool-fails](https://docs.openclaw.ai/gateway/troubleshooting#node-paired-tool-fails)
- [/nodes/troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting)
- [/tools/exec-approvals](https://docs.openclaw.ai/tools/exec-approvals)

Exec suddenly asks for approval

```
openclaw config get tools.exec.host
openclaw config get tools.exec.security
openclaw config get tools.exec.ask
openclaw gateway restart
```

What changed:

- If `tools.exec.host` is unset, the default is `auto`.
- `host=auto` resolves to `sandbox` when a sandbox runtime is active, `gateway` otherwise.
- `host=auto` is routing only; the no-prompt “YOLO” behavior comes from `security=full` plus `ask=off` on gateway/node.
- On `gateway` and `node`, unset `tools.exec.security` defaults to `full`.
- Unset `tools.exec.ask` defaults to `off`.
- Result: if you are seeing approvals, some host-local or per-session policy tightened exec away from the current defaults.

Restore current default no-approval behavior:

```
openclaw config set tools.exec.host gateway
openclaw config set tools.exec.security full
openclaw config set tools.exec.ask off
openclaw gateway restart
```

Safer alternatives:

- Set only `tools.exec.host=gateway` if you just want stable host routing.
- Use `security=allowlist` with `ask=on-miss` if you want host exec but still want review on allowlist misses.
- Enable sandbox mode if you want `host=auto` to resolve back to `sandbox`.

Common log signatures:

- `Approval required.` → command is waiting on `/approve ...`.
- `SYSTEM_RUN_DENIED: approval required` → node-host exec approval is pending.
- `exec host=sandbox requires a sandbox runtime for this session` → implicit/explicit sandbox selection but sandbox mode is off.

Deep pages:

- [/tools/exec](https://docs.openclaw.ai/tools/exec)
- [/tools/exec-approvals](https://docs.openclaw.ai/tools/exec-approvals)
- [/gateway/security#what-the-audit-checks-high-level](https://docs.openclaw.ai/gateway/security#what-the-audit-checks-high-level)

Browser tool fails

```
openclaw status
openclaw gateway status
openclaw browser status
openclaw logs --follow
openclaw doctor
```

Good output looks like:

- Browser status shows `running: true` and a chosen browser/profile.
- `openclaw` starts, or `user` can see local Chrome tabs.

Common log signatures:

- `unknown command "browser"` or `unknown command 'browser'` → `plugins.allow` is set and does not include `browser`.
- `Failed to start Chrome CDP on port` → local browser launch failed.
- `browser.executablePath not found` → configured binary path is wrong.
- `browser.cdpUrl must be http(s) or ws(s)` → the configured CDP URL uses an unsupported scheme.
- `browser.cdpUrl has invalid port` → the configured CDP URL has a bad or out-of-range port.
- `No Chrome tabs found for profile="user"` → the Chrome MCP attach profile has no open local Chrome tabs.
- `Remote CDP for profile "<name>" is not reachable` → the configured remote CDP endpoint is not reachable from this host.
- `Browser attachOnly is enabled ... not reachable` or `Browser attachOnly is enabled and CDP websocket ... is not reachable` → attach-only profile has no live CDP target.
- stale viewport / dark-mode / locale / offline overrides on attach-only or remote CDP profiles → run `openclaw browser stop --browser-profile <name>` to close the active control session and release emulation state without restarting the gateway.

Deep pages:

- [/gateway/troubleshooting#browser-tool-fails](https://docs.openclaw.ai/gateway/troubleshooting#browser-tool-fails)
- [/tools/browser#missing-browser-command-or-tool](https://docs.openclaw.ai/tools/browser#missing-browser-command-or-tool)
- [/tools/browser-linux-troubleshooting](https://docs.openclaw.ai/tools/browser-linux-troubleshooting)
- [/tools/browser-wsl2-windows-remote-cdp-troubleshooting](https://docs.openclaw.ai/tools/browser-wsl2-windows-remote-cdp-troubleshooting)

## [​](https://docs.openclaw.ai/help/troubleshooting\#related)  Related

- [FAQ](https://docs.openclaw.ai/help/faq) — frequently asked questions
- [Gateway Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting) — gateway-specific issues
- [Doctor](https://docs.openclaw.ai/gateway/doctor) — automated health checks and repairs
- [Channel Troubleshooting](https://docs.openclaw.ai/channels/troubleshooting) — channel connectivity issues
- [Automation Troubleshooting](https://docs.openclaw.ai/automation/cron-jobs#troubleshooting) — cron and heartbeat issues

[Help](https://docs.openclaw.ai/help) [Debugging](https://docs.openclaw.ai/help/debugging)

Ctrl+I