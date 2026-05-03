# Nodes

_5 pages from docs.openclaw.ai — full content preserved._

## Contents

- [Nodes - OpenClaw](#nodes---openclaw)
- [Audio and voice notes - OpenClaw](#audio-and-voice-notes---openclaw)
- [Image and media support - OpenClaw](#image-and-media-support---openclaw)
- [Media understanding - OpenClaw](#media-understanding---openclaw)
- [Talk mode - OpenClaw](#talk-mode---openclaw)

---

## Nodes - OpenClaw

_Source: <https://docs.openclaw.ai/nodes>_

# Terminal A (keep running): forward local 18790 -> gateway 127.0.0.1:18789
ssh -N -L 18790:127.0.0.1:18789 user@gateway-host

# Terminal B: export the gateway token and connect through the tunnel
export OPENCLAW_GATEWAY_TOKEN="<gateway-token>"
openclaw node run --host 127.0.0.1 --port 18790 --display-name "Build Node"
```

Notes:

- `openclaw node run` supports token or password auth.
- Env vars are preferred: `OPENCLAW_GATEWAY_TOKEN` / `OPENCLAW_GATEWAY_PASSWORD`.
- Config fallback is `gateway.auth.token` / `gateway.auth.password`.
- In local mode, node host intentionally ignores `gateway.remote.token` / `gateway.remote.password`.
- In remote mode, `gateway.remote.token` / `gateway.remote.password` are eligible per remote precedence rules.
- If active local `gateway.auth.*` SecretRefs are configured but unresolved, node-host auth fails closed.
- Node-host auth resolution only honors `OPENCLAW_GATEWAY_*` env vars.

### Start a node host (service)

```
openclaw node install --host <gateway-host> --port 18789 --display-name "Build Node"
openclaw node start
openclaw node restart
```

### Pair + name

On the gateway host:

```
openclaw devices list
openclaw devices approve <requestId>
openclaw nodes status
```

If the node retries with changed auth details, re-run `openclaw devices list`
and approve the current `requestId`.Naming options:

- `--display-name` on `openclaw node run` / `openclaw node install` (persists in `~/.openclaw/node.json` on the node).
- `openclaw nodes rename --node <id|name|ip> --name "Build Node"` (gateway override).

### Allowlist the commands

Exec approvals are **per node host**. Add allowlist entries from the gateway:

```
openclaw approvals allowlist add --node <id|name|ip> "/usr/bin/uname"
openclaw approvals allowlist add --node <id|name|ip> "/usr/bin/sw_vers"
```

Approvals live on the node host at `~/.openclaw/exec-approvals.json`.

### Point exec at the node

Configure defaults (gateway config):

```
openclaw config set tools.exec.host node
openclaw config set tools.exec.security allowlist
openclaw config set tools.exec.node "<id-or-name>"
```

Or per session:

```
/exec host=node security=allowlist node=<id-or-name>
```

Once set, any `exec` call with `host=node` runs on the node host (subject to the
node allowlist/approvals).`host=auto` will not implicitly choose the node on its own, but an explicit per-call `host=node` request is allowed from `auto`. If you want node exec to be the default for the session, set `tools.exec.host=node` or `/exec host=node ...` explicitly.Related:

- [Node host CLI](https://docs.openclaw.ai/cli/node)
- [Exec tool](https://docs.openclaw.ai/tools/exec)
- [Exec approvals](https://docs.openclaw.ai/tools/exec-approvals)

## Invoking commands

Low-level (raw RPC):

```
openclaw nodes invoke --node <idOrNameOrIp> --command canvas.eval --params '{"javaScript":"location.href"}'
```

Higher-level helpers exist for the common “give the agent a MEDIA attachment” workflows.

## Command policy

Node commands must pass two gates before they can be invoked:

1. The node must declare the command in its WebSocket `connect.commands` list.
2. The gateway’s platform policy must allow the declared command.

Windows and macOS companion nodes allow safe declared commands such as
`canvas.*`, `camera.list`, `location.get`, and `screen.snapshot` by default.
Dangerous or privacy-heavy commands such as `camera.snap`, `camera.clip`, and
`screen.record` still require explicit opt-in with
`gateway.nodes.allowCommands`. `gateway.nodes.denyCommands` always wins over
defaults and extra allowlist entries.Plugin-owned node commands can add a Gateway node-invoke policy. That policy
runs after the allowlist check and before forwarding to the node, so raw
`node.invoke`, CLI helpers, and dedicated agent tools share the same plugin
permission boundary. Dangerous plugin node commands still require explicit
`gateway.nodes.allowCommands` opt-in.After a node changes its declared command list, reject the old device pairing
and approve the new request so the gateway stores the updated command snapshot.

## Screenshots (canvas snapshots)

If the node is showing the Canvas (WebView), `canvas.snapshot` returns `{ format, base64 }`.CLI helper (writes to a temp file and prints `MEDIA:<path>`):

```
openclaw nodes canvas snapshot --node <idOrNameOrIp> --format png
openclaw nodes canvas snapshot --node <idOrNameOrIp> --format jpg --max-width 1200 --quality 0.9
```

### Canvas controls

```
openclaw nodes canvas present --node <idOrNameOrIp> --target https://example.com
openclaw nodes canvas hide --node <idOrNameOrIp>
openclaw nodes canvas navigate https://example.com --node <idOrNameOrIp>
openclaw nodes canvas eval --node <idOrNameOrIp> --js "document.title"
```

Notes:

- `canvas present` accepts URLs or local file paths (`--target`), plus optional `--x/--y/--width/--height` for positioning.
- `canvas eval` accepts inline JS (`--js`) or a positional arg.

### A2UI (Canvas)

```
openclaw nodes canvas a2ui push --node <idOrNameOrIp> --text "Hello"
openclaw nodes canvas a2ui push --node <idOrNameOrIp> --jsonl ./payload.jsonl
openclaw nodes canvas a2ui reset --node <idOrNameOrIp>
```

Notes:

- Only A2UI v0.8 JSONL is supported (v0.9/createSurface is rejected).

## Photos + videos (node camera)

Photos (`jpg`):

```
openclaw nodes camera list --node <idOrNameOrIp>
openclaw nodes camera snap --node <idOrNameOrIp>            # default: both facings (2 MEDIA lines)
openclaw nodes camera snap --node <idOrNameOrIp> --facing front
```

Video clips (`mp4`):

```
openclaw nodes camera clip --node <idOrNameOrIp> --duration 10s
openclaw nodes camera clip --node <idOrNameOrIp> --duration 3000 --no-audio
```

Notes:

- The node must be **foregrounded** for `canvas.*` and `camera.*` (background calls return `NODE_BACKGROUND_UNAVAILABLE`).
- Clip duration is clamped (currently `<= 60s`) to avoid oversized base64 payloads.
- Android will prompt for `CAMERA`/`RECORD_AUDIO` permissions when possible; denied permissions fail with `*_PERMISSION_REQUIRED`.

## Screen recordings (nodes)

Supported nodes expose `screen.record` (mp4). Example:

```
openclaw nodes screen record --node <idOrNameOrIp> --duration 10s --fps 10
openclaw nodes screen record --node <idOrNameOrIp> --duration 10s --fps 10 --no-audio
```

Notes:

- `screen.record` availability depends on node platform.
- Screen recordings are clamped to `<= 60s`.
- `--no-audio` disables microphone capture on supported platforms.
- Use `--screen <index>` to select a display when multiple screens are available.

## Location (nodes)

Nodes expose `location.get` when Location is enabled in settings.CLI helper:

```
openclaw nodes location get --node <idOrNameOrIp>
openclaw nodes location get --node <idOrNameOrIp> --accuracy precise --max-age 15000 --location-timeout 10000
```

Notes:

- Location is **off by default**.
- “Always” requires system permission; background fetch is best-effort.
- The response includes lat/lon, accuracy (meters), and timestamp.

## SMS (Android nodes)

Android nodes can expose `sms.send` when the user grants **SMS** permission and the device supports telephony.Low-level invoke:

```
openclaw nodes invoke --node <idOrNameOrIp> --command sms.send --params '{"to":"+15555550123","message":"Hello from OpenClaw"}'
```

Notes:

- The permission prompt must be accepted on the Android device before the capability is advertised.
- Wi-Fi-only devices without telephony will not advertise `sms.send`.

## Android device + personal data commands

Android nodes can advertise additional command families when the corresponding capabilities are enabled.Available families:

- `device.status`, `device.info`, `device.permissions`, `device.health`
- `notifications.list`, `notifications.actions`
- `photos.latest`
- `contacts.search`, `contacts.add`
- `calendar.events`, `calendar.add`
- `callLog.search`
- `sms.search`
- `motion.activity`, `motion.pedometer`

Example invokes:

```
openclaw nodes invoke --node <idOrNameOrIp> --command device.status --params '{}'
openclaw nodes invoke --node <idOrNameOrIp> --command notifications.list --params '{}'
openclaw nodes invoke --node <idOrNameOrIp> --command photos.latest --params '{"limit":1}'
```

Notes:

- Motion commands are capability-gated by available sensors.

## System commands (node host / mac node)

The macOS node exposes `system.run`, `system.notify`, and `system.execApprovals.get/set`.
The headless node host exposes `system.run`, `system.which`, and `system.execApprovals.get/set`.Examples:

```
openclaw nodes notify --node <idOrNameOrIp> --title "Ping" --body "Gateway ready"
openclaw nodes invoke --node <idOrNameOrIp> --command system.which --params '{"name":"git"}'
```

Notes:

- `system.run` returns stdout/stderr/exit code in the payload.
- Shell execution now goes through the `exec` tool with `host=node`; `nodes` remains the direct-RPC surface for explicit node commands.
- `nodes invoke` does not expose `system.run` or `system.run.prepare`; those stay on the exec path only.
- The exec path prepares a canonical `systemRunPlan` before approval. Once an
approval is granted, the gateway forwards that stored plan, not any later
caller-edited command/cwd/session fields.
- `system.notify` respects notification permission state on the macOS app.
- Unrecognized node `platform` / `deviceFamily` metadata uses a conservative default allowlist that excludes `system.run` and `system.which`. If you intentionally need those commands for an unknown platform, add them explicitly via `gateway.nodes.allowCommands`.
- `system.run` supports `--cwd`, `--env KEY=VAL`, `--command-timeout`, and `--needs-screen-recording`.
- For shell wrappers (`bash|sh|zsh ... -c/-lc`), request-scoped `--env` values are reduced to an explicit allowlist (`TERM`, `LANG`, `LC_*`, `COLORTERM`, `NO_COLOR`, `FORCE_COLOR`).
- For allow-always decisions in allowlist mode, known dispatch wrappers (`env`, `nice`, `nohup`, `stdbuf`, `timeout`) persist inner executable paths instead of wrapper paths. If unwrapping is not safe, no allowlist entry is persisted automatically.
- On Windows node hosts in allowlist mode, shell-wrapper runs via `cmd.exe /c` require approval (allowlist entry alone does not auto-allow the wrapper form).
- `system.notify` supports `--priority <passive|active|timeSensitive>` and `--delivery <system|overlay|auto>`.
- Node hosts ignore `PATH` overrides and strip dangerous startup/shell keys (`DYLD_*`, `LD_*`, `NODE_OPTIONS`, `PYTHON*`, `PERL*`, `RUBYOPT`, `SHELLOPTS`, `PS4`). If you need extra PATH entries, configure the node host service environment (or install tools in standard locations) instead of passing `PATH` via `--env`.
- On macOS node mode, `system.run` is gated by exec approvals in the macOS app (Settings → Exec approvals).
Ask/allowlist/full behave the same as the headless node host; denied prompts return `SYSTEM_RUN_DENIED`.
- On headless node host, `system.run` is gated by exec approvals (`~/.openclaw/exec-approvals.json`).

## Exec node binding

When multiple nodes are available, you can bind exec to a specific node.
This sets the default node for `exec host=node` (and can be overridden per agent).Global default:

```
openclaw config set tools.exec.node "node-id-or-name"
```

Per-agent override:

```
openclaw config get agents.list
openclaw config set agents.list[0].tools.exec.node "node-id-or-name"
```

Unset to allow any node:

```
openclaw config unset tools.exec.node
openclaw config unset agents.list[0].tools.exec.node
```

## Permissions map

Nodes may include a `permissions` map in `node.list` / `node.describe`, keyed by permission name (e.g. `screenRecording`, `accessibility`) with boolean values (`true` = granted).

## Headless node host (cross-platform)

OpenClaw can run a **headless node host** (no UI) that connects to the Gateway
WebSocket and exposes `system.run` / `system.which`. This is useful on Linux/Windows
or for running a minimal node alongside a server.Start it:

```
openclaw node run --host <gateway-host> --port 18789
```

Notes:

- Pairing is still required (the Gateway will show a device pairing prompt).
- The node host stores its node id, token, display name, and gateway connection info in `~/.openclaw/node.json`.
- Exec approvals are enforced locally via `~/.openclaw/exec-approvals.json`
(see [Exec approvals](https://docs.openclaw.ai/tools/exec-approvals)).
- On macOS, the headless node host executes `system.run` locally by default. Set
`OPENCLAW_NODE_EXEC_HOST=app` to route `system.run` through the companion app exec host; add
`OPENCLAW_NODE_EXEC_FALLBACK=0` to require the app host and fail closed if it is unavailable.
- Add `--tls` / `--tls-fingerprint` when the Gateway WS uses TLS.

## Mac node mode

- The macOS menubar app connects to the Gateway WS server as a node (so `openclaw nodes …` works against this Mac).
- In remote mode, the app opens an SSH tunnel for the Gateway port and connects to `localhost`.

[Contributing to the threat model](https://docs.openclaw.ai/security/CONTRIBUTING-THREAT-MODEL) [Node troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting)

Ctrl+I

---

## Audio and voice notes - OpenClaw

_Source: <https://docs.openclaw.ai/nodes/audio>_

# Audio / Voice Notes (2026-01-17)

## What works

- **Media understanding (audio)**: If audio understanding is enabled (or auto‑detected), OpenClaw:

1. Locates the first audio attachment (local path or URL) and downloads it if needed.
2. Enforces `maxBytes` before sending to each model entry.
3. Runs the first eligible model entry in order (provider or CLI).
4. If it fails or skips (size/timeout), it tries the next entry.
5. On success, it replaces `Body` with an `[Audio]` block and sets `{{Transcript}}`.
- **Command parsing**: When transcription succeeds, `CommandBody`/`RawBody` are set to the transcript so slash commands still work.
- **Verbose logging**: In `--verbose`, we log when transcription runs and when it replaces the body.

## Auto-detection (default)

If you **don’t configure models** and `tools.media.audio.enabled` is **not** set to `false`,
OpenClaw auto-detects in this order and stops at the first working option:

1. **Active reply model** when its provider supports audio understanding.
2. **Local CLIs**(if installed)

   - `sherpa-onnx-offline` (requires `SHERPA_ONNX_MODEL_DIR` with encoder/decoder/joiner/tokens)
   - `whisper-cli` (from `whisper-cpp`; uses `WHISPER_CPP_MODEL` or the bundled tiny model)
   - `whisper` (Python CLI; downloads models automatically)
3. **Gemini CLI** (`gemini`) using `read_many_files`
4. **Provider auth**
   - Configured `models.providers.*` entries that support audio are tried first
   - Bundled fallback order: OpenAI → Groq → xAI → Deepgram → Google → SenseAudio → ElevenLabs → Mistral

To disable auto-detection, set `tools.media.audio.enabled: false`.
To customize, set `tools.media.audio.models`.
Note: Binary detection is best-effort across macOS/Linux/Windows; ensure the CLI is on `PATH` (we expand `~`), or set an explicit CLI model with a full command path.

## Config examples

### Provider + CLI fallback (OpenAI + Whisper CLI)

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        maxBytes: 20971520,
        models: [\
          { provider: "openai", model: "gpt-4o-mini-transcribe" },\
          {\
            type: "cli",\
            command: "whisper",\
            args: ["--model", "base", "{{MediaPath}}"],\
            timeoutSeconds: 45,\
          },\
        ],
      },
    },
  },
}
```

### Provider-only with scope gating

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        scope: {
          default: "allow",
          rules: [{ action: "deny", match: { chatType: "group" } }],
        },
        models: [{ provider: "openai", model: "gpt-4o-mini-transcribe" }],
      },
    },
  },
}
```

### Provider-only (Deepgram)

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "deepgram", model: "nova-3" }],
      },
    },
  },
}
```

### Provider-only (Mistral Voxtral)

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "mistral", model: "voxtral-mini-latest" }],
      },
    },
  },
}
```

### Provider-only (SenseAudio)

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "senseaudio", model: "senseaudio-asr-pro-1.5-260319" }],
      },
    },
  },
}
```

