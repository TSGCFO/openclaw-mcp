# Openclaw-Mcp_Docs - Cli

**Pages:** 37

---

## Health

**URL:** https://docs.openclaw.ai/cli/health

**Contents:**
- Health
- Documentation Index
- ​openclaw health
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw health
openclaw health --json
openclaw health --timeout 2500
openclaw health --verbose
openclaw health --debug
```

---

## Update

**URL:** https://docs.openclaw.ai/cli/update

**Contents:**
- Update
- Documentation Index
- ​openclaw update
- ​Usage
- ​Options
- ​update status
- ​update wizard
- ​What it does
- ​Git checkout flow
  - ​Channel selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify clean worktree

Preflight build (dev only)

**Examples:**

Example 1 (sql):
```sql
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

Example 2 (sql):
```sql
openclaw update status
openclaw update status --json
openclaw update status --timeout 10
```

---

## Proxy

**URL:** https://docs.openclaw.ai/cli/proxy

**Contents:**
- Proxy
- Documentation Index
- ​openclaw proxy
- ​Commands
- ​Validate
- ​Query presets
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
openclaw proxy start [--host <host>] [--port <port>]
openclaw proxy run [--host <host>] [--port <port>] -- <cmd...>
openclaw proxy validate [--json] [--proxy-url <url>] [--allowed-url <url>] [--denied-url <url>] [--timeout-ms <ms>]
openclaw proxy coverage
openclaw proxy sessions [--limit <count>]
openclaw proxy query --preset <name> [--session <id>]
openclaw proxy blob --id <blobId>
openclaw proxy purge
```

---

## Configure

**URL:** https://docs.openclaw.ai/cli/configure

**Contents:**
- Configure
- Documentation Index
- ​openclaw configure
- ​Options
- ​Examples
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw configure
openclaw configure --section web
openclaw configure --section model --section channels
openclaw configure --section gateway --section daemon
```

---

## Setup

**URL:** https://docs.openclaw.ai/cli/setup

**Contents:**
- Setup
- Documentation Index
- ​openclaw setup
- ​Examples
- ​Options
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
openclaw setup
openclaw setup --workspace ~/.openclaw/workspace
openclaw setup --wizard
openclaw setup --wizard --import-from hermes --import-source ~/.hermes
openclaw setup --non-interactive --mode remote --remote-url wss://gateway-host:18789 --remote-token <token>
```

Example 2 (unknown):
```unknown
openclaw setup --wizard
```

---

## Node

**URL:** https://docs.openclaw.ai/cli/node

**Contents:**
- Node
- Documentation Index
- ​openclaw node
- ​Why use a node host?
- ​Browser proxy (zero-config)
- ​Run (foreground)
- ​Gateway auth for node host
- ​Service (background)
- ​Pairing
- ​Exec approvals

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  nodeHost: {
    browserProxy: {
      enabled: false,
    },
  },
}
```

Example 2 (unknown):
```unknown
openclaw node run --host <gateway-host> --port 18789
```

Example 3 (unknown):
```unknown
openclaw node install --host <gateway-host> --port 18789
```

Example 4 (unknown):
```unknown
openclaw node status
openclaw node start
openclaw node stop
openclaw node restart
openclaw node uninstall
```

---

## Sandbox CLI

**URL:** https://docs.openclaw.ai/cli/sandbox

**Contents:**
- Sandbox CLI
- Documentation Index
- ​Overview
- ​Commands
  - ​openclaw sandbox explain
  - ​openclaw sandbox list
  - ​openclaw sandbox recreate
- ​Use cases
  - ​After updating a Docker image
  - ​After changing sandbox configuration

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw sandbox explain
openclaw sandbox explain --session agent:main:main
openclaw sandbox explain --agent work
openclaw sandbox explain --json
```

Example 2 (unknown):
```unknown
openclaw sandbox list
openclaw sandbox list --browser  # List only browser containers
openclaw sandbox list --json     # JSON output
```

Example 3 (unknown):
```unknown
openclaw sandbox recreate --all                # Recreate all containers
openclaw sandbox recreate --session main       # Specific session
openclaw sandbox recreate --agent mybot        # Specific agent
openclaw sandbox recreate --browser            # Only browser containers
openclaw sandbox recreate --all --force        # Skip confirmation
```

Example 4 (elixir):
```elixir
# Pull new image
docker pull openclaw-sandbox:latest
docker tag openclaw-sandbox:latest openclaw-sandbox:bookworm-slim

