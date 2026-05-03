---
source_url: https://docs.openclaw.ai/install/gcp
title: "GCP - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/gcp#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Hosting

GCP

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [OpenClaw on GCP Compute Engine (Docker, Production VPS Guide)](https://docs.openclaw.ai/install/gcp#openclaw-on-gcp-compute-engine-docker-production-vps-guide)
- [Goal](https://docs.openclaw.ai/install/gcp#goal)
- [What are we doing (simple terms)?](https://docs.openclaw.ai/install/gcp#what-are-we-doing-simple-terms-)
- [Quick path (experienced operators)](https://docs.openclaw.ai/install/gcp#quick-path-experienced-operators)
- [What you need](https://docs.openclaw.ai/install/gcp#what-you-need)
- [Troubleshooting](https://docs.openclaw.ai/install/gcp#troubleshooting)
- [Service accounts (security best practice)](https://docs.openclaw.ai/install/gcp#service-accounts-security-best-practice)
- [Next steps](https://docs.openclaw.ai/install/gcp#next-steps)
- [Related](https://docs.openclaw.ai/install/gcp#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/install/gcp\#openclaw-on-gcp-compute-engine-docker-production-vps-guide)  OpenClaw on GCP Compute Engine (Docker, Production VPS Guide)

## [​](https://docs.openclaw.ai/install/gcp\#goal)  Goal

Run a persistent OpenClaw Gateway on a GCP Compute Engine VM using Docker, with durable state, baked-in binaries, and safe restart behavior.If you want “OpenClaw 24/7 for ~$5-12/mo”, this is a reliable setup on Google Cloud.
Pricing varies by machine type and region; pick the smallest VM that fits your workload and scale up if you hit OOMs.

## [​](https://docs.openclaw.ai/install/gcp\#what-are-we-doing-simple-terms-)  What are we doing (simple terms)?

- Create a GCP project and enable billing
- Create a Compute Engine VM
- Install Docker (isolated app runtime)
- Start the OpenClaw Gateway in Docker
- Persist `~/.openclaw` \+ `~/.openclaw/workspace` on the host (survives restarts/rebuilds)
- Access the Control UI from your laptop via an SSH tunnel

That mounted `~/.openclaw` state includes `openclaw.json`, per-agent
`agents/<agentId>/agent/auth-profiles.json`, and `.env`.The Gateway can be accessed via:

- SSH port forwarding from your laptop
- Direct port exposure if you manage firewalling and tokens yourself

This guide uses Debian on GCP Compute Engine.
Ubuntu also works; map packages accordingly.
For the generic Docker flow, see [Docker](https://docs.openclaw.ai/install/docker).

* * *

## [​](https://docs.openclaw.ai/install/gcp\#quick-path-experienced-operators)  Quick path (experienced operators)

1. Create GCP project + enable Compute Engine API
2. Create Compute Engine VM (e2-small, Debian 12, 20GB)
3. SSH into the VM
4. Install Docker
5. Clone OpenClaw repository
6. Create persistent host directories
7. Configure `.env` and `docker-compose.yml`
8. Bake required binaries, build, and launch

* * *

## [​](https://docs.openclaw.ai/install/gcp\#what-you-need)  What you need

- GCP account (free tier eligible for e2-micro)
- gcloud CLI installed (or use Cloud Console)
- SSH access from your laptop
- Basic comfort with SSH + copy/paste
- ~20-30 minutes
- Docker and Docker Compose
- Model auth credentials
- Optional provider credentials
  - WhatsApp QR
  - Telegram bot token
  - Gmail OAuth

* * *

1

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Install gcloud CLI (or use Console)

**Option A: gcloud CLI** (recommended for automation)Install from [https://cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install)Initialize and authenticate:

```
gcloud init
gcloud auth login
```

**Option B: Cloud Console**All steps can be done via the web UI at [https://console.cloud.google.com](https://console.cloud.google.com/)

2

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Create a GCP project

**CLI:**

```
gcloud projects create my-openclaw-project --name="OpenClaw Gateway"
gcloud config set project my-openclaw-project
```

Enable billing at [https://console.cloud.google.com/billing](https://console.cloud.google.com/billing) (required for Compute Engine).Enable the Compute Engine API:

```
gcloud services enable compute.googleapis.com
```

**Console:**

1. Go to IAM & Admin > Create Project
2. Name it and create
3. Enable billing for the project
4. Navigate to APIs & Services > Enable APIs > search “Compute Engine API” > Enable

3

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Create the VM

**Machine types:**

| Type | Specs | Cost | Notes |
| --- | --- | --- | --- |
| e2-medium | 2 vCPU, 4GB RAM | ~$25/mo | Most reliable for local Docker builds |
| e2-small | 2 vCPU, 2GB RAM | ~$12/mo | Minimum recommended for Docker build |
| e2-micro | 2 vCPU (shared), 1GB RAM | Free tier eligible | Often fails with Docker build OOM (exit 137) |

**CLI:**

```
gcloud compute instances create openclaw-gateway \
  --zone=us-central1-a \
  --machine-type=e2-small \
  --boot-disk-size=20GB \
  --image-family=debian-12 \
  --image-project=debian-cloud
```

**Console:**

1. Go to Compute Engine > VM instances > Create instance
2. Name: `openclaw-gateway`
3. Region: `us-central1`, Zone: `us-central1-a`
4. Machine type: `e2-small`
5. Boot disk: Debian 12, 20GB
6. Create

4

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

SSH into the VM

**CLI:**

```
gcloud compute ssh openclaw-gateway --zone=us-central1-a
```

**Console:**Click the “SSH” button next to your VM in the Compute Engine dashboard.Note: SSH key propagation can take 1-2 minutes after VM creation. If connection is refused, wait and retry.

5

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Install Docker (on the VM)

```
sudo apt-get update
sudo apt-get install -y git curl ca-certificates
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker $USER
```

Log out and back in for the group change to take effect:

```
exit
```

Then SSH back in:

```
gcloud compute ssh openclaw-gateway --zone=us-central1-a
```

Verify:

```
docker --version
docker compose version
```

6

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Clone the OpenClaw repository

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
```

This guide assumes you will build a custom image to guarantee binary persistence.

7

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Create persistent host directories

Docker containers are ephemeral.
All long-lived state must live on the host.

```
mkdir -p ~/.openclaw
mkdir -p ~/.openclaw/workspace
```

8

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Configure environment variables

Create `.env` in the repository root.

```
OPENCLAW_IMAGE=openclaw:latest
OPENCLAW_GATEWAY_TOKEN=
OPENCLAW_GATEWAY_BIND=lan
OPENCLAW_GATEWAY_PORT=18789

OPENCLAW_CONFIG_DIR=/home/$USER/.openclaw
OPENCLAW_WORKSPACE_DIR=/home/$USER/.openclaw/workspace

GOG_KEYRING_PASSWORD=
XDG_CONFIG_HOME=/home/node/.openclaw
```

Leave `OPENCLAW_GATEWAY_TOKEN` blank unless you explicitly want to
manage it through `.env`; OpenClaw writes a random gateway token to
config on first start. Generate a keyring password and paste it into
`GOG_KEYRING_PASSWORD`:

```
openssl rand -hex 32
```

**Do not commit this file.**This `.env` file is for container/runtime env such as `OPENCLAW_GATEWAY_TOKEN`.
Stored provider OAuth/API-key auth lives in the mounted
`~/.openclaw/agents/<agentId>/agent/auth-profiles.json`.

9

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Docker Compose configuration

Create or update `docker-compose.yml`.

```
services:
  openclaw-gateway:
    image: ${OPENCLAW_IMAGE}
    build: .
    restart: unless-stopped
    env_file:
      - .env
    environment:
      - HOME=/home/node
      - NODE_ENV=production
      - TERM=xterm-256color
      - OPENCLAW_GATEWAY_BIND=${OPENCLAW_GATEWAY_BIND}
      - OPENCLAW_GATEWAY_PORT=${OPENCLAW_GATEWAY_PORT}
      - OPENCLAW_GATEWAY_TOKEN=${OPENCLAW_GATEWAY_TOKEN}
      - GOG_KEYRING_PASSWORD=${GOG_KEYRING_PASSWORD}
      - XDG_CONFIG_HOME=${XDG_CONFIG_HOME}
      - PATH=/home/linuxbrew/.linuxbrew/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    volumes:
      - ${OPENCLAW_CONFIG_DIR}:/home/node/.openclaw
      - ${OPENCLAW_WORKSPACE_DIR}:/home/node/.openclaw/workspace
    ports:
      # Recommended: keep the Gateway loopback-only on the VM; access via SSH tunnel.
      # To expose it publicly, remove the `127.0.0.1:` prefix and firewall accordingly.
      - "127.0.0.1:${OPENCLAW_GATEWAY_PORT}:18789"
    command:
      [\
        "node",\
        "dist/index.js",\
        "gateway",\
        "--bind",\
        "${OPENCLAW_GATEWAY_BIND}",\
        "--port",\
        "${OPENCLAW_GATEWAY_PORT}",\
        "--allow-unconfigured",\
      ]
```

`--allow-unconfigured` is only for bootstrap convenience, it is not a replacement for a proper gateway configuration. Still set auth (`gateway.auth.token` or password) and use safe bind settings for your deployment.

10

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Shared Docker VM runtime steps

Use the shared runtime guide for the common Docker host flow:

- [Bake required binaries into the image](https://docs.openclaw.ai/install/docker-vm-runtime#bake-required-binaries-into-the-image)
- [Build and launch](https://docs.openclaw.ai/install/docker-vm-runtime#build-and-launch)
- [What persists where](https://docs.openclaw.ai/install/docker-vm-runtime#what-persists-where)
- [Updates](https://docs.openclaw.ai/install/docker-vm-runtime#updates)

11

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

GCP-specific launch notes

On GCP, if build fails with `Killed` or `exit code 137` during `pnpm install --frozen-lockfile`, the VM is out of memory. Use `e2-small` minimum, or `e2-medium` for more reliable first builds.When binding to LAN (`OPENCLAW_GATEWAY_BIND=lan`), configure a trusted browser origin before continuing:

```
docker compose run --rm openclaw-cli config set gateway.controlUi.allowedOrigins '["http://127.0.0.1:18789"]' --strict-json
```

If you changed the gateway port, replace `18789` with your configured port.

12

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Access from your laptop

Create an SSH tunnel to forward the Gateway port:

```
gcloud compute ssh openclaw-gateway --zone=us-central1-a -- -L 18789:127.0.0.1:18789
```

Open in your browser:`http://127.0.0.1:18789/`Reprint a clean dashboard link:

```
docker compose run --rm openclaw-cli dashboard --no-open
```

If the UI prompts for shared-secret auth, paste the configured token or
password into Control UI settings. This Docker flow writes a token by
default; if you switch the container config to password auth, use that
password instead.If Control UI shows `unauthorized` or `disconnected (1008): pairing required`, approve the browser device:

```
docker compose run --rm openclaw-cli devices list
docker compose run --rm openclaw-cli devices approve <requestId>
```

Need the shared persistence and update reference again?
See [Docker VM Runtime](https://docs.openclaw.ai/install/docker-vm-runtime#what-persists-where) and [Docker VM Runtime updates](https://docs.openclaw.ai/install/docker-vm-runtime#updates).

* * *

## [​](https://docs.openclaw.ai/install/gcp\#troubleshooting)  Troubleshooting

**SSH connection refused**SSH key propagation can take 1-2 minutes after VM creation. Wait and retry.**OS Login issues**Check your OS Login profile:

```
gcloud compute os-login describe-profile
```

Ensure your account has the required IAM permissions (Compute OS Login or Compute OS Admin Login).**Out of memory (OOM)**If Docker build fails with `Killed` and `exit code 137`, the VM was OOM-killed. Upgrade to e2-small (minimum) or e2-medium (recommended for reliable local builds):

```
# Stop the VM first
gcloud compute instances stop openclaw-gateway --zone=us-central1-a

# Change machine type
gcloud compute instances set-machine-type openclaw-gateway \
  --zone=us-central1-a \
  --machine-type=e2-small

# Start the VM
gcloud compute instances start openclaw-gateway --zone=us-central1-a
```

* * *

## [​](https://docs.openclaw.ai/install/gcp\#service-accounts-security-best-practice)  Service accounts (security best practice)

For personal use, your default user account works fine.For automation or CI/CD pipelines, create a dedicated service account with minimal permissions:

1. Create a service account:














```
gcloud iam service-accounts create openclaw-deploy \
     --display-name="OpenClaw Deployment"
```

2. Grant Compute Instance Admin role (or narrower custom role):














```
gcloud projects add-iam-policy-binding my-openclaw-project \
     --member="serviceAccount:openclaw-deploy@my-openclaw-project.iam.gserviceaccount.com" \
     --role="roles/compute.instanceAdmin.v1"
```


Avoid using the Owner role for automation. Use the principle of least privilege.See [https://cloud.google.com/iam/docs/understanding-roles](https://cloud.google.com/iam/docs/understanding-roles) for IAM role details.

* * *

## [​](https://docs.openclaw.ai/install/gcp\#next-steps)  Next steps

- Set up messaging channels: [Channels](https://docs.openclaw.ai/channels)
- Pair local devices as nodes: [Nodes](https://docs.openclaw.ai/nodes)
- Configure the Gateway: [Gateway configuration](https://docs.openclaw.ai/gateway/configuration)

## [​](https://docs.openclaw.ai/install/gcp\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [Azure](https://docs.openclaw.ai/install/azure)
- [VPS hosting](https://docs.openclaw.ai/vps)

[Fly.io](https://docs.openclaw.ai/install/fly) [Hetzner](https://docs.openclaw.ai/install/hetzner)

Ctrl+I