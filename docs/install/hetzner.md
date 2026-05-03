---
source_url: https://docs.openclaw.ai/install/hetzner
title: "Hetzner - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/hetzner#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Hosting

Hetzner

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [OpenClaw on Hetzner (Docker, Production VPS Guide)](https://docs.openclaw.ai/install/hetzner#openclaw-on-hetzner-docker-production-vps-guide)
- [Goal](https://docs.openclaw.ai/install/hetzner#goal)
- [What are we doing (simple terms)?](https://docs.openclaw.ai/install/hetzner#what-are-we-doing-simple-terms-)
- [Quick path (experienced operators)](https://docs.openclaw.ai/install/hetzner#quick-path-experienced-operators)
- [What you need](https://docs.openclaw.ai/install/hetzner#what-you-need)
- [Infrastructure as Code (Terraform)](https://docs.openclaw.ai/install/hetzner#infrastructure-as-code-terraform)
- [Next steps](https://docs.openclaw.ai/install/hetzner#next-steps)
- [Related](https://docs.openclaw.ai/install/hetzner#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/install/hetzner\#openclaw-on-hetzner-docker-production-vps-guide)  OpenClaw on Hetzner (Docker, Production VPS Guide)

## [​](https://docs.openclaw.ai/install/hetzner\#goal)  Goal

Run a persistent OpenClaw Gateway on a Hetzner VPS using Docker, with durable state, baked-in binaries, and safe restart behavior.If you want “OpenClaw 24/7 for ~$5”, this is the simplest reliable setup.
Hetzner pricing changes; pick the smallest Debian/Ubuntu VPS and scale up if you hit OOMs.Security model reminder:

- Company-shared agents are fine when everyone is in the same trust boundary and the runtime is business-only.
- Keep strict separation: dedicated VPS/runtime + dedicated accounts; no personal Apple/Google/browser/password-manager profiles on that host.
- If users are adversarial to each other, split by gateway/host/OS user.

See [Security](https://docs.openclaw.ai/gateway/security) and [VPS hosting](https://docs.openclaw.ai/vps).

## [​](https://docs.openclaw.ai/install/hetzner\#what-are-we-doing-simple-terms-)  What are we doing (simple terms)?

- Rent a small Linux server (Hetzner VPS)
- Install Docker (isolated app runtime)
- Start the OpenClaw Gateway in Docker
- Persist `~/.openclaw` \+ `~/.openclaw/workspace` on the host (survives restarts/rebuilds)
- Access the Control UI from your laptop via an SSH tunnel

That mounted `~/.openclaw` state includes `openclaw.json`, per-agent
`agents/<agentId>/agent/auth-profiles.json`, and `.env`.The Gateway can be accessed via:

- SSH port forwarding from your laptop
- Direct port exposure if you manage firewalling and tokens yourself

This guide assumes Ubuntu or Debian on Hetzner.

If you are on another Linux VPS, map packages accordingly.
For the generic Docker flow, see [Docker](https://docs.openclaw.ai/install/docker).

* * *

## [​](https://docs.openclaw.ai/install/hetzner\#quick-path-experienced-operators)  Quick path (experienced operators)

1. Provision Hetzner VPS
2. Install Docker
3. Clone OpenClaw repository
4. Create persistent host directories
5. Configure `.env` and `docker-compose.yml`
6. Bake required binaries into the image
7. `docker compose up -d`
8. Verify persistence and Gateway access

* * *

## [​](https://docs.openclaw.ai/install/hetzner\#what-you-need)  What you need

- Hetzner VPS with root access
- SSH access from your laptop
- Basic comfort with SSH + copy/paste
- ~20 minutes
- Docker and Docker Compose
- Model auth credentials
- Optional provider credentials
  - WhatsApp QR
  - Telegram bot token
  - Gmail OAuth

* * *

1

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Provision the VPS

Create an Ubuntu or Debian VPS in Hetzner.Connect as root:

```
ssh root@YOUR_VPS_IP
```

This guide assumes the VPS is stateful.
Do not treat it as disposable infrastructure.

2

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Install Docker (on the VPS)

```
apt-get update
apt-get install -y git curl ca-certificates
curl -fsSL https://get.docker.com | sh
```

Verify:

```
docker --version
docker compose version
```

3

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Clone the OpenClaw repository

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
```

This guide assumes you will build a custom image to guarantee binary persistence.

4

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Create persistent host directories

Docker containers are ephemeral.
All long-lived state must live on the host.

```
mkdir -p /root/.openclaw/workspace

# Set ownership to the container user (uid 1000):
chown -R 1000:1000 /root/.openclaw
```

5

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Configure environment variables

Create `.env` in the repository root.

```
OPENCLAW_IMAGE=openclaw:latest
OPENCLAW_GATEWAY_TOKEN=
OPENCLAW_GATEWAY_BIND=lan
OPENCLAW_GATEWAY_PORT=18789

OPENCLAW_CONFIG_DIR=/root/.openclaw
OPENCLAW_WORKSPACE_DIR=/root/.openclaw/workspace

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

6

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

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
      # Recommended: keep the Gateway loopback-only on the VPS; access via SSH tunnel.
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

7

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Shared Docker VM runtime steps

Use the shared runtime guide for the common Docker host flow:

- [Bake required binaries into the image](https://docs.openclaw.ai/install/docker-vm-runtime#bake-required-binaries-into-the-image)
- [Build and launch](https://docs.openclaw.ai/install/docker-vm-runtime#build-and-launch)
- [What persists where](https://docs.openclaw.ai/install/docker-vm-runtime#what-persists-where)
- [Updates](https://docs.openclaw.ai/install/docker-vm-runtime#updates)

8

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Hetzner-specific access

After the shared build and launch steps, complete the following setup to open the tunnel:**Prerequisite:** Ensure your VPS sshd config allows TCP forwarding. If you
have hardened your SSH config, check `/etc/ssh/sshd_config` and set:

```
AllowTcpForwarding local
```

`local` allows `ssh -L` local forwards from your laptop while blocking
remote forwards from the server. Setting it to `no` will fail the tunnel
with:
`channel 3: open failed: administratively prohibited: open failed`After confirming TCP forwarding is enabled, restart the SSH service
(`systemctl restart ssh`) and run the tunnel from your laptop:

```
ssh -N -L 18789:127.0.0.1:18789 root@YOUR_VPS_IP
```

Open:`http://127.0.0.1:18789/`Paste the configured shared secret. This guide uses the gateway token by
default; if you switched to password auth, use that password instead.

The shared persistence map lives in [Docker VM Runtime](https://docs.openclaw.ai/install/docker-vm-runtime#what-persists-where).

## [​](https://docs.openclaw.ai/install/hetzner\#infrastructure-as-code-terraform)  Infrastructure as Code (Terraform)

For teams preferring infrastructure-as-code workflows, a community-maintained Terraform setup provides:

- Modular Terraform configuration with remote state management
- Automated provisioning via cloud-init
- Deployment scripts (bootstrap, deploy, backup/restore)
- Security hardening (firewall, UFW, SSH-only access)
- SSH tunnel configuration for gateway access

**Repositories:**

- Infrastructure: [openclaw-terraform-hetzner](https://github.com/andreesg/openclaw-terraform-hetzner)
- Docker config: [openclaw-docker-config](https://github.com/andreesg/openclaw-docker-config)

This approach complements the Docker setup above with reproducible deployments, version-controlled infrastructure, and automated disaster recovery.

Community-maintained. For issues or contributions, see the repository links above.

## [​](https://docs.openclaw.ai/install/hetzner\#next-steps)  Next steps

- Set up messaging channels: [Channels](https://docs.openclaw.ai/channels)
- Configure the Gateway: [Gateway configuration](https://docs.openclaw.ai/gateway/configuration)
- Keep OpenClaw up to date: [Updating](https://docs.openclaw.ai/install/updating)

## [​](https://docs.openclaw.ai/install/hetzner\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [Fly.io](https://docs.openclaw.ai/install/fly)
- [Docker](https://docs.openclaw.ai/install/docker)
- [VPS hosting](https://docs.openclaw.ai/vps)

[GCP](https://docs.openclaw.ai/install/gcp) [Hostinger](https://docs.openclaw.ai/install/hostinger)

Ctrl+I