# Update config to use new image
# Edit config: agents.defaults.sandbox.docker.image (or agents.list[].sandbox.docker.image)

# Recreate containers
openclaw sandbox recreate --all
```

---

## Devices

**URL:** https://docs.openclaw.ai/cli/devices

**Contents:**
- Devices
- Documentation Index
- ​openclaw devices
- ​Commands
  - ​openclaw devices list
  - ​openclaw devices remove <deviceId>
  - ​openclaw devices clear --yes [--pending]
  - ​openclaw devices approve [requestId] [--latest]
  - ​openclaw devices reject <requestId>
  - ​openclaw devices rotate --device <id> --role <role> [--scope <scope...>]

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw devices list
openclaw devices list --json
```

Example 2 (typescript):
```typescript
openclaw devices remove <deviceId>
openclaw devices remove <deviceId> --json
```

Example 3 (unknown):
```unknown
openclaw devices clear --yes
openclaw devices clear --yes --pending
openclaw devices clear --yes --pending --json
```

Example 4 (typescript):
```typescript
openclaw devices approve
openclaw devices approve <requestId>
openclaw devices approve --latest
```

---

## Directory

**URL:** https://docs.openclaw.ai/cli/directory

**Contents:**
- Directory
- Documentation Index
- ​openclaw directory
- ​Common flags
- ​Notes
- ​Using results with message send
- ​ID formats (by channel)
- ​Self (“me”)
- ​Peers (contacts/users)
- ​Groups

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw directory peers list --channel slack --query "U0"
openclaw message send --channel slack --target user:U012ABCDEF --message "hello"
```

Example 2 (swift):
```swift
openclaw directory self --channel zalouser
```

Example 3 (unknown):
```unknown
openclaw directory peers list --channel zalouser
openclaw directory peers list --channel zalouser --query "name"
openclaw directory peers list --channel zalouser --limit 50
```

Example 4 (typescript):
```typescript
openclaw directory groups list --channel zalouser
openclaw directory groups list --channel zalouser --query "work"
openclaw directory groups members --channel zalouser --group-id <id>
```

---

## Wiki

**URL:** https://docs.openclaw.ai/cli/wiki

**Contents:**
- Wiki
- Documentation Index
- ​openclaw wiki
- ​What it is for
- ​Common commands
- ​Commands
  - ​wiki status
  - ​wiki doctor
  - ​wiki init
  - ​wiki ingest <path-or-url>

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
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

Example 2 (unknown):
```unknown
openclaw wiki search "bgroux" --mode find-person
openclaw wiki search "who knows Teams rollout?" --mode route-question
openclaw wiki search "maintainer-whois" --mode source-evidence
openclaw wiki search "strong route Teams" --mode raw-claim --json
```

Example 3 (sql):
```sql
openclaw wiki get entity.alpha
openclaw wiki get syntheses/alpha-summary.md --from 1 --lines 80
```

---

## Dashboard

**URL:** https://docs.openclaw.ai/cli/dashboard

**Contents:**
- Dashboard
- Documentation Index
- ​openclaw dashboard
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw dashboard
openclaw dashboard --no-open
```

---

## System

**URL:** https://docs.openclaw.ai/cli/system

