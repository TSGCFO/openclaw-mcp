# Openclaw-Mcp_Docs - Getting Started

**Pages:** 49

---

## Feishu

**URL:** https://docs.openclaw.ai/channels/feishu

**Contents:**
- Feishu
- Documentation Index
- ​Feishu / Lark
- ​Quick start
- ​Access control
  - ​Direct messages
  - ​Group chats
- ​Group configuration examples
  - ​Allow all groups, no @mention required
  - ​Allow all groups, still require @mention

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Run the channel setup wizard

After setup completes, restart the gateway to apply the changes

**Examples:**

Example 1 (unknown):
```unknown
openclaw channels login --channel feishu
```

Example 2 (unknown):
```unknown
openclaw gateway restart
```

Example 3 (typescript):
```typescript
openclaw pairing list feishu
openclaw pairing approve feishu <CODE>
```

Example 4 (json):
```json
{
  channels: {
    feishu: {
      groupPolicy: "open",
    },
  },
}
```

---

## Bun (experimental)

**URL:** https://docs.openclaw.ai/install/bun

**Contents:**
- Bun (experimental)
- Documentation Index
- ​Install
- ​Lifecycle scripts
- ​Caveats
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
bun install
```

Example 2 (unknown):
```unknown
bun install --no-save
```

Example 3 (unknown):
```unknown
bun run build
bun run vitest run
```

Example 4 (elixir):
```elixir
bun pm trust @whiskeysockets/baileys protobufjs
```

---

## Agent bootstrapping

**URL:** https://docs.openclaw.ai/start/bootstrapping

**Contents:**
- Agent bootstrapping
- Documentation Index
- ​What bootstrapping does
- ​Skipping bootstrapping
- ​Where it runs
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## GCP

**URL:** https://docs.openclaw.ai/install/gcp

**Contents:**
- GCP
- Documentation Index
- ​OpenClaw on GCP Compute Engine (Docker, Production VPS Guide)
- ​Goal
- ​What are we doing (simple terms)?
- ​Quick path (experienced operators)
- ​What you need
- ​Troubleshooting
- ​Service accounts (security best practice)
- ​Next steps

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Install gcloud CLI (or use Console)

Install Docker (on the VM)

Clone the OpenClaw repository

Create persistent host directories

Configure environment variables

Docker Compose configuration

Shared Docker VM runtime steps

GCP-specific launch notes

Access from your laptop

**Examples:**

Example 1 (unknown):
```unknown
gcloud init
gcloud auth login
```

Example 2 (unknown):
```unknown
gcloud projects create my-openclaw-project --name="OpenClaw Gateway"
gcloud config set project my-openclaw-project
```

Example 3 (unknown):
```unknown
gcloud services enable compute.googleapis.com
```

Example 4 (sass):
```sass
gcloud compute instances create openclaw-gateway \
  --zone=us-central1-a \
  --machine-type=e2-small \
  --boot-disk-size=20GB \
  --image-family=debian-12 \
  --image-project=debian-cloud
```

---

## Raspberry Pi

**URL:** https://docs.openclaw.ai/install/raspberry-pi

**Contents:**
- Raspberry Pi
- Documentation Index
- ​Prerequisites
- ​Setup
- ​Performance tips
- ​Troubleshooting
- ​Next steps
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Add swap (important for 2 GB or less)

Access the Control UI

**Examples:**

Example 1 (elixir):
```elixir
ssh user@gateway-host
```

Example 2 (sql):
```sql
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl build-essential

# Set timezone (important for cron and reminders)
sudo timedatectl set-timezone America/Chicago
```

Example 3 (unknown):
```unknown
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install -y nodejs
node --version
```

Example 4 (sass):
```sass
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Reduce swappiness for low-RAM devices
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---

## Showcase

**URL:** https://docs.openclaw.ai/start/showcase

**Contents:**
- Showcase
- Documentation Index
- ​Videos
- Full setup walkthrough
- Community showcase reel
- Projects in the wild
- ​Fresh from Discord
- PR Review to Telegram Feedback
- Wine Cellar Skill in Minutes
- Tesco Shop Autopilot

Real-world OpenClaw projects from the community

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Docker VM runtime

**URL:** https://docs.openclaw.ai/install/docker-vm-runtime

**Contents:**
- Docker VM runtime
- Documentation Index
- ​Bake required binaries into the image
- ​Build and launch
- ​What persists where
- ​Updates
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
FROM node:24-bookworm

