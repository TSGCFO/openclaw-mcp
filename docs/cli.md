---
source_url: https://docs.openclaw.ai/cli
title: "CLI reference - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

CLI commands

CLI reference

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Command pages](https://docs.openclaw.ai/cli#command-pages)
- [Global flags](https://docs.openclaw.ai/cli#global-flags)
- [Output modes](https://docs.openclaw.ai/cli#output-modes)
- [Command tree](https://docs.openclaw.ai/cli#command-tree)
- [Chat slash commands](https://docs.openclaw.ai/cli#chat-slash-commands)
- [Usage tracking](https://docs.openclaw.ai/cli#usage-tracking)
- [Related](https://docs.openclaw.ai/cli#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

`openclaw` is the main CLI entry point. Each core command has either a
dedicated reference page or is documented with the command it aliases; this
index lists the commands, the global flags, and the output styling rules that
apply across the CLI.

## [​](https://docs.openclaw.ai/cli\#command-pages)  Command pages

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

## [​](https://docs.openclaw.ai/cli\#global-flags)  Global flags

| Flag | Purpose |
| --- | --- |
| `--dev` | Isolate state under `~/.openclaw-dev` and shift default ports |
| `--profile <name>` | Isolate state under `~/.openclaw-<name>` |
| `--container <name>` | Target a named container for execution |
| `--no-color` | Disable ANSI colors (`NO_COLOR=1` is also respected) |
| `--update` | Shorthand for [`openclaw update`](https://docs.openclaw.ai/cli/update) (source installs only) |
| `-V`, `--version`, `-v` | Print version and exit |

## [​](https://docs.openclaw.ai/cli\#output-modes)  Output modes

- ANSI colors and progress indicators render only in TTY sessions.
- OSC-8 hyperlinks render as clickable links where supported; otherwise the
CLI falls back to plain URLs.
- `--json` (and `--plain` where supported) disables styling for clean output.
- Long-running commands show a progress indicator (OSC 9;4 when supported).

Palette source of truth: `src/terminal/palette.ts`.

## [​](https://docs.openclaw.ai/cli\#command-tree)  Command tree

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

## [​](https://docs.openclaw.ai/cli\#chat-slash-commands)  Chat slash commands

Chat messages support `/...` commands. See [slash commands](https://docs.openclaw.ai/tools/slash-commands).Highlights:

- `/status` — quick diagnostics.
- `/trace` — session-scoped plugin trace/debug lines.
- `/config` — persisted config changes.
- `/debug` — runtime-only config overrides (memory, not disk; requires `commands.debug: true`).

## [​](https://docs.openclaw.ai/cli\#usage-tracking)  Usage tracking

`openclaw status --usage` and the Control UI surface provider usage/quota when
OAuth/API credentials are available. Data comes directly from provider usage
endpoints and is normalized to `X% left`. Providers with current usage
windows: Anthropic, GitHub Copilot, Gemini CLI, OpenAI Codex, MiniMax,
Xiaomi, and z.ai.See [Usage tracking](https://docs.openclaw.ai/concepts/usage-tracking) for details.

## [​](https://docs.openclaw.ai/cli\#related)  Related

- [Slash commands](https://docs.openclaw.ai/tools/slash-commands)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Environment](https://docs.openclaw.ai/help/environment)

[Backup](https://docs.openclaw.ai/cli/backup)

Ctrl+I