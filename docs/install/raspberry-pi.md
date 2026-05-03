---
source_url: https://docs.openclaw.ai/install/raspberry-pi
title: "Raspberry Pi - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/raspberry-pi#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Hosting

Raspberry Pi

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Prerequisites](https://docs.openclaw.ai/install/raspberry-pi#prerequisites)
- [Setup](https://docs.openclaw.ai/install/raspberry-pi#setup)
- [Performance tips](https://docs.openclaw.ai/install/raspberry-pi#performance-tips)
- [Troubleshooting](https://docs.openclaw.ai/install/raspberry-pi#troubleshooting)
- [Next steps](https://docs.openclaw.ai/install/raspberry-pi#next-steps)
- [Related](https://docs.openclaw.ai/install/raspberry-pi#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Run a persistent, always-on OpenClaw Gateway on a Raspberry Pi. Since the Pi is just the gateway (models run in the cloud via API), even a modest Pi handles the workload well.

## [​](https://docs.openclaw.ai/install/raspberry-pi\#prerequisites)  Prerequisites

- Raspberry Pi 4 or 5 with 2 GB+ RAM (4 GB recommended)
- MicroSD card (16 GB+) or USB SSD (better performance)
- Official Pi power supply
- Network connection (Ethernet or WiFi)
- 64-bit Raspberry Pi OS (required — do not use 32-bit)
- About 30 minutes

## [​](https://docs.openclaw.ai/install/raspberry-pi\#setup)  Setup

1

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Flash the OS

Use **Raspberry Pi OS Lite (64-bit)** — no desktop needed for a headless server.

1. Download [Raspberry Pi Imager](https://www.raspberrypi.com/software/).
2. Choose OS: **Raspberry Pi OS Lite (64-bit)**.
3. In the settings dialog, pre-configure:
   - Hostname: `gateway-host`
   - Enable SSH
   - Set username and password
   - Configure WiFi (if not using Ethernet)
4. Flash to your SD card or USB drive, insert it, and boot the Pi.

2

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Connect via SSH

```
ssh user@gateway-host
```

3

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Update the system

```
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl build-essential

# Set timezone (important for cron and reminders)
sudo timedatectl set-timezone America/Chicago
```

4

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Install Node.js 24

```
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install -y nodejs
node --version
```

5

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Add swap (important for 2 GB or less)

```
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Reduce swappiness for low-RAM devices
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

6

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Install OpenClaw

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

7

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Run onboarding

```
openclaw onboard --install-daemon
```

Follow the wizard. API keys are recommended over OAuth for headless devices. Telegram is the easiest channel to start with.

8

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Verify

```
openclaw status
systemctl --user status openclaw-gateway.service
journalctl --user -u openclaw-gateway.service -f
```

9

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Access the Control UI

On your computer, get a dashboard URL from the Pi:

```
ssh user@gateway-host 'openclaw dashboard --no-open'
```

Then create an SSH tunnel in another terminal:

```
ssh -N -L 18789:127.0.0.1:18789 user@gateway-host
```

Open the printed URL in your local browser. For always-on remote access, see [Tailscale integration](https://docs.openclaw.ai/gateway/tailscale).

## [​](https://docs.openclaw.ai/install/raspberry-pi\#performance-tips)  Performance tips

**Use a USB SSD** — SD cards are slow and wear out. A USB SSD dramatically improves performance. See the [Pi USB boot guide](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#usb-mass-storage-boot).**Enable module compile cache** — Speeds up repeated CLI invocations on lower-power Pi hosts:

```
grep -q 'NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache' ~/.bashrc || cat >> ~/.bashrc <<'EOF' # pragma: allowlist secret
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1
EOF
source ~/.bashrc
```

**Reduce memory usage** — For headless setups, free GPU memory and disable unused services:

```
echo 'gpu_mem=16' | sudo tee -a /boot/config.txt
sudo systemctl disable bluetooth
```

## [​](https://docs.openclaw.ai/install/raspberry-pi\#troubleshooting)  Troubleshooting

**Out of memory** — Verify swap is active with `free -h`. Disable unused services (`sudo systemctl disable cups bluetooth avahi-daemon`). Use API-based models only.**Slow performance** — Use a USB SSD instead of an SD card. Check for CPU throttling with `vcgencmd get_throttled` (should return `0x0`).**Service will not start** — Check logs with `journalctl --user -u openclaw-gateway.service --no-pager -n 100` and run `openclaw doctor --non-interactive`. If this is a headless Pi, also verify lingering is enabled: `sudo loginctl enable-linger "$(whoami)"`.**ARM binary issues** — If a skill fails with “exec format error”, check whether the binary has an ARM64 build. Verify architecture with `uname -m` (should show `aarch64`).**WiFi drops** — Disable WiFi power management: `sudo iwconfig wlan0 power off`.

## [​](https://docs.openclaw.ai/install/raspberry-pi\#next-steps)  Next steps

- [Channels](https://docs.openclaw.ai/channels) — connect Telegram, WhatsApp, Discord, and more
- [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) — all config options
- [Updating](https://docs.openclaw.ai/install/updating) — keep OpenClaw up to date

## [​](https://docs.openclaw.ai/install/raspberry-pi\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [Linux server](https://docs.openclaw.ai/vps)
- [Platforms](https://docs.openclaw.ai/platforms)

[Railway](https://docs.openclaw.ai/install/railway) [Render](https://docs.openclaw.ai/install/render)

Ctrl+I