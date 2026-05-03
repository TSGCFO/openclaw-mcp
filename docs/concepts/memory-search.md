---
source_url: https://docs.openclaw.ai/concepts/memory-search
title: "Memory search - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/memory-search#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Memory

Memory search

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/concepts/memory-search#quick-start)
- [Supported providers](https://docs.openclaw.ai/concepts/memory-search#supported-providers)
- [How search works](https://docs.openclaw.ai/concepts/memory-search#how-search-works)
- [Improving search quality](https://docs.openclaw.ai/concepts/memory-search#improving-search-quality)
- [Temporal decay](https://docs.openclaw.ai/concepts/memory-search#temporal-decay)
- [MMR (diversity)](https://docs.openclaw.ai/concepts/memory-search#mmr-diversity)
- [Enable both](https://docs.openclaw.ai/concepts/memory-search#enable-both)
- [Multimodal memory](https://docs.openclaw.ai/concepts/memory-search#multimodal-memory)
- [Session memory search](https://docs.openclaw.ai/concepts/memory-search#session-memory-search)
- [Troubleshooting](https://docs.openclaw.ai/concepts/memory-search#troubleshooting)
- [Further reading](https://docs.openclaw.ai/concepts/memory-search#further-reading)
- [Related](https://docs.openclaw.ai/concepts/memory-search#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

`memory_search` finds relevant notes from your memory files, even when the
wording differs from the original text. It works by indexing memory into small
chunks and searching them using embeddings, keywords, or both.

## [​](https://docs.openclaw.ai/concepts/memory-search\#quick-start)  Quick start

If you have a GitHub Copilot subscription, OpenAI, Gemini, Voyage, or Mistral
API key configured, memory search works automatically. To set a provider
explicitly:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai", // or "gemini", "local", "ollama", etc.
      },
    },
  },
}
```

For multi-endpoint setups, `provider` can also be a custom
`models.providers.<id>` entry, such as `ollama-5080`, when that provider sets
`api: "ollama"` or another embedding adapter owner.For local embeddings with no API key, set `provider: "local"`. Source checkouts
may still require native build approval: `pnpm approve-builds` then
`pnpm rebuild node-llama-cpp`.Some OpenAI-compatible embedding endpoints require asymmetric labels such as
`input_type: "query"` for searches and `input_type: "document"` or `"passage"`
for indexed chunks. Configure those with `memorySearch.queryInputType` and
`memorySearch.documentInputType`; see the [Memory configuration reference](https://docs.openclaw.ai/reference/memory-config#provider-specific-config).

## [​](https://docs.openclaw.ai/concepts/memory-search\#supported-providers)  Supported providers

| Provider | ID | Needs API key | Notes |
| --- | --- | --- | --- |
| Bedrock | `bedrock` | No | Auto-detected when the AWS credential chain resolves |
| Gemini | `gemini` | Yes | Supports image/audio indexing |
| GitHub Copilot | `github-copilot` | No | Auto-detected, uses Copilot subscription |
| Local | `local` | No | GGUF model, ~0.6 GB download |
| Mistral | `mistral` | Yes | Auto-detected |
| Ollama | `ollama` | No | Local, must set explicitly |
| OpenAI | `openai` | Yes | Auto-detected, fast |
| Voyage | `voyage` | Yes | Auto-detected |

## [​](https://docs.openclaw.ai/concepts/memory-search\#how-search-works)  How search works

OpenClaw runs two retrieval paths in parallel and merges the results:

Query

Embedding

Tokenize

Vector Search

BM25 Search

Weighted Merge

Top Results

- **Vector search** finds notes with similar meaning (“gateway host” matches
“the machine running OpenClaw”).
- **BM25 keyword search** finds exact matches (IDs, error strings, config
keys).

If only one path is available (no embeddings or no FTS), the other runs alone.When embeddings are unavailable, OpenClaw still uses lexical ranking over FTS results instead of falling back to raw exact-match ordering only. That degraded mode boosts chunks with stronger query-term coverage and relevant file paths, which keeps recall useful even without `sqlite-vec` or an embedding provider.

## [​](https://docs.openclaw.ai/concepts/memory-search\#improving-search-quality)  Improving search quality

Two optional features help when you have a large note history:

### [​](https://docs.openclaw.ai/concepts/memory-search\#temporal-decay)  Temporal decay

Old notes gradually lose ranking weight so recent information surfaces first.
With the default half-life of 30 days, a note from last month scores at 50% of
its original weight. Evergreen files like `MEMORY.md` are never decayed.

Enable temporal decay if your agent has months of daily notes and stale
information keeps outranking recent context.

### [​](https://docs.openclaw.ai/concepts/memory-search\#mmr-diversity)  MMR (diversity)

Reduces redundant results. If five notes all mention the same router config, MMR
ensures the top results cover different topics instead of repeating.

Enable MMR if `memory_search` keeps returning near-duplicate snippets from
different daily notes.

### [​](https://docs.openclaw.ai/concepts/memory-search\#enable-both)  Enable both

```
{
  agents: {
    defaults: {
      memorySearch: {
        query: {
          hybrid: {
            mmr: { enabled: true },
            temporalDecay: { enabled: true },
          },
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/concepts/memory-search\#multimodal-memory)  Multimodal memory

With Gemini Embedding 2, you can index images and audio files alongside
Markdown. Search queries remain text, but they match against visual and audio
content. See the [Memory configuration reference](https://docs.openclaw.ai/reference/memory-config) for
setup.

## [​](https://docs.openclaw.ai/concepts/memory-search\#session-memory-search)  Session memory search

You can optionally index session transcripts so `memory_search` can recall
earlier conversations. This is opt-in via
`memorySearch.experimental.sessionMemory`. See the
[configuration reference](https://docs.openclaw.ai/reference/memory-config) for details.

## [​](https://docs.openclaw.ai/concepts/memory-search\#troubleshooting)  Troubleshooting

**No results?** Run `openclaw memory status` to check the index. If empty, run
`openclaw memory index --force`.**Only keyword matches?** Your embedding provider may not be configured. Check
`openclaw memory status --deep`.**Local embeddings time out?**`ollama`, `lmstudio`, and `local` use a longer
inline batch timeout by default. If the host is simply slow, set
`agents.defaults.memorySearch.sync.embeddingBatchTimeoutSeconds` and rerun
`openclaw memory index --force`.**CJK text not found?** Rebuild the FTS index with
`openclaw memory index --force`.

## [​](https://docs.openclaw.ai/concepts/memory-search\#further-reading)  Further reading

- [Active Memory](https://docs.openclaw.ai/concepts/active-memory) — sub-agent memory for interactive chat sessions
- [Memory](https://docs.openclaw.ai/concepts/memory) — file layout, backends, tools
- [Memory configuration reference](https://docs.openclaw.ai/reference/memory-config) — all config knobs

## [​](https://docs.openclaw.ai/concepts/memory-search\#related)  Related

- [Memory overview](https://docs.openclaw.ai/concepts/memory)
- [Active memory](https://docs.openclaw.ai/concepts/active-memory)
- [Builtin memory engine](https://docs.openclaw.ai/concepts/memory-builtin)

[Honcho memory](https://docs.openclaw.ai/concepts/memory-honcho) [Active memory](https://docs.openclaw.ai/concepts/active-memory)

Ctrl+I