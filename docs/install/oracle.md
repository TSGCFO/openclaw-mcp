---
source_url: https://docs.openclaw.ai/install/oracle
title: "Oracle Cloud - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/oracle#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Hosting

Oracle Cloud

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Prerequisites](https://docs.openclaw.ai/install/oracle#prerequisites)
- [Setup](https://docs.openclaw.ai/install/oracle#setup)
- [Fallback: SSH tunnel](https://docs.openclaw.ai/install/oracle#fallback-ssh-tunnel)
- [Troubleshooting](https://docs.openclaw.ai/install/oracle#troubleshooting)
- [Next steps](https://docs.openclaw.ai/install/oracle#next-steps)
- [Related](https://docs.openclaw.ai/install/oracle#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Run a persistent OpenClaw Gateway on Oracle Cloud’s **Always Free** ARM tier (up to 4 OCPU, 24 GB RAM, 200 GB storage) at no cost.

## [​](https://docs.openclaw.ai/install/oracle\#prerequisites)  Prerequisites

- Oracle Cloud account ( [signup](https://www.oracle.com/cloud/free/)) — see [community signup guide](https://gist.github.com/rssnyder/51e3cfedd730e7dd5f4a816143b25dbd) if you hit issues
- Tailscale account (free at [tailscale.com](https://tailscale.com/))
- An SSH key pair
- About 30 minutes

## [​](https://docs.openclaw.ai/install/oracle\#setup)  Setup

1

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Create an OCI instance

1. Log into [Oracle Cloud Console](https://cloud.oracle.com/).
2. Navigate to **Compute > Instances > Create Instance**.
3. Configure:
   - **Name:**`openclaw`
   - **Image:** Ubuntu 24.04 (aarch64)
   - **Shape:**`VM.Standard.A1.Flex` (Ampere ARM)
   - **OCPUs:** 2 (or up to 4)
   - **Memory:** 12 GB (or up to 24 GB)
   - **Boot volume:** 50 GB (up to 200 GB free)
   - **SSH key:** Add your public key
4. Click **Create** and note the public IP address.

If instance creation fails with “Out of capacity”, try a different availability domain or retry later. Free tier capacity is limited.

2

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Connect and update the system

```
ssh ubuntu@YOUR_PUBLIC_IP

sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential
```

`build-essential` is required for ARM compilation of some dependencies.

3

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Configure user and hostname

```
sudo hostnamectl set-hostname openclaw
sudo passwd ubuntu
sudo loginctl enable-linger ubuntu
```

Enabling linger keeps user services running after logout.

4

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Install Tailscale

```
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up --ssh --hostname=openclaw
```

From now on, connect via Tailscale: `ssh ubuntu@openclaw`.

5

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Install OpenClaw

```
curl -fsSL https://openclaw.ai/install.sh | bash
source ~/.bashrc
```

When prompted “How do you want to hatch your bot?”, select **Do this later**.

6

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Configure the gateway

Use token auth with Tailscale Serve for secure remote access.

```
openclaw config set gateway.bind loopback
openclaw config set gateway.auth.mode token
openclaw doctor --generate-gateway-token
openclaw config set gateway.tailscale.mode serve
openclaw config set gateway.trustedProxies '["127.0.0.1"]'

systemctl --user restart openclaw-gateway.service
```

`gateway.trustedProxies=["127.0.0.1"]` here is only for the local Tailscale Serve proxy’s forwarded-IP/local-client handling. It is **not**`gateway.auth.mode: "trusted-proxy"`. Diff viewer routes keep fail-closed behavior in this setup: raw `127.0.0.1` viewer requests without forwarded proxy headers can return `Diff not found`. Use `mode=file` / `mode=both` for attachments, or intentionally enable remote viewers and set `plugins.entries.diffs.config.viewerBaseUrl` (or pass a proxy `baseUrl`) if you need shareable viewer links.

7

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Lock down VCN security

Block all traffic except Tailscale at the network edge:

1. Go to **Networking > Virtual Cloud Networks** in the OCI Console.
2. Click your VCN, then **Security Lists > Default Security List**.
3. **Remove** all ingress rules except `0.0.0.0/0 UDP 41641` (Tailscale).
4. Keep default egress rules (allow all outbound).

This blocks SSH on port 22, HTTP, HTTPS, and everything else at the network edge. You can only connect via Tailscale from this point on.

8

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Verify

```
openclaw --version
systemctl --user status openclaw-gateway.service
tailscale serve status
curl http://localhost:18789
```

Access the Control UI from any device on your tailnet:

```
https://openclaw.<tailnet-name>.ts.net/
```

Replace `<tailnet-name>` with your tailnet name (visible in `tailscale status`).

## [​](https://docs.openclaw.ai/install/oracle\#fallback-ssh-tunnel)  Fallback: SSH tunnel

If Tailscale Serve is not working, use an SSH tunnel from your local machine:

```
ssh -L 18789:127.0.0.1:18789 ubuntu@openclaw
```

Then open `http://localhost:18789`.

## [​](https://docs.openclaw.ai/install/oracle\#troubleshooting)  Troubleshooting

**Instance creation fails (“Out of capacity”)** — Free tier ARM instances are popular. Try a different availability domain or retry during off-peak hours.**Tailscale will not connect** — Run `sudo tailscale up --ssh --hostname=openclaw --reset` to re-authenticate.**Gateway will not start** — Run `openclaw doctor --non-interactive` and check logs with `journalctl --user -u openclaw-gateway.service -n 50`.**ARM binary issues** — Most npm packages work on ARM64. For native binaries, look for `linux-arm64` or `aarch64` releases. Verify architecture with `uname -m`.

## [​](https://docs.openclaw.ai/install/oracle\#next-steps)  Next steps

- [Channels](https://docs.openclaw.ai/channels) — connect Telegram, WhatsApp, Discord, and more
- [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) — all config options
- [Updating](https://docs.openclaw.ai/install/updating) — keep OpenClaw up to date

## [​](https://docs.openclaw.ai/install/oracle\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [GCP](https://docs.openclaw.ai/install/gcp)
- [VPS hosting](https://docs.openclaw.ai/vps)

[Northflank](https://docs.openclaw.ai/install/northflank) [Railway](https://docs.openclaw.ai/install/railway)

Ctrl+I