**Contents:**
- System
- Documentation Index
- ​openclaw system
- ​Common commands
- ​system event
- ​system heartbeat last|enable|disable
- ​system presence
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
openclaw system event --text "Check for urgent follow-ups" --mode now
openclaw system event --text "Check for urgent follow-ups" --url ws://127.0.0.1:18789 --token "$OPENCLAW_GATEWAY_TOKEN"
openclaw system heartbeat enable
openclaw system heartbeat last
openclaw system presence
```

---

## Message

**URL:** https://docs.openclaw.ai/cli/message

**Contents:**
- Message
- Documentation Index
- ​openclaw message
- ​Usage
- ​Common flags
- ​SecretRef behavior
- ​Actions
  - ​Core
  - ​Threads
  - ​Emojis

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw message <subcommand> [flags]
```

Example 2 (json):
```json
openclaw message send --channel discord \
  --target channel:123 --message "hi" --reply-to 456
```

Example 3 (json):
```json
openclaw message send --channel discord \
  --target channel:123 --message "Choose:" \
  --presentation '{"blocks":[{"type":"buttons","buttons":[{"label":"Approve","value":"approve","style":"success"},{"label":"Decline","value":"decline","style":"danger"}]}]}'
```

Example 4 (lua):
```lua
openclaw message send --channel googlechat --target spaces/AAA... \
  --message "Choose:" \
  --presentation '{"title":"Deploy approval","tone":"warning","blocks":[{"type":"text","text":"Choose a path"},{"type":"buttons","buttons":[{"label":"Approve","value":"approve"},{"label":"Decline","value":"decline"}]}]}'
```

---

## CLI reference

**URL:** https://docs.openclaw.ai/cli

**Contents:**
- CLI reference
- Documentation Index
- ​Command pages
- ​Global flags
- ​Output modes
- ​Command tree
- ​Chat slash commands
- ​Usage tracking
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
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

---

## Flows (redirect)

**URL:** https://docs.openclaw.ai/cli/flows

**Contents:**
- Flows (redirect)
- Documentation Index
- ​openclaw tasks flow
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw tasks flow list [--json]
openclaw tasks flow show <lookup>
openclaw tasks flow cancel <lookup>
```

---

## Crestodian

**URL:** https://docs.openclaw.ai/cli/crestodian

**Contents:**
- Crestodian
- Documentation Index
- ​openclaw crestodian
- ​What Crestodian shows
- ​Examples
- ​Safe startup
- ​Operations and approval
- ​Setup bootstrap
- ​Model-Assisted Planner
- ​Switching to an agent

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw
openclaw crestodian
openclaw crestodian --json
openclaw crestodian --message "models"
openclaw crestodian --message "validate config"
openclaw crestodian --message "setup workspace ~/Projects/work model openai/gpt-5.5" --yes
openclaw crestodian --message "set default model openai/gpt-5.5" --yes
openclaw onboard --modern
```

Example 2 (powershell):
```powershell
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

Example 3 (unknown):
```unknown
~/.openclaw/audit/crestodian.jsonl
```

Example 4 (unknown):
```unknown
setup
setup workspace ~/Projects/work
setup workspace ~/Projects/work model openai/gpt-5.5
```

---

## Sessions

**URL:** https://docs.openclaw.ai/cli/sessions

**Contents:**
- Sessions
- Documentation Index
- ​openclaw sessions
- ​Cleanup maintenance
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw sessions
openclaw sessions --agent work
openclaw sessions --all-agents
openclaw sessions --active 120
openclaw sessions --verbose
openclaw sessions --json
```

Example 2 (json):
```json
openclaw sessions export-trajectory --session-key "agent:main:telegram:direct:123" --workspace .
openclaw sessions export-trajectory --session-key "agent:main:telegram:direct:123" --output bug-123 --json
```

Example 3 (json):
```json
{
  "path": null,
  "stores": [
    { "agentId": "main", "path": "/home/user/.openclaw/agents/main/sessions/sessions.json" },
    { "agentId": "work", "path": "/home/user/.openclaw/agents/work/sessions/sessions.json" }
  ],
  "allAgents": true,
  "count": 2,
  "activeMinutes": null,
  "sessions": [
    { "agentId": "main", "key": "agent:main:main", "model": "gpt-5" },
    { "agentId": "work", "key": "agent:work:main", "model": "claude-opus-4-6" }
  ]
}
```

