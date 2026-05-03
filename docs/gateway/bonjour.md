---
source_url: https://docs.openclaw.ai/gateway/bonjour
title: "Bonjour discovery - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/bonjour#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Networking and discovery

Bonjour discovery

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Bonjour / mDNS discovery](https://docs.openclaw.ai/gateway/bonjour#bonjour-%2F-mdns-discovery)
- [Wide-area Bonjour (Unicast DNS-SD) over Tailscale](https://docs.openclaw.ai/gateway/bonjour#wide-area-bonjour-unicast-dns-sd-over-tailscale)
- [Gateway config (recommended)](https://docs.openclaw.ai/gateway/bonjour#gateway-config-recommended)
- [One-time DNS server setup (gateway host)](https://docs.openclaw.ai/gateway/bonjour#one-time-dns-server-setup-gateway-host)
- [Tailscale DNS settings](https://docs.openclaw.ai/gateway/bonjour#tailscale-dns-settings)
- [Gateway listener security (recommended)](https://docs.openclaw.ai/gateway/bonjour#gateway-listener-security-recommended)
- [What advertises](https://docs.openclaw.ai/gateway/bonjour#what-advertises)
- [Service types](https://docs.openclaw.ai/gateway/bonjour#service-types)
- [TXT keys (non-secret hints)](https://docs.openclaw.ai/gateway/bonjour#txt-keys-non-secret-hints)
- [Debugging on macOS](https://docs.openclaw.ai/gateway/bonjour#debugging-on-macos)
- [Debugging in Gateway logs](https://docs.openclaw.ai/gateway/bonjour#debugging-in-gateway-logs)
- [Debugging on iOS node](https://docs.openclaw.ai/gateway/bonjour#debugging-on-ios-node)
- [When to disable Bonjour](https://docs.openclaw.ai/gateway/bonjour#when-to-disable-bonjour)
- [Docker gotchas](https://docs.openclaw.ai/gateway/bonjour#docker-gotchas)
- [Troubleshooting disabled Bonjour](https://docs.openclaw.ai/gateway/bonjour#troubleshooting-disabled-bonjour)
- [Common failure modes](https://docs.openclaw.ai/gateway/bonjour#common-failure-modes)
- [Escaped instance names (\\032)](https://docs.openclaw.ai/gateway/bonjour#escaped-instance-names-%5C032)
- [Disabling / configuration](https://docs.openclaw.ai/gateway/bonjour#disabling-%2F-configuration)
- [Related docs](https://docs.openclaw.ai/gateway/bonjour#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/gateway/bonjour\#bonjour-/-mdns-discovery)  Bonjour / mDNS discovery

OpenClaw uses Bonjour (mDNS / DNS‑SD) to discover an active Gateway (WebSocket endpoint).
Multicast `local.` browsing is a **LAN-only convenience**. The bundled `bonjour`
plugin owns LAN advertising and is enabled by default. For cross-network discovery,
the same beacon can also be published through a configured wide-area DNS-SD domain.
Discovery is still best-effort and does **not** replace SSH or Tailnet-based connectivity.

## [​](https://docs.openclaw.ai/gateway/bonjour\#wide-area-bonjour-unicast-dns-sd-over-tailscale)  Wide-area Bonjour (Unicast DNS-SD) over Tailscale

If the node and gateway are on different networks, multicast mDNS won’t cross the
boundary. You can keep the same discovery UX by switching to **unicast DNS‑SD**
(“Wide‑Area Bonjour”) over Tailscale.High‑level steps:

1. Run a DNS server on the gateway host (reachable over Tailnet).
2. Publish DNS‑SD records for `_openclaw-gw._tcp` under a dedicated zone
(example: `openclaw.internal.`).
3. Configure Tailscale **split DNS** so your chosen domain resolves via that
DNS server for clients (including iOS).

OpenClaw supports any discovery domain; `openclaw.internal.` is just an example.
iOS/Android nodes browse both `local.` and your configured wide‑area domain.

### [​](https://docs.openclaw.ai/gateway/bonjour\#gateway-config-recommended)  Gateway config (recommended)

```
{
  gateway: { bind: "tailnet" }, // tailnet-only (recommended)
  discovery: { wideArea: { enabled: true } }, // enables wide-area DNS-SD publishing
}
```

### [​](https://docs.openclaw.ai/gateway/bonjour\#one-time-dns-server-setup-gateway-host)  One-time DNS server setup (gateway host)

```
openclaw dns setup --apply
```

This installs CoreDNS and configures it to:

- listen on port 53 only on the gateway’s Tailscale interfaces
- serve your chosen domain (example: `openclaw.internal.`) from `~/.openclaw/dns/<domain>.db`

Validate from a tailnet‑connected machine:

```
dns-sd -B _openclaw-gw._tcp openclaw.internal.
dig @<TAILNET_IPV4> -p 53 _openclaw-gw._tcp.openclaw.internal PTR +short
```

### [​](https://docs.openclaw.ai/gateway/bonjour\#tailscale-dns-settings)  Tailscale DNS settings

In the Tailscale admin console:

- Add a nameserver pointing at the gateway’s tailnet IP (UDP/TCP 53).
- Add split DNS so your discovery domain uses that nameserver.

Once clients accept tailnet DNS, iOS nodes and CLI discovery can browse
`_openclaw-gw._tcp` in your discovery domain without multicast.

### [​](https://docs.openclaw.ai/gateway/bonjour\#gateway-listener-security-recommended)  Gateway listener security (recommended)

The Gateway WS port (default `18789`) binds to loopback by default. For LAN/tailnet
access, bind explicitly and keep auth enabled.For tailnet‑only setups:

- Set `gateway.bind: "tailnet"` in `~/.openclaw/openclaw.json`.
- Restart the Gateway (or restart the macOS menubar app).

## [​](https://docs.openclaw.ai/gateway/bonjour\#what-advertises)  What advertises

Only the Gateway advertises `_openclaw-gw._tcp`. LAN multicast advertising is
provided by the bundled `bonjour` plugin; wide-area DNS-SD publishing remains
Gateway-owned.

## [​](https://docs.openclaw.ai/gateway/bonjour\#service-types)  Service types

- `_openclaw-gw._tcp` — gateway transport beacon (used by macOS/iOS/Android nodes).

## [​](https://docs.openclaw.ai/gateway/bonjour\#txt-keys-non-secret-hints)  TXT keys (non-secret hints)

The Gateway advertises small non‑secret hints to make UI flows convenient:

- `role=gateway`
- `displayName=<friendly name>`
- `lanHost=<hostname>.local`
- `gatewayPort=<port>` (Gateway WS + HTTP)
- `gatewayTls=1` (only when TLS is enabled)
- `gatewayTlsSha256=<sha256>` (only when TLS is enabled and fingerprint is available)
- `canvasPort=<port>` (only when the canvas host is enabled; currently the same as `gatewayPort`)
- `transport=gateway`
- `tailnetDns=<magicdns>` (mDNS full mode only, optional hint when Tailnet is available)
- `sshPort=<port>` (mDNS full mode only; wide-area DNS-SD may omit it)
- `cliPath=<path>` (mDNS full mode only; wide-area DNS-SD still writes it as a remote-install hint)

Security notes:

- Bonjour/mDNS TXT records are **unauthenticated**. Clients must not treat TXT as authoritative routing.
- Clients should route using the resolved service endpoint (SRV + A/AAAA). Treat `lanHost`, `tailnetDns`, `gatewayPort`, and `gatewayTlsSha256` as hints only.
- SSH auto-targeting should likewise use the resolved service host, not TXT-only hints.
- TLS pinning must never allow an advertised `gatewayTlsSha256` to override a previously stored pin.
- iOS/Android nodes should treat discovery-based direct connects as **TLS-only** and require explicit user confirmation before trusting a first-time fingerprint.

## [​](https://docs.openclaw.ai/gateway/bonjour\#debugging-on-macos)  Debugging on macOS

Useful built‑in tools:

- Browse instances:














```
dns-sd -B _openclaw-gw._tcp local.
```

- Resolve one instance (replace `<instance>`):














```
dns-sd -L "<instance>" _openclaw-gw._tcp local.
```


If browsing works but resolving fails, you’re usually hitting a LAN policy or
mDNS resolver issue.

## [​](https://docs.openclaw.ai/gateway/bonjour\#debugging-in-gateway-logs)  Debugging in Gateway logs

The Gateway writes a rolling log file (printed on startup as
`gateway log file: ...`). Look for `bonjour:` lines, especially:

- `bonjour: advertise failed ...`
- `bonjour: suppressing ciao cancellation ...`
- `bonjour: ... name conflict resolved` / `hostname conflict resolved`
- `bonjour: watchdog detected non-announced service ...`
- `bonjour: disabling advertiser after ... failed restarts ...`

Bonjour uses the system hostname for the advertised `.local` host when it is a
valid DNS label. If the system hostname contains spaces, underscores, or another
invalid DNS-label character, OpenClaw falls back to `openclaw.local`. Set
`OPENCLAW_MDNS_HOSTNAME=<name>` before starting the Gateway when you need an
explicit host label.

## [​](https://docs.openclaw.ai/gateway/bonjour\#debugging-on-ios-node)  Debugging on iOS node

The iOS node uses `NWBrowser` to discover `_openclaw-gw._tcp`.To capture logs:

- Settings → Gateway → Advanced → **Discovery Debug Logs**
- Settings → Gateway → Advanced → **Discovery Logs** → reproduce → **Copy**

The log includes browser state transitions and result‑set changes.

## [​](https://docs.openclaw.ai/gateway/bonjour\#when-to-disable-bonjour)  When to disable Bonjour

Disable Bonjour only when LAN multicast advertising is unavailable or harmful.
The common case is a Gateway running behind Docker bridge networking, WSL, or a
network policy that drops mDNS multicast. In those environments the Gateway is
still reachable through its published URL, SSH, Tailnet, or wide-area DNS-SD,
but LAN auto-discovery is not reliable.Prefer the existing environment override when the problem is deployment-scoped:

```
OPENCLAW_DISABLE_BONJOUR=1
```

That disables LAN multicast advertising without changing plugin configuration.
It is safe for Docker images, service files, launch scripts, and one-off
debugging because the setting disappears when the environment does.Use plugin configuration only when you intentionally want to turn off the
bundled LAN discovery plugin for that OpenClaw config:

```
openclaw plugins disable bonjour
```

## [​](https://docs.openclaw.ai/gateway/bonjour\#docker-gotchas)  Docker gotchas

The bundled Bonjour plugin auto-disables LAN multicast advertising in detected
containers when `OPENCLAW_DISABLE_BONJOUR` is unset. Docker bridge networks
usually do not forward mDNS multicast (`224.0.0.251:5353`) between the container
and the LAN, so advertising from the container rarely makes discovery work.Important gotchas:

- Disabling Bonjour does not stop the Gateway. It only stops LAN multicast
advertising.
- Disabling Bonjour does not change `gateway.bind`; Docker still defaults to
`OPENCLAW_GATEWAY_BIND=lan` so the published host port can work.
- Disabling Bonjour does not disable wide-area DNS-SD. Use wide-area discovery
or Tailnet when the Gateway and node are not on the same LAN.
- Reusing the same `OPENCLAW_CONFIG_DIR` outside Docker does not persist the
container auto-disable policy.
- Set `OPENCLAW_DISABLE_BONJOUR=0` only for host networking, macvlan, or another
network where mDNS multicast is known to pass; set it to `1` to force-disable.

## [​](https://docs.openclaw.ai/gateway/bonjour\#troubleshooting-disabled-bonjour)  Troubleshooting disabled Bonjour

If a node no longer auto-discovers the Gateway after Docker setup:

1. Confirm whether the Gateway is running in auto, forced-on, or forced-off mode:














```
docker compose config | grep OPENCLAW_DISABLE_BONJOUR
```

2. Confirm the Gateway itself is reachable through the published port:














```
curl -fsS http://127.0.0.1:18789/healthz
```

3. Use a direct target when Bonjour is disabled:   - Control UI or local tools: `http://127.0.0.1:18789`
   - LAN clients: `http://<gateway-host>:18789`
   - Cross-network clients: Tailnet MagicDNS, Tailnet IP, SSH tunnel, or
     wide-area DNS-SD
4. If you deliberately enabled Bonjour in Docker with
`OPENCLAW_DISABLE_BONJOUR=0`, test multicast from the host:














```
dns-sd -B _openclaw-gw._tcp local.
```










If browsing is empty or the Gateway logs show repeated ciao watchdog
cancellations, restore `OPENCLAW_DISABLE_BONJOUR=1` and use a direct or
Tailnet route.

## [​](https://docs.openclaw.ai/gateway/bonjour\#common-failure-modes)  Common failure modes

- **Bonjour doesn’t cross networks**: use Tailnet or SSH.
- **Multicast blocked**: some Wi‑Fi networks disable mDNS.
- **Advertiser stuck in probing/announcing**: hosts with blocked multicast,
container bridges, WSL, or interface churn can leave the ciao advertiser in a
non-announced state. OpenClaw retries a few times and then disables Bonjour
for the current Gateway process instead of restarting the advertiser forever.
- **Docker bridge networking**: Bonjour auto-disables in detected containers.
Set `OPENCLAW_DISABLE_BONJOUR=0` only for host, macvlan, or another
mDNS-capable network.
- **Sleep / interface churn**: macOS may temporarily drop mDNS results; retry.
- **Browse works but resolve fails**: keep machine names simple (avoid emojis or
punctuation), then restart the Gateway. The service instance name derives from
the host name, so overly complex names can confuse some resolvers.

## [​](https://docs.openclaw.ai/gateway/bonjour\#escaped-instance-names-\032)  Escaped instance names (`\032`)

Bonjour/DNS‑SD often escapes bytes in service instance names as decimal `\DDD`
sequences (e.g. spaces become `\032`).

- This is normal at the protocol level.
- UIs should decode for display (iOS uses `BonjourEscapes.decode`).

## [​](https://docs.openclaw.ai/gateway/bonjour\#disabling-/-configuration)  Disabling / configuration

- `openclaw plugins disable bonjour` disables LAN multicast advertising by disabling the bundled plugin.
- `openclaw plugins enable bonjour` restores the default LAN discovery plugin.
- `OPENCLAW_DISABLE_BONJOUR=1` disables LAN multicast advertising without changing plugin config; accepted truthy values are `1`, `true`, `yes`, and `on` (legacy: `OPENCLAW_DISABLE_BONJOUR`).
- `OPENCLAW_DISABLE_BONJOUR=0` forces LAN multicast advertising on, including inside detected containers; accepted falsy values are `0`, `false`, `no`, and `off`.
- When `OPENCLAW_DISABLE_BONJOUR` is unset, Bonjour advertises on normal hosts and auto-disables inside detected containers.
- `gateway.bind` in `~/.openclaw/openclaw.json` controls the Gateway bind mode.
- `OPENCLAW_SSH_PORT` overrides the SSH port when `sshPort` is advertised (legacy: `OPENCLAW_SSH_PORT`).
- `OPENCLAW_TAILNET_DNS` publishes a MagicDNS hint in TXT when mDNS full mode is enabled (legacy: `OPENCLAW_TAILNET_DNS`).
- `OPENCLAW_CLI_PATH` overrides the advertised CLI path (legacy: `OPENCLAW_CLI_PATH`).

## [​](https://docs.openclaw.ai/gateway/bonjour\#related-docs)  Related docs

- Discovery policy and transport selection: [Discovery](https://docs.openclaw.ai/gateway/discovery)
- Node pairing + approvals: [Gateway pairing](https://docs.openclaw.ai/gateway/pairing)

[Discovery and transports](https://docs.openclaw.ai/gateway/discovery) [Remote access](https://docs.openclaw.ai/gateway/remote)

Ctrl+I