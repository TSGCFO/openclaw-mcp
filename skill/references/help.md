# Help

_11 pages from docs.openclaw.ai — full content preserved._

## Contents

- [Preguntas frecuentes: modelos y autenticación - OpenClaw](#preguntas-frecuentes-modelos-y-autenticacin---openclaw)
- [Help - OpenClaw](#help---openclaw)
- [Debugging - OpenClaw](#debugging---openclaw)
- [Environment variables - OpenClaw](#environment-variables---openclaw)
- [FAQ - OpenClaw](#faq---openclaw)
- [FAQ: first-run setup - OpenClaw](#faq-first-run-setup---openclaw)
- [Chat commands, aborting tasks, and "it will not stop"](#chat-commands-aborting-tasks-and-it-will-not-stop)
- [Testing - OpenClaw](#testing---openclaw)
- [Testing: updates and plugins - OpenClaw](#testing-updates-and-plugins---openclaw)
- [General troubleshooting - OpenClaw](#general-troubleshooting---openclaw)
- [https://docs.openclaw.ai/sitemap.xml](#httpsdocsopenclawaisitemapxml)

---

## Preguntas frecuentes: modelos y autenticación - OpenClaw

_Source: <https://docs.openclaw.ai/es/help/faq-models>_

# Usa de forma predeterminada el agente predeterminado configurado (omite --agent)
openclaw models auth order get --provider anthropic

# Bloquea la rotación a un solo perfil (probar solo este)
openclaw models auth order set --provider anthropic anthropic:default

# O establece un orden explícito (respaldo dentro del proveedor)
openclaw models auth order set --provider anthropic anthropic:work anthropic:default

# Borra la anulación (volver a config auth.order / round-robin)
openclaw models auth order clear --provider anthropic
```

Para apuntar a un agente específico:

```
openclaw models auth order set --provider anthropic --agent main anthropic:default
```

Para verificar qué se intentará realmente, usa:

```
openclaw models status --probe
```

Si un perfil almacenado se omite del orden explícito, la sonda informa
`excluded_by_auth_order` para ese perfil en lugar de probarlo silenciosamente.

OAuth frente a clave de API: ¿cuál es la diferencia?

OpenClaw admite ambos:

- **OAuth** a menudo aprovecha el acceso por suscripción (cuando corresponde).
- Las **claves de API** usan facturación de pago por token.

El asistente admite explícitamente Anthropic Claude CLI, OpenAI Codex OAuth y claves de API.

## Relacionado

- [FAQ](https://docs.openclaw.ai/es/help/faq) — la FAQ principal
- [FAQ — inicio rápido y configuración de primera ejecución](https://docs.openclaw.ai/es/help/faq-first-run)
- [Selección de modelo](https://docs.openclaw.ai/es/concepts/model-providers)
- [Conmutación por error de modelo](https://docs.openclaw.ai/es/concepts/model-failover)

[First-run FAQ](https://docs.openclaw.ai/es/help/faq-first-run) [Pruebas](https://docs.openclaw.ai/es/help/testing)

Ctrl+I

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
(or as separate thinking blocks).Enable it via CLI:

```
pnpm gateway:watch --raw-stream
```

Optional path override:

```
pnpm gateway:watch --raw-stream --raw-stream-path ~/.openclaw/logs/raw-stream.jsonl
```

Equivalent env vars:

```
OPENCLAW_RAW_STREAM=1
OPENCLAW_RAW_STREAM_PATH=~/.openclaw/logs/raw-stream.jsonl
```

Default file:`~/.openclaw/logs/raw-stream.jsonl`

## Raw chunk logging (pi-mono)

To capture **raw OpenAI-compat chunks** before they are parsed into blocks,
pi-mono exposes a separate logger:

```
PI_RAW_STREAM=1
```

Optional path:

```
PI_RAW_STREAM_PATH=~/.pi-mono/logs/raw-openai-completions.jsonl
```

Default file:`~/.pi-mono/logs/raw-openai-completions.jsonl`

> Note: this is only emitted by processes using pi-mono’s
> `openai-completions` provider.

## Safety notes

- Raw stream logs can include full prompts, tool output, and user data.
- Keep logs local and delete them after debugging.
- If you share logs, scrub secrets and PII first.

## Related

- [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting)
- [FAQ](https://docs.openclaw.ai/help/faq)

[General troubleshooting](https://docs.openclaw.ai/help/troubleshooting) [FAQ](https://docs.openclaw.ai/help/faq)

Ctrl+I

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
| `OPENCLAW_LOG_LEVEL` | Override log level for both file and console (e.g. `debug`, `trace`). Takes precedence over `logging.level` and `logging.consoleLevel` in config. Invalid values are ignored with a warning. |

### `OPENCLAW_HOME`

When set, `OPENCLAW_HOME` replaces the system home directory (`$HOME` / `os.homedir()`) for all internal path resolution. This enables full filesystem isolation for headless service accounts.**Precedence:**`OPENCLAW_HOME` \> `$HOME` \> `USERPROFILE` \> `os.homedir()`**Example** (macOS LaunchDaemon):

```
<key>EnvironmentVariables</key>
<dict>
  <key>OPENCLAW_HOME</key>
  <string>/Users/user</string>
</dict>
```

`OPENCLAW_HOME` can also be set to a tilde path (e.g. `~/svc`), which gets expanded using `$HOME` before use.

## nvm users: web\_fetch TLS failures

If Node.js was installed via **nvm** (not the system package manager), the built-in `fetch()` uses
nvm’s bundled CA store, which may be missing modern root CAs (ISRG Root X1/X2 for Let’s Encrypt,
DigiCert Global Root G2, etc.). This causes `web_fetch` to fail with `"fetch failed"` on most HTTPS sites.On Linux, OpenClaw automatically detects nvm and applies the fix in the actual startup environment:

- `openclaw gateway install` writes `NODE_EXTRA_CA_CERTS` into the systemd service environment
- the `openclaw` CLI entrypoint re-execs itself with `NODE_EXTRA_CA_CERTS` set before Node startup

**Manual fix (for older versions or direct `node ...` launches):**Export the variable before starting OpenClaw:

```
export NODE_EXTRA_CA_CERTS=/etc/ssl/certs/ca-certificates.crt
openclaw gateway run
```

Do not rely on writing only to `~/.openclaw/.env` for this variable; Node reads
`NODE_EXTRA_CA_CERTS` at process startup.

## Legacy environment variables

OpenClaw only reads `OPENCLAW_*` environment variables. The legacy
`CLAWDBOT_*` and `MOLTBOT_*` prefixes from earlier releases are silently
ignored.If any are still set on the Gateway process at startup, OpenClaw emits a
single Node deprecation warning (`OPENCLAW_LEGACY_ENV_VARS`) listing the
detected prefixes and the total count. Rename each value by replacing the
legacy prefix with `OPENCLAW_` (for example `CLAWDBOT_GATEWAY_TOKEN` →
`OPENCLAW_GATEWAY_TOKEN`); the old names take no effect.

## Related

- [Gateway configuration](https://docs.openclaw.ai/gateway/configuration)
- [FAQ: env vars and .env loading](https://docs.openclaw.ai/help/faq#env-vars-and-env-loading)
- [Models overview](https://docs.openclaw.ai/concepts/models)

[Live tests](https://docs.openclaw.ai/help/testing-live) [Diagnostics flags](https://docs.openclaw.ai/diagnostics/flags)

Ctrl+I

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
- Connect Gmail and automate summaries or follow ups.

It can handle large tasks, but it works best when you split them into phases and
use sub agents for parallel work.

What are the top five everyday use cases for OpenClaw?

Everyday wins usually look like:

- **Personal briefings:** summaries of inbox, calendar, and news you care about.
- **Research and drafting:** quick research, summaries, and first drafts for emails or docs.
- **Reminders and follow ups:** cron or heartbeat driven nudges and checklists.
- **Browser automation:** filling forms, collecting data, and repeating web tasks.
- **Cross device coordination:** send a task from your phone, let the Gateway run it on a server, and get the result back in chat.

Can OpenClaw help with lead gen, outreach, ads, and blogs for a SaaS?

Yes for **research, qualification, and drafting**. It can scan sites, build shortlists,
summarize prospects, and write outreach or ad copy drafts.For **outreach or ad runs**, keep a human in the loop. Avoid spam, follow local laws and
platform policies, and review anything before it is sent. The safest pattern is to let
OpenClaw draft and you approve.Docs: [Security](https://docs.openclaw.ai/gateway/security).

What are the advantages vs Claude Code for web development?

OpenClaw is a **personal assistant** and coordination layer, not an IDE replacement. Use
Claude Code or Codex for the fastest direct coding loop inside a repo. Use OpenClaw when you
want durable memory, cross-device access, and tool orchestration.Advantages:

- **Persistent memory + workspace** across sessions
- **Multi-platform access** (WhatsApp, Telegram, TUI, WebChat)
- **Tool orchestration** (browser, files, scheduling, hooks)
- **Always-on Gateway** (run on a VPS, interact from anywhere)
- **Nodes** for local browser/screen/camera/exec

Showcase: [https://openclaw.ai/showcase](https://openclaw.ai/showcase)

## Skills and automation

How do I customize skills without keeping the repo dirty?

Use managed overrides instead of editing the repo copy. Put your changes in `~/.openclaw/skills/<name>/SKILL.md` (or add a folder via `skills.load.extraDirs` in `~/.openclaw/openclaw.json`). Precedence is `<workspace>/skills` → `<workspace>/.agents/skills` → `~/.agents/skills` → `~/.openclaw/skills` → bundled → `skills.load.extraDirs`, so managed overrides still win over bundled skills without touching git. If you need the skill installed globally but only visible to some agents, keep the shared copy in `~/.openclaw/skills` and control visibility with `agents.defaults.skills` and `agents.list[].skills`. Only upstream-worthy edits should live in the repo and go out as PRs.

Can I load skills from a custom folder?

Yes. Add extra directories via `skills.load.extraDirs` in `~/.openclaw/openclaw.json` (lowest precedence). Default precedence is `<workspace>/skills` → `<workspace>/.agents/skills` → `~/.agents/skills` → `~/.openclaw/skills` → bundled → `skills.load.extraDirs`. `clawhub` installs into `./skills` by default, which OpenClaw treats as `<workspace>/skills` on the next session. If the skill should only be visible to certain agents, pair that with `agents.defaults.skills` or `agents.list[].skills`.

How can I use different models for different tasks?

Today the supported patterns are:

- **Cron jobs**: isolated jobs can set a `model` override per job.
- **Sub-agents**: route tasks to separate agents with different default models.
- **On-demand switch**: use `/model` to switch the current session model at any time.

See [Cron jobs](https://docs.openclaw.ai/automation/cron-jobs), [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent), and [Slash commands](https://docs.openclaw.ai/tools/slash-commands).

The bot freezes while doing heavy work. How do I offload that?

Use **sub-agents** for long or parallel tasks. Sub-agents run in their own session,
return a summary, and keep your main chat responsive.Ask your bot to “spawn a sub-agent for this task” or use `/subagents`.
Use `/status` in chat to see what the Gateway is doing right now (and whether it is busy).Token tip: long tasks and sub-agents both consume tokens. If cost is a concern, set a
cheaper model for sub-agents via `agents.defaults.subagents.model`.Docs: [Sub-agents](https://docs.openclaw.ai/tools/subagents), [Background Tasks](https://docs.openclaw.ai/automation/tasks).

How do thread-bound subagent sessions work on Discord?

Use thread bindings. You can bind a Discord thread to a subagent or session target so follow-up messages in that thread stay on that bound session.Basic flow:

- Spawn with `sessions_spawn` using `thread: true` (and optionally `mode: "session"` for persistent follow-up).
- Or manually bind with `/focus <target>`.
- Use `/agents` to inspect binding state.
- Use `/session idle <duration|off>` and `/session max-age <duration|off>` to control auto-unfocus.
- Use `/unfocus` to detach the thread.

Required config:

- Global defaults: `session.threadBindings.enabled`, `session.threadBindings.idleHours`, `session.threadBindings.maxAgeHours`.
- Discord overrides: `channels.discord.threadBindings.enabled`, `channels.discord.threadBindings.idleHours`, `channels.discord.threadBindings.maxAgeHours`.
- Auto-bind on spawn: `channels.discord.threadBindings.spawnSessions` defaults to `true`; set it to `false` to disable thread-bound session spawns.

Docs: [Sub-agents](https://docs.openclaw.ai/tools/subagents), [Discord](https://docs.openclaw.ai/channels/discord), [Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference), [Slash commands](https://docs.openclaw.ai/tools/slash-commands).

A subagent finished, but the completion update went to the wrong place or never posted. What should I check?

Check the resolved requester route first:

- Completion-mode subagent delivery prefers any bound thread or conversation route when one exists.
- If the completion origin only carries a channel, OpenClaw falls back to the requester session’s stored route (`lastChannel` / `lastTo` / `lastAccountId`) so direct delivery can still succeed.
- If neither a bound route nor a usable stored route exists, direct delivery can fail and the result falls back to queued session delivery instead of posting immediately to chat.
- Invalid or stale targets can still force queue fallback or final delivery failure.
- If the child’s last visible assistant reply is the exact silent token `NO_REPLY` / `no_reply`, or exactly `ANNOUNCE_SKIP`, OpenClaw intentionally suppresses the announce instead of posting stale earlier progress.
- If the child timed out after only tool calls, the announce can collapse that into a short partial-progress summary instead of replaying raw tool output.

Debug:

```
openclaw tasks show <runId-or-sessionKey>
```

Docs: [Sub-agents](https://docs.openclaw.ai/tools/subagents), [Background Tasks](https://docs.openclaw.ai/automation/tasks), [Session Tools](https://docs.openclaw.ai/concepts/session-tool).

Cron or reminders do not fire. What should I check?

Cron runs inside the Gateway process. If the Gateway is not running continuously,
scheduled jobs will not run.Checklist:

- Confirm cron is enabled (`cron.enabled`) and `OPENCLAW_SKIP_CRON` is not set.
- Check the Gateway is running 24/7 (no sleep/restarts).
- Verify timezone settings for the job (`--tz` vs host timezone).

Debug:

```
openclaw cron run <jobId>
openclaw cron runs --id <jobId> --limit 50
```

Docs: [Cron jobs](https://docs.openclaw.ai/automation/cron-jobs), [Automation & Tasks](https://docs.openclaw.ai/automation).

Cron fired, but nothing was sent to the channel. Why?

Check the delivery mode first:

- `--no-deliver` / `delivery.mode: "none"` means no runner fallback send is expected.
- Missing or invalid announce target (`channel` / `to`) means the runner skipped outbound delivery.
- Channel auth failures (`unauthorized`, `Forbidden`) mean the runner tried to deliver but credentials blocked it.
- A silent isolated result (`NO_REPLY` / `no_reply` only) is treated as intentionally non-deliverable, so the runner also suppresses queued fallback delivery.

For isolated cron jobs, the agent can still send directly with the `message`
tool when a chat route is available. `--announce` only controls the runner
fallback path for final text that the agent did not already send.Debug:

```
openclaw cron runs --id <jobId> --limit 50
openclaw tasks show <runId-or-sessionKey>
```

Docs: [Cron jobs](https://docs.openclaw.ai/automation/cron-jobs), [Background Tasks](https://docs.openclaw.ai/automation/tasks).

Why did an isolated cron run switch models or retry once?

That is usually the live model-switch path, not duplicate scheduling.Isolated cron can persist a runtime model handoff and retry when the active
run throws `LiveSessionModelSwitchError`. The retry keeps the switched
provider/model, and if the switch carried a new auth profile override, cron
persists that too before retrying.Related selection rules:

- Gmail hook model override wins first when applicable.
- Then per-job `model`.
- Then any stored cron-session model override.
- Then the normal agent/default model selection.

The retry loop is bounded. After the initial attempt plus 2 switch retries,
cron aborts instead of looping forever.Debug:

```
openclaw cron runs --id <jobId> --limit 50
openclaw tasks show <runId-or-sessionKey>
```

Docs: [Cron jobs](https://docs.openclaw.ai/automation/cron-jobs), [cron CLI](https://docs.openclaw.ai/cli/cron).

How do I install skills on Linux?

Use native `openclaw skills` commands or drop skills into your workspace. The macOS Skills UI isn’t available on Linux.
Browse skills at [https://clawhub.ai](https://clawhub.ai/).

```
openclaw skills search "calendar"
openclaw skills search --limit 20
openclaw skills install <skill-slug>
openclaw skills install <skill-slug> --version <version>
openclaw skills install <skill-slug> --force
openclaw skills update --all
openclaw skills list --eligible
openclaw skills check
```

Native `openclaw skills install` writes into the active workspace `skills/`
directory. Install the separate `clawhub` CLI only if you want to publish or
sync your own skills. For shared installs across agents, put the skill under
`~/.openclaw/skills` and use `agents.defaults.skills` or
`agents.list[].skills` if you want to narrow which agents can see it.

Can OpenClaw run tasks on a schedule or continuously in the background?

Yes. Use the Gateway scheduler:

- **Cron jobs** for scheduled or recurring tasks (persist across restarts).
- **Heartbeat** for “main session” periodic checks.
- **Isolated jobs** for autonomous agents that post summaries or deliver to chats.

Docs: [Cron jobs](https://docs.openclaw.ai/automation/cron-jobs), [Automation & Tasks](https://docs.openclaw.ai/automation),
[Heartbeat](https://docs.openclaw.ai/gateway/heartbeat).

Can I run Apple macOS-only skills from Linux?

Not directly. macOS skills are gated by `metadata.openclaw.os` plus required binaries, and skills only appear in the system prompt when they are eligible on the **Gateway host**. On Linux, `darwin`-only skills (like `apple-notes`, `apple-reminders`, `things-mac`) will not load unless you override the gating.You have three supported patterns:**Option A - run the Gateway on a Mac (simplest).**
Run the Gateway where the macOS binaries exist, then connect from Linux in [remote mode](https://docs.openclaw.ai/help/faq#gateway-ports-already-running-and-remote-mode) or over Tailscale. The skills load normally because the Gateway host is macOS.**Option B - use a macOS node (no SSH).**
Run the Gateway on Linux, pair a macOS node (menubar app), and set **Node Run Commands** to “Always Ask” or “Always Allow” on the Mac. OpenClaw can treat macOS-only skills as eligible when the required binaries exist on the node. The agent runs those skills via the `nodes` tool. If you choose “Always Ask”, approving “Always Allow” in the prompt adds that command to the allowlist.**Option C - proxy macOS binaries over SSH (advanced).**
Keep the Gateway on Linux, but make the required CLI binaries resolve to SSH wrappers that run on a Mac. Then override the skill to allow Linux so it stays eligible.

1. Create an SSH wrapper for the binary (example: `memo` for Apple Notes):

```
#!/usr/bin/env bash
set -euo pipefail
exec ssh -T user@mac-host /opt/homebrew/bin/memo "$@"
```

2. Put the wrapper on `PATH` on the Linux host (for example `~/bin/memo`).
3. Override the skill metadata (workspace or `~/.openclaw/skills`) to allow Linux:

```
   ---
name: apple-notes
description: Manage Apple Notes via the memo CLI on macOS.
metadata: { "openclaw": { "os": ["darwin", "linux"], "requires": { "bins": ["memo"] } } }
   ---
```

4. Start a new session so the skills snapshot refreshes.

Do you have a Notion or HeyGen integration?

Not built-in today.Options:

- **Custom skill / plugin:** best for reliable API access (Notion/HeyGen both have APIs).
- **Browser automation:** works without code but is slower and more fragile.

If you want to keep context per client (agency workflows), a simple pattern is:

- One Notion page per client (context + preferences + active work).
- Ask the agent to fetch that page at the start of a session.

If you want a native integration, open a feature request or build a skill
targeting those APIs.Install skills:

```
openclaw skills install <skill-slug>
openclaw skills update --all
```

Native installs land in the active workspace `skills/` directory. For shared skills across agents, place them in `~/.openclaw/skills/<name>/SKILL.md`. If only some agents should see a shared install, configure `agents.defaults.skills` or `agents.list[].skills`. Some skills expect binaries installed via Homebrew; on Linux that means Linuxbrew (see the Homebrew Linux FAQ entry above). See [Skills](https://docs.openclaw.ai/tools/skills), [Skills config](https://docs.openclaw.ai/tools/skills-config), and [ClawHub](https://docs.openclaw.ai/tools/clawhub).

How do I use my existing signed-in Chrome with OpenClaw?

Use the built-in `user` browser profile, which attaches through Chrome DevTools MCP:

```
openclaw browser --browser-profile user tabs
openclaw browser --browser-profile user snapshot
```

If you want a custom name, create an explicit MCP profile:

```
openclaw browser create-profile --name chrome-live --driver existing-session
openclaw browser --browser-profile chrome-live tabs
```

This path can use the local host browser or a connected browser node. If the Gateway runs elsewhere, either run a node host on the browser machine or use remote CDP instead.Current limits on `existing-session` / `user`:

- actions are ref-driven, not CSS-selector driven
- uploads require `ref` / `inputRef` and currently support one file at a time
- `responsebody`, PDF export, download interception, and batch actions still need a managed browser or raw CDP profile

## Sandboxing and memory

Is there a dedicated sandboxing doc?

Yes. See [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing). For Docker-specific setup (full gateway in Docker or sandbox images), see [Docker](https://docs.openclaw.ai/install/docker).

Docker feels limited - how do I enable full features?

The default image is security-first and runs as the `node` user, so it does not
include system packages, Homebrew, or bundled browsers. For a fuller setup:

- Persist `/home/node` with `OPENCLAW_HOME_VOLUME` so caches survive.
- Bake system deps into the image with `OPENCLAW_DOCKER_APT_PACKAGES`.
- Install Playwright browsers via the bundled CLI:
`node /app/node_modules/playwright-core/cli.js install chromium`
- Set `PLAYWRIGHT_BROWSERS_PATH` and ensure the path is persisted.

Docs: [Docker](https://docs.openclaw.ai/install/docker), [Browser](https://docs.openclaw.ai/tools/browser).

Can I keep DMs personal but make groups public/sandboxed with one agent?

Yes - if your private traffic is **DMs** and your public traffic is **groups**.Use `agents.defaults.sandbox.mode: "non-main"` so group/channel sessions (non-main keys) run in the configured sandbox backend, while the main DM session stays on-host. Docker is the default backend if you do not choose one. Then restrict what tools are available in sandboxed sessions via `tools.sandbox.tools`.Setup walkthrough + example config: [Groups: personal DMs + public groups](https://docs.openclaw.ai/channels/groups#pattern-personal-dms-public-groups-single-agent)Key config reference: [Gateway configuration](https://docs.openclaw.ai/gateway/config-agents#agentsdefaultssandbox)

How do I bind a host folder into the sandbox?

Set `agents.defaults.sandbox.docker.binds` to `["host:path:mode"]` (e.g., `"/home/user/src:/src:ro"`). Global + per-agent binds merge; per-agent binds are ignored when `scope: "shared"`. Use `:ro` for anything sensitive and remember binds bypass the sandbox filesystem walls.OpenClaw validates bind sources against both the normalized path and the canonical path resolved through the deepest existing ancestor. That means symlink-parent escapes still fail closed even when the last path segment does not exist yet, and allowed-root checks still apply after symlink resolution.See [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing#custom-bind-mounts) and [Sandbox vs Tool Policy vs Elevated](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated#bind-mounts-security-quick-check) for examples and safety notes.

How does memory work?

OpenClaw memory is just Markdown files in the agent workspace:

- Daily notes in `memory/YYYY-MM-DD.md`
- Curated long-term notes in `MEMORY.md` (main/private sessions only)

OpenClaw also runs a **silent pre-compaction memory flush** to remind the model
to write durable notes before auto-compaction. This only runs when the workspace
is writable (read-only sandboxes skip it). See [Memory](https://docs.openclaw.ai/concepts/memory).

Memory keeps forgetting things. How do I make it stick?

Ask the bot to **write the fact to memory**. Long-term notes belong in `MEMORY.md`,
short-term context goes into `memory/YYYY-MM-DD.md`.This is still an area we are improving. It helps to remind the model to store memories;
it will know what to do. If it keeps forgetting, verify the Gateway is using the same
workspace on every run.Docs: [Memory](https://docs.openclaw.ai/concepts/memory), [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace).

Does memory persist forever? What are the limits?

Memory files live on disk and persist until you delete them. The limit is your
storage, not the model. The **session context** is still limited by the model
context window, so long conversations can compact or truncate. That is why
memory search exists - it pulls only the relevant parts back into context.Docs: [Memory](https://docs.openclaw.ai/concepts/memory), [Context](https://docs.openclaw.ai/concepts/context).

Does semantic memory search require an OpenAI API key?

Only if you use **OpenAI embeddings**. Codex OAuth covers chat/completions and
does **not** grant embeddings access, so **signing in with Codex (OAuth or the**
**Codex CLI login)** does not help for semantic memory search. OpenAI embeddings
still need a real API key (`OPENAI_API_KEY` or `models.providers.openai.apiKey`).If you don’t set a provider explicitly, OpenClaw auto-selects a provider when it
can resolve an API key (auth profiles, `models.providers.*.apiKey`, or env vars).
It prefers OpenAI if an OpenAI key resolves, otherwise Gemini if a Gemini key
resolves, then Voyage, then Mistral. If no remote key is available, memory
search stays disabled until you configure it. If you have a local model path
configured and present, OpenClaw
prefers `local`. Ollama is supported when you explicitly set
`memorySearch.provider = "ollama"`.If you’d rather stay local, set `memorySearch.provider = "local"` (and optionally
`memorySearch.fallback = "none"`). If you want Gemini embeddings, set
`memorySearch.provider = "gemini"` and provide `GEMINI_API_KEY` (or
`memorySearch.remote.apiKey`). We support **OpenAI, Gemini, Voyage, Mistral, Ollama, or local** embedding
models - see [Memory](https://docs.openclaw.ai/concepts/memory) for the setup details.

## Where things live on disk

Is all data used with OpenClaw saved locally?

No - **OpenClaw’s state is local**, but **external services still see what you send them**.

- **Local by default:** sessions, memory files, config, and workspace live on the Gateway host
(`~/.openclaw` \+ your workspace directory).
- **Remote by necessity:** messages you send to model providers (Anthropic/OpenAI/etc.) go to
their APIs, and chat platforms (WhatsApp/Telegram/Slack/etc.) store message data on their
servers.
- **You control the footprint:** using local models keeps prompts on your machine, but channel
traffic still goes through the channel’s servers.

Related: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace), [Memory](https://docs.openclaw.ai/concepts/memory).

Where does OpenClaw store its data?

Everything lives under `$OPENCLAW_STATE_DIR` (default: `~/.openclaw`):

| Path | Purpose |
| --- | --- |
| `$OPENCLAW_STATE_DIR/openclaw.json` | Main config (JSON5) |
| `$OPENCLAW_STATE_DIR/credentials/oauth.json` | Legacy OAuth import (copied into auth profiles on first use) |
| `$OPENCLAW_STATE_DIR/agents/<agentId>/agent/auth-profiles.json` | Auth profiles (OAuth, API keys, and optional `keyRef`/`tokenRef`) |
| `$OPENCLAW_STATE_DIR/secrets.json` | Optional file-backed secret payload for `file` SecretRef providers |
| `$OPENCLAW_STATE_DIR/agents/<agentId>/agent/auth.json` | Legacy compatibility file (static `api_key` entries scrubbed) |
| `$OPENCLAW_STATE_DIR/credentials/` | Provider state (e.g. `whatsapp/<accountId>/creds.json`) |
| `$OPENCLAW_STATE_DIR/agents/` | Per-agent state (agentDir + sessions) |
| `$OPENCLAW_STATE_DIR/agents/<agentId>/sessions/` | Conversation history & state (per agent) |
| `$OPENCLAW_STATE_DIR/agents/<agentId>/sessions/sessions.json` | Session metadata (per agent) |

Legacy single-agent path: `~/.openclaw/agent/*` (migrated by `openclaw doctor`).Your **workspace** (AGENTS.md, memory files, skills, etc.) is separate and configured via `agents.defaults.workspace` (default: `~/.openclaw/workspace`).

Where should AGENTS.md / SOUL.md / USER.md / MEMORY.md live?

These files live in the **agent workspace**, not `~/.openclaw`.

- **Workspace (per agent)**: `AGENTS.md`, `SOUL.md`, `IDENTITY.md`, `USER.md`,
`MEMORY.md`, `memory/YYYY-MM-DD.md`, optional `HEARTBEAT.md`.
Lowercase root `memory.md` is legacy repair input only; `openclaw doctor --fix`
can merge it into `MEMORY.md` when both files exist.
- **State dir (`~/.openclaw`)**: config, channel/provider state, auth profiles, sessions, logs,
and shared skills (`~/.openclaw/skills`).

Default workspace is `~/.openclaw/workspace`, configurable via:

```
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
}
```

If the bot “forgets” after a restart, confirm the Gateway is using the same
workspace on every launch (and remember: remote mode uses the **gateway host’s**
workspace, not your local laptop).Tip: if you want a durable behavior or preference, ask the bot to **write it into**
**AGENTS.md or MEMORY.md** rather than relying on chat history.See [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace) and [Memory](https://docs.openclaw.ai/concepts/memory).

Recommended backup strategy

Put your **agent workspace** in a **private** git repo and back it up somewhere
private (for example GitHub private). This captures memory + AGENTS/SOUL/USER
files, and lets you restore the assistant’s “mind” later.Do **not** commit anything under `~/.openclaw` (credentials, sessions, tokens, or encrypted secrets payloads).
If you need a full restore, back up both the workspace and the state directory
separately (see the migration question above).Docs: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace).

How do I completely uninstall OpenClaw?

See the dedicated guide: [Uninstall](https://docs.openclaw.ai/install/uninstall).

Can agents work outside the workspace?

Yes. The workspace is the **default cwd** and memory anchor, not a hard sandbox.
Relative paths resolve inside the workspace, but absolute paths can access other
host locations unless sandboxing is enabled. If you need isolation, use
[`agents.defaults.sandbox`](https://docs.openclaw.ai/gateway/sandboxing) or per-agent sandbox settings. If you
want a repo to be the default working directory, point that agent’s
`workspace` to the repo root. The OpenClaw repo is just source code; keep the
workspace separate unless you intentionally want the agent to work inside it.Example (repo as default cwd):

```
{
  agents: {
    defaults: {
      workspace: "~/Projects/my-repo",
    },
  },
}
```

Remote mode: where is the session store?

Session state is owned by the **gateway host**. If you’re in remote mode, the session store you care about is on the remote machine, not your local laptop. See [Session management](https://docs.openclaw.ai/concepts/session).

## Config basics

What format is the config? Where is it?

OpenClaw reads an optional **JSON5** config from `$OPENCLAW_CONFIG_PATH` (default: `~/.openclaw/openclaw.json`):

```
$OPENCLAW_CONFIG_PATH
```

If the file is missing, it uses safe-ish defaults (including a default workspace of `~/.openclaw/workspace`).

I set gateway.bind: "lan" (or "tailnet") and now nothing listens / the UI says unauthorized

Non-loopback binds **require a valid gateway auth path**. In practice that means:

- shared-secret auth: token or password
- `gateway.auth.mode: "trusted-proxy"` behind a correctly configured identity-aware reverse proxy

```
{
  gateway: {
    bind: "lan",
    auth: {
      mode: "token",
      token: "replace-me",
    },
  },
}
```

Notes:

- `gateway.remote.token` / `.password` do **not** enable local gateway auth by themselves.
- Local call paths can use `gateway.remote.*` as fallback only when `gateway.auth.*` is unset.
- For password auth, set `gateway.auth.mode: "password"` plus `gateway.auth.password` (or `OPENCLAW_GATEWAY_PASSWORD`) instead.
- If `gateway.auth.token` / `gateway.auth.password` is explicitly configured via SecretRef and unresolved, resolution fails closed (no remote fallback masking).
- Shared-secret Control UI setups authenticate via `connect.params.auth.token` or `connect.params.auth.password` (stored in app/UI settings). Identity-bearing modes such as Tailscale Serve or `trusted-proxy` use request headers instead. Avoid putting shared secrets in URLs.
- With `gateway.auth.mode: "trusted-proxy"`, same-host loopback reverse proxies require explicit `gateway.auth.trustedProxy.allowLoopback = true` and a loopback entry in `gateway.trustedProxies`.

Why do I need a token on localhost now?

OpenClaw enforces gateway auth by default, including loopback. In the normal default path that means token auth: if no explicit auth path is configured, gateway startup resolves to token mode and auto-generates one, saving it to `gateway.auth.token`, so **local WS clients must authenticate**. This blocks other local processes from calling the Gateway.If you prefer a different auth path, you can explicitly choose password mode (or, for identity-aware reverse proxies, `trusted-proxy`). If you **really** want open loopback, set `gateway.auth.mode: "none"` explicitly in your config. Doctor can generate a token for you any time: `openclaw doctor --generate-gateway-token`.

Do I have to restart after changing config?

The Gateway watches the config and supports hot-reload:

- `gateway.reload.mode: "hybrid"` (default): hot-apply safe changes, restart for critical ones
- `hot`, `restart`, `off` are also supported

How do I disable funny CLI taglines?

Set `cli.banner.taglineMode` in config:

```
{
  cli: {
    banner: {
      taglineMode: "off", // random | default | off
    },
  },
}
```

- `off`: hides tagline text but keeps the banner title/version line.
- `default`: uses `All your chats, one OpenClaw.` every time.
- `random`: rotating funny/seasonal taglines (default behavior).
- If you want no banner at all, set env `OPENCLAW_HIDE_BANNER=1`.

How do I enable web search (and web fetch)?

`web_fetch` works without an API key. `web_search` depends on your selected
provider:

- API-backed providers such as Brave, Exa, Firecrawl, Gemini, Grok, Kimi, MiniMax Search, Perplexity, and Tavily require their normal API key setup.
- Ollama Web Search is key-free, but it uses your configured Ollama host and requires `ollama signin`.
- DuckDuckGo is key-free, but it is an unofficial HTML-based integration.
- SearXNG is key-free/self-hosted; configure `SEARXNG_BASE_URL` or `plugins.entries.searxng.config.webSearch.baseUrl`.

**Recommended:** run `openclaw configure --section web` and choose a provider.
Environment alternatives:

- Brave: `BRAVE_API_KEY`
- Exa: `EXA_API_KEY`
- Firecrawl: `FIRECRAWL_API_KEY`
- Gemini: `GEMINI_API_KEY`
- Grok: `XAI_API_KEY`
- Kimi: `KIMI_API_KEY` or `MOONSHOT_API_KEY`
- MiniMax Search: `MINIMAX_CODE_PLAN_KEY`, `MINIMAX_CODING_API_KEY`, or `MINIMAX_API_KEY`
- Perplexity: `PERPLEXITY_API_KEY` or `OPENROUTER_API_KEY`
- SearXNG: `SEARXNG_BASE_URL`
- Tavily: `TAVILY_API_KEY`

```
{
  plugins: {
    entries: {
      brave: {
        config: {
          webSearch: {
            apiKey: "BRAVE_API_KEY_HERE",
          },
        },
      },
    },
    },
    tools: {
      web: {
        search: {
          enabled: true,
          provider: "brave",
          maxResults: 5,
        },
        fetch: {
          enabled: true,
          provider: "firecrawl", // optional; omit for auto-detect
        },
      },
    },
}
```

Provider-specific web-search config now lives under `plugins.entries.<plugin>.config.webSearch.*`.
Legacy `tools.web.search.*` provider paths still load temporarily for compatibility, but they should not be used for new configs.
Firecrawl web-fetch fallback config lives under `plugins.entries.firecrawl.config.webFetch.*`.Notes:

- If you use allowlists, add `web_search`/`web_fetch`/`x_search` or `group:web`.
- `web_fetch` is enabled by default (unless explicitly disabled).
- If `tools.web.fetch.provider` is omitted, OpenClaw auto-detects the first ready fetch fallback provider from available credentials. Today the bundled provider is Firecrawl.
- Daemons read env vars from `~/.openclaw/.env` (or the service environment).

Docs: [Web tools](https://docs.openclaw.ai/tools/web).

config.apply wiped my config. How do I recover and avoid this?

`config.apply` replaces the **entire config**. If you send a partial object, everything
else is removed.Current OpenClaw protects many accidental clobbers:

- OpenClaw-owned config writes validate the full post-change config before writing.
- Invalid or destructive OpenClaw-owned writes are rejected and saved as `openclaw.json.rejected.*`.
- If a direct edit breaks startup or hot reload, the Gateway restores the last-known-good config and saves the rejected file as `openclaw.json.clobbered.*`.
- The main agent receives a boot warning after recovery so it does not blindly write the bad config again.

Recover:

- Check `openclaw logs --follow` for `Config auto-restored from last-known-good`, `Config write rejected:`, or `config reload restored last-known-good config`.
- Inspect the newest `openclaw.json.clobbered.*` or `openclaw.json.rejected.*` beside the active config.
- Keep the active restored config if it works, then copy only the intended keys back with `openclaw config set` or `config.patch`.
- Run `openclaw config validate` and `openclaw doctor`.
- If you have no last-known-good or rejected payload, restore from backup, or re-run `openclaw doctor` and reconfigure channels/models.
- If this was unexpected, file a bug and include your last known config or any backup.
- A local coding agent can often reconstruct a working config from logs or history.

Avoid it:

- Use `openclaw config set` for small changes.
- Use `openclaw configure` for interactive edits.
- Use `config.schema.lookup` first when you are not sure about an exact path or field shape; it returns a shallow schema node plus immediate child summaries for drill-down.
- Use `config.patch` for partial RPC edits; keep `config.apply` for full-config replacement only.
- If you are using the owner-only `gateway` tool from an agent run, it will still reject writes to `tools.exec.ask` / `tools.exec.security` (including legacy `tools.bash.*` aliases that normalize to the same protected exec paths).

Docs: [Config](https://docs.openclaw.ai/cli/config), [Configure](https://docs.openclaw.ai/cli/configure), [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting#gateway-restored-last-known-good-config), [Doctor](https://docs.openclaw.ai/gateway/doctor).

How do I run a central Gateway with specialized workers across devices?

The common pattern is **one Gateway** (e.g. Raspberry Pi) plus **nodes** and **agents**:

- **Gateway (central):** owns channels (Signal/WhatsApp), routing, and sessions.
- **Nodes (devices):** Macs/iOS/Android connect as peripherals and expose local tools (`system.run`, `canvas`, `camera`).
- **Agents (workers):** separate brains/workspaces for special roles (e.g. “Hetzner ops”, “Personal data”).
- **Sub-agents:** spawn background work from a main agent when you want parallelism.
- **TUI:** connect to the Gateway and switch agents/sessions.

Docs: [Nodes](https://docs.openclaw.ai/nodes), [Remote access](https://docs.openclaw.ai/gateway/remote), [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent), [Sub-agents](https://docs.openclaw.ai/tools/subagents), [TUI](https://docs.openclaw.ai/web/tui).

Can the OpenClaw browser run headless?

Yes. It’s a config option:

```
{
  browser: { headless: true },
  agents: {
    defaults: {
      sandbox: { browser: { headless: true } },
    },
  },
}
```

Default is `false` (headful). Headless is more likely to trigger anti-bot checks on some sites. See [Browser](https://docs.openclaw.ai/tools/browser).Headless uses the **same Chromium engine** and works for most automation (forms, clicks, scraping, logins). The main differences:

- No visible browser window (use screenshots if you need visuals).
- Some sites are stricter about automation in headless mode (CAPTCHAs, anti-bot).
For example, X/Twitter often blocks headless sessions.

How do I use Brave for browser control?

Set `browser.executablePath` to your Brave binary (or any Chromium-based browser) and restart the Gateway.
See the full config examples in [Browser](https://docs.openclaw.ai/tools/browser#use-brave-or-another-chromium-based-browser).

## Remote gateways and nodes

How do commands propagate between Telegram, the gateway, and nodes?

Telegram messages are handled by the **gateway**. The gateway runs the agent and
only then calls nodes over the **Gateway WebSocket** when a node tool is needed:Telegram → Gateway → Agent → `node.*` → Node → Gateway → TelegramNodes don’t see inbound provider traffic; they only receive node RPC calls.

How can my agent access my computer if the Gateway is hosted remotely?

Short answer: **pair your computer as a node**. The Gateway runs elsewhere, but it can
call `node.*` tools (screen, camera, system) on your local machine over the Gateway WebSocket.Typical setup:

1. Run the Gateway on the always-on host (VPS/home server).
2. Put the Gateway host + your computer on the same tailnet.
3. Ensure the Gateway WS is reachable (tailnet bind or SSH tunnel).
4. Open the macOS app locally and connect in **Remote over SSH** mode (or direct tailnet)
so it can register as a node.
5. Approve the node on the Gateway:

```
openclaw devices list
openclaw devices approve <requestId>
```

No separate TCP bridge is required; nodes connect over the Gateway WebSocket.Security reminder: pairing a macOS node allows `system.run` on that machine. Only
pair devices you trust, and review [Security](https://docs.openclaw.ai/gateway/security).Docs: [Nodes](https://docs.openclaw.ai/nodes), [Gateway protocol](https://docs.openclaw.ai/gateway/protocol), [macOS remote mode](https://docs.openclaw.ai/platforms/mac/remote), [Security](https://docs.openclaw.ai/gateway/security).

Tailscale is connected but I get no replies. What now?

Check the basics:

- Gateway is running: `openclaw gateway status`
- Gateway health: `openclaw status`
- Channel health: `openclaw channels status`

Then verify auth and routing:

- If you use Tailscale Serve, make sure `gateway.auth.allowTailscale` is set correctly.
- If you connect via SSH tunnel, confirm the local tunnel is up and points at the right port.
- Confirm your allowlists (DM or group) include your account.

Docs: [Tailscale](https://docs.openclaw.ai/gateway/tailscale), [Remote access](https://docs.openclaw.ai/gateway/remote), [Channels](https://docs.openclaw.ai/channels).

Can two OpenClaw instances talk to each other (local + VPS)?

Yes. There is no built-in “bot-to-bot” bridge, but you can wire it up in a few
reliable ways:**Simplest:** use a normal chat channel both bots can access (Telegram/Slack/WhatsApp).
Have Bot A send a message to Bot B, then let Bot B reply as usual.**CLI bridge (generic):** run a script that calls the other Gateway with
`openclaw agent --message ... --deliver`, targeting a chat where the other bot
listens. If one bot is on a remote VPS, point your CLI at that remote Gateway
via SSH/Tailscale (see [Remote access](https://docs.openclaw.ai/gateway/remote)).Example pattern (run from a machine that can reach the target Gateway):

```
openclaw agent --message "Hello from local bot" --deliver --channel telegram --reply-to <chat-id>
```

Tip: add a guardrail so the two bots do not loop endlessly (mention-only, channel
allowlists, or a “do not reply to bot messages” rule).Docs: [Remote access](https://docs.openclaw.ai/gateway/remote), [Agent CLI](https://docs.openclaw.ai/cli/agent), [Agent send](https://docs.openclaw.ai/tools/agent-send).

Do I need separate VPSes for multiple agents?

No. One Gateway can host multiple agents, each with its own workspace, model defaults,
and routing. That is the normal setup and it is much cheaper and simpler than running
one VPS per agent.Use separate VPSes only when you need hard isolation (security boundaries) or very
different configs that you do not want to share. Otherwise, keep one Gateway and
use multiple agents or sub-agents.

Is there a benefit to using a node on my personal laptop instead of SSH from a VPS?

Yes - nodes are the first-class way to reach your laptop from a remote Gateway, and they
unlock more than shell access. The Gateway runs on macOS/Linux (Windows via WSL2) and is
lightweight (a small VPS or Raspberry Pi-class box is fine; 4 GB RAM is plenty), so a common
setup is an always-on host plus your laptop as a node.

- **No inbound SSH required.** Nodes connect out to the Gateway WebSocket and use device pairing.
- **Safer execution controls.**`system.run` is gated by node allowlists/approvals on that laptop.
- **More device tools.** Nodes expose `canvas`, `camera`, and `screen` in addition to `system.run`.
- **Local browser automation.** Keep the Gateway on a VPS, but run Chrome locally through a node host on the laptop, or attach to local Chrome on the host via Chrome MCP.

SSH is fine for ad-hoc shell access, but nodes are simpler for ongoing agent workflows and
device automation.Docs: [Nodes](https://docs.openclaw.ai/nodes), [Nodes CLI](https://docs.openclaw.ai/cli/nodes), [Browser](https://docs.openclaw.ai/tools/browser).

Do nodes run a gateway service?

No. Only **one gateway** should run per host unless you intentionally run isolated profiles (see [Multiple gateways](https://docs.openclaw.ai/gateway/multiple-gateways)). Nodes are peripherals that connect
to the gateway (iOS/Android nodes, or macOS “node mode” in the menubar app). For headless node
hosts and CLI control, see [Node host CLI](https://docs.openclaw.ai/cli/node).A full restart is required for `gateway`, `discovery`, and `canvasHost` changes.

Is there an API / RPC way to apply config?

Yes.

- `config.schema.lookup`: inspect one config subtree with its shallow schema node, matched UI hint, and immediate child summaries before writing
- `config.get`: fetch the current snapshot + hash
- `config.patch`: safe partial update (preferred for most RPC edits); hot-reloads when possible and restarts when required
- `config.apply`: validate + replace the full config; hot-reloads when possible and restarts when required
- The owner-only `gateway` runtime tool still refuses to rewrite `tools.exec.ask` / `tools.exec.security`; legacy `tools.bash.*` aliases normalize to the same protected exec paths

Minimal sane config for a first install

```
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

This sets your workspace and restricts who can trigger the bot.

How do I set up Tailscale on a VPS and connect from my Mac?

Minimal steps:

1. **Install + login on the VPS**

```
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

2. **Install + login on your Mac**   - Use the Tailscale app and sign in to the same tailnet.
3. **Enable MagicDNS (recommended)**   - In the Tailscale admin console, enable MagicDNS so the VPS has a stable name.
4. **Use the tailnet hostname**   - SSH: `ssh user@your-vps.tailnet-xxxx.ts.net`
   - Gateway WS: `ws://your-vps.tailnet-xxxx.ts.net:18789`

If you want the Control UI without SSH, use Tailscale Serve on the VPS:

```
openclaw gateway --tailscale serve
```

This keeps the gateway bound to loopback and exposes HTTPS via Tailscale. See [Tailscale](https://docs.openclaw.ai/gateway/tailscale).

How do I connect a Mac node to a remote Gateway (Tailscale Serve)?

Serve exposes the **Gateway Control UI + WS**. Nodes connect over the same Gateway WS endpoint.Recommended setup:

1. **Make sure the VPS + Mac are on the same tailnet**.
2. **Use the macOS app in Remote mode** (SSH target can be the tailnet hostname).
The app will tunnel the Gateway port and connect as a node.
3. **Approve the node** on the gateway:

```
openclaw devices list
openclaw devices approve <requestId>
```

Docs: [Gateway protocol](https://docs.openclaw.ai/gateway/protocol), [Discovery](https://docs.openclaw.ai/gateway/discovery), [macOS remote mode](https://docs.openclaw.ai/platforms/mac/remote).

Should I install on a second laptop or just add a node?

If you only need **local tools** (screen/camera/exec) on the second laptop, add it as a
**node**. That keeps a single Gateway and avoids duplicated config. Local node tools are
currently macOS-only, but we plan to extend them to other OSes.Install a second Gateway only when you need **hard isolation** or two fully separate bots.Docs: [Nodes](https://docs.openclaw.ai/nodes), [Nodes CLI](https://docs.openclaw.ai/cli/nodes), [Multiple gateways](https://docs.openclaw.ai/gateway/multiple-gateways).

## Env vars and .env loading

How does OpenClaw load environment variables?

OpenClaw reads env vars from the parent process (shell, launchd/systemd, CI, etc.) and additionally loads:

- `.env` from the current working directory
- a global fallback `.env` from `~/.openclaw/.env` (aka `$OPENCLAW_STATE_DIR/.env`)

Neither `.env` file overrides existing env vars.You can also define inline env vars in config (applied only if missing from the process env):

```
{
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: { GROQ_API_KEY: "gsk-..." },
  },
}
```

See [/environment](https://docs.openclaw.ai/help/environment) for full precedence and sources.

I started the Gateway via the service and my env vars disappeared. What now?

Two common fixes:

1. Put the missing keys in `~/.openclaw/.env` so they’re picked up even when the service doesn’t inherit your shell env.
2. Enable shell import (opt-in convenience):

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

This runs your login shell and imports only missing expected keys (never overrides). Env var equivalents:
`OPENCLAW_LOAD_SHELL_ENV=1`, `OPENCLAW_SHELL_ENV_TIMEOUT_MS=15000`.

I set COPILOT\_GITHUB\_TOKEN, but models status shows "Shell env: off." Why?

`openclaw models status` reports whether **shell env import** is enabled. “Shell env: off”
does **not** mean your env vars are missing - it just means OpenClaw won’t load
your login shell automatically.If the Gateway runs as a service (launchd/systemd), it won’t inherit your shell
environment. Fix by doing one of these:

1. Put the token in `~/.openclaw/.env`:

```
COPILOT_GITHUB_TOKEN=...
```

2. Or enable shell import (`env.shellEnv.enabled: true`).
3. Or add it to your config `env` block (applies only if missing).

Then restart the gateway and recheck:

```
openclaw models status
```

Copilot tokens are read from `COPILOT_GITHUB_TOKEN` (also `GH_TOKEN` / `GITHUB_TOKEN`).
See [/concepts/model-providers](https://docs.openclaw.ai/concepts/model-providers) and [/environment](https://docs.openclaw.ai/help/environment).

## Sessions and multiple chats

How do I start a fresh conversation?

Send `/new` or `/reset` as a standalone message. See [Session management](https://docs.openclaw.ai/concepts/session).

Do sessions reset automatically if I never send /new?

Sessions can expire after `session.idleMinutes`, but this is **disabled by default** (default **0**).
Set it to a positive value to enable idle expiry. When enabled, the **next**
message after the idle period starts a fresh session id for that chat key.
This does not delete transcripts - it just starts a new session.

```
{
  session: {
    idleMinutes: 240,
  },
}
```

Is there a way to make a team of OpenClaw instances (one CEO and many agents)?

Yes, via **multi-agent routing** and **sub-agents**. You can create one coordinator
agent and several worker agents with their own workspaces and models.That said, this is best seen as a **fun experiment**. It is token heavy and often
less efficient than using one bot with separate sessions. The typical model we
envision is one bot you talk to, with different sessions for parallel work. That
bot can also spawn sub-agents when needed.Docs: [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent), [Sub-agents](https://docs.openclaw.ai/tools/subagents), [Agents CLI](https://docs.openclaw.ai/cli/agents).

Why did context get truncated mid-task? How do I prevent it?

Session context is limited by the model window. Long chats, large tool outputs, or many
files can trigger compaction or truncation.What helps:

- Ask the bot to summarize the current state and write it to a file.
- Use `/compact` before long tasks, and `/new` when switching topics.
- Keep important context in the workspace and ask the bot to read it back.
- Use sub-agents for long or parallel work so the main chat stays smaller.
- Pick a model with a larger context window if this happens often.

How do I completely reset OpenClaw but keep it installed?

Use the reset command:

```
openclaw reset
```

Non-interactive full reset:

```
openclaw reset --scope full --yes --non-interactive
```

Then re-run setup:

```
openclaw onboard --install-daemon
```

Notes:

- Onboarding also offers **Reset** if it sees an existing config. See [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard).
- If you used profiles (`--profile` / `OPENCLAW_PROFILE`), reset each state dir (defaults are `~/.openclaw-<profile>`).
- Dev reset: `openclaw gateway --dev --reset` (dev-only; wipes dev config + credentials + sessions + workspace).

I am getting "context too large" errors - how do I reset or compact?

Use one of these:

- **Compact** (keeps the conversation but summarizes older turns):

```
/compact
```

or `/compact <instructions>` to guide the summary.
- **Reset** (fresh session ID for the same chat key):

```
/new
/reset
```

If it keeps happening:

- Enable or tune **session pruning** (`agents.defaults.contextPruning`) to trim old tool output.
- Use a model with a larger context window.

Docs: [Compaction](https://docs.openclaw.ai/concepts/compaction), [Session pruning](https://docs.openclaw.ai/concepts/session-pruning), [Session management](https://docs.openclaw.ai/concepts/session).

Why am I seeing "LLM request rejected: messages.content.tool\_use.input field required"?

This is a provider validation error: the model emitted a `tool_use` block without the required
`input`. It usually means the session history is stale or corrupted (often after long threads
or a tool/schema change).Fix: start a fresh session with `/new` (standalone message).

Why am I getting heartbeat messages every 30 minutes?

Heartbeats run every **30m** by default ( **1h** when using OAuth auth). Tune or disable them:

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "2h", // or "0m" to disable
      },
    },
  },
}
```

If `HEARTBEAT.md` exists but is effectively empty (only blank lines and markdown
headers like `# Heading`), OpenClaw skips the heartbeat run to save API calls.
If the file is missing, the heartbeat still runs and the model decides what to do.Per-agent overrides use `agents.list[].heartbeat`. Docs: [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat).

Do I need to add a "bot account" to a WhatsApp group?

No. OpenClaw runs on **your own account**, so if you’re in the group, OpenClaw can see it.
By default, group replies are blocked until you allow senders (`groupPolicy: "allowlist"`).If you want only **you** to be able to trigger group replies:

```
{
  channels: {
    whatsapp: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15551234567"],
    },
  },
}
```

How do I get the JID of a WhatsApp group?

Option 1 (fastest): tail logs and send a test message in the group:

```
openclaw logs --follow --json
```

Look for `chatId` (or `from`) ending in `@g.us`, like:
`1234567890-1234567890@g.us`.Option 2 (if already configured/allowlisted): list groups from config:

```
openclaw directory groups list --channel whatsapp
```

Docs: [WhatsApp](https://docs.openclaw.ai/channels/whatsapp), [Directory](https://docs.openclaw.ai/cli/directory), [Logs](https://docs.openclaw.ai/cli/logs).

Why does OpenClaw not reply in a group?

Two common causes:

- Mention gating is on (default). You must @mention the bot (or match `mentionPatterns`).
- You configured `channels.whatsapp.groups` without `"*"` and the group isn’t allowlisted.

See [Groups](https://docs.openclaw.ai/channels/groups) and [Group messages](https://docs.openclaw.ai/channels/group-messages).

Do groups/threads share context with DMs?

Direct chats collapse to the main session by default. Groups/channels have their own session keys, and Telegram topics / Discord threads are separate sessions. See [Groups](https://docs.openclaw.ai/channels/groups) and [Group messages](https://docs.openclaw.ai/channels/group-messages).

How many workspaces and agents can I create?

No hard limits. Dozens (even hundreds) are fine, but watch for:

- **Disk growth:** sessions + transcripts live under `~/.openclaw/agents/<agentId>/sessions/`.
- **Token cost:** more agents means more concurrent model usage.
- **Ops overhead:** per-agent auth profiles, workspaces, and channel routing.

Tips:

- Keep one **active** workspace per agent (`agents.defaults.workspace`).
- Prune old sessions (delete JSONL or store entries) if disk grows.
- Use `openclaw doctor` to spot stray workspaces and profile mismatches.

Can I run multiple bots or chats at the same time (Slack), and how should I set that up?

Yes. Use **Multi-Agent Routing** to run multiple isolated agents and route inbound messages by
channel/account/peer. Slack is supported as a channel and can be bound to specific agents.Browser access is powerful but not “do anything a human can” - anti-bot, CAPTCHAs, and MFA can
still block automation. For the most reliable browser control, use local Chrome MCP on the host,
or use CDP on the machine that actually runs the browser.Best-practice setup:

- Always-on Gateway host (VPS/Mac mini).
- One agent per role (bindings).
- Slack channel(s) bound to those agents.
- Local browser via Chrome MCP or a node when needed.

Docs: [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent), [Slack](https://docs.openclaw.ai/channels/slack),
[Browser](https://docs.openclaw.ai/tools/browser), [Nodes](https://docs.openclaw.ai/nodes).

## Models, failover, and auth profiles

Model Q&A — defaults, selection, aliases, switching, failover, auth profiles —
lives on the [Models FAQ](https://docs.openclaw.ai/help/faq-models).

## Gateway: ports, “already running”, and remote mode

What port does the Gateway use?

`gateway.port` controls the single multiplexed port for WebSocket + HTTP (Control UI, hooks, etc.).Precedence:

```
--port > OPENCLAW_GATEWAY_PORT > gateway.port > default 18789
```

Why does openclaw gateway status say "Runtime: running" but "Connectivity probe: failed"?

Because “running” is the **supervisor’s** view (launchd/systemd/schtasks). The connectivity probe is the CLI actually connecting to the gateway WebSocket.Use `openclaw gateway status` and trust these lines:

- `Probe target:` (the URL the probe actually used)
- `Listening:` (what’s actually bound on the port)
- `Last gateway error:` (common root cause when the process is alive but the port isn’t listening)

Why does openclaw gateway status show "Config (cli)" and "Config (service)" different?

You’re editing one config file while the service is running another (often a `--profile` / `OPENCLAW_STATE_DIR` mismatch).Fix:

```
openclaw gateway install --force
```

Run that from the same `--profile` / environment you want the service to use.

What does "another gateway instance is already listening" mean?

OpenClaw enforces a runtime lock by binding the WebSocket listener immediately on startup (default `ws://127.0.0.1:18789`). If the bind fails with `EADDRINUSE`, it throws `GatewayLockError` indicating another instance is already listening.Fix: stop the other instance, free the port, or run with `openclaw gateway --port <port>`.

How do I run OpenClaw in remote mode (client connects to a Gateway elsewhere)?

Set `gateway.mode: "remote"` and point to a remote WebSocket URL, optionally with shared-secret remote credentials:

```
{
  gateway: {
    mode: "remote",
    remote: {
      url: "ws://gateway.tailnet:18789",
      token: "your-token",
      password: "your-password",
    },
  },
}
```

Notes:

- `openclaw gateway` only starts when `gateway.mode` is `local` (or you pass the override flag).
- The macOS app watches the config file and switches modes live when these values change.
- `gateway.remote.token` / `.password` are client-side remote credentials only; they do not enable local gateway auth by themselves.

The Control UI says "unauthorized" (or keeps reconnecting). What now?

Your gateway auth path and the UI’s auth method do not match.Facts (from code):

- The Control UI keeps the token in `sessionStorage` for the current browser tab session and selected gateway URL, so same-tab refreshes keep working without restoring long-lived localStorage token persistence.
- On `AUTH_TOKEN_MISMATCH`, trusted clients can attempt one bounded retry with a cached device token when the gateway returns retry hints (`canRetryWithDeviceToken=true`, `recommendedNextStep=retry_with_device_token`).
- That cached-token retry now reuses the cached approved scopes stored with the device token. Explicit `deviceToken` / explicit `scopes` callers still keep their requested scope set instead of inheriting cached scopes.
- Outside that retry path, connect auth precedence is explicit shared token/password first, then explicit `deviceToken`, then stored device token, then bootstrap token.
- Bootstrap token scope checks are role-prefixed. The built-in bootstrap operator allowlist only satisfies operator requests; node or other non-operator roles still need scopes under their own role prefix.

Fix:

- Fastest: `openclaw dashboard` (prints + copies the dashboard URL, tries to open; shows SSH hint if headless).
- If you don’t have a token yet: `openclaw doctor --generate-gateway-token`.
- If remote, tunnel first: `ssh -N -L 18789:127.0.0.1:18789 user@host` then open `http://127.0.0.1:18789/`.
- Shared-secret mode: set `gateway.auth.token` / `OPENCLAW_GATEWAY_TOKEN` or `gateway.auth.password` / `OPENCLAW_GATEWAY_PASSWORD`, then paste the matching secret in Control UI settings.
- Tailscale Serve mode: make sure `gateway.auth.allowTailscale` is enabled and you are opening the Serve URL, not a raw loopback/tailnet URL that bypasses Tailscale identity headers.
- Trusted-proxy mode: make sure you are coming through the configured identity-aware proxy, not a raw gateway URL. Same-host loopback proxies also need `gateway.auth.trustedProxy.allowLoopback = true`.
- If mismatch persists after the one retry, rotate/re-approve the paired device token:
  - `openclaw devices list`
  - `openclaw devices rotate --device <id> --role operator`
- If that rotate call says it was denied, check two things:
  - paired-device sessions can rotate only their **own** device unless they also have `operator.admin`
  - explicit `--scope` values cannot exceed the caller’s current operator scopes
- Still stuck? Run `openclaw status --all` and follow [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting). See [Dashboard](https://docs.openclaw.ai/web/dashboard) for auth details.

I set gateway.bind tailnet but it cannot bind and nothing listens

`tailnet` bind picks a Tailscale IP from your network interfaces (100.64.0.0/10). If the machine isn’t on Tailscale (or the interface is down), there’s nothing to bind to.Fix:

- Start Tailscale on that host (so it has a 100.x address), or
- Switch to `gateway.bind: "loopback"` / `"lan"`.

Note: `tailnet` is explicit. `auto` prefers loopback; use `gateway.bind: "tailnet"` when you want a tailnet-only bind.

Can I run multiple Gateways on the same host?

Usually no - one Gateway can run multiple messaging channels and agents. Use multiple Gateways only when you need redundancy (ex: rescue bot) or hard isolation.Yes, but you must isolate:

- `OPENCLAW_CONFIG_PATH` (per-instance config)
- `OPENCLAW_STATE_DIR` (per-instance state)
- `agents.defaults.workspace` (workspace isolation)
- `gateway.port` (unique ports)

Quick setup (recommended):

- Use `openclaw --profile <name> ...` per instance (auto-creates `~/.openclaw-<name>`).
- Set a unique `gateway.port` in each profile config (or pass `--port` for manual runs).
- Install a per-profile service: `openclaw --profile <name> gateway install`.

Profiles also suffix service names (`ai.openclaw.<profile>`; legacy `com.openclaw.*`, `openclaw-gateway-<profile>.service`, `OpenClaw Gateway (<profile>)`).
Full guide: [Multiple gateways](https://docs.openclaw.ai/gateway/multiple-gateways).

What does "invalid handshake" / code 1008 mean?

The Gateway is a **WebSocket server**, and it expects the very first message to
be a `connect` frame. If it receives anything else, it closes the connection
with **code 1008** (policy violation).Common causes:

- You opened the **HTTP** URL in a browser (`http://...`) instead of a WS client.
- You used the wrong port or path.
- A proxy or tunnel stripped auth headers or sent a non-Gateway request.

Quick fixes:

1. Use the WS URL: `ws://<host>:18789` (or `wss://...` if HTTPS).
2. Don’t open the WS port in a normal browser tab.
3. If auth is on, include the token/password in the `connect` frame.

If you’re using the CLI or TUI, the URL should look like:

```
openclaw tui --url ws://<host>:18789 --token <token>
```

Protocol details: [Gateway protocol](https://docs.openclaw.ai/gateway/protocol).

## Logging and debugging

Where are logs?

File logs (structured):

```
/tmp/openclaw/openclaw-YYYY-MM-DD.log
```

You can set a stable path via `logging.file`. File log level is controlled by `logging.level`. Console verbosity is controlled by `--verbose` and `logging.consoleLevel`.Fastest log tail:

```
openclaw logs --follow
```

Service/supervisor logs (when the gateway runs via launchd/systemd):

- macOS: `$OPENCLAW_STATE_DIR/logs/gateway.log` and `gateway.err.log` (default: `~/.openclaw/logs/...`; profiles use `~/.openclaw-<profile>/logs/...`)
- Linux: `journalctl --user -u openclaw-gateway[-<profile>].service -n 200 --no-pager`
- Windows: `schtasks /Query /TN "OpenClaw Gateway (<profile>)" /V /FO LIST`

See [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting) for more.

How do I start/stop/restart the Gateway service?

Use the gateway helpers:

```
openclaw gateway status
openclaw gateway restart
```

If you run the gateway manually, `openclaw gateway --force` can reclaim the port. See [Gateway](https://docs.openclaw.ai/gateway).

I closed my terminal on Windows - how do I restart OpenClaw?

There are **two Windows install modes**:**1) WSL2 (recommended):** the Gateway runs inside Linux.Open PowerShell, enter WSL, then restart:

```
wsl
openclaw gateway status
openclaw gateway restart
```

If you never installed the service, start it in the foreground:

```
openclaw gateway run
```

**2) Native Windows (not recommended):** the Gateway runs directly in Windows.Open PowerShell and run:

```
openclaw gateway status
openclaw gateway restart
```

If you run it manually (no service), use:

```
openclaw gateway run
```

Docs: [Windows (WSL2)](https://docs.openclaw.ai/platforms/windows), [Gateway service runbook](https://docs.openclaw.ai/gateway).

The Gateway is up but replies never arrive. What should I check?

Start with a quick health sweep:

```
openclaw status
openclaw models status
openclaw channels status
openclaw logs --follow
```

Common causes:

- Model auth not loaded on the **gateway host** (check `models status`).
- Channel pairing/allowlist blocking replies (check channel config + logs).
- WebChat/Dashboard is open without the right token.

If you are remote, confirm the tunnel/Tailscale connection is up and that the
Gateway WebSocket is reachable.Docs: [Channels](https://docs.openclaw.ai/channels), [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting), [Remote access](https://docs.openclaw.ai/gateway/remote).

"Disconnected from gateway: no reason" - what now?

This usually means the UI lost the WebSocket connection. Check:

1. Is the Gateway running? `openclaw gateway status`
2. Is the Gateway healthy? `openclaw status`
3. Does the UI have the right token? `openclaw dashboard`
4. If remote, is the tunnel/Tailscale link up?

Then tail logs:

```
openclaw logs --follow
```

Docs: [Dashboard](https://docs.openclaw.ai/web/dashboard), [Remote access](https://docs.openclaw.ai/gateway/remote), [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting).

Telegram setMyCommands fails. What should I check?

Start with logs and channel status:

```
openclaw channels status
openclaw channels logs --channel telegram
```

Then match the error:

- `BOT_COMMANDS_TOO_MUCH`: the Telegram menu has too many entries. OpenClaw already trims to the Telegram limit and retries with fewer commands, but some menu entries still need to be dropped. Reduce plugin/skill/custom commands, or disable `channels.telegram.commands.native` if you do not need the menu.
- `TypeError: fetch failed`, `Network request for 'setMyCommands' failed!`, or similar network errors: if you are on a VPS or behind a proxy, confirm outbound HTTPS is allowed and DNS works for `api.telegram.org`.

If the Gateway is remote, make sure you are looking at logs on the Gateway host.Docs: [Telegram](https://docs.openclaw.ai/channels/telegram), [Channel troubleshooting](https://docs.openclaw.ai/channels/troubleshooting).

TUI shows no output. What should I check?

First confirm the Gateway is reachable and the agent can run:

```
openclaw status
openclaw models status
openclaw logs --follow
```

In the TUI, use `/status` to see the current state. If you expect replies in a chat
channel, make sure delivery is enabled (`/deliver on`).Docs: [TUI](https://docs.openclaw.ai/web/tui), [Slash commands](https://docs.openclaw.ai/tools/slash-commands).

How do I completely stop then start the Gateway?

If you installed the service:

```
openclaw gateway stop
openclaw gateway start
```

This stops/starts the **supervised service** (launchd on macOS, systemd on Linux).
Use this when the Gateway runs in the background as a daemon.If you’re running in the foreground, stop with Ctrl-C, then:

```
openclaw gateway run
```

Docs: [Gateway service runbook](https://docs.openclaw.ai/gateway).

ELI5: openclaw gateway restart vs openclaw gateway

- `openclaw gateway restart`: restarts the **background service** (launchd/systemd).
- `openclaw gateway`: runs the gateway **in the foreground** for this terminal session.

If you installed the service, use the gateway commands. Use `openclaw gateway` when
you want a one-off, foreground run.

Fastest way to get more details when something fails

Start the Gateway with `--verbose` to get more console detail. Then inspect the log file for channel auth, model routing, and RPC errors.

## Media and attachments

My skill generated an image/PDF, but nothing was sent

Outbound attachments from the agent must include a `MEDIA:<path-or-url>` line (on its own line). See [OpenClaw assistant setup](https://docs.openclaw.ai/start/openclaw) and [Agent send](https://docs.openclaw.ai/tools/agent-send).CLI sending:

```
openclaw message send --target +15555550123 --message "Here you go" --media /path/to/file.png
```

Also check:

- The target channel supports outbound media and isn’t blocked by allowlists.
- The file is within the provider’s size limits (images are resized to max 2048px).
- `tools.fs.workspaceOnly=true` keeps local-path sends limited to workspace, temp/media-store, and sandbox-validated files.
- `tools.fs.workspaceOnly=false` lets `MEDIA:` send host-local files the agent can already read, but only for media plus safe document types (images, audio, video, PDF, and Office docs). Plain text and secret-like files are still blocked.

See [Images](https://docs.openclaw.ai/nodes/images).

## Security and access control

Is it safe to expose OpenClaw to inbound DMs?

Treat inbound DMs as untrusted input. Defaults are designed to reduce risk:

- Default behavior on DM-capable channels is **pairing**:

  - Unknown senders receive a pairing code; the bot does not process their message.
  - Approve with: `openclaw pairing approve --channel <channel> [--account <id>] <code>`
  - Pending requests are capped at **3 per channel**; check `openclaw pairing list --channel <channel> [--account <id>]` if a code didn’t arrive.
- Opening DMs publicly requires explicit opt-in (`dmPolicy: "open"` and allowlist `"*"`).

Run `openclaw doctor` to surface risky DM policies.

Is prompt injection only a concern for public bots?

No. Prompt injection is about **untrusted content**, not just who can DM the bot.
If your assistant reads external content (web search/fetch, browser pages, emails,
docs, attachments, pasted logs), that content can include instructions that try
to hijack the model. This can happen even if **you are the only sender**.The biggest risk is when tools are enabled: the model can be tricked into
exfiltrating context or calling tools on your behalf. Reduce the blast radius by:

- using a read-only or tool-disabled “reader” agent to summarize untrusted content
- keeping `web_search` / `web_fetch` / `browser` off for tool-enabled agents
- treating decoded file/document text as untrusted too: OpenResponses
`input_file` and media-attachment extraction both wrap extracted text in
explicit external-content boundary markers instead of passing raw file text
- sandboxing and strict tool allowlists

Details: [Security](https://docs.openclaw.ai/gateway/security).

Should my bot have its own email, GitHub account, or phone number?

Yes, for most setups. Isolating the bot with separate accounts and phone numbers
reduces the blast radius if something goes wrong. This also makes it easier to rotate
credentials or revoke access without impacting your personal accounts.Start small. Give access only to the tools and accounts you actually need, and expand
later if required.Docs: [Security](https://docs.openclaw.ai/gateway/security), [Pairing](https://docs.openclaw.ai/channels/pairing).

Can I give it autonomy over my text messages and is that safe?

We do **not** recommend full autonomy over your personal messages. The safest pattern is:

- Keep DMs in **pairing mode** or a tight allowlist.
- Use a **separate number or account** if you want it to message on your behalf.
- Let it draft, then **approve before sending**.

If you want to experiment, do it on a dedicated account and keep it isolated. See
[Security](https://docs.openclaw.ai/gateway/security).

Can I use cheaper models for personal assistant tasks?

Yes, **if** the agent is chat-only and the input is trusted. Smaller tiers are
more susceptible to instruction hijacking, so avoid them for tool-enabled agents
or when reading untrusted content. If you must use a smaller model, lock down
tools and run inside a sandbox. See [Security](https://docs.openclaw.ai/gateway/security).

I ran /start in Telegram but did not get a pairing code

Pairing codes are sent **only** when an unknown sender messages the bot and
`dmPolicy: "pairing"` is enabled. `/start` by itself doesn’t generate a code.Check pending requests:

```
openclaw pairing list telegram
```

If you want immediate access, allowlist your sender id or set `dmPolicy: "open"`
for that account.

WhatsApp: will it message my contacts? How does pairing work?

No. Default WhatsApp DM policy is **pairing**. Unknown senders only get a pairing code and their message is **not processed**. OpenClaw only replies to chats it receives or to explicit sends you trigger.Approve pairing with:

```
openclaw pairing approve whatsapp <code>
```

List pending requests:

```
openclaw pairing list whatsapp
```

Wizard phone number prompt: it’s used to set your **allowlist/owner** so your own DMs are permitted. It’s not used for auto-sending. If you run on your personal WhatsApp number, use that number and enable `channels.whatsapp.selfChatMode`.

## Chat commands, aborting tasks, and “it will not stop”

How do I stop internal system messages from showing in chat?

Most internal or tool messages only appear when **verbose**, **trace**, or **reasoning** is enabled
for that session.Fix in the chat where you see it:

```
/verbose off
/trace off
/reasoning off
```

If it is still noisy, check the session settings in the Control UI and set verbose
to **inherit**. Also confirm you are not using a bot profile with `verboseDefault` set
to `on` in config.Docs: [Thinking and verbose](https://docs.openclaw.ai/tools/thinking), [Security](https://docs.openclaw.ai/gateway/security/index#reasoning-and-verbose-output-in-groups).

How do I stop/cancel a running task?

Send any of these **as a standalone message** (no slash):

```
stop
stop action
stop current action
stop run
stop current run
stop agent
stop the agent
stop openclaw
openclaw stop
stop don't do anything
stop do not do anything
stop doing anything
please stop
stop please
abort
esc
wait
exit
interrupt
```

These are abort triggers (not slash commands).For background processes (from the exec tool), you can ask the agent to run:

```
process action:kill sessionId:XXX
```

Slash commands overview: see [Slash commands](https://docs.openclaw.ai/tools/slash-commands).Most commands must be sent as a **standalone** message that starts with `/`, but a few shortcuts (like `/status`) also work inline for allowlisted senders.

How do I send a Discord message from Telegram? ("Cross-context messaging denied")

OpenClaw blocks **cross-provider** messaging by default. If a tool call is bound
to Telegram, it won’t send to Discord unless you explicitly allow it.Enable cross-provider messaging for the agent:

```
{
  tools: {
    message: {
      crossContext: {
        allowAcrossProviders: true,
        marker: { enabled: true, prefix: "[from {channel}] " },
      },
    },
  },
}
```

Restart the gateway after editing config.

Why does it feel like the bot "ignores" rapid-fire messages?

Queue mode controls how new messages interact with an in-flight run. Use `/queue` to change modes:

- `steer` \- queue all pending steering for the next model boundary in the current run
- `queue` \- legacy one-at-a-time steering
- `followup` \- run messages one at a time
- `collect` \- batch messages and reply once
- `steer-backlog` \- steer now, then process backlog
- `interrupt` \- abort current run and start fresh

Default mode is `steer`. You can add options like `debounce:0.5s cap:25 drop:summarize` for followup modes. See [Command queue](https://docs.openclaw.ai/concepts/queue) and [Steering queue](https://docs.openclaw.ai/concepts/queue-steering).

## Miscellaneous

What is the default model for Anthropic with an API key?

In OpenClaw, credentials and model selection are separate. Setting `ANTHROPIC_API_KEY` (or storing an Anthropic API key in auth profiles) enables authentication, but the actual default model is whatever you configure in `agents.defaults.model.primary` (for example, `anthropic/claude-sonnet-4-6` or `anthropic/claude-opus-4-6`). If you see `No credentials found for profile "anthropic:default"`, it means the Gateway couldn’t find Anthropic credentials in the expected `auth-profiles.json` for the agent that’s running.

* * *

Still stuck? Ask in [Discord](https://discord.com/invite/clawd) or open a [GitHub discussion](https://github.com/openclaw/openclaw/discussions).

## Related

- [First-run FAQ](https://docs.openclaw.ai/help/faq-first-run) — install, onboard, auth, subscriptions, early failures
- [Models FAQ](https://docs.openclaw.ai/help/faq-models) — model selection, failover, auth profiles
- [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) — symptom-first triage

[Debugging](https://docs.openclaw.ai/help/debugging) [First-run FAQ](https://docs.openclaw.ai/help/faq-first-run)

Ctrl+I

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

Short answer: **possible, not recommended**. The update flow can restart the
Gateway (which drops the active session), may need a clean git checkout, and
can prompt for confirmation. Safer: run updates from a shell as the operator.Use the CLI:

```
openclaw update
openclaw update status
openclaw update --channel stable|beta|dev
openclaw update --tag <dist-tag|version>
openclaw update --no-restart
```

If you must automate from an agent:

```
openclaw update --yes --no-restart
openclaw gateway restart
```

Docs: [Update](https://docs.openclaw.ai/cli/update), [Updating](https://docs.openclaw.ai/install/updating).

What does onboarding actually do?

`openclaw onboard` is the recommended setup path. In **local mode** it walks you through:

- **Model/auth setup** (provider OAuth, API keys, Anthropic setup-token, plus local model options such as LM Studio)
- **Workspace** location + bootstrap files
- **Gateway settings** (bind/port/auth/tailscale)
- **Channels** (WhatsApp, Telegram, Discord, Mattermost, Signal, iMessage, plus bundled channel plugins like QQ Bot)
- **Daemon install** (LaunchAgent on macOS; systemd user unit on Linux/WSL2)
- **Health checks** and **skills** selection

It also warns if your configured model is unknown or missing auth.

Do I need a Claude or OpenAI subscription to run this?

No. You can run OpenClaw with **API keys** (Anthropic/OpenAI/others) or with
**local-only models** so your data stays on your device. Subscriptions (Claude
Pro/Max or OpenAI Codex) are optional ways to authenticate those providers.For Anthropic in OpenClaw, the practical split is:

- **Anthropic API key**: normal Anthropic API billing
- **Claude CLI / Claude subscription auth in OpenClaw**: Anthropic staff
told us this usage is allowed again, and OpenClaw is treating `claude -p`
usage as sanctioned for this integration unless Anthropic publishes a new
policy

For long-lived gateway hosts, Anthropic API keys are still the more
predictable setup. OpenAI Codex OAuth is explicitly supported for external
tools like OpenClaw.OpenClaw also supports other hosted subscription-style options including
**Qwen Cloud Coding Plan**, **MiniMax Coding Plan**, and
**Z.AI / GLM Coding Plan**.Docs: [Anthropic](https://docs.openclaw.ai/providers/anthropic), [OpenAI](https://docs.openclaw.ai/providers/openai),
[Qwen Cloud](https://docs.openclaw.ai/providers/qwen),
[MiniMax](https://docs.openclaw.ai/providers/minimax), [GLM Models](https://docs.openclaw.ai/providers/glm),
[Local models](https://docs.openclaw.ai/gateway/local-models), [Models](https://docs.openclaw.ai/concepts/models).

Can I use Claude Max subscription without an API key?

Yes.Anthropic staff told us OpenClaw-style Claude CLI usage is allowed again, so
OpenClaw treats Claude subscription auth and `claude -p` usage as sanctioned
for this integration unless Anthropic publishes a new policy. If you want
the most predictable server-side setup, use an Anthropic API key instead.

Do you support Claude subscription auth (Claude Pro or Max)?

Yes.Anthropic staff told us this usage is allowed again, so OpenClaw treats
Claude CLI reuse and `claude -p` usage as sanctioned for this integration
unless Anthropic publishes a new policy.Anthropic setup-token is still available as a supported OpenClaw token path, but OpenClaw now prefers Claude CLI reuse and `claude -p` when available.
For production or multi-user workloads, Anthropic API key auth is still the
safer, more predictable choice. If you want other subscription-style hosted
options in OpenClaw, see [OpenAI](https://docs.openclaw.ai/providers/openai), [Qwen / Model\\
Cloud](https://docs.openclaw.ai/providers/qwen), [MiniMax](https://docs.openclaw.ai/providers/minimax), and [GLM\\
Models](https://docs.openclaw.ai/providers/glm).

Why am I seeing HTTP 429 rate\_limit\_error from Anthropic?

That means your **Anthropic quota/rate limit** is exhausted for the current window. If you
use **Claude CLI**, wait for the window to reset or upgrade your plan. If you
use an **Anthropic API key**, check the Anthropic Console
for usage/billing and raise limits as needed.If the message is specifically:
`Extra usage is required for long context requests`, the request is trying to use
Anthropic’s 1M context beta (`context1m: true`). That only works when your
credential is eligible for long-context billing (API key billing or the
OpenClaw Claude-login path with Extra Usage enabled).Tip: set a **fallback model** so OpenClaw can keep replying while a provider is rate-limited.
See [Models](https://docs.openclaw.ai/cli/models), [OAuth](https://docs.openclaw.ai/concepts/oauth), and
[/gateway/troubleshooting#anthropic-429-extra-usage-required-for-long-context](https://docs.openclaw.ai/gateway/troubleshooting#anthropic-429-extra-usage-required-for-long-context).

Is AWS Bedrock supported?

Yes. OpenClaw has a bundled **Amazon Bedrock (Converse)** provider. With AWS env markers present, OpenClaw can auto-discover the streaming/text Bedrock catalog and merge it as an implicit `amazon-bedrock` provider; otherwise you can explicitly enable `plugins.entries.amazon-bedrock.config.discovery.enabled` or add a manual provider entry. See [Amazon Bedrock](https://docs.openclaw.ai/providers/bedrock) and [Model providers](https://docs.openclaw.ai/providers/models). If you prefer a managed key flow, an OpenAI-compatible proxy in front of Bedrock is still a valid option.

How does Codex auth work?

OpenClaw supports **OpenAI Code (Codex)** via OAuth (ChatGPT sign-in). Use
`openai/gpt-5.5` with `agentRuntime.id: "codex"` for the common setup:
ChatGPT/Codex subscription auth plus native Codex app-server execution. Use
`openai-codex/gpt-5.5` only when you want Codex OAuth through the default
PI runner. Use `openai/gpt-5.5` without the Codex runtime override for
direct OpenAI API-key access.
See [Model providers](https://docs.openclaw.ai/concepts/model-providers) and [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard).

Why does OpenClaw still mention openai-codex?

`openai-codex` is the provider and auth-profile id for ChatGPT/Codex OAuth.
It is also the explicit PI model prefix for Codex OAuth:

- `openai/gpt-5.5` \+ `agentRuntime.id: "codex"` = ChatGPT/Codex subscription auth with native Codex runtime
- `openai-codex/gpt-5.5` = Codex OAuth route in PI
- `openai/gpt-5.5` without a Codex runtime override = direct OpenAI API-key route in PI
- `openai-codex:...` = auth profile id, not a model ref

If you want the direct OpenAI Platform billing/limit path, set
`OPENAI_API_KEY`. If you want ChatGPT/Codex subscription auth, sign in with
`openclaw models auth login --provider openai-codex`. For native Codex
runtime, keep the model ref as `openai/gpt-5.5` and set
`agentRuntime.id: "codex"`. Use `openai-codex/*` model refs only for PI
runs.

Why can Codex OAuth limits differ from ChatGPT web?

Codex OAuth uses OpenAI-managed, plan-dependent quota windows. In practice,
those limits can differ from the ChatGPT website/app experience, even when
both are tied to the same account.OpenClaw can show the currently visible provider usage/quota windows in
`openclaw models status`, but it does not invent or normalize ChatGPT-web
entitlements into direct API access. If you want the direct OpenAI Platform
billing/limit path, use `openai/*` with an API key.

Do you support OpenAI subscription auth (Codex OAuth)?

Yes. OpenClaw fully supports **OpenAI Code (Codex) subscription OAuth**.
OpenAI explicitly allows subscription OAuth usage in external tools/workflows
like OpenClaw. Onboarding can run the OAuth flow for you.See [OAuth](https://docs.openclaw.ai/concepts/oauth), [Model providers](https://docs.openclaw.ai/concepts/model-providers), and [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard).

How do I set up Gemini CLI OAuth?

Gemini CLI uses a **plugin auth flow**, not a client id or secret in `openclaw.json`.Steps:

1. Install Gemini CLI locally so `gemini` is on `PATH`
   - Homebrew: `brew install gemini-cli`
   - npm: `npm install -g @google/gemini-cli`
2. Enable the plugin: `openclaw plugins enable google`
3. Login: `openclaw models auth login --provider google-gemini-cli --set-default`
4. Default model after login: `google-gemini-cli/gemini-3-flash-preview`
5. If requests fail, set `GOOGLE_CLOUD_PROJECT` or `GOOGLE_CLOUD_PROJECT_ID` on the gateway host

This stores OAuth tokens in auth profiles on the gateway host. Details: [Model providers](https://docs.openclaw.ai/concepts/model-providers).

Is a local model OK for casual chats?

Usually no. OpenClaw needs large context + strong safety; small cards truncate and leak. If you must, run the **largest** model build you can locally (LM Studio) and see [/gateway/local-models](https://docs.openclaw.ai/gateway/local-models). Smaller/quantized models increase prompt-injection risk - see [Security](https://docs.openclaw.ai/gateway/security).

How do I keep hosted model traffic in a specific region?

Pick region-pinned endpoints. OpenRouter exposes US-hosted options for MiniMax, Kimi, and GLM; choose the US-hosted variant to keep data in-region. You can still list Anthropic/OpenAI alongside these by using `models.mode: "merge"` so fallbacks stay available while respecting the regioned provider you select.

Do I have to buy a Mac Mini to install this?

No. OpenClaw runs on macOS or Linux (Windows via WSL2). A Mac mini is optional - some people
buy one as an always-on host, but a small VPS, home server, or Raspberry Pi-class box works too.You only need a Mac **for macOS-only tools**. For iMessage, use [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles) (recommended) - the BlueBubbles server runs on any Mac, and the Gateway can run on Linux or elsewhere. If you want other macOS-only tools, run the Gateway on a Mac or pair a macOS node.Docs: [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles), [Nodes](https://docs.openclaw.ai/nodes), [Mac remote mode](https://docs.openclaw.ai/platforms/mac/remote).

Do I need a Mac mini for iMessage support?

You need **some macOS device** signed into Messages. It does **not** have to be a Mac mini -
any Mac works. **Use [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles)** (recommended) for iMessage - the BlueBubbles server runs on macOS, while the Gateway can run on Linux or elsewhere.Common setups:

- Run the Gateway on Linux/VPS, and run the BlueBubbles server on any Mac signed into Messages.
- Run everything on the Mac if you want the simplest single-machine setup.

Docs: [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles), [Nodes](https://docs.openclaw.ai/nodes),
[Mac remote mode](https://docs.openclaw.ai/platforms/mac/remote).

If I buy a Mac mini to run OpenClaw, can I connect it to my MacBook Pro?

Yes. The **Mac mini can run the Gateway**, and your MacBook Pro can connect as a
**node** (companion device). Nodes don’t run the Gateway - they provide extra
capabilities like screen/camera/canvas and `system.run` on that device.Common pattern:

- Gateway on the Mac mini (always-on).
- MacBook Pro runs the macOS app or a node host and pairs to the Gateway.
- Use `openclaw nodes status` / `openclaw nodes list` to see it.

Docs: [Nodes](https://docs.openclaw.ai/nodes), [Nodes CLI](https://docs.openclaw.ai/cli/nodes).

Can I use Bun?

Bun is **not recommended**. We see runtime bugs, especially with WhatsApp and Telegram.
Use **Node** for stable gateways.If you still want to experiment with Bun, do it on a non-production gateway
without WhatsApp/Telegram.

Telegram: what goes in allowFrom?

`channels.telegram.allowFrom` is **the human sender’s Telegram user ID** (numeric). It is not the bot username.Setup asks for numeric user IDs only. If you already have legacy `@username` entries in config, `openclaw doctor --fix` can try to resolve them.Safer (no third-party bot):

- DM your bot, then run `openclaw logs --follow` and read `from.id`.

Official Bot API:

- DM your bot, then call `https://api.telegram.org/bot<bot_token>/getUpdates` and read `message.from.id`.

Third-party (less private):

- DM `@userinfobot` or `@getidsbot`.

See [/channels/telegram](https://docs.openclaw.ai/channels/telegram#access-control-and-activation).

Can multiple people use one WhatsApp number with different OpenClaw instances?

Yes, via **multi-agent routing**. Bind each sender’s WhatsApp **DM** (peer `kind: "direct"`, sender E.164 like `+15551234567`) to a different `agentId`, so each person gets their own workspace and session store. Replies still come from the **same WhatsApp account**, and DM access control (`channels.whatsapp.dmPolicy` / `channels.whatsapp.allowFrom`) is global per WhatsApp account. See [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent) and [WhatsApp](https://docs.openclaw.ai/channels/whatsapp).

Can I run a "fast chat" agent and an "Opus for coding" agent?

Yes. Use multi-agent routing: give each agent its own default model, then bind inbound routes (provider account or specific peers) to each agent. Example config lives in [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent). See also [Models](https://docs.openclaw.ai/concepts/models) and [Configuration](https://docs.openclaw.ai/gateway/configuration).

Does Homebrew work on Linux?

Yes. Homebrew supports Linux (Linuxbrew). Quick setup:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.profile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
brew install <formula>
```

If you run OpenClaw via systemd, ensure the service PATH includes `/home/linuxbrew/.linuxbrew/bin` (or your brew prefix) so `brew`-installed tools resolve in non-login shells.
Recent builds also prepend common user bin dirs on Linux systemd services (for example `~/.local/bin`, `~/.npm-global/bin`, `~/.local/share/pnpm`, `~/.bun/bin`) and honor `PNPM_HOME`, `NPM_CONFIG_PREFIX`, `BUN_INSTALL`, `VOLTA_HOME`, `ASDF_DATA_DIR`, `NVM_DIR`, and `FNM_DIR` when set.

Difference between the hackable git install and npm install

- **Hackable (git) install:** full source checkout, editable, best for contributors.
You run builds locally and can patch code/docs.
- **npm install:** global CLI install, no repo, best for “just run it.”
Updates come from npm dist-tags.

Docs: [Getting started](https://docs.openclaw.ai/start/getting-started), [Updating](https://docs.openclaw.ai/install/updating).

Can I switch between npm and git installs later?

Yes. Use `openclaw update --channel ...` when OpenClaw is already installed.
This **does not delete your data** \- it only changes the OpenClaw code install.
Your state (`~/.openclaw`) and workspace (`~/.openclaw/workspace`) stay untouched.From npm to git:

```
openclaw update --channel dev
```

From git to npm:

```
openclaw update --channel stable
```

Add `--dry-run` to preview the planned mode switch first. The updater runs
Doctor follow-ups, refreshes plugin sources for the target channel, and
restarts the gateway unless you pass `--no-restart`.The installer can force either mode too:

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method npm
```

Backup tips: see [Backup strategy](https://docs.openclaw.ai/help/faq#where-things-live-on-disk).

Should I run the Gateway on my laptop or a VPS?

Short answer: **if you want 24/7 reliability, use a VPS**. If you want the
lowest friction and you’re okay with sleep/restarts, run it locally.**Laptop (local Gateway)**

- **Pros:** no server cost, direct access to local files, live browser window.
- **Cons:** sleep/network drops = disconnects, OS updates/reboots interrupt, must stay awake.

**VPS / cloud**

- **Pros:** always-on, stable network, no laptop sleep issues, easier to keep running.
- **Cons:** often run headless (use screenshots), remote file access only, you must SSH for updates.

**OpenClaw-specific note:** WhatsApp/Telegram/Slack/Mattermost/Discord all work fine from a VPS. The only real trade-off is **headless browser** vs a visible window. See [Browser](https://docs.openclaw.ai/tools/browser).**Recommended default:** VPS if you had gateway disconnects before. Local is great when you’re actively using the Mac and want local file access or UI automation with a visible browser.

How important is it to run OpenClaw on a dedicated machine?

Not required, but **recommended for reliability and isolation**.

- **Dedicated host (VPS/Mac mini/Pi):** always-on, fewer sleep/reboot interruptions, cleaner permissions, easier to keep running.
- **Shared laptop/desktop:** totally fine for testing and active use, but expect pauses when the machine sleeps or updates.

If you want the best of both worlds, keep the Gateway on a dedicated host and pair your laptop as a **node** for local screen/camera/exec tools. See [Nodes](https://docs.openclaw.ai/nodes).
For security guidance, read [Security](https://docs.openclaw.ai/gateway/security).

What are the minimum VPS requirements and recommended OS?

OpenClaw is lightweight. For a basic Gateway + one chat channel:

- **Absolute minimum:** 1 vCPU, 1GB RAM, ~500MB disk.
- **Recommended:** 1-2 vCPU, 2GB RAM or more for headroom (logs, media, multiple channels). Node tools and browser automation can be resource hungry.

OS: use **Ubuntu LTS** (or any modern Debian/Ubuntu). The Linux install path is best tested there.Docs: [Linux](https://docs.openclaw.ai/platforms/linux), [VPS hosting](https://docs.openclaw.ai/vps).

Can I run OpenClaw in a VM and what are the requirements?

Yes. Treat a VM the same as a VPS: it needs to be always on, reachable, and have enough
RAM for the Gateway and any channels you enable.Baseline guidance:

- **Absolute minimum:** 1 vCPU, 1GB RAM.
- **Recommended:** 2GB RAM or more if you run multiple channels, browser automation, or media tools.
- **OS:** Ubuntu LTS or another modern Debian/Ubuntu.

If you are on Windows, **WSL2 is the easiest VM style setup** and has the best tooling
compatibility. See [Windows](https://docs.openclaw.ai/platforms/windows), [VPS hosting](https://docs.openclaw.ai/vps).
If you are running macOS in a VM, see [macOS VM](https://docs.openclaw.ai/install/macos-vm).

## Related

- [FAQ](https://docs.openclaw.ai/help/faq) — the main FAQ (models, sessions, gateway, security, more)
- [Install overview](https://docs.openclaw.ai/install)
- [Getting started](https://docs.openclaw.ai/start/getting-started)
- [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting)

[FAQ](https://docs.openclaw.ai/help/faq) [Models FAQ](https://docs.openclaw.ai/help/faq-models)

Ctrl+I

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
 \\* \*\*Open source and hackable:\*\* inspect, extend, and self-host without vendor lock-in.

 Docs: \[Gateway\](/gateway), \[Channels\](/channels), \[Multi-agent\](/concepts/multi-agent),
 \[Memory\](/concepts/memory).

 Good first projects:

 \\* Build a website (WordPress, Shopify, or a simple static site).
 \\* Prototype a mobile app (outline, screens, API plan).
 \\* Organize files and folders (cleanup, naming, tagging).
 \\* Connect Gmail and automate summaries or follow ups.

 It can handle large tasks, but it works best when you split them into phases and
 use sub agents for parallel work.

 Everyday wins usually look like:

 \\* \*\*Personal briefings:\*\* summaries of inbox, calendar, and news you care about.
 \\* \*\*Research and drafting:\*\* quick research, summaries, and first drafts for emails or docs.
 \\* \*\*Reminders and follow ups:\*\* cron or heartbeat driven nudges and checklists.
 \\* \*\*Browser automation:\*\* filling forms, collecting data, and repeating web tasks.
 \\* \*\*Cross device coordination:\*\* send a task from your phone, let the Gateway run it on a server, and get the result back in chat.

 Yes for \*\*research, qualification, and drafting\*\*. It can scan sites, build shortlists,
 summarize prospects, and write outreach or ad copy drafts.

 For \*\*outreach or ad runs\*\*, keep a human in the loop. Avoid spam, follow local laws and
 platform policies, and review anything before it is sent. The safest pattern is to let
 OpenClaw draft and you approve.

 Docs: \[Security\](/gateway/security).

 OpenClaw is a \*\*personal assistant\*\* and coordination layer, not an IDE replacement. Use
 Claude Code or Codex for the fastest direct coding loop inside a repo. Use OpenClaw when you
 want durable memory, cross-device access, and tool orchestration.

 Advantages:

 \\* \*\*Persistent memory + workspace\*\* across sessions
 \\* \*\*Multi-platform access\*\* (WhatsApp, Telegram, TUI, WebChat)
 \\* \*\*Tool orchestration\*\* (browser, files, scheduling, hooks)
 \\* \*\*Always-on Gateway\*\* (run on a VPS, interact from anywhere)
 \\* \*\*Nodes\*\* for local browser/screen/camera/exec

 Showcase: \[https://openclaw.ai/showcase\](https://openclaw.ai/showcase)

\## Skills and automation

 Use managed overrides instead of editing the repo copy. Put your changes in \`~/.openclaw/skills//SKILL.md\` (or add a folder via \`skills.load.extraDirs\` in \`~/.openclaw/openclaw.json\`). Precedence is \`/skills\` → \`/.agents/skills\` → \`~/.agents/skills\` → \`~/.openclaw/skills\` → bundled → \`skills.load.extraDirs\`, so managed overrides still win over bundled skills without touching git. If you need the skill installed globally but only visible to some agents, keep the shared copy in \`~/.openclaw/skills\` and control visibility with \`agents.defaults.skills\` and \`agents.list\[\].skills\`. Only upstream-worthy edits should live in the repo and go out as PRs.

 Yes. Add extra directories via \`skills.load.extraDirs\` in \`~/.openclaw/openclaw.json\` (lowest precedence). Default precedence is \`/skills\` → \`/.agents/skills\` → \`~/.agents/skills\` → \`~/.openclaw/skills\` → bundled → \`skills.load.extraDirs\`. \`clawhub\` installs into \`./skills\` by default, which OpenClaw treats as \`/skills\` on the next session. If the skill should only be visible to certain agents, pair that with \`agents.defaults.skills\` or \`agents.list\[\].skills\`.

 Today the supported patterns are:

 \\* \*\*Cron jobs\*\*: isolated jobs can set a \`model\` override per job.
 \\* \*\*Sub-agents\*\*: route tasks to separate agents with different default models.
 \\* \*\*On-demand switch\*\*: use \`/model\` to switch the current session model at any time.

 See \[Cron jobs\](/automation/cron-jobs), \[Multi-Agent Routing\](/concepts/multi-agent), and \[Slash commands\](/tools/slash-commands).

 Use \*\*sub-agents\*\* for long or parallel tasks. Sub-agents run in their own session,
 return a summary, and keep your main chat responsive.

 Ask your bot to "spawn a sub-agent for this task" or use \`/subagents\`.
 Use \`/status\` in chat to see what the Gateway is doing right now (and whether it is busy).

 Token tip: long tasks and sub-agents both consume tokens. If cost is a concern, set a
 cheaper model for sub-agents via \`agents.defaults.subagents.model\`.

 Docs: \[Sub-agents\](/tools/subagents), \[Background Tasks\](/automation/tasks).

 Use thread bindings. You can bind a Discord thread to a subagent or session target so follow-up messages in that thread stay on that bound session.

 Basic flow:

 \\* Spawn with \`sessions\_spawn\` using \`thread: true\` (and optionally \`mode: "session"\` for persistent follow-up).
 \\* Or manually bind with \`/focus \`.
 \\* Use \`/agents\` to inspect binding state.
 \\* Use \`/session idle \` and \`/session max-age \` to control auto-unfocus.
 \\* Use \`/unfocus\` to detach the thread.

 Required config:

 \\* Global defaults: \`session.threadBindings.enabled\`, \`session.threadBindings.idleHours\`, \`session.threadBindings.maxAgeHours\`.
 \\* Discord overrides: \`channels.discord.threadBindings.enabled\`, \`channels.discord.threadBindings.idleHours\`, \`channels.discord.threadBindings.maxAgeHours\`.
 \\* Auto-bind on spawn: \`channels.discord.threadBindings.spawnSessions\` defaults to \`true\`; set it to \`false\` to disable thread-bound session spawns.

 Docs: \[Sub-agents\](/tools/subagents), \[Discord\](/channels/discord), \[Configuration Reference\](/gateway/configuration-reference), \[Slash commands\](/tools/slash-commands).

 Check the resolved requester route first:

 \\* Completion-mode subagent delivery prefers any bound thread or conversation route when one exists.
 \\* If the completion origin only carries a channel, OpenClaw falls back to the requester session's stored route (\`lastChannel\` / \`lastTo\` / \`lastAccountId\`) so direct delivery can still succeed.
 \\* If neither a bound route nor a usable stored route exists, direct delivery can fail and the result falls back to queued session delivery instead of posting immediately to chat.
 \\* Invalid or stale targets can still force queue fallback or final delivery failure.
 \\* If the child's last visible assistant reply is the exact silent token \`NO\_REPLY\` / \`no\_reply\`, or exactly \`ANNOUNCE\_SKIP\`, OpenClaw intentionally suppresses the announce instead of posting stale earlier progress.
 \\* If the child timed out after only tool calls, the announce can collapse that into a short partial-progress summary instead of replaying raw tool output.

 Debug:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw tasks show
 \`\`\`

 Docs: \[Sub-agents\](/tools/subagents), \[Background Tasks\](/automation/tasks), \[Session Tools\](/concepts/session-tool).

 Cron runs inside the Gateway process. If the Gateway is not running continuously,
 scheduled jobs will not run.

 Checklist:

 \\* Confirm cron is enabled (\`cron.enabled\`) and \`OPENCLAW\_SKIP\_CRON\` is not set.
 \\* Check the Gateway is running 24/7 (no sleep/restarts).
 \\* Verify timezone settings for the job (\`--tz\` vs host timezone).

 Debug:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron run
 openclaw cron runs --id  --limit 50
 \`\`\`

 Docs: \[Cron jobs\](/automation/cron-jobs), \[Automation & Tasks\](/automation).

 Check the delivery mode first:

 \\* \`--no-deliver\` / \`delivery.mode: "none"\` means no runner fallback send is expected.
 \\* Missing or invalid announce target (\`channel\` / \`to\`) means the runner skipped outbound delivery.
 \\* Channel auth failures (\`unauthorized\`, \`Forbidden\`) mean the runner tried to deliver but credentials blocked it.
 \\* A silent isolated result (\`NO\_REPLY\` / \`no\_reply\` only) is treated as intentionally non-deliverable, so the runner also suppresses queued fallback delivery.

 For isolated cron jobs, the agent can still send directly with the \`message\`
 tool when a chat route is available. \`--announce\` only controls the runner
 fallback path for final text that the agent did not already send.

 Debug:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron runs --id  --limit 50
 openclaw tasks show
 \`\`\`

 Docs: \[Cron jobs\](/automation/cron-jobs), \[Background Tasks\](/automation/tasks).

 That is usually the live model-switch path, not duplicate scheduling.

 Isolated cron can persist a runtime model handoff and retry when the active
 run throws \`LiveSessionModelSwitchError\`. The retry keeps the switched
 provider/model, and if the switch carried a new auth profile override, cron
 persists that too before retrying.

 Related selection rules:

 \\* Gmail hook model override wins first when applicable.
 \\* Then per-job \`model\`.
 \\* Then any stored cron-session model override.
 \\* Then the normal agent/default model selection.

 The retry loop is bounded. After the initial attempt plus 2 switch retries,
 cron aborts instead of looping forever.

 Debug:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw cron runs --id  --limit 50
 openclaw tasks show
 \`\`\`

 Docs: \[Cron jobs\](/automation/cron-jobs), \[cron CLI\](/cli/cron).

 Use native \`openclaw skills\` commands or drop skills into your workspace. The macOS Skills UI isn't available on Linux.
 Browse skills at \[https://clawhub.ai\](https://clawhub.ai).

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw skills search "calendar"
 openclaw skills search --limit 20
 openclaw skills install
 openclaw skills install  --version
 openclaw skills install  --force
 openclaw skills update --all
 openclaw skills list --eligible
 openclaw skills check
 \`\`\`

 Native \`openclaw skills install\` writes into the active workspace \`skills/\`
 directory. Install the separate \`clawhub\` CLI only if you want to publish or
 sync your own skills. For shared installs across agents, put the skill under
 \`~/.openclaw/skills\` and use \`agents.defaults.skills\` or
 \`agents.list\[\].skills\` if you want to narrow which agents can see it.

 Yes. Use the Gateway scheduler:

 \\* \*\*Cron jobs\*\* for scheduled or recurring tasks (persist across restarts).
 \\* \*\*Heartbeat\*\* for "main session" periodic checks.
 \\* \*\*Isolated jobs\*\* for autonomous agents that post summaries or deliver to chats.

 Docs: \[Cron jobs\](/automation/cron-jobs), \[Automation & Tasks\](/automation),
 \[Heartbeat\](/gateway/heartbeat).

 Not directly. macOS skills are gated by \`metadata.openclaw.os\` plus required binaries, and skills only appear in the system prompt when they are eligible on the \*\*Gateway host\*\*. On Linux, \`darwin\`-only skills (like \`apple-notes\`, \`apple-reminders\`, \`things-mac\`) will not load unless you override the gating.

 You have three supported patterns:

 \*\*Option A - run the Gateway on a Mac (simplest).\*\*
 Run the Gateway where the macOS binaries exist, then connect from Linux in \[remote mode\](#gateway-ports-already-running-and-remote-mode) or over Tailscale. The skills load normally because the Gateway host is macOS.

 \*\*Option B - use a macOS node (no SSH).\*\*
 Run the Gateway on Linux, pair a macOS node (menubar app), and set \*\*Node Run Commands\*\* to "Always Ask" or "Always Allow" on the Mac. OpenClaw can treat macOS-only skills as eligible when the required binaries exist on the node. The agent runs those skills via the \`nodes\` tool. If you choose "Always Ask", approving "Always Allow" in the prompt adds that command to the allowlist.

 \*\*Option C - proxy macOS binaries over SSH (advanced).\*\*
 Keep the Gateway on Linux, but make the required CLI binaries resolve to SSH wrappers that run on a Mac. Then override the skill to allow Linux so it stays eligible.

 1\. Create an SSH wrapper for the binary (example: \`memo\` for Apple Notes):

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 #!/usr/bin/env bash
 set -euo pipefail
 exec ssh -T user@mac-host /opt/homebrew/bin/memo "$@"
 \`\`\`

 2\. Put the wrapper on \`PATH\` on the Linux host (for example \`~/bin/memo\`).

 3\. Override the skill metadata (workspace or \`~/.openclaw/skills\`) to allow Linux:

 \`\`\`markdown theme={"theme":{"light":"min-light","dark":"min-dark"}}
 ---
 name: apple-notes
 description: Manage Apple Notes via the memo CLI on macOS.
 metadata: { "openclaw": { "os": \["darwin", "linux"\], "requires": { "bins": \["memo"\] } } }
 ---
 \`\`\`

 4\. Start a new session so the skills snapshot refreshes.

 Not built-in today.

 Options:

 \\* \*\*Custom skill / plugin:\*\* best for reliable API access (Notion/HeyGen both have APIs).
 \\* \*\*Browser automation:\*\* works without code but is slower and more fragile.

 If you want to keep context per client (agency workflows), a simple pattern is:

 \\* One Notion page per client (context + preferences + active work).
 \\* Ask the agent to fetch that page at the start of a session.

 If you want a native integration, open a feature request or build a skill
 targeting those APIs.

 Install skills:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw skills install
 openclaw skills update --all
 \`\`\`

 Native installs land in the active workspace \`skills/\` directory. For shared skills across agents, place them in \`~/.openclaw/skills//SKILL.md\`. If only some agents should see a shared install, configure \`agents.defaults.skills\` or \`agents.list\[\].skills\`. Some skills expect binaries installed via Homebrew; on Linux that means Linuxbrew (see the Homebrew Linux FAQ entry above). See \[Skills\](/tools/skills), \[Skills config\](/tools/skills-config), and \[ClawHub\](/tools/clawhub).

 Use the built-in \`user\` browser profile, which attaches through Chrome DevTools MCP:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw browser --browser-profile user tabs
 openclaw browser --browser-profile user snapshot
 \`\`\`

 If you want a custom name, create an explicit MCP profile:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw browser create-profile --name chrome-live --driver existing-session
 openclaw browser --browser-profile chrome-live tabs
 \`\`\`

 This path can use the local host browser or a connected browser node. If the Gateway runs elsewhere, either run a node host on the browser machine or use remote CDP instead.

 Current limits on \`existing-session\` / \`user\`:

 \\* actions are ref-driven, not CSS-selector driven
 \\* uploads require \`ref\` / \`inputRef\` and currently support one file at a time
 \\* \`responsebody\`, PDF export, download interception, and batch actions still need a managed browser or raw CDP profile

\## Sandboxing and memory

 Yes. See \[Sandboxing\](/gateway/sandboxing). For Docker-specific setup (full gateway in Docker or sandbox images), see \[Docker\](/install/docker).

 The default image is security-first and runs as the \`node\` user, so it does not
 include system packages, Homebrew, or bundled browsers. For a fuller setup:

 \\* Persist \`/home/node\` with \`OPENCLAW\_HOME\_VOLUME\` so caches survive.
 \\* Bake system deps into the image with \`OPENCLAW\_DOCKER\_APT\_PACKAGES\`.
 \\* Install Playwright browsers via the bundled CLI:
 \`node /app/node\_modules/playwright-core/cli.js install chromium\`
 \\* Set \`PLAYWRIGHT\_BROWSERS\_PATH\` and ensure the path is persisted.

 Docs: \[Docker\](/install/docker), \[Browser\](/tools/browser).

 Yes - if your private traffic is \*\*DMs\*\* and your public traffic is \*\*groups\*\*.

 Use \`agents.defaults.sandbox.mode: "non-main"\` so group/channel sessions (non-main keys) run in the configured sandbox backend, while the main DM session stays on-host. Docker is the default backend if you do not choose one. Then restrict what tools are available in sandboxed sessions via \`tools.sandbox.tools\`.

 Setup walkthrough + example config: \[Groups: personal DMs + public groups\](/channels/groups#pattern-personal-dms-public-groups-single-agent)

 Key config reference: \[Gateway configuration\](/gateway/config-agents#agentsdefaultssandbox)

 Set \`agents.defaults.sandbox.docker.binds\` to \`\["host:path:mode"\]\` (e.g., \`"/home/user/src:/src:ro"\`). Global + per-agent binds merge; per-agent binds are ignored when \`scope: "shared"\`. Use \`:ro\` for anything sensitive and remember binds bypass the sandbox filesystem walls.

 OpenClaw validates bind sources against both the normalized path and the canonical path resolved through the deepest existing ancestor. That means symlink-parent escapes still fail closed even when the last path segment does not exist yet, and allowed-root checks still apply after symlink resolution.

 See \[Sandboxing\](/gateway/sandboxing#custom-bind-mounts) and \[Sandbox vs Tool Policy vs Elevated\](/gateway/sandbox-vs-tool-policy-vs-elevated#bind-mounts-security-quick-check) for examples and safety notes.

 OpenClaw memory is just Markdown files in the agent workspace:

 \\* Daily notes in \`memory/YYYY-MM-DD.md\`
 \\* Curated long-term notes in \`MEMORY.md\` (main/private sessions only)

 OpenClaw also runs a \*\*silent pre-compaction memory flush\*\* to remind the model
 to write durable notes before auto-compaction. This only runs when the workspace
 is writable (read-only sandboxes skip it). See \[Memory\](/concepts/memory).

 Ask the bot to \*\*write the fact to memory\*\*. Long-term notes belong in \`MEMORY.md\`,
 short-term context goes into \`memory/YYYY-MM-DD.md\`.

 This is still an area we are improving. It helps to remind the model to store memories;
 it will know what to do. If it keeps forgetting, verify the Gateway is using the same
 workspace on every run.

 Docs: \[Memory\](/concepts/memory), \[Agent workspace\](/concepts/agent-workspace).

 Memory files live on disk and persist until you delete them. The limit is your
 storage, not the model. The \*\*session context\*\* is still limited by the model
 context window, so long conversations can compact or truncate. That is why
 memory search exists - it pulls only the relevant parts back into context.

 Docs: \[Memory\](/concepts/memory), \[Context\](/concepts/context).

 Only if you use \*\*OpenAI embeddings\*\*. Codex OAuth covers chat/completions and
 does \*\*not\*\* grant embeddings access, so \*\*signing in with Codex (OAuth or the
 Codex CLI login)\*\* does not help for semantic memory search. OpenAI embeddings
 still need a real API key (\`OPENAI\_API\_KEY\` or \`models.providers.openai.apiKey\`).

 If you don't set a provider explicitly, OpenClaw auto-selects a provider when it
 can resolve an API key (auth profiles, \`models.providers.\*.apiKey\`, or env vars).
 It prefers OpenAI if an OpenAI key resolves, otherwise Gemini if a Gemini key
 resolves, then Voyage, then Mistral. If no remote key is available, memory
 search stays disabled until you configure it. If you have a local model path
 configured and present, OpenClaw
 prefers \`local\`. Ollama is supported when you explicitly set
 \`memorySearch.provider = "ollama"\`.

 If you'd rather stay local, set \`memorySearch.provider = "local"\` (and optionally
 \`memorySearch.fallback = "none"\`). If you want Gemini embeddings, set
 \`memorySearch.provider = "gemini"\` and provide \`GEMINI\_API\_KEY\` (or
 \`memorySearch.remote.apiKey\`). We support \*\*OpenAI, Gemini, Voyage, Mistral, Ollama, or local\*\* embedding
 models - see \[Memory\](/concepts/memory) for the setup details.

\## Where things live on disk

 No - \*\*OpenClaw's state is local\*\*, but \*\*external services still see what you send them\*\*.

 \\* \*\*Local by default:\*\* sessions, memory files, config, and workspace live on the Gateway host
 (\`~/.openclaw\` + your workspace directory).
 \\* \*\*Remote by necessity:\*\* messages you send to model providers (Anthropic/OpenAI/etc.) go to
 their APIs, and chat platforms (WhatsApp/Telegram/Slack/etc.) store message data on their
 servers.
 \\* \*\*You control the footprint:\*\* using local models keeps prompts on your machine, but channel
 traffic still goes through the channel's servers.

 Related: \[Agent workspace\](/concepts/agent-workspace), \[Memory\](/concepts/memory).

 Everything lives under \`$OPENCLAW\_STATE\_DIR\` (default: \`~/.openclaw\`):

 \| Path \| Purpose \|
 \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
 \| \`$OPENCLAW\_STATE\_DIR/openclaw.json\` \| Main config (JSON5) \|
 \| \`$OPENCLAW\_STATE\_DIR/credentials/oauth.json\` \| Legacy OAuth import (copied into auth profiles on first use) \|
 \| \`$OPENCLAW\_STATE\_DIR/agents//agent/auth-profiles.json\` \| Auth profiles (OAuth, API keys, and optional \`keyRef\`/\`tokenRef\`) \|
 \| \`$OPENCLAW\_STATE\_DIR/secrets.json\` \| Optional file-backed secret payload for \`file\` SecretRef providers \|
 \| \`$OPENCLAW\_STATE\_DIR/agents//agent/auth.json\` \| Legacy compatibility file (static \`api\_key\` entries scrubbed) \|
 \| \`$OPENCLAW\_STATE\_DIR/credentials/\` \| Provider state (e.g. \`whatsapp//creds.json\`) \|
 \| \`$OPENCLAW\_STATE\_DIR/agents/\` \| Per-agent state (agentDir + sessions) \|
 \| \`$OPENCLAW\_STATE\_DIR/agents//sessions/\` \| Conversation history & state (per agent) \|
 \| \`$OPENCLAW\_STATE\_DIR/agents//sessions/sessions.json\` \| Session metadata (per agent) \|

 Legacy single-agent path: \`~/.openclaw/agent/\*\` (migrated by \`openclaw doctor\`).

 Your \*\*workspace\*\* (AGENTS.md, memory files, skills, etc.) is separate and configured via \`agents.defaults.workspace\` (default: \`~/.openclaw/workspace\`).

 These files live in the \*\*agent workspace\*\*, not \`~/.openclaw\`.

 \\* \*\*Workspace (per agent)\*\*: \`AGENTS.md\`, \`SOUL.md\`, \`IDENTITY.md\`, \`USER.md\`,
 \`MEMORY.md\`, \`memory/YYYY-MM-DD.md\`, optional \`HEARTBEAT.md\`.
 Lowercase root \`memory.md\` is legacy repair input only; \`openclaw doctor --fix\`
 can merge it into \`MEMORY.md\` when both files exist.
 \\* \*\*State dir (\`~/.openclaw\`)\*\*: config, channel/provider state, auth profiles, sessions, logs,
 and shared skills (\`~/.openclaw/skills\`).

 Default workspace is \`~/.openclaw/workspace\`, configurable via:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 agents: { defaults: { workspace: "~/.openclaw/workspace" } },
 }
 \`\`\`

 If the bot "forgets" after a restart, confirm the Gateway is using the same
 workspace on every launch (and remember: remote mode uses the \*\*gateway host's\*\*
 workspace, not your local laptop).

 Tip: if you want a durable behavior or preference, ask the bot to \*\*write it into
 AGENTS.md or MEMORY.md\*\* rather than relying on chat history.

 See \[Agent workspace\](/concepts/agent-workspace) and \[Memory\](/concepts/memory).

 Put your \*\*agent workspace\*\* in a \*\*private\*\* git repo and back it up somewhere
 private (for example GitHub private). This captures memory + AGENTS/SOUL/USER
 files, and lets you restore the assistant's "mind" later.

 Do \*\*not\*\* commit anything under \`~/.openclaw\` (credentials, sessions, tokens, or encrypted secrets payloads).
 If you need a full restore, back up both the workspace and the state directory
 separately (see the migration question above).

 Docs: \[Agent workspace\](/concepts/agent-workspace).

 See the dedicated guide: \[Uninstall\](/install/uninstall).

 Yes. The workspace is the \*\*default cwd\*\* and memory anchor, not a hard sandbox.
 Relative paths resolve inside the workspace, but absolute paths can access other
 host locations unless sandboxing is enabled. If you need isolation, use
 \[\`agents.defaults.sandbox\`\](/gateway/sandboxing) or per-agent sandbox settings. If you
 want a repo to be the default working directory, point that agent's
 \`workspace\` to the repo root. The OpenClaw repo is just source code; keep the
 workspace separate unless you intentionally want the agent to work inside it.

 Example (repo as default cwd):

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 agents: {
 defaults: {
 workspace: "~/Projects/my-repo",
 },
 },
 }
 \`\`\`

 Session state is owned by the \*\*gateway host\*\*. If you're in remote mode, the session store you care about is on the remote machine, not your local laptop. See \[Session management\](/concepts/session).

\## Config basics

 OpenClaw reads an optional \*\*JSON5\*\* config from \`$OPENCLAW\_CONFIG\_PATH\` (default: \`~/.openclaw/openclaw.json\`):

 \`\`\`
 $OPENCLAW\_CONFIG\_PATH
 \`\`\`

 If the file is missing, it uses safe-ish defaults (including a default workspace of \`~/.openclaw/workspace\`).

 Non-loopback binds \*\*require a valid gateway auth path\*\*. In practice that means:

 \\* shared-secret auth: token or password
 \\* \`gateway.auth.mode: "trusted-proxy"\` behind a correctly configured identity-aware reverse proxy

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 gateway: {
 bind: "lan",
 auth: {
 mode: "token",
 token: "replace-me",
 },
 },
 }
 \`\`\`

 Notes:

 \\* \`gateway.remote.token\` / \`.password\` do \*\*not\*\* enable local gateway auth by themselves.
 \\* Local call paths can use \`gateway.remote.\*\` as fallback only when \`gateway.auth.\*\` is unset.
 \\* For password auth, set \`gateway.auth.mode: "password"\` plus \`gateway.auth.password\` (or \`OPENCLAW\_GATEWAY\_PASSWORD\`) instead.
 \\* If \`gateway.auth.token\` / \`gateway.auth.password\` is explicitly configured via SecretRef and unresolved, resolution fails closed (no remote fallback masking).
 \\* Shared-secret Control UI setups authenticate via \`connect.params.auth.token\` or \`connect.params.auth.password\` (stored in app/UI settings). Identity-bearing modes such as Tailscale Serve or \`trusted-proxy\` use request headers instead. Avoid putting shared secrets in URLs.
 \\* With \`gateway.auth.mode: "trusted-proxy"\`, same-host loopback reverse proxies require explicit \`gateway.auth.trustedProxy.allowLoopback = true\` and a loopback entry in \`gateway.trustedProxies\`.

 OpenClaw enforces gateway auth by default, including loopback. In the normal default path that means token auth: if no explicit auth path is configured, gateway startup resolves to token mode and auto-generates one, saving it to \`gateway.auth.token\`, so \*\*local WS clients must authenticate\*\*. This blocks other local processes from calling the Gateway.

 If you prefer a different auth path, you can explicitly choose password mode (or, for identity-aware reverse proxies, \`trusted-proxy\`). If you \*\*really\*\* want open loopback, set \`gateway.auth.mode: "none"\` explicitly in your config. Doctor can generate a token for you any time: \`openclaw doctor --generate-gateway-token\`.

 The Gateway watches the config and supports hot-reload:

 \\* \`gateway.reload.mode: "hybrid"\` (default): hot-apply safe changes, restart for critical ones
 \\* \`hot\`, \`restart\`, \`off\` are also supported

 Set \`cli.banner.taglineMode\` in config:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 cli: {
 banner: {
 taglineMode: "off", // random \| default \| off
 },
 },
 }
 \`\`\`

 \\* \`off\`: hides tagline text but keeps the banner title/version line.
 \\* \`default\`: uses \`All your chats, one OpenClaw.\` every time.
 \\* \`random\`: rotating funny/seasonal taglines (default behavior).
 \\* If you want no banner at all, set env \`OPENCLAW\_HIDE\_BANNER=1\`.

 \`web\_fetch\` works without an API key. \`web\_search\` depends on your selected
 provider:

 \\* API-backed providers such as Brave, Exa, Firecrawl, Gemini, Grok, Kimi, MiniMax Search, Perplexity, and Tavily require their normal API key setup.
 \\* Ollama Web Search is key-free, but it uses your configured Ollama host and requires \`ollama signin\`.
 \\* DuckDuckGo is key-free, but it is an unofficial HTML-based integration.
 \\* SearXNG is key-free/self-hosted; configure \`SEARXNG\_BASE\_URL\` or \`plugins.entries.searxng.config.webSearch.baseUrl\`.

 \*\*Recommended:\*\* run \`openclaw configure --section web\` and choose a provider.
 Environment alternatives:

 \\* Brave: \`BRAVE\_API\_KEY\`
 \\* Exa: \`EXA\_API\_KEY\`
 \\* Firecrawl: \`FIRECRAWL\_API\_KEY\`
 \\* Gemini: \`GEMINI\_API\_KEY\`
 \\* Grok: \`XAI\_API\_KEY\`
 \\* Kimi: \`KIMI\_API\_KEY\` or \`MOONSHOT\_API\_KEY\`
 \\* MiniMax Search: \`MINIMAX\_CODE\_PLAN\_KEY\`, \`MINIMAX\_CODING\_API\_KEY\`, or \`MINIMAX\_API\_KEY\`
 \\* Perplexity: \`PERPLEXITY\_API\_KEY\` or \`OPENROUTER\_API\_KEY\`
 \\* SearXNG: \`SEARXNG\_BASE\_URL\`
 \\* Tavily: \`TAVILY\_API\_KEY\`

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 plugins: {
 entries: {
 brave: {
 config: {
 webSearch: {
 apiKey: "BRAVE\_API\_KEY\_HERE",
 },
 },
 },
 },
 },
 tools: {
 web: {
 search: {
 enabled: true,
 provider: "brave",
 maxResults: 5,
 },
 fetch: {
 enabled: true,
 provider: "firecrawl", // optional; omit for auto-detect
 },
 },
 },
 }
 \`\`\`

 Provider-specific web-search config now lives under \`plugins.entries..config.webSearch.\*\`.
 Legacy \`tools.web.search.\*\` provider paths still load temporarily for compatibility, but they should not be used for new configs.
 Firecrawl web-fetch fallback config lives under \`plugins.entries.firecrawl.config.webFetch.\*\`.

 Notes:

 \\* If you use allowlists, add \`web\_search\`/\`web\_fetch\`/\`x\_search\` or \`group:web\`.
 \\* \`web\_fetch\` is enabled by default (unless explicitly disabled).
 \\* If \`tools.web.fetch.provider\` is omitted, OpenClaw auto-detects the first ready fetch fallback provider from available credentials. Today the bundled provider is Firecrawl.
 \\* Daemons read env vars from \`~/.openclaw/.env\` (or the service environment).

 Docs: \[Web tools\](/tools/web).

 \`config.apply\` replaces the \*\*entire config\*\*. If you send a partial object, everything
 else is removed.

 Current OpenClaw protects many accidental clobbers:

 \\* OpenClaw-owned config writes validate the full post-change config before writing.
 \\* Invalid or destructive OpenClaw-owned writes are rejected and saved as \`openclaw.json.rejected.\*\`.
 \\* If a direct edit breaks startup or hot reload, the Gateway restores the last-known-good config and saves the rejected file as \`openclaw.json.clobbered.\*\`.
 \\* The main agent receives a boot warning after recovery so it does not blindly write the bad config again.

 Recover:

 \\* Check \`openclaw logs --follow\` for \`Config auto-restored from last-known-good\`, \`Config write rejected:\`, or \`config reload restored last-known-good config\`.
 \\* Inspect the newest \`openclaw.json.clobbered.\*\` or \`openclaw.json.rejected.\*\` beside the active config.
 \\* Keep the active restored config if it works, then copy only the intended keys back with \`openclaw config set\` or \`config.patch\`.
 \\* Run \`openclaw config validate\` and \`openclaw doctor\`.
 \\* If you have no last-known-good or rejected payload, restore from backup, or re-run \`openclaw doctor\` and reconfigure channels/models.
 \\* If this was unexpected, file a bug and include your last known config or any backup.
 \\* A local coding agent can often reconstruct a working config from logs or history.

 Avoid it:

 \\* Use \`openclaw config set\` for small changes.
 \\* Use \`openclaw configure\` for interactive edits.
 \\* Use \`config.schema.lookup\` first when you are not sure about an exact path or field shape; it returns a shallow schema node plus immediate child summaries for drill-down.
 \\* Use \`config.patch\` for partial RPC edits; keep \`config.apply\` for full-config replacement only.
 \\* If you are using the owner-only \`gateway\` tool from an agent run, it will still reject writes to \`tools.exec.ask\` / \`tools.exec.security\` (including legacy \`tools.bash.\*\` aliases that normalize to the same protected exec paths).

 Docs: \[Config\](/cli/config), \[Configure\](/cli/configure), \[Gateway troubleshooting\](/gateway/troubleshooting#gateway-restored-last-known-good-config), \[Doctor\](/gateway/doctor).

 The common pattern is \*\*one Gateway\*\* (e.g. Raspberry Pi) plus \*\*nodes\*\* and \*\*agents\*\*:

 \\* \*\*Gateway (central):\*\* owns channels (Signal/WhatsApp), routing, and sessions.
 \\* \*\*Nodes (devices):\*\* Macs/iOS/Android connect as peripherals and expose local tools (\`system.run\`, \`canvas\`, \`camera\`).
 \\* \*\*Agents (workers):\*\* separate brains/workspaces for special roles (e.g. "Hetzner ops", "Personal data").
 \\* \*\*Sub-agents:\*\* spawn background work from a main agent when you want parallelism.
 \\* \*\*TUI:\*\* connect to the Gateway and switch agents/sessions.

 Docs: \[Nodes\](/nodes), \[Remote access\](/gateway/remote), \[Multi-Agent Routing\](/concepts/multi-agent), \[Sub-agents\](/tools/subagents), \[TUI\](/web/tui).

 Yes. It's a config option:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 browser: { headless: true },
 agents: {
 defaults: {
 sandbox: { browser: { headless: true } },
 },
 },
 }
 \`\`\`

 Default is \`false\` (headful). Headless is more likely to trigger anti-bot checks on some sites. See \[Browser\](/tools/browser).

 Headless uses the \*\*same Chromium engine\*\* and works for most automation (forms, clicks, scraping, logins). The main differences:

 \\* No visible browser window (use screenshots if you need visuals).
 \\* Some sites are stricter about automation in headless mode (CAPTCHAs, anti-bot).
 For example, X/Twitter often blocks headless sessions.

 Set \`browser.executablePath\` to your Brave binary (or any Chromium-based browser) and restart the Gateway.
 See the full config examples in \[Browser\](/tools/browser#use-brave-or-another-chromium-based-browser).

\## Remote gateways and nodes

 Telegram messages are handled by the \*\*gateway\*\*. The gateway runs the agent and
 only then calls nodes over the \*\*Gateway WebSocket\*\* when a node tool is needed:

 Telegram → Gateway → Agent → \`node.\*\` → Node → Gateway → Telegram

 Nodes don't see inbound provider traffic; they only receive node RPC calls.

 Short answer: \*\*pair your computer as a node\*\*. The Gateway runs elsewhere, but it can
 call \`node.\*\` tools (screen, camera, system) on your local machine over the Gateway WebSocket.

 Typical setup:

 1\. Run the Gateway on the always-on host (VPS/home server).
 2\. Put the Gateway host + your computer on the same tailnet.
 3\. Ensure the Gateway WS is reachable (tailnet bind or SSH tunnel).
 4\. Open the macOS app locally and connect in \*\*Remote over SSH\*\* mode (or direct tailnet)
 so it can register as a node.
 5\. Approve the node on the Gateway:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw devices list
 openclaw devices approve
 \`\`\`

 No separate TCP bridge is required; nodes connect over the Gateway WebSocket.

 Security reminder: pairing a macOS node allows \`system.run\` on that machine. Only
 pair devices you trust, and review \[Security\](/gateway/security).

 Docs: \[Nodes\](/nodes), \[Gateway protocol\](/gateway/protocol), \[macOS remote mode\](/platforms/mac/remote), \[Security\](/gateway/security).

 Check the basics:

 \\* Gateway is running: \`openclaw gateway status\`
 \\* Gateway health: \`openclaw status\`
 \\* Channel health: \`openclaw channels status\`

 Then verify auth and routing:

 \\* If you use Tailscale Serve, make sure \`gateway.auth.allowTailscale\` is set correctly.
 \\* If you connect via SSH tunnel, confirm the local tunnel is up and points at the right port.
 \\* Confirm your allowlists (DM or group) include your account.

 Docs: \[Tailscale\](/gateway/tailscale), \[Remote access\](/gateway/remote), \[Channels\](/channels).

 Yes. There is no built-in "bot-to-bot" bridge, but you can wire it up in a few
 reliable ways:

 \*\*Simplest:\*\* use a normal chat channel both bots can access (Telegram/Slack/WhatsApp).
 Have Bot A send a message to Bot B, then let Bot B reply as usual.

 \*\*CLI bridge (generic):\*\* run a script that calls the other Gateway with
 \`openclaw agent --message ... --deliver\`, targeting a chat where the other bot
 listens. If one bot is on a remote VPS, point your CLI at that remote Gateway
 via SSH/Tailscale (see \[Remote access\](/gateway/remote)).

 Example pattern (run from a machine that can reach the target Gateway):

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw agent --message "Hello from local bot" --deliver --channel telegram --reply-to
 \`\`\`

 Tip: add a guardrail so the two bots do not loop endlessly (mention-only, channel
 allowlists, or a "do not reply to bot messages" rule).

 Docs: \[Remote access\](/gateway/remote), \[Agent CLI\](/cli/agent), \[Agent send\](/tools/agent-send).

 No. One Gateway can host multiple agents, each with its own workspace, model defaults,
 and routing. That is the normal setup and it is much cheaper and simpler than running
 one VPS per agent.

 Use separate VPSes only when you need hard isolation (security boundaries) or very
 different configs that you do not want to share. Otherwise, keep one Gateway and
 use multiple agents or sub-agents.

 Yes - nodes are the first-class way to reach your laptop from a remote Gateway, and they
 unlock more than shell access. The Gateway runs on macOS/Linux (Windows via WSL2) and is
 lightweight (a small VPS or Raspberry Pi-class box is fine; 4 GB RAM is plenty), so a common
 setup is an always-on host plus your laptop as a node.

 \\* \*\*No inbound SSH required.\*\* Nodes connect out to the Gateway WebSocket and use device pairing.
 \\* \*\*Safer execution controls.\*\* \`system.run\` is gated by node allowlists/approvals on that laptop.
 \\* \*\*More device tools.\*\* Nodes expose \`canvas\`, \`camera\`, and \`screen\` in addition to \`system.run\`.
 \\* \*\*Local browser automation.\*\* Keep the Gateway on a VPS, but run Chrome locally through a node host on the laptop, or attach to local Chrome on the host via Chrome MCP.

 SSH is fine for ad-hoc shell access, but nodes are simpler for ongoing agent workflows and
 device automation.

 Docs: \[Nodes\](/nodes), \[Nodes CLI\](/cli/nodes), \[Browser\](/tools/browser).

 No. Only \*\*one gateway\*\* should run per host unless you intentionally run isolated profiles (see \[Multiple gateways\](/gateway/multiple-gateways)). Nodes are peripherals that connect
 to the gateway (iOS/Android nodes, or macOS "node mode" in the menubar app). For headless node
 hosts and CLI control, see \[Node host CLI\](/cli/node).

 A full restart is required for \`gateway\`, \`discovery\`, and \`canvasHost\` changes.

 Yes.

 \\* \`config.schema.lookup\`: inspect one config subtree with its shallow schema node, matched UI hint, and immediate child summaries before writing
 \\* \`config.get\`: fetch the current snapshot + hash
 \\* \`config.patch\`: safe partial update (preferred for most RPC edits); hot-reloads when possible and restarts when required
 \\* \`config.apply\`: validate + replace the full config; hot-reloads when possible and restarts when required
 \\* The owner-only \`gateway\` runtime tool still refuses to rewrite \`tools.exec.ask\` / \`tools.exec.security\`; legacy \`tools.bash.\*\` aliases normalize to the same protected exec paths

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 agents: { defaults: { workspace: "~/.openclaw/workspace" } },
 channels: { whatsapp: { allowFrom: \["+15555550123"\] } },
 }
 \`\`\`

 This sets your workspace and restricts who can trigger the bot.

 Minimal steps:

 1\. \*\*Install + login on the VPS\*\*

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 curl -fsSL https://tailscale.com/install.sh \| sh
 sudo tailscale up
 \`\`\`

 2\. \*\*Install + login on your Mac\*\*
 \\* Use the Tailscale app and sign in to the same tailnet.

 3\. \*\*Enable MagicDNS (recommended)\*\*
 \\* In the Tailscale admin console, enable MagicDNS so the VPS has a stable name.

 4\. \*\*Use the tailnet hostname\*\*
 \\* SSH: \`ssh user@your-vps.tailnet-xxxx.ts.net\`
 \\* Gateway WS: \`ws://your-vps.tailnet-xxxx.ts.net:18789\`

 If you want the Control UI without SSH, use Tailscale Serve on the VPS:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway --tailscale serve
 \`\`\`

 This keeps the gateway bound to loopback and exposes HTTPS via Tailscale. See \[Tailscale\](/gateway/tailscale).

 Serve exposes the \*\*Gateway Control UI + WS\*\*. Nodes connect over the same Gateway WS endpoint.

 Recommended setup:

 1\. \*\*Make sure the VPS + Mac are on the same tailnet\*\*.
 2\. \*\*Use the macOS app in Remote mode\*\* (SSH target can be the tailnet hostname).
 The app will tunnel the Gateway port and connect as a node.
 3\. \*\*Approve the node\*\* on the gateway:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw devices list
 openclaw devices approve
 \`\`\`

 Docs: \[Gateway protocol\](/gateway/protocol), \[Discovery\](/gateway/discovery), \[macOS remote mode\](/platforms/mac/remote).

 If you only need \*\*local tools\*\* (screen/camera/exec) on the second laptop, add it as a
 \*\*node\*\*. That keeps a single Gateway and avoids duplicated config. Local node tools are
 currently macOS-only, but we plan to extend them to other OSes.

 Install a second Gateway only when you need \*\*hard isolation\*\* or two fully separate bots.

 Docs: \[Nodes\](/nodes), \[Nodes CLI\](/cli/nodes), \[Multiple gateways\](/gateway/multiple-gateways).

\## Env vars and .env loading

 OpenClaw reads env vars from the parent process (shell, launchd/systemd, CI, etc.) and additionally loads:

 \\* \`.env\` from the current working directory
 \\* a global fallback \`.env\` from \`~/.openclaw/.env\` (aka \`$OPENCLAW\_STATE\_DIR/.env\`)

 Neither \`.env\` file overrides existing env vars.

 You can also define inline env vars in config (applied only if missing from the process env):

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 env: {
 OPENROUTER\_API\_KEY: "sk-or-...",
 vars: { GROQ\_API\_KEY: "gsk-..." },
 },
 }
 \`\`\`

 See \[/environment\](/help/environment) for full precedence and sources.

 Two common fixes:

 1\. Put the missing keys in \`~/.openclaw/.env\` so they're picked up even when the service doesn't inherit your shell env.
 2\. Enable shell import (opt-in convenience):

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 env: {
 shellEnv: {
 enabled: true,
 timeoutMs: 15000,
 },
 },
 }
 \`\`\`

 This runs your login shell and imports only missing expected keys (never overrides). Env var equivalents:
 \`OPENCLAW\_LOAD\_SHELL\_ENV=1\`, \`OPENCLAW\_SHELL\_ENV\_TIMEOUT\_MS=15000\`.

 \`openclaw models status\` reports whether \*\*shell env import\*\* is enabled. "Shell env: off"
 does \*\*not\*\* mean your env vars are missing - it just means OpenClaw won't load
 your login shell automatically.

 If the Gateway runs as a service (launchd/systemd), it won't inherit your shell
 environment. Fix by doing one of these:

 1\. Put the token in \`~/.openclaw/.env\`:

 \`\`\`
 COPILOT\_GITHUB\_TOKEN=...
 \`\`\`

 2\. Or enable shell import (\`env.shellEnv.enabled: true\`).

 3\. Or add it to your config \`env\` block (applies only if missing).

 Then restart the gateway and recheck:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models status
 \`\`\`

 Copilot tokens are read from \`COPILOT\_GITHUB\_TOKEN\` (also \`GH\_TOKEN\` / \`GITHUB\_TOKEN\`).
 See \[/concepts/model-providers\](/concepts/model-providers) and \[/environment\](/help/environment).

\## Sessions and multiple chats

 Send \`/new\` or \`/reset\` as a standalone message. See \[Session management\](/concepts/session).

 Sessions can expire after \`session.idleMinutes\`, but this is \*\*disabled by default\*\* (default \*\*0\*\*).
 Set it to a positive value to enable idle expiry. When enabled, the \*\*next\*\*
 message after the idle period starts a fresh session id for that chat key.
 This does not delete transcripts - it just starts a new session.

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 session: {
 idleMinutes: 240,
 },
 }
 \`\`\`

 Yes, via \*\*multi-agent routing\*\* and \*\*sub-agents\*\*. You can create one coordinator
 agent and several worker agents with their own workspaces and models.

 That said, this is best seen as a \*\*fun experiment\*\*. It is token heavy and often
 less efficient than using one bot with separate sessions. The typical model we
 envision is one bot you talk to, with different sessions for parallel work. That
 bot can also spawn sub-agents when needed.

 Docs: \[Multi-agent routing\](/concepts/multi-agent), \[Sub-agents\](/tools/subagents), \[Agents CLI\](/cli/agents).

 Session context is limited by the model window. Long chats, large tool outputs, or many
 files can trigger compaction or truncation.

 What helps:

 \\* Ask the bot to summarize the current state and write it to a file.
 \\* Use \`/compact\` before long tasks, and \`/new\` when switching topics.
 \\* Keep important context in the workspace and ask the bot to read it back.
 \\* Use sub-agents for long or parallel work so the main chat stays smaller.
 \\* Pick a model with a larger context window if this happens often.

 Use the reset command:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw reset
 \`\`\`

 Non-interactive full reset:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw reset --scope full --yes --non-interactive
 \`\`\`

 Then re-run setup:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --install-daemon
 \`\`\`

 Notes:

 \\* Onboarding also offers \*\*Reset\*\* if it sees an existing config. See \[Onboarding (CLI)\](/start/wizard).
 \\* If you used profiles (\`--profile\` / \`OPENCLAW\_PROFILE\`), reset each state dir (defaults are \`~/.openclaw-\`).
 \\* Dev reset: \`openclaw gateway --dev --reset\` (dev-only; wipes dev config + credentials + sessions + workspace).

 Use one of these:

 \\* \*\*Compact\*\* (keeps the conversation but summarizes older turns):

 \`\`\`
 /compact
 \`\`\`

 or \`/compact \` to guide the summary.

 \\* \*\*Reset\*\* (fresh session ID for the same chat key):

 \`\`\`
 /new
 /reset
 \`\`\`

 If it keeps happening:

 \\* Enable or tune \*\*session pruning\*\* (\`agents.defaults.contextPruning\`) to trim old tool output.
 \\* Use a model with a larger context window.

 Docs: \[Compaction\](/concepts/compaction), \[Session pruning\](/concepts/session-pruning), \[Session management\](/concepts/session).

 This is a provider validation error: the model emitted a \`tool\_use\` block without the required
 \`input\`. It usually means the session history is stale or corrupted (often after long threads
 or a tool/schema change).

 Fix: start a fresh session with \`/new\` (standalone message).

 Heartbeats run every \*\*30m\*\* by default (\*\*1h\*\* when using OAuth auth). Tune or disable them:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 agents: {
 defaults: {
 heartbeat: {
 every: "2h", // or "0m" to disable
 },
 },
 },
 }
 \`\`\`

 If \`HEARTBEAT.md\` exists but is effectively empty (only blank lines and markdown
 headers like \`# Heading\`), OpenClaw skips the heartbeat run to save API calls.
 If the file is missing, the heartbeat still runs and the model decides what to do.

 Per-agent overrides use \`agents.list\[\].heartbeat\`. Docs: \[Heartbeat\](/gateway/heartbeat).

 No. OpenClaw runs on \*\*your own account\*\*, so if you're in the group, OpenClaw can see it.
 By default, group replies are blocked until you allow senders (\`groupPolicy: "allowlist"\`).

 If you want only \*\*you\*\* to be able to trigger group replies:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 channels: {
 whatsapp: {
 groupPolicy: "allowlist",
 groupAllowFrom: \["+15551234567"\],
 },
 },
 }
 \`\`\`

 Option 1 (fastest): tail logs and send a test message in the group:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw logs --follow --json
 \`\`\`

 Look for \`chatId\` (or \`from\`) ending in \`@g.us\`, like:
 \`1234567890-1234567890@g.us\`.

 Option 2 (if already configured/allowlisted): list groups from config:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw directory groups list --channel whatsapp
 \`\`\`

 Docs: \[WhatsApp\](/channels/whatsapp), \[Directory\](/cli/directory), \[Logs\](/cli/logs).

 Two common causes:

 \\* Mention gating is on (default). You must @mention the bot (or match \`mentionPatterns\`).
 \\* You configured \`channels.whatsapp.groups\` without \`"\*"\` and the group isn't allowlisted.

 See \[Groups\](/channels/groups) and \[Group messages\](/channels/group-messages).

 Direct chats collapse to the main session by default. Groups/channels have their own session keys, and Telegram topics / Discord threads are separate sessions. See \[Groups\](/channels/groups) and \[Group messages\](/channels/group-messages).

 No hard limits. Dozens (even hundreds) are fine, but watch for:

 \\* \*\*Disk growth:\*\* sessions + transcripts live under \`~/.openclaw/agents//sessions/\`.
 \\* \*\*Token cost:\*\* more agents means more concurrent model usage.
 \\* \*\*Ops overhead:\*\* per-agent auth profiles, workspaces, and channel routing.

 Tips:

 \\* Keep one \*\*active\*\* workspace per agent (\`agents.defaults.workspace\`).
 \\* Prune old sessions (delete JSONL or store entries) if disk grows.
 \\* Use \`openclaw doctor\` to spot stray workspaces and profile mismatches.

 Yes. Use \*\*Multi-Agent Routing\*\* to run multiple isolated agents and route inbound messages by
 channel/account/peer. Slack is supported as a channel and can be bound to specific agents.

 Browser access is powerful but not "do anything a human can" - anti-bot, CAPTCHAs, and MFA can
 still block automation. For the most reliable browser control, use local Chrome MCP on the host,
 or use CDP on the machine that actually runs the browser.

 Best-practice setup:

 \\* Always-on Gateway host (VPS/Mac mini).
 \\* One agent per role (bindings).
 \\* Slack channel(s) bound to those agents.
 \\* Local browser via Chrome MCP or a node when needed.

 Docs: \[Multi-Agent Routing\](/concepts/multi-agent), \[Slack\](/channels/slack),
 \[Browser\](/tools/browser), \[Nodes\](/nodes).

\## Models, failover, and auth profiles

Model Q\\&A — defaults, selection, aliases, switching, failover, auth profiles —
lives on the \[Models FAQ\](/help/faq-models).

\## Gateway: ports, "already running", and remote mode

 \`gateway.port\` controls the single multiplexed port for WebSocket + HTTP (Control UI, hooks, etc.).

 Precedence:

 \`\`\`
 --port > OPENCLAW\_GATEWAY\_PORT > gateway.port > default 18789
 \`\`\`

 Because "running" is the \*\*supervisor's\*\* view (launchd/systemd/schtasks). The connectivity probe is the CLI actually connecting to the gateway WebSocket.

 Use \`openclaw gateway status\` and trust these lines:

 \\* \`Probe target:\` (the URL the probe actually used)
 \\* \`Listening:\` (what's actually bound on the port)
 \\* \`Last gateway error:\` (common root cause when the process is alive but the port isn't listening)

 You're editing one config file while the service is running another (often a \`--profile\` / \`OPENCLAW\_STATE\_DIR\` mismatch).

 Fix:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway install --force
 \`\`\`

 Run that from the same \`--profile\` / environment you want the service to use.

 OpenClaw enforces a runtime lock by binding the WebSocket listener immediately on startup (default \`ws://127.0.0.1:18789\`). If the bind fails with \`EADDRINUSE\`, it throws \`GatewayLockError\` indicating another instance is already listening.

 Fix: stop the other instance, free the port, or run with \`openclaw gateway --port \`.

 Set \`gateway.mode: "remote"\` and point to a remote WebSocket URL, optionally with shared-secret remote credentials:

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 gateway: {
 mode: "remote",
 remote: {
 url: "ws://gateway.tailnet:18789",
 token: "your-token",
 password: "your-password",
 },
 },
 }
 \`\`\`

 Notes:

 \\* \`openclaw gateway\` only starts when \`gateway.mode\` is \`local\` (or you pass the override flag).
 \\* The macOS app watches the config file and switches modes live when these values change.
 \\* \`gateway.remote.token\` / \`.password\` are client-side remote credentials only; they do not enable local gateway auth by themselves.

 Your gateway auth path and the UI's auth method do not match.

 Facts (from code):

 \\* The Control UI keeps the token in \`sessionStorage\` for the current browser tab session and selected gateway URL, so same-tab refreshes keep working without restoring long-lived localStorage token persistence.
 \\* On \`AUTH\_TOKEN\_MISMATCH\`, trusted clients can attempt one bounded retry with a cached device token when the gateway returns retry hints (\`canRetryWithDeviceToken=true\`, \`recommendedNextStep=retry\_with\_device\_token\`).
 \\* That cached-token retry now reuses the cached approved scopes stored with the device token. Explicit \`deviceToken\` / explicit \`scopes\` callers still keep their requested scope set instead of inheriting cached scopes.
 \\* Outside that retry path, connect auth precedence is explicit shared token/password first, then explicit \`deviceToken\`, then stored device token, then bootstrap token.
 \\* Bootstrap token scope checks are role-prefixed. The built-in bootstrap operator allowlist only satisfies operator requests; node or other non-operator roles still need scopes under their own role prefix.

 Fix:

 \\* Fastest: \`openclaw dashboard\` (prints + copies the dashboard URL, tries to open; shows SSH hint if headless).
 \\* If you don't have a token yet: \`openclaw doctor --generate-gateway-token\`.
 \\* If remote, tunnel first: \`ssh -N -L 18789:127.0.0.1:18789 user@host\` then open \`http://127.0.0.1:18789/\`.
 \\* Shared-secret mode: set \`gateway.auth.token\` / \`OPENCLAW\_GATEWAY\_TOKEN\` or \`gateway.auth.password\` / \`OPENCLAW\_GATEWAY\_PASSWORD\`, then paste the matching secret in Control UI settings.
 \\* Tailscale Serve mode: make sure \`gateway.auth.allowTailscale\` is enabled and you are opening the Serve URL, not a raw loopback/tailnet URL that bypasses Tailscale identity headers.
 \\* Trusted-proxy mode: make sure you are coming through the configured identity-aware proxy, not a raw gateway URL. Same-host loopback proxies also need \`gateway.auth.trustedProxy.allowLoopback = true\`.
 \\* If mismatch persists after the one retry, rotate/re-approve the paired device token:
 \\* \`openclaw devices list\`
 \\* \`openclaw devices rotate --device  --role operator\`
 \\* If that rotate call says it was denied, check two things:
 \\* paired-device sessions can rotate only their \*\*own\*\* device unless they also have \`operator.admin\`
 \\* explicit \`--scope\` values cannot exceed the caller's current operator scopes
 \\* Still stuck? Run \`openclaw status --all\` and follow \[Troubleshooting\](/gateway/troubleshooting). See \[Dashboard\](/web/dashboard) for auth details.

 \`tailnet\` bind picks a Tailscale IP from your network interfaces (100.64.0.0/10). If the machine isn't on Tailscale (or the interface is down), there's nothing to bind to.

 Fix:

 \\* Start Tailscale on that host (so it has a 100.x address), or
 \\* Switch to \`gateway.bind: "loopback"\` / \`"lan"\`.

 Note: \`tailnet\` is explicit. \`auto\` prefers loopback; use \`gateway.bind: "tailnet"\` when you want a tailnet-only bind.

 Usually no - one Gateway can run multiple messaging channels and agents. Use multiple Gateways only when you need redundancy (ex: rescue bot) or hard isolation.

 Yes, but you must isolate:

 \\* \`OPENCLAW\_CONFIG\_PATH\` (per-instance config)
 \\* \`OPENCLAW\_STATE\_DIR\` (per-instance state)
 \\* \`agents.defaults.workspace\` (workspace isolation)
 \\* \`gateway.port\` (unique ports)

 Quick setup (recommended):

 \\* Use \`openclaw --profile  ...\` per instance (auto-creates \`~/.openclaw-\`).
 \\* Set a unique \`gateway.port\` in each profile config (or pass \`--port\` for manual runs).
 \\* Install a per-profile service: \`openclaw --profile  gateway install\`.

 Profiles also suffix service names (\`ai.openclaw.\`; legacy \`com.openclaw.\*\`, \`openclaw-gateway-.service\`, \`OpenClaw Gateway ()\`).
 Full guide: \[Multiple gateways\](/gateway/multiple-gateways).

 The Gateway is a \*\*WebSocket server\*\*, and it expects the very first message to
 be a \`connect\` frame. If it receives anything else, it closes the connection
 with \*\*code 1008\*\* (policy violation).

 Common causes:

 \\* You opened the \*\*HTTP\*\* URL in a browser (\`http://...\`) instead of a WS client.
 \\* You used the wrong port or path.
 \\* A proxy or tunnel stripped auth headers or sent a non-Gateway request.

 Quick fixes:

 1\. Use the WS URL: \`ws://:18789\` (or \`wss://...\` if HTTPS).
 2\. Don't open the WS port in a normal browser tab.
 3\. If auth is on, include the token/password in the \`connect\` frame.

 If you're using the CLI or TUI, the URL should look like:

 \`\`\`
 openclaw tui --url ws://:18789 --token
 \`\`\`

 Protocol details: \[Gateway protocol\](/gateway/protocol).

\## Logging and debugging

 File logs (structured):

 \`\`\`
 /tmp/openclaw/openclaw-YYYY-MM-DD.log
 \`\`\`

 You can set a stable path via \`logging.file\`. File log level is controlled by \`logging.level\`. Console verbosity is controlled by \`--verbose\` and \`logging.consoleLevel\`.

 Fastest log tail:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw logs --follow
 \`\`\`

 Service/supervisor logs (when the gateway runs via launchd/systemd):

 \\* macOS: \`$OPENCLAW\_STATE\_DIR/logs/gateway.log\` and \`gateway.err.log\` (default: \`~/.openclaw/logs/...\`; profiles use \`~/.openclaw-/logs/...\`)
 \\* Linux: \`journalctl --user -u openclaw-gateway\[-\].service -n 200 --no-pager\`
 \\* Windows: \`schtasks /Query /TN "OpenClaw Gateway ()" /V /FO LIST\`

 See \[Troubleshooting\](/gateway/troubleshooting) for more.

 Use the gateway helpers:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway status
 openclaw gateway restart
 \`\`\`

 If you run the gateway manually, \`openclaw gateway --force\` can reclaim the port. See \[Gateway\](/gateway).

 There are \*\*two Windows install modes\*\*:

 \*\*1) WSL2 (recommended):\*\* the Gateway runs inside Linux.

 Open PowerShell, enter WSL, then restart:

 \`\`\`powershell theme={"theme":{"light":"min-light","dark":"min-dark"}}
 wsl
 openclaw gateway status
 openclaw gateway restart
 \`\`\`

 If you never installed the service, start it in the foreground:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway run
 \`\`\`

 \*\*2) Native Windows (not recommended):\*\* the Gateway runs directly in Windows.

 Open PowerShell and run:

 \`\`\`powershell theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway status
 openclaw gateway restart
 \`\`\`

 If you run it manually (no service), use:

 \`\`\`powershell theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway run
 \`\`\`

 Docs: \[Windows (WSL2)\](/platforms/windows), \[Gateway service runbook\](/gateway).

 Start with a quick health sweep:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw status
 openclaw models status
 openclaw channels status
 openclaw logs --follow
 \`\`\`

 Common causes:

 \\* Model auth not loaded on the \*\*gateway host\*\* (check \`models status\`).
 \\* Channel pairing/allowlist blocking replies (check channel config + logs).
 \\* WebChat/Dashboard is open without the right token.

 If you are remote, confirm the tunnel/Tailscale connection is up and that the
 Gateway WebSocket is reachable.

 Docs: \[Channels\](/channels), \[Troubleshooting\](/gateway/troubleshooting), \[Remote access\](/gateway/remote).

 This usually means the UI lost the WebSocket connection. Check:

 1\. Is the Gateway running? \`openclaw gateway status\`
 2\. Is the Gateway healthy? \`openclaw status\`
 3\. Does the UI have the right token? \`openclaw dashboard\`
 4\. If remote, is the tunnel/Tailscale link up?

 Then tail logs:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw logs --follow
 \`\`\`

 Docs: \[Dashboard\](/web/dashboard), \[Remote access\](/gateway/remote), \[Troubleshooting\](/gateway/troubleshooting).

 Start with logs and channel status:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw channels status
 openclaw channels logs --channel telegram
 \`\`\`

 Then match the error:

 \\* \`BOT\_COMMANDS\_TOO\_MUCH\`: the Telegram menu has too many entries. OpenClaw already trims to the Telegram limit and retries with fewer commands, but some menu entries still need to be dropped. Reduce plugin/skill/custom commands, or disable \`channels.telegram.commands.native\` if you do not need the menu.
 \\* \`TypeError: fetch failed\`, \`Network request for 'setMyCommands' failed!\`, or similar network errors: if you are on a VPS or behind a proxy, confirm outbound HTTPS is allowed and DNS works for \`api.telegram.org\`.

 If the Gateway is remote, make sure you are looking at logs on the Gateway host.

 Docs: \[Telegram\](/channels/telegram), \[Channel troubleshooting\](/channels/troubleshooting).

 First confirm the Gateway is reachable and the agent can run:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw status
 openclaw models status
 openclaw logs --follow
 \`\`\`

 In the TUI, use \`/status\` to see the current state. If you expect replies in a chat
 channel, make sure delivery is enabled (\`/deliver on\`).

 Docs: \[TUI\](/web/tui), \[Slash commands\](/tools/slash-commands).

 If you installed the service:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway stop
 openclaw gateway start
 \`\`\`

 This stops/starts the \*\*supervised service\*\* (launchd on macOS, systemd on Linux).
 Use this when the Gateway runs in the background as a daemon.

 If you're running in the foreground, stop with Ctrl-C, then:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway run
 \`\`\`

 Docs: \[Gateway service runbook\](/gateway).

 \\* \`openclaw gateway restart\`: restarts the \*\*background service\*\* (launchd/systemd).
 \\* \`openclaw gateway\`: runs the gateway \*\*in the foreground\*\* for this terminal session.

 If you installed the service, use the gateway commands. Use \`openclaw gateway\` when
 you want a one-off, foreground run.

 Start the Gateway with \`--verbose\` to get more console detail. Then inspect the log file for channel auth, model routing, and RPC errors.

\## Media and attachments

 Outbound attachments from the agent must include a \`MEDIA:\` line (on its own line). See \[OpenClaw assistant setup\](/start/openclaw) and \[Agent send\](/tools/agent-send).

 CLI sending:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw message send --target +15555550123 --message "Here you go" --media /path/to/file.png
 \`\`\`

 Also check:

 \\* The target channel supports outbound media and isn't blocked by allowlists.
 \\* The file is within the provider's size limits (images are resized to max 2048px).
 \\* \`tools.fs.workspaceOnly=true\` keeps local-path sends limited to workspace, temp/media-store, and sandbox-validated files.
 \\* \`tools.fs.workspaceOnly=false\` lets \`MEDIA:\` send host-local files the agent can already read, but only for media plus safe document types (images, audio, video, PDF, and Office docs). Plain text and secret-like files are still blocked.

 See \[Images\](/nodes/images).

\## Security and access control

 Treat inbound DMs as untrusted input. Defaults are designed to reduce risk:

 \\* Default behavior on DM-capable channels is \*\*pairing\*\*:
 \\* Unknown senders receive a pairing code; the bot does not process their message.
 \\* Approve with: \`openclaw pairing approve --channel  \[--account \] ```
      * Pending requests are capped at **3 per channel**; check `openclaw pairing list --channel  [--account ]` if a code didn't arrive.
    * Opening DMs publicly requires explicit opt-in (`dmPolicy: "open"` and allowlist `"*"`).

    Run `openclaw doctor` to surface risky DM policies.
`````

    No. Prompt injection is about **untrusted content**, not just who can DM the bot.
    If your assistant reads external content (web search/fetch, browser pages, emails,
    docs, attachments, pasted logs), that content can include instructions that try
    to hijack the model. This can happen even if **you are the only sender**.

    The biggest risk is when tools are enabled: the model can be tricked into
    exfiltrating context or calling tools on your behalf. Reduce the blast radius by:

    * using a read-only or tool-disabled "reader" agent to summarize untrusted content
    * keeping `web_search` / `web_fetch` / `browser` off for tool-enabled agents
    * treating decoded file/document text as untrusted too: OpenResponses
      `input_file` and media-attachment extraction both wrap extracted text in
      explicit external-content boundary markers instead of passing raw file text
    * sandboxing and strict tool allowlists

    Details: [Security](/gateway/security).

    Yes, for most setups. Isolating the bot with separate accounts and phone numbers
    reduces the blast radius if something goes wrong. This also makes it easier to rotate
    credentials or revoke access without impacting your personal accounts.

    Start small. Give access only to the tools and accounts you actually need, and expand
    later if required.

    Docs: [Security](/gateway/security), [Pairing](/channels/pairing).

    We do **not** recommend full autonomy over your personal messages. The safest pattern is:

    * Keep DMs in **pairing mode** or a tight allowlist.
    * Use a **separate number or account** if you want it to message on your behalf.
    * Let it draft, then **approve before sending**.

    If you want to experiment, do it on a dedicated account and keep it isolated. See
    [Security](/gateway/security).

    Yes, **if** the agent is chat-only and the input is trusted. Smaller tiers are
    more susceptible to instruction hijacking, so avoid them for tool-enabled agents
    or when reading untrusted content. If you must use a smaller model, lock down
    tools and run inside a sandbox. See [Security](/gateway/security).

    Pairing codes are sent **only** when an unknown sender messages the bot and
    `dmPolicy: "pairing"` is enabled. `/start` by itself doesn't generate a code.

    Check pending requests:

    ```bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
    openclaw pairing list telegram
    ```

    If you want immediate access, allowlist your sender id or set `dmPolicy: "open"`
    for that account.

    No. Default WhatsApp DM policy is **pairing**. Unknown senders only get a pairing code and their message is **not processed**. OpenClaw only replies to chats it receives or to explicit sends you trigger.

    Approve pairing with:

    ```bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
    openclaw pairing approve whatsapp
    ```

    List pending requests:

    ```bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
    openclaw pairing list whatsapp
    ```

    Wizard phone number prompt: it's used to set your **allowlist/owner** so your own DMs are permitted. It's not used for auto-sending. If you run on your personal WhatsApp number, use that number and enable `channels.whatsapp.selfChatMode`.

``````

## Chat commands, aborting tasks, and "it will not stop"

    Most internal or tool messages only appear when **verbose**, **trace**, or **reasoning** is enabled
    for that session.

    Fix in the chat where you see it:

    ```
    /verbose off
    /trace off
    /reasoning off
    ```

    If it is still noisy, check the session settings in the Control UI and set verbose
    to **inherit**. Also confirm you are not using a bot profile with `verboseDefault` set
    to `on` in config.

    Docs: [Thinking and verbose](/tools/thinking), [Security](/gateway/security/index#reasoning-and-verbose-output-in-groups).

    Send any of these **as a standalone message** (no slash):

    ```
    stop
    stop action
    stop current action
    stop run
    stop current run
    stop agent
    stop the agent
    stop openclaw
    openclaw stop
    stop don't do anything
    stop do not do anything
    stop doing anything
    please stop
    stop please
    abort
    esc
    wait
    exit
    interrupt
    ```

    These are abort triggers (not slash commands).

    For background processes (from the exec tool), you can ask the agent to run:

    ```
    process action:kill sessionId:XXX
    ```

    Slash commands overview: see [Slash commands](/tools/slash-commands).

    Most commands must be sent as a **standalone** message that starts with `/`, but a few shortcuts (like `/status`) also work inline for allowlisted senders.

    OpenClaw blocks **cross-provider** messaging by default. If a tool call is bound
    to Telegram, it won't send to Discord unless you explicitly allow it.

    Enable cross-provider messaging for the agent:

    ```json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
    {
      tools: {
        message: {
          crossContext: {
            allowAcrossProviders: true,
            marker: { enabled: true, prefix: "[from {channel}] " },
          },
        },
      },
    }
    ```

    Restart the gateway after editing config.

    Queue mode controls how new messages interact with an in-flight run. Use `/queue` to change modes:

    * `steer` - queue all pending steering for the next model boundary in the current run
    * `queue` - legacy one-at-a-time steering
    * `followup` - run messages one at a time
    * `collect` - batch messages and reply once
    * `steer-backlog` - steer now, then process backlog
    * `interrupt` - abort current run and start fresh

    Default mode is `steer`. You can add options like `debounce:0.5s cap:25 drop:summarize` for followup modes. See [Command queue](/concepts/queue) and [Steering queue](/concepts/queue-steering).

## Miscellaneous

    In OpenClaw, credentials and model selection are separate. Setting `ANTHROPIC_API_KEY` (or storing an Anthropic API key in auth profiles) enables authentication, but the actual default model is whatever you configure in `agents.defaults.model.primary` (for example, `anthropic/claude-sonnet-4-6` or `anthropic/claude-opus-4-6`). If you see `No credentials found for profile "anthropic:default"`, it means the Gateway couldn't find Anthropic credentials in the expected `auth-profiles.json` for the agent that's running.

***

Still stuck? Ask in [Discord](https://discord.com/invite/clawd) or open a [GitHub discussion](https://github.com/openclaw/openclaw/discussions).

## Related

* [First-run FAQ](/help/faq-first-run) — install, onboard, auth, subscriptions, early failures
* [Models FAQ](/help/faq-models) — model selection, failover, auth profiles
* [Troubleshooting](/help/troubleshooting) — symptom-first triage
```

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
  - Runs gateway agent turns through the plugin-owned Codex app-server harness,
    verifies `/codex status` and `/codex models`, and by default exercises image,
    cron MCP, sub-agent, and Guardian probes. Disable the sub-agent probe with
    `OPENCLAW_LIVE_CODEX_HARNESS_SUBAGENT_PROBE=0` when isolating other Codex
    app-server failures. For a focused sub-agent check, disable the other probes:
    `OPENCLAW_LIVE_CODEX_HARNESS_IMAGE_PROBE=0 OPENCLAW_LIVE_CODEX_HARNESS_MCP_PROBE=0 OPENCLAW_LIVE_CODEX_HARNESS_GUARDIAN_PROBE=0 OPENCLAW_LIVE_CODEX_HARNESS_SUBAGENT_PROBE=1 pnpm test:docker:live-codex-harness`.
    This exits after the sub-agent probe unless
    `OPENCLAW_LIVE_CODEX_HARNESS_SUBAGENT_ONLY=0` is set.
- Crestodian rescue command smoke: `pnpm test:live:crestodian-rescue-channel`
  - Opt-in belt-and-suspenders check for the message-channel rescue command
    surface. It exercises `/crestodian status`, queues a persistent model
    change, replies `/crestodian yes`, and verifies the audit/config write path.
- Crestodian planner Docker smoke: `pnpm test:docker:crestodian-planner`
  - Runs Crestodian in a configless container with a fake Claude CLI on `PATH`
    and verifies the fuzzy planner fallback translates into an audited typed
    config write.
- Crestodian first-run Docker smoke: `pnpm test:docker:crestodian-first-run`
  - Starts from an empty OpenClaw state dir, routes bare `openclaw` to
    Crestodian, applies setup/model/agent/Discord plugin + SecretRef writes,
    validates config, and verifies audit entries. The same Ring 0 setup path is
    also covered in QA Lab by
    `pnpm openclaw qa suite --scenario crestodian-ring-zero-setup`.
- Moonshot/Kimi cost smoke: with `MOONSHOT_API_KEY` set, run
`openclaw models list --provider moonshot --json`, then run an isolated
`openclaw agent --local --session-id live-kimi-cost --message 'Reply exactly: KIMI_LIVE_OK' --thinking off --json`
against `moonshot/kimi-k2.6`. Verify the JSON reports Moonshot/K2.6 and the
assistant transcript stores normalized `usage.cost`.

When you only need one failing case, prefer narrowing live tests via the allowlist env vars described below.

## QA-specific runners

These commands sit beside the main test suites when you need QA-lab realism:CI runs QA Lab in dedicated workflows. Agentic parity is nested under
`QA-Lab - All Lanes` and release validation, not a standalone PR workflow.
Broad validation should use `Full Release Validation` with
`rerun_group=qa-parity` or the release-checks QA group. `QA-Lab - All Lanes`
runs nightly on `main` and from manual dispatch with the mock parity lane, live
Matrix lane, Convex-managed live Telegram lane, and Convex-managed live Discord
lane as parallel jobs. Scheduled QA and release checks pass Matrix
`--profile fast` explicitly, while the Matrix CLI and manual workflow input
default remain `all`; manual dispatch can shard `all` into `transport`,
`media`, `e2ee-smoke`, `e2ee-deep`, and `e2ee-cli` jobs. `OpenClaw Release Checks` runs parity plus the fast Matrix and Telegram lanes before release
approval, using `mock-openai/gpt-5.5` for release transport checks so they stay
deterministic and avoid normal provider-plugin startup. These live transport
gateways disable memory search; memory behavior stays covered by the QA parity
suites.Full release live media shards use
`ghcr.io/openclaw/openclaw-live-media-runner:ubuntu-24.04`, which already has
`ffmpeg` and `ffprobe`. Docker live model/backend shards use the shared
`ghcr.io/openclaw/openclaw-live-test:<sha>` image built once per selected
commit, then pull it with `OPENCLAW_SKIP_DOCKER_BUILD=1` instead of rebuilding
inside every shard.

- `pnpm openclaw qa suite`
  - Runs repo-backed QA scenarios directly on the host.
  - Runs multiple selected scenarios in parallel by default with isolated
    gateway workers. `qa-channel` defaults to concurrency 4 (bounded by the
    selected scenario count). Use `--concurrency <count>` to tune the worker
    count, or `--concurrency 1` for the older serial lane.
  - Exits non-zero when any scenario fails. Use `--allow-failures` when you
    want artifacts without a failing exit code.
  - Supports provider modes `live-frontier`, `mock-openai`, and `aimock`.
    `aimock` starts a local AIMock-backed provider server for experimental
    fixture and protocol-mock coverage without replacing the scenario-aware
    `mock-openai` lane.
- `pnpm test:gateway:cpu-scenarios`
  - Runs the gateway startup bench plus a small mock QA Lab scenario pack
    (`channel-chat-baseline`, `memory-failure-fallback`,
    `gateway-restart-inflight-run`) and writes a combined CPU observation
    summary under `.artifacts/gateway-cpu-scenarios/`.
  - Flags only sustained hot CPU observations by default (`--cpu-core-warn`
    plus `--hot-wall-warn-ms`), so short startup bursts are recorded as metrics
    without looking like the minutes-long gateway peg regression.
  - Uses built `dist` artifacts; run a build first when the checkout does not
    already have fresh runtime output.
- `pnpm openclaw qa suite --runner multipass`
  - Runs the same QA suite inside a disposable Multipass Linux VM.
  - Keeps the same scenario-selection behavior as `qa suite` on the host.
  - Reuses the same provider/model selection flags as `qa suite`.
  - Live runs forward the supported QA auth inputs that are practical for the guest:
    env-based provider keys, the QA live provider config path, and `CODEX_HOME`
    when present.
  - Output dirs must stay under the repo root so the guest can write back through
    the mounted workspace.
  - Writes the normal QA report + summary plus Multipass logs under
    `.artifacts/qa-e2e/...`.
- `pnpm qa:lab:up`
  - Starts the Docker-backed QA site for operator-style QA work.
- `pnpm test:docker:npm-onboard-channel-agent`
  - Builds an npm tarball from the current checkout, installs it globally in
    Docker, runs non-interactive OpenAI API-key onboarding, configures Telegram
    by default, verifies the packaged plugin runtime loads without startup
    dependency repair, runs doctor, and runs one local agent turn against a
    mocked OpenAI endpoint.
  - Use `OPENCLAW_NPM_ONBOARD_CHANNEL=discord` to run the same packaged-install
    lane with Discord.
- `pnpm test:docker:session-runtime-context`
  - Runs a deterministic built-app Docker smoke for embedded runtime context
    transcripts. It verifies hidden OpenClaw runtime context is persisted as a
    non-display custom message instead of leaking into the visible user turn,
    then seeds an affected broken session JSONL and verifies
    `openclaw doctor --fix` rewrites it to the active branch with a backup.
- `pnpm test:docker:npm-telegram-live`
  - Installs an OpenClaw package candidate in Docker, runs installed-package
    onboarding, configures Telegram through the installed CLI, then reuses the
    live Telegram QA lane with that installed package as the SUT Gateway.
  - Defaults to `OPENCLAW_NPM_TELEGRAM_PACKAGE_SPEC=openclaw@beta`; set
    `OPENCLAW_NPM_TELEGRAM_PACKAGE_TGZ=/path/to/openclaw-current.tgz` or
    `OPENCLAW_CURRENT_PACKAGE_TGZ` to test a resolved local tarball instead of
    installing from the registry.
  - Uses the same Telegram env credentials or Convex credential source as
    `pnpm openclaw qa telegram`. For CI/release automation, set
    `OPENCLAW_NPM_TELEGRAM_CREDENTIAL_SOURCE=convex` plus
    `OPENCLAW_QA_CONVEX_SITE_URL` and the role secret. If
    `OPENCLAW_QA_CONVEX_SITE_URL` and a Convex role secret are present in CI,
    the Docker wrapper selects Convex automatically.
  - `OPENCLAW_NPM_TELEGRAM_CREDENTIAL_ROLE=ci|maintainer` overrides the shared
    `OPENCLAW_QA_CREDENTIAL_ROLE` for this lane only.
  - GitHub Actions exposes this lane as the manual maintainer workflow
    `NPM Telegram Beta E2E`. It does not run on merge. The workflow uses the
    `qa-live-shared` environment and Convex CI credential leases.
- GitHub Actions also exposes `Package Acceptance` for side-run product proof
against one candidate package. It accepts a trusted ref, published npm spec,
HTTPS tarball URL plus SHA-256, or tarball artifact from another run, uploads
the normalized `openclaw-current.tgz` as `package-under-test`, then runs the
existing Docker E2E scheduler with smoke, package, product, full, or custom
lane profiles. Set `telegram_mode=mock-openai` or `live-frontier` to run the
Telegram QA workflow against the same `package-under-test` artifact.

  - Latest beta product proof:

```
gh workflow run package-acceptance.yml --ref main \
  -f source=npm \
  -f package_spec=openclaw@beta \
  -f suite_profile=product \
  -f telegram_mode=mock-openai
```

- Exact tarball URL proof requires a digest:

```
gh workflow run package-acceptance.yml --ref main \
  -f source=url \
  -f package_url=https://registry.npmjs.org/openclaw/-/openclaw-VERSION.tgz \
  -f package_sha256=<sha256> \
  -f suite_profile=package
```

- Artifact proof downloads a tarball artifact from another Actions run:

```
gh workflow run package-acceptance.yml --ref main \
  -f source=artifact \
  -f artifact_run_id=<run-id> \
  -f artifact_name=<artifact-name> \
  -f suite_profile=smoke
```

- `pnpm test:docker:plugins`  - Packs and installs the current OpenClaw build in Docker, starts the Gateway
      with OpenAI configured, then enables bundled channel/plugins via config
      edits.
  - Verifies setup discovery leaves unconfigured downloadable plugins absent,
    the first configured doctor repair installs each missing downloadable
    plugin explicitly, and a second restart does not run hidden dependency
    repair.
  - Also installs a known older npm baseline, enables Telegram before running
    `openclaw update --tag <candidate>`, and verifies the candidate’s
    post-update doctor cleans legacy plugin dependency debris without a
    harness-side postinstall repair.
- `pnpm test:parallels:npm-update`  - Runs the native packaged-install update smoke across Parallels guests. Each
      selected platform first installs the requested baseline package, then runs
      the installed `openclaw update` command in the same guest and verifies the
      installed version, update status, gateway readiness, and one local agent
      turn.
  - Use `--platform macos`, `--platform windows`, or `--platform linux` while
    iterating on one guest. Use `--json` for the summary artifact path and
    per-lane status.
  - The OpenAI lane uses `openai/gpt-5.5` for the live agent-turn proof by
    default. Pass `--model <provider/model>` or set
    `OPENCLAW_PARALLELS_OPENAI_MODEL` when deliberately validating another
    OpenAI model.
  - Wrap long local runs in a host timeout so Parallels transport stalls cannot
    consume the rest of the testing window:

    ```
    timeout --foreground 150m pnpm test:parallels:npm-update -- --json
    timeout --foreground 90m pnpm test:parallels:npm-update -- --platform windows --json
    ```

  - The script writes nested lane logs under `/tmp/openclaw-parallels-npm-update.*`.
    Inspect `windows-update.log`, `macos-update.log`, or `linux-update.log`
    before assuming the outer wrapper is hung.
  - Windows update can spend 10 to 15 minutes in post-update doctor and package
    update work on a cold guest; that is still healthy when the nested npm
    debug log is advancing.
  - Do not run this aggregate wrapper in parallel with individual Parallels
    macOS, Windows, or Linux smoke lanes. They share VM state and can collide on
    snapshot restore, package serving, or guest gateway state.
  - The post-update proof runs the normal bundled plugin surface because
    capability facades such as speech, image generation, and media
    understanding are loaded through bundled runtime APIs even when the agent
    turn itself only checks a simple text response.
- `pnpm openclaw qa aimock`  - Starts only the local AIMock provider server for direct protocol smoke
      testing.
- `pnpm openclaw qa matrix`  - Runs the Matrix live QA lane against a disposable Docker-backed Tuwunel homeserver. Source-checkout only — packaged installs do not ship `qa-lab`.
  - Full CLI, profile/scenario catalog, env vars, and artifact layout: [Matrix QA](https://docs.openclaw.ai/concepts/qa-matrix).
- `pnpm openclaw qa telegram`  - Runs the Telegram live QA lane against a real private group using the driver and SUT bot tokens from env.
  - Requires `OPENCLAW_QA_TELEGRAM_GROUP_ID`, `OPENCLAW_QA_TELEGRAM_DRIVER_BOT_TOKEN`, and `OPENCLAW_QA_TELEGRAM_SUT_BOT_TOKEN`. The group id must be the numeric Telegram chat id.
  - Supports `--credential-source convex` for shared pooled credentials. Use env mode by default, or set `OPENCLAW_QA_CREDENTIAL_SOURCE=convex` to opt into pooled leases.
  - Exits non-zero when any scenario fails. Use `--allow-failures` when you
    want artifacts without a failing exit code.
  - Requires two distinct bots in the same private group, with the SUT bot exposing a Telegram username.
  - For stable bot-to-bot observation, enable Bot-to-Bot Communication Mode in `@BotFather` for both bots and ensure the driver bot can observe group bot traffic.
  - Writes a Telegram QA report, summary, and observed-messages artifact under `.artifacts/qa-e2e/...`. Replying scenarios include RTT from driver send request to observed SUT reply.

Live transport lanes share one standard contract so new transports do not drift; the per-lane coverage matrix lives in [QA overview → Live transport coverage](https://docs.openclaw.ai/concepts/qa-e2e-automation#live-transport-coverage). `qa-channel` is the broad synthetic suite and is not part of that matrix.

### Shared Telegram credentials via Convex (v1)

When `--credential-source convex` (or `OPENCLAW_QA_CREDENTIAL_SOURCE=convex`) is enabled for
`openclaw qa telegram`, QA lab acquires an exclusive lease from a Convex-backed pool, heartbeats
that lease while the lane is running, and releases the lease on shutdown.Reference Convex project scaffold:

- `qa/convex-credential-broker/`

Required env vars:

- `OPENCLAW_QA_CONVEX_SITE_URL` (for example `https://your-deployment.convex.site`)
- One secret for the selected role:
  - `OPENCLAW_QA_CONVEX_SECRET_MAINTAINER` for `maintainer`
  - `OPENCLAW_QA_CONVEX_SECRET_CI` for `ci`
- Credential role selection:
  - CLI: `--credential-role maintainer|ci`
  - Env default: `OPENCLAW_QA_CREDENTIAL_ROLE` (defaults to `ci` in CI, `maintainer` otherwise)

Optional env vars:

- `OPENCLAW_QA_CREDENTIAL_LEASE_TTL_MS` (default `1200000`)
- `OPENCLAW_QA_CREDENTIAL_HEARTBEAT_INTERVAL_MS` (default `30000`)
- `OPENCLAW_QA_CREDENTIAL_ACQUIRE_TIMEOUT_MS` (default `90000`)
- `OPENCLAW_QA_CREDENTIAL_HTTP_TIMEOUT_MS` (default `15000`)
- `OPENCLAW_QA_CONVEX_ENDPOINT_PREFIX` (default `/qa-credentials/v1`)
- `OPENCLAW_QA_CREDENTIAL_OWNER_ID` (optional trace id)
- `OPENCLAW_QA_ALLOW_INSECURE_HTTP=1` allows loopback `http://` Convex URLs for local-only development.

`OPENCLAW_QA_CONVEX_SITE_URL` should use `https://` in normal operation.Maintainer admin commands (pool add/remove/list) require
`OPENCLAW_QA_CONVEX_SECRET_MAINTAINER` specifically.CLI helpers for maintainers:

```
pnpm openclaw qa credentials doctor
pnpm openclaw qa credentials add --kind telegram --payload-file qa/telegram-credential.json
pnpm openclaw qa credentials list --kind telegram
pnpm openclaw qa credentials remove --credential-id <credential-id>
```

Use `doctor` before live runs to check the Convex site URL, broker secrets,
endpoint prefix, HTTP timeout, and admin/list reachability without printing
secret values. Use `--json` for machine-readable output in scripts and CI
utilities.Default endpoint contract (`OPENCLAW_QA_CONVEX_SITE_URL` \+ `/qa-credentials/v1`):

- `POST /acquire`
  - Request: `{ kind, ownerId, actorRole, leaseTtlMs, heartbeatIntervalMs }`
  - Success: `{ status: "ok", credentialId, leaseToken, payload, leaseTtlMs?, heartbeatIntervalMs? }`
  - Exhausted/retryable: `{ status: "error", code: "POOL_EXHAUSTED" | "NO_CREDENTIAL_AVAILABLE", ... }`
- `POST /heartbeat`
  - Request: `{ kind, ownerId, actorRole, credentialId, leaseToken, leaseTtlMs }`
  - Success: `{ status: "ok" }` (or empty `2xx`)
- `POST /release`
  - Request: `{ kind, ownerId, actorRole, credentialId, leaseToken }`
  - Success: `{ status: "ok" }` (or empty `2xx`)
- `POST /admin/add`(maintainer secret only)

  - Request: `{ kind, actorId, payload, note?, status? }`
  - Success: `{ status: "ok", credential }`
- `POST /admin/remove`(maintainer secret only)

  - Request: `{ credentialId, actorId }`
  - Success: `{ status: "ok", changed, credential }`
  - Active lease guard: `{ status: "error", code: "LEASE_ACTIVE", ... }`
- `POST /admin/list`(maintainer secret only)

  - Request: `{ kind?, status?, includePayload?, limit? }`
  - Success: `{ status: "ok", credentials, count }`

Payload shape for Telegram kind:

- `{ groupId: string, driverToken: string, sutToken: string }`
- `groupId` must be a numeric Telegram chat id string.
- `admin/add` validates this shape for `kind: "telegram"` and rejects malformed payloads.

### Adding a channel to QA

The architecture and scenario-helper names for new channel adapters live in [QA overview → Adding a channel](https://docs.openclaw.ai/concepts/qa-e2e-automation#adding-a-channel). The minimum bar: implement the transport runner on the shared `qa-lab` host seam, declare `qaRunners` in the plugin manifest, mount as `openclaw qa <runner>`, and author scenarios under `qa/scenarios/`.

## Test suites (what runs where)

Think of the suites as “increasing realism” (and increasing flakiness/cost):

### Unit / integration (default)

- Command: `pnpm test`
- Config: untargeted runs use the `vitest.full-*.config.ts` shard set and may expand multi-project shards into per-project configs for parallel scheduling
- Files: core/unit inventories under `src/**/*.test.ts`, `packages/**/*.test.ts`, and `test/**/*.test.ts`; UI unit tests run in the dedicated `unit-ui` shard
- Scope:
  - Pure unit tests
  - In-process integration tests (gateway auth, routing, tooling, parsing, config)
  - Deterministic regressions for known bugs
- Expectations:
  - Runs in CI
  - No real keys required
  - Should be fast and stable
  - Resolver and public-surface loader tests must prove broad `api.js` and
    `runtime-api.js` fallback behavior with generated tiny plugin fixtures, not
    real bundled plugin source APIs. Real plugin API loads belong in
    plugin-owned contract/integration suites.

Projects, shards, and scoped lanes

- Untargeted `pnpm test` runs twelve smaller shard configs (`core-unit-fast`, `core-unit-src`, `core-unit-security`, `core-unit-ui`, `core-unit-support`, `core-support-boundary`, `core-contracts`, `core-bundled`, `core-runtime`, `agentic`, `auto-reply`, `extensions`) instead of one giant native root-project process. This cuts peak RSS on loaded machines and avoids auto-reply/extension work starving unrelated suites.
- `pnpm test --watch` still uses the native root `vitest.config.ts` project graph, because a multi-shard watch loop is not practical.
- `pnpm test`, `pnpm test:watch`, and `pnpm test:perf:imports` route explicit file/directory targets through scoped lanes first, so `pnpm test extensions/discord/src/monitor/message-handler.preflight.test.ts` avoids paying the full root project startup tax.
- `pnpm test:changed` expands changed git paths into cheap scoped lanes by default: direct test edits, sibling `*.test.ts` files, explicit source mappings, and local import-graph dependents. Config/setup/package edits do not broad-run tests unless you explicitly use `OPENCLAW_TEST_CHANGED_BROAD=1 pnpm test:changed`.
- `pnpm check:changed` is the normal smart local check gate for narrow work. It classifies the diff into core, core tests, extensions, extension tests, apps, docs, release metadata, live Docker tooling, and tooling, then runs the matching typecheck, lint, and guard commands. It does not run Vitest tests; call `pnpm test:changed` or explicit `pnpm test <target>` for test proof. Release metadata-only version bumps run targeted version/config/root-dependency checks, with a guard that rejects package changes outside the top-level version field.
- Live Docker ACP harness edits run focused checks: shell syntax for the live Docker auth scripts and a live Docker scheduler dry-run. `package.json` changes are included only when the diff is limited to `scripts["test:docker:live-*"]`; dependency, export, version, and other package-surface edits still use the broader guards.
- Import-light unit tests from agents, commands, plugins, auto-reply helpers, `plugin-sdk`, and similar pure utility areas route through the `unit-fast` lane, which skips `test/setup-openclaw-runtime.ts`; stateful/runtime-heavy files stay on the existing lanes.
- Selected `plugin-sdk` and `commands` helper source files also map changed-mode runs to explicit sibling tests in those light lanes, so helper edits avoid rerunning the full heavy suite for that directory.
- `auto-reply` has dedicated buckets for top-level core helpers, top-level `reply.*` integration tests, and the `src/auto-reply/reply/**` subtree. CI further splits the reply subtree into agent-runner, dispatch, and commands/state-routing shards so one import-heavy bucket does not own the full Node tail.
- Normal PR/main CI intentionally skips the extension batch sweep and release-only `agentic-plugins` shard. Full Release Validation dispatches the separate `Plugin Prerelease` child workflow for those plugin/extension-heavy suites on release candidates.

Embedded runner coverage

- When you change message-tool discovery inputs or compaction runtime
context, keep both levels of coverage.
- Add focused helper regressions for pure routing and normalization
boundaries.
- Keep the embedded runner integration suites healthy:
`src/agents/pi-embedded-runner/compact.hooks.test.ts`,
`src/agents/pi-embedded-runner/run.overflow-compaction.test.ts`, and
`src/agents/pi-embedded-runner/run.overflow-compaction.loop.test.ts`.
- Those suites verify that scoped ids and compaction behavior still flow
through the real `run.ts` / `compact.ts` paths; helper-only tests are
not a sufficient substitute for those integration paths.

Vitest pool and isolation defaults

- Base Vitest config defaults to `threads`.
- The shared Vitest config fixes `isolate: false` and uses the
non-isolated runner across the root projects, e2e, and live configs.
- The root UI lane keeps its `jsdom` setup and optimizer, but runs on the
shared non-isolated runner too.
- Each `pnpm test` shard inherits the same `threads` \+ `isolate: false`
defaults from the shared Vitest config.
- `scripts/run-vitest.mjs` adds `--no-maglev` for Vitest child Node
processes by default to reduce V8 compile churn during big local runs.
Set `OPENCLAW_VITEST_ENABLE_MAGLEV=1` to compare against stock V8
behavior.

Fast local iteration

- `pnpm changed:lanes` shows which architectural lanes a diff triggers.
- The pre-commit hook is formatting-only. It restages formatted files and
does not run lint, typecheck, or tests.
- Run `pnpm check:changed` explicitly before handoff or push when you
need the smart local check gate.
- `pnpm test:changed` routes through cheap scoped lanes by default. Use
`OPENCLAW_TEST_CHANGED_BROAD=1 pnpm test:changed` only when the agent
decides a harness, config, package, or contract edit really needs broader
Vitest coverage.
- `pnpm test:max` and `pnpm test:changed:max` keep the same routing
behavior, just with a higher worker cap.
- Local worker auto-scaling is intentionally conservative and backs off
when the host load average is already high, so multiple concurrent
Vitest runs do less damage by default.
- The base Vitest config marks the projects/config files as
`forceRerunTriggers` so changed-mode reruns stay correct when test
wiring changes.
- The config keeps `OPENCLAW_VITEST_FS_MODULE_CACHE` enabled on supported
hosts; set `OPENCLAW_VITEST_FS_MODULE_CACHE_PATH=/abs/path` if you want
one explicit cache location for direct profiling.

Perf debugging

- `pnpm test:perf:imports` enables Vitest import-duration reporting plus
import-breakdown output.
- `pnpm test:perf:imports:changed` scopes the same profiling view to
files changed since `origin/main`.
- Shard timing data is written to `.artifacts/vitest-shard-timings.json`.
Whole-config runs use the config path as the key; include-pattern CI
shards append the shard name so filtered shards can be tracked
separately.
- When one hot test still spends most of its time in startup imports,
keep heavy dependencies behind a narrow local `*.runtime.ts` seam and
mock that seam directly instead of deep-importing runtime helpers just
to pass them through `vi.mock(...)`.
- `pnpm test:perf:changed:bench -- --ref <git-ref>` compares routed
`test:changed` against the native root-project path for that committed
diff and prints wall time plus macOS max RSS.
- `pnpm test:perf:changed:bench -- --worktree` benchmarks the current
dirty tree by routing the changed file list through
`scripts/test-projects.mjs` and the root Vitest config.
- `pnpm test:perf:profile:main` writes a main-thread CPU profile for
Vitest/Vite startup and transform overhead.
- `pnpm test:perf:profile:runner` writes runner CPU+heap profiles for the
unit suite with file parallelism disabled.

### Stability (gateway)

- Command: `pnpm test:stability:gateway`
- Config: `vitest.gateway.config.ts`, forced to one worker
- Scope:
  - Starts a real loopback Gateway with diagnostics enabled by default
  - Drives synthetic gateway message, memory, and large-payload churn through the diagnostic event path
  - Queries `diagnostics.stability` over the Gateway WS RPC
  - Covers diagnostic stability bundle persistence helpers
  - Asserts the recorder remains bounded, synthetic RSS samples stay under the pressure budget, and per-session queue depths drain back to zero
- Expectations:
  - CI-safe and keyless
  - Narrow lane for stability-regression follow-up, not a substitute for the full Gateway suite

### E2E (gateway smoke)

- Command: `pnpm test:e2e`
- Config: `vitest.e2e.config.ts`
- Files: `src/**/*.e2e.test.ts`, `test/**/*.e2e.test.ts`, and bundled-plugin E2E tests under `extensions/`
- Runtime defaults:
  - Uses Vitest `threads` with `isolate: false`, matching the rest of the repo.
  - Uses adaptive workers (CI: up to 2, local: 1 by default).
  - Runs in silent mode by default to reduce console I/O overhead.
- Useful overrides:
  - `OPENCLAW_E2E_WORKERS=<n>` to force worker count (capped at 16).
  - `OPENCLAW_E2E_VERBOSE=1` to re-enable verbose console output.
- Scope:
  - Multi-instance gateway end-to-end behavior
  - WebSocket/HTTP surfaces, node pairing, and heavier networking
- Expectations:
  - Runs in CI (when enabled in the pipeline)
  - No real keys required
  - More moving parts than unit tests (can be slower)

### E2E: OpenShell backend smoke

- Command: `pnpm test:e2e:openshell`
- File: `extensions/openshell/src/backend.e2e.test.ts`
- Scope:
  - Starts an isolated OpenShell gateway on the host via Docker
  - Creates a sandbox from a temporary local Dockerfile
  - Exercises OpenClaw’s OpenShell backend over real `sandbox ssh-config` \+ SSH exec
  - Verifies remote-canonical filesystem behavior through the sandbox fs bridge
- Expectations:
  - Opt-in only; not part of the default `pnpm test:e2e` run
  - Requires a local `openshell` CLI plus a working Docker daemon
  - Uses isolated `HOME` / `XDG_CONFIG_HOME`, then destroys the test gateway and sandbox
- Useful overrides:
  - `OPENCLAW_E2E_OPENSHELL=1` to enable the test when running the broader e2e suite manually
  - `OPENCLAW_E2E_OPENSHELL_COMMAND=/path/to/openshell` to point at a non-default CLI binary or wrapper script

### Live (real providers + real models)

- Command: `pnpm test:live`
- Config: `vitest.live.config.ts`
- Files: `src/**/*.live.test.ts`, `test/**/*.live.test.ts`, and bundled-plugin live tests under `extensions/`
- Default: **enabled** by `pnpm test:live` (sets `OPENCLAW_LIVE_TEST=1`)
- Scope:
  - “Does this provider/model actually work _today_ with real creds?”
  - Catch provider format changes, tool-calling quirks, auth issues, and rate limit behavior
- Expectations:
  - Not CI-stable by design (real networks, real provider policies, quotas, outages)
  - Costs money / uses rate limits
  - Prefer running narrowed subsets instead of “everything”
- Live runs source `~/.profile` to pick up missing API keys.
- By default, live runs still isolate `HOME` and copy config/auth material into a temp test home so unit fixtures cannot mutate your real `~/.openclaw`.
- Set `OPENCLAW_LIVE_USE_REAL_HOME=1` only when you intentionally need live tests to use your real home directory.
- `pnpm test:live` now defaults to a quieter mode: it keeps `[live] ...` progress output, but suppresses the extra `~/.profile` notice and mutes gateway bootstrap logs/Bonjour chatter. Set `OPENCLAW_LIVE_TEST_QUIET=0` if you want the full startup logs back.
- API key rotation (provider-specific): set `*_API_KEYS` with comma/semicolon format or `*_API_KEY_1`, `*_API_KEY_2` (for example `OPENAI_API_KEYS`, `ANTHROPIC_API_KEYS`, `GEMINI_API_KEYS`) or per-live override via `OPENCLAW_LIVE_*_KEY`; tests retry on rate limit responses.
- Progress/heartbeat output:
  - Live suites now emit progress lines to stderr so long provider calls are visibly active even when Vitest console capture is quiet.
  - `vitest.live.config.ts` disables Vitest console interception so provider/gateway progress lines stream immediately during live runs.
  - Tune direct-model heartbeats with `OPENCLAW_LIVE_HEARTBEAT_MS`.
  - Tune gateway/probe heartbeats with `OPENCLAW_LIVE_GATEWAY_HEARTBEAT_MS`.

## Which suite should I run?

Use this decision table:

- Editing logic/tests: run `pnpm test` (and `pnpm test:coverage` if you changed a lot)
- Touching gateway networking / WS protocol / pairing: add `pnpm test:e2e`
- Debugging “my bot is down” / provider-specific failures / tool calling: run a narrowed `pnpm test:live`

## Live (network-touching) tests

For the live model matrix, CLI backend smokes, ACP smokes, Codex app-server
harness, and all media-provider live tests (Deepgram, BytePlus, ComfyUI, image,
music, video, media harness) — plus credential handling for live runs — see
[Testing live suites](https://docs.openclaw.ai/help/testing-live). For the dedicated update and
plugin validation checklist, see
[Testing updates and plugins](https://docs.openclaw.ai/help/testing-updates-plugins).

## Docker runners (optional “works in Linux” checks)

These Docker runners split into two buckets:

- Live-model runners: `test:docker:live-models` and `test:docker:live-gateway` run only their matching profile-key live file inside the repo Docker image (`src/agents/models.profiles.live.test.ts` and `src/gateway/gateway-models.profiles.live.test.ts`), mounting your local config dir and workspace (and sourcing `~/.profile` if mounted). The matching local entrypoints are `test:live:models-profiles` and `test:live:gateway-profiles`.
- Docker live runners default to a smaller smoke cap so a full Docker sweep stays practical:
`test:docker:live-models` defaults to `OPENCLAW_LIVE_MAX_MODELS=12`, and
`test:docker:live-gateway` defaults to `OPENCLAW_LIVE_GATEWAY_SMOKE=1`,
`OPENCLAW_LIVE_GATEWAY_MAX_MODELS=8`,
`OPENCLAW_LIVE_GATEWAY_STEP_TIMEOUT_MS=45000`, and
`OPENCLAW_LIVE_GATEWAY_MODEL_TIMEOUT_MS=90000`. Override those env vars when you
explicitly want the larger exhaustive scan.
- `test:docker:all` builds the live Docker image once via `test:docker:live-build`, packs OpenClaw once as an npm tarball through `scripts/package-openclaw-for-docker.mjs`, then builds/reuses two `scripts/e2e/Dockerfile` images. The bare image is only the Node/Git runner for install/update/plugin-dependency lanes; those lanes mount the prebuilt tarball. The functional image installs the same tarball into `/app` for built-app functionality lanes. Docker lane definitions live in `scripts/lib/docker-e2e-scenarios.mjs`; planner logic lives in `scripts/lib/docker-e2e-plan.mjs`; `scripts/test-docker-all.mjs` executes the selected plan. The aggregate uses a weighted local scheduler: `OPENCLAW_DOCKER_ALL_PARALLELISM` controls process slots, while resource caps keep heavy live, npm-install, and multi-service lanes from all starting at once. If a single lane is heavier than the active caps, the scheduler can still start it when the pool is empty and then keeps it running alone until capacity is available again. Defaults are 10 slots, `OPENCLAW_DOCKER_ALL_LIVE_LIMIT=9`, `OPENCLAW_DOCKER_ALL_NPM_LIMIT=10`, and `OPENCLAW_DOCKER_ALL_SERVICE_LIMIT=7`; tune `OPENCLAW_DOCKER_ALL_WEIGHT_LIMIT` or `OPENCLAW_DOCKER_ALL_DOCKER_LIMIT` only when the Docker host has more headroom. The runner performs a Docker preflight by default, removes stale OpenClaw E2E containers, prints status every 30 seconds, stores successful lane timings in `.artifacts/docker-tests/lane-timings.json`, and uses those timings to start longer lanes first on later runs. Use `OPENCLAW_DOCKER_ALL_DRY_RUN=1` to print the weighted lane manifest without building or running Docker, or `node scripts/test-docker-all.mjs --plan-json` to print the CI plan for selected lanes, package/image needs, and credentials.
- `Package Acceptance` is the GitHub-native package gate for “does this installable tarball work as a product?” It resolves one candidate package from `source=npm`, `source=ref`, `source=url`, or `source=artifact`, uploads it as `package-under-test`, then runs the reusable Docker E2E lanes against that exact tarball instead of repacking the selected ref. Profiles are ordered by breadth: `smoke`, `package`, `product`, and `full`. See [Testing updates and plugins](https://docs.openclaw.ai/help/testing-updates-plugins) for the package/update/plugin contract, published-upgrade survivor matrix, release defaults, and failure triage.
- Build and release checks run `scripts/check-cli-bootstrap-imports.mjs` after tsdown. The guard walks the static built graph from `dist/entry.js` and `dist/cli/run-main.js` and fails if pre-dispatch startup imports package dependencies such as Commander, prompt UI, undici, or logging before command dispatch; it also keeps the bundled gateway run chunk under budget and rejects static imports of known cold gateway paths. Packaged CLI smoke also covers root help, onboard help, doctor help, status, config schema, and a model-list command.
- Package Acceptance legacy compatibility is capped at `2026.4.25` (`2026.4.25-beta.*` included). Through that cutoff, the harness tolerates only shipped-package metadata gaps: omitted private QA inventory entries, missing `gateway install --wrapper`, missing patch files in the tarball-derived git fixture, missing persisted `update.channel`, legacy plugin install-record locations, missing marketplace install-record persistence, and config metadata migration during `plugins update`. For packages after `2026.4.25`, those paths are strict failures.
- Container smoke runners: `test:docker:openwebui`, `test:docker:onboard`, `test:docker:npm-onboard-channel-agent`, `test:docker:update-channel-switch`, `test:docker:upgrade-survivor`, `test:docker:published-upgrade-survivor`, `test:docker:session-runtime-context`, `test:docker:agents-delete-shared-workspace`, `test:docker:gateway-network`, `test:docker:browser-cdp-snapshot`, `test:docker:mcp-channels`, `test:docker:pi-bundle-mcp-tools`, `test:docker:cron-mcp-cleanup`, `test:docker:plugins`, `test:docker:plugin-update`, `test:docker:plugin-lifecycle-matrix`, and `test:docker:config-reload` boot one or more real containers and verify higher-level integration paths.

The live-model Docker runners also bind-mount only the needed CLI auth homes (or all supported ones when the run is not narrowed), then copy them into the container home before the run so external-CLI OAuth can refresh tokens without mutating the host auth store:

- Direct models: `pnpm test:docker:live-models` (script: `scripts/test-live-models-docker.sh`)
- ACP bind smoke: `pnpm test:docker:live-acp-bind` (script: `scripts/test-live-acp-bind-docker.sh`; covers Claude, Codex, and Gemini by default, with strict Droid/OpenCode coverage via `pnpm test:docker:live-acp-bind:droid` and `pnpm test:docker:live-acp-bind:opencode`)
- CLI backend smoke: `pnpm test:docker:live-cli-backend` (script: `scripts/test-live-cli-backend-docker.sh`)
- Codex app-server harness smoke: `pnpm test:docker:live-codex-harness` (script: `scripts/test-live-codex-harness-docker.sh`)
- Gateway + dev agent: `pnpm test:docker:live-gateway` (script: `scripts/test-live-gateway-models-docker.sh`)
- Observability smoke: `pnpm qa:otel:smoke` is a private QA source-checkout lane. It is intentionally not part of package Docker release lanes because the npm tarball omits QA Lab.
- Open WebUI live smoke: `pnpm test:docker:openwebui` (script: `scripts/e2e/openwebui-docker.sh`)
- Onboarding wizard (TTY, full scaffolding): `pnpm test:docker:onboard` (script: `scripts/e2e/onboard-docker.sh`)
- Npm tarball onboarding/channel/agent smoke: `pnpm test:docker:npm-onboard-channel-agent` installs the packed OpenClaw tarball globally in Docker, configures OpenAI via env-ref onboarding plus Telegram by default, runs doctor, and runs one mocked OpenAI agent turn. Reuse a prebuilt tarball with `OPENCLAW_CURRENT_PACKAGE_TGZ=/path/to/openclaw-*.tgz`, skip the host rebuild with `OPENCLAW_NPM_ONBOARD_HOST_BUILD=0`, or switch channel with `OPENCLAW_NPM_ONBOARD_CHANNEL=discord`.
- Update channel switch smoke: `pnpm test:docker:update-channel-switch` installs the packed OpenClaw tarball globally in Docker, switches from package `stable` to git `dev`, verifies the persisted channel and plugin post-update work, then switches back to package `stable` and checks update status.
- Upgrade survivor smoke: `pnpm test:docker:upgrade-survivor` installs the packed OpenClaw tarball over a dirty old-user fixture with agents, channel config, plugin allowlists, stale plugin dependency state, and existing workspace/session files. It runs package update plus non-interactive doctor without live provider or channel keys, then starts a loopback Gateway and checks config/state preservation plus startup/status budgets.
- Published upgrade survivor smoke: `pnpm test:docker:published-upgrade-survivor` installs `openclaw@latest` by default, seeds realistic existing-user files, configures that baseline with a baked command recipe, validates the resulting config, updates that published install to the candidate tarball, runs non-interactive doctor, writes `.artifacts/upgrade-survivor/summary.json`, then starts a loopback Gateway and checks configured intents, state preservation, startup, `/healthz`, `/readyz`, and RPC status budgets. Override one baseline with `OPENCLAW_UPGRADE_SURVIVOR_BASELINE_SPEC`, ask the aggregate scheduler to expand exact baselines with `OPENCLAW_UPGRADE_SURVIVOR_BASELINE_SPECS` such as `all-since-2026.4.23`, and expand issue-shaped fixtures with `OPENCLAW_UPGRADE_SURVIVOR_SCENARIOS` such as `reported-issues`; the reported-issues set includes `configured-plugin-installs` for automatic external OpenClaw plugin install repair. Package Acceptance exposes those as `published_upgrade_survivor_baseline`, `published_upgrade_survivor_baselines`, and `published_upgrade_survivor_scenarios`.
- Session runtime context smoke: `pnpm test:docker:session-runtime-context` verifies hidden runtime context transcript persistence plus doctor repair of affected duplicated prompt-rewrite branches.
- Bun global install smoke: `bash scripts/e2e/bun-global-install-smoke.sh` packs the current tree, installs it with `bun install -g` in an isolated home, and verifies `openclaw infer image providers --json` returns bundled image providers instead of hanging. Reuse a prebuilt tarball with `OPENCLAW_BUN_GLOBAL_SMOKE_PACKAGE_TGZ=/path/to/openclaw-*.tgz`, skip the host build with `OPENCLAW_BUN_GLOBAL_SMOKE_HOST_BUILD=0`, or copy `dist/` from a built Docker image with `OPENCLAW_BUN_GLOBAL_SMOKE_DIST_IMAGE=openclaw-dockerfile-smoke:local`.
- Installer Docker smoke: `bash scripts/test-install-sh-docker.sh` shares one npm cache across its root, update, and direct-npm containers. Update smoke defaults to npm `latest` as the stable baseline before upgrading to the candidate tarball. Override with `OPENCLAW_INSTALL_SMOKE_UPDATE_BASELINE=2026.4.22` locally, or with the Install Smoke workflow’s `update_baseline_version` input on GitHub. Non-root installer checks keep an isolated npm cache so root-owned cache entries do not mask user-local install behavior. Set `OPENCLAW_INSTALL_SMOKE_NPM_CACHE_DIR=/path/to/cache` to reuse the root/update/direct-npm cache across local reruns.
- Install Smoke CI skips the duplicate direct-npm global update with `OPENCLAW_INSTALL_SMOKE_SKIP_NPM_GLOBAL=1`; run the script locally without that env when direct `npm install -g` coverage is needed.
- Agents delete shared workspace CLI smoke: `pnpm test:docker:agents-delete-shared-workspace` (script: `scripts/e2e/agents-delete-shared-workspace-docker.sh`) builds the root Dockerfile image by default, seeds two agents with one workspace in an isolated container home, runs `agents delete --json`, and verifies valid JSON plus retained workspace behavior. Reuse the install-smoke image with `OPENCLAW_AGENTS_DELETE_SHARED_WORKSPACE_E2E_IMAGE=openclaw-dockerfile-smoke:local OPENCLAW_AGENTS_DELETE_SHARED_WORKSPACE_E2E_SKIP_BUILD=1`.
- Gateway networking (two containers, WS auth + health): `pnpm test:docker:gateway-network` (script: `scripts/e2e/gateway-network-docker.sh`)
- Browser CDP snapshot smoke: `pnpm test:docker:browser-cdp-snapshot` (script: `scripts/e2e/browser-cdp-snapshot-docker.sh`) builds the source E2E image plus a Chromium layer, starts Chromium with raw CDP, runs `browser doctor --deep`, and verifies CDP role snapshots cover link URLs, cursor-promoted clickables, iframe refs, and frame metadata.
- OpenAI Responses web\_search minimal reasoning regression: `pnpm test:docker:openai-web-search-minimal` (script: `scripts/e2e/openai-web-search-minimal-docker.sh`) runs a mocked OpenAI server through Gateway, verifies `web_search` raises `reasoning.effort` from `minimal` to `low`, then forces the provider schema reject and checks the raw detail appears in Gateway logs.
- MCP channel bridge (seeded Gateway + stdio bridge + raw Claude notification-frame smoke): `pnpm test:docker:mcp-channels` (script: `scripts/e2e/mcp-channels-docker.sh`)
- Pi bundle MCP tools (real stdio MCP server + embedded Pi profile allow/deny smoke): `pnpm test:docker:pi-bundle-mcp-tools` (script: `scripts/e2e/pi-bundle-mcp-tools-docker.sh`)
- Cron/subagent MCP cleanup (real Gateway + stdio MCP child teardown after isolated cron and one-shot subagent runs): `pnpm test:docker:cron-mcp-cleanup` (script: `scripts/e2e/cron-mcp-cleanup-docker.sh`)
- Plugins (install/update smoke for local path, `file:`, npm registry with hoisted dependencies, git moving refs, ClawHub kitchen-sink, marketplace updates, and Claude-bundle enable/inspect): `pnpm test:docker:plugins` (script: `scripts/e2e/plugins-docker.sh`)
Set `OPENCLAW_PLUGINS_E2E_CLAWHUB=0` to skip the ClawHub block, or override the default kitchen-sink package/runtime pair with `OPENCLAW_PLUGINS_E2E_CLAWHUB_SPEC` and `OPENCLAW_PLUGINS_E2E_CLAWHUB_ID`. Without `OPENCLAW_CLAWHUB_URL`/`CLAWHUB_URL`, the test uses a hermetic local ClawHub fixture server.
- Plugin update unchanged smoke: `pnpm test:docker:plugin-update` (script: `scripts/e2e/plugin-update-unchanged-docker.sh`)
- Plugin lifecycle matrix smoke: `pnpm test:docker:plugin-lifecycle-matrix` installs the packed OpenClaw tarball in a bare container, installs an npm plugin, toggles enable/disable, upgrades and downgrades it through a local npm registry, deletes the installed code, then verifies uninstall still removes stale state while logging RSS/CPU metrics for each lifecycle phase.
- Config reload metadata smoke: `pnpm test:docker:config-reload` (script: `scripts/e2e/config-reload-source-docker.sh`)
- Plugins: `pnpm test:docker:plugins` covers install/update smoke for local path, `file:`, npm registry with hoisted dependencies, git moving refs, ClawHub fixtures, marketplace updates, and Claude-bundle enable/inspect. `pnpm test:docker:plugin-update` covers unchanged update behavior for installed plugins. `pnpm test:docker:plugin-lifecycle-matrix` covers resource-tracked npm plugin install, enable, disable, upgrade, downgrade, and missing-code uninstall.

To prebuild and reuse the shared functional image manually:

```
OPENCLAW_DOCKER_E2E_IMAGE=openclaw-docker-e2e-functional:local pnpm test:docker:e2e-build
OPENCLAW_DOCKER_E2E_IMAGE=openclaw-docker-e2e-functional:local OPENCLAW_SKIP_DOCKER_BUILD=1 pnpm test:docker:mcp-channels
```

Suite-specific image overrides such as `OPENCLAW_GATEWAY_NETWORK_E2E_IMAGE` still win when set. When `OPENCLAW_SKIP_DOCKER_BUILD=1` points at a remote shared image, the scripts pull it if it is not already local. The QR and installer Docker tests keep their own Dockerfiles because they validate package/install behavior rather than the shared built-app runtime.The live-model Docker runners also bind-mount the current checkout read-only and
stage it into a temporary workdir inside the container. This keeps the runtime
image slim while still running Vitest against your exact local source/config.
The staging step skips large local-only caches and app build outputs such as
`.pnpm-store`, `.worktrees`, `__openclaw_vitest__`, and app-local `.build` or
Gradle output directories so Docker live runs do not spend minutes copying
machine-specific artifacts.
They also set `OPENCLAW_SKIP_CHANNELS=1` so gateway live probes do not start
real Telegram/Discord/etc. channel workers inside the container.
`test:docker:live-models` still runs `pnpm test:live`, so pass through
`OPENCLAW_LIVE_GATEWAY_*` as well when you need to narrow or exclude gateway
live coverage from that Docker lane.
`test:docker:openwebui` is a higher-level compatibility smoke: it starts an
OpenClaw gateway container with the OpenAI-compatible HTTP endpoints enabled,
starts a pinned Open WebUI container against that gateway, signs in through
Open WebUI, verifies `/api/models` exposes `openclaw/default`, then sends a
real chat request through Open WebUI’s `/api/chat/completions` proxy.
The first run can be noticeably slower because Docker may need to pull the
Open WebUI image and Open WebUI may need to finish its own cold-start setup.
This lane expects a usable live model key, and `OPENCLAW_PROFILE_FILE`
(`~/.profile` by default) is the primary way to provide it in Dockerized runs.
Successful runs print a small JSON payload like `{ "ok": true, "model": "openclaw/default", ... }`.
`test:docker:mcp-channels` is intentionally deterministic and does not need a
real Telegram, Discord, or iMessage account. It boots a seeded Gateway
container, starts a second container that spawns `openclaw mcp serve`, then
verifies routed conversation discovery, transcript reads, attachment metadata,
live event queue behavior, outbound send routing, and Claude-style channel +
permission notifications over the real stdio MCP bridge. The notification check
inspects the raw stdio MCP frames directly so the smoke validates what the
bridge actually emits, not just what a specific client SDK happens to surface.
`test:docker:pi-bundle-mcp-tools` is deterministic and does not need a live
model key. It builds the repo Docker image, starts a real stdio MCP probe server
inside the container, materializes that server through the embedded Pi bundle
MCP runtime, executes the tool, then verifies `coding` and `messaging` keep
`bundle-mcp` tools while `minimal` and `tools.deny: ["bundle-mcp"]` filter them.
`test:docker:cron-mcp-cleanup` is deterministic and does not need a live model
key. It starts a seeded Gateway with a real stdio MCP probe server, runs an
isolated cron turn and a `/subagents spawn` one-shot child turn, then verifies
the MCP child process exits after each run.Manual ACP plain-language thread smoke (not CI):

- `bun scripts/dev/discord-acp-plain-language-smoke.ts --channel <discord-channel-id> ...`
- Keep this script for regression/debug workflows. It may be needed again for ACP thread routing validation, so do not delete it.

Useful env vars:

- `OPENCLAW_CONFIG_DIR=...` (default: `~/.openclaw`) mounted to `/home/node/.openclaw`
- `OPENCLAW_WORKSPACE_DIR=...` (default: `~/.openclaw/workspace`) mounted to `/home/node/.openclaw/workspace`
- `OPENCLAW_PROFILE_FILE=...` (default: `~/.profile`) mounted to `/home/node/.profile` and sourced before running tests
- `OPENCLAW_DOCKER_PROFILE_ENV_ONLY=1` to verify only env vars sourced from `OPENCLAW_PROFILE_FILE`, using temporary config/workspace dirs and no external CLI auth mounts
- `OPENCLAW_DOCKER_CLI_TOOLS_DIR=...` (default: `~/.cache/openclaw/docker-cli-tools`) mounted to `/home/node/.npm-global` for cached CLI installs inside Docker
- External CLI auth dirs/files under `$HOME` are mounted read-only under `/host-auth...`, then copied into `/home/node/...` before tests start

  - Default dirs: `.minimax`
  - Default files: `~/.codex/auth.json`, `~/.codex/config.toml`, `.claude.json`, `~/.claude/.credentials.json`, `~/.claude/settings.json`, `~/.claude/settings.local.json`
  - Narrowed provider runs mount only the needed dirs/files inferred from `OPENCLAW_LIVE_PROVIDERS` / `OPENCLAW_LIVE_GATEWAY_PROVIDERS`
  - Override manually with `OPENCLAW_DOCKER_AUTH_DIRS=all`, `OPENCLAW_DOCKER_AUTH_DIRS=none`, or a comma list like `OPENCLAW_DOCKER_AUTH_DIRS=.claude,.codex`
- `OPENCLAW_LIVE_GATEWAY_MODELS=...` / `OPENCLAW_LIVE_MODELS=...` to narrow the run
- `OPENCLAW_LIVE_GATEWAY_PROVIDERS=...` / `OPENCLAW_LIVE_PROVIDERS=...` to filter providers in-container
- `OPENCLAW_SKIP_DOCKER_BUILD=1` to reuse an existing `openclaw:local-live` image for reruns that do not need a rebuild
- `OPENCLAW_LIVE_REQUIRE_PROFILE_KEYS=1` to ensure creds come from the profile store (not env)
- `OPENCLAW_OPENWEBUI_MODEL=...` to choose the model exposed by the gateway for the Open WebUI smoke
- `OPENCLAW_OPENWEBUI_PROMPT=...` to override the nonce-check prompt used by the Open WebUI smoke
- `OPENWEBUI_IMAGE=...` to override the pinned Open WebUI image tag

## Docs sanity

Run docs checks after doc edits: `pnpm check:docs`.
Run full Mintlify anchor validation when you need in-page heading checks too: `pnpm docs:check-links:anchors`.

## Offline regression (CI-safe)

These are “real pipeline” regressions without real providers:

- Gateway tool calling (mock OpenAI, real gateway + agent loop): `src/gateway/gateway.test.ts` (case: “runs a mock OpenAI tool call end-to-end via gateway agent loop”)
- Gateway wizard (WS `wizard.start`/`wizard.next`, writes config + auth enforced): `src/gateway/gateway.test.ts` (case: “runs wizard over ws and writes auth token config”)

## Agent reliability evals (skills)

We already have a few CI-safe tests that behave like “agent reliability evals”:

- Mock tool-calling through the real gateway + agent loop (`src/gateway/gateway.test.ts`).
- End-to-end wizard flows that validate session wiring and config effects (`src/gateway/gateway.test.ts`).

What’s still missing for skills (see [Skills](https://docs.openclaw.ai/tools/skills)):

- **Decisioning:** when skills are listed in the prompt, does the agent pick the right skill (or avoid irrelevant ones)?
- **Compliance:** does the agent read `SKILL.md` before use and follow required steps/args?
- **Workflow contracts:** multi-turn scenarios that assert tool order, session history carryover, and sandbox boundaries.

Future evals should stay deterministic first:

- A scenario runner using mock providers to assert tool calls + order, skill file reads, and session wiring.
- A small suite of skill-focused scenarios (use vs avoid, gating, prompt injection).
- Optional live evals (opt-in, env-gated) only after the CI-safe suite is in place.

## Contract tests (plugin and channel shape)

Contract tests verify that every registered plugin and channel conforms to its
interface contract. They iterate over all discovered plugins and run a suite of
shape and behavior assertions. The default `pnpm test` unit lane intentionally
skips these shared seam and smoke files; run the contract commands explicitly
when you touch shared channel or provider surfaces.

### Commands

- All contracts: `pnpm test:contracts`
- Channel contracts only: `pnpm test:contracts:channels`
- Provider contracts only: `pnpm test:contracts:plugins`

### Channel contracts

Located in `src/channels/plugins/contracts/*.contract.test.ts`:

- **plugin** \- Basic plugin shape (id, name, capabilities)
- **setup** \- Setup wizard contract
- **session-binding** \- Session binding behavior
- **outbound-payload** \- Message payload structure
- **inbound** \- Inbound message handling
- **actions** \- Channel action handlers
- **threading** \- Thread ID handling
- **directory** \- Directory/roster API
- **group-policy** \- Group policy enforcement

### Provider status contracts

Located in `src/plugins/contracts/*.contract.test.ts`.

- **status** \- Channel status probes
- **registry** \- Plugin registry shape

### Provider contracts

Located in `src/plugins/contracts/*.contract.test.ts`:

- **auth** \- Auth flow contract
- **auth-choice** \- Auth choice/selection
- **catalog** \- Model catalog API
- **discovery** \- Plugin discovery
- **loader** \- Plugin loading
- **runtime** \- Provider runtime
- **shape** \- Plugin shape/interface
- **wizard** \- Setup wizard

### When to run

- After changing plugin-sdk exports or subpaths
- After adding or modifying a channel or provider plugin
- After refactoring plugin registration or discovery

Contract tests run in CI and do not require real API keys.

## Adding regressions (guidance)

When you fix a provider/model issue discovered in live:

- Add a CI-safe regression if possible (mock/stub provider, or capture the exact request-shape transformation)
- If it’s inherently live-only (rate limits, auth policies), keep the live test narrow and opt-in via env vars
- Prefer targeting the smallest layer that catches the bug:
  - provider request conversion/replay bug → direct models test
  - gateway session/history/tool pipeline bug → gateway live smoke or CI-safe gateway mock test
- SecretRef traversal guardrail:
  - `src/secrets/exec-secret-ref-id-parity.test.ts` derives one sampled target per SecretRef class from registry metadata (`listSecretTargetRegistryEntries()`), then asserts traversal-segment exec ids are rejected.
  - If you add a new `includeInPlan` SecretRef target family in `src/secrets/target-registry-data.ts`, update `classifyTargetClass` in that test. The test intentionally fails on unclassified target ids so new classes cannot be skipped silently.

## Related

- [Testing live](https://docs.openclaw.ai/help/testing-live)
- [Testing updates and plugins](https://docs.openclaw.ai/help/testing-updates-plugins)
- [CI](https://docs.openclaw.ai/ci)

[Models FAQ](https://docs.openclaw.ai/help/faq-models) [Update and plugin tests](https://docs.openclaw.ai/help/testing-updates-plugins)

Ctrl+I

---

## Testing: updates and plugins - OpenClaw

_Source: <https://docs.openclaw.ai/help/testing-updates-plugins>_

[OpenClaw home page](https://docs.openclaw.ai/)

Testing

Testing: updates and plugins

This is the dedicated checklist for update and plugin validation. The goal is
simple: prove the installable package can update real user state, repair stale
legacy state through `doctor`, and still install, load, update, and uninstall
plugins from the supported sources.For the broader test runner map, see [Testing](https://docs.openclaw.ai/help/testing). For live provider
keys and network-touching suites, see [Testing live](https://docs.openclaw.ai/help/testing-live).

## What we protect

Update and plugin tests protect these contracts:

- A package tarball is complete, has a valid `dist/postinstall-inventory.json`,
and does not depend on unpacked repo files.
- A user can move from an older published package to the candidate package
without losing config, agents, sessions, workspaces, plugin allowlists, or
channel config.
- `openclaw doctor --fix --non-interactive` owns legacy cleanup and repair
paths. Startup should not grow hidden compatibility migrations for stale
plugin state.
- Plugin installs work from local directories, git repos, npm packages, and the
ClawHub registry path.
- Plugin npm dependencies are installed in the managed npm root, scanned before
trust, and removed through npm during uninstall so hoisted dependencies do not
linger.
- Plugin update is stable when nothing changed: install records, resolved
source, installed dependency layout, and enabled state stay intact.

## Local proof during development

Start narrow:

```
pnpm changed:lanes --json
pnpm check:changed
pnpm test:changed
```

For plugin install, uninstall, dependency, or package-inventory changes, also
run the focused tests that cover the edited seam:

```
pnpm test src/plugins/uninstall.test.ts src/infra/package-dist-inventory.test.ts test/scripts/package-acceptance-workflow.test.ts
```

Before any package Docker lane consumes a tarball, prove the package artifact:

```
pnpm release:check
```

`release:check` runs config/docs/API drift checks, writes the package dist
inventory, runs `npm pack --dry-run`, rejects forbidden packed files, installs
the tarball into a temp prefix, runs postinstall, and smokes bundled channel
entrypoints.

## Docker lanes

The Docker lanes are the product-level proof. They install or update a real
package inside Linux containers and assert behavior through CLI commands,
Gateway startup, HTTP probes, RPC status, and filesystem state.Use focused lanes while iterating:

```
pnpm test:docker:plugins
pnpm test:docker:plugin-lifecycle-matrix
pnpm test:docker:plugin-update
pnpm test:docker:upgrade-survivor
pnpm test:docker:published-upgrade-survivor
pnpm test:docker:update-migration
```

Important lanes:

- `test:docker:plugins` validates plugin install smoke, local folder installs,
local folder update skip behavior, local folders with preinstalled
dependencies, `file:` package installs, git installs with CLI execution, git
moving-ref updates, npm registry installs with hoisted transitive
dependencies, npm update no-ops, local ClawHub fixture installs and update
no-ops, marketplace update behavior, and Claude-bundle enable/inspect. Set
`OPENCLAW_PLUGINS_E2E_CLAWHUB=0` to keep the ClawHub block hermetic/offline.
- `test:docker:plugin-lifecycle-matrix` installs the candidate package in a bare
container, runs an npm plugin through install, inspect, disable, enable,
explicit upgrade, explicit downgrade, and uninstall after deleting the plugin
code. It logs RSS and CPU metrics for each phase.
- `test:docker:plugin-update` validates that an unchanged installed plugin does
not reinstall or lose install metadata during `openclaw plugins update`.
- `test:docker:upgrade-survivor` installs the candidate tarball over a dirty
old-user fixture, runs package update plus non-interactive doctor, then starts
a loopback Gateway and checks state preservation.
- `test:docker:published-upgrade-survivor` first installs a published baseline,
configures it through a baked `openclaw config set` recipe, updates it to the
candidate tarball, runs doctor, checks legacy cleanup, starts the Gateway, and
probes `/healthz`, `/readyz`, and RPC status.
- `test:docker:update-migration` is the cleanup-heavy published-update lane. It
starts from a configured Discord/Telegram-style user state, runs baseline
doctor so configured plugin dependencies have a chance to materialize, seeds
legacy plugin dependency debris for a configured packaged plugin, updates to
the candidate tarball, and requires post-update doctor to remove the legacy
dependency roots.

Useful published-upgrade survivor variants:

```
OPENCLAW_UPGRADE_SURVIVOR_BASELINE_SPEC=openclaw@2026.4.23 \
OPENCLAW_UPGRADE_SURVIVOR_SCENARIO=versioned-runtime-deps \
pnpm test:docker:published-upgrade-survivor

OPENCLAW_UPGRADE_SURVIVOR_BASELINE_SPEC=openclaw@latest \
OPENCLAW_UPGRADE_SURVIVOR_SCENARIO=bootstrap-persona \
pnpm test:docker:published-upgrade-survivor
```

Available scenarios are `base`, `feishu-channel`, `bootstrap-persona`,
`plugin-deps-cleanup`, `configured-plugin-installs`, `tilde-log-path`, and
`versioned-runtime-deps`. In aggregate runs,
`OPENCLAW_UPGRADE_SURVIVOR_SCENARIOS=reported-issues` expands to all reported
issue-shaped scenarios, including the configured-plugin install migration.Full update migration is intentionally separate from Full Release CI. Use the
manual `Update Migration` workflow when the release question is “can every
published stable release from 2026.4.23 onward update to this candidate and
clean up plugin dependency debris?”:

```
gh workflow run update-migration.yml \
  --ref main \
  -f workflow_ref=main \
  -f package_ref=main \
  -f baselines=all-since-2026.4.23 \
  -f scenarios=plugin-deps-cleanup
```

## Package Acceptance

Package Acceptance is the GitHub-native package gate. It resolves one candidate
package into a `package-under-test` tarball, records version and SHA-256, then
runs reusable Docker E2E lanes against that exact tarball. The workflow harness
ref is separate from the package source ref, so current test logic can validate
older trusted releases.Candidate sources:

- `source=npm`: validate `openclaw@beta`, `openclaw@latest`, or an exact
published version.
- `source=ref`: pack a trusted branch, tag, or commit with the selected current
harness.
- `source=url`: validate an HTTPS tarball with required `package_sha256`.
- `source=artifact`: reuse a tarball uploaded by another Actions run.

Full Release Validation uses `source=artifact` by default, built from the
resolved release SHA. For post-publish proof, pass
`package_acceptance_package_spec=openclaw@YYYY.M.D` so the same upgrade matrix
targets the shipped npm package instead.Release checks call Package Acceptance with the package/update/plugin set:

```
doctor-switch update-channel-switch upgrade-survivor published-upgrade-survivor plugins-offline plugin-update
```

They also pass:

```
published_upgrade_survivor_baselines=all-since-2026.4.23
published_upgrade_survivor_scenarios=reported-issues
telegram_mode=mock-openai
```

This keeps package migration, update channel switching, stale plugin dependency
cleanup, offline plugin coverage, plugin update behavior, and Telegram package
QA on the same resolved artifact.`all-since-2026.4.23` is the Full Release CI upgrade sample: every stable npm-published release from `2026.4.23` through `latest`. For exhaustive published
update migration coverage, use `all-since-2026.4.23` in the separate Update
Migration workflow instead of Full Release CI. `release-history` remains
available for manual wider sampling when you also want the legacy pre-date
anchor.Run a package profile manually when validating a candidate before release:

```
gh workflow run package-acceptance.yml \
  --ref main \
  -f workflow_ref=main \
  -f source=npm \
  -f package_spec=openclaw@beta \
  -f suite_profile=package \
  -f published_upgrade_survivor_baselines=all-since-2026.4.23 \
  -f published_upgrade_survivor_scenarios=reported-issues \
  -f telegram_mode=mock-openai
```

Use `suite_profile=product` when the release question includes MCP channels,
cron/subagent cleanup, OpenAI web search, or OpenWebUI. Use `suite_profile=full`
only when you need full Docker release-path coverage.

## Release default

For release candidates, the default proof stack is:

1. `pnpm check:changed` and `pnpm test:changed` for source-level regressions.
2. `pnpm release:check` for package artifact integrity.
3. Package Acceptance `package` profile or the release-check custom package
lanes for install/update/plugin contracts.
4. Cross-OS release checks for OS-specific installer, onboarding, and platform
behavior.
5. Live suites only when the changed surface touches provider or hosted-service
behavior.

On maintainer machines, broad gates and Docker/package product proof should run
in Testbox unless explicitly doing local proof.

## Legacy compatibility

Compatibility leniency is narrow and time boxed:

- Packages through `2026.4.25`, including `2026.4.25-beta.*`, may tolerate
already-shipped package metadata gaps in Package Acceptance.
- The published `2026.4.26` package may warn for local build metadata stamp
files already shipped.
- Later packages must satisfy modern contracts. The same gaps fail instead of
warning or skipping.

Do not add new startup migrations for these old shapes. Add or extend a doctor
repair, then prove it with `upgrade-survivor` or `published-upgrade-survivor`.

## Adding coverage

When changing update or plugin behavior, add coverage at the lowest layer that
can fail for the right reason:

- Pure path or metadata logic: unit test beside the source.
- Package inventory or packed-file behavior: `package-dist-inventory` or tarball
checker test.
- CLI install/update behavior: Docker lane assertion or fixture.
- Published-release migration behavior: `published-upgrade-survivor` scenario.
- Registry/package source behavior: `test:docker:plugins` fixture or ClawHub
fixture server.
- Dependency layout or cleanup behavior: assert both runtime execution and the
filesystem boundary. npm dependencies may be hoisted under the managed npm
root, so tests should prove the root is scanned/cleaned instead of assuming a
package-local `node_modules` tree.

Keep new Docker fixtures hermetic by default. Use local fixture registries and
fake packages unless the point of the test is live registry behavior.

## Failure triage

Start with the artifact identity:

- Package Acceptance `resolve_package` summary: source, version, SHA-256, and
artifact name.
- Docker artifacts: `.artifacts/docker-tests/**/summary.json`,
`failures.json`, lane logs, and rerun commands.
- Upgrade survivor summary: `.artifacts/upgrade-survivor/summary.json`,
including baseline version, candidate version, scenario, phase timings, and
recipe steps.

Prefer rerunning the failed exact lane with the same package artifact over
rerunning the whole release umbrella.

[Testing](https://docs.openclaw.ai/help/testing) [Live tests](https://docs.openclaw.ai/help/testing-live)

Ctrl+I

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

## Related

- [FAQ](https://docs.openclaw.ai/help/faq) — frequently asked questions
- [Gateway Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting) — gateway-specific issues
- [Doctor](https://docs.openclaw.ai/gateway/doctor) — automated health checks and repairs
- [Channel Troubleshooting](https://docs.openclaw.ai/channels/troubleshooting) — channel connectivity issues
- [Automation Troubleshooting](https://docs.openclaw.ai/automation/cron-jobs#troubleshooting) — cron and heartbeat issues

[Help](https://docs.openclaw.ai/help) [Debugging](https://docs.openclaw.ai/help/debugging)

Ctrl+I

---

## https://docs.openclaw.ai/sitemap.xml

_Source: <https://docs.openclaw.ai/sitemap.xml>_

https://docs.openclaw.ai/ar/auth-credential-semantics2026-05-01T07:59:26.586Zhttps://docs.openclaw.ai/ar/automation/cron-jobs2026-05-01T07:59:26.576Zhttps://docs.openclaw.ai/ar/automation/hooks2026-05-01T07:59:26.610Zhttps://docs.openclaw.ai/ar/automation2026-05-01T07:59:26.589Zhttps://docs.openclaw.ai/ar/automation/standing-orders2026-05-01T07:59:26.973Zhttps://docs.openclaw.ai/ar/automation/taskflow2026-05-01T07:59:26.979Zhttps://docs.openclaw.ai/ar/automation/tasks2026-05-01T07:59:27.034Zhttps://docs.openclaw.ai/ar/channels/bluebubbles2026-05-01T07:59:26.960Zhttps://docs.openclaw.ai/ar/channels/broadcast-groups2026-05-01T07:59:26.963Zhttps://docs.openclaw.ai/ar/channels/channel-routing2026-05-01T07:59:27.051Zhttps://docs.openclaw.ai/ar/channels/discord2026-05-01T07:59:27.733Zhttps://docs.openclaw.ai/ar/channels/feishu2026-05-01T07:59:27.728Zhttps://docs.openclaw.ai/ar/channels/googlechat2026-05-01T07:59:27.703Zhttps://docs.openclaw.ai/ar/channels/group-messages2026-05-01T07:59:27.707Zhttps://docs.openclaw.ai/ar/channels/groups2026-05-01T07:59:27.700Zhttps://docs.openclaw.ai/ar/channels/imessage2026-05-01T07:59:27.692Zhttps://docs.openclaw.ai/ar/channels2026-05-01T07:59:27.710Zhttps://docs.openclaw.ai/ar/channels/irc2026-05-01T07:59:27.696Zhttps://docs.openclaw.ai/ar/channels/line2026-05-01T07:59:27.689Zhttps://docs.openclaw.ai/ar/channels/location2026-05-01T07:59:27.736Zhttps://docs.openclaw.ai/ar/channels/matrix2026-05-01T07:59:28.191Zhttps://docs.openclaw.ai/ar/channels/matrix-migration2026-05-01T07:59:29.416Zhttps://docs.openclaw.ai/ar/channels/matrix-push-rules2026-05-01T07:59:28.970Zhttps://docs.openclaw.ai/ar/channels/mattermost2026-05-01T07:59:28.152Zhttps://docs.openclaw.ai/ar/channels/msteams2026-05-01T07:59:28.179Zhttps://docs.openclaw.ai/ar/channels/nextcloud-talk2026-05-01T07:59:28.182Zhttps://docs.openclaw.ai/ar/channels/nostr2026-05-01T07:59:28.155Zhttps://docs.openclaw.ai/ar/channels/pairing2026-05-01T07:59:28.138Zhttps://docs.openclaw.ai/ar/channels/qa-channel2026-05-01T07:59:28.149Zhttps://docs.openclaw.ai/ar/channels/qqbot2026-05-01T07:59:28.125Zhttps://docs.openclaw.ai/ar/channels/signal2026-05-01T07:59:29.598Zhttps://docs.openclaw.ai/ar/channels/slack2026-05-01T07:59:29.597Zhttps://docs.openclaw.ai/ar/channels/synology-chat2026-05-01T07:59:29.541Zhttps://docs.openclaw.ai/ar/channels/telegram2026-05-01T07:59:29.588Zhttps://docs.openclaw.ai/ar/channels/tlon2026-05-01T07:59:29.514Zhttps://docs.openclaw.ai/ar/channels/troubleshooting2026-05-01T07:59:29.587Zhttps://docs.openclaw.ai/ar/channels/twitch2026-05-01T07:59:29.513Zhttps://docs.openclaw.ai/ar/channels/wechat2026-05-01T07:59:29.512Zhttps://docs.openclaw.ai/ar/channels/whatsapp2026-05-01T07:59:29.582Zhttps://docs.openclaw.ai/ar/channels/yuanbao2026-05-01T07:59:29.585Zhttps://docs.openclaw.ai/ar/channels/zalo2026-05-01T07:59:29.823Zhttps://docs.openclaw.ai/ar/channels/zalouser2026-05-01T07:59:29.816Zhttps://docs.openclaw.ai/ar/ci2026-05-01T07:59:29.760Zhttps://docs.openclaw.ai/ar/cli/acp2026-05-01T07:59:29.758Zhttps://docs.openclaw.ai/ar/cli/agent2026-05-01T07:59:29.755Zhttps://docs.openclaw.ai/ar/cli/agents2026-05-01T07:59:29.756Zhttps://docs.openclaw.ai/ar/cli/approvals2026-05-01T07:59:29.822Zhttps://docs.openclaw.ai/ar/cli/backup2026-05-01T07:59:29.761Zhttps://docs.openclaw.ai/ar/cli/browser2026-05-01T07:59:29.759Zhttps://docs.openclaw.ai/ar/cli/channels2026-05-01T07:59:29.757Zhttps://docs.openclaw.ai/ar/cli/clawbot2026-05-01T07:59:29.957Zhttps://docs.openclaw.ai/ar/cli/commitments2026-05-01T07:59:29.937Zhttps://docs.openclaw.ai/ar/cli/completion2026-05-01T07:59:29.945Zhttps://docs.openclaw.ai/ar/cli/config2026-05-01T07:59:29.943Zhttps://docs.openclaw.ai/ar/cli/configure2026-05-01T07:59:29.935Zhttps://docs.openclaw.ai/ar/cli/cron2026-05-01T07:59:29.913Zhttps://docs.openclaw.ai/ar/cli/daemon2026-05-01T07:59:29.912Zhttps://docs.openclaw.ai/ar/cli/dashboard2026-05-01T07:59:29.911Zhttps://docs.openclaw.ai/ar/cli/devices2026-05-01T07:59:29.910Zhttps://docs.openclaw.ai/ar/cli/directory2026-05-01T07:59:30.070Zhttps://docs.openclaw.ai/ar/cli/dns2026-05-01T07:59:30.064Zhttps://docs.openclaw.ai/ar/cli/docs2026-05-01T07:59:30.042Zhttps://docs.openclaw.ai/ar/cli/doctor2026-05-01T07:59:30.067Zhttps://docs.openclaw.ai/ar/cli/flows2026-05-01T07:59:30.044Zhttps://docs.openclaw.ai/ar/cli/gateway2026-05-01T07:59:30.045Zhttps://docs.openclaw.ai/ar/cli/health2026-05-01T07:59:30.043Zhttps://docs.openclaw.ai/ar/cli/hooks2026-05-01T07:59:30.040Zhttps://docs.openclaw.ai/ar/cli2026-05-01T07:59:30.041Zhttps://docs.openclaw.ai/ar/cli/infer2026-05-01T07:59:30.039Zhttps://docs.openclaw.ai/ar/cli/logs2026-05-01T07:59:30.215Zhttps://docs.openclaw.ai/ar/cli/mcp2026-05-01T07:59:30.206Zhttps://docs.openclaw.ai/ar/cli/memory2026-05-01T07:59:30.184Zhttps://docs.openclaw.ai/ar/cli/message2026-05-01T07:59:30.185Zhttps://docs.openclaw.ai/ar/cli/migrate2026-05-01T07:59:30.150Zhttps://docs.openclaw.ai/ar/cli/models2026-05-01T07:59:30.216Zhttps://docs.openclaw.ai/ar/cli/node2026-05-01T07:59:30.207Zhttps://docs.openclaw.ai/ar/cli/nodes2026-05-01T07:59:30.149Zhttps://docs.openclaw.ai/ar/cli/onboard2026-05-01T07:59:30.150Zhttps://docs.openclaw.ai/ar/cli/pairing2026-05-01T07:59:30.208Zhttps://docs.openclaw.ai/ar/cli/plugins2026-05-01T07:59:30.364Zhttps://docs.openclaw.ai/ar/cli/proxy2026-05-01T07:59:30.417Zhttps://docs.openclaw.ai/ar/cli/qr2026-05-01T07:59:30.363Zhttps://docs.openclaw.ai/ar/cli/reset2026-05-01T07:59:30.356Zhttps://docs.openclaw.ai/ar/cli/sandbox2026-05-01T07:59:30.371Zhttps://docs.openclaw.ai/ar/cli/secrets2026-05-01T07:59:30.371Zhttps://docs.openclaw.ai/ar/cli/security2026-05-01T07:59:30.416Zhttps://docs.openclaw.ai/ar/cli/sessions2026-05-01T07:59:30.361Zhttps://docs.openclaw.ai/ar/cli/setup2026-05-01T07:59:30.364Zhttps://docs.openclaw.ai/ar/cli/skills2026-05-01T07:59:30.358Zhttps://docs.openclaw.ai/ar/cli/status2026-05-01T07:59:30.564Zhttps://docs.openclaw.ai/ar/cli/system2026-05-01T07:59:30.531Zhttps://docs.openclaw.ai/ar/cli/tasks2026-05-01T07:59:30.555Zhttps://docs.openclaw.ai/ar/cli/tui2026-05-01T07:59:30.525Zhttps://docs.openclaw.ai/ar/cli/uninstall2026-05-01T07:59:30.545Zhttps://docs.openclaw.ai/ar/cli/update2026-05-01T07:59:30.530Zhttps://docs.openclaw.ai/ar/cli/voicecall2026-05-01T07:59:30.522Zhttps://docs.openclaw.ai/ar/cli/webhooks2026-05-01T07:59:30.512Zhttps://docs.openclaw.ai/ar/cli/wiki2026-05-01T07:59:30.464Zhttps://docs.openclaw.ai/ar/concepts/active-memory2026-05-01T07:59:30.467Zhttps://docs.openclaw.ai/ar/concepts/agent2026-05-01T07:59:30.681Zhttps://docs.openclaw.ai/ar/concepts/agent-loop2026-05-01T07:59:30.684Zhttps://docs.openclaw.ai/ar/concepts/agent-runtimes2026-05-01T07:59:30.660Zhttps://docs.openclaw.ai/ar/concepts/agent-workspace2026-05-01T07:59:30.664Zhttps://docs.openclaw.ai/ar/concepts/architecture2026-05-01T07:59:30.661Zhttps://docs.openclaw.ai/ar/concepts/channel-docking2026-05-01T07:59:30.663Zhttps://docs.openclaw.ai/ar/concepts/commitments2026-05-01T07:59:30.662Zhttps://docs.openclaw.ai/ar/concepts/compaction2026-05-01T07:59:30.683Zhttps://docs.openclaw.ai/ar/concepts/context2026-05-01T07:59:30.681Zhttps://docs.openclaw.ai/ar/concepts/context-engine2026-05-01T07:59:30.615Zhttps://docs.openclaw.ai/ar/concepts/delegate-architecture2026-05-01T07:59:30.793Zhttps://docs.openclaw.ai/ar/concepts/dreaming2026-05-01T07:59:30.752Zhttps://docs.openclaw.ai/ar/concepts/experimental-features2026-05-01T07:59:30.750Zhttps://docs.openclaw.ai/ar/concepts/features2026-05-01T07:59:30.792Zhttps://docs.openclaw.ai/ar/concepts/markdown-formatting2026-05-01T07:59:30.780Zhttps://docs.openclaw.ai/ar/concepts/memory2026-05-01T07:59:30.751Zhttps://docs.openclaw.ai/ar/concepts/memory-builtin2026-05-01T07:59:30.779Zhttps://docs.openclaw.ai/ar/concepts/memory-honcho2026-05-01T07:59:30.771Zhttps://docs.openclaw.ai/ar/concepts/memory-qmd2026-05-01T07:59:30.766Zhttps://docs.openclaw.ai/ar/concepts/memory-search2026-05-01T07:59:30.757Zhttps://docs.openclaw.ai/ar/concepts/messages2026-05-01T07:59:30.884Zhttps://docs.openclaw.ai/ar/concepts/model-failover2026-05-01T07:59:30.880Zhttps://docs.openclaw.ai/ar/concepts/model-providers2026-05-01T07:59:30.882Zhttps://docs.openclaw.ai/ar/concepts/models2026-05-01T07:59:30.872Zhttps://docs.openclaw.ai/ar/concepts/multi-agent2026-05-01T07:59:30.870Zhttps://docs.openclaw.ai/ar/concepts/oauth2026-05-01T07:59:30.871Zhttps://docs.openclaw.ai/ar/concepts/openclaw-sdk2026-05-01T07:59:30.870Zhttps://docs.openclaw.ai/ar/concepts/presence2026-05-01T07:59:30.869Zhttps://docs.openclaw.ai/ar/concepts/qa-e2e-automation2026-05-01T07:59:30.867Zhttps://docs.openclaw.ai/ar/concepts/qa-matrix2026-05-01T07:59:30.868Zhttps://docs.openclaw.ai/ar/concepts/queue2026-05-01T07:59:30.966Zhttps://docs.openclaw.ai/ar/concepts/queue-steering2026-05-01T07:59:30.988Zhttps://docs.openclaw.ai/ar/concepts/retry2026-05-01T07:59:30.986Zhttps://docs.openclaw.ai/ar/concepts/session2026-05-01T07:59:30.972Zhttps://docs.openclaw.ai/ar/concepts/session-pruning2026-05-01T07:59:30.972Zhttps://docs.openclaw.ai/ar/concepts/session-tool2026-05-01T07:59:30.962Zhttps://docs.openclaw.ai/ar/concepts/soul2026-05-01T07:59:30.963Zhttps://docs.openclaw.ai/ar/concepts/streaming2026-05-01T07:59:30.961Zhttps://docs.openclaw.ai/ar/concepts/system-prompt2026-05-01T07:59:30.980Zhttps://docs.openclaw.ai/ar/concepts/timezone2026-05-01T07:59:30.919Zhttps://docs.openclaw.ai/ar/concepts/typebox2026-05-01T07:59:31.076Zhttps://docs.openclaw.ai/ar/concepts/typing-indicators2026-05-01T07:59:31.072Zhttps://docs.openclaw.ai/ar/concepts/usage-tracking2026-05-01T07:59:31.077Zhttps://docs.openclaw.ai/ar/date-time2026-05-01T07:59:31.079Zhttps://docs.openclaw.ai/ar/debug/node-issue2026-05-01T07:59:31.067Zhttps://docs.openclaw.ai/ar/diagnostics/flags2026-05-01T07:59:31.071Zhttps://docs.openclaw.ai/ar/gateway/authentication2026-05-01T07:59:31.069Zhttps://docs.openclaw.ai/ar/gateway/background-process2026-05-01T07:59:31.068Zhttps://docs.openclaw.ai/ar/gateway/bonjour2026-05-01T07:59:31.070Zhttps://docs.openclaw.ai/ar/gateway/bridge-protocol2026-05-01T07:59:31.069Zhttps://docs.openclaw.ai/ar/gateway/cli-backends2026-05-01T07:59:31.308Zhttps://docs.openclaw.ai/ar/gateway/config-agents2026-05-01T07:59:31.296Zhttps://docs.openclaw.ai/ar/gateway/config-channels2026-05-01T07:59:31.338Zhttps://docs.openclaw.ai/ar/gateway/config-tools2026-05-01T07:59:31.311Zhttps://docs.openclaw.ai/ar/gateway/configuration2026-05-01T07:59:31.328Zhttps://docs.openclaw.ai/ar/gateway/configuration-examples2026-05-01T07:59:31.337Zhttps://docs.openclaw.ai/ar/gateway/configuration-reference2026-05-01T07:59:31.327Zhttps://docs.openclaw.ai/ar/gateway/diagnostics2026-05-01T07:59:31.309Zhttps://docs.openclaw.ai/ar/gateway/discovery2026-05-01T07:59:31.309Zhttps://docs.openclaw.ai/ar/gateway/doctor2026-05-01T07:59:31.310Zhttps://docs.openclaw.ai/ar/gateway/gateway-lock2026-05-01T07:59:31.491Zhttps://docs.openclaw.ai/ar/gateway/health2026-05-01T07:59:31.488Zhttps://docs.openclaw.ai/ar/gateway/heartbeat2026-05-01T07:59:31.457Zhttps://docs.openclaw.ai/ar/gateway2026-05-01T07:59:31.459Zhttps://docs.openclaw.ai/ar/gateway/local-models2026-05-01T07:59:31.491Zhttps://docs.openclaw.ai/ar/gateway/logging2026-05-01T07:59:31.462Zhttps://docs.openclaw.ai/ar/gateway/multiple-gateways2026-05-01T07:59:31.460Zhttps://docs.openclaw.ai/ar/gateway/network-model2026-05-01T07:59:31.460Zhttps://docs.openclaw.ai/ar/gateway/openai-http-api2026-05-01T07:59:31.461Zhttps://docs.openclaw.ai/ar/gateway/openresponses-http-api2026-05-01T07:59:31.458Zhttps://docs.openclaw.ai/ar/gateway/openshell2026-05-01T07:59:31.616Zhttps://docs.openclaw.ai/ar/gateway/opentelemetry2026-05-01T07:59:31.617Zhttps://docs.openclaw.ai/ar/gateway/pairing2026-05-01T07:59:31.563Zhttps://docs.openclaw.ai/ar/gateway/prometheus2026-05-01T07:59:31.562Zhttps://docs.openclaw.ai/ar/gateway/protocol2026-05-01T07:59:31.562Zhttps://docs.openclaw.ai/ar/gateway/remote2026-05-01T07:59:31.561Zhttps://docs.openclaw.ai/ar/gateway/remote-gateway-readme2026-05-01T07:59:31.560Zhttps://docs.openclaw.ai/ar/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:31.619Zhttps://docs.openclaw.ai/ar/gateway/sandboxing2026-05-01T07:59:31.559Zhttps://docs.openclaw.ai/ar/gateway/secrets2026-05-01T07:59:31.678Zhttps://docs.openclaw.ai/ar/gateway/secrets-plan-contract2026-05-01T07:59:31.559Zhttps://docs.openclaw.ai/ar/gateway/security/audit-checks2026-05-01T07:59:31.699Zhttps://docs.openclaw.ai/ar/gateway/security2026-04-30T08:38:44.413Zhttps://docs.openclaw.ai/ar/gateway/tailscale2026-05-01T07:59:31.716Zhttps://docs.openclaw.ai/ar/gateway/tools-invoke-http-api2026-05-01T07:59:31.715Zhttps://docs.openclaw.ai/ar/gateway/troubleshooting2026-05-01T07:59:31.700Zhttps://docs.openclaw.ai/ar/gateway/trusted-proxy-auth2026-05-01T07:59:31.713Zhttps://docs.openclaw.ai/ar/help/debugging2026-05-01T07:59:31.698Zhttps://docs.openclaw.ai/ar/help/environment2026-05-01T07:59:31.678Zhttps://docs.openclaw.ai/ar/help/faq2026-05-01T07:59:31.831Zhttps://docs.openclaw.ai/ar/help/faq-first-run2026-05-01T07:59:31.677Zhttps://docs.openclaw.ai/ar/help/faq-models2026-05-01T07:59:31.676Zhttps://docs.openclaw.ai/ar/help/gpt55-codex-agentic-parity2026-05-01T07:59:31.817Zhttps://docs.openclaw.ai/ar/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:31.819Zhttps://docs.openclaw.ai/ar/help2026-05-01T07:59:31.816Zhttps://docs.openclaw.ai/ar/help/scripts2026-05-01T07:59:31.828Zhttps://docs.openclaw.ai/ar/help/testing2026-05-01T07:59:31.833Zhttps://docs.openclaw.ai/ar/help/testing-live2026-05-01T07:59:31.818Zhttps://docs.openclaw.ai/ar/help/troubleshooting2026-05-01T07:59:31.815Zhttps://docs.openclaw.ai/ar2026-05-01T07:59:31.814Zhttps://docs.openclaw.ai/ar/install/ansible2026-05-01T07:59:31.829Zhttps://docs.openclaw.ai/ar/install/azure2026-05-01T07:59:31.860Zhttps://docs.openclaw.ai/ar/install/bun2026-05-01T07:59:31.878Zhttps://docs.openclaw.ai/ar/install/clawdock2026-05-01T07:59:31.875Zhttps://docs.openclaw.ai/ar/install/development-channels2026-05-01T07:59:31.878Zhttps://docs.openclaw.ai/ar/install/digitalocean2026-05-01T07:59:31.862Zhttps://docs.openclaw.ai/ar/install/docker2026-05-01T07:59:31.862Zhttps://docs.openclaw.ai/ar/install/docker-vm-runtime2026-05-01T07:59:31.875Zhttps://docs.openclaw.ai/ar/install/exe-dev2026-05-01T07:59:31.861Zhttps://docs.openclaw.ai/ar/install/fly2026-05-01T07:59:31.861Zhttps://docs.openclaw.ai/ar/install/gcp2026-05-01T07:59:31.859Zhttps://docs.openclaw.ai/ar/install/hetzner2026-05-01T07:59:31.916Zhttps://docs.openclaw.ai/ar/install/hostinger2026-05-01T07:59:31.916Zhttps://docs.openclaw.ai/ar/install2026-05-01T07:59:31.913Zhttps://docs.openclaw.ai/ar/install/installer2026-05-01T07:59:31.906Zhttps://docs.openclaw.ai/ar/install/kubernetes2026-05-01T07:59:31.915Zhttps://docs.openclaw.ai/ar/install/macos-vm2026-05-01T07:59:31.908Zhttps://docs.openclaw.ai/ar/install/migrating2026-05-01T07:59:31.907Zhttps://docs.openclaw.ai/ar/install/migrating-claude2026-05-01T07:59:31.913Zhttps://docs.openclaw.ai/ar/install/migrating-hermes2026-05-01T07:59:31.906Zhttps://docs.openclaw.ai/ar/install/nix2026-05-01T07:59:31.905Zhttps://docs.openclaw.ai/ar/install/node2026-05-01T07:59:31.961Zhttps://docs.openclaw.ai/ar/install/northflank2026-05-01T07:59:31.959Zhttps://docs.openclaw.ai/ar/install/oracle2026-05-01T07:59:31.956Zhttps://docs.openclaw.ai/ar/install/podman2026-05-01T07:59:31.958Zhttps://docs.openclaw.ai/ar/install/railway2026-05-01T07:59:31.959Zhttps://docs.openclaw.ai/ar/install/raspberry-pi2026-05-01T07:59:31.956Zhttps://docs.openclaw.ai/ar/install/render2026-05-01T07:59:31.958Zhttps://docs.openclaw.ai/ar/install/uninstall2026-05-01T07:59:31.957Zhttps://docs.openclaw.ai/ar/install/updating2026-05-01T07:59:31.955Zhttps://docs.openclaw.ai/ar/logging2026-05-01T07:59:31.954Zhttps://docs.openclaw.ai/ar/network2026-05-01T07:59:31.998Zhttps://docs.openclaw.ai/ar/nodes/audio2026-05-01T07:59:32.007Zhttps://docs.openclaw.ai/ar/nodes/camera2026-05-01T07:59:32.009Zhttps://docs.openclaw.ai/ar/nodes/images2026-05-01T07:59:32.008Zhttps://docs.openclaw.ai/ar/nodes2026-05-01T07:59:32.000Zhttps://docs.openclaw.ai/ar/nodes/location-command2026-05-01T07:59:32.006Zhttps://docs.openclaw.ai/ar/nodes/media-understanding2026-05-01T07:59:32.005Zhttps://docs.openclaw.ai/ar/nodes/talk2026-05-01T07:59:32.005Zhttps://docs.openclaw.ai/ar/nodes/troubleshooting2026-05-01T07:59:31.999Zhttps://docs.openclaw.ai/ar/nodes/voicewake2026-05-01T07:59:31.999Zhttps://docs.openclaw.ai/ar/pi2026-05-01T07:59:32.048Zhttps://docs.openclaw.ai/ar/pi-dev2026-05-01T07:59:32.049Zhttps://docs.openclaw.ai/ar/platforms/android2026-05-01T07:59:32.039Zhttps://docs.openclaw.ai/ar/platforms2026-05-01T07:59:32.039Zhttps://docs.openclaw.ai/ar/platforms/ios2026-05-01T07:59:32.046Zhttps://docs.openclaw.ai/ar/platforms/linux2026-05-01T07:59:32.042Zhttps://docs.openclaw.ai/ar/platforms/mac/bundled-gateway2026-05-01T07:59:32.091Zhttps://docs.openclaw.ai/ar/platforms/mac/canvas2026-05-01T07:59:32.090Zhttps://docs.openclaw.ai/ar/platforms/mac/child-process2026-05-01T07:59:32.088Zhttps://docs.openclaw.ai/ar/platforms/mac/dev-setup2026-05-01T07:59:32.078Zhttps://docs.openclaw.ai/ar/platforms/mac/health2026-05-01T07:59:32.089Zhttps://docs.openclaw.ai/ar/platforms/mac/icon2026-05-01T07:59:32.083Zhttps://docs.openclaw.ai/ar/platforms/mac/logging2026-05-01T07:59:32.081Zhttps://docs.openclaw.ai/ar/platforms/mac/menu-bar2026-05-01T07:59:32.082Zhttps://docs.openclaw.ai/ar/platforms/mac/peekaboo2026-05-01T07:59:32.080Zhttps://docs.openclaw.ai/ar/platforms/mac/permissions2026-05-01T07:59:32.080Zhttps://docs.openclaw.ai/ar/platforms/mac/remote2026-05-01T07:59:32.142Zhttps://docs.openclaw.ai/ar/platforms/mac/signing2026-05-01T07:59:32.141Zhttps://docs.openclaw.ai/ar/platforms/mac/skills2026-05-01T07:59:32.131Zhttps://docs.openclaw.ai/ar/platforms/mac/voice-overlay2026-05-01T07:59:32.139Zhttps://docs.openclaw.ai/ar/platforms/mac/voicewake2026-05-01T07:59:32.138Zhttps://docs.openclaw.ai/ar/platforms/mac/webchat2026-05-01T07:59:32.134Zhttps://docs.openclaw.ai/ar/platforms/mac/xpc2026-05-01T07:59:32.135Zhttps://docs.openclaw.ai/ar/platforms/macos2026-05-01T07:59:32.134Zhttps://docs.openclaw.ai/ar/platforms/windows2026-05-01T07:59:32.179Zhttps://docs.openclaw.ai/ar/plugins/architecture2026-05-01T07:59:32.169Zhttps://docs.openclaw.ai/ar/plugins/architecture-internals2026-05-01T07:59:32.180Zhttps://docs.openclaw.ai/ar/plugins/building-plugins2026-05-01T07:59:32.168Zhttps://docs.openclaw.ai/ar/plugins/bundles2026-05-01T07:59:32.171Zhttps://docs.openclaw.ai/ar/plugins/codex-computer-use2026-05-01T07:59:32.169Zhttps://docs.openclaw.ai/ar/plugins/codex-harness2026-05-01T07:59:32.177Zhttps://docs.openclaw.ai/ar/plugins/community2026-05-01T07:59:32.167Zhttps://docs.openclaw.ai/ar/plugins/compatibility2026-05-01T07:59:32.219Zhttps://docs.openclaw.ai/ar/plugins/google-meet2026-05-01T07:59:32.221Zhttps://docs.openclaw.ai/ar/plugins/hooks2026-05-01T07:59:32.219Zhttps://docs.openclaw.ai/ar/plugins/manifest2026-05-01T07:59:32.217Zhttps://docs.openclaw.ai/ar/plugins/memory-lancedb2026-05-01T07:59:32.214Zhttps://docs.openclaw.ai/ar/plugins/memory-wiki2026-05-01T07:59:32.207Zhttps://docs.openclaw.ai/ar/plugins/message-presentation2026-05-01T07:59:32.205Zhttps://docs.openclaw.ai/ar/plugins/sdk-agent-harness2026-05-01T07:59:32.206Zhttps://docs.openclaw.ai/ar/plugins/sdk-channel-plugins2026-05-01T07:59:32.205Zhttps://docs.openclaw.ai/ar/plugins/sdk-channel-turn2026-05-01T07:59:32.207Zhttps://docs.openclaw.ai/ar/plugins/sdk-entrypoints2026-05-01T07:59:32.271Zhttps://docs.openclaw.ai/ar/plugins/sdk-migration2026-05-01T07:59:32.270Zhttps://docs.openclaw.ai/ar/plugins/sdk-overview2026-05-01T07:59:32.270Zhttps://docs.openclaw.ai/ar/plugins/sdk-provider-plugins2026-05-01T07:59:32.268Zhttps://docs.openclaw.ai/ar/plugins/sdk-runtime2026-05-01T07:59:32.260Zhttps://docs.openclaw.ai/ar/plugins/sdk-setup2026-05-01T07:59:32.261Zhttps://docs.openclaw.ai/ar/plugins/sdk-subpaths2026-05-01T07:59:32.261Zhttps://docs.openclaw.ai/ar/plugins/sdk-testing2026-05-01T07:59:32.260Zhttps://docs.openclaw.ai/ar/plugins/skill-workshop2026-05-01T07:59:32.259Zhttps://docs.openclaw.ai/ar/plugins/voice-call2026-05-01T07:59:32.258Zhttps://docs.openclaw.ai/ar/plugins/webhooks2026-05-01T07:59:32.302Zhttps://docs.openclaw.ai/ar/plugins/zalouser2026-05-01T07:59:32.302Zhttps://docs.openclaw.ai/ar/prose2026-05-01T07:59:32.301Zhttps://docs.openclaw.ai/ar/providers/alibaba2026-05-01T07:59:32.299Zhttps://docs.openclaw.ai/ar/providers/anthropic2026-05-01T07:59:32.299Zhttps://docs.openclaw.ai/ar/providers/arcee2026-05-01T07:59:32.297Zhttps://docs.openclaw.ai/ar/providers/azure-speech2026-05-01T07:59:32.294Zhttps://docs.openclaw.ai/ar/providers/bedrock2026-05-01T07:59:32.294Zhttps://docs.openclaw.ai/ar/providers/bedrock-mantle2026-05-01T07:59:32.300Zhttps://docs.openclaw.ai/ar/providers/chutes2026-05-01T07:59:32.342Zhttps://docs.openclaw.ai/ar/providers/claude-max-api-proxy2026-05-01T07:59:32.341Zhttps://docs.openclaw.ai/ar/providers/cloudflare-ai-gateway2026-05-01T07:59:32.333Zhttps://docs.openclaw.ai/ar/providers/comfy2026-05-01T07:59:32.332Zhttps://docs.openclaw.ai/ar/providers/deepgram2026-05-01T07:59:32.332Zhttps://docs.openclaw.ai/ar/providers/deepinfra2026-05-01T07:59:32.339Zhttps://docs.openclaw.ai/ar/providers/deepseek2026-05-01T07:59:32.334Zhttps://docs.openclaw.ai/ar/providers/elevenlabs2026-05-01T07:59:32.330Zhttps://docs.openclaw.ai/ar/providers/fal2026-05-01T07:59:32.331Zhttps://docs.openclaw.ai/ar/providers/fireworks2026-05-01T07:59:32.329Zhttps://docs.openclaw.ai/ar/providers/github-copilot2026-05-01T07:59:32.394Zhttps://docs.openclaw.ai/ar/providers/glm2026-05-01T07:59:32.390Zhttps://docs.openclaw.ai/ar/providers/google2026-05-01T07:59:32.374Zhttps://docs.openclaw.ai/ar/providers/gradium2026-05-01T07:59:32.394Zhttps://docs.openclaw.ai/ar/providers/groq2026-05-01T07:59:32.391Zhttps://docs.openclaw.ai/ar/providers/huggingface2026-05-01T07:59:32.386Zhttps://docs.openclaw.ai/ar/providers2026-05-01T07:59:32.386Zhttps://docs.openclaw.ai/ar/providers/inferrs2026-05-01T07:59:32.384Zhttps://docs.openclaw.ai/ar/providers/inworld2026-05-01T07:59:32.385Zhttps://docs.openclaw.ai/ar/providers/kilocode2026-05-01T07:59:32.383Zhttps://docs.openclaw.ai/ar/providers/litellm2026-05-01T07:59:32.436Zhttps://docs.openclaw.ai/ar/providers/lmstudio2026-05-01T07:59:32.437Zhttps://docs.openclaw.ai/ar/providers/minimax2026-05-01T07:59:32.435Zhttps://docs.openclaw.ai/ar/providers/mistral2026-05-01T07:59:32.436Zhttps://docs.openclaw.ai/ar/providers/models2026-05-01T07:59:32.430Zhttps://docs.openclaw.ai/ar/providers/moonshot2026-05-01T07:59:32.427Zhttps://docs.openclaw.ai/ar/providers/nvidia2026-05-01T07:59:32.426Zhttps://docs.openclaw.ai/ar/providers/ollama2026-05-01T07:59:32.434Zhttps://docs.openclaw.ai/ar/providers/openai2026-05-01T07:59:32.426Zhttps://docs.openclaw.ai/ar/providers/opencode2026-05-01T07:59:32.470Zhttps://docs.openclaw.ai/ar/providers/opencode-go2026-05-01T07:59:32.425Zhttps://docs.openclaw.ai/ar/providers/openrouter2026-05-01T07:59:32.468Zhttps://docs.openclaw.ai/ar/providers/perplexity-provider2026-05-01T07:59:32.469Zhttps://docs.openclaw.ai/ar/providers/qianfan2026-05-01T07:59:32.465Zhttps://docs.openclaw.ai/ar/providers/qwen2026-05-01T07:59:32.459Zhttps://docs.openclaw.ai/ar/providers/runway2026-05-01T07:59:32.461Zhttps://docs.openclaw.ai/ar/providers/sglang2026-05-01T07:59:32.467Zhttps://docs.openclaw.ai/ar/providers/stepfun2026-05-01T07:59:32.460Zhttps://docs.openclaw.ai/ar/providers/synthetic2026-05-01T07:59:32.459Zhttps://docs.openclaw.ai/ar/providers/tencent2026-05-01T07:59:32.537Zhttps://docs.openclaw.ai/ar/providers/together2026-05-01T07:59:32.533Zhttps://docs.openclaw.ai/ar/providers/venice2026-05-01T07:59:32.536Zhttps://docs.openclaw.ai/ar/providers/vercel-ai-gateway2026-05-01T07:59:32.529Zhttps://docs.openclaw.ai/ar/providers/vllm2026-05-01T07:59:32.534Zhttps://docs.openclaw.ai/ar/providers/volcengine2026-05-01T07:59:32.529Zhttps://docs.openclaw.ai/ar/providers/vydra2026-05-01T07:59:32.530Zhttps://docs.openclaw.ai/ar/providers/xai2026-05-01T07:59:32.530Zhttps://docs.openclaw.ai/ar/providers/xiaomi2026-05-01T07:59:32.528Zhttps://docs.openclaw.ai/ar/providers/zai2026-05-01T07:59:32.527Zhttps://docs.openclaw.ai/ar/reference/AGENTS.default2026-05-01T07:59:32.584Zhttps://docs.openclaw.ai/ar/reference/RELEASING2026-05-01T07:59:32.568Zhttps://docs.openclaw.ai/ar/reference/api-usage-costs2026-05-01T07:59:32.570Zhttps://docs.openclaw.ai/ar/reference/credits2026-05-01T07:59:32.560Zhttps://docs.openclaw.ai/ar/reference/device-models2026-05-01T07:59:32.560Zhttps://docs.openclaw.ai/ar/reference/full-release-validation2026-05-01T07:59:32.571Zhttps://docs.openclaw.ai/ar/reference/memory-config2026-05-01T07:59:32.570Zhttps://docs.openclaw.ai/ar/reference/openclaw-sdk-api-design2026-05-01T07:59:32.561Zhttps://docs.openclaw.ai/ar/reference/prompt-caching2026-05-01T07:59:32.559Zhttps://docs.openclaw.ai/ar/reference/rich-output-protocol2026-05-01T07:59:32.618Zhttps://docs.openclaw.ai/ar/reference/rpc2026-05-01T07:59:32.612Zhttps://docs.openclaw.ai/ar/reference/secretref-credential-surface2026-05-01T07:59:32.614Zhttps://docs.openclaw.ai/ar/reference/session-management-compaction2026-05-01T07:59:32.613Zhttps://docs.openclaw.ai/ar/reference/templates/AGENTS2026-05-01T07:59:32.603Zhttps://docs.openclaw.ai/ar/reference/templates/BOOT2026-05-01T07:59:32.604Zhttps://docs.openclaw.ai/ar/reference/templates/BOOTSTRAP2026-05-01T07:59:32.611Zhttps://docs.openclaw.ai/ar/reference/templates/HEARTBEAT2026-05-01T07:59:32.610Zhttps://docs.openclaw.ai/ar/reference/templates/IDENTITY2026-05-01T07:59:32.655Zhttps://docs.openclaw.ai/ar/reference/templates/SOUL2026-05-01T07:59:32.654Zhttps://docs.openclaw.ai/ar/reference/templates/TOOLS2026-05-01T07:59:32.645Zhttps://docs.openclaw.ai/ar/reference/templates/USER2026-05-01T07:59:32.642Zhttps://docs.openclaw.ai/ar/reference/test2026-05-01T07:59:32.643Zhttps://docs.openclaw.ai/ar/reference/token-use2026-05-01T07:59:32.646Zhttps://docs.openclaw.ai/ar/reference/transcript-hygiene2026-05-01T07:59:32.644Zhttps://docs.openclaw.ai/ar/reference/wizard2026-05-01T07:59:32.692Zhttps://docs.openclaw.ai/ar/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:32.689Zhttps://docs.openclaw.ai/ar/security/THREAT-MODEL-ATLAS2026-05-01T07:59:32.686Zhttps://docs.openclaw.ai/ar/security/formal-verification2026-05-01T07:59:32.688Zhttps://docs.openclaw.ai/ar/security/network-proxy2026-05-01T07:59:32.677Zhttps://docs.openclaw.ai/ar/start/bootstrapping2026-05-01T07:59:32.679Zhttps://docs.openclaw.ai/ar/start/docs-directory2026-05-01T07:59:32.680Zhttps://docs.openclaw.ai/ar/start/getting-started2026-05-01T07:59:32.676Zhttps://docs.openclaw.ai/ar/start/hubs2026-05-01T07:59:32.678Zhttps://docs.openclaw.ai/ar/start/lore2026-05-01T07:59:32.689Zhttps://docs.openclaw.ai/ar/start/onboarding2026-05-01T07:59:32.739Zhttps://docs.openclaw.ai/ar/start/onboarding-overview2026-05-01T07:59:32.751Zhttps://docs.openclaw.ai/ar/start/openclaw2026-05-01T07:59:32.748Zhttps://docs.openclaw.ai/ar/start/setup2026-05-01T07:59:32.750Zhttps://docs.openclaw.ai/ar/start/showcase2026-04-24T17:32:59.979Zhttps://docs.openclaw.ai/ar/start/wizard2026-05-01T07:59:32.740Zhttps://docs.openclaw.ai/ar/start/wizard-cli-automation2026-05-01T07:59:32.742Zhttps://docs.openclaw.ai/ar/start/wizard-cli-reference2026-05-01T07:59:32.741Zhttps://docs.openclaw.ai/ar/tools/acp-agents2026-05-01T07:59:32.784Zhttps://docs.openclaw.ai/ar/tools/acp-agents-setup2026-05-01T07:59:32.786Zhttps://docs.openclaw.ai/ar/tools/agent-send2026-05-01T07:59:32.785Zhttps://docs.openclaw.ai/ar/tools/apply-patch2026-05-01T07:59:32.775Zhttps://docs.openclaw.ai/ar/tools/brave-search2026-05-01T07:59:32.774Zhttps://docs.openclaw.ai/ar/tools/browser2026-05-01T07:59:32.776Zhttps://docs.openclaw.ai/ar/tools/browser-control2026-05-01T07:59:32.773Zhttps://docs.openclaw.ai/ar/tools/browser-linux-troubleshooting2026-05-01T07:59:32.774Zhttps://docs.openclaw.ai/ar/tools/browser-login2026-05-01T07:59:32.785Zhttps://docs.openclaw.ai/ar/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:32.776Zhttps://docs.openclaw.ai/ar/tools/btw2026-05-01T07:59:32.818Zhttps://docs.openclaw.ai/ar/tools/clawhub2026-05-01T07:59:32.807Zhttps://docs.openclaw.ai/ar/tools/code-execution2026-05-01T07:59:32.816Zhttps://docs.openclaw.ai/ar/tools/creating-skills2026-05-01T07:59:32.813Zhttps://docs.openclaw.ai/ar/tools/diffs2026-05-01T07:59:32.808Zhttps://docs.openclaw.ai/ar/tools/duckduckgo-search2026-05-01T07:59:32.815Zhttps://docs.openclaw.ai/ar/tools/elevated2026-05-01T07:59:32.808Zhttps://docs.openclaw.ai/ar/tools/exa-search2026-05-01T07:59:32.809Zhttps://docs.openclaw.ai/ar/tools/exec2026-05-01T07:59:32.860Zhttps://docs.openclaw.ai/ar/tools/exec-approvals2026-05-01T07:59:32.861Zhttps://docs.openclaw.ai/ar/tools/exec-approvals-advanced2026-05-01T07:59:32.807Zhttps://docs.openclaw.ai/ar/tools/firecrawl2026-05-01T07:59:32.860Zhttps://docs.openclaw.ai/ar/tools/gemini-search2026-05-01T07:59:32.853Zhttps://docs.openclaw.ai/ar/tools/grok-search2026-05-01T07:59:32.856Zhttps://docs.openclaw.ai/ar/tools/image-generation2026-05-01T07:59:32.852Zhttps://docs.openclaw.ai/ar/tools2026-05-01T07:59:32.853Zhttps://docs.openclaw.ai/ar/tools/kimi-search2026-05-01T07:59:32.852Zhttps://docs.openclaw.ai/ar/tools/llm-task2026-05-01T07:59:32.851Zhttps://docs.openclaw.ai/ar/tools/lobster2026-05-01T07:59:32.850Zhttps://docs.openclaw.ai/ar/tools/loop-detection2026-05-01T07:59:32.892Zhttps://docs.openclaw.ai/ar/tools/media-overview2026-05-01T07:59:32.890Zhttps://docs.openclaw.ai/ar/tools/minimax-search2026-05-01T07:59:32.891Zhttps://docs.openclaw.ai/ar/tools/multi-agent-sandbox-tools2026-05-01T07:59:32.888Zhttps://docs.openclaw.ai/ar/tools/music-generation2026-05-01T07:59:32.891Zhttps://docs.openclaw.ai/ar/tools/ollama-search2026-05-01T07:59:32.889Zhttps://docs.openclaw.ai/ar/tools/pdf2026-05-01T07:59:32.882Zhttps://docs.openclaw.ai/ar/tools/perplexity-search2026-05-01T07:59:32.882Zhttps://docs.openclaw.ai/ar/tools/plugin2026-05-01T07:59:32.881Zhttps://docs.openclaw.ai/ar/tools/reactions2026-05-01T07:59:32.880Zhttps://docs.openclaw.ai/ar/tools/searxng-search2026-05-01T07:59:32.927Zhttps://docs.openclaw.ai/ar/tools/skills2026-05-01T07:59:32.924Zhttps://docs.openclaw.ai/ar/tools/skills-config2026-05-01T07:59:32.921Zhttps://docs.openclaw.ai/ar/tools/slash-commands2026-05-01T07:59:32.922Zhttps://docs.openclaw.ai/ar/tools/subagents2026-05-01T07:59:32.914Zhttps://docs.openclaw.ai/ar/tools/tavily2026-05-01T07:59:32.923Zhttps://docs.openclaw.ai/ar/tools/thinking2026-05-01T07:59:32.915Zhttps://docs.openclaw.ai/ar/tools/tokenjuice2026-05-01T07:59:32.915Zhttps://docs.openclaw.ai/ar/tools/trajectory2026-05-01T07:59:32.925Zhttps://docs.openclaw.ai/ar/tools/tts2026-05-01T07:59:32.913Zhttps://docs.openclaw.ai/ar/tools/video-generation2026-05-01T07:59:32.976Zhttps://docs.openclaw.ai/ar/tools/web2026-05-01T07:59:32.969Zhttps://docs.openclaw.ai/ar/tools/web-fetch2026-05-01T07:59:32.968Zhttps://docs.openclaw.ai/ar/vps2026-05-01T07:59:32.966Zhttps://docs.openclaw.ai/ar/web/control-ui2026-05-01T07:59:32.973Zhttps://docs.openclaw.ai/ar/web/dashboard2026-05-01T07:59:32.968Zhttps://docs.openclaw.ai/ar/web2026-05-01T07:59:32.969Zhttps://docs.openclaw.ai/ar/web/tui2026-05-01T07:59:32.970Zhttps://docs.openclaw.ai/ar/web/webchat2026-05-01T07:59:32.967Zhttps://docs.openclaw.ai/auth-credential-semantics2026-05-01T07:59:33.016Zhttps://docs.openclaw.ai/automation/cron-jobs2026-05-01T07:59:33.005Zhttps://docs.openclaw.ai/automation/hooks2026-05-01T07:59:33.007Zhttps://docs.openclaw.ai/automation2026-05-01T07:59:33.003Zhttps://docs.openclaw.ai/automation/standing-orders2026-05-01T07:59:33.011Zhttps://docs.openclaw.ai/automation/taskflow2026-05-01T07:59:33.049Zhttps://docs.openclaw.ai/automation/tasks2026-05-01T07:59:33.047Zhttps://docs.openclaw.ai/channels/bluebubbles2026-05-01T07:59:33.048Zhttps://docs.openclaw.ai/channels/broadcast-groups2026-05-01T07:59:33.046Zhttps://docs.openclaw.ai/channels/channel-routing2026-05-01T07:59:33.038Zhttps://docs.openclaw.ai/channels/discord2026-05-01T11:53:02.399Zhttps://docs.openclaw.ai/channels/feishu2026-05-01T07:59:33.037Zhttps://docs.openclaw.ai/channels/googlechat2026-05-01T07:59:33.091Zhttps://docs.openclaw.ai/channels/group-messages2026-05-01T07:59:33.089Zhttps://docs.openclaw.ai/channels/groups2026-05-01T07:59:33.090Zhttps://docs.openclaw.ai/channels/imessage2026-05-01T07:59:33.089Zhttps://docs.openclaw.ai/channels2026-05-01T07:59:33.082Zhttps://docs.openclaw.ai/channels/irc2026-05-01T07:59:33.086Zhttps://docs.openclaw.ai/channels/line2026-05-01T07:59:33.082Zhttps://docs.openclaw.ai/channels/location2026-05-01T07:59:33.080Zhttps://docs.openclaw.ai/channels/matrix2026-05-01T07:59:33.136Zhttps://docs.openclaw.ai/channels/matrix-migration2026-05-01T07:59:33.081Zhttps://docs.openclaw.ai/channels/matrix-push-rules2026-05-01T07:59:33.080Zhttps://docs.openclaw.ai/channels/mattermost2026-05-01T15:13:57.862Zhttps://docs.openclaw.ai/channels/msteams2026-05-01T07:59:33.134Zhttps://docs.openclaw.ai/channels/nextcloud-talk2026-05-01T07:59:33.123Zhttps://docs.openclaw.ai/channels/nostr2026-05-01T07:59:33.132Zhttps://docs.openclaw.ai/channels/pairing2026-05-01T07:59:33.120Zhttps://docs.openclaw.ai/channels/qa-channel2026-05-01T07:59:33.121Zhttps://docs.openclaw.ai/channels/qqbot2026-05-01T07:59:33.122Zhttps://docs.openclaw.ai/channels/signal2026-05-01T07:59:33.118Zhttps://docs.openclaw.ai/channels/slack2026-05-01T13:23:47.247Zhttps://docs.openclaw.ai/channels/synology-chat2026-05-01T07:59:33.172Zhttps://docs.openclaw.ai/channels/telegram2026-05-01T07:59:33.172Zhttps://docs.openclaw.ai/channels/tlon2026-05-01T07:59:33.171Zhttps://docs.openclaw.ai/channels/troubleshooting2026-05-01T07:59:33.169Zhttps://docs.openclaw.ai/channels/twitch2026-05-01T07:59:33.163Zhttps://docs.openclaw.ai/channels/wechat2026-05-01T07:59:33.168Zhttps://docs.openclaw.ai/channels/whatsapp2026-05-01T07:59:33.164Zhttps://docs.openclaw.ai/channels/yuanbao2026-05-01T07:59:33.163Zhttps://docs.openclaw.ai/channels/zalo2026-05-01T07:59:33.162Zhttps://docs.openclaw.ai/channels/zalouser2026-05-01T07:59:33.162Zhttps://docs.openclaw.ai/ci2026-05-01T08:32:46.344Zhttps://docs.openclaw.ai/cli/acp2026-05-01T07:59:33.212Zhttps://docs.openclaw.ai/cli/agent2026-05-01T07:59:33.214Zhttps://docs.openclaw.ai/cli/agents2026-05-01T07:59:33.208Zhttps://docs.openclaw.ai/cli/approvals2026-05-01T07:59:33.205Zhttps://docs.openclaw.ai/cli/backup2026-05-01T07:59:33.205Zhttps://docs.openclaw.ai/cli/browser2026-05-01T07:59:33.214Zhttps://docs.openclaw.ai/cli/channels2026-05-01T07:59:33.204Zhttps://docs.openclaw.ai/cli/clawbot2026-05-01T07:59:33.204Zhttps://docs.openclaw.ai/cli/commitments2026-05-01T07:59:33.203Zhttps://docs.openclaw.ai/cli/completion2026-05-01T07:59:33.261Zhttps://docs.openclaw.ai/cli/config2026-05-01T07:59:33.259Zhttps://docs.openclaw.ai/cli/configure2026-05-01T07:59:33.258Zhttps://docs.openclaw.ai/cli/cron2026-05-01T07:59:33.243Zhttps://docs.openclaw.ai/cli/daemon2026-05-01T07:59:33.259Zhttps://docs.openclaw.ai/cli/dashboard2026-05-01T07:59:33.254Zhttps://docs.openclaw.ai/cli/devices2026-05-01T07:59:33.253Zhttps://docs.openclaw.ai/cli/directory2026-05-01T07:59:33.255Zhttps://docs.openclaw.ai/cli/dns2026-05-01T07:59:33.253Zhttps://docs.openclaw.ai/cli/docs2026-05-01T07:59:33.300Zhttps://docs.openclaw.ai/cli/doctor2026-05-01T15:25:17.670Zhttps://docs.openclaw.ai/cli/flows2026-05-01T07:59:33.297Zhttps://docs.openclaw.ai/cli/gateway2026-05-01T07:59:33.297Zhttps://docs.openclaw.ai/cli/health2026-05-01T07:59:33.296Zhttps://docs.openclaw.ai/cli/hooks2026-05-01T07:59:33.293Zhttps://docs.openclaw.ai/cli2026-05-01T07:59:33.292Zhttps://docs.openclaw.ai/cli/infer2026-05-01T07:59:33.291Zhttps://docs.openclaw.ai/cli/logs2026-05-01T07:59:33.290Zhttps://docs.openclaw.ai/cli/mcp2026-05-01T07:59:33.296Zhttps://docs.openclaw.ai/cli/memory2026-05-01T07:59:33.363Zhttps://docs.openclaw.ai/cli/message2026-05-01T07:59:33.365Zhttps://docs.openclaw.ai/cli/migrate2026-05-01T07:59:33.365Zhttps://docs.openclaw.ai/cli/models2026-05-01T07:59:33.364Zhttps://docs.openclaw.ai/cli/node2026-05-01T07:59:33.363Zhttps://docs.openclaw.ai/cli/nodes2026-05-01T07:59:33.361Zhttps://docs.openclaw.ai/cli/onboard2026-05-01T07:59:33.362Zhttps://docs.openclaw.ai/cli/pairing2026-05-01T07:59:33.362Zhttps://docs.openclaw.ai/cli/plugins2026-05-01T10:02:50.284Zhttps://docs.openclaw.ai/cli/proxy2026-05-01T07:59:33.356Zhttps://docs.openclaw.ai/cli/qr2026-05-01T07:59:33.394Zhttps://docs.openclaw.ai/cli/reset2026-05-01T07:59:33.393Zhttps://docs.openclaw.ai/cli/sandbox2026-05-01T07:59:33.392Zhttps://docs.openclaw.ai/cli/secrets2026-05-01T07:59:33.395Zhttps://docs.openclaw.ai/cli/security2026-05-01T15:26:44.771Zhttps://docs.openclaw.ai/cli/sessions2026-05-01T07:59:33.386Zhttps://docs.openclaw.ai/cli/setup2026-05-01T07:59:33.386Zhttps://docs.openclaw.ai/cli/skills2026-05-01T07:59:33.390Zhttps://docs.openclaw.ai/cli/status2026-05-01T07:59:33.385Zhttps://docs.openclaw.ai/cli/system2026-05-01T07:59:33.385Zhttps://docs.openclaw.ai/cli/tasks2026-05-01T07:59:33.436Zhttps://docs.openclaw.ai/cli/tui2026-05-01T07:59:33.435Zhttps://docs.openclaw.ai/cli/uninstall2026-05-01T07:59:33.435Zhttps://docs.openclaw.ai/cli/update2026-05-01T08:58:38.082Zhttps://docs.openclaw.ai/cli/voicecall2026-05-01T07:59:33.421Zhttps://docs.openclaw.ai/cli/webhooks2026-05-01T07:59:33.432Zhttps://docs.openclaw.ai/cli/wiki2026-05-01T07:59:33.418Zhttps://docs.openclaw.ai/concepts/active-memory2026-05-01T07:59:33.417Zhttps://docs.openclaw.ai/concepts/agent2026-05-01T07:59:33.466Zhttps://docs.openclaw.ai/concepts/agent-loop2026-05-01T07:59:33.417Zhttps://docs.openclaw.ai/concepts/agent-runtimes2026-05-01T07:59:33.416Zhttps://docs.openclaw.ai/concepts/agent-workspace2026-05-01T07:59:33.467Zhttps://docs.openclaw.ai/concepts/architecture2026-05-01T07:59:33.465Zhttps://docs.openclaw.ai/concepts/channel-docking2026-05-01T07:59:33.464Zhttps://docs.openclaw.ai/concepts/commitments2026-05-01T07:59:33.462Zhttps://docs.openclaw.ai/concepts/compaction2026-05-01T07:59:33.464Zhttps://docs.openclaw.ai/concepts/context2026-05-01T07:59:33.457Zhttps://docs.openclaw.ai/concepts/context-engine2026-05-01T14:47:20.734Zhttps://docs.openclaw.ai/concepts/delegate-architecture2026-05-01T07:59:33.458Zhttps://docs.openclaw.ai/concepts/dreaming2026-05-01T07:59:33.456Zhttps://docs.openclaw.ai/concepts/experimental-features2026-05-01T07:59:33.499Zhttps://docs.openclaw.ai/concepts/features2026-05-01T07:59:33.497Zhttps://docs.openclaw.ai/concepts/markdown-formatting2026-05-01T07:59:33.496Zhttps://docs.openclaw.ai/concepts/memory2026-05-01T07:59:33.489Zhttps://docs.openclaw.ai/concepts/memory-builtin2026-05-01T07:59:33.498Zhttps://docs.openclaw.ai/concepts/memory-honcho2026-05-01T07:59:33.489Zhttps://docs.openclaw.ai/concepts/memory-qmd2026-05-01T07:59:33.496Zhttps://docs.openclaw.ai/concepts/memory-search2026-05-01T07:59:33.498Zhttps://docs.openclaw.ai/concepts/messages2026-05-01T07:59:33.488Zhttps://docs.openclaw.ai/concepts/model-failover2026-05-01T07:59:33.487Zhttps://docs.openclaw.ai/concepts/model-providers2026-05-01T07:59:33.543Zhttps://docs.openclaw.ai/concepts/models2026-05-01T07:59:33.524Zhttps://docs.openclaw.ai/concepts/multi-agent2026-05-01T07:59:33.542Zhttps://docs.openclaw.ai/concepts/oauth2026-05-01T07:59:33.524Zhttps://docs.openclaw.ai/concepts/openclaw-sdk2026-05-01T08:20:30.352Zhttps://docs.openclaw.ai/concepts/presence2026-05-01T07:59:33.528Zhttps://docs.openclaw.ai/concepts/qa-e2e-automation2026-05-01T07:59:33.523Zhttps://docs.openclaw.ai/concepts/qa-matrix2026-05-01T07:59:33.525Zhttps://docs.openclaw.ai/concepts/queue2026-05-01T07:59:33.522Zhttps://docs.openclaw.ai/concepts/queue-steering2026-05-01T07:59:33.523Zhttps://docs.openclaw.ai/concepts/retry2026-05-01T07:59:33.570Zhttps://docs.openclaw.ai/concepts/session2026-05-01T12:18:51.526Zhttps://docs.openclaw.ai/concepts/session-pruning2026-05-01T07:59:33.569Zhttps://docs.openclaw.ai/concepts/session-tool2026-05-01T07:59:33.568Zhttps://docs.openclaw.ai/concepts/soul2026-05-01T07:59:33.569Zhttps://docs.openclaw.ai/concepts/streaming2026-05-01T07:59:33.561Zhttps://docs.openclaw.ai/concepts/system-prompt2026-05-01T07:59:33.568Zhttps://docs.openclaw.ai/concepts/timezone2026-05-01T07:59:33.561Zhttps://docs.openclaw.ai/concepts/typebox2026-05-01T07:59:33.571Zhttps://docs.openclaw.ai/concepts/typing-indicators2026-05-01T07:59:33.560Zhttps://docs.openclaw.ai/concepts/usage-tracking2026-05-01T07:59:33.600Zhttps://docs.openclaw.ai/date-time2026-05-01T07:59:33.599Zhttps://docs.openclaw.ai/de/auth-credential-semantics2026-05-01T07:59:33.597Zhttps://docs.openclaw.ai/de/automation/cron-jobs2026-05-01T07:59:33.596Zhttps://docs.openclaw.ai/de/automation/hooks2026-05-01T07:59:33.589Zhttps://docs.openclaw.ai/de/automation2026-05-01T07:59:33.632Zhttps://docs.openclaw.ai/de/automation/standing-orders2026-05-01T07:59:33.629Zhttps://docs.openclaw.ai/de/automation/taskflow2026-05-01T07:59:33.621Zhttps://docs.openclaw.ai/de/automation/tasks2026-05-01T07:59:33.629Zhttps://docs.openclaw.ai/de/channels/bluebubbles2026-05-01T07:59:33.622Zhttps://docs.openclaw.ai/de/channels/broadcast-groups2026-05-01T07:59:33.621Zhttps://docs.openclaw.ai/de/channels/channel-routing2026-05-01T07:59:33.670Zhttps://docs.openclaw.ai/de/channels/discord2026-05-01T07:59:33.662Zhttps://docs.openclaw.ai/de/channels/feishu2026-05-01T07:59:33.670Zhttps://docs.openclaw.ai/de/channels/googlechat2026-05-01T07:59:33.669Zhttps://docs.openclaw.ai/de/channels/group-messages2026-05-01T07:59:33.671Zhttps://docs.openclaw.ai/de/channels/groups2026-05-01T07:59:33.668Zhttps://docs.openclaw.ai/de/channels/imessage2026-05-01T07:59:33.663Zhttps://docs.openclaw.ai/de/channels2026-05-01T07:59:33.663Zhttps://docs.openclaw.ai/de/channels/irc2026-05-01T07:59:33.662Zhttps://docs.openclaw.ai/de/channels/line2026-05-01T07:59:33.661Zhttps://docs.openclaw.ai/de/channels/location2026-05-01T07:59:33.704Zhttps://docs.openclaw.ai/de/channels/matrix2026-05-01T07:59:33.701Zhttps://docs.openclaw.ai/de/channels/matrix-migration2026-05-01T07:59:33.697Zhttps://docs.openclaw.ai/de/channels/matrix-push-rules2026-05-01T07:59:33.698Zhttps://docs.openclaw.ai/de/channels/mattermost2026-05-01T07:59:33.699Zhttps://docs.openclaw.ai/de/channels/msteams2026-05-01T07:59:33.700Zhttps://docs.openclaw.ai/de/channels/nextcloud-talk2026-05-01T07:59:33.698Zhttps://docs.openclaw.ai/de/channels/nostr2026-05-01T07:59:33.690Zhttps://docs.openclaw.ai/de/channels/pairing2026-05-01T07:59:33.690Zhttps://docs.openclaw.ai/de/channels/qa-channel2026-05-01T07:59:33.689Zhttps://docs.openclaw.ai/de/channels/qqbot2026-05-01T07:59:33.750Zhttps://docs.openclaw.ai/de/channels/signal2026-05-01T07:59:33.731Zhttps://docs.openclaw.ai/de/channels/slack2026-05-01T07:59:33.730Zhttps://docs.openclaw.ai/de/channels/synology-chat2026-05-01T07:59:33.734Zhttps://docs.openclaw.ai/de/channels/telegram2026-05-01T07:59:33.738Zhttps://docs.openclaw.ai/de/channels/tlon2026-05-01T07:59:33.731Zhttps://docs.openclaw.ai/de/channels/troubleshooting2026-05-01T07:59:33.737Zhttps://docs.openclaw.ai/de/channels/twitch2026-05-01T07:59:33.730Zhttps://docs.openclaw.ai/de/channels/wechat2026-05-01T07:59:33.729Zhttps://docs.openclaw.ai/de/channels/whatsapp2026-05-01T07:59:33.728Zhttps://docs.openclaw.ai/de/channels/yuanbao2026-05-01T07:59:33.787Zhttps://docs.openclaw.ai/de/channels/zalo2026-05-01T07:59:33.789Zhttps://docs.openclaw.ai/de/channels/zalouser2026-05-01T07:59:33.778Zhttps://docs.openclaw.ai/de/ci2026-05-01T07:59:33.788Zhttps://docs.openclaw.ai/de/cli/acp2026-05-01T07:59:33.779Zhttps://docs.openclaw.ai/de/cli/agent2026-05-01T07:59:33.788Zhttps://docs.openclaw.ai/de/cli/agents2026-05-01T07:59:33.777Zhttps://docs.openclaw.ai/de/cli/approvals2026-05-01T07:59:33.778Zhttps://docs.openclaw.ai/de/cli/backup2026-05-01T07:59:33.776Zhttps://docs.openclaw.ai/de/cli/browser2026-05-01T07:59:33.775Zhttps://docs.openclaw.ai/de/cli/channels2026-05-01T07:59:33.818Zhttps://docs.openclaw.ai/de/cli/clawbot2026-05-01T07:59:33.814Zhttps://docs.openclaw.ai/de/cli/commitments2026-05-01T07:59:33.809Zhttps://docs.openclaw.ai/de/cli/completion2026-05-01T07:59:33.819Zhttps://docs.openclaw.ai/de/cli/config2026-05-01T07:59:33.816Zhttps://docs.openclaw.ai/de/cli/configure2026-05-01T07:59:33.817Zhttps://docs.openclaw.ai/de/cli/cron2026-05-01T07:59:33.817Zhttps://docs.openclaw.ai/de/cli/daemon2026-05-01T07:59:33.808Zhttps://docs.openclaw.ai/de/cli/dashboard2026-05-01T07:59:33.807Zhttps://docs.openclaw.ai/de/cli/devices2026-05-01T07:59:33.853Zhttps://docs.openclaw.ai/de/cli/directory2026-05-01T07:59:33.852Zhttps://docs.openclaw.ai/de/cli/dns2026-05-01T07:59:33.844Zhttps://docs.openclaw.ai/de/cli/docs2026-05-01T07:59:33.851Zhttps://docs.openclaw.ai/de/cli/doctor2026-05-01T07:59:33.848Zhttps://docs.openclaw.ai/de/cli/flows2026-05-01T07:59:33.848Zhttps://docs.openclaw.ai/de/cli/gateway2026-05-01T07:59:33.845Zhttps://docs.openclaw.ai/de/cli/health2026-05-01T07:59:33.845Zhttps://docs.openclaw.ai/de/cli/hooks2026-05-01T07:59:33.844Zhttps://docs.openclaw.ai/de/cli2026-05-01T07:59:33.843Zhttps://docs.openclaw.ai/de/cli/infer2026-05-01T07:59:33.894Zhttps://docs.openclaw.ai/de/cli/logs2026-05-01T07:59:33.893Zhttps://docs.openclaw.ai/de/cli/mcp2026-05-01T07:59:33.892Zhttps://docs.openclaw.ai/de/cli/memory2026-05-01T07:59:33.884Zhttps://docs.openclaw.ai/de/cli/message2026-05-01T07:59:33.891Zhttps://docs.openclaw.ai/de/cli/migrate2026-05-01T07:59:33.890Zhttps://docs.openclaw.ai/de/cli/models2026-05-01T07:59:33.886Zhttps://docs.openclaw.ai/de/cli/node2026-05-01T07:59:33.886Zhttps://docs.openclaw.ai/de/cli/nodes2026-05-01T07:59:33.885Zhttps://docs.openclaw.ai/de/cli/onboard2026-05-01T07:59:33.885Zhttps://docs.openclaw.ai/de/cli/pairing2026-05-01T07:59:33.923Zhttps://docs.openclaw.ai/de/cli/plugins2026-05-01T07:59:33.924Zhttps://docs.openclaw.ai/de/cli/proxy2026-05-01T07:59:33.922Zhttps://docs.openclaw.ai/de/cli/qr2026-05-01T07:59:33.922Zhttps://docs.openclaw.ai/de/cli/reset2026-05-01T07:59:33.916Zhttps://docs.openclaw.ai/de/cli/sandbox2026-05-01T07:59:33.915Zhttps://docs.openclaw.ai/de/cli/secrets2026-05-01T07:59:33.915Zhttps://docs.openclaw.ai/de/cli/security2026-05-01T07:59:33.914Zhttps://docs.openclaw.ai/de/cli/sessions2026-05-01T07:59:33.913Zhttps://docs.openclaw.ai/de/cli/setup2026-05-01T07:59:33.919Zhttps://docs.openclaw.ai/de/cli/skills2026-05-01T07:59:33.956Zhttps://docs.openclaw.ai/de/cli/status2026-05-01T07:59:33.957Zhttps://docs.openclaw.ai/de/cli/system2026-05-01T07:59:33.949Zhttps://docs.openclaw.ai/de/cli/tasks2026-05-01T07:59:33.955Zhttps://docs.openclaw.ai/de/cli/tui2026-05-01T07:59:33.951Zhttps://docs.openclaw.ai/de/cli/uninstall2026-05-01T07:59:33.947Zhttps://docs.openclaw.ai/de/cli/update2026-05-01T07:59:33.948Zhttps://docs.openclaw.ai/de/cli/voicecall2026-05-01T07:59:33.948Zhttps://docs.openclaw.ai/de/cli/webhooks2026-05-01T07:59:33.947Zhttps://docs.openclaw.ai/de/cli/wiki2026-05-01T07:59:33.946Zhttps://docs.openclaw.ai/de/concepts/active-memory2026-05-01T07:59:34.002Zhttps://docs.openclaw.ai/de/concepts/agent2026-05-01T07:59:34.000Zhttps://docs.openclaw.ai/de/concepts/agent-loop2026-05-01T07:59:34.003Zhttps://docs.openclaw.ai/de/concepts/agent-runtimes2026-05-01T07:59:34.001Zhttps://docs.openclaw.ai/de/concepts/agent-workspace2026-05-01T07:59:33.998Zhttps://docs.openclaw.ai/de/concepts/architecture2026-05-01T07:59:33.988Zhttps://docs.openclaw.ai/de/concepts/channel-docking2026-05-01T07:59:33.990Zhttps://docs.openclaw.ai/de/concepts/commitments2026-05-01T07:59:33.990Zhttps://docs.openclaw.ai/de/concepts/compaction2026-05-01T07:59:33.989Zhttps://docs.openclaw.ai/de/concepts/context2026-05-01T07:59:34.034Zhttps://docs.openclaw.ai/de/concepts/context-engine2026-05-01T07:59:33.987Zhttps://docs.openclaw.ai/de/concepts/delegate-architecture2026-05-01T07:59:34.035Zhttps://docs.openclaw.ai/de/concepts/dreaming2026-05-01T07:59:34.033Zhttps://docs.openclaw.ai/de/concepts/experimental-features2026-05-01T07:59:34.033Zhttps://docs.openclaw.ai/de/concepts/features2026-05-01T07:59:34.030Zhttps://docs.openclaw.ai/de/concepts/markdown-formatting2026-05-01T07:59:34.032Zhttps://docs.openclaw.ai/de/concepts/memory2026-05-01T07:59:34.085Zhttps://docs.openclaw.ai/de/concepts/memory-builtin2026-05-01T07:59:34.025Zhttps://docs.openclaw.ai/de/concepts/memory-honcho2026-05-01T07:59:34.024Zhttps://docs.openclaw.ai/de/concepts/memory-qmd2026-05-01T07:59:34.025Zhttps://docs.openclaw.ai/de/concepts/memory-search2026-05-01T07:59:34.024Zhttps://docs.openclaw.ai/de/concepts/messages2026-05-01T07:59:34.084Zhttps://docs.openclaw.ai/de/concepts/model-failover2026-05-01T07:59:34.083Zhttps://docs.openclaw.ai/de/concepts/model-providers2026-05-01T07:59:34.084Zhttps://docs.openclaw.ai/de/concepts/models2026-05-01T07:59:34.076Zhttps://docs.openclaw.ai/de/concepts/multi-agent2026-05-01T07:59:34.082Zhttps://docs.openclaw.ai/de/concepts/oauth2026-05-01T07:59:34.085Zhttps://docs.openclaw.ai/de/concepts/openclaw-sdk2026-05-01T07:59:34.076Zhttps://docs.openclaw.ai/de/concepts/presence2026-05-01T07:59:34.075Zhttps://docs.openclaw.ai/de/concepts/qa-e2e-automation2026-05-01T07:59:34.074Zhttps://docs.openclaw.ai/de/concepts/qa-matrix2026-05-01T07:59:34.124Zhttps://docs.openclaw.ai/de/concepts/queue2026-05-01T07:59:34.123Zhttps://docs.openclaw.ai/de/concepts/queue-steering2026-05-01T07:59:34.113Zhttps://docs.openclaw.ai/de/concepts/retry2026-05-01T07:59:34.114Zhttps://docs.openclaw.ai/de/concepts/session2026-05-01T07:59:34.121Zhttps://docs.openclaw.ai/de/concepts/session-pruning2026-05-01T07:59:34.122Zhttps://docs.openclaw.ai/de/concepts/session-tool2026-05-01T07:59:34.112Zhttps://docs.openclaw.ai/de/concepts/soul2026-05-01T07:59:34.120Zhttps://docs.openclaw.ai/de/concepts/streaming2026-05-01T07:59:34.113Zhttps://docs.openclaw.ai/de/concepts/system-prompt2026-05-01T07:59:34.112Zhttps://docs.openclaw.ai/de/concepts/timezone2026-05-01T07:59:34.164Zhttps://docs.openclaw.ai/de/concepts/typebox2026-05-01T07:59:34.163Zhttps://docs.openclaw.ai/de/concepts/typing-indicators2026-05-01T07:59:34.164Zhttps://docs.openclaw.ai/de/concepts/usage-tracking2026-05-01T07:59:34.147Zhttps://docs.openclaw.ai/de/date-time2026-05-01T07:59:34.146Zhttps://docs.openclaw.ai/de/debug/node-issue2026-05-01T07:59:34.147Zhttps://docs.openclaw.ai/de/diagnostics/flags2026-05-01T07:59:34.146Zhttps://docs.openclaw.ai/de/gateway/authentication2026-05-01T07:59:34.145Zhttps://docs.openclaw.ai/de/gateway/background-process2026-05-01T07:59:34.151Zhttps://docs.openclaw.ai/de/gateway/bonjour2026-05-01T07:59:34.145Zhttps://docs.openclaw.ai/de/gateway/bridge-protocol2026-05-01T07:59:34.201Zhttps://docs.openclaw.ai/de/gateway/cli-backends2026-05-01T07:59:34.200Zhttps://docs.openclaw.ai/de/gateway/config-agents2026-05-01T07:59:34.199Zhttps://docs.openclaw.ai/de/gateway/config-channels2026-05-01T07:59:34.198Zhttps://docs.openclaw.ai/de/gateway/config-tools2026-05-01T07:59:34.190Zhttps://docs.openclaw.ai/de/gateway/configuration2026-05-01T07:59:34.192Zhttps://docs.openclaw.ai/de/gateway/configuration-examples2026-05-01T07:59:34.193Zhttps://docs.openclaw.ai/de/gateway/configuration-reference2026-05-01T07:59:34.197Zhttps://docs.openclaw.ai/de/gateway/diagnostics2026-05-01T07:59:34.190Zhttps://docs.openclaw.ai/de/gateway/discovery2026-05-01T07:59:34.189Zhttps://docs.openclaw.ai/de/gateway/doctor2026-05-01T07:59:34.232Zhttps://docs.openclaw.ai/de/gateway/gateway-lock2026-05-01T07:59:34.230Zhttps://docs.openclaw.ai/de/gateway/health2026-05-01T07:59:34.231Zhttps://docs.openclaw.ai/de/gateway/heartbeat2026-05-01T07:59:34.228Zhttps://docs.openclaw.ai/de/gateway2026-05-01T07:59:34.223Zhttps://docs.openclaw.ai/de/gateway/local-models2026-05-01T07:59:34.224Zhttps://docs.openclaw.ai/de/gateway/logging2026-05-01T07:59:34.223Zhttps://docs.openclaw.ai/de/gateway/multiple-gateways2026-05-01T07:59:34.224Zhttps://docs.openclaw.ai/de/gateway/network-model2026-05-01T07:59:34.222Zhttps://docs.openclaw.ai/de/gateway/openai-http-api2026-05-01T07:59:34.221Zhttps://docs.openclaw.ai/de/gateway/openresponses-http-api2026-05-01T07:59:34.277Zhttps://docs.openclaw.ai/de/gateway/openshell2026-05-01T07:59:34.263Zhttps://docs.openclaw.ai/de/gateway/opentelemetry2026-05-01T07:59:34.276Zhttps://docs.openclaw.ai/de/gateway/pairing2026-05-01T07:59:34.258Zhttps://docs.openclaw.ai/de/gateway/prometheus2026-05-01T07:59:34.259Zhttps://docs.openclaw.ai/de/gateway/protocol2026-05-01T07:59:34.263Zhttps://docs.openclaw.ai/de/gateway/remote2026-05-01T07:59:34.259Zhttps://docs.openclaw.ai/de/gateway/remote-gateway-readme2026-05-01T07:59:34.260Zhttps://docs.openclaw.ai/de/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:34.258Zhttps://docs.openclaw.ai/de/gateway/sandboxing2026-05-01T07:59:34.264Zhttps://docs.openclaw.ai/de/gateway/secrets2026-05-01T07:59:34.305Zhttps://docs.openclaw.ai/de/gateway/secrets-plan-contract2026-05-01T07:59:34.308Zhttps://docs.openclaw.ai/de/gateway/security/audit-checks2026-05-01T07:59:34.306Zhttps://docs.openclaw.ai/de/gateway/security2026-04-30T07:24:44.963Zhttps://docs.openclaw.ai/de/gateway/tailscale2026-05-01T07:59:34.302Zhttps://docs.openclaw.ai/de/gateway/tools-invoke-http-api2026-05-01T07:59:34.306Zhttps://docs.openclaw.ai/de/gateway/troubleshooting2026-05-01T07:59:34.298Zhttps://docs.openclaw.ai/de/gateway/trusted-proxy-auth2026-05-01T07:59:34.303Zhttps://docs.openclaw.ai/de/help/debugging2026-05-01T07:59:34.302Zhttps://docs.openclaw.ai/de/help/environment2026-05-01T07:59:34.298Zhttps://docs.openclaw.ai/de/help/faq2026-05-01T07:59:34.339Zhttps://docs.openclaw.ai/de/help/faq-first-run2026-05-01T07:59:34.297Zhttps://docs.openclaw.ai/de/help/faq-models2026-05-01T07:59:34.341Zhttps://docs.openclaw.ai/de/help/gpt55-codex-agentic-parity2026-05-01T07:59:34.340Zhttps://docs.openclaw.ai/de/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:34.330Zhttps://docs.openclaw.ai/de/help2026-05-01T07:59:34.329Zhttps://docs.openclaw.ai/de/help/scripts2026-05-01T07:59:34.337Zhttps://docs.openclaw.ai/de/help/testing2026-05-01T07:59:34.338Zhttps://docs.openclaw.ai/de/help/testing-live2026-05-01T07:59:34.330Zhttps://docs.openclaw.ai/de/help/troubleshooting2026-05-01T07:59:34.329Zhttps://docs.openclaw.ai/de2026-05-01T07:59:34.328Zhttps://docs.openclaw.ai/de/install/ansible2026-05-01T07:59:34.388Zhttps://docs.openclaw.ai/de/install/azure2026-05-01T07:59:34.370Zhttps://docs.openclaw.ai/de/install/bun2026-05-01T07:59:34.389Zhttps://docs.openclaw.ai/de/install/clawdock2026-05-01T07:59:34.373Zhttps://docs.openclaw.ai/de/install/development-channels2026-05-01T07:59:34.374Zhttps://docs.openclaw.ai/de/install/digitalocean2026-05-01T07:59:34.362Zhttps://docs.openclaw.ai/de/install/docker2026-05-01T07:59:34.363Zhttps://docs.openclaw.ai/de/install/docker-vm-runtime2026-05-01T07:59:34.371Zhttps://docs.openclaw.ai/de/install/exe-dev2026-05-01T07:59:34.361Zhttps://docs.openclaw.ai/de/install/fly2026-05-01T07:59:34.360Zhttps://docs.openclaw.ai/de/install/gcp2026-05-01T07:59:34.419Zhttps://docs.openclaw.ai/de/install/hetzner2026-05-01T07:59:34.420Zhttps://docs.openclaw.ai/de/install/hostinger2026-05-01T07:59:34.416Zhttps://docs.openclaw.ai/de/install2026-05-01T07:59:34.413Zhttps://docs.openclaw.ai/de/install/installer2026-05-01T07:59:34.412Zhttps://docs.openclaw.ai/de/install/kubernetes2026-05-01T07:59:34.419Zhttps://docs.openclaw.ai/de/install/macos-vm2026-05-01T07:59:34.413Zhttps://docs.openclaw.ai/de/install/migrating2026-05-01T07:59:34.411Zhttps://docs.openclaw.ai/de/install/migrating-claude2026-05-01T07:59:34.412Zhttps://docs.openclaw.ai/de/install/migrating-hermes2026-05-01T07:59:34.411Zhttps://docs.openclaw.ai/de/install/nix2026-05-01T07:59:34.444Zhttps://docs.openclaw.ai/de/install/node2026-05-01T07:59:34.447Zhttps://docs.openclaw.ai/de/install/northflank2026-05-01T07:59:34.445Zhttps://docs.openclaw.ai/de/install/oracle2026-05-01T07:59:34.440Zhttps://docs.openclaw.ai/de/install/podman2026-05-01T07:59:34.446Zhttps://docs.openclaw.ai/de/install/railway2026-05-01T07:59:34.444Zhttps://docs.openclaw.ai/de/install/raspberry-pi2026-05-01T07:59:34.441Zhttps://docs.openclaw.ai/de/install/render2026-05-01T07:59:34.441Zhttps://docs.openclaw.ai/de/install/uninstall2026-05-01T07:59:34.440Zhttps://docs.openclaw.ai/de/install/updating2026-05-01T07:59:34.439Zhttps://docs.openclaw.ai/de/logging2026-05-01T07:59:34.475Zhttps://docs.openclaw.ai/de/network2026-05-01T07:59:34.473Zhttps://docs.openclaw.ai/de/nodes/audio2026-05-01T07:59:34.476Zhttps://docs.openclaw.ai/de/nodes/camera2026-05-01T07:59:34.471Zhttps://docs.openclaw.ai/de/nodes/images2026-05-01T07:59:34.475Zhttps://docs.openclaw.ai/de/nodes2026-05-01T07:59:34.474Zhttps://docs.openclaw.ai/de/nodes/location-command2026-05-01T07:59:34.466Zhttps://docs.openclaw.ai/de/nodes/media-understanding2026-05-01T07:59:34.473Zhttps://docs.openclaw.ai/de/nodes/talk2026-05-01T07:59:34.465Zhttps://docs.openclaw.ai/de/nodes/troubleshooting2026-05-01T07:59:34.464Zhttps://docs.openclaw.ai/de/nodes/voicewake2026-05-01T07:59:34.514Zhttps://docs.openclaw.ai/de/pi2026-05-01T07:59:34.513Zhttps://docs.openclaw.ai/de/pi-dev2026-05-01T07:59:34.514Zhttps://docs.openclaw.ai/de/platforms/android2026-05-01T07:59:34.511Zhttps://docs.openclaw.ai/de/platforms2026-05-01T07:59:34.504Zhttps://docs.openclaw.ai/de/platforms/ios2026-05-01T07:59:34.504Zhttps://docs.openclaw.ai/de/platforms/linux2026-05-01T07:59:34.543Zhttps://docs.openclaw.ai/de/platforms/mac/bundled-gateway2026-05-01T07:59:34.542Zhttps://docs.openclaw.ai/de/platforms/mac/canvas2026-05-01T07:59:34.541Zhttps://docs.openclaw.ai/de/platforms/mac/child-process2026-05-01T07:59:34.533Zhttps://docs.openclaw.ai/de/platforms/mac/dev-setup2026-05-01T07:59:34.540Zhttps://docs.openclaw.ai/de/platforms/mac/health2026-05-01T07:59:34.538Zhttps://docs.openclaw.ai/de/platforms/mac/icon2026-05-01T07:59:34.541Zhttps://docs.openclaw.ai/de/platforms/mac/logging2026-05-01T07:59:34.533Zhttps://docs.openclaw.ai/de/platforms/mac/menu-bar2026-05-01T07:59:34.532Zhttps://docs.openclaw.ai/de/platforms/mac/peekaboo2026-05-01T07:59:34.539Zhttps://docs.openclaw.ai/de/platforms/mac/permissions2026-05-01T07:59:34.573Zhttps://docs.openclaw.ai/de/platforms/mac/remote2026-05-01T07:59:34.572Zhttps://docs.openclaw.ai/de/platforms/mac/signing2026-05-01T07:59:34.563Zhttps://docs.openclaw.ai/de/platforms/mac/skills2026-05-01T07:59:34.572Zhttps://docs.openclaw.ai/de/platforms/mac/voice-overlay2026-05-01T07:59:34.566Zhttps://docs.openclaw.ai/de/platforms/mac/voicewake2026-05-01T07:59:34.570Zhttps://docs.openclaw.ai/de/platforms/mac/webchat2026-05-01T07:59:34.566Zhttps://docs.openclaw.ai/de/platforms/mac/xpc2026-05-01T07:59:34.564Zhttps://docs.openclaw.ai/de/platforms/macos2026-05-01T07:59:34.565Zhttps://docs.openclaw.ai/de/platforms/windows2026-05-01T07:59:34.613Zhttps://docs.openclaw.ai/de/plugins/architecture2026-05-01T07:59:34.606Zhttps://docs.openclaw.ai/de/plugins/architecture-internals2026-05-01T07:59:34.605Zhttps://docs.openclaw.ai/de/plugins/building-plugins2026-05-01T07:59:34.607Zhttps://docs.openclaw.ai/de/plugins/bundles2026-05-01T07:59:34.607Zhttps://docs.openclaw.ai/de/plugins/codex-computer-use2026-05-01T07:59:34.606Zhttps://docs.openclaw.ai/de/plugins/codex-harness2026-05-01T07:59:34.611Zhttps://docs.openclaw.ai/de/plugins/community2026-05-01T07:59:34.650Zhttps://docs.openclaw.ai/de/plugins/compatibility2026-05-01T07:59:34.643Zhttps://docs.openclaw.ai/de/plugins/google-meet2026-05-01T07:59:34.644Zhttps://docs.openclaw.ai/de/plugins/hooks2026-05-01T07:59:34.635Zhttps://docs.openclaw.ai/de/plugins/manifest2026-05-01T07:59:34.646Zhttps://docs.openclaw.ai/de/plugins/memory-lancedb2026-05-01T07:59:34.636Zhttps://docs.openclaw.ai/de/plugins/memory-wiki2026-05-01T07:59:34.644Zhttps://docs.openclaw.ai/de/plugins/message-presentation2026-05-01T07:59:34.636Zhttps://docs.openclaw.ai/de/plugins/sdk-agent-harness2026-05-01T07:59:34.645Zhttps://docs.openclaw.ai/de/plugins/sdk-channel-plugins2026-05-01T07:59:34.634Zhttps://docs.openclaw.ai/de/plugins/sdk-channel-turn2026-05-01T07:59:34.704Zhttps://docs.openclaw.ai/de/plugins/sdk-entrypoints2026-05-01T07:59:34.684Zhttps://docs.openclaw.ai/de/plugins/sdk-migration2026-05-01T07:59:34.689Zhttps://docs.openclaw.ai/de/plugins/sdk-overview2026-05-01T07:59:34.691Zhttps://docs.openclaw.ai/de/plugins/sdk-provider-plugins2026-05-01T07:59:34.692Zhttps://docs.openclaw.ai/de/plugins/sdk-runtime2026-05-01T07:59:34.692Zhttps://docs.openclaw.ai/de/plugins/sdk-setup2026-05-01T07:59:34.680Zhttps://docs.openclaw.ai/de/plugins/sdk-subpaths2026-05-01T07:59:34.681Zhttps://docs.openclaw.ai/de/plugins/sdk-testing2026-05-01T07:59:34.679Zhttps://docs.openclaw.ai/de/plugins/skill-workshop2026-05-01T07:59:34.679Zhttps://docs.openclaw.ai/de/plugins/voice-call2026-05-01T07:59:34.736Zhttps://docs.openclaw.ai/de/plugins/webhooks2026-05-01T07:59:34.728Zhttps://docs.openclaw.ai/de/plugins/zalouser2026-05-01T07:59:34.729Zhttps://docs.openclaw.ai/de/prose2026-05-01T07:59:34.731Zhttps://docs.openclaw.ai/de/providers/alibaba2026-05-01T07:59:34.733Zhttps://docs.openclaw.ai/de/providers/anthropic2026-05-01T07:59:34.730Zhttps://docs.openclaw.ai/de/providers/arcee2026-05-01T07:59:34.729Zhttps://docs.openclaw.ai/de/providers/azure-speech2026-05-01T07:59:34.722Zhttps://docs.openclaw.ai/de/providers/bedrock2026-05-01T07:59:34.722Zhttps://docs.openclaw.ai/de/providers/bedrock-mantle2026-05-01T07:59:34.732Zhttps://docs.openclaw.ai/de/providers/chutes2026-05-01T07:59:34.764Zhttps://docs.openclaw.ai/de/providers/claude-max-api-proxy2026-05-01T07:59:34.756Zhttps://docs.openclaw.ai/de/providers/cloudflare-ai-gateway2026-05-01T07:59:34.760Zhttps://docs.openclaw.ai/de/providers/comfy2026-05-01T07:59:34.763Zhttps://docs.openclaw.ai/de/providers/deepgram2026-05-01T07:59:34.755Zhttps://docs.openclaw.ai/de/providers/deepinfra2026-05-01T07:59:34.765Zhttps://docs.openclaw.ai/de/providers/deepseek2026-05-01T07:59:34.763Zhttps://docs.openclaw.ai/de/providers/elevenlabs2026-05-01T07:59:34.756Zhttps://docs.openclaw.ai/de/providers/fal2026-05-01T07:59:34.755Zhttps://docs.openclaw.ai/de/providers/fireworks2026-05-01T07:59:34.822Zhttps://docs.openclaw.ai/de/providers/github-copilot2026-05-01T07:59:34.813Zhttps://docs.openclaw.ai/de/providers/glm2026-05-01T07:59:34.821Zhttps://docs.openclaw.ai/de/providers/google2026-05-01T07:59:34.821Zhttps://docs.openclaw.ai/de/providers/gradium2026-05-01T07:59:34.823Zhttps://docs.openclaw.ai/de/providers/groq2026-05-01T07:59:34.819Zhttps://docs.openclaw.ai/de/providers/huggingface2026-05-01T07:59:34.813Zhttps://docs.openclaw.ai/de/providers2026-05-01T07:59:34.820Zhttps://docs.openclaw.ai/de/providers/inferrs2026-05-01T07:59:34.814Zhttps://docs.openclaw.ai/de/providers/inworld2026-05-01T07:59:34.812Zhttps://docs.openclaw.ai/de/providers/kilocode2026-05-01T07:59:34.846Zhttps://docs.openclaw.ai/de/providers/litellm2026-05-01T07:59:34.854Zhttps://docs.openclaw.ai/de/providers/lmstudio2026-05-01T07:59:34.854Zhttps://docs.openclaw.ai/de/providers/minimax2026-05-01T07:59:34.847Zhttps://docs.openclaw.ai/de/providers/mistral2026-05-01T07:59:34.847Zhttps://docs.openclaw.ai/de/providers/models2026-05-01T07:59:34.849Zhttps://docs.openclaw.ai/de/providers/moonshot2026-05-01T07:59:34.848Zhttps://docs.openclaw.ai/de/providers/nvidia2026-05-01T07:59:34.840Zhttps://docs.openclaw.ai/de/providers/ollama2026-05-01T07:59:34.851Zhttps://docs.openclaw.ai/de/providers/openai2026-05-01T07:59:34.840Zhttps://docs.openclaw.ai/de/providers/opencode2026-05-01T07:59:34.883Zhttps://docs.openclaw.ai/de/providers/opencode-go2026-05-01T07:59:34.894Zhttps://docs.openclaw.ai/de/providers/openrouter2026-05-01T07:59:34.881Zhttps://docs.openclaw.ai/de/providers/perplexity-provider2026-05-01T07:59:34.879Zhttps://docs.openclaw.ai/de/providers/qianfan2026-05-01T07:59:34.874Zhttps://docs.openclaw.ai/de/providers/qwen2026-05-01T07:59:34.882Zhttps://docs.openclaw.ai/de/providers/runway2026-05-01T07:59:34.873Zhttps://docs.openclaw.ai/de/providers/sglang2026-05-01T07:59:34.874Zhttps://docs.openclaw.ai/de/providers/stepfun2026-05-01T07:59:34.873Zhttps://docs.openclaw.ai/de/providers/synthetic2026-05-01T07:59:34.923Zhttps://docs.openclaw.ai/de/providers/tencent2026-05-01T07:59:34.922Zhttps://docs.openclaw.ai/de/providers/together2026-05-01T07:59:34.922Zhttps://docs.openclaw.ai/de/providers/venice2026-05-01T07:59:34.916Zhttps://docs.openclaw.ai/de/providers/vercel-ai-gateway2026-05-01T07:59:34.915Zhttps://docs.openclaw.ai/de/providers/vllm2026-05-01T07:59:34.916Zhttps://docs.openclaw.ai/de/providers/volcengine2026-05-01T07:59:34.915Zhttps://docs.openclaw.ai/de/providers/vydra2026-05-01T07:59:34.914Zhttps://docs.openclaw.ai/de/providers/xai2026-05-01T07:59:34.914Zhttps://docs.openclaw.ai/de/providers/xiaomi2026-05-01T07:59:34.920Zhttps://docs.openclaw.ai/de/providers/zai2026-05-01T07:59:34.952Zhttps://docs.openclaw.ai/de/reference/AGENTS.default2026-05-01T07:59:34.951Zhttps://docs.openclaw.ai/de/reference/RELEASING2026-05-01T07:59:34.952Zhttps://docs.openclaw.ai/de/reference/api-usage-costs2026-05-01T07:59:34.949Zhttps://docs.openclaw.ai/de/reference/credits2026-05-01T07:59:34.944Zhttps://docs.openclaw.ai/de/reference/device-models2026-05-01T07:59:34.945Zhttps://docs.openclaw.ai/de/reference/full-release-validation2026-05-01T07:59:34.944Zhttps://docs.openclaw.ai/de/reference/memory-config2026-05-01T07:59:34.943Zhttps://docs.openclaw.ai/de/reference/openclaw-sdk-api-design2026-05-01T07:59:34.943Zhttps://docs.openclaw.ai/de/reference/prompt-caching2026-05-01T07:59:34.992Zhttps://docs.openclaw.ai/de/reference/rich-output-protocol2026-05-01T07:59:34.992Zhttps://docs.openclaw.ai/de/reference/rpc2026-05-01T07:59:34.981Zhttps://docs.openclaw.ai/de/reference/secretref-credential-surface2026-05-01T07:59:34.979Zhttps://docs.openclaw.ai/de/reference/session-management-compaction2026-05-01T07:59:34.974Zhttps://docs.openclaw.ai/de/reference/templates/AGENTS2026-05-01T07:59:34.975Zhttps://docs.openclaw.ai/de/reference/templates/BOOT2026-05-01T07:59:34.974Zhttps://docs.openclaw.ai/de/reference/templates/BOOTSTRAP2026-05-01T07:59:34.973Zhttps://docs.openclaw.ai/de/reference/templates/HEARTBEAT2026-05-01T07:59:34.973Zhttps://docs.openclaw.ai/de/reference/templates/IDENTITY2026-05-01T07:59:35.025Zhttps://docs.openclaw.ai/de/reference/templates/SOUL2026-05-01T07:59:35.015Zhttps://docs.openclaw.ai/de/reference/templates/TOOLS2026-05-01T07:59:35.023Zhttps://docs.openclaw.ai/de/reference/templates/USER2026-05-01T07:59:35.016Zhttps://docs.openclaw.ai/de/reference/test2026-05-01T07:59:35.015Zhttps://docs.openclaw.ai/de/reference/token-use2026-05-01T07:59:35.017Zhttps://docs.openclaw.ai/de/reference/transcript-hygiene2026-05-01T07:59:35.053Zhttps://docs.openclaw.ai/de/reference/wizard2026-05-01T07:59:35.054Zhttps://docs.openclaw.ai/de/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:35.051Zhttps://docs.openclaw.ai/de/security/THREAT-MODEL-ATLAS2026-05-01T07:59:35.052Zhttps://docs.openclaw.ai/de/security/formal-verification2026-05-01T07:59:35.049Zhttps://docs.openclaw.ai/de/security/network-proxy2026-05-01T07:59:35.050Zhttps://docs.openclaw.ai/de/start/bootstrapping2026-05-01T07:59:35.052Zhttps://docs.openclaw.ai/de/start/docs-directory2026-05-01T07:59:35.043Zhttps://docs.openclaw.ai/de/start/getting-started2026-05-01T07:59:35.042Zhttps://docs.openclaw.ai/de/start/hubs2026-05-01T07:59:35.042Zhttps://docs.openclaw.ai/de/start/lore2026-05-01T07:59:35.083Zhttps://docs.openclaw.ai/de/start/onboarding2026-05-01T07:59:35.076Zhttps://docs.openclaw.ai/de/start/onboarding-overview2026-05-01T07:59:35.083Zhttps://docs.openclaw.ai/de/start/openclaw2026-05-01T07:59:35.076Zhttps://docs.openclaw.ai/de/start/setup2026-05-01T07:59:35.082Zhttps://docs.openclaw.ai/de/start/showcase2026-04-24T17:33:03.034Zhttps://docs.openclaw.ai/de/start/wizard2026-05-01T07:59:35.080Zhttps://docs.openclaw.ai/de/start/wizard-cli-automation2026-05-01T07:59:35.075Zhttps://docs.openclaw.ai/de/start/wizard-cli-reference2026-05-01T07:59:35.074Zhttps://docs.openclaw.ai/de/tools/acp-agents2026-05-01T07:59:35.116Zhttps://docs.openclaw.ai/de/tools/acp-agents-setup2026-05-01T07:59:35.122Zhttps://docs.openclaw.ai/de/tools/agent-send2026-05-01T07:59:35.118Zhttps://docs.openclaw.ai/de/tools/apply-patch2026-05-01T07:59:35.117Zhttps://docs.openclaw.ai/de/tools/brave-search2026-05-01T07:59:35.120Zhttps://docs.openclaw.ai/de/tools/browser2026-05-01T07:59:35.152Zhttps://docs.openclaw.ai/de/tools/browser-control2026-05-01T07:59:35.120Zhttps://docs.openclaw.ai/de/tools/browser-linux-troubleshooting2026-05-01T07:59:35.111Zhttps://docs.openclaw.ai/de/tools/browser-login2026-05-01T07:59:35.110Zhttps://docs.openclaw.ai/de/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:35.110Zhttps://docs.openclaw.ai/de/tools/btw2026-05-01T07:59:35.151Zhttps://docs.openclaw.ai/de/tools/clawhub2026-05-01T07:59:35.150Zhttps://docs.openclaw.ai/de/tools/code-execution2026-05-01T07:59:35.148Zhttps://docs.openclaw.ai/de/tools/creating-skills2026-05-01T07:59:35.145Zhttps://docs.openclaw.ai/de/tools/diffs2026-05-01T07:59:35.146Zhttps://docs.openclaw.ai/de/tools/duckduckgo-search2026-05-01T07:59:35.149Zhttps://docs.openclaw.ai/de/tools/elevated2026-05-01T07:59:35.140Zhttps://docs.openclaw.ai/de/tools/exa-search2026-05-01T07:59:35.139Zhttps://docs.openclaw.ai/de/tools/exec2026-05-01T07:59:35.180Zhttps://docs.openclaw.ai/de/tools/exec-approvals2026-05-01T07:59:35.181Zhttps://docs.openclaw.ai/de/tools/exec-approvals-advanced2026-05-01T07:59:35.182Zhttps://docs.openclaw.ai/de/tools/firecrawl2026-05-01T07:59:35.172Zhttps://docs.openclaw.ai/de/tools/gemini-search2026-05-01T07:59:35.177Zhttps://docs.openclaw.ai/de/tools/grok-search2026-05-01T07:59:35.180Zhttps://docs.openclaw.ai/de/tools/image-generation2026-05-01T07:59:35.173Zhttps://docs.openclaw.ai/de/tools2026-05-01T07:59:35.181Zhttps://docs.openclaw.ai/de/tools/kimi-search2026-05-01T07:59:35.172Zhttps://docs.openclaw.ai/de/tools/llm-task2026-05-01T07:59:35.171Zhttps://docs.openclaw.ai/de/tools/lobster2026-05-01T07:59:35.220Zhttps://docs.openclaw.ai/de/tools/loop-detection2026-05-01T07:59:35.219Zhttps://docs.openclaw.ai/de/tools/media-overview2026-05-01T07:59:35.218Zhttps://docs.openclaw.ai/de/tools/minimax-search2026-05-01T07:59:35.212Zhttps://docs.openclaw.ai/de/tools/multi-agent-sandbox-tools2026-05-01T07:59:35.218Zhttps://docs.openclaw.ai/de/tools/music-generation2026-05-01T07:59:35.209Zhttps://docs.openclaw.ai/de/tools/ollama-search2026-05-01T07:59:35.211Zhttps://docs.openclaw.ai/de/tools/pdf2026-05-01T07:59:35.211Zhttps://docs.openclaw.ai/de/tools/perplexity-search2026-05-01T07:59:35.210Zhttps://docs.openclaw.ai/de/tools/plugin2026-05-01T07:59:35.216Zhttps://docs.openclaw.ai/de/tools/reactions2026-05-01T07:59:35.246Zhttps://docs.openclaw.ai/de/tools/searxng-search2026-05-01T07:59:35.244Zhttps://docs.openclaw.ai/de/tools/skills2026-05-01T07:59:35.248Zhttps://docs.openclaw.ai/de/tools/skills-config2026-05-01T07:59:35.243Zhttps://docs.openclaw.ai/de/tools/slash-commands2026-05-01T07:59:35.248Zhttps://docs.openclaw.ai/de/tools/subagents2026-05-01T07:59:35.245Zhttps://docs.openclaw.ai/de/tools/tavily2026-05-01T07:59:35.244Zhttps://docs.openclaw.ai/de/tools/thinking2026-05-01T07:59:35.246Zhttps://docs.openclaw.ai/de/tools/tokenjuice2026-05-01T07:59:35.245Zhttps://docs.openclaw.ai/de/tools/trajectory2026-05-01T07:59:35.247Zhttps://docs.openclaw.ai/de/tools/tts2026-05-01T07:59:35.276Zhttps://docs.openclaw.ai/de/tools/video-generation2026-05-01T07:59:35.272Zhttps://docs.openclaw.ai/de/tools/web2026-05-01T07:59:35.270Zhttps://docs.openclaw.ai/de/tools/web-fetch2026-05-01T07:59:35.273Zhttps://docs.openclaw.ai/de/vps2026-05-01T07:59:35.273Zhttps://docs.openclaw.ai/de/web/control-ui2026-05-01T07:59:35.271Zhttps://docs.openclaw.ai/de/web/dashboard2026-05-01T07:59:35.289Zhttps://docs.openclaw.ai/de/web2026-05-01T07:59:35.272Zhttps://docs.openclaw.ai/de/web/tui2026-05-01T07:59:35.289Zhttps://docs.openclaw.ai/de/web/webchat2026-05-01T07:59:35.318Zhttps://docs.openclaw.ai/debug/node-issue2026-05-01T07:59:35.317Zhttps://docs.openclaw.ai/diagnostics/flags2026-05-01T07:59:35.318Zhttps://docs.openclaw.ai/es/auth-credential-semantics2026-05-01T07:59:35.316Zhttps://docs.openclaw.ai/es/automation/cron-jobs2026-05-01T07:59:35.316Zhttps://docs.openclaw.ai/es/automation/hooks2026-05-01T07:59:35.352Zhttps://docs.openclaw.ai/es/automation2026-05-01T07:59:35.351Zhttps://docs.openclaw.ai/es/automation/standing-orders2026-05-01T07:59:35.342Zhttps://docs.openclaw.ai/es/automation/taskflow2026-05-01T07:59:35.354Zhttps://docs.openclaw.ai/es/automation/tasks2026-05-01T07:59:35.346Zhttps://docs.openclaw.ai/es/channels/bluebubbles2026-05-01T07:59:35.340Zhttps://docs.openclaw.ai/es/channels/broadcast-groups2026-05-01T07:59:35.384Zhttps://docs.openclaw.ai/es/channels/channel-routing2026-05-01T07:59:35.383Zhttps://docs.openclaw.ai/es/channels/discord2026-05-01T07:59:35.410Zhttps://docs.openclaw.ai/es/channels/feishu2026-05-01T07:59:35.409Zhttps://docs.openclaw.ai/es/channels/googlechat2026-05-01T07:59:35.409Zhttps://docs.openclaw.ai/es/channels/group-messages2026-05-01T07:59:35.392Zhttps://docs.openclaw.ai/es/channels/groups2026-05-01T07:59:35.384Zhttps://docs.openclaw.ai/es/channels/imessage2026-05-01T07:59:35.385Zhttps://docs.openclaw.ai/es/channels2026-05-01T07:59:35.382Zhttps://docs.openclaw.ai/es/channels/irc2026-05-01T07:59:35.382Zhttps://docs.openclaw.ai/es/channels/line2026-05-01T07:59:35.453Zhttps://docs.openclaw.ai/es/channels/location2026-05-01T07:59:35.445Zhttps://docs.openclaw.ai/es/channels/matrix2026-05-01T07:59:35.444Zhttps://docs.openclaw.ai/es/channels/matrix-migration2026-05-01T07:59:35.452Zhttps://docs.openclaw.ai/es/channels/matrix-push-rules2026-05-01T07:59:35.449Zhttps://docs.openclaw.ai/es/channels/mattermost2026-05-01T07:59:35.449Zhttps://docs.openclaw.ai/es/channels/msteams2026-05-01T07:59:35.445Zhttps://docs.openclaw.ai/es/channels/nextcloud-talk2026-05-01T07:59:35.443Zhttps://docs.openclaw.ai/es/channels/nostr2026-05-01T07:59:35.443Zhttps://docs.openclaw.ai/es/channels/pairing2026-05-01T07:59:35.442Zhttps://docs.openclaw.ai/es/channels/qa-channel2026-05-01T07:59:35.541Zhttps://docs.openclaw.ai/es/channels/qqbot2026-05-01T07:59:35.483Zhttps://docs.openclaw.ai/es/channels/signal2026-05-01T07:59:35.484Zhttps://docs.openclaw.ai/es/channels/slack2026-05-01T07:59:35.485Zhttps://docs.openclaw.ai/es/channels/synology-chat2026-05-01T07:59:35.482Zhttps://docs.openclaw.ai/es/channels/telegram2026-05-01T07:59:35.476Zhttps://docs.openclaw.ai/es/channels/tlon2026-05-01T07:59:35.483Zhttps://docs.openclaw.ai/es/channels/troubleshooting2026-05-01T07:59:35.476Zhttps://docs.openclaw.ai/es/channels/twitch2026-05-01T07:59:35.475Zhttps://docs.openclaw.ai/es/channels/wechat2026-05-01T07:59:35.474Zhttps://docs.openclaw.ai/es/channels/whatsapp2026-05-01T07:59:35.577Zhttps://docs.openclaw.ai/es/channels/yuanbao2026-05-01T07:59:35.569Zhttps://docs.openclaw.ai/es/channels/zalo2026-05-01T07:59:35.573Zhttps://docs.openclaw.ai/es/channels/zalouser2026-05-01T07:59:35.576Zhttps://docs.openclaw.ai/es/ci2026-05-01T07:59:35.570Zhttps://docs.openclaw.ai/es/cli/acp2026-05-01T07:59:35.575Zhttps://docs.openclaw.ai/es/cli/agent2026-05-01T07:59:35.568Zhttps://docs.openclaw.ai/es/cli/agents2026-05-01T07:59:35.569Zhttps://docs.openclaw.ai/es/cli/approvals2026-05-01T07:59:35.567Zhttps://docs.openclaw.ai/es/cli/backup2026-05-01T07:59:35.567Zhttps://docs.openclaw.ai/es/cli/browser2026-05-01T07:59:35.606Zhttps://docs.openclaw.ai/es/cli/channels2026-05-01T07:59:35.605Zhttps://docs.openclaw.ai/es/cli/clawbot2026-05-01T07:59:35.604Zhttps://docs.openclaw.ai/es/cli/commitments2026-05-01T07:59:35.601Zhttps://docs.openclaw.ai/es/cli/completion2026-05-01T07:59:35.597Zhttps://docs.openclaw.ai/es/cli/config2026-05-01T07:59:35.604Zhttps://docs.openclaw.ai/es/cli/configure2026-05-01T07:59:35.603Zhttps://docs.openclaw.ai/es/cli/cron2026-05-01T07:59:35.596Zhttps://docs.openclaw.ai/es/cli/daemon2026-05-01T07:59:35.595Zhttps://docs.openclaw.ai/es/cli/dashboard2026-05-01T07:59:35.649Zhttps://docs.openclaw.ai/es/cli/devices2026-05-01T07:59:35.642Zhttps://docs.openclaw.ai/es/cli/directory2026-05-01T07:59:35.641Zhttps://docs.openclaw.ai/es/cli/dns2026-05-01T07:59:35.643Zhttps://docs.openclaw.ai/es/cli/docs2026-05-01T07:59:35.626Zhttps://docs.openclaw.ai/es/cli/doctor2026-05-01T07:59:35.644Zhttps://docs.openclaw.ai/es/cli/flows2026-05-01T07:59:35.643Zhttps://docs.openclaw.ai/es/cli/gateway2026-05-01T07:59:35.627Zhttps://docs.openclaw.ai/es/cli/health2026-05-01T07:59:35.625Zhttps://docs.openclaw.ai/es/cli/hooks2026-05-01T07:59:35.625Zhttps://docs.openclaw.ai/es/cli2026-05-01T07:59:35.677Zhttps://docs.openclaw.ai/es/cli/infer2026-05-01T07:59:35.676Zhttps://docs.openclaw.ai/es/cli/logs2026-05-01T07:59:35.673Zhttps://docs.openclaw.ai/es/cli/mcp2026-05-01T07:59:35.675Zhttps://docs.openclaw.ai/es/cli/memory2026-05-01T07:59:35.667Zhttps://docs.openclaw.ai/es/cli/message2026-05-01T07:59:35.676Zhttps://docs.openclaw.ai/es/cli/migrate2026-05-01T07:59:35.668Zhttps://docs.openclaw.ai/es/cli/models2026-05-01T07:59:35.674Zhttps://docs.openclaw.ai/es/cli/node2026-05-01T07:59:35.667Zhttps://docs.openclaw.ai/es/cli/nodes2026-05-01T07:59:35.666Zhttps://docs.openclaw.ai/es/cli/onboard2026-05-01T07:59:35.705Zhttps://docs.openclaw.ai/es/cli/pairing2026-05-01T07:59:35.704Zhttps://docs.openclaw.ai/es/cli/plugins2026-05-01T07:59:35.704Zhttps://docs.openclaw.ai/es/cli/proxy2026-05-01T07:59:35.706Zhttps://docs.openclaw.ai/es/cli/qr2026-05-01T07:59:35.697Zhttps://docs.openclaw.ai/es/cli/reset2026-05-01T07:59:35.701Zhttps://docs.openclaw.ai/es/cli/sandbox2026-05-01T07:59:35.698Zhttps://docs.openclaw.ai/es/cli/secrets2026-05-01T07:59:35.697Zhttps://docs.openclaw.ai/es/cli/security2026-05-01T07:59:35.696Zhttps://docs.openclaw.ai/es/cli/sessions2026-05-01T07:59:35.696Zhttps://docs.openclaw.ai/es/cli/setup2026-05-01T07:59:35.745Zhttps://docs.openclaw.ai/es/cli/skills2026-05-01T07:59:35.746Zhttps://docs.openclaw.ai/es/cli/status2026-05-01T07:59:35.744Zhttps://docs.openclaw.ai/es/cli/system2026-05-01T07:59:35.743Zhttps://docs.openclaw.ai/es/cli/tasks2026-05-01T07:59:35.730Zhttps://docs.openclaw.ai/es/cli/tui2026-05-01T07:59:35.743Zhttps://docs.openclaw.ai/es/cli/uninstall2026-05-01T07:59:35.725Zhttps://docs.openclaw.ai/es/cli/update2026-05-01T07:59:35.726Zhttps://docs.openclaw.ai/es/cli/voicecall2026-05-01T07:59:35.725Zhttps://docs.openclaw.ai/es/cli/webhooks2026-05-01T07:59:35.724Zhttps://docs.openclaw.ai/es/cli/wiki2026-05-01T07:59:35.775Zhttps://docs.openclaw.ai/es/concepts/active-memory2026-05-01T07:59:35.775Zhttps://docs.openclaw.ai/es/concepts/agent2026-05-01T07:59:35.770Zhttps://docs.openclaw.ai/es/concepts/agent-loop2026-05-01T07:59:35.774Zhttps://docs.openclaw.ai/es/concepts/agent-runtimes2026-05-01T07:59:35.773Zhttps://docs.openclaw.ai/es/concepts/agent-workspace2026-05-01T07:59:35.773Zhttps://docs.openclaw.ai/es/concepts/architecture2026-05-01T07:59:35.766Zhttps://docs.openclaw.ai/es/concepts/channel-docking2026-05-01T07:59:35.765Zhttps://docs.openclaw.ai/es/concepts/commitments2026-05-01T07:59:35.765Zhttps://docs.openclaw.ai/es/concepts/compaction2026-05-01T07:59:35.764Zhttps://docs.openclaw.ai/es/concepts/context2026-05-01T07:59:35.803Zhttps://docs.openclaw.ai/es/concepts/context-engine2026-05-01T07:59:35.805Zhttps://docs.openclaw.ai/es/concepts/delegate-architecture2026-05-01T07:59:35.804Zhttps://docs.openclaw.ai/es/concepts/dreaming2026-05-01T07:59:35.804Zhttps://docs.openclaw.ai/es/concepts/experimental-features2026-05-01T07:59:35.795Zhttps://docs.openclaw.ai/es/concepts/features2026-05-01T07:59:35.800Zhttps://docs.openclaw.ai/es/concepts/markdown-formatting2026-05-01T07:59:35.796Zhttps://docs.openclaw.ai/es/concepts/memory2026-05-01T07:59:35.843Zhttps://docs.openclaw.ai/es/concepts/memory-builtin2026-05-01T07:59:35.802Zhttps://docs.openclaw.ai/es/concepts/memory-honcho2026-05-01T07:59:35.795Zhttps://docs.openclaw.ai/es/concepts/memory-qmd2026-05-01T07:59:35.794Zhttps://docs.openclaw.ai/es/concepts/memory-search2026-05-01T07:59:35.845Zhttps://docs.openclaw.ai/es/concepts/messages2026-05-01T07:59:35.846Zhttps://docs.openclaw.ai/es/concepts/model-failover2026-05-01T07:59:35.845Zhttps://docs.openclaw.ai/es/concepts/model-providers2026-05-01T07:59:35.825Zhttps://docs.openclaw.ai/es/concepts/models2026-05-01T07:59:35.832Zhttps://docs.openclaw.ai/es/concepts/multi-agent2026-05-01T07:59:35.826Zhttps://docs.openclaw.ai/es/concepts/oauth2026-05-01T07:59:35.844Zhttps://docs.openclaw.ai/es/concepts/openclaw-sdk2026-05-01T07:59:35.825Zhttps://docs.openclaw.ai/es/concepts/presence2026-05-01T07:59:35.824Zhttps://docs.openclaw.ai/es/concepts/qa-e2e-automation2026-05-01T07:59:35.876Zhttps://docs.openclaw.ai/es/concepts/qa-matrix2026-05-01T07:59:35.869Zhttps://docs.openclaw.ai/es/concepts/queue2026-05-01T07:59:35.868Zhttps://docs.openclaw.ai/es/concepts/queue-steering2026-05-01T07:59:35.875Zhttps://docs.openclaw.ai/es/concepts/retry2026-05-01T07:59:35.875Zhttps://docs.openclaw.ai/es/concepts/session2026-05-01T07:59:35.868Zhttps://docs.openclaw.ai/es/concepts/session-pruning2026-05-01T07:59:35.867Zhttps://docs.openclaw.ai/es/concepts/session-tool2026-05-01T07:59:35.871Zhttps://docs.openclaw.ai/es/concepts/soul2026-05-01T07:59:35.867Zhttps://docs.openclaw.ai/es/concepts/streaming2026-05-01T07:59:35.866Zhttps://docs.openclaw.ai/es/concepts/system-prompt2026-05-01T07:59:35.905Zhttps://docs.openclaw.ai/es/concepts/timezone2026-05-01T07:59:35.903Zhttps://docs.openclaw.ai/es/concepts/typebox2026-05-01T07:59:35.904Zhttps://docs.openclaw.ai/es/concepts/typing-indicators2026-05-01T07:59:35.900Zhttps://docs.openclaw.ai/es/concepts/usage-tracking2026-05-01T07:59:35.902Zhttps://docs.openclaw.ai/es/date-time2026-05-01T07:59:35.903Zhttps://docs.openclaw.ai/es/debug/node-issue2026-05-01T07:59:35.895Zhttps://docs.openclaw.ai/es/diagnostics/flags2026-05-01T07:59:35.896Zhttps://docs.openclaw.ai/es/gateway/authentication2026-05-01T07:59:35.895Zhttps://docs.openclaw.ai/es/gateway/background-process2026-05-01T07:59:35.894Zhttps://docs.openclaw.ai/es/gateway/bonjour2026-05-01T07:59:35.948Zhttps://docs.openclaw.ai/es/gateway/bridge-protocol2026-05-01T07:59:35.934Zhttps://docs.openclaw.ai/es/gateway/cli-backends2026-05-01T07:59:35.933Zhttps://docs.openclaw.ai/es/gateway/config-agents2026-05-01T07:59:35.927Zhttps://docs.openclaw.ai/es/gateway/config-channels2026-05-01T07:59:35.947Zhttps://docs.openclaw.ai/es/gateway/config-tools2026-05-01T07:59:35.935Zhttps://docs.openclaw.ai/es/gateway/configuration2026-05-01T07:59:35.926Zhttps://docs.openclaw.ai/es/gateway/configuration-examples2026-05-01T07:59:35.926Zhttps://docs.openclaw.ai/es/gateway/configuration-reference2026-05-01T07:59:35.936Zhttps://docs.openclaw.ai/es/gateway/diagnostics2026-05-01T07:59:35.925Zhttps://docs.openclaw.ai/es/gateway/discovery2026-05-01T07:59:35.978Zhttps://docs.openclaw.ai/es/gateway/doctor2026-05-01T07:59:35.975Zhttps://docs.openclaw.ai/es/gateway/gateway-lock2026-05-01T07:59:35.973Zhttps://docs.openclaw.ai/es/gateway/health2026-05-01T07:59:35.976Zhttps://docs.openclaw.ai/es/gateway/heartbeat2026-05-01T07:59:35.977Zhttps://docs.openclaw.ai/es/gateway2026-05-01T07:59:35.967Zhttps://docs.openclaw.ai/es/gateway/local-models2026-05-01T07:59:35.976Zhttps://docs.openclaw.ai/es/gateway/logging2026-05-01T07:59:35.968Zhttps://docs.openclaw.ai/es/gateway/multiple-gateways2026-05-01T07:59:35.967Zhttps://docs.openclaw.ai/es/gateway/network-model2026-05-01T07:59:35.966Zhttps://docs.openclaw.ai/es/gateway/openai-http-api2026-05-01T07:59:36.009Zhttps://docs.openclaw.ai/es/gateway/openresponses-http-api2026-05-01T07:59:36.008Zhttps://docs.openclaw.ai/es/gateway/openshell2026-05-01T07:59:36.009Zhttps://docs.openclaw.ai/es/gateway/opentelemetry2026-05-01T07:59:36.001Zhttps://docs.openclaw.ai/es/gateway/pairing2026-05-01T07:59:36.002Zhttps://docs.openclaw.ai/es/gateway/prometheus2026-05-01T07:59:36.000Zhttps://docs.openclaw.ai/es/gateway/protocol2026-05-01T07:59:36.002Zhttps://docs.openclaw.ai/es/gateway/remote2026-05-01T07:59:36.001Zhttps://docs.openclaw.ai/es/gateway/remote-gateway-readme2026-05-01T07:59:36.006Zhttps://docs.openclaw.ai/es/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:36.007Zhttps://docs.openclaw.ai/es/gateway/sandboxing2026-05-01T07:59:36.049Zhttps://docs.openclaw.ai/es/gateway/secrets2026-05-01T07:59:36.052Zhttps://docs.openclaw.ai/es/gateway/secrets-plan-contract2026-05-01T07:59:36.052Zhttps://docs.openclaw.ai/es/gateway/security/audit-checks2026-05-01T07:59:36.038Zhttps://docs.openclaw.ai/es/gateway/security2026-05-01T07:59:36.032Zhttps://docs.openclaw.ai/es/gateway/tailscale2026-05-01T07:59:36.051Zhttps://docs.openclaw.ai/es/gateway/tools-invoke-http-api2026-05-01T07:59:36.033Zhttps://docs.openclaw.ai/es/gateway/troubleshooting2026-05-01T07:59:36.032Zhttps://docs.openclaw.ai/es/gateway/trusted-proxy-auth2026-05-01T07:59:36.033Zhttps://docs.openclaw.ai/es/help/debugging2026-05-01T07:59:36.031Zhttps://docs.openclaw.ai/es/help/environment2026-05-01T07:59:36.086Zhttps://docs.openclaw.ai/es/help/faq2026-05-01T07:59:36.088Zhttps://docs.openclaw.ai/es/help/faq-first-run2026-05-01T07:59:36.087Zhttps://docs.openclaw.ai/es/help/faq-models2026-05-01T07:59:36.076Zhttps://docs.openclaw.ai/es/help/gpt55-codex-agentic-parity2026-05-01T07:59:36.077Zhttps://docs.openclaw.ai/es/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:36.077Zhttps://docs.openclaw.ai/es/help2026-05-01T07:59:36.076Zhttps://docs.openclaw.ai/es/help/scripts2026-05-01T07:59:36.075Zhttps://docs.openclaw.ai/es/help/testing2026-05-01T07:59:36.087Zhttps://docs.openclaw.ai/es/help/testing-live2026-05-01T07:59:36.078Zhttps://docs.openclaw.ai/es/help/troubleshooting2026-05-01T07:59:36.137Zhttps://docs.openclaw.ai/es2026-05-01T07:59:36.116Zhttps://docs.openclaw.ai/es/install/ansible2026-05-01T07:59:36.128Zhttps://docs.openclaw.ai/es/install/azure2026-05-01T07:59:36.136Zhttps://docs.openclaw.ai/es/install/bun2026-05-01T07:59:36.117Zhttps://docs.openclaw.ai/es/install/clawdock2026-05-01T07:59:36.134Zhttps://docs.openclaw.ai/es/install/development-channels2026-05-01T07:59:36.126Zhttps://docs.openclaw.ai/es/install/digitalocean2026-05-01T07:59:36.128Zhttps://docs.openclaw.ai/es/install/docker2026-05-01T07:59:36.129Zhttps://docs.openclaw.ai/es/install/docker-vm-runtime2026-05-01T07:59:36.127Zhttps://docs.openclaw.ai/es/install/exe-dev2026-05-01T07:59:36.193Zhttps://docs.openclaw.ai/es/install/fly2026-05-01T07:59:36.171Zhttps://docs.openclaw.ai/es/install/gcp2026-05-01T07:59:36.170Zhttps://docs.openclaw.ai/es/install/hetzner2026-05-01T07:59:36.168Zhttps://docs.openclaw.ai/es/install/hostinger2026-05-01T07:59:36.193Zhttps://docs.openclaw.ai/es/install2026-05-01T07:59:36.175Zhttps://docs.openclaw.ai/es/install/installer2026-05-01T07:59:36.169Zhttps://docs.openclaw.ai/es/install/kubernetes2026-05-01T07:59:36.171Zhttps://docs.openclaw.ai/es/install/macos-vm2026-05-01T07:59:36.158Zhttps://docs.openclaw.ai/es/install/migrating2026-05-01T07:59:36.225Zhttps://docs.openclaw.ai/es/install/migrating-claude2026-05-01T07:59:36.176Zhttps://docs.openclaw.ai/es/install/migrating-hermes2026-05-01T07:59:36.226Zhttps://docs.openclaw.ai/es/install/nix2026-05-01T07:59:36.232Zhttps://docs.openclaw.ai/es/install/node2026-05-01T07:59:36.228Zhttps://docs.openclaw.ai/es/install/northflank2026-05-01T07:59:36.229Zhttps://docs.openclaw.ai/es/install/oracle2026-05-01T07:59:36.227Zhttps://docs.openclaw.ai/es/install/podman2026-05-01T07:59:36.224Zhttps://docs.openclaw.ai/es/install/railway2026-05-01T07:59:36.222Zhttps://docs.openclaw.ai/es/install/raspberry-pi2026-05-01T07:59:36.224Zhttps://docs.openclaw.ai/es/install/render2026-05-01T07:59:36.228Zhttps://docs.openclaw.ai/es/install/uninstall2026-05-01T07:59:36.258Zhttps://docs.openclaw.ai/es/install/updating2026-05-01T07:59:36.327Zhttps://docs.openclaw.ai/es/logging2026-05-01T07:59:36.269Zhttps://docs.openclaw.ai/es/network2026-05-01T07:59:36.272Zhttps://docs.openclaw.ai/es/nodes/audio2026-05-01T07:59:36.270Zhttps://docs.openclaw.ai/es/nodes/camera2026-05-01T07:59:36.261Zhttps://docs.openclaw.ai/es/nodes/images2026-05-01T07:59:36.269Zhttps://docs.openclaw.ai/es/nodes2026-05-01T07:59:36.260Zhttps://docs.openclaw.ai/es/nodes/location-command2026-05-01T07:59:36.259Zhttps://docs.openclaw.ai/es/nodes/media-understanding2026-05-01T07:59:36.258Zhttps://docs.openclaw.ai/es/nodes/talk2026-05-01T07:59:36.371Zhttps://docs.openclaw.ai/es/nodes/troubleshooting2026-05-01T07:59:36.364Zhttps://docs.openclaw.ai/es/nodes/voicewake2026-05-01T07:59:36.363Zhttps://docs.openclaw.ai/es/pi2026-05-01T07:59:36.364Zhttps://docs.openclaw.ai/es/pi-dev2026-05-01T07:59:36.370Zhttps://docs.openclaw.ai/es/platforms/android2026-05-01T07:59:36.367Zhttps://docs.openclaw.ai/es/platforms2026-05-01T07:59:36.399Zhttps://docs.openclaw.ai/es/platforms/ios2026-05-01T07:59:36.397Zhttps://docs.openclaw.ai/es/platforms/linux2026-05-01T07:59:36.398Zhttps://docs.openclaw.ai/es/platforms/mac/bundled-gateway2026-05-01T07:59:36.394Zhttps://docs.openclaw.ai/es/platforms/mac/canvas2026-05-01T07:59:36.388Zhttps://docs.openclaw.ai/es/platforms/mac/child-process2026-05-01T07:59:36.397Zhttps://docs.openclaw.ai/es/platforms/mac/dev-setup2026-05-01T07:59:36.396Zhttps://docs.openclaw.ai/es/platforms/mac/health2026-05-01T07:59:36.398Zhttps://docs.openclaw.ai/es/platforms/mac/icon2026-05-01T07:59:36.395Zhttps://docs.openclaw.ai/es/platforms/mac/logging2026-05-01T07:59:36.388Zhttps://docs.openclaw.ai/es/platforms/mac/menu-bar2026-05-01T07:59:36.429Zhttps://docs.openclaw.ai/es/platforms/mac/peekaboo2026-05-01T07:59:36.428Zhttps://docs.openclaw.ai/es/platforms/mac/permissions2026-05-01T07:59:36.419Zhttps://docs.openclaw.ai/es/platforms/mac/remote2026-05-01T07:59:36.427Zhttps://docs.openclaw.ai/es/platforms/mac/signing2026-05-01T07:59:36.427Zhttps://docs.openclaw.ai/es/platforms/mac/skills2026-05-01T07:59:36.421Zhttps://docs.openclaw.ai/es/platforms/mac/voice-overlay2026-05-01T07:59:36.421Zhttps://docs.openclaw.ai/es/platforms/mac/voicewake2026-05-01T07:59:36.424Zhttps://docs.openclaw.ai/es/platforms/mac/webchat2026-05-01T07:59:36.420Zhttps://docs.openclaw.ai/es/platforms/mac/xpc2026-05-01T07:59:36.419Zhttps://docs.openclaw.ai/es/platforms/macos2026-05-01T07:59:36.470Zhttps://docs.openclaw.ai/es/platforms/windows2026-05-01T07:59:36.467Zhttps://docs.openclaw.ai/es/plugins/architecture2026-05-01T07:59:36.462Zhttps://docs.openclaw.ai/es/plugins/architecture-internals2026-05-01T07:59:36.466Zhttps://docs.openclaw.ai/es/plugins/building-plugins2026-05-01T07:59:36.462Zhttps://docs.openclaw.ai/es/plugins/bundles2026-05-01T07:59:36.461Zhttps://docs.openclaw.ai/es/plugins/codex-computer-use2026-05-01T07:59:36.503Zhttps://docs.openclaw.ai/es/plugins/codex-harness2026-05-01T07:59:36.502Zhttps://docs.openclaw.ai/es/plugins/community2026-05-01T07:59:36.494Zhttps://docs.openclaw.ai/es/plugins/compatibility2026-05-01T07:59:36.493Zhttps://docs.openclaw.ai/es/plugins/google-meet2026-05-01T07:59:36.502Zhttps://docs.openclaw.ai/es/plugins/hooks2026-05-01T07:59:36.493Zhttps://docs.openclaw.ai/es/plugins/manifest2026-05-01T07:59:36.492Zhttps://docs.openclaw.ai/es/plugins/memory-lancedb2026-05-01T07:59:36.494Zhttps://docs.openclaw.ai/es/plugins/memory-wiki2026-05-01T07:59:36.500Zhttps://docs.openclaw.ai/es/plugins/message-presentation2026-05-01T07:59:36.500Zhttps://docs.openclaw.ai/es/plugins/sdk-agent-harness2026-05-01T07:59:36.537Zhttps://docs.openclaw.ai/es/plugins/sdk-channel-plugins2026-05-01T07:59:36.537Zhttps://docs.openclaw.ai/es/plugins/sdk-channel-turn2026-05-01T07:59:36.535Zhttps://docs.openclaw.ai/es/plugins/sdk-entrypoints2026-05-01T07:59:36.519Zhttps://docs.openclaw.ai/es/plugins/sdk-migration2026-05-01T07:59:36.535Zhttps://docs.openclaw.ai/es/plugins/sdk-overview2026-05-01T07:59:36.536Zhttps://docs.openclaw.ai/es/plugins/sdk-provider-plugins2026-05-01T07:59:36.527Zhttps://docs.openclaw.ai/es/plugins/sdk-runtime2026-05-01T07:59:36.527Zhttps://docs.openclaw.ai/es/plugins/sdk-setup2026-05-01T07:59:36.536Zhttps://docs.openclaw.ai/es/plugins/sdk-subpaths2026-05-01T07:59:36.528Zhttps://docs.openclaw.ai/es/plugins/sdk-testing2026-05-01T07:59:36.577Zhttps://docs.openclaw.ai/es/plugins/skill-workshop2026-05-01T07:59:36.568Zhttps://docs.openclaw.ai/es/plugins/voice-call2026-05-01T07:59:36.567Zhttps://docs.openclaw.ai/es/plugins/webhooks2026-05-01T07:59:36.567Zhttps://docs.openclaw.ai/es/plugins/zalouser2026-05-01T07:59:36.571Zhttps://docs.openclaw.ai/es/prose2026-05-01T07:59:36.573Zhttps://docs.openclaw.ai/es/providers/alibaba2026-05-01T07:59:36.574Zhttps://docs.openclaw.ai/es/providers/anthropic2026-05-01T07:59:36.575Zhttps://docs.openclaw.ai/es/providers/arcee2026-05-01T07:59:36.576Zhttps://docs.openclaw.ai/es/providers/azure-speech2026-05-01T07:59:36.566Zhttps://docs.openclaw.ai/es/providers/bedrock2026-05-01T07:59:36.605Zhttps://docs.openclaw.ai/es/providers/bedrock-mantle2026-05-01T07:59:36.606Zhttps://docs.openclaw.ai/es/providers/chutes2026-05-01T07:59:36.597Zhttps://docs.openclaw.ai/es/providers/claude-max-api-proxy2026-05-01T07:59:36.605Zhttps://docs.openclaw.ai/es/providers/cloudflare-ai-gateway2026-05-01T07:59:36.603Zhttps://docs.openclaw.ai/es/providers/comfy2026-05-01T07:59:36.598Zhttps://docs.openclaw.ai/es/providers/deepgram2026-05-01T07:59:36.598Zhttps://docs.openclaw.ai/es/providers/deepinfra2026-05-01T07:59:36.597Zhttps://docs.openclaw.ai/es/providers/deepseek2026-05-01T07:59:36.599Zhttps://docs.openclaw.ai/es/providers/elevenlabs2026-05-01T07:59:36.637Zhttps://docs.openclaw.ai/es/providers/fal2026-05-01T07:59:36.636Zhttps://docs.openclaw.ai/es/providers/fireworks2026-05-01T07:59:36.634Zhttps://docs.openclaw.ai/es/providers/github-copilot2026-05-01T07:59:36.625Zhttps://docs.openclaw.ai/es/providers/glm2026-05-01T07:59:36.635Zhttps://docs.openclaw.ai/es/providers/google2026-05-01T07:59:36.627Zhttps://docs.openclaw.ai/es/providers/gradium2026-05-01T07:59:36.631Zhttps://docs.openclaw.ai/es/providers/groq2026-05-01T07:59:36.633Zhttps://docs.openclaw.ai/es/providers/huggingface2026-05-01T07:59:36.626Zhttps://docs.openclaw.ai/es/providers2026-05-01T07:59:36.625Zhttps://docs.openclaw.ai/es/providers/inferrs2026-05-01T07:59:36.676Zhttps://docs.openclaw.ai/es/providers/inworld2026-05-01T07:59:36.675Zhttps://docs.openclaw.ai/es/providers/kilocode2026-05-01T07:59:36.673Zhttps://docs.openclaw.ai/es/providers/litellm2026-05-01T07:59:36.675Zhttps://docs.openclaw.ai/es/providers/lmstudio2026-05-01T07:59:36.672Zhttps://docs.openclaw.ai/es/providers/minimax2026-05-01T07:59:36.670Zhttps://docs.openclaw.ai/es/providers/mistral2026-05-01T07:59:36.673Zhttps://docs.openclaw.ai/es/providers/models2026-05-01T07:59:36.672Zhttps://docs.openclaw.ai/es/providers/moonshot2026-05-01T07:59:36.671Zhttps://docs.openclaw.ai/es/providers/nvidia2026-05-01T07:59:36.670Zhttps://docs.openclaw.ai/es/providers/ollama2026-05-01T07:59:36.708Zhttps://docs.openclaw.ai/es/providers/openai2026-05-01T07:59:36.706Zhttps://docs.openclaw.ai/es/providers/opencode2026-05-01T07:59:36.702Zhttps://docs.openclaw.ai/es/providers/opencode-go2026-05-01T07:59:36.703Zhttps://docs.openclaw.ai/es/providers/openrouter2026-05-01T07:59:36.705Zhttps://docs.openclaw.ai/es/providers/perplexity-provider2026-05-01T07:59:36.704Zhttps://docs.openclaw.ai/es/providers/qianfan2026-05-01T07:59:36.702Zhttps://docs.openclaw.ai/es/providers/qwen2026-05-01T07:59:36.698Zhttps://docs.openclaw.ai/es/providers/runway2026-05-01T07:59:36.703Zhttps://docs.openclaw.ai/es/providers/sglang2026-05-01T07:59:36.739Zhttps://docs.openclaw.ai/es/providers/stepfun2026-05-01T07:59:36.736Zhttps://docs.openclaw.ai/es/providers/synthetic2026-05-01T07:59:36.735Zhttps://docs.openclaw.ai/es/providers/tencent2026-05-01T07:59:36.733Zhttps://docs.openclaw.ai/es/providers/together2026-05-01T07:59:36.733Zhttps://docs.openclaw.ai/es/providers/venice2026-05-01T07:59:36.736Zhttps://docs.openclaw.ai/es/providers/vercel-ai-gateway2026-05-01T07:59:36.726Zhttps://docs.openclaw.ai/es/providers/vllm2026-05-01T07:59:36.737Zhttps://docs.openclaw.ai/es/providers/volcengine2026-05-01T07:59:36.735Zhttps://docs.openclaw.ai/es/providers/vydra2026-05-01T07:59:36.734Zhttps://docs.openclaw.ai/es/providers/xai2026-05-01T07:59:36.782Zhttps://docs.openclaw.ai/es/providers/xiaomi2026-05-01T07:59:36.776Zhttps://docs.openclaw.ai/es/providers/zai2026-05-01T07:59:36.778Zhttps://docs.openclaw.ai/es/reference/AGENTS.default2026-05-01T07:59:36.779Zhttps://docs.openclaw.ai/es/reference/RELEASING2026-05-01T07:59:36.777Zhttps://docs.openclaw.ai/es/reference/api-usage-costs2026-05-01T07:59:36.775Zhttps://docs.openclaw.ai/es/reference/credits2026-05-01T07:59:36.776Zhttps://docs.openclaw.ai/es/reference/device-models2026-05-01T07:59:36.771Zhttps://docs.openclaw.ai/es/reference/full-release-validation2026-05-01T07:59:36.774Zhttps://docs.openclaw.ai/es/reference/memory-config2026-05-01T07:59:36.815Zhttps://docs.openclaw.ai/es/reference/openclaw-sdk-api-design2026-05-01T07:59:36.812Zhttps://docs.openclaw.ai/es/reference/prompt-caching2026-05-01T07:59:36.813Zhttps://docs.openclaw.ai/es/reference/rich-output-protocol2026-05-01T07:59:36.810Zhttps://docs.openclaw.ai/es/reference/rpc2026-05-01T07:59:36.802Zhttps://docs.openclaw.ai/es/reference/secretref-credential-surface2026-05-01T07:59:36.806Zhttps://docs.openclaw.ai/es/reference/session-management-compaction2026-05-01T07:59:36.805Zhttps://docs.openclaw.ai/es/reference/templates/AGENTS2026-05-01T07:59:36.802Zhttps://docs.openclaw.ai/es/reference/templates/BOOT2026-05-01T07:59:36.807Zhttps://docs.openclaw.ai/es/reference/templates/BOOTSTRAP2026-05-01T07:59:36.845Zhttps://docs.openclaw.ai/es/reference/templates/HEARTBEAT2026-05-01T07:59:36.841Zhttps://docs.openclaw.ai/es/reference/templates/IDENTITY2026-05-01T07:59:36.839Zhttps://docs.openclaw.ai/es/reference/templates/SOUL2026-05-01T07:59:36.840Zhttps://docs.openclaw.ai/es/reference/templates/TOOLS2026-05-01T07:59:36.839Zhttps://docs.openclaw.ai/es/reference/templates/USER2026-05-01T07:59:36.838Zhttps://docs.openclaw.ai/es/reference/test2026-05-01T07:59:36.887Zhttps://docs.openclaw.ai/es/reference/token-use2026-05-01T07:59:36.884Zhttps://docs.openclaw.ai/es/reference/transcript-hygiene2026-05-01T07:59:36.883Zhttps://docs.openclaw.ai/es/reference/wizard2026-05-01T07:59:36.885Zhttps://docs.openclaw.ai/es/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:36.883Zhttps://docs.openclaw.ai/es/security/THREAT-MODEL-ATLAS2026-05-01T07:59:36.882Zhttps://docs.openclaw.ai/es/security/formal-verification2026-05-01T07:59:36.877Zhttps://docs.openclaw.ai/es/security/network-proxy2026-05-01T07:59:36.882Zhttps://docs.openclaw.ai/es/start/bootstrapping2026-05-01T07:59:36.881Zhttps://docs.openclaw.ai/es/start/docs-directory2026-05-01T07:59:36.881Zhttps://docs.openclaw.ai/es/start/getting-started2026-05-01T07:59:36.919Zhttps://docs.openclaw.ai/es/start/hubs2026-05-01T07:59:36.915Zhttps://docs.openclaw.ai/es/start/lore2026-05-01T07:59:36.915Zhttps://docs.openclaw.ai/es/start/onboarding2026-05-01T07:59:36.914Zhttps://docs.openclaw.ai/es/start/onboarding-overview2026-05-01T07:59:36.913Zhttps://docs.openclaw.ai/es/start/openclaw2026-05-01T07:59:36.912Zhttps://docs.openclaw.ai/es/start/setup2026-05-01T07:59:36.911Zhttps://docs.openclaw.ai/es/start/showcase2026-04-24T17:33:05.041Zhttps://docs.openclaw.ai/es/start/wizard2026-05-01T07:59:36.950Zhttps://docs.openclaw.ai/es/start/wizard-cli-automation2026-05-01T07:59:36.914Zhttps://docs.openclaw.ai/es/start/wizard-cli-reference2026-05-01T07:59:36.951Zhttps://docs.openclaw.ai/es/tools/acp-agents2026-05-01T07:59:36.948Zhttps://docs.openclaw.ai/es/tools/acp-agents-setup2026-05-01T07:59:36.949Zhttps://docs.openclaw.ai/es/tools/agent-send2026-05-01T07:59:36.949Zhttps://docs.openclaw.ai/es/tools/apply-patch2026-05-01T07:59:36.942Zhttps://docs.openclaw.ai/es/tools/brave-search2026-05-01T07:59:36.947Zhttps://docs.openclaw.ai/es/tools/browser2026-05-01T07:59:36.994Zhttps://docs.openclaw.ai/es/tools/browser-control2026-05-01T07:59:36.946Zhttps://docs.openclaw.ai/es/tools/browser-linux-troubleshooting2026-05-01T07:59:36.941Zhttps://docs.openclaw.ai/es/tools/browser-login2026-05-01T07:59:36.994Zhttps://docs.openclaw.ai/es/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:36.993Zhttps://docs.openclaw.ai/es/tools/btw2026-05-01T07:59:36.990Zhttps://docs.openclaw.ai/es/tools/clawhub2026-05-01T07:59:36.987Zhttps://docs.openclaw.ai/es/tools/code-execution2026-05-01T07:59:36.986Zhttps://docs.openclaw.ai/es/tools/creating-skills2026-05-01T07:59:36.985Zhttps://docs.openclaw.ai/es/tools/diffs2026-05-01T07:59:36.986Zhttps://docs.openclaw.ai/es/tools/duckduckgo-search2026-05-01T07:59:36.984Zhttps://docs.openclaw.ai/es/tools/elevated2026-05-01T07:59:37.026Zhttps://docs.openclaw.ai/es/tools/exa-search2026-05-01T07:59:37.020Zhttps://docs.openclaw.ai/es/tools/exec2026-05-01T07:59:37.022Zhttps://docs.openclaw.ai/es/tools/exec-approvals2026-05-01T07:59:37.023Zhttps://docs.openclaw.ai/es/tools/exec-approvals-advanced2026-05-01T07:59:37.019Zhttps://docs.openclaw.ai/es/tools/firecrawl2026-05-01T07:59:37.020Zhttps://docs.openclaw.ai/es/tools/gemini-search2026-05-01T07:59:37.013Zhttps://docs.openclaw.ai/es/tools/grok-search2026-05-01T07:59:37.012Zhttps://docs.openclaw.ai/es/tools/image-generation2026-05-01T07:59:37.021Zhttps://docs.openclaw.ai/es/tools2026-05-01T07:59:37.012Zhttps://docs.openclaw.ai/es/tools/kimi-search2026-05-01T07:59:37.089Zhttps://docs.openclaw.ai/es/tools/llm-task2026-05-01T07:59:37.079Zhttps://docs.openclaw.ai/es/tools/lobster2026-05-01T07:59:37.079Zhttps://docs.openclaw.ai/es/tools/loop-detection2026-05-01T07:59:37.084Zhttps://docs.openclaw.ai/es/tools/media-overview2026-05-01T07:59:37.088Zhttps://docs.openclaw.ai/es/tools/minimax-search2026-05-01T07:59:37.087Zhttps://docs.openclaw.ai/es/tools/multi-agent-sandbox-tools2026-05-01T07:59:37.088Zhttps://docs.openclaw.ai/es/tools/music-generation2026-05-01T07:59:37.087Zhttps://docs.openclaw.ai/es/tools/ollama-search2026-05-01T07:59:37.080Zhttps://docs.openclaw.ai/es/tools/pdf2026-05-01T07:59:37.078Zhttps://docs.openclaw.ai/es/tools/perplexity-search2026-05-01T07:59:37.121Zhttps://docs.openclaw.ai/es/tools/plugin2026-05-01T07:59:37.114Zhttps://docs.openclaw.ai/es/tools/reactions2026-05-01T07:59:37.114Zhttps://docs.openclaw.ai/es/tools/searxng-search2026-05-01T07:59:37.115Zhttps://docs.openclaw.ai/es/tools/skills2026-05-01T07:59:37.116Zhttps://docs.openclaw.ai/es/tools/skills-config2026-05-01T07:59:37.108Zhttps://docs.openclaw.ai/es/tools/slash-commands2026-05-01T07:59:37.107Zhttps://docs.openclaw.ai/es/tools/subagents2026-05-01T07:59:37.117Zhttps://docs.openclaw.ai/es/tools/tavily2026-05-01T07:59:37.117Zhttps://docs.openclaw.ai/es/tools/thinking2026-05-01T07:59:37.107Zhttps://docs.openclaw.ai/es/tools/tokenjuice2026-05-01T07:59:37.153Zhttps://docs.openclaw.ai/es/tools/trajectory2026-05-01T07:59:37.140Zhttps://docs.openclaw.ai/es/tools/tts2026-05-01T07:59:37.149Zhttps://docs.openclaw.ai/es/tools/video-generation2026-05-01T07:59:37.148Zhttps://docs.openclaw.ai/es/tools/web2026-05-01T07:59:37.146Zhttps://docs.openclaw.ai/es/tools/web-fetch2026-05-01T07:59:37.139Zhttps://docs.openclaw.ai/es/vps2026-05-01T07:59:37.147Zhttps://docs.openclaw.ai/es/web/control-ui2026-05-01T07:59:37.150Zhttps://docs.openclaw.ai/es/web/dashboard2026-05-01T07:59:37.139Zhttps://docs.openclaw.ai/es/web2026-05-01T07:59:37.191Zhttps://docs.openclaw.ai/es/web/tui2026-05-01T07:59:37.183Zhttps://docs.openclaw.ai/es/web/webchat2026-05-01T07:59:37.183Zhttps://docs.openclaw.ai/fr/auth-credential-semantics2026-05-01T07:59:38.915Zhttps://docs.openclaw.ai/fr/automation/cron-jobs2026-05-01T07:59:38.904Zhttps://docs.openclaw.ai/fr/automation/hooks2026-05-01T07:59:38.901Zhttps://docs.openclaw.ai/fr/automation2026-05-01T07:59:38.895Zhttps://docs.openclaw.ai/fr/automation/standing-orders2026-05-01T07:59:38.894Zhttps://docs.openclaw.ai/fr/automation/taskflow2026-05-01T07:59:38.947Zhttps://docs.openclaw.ai/fr/automation/tasks2026-05-01T07:59:38.946Zhttps://docs.openclaw.ai/fr/channels/bluebubbles2026-05-01T07:59:38.944Zhttps://docs.openclaw.ai/fr/channels/broadcast-groups2026-05-01T07:59:38.939Zhttps://docs.openclaw.ai/fr/channels/channel-routing2026-05-01T07:59:38.940Zhttps://docs.openclaw.ai/fr/channels/discord2026-05-01T07:59:38.938Zhttps://docs.openclaw.ai/fr/channels/feishu2026-05-01T07:59:38.938Zhttps://docs.openclaw.ai/fr/channels/googlechat2026-05-01T07:59:38.979Zhttps://docs.openclaw.ai/fr/channels/group-messages2026-05-01T07:59:38.973Zhttps://docs.openclaw.ai/fr/channels/groups2026-05-01T07:59:38.972Zhttps://docs.openclaw.ai/fr/channels/imessage2026-05-01T07:59:38.975Zhttps://docs.openclaw.ai/fr/channels2026-05-01T07:59:38.976Zhttps://docs.openclaw.ai/fr/channels/irc2026-05-01T07:59:38.973Zhttps://docs.openclaw.ai/fr/channels/line2026-05-01T07:59:38.966Zhttps://docs.openclaw.ai/fr/channels/location2026-05-01T07:59:38.966Zhttps://docs.openclaw.ai/fr/channels/matrix2026-05-01T07:59:39.024Zhttps://docs.openclaw.ai/fr/channels/matrix-migration2026-05-01T07:59:38.975Zhttps://docs.openclaw.ai/fr/channels/matrix-push-rules2026-05-01T07:59:38.965Zhttps://docs.openclaw.ai/fr/channels/mattermost2026-05-01T07:59:39.007Zhttps://docs.openclaw.ai/fr/channels/msteams2026-05-01T07:59:38.998Zhttps://docs.openclaw.ai/fr/channels/nextcloud-talk2026-05-01T07:59:38.997Zhttps://docs.openclaw.ai/fr/channels/nostr2026-05-01T07:59:39.005Zhttps://docs.openclaw.ai/fr/channels/pairing2026-05-01T07:59:39.011Zhttps://docs.openclaw.ai/fr/channels/qa-channel2026-05-01T07:59:39.002Zhttps://docs.openclaw.ai/fr/channels/qqbot2026-05-01T07:59:39.022Zhttps://docs.openclaw.ai/fr/channels/signal2026-05-01T07:59:39.023Zhttps://docs.openclaw.ai/fr/channels/slack2026-05-01T07:59:39.008Zhttps://docs.openclaw.ai/fr/channels/synology-chat2026-05-01T07:59:39.054Zhttps://docs.openclaw.ai/fr/channels/telegram2026-05-01T07:59:39.045Zhttps://docs.openclaw.ai/fr/channels/tlon2026-05-01T07:59:39.045Zhttps://docs.openclaw.ai/fr/channels/troubleshooting2026-05-01T07:59:39.053Zhttps://docs.openclaw.ai/fr/channels/twitch2026-05-01T07:59:39.044Zhttps://docs.openclaw.ai/fr/channels/wechat2026-05-01T07:59:39.051Zhttps://docs.openclaw.ai/fr/channels/whatsapp2026-05-01T07:59:39.055Zhttps://docs.openclaw.ai/fr/channels/yuanbao2026-05-01T07:59:39.044Zhttps://docs.openclaw.ai/fr/channels/zalo2026-05-01T07:59:39.053Zhttps://docs.openclaw.ai/fr/channels/zalouser2026-05-01T07:59:39.052Zhttps://docs.openclaw.ai/fr/ci2026-05-01T07:59:39.085Zhttps://docs.openclaw.ai/fr/cli/acp2026-05-01T07:59:39.084Zhttps://docs.openclaw.ai/fr/cli/agent2026-05-01T07:59:39.083Zhttps://docs.openclaw.ai/fr/cli/agents2026-05-01T07:59:39.082Zhttps://docs.openclaw.ai/fr/cli/approvals2026-05-01T07:59:39.083Zhttps://docs.openclaw.ai/fr/cli/backup2026-05-01T07:59:39.079Zhttps://docs.openclaw.ai/fr/cli/browser2026-05-01T07:59:39.074Zhttps://docs.openclaw.ai/fr/cli/channels2026-05-01T07:59:39.075Zhttps://docs.openclaw.ai/fr/cli/clawbot2026-05-01T07:59:39.074Zhttps://docs.openclaw.ai/fr/cli/commitments2026-05-01T07:59:39.073Zhttps://docs.openclaw.ai/fr/cli/completion2026-05-01T07:59:39.126Zhttps://docs.openclaw.ai/fr/cli/config2026-05-01T07:59:39.113Zhttps://docs.openclaw.ai/fr/cli/configure2026-05-01T07:59:39.125Zhttps://docs.openclaw.ai/fr/cli/cron2026-05-01T07:59:39.106Zhttps://docs.openclaw.ai/fr/cli/daemon2026-05-01T07:59:39.111Zhttps://docs.openclaw.ai/fr/cli/dashboard2026-05-01T07:59:39.105Zhttps://docs.openclaw.ai/fr/cli/devices2026-05-01T07:59:39.104Zhttps://docs.openclaw.ai/fr/cli/directory2026-05-01T07:59:39.112Zhttps://docs.openclaw.ai/fr/cli/dns2026-05-01T07:59:39.104Zhttps://docs.openclaw.ai/fr/cli/docs2026-05-01T07:59:39.155Zhttps://docs.openclaw.ai/fr/cli/doctor2026-05-01T07:59:39.155Zhttps://docs.openclaw.ai/fr/cli/flows2026-05-01T07:59:39.151Zhttps://docs.openclaw.ai/fr/cli/gateway2026-05-01T07:59:39.154Zhttps://docs.openclaw.ai/fr/cli/health2026-05-01T07:59:39.153Zhttps://docs.openclaw.ai/fr/cli/hooks2026-05-01T07:59:39.145Zhttps://docs.openclaw.ai/fr/cli2026-05-01T07:59:39.154Zhttps://docs.openclaw.ai/fr/cli/infer2026-05-01T07:59:39.153Zhttps://docs.openclaw.ai/fr/cli/logs2026-05-01T07:59:39.145Zhttps://docs.openclaw.ai/fr/cli/mcp2026-05-01T07:59:39.144Zhttps://docs.openclaw.ai/fr/cli/memory2026-05-01T07:59:39.186Zhttps://docs.openclaw.ai/fr/cli/message2026-05-01T07:59:39.185Zhttps://docs.openclaw.ai/fr/cli/migrate2026-05-01T07:59:39.175Zhttps://docs.openclaw.ai/fr/cli/models2026-05-01T07:59:39.185Zhttps://docs.openclaw.ai/fr/cli/node2026-05-01T07:59:39.176Zhttps://docs.openclaw.ai/fr/cli/nodes2026-05-01T07:59:39.184Zhttps://docs.openclaw.ai/fr/cli/onboard2026-05-01T07:59:39.181Zhttps://docs.openclaw.ai/fr/cli/pairing2026-05-01T07:59:39.176Zhttps://docs.openclaw.ai/fr/cli/plugins2026-05-01T07:59:39.183Zhttps://docs.openclaw.ai/fr/cli/proxy2026-05-01T07:59:39.174Zhttps://docs.openclaw.ai/fr/cli/qr2026-05-01T07:59:39.239Zhttps://docs.openclaw.ai/fr/cli/reset2026-05-01T07:59:39.238Zhttps://docs.openclaw.ai/fr/cli/sandbox2026-05-01T07:59:39.237Zhttps://docs.openclaw.ai/fr/cli/secrets2026-05-01T07:59:39.236Zhttps://docs.openclaw.ai/fr/cli/security2026-05-01T07:59:39.229Zhttps://docs.openclaw.ai/fr/cli/sessions2026-05-01T07:59:39.234Zhttps://docs.openclaw.ai/fr/cli/setup2026-05-01T07:59:39.230Zhttps://docs.openclaw.ai/fr/cli/skills2026-05-01T07:59:39.230Zhttps://docs.openclaw.ai/fr/cli/status2026-05-01T07:59:39.237Zhttps://docs.openclaw.ai/fr/cli/system2026-05-01T07:59:39.229Zhttps://docs.openclaw.ai/fr/cli/tasks2026-05-01T07:59:39.268Zhttps://docs.openclaw.ai/fr/cli/tui2026-05-01T07:59:39.267Zhttps://docs.openclaw.ai/fr/cli/uninstall2026-05-01T07:59:39.267Zhttps://docs.openclaw.ai/fr/cli/update2026-05-01T07:59:39.266Zhttps://docs.openclaw.ai/fr/cli/voicecall2026-05-01T07:59:39.260Zhttps://docs.openclaw.ai/fr/cli/webhooks2026-05-01T07:59:39.264Zhttps://docs.openclaw.ai/fr/cli/wiki2026-05-01T07:59:39.261Zhttps://docs.openclaw.ai/fr/concepts/active-memory2026-05-01T07:59:39.260Zhttps://docs.openclaw.ai/fr/concepts/agent2026-05-01T07:59:39.307Zhttps://docs.openclaw.ai/fr/concepts/agent-loop2026-05-01T07:59:39.259Zhttps://docs.openclaw.ai/fr/concepts/agent-runtimes2026-05-01T07:59:39.258Zhttps://docs.openclaw.ai/fr/concepts/agent-workspace2026-05-01T07:59:39.308Zhttps://docs.openclaw.ai/fr/concepts/architecture2026-05-01T07:59:39.307Zhttps://docs.openclaw.ai/fr/concepts/channel-docking2026-05-01T07:59:39.289Zhttps://docs.openclaw.ai/fr/concepts/commitments2026-05-01T07:59:39.289Zhttps://docs.openclaw.ai/fr/concepts/compaction2026-05-01T07:59:39.290Zhttps://docs.openclaw.ai/fr/concepts/context2026-05-01T07:59:39.292Zhttps://docs.openclaw.ai/fr/concepts/context-engine2026-05-01T07:59:39.291Zhttps://docs.openclaw.ai/fr/concepts/delegate-architecture2026-05-01T07:59:39.290Zhttps://docs.openclaw.ai/fr/concepts/dreaming2026-05-01T07:59:39.295Zhttps://docs.openclaw.ai/fr/concepts/experimental-features2026-05-01T07:59:39.339Zhttps://docs.openclaw.ai/fr/concepts/features2026-05-01T07:59:39.337Zhttps://docs.openclaw.ai/fr/concepts/markdown-formatting2026-05-01T07:59:39.338Zhttps://docs.openclaw.ai/fr/concepts/memory2026-05-01T07:59:39.337Zhttps://docs.openclaw.ai/fr/concepts/memory-builtin2026-05-01T07:59:39.335Zhttps://docs.openclaw.ai/fr/concepts/memory-honcho2026-05-01T07:59:39.339Zhttps://docs.openclaw.ai/fr/concepts/memory-qmd2026-05-01T07:59:39.329Zhttps://docs.openclaw.ai/fr/concepts/memory-search2026-05-01T07:59:39.329Zhttps://docs.openclaw.ai/fr/concepts/messages2026-05-01T07:59:39.330Zhttps://docs.openclaw.ai/fr/concepts/model-failover2026-05-01T07:59:39.328Zhttps://docs.openclaw.ai/fr/concepts/model-providers2026-05-01T07:59:39.370Zhttps://docs.openclaw.ai/fr/concepts/models2026-05-01T07:59:39.368Zhttps://docs.openclaw.ai/fr/concepts/multi-agent2026-05-01T07:59:39.367Zhttps://docs.openclaw.ai/fr/concepts/oauth2026-05-01T07:59:39.369Zhttps://docs.openclaw.ai/fr/concepts/openclaw-sdk2026-05-01T07:59:39.369Zhttps://docs.openclaw.ai/fr/concepts/presence2026-05-01T07:59:39.366Zhttps://docs.openclaw.ai/fr/concepts/qa-e2e-automation2026-05-01T07:59:39.360Zhttps://docs.openclaw.ai/fr/concepts/qa-matrix2026-05-01T07:59:39.360Zhttps://docs.openclaw.ai/fr/concepts/queue2026-05-01T07:59:39.359Zhttps://docs.openclaw.ai/fr/concepts/queue-steering2026-05-01T07:59:39.359Zhttps://docs.openclaw.ai/fr/concepts/retry2026-05-01T07:59:39.410Zhttps://docs.openclaw.ai/fr/concepts/session2026-05-01T07:59:39.390Zhttps://docs.openclaw.ai/fr/concepts/session-pruning2026-05-01T07:59:39.398Zhttps://docs.openclaw.ai/fr/concepts/session-tool2026-05-01T07:59:39.396Zhttps://docs.openclaw.ai/fr/concepts/soul2026-05-01T07:59:39.397Zhttps://docs.openclaw.ai/fr/concepts/streaming2026-05-01T07:59:39.395Zhttps://docs.openclaw.ai/fr/concepts/system-prompt2026-05-01T07:59:39.396Zhttps://docs.openclaw.ai/fr/concepts/timezone2026-05-01T07:59:39.389Zhttps://docs.openclaw.ai/fr/concepts/typebox2026-05-01T07:59:39.388Zhttps://docs.openclaw.ai/fr/concepts/typing-indicators2026-05-01T07:59:39.388Zhttps://docs.openclaw.ai/fr/concepts/usage-tracking2026-05-01T07:59:39.439Zhttps://docs.openclaw.ai/fr/date-time2026-05-01T07:59:39.434Zhttps://docs.openclaw.ai/fr/debug/node-issue2026-05-01T07:59:39.437Zhttps://docs.openclaw.ai/fr/diagnostics/flags2026-05-01T07:59:39.438Zhttps://docs.openclaw.ai/fr/gateway/authentication2026-05-01T07:59:39.436Zhttps://docs.openclaw.ai/fr/gateway/background-process2026-05-01T07:59:39.436Zhttps://docs.openclaw.ai/fr/gateway/bonjour2026-05-01T07:59:39.437Zhttps://docs.openclaw.ai/fr/gateway/bridge-protocol2026-05-01T07:59:39.429Zhttps://docs.openclaw.ai/fr/gateway/cli-backends2026-05-01T07:59:39.428Zhttps://docs.openclaw.ai/fr/gateway/config-agents2026-05-01T07:59:39.428Zhttps://docs.openclaw.ai/fr/gateway/config-channels2026-05-01T07:59:39.471Zhttps://docs.openclaw.ai/fr/gateway/config-tools2026-05-01T07:59:39.469Zhttps://docs.openclaw.ai/fr/gateway/configuration2026-05-01T07:59:39.470Zhttps://docs.openclaw.ai/fr/gateway/configuration-examples2026-05-01T07:59:39.468Zhttps://docs.openclaw.ai/fr/gateway/configuration-reference2026-05-01T07:59:39.469Zhttps://docs.openclaw.ai/fr/gateway/diagnostics2026-05-01T07:59:39.459Zhttps://docs.openclaw.ai/fr/gateway/discovery2026-05-01T07:59:39.458Zhttps://docs.openclaw.ai/fr/gateway/doctor2026-05-01T07:59:39.459Zhttps://docs.openclaw.ai/fr/gateway/gateway-lock2026-05-01T07:59:39.458Zhttps://docs.openclaw.ai/fr/gateway/health2026-05-01T07:59:39.466Zhttps://docs.openclaw.ai/fr/gateway/heartbeat2026-05-01T07:59:39.513Zhttps://docs.openclaw.ai/fr/gateway2026-05-01T07:59:39.512Zhttps://docs.openclaw.ai/fr/gateway/local-models2026-05-01T07:59:39.500Zhttps://docs.openclaw.ai/fr/gateway/logging2026-05-01T07:59:39.499Zhttps://docs.openclaw.ai/fr/gateway/multiple-gateways2026-05-01T07:59:39.499Zhttps://docs.openclaw.ai/fr/gateway/network-model2026-05-01T07:59:39.496Zhttps://docs.openclaw.ai/fr/gateway/openai-http-api2026-05-01T07:59:39.491Zhttps://docs.openclaw.ai/fr/gateway/openresponses-http-api2026-05-01T07:59:39.492Zhttps://docs.openclaw.ai/fr/gateway/openshell2026-05-01T07:59:39.491Zhttps://docs.openclaw.ai/fr/gateway/opentelemetry2026-05-01T07:59:39.490Zhttps://docs.openclaw.ai/fr/gateway/pairing2026-05-01T07:59:39.544Zhttps://docs.openclaw.ai/fr/gateway/prometheus2026-05-01T07:59:39.543Zhttps://docs.openclaw.ai/fr/gateway/protocol2026-05-01T07:59:39.540Zhttps://docs.openclaw.ai/fr/gateway/remote2026-05-01T07:59:39.542Zhttps://docs.openclaw.ai/fr/gateway/remote-gateway-readme2026-05-01T07:59:39.542Zhttps://docs.openclaw.ai/fr/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:39.543Zhttps://docs.openclaw.ai/fr/gateway/sandboxing2026-05-01T07:59:39.535Zhttps://docs.openclaw.ai/fr/gateway/secrets2026-05-01T07:59:39.534Zhttps://docs.openclaw.ai/fr/gateway/secrets-plan-contract2026-05-01T07:59:39.535Zhttps://docs.openclaw.ai/fr/gateway/security/audit-checks2026-05-01T07:59:39.534Zhttps://docs.openclaw.ai/fr/gateway/security2026-04-30T07:58:53.591Zhttps://docs.openclaw.ai/fr/gateway/tailscale2026-05-01T07:59:39.576Zhttps://docs.openclaw.ai/fr/gateway/tools-invoke-http-api2026-05-01T07:59:39.575Zhttps://docs.openclaw.ai/fr/gateway/troubleshooting2026-05-01T07:59:39.575Zhttps://docs.openclaw.ai/fr/gateway/trusted-proxy-auth2026-05-01T07:59:39.574Zhttps://docs.openclaw.ai/fr/help/debugging2026-05-01T07:59:39.566Zhttps://docs.openclaw.ai/fr/help/environment2026-05-01T07:59:39.572Zhttps://docs.openclaw.ai/fr/help/faq2026-05-01T07:59:39.565Zhttps://docs.openclaw.ai/fr/help/faq-first-run2026-05-01T07:59:39.566Zhttps://docs.openclaw.ai/fr/help/faq-models2026-05-01T07:59:39.573Zhttps://docs.openclaw.ai/fr/help/gpt55-codex-agentic-parity2026-05-01T07:59:39.616Zhttps://docs.openclaw.ai/fr/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:39.565Zhttps://docs.openclaw.ai/fr/help2026-05-01T07:59:39.618Zhttps://docs.openclaw.ai/fr/help/scripts2026-05-01T07:59:39.617Zhttps://docs.openclaw.ai/fr/help/testing2026-05-01T07:59:39.597Zhttps://docs.openclaw.ai/fr/help/testing-live2026-05-01T07:59:39.618Zhttps://docs.openclaw.ai/fr/help/troubleshooting2026-05-01T07:59:39.604Zhttps://docs.openclaw.ai/fr2026-05-01T07:59:39.619Zhttps://docs.openclaw.ai/fr/install/ansible2026-05-01T07:59:39.598Zhttps://docs.openclaw.ai/fr/install/azure2026-05-01T07:59:39.597Zhttps://docs.openclaw.ai/fr/install/bun2026-05-01T07:59:39.596Zhttps://docs.openclaw.ai/fr/install/clawdock2026-05-01T07:59:39.647Zhttps://docs.openclaw.ai/fr/install/development-channels2026-05-01T07:59:39.639Zhttps://docs.openclaw.ai/fr/install/digitalocean2026-05-01T07:59:39.646Zhttps://docs.openclaw.ai/fr/install/docker2026-05-01T07:59:39.648Zhttps://docs.openclaw.ai/fr/install/docker-vm-runtime2026-05-01T07:59:39.647Zhttps://docs.openclaw.ai/fr/install/exe-dev2026-05-01T07:59:39.638Zhttps://docs.openclaw.ai/fr/install/fly2026-05-01T07:59:39.640Zhttps://docs.openclaw.ai/fr/install/gcp2026-05-01T07:59:39.640Zhttps://docs.openclaw.ai/fr/install/hetzner2026-05-01T07:59:39.639Zhttps://docs.openclaw.ai/fr/install/hostinger2026-05-01T07:59:39.643Zhttps://docs.openclaw.ai/fr/install2026-05-01T07:59:39.678Zhttps://docs.openclaw.ai/fr/install/installer2026-05-01T07:59:39.679Zhttps://docs.openclaw.ai/fr/install/kubernetes2026-05-01T07:59:39.672Zhttps://docs.openclaw.ai/fr/install/macos-vm2026-05-01T07:59:39.676Zhttps://docs.openclaw.ai/fr/install/migrating2026-05-01T07:59:39.672Zhttps://docs.openclaw.ai/fr/install/migrating-claude2026-05-01T07:59:39.671Zhttps://docs.openclaw.ai/fr/install/migrating-hermes2026-05-01T07:59:39.671Zhttps://docs.openclaw.ai/fr/install/nix2026-05-01T07:59:39.670Zhttps://docs.openclaw.ai/fr/install/node2026-05-01T07:59:39.676Zhttps://docs.openclaw.ai/fr/install/northflank2026-05-01T07:59:39.675Zhttps://docs.openclaw.ai/fr/install/oracle2026-05-01T07:59:39.703Zhttps://docs.openclaw.ai/fr/install/podman2026-05-01T07:59:39.698Zhttps://docs.openclaw.ai/fr/install/railway2026-05-01T07:59:39.700Zhttps://docs.openclaw.ai/fr/install/raspberry-pi2026-05-01T07:59:39.718Zhttps://docs.openclaw.ai/fr/install/render2026-05-01T07:59:39.702Zhttps://docs.openclaw.ai/fr/install/uninstall2026-05-01T07:59:39.706Zhttps://docs.openclaw.ai/fr/install/updating2026-05-01T07:59:39.699Zhttps://docs.openclaw.ai/fr/logging2026-05-01T07:59:39.705Zhttps://docs.openclaw.ai/fr/network2026-05-01T07:59:39.699Zhttps://docs.openclaw.ai/fr/nodes/audio2026-05-01T07:59:39.698Zhttps://docs.openclaw.ai/fr/nodes/camera2026-05-01T07:59:39.747Zhttps://docs.openclaw.ai/fr/nodes/images2026-05-01T07:59:39.744Zhttps://docs.openclaw.ai/fr/nodes2026-05-01T07:59:39.746Zhttps://docs.openclaw.ai/fr/nodes/location-command2026-05-01T07:59:39.747Zhttps://docs.openclaw.ai/fr/nodes/media-understanding2026-05-01T07:59:39.739Zhttps://docs.openclaw.ai/fr/nodes/talk2026-05-01T07:59:39.746Zhttps://docs.openclaw.ai/fr/nodes/troubleshooting2026-05-01T07:59:39.745Zhttps://docs.openclaw.ai/fr/nodes/voicewake2026-05-01T07:59:39.738Zhttps://docs.openclaw.ai/fr/pi2026-05-01T07:59:39.778Zhttps://docs.openclaw.ai/fr/pi-dev2026-05-01T07:59:39.737Zhttps://docs.openclaw.ai/fr/platforms/android2026-05-01T07:59:39.777Zhttps://docs.openclaw.ai/fr/platforms2026-05-01T07:59:39.768Zhttps://docs.openclaw.ai/fr/platforms/ios2026-05-01T07:59:39.768Zhttps://docs.openclaw.ai/fr/platforms/linux2026-05-01T07:59:39.769Zhttps://docs.openclaw.ai/fr/platforms/mac/bundled-gateway2026-05-01T07:59:39.767Zhttps://docs.openclaw.ai/fr/platforms/mac/canvas2026-05-01T07:59:39.774Zhttps://docs.openclaw.ai/fr/platforms/mac/child-process2026-05-01T07:59:39.812Zhttps://docs.openclaw.ai/fr/platforms/mac/dev-setup2026-05-01T07:59:39.811Zhttps://docs.openclaw.ai/fr/platforms/mac/health2026-05-01T07:59:39.811Zhttps://docs.openclaw.ai/fr/platforms/mac/icon2026-05-01T07:59:39.814Zhttps://docs.openclaw.ai/fr/platforms/mac/logging2026-05-01T07:59:39.799Zhttps://docs.openclaw.ai/fr/platforms/mac/menu-bar2026-05-01T07:59:39.800Zhttps://docs.openclaw.ai/fr/platforms/mac/peekaboo2026-05-01T07:59:39.810Zhttps://docs.openclaw.ai/fr/platforms/mac/permissions2026-05-01T07:59:39.800Zhttps://docs.openclaw.ai/fr/platforms/mac/remote2026-05-01T07:59:39.806Zhttps://docs.openclaw.ai/fr/platforms/mac/signing2026-05-01T07:59:39.813Zhttps://docs.openclaw.ai/fr/platforms/mac/skills2026-05-01T07:59:39.856Zhttps://docs.openclaw.ai/fr/platforms/mac/voice-overlay2026-05-01T07:59:39.850Zhttps://docs.openclaw.ai/fr/platforms/mac/voicewake2026-05-01T07:59:39.861Zhttps://docs.openclaw.ai/fr/platforms/mac/webchat2026-05-01T07:59:39.859Zhttps://docs.openclaw.ai/fr/platforms/mac/xpc2026-05-01T07:59:39.860Zhttps://docs.openclaw.ai/fr/platforms/macos2026-05-01T07:59:39.851Zhttps://docs.openclaw.ai/fr/platforms/windows2026-05-01T07:59:39.852Zhttps://docs.openclaw.ai/fr/plugins/architecture2026-05-01T07:59:39.896Zhttps://docs.openclaw.ai/fr/plugins/architecture-internals2026-05-01T07:59:39.897Zhttps://docs.openclaw.ai/fr/plugins/building-plugins2026-05-01T07:59:39.894Zhttps://docs.openclaw.ai/fr/plugins/bundles2026-05-01T07:59:39.898Zhttps://docs.openclaw.ai/fr/plugins/codex-computer-use2026-05-01T07:59:39.888Zhttps://docs.openclaw.ai/fr/plugins/codex-harness2026-05-01T07:59:39.889Zhttps://docs.openclaw.ai/fr/plugins/community2026-05-01T07:59:39.888Zhttps://docs.openclaw.ai/fr/plugins/compatibility2026-05-01T07:59:39.887Zhttps://docs.openclaw.ai/fr/plugins/google-meet2026-05-01T07:59:39.887Zhttps://docs.openclaw.ai/fr/plugins/hooks2026-05-01T07:59:39.958Zhttps://docs.openclaw.ai/fr/plugins/manifest2026-05-01T07:59:39.950Zhttps://docs.openclaw.ai/fr/plugins/memory-lancedb2026-05-01T07:59:39.957Zhttps://docs.openclaw.ai/fr/plugins/memory-wiki2026-05-01T07:59:39.956Zhttps://docs.openclaw.ai/fr/plugins/message-presentation2026-05-01T07:59:39.954Zhttps://docs.openclaw.ai/fr/plugins/sdk-agent-harness2026-05-01T07:59:39.949Zhttps://docs.openclaw.ai/fr/plugins/sdk-channel-plugins2026-05-01T07:59:39.950Zhttps://docs.openclaw.ai/fr/plugins/sdk-channel-turn2026-05-01T07:59:39.949Zhttps://docs.openclaw.ai/fr/plugins/sdk-entrypoints2026-05-01T07:59:39.948Zhttps://docs.openclaw.ai/fr/plugins/sdk-migration2026-05-01T07:59:39.953Zhttps://docs.openclaw.ai/fr/plugins/sdk-overview2026-05-01T07:59:39.996Zhttps://docs.openclaw.ai/fr/plugins/sdk-provider-plugins2026-05-01T07:59:39.989Zhttps://docs.openclaw.ai/fr/plugins/sdk-runtime2026-05-01T07:59:39.990Zhttps://docs.openclaw.ai/fr/plugins/sdk-setup2026-05-01T07:59:39.992Zhttps://docs.openclaw.ai/fr/plugins/sdk-subpaths2026-05-01T07:59:39.987Zhttps://docs.openclaw.ai/fr/plugins/sdk-testing2026-05-01T07:59:39.993Zhttps://docs.openclaw.ai/fr/plugins/skill-workshop2026-05-01T07:59:39.979Zhttps://docs.openclaw.ai/fr/plugins/voice-call2026-05-01T07:59:39.990Zhttps://docs.openclaw.ai/fr/plugins/webhooks2026-05-01T07:59:39.980Zhttps://docs.openclaw.ai/fr/plugins/zalouser2026-05-01T07:59:39.978Zhttps://docs.openclaw.ai/fr/prose2026-05-01T07:59:40.035Zhttps://docs.openclaw.ai/fr/providers/alibaba2026-05-01T07:59:40.032Zhttps://docs.openclaw.ai/fr/providers/anthropic2026-05-01T07:59:40.018Zhttps://docs.openclaw.ai/fr/providers/arcee2026-05-01T07:59:40.035Zhttps://docs.openclaw.ai/fr/providers/azure-speech2026-05-01T07:59:40.022Zhttps://docs.openclaw.ai/fr/providers/bedrock2026-05-01T07:59:40.021Zhttps://docs.openclaw.ai/fr/providers/bedrock-mantle2026-05-01T07:59:40.017Zhttps://docs.openclaw.ai/fr/providers/chutes2026-05-01T07:59:40.019Zhttps://docs.openclaw.ai/fr/providers/claude-max-api-proxy2026-05-01T07:59:40.019Zhttps://docs.openclaw.ai/fr/providers/cloudflare-ai-gateway2026-05-01T07:59:40.064Zhttps://docs.openclaw.ai/fr/providers/comfy2026-05-01T07:59:40.063Zhttps://docs.openclaw.ai/fr/providers/deepgram2026-05-01T07:59:40.062Zhttps://docs.openclaw.ai/fr/providers/deepinfra2026-05-01T07:59:40.061Zhttps://docs.openclaw.ai/fr/providers/deepseek2026-05-01T07:59:40.056Zhttps://docs.openclaw.ai/fr/providers/elevenlabs2026-05-01T07:59:40.054Zhttps://docs.openclaw.ai/fr/providers/fal2026-05-01T07:59:40.055Zhttps://docs.openclaw.ai/fr/providers/fireworks2026-05-01T07:59:40.054Zhttps://docs.openclaw.ai/fr/providers/github-copilot2026-05-01T07:59:40.064Zhttps://docs.openclaw.ai/fr/providers/glm2026-05-01T07:59:40.063Zhttps://docs.openclaw.ai/fr/providers/google2026-05-01T07:59:40.093Zhttps://docs.openclaw.ai/fr/providers/gradium2026-05-01T07:59:40.093Zhttps://docs.openclaw.ai/fr/providers/groq2026-05-01T07:59:40.092Zhttps://docs.openclaw.ai/fr/providers/huggingface2026-05-01T07:59:40.085Zhttps://docs.openclaw.ai/fr/providers2026-05-01T07:59:40.092Zhttps://docs.openclaw.ai/fr/providers/inferrs2026-05-01T07:59:40.089Zhttps://docs.openclaw.ai/fr/providers/inworld2026-05-01T07:59:40.091Zhttps://docs.openclaw.ai/fr/providers/kilocode2026-05-01T07:59:40.084Zhttps://docs.openclaw.ai/fr/providers/litellm2026-05-01T07:59:40.083Zhttps://docs.openclaw.ai/fr/providers/lmstudio2026-05-01T07:59:40.083Zhttps://docs.openclaw.ai/fr/providers/minimax2026-05-01T07:59:40.135Zhttps://docs.openclaw.ai/fr/providers/mistral2026-05-01T07:59:40.123Zhttps://docs.openclaw.ai/fr/providers/models2026-05-01T07:59:40.123Zhttps://docs.openclaw.ai/fr/providers/moonshot2026-05-01T07:59:40.122Zhttps://docs.openclaw.ai/fr/providers/nvidia2026-05-01T07:59:40.115Zhttps://docs.openclaw.ai/fr/providers/ollama2026-05-01T07:59:40.120Zhttps://docs.openclaw.ai/fr/providers/openai2026-05-01T07:59:40.116Zhttps://docs.openclaw.ai/fr/providers/opencode2026-05-01T07:59:40.114Zhttps://docs.openclaw.ai/fr/providers/opencode-go2026-05-01T07:59:40.115Zhttps://docs.openclaw.ai/fr/providers/openrouter2026-05-01T07:59:40.114Zhttps://docs.openclaw.ai/fr/providers/perplexity-provider2026-05-01T07:59:40.163Zhttps://docs.openclaw.ai/fr/providers/qianfan2026-05-01T07:59:40.158Zhttps://docs.openclaw.ai/fr/providers/qwen2026-05-01T07:59:40.162Zhttps://docs.openclaw.ai/fr/providers/runway2026-05-01T07:59:40.161Zhttps://docs.openclaw.ai/fr/providers/sglang2026-05-01T07:59:40.161Zhttps://docs.openclaw.ai/fr/providers/stepfun2026-05-01T07:59:40.154Zhttps://docs.openclaw.ai/fr/providers/synthetic2026-05-01T07:59:40.154Zhttps://docs.openclaw.ai/fr/providers/tencent2026-05-01T07:59:40.153Zhttps://docs.openclaw.ai/fr/providers/together2026-05-01T07:59:40.153Zhttps://docs.openclaw.ai/fr/providers/venice2026-05-01T07:59:40.193Zhttps://docs.openclaw.ai/fr/providers/vercel-ai-gateway2026-05-01T07:59:40.192Zhttps://docs.openclaw.ai/fr/providers/vllm2026-05-01T07:59:40.192Zhttps://docs.openclaw.ai/fr/providers/volcengine2026-05-01T07:59:40.184Zhttps://docs.openclaw.ai/fr/providers/vydra2026-05-01T07:59:40.185Zhttps://docs.openclaw.ai/fr/providers/xai2026-05-01T07:59:40.190Zhttps://docs.openclaw.ai/fr/providers/xiaomi2026-05-01T07:59:40.185Zhttps://docs.openclaw.ai/fr/providers/zai2026-05-01T07:59:40.184Zhttps://docs.openclaw.ai/fr/reference/AGENTS.default2026-05-01T07:59:40.183Zhttps://docs.openclaw.ai/fr/reference/RELEASING2026-05-01T07:59:40.186Zhttps://docs.openclaw.ai/fr/reference/api-usage-costs2026-05-01T07:59:40.232Zhttps://docs.openclaw.ai/fr/reference/credits2026-05-01T07:59:40.221Zhttps://docs.openclaw.ai/fr/reference/device-models2026-05-01T07:59:40.217Zhttps://docs.openclaw.ai/fr/reference/full-release-validation2026-05-01T07:59:40.213Zhttps://docs.openclaw.ai/fr/reference/memory-config2026-05-01T07:59:40.211Zhttps://docs.openclaw.ai/fr/reference/openclaw-sdk-api-design2026-05-01T07:59:40.220Zhttps://docs.openclaw.ai/fr/reference/prompt-caching2026-05-01T07:59:40.212Zhttps://docs.openclaw.ai/fr/reference/rich-output-protocol2026-05-01T07:59:40.220Zhttps://docs.openclaw.ai/fr/reference/rpc2026-05-01T07:59:40.213Zhttps://docs.openclaw.ai/fr/reference/secretref-credential-surface2026-05-01T07:59:40.260Zhttps://docs.openclaw.ai/fr/reference/session-management-compaction2026-05-01T07:59:40.258Zhttps://docs.openclaw.ai/fr/reference/templates/AGENTS2026-05-01T07:59:40.258Zhttps://docs.openclaw.ai/fr/reference/templates/BOOT2026-05-01T07:59:40.260Zhttps://docs.openclaw.ai/fr/reference/templates/BOOTSTRAP2026-05-01T07:59:40.256Zhttps://docs.openclaw.ai/fr/reference/templates/HEARTBEAT2026-05-01T07:59:40.251Zhttps://docs.openclaw.ai/fr/reference/templates/IDENTITY2026-05-01T07:59:40.251Zhttps://docs.openclaw.ai/fr/reference/templates/SOUL2026-05-01T07:59:40.289Zhttps://docs.openclaw.ai/fr/reference/templates/TOOLS2026-05-01T07:59:40.288Zhttps://docs.openclaw.ai/fr/reference/templates/USER2026-05-01T07:59:40.282Zhttps://docs.openclaw.ai/fr/reference/test2026-05-01T07:59:40.285Zhttps://docs.openclaw.ai/fr/reference/token-use2026-05-01T07:59:40.281Zhttps://docs.openclaw.ai/fr/reference/transcript-hygiene2026-05-01T07:59:40.281Zhttps://docs.openclaw.ai/fr/reference/wizard2026-05-01T07:59:40.280Zhttps://docs.openclaw.ai/fr/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:40.280Zhttps://docs.openclaw.ai/fr/security/THREAT-MODEL-ATLAS2026-05-01T07:59:40.329Zhttps://docs.openclaw.ai/fr/security/formal-verification2026-05-01T07:59:40.312Zhttps://docs.openclaw.ai/fr/security/network-proxy2026-05-01T07:59:40.329Zhttps://docs.openclaw.ai/fr/start/bootstrapping2026-05-01T07:59:40.317Zhttps://docs.openclaw.ai/fr/start/docs-directory2026-05-01T07:59:40.314Zhttps://docs.openclaw.ai/fr/start/getting-started2026-05-01T07:59:40.310Zhttps://docs.openclaw.ai/fr/start/hubs2026-05-01T07:59:40.317Zhttps://docs.openclaw.ai/fr/start/lore2026-05-01T07:59:40.310Zhttps://docs.openclaw.ai/fr/start/onboarding2026-05-01T07:59:40.309Zhttps://docs.openclaw.ai/fr/start/onboarding-overview2026-05-01T07:59:40.309Zhttps://docs.openclaw.ai/fr/start/openclaw2026-05-01T07:59:40.360Zhttps://docs.openclaw.ai/fr/start/setup2026-05-01T07:59:40.357Zhttps://docs.openclaw.ai/fr/start/showcase2026-04-24T17:33:07.020Zhttps://docs.openclaw.ai/fr/start/wizard2026-05-01T07:59:40.358Zhttps://docs.openclaw.ai/fr/start/wizard-cli-automation2026-05-01T07:59:40.355Zhttps://docs.openclaw.ai/fr/start/wizard-cli-reference2026-05-01T07:59:40.359Zhttps://docs.openclaw.ai/fr/tools/acp-agents2026-05-01T07:59:40.349Zhttps://docs.openclaw.ai/fr/tools/acp-agents-setup2026-05-01T07:59:40.351Zhttps://docs.openclaw.ai/fr/tools/agent-send2026-05-01T07:59:40.390Zhttps://docs.openclaw.ai/fr/tools/apply-patch2026-05-01T07:59:40.389Zhttps://docs.openclaw.ai/fr/tools/brave-search2026-05-01T07:59:40.389Zhttps://docs.openclaw.ai/fr/tools/browser2026-05-01T07:59:40.381Zhttps://docs.openclaw.ai/fr/tools/browser-control2026-05-01T07:59:40.382Zhttps://docs.openclaw.ai/fr/tools/browser-linux-troubleshooting2026-05-01T07:59:40.382Zhttps://docs.openclaw.ai/fr/tools/browser-login2026-05-01T07:59:40.385Zhttps://docs.openclaw.ai/fr/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:40.381Zhttps://docs.openclaw.ai/fr/tools/btw2026-05-01T07:59:40.380Zhttps://docs.openclaw.ai/fr/tools/clawhub2026-05-01T07:59:40.435Zhttps://docs.openclaw.ai/fr/tools/code-execution2026-05-01T07:59:40.424Zhttps://docs.openclaw.ai/fr/tools/creating-skills2026-05-01T07:59:40.423Zhttps://docs.openclaw.ai/fr/tools/diffs2026-05-01T07:59:40.416Zhttps://docs.openclaw.ai/fr/tools/duckduckgo-search2026-05-01T07:59:40.420Zhttps://docs.openclaw.ai/fr/tools/elevated2026-05-01T07:59:40.418Zhttps://docs.openclaw.ai/fr/tools/exa-search2026-05-01T07:59:40.417Zhttps://docs.openclaw.ai/fr/tools/exec2026-05-01T07:59:40.415Zhttps://docs.openclaw.ai/fr/tools/exec-approvals2026-05-01T07:59:40.416Zhttps://docs.openclaw.ai/fr/tools/exec-approvals-advanced2026-05-01T07:59:40.417Zhttps://docs.openclaw.ai/fr/tools/firecrawl2026-05-01T07:59:40.464Zhttps://docs.openclaw.ai/fr/tools/gemini-search2026-05-01T07:59:40.462Zhttps://docs.openclaw.ai/fr/tools/grok-search2026-05-01T07:59:40.463Zhttps://docs.openclaw.ai/fr/tools/image-generation2026-05-01T07:59:40.461Zhttps://docs.openclaw.ai/fr/tools2026-05-01T07:59:40.464Zhttps://docs.openclaw.ai/fr/tools/kimi-search2026-05-01T07:59:40.455Zhttps://docs.openclaw.ai/fr/tools/llm-task2026-05-01T07:59:40.454Zhttps://docs.openclaw.ai/fr/tools/lobster2026-05-01T07:59:40.462Zhttps://docs.openclaw.ai/fr/tools/loop-detection2026-05-01T07:59:40.455Zhttps://docs.openclaw.ai/fr/tools/media-overview2026-05-01T07:59:40.454Zhttps://docs.openclaw.ai/fr/tools/minimax-search2026-05-01T07:59:40.494Zhttps://docs.openclaw.ai/fr/tools/multi-agent-sandbox-tools2026-05-01T07:59:40.493Zhttps://docs.openclaw.ai/fr/tools/music-generation2026-05-01T07:59:40.492Zhttps://docs.openclaw.ai/fr/tools/ollama-search2026-05-01T07:59:40.492Zhttps://docs.openclaw.ai/fr/tools/pdf2026-05-01T07:59:40.491Zhttps://docs.openclaw.ai/fr/tools/perplexity-search2026-05-01T07:59:40.489Zhttps://docs.openclaw.ai/fr/tools/plugin2026-05-01T07:59:40.485Zhttps://docs.openclaw.ai/fr/tools/reactions2026-05-01T07:59:40.484Zhttps://docs.openclaw.ai/fr/tools/searxng-search2026-05-01T07:59:40.484Zhttps://docs.openclaw.ai/fr/tools/skills2026-05-01T07:59:40.535Zhttps://docs.openclaw.ai/fr/tools/skills-config2026-05-01T07:59:40.483Zhttps://docs.openclaw.ai/fr/tools/slash-commands2026-05-01T07:59:40.534Zhttps://docs.openclaw.ai/fr/tools/subagents2026-05-01T07:59:40.517Zhttps://docs.openclaw.ai/fr/tools/tavily2026-05-01T07:59:40.535Zhttps://docs.openclaw.ai/fr/tools/thinking2026-05-01T07:59:40.522Zhttps://docs.openclaw.ai/fr/tools/tokenjuice2026-05-01T07:59:40.521Zhttps://docs.openclaw.ai/fr/tools/trajectory2026-05-01T07:59:40.517Zhttps://docs.openclaw.ai/fr/tools/tts2026-05-01T07:59:40.516Zhttps://docs.openclaw.ai/fr/tools/video-generation2026-05-01T07:59:40.516Zhttps://docs.openclaw.ai/fr/tools/web2026-05-01T07:59:40.567Zhttps://docs.openclaw.ai/fr/tools/web-fetch2026-05-01T07:59:40.522Zhttps://docs.openclaw.ai/fr/vps2026-05-01T07:59:40.566Zhttps://docs.openclaw.ai/fr/web/control-ui2026-05-01T07:59:40.558Zhttps://docs.openclaw.ai/fr/web/dashboard2026-05-01T07:59:40.556Zhttps://docs.openclaw.ai/fr/web2026-05-01T07:59:40.561Zhttps://docs.openclaw.ai/fr/web/tui2026-05-01T07:59:40.557Zhttps://docs.openclaw.ai/fr/web/webchat2026-05-01T07:59:40.565Zhttps://docs.openclaw.ai/gateway/authentication2026-05-01T07:59:40.557Zhttps://docs.openclaw.ai/gateway/background-process2026-05-01T07:59:40.556Zhttps://docs.openclaw.ai/gateway/bonjour2026-05-01T07:59:40.603Zhttps://docs.openclaw.ai/gateway/bridge-protocol2026-05-01T07:59:40.600Zhttps://docs.openclaw.ai/gateway/cli-backends2026-05-01T07:59:40.603Zhttps://docs.openclaw.ai/gateway/config-agents2026-05-01T11:03:07.139Zhttps://docs.openclaw.ai/gateway/config-channels2026-05-01T11:53:02.401Zhttps://docs.openclaw.ai/gateway/config-tools2026-05-01T07:59:40.602Zhttps://docs.openclaw.ai/gateway/configuration2026-05-01T11:03:07.124Zhttps://docs.openclaw.ai/gateway/configuration-examples2026-05-01T07:59:40.602Zhttps://docs.openclaw.ai/gateway/configuration-reference2026-05-01T07:59:40.592Zhttps://docs.openclaw.ai/gateway/diagnostics2026-05-01T07:59:40.590Zhttps://docs.openclaw.ai/gateway/discovery2026-05-01T07:59:40.683Zhttps://docs.openclaw.ai/gateway/doctor2026-05-01T08:31:12.082Zhttps://docs.openclaw.ai/gateway/gateway-lock2026-05-01T07:59:40.680Zhttps://docs.openclaw.ai/gateway/health2026-05-01T07:59:40.681Zhttps://docs.openclaw.ai/gateway/heartbeat2026-05-01T07:59:40.682Zhttps://docs.openclaw.ai/gateway2026-05-01T07:59:40.679Zhttps://docs.openclaw.ai/gateway/local-models2026-05-01T07:59:40.680Zhttps://docs.openclaw.ai/gateway/logging2026-05-01T07:59:40.681Zhttps://docs.openclaw.ai/gateway/multiple-gateways2026-05-01T07:59:40.679Zhttps://docs.openclaw.ai/gateway/network-model2026-05-01T07:59:40.678Zhttps://docs.openclaw.ai/gateway/openai-http-api2026-05-01T07:59:40.751Zhttps://docs.openclaw.ai/gateway/openresponses-http-api2026-05-01T07:59:40.750Zhttps://docs.openclaw.ai/gateway/openshell2026-05-01T07:59:40.750Zhttps://docs.openclaw.ai/gateway/opentelemetry2026-05-01T07:59:40.743Zhttps://docs.openclaw.ai/gateway/pairing2026-05-01T07:59:40.744Zhttps://docs.openclaw.ai/gateway/prometheus2026-05-01T07:59:40.744Zhttps://docs.openclaw.ai/gateway/protocol2026-05-01T08:58:38.086Zhttps://docs.openclaw.ai/gateway/remote2026-05-01T07:59:40.747Zhttps://docs.openclaw.ai/gateway/remote-gateway-readme2026-05-01T07:59:40.742Zhttps://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:40.742Zhttps://docs.openclaw.ai/gateway/sandboxing2026-05-01T11:03:07.126Zhttps://docs.openclaw.ai/gateway/secrets2026-05-01T07:59:40.791Zhttps://docs.openclaw.ai/gateway/secrets-plan-contract2026-05-01T07:59:40.791Zhttps://docs.openclaw.ai/gateway/security/audit-checks2026-05-01T07:59:40.776Zhttps://docs.openclaw.ai/gateway/security2026-05-01T07:59:40.792Zhttps://docs.openclaw.ai/gateway/tailscale2026-05-01T07:59:40.790Zhttps://docs.openclaw.ai/gateway/tools-invoke-http-api2026-05-01T07:59:40.772Zhttps://docs.openclaw.ai/gateway/troubleshooting2026-05-01T07:59:40.772Zhttps://docs.openclaw.ai/gateway/trusted-proxy-auth2026-05-01T07:59:40.771Zhttps://docs.openclaw.ai/help/debugging2026-05-01T07:59:40.770Zhttps://docs.openclaw.ai/help/environment2026-05-01T07:59:40.822Zhttps://docs.openclaw.ai/help/faq2026-05-01T07:59:40.819Zhttps://docs.openclaw.ai/help/faq-first-run2026-05-01T07:59:40.812Zhttps://docs.openclaw.ai/help/faq-models2026-05-01T07:59:40.821Zhttps://docs.openclaw.ai/help/gpt55-codex-agentic-parity2026-05-01T07:59:40.814Zhttps://docs.openclaw.ai/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:40.824Zhttps://docs.openclaw.ai/help2026-05-01T07:59:40.813Zhttps://docs.openclaw.ai/help/scripts2026-05-01T07:59:40.813Zhttps://docs.openclaw.ai/help/testing2026-05-01T08:28:03.731Zhttps://docs.openclaw.ai/help/testing-live2026-05-01T07:59:40.820Zhttps://docs.openclaw.ai/help/troubleshooting2026-05-01T07:59:40.852Zhttps://docs.openclaw.ai/id/auth-credential-semantics2026-05-01T07:59:40.851Zhttps://docs.openclaw.ai/id/automation/cron-jobs2026-05-01T07:59:40.843Zhttps://docs.openclaw.ai/id/automation/hooks2026-05-01T07:59:40.850Zhttps://docs.openclaw.ai/id/automation2026-05-01T07:59:40.842Zhttps://docs.openclaw.ai/id/automation/standing-orders2026-05-01T07:59:40.893Zhttps://docs.openclaw.ai/id/automation/taskflow2026-05-01T07:59:40.892Zhttps://docs.openclaw.ai/id/automation/tasks2026-05-01T09:33:05.104Zhttps://docs.openclaw.ai/id/channels/bluebubbles2026-05-01T09:33:05.097Zhttps://docs.openclaw.ai/id/channels/broadcast-groups2026-05-01T07:59:40.874Zhttps://docs.openclaw.ai/id/channels/channel-routing2026-05-01T07:59:40.873Zhttps://docs.openclaw.ai/id/channels/discord2026-05-01T07:59:40.922Zhttps://docs.openclaw.ai/id/channels/feishu2026-05-01T07:59:40.921Zhttps://docs.openclaw.ai/id/channels/googlechat2026-05-01T07:59:40.914Zhttps://docs.openclaw.ai/id/channels/group-messages2026-05-01T07:59:40.923Zhttps://docs.openclaw.ai/id/channels/groups2026-05-01T09:33:05.090Zhttps://docs.openclaw.ai/id/channels/imessage2026-05-01T07:59:40.915Zhttps://docs.openclaw.ai/id/channels2026-05-01T07:59:40.913Zhttps://docs.openclaw.ai/id/channels/irc2026-05-01T07:59:40.914Zhttps://docs.openclaw.ai/id/channels/line2026-05-01T07:59:40.919Zhttps://docs.openclaw.ai/id/channels/location2026-05-01T07:59:40.920Zhttps://docs.openclaw.ai/id/channels/matrix2026-05-01T07:59:40.952Zhttps://docs.openclaw.ai/id/channels/matrix-migration2026-05-01T07:59:40.954Zhttps://docs.openclaw.ai/id/channels/matrix-push-rules2026-05-01T07:59:40.951Zhttps://docs.openclaw.ai/id/channels/mattermost2026-05-01T07:59:40.950Zhttps://docs.openclaw.ai/id/channels/msteams2026-05-01T07:59:40.942Zhttps://docs.openclaw.ai/id/channels/nextcloud-talk2026-05-01T07:59:40.942Zhttps://docs.openclaw.ai/id/channels/nostr2026-05-01T07:59:40.944Zhttps://docs.openclaw.ai/id/channels/pairing2026-05-01T07:59:40.952Zhttps://docs.openclaw.ai/id/channels/qa-channel2026-05-01T09:33:05.093Zhttps://docs.openclaw.ai/id/channels/qqbot2026-05-01T07:59:40.943Zhttps://docs.openclaw.ai/id/channels/signal2026-05-01T07:59:40.992Zhttps://docs.openclaw.ai/id/channels/slack2026-05-01T07:59:40.991Zhttps://docs.openclaw.ai/id/channels/synology-chat2026-05-01T07:59:40.974Zhttps://docs.openclaw.ai/id/channels/telegram2026-05-01T07:59:40.993Zhttps://docs.openclaw.ai/id/channels/tlon2026-05-01T07:59:40.993Zhttps://docs.openclaw.ai/id/channels/troubleshooting2026-05-01T07:59:40.979Zhttps://docs.openclaw.ai/id/channels/twitch2026-05-01T07:59:40.994Zhttps://docs.openclaw.ai/id/channels/wechat2026-05-01T07:59:40.973Zhttps://docs.openclaw.ai/id/channels/whatsapp2026-05-01T07:59:40.973Zhttps://docs.openclaw.ai/id/channels/yuanbao2026-05-01T07:59:40.972Zhttps://docs.openclaw.ai/id/channels/zalo2026-05-01T07:59:41.022Zhttps://docs.openclaw.ai/id/channels/zalouser2026-05-01T07:59:41.014Zhttps://docs.openclaw.ai/id/ci2026-05-01T09:33:05.099Zhttps://docs.openclaw.ai/id/cli/acp2026-05-01T07:59:41.022Zhttps://docs.openclaw.ai/id/cli/agent2026-05-01T07:59:41.021Zhttps://docs.openclaw.ai/id/cli/agents2026-05-01T07:59:41.020Zhttps://docs.openclaw.ai/id/cli/approvals2026-05-01T07:59:41.020Zhttps://docs.openclaw.ai/id/cli/backup2026-05-01T07:59:41.017Zhttps://docs.openclaw.ai/id/cli/browser2026-05-01T07:59:41.012Zhttps://docs.openclaw.ai/id/cli/channels2026-05-01T09:33:05.094Zhttps://docs.openclaw.ai/id/cli/clawbot2026-05-01T07:59:41.051Zhttps://docs.openclaw.ai/id/cli/commitments2026-05-01T07:59:41.050Zhttps://docs.openclaw.ai/id/cli/completion2026-05-01T07:59:41.050Zhttps://docs.openclaw.ai/id/cli/config2026-05-01T07:59:41.042Zhttps://docs.openclaw.ai/id/cli/configure2026-05-01T09:33:05.094Zhttps://docs.openclaw.ai/id/cli/cron2026-05-01T07:59:41.049Zhttps://docs.openclaw.ai/id/cli/daemon2026-05-01T07:59:41.042Zhttps://docs.openclaw.ai/id/cli/dashboard2026-05-01T07:59:41.041Zhttps://docs.openclaw.ai/id/cli/devices2026-05-01T07:59:41.041Zhttps://docs.openclaw.ai/id/cli/directory2026-05-01T07:59:41.093Zhttps://docs.openclaw.ai/id/cli/dns2026-05-01T07:59:41.092Zhttps://docs.openclaw.ai/id/cli/docs2026-05-01T07:59:41.091Zhttps://docs.openclaw.ai/id/cli/doctor2026-05-01T07:59:41.073Zhttps://docs.openclaw.ai/id/cli/flows2026-05-01T07:59:41.090Zhttps://docs.openclaw.ai/id/cli/gateway2026-05-01T09:33:05.091Zhttps://docs.openclaw.ai/id/cli/health2026-05-01T07:59:41.073Zhttps://docs.openclaw.ai/id/cli/hooks2026-05-01T07:59:41.072Zhttps://docs.openclaw.ai/id/cli2026-05-01T07:59:41.072Zhttps://docs.openclaw.ai/id/cli/infer2026-05-01T07:59:41.071Zhttps://docs.openclaw.ai/id/cli/logs2026-05-01T07:59:41.122Zhttps://docs.openclaw.ai/id/cli/mcp2026-05-01T07:59:41.121Zhttps://docs.openclaw.ai/id/cli/memory2026-05-01T07:59:41.120Zhttps://docs.openclaw.ai/id/cli/message2026-05-01T07:59:41.119Zhttps://docs.openclaw.ai/id/cli/migrate2026-05-01T07:59:41.117Zhttps://docs.openclaw.ai/id/cli/models2026-05-01T09:33:05.092Zhttps://docs.openclaw.ai/id/cli/node2026-05-01T07:59:41.113Zhttps://docs.openclaw.ai/id/cli/nodes2026-05-01T07:59:41.112Zhttps://docs.openclaw.ai/id/cli/onboard2026-05-01T09:33:05.098Zhttps://docs.openclaw.ai/id/cli/pairing2026-05-01T07:59:41.111Zhttps://docs.openclaw.ai/id/cli/plugins2026-05-01T09:33:07.110Zhttps://docs.openclaw.ai/id/cli/proxy2026-05-01T09:33:07.111Zhttps://docs.openclaw.ai/id/cli/qr2026-05-01T07:59:41.149Zhttps://docs.openclaw.ai/id/cli/reset2026-05-01T07:59:41.148Zhttps://docs.openclaw.ai/id/cli/sandbox2026-05-01T07:59:41.141Zhttps://docs.openclaw.ai/id/cli/secrets2026-05-01T07:59:41.145Zhttps://docs.openclaw.ai/id/cli/security2026-05-01T07:59:41.149Zhttps://docs.openclaw.ai/id/cli/sessions2026-05-01T07:59:41.141Zhttps://docs.openclaw.ai/id/cli/setup2026-05-01T07:59:41.140Zhttps://docs.openclaw.ai/id/cli/skills2026-05-01T07:59:41.140Zhttps://docs.openclaw.ai/id/cli/status2026-05-01T07:59:41.190Zhttps://docs.openclaw.ai/id/cli/system2026-05-01T07:59:41.177Zhttps://docs.openclaw.ai/id/cli/tasks2026-05-01T07:59:41.176Zhttps://docs.openclaw.ai/id/cli/tui2026-05-01T07:59:41.170Zhttps://docs.openclaw.ai/id/cli/uninstall2026-05-01T07:59:41.177Zhttps://docs.openclaw.ai/id/cli/update2026-05-01T09:33:07.104Zhttps://docs.openclaw.ai/id/cli/voicecall2026-05-01T09:33:07.108Zhttps://docs.openclaw.ai/id/cli/webhooks2026-05-01T07:59:41.170Zhttps://docs.openclaw.ai/id/cli/wiki2026-05-01T07:59:41.169Zhttps://docs.openclaw.ai/id/concepts/active-memory2026-05-01T07:59:41.169Zhttps://docs.openclaw.ai/id/concepts/agent2026-05-01T07:59:41.216Zhttps://docs.openclaw.ai/id/concepts/agent-loop2026-05-01T07:59:41.219Zhttps://docs.openclaw.ai/id/concepts/agent-runtimes2026-05-01T07:59:41.218Zhttps://docs.openclaw.ai/id/concepts/agent-workspace2026-05-01T07:59:41.217Zhttps://docs.openclaw.ai/id/concepts/architecture2026-05-01T07:59:41.216Zhttps://docs.openclaw.ai/id/concepts/channel-docking2026-05-01T07:59:41.213Zhttps://docs.openclaw.ai/id/concepts/commitments2026-05-01T09:33:07.107Zhttps://docs.openclaw.ai/id/concepts/compaction2026-05-01T07:59:41.209Zhttps://docs.openclaw.ai/id/concepts/context2026-05-01T07:59:41.207Zhttps://docs.openclaw.ai/id/concepts/context-engine2026-05-01T07:59:41.208Zhttps://docs.openclaw.ai/id/concepts/delegate-architecture2026-05-01T07:59:41.248Zhttps://docs.openclaw.ai/id/concepts/dreaming2026-05-01T07:59:41.247Zhttps://docs.openclaw.ai/id/concepts/experimental-features2026-05-01T07:59:41.245Zhttps://docs.openclaw.ai/id/concepts/features2026-05-01T07:59:41.246Zhttps://docs.openclaw.ai/id/concepts/markdown-formatting2026-05-01T07:59:41.239Zhttps://docs.openclaw.ai/id/concepts/memory2026-05-01T07:59:41.237Zhttps://docs.openclaw.ai/id/concepts/memory-builtin2026-05-01T07:59:41.246Zhttps://docs.openclaw.ai/id/concepts/memory-honcho2026-05-01T07:59:41.243Zhttps://docs.openclaw.ai/id/concepts/memory-qmd2026-05-01T07:59:41.238Zhttps://docs.openclaw.ai/id/concepts/memory-search2026-05-01T07:59:41.238Zhttps://docs.openclaw.ai/id/concepts/messages2026-05-01T07:59:41.289Zhttps://docs.openclaw.ai/id/concepts/model-failover2026-05-01T07:59:41.278Zhttps://docs.openclaw.ai/id/concepts/model-providers2026-05-01T07:59:41.270Zhttps://docs.openclaw.ai/id/concepts/models2026-05-01T07:59:41.277Zhttps://docs.openclaw.ai/id/concepts/multi-agent2026-05-01T07:59:41.269Zhttps://docs.openclaw.ai/id/concepts/oauth2026-05-01T07:59:41.274Zhttps://docs.openclaw.ai/id/concepts/openclaw-sdk2026-05-01T09:33:07.107Zhttps://docs.openclaw.ai/id/concepts/presence2026-05-01T07:59:41.271Zhttps://docs.openclaw.ai/id/concepts/qa-e2e-automation2026-05-01T07:59:41.269Zhttps://docs.openclaw.ai/id/concepts/qa-matrix2026-05-01T07:59:41.268Zhttps://docs.openclaw.ai/id/concepts/queue2026-05-01T07:59:41.316Zhttps://docs.openclaw.ai/id/concepts/queue-steering2026-05-01T07:59:41.317Zhttps://docs.openclaw.ai/id/concepts/retry2026-05-01T07:59:41.317Zhttps://docs.openclaw.ai/id/concepts/session2026-05-01T07:59:41.308Zhttps://docs.openclaw.ai/id/concepts/session-pruning2026-05-01T07:59:41.309Zhttps://docs.openclaw.ai/id/concepts/session-tool2026-05-01T07:59:41.314Zhttps://docs.openclaw.ai/id/concepts/soul2026-05-01T07:59:41.315Zhttps://docs.openclaw.ai/id/concepts/streaming2026-05-01T07:59:41.309Zhttps://docs.openclaw.ai/id/concepts/system-prompt2026-05-01T07:59:41.310Zhttps://docs.openclaw.ai/id/concepts/timezone2026-05-01T07:59:41.308Zhttps://docs.openclaw.ai/id/concepts/typebox2026-05-01T07:59:41.346Zhttps://docs.openclaw.ai/id/concepts/typing-indicators2026-05-01T07:59:41.345Zhttps://docs.openclaw.ai/id/concepts/usage-tracking2026-05-01T07:59:41.346Zhttps://docs.openclaw.ai/id/date-time2026-05-01T07:59:41.339Zhttps://docs.openclaw.ai/id/debug/node-issue2026-05-01T07:59:41.343Zhttps://docs.openclaw.ai/id/diagnostics/flags2026-05-01T07:59:41.338Zhttps://docs.openclaw.ai/id/gateway/authentication2026-05-01T07:59:41.345Zhttps://docs.openclaw.ai/id/gateway/background-process2026-05-01T07:59:41.337Zhttps://docs.openclaw.ai/id/gateway/bonjour2026-05-01T07:59:41.338Zhttps://docs.openclaw.ai/id/gateway/bridge-protocol2026-05-01T07:59:41.337Zhttps://docs.openclaw.ai/id/gateway/cli-backends2026-05-01T07:59:41.406Zhttps://docs.openclaw.ai/id/gateway/config-agents2026-05-01T09:33:07.113Zhttps://docs.openclaw.ai/id/gateway/config-channels2026-05-01T09:33:07.110Zhttps://docs.openclaw.ai/id/gateway/config-tools2026-05-01T09:33:07.112Zhttps://docs.openclaw.ai/id/gateway/configuration2026-05-01T07:59:41.391Zhttps://docs.openclaw.ai/id/gateway/configuration-examples2026-05-01T07:59:41.401Zhttps://docs.openclaw.ai/id/gateway/configuration-reference2026-05-01T07:59:41.402Zhttps://docs.openclaw.ai/id/gateway/diagnostics2026-05-01T07:59:41.392Zhttps://docs.openclaw.ai/id/gateway/discovery2026-05-01T07:59:41.391Zhttps://docs.openclaw.ai/id/gateway/doctor2026-05-01T09:33:07.122Zhttps://docs.openclaw.ai/id/gateway/gateway-lock2026-05-01T07:59:41.440Zhttps://docs.openclaw.ai/id/gateway/health2026-05-01T07:59:41.436Zhttps://docs.openclaw.ai/id/gateway/heartbeat2026-05-01T07:59:41.435Zhttps://docs.openclaw.ai/id/gateway2026-05-01T07:59:41.437Zhttps://docs.openclaw.ai/id/gateway/local-models2026-05-01T07:59:41.434Zhttps://docs.openclaw.ai/id/gateway/logging2026-05-01T09:33:10.331Zhttps://docs.openclaw.ai/id/gateway/multiple-gateways2026-05-01T07:59:41.427Zhttps://docs.openclaw.ai/id/gateway/network-model2026-05-01T07:59:41.426Zhttps://docs.openclaw.ai/id/gateway/openai-http-api2026-05-01T07:59:41.425Zhttps://docs.openclaw.ai/id/gateway/openresponses-http-api2026-05-01T07:59:41.424Zhttps://docs.openclaw.ai/id/gateway/openshell2026-05-01T07:59:41.479Zhttps://docs.openclaw.ai/id/gateway/opentelemetry2026-05-01T07:59:41.466Zhttps://docs.openclaw.ai/id/gateway/pairing2026-05-01T07:59:41.466Zhttps://docs.openclaw.ai/id/gateway/prometheus2026-05-01T07:59:41.464Zhttps://docs.openclaw.ai/id/gateway/protocol2026-05-01T09:33:10.332Zhttps://docs.openclaw.ai/id/gateway/remote2026-05-01T07:59:41.459Zhttps://docs.openclaw.ai/id/gateway/remote-gateway-readme2026-05-01T07:59:41.467Zhttps://docs.openclaw.ai/id/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:41.459Zhttps://docs.openclaw.ai/id/gateway/sandboxing2026-05-01T07:59:41.458Zhttps://docs.openclaw.ai/id/gateway/secrets2026-05-01T07:59:41.509Zhttps://docs.openclaw.ai/id/gateway/secrets-plan-contract2026-05-01T07:59:41.458Zhttps://docs.openclaw.ai/id/gateway/security/audit-checks2026-05-01T07:59:41.508Zhttps://docs.openclaw.ai/id/gateway/security2026-04-30T10:22:16.796Zhttps://docs.openclaw.ai/id/gateway/tailscale2026-05-01T07:59:41.504Zhttps://docs.openclaw.ai/id/gateway/tools-invoke-http-api2026-05-01T07:59:41.506Zhttps://docs.openclaw.ai/id/gateway/troubleshooting2026-05-01T09:33:10.349Zhttps://docs.openclaw.ai/id/gateway/trusted-proxy-auth2026-05-01T07:59:41.507Zhttps://docs.openclaw.ai/id/help/debugging2026-05-01T07:59:41.499Zhttps://docs.openclaw.ai/id/help/environment2026-05-01T07:59:41.499Zhttps://docs.openclaw.ai/id/help/faq2026-05-01T07:59:41.541Zhttps://docs.openclaw.ai/id/help/faq-first-run2026-05-01T07:59:41.498Zhttps://docs.openclaw.ai/id/help/faq-models2026-05-01T07:59:41.498Zhttps://docs.openclaw.ai/id/help/gpt55-codex-agentic-parity2026-05-01T07:59:41.530Zhttps://docs.openclaw.ai/id/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:41.539Zhttps://docs.openclaw.ai/id/help2026-05-01T07:59:41.537Zhttps://docs.openclaw.ai/id/help/scripts2026-05-01T07:59:41.538Zhttps://docs.openclaw.ai/id/help/testing2026-05-01T09:33:10.327Zhttps://docs.openclaw.ai/id/help/testing-live2026-05-01T07:59:41.537Zhttps://docs.openclaw.ai/id/help/troubleshooting2026-05-01T07:59:41.539Zhttps://docs.openclaw.ai/id2026-05-01T07:59:41.528Zhttps://docs.openclaw.ai/id/install/ansible2026-05-01T07:59:41.527Zhttps://docs.openclaw.ai/id/install/azure2026-05-01T07:59:41.580Zhttps://docs.openclaw.ai/id/install/bun2026-05-01T07:59:41.579Zhttps://docs.openclaw.ai/id/install/clawdock2026-05-01T07:59:41.561Zhttps://docs.openclaw.ai/id/install/development-channels2026-05-01T07:59:41.579Zhttps://docs.openclaw.ai/id/install/digitalocean2026-05-01T07:59:41.578Zhttps://docs.openclaw.ai/id/install/docker2026-05-01T07:59:41.562Zhttps://docs.openclaw.ai/id/install/docker-vm-runtime2026-05-01T07:59:41.565Zhttps://docs.openclaw.ai/id/install/exe-dev2026-05-01T07:59:41.562Zhttps://docs.openclaw.ai/id/install/fly2026-05-01T07:59:41.561Zhttps://docs.openclaw.ai/id/install/gcp2026-05-01T07:59:41.560Zhttps://docs.openclaw.ai/id/install/hetzner2026-05-01T07:59:41.609Zhttps://docs.openclaw.ai/id/install/hostinger2026-05-01T07:59:41.608Zhttps://docs.openclaw.ai/id/install2026-05-01T07:59:41.608Zhttps://docs.openclaw.ai/id/install/installer2026-05-01T07:59:41.602Zhttps://docs.openclaw.ai/id/install/kubernetes2026-05-01T07:59:41.606Zhttps://docs.openclaw.ai/id/install/macos-vm2026-05-01T07:59:41.602Zhttps://docs.openclaw.ai/id/install/migrating2026-05-01T07:59:41.600Zhttps://docs.openclaw.ai/id/install/migrating-claude2026-05-01T07:59:41.600Zhttps://docs.openclaw.ai/id/install/migrating-hermes2026-05-01T07:59:41.601Zhttps://docs.openclaw.ai/id/install/nix2026-05-01T07:59:41.605Zhttps://docs.openclaw.ai/id/install/node2026-05-01T07:59:41.632Zhttps://docs.openclaw.ai/id/install/northflank2026-05-01T07:59:41.633Zhttps://docs.openclaw.ai/id/install/oracle2026-05-01T07:59:41.627Zhttps://docs.openclaw.ai/id/install/podman2026-05-01T07:59:41.635Zhttps://docs.openclaw.ai/id/install/railway2026-05-01T07:59:41.630Zhttps://docs.openclaw.ai/id/install/raspberry-pi2026-05-01T07:59:41.635Zhttps://docs.openclaw.ai/id/install/render2026-05-01T07:59:41.632Zhttps://docs.openclaw.ai/id/install/uninstall2026-05-01T07:59:41.629Zhttps://docs.openclaw.ai/id/install/updating2026-05-01T09:33:10.330Zhttps://docs.openclaw.ai/id/logging2026-05-01T09:33:10.332Zhttps://docs.openclaw.ai/id/network2026-05-01T07:59:41.654Zhttps://docs.openclaw.ai/id/nodes/audio2026-05-01T07:59:41.665Zhttps://docs.openclaw.ai/id/nodes/camera2026-05-01T07:59:41.666Zhttps://docs.openclaw.ai/id/nodes/images2026-05-01T07:59:41.659Zhttps://docs.openclaw.ai/id/nodes2026-05-01T07:59:41.664Zhttps://docs.openclaw.ai/id/nodes/location-command2026-05-01T07:59:41.662Zhttps://docs.openclaw.ai/id/nodes/media-understanding2026-05-01T07:59:41.654Zhttps://docs.openclaw.ai/id/nodes/talk2026-05-01T07:59:41.663Zhttps://docs.openclaw.ai/id/nodes/troubleshooting2026-05-01T07:59:41.653Zhttps://docs.openclaw.ai/id/nodes/voicewake2026-05-01T07:59:41.653Zhttps://docs.openclaw.ai/id/pi2026-05-01T07:59:41.705Zhttps://docs.openclaw.ai/id/pi-dev2026-05-01T07:59:41.703Zhttps://docs.openclaw.ai/id/platforms/android2026-05-01T07:59:41.697Zhttps://docs.openclaw.ai/id/platforms2026-05-01T07:59:41.699Zhttps://docs.openclaw.ai/id/platforms/ios2026-05-01T07:59:41.698Zhttps://docs.openclaw.ai/id/platforms/linux2026-05-01T07:59:41.697Zhttps://docs.openclaw.ai/id/platforms/mac/bundled-gateway2026-05-01T07:59:41.735Zhttps://docs.openclaw.ai/id/platforms/mac/canvas2026-05-01T07:59:41.734Zhttps://docs.openclaw.ai/id/platforms/mac/child-process2026-05-01T07:59:41.734Zhttps://docs.openclaw.ai/id/platforms/mac/dev-setup2026-05-01T07:59:41.733Zhttps://docs.openclaw.ai/id/platforms/mac/health2026-05-01T07:59:41.727Zhttps://docs.openclaw.ai/id/platforms/mac/icon2026-05-01T07:59:41.726Zhttps://docs.openclaw.ai/id/platforms/mac/logging2026-05-01T07:59:41.727Zhttps://docs.openclaw.ai/id/platforms/mac/menu-bar2026-05-01T09:33:10.329Zhttps://docs.openclaw.ai/id/platforms/mac/peekaboo2026-05-01T07:59:41.730Zhttps://docs.openclaw.ai/id/platforms/mac/permissions2026-05-01T07:59:41.725Zhttps://docs.openclaw.ai/id/platforms/mac/remote2026-05-01T07:59:41.764Zhttps://docs.openclaw.ai/id/platforms/mac/signing2026-05-01T07:59:41.763Zhttps://docs.openclaw.ai/id/platforms/mac/skills2026-05-01T07:59:41.763Zhttps://docs.openclaw.ai/id/platforms/mac/voice-overlay2026-05-01T07:59:41.762Zhttps://docs.openclaw.ai/id/platforms/mac/voicewake2026-05-01T07:59:41.759Zhttps://docs.openclaw.ai/id/platforms/mac/webchat2026-05-01T07:59:41.755Zhttps://docs.openclaw.ai/id/platforms/mac/xpc2026-05-01T07:59:41.756Zhttps://docs.openclaw.ai/id/platforms/macos2026-05-01T07:59:41.755Zhttps://docs.openclaw.ai/id/platforms/windows2026-05-01T07:59:41.803Zhttps://docs.openclaw.ai/id/plugins/architecture2026-05-01T07:59:41.800Zhttps://docs.openclaw.ai/id/plugins/architecture-internals2026-05-01T07:59:41.797Zhttps://docs.openclaw.ai/id/plugins/building-plugins2026-05-01T07:59:41.790Zhttps://docs.openclaw.ai/id/plugins/bundles2026-05-01T07:59:41.791Zhttps://docs.openclaw.ai/id/plugins/codex-computer-use2026-05-01T07:59:41.791Zhttps://docs.openclaw.ai/id/plugins/codex-harness2026-05-01T09:33:10.335Zhttps://docs.openclaw.ai/id/plugins/community2026-05-01T07:59:41.790Zhttps://docs.openclaw.ai/id/plugins/compatibility2026-05-01T07:59:41.838Zhttps://docs.openclaw.ai/id/plugins/dependency-resolution2026-05-01T09:33:10.329Zhttps://docs.openclaw.ai/id/plugins/google-meet2026-05-01T09:33:10.337Zhttps://docs.openclaw.ai/id/plugins/hooks2026-05-01T07:59:41.831Zhttps://docs.openclaw.ai/id/plugins/manifest2026-05-01T07:59:41.834Zhttps://docs.openclaw.ai/id/plugins/memory-lancedb2026-05-01T07:59:41.832Zhttps://docs.openclaw.ai/id/plugins/memory-wiki2026-05-01T07:59:41.831Zhttps://docs.openclaw.ai/id/plugins/message-presentation2026-05-01T07:59:41.824Zhttps://docs.openclaw.ai/id/plugins/sdk-agent-harness2026-05-01T07:59:41.823Zhttps://docs.openclaw.ai/id/plugins/sdk-channel-plugins2026-05-01T07:59:41.824Zhttps://docs.openclaw.ai/id/plugins/sdk-channel-turn2026-05-01T07:59:41.822Zhttps://docs.openclaw.ai/id/plugins/sdk-entrypoints2026-05-01T07:59:41.879Zhttps://docs.openclaw.ai/id/plugins/sdk-migration2026-05-01T07:59:41.881Zhttps://docs.openclaw.ai/id/plugins/sdk-overview2026-05-01T07:59:41.860Zhttps://docs.openclaw.ai/id/plugins/sdk-provider-plugins2026-05-01T09:33:12.792Zhttps://docs.openclaw.ai/id/plugins/sdk-runtime2026-05-01T07:59:41.867Zhttps://docs.openclaw.ai/id/plugins/sdk-setup2026-05-01T07:59:41.881Zhttps://docs.openclaw.ai/id/plugins/sdk-subpaths2026-05-01T07:59:41.880Zhttps://docs.openclaw.ai/id/plugins/sdk-testing2026-05-01T07:59:41.859Zhttps://docs.openclaw.ai/id/plugins/skill-workshop2026-05-01T07:59:41.860Zhttps://docs.openclaw.ai/id/plugins/voice-call2026-05-01T09:33:12.789Zhttps://docs.openclaw.ai/id/plugins/webhooks2026-05-01T07:59:41.910Zhttps://docs.openclaw.ai/id/plugins/zalouser2026-05-01T07:59:41.910Zhttps://docs.openclaw.ai/id/prose2026-05-01T07:59:41.902Zhttps://docs.openclaw.ai/id/providers/alibaba2026-05-01T07:59:41.911Zhttps://docs.openclaw.ai/id/providers/anthropic2026-05-01T07:59:41.909Zhttps://docs.openclaw.ai/id/providers/arcee2026-05-01T07:59:41.902Zhttps://docs.openclaw.ai/id/providers/azure-speech2026-05-01T07:59:41.906Zhttps://docs.openclaw.ai/id/providers/bedrock2026-05-01T07:59:41.903Zhttps://docs.openclaw.ai/id/providers/bedrock-mantle2026-05-01T07:59:41.903Zhttps://docs.openclaw.ai/id/providers/chutes2026-05-01T07:59:41.945Zhttps://docs.openclaw.ai/id/providers/claude-max-api-proxy2026-05-01T07:59:41.942Zhttps://docs.openclaw.ai/id/providers/cloudflare-ai-gateway2026-05-01T07:59:41.943Zhttps://docs.openclaw.ai/id/providers/comfy2026-05-01T07:59:41.933Zhttps://docs.openclaw.ai/id/providers/deepgram2026-05-01T07:59:41.940Zhttps://docs.openclaw.ai/id/providers/deepinfra2026-05-01T07:59:41.932Zhttps://docs.openclaw.ai/id/providers/deepseek2026-05-01T07:59:41.933Zhttps://docs.openclaw.ai/id/providers/elevenlabs2026-05-01T07:59:41.931Zhttps://docs.openclaw.ai/id/providers/fal2026-05-01T07:59:41.939Zhttps://docs.openclaw.ai/id/providers/fireworks2026-05-01T07:59:41.931Zhttps://docs.openclaw.ai/id/providers/github-copilot2026-05-01T07:59:42.093Zhttps://docs.openclaw.ai/id/providers/glm2026-05-01T07:59:42.085Zhttps://docs.openclaw.ai/id/providers/google2026-05-01T07:59:42.066Zhttps://docs.openclaw.ai/id/providers/gradium2026-05-01T07:59:42.088Zhttps://docs.openclaw.ai/id/providers/groq2026-05-01T07:59:42.015Zhttps://docs.openclaw.ai/id/providers/huggingface2026-05-01T07:59:42.027Zhttps://docs.openclaw.ai/id/providers2026-05-01T07:59:42.026Zhttps://docs.openclaw.ai/id/providers/inferrs2026-05-01T07:59:42.028Zhttps://docs.openclaw.ai/id/providers/inworld2026-05-01T07:59:42.013Zhttps://docs.openclaw.ai/id/providers/kilocode2026-05-01T07:59:42.012Zhttps://docs.openclaw.ai/id/providers/litellm2026-05-01T07:59:42.649Zhttps://docs.openclaw.ai/id/providers/lmstudio2026-05-01T07:59:42.352Zhttps://docs.openclaw.ai/id/providers/minimax2026-05-01T07:59:42.480Zhttps://docs.openclaw.ai/id/providers/mistral2026-05-01T07:59:42.485Zhttps://docs.openclaw.ai/id/providers/models2026-05-01T07:59:42.409Zhttps://docs.openclaw.ai/id/providers/moonshot2026-05-01T07:59:42.455Zhttps://docs.openclaw.ai/id/providers/nvidia2026-05-01T07:59:42.415Zhttps://docs.openclaw.ai/id/providers/ollama2026-05-01T07:59:42.665Zhttps://docs.openclaw.ai/id/providers/openai2026-05-01T07:59:42.449Zhttps://docs.openclaw.ai/id/providers/opencode2026-05-01T07:59:43.268Zhttps://docs.openclaw.ai/id/providers/opencode-go2026-05-01T07:59:42.382Zhttps://docs.openclaw.ai/id/providers/openrouter2026-05-01T07:59:43.487Zhttps://docs.openclaw.ai/id/providers/perplexity-provider2026-05-01T07:59:43.322Zhttps://docs.openclaw.ai/id/providers/qianfan2026-05-01T07:59:43.338Zhttps://docs.openclaw.ai/id/providers/qwen2026-05-01T07:59:43.294Zhttps://docs.openclaw.ai/id/providers/runway2026-05-01T07:59:43.274Zhttps://docs.openclaw.ai/id/providers/sglang2026-05-01T07:59:43.318Zhttps://docs.openclaw.ai/id/providers/stepfun2026-05-01T07:59:43.220Zhttps://docs.openclaw.ai/id/providers/synthetic2026-05-01T07:59:43.247Zhttps://docs.openclaw.ai/id/providers/tencent2026-05-01T07:59:44.830Zhttps://docs.openclaw.ai/id/providers/together2026-05-01T07:59:44.775Zhttps://docs.openclaw.ai/id/providers/venice2026-05-01T07:59:44.770Zhttps://docs.openclaw.ai/id/providers/vercel-ai-gateway2026-05-01T07:59:44.515Zhttps://docs.openclaw.ai/id/providers/vllm2026-05-01T07:59:44.530Zhttps://docs.openclaw.ai/id/providers/volcengine2026-05-01T07:59:44.510Zhttps://docs.openclaw.ai/id/providers/vydra2026-05-01T07:59:44.497Zhttps://docs.openclaw.ai/id/providers/xai2026-05-01T07:59:44.746Zhttps://docs.openclaw.ai/id/providers/xiaomi2026-05-01T07:59:44.492Zhttps://docs.openclaw.ai/id/providers/zai2026-05-01T07:59:44.465Zhttps://docs.openclaw.ai/id/reference/AGENTS.default2026-05-01T07:59:45.443Zhttps://docs.openclaw.ai/id/reference/RELEASING2026-05-01T09:33:12.784Zhttps://docs.openclaw.ai/id/reference/api-usage-costs2026-05-01T07:59:45.442Zhttps://docs.openclaw.ai/id/reference/credits2026-05-01T07:59:45.435Zhttps://docs.openclaw.ai/id/reference/device-models2026-05-01T07:59:45.424Zhttps://docs.openclaw.ai/id/reference/full-release-validation2026-05-01T09:33:12.783Zhttps://docs.openclaw.ai/id/reference/memory-config2026-05-01T07:59:45.437Zhttps://docs.openclaw.ai/id/reference/openclaw-sdk-api-design2026-05-01T07:59:45.436Zhttps://docs.openclaw.ai/id/reference/prompt-caching2026-05-01T07:59:45.422Zhttps://docs.openclaw.ai/id/reference/rich-output-protocol2026-05-01T07:59:45.445Zhttps://docs.openclaw.ai/id/reference/rpc2026-05-01T07:59:45.513Zhttps://docs.openclaw.ai/id/reference/secretref-credential-surface2026-05-01T09:33:12.784Zhttps://docs.openclaw.ai/id/reference/session-management-compaction2026-05-01T07:59:45.515Zhttps://docs.openclaw.ai/id/reference/templates/AGENTS2026-05-01T07:59:45.501Zhttps://docs.openclaw.ai/id/reference/templates/BOOT2026-05-01T07:59:45.500Zhttps://docs.openclaw.ai/id/reference/templates/BOOTSTRAP2026-05-01T07:59:45.507Zhttps://docs.openclaw.ai/id/reference/templates/HEARTBEAT2026-05-01T07:59:45.507Zhttps://docs.openclaw.ai/id/reference/templates/IDENTITY2026-05-01T07:59:45.601Zhttps://docs.openclaw.ai/id/reference/templates/SOUL2026-05-01T07:59:45.596Zhttps://docs.openclaw.ai/id/reference/templates/TOOLS2026-05-01T07:59:45.575Zhttps://docs.openclaw.ai/id/reference/templates/USER2026-05-01T07:59:45.581Zhttps://docs.openclaw.ai/id/reference/test2026-05-01T09:33:12.789Zhttps://docs.openclaw.ai/id/reference/token-use2026-05-01T07:59:45.574Zhttps://docs.openclaw.ai/id/reference/transcript-hygiene2026-05-01T07:59:45.573Zhttps://docs.openclaw.ai/id/reference/wizard2026-05-01T07:59:45.670Zhttps://docs.openclaw.ai/id/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:45.675Zhttps://docs.openclaw.ai/id/security/THREAT-MODEL-ATLAS2026-05-01T07:59:45.697Zhttps://docs.openclaw.ai/id/security/formal-verification2026-05-01T07:59:45.664Zhttps://docs.openclaw.ai/id/security/network-proxy2026-05-01T09:33:12.782Zhttps://docs.openclaw.ai/id/start/bootstrapping2026-05-01T07:59:45.667Zhttps://docs.openclaw.ai/id/start/docs-directory2026-05-01T07:59:45.667Zhttps://docs.openclaw.ai/id/start/getting-started2026-05-01T07:59:45.666Zhttps://docs.openclaw.ai/id/start/hubs2026-05-01T07:59:45.665Zhttps://docs.openclaw.ai/id/start/lore2026-05-01T07:59:45.665Zhttps://docs.openclaw.ai/id/start/onboarding2026-05-01T07:59:45.762Zhttps://docs.openclaw.ai/id/start/onboarding-overview2026-05-01T07:59:45.763Zhttps://docs.openclaw.ai/id/start/openclaw2026-05-01T07:59:45.753Zhttps://docs.openclaw.ai/id/start/setup2026-05-01T07:59:45.738Zhttps://docs.openclaw.ai/id/start/showcase2026-04-24T17:33:09.199Zhttps://docs.openclaw.ai/id/start/wizard2026-05-01T07:59:45.730Zhttps://docs.openclaw.ai/id/start/wizard-cli-automation2026-05-01T07:59:45.740Zhttps://docs.openclaw.ai/id/start/wizard-cli-reference2026-05-01T07:59:45.737Zhttps://docs.openclaw.ai/id/tools/acp-agents2026-05-01T09:33:12.785Zhttps://docs.openclaw.ai/id/tools/acp-agents-setup2026-05-01T07:59:45.834Zhttps://docs.openclaw.ai/id/tools/agent-send2026-05-01T07:59:45.830Zhttps://docs.openclaw.ai/id/tools/apply-patch2026-05-01T07:59:45.814Zhttps://docs.openclaw.ai/id/tools/brave-search2026-05-01T07:59:45.833Zhttps://docs.openclaw.ai/id/tools/browser2026-05-01T07:59:45.831Zhttps://docs.openclaw.ai/id/tools/browser-control2026-05-01T07:59:45.810Zhttps://docs.openclaw.ai/id/tools/browser-linux-troubleshooting2026-05-01T07:59:45.811Zhttps://docs.openclaw.ai/id/tools/browser-login2026-05-01T07:59:45.810Zhttps://docs.openclaw.ai/id/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:45.805Zhttps://docs.openclaw.ai/id/tools/btw2026-05-01T07:59:45.934Zhttps://docs.openclaw.ai/id/tools/clawhub2026-05-01T07:59:45.921Zhttps://docs.openclaw.ai/id/tools/code-execution2026-05-01T07:59:45.902Zhttps://docs.openclaw.ai/id/tools/creating-skills2026-05-01T07:59:45.906Zhttps://docs.openclaw.ai/id/tools/diffs2026-05-01T07:59:45.905Zhttps://docs.openclaw.ai/id/tools/duckduckgo-search2026-05-01T07:59:45.905Zhttps://docs.openclaw.ai/id/tools/elevated2026-05-01T07:59:45.904Zhttps://docs.openclaw.ai/id/tools/exa-search2026-05-01T07:59:45.916Zhttps://docs.openclaw.ai/id/tools/exec2026-05-01T07:59:46.007Zhttps://docs.openclaw.ai/id/tools/exec-approvals2026-05-01T07:59:46.016Zhttps://docs.openclaw.ai/id/tools/exec-approvals-advanced2026-05-01T07:59:45.903Zhttps://docs.openclaw.ai/id/tools/firecrawl2026-05-01T07:59:45.998Zhttps://docs.openclaw.ai/id/tools/gemini-search2026-05-01T07:59:45.998Zhttps://docs.openclaw.ai/id/tools/grok-search2026-05-01T07:59:45.999Zhttps://docs.openclaw.ai/id/tools/image-generation2026-05-01T07:59:46.006Zhttps://docs.openclaw.ai/id/tools2026-05-01T07:59:45.996Zhttps://docs.openclaw.ai/id/tools/kimi-search2026-05-01T07:59:45.997Zhttps://docs.openclaw.ai/id/tools/llm-task2026-05-01T07:59:45.975Zhttps://docs.openclaw.ai/id/tools/lobster2026-05-01T07:59:46.005Zhttps://docs.openclaw.ai/id/tools/loop-detection2026-05-01T07:59:46.125Zhttps://docs.openclaw.ai/id/tools/media-overview2026-05-01T07:59:46.109Zhttps://docs.openclaw.ai/id/tools/minimax-search2026-05-01T07:59:46.124Zhttps://docs.openclaw.ai/id/tools/multi-agent-sandbox-tools2026-05-01T07:59:46.075Zhttps://docs.openclaw.ai/id/tools/music-generation2026-05-01T07:59:46.092Zhttps://docs.openclaw.ai/id/tools/ollama-search2026-05-01T07:59:46.092Zhttps://docs.openclaw.ai/id/tools/pdf2026-05-01T07:59:46.136Zhttps://docs.openclaw.ai/id/tools/perplexity-search2026-05-01T07:59:46.074Zhttps://docs.openclaw.ai/id/tools/plugin2026-05-01T09:33:12.787Zhttps://docs.openclaw.ai/id/tools/reactions2026-05-01T07:59:46.073Zhttps://docs.openclaw.ai/id/tools/searxng-search2026-05-01T07:59:46.224Zhttps://docs.openclaw.ai/id/tools/skills2026-05-01T07:59:46.276Zhttps://docs.openclaw.ai/id/tools/skills-config2026-05-01T07:59:46.180Zhttps://docs.openclaw.ai/id/tools/slash-commands2026-05-01T07:59:46.275Zhttps://docs.openclaw.ai/id/tools/subagents2026-05-01T07:59:46.205Zhttps://docs.openclaw.ai/id/tools/tavily2026-05-01T07:59:46.226Zhttps://docs.openclaw.ai/id/tools/thinking2026-05-01T07:59:46.225Zhttps://docs.openclaw.ai/id/tools/tokenjuice2026-05-01T07:59:46.204Zhttps://docs.openclaw.ai/id/tools/trajectory2026-05-01T07:59:46.204Zhttps://docs.openclaw.ai/id/tools/tts2026-05-01T07:59:46.232Zhttps://docs.openclaw.ai/id/tools/video-generation2026-05-01T07:59:46.335Zhttps://docs.openclaw.ai/id/tools/web2026-05-01T07:59:46.326Zhttps://docs.openclaw.ai/id/tools/web-fetch2026-05-01T07:59:46.327Zhttps://docs.openclaw.ai/id/vps2026-05-01T07:59:46.325Zhttps://docs.openclaw.ai/id/web/control-ui2026-05-01T07:59:46.334Zhttps://docs.openclaw.ai/id/web/dashboard2026-05-01T07:59:46.326Zhttps://docs.openclaw.ai/id/web2026-05-01T07:59:46.324Zhttps://docs.openclaw.ai/id/web/tui2026-05-01T07:59:46.324Zhttps://docs.openclaw.ai/id/web/webchat2026-05-01T07:59:46.323Zhttps://docs.openclaw.ai2026-05-01T07:59:46.410Zhttps://docs.openclaw.ai/install/ansible2026-05-01T11:03:07.118Zhttps://docs.openclaw.ai/install/azure2026-05-01T07:59:46.403Zhttps://docs.openclaw.ai/install/bun2026-05-01T07:59:46.409Zhttps://docs.openclaw.ai/install/clawdock2026-05-01T07:59:46.393Zhttps://docs.openclaw.ai/install/development-channels2026-05-01T07:59:46.394Zhttps://docs.openclaw.ai/install/digitalocean2026-05-01T07:59:46.393Zhttps://docs.openclaw.ai/install/docker2026-05-01T11:03:07.122Zhttps://docs.openclaw.ai/install/docker-vm-runtime2026-05-01T07:59:46.407Zhttps://docs.openclaw.ai/install/exe-dev2026-05-01T07:59:46.391Zhttps://docs.openclaw.ai/install/fly2026-05-01T07:59:46.474Zhttps://docs.openclaw.ai/install/gcp2026-05-01T07:59:46.514Zhttps://docs.openclaw.ai/install/hetzner2026-05-01T07:59:46.472Zhttps://docs.openclaw.ai/install/hostinger2026-05-01T07:59:46.470Zhttps://docs.openclaw.ai/install2026-05-01T07:59:46.500Zhttps://docs.openclaw.ai/install/installer2026-05-01T07:59:46.471Zhttps://docs.openclaw.ai/install/kubernetes2026-05-01T07:59:46.471Zhttps://docs.openclaw.ai/install/macos-vm2026-05-01T07:59:46.469Zhttps://docs.openclaw.ai/install/migrating2026-05-01T07:59:46.602Zhttps://docs.openclaw.ai/install/migrating-claude2026-05-01T07:59:46.469Zhttps://docs.openclaw.ai/install/migrating-hermes2026-05-01T07:59:46.468Zhttps://docs.openclaw.ai/install/nix2026-05-01T07:59:46.600Zhttps://docs.openclaw.ai/install/node2026-05-01T07:59:46.575Zhttps://docs.openclaw.ai/install/northflank2026-05-01T07:59:46.598Zhttps://docs.openclaw.ai/install/oracle2026-05-01T07:59:46.574Zhttps://docs.openclaw.ai/install/podman2026-05-01T07:59:46.596Zhttps://docs.openclaw.ai/install/railway2026-05-01T07:59:46.572Zhttps://docs.openclaw.ai/install/raspberry-pi2026-05-01T07:59:46.573Zhttps://docs.openclaw.ai/install/render2026-05-01T07:59:46.570Zhttps://docs.openclaw.ai/install/uninstall2026-05-01T07:59:46.573Zhttps://docs.openclaw.ai/install/updating2026-05-01T08:58:38.080Zhttps://docs.openclaw.ai/it/auth-credential-semantics2026-05-01T07:59:46.662Zhttps://docs.openclaw.ai/it/automation/cron-jobs2026-05-01T07:59:46.651Zhttps://docs.openclaw.ai/it/automation/hooks2026-05-01T07:59:46.652Zhttps://docs.openclaw.ai/it/automation2026-05-01T07:59:46.652Zhttps://docs.openclaw.ai/it/automation/standing-orders2026-05-01T07:59:46.738Zhttps://docs.openclaw.ai/it/automation/taskflow2026-05-01T07:59:46.744Zhttps://docs.openclaw.ai/it/automation/tasks2026-05-01T08:38:21.691Zhttps://docs.openclaw.ai/it/channels/bluebubbles2026-05-01T08:38:21.689Zhttps://docs.openclaw.ai/it/channels/broadcast-groups2026-05-01T07:59:46.752Zhttps://docs.openclaw.ai/it/channels/channel-routing2026-05-01T07:59:46.706Zhttps://docs.openclaw.ai/it/channels/discord2026-05-01T07:59:46.893Zhttps://docs.openclaw.ai/it/channels/feishu2026-05-01T07:59:46.859Zhttps://docs.openclaw.ai/it/channels/googlechat2026-05-01T07:59:46.891Zhttps://docs.openclaw.ai/it/channels/group-messages2026-05-01T07:59:46.892Zhttps://docs.openclaw.ai/it/channels/groups2026-05-01T08:38:21.688Zhttps://docs.openclaw.ai/it/channels/imessage2026-05-01T07:59:46.861Zhttps://docs.openclaw.ai/it/channels2026-05-01T07:59:46.862Zhttps://docs.openclaw.ai/it/channels/irc2026-05-01T07:59:46.893Zhttps://docs.openclaw.ai/it/channels/line2026-05-01T07:59:46.862Zhttps://docs.openclaw.ai/it/channels/location2026-05-01T07:59:46.859Zhttps://docs.openclaw.ai/it/channels/matrix2026-05-01T07:59:46.952Zhttps://docs.openclaw.ai/it/channels/matrix-migration2026-05-01T07:59:46.969Zhttps://docs.openclaw.ai/it/channels/matrix-push-rules2026-05-01T07:59:46.956Zhttps://docs.openclaw.ai/it/channels/mattermost2026-05-01T07:59:46.966Zhttps://docs.openclaw.ai/it/channels/msteams2026-05-01T07:59:46.950Zhttps://docs.openclaw.ai/it/channels/nextcloud-talk2026-05-01T07:59:46.951Zhttps://docs.openclaw.ai/it/channels/nostr2026-05-01T07:59:46.952Zhttps://docs.openclaw.ai/it/channels/pairing2026-05-01T07:59:46.953Zhttps://docs.openclaw.ai/it/channels/qa-channel2026-05-01T08:38:21.687Zhttps://docs.openclaw.ai/it/channels/qqbot2026-05-01T07:59:46.938Zhttps://docs.openclaw.ai/it/channels/signal2026-05-01T07:59:47.034Zhttps://docs.openclaw.ai/it/channels/slack2026-05-01T07:59:47.030Zhttps://docs.openclaw.ai/it/channels/synology-chat2026-05-01T07:59:47.034Zhttps://docs.openclaw.ai/it/channels/telegram2026-05-01T07:59:47.026Zhttps://docs.openclaw.ai/it/channels/tlon2026-05-01T07:59:47.027Zhttps://docs.openclaw.ai/it/channels/troubleshooting2026-05-01T07:59:47.028Zhttps://docs.openclaw.ai/it/channels/twitch2026-05-01T07:59:47.027Zhttps://docs.openclaw.ai/it/channels/wechat2026-05-01T07:59:47.026Zhttps://docs.openclaw.ai/it/channels/whatsapp2026-05-01T07:59:47.006Zhttps://docs.openclaw.ai/it/channels/yuanbao2026-05-01T07:59:47.031Zhttps://docs.openclaw.ai/it/channels/zalo2026-05-01T07:59:47.108Zhttps://docs.openclaw.ai/it/channels/zalouser2026-05-01T07:59:47.107Zhttps://docs.openclaw.ai/it/ci2026-05-01T08:38:21.696Zhttps://docs.openclaw.ai/it/cli/acp2026-05-01T07:59:47.105Zhttps://docs.openclaw.ai/it/cli/agent2026-05-01T07:59:47.103Zhttps://docs.openclaw.ai/it/cli/agents2026-05-01T07:59:47.100Zhttps://docs.openclaw.ai/it/cli/approvals2026-05-01T07:59:47.101Zhttps://docs.openclaw.ai/it/cli/backup2026-05-01T07:59:47.101Zhttps://docs.openclaw.ai/it/cli/browser2026-05-01T07:59:47.100Zhttps://docs.openclaw.ai/it/cli/channels2026-05-01T08:38:21.689Zhttps://docs.openclaw.ai/it/cli/clawbot2026-05-01T07:59:47.144Zhttps://docs.openclaw.ai/it/cli/commitments2026-05-01T07:59:47.139Zhttps://docs.openclaw.ai/it/cli/completion2026-05-01T07:59:47.142Zhttps://docs.openclaw.ai/it/cli/config2026-05-01T07:59:47.143Zhttps://docs.openclaw.ai/it/cli/configure2026-05-01T08:38:21.685Zhttps://docs.openclaw.ai/it/cli/cron2026-05-01T07:59:47.135Zhttps://docs.openclaw.ai/it/cli/daemon2026-05-01T07:59:47.136Zhttps://docs.openclaw.ai/it/cli/dashboard2026-05-01T07:59:47.144Zhttps://docs.openclaw.ai/it/cli/devices2026-05-01T07:59:47.135Zhttps://docs.openclaw.ai/it/cli/directory2026-05-01T07:59:47.166Zhttps://docs.openclaw.ai/it/cli/dns2026-05-01T07:59:47.173Zhttps://docs.openclaw.ai/it/cli/docs2026-05-01T07:59:47.164Zhttps://docs.openclaw.ai/it/cli/doctor2026-05-01T07:59:47.172Zhttps://docs.openclaw.ai/it/cli/flows2026-05-01T07:59:47.172Zhttps://docs.openclaw.ai/it/cli/gateway2026-05-01T08:38:21.692Zhttps://docs.openclaw.ai/it/cli/health2026-05-01T07:59:47.171Zhttps://docs.openclaw.ai/it/cli/hooks2026-05-01T07:59:47.165Zhttps://docs.openclaw.ai/it/cli2026-05-01T07:59:47.165Zhttps://docs.openclaw.ai/it/cli/infer2026-05-01T07:59:47.164Zhttps://docs.openclaw.ai/it/cli/logs2026-05-01T07:59:47.201Zhttps://docs.openclaw.ai/it/cli/mcp2026-05-01T07:59:47.200Zhttps://docs.openclaw.ai/it/cli/memory2026-05-01T07:59:47.201Zhttps://docs.openclaw.ai/it/cli/message2026-05-01T07:59:47.194Zhttps://docs.openclaw.ai/it/cli/migrate2026-05-01T07:59:47.199Zhttps://docs.openclaw.ai/it/cli/models2026-05-01T08:38:21.686Zhttps://docs.openclaw.ai/it/cli/node2026-05-01T07:59:47.193Zhttps://docs.openclaw.ai/it/cli/nodes2026-05-01T07:59:47.193Zhttps://docs.openclaw.ai/it/cli/onboard2026-05-01T08:38:21.693Zhttps://docs.openclaw.ai/it/cli/pairing2026-05-01T07:59:47.195Zhttps://docs.openclaw.ai/it/cli/plugins2026-05-01T08:38:24.197Zhttps://docs.openclaw.ai/it/cli/proxy2026-05-01T08:38:24.192Zhttps://docs.openclaw.ai/it/cli/qr2026-05-01T07:59:47.240Zhttps://docs.openclaw.ai/it/cli/reset2026-05-01T07:59:47.222Zhttps://docs.openclaw.ai/it/cli/sandbox2026-05-01T07:59:47.239Zhttps://docs.openclaw.ai/it/cli/secrets2026-05-01T07:59:47.240Zhttps://docs.openclaw.ai/it/cli/security2026-05-01T07:59:47.221Zhttps://docs.openclaw.ai/it/cli/sessions2026-05-01T07:59:47.228Zhttps://docs.openclaw.ai/it/cli/setup2026-05-01T07:59:47.223Zhttps://docs.openclaw.ai/it/cli/skills2026-05-01T07:59:47.222Zhttps://docs.openclaw.ai/it/cli/status2026-05-01T07:59:47.269Zhttps://docs.openclaw.ai/it/cli/system2026-05-01T07:59:47.268Zhttps://docs.openclaw.ai/it/cli/tasks2026-05-01T07:59:47.267Zhttps://docs.openclaw.ai/it/cli/tui2026-05-01T07:59:47.260Zhttps://docs.openclaw.ai/it/cli/uninstall2026-05-01T07:59:47.267Zhttps://docs.openclaw.ai/it/cli/update2026-05-01T07:59:47.261Zhttps://docs.openclaw.ai/it/cli/voicecall2026-05-01T08:38:24.189Zhttps://docs.openclaw.ai/it/cli/webhooks2026-05-01T07:59:47.268Zhttps://docs.openclaw.ai/it/cli/wiki2026-05-01T07:59:47.259Zhttps://docs.openclaw.ai/it/concepts/active-memory2026-05-01T07:59:47.265Zhttps://docs.openclaw.ai/it/concepts/agent2026-05-01T07:59:47.298Zhttps://docs.openclaw.ai/it/concepts/agent-loop2026-05-01T07:59:47.298Zhttps://docs.openclaw.ai/it/concepts/agent-runtimes2026-05-01T07:59:47.297Zhttps://docs.openclaw.ai/it/concepts/agent-workspace2026-05-01T07:59:47.289Zhttps://docs.openclaw.ai/it/concepts/architecture2026-05-01T07:59:47.297Zhttps://docs.openclaw.ai/it/concepts/channel-docking2026-05-01T07:59:47.295Zhttps://docs.openclaw.ai/it/concepts/commitments2026-05-01T08:38:24.193Zhttps://docs.openclaw.ai/it/concepts/compaction2026-05-01T07:59:47.296Zhttps://docs.openclaw.ai/it/concepts/context2026-05-01T07:59:47.288Zhttps://docs.openclaw.ai/it/concepts/context-engine2026-05-01T07:59:47.289Zhttps://docs.openclaw.ai/it/concepts/delegate-architecture2026-05-01T07:59:47.337Zhttps://docs.openclaw.ai/it/concepts/dreaming2026-05-01T07:59:47.336Zhttps://docs.openclaw.ai/it/concepts/experimental-features2026-05-01T07:59:47.325Zhttps://docs.openclaw.ai/it/concepts/features2026-05-01T07:59:47.326Zhttps://docs.openclaw.ai/it/concepts/markdown-formatting2026-05-01T07:59:47.324Zhttps://docs.openclaw.ai/it/concepts/memory2026-05-01T07:59:47.316Zhttps://docs.openclaw.ai/it/concepts/memory-builtin2026-05-01T07:59:47.322Zhttps://docs.openclaw.ai/it/concepts/memory-honcho2026-05-01T07:59:47.317Zhttps://docs.openclaw.ai/it/concepts/memory-qmd2026-05-01T07:59:47.324Zhttps://docs.openclaw.ai/it/concepts/memory-search2026-05-01T07:59:47.317Zhttps://docs.openclaw.ai/it/concepts/messages2026-05-01T07:59:47.367Zhttps://docs.openclaw.ai/it/concepts/model-failover2026-05-01T07:59:47.366Zhttps://docs.openclaw.ai/it/concepts/model-providers2026-05-01T07:59:47.364Zhttps://docs.openclaw.ai/it/concepts/models2026-05-01T07:59:47.364Zhttps://docs.openclaw.ai/it/concepts/multi-agent2026-05-01T07:59:47.356Zhttps://docs.openclaw.ai/it/concepts/oauth2026-05-01T07:59:47.356Zhttps://docs.openclaw.ai/it/concepts/openclaw-sdk2026-05-01T08:38:24.190Zhttps://docs.openclaw.ai/it/concepts/presence2026-05-01T07:59:47.357Zhttps://docs.openclaw.ai/it/concepts/qa-e2e-automation2026-05-01T07:59:47.365Zhttps://docs.openclaw.ai/it/concepts/qa-matrix2026-05-01T07:59:47.355Zhttps://docs.openclaw.ai/it/concepts/queue2026-05-01T07:59:47.395Zhttps://docs.openclaw.ai/it/concepts/queue-steering2026-05-01T07:59:47.396Zhttps://docs.openclaw.ai/it/concepts/retry2026-05-01T07:59:47.394Zhttps://docs.openclaw.ai/it/concepts/session2026-05-01T07:59:47.391Zhttps://docs.openclaw.ai/it/concepts/session-pruning2026-05-01T07:59:47.387Zhttps://docs.openclaw.ai/it/concepts/session-tool2026-05-01T07:59:47.394Zhttps://docs.openclaw.ai/it/concepts/soul2026-05-01T07:59:47.386Zhttps://docs.openclaw.ai/it/concepts/streaming2026-05-01T07:59:47.393Zhttps://docs.openclaw.ai/it/concepts/system-prompt2026-05-01T07:59:47.386Zhttps://docs.openclaw.ai/it/concepts/timezone2026-05-01T07:59:47.385Zhttps://docs.openclaw.ai/it/concepts/typebox2026-05-01T07:59:47.423Zhttps://docs.openclaw.ai/it/concepts/typing-indicators2026-05-01T07:59:47.424Zhttps://docs.openclaw.ai/it/concepts/usage-tracking2026-05-01T07:59:47.422Zhttps://docs.openclaw.ai/it/date-time2026-05-01T07:59:47.421Zhttps://docs.openclaw.ai/it/debug/node-issue2026-05-01T07:59:47.414Zhttps://docs.openclaw.ai/it/diagnostics/flags2026-05-01T07:59:47.420Zhttps://docs.openclaw.ai/it/gateway/authentication2026-05-01T07:59:47.422Zhttps://docs.openclaw.ai/it/gateway/background-process2026-05-01T07:59:47.414Zhttps://docs.openclaw.ai/it/gateway/bonjour2026-05-01T07:59:47.423Zhttps://docs.openclaw.ai/it/gateway/bridge-protocol2026-05-01T07:59:47.413Zhttps://docs.openclaw.ai/it/gateway/cli-backends2026-05-01T07:59:47.452Zhttps://docs.openclaw.ai/it/gateway/config-agents2026-05-01T07:59:47.461Zhttps://docs.openclaw.ai/it/gateway/config-channels2026-05-01T08:38:24.194Zhttps://docs.openclaw.ai/it/gateway/config-tools2026-05-01T08:38:24.196Zhttps://docs.openclaw.ai/it/gateway/configuration2026-05-01T07:59:47.467Zhttps://docs.openclaw.ai/it/gateway/configuration-examples2026-05-01T07:59:47.463Zhttps://docs.openclaw.ai/it/gateway/configuration-reference2026-05-01T07:59:47.460Zhttps://docs.openclaw.ai/it/gateway/diagnostics2026-05-01T07:59:47.453Zhttps://docs.openclaw.ai/it/gateway/discovery2026-05-01T07:59:47.453Zhttps://docs.openclaw.ai/it/gateway/doctor2026-05-01T08:38:24.190Zhttps://docs.openclaw.ai/it/gateway/gateway-lock2026-05-01T07:59:47.496Zhttps://docs.openclaw.ai/it/gateway/health2026-05-01T07:59:47.495Zhttps://docs.openclaw.ai/it/gateway/heartbeat2026-05-01T07:59:47.487Zhttps://docs.openclaw.ai/it/gateway2026-05-01T07:59:47.496Zhttps://docs.openclaw.ai/it/gateway/local-models2026-05-01T07:59:47.493Zhttps://docs.openclaw.ai/it/gateway/logging2026-05-01T08:38:24.188Zhttps://docs.openclaw.ai/it/gateway/multiple-gateways2026-05-01T07:59:47.488Zhttps://docs.openclaw.ai/it/gateway/network-model2026-05-01T07:59:47.487Zhttps://docs.openclaw.ai/it/gateway/openai-http-api2026-05-01T07:59:47.488Zhttps://docs.openclaw.ai/it/gateway/openresponses-http-api2026-05-01T07:59:47.486Zhttps://docs.openclaw.ai/it/gateway/openshell2026-05-01T07:59:47.537Zhttps://docs.openclaw.ai/it/gateway/opentelemetry2026-05-01T07:59:47.524Zhttps://docs.openclaw.ai/it/gateway/pairing2026-05-01T07:59:47.524Zhttps://docs.openclaw.ai/it/gateway/prometheus2026-05-01T07:59:47.525Zhttps://docs.openclaw.ai/it/gateway/protocol2026-05-01T08:38:24.191Zhttps://docs.openclaw.ai/it/gateway/remote2026-05-01T07:59:47.517Zhttps://docs.openclaw.ai/it/gateway/remote-gateway-readme2026-05-01T07:59:47.516Zhttps://docs.openclaw.ai/it/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:47.535Zhttps://docs.openclaw.ai/it/gateway/sandboxing2026-05-01T07:59:47.516Zhttps://docs.openclaw.ai/it/gateway/secrets2026-05-01T07:59:47.568Zhttps://docs.openclaw.ai/it/gateway/secrets-plan-contract2026-05-01T07:59:47.515Zhttps://docs.openclaw.ai/it/gateway/security/audit-checks2026-05-01T07:59:47.558Zhttps://docs.openclaw.ai/it/gateway/security2026-04-30T09:22:30.084Zhttps://docs.openclaw.ai/it/gateway/tailscale2026-05-01T07:59:47.564Zhttps://docs.openclaw.ai/it/gateway/tools-invoke-http-api2026-05-01T07:59:47.566Zhttps://docs.openclaw.ai/it/gateway/troubleshooting2026-05-01T08:38:26.507Zhttps://docs.openclaw.ai/it/gateway/trusted-proxy-auth2026-05-01T07:59:47.567Zhttps://docs.openclaw.ai/it/help/debugging2026-05-01T07:59:47.558Zhttps://docs.openclaw.ai/it/help/environment2026-05-01T07:59:47.559Zhttps://docs.openclaw.ai/it/help/faq2026-05-01T07:59:47.600Zhttps://docs.openclaw.ai/it/help/faq-first-run2026-05-01T07:59:47.557Zhttps://docs.openclaw.ai/it/help/faq-models2026-05-01T07:59:47.557Zhttps://docs.openclaw.ai/it/help/gpt55-codex-agentic-parity2026-05-01T07:59:47.596Zhttps://docs.openclaw.ai/it/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:47.599Zhttps://docs.openclaw.ai/it/help2026-05-01T07:59:47.599Zhttps://docs.openclaw.ai/it/help/scripts2026-05-01T07:59:47.594Zhttps://docs.openclaw.ai/it/help/testing2026-05-01T08:38:26.535Zhttps://docs.openclaw.ai/it/help/testing-live2026-05-01T07:59:47.589Zhttps://docs.openclaw.ai/it/help/troubleshooting2026-05-01T07:59:47.598Zhttps://docs.openclaw.ai/it2026-05-01T07:59:47.587Zhttps://docs.openclaw.ai/it/install/ansible2026-05-01T07:59:47.587Zhttps://docs.openclaw.ai/it/install/azure2026-05-01T07:59:47.640Zhttps://docs.openclaw.ai/it/install/bun2026-05-01T07:59:47.627Zhttps://docs.openclaw.ai/it/install/clawdock2026-05-01T07:59:47.626Zhttps://docs.openclaw.ai/it/install/development-channels2026-05-01T07:59:47.619Zhttps://docs.openclaw.ai/it/install/digitalocean2026-05-01T07:59:47.628Zhttps://docs.openclaw.ai/it/install/docker2026-05-01T07:59:47.625Zhttps://docs.openclaw.ai/it/install/docker-vm-runtime2026-05-01T07:59:47.627Zhttps://docs.openclaw.ai/it/install/exe-dev2026-05-01T07:59:47.628Zhttps://docs.openclaw.ai/it/install/fly2026-05-01T07:59:47.619Zhttps://docs.openclaw.ai/it/install/gcp2026-05-01T07:59:47.618Zhttps://docs.openclaw.ai/it/install/hetzner2026-05-01T07:59:47.670Zhttps://docs.openclaw.ai/it/install/hostinger2026-05-01T07:59:47.660Zhttps://docs.openclaw.ai/it/install2026-05-01T07:59:47.662Zhttps://docs.openclaw.ai/it/install/installer2026-05-01T07:59:47.661Zhttps://docs.openclaw.ai/it/install/kubernetes2026-05-01T07:59:47.668Zhttps://docs.openclaw.ai/it/install/macos-vm2026-05-01T07:59:47.669Zhttps://docs.openclaw.ai/it/install/migrating2026-05-01T07:59:47.660Zhttps://docs.openclaw.ai/it/install/migrating-claude2026-05-01T07:59:47.662Zhttps://docs.openclaw.ai/it/install/migrating-hermes2026-05-01T07:59:47.663Zhttps://docs.openclaw.ai/it/install/nix2026-05-01T07:59:47.665Zhttps://docs.openclaw.ai/it/install/node2026-05-01T07:59:47.694Zhttps://docs.openclaw.ai/it/install/northflank2026-05-01T07:59:47.695Zhttps://docs.openclaw.ai/it/install/oracle2026-05-01T07:59:47.697Zhttps://docs.openclaw.ai/it/install/podman2026-05-01T07:59:47.696Zhttps://docs.openclaw.ai/it/install/railway2026-05-01T07:59:47.693Zhttps://docs.openclaw.ai/it/install/raspberry-pi2026-05-01T07:59:47.690Zhttps://docs.openclaw.ai/it/install/render2026-05-01T07:59:47.691Zhttps://docs.openclaw.ai/it/install/uninstall2026-05-01T07:59:47.690Zhttps://docs.openclaw.ai/it/install/updating2026-05-01T07:59:47.689Zhttps://docs.openclaw.ai/it/logging2026-05-01T08:38:26.508Zhttps://docs.openclaw.ai/it/network2026-05-01T07:59:47.716Zhttps://docs.openclaw.ai/it/nodes/audio2026-05-01T07:59:47.723Zhttps://docs.openclaw.ai/it/nodes/camera2026-05-01T07:59:47.726Zhttps://docs.openclaw.ai/it/nodes/images2026-05-01T07:59:47.724Zhttps://docs.openclaw.ai/it/nodes2026-05-01T07:59:47.721Zhttps://docs.openclaw.ai/it/nodes/location-command2026-05-01T07:59:47.724Zhttps://docs.openclaw.ai/it/nodes/media-understanding2026-05-01T07:59:47.723Zhttps://docs.openclaw.ai/it/nodes/talk2026-05-01T07:59:47.716Zhttps://docs.openclaw.ai/it/nodes/troubleshooting2026-05-01T07:59:47.715Zhttps://docs.openclaw.ai/it/nodes/voicewake2026-05-01T07:59:47.714Zhttps://docs.openclaw.ai/it/pi2026-05-01T07:59:47.776Zhttps://docs.openclaw.ai/it/pi-dev2026-05-01T07:59:47.776Zhttps://docs.openclaw.ai/it/platforms/android2026-05-01T07:59:47.773Zhttps://docs.openclaw.ai/it/platforms2026-05-01T07:59:47.770Zhttps://docs.openclaw.ai/it/platforms/ios2026-05-01T07:59:47.768Zhttps://docs.openclaw.ai/it/platforms/linux2026-05-01T07:59:47.767Zhttps://docs.openclaw.ai/it/platforms/mac/bundled-gateway2026-05-01T07:59:47.813Zhttps://docs.openclaw.ai/it/platforms/mac/canvas2026-05-01T07:59:47.812Zhttps://docs.openclaw.ai/it/platforms/mac/child-process2026-05-01T07:59:47.813Zhttps://docs.openclaw.ai/it/platforms/mac/dev-setup2026-05-01T07:59:47.814Zhttps://docs.openclaw.ai/it/platforms/mac/health2026-05-01T07:59:47.809Zhttps://docs.openclaw.ai/it/platforms/mac/icon2026-05-01T07:59:47.815Zhttps://docs.openclaw.ai/it/platforms/mac/logging2026-05-01T07:59:47.806Zhttps://docs.openclaw.ai/it/platforms/mac/menu-bar2026-05-01T08:38:26.506Zhttps://docs.openclaw.ai/it/platforms/mac/peekaboo2026-05-01T07:59:47.805Zhttps://docs.openclaw.ai/it/platforms/mac/permissions2026-05-01T07:59:47.804Zhttps://docs.openclaw.ai/it/platforms/mac/remote2026-05-01T07:59:47.834Zhttps://docs.openclaw.ai/it/platforms/mac/signing2026-05-01T07:59:47.843Zhttps://docs.openclaw.ai/it/platforms/mac/skills2026-05-01T07:59:47.841Zhttps://docs.openclaw.ai/it/platforms/mac/voice-overlay2026-05-01T07:59:47.842Zhttps://docs.openclaw.ai/it/platforms/mac/voicewake2026-05-01T07:59:47.841Zhttps://docs.openclaw.ai/it/platforms/mac/webchat2026-05-01T07:59:47.838Zhttps://docs.openclaw.ai/it/platforms/mac/xpc2026-05-01T07:59:47.834Zhttps://docs.openclaw.ai/it/platforms/macos2026-05-01T07:59:47.840Zhttps://docs.openclaw.ai/it/platforms/windows2026-05-01T07:59:47.875Zhttps://docs.openclaw.ai/it/plugins/architecture2026-05-01T07:59:47.872Zhttps://docs.openclaw.ai/it/plugins/architecture-internals2026-05-01T07:59:47.870Zhttps://docs.openclaw.ai/it/plugins/building-plugins2026-05-01T07:59:47.871Zhttps://docs.openclaw.ai/it/plugins/bundles2026-05-01T07:59:47.863Zhttps://docs.openclaw.ai/it/plugins/codex-computer-use2026-05-01T07:59:47.871Zhttps://docs.openclaw.ai/it/plugins/codex-harness2026-05-01T08:38:26.501Zhttps://docs.openclaw.ai/it/plugins/community2026-05-01T07:59:47.862Zhttps://docs.openclaw.ai/it/plugins/compatibility2026-05-01T07:59:47.916Zhttps://docs.openclaw.ai/it/plugins/dependency-resolution2026-05-01T08:38:26.504Zhttps://docs.openclaw.ai/it/plugins/google-meet2026-05-01T08:38:26.502Zhttps://docs.openclaw.ai/it/plugins/hooks2026-05-01T07:59:47.894Zhttps://docs.openclaw.ai/it/plugins/manifest2026-05-01T07:59:47.913Zhttps://docs.openclaw.ai/it/plugins/memory-lancedb2026-05-01T07:59:47.913Zhttps://docs.openclaw.ai/it/plugins/memory-wiki2026-05-01T07:59:47.912Zhttps://docs.openclaw.ai/it/plugins/message-presentation2026-05-01T07:59:47.914Zhttps://docs.openclaw.ai/it/plugins/sdk-agent-harness2026-05-01T07:59:47.895Zhttps://docs.openclaw.ai/it/plugins/sdk-channel-plugins2026-05-01T07:59:47.894Zhttps://docs.openclaw.ai/it/plugins/sdk-channel-turn2026-05-01T07:59:47.893Zhttps://docs.openclaw.ai/it/plugins/sdk-entrypoints2026-05-01T07:59:47.950Zhttps://docs.openclaw.ai/it/plugins/sdk-migration2026-05-01T07:59:47.948Zhttps://docs.openclaw.ai/it/plugins/sdk-overview2026-05-01T07:59:47.936Zhttps://docs.openclaw.ai/it/plugins/sdk-provider-plugins2026-05-01T08:38:26.505Zhttps://docs.openclaw.ai/it/plugins/sdk-runtime2026-05-01T07:59:47.947Zhttps://docs.openclaw.ai/it/plugins/sdk-setup2026-05-01T07:59:47.943Zhttps://docs.openclaw.ai/it/plugins/sdk-subpaths2026-05-01T07:59:47.949Zhttps://docs.openclaw.ai/it/plugins/sdk-testing2026-05-01T07:59:47.937Zhttps://docs.openclaw.ai/it/plugins/skill-workshop2026-05-01T07:59:47.937Zhttps://docs.openclaw.ai/it/plugins/voice-call2026-05-01T08:38:26.359Zhttps://docs.openclaw.ai/it/plugins/webhooks2026-05-01T07:59:47.979Zhttps://docs.openclaw.ai/it/plugins/zalouser2026-05-01T07:59:47.976Zhttps://docs.openclaw.ai/it/prose2026-05-01T07:59:47.976Zhttps://docs.openclaw.ai/it/providers/alibaba2026-05-01T07:59:47.974Zhttps://docs.openclaw.ai/it/providers/anthropic2026-05-01T07:59:47.978Zhttps://docs.openclaw.ai/it/providers/arcee2026-05-01T07:59:47.977Zhttps://docs.openclaw.ai/it/providers/azure-speech2026-05-01T07:59:47.969Zhttps://docs.openclaw.ai/it/providers/bedrock2026-05-01T07:59:47.970Zhttps://docs.openclaw.ai/it/providers/bedrock-mantle2026-05-01T07:59:47.969Zhttps://docs.openclaw.ai/it/providers/chutes2026-05-01T07:59:48.015Zhttps://docs.openclaw.ai/it/providers/claude-max-api-proxy2026-05-01T07:59:48.014Zhttps://docs.openclaw.ai/it/providers/cloudflare-ai-gateway2026-05-01T07:59:48.017Zhttps://docs.openclaw.ai/it/providers/comfy2026-05-01T07:59:48.016Zhttps://docs.openclaw.ai/it/providers/deepgram2026-05-01T07:59:48.000Zhttps://docs.openclaw.ai/it/providers/deepinfra2026-05-01T07:59:48.003Zhttps://docs.openclaw.ai/it/providers/deepseek2026-05-01T07:59:47.999Zhttps://docs.openclaw.ai/it/providers/elevenlabs2026-05-01T07:59:47.999Zhttps://docs.openclaw.ai/it/providers/fal2026-05-01T07:59:47.998Zhttps://docs.openclaw.ai/it/providers/fireworks2026-05-01T07:59:47.998Zhttps://docs.openclaw.ai/it/providers/github-copilot2026-05-01T07:59:48.045Zhttps://docs.openclaw.ai/it/providers/glm2026-05-01T07:59:48.044Zhttps://docs.openclaw.ai/it/providers/google2026-05-01T07:59:48.046Zhttps://docs.openclaw.ai/it/providers/gradium2026-05-01T07:59:48.044Zhttps://docs.openclaw.ai/it/providers/groq2026-05-01T07:59:48.037Zhttps://docs.openclaw.ai/it/providers/huggingface2026-05-01T07:59:48.037Zhttps://docs.openclaw.ai/it/providers2026-05-01T07:59:48.036Zhttps://docs.openclaw.ai/it/providers/inferrs2026-05-01T07:59:48.035Zhttps://docs.openclaw.ai/it/providers/inworld2026-05-01T07:59:48.041Zhttps://docs.openclaw.ai/it/providers/kilocode2026-05-01T07:59:48.043Zhttps://docs.openclaw.ai/it/providers/litellm2026-05-01T07:59:48.075Zhttps://docs.openclaw.ai/it/providers/lmstudio2026-05-01T07:59:48.076Zhttps://docs.openclaw.ai/it/providers/minimax2026-05-01T07:59:48.074Zhttps://docs.openclaw.ai/it/providers/mistral2026-05-01T07:59:48.075Zhttps://docs.openclaw.ai/it/providers/models2026-05-01T07:59:48.065Zhttps://docs.openclaw.ai/it/providers/moonshot2026-05-01T07:59:48.072Zhttps://docs.openclaw.ai/it/providers/nvidia2026-05-01T07:59:48.067Zhttps://docs.openclaw.ai/it/providers/ollama2026-05-01T07:59:48.074Zhttps://docs.openclaw.ai/it/providers/openai2026-05-01T07:59:48.066Zhttps://docs.openclaw.ai/it/providers/opencode2026-05-01T07:59:48.115Zhttps://docs.openclaw.ai/it/providers/opencode-go2026-05-01T07:59:48.066Zhttps://docs.openclaw.ai/it/providers/openrouter2026-05-01T07:59:48.114Zhttps://docs.openclaw.ai/it/providers/perplexity-provider2026-05-01T07:59:48.114Zhttps://docs.openclaw.ai/it/providers/qianfan2026-05-01T07:59:48.098Zhttps://docs.openclaw.ai/it/providers/qwen2026-05-01T07:59:48.100Zhttps://docs.openclaw.ai/it/providers/runway2026-05-01T07:59:48.102Zhttps://docs.openclaw.ai/it/providers/sglang2026-05-01T07:59:48.098Zhttps://docs.openclaw.ai/it/providers/stepfun2026-05-01T07:59:48.097Zhttps://docs.openclaw.ai/it/providers/synthetic2026-05-01T07:59:48.096Zhttps://docs.openclaw.ai/it/providers/tencent2026-05-01T07:59:48.144Zhttps://docs.openclaw.ai/it/providers/together2026-05-01T07:59:48.143Zhttps://docs.openclaw.ai/it/providers/venice2026-05-01T07:59:48.141Zhttps://docs.openclaw.ai/it/providers/vercel-ai-gateway2026-05-01T07:59:48.139Zhttps://docs.openclaw.ai/it/providers/vllm2026-05-01T07:59:48.142Zhttps://docs.openclaw.ai/it/providers/volcengine2026-05-01T07:59:48.142Zhttps://docs.openclaw.ai/it/providers/vydra2026-05-01T07:59:48.134Zhttps://docs.openclaw.ai/it/providers/xai2026-05-01T07:59:48.141Zhttps://docs.openclaw.ai/it/providers/xiaomi2026-05-01T07:59:48.133Zhttps://docs.openclaw.ai/it/providers/zai2026-05-01T07:59:48.133Zhttps://docs.openclaw.ai/it/reference/AGENTS.default2026-05-01T07:59:48.174Zhttps://docs.openclaw.ai/it/reference/RELEASING2026-05-01T08:38:26.506Zhttps://docs.openclaw.ai/it/reference/api-usage-costs2026-05-01T07:59:48.167Zhttps://docs.openclaw.ai/it/reference/credits2026-05-01T07:59:48.173Zhttps://docs.openclaw.ai/it/reference/device-models2026-05-01T07:59:48.165Zhttps://docs.openclaw.ai/it/reference/full-release-validation2026-05-01T08:38:29.124Zhttps://docs.openclaw.ai/it/reference/memory-config2026-05-01T07:59:48.171Zhttps://docs.openclaw.ai/it/reference/openclaw-sdk-api-design2026-05-01T07:59:48.166Zhttps://docs.openclaw.ai/it/reference/prompt-caching2026-05-01T07:59:48.166Zhttps://docs.openclaw.ai/it/reference/rich-output-protocol2026-05-01T07:59:48.164Zhttps://docs.openclaw.ai/it/reference/rpc2026-05-01T07:59:48.213Zhttps://docs.openclaw.ai/it/reference/secretref-credential-surface2026-05-01T08:38:29.128Zhttps://docs.openclaw.ai/it/reference/session-management-compaction2026-05-01T07:59:48.201Zhttps://docs.openclaw.ai/it/reference/templates/AGENTS2026-05-01T07:59:48.198Zhttps://docs.openclaw.ai/it/reference/templates/BOOT2026-05-01T07:59:48.193Zhttps://docs.openclaw.ai/it/reference/templates/BOOTSTRAP2026-05-01T07:59:48.200Zhttps://docs.openclaw.ai/it/reference/templates/HEARTBEAT2026-05-01T07:59:48.200Zhttps://docs.openclaw.ai/it/reference/templates/IDENTITY2026-05-01T07:59:48.241Zhttps://docs.openclaw.ai/it/reference/templates/SOUL2026-05-01T07:59:48.240Zhttps://docs.openclaw.ai/it/reference/templates/TOOLS2026-05-01T07:59:48.232Zhttps://docs.openclaw.ai/it/reference/templates/USER2026-05-01T07:59:48.237Zhttps://docs.openclaw.ai/it/reference/test2026-05-01T08:38:29.127Zhttps://docs.openclaw.ai/it/reference/token-use2026-05-01T07:59:48.233Zhttps://docs.openclaw.ai/it/reference/transcript-hygiene2026-05-01T07:59:48.232Zhttps://docs.openclaw.ai/it/reference/wizard2026-05-01T07:59:48.270Zhttps://docs.openclaw.ai/it/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:48.271Zhttps://docs.openclaw.ai/it/security/THREAT-MODEL-ATLAS2026-05-01T07:59:48.270Zhttps://docs.openclaw.ai/it/security/formal-verification2026-05-01T07:59:48.263Zhttps://docs.openclaw.ai/it/security/network-proxy2026-05-01T08:38:29.133Zhttps://docs.openclaw.ai/it/start/bootstrapping2026-05-01T07:59:48.269Zhttps://docs.openclaw.ai/it/start/docs-directory2026-05-01T07:59:48.261Zhttps://docs.openclaw.ai/it/start/getting-started2026-05-01T07:59:48.262Zhttps://docs.openclaw.ai/it/start/hubs2026-05-01T07:59:48.262Zhttps://docs.openclaw.ai/it/start/lore2026-05-01T07:59:48.261Zhttps://docs.openclaw.ai/it/start/onboarding2026-05-01T07:59:48.299Zhttps://docs.openclaw.ai/it/start/onboarding-overview2026-05-01T07:59:48.310Zhttps://docs.openclaw.ai/it/start/openclaw2026-05-01T07:59:48.298Zhttps://docs.openclaw.ai/it/start/setup2026-05-01T07:59:48.296Zhttps://docs.openclaw.ai/it/start/showcase2026-04-24T17:33:11.213Zhttps://docs.openclaw.ai/it/start/wizard2026-05-01T07:59:48.291Zhttps://docs.openclaw.ai/it/start/wizard-cli-automation2026-05-01T07:59:48.298Zhttps://docs.openclaw.ai/it/start/wizard-cli-reference2026-05-01T07:59:48.291Zhttps://docs.openclaw.ai/it/tools/acp-agents2026-05-01T08:38:29.130Zhttps://docs.openclaw.ai/it/tools/acp-agents-setup2026-05-01T07:59:48.340Zhttps://docs.openclaw.ai/it/tools/agent-send2026-05-01T07:59:48.331Zhttps://docs.openclaw.ai/it/tools/apply-patch2026-05-01T07:59:48.339Zhttps://docs.openclaw.ai/it/tools/brave-search2026-05-01T07:59:48.338Zhttps://docs.openclaw.ai/it/tools/browser2026-05-01T07:59:48.331Zhttps://docs.openclaw.ai/it/tools/browser-control2026-05-01T07:59:48.329Zhttps://docs.openclaw.ai/it/tools/browser-linux-troubleshooting2026-05-01T07:59:48.330Zhttps://docs.openclaw.ai/it/tools/browser-login2026-05-01T07:59:48.330Zhttps://docs.openclaw.ai/it/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:48.335Zhttps://docs.openclaw.ai/it/tools/btw2026-05-01T07:59:48.371Zhttps://docs.openclaw.ai/it/tools/clawhub2026-05-01T07:59:48.369Zhttps://docs.openclaw.ai/it/tools/code-execution2026-05-01T07:59:48.363Zhttps://docs.openclaw.ai/it/tools/creating-skills2026-05-01T07:59:48.366Zhttps://docs.openclaw.ai/it/tools/diffs2026-05-01T07:59:48.362Zhttps://docs.openclaw.ai/it/tools/duckduckgo-search2026-05-01T07:59:48.363Zhttps://docs.openclaw.ai/it/tools/elevated2026-05-01T07:59:48.361Zhttps://docs.openclaw.ai/it/tools/exa-search2026-05-01T07:59:48.362Zhttps://docs.openclaw.ai/it/tools/exec2026-05-01T07:59:48.399Zhttps://docs.openclaw.ai/it/tools/exec-approvals2026-05-01T07:59:48.410Zhttps://docs.openclaw.ai/it/tools/exec-approvals-advanced2026-05-01T07:59:48.361Zhttps://docs.openclaw.ai/it/tools/firecrawl2026-05-01T07:59:48.397Zhttps://docs.openclaw.ai/it/tools/gemini-search2026-05-01T07:59:48.395Zhttps://docs.openclaw.ai/it/tools/grok-search2026-05-01T07:59:48.398Zhttps://docs.openclaw.ai/it/tools/image-generation2026-05-01T07:59:48.391Zhttps://docs.openclaw.ai/it/tools2026-05-01T07:59:48.399Zhttps://docs.openclaw.ai/it/tools/kimi-search2026-05-01T07:59:48.390Zhttps://docs.openclaw.ai/it/tools/llm-task2026-05-01T07:59:48.391Zhttps://docs.openclaw.ai/it/tools/lobster2026-05-01T07:59:48.389Zhttps://docs.openclaw.ai/it/tools/loop-detection2026-05-01T07:59:48.453Zhttps://docs.openclaw.ai/it/tools/media-overview2026-05-01T07:59:48.452Zhttps://docs.openclaw.ai/it/tools/minimax-search2026-05-01T07:59:48.451Zhttps://docs.openclaw.ai/it/tools/multi-agent-sandbox-tools2026-05-01T07:59:48.443Zhttps://docs.openclaw.ai/it/tools/music-generation2026-05-01T07:59:48.452Zhttps://docs.openclaw.ai/it/tools/ollama-search2026-05-01T07:59:48.448Zhttps://docs.openclaw.ai/it/tools/pdf2026-05-01T07:59:48.451Zhttps://docs.openclaw.ai/it/tools/perplexity-search2026-05-01T07:59:48.444Zhttps://docs.openclaw.ai/it/tools/plugin2026-05-01T08:38:29.125Zhttps://docs.openclaw.ai/it/tools/reactions2026-05-01T07:59:48.443Zhttps://docs.openclaw.ai/it/tools/searxng-search2026-05-01T07:59:48.494Zhttps://docs.openclaw.ai/it/tools/skills2026-05-01T07:59:48.487Zhttps://docs.openclaw.ai/it/tools/skills-config2026-05-01T07:59:48.493Zhttps://docs.openclaw.ai/it/tools/slash-commands2026-05-01T07:59:48.486Zhttps://docs.openclaw.ai/it/tools/subagents2026-05-01T07:59:48.487Zhttps://docs.openclaw.ai/it/tools/tavily2026-05-01T07:59:48.489Zhttps://docs.openclaw.ai/it/tools/thinking2026-05-01T07:59:48.485Zhttps://docs.openclaw.ai/it/tools/tokenjuice2026-05-01T07:59:48.485Zhttps://docs.openclaw.ai/it/tools/trajectory2026-05-01T07:59:48.486Zhttps://docs.openclaw.ai/it/tools/tts2026-05-01T07:59:48.484Zhttps://docs.openclaw.ai/it/tools/video-generation2026-05-01T07:59:48.523Zhttps://docs.openclaw.ai/it/tools/web2026-05-01T07:59:48.520Zhttps://docs.openclaw.ai/it/tools/web-fetch2026-05-01T07:59:48.522Zhttps://docs.openclaw.ai/it/vps2026-05-01T07:59:48.516Zhttps://docs.openclaw.ai/it/web/control-ui2026-05-01T07:59:48.514Zhttps://docs.openclaw.ai/it/web/dashboard2026-05-01T07:59:48.515Zhttps://docs.openclaw.ai/it/web2026-05-01T07:59:48.514Zhttps://docs.openclaw.ai/it/web/tui2026-05-01T07:59:48.515Zhttps://docs.openclaw.ai/it/web/webchat2026-05-01T07:59:48.516Zhttps://docs.openclaw.ai/ja-JP/auth-credential-semantics2026-05-01T07:59:48.547Zhttps://docs.openclaw.ai/ja-JP/automation/cron-jobs2026-05-01T07:59:48.549Zhttps://docs.openclaw.ai/ja-JP/automation/hooks2026-05-01T07:59:48.548Zhttps://docs.openclaw.ai/ja-JP/automation2026-05-01T07:59:48.541Zhttps://docs.openclaw.ai/ja-JP/automation/standing-orders2026-05-01T07:59:48.594Zhttps://docs.openclaw.ai/ja-JP/automation/taskflow2026-05-01T07:59:48.585Zhttps://docs.openclaw.ai/ja-JP/automation/tasks2026-05-01T07:59:48.586Zhttps://docs.openclaw.ai/ja-JP/channels/bluebubbles2026-05-01T07:59:48.593Zhttps://docs.openclaw.ai/ja-JP/channels/broadcast-groups2026-05-01T07:59:48.586Zhttps://docs.openclaw.ai/ja-JP/channels/channel-routing2026-05-01T07:59:48.587Zhttps://docs.openclaw.ai/ja-JP/channels/discord2026-05-01T07:59:48.584Zhttps://docs.openclaw.ai/ja-JP/channels/feishu2026-05-01T07:59:48.623Zhttps://docs.openclaw.ai/ja-JP/channels/googlechat2026-05-01T07:59:48.621Zhttps://docs.openclaw.ai/ja-JP/channels/group-messages2026-05-01T07:59:48.621Zhttps://docs.openclaw.ai/ja-JP/channels/groups2026-05-01T07:59:48.620Zhttps://docs.openclaw.ai/ja-JP/channels/imessage2026-05-01T07:59:48.622Zhttps://docs.openclaw.ai/ja-JP/channels2026-05-01T07:59:48.613Zhttps://docs.openclaw.ai/ja-JP/channels/irc2026-05-01T07:59:48.614Zhttps://docs.openclaw.ai/ja-JP/channels/line2026-05-01T07:59:48.623Zhttps://docs.openclaw.ai/ja-JP/channels/location2026-05-01T07:59:48.614Zhttps://docs.openclaw.ai/ja-JP/channels/matrix2026-05-01T07:59:48.647Zhttps://docs.openclaw.ai/ja-JP/channels/matrix-migration2026-05-01T07:59:48.613Zhttps://docs.openclaw.ai/ja-JP/channels/matrix-push-rules2026-05-01T07:59:48.655Zhttps://docs.openclaw.ai/ja-JP/channels/mattermost2026-05-01T07:59:48.655Zhttps://docs.openclaw.ai/ja-JP/channels/msteams2026-05-01T07:59:48.648Zhttps://docs.openclaw.ai/ja-JP/channels/nextcloud-talk2026-05-01T07:59:48.654Zhttps://docs.openclaw.ai/ja-JP/channels/nostr2026-05-01T07:59:48.649Zhttps://docs.openclaw.ai/ja-JP/channels/pairing2026-05-01T07:59:48.648Zhttps://docs.openclaw.ai/ja-JP/channels/qa-channel2026-05-01T07:59:48.646Zhttps://docs.openclaw.ai/ja-JP/channels/qqbot2026-05-01T07:59:48.646Zhttps://docs.openclaw.ai/ja-JP/channels/signal2026-05-01T07:59:48.652Zhttps://docs.openclaw.ai/ja-JP/channels/slack2026-05-01T07:59:48.693Zhttps://docs.openclaw.ai/ja-JP/channels/synology-chat2026-05-01T07:59:48.700Zhttps://docs.openclaw.ai/ja-JP/channels/telegram2026-05-01T07:59:48.699Zhttps://docs.openclaw.ai/ja-JP/channels/tlon2026-05-01T07:59:48.695Zhttps://docs.openclaw.ai/ja-JP/channels/troubleshooting2026-05-01T07:59:48.676Zhttps://docs.openclaw.ai/ja-JP/channels/twitch2026-05-01T07:59:48.692Zhttps://docs.openclaw.ai/ja-JP/channels/wechat2026-05-01T07:59:48.678Zhttps://docs.openclaw.ai/ja-JP/channels/whatsapp2026-05-01T07:59:48.677Zhttps://docs.openclaw.ai/ja-JP/channels/yuanbao2026-05-01T07:59:48.677Zhttps://docs.openclaw.ai/ja-JP/channels/zalo2026-05-01T07:59:48.676Zhttps://docs.openclaw.ai/ja-JP/channels/zalouser2026-05-01T07:59:48.729Zhttps://docs.openclaw.ai/ja-JP/ci2026-05-01T07:59:48.728Zhttps://docs.openclaw.ai/ja-JP/cli/acp2026-05-01T07:59:48.726Zhttps://docs.openclaw.ai/ja-JP/cli/agent2026-05-01T07:59:48.727Zhttps://docs.openclaw.ai/ja-JP/cli/agents2026-05-01T07:59:48.729Zhttps://docs.openclaw.ai/ja-JP/cli/approvals2026-05-01T07:59:48.728Zhttps://docs.openclaw.ai/ja-JP/cli/backup2026-05-01T07:59:48.719Zhttps://docs.openclaw.ai/ja-JP/cli/browser2026-05-01T07:59:48.720Zhttps://docs.openclaw.ai/ja-JP/cli/channels2026-05-01T07:59:48.719Zhttps://docs.openclaw.ai/ja-JP/cli/clawbot2026-05-01T07:59:48.718Zhttps://docs.openclaw.ai/ja-JP/cli/commitments2026-05-01T07:59:48.758Zhttps://docs.openclaw.ai/ja-JP/cli/completion2026-05-01T07:59:48.757Zhttps://docs.openclaw.ai/ja-JP/cli/config2026-05-01T07:59:48.757Zhttps://docs.openclaw.ai/ja-JP/cli/configure2026-05-01T07:59:48.750Zhttps://docs.openclaw.ai/ja-JP/cli/cron2026-05-01T07:59:48.749Zhttps://docs.openclaw.ai/ja-JP/cli/daemon2026-05-01T07:59:48.751Zhttps://docs.openclaw.ai/ja-JP/cli/dashboard2026-05-01T07:59:48.750Zhttps://docs.openclaw.ai/ja-JP/cli/devices2026-05-01T07:59:48.749Zhttps://docs.openclaw.ai/ja-JP/cli/directory2026-05-01T07:59:48.755Zhttps://docs.openclaw.ai/ja-JP/cli/dns2026-05-01T07:59:48.798Zhttps://docs.openclaw.ai/ja-JP/cli/docs2026-05-01T07:59:48.797Zhttps://docs.openclaw.ai/ja-JP/cli/doctor2026-05-01T07:59:48.797Zhttps://docs.openclaw.ai/ja-JP/cli/flows2026-05-01T07:59:48.798Zhttps://docs.openclaw.ai/ja-JP/cli/gateway2026-05-01T07:59:48.785Zhttps://docs.openclaw.ai/ja-JP/cli/health2026-05-01T07:59:48.779Zhttps://docs.openclaw.ai/ja-JP/cli/hooks2026-05-01T07:59:48.780Zhttps://docs.openclaw.ai/ja-JP/cli2026-05-01T07:59:48.796Zhttps://docs.openclaw.ai/ja-JP/cli/infer2026-05-01T07:59:48.779Zhttps://docs.openclaw.ai/ja-JP/cli/logs2026-05-01T07:59:48.778Zhttps://docs.openclaw.ai/ja-JP/cli/mcp2026-05-01T07:59:48.828Zhttps://docs.openclaw.ai/ja-JP/cli/memory2026-05-01T07:59:48.826Zhttps://docs.openclaw.ai/ja-JP/cli/message2026-05-01T07:59:48.828Zhttps://docs.openclaw.ai/ja-JP/cli/migrate2026-05-01T07:59:48.827Zhttps://docs.openclaw.ai/ja-JP/cli/models2026-05-01T07:59:48.819Zhttps://docs.openclaw.ai/ja-JP/cli/node2026-05-01T07:59:48.824Zhttps://docs.openclaw.ai/ja-JP/cli/nodes2026-05-01T07:59:48.826Zhttps://docs.openclaw.ai/ja-JP/cli/onboard2026-05-01T07:59:48.819Zhttps://docs.openclaw.ai/ja-JP/cli/pairing2026-05-01T07:59:48.818Zhttps://docs.openclaw.ai/ja-JP/cli/plugins2026-05-01T07:59:48.817Zhttps://docs.openclaw.ai/ja-JP/cli/proxy2026-05-01T07:59:48.857Zhttps://docs.openclaw.ai/ja-JP/cli/qr2026-05-01T07:59:48.856Zhttps://docs.openclaw.ai/ja-JP/cli/reset2026-05-01T07:59:48.856Zhttps://docs.openclaw.ai/ja-JP/cli/sandbox2026-05-01T07:59:48.850Zhttps://docs.openclaw.ai/ja-JP/cli/secrets2026-05-01T07:59:48.850Zhttps://docs.openclaw.ai/ja-JP/cli/security2026-05-01T07:59:48.854Zhttps://docs.openclaw.ai/ja-JP/cli/sessions2026-05-01T07:59:48.849Zhttps://docs.openclaw.ai/ja-JP/cli/setup2026-05-01T07:59:48.849Zhttps://docs.openclaw.ai/ja-JP/cli/skills2026-05-01T07:59:48.848Zhttps://docs.openclaw.ai/ja-JP/cli/status2026-05-01T07:59:48.854Zhttps://docs.openclaw.ai/ja-JP/cli/system2026-05-01T07:59:48.896Zhttps://docs.openclaw.ai/ja-JP/cli/tasks2026-05-01T07:59:48.885Zhttps://docs.openclaw.ai/ja-JP/cli/tui2026-05-01T07:59:48.886Zhttps://docs.openclaw.ai/ja-JP/cli/uninstall2026-05-01T07:59:48.876Zhttps://docs.openclaw.ai/ja-JP/cli/update2026-05-01T07:59:48.883Zhttps://docs.openclaw.ai/ja-JP/cli/voicecall2026-05-01T07:59:48.884Zhttps://docs.openclaw.ai/ja-JP/cli/webhooks2026-05-01T07:59:48.885Zhttps://docs.openclaw.ai/ja-JP/cli/wiki2026-05-01T07:59:48.878Zhttps://docs.openclaw.ai/ja-JP/concepts/active-memory2026-05-01T07:59:48.877Zhttps://docs.openclaw.ai/ja-JP/concepts/agent2026-05-01T07:59:48.919Zhttps://docs.openclaw.ai/ja-JP/concepts/agent-loop2026-05-01T07:59:48.877Zhttps://docs.openclaw.ai/ja-JP/concepts/agent-runtimes2026-05-01T07:59:48.927Zhttps://docs.openclaw.ai/ja-JP/concepts/agent-workspace2026-05-01T07:59:48.926Zhttps://docs.openclaw.ai/ja-JP/concepts/architecture2026-05-01T07:59:48.919Zhttps://docs.openclaw.ai/ja-JP/concepts/channel-docking2026-05-01T07:59:48.924Zhttps://docs.openclaw.ai/ja-JP/concepts/commitments2026-05-01T07:59:48.927Zhttps://docs.openclaw.ai/ja-JP/concepts/compaction2026-05-01T07:59:48.920Zhttps://docs.openclaw.ai/ja-JP/concepts/context2026-05-01T07:59:48.918Zhttps://docs.openclaw.ai/ja-JP/concepts/context-engine2026-05-01T07:59:48.920Zhttps://docs.openclaw.ai/ja-JP/concepts/delegate-architecture2026-05-01T07:59:48.918Zhttps://docs.openclaw.ai/ja-JP/concepts/dreaming2026-05-01T07:59:48.957Zhttps://docs.openclaw.ai/ja-JP/concepts/experimental-features2026-05-01T07:59:48.955Zhttps://docs.openclaw.ai/ja-JP/concepts/features2026-05-01T07:59:48.954Zhttps://docs.openclaw.ai/ja-JP/concepts/markdown-formatting2026-05-01T07:59:48.952Zhttps://docs.openclaw.ai/ja-JP/concepts/memory2026-05-01T07:59:48.947Zhttps://docs.openclaw.ai/ja-JP/concepts/memory-builtin2026-05-01T07:59:48.947Zhttps://docs.openclaw.ai/ja-JP/concepts/memory-honcho2026-05-01T07:59:48.948Zhttps://docs.openclaw.ai/ja-JP/concepts/memory-qmd2026-05-01T07:59:48.956Zhttps://docs.openclaw.ai/ja-JP/concepts/memory-search2026-05-01T07:59:48.956Zhttps://docs.openclaw.ai/ja-JP/concepts/messages2026-05-01T07:59:48.946Zhttps://docs.openclaw.ai/ja-JP/concepts/model-failover2026-05-01T07:59:48.999Zhttps://docs.openclaw.ai/ja-JP/concepts/model-providers2026-05-01T07:59:48.988Zhttps://docs.openclaw.ai/ja-JP/concepts/models2026-05-01T07:59:48.978Zhttps://docs.openclaw.ai/ja-JP/concepts/multi-agent2026-05-01T07:59:48.987Zhttps://docs.openclaw.ai/ja-JP/concepts/oauth2026-05-01T07:59:48.987Zhttps://docs.openclaw.ai/ja-JP/concepts/openclaw-sdk2026-05-01T07:59:48.984Zhttps://docs.openclaw.ai/ja-JP/concepts/presence2026-05-01T07:59:48.978Zhttps://docs.openclaw.ai/ja-JP/concepts/qa-e2e-automation2026-05-01T07:59:48.977Zhttps://docs.openclaw.ai/ja-JP/concepts/qa-matrix2026-05-01T07:59:48.986Zhttps://docs.openclaw.ai/ja-JP/concepts/queue2026-05-01T07:59:49.030Zhttps://docs.openclaw.ai/ja-JP/concepts/queue-steering2026-05-01T07:59:48.977Zhttps://docs.openclaw.ai/ja-JP/concepts/retry2026-05-01T07:59:49.027Zhttps://docs.openclaw.ai/ja-JP/concepts/session2026-05-01T07:59:49.021Zhttps://docs.openclaw.ai/ja-JP/concepts/session-pruning2026-05-01T07:59:49.023Zhttps://docs.openclaw.ai/ja-JP/concepts/session-tool2026-05-01T07:59:49.030Zhttps://docs.openclaw.ai/ja-JP/concepts/soul2026-05-01T07:59:49.022Zhttps://docs.openclaw.ai/ja-JP/concepts/streaming2026-05-01T07:59:49.029Zhttps://docs.openclaw.ai/ja-JP/concepts/system-prompt2026-05-01T07:59:49.023Zhttps://docs.openclaw.ai/ja-JP/concepts/timezone2026-05-01T07:59:49.027Zhttps://docs.openclaw.ai/ja-JP/concepts/typebox2026-05-01T07:59:49.022Zhttps://docs.openclaw.ai/ja-JP/concepts/typing-indicators2026-05-01T07:59:49.061Zhttps://docs.openclaw.ai/ja-JP/concepts/usage-tracking2026-05-01T07:59:49.058Zhttps://docs.openclaw.ai/ja-JP/date-time2026-05-01T07:59:49.051Zhttps://docs.openclaw.ai/ja-JP/debug/node-issue2026-05-01T07:59:49.051Zhttps://docs.openclaw.ai/ja-JP/diagnostics/flags2026-05-01T07:59:49.059Zhttps://docs.openclaw.ai/ja-JP/gateway/authentication2026-05-01T07:59:49.060Zhttps://docs.openclaw.ai/ja-JP/gateway/background-process2026-05-01T07:59:49.052Zhttps://docs.openclaw.ai/ja-JP/gateway/bonjour2026-05-01T07:59:49.050Zhttps://docs.openclaw.ai/ja-JP/gateway/bridge-protocol2026-05-01T07:59:49.055Zhttps://docs.openclaw.ai/ja-JP/gateway/cli-backends2026-05-01T07:59:49.052Zhttps://docs.openclaw.ai/ja-JP/gateway/config-agents2026-05-01T07:59:49.092Zhttps://docs.openclaw.ai/ja-JP/gateway/config-channels2026-05-01T07:59:49.092Zhttps://docs.openclaw.ai/ja-JP/gateway/config-tools2026-05-01T07:59:49.083Zhttps://docs.openclaw.ai/ja-JP/gateway/configuration2026-05-01T07:59:49.097Zhttps://docs.openclaw.ai/ja-JP/gateway/configuration-examples2026-05-01T07:59:49.091Zhttps://docs.openclaw.ai/ja-JP/gateway/configuration-reference2026-05-01T07:59:49.093Zhttps://docs.openclaw.ai/ja-JP/gateway/diagnostics2026-05-01T07:59:49.082Zhttps://docs.openclaw.ai/ja-JP/gateway/discovery2026-05-01T07:59:49.093Zhttps://docs.openclaw.ai/ja-JP/gateway/doctor2026-05-01T07:59:49.082Zhttps://docs.openclaw.ai/ja-JP/gateway/gateway-lock2026-05-01T07:59:49.081Zhttps://docs.openclaw.ai/ja-JP/gateway/health2026-05-01T07:59:49.167Zhttps://docs.openclaw.ai/ja-JP/gateway/heartbeat2026-05-01T07:59:49.159Zhttps://docs.openclaw.ai/ja-JP/gateway2026-05-01T07:59:49.164Zhttps://docs.openclaw.ai/ja-JP/gateway/local-models2026-05-01T07:59:49.167Zhttps://docs.openclaw.ai/ja-JP/gateway/logging2026-05-01T07:59:49.159Zhttps://docs.openclaw.ai/ja-JP/gateway/multiple-gateways2026-05-01T07:59:49.166Zhttps://docs.openclaw.ai/ja-JP/gateway/network-model2026-05-01T07:59:49.160Zhttps://docs.openclaw.ai/ja-JP/gateway/openai-http-api2026-05-01T07:59:49.161Zhttps://docs.openclaw.ai/ja-JP/gateway/openresponses-http-api2026-05-01T07:59:49.160Zhttps://docs.openclaw.ai/ja-JP/gateway/openshell2026-05-01T07:59:49.158Zhttps://docs.openclaw.ai/ja-JP/gateway/opentelemetry2026-05-01T07:59:49.207Zhttps://docs.openclaw.ai/ja-JP/gateway/pairing2026-05-01T07:59:49.206Zhttps://docs.openclaw.ai/ja-JP/gateway/prometheus2026-05-01T07:59:49.205Zhttps://docs.openclaw.ai/ja-JP/gateway/protocol2026-05-01T07:59:49.201Zhttps://docs.openclaw.ai/ja-JP/gateway/remote2026-05-01T07:59:49.207Zhttps://docs.openclaw.ai/ja-JP/gateway/remote-gateway-readme2026-05-01T07:59:49.208Zhttps://docs.openclaw.ai/ja-JP/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:49.198Zhttps://docs.openclaw.ai/ja-JP/gateway/sandboxing2026-05-01T07:59:49.198Zhttps://docs.openclaw.ai/ja-JP/gateway/secrets2026-05-01T07:59:49.197Zhttps://docs.openclaw.ai/ja-JP/gateway/secrets-plan-contract2026-05-01T07:59:49.199Zhttps://docs.openclaw.ai/ja-JP/gateway/security/audit-checks2026-05-01T07:59:49.239Zhttps://docs.openclaw.ai/ja-JP/gateway/security2026-04-30T05:44:50.059Zhttps://docs.openclaw.ai/ja-JP/gateway/tailscale2026-05-01T07:59:49.240Zhttps://docs.openclaw.ai/ja-JP/gateway/tools-invoke-http-api2026-05-01T07:59:49.230Zhttps://docs.openclaw.ai/ja-JP/gateway/troubleshooting2026-05-01T07:59:49.241Zhttps://docs.openclaw.ai/ja-JP/gateway/trusted-proxy-auth2026-05-01T07:59:49.238Zhttps://docs.openclaw.ai/ja-JP/help/debugging2026-05-01T07:59:49.236Zhttps://docs.openclaw.ai/ja-JP/help/environment2026-05-01T07:59:49.236Zhttps://docs.openclaw.ai/ja-JP/help/faq2026-05-01T07:59:49.234Zhttps://docs.openclaw.ai/ja-JP/help/faq-first-run2026-05-01T07:59:49.230Zhttps://docs.openclaw.ai/ja-JP/help/faq-models2026-05-01T07:59:49.229Zhttps://docs.openclaw.ai/ja-JP/help/gpt55-codex-agentic-parity2026-05-01T07:59:49.270Zhttps://docs.openclaw.ai/ja-JP/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:49.270Zhttps://docs.openclaw.ai/ja-JP/help2026-05-01T07:59:49.271Zhttps://docs.openclaw.ai/ja-JP/help/scripts2026-05-01T07:59:49.269Zhttps://docs.openclaw.ai/ja-JP/help/testing2026-05-01T07:59:49.268Zhttps://docs.openclaw.ai/ja-JP/help/testing-live2026-05-01T07:59:49.262Zhttps://docs.openclaw.ai/ja-JP/help/troubleshooting2026-05-01T07:59:49.261Zhttps://docs.openclaw.ai/ja-JP2026-05-01T07:59:49.263Zhttps://docs.openclaw.ai/ja-JP/install/ansible2026-05-01T07:59:49.269Zhttps://docs.openclaw.ai/ja-JP/install/azure2026-05-01T07:59:49.261Zhttps://docs.openclaw.ai/ja-JP/install/bun2026-05-01T07:59:49.314Zhttps://docs.openclaw.ai/ja-JP/install/clawdock2026-05-01T07:59:49.307Zhttps://docs.openclaw.ai/ja-JP/install/development-channels2026-05-01T07:59:49.292Zhttps://docs.openclaw.ai/ja-JP/install/digitalocean2026-05-01T07:59:49.291Zhttps://docs.openclaw.ai/ja-JP/install/docker2026-05-01T07:59:49.310Zhttps://docs.openclaw.ai/ja-JP/install/docker-vm-runtime2026-05-01T07:59:49.307Zhttps://docs.openclaw.ai/ja-JP/install/exe-dev2026-05-01T07:59:49.308Zhttps://docs.openclaw.ai/ja-JP/install/fly2026-05-01T07:59:49.308Zhttps://docs.openclaw.ai/ja-JP/install/gcp2026-05-01T07:59:49.291Zhttps://docs.openclaw.ai/ja-JP/install/hetzner2026-05-01T07:59:49.290Zhttps://docs.openclaw.ai/ja-JP/install/hostinger2026-05-01T07:59:49.342Zhttps://docs.openclaw.ai/ja-JP/install2026-05-01T07:59:49.337Zhttps://docs.openclaw.ai/ja-JP/install/installer2026-05-01T07:59:49.332Zhttps://docs.openclaw.ai/ja-JP/install/kubernetes2026-05-01T07:59:49.341Zhttps://docs.openclaw.ai/ja-JP/install/macos-vm2026-05-01T07:59:49.332Zhttps://docs.openclaw.ai/ja-JP/install/migrating2026-05-01T07:59:49.341Zhttps://docs.openclaw.ai/ja-JP/install/migrating-claude2026-05-01T07:59:49.340Zhttps://docs.openclaw.ai/ja-JP/install/migrating-hermes2026-05-01T07:59:49.340Zhttps://docs.openclaw.ai/ja-JP/install/nix2026-05-01T07:59:49.339Zhttps://docs.openclaw.ai/ja-JP/install/node2026-05-01T07:59:49.331Zhttps://docs.openclaw.ai/ja-JP/install/northflank2026-05-01T07:59:49.366Zhttps://docs.openclaw.ai/ja-JP/install/oracle2026-05-01T07:59:49.370Zhttps://docs.openclaw.ai/ja-JP/install/podman2026-05-01T07:59:49.368Zhttps://docs.openclaw.ai/ja-JP/install/railway2026-05-01T07:59:49.360Zhttps://docs.openclaw.ai/ja-JP/install/raspberry-pi2026-05-01T07:59:49.363Zhttps://docs.openclaw.ai/ja-JP/install/render2026-05-01T07:59:49.358Zhttps://docs.openclaw.ai/ja-JP/install/uninstall2026-05-01T07:59:49.367Zhttps://docs.openclaw.ai/ja-JP/install/updating2026-05-01T07:59:49.364Zhttps://docs.openclaw.ai/ja-JP/logging2026-05-01T07:59:49.365Zhttps://docs.openclaw.ai/ja-JP/network2026-05-01T07:59:49.367Zhttps://docs.openclaw.ai/ja-JP/nodes/audio2026-05-01T07:59:49.397Zhttps://docs.openclaw.ai/ja-JP/nodes/camera2026-05-01T07:59:49.396Zhttps://docs.openclaw.ai/ja-JP/nodes/images2026-05-01T07:59:49.410Zhttps://docs.openclaw.ai/ja-JP/nodes2026-05-01T07:59:49.388Zhttps://docs.openclaw.ai/ja-JP/nodes/location-command2026-05-01T07:59:49.394Zhttps://docs.openclaw.ai/ja-JP/nodes/media-understanding2026-05-01T07:59:49.389Zhttps://docs.openclaw.ai/ja-JP/nodes/talk2026-05-01T07:59:49.396Zhttps://docs.openclaw.ai/ja-JP/nodes/troubleshooting2026-05-01T07:59:49.389Zhttps://docs.openclaw.ai/ja-JP/nodes/voicewake2026-05-01T07:59:49.409Zhttps://docs.openclaw.ai/ja-JP/pi2026-05-01T07:59:49.437Zhttps://docs.openclaw.ai/ja-JP/pi-dev2026-05-01T07:59:49.440Zhttps://docs.openclaw.ai/ja-JP/platforms/android2026-05-01T07:59:49.438Zhttps://docs.openclaw.ai/ja-JP/platforms2026-05-01T07:59:49.430Zhttps://docs.openclaw.ai/ja-JP/platforms/ios2026-05-01T07:59:49.430Zhttps://docs.openclaw.ai/ja-JP/platforms/linux2026-05-01T07:59:49.429Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/bundled-gateway2026-05-01T07:59:49.435Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/canvas2026-05-01T07:59:49.471Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/child-process2026-05-01T07:59:49.470Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/dev-setup2026-05-01T07:59:49.469Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/health2026-05-01T07:59:49.469Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/icon2026-05-01T07:59:49.463Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/logging2026-05-01T07:59:49.466Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/menu-bar2026-05-01T07:59:49.463Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/peekaboo2026-05-01T07:59:49.462Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/permissions2026-05-01T07:59:49.461Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/remote2026-05-01T07:59:49.461Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/signing2026-05-01T07:59:49.501Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/skills2026-05-01T07:59:49.501Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/voice-overlay2026-05-01T07:59:49.494Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/voicewake2026-05-01T07:59:49.500Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/webchat2026-05-01T07:59:49.492Zhttps://docs.openclaw.ai/ja-JP/platforms/mac/xpc2026-05-01T07:59:49.494Zhttps://docs.openclaw.ai/ja-JP/platforms/macos2026-05-01T07:59:49.493Zhttps://docs.openclaw.ai/ja-JP/platforms/windows2026-05-01T07:59:49.498Zhttps://docs.openclaw.ai/ja-JP/plugins/architecture2026-05-01T07:59:49.534Zhttps://docs.openclaw.ai/ja-JP/plugins/architecture-internals2026-05-01T07:59:49.540Zhttps://docs.openclaw.ai/ja-JP/plugins/building-plugins2026-05-01T07:59:49.531Zhttps://docs.openclaw.ai/ja-JP/plugins/bundles2026-05-01T07:59:49.538Zhttps://docs.openclaw.ai/ja-JP/plugins/codex-computer-use2026-05-01T07:59:49.529Zhttps://docs.openclaw.ai/ja-JP/plugins/codex-harness2026-05-01T07:59:49.537Zhttps://docs.openclaw.ai/ja-JP/plugins/community2026-05-01T07:59:49.528Zhttps://docs.openclaw.ai/ja-JP/plugins/compatibility2026-05-01T07:59:49.528Zhttps://docs.openclaw.ai/ja-JP/plugins/google-meet2026-05-01T07:59:49.573Zhttps://docs.openclaw.ai/ja-JP/plugins/hooks2026-05-01T07:59:49.571Zhttps://docs.openclaw.ai/ja-JP/plugins/manifest2026-05-01T07:59:49.572Zhttps://docs.openclaw.ai/ja-JP/plugins/memory-lancedb2026-05-01T07:59:49.571Zhttps://docs.openclaw.ai/ja-JP/plugins/memory-wiki2026-05-01T07:59:49.561Zhttps://docs.openclaw.ai/ja-JP/plugins/message-presentation2026-05-01T07:59:49.568Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-agent-harness2026-05-01T07:59:49.572Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-channel-plugins2026-05-01T07:59:49.561Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-channel-turn2026-05-01T07:59:49.560Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-entrypoints2026-05-01T07:59:49.560Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-migration2026-05-01T07:59:49.619Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-overview2026-05-01T07:59:49.605Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-provider-plugins2026-05-01T07:59:49.598Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-runtime2026-05-01T07:59:49.607Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-setup2026-05-01T07:59:49.595Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-subpaths2026-05-01T07:59:49.604Zhttps://docs.openclaw.ai/ja-JP/plugins/sdk-testing2026-05-01T07:59:49.595Zhttps://docs.openclaw.ai/ja-JP/plugins/skill-workshop2026-05-01T07:59:49.606Zhttps://docs.openclaw.ai/ja-JP/plugins/voice-call2026-05-01T07:59:49.594Zhttps://docs.openclaw.ai/ja-JP/plugins/webhooks2026-05-01T07:59:49.594Zhttps://docs.openclaw.ai/ja-JP/plugins/zalouser2026-05-01T07:59:49.648Zhttps://docs.openclaw.ai/ja-JP/prose2026-05-01T07:59:49.646Zhttps://docs.openclaw.ai/ja-JP/providers/alibaba2026-05-01T07:59:49.638Zhttps://docs.openclaw.ai/ja-JP/providers/anthropic2026-05-01T07:59:49.647Zhttps://docs.openclaw.ai/ja-JP/providers/arcee2026-05-01T07:59:49.643Zhttps://docs.openclaw.ai/ja-JP/providers/azure-speech2026-05-01T07:59:49.645Zhttps://docs.openclaw.ai/ja-JP/providers/bedrock2026-05-01T07:59:49.638Zhttps://docs.openclaw.ai/ja-JP/providers/bedrock-mantle2026-05-01T07:59:49.639Zhttps://docs.openclaw.ai/ja-JP/providers/chutes2026-05-01T07:59:49.646Zhttps://docs.openclaw.ai/ja-JP/providers/claude-max-api-proxy2026-05-01T07:59:49.679Zhttps://docs.openclaw.ai/ja-JP/providers/cloudflare-ai-gateway2026-05-01T07:59:49.677Zhttps://docs.openclaw.ai/ja-JP/providers/comfy2026-05-01T07:59:49.677Zhttps://docs.openclaw.ai/ja-JP/providers/deepgram2026-05-01T07:59:49.669Zhttps://docs.openclaw.ai/ja-JP/providers/deepinfra2026-05-01T07:59:49.676Zhttps://docs.openclaw.ai/ja-JP/providers/deepseek2026-05-01T07:59:49.673Zhttps://docs.openclaw.ai/ja-JP/providers/elevenlabs2026-05-01T07:59:49.675Zhttps://docs.openclaw.ai/ja-JP/providers/fal2026-05-01T07:59:49.669Zhttps://docs.openclaw.ai/ja-JP/providers/fireworks2026-05-01T07:59:49.668Zhttps://docs.openclaw.ai/ja-JP/providers/github-copilot2026-05-01T07:59:49.667Zhttps://docs.openclaw.ai/ja-JP/providers/glm2026-05-01T07:59:49.711Zhttps://docs.openclaw.ai/ja-JP/providers/google2026-05-01T07:59:49.710Zhttps://docs.openclaw.ai/ja-JP/providers/gradium2026-05-01T07:59:49.710Zhttps://docs.openclaw.ai/ja-JP/providers/groq2026-05-01T07:59:49.708Zhttps://docs.openclaw.ai/ja-JP/providers/huggingface2026-05-01T07:59:49.703Zhttps://docs.openclaw.ai/ja-JP/providers2026-05-01T07:59:49.708Zhttps://docs.openclaw.ai/ja-JP/providers/inferrs2026-05-01T07:59:49.703Zhttps://docs.openclaw.ai/ja-JP/providers/inworld2026-05-01T07:59:49.704Zhttps://docs.openclaw.ai/ja-JP/providers/kilocode2026-05-01T07:59:49.702Zhttps://docs.openclaw.ai/ja-JP/providers/litellm2026-05-01T07:59:49.702Zhttps://docs.openclaw.ai/ja-JP/providers/lmstudio2026-05-01T07:59:49.751Zhttps://docs.openclaw.ai/ja-JP/providers/minimax2026-05-01T07:59:49.750Zhttps://docs.openclaw.ai/ja-JP/providers/mistral2026-05-01T07:59:49.750Zhttps://docs.openclaw.ai/ja-JP/providers/models2026-05-01T07:59:49.747Zhttps://docs.openclaw.ai/ja-JP/providers/moonshot2026-05-01T07:59:49.742Zhttps://docs.openclaw.ai/ja-JP/providers/nvidia2026-05-01T07:59:49.749Zhttps://docs.openclaw.ai/ja-JP/providers/ollama2026-05-01T07:59:49.742Zhttps://docs.openclaw.ai/ja-JP/providers/openai2026-05-01T07:59:49.741Zhttps://docs.openclaw.ai/ja-JP/providers/opencode2026-05-01T07:59:49.740Zhttps://docs.openclaw.ai/ja-JP/providers/opencode-go2026-05-01T07:59:49.748Zhttps://docs.openclaw.ai/ja-JP/providers/openrouter2026-05-01T07:59:49.781Zhttps://docs.openclaw.ai/ja-JP/providers/perplexity-provider2026-05-01T07:59:49.779Zhttps://docs.openclaw.ai/ja-JP/providers/qianfan2026-05-01T07:59:49.781Zhttps://docs.openclaw.ai/ja-JP/providers/qwen2026-05-01T07:59:49.773Zhttps://docs.openclaw.ai/ja-JP/providers/runway2026-05-01T07:59:49.772Zhttps://docs.openclaw.ai/ja-JP/providers/sglang2026-05-01T07:59:49.774Zhttps://docs.openclaw.ai/ja-JP/providers/stepfun2026-05-01T07:59:49.773Zhttps://docs.openclaw.ai/ja-JP/providers/synthetic2026-05-01T07:59:49.772Zhttps://docs.openclaw.ai/ja-JP/providers/tencent2026-05-01T07:59:49.776Zhttps://docs.openclaw.ai/ja-JP/providers/together2026-05-01T07:59:49.812Zhttps://docs.openclaw.ai/ja-JP/providers/venice2026-05-01T07:59:49.809Zhttps://docs.openclaw.ai/ja-JP/providers/vercel-ai-gateway2026-05-01T07:59:49.811Zhttps://docs.openclaw.ai/ja-JP/providers/vllm2026-05-01T07:59:49.808Zhttps://docs.openclaw.ai/ja-JP/providers/volcengine2026-05-01T07:59:49.805Zhttps://docs.openclaw.ai/ja-JP/providers/vydra2026-05-01T07:59:49.809Zhttps://docs.openclaw.ai/ja-JP/providers/xai2026-05-01T07:59:49.810Zhttps://docs.openclaw.ai/ja-JP/providers/xiaomi2026-05-01T07:59:49.806Zhttps://docs.openclaw.ai/ja-JP/providers/zai2026-05-01T07:59:49.800Zhttps://docs.openclaw.ai/ja-JP/reference/AGENTS.default2026-05-01T07:59:49.799Zhttps://docs.openclaw.ai/ja-JP/reference/RELEASING2026-05-01T07:59:49.863Zhttps://docs.openclaw.ai/ja-JP/reference/api-usage-costs2026-05-01T07:59:49.863Zhttps://docs.openclaw.ai/ja-JP/reference/credits2026-05-01T07:59:49.855Zhttps://docs.openclaw.ai/ja-JP/reference/device-models2026-05-01T07:59:49.862Zhttps://docs.openclaw.ai/ja-JP/reference/full-release-validation2026-05-01T07:59:49.853Zhttps://docs.openclaw.ai/ja-JP/reference/memory-config2026-05-01T07:59:49.854Zhttps://docs.openclaw.ai/ja-JP/reference/openclaw-sdk-api-design2026-05-01T07:59:49.862Zhttps://docs.openclaw.ai/ja-JP/reference/prompt-caching2026-05-01T07:59:49.855Zhttps://docs.openclaw.ai/ja-JP/reference/rich-output-protocol2026-05-01T07:59:49.860Zhttps://docs.openclaw.ai/ja-JP/reference/rpc2026-05-01T07:59:49.900Zhttps://docs.openclaw.ai/ja-JP/reference/secretref-credential-surface2026-05-01T07:59:49.900Zhttps://docs.openclaw.ai/ja-JP/reference/session-management-compaction2026-05-01T07:59:49.902Zhttps://docs.openclaw.ai/ja-JP/reference/templates/AGENTS2026-05-01T07:59:49.896Zhttps://docs.openclaw.ai/ja-JP/reference/templates/BOOT2026-05-01T07:59:49.893Zhttps://docs.openclaw.ai/ja-JP/reference/templates/BOOTSTRAP2026-05-01T07:59:49.892Zhttps://docs.openclaw.ai/ja-JP/reference/templates/HEARTBEAT2026-05-01T07:59:49.892Zhttps://docs.openclaw.ai/ja-JP/reference/templates/IDENTITY2026-05-01T07:59:49.891Zhttps://docs.openclaw.ai/ja-JP/reference/templates/SOUL2026-05-01T07:59:49.926Zhttps://docs.openclaw.ai/ja-JP/reference/templates/TOOLS2026-05-01T07:59:49.928Zhttps://docs.openclaw.ai/ja-JP/reference/templates/USER2026-05-01T07:59:49.921Zhttps://docs.openclaw.ai/ja-JP/reference/test2026-05-01T07:59:49.922Zhttps://docs.openclaw.ai/ja-JP/reference/token-use2026-05-01T07:59:49.927Zhttps://docs.openclaw.ai/ja-JP/reference/transcript-hygiene2026-05-01T07:59:49.922Zhttps://docs.openclaw.ai/ja-JP/reference/wizard2026-05-01T07:59:49.925Zhttps://docs.openclaw.ai/ja-JP/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:49.958Zhttps://docs.openclaw.ai/ja-JP/security/THREAT-MODEL-ATLAS2026-05-01T07:59:49.957Zhttps://docs.openclaw.ai/ja-JP/security/formal-verification2026-05-01T07:59:49.957Zhttps://docs.openclaw.ai/ja-JP/security/network-proxy2026-05-01T07:59:49.949Zhttps://docs.openclaw.ai/ja-JP/start/bootstrapping2026-05-01T07:59:49.956Zhttps://docs.openclaw.ai/ja-JP/start/docs-directory2026-05-01T07:59:49.954Zhttps://docs.openclaw.ai/ja-JP/start/getting-started2026-05-01T07:59:49.948Zhttps://docs.openclaw.ai/ja-JP/start/hubs2026-05-01T07:59:49.950Zhttps://docs.openclaw.ai/ja-JP/start/lore2026-05-01T07:59:49.949Zhttps://docs.openclaw.ai/ja-JP/start/onboarding2026-05-01T07:59:49.994Zhttps://docs.openclaw.ai/ja-JP/start/onboarding-overview2026-05-01T07:59:49.950Zhttps://docs.openclaw.ai/ja-JP/start/openclaw2026-05-01T07:59:49.997Zhttps://docs.openclaw.ai/ja-JP/start/setup2026-05-01T07:59:49.996Zhttps://docs.openclaw.ai/ja-JP/start/showcase2026-04-24T17:33:15.930Zhttps://docs.openclaw.ai/ja-JP/start/wizard2026-05-01T07:59:49.978Zhttps://docs.openclaw.ai/ja-JP/start/wizard-cli-automation2026-05-01T07:59:49.997Zhttps://docs.openclaw.ai/ja-JP/start/wizard-cli-reference2026-05-01T07:59:49.979Zhttps://docs.openclaw.ai/ja-JP/tools/acp-agents2026-05-01T07:59:50.019Zhttps://docs.openclaw.ai/ja-JP/tools/acp-agents-setup2026-05-01T07:59:49.984Zhttps://docs.openclaw.ai/ja-JP/tools/agent-send2026-05-01T07:59:50.027Zhttps://docs.openclaw.ai/ja-JP/tools/apply-patch2026-05-01T07:59:50.026Zhttps://docs.openclaw.ai/ja-JP/tools/brave-search2026-05-01T07:59:50.027Zhttps://docs.openclaw.ai/ja-JP/tools/browser2026-05-01T07:59:50.024Zhttps://docs.openclaw.ai/ja-JP/tools/browser-control2026-05-01T07:59:50.021Zhttps://docs.openclaw.ai/ja-JP/tools/browser-linux-troubleshooting2026-05-01T07:59:50.020Zhttps://docs.openclaw.ai/ja-JP/tools/browser-login2026-05-01T07:59:50.020Zhttps://docs.openclaw.ai/ja-JP/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:50.019Zhttps://docs.openclaw.ai/ja-JP/tools/btw2026-05-01T07:59:50.018Zhttps://docs.openclaw.ai/ja-JP/tools/clawhub2026-05-01T07:59:50.054Zhttps://docs.openclaw.ai/ja-JP/tools/code-execution2026-05-01T07:59:50.055Zhttps://docs.openclaw.ai/ja-JP/tools/creating-skills2026-05-01T07:59:50.053Zhttps://docs.openclaw.ai/ja-JP/tools/diffs2026-05-01T07:59:50.056Zhttps://docs.openclaw.ai/ja-JP/tools/duckduckgo-search2026-05-01T07:59:50.048Zhttps://docs.openclaw.ai/ja-JP/tools/elevated2026-05-01T07:59:50.055Zhttps://docs.openclaw.ai/ja-JP/tools/exa-search2026-05-01T07:59:50.047Zhttps://docs.openclaw.ai/ja-JP/tools/exec2026-05-01T07:59:50.094Zhttps://docs.openclaw.ai/ja-JP/tools/exec-approvals2026-05-01T07:59:50.046Zhttps://docs.openclaw.ai/ja-JP/tools/exec-approvals-advanced2026-05-01T07:59:50.047Zhttps://docs.openclaw.ai/ja-JP/tools/firecrawl2026-05-01T07:59:50.078Zhttps://docs.openclaw.ai/ja-JP/tools/gemini-search2026-05-01T07:59:50.097Zhttps://docs.openclaw.ai/ja-JP/tools/grok-search2026-05-01T07:59:50.077Zhttps://docs.openclaw.ai/ja-JP/tools/image-generation2026-05-01T07:59:50.096Zhttps://docs.openclaw.ai/ja-JP/tools2026-05-01T07:59:50.083Zhttps://docs.openclaw.ai/ja-JP/tools/kimi-search2026-05-01T07:59:50.078Zhttps://docs.openclaw.ai/ja-JP/tools/llm-task2026-05-01T07:59:50.095Zhttps://docs.openclaw.ai/ja-JP/tools/lobster2026-05-01T07:59:50.077Zhttps://docs.openclaw.ai/ja-JP/tools/loop-detection2026-05-01T07:59:50.076Zhttps://docs.openclaw.ai/ja-JP/tools/media-overview2026-05-01T07:59:50.127Zhttps://docs.openclaw.ai/ja-JP/tools/minimax-search2026-05-01T07:59:50.127Zhttps://docs.openclaw.ai/ja-JP/tools/multi-agent-sandbox-tools2026-05-01T07:59:50.118Zhttps://docs.openclaw.ai/ja-JP/tools/music-generation2026-05-01T07:59:50.128Zhttps://docs.openclaw.ai/ja-JP/tools/ollama-search2026-05-01T07:59:50.119Zhttps://docs.openclaw.ai/ja-JP/tools/pdf2026-05-01T07:59:50.120Zhttps://docs.openclaw.ai/ja-JP/tools/perplexity-search2026-05-01T07:59:50.119Zhttps://docs.openclaw.ai/ja-JP/tools/plugin2026-05-01T07:59:50.117Zhttps://docs.openclaw.ai/ja-JP/tools/reactions2026-05-01T07:59:50.118Zhttps://docs.openclaw.ai/ja-JP/tools/searxng-search2026-05-01T07:59:50.123Zhttps://docs.openclaw.ai/ja-JP/tools/skills2026-05-01T07:59:50.158Zhttps://docs.openclaw.ai/ja-JP/tools/skills-config2026-05-01T07:59:50.160Zhttps://docs.openclaw.ai/ja-JP/tools/slash-commands2026-05-01T07:59:50.159Zhttps://docs.openclaw.ai/ja-JP/tools/subagents2026-05-01T07:59:50.157Zhttps://docs.openclaw.ai/ja-JP/tools/tavily2026-05-01T07:59:50.152Zhttps://docs.openclaw.ai/ja-JP/tools/thinking2026-05-01T07:59:50.152Zhttps://docs.openclaw.ai/ja-JP/tools/tokenjuice2026-05-01T07:59:50.151Zhttps://docs.openclaw.ai/ja-JP/tools/trajectory2026-05-01T07:59:50.151Zhttps://docs.openclaw.ai/ja-JP/tools/tts2026-05-01T07:59:50.150Zhttps://docs.openclaw.ai/ja-JP/tools/video-generation2026-05-01T07:59:50.150Zhttps://docs.openclaw.ai/ja-JP/tools/web2026-05-01T07:59:50.197Zhttps://docs.openclaw.ai/ja-JP/tools/web-fetch2026-05-01T07:59:50.199Zhttps://docs.openclaw.ai/ja-JP/vps2026-05-01T07:59:50.184Zhttps://docs.openclaw.ai/ja-JP/web/control-ui2026-05-01T07:59:50.180Zhttps://docs.openclaw.ai/ja-JP/web/dashboard2026-05-01T07:59:50.198Zhttps://docs.openclaw.ai/ja-JP/web2026-05-01T07:59:50.186Zhttps://docs.openclaw.ai/ja-JP/web/tui2026-05-01T07:59:50.179Zhttps://docs.openclaw.ai/ja-JP/web/webchat2026-05-01T07:59:50.179Zhttps://docs.openclaw.ai/ko/auth-credential-semantics2026-05-01T07:59:50.227Zhttps://docs.openclaw.ai/ko/automation/cron-jobs2026-05-01T07:59:50.223Zhttps://docs.openclaw.ai/ko/automation/hooks2026-05-01T07:59:50.225Zhttps://docs.openclaw.ai/ko/automation2026-05-01T07:59:50.223Zhttps://docs.openclaw.ai/ko/automation/standing-orders2026-05-01T07:59:50.217Zhttps://docs.openclaw.ai/ko/automation/taskflow2026-05-01T07:59:50.258Zhttps://docs.openclaw.ai/ko/automation/tasks2026-05-01T07:59:50.257Zhttps://docs.openclaw.ai/ko/channels/bluebubbles2026-05-01T07:59:50.256Zhttps://docs.openclaw.ai/ko/channels/broadcast-groups2026-05-01T07:59:50.249Zhttps://docs.openclaw.ai/ko/channels/channel-routing2026-05-01T07:59:50.249Zhttps://docs.openclaw.ai/ko/channels/discord2026-05-01T07:59:50.248Zhttps://docs.openclaw.ai/ko/channels/feishu2026-05-01T07:59:50.247Zhttps://docs.openclaw.ai/ko/channels/googlechat2026-05-01T07:59:50.299Zhttps://docs.openclaw.ai/ko/channels/group-messages2026-05-01T07:59:50.287Zhttps://docs.openclaw.ai/ko/channels/groups2026-05-01T07:59:50.286Zhttps://docs.openclaw.ai/ko/channels/imessage2026-05-01T07:59:50.286Zhttps://docs.openclaw.ai/ko/channels2026-05-01T07:59:50.283Zhttps://docs.openclaw.ai/ko/channels/irc2026-05-01T07:59:50.279Zhttps://docs.openclaw.ai/ko/channels/line2026-05-01T07:59:50.280Zhttps://docs.openclaw.ai/ko/channels/location2026-05-01T07:59:50.279Zhttps://docs.openclaw.ai/ko/channels/matrix2026-05-01T07:59:50.331Zhttps://docs.openclaw.ai/ko/channels/matrix-migration2026-05-01T07:59:50.278Zhttps://docs.openclaw.ai/ko/channels/matrix-push-rules2026-05-01T07:59:50.277Zhttps://docs.openclaw.ai/ko/channels/mattermost2026-05-01T07:59:50.326Zhttps://docs.openclaw.ai/ko/channels/msteams2026-05-01T07:59:50.329Zhttps://docs.openclaw.ai/ko/channels/nextcloud-talk2026-05-01T07:59:50.328Zhttps://docs.openclaw.ai/ko/channels/nostr2026-05-01T07:59:50.327Zhttps://docs.openclaw.ai/ko/channels/pairing2026-05-01T07:59:50.319Zhttps://docs.openclaw.ai/ko/channels/qa-channel2026-05-01T07:59:50.318Zhttps://docs.openclaw.ai/ko/channels/qqbot2026-05-01T07:59:50.327Zhttps://docs.openclaw.ai/ko/channels/signal2026-05-01T07:59:50.319Zhttps://docs.openclaw.ai/ko/channels/slack2026-05-01T07:59:50.318Zhttps://docs.openclaw.ai/ko/channels/synology-chat2026-05-01T07:59:50.362Zhttps://docs.openclaw.ai/ko/channels/telegram2026-05-01T07:59:50.361Zhttps://docs.openclaw.ai/ko/channels/tlon2026-05-01T07:59:50.360Zhttps://docs.openclaw.ai/ko/channels/troubleshooting2026-05-01T07:59:50.359Zhttps://docs.openclaw.ai/ko/channels/twitch2026-05-01T07:59:50.351Zhttps://docs.openclaw.ai/ko/channels/wechat2026-05-01T07:59:50.350Zhttps://docs.openclaw.ai/ko/channels/whatsapp2026-05-01T07:59:50.359Zhttps://docs.openclaw.ai/ko/channels/yuanbao2026-05-01T07:59:50.350Zhttps://docs.openclaw.ai/ko/channels/zalo2026-05-01T07:59:50.352Zhttps://docs.openclaw.ai/ko/channels/zalouser2026-05-01T07:59:50.357Zhttps://docs.openclaw.ai/ko/ci2026-05-01T07:59:50.402Zhttps://docs.openclaw.ai/ko/cli/acp2026-05-01T07:59:50.401Zhttps://docs.openclaw.ai/ko/cli/agent2026-05-01T07:59:50.390Zhttps://docs.openclaw.ai/ko/cli/agents2026-05-01T07:59:50.381Zhttps://docs.openclaw.ai/ko/cli/approvals2026-05-01T07:59:50.391Zhttps://docs.openclaw.ai/ko/cli/backup2026-05-01T07:59:50.386Zhttps://docs.openclaw.ai/ko/cli/browser2026-05-01T07:59:50.382Zhttps://docs.openclaw.ai/ko/cli/channels2026-05-01T07:59:50.389Zhttps://docs.openclaw.ai/ko/cli/clawbot2026-05-01T07:59:50.381Zhttps://docs.openclaw.ai/ko/cli/commitments2026-05-01T07:59:50.380Zhttps://docs.openclaw.ai/ko/cli/completion2026-05-01T07:59:50.441Zhttps://docs.openclaw.ai/ko/cli/config2026-05-01T07:59:50.440Zhttps://docs.openclaw.ai/ko/cli/configure2026-05-01T07:59:50.432Zhttps://docs.openclaw.ai/ko/cli/cron2026-05-01T07:59:50.431Zhttps://docs.openclaw.ai/ko/cli/daemon2026-05-01T07:59:50.438Zhttps://docs.openclaw.ai/ko/cli/dashboard2026-05-01T07:59:50.432Zhttps://docs.openclaw.ai/ko/cli/devices2026-05-01T07:59:50.430Zhttps://docs.openclaw.ai/ko/cli/directory2026-05-01T07:59:50.430Zhttps://docs.openclaw.ai/ko/cli/dns2026-05-01T07:59:50.429Zhttps://docs.openclaw.ai/ko/cli/docs2026-05-01T07:59:50.471Zhttps://docs.openclaw.ai/ko/cli/doctor2026-05-01T07:59:50.471Zhttps://docs.openclaw.ai/ko/cli/flows2026-05-01T07:59:50.470Zhttps://docs.openclaw.ai/ko/cli/gateway2026-05-01T07:59:50.468Zhttps://docs.openclaw.ai/ko/cli/health2026-05-01T07:59:50.469Zhttps://docs.openclaw.ai/ko/cli/hooks2026-05-01T07:59:50.464Zhttps://docs.openclaw.ai/ko/cli2026-05-01T07:59:50.463Zhttps://docs.openclaw.ai/ko/cli/infer2026-05-01T07:59:50.463Zhttps://docs.openclaw.ai/ko/cli/logs2026-05-01T07:59:50.462Zhttps://docs.openclaw.ai/ko/cli/mcp2026-05-01T07:59:50.462Zhttps://docs.openclaw.ai/ko/cli/memory2026-05-01T07:59:50.502Zhttps://docs.openclaw.ai/ko/cli/message2026-05-01T07:59:50.500Zhttps://docs.openclaw.ai/ko/cli/migrate2026-05-01T07:59:50.501Zhttps://docs.openclaw.ai/ko/cli/models2026-05-01T07:59:50.499Zhttps://docs.openclaw.ai/ko/cli/node2026-05-01T07:59:50.496Zhttps://docs.openclaw.ai/ko/cli/nodes2026-05-01T07:59:50.499Zhttps://docs.openclaw.ai/ko/cli/onboard2026-05-01T07:59:50.492Zhttps://docs.openclaw.ai/ko/cli/pairing2026-05-01T07:59:50.491Zhttps://docs.openclaw.ai/ko/cli/plugins2026-05-01T07:59:50.491Zhttps://docs.openclaw.ai/ko/cli/proxy2026-05-01T07:59:50.490Zhttps://docs.openclaw.ai/ko/cli/qr2026-05-01T07:59:50.550Zhttps://docs.openclaw.ai/ko/cli/reset2026-05-01T07:59:50.550Zhttps://docs.openclaw.ai/ko/cli/sandbox2026-05-01T07:59:50.546Zhttps://docs.openclaw.ai/ko/cli/secrets2026-05-01T07:59:50.549Zhttps://docs.openclaw.ai/ko/cli/security2026-05-01T07:59:50.552Zhttps://docs.openclaw.ai/ko/cli/sessions2026-05-01T07:59:50.542Zhttps://docs.openclaw.ai/ko/cli/setup2026-05-01T07:59:50.543Zhttps://docs.openclaw.ai/ko/cli/skills2026-05-01T07:59:50.543Zhttps://docs.openclaw.ai/ko/cli/status2026-05-01T07:59:50.542Zhttps://docs.openclaw.ai/ko/cli/system2026-05-01T07:59:50.547Zhttps://docs.openclaw.ai/ko/cli/tasks2026-05-01T07:59:50.589Zhttps://docs.openclaw.ai/ko/cli/tui2026-05-01T07:59:50.590Zhttps://docs.openclaw.ai/ko/cli/uninstall2026-05-01T07:59:50.588Zhttps://docs.openclaw.ai/ko/cli/update2026-05-01T07:59:50.590Zhttps://docs.openclaw.ai/ko/cli/voicecall2026-05-01T07:59:50.585Zhttps://docs.openclaw.ai/ko/cli/webhooks2026-05-01T07:59:50.583Zhttps://docs.openclaw.ai/ko/cli/wiki2026-05-01T07:59:50.582Zhttps://docs.openclaw.ai/ko/concepts/active-memory2026-05-01T07:59:50.582Zhttps://docs.openclaw.ai/ko/concepts/agent2026-05-01T07:59:50.616Zhttps://docs.openclaw.ai/ko/concepts/agent-loop2026-05-01T07:59:50.581Zhttps://docs.openclaw.ai/ko/concepts/agent-runtimes2026-05-01T07:59:50.581Zhttps://docs.openclaw.ai/ko/concepts/agent-workspace2026-05-01T07:59:50.616Zhttps://docs.openclaw.ai/ko/concepts/architecture2026-05-01T07:59:50.618Zhttps://docs.openclaw.ai/ko/concepts/channel-docking2026-05-01T07:59:50.617Zhttps://docs.openclaw.ai/ko/concepts/commitments2026-05-01T07:59:50.614Zhttps://docs.openclaw.ai/ko/concepts/compaction2026-05-01T07:59:50.618Zhttps://docs.openclaw.ai/ko/concepts/context2026-05-01T07:59:50.619Zhttps://docs.openclaw.ai/ko/concepts/context-engine2026-05-01T07:59:50.608Zhttps://docs.openclaw.ai/ko/concepts/delegate-architecture2026-05-01T07:59:50.615Zhttps://docs.openclaw.ai/ko/concepts/dreaming2026-05-01T07:59:50.607Zhttps://docs.openclaw.ai/ko/concepts/experimental-features2026-05-01T07:59:50.640Zhttps://docs.openclaw.ai/ko/concepts/features2026-05-01T07:59:50.648Zhttps://docs.openclaw.ai/ko/concepts/markdown-formatting2026-05-01T07:59:50.648Zhttps://docs.openclaw.ai/ko/concepts/memory2026-05-01T07:59:50.645Zhttps://docs.openclaw.ai/ko/concepts/memory-builtin2026-05-01T07:59:50.641Zhttps://docs.openclaw.ai/ko/concepts/memory-honcho2026-05-01T07:59:50.647Zhttps://docs.openclaw.ai/ko/concepts/memory-qmd2026-05-01T07:59:50.641Zhttps://docs.openclaw.ai/ko/concepts/memory-search2026-05-01T07:59:50.639Zhttps://docs.openclaw.ai/ko/concepts/messages2026-05-01T07:59:50.640Zhttps://docs.openclaw.ai/ko/concepts/model-failover2026-05-01T07:59:50.644Zhttps://docs.openclaw.ai/ko/concepts/model-providers2026-05-01T07:59:50.685Zhttps://docs.openclaw.ai/ko/concepts/models2026-05-01T07:59:50.688Zhttps://docs.openclaw.ai/ko/concepts/multi-agent2026-05-01T07:59:50.687Zhttps://docs.openclaw.ai/ko/concepts/oauth2026-05-01T07:59:50.674Zhttps://docs.openclaw.ai/ko/concepts/openclaw-sdk2026-05-01T07:59:50.666Zhttps://docs.openclaw.ai/ko/concepts/presence2026-05-01T07:59:50.668Zhttps://docs.openclaw.ai/ko/concepts/qa-e2e-automation2026-05-01T07:59:50.688Zhttps://docs.openclaw.ai/ko/concepts/qa-matrix2026-05-01T07:59:50.668Zhttps://docs.openclaw.ai/ko/concepts/queue2026-05-01T07:59:50.667Zhttps://docs.openclaw.ai/ko/concepts/queue-steering2026-05-01T07:59:50.687Zhttps://docs.openclaw.ai/ko/concepts/retry2026-05-01T07:59:50.716Zhttps://docs.openclaw.ai/ko/concepts/session2026-05-01T07:59:50.709Zhttps://docs.openclaw.ai/ko/concepts/session-pruning2026-05-01T07:59:50.710Zhttps://docs.openclaw.ai/ko/concepts/session-tool2026-05-01T07:59:50.716Zhttps://docs.openclaw.ai/ko/concepts/soul2026-05-01T07:59:50.717Zhttps://docs.openclaw.ai/ko/concepts/streaming2026-05-01T07:59:50.709Zhttps://docs.openclaw.ai/ko/concepts/system-prompt2026-05-01T07:59:50.708Zhttps://docs.openclaw.ai/ko/concepts/timezone2026-05-01T07:59:50.714Zhttps://docs.openclaw.ai/ko/concepts/typebox2026-05-01T07:59:50.710Zhttps://docs.openclaw.ai/ko/concepts/typing-indicators2026-05-01T07:59:50.713Zhttps://docs.openclaw.ai/ko/concepts/usage-tracking2026-05-01T07:59:50.747Zhttps://docs.openclaw.ai/ko/date-time2026-05-01T07:59:50.747Zhttps://docs.openclaw.ai/ko/debug/node-issue2026-05-01T07:59:50.748Zhttps://docs.openclaw.ai/ko/diagnostics/flags2026-05-01T07:59:50.744Zhttps://docs.openclaw.ai/ko/gateway/authentication2026-05-01T07:59:50.741Zhttps://docs.openclaw.ai/ko/gateway/background-process2026-05-01T07:59:50.741Zhttps://docs.openclaw.ai/ko/gateway/bonjour2026-05-01T07:59:50.740Zhttps://docs.openclaw.ai/ko/gateway/bridge-protocol2026-05-01T07:59:50.740Zhttps://docs.openclaw.ai/ko/gateway/cli-backends2026-05-01T07:59:50.739Zhttps://docs.openclaw.ai/ko/gateway/config-agents2026-05-01T07:59:50.739Zhttps://docs.openclaw.ai/ko/gateway/config-channels2026-05-01T07:59:50.776Zhttps://docs.openclaw.ai/ko/gateway/config-tools2026-05-01T07:59:50.777Zhttps://docs.openclaw.ai/ko/gateway/configuration2026-05-01T07:59:50.790Zhttps://docs.openclaw.ai/ko/gateway/configuration-examples2026-05-01T07:59:50.777Zhttps://docs.openclaw.ai/ko/gateway/configuration-reference2026-05-01T07:59:50.778Zhttps://docs.openclaw.ai/ko/gateway/diagnostics2026-05-01T07:59:50.769Zhttps://docs.openclaw.ai/ko/gateway/discovery2026-05-01T07:59:50.768Zhttps://docs.openclaw.ai/ko/gateway/doctor2026-05-01T07:59:50.769Zhttps://docs.openclaw.ai/ko/gateway/gateway-lock2026-05-01T07:59:50.791Zhttps://docs.openclaw.ai/ko/gateway/health2026-05-01T07:59:50.768Zhttps://docs.openclaw.ai/ko/gateway/heartbeat2026-05-01T07:59:50.810Zhttps://docs.openclaw.ai/ko/gateway2026-05-01T07:59:50.818Zhttps://docs.openclaw.ai/ko/gateway/local-models2026-05-01T07:59:50.816Zhttps://docs.openclaw.ai/ko/gateway/logging2026-05-01T07:59:50.820Zhttps://docs.openclaw.ai/ko/gateway/multiple-gateways2026-05-01T07:59:50.819Zhttps://docs.openclaw.ai/ko/gateway/network-model2026-05-01T07:59:50.819Zhttps://docs.openclaw.ai/ko/gateway/openai-http-api2026-05-01T07:59:50.811Zhttps://docs.openclaw.ai/ko/gateway/openresponses-http-api2026-05-01T07:59:50.818Zhttps://docs.openclaw.ai/ko/gateway/openshell2026-05-01T07:59:50.811Zhttps://docs.openclaw.ai/ko/gateway/opentelemetry2026-05-01T07:59:50.810Zhttps://docs.openclaw.ai/ko/gateway/pairing2026-05-01T07:59:50.852Zhttps://docs.openclaw.ai/ko/gateway/prometheus2026-05-01T07:59:50.850Zhttps://docs.openclaw.ai/ko/gateway/protocol2026-05-01T07:59:50.851Zhttps://docs.openclaw.ai/ko/gateway/remote2026-05-01T07:59:50.850Zhttps://docs.openclaw.ai/ko/gateway/remote-gateway-readme2026-05-01T07:59:50.842Zhttps://docs.openclaw.ai/ko/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:50.848Zhttps://docs.openclaw.ai/ko/gateway/sandboxing2026-05-01T07:59:50.842Zhttps://docs.openclaw.ai/ko/gateway/secrets2026-05-01T07:59:50.843Zhttps://docs.openclaw.ai/ko/gateway/secrets-plan-contract2026-05-01T07:59:50.841Zhttps://docs.openclaw.ai/ko/gateway/security/audit-checks2026-05-01T07:59:50.841Zhttps://docs.openclaw.ai/ko/gateway/security2026-04-30T07:00:40.914Zhttps://docs.openclaw.ai/ko/gateway/tailscale2026-05-01T07:59:50.897Zhttps://docs.openclaw.ai/ko/gateway/tools-invoke-http-api2026-05-01T07:59:50.896Zhttps://docs.openclaw.ai/ko/gateway/troubleshooting2026-05-01T07:59:50.896Zhttps://docs.openclaw.ai/ko/gateway/trusted-proxy-auth2026-05-01T07:59:50.883Zhttps://docs.openclaw.ai/ko/help/debugging2026-05-01T07:59:50.884Zhttps://docs.openclaw.ai/ko/help/environment2026-05-01T07:59:50.878Zhttps://docs.openclaw.ai/ko/help/faq2026-05-01T07:59:50.877Zhttps://docs.openclaw.ai/ko/help/faq-first-run2026-05-01T07:59:50.879Zhttps://docs.openclaw.ai/ko/help/faq-models2026-05-01T07:59:50.878Zhttps://docs.openclaw.ai/ko/help/gpt55-codex-agentic-parity2026-05-01T07:59:50.928Zhttps://docs.openclaw.ai/ko/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:50.877Zhttps://docs.openclaw.ai/ko/help2026-05-01T07:59:50.927Zhttps://docs.openclaw.ai/ko/help/scripts2026-05-01T07:59:50.927Zhttps://docs.openclaw.ai/ko/help/testing2026-05-01T07:59:50.926Zhttps://docs.openclaw.ai/ko/help/testing-live2026-05-01T07:59:50.925Zhttps://docs.openclaw.ai/ko/help/troubleshooting2026-05-01T07:59:50.920Zhttps://docs.openclaw.ai/ko2026-05-01T07:59:50.919Zhttps://docs.openclaw.ai/ko/install/ansible2026-05-01T07:59:50.919Zhttps://docs.openclaw.ai/ko/install/azure2026-05-01T07:59:50.918Zhttps://docs.openclaw.ai/ko/install/bun2026-05-01T07:59:50.917Zhttps://docs.openclaw.ai/ko/install/clawdock2026-05-01T07:59:50.958Zhttps://docs.openclaw.ai/ko/install/development-channels2026-05-01T07:59:50.957Zhttps://docs.openclaw.ai/ko/install/digitalocean2026-05-01T07:59:50.956Zhttps://docs.openclaw.ai/ko/install/docker2026-05-01T07:59:50.948Zhttps://docs.openclaw.ai/ko/install/docker-vm-runtime2026-05-01T07:59:50.950Zhttps://docs.openclaw.ai/ko/install/exe-dev2026-05-01T07:59:50.949Zhttps://docs.openclaw.ai/ko/install/fly2026-05-01T07:59:50.949Zhttps://docs.openclaw.ai/ko/install/gcp2026-05-01T07:59:50.957Zhttps://docs.openclaw.ai/ko/install/hetzner2026-05-01T07:59:50.948Zhttps://docs.openclaw.ai/ko/install/hostinger2026-05-01T07:59:50.953Zhttps://docs.openclaw.ai/ko/install2026-05-01T07:59:50.997Zhttps://docs.openclaw.ai/ko/install/installer2026-05-01T07:59:50.978Zhttps://docs.openclaw.ai/ko/install/kubernetes2026-05-01T07:59:50.986Zhttps://docs.openclaw.ai/ko/install/macos-vm2026-05-01T07:59:50.983Zhttps://docs.openclaw.ai/ko/install/migrating2026-05-01T07:59:50.979Zhttps://docs.openclaw.ai/ko/install/migrating-claude2026-05-01T07:59:50.986Zhttps://docs.openclaw.ai/ko/install/migrating-hermes2026-05-01T07:59:50.985Zhttps://docs.openclaw.ai/ko/install/nix2026-05-01T07:59:50.978Zhttps://docs.openclaw.ai/ko/install/node2026-05-01T07:59:50.977Zhttps://docs.openclaw.ai/ko/install/northflank2026-05-01T07:59:50.984Zhttps://docs.openclaw.ai/ko/install/oracle2026-05-01T07:59:51.020Zhttps://docs.openclaw.ai/ko/install/podman2026-05-01T07:59:51.024Zhttps://docs.openclaw.ai/ko/install/railway2026-05-01T07:59:51.019Zhttps://docs.openclaw.ai/ko/install/raspberry-pi2026-05-01T07:59:51.023Zhttps://docs.openclaw.ai/ko/install/render2026-05-01T07:59:51.021Zhttps://docs.openclaw.ai/ko/install/uninstall2026-05-01T07:59:51.022Zhttps://docs.openclaw.ai/ko/install/updating2026-05-01T07:59:51.022Zhttps://docs.openclaw.ai/ko/logging2026-05-01T07:59:51.015Zhttps://docs.openclaw.ai/ko/network2026-05-01T07:59:51.016Zhttps://docs.openclaw.ai/ko/nodes/audio2026-05-01T07:59:51.015Zhttps://docs.openclaw.ai/ko/nodes/camera2026-05-01T07:59:51.052Zhttps://docs.openclaw.ai/ko/nodes/images2026-05-01T07:59:51.050Zhttps://docs.openclaw.ai/ko/nodes2026-05-01T07:59:51.043Zhttps://docs.openclaw.ai/ko/nodes/location-command2026-05-01T07:59:51.043Zhttps://docs.openclaw.ai/ko/nodes/media-understanding2026-05-01T07:59:51.051Zhttps://docs.openclaw.ai/ko/nodes/talk2026-05-01T07:59:51.048Zhttps://docs.openclaw.ai/ko/nodes/troubleshooting2026-05-01T07:59:51.051Zhttps://docs.openclaw.ai/ko/nodes/voicewake2026-05-01T07:59:51.050Zhttps://docs.openclaw.ai/ko/pi2026-05-01T07:59:51.085Zhttps://docs.openclaw.ai/ko/pi-dev2026-05-01T07:59:51.042Zhttps://docs.openclaw.ai/ko/platforms/android2026-05-01T07:59:51.081Zhttps://docs.openclaw.ai/ko/platforms2026-05-01T07:59:51.080Zhttps://docs.openclaw.ai/ko/platforms/ios2026-05-01T07:59:51.082Zhttps://docs.openclaw.ai/ko/platforms/linux2026-05-01T07:59:51.073Zhttps://docs.openclaw.ai/ko/platforms/mac/bundled-gateway2026-05-01T07:59:51.072Zhttps://docs.openclaw.ai/ko/platforms/mac/canvas2026-05-01T07:59:51.071Zhttps://docs.openclaw.ai/ko/platforms/mac/child-process2026-05-01T07:59:51.123Zhttps://docs.openclaw.ai/ko/platforms/mac/dev-setup2026-05-01T07:59:51.119Zhttps://docs.openclaw.ai/ko/platforms/mac/health2026-05-01T07:59:51.116Zhttps://docs.openclaw.ai/ko/platforms/mac/icon2026-05-01T07:59:51.114Zhttps://docs.openclaw.ai/ko/platforms/mac/logging2026-05-01T07:59:51.122Zhttps://docs.openclaw.ai/ko/platforms/mac/menu-bar2026-05-01T07:59:51.122Zhttps://docs.openclaw.ai/ko/platforms/mac/peekaboo2026-05-01T07:59:51.115Zhttps://docs.openclaw.ai/ko/platforms/mac/permissions2026-05-01T07:59:51.116Zhttps://docs.openclaw.ai/ko/platforms/mac/remote2026-05-01T07:59:51.120Zhttps://docs.openclaw.ai/ko/platforms/mac/signing2026-05-01T07:59:51.115Zhttps://docs.openclaw.ai/ko/platforms/mac/skills2026-05-01T07:59:51.156Zhttps://docs.openclaw.ai/ko/platforms/mac/voice-overlay2026-05-01T07:59:51.155Zhttps://docs.openclaw.ai/ko/platforms/mac/voicewake2026-05-01T07:59:51.154Zhttps://docs.openclaw.ai/ko/platforms/mac/webchat2026-05-01T07:59:51.143Zhttps://docs.openclaw.ai/ko/platforms/mac/xpc2026-05-01T07:59:51.145Zhttps://docs.openclaw.ai/ko/platforms/macos2026-05-01T07:59:51.145Zhttps://docs.openclaw.ai/ko/platforms/windows2026-05-01T07:59:51.143Zhttps://docs.openclaw.ai/ko/plugins/architecture2026-05-01T07:59:51.199Zhttps://docs.openclaw.ai/ko/plugins/architecture-internals2026-05-01T07:59:51.200Zhttps://docs.openclaw.ai/ko/plugins/building-plugins2026-05-01T07:59:51.196Zhttps://docs.openclaw.ai/ko/plugins/bundles2026-05-01T07:59:51.189Zhttps://docs.openclaw.ai/ko/plugins/codex-computer-use2026-05-01T07:59:51.188Zhttps://docs.openclaw.ai/ko/plugins/codex-harness2026-05-01T07:59:51.186Zhttps://docs.openclaw.ai/ko/plugins/community2026-05-01T07:59:51.188Zhttps://docs.openclaw.ai/ko/plugins/compatibility2026-05-01T07:59:51.187Zhttps://docs.openclaw.ai/ko/plugins/google-meet2026-05-01T07:59:51.185Zhttps://docs.openclaw.ai/ko/plugins/hooks2026-05-01T07:59:51.270Zhttps://docs.openclaw.ai/ko/plugins/manifest2026-05-01T07:59:51.271Zhttps://docs.openclaw.ai/ko/plugins/memory-lancedb2026-05-01T07:59:51.266Zhttps://docs.openclaw.ai/ko/plugins/memory-wiki2026-05-01T07:59:51.243Zhttps://docs.openclaw.ai/ko/plugins/message-presentation2026-05-01T07:59:51.263Zhttps://docs.openclaw.ai/ko/plugins/sdk-agent-harness2026-05-01T07:59:51.266Zhttps://docs.openclaw.ai/ko/plugins/sdk-channel-plugins2026-05-01T07:59:51.262Zhttps://docs.openclaw.ai/ko/plugins/sdk-channel-turn2026-05-01T07:59:51.261Zhttps://docs.openclaw.ai/ko/plugins/sdk-entrypoints2026-05-01T07:59:51.262Zhttps://docs.openclaw.ai/ko/plugins/sdk-migration2026-05-01T07:59:51.265Zhttps://docs.openclaw.ai/ko/plugins/sdk-overview2026-05-01T07:59:51.313Zhttps://docs.openclaw.ai/ko/plugins/sdk-provider-plugins2026-05-01T07:59:51.312Zhttps://docs.openclaw.ai/ko/plugins/sdk-runtime2026-05-01T07:59:51.311Zhttps://docs.openclaw.ai/ko/plugins/sdk-setup2026-05-01T07:59:51.304Zhttps://docs.openclaw.ai/ko/plugins/sdk-subpaths2026-05-01T07:59:51.307Zhttps://docs.openclaw.ai/ko/plugins/sdk-testing2026-05-01T07:59:51.312Zhttps://docs.openclaw.ai/ko/plugins/skill-workshop2026-05-01T07:59:51.303Zhttps://docs.openclaw.ai/ko/plugins/voice-call2026-05-01T07:59:51.304Zhttps://docs.openclaw.ai/ko/plugins/webhooks2026-05-01T07:59:51.303Zhttps://docs.openclaw.ai/ko/plugins/zalouser2026-05-01T07:59:51.302Zhttps://docs.openclaw.ai/ko/prose2026-05-01T07:59:51.342Zhttps://docs.openclaw.ai/ko/providers/alibaba2026-05-01T07:59:51.341Zhttps://docs.openclaw.ai/ko/providers/anthropic2026-05-01T07:59:51.339Zhttps://docs.openclaw.ai/ko/providers/arcee2026-05-01T07:59:51.341Zhttps://docs.openclaw.ai/ko/providers/azure-speech2026-05-01T07:59:51.337Zhttps://docs.openclaw.ai/ko/providers/bedrock2026-05-01T07:59:51.340Zhttps://docs.openclaw.ai/ko/providers/bedrock-mantle2026-05-01T07:59:51.332Zhttps://docs.openclaw.ai/ko/providers/chutes2026-05-01T07:59:51.333Zhttps://docs.openclaw.ai/ko/providers/claude-max-api-proxy2026-05-01T07:59:51.332Zhttps://docs.openclaw.ai/ko/providers/cloudflare-ai-gateway2026-05-01T07:59:51.371Zhttps://docs.openclaw.ai/ko/providers/comfy2026-05-01T07:59:51.370Zhttps://docs.openclaw.ai/ko/providers/deepgram2026-05-01T07:59:51.370Zhttps://docs.openclaw.ai/ko/providers/deepinfra2026-05-01T07:59:51.364Zhttps://docs.openclaw.ai/ko/providers/deepseek2026-05-01T07:59:51.364Zhttps://docs.openclaw.ai/ko/providers/elevenlabs2026-05-01T07:59:51.367Zhttps://docs.openclaw.ai/ko/providers/fal2026-05-01T07:59:51.362Zhttps://docs.openclaw.ai/ko/providers/fireworks2026-05-01T07:59:51.362Zhttps://docs.openclaw.ai/ko/providers/github-copilot2026-05-01T07:59:51.363Zhttps://docs.openclaw.ai/ko/providers/glm2026-05-01T07:59:51.368Zhttps://docs.openclaw.ai/ko/providers/google2026-05-01T07:59:51.408Zhttps://docs.openclaw.ai/ko/providers/gradium2026-05-01T07:59:51.411Zhttps://docs.openclaw.ai/ko/providers/groq2026-05-01T07:59:51.407Zhttps://docs.openclaw.ai/ko/providers/huggingface2026-05-01T07:59:51.395Zhttps://docs.openclaw.ai/ko/providers2026-05-01T07:59:51.390Zhttps://docs.openclaw.ai/ko/providers/inferrs2026-05-01T07:59:51.390Zhttps://docs.openclaw.ai/ko/providers/inworld2026-05-01T07:59:51.410Zhttps://docs.openclaw.ai/ko/providers/kilocode2026-05-01T07:59:51.409Zhttps://docs.openclaw.ai/ko/providers/litellm2026-05-01T07:59:51.391Zhttps://docs.openclaw.ai/ko/providers/lmstudio2026-05-01T07:59:51.391Zhttps://docs.openclaw.ai/ko/providers/minimax2026-05-01T07:59:51.440Zhttps://docs.openclaw.ai/ko/providers/mistral2026-05-01T07:59:51.432Zhttps://docs.openclaw.ai/ko/providers/models2026-05-01T07:59:51.431Zhttps://docs.openclaw.ai/ko/providers/moonshot2026-05-01T07:59:51.441Zhttps://docs.openclaw.ai/ko/providers/nvidia2026-05-01T07:59:51.440Zhttps://docs.openclaw.ai/ko/providers/ollama2026-05-01T07:59:51.432Zhttps://docs.openclaw.ai/ko/providers/openai2026-05-01T07:59:51.439Zhttps://docs.openclaw.ai/ko/providers/opencode2026-05-01T07:59:51.437Zhttps://docs.openclaw.ai/ko/providers/opencode-go2026-05-01T07:59:51.431Zhttps://docs.openclaw.ai/ko/providers/openrouter2026-05-01T07:59:51.430Zhttps://docs.openclaw.ai/ko/providers/perplexity-provider2026-05-01T07:59:51.470Zhttps://docs.openclaw.ai/ko/providers/qianfan2026-05-01T07:59:51.468Zhttps://docs.openclaw.ai/ko/providers/qwen2026-05-01T07:59:51.469Zhttps://docs.openclaw.ai/ko/providers/runway2026-05-01T07:59:51.469Zhttps://docs.openclaw.ai/ko/providers/sglang2026-05-01T07:59:51.468Zhttps://docs.openclaw.ai/ko/providers/stepfun2026-05-01T07:59:51.461Zhttps://docs.openclaw.ai/ko/providers/synthetic2026-05-01T07:59:51.460Zhttps://docs.openclaw.ai/ko/providers/tencent2026-05-01T07:59:51.461Zhttps://docs.openclaw.ai/ko/providers/together2026-05-01T07:59:51.460Zhttps://docs.openclaw.ai/ko/providers/venice2026-05-01T07:59:51.508Zhttps://docs.openclaw.ai/ko/providers/vercel-ai-gateway2026-05-01T07:59:51.511Zhttps://docs.openclaw.ai/ko/providers/vllm2026-05-01T07:59:51.492Zhttps://docs.openclaw.ai/ko/providers/volcengine2026-05-01T07:59:51.511Zhttps://docs.openclaw.ai/ko/providers/vydra2026-05-01T07:59:51.510Zhttps://docs.openclaw.ai/ko/providers/xai2026-05-01T07:59:51.496Zhttps://docs.openclaw.ai/ko/providers/xiaomi2026-05-01T07:59:51.491Zhttps://docs.openclaw.ai/ko/providers/zai2026-05-01T07:59:51.491Zhttps://docs.openclaw.ai/ko/reference/AGENTS.default2026-05-01T07:59:51.490Zhttps://docs.openclaw.ai/ko/reference/RELEASING2026-05-01T07:59:51.489Zhttps://docs.openclaw.ai/ko/reference/api-usage-costs2026-05-01T07:59:51.540Zhttps://docs.openclaw.ai/ko/reference/credits2026-05-01T07:59:51.534Zhttps://docs.openclaw.ai/ko/reference/device-models2026-05-01T07:59:51.541Zhttps://docs.openclaw.ai/ko/reference/full-release-validation2026-05-01T07:59:51.534Zhttps://docs.openclaw.ai/ko/reference/memory-config2026-05-01T07:59:51.533Zhttps://docs.openclaw.ai/ko/reference/openclaw-sdk-api-design2026-05-01T07:59:51.533Zhttps://docs.openclaw.ai/ko/reference/prompt-caching2026-05-01T07:59:51.532Zhttps://docs.openclaw.ai/ko/reference/rich-output-protocol2026-05-01T07:59:51.537Zhttps://docs.openclaw.ai/ko/reference/rpc2026-05-01T07:59:51.532Zhttps://docs.openclaw.ai/ko/reference/secretref-credential-surface2026-05-01T07:59:51.571Zhttps://docs.openclaw.ai/ko/reference/session-management-compaction2026-05-01T07:59:51.569Zhttps://docs.openclaw.ai/ko/reference/templates/AGENTS2026-05-01T07:59:51.566Zhttps://docs.openclaw.ai/ko/reference/templates/BOOT2026-05-01T07:59:51.568Zhttps://docs.openclaw.ai/ko/reference/templates/BOOTSTRAP2026-05-01T07:59:51.570Zhttps://docs.openclaw.ai/ko/reference/templates/HEARTBEAT2026-05-01T07:59:51.562Zhttps://docs.openclaw.ai/ko/reference/templates/IDENTITY2026-05-01T07:59:51.561Zhttps://docs.openclaw.ai/ko/reference/templates/SOUL2026-05-01T07:59:51.611Zhttps://docs.openclaw.ai/ko/reference/templates/TOOLS2026-05-01T07:59:51.600Zhttps://docs.openclaw.ai/ko/reference/templates/USER2026-05-01T07:59:51.592Zhttps://docs.openclaw.ai/ko/reference/test2026-05-01T07:59:51.590Zhttps://docs.openclaw.ai/ko/reference/token-use2026-05-01T07:59:51.591Zhttps://docs.openclaw.ai/ko/reference/transcript-hygiene2026-05-01T07:59:51.591Zhttps://docs.openclaw.ai/ko/reference/wizard2026-05-01T07:59:51.598Zhttps://docs.openclaw.ai/ko/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:51.596Zhttps://docs.openclaw.ai/ko/security/THREAT-MODEL-ATLAS2026-05-01T07:59:51.640Zhttps://docs.openclaw.ai/ko/security/formal-verification2026-05-01T07:59:51.639Zhttps://docs.openclaw.ai/ko/security/network-proxy2026-05-01T07:59:51.638Zhttps://docs.openclaw.ai/ko/start/bootstrapping2026-05-01T07:59:51.629Zhttps://docs.openclaw.ai/ko/start/docs-directory2026-05-01T07:59:51.639Zhttps://docs.openclaw.ai/ko/start/getting-started2026-05-01T07:59:51.635Zhttps://docs.openclaw.ai/ko/start/hubs2026-05-01T07:59:51.638Zhttps://docs.openclaw.ai/ko/start/lore2026-05-01T07:59:51.630Zhttps://docs.openclaw.ai/ko/start/onboarding2026-05-01T07:59:51.631Zhttps://docs.openclaw.ai/ko/start/onboarding-overview2026-05-01T07:59:51.631Zhttps://docs.openclaw.ai/ko/start/openclaw2026-05-01T07:59:51.671Zhttps://docs.openclaw.ai/ko/start/setup2026-05-01T07:59:51.669Zhttps://docs.openclaw.ai/ko/start/showcase2026-04-24T17:33:18.582Zhttps://docs.openclaw.ai/ko/start/wizard2026-05-01T07:59:51.668Zhttps://docs.openclaw.ai/ko/start/wizard-cli-automation2026-05-01T07:59:51.661Zhttps://docs.openclaw.ai/ko/start/wizard-cli-reference2026-05-01T07:59:51.667Zhttps://docs.openclaw.ai/ko/tools/acp-agents2026-05-01T07:59:51.661Zhttps://docs.openclaw.ai/ko/tools/acp-agents-setup2026-05-01T07:59:51.662Zhttps://docs.openclaw.ai/ko/tools/agent-send2026-05-01T07:59:51.713Zhttps://docs.openclaw.ai/ko/tools/apply-patch2026-05-01T07:59:51.696Zhttps://docs.openclaw.ai/ko/tools/brave-search2026-05-01T07:59:51.694Zhttps://docs.openclaw.ai/ko/tools/browser2026-05-01T07:59:51.698Zhttps://docs.openclaw.ai/ko/tools/browser-control2026-05-01T07:59:51.695Zhttps://docs.openclaw.ai/ko/tools/browser-linux-troubleshooting2026-05-01T07:59:51.697Zhttps://docs.openclaw.ai/ko/tools/browser-login2026-05-01T07:59:51.701Zhttps://docs.openclaw.ai/ko/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:51.694Zhttps://docs.openclaw.ai/ko/tools/btw2026-05-01T07:59:51.699Zhttps://docs.openclaw.ai/ko/tools/clawhub2026-05-01T07:59:51.742Zhttps://docs.openclaw.ai/ko/tools/code-execution2026-05-01T07:59:51.731Zhttps://docs.openclaw.ai/ko/tools/creating-skills2026-05-01T07:59:51.732Zhttps://docs.openclaw.ai/ko/tools/diffs2026-05-01T07:59:51.738Zhttps://docs.openclaw.ai/ko/tools/duckduckgo-search2026-05-01T07:59:51.739Zhttps://docs.openclaw.ai/ko/tools/elevated2026-05-01T07:59:51.741Zhttps://docs.openclaw.ai/ko/tools/exa-search2026-05-01T07:59:51.733Zhttps://docs.openclaw.ai/ko/tools/exec2026-05-01T07:59:51.731Zhttps://docs.openclaw.ai/ko/tools/exec-approvals2026-05-01T07:59:51.740Zhttps://docs.openclaw.ai/ko/tools/exec-approvals-advanced2026-05-01T07:59:51.741Zhttps://docs.openclaw.ai/ko/tools/firecrawl2026-05-01T07:59:51.771Zhttps://docs.openclaw.ai/ko/tools/gemini-search2026-05-01T07:59:51.772Zhttps://docs.openclaw.ai/ko/tools/grok-search2026-05-01T07:59:51.770Zhttps://docs.openclaw.ai/ko/tools/image-generation2026-05-01T07:59:51.770Zhttps://docs.openclaw.ai/ko/tools2026-05-01T07:59:51.769Zhttps://docs.openclaw.ai/ko/tools/kimi-search2026-05-01T07:59:51.761Zhttps://docs.openclaw.ai/ko/tools/llm-task2026-05-01T07:59:51.762Zhttps://docs.openclaw.ai/ko/tools/lobster2026-05-01T07:59:51.763Zhttps://docs.openclaw.ai/ko/tools/loop-detection2026-05-01T07:59:51.762Zhttps://docs.openclaw.ai/ko/tools/media-overview2026-05-01T07:59:51.768Zhttps://docs.openclaw.ai/ko/tools/minimax-search2026-05-01T07:59:51.800Zhttps://docs.openclaw.ai/ko/tools/multi-agent-sandbox-tools2026-05-01T07:59:51.801Zhttps://docs.openclaw.ai/ko/tools/music-generation2026-05-01T07:59:51.799Zhttps://docs.openclaw.ai/ko/tools/ollama-search2026-05-01T07:59:51.800Zhttps://docs.openclaw.ai/ko/tools/pdf2026-05-01T07:59:51.799Zhttps://docs.openclaw.ai/ko/tools/perplexity-search2026-05-01T07:59:51.796Zhttps://docs.openclaw.ai/ko/tools/plugin2026-05-01T07:59:51.791Zhttps://docs.openclaw.ai/ko/tools/reactions2026-05-01T07:59:51.792Zhttps://docs.openclaw.ai/ko/tools/searxng-search2026-05-01T07:59:51.791Zhttps://docs.openclaw.ai/ko/tools/skills2026-05-01T07:59:51.840Zhttps://docs.openclaw.ai/ko/tools/skills-config2026-05-01T07:59:51.790Zhttps://docs.openclaw.ai/ko/tools/slash-commands2026-05-01T07:59:51.829Zhttps://docs.openclaw.ai/ko/tools/subagents2026-05-01T07:59:51.838Zhttps://docs.openclaw.ai/ko/tools/tavily2026-05-01T07:59:51.838Zhttps://docs.openclaw.ai/ko/tools/thinking2026-05-01T07:59:51.839Zhttps://docs.openclaw.ai/ko/tools/tokenjuice2026-05-01T07:59:51.834Zhttps://docs.openclaw.ai/ko/tools/trajectory2026-05-01T07:59:51.829Zhttps://docs.openclaw.ai/ko/tools/tts2026-05-01T07:59:51.837Zhttps://docs.openclaw.ai/ko/tools/video-generation2026-05-01T07:59:51.828Zhttps://docs.openclaw.ai/ko/tools/web2026-05-01T07:59:51.869Zhttps://docs.openclaw.ai/ko/tools/web-fetch2026-05-01T07:59:51.828Zhttps://docs.openclaw.ai/ko/vps2026-05-01T07:59:51.862Zhttps://docs.openclaw.ai/ko/web/control-ui2026-05-01T07:59:51.865Zhttps://docs.openclaw.ai/ko/web/dashboard2026-05-01T07:59:51.867Zhttps://docs.openclaw.ai/ko/web2026-05-01T07:59:51.866Zhttps://docs.openclaw.ai/ko/web/tui2026-05-01T07:59:51.861Zhttps://docs.openclaw.ai/ko/web/webchat2026-05-01T07:59:51.860Zhttps://docs.openclaw.ai/logging2026-05-01T07:59:51.861Zhttps://docs.openclaw.ai/network2026-05-01T07:59:51.865Zhttps://docs.openclaw.ai/nl/auth-credential-semantics2026-05-01T07:59:51.896Zhttps://docs.openclaw.ai/nl/automation/cron-jobs2026-05-01T07:59:51.897Zhttps://docs.openclaw.ai/nl/automation/hooks2026-05-01T07:59:51.890Zhttps://docs.openclaw.ai/nl/automation2026-05-01T07:59:51.888Zhttps://docs.openclaw.ai/nl/automation/standing-orders2026-05-01T07:59:51.947Zhttps://docs.openclaw.ai/nl/automation/taskflow2026-05-01T07:59:51.950Zhttps://docs.openclaw.ai/nl/automation/tasks2026-05-01T11:27:08.349Zhttps://docs.openclaw.ai/nl/channels/bluebubbles2026-05-01T11:27:08.351Zhttps://docs.openclaw.ai/nl/channels/broadcast-groups2026-05-01T07:59:51.944Zhttps://docs.openclaw.ai/nl/channels/channel-routing2026-05-01T07:59:51.924Zhttps://docs.openclaw.ai/nl/channels/discord2026-05-01T11:27:08.356Zhttps://docs.openclaw.ai/nl/channels/feishu2026-05-01T07:59:51.989Zhttps://docs.openclaw.ai/nl/channels/googlechat2026-05-01T07:59:51.987Zhttps://docs.openclaw.ai/nl/channels/group-messages2026-05-01T07:59:51.988Zhttps://docs.openclaw.ai/nl/channels/groups2026-05-01T11:27:08.354Zhttps://docs.openclaw.ai/nl/channels/imessage2026-05-01T07:59:51.981Zhttps://docs.openclaw.ai/nl/channels2026-05-01T07:59:51.984Zhttps://docs.openclaw.ai/nl/channels/irc2026-05-01T07:59:51.989Zhttps://docs.openclaw.ai/nl/channels/line2026-05-01T07:59:51.981Zhttps://docs.openclaw.ai/nl/channels/location2026-05-01T07:59:51.980Zhttps://docs.openclaw.ai/nl/channels/matrix2026-05-01T07:59:52.008Zhttps://docs.openclaw.ai/nl/channels/matrix-migration2026-05-01T07:59:51.979Zhttps://docs.openclaw.ai/nl/channels/matrix-push-rules2026-05-01T07:59:52.019Zhttps://docs.openclaw.ai/nl/channels/mattermost2026-05-01T07:59:52.018Zhttps://docs.openclaw.ai/nl/channels/msteams2026-05-01T07:59:52.018Zhttps://docs.openclaw.ai/nl/channels/nextcloud-talk2026-05-01T07:59:52.011Zhttps://docs.openclaw.ai/nl/channels/nostr2026-05-01T07:59:52.017Zhttps://docs.openclaw.ai/nl/channels/pairing2026-05-01T07:59:52.009Zhttps://docs.openclaw.ai/nl/channels/qa-channel2026-05-01T11:27:08.351Zhttps://docs.openclaw.ai/nl/channels/qqbot2026-05-01T07:59:52.016Zhttps://docs.openclaw.ai/nl/channels/signal2026-05-01T07:59:52.012Zhttps://docs.openclaw.ai/nl/channels/slack2026-05-01T07:59:52.050Zhttps://docs.openclaw.ai/nl/channels/synology-chat2026-05-01T07:59:52.049Zhttps://docs.openclaw.ai/nl/channels/telegram2026-05-01T07:59:52.049Zhttps://docs.openclaw.ai/nl/channels/tlon2026-05-01T07:59:52.040Zhttps://docs.openclaw.ai/nl/channels/troubleshooting2026-05-01T07:59:52.041Zhttps://docs.openclaw.ai/nl/channels/twitch2026-05-01T07:59:52.042Zhttps://docs.openclaw.ai/nl/channels/wechat2026-05-01T07:59:52.040Zhttps://docs.openclaw.ai/nl/channels/whatsapp2026-05-01T07:59:52.041Zhttps://docs.openclaw.ai/nl/channels/yuanbao2026-05-01T07:59:52.042Zhttps://docs.openclaw.ai/nl/channels/zalo2026-05-01T07:59:52.047Zhttps://docs.openclaw.ai/nl/channels/zalouser2026-05-01T07:59:52.092Zhttps://docs.openclaw.ai/nl/ci2026-05-01T11:27:08.353Zhttps://docs.openclaw.ai/nl/cli/acp2026-05-01T07:59:52.084Zhttps://docs.openclaw.ai/nl/cli/agent2026-05-01T07:59:52.087Zhttps://docs.openclaw.ai/nl/cli/agents2026-05-01T07:59:52.085Zhttps://docs.openclaw.ai/nl/cli/approvals2026-05-01T07:59:52.069Zhttps://docs.openclaw.ai/nl/cli/backup2026-05-01T07:59:52.086Zhttps://docs.openclaw.ai/nl/cli/browser2026-05-01T07:59:52.069Zhttps://docs.openclaw.ai/nl/cli/channels2026-05-01T11:27:08.352Zhttps://docs.openclaw.ai/nl/cli/clawbot2026-05-01T07:59:52.070Zhttps://docs.openclaw.ai/nl/cli/commitments2026-05-01T07:59:52.120Zhttps://docs.openclaw.ai/nl/cli/completion2026-05-01T07:59:52.118Zhttps://docs.openclaw.ai/nl/cli/config2026-05-01T07:59:52.117Zhttps://docs.openclaw.ai/nl/cli/configure2026-05-01T11:27:08.350Zhttps://docs.openclaw.ai/nl/cli/cron2026-05-01T07:59:52.119Zhttps://docs.openclaw.ai/nl/cli/daemon2026-05-01T07:59:52.111Zhttps://docs.openclaw.ai/nl/cli/dashboard2026-05-01T07:59:52.118Zhttps://docs.openclaw.ai/nl/cli/devices2026-05-01T07:59:52.111Zhttps://docs.openclaw.ai/nl/cli/directory2026-05-01T07:59:52.110Zhttps://docs.openclaw.ai/nl/cli/dns2026-05-01T07:59:52.149Zhttps://docs.openclaw.ai/nl/cli/docs2026-05-01T07:59:52.146Zhttps://docs.openclaw.ai/nl/cli/doctor2026-05-01T07:59:52.147Zhttps://docs.openclaw.ai/nl/cli/flows2026-05-01T07:59:52.148Zhttps://docs.openclaw.ai/nl/cli/gateway2026-05-01T11:27:08.355Zhttps://docs.openclaw.ai/nl/cli/health2026-05-01T07:59:52.144Zhttps://docs.openclaw.ai/nl/cli/hooks2026-05-01T07:59:52.147Zhttps://docs.openclaw.ai/nl/cli2026-05-01T07:59:52.140Zhttps://docs.openclaw.ai/nl/cli/infer2026-05-01T07:59:52.140Zhttps://docs.openclaw.ai/nl/cli/logs2026-05-01T07:59:52.139Zhttps://docs.openclaw.ai/nl/cli/mcp2026-05-01T07:59:52.186Zhttps://docs.openclaw.ai/nl/cli/memory2026-05-01T07:59:52.185Zhttps://docs.openclaw.ai/nl/cli/message2026-05-01T07:59:52.188Zhttps://docs.openclaw.ai/nl/cli/migrate2026-05-01T07:59:52.187Zhttps://docs.openclaw.ai/nl/cli/models2026-05-01T11:27:08.345Zhttps://docs.openclaw.ai/nl/cli/node2026-05-01T07:59:52.173Zhttps://docs.openclaw.ai/nl/cli/nodes2026-05-01T07:59:52.168Zhttps://docs.openclaw.ai/nl/cli/onboard2026-05-01T11:27:11.553Zhttps://docs.openclaw.ai/nl/cli/pairing2026-05-01T07:59:52.168Zhttps://docs.openclaw.ai/nl/cli/plugins2026-05-01T11:27:11.558Zhttps://docs.openclaw.ai/nl/cli/proxy2026-05-01T11:27:11.552Zhttps://docs.openclaw.ai/nl/cli/qr2026-05-01T07:59:52.208Zhttps://docs.openclaw.ai/nl/cli/reset2026-05-01T07:59:52.214Zhttps://docs.openclaw.ai/nl/cli/sandbox2026-05-01T07:59:52.216Zhttps://docs.openclaw.ai/nl/cli/secrets2026-05-01T07:59:52.215Zhttps://docs.openclaw.ai/nl/cli/security2026-05-01T07:59:52.209Zhttps://docs.openclaw.ai/nl/cli/sessions2026-05-01T07:59:52.208Zhttps://docs.openclaw.ai/nl/cli/setup2026-05-01T07:59:52.207Zhttps://docs.openclaw.ai/nl/cli/skills2026-05-01T07:59:52.211Zhttps://docs.openclaw.ai/nl/cli/status2026-05-01T07:59:52.207Zhttps://docs.openclaw.ai/nl/cli/system2026-05-01T07:59:52.246Zhttps://docs.openclaw.ai/nl/cli/tasks2026-05-01T07:59:52.245Zhttps://docs.openclaw.ai/nl/cli/tui2026-05-01T07:59:52.244Zhttps://docs.openclaw.ai/nl/cli/uninstall2026-05-01T07:59:52.239Zhttps://docs.openclaw.ai/nl/cli/update2026-05-01T11:27:11.549Zhttps://docs.openclaw.ai/nl/cli/voicecall2026-05-01T11:27:11.550Zhttps://docs.openclaw.ai/nl/cli/webhooks2026-05-01T07:59:52.237Zhttps://docs.openclaw.ai/nl/cli/wiki2026-05-01T07:59:52.238Zhttps://docs.openclaw.ai/nl/concepts/active-memory2026-05-01T07:59:52.237Zhttps://docs.openclaw.ai/nl/concepts/agent2026-05-01T07:59:52.273Zhttps://docs.openclaw.ai/nl/concepts/agent-loop2026-05-01T07:59:52.236Zhttps://docs.openclaw.ai/nl/concepts/agent-runtimes2026-05-01T07:59:52.286Zhttps://docs.openclaw.ai/nl/concepts/agent-workspace2026-05-01T07:59:52.285Zhttps://docs.openclaw.ai/nl/concepts/architecture2026-05-01T07:59:52.270Zhttps://docs.openclaw.ai/nl/concepts/channel-docking2026-05-01T07:59:52.273Zhttps://docs.openclaw.ai/nl/concepts/commitments2026-05-01T11:27:11.551Zhttps://docs.openclaw.ai/nl/concepts/compaction2026-05-01T07:59:52.265Zhttps://docs.openclaw.ai/nl/concepts/context2026-05-01T07:59:52.265Zhttps://docs.openclaw.ai/nl/concepts/context-engine2026-05-01T07:59:52.272Zhttps://docs.openclaw.ai/nl/concepts/delegate-architecture2026-05-01T07:59:52.264Zhttps://docs.openclaw.ai/nl/concepts/dreaming2026-05-01T07:59:52.315Zhttps://docs.openclaw.ai/nl/concepts/experimental-features2026-05-01T07:59:52.313Zhttps://docs.openclaw.ai/nl/concepts/features2026-05-01T07:59:52.310Zhttps://docs.openclaw.ai/nl/concepts/markdown-formatting2026-05-01T07:59:52.313Zhttps://docs.openclaw.ai/nl/concepts/memory2026-05-01T07:59:52.306Zhttps://docs.openclaw.ai/nl/concepts/memory-builtin2026-05-01T07:59:52.314Zhttps://docs.openclaw.ai/nl/concepts/memory-honcho2026-05-01T07:59:52.305Zhttps://docs.openclaw.ai/nl/concepts/memory-qmd2026-05-01T07:59:52.306Zhttps://docs.openclaw.ai/nl/concepts/memory-search2026-05-01T07:59:52.312Zhttps://docs.openclaw.ai/nl/concepts/messages2026-05-01T07:59:52.304Zhttps://docs.openclaw.ai/nl/concepts/model-failover2026-05-01T07:59:52.346Zhttps://docs.openclaw.ai/nl/concepts/model-providers2026-05-01T07:59:52.345Zhttps://docs.openclaw.ai/nl/concepts/models2026-05-01T07:59:52.344Zhttps://docs.openclaw.ai/nl/concepts/multi-agent2026-05-01T07:59:52.337Zhttps://docs.openclaw.ai/nl/concepts/oauth2026-05-01T07:59:52.336Zhttps://docs.openclaw.ai/nl/concepts/openclaw-sdk2026-05-01T11:27:11.559Zhttps://docs.openclaw.ai/nl/concepts/presence2026-05-01T07:59:52.344Zhttps://docs.openclaw.ai/nl/concepts/qa-e2e-automation2026-05-01T07:59:52.336Zhttps://docs.openclaw.ai/nl/concepts/qa-matrix2026-05-01T07:59:52.337Zhttps://docs.openclaw.ai/nl/concepts/queue2026-05-01T07:59:52.386Zhttps://docs.openclaw.ai/nl/concepts/queue-steering2026-05-01T07:59:52.335Zhttps://docs.openclaw.ai/nl/concepts/retry2026-05-01T07:59:52.373Zhttps://docs.openclaw.ai/nl/concepts/session2026-05-01T07:59:52.371Zhttps://docs.openclaw.ai/nl/concepts/session-pruning2026-05-01T07:59:52.373Zhttps://docs.openclaw.ai/nl/concepts/session-tool2026-05-01T07:59:52.374Zhttps://docs.openclaw.ai/nl/concepts/soul2026-05-01T07:59:52.374Zhttps://docs.openclaw.ai/nl/concepts/streaming2026-05-01T07:59:52.366Zhttps://docs.openclaw.ai/nl/concepts/system-prompt2026-05-01T07:59:52.366Zhttps://docs.openclaw.ai/nl/concepts/timezone2026-05-01T07:59:52.365Zhttps://docs.openclaw.ai/nl/concepts/typebox2026-05-01T07:59:52.365Zhttps://docs.openclaw.ai/nl/concepts/typing-indicators2026-05-01T07:59:52.415Zhttps://docs.openclaw.ai/nl/concepts/usage-tracking2026-05-01T07:59:52.413Zhttps://docs.openclaw.ai/nl/date-time2026-05-01T07:59:52.412Zhttps://docs.openclaw.ai/nl/debug/node-issue2026-05-01T07:59:52.412Zhttps://docs.openclaw.ai/nl/diagnostics/flags2026-05-01T07:59:52.414Zhttps://docs.openclaw.ai/nl/gateway/authentication2026-05-01T07:59:52.410Zhttps://docs.openclaw.ai/nl/gateway/background-process2026-05-01T07:59:52.405Zhttps://docs.openclaw.ai/nl/gateway/bonjour2026-05-01T07:59:52.406Zhttps://docs.openclaw.ai/nl/gateway/bridge-protocol2026-05-01T07:59:52.405Zhttps://docs.openclaw.ai/nl/gateway/cli-backends2026-05-01T07:59:52.404Zhttps://docs.openclaw.ai/nl/gateway/config-agents2026-05-01T11:27:11.561Zhttps://docs.openclaw.ai/nl/gateway/config-channels2026-05-01T11:27:11.563Zhttps://docs.openclaw.ai/nl/gateway/config-tools2026-05-01T11:27:11.557Zhttps://docs.openclaw.ai/nl/gateway/configuration2026-05-01T11:27:14.329Zhttps://docs.openclaw.ai/nl/gateway/configuration-examples2026-05-01T07:59:52.442Zhttps://docs.openclaw.ai/nl/gateway/configuration-reference2026-05-01T07:59:52.446Zhttps://docs.openclaw.ai/nl/gateway/diagnostics2026-05-01T07:59:52.435Zhttps://docs.openclaw.ai/nl/gateway/discovery2026-05-01T07:59:52.435Zhttps://docs.openclaw.ai/nl/gateway/doctor2026-05-01T11:27:14.342Zhttps://docs.openclaw.ai/nl/gateway/gateway-lock2026-05-01T07:59:52.434Zhttps://docs.openclaw.ai/nl/gateway/health2026-05-01T07:59:52.490Zhttps://docs.openclaw.ai/nl/gateway/heartbeat2026-05-01T07:59:52.468Zhttps://docs.openclaw.ai/nl/gateway2026-05-01T07:59:52.476Zhttps://docs.openclaw.ai/nl/gateway/local-models2026-05-01T07:59:52.475Zhttps://docs.openclaw.ai/nl/gateway/logging2026-05-01T11:27:14.320Zhttps://docs.openclaw.ai/nl/gateway/multiple-gateways2026-05-01T07:59:52.477Zhttps://docs.openclaw.ai/nl/gateway/network-model2026-05-01T07:59:52.478Zhttps://docs.openclaw.ai/nl/gateway/openai-http-api2026-05-01T07:59:52.469Zhttps://docs.openclaw.ai/nl/gateway/openresponses-http-api2026-05-01T07:59:52.477Zhttps://docs.openclaw.ai/nl/gateway/openshell2026-05-01T07:59:52.467Zhttps://docs.openclaw.ai/nl/gateway/opentelemetry2026-05-01T07:59:52.522Zhttps://docs.openclaw.ai/nl/gateway/pairing2026-05-01T07:59:52.517Zhttps://docs.openclaw.ai/nl/gateway/prometheus2026-05-01T07:59:52.515Zhttps://docs.openclaw.ai/nl/gateway/protocol2026-05-01T11:27:14.325Zhttps://docs.openclaw.ai/nl/gateway/remote2026-05-01T07:59:52.508Zhttps://docs.openclaw.ai/nl/gateway/remote-gateway-readme2026-05-01T07:59:52.509Zhttps://docs.openclaw.ai/nl/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:52.516Zhttps://docs.openclaw.ai/nl/gateway/sandboxing2026-05-01T11:27:14.321Zhttps://docs.openclaw.ai/nl/gateway/secrets2026-05-01T07:59:52.508Zhttps://docs.openclaw.ai/nl/gateway/secrets-plan-contract2026-05-01T07:59:52.515Zhttps://docs.openclaw.ai/nl/gateway/security/audit-checks2026-05-01T07:59:52.555Zhttps://docs.openclaw.ai/nl/gateway/security2026-04-30T01:17:27.580Zhttps://docs.openclaw.ai/nl/gateway/tailscale2026-05-01T07:59:52.551Zhttps://docs.openclaw.ai/nl/gateway/tools-invoke-http-api2026-05-01T07:59:52.552Zhttps://docs.openclaw.ai/nl/gateway/troubleshooting2026-05-01T11:27:14.345Zhttps://docs.openclaw.ai/nl/gateway/trusted-proxy-auth2026-05-01T07:59:52.550Zhttps://docs.openclaw.ai/nl/help/debugging2026-05-01T07:59:52.552Zhttps://docs.openclaw.ai/nl/help/environment2026-05-01T07:59:52.553Zhttps://docs.openclaw.ai/nl/help/faq2026-05-01T07:59:52.548Zhttps://docs.openclaw.ai/nl/help/faq-first-run2026-05-01T07:59:52.544Zhttps://docs.openclaw.ai/nl/help/faq-models2026-05-01T07:59:52.544Zhttps://docs.openclaw.ai/nl/help/gpt55-codex-agentic-parity2026-05-01T07:59:52.595Zhttps://docs.openclaw.ai/nl/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:52.592Zhttps://docs.openclaw.ai/nl/help2026-05-01T07:59:52.594Zhttps://docs.openclaw.ai/nl/help/scripts2026-05-01T07:59:52.594Zhttps://docs.openclaw.ai/nl/help/testing2026-05-01T11:27:14.348Zhttps://docs.openclaw.ai/nl/help/testing-live2026-05-01T07:59:52.595Zhttps://docs.openclaw.ai/nl/help/troubleshooting2026-05-01T07:59:52.574Zhttps://docs.openclaw.ai/nl2026-05-01T07:59:52.575Zhttps://docs.openclaw.ai/nl/install/ansible2026-05-01T11:27:14.318Zhttps://docs.openclaw.ai/nl/install/azure2026-05-01T07:59:52.576Zhttps://docs.openclaw.ai/nl/install/bun2026-05-01T07:59:52.619Zhttps://docs.openclaw.ai/nl/install/clawdock2026-05-01T07:59:52.641Zhttps://docs.openclaw.ai/nl/install/development-channels2026-05-01T07:59:52.640Zhttps://docs.openclaw.ai/nl/install/digitalocean2026-05-01T07:59:52.634Zhttps://docs.openclaw.ai/nl/install/docker2026-05-01T11:27:14.327Zhttps://docs.openclaw.ai/nl/install/docker-vm-runtime2026-05-01T07:59:52.641Zhttps://docs.openclaw.ai/nl/install/exe-dev2026-05-01T07:59:52.634Zhttps://docs.openclaw.ai/nl/install/fly2026-05-01T07:59:52.638Zhttps://docs.openclaw.ai/nl/install/gcp2026-05-01T07:59:52.635Zhttps://docs.openclaw.ai/nl/install/hetzner2026-05-01T07:59:52.618Zhttps://docs.openclaw.ai/nl/install/hostinger2026-05-01T07:59:52.679Zhttps://docs.openclaw.ai/nl/install2026-05-01T07:59:52.678Zhttps://docs.openclaw.ai/nl/install/installer2026-05-01T07:59:52.678Zhttps://docs.openclaw.ai/nl/install/kubernetes2026-05-01T07:59:52.670Zhttps://docs.openclaw.ai/nl/install/macos-vm2026-05-01T07:59:52.672Zhttps://docs.openclaw.ai/nl/install/migrating2026-05-01T07:59:52.671Zhttps://docs.openclaw.ai/nl/install/migrating-claude2026-05-01T07:59:52.671Zhttps://docs.openclaw.ai/nl/install/migrating-hermes2026-05-01T07:59:52.676Zhttps://docs.openclaw.ai/nl/install/nix2026-05-01T07:59:52.676Zhttps://docs.openclaw.ai/nl/install/node2026-05-01T07:59:52.670Zhttps://docs.openclaw.ai/nl/install/northflank2026-05-01T07:59:52.703Zhttps://docs.openclaw.ai/nl/install/oracle2026-05-01T07:59:52.705Zhttps://docs.openclaw.ai/nl/install/podman2026-05-01T07:59:52.698Zhttps://docs.openclaw.ai/nl/install/railway2026-05-01T07:59:52.702Zhttps://docs.openclaw.ai/nl/install/raspberry-pi2026-05-01T07:59:52.704Zhttps://docs.openclaw.ai/nl/install/render2026-05-01T07:59:52.699Zhttps://docs.openclaw.ai/nl/install/uninstall2026-05-01T07:59:52.698Zhttps://docs.openclaw.ai/nl/install/updating2026-05-01T11:27:14.317Zhttps://docs.openclaw.ai/nl/logging2026-05-01T11:27:16.769Zhttps://docs.openclaw.ai/nl/network2026-05-01T07:59:52.697Zhttps://docs.openclaw.ai/nl/nodes/audio2026-05-01T07:59:52.732Zhttps://docs.openclaw.ai/nl/nodes/camera2026-05-01T07:59:52.732Zhttps://docs.openclaw.ai/nl/nodes/images2026-05-01T07:59:52.733Zhttps://docs.openclaw.ai/nl/nodes2026-05-01T07:59:52.730Zhttps://docs.openclaw.ai/nl/nodes/location-command2026-05-01T07:59:52.731Zhttps://docs.openclaw.ai/nl/nodes/media-understanding2026-05-01T07:59:52.731Zhttps://docs.openclaw.ai/nl/nodes/talk2026-05-01T07:59:52.724Zhttps://docs.openclaw.ai/nl/nodes/troubleshooting2026-05-01T07:59:52.723Zhttps://docs.openclaw.ai/nl/nodes/voicewake2026-05-01T07:59:52.724Zhttps://docs.openclaw.ai/nl/pi2026-05-01T07:59:52.764Zhttps://docs.openclaw.ai/nl/pi-dev2026-05-01T07:59:52.773Zhttps://docs.openclaw.ai/nl/platforms/android2026-05-01T07:59:52.771Zhttps://docs.openclaw.ai/nl/platforms2026-05-01T07:59:52.764Zhttps://docs.openclaw.ai/nl/platforms/ios2026-05-01T07:59:52.763Zhttps://docs.openclaw.ai/nl/platforms/linux2026-05-01T07:59:52.763Zhttps://docs.openclaw.ai/nl/platforms/mac/bundled-gateway2026-05-01T07:59:52.766Zhttps://docs.openclaw.ai/nl/platforms/mac/canvas2026-05-01T07:59:52.801Zhttps://docs.openclaw.ai/nl/platforms/mac/child-process2026-05-01T07:59:52.800Zhttps://docs.openclaw.ai/nl/platforms/mac/dev-setup2026-05-01T07:59:52.799Zhttps://docs.openclaw.ai/nl/platforms/mac/health2026-05-01T07:59:52.796Zhttps://docs.openclaw.ai/nl/platforms/mac/icon2026-05-01T07:59:52.791Zhttps://docs.openclaw.ai/nl/platforms/mac/logging2026-05-01T07:59:52.792Zhttps://docs.openclaw.ai/nl/platforms/mac/menu-bar2026-05-01T11:27:16.782Zhttps://docs.openclaw.ai/nl/platforms/mac/peekaboo2026-05-01T07:59:52.792Zhttps://docs.openclaw.ai/nl/platforms/mac/permissions2026-05-01T07:59:52.798Zhttps://docs.openclaw.ai/nl/platforms/mac/remote2026-05-01T07:59:52.791Zhttps://docs.openclaw.ai/nl/platforms/mac/signing2026-05-01T07:59:52.830Zhttps://docs.openclaw.ai/nl/platforms/mac/skills2026-05-01T07:59:52.829Zhttps://docs.openclaw.ai/nl/platforms/mac/voice-overlay2026-05-01T07:59:52.828Zhttps://docs.openclaw.ai/nl/platforms/mac/voicewake2026-05-01T07:59:52.828Zhttps://docs.openclaw.ai/nl/platforms/mac/webchat2026-05-01T07:59:52.821Zhttps://docs.openclaw.ai/nl/platforms/mac/xpc2026-05-01T07:59:52.825Zhttps://docs.openclaw.ai/nl/platforms/macos2026-05-01T07:59:52.827Zhttps://docs.openclaw.ai/nl/platforms/windows2026-05-01T07:59:52.820Zhttps://docs.openclaw.ai/nl/plugins/architecture2026-05-01T07:59:52.865Zhttps://docs.openclaw.ai/nl/plugins/architecture-internals2026-05-01T07:59:52.872Zhttps://docs.openclaw.ai/nl/plugins/building-plugins2026-05-01T11:27:16.787Zhttps://docs.openclaw.ai/nl/plugins/bundles2026-05-01T07:59:52.868Zhttps://docs.openclaw.ai/nl/plugins/codex-computer-use2026-05-01T07:59:52.850Zhttps://docs.openclaw.ai/nl/plugins/codex-harness2026-05-01T11:27:16.790Zhttps://docs.openclaw.ai/nl/plugins/community2026-05-01T07:59:52.851Zhttps://docs.openclaw.ai/nl/plugins/compatibility2026-05-01T07:59:52.849Zhttps://docs.openclaw.ai/nl/plugins/dependency-resolution2026-05-01T11:27:16.781Zhttps://docs.openclaw.ai/nl/plugins/google-meet2026-05-01T11:27:16.791Zhttps://docs.openclaw.ai/nl/plugins/hooks2026-05-01T07:59:52.900Zhttps://docs.openclaw.ai/nl/plugins/manifest2026-05-01T07:59:52.900Zhttps://docs.openclaw.ai/nl/plugins/memory-lancedb2026-05-01T07:59:52.892Zhttps://docs.openclaw.ai/nl/plugins/memory-wiki2026-05-01T07:59:52.902Zhttps://docs.openclaw.ai/nl/plugins/message-presentation2026-05-01T07:59:52.903Zhttps://docs.openclaw.ai/nl/plugins/sdk-agent-harness2026-05-01T07:59:52.902Zhttps://docs.openclaw.ai/nl/plugins/sdk-channel-plugins2026-05-01T07:59:52.892Zhttps://docs.openclaw.ai/nl/plugins/sdk-channel-turn2026-05-01T07:59:52.891Zhttps://docs.openclaw.ai/nl/plugins/sdk-entrypoints2026-05-01T07:59:52.891Zhttps://docs.openclaw.ai/nl/plugins/sdk-migration2026-05-01T07:59:52.939Zhttps://docs.openclaw.ai/nl/plugins/sdk-overview2026-05-01T07:59:52.932Zhttps://docs.openclaw.ai/nl/plugins/sdk-provider-plugins2026-05-01T11:27:16.786Zhttps://docs.openclaw.ai/nl/plugins/sdk-runtime2026-05-01T07:59:52.935Zhttps://docs.openclaw.ai/nl/plugins/sdk-setup2026-05-01T07:59:52.934Zhttps://docs.openclaw.ai/nl/plugins/sdk-subpaths2026-05-01T07:59:52.931Zhttps://docs.openclaw.ai/nl/plugins/sdk-testing2026-05-01T07:59:52.925Zhttps://docs.openclaw.ai/nl/plugins/skill-workshop2026-05-01T07:59:52.924Zhttps://docs.openclaw.ai/nl/plugins/voice-call2026-05-01T11:27:16.797Zhttps://docs.openclaw.ai/nl/plugins/webhooks2026-05-01T07:59:52.923Zhttps://docs.openclaw.ai/nl/plugins/zalouser2026-05-01T07:59:52.982Zhttps://docs.openclaw.ai/nl/prose2026-05-01T07:59:52.976Zhttps://docs.openclaw.ai/nl/providers/alibaba2026-05-01T07:59:52.975Zhttps://docs.openclaw.ai/nl/providers/anthropic2026-05-01T07:59:52.976Zhttps://docs.openclaw.ai/nl/providers/arcee2026-05-01T07:59:52.975Zhttps://docs.openclaw.ai/nl/providers/azure-speech2026-05-01T07:59:52.974Zhttps://docs.openclaw.ai/nl/providers/bedrock2026-05-01T07:59:52.959Zhttps://docs.openclaw.ai/nl/providers/bedrock-mantle2026-05-01T07:59:52.959Zhttps://docs.openclaw.ai/nl/providers/chutes2026-05-01T07:59:52.958Zhttps://docs.openclaw.ai/nl/providers/claude-max-api-proxy2026-05-01T07:59:53.010Zhttps://docs.openclaw.ai/nl/providers/cloudflare-ai-gateway2026-05-01T07:59:53.001Zhttps://docs.openclaw.ai/nl/providers/comfy2026-05-01T07:59:53.006Zhttps://docs.openclaw.ai/nl/providers/deepgram2026-05-01T07:59:53.009Zhttps://docs.openclaw.ai/nl/providers/deepinfra2026-05-01T07:59:53.007Zhttps://docs.openclaw.ai/nl/providers/deepseek2026-05-01T07:59:53.009Zhttps://docs.openclaw.ai/nl/providers/elevenlabs2026-05-01T07:59:53.001Zhttps://docs.openclaw.ai/nl/providers/fal2026-05-01T07:59:53.008Zhttps://docs.openclaw.ai/nl/providers/fireworks2026-05-01T07:59:53.000Zhttps://docs.openclaw.ai/nl/providers/github-copilot2026-05-01T07:59:53.000Zhttps://docs.openclaw.ai/nl/providers/glm2026-05-01T07:59:53.040Zhttps://docs.openclaw.ai/nl/providers/google2026-05-01T07:59:53.039Zhttps://docs.openclaw.ai/nl/providers/gradium2026-05-01T07:59:53.038Zhttps://docs.openclaw.ai/nl/providers/groq2026-05-01T07:59:53.038Zhttps://docs.openclaw.ai/nl/providers/huggingface2026-05-01T07:59:53.031Zhttps://docs.openclaw.ai/nl/providers2026-05-01T07:59:53.035Zhttps://docs.openclaw.ai/nl/providers/inferrs2026-05-01T07:59:53.031Zhttps://docs.openclaw.ai/nl/providers/inworld2026-05-01T07:59:53.032Zhttps://docs.openclaw.ai/nl/providers/kilocode2026-05-01T07:59:53.030Zhttps://docs.openclaw.ai/nl/providers/litellm2026-05-01T07:59:53.030Zhttps://docs.openclaw.ai/nl/providers/lmstudio2026-05-01T07:59:53.079Zhttps://docs.openclaw.ai/nl/providers/minimax2026-05-01T07:59:53.081Zhttps://docs.openclaw.ai/nl/providers/mistral2026-05-01T07:59:53.081Zhttps://docs.openclaw.ai/nl/providers/models2026-05-01T07:59:53.062Zhttps://docs.openclaw.ai/nl/providers/moonshot2026-05-01T07:59:53.061Zhttps://docs.openclaw.ai/nl/providers/nvidia2026-05-01T07:59:53.064Zhttps://docs.openclaw.ai/nl/providers/ollama2026-05-01T07:59:53.062Zhttps://docs.openclaw.ai/nl/providers/openai2026-05-01T07:59:53.063Zhttps://docs.openclaw.ai/nl/providers/opencode2026-05-01T07:59:53.067Zhttps://docs.openclaw.ai/nl/providers/opencode-go2026-05-01T07:59:53.063Zhttps://docs.openclaw.ai/nl/providers/openrouter2026-05-01T07:59:53.111Zhttps://docs.openclaw.ai/nl/providers/perplexity-provider2026-05-01T07:59:53.110Zhttps://docs.openclaw.ai/nl/providers/qianfan2026-05-01T07:59:53.112Zhttps://docs.openclaw.ai/nl/providers/qwen2026-05-01T07:59:53.104Zhttps://docs.openclaw.ai/nl/providers/runway2026-05-01T07:59:53.105Zhttps://docs.openclaw.ai/nl/providers/sglang2026-05-01T07:59:53.103Zhttps://docs.openclaw.ai/nl/providers/stepfun2026-05-01T07:59:53.104Zhttps://docs.openclaw.ai/nl/providers/synthetic2026-05-01T07:59:53.107Zhttps://docs.openclaw.ai/nl/providers/tencent2026-05-01T07:59:53.102Zhttps://docs.openclaw.ai/nl/providers/together2026-05-01T07:59:53.141Zhttps://docs.openclaw.ai/nl/providers/venice2026-05-01T07:59:53.140Zhttps://docs.openclaw.ai/nl/providers/vercel-ai-gateway2026-05-01T07:59:53.140Zhttps://docs.openclaw.ai/nl/providers/vllm2026-05-01T07:59:53.131Zhttps://docs.openclaw.ai/nl/providers/volcengine2026-05-01T07:59:53.133Zhttps://docs.openclaw.ai/nl/providers/vydra2026-05-01T07:59:53.133Zhttps://docs.openclaw.ai/nl/providers/xai2026-05-01T07:59:53.139Zhttps://docs.openclaw.ai/nl/providers/xiaomi2026-05-01T07:59:53.132Zhttps://docs.openclaw.ai/nl/providers/zai2026-05-01T07:59:53.132Zhttps://docs.openclaw.ai/nl/reference/AGENTS.default2026-05-01T07:59:53.137Zhttps://docs.openclaw.ai/nl/reference/RELEASING2026-05-01T11:27:16.793Zhttps://docs.openclaw.ai/nl/reference/api-usage-costs2026-05-01T07:59:53.168Zhttps://docs.openclaw.ai/nl/reference/credits2026-05-01T07:59:53.161Zhttps://docs.openclaw.ai/nl/reference/device-models2026-05-01T07:59:53.166Zhttps://docs.openclaw.ai/nl/reference/full-release-validation2026-05-01T11:27:16.785Zhttps://docs.openclaw.ai/nl/reference/memory-config2026-05-01T07:59:53.169Zhttps://docs.openclaw.ai/nl/reference/openclaw-sdk-api-design2026-05-01T07:59:53.161Zhttps://docs.openclaw.ai/nl/reference/prompt-caching2026-05-01T07:59:53.162Zhttps://docs.openclaw.ai/nl/reference/rich-output-protocol2026-05-01T07:59:53.170Zhttps://docs.openclaw.ai/nl/reference/rpc2026-05-01T07:59:53.160Zhttps://docs.openclaw.ai/nl/reference/secretref-credential-surface2026-05-01T11:27:19.699Zhttps://docs.openclaw.ai/nl/reference/session-management-compaction2026-05-01T07:59:53.207Zhttps://docs.openclaw.ai/nl/reference/templates/AGENTS2026-05-01T07:59:53.202Zhttps://docs.openclaw.ai/nl/reference/templates/BOOT2026-05-01T07:59:53.209Zhttps://docs.openclaw.ai/nl/reference/templates/BOOTSTRAP2026-05-01T07:59:53.210Zhttps://docs.openclaw.ai/nl/reference/templates/HEARTBEAT2026-05-01T07:59:53.203Zhttps://docs.openclaw.ai/nl/reference/templates/IDENTITY2026-05-01T07:59:53.202Zhttps://docs.openclaw.ai/nl/reference/templates/SOUL2026-05-01T07:59:53.241Zhttps://docs.openclaw.ai/nl/reference/templates/TOOLS2026-05-01T07:59:53.241Zhttps://docs.openclaw.ai/nl/reference/templates/USER2026-05-01T07:59:53.239Zhttps://docs.openclaw.ai/nl/reference/test2026-05-01T11:27:19.700Zhttps://docs.openclaw.ai/nl/reference/token-use2026-05-01T07:59:53.234Zhttps://docs.openclaw.ai/nl/reference/transcript-hygiene2026-05-01T07:59:53.233Zhttps://docs.openclaw.ai/nl/reference/wizard2026-05-01T07:59:53.234Zhttps://docs.openclaw.ai/nl/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:53.232Zhttps://docs.openclaw.ai/nl/security/THREAT-MODEL-ATLAS2026-05-01T07:59:53.291Zhttps://docs.openclaw.ai/nl/security/formal-verification2026-05-01T07:59:53.289Zhttps://docs.openclaw.ai/nl/security/network-proxy2026-05-01T11:27:19.702Zhttps://docs.openclaw.ai/nl/start/bootstrapping2026-05-01T07:59:53.270Zhttps://docs.openclaw.ai/nl/start/docs-directory2026-05-01T07:59:53.272Zhttps://docs.openclaw.ai/nl/start/getting-started2026-05-01T07:59:53.270Zhttps://docs.openclaw.ai/nl/start/hubs2026-05-01T07:59:53.261Zhttps://docs.openclaw.ai/nl/start/lore2026-05-01T07:59:53.260Zhttps://docs.openclaw.ai/nl/start/onboarding2026-05-01T07:59:53.260Zhttps://docs.openclaw.ai/nl/start/onboarding-overview2026-05-01T07:59:53.261Zhttps://docs.openclaw.ai/nl/start/openclaw2026-05-01T07:59:53.355Zhttps://docs.openclaw.ai/nl/start/setup2026-05-01T07:59:53.351Zhttps://docs.openclaw.ai/nl/start/showcase2026-04-29T23:34:19.168Zhttps://docs.openclaw.ai/nl/start/wizard2026-05-01T07:59:53.318Zhttps://docs.openclaw.ai/nl/start/wizard-cli-automation2026-05-01T07:59:53.350Zhttps://docs.openclaw.ai/nl/start/wizard-cli-reference2026-05-01T07:59:53.349Zhttps://docs.openclaw.ai/nl/tools/acp-agents2026-05-01T11:27:19.706Zhttps://docs.openclaw.ai/nl/tools/acp-agents-setup2026-05-01T07:59:53.317Zhttps://docs.openclaw.ai/nl/tools/agent-send2026-05-01T07:59:53.393Zhttps://docs.openclaw.ai/nl/tools/apply-patch2026-05-01T07:59:53.392Zhttps://docs.openclaw.ai/nl/tools/brave-search2026-05-01T07:59:53.389Zhttps://docs.openclaw.ai/nl/tools/browser2026-05-01T07:59:53.384Zhttps://docs.openclaw.ai/nl/tools/browser-control2026-05-01T07:59:53.384Zhttps://docs.openclaw.ai/nl/tools/browser-linux-troubleshooting2026-05-01T07:59:53.393Zhttps://docs.openclaw.ai/nl/tools/browser-login2026-05-01T07:59:53.385Zhttps://docs.openclaw.ai/nl/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:53.391Zhttps://docs.openclaw.ai/nl/tools/btw2026-05-01T07:59:53.383Zhttps://docs.openclaw.ai/nl/tools/clawhub2026-05-01T07:59:53.422Zhttps://docs.openclaw.ai/nl/tools/code-execution2026-05-01T07:59:53.421Zhttps://docs.openclaw.ai/nl/tools/creating-skills2026-05-01T07:59:53.421Zhttps://docs.openclaw.ai/nl/tools/diffs2026-05-01T07:59:53.420Zhttps://docs.openclaw.ai/nl/tools/duckduckgo-search2026-05-01T07:59:53.413Zhttps://docs.openclaw.ai/nl/tools/elevated2026-05-01T07:59:53.417Zhttps://docs.openclaw.ai/nl/tools/exa-search2026-05-01T07:59:53.420Zhttps://docs.openclaw.ai/nl/tools/exec2026-05-01T07:59:53.412Zhttps://docs.openclaw.ai/nl/tools/exec-approvals2026-05-01T07:59:53.412Zhttps://docs.openclaw.ai/nl/tools/exec-approvals-advanced2026-05-01T07:59:53.413Zhttps://docs.openclaw.ai/nl/tools/firecrawl2026-05-01T07:59:53.452Zhttps://docs.openclaw.ai/nl/tools/gemini-search2026-05-01T07:59:53.450Zhttps://docs.openclaw.ai/nl/tools/grok-search2026-05-01T07:59:53.451Zhttps://docs.openclaw.ai/nl/tools/image-generation2026-05-01T07:59:53.444Zhttps://docs.openclaw.ai/nl/tools2026-05-01T07:59:53.450Zhttps://docs.openclaw.ai/nl/tools/kimi-search2026-05-01T07:59:53.447Zhttps://docs.openclaw.ai/nl/tools/llm-task2026-05-01T07:59:53.443Zhttps://docs.openclaw.ai/nl/tools/lobster2026-05-01T07:59:53.444Zhttps://docs.openclaw.ai/nl/tools/loop-detection2026-05-01T07:59:53.443Zhttps://docs.openclaw.ai/nl/tools/media-overview2026-05-01T07:59:53.442Zhttps://docs.openclaw.ai/nl/tools/minimax-search2026-05-01T07:59:53.490Zhttps://docs.openclaw.ai/nl/tools/multi-agent-sandbox-tools2026-05-01T07:59:53.490Zhttps://docs.openclaw.ai/nl/tools/music-generation2026-05-01T07:59:53.489Zhttps://docs.openclaw.ai/nl/tools/ollama-search2026-05-01T07:59:53.491Zhttps://docs.openclaw.ai/nl/tools/pdf2026-05-01T07:59:53.486Zhttps://docs.openclaw.ai/nl/tools/perplexity-search2026-05-01T07:59:53.492Zhttps://docs.openclaw.ai/nl/tools/plugin2026-05-01T11:27:19.701Zhttps://docs.openclaw.ai/nl/tools/reactions2026-05-01T07:59:53.482Zhttps://docs.openclaw.ai/nl/tools/searxng-search2026-05-01T07:59:53.482Zhttps://docs.openclaw.ai/nl/tools/skills2026-05-01T07:59:53.519Zhttps://docs.openclaw.ai/nl/tools/skills-config2026-05-01T07:59:53.481Zhttps://docs.openclaw.ai/nl/tools/slash-commands2026-05-01T11:27:19.698Zhttps://docs.openclaw.ai/nl/tools/subagents2026-05-01T07:59:53.520Zhttps://docs.openclaw.ai/nl/tools/tavily2026-05-01T07:59:53.521Zhttps://docs.openclaw.ai/nl/tools/thinking2026-05-01T07:59:53.520Zhttps://docs.openclaw.ai/nl/tools/tokenjuice2026-05-01T07:59:53.517Zhttps://docs.openclaw.ai/nl/tools/trajectory2026-05-01T07:59:53.512Zhttps://docs.openclaw.ai/nl/tools/tts2026-05-01T07:59:53.511Zhttps://docs.openclaw.ai/nl/tools/video-generation2026-05-01T07:59:53.511Zhttps://docs.openclaw.ai/nl/tools/web2026-05-01T07:59:53.550Zhttps://docs.openclaw.ai/nl/tools/web-fetch2026-05-01T07:59:53.510Zhttps://docs.openclaw.ai/nl/vps2026-05-01T07:59:53.549Zhttps://docs.openclaw.ai/nl/web/control-ui2026-05-01T07:59:53.548Zhttps://docs.openclaw.ai/nl/web/dashboard2026-05-01T07:59:53.540Zhttps://docs.openclaw.ai/nl/web2026-05-01T07:59:53.545Zhttps://docs.openclaw.ai/nl/web/tui2026-05-01T07:59:53.541Zhttps://docs.openclaw.ai/nl/web/webchat2026-05-01T07:59:53.548Zhttps://docs.openclaw.ai/nodes/audio2026-05-01T07:59:53.540Zhttps://docs.openclaw.ai/nodes/camera2026-05-01T07:59:53.539Zhttps://docs.openclaw.ai/nodes/images2026-05-01T07:59:53.592Zhttps://docs.openclaw.ai/nodes2026-05-01T07:59:53.585Zhttps://docs.openclaw.ai/nodes/location-command2026-05-01T07:59:53.571Zhttps://docs.openclaw.ai/nodes/media-understanding2026-05-01T07:59:53.570Zhttps://docs.openclaw.ai/nodes/talk2026-05-01T07:59:53.591Zhttps://docs.openclaw.ai/nodes/troubleshooting2026-05-01T07:59:53.588Zhttps://docs.openclaw.ai/nodes/voicewake2026-05-01T07:59:53.586Zhttps://docs.openclaw.ai/pi2026-05-01T07:59:53.569Zhttps://docs.openclaw.ai/pi-dev2026-05-01T07:59:53.570Zhttps://docs.openclaw.ai/pl/auth-credential-semantics2026-05-01T07:59:53.619Zhttps://docs.openclaw.ai/pl/automation/cron-jobs2026-05-01T07:59:53.619Zhttps://docs.openclaw.ai/pl/automation/hooks2026-05-01T07:59:53.611Zhttps://docs.openclaw.ai/pl/automation2026-05-01T07:59:53.617Zhttps://docs.openclaw.ai/pl/automation/standing-orders2026-05-01T07:59:53.651Zhttps://docs.openclaw.ai/pl/automation/taskflow2026-05-01T07:59:53.649Zhttps://docs.openclaw.ai/pl/automation/tasks2026-05-01T10:08:36.815Zhttps://docs.openclaw.ai/pl/channels/bluebubbles2026-05-01T10:08:36.817Zhttps://docs.openclaw.ai/pl/channels/broadcast-groups2026-05-01T07:59:53.640Zhttps://docs.openclaw.ai/pl/channels/channel-routing2026-05-01T07:59:53.642Zhttps://docs.openclaw.ai/pl/channels/discord2026-05-01T07:59:53.640Zhttps://docs.openclaw.ai/pl/channels/feishu2026-05-01T07:59:53.688Zhttps://docs.openclaw.ai/pl/channels/googlechat2026-05-01T07:59:53.691Zhttps://docs.openclaw.ai/pl/channels/group-messages2026-05-01T07:59:53.691Zhttps://docs.openclaw.ai/pl/channels/groups2026-05-01T10:08:36.602Zhttps://docs.openclaw.ai/pl/channels/imessage2026-05-01T07:59:53.678Zhttps://docs.openclaw.ai/pl/channels2026-05-01T07:59:53.673Zhttps://docs.openclaw.ai/pl/channels/irc2026-05-01T07:59:53.673Zhttps://docs.openclaw.ai/pl/channels/line2026-05-01T07:59:53.674Zhttps://docs.openclaw.ai/pl/channels/location2026-05-01T07:59:53.672Zhttps://docs.openclaw.ai/pl/channels/matrix2026-05-01T07:59:53.721Zhttps://docs.openclaw.ai/pl/channels/matrix-migration2026-05-01T07:59:53.671Zhttps://docs.openclaw.ai/pl/channels/matrix-push-rules2026-05-01T07:59:53.720Zhttps://docs.openclaw.ai/pl/channels/mattermost2026-05-01T07:59:53.721Zhttps://docs.openclaw.ai/pl/channels/msteams2026-05-01T07:59:53.719Zhttps://docs.openclaw.ai/pl/channels/nextcloud-talk2026-05-01T07:59:53.719Zhttps://docs.openclaw.ai/pl/channels/nostr2026-05-01T07:59:53.711Zhttps://docs.openclaw.ai/pl/channels/pairing2026-05-01T07:59:53.710Zhttps://docs.openclaw.ai/pl/channels/qa-channel2026-05-01T10:08:36.812Zhttps://docs.openclaw.ai/pl/channels/qqbot2026-05-01T07:59:53.717Zhttps://docs.openclaw.ai/pl/channels/signal2026-05-01T07:59:53.710Zhttps://docs.openclaw.ai/pl/channels/slack2026-05-01T07:59:53.755Zhttps://docs.openclaw.ai/pl/channels/synology-chat2026-05-01T07:59:53.752Zhttps://docs.openclaw.ai/pl/channels/telegram2026-05-01T07:59:53.752Zhttps://docs.openclaw.ai/pl/channels/tlon2026-05-01T07:59:53.743Zhttps://docs.openclaw.ai/pl/channels/troubleshooting2026-05-01T07:59:53.751Zhttps://docs.openclaw.ai/pl/channels/twitch2026-05-01T07:59:53.750Zhttps://docs.openclaw.ai/pl/channels/wechat2026-05-01T07:59:53.743Zhttps://docs.openclaw.ai/pl/channels/whatsapp2026-05-01T07:59:53.742Zhttps://docs.openclaw.ai/pl/channels/yuanbao2026-05-01T07:59:53.744Zhttps://docs.openclaw.ai/pl/channels/zalo2026-05-01T07:59:53.742Zhttps://docs.openclaw.ai/pl/channels/zalouser2026-05-01T07:59:53.786Zhttps://docs.openclaw.ai/pl/ci2026-05-01T10:08:36.816Zhttps://docs.openclaw.ai/pl/cli/acp2026-05-01T07:59:53.785Zhttps://docs.openclaw.ai/pl/cli/agent2026-05-01T07:59:53.786Zhttps://docs.openclaw.ai/pl/cli/agents2026-05-01T07:59:53.789Zhttps://docs.openclaw.ai/pl/cli/approvals2026-05-01T07:59:53.802Zhttps://docs.openclaw.ai/pl/cli/backup2026-05-01T07:59:53.777Zhttps://docs.openclaw.ai/pl/cli/browser2026-05-01T07:59:53.776Zhttps://docs.openclaw.ai/pl/cli/channels2026-05-01T10:08:36.672Zhttps://docs.openclaw.ai/pl/cli/clawbot2026-05-01T07:59:53.774Zhttps://docs.openclaw.ai/pl/cli/commitments2026-05-01T07:59:53.822Zhttps://docs.openclaw.ai/pl/cli/completion2026-05-01T07:59:53.830Zhttps://docs.openclaw.ai/pl/cli/config2026-05-01T07:59:53.828Zhttps://docs.openclaw.ai/pl/cli/configure2026-05-01T10:08:36.670Zhttps://docs.openclaw.ai/pl/cli/cron2026-05-01T07:59:53.831Zhttps://docs.openclaw.ai/pl/cli/daemon2026-05-01T07:59:53.829Zhttps://docs.openclaw.ai/pl/cli/dashboard2026-05-01T07:59:53.823Zhttps://docs.openclaw.ai/pl/cli/devices2026-05-01T07:59:53.823Zhttps://docs.openclaw.ai/pl/cli/directory2026-05-01T07:59:53.821Zhttps://docs.openclaw.ai/pl/cli/dns2026-05-01T07:59:53.860Zhttps://docs.openclaw.ai/pl/cli/docs2026-05-01T07:59:53.859Zhttps://docs.openclaw.ai/pl/cli/doctor2026-05-01T07:59:53.858Zhttps://docs.openclaw.ai/pl/cli/flows2026-05-01T07:59:53.859Zhttps://docs.openclaw.ai/pl/cli/gateway2026-05-01T10:08:36.813Zhttps://docs.openclaw.ai/pl/cli/health2026-05-01T07:59:53.858Zhttps://docs.openclaw.ai/pl/cli/hooks2026-05-01T07:59:53.851Zhttps://docs.openclaw.ai/pl/cli2026-05-01T07:59:53.851Zhttps://docs.openclaw.ai/pl/cli/infer2026-05-01T07:59:53.852Zhttps://docs.openclaw.ai/pl/cli/logs2026-05-01T07:59:53.850Zhttps://docs.openclaw.ai/pl/cli/mcp2026-05-01T07:59:53.902Zhttps://docs.openclaw.ai/pl/cli/memory2026-05-01T07:59:53.901Zhttps://docs.openclaw.ai/pl/cli/message2026-05-01T07:59:53.902Zhttps://docs.openclaw.ai/pl/cli/migrate2026-05-01T07:59:53.882Zhttps://docs.openclaw.ai/pl/cli/models2026-05-01T10:08:36.595Zhttps://docs.openclaw.ai/pl/cli/node2026-05-01T07:59:53.880Zhttps://docs.openclaw.ai/pl/cli/nodes2026-05-01T07:59:53.883Zhttps://docs.openclaw.ai/pl/cli/onboard2026-05-01T10:08:36.814Zhttps://docs.openclaw.ai/pl/cli/pairing2026-05-01T07:59:53.881Zhttps://docs.openclaw.ai/pl/cli/plugins2026-05-01T10:08:39.477Zhttps://docs.openclaw.ai/pl/cli/proxy2026-05-01T10:08:39.495Zhttps://docs.openclaw.ai/pl/cli/qr2026-05-01T07:59:53.937Zhttps://docs.openclaw.ai/pl/cli/reset2026-05-01T07:59:53.935Zhttps://docs.openclaw.ai/pl/cli/sandbox2026-05-01T07:59:53.927Zhttps://docs.openclaw.ai/pl/cli/secrets2026-05-01T07:59:53.930Zhttps://docs.openclaw.ai/pl/cli/security2026-05-01T07:59:53.926Zhttps://docs.openclaw.ai/pl/cli/sessions2026-05-01T07:59:53.927Zhttps://docs.openclaw.ai/pl/cli/setup2026-05-01T07:59:53.926Zhttps://docs.openclaw.ai/pl/cli/skills2026-05-01T07:59:53.925Zhttps://docs.openclaw.ai/pl/cli/status2026-05-01T07:59:53.925Zhttps://docs.openclaw.ai/pl/cli/system2026-05-01T07:59:53.969Zhttps://docs.openclaw.ai/pl/cli/tasks2026-05-01T07:59:53.969Zhttps://docs.openclaw.ai/pl/cli/tui2026-05-01T07:59:53.968Zhttps://docs.openclaw.ai/pl/cli/uninstall2026-05-01T07:59:53.962Zhttps://docs.openclaw.ai/pl/cli/update2026-05-01T10:08:39.499Zhttps://docs.openclaw.ai/pl/cli/voicecall2026-05-01T10:08:39.499Zhttps://docs.openclaw.ai/pl/cli/webhooks2026-05-01T07:59:53.958Zhttps://docs.openclaw.ai/pl/cli/wiki2026-05-01T07:59:53.959Zhttps://docs.openclaw.ai/pl/concepts/active-memory2026-05-01T07:59:53.958Zhttps://docs.openclaw.ai/pl/concepts/agent2026-05-01T07:59:54.013Zhttps://docs.openclaw.ai/pl/concepts/agent-loop2026-05-01T07:59:53.957Zhttps://docs.openclaw.ai/pl/concepts/agent-runtimes2026-05-01T07:59:54.015Zhttps://docs.openclaw.ai/pl/concepts/agent-workspace2026-05-01T07:59:54.013Zhttps://docs.openclaw.ai/pl/concepts/architecture2026-05-01T07:59:54.012Zhttps://docs.openclaw.ai/pl/concepts/channel-docking2026-05-01T07:59:54.011Zhttps://docs.openclaw.ai/pl/concepts/commitments2026-05-01T10:08:39.493Zhttps://docs.openclaw.ai/pl/concepts/compaction2026-05-01T07:59:54.012Zhttps://docs.openclaw.ai/pl/concepts/context2026-05-01T07:59:54.014Zhttps://docs.openclaw.ai/pl/concepts/context-engine2026-05-01T07:59:54.011Zhttps://docs.openclaw.ai/pl/concepts/delegate-architecture2026-05-01T07:59:54.010Zhttps://docs.openclaw.ai/pl/concepts/dreaming2026-05-01T07:59:54.104Zhttps://docs.openclaw.ai/pl/concepts/experimental-features2026-05-01T07:59:54.101Zhttps://docs.openclaw.ai/pl/concepts/features2026-05-01T07:59:54.080Zhttps://docs.openclaw.ai/pl/concepts/markdown-formatting2026-05-01T07:59:54.080Zhttps://docs.openclaw.ai/pl/concepts/memory2026-05-01T07:59:54.100Zhttps://docs.openclaw.ai/pl/concepts/memory-builtin2026-05-01T07:59:54.103Zhttps://docs.openclaw.ai/pl/concepts/memory-honcho2026-05-01T07:59:54.081Zhttps://docs.openclaw.ai/pl/concepts/memory-qmd2026-05-01T07:59:54.102Zhttps://docs.openclaw.ai/pl/concepts/memory-search2026-05-01T07:59:54.103Zhttps://docs.openclaw.ai/pl/concepts/messages2026-05-01T07:59:54.081Zhttps://docs.openclaw.ai/pl/concepts/model-failover2026-05-01T07:59:54.144Zhttps://docs.openclaw.ai/pl/concepts/model-providers2026-05-01T07:59:54.142Zhttps://docs.openclaw.ai/pl/concepts/models2026-05-01T07:59:54.143Zhttps://docs.openclaw.ai/pl/concepts/multi-agent2026-05-01T07:59:54.134Zhttps://docs.openclaw.ai/pl/concepts/oauth2026-05-01T07:59:54.143Zhttps://docs.openclaw.ai/pl/concepts/openclaw-sdk2026-05-01T10:08:39.494Zhttps://docs.openclaw.ai/pl/concepts/presence2026-05-01T07:59:54.142Zhttps://docs.openclaw.ai/pl/concepts/qa-e2e-automation2026-05-01T07:59:54.135Zhttps://docs.openclaw.ai/pl/concepts/qa-matrix2026-05-01T07:59:54.134Zhttps://docs.openclaw.ai/pl/concepts/queue2026-05-01T07:59:54.173Zhttps://docs.openclaw.ai/pl/concepts/queue-steering2026-05-01T07:59:54.133Zhttps://docs.openclaw.ai/pl/concepts/retry2026-05-01T07:59:54.171Zhttps://docs.openclaw.ai/pl/concepts/session2026-05-01T07:59:54.164Zhttps://docs.openclaw.ai/pl/concepts/session-pruning2026-05-01T07:59:54.172Zhttps://docs.openclaw.ai/pl/concepts/session-tool2026-05-01T07:59:54.172Zhttps://docs.openclaw.ai/pl/concepts/soul2026-05-01T07:59:54.168Zhttps://docs.openclaw.ai/pl/concepts/streaming2026-05-01T07:59:54.170Zhttps://docs.openclaw.ai/pl/concepts/system-prompt2026-05-01T07:59:54.164Zhttps://docs.openclaw.ai/pl/concepts/timezone2026-05-01T07:59:54.163Zhttps://docs.openclaw.ai/pl/concepts/typebox2026-05-01T07:59:54.163Zhttps://docs.openclaw.ai/pl/concepts/typing-indicators2026-05-01T07:59:54.202Zhttps://docs.openclaw.ai/pl/concepts/usage-tracking2026-05-01T07:59:54.201Zhttps://docs.openclaw.ai/pl/date-time2026-05-01T07:59:54.200Zhttps://docs.openclaw.ai/pl/debug/node-issue2026-05-01T07:59:54.200Zhttps://docs.openclaw.ai/pl/diagnostics/flags2026-05-01T07:59:54.192Zhttps://docs.openclaw.ai/pl/gateway/authentication2026-05-01T07:59:54.197Zhttps://docs.openclaw.ai/pl/gateway/background-process2026-05-01T07:59:54.193Zhttps://docs.openclaw.ai/pl/gateway/bonjour2026-05-01T07:59:54.201Zhttps://docs.openclaw.ai/pl/gateway/bridge-protocol2026-05-01T07:59:54.193Zhttps://docs.openclaw.ai/pl/gateway/cli-backends2026-05-01T07:59:54.192Zhttps://docs.openclaw.ai/pl/gateway/config-agents2026-05-01T10:08:39.509Zhttps://docs.openclaw.ai/pl/gateway/config-channels2026-05-01T10:08:39.498Zhttps://docs.openclaw.ai/pl/gateway/config-tools2026-05-01T10:08:39.496Zhttps://docs.openclaw.ai/pl/gateway/configuration2026-05-01T07:59:54.245Zhttps://docs.openclaw.ai/pl/gateway/configuration-examples2026-05-01T07:59:54.225Zhttps://docs.openclaw.ai/pl/gateway/configuration-reference2026-05-01T07:59:54.243Zhttps://docs.openclaw.ai/pl/gateway/diagnostics2026-05-01T07:59:54.226Zhttps://docs.openclaw.ai/pl/gateway/discovery2026-05-01T07:59:54.224Zhttps://docs.openclaw.ai/pl/gateway/doctor2026-05-01T10:08:39.488Zhttps://docs.openclaw.ai/pl/gateway/gateway-lock2026-05-01T07:59:54.227Zhttps://docs.openclaw.ai/pl/gateway/health2026-05-01T07:59:54.278Zhttps://docs.openclaw.ai/pl/gateway/heartbeat2026-05-01T07:59:54.274Zhttps://docs.openclaw.ai/pl/gateway2026-05-01T07:59:54.277Zhttps://docs.openclaw.ai/pl/gateway/local-models2026-05-01T07:59:54.276Zhttps://docs.openclaw.ai/pl/gateway/logging2026-05-01T10:08:42.524Zhttps://docs.openclaw.ai/pl/gateway/multiple-gateways2026-05-01T07:59:54.269Zhttps://docs.openclaw.ai/pl/gateway/network-model2026-05-01T07:59:54.269Zhttps://docs.openclaw.ai/pl/gateway/openai-http-api2026-05-01T07:59:54.270Zhttps://docs.openclaw.ai/pl/gateway/openresponses-http-api2026-05-01T07:59:54.270Zhttps://docs.openclaw.ai/pl/gateway/openshell2026-05-01T07:59:54.268Zhttps://docs.openclaw.ai/pl/gateway/opentelemetry2026-05-01T07:59:54.309Zhttps://docs.openclaw.ai/pl/gateway/pairing2026-05-01T07:59:54.308Zhttps://docs.openclaw.ai/pl/gateway/prometheus2026-05-01T07:59:54.307Zhttps://docs.openclaw.ai/pl/gateway/protocol2026-05-01T10:08:42.549Zhttps://docs.openclaw.ai/pl/gateway/remote2026-05-01T07:59:54.300Zhttps://docs.openclaw.ai/pl/gateway/remote-gateway-readme2026-05-01T07:59:54.301Zhttps://docs.openclaw.ai/pl/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:54.301Zhttps://docs.openclaw.ai/pl/gateway/sandboxing2026-05-01T07:59:54.299Zhttps://docs.openclaw.ai/pl/gateway/secrets2026-05-01T07:59:54.299Zhttps://docs.openclaw.ai/pl/gateway/secrets-plan-contract2026-05-01T07:59:54.300Zhttps://docs.openclaw.ai/pl/gateway/security/audit-checks2026-05-01T07:59:54.356Zhttps://docs.openclaw.ai/pl/gateway/security2026-04-30T10:29:49.291Zhttps://docs.openclaw.ai/pl/gateway/tailscale2026-05-01T07:59:54.338Zhttps://docs.openclaw.ai/pl/gateway/tools-invoke-http-api2026-05-01T07:59:54.338Zhttps://docs.openclaw.ai/pl/gateway/troubleshooting2026-05-01T10:08:42.538Zhttps://docs.openclaw.ai/pl/gateway/trusted-proxy-auth2026-05-01T07:59:54.339Zhttps://docs.openclaw.ai/pl/help/debugging2026-05-01T07:59:54.352Zhttps://docs.openclaw.ai/pl/help/environment2026-05-01T07:59:54.328Zhttps://docs.openclaw.ai/pl/help/faq2026-05-01T07:59:54.337Zhttps://docs.openclaw.ai/pl/help/faq-first-run2026-05-01T07:59:54.353Zhttps://docs.openclaw.ai/pl/help/faq-models2026-05-01T07:59:54.328Zhttps://docs.openclaw.ai/pl/help/gpt55-codex-agentic-parity2026-05-01T07:59:54.387Zhttps://docs.openclaw.ai/pl/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:54.395Zhttps://docs.openclaw.ai/pl/help2026-05-01T07:59:54.391Zhttps://docs.openclaw.ai/pl/help/scripts2026-05-01T07:59:54.388Zhttps://docs.openclaw.ai/pl/help/testing2026-05-01T10:08:42.528Zhttps://docs.openclaw.ai/pl/help/testing-live2026-05-01T07:59:54.394Zhttps://docs.openclaw.ai/pl/help/troubleshooting2026-05-01T07:59:54.387Zhttps://docs.openclaw.ai/pl2026-05-01T07:59:54.388Zhttps://docs.openclaw.ai/pl/install/ansible2026-05-01T07:59:54.386Zhttps://docs.openclaw.ai/pl/install/azure2026-05-01T07:59:54.386Zhttps://docs.openclaw.ai/pl/install/bun2026-05-01T07:59:54.424Zhttps://docs.openclaw.ai/pl/install/clawdock2026-05-01T07:59:54.423Zhttps://docs.openclaw.ai/pl/install/development-channels2026-05-01T07:59:54.423Zhttps://docs.openclaw.ai/pl/install/digitalocean2026-05-01T07:59:54.419Zhttps://docs.openclaw.ai/pl/install/docker2026-05-01T07:59:54.422Zhttps://docs.openclaw.ai/pl/install/docker-vm-runtime2026-05-01T07:59:54.415Zhttps://docs.openclaw.ai/pl/install/exe-dev2026-05-01T07:59:54.422Zhttps://docs.openclaw.ai/pl/install/fly2026-05-01T07:59:54.415Zhttps://docs.openclaw.ai/pl/install/gcp2026-05-01T07:59:54.414Zhttps://docs.openclaw.ai/pl/install/hetzner2026-05-01T07:59:54.414Zhttps://docs.openclaw.ai/pl/install/hostinger2026-05-01T07:59:54.464Zhttps://docs.openclaw.ai/pl/install2026-05-01T07:59:54.463Zhttps://docs.openclaw.ai/pl/install/installer2026-05-01T07:59:54.464Zhttps://docs.openclaw.ai/pl/install/kubernetes2026-05-01T07:59:54.446Zhttps://docs.openclaw.ai/pl/install/macos-vm2026-05-01T07:59:54.446Zhttps://docs.openclaw.ai/pl/install/migrating2026-05-01T07:59:54.445Zhttps://docs.openclaw.ai/pl/install/migrating-claude2026-05-01T07:59:54.450Zhttps://docs.openclaw.ai/pl/install/migrating-hermes2026-05-01T07:59:54.445Zhttps://docs.openclaw.ai/pl/install/nix2026-05-01T07:59:54.444Zhttps://docs.openclaw.ai/pl/install/node2026-05-01T07:59:54.450Zhttps://docs.openclaw.ai/pl/install/northflank2026-05-01T07:59:54.487Zhttps://docs.openclaw.ai/pl/install/oracle2026-05-01T07:59:54.493Zhttps://docs.openclaw.ai/pl/install/podman2026-05-01T07:59:54.485Zhttps://docs.openclaw.ai/pl/install/railway2026-05-01T07:59:54.490Zhttps://docs.openclaw.ai/pl/install/raspberry-pi2026-05-01T07:59:54.493Zhttps://docs.openclaw.ai/pl/install/render2026-05-01T07:59:54.486Zhttps://docs.openclaw.ai/pl/install/uninstall2026-05-01T07:59:54.486Zhttps://docs.openclaw.ai/pl/install/updating2026-05-01T10:08:42.522Zhttps://docs.openclaw.ai/pl/logging2026-05-01T10:08:42.518Zhttps://docs.openclaw.ai/pl/network2026-05-01T07:59:54.485Zhttps://docs.openclaw.ai/pl/nodes/audio2026-05-01T07:59:54.512Zhttps://docs.openclaw.ai/pl/nodes/camera2026-05-01T07:59:54.521Zhttps://docs.openclaw.ai/pl/nodes/images2026-05-01T07:59:54.522Zhttps://docs.openclaw.ai/pl/nodes2026-05-01T07:59:54.518Zhttps://docs.openclaw.ai/pl/nodes/location-command2026-05-01T07:59:54.521Zhttps://docs.openclaw.ai/pl/nodes/media-understanding2026-05-01T07:59:54.520Zhttps://docs.openclaw.ai/pl/nodes/talk2026-05-01T07:59:54.519Zhttps://docs.openclaw.ai/pl/nodes/troubleshooting2026-05-01T07:59:54.513Zhttps://docs.openclaw.ai/pl/nodes/voicewake2026-05-01T07:59:54.512Zhttps://docs.openclaw.ai/pl/pi2026-05-01T07:59:54.550Zhttps://docs.openclaw.ai/pl/pi-dev2026-05-01T07:59:54.562Zhttps://docs.openclaw.ai/pl/platforms/android2026-05-01T07:59:54.542Zhttps://docs.openclaw.ai/pl/platforms2026-05-01T07:59:54.542Zhttps://docs.openclaw.ai/pl/platforms/ios2026-05-01T07:59:54.543Zhttps://docs.openclaw.ai/pl/platforms/linux2026-05-01T07:59:54.541Zhttps://docs.openclaw.ai/pl/platforms/mac/bundled-gateway2026-05-01T07:59:54.541Zhttps://docs.openclaw.ai/pl/platforms/mac/canvas2026-05-01T07:59:54.592Zhttps://docs.openclaw.ai/pl/platforms/mac/child-process2026-05-01T07:59:54.591Zhttps://docs.openclaw.ai/pl/platforms/mac/dev-setup2026-05-01T07:59:54.582Zhttps://docs.openclaw.ai/pl/platforms/mac/health2026-05-01T07:59:54.591Zhttps://docs.openclaw.ai/pl/platforms/mac/icon2026-05-01T07:59:54.584Zhttps://docs.openclaw.ai/pl/platforms/mac/logging2026-05-01T07:59:54.583Zhttps://docs.openclaw.ai/pl/platforms/mac/menu-bar2026-05-01T10:08:42.519Zhttps://docs.openclaw.ai/pl/platforms/mac/peekaboo2026-05-01T07:59:54.584Zhttps://docs.openclaw.ai/pl/platforms/mac/permissions2026-05-01T07:59:54.583Zhttps://docs.openclaw.ai/pl/platforms/mac/remote2026-05-01T07:59:54.587Zhttps://docs.openclaw.ai/pl/platforms/mac/signing2026-05-01T07:59:54.621Zhttps://docs.openclaw.ai/pl/platforms/mac/skills2026-05-01T07:59:54.620Zhttps://docs.openclaw.ai/pl/platforms/mac/voice-overlay2026-05-01T07:59:54.620Zhttps://docs.openclaw.ai/pl/platforms/mac/voicewake2026-05-01T07:59:54.616Zhttps://docs.openclaw.ai/pl/platforms/mac/webchat2026-05-01T07:59:54.619Zhttps://docs.openclaw.ai/pl/platforms/mac/xpc2026-05-01T07:59:54.619Zhttps://docs.openclaw.ai/pl/platforms/macos2026-05-01T07:59:54.612Zhttps://docs.openclaw.ai/pl/platforms/windows2026-05-01T07:59:54.611Zhttps://docs.openclaw.ai/pl/plugins/architecture2026-05-01T07:59:54.651Zhttps://docs.openclaw.ai/pl/plugins/architecture-internals2026-05-01T07:59:54.652Zhttps://docs.openclaw.ai/pl/plugins/building-plugins2026-05-01T07:59:54.643Zhttps://docs.openclaw.ai/pl/plugins/bundles2026-05-01T07:59:54.649Zhttps://docs.openclaw.ai/pl/plugins/codex-computer-use2026-05-01T07:59:54.643Zhttps://docs.openclaw.ai/pl/plugins/codex-harness2026-05-01T10:08:42.529Zhttps://docs.openclaw.ai/pl/plugins/community2026-05-01T07:59:54.642Zhttps://docs.openclaw.ai/pl/plugins/compatibility2026-05-01T07:59:54.641Zhttps://docs.openclaw.ai/pl/plugins/dependency-resolution2026-05-01T10:08:42.513Zhttps://docs.openclaw.ai/pl/plugins/google-meet2026-05-01T10:08:42.537Zhttps://docs.openclaw.ai/pl/plugins/hooks2026-05-01T07:59:54.693Zhttps://docs.openclaw.ai/pl/plugins/manifest2026-05-01T07:59:54.692Zhttps://docs.openclaw.ai/pl/plugins/memory-lancedb2026-05-01T07:59:54.695Zhttps://docs.openclaw.ai/pl/plugins/memory-wiki2026-05-01T07:59:54.695Zhttps://docs.openclaw.ai/pl/plugins/message-presentation2026-05-01T07:59:54.694Zhttps://docs.openclaw.ai/pl/plugins/sdk-agent-harness2026-05-01T07:59:54.683Zhttps://docs.openclaw.ai/pl/plugins/sdk-channel-plugins2026-05-01T07:59:54.684Zhttps://docs.openclaw.ai/pl/plugins/sdk-channel-turn2026-05-01T07:59:54.682Zhttps://docs.openclaw.ai/pl/plugins/sdk-entrypoints2026-05-01T07:59:54.682Zhttps://docs.openclaw.ai/pl/plugins/sdk-migration2026-05-01T07:59:54.729Zhttps://docs.openclaw.ai/pl/plugins/sdk-overview2026-05-01T07:59:54.729Zhttps://docs.openclaw.ai/pl/plugins/sdk-provider-plugins2026-05-01T10:08:45.209Zhttps://docs.openclaw.ai/pl/plugins/sdk-runtime2026-05-01T07:59:54.728Zhttps://docs.openclaw.ai/pl/plugins/sdk-setup2026-05-01T07:59:54.726Zhttps://docs.openclaw.ai/pl/plugins/sdk-subpaths2026-05-01T07:59:54.726Zhttps://docs.openclaw.ai/pl/plugins/sdk-testing2026-05-01T07:59:54.717Zhttps://docs.openclaw.ai/pl/plugins/skill-workshop2026-05-01T07:59:54.728Zhttps://docs.openclaw.ai/pl/plugins/voice-call2026-05-01T10:08:45.189Zhttps://docs.openclaw.ai/pl/plugins/webhooks2026-05-01T07:59:54.715Zhttps://docs.openclaw.ai/pl/plugins/zalouser2026-05-01T07:59:54.770Zhttps://docs.openclaw.ai/pl/prose2026-05-01T07:59:54.770Zhttps://docs.openclaw.ai/pl/providers/alibaba2026-05-01T07:59:54.753Zhttps://docs.openclaw.ai/pl/providers/anthropic2026-05-01T07:59:54.751Zhttps://docs.openclaw.ai/pl/providers/arcee2026-05-01T07:59:54.769Zhttps://docs.openclaw.ai/pl/providers/azure-speech2026-05-01T07:59:54.756Zhttps://docs.openclaw.ai/pl/providers/bedrock2026-05-01T07:59:54.752Zhttps://docs.openclaw.ai/pl/providers/bedrock-mantle2026-05-01T07:59:54.752Zhttps://docs.openclaw.ai/pl/providers/chutes2026-05-01T07:59:54.757Zhttps://docs.openclaw.ai/pl/providers/claude-max-api-proxy2026-05-01T07:59:54.814Zhttps://docs.openclaw.ai/pl/providers/cloudflare-ai-gateway2026-05-01T07:59:54.813Zhttps://docs.openclaw.ai/pl/providers/comfy2026-05-01T07:59:54.813Zhttps://docs.openclaw.ai/pl/providers/deepgram2026-05-01T07:59:54.793Zhttps://docs.openclaw.ai/pl/providers/deepinfra2026-05-01T07:59:54.792Zhttps://docs.openclaw.ai/pl/providers/deepseek2026-05-01T07:59:54.791Zhttps://docs.openclaw.ai/pl/providers/elevenlabs2026-05-01T07:59:54.792Zhttps://docs.openclaw.ai/pl/providers/fal2026-05-01T07:59:54.796Zhttps://docs.openclaw.ai/pl/providers/fireworks2026-05-01T07:59:54.811Zhttps://docs.openclaw.ai/pl/providers/github-copilot2026-05-01T07:59:54.790Zhttps://docs.openclaw.ai/pl/providers/glm2026-05-01T07:59:54.851Zhttps://docs.openclaw.ai/pl/providers/google2026-05-01T07:59:54.850Zhttps://docs.openclaw.ai/pl/providers/gradium2026-05-01T07:59:54.850Zhttps://docs.openclaw.ai/pl/providers/groq2026-05-01T07:59:54.843Zhttps://docs.openclaw.ai/pl/providers/huggingface2026-05-01T07:59:54.844Zhttps://docs.openclaw.ai/pl/providers2026-05-01T07:59:54.848Zhttps://docs.openclaw.ai/pl/providers/inferrs2026-05-01T07:59:54.847Zhttps://docs.openclaw.ai/pl/providers/inworld2026-05-01T07:59:54.844Zhttps://docs.openclaw.ai/pl/providers/kilocode2026-05-01T07:59:54.842Zhttps://docs.openclaw.ai/pl/providers/litellm2026-05-01T07:59:54.843Zhttps://docs.openclaw.ai/pl/providers/lmstudio2026-05-01T07:59:54.881Zhttps://docs.openclaw.ai/pl/providers/minimax2026-05-01T07:59:54.880Zhttps://docs.openclaw.ai/pl/providers/mistral2026-05-01T07:59:54.880Zhttps://docs.openclaw.ai/pl/providers/models2026-05-01T07:59:54.879Zhttps://docs.openclaw.ai/pl/providers/moonshot2026-05-01T07:59:54.872Zhttps://docs.openclaw.ai/pl/providers/nvidia2026-05-01T07:59:54.873Zhttps://docs.openclaw.ai/pl/providers/ollama2026-05-01T07:59:54.872Zhttps://docs.openclaw.ai/pl/providers/openai2026-05-01T07:59:54.871Zhttps://docs.openclaw.ai/pl/providers/opencode2026-05-01T07:59:54.877Zhttps://docs.openclaw.ai/pl/providers/opencode-go2026-05-01T07:59:54.878Zhttps://docs.openclaw.ai/pl/providers/openrouter2026-05-01T07:59:54.910Zhttps://docs.openclaw.ai/pl/providers/perplexity-provider2026-05-01T07:59:54.909Zhttps://docs.openclaw.ai/pl/providers/qianfan2026-05-01T07:59:54.909Zhttps://docs.openclaw.ai/pl/providers/qwen2026-05-01T07:59:54.901Zhttps://docs.openclaw.ai/pl/providers/runway2026-05-01T07:59:54.905Zhttps://docs.openclaw.ai/pl/providers/sglang2026-05-01T07:59:54.908Zhttps://docs.openclaw.ai/pl/providers/stepfun2026-05-01T07:59:54.907Zhttps://docs.openclaw.ai/pl/providers/synthetic2026-05-01T07:59:54.901Zhttps://docs.openclaw.ai/pl/providers/tencent2026-05-01T07:59:54.900Zhttps://docs.openclaw.ai/pl/providers/together2026-05-01T07:59:54.948Zhttps://docs.openclaw.ai/pl/providers/venice2026-05-01T07:59:54.947Zhttps://docs.openclaw.ai/pl/providers/vercel-ai-gateway2026-05-01T07:59:54.947Zhttps://docs.openclaw.ai/pl/providers/vllm2026-05-01T07:59:54.941Zhttps://docs.openclaw.ai/pl/providers/volcengine2026-05-01T07:59:54.941Zhttps://docs.openclaw.ai/pl/providers/vydra2026-05-01T07:59:54.939Zhttps://docs.openclaw.ai/pl/providers/xai2026-05-01T07:59:54.945Zhttps://docs.openclaw.ai/pl/providers/xiaomi2026-05-01T07:59:54.945Zhttps://docs.openclaw.ai/pl/providers/zai2026-05-01T07:59:54.940Zhttps://docs.openclaw.ai/pl/reference/AGENTS.default2026-05-01T07:59:54.940Zhttps://docs.openclaw.ai/pl/reference/RELEASING2026-05-01T10:08:45.186Zhttps://docs.openclaw.ai/pl/reference/api-usage-costs2026-05-01T07:59:54.977Zhttps://docs.openclaw.ai/pl/reference/credits2026-05-01T07:59:54.970Zhttps://docs.openclaw.ai/pl/reference/device-models2026-05-01T07:59:54.969Zhttps://docs.openclaw.ai/pl/reference/full-release-validation2026-05-01T10:08:45.180Zhttps://docs.openclaw.ai/pl/reference/memory-config2026-05-01T07:59:54.970Zhttps://docs.openclaw.ai/pl/reference/openclaw-sdk-api-design2026-05-01T07:59:54.969Zhttps://docs.openclaw.ai/pl/reference/prompt-caching2026-05-01T07:59:54.968Zhttps://docs.openclaw.ai/pl/reference/rich-output-protocol2026-05-01T07:59:54.973Zhttps://docs.openclaw.ai/pl/reference/rpc2026-05-01T07:59:54.968Zhttps://docs.openclaw.ai/pl/reference/secretref-credential-surface2026-05-01T10:08:45.181Zhttps://docs.openclaw.ai/pl/reference/session-management-compaction2026-05-01T07:59:55.006Zhttps://docs.openclaw.ai/pl/reference/templates/AGENTS2026-05-01T07:59:55.003Zhttps://docs.openclaw.ai/pl/reference/templates/BOOT2026-05-01T07:59:54.998Zhttps://docs.openclaw.ai/pl/reference/templates/BOOTSTRAP2026-05-01T07:59:55.000Zhttps://docs.openclaw.ai/pl/reference/templates/HEARTBEAT2026-05-01T07:59:54.999Zhttps://docs.openclaw.ai/pl/reference/templates/IDENTITY2026-05-01T07:59:54.998Zhttps://docs.openclaw.ai/pl/reference/templates/SOUL2026-05-01T07:59:55.046Zhttps://docs.openclaw.ai/pl/reference/templates/TOOLS2026-05-01T07:59:55.038Zhttps://docs.openclaw.ai/pl/reference/templates/USER2026-05-01T07:59:55.039Zhttps://docs.openclaw.ai/pl/reference/test2026-05-01T10:08:45.187Zhttps://docs.openclaw.ai/pl/reference/token-use2026-05-01T07:59:55.040Zhttps://docs.openclaw.ai/pl/reference/transcript-hygiene2026-05-01T07:59:55.043Zhttps://docs.openclaw.ai/pl/reference/wizard2026-05-01T07:59:55.038Zhttps://docs.openclaw.ai/pl/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:55.077Zhttps://docs.openclaw.ai/pl/security/THREAT-MODEL-ATLAS2026-05-01T07:59:55.075Zhttps://docs.openclaw.ai/pl/security/formal-verification2026-05-01T07:59:55.076Zhttps://docs.openclaw.ai/pl/security/network-proxy2026-05-01T10:08:45.188Zhttps://docs.openclaw.ai/pl/start/bootstrapping2026-05-01T07:59:55.069Zhttps://docs.openclaw.ai/pl/start/docs-directory2026-05-01T07:59:55.068Zhttps://docs.openclaw.ai/pl/start/getting-started2026-05-01T07:59:55.068Zhttps://docs.openclaw.ai/pl/start/hubs2026-05-01T07:59:55.067Zhttps://docs.openclaw.ai/pl/start/lore2026-05-01T07:59:55.067Zhttps://docs.openclaw.ai/pl/start/onboarding2026-05-01T07:59:55.106Zhttps://docs.openclaw.ai/pl/start/onboarding-overview2026-05-01T07:59:55.072Zhttps://docs.openclaw.ai/pl/start/openclaw2026-05-01T07:59:55.104Zhttps://docs.openclaw.ai/pl/start/setup2026-05-01T07:59:55.102Zhttps://docs.openclaw.ai/pl/start/showcase2026-04-24T17:33:20.549Zhttps://docs.openclaw.ai/pl/start/wizard2026-05-01T07:59:55.095Zhttps://docs.openclaw.ai/pl/start/wizard-cli-automation2026-05-01T07:59:55.103Zhttps://docs.openclaw.ai/pl/start/wizard-cli-reference2026-05-01T07:59:55.105Zhttps://docs.openclaw.ai/pl/tools/acp-agents2026-05-01T10:08:45.206Zhttps://docs.openclaw.ai/pl/tools/acp-agents-setup2026-05-01T07:59:55.095Zhttps://docs.openclaw.ai/pl/tools/agent-send2026-05-01T07:59:55.144Zhttps://docs.openclaw.ai/pl/tools/apply-patch2026-05-01T07:59:55.138Zhttps://docs.openclaw.ai/pl/tools/brave-search2026-05-01T07:59:55.145Zhttps://docs.openclaw.ai/pl/tools/browser2026-05-01T07:59:55.138Zhttps://docs.openclaw.ai/pl/tools/browser-control2026-05-01T07:59:55.143Zhttps://docs.openclaw.ai/pl/tools/browser-linux-troubleshooting2026-05-01T07:59:55.136Zhttps://docs.openclaw.ai/pl/tools/browser-login2026-05-01T07:59:55.142Zhttps://docs.openclaw.ai/pl/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:55.137Zhttps://docs.openclaw.ai/pl/tools/btw2026-05-01T07:59:55.137Zhttps://docs.openclaw.ai/pl/tools/clawhub2026-05-01T07:59:55.173Zhttps://docs.openclaw.ai/pl/tools/code-execution2026-05-01T07:59:55.174Zhttps://docs.openclaw.ai/pl/tools/creating-skills2026-05-01T07:59:55.172Zhttps://docs.openclaw.ai/pl/tools/diffs2026-05-01T07:59:55.173Zhttps://docs.openclaw.ai/pl/tools/duckduckgo-search2026-05-01T07:59:55.165Zhttps://docs.openclaw.ai/pl/tools/elevated2026-05-01T07:59:55.166Zhttps://docs.openclaw.ai/pl/tools/exa-search2026-05-01T07:59:55.165Zhttps://docs.openclaw.ai/pl/tools/exec2026-05-01T07:59:55.204Zhttps://docs.openclaw.ai/pl/tools/exec-approvals2026-05-01T07:59:55.171Zhttps://docs.openclaw.ai/pl/tools/exec-approvals-advanced2026-05-01T07:59:55.164Zhttps://docs.openclaw.ai/pl/tools/firecrawl2026-05-01T07:59:55.202Zhttps://docs.openclaw.ai/pl/tools/gemini-search2026-05-01T07:59:55.202Zhttps://docs.openclaw.ai/pl/tools/grok-search2026-05-01T07:59:55.203Zhttps://docs.openclaw.ai/pl/tools/image-generation2026-05-01T07:59:55.201Zhttps://docs.openclaw.ai/pl/tools2026-05-01T07:59:55.200Zhttps://docs.openclaw.ai/pl/tools/kimi-search2026-05-01T07:59:55.195Zhttps://docs.openclaw.ai/pl/tools/llm-task2026-05-01T07:59:55.194Zhttps://docs.openclaw.ai/pl/tools/lobster2026-05-01T07:59:55.194Zhttps://docs.openclaw.ai/pl/tools/loop-detection2026-05-01T07:59:55.193Zhttps://docs.openclaw.ai/pl/tools/media-overview2026-05-01T07:59:55.242Zhttps://docs.openclaw.ai/pl/tools/minimax-search2026-05-01T07:59:55.241Zhttps://docs.openclaw.ai/pl/tools/multi-agent-sandbox-tools2026-05-01T07:59:55.239Zhttps://docs.openclaw.ai/pl/tools/music-generation2026-05-01T07:59:55.242Zhttps://docs.openclaw.ai/pl/tools/ollama-search2026-05-01T07:59:55.239Zhttps://docs.openclaw.ai/pl/tools/pdf2026-05-01T07:59:55.233Zhttps://docs.openclaw.ai/pl/tools/perplexity-search2026-05-01T07:59:55.234Zhttps://docs.openclaw.ai/pl/tools/plugin2026-05-01T10:08:45.184Zhttps://docs.openclaw.ai/pl/tools/reactions2026-05-01T07:59:55.235Zhttps://docs.openclaw.ai/pl/tools/searxng-search2026-05-01T07:59:55.234Zhttps://docs.openclaw.ai/pl/tools/skills2026-05-01T07:59:55.273Zhttps://docs.openclaw.ai/pl/tools/skills-config2026-05-01T07:59:55.274Zhttps://docs.openclaw.ai/pl/tools/slash-commands2026-05-01T07:59:55.273Zhttps://docs.openclaw.ai/pl/tools/subagents2026-05-01T07:59:55.266Zhttps://docs.openclaw.ai/pl/tools/tavily2026-05-01T07:59:55.265Zhttps://docs.openclaw.ai/pl/tools/thinking2026-05-01T07:59:55.265Zhttps://docs.openclaw.ai/pl/tools/tokenjuice2026-05-01T07:59:55.267Zhttps://docs.openclaw.ai/pl/tools/trajectory2026-05-01T07:59:55.264Zhttps://docs.openclaw.ai/pl/tools/tts2026-05-01T07:59:55.264Zhttps://docs.openclaw.ai/pl/tools/video-generation2026-05-01T07:59:55.270Zhttps://docs.openclaw.ai/pl/tools/web2026-05-01T07:59:55.304Zhttps://docs.openclaw.ai/pl/tools/web-fetch2026-05-01T07:59:55.305Zhttps://docs.openclaw.ai/pl/vps2026-05-01T07:59:55.295Zhttps://docs.openclaw.ai/pl/web/control-ui2026-05-01T07:59:55.295Zhttps://docs.openclaw.ai/pl/web/dashboard2026-05-01T07:59:55.297Zhttps://docs.openclaw.ai/pl/web2026-05-01T07:59:55.303Zhttps://docs.openclaw.ai/pl/web/tui2026-05-01T07:59:55.296Zhttps://docs.openclaw.ai/pl/web/webchat2026-05-01T07:59:55.296Zhttps://docs.openclaw.ai/platforms/android2026-05-01T07:59:55.342Zhttps://docs.openclaw.ai/platforms2026-05-01T07:59:55.340Zhttps://docs.openclaw.ai/platforms/ios2026-05-01T07:59:55.334Zhttps://docs.openclaw.ai/platforms/linux2026-05-01T07:59:55.336Zhttps://docs.openclaw.ai/platforms/mac/bundled-gateway2026-05-01T07:59:55.335Zhttps://docs.openclaw.ai/platforms/mac/canvas2026-05-01T07:59:55.336Zhttps://docs.openclaw.ai/platforms/mac/child-process2026-05-01T07:59:55.340Zhttps://docs.openclaw.ai/platforms/mac/dev-setup2026-05-01T07:59:55.335Zhttps://docs.openclaw.ai/platforms/mac/health2026-05-01T07:59:55.381Zhttps://docs.openclaw.ai/platforms/mac/icon2026-05-01T07:59:55.377Zhttps://docs.openclaw.ai/platforms/mac/logging2026-05-01T07:59:55.380Zhttps://docs.openclaw.ai/platforms/mac/menu-bar2026-05-01T07:59:55.373Zhttps://docs.openclaw.ai/platforms/mac/peekaboo2026-05-01T07:59:55.371Zhttps://docs.openclaw.ai/platforms/mac/permissions2026-05-01T07:59:55.373Zhttps://docs.openclaw.ai/platforms/mac/remote2026-05-01T07:59:55.372Zhttps://docs.openclaw.ai/platforms/mac/signing2026-05-01T07:59:55.371Zhttps://docs.openclaw.ai/platforms/mac/skills2026-05-01T07:59:55.378Zhttps://docs.openclaw.ai/platforms/mac/voice-overlay2026-05-01T07:59:55.370Zhttps://docs.openclaw.ai/platforms/mac/voicewake2026-05-01T07:59:55.418Zhttps://docs.openclaw.ai/platforms/mac/webchat2026-05-01T07:59:55.419Zhttps://docs.openclaw.ai/platforms/mac/xpc2026-05-01T07:59:55.414Zhttps://docs.openclaw.ai/platforms/macos2026-05-01T07:59:55.417Zhttps://docs.openclaw.ai/platforms/windows2026-05-01T07:59:55.406Zhttps://docs.openclaw.ai/plugins/architecture2026-05-01T07:59:55.405Zhttps://docs.openclaw.ai/plugins/architecture-internals2026-05-01T07:59:55.407Zhttps://docs.openclaw.ai/plugins/building-plugins2026-05-01T10:02:50.283Zhttps://docs.openclaw.ai/plugins/bundles2026-05-01T07:59:55.456Zhttps://docs.openclaw.ai/plugins/codex-computer-use2026-05-01T07:59:55.455Zhttps://docs.openclaw.ai/plugins/codex-harness2026-05-01T11:41:02.127Zhttps://docs.openclaw.ai/plugins/community2026-05-01T07:59:55.455Zhttps://docs.openclaw.ai/plugins/compatibility2026-05-01T07:59:55.453Zhttps://docs.openclaw.ai/plugins/dependency-resolution2026-05-01T07:59:55.453Zhttps://docs.openclaw.ai/plugins/google-meet2026-05-01T13:33:16.406Zhttps://docs.openclaw.ai/plugins/hooks2026-05-01T07:59:55.459Zhttps://docs.openclaw.ai/plugins/manifest2026-05-01T07:59:55.513Zhttps://docs.openclaw.ai/plugins/memory-lancedb2026-05-01T07:59:55.511Zhttps://docs.openclaw.ai/plugins/memory-wiki2026-05-01T07:59:55.510Zhttps://docs.openclaw.ai/plugins/message-presentation2026-05-01T07:59:55.513Zhttps://docs.openclaw.ai/plugins/sdk-agent-harness2026-05-01T07:59:55.512Zhttps://docs.openclaw.ai/plugins/sdk-channel-plugins2026-05-01T07:59:55.491Zhttps://docs.openclaw.ai/plugins/sdk-channel-turn2026-05-01T07:59:55.490Zhttps://docs.openclaw.ai/plugins/sdk-entrypoints2026-05-01T07:59:55.486Zhttps://docs.openclaw.ai/plugins/sdk-migration2026-05-01T07:59:55.510Zhttps://docs.openclaw.ai/plugins/sdk-overview2026-05-01T07:59:55.486Zhttps://docs.openclaw.ai/plugins/sdk-provider-plugins2026-05-01T08:05:02.925Zhttps://docs.openclaw.ai/plugins/sdk-runtime2026-05-01T07:59:55.539Zhttps://docs.openclaw.ai/plugins/sdk-setup2026-05-01T07:59:55.539Zhttps://docs.openclaw.ai/plugins/sdk-subpaths2026-05-01T07:59:55.550Zhttps://docs.openclaw.ai/plugins/sdk-testing2026-05-01T07:59:55.549Zhttps://docs.openclaw.ai/plugins/skill-workshop2026-05-01T07:59:55.548Zhttps://docs.openclaw.ai/plugins/voice-call2026-05-01T11:39:30.450Zhttps://docs.openclaw.ai/plugins/webhooks2026-05-01T07:59:55.548Zhttps://docs.openclaw.ai/plugins/zalouser2026-05-01T07:59:55.540Zhttps://docs.openclaw.ai/prose2026-05-01T07:59:55.540Zhttps://docs.openclaw.ai/providers/alibaba2026-05-01T07:59:55.580Zhttps://docs.openclaw.ai/providers/anthropic2026-05-01T07:59:55.571Zhttps://docs.openclaw.ai/providers/arcee2026-05-01T07:59:55.578Zhttps://docs.openclaw.ai/providers/azure-speech2026-05-01T07:59:55.579Zhttps://docs.openclaw.ai/providers/bedrock2026-05-01T07:59:55.575Zhttps://docs.openclaw.ai/providers/bedrock-mantle2026-05-01T07:59:55.572Zhttps://docs.openclaw.ai/providers/chutes2026-05-01T07:59:55.570Zhttps://docs.openclaw.ai/providers/claude-max-api-proxy2026-05-01T07:59:55.571Zhttps://docs.openclaw.ai/providers/cloudflare-ai-gateway2026-05-01T07:59:55.570Zhttps://docs.openclaw.ai/providers/comfy2026-05-01T07:59:55.609Zhttps://docs.openclaw.ai/providers/deepgram2026-05-01T07:59:55.607Zhttps://docs.openclaw.ai/providers/deepinfra2026-05-01T07:59:55.608Zhttps://docs.openclaw.ai/providers/deepseek2026-05-01T07:59:55.600Zhttps://docs.openclaw.ai/providers/elevenlabs2026-05-01T07:59:55.600Zhttps://docs.openclaw.ai/providers/fal2026-05-01T07:59:55.601Zhttps://docs.openclaw.ai/providers/fireworks2026-05-01T07:59:55.602Zhttps://docs.openclaw.ai/providers/github-copilot2026-05-01T07:59:55.604Zhttps://docs.openclaw.ai/providers/glm2026-05-01T07:59:55.605Zhttps://docs.openclaw.ai/providers/google2026-05-01T07:59:55.601Zhttps://docs.openclaw.ai/providers/gradium2026-05-01T07:59:55.647Zhttps://docs.openclaw.ai/providers/groq2026-05-01T12:49:52.542Zhttps://docs.openclaw.ai/providers/huggingface2026-05-01T07:59:55.646Zhttps://docs.openclaw.ai/providers2026-05-01T07:59:55.638Zhttps://docs.openclaw.ai/providers/inferrs2026-05-01T07:59:55.643Zhttps://docs.openclaw.ai/providers/inworld2026-05-01T07:59:55.639Zhttps://docs.openclaw.ai/providers/kilocode2026-05-01T07:59:55.643Zhttps://docs.openclaw.ai/providers/litellm2026-05-01T07:59:55.640Zhttps://docs.openclaw.ai/providers/lmstudio2026-05-01T07:59:55.638Zhttps://docs.openclaw.ai/providers/minimax2026-05-01T07:59:55.639Zhttps://docs.openclaw.ai/providers/mistral2026-05-01T07:59:55.676Zhttps://docs.openclaw.ai/providers/models2026-05-01T07:59:55.675Zhttps://docs.openclaw.ai/providers/moonshot2026-05-01T07:59:55.675Zhttps://docs.openclaw.ai/providers/nvidia2026-05-01T07:59:55.669Zhttps://docs.openclaw.ai/providers/ollama2026-05-01T07:59:55.668Zhttps://docs.openclaw.ai/providers/openai2026-05-01T07:59:55.670Zhttps://docs.openclaw.ai/providers/opencode2026-05-01T07:59:55.668Zhttps://docs.openclaw.ai/providers/opencode-go2026-05-01T07:59:55.669Zhttps://docs.openclaw.ai/providers/openrouter2026-05-01T07:59:55.673Zhttps://docs.openclaw.ai/providers/perplexity-provider2026-05-01T07:59:55.667Zhttps://docs.openclaw.ai/providers/qianfan2026-05-01T07:59:55.705Zhttps://docs.openclaw.ai/providers/qwen2026-05-01T07:59:55.704Zhttps://docs.openclaw.ai/providers/runway2026-05-01T07:59:55.704Zhttps://docs.openclaw.ai/providers/sglang2026-05-01T07:59:55.703Zhttps://docs.openclaw.ai/providers/stepfun2026-05-01T07:59:55.702Zhttps://docs.openclaw.ai/providers/synthetic2026-05-01T07:59:55.696Zhttps://docs.openclaw.ai/providers/tencent2026-05-01T07:59:55.696Zhttps://docs.openclaw.ai/providers/together2026-05-01T07:59:55.695Zhttps://docs.openclaw.ai/providers/venice2026-05-01T12:58:54.645Zhttps://docs.openclaw.ai/providers/vercel-ai-gateway2026-05-01T07:59:55.743Zhttps://docs.openclaw.ai/providers/vllm2026-05-01T07:59:55.742Zhttps://docs.openclaw.ai/providers/volcengine2026-05-01T07:59:55.740Zhttps://docs.openclaw.ai/providers/vydra2026-05-01T07:59:55.745Zhttps://docs.openclaw.ai/providers/xai2026-05-01T07:59:55.744Zhttps://docs.openclaw.ai/providers/xiaomi2026-05-01T07:59:55.738Zhttps://docs.openclaw.ai/providers/zai2026-05-01T13:14:16.780Zhttps://docs.openclaw.ai/pt-BR/auth-credential-semantics2026-05-01T07:59:55.735Zhttps://docs.openclaw.ai/pt-BR/automation/cron-jobs2026-05-01T07:59:55.772Zhttps://docs.openclaw.ai/pt-BR/automation/hooks2026-05-01T07:59:55.772Zhttps://docs.openclaw.ai/pt-BR/automation2026-05-01T07:59:55.764Zhttps://docs.openclaw.ai/pt-BR/automation/standing-orders2026-05-01T07:59:55.773Zhttps://docs.openclaw.ai/pt-BR/automation/taskflow2026-05-01T07:59:55.764Zhttps://docs.openclaw.ai/pt-BR/automation/tasks2026-05-01T07:59:55.770Zhttps://docs.openclaw.ai/pt-BR/channels/bluebubbles2026-05-01T07:59:55.804Zhttps://docs.openclaw.ai/pt-BR/channels/broadcast-groups2026-05-01T07:59:55.802Zhttps://docs.openclaw.ai/pt-BR/channels/channel-routing2026-05-01T07:59:55.800Zhttps://docs.openclaw.ai/pt-BR/channels/discord2026-05-01T07:59:55.803Zhttps://docs.openclaw.ai/pt-BR/channels/feishu2026-05-01T07:59:55.794Zhttps://docs.openclaw.ai/pt-BR/channels/googlechat2026-05-01T07:59:55.795Zhttps://docs.openclaw.ai/pt-BR/channels/group-messages2026-05-01T07:59:55.793Zhttps://docs.openclaw.ai/pt-BR/channels/groups2026-05-01T07:59:55.848Zhttps://docs.openclaw.ai/pt-BR/channels/imessage2026-05-01T07:59:55.848Zhttps://docs.openclaw.ai/pt-BR/channels2026-05-01T07:59:55.842Zhttps://docs.openclaw.ai/pt-BR/channels/irc2026-05-01T07:59:55.841Zhttps://docs.openclaw.ai/pt-BR/channels/line2026-05-01T07:59:55.827Zhttps://docs.openclaw.ai/pt-BR/channels/location2026-05-01T07:59:55.843Zhttps://docs.openclaw.ai/pt-BR/channels/matrix2026-05-01T07:59:55.826Zhttps://docs.openclaw.ai/pt-BR/channels/matrix-migration2026-05-01T07:59:55.826Zhttps://docs.openclaw.ai/pt-BR/channels/matrix-push-rules2026-05-01T07:59:55.827Zhttps://docs.openclaw.ai/pt-BR/channels/mattermost2026-05-01T07:59:55.825Zhttps://docs.openclaw.ai/pt-BR/channels/msteams2026-05-01T07:59:55.879Zhttps://docs.openclaw.ai/pt-BR/channels/nextcloud-talk2026-05-01T07:59:55.875Zhttps://docs.openclaw.ai/pt-BR/channels/nostr2026-05-01T07:59:55.873Zhttps://docs.openclaw.ai/pt-BR/channels/pairing2026-05-01T07:59:55.878Zhttps://docs.openclaw.ai/pt-BR/channels/qa-channel2026-05-01T07:59:55.869Zhttps://docs.openclaw.ai/pt-BR/channels/qqbot2026-05-01T07:59:55.877Zhttps://docs.openclaw.ai/pt-BR/channels/signal2026-05-01T07:59:55.868Zhttps://docs.openclaw.ai/pt-BR/channels/slack2026-05-01T07:59:55.869Zhttps://docs.openclaw.ai/pt-BR/channels/synology-chat2026-05-01T07:59:55.876Zhttps://docs.openclaw.ai/pt-BR/channels/telegram2026-05-01T07:59:55.867Zhttps://docs.openclaw.ai/pt-BR/channels/tlon2026-05-01T07:59:55.909Zhttps://docs.openclaw.ai/pt-BR/channels/troubleshooting2026-05-01T07:59:55.907Zhttps://docs.openclaw.ai/pt-BR/channels/twitch2026-05-01T07:59:55.906Zhttps://docs.openclaw.ai/pt-BR/channels/wechat2026-05-01T07:59:55.896Zhttps://docs.openclaw.ai/pt-BR/channels/whatsapp2026-05-01T07:59:55.908Zhttps://docs.openclaw.ai/pt-BR/channels/yuanbao2026-05-01T07:59:55.903Zhttps://docs.openclaw.ai/pt-BR/channels/zalo2026-05-01T07:59:55.907Zhttps://docs.openclaw.ai/pt-BR/channels/zalouser2026-05-01T07:59:55.896Zhttps://docs.openclaw.ai/pt-BR/ci2026-05-01T07:59:55.905Zhttps://docs.openclaw.ai/pt-BR/cli/acp2026-05-01T07:59:55.906Zhttps://docs.openclaw.ai/pt-BR/cli/agent2026-05-01T07:59:55.949Zhttps://docs.openclaw.ai/pt-BR/cli/agents2026-05-01T07:59:55.947Zhttps://docs.openclaw.ai/pt-BR/cli/approvals2026-05-01T07:59:55.949Zhttps://docs.openclaw.ai/pt-BR/cli/backup2026-05-01T07:59:55.948Zhttps://docs.openclaw.ai/pt-BR/cli/browser2026-05-01T07:59:55.948Zhttps://docs.openclaw.ai/pt-BR/cli/channels2026-05-01T07:59:55.946Zhttps://docs.openclaw.ai/pt-BR/cli/clawbot2026-05-01T07:59:55.929Zhttps://docs.openclaw.ai/pt-BR/cli/commitments2026-05-01T07:59:55.928Zhttps://docs.openclaw.ai/pt-BR/cli/completion2026-05-01T07:59:55.929Zhttps://docs.openclaw.ai/pt-BR/cli/config2026-05-01T07:59:55.928Zhttps://docs.openclaw.ai/pt-BR/cli/configure2026-05-01T07:59:55.978Zhttps://docs.openclaw.ai/pt-BR/cli/cron2026-05-01T07:59:55.977Zhttps://docs.openclaw.ai/pt-BR/cli/daemon2026-05-01T07:59:55.972Zhttps://docs.openclaw.ai/pt-BR/cli/dashboard2026-05-01T07:59:55.978Zhttps://docs.openclaw.ai/pt-BR/cli/devices2026-05-01T07:59:55.975Zhttps://docs.openclaw.ai/pt-BR/cli/directory2026-05-01T07:59:55.970Zhttps://docs.openclaw.ai/pt-BR/cli/dns2026-05-01T07:59:55.971Zhttps://docs.openclaw.ai/pt-BR/cli/docs2026-05-01T07:59:55.970Zhttps://docs.openclaw.ai/pt-BR/cli/doctor2026-05-01T07:59:55.976Zhttps://docs.openclaw.ai/pt-BR/cli/flows2026-05-01T07:59:56.007Zhttps://docs.openclaw.ai/pt-BR/cli/gateway2026-05-01T07:59:56.006Zhttps://docs.openclaw.ai/pt-BR/cli/health2026-05-01T07:59:56.005Zhttps://docs.openclaw.ai/pt-BR/cli/hooks2026-05-01T07:59:56.003Zhttps://docs.openclaw.ai/pt-BR/cli2026-05-01T07:59:55.996Zhttps://docs.openclaw.ai/pt-BR/cli/infer2026-05-01T07:59:56.005Zhttps://docs.openclaw.ai/pt-BR/cli/logs2026-05-01T07:59:56.006Zhttps://docs.openclaw.ai/pt-BR/cli/mcp2026-05-01T07:59:56.007Zhttps://docs.openclaw.ai/pt-BR/cli/memory2026-05-01T07:59:56.004Zhttps://docs.openclaw.ai/pt-BR/cli/message2026-05-01T07:59:55.996Zhttps://docs.openclaw.ai/pt-BR/cli/migrate2026-05-01T07:59:56.037Zhttps://docs.openclaw.ai/pt-BR/cli/models2026-05-01T07:59:56.059Zhttps://docs.openclaw.ai/pt-BR/cli/node2026-05-01T07:59:56.056Zhttps://docs.openclaw.ai/pt-BR/cli/nodes2026-05-01T07:59:56.038Zhttps://docs.openclaw.ai/pt-BR/cli/onboard2026-05-01T07:59:56.055Zhttps://docs.openclaw.ai/pt-BR/cli/pairing2026-05-01T07:59:56.056Zhttps://docs.openclaw.ai/pt-BR/cli/plugins2026-05-01T07:59:56.057Zhttps://docs.openclaw.ai/pt-BR/cli/proxy2026-05-01T07:59:56.038Zhttps://docs.openclaw.ai/pt-BR/cli/qr2026-05-01T07:59:56.042Zhttps://docs.openclaw.ai/pt-BR/cli/reset2026-05-01T07:59:56.037Zhttps://docs.openclaw.ai/pt-BR/cli/sandbox2026-05-01T07:59:56.089Zhttps://docs.openclaw.ai/pt-BR/cli/secrets2026-05-01T07:59:56.087Zhttps://docs.openclaw.ai/pt-BR/cli/security2026-05-01T07:59:56.088Zhttps://docs.openclaw.ai/pt-BR/cli/sessions2026-05-01T07:59:56.087Zhttps://docs.openclaw.ai/pt-BR/cli/setup2026-05-01T07:59:56.086Zhttps://docs.openclaw.ai/pt-BR/cli/skills2026-05-01T07:59:56.084Zhttps://docs.openclaw.ai/pt-BR/cli/status2026-05-01T07:59:56.079Zhttps://docs.openclaw.ai/pt-BR/cli/system2026-05-01T07:59:56.078Zhttps://docs.openclaw.ai/pt-BR/cli/tasks2026-05-01T07:59:56.077Zhttps://docs.openclaw.ai/pt-BR/cli/tui2026-05-01T07:59:56.077Zhttps://docs.openclaw.ai/pt-BR/cli/uninstall2026-05-01T07:59:56.123Zhttps://docs.openclaw.ai/pt-BR/cli/update2026-05-01T07:59:56.122Zhttps://docs.openclaw.ai/pt-BR/cli/voicecall2026-05-01T07:59:56.116Zhttps://docs.openclaw.ai/pt-BR/cli/webhooks2026-05-01T07:59:56.121Zhttps://docs.openclaw.ai/pt-BR/cli/wiki2026-05-01T07:59:56.120Zhttps://docs.openclaw.ai/pt-BR/concepts/active-memory2026-05-01T07:59:56.114Zhttps://docs.openclaw.ai/pt-BR/concepts/agent2026-05-01T07:59:56.114Zhttps://docs.openclaw.ai/pt-BR/concepts/agent-loop2026-05-01T07:59:56.119Zhttps://docs.openclaw.ai/pt-BR/concepts/agent-runtimes2026-05-01T07:59:56.116Zhttps://docs.openclaw.ai/pt-BR/concepts/agent-workspace2026-05-01T07:59:56.115Zhttps://docs.openclaw.ai/pt-BR/concepts/architecture2026-05-01T07:59:56.163Zhttps://docs.openclaw.ai/pt-BR/concepts/channel-docking2026-05-01T07:59:56.163Zhttps://docs.openclaw.ai/pt-BR/concepts/commitments2026-05-01T07:59:56.161Zhttps://docs.openclaw.ai/pt-BR/concepts/compaction2026-05-01T07:59:56.144Zhttps://docs.openclaw.ai/pt-BR/concepts/context2026-05-01T07:59:56.162Zhttps://docs.openclaw.ai/pt-BR/concepts/context-engine2026-05-01T07:59:56.145Zhttps://docs.openclaw.ai/pt-BR/concepts/delegate-architecture2026-05-01T07:59:56.143Zhttps://docs.openclaw.ai/pt-BR/concepts/dreaming2026-05-01T07:59:56.143Zhttps://docs.openclaw.ai/pt-BR/concepts/experimental-features2026-05-01T07:59:56.144Zhttps://docs.openclaw.ai/pt-BR/concepts/features2026-05-01T07:59:56.148Zhttps://docs.openclaw.ai/pt-BR/concepts/markdown-formatting2026-05-01T07:59:56.208Zhttps://docs.openclaw.ai/pt-BR/concepts/memory2026-05-01T07:59:56.185Zhttps://docs.openclaw.ai/pt-BR/concepts/memory-builtin2026-05-01T07:59:56.207Zhttps://docs.openclaw.ai/pt-BR/concepts/memory-honcho2026-05-01T07:59:56.187Zhttps://docs.openclaw.ai/pt-BR/concepts/memory-qmd2026-05-01T07:59:56.206Zhttps://docs.openclaw.ai/pt-BR/concepts/memory-search2026-05-01T07:59:56.204Zhttps://docs.openclaw.ai/pt-BR/concepts/messages2026-05-01T07:59:56.185Zhttps://docs.openclaw.ai/pt-BR/concepts/model-failover2026-05-01T07:59:56.187Zhttps://docs.openclaw.ai/pt-BR/concepts/model-providers2026-05-01T07:59:56.186Zhttps://docs.openclaw.ai/pt-BR/concepts/models2026-05-01T07:59:56.184Zhttps://docs.openclaw.ai/pt-BR/concepts/multi-agent2026-05-01T07:59:56.246Zhttps://docs.openclaw.ai/pt-BR/concepts/oauth2026-05-01T07:59:56.245Zhttps://docs.openclaw.ai/pt-BR/concepts/openclaw-sdk2026-05-01T07:59:56.244Zhttps://docs.openclaw.ai/pt-BR/concepts/presence2026-05-01T07:59:56.237Zhttps://docs.openclaw.ai/pt-BR/concepts/qa-e2e-automation2026-05-01T07:59:56.242Zhttps://docs.openclaw.ai/pt-BR/concepts/qa-matrix2026-05-01T07:59:56.243Zhttps://docs.openclaw.ai/pt-BR/concepts/queue2026-05-01T07:59:56.239Zhttps://docs.openclaw.ai/pt-BR/concepts/queue-steering2026-05-01T07:59:56.238Zhttps://docs.openclaw.ai/pt-BR/concepts/retry2026-05-01T07:59:56.238Zhttps://docs.openclaw.ai/pt-BR/concepts/session2026-05-01T07:59:56.274Zhttps://docs.openclaw.ai/pt-BR/concepts/session-pruning2026-05-01T07:59:56.237Zhttps://docs.openclaw.ai/pt-BR/concepts/session-tool2026-05-01T07:59:56.276Zhttps://docs.openclaw.ai/pt-BR/concepts/soul2026-05-01T07:59:56.273Zhttps://docs.openclaw.ai/pt-BR/concepts/streaming2026-05-01T07:59:56.273Zhttps://docs.openclaw.ai/pt-BR/concepts/system-prompt2026-05-01T07:59:56.270Zhttps://docs.openclaw.ai/pt-BR/concepts/timezone2026-05-01T07:59:56.270Zhttps://docs.openclaw.ai/pt-BR/concepts/typebox2026-05-01T07:59:56.268Zhttps://docs.openclaw.ai/pt-BR/concepts/typing-indicators2026-05-01T07:59:56.266Zhttps://docs.openclaw.ai/pt-BR/concepts/usage-tracking2026-05-01T07:59:56.266Zhttps://docs.openclaw.ai/pt-BR/date-time2026-05-01T07:59:56.271Zhttps://docs.openclaw.ai/pt-BR/debug/node-issue2026-05-01T07:59:56.307Zhttps://docs.openclaw.ai/pt-BR/diagnostics/flags2026-05-01T07:59:56.304Zhttps://docs.openclaw.ai/pt-BR/gateway/authentication2026-05-01T07:59:56.298Zhttps://docs.openclaw.ai/pt-BR/gateway/background-process2026-05-01T07:59:56.306Zhttps://docs.openclaw.ai/pt-BR/gateway/bonjour2026-05-01T07:59:56.303Zhttps://docs.openclaw.ai/pt-BR/gateway/bridge-protocol2026-05-01T07:59:56.301Zhttps://docs.openclaw.ai/pt-BR/gateway/cli-backends2026-05-01T07:59:56.299Zhttps://docs.openclaw.ai/pt-BR/gateway/config-agents2026-05-01T07:59:56.305Zhttps://docs.openclaw.ai/pt-BR/gateway/config-channels2026-05-01T07:59:56.304Zhttps://docs.openclaw.ai/pt-BR/gateway/config-tools2026-05-01T07:59:56.293Zhttps://docs.openclaw.ai/pt-BR/gateway/configuration2026-05-01T07:59:56.340Zhttps://docs.openclaw.ai/pt-BR/gateway/configuration-examples2026-05-01T07:59:56.349Zhttps://docs.openclaw.ai/pt-BR/gateway/configuration-reference2026-05-01T07:59:56.350Zhttps://docs.openclaw.ai/pt-BR/gateway/diagnostics2026-05-01T07:59:56.340Zhttps://docs.openclaw.ai/pt-BR/gateway/discovery2026-05-01T07:59:56.349Zhttps://docs.openclaw.ai/pt-BR/gateway/doctor2026-05-01T07:59:56.341Zhttps://docs.openclaw.ai/pt-BR/gateway/gateway-lock2026-05-01T07:59:56.347Zhttps://docs.openclaw.ai/pt-BR/gateway/health2026-05-01T07:59:56.341Zhttps://docs.openclaw.ai/pt-BR/gateway/heartbeat2026-05-01T07:59:56.348Zhttps://docs.openclaw.ai/pt-BR/gateway2026-05-01T07:59:56.339Zhttps://docs.openclaw.ai/pt-BR/gateway/local-models2026-05-01T07:59:56.380Zhttps://docs.openclaw.ai/pt-BR/gateway/logging2026-05-01T07:59:56.381Zhttps://docs.openclaw.ai/pt-BR/gateway/multiple-gateways2026-05-01T07:59:56.379Zhttps://docs.openclaw.ai/pt-BR/gateway/network-model2026-05-01T07:59:56.379Zhttps://docs.openclaw.ai/pt-BR/gateway/openai-http-api2026-05-01T07:59:56.376Zhttps://docs.openclaw.ai/pt-BR/gateway/openresponses-http-api2026-05-01T07:59:56.373Zhttps://docs.openclaw.ai/pt-BR/gateway/openshell2026-05-01T07:59:56.372Zhttps://docs.openclaw.ai/pt-BR/gateway/opentelemetry2026-05-01T07:59:56.372Zhttps://docs.openclaw.ai/pt-BR/gateway/pairing2026-05-01T07:59:56.371Zhttps://docs.openclaw.ai/pt-BR/gateway/prometheus2026-05-01T07:59:56.371Zhttps://docs.openclaw.ai/pt-BR/gateway/protocol2026-05-01T07:59:56.411Zhttps://docs.openclaw.ai/pt-BR/gateway/remote2026-05-01T07:59:56.410Zhttps://docs.openclaw.ai/pt-BR/gateway/remote-gateway-readme2026-05-01T07:59:56.409Zhttps://docs.openclaw.ai/pt-BR/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T07:59:56.410Zhttps://docs.openclaw.ai/pt-BR/gateway/sandboxing2026-05-01T07:59:56.407Zhttps://docs.openclaw.ai/pt-BR/gateway/secrets2026-05-01T07:59:56.401Zhttps://docs.openclaw.ai/pt-BR/gateway/secrets-plan-contract2026-05-01T07:59:56.408Zhttps://docs.openclaw.ai/pt-BR/gateway/security/audit-checks2026-05-01T07:59:56.402Zhttps://docs.openclaw.ai/pt-BR/gateway/security2026-04-30T10:18:05.550Zhttps://docs.openclaw.ai/pt-BR/gateway/tailscale2026-05-01T07:59:56.401Zhttps://docs.openclaw.ai/pt-BR/gateway/tools-invoke-http-api2026-05-01T07:59:56.400Zhttps://docs.openclaw.ai/pt-BR/gateway/troubleshooting2026-05-01T07:59:56.454Zhttps://docs.openclaw.ai/pt-BR/gateway/trusted-proxy-auth2026-05-01T07:59:56.453Zhttps://docs.openclaw.ai/pt-BR/help/debugging2026-05-01T07:59:56.446Zhttps://docs.openclaw.ai/pt-BR/help/environment2026-05-01T07:59:56.453Zhttps://docs.openclaw.ai/pt-BR/help/faq2026-05-01T07:59:56.446Zhttps://docs.openclaw.ai/pt-BR/help/faq-first-run2026-05-01T07:59:56.445Zhttps://docs.openclaw.ai/pt-BR/help/faq-models2026-05-01T07:59:56.447Zhttps://docs.openclaw.ai/pt-BR/help/gpt55-codex-agentic-parity2026-05-01T07:59:56.444Zhttps://docs.openclaw.ai/pt-BR/help/gpt55-codex-agentic-parity-maintainers2026-05-01T07:59:56.444Zhttps://docs.openclaw.ai/pt-BR/help2026-05-01T07:59:56.447Zhttps://docs.openclaw.ai/pt-BR/help/scripts2026-05-01T07:59:56.485Zhttps://docs.openclaw.ai/pt-BR/help/testing2026-05-01T07:59:56.484Zhttps://docs.openclaw.ai/pt-BR/help/testing-live2026-05-01T07:59:56.484Zhttps://docs.openclaw.ai/pt-BR/help/troubleshooting2026-05-01T07:59:56.476Zhttps://docs.openclaw.ai/pt-BR2026-05-01T07:59:56.482Zhttps://docs.openclaw.ai/pt-BR/install/ansible2026-05-01T07:59:56.477Zhttps://docs.openclaw.ai/pt-BR/install/azure2026-05-01T07:59:56.475Zhttps://docs.openclaw.ai/pt-BR/install/bun2026-05-01T07:59:56.477Zhttps://docs.openclaw.ai/pt-BR/install/clawdock2026-05-01T07:59:56.476Zhttps://docs.openclaw.ai/pt-BR/install/development-channels2026-05-01T07:59:56.474Zhttps://docs.openclaw.ai/pt-BR/install/digitalocean2026-05-01T07:59:56.515Zhttps://docs.openclaw.ai/pt-BR/install/docker2026-05-01T07:59:56.513Zhttps://docs.openclaw.ai/pt-BR/install/docker-vm-runtime2026-05-01T07:59:56.513Zhttps://docs.openclaw.ai/pt-BR/install/exe-dev2026-05-01T07:59:56.510Zhttps://docs.openclaw.ai/pt-BR/install/fly2026-05-01T07:59:56.506Zhttps://docs.openclaw.ai/pt-BR/install/gcp2026-05-01T07:59:56.514Zhttps://docs.openclaw.ai/pt-BR/install/hetzner2026-05-01T07:59:56.504Zhttps://docs.openclaw.ai/pt-BR/install/hostinger2026-05-01T07:59:56.505Zhttps://docs.openclaw.ai/pt-BR/install2026-05-01T07:59:56.512Zhttps://docs.openclaw.ai/pt-BR/install/installer2026-05-01T07:59:56.505Zhttps://docs.openclaw.ai/pt-BR/install/kubernetes2026-05-01T07:59:56.551Zhttps://docs.openclaw.ai/pt-BR/install/macos-vm2026-05-01T07:59:56.550Zhttps://docs.openclaw.ai/pt-BR/install/migrating2026-05-01T07:59:56.549Zhttps://docs.openclaw.ai/pt-BR/install/migrating-claude2026-05-01T07:59:56.552Zhttps://docs.openclaw.ai/pt-BR/install/migrating-hermes2026-05-01T07:59:56.534Zhttps://docs.openclaw.ai/pt-BR/install/nix2026-05-01T07:59:56.553Zhttps://docs.openclaw.ai/pt-BR/install/node2026-05-01T07:59:56.552Zhttps://docs.openclaw.ai/pt-BR/install/northflank2026-05-01T07:59:56.549Zhttps://docs.openclaw.ai/pt-BR/install/oracle2026-05-01T07:59:56.533Zhttps://docs.openclaw.ai/pt-BR/install/podman2026-05-01T07:59:56.533Zhttps://docs.openclaw.ai/pt-BR/install/railway2026-05-01T07:59:56.569Zhttps://docs.openclaw.ai/pt-BR/install/raspberry-pi2026-05-01T07:59:56.573Zhttps://docs.openclaw.ai/pt-BR/install/render2026-05-01T07:59:56.576Zhttps://docs.openclaw.ai/pt-BR/install/uninstall2026-05-01T07:59:56.580Zhttps://docs.openclaw.ai/pt-BR/install/updating2026-05-01T07:59:56.579Zhttps://docs.openclaw.ai/pt-BR/logging2026-05-01T07:59:56.582Zhttps://docs.openclaw.ai/pt-BR/network2026-05-01T07:59:56.577Zhttps://docs.openclaw.ai/pt-BR/nodes/audio2026-05-01T07:59:56.576Zhttps://docs.openclaw.ai/pt-BR/nodes/camera2026-05-01T07:59:56.574Zhttps://docs.openclaw.ai/pt-BR/nodes/images2026-05-01T07:59:56.572Zhttps://docs.openclaw.ai/pt-BR/nodes2026-05-01T07:59:56.612Zhttps://docs.openclaw.ai/pt-BR/nodes/location-command2026-05-01T07:59:56.610Zhttps://docs.openclaw.ai/pt-BR/nodes/media-understanding2026-05-01T07:59:56.609Zhttps://docs.openclaw.ai/pt-BR/nodes/talk2026-05-01T07:59:56.606Zhttps://docs.openclaw.ai/pt-BR/nodes/troubleshooting2026-05-01T07:59:56.611Zhttps://docs.openclaw.ai/pt-BR/nodes/voicewake2026-05-01T07:59:56.602Zhttps://docs.openclaw.ai/pt-BR/pi2026-05-01T07:59:56.600Zhttps://docs.openclaw.ai/pt-BR/pi-dev2026-05-01T07:59:56.601Zhttps://docs.openclaw.ai/pt-BR/platforms/android2026-05-01T07:59:56.639Zhttps://docs.openclaw.ai/pt-BR/platforms2026-05-01T07:59:56.630Zhttps://docs.openclaw.ai/pt-BR/platforms/ios2026-05-01T07:59:56.638Zhttps://docs.openclaw.ai/pt-BR/platforms/linux2026-05-01T07:59:56.635Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/bundled-gateway2026-05-01T07:59:56.637Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/canvas2026-05-01T07:59:56.631Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/child-process2026-05-01T07:59:56.630Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/dev-setup2026-05-01T07:59:56.629Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/health2026-05-01T07:59:56.679Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/icon2026-05-01T07:59:56.677Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/logging2026-05-01T07:59:56.675Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/menu-bar2026-05-01T07:59:56.678Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/peekaboo2026-05-01T07:59:56.674Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/permissions2026-05-01T07:59:56.671Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/remote2026-05-01T07:59:56.672Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/signing2026-05-01T07:59:56.671Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/skills2026-05-01T07:59:56.670Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/voice-overlay2026-05-01T07:59:56.676Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/voicewake2026-05-01T07:59:56.711Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/webchat2026-05-01T07:59:56.710Zhttps://docs.openclaw.ai/pt-BR/platforms/mac/xpc2026-05-01T07:59:56.710Zhttps://docs.openclaw.ai/pt-BR/platforms/macos2026-05-01T07:59:56.702Zhttps://docs.openclaw.ai/pt-BR/platforms/windows2026-05-01T07:59:56.712Zhttps://docs.openclaw.ai/pt-BR/plugins/architecture2026-05-01T07:59:56.702Zhttps://docs.openclaw.ai/pt-BR/plugins/architecture-internals2026-05-01T07:59:56.703Zhttps://docs.openclaw.ai/pt-BR/plugins/building-plugins2026-05-01T07:59:56.758Zhttps://docs.openclaw.ai/pt-BR/plugins/bundles2026-05-01T07:59:56.758Zhttps://docs.openclaw.ai/pt-BR/plugins/codex-computer-use2026-05-01T07:59:56.757Zhttps://docs.openclaw.ai/pt-BR/plugins/codex-harness2026-05-01T07:59:56.759Zhttps://docs.openclaw.ai/pt-BR/plugins/community2026-05-01T07:59:56.742Zhttps://docs.openclaw.ai/pt-BR/plugins/compatibility2026-05-01T07:59:56.740Zhttps://docs.openclaw.ai/pt-BR/plugins/google-meet2026-05-01T07:59:56.739Zhttps://docs.openclaw.ai/pt-BR/plugins/hooks2026-05-01T07:59:56.739Zhttps://docs.openclaw.ai/pt-BR/plugins/manifest2026-05-01T07:59:56.738Zhttps://docs.openclaw.ai/pt-BR/plugins/memory-lancedb2026-05-01T07:59:56.790Zhttps://docs.openclaw.ai/pt-BR/plugins/memory-wiki2026-05-01T07:59:56.794Zhttps://docs.openclaw.ai/pt-BR/plugins/message-presentation2026-05-01T07:59:56.789Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-agent-harness2026-05-01T07:59:56.787Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-channel-plugins2026-05-01T07:59:56.789Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-channel-turn2026-05-01T07:59:56.779Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-entrypoints2026-05-01T07:59:56.778Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-migration2026-05-01T07:59:56.786Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-overview2026-05-01T07:59:56.787Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-provider-plugins2026-05-01T07:59:56.778Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-runtime2026-05-01T07:59:56.822Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-setup2026-05-01T07:59:56.824Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-subpaths2026-05-01T07:59:56.825Zhttps://docs.openclaw.ai/pt-BR/plugins/sdk-testing2026-05-01T07:59:56.822Zhttps://docs.openclaw.ai/pt-BR/plugins/skill-workshop2026-05-01T07:59:56.824Zhttps://docs.openclaw.ai/pt-BR/plugins/voice-call2026-05-01T07:59:56.825Zhttps://docs.openclaw.ai/pt-BR/plugins/webhooks2026-05-01T07:59:56.823Zhttps://docs.openclaw.ai/pt-BR/plugins/zalouser2026-05-01T07:59:56.813Zhttps://docs.openclaw.ai/pt-BR/prose2026-05-01T07:59:56.813Zhttps://docs.openclaw.ai/pt-BR/providers/alibaba2026-05-01T07:59:56.812Zhttps://docs.openclaw.ai/pt-BR/providers/anthropic2026-05-01T07:59:56.847Zhttps://docs.openclaw.ai/pt-BR/providers/arcee2026-05-01T07:59:56.848Zhttps://docs.openclaw.ai/pt-BR/providers/azure-speech2026-05-01T07:59:56.865Zhttps://docs.openclaw.ai/pt-BR/providers/bedrock2026-05-01T07:59:56.849Zhttps://docs.openclaw.ai/pt-BR/providers/bedrock-mantle2026-05-01T07:59:56.866Zhttps://docs.openclaw.ai/pt-BR/providers/chutes2026-05-01T07:59:56.851Zhttps://docs.openclaw.ai/pt-BR/providers/claude-max-api-proxy2026-05-01T07:59:56.848Zhttps://docs.openclaw.ai/pt-BR/providers/cloudflare-ai-gateway2026-05-01T07:59:56.847Zhttps://docs.openclaw.ai/pt-BR/providers/comfy2026-05-01T07:59:56.846Zhttps://docs.openclaw.ai/pt-BR/providers/deepgram2026-05-01T07:59:56.909Zhttps://docs.openclaw.ai/pt-BR/providers/deepinfra2026-05-01T07:59:56.908Zhttps://docs.openclaw.ai/pt-BR/providers/deepseek2026-05-01T07:59:56.909Zhttps://docs.openclaw.ai/pt-BR/providers/elevenlabs2026-05-01T07:59:56.907Zhttps://docs.openclaw.ai/pt-BR/providers/fal2026-05-01T07:59:56.887Zhttps://docs.openclaw.ai/pt-BR/providers/fireworks2026-05-01T07:59:56.886Zhttps://docs.openclaw.ai/pt-BR/providers/github-copilot2026-05-01T07:59:56.887Zhttps://docs.openclaw.ai/pt-BR/providers/glm2026-05-01T07:59:56.886Zhttps://docs.openclaw.ai/pt-BR/providers/google2026-05-01T07:59:56.892Zhttps://docs.openclaw.ai/pt-BR/providers/gradium2026-05-01T07:59:56.885Zhttps://docs.openclaw.ai/pt-BR/providers/groq2026-05-01T07:59:56.947Zhttps://docs.openclaw.ai/pt-BR/providers/huggingface2026-05-01T07:59:56.946Zhttps://docs.openclaw.ai/pt-BR/providers2026-05-01T07:59:56.946Zhttps://docs.openclaw.ai/pt-BR/providers/inferrs2026-05-01T07:59:56.944Zhttps://docs.openclaw.ai/pt-BR/providers/inworld2026-05-01T07:59:56.938Zhttps://docs.openclaw.ai/pt-BR/providers/kilocode2026-05-01T07:59:56.944Zhttps://docs.openclaw.ai/pt-BR/providers/litellm2026-05-01T07:59:56.940Zhttps://docs.openclaw.ai/pt-BR/providers/lmstudio2026-05-01T07:59:56.940Zhttps://docs.openclaw.ai/pt-BR/providers/minimax2026-05-01T07:59:56.939Zhttps://docs.openclaw.ai/pt-BR/providers/mistral2026-05-01T07:59:56.939Zhttps://docs.openclaw.ai/pt-BR/providers/models2026-05-01T07:59:56.978Zhttps://docs.openclaw.ai/pt-BR/providers/moonshot2026-05-01T07:59:56.977Zhttps://docs.openclaw.ai/pt-BR/providers/nvidia2026-05-01T07:59:56.977Zhttps://docs.openclaw.ai/pt-BR/providers/ollama2026-05-01T07:59:56.970Zhttps://docs.openclaw.ai/pt-BR/providers/openai2026-05-01T07:59:56.970Zhttps://docs.openclaw.ai/pt-BR/providers/opencode2026-05-01T07:59:56.969Zhttps://docs.openclaw.ai/pt-BR/providers/opencode-go2026-05-01T07:59:56.969Zhttps://docs.openclaw.ai/pt-BR/providers/openrouter2026-05-01T07:59:56.968Zhttps://docs.openclaw.ai/pt-BR/providers/perplexity-provider2026-05-01T07:59:56.974Zhttps://docs.openclaw.ai/pt-BR/providers/qianfan2026-05-01T07:59:56.971Zhttps://docs.openclaw.ai/pt-BR/providers/qwen2026-05-01T07:59:57.007Zhttps://docs.openclaw.ai/pt-BR/providers/runway2026-05-01T07:59:57.006Zhttps://docs.openclaw.ai/pt-BR/providers/sglang2026-05-01T07:59:56.997Zhttps://docs.openclaw.ai/pt-BR/providers/stepfun2026-05-01T07:59:57.002Zhttps://docs.openclaw.ai/pt-BR/providers/synthetic2026-05-01T07:59:57.005Zhttps://docs.openclaw.ai/pt-BR/providers/tencent2026-05-01T07:59:57.006Zhttps://docs.openclaw.ai/pt-BR/providers/together2026-05-01T07:59:56.998Zhttps://docs.openclaw.ai/pt-BR/providers/venice2026-05-01T07:59:56.998Zhttps://docs.openclaw.ai/pt-BR/providers/vercel-ai-gateway2026-05-01T07:59:56.997Zhttps://docs.openclaw.ai/pt-BR/providers/vllm2026-05-01T07:59:57.047Zhttps://docs.openclaw.ai/pt-BR/providers/volcengine2026-05-01T07:59:57.047Zhttps://docs.openclaw.ai/pt-BR/providers/vydra2026-05-01T07:59:57.048Zhttps://docs.openclaw.ai/pt-BR/providers/xai2026-05-01T07:59:57.049Zhttps://docs.openclaw.ai/pt-BR/providers/xiaomi2026-05-01T07:59:57.046Zhttps://docs.openclaw.ai/pt-BR/providers/zai2026-05-01T07:59:57.043Zhttps://docs.openclaw.ai/pt-BR/reference/AGENTS.default2026-05-01T07:59:57.040Zhttps://docs.openclaw.ai/pt-BR/reference/RELEASING2026-05-01T07:59:57.039Zhttps://docs.openclaw.ai/pt-BR/reference/api-usage-costs2026-05-01T07:59:57.039Zhttps://docs.openclaw.ai/pt-BR/reference/credits2026-05-01T07:59:57.071Zhttps://docs.openclaw.ai/pt-BR/reference/device-models2026-05-01T07:59:57.079Zhttps://docs.openclaw.ai/pt-BR/reference/full-release-validation2026-05-01T07:59:57.071Zhttps://docs.openclaw.ai/pt-BR/reference/memory-config2026-05-01T07:59:57.078Zhttps://docs.openclaw.ai/pt-BR/reference/openclaw-sdk-api-design2026-05-01T07:59:57.078Zhttps://docs.openclaw.ai/pt-BR/reference/prompt-caching2026-05-01T07:59:57.076Zhttps://docs.openclaw.ai/pt-BR/reference/rich-output-protocol2026-05-01T07:59:57.072Zhttps://docs.openclaw.ai/pt-BR/reference/rpc2026-05-01T07:59:57.070Zhttps://docs.openclaw.ai/pt-BR/reference/secretref-credential-surface2026-05-01T07:59:57.070Zhttps://docs.openclaw.ai/pt-BR/reference/session-management-compaction2026-05-01T07:59:57.069Zhttps://docs.openclaw.ai/pt-BR/reference/templates/AGENTS2026-05-01T07:59:57.107Zhttps://docs.openclaw.ai/pt-BR/reference/templates/BOOT2026-05-01T07:59:57.107Zhttps://docs.openclaw.ai/pt-BR/reference/templates/BOOTSTRAP2026-05-01T07:59:57.103Zhttps://docs.openclaw.ai/pt-BR/reference/templates/HEARTBEAT2026-05-01T07:59:57.099Zhttps://docs.openclaw.ai/pt-BR/reference/templates/IDENTITY2026-05-01T07:59:57.106Zhttps://docs.openclaw.ai/pt-BR/reference/templates/SOUL2026-05-01T07:59:57.098Zhttps://docs.openclaw.ai/pt-BR/reference/templates/TOOLS2026-05-01T07:59:57.153Zhttps://docs.openclaw.ai/pt-BR/reference/templates/USER2026-05-01T07:59:57.145Zhttps://docs.openclaw.ai/pt-BR/reference/test2026-05-01T07:59:57.131Zhttps://docs.openclaw.ai/pt-BR/reference/token-use2026-05-01T07:59:57.130Zhttps://docs.openclaw.ai/pt-BR/reference/transcript-hygiene2026-05-01T07:59:57.147Zhttps://docs.openclaw.ai/pt-BR/reference/wizard2026-05-01T07:59:57.130Zhttps://docs.openclaw.ai/pt-BR/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:57.131Zhttps://docs.openclaw.ai/pt-BR/security/THREAT-MODEL-ATLAS2026-05-01T07:59:57.129Zhttps://docs.openclaw.ai/pt-BR/security/formal-verification2026-05-01T07:59:57.129Zhttps://docs.openclaw.ai/pt-BR/security/network-proxy2026-05-01T07:59:57.184Zhttps://docs.openclaw.ai/pt-BR/start/bootstrapping2026-05-01T07:59:57.179Zhttps://docs.openclaw.ai/pt-BR/start/docs-directory2026-05-01T07:59:57.176Zhttps://docs.openclaw.ai/pt-BR/start/getting-started2026-05-01T07:59:57.183Zhttps://docs.openclaw.ai/pt-BR/start/hubs2026-05-01T07:59:57.168Zhttps://docs.openclaw.ai/pt-BR/start/lore2026-05-01T07:59:57.176Zhttps://docs.openclaw.ai/pt-BR/start/onboarding2026-05-01T07:59:57.181Zhttps://docs.openclaw.ai/pt-BR/start/onboarding-overview2026-05-01T07:59:57.174Zhttps://docs.openclaw.ai/pt-BR/start/openclaw2026-05-01T07:59:57.177Zhttps://docs.openclaw.ai/pt-BR/start/setup2026-05-01T07:59:57.214Zhttps://docs.openclaw.ai/pt-BR/start/showcase2026-04-24T17:33:22.966Zhttps://docs.openclaw.ai/pt-BR/start/wizard2026-05-01T07:59:57.205Zhttps://docs.openclaw.ai/pt-BR/start/wizard-cli-automation2026-05-01T07:59:57.213Zhttps://docs.openclaw.ai/pt-BR/start/wizard-cli-reference2026-05-01T07:59:57.211Zhttps://docs.openclaw.ai/pt-BR/tools/acp-agents2026-05-01T07:59:57.204Zhttps://docs.openclaw.ai/pt-BR/tools/acp-agents-setup2026-05-01T07:59:57.212Zhttps://docs.openclaw.ai/pt-BR/tools/agent-send2026-05-01T07:59:57.206Zhttps://docs.openclaw.ai/pt-BR/tools/apply-patch2026-05-01T07:59:57.205Zhttps://docs.openclaw.ai/pt-BR/tools/brave-search2026-05-01T07:59:57.254Zhttps://docs.openclaw.ai/pt-BR/tools/browser2026-05-01T07:59:57.253Zhttps://docs.openclaw.ai/pt-BR/tools/browser-control2026-05-01T07:59:57.253Zhttps://docs.openclaw.ai/pt-BR/tools/browser-linux-troubleshooting2026-05-01T07:59:57.252Zhttps://docs.openclaw.ai/pt-BR/tools/browser-login2026-05-01T07:59:57.251Zhttps://docs.openclaw.ai/pt-BR/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T07:59:57.240Zhttps://docs.openclaw.ai/pt-BR/tools/btw2026-05-01T07:59:57.235Zhttps://docs.openclaw.ai/pt-BR/tools/clawhub2026-05-01T07:59:57.234Zhttps://docs.openclaw.ai/pt-BR/tools/code-execution2026-05-01T07:59:57.233Zhttps://docs.openclaw.ai/pt-BR/tools/creating-skills2026-05-01T07:59:57.284Zhttps://docs.openclaw.ai/pt-BR/tools/diffs2026-05-01T07:59:57.283Zhttps://docs.openclaw.ai/pt-BR/tools/duckduckgo-search2026-05-01T07:59:57.283Zhttps://docs.openclaw.ai/pt-BR/tools/elevated2026-05-01T07:59:57.275Zhttps://docs.openclaw.ai/pt-BR/tools/exa-search2026-05-01T07:59:57.277Zhttps://docs.openclaw.ai/pt-BR/tools/exec2026-05-01T07:59:57.275Zhttps://docs.openclaw.ai/pt-BR/tools/exec-approvals2026-05-01T07:59:57.276Zhttps://docs.openclaw.ai/pt-BR/tools/exec-approvals-advanced2026-05-01T07:59:57.280Zhttps://docs.openclaw.ai/pt-BR/tools/firecrawl2026-05-01T07:59:57.277Zhttps://docs.openclaw.ai/pt-BR/tools/gemini-search2026-05-01T07:59:57.274Zhttps://docs.openclaw.ai/pt-BR/tools/grok-search2026-05-01T07:59:57.314Zhttps://docs.openclaw.ai/pt-BR/tools/image-generation2026-05-01T07:59:57.312Zhttps://docs.openclaw.ai/pt-BR/tools2026-05-01T07:59:57.313Zhttps://docs.openclaw.ai/pt-BR/tools/kimi-search2026-05-01T07:59:57.309Zhttps://docs.openclaw.ai/pt-BR/tools/llm-task2026-05-01T07:59:57.304Zhttps://docs.openclaw.ai/pt-BR/tools/lobster2026-05-01T07:59:57.311Zhttps://docs.openclaw.ai/pt-BR/tools/loop-detection2026-05-01T07:59:57.304Zhttps://docs.openclaw.ai/pt-BR/tools/media-overview2026-05-01T07:59:57.305Zhttps://docs.openclaw.ai/pt-BR/tools/minimax-search2026-05-01T07:59:57.305Zhttps://docs.openclaw.ai/pt-BR/tools/multi-agent-sandbox-tools2026-05-01T07:59:57.303Zhttps://docs.openclaw.ai/pt-BR/tools/music-generation2026-05-01T07:59:57.354Zhttps://docs.openclaw.ai/pt-BR/tools/ollama-search2026-05-01T07:59:57.353Zhttps://docs.openclaw.ai/pt-BR/tools/pdf2026-05-01T07:59:57.353Zhttps://docs.openclaw.ai/pt-BR/tools/perplexity-search2026-05-01T07:59:57.354Zhttps://docs.openclaw.ai/pt-BR/tools/plugin2026-05-01T07:59:57.340Zhttps://docs.openclaw.ai/pt-BR/tools/reactions2026-05-01T07:59:57.352Zhttps://docs.openclaw.ai/pt-BR/tools/searxng-search2026-05-01T07:59:57.336Zhttps://docs.openclaw.ai/pt-BR/tools/skills2026-05-01T07:59:57.334Zhttps://docs.openclaw.ai/pt-BR/tools/skills-config2026-05-01T07:59:57.335Zhttps://docs.openclaw.ai/pt-BR/tools/slash-commands2026-05-01T07:59:57.334Zhttps://docs.openclaw.ai/pt-BR/tools/subagents2026-05-01T07:59:57.385Zhttps://docs.openclaw.ai/pt-BR/tools/tavily2026-05-01T07:59:57.377Zhttps://docs.openclaw.ai/pt-BR/tools/thinking2026-05-01T07:59:57.384Zhttps://docs.openclaw.ai/pt-BR/tools/tokenjuice2026-05-01T07:59:57.384Zhttps://docs.openclaw.ai/pt-BR/tools/trajectory2026-05-01T07:59:57.378Zhttps://docs.openclaw.ai/pt-BR/tools/tts2026-05-01T07:59:57.381Zhttps://docs.openclaw.ai/pt-BR/tools/video-generation2026-05-01T07:59:57.377Zhttps://docs.openclaw.ai/pt-BR/tools/web2026-05-01T07:59:57.376Zhttps://docs.openclaw.ai/pt-BR/tools/web-fetch2026-05-01T07:59:57.376Zhttps://docs.openclaw.ai/pt-BR/vps2026-05-01T07:59:57.415Zhttps://docs.openclaw.ai/pt-BR/web/control-ui2026-05-01T07:59:57.413Zhttps://docs.openclaw.ai/pt-BR/web/dashboard2026-05-01T07:59:57.413Zhttps://docs.openclaw.ai/pt-BR/web2026-05-01T07:59:57.411Zhttps://docs.openclaw.ai/pt-BR/web/tui2026-05-01T07:59:57.414Zhttps://docs.openclaw.ai/pt-BR/web/webchat2026-05-01T07:59:57.414Zhttps://docs.openclaw.ai/reference/AGENTS.default2026-05-01T07:59:57.405Zhttps://docs.openclaw.ai/reference/RELEASING2026-05-01T07:59:57.406Zhttps://docs.openclaw.ai/reference/api-usage-costs2026-05-01T07:59:57.405Zhttps://docs.openclaw.ai/reference/credits2026-05-01T07:59:57.454Zhttps://docs.openclaw.ai/reference/device-models2026-05-01T07:59:57.437Zhttps://docs.openclaw.ai/reference/full-release-validation2026-05-01T07:59:57.438Zhttps://docs.openclaw.ai/reference/memory-config2026-05-01T07:59:57.456Zhttps://docs.openclaw.ai/reference/openclaw-sdk-api-design2026-05-01T07:59:57.456Zhttps://docs.openclaw.ai/reference/prompt-caching2026-05-01T07:59:57.441Zhttps://docs.openclaw.ai/reference/rich-output-protocol2026-05-01T07:59:57.437Zhttps://docs.openclaw.ai/reference/rpc2026-05-01T07:59:57.438Zhttps://docs.openclaw.ai/reference/secretref-credential-surface2026-05-01T07:59:57.436Zhttps://docs.openclaw.ai/reference/session-management-compaction2026-05-01T12:18:51.531Zhttps://docs.openclaw.ai/reference/templates/AGENTS2026-05-01T07:59:57.485Zhttps://docs.openclaw.ai/reference/templates/BOOT2026-05-01T07:59:57.486Zhttps://docs.openclaw.ai/reference/templates/BOOTSTRAP2026-05-01T07:59:57.478Zhttps://docs.openclaw.ai/reference/templates/HEARTBEAT2026-05-01T07:59:57.477Zhttps://docs.openclaw.ai/reference/templates/IDENTITY2026-05-01T07:59:57.479Zhttps://docs.openclaw.ai/reference/templates/SOUL2026-05-01T07:59:57.481Zhttps://docs.openclaw.ai/reference/templates/TOOLS2026-05-01T07:59:57.524Zhttps://docs.openclaw.ai/reference/templates/USER2026-05-01T07:59:57.517Zhttps://docs.openclaw.ai/reference/test2026-05-01T08:28:03.726Zhttps://docs.openclaw.ai/reference/token-use2026-05-01T07:59:57.516Zhttps://docs.openclaw.ai/reference/transcript-hygiene2026-05-01T07:59:57.506Zhttps://docs.openclaw.ai/reference/wizard2026-05-01T07:59:57.508Zhttps://docs.openclaw.ai/security/CONTRIBUTING-THREAT-MODEL2026-05-01T07:59:57.507Zhttps://docs.openclaw.ai/security/THREAT-MODEL-ATLAS2026-05-01T07:59:57.507Zhttps://docs.openclaw.ai/security/formal-verification2026-05-01T07:59:57.506Zhttps://docs.openclaw.ai/security/network-proxy2026-05-01T07:59:57.572Zhttps://docs.openclaw.ai/start/bootstrapping2026-05-01T07:59:57.556Zhttps://docs.openclaw.ai/start/docs-directory2026-05-01T07:59:57.559Zhttps://docs.openclaw.ai/start/getting-started2026-05-01T07:59:57.549Zhttps://docs.openclaw.ai/start/hubs2026-05-01T07:59:57.560Zhttps://docs.openclaw.ai/start/lore2026-05-01T07:59:57.551Zhttps://docs.openclaw.ai/start/onboarding2026-05-01T07:59:57.551Zhttps://docs.openclaw.ai/start/onboarding-overview2026-05-01T07:59:57.550Zhttps://docs.openclaw.ai/start/openclaw2026-05-01T07:59:57.548Zhttps://docs.openclaw.ai/start/setup2026-05-01T07:59:57.628Zhttps://docs.openclaw.ai/start/showcase2026-04-24T03:49:22.817Zhttps://docs.openclaw.ai/start/wizard2026-05-01T07:59:57.601Zhttps://docs.openclaw.ai/start/wizard-cli-automation2026-05-01T07:59:57.602Zhttps://docs.openclaw.ai/start/wizard-cli-reference2026-05-01T07:59:57.626Zhttps://docs.openclaw.ai/tools/acp-agents2026-05-01T08:00:03.609Zhttps://docs.openclaw.ai/tools/acp-agents-setup2026-05-01T08:00:03.600Zhttps://docs.openclaw.ai/tools/agent-send2026-05-01T08:00:03.605Zhttps://docs.openclaw.ai/tools/apply-patch2026-05-01T08:00:03.600Zhttps://docs.openclaw.ai/tools/brave-search2026-05-01T08:00:03.601Zhttps://docs.openclaw.ai/tools/browser2026-05-01T08:00:03.638Zhttps://docs.openclaw.ai/tools/browser-control2026-05-01T08:00:03.599Zhttps://docs.openclaw.ai/tools/browser-linux-troubleshooting2026-05-01T08:00:03.599Zhttps://docs.openclaw.ai/tools/browser-login2026-05-01T08:00:03.650Zhttps://docs.openclaw.ai/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T08:00:03.639Zhttps://docs.openclaw.ai/tools/btw2026-05-01T08:00:03.638Zhttps://docs.openclaw.ai/tools/clawhub2026-05-01T08:00:03.630Zhttps://docs.openclaw.ai/tools/code-execution2026-05-01T08:00:03.631Zhttps://docs.openclaw.ai/tools/creating-skills2026-05-01T08:00:03.637Zhttps://docs.openclaw.ai/tools/diffs2026-05-01T08:00:03.630Zhttps://docs.openclaw.ai/tools/duckduckgo-search2026-05-01T08:00:03.629Zhttps://docs.openclaw.ai/tools/elevated2026-05-01T08:00:03.679Zhttps://docs.openclaw.ai/tools/exa-search2026-05-01T08:00:03.677Zhttps://docs.openclaw.ai/tools/exec2026-05-01T08:00:03.678Zhttps://docs.openclaw.ai/tools/exec-approvals2026-05-01T08:00:03.670Zhttps://docs.openclaw.ai/tools/exec-approvals-advanced2026-05-01T08:00:03.676Zhttps://docs.openclaw.ai/tools/firecrawl2026-05-01T08:00:03.674Zhttps://docs.openclaw.ai/tools/gemini-search2026-05-01T08:00:03.677Zhttps://docs.openclaw.ai/tools/grok-search2026-05-01T08:00:03.670Zhttps://docs.openclaw.ai/tools/image-generation2026-05-01T08:00:03.669Zhttps://docs.openclaw.ai/tools2026-05-01T08:00:03.669Zhttps://docs.openclaw.ai/tools/kimi-search2026-05-01T08:00:03.708Zhttps://docs.openclaw.ai/tools/llm-task2026-05-01T08:00:03.707Zhttps://docs.openclaw.ai/tools/lobster2026-05-01T08:00:03.707Zhttps://docs.openclaw.ai/tools/loop-detection2026-05-01T08:00:03.700Zhttps://docs.openclaw.ai/tools/media-overview2026-05-01T08:00:03.701Zhttps://docs.openclaw.ai/tools/minimax-search2026-05-01T08:00:03.704Zhttps://docs.openclaw.ai/tools/multi-agent-sandbox-tools2026-05-01T08:00:03.700Zhttps://docs.openclaw.ai/tools/music-generation2026-05-01T08:00:03.701Zhttps://docs.openclaw.ai/tools/ollama-search2026-05-01T08:00:03.699Zhttps://docs.openclaw.ai/tools/pdf2026-05-01T08:00:03.705Zhttps://docs.openclaw.ai/tools/perplexity-search2026-05-01T08:00:03.749Zhttps://docs.openclaw.ai/tools/plugin2026-05-01T10:02:50.278Zhttps://docs.openclaw.ai/tools/reactions2026-05-01T08:00:03.732Zhttps://docs.openclaw.ai/tools/searxng-search2026-05-01T08:00:03.734Zhttps://docs.openclaw.ai/tools/skills2026-05-01T08:00:03.736Zhttps://docs.openclaw.ai/tools/skills-config2026-05-01T08:00:03.728Zhttps://docs.openclaw.ai/tools/slash-commands2026-05-01T10:02:50.284Zhttps://docs.openclaw.ai/tools/subagents2026-05-01T08:00:03.729Zhttps://docs.openclaw.ai/tools/tavily2026-05-01T08:00:03.735Zhttps://docs.openclaw.ai/tools/thinking2026-05-01T08:00:03.728Zhttps://docs.openclaw.ai/tools/tokenjuice2026-05-01T08:00:03.780Zhttps://docs.openclaw.ai/tools/trajectory2026-05-01T08:00:03.778Zhttps://docs.openclaw.ai/tools/tts2026-05-01T08:00:03.775Zhttps://docs.openclaw.ai/tools/video-generation2026-05-01T08:00:03.772Zhttps://docs.openclaw.ai/tools/web2026-05-01T08:00:03.779Zhttps://docs.openclaw.ai/tools/web-fetch2026-05-01T08:00:03.775Zhttps://docs.openclaw.ai/tr/auth-credential-semantics2026-05-01T08:00:03.771Zhttps://docs.openclaw.ai/tr/automation/cron-jobs2026-05-01T08:00:03.810Zhttps://docs.openclaw.ai/tr/automation/hooks2026-05-01T08:00:03.806Zhttps://docs.openclaw.ai/tr/automation2026-05-01T08:00:03.808Zhttps://docs.openclaw.ai/tr/automation/standing-orders2026-05-01T08:00:03.801Zhttps://docs.openclaw.ai/tr/automation/taskflow2026-05-01T08:00:03.802Zhttps://docs.openclaw.ai/tr/automation/tasks2026-05-01T09:08:31.481Zhttps://docs.openclaw.ai/tr/channels/bluebubbles2026-05-01T09:08:31.484Zhttps://docs.openclaw.ai/tr/channels/broadcast-groups2026-05-01T08:00:03.836Zhttps://docs.openclaw.ai/tr/channels/channel-routing2026-05-01T08:00:03.837Zhttps://docs.openclaw.ai/tr/channels/discord2026-05-01T08:00:03.832Zhttps://docs.openclaw.ai/tr/channels/feishu2026-05-01T08:00:03.848Zhttps://docs.openclaw.ai/tr/channels/googlechat2026-05-01T08:00:03.832Zhttps://docs.openclaw.ai/tr/channels/group-messages2026-05-01T08:00:03.831Zhttps://docs.openclaw.ai/tr/channels/groups2026-05-01T09:08:31.479Zhttps://docs.openclaw.ai/tr/channels/imessage2026-05-01T08:00:03.880Zhttps://docs.openclaw.ai/tr/channels2026-05-01T08:00:03.879Zhttps://docs.openclaw.ai/tr/channels/irc2026-05-01T08:00:03.878Zhttps://docs.openclaw.ai/tr/channels/line2026-05-01T08:00:03.875Zhttps://docs.openclaw.ai/tr/channels/location2026-05-01T08:00:03.869Zhttps://docs.openclaw.ai/tr/channels/matrix2026-05-01T08:00:03.870Zhttps://docs.openclaw.ai/tr/channels/matrix-migration2026-05-01T08:00:03.879Zhttps://docs.openclaw.ai/tr/channels/matrix-push-rules2026-05-01T08:00:03.871Zhttps://docs.openclaw.ai/tr/channels/mattermost2026-05-01T08:00:03.877Zhttps://docs.openclaw.ai/tr/channels/msteams2026-05-01T08:00:03.870Zhttps://docs.openclaw.ai/tr/channels/nextcloud-talk2026-05-01T08:00:03.912Zhttps://docs.openclaw.ai/tr/channels/nostr2026-05-01T08:00:03.910Zhttps://docs.openclaw.ai/tr/channels/pairing2026-05-01T08:00:03.903Zhttps://docs.openclaw.ai/tr/channels/qa-channel2026-05-01T09:08:31.477Zhttps://docs.openclaw.ai/tr/channels/qqbot2026-05-01T08:00:03.902Zhttps://docs.openclaw.ai/tr/channels/signal2026-05-01T08:00:03.908Zhttps://docs.openclaw.ai/tr/channels/slack2026-05-01T08:00:03.910Zhttps://docs.openclaw.ai/tr/channels/synology-chat2026-05-01T08:00:03.903Zhttps://docs.openclaw.ai/tr/channels/telegram2026-05-01T08:00:03.902Zhttps://docs.openclaw.ai/tr/channels/tlon2026-05-01T08:00:03.901Zhttps://docs.openclaw.ai/tr/channels/troubleshooting2026-05-01T08:00:03.951Zhttps://docs.openclaw.ai/tr/channels/twitch2026-05-01T08:00:03.953Zhttps://docs.openclaw.ai/tr/channels/wechat2026-05-01T08:00:03.952Zhttps://docs.openclaw.ai/tr/channels/whatsapp2026-05-01T08:00:03.934Zhttps://docs.openclaw.ai/tr/channels/yuanbao2026-05-01T08:00:03.940Zhttps://docs.openclaw.ai/tr/channels/zalo2026-05-01T08:00:03.952Zhttps://docs.openclaw.ai/tr/channels/zalouser2026-05-01T08:00:03.932Zhttps://docs.openclaw.ai/tr/ci2026-05-01T09:08:31.482Zhttps://docs.openclaw.ai/tr/cli/acp2026-05-01T08:00:03.932Zhttps://docs.openclaw.ai/tr/cli/agent2026-05-01T08:00:03.931Zhttps://docs.openclaw.ai/tr/cli/agents2026-05-01T08:00:03.979Zhttps://docs.openclaw.ai/tr/cli/approvals2026-05-01T08:00:03.981Zhttps://docs.openclaw.ai/tr/cli/backup2026-05-01T08:00:03.979Zhttps://docs.openclaw.ai/tr/cli/browser2026-05-01T08:00:03.982Zhttps://docs.openclaw.ai/tr/cli/channels2026-05-01T09:08:31.481Zhttps://docs.openclaw.ai/tr/cli/clawbot2026-05-01T08:00:03.976Zhttps://docs.openclaw.ai/tr/cli/commitments2026-05-01T08:00:03.973Zhttps://docs.openclaw.ai/tr/cli/completion2026-05-01T08:00:03.972Zhttps://docs.openclaw.ai/tr/cli/config2026-05-01T08:00:03.972Zhttps://docs.openclaw.ai/tr/cli/configure2026-05-01T09:08:31.486Zhttps://docs.openclaw.ai/tr/cli/cron2026-05-01T08:00:04.034Zhttps://docs.openclaw.ai/tr/cli/daemon2026-05-01T08:00:04.023Zhttps://docs.openclaw.ai/tr/cli/dashboard2026-05-01T08:00:04.003Zhttps://docs.openclaw.ai/tr/cli/devices2026-05-01T08:00:04.033Zhttps://docs.openclaw.ai/tr/cli/directory2026-05-01T08:00:04.000Zhttps://docs.openclaw.ai/tr/cli/dns2026-05-01T08:00:04.002Zhttps://docs.openclaw.ai/tr/cli/docs2026-05-01T08:00:04.002Zhttps://docs.openclaw.ai/tr/cli/doctor2026-05-01T08:00:04.001Zhttps://docs.openclaw.ai/tr/cli/flows2026-05-01T08:00:04.006Zhttps://docs.openclaw.ai/tr/cli/gateway2026-05-01T09:08:31.483Zhttps://docs.openclaw.ai/tr/cli/health2026-05-01T08:00:04.062Zhttps://docs.openclaw.ai/tr/cli/hooks2026-05-01T08:00:04.062Zhttps://docs.openclaw.ai/tr/cli2026-05-01T08:00:04.053Zhttps://docs.openclaw.ai/tr/cli/infer2026-05-01T08:00:04.063Zhttps://docs.openclaw.ai/tr/cli/logs2026-05-01T08:00:04.064Zhttps://docs.openclaw.ai/tr/cli/mcp2026-05-01T08:00:04.061Zhttps://docs.openclaw.ai/tr/cli/memory2026-05-01T08:00:04.054Zhttps://docs.openclaw.ai/tr/cli/message2026-05-01T08:00:04.053Zhttps://docs.openclaw.ai/tr/cli/migrate2026-05-01T08:00:04.060Zhttps://docs.openclaw.ai/tr/cli/models2026-05-01T09:08:31.478Zhttps://docs.openclaw.ai/tr/cli/node2026-05-01T08:00:04.095Zhttps://docs.openclaw.ai/tr/cli/nodes2026-05-01T08:00:04.092Zhttps://docs.openclaw.ai/tr/cli/onboard2026-05-01T09:08:31.479Zhttps://docs.openclaw.ai/tr/cli/pairing2026-05-01T08:00:04.094Zhttps://docs.openclaw.ai/tr/cli/plugins2026-05-01T09:08:33.563Zhttps://docs.openclaw.ai/tr/cli/proxy2026-05-01T09:08:33.561Zhttps://docs.openclaw.ai/tr/cli/qr2026-05-01T08:00:04.084Zhttps://docs.openclaw.ai/tr/cli/reset2026-05-01T08:00:04.084Zhttps://docs.openclaw.ai/tr/cli/sandbox2026-05-01T08:00:04.083Zhttps://docs.openclaw.ai/tr/cli/secrets2026-05-01T08:00:04.124Zhttps://docs.openclaw.ai/tr/cli/security2026-05-01T08:00:04.123Zhttps://docs.openclaw.ai/tr/cli/sessions2026-05-01T08:00:04.122Zhttps://docs.openclaw.ai/tr/cli/setup2026-05-01T08:00:04.122Zhttps://docs.openclaw.ai/tr/cli/skills2026-05-01T08:00:04.121Zhttps://docs.openclaw.ai/tr/cli/status2026-05-01T08:00:04.119Zhttps://docs.openclaw.ai/tr/cli/system2026-05-01T08:00:04.114Zhttps://docs.openclaw.ai/tr/cli/tasks2026-05-01T08:00:04.115Zhttps://docs.openclaw.ai/tr/cli/tui2026-05-01T08:00:04.115Zhttps://docs.openclaw.ai/tr/cli/uninstall2026-05-01T08:00:04.113Zhttps://docs.openclaw.ai/tr/cli/update2026-05-01T09:08:33.560Zhttps://docs.openclaw.ai/tr/cli/voicecall2026-05-01T09:08:33.553Zhttps://docs.openclaw.ai/tr/cli/webhooks2026-05-01T08:00:04.155Zhttps://docs.openclaw.ai/tr/cli/wiki2026-05-01T08:00:04.155Zhttps://docs.openclaw.ai/tr/concepts/active-memory2026-05-01T08:00:04.154Zhttps://docs.openclaw.ai/tr/concepts/agent2026-05-01T08:00:04.153Zhttps://docs.openclaw.ai/tr/concepts/agent-loop2026-05-01T08:00:04.158Zhttps://docs.openclaw.ai/tr/concepts/agent-runtimes2026-05-01T08:00:04.154Zhttps://docs.openclaw.ai/tr/concepts/agent-workspace2026-05-01T08:00:04.153Zhttps://docs.openclaw.ai/tr/concepts/architecture2026-05-01T08:00:04.158Zhttps://docs.openclaw.ai/tr/concepts/channel-docking2026-05-01T08:00:04.193Zhttps://docs.openclaw.ai/tr/concepts/commitments2026-05-01T09:08:33.552Zhttps://docs.openclaw.ai/tr/concepts/compaction2026-05-01T08:00:04.180Zhttps://docs.openclaw.ai/tr/concepts/context2026-05-01T08:00:04.190Zhttps://docs.openclaw.ai/tr/concepts/context-engine2026-05-01T08:00:04.187Zhttps://docs.openclaw.ai/tr/concepts/delegate-architecture2026-05-01T08:00:04.191Zhttps://docs.openclaw.ai/tr/concepts/dreaming2026-05-01T08:00:04.181Zhttps://docs.openclaw.ai/tr/concepts/experimental-features2026-05-01T08:00:04.189Zhttps://docs.openclaw.ai/tr/concepts/features2026-05-01T08:00:04.181Zhttps://docs.openclaw.ai/tr/concepts/markdown-formatting2026-05-01T08:00:04.191Zhttps://docs.openclaw.ai/tr/concepts/memory2026-05-01T08:00:04.221Zhttps://docs.openclaw.ai/tr/concepts/memory-builtin2026-05-01T08:00:04.235Zhttps://docs.openclaw.ai/tr/concepts/memory-honcho2026-05-01T08:00:04.220Zhttps://docs.openclaw.ai/tr/concepts/memory-qmd2026-05-01T08:00:04.213Zhttps://docs.openclaw.ai/tr/concepts/memory-search2026-05-01T08:00:04.223Zhttps://docs.openclaw.ai/tr/concepts/messages2026-05-01T08:00:04.223Zhttps://docs.openclaw.ai/tr/concepts/model-failover2026-05-01T08:00:04.222Zhttps://docs.openclaw.ai/tr/concepts/model-providers2026-05-01T08:00:04.219Zhttps://docs.openclaw.ai/tr/concepts/models2026-05-01T08:00:04.220Zhttps://docs.openclaw.ai/tr/concepts/multi-agent2026-05-01T08:00:04.212Zhttps://docs.openclaw.ai/tr/concepts/oauth2026-05-01T08:00:04.269Zhttps://docs.openclaw.ai/tr/concepts/openclaw-sdk2026-05-01T09:08:33.560Zhttps://docs.openclaw.ai/tr/concepts/presence2026-05-01T08:00:04.260Zhttps://docs.openclaw.ai/tr/concepts/qa-e2e-automation2026-05-01T08:00:04.266Zhttps://docs.openclaw.ai/tr/concepts/qa-matrix2026-05-01T08:00:04.268Zhttps://docs.openclaw.ai/tr/concepts/queue2026-05-01T08:00:04.261Zhttps://docs.openclaw.ai/tr/concepts/queue-steering2026-05-01T08:00:04.268Zhttps://docs.openclaw.ai/tr/concepts/retry2026-05-01T08:00:04.261Zhttps://docs.openclaw.ai/tr/concepts/session2026-05-01T08:00:04.298Zhttps://docs.openclaw.ai/tr/concepts/session-pruning2026-05-01T08:00:04.262Zhttps://docs.openclaw.ai/tr/concepts/session-tool2026-05-01T08:00:04.259Zhttps://docs.openclaw.ai/tr/concepts/soul2026-05-01T08:00:04.297Zhttps://docs.openclaw.ai/tr/concepts/streaming2026-05-01T08:00:04.296Zhttps://docs.openclaw.ai/tr/concepts/system-prompt2026-05-01T08:00:04.293Zhttps://docs.openclaw.ai/tr/concepts/timezone2026-05-01T08:00:04.295Zhttps://docs.openclaw.ai/tr/concepts/typebox2026-05-01T08:00:04.288Zhttps://docs.openclaw.ai/tr/concepts/typing-indicators2026-05-01T08:00:04.288Zhttps://docs.openclaw.ai/tr/concepts/usage-tracking2026-05-01T08:00:04.289Zhttps://docs.openclaw.ai/tr/date-time2026-05-01T08:00:04.296Zhttps://docs.openclaw.ai/tr/debug/node-issue2026-05-01T08:00:04.287Zhttps://docs.openclaw.ai/tr/diagnostics/flags2026-05-01T08:00:04.337Zhttps://docs.openclaw.ai/tr/gateway/authentication2026-05-01T08:00:04.339Zhttps://docs.openclaw.ai/tr/gateway/background-process2026-05-01T08:00:04.339Zhttps://docs.openclaw.ai/tr/gateway/bonjour2026-05-01T08:00:04.338Zhttps://docs.openclaw.ai/tr/gateway/bridge-protocol2026-05-01T08:00:04.321Zhttps://docs.openclaw.ai/tr/gateway/cli-backends2026-05-01T08:00:04.326Zhttps://docs.openclaw.ai/tr/gateway/config-agents2026-05-01T08:00:04.320Zhttps://docs.openclaw.ai/tr/gateway/config-channels2026-05-01T09:08:33.565Zhttps://docs.openclaw.ai/tr/gateway/config-tools2026-05-01T09:08:33.562Zhttps://docs.openclaw.ai/tr/gateway/configuration2026-05-01T08:00:04.373Zhttps://docs.openclaw.ai/tr/gateway/configuration-examples2026-05-01T08:00:04.319Zhttps://docs.openclaw.ai/tr/gateway/configuration-reference2026-05-01T08:00:04.369Zhttps://docs.openclaw.ai/tr/gateway/diagnostics2026-05-01T08:00:04.360Zhttps://docs.openclaw.ai/tr/gateway/discovery2026-05-01T08:00:04.370Zhttps://docs.openclaw.ai/tr/gateway/doctor2026-05-01T09:08:33.562Zhttps://docs.openclaw.ai/tr/gateway/gateway-lock2026-05-01T08:00:04.377Zhttps://docs.openclaw.ai/tr/gateway/health2026-05-01T08:00:04.360Zhttps://docs.openclaw.ai/tr/gateway/heartbeat2026-05-01T08:00:04.371Zhttps://docs.openclaw.ai/tr/gateway2026-05-01T08:00:04.359Zhttps://docs.openclaw.ai/tr/gateway/local-models2026-05-01T08:00:04.359Zhttps://docs.openclaw.ai/tr/gateway/logging2026-05-01T09:08:33.554Zhttps://docs.openclaw.ai/tr/gateway/multiple-gateways2026-05-01T08:00:04.405Zhttps://docs.openclaw.ai/tr/gateway/network-model2026-05-01T08:00:04.404Zhttps://docs.openclaw.ai/tr/gateway/openai-http-api2026-05-01T08:00:04.402Zhttps://docs.openclaw.ai/tr/gateway/openresponses-http-api2026-05-01T08:00:04.407Zhttps://docs.openclaw.ai/tr/gateway/openshell2026-05-01T08:00:04.406Zhttps://docs.openclaw.ai/tr/gateway/opentelemetry2026-05-01T08:00:04.404Zhttps://docs.openclaw.ai/tr/gateway/pairing2026-05-01T08:00:04.397Zhttps://docs.openclaw.ai/tr/gateway/prometheus2026-05-01T08:00:04.397Zhttps://docs.openclaw.ai/tr/gateway/protocol2026-05-01T09:08:35.538Zhttps://docs.openclaw.ai/tr/gateway/remote2026-05-01T08:00:04.446Zhttps://docs.openclaw.ai/tr/gateway/remote-gateway-readme2026-05-01T08:00:04.445Zhttps://docs.openclaw.ai/tr/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T08:00:04.448Zhttps://docs.openclaw.ai/tr/gateway/sandboxing2026-05-01T08:00:04.428Zhttps://docs.openclaw.ai/tr/gateway/secrets2026-05-01T08:00:04.434Zhttps://docs.openclaw.ai/tr/gateway/secrets-plan-contract2026-05-01T08:00:04.448Zhttps://docs.openclaw.ai/tr/gateway/security/audit-checks2026-05-01T08:00:04.449Zhttps://docs.openclaw.ai/tr/gateway/security2026-04-30T09:55:58.715Zhttps://docs.openclaw.ai/tr/gateway/tailscale2026-05-01T08:00:04.428Zhttps://docs.openclaw.ai/tr/gateway/tools-invoke-http-api2026-05-01T08:00:04.429Zhttps://docs.openclaw.ai/tr/gateway/troubleshooting2026-05-01T09:08:35.535Zhttps://docs.openclaw.ai/tr/gateway/trusted-proxy-auth2026-05-01T08:00:04.479Zhttps://docs.openclaw.ai/tr/help/debugging2026-05-01T08:00:04.468Zhttps://docs.openclaw.ai/tr/help/environment2026-05-01T08:00:04.478Zhttps://docs.openclaw.ai/tr/help/faq2026-05-01T08:00:04.478Zhttps://docs.openclaw.ai/tr/help/faq-first-run2026-05-01T08:00:04.480Zhttps://docs.openclaw.ai/tr/help/faq-models2026-05-01T08:00:04.480Zhttps://docs.openclaw.ai/tr/help/gpt55-codex-agentic-parity2026-05-01T08:00:04.469Zhttps://docs.openclaw.ai/tr/help/gpt55-codex-agentic-parity-maintainers2026-05-01T08:00:04.468Zhttps://docs.openclaw.ai/tr/help2026-05-01T08:00:04.475Zhttps://docs.openclaw.ai/tr/help/scripts2026-05-01T08:00:04.467Zhttps://docs.openclaw.ai/tr/help/testing2026-05-01T09:08:35.531Zhttps://docs.openclaw.ai/tr/help/testing-live2026-05-01T08:00:04.512Zhttps://docs.openclaw.ai/tr/help/troubleshooting2026-05-01T08:00:04.510Zhttps://docs.openclaw.ai/tr2026-05-01T08:00:04.502Zhttps://docs.openclaw.ai/tr/install/ansible2026-05-01T08:00:04.510Zhttps://docs.openclaw.ai/tr/install/azure2026-05-01T08:00:04.507Zhttps://docs.openclaw.ai/tr/install/bun2026-05-01T08:00:04.501Zhttps://docs.openclaw.ai/tr/install/clawdock2026-05-01T08:00:04.501Zhttps://docs.openclaw.ai/tr/install/development-channels2026-05-01T08:00:04.502Zhttps://docs.openclaw.ai/tr/install/digitalocean2026-05-01T08:00:04.500Zhttps://docs.openclaw.ai/tr/install/docker2026-05-01T08:00:04.551Zhttps://docs.openclaw.ai/tr/install/docker-vm-runtime2026-05-01T08:00:04.548Zhttps://docs.openclaw.ai/tr/install/exe-dev2026-05-01T08:00:04.551Zhttps://docs.openclaw.ai/tr/install/fly2026-05-01T08:00:04.552Zhttps://docs.openclaw.ai/tr/install/gcp2026-05-01T08:00:04.538Zhttps://docs.openclaw.ai/tr/install/hetzner2026-05-01T08:00:04.532Zhttps://docs.openclaw.ai/tr/install/hostinger2026-05-01T08:00:04.550Zhttps://docs.openclaw.ai/tr/install2026-05-01T08:00:04.532Zhttps://docs.openclaw.ai/tr/install/installer2026-05-01T08:00:04.531Zhttps://docs.openclaw.ai/tr/install/kubernetes2026-05-01T08:00:04.531Zhttps://docs.openclaw.ai/tr/install/macos-vm2026-05-01T08:00:04.581Zhttps://docs.openclaw.ai/tr/install/migrating2026-05-01T08:00:04.574Zhttps://docs.openclaw.ai/tr/install/migrating-claude2026-05-01T08:00:04.573Zhttps://docs.openclaw.ai/tr/install/migrating-hermes2026-05-01T08:00:04.582Zhttps://docs.openclaw.ai/tr/install/nix2026-05-01T08:00:04.574Zhttps://docs.openclaw.ai/tr/install/node2026-05-01T08:00:04.573Zhttps://docs.openclaw.ai/tr/install/northflank2026-05-01T08:00:04.575Zhttps://docs.openclaw.ai/tr/install/oracle2026-05-01T08:00:04.577Zhttps://docs.openclaw.ai/tr/install/podman2026-05-01T08:00:04.572Zhttps://docs.openclaw.ai/tr/install/railway2026-05-01T08:00:04.576Zhttps://docs.openclaw.ai/tr/install/raspberry-pi2026-05-01T08:00:04.609Zhttps://docs.openclaw.ai/tr/install/render2026-05-01T08:00:04.607Zhttps://docs.openclaw.ai/tr/install/uninstall2026-05-01T08:00:04.610Zhttps://docs.openclaw.ai/tr/install/updating2026-05-01T09:08:35.525Zhttps://docs.openclaw.ai/tr/logging2026-05-01T09:08:35.526Zhttps://docs.openclaw.ai/tr/network2026-05-01T08:00:04.602Zhttps://docs.openclaw.ai/tr/nodes/audio2026-05-01T08:00:04.609Zhttps://docs.openclaw.ai/tr/nodes/camera2026-05-01T08:00:04.603Zhttps://docs.openclaw.ai/tr/nodes/images2026-05-01T08:00:04.602Zhttps://docs.openclaw.ai/tr/nodes2026-05-01T08:00:04.601Zhttps://docs.openclaw.ai/tr/nodes/location-command2026-05-01T08:00:04.639Zhttps://docs.openclaw.ai/tr/nodes/media-understanding2026-05-01T08:00:04.638Zhttps://docs.openclaw.ai/tr/nodes/talk2026-05-01T08:00:04.640Zhttps://docs.openclaw.ai/tr/nodes/troubleshooting2026-05-01T08:00:04.631Zhttps://docs.openclaw.ai/tr/nodes/voicewake2026-05-01T08:00:04.651Zhttps://docs.openclaw.ai/tr/pi2026-05-01T08:00:04.638Zhttps://docs.openclaw.ai/tr/pi-dev2026-05-01T08:00:04.631Zhttps://docs.openclaw.ai/tr/platforms/android2026-05-01T08:00:04.686Zhttps://docs.openclaw.ai/tr/platforms2026-05-01T08:00:04.678Zhttps://docs.openclaw.ai/tr/platforms/ios2026-05-01T08:00:04.678Zhttps://docs.openclaw.ai/tr/platforms/linux2026-05-01T08:00:04.682Zhttps://docs.openclaw.ai/tr/platforms/mac/bundled-gateway2026-05-01T08:00:04.683Zhttps://docs.openclaw.ai/tr/platforms/mac/canvas2026-05-01T08:00:04.685Zhttps://docs.openclaw.ai/tr/platforms/mac/child-process2026-05-01T08:00:04.679Zhttps://docs.openclaw.ai/tr/platforms/mac/dev-setup2026-05-01T08:00:04.679Zhttps://docs.openclaw.ai/tr/platforms/mac/health2026-05-01T08:00:04.677Zhttps://docs.openclaw.ai/tr/platforms/mac/icon2026-05-01T08:00:04.730Zhttps://docs.openclaw.ai/tr/platforms/mac/logging2026-05-01T08:00:04.705Zhttps://docs.openclaw.ai/tr/platforms/mac/menu-bar2026-05-01T09:08:35.526Zhttps://docs.openclaw.ai/tr/platforms/mac/peekaboo2026-05-01T08:00:04.729Zhttps://docs.openclaw.ai/tr/platforms/mac/permissions2026-05-01T08:00:04.714Zhttps://docs.openclaw.ai/tr/platforms/mac/remote2026-05-01T08:00:04.713Zhttps://docs.openclaw.ai/tr/platforms/mac/signing2026-05-01T08:00:04.706Zhttps://docs.openclaw.ai/tr/platforms/mac/skills2026-05-01T08:00:04.713Zhttps://docs.openclaw.ai/tr/platforms/mac/voice-overlay2026-05-01T08:00:04.706Zhttps://docs.openclaw.ai/tr/platforms/mac/voicewake2026-05-01T08:00:04.705Zhttps://docs.openclaw.ai/tr/platforms/mac/webchat2026-05-01T08:00:04.764Zhttps://docs.openclaw.ai/tr/platforms/mac/xpc2026-05-01T08:00:04.765Zhttps://docs.openclaw.ai/tr/platforms/macos2026-05-01T08:00:04.765Zhttps://docs.openclaw.ai/tr/platforms/windows2026-05-01T08:00:04.762Zhttps://docs.openclaw.ai/tr/plugins/architecture2026-05-01T08:00:04.757Zhttps://docs.openclaw.ai/tr/plugins/architecture-internals2026-05-01T08:00:04.756Zhttps://docs.openclaw.ai/tr/plugins/building-plugins2026-05-01T08:00:04.798Zhttps://docs.openclaw.ai/tr/plugins/bundles2026-05-01T08:00:04.797Zhttps://docs.openclaw.ai/tr/plugins/codex-computer-use2026-05-01T08:00:04.797Zhttps://docs.openclaw.ai/tr/plugins/codex-harness2026-05-01T09:08:35.529Zhttps://docs.openclaw.ai/tr/plugins/community2026-05-01T08:00:04.789Zhttps://docs.openclaw.ai/tr/plugins/compatibility2026-05-01T08:00:04.787Zhttps://docs.openclaw.ai/tr/plugins/dependency-resolution2026-05-01T09:08:35.524Zhttps://docs.openclaw.ai/tr/plugins/google-meet2026-05-01T09:08:35.528Zhttps://docs.openclaw.ai/tr/plugins/hooks2026-05-01T08:00:04.788Zhttps://docs.openclaw.ai/tr/plugins/manifest2026-05-01T08:00:04.789Zhttps://docs.openclaw.ai/tr/plugins/memory-lancedb2026-05-01T08:00:04.795Zhttps://docs.openclaw.ai/tr/plugins/memory-wiki2026-05-01T08:00:04.842Zhttps://docs.openclaw.ai/tr/plugins/message-presentation2026-05-01T08:00:04.841Zhttps://docs.openclaw.ai/tr/plugins/sdk-agent-harness2026-05-01T08:00:04.841Zhttps://docs.openclaw.ai/tr/plugins/sdk-channel-plugins2026-05-01T08:00:04.825Zhttps://docs.openclaw.ai/tr/plugins/sdk-channel-turn2026-05-01T08:00:04.828Zhttps://docs.openclaw.ai/tr/plugins/sdk-entrypoints2026-05-01T08:00:04.824Zhttps://docs.openclaw.ai/tr/plugins/sdk-migration2026-05-01T08:00:04.823Zhttps://docs.openclaw.ai/tr/plugins/sdk-overview2026-05-01T08:00:04.824Zhttps://docs.openclaw.ai/tr/plugins/sdk-provider-plugins2026-05-01T09:08:35.527Zhttps://docs.openclaw.ai/tr/plugins/sdk-runtime2026-05-01T08:00:04.822Zhttps://docs.openclaw.ai/tr/plugins/sdk-setup2026-05-01T08:00:04.874Zhttps://docs.openclaw.ai/tr/plugins/sdk-subpaths2026-05-01T08:00:04.873Zhttps://docs.openclaw.ai/tr/plugins/sdk-testing2026-05-01T08:00:04.873Zhttps://docs.openclaw.ai/tr/plugins/skill-workshop2026-05-01T08:00:04.863Zhttps://docs.openclaw.ai/tr/plugins/voice-call2026-05-01T09:08:38.261Zhttps://docs.openclaw.ai/tr/plugins/webhooks2026-05-01T08:00:04.872Zhttps://docs.openclaw.ai/tr/plugins/zalouser2026-05-01T08:00:04.865Zhttps://docs.openclaw.ai/tr/prose2026-05-01T08:00:04.864Zhttps://docs.openclaw.ai/tr/providers/alibaba2026-05-01T08:00:04.863Zhttps://docs.openclaw.ai/tr/providers/anthropic2026-05-01T08:00:04.871Zhttps://docs.openclaw.ai/tr/providers/arcee2026-05-01T08:00:04.904Zhttps://docs.openclaw.ai/tr/providers/azure-speech2026-05-01T08:00:04.903Zhttps://docs.openclaw.ai/tr/providers/bedrock2026-05-01T08:00:04.902Zhttps://docs.openclaw.ai/tr/providers/bedrock-mantle2026-05-01T08:00:04.903Zhttps://docs.openclaw.ai/tr/providers/chutes2026-05-01T08:00:04.894Zhttps://docs.openclaw.ai/tr/providers/claude-max-api-proxy2026-05-01T08:00:04.902Zhttps://docs.openclaw.ai/tr/providers/cloudflare-ai-gateway2026-05-01T08:00:04.893Zhttps://docs.openclaw.ai/tr/providers/comfy2026-05-01T08:00:04.895Zhttps://docs.openclaw.ai/tr/providers/deepgram2026-05-01T08:00:04.893Zhttps://docs.openclaw.ai/tr/providers/deepinfra2026-05-01T08:00:04.944Zhttps://docs.openclaw.ai/tr/providers/deepseek2026-05-01T08:00:04.943Zhttps://docs.openclaw.ai/tr/providers/elevenlabs2026-05-01T08:00:04.943Zhttps://docs.openclaw.ai/tr/providers/fal2026-05-01T08:00:04.928Zhttps://docs.openclaw.ai/tr/providers/fireworks2026-05-01T08:00:04.927Zhttps://docs.openclaw.ai/tr/providers/github-copilot2026-05-01T08:00:04.931Zhttps://docs.openclaw.ai/tr/providers/glm2026-05-01T08:00:04.926Zhttps://docs.openclaw.ai/tr/providers/google2026-05-01T08:00:04.926Zhttps://docs.openclaw.ai/tr/providers/gradium2026-05-01T08:00:04.927Zhttps://docs.openclaw.ai/tr/providers/groq2026-05-01T08:00:04.925Zhttps://docs.openclaw.ai/tr/providers/huggingface2026-05-01T08:00:04.973Zhttps://docs.openclaw.ai/tr/providers2026-05-01T08:00:04.972Zhttps://docs.openclaw.ai/tr/providers/inferrs2026-05-01T08:00:04.973Zhttps://docs.openclaw.ai/tr/providers/inworld2026-05-01T08:00:04.968Zhttps://docs.openclaw.ai/tr/providers/kilocode2026-05-01T08:00:04.967Zhttps://docs.openclaw.ai/tr/providers/litellm2026-05-01T08:00:04.971Zhttps://docs.openclaw.ai/tr/providers/lmstudio2026-05-01T08:00:04.964Zhttps://docs.openclaw.ai/tr/providers/minimax2026-05-01T08:00:04.964Zhttps://docs.openclaw.ai/tr/providers/mistral2026-05-01T08:00:04.965Zhttps://docs.openclaw.ai/tr/providers/models2026-05-01T08:00:04.963Zhttps://docs.openclaw.ai/tr/providers/moonshot2026-05-01T08:00:05.008Zhttps://docs.openclaw.ai/tr/providers/nvidia2026-05-01T08:00:05.003Zhttps://docs.openclaw.ai/tr/providers/ollama2026-05-01T08:00:05.004Zhttps://docs.openclaw.ai/tr/providers/openai2026-05-01T08:00:04.996Zhttps://docs.openclaw.ai/tr/providers/opencode2026-05-01T08:00:05.003Zhttps://docs.openclaw.ai/tr/providers/opencode-go2026-05-01T08:00:05.000Zhttps://docs.openclaw.ai/tr/providers/openrouter2026-05-01T08:00:04.994Zhttps://docs.openclaw.ai/tr/providers/perplexity-provider2026-05-01T08:00:04.995Zhttps://docs.openclaw.ai/tr/providers/qianfan2026-05-01T08:00:04.995Zhttps://docs.openclaw.ai/tr/providers/qwen2026-05-01T08:00:04.994Zhttps://docs.openclaw.ai/tr/providers/runway2026-05-01T08:00:05.047Zhttps://docs.openclaw.ai/tr/providers/sglang2026-05-01T08:00:05.046Zhttps://docs.openclaw.ai/tr/providers/stepfun2026-05-01T08:00:05.045Zhttps://docs.openclaw.ai/tr/providers/synthetic2026-05-01T08:00:05.033Zhttps://docs.openclaw.ai/tr/providers/tencent2026-05-01T08:00:05.045Zhttps://docs.openclaw.ai/tr/providers/together2026-05-01T08:00:05.028Zhttps://docs.openclaw.ai/tr/providers/venice2026-05-01T08:00:05.028Zhttps://docs.openclaw.ai/tr/providers/vercel-ai-gateway2026-05-01T08:00:05.027Zhttps://docs.openclaw.ai/tr/providers/vllm2026-05-01T08:00:05.026Zhttps://docs.openclaw.ai/tr/providers/volcengine2026-05-01T08:00:05.077Zhttps://docs.openclaw.ai/tr/providers/vydra2026-05-01T08:00:05.075Zhttps://docs.openclaw.ai/tr/providers/xai2026-05-01T08:00:05.076Zhttps://docs.openclaw.ai/tr/providers/xiaomi2026-05-01T08:00:05.075Zhttps://docs.openclaw.ai/tr/providers/zai2026-05-01T08:00:05.076Zhttps://docs.openclaw.ai/tr/reference/AGENTS.default2026-05-01T08:00:05.073Zhttps://docs.openclaw.ai/tr/reference/RELEASING2026-05-01T09:08:38.259Zhttps://docs.openclaw.ai/tr/reference/api-usage-costs2026-05-01T08:00:05.068Zhttps://docs.openclaw.ai/tr/reference/credits2026-05-01T08:00:05.067Zhttps://docs.openclaw.ai/tr/reference/device-models2026-05-01T08:00:05.107Zhttps://docs.openclaw.ai/tr/reference/full-release-validation2026-05-01T09:08:38.248Zhttps://docs.openclaw.ai/tr/reference/memory-config2026-05-01T08:00:05.105Zhttps://docs.openclaw.ai/tr/reference/openclaw-sdk-api-design2026-05-01T08:00:05.106Zhttps://docs.openclaw.ai/tr/reference/prompt-caching2026-05-01T08:00:05.107Zhttps://docs.openclaw.ai/tr/reference/rich-output-protocol2026-05-01T08:00:05.105Zhttps://docs.openclaw.ai/tr/reference/rpc2026-05-01T08:00:05.103Zhttps://docs.openclaw.ai/tr/reference/secretref-credential-surface2026-05-01T09:08:38.249Zhttps://docs.openclaw.ai/tr/reference/session-management-compaction2026-05-01T08:00:05.097Zhttps://docs.openclaw.ai/tr/reference/templates/AGENTS2026-05-01T08:00:05.096Zhttps://docs.openclaw.ai/tr/reference/templates/BOOT2026-05-01T08:00:05.144Zhttps://docs.openclaw.ai/tr/reference/templates/BOOTSTRAP2026-05-01T08:00:05.146Zhttps://docs.openclaw.ai/tr/reference/templates/HEARTBEAT2026-05-01T08:00:05.130Zhttps://docs.openclaw.ai/tr/reference/templates/IDENTITY2026-05-01T08:00:05.133Zhttps://docs.openclaw.ai/tr/reference/templates/SOUL2026-05-01T08:00:05.129Zhttps://docs.openclaw.ai/tr/reference/templates/TOOLS2026-05-01T08:00:05.128Zhttps://docs.openclaw.ai/tr/reference/templates/USER2026-05-01T08:00:05.175Zhttps://docs.openclaw.ai/tr/reference/test2026-05-01T09:08:38.250Zhttps://docs.openclaw.ai/tr/reference/token-use2026-05-01T08:00:05.174Zhttps://docs.openclaw.ai/tr/reference/transcript-hygiene2026-05-01T08:00:05.168Zhttps://docs.openclaw.ai/tr/reference/wizard2026-05-01T08:00:05.168Zhttps://docs.openclaw.ai/tr/security/CONTRIBUTING-THREAT-MODEL2026-05-01T08:00:05.167Zhttps://docs.openclaw.ai/tr/security/THREAT-MODEL-ATLAS2026-05-01T08:00:05.167Zhttps://docs.openclaw.ai/tr/security/formal-verification2026-05-01T08:00:05.172Zhttps://docs.openclaw.ai/tr/security/network-proxy2026-05-01T09:08:38.256Zhttps://docs.openclaw.ai/tr/start/bootstrapping2026-05-01T08:00:05.205Zhttps://docs.openclaw.ai/tr/start/docs-directory2026-05-01T08:00:05.204Zhttps://docs.openclaw.ai/tr/start/getting-started2026-05-01T08:00:05.203Zhttps://docs.openclaw.ai/tr/start/hubs2026-05-01T08:00:05.203Zhttps://docs.openclaw.ai/tr/start/lore2026-05-01T08:00:05.201Zhttps://docs.openclaw.ai/tr/start/onboarding2026-05-01T08:00:05.196Zhttps://docs.openclaw.ai/tr/start/onboarding-overview2026-05-01T08:00:05.204Zhttps://docs.openclaw.ai/tr/start/openclaw2026-05-01T08:00:05.196Zhttps://docs.openclaw.ai/tr/start/setup2026-05-01T08:00:05.195Zhttps://docs.openclaw.ai/tr/start/showcase2026-04-24T17:33:27.262Zhttps://docs.openclaw.ai/tr/start/wizard2026-05-01T08:00:05.246Zhttps://docs.openclaw.ai/tr/start/wizard-cli-automation2026-05-01T08:00:05.245Zhttps://docs.openclaw.ai/tr/start/wizard-cli-reference2026-05-01T08:00:05.228Zhttps://docs.openclaw.ai/tr/tools/acp-agents2026-05-01T09:08:38.252Zhttps://docs.openclaw.ai/tr/tools/acp-agents-setup2026-05-01T08:00:05.226Zhttps://docs.openclaw.ai/tr/tools/agent-send2026-05-01T08:00:05.227Zhttps://docs.openclaw.ai/tr/tools/apply-patch2026-05-01T08:00:05.227Zhttps://docs.openclaw.ai/tr/tools/brave-search2026-05-01T08:00:05.231Zhttps://docs.openclaw.ai/tr/tools/browser2026-05-01T08:00:05.267Zhttps://docs.openclaw.ai/tr/tools/browser-control2026-05-01T08:00:05.275Zhttps://docs.openclaw.ai/tr/tools/browser-linux-troubleshooting2026-05-01T08:00:05.275Zhttps://docs.openclaw.ai/tr/tools/browser-login2026-05-01T08:00:05.268Zhttps://docs.openclaw.ai/tr/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T08:00:05.267Zhttps://docs.openclaw.ai/tr/tools/btw2026-05-01T08:00:05.274Zhttps://docs.openclaw.ai/tr/tools/clawhub2026-05-01T08:00:05.276Zhttps://docs.openclaw.ai/tr/tools/code-execution2026-05-01T08:00:05.271Zhttps://docs.openclaw.ai/tr/tools/creating-skills2026-05-01T08:00:05.266Zhttps://docs.openclaw.ai/tr/tools/diffs2026-05-01T08:00:05.305Zhttps://docs.openclaw.ai/tr/tools/duckduckgo-search2026-05-01T08:00:05.304Zhttps://docs.openclaw.ai/tr/tools/elevated2026-05-01T08:00:05.305Zhttps://docs.openclaw.ai/tr/tools/exa-search2026-05-01T08:00:05.304Zhttps://docs.openclaw.ai/tr/tools/exec2026-05-01T08:00:05.297Zhttps://docs.openclaw.ai/tr/tools/exec-approvals2026-05-01T08:00:05.298Zhttps://docs.openclaw.ai/tr/tools/exec-approvals-advanced2026-05-01T08:00:05.302Zhttps://docs.openclaw.ai/tr/tools/firecrawl2026-05-01T08:00:05.297Zhttps://docs.openclaw.ai/tr/tools/gemini-search2026-05-01T08:00:05.296Zhttps://docs.openclaw.ai/tr/tools/grok-search2026-05-01T08:00:05.296Zhttps://docs.openclaw.ai/tr/tools/image-generation2026-05-01T08:00:05.351Zhttps://docs.openclaw.ai/tr/tools2026-05-01T08:00:05.346Zhttps://docs.openclaw.ai/tr/tools/kimi-search2026-05-01T08:00:05.346Zhttps://docs.openclaw.ai/tr/tools/llm-task2026-05-01T08:00:05.345Zhttps://docs.openclaw.ai/tr/tools/lobster2026-05-01T08:00:05.325Zhttps://docs.openclaw.ai/tr/tools/loop-detection2026-05-01T08:00:05.327Zhttps://docs.openclaw.ai/tr/tools/media-overview2026-05-01T08:00:05.328Zhttps://docs.openclaw.ai/tr/tools/minimax-search2026-05-01T08:00:05.327Zhttps://docs.openclaw.ai/tr/tools/multi-agent-sandbox-tools2026-05-01T08:00:05.326Zhttps://docs.openclaw.ai/tr/tools/music-generation2026-05-01T08:00:05.332Zhttps://docs.openclaw.ai/tr/tools/ollama-search2026-05-01T08:00:05.390Zhttps://docs.openclaw.ai/tr/tools/pdf2026-05-01T08:00:05.389Zhttps://docs.openclaw.ai/tr/tools/perplexity-search2026-05-01T08:00:05.390Zhttps://docs.openclaw.ai/tr/tools/plugin2026-05-01T09:08:38.254Zhttps://docs.openclaw.ai/tr/tools/reactions2026-05-01T08:00:05.381Zhttps://docs.openclaw.ai/tr/tools/searxng-search2026-05-01T08:00:05.380Zhttps://docs.openclaw.ai/tr/tools/skills2026-05-01T08:00:05.379Zhttps://docs.openclaw.ai/tr/tools/skills-config2026-05-01T08:00:05.381Zhttps://docs.openclaw.ai/tr/tools/slash-commands2026-05-01T08:00:05.378Zhttps://docs.openclaw.ai/tr/tools/subagents2026-05-01T08:00:05.378Zhttps://docs.openclaw.ai/tr/tools/tavily2026-05-01T08:00:05.460Zhttps://docs.openclaw.ai/tr/tools/thinking2026-05-01T08:00:05.426Zhttps://docs.openclaw.ai/tr/tools/tokenjuice2026-05-01T08:00:05.459Zhttps://docs.openclaw.ai/tr/tools/trajectory2026-05-01T08:00:05.409Zhttps://docs.openclaw.ai/tr/tools/tts2026-05-01T08:00:05.421Zhttps://docs.openclaw.ai/tr/tools/video-generation2026-05-01T08:00:05.422Zhttps://docs.openclaw.ai/tr/tools/web2026-05-01T08:00:05.420Zhttps://docs.openclaw.ai/tr/tools/web-fetch2026-05-01T08:00:05.422Zhttps://docs.openclaw.ai/tr/vps2026-05-01T08:00:05.427Zhttps://docs.openclaw.ai/tr/web/control-ui2026-05-01T08:00:05.493Zhttps://docs.openclaw.ai/tr/web/dashboard2026-05-01T08:00:05.492Zhttps://docs.openclaw.ai/tr/web2026-05-01T08:00:05.487Zhttps://docs.openclaw.ai/tr/web/tui2026-05-01T08:00:05.491Zhttps://docs.openclaw.ai/tr/web/webchat2026-05-01T08:00:05.483Zhttps://docs.openclaw.ai/uk/auth-credential-semantics2026-05-01T08:00:05.483Zhttps://docs.openclaw.ai/uk/automation/cron-jobs2026-05-01T08:00:05.523Zhttps://docs.openclaw.ai/uk/automation/hooks2026-05-01T08:00:05.519Zhttps://docs.openclaw.ai/uk/automation2026-05-01T08:00:05.515Zhttps://docs.openclaw.ai/uk/automation/standing-orders2026-05-01T08:00:05.515Zhttps://docs.openclaw.ai/uk/automation/taskflow2026-05-01T08:00:05.514Zhttps://docs.openclaw.ai/uk/automation/tasks2026-05-01T08:00:05.514Zhttps://docs.openclaw.ai/uk/channels/bluebubbles2026-05-01T08:00:05.566Zhttps://docs.openclaw.ai/uk/channels/broadcast-groups2026-05-01T08:00:05.547Zhttps://docs.openclaw.ai/uk/channels/channel-routing2026-05-01T08:00:05.548Zhttps://docs.openclaw.ai/uk/channels/discord2026-05-01T11:58:47.476Zhttps://docs.openclaw.ai/uk/channels/feishu2026-05-01T08:00:05.545Zhttps://docs.openclaw.ai/uk/channels/googlechat2026-05-01T08:00:05.545Zhttps://docs.openclaw.ai/uk/channels/group-messages2026-05-01T08:00:05.544Zhttps://docs.openclaw.ai/uk/channels/groups2026-05-01T08:00:05.553Zhttps://docs.openclaw.ai/uk/channels/imessage2026-05-01T08:00:05.599Zhttps://docs.openclaw.ai/uk/channels2026-05-01T08:00:05.598Zhttps://docs.openclaw.ai/uk/channels/irc2026-05-01T08:00:05.598Zhttps://docs.openclaw.ai/uk/channels/line2026-05-01T08:00:05.592Zhttps://docs.openclaw.ai/uk/channels/location2026-05-01T08:00:05.592Zhttps://docs.openclaw.ai/uk/channels/matrix2026-05-01T08:00:05.590Zhttps://docs.openclaw.ai/uk/channels/matrix-migration2026-05-01T08:00:05.596Zhttps://docs.openclaw.ai/uk/channels/matrix-push-rules2026-05-01T08:00:05.591Zhttps://docs.openclaw.ai/uk/channels/mattermost2026-05-01T15:17:17.985Zhttps://docs.openclaw.ai/uk/channels/msteams2026-05-01T08:00:05.589Zhttps://docs.openclaw.ai/uk/channels/nextcloud-talk2026-05-01T08:00:05.630Zhttps://docs.openclaw.ai/uk/channels/nostr2026-05-01T08:00:05.629Zhttps://docs.openclaw.ai/uk/channels/pairing2026-05-01T08:00:05.629Zhttps://docs.openclaw.ai/uk/channels/qa-channel2026-05-01T08:00:05.623Zhttps://docs.openclaw.ai/uk/channels/qqbot2026-05-01T08:00:05.623Zhttps://docs.openclaw.ai/uk/channels/signal2026-05-01T08:00:05.627Zhttps://docs.openclaw.ai/uk/channels/slack2026-05-01T13:28:00.526Zhttps://docs.openclaw.ai/uk/channels/synology-chat2026-05-01T08:00:05.621Zhttps://docs.openclaw.ai/uk/channels/telegram2026-05-01T08:00:05.622Zhttps://docs.openclaw.ai/uk/channels/tlon2026-05-01T08:00:05.621Zhttps://docs.openclaw.ai/uk/channels/troubleshooting2026-05-01T08:00:05.674Zhttps://docs.openclaw.ai/uk/channels/twitch2026-05-01T08:00:05.660Zhttps://docs.openclaw.ai/uk/channels/wechat2026-05-01T08:00:05.658Zhttps://docs.openclaw.ai/uk/channels/whatsapp2026-05-01T08:00:05.648Zhttps://docs.openclaw.ai/uk/channels/yuanbao2026-05-01T08:00:05.675Zhttps://docs.openclaw.ai/uk/channels/zalo2026-05-01T08:00:05.659Zhttps://docs.openclaw.ai/uk/channels/zalouser2026-05-01T08:00:05.660Zhttps://docs.openclaw.ai/uk/ci2026-05-01T08:56:42.145Zhttps://docs.openclaw.ai/uk/cli/acp2026-05-01T08:00:05.650Zhttps://docs.openclaw.ai/uk/cli/agent2026-05-01T08:00:05.649Zhttps://docs.openclaw.ai/uk/cli/agents2026-05-01T08:00:05.694Zhttps://docs.openclaw.ai/uk/cli/approvals2026-05-01T08:00:05.702Zhttps://docs.openclaw.ai/uk/cli/backup2026-05-01T08:00:05.705Zhttps://docs.openclaw.ai/uk/cli/browser2026-05-01T08:00:05.701Zhttps://docs.openclaw.ai/uk/cli/channels2026-05-01T08:03:30.630Zhttps://docs.openclaw.ai/uk/cli/clawbot2026-05-01T08:00:05.703Zhttps://docs.openclaw.ai/uk/cli/commitments2026-05-01T08:00:05.703Zhttps://docs.openclaw.ai/uk/cli/completion2026-05-01T08:00:05.695Zhttps://docs.openclaw.ai/uk/cli/config2026-05-01T08:00:05.695Zhttps://docs.openclaw.ai/uk/cli/configure2026-05-01T08:03:30.634Zhttps://docs.openclaw.ai/uk/cli/cron2026-05-01T08:00:05.733Zhttps://docs.openclaw.ai/uk/cli/daemon2026-05-01T08:00:05.731Zhttps://docs.openclaw.ai/uk/cli/dashboard2026-05-01T08:00:05.732Zhttps://docs.openclaw.ai/uk/cli/devices2026-05-01T08:00:05.725Zhttps://docs.openclaw.ai/uk/cli/directory2026-05-01T08:00:05.729Zhttps://docs.openclaw.ai/uk/cli/dns2026-05-01T08:00:05.724Zhttps://docs.openclaw.ai/uk/cli/docs2026-05-01T08:00:05.723Zhttps://docs.openclaw.ai/uk/cli/doctor2026-05-01T15:29:15.214Zhttps://docs.openclaw.ai/uk/cli/flows2026-05-01T08:00:05.732Zhttps://docs.openclaw.ai/uk/cli/gateway2026-05-01T08:03:30.639Zhttps://docs.openclaw.ai/uk/cli/health2026-05-01T08:00:05.769Zhttps://docs.openclaw.ai/uk/cli/hooks2026-05-01T08:00:05.768Zhttps://docs.openclaw.ai/uk/cli2026-05-01T08:00:05.762Zhttps://docs.openclaw.ai/uk/cli/infer2026-05-01T08:00:05.765Zhttps://docs.openclaw.ai/uk/cli/logs2026-05-01T08:00:05.765Zhttps://docs.openclaw.ai/uk/cli/mcp2026-05-01T08:00:05.762Zhttps://docs.openclaw.ai/uk/cli/memory2026-05-01T08:00:05.761Zhttps://docs.openclaw.ai/uk/cli/message2026-05-01T08:00:05.761Zhttps://docs.openclaw.ai/uk/cli/migrate2026-05-01T08:00:05.760Zhttps://docs.openclaw.ai/uk/cli/models2026-05-01T08:00:05.810Zhttps://docs.openclaw.ai/uk/cli/node2026-05-01T08:00:05.809Zhttps://docs.openclaw.ai/uk/cli/nodes2026-05-01T08:00:05.808Zhttps://docs.openclaw.ai/uk/cli/onboard2026-05-01T08:03:30.644Zhttps://docs.openclaw.ai/uk/cli/pairing2026-05-01T08:00:05.808Zhttps://docs.openclaw.ai/uk/cli/plugins2026-05-01T10:06:51.631Zhttps://docs.openclaw.ai/uk/cli/proxy2026-05-01T08:00:05.801Zhttps://docs.openclaw.ai/uk/cli/qr2026-05-01T08:00:05.800Zhttps://docs.openclaw.ai/uk/cli/reset2026-05-01T08:00:05.800Zhttps://docs.openclaw.ai/uk/cli/sandbox2026-05-01T08:00:05.799Zhttps://docs.openclaw.ai/uk/cli/secrets2026-05-01T08:00:05.839Zhttps://docs.openclaw.ai/uk/cli/security2026-05-01T15:50:25.391Zhttps://docs.openclaw.ai/uk/cli/sessions2026-05-01T08:00:05.837Zhttps://docs.openclaw.ai/uk/cli/setup2026-05-01T08:00:05.836Zhttps://docs.openclaw.ai/uk/cli/skills2026-05-01T08:00:05.837Zhttps://docs.openclaw.ai/uk/cli/status2026-05-01T08:00:05.834Zhttps://docs.openclaw.ai/uk/cli/system2026-05-01T08:00:05.830Zhttps://docs.openclaw.ai/uk/cli/tasks2026-05-01T08:00:05.829Zhttps://docs.openclaw.ai/uk/cli/tui2026-05-01T08:00:05.829Zhttps://docs.openclaw.ai/uk/cli/uninstall2026-05-01T08:00:05.828Zhttps://docs.openclaw.ai/uk/cli/update2026-05-01T09:02:40.672Zhttps://docs.openclaw.ai/uk/cli/voicecall2026-05-01T08:00:05.866Zhttps://docs.openclaw.ai/uk/cli/webhooks2026-05-01T08:00:05.867Zhttps://docs.openclaw.ai/uk/cli/wiki2026-05-01T08:00:05.867Zhttps://docs.openclaw.ai/uk/concepts/active-memory2026-05-01T08:00:05.866Zhttps://docs.openclaw.ai/uk/concepts/agent2026-05-01T08:00:05.858Zhttps://docs.openclaw.ai/uk/concepts/agent-loop2026-05-01T08:00:05.865Zhttps://docs.openclaw.ai/uk/concepts/agent-runtimes2026-05-01T08:00:05.869Zhttps://docs.openclaw.ai/uk/concepts/agent-workspace2026-05-01T08:00:05.858Zhttps://docs.openclaw.ai/uk/concepts/architecture2026-05-01T08:00:05.857Zhttps://docs.openclaw.ai/uk/concepts/channel-docking2026-05-01T08:00:05.896Zhttps://docs.openclaw.ai/uk/concepts/commitments2026-05-01T08:00:05.906Zhttps://docs.openclaw.ai/uk/concepts/compaction2026-05-01T08:00:05.906Zhttps://docs.openclaw.ai/uk/concepts/context2026-05-01T08:00:05.905Zhttps://docs.openclaw.ai/uk/concepts/context-engine2026-05-01T14:50:36.118Zhttps://docs.openclaw.ai/uk/concepts/delegate-architecture2026-05-01T08:00:05.904Zhttps://docs.openclaw.ai/uk/concepts/dreaming2026-05-01T08:00:05.903Zhttps://docs.openclaw.ai/uk/concepts/experimental-features2026-05-01T08:00:05.897Zhttps://docs.openclaw.ai/uk/concepts/features2026-05-01T08:00:05.896Zhttps://docs.openclaw.ai/uk/concepts/markdown-formatting2026-05-01T08:00:05.895Zhttps://docs.openclaw.ai/uk/concepts/memory2026-05-01T08:00:05.935Zhttps://docs.openclaw.ai/uk/concepts/memory-builtin2026-05-01T08:00:05.939Zhttps://docs.openclaw.ai/uk/concepts/memory-honcho2026-05-01T08:00:05.936Zhttps://docs.openclaw.ai/uk/concepts/memory-qmd2026-05-01T08:00:05.935Zhttps://docs.openclaw.ai/uk/concepts/memory-search2026-05-01T08:00:05.936Zhttps://docs.openclaw.ai/uk/concepts/messages2026-05-01T08:00:05.934Zhttps://docs.openclaw.ai/uk/concepts/model-failover2026-05-01T08:00:05.926Zhttps://docs.openclaw.ai/uk/concepts/model-providers2026-05-01T08:00:05.937Zhttps://docs.openclaw.ai/uk/concepts/models2026-05-01T08:00:05.927Zhttps://docs.openclaw.ai/uk/concepts/multi-agent2026-05-01T08:00:05.926Zhttps://docs.openclaw.ai/uk/concepts/oauth2026-05-01T08:00:05.980Zhttps://docs.openclaw.ai/uk/concepts/openclaw-sdk2026-05-01T08:24:50.930Zhttps://docs.openclaw.ai/uk/concepts/presence2026-05-01T08:00:05.969Zhttps://docs.openclaw.ai/uk/concepts/qa-e2e-automation2026-05-01T08:00:05.968Zhttps://docs.openclaw.ai/uk/concepts/qa-matrix2026-05-01T08:00:05.960Zhttps://docs.openclaw.ai/uk/concepts/queue2026-05-01T08:00:05.966Zhttps://docs.openclaw.ai/uk/concepts/queue-steering2026-05-01T08:00:05.969Zhttps://docs.openclaw.ai/uk/concepts/retry2026-05-01T08:00:05.960Zhttps://docs.openclaw.ai/uk/concepts/session2026-05-01T12:23:10.033Zhttps://docs.openclaw.ai/uk/concepts/session-pruning2026-05-01T08:00:05.967Zhttps://docs.openclaw.ai/uk/concepts/session-tool2026-05-01T08:00:05.959Zhttps://docs.openclaw.ai/uk/concepts/soul2026-05-01T08:00:06.010Zhttps://docs.openclaw.ai/uk/concepts/streaming2026-05-01T08:00:06.012Zhttps://docs.openclaw.ai/uk/concepts/system-prompt2026-05-01T08:00:06.009Zhttps://docs.openclaw.ai/uk/concepts/timezone2026-05-01T08:00:06.001Zhttps://docs.openclaw.ai/uk/concepts/typebox2026-05-01T08:00:05.999Zhttps://docs.openclaw.ai/uk/concepts/typing-indicators2026-05-01T08:00:06.002Zhttps://docs.openclaw.ai/uk/concepts/usage-tracking2026-05-01T08:00:06.000Zhttps://docs.openclaw.ai/uk/date-time2026-05-01T08:00:06.001Zhttps://docs.openclaw.ai/uk/debug/node-issue2026-05-01T08:00:06.006Zhttps://docs.openclaw.ai/uk/diagnostics/flags2026-05-01T08:00:06.046Zhttps://docs.openclaw.ai/uk/gateway/authentication2026-05-01T08:00:06.046Zhttps://docs.openclaw.ai/uk/gateway/background-process2026-05-01T08:00:06.045Zhttps://docs.openclaw.ai/uk/gateway/bonjour2026-05-01T08:00:06.038Zhttps://docs.openclaw.ai/uk/gateway/bridge-protocol2026-05-01T08:00:06.039Zhttps://docs.openclaw.ai/uk/gateway/cli-backends2026-05-01T08:00:06.038Zhttps://docs.openclaw.ai/uk/gateway/config-agents2026-05-01T11:46:06.067Zhttps://docs.openclaw.ai/uk/gateway/config-channels2026-05-01T11:58:47.480Zhttps://docs.openclaw.ai/uk/gateway/config-tools2026-05-01T08:00:06.037Zhttps://docs.openclaw.ai/uk/gateway/configuration2026-05-01T11:46:06.065Zhttps://docs.openclaw.ai/uk/gateway/configuration-examples2026-05-01T08:00:06.043Zhttps://docs.openclaw.ai/uk/gateway/configuration-reference2026-05-01T08:00:06.094Zhttps://docs.openclaw.ai/uk/gateway/diagnostics2026-05-01T08:00:06.079Zhttps://docs.openclaw.ai/uk/gateway/discovery2026-05-01T08:00:06.077Zhttps://docs.openclaw.ai/uk/gateway/doctor2026-05-01T08:56:42.149Zhttps://docs.openclaw.ai/uk/gateway/gateway-lock2026-05-01T08:00:06.070Zhttps://docs.openclaw.ai/uk/gateway/health2026-05-01T08:00:06.080Zhttps://docs.openclaw.ai/uk/gateway/heartbeat2026-05-01T08:00:06.082Zhttps://docs.openclaw.ai/uk/gateway2026-05-01T08:00:06.068Zhttps://docs.openclaw.ai/uk/gateway/local-models2026-05-01T08:00:06.069Zhttps://docs.openclaw.ai/uk/gateway/logging2026-05-01T08:00:06.125Zhttps://docs.openclaw.ai/uk/gateway/multiple-gateways2026-05-01T08:00:06.125Zhttps://docs.openclaw.ai/uk/gateway/network-model2026-05-01T08:00:06.123Zhttps://docs.openclaw.ai/uk/gateway/openai-http-api2026-05-01T08:00:06.116Zhttps://docs.openclaw.ai/uk/gateway/openresponses-http-api2026-05-01T08:00:06.123Zhttps://docs.openclaw.ai/uk/gateway/openshell2026-05-01T08:00:06.122Zhttps://docs.openclaw.ai/uk/gateway/opentelemetry2026-05-01T08:00:06.124Zhttps://docs.openclaw.ai/uk/gateway/pairing2026-05-01T08:00:06.115Zhttps://docs.openclaw.ai/uk/gateway/prometheus2026-05-01T08:00:06.115Zhttps://docs.openclaw.ai/uk/gateway/protocol2026-05-01T09:02:40.678Zhttps://docs.openclaw.ai/uk/gateway/remote2026-05-01T08:00:06.155Zhttps://docs.openclaw.ai/uk/gateway/remote-gateway-readme2026-05-01T08:00:06.183Zhttps://docs.openclaw.ai/uk/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T08:00:06.147Zhttps://docs.openclaw.ai/uk/gateway/sandboxing2026-05-01T11:46:06.066Zhttps://docs.openclaw.ai/uk/gateway/secrets2026-05-01T08:00:06.156Zhttps://docs.openclaw.ai/uk/gateway/secrets-plan-contract2026-05-01T08:00:06.154Zhttps://docs.openclaw.ai/uk/gateway/security/audit-checks2026-05-01T08:00:06.157Zhttps://docs.openclaw.ai/uk/gateway/security2026-04-30T01:17:42.882Zhttps://docs.openclaw.ai/uk/gateway/tailscale2026-05-01T08:00:06.147Zhttps://docs.openclaw.ai/uk/gateway/tools-invoke-http-api2026-05-01T08:00:06.148Zhttps://docs.openclaw.ai/uk/gateway/troubleshooting2026-05-01T08:00:06.146Zhttps://docs.openclaw.ai/uk/gateway/trusted-proxy-auth2026-05-01T08:00:06.218Zhttps://docs.openclaw.ai/uk/help/debugging2026-05-01T08:00:06.204Zhttps://docs.openclaw.ai/uk/help/environment2026-05-01T08:00:06.213Zhttps://docs.openclaw.ai/uk/help/faq2026-05-01T08:00:06.216Zhttps://docs.openclaw.ai/uk/help/faq-first-run2026-05-01T08:00:06.214Zhttps://docs.openclaw.ai/uk/help/faq-models2026-05-01T08:00:06.215Zhttps://docs.openclaw.ai/uk/help/gpt55-codex-agentic-parity2026-05-01T08:00:06.203Zhttps://docs.openclaw.ai/uk/help/gpt55-codex-agentic-parity-maintainers2026-05-01T08:00:06.203Zhttps://docs.openclaw.ai/uk/help2026-05-01T08:00:06.214Zhttps://docs.openclaw.ai/uk/help/scripts2026-05-01T08:00:06.202Zhttps://docs.openclaw.ai/uk/help/testing2026-05-01T08:35:28.163Zhttps://docs.openclaw.ai/uk/help/testing-live2026-05-01T08:00:06.249Zhttps://docs.openclaw.ai/uk/help/troubleshooting2026-05-01T08:00:06.248Zhttps://docs.openclaw.ai/uk2026-05-01T08:00:06.246Zhttps://docs.openclaw.ai/uk/install/ansible2026-05-01T11:46:06.060Zhttps://docs.openclaw.ai/uk/install/azure2026-05-01T08:00:06.245Zhttps://docs.openclaw.ai/uk/install/bun2026-05-01T08:00:06.247Zhttps://docs.openclaw.ai/uk/install/clawdock2026-05-01T08:00:06.237Zhttps://docs.openclaw.ai/uk/install/development-channels2026-05-01T08:00:06.237Zhttps://docs.openclaw.ai/uk/install/digitalocean2026-05-01T08:00:06.236Zhttps://docs.openclaw.ai/uk/install/docker2026-05-01T11:46:06.069Zhttps://docs.openclaw.ai/uk/install/docker-vm-runtime2026-05-01T08:00:06.287Zhttps://docs.openclaw.ai/uk/install/exe-dev2026-05-01T08:00:06.290Zhttps://docs.openclaw.ai/uk/install/fly2026-05-01T08:00:06.270Zhttps://docs.openclaw.ai/uk/install/gcp2026-05-01T08:00:06.289Zhttps://docs.openclaw.ai/uk/install/hetzner2026-05-01T08:00:06.276Zhttps://docs.openclaw.ai/uk/install/hostinger2026-05-01T08:00:06.270Zhttps://docs.openclaw.ai/uk/install2026-05-01T08:00:06.288Zhttps://docs.openclaw.ai/uk/install/installer2026-05-01T08:00:06.269Zhttps://docs.openclaw.ai/uk/install/kubernetes2026-05-01T08:00:06.269Zhttps://docs.openclaw.ai/uk/install/macos-vm2026-05-01T08:00:06.318Zhttps://docs.openclaw.ai/uk/install/migrating2026-05-01T08:00:06.316Zhttps://docs.openclaw.ai/uk/install/migrating-claude2026-05-01T08:00:06.311Zhttps://docs.openclaw.ai/uk/install/migrating-hermes2026-05-01T08:00:06.319Zhttps://docs.openclaw.ai/uk/install/nix2026-05-01T08:00:06.310Zhttps://docs.openclaw.ai/uk/install/node2026-05-01T08:00:06.314Zhttps://docs.openclaw.ai/uk/install/northflank2026-05-01T08:00:06.311Zhttps://docs.openclaw.ai/uk/install/oracle2026-05-01T08:00:06.310Zhttps://docs.openclaw.ai/uk/install/podman2026-05-01T08:00:06.309Zhttps://docs.openclaw.ai/uk/install/railway2026-05-01T08:00:06.312Zhttps://docs.openclaw.ai/uk/install/raspberry-pi2026-05-01T08:00:06.346Zhttps://docs.openclaw.ai/uk/install/render2026-05-01T08:00:06.344Zhttps://docs.openclaw.ai/uk/install/uninstall2026-05-01T08:00:06.347Zhttps://docs.openclaw.ai/uk/install/updating2026-05-01T09:02:40.670Zhttps://docs.openclaw.ai/uk/logging2026-05-01T08:00:06.345Zhttps://docs.openclaw.ai/uk/network2026-05-01T08:00:06.347Zhttps://docs.openclaw.ai/uk/nodes/audio2026-05-01T08:00:06.339Zhttps://docs.openclaw.ai/uk/nodes/camera2026-05-01T08:00:06.339Zhttps://docs.openclaw.ai/uk/nodes/images2026-05-01T08:00:06.338Zhttps://docs.openclaw.ai/uk/nodes2026-05-01T08:00:06.338Zhttps://docs.openclaw.ai/uk/nodes/location-command2026-05-01T08:00:06.389Zhttps://docs.openclaw.ai/uk/nodes/media-understanding2026-05-01T08:00:06.371Zhttps://docs.openclaw.ai/uk/nodes/talk2026-05-01T08:00:06.388Zhttps://docs.openclaw.ai/uk/nodes/troubleshooting2026-05-01T08:00:06.372Zhttps://docs.openclaw.ai/uk/nodes/voicewake2026-05-01T08:00:06.389Zhttps://docs.openclaw.ai/uk/pi2026-05-01T08:00:06.371Zhttps://docs.openclaw.ai/uk/pi-dev2026-05-01T08:00:06.370Zhttps://docs.openclaw.ai/uk/platforms/android2026-05-01T08:00:06.418Zhttps://docs.openclaw.ai/uk/platforms2026-05-01T08:00:06.418Zhttps://docs.openclaw.ai/uk/platforms/ios2026-05-01T08:00:06.408Zhttps://docs.openclaw.ai/uk/platforms/linux2026-05-01T08:00:06.410Zhttps://docs.openclaw.ai/uk/platforms/mac/bundled-gateway2026-05-01T08:00:06.413Zhttps://docs.openclaw.ai/uk/platforms/mac/canvas2026-05-01T08:00:06.416Zhttps://docs.openclaw.ai/uk/platforms/mac/child-process2026-05-01T08:00:06.409Zhttps://docs.openclaw.ai/uk/platforms/mac/dev-setup2026-05-01T08:00:06.410Zhttps://docs.openclaw.ai/uk/platforms/mac/health2026-05-01T08:00:06.409Zhttps://docs.openclaw.ai/uk/platforms/mac/icon2026-05-01T08:00:06.447Zhttps://docs.openclaw.ai/uk/platforms/mac/logging2026-05-01T08:00:06.446Zhttps://docs.openclaw.ai/uk/platforms/mac/menu-bar2026-05-01T08:00:06.446Zhttps://docs.openclaw.ai/uk/platforms/mac/peekaboo2026-05-01T08:00:06.445Zhttps://docs.openclaw.ai/uk/platforms/mac/permissions2026-05-01T08:00:06.442Zhttps://docs.openclaw.ai/uk/platforms/mac/remote2026-05-01T08:00:06.444Zhttps://docs.openclaw.ai/uk/platforms/mac/signing2026-05-01T08:00:06.438Zhttps://docs.openclaw.ai/uk/platforms/mac/skills2026-05-01T08:00:06.438Zhttps://docs.openclaw.ai/uk/platforms/mac/voice-overlay2026-05-01T08:00:06.437Zhttps://docs.openclaw.ai/uk/platforms/mac/voicewake2026-05-01T08:00:06.437Zhttps://docs.openclaw.ai/uk/platforms/mac/webchat2026-05-01T08:00:06.478Zhttps://docs.openclaw.ai/uk/platforms/mac/xpc2026-05-01T08:00:06.477Zhttps://docs.openclaw.ai/uk/platforms/macos2026-05-01T08:00:06.477Zhttps://docs.openclaw.ai/uk/platforms/windows2026-05-01T08:00:06.471Zhttps://docs.openclaw.ai/uk/plugins/architecture2026-05-01T08:00:06.467Zhttps://docs.openclaw.ai/uk/plugins/architecture-internals2026-05-01T08:00:06.479Zhttps://docs.openclaw.ai/uk/plugins/building-plugins2026-05-01T10:06:51.624Zhttps://docs.openclaw.ai/uk/plugins/bundles2026-05-01T08:00:06.520Zhttps://docs.openclaw.ai/uk/plugins/codex-computer-use2026-05-01T08:00:06.511Zhttps://docs.openclaw.ai/uk/plugins/codex-harness2026-05-01T11:46:06.076Zhttps://docs.openclaw.ai/uk/plugins/community2026-05-01T08:00:06.522Zhttps://docs.openclaw.ai/uk/plugins/compatibility2026-05-01T08:00:06.518Zhttps://docs.openclaw.ai/uk/plugins/dependency-resolution2026-05-01T08:03:30.631Zhttps://docs.openclaw.ai/uk/plugins/google-meet2026-05-01T13:37:51.603Zhttps://docs.openclaw.ai/uk/plugins/hooks2026-05-01T08:00:06.508Zhttps://docs.openclaw.ai/uk/plugins/manifest2026-05-01T08:00:06.508Zhttps://docs.openclaw.ai/uk/plugins/memory-lancedb2026-05-01T08:00:06.507Zhttps://docs.openclaw.ai/uk/plugins/memory-wiki2026-05-01T08:00:06.560Zhttps://docs.openclaw.ai/uk/plugins/message-presentation2026-05-01T08:00:06.557Zhttps://docs.openclaw.ai/uk/plugins/sdk-agent-harness2026-05-01T08:00:06.557Zhttps://docs.openclaw.ai/uk/plugins/sdk-channel-plugins2026-05-01T08:00:06.546Zhttps://docs.openclaw.ai/uk/plugins/sdk-channel-turn2026-05-01T08:00:06.555Zhttps://docs.openclaw.ai/uk/plugins/sdk-entrypoints2026-05-01T08:00:06.547Zhttps://docs.openclaw.ai/uk/plugins/sdk-migration2026-05-01T08:00:06.556Zhttps://docs.openclaw.ai/uk/plugins/sdk-overview2026-05-01T08:00:06.548Zhttps://docs.openclaw.ai/uk/plugins/sdk-provider-plugins2026-05-01T08:09:23.021Zhttps://docs.openclaw.ai/uk/plugins/sdk-runtime2026-05-01T08:00:06.546Zhttps://docs.openclaw.ai/uk/plugins/sdk-setup2026-05-01T08:00:06.605Zhttps://docs.openclaw.ai/uk/plugins/sdk-subpaths2026-05-01T08:00:06.604Zhttps://docs.openclaw.ai/uk/plugins/sdk-testing2026-05-01T08:00:06.592Zhttps://docs.openclaw.ai/uk/plugins/skill-workshop2026-05-01T08:00:06.592Zhttps://docs.openclaw.ai/uk/plugins/voice-call2026-05-01T11:46:06.071Zhttps://docs.openclaw.ai/uk/plugins/webhooks2026-05-01T08:00:06.581Zhttps://docs.openclaw.ai/uk/plugins/zalouser2026-05-01T08:00:06.591Zhttps://docs.openclaw.ai/uk/prose2026-05-01T08:00:06.581Zhttps://docs.openclaw.ai/uk/providers/alibaba2026-05-01T08:00:06.582Zhttps://docs.openclaw.ai/uk/providers/anthropic2026-05-01T08:00:06.580Zhttps://docs.openclaw.ai/uk/providers/arcee2026-05-01T08:00:06.634Zhttps://docs.openclaw.ai/uk/providers/azure-speech2026-05-01T08:00:06.632Zhttps://docs.openclaw.ai/uk/providers/bedrock2026-05-01T08:00:06.633Zhttps://docs.openclaw.ai/uk/providers/bedrock-mantle2026-05-01T08:00:06.629Zhttps://docs.openclaw.ai/uk/providers/chutes2026-05-01T08:00:06.624Zhttps://docs.openclaw.ai/uk/providers/claude-max-api-proxy2026-05-01T08:00:06.631Zhttps://docs.openclaw.ai/uk/providers/cloudflare-ai-gateway2026-05-01T08:00:06.632Zhttps://docs.openclaw.ai/uk/providers/comfy2026-05-01T08:00:06.624Zhttps://docs.openclaw.ai/uk/providers/deepgram2026-05-01T08:00:06.623Zhttps://docs.openclaw.ai/uk/providers/deepinfra2026-05-01T08:00:06.664Zhttps://docs.openclaw.ai/uk/providers/deepseek2026-05-01T08:00:06.662Zhttps://docs.openclaw.ai/uk/providers/elevenlabs2026-05-01T08:00:06.663Zhttps://docs.openclaw.ai/uk/providers/fal2026-05-01T08:00:06.655Zhttps://docs.openclaw.ai/uk/providers/fireworks2026-05-01T08:00:06.662Zhttps://docs.openclaw.ai/uk/providers/github-copilot2026-05-01T08:00:06.654Zhttps://docs.openclaw.ai/uk/providers/glm2026-05-01T08:00:06.659Zhttps://docs.openclaw.ai/uk/providers/google2026-05-01T08:00:06.661Zhttps://docs.openclaw.ai/uk/providers/gradium2026-05-01T08:00:06.654Zhttps://docs.openclaw.ai/uk/providers/groq2026-05-01T12:52:47.251Zhttps://docs.openclaw.ai/uk/providers/huggingface2026-05-01T08:00:06.690Zhttps://docs.openclaw.ai/uk/providers2026-05-01T08:00:06.692Zhttps://docs.openclaw.ai/uk/providers/inferrs2026-05-01T08:00:06.704Zhttps://docs.openclaw.ai/uk/providers/inworld2026-05-01T08:00:06.691Zhttps://docs.openclaw.ai/uk/providers/kilocode2026-05-01T08:00:06.684Zhttps://docs.openclaw.ai/uk/providers/litellm2026-05-01T08:00:06.689Zhttps://docs.openclaw.ai/uk/providers/lmstudio2026-05-01T08:00:06.692Zhttps://docs.openclaw.ai/uk/providers/minimax2026-05-01T08:00:06.688Zhttps://docs.openclaw.ai/uk/providers/mistral2026-05-01T08:00:06.683Zhttps://docs.openclaw.ai/uk/providers/models2026-05-01T08:00:06.683Zhttps://docs.openclaw.ai/uk/providers/moonshot2026-05-01T08:00:06.733Zhttps://docs.openclaw.ai/uk/providers/nvidia2026-05-01T08:00:06.734Zhttps://docs.openclaw.ai/uk/providers/ollama2026-05-01T08:00:06.725Zhttps://docs.openclaw.ai/uk/providers/openai2026-05-01T08:00:06.733Zhttps://docs.openclaw.ai/uk/providers/opencode2026-05-01T08:00:06.730Zhttps://docs.openclaw.ai/uk/providers/opencode-go2026-05-01T08:00:06.735Zhttps://docs.openclaw.ai/uk/providers/openrouter2026-05-01T08:00:06.725Zhttps://docs.openclaw.ai/uk/providers/perplexity-provider2026-05-01T08:00:06.726Zhttps://docs.openclaw.ai/uk/providers/qianfan2026-05-01T08:00:06.724Zhttps://docs.openclaw.ai/uk/providers/qwen2026-05-01T08:00:06.724Zhttps://docs.openclaw.ai/uk/providers/runway2026-05-01T08:00:06.764Zhttps://docs.openclaw.ai/uk/providers/sglang2026-05-01T08:00:06.763Zhttps://docs.openclaw.ai/uk/providers/stepfun2026-05-01T08:00:06.763Zhttps://docs.openclaw.ai/uk/providers/synthetic2026-05-01T08:00:06.760Zhttps://docs.openclaw.ai/uk/providers/tencent2026-05-01T08:00:06.755Zhttps://docs.openclaw.ai/uk/providers/together2026-05-01T08:00:06.755Zhttps://docs.openclaw.ai/uk/providers/venice2026-05-01T13:01:52.309Zhttps://docs.openclaw.ai/uk/providers/vercel-ai-gateway2026-05-01T08:00:06.754Zhttps://docs.openclaw.ai/uk/providers/vllm2026-05-01T08:00:06.753Zhttps://docs.openclaw.ai/uk/providers/volcengine2026-05-01T08:00:06.806Zhttps://docs.openclaw.ai/uk/providers/vydra2026-05-01T08:00:06.793Zhttps://docs.openclaw.ai/uk/providers/xai2026-05-01T08:00:06.794Zhttps://docs.openclaw.ai/uk/providers/xiaomi2026-05-01T08:00:06.790Zhttps://docs.openclaw.ai/uk/providers/zai2026-05-01T13:15:49.043Zhttps://docs.openclaw.ai/uk/reference/AGENTS.default2026-05-01T08:00:06.791Zhttps://docs.openclaw.ai/uk/reference/RELEASING2026-05-01T08:00:06.791Zhttps://docs.openclaw.ai/uk/reference/api-usage-costs2026-05-01T08:00:06.787Zhttps://docs.openclaw.ai/uk/reference/credits2026-05-01T08:00:06.786Zhttps://docs.openclaw.ai/uk/reference/device-models2026-05-01T08:00:06.840Zhttps://docs.openclaw.ai/uk/reference/full-release-validation2026-05-01T08:00:06.833Zhttps://docs.openclaw.ai/uk/reference/memory-config2026-05-01T08:00:06.836Zhttps://docs.openclaw.ai/uk/reference/openclaw-sdk-api-design2026-05-01T08:00:06.835Zhttps://docs.openclaw.ai/uk/reference/prompt-caching2026-05-01T08:00:06.827Zhttps://docs.openclaw.ai/uk/reference/rich-output-protocol2026-05-01T08:00:06.833Zhttps://docs.openclaw.ai/uk/reference/rpc2026-05-01T08:00:06.834Zhttps://docs.openclaw.ai/uk/reference/secretref-credential-surface2026-05-01T08:00:06.827Zhttps://docs.openclaw.ai/uk/reference/session-management-compaction2026-05-01T12:23:10.029Zhttps://docs.openclaw.ai/uk/reference/templates/AGENTS2026-05-01T08:00:06.868Zhttps://docs.openclaw.ai/uk/reference/templates/BOOT2026-05-01T08:00:06.864Zhttps://docs.openclaw.ai/uk/reference/templates/BOOTSTRAP2026-05-01T08:00:06.863Zhttps://docs.openclaw.ai/uk/reference/templates/HEARTBEAT2026-05-01T08:00:06.867Zhttps://docs.openclaw.ai/uk/reference/templates/IDENTITY2026-05-01T08:00:06.866Zhttps://docs.openclaw.ai/uk/reference/templates/SOUL2026-05-01T08:00:06.866Zhttps://docs.openclaw.ai/uk/reference/templates/TOOLS2026-05-01T08:00:06.857Zhttps://docs.openclaw.ai/uk/reference/templates/USER2026-05-01T08:00:06.920Zhttps://docs.openclaw.ai/uk/reference/test2026-05-01T08:35:28.156Zhttps://docs.openclaw.ai/uk/reference/token-use2026-05-01T08:00:06.914Zhttps://docs.openclaw.ai/uk/reference/transcript-hygiene2026-05-01T08:00:06.911Zhttps://docs.openclaw.ai/uk/reference/wizard2026-05-01T08:00:06.918Zhttps://docs.openclaw.ai/uk/security/CONTRIBUTING-THREAT-MODEL2026-05-01T08:00:06.916Zhttps://docs.openclaw.ai/uk/security/THREAT-MODEL-ATLAS2026-05-01T08:00:06.919Zhttps://docs.openclaw.ai/uk/security/formal-verification2026-05-01T08:00:06.910Zhttps://docs.openclaw.ai/uk/security/network-proxy2026-05-01T08:00:06.910Zhttps://docs.openclaw.ai/uk/start/bootstrapping2026-05-01T08:00:06.949Zhttps://docs.openclaw.ai/uk/start/docs-directory2026-05-01T08:00:06.947Zhttps://docs.openclaw.ai/uk/start/getting-started2026-05-01T08:00:06.948Zhttps://docs.openclaw.ai/uk/start/hubs2026-05-01T08:00:06.949Zhttps://docs.openclaw.ai/uk/start/lore2026-05-01T08:00:06.943Zhttps://docs.openclaw.ai/uk/start/onboarding2026-05-01T08:00:06.946Zhttps://docs.openclaw.ai/uk/start/onboarding-overview2026-05-01T08:00:06.947Zhttps://docs.openclaw.ai/uk/start/openclaw2026-05-01T08:00:06.939Zhttps://docs.openclaw.ai/uk/start/setup2026-05-01T08:00:06.938Zhttps://docs.openclaw.ai/uk/start/showcase2026-04-24T04:32:14.878Zhttps://docs.openclaw.ai/uk/start/wizard2026-05-01T08:00:06.980Zhttps://docs.openclaw.ai/uk/start/wizard-cli-automation2026-05-01T08:00:06.980Zhttps://docs.openclaw.ai/uk/start/wizard-cli-reference2026-05-01T08:00:06.978Zhttps://docs.openclaw.ai/uk/tools/acp-agents2026-05-01T08:03:30.660Zhttps://docs.openclaw.ai/uk/tools/acp-agents-setup2026-05-01T08:00:06.971Zhttps://docs.openclaw.ai/uk/tools/agent-send2026-05-01T08:00:06.971Zhttps://docs.openclaw.ai/uk/tools/apply-patch2026-05-01T08:00:06.970Zhttps://docs.openclaw.ai/uk/tools/brave-search2026-05-01T08:00:06.969Zhttps://docs.openclaw.ai/uk/tools/browser2026-05-01T08:00:07.016Zhttps://docs.openclaw.ai/uk/tools/browser-control2026-05-01T08:00:07.014Zhttps://docs.openclaw.ai/uk/tools/browser-linux-troubleshooting2026-05-01T08:00:07.014Zhttps://docs.openclaw.ai/uk/tools/browser-login2026-05-01T08:00:07.009Zhttps://docs.openclaw.ai/uk/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T08:00:07.015Zhttps://docs.openclaw.ai/uk/tools/btw2026-05-01T08:00:07.016Zhttps://docs.openclaw.ai/uk/tools/clawhub2026-05-01T08:00:07.013Zhttps://docs.openclaw.ai/uk/tools/code-execution2026-05-01T08:00:07.004Zhttps://docs.openclaw.ai/uk/tools/creating-skills2026-05-01T08:00:07.003Zhttps://docs.openclaw.ai/uk/tools/diffs2026-05-01T08:00:07.046Zhttps://docs.openclaw.ai/uk/tools/duckduckgo-search2026-05-01T08:00:07.044Zhttps://docs.openclaw.ai/uk/tools/elevated2026-05-01T08:00:07.045Zhttps://docs.openclaw.ai/uk/tools/exa-search2026-05-01T08:00:07.044Zhttps://docs.openclaw.ai/uk/tools/exec2026-05-01T08:00:07.043Zhttps://docs.openclaw.ai/uk/tools/exec-approvals2026-05-01T08:00:07.035Zhttps://docs.openclaw.ai/uk/tools/exec-approvals-advanced2026-05-01T08:00:07.045Zhttps://docs.openclaw.ai/uk/tools/firecrawl2026-05-01T08:00:07.041Zhttps://docs.openclaw.ai/uk/tools/gemini-search2026-05-01T08:00:07.035Zhttps://docs.openclaw.ai/uk/tools/grok-search2026-05-01T08:00:07.034Zhttps://docs.openclaw.ai/uk/tools/image-generation2026-05-01T08:00:07.087Zhttps://docs.openclaw.ai/uk/tools2026-05-01T08:00:07.074Zhttps://docs.openclaw.ai/uk/tools/kimi-search2026-05-01T08:00:07.074Zhttps://docs.openclaw.ai/uk/tools/llm-task2026-05-01T08:00:07.075Zhttps://docs.openclaw.ai/uk/tools/lobster2026-05-01T08:00:07.075Zhttps://docs.openclaw.ai/uk/tools/loop-detection2026-05-01T08:00:07.073Zhttps://docs.openclaw.ai/uk/tools/media-overview2026-05-01T08:00:07.072Zhttps://docs.openclaw.ai/uk/tools/minimax-search2026-05-01T08:00:07.066Zhttps://docs.openclaw.ai/uk/tools/multi-agent-sandbox-tools2026-05-01T08:00:07.065Zhttps://docs.openclaw.ai/uk/tools/music-generation2026-05-01T08:00:07.064Zhttps://docs.openclaw.ai/uk/tools/ollama-search2026-05-01T08:00:07.119Zhttps://docs.openclaw.ai/uk/tools/pdf2026-05-01T08:00:07.110Zhttps://docs.openclaw.ai/uk/tools/perplexity-search2026-05-01T08:00:07.116Zhttps://docs.openclaw.ai/uk/tools/plugin2026-05-01T10:06:51.627Zhttps://docs.openclaw.ai/uk/tools/reactions2026-05-01T08:00:07.109Zhttps://docs.openclaw.ai/uk/tools/searxng-search2026-05-01T08:00:07.118Zhttps://docs.openclaw.ai/uk/tools/skills2026-05-01T08:00:07.109Zhttps://docs.openclaw.ai/uk/tools/skills-config2026-05-01T08:00:07.114Zhttps://docs.openclaw.ai/uk/tools/slash-commands2026-05-01T10:06:51.627Zhttps://docs.openclaw.ai/uk/tools/subagents2026-05-01T08:00:07.108Zhttps://docs.openclaw.ai/uk/tools/tavily2026-05-01T08:00:07.151Zhttps://docs.openclaw.ai/uk/tools/thinking2026-05-01T08:00:07.145Zhttps://docs.openclaw.ai/uk/tools/tokenjuice2026-05-01T08:00:07.144Zhttps://docs.openclaw.ai/uk/tools/trajectory2026-05-01T08:00:07.147Zhttps://docs.openclaw.ai/uk/tools/tts2026-05-01T08:00:07.147Zhttps://docs.openclaw.ai/uk/tools/video-generation2026-05-01T08:00:07.145Zhttps://docs.openclaw.ai/uk/tools/web2026-05-01T08:00:07.138Zhttps://docs.openclaw.ai/uk/tools/web-fetch2026-05-01T08:00:07.148Zhttps://docs.openclaw.ai/uk/vps2026-05-01T08:00:07.137Zhttps://docs.openclaw.ai/uk/web/control-ui2026-05-01T08:00:07.190Zhttps://docs.openclaw.ai/uk/web/dashboard2026-05-01T08:00:07.179Zhttps://docs.openclaw.ai/uk/web2026-05-01T08:00:07.178Zhttps://docs.openclaw.ai/uk/web/tui2026-05-01T08:00:07.176Zhttps://docs.openclaw.ai/uk/web/webchat2026-05-01T08:00:07.170Zhttps://docs.openclaw.ai/vi/auth-credential-semantics2026-05-01T08:00:07.177Zhttps://docs.openclaw.ai/vi/automation/cron-jobs2026-05-01T08:00:07.169Zhttps://docs.openclaw.ai/vi/automation/hooks2026-05-01T08:00:07.216Zhttps://docs.openclaw.ai/vi/automation2026-05-01T08:00:07.217Zhttps://docs.openclaw.ai/vi/automation/standing-orders2026-05-01T08:00:07.218Zhttps://docs.openclaw.ai/vi/automation/taskflow2026-05-01T08:00:07.214Zhttps://docs.openclaw.ai/vi/automation/tasks2026-05-01T10:57:43.450Zhttps://docs.openclaw.ai/vi/channels/bluebubbles2026-05-01T10:57:43.451Zhttps://docs.openclaw.ai/vi/channels/broadcast-groups2026-05-01T08:00:07.248Zhttps://docs.openclaw.ai/vi/channels/channel-routing2026-05-01T08:00:07.246Zhttps://docs.openclaw.ai/vi/channels/discord2026-05-01T10:57:43.454Zhttps://docs.openclaw.ai/vi/channels/feishu2026-05-01T08:00:07.239Zhttps://docs.openclaw.ai/vi/channels/googlechat2026-05-01T08:00:07.247Zhttps://docs.openclaw.ai/vi/channels/group-messages2026-05-01T08:00:07.238Zhttps://docs.openclaw.ai/vi/channels/groups2026-05-01T10:57:43.449Zhttps://docs.openclaw.ai/vi/channels/imessage2026-05-01T08:00:07.237Zhttps://docs.openclaw.ai/vi/channels2026-05-01T08:00:07.294Zhttps://docs.openclaw.ai/vi/channels/irc2026-05-01T08:00:07.273Zhttps://docs.openclaw.ai/vi/channels/line2026-05-01T08:00:07.281Zhttps://docs.openclaw.ai/vi/channels/location2026-05-01T08:00:07.278Zhttps://docs.openclaw.ai/vi/channels/matrix2026-05-01T08:00:07.273Zhttps://docs.openclaw.ai/vi/channels/matrix-migration2026-05-01T08:00:07.281Zhttps://docs.openclaw.ai/vi/channels/matrix-push-rules2026-05-01T08:00:07.293Zhttps://docs.openclaw.ai/vi/channels/mattermost2026-05-01T08:00:07.280Zhttps://docs.openclaw.ai/vi/channels/msteams2026-05-01T08:00:07.272Zhttps://docs.openclaw.ai/vi/channels/nextcloud-talk2026-05-01T08:00:07.272Zhttps://docs.openclaw.ai/vi/channels/nostr2026-05-01T08:00:07.325Zhttps://docs.openclaw.ai/vi/channels/pairing2026-05-01T08:00:07.323Zhttps://docs.openclaw.ai/vi/channels/qa-channel2026-05-01T10:57:43.447Zhttps://docs.openclaw.ai/vi/channels/qqbot2026-05-01T08:00:07.322Zhttps://docs.openclaw.ai/vi/channels/signal2026-05-01T08:00:07.324Zhttps://docs.openclaw.ai/vi/channels/slack2026-05-01T08:00:07.316Zhttps://docs.openclaw.ai/vi/channels/synology-chat2026-05-01T08:00:07.323Zhttps://docs.openclaw.ai/vi/channels/telegram2026-05-01T08:00:07.315Zhttps://docs.openclaw.ai/vi/channels/tlon2026-05-01T08:00:07.314Zhttps://docs.openclaw.ai/vi/channels/troubleshooting2026-05-01T08:00:07.314Zhttps://docs.openclaw.ai/vi/channels/twitch2026-05-01T08:00:07.355Zhttps://docs.openclaw.ai/vi/channels/wechat2026-05-01T08:00:07.353Zhttps://docs.openclaw.ai/vi/channels/whatsapp2026-05-01T08:00:07.355Zhttps://docs.openclaw.ai/vi/channels/yuanbao2026-05-01T08:00:07.353Zhttps://docs.openclaw.ai/vi/channels/zalo2026-05-01T08:00:07.354Zhttps://docs.openclaw.ai/vi/channels/zalouser2026-05-01T08:00:07.350Zhttps://docs.openclaw.ai/vi/ci2026-05-01T10:57:43.457Zhttps://docs.openclaw.ai/vi/cli/acp2026-05-01T08:00:07.345Zhttps://docs.openclaw.ai/vi/cli/agent2026-05-01T08:00:07.344Zhttps://docs.openclaw.ai/vi/cli/agents2026-05-01T08:00:07.344Zhttps://docs.openclaw.ai/vi/cli/approvals2026-05-01T08:00:07.396Zhttps://docs.openclaw.ai/vi/cli/backup2026-05-01T08:00:07.396Zhttps://docs.openclaw.ai/vi/cli/browser2026-05-01T08:00:07.384Zhttps://docs.openclaw.ai/vi/cli/channels2026-05-01T10:57:43.448Zhttps://docs.openclaw.ai/vi/cli/clawbot2026-05-01T08:00:07.384Zhttps://docs.openclaw.ai/vi/cli/commitments2026-05-01T08:00:07.381Zhttps://docs.openclaw.ai/vi/cli/completion2026-05-01T08:00:07.377Zhttps://docs.openclaw.ai/vi/cli/config2026-05-01T08:00:07.376Zhttps://docs.openclaw.ai/vi/cli/configure2026-05-01T10:57:43.451Zhttps://docs.openclaw.ai/vi/cli/cron2026-05-01T08:00:07.425Zhttps://docs.openclaw.ai/vi/cli/daemon2026-05-01T08:00:07.425Zhttps://docs.openclaw.ai/vi/cli/dashboard2026-05-01T08:00:07.420Zhttps://docs.openclaw.ai/vi/cli/devices2026-05-01T08:00:07.424Zhttps://docs.openclaw.ai/vi/cli/directory2026-05-01T08:00:07.416Zhttps://docs.openclaw.ai/vi/cli/dns2026-05-01T08:00:07.423Zhttps://docs.openclaw.ai/vi/cli/docs2026-05-01T08:00:07.422Zhttps://docs.openclaw.ai/vi/cli/doctor2026-05-01T08:00:07.422Zhttps://docs.openclaw.ai/vi/cli/flows2026-05-01T08:00:07.415Zhttps://docs.openclaw.ai/vi/cli/gateway2026-05-01T10:57:43.460Zhttps://docs.openclaw.ai/vi/cli/health2026-05-01T08:00:07.456Zhttps://docs.openclaw.ai/vi/cli/hooks2026-05-01T08:00:07.455Zhttps://docs.openclaw.ai/vi/cli2026-05-01T08:00:07.455Zhttps://docs.openclaw.ai/vi/cli/infer2026-05-01T08:00:07.454Zhttps://docs.openclaw.ai/vi/cli/logs2026-05-01T08:00:07.447Zhttps://docs.openclaw.ai/vi/cli/mcp2026-05-01T08:00:07.447Zhttps://docs.openclaw.ai/vi/cli/memory2026-05-01T08:00:07.452Zhttps://docs.openclaw.ai/vi/cli/message2026-05-01T08:00:07.454Zhttps://docs.openclaw.ai/vi/cli/migrate2026-05-01T08:00:07.446Zhttps://docs.openclaw.ai/vi/cli/models2026-05-01T10:57:43.452Zhttps://docs.openclaw.ai/vi/cli/node2026-05-01T08:00:07.507Zhttps://docs.openclaw.ai/vi/cli/nodes2026-05-01T08:00:07.495Zhttps://docs.openclaw.ai/vi/cli/onboard2026-05-01T10:57:46.441Zhttps://docs.openclaw.ai/vi/cli/pairing2026-05-01T08:00:07.484Zhttps://docs.openclaw.ai/vi/cli/plugins2026-05-01T10:57:46.446Zhttps://docs.openclaw.ai/vi/cli/proxy2026-05-01T10:57:46.435Zhttps://docs.openclaw.ai/vi/cli/qr2026-05-01T08:00:07.485Zhttps://docs.openclaw.ai/vi/cli/reset2026-05-01T08:00:07.485Zhttps://docs.openclaw.ai/vi/cli/sandbox2026-05-01T08:00:07.483Zhttps://docs.openclaw.ai/vi/cli/secrets2026-05-01T08:00:07.483Zhttps://docs.openclaw.ai/vi/cli/security2026-05-01T08:00:07.543Zhttps://docs.openclaw.ai/vi/cli/sessions2026-05-01T08:00:07.539Zhttps://docs.openclaw.ai/vi/cli/setup2026-05-01T08:00:07.535Zhttps://docs.openclaw.ai/vi/cli/skills2026-05-01T08:00:07.540Zhttps://docs.openclaw.ai/vi/cli/status2026-05-01T08:00:07.542Zhttps://docs.openclaw.ai/vi/cli/system2026-05-01T08:00:07.536Zhttps://docs.openclaw.ai/vi/cli/tasks2026-05-01T08:00:07.534Zhttps://docs.openclaw.ai/vi/cli/tui2026-05-01T08:00:07.534Zhttps://docs.openclaw.ai/vi/cli/uninstall2026-05-01T08:00:07.533Zhttps://docs.openclaw.ai/vi/cli/update2026-05-01T10:57:46.442Zhttps://docs.openclaw.ai/vi/cli/voicecall2026-05-01T10:57:46.442Zhttps://docs.openclaw.ai/vi/cli/webhooks2026-05-01T08:00:07.572Zhttps://docs.openclaw.ai/vi/cli/wiki2026-05-01T08:00:07.569Zhttps://docs.openclaw.ai/vi/concepts/active-memory2026-05-01T08:00:07.571Zhttps://docs.openclaw.ai/vi/concepts/agent2026-05-01T08:00:07.572Zhttps://docs.openclaw.ai/vi/concepts/agent-loop2026-05-01T08:00:07.562Zhttps://docs.openclaw.ai/vi/concepts/agent-runtimes2026-05-01T08:00:07.569Zhttps://docs.openclaw.ai/vi/concepts/agent-workspace2026-05-01T08:00:07.570Zhttps://docs.openclaw.ai/vi/concepts/architecture2026-05-01T08:00:07.563Zhttps://docs.openclaw.ai/vi/concepts/channel-docking2026-05-01T08:00:07.562Zhttps://docs.openclaw.ai/vi/concepts/commitments2026-05-01T10:57:46.438Zhttps://docs.openclaw.ai/vi/concepts/compaction2026-05-01T08:00:07.625Zhttps://docs.openclaw.ai/vi/concepts/context2026-05-01T08:00:07.617Zhttps://docs.openclaw.ai/vi/concepts/context-engine2026-05-01T08:00:07.627Zhttps://docs.openclaw.ai/vi/concepts/delegate-architecture2026-05-01T08:00:07.626Zhttps://docs.openclaw.ai/vi/concepts/dreaming2026-05-01T08:00:07.627Zhttps://docs.openclaw.ai/vi/concepts/experimental-features2026-05-01T08:00:07.626Zhttps://docs.openclaw.ai/vi/concepts/features2026-05-01T08:00:07.618Zhttps://docs.openclaw.ai/vi/concepts/markdown-formatting2026-05-01T08:00:07.618Zhttps://docs.openclaw.ai/vi/concepts/memory2026-05-01T08:00:07.650Zhttps://docs.openclaw.ai/vi/concepts/memory-builtin2026-05-01T08:00:07.623Zhttps://docs.openclaw.ai/vi/concepts/memory-honcho2026-05-01T08:00:07.660Zhttps://docs.openclaw.ai/vi/concepts/memory-qmd2026-05-01T08:00:07.658Zhttps://docs.openclaw.ai/vi/concepts/memory-search2026-05-01T08:00:07.649Zhttps://docs.openclaw.ai/vi/concepts/messages2026-05-01T08:00:07.654Zhttps://docs.openclaw.ai/vi/concepts/model-failover2026-05-01T08:00:07.658Zhttps://docs.openclaw.ai/vi/concepts/model-providers2026-05-01T08:00:07.656Zhttps://docs.openclaw.ai/vi/concepts/models2026-05-01T08:00:07.657Zhttps://docs.openclaw.ai/vi/concepts/multi-agent2026-05-01T08:00:07.651Zhttps://docs.openclaw.ai/vi/concepts/oauth2026-05-01T08:00:07.649Zhttps://docs.openclaw.ai/vi/concepts/openclaw-sdk2026-05-01T10:57:46.437Zhttps://docs.openclaw.ai/vi/concepts/presence2026-05-01T08:00:07.688Zhttps://docs.openclaw.ai/vi/concepts/qa-e2e-automation2026-05-01T08:00:07.688Zhttps://docs.openclaw.ai/vi/concepts/qa-matrix2026-05-01T08:00:07.680Zhttps://docs.openclaw.ai/vi/concepts/queue2026-05-01T08:00:07.689Zhttps://docs.openclaw.ai/vi/concepts/queue-steering2026-05-01T08:00:07.685Zhttps://docs.openclaw.ai/vi/concepts/retry2026-05-01T08:00:07.680Zhttps://docs.openclaw.ai/vi/concepts/session2026-05-01T08:00:07.679Zhttps://docs.openclaw.ai/vi/concepts/session-pruning2026-05-01T08:00:07.681Zhttps://docs.openclaw.ai/vi/concepts/session-tool2026-05-01T08:00:07.689Zhttps://docs.openclaw.ai/vi/concepts/soul2026-05-01T08:00:07.728Zhttps://docs.openclaw.ai/vi/concepts/streaming2026-05-01T08:00:07.727Zhttps://docs.openclaw.ai/vi/concepts/system-prompt2026-05-01T08:00:07.722Zhttps://docs.openclaw.ai/vi/concepts/timezone2026-05-01T08:00:07.728Zhttps://docs.openclaw.ai/vi/concepts/typebox2026-05-01T08:00:07.721Zhttps://docs.openclaw.ai/vi/concepts/typing-indicators2026-05-01T08:00:07.725Zhttps://docs.openclaw.ai/vi/concepts/usage-tracking2026-05-01T08:00:07.720Zhttps://docs.openclaw.ai/vi/date-time2026-05-01T08:00:07.720Zhttps://docs.openclaw.ai/vi/debug/node-issue2026-05-01T08:00:07.721Zhttps://docs.openclaw.ai/vi/diagnostics/flags2026-05-01T08:00:07.725Zhttps://docs.openclaw.ai/vi/gateway/authentication2026-05-01T08:00:07.761Zhttps://docs.openclaw.ai/vi/gateway/background-process2026-05-01T08:00:07.761Zhttps://docs.openclaw.ai/vi/gateway/bonjour2026-05-01T08:00:07.760Zhttps://docs.openclaw.ai/vi/gateway/bridge-protocol2026-05-01T08:00:07.754Zhttps://docs.openclaw.ai/vi/gateway/cli-backends2026-05-01T08:00:07.758Zhttps://docs.openclaw.ai/vi/gateway/config-agents2026-05-01T10:57:46.445Zhttps://docs.openclaw.ai/vi/gateway/config-channels2026-05-01T10:57:46.452Zhttps://docs.openclaw.ai/vi/gateway/config-tools2026-05-01T10:57:46.443Zhttps://docs.openclaw.ai/vi/gateway/configuration2026-05-01T08:00:07.790Zhttps://docs.openclaw.ai/vi/gateway/configuration-examples2026-05-01T08:00:07.752Zhttps://docs.openclaw.ai/vi/gateway/configuration-reference2026-05-01T08:00:07.750Zhttps://docs.openclaw.ai/vi/gateway/diagnostics2026-05-01T08:00:07.792Zhttps://docs.openclaw.ai/vi/gateway/discovery2026-05-01T08:00:07.790Zhttps://docs.openclaw.ai/vi/gateway/doctor2026-05-01T10:57:50.067Zhttps://docs.openclaw.ai/vi/gateway/gateway-lock2026-05-01T08:00:07.782Zhttps://docs.openclaw.ai/vi/gateway/health2026-05-01T08:00:07.792Zhttps://docs.openclaw.ai/vi/gateway/heartbeat2026-05-01T08:00:07.781Zhttps://docs.openclaw.ai/vi/gateway2026-05-01T08:00:07.791Zhttps://docs.openclaw.ai/vi/gateway/local-models2026-05-01T08:00:07.783Zhttps://docs.openclaw.ai/vi/gateway/logging2026-05-01T10:57:50.055Zhttps://docs.openclaw.ai/vi/gateway/multiple-gateways2026-05-01T08:00:07.820Zhttps://docs.openclaw.ai/vi/gateway/network-model2026-05-01T08:00:07.819Zhttps://docs.openclaw.ai/vi/gateway/openai-http-api2026-05-01T08:00:07.830Zhttps://docs.openclaw.ai/vi/gateway/openresponses-http-api2026-05-01T08:00:07.828Zhttps://docs.openclaw.ai/vi/gateway/openshell2026-05-01T08:00:07.825Zhttps://docs.openclaw.ai/vi/gateway/opentelemetry2026-05-01T08:00:07.829Zhttps://docs.openclaw.ai/vi/gateway/pairing2026-05-01T08:00:07.827Zhttps://docs.openclaw.ai/vi/gateway/prometheus2026-05-01T08:00:07.827Zhttps://docs.openclaw.ai/vi/gateway/protocol2026-05-01T10:57:50.068Zhttps://docs.openclaw.ai/vi/gateway/remote2026-05-01T08:00:07.863Zhttps://docs.openclaw.ai/vi/gateway/remote-gateway-readme2026-05-01T08:00:07.819Zhttps://docs.openclaw.ai/vi/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T08:00:07.862Zhttps://docs.openclaw.ai/vi/gateway/sandboxing2026-05-01T08:00:07.861Zhttps://docs.openclaw.ai/vi/gateway/secrets2026-05-01T08:00:07.853Zhttps://docs.openclaw.ai/vi/gateway/secrets-plan-contract2026-05-01T08:00:07.854Zhttps://docs.openclaw.ai/vi/gateway/security/audit-checks2026-05-01T08:00:07.859Zhttps://docs.openclaw.ai/vi/gateway/security2026-04-30T01:17:44.839Zhttps://docs.openclaw.ai/vi/gateway/tailscale2026-05-01T08:00:07.860Zhttps://docs.openclaw.ai/vi/gateway/tools-invoke-http-api2026-05-01T08:00:07.853Zhttps://docs.openclaw.ai/vi/gateway/troubleshooting2026-05-01T10:57:50.062Zhttps://docs.openclaw.ai/vi/gateway/trusted-proxy-auth2026-05-01T08:00:07.852Zhttps://docs.openclaw.ai/vi/help/debugging2026-05-01T08:00:07.909Zhttps://docs.openclaw.ai/vi/help/environment2026-05-01T08:00:07.896Zhttps://docs.openclaw.ai/vi/help/faq2026-05-01T08:00:07.893Zhttps://docs.openclaw.ai/vi/help/faq-first-run2026-05-01T08:00:07.895Zhttps://docs.openclaw.ai/vi/help/faq-models2026-05-01T08:00:07.894Zhttps://docs.openclaw.ai/vi/help/gpt55-codex-agentic-parity2026-05-01T08:00:07.895Zhttps://docs.openclaw.ai/vi/help/gpt55-codex-agentic-parity-maintainers2026-05-01T08:00:07.884Zhttps://docs.openclaw.ai/vi/help2026-05-01T08:00:07.884Zhttps://docs.openclaw.ai/vi/help/scripts2026-05-01T08:00:07.883Zhttps://docs.openclaw.ai/vi/help/testing2026-05-01T10:57:50.079Zhttps://docs.openclaw.ai/vi/help/testing-live2026-05-01T08:00:07.883Zhttps://docs.openclaw.ai/vi/help/troubleshooting2026-05-01T08:00:07.935Zhttps://docs.openclaw.ai/vi2026-05-01T08:00:07.937Zhttps://docs.openclaw.ai/vi/install/ansible2026-05-01T08:00:07.938Zhttps://docs.openclaw.ai/vi/install/azure2026-05-01T08:00:07.929Zhttps://docs.openclaw.ai/vi/install/bun2026-05-01T08:00:07.936Zhttps://docs.openclaw.ai/vi/install/clawdock2026-05-01T08:00:07.937Zhttps://docs.openclaw.ai/vi/install/development-channels2026-05-01T08:00:07.929Zhttps://docs.openclaw.ai/vi/install/digitalocean2026-05-01T08:00:07.928Zhttps://docs.openclaw.ai/vi/install/docker2026-05-01T08:00:07.997Zhttps://docs.openclaw.ai/vi/install/docker-vm-runtime2026-05-01T08:00:07.927Zhttps://docs.openclaw.ai/vi/install/exe-dev2026-05-01T08:00:07.997Zhttps://docs.openclaw.ai/vi/install/fly2026-05-01T08:00:07.996Zhttps://docs.openclaw.ai/vi/install/gcp2026-05-01T08:00:07.995Zhttps://docs.openclaw.ai/vi/install/hetzner2026-05-01T08:00:07.996Zhttps://docs.openclaw.ai/vi/install/hostinger2026-05-01T08:00:07.993Zhttps://docs.openclaw.ai/vi/install2026-05-01T08:00:07.962Zhttps://docs.openclaw.ai/vi/install/installer2026-05-01T08:00:07.966Zhttps://docs.openclaw.ai/vi/install/kubernetes2026-05-01T08:00:07.994Zhttps://docs.openclaw.ai/vi/install/macos-vm2026-05-01T08:00:07.962Zhttps://docs.openclaw.ai/vi/install/migrating2026-05-01T08:00:08.054Zhttps://docs.openclaw.ai/vi/install/migrating-claude2026-05-01T08:00:08.072Zhttps://docs.openclaw.ai/vi/install/migrating-hermes2026-05-01T08:00:08.061Zhttps://docs.openclaw.ai/vi/install/nix2026-05-01T08:00:08.062Zhttps://docs.openclaw.ai/vi/install/node2026-05-01T08:00:08.059Zhttps://docs.openclaw.ai/vi/install/northflank2026-05-01T08:00:08.059Zhttps://docs.openclaw.ai/vi/install/oracle2026-05-01T08:00:08.054Zhttps://docs.openclaw.ai/vi/install/podman2026-05-01T08:00:08.055Zhttps://docs.openclaw.ai/vi/install/railway2026-05-01T08:00:08.055Zhttps://docs.openclaw.ai/vi/install/raspberry-pi2026-05-01T08:00:08.053Zhttps://docs.openclaw.ai/vi/install/render2026-05-01T08:00:08.092Zhttps://docs.openclaw.ai/vi/install/uninstall2026-05-01T08:00:08.098Zhttps://docs.openclaw.ai/vi/install/updating2026-05-01T10:57:50.066Zhttps://docs.openclaw.ai/vi/logging2026-05-01T10:57:50.063Zhttps://docs.openclaw.ai/vi/network2026-05-01T08:00:08.096Zhttps://docs.openclaw.ai/vi/nodes/audio2026-05-01T08:00:08.100Zhttps://docs.openclaw.ai/vi/nodes/camera2026-05-01T08:00:08.091Zhttps://docs.openclaw.ai/vi/nodes/images2026-05-01T08:00:08.098Zhttps://docs.openclaw.ai/vi/nodes2026-05-01T08:00:08.090Zhttps://docs.openclaw.ai/vi/nodes/location-command2026-05-01T08:00:08.090Zhttps://docs.openclaw.ai/vi/nodes/media-understanding2026-05-01T08:00:08.131Zhttps://docs.openclaw.ai/vi/nodes/talk2026-05-01T08:00:08.130Zhttps://docs.openclaw.ai/vi/nodes/troubleshooting2026-05-01T08:00:08.130Zhttps://docs.openclaw.ai/vi/nodes/voicewake2026-05-01T08:00:08.121Zhttps://docs.openclaw.ai/vi/pi2026-05-01T08:00:08.122Zhttps://docs.openclaw.ai/vi/pi-dev2026-05-01T08:00:08.126Zhttps://docs.openclaw.ai/vi/platforms/android2026-05-01T08:00:08.120Zhttps://docs.openclaw.ai/vi/platforms2026-05-01T08:00:08.159Zhttps://docs.openclaw.ai/vi/platforms/ios2026-05-01T08:00:08.151Zhttps://docs.openclaw.ai/vi/platforms/linux2026-05-01T08:00:08.150Zhttps://docs.openclaw.ai/vi/platforms/mac/bundled-gateway2026-05-01T08:00:08.159Zhttps://docs.openclaw.ai/vi/platforms/mac/canvas2026-05-01T08:00:08.156Zhttps://docs.openclaw.ai/vi/platforms/mac/child-process2026-05-01T08:00:08.151Zhttps://docs.openclaw.ai/vi/platforms/mac/dev-setup2026-05-01T08:00:08.157Zhttps://docs.openclaw.ai/vi/platforms/mac/health2026-05-01T08:00:08.158Zhttps://docs.openclaw.ai/vi/platforms/mac/icon2026-05-01T08:00:08.150Zhttps://docs.openclaw.ai/vi/platforms/mac/logging2026-05-01T08:00:08.196Zhttps://docs.openclaw.ai/vi/platforms/mac/menu-bar2026-05-01T10:57:50.061Zhttps://docs.openclaw.ai/vi/platforms/mac/peekaboo2026-05-01T08:00:08.193Zhttps://docs.openclaw.ai/vi/platforms/mac/permissions2026-05-01T08:00:08.186Zhttps://docs.openclaw.ai/vi/platforms/mac/remote2026-05-01T08:00:08.187Zhttps://docs.openclaw.ai/vi/platforms/mac/signing2026-05-01T08:00:08.191Zhttps://docs.openclaw.ai/vi/platforms/mac/skills2026-05-01T08:00:08.194Zhttps://docs.openclaw.ai/vi/platforms/mac/voice-overlay2026-05-01T08:00:08.194Zhttps://docs.openclaw.ai/vi/platforms/mac/voicewake2026-05-01T08:00:08.187Zhttps://docs.openclaw.ai/vi/platforms/mac/webchat2026-05-01T08:00:08.185Zhttps://docs.openclaw.ai/vi/platforms/mac/xpc2026-05-01T08:00:08.227Zhttps://docs.openclaw.ai/vi/platforms/macos2026-05-01T08:00:08.225Zhttps://docs.openclaw.ai/vi/platforms/windows2026-05-01T08:00:08.226Zhttps://docs.openclaw.ai/vi/plugins/architecture2026-05-01T08:00:08.225Zhttps://docs.openclaw.ai/vi/plugins/architecture-internals2026-05-01T08:00:08.224Zhttps://docs.openclaw.ai/vi/plugins/building-plugins2026-05-01T10:57:50.066Zhttps://docs.openclaw.ai/vi/plugins/bundles2026-05-01T08:00:08.261Zhttps://docs.openclaw.ai/vi/plugins/codex-computer-use2026-05-01T08:00:08.260Zhttps://docs.openclaw.ai/vi/plugins/codex-harness2026-05-01T10:57:50.083Zhttps://docs.openclaw.ai/vi/plugins/community2026-05-01T08:00:08.249Zhttps://docs.openclaw.ai/vi/plugins/compatibility2026-05-01T08:00:08.250Zhttps://docs.openclaw.ai/vi/plugins/dependency-resolution2026-05-01T10:57:52.607Zhttps://docs.openclaw.ai/vi/plugins/google-meet2026-05-01T10:57:52.625Zhttps://docs.openclaw.ai/vi/plugins/hooks2026-05-01T08:00:08.253Zhttps://docs.openclaw.ai/vi/plugins/manifest2026-05-01T08:00:08.258Zhttps://docs.openclaw.ai/vi/plugins/memory-lancedb2026-05-01T08:00:08.249Zhttps://docs.openclaw.ai/vi/plugins/memory-wiki2026-05-01T08:00:08.259Zhttps://docs.openclaw.ai/vi/plugins/message-presentation2026-05-01T08:00:08.299Zhttps://docs.openclaw.ai/vi/plugins/sdk-agent-harness2026-05-01T08:00:08.287Zhttps://docs.openclaw.ai/vi/plugins/sdk-channel-plugins2026-05-01T08:00:08.288Zhttps://docs.openclaw.ai/vi/plugins/sdk-channel-turn2026-05-01T08:00:08.294Zhttps://docs.openclaw.ai/vi/plugins/sdk-entrypoints2026-05-01T08:00:08.302Zhttps://docs.openclaw.ai/vi/plugins/sdk-migration2026-05-01T08:00:08.299Zhttps://docs.openclaw.ai/vi/plugins/sdk-overview2026-05-01T08:00:08.301Zhttps://docs.openclaw.ai/vi/plugins/sdk-provider-plugins2026-05-01T10:57:52.621Zhttps://docs.openclaw.ai/vi/plugins/sdk-runtime2026-05-01T08:00:08.288Zhttps://docs.openclaw.ai/vi/plugins/sdk-setup2026-05-01T08:00:08.300Zhttps://docs.openclaw.ai/vi/plugins/sdk-subpaths2026-05-01T08:00:08.329Zhttps://docs.openclaw.ai/vi/plugins/sdk-testing2026-05-01T08:00:08.331Zhttps://docs.openclaw.ai/vi/plugins/skill-workshop2026-05-01T08:00:08.329Zhttps://docs.openclaw.ai/vi/plugins/voice-call2026-05-01T10:57:52.631Zhttps://docs.openclaw.ai/vi/plugins/webhooks2026-05-01T08:00:08.331Zhttps://docs.openclaw.ai/vi/plugins/zalouser2026-05-01T08:00:08.322Zhttps://docs.openclaw.ai/vi/prose2026-05-01T08:00:08.360Zhttps://docs.openclaw.ai/vi/providers/alibaba2026-05-01T08:00:08.332Zhttps://docs.openclaw.ai/vi/providers/anthropic2026-05-01T08:00:08.321Zhttps://docs.openclaw.ai/vi/providers/arcee2026-05-01T08:00:08.320Zhttps://docs.openclaw.ai/vi/providers/azure-speech2026-05-01T08:00:08.389Zhttps://docs.openclaw.ai/vi/providers/bedrock2026-05-01T08:00:08.387Zhttps://docs.openclaw.ai/vi/providers/bedrock-mantle2026-05-01T08:00:08.386Zhttps://docs.openclaw.ai/vi/providers/chutes2026-05-01T08:00:08.387Zhttps://docs.openclaw.ai/vi/providers/claude-max-api-proxy2026-05-01T08:00:08.388Zhttps://docs.openclaw.ai/vi/providers/cloudflare-ai-gateway2026-05-01T08:00:08.380Zhttps://docs.openclaw.ai/vi/providers/comfy2026-05-01T08:00:08.379Zhttps://docs.openclaw.ai/vi/providers/deepgram2026-05-01T08:00:08.379Zhttps://docs.openclaw.ai/vi/providers/deepinfra2026-05-01T08:00:08.378Zhttps://docs.openclaw.ai/vi/providers/deepseek2026-05-01T08:00:08.418Zhttps://docs.openclaw.ai/vi/providers/elevenlabs2026-05-01T08:00:08.416Zhttps://docs.openclaw.ai/vi/providers/fal2026-05-01T08:00:08.417Zhttps://docs.openclaw.ai/vi/providers/fireworks2026-05-01T08:00:08.411Zhttps://docs.openclaw.ai/vi/providers/github-copilot2026-05-01T08:00:08.410Zhttps://docs.openclaw.ai/vi/providers/glm2026-05-01T08:00:08.413Zhttps://docs.openclaw.ai/vi/providers/google2026-05-01T08:00:08.411Zhttps://docs.openclaw.ai/vi/providers/gradium2026-05-01T08:00:08.410Zhttps://docs.openclaw.ai/vi/providers/groq2026-05-01T08:00:08.409Zhttps://docs.openclaw.ai/vi/providers/huggingface2026-05-01T08:00:08.408Zhttps://docs.openclaw.ai/vi/providers2026-05-01T08:00:08.457Zhttps://docs.openclaw.ai/vi/providers/inferrs2026-05-01T08:00:08.445Zhttps://docs.openclaw.ai/vi/providers/inworld2026-05-01T08:00:08.445Zhttps://docs.openclaw.ai/vi/providers/kilocode2026-05-01T08:00:08.442Zhttps://docs.openclaw.ai/vi/providers/litellm2026-05-01T08:00:08.446Zhttps://docs.openclaw.ai/vi/providers/lmstudio2026-05-01T08:00:08.437Zhttps://docs.openclaw.ai/vi/providers/minimax2026-05-01T08:00:08.438Zhttps://docs.openclaw.ai/vi/providers/mistral2026-05-01T08:00:08.444Zhttps://docs.openclaw.ai/vi/providers/models2026-05-01T08:00:08.437Zhttps://docs.openclaw.ai/vi/providers/moonshot2026-05-01T08:00:08.436Zhttps://docs.openclaw.ai/vi/providers/nvidia2026-05-01T08:00:08.486Zhttps://docs.openclaw.ai/vi/providers/ollama2026-05-01T08:00:08.486Zhttps://docs.openclaw.ai/vi/providers/openai2026-05-01T08:00:08.485Zhttps://docs.openclaw.ai/vi/providers/opencode2026-05-01T08:00:08.477Zhttps://docs.openclaw.ai/vi/providers/opencode-go2026-05-01T08:00:08.485Zhttps://docs.openclaw.ai/vi/providers/openrouter2026-05-01T08:00:08.484Zhttps://docs.openclaw.ai/vi/providers/perplexity-provider2026-05-01T08:00:08.478Zhttps://docs.openclaw.ai/vi/providers/qianfan2026-05-01T08:00:08.477Zhttps://docs.openclaw.ai/vi/providers/qwen2026-05-01T08:00:08.476Zhttps://docs.openclaw.ai/vi/providers/runway2026-05-01T08:00:08.476Zhttps://docs.openclaw.ai/vi/providers/sglang2026-05-01T08:00:08.514Zhttps://docs.openclaw.ai/vi/providers/stepfun2026-05-01T08:00:08.515Zhttps://docs.openclaw.ai/vi/providers/synthetic2026-05-01T08:00:08.514Zhttps://docs.openclaw.ai/vi/providers/tencent2026-05-01T08:00:08.512Zhttps://docs.openclaw.ai/vi/providers/together2026-05-01T08:00:08.507Zhttps://docs.openclaw.ai/vi/providers/venice2026-05-01T08:00:08.508Zhttps://docs.openclaw.ai/vi/providers/vercel-ai-gateway2026-05-01T08:00:08.508Zhttps://docs.openclaw.ai/vi/providers/vllm2026-05-01T08:00:08.506Zhttps://docs.openclaw.ai/vi/providers/volcengine2026-05-01T08:00:08.506Zhttps://docs.openclaw.ai/vi/providers/vydra2026-05-01T08:00:08.557Zhttps://docs.openclaw.ai/vi/providers/xai2026-05-01T08:00:08.546Zhttps://docs.openclaw.ai/vi/providers/xiaomi2026-05-01T08:00:08.545Zhttps://docs.openclaw.ai/vi/providers/zai2026-05-01T08:00:08.544Zhttps://docs.openclaw.ai/vi/reference/AGENTS.default2026-05-01T08:00:08.542Zhttps://docs.openclaw.ai/vi/reference/RELEASING2026-05-01T10:57:52.627Zhttps://docs.openclaw.ai/vi/reference/api-usage-costs2026-05-01T08:00:08.538Zhttps://docs.openclaw.ai/vi/reference/credits2026-05-01T08:00:08.537Zhttps://docs.openclaw.ai/vi/reference/device-models2026-05-01T08:00:08.536Zhttps://docs.openclaw.ai/vi/reference/full-release-validation2026-05-01T10:57:52.619Zhttps://docs.openclaw.ai/vi/reference/memory-config2026-05-01T08:00:08.587Zhttps://docs.openclaw.ai/vi/reference/openclaw-sdk-api-design2026-05-01T08:00:08.585Zhttps://docs.openclaw.ai/vi/reference/prompt-caching2026-05-01T08:00:08.585Zhttps://docs.openclaw.ai/vi/reference/rich-output-protocol2026-05-01T08:00:08.583Zhttps://docs.openclaw.ai/vi/reference/rpc2026-05-01T08:00:08.581Zhttps://docs.openclaw.ai/vi/reference/secretref-credential-surface2026-05-01T10:57:52.618Zhttps://docs.openclaw.ai/vi/reference/session-management-compaction2026-05-01T08:00:08.586Zhttps://docs.openclaw.ai/vi/reference/templates/AGENTS2026-05-01T08:00:08.575Zhttps://docs.openclaw.ai/vi/reference/templates/BOOT2026-05-01T08:00:08.575Zhttps://docs.openclaw.ai/vi/reference/templates/BOOTSTRAP2026-05-01T08:00:08.616Zhttps://docs.openclaw.ai/vi/reference/templates/HEARTBEAT2026-05-01T08:00:08.607Zhttps://docs.openclaw.ai/vi/reference/templates/IDENTITY2026-05-01T08:00:08.614Zhttps://docs.openclaw.ai/vi/reference/templates/SOUL2026-05-01T08:00:08.615Zhttps://docs.openclaw.ai/vi/reference/templates/TOOLS2026-05-01T08:00:08.608Zhttps://docs.openclaw.ai/vi/reference/templates/USER2026-05-01T08:00:08.606Zhttps://docs.openclaw.ai/vi/reference/test2026-05-01T10:57:52.620Zhttps://docs.openclaw.ai/vi/reference/token-use2026-05-01T08:00:08.641Zhttps://docs.openclaw.ai/vi/reference/transcript-hygiene2026-05-01T08:00:08.642Zhttps://docs.openclaw.ai/vi/reference/wizard2026-05-01T08:00:08.642Zhttps://docs.openclaw.ai/vi/security/CONTRIBUTING-THREAT-MODEL2026-05-01T08:00:08.644Zhttps://docs.openclaw.ai/vi/security/THREAT-MODEL-ATLAS2026-05-01T08:00:08.645Zhttps://docs.openclaw.ai/vi/security/formal-verification2026-05-01T08:00:08.635Zhttps://docs.openclaw.ai/vi/security/network-proxy2026-05-01T10:57:52.623Zhttps://docs.openclaw.ai/vi/start/bootstrapping2026-05-01T08:00:08.636Zhttps://docs.openclaw.ai/vi/start/docs-directory2026-05-01T08:00:08.634Zhttps://docs.openclaw.ai/vi/start/getting-started2026-05-01T08:00:08.685Zhttps://docs.openclaw.ai/vi/start/hubs2026-05-01T08:00:08.676Zhttps://docs.openclaw.ai/vi/start/lore2026-05-01T08:00:08.684Zhttps://docs.openclaw.ai/vi/start/onboarding2026-05-01T08:00:08.685Zhttps://docs.openclaw.ai/vi/start/onboarding-overview2026-05-01T08:00:08.684Zhttps://docs.openclaw.ai/vi/start/openclaw2026-05-01T08:00:08.678Zhttps://docs.openclaw.ai/vi/start/setup2026-05-01T08:00:08.678Zhttps://docs.openclaw.ai/vi/start/showcase2026-04-29T23:31:13.798Zhttps://docs.openclaw.ai/vi/start/wizard2026-05-01T08:00:08.715Zhttps://docs.openclaw.ai/vi/start/wizard-cli-automation2026-05-01T08:00:08.682Zhttps://docs.openclaw.ai/vi/start/wizard-cli-reference2026-05-01T08:00:08.715Zhttps://docs.openclaw.ai/vi/tools/acp-agents2026-05-01T10:57:52.622Zhttps://docs.openclaw.ai/vi/tools/acp-agents-setup2026-05-01T08:00:08.713Zhttps://docs.openclaw.ai/vi/tools/agent-send2026-05-01T08:00:08.707Zhttps://docs.openclaw.ai/vi/tools/apply-patch2026-05-01T08:00:08.706Zhttps://docs.openclaw.ai/vi/tools/brave-search2026-05-01T08:00:08.707Zhttps://docs.openclaw.ai/vi/tools/browser2026-05-01T08:00:08.746Zhttps://docs.openclaw.ai/vi/tools/browser-control2026-05-01T08:00:08.705Zhttps://docs.openclaw.ai/vi/tools/browser-linux-troubleshooting2026-05-01T08:00:08.706Zhttps://docs.openclaw.ai/vi/tools/browser-login2026-05-01T08:00:08.757Zhttps://docs.openclaw.ai/vi/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T08:00:08.745Zhttps://docs.openclaw.ai/vi/tools/btw2026-05-01T08:00:08.744Zhttps://docs.openclaw.ai/vi/tools/clawhub2026-05-01T08:00:08.737Zhttps://docs.openclaw.ai/vi/tools/code-execution2026-05-01T08:00:08.744Zhttps://docs.openclaw.ai/vi/tools/creating-skills2026-05-01T08:00:08.736Zhttps://docs.openclaw.ai/vi/tools/diffs2026-05-01T08:00:08.736Zhttps://docs.openclaw.ai/vi/tools/duckduckgo-search2026-05-01T08:00:08.735Zhttps://docs.openclaw.ai/vi/tools/elevated2026-05-01T08:00:08.787Zhttps://docs.openclaw.ai/vi/tools/exa-search2026-05-01T08:00:08.786Zhttps://docs.openclaw.ai/vi/tools/exec2026-05-01T08:00:08.786Zhttps://docs.openclaw.ai/vi/tools/exec-approvals2026-05-01T08:00:08.778Zhttps://docs.openclaw.ai/vi/tools/exec-approvals-advanced2026-05-01T08:00:08.779Zhttps://docs.openclaw.ai/vi/tools/firecrawl2026-05-01T08:00:08.783Zhttps://docs.openclaw.ai/vi/tools/gemini-search2026-05-01T08:00:08.779Zhttps://docs.openclaw.ai/vi/tools/grok-search2026-05-01T08:00:08.785Zhttps://docs.openclaw.ai/vi/tools/image-generation2026-05-01T08:00:08.778Zhttps://docs.openclaw.ai/vi/tools2026-05-01T08:00:08.777Zhttps://docs.openclaw.ai/vi/tools/kimi-search2026-05-01T08:00:08.817Zhttps://docs.openclaw.ai/vi/tools/llm-task2026-05-01T08:00:08.814Zhttps://docs.openclaw.ai/vi/tools/lobster2026-05-01T08:00:08.816Zhttps://docs.openclaw.ai/vi/tools/loop-detection2026-05-01T08:00:08.808Zhttps://docs.openclaw.ai/vi/tools/media-overview2026-05-01T08:00:08.814Zhttps://docs.openclaw.ai/vi/tools/minimax-search2026-05-01T08:00:08.816Zhttps://docs.openclaw.ai/vi/tools/multi-agent-sandbox-tools2026-05-01T08:00:08.815Zhttps://docs.openclaw.ai/vi/tools/music-generation2026-05-01T08:00:08.807Zhttps://docs.openclaw.ai/vi/tools/ollama-search2026-05-01T08:00:08.808Zhttps://docs.openclaw.ai/vi/tools/pdf2026-05-01T08:00:08.806Zhttps://docs.openclaw.ai/vi/tools/perplexity-search2026-05-01T08:00:08.859Zhttps://docs.openclaw.ai/vi/tools/plugin2026-05-01T10:57:55.336Zhttps://docs.openclaw.ai/vi/tools/reactions2026-05-01T08:00:08.847Zhttps://docs.openclaw.ai/vi/tools/searxng-search2026-05-01T08:00:08.840Zhttps://docs.openclaw.ai/vi/tools/skills2026-05-01T08:00:08.846Zhttps://docs.openclaw.ai/vi/tools/skills-config2026-05-01T08:00:08.848Zhttps://docs.openclaw.ai/vi/tools/slash-commands2026-05-01T10:57:55.335Zhttps://docs.openclaw.ai/vi/tools/subagents2026-05-01T08:00:08.839Zhttps://docs.openclaw.ai/vi/tools/tavily2026-05-01T08:00:08.839Zhttps://docs.openclaw.ai/vi/tools/thinking2026-05-01T08:00:08.838Zhttps://docs.openclaw.ai/vi/tools/tokenjuice2026-05-01T08:00:08.892Zhttps://docs.openclaw.ai/vi/tools/trajectory2026-05-01T08:00:08.888Zhttps://docs.openclaw.ai/vi/tools/tts2026-05-01T08:00:08.886Zhttps://docs.openclaw.ai/vi/tools/video-generation2026-05-01T08:00:08.886Zhttps://docs.openclaw.ai/vi/tools/web2026-05-01T08:00:08.879Zhttps://docs.openclaw.ai/vi/tools/web-fetch2026-05-01T08:00:08.889Zhttps://docs.openclaw.ai/vi/vps2026-05-01T08:00:08.889Zhttps://docs.openclaw.ai/vi/web/control-ui2026-05-01T08:00:08.879Zhttps://docs.openclaw.ai/vi/web/dashboard2026-05-01T08:00:08.878Zhttps://docs.openclaw.ai/vi/web2026-05-01T08:00:08.921Zhttps://docs.openclaw.ai/vi/web/tui2026-05-01T08:00:08.918Zhttps://docs.openclaw.ai/vi/web/webchat2026-05-01T08:00:08.911Zhttps://docs.openclaw.ai/vps2026-05-01T08:00:08.917Zhttps://docs.openclaw.ai/web/control-ui2026-05-01T08:00:08.920Zhttps://docs.openclaw.ai/web/dashboard2026-05-01T08:00:08.912Zhttps://docs.openclaw.ai/web2026-05-01T08:00:08.919Zhttps://docs.openclaw.ai/web/tui2026-05-01T08:00:08.919Zhttps://docs.openclaw.ai/web/webchat2026-05-01T08:00:08.911Zhttps://docs.openclaw.ai/zh-CN/AGENTS2026-05-01T08:00:08.910Zhttps://docs.openclaw.ai/zh-CN/automation/cron-jobs2026-05-01T08:00:08.941Zhttps://docs.openclaw.ai/zh-CN/automation/hooks2026-05-01T08:00:08.948Zhttps://docs.openclaw.ai/zh-CN/brave-search2026-05-01T08:00:08.980Zhttps://docs.openclaw.ai/zh-CN/channels/bluebubbles2026-05-01T08:00:08.985Zhttps://docs.openclaw.ai/zh-CN/channels/broadcast-groups2026-05-01T08:00:08.982Zhttps://docs.openclaw.ai/zh-CN/channels/channel-routing2026-05-01T08:00:08.981Zhttps://docs.openclaw.ai/zh-CN/channels/discord2026-05-01T11:56:04.082Zhttps://docs.openclaw.ai/zh-CN/channels/feishu2026-05-01T08:00:08.982Zhttps://docs.openclaw.ai/zh-CN/channels/googlechat2026-05-01T08:00:09.018Zhttps://docs.openclaw.ai/zh-CN/channels/group-messages2026-05-01T08:00:09.017Zhttps://docs.openclaw.ai/zh-CN/channels/groups2026-05-01T08:00:09.018Zhttps://docs.openclaw.ai/zh-CN/channels/imessage2026-05-01T08:00:09.011Zhttps://docs.openclaw.ai/zh-CN/channels2026-05-01T08:00:09.016Zhttps://docs.openclaw.ai/zh-CN/channels/line2026-05-01T08:00:09.016Zhttps://docs.openclaw.ai/zh-CN/channels/location2026-05-01T08:00:09.008Zhttps://docs.openclaw.ai/zh-CN/channels/matrix2026-05-01T08:00:09.071Zhttps://docs.openclaw.ai/zh-CN/channels/mattermost2026-05-01T15:15:58.701Zhttps://docs.openclaw.ai/zh-CN/channels/msteams2026-05-01T08:00:09.068Zhttps://docs.openclaw.ai/zh-CN/channels/nextcloud-talk2026-05-01T08:00:09.070Zhttps://docs.openclaw.ai/zh-CN/channels/nostr2026-05-01T08:00:09.070Zhttps://docs.openclaw.ai/zh-CN/channels/pairing2026-05-01T08:00:09.066Zhttps://docs.openclaw.ai/zh-CN/channels/signal2026-05-01T08:00:09.061Zhttps://docs.openclaw.ai/zh-CN/channels/slack2026-05-01T13:26:40.073Zhttps://docs.openclaw.ai/zh-CN/channels/telegram2026-05-01T08:00:09.100Zhttps://docs.openclaw.ai/zh-CN/channels/tlon2026-05-01T08:00:09.099Zhttps://docs.openclaw.ai/zh-CN/channels/troubleshooting2026-05-01T08:00:09.099Zhttps://docs.openclaw.ai/zh-CN/channels/twitch2026-05-01T08:00:09.101Zhttps://docs.openclaw.ai/zh-CN/channels/whatsapp2026-05-01T08:00:09.092Zhttps://docs.openclaw.ai/zh-CN/channels/zalo2026-05-01T08:00:09.091Zhttps://docs.openclaw.ai/zh-CN/channels/zalouser2026-05-01T08:00:09.090Zhttps://docs.openclaw.ai/zh-CN/cli/acp2026-05-01T08:00:09.130Zhttps://docs.openclaw.ai/zh-CN/cli/agent2026-05-01T08:00:09.122Zhttps://docs.openclaw.ai/zh-CN/cli/agents2026-05-01T08:00:09.130Zhttps://docs.openclaw.ai/zh-CN/cli/approvals2026-05-01T08:00:09.128Zhttps://docs.openclaw.ai/zh-CN/cli/browser2026-05-01T08:00:09.123Zhttps://docs.openclaw.ai/zh-CN/cli/channels2026-05-01T08:01:54.279Zhttps://docs.openclaw.ai/zh-CN/cli/config2026-05-01T08:00:09.168Zhttps://docs.openclaw.ai/zh-CN/cli/configure2026-05-01T08:01:54.309Zhttps://docs.openclaw.ai/zh-CN/cli/cron2026-05-01T08:00:09.164Zhttps://docs.openclaw.ai/zh-CN/cli/dashboard2026-05-01T08:00:09.159Zhttps://docs.openclaw.ai/zh-CN/cli/devices2026-05-01T08:00:09.160Zhttps://docs.openclaw.ai/zh-CN/cli/directory2026-05-01T08:00:09.159Zhttps://docs.openclaw.ai/zh-CN/cli/dns2026-05-01T08:00:09.158Zhttps://docs.openclaw.ai/zh-CN/cli/docs2026-05-01T08:00:09.201Zhttps://docs.openclaw.ai/zh-CN/cli/doctor2026-05-01T15:28:17.396Zhttps://docs.openclaw.ai/zh-CN/cli/gateway2026-05-01T08:01:54.277Zhttps://docs.openclaw.ai/zh-CN/cli/health2026-05-01T08:00:09.199Zhttps://docs.openclaw.ai/zh-CN/cli/hooks2026-05-01T08:00:09.196Zhttps://docs.openclaw.ai/zh-CN/cli2026-05-01T08:00:09.198Zhttps://docs.openclaw.ai/zh-CN/cli/logs2026-05-01T08:00:09.190Zhttps://docs.openclaw.ai/zh-CN/cli/memory2026-05-01T08:00:09.230Zhttps://docs.openclaw.ai/zh-CN/cli/message2026-05-01T08:00:09.229Zhttps://docs.openclaw.ai/zh-CN/cli/models2026-05-01T08:00:09.227Zhttps://docs.openclaw.ai/zh-CN/cli/node2026-05-01T08:00:09.221Zhttps://docs.openclaw.ai/zh-CN/cli/nodes2026-05-01T08:00:09.225Zhttps://docs.openclaw.ai/zh-CN/cli/onboard2026-05-01T08:01:54.314Zhttps://docs.openclaw.ai/zh-CN/cli/pairing2026-05-01T08:00:09.229Zhttps://docs.openclaw.ai/zh-CN/cli/plugins2026-05-01T10:05:21.006Zhttps://docs.openclaw.ai/zh-CN/cli/reset2026-05-01T08:00:09.268Zhttps://docs.openclaw.ai/zh-CN/cli/sandbox2026-05-01T08:00:09.261Zhttps://docs.openclaw.ai/zh-CN/cli/security2026-05-01T15:38:13.053Zhttps://docs.openclaw.ai/zh-CN/cli/sessions2026-05-01T08:00:09.260Zhttps://docs.openclaw.ai/zh-CN/cli/setup2026-05-01T08:00:09.262Zhttps://docs.openclaw.ai/zh-CN/cli/skills2026-05-01T08:00:09.265Zhttps://docs.openclaw.ai/zh-CN/cli/status2026-05-01T08:00:09.261Zhttps://docs.openclaw.ai/zh-CN/cli/system2026-05-01T08:00:09.260Zhttps://docs.openclaw.ai/zh-CN/cli/tui2026-05-01T08:00:09.295Zhttps://docs.openclaw.ai/zh-CN/cli/uninstall2026-05-01T08:00:09.295Zhttps://docs.openclaw.ai/zh-CN/cli/update2026-05-01T09:01:21.067Zhttps://docs.openclaw.ai/zh-CN/cli/voicecall2026-05-01T08:00:09.291Zhttps://docs.openclaw.ai/zh-CN/cli/webhooks2026-05-01T08:00:09.292Zhttps://docs.openclaw.ai/zh-CN/concepts/agent2026-05-01T08:00:09.326Zhttps://docs.openclaw.ai/zh-CN/concepts/agent-loop2026-05-01T08:00:09.288Zhttps://docs.openclaw.ai/zh-CN/concepts/agent-workspace2026-05-01T08:00:09.328Zhttps://docs.openclaw.ai/zh-CN/concepts/architecture2026-05-01T08:00:09.327Zhttps://docs.openclaw.ai/zh-CN/concepts/compaction2026-05-01T08:00:09.323Zhttps://docs.openclaw.ai/zh-CN/concepts/context2026-05-01T08:00:09.319Zhttps://docs.openclaw.ai/zh-CN/concepts/features2026-05-01T08:00:09.369Zhttps://docs.openclaw.ai/zh-CN/concepts/markdown-formatting2026-05-01T08:00:09.362Zhttps://docs.openclaw.ai/zh-CN/concepts/memory2026-05-01T08:00:09.361Zhttps://docs.openclaw.ai/zh-CN/concepts/messages2026-05-01T08:00:09.363Zhttps://docs.openclaw.ai/zh-CN/concepts/model-failover2026-05-01T08:00:09.361Zhttps://docs.openclaw.ai/zh-CN/concepts/model-providers2026-05-01T08:00:09.399Zhttps://docs.openclaw.ai/zh-CN/concepts/models2026-05-01T08:00:09.398Zhttps://docs.openclaw.ai/zh-CN/concepts/multi-agent2026-05-01T08:00:09.399Zhttps://docs.openclaw.ai/zh-CN/concepts/oauth2026-05-01T08:00:09.396Zhttps://docs.openclaw.ai/zh-CN/concepts/presence2026-05-01T08:00:09.389Zhttps://docs.openclaw.ai/zh-CN/concepts/queue2026-05-01T08:00:09.394Zhttps://docs.openclaw.ai/zh-CN/concepts/retry2026-05-01T08:00:09.429Zhttps://docs.openclaw.ai/zh-CN/concepts/session2026-05-01T12:21:04.972Zhttps://docs.openclaw.ai/zh-CN/concepts/session-pruning2026-05-01T08:00:09.428Zhttps://docs.openclaw.ai/zh-CN/concepts/session-tool2026-05-01T08:00:09.427Zhttps://docs.openclaw.ai/zh-CN/concepts/streaming2026-05-01T08:00:09.424Zhttps://docs.openclaw.ai/zh-CN/concepts/system-prompt2026-05-01T08:00:09.419Zhttps://docs.openclaw.ai/zh-CN/concepts/timezone2026-05-01T08:00:09.427Zhttps://docs.openclaw.ai/zh-CN/concepts/typebox2026-05-01T08:00:09.419Zhttps://docs.openclaw.ai/zh-CN/concepts/typing-indicators2026-05-01T08:00:09.418Zhttps://docs.openclaw.ai/zh-CN/concepts/usage-tracking2026-05-01T08:00:09.468Zhttps://docs.openclaw.ai/zh-CN/date-time2026-05-01T08:00:09.468Zhttps://docs.openclaw.ai/zh-CN/debug/node-issue2026-05-01T08:00:09.465Zhttps://docs.openclaw.ai/zh-CN/diagnostics/flags2026-05-01T08:00:09.465Zhttps://docs.openclaw.ai/zh-CN/gateway/authentication2026-05-01T08:00:09.467Zhttps://docs.openclaw.ai/zh-CN/gateway/background-process2026-05-01T08:00:09.459Zhttps://docs.openclaw.ai/zh-CN/gateway/bonjour2026-05-01T08:00:09.460Zhttps://docs.openclaw.ai/zh-CN/gateway/bridge-protocol2026-05-01T08:00:09.461Zhttps://docs.openclaw.ai/zh-CN/gateway/cli-backends2026-05-01T08:00:09.462Zhttps://docs.openclaw.ai/zh-CN/gateway/configuration2026-05-01T11:44:32.469Zhttps://docs.openclaw.ai/zh-CN/gateway/configuration-examples2026-05-01T08:00:09.497Zhttps://docs.openclaw.ai/zh-CN/gateway/discovery2026-05-01T08:00:09.488Zhttps://docs.openclaw.ai/zh-CN/gateway/doctor2026-05-01T08:55:21.034Zhttps://docs.openclaw.ai/zh-CN/gateway/gateway-lock2026-05-01T08:00:09.487Zhttps://docs.openclaw.ai/zh-CN/gateway/health2026-05-01T08:00:09.494Zhttps://docs.openclaw.ai/zh-CN/gateway/heartbeat2026-05-01T08:00:09.531Zhttps://docs.openclaw.ai/zh-CN/gateway2026-05-01T08:00:09.527Zhttps://docs.openclaw.ai/zh-CN/gateway/local-models2026-05-01T08:00:09.530Zhttps://docs.openclaw.ai/zh-CN/gateway/logging2026-05-01T08:00:09.529Zhttps://docs.openclaw.ai/zh-CN/gateway/multiple-gateways2026-05-01T08:00:09.529Zhttps://docs.openclaw.ai/zh-CN/gateway/network-model2026-05-01T08:00:09.526Zhttps://docs.openclaw.ai/zh-CN/gateway/openai-http-api2026-05-01T08:00:09.521Zhttps://docs.openclaw.ai/zh-CN/gateway/openresponses-http-api2026-05-01T08:00:09.521Zhttps://docs.openclaw.ai/zh-CN/gateway/pairing2026-05-01T08:00:09.571Zhttps://docs.openclaw.ai/zh-CN/gateway/protocol2026-05-01T09:01:21.068Zhttps://docs.openclaw.ai/zh-CN/gateway/remote2026-05-01T08:00:09.570Zhttps://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme2026-05-01T08:00:09.564Zhttps://docs.openclaw.ai/zh-CN/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T08:00:09.561Zhttps://docs.openclaw.ai/zh-CN/gateway/sandboxing2026-05-01T11:44:32.465Zhttps://docs.openclaw.ai/zh-CN/gateway/security2026-04-30T01:17:46.866Zhttps://docs.openclaw.ai/zh-CN/gateway/tailscale2026-05-01T08:00:09.607Zhttps://docs.openclaw.ai/zh-CN/gateway/tools-invoke-http-api2026-05-01T08:00:09.608Zhttps://docs.openclaw.ai/zh-CN/gateway/troubleshooting2026-05-01T08:00:09.610Zhttps://docs.openclaw.ai/zh-CN/help/debugging2026-05-01T08:00:09.597Zhttps://docs.openclaw.ai/zh-CN/help/environment2026-05-01T08:00:09.598Zhttps://docs.openclaw.ai/zh-CN/help/faq2026-05-01T08:00:09.605Zhttps://docs.openclaw.ai/zh-CN/help2026-05-01T08:00:09.648Zhttps://docs.openclaw.ai/zh-CN/help/scripts2026-05-01T08:00:09.648Zhttps://docs.openclaw.ai/zh-CN/help/testing2026-05-01T08:34:18.737Zhttps://docs.openclaw.ai/zh-CN/help/troubleshooting2026-05-01T08:00:09.645Zhttps://docs.openclaw.ai/zh-CN2026-05-01T08:00:09.647Zhttps://docs.openclaw.ai/zh-CN/install/ansible2026-05-01T11:44:32.464Zhttps://docs.openclaw.ai/zh-CN/install/bun2026-05-01T08:00:09.636Zhttps://docs.openclaw.ai/zh-CN/install/development-channels2026-05-01T08:00:09.693Zhttps://docs.openclaw.ai/zh-CN/install/docker2026-05-01T11:44:32.492Zhttps://docs.openclaw.ai/zh-CN/install/exe-dev2026-05-01T08:00:09.700Zhttps://docs.openclaw.ai/zh-CN/install/fly2026-05-01T08:00:09.694Zhttps://docs.openclaw.ai/zh-CN/install/gcp2026-05-01T08:00:09.694Zhttps://docs.openclaw.ai/zh-CN/install/hetzner2026-05-01T08:00:09.692Zhttps://docs.openclaw.ai/zh-CN/install2026-05-01T08:00:09.728Zhttps://docs.openclaw.ai/zh-CN/install/installer2026-05-01T08:00:09.730Zhttps://docs.openclaw.ai/zh-CN/install/macos-vm2026-05-01T08:00:09.721Zhttps://docs.openclaw.ai/zh-CN/install/migrating2026-05-01T08:00:09.721Zhttps://docs.openclaw.ai/zh-CN/install/nix2026-05-01T08:00:09.720Zhttps://docs.openclaw.ai/zh-CN/install/node2026-05-01T08:00:09.726Zhttps://docs.openclaw.ai/zh-CN/install/northflank2026-05-01T08:00:09.727Zhttps://docs.openclaw.ai/zh-CN/install/railway2026-05-01T08:00:09.777Zhttps://docs.openclaw.ai/zh-CN/install/render2026-05-01T08:00:09.772Zhttps://docs.openclaw.ai/zh-CN/install/uninstall2026-05-01T08:00:09.779Zhttps://docs.openclaw.ai/zh-CN/install/updating2026-05-01T09:01:21.063Zhttps://docs.openclaw.ai/zh-CN/logging2026-05-01T08:00:09.776Zhttps://docs.openclaw.ai/zh-CN/network2026-05-01T08:00:09.775Zhttps://docs.openclaw.ai/zh-CN/nodes/audio2026-05-01T08:00:09.774Zhttps://docs.openclaw.ai/zh-CN/nodes/camera2026-05-01T08:00:09.812Zhttps://docs.openclaw.ai/zh-CN/nodes/images2026-05-01T08:00:09.810Zhttps://docs.openclaw.ai/zh-CN/nodes2026-05-01T08:00:09.811Zhttps://docs.openclaw.ai/zh-CN/nodes/location-command2026-05-01T08:00:09.808Zhttps://docs.openclaw.ai/zh-CN/nodes/media-understanding2026-05-01T08:00:09.812Zhttps://docs.openclaw.ai/zh-CN/nodes/talk2026-05-01T08:00:09.803Zhttps://docs.openclaw.ai/zh-CN/nodes/troubleshooting2026-05-01T08:00:09.803Zhttps://docs.openclaw.ai/zh-CN/nodes/voicewake2026-05-01T08:00:09.802Zhttps://docs.openclaw.ai/zh-CN/perplexity2026-05-01T08:00:09.801Zhttps://docs.openclaw.ai/zh-CN/pi2026-05-01T08:00:09.840Zhttps://docs.openclaw.ai/zh-CN/pi-dev2026-05-01T08:00:09.810Zhttps://docs.openclaw.ai/zh-CN/platforms/android2026-05-01T08:00:09.841Zhttps://docs.openclaw.ai/zh-CN/platforms/digitalocean2026-05-01T08:00:09.839Zhttps://docs.openclaw.ai/zh-CN/platforms2026-05-01T08:00:09.836Zhttps://docs.openclaw.ai/zh-CN/platforms/ios2026-05-01T08:00:09.831Zhttps://docs.openclaw.ai/zh-CN/platforms/linux2026-05-01T08:00:09.832Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/bundled-gateway2026-05-01T08:00:09.832Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/canvas2026-05-01T08:00:09.830Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/child-process2026-05-01T08:00:09.880Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup2026-05-01T08:00:09.879Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/health2026-05-01T08:00:09.879Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/icon2026-05-01T08:00:09.880Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/logging2026-05-01T08:00:09.872Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/menu-bar2026-05-01T08:00:09.877Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/peekaboo2026-05-01T08:00:09.872Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/permissions2026-05-01T08:00:09.871Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/remote2026-05-01T08:00:09.873Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/signing2026-05-01T08:00:09.871Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/skills2026-05-01T08:00:09.909Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/voice-overlay2026-05-01T08:00:09.908Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/voicewake2026-05-01T08:00:09.906Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/webchat2026-05-01T08:00:09.907Zhttps://docs.openclaw.ai/zh-CN/platforms/mac/xpc2026-05-01T08:00:09.904Zhttps://docs.openclaw.ai/zh-CN/platforms/macos2026-05-01T08:00:09.900Zhttps://docs.openclaw.ai/zh-CN/platforms/oracle2026-05-01T08:00:09.900Zhttps://docs.openclaw.ai/zh-CN/platforms/raspberry-pi2026-05-01T08:00:09.907Zhttps://docs.openclaw.ai/zh-CN/platforms/windows2026-05-01T08:00:09.899Zhttps://docs.openclaw.ai/zh-CN/plugins/agent-tools2026-05-01T08:00:09.898Zhttps://docs.openclaw.ai/zh-CN/plugins/manifest2026-05-01T08:00:09.963Zhttps://docs.openclaw.ai/zh-CN/plugins/voice-call2026-05-01T11:44:32.473Zhttps://docs.openclaw.ai/zh-CN/plugins/zalouser2026-05-01T08:00:10.010Zhttps://docs.openclaw.ai/zh-CN/prose2026-05-01T08:00:10.054Zhttps://docs.openclaw.ai/zh-CN/providers/anthropic2026-05-01T08:00:10.055Zhttps://docs.openclaw.ai/zh-CN/providers/bedrock2026-05-01T08:00:10.051Zhttps://docs.openclaw.ai/zh-CN/providers/claude-max-api-proxy2026-05-01T08:00:10.045Zhttps://docs.openclaw.ai/zh-CN/providers/deepgram2026-05-01T08:00:10.090Zhttps://docs.openclaw.ai/zh-CN/providers/github-copilot2026-05-01T08:00:10.073Zhttps://docs.openclaw.ai/zh-CN/providers/glm2026-05-01T08:00:10.072Zhttps://docs.openclaw.ai/zh-CN/providers2026-05-01T08:00:10.125Zhttps://docs.openclaw.ai/zh-CN/providers/minimax2026-05-01T08:00:10.157Zhttps://docs.openclaw.ai/zh-CN/providers/models2026-05-01T08:00:10.147Zhttps://docs.openclaw.ai/zh-CN/providers/moonshot2026-05-01T08:00:10.147Zhttps://docs.openclaw.ai/zh-CN/providers/ollama2026-05-01T08:00:10.146Zhttps://docs.openclaw.ai/zh-CN/providers/openai2026-05-01T08:00:10.155Zhttps://docs.openclaw.ai/zh-CN/providers/opencode2026-05-01T08:00:10.148Zhttps://docs.openclaw.ai/zh-CN/providers/openrouter2026-05-01T08:00:10.152Zhttps://docs.openclaw.ai/zh-CN/providers/qianfan2026-05-01T08:00:10.183Zhttps://docs.openclaw.ai/zh-CN/providers/qwen2026-05-01T08:00:10.183Zhttps://docs.openclaw.ai/zh-CN/providers/synthetic2026-05-01T08:00:10.175Zhttps://docs.openclaw.ai/zh-CN/providers/venice2026-05-01T13:00:43.406Zhttps://docs.openclaw.ai/zh-CN/providers/vercel-ai-gateway2026-05-01T08:00:10.225Zhttps://docs.openclaw.ai/zh-CN/providers/xiaomi2026-05-01T08:00:10.224Zhttps://docs.openclaw.ai/zh-CN/providers/zai2026-05-01T13:17:22.269Zhttps://docs.openclaw.ai/zh-CN/reference/AGENTS.default2026-05-01T08:00:10.215Zhttps://docs.openclaw.ai/zh-CN/reference/RELEASING2026-05-01T08:00:10.214Zhttps://docs.openclaw.ai/zh-CN/reference/api-usage-costs2026-05-01T08:00:10.255Zhttps://docs.openclaw.ai/zh-CN/reference/credits2026-05-01T08:00:10.254Zhttps://docs.openclaw.ai/zh-CN/reference/device-models2026-05-01T08:00:10.248Zhttps://docs.openclaw.ai/zh-CN/reference/rpc2026-05-01T08:00:10.252Zhttps://docs.openclaw.ai/zh-CN/reference/session-management-compaction2026-05-01T12:21:04.970Zhttps://docs.openclaw.ai/zh-CN/reference/templates/AGENTS2026-05-01T08:00:10.275Zhttps://docs.openclaw.ai/zh-CN/reference/templates/BOOT2026-05-01T08:00:10.280Zhttps://docs.openclaw.ai/zh-CN/reference/templates/BOOTSTRAP2026-05-01T08:00:10.278Zhttps://docs.openclaw.ai/zh-CN/reference/templates/HEARTBEAT2026-05-01T08:00:10.277Zhttps://docs.openclaw.ai/zh-CN/reference/templates/IDENTITY2026-05-01T08:00:10.277Zhttps://docs.openclaw.ai/zh-CN/reference/templates/SOUL2026-05-01T08:00:10.324Zhttps://docs.openclaw.ai/zh-CN/reference/templates/TOOLS2026-05-01T08:00:10.322Zhttps://docs.openclaw.ai/zh-CN/reference/templates/USER2026-05-01T08:00:10.321Zhttps://docs.openclaw.ai/zh-CN/reference/test2026-05-01T08:34:18.727Zhttps://docs.openclaw.ai/zh-CN/reference/token-use2026-05-01T08:00:10.322Zhttps://docs.openclaw.ai/zh-CN/reference/transcript-hygiene2026-05-01T08:00:10.315Zhttps://docs.openclaw.ai/zh-CN/reference/wizard2026-05-01T08:00:10.314Zhttps://docs.openclaw.ai/zh-CN/security/formal-verification2026-05-01T08:00:10.354Zhttps://docs.openclaw.ai/zh-CN/start/bootstrapping2026-05-01T08:00:10.347Zhttps://docs.openclaw.ai/zh-CN/start/docs-directory2026-05-01T08:00:10.352Zhttps://docs.openclaw.ai/zh-CN/start/getting-started2026-05-01T08:00:10.353Zhttps://docs.openclaw.ai/zh-CN/start/hubs2026-05-01T08:00:10.347Zhttps://docs.openclaw.ai/zh-CN/start/lore2026-05-01T08:00:10.348Zhttps://docs.openclaw.ai/zh-CN/start/onboarding2026-05-01T08:00:10.346Zhttps://docs.openclaw.ai/zh-CN/start/openclaw2026-05-01T08:00:10.385Zhttps://docs.openclaw.ai/zh-CN/start/setup2026-05-01T08:00:10.384Zhttps://docs.openclaw.ai/zh-CN/start/showcase2026-04-24T04:24:55.444Zhttps://docs.openclaw.ai/zh-CN/start/wizard2026-05-01T08:00:10.377Zhttps://docs.openclaw.ai/zh-CN/tools/agent-send2026-05-01T08:00:10.422Zhttps://docs.openclaw.ai/zh-CN/tools/apply-patch2026-05-01T08:00:10.421Zhttps://docs.openclaw.ai/zh-CN/tools/browser2026-05-01T08:00:10.413Zhttps://docs.openclaw.ai/zh-CN/tools/browser-linux-troubleshooting2026-05-01T08:00:10.415Zhttps://docs.openclaw.ai/zh-CN/tools/browser-login2026-05-01T08:00:10.414Zhttps://docs.openclaw.ai/zh-CN/tools/clawhub2026-05-01T08:00:10.473Zhttps://docs.openclaw.ai/zh-CN/tools/creating-skills2026-05-01T08:00:10.473Zhttps://docs.openclaw.ai/zh-CN/tools/elevated2026-05-01T08:00:10.474Zhttps://docs.openclaw.ai/zh-CN/tools/exec2026-05-01T08:00:10.465Zhttps://docs.openclaw.ai/zh-CN/tools/exec-approvals2026-05-01T08:00:10.465Zhttps://docs.openclaw.ai/zh-CN/tools/firecrawl2026-05-01T08:00:10.502Zhttps://docs.openclaw.ai/zh-CN/tools2026-05-01T08:00:10.502Zhttps://docs.openclaw.ai/zh-CN/tools/llm-task2026-05-01T08:00:10.501Zhttps://docs.openclaw.ai/zh-CN/tools/lobster2026-05-01T08:00:10.494Zhttps://docs.openclaw.ai/zh-CN/tools/multi-agent-sandbox-tools2026-05-01T08:00:10.532Zhttps://docs.openclaw.ai/zh-CN/tools/plugin2026-05-01T10:05:21.004Zhttps://docs.openclaw.ai/zh-CN/tools/reactions2026-05-01T08:00:10.524Zhttps://docs.openclaw.ai/zh-CN/tools/skills2026-05-01T08:00:10.571Zhttps://docs.openclaw.ai/zh-CN/tools/skills-config2026-05-01T08:00:10.526Zhttps://docs.openclaw.ai/zh-CN/tools/slash-commands2026-05-01T10:05:21.007Zhttps://docs.openclaw.ai/zh-CN/tools/subagents2026-05-01T08:00:10.571Zhttps://docs.openclaw.ai/zh-CN/tools/thinking2026-05-01T08:00:10.557Zhttps://docs.openclaw.ai/zh-CN/tools/web2026-05-01T08:00:10.599Zhttps://docs.openclaw.ai/zh-CN/tts2026-05-01T08:00:10.598Zhttps://docs.openclaw.ai/zh-CN/vps2026-05-01T08:00:10.602Zhttps://docs.openclaw.ai/zh-CN/web/control-ui2026-05-01T08:00:10.594Zhttps://docs.openclaw.ai/zh-CN/web/dashboard2026-05-01T08:00:10.596Zhttps://docs.openclaw.ai/zh-CN/web2026-05-01T08:00:10.595Zhttps://docs.openclaw.ai/zh-CN/web/tui2026-05-01T08:00:10.595Zhttps://docs.openclaw.ai/zh-CN/web/webchat2026-05-01T08:00:10.601Zhttps://docs.openclaw.ai/zh-TW/auth-credential-semantics2026-05-01T08:00:10.588Zhttps://docs.openclaw.ai/zh-TW/automation/cron-jobs2026-05-01T08:00:10.628Zhttps://docs.openclaw.ai/zh-TW/automation/hooks2026-05-01T08:00:10.621Zhttps://docs.openclaw.ai/zh-TW/automation2026-05-01T08:00:10.620Zhttps://docs.openclaw.ai/zh-TW/automation/standing-orders2026-05-01T08:00:10.628Zhttps://docs.openclaw.ai/zh-TW/automation/taskflow2026-05-01T08:00:10.626Zhttps://docs.openclaw.ai/zh-TW/automation/tasks2026-05-01T08:00:10.670Zhttps://docs.openclaw.ai/zh-TW/channels/bluebubbles2026-05-01T08:00:10.653Zhttps://docs.openclaw.ai/zh-TW/channels/broadcast-groups2026-05-01T08:00:10.657Zhttps://docs.openclaw.ai/zh-TW/channels/channel-routing2026-05-01T08:00:10.652Zhttps://docs.openclaw.ai/zh-TW/channels/discord2026-05-01T08:00:10.653Zhttps://docs.openclaw.ai/zh-TW/channels/feishu2026-05-01T08:00:10.652Zhttps://docs.openclaw.ai/zh-TW/channels/googlechat2026-05-01T08:00:10.658Zhttps://docs.openclaw.ai/zh-TW/channels/group-messages2026-05-01T08:00:10.701Zhttps://docs.openclaw.ai/zh-TW/channels/groups2026-05-01T08:00:10.701Zhttps://docs.openclaw.ai/zh-TW/channels/imessage2026-05-01T08:00:10.700Zhttps://docs.openclaw.ai/zh-TW/channels2026-05-01T08:00:10.694Zhttps://docs.openclaw.ai/zh-TW/channels/irc2026-05-01T08:00:10.698Zhttps://docs.openclaw.ai/zh-TW/channels/line2026-05-01T08:00:10.694Zhttps://docs.openclaw.ai/zh-TW/channels/location2026-05-01T08:00:10.692Zhttps://docs.openclaw.ai/zh-TW/channels/matrix2026-05-01T08:00:10.692Zhttps://docs.openclaw.ai/zh-TW/channels/matrix-migration2026-05-01T08:00:10.693Zhttps://docs.openclaw.ai/zh-TW/channels/matrix-push-rules2026-05-01T08:00:10.693Zhttps://docs.openclaw.ai/zh-TW/channels/mattermost2026-05-01T08:00:10.732Zhttps://docs.openclaw.ai/zh-TW/channels/msteams2026-05-01T08:00:10.731Zhttps://docs.openclaw.ai/zh-TW/channels/nextcloud-talk2026-05-01T08:00:10.731Zhttps://docs.openclaw.ai/zh-TW/channels/nostr2026-05-01T08:00:10.721Zhttps://docs.openclaw.ai/zh-TW/channels/pairing2026-05-01T08:00:10.723Zhttps://docs.openclaw.ai/zh-TW/channels/qa-channel2026-05-01T08:00:10.730Zhttps://docs.openclaw.ai/zh-TW/channels/qqbot2026-05-01T08:00:10.724Zhttps://docs.openclaw.ai/zh-TW/channels/signal2026-05-01T08:00:10.723Zhttps://docs.openclaw.ai/zh-TW/channels/slack2026-05-01T08:00:10.729Zhttps://docs.openclaw.ai/zh-TW/channels/synology-chat2026-05-01T08:00:10.722Zhttps://docs.openclaw.ai/zh-TW/channels/telegram2026-05-01T08:00:10.774Zhttps://docs.openclaw.ai/zh-TW/channels/tlon2026-05-01T08:00:10.773Zhttps://docs.openclaw.ai/zh-TW/channels/troubleshooting2026-05-01T08:00:10.761Zhttps://docs.openclaw.ai/zh-TW/channels/twitch2026-05-01T08:00:10.755Zhttps://docs.openclaw.ai/zh-TW/channels/wechat2026-05-01T08:00:10.759Zhttps://docs.openclaw.ai/zh-TW/channels/whatsapp2026-05-01T08:00:10.755Zhttps://docs.openclaw.ai/zh-TW/channels/yuanbao2026-05-01T08:00:10.773Zhttps://docs.openclaw.ai/zh-TW/channels/zalo2026-05-01T08:00:10.754Zhttps://docs.openclaw.ai/zh-TW/channels/zalouser2026-05-01T08:00:10.754Zhttps://docs.openclaw.ai/zh-TW/ci2026-05-01T08:00:10.753Zhttps://docs.openclaw.ai/zh-TW/cli/acp2026-05-01T08:00:10.803Zhttps://docs.openclaw.ai/zh-TW/cli/agent2026-05-01T08:00:10.795Zhttps://docs.openclaw.ai/zh-TW/cli/agents2026-05-01T08:00:10.801Zhttps://docs.openclaw.ai/zh-TW/cli/approvals2026-05-01T08:00:10.798Zhttps://docs.openclaw.ai/zh-TW/cli/backup2026-05-01T08:00:10.801Zhttps://docs.openclaw.ai/zh-TW/cli/browser2026-05-01T08:00:10.802Zhttps://docs.openclaw.ai/zh-TW/cli/channels2026-05-01T08:00:10.794Zhttps://docs.openclaw.ai/zh-TW/cli/clawbot2026-05-01T08:00:10.795Zhttps://docs.openclaw.ai/zh-TW/cli/commitments2026-05-01T08:00:10.794Zhttps://docs.openclaw.ai/zh-TW/cli/completion2026-05-01T08:00:10.793Zhttps://docs.openclaw.ai/zh-TW/cli/config2026-05-01T08:00:10.834Zhttps://docs.openclaw.ai/zh-TW/cli/configure2026-05-01T08:00:10.831Zhttps://docs.openclaw.ai/zh-TW/cli/cron2026-05-01T08:00:10.830Zhttps://docs.openclaw.ai/zh-TW/cli/daemon2026-05-01T08:00:10.827Zhttps://docs.openclaw.ai/zh-TW/cli/dashboard2026-05-01T08:00:10.831Zhttps://docs.openclaw.ai/zh-TW/cli/devices2026-05-01T08:00:10.823Zhttps://docs.openclaw.ai/zh-TW/cli/directory2026-05-01T08:00:10.823Zhttps://docs.openclaw.ai/zh-TW/cli/dns2026-05-01T08:00:10.822Zhttps://docs.openclaw.ai/zh-TW/cli/docs2026-05-01T08:00:10.821Zhttps://docs.openclaw.ai/zh-TW/cli/doctor2026-05-01T08:00:10.863Zhttps://docs.openclaw.ai/zh-TW/cli/flows2026-05-01T08:00:10.863Zhttps://docs.openclaw.ai/zh-TW/cli/gateway2026-05-01T08:00:10.855Zhttps://docs.openclaw.ai/zh-TW/cli/health2026-05-01T08:00:10.862Zhttps://docs.openclaw.ai/zh-TW/cli/hooks2026-05-01T08:00:10.862Zhttps://docs.openclaw.ai/zh-TW/cli2026-05-01T08:00:10.860Zhttps://docs.openclaw.ai/zh-TW/cli/infer2026-05-01T08:00:10.855Zhttps://docs.openclaw.ai/zh-TW/cli/logs2026-05-01T08:00:10.856Zhttps://docs.openclaw.ai/zh-TW/cli/mcp2026-05-01T08:00:10.854Zhttps://docs.openclaw.ai/zh-TW/cli/memory2026-05-01T08:00:10.854Zhttps://docs.openclaw.ai/zh-TW/cli/message2026-05-01T08:00:10.898Zhttps://docs.openclaw.ai/zh-TW/cli/migrate2026-05-01T08:00:10.897Zhttps://docs.openclaw.ai/zh-TW/cli/models2026-05-01T08:00:10.897Zhttps://docs.openclaw.ai/zh-TW/cli/node2026-05-01T08:00:10.894Zhttps://docs.openclaw.ai/zh-TW/cli/nodes2026-05-01T08:00:10.899Zhttps://docs.openclaw.ai/zh-TW/cli/onboard2026-05-01T08:00:10.890Zhttps://docs.openclaw.ai/zh-TW/cli/pairing2026-05-01T08:00:10.898Zhttps://docs.openclaw.ai/zh-TW/cli/plugins2026-05-01T08:00:10.890Zhttps://docs.openclaw.ai/zh-TW/cli/proxy2026-05-01T08:00:10.889Zhttps://docs.openclaw.ai/zh-TW/cli/qr2026-05-01T08:00:10.889Zhttps://docs.openclaw.ai/zh-TW/cli/reset2026-05-01T08:00:10.929Zhttps://docs.openclaw.ai/zh-TW/cli/sandbox2026-05-01T08:00:10.927Zhttps://docs.openclaw.ai/zh-TW/cli/secrets2026-05-01T08:00:10.920Zhttps://docs.openclaw.ai/zh-TW/cli/security2026-05-01T08:00:10.928Zhttps://docs.openclaw.ai/zh-TW/cli/sessions2026-05-01T08:00:10.922Zhttps://docs.openclaw.ai/zh-TW/cli/setup2026-05-01T08:00:10.924Zhttps://docs.openclaw.ai/zh-TW/cli/skills2026-05-01T08:00:10.921Zhttps://docs.openclaw.ai/zh-TW/cli/status2026-05-01T08:00:10.921Zhttps://docs.openclaw.ai/zh-TW/cli/system2026-05-01T08:00:10.920Zhttps://docs.openclaw.ai/zh-TW/cli/tasks2026-05-01T08:00:10.919Zhttps://docs.openclaw.ai/zh-TW/cli/tui2026-05-01T08:00:10.959Zhttps://docs.openclaw.ai/zh-TW/cli/uninstall2026-05-01T08:00:10.957Zhttps://docs.openclaw.ai/zh-TW/cli/update2026-05-01T08:00:10.958Zhttps://docs.openclaw.ai/zh-TW/cli/voicecall2026-05-01T08:00:10.955Zhttps://docs.openclaw.ai/zh-TW/cli/webhooks2026-05-01T08:00:10.952Zhttps://docs.openclaw.ai/zh-TW/cli/wiki2026-05-01T08:00:10.951Zhttps://docs.openclaw.ai/zh-TW/concepts/active-memory2026-05-01T08:00:10.952Zhttps://docs.openclaw.ai/zh-TW/concepts/agent2026-05-01T08:00:11.000Zhttps://docs.openclaw.ai/zh-TW/concepts/agent-loop2026-05-01T08:00:10.951Zhttps://docs.openclaw.ai/zh-TW/concepts/agent-runtimes2026-05-01T08:00:10.950Zhttps://docs.openclaw.ai/zh-TW/concepts/agent-workspace2026-05-01T08:00:10.950Zhttps://docs.openclaw.ai/zh-TW/concepts/architecture2026-05-01T08:00:10.998Zhttps://docs.openclaw.ai/zh-TW/concepts/channel-docking2026-05-01T08:00:10.998Zhttps://docs.openclaw.ai/zh-TW/concepts/commitments2026-05-01T08:00:10.994Zhttps://docs.openclaw.ai/zh-TW/concepts/compaction2026-05-01T08:00:10.992Zhttps://docs.openclaw.ai/zh-TW/concepts/context2026-05-01T08:00:10.991Zhttps://docs.openclaw.ai/zh-TW/concepts/context-engine2026-05-01T08:00:10.994Zhttps://docs.openclaw.ai/zh-TW/concepts/delegate-architecture2026-05-01T08:00:10.991Zhttps://docs.openclaw.ai/zh-TW/concepts/dreaming2026-05-01T08:00:10.990Zhttps://docs.openclaw.ai/zh-TW/concepts/experimental-features2026-05-01T08:00:10.989Zhttps://docs.openclaw.ai/zh-TW/concepts/features2026-05-01T08:00:11.028Zhttps://docs.openclaw.ai/zh-TW/concepts/markdown-formatting2026-05-01T08:00:11.027Zhttps://docs.openclaw.ai/zh-TW/concepts/memory2026-05-01T08:00:11.020Zhttps://docs.openclaw.ai/zh-TW/concepts/memory-builtin2026-05-01T08:00:11.026Zhttps://docs.openclaw.ai/zh-TW/concepts/memory-honcho2026-05-01T08:00:11.023Zhttps://docs.openclaw.ai/zh-TW/concepts/memory-qmd2026-05-01T08:00:11.030Zhttps://docs.openclaw.ai/zh-TW/concepts/memory-search2026-05-01T08:00:11.030Zhttps://docs.openclaw.ai/zh-TW/concepts/messages2026-05-01T08:00:11.029Zhttps://docs.openclaw.ai/zh-TW/concepts/model-failover2026-05-01T08:00:11.019Zhttps://docs.openclaw.ai/zh-TW/concepts/model-providers2026-05-01T08:00:11.028Zhttps://docs.openclaw.ai/zh-TW/concepts/models2026-05-01T08:00:11.059Zhttps://docs.openclaw.ai/zh-TW/concepts/multi-agent2026-05-01T08:00:11.058Zhttps://docs.openclaw.ai/zh-TW/concepts/oauth2026-05-01T08:00:11.049Zhttps://docs.openclaw.ai/zh-TW/concepts/openclaw-sdk2026-05-01T08:00:11.060Zhttps://docs.openclaw.ai/zh-TW/concepts/presence2026-05-01T08:00:11.056Zhttps://docs.openclaw.ai/zh-TW/concepts/qa-e2e-automation2026-05-01T08:00:11.059Zhttps://docs.openclaw.ai/zh-TW/concepts/qa-matrix2026-05-01T08:00:11.057Zhttps://docs.openclaw.ai/zh-TW/concepts/queue2026-05-01T08:00:11.050Zhttps://docs.openclaw.ai/zh-TW/concepts/queue-steering2026-05-01T08:00:11.049Zhttps://docs.openclaw.ai/zh-TW/concepts/retry2026-05-01T08:00:11.050Zhttps://docs.openclaw.ai/zh-TW/concepts/session2026-05-01T08:00:11.096Zhttps://docs.openclaw.ai/zh-TW/concepts/session-pruning2026-05-01T08:00:11.096Zhttps://docs.openclaw.ai/zh-TW/concepts/session-tool2026-05-01T08:00:11.099Zhttps://docs.openclaw.ai/zh-TW/concepts/soul2026-05-01T08:00:11.092Zhttps://docs.openclaw.ai/zh-TW/concepts/streaming2026-05-01T08:00:11.099Zhttps://docs.openclaw.ai/zh-TW/concepts/system-prompt2026-05-01T08:00:11.090Zhttps://docs.openclaw.ai/zh-TW/concepts/timezone2026-05-01T08:00:11.098Zhttps://docs.openclaw.ai/zh-TW/concepts/typebox2026-05-01T08:00:11.091Zhttps://docs.openclaw.ai/zh-TW/concepts/typing-indicators2026-05-01T08:00:11.092Zhttps://docs.openclaw.ai/zh-TW/concepts/usage-tracking2026-05-01T08:00:11.091Zhttps://docs.openclaw.ai/zh-TW/date-time2026-05-01T08:00:11.151Zhttps://docs.openclaw.ai/zh-TW/debug/node-issue2026-05-01T08:00:11.151Zhttps://docs.openclaw.ai/zh-TW/diagnostics/flags2026-05-01T08:00:11.135Zhttps://docs.openclaw.ai/zh-TW/gateway/authentication2026-05-01T08:00:11.152Zhttps://docs.openclaw.ai/zh-TW/gateway/background-process2026-05-01T08:00:11.135Zhttps://docs.openclaw.ai/zh-TW/gateway/bonjour2026-05-01T08:00:11.136Zhttps://docs.openclaw.ai/zh-TW/gateway/bridge-protocol2026-05-01T08:00:11.136Zhttps://docs.openclaw.ai/zh-TW/gateway/cli-backends2026-05-01T08:00:11.134Zhttps://docs.openclaw.ai/zh-TW/gateway/config-agents2026-05-01T08:00:11.152Zhttps://docs.openclaw.ai/zh-TW/gateway/config-channels2026-05-01T08:00:11.137Zhttps://docs.openclaw.ai/zh-TW/gateway/config-tools2026-05-01T08:00:11.186Zhttps://docs.openclaw.ai/zh-TW/gateway/configuration2026-05-01T08:00:11.181Zhttps://docs.openclaw.ai/zh-TW/gateway/configuration-examples2026-05-01T08:00:11.180Zhttps://docs.openclaw.ai/zh-TW/gateway/configuration-reference2026-05-01T08:00:11.179Zhttps://docs.openclaw.ai/zh-TW/gateway/diagnostics2026-05-01T08:00:11.171Zhttps://docs.openclaw.ai/zh-TW/gateway/discovery2026-05-01T08:00:11.179Zhttps://docs.openclaw.ai/zh-TW/gateway/doctor2026-05-01T08:00:11.172Zhttps://docs.openclaw.ai/zh-TW/gateway/gateway-lock2026-05-01T08:00:11.182Zhttps://docs.openclaw.ai/zh-TW/gateway/health2026-05-01T08:00:11.182Zhttps://docs.openclaw.ai/zh-TW/gateway/heartbeat2026-05-01T08:00:11.172Zhttps://docs.openclaw.ai/zh-TW/gateway2026-05-01T08:00:11.215Zhttps://docs.openclaw.ai/zh-TW/gateway/local-models2026-05-01T08:00:11.206Zhttps://docs.openclaw.ai/zh-TW/gateway/logging2026-05-01T08:00:11.205Zhttps://docs.openclaw.ai/zh-TW/gateway/multiple-gateways2026-05-01T08:00:11.213Zhttps://docs.openclaw.ai/zh-TW/gateway/network-model2026-05-01T08:00:11.212Zhttps://docs.openclaw.ai/zh-TW/gateway/openai-http-api2026-05-01T08:00:11.210Zhttps://docs.openclaw.ai/zh-TW/gateway/openresponses-http-api2026-05-01T08:00:11.214Zhttps://docs.openclaw.ai/zh-TW/gateway/openshell2026-05-01T08:00:11.206Zhttps://docs.openclaw.ai/zh-TW/gateway/opentelemetry2026-05-01T08:00:11.207Zhttps://docs.openclaw.ai/zh-TW/gateway/pairing2026-05-01T08:00:11.205Zhttps://docs.openclaw.ai/zh-TW/gateway/prometheus2026-05-01T08:00:11.255Zhttps://docs.openclaw.ai/zh-TW/gateway/protocol2026-05-01T08:00:11.242Zhttps://docs.openclaw.ai/zh-TW/gateway/remote2026-05-01T08:00:11.254Zhttps://docs.openclaw.ai/zh-TW/gateway/remote-gateway-readme2026-05-01T08:00:11.243Zhttps://docs.openclaw.ai/zh-TW/gateway/sandbox-vs-tool-policy-vs-elevated2026-05-01T08:00:11.240Zhttps://docs.openclaw.ai/zh-TW/gateway/sandboxing2026-05-01T08:00:11.243Zhttps://docs.openclaw.ai/zh-TW/gateway/secrets2026-05-01T08:00:11.235Zhttps://docs.openclaw.ai/zh-TW/gateway/secrets-plan-contract2026-05-01T08:00:11.234Zhttps://docs.openclaw.ai/zh-TW/gateway/security/audit-checks2026-05-01T08:00:11.235Zhttps://docs.openclaw.ai/zh-TW/gateway/security2026-04-30T03:53:13.822Zhttps://docs.openclaw.ai/zh-TW/gateway/tailscale2026-05-01T08:00:11.234Zhttps://docs.openclaw.ai/zh-TW/gateway/tools-invoke-http-api2026-05-01T08:00:11.286Zhttps://docs.openclaw.ai/zh-TW/gateway/troubleshooting2026-05-01T08:00:11.285Zhttps://docs.openclaw.ai/zh-TW/gateway/trusted-proxy-auth2026-05-01T08:00:11.283Zhttps://docs.openclaw.ai/zh-TW/help/debugging2026-05-01T08:00:11.284Zhttps://docs.openclaw.ai/zh-TW/help/environment2026-05-01T08:00:11.284Zhttps://docs.openclaw.ai/zh-TW/help/faq2026-05-01T08:00:11.276Zhttps://docs.openclaw.ai/zh-TW/help/faq-first-run2026-05-01T08:00:11.285Zhttps://docs.openclaw.ai/zh-TW/help/faq-models2026-05-01T08:00:11.277Zhttps://docs.openclaw.ai/zh-TW/help/gpt55-codex-agentic-parity2026-05-01T08:00:11.275Zhttps://docs.openclaw.ai/zh-TW/help/gpt55-codex-agentic-parity-maintainers2026-05-01T08:00:11.276Zhttps://docs.openclaw.ai/zh-TW/help2026-05-01T08:00:11.318Zhttps://docs.openclaw.ai/zh-TW/help/scripts2026-05-01T08:00:11.311Zhttps://docs.openclaw.ai/zh-TW/help/testing2026-05-01T08:00:11.314Zhttps://docs.openclaw.ai/zh-TW/help/testing-live2026-05-01T08:00:11.315Zhttps://docs.openclaw.ai/zh-TW/help/troubleshooting2026-05-01T08:00:11.315Zhttps://docs.openclaw.ai/zh-TW2026-05-01T08:00:11.313Zhttps://docs.openclaw.ai/zh-TW/install/ansible2026-05-01T08:00:11.305Zhttps://docs.openclaw.ai/zh-TW/install/azure2026-05-01T08:00:11.305Zhttps://docs.openclaw.ai/zh-TW/install/bun2026-05-01T08:00:11.314Zhttps://docs.openclaw.ai/zh-TW/install/clawdock2026-05-01T08:00:11.304Zhttps://docs.openclaw.ai/zh-TW/install/development-channels2026-05-01T08:00:11.359Zhttps://docs.openclaw.ai/zh-TW/install/digitalocean2026-05-01T08:00:11.346Zhttps://docs.openclaw.ai/zh-TW/install/docker2026-05-01T08:00:11.346Zhttps://docs.openclaw.ai/zh-TW/install/docker-vm-runtime2026-05-01T08:00:11.345Zhttps://docs.openclaw.ai/zh-TW/install/exe-dev2026-05-01T08:00:11.337Zhttps://docs.openclaw.ai/zh-TW/install/fly2026-05-01T08:00:11.344Zhttps://docs.openclaw.ai/zh-TW/install/gcp2026-05-01T08:00:11.347Zhttps://docs.openclaw.ai/zh-TW/install/hetzner2026-05-01T08:00:11.338Zhttps://docs.openclaw.ai/zh-TW/install/hostinger2026-05-01T08:00:11.345Zhttps://docs.openclaw.ai/zh-TW/install2026-05-01T08:00:11.336Zhttps://docs.openclaw.ai/zh-TW/install/installer2026-05-01T08:00:11.386Zhttps://docs.openclaw.ai/zh-TW/install/kubernetes2026-05-01T08:00:11.387Zhttps://docs.openclaw.ai/zh-TW/install/macos-vm2026-05-01T08:00:11.386Zhttps://docs.openclaw.ai/zh-TW/install/migrating2026-05-01T08:00:11.383Zhttps://docs.openclaw.ai/zh-TW/install/migrating-claude2026-05-01T08:00:11.379Zhttps://docs.openclaw.ai/zh-TW/install/migrating-hermes2026-05-01T08:00:11.385Zhttps://docs.openclaw.ai/zh-TW/install/nix2026-05-01T08:00:11.378Zhttps://docs.openclaw.ai/zh-TW/install/node2026-05-01T08:00:11.378Zhttps://docs.openclaw.ai/zh-TW/install/northflank2026-05-01T08:00:11.384Zhttps://docs.openclaw.ai/zh-TW/install/oracle2026-05-01T08:00:11.377Zhttps://docs.openclaw.ai/zh-TW/install/podman2026-05-01T08:00:11.416Zhttps://docs.openclaw.ai/zh-TW/install/railway2026-05-01T08:00:11.409Zhttps://docs.openclaw.ai/zh-TW/install/raspberry-pi2026-05-01T08:00:11.407Zhttps://docs.openclaw.ai/zh-TW/install/render2026-05-01T08:00:11.412Zhttps://docs.openclaw.ai/zh-TW/install/uninstall2026-05-01T08:00:11.418Zhttps://docs.openclaw.ai/zh-TW/install/updating2026-05-01T08:00:11.412Zhttps://docs.openclaw.ai/zh-TW/logging2026-05-01T08:00:11.408Zhttps://docs.openclaw.ai/zh-TW/network2026-05-01T08:00:11.407Zhttps://docs.openclaw.ai/zh-TW/nodes/audio2026-05-01T08:00:11.415Zhttps://docs.openclaw.ai/zh-TW/nodes/camera2026-05-01T08:00:11.406Zhttps://docs.openclaw.ai/zh-TW/nodes/images2026-05-01T08:00:11.447Zhttps://docs.openclaw.ai/zh-TW/nodes2026-05-01T08:00:11.445Zhttps://docs.openclaw.ai/zh-TW/nodes/location-command2026-05-01T08:00:11.437Zhttps://docs.openclaw.ai/zh-TW/nodes/media-understanding2026-05-01T08:00:11.443Zhttps://docs.openclaw.ai/zh-TW/nodes/talk2026-05-01T08:00:11.446Zhttps://docs.openclaw.ai/zh-TW/nodes/troubleshooting2026-05-01T08:00:11.446Zhttps://docs.openclaw.ai/zh-TW/nodes/voicewake2026-05-01T08:00:11.445Zhttps://docs.openclaw.ai/zh-TW/pi2026-05-01T08:00:11.436Zhttps://docs.openclaw.ai/zh-TW/pi-dev2026-05-01T08:00:11.438Zhttps://docs.openclaw.ai/zh-TW/platforms/android2026-05-01T08:00:11.485Zhttps://docs.openclaw.ai/zh-TW/platforms2026-05-01T08:00:11.484Zhttps://docs.openclaw.ai/zh-TW/platforms/ios2026-05-01T08:00:11.477Zhttps://docs.openclaw.ai/zh-TW/platforms/linux2026-05-01T08:00:11.480Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/bundled-gateway2026-05-01T08:00:11.483Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/canvas2026-05-01T08:00:11.478Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/child-process2026-05-01T08:00:11.478Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/dev-setup2026-05-01T08:00:11.516Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/health2026-05-01T08:00:11.514Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/icon2026-05-01T08:00:11.515Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/logging2026-05-01T08:00:11.513Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/menu-bar2026-05-01T08:00:11.507Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/peekaboo2026-05-01T08:00:11.507Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/permissions2026-05-01T08:00:11.508Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/remote2026-05-01T08:00:11.506Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/signing2026-05-01T08:00:11.506Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/skills2026-05-01T08:00:11.511Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/voice-overlay2026-05-01T08:00:11.545Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/voicewake2026-05-01T08:00:11.543Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/webchat2026-05-01T08:00:11.543Zhttps://docs.openclaw.ai/zh-TW/platforms/mac/xpc2026-05-01T08:00:11.542Zhttps://docs.openclaw.ai/zh-TW/platforms/macos2026-05-01T08:00:11.541Zhttps://docs.openclaw.ai/zh-TW/platforms/windows2026-05-01T08:00:11.536Zhttps://docs.openclaw.ai/zh-TW/plugins/architecture2026-05-01T08:00:11.586Zhttps://docs.openclaw.ai/zh-TW/plugins/architecture-internals2026-05-01T08:00:11.535Zhttps://docs.openclaw.ai/zh-TW/plugins/building-plugins2026-05-01T08:00:11.577Zhttps://docs.openclaw.ai/zh-TW/plugins/bundles2026-05-01T08:00:11.585Zhttps://docs.openclaw.ai/zh-TW/plugins/codex-computer-use2026-05-01T08:00:11.579Zhttps://docs.openclaw.ai/zh-TW/plugins/codex-harness2026-05-01T08:00:11.576Zhttps://docs.openclaw.ai/zh-TW/plugins/community2026-05-01T08:00:11.578Zhttps://docs.openclaw.ai/zh-TW/plugins/compatibility2026-05-01T08:00:11.578Zhttps://docs.openclaw.ai/zh-TW/plugins/google-meet2026-05-01T08:00:11.583Zhttps://docs.openclaw.ai/zh-TW/plugins/hooks2026-05-01T08:00:11.577Zhttps://docs.openclaw.ai/zh-TW/plugins/manifest2026-05-01T08:00:11.619Zhttps://docs.openclaw.ai/zh-TW/plugins/memory-lancedb2026-05-01T08:00:11.616Zhttps://docs.openclaw.ai/zh-TW/plugins/memory-wiki2026-05-01T08:00:11.616Zhttps://docs.openclaw.ai/zh-TW/plugins/message-presentation2026-05-01T08:00:11.606Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-agent-harness2026-05-01T08:00:11.607Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-channel-plugins2026-05-01T08:00:11.617Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-channel-turn2026-05-01T08:00:11.607Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-entrypoints2026-05-01T08:00:11.617Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-migration2026-05-01T08:00:11.612Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-overview2026-05-01T08:00:11.615Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-provider-plugins2026-05-01T08:00:11.652Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-runtime2026-05-01T08:00:11.640Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-setup2026-05-01T08:00:11.640Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-subpaths2026-05-01T08:00:11.650Zhttps://docs.openclaw.ai/zh-TW/plugins/sdk-testing2026-05-01T08:00:11.648Zhttps://docs.openclaw.ai/zh-TW/plugins/skill-workshop2026-05-01T08:00:11.648Zhttps://docs.openclaw.ai/zh-TW/plugins/voice-call2026-05-01T08:00:11.639Zhttps://docs.openclaw.ai/zh-TW/plugins/webhooks2026-05-01T08:00:11.650Zhttps://docs.openclaw.ai/zh-TW/plugins/zalouser2026-05-01T08:00:11.641Zhttps://docs.openclaw.ai/zh-TW/prose2026-05-01T08:00:11.649Zhttps://docs.openclaw.ai/zh-TW/providers/alibaba2026-05-01T08:00:11.688Zhttps://docs.openclaw.ai/zh-TW/providers/anthropic2026-05-01T08:00:11.688Zhttps://docs.openclaw.ai/zh-TW/providers/arcee2026-05-01T08:00:11.680Zhttps://docs.openclaw.ai/zh-TW/providers/azure-speech2026-05-01T08:00:11.679Zhttps://docs.openclaw.ai/zh-TW/providers/bedrock2026-05-01T08:00:11.685Zhttps://docs.openclaw.ai/zh-TW/providers/bedrock-mantle2026-05-01T08:00:11.687Zhttps://docs.openclaw.ai/zh-TW/providers/chutes2026-05-01T08:00:11.687Zhttps://docs.openclaw.ai/zh-TW/providers/claude-max-api-proxy2026-05-01T08:00:11.679Zhttps://docs.openclaw.ai/zh-TW/providers/cloudflare-ai-gateway2026-05-01T08:00:11.678Zhttps://docs.openclaw.ai/zh-TW/providers/comfy2026-05-01T08:00:11.718Zhttps://docs.openclaw.ai/zh-TW/providers/deepgram2026-05-01T08:00:11.717Zhttps://docs.openclaw.ai/zh-TW/providers/deepinfra2026-05-01T08:00:11.716Zhttps://docs.openclaw.ai/zh-TW/providers/deepseek2026-05-01T08:00:11.716Zhttps://docs.openclaw.ai/zh-TW/providers/elevenlabs2026-05-01T08:00:11.708Zhttps://docs.openclaw.ai/zh-TW/providers/fal2026-05-01T08:00:11.713Zhttps://docs.openclaw.ai/zh-TW/providers/fireworks2026-05-01T08:00:11.709Zhttps://docs.openclaw.ai/zh-TW/providers/github-copilot2026-05-01T08:00:11.715Zhttps://docs.openclaw.ai/zh-TW/providers/glm2026-05-01T08:00:11.708Zhttps://docs.openclaw.ai/zh-TW/providers/google2026-05-01T08:00:11.707Zhttps://docs.openclaw.ai/zh-TW/providers/gradium2026-05-01T08:00:11.750Zhttps://docs.openclaw.ai/zh-TW/providers/groq2026-05-01T08:00:11.749Zhttps://docs.openclaw.ai/zh-TW/providers/huggingface2026-05-01T08:00:11.754Zhttps://docs.openclaw.ai/zh-TW/providers2026-05-01T08:00:11.743Zhttps://docs.openclaw.ai/zh-TW/providers/inferrs2026-05-01T08:00:11.753Zhttps://docs.openclaw.ai/zh-TW/providers/inworld2026-05-01T08:00:11.745Zhttps://docs.openclaw.ai/zh-TW/providers/kilocode2026-05-01T08:00:11.742Zhttps://docs.openclaw.ai/zh-TW/providers/litellm2026-05-01T08:00:11.742Zhttps://docs.openclaw.ai/zh-TW/providers/lmstudio2026-05-01T08:00:11.741Zhttps://docs.openclaw.ai/zh-TW/providers/minimax2026-05-01T08:00:11.740Zhttps://docs.openclaw.ai/zh-TW/providers/mistral2026-05-01T08:00:11.805Zhttps://docs.openclaw.ai/zh-TW/providers/models2026-05-01T08:00:11.800Zhttps://docs.openclaw.ai/zh-TW/providers/moonshot2026-05-01T08:00:11.809Zhttps://docs.openclaw.ai/zh-TW/providers/nvidia2026-05-01T08:00:11.803Zhttps://docs.openclaw.ai/zh-TW/providers/ollama2026-05-01T08:00:11.798Zhttps://docs.openclaw.ai/zh-TW/providers/openai2026-05-01T08:00:11.801Zhttps://docs.openclaw.ai/zh-TW/providers/opencode2026-05-01T08:00:11.801Zhttps://docs.openclaw.ai/zh-TW/providers/opencode-go2026-05-01T08:00:11.802Zhttps://docs.openclaw.ai/zh-TW/providers/openrouter2026-05-01T08:00:11.799Zhttps://docs.openclaw.ai/zh-TW/providers/perplexity-provider2026-05-01T08:00:11.806Zhttps://docs.openclaw.ai/zh-TW/providers/qianfan2026-05-01T08:00:11.870Zhttps://docs.openclaw.ai/zh-TW/providers/qwen2026-05-01T08:00:11.867Zhttps://docs.openclaw.ai/zh-TW/providers/runway2026-05-01T08:00:11.869Zhttps://docs.openclaw.ai/zh-TW/providers/sglang2026-05-01T08:00:11.864Zhttps://docs.openclaw.ai/zh-TW/providers/stepfun2026-05-01T08:00:11.863Zhttps://docs.openclaw.ai/zh-TW/providers/synthetic2026-05-01T08:00:11.862Zhttps://docs.openclaw.ai/zh-TW/providers/tencent2026-05-01T08:00:11.863Zhttps://docs.openclaw.ai/zh-TW/providers/together2026-05-01T08:00:11.862Zhttps://docs.openclaw.ai/zh-TW/providers/venice2026-05-01T08:00:11.861Zhttps://docs.openclaw.ai/zh-TW/providers/vercel-ai-gateway2026-05-01T08:00:11.899Zhttps://docs.openclaw.ai/zh-TW/providers/vllm2026-05-01T08:00:11.897Zhttps://docs.openclaw.ai/zh-TW/providers/volcengine2026-05-01T08:00:11.898Zhttps://docs.openclaw.ai/zh-TW/providers/vydra2026-05-01T08:00:11.888Zhttps://docs.openclaw.ai/zh-TW/providers/xai2026-05-01T08:00:11.897Zhttps://docs.openclaw.ai/zh-TW/providers/xiaomi2026-05-01T08:00:11.889Zhttps://docs.openclaw.ai/zh-TW/providers/zai2026-05-01T08:00:11.898Zhttps://docs.openclaw.ai/zh-TW/reference/AGENTS.default2026-05-01T08:00:11.896Zhttps://docs.openclaw.ai/zh-TW/reference/RELEASING2026-05-01T08:00:11.889Zhttps://docs.openclaw.ai/zh-TW/reference/api-usage-costs2026-05-01T08:00:11.895Zhttps://docs.openclaw.ai/zh-TW/reference/credits2026-05-01T08:00:11.924Zhttps://docs.openclaw.ai/zh-TW/reference/device-models2026-05-01T08:00:11.926Zhttps://docs.openclaw.ai/zh-TW/reference/full-release-validation2026-05-01T08:00:11.925Zhttps://docs.openclaw.ai/zh-TW/reference/memory-config2026-05-01T08:00:11.928Zhttps://docs.openclaw.ai/zh-TW/reference/openclaw-sdk-api-design2026-05-01T08:00:11.919Zhttps://docs.openclaw.ai/zh-TW/reference/prompt-caching2026-05-01T08:00:11.927Zhttps://docs.openclaw.ai/zh-TW/reference/rich-output-protocol2026-05-01T08:00:11.925Zhttps://docs.openclaw.ai/zh-TW/reference/rpc2026-05-01T08:00:11.918Zhttps://docs.openclaw.ai/zh-TW/reference/secretref-credential-surface2026-05-01T08:00:11.918Zhttps://docs.openclaw.ai/zh-TW/reference/session-management-compaction2026-05-01T08:00:11.968Zhttps://docs.openclaw.ai/zh-TW/reference/templates/AGENTS2026-05-01T08:00:11.950Zhttps://docs.openclaw.ai/zh-TW/reference/templates/BOOT2026-05-01T08:00:11.951Zhttps://docs.openclaw.ai/zh-TW/reference/templates/BOOTSTRAP2026-05-01T08:00:11.970Zhttps://docs.openclaw.ai/zh-TW/reference/templates/HEARTBEAT2026-05-01T08:00:11.970Zhttps://docs.openclaw.ai/zh-TW/reference/templates/IDENTITY2026-05-01T08:00:11.952Zhttps://docs.openclaw.ai/zh-TW/reference/templates/SOUL2026-05-01T08:00:11.955Zhttps://docs.openclaw.ai/zh-TW/reference/templates/TOOLS2026-05-01T08:00:11.991Zhttps://docs.openclaw.ai/zh-TW/reference/templates/USER2026-05-01T08:00:11.999Zhttps://docs.openclaw.ai/zh-TW/reference/test2026-05-01T08:00:11.998Zhttps://docs.openclaw.ai/zh-TW/reference/token-use2026-05-01T08:00:11.990Zhttps://docs.openclaw.ai/zh-TW/reference/transcript-hygiene2026-05-01T08:00:11.998Zhttps://docs.openclaw.ai/zh-TW/reference/wizard2026-05-01T08:00:11.991Zhttps://docs.openclaw.ai/zh-TW/security/CONTRIBUTING-THREAT-MODEL2026-05-01T08:00:11.996Zhttps://docs.openclaw.ai/zh-TW/security/THREAT-MODEL-ATLAS2026-05-01T08:00:11.995Zhttps://docs.openclaw.ai/zh-TW/security/formal-verification2026-05-01T08:00:12.029Zhttps://docs.openclaw.ai/zh-TW/security/network-proxy2026-05-01T08:00:12.029Zhttps://docs.openclaw.ai/zh-TW/start/bootstrapping2026-05-01T08:00:12.025Zhttps://docs.openclaw.ai/zh-TW/start/docs-directory2026-05-01T08:00:12.027Zhttps://docs.openclaw.ai/zh-TW/start/getting-started2026-05-01T08:00:12.022Zhttps://docs.openclaw.ai/zh-TW/start/hubs2026-05-01T08:00:12.028Zhttps://docs.openclaw.ai/zh-TW/start/lore2026-05-01T08:00:12.021Zhttps://docs.openclaw.ai/zh-TW/start/onboarding2026-05-01T08:00:12.021Zhttps://docs.openclaw.ai/zh-TW/start/onboarding-overview2026-05-01T08:00:12.020Zhttps://docs.openclaw.ai/zh-TW/start/openclaw2026-05-01T08:00:12.026Zhttps://docs.openclaw.ai/zh-TW/start/setup2026-05-01T08:00:12.056Zhttps://docs.openclaw.ai/zh-TW/start/showcase2026-04-30T03:53:50.340Zhttps://docs.openclaw.ai/zh-TW/start/wizard2026-05-01T08:00:12.057Zhttps://docs.openclaw.ai/zh-TW/start/wizard-cli-automation2026-05-01T08:00:12.055Zhttps://docs.openclaw.ai/zh-TW/start/wizard-cli-reference2026-05-01T08:00:12.049Zhttps://docs.openclaw.ai/zh-TW/tools/acp-agents2026-05-01T08:00:12.058Zhttps://docs.openclaw.ai/zh-TW/tools/acp-agents-setup2026-05-01T08:00:12.048Zhttps://docs.openclaw.ai/zh-TW/tools/agent-send2026-05-01T08:00:12.049Zhttps://docs.openclaw.ai/zh-TW/tools/apply-patch2026-05-01T08:00:12.099Zhttps://docs.openclaw.ai/zh-TW/tools/brave-search2026-05-01T08:00:12.095Zhttps://docs.openclaw.ai/zh-TW/tools/browser2026-05-01T08:00:12.097Zhttps://docs.openclaw.ai/zh-TW/tools/browser-control2026-05-01T08:00:12.098Zhttps://docs.openclaw.ai/zh-TW/tools/browser-linux-troubleshooting2026-05-01T08:00:12.089Zhttps://docs.openclaw.ai/zh-TW/tools/browser-login2026-05-01T08:00:12.098Zhttps://docs.openclaw.ai/zh-TW/tools/browser-wsl2-windows-remote-cdp-troubleshooting2026-05-01T08:00:12.096Zhttps://docs.openclaw.ai/zh-TW/tools/btw2026-05-01T08:00:12.090Zhttps://docs.openclaw.ai/zh-TW/tools/clawhub2026-05-01T08:00:12.088Zhttps://docs.openclaw.ai/zh-TW/tools/code-execution2026-05-01T08:00:12.129Zhttps://docs.openclaw.ai/zh-TW/tools/creating-skills2026-05-01T08:00:12.128Zhttps://docs.openclaw.ai/zh-TW/tools/diffs2026-05-01T08:00:12.127Zhttps://docs.openclaw.ai/zh-TW/tools/duckduckgo-search2026-05-01T08:00:12.119Zhttps://docs.openclaw.ai/zh-TW/tools/elevated2026-05-01T08:00:12.118Zhttps://docs.openclaw.ai/zh-TW/tools/exa-search2026-05-01T08:00:12.124Zhttps://docs.openclaw.ai/zh-TW/tools/exec2026-05-01T08:00:12.126Zhttps://docs.openclaw.ai/zh-TW/tools/exec-approvals2026-05-01T08:00:12.119Zhttps://docs.openclaw.ai/zh-TW/tools/exec-approvals-advanced2026-05-01T08:00:12.127Zhttps://docs.openclaw.ai/zh-TW/tools/firecrawl2026-05-01T08:00:12.118Zhttps://docs.openclaw.ai/zh-TW/tools/gemini-search2026-05-01T08:00:12.168Zhttps://docs.openclaw.ai/zh-TW/tools/grok-search2026-05-01T08:00:12.156Zhttps://docs.openclaw.ai/zh-TW/tools/image-generation2026-05-01T08:00:12.156Zhttps://docs.openclaw.ai/zh-TW/tools2026-05-01T08:00:12.157Zhttps://docs.openclaw.ai/zh-TW/tools/kimi-search2026-05-01T08:00:12.148Zhttps://docs.openclaw.ai/zh-TW/tools/llm-task2026-05-01T08:00:12.154Zhttps://docs.openclaw.ai/zh-TW/tools/lobster2026-05-01T08:00:12.149Zhttps://docs.openclaw.ai/zh-TW/tools/loop-detection2026-05-01T08:00:12.148Zhttps://docs.openclaw.ai/zh-TW/tools/media-overview2026-05-01T08:00:12.155Zhttps://docs.openclaw.ai/zh-TW/tools/minimax-search2026-05-01T08:00:12.147Zhttps://docs.openclaw.ai/zh-TW/tools/multi-agent-sandbox-tools2026-05-01T08:00:12.196Zhttps://docs.openclaw.ai/zh-TW/tools/music-generation2026-05-01T08:00:12.194Zhttps://docs.openclaw.ai/zh-TW/tools/ollama-search2026-05-01T08:00:12.194Zhttps://docs.openclaw.ai/zh-TW/tools/pdf2026-05-01T08:00:12.195Zhttps://docs.openclaw.ai/zh-TW/tools/perplexity-search2026-05-01T08:00:12.196Zhttps://docs.openclaw.ai/zh-TW/tools/plugin2026-05-01T08:00:12.193Zhttps://docs.openclaw.ai/zh-TW/tools/reactions2026-05-01T08:00:12.195Zhttps://docs.openclaw.ai/zh-TW/tools/searxng-search2026-05-01T08:00:12.187Zhttps://docs.openclaw.ai/zh-TW/tools/skills2026-05-01T08:00:12.186Zhttps://docs.openclaw.ai/zh-TW/tools/skills-config2026-05-01T08:00:12.186Zhttps://docs.openclaw.ai/zh-TW/tools/slash-commands2026-05-01T08:00:12.227Zhttps://docs.openclaw.ai/zh-TW/tools/subagents2026-05-01T08:00:12.220Zhttps://docs.openclaw.ai/zh-TW/tools/tavily2026-05-01T08:00:12.227Zhttps://docs.openclaw.ai/zh-TW/tools/thinking2026-05-01T08:00:12.219Zhttps://docs.openclaw.ai/zh-TW/tools/tokenjuice2026-05-01T08:00:12.226Zhttps://docs.openclaw.ai/zh-TW/tools/trajectory2026-05-01T08:00:12.220Zhttps://docs.openclaw.ai/zh-TW/tools/tts2026-05-01T08:00:12.218Zhttps://docs.openclaw.ai/zh-TW/tools/video-generation2026-05-01T08:00:12.219Zhttps://docs.openclaw.ai/zh-TW/tools/web2026-05-01T08:00:12.224Zhttps://docs.openclaw.ai/zh-TW/tools/web-fetch2026-05-01T08:00:12.218Zhttps://docs.openclaw.ai/zh-TW/vps2026-05-01T08:00:12.246Zhttps://docs.openclaw.ai/zh-TW/web/control-ui2026-05-01T08:00:12.248Zhttps://docs.openclaw.ai/zh-TW/web/dashboard2026-05-01T08:00:12.246Zhttps://docs.openclaw.ai/zh-TW/web2026-05-01T08:00:12.247Zhttps://docs.openclaw.ai/zh-TW/web/tui2026-05-01T08:00:12.243Zhttps://docs.openclaw.ai/zh-TW/web/webchat2026-05-01T08:00:12.247Z

---
