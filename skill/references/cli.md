# Cli

_55 pages from docs.openclaw.ai — full content preserved._

## Contents

- [CLI بيئة الاختبار المعزولة - OpenClaw](#cli---openclaw)
- [CLI reference - OpenClaw](#cli-reference---openclaw)
- [ACP - OpenClaw](#acp---openclaw)
- [https://docs.openclaw.ai/cli/acp.md](#httpsdocsopenclawaicliacpmd)
- [Agent - OpenClaw](#agent---openclaw)
- [Agents - OpenClaw](#agents---openclaw)
- [Backup - OpenClaw](#backup---openclaw)
- [Backup - OpenClaw](#backup---openclaw)
- [https://docs.openclaw.ai/cli/backup.md](#httpsdocsopenclawaiclibackupmd)
- [Browser - OpenClaw](#browser---openclaw)
- [Channels - OpenClaw](#channels---openclaw)
- [https://docs.openclaw.ai/cli/channels.md](#httpsdocsopenclawaiclichannelsmd)
- [`openclaw commitments` - OpenClaw](#openclaw-commitments---openclaw)
- [Config - OpenClaw](#config---openclaw)
- [Configure - OpenClaw](#configure---openclaw)
- [Crestodian - OpenClaw](#crestodian---openclaw)
- [Cron - OpenClaw](#cron---openclaw)
- [Daemon - OpenClaw](#daemon---openclaw)
- [Dashboard - OpenClaw](#dashboard---openclaw)
- [Devices - OpenClaw](#devices---openclaw)
- [Directory - OpenClaw](#directory---openclaw)
- [Docs - OpenClaw](#docs---openclaw)
- [Doctor - OpenClaw](#doctor---openclaw)
- [Gateway - OpenClaw](#gateway---openclaw)
- [Health - OpenClaw](#health---openclaw)
- [Hooks - OpenClaw](#hooks---openclaw)
- [MCP - OpenClaw](#mcp---openclaw)
- [https://docs.openclaw.ai/cli/mcp.md](#httpsdocsopenclawaiclimcpmd)
- [Memory - OpenClaw](#memory---openclaw)
- [Message - OpenClaw](#message---openclaw)
- [Models - OpenClaw](#models---openclaw)
- [Nodes - OpenClaw](#nodes---openclaw)
- [Onboard - OpenClaw](#onboard---openclaw)
- [Pairing - OpenClaw](#pairing---openclaw)
- [`pairing list`](#pairing-list)
- [Plugins - OpenClaw](#plugins---openclaw)
- [Proxy - OpenClaw](#proxy---openclaw)
- [https://docs.openclaw.ai/cli/qr.md](#httpsdocsopenclawaicliqrmd)
- [Reset - OpenClaw](#reset---openclaw)
- [Secrets - OpenClaw](#secrets---openclaw)
- [Sessions - OpenClaw](#sessions---openclaw)
- [Setup - OpenClaw](#setup---openclaw)
- [Skills - OpenClaw](#skills---openclaw)
- [https://docs.openclaw.ai/cli/skills.md](#httpsdocsopenclawaicliskillsmd)
- [Status - OpenClaw](#status---openclaw)
- [TUI - OpenClaw](#tui---openclaw)
- [Uninstall - OpenClaw](#uninstall---openclaw)
- [Update - OpenClaw](#update---openclaw)
- [Wiki - OpenClaw](#wiki---openclaw)
- [Log - OpenClaw](#log---openclaw)
- [Node - OpenClaw](#node---openclaw)
- [Flows(리디렉션) - OpenClaw](#flows---openclaw)
- [DNS - OpenClaw](#dns---openclaw)
- [Node - OpenClaw](#node---openclaw)
- [`openclaw tasks` - OpenClaw](#openclaw-tasks---openclaw)

---

## CLI بيئة الاختبار المعزولة - OpenClaw

_Source: <https://docs.openclaw.ai/ar/cli/sandbox>_

# Pull new image
docker pull openclaw-sandbox:latest
docker tag openclaw-sandbox:latest openclaw-sandbox:bookworm-slim

# Update config to use new image
# Edit config: agents.defaults.sandbox.docker.image (or agents.list[].sandbox.docker.image)

# Recreate containers
openclaw sandbox recreate --all
```

### بعد تغيير إعدادات sandbox

```
# Edit config: agents.defaults.sandbox.* (or agents.list[].sandbox.*)

# Recreate to apply new config
openclaw sandbox recreate --all
```

### بعد تغيير هدف SSH أو مواد مصادقة SSH

```
# Edit config:
# - agents.defaults.sandbox.backend
# - agents.defaults.sandbox.ssh.target
# - agents.defaults.sandbox.ssh.workspaceRoot
# - agents.defaults.sandbox.ssh.identityFile / certificateFile / knownHostsFile
# - agents.defaults.sandbox.ssh.identityData / certificateData / knownHostsData

openclaw sandbox recreate --all
```

بالنسبة إلى خلفية `ssh` الأساسية، تحذف إعادة الإنشاء جذر مساحة العمل البعيدة لكل نطاق
على هدف SSH. يؤدي التشغيل التالي إلى بذرها مرة أخرى من مساحة العمل المحلية.

### بعد تغيير مصدر OpenShell أو سياسته أو وضعه

```
# Edit config:
# - agents.defaults.sandbox.backend
# - plugins.entries.openshell.config.from
# - plugins.entries.openshell.config.mode
# - plugins.entries.openshell.config.policy

openclaw sandbox recreate --all
```

بالنسبة إلى وضع OpenShell `remote`، تحذف إعادة الإنشاء مساحة العمل البعيدة المعتمدة
لذلك النطاق. يؤدي التشغيل التالي إلى بذرها مرة أخرى من مساحة العمل المحلية.

### بعد تغيير setupCommand

```
openclaw sandbox recreate --all
# or just one agent:
openclaw sandbox recreate --agent family
```

### لوكيل محدد فقط

```
# Update only one agent's containers
openclaw sandbox recreate --agent alfred
```

## سبب الحاجة إلى ذلك

عند تحديث إعدادات sandbox:

- تستمر أوقات التشغيل الحالية بالعمل بالإعدادات القديمة.
- لا تُزال أوقات التشغيل إلا بعد 24 ساعة من عدم النشاط.
- يحافظ الوكلاء المستخدمون بانتظام على أوقات التشغيل القديمة إلى أجل غير مسمى.

استخدم `openclaw sandbox recreate` لفرض إزالة أوقات التشغيل القديمة. تُعاد إنشاؤها تلقائياً بالإعدادات الحالية عند الحاجة إليها لاحقاً.

فضّل `openclaw sandbox recreate` على التنظيف اليدوي الخاص بالخلفية. فهو يستخدم سجل أوقات التشغيل في Gateway ويتجنب حالات عدم التطابق عندما تتغير مفاتيح النطاق أو الجلسة.

## الإعدادات

توجد إعدادات sandbox في `~/.openclaw/openclaw.json` ضمن `agents.defaults.sandbox` (توضع التجاوزات الخاصة بكل وكيل في `agents.list[].sandbox`):

```
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "all", // off, non-main, all
        "backend": "docker", // docker, ssh, openshell
        "scope": "agent", // session, agent, shared
        "docker": {
          "image": "openclaw-sandbox:bookworm-slim",
          "containerPrefix": "openclaw-sbx-",
          // ... more Docker options
        },
        "prune": {
          "idleHours": 24, // Auto-prune after 24h idle
          "maxAgeDays": 7, // Auto-prune after 7 days
        },
      },
    },
  },
}
```

## ذات صلة

- [مرجع CLI](https://docs.openclaw.ai/ar/cli)
- [Sandboxing](https://docs.openclaw.ai/ar/gateway/sandboxing)
- [مساحة عمل الوكيل](https://docs.openclaw.ai/ar/concepts/agent-workspace)
- [Doctor](https://docs.openclaw.ai/ar/gateway/doctor): يتحقق من إعداد sandbox.

[العُقَد](https://docs.openclaw.ai/ar/cli/nodes) [Config](https://docs.openclaw.ai/ar/cli/config)

Ctrl+I

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
| `--update` | Shorthand for [`openclaw update`](https://docs.openclaw.ai/cli/update) (source installs only) |
| `-V`, `--version`, `-v` | Print version and exit |

## Output modes

- ANSI colors and progress indicators render only in TTY sessions.
- OSC-8 hyperlinks render as clickable links where supported; otherwise the
CLI falls back to plain URLs.
- `--json` (and `--plain` where supported) disables styling for clean output.
- Long-running commands show a progress indicator (OSC 9;4 when supported).

Palette source of truth: `src/terminal/palette.ts`.

## Command tree

Full command tree

```
openclaw [--dev] [--profile <name>] <command>
  crestodian
  setup
  onboard
  configure
  config
    get
    set
    unset
    file
    schema
    validate
  completion
  doctor
  dashboard
  backup
    create
    verify
  security
    audit
  secrets
    reload
    audit
    configure
    apply
  reset
  uninstall
  update
    wizard
    status
  channels
    list
    status
    capabilities
    resolve
    logs
    add
    remove
    login
    logout
  directory
    self
    peers list
    groups list|members
  skills
    search
    install
    update
    list
    info
    check
  plugins
    list
    inspect
    install
    uninstall
    update
    enable
    disable
    doctor
    marketplace list
  memory
    status
    index
    search
  commitments
    list
    dismiss
  wiki
    status
    doctor
    init
    ingest
    compile
    lint
    search
    get
    apply
    bridge import
    unsafe-local import
    obsidian status|search|open|command|daily
  message
    send
    broadcast
    poll
    react
    reactions
    read
    edit
    delete
    pin
    unpin
    pins
    permissions
    search
    thread create|list|reply
    emoji list|upload
    sticker send|upload
    role info|add|remove
    channel info|list
    member info
    voice status
    event list|create
    timeout
    kick
    ban
  agent
  agents
    list
    add
    delete
    bindings
    bind
    unbind
    set-identity
  acp
  mcp
    serve
    list
    show
    set
    unset
  status
  health
  sessions
    cleanup
  tasks
    list
    audit
    maintenance
    show
    notify
    cancel
    flow list|show|cancel
  gateway
    call
    usage-cost
    health
    status
    probe
    discover
    install
    uninstall
    start
    stop
    restart
    run
  daemon
    status
    install
    uninstall
    start
    stop
    restart
  logs
  system
    event
    heartbeat last|enable|disable
    presence
  models
    list
    status
    set
    set-image
    aliases list|add|remove
    fallbacks list|add|remove|clear
    image-fallbacks list|add|remove|clear
    scan
  infer (alias: capability)
    list
    inspect
    model run|list|inspect|providers|auth login|logout|status
    image generate|edit|describe|describe-many|providers
    audio transcribe|providers
    tts convert|voices|providers|status|enable|disable|set-provider
    video generate|describe|providers
    web search|fetch|providers
    embedding create|providers
    auth add|login|login-github-copilot|setup-token|paste-token
    auth order get|set|clear
  sandbox
    list
    recreate
    explain
  cron
    status
    list
    add
    edit
    rm
    enable
    disable
    runs
    run
  nodes
    status
    describe
    list
    pending
    approve
    reject
    rename
    invoke
    notify
    push
    canvas snapshot|present|hide|navigate|eval
    canvas a2ui push|reset
    camera list|snap|clip
    screen record
    location get
  devices
    list
    remove
    clear
    approve
    reject
    rotate
    revoke
  node
    run
    status
    install
    uninstall
    stop
    restart
  approvals
    get
    set
    allowlist add|remove
  exec-policy
    show
    preset
    set
  browser
    status
    start
    stop
    reset-profile
    tabs
    open
    focus
    close
    profiles
    create-profile
    delete-profile
    screenshot
    snapshot
    navigate
    resize
    click
    type
    press
    hover
    drag
    select
    upload
    fill
    dialog
    wait
    evaluate
    console
    pdf
  hooks
    list
    info
    check
    enable
    disable
    install
    update
  webhooks
    gmail setup|run
  proxy
    start
    run
    coverage
    sessions
    query
    blob
    purge
  pairing
    list
    approve
  qr
  clawbot
    qr
  docs
  dns
    setup
  tui
  chat (alias: tui --local)
  terminal (alias: tui --local)
```

Plugins can add additional top-level commands (for example `openclaw voicecall`).

## Chat slash commands

Chat messages support `/...` commands. See [slash commands](https://docs.openclaw.ai/tools/slash-commands).Highlights:

- `/status` — quick diagnostics.
- `/trace` — session-scoped plugin trace/debug lines.
- `/config` — persisted config changes.
- `/debug` — runtime-only config overrides (memory, not disk; requires `commands.debug: true`).

## Usage tracking

`openclaw status --usage` and the Control UI surface provider usage/quota when
OAuth/API credentials are available. Data comes directly from provider usage
endpoints and is normalized to `X% left`. Providers with current usage
windows: Anthropic, GitHub Copilot, Gemini CLI, OpenAI Codex, MiniMax,
Xiaomi, and z.ai.See [Usage tracking](https://docs.openclaw.ai/concepts/usage-tracking) for details.

## Related

- [Slash commands](https://docs.openclaw.ai/tools/slash-commands)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Environment](https://docs.openclaw.ai/help/environment)

[Backup](https://docs.openclaw.ai/cli/backup)

Ctrl+I

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
acpx openclaw exec "Summarize the active OpenClaw session state."

# Persistent named session for follow-up turns
acpx openclaw sessions ensure --name codex-bridge
acpx openclaw -s codex-bridge --cwd /path/to/repo \
  "Ask my OpenClaw work agent for recent context relevant to this repo."
```

If you want `acpx openclaw` to target a specific Gateway and session key every
time, override the `openclaw` agent command in `~/.acpx/config.json`:

```
{
  "agents": {
    "openclaw": {
      "command": "env OPENCLAW_HIDE_BANNER=1 OPENCLAW_SUPPRESS_NOTES=1 openclaw acp --url ws://127.0.0.1:18789 --token-file ~/.openclaw/gateway.token --session agent:main:main"
    }
  }
}
```

For a repo-local OpenClaw checkout, use the direct CLI entrypoint instead of the
dev runner so the ACP stream stays clean. For example:

```
env OPENCLAW_HIDE_BANNER=1 OPENCLAW_SUPPRESS_NOTES=1 node openclaw.mjs acp ...
```

This is the easiest way to let Codex, Claude Code, or another ACP-aware client
pull contextual information from an OpenClaw agent without scraping a terminal.

## Zed editor setup

Add a custom ACP agent in `~/.config/zed/settings.json` (or use Zed’s Settings UI):

```
{
  "agent_servers": {
    "OpenClaw ACP": {
      "type": "custom",
      "command": "openclaw",
      "args": ["acp"],
      "env": {}
    }
  }
}
```

To target a specific Gateway or agent:

```
{
  "agent_servers": {
    "OpenClaw ACP": {
      "type": "custom",
      "command": "openclaw",
      "args": [\
        "acp",\
        "--url",\
        "wss://gateway-host:18789",\
        "--token",\
        "<token>",\
        "--session",\
        "agent:design:main"\
      ],
      "env": {}
    }
  }
}
```

In Zed, open the Agent panel and select “OpenClaw ACP” to start a thread.

## Session mapping

By default, ACP sessions get an isolated Gateway session key with an `acp:` prefix.
To reuse a known session, pass a session key or label:

- `--session <key>`: use a specific Gateway session key.
- `--session-label <label>`: resolve an existing session by label.
- `--reset-session`: mint a fresh session id for that key (same key, new transcript).

If your ACP client supports metadata, you can override per session:

```
{
  "_meta": {
    "sessionKey": "agent:main:main",
    "sessionLabel": "support inbox",
    "resetSession": true
  }
}
```

Learn more about session keys at [/concepts/session](https://docs.openclaw.ai/concepts/session).

## Options

- `--url <url>`: Gateway WebSocket URL (defaults to gateway.remote.url when configured).
- `--token <token>`: Gateway auth token.
- `--token-file <path>`: read Gateway auth token from file.
- `--password <password>`: Gateway auth password.
- `--password-file <path>`: read Gateway auth password from file.
- `--session <key>`: default session key.
- `--session-label <label>`: default session label to resolve.
- `--require-existing`: fail if the session key/label does not exist.
- `--reset-session`: reset the session key before first use.
- `--no-prefix-cwd`: do not prefix prompts with the working directory.
- `--provenance <off|meta|meta+receipt>`: include ACP provenance metadata or receipts.
- `--verbose, -v`: verbose logging to stderr.

Security note:

- `--token` and `--password` can be visible in local process listings on some systems.
- Prefer `--token-file`/`--password-file` or environment variables (`OPENCLAW_GATEWAY_TOKEN`, `OPENCLAW_GATEWAY_PASSWORD`).
- Gateway auth resolution follows the shared contract used by other Gateway clients:
  - local mode: env (`OPENCLAW_GATEWAY_*`) -\> `gateway.auth.*` -\> `gateway.remote.*` fallback only when `gateway.auth.*` is unset (configured-but-unresolved local SecretRefs fail closed)
  - remote mode: `gateway.remote.*` with env/config fallback per remote precedence rules
  - `--url` is override-safe and does not reuse implicit config/env credentials; pass explicit `--token`/`--password` (or file variants)
- ACP runtime backend child processes receive `OPENCLAW_SHELL=acp`, which can be used for context-specific shell/profile rules.
- `openclaw acp client` sets `OPENCLAW_SHELL=acp-client` on the spawned bridge process.

### `acp client` options

- `--cwd <dir>`: working directory for the ACP session.
- `--server <command>`: ACP server command (default: `openclaw`).
- `--server-args <args...>`: extra arguments passed to the ACP server.
- `--server-verbose`: enable verbose logging on the ACP server.
- `--verbose, -v`: verbose client logging.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [ACP agents](https://docs.openclaw.ai/tools/acp-agents)

[TUI](https://docs.openclaw.ai/cli/tui) [Clawbot](https://docs.openclaw.ai/cli/clawbot)

Ctrl+I

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
\| Client terminal methods (\`terminal/\*\`) \| Unsupported \| The bridge does not create ACP client terminals or stream terminal ids through tool calls. \|
\| Session plans / thought streaming \| Unsupported \| The bridge currently emits output text and tool status, not ACP plan or thought updates. \|

\## Known Limitations

\\* \`loadSession\` replays stored user and assistant text history, but it does not
 reconstruct historic tool calls, system notices, or richer ACP-native event
 types.
\\* If multiple ACP clients share the same Gateway session key, event and cancel
 routing are best-effort rather than strictly isolated per client. Prefer the
 default isolated \`acp:\` sessions when you need clean editor-local
 turns.
\\* Gateway stop states are translated into ACP stop reasons, but that mapping is
 less expressive than a fully ACP-native runtime.
\\* Initial session controls currently surface a focused subset of Gateway knobs:
 thought level, tool verbosity, reasoning, usage detail, and elevated
 actions. Model selection and exec-host controls are not yet exposed as ACP
 config options.
\\* \`session\_info\_update\` and \`usage\_update\` are derived from Gateway session
 snapshots, not live ACP-native runtime accounting. Usage is approximate,
 carries no cost data, and is only emitted when the Gateway marks total token
 data as fresh.
\\* Tool follow-along data is best-effort. The bridge can surface file paths that
 appear in known tool args/results, but it does not yet emit ACP terminals or
 structured file diffs.

\## Usage

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw acp

\# Remote Gateway
openclaw acp --url wss://gateway-host:18789 --token

\# Remote Gateway (token from file)
openclaw acp --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token

\# Attach to an existing session key
openclaw acp --session agent:main:main

\# Attach by label (must already exist)
openclaw acp --session-label "support inbox"

\# Reset the session key before the first prompt
openclaw acp --session agent:main:main --reset-session
\`\`\`

\## ACP client (debug)

Use the built-in ACP client to sanity-check the bridge without an IDE.
It spawns the ACP bridge and lets you type prompts interactively.

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw acp client

\# Point the spawned bridge at a remote Gateway
openclaw acp client --server-args --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token

\# Override the server command (default: openclaw)
openclaw acp client --server "node" --server-args openclaw.mjs acp --url ws://127.0.0.1:19001
\`\`\`

Permission model (client debug mode):

\\* Auto-approval is allowlist-based and only applies to trusted core tool IDs.
\\* \`read\` auto-approval is scoped to the current working directory (\`--cwd\` when set).
\\* ACP only auto-approves narrow readonly classes: scoped \`read\` calls under the active cwd plus readonly search tools (\`search\`, \`web\_search\`, \`memory\_search\`). Unknown/non-core tools, out-of-scope reads, exec-capable tools, control-plane tools, mutating tools, and interactive flows always require explicit prompt approval.
\\* Server-provided \`toolCall.kind\` is treated as untrusted metadata (not an authorization source).
\\* This ACP bridge policy is separate from ACPX harness permissions. If you run OpenClaw through the \`acpx\` backend, \`plugins.entries.acpx.config.permissionMode=approve-all\` is the break-glass “yolo” switch for that harness session.

\## How to use this

Use ACP when an IDE (or other client) speaks Agent Client Protocol and you want
it to drive an OpenClaw Gateway session.

1\. Ensure the Gateway is running (local or remote).
2\. Configure the Gateway target (config or flags).
3\. Point your IDE to run \`openclaw acp\` over stdio.

Example config (persisted):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw config set gateway.remote.url wss://gateway-host:18789
openclaw config set gateway.remote.token
\`\`\`

Example direct run (no config write):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw acp --url wss://gateway-host:18789 --token
\# preferred for local process safety
openclaw acp --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token
\`\`\`

\## Selecting agents

ACP does not pick agents directly. It routes by the Gateway session key.

Use agent-scoped session keys to target a specific agent:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw acp --session agent:main:main
openclaw acp --session agent:design:main
openclaw acp --session agent:qa:bug-123
\`\`\`

Each ACP session maps to a single Gateway session key. One agent can have many
sessions; ACP defaults to an isolated \`acp:\` session unless you override
the key or label.

Per-session \`mcpServers\` are not supported in bridge mode. If an ACP client
sends them during \`newSession\` or \`loadSession\`, the bridge returns a clear
error instead of silently ignoring them.

If you want ACPX-backed sessions to see OpenClaw plugin tools or selected
built-in tools such as \`cron\`, enable the gateway-side ACPX MCP bridges instead
of trying to pass per-session \`mcpServers\`. See
\[ACP Agents\](/tools/acp-agents-setup#plugin-tools-mcp-bridge) and
\[OpenClaw tools MCP bridge\](/tools/acp-agents-setup#openclaw-tools-mcp-bridge).

\## Use from \`acpx\` (Codex, Claude, other ACP clients)

If you want a coding agent such as Codex or Claude Code to talk to your
OpenClaw bot over ACP, use \`acpx\` with its built-in \`openclaw\` target.

Typical flow:

1\. Run the Gateway and make sure the ACP bridge can reach it.
2\. Point \`acpx openclaw\` at \`openclaw acp\`.
3\. Target the OpenClaw session key you want the coding agent to use.

Examples:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
\# One-shot request into your default OpenClaw ACP session
acpx openclaw exec "Summarize the active OpenClaw session state."

\# Persistent named session for follow-up turns
acpx openclaw sessions ensure --name codex-bridge
acpx openclaw -s codex-bridge --cwd /path/to/repo \
 "Ask my OpenClaw work agent for recent context relevant to this repo."
\`\`\`

If you want \`acpx openclaw\` to target a specific Gateway and session key every
time, override the \`openclaw\` agent command in \`~/.acpx/config.json\`:

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "agents": {
 "openclaw": {
 "command": "env OPENCLAW\_HIDE\_BANNER=1 OPENCLAW\_SUPPRESS\_NOTES=1 openclaw acp --url ws://127.0.0.1:18789 --token-file ~/.openclaw/gateway.token --session agent:main:main"
 }
 }
}
\`\`\`

For a repo-local OpenClaw checkout, use the direct CLI entrypoint instead of the
dev runner so the ACP stream stays clean. For example:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
env OPENCLAW\_HIDE\_BANNER=1 OPENCLAW\_SUPPRESS\_NOTES=1 node openclaw.mjs acp ...
\`\`\`

This is the easiest way to let Codex, Claude Code, or another ACP-aware client
pull contextual information from an OpenClaw agent without scraping a terminal.

\## Zed editor setup

Add a custom ACP agent in \`~/.config/zed/settings.json\` (or use Zed’s Settings UI):

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "agent\_servers": {
 "OpenClaw ACP": {
 "type": "custom",
 "command": "openclaw",
 "args": \["acp"\],
 "env": {}
 }
 }
}
\`\`\`

To target a specific Gateway or agent:

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "agent\_servers": {
 "OpenClaw ACP": {
 "type": "custom",
 "command": "openclaw",
 "args": \[\
 "acp",\
 "--url",\
 "wss://gateway-host:18789",\
 "--token",\
 "",\
 "--session",\
 "agent:design:main"\
 \],
 "env": {}
 }
 }
}
\`\`\`

In Zed, open the Agent panel and select “OpenClaw ACP” to start a thread.

\## Session mapping

By default, ACP sessions get an isolated Gateway session key with an \`acp:\` prefix.
To reuse a known session, pass a session key or label:

\\* \`--session \`: use a specific Gateway session key.
\\* \`--session-label \`: resolve an existing session by label.
\\* \`--reset-session\`: mint a fresh session id for that key (same key, new transcript).

If your ACP client supports metadata, you can override per session:

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "\_meta": {
 "sessionKey": "agent:main:main",
 "sessionLabel": "support inbox",
 "resetSession": true
 }
}
\`\`\`

Learn more about session keys at \[/concepts/session\](/concepts/session).

\## Options

\\* \`--url \`: Gateway WebSocket URL (defaults to gateway.remote.url when configured).
\\* \`--token \`: Gateway auth token.
\\* \`--token-file \`: read Gateway auth token from file.
\\* \`--password \`: Gateway auth password.
\\* \`--password-file \`: read Gateway auth password from file.
\\* \`--session \`: default session key.
\\* \`--session-label \`: default session label to resolve.
\\* \`--require-existing\`: fail if the session key/label does not exist.
\\* \`--reset-session\`: reset the session key before first use.
\\* \`--no-prefix-cwd\`: do not prefix prompts with the working directory.
\\* \`--provenance \`: include ACP provenance metadata or receipts.
\\* \`--verbose, -v\`: verbose logging to stderr.

Security note:

\\* \`--token\` and \`--password\` can be visible in local process listings on some systems.
\\* Prefer \`--token-file\`/\`--password-file\` or environment variables (\`OPENCLAW\_GATEWAY\_TOKEN\`, \`OPENCLAW\_GATEWAY\_PASSWORD\`).
\\* Gateway auth resolution follows the shared contract used by other Gateway clients:
 \\* local mode: env (\`OPENCLAW\_GATEWAY\_\*\`) -> \`gateway.auth.\*\` -> \`gateway.remote.\*\` fallback only when \`gateway.auth.\*\` is unset (configured-but-unresolved local SecretRefs fail closed)
 \\* remote mode: \`gateway.remote.\*\` with env/config fallback per remote precedence rules
 \\* \`--url\` is override-safe and does not reuse implicit config/env credentials; pass explicit \`--token\`/\`--password\` (or file variants)
\\* ACP runtime backend child processes receive \`OPENCLAW\_SHELL=acp\`, which can be used for context-specific shell/profile rules.
\\* \`openclaw acp client\` sets \`OPENCLAW\_SHELL=acp-client\` on the spawned bridge process.

\### \`acp client\` options

\\* \`--cwd \`: working directory for the ACP session.
\\* \`--server \`: ACP server command (default: \`openclaw\`).
\\* \`--server-args \`: extra arguments passed to the ACP server.
\\* \`--server-verbose\`: enable verbose logging on the ACP server.
\\* \`--verbose, -v\`: verbose client logging.

\## Related

\\* \[CLI reference\](/cli)
\\* \[ACP agents\](/tools/acp-agents)

---

## Agent - OpenClaw

_Source: <https://docs.openclaw.ai/cli/agent>_

# `openclaw agent`

Run an agent turn via the Gateway (use `--local` for embedded).
Use `--agent <id>` to target a configured agent directly.Pass at least one session selector:

- `--to <dest>`
- `--session-id <id>`
- `--agent <id>`

Related:

- Agent send tool: [Agent send](https://docs.openclaw.ai/tools/agent-send)

## Options

- `-m, --message <text>`: required message body
- `-t, --to <dest>`: recipient used to derive the session key
- `--session-id <id>`: explicit session id
- `--agent <id>`: agent id; overrides routing bindings
- `--model <id>`: model override for this run (`provider/model` or model id)
- `--thinking <level>`: agent thinking level (`off`, `minimal`, `low`, `medium`, `high`, plus provider-supported custom levels such as `xhigh`, `adaptive`, or `max`)
- `--verbose <on|off>`: persist verbose level for the session
- `--channel <channel>`: delivery channel; omit to use the main session channel
- `--reply-to <target>`: delivery target override
- `--reply-channel <channel>`: delivery channel override
- `--reply-account <id>`: delivery account override
- `--local`: run the embedded agent directly (after plugin registry preload)
- `--deliver`: send the reply back to the selected channel/target
- `--timeout <seconds>`: override agent timeout (default 600 or config value)
- `--json`: output JSON

## Examples

```
openclaw agent --to +15555550123 --message "status update" --deliver
openclaw agent --agent ops --message "Summarize logs"
openclaw agent --agent ops --model openai/gpt-5.4 --message "Summarize logs"
openclaw agent --session-id 1234 --message "Summarize inbox" --thinking medium
openclaw agent --to +15555550123 --message "Trace logs" --verbose on --json
openclaw agent --agent ops --message "Generate report" --deliver --reply-channel slack --reply-to "#reports"
openclaw agent --agent ops --message "Run locally" --local
```

## Notes

- Gateway mode falls back to the embedded agent when the Gateway request fails. Use `--local` to force embedded execution up front.
- `--local` still preloads the plugin registry first, so plugin-provided providers, tools, and channels stay available during embedded runs.
- `--local` and embedded fallback runs are treated as one-shot runs. Bundled MCP loopback resources and warm Claude stdio sessions opened for that local process are retired after the reply, so scripted invocations do not keep local child processes alive.
- Gateway-backed runs leave Gateway-owned MCP loopback resources under the running Gateway process; older clients may still send the historical cleanup flag, but the Gateway accepts it as a compatibility no-op.
- `--channel`, `--reply-channel`, and `--reply-account` affect reply delivery, not session routing.
- `--json` keeps stdout reserved for the JSON response. Gateway, plugin, and embedded-fallback diagnostics are routed to stderr so scripts can parse stdout directly.
- Embedded fallback JSON includes `meta.transport: "embedded"` and `meta.fallbackFrom: "gateway"` so scripts can distinguish fallback runs from Gateway runs.
- If the Gateway accepts an agent run but the CLI times out waiting for the final reply, embedded fallback uses a fresh explicit `gateway-fallback-*` session/run id and reports `meta.fallbackReason: "gateway_timeout"` plus the fallback session fields. This avoids racing the Gateway-owned transcript lock or silently replacing the original routed conversation session.
- When this command triggers `models.json` regeneration, SecretRef-managed provider credentials are persisted as non-secret markers (for example env var names, `secretref-env:ENV_VAR_NAME`, or `secretref-managed`), not resolved secret plaintext.
- Marker writes are source-authoritative: OpenClaw persists markers from the active source config snapshot, not from resolved runtime secret values.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Agent runtime](https://docs.openclaw.ai/concepts/agent)

[Update](https://docs.openclaw.ai/cli/update) [Agents](https://docs.openclaw.ai/cli/agents)

Ctrl+I

---

## Agents - OpenClaw

_Source: <https://docs.openclaw.ai/cli/agents>_

# `openclaw agents`

Manage isolated agents (workspaces + auth + routing).Related:

- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)
- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)
- [Skills config](https://docs.openclaw.ai/tools/skills-config): skill visibility configuration.

## Examples

```
openclaw agents list
openclaw agents list --bindings
openclaw agents add work --workspace ~/.openclaw/workspace-work
openclaw agents add ops --workspace ~/.openclaw/workspace-ops --bind telegram:ops --non-interactive
openclaw agents bindings
openclaw agents bind --agent work --bind telegram:ops
openclaw agents unbind --agent work --bind telegram:ops
openclaw agents set-identity --workspace ~/.openclaw/workspace --from-identity
openclaw agents set-identity --agent main --avatar avatars/openclaw.png
openclaw agents delete work
```

## Routing bindings

Use routing bindings to pin inbound channel traffic to a specific agent.If you also want different visible skills per agent, configure `agents.defaults.skills` and `agents.list[].skills` in `openclaw.json`. See [Skills config](https://docs.openclaw.ai/tools/skills-config) and [Configuration reference](https://docs.openclaw.ai/gateway/config-agents#agents-defaults-skills).List bindings:

```
openclaw agents bindings
openclaw agents bindings --agent work
openclaw agents bindings --json
```

Add bindings:

```
openclaw agents bind --agent work --bind telegram:ops --bind discord:guild-a
```

If you omit `accountId` (`--bind <channel>`), OpenClaw resolves it from channel defaults and plugin setup hooks when available.If you omit `--agent` for `bind` or `unbind`, OpenClaw targets the current default agent.

### Binding scope behavior

- A binding without `accountId` matches the channel default account only.
- `accountId: "*"` is the channel-wide fallback (all accounts) and is less specific than an explicit account binding.
- If the same agent already has a matching channel binding without `accountId`, and you later bind with an explicit or resolved `accountId`, OpenClaw upgrades that existing binding in place instead of adding a duplicate.

Example:

```
# initial channel-only binding
openclaw agents bind --agent work --bind telegram

# later upgrade to account-scoped binding
openclaw agents bind --agent work --bind telegram:ops
```

After the upgrade, routing for that binding is scoped to `telegram:ops`. If you also want default-account routing, add it explicitly (for example `--bind telegram:default`).Remove bindings:

```
openclaw agents unbind --agent work --bind telegram:ops
openclaw agents unbind --agent work --all
```

`unbind` accepts either `--all` or one or more `--bind` values, not both.

## Command surface

### `agents`

Running `openclaw agents` with no subcommand is equivalent to `openclaw agents list`.

### `agents list`

Options:

- `--json`
- `--bindings`: include full routing rules, not only per-agent counts/summaries

### `agents add [name]`

Options:

- `--workspace <dir>`
- `--model <id>`
- `--agent-dir <dir>`
- `--bind <channel[:accountId]>` (repeatable)
- `--non-interactive`
- `--json`

Notes:

- Passing any explicit add flags switches the command into the non-interactive path.
- Non-interactive mode requires both an agent name and `--workspace`.
- `main` is reserved and cannot be used as the new agent id.
- In interactive mode, auth seeding copies only portable static profiles
(`api_key` and static `token` by default). OAuth refresh-token profiles remain
available only by read-through inheritance from the real `main` agent store.
If the configured default agent is not `main`, sign in separately for OAuth
profiles on the new agent.

### `agents bindings`

Options:

- `--agent <id>`
- `--json`

### `agents bind`

Options:

- `--agent <id>` (defaults to the current default agent)
- `--bind <channel[:accountId]>` (repeatable)
- `--json`

### `agents unbind`

Options:

- `--agent <id>` (defaults to the current default agent)
- `--bind <channel[:accountId]>` (repeatable)
- `--all`
- `--json`

### `agents delete <id>`

Options:

- `--force`
- `--json`

Notes:

- `main` cannot be deleted.
- Without `--force`, interactive confirmation is required.
- Workspace, agent state, and session transcript directories are moved to Trash, not hard-deleted.
- When the Gateway is reachable, deletion is sent through the Gateway so config and session-store cleanup share the same writer as runtime traffic. If the Gateway cannot be reached, the CLI falls back to the offline local path.
- If another agent’s workspace is the same path, inside this workspace, or contains this workspace,
the workspace is retained and `--json` reports `workspaceRetained`,
`workspaceRetainedReason`, and `workspaceSharedWith`.

## Identity files

Each agent workspace can include an `IDENTITY.md` at the workspace root:

- Example path: `~/.openclaw/workspace/IDENTITY.md`
- `set-identity --from-identity` reads from the workspace root (or an explicit `--identity-file`)

Avatar paths resolve relative to the workspace root.

## Set identity

`set-identity` writes fields into `agents.list[].identity`:

- `name`
- `theme`
- `emoji`
- `avatar` (workspace-relative path, http(s) URL, or data URI)

Options:

- `--agent <id>`
- `--workspace <dir>`
- `--identity-file <path>`
- `--from-identity`
- `--name <name>`
- `--theme <theme>`
- `--emoji <emoji>`
- `--avatar <value>`
- `--json`

Notes:

- `--agent` or `--workspace` can be used to select the target agent.
- If you rely on `--workspace` and multiple agents share that workspace, the command fails and asks you to pass `--agent`.
- When no explicit identity fields are provided, the command reads identity data from `IDENTITY.md`.

Load from `IDENTITY.md`:

```
openclaw agents set-identity --workspace ~/.openclaw/workspace --from-identity
```

Override fields explicitly:

```
openclaw agents set-identity --agent main --name "OpenClaw" --emoji "🦞" --avatar avatars/openclaw.png
```

Config sample:

```
{
  agents: {
    list: [\
      {\
        id: "main",\
        identity: {\
          name: "OpenClaw",\
          theme: "space lobster",\
          emoji: "🦞",\
          avatar: "avatars/openclaw.png",\
        },\
      },\
    ],
  },
}
```

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)
- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)

[Agent](https://docs.openclaw.ai/cli/agent) [Hooks](https://docs.openclaw.ai/cli/hooks)

Ctrl+I

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
- Time to rescan the archive if you use `openclaw backup create --verify` or run `openclaw backup verify`
- Filesystem behavior at the destination path. OpenClaw prefers a no-overwrite hard-link publish step and falls back to exclusive copy when hard links are unsupported

Large workspaces are usually the main driver of archive size. If you want a smaller or faster backup, use `--no-include-workspace`.For the smallest archive, use `--only-config`.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)

[CLI reference](https://docs.openclaw.ai/cli) [Crestodian](https://docs.openclaw.ai/cli/crestodian)

Ctrl+I

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
- Time to rescan the archive if you use `openclaw backup create --verify` or run `openclaw backup verify`
- Filesystem behavior at the destination path. OpenClaw prefers a no-overwrite hard-link publish step and falls back to exclusive copy when hard links are unsupported

Large workspaces are usually the main driver of archive size. If you want a smaller or faster backup, use `--no-include-workspace`.For the smallest archive, use `--only-config`.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)

[CLI reference](https://docs.openclaw.ai/cli) [Crestodian](https://docs.openclaw.ai/cli/crestodian)

Ctrl+I

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

OpenClaw does not enforce a built-in maximum backup size or per-file size limit.

Practical limits come from the local machine and destination filesystem:

\\* Available space for the temporary archive write plus the final archive
\\* Time to walk large workspace trees and compress them into a \`.tar.gz\`
\\* Time to rescan the archive if you use \`openclaw backup create --verify\` or run \`openclaw backup verify\`
\\* Filesystem behavior at the destination path. OpenClaw prefers a no-overwrite hard-link publish step and falls back to exclusive copy when hard links are unsupported

Large workspaces are usually the main driver of archive size. If you want a smaller or faster backup, use \`--no-include-workspace\`.

For the smallest archive, use \`--only-config\`.

\## Related

\\* \[CLI reference\](/cli)

---

## Browser - OpenClaw

_Source: <https://docs.openclaw.ai/cli/browser>_

# `openclaw browser`

Manage OpenClaw’s browser control surface and run browser actions (lifecycle, profiles, tabs, snapshots, screenshots, navigation, input, state emulation, and debugging).Related:

- Browser tool + API: [Browser tool](https://docs.openclaw.ai/tools/browser)

## Common flags

- `--url <gatewayWsUrl>`: Gateway WebSocket URL (defaults to config).
- `--token <token>`: Gateway token (if required).
- `--timeout <ms>`: request timeout (ms).
- `--expect-final`: wait for a final Gateway response.
- `--browser-profile <name>`: choose a browser profile (default from config).
- `--json`: machine-readable output (where supported).

## Quick start (local)

```
openclaw browser profiles
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw open https://example.com
openclaw browser --browser-profile openclaw snapshot
```

Agents can run the same readiness check with `browser({ action: "doctor" })`.

## Quick troubleshooting

If `start` fails with `not reachable after start`, troubleshoot CDP readiness first. If `start` and `tabs` succeed but `open` or `navigate` fails, the browser control plane is healthy and the failure is usually navigation SSRF policy.Minimal sequence:

```
openclaw browser --browser-profile openclaw doctor
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw tabs
openclaw browser --browser-profile openclaw open https://example.com
```

Detailed guidance: [Browser troubleshooting](https://docs.openclaw.ai/tools/browser#cdp-startup-failure-vs-navigation-ssrf-block)

## Lifecycle

```
openclaw browser status
openclaw browser doctor
openclaw browser doctor --deep
openclaw browser start
openclaw browser start --headless
openclaw browser stop
openclaw browser --browser-profile openclaw reset-profile
```

Notes:

- `doctor --deep` adds a live snapshot probe. It is useful when basic CDP
readiness is green but you want proof that the current tab can be inspected.
- For `attachOnly` and remote CDP profiles, `openclaw browser stop` closes the
active control session and clears temporary emulation overrides even when
OpenClaw did not launch the browser process itself.
- For local managed profiles, `openclaw browser stop` stops the spawned browser
process.
- `openclaw browser start --headless` applies only to that start request and
only when OpenClaw launches a local managed browser. It does not rewrite
`browser.headless` or profile config, and it is a no-op for an already-running
browser.
- On Linux hosts without `DISPLAY` or `WAYLAND_DISPLAY`, local managed profiles
run headless automatically unless `OPENCLAW_BROWSER_HEADLESS=0`,
`browser.headless=false`, or `browser.profiles.<name>.headless=false`
explicitly requests a visible browser.

## If the command is missing

If `openclaw browser` is an unknown command, check `plugins.allow` in
`~/.openclaw/openclaw.json`.When `plugins.allow` is present, list the bundled browser plugin explicitly
unless the config already has a root `browser` block:

```
{
  plugins: {
    allow: ["telegram", "browser"],
  },
}
```

An explicit root `browser` block, for example `browser.enabled=true` or
`browser.profiles.<name>`, also activates the bundled browser plugin under a
restrictive plugin allowlist.Related: [Browser tool](https://docs.openclaw.ai/tools/browser#missing-browser-command-or-tool)

## Profiles

Profiles are named browser routing configs. In practice:

- `openclaw`: launches or attaches to a dedicated OpenClaw-managed Chrome instance (isolated user data dir).
- `user`: controls your existing signed-in Chrome session via Chrome DevTools MCP.
- custom CDP profiles: point at a local or remote CDP endpoint.

```
openclaw browser profiles
openclaw browser create-profile --name work --color "#FF5A36"
openclaw browser create-profile --name chrome-live --driver existing-session
openclaw browser create-profile --name remote --cdp-url https://browser-host.example.com
openclaw browser delete-profile --name work
```

Use a specific profile:

```
openclaw browser --browser-profile work tabs
```

## Tabs

```
openclaw browser tabs
openclaw browser tab new --label docs
openclaw browser tab label t1 docs
openclaw browser tab select 2
openclaw browser tab close 2
openclaw browser open https://docs.openclaw.ai --label docs
openclaw browser focus docs
openclaw browser close t1
```

`tabs` returns `suggestedTargetId` first, then the stable `tabId` such as `t1`,
the optional label, and the raw `targetId`. Agents should pass
`suggestedTargetId` back into `focus`, `close`, snapshots, and actions. You can
assign a label with `open --label`, `tab new --label`, or `tab label`; labels,
tab ids, raw target ids, and unique target-id prefixes are all accepted.
When Chromium replaces the underlying raw target during a navigation or form
submit, OpenClaw keeps the stable `tabId`/label attached to the replacement tab
when it can prove the match. Raw target ids remain volatile; prefer
`suggestedTargetId`.

## Snapshot / screenshot / actions

Snapshot:

```
openclaw browser snapshot
openclaw browser snapshot --urls
```

Screenshot:

```
openclaw browser screenshot
openclaw browser screenshot --full-page
openclaw browser screenshot --ref e12
openclaw browser screenshot --labels
```

Notes:

- `--full-page` is for page captures only; it cannot be combined with `--ref`
or `--element`.
- `existing-session` / `user` profiles support page screenshots and `--ref`
screenshots from snapshot output, but not CSS `--element` screenshots.
- `--labels` overlays current snapshot refs on the screenshot.
- `snapshot --urls` appends discovered link destinations to AI snapshots so
agents can choose direct navigation targets instead of guessing from link
text alone.

Navigate/click/type (ref-based UI automation):

```
openclaw browser navigate https://example.com
openclaw browser click <ref>
openclaw browser click-coords 120 340
openclaw browser type <ref> "hello"
openclaw browser press Enter
openclaw browser hover <ref>
openclaw browser scrollintoview <ref>
openclaw browser drag <startRef> <endRef>
openclaw browser select <ref> OptionA OptionB
openclaw browser fill --fields '[{"ref":"1","value":"Ada"}]'
openclaw browser wait --text "Done"
openclaw browser evaluate --fn '(el) => el.textContent' --ref <ref>
```

Action responses return the current raw `targetId` after action-triggered page
replacement when OpenClaw can prove the replacement tab. Scripts should still
store and pass `suggestedTargetId`/labels for long-lived workflows.File + dialog helpers:

```
openclaw browser upload /tmp/openclaw/uploads/file.pdf --ref <ref>
openclaw browser waitfordownload
openclaw browser download <ref> report.pdf
openclaw browser dialog --accept
```

Managed Chrome profiles save ordinary click-triggered downloads into the OpenClaw
downloads directory (`/tmp/openclaw/downloads` by default, or the configured temp
root). Use `waitfordownload` or `download` when the agent needs to wait for a
specific file and return its path; those explicit waiters own the next download.

## State and storage

Viewport + emulation:

```
openclaw browser resize 1280 720
openclaw browser set viewport 1280 720
openclaw browser set offline on
openclaw browser set media dark
openclaw browser set timezone Europe/London
openclaw browser set locale en-GB
openclaw browser set geo 51.5074 -0.1278 --accuracy 25
openclaw browser set device "iPhone 14"
openclaw browser set headers '{"x-test":"1"}'
openclaw browser set credentials myuser mypass
```

Cookies + storage:

```
openclaw browser cookies
openclaw browser cookies set session abc123 --url https://example.com
openclaw browser cookies clear
openclaw browser storage local get
openclaw browser storage local set token abc123
openclaw browser storage session clear
```

## Debugging

```
openclaw browser console --level error
openclaw browser pdf
openclaw browser responsebody "**/api"
openclaw browser highlight <ref>
openclaw browser errors --clear
openclaw browser requests --filter api
openclaw browser trace start
openclaw browser trace stop --out trace.zip
```

## Existing Chrome via MCP

Use the built-in `user` profile, or create your own `existing-session` profile:

```
openclaw browser --browser-profile user tabs
openclaw browser create-profile --name chrome-live --driver existing-session
openclaw browser create-profile --name brave-live --driver existing-session --user-data-dir "~/Library/Application Support/BraveSoftware/Brave-Browser"
openclaw browser --browser-profile chrome-live tabs
```

This path is host-only. For Docker, headless servers, Browserless, or other remote setups, use a CDP profile instead.Current existing-session limits:

- snapshot-driven actions use refs, not CSS selectors
- `browser.actionTimeoutMs` defaults supported `act` requests to 60000 ms when
callers omit `timeoutMs`; per-call `timeoutMs` still wins.
- `click` is left-click only
- `type` does not support `slowly=true`
- `press` does not support `delayMs`
- `hover`, `scrollintoview`, `drag`, `select`, `fill`, and `evaluate` reject
per-call timeout overrides
- `select` supports one value only
- `wait --load networkidle` is not supported
- file uploads require `--ref` / `--input-ref`, do not support CSS
`--element`, and currently support one file at a time
- dialog hooks do not support `--timeout`
- screenshots support page captures and `--ref`, but not CSS `--element`
- `responsebody`, download interception, PDF export, and batch actions still
require a managed browser or raw CDP profile

## Remote browser control (node host proxy)

If the Gateway runs on a different machine than the browser, run a **node host** on the machine that has Chrome/Brave/Edge/Chromium. The Gateway will proxy browser actions to that node (no separate browser control server required).Use `gateway.nodes.browser.mode` to control auto-routing and `gateway.nodes.browser.node` to pin a specific node if multiple are connected.Security + remote setup: [Browser tool](https://docs.openclaw.ai/tools/browser), [Remote access](https://docs.openclaw.ai/gateway/remote), [Tailscale](https://docs.openclaw.ai/gateway/tailscale), [Security](https://docs.openclaw.ai/gateway/security)

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Browser](https://docs.openclaw.ai/tools/browser)

[Approvals](https://docs.openclaw.ai/cli/approvals) [Cron](https://docs.openclaw.ai/cli/cron)

Ctrl+I

---

## Channels - OpenClaw

_Source: <https://docs.openclaw.ai/cli/channels>_

# `openclaw channels`

Manage chat channel accounts and their runtime status on the Gateway.Related docs:

- Channel guides: [Channels](https://docs.openclaw.ai/channels)
- Gateway configuration: [Configuration](https://docs.openclaw.ai/gateway/configuration)

## Common commands

```
openclaw channels list
openclaw channels status
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
openclaw channels resolve --channel slack "#general" "@jane"
openclaw channels logs --channel all
```

## Status / capabilities / resolve / logs

- `channels status`: `--probe`, `--timeout <ms>`, `--json`
- `channels capabilities`: `--channel <name>`, `--account <id>` (only with `--channel`), `--target <dest>`, `--timeout <ms>`, `--json`
- `channels resolve`: `<entries...>`, `--channel <name>`, `--account <id>`, `--kind <auto|user|group>`, `--json`
- `channels logs`: `--channel <name|all>`, `--lines <n>`, `--json`

`channels status --probe` is the live path: on a reachable gateway it runs per-account
`probeAccount` and optional `auditAccount` checks, so output can include transport
state plus probe results such as `works`, `probe failed`, `audit ok`, or `audit failed`.
If the gateway is unreachable, `channels status` falls back to config-only summaries
instead of live probe output.Do not use `openclaw sessions`, Gateway `sessions.list`, or the agent
`sessions_list` tool as a channel socket-health signal. Those surfaces report
stored conversation rows, not provider runtime state. After a Discord provider
restart, a connected but quiet account may be healthy while no Discord session
row appears until the next inbound or outbound conversation event.

## Add / remove accounts

```
openclaw channels add --channel telegram --token <bot-token>
openclaw channels add --channel nostr --private-key "$NOSTR_PRIVATE_KEY"
openclaw channels remove --channel telegram --delete
```

`openclaw channels add --help` shows per-channel flags (token, private key, app token, signal-cli paths, etc).

`channels remove` only operates on installed/configured channel plugins. Use `channels add` first for installable catalog channels.
For runtime-backed channel plugins, `channels remove` also asks the running Gateway to stop the selected account before it updates config, so disabling or deleting an account does not leave the old listener active until restart.Common non-interactive add surfaces include:

- bot-token channels: `--token`, `--bot-token`, `--app-token`, `--token-file`
- Signal/iMessage transport fields: `--signal-number`, `--cli-path`, `--http-url`, `--http-host`, `--http-port`, `--db-path`, `--service`, `--region`
- Google Chat fields: `--webhook-path`, `--webhook-url`, `--audience-type`, `--audience`
- Matrix fields: `--homeserver`, `--user-id`, `--access-token`, `--password`, `--device-name`, `--initial-sync-limit`
- Nostr fields: `--private-key`, `--relay-urls`
- Tlon fields: `--ship`, `--url`, `--code`, `--group-channels`, `--dm-allowlist`, `--auto-discover-channels`
- `--use-env` for default-account env-backed auth where supported

If a channel plugin needs to be installed during a flag-driven add command, OpenClaw uses the channel’s default install source without opening the interactive plugin install prompt.When you run `openclaw channels add` without flags, the interactive wizard can prompt:

- account ids per selected channel
- optional display names for those accounts
- `Bind configured channel accounts to agents now?`

If you confirm bind now, the wizard asks which agent should own each configured channel account and writes account-scoped routing bindings.You can also manage the same routing rules later with `openclaw agents bindings`, `openclaw agents bind`, and `openclaw agents unbind` (see [agents](https://docs.openclaw.ai/cli/agents)).When you add a non-default account to a channel that is still using single-account top-level settings, OpenClaw promotes account-scoped top-level values into the channel’s account map before writing the new account. Most channels land those values in `channels.<channel>.accounts.default`, but bundled channels can preserve an existing matching promoted account instead. Matrix is the current example: if one named account already exists, or `defaultAccount` points at an existing named account, promotion preserves that account instead of creating a new `accounts.default`.Routing behavior stays consistent:

- Existing channel-only bindings (no `accountId`) continue to match the default account.
- `channels add` does not auto-create or rewrite bindings in non-interactive mode.
- Interactive setup can optionally add account-scoped bindings.

If your config was already in a mixed state (named accounts present and top-level single-account values still set), run `openclaw doctor --fix` to move account-scoped values into the promoted account chosen for that channel. Most channels promote into `accounts.default`; Matrix can preserve an existing named/default target instead.

## Login and logout (interactive)

```
openclaw channels login --channel whatsapp
openclaw channels logout --channel whatsapp
```

- `channels login` supports `--verbose`.
- `channels login` and `logout` can infer the channel when only one supported login target is configured.
- `channels logout` prefers the live Gateway path when reachable, so logout stops any active listener before clearing channel auth state. If a local Gateway is not reachable, it falls back to local auth cleanup.
- Run `channels login` from a terminal on the gateway host. Agent `exec` blocks this interactive login flow; channel-native agent login tools, such as `whatsapp_login`, should be used from chat when available.

## Troubleshooting

- Run `openclaw status --deep` for a broad probe.
- Use `openclaw doctor` for guided fixes.
- `openclaw channels list` prints `Claude: HTTP 403 ... user:profile` → usage snapshot needs the `user:profile` scope. Use `--no-usage`, or provide a claude.ai session key (`CLAUDE_WEB_SESSION_KEY` / `CLAUDE_WEB_COOKIE`), or re-auth via Claude CLI.
- `openclaw channels status` falls back to config-only summaries when the gateway is unreachable. If a supported channel credential is configured via SecretRef but unavailable in the current command path, it reports that account as configured with degraded notes instead of showing it as not configured.

## Capabilities probe

Fetch provider capability hints (intents/scopes where available) plus static feature support:

```
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
```

Notes:

- `--channel` is optional; omit it to list every channel (including extensions).
- `--account` is only valid with `--channel`.
- `--target` accepts `channel:<id>` or a raw numeric channel id and only applies to Discord.
- Probes are provider-specific: Discord intents + optional channel permissions; Slack bot + user scopes; Telegram bot flags + webhook; Signal daemon version; Microsoft Teams app token + Graph roles/scopes (annotated where known). Channels without probes report `Probe: unavailable`.

## Resolve names to IDs

Resolve channel/user names to IDs using the provider directory:

```
openclaw channels resolve --channel slack "#general" "@jane"
openclaw channels resolve --channel discord "My Server/#support" "@someone"
openclaw channels resolve --channel matrix "Project Room"
```

Notes:

- Use `--kind user|group|auto` to force the target type.
- Resolution prefers active matches when multiple entries share the same name.
- `channels resolve` is read-only. If a selected account is configured via SecretRef but that credential is unavailable in the current command path, the command returns degraded unresolved results with notes instead of aborting the entire run.
- `channels resolve` does not install channel plugins. Use `channels add --channel <name>` before resolving names for an installable catalog channel.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Channels overview](https://docs.openclaw.ai/channels)

[\`openclaw tasks\`](https://docs.openclaw.ai/cli/tasks) [Devices](https://docs.openclaw.ai/cli/devices)

Ctrl+I

---

## https://docs.openclaw.ai/cli/channels.md

_Source: <https://docs.openclaw.ai/cli/channels.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Channels

\# \`openclaw channels\`

Manage chat channel accounts and their runtime status on the Gateway.

Related docs:

\\* Channel guides: \[Channels\](/channels)
\\* Gateway configuration: \[Configuration\](/gateway/configuration)

\## Common commands

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw channels list
openclaw channels status
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
openclaw channels resolve --channel slack "#general" "@jane"
openclaw channels logs --channel all
\`\`\`

\## Status / capabilities / resolve / logs

\\* \`channels status\`: \`--probe\`, \`--timeout \`, \`--json\`
\\* \`channels capabilities\`: \`--channel \`, \`--account \` (only with \`--channel\`), \`--target \`, \`--timeout \`, \`--json\`
\\* \`channels resolve\`: \`\`, \`--channel \`, \`--account \`, \`--kind \`, \`--json\`
\\* \`channels logs\`: \`--channel \`, \`--lines \`, \`--json\`

\`channels status --probe\` is the live path: on a reachable gateway it runs per-account
\`probeAccount\` and optional \`auditAccount\` checks, so output can include transport
state plus probe results such as \`works\`, \`probe failed\`, \`audit ok\`, or \`audit failed\`.
If the gateway is unreachable, \`channels status\` falls back to config-only summaries
instead of live probe output.

Do not use \`openclaw sessions\`, Gateway \`sessions.list\`, or the agent
\`sessions\_list\` tool as a channel socket-health signal. Those surfaces report
stored conversation rows, not provider runtime state. After a Discord provider
restart, a connected but quiet account may be healthy while no Discord session
row appears until the next inbound or outbound conversation event.

\## Add / remove accounts

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw channels add --channel telegram --token
openclaw channels add --channel nostr --private-key "$NOSTR\_PRIVATE\_KEY"
openclaw channels remove --channel telegram --delete
\`\`\`

 \`openclaw channels add --help\` shows per-channel flags (token, private key, app token, signal-cli paths, etc).

\`channels remove\` only operates on installed/configured channel plugins. Use \`channels add\` first for installable catalog channels.
For runtime-backed channel plugins, \`channels remove\` also asks the running Gateway to stop the selected account before it updates config, so disabling or deleting an account does not leave the old listener active until restart.

Common non-interactive add surfaces include:

\\* bot-token channels: \`--token\`, \`--bot-token\`, \`--app-token\`, \`--token-file\`
\\* Signal/iMessage transport fields: \`--signal-number\`, \`--cli-path\`, \`--http-url\`, \`--http-host\`, \`--http-port\`, \`--db-path\`, \`--service\`, \`--region\`
\\* Google Chat fields: \`--webhook-path\`, \`--webhook-url\`, \`--audience-type\`, \`--audience\`
\\* Matrix fields: \`--homeserver\`, \`--user-id\`, \`--access-token\`, \`--password\`, \`--device-name\`, \`--initial-sync-limit\`
\\* Nostr fields: \`--private-key\`, \`--relay-urls\`
\\* Tlon fields: \`--ship\`, \`--url\`, \`--code\`, \`--group-channels\`, \`--dm-allowlist\`, \`--auto-discover-channels\`
\\* \`--use-env\` for default-account env-backed auth where supported

If a channel plugin needs to be installed during a flag-driven add command, OpenClaw uses the channel's default install source without opening the interactive plugin install prompt.

When you run \`openclaw channels add\` without flags, the interactive wizard can prompt:

\\* account ids per selected channel
\\* optional display names for those accounts
\\* \`Bind configured channel accounts to agents now?\`

If you confirm bind now, the wizard asks which agent should own each configured channel account and writes account-scoped routing bindings.

You can also manage the same routing rules later with \`openclaw agents bindings\`, \`openclaw agents bind\`, and \`openclaw agents unbind\` (see \[agents\](/cli/agents)).

When you add a non-default account to a channel that is still using single-account top-level settings, OpenClaw promotes account-scoped top-level values into the channel's account map before writing the new account. Most channels land those values in \`channels..accounts.default\`, but bundled channels can preserve an existing matching promoted account instead. Matrix is the current example: if one named account already exists, or \`defaultAccount\` points at an existing named account, promotion preserves that account instead of creating a new \`accounts.default\`.

Routing behavior stays consistent:

\\* Existing channel-only bindings (no \`accountId\`) continue to match the default account.
\\* \`channels add\` does not auto-create or rewrite bindings in non-interactive mode.
\\* Interactive setup can optionally add account-scoped bindings.

If your config was already in a mixed state (named accounts present and top-level single-account values still set), run \`openclaw doctor --fix\` to move account-scoped values into the promoted account chosen for that channel. Most channels promote into \`accounts.default\`; Matrix can preserve an existing named/default target instead.

\## Login and logout (interactive)

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw channels login --channel whatsapp
openclaw channels logout --channel whatsapp
\`\`\`

\\* \`channels login\` supports \`--verbose\`.
\\* \`channels login\` and \`logout\` can infer the channel when only one supported login target is configured.
\\* \`channels logout\` prefers the live Gateway path when reachable, so logout stops any active listener before clearing channel auth state. If a local Gateway is not reachable, it falls back to local auth cleanup.
\\* Run \`channels login\` from a terminal on the gateway host. Agent \`exec\` blocks this interactive login flow; channel-native agent login tools, such as \`whatsapp\_login\`, should be used from chat when available.

\## Troubleshooting

\\* Run \`openclaw status --deep\` for a broad probe.
\\* Use \`openclaw doctor\` for guided fixes.
\\* \`openclaw channels list\` prints \`Claude: HTTP 403 ... user:profile\` → usage snapshot needs the \`user:profile\` scope. Use \`--no-usage\`, or provide a claude.ai session key (\`CLAUDE\_WEB\_SESSION\_KEY\` / \`CLAUDE\_WEB\_COOKIE\`), or re-auth via Claude CLI.
\\* \`openclaw channels status\` falls back to config-only summaries when the gateway is unreachable. If a supported channel credential is configured via SecretRef but unavailable in the current command path, it reports that account as configured with degraded notes instead of showing it as not configured.

\## Capabilities probe

Fetch provider capability hints (intents/scopes where available) plus static feature support:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
\`\`\`

Notes:

\\* \`--channel\` is optional; omit it to list every channel (including extensions).
\\* \`--account\` is only valid with \`--channel\`.
\\* \`--target\` accepts \`channel:\` or a raw numeric channel id and only applies to Discord.
\\* Probes are provider-specific: Discord intents + optional channel permissions; Slack bot + user scopes; Telegram bot flags + webhook; Signal daemon version; Microsoft Teams app token + Graph roles/scopes (annotated where known). Channels without probes report \`Probe: unavailable\`.

\## Resolve names to IDs

Resolve channel/user names to IDs using the provider directory:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw channels resolve --channel slack "#general" "@jane"
openclaw channels resolve --channel discord "My Server/#support" "@someone"
openclaw channels resolve --channel matrix "Project Room"
\`\`\`

Notes:

\\* Use \`--kind user\|group\|auto\` to force the target type.
\\* Resolution prefers active matches when multiple entries share the same name.
\\* \`channels resolve\` is read-only. If a selected account is configured via SecretRef but that credential is unavailable in the current command path, the command returns degraded unresolved results with notes instead of aborting the entire run.
\\* \`channels resolve\` does not install channel plugins. Use \`channels add --channel \` before resolving names for an installable catalog channel.

\## Related

\\* \[CLI reference\](/cli)
\\* \[Channels overview\](/channels)

---

## `openclaw commitments` - OpenClaw

_Source: <https://docs.openclaw.ai/cli/commitments>_

[OpenClaw home page](https://docs.openclaw.ai/)

Agents and sessions

\`openclaw commitments\`

List and manage inferred follow-up commitments.Commitments are opt-in, short-lived follow-up memories created from
conversation context. See [Inferred commitments](https://docs.openclaw.ai/concepts/commitments) for the
conceptual guide.With no subcommand, `openclaw commitments` lists pending commitments.

## Usage

```
openclaw commitments [--all] [--agent <id>] [--status <status>] [--json]
openclaw commitments list [--all] [--agent <id>] [--status <status>] [--json]
openclaw commitments dismiss <id...> [--json]
```

## Options

- `--all`: show all statuses instead of only pending commitments.
- `--agent <id>`: filter to one agent id.
- `--status <status>`: filter by status. Values: `pending`, `sent`,
`dismissed`, `snoozed`, or `expired`.
- `--json`: output machine-readable JSON.

## Examples

List pending commitments:

```
openclaw commitments
```

List every stored commitment:

```
openclaw commitments --all
```

Filter to one agent:

```
openclaw commitments --agent main
```

Find snoozed commitments:

```
openclaw commitments --status snoozed
```

Dismiss one or more commitments:

```
openclaw commitments dismiss cm_abc123 cm_def456
```

Export as JSON:

```
openclaw commitments --all --json
```

## Output

Text output includes:

- commitment id
- status
- kind
- earliest due time
- scope
- suggested check-in text

JSON output also includes the commitment store path and full stored records.

## Related

- [Inferred commitments](https://docs.openclaw.ai/concepts/commitments)
- [Memory overview](https://docs.openclaw.ai/concepts/memory)
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat)
- [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs)

[Memory](https://docs.openclaw.ai/cli/memory) [Message](https://docs.openclaw.ai/cli/message)

Ctrl+I

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

Object assignment replaces the target path by default. Protected map/list paths that commonly hold user-added entries, such as `agents.defaults.models`, `models.providers`, `models.providers.<id>.models`, `plugins.entries`, and `auth.profiles`, refuse replacements that would remove existing entries unless you pass `--replace`.

Use `--merge` when adding entries to those maps:

```
openclaw config set agents.defaults.models '{"openai/gpt-5.4":{}}' --strict-json --merge
openclaw config set models.providers.ollama.models '[{"id":"llama3.2","name":"Llama 3.2"}]' --strict-json --merge
```

Use `--replace` only when you intentionally want the provided value to become the complete target value.

## `config set` modes

`openclaw config set` supports four assignment styles:

- Value mode

- SecretRef builder mode

- Provider builder mode

- Batch mode

```
openclaw config set <path> <value>
```

```
openclaw config set channels.discord.token \
  --ref-provider default \
  --ref-source env \
  --ref-id DISCORD_BOT_TOKEN
```

Provider builder mode targets `secrets.providers.<alias>` paths only:

```
openclaw config set secrets.providers.vault \
  --provider-source exec \
  --provider-command /usr/local/bin/openclaw-vault \
  --provider-arg read \
  --provider-arg openai/api-key \
  --provider-timeout-ms 5000
```

```
openclaw config set --batch-json '[\
  {\
    "path": "secrets.providers.default",\
    "provider": { "source": "env" }\
  },\
  {\
    "path": "channels.discord.token",\
    "ref": { "source": "env", "provider": "default", "id": "DISCORD_BOT_TOKEN" }\
  }\
]'
```

```
openclaw config set --batch-file ./config-set.batch.json --dry-run
```

SecretRef assignments are rejected on unsupported runtime-mutable surfaces (for example `hooks.token`, `commands.ownerDisplaySecret`, Discord thread-binding webhook tokens, and WhatsApp creds JSON). See [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface).

Batch parsing always uses the batch payload (`--batch-json`/`--batch-file`) as the source of truth. `--strict-json` / `--json` do not change batch parsing behavior.

## `config patch`

Use `config patch` when you want to paste or pipe a config-shaped patch instead of running many path-based `config set` commands. The input is a JSON5 object. Objects merge recursively, arrays and scalar values replace the target value, and `null` deletes the target path.

```
openclaw config patch --file ./openclaw.patch.json5 --dry-run
openclaw config patch --file ./openclaw.patch.json5
```

You can also pipe a patch over stdin, which is useful for remote setup scripts:

```
ssh openclaw-host 'openclaw config patch --stdin --dry-run' < ./openclaw.patch.json5
ssh openclaw-host 'openclaw config patch --stdin' < ./openclaw.patch.json5
```

Example patch:

```
{
  channels: {
    slack: {
      enabled: true,
      mode: "socket",
      botToken: { source: "env", provider: "default", id: "SLACK_BOT_TOKEN" },
      appToken: { source: "env", provider: "default", id: "SLACK_APP_TOKEN" },
      groupPolicy: "open",
      requireMention: false,
    },
    discord: {
      enabled: true,
      token: { source: "env", provider: "default", id: "DISCORD_BOT_TOKEN" },
      dmPolicy: "disabled",
      dm: { enabled: false },
      groupPolicy: "allowlist",
    },
  },
  agents: {
    defaults: {
      model: { primary: "openai/gpt-5.5" },
      models: {
        "openai/gpt-5.5": { params: { fastMode: true } },
      },
    },
  },
}
```

Use `--replace-path <path>` when one object or array must become exactly the provided value instead of being recursively patched:

```
openclaw config patch --file ./discord.patch.json5 --replace-path 'channels.discord.guilds["123"].channels'
```

`--dry-run` runs schema and SecretRef resolvability checks without writing. Exec-backed SecretRefs are skipped by default during dry-run; add `--allow-exec` when you intentionally want dry-run to execute provider commands.JSON path/value mode remains supported for both SecretRefs and providers:

```
openclaw config set channels.discord.token \
  '{"source":"env","provider":"default","id":"DISCORD_BOT_TOKEN"}' \
  --strict-json

openclaw config set secrets.providers.vaultfile \
  '{"source":"file","path":"/etc/openclaw/secrets.json","mode":"json"}' \
  --strict-json
```

## Provider builder flags

Provider builder targets must use `secrets.providers.<alias>` as the path.

Common flags

- `--provider-source <env|file|exec>`
- `--provider-timeout-ms <ms>` (`file`, `exec`)

Env provider (--provider-source env)

- `--provider-allowlist <ENV_VAR>` (repeatable)

File provider (--provider-source file)

- `--provider-path <path>` (required)
- `--provider-mode <singleValue|json>`
- `--provider-max-bytes <bytes>`
- `--provider-allow-insecure-path`

Exec provider (--provider-source exec)

- `--provider-command <path>` (required)
- `--provider-arg <arg>` (repeatable)
- `--provider-no-output-timeout-ms <ms>`
- `--provider-max-output-bytes <bytes>`
- `--provider-json-only`
- `--provider-env <KEY=VALUE>` (repeatable)
- `--provider-pass-env <ENV_VAR>` (repeatable)
- `--provider-trusted-dir <path>` (repeatable)
- `--provider-allow-insecure-path`
- `--provider-allow-symlink-command`

Hardened exec provider example:

```
openclaw config set secrets.providers.vault \
  --provider-source exec \
  --provider-command /usr/local/bin/openclaw-vault \
  --provider-arg read \
  --provider-arg openai/api-key \
  --provider-json-only \
  --provider-pass-env VAULT_TOKEN \
  --provider-trusted-dir /usr/local/bin \
  --provider-timeout-ms 5000
```

## Dry run

Use `--dry-run` to validate changes without writing `openclaw.json`.

```
openclaw config set channels.discord.token \
  --ref-provider default \
  --ref-source env \
  --ref-id DISCORD_BOT_TOKEN \
  --dry-run

openclaw config set channels.discord.token \
  --ref-provider default \
  --ref-source env \
  --ref-id DISCORD_BOT_TOKEN \
  --dry-run \
  --json

openclaw config set channels.discord.token \
  --ref-provider vault \
  --ref-source exec \
  --ref-id discord/token \
  --dry-run \
  --allow-exec
```

Dry-run behavior

- Builder mode: runs SecretRef resolvability checks for changed refs/providers.
- JSON mode (`--strict-json`, `--json`, or batch mode): runs schema validation plus SecretRef resolvability checks.
- Policy validation also runs for known unsupported SecretRef target surfaces.
- Policy checks evaluate the full post-change config, so parent-object writes (for example setting `hooks` as an object) cannot bypass unsupported-surface validation.
- Exec SecretRef checks are skipped by default during dry-run to avoid command side effects.
- Use `--allow-exec` with `--dry-run` to opt in to exec SecretRef checks (this may execute provider commands).
- `--allow-exec` is dry-run only and errors if used without `--dry-run`.

--dry-run --json fields

`--dry-run --json` prints a machine-readable report:

- `ok`: whether dry-run passed
- `operations`: number of assignments evaluated
- `checks`: whether schema/resolvability checks ran
- `checks.resolvabilityComplete`: whether resolvability checks ran to completion (false when exec refs are skipped)
- `refsChecked`: number of refs actually resolved during dry-run
- `skippedExecRefs`: number of exec refs skipped because `--allow-exec` was not set
- `errors`: structured schema/resolvability failures when `ok=false`

### JSON output shape

```
{
  ok: boolean,
  operations: number,
  configPath: string,
  inputModes: ["value" | "json" | "builder", ...],
  checks: {
    schema: boolean,
    resolvability: boolean,
    resolvabilityComplete: boolean,
  },
  refsChecked: number,
  skippedExecRefs: number,
  errors?: [\
    {\
      kind: "schema" | "resolvability",\
      message: string,\
      ref?: string, // present for resolvability errors\
    },\
  ],
}
```

- Success example

- Failure example

```
{
  "ok": true,
  "operations": 1,
  "configPath": "~/.openclaw/openclaw.json",
  "inputModes": ["builder"],
  "checks": {
    "schema": false,
    "resolvability": true,
    "resolvabilityComplete": true
  },
  "refsChecked": 1,
  "skippedExecRefs": 0
}
```

```
{
  "ok": false,
  "operations": 1,
  "configPath": "~/.openclaw/openclaw.json",
  "inputModes": ["builder"],
  "checks": {
    "schema": false,
    "resolvability": true,
    "resolvabilityComplete": true
  },
  "refsChecked": 1,
  "skippedExecRefs": 0,
  "errors": [\
    {\
      "kind": "resolvability",\
      "message": "Error: Environment variable \"MISSING_TEST_SECRET\" is not set.",\
      "ref": "env:default:MISSING_TEST_SECRET"\
    }\
  ]
}
```

If dry-run fails

- `config schema validation failed`: your post-change config shape is invalid; fix path/value or provider/ref object shape.
- `Config policy validation failed: unsupported SecretRef usage`: move that credential back to plaintext/string input and keep SecretRefs on supported surfaces only.
- `SecretRef assignment(s) could not be resolved`: referenced provider/ref currently cannot resolve (missing env var, invalid file pointer, exec provider failure, or provider/source mismatch).
- `Dry run note: skipped <n> exec SecretRef resolvability check(s)`: dry-run skipped exec refs; rerun with `--allow-exec` if you need exec resolvability validation.
- For batch mode, fix failing entries and rerun `--dry-run` before writing.

## Write safety

`openclaw config set` and other OpenClaw-owned config writers validate the full post-change config before committing it to disk. If the new payload fails schema validation or looks like a destructive clobber, the active config is left alone and the rejected payload is saved beside it as `openclaw.json.rejected.*`.

The active config path must be a regular file. Symlinked `openclaw.json` layouts are unsupported for writes; use `OPENCLAW_CONFIG_PATH` to point directly at the real file instead.

Prefer CLI writes for small edits:

```
openclaw config set gateway.reload.mode hybrid --dry-run
openclaw config set gateway.reload.mode hybrid
openclaw config validate
```

If a write is rejected, inspect the saved payload and fix the full config shape:

```
CONFIG="$(openclaw config file)"
ls -lt "$CONFIG".rejected.* 2>/dev/null | head
openclaw config validate
```

Direct editor writes are still allowed, but the running Gateway treats them as untrusted until they validate. Invalid direct edits can be restored from the last-known-good backup during startup or hot reload. See [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting#gateway-restored-last-known-good-config).Whole-file recovery is reserved for globally broken config, such as parse errors, root-level schema failures, legacy migration failures, or mixed plugin and root failures. If validation fails only under `plugins.entries.<id>...`, OpenClaw keeps the active `openclaw.json` in place and reports the plugin-local issue instead of restoring `.last-good`. This prevents plugin schema changes or `minHostVersion` skew from rolling back unrelated user settings such as models, providers, auth profiles, channels, gateway exposure, tools, memory, browser, or cron config.

## Subcommands

- `config file`: Print the active config file path (resolved from `OPENCLAW_CONFIG_PATH` or default location). The path should name a regular file, not a symlink.

Restart the gateway after edits.

## Validate

Validate the current config against the active schema without starting the gateway.

```
openclaw config validate
openclaw config validate --json
```

After `openclaw config validate` is passing, you can use the local TUI to have an embedded agent compare the active config against the docs while you validate each change from the same terminal:

If validation is already failing, start with `openclaw configure` or `openclaw doctor --fix`. `openclaw chat` does not bypass the invalid-config guard.

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

Typical repair loop:

1

[Navigate to header](https://docs.openclaw.ai/cli/config#)

Compare with docs

Ask the agent to compare your current config with the relevant docs page and suggest the smallest fix.

2

[Navigate to header](https://docs.openclaw.ai/cli/config#)

Apply targeted edits

Apply targeted edits with `openclaw config set` or `openclaw configure`.

3

[Navigate to header](https://docs.openclaw.ai/cli/config#)

Re-validate

Rerun `openclaw config validate` after each change.

4

[Navigate to header](https://docs.openclaw.ai/cli/config#)

Doctor for runtime issues

If validation passes but the runtime is still unhealthy, run `openclaw doctor` or `openclaw doctor --fix` for migration and repair help.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)

[Sandbox CLI](https://docs.openclaw.ai/cli/sandbox) [Configure](https://docs.openclaw.ai/cli/configure)

Ctrl+I

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
config operations and asks for approval first.

```
setup
setup workspace ~/Projects/work
setup workspace ~/Projects/work model openai/gpt-5.5
```

When no model is configured, setup selects the first usable backend in this
order and tells you what it chose:

- existing explicit model, if already configured
- `OPENAI_API_KEY` -\> `openai/gpt-5.5`
- `ANTHROPIC_API_KEY` -\> `anthropic/claude-opus-4-7`
- Claude Code CLI -> `claude-cli/claude-opus-4-7`
- Codex CLI -> `codex-cli/gpt-5.5`

If none are available, setup still writes the default workspace and leaves the
model unset. Install or log into Codex/Claude Code, or expose
`OPENAI_API_KEY`/`ANTHROPIC_API_KEY`, then run setup again.

## Model-Assisted Planner

Crestodian always starts in deterministic mode. For fuzzy commands that the
deterministic parser does not understand, local Crestodian can make one bounded
planner turn through OpenClaw’s normal runtime paths. It first uses the
configured OpenClaw model. If no configured model is usable yet, it can fall
back to local runtimes already present on the machine:

- Claude Code CLI: `claude-cli/claude-opus-4-7`
- Codex app-server harness: `openai/gpt-5.5` with `agentRuntime.id: "codex"`
- Codex CLI: `codex-cli/gpt-5.5`

The model-assisted planner cannot mutate config directly. It must translate the
request into one of Crestodian’s typed commands, then the normal approval and
audit rules apply. Crestodian prints the model it used and the interpreted
command before it runs anything. Configless fallback planner turns are
temporary, tool-disabled where the runtime supports it, and use a temporary
workspace/session.Message-channel rescue mode does not use the model-assisted planner. Remote
rescue stays deterministic so a broken or compromised normal agent path cannot
be used as a config editor.

## Switching to an agent

Use a natural-language selector to leave Crestodian and open the normal TUI:

```
talk to agent
talk to work agent
switch to main agent
```

`openclaw tui`, `openclaw chat`, and `openclaw terminal` still open the normal
agent TUI directly. They do not start Crestodian.After switching into the normal TUI, use `/crestodian` to return to Crestodian.
You can include a follow-up request:

```
/crestodian
/crestodian restart gateway
```

Agent switches inside the TUI leave a breadcrumb that `/crestodian` is available.

## Message rescue mode

Message rescue mode is the message-channel entrypoint for Crestodian. It is for
the case where your normal agent is dead, but a trusted channel such as WhatsApp
still receives commands.Supported text command:

- `/crestodian <request>`

Operator flow:

```
You, in a trusted owner DM: /crestodian status
OpenClaw: Crestodian rescue mode. Gateway reachable: no. Config valid: no.
You: /crestodian restart gateway
OpenClaw: Plan: restart the Gateway. Reply /crestodian yes to apply.
You: /crestodian yes
OpenClaw: Applied. Audit entry written.
```

Agent creation can also be queued from the local prompt or rescue mode:

```
create agent work workspace ~/Projects/work model openai/gpt-5.5
/crestodian create agent work workspace ~/Projects/work
```

Remote rescue mode is an admin surface. It must be treated like remote config
repair, not like normal chat.Security contract for remote rescue:

- Disabled when sandboxing is active. If an agent/session is sandboxed,
Crestodian must refuse remote rescue and explain that local CLI repair is
required.
- Default effective state is `auto`: allow remote rescue only in trusted YOLO
operation, where the runtime already has unsandboxed local authority.
- Require an explicit owner identity. Rescue must not accept wildcard sender
rules, open group policy, unauthenticated webhooks, or anonymous channels.
- Owner DMs only by default. Group/channel rescue requires explicit opt-in.
- Plugin search and list are read-only. Plugin install is local-only by default
because it downloads executable code. Plugin uninstall can be allowed as an
approved repair operation when rescue policy permits persistent writes.
- Remote rescue cannot open the local TUI or switch into an interactive agent
session. Use local `openclaw` for agent handoff.
- Persistent writes still require approval, even in rescue mode.
- Audit every applied rescue operation. Message-channel rescue records channel,
account, sender, and source-address metadata. Config-mutating operations also
record config hashes before and after.
- Never echo secrets. SecretRef inspection should report availability, not
values.
- If the Gateway is alive, prefer Gateway typed operations. If the Gateway is
dead, use only the minimal local repair surface that does not depend on the
normal agent loop.

Config shape:

```
{
  "crestodian": {
    "rescue": {
      "enabled": "auto",
      "ownerDmOnly": true,
    },
  },
}
```

`enabled` should accept:

- `"auto"`: default. Allow only when the effective runtime is YOLO and
sandboxing is off.
- `false`: never allow message-channel rescue.
- `true`: explicitly allow rescue when the owner/channel checks pass. This
still must not bypass the sandboxing denial.

The default `"auto"` YOLO posture is:

- sandbox mode resolves to `off`
- `tools.exec.security` resolves to `full`
- `tools.exec.ask` resolves to `off`

Remote rescue is covered by the Docker lane:

```
pnpm test:docker:crestodian-rescue
```

Configless local planner fallback is covered by:

```
pnpm test:docker:crestodian-planner
```

An opt-in live channel command-surface smoke checks `/crestodian status` plus a
persistent approval roundtrip through the rescue handler:

```
pnpm test:live:crestodian-rescue-channel
```

Fresh configless setup through Crestodian is covered by:

```
pnpm test:docker:crestodian-first-run
```

That lane starts with an empty state dir, routes bare `openclaw` to Crestodian,
sets the default model, creates an additional agent, configures Discord through
a plugin enablement plus token SecretRef, validates config, and checks the audit
log. QA Lab also has a repo-backed scenario for the same Ring 0 flow:

```
pnpm openclaw qa suite --scenario crestodian-ring-zero-setup
```

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Doctor](https://docs.openclaw.ai/cli/doctor)
- [TUI](https://docs.openclaw.ai/cli/tui)
- [Sandbox](https://docs.openclaw.ai/cli/sandbox)
- [Security](https://docs.openclaw.ai/cli/security)

[Backup](https://docs.openclaw.ai/cli/backup) [Daemon](https://docs.openclaw.ai/cli/daemon)

Ctrl+I

---

## Cron - OpenClaw

_Source: <https://docs.openclaw.ai/cli/cron>_

# `openclaw cron`

Manage cron jobs for the Gateway scheduler.

Run `openclaw cron --help` for the full command surface. See [Cron jobs](https://docs.openclaw.ai/automation/cron-jobs) for the conceptual guide.

## Sessions

`--session` accepts `main`, `isolated`, `current`, or `session:<id>`.

Session keys

- `main` binds to the agent’s main session.
- `isolated` creates a fresh transcript and session id for each run.
- `current` binds to the active session at creation time.
- `session:<id>` pins to an explicit persistent session key.

Isolated session semantics

Isolated runs reset ambient conversation context. Channel and group routing, send/queue policy, elevation, origin, and ACP runtime binding are reset for the new run. Safe preferences and explicit user-selected model or auth overrides can carry across runs.

## Delivery

`openclaw cron list` and `openclaw cron show <job-id>` preview the resolved delivery route. For `channel: "last"`, the preview shows whether the route resolved from the main or current session, or will fail closed.Provider-prefixed targets can disambiguate unresolved announce channels. For example, `to: "telegram:123"` selects Telegram when `delivery.channel` is omitted or `last`. Only prefixes advertised by the loaded plugin are provider selectors. If `delivery.channel` is explicit, the prefix must match that channel; `channel: "whatsapp"` with `to: "telegram:123"` is rejected. Service prefixes such as `imessage:` and `sms:` remain channel-owned target syntax.

Isolated `cron add` jobs default to `--announce` delivery. Use `--no-deliver` to keep output internal. `--deliver` remains as a deprecated alias for `--announce`.

### Delivery ownership

Isolated cron chat delivery is shared between the agent and the runner:

- The agent can send directly using the `message` tool when a chat route is available.
- `announce` fallback-delivers the final reply only when the agent did not send directly to the resolved target.
- `webhook` posts the finished payload to a URL.
- `none` disables runner fallback delivery.

`--announce` is runner fallback delivery for the final reply. `--no-deliver` disables that fallback but does not remove the agent’s `message` tool when a chat route is available.Reminders created from an active chat preserve the live chat delivery target for fallback announce delivery. Internal session keys may be lowercase; do not use them as a source of truth for case-sensitive provider IDs such as Matrix room IDs.

### Failure delivery

Failure notifications resolve in this order:

1. `delivery.failureDestination` on the job.
2. Global `cron.failureDestination`.
3. The job’s primary announce target (when no explicit failure destination is set).

Main-session jobs may only use `delivery.failureDestination` when primary delivery mode is `webhook`. Isolated jobs accept it in all modes.

Note: isolated cron runs treat run-level agent failures as job errors even when
no reply payload is produced, so model/provider failures still increment error
counters and trigger failure notifications.

## Scheduling

### One-shot jobs

`--at <datetime>` schedules a one-shot run. Offset-less datetimes are treated as UTC unless you also pass `--tz <iana>`, which interprets the wall-clock time in the given timezone.

One-shot jobs delete after success by default. Use `--keep-after-run` to preserve them.

### Recurring jobs

Recurring jobs use exponential retry backoff after consecutive errors: 30s, 1m, 5m, 15m, 60m. The schedule returns to normal after the next successful run.Skipped runs are tracked separately from execution errors. They do not affect retry backoff, but `openclaw cron edit <job-id> --failure-alert-include-skipped` can opt failure alerts into repeated skipped-run notifications.For isolated jobs that target a local configured model provider, cron runs a lightweight provider preflight before starting the agent turn. Loopback, private-network, and `.local``api: "ollama"` providers are probed at `/api/tags`; local OpenAI-compatible providers such as vLLM, SGLang, and LM Studio are probed at `/models`. If the endpoint is unreachable, the run is recorded as `skipped` and retried on a later schedule; matching dead endpoints are cached for 5 minutes to avoid many jobs hammering the same local server.Note: cron job definitions live in `jobs.json`, while pending runtime state lives in `jobs-state.json`. If `jobs.json` is edited externally, the Gateway reloads changed schedules and clears stale pending slots; formatting-only rewrites do not clear the pending slot.

### Manual runs

`openclaw cron run` returns as soon as the manual run is queued. Successful responses include `{ ok: true, enqueued: true, runId }`. Use `openclaw cron runs --id <job-id>` to follow the eventual outcome.

`openclaw cron run <job-id>` force-runs by default. Use `--due` to keep the older “only run if due” behavior.

## Models

`cron add|edit --model <ref>` selects an allowed model for the job.

If the model is not allowed or cannot be resolved, cron fails the run with an explicit validation error instead of falling back to the job’s agent or default model selection.

Cron `--model` is a **job primary**, not a chat-session `/model` override. That means:

- Configured model fallbacks still apply when the selected job model fails.
- Per-job payload `fallbacks` replaces the configured fallback list when present.
- An empty per-job fallback list (`fallbacks: []` in the job payload/API) makes the cron run strict.
- When a job has `--model` but no fallback list is configured, OpenClaw passes an explicit empty fallback override so the agent primary is not appended as a hidden retry target.

### Isolated cron model precedence

Isolated cron resolves the active model in this order:

1. Gmail-hook override.
2. Per-job `--model`.
3. Stored cron-session model override (when the user selected one).
4. Agent or default model selection.

### Fast mode

Isolated cron fast mode follows the resolved live model selection. Model config `params.fastMode` applies by default, but a stored session `fastMode` override still wins over config.

### Live model switch retries

If an isolated run throws `LiveSessionModelSwitchError`, cron persists the switched provider and model (and switched auth profile override when present) for the active run before retrying. The outer retry loop is bounded to two switch retries after the initial attempt, then aborts instead of looping forever.

## Run output and denials

### Stale acknowledgement suppression

Isolated cron turns suppress stale acknowledgement-only replies. If the first result is just an interim status update and no descendant subagent run is responsible for the eventual answer, cron re-prompts once for the real result before delivery.

### Silent token suppression

If an isolated cron run returns only the silent token (`NO_REPLY` or `no_reply`), cron suppresses both direct outbound delivery and the fallback queued summary path, so nothing is posted back to chat.

### Structured denials

Isolated cron runs prefer structured execution-denial metadata from the embedded run, then fall back to known denial markers in final output, such as `SYSTEM_RUN_DENIED`, `INVALID_REQUEST`, and approval-binding refusal phrases.`cron list` and run history surface the denial reason instead of reporting a blocked command as `ok`.

## Retention

Retention and pruning are controlled in config:

- `cron.sessionRetention` (default `24h`) prunes completed isolated run sessions.
- `cron.runLog.maxBytes` and `cron.runLog.keepLines` prune `~/.openclaw/cron/runs/<jobId>.jsonl`.

## Migrating older jobs

If you have cron jobs from before the current delivery and store format, run `openclaw doctor --fix`. Doctor normalizes legacy cron fields (`jobId`, `schedule.cron`, top-level delivery fields including legacy `threadId`, payload `provider` delivery aliases) and migrates simple `notify: true` webhook fallback jobs to explicit webhook delivery when `cron.webhook` is configured.

## Common edits

Update delivery settings without changing the message:

```
openclaw cron edit <job-id> --announce --channel telegram --to "123456789"
```

Disable delivery for an isolated job:

```
openclaw cron edit <job-id> --no-deliver
```

Enable lightweight bootstrap context for an isolated job:

```
openclaw cron edit <job-id> --light-context
```

Announce to a specific channel:

```
openclaw cron edit <job-id> --announce --channel slack --to "channel:C1234567890"
```

Announce to a Telegram forum topic:

```
openclaw cron edit <job-id> --announce --channel telegram --to "-1001234567890" --thread-id 42
```

Create an isolated job with lightweight bootstrap context:

```
openclaw cron add \
  --name "Lightweight morning brief" \
  --cron "0 7 * * *" \
  --session isolated \
  --message "Summarize overnight updates." \
  --light-context \
  --no-deliver
```

`--light-context` applies to isolated agent-turn jobs only. For cron runs, lightweight mode keeps bootstrap context empty instead of injecting the full workspace bootstrap set.

## Common admin commands

Manual run and inspection:

```
openclaw cron list
openclaw cron show <job-id>
openclaw cron run <job-id>
openclaw cron run <job-id> --due
openclaw cron runs --id <job-id> --limit 50
```

`cron runs` entries include delivery diagnostics with the intended cron target, the resolved target, message-tool sends, fallback use, and delivered state.Agent and session retargeting:

```
openclaw cron edit <job-id> --agent ops
openclaw cron edit <job-id> --clear-agent
openclaw cron edit <job-id> --session current
openclaw cron edit <job-id> --session "session:daily-brief"
```

`openclaw cron add` warns when `--agent` is omitted on agent-turn jobs and falls back to the default agent (`main`). Pass `--agent <id>` at create time to pin a specific agent.Delivery tweaks:

```
openclaw cron edit <job-id> --announce --channel slack --to "channel:C1234567890"
openclaw cron edit <job-id> --best-effort-deliver
openclaw cron edit <job-id> --no-best-effort-deliver
openclaw cron edit <job-id> --no-deliver
```

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs)

[Browser](https://docs.openclaw.ai/cli/browser) [Flows (redirect)](https://docs.openclaw.ai/cli/flows)

Ctrl+I

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

Returns the revoke result as JSON.

## Common options

- `--url <url>`: Gateway WebSocket URL (defaults to `gateway.remote.url` when configured).
- `--token <token>`: Gateway token (if required).
- `--password <password>`: Gateway password (password auth).
- `--timeout <ms>`: RPC timeout.
- `--json`: JSON output (recommended for scripting).

When you set `--url`, the CLI does not fall back to config or environment credentials. Pass `--token` or `--password` explicitly. Missing explicit credentials is an error.

## Notes

- Token rotation returns a new token (sensitive). Treat it like a secret.
- These commands require `operator.pairing` (or `operator.admin`) scope.
- `gateway.nodes.pairing.autoApproveCidrs` is an opt-in Gateway policy for
fresh node device pairing only; it does not change CLI approval authority.
- Token rotation and revocation stay inside the approved pairing role set and
approved scope baseline for that device. A stray cached token entry does not
grant a token-management target.
- For paired-device token sessions, cross-device management is admin-only:
`remove`, `rotate`, and `revoke` are self-only unless the caller has
`operator.admin`.
- Token mutation is also caller-scope contained: a pairing-only session cannot
rotate or revoke a token that currently carries `operator.admin` or
`operator.write`.
- `devices clear` is intentionally gated by `--yes`.
- If pairing scope is unavailable on local loopback (and no explicit `--url` is passed), list/approve can use a local pairing fallback.
- `devices approve` requires an explicit request ID before minting tokens; omitting `requestId` or passing `--latest` only previews the newest pending request.

## Token drift recovery checklist

Use this when Control UI or other clients keep failing with `AUTH_TOKEN_MISMATCH` or `AUTH_DEVICE_TOKEN_MISMATCH`.

1. Confirm current gateway token source:

```
openclaw config get gateway.auth.token
```

2. List paired devices and identify the affected device id:

```
openclaw devices list
```

3. Rotate operator token for the affected device:

```
openclaw devices rotate --device <deviceId> --role operator
```

4. If rotation is not enough, remove stale pairing and approve again:

```
openclaw devices remove <deviceId>
openclaw devices list
openclaw devices approve <requestId>
```

5. Retry client connection with the current shared token/password.

Notes:

- Normal reconnect auth precedence is explicit shared token/password first, then explicit `deviceToken`, then stored device token, then bootstrap token.
- Trusted `AUTH_TOKEN_MISMATCH` recovery can temporarily send both the shared token and the stored device token together for the one bounded retry.

Related:

- [Dashboard auth troubleshooting](https://docs.openclaw.ai/web/dashboard#if-you-see-unauthorized-1008)
- [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting#dashboard-control-ui-connectivity)

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Nodes](https://docs.openclaw.ai/nodes)

[Channels](https://docs.openclaw.ai/cli/channels) [Directory](https://docs.openclaw.ai/cli/directory)

Ctrl+I

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
- Doctor auto-migrates legacy flat Talk config (`talk.voiceId`, `talk.modelId`, and friends) into `talk.provider` \+ `talk.providers.<provider>`.
- Repeat `doctor --fix` runs no longer report/apply Talk normalization when the only difference is object key order.
- Doctor includes a memory-search readiness check and can recommend `openclaw configure --section model` when embedding credentials are missing.
- Doctor warns when no command owner is configured. The command owner is the human operator account allowed to run owner-only commands and approve dangerous actions. DM pairing only lets someone talk to the bot; if you approved a sender before first-owner bootstrap existed, set `commands.ownerAllowFrom` explicitly.
- Doctor warns when Codex-mode agents are configured and personal Codex CLI assets exist in the operator’s Codex home. Local Codex app-server launches use isolated per-agent homes, so use `openclaw migrate codex --dry-run` to inventory assets that should be promoted deliberately.
- Doctor warns when skills allowed for the default agent are unavailable in the current runtime environment because bins, env vars, config, or OS requirements are missing. `doctor --fix` can disable those unavailable skills with `skills.entries.<skill>.enabled=false`; install/configure the missing requirement instead when you want to keep the skill active.
- If sandbox mode is enabled but Docker is unavailable, doctor reports a high-signal warning with remediation (`install Docker` or `openclaw config set agents.defaults.sandbox.mode off`).
- If `gateway.auth.token`/`gateway.auth.password` are SecretRef-managed and unavailable in the current command path, doctor reports a read-only warning and does not write plaintext fallback credentials.
- If channel SecretRef inspection fails in a fix path, doctor continues and reports a warning instead of exiting early.
- After state-directory migrations, doctor warns when enabled default Telegram or Discord accounts depend on env fallback and `TELEGRAM_BOT_TOKEN` or `DISCORD_BOT_TOKEN` is unavailable to the doctor process.
- Telegram `allowFrom` username auto-resolution (`doctor --fix`) requires a resolvable Telegram token in the current command path. If token inspection is unavailable, doctor reports a warning and skips auto-resolution for that pass.

## macOS: `launchctl` env overrides

If you previously ran `launchctl setenv OPENCLAW_GATEWAY_TOKEN ...` (or `...PASSWORD`), that value overrides your config file and can cause persistent “unauthorized” errors.

```
launchctl getenv OPENCLAW_GATEWAY_TOKEN
launchctl getenv OPENCLAW_GATEWAY_PASSWORD

launchctl unsetenv OPENCLAW_GATEWAY_TOKEN
launchctl unsetenv OPENCLAW_GATEWAY_PASSWORD
```

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Gateway doctor](https://docs.openclaw.ai/gateway/doctor)

[Daemon](https://docs.openclaw.ai/cli/daemon) [Gateway](https://docs.openclaw.ai/cli/gateway)

Ctrl+I

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

### Startup profiling

- Set `OPENCLAW_GATEWAY_STARTUP_TRACE=1` to log phase timings during Gateway startup, including per-phase `eventLoopMax` delay and plugin lookup-table timings for installed-index, manifest registry, startup planning, and owner-map work.
- Set `OPENCLAW_DIAGNOSTICS=timeline` with `OPENCLAW_DIAGNOSTICS_TIMELINE_PATH=<path>` to write a best-effort JSONL startup diagnostics timeline for external QA harnesses. You can also enable the flag with `diagnostics.flags: ["timeline"]` in config; the path is still env-provided. Add `OPENCLAW_DIAGNOSTICS_EVENT_LOOP=1` to include event-loop samples.
- Run `pnpm test:startup:gateway -- --runs 5 --warmup 1` to benchmark Gateway startup. The benchmark records first process output, `/healthz`, `/readyz`, startup trace timings, event-loop delay, and plugin lookup-table timing details.

## Query a running Gateway

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

### `gateway health`

```
openclaw gateway health --url ws://127.0.0.1:18789
```

The HTTP `/healthz` endpoint is a liveness probe: it returns once the server can answer HTTP. The HTTP `/readyz` endpoint is stricter and stays red while startup plugin sidecars, channels, or configured hooks are still settling. Local or authenticated detailed readiness responses include an `eventLoop` diagnostic block with event-loop delay, event-loop utilization, CPU core ratio, and a `degraded` flag.

### `gateway usage-cost`

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

### `gateway stability`

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

### `gateway diagnostics export`

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

### `gateway status`

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

### `gateway probe`

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

#### Remote over SSH (Mac app parity)

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

### `gateway call <method>`

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

## Manage the Gateway service

```
openclaw gateway install
openclaw gateway start
openclaw gateway stop
openclaw gateway restart
openclaw gateway uninstall
```

### Install with a wrapper

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

## Discover gateways (Bonjour)

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

### `gateway discover`

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

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Gateway runbook](https://docs.openclaw.ai/gateway)

[Doctor](https://docs.openclaw.ai/cli/doctor) [Health](https://docs.openclaw.ai/cli/health)

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

## Hooks - OpenClaw

_Source: <https://docs.openclaw.ai/cli/hooks>_

# `openclaw hooks`

Manage agent hooks (event-driven automations for commands like `/new`, `/reset`, and gateway startup).Running `openclaw hooks` with no subcommand is equivalent to `openclaw hooks list`.Related:

- Hooks: [Hooks](https://docs.openclaw.ai/automation/hooks)
- Plugin hooks: [Plugin hooks](https://docs.openclaw.ai/plugins/hooks)

## List all hooks

```
openclaw hooks list
```

List all discovered hooks from workspace, managed, extra, and bundled directories.
Gateway startup does not load internal hook handlers until at least one internal hook is configured.**Options:**

- `--eligible`: Show only eligible hooks (requirements met)
- `--json`: Output as JSON
- `-v, --verbose`: Show detailed information including missing requirements

**Example output:**

```
Hooks (4/4 ready)

Ready:
  🚀 boot-md ✓ - Run BOOT.md on gateway startup
  📎 bootstrap-extra-files ✓ - Inject extra workspace bootstrap files during agent bootstrap
  📝 command-logger ✓ - Log all command events to a centralized audit file
  💾 session-memory ✓ - Save session context to memory when /new or /reset command is issued
```

**Example (verbose):**

```
openclaw hooks list --verbose
```

Shows missing requirements for ineligible hooks.**Example (JSON):**

```
openclaw hooks list --json
```

Returns structured JSON for programmatic use.

## Get hook information

```
openclaw hooks info <name>
```

Show detailed information about a specific hook.**Arguments:**

- `<name>`: Hook name or hook key (e.g., `session-memory`)

**Options:**

- `--json`: Output as JSON

**Example:**

```
openclaw hooks info session-memory
```

**Output:**

```
💾 session-memory ✓ Ready

Save session context to memory when /new or /reset command is issued

Details:
  Source: openclaw-bundled
  Path: /path/to/openclaw/hooks/bundled/session-memory/HOOK.md
  Handler: /path/to/openclaw/hooks/bundled/session-memory/handler.ts
  Homepage: https://docs.openclaw.ai/automation/hooks#session-memory
  Events: command:new, command:reset

Requirements:
  Config: ✓ workspace.dir
```

## Check hooks eligibility

```
openclaw hooks check
```

Show summary of hook eligibility status (how many are ready vs. not ready).**Options:**

- `--json`: Output as JSON

**Example output:**

```
Hooks Status

Total hooks: 4
Ready: 4
Not ready: 0
```

## Enable a Hook

```
openclaw hooks enable <name>
```

Enable a specific hook by adding it to your config (`~/.openclaw/openclaw.json` by default).**Note:** Workspace hooks are disabled by default until enabled here or in config. Hooks managed by plugins show `plugin:<id>` in `openclaw hooks list` and can’t be enabled/disabled here. Enable/disable the plugin instead.**Arguments:**

- `<name>`: Hook name (e.g., `session-memory`)

**Example:**

```
openclaw hooks enable session-memory
```

**Output:**

```
✓ Enabled hook: 💾 session-memory
```

**What it does:**

- Checks if hook exists and is eligible
- Updates `hooks.internal.entries.<name>.enabled = true` in your config
- Saves config to disk

If the hook came from `<workspace>/hooks/`, this opt-in step is required before
the Gateway will load it.**After enabling:**

- Restart the gateway so hooks reload (menu bar app restart on macOS, or restart your gateway process in dev).

## Disable a Hook

```
openclaw hooks disable <name>
```

Disable a specific hook by updating your config.**Arguments:**

- `<name>`: Hook name (e.g., `command-logger`)

**Example:**

```
openclaw hooks disable command-logger
```

**Output:**

```
⏸ Disabled hook: 📝 command-logger
```

**After disabling:**

- Restart the gateway so hooks reload

## Notes

- `openclaw hooks list --json`, `info --json`, and `check --json` write structured JSON directly to stdout.
- Plugin-managed hooks cannot be enabled or disabled here; enable or disable the owning plugin instead.

## Install hook packs

```
openclaw plugins install <package>        # ClawHub first, then npm
openclaw plugins install npm:<package>    # npm only
openclaw plugins install <package> --pin  # pin version
openclaw plugins install <path>           # local path
```

Install hook packs through the unified plugins installer.`openclaw hooks install` still works as a compatibility alias, but it prints a
deprecation warning and forwards to `openclaw plugins install`.Npm specs are **registry-only** (package name + optional **exact version** or
**dist-tag**). Git/URL/file specs and semver ranges are rejected. Dependency
installs run project-local with `--ignore-scripts` for safety, even when your
shell has global npm install settings.Bare specs and `@latest` stay on the stable track. If npm resolves either of
those to a prerelease, OpenClaw stops and asks you to opt in explicitly with a
prerelease tag such as `@beta`/`@rc` or an exact prerelease version.**What it does:**

- Copies the hook pack into `~/.openclaw/hooks/<id>`
- Enables the installed hooks in `hooks.internal.entries.*`
- Records the install under `hooks.internal.installs`

**Options:**

- `-l, --link`: Link a local directory instead of copying (adds it to `hooks.internal.load.extraDirs`)
- `--pin`: Record npm installs as exact resolved `name@version` in `hooks.internal.installs`

**Supported archives:**`.zip`, `.tgz`, `.tar.gz`, `.tar`**Examples:**

```
# Local directory
openclaw plugins install ./my-hook-pack

# Local archive
openclaw plugins install ./my-hook-pack.zip

# NPM package
openclaw plugins install @openclaw/my-hook-pack

# Link a local directory without copying
openclaw plugins install -l ./my-hook-pack
```

Linked hook packs are treated as managed hooks from an operator-configured
directory, not as workspace hooks.

## Update hook packs

```
openclaw plugins update <id>
openclaw plugins update --all
```

Update tracked npm-based hook packs through the unified plugins updater.`openclaw hooks update` still works as a compatibility alias, but it prints a
deprecation warning and forwards to `openclaw plugins update`.**Options:**

- `--all`: Update all tracked hook packs
- `--dry-run`: Show what would change without writing

When a stored integrity hash exists and the fetched artifact hash changes,
OpenClaw prints a warning and asks for confirmation before proceeding. Use
global `--yes` to bypass prompts in CI/non-interactive runs.

## Bundled hooks

### session-memory

Saves session context to memory when you issue `/new` or `/reset`.**Enable:**

```
openclaw hooks enable session-memory
```

**Output:**`~/.openclaw/workspace/memory/YYYY-MM-DD-slug.md`**See:** [session-memory documentation](https://docs.openclaw.ai/automation/hooks#session-memory)

### bootstrap-extra-files

Injects additional bootstrap files (for example monorepo-local `AGENTS.md` / `TOOLS.md`) during `agent:bootstrap`.**Enable:**

```
openclaw hooks enable bootstrap-extra-files
```

**See:** [bootstrap-extra-files documentation](https://docs.openclaw.ai/automation/hooks#bootstrap-extra-files)

### command-logger

Logs all command events to a centralized audit file.**Enable:**

```
openclaw hooks enable command-logger
```

**Output:**`~/.openclaw/logs/commands.log`**View logs:**

```
# Recent commands
tail -n 20 ~/.openclaw/logs/commands.log

# Pretty-print
cat ~/.openclaw/logs/commands.log | jq .

# Filter by action
grep '"action":"new"' ~/.openclaw/logs/commands.log | jq .
```

**See:** [command-logger documentation](https://docs.openclaw.ai/automation/hooks#command-logger)

### boot-md

Runs `BOOT.md` when the gateway starts (after channels start).**Events**: `gateway:startup`**Enable**:

```
openclaw hooks enable boot-md
```

**See:** [boot-md documentation](https://docs.openclaw.ai/automation/hooks#boot-md)

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Automation hooks](https://docs.openclaw.ai/automation/hooks)

[Agents](https://docs.openclaw.ai/cli/agents) [Inference CLI](https://docs.openclaw.ai/cli/infer)

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
- read recent transcript history
- wait for new inbound events
- send a reply back through the same route
- see approval requests that arrive while the bridge is connected

### Usage

- Local Gateway

- Remote Gateway (token)

- Remote Gateway (password)

- Verbose / Claude off

```
openclaw mcp serve
```

```
openclaw mcp serve --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token
```

```
openclaw mcp serve --url wss://gateway-host:18789 --password-file ~/.openclaw/gateway.password
```

```
openclaw mcp serve --verbose
openclaw mcp serve --claude-channel-mode off
```

### Bridge tools

The current bridge exposes these MCP tools:

conversations\_list

Lists recent session-backed conversations that already have route metadata in Gateway session state.Useful filters:

- `limit`
- `search`
- `channel`
- `includeDerivedTitles`
- `includeLastMessage`

conversation\_get

Returns one conversation by `session_key`.

messages\_read

Reads recent transcript messages for one session-backed conversation.

attachments\_fetch

Extracts non-text message content blocks from one transcript message. This is a metadata view over transcript content, not a standalone durable attachment blob store.

events\_poll

Reads queued live events since a numeric cursor.

events\_wait

Long-polls until the next matching queued event arrives or a timeout expires.Use this when a generic MCP client needs near-real-time delivery without a Claude-specific push protocol.

messages\_send

Sends text back through the same route already recorded on the session.Current behavior:

- requires an existing conversation route
- uses the session’s channel, recipient, account id, and thread id
- sends text only

permissions\_list\_open

Lists pending exec/plugin approval requests the bridge has observed since it connected to the Gateway.

permissions\_respond

Resolves one pending exec/plugin approval request with:

- `allow-once`
- `allow-always`
- `deny`

### Event model

The bridge keeps an in-memory event queue while it is connected.Current event types:

- `message`
- `exec_approval_requested`
- `exec_approval_resolved`
- `plugin_approval_requested`
- `plugin_approval_resolved`
- `claude_permission_request`

- the queue is live-only; it starts when the MCP bridge starts
- `events_poll` and `events_wait` do not replay older Gateway history by themselves
- durable backlog should be read with `messages_read`

### Claude channel notifications

The bridge can also expose Claude-specific channel notifications. This is the OpenClaw equivalent of a Claude Code channel adapter: standard MCP tools remain available, but live inbound messages can also arrive as Claude-specific MCP notifications.

- off

- on

- auto (default)

`--claude-channel-mode off`: standard MCP tools only.

`--claude-channel-mode on`: enable Claude channel notifications.

`--claude-channel-mode auto`: current default; same bridge behavior as `on`.

When Claude channel mode is enabled, the server advertises Claude experimental capabilities and can emit:

- `notifications/claude/channel`
- `notifications/claude/channel/permission`

Current bridge behavior:

- inbound `user` transcript messages are forwarded as `notifications/claude/channel`
- Claude permission requests received over MCP are tracked in-memory
- if the linked conversation later sends `yes abcde` or `no abcde`, the bridge converts that to `notifications/claude/channel/permission`
- these notifications are live-session only; if the MCP client disconnects, there is no push target

This is intentionally client-specific. Generic MCP clients should rely on the standard polling tools.

### MCP client config

Example stdio client config:

```
{
  "mcpServers": {
    "openclaw": {
      "command": "openclaw",
      "args": [\
        "mcp",\
        "serve",\
        "--url",\
        "wss://gateway-host:18789",\
        "--token-file",\
        "/path/to/gateway.token"\
      ]
    }
  }
}
```

For most generic MCP clients, start with the standard tool surface and ignore Claude mode. Turn Claude mode on only for clients that actually understand the Claude-specific notification methods.

### Options

`openclaw mcp serve` supports:

[​](https://docs.openclaw.ai/cli/mcp#param-url)

--url

string

Gateway WebSocket URL.

[​](https://docs.openclaw.ai/cli/mcp#param-token)

--token

string

Gateway token.

[​](https://docs.openclaw.ai/cli/mcp#param-token-file)

--token-file

string

Read token from file.

[​](https://docs.openclaw.ai/cli/mcp#param-password)

--password

string

Gateway password.

[​](https://docs.openclaw.ai/cli/mcp#param-password-file)

--password-file

string

Read password from file.

[​](https://docs.openclaw.ai/cli/mcp#param-claude-channel-mode)

--claude-channel-mode

"auto" \| "on" \| "off"

Claude notification mode.

[​](https://docs.openclaw.ai/cli/mcp#param-v-verbose)

-v, --verbose

boolean

Verbose logs on stderr.

Prefer `--token-file` or `--password-file` over inline secrets when possible.

### Security and trust boundary

The bridge does not invent routing. It only exposes conversations that Gateway already knows how to route.That means:

- sender allowlists, pairing, and channel-level trust still belong to the underlying OpenClaw channel configuration
- `messages_send` can only reply through an existing stored route
- approval state is live/in-memory only for the current bridge session
- bridge auth should use the same Gateway token or password controls you would trust for any other remote Gateway client

If a conversation is missing from `conversations_list`, the usual cause is not MCP configuration. It is missing or incomplete route metadata in the underlying Gateway session.

### Testing

OpenClaw ships a deterministic Docker smoke for this bridge:

```
pnpm test:docker:mcp-channels
```

That smoke:

- starts a seeded Gateway container
- starts a second container that spawns `openclaw mcp serve`
- verifies conversation discovery, transcript reads, attachment metadata reads, live event queue behavior, and outbound send routing
- validates Claude-style channel and permission notifications over the real stdio MCP bridge

This is the fastest way to prove the bridge works without wiring a real Telegram, Discord, or iMessage account into the test run.For broader testing context, see [Testing](https://docs.openclaw.ai/help/testing).

### Troubleshooting

No conversations returned

Usually means the Gateway session is not already routable. Confirm that the underlying session has stored channel/provider, recipient, and optional account/thread route metadata.

events\_poll or events\_wait misses older messages

Expected. The live queue starts when the bridge connects. Read older transcript history with `messages_read`.

Claude notifications do not show up

Check all of these:

- the client kept the stdio MCP session open
- `--claude-channel-mode` is `on` or `auto`
- the client actually understands the Claude-specific notification methods
- the inbound message happened after the bridge connected

Approvals are missing

`permissions_list_open` only shows approval requests observed while the bridge was connected. It is not a durable approval history API.

## OpenClaw as an MCP client registry

This is the `openclaw mcp list`, `show`, `set`, and `unset` path.These commands do not expose OpenClaw over MCP. They manage OpenClaw-owned MCP server definitions under `mcp.servers` in OpenClaw config.Those saved definitions are for runtimes that OpenClaw launches or configures later, such as embedded Pi and other runtime adapters. OpenClaw stores the definitions centrally so those runtimes do not need to keep their own duplicate MCP server lists.

Important behavior

- these commands only read or write OpenClaw config
- they do not connect to the target MCP server
- they do not validate whether the command, URL, or remote transport is reachable right now
- runtime adapters decide which transport shapes they actually support at execution time
- embedded Pi exposes configured MCP tools in normal `coding` and `messaging` tool profiles; `minimal` still hides them, and `tools.deny: ["bundle-mcp"]` disables them explicitly
- session-scoped bundled MCP runtimes are reaped after `mcp.sessionIdleTtlMs` milliseconds of idle time (default 10 minutes; set `0` to disable) and one-shot embedded runs clean them up at run end

Runtime adapters may normalize this shared registry into the shape their downstream client expects. For example, embedded Pi consumes OpenClaw `transport` values directly, while Claude Code and Gemini receive CLI-native `type` values such as `http`, `sse`, or `stdio`.

### Saved MCP server definitions

OpenClaw also stores a lightweight MCP server registry in config for surfaces that want OpenClaw-managed MCP definitions.Commands:

- `openclaw mcp list`
- `openclaw mcp show [name]`
- `openclaw mcp set <name> <json>`
- `openclaw mcp unset <name>`

Notes:

- `list` sorts server names.
- `show` without a name prints the full configured MCP server object.
- `set` expects one JSON object value on the command line.
- Use `transport: "streamable-http"` for Streamable HTTP MCP servers. `openclaw mcp set` also normalizes CLI-native `type: "http"` to the same canonical config shape for compatibility.
- `unset` fails if the named server does not exist.

Examples:

```
openclaw mcp list
openclaw mcp show context7 --json
openclaw mcp set context7 '{"command":"uvx","args":["context7-mcp"]}'
openclaw mcp set docs '{"url":"https://mcp.example.com","transport":"streamable-http"}'
openclaw mcp unset context7
```

Example config shape:

```
{
  "mcp": {
    "servers": {
      "context7": {
        "command": "uvx",
        "args": ["context7-mcp"]
      },
      "docs": {
        "url": "https://mcp.example.com",
        "transport": "streamable-http"
      }
    }
  }
}
```

### Stdio transport

Launches a local child process and communicates over stdin/stdout.

| Field | Description |
| --- | --- |
| `command` | Executable to spawn (required) |
| `args` | Array of command-line arguments |
| `env` | Extra environment variables |
| `cwd` / `workingDirectory` | Working directory for the process |

**Stdio env safety filter**OpenClaw rejects interpreter-startup env keys that can alter how a stdio MCP server starts up before the first RPC, even if they appear in a server’s `env` block. Blocked keys include `NODE_OPTIONS`, `PYTHONSTARTUP`, `PYTHONPATH`, `PERL5OPT`, `RUBYOPT`, `SHELLOPTS`, `PS4`, and similar runtime-control variables. Startup rejects these with a configuration error so they cannot inject an implicit prelude, swap the interpreter, or enable a debugger against the stdio process. Ordinary credential, proxy, and server-specific env vars (`GITHUB_TOKEN`, `HTTP_PROXY`, custom `*_API_KEY`, etc.) are unaffected.If your MCP server genuinely needs one of the blocked variables, set it on the gateway host process instead of under the stdio server’s `env`.

### SSE / HTTP transport

Connects to a remote MCP server over HTTP Server-Sent Events.

| Field | Description |
| --- | --- |
| `url` | HTTP or HTTPS URL of the remote server (required) |
| `headers` | Optional key-value map of HTTP headers (for example auth tokens) |
| `connectionTimeoutMs` | Per-server connection timeout in ms (optional) |

Example:

```
{
  "mcp": {
    "servers": {
      "remote-tools": {
        "url": "https://mcp.example.com",
        "headers": {
          "Authorization": "Bearer <token>"
        }
      }
    }
  }
}
```

Sensitive values in `url` (userinfo) and `headers` are redacted in logs and status output.

### Streamable HTTP transport

`streamable-http` is an additional transport option alongside `sse` and `stdio`. It uses HTTP streaming for bidirectional communication with remote MCP servers.

| Field | Description |
| --- | --- |
| `url` | HTTP or HTTPS URL of the remote server (required) |
| `transport` | Set to `"streamable-http"` to select this transport; when omitted, OpenClaw uses `sse` |
| `headers` | Optional key-value map of HTTP headers (for example auth tokens) |
| `connectionTimeoutMs` | Per-server connection timeout in ms (optional) |

OpenClaw config uses `transport: "streamable-http"` as the canonical spelling. CLI-native MCP `type: "http"` values are accepted when saved through `openclaw mcp set` and repaired by `openclaw doctor --fix` in existing config, but `transport` is what embedded Pi consumes directly.Example:

```
{
  "mcp": {
    "servers": {
      "streaming-tools": {
        "url": "https://mcp.example.com/stream",
        "transport": "streamable-http",
        "connectionTimeoutMs": 10000,
        "headers": {
          "Authorization": "Bearer <token>"
        }
      }
    }
  }
}
```

These commands manage saved config only. They do not start the channel bridge, open a live MCP client session, or prove the target server is reachable.

## Current limits

This page documents the bridge as shipped today.Current limits:

- conversation discovery depends on existing Gateway session route metadata
- no generic push protocol beyond the Claude-specific adapter
- no message edit or react tools yet
- HTTP/SSE/streamable-http transport connects to a single remote server; no multiplexed upstream yet
- `permissions_list_open` only includes approvals observed while the bridge is connected

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Plugins](https://docs.openclaw.ai/cli/plugins)

[Docs](https://docs.openclaw.ai/cli/docs) [Proxy](https://docs.openclaw.ai/cli/proxy)

Ctrl+I

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

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw mcp serve --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw mcp serve --url wss://gateway-host:18789 --password-file ~/.openclaw/gateway.password
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw mcp serve --verbose
 openclaw mcp serve --claude-channel-mode off
 \`\`\`

\### Bridge tools

The current bridge exposes these MCP tools:

 Lists recent session-backed conversations that already have route metadata in Gateway session state.

 Useful filters:

 \\* \`limit\`
 \\* \`search\`
 \\* \`channel\`
 \\* \`includeDerivedTitles\`
 \\* \`includeLastMessage\`

 Returns one conversation by \`session\_key\` using a direct Gateway session lookup.

 Reads recent transcript messages for one session-backed conversation.

 Extracts non-text message content blocks from one transcript message. This is a metadata view over transcript content, not a standalone durable attachment blob store.

 Reads queued live events since a numeric cursor.

 Long-polls until the next matching queued event arrives or a timeout expires.

 Use this when a generic MCP client needs near-real-time delivery without a Claude-specific push protocol.

 Sends text back through the same route already recorded on the session.

 Current behavior:

 \\* requires an existing conversation route
 \\* uses the session's channel, recipient, account id, and thread id
 \\* sends text only

 Lists pending exec/plugin approval requests the bridge has observed since it connected to the Gateway.

 Resolves one pending exec/plugin approval request with:

 \\* \`allow-once\`
 \\* \`allow-always\`
 \\* \`deny\`

\### Event model

The bridge keeps an in-memory event queue while it is connected.

Current event types:

\\* \`message\`
\\* \`exec\_approval\_requested\`
\\* \`exec\_approval\_resolved\`
\\* \`plugin\_approval\_requested\`
\\* \`plugin\_approval\_resolved\`
\\* \`claude\_permission\_request\`

 \- the queue is live-only; it starts when the MCP bridge starts
 \- \`events\_poll\` and \`events\_wait\` do not replay older Gateway history by themselves
 \- durable backlog should be read with \`messages\_read\`

\### Claude channel notifications

The bridge can also expose Claude-specific channel notifications. This is the OpenClaw equivalent of a Claude Code channel adapter: standard MCP tools remain available, but live inbound messages can also arrive as Claude-specific MCP notifications.

 \`--claude-channel-mode off\`: standard MCP tools only.

 \`--claude-channel-mode on\`: enable Claude channel notifications.

 \`--claude-channel-mode auto\`: current default; same bridge behavior as \`on\`.

When Claude channel mode is enabled, the server advertises Claude experimental capabilities and can emit:

\\* \`notifications/claude/channel\`
\\* \`notifications/claude/channel/permission\`

Current bridge behavior:

\\* inbound \`user\` transcript messages are forwarded as \`notifications/claude/channel\`
\\* Claude permission requests received over MCP are tracked in-memory
\\* if the linked conversation later sends \`yes abcde\` or \`no abcde\`, the bridge converts that to \`notifications/claude/channel/permission\`
\\* these notifications are live-session only; if the MCP client disconnects, there is no push target

This is intentionally client-specific. Generic MCP clients should rely on the standard polling tools.

\### MCP client config

Example stdio client config:

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "mcpServers": {
 "openclaw": {
 "command": "openclaw",
 "args": \[\
 "mcp",\
 "serve",\
 "--url",\
 "wss://gateway-host:18789",\
 "--token-file",\
 "/path/to/gateway.token"\
 \]
 }
 }
}
\`\`\`

For most generic MCP clients, start with the standard tool surface and ignore Claude mode. Turn Claude mode on only for clients that actually understand the Claude-specific notification methods.

\### Options

\`openclaw mcp serve\` supports:

 Gateway WebSocket URL.

 Gateway token.

 Read token from file.

 Gateway password.

 Read password from file.

 Claude notification mode.

 Verbose logs on stderr.

 Prefer \`--token-file\` or \`--password-file\` over inline secrets when possible.

\### Security and trust boundary

The bridge does not invent routing. It only exposes conversations that Gateway already knows how to route.

That means:

\\* sender allowlists, pairing, and channel-level trust still belong to the underlying OpenClaw channel configuration
\\* \`messages\_send\` can only reply through an existing stored route
\\* approval state is live/in-memory only for the current bridge session
\\* bridge auth should use the same Gateway token or password controls you would trust for any other remote Gateway client

If a conversation is missing from \`conversations\_list\`, the usual cause is not MCP configuration. It is missing or incomplete route metadata in the underlying Gateway session.

\### Testing

OpenClaw ships a deterministic Docker smoke for this bridge:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
pnpm test:docker:mcp-channels
\`\`\`

That smoke:

\\* starts a seeded Gateway container
\\* starts a second container that spawns \`openclaw mcp serve\`
\\* verifies conversation discovery, transcript reads, attachment metadata reads, live event queue behavior, and outbound send routing
\\* validates Claude-style channel and permission notifications over the real stdio MCP bridge

This is the fastest way to prove the bridge works without wiring a real Telegram, Discord, or iMessage account into the test run.

For broader testing context, see \[Testing\](/help/testing).

\### Troubleshooting

 Usually means the Gateway session is not already routable. Confirm that the underlying session has stored channel/provider, recipient, and optional account/thread route metadata.

 Expected. The live queue starts when the bridge connects. Read older transcript history with \`messages\_read\`.

 Check all of these:

 \\* the client kept the stdio MCP session open
 \\* \`--claude-channel-mode\` is \`on\` or \`auto\`
 \\* the client actually understands the Claude-specific notification methods
 \\* the inbound message happened after the bridge connected

 \`permissions\_list\_open\` only shows approval requests observed while the bridge was connected. It is not a durable approval history API.

\## OpenClaw as an MCP client registry

This is the \`openclaw mcp list\`, \`show\`, \`set\`, and \`unset\` path.

These commands do not expose OpenClaw over MCP. They manage OpenClaw-owned MCP server definitions under \`mcp.servers\` in OpenClaw config.

Those saved definitions are for runtimes that OpenClaw launches or configures later, such as embedded Pi and other runtime adapters. OpenClaw stores the definitions centrally so those runtimes do not need to keep their own duplicate MCP server lists.

 \\* these commands only read or write OpenClaw config
 \\* they do not connect to the target MCP server
 \\* they do not validate whether the command, URL, or remote transport is reachable right now
 \\* runtime adapters decide which transport shapes they actually support at execution time
 \\* embedded Pi exposes configured MCP tools in normal \`coding\` and \`messaging\` tool profiles; \`minimal\` still hides them, and \`tools.deny: \["bundle-mcp"\]\` disables them explicitly
 \\* session-scoped bundled MCP runtimes are reaped after \`mcp.sessionIdleTtlMs\` milliseconds of idle time (default 10 minutes; set \`0\` to disable) and one-shot embedded runs clean them up at run end

Runtime adapters may normalize this shared registry into the shape their downstream client expects. For example, embedded Pi consumes OpenClaw \`transport\` values directly, while Claude Code and Gemini receive CLI-native \`type\` values such as \`http\`, \`sse\`, or \`stdio\`.

\### Saved MCP server definitions

OpenClaw also stores a lightweight MCP server registry in config for surfaces that want OpenClaw-managed MCP definitions.

Commands:

\\* \`openclaw mcp list\`
\\* \`openclaw mcp show \[name\]\`
\\* \`openclaw mcp set \`
\\* \`openclaw mcp unset \`

Notes:

\\* \`list\` sorts server names.
\\* \`show\` without a name prints the full configured MCP server object.
\\* \`set\` expects one JSON object value on the command line.
\\* Use \`transport: "streamable-http"\` for Streamable HTTP MCP servers. \`openclaw mcp set\` also normalizes CLI-native \`type: "http"\` to the same canonical config shape for compatibility.
\\* \`unset\` fails if the named server does not exist.

Examples:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw mcp list
openclaw mcp show context7 --json
openclaw mcp set context7 '{"command":"uvx","args":\["context7-mcp"\]}'
openclaw mcp set docs '{"url":"https://mcp.example.com","transport":"streamable-http"}'
openclaw mcp unset context7
\`\`\`

Example config shape:

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "mcp": {
 "servers": {
 "context7": {
 "command": "uvx",
 "args": \["context7-mcp"\]
 },
 "docs": {
 "url": "https://mcp.example.com",
 "transport": "streamable-http"
 }
 }
 }
}
\`\`\`

\### Stdio transport

Launches a local child process and communicates over stdin/stdout.

\| Field \| Description \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`command\` \| Executable to spawn (required) \|
\| \`args\` \| Array of command-line arguments \|
\| \`env\` \| Extra environment variables \|
\| \`cwd\` / \`workingDirectory\` \| Working directory for the process \|

 \*\*Stdio env safety filter\*\*

 OpenClaw rejects interpreter-startup env keys that can alter how a stdio MCP server starts up before the first RPC, even if they appear in a server's \`env\` block. Blocked keys include \`NODE\_OPTIONS\`, \`PYTHONSTARTUP\`, \`PYTHONPATH\`, \`PERL5OPT\`, \`RUBYOPT\`, \`SHELLOPTS\`, \`PS4\`, and similar runtime-control variables. Startup rejects these with a configuration error so they cannot inject an implicit prelude, swap the interpreter, or enable a debugger against the stdio process. Ordinary credential, proxy, and server-specific env vars (\`GITHUB\_TOKEN\`, \`HTTP\_PROXY\`, custom \`\*\_API\_KEY\`, etc.) are unaffected.

 If your MCP server genuinely needs one of the blocked variables, set it on the gateway host process instead of under the stdio server's \`env\`.

\### SSE / HTTP transport

Connects to a remote MCP server over HTTP Server-Sent Events.

\| Field \| Description \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`url\` \| HTTP or HTTPS URL of the remote server (required) \|
\| \`headers\` \| Optional key-value map of HTTP headers (for example auth tokens) \|
\| \`connectionTimeoutMs\` \| Per-server connection timeout in ms (optional) \|

Example:

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "mcp": {
 "servers": {
 "remote-tools": {
 "url": "https://mcp.example.com",
 "headers": {
 "Authorization": "Bearer "
 }
 }
 }
 }
}
\`\`\`

Sensitive values in \`url\` (userinfo) and \`headers\` are redacted in logs and status output.

\### Streamable HTTP transport

\`streamable-http\` is an additional transport option alongside \`sse\` and \`stdio\`. It uses HTTP streaming for bidirectional communication with remote MCP servers.

\| Field \| Description \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`url\` \| HTTP or HTTPS URL of the remote server (required) \|
\| \`transport\` \| Set to \`"streamable-http"\` to select this transport; when omitted, OpenClaw uses \`sse\` \|
\| \`headers\` \| Optional key-value map of HTTP headers (for example auth tokens) \|
\| \`connectionTimeoutMs\` \| Per-server connection timeout in ms (optional) \|

OpenClaw config uses \`transport: "streamable-http"\` as the canonical spelling. CLI-native MCP \`type: "http"\` values are accepted when saved through \`openclaw mcp set\` and repaired by \`openclaw doctor --fix\` in existing config, but \`transport\` is what embedded Pi consumes directly.

Example:

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "mcp": {
 "servers": {
 "streaming-tools": {
 "url": "https://mcp.example.com/stream",
 "transport": "streamable-http",
 "connectionTimeoutMs": 10000,
 "headers": {
 "Authorization": "Bearer "
 }
 }
 }
 }
}
\`\`\`

 These commands manage saved config only. They do not start the channel bridge, open a live MCP client session, or prove the target server is reachable.

\## Current limits

This page documents the bridge as shipped today.

Current limits:

\\* conversation discovery depends on existing Gateway session route metadata
\\* no generic push protocol beyond the Claude-specific adapter
\\* no message edit or react tools yet
\\* HTTP/SSE/streamable-http transport connects to a single remote server; no multiplexed upstream yet
\\* \`permissions\_list\_open\` only includes approvals observed while the bridge is connected

\## Related

\\* \[CLI reference\](/cli)
\\* \[Plugins\](/cli/plugins)

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
- `--json`: print JSON output.

`memory promote-explain`:Explain a specific promotion candidate and its score breakdown.

```
openclaw memory promote-explain <selector> [--agent <id>] [--include-promoted] [--json]
```

- `<selector>`: candidate key, path fragment, or snippet fragment to look up.
- `--agent <id>`: scope to a single agent (default: the default agent).
- `--include-promoted`: include already promoted candidates.
- `--json`: print JSON output.

`memory rem-harness`:Preview REM reflections, candidate truths, and deep promotion output without writing anything.

```
openclaw memory rem-harness [--agent <id>] [--include-promoted] [--json]
```

- `--agent <id>`: scope to a single agent (default: the default agent).
- `--include-promoted`: include already promoted deep candidates.
- `--json`: print JSON output.

## Dreaming

Dreaming is the background memory consolidation system with three cooperative
phases: **light** (sort/stage short-term material), **deep** (promote durable
facts into `MEMORY.md`), and **REM** (reflect and surface themes).

- Enable with `plugins.entries.memory-core.config.dreaming.enabled: true`.
- Toggle from chat with `/dreaming on|off` (or inspect with `/dreaming status`).
- Dreaming runs on one managed sweep schedule (`dreaming.frequency`) and executes phases in order: light, REM, deep.
- Only the deep phase writes durable memory to `MEMORY.md`.
- Human-readable phase output and diary entries are written to `DREAMS.md` (or existing `dreams.md`), with optional per-phase reports in `memory/dreaming/<phase>/YYYY-MM-DD.md`.
- Ranking uses weighted signals: recall frequency, retrieval relevance, query diversity, temporal recency, cross-day consolidation, and derived concept richness.
- Promotion re-reads the live daily note before writing to `MEMORY.md`, so edited or deleted short-term snippets do not get promoted from stale recall-store snapshots.
- Scheduled and manual `memory promote` runs share the same deep phase defaults unless you pass CLI threshold overrides.
- Automatic runs fan out across configured memory workspaces.

Default scheduling:

- **Sweep cadence**: `dreaming.frequency = 0 3 * * *`
- **Deep thresholds**: `minScore=0.8`, `minRecallCount=3`, `minUniqueQueries=3`, `recencyHalfLifeDays=14`, `maxAgeDays=30`

Example:

```
{
  "plugins": {
    "entries": {
      "memory-core": {
        "config": {
          "dreaming": {
            "enabled": true
          }
        }
      }
    }
  }
}
```

Notes:

- `memory index --verbose` prints per-phase details (provider, model, sources, batch activity).
- `memory status` includes any extra paths configured via `memorySearch.extraPaths`.
- If effectively active memory remote API key fields are configured as SecretRefs, the command resolves those values from the active gateway snapshot. If gateway is unavailable, the command fails fast.
- Gateway version skew note: this command path requires a gateway that supports `secrets.resolve`; older gateways return an unknown-method error.
- Tune scheduled sweep cadence with `dreaming.frequency`. Deep promotion policy is otherwise internal; use CLI flags on `memory promote` when you need one-off manual overrides.
- `memory rem-harness --path <file-or-dir> --grounded` previews grounded `What Happened`, `Reflections`, and `Possible Lasting Updates` from historical daily notes without writing anything.
- `memory rem-backfill --path <file-or-dir>` writes reversible grounded diary entries into `DREAMS.md` for UI review.
- `memory rem-backfill --path <file-or-dir> --stage-short-term` also seeds grounded durable candidates into the live short-term promotion store so the normal deep phase can rank them.
- `memory rem-backfill --rollback` removes previously written grounded diary entries, and `memory rem-backfill --rollback-short-term` removes previously staged grounded short-term candidates.
- See [Dreaming](https://docs.openclaw.ai/concepts/dreaming) for full phase descriptions and configuration reference.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Memory overview](https://docs.openclaw.ai/concepts/memory)

[Inference CLI](https://docs.openclaw.ai/cli/infer) [\`openclaw commitments\`](https://docs.openclaw.ai/cli/commitments)

Ctrl+I

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
  - Required: `--target`, `--poll-question`, `--poll-option` (repeat)
  - Optional: `--poll-multi`
  - Discord only: `--poll-duration-hours`, `--silent`, `--message`
  - Telegram only: `--poll-duration-seconds` (5-600), `--silent`, `--poll-anonymous` / `--poll-public`, `--thread-id`
- `react`  - Channels: Discord/Google Chat/Slack/Telegram/WhatsApp/Signal/Matrix
  - Required: `--message-id`, `--target`
  - Optional: `--emoji`, `--remove`, `--participant`, `--from-me`, `--target-author`, `--target-author-uuid`
  - Note: `--remove` requires `--emoji` (omit `--emoji` to clear own reactions where supported; see /tools/reactions)
  - WhatsApp only: `--participant`, `--from-me`
  - Signal group reactions: `--target-author` or `--target-author-uuid` required
- `reactions`  - Channels: Discord/Google Chat/Slack/Matrix
  - Required: `--message-id`, `--target`
  - Optional: `--limit`
- `read`  - Channels: Discord/Slack/Matrix
  - Required: `--target`
  - Optional: `--limit`, `--message-id`, `--before`, `--after`
  - Slack only: `--message-id` reads a specific Slack message timestamp; combine with `--thread-id` to read an exact thread reply.
  - Discord only: `--around`
- `edit`  - Channels: Discord/Slack/Matrix
  - Required: `--message-id`, `--message`, `--target`
- `delete`  - Channels: Discord/Slack/Telegram/Matrix
  - Required: `--message-id`, `--target`
- `pin` / `unpin`  - Channels: Discord/Slack/Matrix
  - Required: `--message-id`, `--target`
- `pins` (list)  - Channels: Discord/Slack/Matrix
  - Required: `--target`
- `permissions`  - Channels: Discord/Matrix
  - Required: `--target`
  - Matrix only: available when Matrix encryption is enabled and verification actions are allowed
- `search`  - Channels: Discord
  - Required: `--guild-id`, `--query`
  - Optional: `--channel-id`, `--channel-ids` (repeat), `--author-id`, `--author-ids` (repeat), `--limit`

### Threads

- `thread create`  - Channels: Discord
  - Required: `--thread-name`, `--target` (channel id)
  - Optional: `--message-id`, `--message`, `--auto-archive-min`
- `thread list`  - Channels: Discord
  - Required: `--guild-id`
  - Optional: `--channel-id`, `--include-archived`, `--before`, `--limit`
- `thread reply`  - Channels: Discord
  - Required: `--target` (thread id), `--message`
  - Optional: `--media`, `--reply-to`

### Emojis

- `emoji list`  - Discord: `--guild-id`
  - Slack: no extra flags
- `emoji upload`  - Channels: Discord
  - Required: `--guild-id`, `--emoji-name`, `--media`
  - Optional: `--role-ids` (repeat)

### Stickers

- `sticker send`  - Channels: Discord
  - Required: `--target`, `--sticker-id` (repeat)
  - Optional: `--message`
- `sticker upload`  - Channels: Discord
  - Required: `--guild-id`, `--sticker-name`, `--sticker-desc`, `--sticker-tags`, `--media`

### Roles / Channels / Members / Voice

- `role info` (Discord): `--guild-id`
- `role add` / `role remove` (Discord): `--guild-id`, `--user-id`, `--role-id`
- `channel info` (Discord): `--target`
- `channel list` (Discord): `--guild-id`
- `member info` (Discord/Slack): `--user-id` (\+ `--guild-id` for Discord)
- `voice status` (Discord): `--guild-id`, `--user-id`

### Events

- `event list` (Discord): `--guild-id`
- `event create` (Discord): `--guild-id`, `--event-name`, `--start-time`
  - Optional: `--end-time`, `--desc`, `--channel-id`, `--location`, `--event-type`

### Moderation (Discord)

- `timeout`: `--guild-id`, `--user-id` (optional `--duration-min` or `--until`; omit both to clear timeout)
- `kick`: `--guild-id`, `--user-id` (\+ `--reason`)
- `ban`: `--guild-id`, `--user-id` (\+ `--delete-days`, `--reason`)

  - `timeout` also supports `--reason`

### Broadcast

- `broadcast`
  - Channels: any configured channel; use `--channel all` to target all providers
  - Required: `--targets <target...>`
  - Optional: `--message`, `--media`, `--dry-run`

## Examples

Send a Discord reply:

```
openclaw message send --channel discord \
  --target channel:123 --message "hi" --reply-to 456
```

Send a message with semantic buttons:

```
openclaw message send --channel discord \
  --target channel:123 --message "Choose:" \
  --presentation '{"blocks":[{"type":"buttons","buttons":[{"label":"Approve","value":"approve","style":"success"},{"label":"Decline","value":"decline","style":"danger"}]}]}'
```

Core renders the same `presentation` payload into Discord components, Slack blocks, Telegram inline buttons, Mattermost props, or Teams/Feishu cards depending on channel capability. See [Message Presentation](https://docs.openclaw.ai/plugins/message-presentation) for the full contract and fallback rules.Send a richer presentation payload:

```
openclaw message send --channel googlechat --target spaces/AAA... \
  --message "Choose:" \
  --presentation '{"title":"Deploy approval","tone":"warning","blocks":[{"type":"text","text":"Choose a path"},{"type":"buttons","buttons":[{"label":"Approve","value":"approve"},{"label":"Decline","value":"decline"}]}]}'
```

Create a Discord poll:

```
openclaw message poll --channel discord \
  --target channel:123 \
  --poll-question "Snack?" \
  --poll-option Pizza --poll-option Sushi \
  --poll-multi --poll-duration-hours 48
```

Create a Telegram poll (auto-close in 2 minutes):

```
openclaw message poll --channel telegram \
  --target @mychat \
  --poll-question "Lunch?" \
  --poll-option Pizza --poll-option Sushi \
  --poll-duration-seconds 120 --silent
```

Send a Teams proactive message:

```
openclaw message send --channel msteams \
  --target conversation:19:abc@thread.tacv2 --message "hi"
```

Create a Teams poll:

```
openclaw message poll --channel msteams \
  --target conversation:19:abc@thread.tacv2 \
  --poll-question "Lunch?" \
  --poll-option Pizza --poll-option Sushi
```

React in Slack:

```
openclaw message react --channel slack \
  --target C123 --message-id 456 --emoji "✅"
```

React in a Signal group:

```
openclaw message react --channel signal \
  --target signal:group:abc123 --message-id 1737630212345 \
  --emoji "✅" --target-author-uuid 123e4567-e89b-12d3-a456-426614174000
```

Send Telegram inline buttons through generic presentation:

```
openclaw message send --channel telegram --target @mychat --message "Choose:" \
  --presentation '{"blocks":[{"type":"buttons","buttons":[{"label":"Yes","value":"cmd:yes"},{"label":"No","value":"cmd:no"}]}]}'
```

Send a Teams card through generic presentation:

```
openclaw message send --channel msteams \
  --target conversation:19:abc@thread.tacv2 \
  --presentation '{"title":"Status update","blocks":[{"type":"text","text":"Build completed"}]}'
```

Send a Telegram image as a document to avoid compression:

```
openclaw message send --channel telegram --target @mychat \
  --media ./diagram.png --force-document
```

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Agent send](https://docs.openclaw.ai/tools/agent-send)

[\`openclaw commitments\`](https://docs.openclaw.ai/cli/commitments) [Models](https://docs.openclaw.ai/cli/models)

Ctrl+I

---

## Models - OpenClaw

_Source: <https://docs.openclaw.ai/cli/models>_

# `openclaw models`

Model discovery, scanning, and configuration (default model, fallbacks, auth profiles).Related:

- Providers + models: [Models](https://docs.openclaw.ai/providers/models)
- Model selection concepts + `/models` slash command: [Models concept](https://docs.openclaw.ai/concepts/models)
- Provider auth setup: [Getting started](https://docs.openclaw.ai/start/getting-started)

## Common commands

```
openclaw models status
openclaw models list
openclaw models set <model-or-alias>
openclaw models scan
```

`openclaw models status` shows the resolved default/fallbacks plus an auth overview.
When provider usage snapshots are available, the OAuth/API-key status section includes
provider usage windows and quota snapshots.
Current usage-window providers: Anthropic, GitHub Copilot, Gemini CLI, OpenAI
Codex, MiniMax, Xiaomi, and z.ai. Usage auth comes from provider-specific hooks
when available; otherwise OpenClaw falls back to matching OAuth/API-key
credentials from auth profiles, env, or config.
In `--json` output, `auth.providers` is the env/config/store-aware provider
overview, while `auth.oauth` is auth-store profile health only.
Add `--probe` to run live auth probes against each configured provider profile.
Probes are real requests (may consume tokens and trigger rate limits).
Use `--agent <id>` to inspect a configured agent’s model/auth state. When omitted,
the command uses `OPENCLAW_AGENT_DIR`/`PI_CODING_AGENT_DIR` if set, otherwise the
configured default agent.
Probe rows can come from auth profiles, env credentials, or `models.json`.Notes:

- `models set <model-or-alias>` accepts `provider/model` or an alias.
- `models list` is read-only: it reads config, auth profiles, existing catalog
state, and provider-owned catalog rows, but it does not rewrite
`models.json`.
- The `Auth` column is provider-level and read-only. It is computed from local
auth profile metadata, env markers, configured provider keys, local-provider
markers, AWS Bedrock env/profile markers, and plugin synthetic-auth metadata;
it does not load provider runtime, read keychain secrets, call provider
APIs, or prove exact per-model execution readiness.
- `models list --all --provider <id>` can include provider-owned static catalog
rows from plugin manifests or bundled provider catalog metadata even when you
have not authenticated with that provider yet. Those rows still show as
unavailable until matching auth is configured.
- `models list` keeps the control plane responsive while provider catalog
discovery is slow. The default and configured views fall back to configured or
synthetic model rows after a short wait and let discovery finish in the
background. Use `--all` when you need the exact full discovered catalog and
are willing to wait for provider discovery.
- Broad `models list --all` merges manifest catalog rows over registry rows
without loading provider runtime supplement hooks. Provider-filtered manifest
fast paths use only providers marked `static`; providers marked `refreshable`
stay registry/cache-backed and append manifest rows as supplements, while
providers marked `runtime` stay on registry/runtime discovery.
- `models list` keeps native model metadata and runtime caps distinct. In table
output, `Ctx` shows `contextTokens/contextWindow` when an effective runtime
cap differs from the native context window; JSON rows include `contextTokens`
when a provider exposes that cap.
- `models list --provider <id>` filters by provider id, such as `moonshot` or
`openai-codex`. It does not accept display labels from interactive provider
pickers, such as `Moonshot AI`.
- Model refs are parsed by splitting on the **first**`/`. If the model ID includes `/` (OpenRouter-style), include the provider prefix (example: `openrouter/moonshotai/kimi-k2`).
- If you omit the provider, OpenClaw resolves the input as an alias first, then
as a unique configured-provider match for that exact model id, and only then
falls back to the configured default provider with a deprecation warning.
If that provider no longer exposes the configured default model, OpenClaw
falls back to the first configured provider/model instead of surfacing a
stale removed-provider default.
- `models status` may show `marker(<value>)` in auth output for non-secret placeholders (for example `OPENAI_API_KEY`, `secretref-managed`, `minimax-oauth`, `oauth:chutes`, `ollama-local`) instead of masking them as secrets.

### Models scan

`models scan` reads OpenRouter’s public `:free` catalog and ranks candidates for
fallback use. The catalog itself is public, so metadata-only scans do not need
an OpenRouter key.By default OpenClaw tries to probe tool and image support with live model calls.
If no OpenRouter key is configured, the command falls back to metadata-only
output and explains that `:free` models still require `OPENROUTER_API_KEY` for
probes and inference.Options:

- `--no-probe` (metadata only; no config/secrets lookup)
- `--min-params <b>`
- `--max-age-days <days>`
- `--provider <name>`
- `--max-candidates <n>`
- `--timeout <ms>` (catalog request and per-probe timeout)
- `--concurrency <n>`
- `--yes`
- `--no-input`
- `--set-default`
- `--set-image`
- `--json`

`--set-default` and `--set-image` require live probes; metadata-only scan
results are informational and are not applied to config.

### Models status

Options:

- `--json`
- `--plain`
- `--check` (exit 1=expired/missing, 2=expiring)
- `--probe` (live probe of configured auth profiles)
- `--probe-provider <name>` (probe one provider)
- `--probe-profile <id>` (repeat or comma-separated profile ids)
- `--probe-timeout <ms>`
- `--probe-concurrency <n>`
- `--probe-max-tokens <n>`
- `--agent <id>` (configured agent id; overrides `OPENCLAW_AGENT_DIR`/`PI_CODING_AGENT_DIR`)

`--json` keeps stdout reserved for the JSON payload. Auth-profile, provider,
and startup diagnostics are routed to stderr so scripts can pipe stdout directly
into tools such as `jq`.Probe status buckets:

- `ok`
- `auth`
- `rate_limit`
- `billing`
- `timeout`
- `format`
- `unknown`
- `no_model`

Probe detail/reason-code cases to expect:

- `excluded_by_auth_order`: a stored profile exists, but explicit
`auth.order.<provider>` omitted it, so probe reports the exclusion instead of
trying it.
- `missing_credential`, `invalid_expires`, `expired`, `unresolved_ref`:
profile is present but not eligible/resolvable.
- `no_model`: provider auth exists, but OpenClaw could not resolve a probeable
model candidate for that provider.

## Aliases + fallbacks

```
openclaw models aliases list
openclaw models fallbacks list
```

## Auth profiles

```
openclaw models auth add
openclaw models auth login --provider <id>
openclaw models auth setup-token --provider <id>
openclaw models auth paste-token
```

`models auth add` is the interactive auth helper. It can launch a provider auth
flow (OAuth/API key) or guide you into manual token paste, depending on the
provider you choose.`models auth login` runs a provider plugin’s auth flow (OAuth/API key). Use
`openclaw plugins list` to see which providers are installed.
Use `openclaw models auth --agent <id> <subcommand>` to write auth results to a
specific configured agent store. The parent `--agent` flag is honored by
`add`, `login`, `setup-token`, `paste-token`, and `login-github-copilot`.Examples:

```
openclaw models auth login --provider openai-codex --set-default
```

Notes:

- `setup-token` and `paste-token` remain generic token commands for providers
that expose token auth methods.
- `setup-token` requires an interactive TTY and runs the provider’s token-auth
method (defaulting to that provider’s `setup-token` method when it exposes
one).
- `paste-token` accepts a token string generated elsewhere or from automation.
- `paste-token` requires `--provider`, prompts for the token value, and writes
it to the default profile id `<provider>:manual` unless you pass
`--profile-id`.
- `paste-token --expires-in <duration>` stores an absolute token expiry from a
relative duration such as `365d` or `12h`.
- Anthropic note: Anthropic staff told us OpenClaw-style Claude CLI usage is allowed again, so OpenClaw treats Claude CLI reuse and `claude -p` usage as sanctioned for this integration unless Anthropic publishes a new policy.
- Anthropic `setup-token` / `paste-token` remain available as a supported OpenClaw token path, but OpenClaw now prefers Claude CLI reuse and `claude -p` when available.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Model selection](https://docs.openclaw.ai/concepts/model-providers)
- [Model failover](https://docs.openclaw.ai/concepts/model-failover)

[Message](https://docs.openclaw.ai/cli/message) [Sessions](https://docs.openclaw.ai/cli/sessions)

Ctrl+I

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

## Onboard - OpenClaw

_Source: <https://docs.openclaw.ai/cli/onboard>_

# `openclaw onboard`

Interactive onboarding for local or remote Gateway setup.

## Related guides

[**CLI onboarding hub** \\
\\
Walkthrough of the interactive CLI flow.](https://docs.openclaw.ai/start/wizard)

[**Onboarding overview** \\
\\
How OpenClaw onboarding fits together.](https://docs.openclaw.ai/start/onboarding-overview)

[**CLI setup reference** \\
\\
Outputs, internals, and per-step behavior.](https://docs.openclaw.ai/start/wizard-cli-reference)

[**CLI automation** \\
\\
Non-interactive flags and scripted setups.](https://docs.openclaw.ai/start/wizard-cli-automation)

[**macOS app onboarding** \\
\\
Onboarding flow for the macOS menu bar app.](https://docs.openclaw.ai/start/onboarding)

## Examples

```
openclaw onboard
openclaw onboard --modern
openclaw onboard --flow quickstart
openclaw onboard --flow manual
openclaw onboard --flow import
openclaw onboard --import-from hermes --import-source ~/.hermes
openclaw onboard --skip-bootstrap
openclaw onboard --mode remote --remote-url wss://gateway-host:18789
```

`--flow import` uses plugin-owned migration providers such as Hermes. It only runs against a fresh OpenClaw setup; if existing config, credentials, sessions, or workspace memory/identity files are present, reset or choose a fresh setup before importing.`--modern` starts the Crestodian conversational onboarding preview. Without
`--modern`, `openclaw onboard` keeps the classic onboarding flow.For plaintext private-network `ws://` targets (trusted networks only), set
`OPENCLAW_ALLOW_INSECURE_PRIVATE_WS=1` in the onboarding process environment.
There is no `openclaw.json` equivalent for this client-side transport
break-glass.Non-interactive custom provider:

```
openclaw onboard --non-interactive \
  --auth-choice custom-api-key \
  --custom-base-url "https://llm.example.com/v1" \
  --custom-model-id "foo-large" \
  --custom-api-key "$CUSTOM_API_KEY" \
  --secret-input-mode plaintext \
  --custom-compatibility openai \
  --custom-image-input
```

`--custom-api-key` is optional in non-interactive mode. If omitted, onboarding checks `CUSTOM_API_KEY`.
OpenClaw marks common vision model IDs as image-capable automatically. Pass `--custom-image-input` for unknown custom vision IDs, or `--custom-text-input` to force text-only metadata.LM Studio also supports a provider-specific key flag in non-interactive mode:

```
openclaw onboard --non-interactive \
  --auth-choice lmstudio \
  --custom-base-url "http://localhost:1234/v1" \
  --custom-model-id "qwen/qwen3.5-9b" \
  --lmstudio-api-key "$LM_API_TOKEN" \
  --accept-risk
```

Non-interactive Ollama:

```
openclaw onboard --non-interactive \
  --auth-choice ollama \
  --custom-base-url "http://ollama-host:11434" \
  --custom-model-id "qwen3.5:27b" \
  --accept-risk
```

`--custom-base-url` defaults to `http://127.0.0.1:11434`. `--custom-model-id` is optional; if omitted, onboarding uses Ollama’s suggested defaults. Cloud model IDs such as `kimi-k2.5:cloud` also work here.Store provider keys as refs instead of plaintext:

```
openclaw onboard --non-interactive \
  --auth-choice openai-api-key \
  --secret-input-mode ref \
  --accept-risk
```

With `--secret-input-mode ref`, onboarding writes env-backed refs instead of plaintext key values.
For auth-profile backed providers this writes `keyRef` entries; for custom providers this writes `models.providers.<id>.apiKey` as an env ref (for example `{ source: "env", provider: "default", id: "CUSTOM_API_KEY" }`).Non-interactive `ref` mode contract:

- Set the provider env var in the onboarding process environment (for example `OPENAI_API_KEY`).
- Do not pass inline key flags (for example `--openai-api-key`) unless that env var is also set.
- If an inline key flag is passed without the required env var, onboarding fails fast with guidance.

Gateway token options in non-interactive mode:

- `--gateway-auth token --gateway-token <token>` stores a plaintext token.
- `--gateway-auth token --gateway-token-ref-env <name>` stores `gateway.auth.token` as an env SecretRef.
- `--gateway-token` and `--gateway-token-ref-env` are mutually exclusive.
- `--gateway-token-ref-env` requires a non-empty env var in the onboarding process environment.
- With `--install-daemon`, when token auth requires a token, SecretRef-managed gateway tokens are validated but not persisted as resolved plaintext in supervisor service environment metadata.
- With `--install-daemon`, if token mode requires a token and the configured token SecretRef is unresolved, onboarding fails closed with remediation guidance.
- With `--install-daemon`, if both `gateway.auth.token` and `gateway.auth.password` are configured and `gateway.auth.mode` is unset, onboarding blocks install until mode is set explicitly.
- Local onboarding writes `gateway.mode="local"` into the config. If a later config file is missing `gateway.mode`, treat that as config damage or an incomplete manual edit, not as a valid local-mode shortcut.
- Local onboarding installs selected downloadable plugins when the chosen setup path requires them.
- Remote onboarding only writes connection info for the remote Gateway and does not install local plugin packages.
- `--allow-unconfigured` is a separate gateway runtime escape hatch. It does not mean onboarding may omit `gateway.mode`.

Example:

```
export OPENCLAW_GATEWAY_TOKEN="your-token"
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice skip \
  --gateway-auth token \
  --gateway-token-ref-env OPENCLAW_GATEWAY_TOKEN \
  --accept-risk
```

Non-interactive local gateway health:

- Unless you pass `--skip-health`, onboarding waits for a reachable local gateway before it exits successfully.
- `--install-daemon` starts the managed gateway install path first. Without it, you must already have a local gateway running, for example `openclaw gateway run`.
- If you only want config/workspace/bootstrap writes in automation, use `--skip-health`.
- If you manage workspace files yourself, pass `--skip-bootstrap` to set `agents.defaults.skipBootstrap: true` and skip creating `AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`, and `BOOTSTRAP.md`.
- On native Windows, `--install-daemon` tries Scheduled Tasks first and falls back to a per-user Startup-folder login item if task creation is denied.

Interactive onboarding behavior with reference mode:

- Choose **Use secret reference** when prompted.
- Then choose either:
  - Environment variable
  - Configured secret provider (`file` or `exec`)
- Onboarding performs a fast preflight validation before saving the ref.
  - If validation fails, onboarding shows the error and lets you retry.

### Non-interactive Z.AI endpoint choices

`--auth-choice zai-api-key` auto-detects the best Z.AI endpoint for your key (prefers the general API with `zai/glm-5.1`). If you specifically want the GLM Coding Plan endpoints, pick `zai-coding-global` or `zai-coding-cn`.

```
# Promptless endpoint selection
openclaw onboard --non-interactive \
  --auth-choice zai-coding-global \
  --zai-api-key "$ZAI_API_KEY"

# Other Z.AI endpoint choices:
# --auth-choice zai-coding-cn
# --auth-choice zai-global
# --auth-choice zai-cn
```

Non-interactive Mistral example:

```
openclaw onboard --non-interactive \
  --auth-choice mistral-api-key \
  --mistral-api-key "$MISTRAL_API_KEY"
```

## Flow notes

Flow types

- `quickstart`: minimal prompts, auto-generates a gateway token.
- `manual`: full prompts for port, bind, and auth (alias of `advanced`).
- `import`: runs a detected migration provider, previews the plan, then applies after confirmation.

Provider prefiltering

When an auth choice implies a preferred provider, onboarding prefilters the default-model and allowlist pickers to that provider. For Volcengine and BytePlus, this also matches the coding-plan variants (`volcengine-plan/*`, `byteplus-plan/*`).If the preferred-provider filter yields no loaded models yet, onboarding falls back to the unfiltered catalog instead of leaving the picker empty.

Web-search follow-ups

Some web-search providers trigger provider-specific follow-up prompts:

- **Grok** can offer optional `x_search` setup with the same `XAI_API_KEY` and an `x_search` model choice.
- **Kimi** can ask for the Moonshot API region (`api.moonshot.ai` vs `api.moonshot.cn`) and the default Kimi web-search model.

Other behaviors

- Local onboarding DM scope behavior: [CLI setup reference](https://docs.openclaw.ai/start/wizard-cli-reference#outputs-and-internals).
- Fastest first chat: `openclaw dashboard` (Control UI, no channel setup).
- Custom provider: connect any OpenAI or Anthropic compatible endpoint, including hosted providers not listed. Use Unknown to auto-detect.
- If Hermes state is detected, onboarding offers a migration flow. Use [Migrate](https://docs.openclaw.ai/cli/migrate) for dry-run plans, overwrite mode, reports, and exact mappings.

## Common follow-up commands

```
openclaw configure
openclaw agents add <name>
```

`--json` does not imply non-interactive mode. Use `--non-interactive` for scripts.

[Migrate](https://docs.openclaw.ai/cli/migrate) [Reset](https://docs.openclaw.ai/cli/reset)

Ctrl+I

---

## Pairing - OpenClaw

_Source: <https://docs.openclaw.ai/cli/pairing>_

# `openclaw pairing`

Approve or inspect DM pairing requests (for channels that support pairing).Related:

- Pairing flow: [Pairing](https://docs.openclaw.ai/channels/pairing)

## Commands

```
openclaw pairing list telegram
openclaw pairing list --channel telegram --account work
openclaw pairing list telegram --json

openclaw pairing approve <code>
openclaw pairing approve telegram <code>
openclaw pairing approve --channel telegram --account work <code> --notify
```

## `pairing list`

List pending pairing requests for one channel.Options:

- `[channel]`: positional channel id
- `--channel <channel>`: explicit channel id
- `--account <accountId>`: account id for multi-account channels
- `--json`: machine-readable output

Notes:

- If multiple pairing-capable channels are configured, you must provide a channel either positionally or with `--channel`.
- Extension channels are allowed as long as the channel id is valid.

## `pairing approve`

Approve a pending pairing code and allow that sender.Usage:

- `openclaw pairing approve <channel> <code>`
- `openclaw pairing approve --channel <channel> <code>`
- `openclaw pairing approve <code>` when exactly one pairing-capable channel is configured

Options:

- `--channel <channel>`: explicit channel id
- `--account <accountId>`: account id for multi-account channels
- `--notify`: send a confirmation back to the requester on the same channel

Owner bootstrap:

- If `commands.ownerAllowFrom` is empty when you approve a pairing code, OpenClaw also records the approved sender as the command owner, using a channel-scoped entry such as `telegram:123456789`.
- This only bootstraps the first owner. Later pairing approvals do not replace or expand `commands.ownerAllowFrom`.
- The command owner is the human operator account allowed to run owner-only commands and approve dangerous actions such as `/diagnostics`, `/export-trajectory`, `/config`, and exec approvals.

## Notes

- Channel input: pass it positionally (`pairing list telegram`) or with `--channel <channel>`.
- `pairing list` supports `--account <accountId>` for multi-account channels.
- `pairing approve` supports `--account <accountId>` and `--notify`.
- If only one pairing-capable channel is configured, `pairing approve <code>` is allowed.
- If you approved a sender before this bootstrap existed, run `openclaw doctor`; it warns when no command owner is configured and shows the `openclaw config set commands.ownerAllowFrom ...` command to fix it.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Channel pairing](https://docs.openclaw.ai/channels/pairing)

[Directory](https://docs.openclaw.ai/cli/directory) [QR](https://docs.openclaw.ai/cli/qr)

Ctrl+I

---

## `pairing list`

_Source: <https://docs.openclaw.ai/cli/pairing.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Pairing

\# \`openclaw pairing\`

Approve or inspect DM pairing requests (for channels that support pairing).

Related:

\\* Pairing flow: \[Pairing\](/channels/pairing)

\## Commands

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw pairing list telegram
openclaw pairing list --channel telegram --account work
openclaw pairing list telegram --json

openclaw pairing approve ```
openclaw pairing approve telegram
openclaw pairing approve --channel telegram --account work  --notify
```

## `pairing list`

List pending pairing requests for one channel.

Options:

* `[channel]`: positional channel id
* `--channel `: explicit channel id
* `--account `: account id for multi-account channels
* `--json`: machine-readable output

Notes:

* If multiple pairing-capable channels are configured, you must provide a channel either positionally or with `--channel`.
* Extension channels are allowed as long as the channel id is valid.

## `pairing approve`

Approve a pending pairing code and allow that sender.

Usage:

* `openclaw pairing approve  `
* `openclaw pairing approve --channel  `
* `openclaw pairing approve ` when exactly one pairing-capable channel is configured

Options:

* `--channel `: explicit channel id
* `--account `: account id for multi-account channels
* `--notify`: send a confirmation back to the requester on the same channel

Owner bootstrap:

* If `commands.ownerAllowFrom` is empty when you approve a pairing code, OpenClaw also records the approved sender as the command owner, using a channel-scoped entry such as `telegram:123456789`.
* This only bootstraps the first owner. Later pairing approvals do not replace or expand `commands.ownerAllowFrom`.
* The command owner is the human operator account allowed to run owner-only commands and approve dangerous actions such as `/diagnostics`, `/export-trajectory`, `/config`, and exec approvals.

## Notes

* Channel input: pass it positionally (`pairing list telegram`) or with `--channel `.
* `pairing list` supports `--account ` for multi-account channels.
* `pairing approve` supports `--account ` and `--notify`.
* If only one pairing-capable channel is configured, `pairing approve ` is allowed.
* If you approved a sender before this bootstrap existed, run `openclaw doctor`; it warns when no command owner is configured and shows the `openclaw config set commands.ownerAllowFrom ...` command to fix it.

## Related

* [CLI reference](/cli)
* [Channel pairing](/channels/pairing)
```

---

## Plugins - OpenClaw

_Source: <https://docs.openclaw.ai/cli/plugins>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugins and skills

Plugins

Manage Gateway plugins, hook packs, and compatible bundles.

[**Plugin system** \\
\\
End-user guide for installing, enabling, and troubleshooting plugins.](https://docs.openclaw.ai/tools/plugin)

[**Manage plugins** \\
\\
Quick examples for install, list, update, uninstall, and publishing.](https://docs.openclaw.ai/plugins/manage-plugins)

[**Plugin bundles** \\
\\
Bundle compatibility model.](https://docs.openclaw.ai/plugins/bundles)

[**Plugin manifest** \\
\\
Manifest fields and config schema.](https://docs.openclaw.ai/plugins/manifest)

[**Security** \\
\\
Security hardening for plugin installs.](https://docs.openclaw.ai/gateway/security)

## Commands

```
openclaw plugins list
openclaw plugins list --enabled
openclaw plugins list --verbose
openclaw plugins list --json
openclaw plugins search <query>
openclaw plugins search <query> --limit 20
openclaw plugins search <query> --json
openclaw plugins install <path-or-spec>
openclaw plugins inspect <id>
openclaw plugins inspect <id> --runtime
openclaw plugins inspect <id> --json
openclaw plugins inspect --all
openclaw plugins info <id>
openclaw plugins enable <id>
openclaw plugins disable <id>
openclaw plugins registry
openclaw plugins registry --refresh
openclaw plugins uninstall <id>
openclaw plugins doctor
openclaw plugins update <id-or-npm-spec>
openclaw plugins update --all
openclaw plugins marketplace list <marketplace>
openclaw plugins marketplace list <marketplace> --json
```

For slow install, inspect, uninstall, or registry-refresh investigation, run the
command with `OPENCLAW_PLUGIN_LIFECYCLE_TRACE=1`. The trace writes phase timings
to stderr and keeps JSON output parseable. See [Debugging](https://docs.openclaw.ai/help/debugging#plugin-lifecycle-trace).

Bundled plugins ship with OpenClaw. Some are enabled by default (for example bundled model providers, bundled speech providers, and the bundled browser plugin); others require `plugins enable`.Native OpenClaw plugins must ship `openclaw.plugin.json` with an inline JSON Schema (`configSchema`, even if empty). Compatible bundles use their own bundle manifests instead.`plugins list` shows `Format: openclaw` or `Format: bundle`. Verbose list/info output also shows the bundle subtype (`codex`, `claude`, or `cursor`) plus detected bundle capabilities.

### Install

```
openclaw plugins search "calendar"                   # search ClawHub plugins
openclaw plugins install <package>                      # npm by default
openclaw plugins install clawhub:<package>              # ClawHub only
openclaw plugins install npm:<package>                  # npm only
openclaw plugins install git:github.com/<owner>/<repo>  # git repo
openclaw plugins install git:github.com/<owner>/<repo>@<ref>
openclaw plugins install <package> --force              # overwrite existing install
openclaw plugins install <package> --pin                # pin version
openclaw plugins install <package> --dangerously-force-unsafe-install
openclaw plugins install <path>                         # local path
openclaw plugins install <plugin>@<marketplace>         # marketplace
openclaw plugins install <plugin> --marketplace <name>  # marketplace (explicit)
openclaw plugins install <plugin> --marketplace https://github.com/<owner>/<repo>
```

Bare package names install from npm by default during the launch cutover. Use `clawhub:<package>` for ClawHub. Treat plugin installs like running code. Prefer pinned versions.

`plugins search` queries ClawHub for installable plugin packages and prints
install-ready package names. It searches code-plugin and bundle-plugin packages,
not skills. Use `openclaw skills search` for ClawHub skills.

ClawHub is the primary distribution and discovery surface for most plugins. Npm
remains a supported fallback and direct-install path. OpenClaw-owned
`@openclaw/*` plugin packages are published on npm again; see the current list
on [npmjs.com/org/openclaw](https://www.npmjs.com/org/openclaw) or the
[plugin inventory](https://docs.openclaw.ai/plugins/plugin-inventory). Stable installs use `latest`.
Beta-channel installs and updates prefer the npm `beta` dist-tag when that tag
is available, then fall back to `latest`.

Config includes and invalid-config recovery

If your `plugins` section is backed by a single-file `$include`, `plugins install/update/enable/disable/uninstall` write through to that included file and leave `openclaw.json` untouched. Root includes, include arrays, and includes with sibling overrides fail closed instead of flattening. See [Config includes](https://docs.openclaw.ai/gateway/configuration) for the supported shapes.If config is invalid during install, `plugins install` normally fails closed and tells you to run `openclaw doctor --fix` first. During Gateway startup, invalid config for one plugin is isolated to that plugin so other channels and plugins can keep running; `openclaw doctor --fix` can quarantine the invalid plugin entry. The only documented install-time exception is a narrow bundled-plugin recovery path for plugins that explicitly opt into `openclaw.install.allowInvalidConfigRecovery`.

--force and reinstall vs update

`--force` reuses the existing install target and overwrites an already-installed plugin or hook pack in place. Use it when you are intentionally reinstalling the same id from a new local path, archive, ClawHub package, or npm artifact. For routine upgrades of an already tracked npm plugin, prefer `openclaw plugins update <id-or-npm-spec>`.If you run `plugins install` for a plugin id that is already installed, OpenClaw stops and points you at `plugins update <id-or-npm-spec>` for a normal upgrade, or at `plugins install <package> --force` when you genuinely want to overwrite the current install from a different source.

--pin scope

`--pin` applies to npm installs only. It is not supported with `git:` installs; use an explicit git ref such as `git:github.com/acme/plugin@v1.2.3` when you want a pinned source. It is not supported with `--marketplace`, because marketplace installs persist marketplace source metadata instead of an npm spec.

--dangerously-force-unsafe-install

`--dangerously-force-unsafe-install` is a break-glass option for false positives in the built-in dangerous-code scanner. It allows the install to continue even when the built-in scanner reports `critical` findings, but it does **not** bypass plugin `before_install` hook policy blocks and does **not** bypass scan failures.This CLI flag applies to plugin install/update flows. Gateway-backed skill dependency installs use the matching `dangerouslyForceUnsafeInstall` request override, while `openclaw skills install` remains a separate ClawHub skill download/install flow.If a plugin you published on ClawHub is blocked by a registry scan, use the publisher steps in [ClawHub](https://docs.openclaw.ai/tools/clawhub).

Hook packs and npm specs

`plugins install` is also the install surface for hook packs that expose `openclaw.hooks` in `package.json`. Use `openclaw hooks` for filtered hook visibility and per-hook enablement, not package installation.Npm specs are **registry-only** (package name + optional **exact version** or **dist-tag**). Git/URL/file specs and semver ranges are rejected. Dependency installs run project-local with `--ignore-scripts` for safety, even when your shell has global npm install settings.Use `npm:<package>` when you want to make npm resolution explicit. Bare package specs also install directly from npm during the launch cutover.Bare specs and `@latest` stay on the stable track. If npm resolves either of those to a prerelease, OpenClaw stops and asks you to opt in explicitly with a prerelease tag such as `@beta`/`@rc` or an exact prerelease version such as `@1.2.3-beta.4`.If a bare install spec matches an official plugin id (for example `diffs`), OpenClaw installs the catalog entry directly. To install an npm package with the same name, use an explicit scoped spec (for example `@scope/diffs`).

Git repositories

Use `git:<repo>` to install directly from a git repository. Supported forms include `git:github.com/owner/repo`, `git:owner/repo`, full `https://`, `ssh://`, `git://`, `file://`, and `git@host:owner/repo.git` clone URLs. Add `@<ref>` or `#<ref>` to check out a branch, tag, or commit before install.Git installs clone into a temporary directory, check out the requested ref when present, then use the normal plugin directory installer. That means manifest validation, dangerous-code scanning, package-manager install work, and install records behave like npm installs. Recorded git installs include the source URL/ref plus the resolved commit so `openclaw plugins update` can re-resolve the source later.After installing from git, use `openclaw plugins inspect <id> --runtime --json` to verify runtime registrations such as gateway methods and CLI commands. If the plugin registered a CLI root with `api.registerCli`, execute that command directly through the OpenClaw root CLI, for example `openclaw demo-plugin ping`.

Archives

Supported archives: `.zip`, `.tgz`, `.tar.gz`, `.tar`. Native OpenClaw plugin archives must contain a valid `openclaw.plugin.json` at the extracted plugin root; archives that only contain `package.json` are rejected before OpenClaw writes install records.Claude marketplace installs are also supported.

ClawHub installs use an explicit `clawhub:<package>` locator:

```
openclaw plugins install clawhub:openclaw-codex-app-server
openclaw plugins install clawhub:openclaw-codex-app-server@1.2.3
```

Bare npm-safe plugin specs install from npm by default during the launch cutover:

```
openclaw plugins install openclaw-codex-app-server
```

Use `npm:` to make npm-only resolution explicit:

```
openclaw plugins install npm:openclaw-codex-app-server
openclaw plugins install npm:@scope/plugin-name@1.0.1
```

OpenClaw checks the advertised plugin API / minimum gateway compatibility before install. When the selected ClawHub version publishes a ClawPack artifact, OpenClaw downloads the versioned npm-pack `.tgz`, verifies the ClawHub digest header and the artifact digest, then installs it through the normal archive path. Older ClawHub versions without ClawPack metadata still install through the legacy package archive verification path. Recorded installs keep their ClawHub source metadata, artifact kind, npm integrity, npm shasum, tarball name, and ClawPack digest facts for later updates.
Unversioned ClawHub installs keep an unversioned recorded spec so `openclaw plugins update` can follow newer ClawHub releases; explicit version or tag selectors such as `clawhub:pkg@1.2.3` and `clawhub:pkg@beta` remain pinned to that selector.

#### Marketplace shorthand

Use `plugin@marketplace` shorthand when the marketplace name exists in Claude’s local registry cache at `~/.claude/plugins/known_marketplaces.json`:

```
openclaw plugins marketplace list <marketplace-name>
openclaw plugins install <plugin-name>@<marketplace-name>
```

Use `--marketplace` when you want to pass the marketplace source explicitly:

```
openclaw plugins install <plugin-name> --marketplace <marketplace-name>
openclaw plugins install <plugin-name> --marketplace <owner/repo>
openclaw plugins install <plugin-name> --marketplace https://github.com/<owner>/<repo>
openclaw plugins install <plugin-name> --marketplace ./my-marketplace
```

- Marketplace sources

- Remote marketplace rules

- a Claude known-marketplace name from `~/.claude/plugins/known_marketplaces.json`
- a local marketplace root or `marketplace.json` path
- a GitHub repo shorthand such as `owner/repo`
- a GitHub repo URL such as `https://github.com/owner/repo`
- a git URL

For remote marketplaces loaded from GitHub or git, plugin entries must stay inside the cloned marketplace repo. OpenClaw accepts relative path sources from that repo and rejects HTTP(S), absolute-path, git, GitHub, and other non-path plugin sources from remote manifests.

For local paths and archives, OpenClaw auto-detects:

- native OpenClaw plugins (`openclaw.plugin.json`)
- Codex-compatible bundles (`.codex-plugin/plugin.json`)
- Claude-compatible bundles (`.claude-plugin/plugin.json` or the default Claude component layout)
- Cursor-compatible bundles (`.cursor-plugin/plugin.json`)

Compatible bundles install into the normal plugin root and participate in the same list/info/enable/disable flow. Today, bundle skills, Claude command-skills, Claude `settings.json` defaults, Claude `.lsp.json` / manifest-declared `lspServers` defaults, Cursor command-skills, and compatible Codex hook directories are supported; other detected bundle capabilities are shown in diagnostics/info but are not yet wired into runtime execution.

### List

```
openclaw plugins list
openclaw plugins list --enabled
openclaw plugins list --verbose
openclaw plugins list --json
openclaw plugins search <query>
openclaw plugins search <query> --limit 20
openclaw plugins search <query> --json
```

[​](https://docs.openclaw.ai/cli/plugins#param-enabled)

--enabled

boolean

Show only enabled plugins.

[​](https://docs.openclaw.ai/cli/plugins#param-verbose)

--verbose

boolean

Switch from the table view to per-plugin detail lines with source/origin/version/activation metadata.

[​](https://docs.openclaw.ai/cli/plugins#param-json)

--json

boolean

Machine-readable inventory plus registry diagnostics and package dependency install state.

`plugins list` reads the persisted local plugin registry first, with a manifest-only derived fallback when the registry is missing or invalid. It is useful for checking whether a plugin is installed, enabled, and visible to cold startup planning, but it is not a live runtime probe of an already-running Gateway process. After changing plugin code, enablement, hook policy, or `plugins.load.paths`, restart the Gateway that serves the channel before expecting new `register(api)` code or hooks to run. For remote/container deployments, verify you are restarting the actual `openclaw gateway run` child, not only a wrapper process.`plugins list --json` includes each plugin’s `dependencyStatus` from `package.json``dependencies` and `optionalDependencies`. OpenClaw checks whether those package
names are present along the plugin’s normal Node `node_modules` lookup path; it
does not import plugin runtime code, run a package manager, or repair missing
dependencies.

`plugins search` is a remote ClawHub catalog lookup. It does not inspect local
state, mutate config, install packages, or load plugin runtime code. Search
results include the ClawHub package name, family, channel, version, summary, and
an install hint such as `openclaw plugins install clawhub:<package>`.For bundled plugin work inside a packaged Docker image, bind-mount the plugin
source directory over the matching packaged source path, such as
`/app/extensions/synology-chat`. OpenClaw will discover that mounted source
overlay before `/app/dist/extensions/synology-chat`; a plain copied source
directory remains inert so normal packaged installs still use compiled dist.For runtime hook debugging:

- `openclaw plugins inspect <id> --runtime --json` shows registered hooks and diagnostics from a module-loaded inspection pass. Runtime inspection never installs dependencies; use `openclaw doctor --fix` to clean legacy dependency state or install missing configured downloadable plugins.
- `openclaw gateway status --deep --require-rpc` confirms the reachable Gateway, service/process hints, config path, and RPC health.
- Non-bundled conversation hooks (`llm_input`, `llm_output`, `before_agent_finalize`, `agent_end`) require `plugins.entries.<id>.hooks.allowConversationAccess=true`.

Use `--link` to avoid copying a local directory (adds to `plugins.load.paths`):

```
openclaw plugins install -l ./my-plugin
```

`--force` is not supported with `--link` because linked installs reuse the source path instead of copying over a managed install target.Use `--pin` on npm installs to save the resolved exact spec (`name@version`) in the managed plugin index while keeping the default behavior unpinned.

### Plugin index

Plugin install metadata is machine-managed state, not user config. Installs and updates write it to `plugins/installs.json` under the active OpenClaw state directory. Its top-level `installRecords` map is the durable source of install metadata, including records for broken or missing plugin manifests. The `plugins` array is the manifest-derived cold registry cache. The file includes a do-not-edit warning and is used by `openclaw plugins update`, uninstall, diagnostics, and the cold plugin registry.When OpenClaw sees shipped legacy `plugins.installs` records in config, it moves them into the plugin index and removes the config key; if either write fails, the config records are kept so the install metadata is not lost.

### Uninstall

```
openclaw plugins uninstall <id>
openclaw plugins uninstall <id> --dry-run
openclaw plugins uninstall <id> --keep-files
```

`uninstall` removes plugin records from `plugins.entries`, the persisted plugin index, plugin allow/deny list entries, and linked `plugins.load.paths` entries when applicable. Unless `--keep-files` is set, uninstall also removes the tracked managed install directory when it is inside OpenClaw’s plugin extensions root. For active memory plugins, the memory slot resets to `memory-core`.

`--keep-config` is supported as a deprecated alias for `--keep-files`.

### Update

```
openclaw plugins update <id-or-npm-spec>
openclaw plugins update --all
openclaw plugins update <id-or-npm-spec> --dry-run
openclaw plugins update @openclaw/voice-call
openclaw plugins update openclaw-codex-app-server --dangerously-force-unsafe-install
```

Updates apply to tracked plugin installs in the managed plugin index and tracked hook-pack installs in `hooks.internal.installs`.

Resolving plugin id vs npm spec

When you pass a plugin id, OpenClaw reuses the recorded install spec for that plugin. That means previously stored dist-tags such as `@beta` and exact pinned versions continue to be used on later `update <id>` runs.For npm installs, you can also pass an explicit npm package spec with a dist-tag or exact version. OpenClaw resolves that package name back to the tracked plugin record, updates that installed plugin, and records the new npm spec for future id-based updates.Passing the npm package name without a version or tag also resolves back to the tracked plugin record. Use this when a plugin was pinned to an exact version and you want to move it back to the registry’s default release line.

Beta channel updates

`openclaw plugins update` reuses the tracked plugin spec unless you pass a new spec. `openclaw update` additionally knows the active OpenClaw update channel: on the beta channel, default-line npm and ClawHub plugin records try `@beta` first, then fall back to the recorded default/latest spec if no plugin beta release exists. Exact versions and explicit tags stay pinned to that selector.

Version checks and integrity drift

Before a live npm update, OpenClaw checks the installed package version against the npm registry metadata. If the installed version and recorded artifact identity already match the resolved target, the update is skipped without downloading, reinstalling, or rewriting `openclaw.json`.When a stored integrity hash exists and the fetched artifact hash changes, OpenClaw treats that as npm artifact drift. The interactive `openclaw plugins update` command prints the expected and actual hashes and asks for confirmation before proceeding. Non-interactive update helpers fail closed unless the caller supplies an explicit continuation policy.

--dangerously-force-unsafe-install on update

`--dangerously-force-unsafe-install` is also available on `plugins update` as a break-glass override for built-in dangerous-code scan false positives during plugin updates. It still does not bypass plugin `before_install` policy blocks or scan-failure blocking, and it only applies to plugin updates, not hook-pack updates.

### Inspect

```
openclaw plugins inspect <id>
openclaw plugins inspect <id> --runtime
openclaw plugins inspect <id> --json
```

Inspect shows identity, load status, source, manifest capabilities, policy flags, diagnostics, install metadata, bundle capabilities, and any detected MCP or LSP server support without importing plugin runtime by default. Add `--runtime` to load the plugin module and include registered hooks, tools, commands, services, gateway methods, and HTTP routes. Runtime inspection reports missing plugin dependencies directly; installs and repairs stay in `openclaw plugins install`, `openclaw plugins update`, and `openclaw doctor --fix`.Plugin-owned CLI commands are installed as root `openclaw` command groups. After `inspect --runtime` shows a command under `cliCommands`, run it as `openclaw <command> ...`; for example a plugin that registers `demo-git` can be verified with `openclaw demo-git ping`.Each plugin is classified by what it actually registers at runtime:

- **plain-capability** — one capability type (e.g. a provider-only plugin)
- **hybrid-capability** — multiple capability types (e.g. text + speech + images)
- **hook-only** — only hooks, no capabilities or surfaces
- **non-capability** — tools/commands/services but no capabilities

See [Plugin shapes](https://docs.openclaw.ai/plugins/architecture#plugin-shapes) for more on the capability model.

The `--json` flag outputs a machine-readable report suitable for scripting and auditing. `inspect --all` renders a fleet-wide table with shape, capability kinds, compatibility notices, bundle capabilities, and hook summary columns. `info` is an alias for `inspect`.

### Doctor

```
openclaw plugins doctor
```

`doctor` reports plugin load errors, manifest/discovery diagnostics, and compatibility notices. When everything is clean it prints `No plugin issues detected.`For module-shape failures such as missing `register`/`activate` exports, rerun with `OPENCLAW_PLUGIN_LOAD_DEBUG=1` to include a compact export-shape summary in the diagnostic output.

### Registry

```
openclaw plugins registry
openclaw plugins registry --refresh
openclaw plugins registry --json
```

The local plugin registry is OpenClaw’s persisted cold read model for installed plugin identity, enablement, source metadata, and contribution ownership. Normal startup, provider owner lookup, channel setup classification, and plugin inventory can read it without importing plugin runtime modules.Use `plugins registry` to inspect whether the persisted registry is present, current, or stale. Use `--refresh` to rebuild it from the persisted plugin index, config policy, and manifest/package metadata. This is a repair path, not a runtime activation path.

`OPENCLAW_DISABLE_PERSISTED_PLUGIN_REGISTRY=1` is a deprecated break-glass compatibility switch for registry read failures. Prefer `plugins registry --refresh` or `openclaw doctor --fix`; the env fallback is only for emergency startup recovery while the migration rolls out.

### Marketplace

```
openclaw plugins marketplace list <source>
openclaw plugins marketplace list <source> --json
```

Marketplace list accepts a local marketplace path, a `marketplace.json` path, a GitHub shorthand like `owner/repo`, a GitHub repo URL, or a git URL. `--json` prints the resolved source label plus the parsed marketplace manifest and plugin entries.

## Related

- [Building plugins](https://docs.openclaw.ai/plugins/building-plugins)
- [CLI reference](https://docs.openclaw.ai/cli)
- [Community plugins](https://docs.openclaw.ai/plugins/community)

[Webhooks](https://docs.openclaw.ai/cli/webhooks) [Skills](https://docs.openclaw.ai/cli/skills)

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
- `--skip-provider-setup`: skip provider setup and map credentials to existing providers.
- `--agent <id>`: scope `auth-profiles.json` target discovery and writes to one agent store.
- `--allow-exec`: allow exec SecretRef checks during preflight/apply (may execute provider commands).

Notes:

- Requires an interactive TTY.
- You cannot combine `--providers-only` with `--skip-provider-setup`.
- `configure` targets secret-bearing fields in `openclaw.json` plus `auth-profiles.json` for the selected agent scope.
- `configure` supports creating new `auth-profiles.json` mappings directly in the picker flow.
- Canonical supported surface: [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface).
- It performs preflight resolution before apply.
- If preflight/apply includes exec refs, keep `--allow-exec` set for both steps.
- Generated plans default to scrub options (`scrubEnv`, `scrubAuthProfilesForProviderTargets`, `scrubLegacyAuthJson` all enabled).
- Apply path is one-way for scrubbed plaintext values.
- Without `--apply`, CLI still prompts `Apply this plan now?` after preflight.
- With `--apply` (and no `--yes`), CLI prompts an extra irreversible confirmation.
- `--json` prints the plan + preflight report, but the command still requires an interactive TTY.

Exec provider safety note:

- Homebrew installs often expose symlinked binaries under `/opt/homebrew/bin/*`.
- Set `allowSymlinkCommand: true` only when needed for trusted package-manager paths, and pair it with `trustedDirs` (for example `["/opt/homebrew"]`).
- On Windows, if ACL verification is unavailable for a provider path, OpenClaw fails closed. For trusted paths only, set `allowInsecurePath: true` on that provider to bypass path security checks.

## Apply a saved plan

Apply or preflight a plan generated previously:

```
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --allow-exec
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run --allow-exec
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --json
```

Exec behavior:

- `--dry-run` validates preflight without writing files.
- exec SecretRef checks are skipped by default in dry-run.
- write mode rejects plans that contain exec SecretRefs/providers unless `--allow-exec` is set.
- Use `--allow-exec` to opt in to exec provider checks/execution in either mode.

Plan contract details (allowed target paths, validation rules, and failure semantics):

- [Secrets Apply Plan Contract](https://docs.openclaw.ai/gateway/secrets-plan-contract)

What `apply` may update:

- `openclaw.json` (SecretRef targets + provider upserts/deletes)
- `auth-profiles.json` (provider-target scrubbing)
- legacy `auth.json` residues
- `~/.openclaw/.env` known secret keys whose values were migrated

## Why no rollback backups

`secrets apply` intentionally does not write rollback backups containing old plaintext values.Safety comes from strict preflight + atomic-ish apply with best-effort in-memory restore on failure.

## Example

```
openclaw secrets audit --check
openclaw secrets configure
openclaw secrets audit --check
```

If `audit --check` still reports plaintext findings, update the remaining reported target paths and rerun audit.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Secrets management](https://docs.openclaw.ai/gateway/secrets)

[Reset](https://docs.openclaw.ai/cli/reset) [Security](https://docs.openclaw.ai/cli/security)

Ctrl+I

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
- `--store <path>`: run against a specific `sessions.json` file.
- `--json`: print a JSON summary. With `--all-agents`, output includes one summary per store.

`openclaw sessions cleanup --all-agents --dry-run --json`:

```
{
  "allAgents": true,
  "mode": "warn",
  "dryRun": true,
  "stores": [\
    {\
      "agentId": "main",\
      "storePath": "/home/user/.openclaw/agents/main/sessions/sessions.json",\
      "beforeCount": 120,\
      "afterCount": 80,\
      "pruned": 40,\
      "capped": 0\
    },\
    {\
      "agentId": "work",\
      "storePath": "/home/user/.openclaw/agents/work/sessions/sessions.json",\
      "beforeCount": 18,\
      "afterCount": 18,\
      "pruned": 0,\
      "capped": 0\
    }\
  ]
}
```

Related:

- Session config: [Configuration reference](https://docs.openclaw.ai/gateway/config-agents#session)

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Session management](https://docs.openclaw.ai/concepts/session)

[Models](https://docs.openclaw.ai/cli/models) [System](https://docs.openclaw.ai/cli/system)

Ctrl+I

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

## Skills - OpenClaw

_Source: <https://docs.openclaw.ai/cli/skills>_

# `openclaw skills`

Inspect local skills and install/update skills from ClawHub.Related:

- Skills system: [Skills](https://docs.openclaw.ai/tools/skills)
- Skills config: [Skills config](https://docs.openclaw.ai/tools/skills-config)
- ClawHub installs: [ClawHub](https://docs.openclaw.ai/tools/clawhub)

## Commands

```
openclaw skills search "calendar"
openclaw skills search --limit 20 --json
openclaw skills install <slug>
openclaw skills install <slug> --version <version>
openclaw skills install <slug> --force
openclaw skills install <slug> --agent <id>
openclaw skills update <slug>
openclaw skills update --all
openclaw skills update --all --agent <id>
openclaw skills list
openclaw skills list --eligible
openclaw skills list --json
openclaw skills list --verbose
openclaw skills list --agent <id>
openclaw skills info <name>
openclaw skills info <name> --json
openclaw skills info <name> --agent <id>
openclaw skills check
openclaw skills check --agent <id>
openclaw skills check --json
```

`search`/`install`/`update` use ClawHub directly and install into the active
workspace `skills/` directory. `list`/`info`/`check` still inspect the local
skills visible to the current workspace and config. Workspace-backed commands
resolve the target workspace from `--agent <id>`, then the current working
directory when it is inside a configured agent workspace, then the default
agent.This CLI `install` command downloads skill folders from ClawHub. Gateway-backed
skill dependency installs triggered from onboarding or Skills settings use the
separate `skills.install` request path instead.Notes:

- `search [query...]` accepts an optional query; omit it to browse the default
ClawHub search feed.
- `search --limit <n>` caps returned results.
- `install --force` overwrites an existing workspace skill folder for the same
slug.
- `--agent <id>` targets one configured agent workspace and overrides current
working directory inference.
- `update --all` only updates tracked ClawHub installs in the active workspace.
- `check --agent <id>` checks the selected agent’s workspace and reports which
ready skills are actually visible to that agent’s prompt or command surface.
- `list` is the default action when no subcommand is provided.
- `list`, `info`, and `check` write their rendered output to stdout. With
`--json`, that means the machine-readable payload stays on stdout for pipes
and scripts.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Skills](https://docs.openclaw.ai/tools/skills)

[Plugins](https://docs.openclaw.ai/cli/plugins) [Dashboard](https://docs.openclaw.ai/cli/dashboard)

Ctrl+I

---

## https://docs.openclaw.ai/cli/skills.md

_Source: <https://docs.openclaw.ai/cli/skills.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Skills

\# \`openclaw skills\`

Inspect local skills and install/update skills from ClawHub.

Related:

\\* Skills system: \[Skills\](/tools/skills)
\\* Skills config: \[Skills config\](/tools/skills-config)
\\* ClawHub installs: \[ClawHub\](/tools/clawhub)

\## Commands

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw skills search "calendar"
openclaw skills search --limit 20 --json
openclaw skills install
openclaw skills install  --version
openclaw skills install  --force
openclaw skills install  --agent
openclaw skills update
openclaw skills update --all
openclaw skills update --all --agent
openclaw skills list
openclaw skills list --eligible
openclaw skills list --json
openclaw skills list --verbose
openclaw skills list --agent
openclaw skills info
openclaw skills info  --json
openclaw skills info  --agent
openclaw skills check
openclaw skills check --json
openclaw skills check --agent
\`\`\`

\`search\`/\`install\`/\`update\` use ClawHub directly and install into the active
workspace \`skills/\` directory. \`list\`/\`info\`/\`check\` still inspect the local
skills visible to the current workspace and config. Workspace-backed commands
resolve the target workspace from \`--agent \`, then the current working
directory when it is inside a configured agent workspace, then the default
agent.

This CLI \`install\` command downloads skill folders from ClawHub. Gateway-backed
skill dependency installs triggered from onboarding or Skills settings use the
separate \`skills.install\` request path instead.

Notes:

\\* \`search \[query...\]\` accepts an optional query; omit it to browse the default
 ClawHub search feed.
\\* \`search --limit \` caps returned results.
\\* \`install --force\` overwrites an existing workspace skill folder for the same
 slug.
\\* \`--agent \` targets one configured agent workspace and overrides current
 working directory inference.
\\* \`update --all\` only updates tracked ClawHub installs in the active workspace.
\\* \`list\` is the default action when no subcommand is provided.
\\* \`list\`, \`info\`, and \`check\` write their rendered output to stdout. With
 \`--json\`, that means the machine-readable payload stays on stdout for pipes
 and scripts.

\## Related

\\* \[CLI reference\](/cli)
\\* \[Skills\](/tools/skills)

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

## Uninstall - OpenClaw

_Source: <https://docs.openclaw.ai/cli/uninstall>_

# `openclaw uninstall`

Uninstall the gateway service + local data (CLI remains).Options:

- `--service`: remove the gateway service
- `--state`: remove state and config
- `--workspace`: remove workspace directories
- `--app`: remove the macOS app
- `--all`: remove service, state, workspace, and app
- `--yes`: skip confirmation prompts
- `--non-interactive`: disable prompts; requires `--yes`
- `--dry-run`: print actions without removing files

Examples:

```
openclaw backup create
openclaw uninstall
openclaw uninstall --service --yes --non-interactive
openclaw uninstall --state --workspace --yes --non-interactive
openclaw uninstall --all --yes
openclaw uninstall --dry-run
```

Notes:

- Run `openclaw backup create` first if you want a restorable snapshot before removing state or workspaces.
- `--all` is shorthand for removing service, state, workspace, and app together.
- `--non-interactive` requires `--yes`.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Uninstall](https://docs.openclaw.ai/install/uninstall)

[Status](https://docs.openclaw.ai/cli/status) [Update](https://docs.openclaw.ai/cli/update)

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
package-manager updates stop the running service before replacing the package
tree, then refresh the service metadata from the updated install, restart the
service, and verify the restarted Gateway reports the expected version. With
`--no-restart`, package replacement still runs but the managed service is not
stopped or restarted, so the running Gateway may keep old code until you restart
it manually.

## Git checkout flow

### Channel selection

- `stable`: checkout the latest non-beta tag, then build and doctor.
- `beta`: prefer the latest `-beta` tag, but fall back to the latest stable tag when beta is missing or older.
- `dev`: checkout `main`, then fetch and rebase.

### Update steps

1

[Navigate to header](https://docs.openclaw.ai/cli/update#)

Verify clean worktree

Requires no uncommitted changes.

2

[Navigate to header](https://docs.openclaw.ai/cli/update#)

Switch channel

Switches to the selected channel (tag or branch).

3

[Navigate to header](https://docs.openclaw.ai/cli/update#)

Fetch upstream

Dev only.

4

[Navigate to header](https://docs.openclaw.ai/cli/update#)

Preflight build (dev only)

Runs lint and TypeScript build in a temp worktree. If the tip fails, walks back up to 10 commits to find the newest clean build.

5

[Navigate to header](https://docs.openclaw.ai/cli/update#)

Rebase

Rebases onto the selected commit (dev only).

6

[Navigate to header](https://docs.openclaw.ai/cli/update#)

Install dependencies

Uses the repo package manager. For pnpm checkouts, the updater bootstraps `pnpm` on demand (via `corepack` first, then a temporary `npm install pnpm@10` fallback) instead of running `npm run build` inside a pnpm workspace.

7

[Navigate to header](https://docs.openclaw.ai/cli/update#)

Build Control UI

Builds the gateway and the Control UI.

8

[Navigate to header](https://docs.openclaw.ai/cli/update#)

Run doctor

`openclaw doctor` runs as the final safe-update check.

9

[Navigate to header](https://docs.openclaw.ai/cli/update#)

Sync plugins

Syncs plugins to the active channel. Dev uses bundled plugins; stable and beta use npm. Updates tracked plugin installs.

On the beta update channel, tracked npm and ClawHub plugin installs that follow
the default/latest line try a plugin `@beta` release first. If the plugin has no
beta release, OpenClaw falls back to the recorded default/latest spec. Exact
versions and explicit tags are not rewritten.

If an exact pinned npm plugin update resolves to an artifact whose integrity differs from the stored install record, `openclaw update` aborts that plugin artifact update instead of installing it. Reinstall or update the plugin explicitly only after verifying that you trust the new artifact.

Post-update plugin sync failures fail the update result and stop restart follow-up work. Fix the plugin install or update error, then rerun `openclaw update`.When the updated Gateway starts, plugin loading is verify-only: startup does not run package managers or mutate dependency trees. Package-manager `update.run` restarts bypass the normal idle deferral and restart cooldown after the package tree has been swapped, so the old process cannot keep lazy-loading removed chunks.If pnpm bootstrap still fails, the updater stops early with a package-manager-specific error instead of trying `npm run build` inside the checkout.

## `--update` shorthand

`openclaw --update` rewrites to `openclaw update` (useful for shells and launcher scripts).

## Related

- `openclaw doctor` (offers to run update first on git checkouts)
- [Development channels](https://docs.openclaw.ai/install/development-channels)
- [Updating](https://docs.openclaw.ai/install/updating)
- [CLI reference](https://docs.openclaw.ai/cli)

[Uninstall](https://docs.openclaw.ai/cli/uninstall) [Agent](https://docs.openclaw.ai/cli/agent)

Ctrl+I

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

Examples:

```
openclaw wiki search "bgroux" --mode find-person
openclaw wiki search "who knows Teams rollout?" --mode route-question
openclaw wiki search "maintainer-whois" --mode source-evidence
openclaw wiki search "strong route Teams" --mode raw-claim --json
```

Text output includes `Claim:` and `Evidence:` lines when a result matches a
structured claim. JSON output additionally exposes `matchedClaimId`,
`matchedClaimStatus`, `matchedClaimConfidence`, `evidenceKinds`, and
`evidenceSourceIds` for agent-side drilldown.

### `wiki get <lookup>`

Read a wiki page by id or relative path.Examples:

```
openclaw wiki get entity.alpha
openclaw wiki get syntheses/alpha-summary.md --from 1 --lines 80
```

### `wiki apply`

Apply narrow mutations without freeform page surgery.Supported flows include:

- create/update a synthesis page
- update page metadata
- attach source ids
- add questions
- add contradictions
- update confidence/status
- write structured claims

This command exists so the wiki can evolve safely without manually editing
managed blocks.

### `wiki bridge import`

Import public memory artifacts from the active memory plugin into bridge-backed
source pages.Use this in `bridge` mode when you want the latest exported memory artifacts
pulled into the wiki vault.For active bridge artifact reads, the CLI routes the import through Gateway RPC
so the import uses the runtime memory plugin context. If bridge imports are
disabled or artifact reads are turned off, the command keeps the local/offline
zero-import behavior.

### `wiki unsafe-local import`

Import from explicitly configured local paths in `unsafe-local` mode.This is intentionally experimental and same-machine only.

### `wiki obsidian ...`

Obsidian helper commands for vaults running in Obsidian-friendly mode.Subcommands:

- `status`
- `search`
- `open`
- `command`
- `daily`

These require the official `obsidian` CLI on `PATH` when
`obsidian.useOfficialCli` is enabled.

## Practical usage guidance

- Use `wiki search` \+ `wiki get` when provenance and page identity matter.
- Use `wiki apply` instead of hand-editing managed generated sections.
- Use `wiki lint` before trusting contradictory or low-confidence content.
- Use `wiki compile` after bulk imports or source changes when you want fresh
dashboards and compiled digests immediately.
- Use `wiki bridge import` when bridge mode depends on newly exported memory
artifacts.

## Configuration tie-ins

`openclaw wiki` behavior is shaped by:

- `plugins.entries.memory-wiki.config.vaultMode`
- `plugins.entries.memory-wiki.config.search.backend`
- `plugins.entries.memory-wiki.config.search.corpus`
- `plugins.entries.memory-wiki.config.bridge.*`
- `plugins.entries.memory-wiki.config.obsidian.*`
- `plugins.entries.memory-wiki.config.render.*`
- `plugins.entries.memory-wiki.config.context.includeCompiledDigestPrompt`

See [Memory Wiki plugin](https://docs.openclaw.ai/plugins/memory-wiki) for the full config model.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Memory wiki](https://docs.openclaw.ai/plugins/memory-wiki)

[Proxy](https://docs.openclaw.ai/cli/proxy) [RPC adapters](https://docs.openclaw.ai/reference/rpc)

Ctrl+I

---

## Log - OpenClaw

_Source: <https://docs.openclaw.ai/it/cli/logs>_

# `openclaw logs`

Esegue il tail dei log su file del Gateway tramite RPC (funziona in modalità remota).Correlati:

- Panoramica dei log: [Log](https://docs.openclaw.ai/it/logging)
- CLI del Gateway: [gateway](https://docs.openclaw.ai/it/cli/gateway)

## Opzioni

- `--limit <n>`: numero massimo di righe di log da restituire (predefinito `200`)
- `--max-bytes <n>`: numero massimo di byte da leggere dal file di log (predefinito `250000`)
- `--follow`: segue lo stream dei log
- `--interval <ms>`: intervallo di polling durante il follow (predefinito `1000`)
- `--json`: emette eventi JSON delimitati da righe
- `--plain`: output in testo normale senza formattazione stilizzata
- `--no-color`: disabilita i colori ANSI
- `--local-time`: mostra i timestamp nel tuo fuso orario locale

## Opzioni RPC condivise del Gateway

`openclaw logs` accetta anche i flag client standard del Gateway:

- `--url <url>`: URL WebSocket del Gateway
- `--token <token>`: token del Gateway
- `--timeout <ms>`: timeout in ms (predefinito `30000`)
- `--expect-final`: attende una risposta finale quando la chiamata al Gateway è supportata da un agente

Quando passi `--url`, la CLI non applica automaticamente la configurazione o le credenziali dell’ambiente. Includi `--token` esplicitamente se il Gateway di destinazione richiede l’autenticazione.

## Esempi

```
openclaw logs
openclaw logs --follow
openclaw logs --follow --interval 2000
openclaw logs --limit 500 --max-bytes 500000
openclaw logs --json
openclaw logs --plain
openclaw logs --no-color
openclaw logs --limit 500
openclaw logs --local-time
openclaw logs --follow --local-time
openclaw logs --url ws://127.0.0.1:18789 --token "$OPENCLAW_GATEWAY_TOKEN"
```

## Note

- Usa `--local-time` per mostrare i timestamp nel tuo fuso orario locale.
- Se il Gateway local loopback implicito richiede l’abbinamento, si chiude durante la connessione o va in timeout prima che `logs.tail` risponda, `openclaw logs` ripiega automaticamente sul log su file del Gateway configurato. Le destinazioni `--url` esplicite non usano questo fallback.

## Correlati

- [Riferimento CLI](https://docs.openclaw.ai/it/cli)
- [Log del Gateway](https://docs.openclaw.ai/it/gateway/logging)

[Salute](https://docs.openclaw.ai/it/cli/health) [Migrare](https://docs.openclaw.ai/it/cli/migrate)

Ctrl+I

---

## Node - OpenClaw

_Source: <https://docs.openclaw.ai/it/cli/node>_

# `openclaw node`

Esegui un **host Node headless** che si connette al WebSocket del Gateway ed espone
`system.run` / `system.which` su questa macchina.

## Perché usare un host Node?

Usa un host Node quando vuoi che gli agenti **eseguano comandi su altre macchine** nella tua
rete senza installare lì un’app companion macOS completa.Casi d’uso comuni:

- Eseguire comandi su macchine Linux/Windows remote (server di build, macchine di laboratorio, NAS).
- Mantenere l’exec **sandboxed** sul gateway, ma delegare le esecuzioni approvate ad altri host.
- Fornire un target di esecuzione leggero e headless per nodi di automazione o CI.

L’esecuzione resta comunque protetta da **approvazioni exec** e da allowlist per agente sull’host
Node, così puoi mantenere l’accesso ai comandi limitato ed esplicito.

## Proxy browser (zero-config)

Gli host Node pubblicizzano automaticamente un proxy browser se `browser.enabled` non è
disabilitato sul nodo. Questo consente all’agente di usare l’automazione del browser su quel nodo
senza configurazione aggiuntiva.Per impostazione predefinita, il proxy espone la normale superficie del profilo browser del nodo. Se
imposti `nodeHost.browserProxy.allowProfiles`, il proxy diventa restrittivo:
il targeting di profili non presenti nella allowlist viene rifiutato e le route di
creazione/eliminazione di profili persistenti vengono bloccate tramite il proxy.Disabilitalo sul nodo, se necessario:

```
{
  nodeHost: {
    browserProxy: {
      enabled: false,
    },
  },
}
```

## Esecuzione (foreground)

```
openclaw node run --host <gateway-host> --port 18789
```

Opzioni:

- `--host <host>`: host WebSocket del Gateway (predefinito: `127.0.0.1`)
- `--port <port>`: porta WebSocket del Gateway (predefinito: `18789`)
- `--tls`: usa TLS per la connessione al gateway
- `--tls-fingerprint <sha256>`: fingerprint attesa del certificato TLS (sha256)
- `--node-id <id>`: sovrascrive l’ID del nodo (cancella il token di abbinamento)
- `--display-name <name>`: sovrascrive il nome visualizzato del nodo

## Autenticazione Gateway per host Node

`openclaw node run` e `openclaw node install` risolvono l’autenticazione del gateway da config/env (nessun flag `--token`/`--password` nei comandi del nodo):

- `OPENCLAW_GATEWAY_TOKEN` / `OPENCLAW_GATEWAY_PASSWORD` vengono controllati per primi.
- Poi fallback alla configurazione locale: `gateway.auth.token` / `gateway.auth.password`.
- In modalità locale, l’host Node intenzionalmente non eredita `gateway.remote.token` / `gateway.remote.password`.
- Se `gateway.auth.token` / `gateway.auth.password` è configurato esplicitamente tramite SecretRef e non viene risolto, la risoluzione dell’autenticazione del nodo fallisce in modo chiuso (nessun fallback remoto a mascherarlo).
- In `gateway.mode=remote`, anche i campi del client remoto (`gateway.remote.token` / `gateway.remote.password`) sono idonei secondo le regole di precedenza remota.
- La risoluzione dell’autenticazione dell’host Node onora solo le variabili d’ambiente `OPENCLAW_GATEWAY_*`.

Per un nodo che si connette a un Gateway `ws://` non-loopback su una rete privata
fidata, imposta `OPENCLAW_ALLOW_INSECURE_PRIVATE_WS=1`. Senza questa impostazione, l’avvio del nodo
fallisce in modo chiuso e richiede di usare `wss://`, un tunnel SSH o Tailscale.
Si tratta di un opt-in dell’ambiente di processo, non di una chiave di configurazione in `openclaw.json`.
`openclaw node install` lo rende persistente nel servizio del nodo supervisionato quando è
presente nell’ambiente del comando di installazione.

## Servizio (background)

Installa un host Node headless come servizio utente.

```
openclaw node install --host <gateway-host> --port 18789
```

Opzioni:

- `--host <host>`: host WebSocket del Gateway (predefinito: `127.0.0.1`)
- `--port <port>`: porta WebSocket del Gateway (predefinito: `18789`)
- `--tls`: usa TLS per la connessione al gateway
- `--tls-fingerprint <sha256>`: fingerprint attesa del certificato TLS (sha256)
- `--node-id <id>`: sovrascrive l’ID del nodo (cancella il token di abbinamento)
- `--display-name <name>`: sovrascrive il nome visualizzato del nodo
- `--runtime <runtime>`: runtime del servizio (`node` o `bun`)
- `--force`: reinstalla/sovrascrive se già installato

Gestisci il servizio:

```
openclaw node status
openclaw node start
openclaw node stop
openclaw node restart
openclaw node uninstall
```

Usa `openclaw node run` per un host Node in foreground (senza servizio).I comandi del servizio accettano `--json` per output leggibile dalle macchine.L’host Node ritenta riavvio del Gateway e chiusure di rete all’interno del processo. Se il
Gateway segnala una pausa terminale di autenticazione token/password/bootstrap, l’host Node
registra il dettaglio della chiusura ed esce con codice diverso da zero così launchd/systemd può
riavviarlo con configurazione e credenziali aggiornate. Le pause che richiedono abbinamento restano nel flusso
foreground così la richiesta in sospeso può essere approvata.

## Abbinamento

La prima connessione crea una richiesta di abbinamento dispositivo in sospeso (`role: node`) sul Gateway.
Approvala tramite:

```
openclaw devices list
openclaw devices approve <requestId>
```

Su reti di nodi strettamente controllate, l’operatore del Gateway può scegliere esplicitamente
di approvare automaticamente il primo abbinamento del nodo da CIDR fidati:

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

Questa opzione è disabilitata per impostazione predefinita. Si applica solo ad abbinamenti iniziali `role: node` con
nessuno scope richiesto. Client operatore/browser, Control UI, WebChat e aggiornamenti di ruolo,
scope, metadati o chiave pubblica richiedono comunque approvazione manuale.Se il nodo ritenta l’abbinamento con dettagli di autenticazione cambiati (ruolo/scope/chiave pubblica),
la precedente richiesta in sospeso viene sostituita e viene creato un nuovo `requestId`.
Esegui di nuovo `openclaw devices list` prima dell’approvazione.L’host Node memorizza ID nodo, token, nome visualizzato e informazioni di connessione al gateway in
`~/.openclaw/node.json`.

## Approvazioni exec

`system.run` è protetto dalle approvazioni exec locali:

- `~/.openclaw/exec-approvals.json`
- [Approvazioni exec](https://docs.openclaw.ai/it/tools/exec-approvals)
- `openclaw approvals --node <id|name|ip>` (modifica dal Gateway)

Per exec asincrono del nodo approvato, OpenClaw prepara un `systemRunPlan` canonico
prima della richiesta di approvazione. Il successivo inoltro `system.run` approvato riutilizza quel piano
memorizzato, quindi le modifiche ai campi command/cwd/session dopo la creazione della richiesta di approvazione
vengono rifiutate invece di cambiare ciò che il nodo esegue.

## Correlati

- [Riferimento CLI](https://docs.openclaw.ai/it/cli)
- [Nodes](https://docs.openclaw.ai/it/nodes)

[Flussi (reindirizzamento)](https://docs.openclaw.ai/it/cli/flows) [Nodi](https://docs.openclaw.ai/it/cli/nodes)

Ctrl+I

---

## Flows(리디렉션) - OpenClaw

_Source: <https://docs.openclaw.ai/ko/cli/flows>_

# `openclaw tasks flow`

Flow 명령어는 독립적인 `flows` 명령어가 아니라 `openclaw tasks`의 하위 명령어입니다.

```
openclaw tasks flow list [--json]
openclaw tasks flow show <lookup>
openclaw tasks flow cancel <lookup>
```

전체 문서는 [TaskFlow](https://docs.openclaw.ai/ko/automation/taskflow) 및 [tasks CLI 참조](https://docs.openclaw.ai/ko/cli/tasks) 를 참조하세요.

## 관련 항목

- [CLI 참조](https://docs.openclaw.ai/ko/cli)
- [자동화](https://docs.openclaw.ai/ko/automation)

[Cron](https://docs.openclaw.ai/ko/cli/cron) [Node](https://docs.openclaw.ai/ko/cli/node)

Ctrl+I

---

## DNS - OpenClaw

_Source: <https://docs.openclaw.ai/pl/cli/dns>_

# `openclaw dns`

Narzędzia pomocnicze DNS do wykrywania w sieci rozległej (Tailscale + CoreDNS). Obecnie skupiają się na macOS + CoreDNS z Homebrew.Powiązane:

- Wykrywanie Gateway: [Discovery](https://docs.openclaw.ai/pl/gateway/discovery)
- Konfiguracja wykrywania w sieci rozległej: [Configuration](https://docs.openclaw.ai/pl/gateway/configuration)

## Konfiguracja

```
openclaw dns setup
openclaw dns setup --domain openclaw.internal
openclaw dns setup --apply
```

## `dns setup`

Zaplanuj lub zastosuj konfigurację CoreDNS dla wykrywania unicast DNS-SD.Opcje:

- `--domain <domain>`: domena wykrywania w sieci rozległej (na przykład `openclaw.internal`)
- `--apply`: zainstaluj lub zaktualizuj konfigurację CoreDNS i uruchom ponownie usługę (wymaga sudo; tylko macOS)

Co pokazuje:

- rozwiązaną domenę wykrywania
- ścieżkę pliku strefy
- bieżące adresy IP tailnet
- zalecaną konfigurację wykrywania `openclaw.json`
- wartości serwera nazw/domeny Tailscale Split DNS do ustawienia

Uwagi:

- Bez `--apply` polecenie jest tylko narzędziem pomocniczym do planowania i wypisuje zalecaną konfigurację.
- Jeśli pominięto `--domain`, OpenClaw używa `discovery.wideArea.domain` z konfiguracji.
- `--apply` obecnie obsługuje tylko macOS i zakłada CoreDNS z Homebrew.
- `--apply` inicjalizuje plik strefy, jeśli to konieczne, zapewnia istnienie sekcji importu CoreDNS i uruchamia ponownie usługę `coredns` z brew.

## Powiązane

- [Dokumentacja referencyjna CLI](https://docs.openclaw.ai/pl/cli)
- [Discovery](https://docs.openclaw.ai/pl/gateway/discovery)

[Autouzupełnianie](https://docs.openclaw.ai/pl/cli/completion) [Dokumentacja](https://docs.openclaw.ai/pl/cli/docs)

Ctrl+I

---

## Node - OpenClaw

_Source: <https://docs.openclaw.ai/pl/cli/node>_

# `openclaw node`

Uruchamia **bezgłowego hosta Node**, który łączy się z Gateway WebSocket i udostępnia
na tej maszynie `system.run` / `system.which`.

## Dlaczego warto używać hosta Node?

Użyj hosta Node, gdy chcesz, aby agenci **uruchamiali polecenia na innych maszynach**
w Twojej sieci bez instalowania na nich pełnej aplikacji towarzyszącej dla macOS.Typowe przypadki użycia:

- Uruchamianie poleceń na zdalnych maszynach Linux/Windows (serwery buildów, maszyny laboratoryjne, NAS).
- Utrzymanie **sandboxingu** exec na Gateway, ale delegowanie zatwierdzonych uruchomień do innych hostów.
- Zapewnienie lekkiego, bezgłowego celu wykonawczego dla automatyzacji lub węzłów CI.

Wykonanie nadal jest chronione przez **zatwierdzenia exec** i listy dozwolonych per agent
na hoście Node, dzięki czemu możesz zachować zakres dostępu do poleceń jako ograniczony i jawny.

## Proxy przeglądarki (zero-config)

Hosty Node automatycznie anonsują proxy przeglądarki, jeśli `browser.enabled` nie jest
wyłączone na węźle. Pozwala to agentowi używać automatyzacji przeglądarki na tym węźle
bez dodatkowej konfiguracji.Domyślnie proxy udostępnia zwykłą powierzchnię profilu przeglądarki węzła. Jeśli
ustawisz `nodeHost.browserProxy.allowProfiles`, proxy staje się restrykcyjne:
adresowanie profili spoza listy dozwolonych jest odrzucane, a trasy tworzenia/usuwania
trwałych profili są blokowane przez proxy.W razie potrzeby wyłącz je na węźle:

```
{
  nodeHost: {
    browserProxy: {
      enabled: false,
    },
  },
}
```

## Uruchomienie (na pierwszym planie)

```
openclaw node run --host <gateway-host> --port 18789
```

Opcje:

- `--host <host>`: host Gateway WebSocket (domyślnie: `127.0.0.1`)
- `--port <port>`: port Gateway WebSocket (domyślnie: `18789`)
- `--tls`: używa TLS dla połączenia z Gateway
- `--tls-fingerprint <sha256>`: oczekiwany fingerprint certyfikatu TLS (sha256)
- `--node-id <id>`: nadpisuje identyfikator Node (czyści token Pairing)
- `--display-name <name>`: nadpisuje wyświetlaną nazwę węzła

## Uwierzytelnianie Gateway dla hosta Node

`openclaw node run` i `openclaw node install` rozwiązują uwierzytelnianie Gateway z config/env (polecenia node nie mają flag `--token`/`--password`):

- Najpierw sprawdzane są `OPENCLAW_GATEWAY_TOKEN` / `OPENCLAW_GATEWAY_PASSWORD`.
- Następnie fallback do lokalnej konfiguracji: `gateway.auth.token` / `gateway.auth.password`.
- W trybie lokalnym host Node celowo nie dziedziczy `gateway.remote.token` / `gateway.remote.password`.
- Jeśli `gateway.auth.token` / `gateway.auth.password` są jawnie skonfigurowane przez SecretRef i nierozwiązane, rozwiązywanie uwierzytelniania Node kończy się w trybie fail-closed (bez maskującego fallbacku z trybu zdalnego).
- W `gateway.mode=remote` pola klienta zdalnego (`gateway.remote.token` / `gateway.remote.password`) także kwalifikują się zgodnie z regułami priorytetu trybu zdalnego.
- Rozwiązywanie uwierzytelniania hosta Node honoruje tylko zmienne środowiskowe `OPENCLAW_GATEWAY_*`.

Dla węzła łączącego się z Gateway `ws://` innym niż local loopback w zaufanej sieci
prywatnej ustaw `OPENCLAW_ALLOW_INSECURE_PRIVATE_WS=1`. Bez tego uruchomienie węzła
kończy się w trybie fail-closed i prosi o użycie `wss://`, tunelu SSH lub Tailscale.
Jest to opt-in na poziomie środowiska procesu, a nie klucz konfiguracji `openclaw.json`.
`openclaw node install` utrwala tę wartość w nadzorowanej usłudze węzła, gdy jest ona
obecna w środowisku polecenia instalacji.

## Usługa (w tle)

Zainstaluj bezgłowego hosta Node jako usługę użytkownika.

```
openclaw node install --host <gateway-host> --port 18789
```

Opcje:

- `--host <host>`: host Gateway WebSocket (domyślnie: `127.0.0.1`)
- `--port <port>`: port Gateway WebSocket (domyślnie: `18789`)
- `--tls`: używa TLS dla połączenia z Gateway
- `--tls-fingerprint <sha256>`: oczekiwany fingerprint certyfikatu TLS (sha256)
- `--node-id <id>`: nadpisuje identyfikator Node (czyści token Pairing)
- `--display-name <name>`: nadpisuje wyświetlaną nazwę węzła
- `--runtime <runtime>`: runtime usługi (`node` lub `bun`)
- `--force`: reinstaluje/nadpisuje, jeśli już zainstalowane

Zarządzanie usługą:

```
openclaw node status
openclaw node start
openclaw node stop
openclaw node restart
openclaw node uninstall
```

Użyj `openclaw node run` dla hosta Node na pierwszym planie (bez usługi).Polecenia usługi akceptują `--json` dla danych wyjściowych czytelnych maszynowo.Host Node ponawia restart Gateway i zamknięcia sieci w obrębie procesu. Jeśli
Gateway zgłosi terminalną pauzę uwierzytelniania tokenu/hasła/bootstrap, host Node
loguje szczegóły zamknięcia i kończy się niezerowym kodem, aby launchd/systemd mogły
uruchomić go ponownie ze świeżą konfiguracją i poświadczeniami. Pauzy wymagające Pairing
pozostają w przepływie na pierwszym planie, aby oczekujące żądanie mogło zostać zatwierdzone.

## Pairing

Pierwsze połączenie tworzy w Gateway oczekujące żądanie Pairing urządzenia (`role: node`).
Zatwierdź je przez:

```
openclaw devices list
openclaw devices approve <requestId>
```

W ściśle kontrolowanych sieciach węzłów operator Gateway może jawnie włączyć
automatyczne zatwierdzanie pierwszego Pairing węzła z zaufanych zakresów CIDR:

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

To ustawienie jest domyślnie wyłączone. Dotyczy tylko świeżego Pairing `role: node` bez
żądanych zakresów. Klienci operator/browser, Control UI, WebChat oraz aktualizacje roli,
zakresów, metadanych lub klucza publicznego nadal wymagają ręcznego zatwierdzenia.Jeśli węzeł ponowi próbę Pairing ze zmienionymi szczegółami uwierzytelniania
(rola/zakresy/klucz publiczny), poprzednie oczekujące żądanie zostaje zastąpione,
a tworzony jest nowy `requestId`.
Uruchom ponownie `openclaw devices list` przed zatwierdzeniem.Host Node przechowuje identyfikator Node, token, wyświetlaną nazwę i informacje o połączeniu z Gateway w
`~/.openclaw/node.json`.

## Zatwierdzenia exec

`system.run` jest chronione przez lokalne zatwierdzenia exec:

- `~/.openclaw/exec-approvals.json`
- [Exec approvals](https://docs.openclaw.ai/pl/tools/exec-approvals)
- `openclaw approvals --node <id|name|ip>` (edycja z Gateway)

Dla zatwierdzonego asynchronicznego exec na Node OpenClaw przygotowuje kanoniczny
`systemRunPlan` przed wyświetleniem promptu. Późniejsze zatwierdzone przekazanie
`system.run` ponownie używa tego zapisanego planu, więc edycje pól command/cwd/session
po utworzeniu żądania zatwierdzenia są odrzucane zamiast zmieniać to, co wykona węzeł.

## Powiązane

- [Dokumentacja CLI](https://docs.openclaw.ai/pl/cli)
- [Nodes](https://docs.openclaw.ai/pl/nodes)

[Flows (przekierowanie)](https://docs.openclaw.ai/pl/cli/flows) [Węzły](https://docs.openclaw.ai/pl/cli/nodes)

Ctrl+I

---

## `openclaw tasks` - OpenClaw

_Source: <https://docs.openclaw.ai/pl/cli/tasks>_

[Przejdź do głównej treści](https://docs.openclaw.ai/pl/cli/tasks#content-area)

[OpenClaw home page](https://docs.openclaw.ai/pl)

Polski

Szukaj...

Szukaj...

Agents and sessions

\`openclaw tasks\`

[Get started](https://docs.openclaw.ai/pl) [Install](https://docs.openclaw.ai/pl/install) [Channels](https://docs.openclaw.ai/pl/channels) [Agents](https://docs.openclaw.ai/pl/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pl/tools) [Models](https://docs.openclaw.ai/pl/providers) [Platforms](https://docs.openclaw.ai/pl/platforms) [Gateway & Ops](https://docs.openclaw.ai/pl/gateway) [Reference](https://docs.openclaw.ai/pl/cli) [Help](https://docs.openclaw.ai/pl/help)

Na tej stronie

- [Użycie](https://docs.openclaw.ai/pl/cli/tasks#u%C5%BCycie)
- [Opcje główne](https://docs.openclaw.ai/pl/cli/tasks#opcje-g%C5%82%C3%B3wne)
- [Podpolecenia](https://docs.openclaw.ai/pl/cli/tasks#podpolecenia)
- [list](https://docs.openclaw.ai/pl/cli/tasks#list)
- [show](https://docs.openclaw.ai/pl/cli/tasks#show)
- [notify](https://docs.openclaw.ai/pl/cli/tasks#notify)
- [cancel](https://docs.openclaw.ai/pl/cli/tasks#cancel)
- [audit](https://docs.openclaw.ai/pl/cli/tasks#audit)
- [maintenance](https://docs.openclaw.ai/pl/cli/tasks#maintenance)
- [flow](https://docs.openclaw.ai/pl/cli/tasks#flow)
- [Powiązane](https://docs.openclaw.ai/pl/cli/tasks#powi%C4%85zane)

Sprawdzaj trwałe zadania w tle i stan TaskFlow. Bez podpolecenia
`openclaw tasks` jest równoważne z `openclaw tasks list`.Zobacz [Zadania w tle](https://docs.openclaw.ai/pl/automation/tasks), aby poznać model cyklu życia i dostarczania.

## Użycie

```
openclaw tasks
openclaw tasks list
openclaw tasks list --runtime acp
openclaw tasks list --status running
openclaw tasks show <lookup>
openclaw tasks notify <lookup> state_changes
openclaw tasks cancel <lookup>
openclaw tasks audit
openclaw tasks maintenance
openclaw tasks maintenance --apply
openclaw tasks flow list
openclaw tasks flow show <lookup>
openclaw tasks flow cancel <lookup>
```

## Opcje główne

- `--json`: wyjście JSON.
- `--runtime <name>`: filtruj według rodzaju: `subagent`, `acp`, `cron` lub `cli`.
- `--status <name>`: filtruj według statusu: `queued`, `running`, `succeeded`, `failed`, `timed_out`, `cancelled` lub `lost`.

## Podpolecenia

### `list`

```
openclaw tasks list [--runtime <name>] [--status <name>] [--json]
```

Wyświetla śledzone zadania w tle od najnowszych.

### `show`

```
openclaw tasks show <lookup> [--json]
```

Pokazuje jedno zadanie według identyfikatora zadania, identyfikatora uruchomienia lub klucza sesji.

### `notify`

```
openclaw tasks notify <lookup> <done_only|state_changes|silent>
```

Zmienia politykę powiadomień dla uruchomionego zadania.

### `cancel`

```
openclaw tasks cancel <lookup>
```

Anuluje uruchomione zadanie w tle.

### `audit`

```
openclaw tasks audit [--severity <warn|error>] [--code <name>] [--limit <n>] [--json]
```

Ujawnia nieaktualne, utracone, z błędami dostarczania lub w inny sposób niespójne rekordy zadań i TaskFlow. Utracone zadania zachowane do czasu `cleanupAfter` są ostrzeżeniami; wygasłe lub nieoznaczone utracone zadania są błędami.

### `maintenance`

```
openclaw tasks maintenance [--apply] [--json]
```

Wyświetla podgląd lub stosuje uzgadnianie zadań i TaskFlow, oznaczanie czyszczenia oraz przycinanie.
W przypadku zadań Cron uzgadnianie używa utrwalonych logów uruchomień/stanu zadań przed oznaczeniem
starego aktywnego zadania jako `lost`, dzięki czemu ukończone uruchomienia Cron nie stają się fałszywymi błędami audytu
tylko dlatego, że zniknął stan działania Gateway przechowywany w pamięci. Audyt CLI offline
nie jest autorytatywny dla lokalnego w procesie zestawu aktywnych zadań Cron Gateway.

### `flow`

```
openclaw tasks flow list [--status <name>] [--json]
openclaw tasks flow show <lookup> [--json]
openclaw tasks flow cancel <lookup>
```

Sprawdza lub anuluje trwały stan TaskFlow w rejestrze zadań.

## Powiązane

- [Dokumentacja CLI](https://docs.openclaw.ai/pl/cli)
- [Zadania w tle](https://docs.openclaw.ai/pl/automation/tasks)

[System](https://docs.openclaw.ai/pl/cli/system) [Kanały](https://docs.openclaw.ai/pl/cli/channels)

Ctrl+I

---