Example 4 (json):
```json
openclaw sessions cleanup --dry-run
openclaw sessions cleanup --agent work --dry-run
openclaw sessions cleanup --all-agents --dry-run
openclaw sessions cleanup --enforce
openclaw sessions cleanup --enforce --active-key "agent:main:telegram:direct:123"
openclaw sessions cleanup --json
```

---

## Memory

**URL:** https://docs.openclaw.ai/cli/memory

**Contents:**
- Memory
- Documentation Index
- ​openclaw memory
- ​Examples
- ​Options
- ​Dreaming
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
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

Example 2 (typescript):
```typescript
openclaw memory promote [--apply] [--limit <n>] [--include-promoted]
```

Example 3 (typescript):
```typescript
openclaw memory promote-explain <selector> [--agent <id>] [--include-promoted] [--json]
```

Example 4 (typescript):
```typescript
openclaw memory rem-harness [--agent <id>] [--include-promoted] [--json]
```

---

## `openclaw tasks`

**URL:** https://docs.openclaw.ai/cli/tasks

**Contents:**
- `openclaw tasks`
- Documentation Index
- ​Usage
- ​Root Options
- ​Subcommands
  - ​list
  - ​show
  - ​notify
  - ​cancel
  - ​audit

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
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

Example 2 (typescript):
```typescript
openclaw tasks list [--runtime <name>] [--status <name>] [--json]
```

Example 3 (typescript):
```typescript
openclaw tasks show <lookup> [--json]
```

Example 4 (typescript):
```typescript
openclaw tasks notify <lookup> <done_only|state_changes|silent>
```

---

## DNS

**URL:** https://docs.openclaw.ai/cli/dns

**Contents:**
- DNS
- Documentation Index
- ​openclaw dns
- ​Setup
- ​dns setup
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (swift):
```swift
openclaw dns setup
openclaw dns setup --domain openclaw.internal
openclaw dns setup --apply
```

---

## Config

**URL:** https://docs.openclaw.ai/cli/config

**Contents:**
- Config
- Documentation Index
- ​Root options
- ​Examples
  - ​config schema
  - ​Paths
- ​Values
- ​config set modes
- ​config patch
- ​Provider builder flags

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Env provider (--provider-source env)

File provider (--provider-source file)

Exec provider (--provider-source exec)

--dry-run --json fields

Doctor for runtime issues

**Examples:**

Example 1 (json):
```json
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

Example 2 (unknown):
```unknown
openclaw config schema
```

Example 3 (unknown):
```unknown
openclaw config schema > openclaw.schema.json
```

Example 4 (unknown):
```unknown
openclaw config get agents.defaults.workspace
openclaw config get agents.list[0].id
```

---

## Inference CLI

**URL:** https://docs.openclaw.ai/cli/infer

**Contents:**
- Inference CLI
- Documentation Index
- ​Turn infer into a skill
- ​Why use infer
- ​Command tree
- ​Common tasks
- ​Behavior
- ​Model
- ​Image
- ​Audio

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
Read https://docs.openclaw.ai/cli/infer, then create a skill that routes my common workflows to `openclaw infer`.
Focus on model runs, image generation, video generation, audio transcription, TTS, web search, and embeddings.
```

Example 2 (powershell):
```powershell
openclaw infer
  list
  inspect

  model
    run
    list
    inspect
    providers
    auth login
    auth logout
    auth status

  image
    generate
    edit
    describe
    describe-many
    providers

  audio
    transcribe
    providers

  tts
    convert
    voices
    providers
    status
    enable
    disable
    set-provider

  video
    generate
    describe
    providers

  web
    search
    fetch
    providers

  embedding
    create
    providers
```

Example 3 (unknown):
```unknown
openclaw infer model run --prompt "Reply with exactly: smoke-ok" --json
openclaw infer model run --prompt "Summarize this changelog entry" --model openai/gpt-5.4 --json
openclaw infer model run --prompt "Describe this image in one sentence" --file ./photo.jpg --model google/gemini-2.5-flash --json
openclaw infer model providers --json
openclaw infer model inspect --name gpt-5.5 --json
```

