---
source_url: https://docs.openclaw.ai/vps
title: "Linux server - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/vps#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Hosting

Linux server

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Pick a provider](https://docs.openclaw.ai/vps#pick-a-provider)
- [How cloud setups work](https://docs.openclaw.ai/vps#how-cloud-setups-work)
- [Harden admin access first](https://docs.openclaw.ai/vps#harden-admin-access-first)
- [Shared company agent on a VPS](https://docs.openclaw.ai/vps#shared-company-agent-on-a-vps)
- [Using nodes with a VPS](https://docs.openclaw.ai/vps#using-nodes-with-a-vps)
- [Startup tuning for small VMs and ARM hosts](https://docs.openclaw.ai/vps#startup-tuning-for-small-vms-and-arm-hosts)
- [systemd tuning checklist (optional)](https://docs.openclaw.ai/vps#systemd-tuning-checklist-optional)
- [Related](https://docs.openclaw.ai/vps#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Run the OpenClaw Gateway on any Linux server or cloud VPS. This page helps you
pick a provider, explains how cloud deployments work, and covers generic Linux
tuning that applies everywhere.

## [​](https://docs.openclaw.ai/vps\#pick-a-provider)  Pick a provider

[**Railway** \\
\\
One-click, browser setup](https://docs.openclaw.ai/install/railway)

[**Northflank** \\
\\
One-click, browser setup](https://docs.openclaw.ai/install/northflank)

[**DigitalOcean** \\
\\
Simple paid VPS](https://docs.openclaw.ai/install/digitalocean)

[**Oracle Cloud** \\
\\
Always Free ARM tier](https://docs.openclaw.ai/install/oracle)

[**Fly.io** \\
\\
Fly Machines](https://docs.openclaw.ai/install/fly)

[**Hetzner** \\
\\
Docker on Hetzner VPS](https://docs.openclaw.ai/install/hetzner)

[**Hostinger** \\
\\
VPS with one-click setup](https://docs.openclaw.ai/install/hostinger)

[**GCP** \\
\\
Compute Engine](https://docs.openclaw.ai/install/gcp)

[**Azure** \\
\\
Linux VM](https://docs.openclaw.ai/install/azure)

[**exe.dev** \\
\\
VM with HTTPS proxy](https://docs.openclaw.ai/install/exe-dev)

[**Raspberry Pi** \\
\\
ARM self-hosted](https://docs.openclaw.ai/install/raspberry-pi)

**AWS (EC2 / Lightsail / free tier)** also works well.
A community video walkthrough is available at
[x.com/techfrenAJ/status/2014934471095812547](https://x.com/techfrenAJ/status/2014934471095812547)
(community resource — may become unavailable).

## [​](https://docs.openclaw.ai/vps\#how-cloud-setups-work)  How cloud setups work

- The **Gateway runs on the VPS** and owns state + workspace.
- You connect from your laptop or phone via the **Control UI** or **Tailscale/SSH**.
- Treat the VPS as the source of truth and **back up** the state + workspace regularly.
- Secure default: keep the Gateway on loopback and access it via SSH tunnel or Tailscale Serve.
If you bind to `lan` or `tailnet`, require `gateway.auth.token` or `gateway.auth.password`.

Related pages: [Gateway remote access](https://docs.openclaw.ai/gateway/remote), [Platforms hub](https://docs.openclaw.ai/platforms).

## [​](https://docs.openclaw.ai/vps\#harden-admin-access-first)  Harden admin access first

Before you install OpenClaw on a public VPS, decide how you want to administer
the box itself.

- If you want Tailnet-only admin access, install Tailscale first, join the VPS
to your tailnet, verify a second SSH session over the Tailscale IP or
MagicDNS name, then restrict public SSH.
- If you are not using Tailscale, apply the equivalent hardening for your SSH
path before exposing more services.
- This is separate from Gateway access. You can still keep OpenClaw bound to
loopback and use an SSH tunnel or Tailscale Serve for the dashboard.

Tailscale-specific Gateway options live in [Tailscale](https://docs.openclaw.ai/gateway/tailscale).

## [​](https://docs.openclaw.ai/vps\#shared-company-agent-on-a-vps)  Shared company agent on a VPS

Running a single agent for a team is a valid setup when every user is in the same trust boundary and the agent is business-only.

- Keep it on a dedicated runtime (VPS/VM/container + dedicated OS user/accounts).
- Do not sign that runtime into personal Apple/Google accounts or personal browser/password-manager profiles.
- If users are adversarial to each other, split by gateway/host/OS user.

Security model details: [Security](https://docs.openclaw.ai/gateway/security).

## [​](https://docs.openclaw.ai/vps\#using-nodes-with-a-vps)  Using nodes with a VPS

You can keep the Gateway in the cloud and pair **nodes** on your local devices
(Mac/iOS/Android/headless). Nodes provide local screen/camera/canvas and `system.run`
capabilities while the Gateway stays in the cloud.Docs: [Nodes](https://docs.openclaw.ai/nodes), [Nodes CLI](https://docs.openclaw.ai/cli/nodes).

## [​](https://docs.openclaw.ai/vps\#startup-tuning-for-small-vms-and-arm-hosts)  Startup tuning for small VMs and ARM hosts

If CLI commands feel slow on low-power VMs (or ARM hosts), enable Node’s module compile cache:

```
grep -q 'NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache' ~/.bashrc || cat >> ~/.bashrc <<'EOF'
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1
EOF
source ~/.bashrc
```

- `NODE_COMPILE_CACHE` improves repeated command startup times.
- `OPENCLAW_NO_RESPAWN=1` avoids extra startup overhead from a self-respawn path.
- First command run warms the cache; subsequent runs are faster.
- For Raspberry Pi specifics, see [Raspberry Pi](https://docs.openclaw.ai/install/raspberry-pi).

### [​](https://docs.openclaw.ai/vps\#systemd-tuning-checklist-optional)  systemd tuning checklist (optional)

For VM hosts using `systemd`, consider:

- Add service env for a stable startup path:
  - `OPENCLAW_NO_RESPAWN=1`
  - `NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache`
- Keep restart behavior explicit:
  - `Restart=always`
  - `RestartSec=2`
  - `TimeoutStartSec=90`
- Prefer SSD-backed disks for state/cache paths to reduce random-I/O cold-start penalties.

For the standard `openclaw onboard --install-daemon` path, edit the user unit:

```
systemctl --user edit openclaw-gateway.service
```

```
[Service]
Environment=OPENCLAW_NO_RESPAWN=1
Environment=NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
Restart=always
RestartSec=2
TimeoutStartSec=90
```

If you deliberately installed a system unit instead, edit
`openclaw-gateway.service` via `sudo systemctl edit openclaw-gateway.service`.How `Restart=` policies help automated recovery:
[systemd can automate service recovery](https://www.redhat.com/en/blog/systemd-automate-recovery).For Linux OOM behavior, child process victim selection, and `exit 137`
diagnostics, see [Linux memory pressure and OOM kills](https://docs.openclaw.ai/platforms/linux#memory-pressure-and-oom-kills).

## [​](https://docs.openclaw.ai/vps\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [DigitalOcean](https://docs.openclaw.ai/install/digitalocean)
- [Fly.io](https://docs.openclaw.ai/install/fly)
- [Hetzner](https://docs.openclaw.ai/install/hetzner)

[Kubernetes](https://docs.openclaw.ai/install/kubernetes) [macOS VMs](https://docs.openclaw.ai/install/macos-vm)

Ctrl+I