### Echo transcript to chat (opt-in)

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        echoTranscript: true, // default is false
        echoFormat: '📝 "{transcript}"', // optional, supports {transcript}
        models: [{ provider: "openai", model: "gpt-4o-mini-transcribe" }],
      },
    },
  },
}
```

## Notes & limits

- Provider auth follows the standard model auth order (auth profiles, env vars, `models.providers.*.apiKey`).
- Groq setup details: [Groq](https://docs.openclaw.ai/providers/groq).
- Deepgram picks up `DEEPGRAM_API_KEY` when `provider: "deepgram"` is used.
- Deepgram setup details: [Deepgram (audio transcription)](https://docs.openclaw.ai/providers/deepgram).
- Mistral setup details: [Mistral](https://docs.openclaw.ai/providers/mistral).
- SenseAudio picks up `SENSEAUDIO_API_KEY` when `provider: "senseaudio"` is used.
- SenseAudio setup details: [SenseAudio](https://docs.openclaw.ai/providers/senseaudio).
- Audio providers can override `baseUrl`, `headers`, and `providerOptions` via `tools.media.audio`.
- Default size cap is 20MB (`tools.media.audio.maxBytes`). Oversize audio is skipped for that model and the next entry is tried.
- Tiny/empty audio files below 1024 bytes are skipped before provider/CLI transcription.
- Default `maxChars` for audio is **unset** (full transcript). Set `tools.media.audio.maxChars` or per-entry `maxChars` to trim output.
- OpenAI auto default is `gpt-4o-mini-transcribe`; set `model: "gpt-4o-transcribe"` for higher accuracy.
- Use `tools.media.audio.attachments` to process multiple voice notes (`mode: "all"` \+ `maxAttachments`).
- Transcript is available to templates as `{{Transcript}}`.
- `tools.media.audio.echoTranscript` is off by default; enable it to send transcript confirmation back to the originating chat before agent processing.
- `tools.media.audio.echoFormat` customizes the echo text (placeholder: `{transcript}`).
- CLI stdout is capped (5MB); keep CLI output concise.
- CLI `args` should use `{{MediaPath}}` for the local audio file path. Run `openclaw doctor --fix` to migrate deprecated `{input}` placeholders from older `audio.transcription.command` configs.

### Proxy environment support

Provider-based audio transcription honors standard outbound proxy env vars:

- `HTTPS_PROXY`
- `HTTP_PROXY`
- `ALL_PROXY`
- `https_proxy`
- `http_proxy`
- `all_proxy`

If no proxy env vars are set, direct egress is used. If proxy config is malformed, OpenClaw logs a warning and falls back to direct fetch.

## Mention detection in groups

When `requireMention: true` is set for a group chat, OpenClaw now transcribes audio **before** checking for mentions. This allows voice notes to be processed even when they contain mentions.**How it works:**

1. If a voice message has no text body and the group requires mentions, OpenClaw performs a “preflight” transcription.
2. The transcript is checked for mention patterns (e.g., `@BotName`, emoji triggers).
3. If a mention is found, the message proceeds through the full reply pipeline.
4. The transcript is used for mention detection so voice notes can pass the mention gate.

**Fallback behavior:**

- If transcription fails during preflight (timeout, API error, etc.), the message is processed based on text-only mention detection.
- This ensures that mixed messages (text + audio) are never incorrectly dropped.

**Opt-out per Telegram group/topic:**

- Set `channels.telegram.groups.<chatId>.disableAudioPreflight: true` to skip preflight transcript mention checks for that group.
- Set `channels.telegram.groups.<chatId>.topics.<threadId>.disableAudioPreflight` to override per-topic (`true` to skip, `false` to force-enable).
- Default is `false` (preflight enabled when mention-gated conditions match).

**Example:** A user sends a voice note saying “Hey @Claude, what’s the weather?” in a Telegram group with `requireMention: true`. The voice note is transcribed, the mention is detected, and the agent replies.

## Gotchas

- Scope rules use first-match wins. `chatType` is normalized to `direct`, `group`, or `room`.
- Ensure your CLI exits 0 and prints plain text; JSON needs to be massaged via `jq -r .text`.
- For `parakeet-mlx`, if you pass `--output-dir`, OpenClaw reads `<output-dir>/<media-basename>.txt` when `--output-format` is `txt` (or omitted); non-`txt` output formats fall back to stdout parsing.
- Keep timeouts reasonable (`timeoutSeconds`, default 60s) to avoid blocking the reply queue.
- Preflight transcription only processes the **first** audio attachment for mention detection. Additional audio is processed during the main media understanding phase.

## Related

- [Media understanding](https://docs.openclaw.ai/nodes/media-understanding)
- [Talk mode](https://docs.openclaw.ai/nodes/talk)
- [Voice wake](https://docs.openclaw.ai/nodes/voicewake)

[Image and media support](https://docs.openclaw.ai/nodes/images) [Camera capture](https://docs.openclaw.ai/nodes/camera)

Ctrl+I

---

## Image and media support - OpenClaw

_Source: <https://docs.openclaw.ai/nodes/images>_

# Image & Media Support (2025-12-05)

The WhatsApp channel runs via **Baileys Web**. This document captures the current media handling rules for send, gateway, and agent replies.

## Goals

- Send media with optional captions via `openclaw message send --media`.
- Allow auto-replies from the web inbox to include media alongside text.
- Keep per-type limits sane and predictable.

## CLI Surface

- `openclaw message send --media <path-or-url> [--message <caption>]`
  - `--media` optional; caption can be empty for media-only sends.
  - `--dry-run` prints the resolved payload; `--json` emits `{ channel, to, messageId, mediaUrl, caption }`.

## WhatsApp Web channel behavior

- Input: local file path **or** HTTP(S) URL.
- Flow: load into a Buffer, detect media kind, and build the correct payload:
  - **Images:** resize & recompress to JPEG (max side 2048px) targeting `channels.whatsapp.mediaMaxMb` (default: 50 MB).
  - **Audio/Voice/Video:** pass-through up to 16 MB; audio is sent as a voice note (`ptt: true`).
  - **Documents:** anything else, up to 100 MB, with filename preserved when available.
- WhatsApp GIF-style playback: send an MP4 with `gifPlayback: true` (CLI: `--gif-playback`) so mobile clients loop inline.
- MIME detection prefers magic bytes, then headers, then file extension.
- Caption comes from `--message` or `reply.text`; empty caption is allowed.
- Logging: non-verbose shows `↩️`/`✅`; verbose includes size and source path/URL.

## Auto-Reply Pipeline

- `getReplyFromConfig` returns `{ text?, mediaUrl?, mediaUrls? }`.
- When media is present, the web sender resolves local paths or URLs using the same pipeline as `openclaw message send`.
- Multiple media entries are sent sequentially if provided.

## Inbound media to commands (Pi)

- When inbound web messages include media, OpenClaw downloads to a temp file and exposes templating variables:
  - `{{MediaUrl}}` pseudo-URL for the inbound media.
  - `{{MediaPath}}` local temp path written before running the command.
- When a per-session Docker sandbox is enabled, inbound media is copied into the sandbox workspace and `MediaPath`/`MediaUrl` are rewritten to a relative path like `media/inbound/<filename>`.
- Media understanding (if configured via `tools.media.*` or shared `tools.media.models`) runs before templating and can insert `[Image]`, `[Audio]`, and `[Video]` blocks into `Body`.

  - Audio sets `{{Transcript}}` and uses the transcript for command parsing so slash commands still work.
  - Video and image descriptions preserve any caption text for command parsing.
  - If the active primary image model already supports vision natively, OpenClaw skips the `[Image]` summary block and passes the original image to the model instead.
- By default only the first matching image/audio/video attachment is processed; set `tools.media.<cap>.attachments` to process multiple attachments.

## Limits & Errors

**Outbound send caps (WhatsApp web send)**

- Images: up to `channels.whatsapp.mediaMaxMb` (default: 50 MB) after recompression.
- Audio/voice/video: 16 MB cap; documents: 100 MB cap.
- Oversize or unreadable media → clear error in logs and the reply is skipped.

**Media understanding caps (transcription/description)**

- Image default: 10 MB (`tools.media.image.maxBytes`).
- Audio default: 20 MB (`tools.media.audio.maxBytes`).
- Video default: 50 MB (`tools.media.video.maxBytes`).
- Oversize media skips understanding, but replies still go through with the original body.

## Notes for Tests

- Cover send + reply flows for image/audio/document cases.
- Validate recompression for images (size bound) and voice-note flag for audio.
- Ensure multi-media replies fan out as sequential sends.

## Related

- [Camera capture](https://docs.openclaw.ai/nodes/camera)
- [Media understanding](https://docs.openclaw.ai/nodes/media-understanding)
- [Audio and voice notes](https://docs.openclaw.ai/nodes/audio)

[Media understanding](https://docs.openclaw.ai/nodes/media-understanding) [Audio and voice notes](https://docs.openclaw.ai/nodes/audio)

Ctrl+I

---

## Media understanding - OpenClaw

_Source: <https://docs.openclaw.ai/nodes/media-understanding>_

[OpenClaw home page](https://docs.openclaw.ai/)

Media capabilities

Media understanding

OpenClaw can **summarize inbound media** (image/audio/video) before the reply pipeline runs. It auto-detects when local tools or provider keys are available, and can be disabled or customized. If understanding is off, models still receive the original files/URLs as usual.Vendor-specific media behavior is registered by vendor plugins, while OpenClaw core owns the shared `tools.media` config, fallback order, and reply-pipeline integration.

## Goals

- Optional: pre-digest inbound media into short text for faster routing + better command parsing.
- Preserve original media delivery to the model (always).
- Support **provider APIs** and **CLI fallbacks**.
- Allow multiple models with ordered fallback (error/size/timeout).

## High-level behavior

1

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

Collect attachments

Collect inbound attachments (`MediaPaths`, `MediaUrls`, `MediaTypes`).

2

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

Select per-capability

For each enabled capability (image/audio/video), select attachments per policy (default: **first**).

3

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

Choose model

Choose the first eligible model entry (size + capability + auth).

4

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

Fallback on failure

If a model fails or the media is too large, **fall back to the next entry**.

5

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

Apply success block

On success:

- `Body` becomes `[Image]`, `[Audio]`, or `[Video]` block.
- Audio sets `{{Transcript}}`; command parsing uses caption text when present, otherwise the transcript.
- Captions are preserved as `User text:` inside the block.

If understanding fails or is disabled, **the reply flow continues** with the original body + attachments.

## Config overview

`tools.media` supports **shared models** plus per-capability overrides:

Top-level keys

- `tools.media.models`: shared model list (use `capabilities` to gate).
- `tools.media.image` / `tools.media.audio` / `tools.media.video`:

  - defaults (`prompt`, `maxChars`, `maxBytes`, `timeoutSeconds`, `language`)
  - provider overrides (`baseUrl`, `headers`, `providerOptions`)
  - Deepgram audio options via `tools.media.audio.providerOptions.deepgram`
  - audio transcript echo controls (`echoTranscript`, default `false`; `echoFormat`)
  - optional **per-capability `models` list** (preferred before shared models)
  - `attachments` policy (`mode`, `maxAttachments`, `prefer`)
  - `scope` (optional gating by channel/chatType/session key)
- `tools.media.concurrency`: max concurrent capability runs (default **2**).

```
{
  tools: {
    media: {
      models: [\
        /* shared list */\
      ],
      image: {
        /* optional overrides */
      },
      audio: {
        /* optional overrides */
        echoTranscript: true,
        echoFormat: '📝 "{transcript}"',
      },
      video: {
        /* optional overrides */
      },
    },
  },
}
```

### Model entries

Each `models[]` entry can be **provider** or **CLI**:

- Provider entry

- CLI entry

```
{
  type: "provider", // default if omitted
  provider: "openai",
  model: "gpt-5.5",
  prompt: "Describe the image in <= 500 chars.",
  maxChars: 500,
  maxBytes: 10485760,
  timeoutSeconds: 60,
  capabilities: ["image"], // optional, used for multi-modal entries
  profile: "vision-profile",
  preferredProfile: "vision-fallback",
}
```

```
{
  type: "cli",
  command: "gemini",
  args: [\
    "-m",\
    "gemini-3-flash",\
    "--allowed-tools",\
    "read_file",\
    "Read the media at {{MediaPath}} and describe it in <= {{MaxChars}} characters.",\
  ],
  maxChars: 500,
  maxBytes: 52428800,
  timeoutSeconds: 120,
  capabilities: ["video", "image"],
}
```

CLI templates can also use:

- `{{MediaDir}}` (directory containing the media file)
- `{{OutputDir}}` (scratch dir created for this run)
- `{{OutputBase}}` (scratch file base path, no extension)

## Defaults and limits

Recommended defaults:

- `maxChars`: **500** for image/video (short, command-friendly)
- `maxChars`: **unset** for audio (full transcript unless you set a limit)
- `maxBytes`:

  - image: **10MB**
  - audio: **20MB**
  - video: **50MB**

Rules

- If media exceeds `maxBytes`, that model is skipped and the **next model is tried**.
- Audio files smaller than **1024 bytes** are treated as empty/corrupt and skipped before provider/CLI transcription; inbound reply context receives a deterministic placeholder transcript so the agent knows the note was too small.
- If the model returns more than `maxChars`, output is trimmed.
- `prompt` defaults to simple “Describe the .” plus the `maxChars` guidance (image/video only).
- If the active primary image model already supports vision natively, OpenClaw skips the `[Image]` summary block and passes the original image into the model instead.
- If a Gateway/WebChat primary model is text-only, image attachments are preserved as offloaded `media://inbound/*` refs so the image/PDF tools or configured image model can still inspect them instead of losing the attachment.
- Explicit `openclaw infer image describe --model <provider/model>` requests are different: they run that image-capable provider/model directly, including Ollama refs such as `ollama/qwen2.5vl:7b`.
- If `<capability>.enabled: true` but no models are configured, OpenClaw tries the **active reply model** when its provider supports the capability.