Example 4 (json):
```json
openclaw infer model run --local --model anthropic/claude-sonnet-4-6 --prompt "Reply with exactly: pong" --json
openclaw infer model run --local --model cerebras/zai-glm-4.7 --prompt "Reply with exactly: pong" --json
openclaw infer model run --local --model google/gemini-2.5-flash --prompt "Reply with exactly: pong" --json
openclaw infer model run --local --model groq/llama-3.1-8b-instant --prompt "Reply with exactly: pong" --json
openclaw infer model run --local --model mistral/mistral-small-latest --prompt "Reply with exactly: pong" --json
openclaw infer model run --local --model openai/gpt-4.1 --prompt "Reply with exactly: pong" --json
openclaw infer model run --local --model ollama/qwen2.5vl:7b --prompt "Describe this image." --file ./photo.jpg --json
```

---

## Nodes

**URL:** https://docs.openclaw.ai/cli/nodes

**Contents:**
- Nodes
- Documentation Index
- ​openclaw nodes
- ​Common commands
- ​Invoke
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
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

Example 2 (typescript):
```typescript
openclaw nodes invoke --node <id|name|ip> --command <command> --params <json>
```

---

## Backup

**URL:** https://docs.openclaw.ai/cli/backup

**Contents:**
- Backup
- Documentation Index
- ​openclaw backup
- ​Notes
- ​What gets backed up
- ​Invalid config behavior
- ​Size and performance
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw backup create
openclaw backup create --output ~/Backups
openclaw backup create --dry-run --json
openclaw backup create --verify
openclaw backup create --no-include-workspace
openclaw backup create --only-config
openclaw backup verify ./2026-03-09T00-00-00.000Z-openclaw-backup.tar.gz
```

Example 2 (unknown):
```unknown
openclaw backup create --no-include-workspace
```

---

## Migrate

**URL:** https://docs.openclaw.ai/cli/migrate

**Contents:**
- Migrate
- Documentation Index
- ​openclaw migrate
- ​Commands
- ​Safety model
- ​Claude provider
  - ​What Claude imports
  - ​Archive and manual-review state
- ​Codex provider
  - ​What Codex imports

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
openclaw migrate list
openclaw migrate claude --dry-run
openclaw migrate codex --dry-run
openclaw migrate codex --skill gog-vault77-google-workspace
openclaw migrate hermes --dry-run
openclaw migrate hermes
openclaw migrate apply codex --yes --skill gog-vault77-google-workspace
openclaw migrate apply codex --yes
openclaw migrate apply claude --yes
openclaw migrate apply hermes --yes
openclaw migrate apply hermes --include-secrets --yes
openclaw onboard --flow import
openclaw onboard --import-from claude --import-source ~/.claude
openclaw onboard --import-from hermes --import-source ~/.hermes
```

Example 2 (unknown):
```unknown
openclaw migrate codex --dry-run --skill gog-vault77-google-workspace
openclaw migrate apply codex --yes --skill gog-vault77-google-workspace
```

Example 3 (unknown):
```unknown
openclaw doctor
```

Example 4 (json):
```json
{
  "contracts": {
    "migrationProviders": ["hermes"]
  }
}
```

---

## MCP

**URL:** https://docs.openclaw.ai/cli/mcp

**Contents:**
- MCP
- Documentation Index
- ​OpenClaw as an MCP server
  - ​When to use serve
  - ​How it works
  - ​Choose a client mode
  - ​What serve exposes
  - ​Usage
  - ​Bridge tools
  - ​Event model

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Client spawns the bridge

Bridge connects to Gateway

Sessions become MCP conversations

permissions_list_open

No conversations returned

events_poll or events_wait misses older messages

Claude notifications do not show up

Approvals are missing

**Examples:**

Example 1 (unknown):
```unknown
openclaw mcp serve
```

Example 2 (json):
```json
openclaw mcp serve --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token
```

Example 3 (json):
```json
openclaw mcp serve --url wss://gateway-host:18789 --password-file ~/.openclaw/gateway.password
```

