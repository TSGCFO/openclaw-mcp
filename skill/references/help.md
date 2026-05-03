# Help

_8 pages from docs.openclaw.ai_


---

## Help - OpenClaw

_Source: <https://docs.openclaw.ai/help>_

[OpenClaw home page](https://docs.openclaw.ai/)

Start here

Help

Quick “get unstuck” path for the most common problems:

- [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) — symptom-first decision tree
- [Debugging](https://docs.openclaw.ai/help/debugging) — watch mode, raw streams, dev profile
- [Install sanity](https://docs.openclaw.ai/install/node#troubleshooting) — Node / npm / PATH checks
- [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting) — gateway-specific issues
- [Doctor](https://docs.openclaw.ai/gateway/doctor) — automated repair + diagnostic bundle

## FAQ

- [FAQ](https://docs.openclaw.ai/help/faq) — day-to-day concepts and operational questions
- [First-run FAQ](https://docs.openclaw.ai/help/faq-first-run) — install, onboard, auth, subscriptions, early failures
- [Models FAQ](https://docs.openclaw.ai/help/faq-models) — model selection, failover, auth profiles

## Diagnostics

- [Environment variables](https://docs.openclaw.ai/help/environment) — where OpenClaw loads env vars and precedence
- [Diagnostics flags](https://docs.openclaw.ai/diagnostics/flags) — runtime diagnostics and verbose modes
- [Node + tsx crash](https://docs.openclaw.ai/debug/node-issue) — specific Node / tsx runtime crash scenarios

## Testing

- [Testing](https://docs.openclaw.ai/help/testing) — test suites and Docker runners
- [Update and plugin tests](https://docs.openclaw.ai/help/testing-updates-plugins) — package update, migration, and plugin install validation
- [Live tests](https://docs.openclaw.ai/help/testing-live) — network-touching provider and CLI smokes

## Community and meta

- [OpenClaw lore](https://docs.openclaw.ai/start/lore) — the story
- [Docs hubs](https://docs.openclaw.ai/start/hubs) — how this documentation is organized
- [Docs directory](https://docs.openclaw.ai/start/docs-directory) — full file map

[General troubleshooting](https://docs.openclaw.ai/help/troubleshooting)

Ctrl+I


---

## Debugging - OpenClaw

_Source: <https://docs.openclaw.ai/help/debugging>_

# or
OPENCLAW_GATEWAY_WATCH_TMUX=0 pnpm gateway:watch
```

Disable auto-attach while keeping tmux management:

```
OPENCLAW_GATEWAY_WATCH_ATTACH=0 pnpm gateway:watch
```

Profile watched Gateway CPU time when debugging startup/runtime hotspots:

```
pnpm gateway:watch --benchmark
```

The watch wrapper consumes `--benchmark` before invoking the Gateway and writes
one V8 `.cpuprofile` per Gateway child exit under
`.artifacts/gateway-watch-profiles/`. Stop or restart the watched gateway to
flush the current profile, then open it with Chrome DevTools or Speedscope:

```
npx speedscope .artifacts/gateway-watch-profiles/*.cpuprofile
```

Use `--benchmark-dir <path>` when you want profiles somewhere else.
Use `--benchmark-no-force` when you want the benchmarked child to skip the
default `--force` port cleanup and fail fast if the Gateway port is already in
use.The tmux wrapper carries common non-secret runtime selectors such as
`OPENCLAW_PROFILE`, `OPENCLAW_CONFIG_PATH`, `OPENCLAW_STATE_DIR`,
`OPENCLAW_GATEWAY_PORT`, and `OPENCLAW_SKIP_CHANNELS` into the pane. Put
provider credentials in your normal profile/config, or use raw foreground mode
for one-off ephemeral secrets.
The managed tmux pane also defaults to colored Gateway logs for readability;
set `FORCE_COLOR=0` when starting `pnpm gateway:watch` to disable ANSI output.The watcher restarts on build-relevant files under `src/`, extension source files,
extension `package.json` and `openclaw.plugin.json` metadata, `tsconfig.json`,
`package.json`, and `tsdown.config.ts`. Extension metadata changes restart the
gateway without forcing a `tsdown` rebuild; source and config changes still
rebuild `dist` first.Add any gateway CLI flags after `gateway:watch` and they will be passed through on
each restart. Re-running the same watch command respawns the named tmux pane, and
the raw watcher still keeps its single-watcher lock so duplicate watcher parents
are replaced instead of piling up.

## Dev profile + dev gateway (—dev)

Use the dev profile to isolate state and spin up a safe, disposable setup for
debugging. There are **two**`--dev` flags:

- **Global `--dev` (profile):** isolates state under `~/.openclaw-dev` and
defaults the gateway port to `19001` (derived ports shift with it).
- **`gateway --dev`: tells the Gateway to auto-create a default config +**
**workspace** when missing (and skip BOOTSTRAP.md).

Recommended flow (dev profile + dev bootstrap):

```
pnpm gateway:dev
OPENCLAW_PROFILE=dev openclaw tui
```

If you don’t have a global install yet, run the CLI via `pnpm openclaw ...`.What this does:

1. **Profile isolation** (global `--dev`)   - `OPENCLAW_PROFILE=dev`
   - `OPENCLAW_STATE_DIR=~/.openclaw-dev`
   - `OPENCLAW_CONFIG_PATH=~/.openclaw-dev/openclaw.json`
   - `OPENCLAW_GATEWAY_PORT=19001` (browser/canvas shift accordingly)
2. **Dev bootstrap** (`gateway --dev`)   - Writes a minimal config if missing (`gateway.mode=local`, bind loopback).
   - Sets `agent.workspace` to the dev workspace.
   - Sets `agent.skipBootstrap=true` (no BOOTSTRAP.md).
   - Seeds the workspace files if missing:
     `AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`.
   - Default identity: **C3‑PO** (protocol droid).
   - Skips channel providers in dev mode (`OPENCLAW_SKIP_CHANNELS=1`).

Reset flow (fresh start):

```
pnpm gateway:dev:reset
```

`--dev` is a **global** profile flag and gets eaten by some runners. If you need to spell it out, use the env var form:

```
OPENCLAW_PROFILE=dev openclaw gateway --dev --reset
```

`--reset` wipes config, credentials, sessions, and the dev workspace (using
`trash`, not `rm`), then recreates the default dev setup.

If a non-dev gateway is already running (launchd or systemd), stop it first:

```
openclaw gateway stop
```

## Raw stream logging (OpenClaw)

OpenClaw can log the **raw assistant stream** before any filtering/formatting.
This is the best way to see whether reasoning is arriving as plain text deltas
(or as separate thin

_… [truncated; see https://docs.openclaw.ai/help/debugging for full content]_


---

## Environment variables - OpenClaw

_Source: <https://docs.openclaw.ai/help/environment>_

[OpenClaw home page](https://docs.openclaw.ai/)

Diagnostics

Environment variables

OpenClaw pulls environment variables from multiple sources. The rule is **never override existing values**.

## Precedence (highest → lowest)

1. **Process environment** (what the Gateway process already has from the parent shell/daemon).
2. **`.env` in the current working directory** (dotenv default; does not override).
3. **Global `.env`** at `~/.openclaw/.env` (aka `$OPENCLAW_STATE_DIR/.env`; does not override).
4. **Config `env` block** in `~/.openclaw/openclaw.json` (applied only if missing).
5. **Optional login-shell import** (`env.shellEnv.enabled` or `OPENCLAW_LOAD_SHELL_ENV=1`), applied only for missing expected keys.

On Ubuntu fresh installs that use the default state dir, OpenClaw also treats `~/.config/openclaw/gateway.env` as a compatibility fallback after the global `.env`. If both files exist and disagree, OpenClaw keeps `~/.openclaw/.env` and prints a warning.If the config file is missing entirely, step 4 is skipped; shell import still runs if enabled.

## Config `env` block

Two equivalent ways to set inline env vars (both are non-overriding):

```
{
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: {
      GROQ_API_KEY: "gsk-...",
    },
  },
}
```

## Shell env import

`env.shellEnv` runs your login shell and imports only **missing** expected keys:

```
{
  env: {
    shellEnv: {
      enabled: true,
      timeoutMs: 15000,
    },
  },
}
```

Env var equivalents:

- `OPENCLAW_LOAD_SHELL_ENV=1`
- `OPENCLAW_SHELL_ENV_TIMEOUT_MS=15000`

## Runtime-injected env vars

OpenClaw also injects context markers into spawned child processes:

- `OPENCLAW_SHELL=exec`: set for commands run through the `exec` tool.
- `OPENCLAW_SHELL=acp`: set for ACP runtime backend process spawns (for example `acpx`).
- `OPENCLAW_SHELL=acp-client`: set for `openclaw acp client` when it spawns the ACP bridge process.
- `OPENCLAW_SHELL=tui-local`: set for local TUI `!` shell commands.

These are runtime markers (not required user config). They can be used in shell/profile logic
to apply context-specific rules.

## UI env vars

- `OPENCLAW_THEME=light`: force the light TUI palette when your terminal has a light background.
- `OPENCLAW_THEME=dark`: force the dark TUI palette.
- `COLORFGBG`: if your terminal exports it, OpenClaw uses the background color hint to auto-pick the TUI palette.

## Env var substitution in config

You can reference env vars directly in config string values using `${VAR_NAME}` syntax:

```
{
  models: {
    providers: {
      "vercel-gateway": {
        apiKey: "${VERCEL_GATEWAY_API_KEY}",
      },
    },
  },
}
```

See [Configuration: Env var substitution](https://docs.openclaw.ai/gateway/configuration-reference#env-var-substitution) for full details.

## Secret refs vs `${ENV}` strings

OpenClaw supports two env-driven patterns:

- `${VAR}` string substitution in config values.
- SecretRef objects (`{ source: "env", provider: "default", id: "VAR" }`) for fields that support secrets references.

Both resolve from process env at activation time. SecretRef details are documented in [Secrets Management](https://docs.openclaw.ai/gateway/secrets).

## Path-related env vars

| Variable | Purpose |
| --- | --- |
| `OPENCLAW_HOME` | Override the home directory used for all internal path resolution (`~/.openclaw/`, agent dirs, sessions, credentials). Useful when running OpenClaw as a dedicated service user. |
| `OPENCLAW_STATE_DIR` | Override the state directory (default `~/.openclaw`). |
| `OPENCLAW_CONFIG_PATH` | Override the config file path (default `~/.openclaw/openclaw.json`). |
| `OPENCLAW_INCLUDE_ROOTS` | Path-list of directories where `$include` directives may resolve files outside the config directory (default: none — `$include` is confined to the config dir). Tilde-expanded. |

## Logging

| Variable | Purpose |
| --- | --- |
| `OPENCLAW_LOG_LEVEL` | Override log level for both file and console (e.g. `debug`, `trace`). Take

_… [truncated; see https://docs.openclaw.ai/help/environment for full content]_


---

## FAQ - OpenClaw

_Source: <https://docs.openclaw.ai/help/faq>_

[OpenClaw home page](https://docs.openclaw.ai/)

FAQ

FAQ

Quick answers plus deeper troubleshooting for real-world setups (local dev, VPS, multi-agent, OAuth/API keys, model failover). For runtime diagnostics, see [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting). For the full config reference, see [Configuration](https://docs.openclaw.ai/gateway/configuration).

## First 60 seconds if something is broken

1. **Quick status (first check)**

```
openclaw status
```

Fast local summary: OS + update, gateway/service reachability, agents/sessions, provider config + runtime issues (when gateway is reachable).
2. **Pasteable report (safe to share)**

```
openclaw status --all
```

Read-only diagnosis with log tail (tokens redacted).
3. **Daemon + port state**

```
openclaw gateway status
```

Shows supervisor runtime vs RPC reachability, the probe target URL, and which config the service likely used.
4. **Deep probes**

```
openclaw status --deep
```

Runs a live gateway health probe, including channel probes when supported
(requires a reachable gateway). See [Health](https://docs.openclaw.ai/gateway/health).
5. **Tail the latest log**

```
openclaw logs --follow
```

If RPC is down, fall back to:

```
tail -f "$(ls -t /tmp/openclaw/openclaw-*.log | head -1)"
```

File logs are separate from service logs; see [Logging](https://docs.openclaw.ai/logging) and [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting).
6. **Run the doctor (repairs)**

```
openclaw doctor
```

Repairs/migrates config/state + runs health checks. See [Doctor](https://docs.openclaw.ai/gateway/doctor).
7. **Gateway snapshot**

```
openclaw health --json
openclaw health --verbose   # shows the target URL + config path on errors
```

Asks the running gateway for a full snapshot (WS-only). See [Health](https://docs.openclaw.ai/gateway/health).

## Quick start and first-run setup

First-run Q&A — install, onboard, auth routes, subscriptions, initial failures —
lives on the [First-run FAQ](https://docs.openclaw.ai/help/faq-first-run).

## What is OpenClaw?

What is OpenClaw, in one paragraph?

OpenClaw is a personal AI assistant you run on your own devices. It replies on the messaging surfaces you already use (WhatsApp, Telegram, Slack, Mattermost, Discord, Google Chat, Signal, iMessage, WebChat, and bundled channel plugins such as QQ Bot) and can also do voice + a live Canvas on supported platforms. The **Gateway** is the always-on control plane; the assistant is the product.

Value proposition

OpenClaw is not “just a Claude wrapper.” It’s a **local-first control plane** that lets you run a
capable assistant on **your own hardware**, reachable from the chat apps you already use, with
stateful sessions, memory, and tools - without handing control of your workflows to a hosted
SaaS.Highlights:

- **Your devices, your data:** run the Gateway wherever you want (Mac, Linux, VPS) and keep the
workspace + session history local.
- **Real channels, not a web sandbox:** WhatsApp/Telegram/Slack/Discord/Signal/iMessage/etc,
plus mobile voice and Canvas on supported platforms.
- **Model-agnostic:** use Anthropic, OpenAI, MiniMax, OpenRouter, etc., with per-agent routing
and failover.
- **Local-only option:** run local models so **all data can stay on your device** if you want.
- **Multi-agent routing:** separate agents per channel, account, or task, each with its own
workspace and defaults.
- **Open source and hackable:** inspect, extend, and self-host without vendor lock-in.

Docs: [Gateway](https://docs.openclaw.ai/gateway), [Channels](https://docs.openclaw.ai/channels), [Multi-agent](https://docs.openclaw.ai/concepts/multi-agent),
[Memory](https://docs.openclaw.ai/concepts/memory).

I just set it up - what should I do first?

Good first projects:

- Build a website (WordPress, Shopify, or a simple static site).
- Prototype a mobile app (outline, screens, API plan).
- Organize files and folders (cleanup, naming, tagging).
- Connect Gmail and au

_… [truncated; see https://docs.openclaw.ai/help/faq for full content]_


---

## FAQ: first-run setup - OpenClaw

_Source: <https://docs.openclaw.ai/help/faq-first-run>_

# install.ps1 has no dedicated -Verbose flag yet.
Set-PSDebug -Trace 1
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
Set-PSDebug -Trace 0
```

More options: [Installer flags](https://docs.openclaw.ai/install/installer).

Windows install says git not found or openclaw not recognized

Two common Windows issues:**1) npm error spawn git / git not found**

- Install **Git for Windows** and make sure `git` is on your PATH.
- Close and reopen PowerShell, then re-run the installer.

**2) openclaw is not recognized after install**

- Your npm global bin folder is not on PATH.
- Check the path:

```
npm config get prefix
```

- Add that directory to your user PATH (no `\bin` suffix needed on Windows; on most systems it is `%AppData%\npm`).
- Close and reopen PowerShell after updating PATH.

If you want the smoothest Windows setup, use **WSL2** instead of native Windows.
Docs: [Windows](https://docs.openclaw.ai/platforms/windows).

Windows exec output shows garbled Chinese text - what should I do?

This is usually a console code page mismatch on native Windows shells.Symptoms:

- `system.run`/`exec` output renders Chinese as mojibake
- The same command looks fine in another terminal profile

Quick workaround in PowerShell:

```
chcp 65001
[Console]::InputEncoding = [System.Text.UTF8Encoding]::new($false)
[Console]::OutputEncoding = [System.Text.UTF8Encoding]::new($false)
$OutputEncoding = [System.Text.UTF8Encoding]::new($false)
```

Then restart the Gateway and retry your command:

```
openclaw gateway restart
```

If you still reproduce this on latest OpenClaw, track/report it in:

- [Issue #30640](https://github.com/openclaw/openclaw/issues/30640)

The docs did not answer my question - how do I get a better answer?

Use the **hackable (git) install** so you have the full source and docs locally, then ask
your bot (or Claude/Codex) _from that folder_ so it can read the repo and answer precisely.

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git
```

More detail: [Install](https://docs.openclaw.ai/install) and [Installer flags](https://docs.openclaw.ai/install/installer).

How do I install OpenClaw on Linux?

Short answer: follow the Linux guide, then run onboarding.

- Linux quick path + service install: [Linux](https://docs.openclaw.ai/platforms/linux).
- Full walkthrough: [Getting Started](https://docs.openclaw.ai/start/getting-started).
- Installer + updates: [Install & updates](https://docs.openclaw.ai/install/updating).

How do I install OpenClaw on a VPS?

Any Linux VPS works. Install on the server, then use SSH/Tailscale to reach the Gateway.Guides: [exe.dev](https://docs.openclaw.ai/install/exe-dev), [Hetzner](https://docs.openclaw.ai/install/hetzner), [Fly.io](https://docs.openclaw.ai/install/fly).
Remote access: [Gateway remote](https://docs.openclaw.ai/gateway/remote).

Where are the cloud/VPS install guides?

We keep a **hosting hub** with the common providers. Pick one and follow the guide:

- [VPS hosting](https://docs.openclaw.ai/vps) (all providers in one place)
- [Fly.io](https://docs.openclaw.ai/install/fly)
- [Hetzner](https://docs.openclaw.ai/install/hetzner)
- [exe.dev](https://docs.openclaw.ai/install/exe-dev)

How it works in the cloud: the **Gateway runs on the server**, and you access it
from your laptop/phone via the Control UI (or Tailscale/SSH). Your state + workspace
live on the server, so treat the host as the source of truth and back it up.You can pair **nodes** (Mac/iOS/Android/headless) to that cloud Gateway to access
local screen/camera/canvas or run commands on your laptop while keeping the
Gateway in the cloud.Hub: [Platforms](https://docs.openclaw.ai/platforms). Remote access: [Gateway remote](https://docs.openclaw.ai/gateway/remote).
Nodes: [Nodes](https://docs.openclaw.ai/nodes), [Nodes CLI](https://docs.openclaw.ai/cli/nodes).

Can I ask OpenClaw to update itself?

Short answer: **possible, not recommended**. The update flow can re

_… [truncated; see https://docs.openclaw.ai/help/faq-first-run for full content]_


---

## Chat commands, aborting tasks, and "it will not stop"

_Source: <https://docs.openclaw.ai/help/faq.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# FAQ

Quick answers plus deeper troubleshooting for real-world setups (local dev, VPS, multi-agent, OAuth/API keys, model failover). For runtime diagnostics, see \[Troubleshooting\](/gateway/troubleshooting). For the full config reference, see \[Configuration\](/gateway/configuration).

\## First 60 seconds if something is broken

1\. \*\*Quick status (first check)\*\*

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw status
 \`\`\`

 Fast local summary: OS + update, gateway/service reachability, agents/sessions, provider config + runtime issues (when gateway is reachable).

2\. \*\*Pasteable report (safe to share)\*\*

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw status --all
 \`\`\`

 Read-only diagnosis with log tail (tokens redacted).

3\. \*\*Daemon + port state\*\*

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway status
 \`\`\`

 Shows supervisor runtime vs RPC reachability, the probe target URL, and which config the service likely used.

4\. \*\*Deep probes\*\*

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw status --deep
 \`\`\`

 Runs a live gateway health probe, including channel probes when supported
 (requires a reachable gateway). See \[Health\](/gateway/health).

5\. \*\*Tail the latest log\*\*

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw logs --follow
 \`\`\`

 If RPC is down, fall back to:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 tail -f "$(ls -t /tmp/openclaw/openclaw-\*.log \| head -1)"
 \`\`\`

 File logs are separate from service logs; see \[Logging\](/logging) and \[Troubleshooting\](/gateway/troubleshooting).

6\. \*\*Run the doctor (repairs)\*\*

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw doctor
 \`\`\`

 Repairs/migrates config/state + runs health checks. See \[Doctor\](/gateway/doctor).

7\. \*\*Gateway snapshot\*\*

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw health --json
 openclaw health --verbose # shows the target URL + config path on errors
 \`\`\`

 Asks the running gateway for a full snapshot (WS-only). See \[Health\](/gateway/health).

\## Quick start and first-run setup

First-run Q\\&A — install, onboard, auth routes, subscriptions, initial failures —
lives on the \[First-run FAQ\](/help/faq-first-run).

\## What is OpenClaw?

 OpenClaw is a personal AI assistant you run on your own devices. It replies on the messaging surfaces you already use (WhatsApp, Telegram, Slack, Mattermost, Discord, Google Chat, Signal, iMessage, WebChat, and bundled channel plugins such as QQ Bot) and can also do voice + a live Canvas on supported platforms. The \*\*Gateway\*\* is the always-on control plane; the assistant is the product.

 OpenClaw is not "just a Claude wrapper." It's a \*\*local-first control plane\*\* that lets you run a
 capable assistant on \*\*your own hardware\*\*, reachable from the chat apps you already use, with
 stateful sessions, memory, and tools - without handing control of your workflows to a hosted
 SaaS.

 Highlights:

 \\* \*\*Your devices, your data:\*\* run the Gateway wherever you want (Mac, Linux, VPS) and keep the
 workspace + session history local.
 \\* \*\*Real channels, not a web sandbox:\*\* WhatsApp/Telegram/Slack/Discord/Signal/iMessage/etc,
 plus mobile voice and Canvas on supported platforms.
 \\* \*\*Model-agnostic:\*\* use Anthropic, OpenAI, MiniMax, OpenRouter, etc., with per-agent routing
 and failover.
 \\* \*\*Local-only option:\*\* run local models so \*\*all data can stay on your device\*\* if you want.
 \\* \*\*Multi-agent routing:\*\* separate agents per channel, account, or task, each with its own
 workspace and defaults.
 \\* \*\*Open source

_… [truncated; see https://docs.openclaw.ai/help/faq.md for full content]_


---

## Testing - OpenClaw

_Source: <https://docs.openclaw.ai/help/testing>_

[OpenClaw home page](https://docs.openclaw.ai/)

Testing

Testing

OpenClaw has three Vitest suites (unit/integration, e2e, live) and a small set
of Docker runners. This doc is a “how we test” guide:

- What each suite covers (and what it deliberately does _not_ cover).
- Which commands to run for common workflows (local, pre-push, debugging).
- How live tests discover credentials and select models/providers.
- How to add regressions for real-world model/provider issues.

**QA stack (qa-lab, qa-channel, live transport lanes)** is documented separately:

- [QA overview](https://docs.openclaw.ai/concepts/qa-e2e-automation) — architecture, command surface, scenario authoring.
- [Matrix QA](https://docs.openclaw.ai/concepts/qa-matrix) — reference for `pnpm openclaw qa matrix`.
- [QA channel](https://docs.openclaw.ai/channels/qa-channel) — the synthetic transport plugin used by repo-backed scenarios.

This page covers running the regular test suites and Docker/Parallels runners. The QA-specific runners section below ( [QA-specific runners](https://docs.openclaw.ai/help/testing#qa-specific-runners)) lists the concrete `qa` invocations and points back at the references above.

## Quick start

Most days:

- Full gate (expected before push): `pnpm build && pnpm check && pnpm check:test-types && pnpm test`
- Faster local full-suite run on a roomy machine: `pnpm test:max`
- Direct Vitest watch loop: `pnpm test:watch`
- Direct file targeting now routes extension/channel paths too: `pnpm test extensions/discord/src/monitor/message-handler.preflight.test.ts`
- Prefer targeted runs first when you are iterating on a single failure.
- Docker-backed QA site: `pnpm qa:lab:up`
- Linux VM-backed QA lane: `pnpm openclaw qa suite --runner multipass --scenario channel-chat-baseline`

When you touch tests or want extra confidence:

- Coverage gate: `pnpm test:coverage`
- E2E suite: `pnpm test:e2e`

When debugging real providers/models (requires real creds):

- Live suite (models + gateway tool/image probes): `pnpm test:live`
- Target one live file quietly: `pnpm test:live -- src/agents/models.profiles.live.test.ts`
- Runtime performance reports: dispatch `OpenClaw Performance` with
`live_gpt54=true` for a real `openai/gpt-5.4` agent turn or
`deep_profile=true` for Kova CPU/heap/trace artifacts. Daily scheduled runs
publish mock-provider, deep-profile, and GPT 5.4 lane artifacts to
`openclaw/clawgrit-reports` when `CLAWGRIT_REPORTS_TOKEN` is configured. The
mock-provider report also includes source-level gateway boot, memory,
plugin-pressure, repeated fake-model hello-loop, and CLI startup numbers.
- Docker live model sweep: `pnpm test:docker:live-models`
  - Each selected model now runs a text turn plus a small file-read-style probe.
    Models whose metadata advertises `image` input also run a tiny image turn.
    Disable the extra probes with `OPENCLAW_LIVE_MODEL_FILE_PROBE=0` or
    `OPENCLAW_LIVE_MODEL_IMAGE_PROBE=0` when isolating provider failures.
  - CI coverage: daily `OpenClaw Scheduled Live And E2E Checks` and manual
    `OpenClaw Release Checks` both call the reusable live/E2E workflow with
    `include_live_suites: true`, which includes separate Docker live model
    matrix jobs sharded by provider.
  - For focused CI reruns, dispatch `OpenClaw Live And E2E Checks (Reusable)`
    with `include_live_suites: true` and `live_models_only: true`.
  - Add new high-signal provider secrets to `scripts/ci-hydrate-live-auth.sh`
    plus `.github/workflows/openclaw-live-and-e2e-checks-reusable.yml` and its
    scheduled/release callers.
- Native Codex bound-chat smoke: `pnpm test:docker:live-codex-bind`
  - Runs a Docker live lane against the Codex app-server path, binds a synthetic
    Slack DM with `/codex bind`, exercises `/codex fast` and
    `/codex permissions`, then verifies a plain reply and an image attachment
    route through the native plugin binding instead of ACP.
- Codex app-server harness smoke: `pnpm test:docker:live-codex-harness`

_… [truncated; see https://docs.openclaw.ai/help/testing for full content]_


---

## General troubleshooting - OpenClaw

_Source: <https://docs.openclaw.ai/help/troubleshooting>_

[OpenClaw home page](https://docs.openclaw.ai/)

Start here

General troubleshooting

If you only have 2 minutes, use this page as a triage front door.

## First 60 seconds

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

## Anthropic long context 429

If you see:
`HTTP 429: rate_limit_error: Extra usage is required for long context requests`,
go to [/gateway/troubleshooting#anthropic-429-extra-usage-required-for-long-context](https://docs.openclaw.ai/gateway/troubleshooting#anthropic-429-extra-usage-required-for-long-context).

## Local OpenAI-compatible backend works directly but fails in OpenClaw

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

## Plugin install fails with missing openclaw extensions

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

## Decision tree

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
- Your channe

_… [truncated; see https://docs.openclaw.ai/help/troubleshooting for full content]_
