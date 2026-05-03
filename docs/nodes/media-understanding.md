---
source_url: https://docs.openclaw.ai/nodes/media-understanding
title: "Media understanding - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/nodes/media-understanding#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Media capabilities

Media understanding

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Goals](https://docs.openclaw.ai/nodes/media-understanding#goals)
- [High-level behavior](https://docs.openclaw.ai/nodes/media-understanding#high-level-behavior)
- [Config overview](https://docs.openclaw.ai/nodes/media-understanding#config-overview)
- [Model entries](https://docs.openclaw.ai/nodes/media-understanding#model-entries)
- [Defaults and limits](https://docs.openclaw.ai/nodes/media-understanding#defaults-and-limits)
- [Auto-detect media understanding (default)](https://docs.openclaw.ai/nodes/media-understanding#auto-detect-media-understanding-default)
- [Proxy environment support (provider models)](https://docs.openclaw.ai/nodes/media-understanding#proxy-environment-support-provider-models)
- [Capabilities (optional)](https://docs.openclaw.ai/nodes/media-understanding#capabilities-optional)
- [Provider support matrix (OpenClaw integrations)](https://docs.openclaw.ai/nodes/media-understanding#provider-support-matrix-openclaw-integrations)
- [Model selection guidance](https://docs.openclaw.ai/nodes/media-understanding#model-selection-guidance)
- [Attachment policy](https://docs.openclaw.ai/nodes/media-understanding#attachment-policy)
- [Config examples](https://docs.openclaw.ai/nodes/media-understanding#config-examples)
- [Status output](https://docs.openclaw.ai/nodes/media-understanding#status-output)
- [Notes](https://docs.openclaw.ai/nodes/media-understanding#notes)
- [Related](https://docs.openclaw.ai/nodes/media-understanding#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw can **summarize inbound media** (image/audio/video) before the reply pipeline runs. It auto-detects when local tools or provider keys are available, and can be disabled or customized. If understanding is off, models still receive the original files/URLs as usual.Vendor-specific media behavior is registered by vendor plugins, while OpenClaw core owns the shared `tools.media` config, fallback order, and reply-pipeline integration.

## [​](https://docs.openclaw.ai/nodes/media-understanding\#goals)  Goals

- Optional: pre-digest inbound media into short text for faster routing + better command parsing.
- Preserve original media delivery to the model (always).
- Support **provider APIs** and **CLI fallbacks**.
- Allow multiple models with ordered fallback (error/size/timeout).

## [​](https://docs.openclaw.ai/nodes/media-understanding\#high-level-behavior)  High-level behavior

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

## [​](https://docs.openclaw.ai/nodes/media-understanding\#config-overview)  Config overview

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

### [​](https://docs.openclaw.ai/nodes/media-understanding\#model-entries)  Model entries

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

## [​](https://docs.openclaw.ai/nodes/media-understanding\#defaults-and-limits)  Defaults and limits

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

### [​](https://docs.openclaw.ai/nodes/media-understanding\#auto-detect-media-understanding-default)  Auto-detect media understanding (default)

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

### [​](https://docs.openclaw.ai/nodes/media-understanding\#proxy-environment-support-provider-models)  Proxy environment support (provider models)

When provider-based **audio** and **video** media understanding is enabled, OpenClaw honors standard outbound proxy environment variables for provider HTTP calls:

- `HTTPS_PROXY`
- `HTTP_PROXY`
- `ALL_PROXY`
- `https_proxy`
- `http_proxy`
- `all_proxy`

If no proxy env vars are set, media understanding uses direct egress. If the proxy value is malformed, OpenClaw logs a warning and falls back to direct fetch.

## [​](https://docs.openclaw.ai/nodes/media-understanding\#capabilities-optional)  Capabilities (optional)

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

## [​](https://docs.openclaw.ai/nodes/media-understanding\#provider-support-matrix-openclaw-integrations)  Provider support matrix (OpenClaw integrations)

| Capability | Provider integration | Notes |
| --- | --- | --- |
| Image | OpenAI, OpenAI Codex OAuth, Codex app-server, OpenRouter, Anthropic, Google, MiniMax, Moonshot, Qwen, Z.AI, config providers | Vendor plugins register image support; `openai-codex/*` uses OAuth provider plumbing; `codex/*` uses a bounded Codex app-server turn; MiniMax and MiniMax OAuth both use `MiniMax-VL-01`; image-capable config providers auto-register. |
| Audio | OpenAI, Groq, xAI, Deepgram, Google, SenseAudio, ElevenLabs, Mistral | Provider transcription (Whisper/Groq/xAI/Deepgram/Gemini/SenseAudio/Scribe/Voxtral). |
| Video | Google, Qwen, Moonshot | Provider video understanding via vendor plugins; Qwen video understanding uses the Standard DashScope endpoints. |

**MiniMax note**

- `minimax` and `minimax-portal` image understanding comes from the plugin-owned `MiniMax-VL-01` media provider.
- The bundled MiniMax text catalog still starts text-only; explicit `models.providers.minimax` entries materialize image-capable M2.7 chat refs.

## [​](https://docs.openclaw.ai/nodes/media-understanding\#model-selection-guidance)  Model selection guidance

- Prefer the strongest latest-generation model available for each media capability when quality and safety matter.
- For tool-enabled agents handling untrusted inputs, avoid older/weaker media models.
- Keep at least one fallback per capability for availability (quality model + faster/cheaper model).
- CLI fallbacks (`whisper-cli`, `whisper`, `gemini`) are useful when provider APIs are unavailable.
- `parakeet-mlx` note: with `--output-dir`, OpenClaw reads `<output-dir>/<media-basename>.txt` when output format is `txt` (or unspecified); non-`txt` formats fall back to stdout.

## [​](https://docs.openclaw.ai/nodes/media-understanding\#attachment-policy)  Attachment policy

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

## [​](https://docs.openclaw.ai/nodes/media-understanding\#config-examples)  Config examples

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

## [​](https://docs.openclaw.ai/nodes/media-understanding\#status-output)  Status output

When media understanding runs, `/status` includes a short summary line:

```
📎 Media: image ok (openai/gpt-5.4) · audio skipped (maxBytes)
```

This shows per-capability outcomes and the chosen provider/model when applicable.

## [​](https://docs.openclaw.ai/nodes/media-understanding\#notes)  Notes

- Understanding is **best-effort**. Errors do not block replies.
- Attachments are still passed to models even when understanding is disabled.
- Use `scope` to limit where understanding runs (e.g. only DMs).

## [​](https://docs.openclaw.ai/nodes/media-understanding\#related)  Related

- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Image & media support](https://docs.openclaw.ai/nodes/images)

[Node troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting) [Image and media support](https://docs.openclaw.ai/nodes/images)

Ctrl+I