---
source_url: https://docs.openclaw.ai/tools/image-generation
title: "Image generation - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/image-generation#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Tools

Image generation

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/tools/image-generation#quick-start)
- [Common routes](https://docs.openclaw.ai/tools/image-generation#common-routes)
- [Supported providers](https://docs.openclaw.ai/tools/image-generation#supported-providers)
- [Provider capabilities](https://docs.openclaw.ai/tools/image-generation#provider-capabilities)
- [Tool parameters](https://docs.openclaw.ai/tools/image-generation#tool-parameters)
- [Configuration](https://docs.openclaw.ai/tools/image-generation#configuration)
- [Model selection](https://docs.openclaw.ai/tools/image-generation#model-selection)
- [Provider selection order](https://docs.openclaw.ai/tools/image-generation#provider-selection-order)
- [Image editing](https://docs.openclaw.ai/tools/image-generation#image-editing)
- [Provider deep dives](https://docs.openclaw.ai/tools/image-generation#provider-deep-dives)
- [Examples](https://docs.openclaw.ai/tools/image-generation#examples)
- [Related](https://docs.openclaw.ai/tools/image-generation#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The `image_generate` tool lets the agent create and edit images using your
configured providers. Generated images are delivered automatically as media
attachments in the agent’s reply.

The tool only appears when at least one image-generation provider is
available. If you do not see `image_generate` in your agent’s tools,
configure `agents.defaults.imageGenerationModel`, set up a provider API key,
or sign in with OpenAI Codex OAuth.

## [​](https://docs.openclaw.ai/tools/image-generation\#quick-start)  Quick start

1

[Navigate to header](https://docs.openclaw.ai/tools/image-generation#)

Configure auth

Set an API key for at least one provider (for example `OPENAI_API_KEY`,
`GEMINI_API_KEY`, `OPENROUTER_API_KEY`) or sign in with OpenAI Codex OAuth.

2

[Navigate to header](https://docs.openclaw.ai/tools/image-generation#)

Pick a default model (optional)

```
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "openai/gpt-image-2",
        timeoutMs: 180_000,
      },
    },
  },
}
```

Codex OAuth uses the same `openai/gpt-image-2` model ref. When an
`openai-codex` OAuth profile is configured, OpenClaw routes image
requests through that OAuth profile instead of first trying
`OPENAI_API_KEY`. Explicit `models.providers.openai` config (API key,
custom/Azure base URL) opts back into the direct OpenAI Images API
route.

3

[Navigate to header](https://docs.openclaw.ai/tools/image-generation#)

Ask the agent

_“Generate an image of a friendly robot mascot.”_The agent calls `image_generate` automatically. No tool allow-listing
needed — it is enabled by default when a provider is available.

For OpenAI-compatible LAN endpoints such as LocalAI, keep the custom
`models.providers.openai.baseUrl` and explicitly opt in with
`browser.ssrfPolicy.dangerouslyAllowPrivateNetwork: true`. Private and
internal image endpoints remain blocked by default.

## [​](https://docs.openclaw.ai/tools/image-generation\#common-routes)  Common routes

| Goal | Model ref | Auth |
| --- | --- | --- |
| OpenAI image generation with API billing | `openai/gpt-image-2` | `OPENAI_API_KEY` |
| OpenAI image generation with Codex subscription auth | `openai/gpt-image-2` | OpenAI Codex OAuth |
| OpenAI transparent-background PNG/WebP | `openai/gpt-image-1.5` | `OPENAI_API_KEY` or OpenAI Codex OAuth |
| DeepInfra image generation | `deepinfra/black-forest-labs/FLUX-1-schnell` | `DEEPINFRA_API_KEY` |
| OpenRouter image generation | `openrouter/google/gemini-3.1-flash-image-preview` | `OPENROUTER_API_KEY` |
| LiteLLM image generation | `litellm/gpt-image-2` | `LITELLM_API_KEY` |
| Google Gemini image generation | `google/gemini-3.1-flash-image-preview` | `GEMINI_API_KEY` or `GOOGLE_API_KEY` |

The same `image_generate` tool handles text-to-image and reference-image
editing. Use `image` for one reference or `images` for multiple references.
Provider-supported output hints such as `quality`, `outputFormat`, and
`background` are forwarded when available and reported as ignored when a
provider does not support them. Bundled transparent-background support is
OpenAI-specific; other providers may still preserve PNG alpha if their
backend emits it.

## [​](https://docs.openclaw.ai/tools/image-generation\#supported-providers)  Supported providers

| Provider | Default model | Edit support | Auth |
| --- | --- | --- | --- |
| ComfyUI | `workflow` | Yes (1 image, workflow-configured) | `COMFY_API_KEY` or `COMFY_CLOUD_API_KEY` for cloud |
| DeepInfra | `black-forest-labs/FLUX-1-schnell` | Yes (1 image) | `DEEPINFRA_API_KEY` |
| fal | `fal-ai/flux/dev` | Yes | `FAL_KEY` |
| Google | `gemini-3.1-flash-image-preview` | Yes | `GEMINI_API_KEY` or `GOOGLE_API_KEY` |
| LiteLLM | `gpt-image-2` | Yes (up to 5 input images) | `LITELLM_API_KEY` |
| MiniMax | `image-01` | Yes (subject reference) | `MINIMAX_API_KEY` or MiniMax OAuth (`minimax-portal`) |
| OpenAI | `gpt-image-2` | Yes (up to 4 images) | `OPENAI_API_KEY` or OpenAI Codex OAuth |
| OpenRouter | `google/gemini-3.1-flash-image-preview` | Yes (up to 5 input images) | `OPENROUTER_API_KEY` |
| Vydra | `grok-imagine` | No | `VYDRA_API_KEY` |
| xAI | `grok-imagine-image` | Yes (up to 5 images) | `XAI_API_KEY` |

Use `action: "list"` to inspect available providers and models at runtime:

```
/tool image_generate action=list
```

## [​](https://docs.openclaw.ai/tools/image-generation\#provider-capabilities)  Provider capabilities

| Capability | ComfyUI | DeepInfra | fal | Google | MiniMax | OpenAI | Vydra | xAI |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Generate (max count) | Workflow-defined | 4 | 4 | 4 | 9 | 4 | 1 | 4 |
| Edit / reference | 1 image (workflow) | 1 image | 1 image | Up to 5 images | 1 image (subject ref) | Up to 5 images | — | Up to 5 images |
| Size control | — | ✓ | ✓ | ✓ | — | Up to 4K | — | — |
| Aspect ratio | — | — | ✓ (generate only) | ✓ | ✓ | — | — | ✓ |
| Resolution (1K/2K/4K) | — | — | ✓ | ✓ | — | — | — | 1K, 2K |

## [​](https://docs.openclaw.ai/tools/image-generation\#tool-parameters)  Tool parameters

[​](https://docs.openclaw.ai/tools/image-generation#param-prompt)

prompt

string

required

Image generation prompt. Required for `action: "generate"`.

[​](https://docs.openclaw.ai/tools/image-generation#param-action)

action

"generate" \| "list"

default:"generate"

Use `"list"` to inspect available providers and models at runtime.

[​](https://docs.openclaw.ai/tools/image-generation#param-model)

model

string

Provider/model override (e.g. `openai/gpt-image-2`). Use
`openai/gpt-image-1.5` for transparent OpenAI backgrounds.

[​](https://docs.openclaw.ai/tools/image-generation#param-image)

image

string

Single reference image path or URL for edit mode.

[​](https://docs.openclaw.ai/tools/image-generation#param-images)

images

string\[\]

Multiple reference images for edit mode (up to 5 on supporting providers).

[​](https://docs.openclaw.ai/tools/image-generation#param-size)

size

string

Size hint: `1024x1024`, `1536x1024`, `1024x1536`, `2048x2048`, `3840x2160`.

[​](https://docs.openclaw.ai/tools/image-generation#param-aspect-ratio)

aspectRatio

string

Aspect ratio: `1:1`, `2:3`, `3:2`, `3:4`, `4:3`, `4:5`, `5:4`, `9:16`, `16:9`, `21:9`.

[​](https://docs.openclaw.ai/tools/image-generation#param-resolution)

resolution

"1K" \| "2K" \| "4K"

Resolution hint.

[​](https://docs.openclaw.ai/tools/image-generation#param-quality)

quality

"low" \| "medium" \| "high" \| "auto"

Quality hint when the provider supports it.

[​](https://docs.openclaw.ai/tools/image-generation#param-output-format)

outputFormat

"png" \| "jpeg" \| "webp"

Output format hint when the provider supports it.

[​](https://docs.openclaw.ai/tools/image-generation#param-background)

background

"transparent" \| "opaque" \| "auto"

Background hint when the provider supports it. Use `transparent` with
`outputFormat: "png"` or `"webp"` for transparency-capable providers.

[​](https://docs.openclaw.ai/tools/image-generation#param-count)

count

number

Number of images to generate (1–4).

[​](https://docs.openclaw.ai/tools/image-generation#param-timeout-ms)

timeoutMs

number

Optional provider request timeout in milliseconds.

[​](https://docs.openclaw.ai/tools/image-generation#param-filename)

filename

string

Output filename hint.

[​](https://docs.openclaw.ai/tools/image-generation#param-openai)

openai

object

OpenAI-only hints: `background`, `moderation`, `outputCompression`, and `user`.

Not all providers support all parameters. When a fallback provider supports a
nearby geometry option instead of the exact requested one, OpenClaw remaps to
the closest supported size, aspect ratio, or resolution before submission.
Unsupported output hints are dropped for providers that do not declare
support and reported in the tool result. Tool results report the applied
settings; `details.normalization` captures any requested-to-applied
translation.

## [​](https://docs.openclaw.ai/tools/image-generation\#configuration)  Configuration

### [​](https://docs.openclaw.ai/tools/image-generation\#model-selection)  Model selection

```
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "openai/gpt-image-2",
        timeoutMs: 180_000,
        fallbacks: [\
          "openrouter/google/gemini-3.1-flash-image-preview",\
          "google/gemini-3.1-flash-image-preview",\
          "fal/fal-ai/flux/dev",\
        ],
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/tools/image-generation\#provider-selection-order)  Provider selection order

OpenClaw tries providers in this order:

1. **`model` parameter** from the tool call (if the agent specifies one).
2. **`imageGenerationModel.primary`** from config.
3. **`imageGenerationModel.fallbacks`** in order.
4. **Auto-detection**— auth-backed provider defaults only:

   - current default provider first;
   - remaining registered image-generation providers in provider-id order.

If a provider fails (auth error, rate limit, etc.), the next configured
candidate is tried automatically. If all fail, the error includes details
from each attempt.

Per-call model overrides are exact

A per-call `model` override tries only that provider/model and does
not continue to configured primary/fallback or auto-detected providers.

Auto-detection is auth-aware

A provider default only enters the candidate list when OpenClaw can
actually authenticate that provider. Set
`agents.defaults.mediaGenerationAutoProviderFallback: false` to use only
explicit `model`, `primary`, and `fallbacks` entries.

Timeouts

Set `agents.defaults.imageGenerationModel.timeoutMs` for slow image
backends. A per-call `timeoutMs` tool parameter overrides the configured
default.

Inspect at runtime

Use `action: "list"` to inspect the currently registered providers,
their default models, and auth env-var hints.

### [​](https://docs.openclaw.ai/tools/image-generation\#image-editing)  Image editing

OpenAI, OpenRouter, Google, DeepInfra, fal, MiniMax, ComfyUI, and xAI support editing
reference images. Pass a reference image path or URL:

```
"Generate a watercolor version of this photo" + image: "/path/to/photo.jpg"
```

OpenAI, OpenRouter, Google, and xAI support up to 5 reference images via the
`images` parameter. fal, MiniMax, and ComfyUI support 1.

## [​](https://docs.openclaw.ai/tools/image-generation\#provider-deep-dives)  Provider deep dives

OpenAI gpt-image-2 (and gpt-image-1.5)

OpenAI image generation defaults to `openai/gpt-image-2`. If an
`openai-codex` OAuth profile is configured, OpenClaw reuses the same
OAuth profile used by Codex subscription chat models and sends the
image request through the Codex Responses backend. Legacy Codex base
URLs such as `https://chatgpt.com/backend-api` are canonicalized to
`https://chatgpt.com/backend-api/codex` for image requests. OpenClaw
does **not** silently fall back to `OPENAI_API_KEY` for that request —
to force direct OpenAI Images API routing, configure
`models.providers.openai` explicitly with an API key, custom base URL,
or Azure endpoint.The `openai/gpt-image-1.5`, `openai/gpt-image-1`, and
`openai/gpt-image-1-mini` models can still be selected explicitly. Use
`gpt-image-1.5` for transparent-background PNG/WebP output; the current
`gpt-image-2` API rejects `background: "transparent"`.`gpt-image-2` supports both text-to-image generation and
reference-image editing through the same `image_generate` tool.
OpenClaw forwards `prompt`, `count`, `size`, `quality`, `outputFormat`,
and reference images to OpenAI. OpenAI does **not** receive
`aspectRatio` or `resolution` directly; when possible OpenClaw maps
those into a supported `size`, otherwise the tool reports them as
ignored overrides.OpenAI-specific options live under the `openai` object:

```
{
  "quality": "low",
  "outputFormat": "jpeg",
  "openai": {
    "background": "opaque",
    "moderation": "low",
    "outputCompression": 60,
    "user": "end-user-42"
  }
}
```

`openai.background` accepts `transparent`, `opaque`, or `auto`;
transparent outputs require `outputFormat``png` or `webp` and a
transparency-capable OpenAI image model. OpenClaw routes default
`gpt-image-2` transparent-background requests to `gpt-image-1.5`.
`openai.outputCompression` applies to JPEG/WebP outputs.The top-level `background` hint is provider-neutral and currently maps
to the same OpenAI `background` request field when the OpenAI provider
is selected. Providers that do not declare background support return
it in `ignoredOverrides` instead of receiving the unsupported parameter.To route OpenAI image generation through an Azure OpenAI deployment
instead of `api.openai.com`, see
[Azure OpenAI endpoints](https://docs.openclaw.ai/providers/openai#azure-openai-endpoints).

OpenRouter image models

OpenRouter image generation uses the same `OPENROUTER_API_KEY` and
routes through OpenRouter’s chat completions image API. Select
OpenRouter image models with the `openrouter/` prefix:

```
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "openrouter/google/gemini-3.1-flash-image-preview",
      },
    },
  },
}
```

OpenClaw forwards `prompt`, `count`, reference images, and
Gemini-compatible `aspectRatio` / `resolution` hints to OpenRouter.
Current built-in OpenRouter image model shortcuts include
`google/gemini-3.1-flash-image-preview`,
`google/gemini-3-pro-image-preview`, and `openai/gpt-5.4-image-2`. Use
`action: "list"` to see what your configured plugin exposes.

MiniMax dual-auth

MiniMax image generation is available through both bundled MiniMax
auth paths:

- `minimax/image-01` for API-key setups
- `minimax-portal/image-01` for OAuth setups

xAI grok-imagine-image

The bundled xAI provider uses `/v1/images/generations` for prompt-only
requests and `/v1/images/edits` when `image` or `images` is present.

- Models: `xai/grok-imagine-image`, `xai/grok-imagine-image-pro`
- Count: up to 4
- References: one `image` or up to five `images`
- Aspect ratios: `1:1`, `16:9`, `9:16`, `4:3`, `3:4`, `2:3`, `3:2`
- Resolutions: `1K`, `2K`
- Outputs: returned as OpenClaw-managed image attachments

OpenClaw intentionally does not expose xAI-native `quality`, `mask`,
`user`, or extra native-only aspect ratios until those controls exist
in the shared cross-provider `image_generate` contract.

## [​](https://docs.openclaw.ai/tools/image-generation\#examples)  Examples

- Generate (4K landscape)

- Generate (transparent PNG)

- Generate (two square)

- Edit (one reference)

- Edit (multiple references)


```
/tool image_generate action=generate model=openai/gpt-image-2 prompt="A clean editorial poster for OpenClaw image generation" size=3840x2160 count=1
```

```
/tool image_generate action=generate model=openai/gpt-image-1.5 prompt="A simple red circle sticker on a transparent background" outputFormat=png background=transparent
```

Equivalent CLI:

```
openclaw infer image generate \
  --model openai/gpt-image-1.5 \
  --output-format png \
  --background transparent \
  --prompt "A simple red circle sticker on a transparent background" \
  --json
```

```
/tool image_generate action=generate model=openai/gpt-image-2 prompt="Two visual directions for a calm productivity app icon" size=1024x1024 count=2
```

```
/tool image_generate action=generate model=openai/gpt-image-2 prompt="Keep the subject, replace the background with a bright studio setup" image=/path/to/reference.png size=1024x1536
```

```
/tool image_generate action=generate model=openai/gpt-image-2 prompt="Combine the character identity from the first image with the color palette from the second" images='["/path/to/character.png","/path/to/palette.jpg"]' size=1536x1024
```

The same `--output-format` and `--background` flags are available on
`openclaw infer image edit`; `--openai-background` remains as an
OpenAI-specific alias. Bundled providers other than OpenAI do not declare
explicit background control today, so `background: "transparent"` is reported
as ignored for them.

## [​](https://docs.openclaw.ai/tools/image-generation\#related)  Related

- [Tools overview](https://docs.openclaw.ai/tools) — all available agent tools
- [ComfyUI](https://docs.openclaw.ai/providers/comfy) — local ComfyUI and Comfy Cloud workflow setup
- [fal](https://docs.openclaw.ai/providers/fal) — fal image and video provider setup
- [Google (Gemini)](https://docs.openclaw.ai/providers/google) — Gemini image provider setup
- [MiniMax](https://docs.openclaw.ai/providers/minimax) — MiniMax image provider setup
- [OpenAI](https://docs.openclaw.ai/providers/openai) — OpenAI Images provider setup
- [Vydra](https://docs.openclaw.ai/providers/vydra) — Vydra image, video, and speech setup
- [xAI](https://docs.openclaw.ai/providers/xai) — Grok image, video, search, code execution, and TTS setup
- [Configuration reference](https://docs.openclaw.ai/gateway/config-agents#agent-defaults) — `imageGenerationModel` config
- [Models](https://docs.openclaw.ai/concepts/models) — model configuration and failover

[Exec tool](https://docs.openclaw.ai/tools/exec) [LLM task](https://docs.openclaw.ai/tools/llm-task)

Ctrl+I