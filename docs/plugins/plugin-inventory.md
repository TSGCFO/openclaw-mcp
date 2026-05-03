---
source_url: https://docs.openclaw.ai/plugins/plugin-inventory
title: "Plugin inventory - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/plugins/plugin-inventory#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Plugins

Plugin inventory

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Plugin inventory](https://docs.openclaw.ai/plugins/plugin-inventory#plugin-inventory)
- [Definitions](https://docs.openclaw.ai/plugins/plugin-inventory#definitions)
- [Core npm package](https://docs.openclaw.ai/plugins/plugin-inventory#core-npm-package)
- [Official external packages](https://docs.openclaw.ai/plugins/plugin-inventory#official-external-packages)
- [Source checkout only](https://docs.openclaw.ai/plugins/plugin-inventory#source-checkout-only)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/plugins/plugin-inventory\#plugin-inventory)  Plugin inventory

This page is generated from `extensions/*/package.json`, `openclaw.plugin.json`,
and the root npm package `files` exclusions. Regenerate it with:

```
pnpm plugins:inventory:gen
```

## [​](https://docs.openclaw.ai/plugins/plugin-inventory\#definitions)  Definitions

- **Core npm package:** built into the `openclaw` npm package and available without a separate plugin install.
- **Official external package:** OpenClaw-maintained plugin omitted from the core npm package, kept in this official inventory, and installed on demand through ClawHub and/or npm.
- **Source checkout only:** repo-local plugin omitted from published npm artifacts and not advertised as an installable package.

Source checkouts are different from npm installs: after `pnpm install`, bundled
plugins load from `extensions/<id>` so local edits and package-local workspace
dependencies are available.

## [​](https://docs.openclaw.ai/plugins/plugin-inventory\#core-npm-package)  Core npm package

| Plugin | Description | Distribution | Surface |
| --- | --- | --- | --- |
| [alibaba](https://docs.openclaw.ai/plugins/reference/alibaba) | Adds video generation provider support. | `@openclaw/alibaba-provider`<br>included in OpenClaw | contracts: videoGenerationProviders |
| [amazon-bedrock](https://docs.openclaw.ai/plugins/reference/amazon-bedrock) | Adds Amazon Bedrock model provider support to OpenClaw. | `@openclaw/amazon-bedrock-provider`<br>included in OpenClaw | providers: amazon-bedrock; contracts: memoryEmbeddingProviders |
| [amazon-bedrock-mantle](https://docs.openclaw.ai/plugins/reference/amazon-bedrock-mantle) | Adds Amazon Bedrock Mantle model provider support to OpenClaw. | `@openclaw/amazon-bedrock-mantle-provider`<br>included in OpenClaw | providers: amazon-bedrock-mantle |
| [anthropic](https://docs.openclaw.ai/plugins/reference/anthropic) | Adds Anthropic model provider support to OpenClaw. | `@openclaw/anthropic-provider`<br>included in OpenClaw | providers: anthropic; contracts: mediaUnderstandingProviders |
| [anthropic-vertex](https://docs.openclaw.ai/plugins/reference/anthropic-vertex) | Adds Anthropic Vertex model provider support to OpenClaw. | `@openclaw/anthropic-vertex-provider`<br>included in OpenClaw | providers: anthropic-vertex |
| [arcee](https://docs.openclaw.ai/plugins/reference/arcee) | Adds Arcee model provider support to OpenClaw. | `@openclaw/arcee-provider`<br>included in OpenClaw | providers: arcee |
| [azure-speech](https://docs.openclaw.ai/plugins/reference/azure-speech) | Azure AI Speech text-to-speech (MP3, native Ogg/Opus voice notes, PCM telephony). | `@openclaw/azure-speech`<br>included in OpenClaw | contracts: speechProviders |
| [bonjour](https://docs.openclaw.ai/plugins/reference/bonjour) | Advertise the local OpenClaw gateway over Bonjour/mDNS. | `@openclaw/bonjour`<br>included in OpenClaw | plugin |
| [browser](https://docs.openclaw.ai/plugins/reference/browser) | Adds agent-callable tools. | `@openclaw/browser-plugin`<br>included in OpenClaw | contracts: tools; skills |
| [byteplus](https://docs.openclaw.ai/plugins/reference/byteplus) | Adds BytePlus, BytePlus Plan model provider support to OpenClaw. | `@openclaw/byteplus-provider`<br>included in OpenClaw | providers: byteplus, byteplus-plan; contracts: videoGenerationProviders |
| [cerebras](https://docs.openclaw.ai/plugins/reference/cerebras) | Adds Cerebras model provider support to OpenClaw. | `@openclaw/cerebras-provider`<br>included in OpenClaw | providers: cerebras |
| [chutes](https://docs.openclaw.ai/plugins/reference/chutes) | Adds Chutes model provider support to OpenClaw. | `@openclaw/chutes-provider`<br>included in OpenClaw | providers: chutes |
| [cloudflare-ai-gateway](https://docs.openclaw.ai/plugins/reference/cloudflare-ai-gateway) | Adds Cloudflare AI Gateway model provider support to OpenClaw. | `@openclaw/cloudflare-ai-gateway-provider`<br>included in OpenClaw | providers: cloudflare-ai-gateway |
| [comfy](https://docs.openclaw.ai/plugins/reference/comfy) | Adds ComfyUI model provider support to OpenClaw. | `@openclaw/comfy-provider`<br>included in OpenClaw | providers: comfy; contracts: imageGenerationProviders, musicGenerationProviders, videoGenerationProviders |
| [copilot-proxy](https://docs.openclaw.ai/plugins/reference/copilot-proxy) | Adds Copilot Proxy model provider support to OpenClaw. | `@openclaw/copilot-proxy`<br>included in OpenClaw | providers: copilot-proxy |
| [deepgram](https://docs.openclaw.ai/plugins/reference/deepgram) | Adds media understanding provider support. Adds realtime transcription provider support. | `@openclaw/deepgram-provider`<br>included in OpenClaw | contracts: mediaUnderstandingProviders, realtimeTranscriptionProviders |
| [deepinfra](https://docs.openclaw.ai/plugins/reference/deepinfra) | Adds DeepInfra model provider support to OpenClaw. | `@openclaw/deepinfra-provider`<br>included in OpenClaw | providers: deepinfra; contracts: imageGenerationProviders, mediaUnderstandingProviders, memoryEmbeddingProviders, speechProviders, videoGenerationProviders |
| [deepseek](https://docs.openclaw.ai/plugins/reference/deepseek) | Adds DeepSeek model provider support to OpenClaw. | `@openclaw/deepseek-provider`<br>included in OpenClaw | providers: deepseek |
| [document-extract](https://docs.openclaw.ai/plugins/reference/document-extract) | Extract text and fallback page images from local document attachments. | `@openclaw/document-extract-plugin`<br>included in OpenClaw | contracts: documentExtractors |
| [duckduckgo](https://docs.openclaw.ai/plugins/reference/duckduckgo) | Adds web search provider support. | `@openclaw/duckduckgo-plugin`<br>included in OpenClaw | contracts: webSearchProviders |
| [elevenlabs](https://docs.openclaw.ai/plugins/reference/elevenlabs) | Adds media understanding provider support. Adds realtime transcription provider support. Adds text-to-speech provider support. | `@openclaw/elevenlabs-speech`<br>included in OpenClaw | contracts: mediaUnderstandingProviders, realtimeTranscriptionProviders, speechProviders |
| [exa](https://docs.openclaw.ai/plugins/reference/exa) | Adds web search provider support. | `@openclaw/exa-plugin`<br>included in OpenClaw | contracts: webSearchProviders |
| [fal](https://docs.openclaw.ai/plugins/reference/fal) | Adds fal model provider support to OpenClaw. | `@openclaw/fal-provider`<br>included in OpenClaw | providers: fal; contracts: imageGenerationProviders, videoGenerationProviders |
| [file-transfer](https://docs.openclaw.ai/plugins/reference/file-transfer) | Fetch, list, and write files on paired nodes via dedicated node commands. Bypasses bash stdout truncation by using base64 over node.invoke for binaries up to 16 MB. | `@openclaw/file-transfer`<br>included in OpenClaw | contracts: tools |
| [firecrawl](https://docs.openclaw.ai/plugins/reference/firecrawl) | Adds agent-callable tools. Adds web fetch provider support. Adds web search provider support. | `@openclaw/firecrawl-plugin`<br>included in OpenClaw | contracts: tools, webFetchProviders, webSearchProviders |
| [fireworks](https://docs.openclaw.ai/plugins/reference/fireworks) | Adds Fireworks model provider support to OpenClaw. | `@openclaw/fireworks-provider`<br>included in OpenClaw | providers: fireworks |
| [github-copilot](https://docs.openclaw.ai/plugins/reference/github-copilot) | Adds GitHub Copilot model provider support to OpenClaw. | `@openclaw/github-copilot-provider`<br>included in OpenClaw | providers: github-copilot; contracts: memoryEmbeddingProviders |
| [google](https://docs.openclaw.ai/plugins/reference/google) | Adds Google, Google Gemini CLI, Google Vertex model provider support to OpenClaw. | `@openclaw/google-plugin`<br>included in OpenClaw | providers: google, google-gemini-cli, google-vertex; contracts: imageGenerationProviders, mediaUnderstandingProviders, memoryEmbeddingProviders, musicGenerationProviders, realtimeVoiceProviders, speechProviders, videoGenerationProviders, webSearchProviders |
| [gradium](https://docs.openclaw.ai/plugins/reference/gradium) | Adds text-to-speech provider support. | `@openclaw/gradium-speech`<br>included in OpenClaw | contracts: speechProviders |
| [groq](https://docs.openclaw.ai/plugins/reference/groq) | Adds Groq model provider support to OpenClaw. | `@openclaw/groq-provider`<br>included in OpenClaw | providers: groq; contracts: mediaUnderstandingProviders |
| [huggingface](https://docs.openclaw.ai/plugins/reference/huggingface) | Adds Hugging Face model provider support to OpenClaw. | `@openclaw/huggingface-provider`<br>included in OpenClaw | providers: huggingface |
| [imessage](https://docs.openclaw.ai/plugins/reference/imessage) | Adds the iMessage channel surface for sending and receiving OpenClaw messages. | `@openclaw/imessage`<br>included in OpenClaw | channels: imessage |
| [inworld](https://docs.openclaw.ai/plugins/reference/inworld) | Inworld streaming text-to-speech (MP3, OGG\_OPUS, PCM telephony). | `@openclaw/inworld-speech`<br>included in OpenClaw | contracts: speechProviders |
| [irc](https://docs.openclaw.ai/plugins/reference/irc) | Adds the IRC channel surface for sending and receiving OpenClaw messages. | `@openclaw/irc`<br>included in OpenClaw | channels: irc |
| [kilocode](https://docs.openclaw.ai/plugins/reference/kilocode) | Adds Kilocode model provider support to OpenClaw. | `@openclaw/kilocode-provider`<br>included in OpenClaw | providers: kilocode |
| [kimi](https://docs.openclaw.ai/plugins/reference/kimi) | Adds Kimi, Kimi Coding model provider support to OpenClaw. | `@openclaw/kimi-provider`<br>included in OpenClaw | providers: kimi, kimi-coding |
| [litellm](https://docs.openclaw.ai/plugins/reference/litellm) | Adds LiteLLM model provider support to OpenClaw. | `@openclaw/litellm-provider`<br>included in OpenClaw | providers: litellm; contracts: imageGenerationProviders |
| [llm-task](https://docs.openclaw.ai/plugins/reference/llm-task) | Generic JSON-only LLM tool for structured tasks callable from workflows. | `@openclaw/llm-task`<br>included in OpenClaw | contracts: tools |
| [lmstudio](https://docs.openclaw.ai/plugins/reference/lmstudio) | Adds LM Studio model provider support to OpenClaw. | `@openclaw/lmstudio-provider`<br>included in OpenClaw | providers: lmstudio; contracts: memoryEmbeddingProviders |
| [matrix](https://docs.openclaw.ai/plugins/reference/matrix) | Adds the Matrix channel surface for sending and receiving OpenClaw messages. | `@openclaw/matrix`<br>included in OpenClaw | channels: matrix |
| [mattermost](https://docs.openclaw.ai/plugins/reference/mattermost) | Adds the Mattermost channel surface for sending and receiving OpenClaw messages. | `@openclaw/mattermost`<br>included in OpenClaw | channels: mattermost |
| [memory-core](https://docs.openclaw.ai/plugins/reference/memory-core) | Adds memory embedding provider support. Adds agent-callable tools. | `@openclaw/memory-core`<br>included in OpenClaw | contracts: memoryEmbeddingProviders, tools |
| [memory-wiki](https://docs.openclaw.ai/plugins/reference/memory-wiki) | Persistent wiki compiler and Obsidian-friendly knowledge vault for OpenClaw. | `@openclaw/memory-wiki`<br>included in OpenClaw | contracts: tools; skills |
| [microsoft](https://docs.openclaw.ai/plugins/reference/microsoft) | Adds text-to-speech provider support. | `@openclaw/microsoft-speech`<br>included in OpenClaw | contracts: speechProviders |
| [microsoft-foundry](https://docs.openclaw.ai/plugins/reference/microsoft-foundry) | Adds Microsoft Foundry model provider support to OpenClaw. | `@openclaw/microsoft-foundry`<br>included in OpenClaw | providers: microsoft-foundry |
| [migrate-claude](https://docs.openclaw.ai/plugins/reference/migrate-claude) | Imports Claude Code and Claude Desktop instructions, MCP servers, skills, and safe configuration into OpenClaw. | `@openclaw/migrate-claude`<br>included in OpenClaw | contracts: migrationProviders |
| [migrate-hermes](https://docs.openclaw.ai/plugins/reference/migrate-hermes) | Imports Hermes configuration, memories, skills, and supported credentials into OpenClaw. | `@openclaw/migrate-hermes`<br>included in OpenClaw | contracts: migrationProviders |
| [minimax](https://docs.openclaw.ai/plugins/reference/minimax) | Adds MiniMax, MiniMax Portal model provider support to OpenClaw. | `@openclaw/minimax-provider`<br>included in OpenClaw | providers: minimax, minimax-portal; contracts: imageGenerationProviders, mediaUnderstandingProviders, musicGenerationProviders, speechProviders, videoGenerationProviders, webSearchProviders |
| [mistral](https://docs.openclaw.ai/plugins/reference/mistral) | Adds Mistral model provider support to OpenClaw. | `@openclaw/mistral-provider`<br>included in OpenClaw | providers: mistral; contracts: mediaUnderstandingProviders, memoryEmbeddingProviders, realtimeTranscriptionProviders |
| [moonshot](https://docs.openclaw.ai/plugins/reference/moonshot) | Adds Moonshot model provider support to OpenClaw. | `@openclaw/moonshot-provider`<br>included in OpenClaw | providers: moonshot; contracts: mediaUnderstandingProviders, webSearchProviders |
| [nvidia](https://docs.openclaw.ai/plugins/reference/nvidia) | Adds NVIDIA model provider support to OpenClaw. | `@openclaw/nvidia-provider`<br>included in OpenClaw | providers: nvidia |
| [ollama](https://docs.openclaw.ai/plugins/reference/ollama) | Adds Ollama model provider support to OpenClaw. | `@openclaw/ollama-provider`<br>included in OpenClaw | providers: ollama; contracts: memoryEmbeddingProviders, webSearchProviders |
| [open-prose](https://docs.openclaw.ai/plugins/reference/open-prose) | OpenProse VM skill pack with a /prose slash command. | `@openclaw/open-prose`<br>included in OpenClaw | skills |
| [openai](https://docs.openclaw.ai/plugins/reference/openai) | Adds OpenAI, OpenAI Codex model provider support to OpenClaw. | `@openclaw/openai-provider`<br>included in OpenClaw | providers: openai, openai-codex; contracts: imageGenerationProviders, mediaUnderstandingProviders, memoryEmbeddingProviders, realtimeTranscriptionProviders, realtimeVoiceProviders, speechProviders, videoGenerationProviders |
| [opencode](https://docs.openclaw.ai/plugins/reference/opencode) | Adds OpenCode model provider support to OpenClaw. | `@openclaw/opencode-provider`<br>included in OpenClaw | providers: opencode; contracts: mediaUnderstandingProviders |
| [opencode-go](https://docs.openclaw.ai/plugins/reference/opencode-go) | Adds OpenCode Go model provider support to OpenClaw. | `@openclaw/opencode-go-provider`<br>included in OpenClaw | providers: opencode-go; contracts: mediaUnderstandingProviders |
| [openrouter](https://docs.openclaw.ai/plugins/reference/openrouter) | Adds OpenRouter model provider support to OpenClaw. | `@openclaw/openrouter-provider`<br>included in OpenClaw | providers: openrouter; contracts: imageGenerationProviders, mediaUnderstandingProviders, speechProviders, videoGenerationProviders |
| [openshell](https://docs.openclaw.ai/plugins/reference/openshell) | Sandbox backend powered by OpenShell with mirrored local workspaces and SSH-based command execution. | `@openclaw/openshell-sandbox`<br>included in OpenClaw | plugin |
| [perplexity](https://docs.openclaw.ai/plugins/reference/perplexity) | Adds web search provider support. | `@openclaw/perplexity-plugin`<br>included in OpenClaw | contracts: webSearchProviders |
| [qianfan](https://docs.openclaw.ai/plugins/reference/qianfan) | Adds Qianfan model provider support to OpenClaw. | `@openclaw/qianfan-provider`<br>included in OpenClaw | providers: qianfan |
| [qwen](https://docs.openclaw.ai/plugins/reference/qwen) | Adds Qwen, Qwen Cloud, Model Studio, DashScope model provider support to OpenClaw. | `@openclaw/qwen-provider`<br>included in OpenClaw | providers: qwen, qwencloud, modelstudio, dashscope; contracts: mediaUnderstandingProviders, videoGenerationProviders |
| [runway](https://docs.openclaw.ai/plugins/reference/runway) | Adds video generation provider support. | `@openclaw/runway-provider`<br>included in OpenClaw | contracts: videoGenerationProviders |
| [searxng](https://docs.openclaw.ai/plugins/reference/searxng) | Adds web search provider support. | `@openclaw/searxng-plugin`<br>included in OpenClaw | contracts: webSearchProviders |
| [senseaudio](https://docs.openclaw.ai/plugins/reference/senseaudio) | Adds media understanding provider support. | `@openclaw/senseaudio-provider`<br>included in OpenClaw | contracts: mediaUnderstandingProviders |
| [sglang](https://docs.openclaw.ai/plugins/reference/sglang) | Adds SGLang model provider support to OpenClaw. | `@openclaw/sglang-provider`<br>included in OpenClaw | providers: sglang |
| [signal](https://docs.openclaw.ai/plugins/reference/signal) | Adds the Signal channel surface for sending and receiving OpenClaw messages. | `@openclaw/signal`<br>included in OpenClaw | channels: signal |
| [skill-workshop](https://docs.openclaw.ai/plugins/reference/skill-workshop) | Captures repeatable workflows as workspace skills, with pending review, safe writes, and skill prompt refresh. | `@openclaw/skill-workshop`<br>included in OpenClaw | contracts: tools |
| [slack](https://docs.openclaw.ai/plugins/reference/slack) | Adds the Slack channel surface for sending and receiving OpenClaw messages. | `@openclaw/slack`<br>included in OpenClaw | channels: slack |
| [stepfun](https://docs.openclaw.ai/plugins/reference/stepfun) | Adds StepFun, StepFun Plan model provider support to OpenClaw. | `@openclaw/stepfun-provider`<br>included in OpenClaw | providers: stepfun, stepfun-plan |
| [synthetic](https://docs.openclaw.ai/plugins/reference/synthetic) | Adds Synthetic model provider support to OpenClaw. | `@openclaw/synthetic-provider`<br>included in OpenClaw | providers: synthetic |
| [tavily](https://docs.openclaw.ai/plugins/reference/tavily) | Adds agent-callable tools. Adds web search provider support. | `@openclaw/tavily-plugin`<br>included in OpenClaw | contracts: tools, webSearchProviders; skills |
| [telegram](https://docs.openclaw.ai/plugins/reference/telegram) | Adds the Telegram channel surface for sending and receiving OpenClaw messages. | `@openclaw/telegram`<br>included in OpenClaw | channels: telegram |
| [tencent](https://docs.openclaw.ai/plugins/reference/tencent) | Adds Tencent TokenHub model provider support to OpenClaw. | `@openclaw/tencent-provider`<br>included in OpenClaw | providers: tencent-tokenhub |
| [together](https://docs.openclaw.ai/plugins/reference/together) | Adds Together model provider support to OpenClaw. | `@openclaw/together-provider`<br>included in OpenClaw | providers: together; contracts: videoGenerationProviders |
| [tokenjuice](https://docs.openclaw.ai/plugins/reference/tokenjuice) | Compacts exec and bash tool results with tokenjuice reducers. | `@openclaw/tokenjuice`<br>included in OpenClaw | contracts: agentToolResultMiddleware |
| [tts-local-cli](https://docs.openclaw.ai/plugins/reference/tts-local-cli) | Adds text-to-speech provider support. | `@openclaw/tts-local-cli`<br>included in OpenClaw | contracts: speechProviders |
| [venice](https://docs.openclaw.ai/plugins/reference/venice) | Adds Venice model provider support to OpenClaw. | `@openclaw/venice-provider`<br>included in OpenClaw | providers: venice |
| [vercel-ai-gateway](https://docs.openclaw.ai/plugins/reference/vercel-ai-gateway) | Adds Vercel AI Gateway model provider support to OpenClaw. | `@openclaw/vercel-ai-gateway-provider`<br>included in OpenClaw | providers: vercel-ai-gateway |
| [vllm](https://docs.openclaw.ai/plugins/reference/vllm) | Adds vLLM model provider support to OpenClaw. | `@openclaw/vllm-provider`<br>included in OpenClaw | providers: vllm |
| [volcengine](https://docs.openclaw.ai/plugins/reference/volcengine) | Adds Volcengine, Volcengine Plan model provider support to OpenClaw. | `@openclaw/volcengine-provider`<br>included in OpenClaw | providers: volcengine, volcengine-plan; contracts: speechProviders |
| [voyage](https://docs.openclaw.ai/plugins/reference/voyage) | Adds memory embedding provider support. | `@openclaw/voyage-provider`<br>included in OpenClaw | contracts: memoryEmbeddingProviders |
| [vydra](https://docs.openclaw.ai/plugins/reference/vydra) | Adds Vydra model provider support to OpenClaw. | `@openclaw/vydra-provider`<br>included in OpenClaw | providers: vydra; contracts: imageGenerationProviders, speechProviders, videoGenerationProviders |
| [web-readability](https://docs.openclaw.ai/plugins/reference/web-readability) | Extract readable article content from local HTML web fetch responses. | `@openclaw/web-readability-plugin`<br>included in OpenClaw | contracts: webContentExtractors |
| [webhooks](https://docs.openclaw.ai/plugins/reference/webhooks) | Authenticated inbound webhooks that bind external automation to OpenClaw TaskFlows. | `@openclaw/webhooks`<br>included in OpenClaw | plugin |
| [xai](https://docs.openclaw.ai/plugins/reference/xai) | Adds xAI model provider support to OpenClaw. | `@openclaw/xai-plugin`<br>included in OpenClaw | providers: xai; contracts: imageGenerationProviders, mediaUnderstandingProviders, realtimeTranscriptionProviders, speechProviders, tools, videoGenerationProviders, webSearchProviders |
| [xiaomi](https://docs.openclaw.ai/plugins/reference/xiaomi) | Adds Xiaomi model provider support to OpenClaw. | `@openclaw/xiaomi-provider`<br>included in OpenClaw | providers: xiaomi; contracts: speechProviders |
| [zai](https://docs.openclaw.ai/plugins/reference/zai) | Adds Z.AI model provider support to OpenClaw. | `@openclaw/zai-provider`<br>included in OpenClaw | providers: zai; contracts: mediaUnderstandingProviders |

## [​](https://docs.openclaw.ai/plugins/plugin-inventory\#official-external-packages)  Official external packages

| Plugin | Description | Distribution | Surface |
| --- | --- | --- | --- |
| [acpx](https://docs.openclaw.ai/plugins/reference/acpx) | Embedded ACP runtime backend with plugin-owned session and transport management. | `@openclaw/acpx`<br>npm; ClawHub | skills |
| [bluebubbles](https://docs.openclaw.ai/plugins/reference/bluebubbles) | Adds the BlueBubbles channel surface for sending and receiving OpenClaw messages. | `@openclaw/bluebubbles`<br>npm; ClawHub | channels: bluebubbles |
| [brave](https://docs.openclaw.ai/plugins/reference/brave) | Adds web search provider support. | `@openclaw/brave-plugin`<br>npm; ClawHub | contracts: webSearchProviders |
| [codex](https://docs.openclaw.ai/plugins/reference/codex) | Codex app-server harness and Codex-managed GPT model catalog. | `@openclaw/codex`<br>npm; ClawHub | providers: codex; contracts: mediaUnderstandingProviders, migrationProviders |
| [diagnostics-otel](https://docs.openclaw.ai/plugins/reference/diagnostics-otel) | OpenClaw diagnostics OpenTelemetry exporter. | `@openclaw/diagnostics-otel`<br>npm; ClawHub: `clawhub:@openclaw/diagnostics-otel` | plugin |
| [diagnostics-prometheus](https://docs.openclaw.ai/plugins/reference/diagnostics-prometheus) | OpenClaw diagnostics Prometheus exporter. | `@openclaw/diagnostics-prometheus`<br>npm; ClawHub: `clawhub:@openclaw/diagnostics-prometheus` | plugin |
| [diffs](https://docs.openclaw.ai/plugins/reference/diffs) | Read-only diff viewer and file renderer for agents. | `@openclaw/diffs`<br>npm; ClawHub | contracts: tools; skills |
| [discord](https://docs.openclaw.ai/plugins/reference/discord) | Adds the Discord channel surface for sending and receiving OpenClaw messages. | `@openclaw/discord`<br>npm; ClawHub | channels: discord |
| [feishu](https://docs.openclaw.ai/plugins/reference/feishu) | Adds the Feishu channel surface for sending and receiving OpenClaw messages. | `@openclaw/feishu`<br>npm; ClawHub | channels: feishu; contracts: tools; skills |
| [google-meet](https://docs.openclaw.ai/plugins/reference/google-meet) | Join Google Meet calls through Chrome or Twilio transports. | `@openclaw/google-meet`<br>npm; ClawHub | contracts: tools |
| [googlechat](https://docs.openclaw.ai/plugins/reference/googlechat) | Adds the Google Chat channel surface for sending and receiving OpenClaw messages. | `@openclaw/googlechat`<br>npm; ClawHub | channels: googlechat |
| [line](https://docs.openclaw.ai/plugins/reference/line) | Adds the LINE channel surface for sending and receiving OpenClaw messages. | `@openclaw/line`<br>npm; ClawHub | channels: line |
| [lobster](https://docs.openclaw.ai/plugins/reference/lobster) | Typed workflow tool with resumable approvals. | `@openclaw/lobster`<br>npm; ClawHub | contracts: tools |
| [memory-lancedb](https://docs.openclaw.ai/plugins/reference/memory-lancedb) | Adds agent-callable tools. | `@openclaw/memory-lancedb`<br>npm; ClawHub | contracts: tools |
| [msteams](https://docs.openclaw.ai/plugins/reference/msteams) | Adds the Microsoft Teams channel surface for sending and receiving OpenClaw messages. | `@openclaw/msteams`<br>npm; ClawHub | channels: msteams |
| [nextcloud-talk](https://docs.openclaw.ai/plugins/reference/nextcloud-talk) | Adds the Nextcloud Talk channel surface for sending and receiving OpenClaw messages. | `@openclaw/nextcloud-talk`<br>npm; ClawHub | channels: nextcloud-talk |
| [nostr](https://docs.openclaw.ai/plugins/reference/nostr) | Adds the Nostr channel surface for sending and receiving OpenClaw messages. | `@openclaw/nostr`<br>npm; ClawHub | channels: nostr |
| [qqbot](https://docs.openclaw.ai/plugins/reference/qqbot) | Adds the QQ Bot channel surface for sending and receiving OpenClaw messages. | `@openclaw/qqbot`<br>npm; ClawHub | channels: qqbot; contracts: tools; skills |
| [synology-chat](https://docs.openclaw.ai/plugins/reference/synology-chat) | Adds the Synology Chat channel surface for sending and receiving OpenClaw messages. | `@openclaw/synology-chat`<br>npm; ClawHub | channels: synology-chat |
| [tlon](https://docs.openclaw.ai/plugins/reference/tlon) | Adds the Tlon channel surface for sending and receiving OpenClaw messages. | `@openclaw/tlon`<br>npm; ClawHub | channels: tlon; contracts: tools; skills |
| [twitch](https://docs.openclaw.ai/plugins/reference/twitch) | Adds the Twitch channel surface for sending and receiving OpenClaw messages. | `@openclaw/twitch`<br>npm; ClawHub | channels: twitch |
| [voice-call](https://docs.openclaw.ai/plugins/reference/voice-call) | Adds agent-callable tools. | `@openclaw/voice-call`<br>npm; ClawHub | contracts: tools |
| [whatsapp](https://docs.openclaw.ai/plugins/reference/whatsapp) | Adds the WhatsApp channel surface for sending and receiving OpenClaw messages. | `@openclaw/whatsapp`<br>npm; ClawHub | channels: whatsapp |
| [zalo](https://docs.openclaw.ai/plugins/reference/zalo) | Adds the Zalo channel surface for sending and receiving OpenClaw messages. | `@openclaw/zalo`<br>npm; ClawHub | channels: zalo |
| [zalouser](https://docs.openclaw.ai/plugins/reference/zalouser) | Adds the Zalo Personal channel surface for sending and receiving OpenClaw messages. | `@openclaw/zalouser`<br>npm; ClawHub | channels: zalouser; contracts: tools |

## [​](https://docs.openclaw.ai/plugins/plugin-inventory\#source-checkout-only)  Source checkout only

| Plugin | Description | Distribution | Surface |
| --- | --- | --- | --- |
| [qa-channel](https://docs.openclaw.ai/plugins/reference/qa-channel) | Adds the QA Channel surface for sending and receiving OpenClaw messages. | `@openclaw/qa-channel`<br>source checkout only | channels: qa-channel |
| [qa-lab](https://docs.openclaw.ai/plugins/reference/qa-lab) | OpenClaw QA lab plugin with private debugger UI and scenario runner. | `@openclaw/qa-lab`<br>source checkout only | plugin |
| [qa-matrix](https://docs.openclaw.ai/plugins/reference/qa-matrix) | Matrix QA transport runner and substrate. | `@openclaw/qa-matrix`<br>source checkout only | plugin |

[Community plugins](https://docs.openclaw.ai/plugins/community) [Plugin reference](https://docs.openclaw.ai/plugins/reference)

Ctrl+I