Example 4 (unknown):
```unknown
openclaw mcp serve --verbose
openclaw mcp serve --claude-channel-mode off
```

---

## TUI

**URL:** https://docs.openclaw.ai/cli/tui

**Contents:**
- TUI
- Documentation Index
- ​openclaw tui
- ​Examples
- ​Config repair loop
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
openclaw chat
openclaw tui --local
openclaw tui
openclaw tui --url ws://127.0.0.1:18789 --token <token>
openclaw tui --session main --deliver
openclaw chat --message "Compare my config to the docs and tell me what to fix"
# when run inside an agent workspace, infers that agent automatically
openclaw tui --session bugfix
```

Example 2 (unknown):
```unknown
openclaw chat
```

Example 3 (unknown):
```unknown
!openclaw config file
!openclaw docs gateway auth token secretref
!openclaw config validate
!openclaw doctor
```

---

## Onboard

**URL:** https://docs.openclaw.ai/cli/onboard

**Contents:**
- Onboard
- Documentation Index
- ​openclaw onboard
- ​Related guides
- CLI onboarding hub
- Onboarding overview
- CLI setup reference
- CLI automation
- macOS app onboarding
- ​Examples

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Provider prefiltering

Web-search follow-ups

**Examples:**

Example 1 (sql):
```sql
openclaw onboard
openclaw onboard --modern
openclaw onboard --flow quickstart
openclaw onboard --flow manual
openclaw onboard --flow import
openclaw onboard --import-from hermes --import-source ~/.hermes
openclaw onboard --skip-bootstrap
openclaw onboard --mode remote --remote-url wss://gateway-host:18789
```

Example 2 (bash):
```bash
openclaw onboard --non-interactive \
  --auth-choice custom-api-key \
  --custom-base-url "https://llm.example.com/v1" \
  --custom-model-id "foo-large" \
  --custom-api-key "$CUSTOM_API_KEY" \
  --secret-input-mode plaintext \
  --custom-compatibility openai \
  --custom-image-input
```

Example 3 (json):
```json
openclaw onboard --non-interactive \
  --auth-choice lmstudio \
  --custom-base-url "http://localhost:1234/v1" \
  --custom-model-id "qwen/qwen3.5-9b" \
  --lmstudio-api-key "$LM_API_TOKEN" \
  --accept-risk
```

Example 4 (json):
```json
openclaw onboard --non-interactive \
  --auth-choice ollama \
  --custom-base-url "http://ollama-host:11434" \
  --custom-model-id "qwen3.5:27b" \
  --accept-risk
```

---

## QR

**URL:** https://docs.openclaw.ai/cli/qr

**Contents:**
- QR
- Documentation Index
- ​openclaw qr
- ​Usage
- ​Options
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw qr
openclaw qr --setup-code-only
openclaw qr --json
openclaw qr --remote
openclaw qr --url wss://gateway.example/ws
```

---

## Daemon

**URL:** https://docs.openclaw.ai/cli/daemon

**Contents:**
- Daemon
- Documentation Index
- ​openclaw daemon
- ​Usage
- ​Subcommands
- ​Common options
- ​Prefer
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw daemon status
openclaw daemon install
openclaw daemon start
openclaw daemon stop
openclaw daemon restart
openclaw daemon uninstall
```

---

## Secrets

**URL:** https://docs.openclaw.ai/cli/secrets

**Contents:**
- Secrets
- Documentation Index
- ​openclaw secrets
- ​Reload runtime snapshot
- ​Audit
- ​Configure (interactive helper)
- ​Apply a saved plan
- ​Why no rollback backups
- ​Example
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
openclaw secrets audit --check
openclaw secrets configure
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json
openclaw secrets audit --check
openclaw secrets reload
```

Example 2 (typescript):
```typescript
openclaw secrets reload
openclaw secrets reload --json
openclaw secrets reload --url ws://127.0.0.1:18789 --token <token>
```

Example 3 (unknown):
```unknown
openclaw secrets audit
openclaw secrets audit --check
openclaw secrets audit --json
openclaw secrets audit --allow-exec
```

