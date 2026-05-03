---
source_url: https://docs.openclaw.ai/install/docker-vm-runtime#build-and-launch
title: "Docker VM runtime - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/docker-vm-runtime#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Hosting

Docker VM runtime

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Bake required binaries into the image](https://docs.openclaw.ai/install/docker-vm-runtime#bake-required-binaries-into-the-image)
- [Build and launch](https://docs.openclaw.ai/install/docker-vm-runtime#build-and-launch)
- [What persists where](https://docs.openclaw.ai/install/docker-vm-runtime#what-persists-where)
- [Updates](https://docs.openclaw.ai/install/docker-vm-runtime#updates)
- [Related](https://docs.openclaw.ai/install/docker-vm-runtime#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Shared runtime steps for VM-based Docker installs such as GCP, Hetzner, and similar VPS providers.

## [​](https://docs.openclaw.ai/install/docker-vm-runtime\#bake-required-binaries-into-the-image)  Bake required binaries into the image

Installing binaries inside a running container is a trap.
Anything installed at runtime will be lost on restart.All external binaries required by skills must be installed at image build time.The examples below show three common binaries only:

- `gog` (from `gogcli`) for Gmail access
- `goplaces` for Google Places
- `wacli` for WhatsApp

These are examples, not a complete list.
You may install as many binaries as needed using the same pattern.If you add new skills later that depend on additional binaries, you must:

1. Update the Dockerfile
2. Rebuild the image
3. Restart the containers

**Example Dockerfile**

```
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

The URLs above are examples. For ARM-based VMs, choose the `arm64` assets. For reproducible builds, pin versioned release URLs.

## [​](https://docs.openclaw.ai/install/docker-vm-runtime\#build-and-launch)  Build and launch

```
docker compose build
docker compose up -d openclaw-gateway
```

If build fails with `Killed` or `exit code 137` during `pnpm install --frozen-lockfile`, the VM is out of memory.
Use a larger machine class before retrying.Verify binaries:

```
docker compose exec openclaw-gateway which gog
docker compose exec openclaw-gateway which goplaces
docker compose exec openclaw-gateway which wacli
```

Expected output:

```
/usr/local/bin/gog
/usr/local/bin/goplaces
/usr/local/bin/wacli
```

Verify Gateway:

```
docker compose logs -f openclaw-gateway
```

Expected output:

```
[gateway] listening on ws://0.0.0.0:18789
```

## [​](https://docs.openclaw.ai/install/docker-vm-runtime\#what-persists-where)  What persists where

OpenClaw runs in Docker, but Docker is not the source of truth.
All long-lived state must survive restarts, rebuilds, and reboots.

| Component | Location | Persistence mechanism | Notes |
| --- | --- | --- | --- |
| Gateway config | `/home/node/.openclaw/` | Host volume mount | Includes `openclaw.json`, `.env` |
| Model auth profiles | `/home/node/.openclaw/agents/` | Host volume mount | `agents/<agentId>/agent/auth-profiles.json` (OAuth, API keys) |
| Skill configs | `/home/node/.openclaw/skills/` | Host volume mount | Skill-level state |
| Agent workspace | `/home/node/.openclaw/workspace/` | Host volume mount | Code and agent artifacts |
| WhatsApp session | `/home/node/.openclaw/` | Host volume mount | Preserves QR login |
| Gmail keyring | `/home/node/.openclaw/` | Host volume + password | Requires `GOG_KEYRING_PASSWORD` |
| Plugin runtime deps | `/var/lib/openclaw/plugin-runtime-deps/` | Docker named volume | Generated bundled plugin deps and runtime mirrors |
| External binaries | `/usr/local/bin/` | Docker image | Must be baked at build time |
| Node runtime | Container filesystem | Docker image | Rebuilt every image build |
| OS packages | Container filesystem | Docker image | Do not install at runtime |
| Docker container | Ephemeral | Restartable | Safe to destroy |

## [​](https://docs.openclaw.ai/install/docker-vm-runtime\#updates)  Updates

To update OpenClaw on the VM:

```
git pull
docker compose build
docker compose up -d
```

## [​](https://docs.openclaw.ai/install/docker-vm-runtime\#related)  Related

- [Docker](https://docs.openclaw.ai/install/docker)
- [Podman](https://docs.openclaw.ai/install/podman)
- [ClawDock](https://docs.openclaw.ai/install/clawdock)

[DigitalOcean](https://docs.openclaw.ai/install/digitalocean) [exe.dev](https://docs.openclaw.ai/install/exe-dev)

Ctrl+I