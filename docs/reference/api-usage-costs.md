---
source_url: https://docs.openclaw.ai/reference/api-usage-costs
title: "API usage and costs - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/reference/api-usage-costs#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Technical reference

API usage and costs

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [API usage & costs](https://docs.openclaw.ai/reference/api-usage-costs#api-usage-%26-costs)
- [Where costs show up (chat + CLI)](https://docs.openclaw.ai/reference/api-usage-costs#where-costs-show-up-chat-%2B-cli)
- [How keys are discovered](https://docs.openclaw.ai/reference/api-usage-costs#how-keys-are-discovered)
- [Features that can spend keys](https://docs.openclaw.ai/reference/api-usage-costs#features-that-can-spend-keys)
- [1) Core model responses (chat + tools)](https://docs.openclaw.ai/reference/api-usage-costs#1-core-model-responses-chat-%2B-tools)
- [2) Media understanding (audio/image/video)](https://docs.openclaw.ai/reference/api-usage-costs#2-media-understanding-audio%2Fimage%2Fvideo)
- [3) Image and video generation](https://docs.openclaw.ai/reference/api-usage-costs#3-image-and-video-generation)
- [4) Memory embeddings + semantic search](https://docs.openclaw.ai/reference/api-usage-costs#4-memory-embeddings-%2B-semantic-search)
- [5) Web search tool](https://docs.openclaw.ai/reference/api-usage-costs#5-web-search-tool)
- [5) Web fetch tool (Firecrawl)](https://docs.openclaw.ai/reference/api-usage-costs#5-web-fetch-tool-firecrawl)
- [6) Provider usage snapshots (status/health)](https://docs.openclaw.ai/reference/api-usage-costs#6-provider-usage-snapshots-status%2Fhealth)
- [7) Compaction safeguard summarization](https://docs.openclaw.ai/reference/api-usage-costs#7-compaction-safeguard-summarization)
- [8) Model scan / probe](https://docs.openclaw.ai/reference/api-usage-costs#8-model-scan-%2F-probe)
- [9) Talk (speech)](https://docs.openclaw.ai/reference/api-usage-costs#9-talk-speech)
- [10) Skills (third-party APIs)](https://docs.openclaw.ai/reference/api-usage-costs#10-skills-third-party-apis)
- [Related](https://docs.openclaw.ai/reference/api-usage-costs#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/reference/api-usage-costs\#api-usage-&-costs)  API usage & costs

This doc lists **features that can invoke API keys** and where their costs show up. It focuses on
OpenClaw features that can generate provider usage or paid API calls.

## [​](https://docs.openclaw.ai/reference/api-usage-costs\#where-costs-show-up-chat-+-cli)  Where costs show up (chat + CLI)

**Per-session cost snapshot**

- `/status` shows the current session model, context usage, and last response tokens.
- If the model uses **API-key auth**, `/status` also shows **estimated cost** for the last reply.
- If live session metadata is sparse, `/status` can recover token/cache
counters and the active runtime model label from the latest transcript usage
entry. Existing nonzero live values still take precedence, and prompt-sized
transcript totals can win when stored totals are missing or smaller.

**Per-message cost footer**

- `/usage full` appends a usage footer to every reply, including **estimated cost** (API-key only).
- `/usage tokens` shows tokens only; subscription-style OAuth/token and CLI flows hide dollar cost.
- Gemini CLI note: when the CLI returns JSON output, OpenClaw reads usage from
`stats`, normalizes `stats.cached` into `cacheRead`, and derives input tokens
from `stats.input_tokens - stats.cached` when needed.

Anthropic note: Anthropic staff told us OpenClaw-style Claude CLI usage is
allowed again, so OpenClaw treats Claude CLI reuse and `claude -p` usage as
sanctioned for this integration unless Anthropic publishes a new policy.
Anthropic still does not expose a per-message dollar estimate that OpenClaw can
show in `/usage full`.**CLI usage windows (provider quotas)**

- `openclaw status --usage` and `openclaw channels list` show provider **usage windows**
(quota snapshots, not per-message costs).
- Human output is normalized to `X% left` across providers.
- Current usage-window providers: Anthropic, GitHub Copilot, Gemini CLI,
OpenAI Codex, MiniMax, Xiaomi, and z.ai.
- MiniMax note: its raw `usage_percent` / `usagePercent` fields mean remaining
quota, so OpenClaw inverts them before display. Count-based fields still win
when present. If the provider returns `model_remains`, OpenClaw prefers the
chat-model entry, derives the window label from timestamps when needed, and
includes the model name in the plan label.
- Usage auth for those quota windows comes from provider-specific hooks when
available; otherwise OpenClaw falls back to matching OAuth/API-key
credentials from auth profiles, env, or config.

See [Token use & costs](https://docs.openclaw.ai/reference/token-use) for details and examples.

## [​](https://docs.openclaw.ai/reference/api-usage-costs\#how-keys-are-discovered)  How keys are discovered

OpenClaw can pick up credentials from:

- **Auth profiles** (per-agent, stored in `auth-profiles.json`).
- **Environment variables** (e.g. `OPENAI_API_KEY`, `BRAVE_API_KEY`, `FIRECRAWL_API_KEY`).
- **Config** (`models.providers.*.apiKey`, `plugins.entries.*.config.webSearch.apiKey`,
`plugins.entries.firecrawl.config.webFetch.apiKey`, `memorySearch.*`,
`talk.providers.*.apiKey`).
- **Skills** (`skills.entries.<name>.apiKey`) which may export keys to the skill process env.

## [​](https://docs.openclaw.ai/reference/api-usage-costs\#features-that-can-spend-keys)  Features that can spend keys

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#1-core-model-responses-chat-+-tools)  1) Core model responses (chat + tools)

Every reply or tool call uses the **current model provider** (OpenAI, Anthropic, etc). This is the
primary source of usage and cost.This also includes subscription-style hosted providers that still bill outside
OpenClaw’s local UI, such as **OpenAI Codex**, **Alibaba Cloud Model Studio**
**Coding Plan**, **MiniMax Coding Plan**, **Z.AI / GLM Coding Plan**, and
Anthropic’s OpenClaw Claude-login path with **Extra Usage** enabled.See [Models](https://docs.openclaw.ai/providers/models) for pricing config and [Token use & costs](https://docs.openclaw.ai/reference/token-use) for display.

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#2-media-understanding-audio/image/video)  2) Media understanding (audio/image/video)

Inbound media can be summarized/transcribed before the reply runs. This uses model/provider APIs.

- Audio: OpenAI / Groq / Deepgram / DeepInfra / Google / Mistral.
- Image: OpenAI / OpenRouter / Anthropic / DeepInfra / Google / MiniMax / Moonshot / Qwen / Z.AI.
- Video: Google / Qwen / Moonshot.

See [Media understanding](https://docs.openclaw.ai/nodes/media-understanding).

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#3-image-and-video-generation)  3) Image and video generation

Shared generation capabilities can also spend provider keys:

- Image generation: OpenAI / Google / DeepInfra / fal / MiniMax
- Video generation: DeepInfra / Qwen

Image generation can infer an auth-backed provider default when
`agents.defaults.imageGenerationModel` is unset. Video generation currently
requires an explicit `agents.defaults.videoGenerationModel` such as
`qwen/wan2.6-t2v`.See [Image generation](https://docs.openclaw.ai/tools/image-generation), [Qwen Cloud](https://docs.openclaw.ai/providers/qwen),
and [Models](https://docs.openclaw.ai/concepts/models).

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#4-memory-embeddings-+-semantic-search)  4) Memory embeddings + semantic search

Semantic memory search uses **embedding APIs** when configured for remote providers:

- `memorySearch.provider = "openai"` → OpenAI embeddings
- `memorySearch.provider = "gemini"` → Gemini embeddings
- `memorySearch.provider = "voyage"` → Voyage embeddings
- `memorySearch.provider = "mistral"` → Mistral embeddings
- `memorySearch.provider = "deepinfra"` → DeepInfra embeddings
- `memorySearch.provider = "lmstudio"` → LM Studio embeddings (local/self-hosted)
- `memorySearch.provider = "ollama"` → Ollama embeddings (local/self-hosted; typically no hosted API billing)
- Optional fallback to a remote provider if local embeddings fail

You can keep it local with `memorySearch.provider = "local"` (no API usage).See [Memory](https://docs.openclaw.ai/concepts/memory).

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#5-web-search-tool)  5) Web search tool

`web_search` may incur usage charges depending on your provider:

- **Brave Search API**: `BRAVE_API_KEY` or `plugins.entries.brave.config.webSearch.apiKey`
- **Exa**: `EXA_API_KEY` or `plugins.entries.exa.config.webSearch.apiKey`
- **Firecrawl**: `FIRECRAWL_API_KEY` or `plugins.entries.firecrawl.config.webSearch.apiKey`
- **Gemini (Google Search)**: `GEMINI_API_KEY` or `plugins.entries.google.config.webSearch.apiKey`
- **Grok (xAI)**: `XAI_API_KEY` or `plugins.entries.xai.config.webSearch.apiKey`
- **Kimi (Moonshot)**: `KIMI_API_KEY`, `MOONSHOT_API_KEY`, or `plugins.entries.moonshot.config.webSearch.apiKey`
- **MiniMax Search**: `MINIMAX_CODE_PLAN_KEY`, `MINIMAX_CODING_API_KEY`, `MINIMAX_API_KEY`, or `plugins.entries.minimax.config.webSearch.apiKey`
- **Ollama Web Search**: key-free for a reachable signed-in local Ollama host; direct `https://ollama.com` search uses `OLLAMA_API_KEY`, and auth-protected hosts can reuse normal Ollama provider bearer auth
- **Perplexity Search API**: `PERPLEXITY_API_KEY`, `OPENROUTER_API_KEY`, or `plugins.entries.perplexity.config.webSearch.apiKey`
- **Tavily**: `TAVILY_API_KEY` or `plugins.entries.tavily.config.webSearch.apiKey`
- **DuckDuckGo**: key-free fallback (no API billing, but unofficial and HTML-based)
- **SearXNG**: `SEARXNG_BASE_URL` or `plugins.entries.searxng.config.webSearch.baseUrl` (key-free/self-hosted; no hosted API billing)

Legacy `tools.web.search.*` provider paths still load through the temporary compatibility shim, but they are no longer the recommended config surface.**Brave Search free credit:** Each Brave plan includes $5/month in renewing
free credit. The Search plan costs $5 per 1,000 requests, so the credit covers
1,000 requests/month at no charge. Set your usage limit in the Brave dashboard
to avoid unexpected charges.See [Web tools](https://docs.openclaw.ai/tools/web).

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#5-web-fetch-tool-firecrawl)  5) Web fetch tool (Firecrawl)

`web_fetch` can call **Firecrawl** when an API key is present:

- `FIRECRAWL_API_KEY` or `plugins.entries.firecrawl.config.webFetch.apiKey`

If Firecrawl isn’t configured, the tool falls back to direct fetch plus the bundled `web-readability` plugin (no paid API). Disable `plugins.entries.web-readability.enabled` to skip local Readability extraction.See [Web tools](https://docs.openclaw.ai/tools/web).

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#6-provider-usage-snapshots-status/health)  6) Provider usage snapshots (status/health)

Some status commands call **provider usage endpoints** to display quota windows or auth health.
These are typically low-volume calls but still hit provider APIs:

- `openclaw status --usage`
- `openclaw models status --json`

See [Models CLI](https://docs.openclaw.ai/cli/models).

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#7-compaction-safeguard-summarization)  7) Compaction safeguard summarization

The compaction safeguard can summarize session history using the **current model**, which
invokes provider APIs when it runs.See [Session management + compaction](https://docs.openclaw.ai/reference/session-management-compaction).

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#8-model-scan-/-probe)  8) Model scan / probe

`openclaw models scan` can probe OpenRouter models and uses `OPENROUTER_API_KEY` when
probing is enabled.See [Models CLI](https://docs.openclaw.ai/cli/models).

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#9-talk-speech)  9) Talk (speech)

Talk mode can invoke **ElevenLabs** when configured:

- `ELEVENLABS_API_KEY` or `talk.providers.elevenlabs.apiKey`

See [Talk mode](https://docs.openclaw.ai/nodes/talk).

### [​](https://docs.openclaw.ai/reference/api-usage-costs\#10-skills-third-party-apis)  10) Skills (third-party APIs)

Skills can store `apiKey` in `skills.entries.<name>.apiKey`. If a skill uses that key for external
APIs, it can incur costs according to the skill’s provider.See [Skills](https://docs.openclaw.ai/tools/skills).

## [​](https://docs.openclaw.ai/reference/api-usage-costs\#related)  Related

- [Token use and costs](https://docs.openclaw.ai/reference/token-use)
- [Prompt caching](https://docs.openclaw.ai/reference/prompt-caching)
- [Usage tracking](https://docs.openclaw.ai/concepts/usage-tracking)

[Prompt caching](https://docs.openclaw.ai/reference/prompt-caching) [Transcript hygiene](https://docs.openclaw.ai/reference/transcript-hygiene)

Ctrl+I