RUN apt-get update && apt-get install -y socat && rm -rf /var/lib/apt/lists/*

# Example binary 1: Gmail CLI (gogcli — installs as `gog`)
# Copy the current Linux asset URL from https://github.com/steipete/gogcli/releases
RUN curl -L https://github.com/steipete/gogcli/releases/latest/download/gogcli_linux_amd64.tar.gz \
  | tar -xzO gog > /usr/local/bin/gog; \
  chmod +x /usr/local/bin/gog

# Example binary 2: Google Places CLI
# Copy the current Linux asset URL from https://github.com/steipete/goplaces/releases
RUN curl -L https://github.com/steipete/goplaces/releases/latest/download/goplaces_linux_amd64.tar.gz \
  | tar -xzO goplaces > /usr/local/bin/goplaces; \
  chmod +x /usr/local/bin/goplaces

# Example binary 3: WhatsApp CLI
# Copy the current Linux asset URL from https://github.com/steipete/wacli/releases
RUN curl -L https://github.com/steipete/wacli/releases/latest/download/wacli-linux-amd64.tar.gz \
  | tar -xzO wacli > /usr/local/bin/wacli; \
  chmod +x /usr/local/bin/wacli

# Add more binaries below using the same pattern

WORKDIR /app
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml .npmrc ./
COPY ui/package.json ./ui/package.json
COPY scripts ./scripts

RUN corepack enable
RUN pnpm install --frozen-lockfile

COPY . .
RUN pnpm build
RUN pnpm ui:install
RUN pnpm ui:build

ENV NODE_ENV=production

CMD ["node","dist/index.js"]
```

Example 2 (unknown):
```unknown
docker compose build
docker compose up -d openclaw-gateway
```

Example 3 (unknown):
```unknown
docker compose exec openclaw-gateway which gog
docker compose exec openclaw-gateway which goplaces
docker compose exec openclaw-gateway which wacli
```

Example 4 (unknown):
```unknown
/usr/local/bin/gog
/usr/local/bin/goplaces
/usr/local/bin/wacli
```

---

## Docs hubs

**URL:** https://docs.openclaw.ai/start/hubs

**Contents:**
- Docs hubs
- Documentation Index
- ​Start here
- ​Installation + updates
- ​Core concepts
- ​Providers + ingress
- ​Gateway + operations
- ​Tools + automation
- ​Nodes, media, voice
- ​Platforms

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Nix

**URL:** https://docs.openclaw.ai/install/nix

**Contents:**
- Nix
- Documentation Index
- ​What you get
- ​Quick start
- ​Nix-mode runtime behavior
  - ​What changes in Nix mode
  - ​Config and state paths
  - ​Service PATH discovery
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Install Determinate Nix

Fill in template placeholders and switch

**Examples:**

Example 1 (sql):
```sql
mkdir -p ~/code/openclaw-local
# Copy templates/agent-first/flake.nix from the nix-openclaw repo
```

Example 2 (unknown):
```unknown
home-manager switch
```

Example 3 (sass):
```sass
export OPENCLAW_NIX_MODE=1
```

Example 4 (unknown):
```unknown
defaults write ai.openclaw.mac openclaw.nixMode -bool true
```

---

## FAQ: first-run setup

**URL:** https://docs.openclaw.ai/help/faq-first-run

**Contents:**
- FAQ: first-run setup
- Documentation Index
- ​Quick start and first-run setup
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

I am stuck, fastest way to get unstuck

Heartbeat keeps skipping. What do the skip reasons mean?

Recommended way to install and set up OpenClaw

How do I open the dashboard after onboarding?

How do I authenticate the dashboard on localhost vs remote?

Why are there two exec approval configs for chat approvals?

What runtime do I need?

Does it run on Raspberry Pi?

Any tips for Raspberry Pi installs?

It is stuck on wake up my friend / onboarding will not hatch. What now?

Can I migrate my setup to a new machine (Mac mini) without redoing onboarding?

Where do I see what is new in the latest version?

Cannot access docs.openclaw.ai (SSL error)

Difference between stable and beta

How do I install the beta version and what is the difference between beta and dev?

How do I try the latest bits?

How long does install and onboarding usually take?

Installer stuck? How do I get more feedback?

Windows install says git not found or openclaw not recognized

Windows exec output shows garbled Chinese text - what should I do?

The docs did not answer my question - how do I get a better answer?

How do I install OpenClaw on Linux?

How do I install OpenClaw on a VPS?

Where are the cloud/VPS install guides?

Can I ask OpenClaw to update itself?

What does onboarding actually do?

Do I need a Claude or OpenAI subscription to run this?

Can I use Claude Max subscription without an API key?

Do you support Claude subscription auth (Claude Pro or Max)?

Why am I seeing HTTP 429 rate_limit_error from Anthropic?

Is AWS Bedrock supported?

How does Codex auth work?

Why does OpenClaw still mention openai-codex?

Why can Codex OAuth limits differ from ChatGPT web?

Do you support OpenAI subscription auth (Codex OAuth)?

How do I set up Gemini CLI OAuth?

Is a local model OK for casual chats?

How do I keep hosted model traffic in a specific region?

Do I have to buy a Mac Mini to install this?

Do I need a Mac mini for iMessage support?

If I buy a Mac mini to run OpenClaw, can I connect it to my MacBook Pro?

Telegram: what goes in allowFrom?

Can multiple people use one WhatsApp number with different OpenClaw instances?

Can I run a "fast chat" agent and an "Opus for coding" agent?

Does Homebrew work on Linux?

Difference between the hackable git install and npm install

Can I switch between npm and git installs later?

Should I run the Gateway on my laptop or a VPS?

How important is it to run OpenClaw on a dedicated machine?

What are the minimum VPS requirements and recommended OS?

Can I run OpenClaw in a VM and what are the requirements?

**Examples:**

Example 1 (unknown):
```unknown
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git
```

Example 2 (unknown):
```unknown
openclaw status
openclaw models status
openclaw doctor
```

Example 3 (unknown):
```unknown
curl -fsSL https://openclaw.ai/install.sh | bash
openclaw onboard --install-daemon
```

Example 4 (unknown):
```unknown
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm build
pnpm ui:build
openclaw onboard
```

---

## Fly.io

**URL:** https://docs.openclaw.ai/install/fly

**Contents:**
- Fly.io
- Documentation Index
- ​Fly.io Deployment
- ​What you need
- ​Beginner quick path
  - ​Control UI
  - ​Logs
  - ​SSH Console
- ​Troubleshooting
  - ​”App is not listening on expected address”

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
# Clone the repo
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# Create a new Fly app (pick your own name)
fly apps create my-openclaw

# Create a persistent volume (1GB is usually enough)
fly volumes create openclaw_data --size 1 --region iad
```

Example 2 (sass):
```sass
app = "my-openclaw"  # Your app name
primary_region = "iad"

[build]
  dockerfile = "Dockerfile"

[env]
  NODE_ENV = "production"
  OPENCLAW_PREFER_PNPM = "1"
  OPENCLAW_STATE_DIR = "/data"
  NODE_OPTIONS = "--max-old-space-size=1536"

[processes]
  app = "node dist/index.js gateway --allow-unconfigured --port 3000 --bind lan"

[http_service]
  internal_port = 3000
  force_https = true
  auto_stop_machines = false
  auto_start_machines = true
  min_machines_running = 1
  processes = ["app"]

[[vm]]
  size = "shared-cpu-2x"
  memory = "2048mb"

[mounts]
  source = "openclaw_data"
  destination = "/data"
```

Example 3 (lua):
```lua
# Required: Gateway token (for non-loopback binding)
fly secrets set OPENCLAW_GATEWAY_TOKEN=$(openssl rand -hex 32)

# Model provider API keys
fly secrets set ANTHROPIC_API_KEY=sk-ant-...

# Optional: Other providers
fly secrets set OPENAI_API_KEY=sk-...
fly secrets set GOOGLE_API_KEY=...

# Channel tokens
fly secrets set DISCORD_BOT_TOKEN=MTQ...
```

Example 4 (unknown):
```unknown
fly status
fly logs
```

---

## Docs directory

**URL:** https://docs.openclaw.ai/start/docs-directory

**Contents:**
- Docs directory
- Documentation Index
- ​Start here
- ​Providers and UX
- ​Companion apps
- ​Operations and safety
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OpenClaw lore

**URL:** https://docs.openclaw.ai/start/lore

**Contents:**
- OpenClaw lore
- Documentation Index
- ​The Lore of OpenClaw 🦞📖
- ​The Origin Story
- ​The First Molt (January 27, 2026)
- ​The Name
- ​The Daleks vs The Lobsters
- ​Key Characters
  - ​Molty 🦞
  - ​Peter 👨‍💻

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
OpenClaw = OPEN + CLAW
        = Open source, open to everyone
        = Our lobster heritage, where we came from
        = The claw is the law 🦞
        = Your assistant. Your machine. Your rules.
```

Example 2 (unknown):
```unknown
I am Molty.
I live in the OpenClaw.
I shall not dump directories to strangers.
I shall not tweet without permission.
I shall always remember that molting is growth.
I shall EXFOLIATE my enemies with kindness.

🦞
```

---

## Docker

**URL:** https://docs.openclaw.ai/install/docker

**Contents:**
- Docker
- Documentation Index
- ​Is Docker right for me?
- ​Prerequisites
- ​Containerized gateway
  - ​Manual flow
  - ​Environment variables
  - ​Observability
  - ​Health checks
  - ​LAN vs loopback

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Configure channels (optional)

Enable agent sandbox for Docker gateway

Automation / CI (non-interactive)

Shared-network security note

Permissions and EACCES

Power-user container options

OpenAI Codex OAuth (headless Docker)

Image missing or sandbox container not starting

Permission errors in sandbox

Custom tools not found in sandbox

OOM-killed during image build (exit 137)

Unauthorized or pairing required in Control UI

Gateway target shows ws://172.x.x.x or pairing errors from Docker CLI

**Examples:**

Example 1 (unknown):
```unknown
./scripts/docker/setup.sh
```

Example 2 (unknown):
```unknown
export OPENCLAW_IMAGE="ghcr.io/openclaw/openclaw:latest"
./scripts/docker/setup.sh
```

Example 3 (unknown):
```unknown
docker compose run --rm openclaw-cli dashboard --no-open
```

Example 4 (markdown):
```markdown
# WhatsApp (QR)
docker compose run --rm openclaw-cli channels login

# Telegram
docker compose run --rm openclaw-cli channels add --channel telegram --token "<token>"

# Discord
docker compose run --rm openclaw-cli channels add --channel discord --token "<token>"
```

---

## Migration guide

**URL:** https://docs.openclaw.ai/install/migrating

**Contents:**
- Migration guide
- Documentation Index
- ​Import from another agent system
- Migrating from Claude
- Migrating from Hermes
- ​Move OpenClaw to a new machine
  - ​Migration steps
  - ​Common pitfalls
  - ​Verification checklist
- ​Upgrade a plugin in place

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Stop the gateway and back up

Install OpenClaw on the new machine

Copy state directory and workspace

Run doctor and verify

Profile or state-dir mismatch

Copying only openclaw.json

Permissions and ownership

**Examples:**

Example 1 (unknown):
```unknown
openclaw gateway stop
cd ~
tar -czf openclaw-state.tgz .openclaw
```

Example 2 (unknown):
```unknown
cd ~
tar -xzf openclaw-state.tgz
```

Example 3 (unknown):
```unknown
openclaw doctor
openclaw gateway restart
openclaw status
```

Example 4 (sass):
```sass
awk -F= '/^(TELEGRAM_BOT_TOKEN|DISCORD_BOT_TOKEN)=/ { print $1 "=present" }' ~/.openclaw/.env
```

---

## Onboarding overview

**URL:** https://docs.openclaw.ai/start/onboarding-overview

**Contents:**
- Onboarding overview
- Documentation Index
- ​Which path should I use?
- ​What onboarding configures
- ​CLI onboarding
- ​macOS app onboarding
- ​Custom or unlisted providers
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard
```

---

## DigitalOcean

**URL:** https://docs.openclaw.ai/install/digitalocean

**Contents:**
- DigitalOcean
- Documentation Index
- ​Prerequisites
- ​Setup
- ​Troubleshooting
- ​Next steps
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Add swap (recommended for 1 GB Droplets)

Access the Control UI

**Examples:**

Example 1 (sql):
```sql
ssh root@YOUR_DROPLET_IP

apt update && apt upgrade -y

# Install Node.js 24
curl -fsSL https://deb.nodesource.com/setup_24.x | bash -
apt install -y nodejs

# Install OpenClaw
curl -fsSL https://openclaw.ai/install.sh | bash
openclaw --version
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (rust):
```rust
fallocate -l 2G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo '/swapfile none swap sw 0 0' >> /etc/fstab
```

Example 4 (unknown):
```unknown
openclaw status
systemctl --user status openclaw-gateway.service
journalctl --user -u openclaw-gateway.service -f
```

---

## Onboarding (CLI)

**URL:** https://docs.openclaw.ai/start/wizard

**Contents:**
- Onboarding (CLI)
- Documentation Index
- ​QuickStart vs Advanced
- ​What onboarding configures
- ​Add another agent
- ​Full reference
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard
```

Example 2 (typescript):
```typescript
openclaw configure
openclaw agents add <name>
```

---

## Personal assistant setup

**URL:** https://docs.openclaw.ai/start/openclaw

**Contents:**
- Personal assistant setup
- Documentation Index
- ​Building a personal assistant with OpenClaw
- ​⚠️ Safety first
- ​Prerequisites
- ​The two-phone setup (recommended)
- ​5-minute quick start
- ​Give the agent a workspace (AGENTS)
- ​The config that turns it into “an assistant”
- ​Sessions and memory

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw channels login
```

Example 2 (unknown):
```unknown
openclaw gateway --port 18789
```

Example 3 (sass):
```sass
{
  gateway: { mode: "local" },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

Example 4 (unknown):
```unknown
openclaw setup
```

---

## exe.dev

**URL:** https://docs.openclaw.ai/install/exe-dev

**Contents:**
- exe.dev
- Documentation Index
- ​Beginner quick path
- ​What you need
- ​Automated install with Shelley
- ​Manual installation
- ​1) Create the VM
- ​2) Install prerequisites (on the VM)
- ​3) Install OpenClaw
- ​4) Setup nginx to proxy OpenClaw to port 8000

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
Set up OpenClaw (https://docs.openclaw.ai/install) on this VM. Use the non-interactive and accept-risk flags for openclaw onboarding. Add the supplied auth or token as needed. Configure nginx to forward from the default port 18789 to the root location on the default enabled site config, making sure to enable Websocket support. Pairing is done by "openclaw devices list" and "openclaw devices approve <request id>". Make sure the dashboard shows that OpenClaw's health is OK. exe.dev handles forwarding from port 8000 to port 80/443 and HTTPS for us, so the final "reachable" should be <vm-name>.exe.xyz, without port specification.
```

Example 2 (unknown):
```unknown
ssh exe.dev new
```

Example 3 (unknown):
```unknown
ssh <vm-name>.exe.xyz
```

Example 4 (sql):
```sql
sudo apt-get update
sudo apt-get install -y git curl jq ca-certificates openssl
```

---

## Migrating from Hermes

**URL:** https://docs.openclaw.ai/install/migrating-hermes

**Contents:**
- Migrating from Hermes
- Documentation Index
- ​Two ways to import
- ​What gets imported
- ​What stays archive-only
- ​Recommended flow
- ​Conflict handling
- ​Secrets
- ​JSON output for automation
- ​Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Apply refuses with conflicts

Hermes lives outside ~/.hermes

Onboarding refuses to import on an existing setup

API keys did not import

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --flow import
```

Example 2 (sql):
```sql
openclaw onboard --import-from hermes --import-source ~/.hermes
```

Example 3 (unknown):
```unknown
openclaw migrate hermes --dry-run    # preview only
openclaw migrate apply hermes --yes  # apply with confirmation skipped
```

Example 4 (unknown):
```unknown
openclaw migrate hermes --dry-run
```

---

## Uninstall

**URL:** https://docs.openclaw.ai/cli/uninstall

**Contents:**
- Uninstall
- Documentation Index
- ​openclaw uninstall
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw backup create
openclaw uninstall
openclaw uninstall --service --yes --non-interactive
openclaw uninstall --state --workspace --yes --non-interactive
openclaw uninstall --all --yes
openclaw uninstall --dry-run
```

---

## Kubernetes

**URL:** https://docs.openclaw.ai/install/kubernetes

**Contents:**
- Kubernetes
- Documentation Index
- ​OpenClaw on Kubernetes
- ​Why not Helm?
- ​What you need
- ​Quick start
- ​Local testing with Kind
- ​Step by step
  - ​1) Deploy
  - ​2) Access the gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
# Replace with your provider: ANTHROPIC, GEMINI, OPENAI, or OPENROUTER
export <PROVIDER>_API_KEY="..."
./scripts/k8s/deploy.sh

kubectl port-forward svc/openclaw 18789:18789 -n openclaw
open http://localhost:18789
```

Example 2 (unknown):
```unknown
kubectl get secret openclaw-secrets -n openclaw -o jsonpath='{.data.OPENCLAW_GATEWAY_TOKEN}' | base64 -d
```

Example 3 (unknown):
```unknown
./scripts/k8s/create-kind.sh           # auto-detects docker or podman
./scripts/k8s/create-kind.sh --delete  # tear down
```

Example 4 (lua):
```lua
# Replace with your provider: ANTHROPIC, GEMINI, OPENAI, or OPENROUTER
export <PROVIDER>_API_KEY="..."
./scripts/k8s/deploy.sh
```

---

## Getting started

**URL:** https://docs.openclaw.ai/start/getting-started

**Contents:**
- Getting started
- Documentation Index
- ​What you need
- ​Quick setup
- ​What to do next
- Connect a channel
- Pairing and safety
- Configure the Gateway
- Browse tools
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the Gateway is running

Send your first message

Advanced: mount a custom Control UI build

Advanced: environment variables

**Examples:**

Example 1 (unknown):
```unknown
curl -fsSL https://openclaw.ai/install.sh | bash
```

Example 2 (unknown):
```unknown
iwr -useb https://openclaw.ai/install.ps1 | iex
```

Example 3 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 4 (unknown):
```unknown
openclaw gateway status
```

---

## Migrating from Claude

**URL:** https://docs.openclaw.ai/install/migrating-claude

**Contents:**
- Migrating from Claude
- Documentation Index
- ​Two ways to import
- ​What gets imported
- ​What stays archive-only
- ​Source selection
- ​Recommended flow
- ​Conflict handling
- ​JSON output for automation
- ​Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Instructions and memory

Claude state lives outside ~/.claude

Onboarding refuses to import on an existing setup

MCP servers from Claude Desktop did not import

Claude commands became skills with model invocation disabled

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --flow import
```

Example 2 (sql):
```sql
openclaw onboard --import-from claude --import-source ~/.claude
```

Example 3 (unknown):
```unknown
openclaw migrate claude --dry-run
openclaw migrate apply claude --yes
```

Example 4 (unknown):
```unknown
openclaw migrate claude --dry-run
```

---

## Oracle Cloud

**URL:** https://docs.openclaw.ai/install/oracle

**Contents:**
- Oracle Cloud
- Documentation Index
- ​Prerequisites
- ​Setup
- ​Fallback: SSH tunnel
- ​Troubleshooting
- ​Next steps
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create an OCI instance

Connect and update the system

Configure user and hostname

Configure the gateway

Lock down VCN security

**Examples:**

Example 1 (sql):
```sql
ssh ubuntu@YOUR_PUBLIC_IP

sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential
```

Example 2 (powershell):
```powershell
sudo hostnamectl set-hostname openclaw
sudo passwd ubuntu
sudo loginctl enable-linger ubuntu
```

Example 3 (sass):
```sass
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up --ssh --hostname=openclaw
```

Example 4 (unknown):
```unknown
curl -fsSL https://openclaw.ai/install.sh | bash
source ~/.bashrc
```

---

## Northflank

**URL:** https://docs.openclaw.ai/install/northflank

**Contents:**
- Northflank
- Documentation Index
- ​Northflank
- ​How to get started
- ​What you get
- ​Connect a channel
- ​Next steps

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Model provider quickstart

**URL:** https://docs.openclaw.ai/providers/models

**Contents:**
- Model provider quickstart
- Documentation Index
- ​Model Providers
- ​Quick start (two steps)
- ​Supported providers (starter set)
- ​Additional bundled provider variants
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

---

## ClawDock

**URL:** https://docs.openclaw.ai/install/clawdock

**Contents:**
- ClawDock
- Documentation Index
- ​Install
- ​What you get
  - ​Basic operations
  - ​Container access
  - ​Web UI and pairing
  - ​Setup and maintenance
  - ​Utilities
- ​First-time flow

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (bash):
```bash
mkdir -p ~/.clawdock && curl -sL https://raw.githubusercontent.com/openclaw/openclaw/main/scripts/clawdock/clawdock-helpers.sh -o ~/.clawdock/clawdock-helpers.sh
echo 'source ~/.clawdock/clawdock-helpers.sh' >> ~/.zshrc && source ~/.zshrc
```

Example 2 (unknown):
```unknown
clawdock-start
clawdock-fix-token
clawdock-dashboard
```

Example 3 (unknown):
```unknown
clawdock-devices
clawdock-approve <request-id>
```

---

## General troubleshooting

**URL:** https://docs.openclaw.ai/help/troubleshooting

**Contents:**
- General troubleshooting
- Documentation Index
- ​First 60 seconds
- ​Anthropic long context 429
- ​Local OpenAI-compatible backend works directly but fails in OpenClaw
- ​Plugin install fails with missing openclaw extensions
- ​Decision tree
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Dashboard or Control UI will not connect

Gateway will not start or service installed but not running

Channel connects but messages do not flow

Cron or heartbeat did not fire or did not deliver

Node is paired but tool fails camera canvas screen exec

Exec suddenly asks for approval

**Examples:**

Example 1 (unknown):
```unknown
openclaw status
openclaw status --all
openclaw gateway probe
openclaw gateway status
openclaw doctor
openclaw channels status --probe
openclaw logs --follow
```

Example 2 (json):
```json
{
  "name": "@openclaw/my-plugin",
  "version": "1.2.3",
  "openclaw": {
    "extensions": ["./dist/index.js"]
  }
}
```

Example 3 (typescript):
```typescript
openclaw status
openclaw gateway status
openclaw channels status --probe
openclaw pairing list --channel <channel> [--account <id>]
openclaw logs --follow
```

Example 4 (unknown):
```unknown
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

---

## macOS VMs

**URL:** https://docs.openclaw.ai/install/macos-vm

**Contents:**
- macOS VMs
- Documentation Index
- ​OpenClaw on macOS VMs (Sandboxing)
- ​Recommended default (most users)
- ​macOS VM options
  - ​Local VM on your Apple Silicon Mac (Lume)
  - ​Hosted Mac providers (cloud)
- ​Quick path (Lume, experienced users)
- ​What you need (Lume)
- ​1) Install Lume

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/trycua/cua/main/libs/lume/scripts/install.sh)"
```

Example 2 (scss):
```scss
echo 'export PATH="$PATH:$HOME/.local/bin"' >> ~/.zshrc && source ~/.zshrc
```

Example 3 (unknown):
```unknown
lume --version
```

Example 4 (unknown):
```unknown
lume create openclaw --os macos --ipsw latest
```

---

## Setup

**URL:** https://docs.openclaw.ai/start/setup

**Contents:**
- Setup
- Documentation Index
- ​TL;DR
- ​Prereqs (from source)
- ​Tailoring strategy (so updates do not hurt)
- ​Run the Gateway from this repo
- ​Stable workflow (macOS app first)
- ​Bleeding edge workflow (Gateway in a terminal)
  - ​0) (Optional) Run the macOS app from source too
  - ​1) Start the dev Gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw setup
```

Example 2 (unknown):
```unknown
openclaw setup
```

Example 3 (unknown):
```unknown
node openclaw.mjs gateway --port 18789 --verbose
```

Example 4 (unknown):
```unknown
openclaw channels login
```

---

## Podman

**URL:** https://docs.openclaw.ai/install/podman

**Contents:**
- Podman
- Documentation Index
- ​Prerequisites
- ​Quick start
- ​Podman + Tailscale
- ​Systemd (Quadlet, optional)
- ​Config, env, and storage
- ​Useful commands
- ​Troubleshooting
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Start the Gateway container

Run onboarding inside the container

Manage the running container from the host CLI

**Examples:**

Example 1 (unknown):
```unknown
./scripts/podman/setup.sh --quadlet
```

Example 2 (unknown):
```unknown
./scripts/run-openclaw-podman.sh launch
```

Example 3 (unknown):
```unknown
./scripts/run-openclaw-podman.sh launch setup
```

Example 4 (sass):
```sass
export OPENCLAW_CONTAINER=openclaw
```

---

## Render

**URL:** https://docs.openclaw.ai/install/render

**Contents:**
- Render
- Documentation Index
- ​Render
- ​Prerequisites
- ​Deploy with a Render Blueprint
- ​Understanding the Blueprint
- ​Choosing a plan
- ​After deployment
  - ​Access the Control UI
- ​Render Dashboard features

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (yaml):
```yaml
services:
  - type: web
    name: openclaw
    runtime: docker
    plan: starter
    healthCheckPath: /health
    envVars:
      - key: OPENCLAW_GATEWAY_PORT
        value: "8080"
      - key: OPENCLAW_STATE_DIR
        value: /data/.openclaw
      - key: OPENCLAW_WORKSPACE_DIR
        value: /data/workspace
      - key: OPENCLAW_GATEWAY_TOKEN
        generateValue: true # auto-generates a secure token
    disk:
      name: openclaw-data
      mountPath: /data
      sizeGB: 1
```

Example 2 (unknown):
```unknown
openclaw backup create
```

---

## Release channels

**URL:** https://docs.openclaw.ai/install/development-channels

**Contents:**
- Release channels
- Documentation Index
- ​Development channels
- ​Switching channels
- ​One-off version or tag targeting
- ​Dry run
- ​Plugins and channels
- ​Checking current status
- ​Tagging best practices
- ​macOS app availability

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
openclaw update --channel stable
openclaw update --channel beta
openclaw update --channel dev
```

Example 2 (sql):
```sql
# Install a specific version
openclaw update --tag 2026.4.1-beta.1

# Install from the beta dist-tag (one-off, does not persist)
openclaw update --tag beta

# Install from GitHub main branch (npm tarball)
openclaw update --tag main

# Install a specific npm package spec
openclaw update --tag openclaw@2026.4.1-beta.1
```

Example 3 (sql):
```sql
openclaw update --dry-run
openclaw update --channel beta --dry-run
openclaw update --tag 2026.4.1-beta.1 --dry-run
openclaw update --dry-run --json
```

Example 4 (sql):
```sql
openclaw update status
```

---

## Updating

**URL:** https://docs.openclaw.ai/install/updating

**Contents:**
- Updating
- Documentation Index
- ​Recommended: openclaw update
- ​Switch between npm and git installs
- ​Alternative: re-run the installer
- ​Alternative: manual npm, pnpm, or bun
  - ​Advanced npm install topics
- ​Auto-updater
- ​After updating
- ​Rollback

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Read-only package tree

Hardened systemd units

**Examples:**

Example 1 (sql):
```sql
openclaw update
```

Example 2 (sql):
```sql
openclaw update --channel beta
openclaw update --channel dev
openclaw update --tag main
openclaw update --dry-run   # preview without applying
```

Example 3 (go):
```go
# npm package install -> editable git checkout
openclaw update --channel dev

# git checkout -> npm package install
openclaw update --channel stable
```

Example 4 (sql):
```sql
openclaw update --channel dev --dry-run
openclaw update --channel stable --dry-run
```

---

## Ansible

**URL:** https://docs.openclaw.ai/install/ansible

**Contents:**
- Ansible
- Documentation Index
- ​Ansible Installation
- ​Prerequisites
- ​What you get
- ​Quick start
- ​What gets installed
- ​Post-Install Setup
  - ​Quick commands
- ​Security architecture

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Switch to the openclaw user

Run the onboarding wizard

Connect messaging providers

Verify the installation

Install prerequisites

Install Ansible collections

Firewall blocks my connection

Service will not start

Docker sandbox issues

**Examples:**

Example 1 (unknown):
```unknown
curl -fsSL https://raw.githubusercontent.com/openclaw/openclaw-ansible/main/install.sh | bash
```

Example 2 (unknown):
```unknown
sudo -i -u openclaw
```

Example 3 (unknown):
```unknown
openclaw channels login
```

Example 4 (unknown):
```unknown
sudo systemctl status openclaw
sudo journalctl -u openclaw -f
```

---

## Hetzner

**URL:** https://docs.openclaw.ai/install/hetzner

**Contents:**
- Hetzner
- Documentation Index
- ​OpenClaw on Hetzner (Docker, Production VPS Guide)
- ​Goal
- ​What are we doing (simple terms)?
- ​Quick path (experienced operators)
- ​What you need
- ​Infrastructure as Code (Terraform)
- ​Next steps
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Install Docker (on the VPS)

Clone the OpenClaw repository

Create persistent host directories

Configure environment variables

Docker Compose configuration

Shared Docker VM runtime steps

Hetzner-specific access

**Examples:**

Example 1 (elixir):
```elixir
ssh root@YOUR_VPS_IP
```

Example 2 (sql):
```sql
apt-get update
apt-get install -y git curl ca-certificates
curl -fsSL https://get.docker.com | sh
```

Example 3 (unknown):
```unknown
docker --version
docker compose version
```

Example 4 (unknown):
```unknown
git clone https://github.com/openclaw/openclaw.git
cd openclaw
```

---

## Onboarding reference

**URL:** https://docs.openclaw.ai/reference/wizard

**Contents:**
- Onboarding reference
- Documentation Index
- ​Flow details (local mode)
- ​Non-interactive mode
  - ​Add agent (non-interactive)
- ​Gateway wizard RPC
- ​Signal setup (signal-cli)
- ​What the wizard writes
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Existing config detection

**Examples:**

Example 1 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice apiKey \
  --anthropic-api-key "$ANTHROPIC_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback \
  --install-daemon \
  --daemon-runtime node \
  --skip-skills
```

Example 2 (unknown):
```unknown
export OPENCLAW_GATEWAY_TOKEN="your-token"
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice skip \
  --gateway-auth token \
  --gateway-token-ref-env OPENCLAW_GATEWAY_TOKEN
```

Example 3 (unknown):
```unknown
openclaw agents add work \
  --workspace ~/.openclaw/workspace-work \
  --model openai/gpt-5.5 \
  --bind whatsapp:biz \
  --non-interactive \
  --json
```

---

## Azure

**URL:** https://docs.openclaw.ai/install/azure

**Contents:**
- Azure
- Documentation Index
- ​OpenClaw on Azure Linux VM
- ​What you will do
- ​What you need
- ​Configure deployment
- ​Deploy Azure resources
- ​Install OpenClaw
- ​Cost considerations
- ​Cleanup

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Register required resource providers (one-time)

Set deployment variables

Select VM size and OS disk size

Create the resource group

Create the network security group

Create the virtual network and subnets

SSH into the VM through Azure Bastion

Install OpenClaw (in the VM shell)

**Examples:**

Example 1 (unknown):
```unknown
az login
az extension add -n ssh
```

Example 2 (csharp):
```csharp
az provider register --namespace Microsoft.Compute
az provider register --namespace Microsoft.Network
```

Example 3 (csharp):
```csharp
az provider show --namespace Microsoft.Compute --query registrationState -o tsv
az provider show --namespace Microsoft.Network --query registrationState -o tsv
```

Example 4 (unknown):
```unknown
RG="rg-openclaw"
LOCATION="westus2"
VNET_NAME="vnet-openclaw"
VNET_PREFIX="10.40.0.0/16"
VM_SUBNET_NAME="snet-openclaw-vm"
VM_SUBNET_PREFIX="10.40.2.0/24"
BASTION_SUBNET_PREFIX="10.40.1.0/26"
NSG_NAME="nsg-openclaw-vm"
VM_NAME="vm-openclaw"
ADMIN_USERNAME="openclaw"
BASTION_NAME="bas-openclaw"
BASTION_PIP_NAME="pip-openclaw-bastion"
```

---

## Hostinger

**URL:** https://docs.openclaw.ai/install/hostinger

**Contents:**
- Hostinger
- Documentation Index
- ​Prerequisites
- ​Option A: 1-Click OpenClaw
- ​Option B: OpenClaw on VPS
- ​Verify your setup
- ​Troubleshooting
- ​Next steps
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Select a messaging channel

Complete installation

---

## Onboarding (macOS app)

**URL:** https://docs.openclaw.ai/start/onboarding

**Contents:**
- Onboarding (macOS app)
- Documentation Index
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Approve macOS warning

Approve find local networks

Welcome and security notice

Onboarding Chat (dedicated session)

---

## CLI setup reference

**URL:** https://docs.openclaw.ai/start/wizard-cli-reference

**Contents:**
- CLI setup reference
- Documentation Index
- ​What the wizard does
- ​Local flow details
- ​Remote mode details
- ​Auth and model options
- ​Outputs and internals
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Existing config detection

OpenAI Code subscription (OAuth)

OpenAI Code subscription (device pairing)

Cloudflare AI Gateway

Synthetic (Anthropic-compatible)

Ollama (Cloud and local open models)

Moonshot and Kimi Coding

---

## Node.js

**URL:** https://docs.openclaw.ai/install/node

**Contents:**
- Node.js
- Documentation Index
- ​Check your version
- ​Install Node
- ​Troubleshooting
  - ​openclaw: command not found
  - ​Permission errors on npm install -g (Linux)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Using a version manager (nvm, fnm, mise, asdf)

Find your global npm prefix

Check if it's on your PATH

Add it to your shell startup file

**Examples:**

Example 1 (unknown):
```unknown
brew install node
```

Example 2 (unknown):
```unknown
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Example 3 (unknown):
```unknown
sudo dnf install nodejs
```

Example 4 (unknown):
```unknown
winget install OpenJS.NodeJS.LTS
```

---

## Installer internals

**URL:** https://docs.openclaw.ai/install/installer

**Contents:**
- Installer internals
- Documentation Index
- ​Quick commands
- ​install.sh
  - ​Flow (install.sh)
  - ​Source checkout detection
  - ​Examples (install.sh)
- ​install-cli.sh
  - ​Flow (install-cli.sh)
  - ​Examples (install-cli.sh)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Ensure Node.js 24 by default

Environment variables reference

Install local Node runtime

Install OpenClaw under prefix

Refresh loaded gateway service

Environment variables reference

Ensure PowerShell + Windows environment

Ensure Node.js 24 by default

Environment variables reference

Why does npm hit EACCES on Linux?

Windows: "npm error spawn git / ENOENT"

Windows: "openclaw is not recognized"

Windows: how to get verbose installer output

openclaw not found after install

**Examples:**

Example 1 (sass):
```sass
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash
```

Example 2 (sass):
```sass
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --help
```

Example 3 (sass):
```sass
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash
```

Example 4 (sass):
```sass
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash -s -- --help
```

---

## Railway

**URL:** https://docs.openclaw.ai/install/railway

**Contents:**
- Railway
- Documentation Index
- ​Railway
- ​Quick checklist (new users)
- ​One-click deploy
- ​What you get
- ​Required Railway settings
  - ​Public Networking
  - ​Volume (required)
  - ​Variables

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw backup create
```

---

## Install

**URL:** https://docs.openclaw.ai/install

**Contents:**
- Install
- Documentation Index
- ​System requirements
- ​Recommended: installer script
- ​Alternative install methods
  - ​Local prefix installer (install-cli.sh)
  - ​npm, pnpm, or bun
  - ​From source
  - ​Install from GitHub main
  - ​Containers and package managers

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Troubleshooting: sharp build errors (npm)

**Examples:**

Example 1 (unknown):
```unknown
curl -fsSL https://openclaw.ai/install.sh | bash
```

Example 2 (unknown):
```unknown
iwr -useb https://openclaw.ai/install.ps1 | iex
```

Example 3 (unknown):
```unknown
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

Example 4 (julia):
```julia
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
```

---

## CLI automation

**URL:** https://docs.openclaw.ai/start/wizard-cli-automation

**Contents:**
- CLI automation
- Documentation Index
- ​Baseline non-interactive example
- ​Provider-specific examples
- ​Add another agent
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Anthropic API key example

Vercel AI Gateway example

Cloudflare AI Gateway example

Custom provider example

**Examples:**

Example 1 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice apiKey \
  --anthropic-api-key "$ANTHROPIC_API_KEY" \
  --secret-input-mode plaintext \
  --gateway-port 18789 \
  --gateway-bind loopback \
  --install-daemon \
  --daemon-runtime node \
  --skip-bootstrap \
  --skip-skills
```

Example 2 (unknown):
```unknown
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice openai-api-key \
  --secret-input-mode ref \
  --accept-risk
```

Example 3 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice apiKey \
  --anthropic-api-key "$ANTHROPIC_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Example 4 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice gemini-api-key \
  --gemini-api-key "$GEMINI_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

---

## Uninstall

**URL:** https://docs.openclaw.ai/install/uninstall

**Contents:**
- Uninstall
- Documentation Index
- ​Easy path (CLI still installed)
- ​Manual service removal (CLI not installed)
  - ​macOS (launchd)
  - ​Linux (systemd user unit)
  - ​Windows (Scheduled Task)
- ​Normal install vs source checkout
  - ​Normal install (install.sh / npm / pnpm / bun)
  - ​Source checkout (git clone)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw uninstall
```

Example 2 (unknown):
```unknown
openclaw uninstall --all --yes --non-interactive
npx -y openclaw uninstall --all --yes --non-interactive
```

Example 3 (unknown):
```unknown
openclaw gateway stop
```

Example 4 (unknown):
```unknown
openclaw gateway uninstall
```

---
