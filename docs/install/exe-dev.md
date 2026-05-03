---
source_url: https://docs.openclaw.ai/install/exe-dev
title: "exe.dev - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/exe-dev#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Hosting

exe.dev

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Beginner quick path](https://docs.openclaw.ai/install/exe-dev#beginner-quick-path)
- [What you need](https://docs.openclaw.ai/install/exe-dev#what-you-need)
- [Automated install with Shelley](https://docs.openclaw.ai/install/exe-dev#automated-install-with-shelley)
- [Manual installation](https://docs.openclaw.ai/install/exe-dev#manual-installation)
- [1) Create the VM](https://docs.openclaw.ai/install/exe-dev#1-create-the-vm)
- [2) Install prerequisites (on the VM)](https://docs.openclaw.ai/install/exe-dev#2-install-prerequisites-on-the-vm)
- [3) Install OpenClaw](https://docs.openclaw.ai/install/exe-dev#3-install-openclaw)
- [4) Setup nginx to proxy OpenClaw to port 8000](https://docs.openclaw.ai/install/exe-dev#4-setup-nginx-to-proxy-openclaw-to-port-8000)
- [5) Access OpenClaw and grant privileges](https://docs.openclaw.ai/install/exe-dev#5-access-openclaw-and-grant-privileges)
- [Remote channel setup](https://docs.openclaw.ai/install/exe-dev#remote-channel-setup)
- [Remote access](https://docs.openclaw.ai/install/exe-dev#remote-access)
- [Updating](https://docs.openclaw.ai/install/exe-dev#updating)
- [Related](https://docs.openclaw.ai/install/exe-dev#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Goal: OpenClaw Gateway running on an exe.dev VM, reachable from your laptop via: `https://<vm-name>.exe.xyz`This page assumes exe.dev’s default **exeuntu** image. If you picked a different distro, map packages accordingly.

## [​](https://docs.openclaw.ai/install/exe-dev\#beginner-quick-path)  Beginner quick path

1. [https://exe.new/openclaw](https://exe.new/openclaw)
2. Fill in your auth key/token as needed
3. Click on “Agent” next to your VM and wait for Shelley to finish provisioning
4. Open `https://<vm-name>.exe.xyz/` and authenticate with the configured shared secret (this guide uses token auth by default, but password auth works too if you switch `gateway.auth.mode`)
5. Approve any pending device pairing requests with `openclaw devices approve <requestId>`

## [​](https://docs.openclaw.ai/install/exe-dev\#what-you-need)  What you need

- exe.dev account
- `ssh exe.dev` access to [exe.dev](https://exe.dev/) virtual machines (optional)

## [​](https://docs.openclaw.ai/install/exe-dev\#automated-install-with-shelley)  Automated install with Shelley

Shelley, [exe.dev](https://exe.dev/)’s agent, can install OpenClaw instantly with our
prompt. The prompt used is as below:

```
Set up OpenClaw (https://docs.openclaw.ai/install) on this VM. Use the non-interactive and accept-risk flags for openclaw onboarding. Add the supplied auth or token as needed. Configure nginx to forward from the default port 18789 to the root location on the default enabled site config, making sure to enable Websocket support. Pairing is done by "openclaw devices list" and "openclaw devices approve <request id>". Make sure the dashboard shows that OpenClaw's health is OK. exe.dev handles forwarding from port 8000 to port 80/443 and HTTPS for us, so the final "reachable" should be <vm-name>.exe.xyz, without port specification.
```

## [​](https://docs.openclaw.ai/install/exe-dev\#manual-installation)  Manual installation

## [​](https://docs.openclaw.ai/install/exe-dev\#1-create-the-vm)  1) Create the VM

From your device:

```
ssh exe.dev new
```

Then connect:

```
ssh <vm-name>.exe.xyz
```

Keep this VM **stateful**. OpenClaw stores `openclaw.json`, per-agent `auth-profiles.json`, sessions, and channel/provider state under `~/.openclaw/`, plus the workspace under `~/.openclaw/workspace/`.

## [​](https://docs.openclaw.ai/install/exe-dev\#2-install-prerequisites-on-the-vm)  2) Install prerequisites (on the VM)

```
sudo apt-get update
sudo apt-get install -y git curl jq ca-certificates openssl
```

## [​](https://docs.openclaw.ai/install/exe-dev\#3-install-openclaw)  3) Install OpenClaw

Run the OpenClaw install script:

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

## [​](https://docs.openclaw.ai/install/exe-dev\#4-setup-nginx-to-proxy-openclaw-to-port-8000)  4) Setup nginx to proxy OpenClaw to port 8000

Edit `/etc/nginx/sites-enabled/default` with

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    listen 8000;
    listen [::]:8000;

    server_name _;

    location / {
        proxy_pass http://127.0.0.1:18789;
        proxy_http_version 1.1;

        # WebSocket support
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        # Standard proxy headers
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Timeout settings for long-lived connections
        proxy_read_timeout 86400s;
        proxy_send_timeout 86400s;
    }
}
```

Overwrite forwarding headers instead of preserving client-supplied chains.
OpenClaw trusts forwarded IP metadata only from explicitly configured proxies,
and append-style `X-Forwarded-For` chains are treated as a hardening risk.

## [​](https://docs.openclaw.ai/install/exe-dev\#5-access-openclaw-and-grant-privileges)  5) Access OpenClaw and grant privileges

Access `https://<vm-name>.exe.xyz/` (see the Control UI output from onboarding). If it prompts for auth, paste the
configured shared secret from the VM. This guide uses token auth, so retrieve `gateway.auth.token`
with `openclaw config get gateway.auth.token` (or generate one with `openclaw doctor --generate-gateway-token`).
If you changed the gateway to password auth, use `gateway.auth.password` / `OPENCLAW_GATEWAY_PASSWORD` instead.
Approve devices with `openclaw devices list` and `openclaw devices approve <requestId>`. When in doubt, use Shelley from your browser!

## [​](https://docs.openclaw.ai/install/exe-dev\#remote-channel-setup)  Remote channel setup

For remote hosts, prefer one `config patch` call over many SSH calls to `config set`. Keep real tokens in the VM environment or `~/.openclaw/.env`, and put only SecretRefs in `openclaw.json`.On the VM, make the service environment contain the secrets it needs:

```
cat >> ~/.openclaw/.env <<'EOF'
SLACK_BOT_TOKEN=xoxb-...
SLACK_APP_TOKEN=xapp-...
DISCORD_BOT_TOKEN=...
OPENAI_API_KEY=sk-...
EOF
```

From your local machine, create a patch file and pipe it to the VM:

```
// openclaw.remote.patch.json5
{
  secrets: {
    providers: {
      default: { source: "env" },
    },
  },
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

```
ssh <vm-name>.exe.xyz 'openclaw config patch --stdin --dry-run' < ./openclaw.remote.patch.json5
ssh <vm-name>.exe.xyz 'openclaw config patch --stdin' < ./openclaw.remote.patch.json5
ssh <vm-name>.exe.xyz 'openclaw gateway restart && openclaw health'
```

Use `--replace-path` when a nested allowlist should become exactly the patch value, for example when replacing a Discord channel allowlist:

```
ssh <vm-name>.exe.xyz 'openclaw config patch --stdin --replace-path "channels.discord.guilds[\"123\"].channels"' < ./discord.patch.json5
```

## [​](https://docs.openclaw.ai/install/exe-dev\#remote-access)  Remote access

Remote access is handled by [exe.dev](https://exe.dev/)’s authentication. By
default, HTTP traffic from port 8000 is forwarded to `https://<vm-name>.exe.xyz`
with email auth.

## [​](https://docs.openclaw.ai/install/exe-dev\#updating)  Updating

```
npm i -g openclaw@latest
openclaw doctor
openclaw gateway restart
openclaw health
```

Guide: [Updating](https://docs.openclaw.ai/install/updating)

## [​](https://docs.openclaw.ai/install/exe-dev\#related)  Related

- [Remote gateway](https://docs.openclaw.ai/gateway/remote)
- [Install overview](https://docs.openclaw.ai/install)

[Docker VM runtime](https://docs.openclaw.ai/install/docker-vm-runtime) [Fly.io](https://docs.openclaw.ai/install/fly)

Ctrl+I