Example 4 (unknown):
```unknown
openclaw secrets configure
openclaw secrets configure --plan-out /tmp/openclaw-secrets-plan.json
openclaw secrets configure --apply --yes
openclaw secrets configure --providers-only
openclaw secrets configure --skip-provider-setup
openclaw secrets configure --agent ops
openclaw secrets configure --json
```

---

## Completion

**URL:** https://docs.openclaw.ai/cli/completion

**Contents:**
- Completion
- Documentation Index
- ​openclaw completion
- ​Usage
- ​Options
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw completion
openclaw completion --shell zsh
openclaw completion --install
openclaw completion --shell fish --install
openclaw completion --write-state
openclaw completion --shell bash --write-state
```

---

## Voicecall

**URL:** https://docs.openclaw.ai/cli/voicecall

**Contents:**
- Voicecall
- Documentation Index
- ​openclaw voicecall
- ​Common commands
- ​Exposing webhooks (Tailscale)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
openclaw voicecall setup
openclaw voicecall smoke
openclaw voicecall status --json
openclaw voicecall status --call-id <id>
openclaw voicecall call --to "+15555550123" --message "Hello" --mode notify
openclaw voicecall continue --call-id <id> --message "Any questions?"
openclaw voicecall dtmf --call-id <id> --digits "ww123456#"
openclaw voicecall end --call-id <id>
```

Example 2 (unknown):
```unknown
openclaw voicecall setup --json
```

Example 3 (sass):
```sass
openclaw voicecall smoke --to "+15555550123"        # dry run
openclaw voicecall smoke --to "+15555550123" --yes  # live notify call
```

Example 4 (unknown):
```unknown
openclaw voicecall expose --mode serve
openclaw voicecall expose --mode funnel
openclaw voicecall expose --mode off
```

---

## Clawbot

**URL:** https://docs.openclaw.ai/cli/clawbot

**Contents:**
- Clawbot
- Documentation Index
- ​openclaw clawbot
- ​Migration
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Reset

**URL:** https://docs.openclaw.ai/cli/reset

**Contents:**
- Reset
- Documentation Index
- ​openclaw reset
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
openclaw backup create
openclaw reset
openclaw reset --dry-run
openclaw reset --scope config --yes --non-interactive
openclaw reset --scope config+creds+sessions --yes --non-interactive
openclaw reset --scope full --yes --non-interactive
```

---

## Approvals

**URL:** https://docs.openclaw.ai/cli/approvals

**Contents:**
- Approvals
- Documentation Index
- ​openclaw approvals
- ​openclaw exec-policy
- ​Common commands
- ​Replace approvals from a file
- ​”Never prompt” / YOLO example
- ​Allowlist helpers
- ​Common options
- ​Notes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw exec-policy show
openclaw exec-policy show --json

openclaw exec-policy preset yolo
openclaw exec-policy preset cautious --json

openclaw exec-policy set --host gateway --security full --ask off --ask-fallback full
```

Example 2 (unknown):
```unknown
openclaw approvals get
openclaw approvals get --node <id|name|ip>
openclaw approvals get --gateway
```

Example 3 (json):
```json
openclaw approvals set --file ./exec-approvals.json
openclaw approvals set --stdin <<'EOF'
{ version: 1, defaults: { security: "full", ask: "off" } }
EOF
openclaw approvals set --node <id|name|ip> --file ./exec-approvals.json
openclaw approvals set --gateway --file ./exec-approvals.json
```

Example 4 (json):
```json
openclaw approvals set --stdin <<'EOF'
{
  version: 1,
  defaults: {
    security: "full",
    ask: "off",
    askFallback: "full"
  }
}
EOF
```

---

## Docs

**URL:** https://docs.openclaw.ai/cli/docs

**Contents:**
- Docs
- Documentation Index
- ​openclaw docs
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw docs
openclaw docs browser existing-session
openclaw docs sandbox allowHostControl
openclaw docs gateway token secretref
```

---