### Auto-detect media understanding (default)

If `tools.media.<capability>.enabled` is **not** set to `false` and you haven’t configured models, OpenClaw auto-detects in this order and **stops at the first working option**:

1

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

Active reply model

Active reply model when its provider supports the capability.

2

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

agents.defaults.imageModel

`agents.defaults.imageModel` primary/fallback refs (image only).
Prefer `provider/model` refs. Bare refs are qualified from configured image-capable provider model entries only when the match is unique.

3

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

Local CLIs (audio only)

Local CLIs (if installed):

- `sherpa-onnx-offline` (requires `SHERPA_ONNX_MODEL_DIR` with encoder/decoder/joiner/tokens)
- `whisper-cli` (`whisper-cpp`; uses `WHISPER_CPP_MODEL` or the bundled tiny model)
- `whisper` (Python CLI; downloads models automatically)

4

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

Gemini CLI

`gemini` using `read_many_files`.

5

[Navigate to header](https://docs.openclaw.ai/nodes/media-understanding#)

Provider auth

- Configured `models.providers.*` entries that support the capability are tried before the bundled fallback order.
- Image-only config providers with an image-capable model auto-register for media understanding even when they are not a bundled vendor plugin.
- Ollama image understanding is available when selected explicitly, for example through `agents.defaults.imageModel` or `openclaw infer image describe --model ollama/<vision-model>`.

Bundled fallback order:

- Audio: OpenAI → Groq → xAI → Deepgram → Google → SenseAudio → ElevenLabs → Mistral
- Image: OpenAI → Anthropic → Google → MiniMax → MiniMax Portal → Z.AI
- Video: Google → Qwen → Moonshot

To disable auto-detection, set:

```
{
  tools: {
    media: {
      audio: {
        enabled: false,
      },
    },
  },
}
```

Binary detection is best-effort across macOS/Linux/Windows; ensure the CLI is on `PATH` (we expand `~`), or set an explicit CLI model with a full command path.

### Proxy environment support (provider models)

When provider-based **audio** and **video** media understanding is enabled, OpenClaw honors standard outbound proxy environment variables for provider HTTP calls:

- `HTTPS_PROXY`
- `HTTP_PROXY`
- `ALL_PROXY`
- `https_proxy`
- `http_proxy`
- `all_proxy`

If no proxy env vars are set, media understanding uses direct egress. If the proxy value is malformed, OpenClaw logs a warning and falls back to direct fetch.

## Capabilities (optional)

If you set `capabilities`, the entry only runs for those media types. For shared lists, OpenClaw can infer defaults:

- `openai`, `anthropic`, `minimax`: **image**
- `minimax-portal`: **image**
- `moonshot`: **image + video**
- `openrouter`: **image**
- `google` (Gemini API): **image + audio + video**
- `qwen`: **image + video**
- `mistral`: **audio**
- `zai`: **image**
- `groq`: **audio**
- `xai`: **audio**
- `deepgram`: **audio**
- Any `models.providers.<id>.models[]` catalog with an image-capable model: **image**

For CLI entries, **set `capabilities` explicitly** to avoid surprising matches. If you omit `capabilities`, the entry is eligible for the list it appears in.

## Provider support matrix (OpenClaw integrations)

| Capability | Provider integration | Notes |
| --- | --- | --- |
| Image | OpenAI, OpenAI Codex OAuth, Codex app-server, OpenRouter, Anthropic, Google, MiniMax, Moonshot, Qwen, Z.AI, config providers | Vendor plugins register image support; `openai-codex/*` uses OAuth provider plumbing; `codex/*` uses a bounded Codex app-server turn; MiniMax and MiniMax OAuth both use `MiniMax-VL-01`; image-capable config providers auto-register. |
| Audio | OpenAI, Groq, xAI, Deepgram, Google, SenseAudio, ElevenLabs, Mistral | Provider transcription (Whisper/Groq/xAI/Deepgram/Gemini/SenseAudio/Scribe/Voxtral). |
| Video | Google, Qwen, Moonshot | Provider video understanding via vendor plugins; Qwen video understanding uses the Standard DashScope endpoints. |

**MiniMax note**

- `minimax` and `minimax-portal` image understanding comes from the plugin-owned `MiniMax-VL-01` media provider.
- The bundled MiniMax text catalog still starts text-only; explicit `models.providers.minimax` entries materialize image-capable M2.7 chat refs.

## Model selection guidance

- Prefer the strongest latest-generation model available for each media capability when quality and safety matter.
- For tool-enabled agents handling untrusted inputs, avoid older/weaker media models.
- Keep at least one fallback per capability for availability (quality model + faster/cheaper model).
- CLI fallbacks (`whisper-cli`, `whisper`, `gemini`) are useful when provider APIs are unavailable.
- `parakeet-mlx` note: with `--output-dir`, OpenClaw reads `<output-dir>/<media-basename>.txt` when output format is `txt` (or unspecified); non-`txt` formats fall back to stdout.

## Attachment policy

Per-capability `attachments` controls which attachments are processed:

[​](https://docs.openclaw.ai/nodes/media-understanding#param-mode)

mode

"first" \| "all"

default:"first"

Whether to process the first selected attachment or all of them.

[​](https://docs.openclaw.ai/nodes/media-understanding#param-max-attachments)

maxAttachments

number

default:"1"

Cap the number processed.

[​](https://docs.openclaw.ai/nodes/media-understanding#param-prefer)

prefer

"first" \| "last" \| "path" \| "url"

Selection preference among candidate attachments.

When `mode: "all"`, outputs are labeled `[Image 1/2]`, `[Audio 2/2]`, etc.

File-attachment extraction behavior

- Extracted file text is wrapped as **untrusted external content** before it is appended to the media prompt.
- The injected block uses explicit boundary markers like `<<<EXTERNAL_UNTRUSTED_CONTENT id="...">>>` / `<<<END_EXTERNAL_UNTRUSTED_CONTENT id="...">>>` and includes a `Source: External` metadata line.
- This attachment-extraction path intentionally omits the long `SECURITY NOTICE:` banner to avoid bloating the media prompt; the boundary markers and metadata still remain.
- If a file has no extractable text, OpenClaw injects `[No extractable text]`.
- If a PDF falls back to rendered page images in this path, the media prompt keeps the placeholder `[PDF content rendered to images; images not forwarded to model]` because this attachment-extraction step forwards text blocks, not the rendered PDF images.

## Config examples

- Shared models + overrides

- Audio + video only

- Image-only

- Multi-modal single entry

```
{
  tools: {
    media: {
      models: [\
        { provider: "openai", model: "gpt-5.5", capabilities: ["image"] },\
        {\
          provider: "google",\
          model: "gemini-3-flash-preview",\
          capabilities: ["image", "audio", "video"],\
        },\
        {\
          type: "cli",\
          command: "gemini",\
          args: [\
            "-m",\
            "gemini-3-flash",\
            "--allowed-tools",\
            "read_file",\
            "Read the media at {{MediaPath}} and describe it in <= {{MaxChars}} characters.",\
          ],\
          capabilities: ["image", "video"],\
        },\
      ],
      audio: {
        attachments: { mode: "all", maxAttachments: 2 },
      },
      video: {
        maxChars: 500,
      },
    },
  },
}
```

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [\
          { provider: "openai", model: "gpt-4o-mini-transcribe" },\
          {\
            type: "cli",\
            command: "whisper",\
            args: ["--model", "base", "{{MediaPath}}"],\
          },\
        ],
      },
      video: {
        enabled: true,
        maxChars: 500,
        models: [\
          { provider: "google", model: "gemini-3-flash-preview" },\
          {\
            type: "cli",\
            command: "gemini",\
            args: [\
              "-m",\
              "gemini-3-flash",\
              "--allowed-tools",\
              "read_file",\
              "Read the media at {{MediaPath}} and describe it in <= {{MaxChars}} characters.",\
            ],\
          },\
        ],
      },
    },
  },
}
```

```
{
  tools: {
    media: {
      image: {
        enabled: true,
        maxBytes: 10485760,
        maxChars: 500,
        models: [\
          { provider: "openai", model: "gpt-5.5" },\
          { provider: "anthropic", model: "claude-opus-4-6" },\
          {\
            type: "cli",\
            command: "gemini",\
            args: [\
              "-m",\
              "gemini-3-flash",\
              "--allowed-tools",\
              "read_file",\
              "Read the media at {{MediaPath}} and describe it in <= {{MaxChars}} characters.",\
            ],\
          },\
        ],
      },
    },
  },
}
```

```
{
  tools: {
    media: {
      image: {
        models: [\
          {\
            provider: "google",\
            model: "gemini-3.1-pro-preview",\
            capabilities: ["image", "video", "audio"],\
          },\
        ],
      },
      audio: {
        models: [\
          {\
            provider: "google",\
            model: "gemini-3.1-pro-preview",\
            capabilities: ["image", "video", "audio"],\
          },\
        ],
      },
      video: {
        models: [\
          {\
            provider: "google",\
            model: "gemini-3.1-pro-preview",\
            capabilities: ["image", "video", "audio"],\
          },\
        ],
      },
    },
  },
}
```

## Status output

When media understanding runs, `/status` includes a short summary line:

```
📎 Media: image ok (openai/gpt-5.4) · audio skipped (maxBytes)
```

This shows per-capability outcomes and the chosen provider/model when applicable.

## Notes

- Understanding is **best-effort**. Errors do not block replies.
- Attachments are still passed to models even when understanding is disabled.
- Use `scope` to limit where understanding runs (e.g. only DMs).

## Related

- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Image & media support](https://docs.openclaw.ai/nodes/images)

[Node troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting) [Image and media support](https://docs.openclaw.ai/nodes/images)

Ctrl+I

---

## Talk mode - OpenClaw

_Source: <https://docs.openclaw.ai/nodes/talk>_

[OpenClaw home page](https://docs.openclaw.ai/)

Node features

Talk mode

Talk mode is a continuous voice conversation loop:

1. Listen for speech
2. Send transcript to the model (main session, chat.send)
3. Wait for the response
4. Speak it via the configured Talk provider (`talk.speak`)

## Behavior (macOS)

- **Always-on overlay** while Talk mode is enabled.
- **Listening → Thinking → Speaking** phase transitions.
- On a **short pause** (silence window), the current transcript is sent.
- Replies are **written to WebChat** (same as typing).
- **Interrupt on speech** (default on): if the user starts talking while the assistant is speaking, we stop playback and note the interruption timestamp for the next prompt.

## Voice directives in replies

The assistant may prefix its reply with a **single JSON line** to control voice:

```
{ "voice": "<voice-id>", "once": true }
```

Rules:

- First non-empty line only.
- Unknown keys are ignored.
- `once: true` applies to the current reply only.
- Without `once`, the voice becomes the new default for Talk mode.
- The JSON line is stripped before TTS playback.

Supported keys:

- `voice` / `voice_id` / `voiceId`
- `model` / `model_id` / `modelId`
- `speed`, `rate` (WPM), `stability`, `similarity`, `style`, `speakerBoost`
- `seed`, `normalize`, `lang`, `output_format`, `latency_tier`
- `once`

## Config (`~/.openclaw/openclaw.json`)

```
{
  talk: {
    provider: "elevenlabs",
    providers: {
      elevenlabs: {
        voiceId: "elevenlabs_voice_id",
        modelId: "eleven_v3",
        outputFormat: "mp3_44100_128",
        apiKey: "elevenlabs_api_key",
      },
      mlx: {
        modelId: "mlx-community/Soprano-80M-bf16",
      },
      system: {},
    },
    speechLocale: "ru-RU",
    silenceTimeoutMs: 1500,
    interruptOnSpeech: true,
  },
}
```

Defaults:

- `interruptOnSpeech`: true
- `silenceTimeoutMs`: when unset, Talk keeps the platform default pause window before sending the transcript (`700 ms on macOS and Android, 900 ms on iOS`)
- `provider`: selects the active Talk provider. Use `elevenlabs`, `mlx`, or `system` for the macOS-local playback paths.
- `providers.<provider>.voiceId`: falls back to `ELEVENLABS_VOICE_ID` / `SAG_VOICE_ID` for ElevenLabs (or first ElevenLabs voice when API key is available).
- `providers.elevenlabs.modelId`: defaults to `eleven_v3` when unset.
- `providers.mlx.modelId`: defaults to `mlx-community/Soprano-80M-bf16` when unset.
- `providers.elevenlabs.apiKey`: falls back to `ELEVENLABS_API_KEY` (or gateway shell profile if available).
- `speechLocale`: optional BCP 47 locale id for on-device Talk speech recognition on iOS/macOS. Leave unset to use the device default.
- `outputFormat`: defaults to `pcm_44100` on macOS/iOS and `pcm_24000` on Android (set `mp3_*` to force MP3 streaming)

## macOS UI

- Menu bar toggle: **Talk**
- Config tab: **Talk Mode** group (voice id + interrupt toggle)
- Overlay:
  - **Listening**: cloud pulses with mic level
  - **Thinking**: sinking animation
  - **Speaking**: radiating rings
  - Click cloud: stop speaking
  - Click X: exit Talk mode

## Android UI

- Voice tab toggle: **Talk**
- Manual **Mic** and **Talk** are mutually exclusive runtime capture modes.
- Manual Mic stops when the app leaves the foreground or the user leaves the Voice tab.
- Talk Mode keeps running until toggled off or the Android node disconnects, and uses Android’s microphone foreground-service type while active.

## Notes

- Requires Speech + Microphone permissions.
- Uses `chat.send` against session key `main`.
- The gateway resolves Talk playback through `talk.speak` using the active Talk provider. Android falls back to local system TTS only when that RPC is unavailable.
- macOS local MLX playback uses the bundled `openclaw-mlx-tts` helper when present, or an executable on `PATH`. Set `OPENCLAW_MLX_TTS_BIN` to point at a custom helper binary during development.
- `stability` for `eleven_v3` is validated to `0.0`, `0.5`, or `1.0`; other models accept `0..1`.
- `latency_tier` is validated to `0..4` when set.
- Android supports `pcm_16000`, `pcm_22050`, `pcm_24000`, and `pcm_44100` output formats for low-latency AudioTrack streaming.

## Related

- [Voice wake](https://docs.openclaw.ai/nodes/voicewake)
- [Audio and voice notes](https://docs.openclaw.ai/nodes/audio)
- [Media understanding](https://docs.openclaw.ai/nodes/media-understanding)

[Text to speech (TTS)](https://docs.openclaw.ai/tools/tts) [Voice wake](https://docs.openclaw.ai/nodes/voicewake)

Ctrl+I

---
