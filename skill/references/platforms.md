# Platforms

_15 pages from docs.openclaw.ai — full content preserved._

## Contents

- [Registro en macOS - OpenClaw](#registro-en-macos---openclaw)
- [Bilah menu - OpenClaw](#bilah-menu---openclaw)
- [Platforms - OpenClaw](#platforms---openclaw)
- [Android app - OpenClaw](#android-app---openclaw)
- [iOS app - OpenClaw](#ios-app---openclaw)
- [Linux app - OpenClaw](#linux-app---openclaw)
- [Canvas - OpenClaw](#canvas---openclaw)
- [macOS permissions - OpenClaw](#macos-permissions---openclaw)
- [macOS IPC - OpenClaw](#macos-ipc---openclaw)
- [macOS app - OpenClaw](#macos-app---openclaw)
- [Windows - OpenClaw](#windows---openclaw)
- [Мережа - OpenClaw](#--openclaw)
- [Linux server - OpenClaw](#linux-server---openclaw)
- [https://docs.openclaw.ai/vps.md](#httpsdocsopenclawaivpsmd)
- [macOS 开发环境设置 - OpenClaw](#macos---openclaw)

---

## Registro en macOS - OpenClaw

_Source: <https://docs.openclaw.ai/es/platforms/mac/logging>_

# Registro (macOS)

## Registro rotativo de diagnóstico en archivo (panel Debug)

OpenClaw enruta los registros de la app de macOS mediante swift-log (registro unificado de forma predeterminada) y puede escribir un registro local rotativo en archivo cuando necesitas una captura duradera.

- Nivel de detalle: **panel Debug → Logs → App logging → Verbosity**
- Habilitar: **panel Debug → Logs → App logging → “Write rolling diagnostics log (JSONL)”**
- Ubicación: `~/Library/Logs/OpenClaw/diagnostics.jsonl` (rota automáticamente; los archivos antiguos se sufijan con `.1`, `.2`, …)
- Borrar: **panel Debug → Logs → App logging → “Clear”**

Notas:

- Esto está **desactivado por defecto**. Habilítalo solo mientras estés depurando activamente.
- Trata el archivo como sensible; no lo compartas sin revisarlo.

## Datos privados del registro unificado en macOS

El registro unificado redacta la mayoría de las cargas útiles salvo que un subsistema active `privacy -off`. Según el artículo de Peter sobre macOS [logging privacy shenanigans](https://steipete.me/posts/2025/logging-privacy-shenanigans) (2025), esto se controla mediante un plist en `/Library/Preferences/Logging/Subsystems/` indexado por el nombre del subsistema. Solo las nuevas entradas del registro recogen la flag, así que actívala antes de reproducir un problema.

## Habilitar para OpenClaw (`ai.openclaw`)

- Escribe primero el plist en un archivo temporal y luego instálalo atómicamente como root:

```
cat <<'EOF' >/tmp/ai.openclaw.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>DEFAULT-OPTIONS</key>
    <dict>
        <key>Enable-Private-Data</key>
        <true/>
    </dict>
</dict>
</plist>
EOF
sudo install -m 644 -o root -g wheel /tmp/ai.openclaw.plist /Library/Preferences/Logging/Subsystems/ai.openclaw.plist
```

- No hace falta reiniciar; logd detecta el archivo rápidamente, pero solo las nuevas líneas del registro incluirán cargas útiles privadas.
- Visualiza la salida enriquecida con el helper existente, por ejemplo `./scripts/clawlog.sh --category WebChat --last 5m`.

## Deshabilitar después de depurar

- Elimina la sobrescritura: `sudo rm /Library/Preferences/Logging/Subsystems/ai.openclaw.plist`.
- Opcionalmente ejecuta `sudo log config --reload` para forzar a logd a descartar inmediatamente la sobrescritura.
- Recuerda que esta superficie puede incluir números de teléfono y cuerpos de mensajes; mantén el plist activo solo mientras necesites realmente ese nivel adicional de detalle.

## Relacionado

- [App de macOS](https://docs.openclaw.ai/es/platforms/macos)
- [Registro del Gateway](https://docs.openclaw.ai/es/gateway/logging)

[Comprobaciones de estado (macOS)](https://docs.openclaw.ai/es/platforms/mac/health) [Control remoto](https://docs.openclaw.ai/es/platforms/mac/remote)

Ctrl+I

---

## Bilah menu - OpenClaw

_Source: <https://docs.openclaw.ai/id/platforms/mac/menu-bar>_

# Logika Status Bilah Menu

## Yang ditampilkan

- Kami menampilkan status kerja agen saat ini pada ikon bilah menu dan pada baris status pertama di menu.
- Status kesehatan disembunyikan saat pekerjaan sedang aktif; status ini kembali saat semua sesi idle.
- Submenu “Konteks” root berisi sesi terbaru alih-alih memperluasnya langsung di menu root.
- Blok “Node” di menu root hanya mencantumkan **perangkat** (node yang dipasangkan melalui `node.list`), bukan entri klien/kehadiran.
- Bagian “Penggunaan” root muncul di bawah Konteks saat snapshot penggunaan penyedia tersedia, diikuti detail biaya penggunaan jika tersedia.

## Model status

- Sesi: peristiwa datang dengan `runId` (per-run) plus `sessionKey` di payload. Sesi “utama” adalah kunci `main`; jika tidak ada, kami kembali ke sesi yang paling baru diperbarui.
- Prioritas: utama selalu menang. Jika utama aktif, statusnya langsung ditampilkan. Jika utama idle, sesi non-utama yang paling baru aktif akan ditampilkan. Kami tidak bolak-balik di tengah aktivitas; kami hanya beralih saat sesi saat ini menjadi idle atau utama menjadi aktif.
- Jenis aktivitas:
  - `job`: eksekusi perintah tingkat tinggi (`state: started|streaming|done|error`).
  - `tool`: `phase: start|result` dengan `toolName` dan `meta/args`.

## Enum IconState (Swift)

- `idle`
- `workingMain(ActivityKind)`
- `workingOther(ActivityKind)`
- `overridden(ActivityKind)` (override debug)

### ActivityKind → glif

- `exec` → 💻
- `read` → 📄
- `write` → ✍️
- `edit` → 📝
- `attach` → 📎
- default → 🛠️

### Pemetaan visual

- `idle`: makhluk normal.
- `workingMain`: badge dengan glif, tint penuh, animasi kaki “bekerja”.
- `workingOther`: badge dengan glif, tint diredam, tanpa gerakan cepat.
- `overridden`: menggunakan glif/tint yang dipilih terlepas dari aktivitas.

## Submenu Konteks

- Menu root menampilkan satu baris “Konteks” dengan jumlah/status sesi dan membuka submenu.
- Header submenu Konteks menampilkan jumlah sesi aktif selama 24 jam terakhir.
- Setiap baris sesi mempertahankan bilah token, umur, pratinjau, berpikir/verbose, reset, compact, dan tindakan hapusnya.
- Pesan pemuatan, terputus, dan kesalahan pemuatan sesi muncul di dalam submenu Konteks.
- Penggunaan penyedia dan detail biaya penggunaan tetap berada di tingkat root di bawah Konteks agar tetap dapat dilihat sekilas tanpa membuka submenu.

## Teks baris status (menu)

- Saat pekerjaan aktif: `<Session role> · <activity label>`
  - Contoh: `Main · exec: pnpm test`, `Other · read: apps/macos/Sources/OpenClaw/AppState.swift`.
- Saat idle: kembali ke ringkasan kesehatan.

## Ingesti peristiwa

- Sumber: peristiwa `agent` control-channel (`ControlChannel.handleAgentEvent`).
- Bidang yang diurai:
  - `stream: "job"` dengan `data.state` untuk mulai/berhenti.
  - `stream: "tool"` dengan `data.phase`, `name`, opsional `meta`/`args`.
- Label:
  - `exec`: baris pertama dari `args.command`.
  - `read`/`write`: jalur yang dipersingkat.
  - `edit`: jalur plus jenis perubahan yang disimpulkan dari jumlah `meta`/diff.
  - fallback: nama alat.

## Override debug

- Pengaturan ▸ Debug ▸ pemilih “Override ikon”:
  - `System (auto)` (default)
  - `Working: main` (per jenis alat)
  - `Working: other` (per jenis alat)
  - `Idle`
- Disimpan melalui `@AppStorage("iconOverride")`; dipetakan ke `IconState.overridden`.

## Checklist pengujian

- Picu job sesi utama: verifikasi ikon langsung beralih dan baris status menampilkan label utama.
- Picu job sesi non-utama saat utama idle: ikon/status menampilkan non-utama; tetap stabil hingga selesai.
- Mulai utama saat yang lain aktif: ikon langsung beralih ke utama.
- Burst alat cepat: pastikan badge tidak berkedip (tenggang TTL pada hasil alat).
- Baris kesehatan muncul kembali setelah semua sesi idle.

## Terkait

- [aplikasi macOS](https://docs.openclaw.ai/id/platforms/macos)
- [Ikon bilah menu](https://docs.openclaw.ai/id/platforms/mac/icon)

[Penyiapan pengembangan macOS](https://docs.openclaw.ai/id/platforms/mac/dev-setup) [Ikon bilah menu](https://docs.openclaw.ai/id/platforms/mac/icon)

Ctrl+I

---

## Platforms - OpenClaw

_Source: <https://docs.openclaw.ai/platforms>_

[OpenClaw home page](https://docs.openclaw.ai/)

Platforms overview

Platforms

OpenClaw core is written in TypeScript. **Node is the recommended runtime**.
Bun is not recommended for the Gateway — known issues with WhatsApp and
Telegram channels; see [Bun (experimental)](https://docs.openclaw.ai/install/bun) for details.Companion apps exist for macOS (menu bar app) and mobile nodes (iOS/Android). Windows and
Linux companion apps are planned, but the Gateway is fully supported today.
Native companion apps for Windows are also planned; the Gateway is recommended via WSL2.

## Choose your OS

- macOS: [macOS](https://docs.openclaw.ai/platforms/macos)
- iOS: [iOS](https://docs.openclaw.ai/platforms/ios)
- Android: [Android](https://docs.openclaw.ai/platforms/android)
- Windows: [Windows](https://docs.openclaw.ai/platforms/windows)
- Linux: [Linux](https://docs.openclaw.ai/platforms/linux)

## VPS & hosting

- VPS hub: [VPS hosting](https://docs.openclaw.ai/vps)
- Fly.io: [Fly.io](https://docs.openclaw.ai/install/fly)
- Hetzner (Docker): [Hetzner](https://docs.openclaw.ai/install/hetzner)
- GCP (Compute Engine): [GCP](https://docs.openclaw.ai/install/gcp)
- Azure (Linux VM): [Azure](https://docs.openclaw.ai/install/azure)
- exe.dev (VM + HTTPS proxy): [exe.dev](https://docs.openclaw.ai/install/exe-dev)

## Common links

- Install guide: [Getting Started](https://docs.openclaw.ai/start/getting-started)
- Gateway runbook: [Gateway](https://docs.openclaw.ai/gateway)
- Gateway configuration: [Configuration](https://docs.openclaw.ai/gateway/configuration)
- Service status: `openclaw gateway status`

## Gateway service install (CLI)

Use one of these (all supported):

- Wizard (recommended): `openclaw onboard --install-daemon`
- Direct: `openclaw gateway install`
- Configure flow: `openclaw configure` → select **Gateway service**
- Repair/migrate: `openclaw doctor` (offers to install or fix the service)

The service target depends on OS:

- macOS: LaunchAgent (`ai.openclaw.gateway` or `ai.openclaw.<profile>`; legacy `com.openclaw.*`)
- Linux/WSL2: systemd user service (`openclaw-gateway[-<profile>].service`)
- Native Windows: Scheduled Task (`OpenClaw Gateway` or `OpenClaw Gateway (<profile>)`), with a per-user Startup-folder login item fallback if task creation is denied

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [macOS app](https://docs.openclaw.ai/platforms/macos)
- [iOS app](https://docs.openclaw.ai/platforms/ios)

[macOS app](https://docs.openclaw.ai/platforms/macos)

Ctrl+I

---

## Android app - OpenClaw

_Source: <https://docs.openclaw.ai/platforms/android>_

[OpenClaw home page](https://docs.openclaw.ai/)

Platforms overview

Android app

The Android app has not been publicly released yet. The source code is available in the [OpenClaw repository](https://github.com/openclaw/openclaw) under `apps/android`. You can build it yourself using Java 17 and the Android SDK (`./gradlew :app:assemblePlayDebug`). See [apps/android/README.md](https://github.com/openclaw/openclaw/blob/main/apps/android/README.md) for build instructions.

## Support snapshot

- Role: companion node app (Android does not host the Gateway).
- Gateway required: yes (run it on macOS, Linux, or Windows via WSL2).
- Install: [Getting Started](https://docs.openclaw.ai/start/getting-started) \+ [Pairing](https://docs.openclaw.ai/channels/pairing).
- Gateway: [Runbook](https://docs.openclaw.ai/gateway) \+ [Configuration](https://docs.openclaw.ai/gateway/configuration).

  - Protocols: [Gateway protocol](https://docs.openclaw.ai/gateway/protocol) (nodes + control plane).

## System control

System control (launchd/systemd) lives on the Gateway host. See [Gateway](https://docs.openclaw.ai/gateway).

## Connection runbook

Android node app ⇄ (mDNS/NSD + WebSocket) ⇄ **Gateway**Android connects directly to the Gateway WebSocket and uses device pairing (`role: node`).For Tailscale or public hosts, Android requires a secure endpoint:

- Preferred: Tailscale Serve / Funnel with `https://<magicdns>` / `wss://<magicdns>`
- Also supported: any other `wss://` Gateway URL with a real TLS endpoint
- Cleartext `ws://` remains supported on private LAN addresses / `.local` hosts, plus `localhost`, `127.0.0.1`, and the Android emulator bridge (`10.0.2.2`)

### Prerequisites

- You can run the Gateway on the “master” machine.
- Android device/emulator can reach the gateway WebSocket:
  - Same LAN with mDNS/NSD, **or**
  - Same Tailscale tailnet using Wide-Area Bonjour / unicast DNS-SD (see below), **or**
  - Manual gateway host/port (fallback)
- Tailnet/public mobile pairing does **not** use raw tailnet IP `ws://` endpoints. Use Tailscale Serve or another `wss://` URL instead.
- You can run the CLI (`openclaw`) on the gateway machine (or via SSH).

### 1) Start the Gateway

```
openclaw gateway --port 18789 --verbose
```

Confirm in logs you see something like:

- `listening on ws://0.0.0.0:18789`

For remote Android access over Tailscale, prefer Serve/Funnel instead of a raw tailnet bind:

```
openclaw gateway --tailscale serve
```

This gives Android a secure `wss://` / `https://` endpoint. A plain `gateway.bind: "tailnet"` setup is not enough for first-time remote Android pairing unless you also terminate TLS separately.

### 2) Verify discovery (optional)

From the gateway machine:

```
dns-sd -B _openclaw-gw._tcp local.
```

More debugging notes: [Bonjour](https://docs.openclaw.ai/gateway/bonjour).If you also configured a wide-area discovery domain, compare against:

```
openclaw gateway discover --json
```

That shows `local.` plus the configured wide-area domain in one pass and uses the resolved
service endpoint instead of TXT-only hints.

#### Tailnet (Vienna ⇄ London) discovery via unicast DNS-SD

Android NSD/mDNS discovery won’t cross networks. If your Android node and the gateway are on different networks but connected via Tailscale, use Wide-Area Bonjour / unicast DNS-SD instead.Discovery alone is not sufficient for tailnet/public Android pairing. The discovered route still needs a secure endpoint (`wss://` or Tailscale Serve):

1. Set up a DNS-SD zone (example `openclaw.internal.`) on the gateway host and publish `_openclaw-gw._tcp` records.
2. Configure Tailscale split DNS for your chosen domain pointing at that DNS server.

Details and example CoreDNS config: [Bonjour](https://docs.openclaw.ai/gateway/bonjour).

### 3) Connect from Android

In the Android app:

- The app keeps its gateway connection alive via a **foreground service** (persistent notification).
- Open the **Connect** tab.
- Use **Setup Code** or **Manual** mode.
- If discovery is blocked, use manual host/port in **Advanced controls**. For private LAN hosts, `ws://` still works. For Tailscale/public hosts, turn on TLS and use a `wss://` / Tailscale Serve endpoint.

After the first successful pairing, Android auto-reconnects on launch:

- Manual endpoint (if enabled), otherwise
- The last discovered gateway (best-effort).

### Presence alive beacons

After the authenticated node session connects, and when the app moves to the background while the
foreground service is still connected, Android calls `node.event` with
`event: "node.presence.alive"`. The gateway records this as `lastSeenAtMs`/`lastSeenReason` on the
paired node/device metadata only after the authenticated node device identity is known.The app counts the beacon as successfully recorded only when the gateway response includes
`handled: true`. Older gateways may acknowledge `node.event` with `{ "ok": true }`; that response is
compatible but does not count as a durable last-seen update.

### 4) Approve pairing (CLI)

On the gateway machine:

```
openclaw devices list
openclaw devices approve <requestId>
openclaw devices reject <requestId>
```

Pairing details: [Pairing](https://docs.openclaw.ai/channels/pairing).Optional: if the Android node always connects from a tightly controlled subnet,
you can opt in to first-time node auto-approval with explicit CIDRs or exact IPs:

```
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24"],
      },
    },
  },
}
```

This is disabled by default. It applies only to fresh `role: node` pairing with
no requested scopes. Operator/browser pairing and any role, scope, metadata, or
public-key change still require manual approval.

### 5) Verify the node is connected

- Via nodes status:

```
openclaw nodes status
```

- Via Gateway:

```
openclaw gateway call node.list --params "{}"
```

### 6) Chat + history

The Android Chat tab supports session selection (default `main`, plus other existing sessions):

- History: `chat.history` (display-normalized; inline directive tags are
stripped from visible text, plain-text tool-call XML payloads (including
`<tool_call>...</tool_call>`, `<function_call>...</function_call>`,
`<tool_calls>...</tool_calls>`, `<function_calls>...</function_calls>`, and
truncated tool-call blocks) and leaked ASCII/full-width model control tokens
are stripped, pure silent-token assistant rows such as exact `NO_REPLY` /
`no_reply` are omitted, and oversized rows can be replaced with placeholders)
- Send: `chat.send`
- Push updates (best-effort): `chat.subscribe` → `event:"chat"`

### 7) Canvas + camera

#### Gateway Canvas Host (recommended for web content)

If you want the node to show real HTML/CSS/JS that the agent can edit on disk, point the node at the Gateway canvas host.

Nodes load canvas from the Gateway HTTP server (same port as `gateway.port`, default `18789`).

1. Create `~/.openclaw/workspace/canvas/index.html` on the gateway host.
2. Navigate the node to it (LAN):

```
openclaw nodes invoke --node "<Android Node>" --command canvas.navigate --params '{"url":"http://<gateway-hostname>.local:18789/__openclaw__/canvas/"}'
```

Tailnet (optional): if both devices are on Tailscale, use a MagicDNS name or tailnet IP instead of `.local`, e.g. `http://<gateway-magicdns>:18789/__openclaw__/canvas/`.This server injects a live-reload client into HTML and reloads on file changes.
The A2UI host lives at `http://<gateway-host>:18789/__openclaw__/a2ui/`.Canvas commands (foreground only):

- `canvas.eval`, `canvas.snapshot`, `canvas.navigate` (use `{"url":""}` or `{"url":"/"}` to return to the default scaffold). `canvas.snapshot` returns `{ format, base64 }` (default `format="jpeg"`).
- A2UI: `canvas.a2ui.push`, `canvas.a2ui.reset` (`canvas.a2ui.pushJSONL` legacy alias)

Camera commands (foreground only; permission-gated):

- `camera.snap` (jpg)
- `camera.clip` (mp4)

See [Camera node](https://docs.openclaw.ai/nodes/camera) for parameters and CLI helpers.

### 8) Voice + expanded Android command surface

- Voice tab: Android has two explicit capture modes. **Mic** is a manual Voice-tab session that sends each pause as a chat turn and stops when the app leaves the foreground or the user leaves the Voice tab. **Talk** is continuous Talk Mode and keeps listening until toggled off or the node disconnects.
- Talk Mode promotes the existing foreground service from `dataSync` to `dataSync|microphone` before capture starts, then demotes it when Talk Mode stops. Android 14+ requires the `FOREGROUND_SERVICE_MICROPHONE` declaration, the `RECORD_AUDIO` runtime grant, and the microphone service type at runtime.
- Spoken replies use `talk.speak` through the configured gateway Talk provider. Local system TTS is used only when `talk.speak` is unavailable.
- Voice wake remains disabled in the Android UX/runtime.
- Additional Android command families (availability depends on device + permissions):
  - `device.status`, `device.info`, `device.permissions`, `device.health`
  - `notifications.list`, `notifications.actions` (see [Notification forwarding](https://docs.openclaw.ai/platforms/android#notification-forwarding) below)
  - `photos.latest`
  - `contacts.search`, `contacts.add`
  - `calendar.events`, `calendar.add`
  - `callLog.search`
  - `sms.search`
  - `motion.activity`, `motion.pedometer`

## Assistant entrypoints

Android supports launching OpenClaw from the system assistant trigger (Google
Assistant). When configured, holding the home button or saying “Hey Google, ask
OpenClaw…” opens the app and hands the prompt into the chat composer.This uses Android **App Actions** metadata declared in the app manifest. No
extra configuration is needed on the gateway side — the assistant intent is
handled entirely by the Android app and forwarded as a normal chat message.

App Actions availability depends on the device, Google Play Services version,
and whether the user has set OpenClaw as the default assistant app.

## Notification forwarding

Android can forward device notifications to the gateway as events. Several controls let you scope which notifications are forwarded and when.

| Key | Type | Description |
| --- | --- | --- |
| `notifications.allowPackages` | string\[\] | Only forward notifications from these package names. If set, all other packages are ignored. |
| `notifications.denyPackages` | string\[\] | Never forward notifications from these package names. Applied after `allowPackages`. |
| `notifications.quietHours.start` | string (HH:mm) | Start of quiet hours window (local device time). Notifications are suppressed during this window. |
| `notifications.quietHours.end` | string (HH:mm) | End of quiet hours window. |
| `notifications.rateLimit` | number | Maximum forwarded notifications per package per minute. Excess notifications are dropped. |

The notification picker also uses safer behavior for forwarded notification events, preventing accidental forwarding of sensitive system notifications.Example configuration:

```
{
  notifications: {
    allowPackages: ["com.slack", "com.whatsapp"],
    denyPackages: ["com.android.systemui"],
    quietHours: {
      start: "22:00",
      end: "07:00",
    },
    rateLimit: 5,
  },
}
```

Notification forwarding requires the Android Notification Listener permission. The app prompts for this during setup.

## Related

- [iOS app](https://docs.openclaw.ai/platforms/ios)
- [Nodes](https://docs.openclaw.ai/nodes)
- [Android node troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting)

[Windows](https://docs.openclaw.ai/platforms/windows) [iOS app](https://docs.openclaw.ai/platforms/ios)

Ctrl+I

---

## iOS app - OpenClaw

_Source: <https://docs.openclaw.ai/platforms/ios>_

[OpenClaw home page](https://docs.openclaw.ai/)

Platforms overview

iOS app

Availability: internal preview. The iOS app is not publicly distributed yet.

## What it does

- Connects to a Gateway over WebSocket (LAN or tailnet).
- Exposes node capabilities: Canvas, Screen snapshot, Camera capture, Location, Talk mode, Voice wake.
- Receives `node.invoke` commands and reports node status events.

## Requirements

- Gateway running on another device (macOS, Linux, or Windows via WSL2).
- Network path:
  - Same LAN via Bonjour, **or**
  - Tailnet via unicast DNS-SD (example domain: `openclaw.internal.`), **or**
  - Manual host/port (fallback).

## Quick start (pair + connect)

1. Start the Gateway:

```
openclaw gateway --port 18789
```

2. In the iOS app, open Settings and pick a discovered gateway (or enable Manual Host and enter host/port).
3. Approve the pairing request on the gateway host:

```
openclaw devices list
openclaw devices approve <requestId>
```

If the app retries pairing with changed auth details (role/scopes/public key),
the previous pending request is superseded and a new `requestId` is created.
Run `openclaw devices list` again before approval.Optional: if the iOS node always connects from a tightly controlled subnet, you
can opt in to first-time node auto-approval with explicit CIDRs or exact IPs:

```
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24"],
      },
    },
  },
}
```

This is disabled by default. It applies only to fresh `role: node` pairing with
no requested scopes. Operator/browser pairing and any role, scope, metadata, or
public-key change still require manual approval.

4. Verify connection:

```
openclaw nodes status
openclaw gateway call node.list --params "{}"
```

## Relay-backed push for official builds

Official distributed iOS builds use the external push relay instead of publishing the raw APNs
token to the gateway.Gateway-side requirement:

```
{
  gateway: {
    push: {
      apns: {
        relay: {
          baseUrl: "https://relay.example.com",
        },
      },
    },
  },
}
```

How the flow works:

- The iOS app registers with the relay using App Attest and a StoreKit app transaction JWS.
- The relay returns an opaque relay handle plus a registration-scoped send grant.
- The iOS app fetches the paired gateway identity and includes it in relay registration, so the relay-backed registration is delegated to that specific gateway.
- The app forwards that relay-backed registration to the paired gateway with `push.apns.register`.
- The gateway uses that stored relay handle for `push.test`, background wakes, and wake nudges.
- The gateway relay base URL must match the relay URL baked into the official/TestFlight iOS build.
- If the app later connects to a different gateway or a build with a different relay base URL, it refreshes the relay registration instead of reusing the old binding.

What the gateway does **not** need for this path:

- No deployment-wide relay token.
- No direct APNs key for official/TestFlight relay-backed sends.

Expected operator flow:

1. Install the official/TestFlight iOS build.
2. Set `gateway.push.apns.relay.baseUrl` on the gateway.
3. Pair the app to the gateway and let it finish connecting.
4. The app publishes `push.apns.register` automatically after it has an APNs token, the operator session is connected, and relay registration succeeds.
5. After that, `push.test`, reconnect wakes, and wake nudges can use the stored relay-backed registration.

## Background alive beacons

When iOS wakes the app for a silent push, background refresh, or significant-location event, the app
attempts a short node reconnect and then calls `node.event` with `event: "node.presence.alive"`.
The gateway records this as `lastSeenAtMs`/`lastSeenReason` on the paired node/device metadata only
after the authenticated node device identity is known.The app treats a background wake as successfully recorded only when the gateway response includes
`handled: true`. Older gateways may acknowledge `node.event` with `{ "ok": true }`; that response is
compatible but does not count as a durable last-seen update.Compatibility note:

- `OPENCLAW_APNS_RELAY_BASE_URL` still works as a temporary env override for the gateway.

## Authentication and trust flow

The relay exists to enforce two constraints that direct APNs-on-gateway cannot provide for
official iOS builds:

- Only genuine OpenClaw iOS builds distributed through Apple can use the hosted relay.
- A gateway can send relay-backed pushes only for iOS devices that paired with that specific
gateway.

Hop by hop:

1. `iOS app -> gateway`   - The app first pairs with the gateway through the normal Gateway auth flow.
   - That gives the app an authenticated node session plus an authenticated operator session.
   - The operator session is used to call `gateway.identity.get`.
2. `iOS app -> relay`   - The app calls the relay registration endpoints over HTTPS.
   - Registration includes App Attest proof plus a StoreKit app transaction JWS.
   - The relay validates the bundle ID, App Attest proof, and Apple distribution proof, and requires the
     official/production distribution path.
   - This is what blocks local Xcode/dev builds from using the hosted relay. A local build may be
     signed, but it does not satisfy the official Apple distribution proof the relay expects.
3. `gateway identity delegation`   - Before relay registration, the app fetches the paired gateway identity from
        `gateway.identity.get`.
   - The app includes that gateway identity in the relay registration payload.
   - The relay returns a relay handle and a registration-scoped send grant that are delegated to
     that gateway identity.
4. `gateway -> relay`   - The gateway stores the relay handle and send grant from `push.apns.register`.
   - On `push.test`, reconnect wakes, and wake nudges, the gateway signs the send request with its
     own device identity.
   - The relay verifies both the stored send grant and the gateway signature against the delegated
     gateway identity from registration.
   - Another gateway cannot reuse that stored registration, even if it somehow obtains the handle.
5. `relay -> APNs`   - The relay owns the production APNs credentials and the raw APNs token for the official build.
   - The gateway never stores the raw APNs token for relay-backed official builds.
   - The relay sends the final push to APNs on behalf of the paired gateway.

Why this design was created:

- To keep production APNs credentials out of user gateways.
- To avoid storing raw official-build APNs tokens on the gateway.
- To allow hosted relay usage only for official/TestFlight OpenClaw builds.
- To prevent one gateway from sending wake pushes to iOS devices owned by a different gateway.

Local/manual builds remain on direct APNs. If you are testing those builds without the relay, the
gateway still needs direct APNs credentials:

```
export OPENCLAW_APNS_TEAM_ID="TEAMID"
export OPENCLAW_APNS_KEY_ID="KEYID"
export OPENCLAW_APNS_PRIVATE_KEY_P8="$(cat /path/to/AuthKey_KEYID.p8)"
```

These are gateway-host runtime env vars, not Fastlane settings. `apps/ios/fastlane/.env` only stores
App Store Connect / TestFlight auth such as `ASC_KEY_ID` and `ASC_ISSUER_ID`; it does not configure
direct APNs delivery for local iOS builds.Recommended gateway-host storage:

```
mkdir -p ~/.openclaw/credentials/apns
chmod 700 ~/.openclaw/credentials/apns
mv /path/to/AuthKey_KEYID.p8 ~/.openclaw/credentials/apns/AuthKey_KEYID.p8
chmod 600 ~/.openclaw/credentials/apns/AuthKey_KEYID.p8
export OPENCLAW_APNS_PRIVATE_KEY_PATH="$HOME/.openclaw/credentials/apns/AuthKey_KEYID.p8"
```

Do not commit the `.p8` file or place it under the repo checkout.

## Discovery paths

### Bonjour (LAN)

The iOS app browses `_openclaw-gw._tcp` on `local.` and, when configured, the same
wide-area DNS-SD discovery domain. Same-LAN gateways appear automatically from `local.`;
cross-network discovery can use the configured wide-area domain without changing the beacon type.

### Tailnet (cross-network)

If mDNS is blocked, use a unicast DNS-SD zone (choose a domain; example:
`openclaw.internal.`) and Tailscale split DNS.
See [Bonjour](https://docs.openclaw.ai/gateway/bonjour) for the CoreDNS example.

### Manual host/port

In Settings, enable **Manual Host** and enter the gateway host + port (default `18789`).

## Canvas + A2UI

The iOS node renders a WKWebView canvas. Use `node.invoke` to drive it:

```
openclaw nodes invoke --node "iOS Node" --command canvas.navigate --params '{"url":"http://<gateway-host>:18789/__openclaw__/canvas/"}'
```

Notes:

- The Gateway canvas host serves `/__openclaw__/canvas/` and `/__openclaw__/a2ui/`.
- It is served from the Gateway HTTP server (same port as `gateway.port`, default `18789`).
- The iOS node auto-navigates to A2UI on connect when a canvas host URL is advertised.
- Return to the built-in scaffold with `canvas.navigate` and `{"url":""}`.

## Computer Use relationship

The iOS app is a mobile node surface, not a Codex Computer Use backend. Codex
Computer Use and `cua-driver mcp` control a local macOS desktop through MCP
tools; the iOS app exposes iPhone capabilities through OpenClaw node commands
such as `canvas.*`, `camera.*`, `screen.*`, `location.*`, and `talk.*`.Agents can still operate the iOS app through OpenClaw by invoking node
commands, but those calls go through the gateway node protocol and follow iOS
foreground/background limits. Use [Codex Computer Use](https://docs.openclaw.ai/plugins/codex-computer-use)
for local desktop control and this page for iOS node capabilities.

### Canvas eval / snapshot

```
openclaw nodes invoke --node "iOS Node" --command canvas.eval --params '{"javaScript":"(() => { const {ctx} = window.__openclaw; ctx.clearRect(0,0,innerWidth,innerHeight); ctx.lineWidth=6; ctx.strokeStyle=\"#ff2d55\"; ctx.beginPath(); ctx.moveTo(40,40); ctx.lineTo(innerWidth-40, innerHeight-40); ctx.stroke(); return \"ok\"; })()"}'
```

```
openclaw nodes invoke --node "iOS Node" --command canvas.snapshot --params '{"maxWidth":900,"format":"jpeg"}'
```

## Voice wake + talk mode

- Voice wake and talk mode are available in Settings.
- iOS may suspend background audio; treat voice features as best-effort when the app is not active.

## Common errors

- `NODE_BACKGROUND_UNAVAILABLE`: bring the iOS app to the foreground (canvas/camera/screen commands require it).
- `A2UI_HOST_NOT_CONFIGURED`: the Gateway did not advertise a canvas host URL; check `canvasHost` in [Gateway configuration](https://docs.openclaw.ai/gateway/configuration).
- Pairing prompt never appears: run `openclaw devices list` and approve manually.
- Reconnect fails after reinstall: the Keychain pairing token was cleared; re-pair the node.

## Related docs

- [Pairing](https://docs.openclaw.ai/channels/pairing)
- [Discovery](https://docs.openclaw.ai/gateway/discovery)
- [Bonjour](https://docs.openclaw.ai/gateway/bonjour)

[Android app](https://docs.openclaw.ai/platforms/android) [macOS dev setup](https://docs.openclaw.ai/platforms/mac/dev-setup)

Ctrl+I

---

## Linux app - OpenClaw

_Source: <https://docs.openclaw.ai/platforms/linux>_

[OpenClaw home page](https://docs.openclaw.ai/)

Platforms overview

Linux app

The Gateway is fully supported on Linux. **Node is the recommended runtime**.
Bun is not recommended for the Gateway (WhatsApp/Telegram bugs).Native Linux companion apps are planned. Contributions are welcome if you want to help build one.

## Beginner quick path (VPS)

1. Install Node 24 (recommended; Node 22 LTS, currently `22.14+`, still works for compatibility)
2. `npm i -g openclaw@latest`
3. `openclaw onboard --install-daemon`
4. From your laptop: `ssh -N -L 18789:127.0.0.1:18789 <user>@<host>`
5. Open `http://127.0.0.1:18789/` and authenticate with the configured shared secret (token by default; password if you set `gateway.auth.mode: "password"`)

Full Linux server guide: [Linux Server](https://docs.openclaw.ai/vps). Step-by-step VPS example: [exe.dev](https://docs.openclaw.ai/install/exe-dev)

## Install

- [Getting Started](https://docs.openclaw.ai/start/getting-started)
- [Install & updates](https://docs.openclaw.ai/install/updating)
- Optional flows: [Bun (experimental)](https://docs.openclaw.ai/install/bun), [Nix](https://docs.openclaw.ai/install/nix), [Docker](https://docs.openclaw.ai/install/docker)

## Gateway

- [Gateway runbook](https://docs.openclaw.ai/gateway)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)

## Gateway service install (CLI)

Use one of these:

```
openclaw onboard --install-daemon
```

Or:

```
openclaw gateway install
```

Or:

```
openclaw configure
```

Select **Gateway service** when prompted.Repair/migrate:

```
openclaw doctor
```

## System control (systemd user unit)

OpenClaw installs a systemd **user** service by default. Use a **system**
service for shared or always-on servers. `openclaw gateway install` and
`openclaw onboard --install-daemon` already render the current canonical unit
for you; write one by hand only when you need a custom system/service-manager
setup. The full service guidance lives in the [Gateway runbook](https://docs.openclaw.ai/gateway).Minimal setup:Create `~/.config/systemd/user/openclaw-gateway[-<profile>].service`:

```
[Unit]
Description=OpenClaw Gateway (profile: <profile>, v<version>)
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/local/bin/openclaw gateway --port 18789
Restart=always
RestartSec=5
TimeoutStopSec=30
TimeoutStartSec=30
SuccessExitStatus=0 143
KillMode=control-group

[Install]
WantedBy=default.target
```

Enable it:

```
systemctl --user enable --now openclaw-gateway[-<profile>].service
```

## Memory pressure and OOM kills

On Linux, the kernel chooses an OOM victim when a host, VM, or container cgroup
runs out of memory. The Gateway can be a poor victim because it owns long-lived
sessions and channel connections. OpenClaw therefore biases transient child
processes to be killed before the Gateway when possible.For eligible Linux child spawns, OpenClaw starts the child through a short
`/bin/sh` wrapper that raises the child’s own `oom_score_adj` to `1000`, then
`exec`s the real command. This is an unprivileged operation because the child is
only increasing its own OOM kill likelihood.Covered child process surfaces include:

- supervisor-managed command children,
- PTY shell children,
- MCP stdio server children,
- OpenClaw-launched browser/Chrome processes.

The wrapper is Linux-only and is skipped when `/bin/sh` is unavailable. It is
also skipped if the child env sets `OPENCLAW_CHILD_OOM_SCORE_ADJ=0`, `false`,
`no`, or `off`.To verify a child process:

```
cat /proc/<child-pid>/oom_score_adj
```

Expected value for covered children is `1000`. The Gateway process should keep
its normal score, usually `0`.This does not replace normal memory tuning. If a VPS or container repeatedly
kills children, increase the memory limit, reduce concurrency, or add stronger
resource controls such as systemd `MemoryMax=` or container-level memory limits.

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [Linux server](https://docs.openclaw.ai/vps)
- [Raspberry Pi](https://docs.openclaw.ai/platforms/raspberry-pi)

[macOS app](https://docs.openclaw.ai/platforms/macos) [Windows](https://docs.openclaw.ai/platforms/windows)

Ctrl+I

---

## Canvas - OpenClaw

_Source: <https://docs.openclaw.ai/platforms/mac/canvas>_

[OpenClaw home page](https://docs.openclaw.ai/)

Features

Canvas

The macOS app embeds an agent‑controlled **Canvas panel** using `WKWebView`. It
is a lightweight visual workspace for HTML/CSS/JS, A2UI, and small interactive
UI surfaces.

## Where Canvas lives

Canvas state is stored under Application Support:

- `~/Library/Application Support/OpenClaw/canvas/<session>/...`

The Canvas panel serves those files via a **custom URL scheme**:

- `openclaw-canvas://<session>/<path>`

Examples:

- `openclaw-canvas://main/` → `<canvasRoot>/main/index.html`
- `openclaw-canvas://main/assets/app.css` → `<canvasRoot>/main/assets/app.css`
- `openclaw-canvas://main/widgets/todo/` → `<canvasRoot>/main/widgets/todo/index.html`

If no `index.html` exists at the root, the app shows a **built‑in scaffold page**.

## Panel behavior

- Borderless, resizable panel anchored near the menu bar (or mouse cursor).
- Remembers size/position per session.
- Auto‑reloads when local canvas files change.
- Only one Canvas panel is visible at a time (session is switched as needed).

Canvas can be disabled from Settings → **Allow Canvas**. When disabled, canvas
node commands return `CANVAS_DISABLED`.

## Agent API surface

Canvas is exposed via the **Gateway WebSocket**, so the agent can:

- show/hide the panel
- navigate to a path or URL
- evaluate JavaScript
- capture a snapshot image

CLI examples:

```
openclaw nodes canvas present --node <id>
openclaw nodes canvas navigate --node <id> --url "/"
openclaw nodes canvas eval --node <id> --js "document.title"
openclaw nodes canvas snapshot --node <id>
```

Notes:

- `canvas.navigate` accepts **local canvas paths**, `http(s)` URLs, and `file://` URLs.
- If you pass `"/"`, the Canvas shows the local scaffold or `index.html`.

## A2UI in Canvas

A2UI is hosted by the Gateway canvas host and rendered inside the Canvas panel.
When the Gateway advertises a Canvas host, the macOS app auto‑navigates to the
A2UI host page on first open.Default A2UI host URL:

```
http://<gateway-host>:18789/__openclaw__/a2ui/
```

### A2UI commands (v0.8)

Canvas currently accepts **A2UI v0.8** server→client messages:

- `beginRendering`
- `surfaceUpdate`
- `dataModelUpdate`
- `deleteSurface`

`createSurface` (v0.9) is not supported.CLI example:

```
cat > /tmp/a2ui-v0.8.jsonl <<'EOFA2'
{"surfaceUpdate":{"surfaceId":"main","components":[{"id":"root","component":{"Column":{"children":{"explicitList":["title","content"]}}}},{"id":"title","component":{"Text":{"text":{"literalString":"Canvas (A2UI v0.8)"},"usageHint":"h1"}}},{"id":"content","component":{"Text":{"text":{"literalString":"If you can read this, A2UI push works."},"usageHint":"body"}}}]}}
{"beginRendering":{"surfaceId":"main","root":"root"}}
EOFA2

openclaw nodes canvas a2ui push --jsonl /tmp/a2ui-v0.8.jsonl --node <id>
```

Quick smoke:

```
openclaw nodes canvas a2ui push --node <id> --text "Hello from A2UI"
```

## Triggering agent runs from Canvas

Canvas can trigger new agent runs via deep links:

- `openclaw://agent?...`

Example (in JS):

```
window.location.href = "openclaw://agent?message=Review%20this%20design";
```

The app prompts for confirmation unless a valid key is provided.

## Security notes

- Canvas scheme blocks directory traversal; files must live under the session root.
- Local Canvas content uses a custom scheme (no loopback server required).
- External `http(s)` URLs are allowed only when explicitly navigated.

## Related

- [macOS app](https://docs.openclaw.ai/platforms/macos)
- [WebChat](https://docs.openclaw.ai/web/webchat)

[WebChat (macOS)](https://docs.openclaw.ai/platforms/mac/webchat) [Skills (macOS)](https://docs.openclaw.ai/platforms/mac/skills)

Ctrl+I

---

## macOS permissions - OpenClaw

_Source: <https://docs.openclaw.ai/platforms/mac/permissions>_

[OpenClaw home page](https://docs.openclaw.ai/)

Setup

macOS permissions

macOS permission grants are fragile. TCC associates a permission grant with the
app’s code signature, bundle identifier, and on-disk path. If any of those change,
macOS treats the app as new and may drop or hide prompts.

## Requirements for stable permissions

- Same path: run the app from a fixed location (for OpenClaw, `dist/OpenClaw.app`).
- Same bundle identifier: changing the bundle ID creates a new permission identity.
- Signed app: unsigned or ad-hoc signed builds do not persist permissions.
- Consistent signature: use a real Apple Development or Developer ID certificate
so the signature stays stable across rebuilds.

Ad-hoc signatures generate a new identity every build. macOS will forget previous
grants, and prompts can disappear entirely until the stale entries are cleared.

## Recovery checklist when prompts disappear

1. Quit the app.
2. Remove the app entry in System Settings -> Privacy & Security.
3. Relaunch the app from the same path and re-grant permissions.
4. If the prompt still does not appear, reset TCC entries with `tccutil` and try again.
5. Some permissions only reappear after a full macOS restart.

Example resets (replace bundle ID as needed):

```
sudo tccutil reset Accessibility ai.openclaw.mac
sudo tccutil reset ScreenCapture ai.openclaw.mac
sudo tccutil reset AppleEvents
```

## Files and folders permissions (Desktop/Documents/Downloads)

macOS may also gate Desktop, Documents, and Downloads for terminal/background processes. If file reads or directory listings hang, grant access to the same process context that performs file operations (for example Terminal/iTerm, LaunchAgent-launched app, or SSH process).Workaround: move files into the OpenClaw workspace (`~/.openclaw/workspace`) if you want to avoid per-folder grants.If you are testing permissions, always sign with a real certificate. Ad-hoc
builds are only acceptable for quick local runs where permissions do not matter.

## Related

- [macOS app](https://docs.openclaw.ai/platforms/macos)
- [macOS signing](https://docs.openclaw.ai/platforms/mac/signing)

[Menu bar icon](https://docs.openclaw.ai/platforms/mac/icon) [macOS signing](https://docs.openclaw.ai/platforms/mac/signing)

Ctrl+I

---

## macOS IPC - OpenClaw

_Source: <https://docs.openclaw.ai/platforms/mac/xpc>_

# OpenClaw macOS IPC architecture

**Current model:** a local Unix socket connects the **node host service** to the **macOS app** for exec approvals + `system.run`. A `openclaw-mac` debug CLI exists for discovery/connect checks; agent actions still flow through the Gateway WebSocket and `node.invoke`. UI automation uses PeekabooBridge.

## Goals

- Single GUI app instance that owns all TCC-facing work (notifications, screen recording, mic, speech, AppleScript).
- A small surface for automation: Gateway + node commands, plus PeekabooBridge for UI automation.
- Predictable permissions: always the same signed bundle ID, launched by launchd, so TCC grants stick.

## How it works

### Gateway + node transport

- The app runs the Gateway (local mode) and connects to it as a node.
- Agent actions are performed via `node.invoke` (e.g. `system.run`, `system.notify`, `canvas.*`).

### Node service + app IPC

- A headless node host service connects to the Gateway WebSocket.
- `system.run` requests are forwarded to the macOS app over a local Unix socket.
- The app performs the exec in UI context, prompts if needed, and returns output.

Diagram (SCI):

```
Agent -> Gateway -> Node Service (WS)
                      |  IPC (UDS + token + HMAC + TTL)
                      v
                  Mac App (UI + TCC + system.run)
```

### PeekabooBridge (UI automation)

- UI automation uses a separate UNIX socket named `bridge.sock` and the PeekabooBridge JSON protocol.
- Host preference order (client-side): Peekaboo.app → Claude.app → OpenClaw.app → local execution.
- Security: bridge hosts require an allowed TeamID; DEBUG-only same-UID escape hatch is guarded by `PEEKABOO_ALLOW_UNSIGNED_SOCKET_CLIENTS=1` (Peekaboo convention).
- See: [PeekabooBridge usage](https://docs.openclaw.ai/platforms/mac/peekaboo) for details.

## Operational flows

- Restart/rebuild: `SIGN_IDENTITY="Apple Development: <Developer Name> (<TEAMID>)" scripts/restart-mac.sh`
  - Kills existing instances
  - Swift build + package
  - Writes/bootstraps/kickstarts the LaunchAgent
- Single instance: app exits early if another instance with the same bundle ID is running.

## Hardening notes

- Prefer requiring a TeamID match for all privileged surfaces.
- PeekabooBridge: `PEEKABOO_ALLOW_UNSIGNED_SOCKET_CLIENTS=1` (DEBUG-only) may allow same-UID callers for local development.
- All communication remains local-only; no network sockets are exposed.
- TCC prompts originate only from the GUI app bundle; keep the signed bundle ID stable across rebuilds.
- IPC hardening: socket mode `0600`, token, peer-UID checks, HMAC challenge/response, short TTL.

## Related

- [macOS app](https://docs.openclaw.ai/platforms/macos)
- [macOS IPC flow (Exec approvals)](https://docs.openclaw.ai/tools/exec-approvals-advanced#macos-ipc-flow)

[Remote control](https://docs.openclaw.ai/platforms/mac/remote) [Voice wake (macOS)](https://docs.openclaw.ai/platforms/mac/voicewake)

Ctrl+I

---

## macOS app - OpenClaw

_Source: <https://docs.openclaw.ai/platforms/macos>_

[OpenClaw home page](https://docs.openclaw.ai/)

Platforms overview

macOS app

The macOS app is the **menu‑bar companion** for OpenClaw. It owns permissions,
manages/attaches to the Gateway locally (launchd or manual), and exposes macOS
capabilities to the agent as a node.

## What it does

- Shows native notifications and status in the menu bar.
- Owns TCC prompts (Notifications, Accessibility, Screen Recording, Microphone,
Speech Recognition, Automation/AppleScript).
- Runs or connects to the Gateway (local or remote).
- Exposes macOS‑only tools (Canvas, Camera, Screen Recording, `system.run`).
- Starts the local node host service in **remote** mode (launchd), and stops it in **local** mode.
- Optionally hosts **PeekabooBridge** for UI automation.
- Installs the global CLI (`openclaw`) on request via npm, pnpm, or bun (the app prefers npm, then pnpm, then bun; Node remains the recommended Gateway runtime).

## Local vs remote mode

- **Local** (default): the app attaches to a running local Gateway if present;
otherwise it enables the launchd service via `openclaw gateway install`.
- **Remote**: the app connects to a Gateway over SSH/Tailscale and never starts
a local process.
The app starts the local **node host service** so the remote Gateway can reach this Mac.
The app does not spawn the Gateway as a child process.
Gateway discovery now prefers Tailscale MagicDNS names over raw tailnet IPs,
so the Mac app recovers more reliably when tailnet IPs change.

## Launchd control

The app manages a per‑user LaunchAgent labeled `ai.openclaw.gateway`
(or `ai.openclaw.<profile>` when using `--profile`/`OPENCLAW_PROFILE`; legacy `com.openclaw.*` still unloads).

```
launchctl kickstart -k gui/$UID/ai.openclaw.gateway
launchctl bootout gui/$UID/ai.openclaw.gateway
```

Replace the label with `ai.openclaw.<profile>` when running a named profile.If the LaunchAgent isn’t installed, enable it from the app or run
`openclaw gateway install`.

## Node capabilities (mac)

The macOS app presents itself as a node. Common commands:

- Canvas: `canvas.present`, `canvas.navigate`, `canvas.eval`, `canvas.snapshot`, `canvas.a2ui.*`
- Camera: `camera.snap`, `camera.clip`
- Screen: `screen.snapshot`, `screen.record`
- System: `system.run`, `system.notify`

The node reports a `permissions` map so agents can decide what’s allowed.Node service + app IPC:

- When the headless node host service is running (remote mode), it connects to the Gateway WS as a node.
- `system.run` executes in the macOS app (UI/TCC context) over a local Unix socket; prompts + output stay in-app.

Diagram (SCI):

```
Gateway -> Node Service (WS)
                 |  IPC (UDS + token + HMAC + TTL)
                 v
             Mac App (UI + TCC + system.run)
```

## Exec approvals (system.run)

`system.run` is controlled by **Exec approvals** in the macOS app (Settings → Exec approvals).
Security + ask + allowlist are stored locally on the Mac in:

```
~/.openclaw/exec-approvals.json
```

Example:

```
{
  "version": 1,
  "defaults": {
    "security": "deny",
    "ask": "on-miss"
  },
  "agents": {
    "main": {
      "security": "allowlist",
      "ask": "on-miss",
      "allowlist": [{ "pattern": "/opt/homebrew/bin/rg" }]
    }
  }
}
```

Notes:

- `allowlist` entries are glob patterns for resolved binary paths, or bare command names for PATH-invoked commands.
- Raw shell command text that contains shell control or expansion syntax (`&&`, `||`, `;`, `|`, `````, `$`, `<`, `>`, `(`, `)`) is treated as an allowlist miss and requires explicit approval (or allowlisting the shell binary).
- Choosing “Always Allow” in the prompt adds that command to the allowlist.
- `system.run` environment overrides are filtered (drops `PATH`, `DYLD_*`, `LD_*`, `NODE_OPTIONS`, `PYTHON*`, `PERL*`, `RUBYOPT`, `SHELLOPTS`, `PS4`) and then merged with the app’s environment.
- For shell wrappers (`bash|sh|zsh ... -c/-lc`), request-scoped environment overrides are reduced to a small explicit allowlist (`TERM`, `LANG`, `LC_*`, `COLORTERM`, `NO_COLOR`, `FORCE_COLOR`).
- For allow-always decisions in allowlist mode, known dispatch wrappers (`env`, `nice`, `nohup`, `stdbuf`, `timeout`) persist inner executable paths instead of wrapper paths. If unwrapping is not safe, no allowlist entry is persisted automatically.

## Deep links

The app registers the `openclaw://` URL scheme for local actions.

### `openclaw://agent`

Triggers a Gateway `agent` request.

```
open 'openclaw://agent?message=Hello%20from%20deep%20link'
```

Query parameters:

- `message` (required)
- `sessionKey` (optional)
- `thinking` (optional)
- `deliver` / `to` / `channel` (optional)
- `timeoutSeconds` (optional)
- `key` (optional unattended mode key)

Safety:

- Without `key`, the app prompts for confirmation.
- Without `key`, the app enforces a short message limit for the confirmation prompt and ignores `deliver` / `to` / `channel`.
- With a valid `key`, the run is unattended (intended for personal automations).

## Onboarding flow (typical)

1. Install and launch **OpenClaw.app**.
2. Complete the permissions checklist (TCC prompts).
3. Ensure **Local** mode is active and the Gateway is running.
4. Install the CLI if you want terminal access.

## State dir placement (macOS)

Avoid putting your OpenClaw state dir in iCloud or other cloud-synced folders.
Sync-backed paths can add latency and occasionally cause file-lock/sync races for
sessions and credentials.Prefer a local non-synced state path such as:

```
OPENCLAW_STATE_DIR=~/.openclaw
```

If `openclaw doctor` detects state under:

- `~/Library/Mobile Documents/com~apple~CloudDocs/...`
- `~/Library/CloudStorage/...`

it will warn and recommend moving back to a local path.

## Build & dev workflow (native)

- `cd apps/macos && swift build`
- `swift run OpenClaw` (or Xcode)
- Package app: `scripts/package-mac-app.sh`

## Debug gateway connectivity (macOS CLI)

Use the debug CLI to exercise the same Gateway WebSocket handshake and discovery
logic that the macOS app uses, without launching the app.

```
cd apps/macos
swift run openclaw-mac connect --json
swift run openclaw-mac discover --timeout 3000 --json
```

Connect options:

- `--url <ws://host:port>`: override config
- `--mode <local|remote>`: resolve from config (default: config or local)
- `--probe`: force a fresh health probe
- `--timeout <ms>`: request timeout (default: `15000`)
- `--json`: structured output for diffing

Discovery options:

- `--include-local`: include gateways that would be filtered as “local”
- `--timeout <ms>`: overall discovery window (default: `2000`)
- `--json`: structured output for diffing

Compare against `openclaw gateway discover --json` to see whether the macOS app’s discovery pipeline (`local.` plus the configured wide-area domain, with wide-area and Tailscale Serve fallbacks) differs from the Node CLI’s `dns-sd` based discovery.

## Remote connection plumbing (SSH tunnels)

When the macOS app runs in **Remote** mode, it opens an SSH tunnel so local UI
components can talk to a remote Gateway as if it were on localhost.

### Control tunnel (Gateway WebSocket port)

- **Purpose:** health checks, status, Web Chat, config, and other control-plane calls.
- **Local port:** the Gateway port (default `18789`), always stable.
- **Remote port:** the same Gateway port on the remote host.
- **Behavior:** no random local port; the app reuses an existing healthy tunnel
or restarts it if needed.
- **SSH shape:**`ssh -N -L <local>:127.0.0.1:<remote>` with BatchMode +
ExitOnForwardFailure + keepalive options.
- **IP reporting:** the SSH tunnel uses loopback, so the gateway will see the node
IP as `127.0.0.1`. Use **Direct (ws/wss)** transport if you want the real client
IP to appear (see [macOS remote access](https://docs.openclaw.ai/platforms/mac/remote)).

For setup steps, see [macOS remote access](https://docs.openclaw.ai/platforms/mac/remote). For protocol
details, see [Gateway protocol](https://docs.openclaw.ai/gateway/protocol).

## Related docs

- [Gateway runbook](https://docs.openclaw.ai/gateway)
- [Gateway (macOS)](https://docs.openclaw.ai/platforms/mac/bundled-gateway)
- [macOS permissions](https://docs.openclaw.ai/platforms/mac/permissions)
- [Canvas](https://docs.openclaw.ai/platforms/mac/canvas)

[Platforms](https://docs.openclaw.ai/platforms) [Linux app](https://docs.openclaw.ai/platforms/linux)

Ctrl+I

---

## Windows - OpenClaw

_Source: <https://docs.openclaw.ai/platforms/windows>_

# Or pick a distro explicitly:
wsl --list --online
wsl --install -d Ubuntu-24.04
```

Reboot if Windows asks.

### 2) Enable systemd (required for gateway install)

In your WSL terminal:

```
sudo tee /etc/wsl.conf >/dev/null <<'EOF'
[boot]
systemd=true
EOF
```

Then from PowerShell:

```
wsl --shutdown
```

Re-open Ubuntu, then verify:

```
systemctl --user status
```

### 3) Install OpenClaw (inside WSL)

For a normal first-time setup inside WSL, follow the Linux Getting Started flow:

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm build
pnpm ui:build
pnpm openclaw onboard --install-daemon
```

If you are developing from source instead of doing first-time onboarding, use the
source dev loop from [Setup](https://docs.openclaw.ai/start/setup):

```
pnpm install
# First run only (or after resetting local OpenClaw config/workspace)
pnpm openclaw setup
pnpm gateway:watch
```

Full guide: [Getting Started](https://docs.openclaw.ai/start/getting-started)

## Windows companion app

We do not have a Windows companion app yet. Contributions are welcome if you want
contributions to make it happen.

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [Platforms](https://docs.openclaw.ai/platforms)

[Linux app](https://docs.openclaw.ai/platforms/linux) [Android app](https://docs.openclaw.ai/platforms/android)

Ctrl+I

---

## Мережа - OpenClaw

_Source: <https://docs.openclaw.ai/uk/network>_

# Мережевий хаб

Цей хаб містить посилання на основну документацію про те, як OpenClaw підключає, сполучає та захищає
пристрої через localhost, LAN і tailnet.

## Основна модель

Більшість операцій проходить через Gateway (`openclaw gateway`) — єдиний довготривалий процес, який керує підключеннями каналів і площиною керування WebSocket.

- **Спочатку loopback**: WS Gateway типово використовує `ws://127.0.0.1:18789`.
Прив’язки не до loopback вимагають дійсного шляху автентифікації gateway: автентифікація
токеном/паролем зі спільним секретом або правильно налаштоване розгортання
`trusted-proxy` не на loopback.
- **Один Gateway на хост** — рекомендований варіант. Для ізоляції запускайте кілька gateway з ізольованими профілями та портами ( [Кілька Gateway](https://docs.openclaw.ai/uk/gateway/multiple-gateways)).
- **Canvas host** обслуговується на тому самому порту, що й Gateway (`/__openclaw__/canvas/`, `/__openclaw__/a2ui/`), і захищений автентифікацією Gateway, коли прив’язаний не лише до loopback.
- **Віддалений доступ** зазвичай здійснюється через SSH tunnel або VPN Tailscale ( [Віддалений доступ](https://docs.openclaw.ai/uk/gateway/remote)).

Ключові посилання:

- [Архітектура Gateway](https://docs.openclaw.ai/uk/concepts/architecture)
- [Протокол Gateway](https://docs.openclaw.ai/uk/gateway/protocol)
- [Runbook Gateway](https://docs.openclaw.ai/uk/gateway)
- [Вебповерхні та режими прив’язки](https://docs.openclaw.ai/uk/web)

## Сполучення та ідентичність

- [Огляд сполучення (DM + nodes)](https://docs.openclaw.ai/uk/channels/pairing)
- [Сполучення node під керуванням Gateway](https://docs.openclaw.ai/uk/gateway/pairing)
- [CLI Devices (сполучення + ротація токенів)](https://docs.openclaw.ai/uk/cli/devices)
- [CLI Pairing (схвалення DM)](https://docs.openclaw.ai/uk/cli/pairing)

Локальна довіра:

- Прямі локальні підключення loopback можуть автоматично схвалюватися для сполучення, щоб
забезпечити зручний UX на тому самому хості.
- OpenClaw також має вузький шлях самопідключення backend/container-local для
довірених допоміжних потоків зі спільним секретом.
- Клієнти tailnet і LAN, включно з прив’язками tailnet на тому самому хості, все одно потребують
явного схвалення сполучення.

## Виявлення та транспорти

- [Виявлення та транспорти](https://docs.openclaw.ai/uk/gateway/discovery)
- [Bonjour / mDNS](https://docs.openclaw.ai/uk/gateway/bonjour)
- [Віддалений доступ (SSH)](https://docs.openclaw.ai/uk/gateway/remote)
- [Tailscale](https://docs.openclaw.ai/uk/gateway/tailscale)

## Nodes і транспорти

- [Огляд Nodes](https://docs.openclaw.ai/uk/nodes)
- [Протокол Bridge (застарілі nodes, історично)](https://docs.openclaw.ai/uk/gateway/bridge-protocol)
- [Runbook node: iOS](https://docs.openclaw.ai/uk/platforms/ios)
- [Runbook node: Android](https://docs.openclaw.ai/uk/platforms/android)

## Безпека

- [Огляд безпеки](https://docs.openclaw.ai/uk/gateway/security)
- [Довідка з конфігурації Gateway](https://docs.openclaw.ai/uk/gateway/configuration)
- [Усунення несправностей](https://docs.openclaw.ai/uk/gateway/troubleshooting)
- [Doctor](https://docs.openclaw.ai/uk/gateway/doctor)

## Пов’язане

- [Мережева модель Gateway](https://docs.openclaw.ai/uk/gateway/network-model)
- [Віддалений доступ](https://docs.openclaw.ai/uk/gateway/remote)

[Локальні моделі](https://docs.openclaw.ai/uk/gateway/local-models) [Мережева модель](https://docs.openclaw.ai/uk/gateway/network-model)

Ctrl+I

---

## Linux server - OpenClaw

_Source: <https://docs.openclaw.ai/vps>_

[OpenClaw home page](https://docs.openclaw.ai/)

Hosting

Linux server

Run the OpenClaw Gateway on any Linux server or cloud VPS. This page helps you
pick a provider, explains how cloud deployments work, and covers generic Linux
tuning that applies everywhere.

## Pick a provider

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

## How cloud setups work

- The **Gateway runs on the VPS** and owns state + workspace.
- You connect from your laptop or phone via the **Control UI** or **Tailscale/SSH**.
- Treat the VPS as the source of truth and **back up** the state + workspace regularly.
- Secure default: keep the Gateway on loopback and access it via SSH tunnel or Tailscale Serve.
If you bind to `lan` or `tailnet`, require `gateway.auth.token` or `gateway.auth.password`.

Related pages: [Gateway remote access](https://docs.openclaw.ai/gateway/remote), [Platforms hub](https://docs.openclaw.ai/platforms).

## Harden admin access first

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

## Shared company agent on a VPS

Running a single agent for a team is a valid setup when every user is in the same trust boundary and the agent is business-only.

- Keep it on a dedicated runtime (VPS/VM/container + dedicated OS user/accounts).
- Do not sign that runtime into personal Apple/Google accounts or personal browser/password-manager profiles.
- If users are adversarial to each other, split by gateway/host/OS user.

Security model details: [Security](https://docs.openclaw.ai/gateway/security).

## Using nodes with a VPS

You can keep the Gateway in the cloud and pair **nodes** on your local devices
(Mac/iOS/Android/headless). Nodes provide local screen/camera/canvas and `system.run`
capabilities while the Gateway stays in the cloud.Docs: [Nodes](https://docs.openclaw.ai/nodes), [Nodes CLI](https://docs.openclaw.ai/cli/nodes).

## Startup tuning for small VMs and ARM hosts

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

### systemd tuning checklist (optional)

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

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [DigitalOcean](https://docs.openclaw.ai/install/digitalocean)
- [Fly.io](https://docs.openclaw.ai/install/fly)
- [Hetzner](https://docs.openclaw.ai/install/hetzner)

[Kubernetes](https://docs.openclaw.ai/install/kubernetes) [macOS VMs](https://docs.openclaw.ai/install/macos-vm)

Ctrl+I

---

## https://docs.openclaw.ai/vps.md

_Source: <https://docs.openclaw.ai/vps.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Linux server

Run the OpenClaw Gateway on any Linux server or cloud VPS. This page helps you
pick a provider, explains how cloud deployments work, and covers generic Linux
tuning that applies everywhere.

\## Pick a provider

One-click, browser setupOne-click, browser setupSimple paid VPSAlways Free ARM tierFly MachinesDocker on Hetzner VPSVPS with one-click setupCompute EngineLinux VMVM with HTTPS proxyARM self-hosted

\*\*AWS (EC2 / Lightsail / free tier)\*\* also works well.
A community video walkthrough is available at
\[x.com/techfrenAJ/status/2014934471095812547\](https://x.com/techfrenAJ/status/2014934471095812547)
(community resource -- may become unavailable).

\## How cloud setups work

\\* The \*\*Gateway runs on the VPS\*\* and owns state + workspace.
\\* You connect from your laptop or phone via the \*\*Control UI\*\* or \*\*Tailscale/SSH\*\*.
\\* Treat the VPS as the source of truth and \*\*back up\*\* the state + workspace regularly.
\\* Secure default: keep the Gateway on loopback and access it via SSH tunnel or Tailscale Serve.
 If you bind to \`lan\` or \`tailnet\`, require \`gateway.auth.token\` or \`gateway.auth.password\`.

Related pages: \[Gateway remote access\](/gateway/remote), \[Platforms hub\](/platforms).

\## Harden admin access first

Before you install OpenClaw on a public VPS, decide how you want to administer
the box itself.

\\* If you want Tailnet-only admin access, install Tailscale first, join the VPS
 to your tailnet, verify a second SSH session over the Tailscale IP or
 MagicDNS name, then restrict public SSH.
\\* If you are not using Tailscale, apply the equivalent hardening for your SSH
 path before exposing more services.
\\* This is separate from Gateway access. You can still keep OpenClaw bound to
 loopback and use an SSH tunnel or Tailscale Serve for the dashboard.

Tailscale-specific Gateway options live in \[Tailscale\](/gateway/tailscale).

\## Shared company agent on a VPS

Running a single agent for a team is a valid setup when every user is in the same trust boundary and the agent is business-only.

\\* Keep it on a dedicated runtime (VPS/VM/container + dedicated OS user/accounts).
\\* Do not sign that runtime into personal Apple/Google accounts or personal browser/password-manager profiles.
\\* If users are adversarial to each other, split by gateway/host/OS user.

Security model details: \[Security\](/gateway/security).

\## Using nodes with a VPS

You can keep the Gateway in the cloud and pair \*\*nodes\*\* on your local devices
(Mac/iOS/Android/headless). Nodes provide local screen/camera/canvas and \`system.run\`
capabilities while the Gateway stays in the cloud.

Docs: \[Nodes\](/nodes), \[Nodes CLI\](/cli/nodes).

\## Startup tuning for small VMs and ARM hosts

If CLI commands feel slow on low-power VMs (or ARM hosts), enable Node's module compile cache:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
grep -q 'NODE\_COMPILE\_CACHE=/var/tmp/openclaw-compile-cache' ~/.bashrc \|\| cat >> ~/.bashrc <<'EOF'
export NODE\_COMPILE\_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW\_NO\_RESPAWN=1
EOF
source ~/.bashrc
\`\`\`

\\* \`NODE\_COMPILE\_CACHE\` improves repeated command startup times.
\\* \`OPENCLAW\_NO\_RESPAWN=1\` avoids extra startup overhead from a self-respawn path.
\\* First command run warms the cache; subsequent runs are faster.
\\* For Raspberry Pi specifics, see \[Raspberry Pi\](/install/raspberry-pi).

\### systemd tuning checklist (optional)

For VM hosts using \`systemd\`, consider:

\\* Add service env for a stable startup path:
 \\* \`OPENCLAW\_NO\_RESPAWN=1\`
 \\* \`NODE\_COMPILE\_CACHE=/var/tmp/openclaw-compile-cache\`
\\* Keep restart behavior explicit:
 \\* \`Restart=always\`
 \\* \`RestartSec=2\`
 \\* \`TimeoutStartSec=90\`
\\* Prefer SSD-backed disks for state/cache paths to reduce random-I/O cold-start penalties.

For the standard \`openclaw onboard --install-daemon\` path, edit the user unit:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
systemctl --user edit openclaw-gateway.service
\`\`\`

\`\`\`ini theme={"theme":{"light":"min-light","dark":"min-dark"}}
\[Service\]
Environment=OPENCLAW\_NO\_RESPAWN=1
Environment=NODE\_COMPILE\_CACHE=/var/tmp/openclaw-compile-cache
Restart=always
RestartSec=2
TimeoutStartSec=90
\`\`\`

If you deliberately installed a system unit instead, edit
\`openclaw-gateway.service\` via \`sudo systemctl edit openclaw-gateway.service\`.

How \`Restart=\` policies help automated recovery:
\[systemd can automate service recovery\](https://www.redhat.com/en/blog/systemd-automate-recovery).

For Linux OOM behavior, child process victim selection, and \`exit 137\`
diagnostics, see \[Linux memory pressure and OOM kills\](/platforms/linux#memory-pressure-and-oom-kills).

\## Related

\\* \[Install overview\](/install)
\\* \[DigitalOcean\](/install/digitalocean)
\\* \[Fly.io\](/install/fly)
\\* \[Hetzner\](/install/hetzner)

---

## macOS 开发环境设置 - OpenClaw

_Source: <https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup>_

# macOS 开发者设置

从源代码构建并运行 OpenClaw macOS 应用。

## 前置要求

在构建应用之前，请确保你已安装以下内容：

1. **Xcode 26.2+**：Swift 开发所必需。
2. **Node.js 24 和 pnpm**：推荐用于 Gateway 网关、CLI 和打包脚本。Node 22 LTS（当前为 `22.14+`）仍受支持，以保持兼容性。

## 1\. 安装依赖

安装整个项目所需的依赖：

```
pnpm install
```

## 2\. 构建并打包应用

要构建 macOS 应用并将其打包到 `dist/OpenClaw.app`，请运行：

```
./scripts/package-mac-app.sh
```

如果你没有 Apple Developer ID 证书，脚本会自动使用 **临时签名**（`-`）。关于开发运行模式、签名标志以及 Team ID 故障排除，请参阅 macOS 应用 README：
[https://github.com/openclaw/openclaw/blob/main/apps/macos/README.md](https://github.com/openclaw/openclaw/blob/main/apps/macos/README.md)

> **注意**：使用临时签名的应用可能会触发安全提示。如果应用立即崩溃并显示 “Abort trap 6”，请参阅 [故障排除](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#troubleshooting) 部分。

## 3\. 安装 CLI

macOS 应用需要全局安装 `openclaw` CLI 来管理后台任务。**安装方法（推荐）：**

1. 打开 OpenClaw 应用。
2. 前往 **General** 设置标签页。
3. 点击 **“Install CLI”**。

你也可以手动安装：

```
npm install -g openclaw@<version>
```

`pnpm add -g openclaw@<version>` 和 `bun add -g openclaw@<version>` 也可以使用。
对于 Gateway 网关运行时，仍推荐使用 Node。

## 故障排除

### 构建失败：工具链或 SDK 不匹配

macOS 应用构建需要最新的 macOS SDK 和 Swift 6.2 工具链。**系统依赖（必需）：**

- **Software Update 中可用的最新 macOS 版本**（Xcode 26.2 SDK 所必需）
- **Xcode 26.2**（Swift 6.2 工具链）

**检查命令：**

```
xcodebuild -version
xcrun swift --version
```

如果版本不匹配，请更新 macOS / Xcode 后重新构建。

### 授予权限时应用崩溃

如果你在允许 **语音识别** 或 **麦克风** 访问时应用崩溃，可能是由于损坏的 TCC 缓存或签名不匹配导致。**修复方法：**

1. 重置 TCC 权限：

```
tccutil reset All ai.openclaw.mac.debug
```

2. 如果仍然无效，请临时修改 [`scripts/package-mac-app.sh`](https://github.com/openclaw/openclaw/blob/main/scripts/package-mac-app.sh) 中的 `BUNDLE_ID`，以强制 macOS 生成一个“全新状态”。

### Gateway 网关一直显示 “Starting…”

如果 Gateway 网关状态一直停留在 “Starting…”，请检查是否有僵尸进程占用了端口：

```
openclaw gateway status
openclaw gateway stop

# 如果你没有使用 LaunchAgent（开发模式 / 手动运行），请查找监听进程：
lsof -nP -iTCP:18789 -sTCP:LISTEN
```

如果是手动运行的进程占用了端口，请停止该进程（Ctrl+C）。作为最后手段，可以杀掉上面查到的 PID。

## 相关内容

- [macOS 应用](https://docs.openclaw.ai/zh-CN/platforms/macos)
- [安装概览](https://docs.openclaw.ai/zh-CN/install)

[Raspberry Pi（平台）](https://docs.openclaw.ai/zh-CN/platforms/raspberry-pi) [菜单栏](https://docs.openclaw.ai/zh-CN/platforms/mac/menu-bar)

Ctrl+I

---
