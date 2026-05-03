---
source_url: https://docs.openclaw.ai/concepts/memory-builtin
title: "Builtin memory engine - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/memory-builtin#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Memory

Builtin memory engine

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What it provides](https://docs.openclaw.ai/concepts/memory-builtin#what-it-provides)
- [Getting started](https://docs.openclaw.ai/concepts/memory-builtin#getting-started)
- [Supported embedding providers](https://docs.openclaw.ai/concepts/memory-builtin#supported-embedding-providers)
- [How indexing works](https://docs.openclaw.ai/concepts/memory-builtin#how-indexing-works)
- [When to use](https://docs.openclaw.ai/concepts/memory-builtin#when-to-use)
- [Troubleshooting](https://docs.openclaw.ai/concepts/memory-builtin#troubleshooting)
- [Configuration](https://docs.openclaw.ai/concepts/memory-builtin#configuration)
- [Related](https://docs.openclaw.ai/concepts/memory-builtin#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The builtin engine is the default memory backend. It stores your memory index in
a per-agent SQLite database and needs no extra dependencies to get started.

## [​](https://docs.openclaw.ai/concepts/memory-builtin\#what-it-provides)  What it provides

- **Keyword search** via FTS5 full-text indexing (BM25 scoring).
- **Vector search** via embeddings from any supported provider.
- **Hybrid search** that combines both for best results.
- **CJK support** via trigram tokenization for Chinese, Japanese, and Korean.
- **sqlite-vec acceleration** for in-database vector queries (optional).

## [​](https://docs.openclaw.ai/concepts/memory-builtin\#getting-started)  Getting started

If you have an API key for OpenAI, Gemini, Voyage, Mistral, or DeepInfra, the builtin
engine auto-detects it and enables vector search. No config needed.To set a provider explicitly:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
      },
    },
  },
}
```

Without an embedding provider, only keyword search is available.To force the built-in local embedding provider, install the optional
`node-llama-cpp` runtime package next to OpenClaw, then point `local.modelPath`
at a GGUF file:

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "local",
        fallback: "none",
        local: {
          modelPath: "~/.node-llama-cpp/models/embeddinggemma-300m-qat-Q8_0.gguf",
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/concepts/memory-builtin\#supported-embedding-providers)  Supported embedding providers

| Provider | ID | Auto-detected | Notes |
| --- | --- | --- | --- |
| OpenAI | `openai` | Yes | Default: `text-embedding-3-small` |
| Gemini | `gemini` | Yes | Supports multimodal (image + audio) |
| Voyage | `voyage` | Yes |  |
| Mistral | `mistral` | Yes |  |
| DeepInfra | `deepinfra` | Yes | Default: `BAAI/bge-m3` |
| Ollama | `ollama` | No | Local, set explicitly |
| Local | `local` | Yes (first) | Optional `node-llama-cpp` runtime |

Auto-detection picks the first provider whose API key can be resolved, in the
order shown. Set `memorySearch.provider` to override.

## [​](https://docs.openclaw.ai/concepts/memory-builtin\#how-indexing-works)  How indexing works

OpenClaw indexes `MEMORY.md` and `memory/*.md` into chunks (~400 tokens with
80-token overlap) and stores them in a per-agent SQLite database.

- **Index location:**`~/.openclaw/memory/<agentId>.sqlite`
- **Storage maintenance:** SQLite WAL sidecars are bounded with periodic and
shutdown checkpoints.
- **File watching:** changes to memory files trigger a debounced reindex (1.5s).
- **Auto-reindex:** when the embedding provider, model, or chunking config
changes, the entire index is rebuilt automatically.
- **Reindex on demand:**`openclaw memory index --force`

You can also index Markdown files outside the workspace with
`memorySearch.extraPaths`. See the
[configuration reference](https://docs.openclaw.ai/reference/memory-config#additional-memory-paths).

## [​](https://docs.openclaw.ai/concepts/memory-builtin\#when-to-use)  When to use

The builtin engine is the right choice for most users:

- Works out of the box with no extra dependencies.
- Handles keyword and vector search well.
- Supports all embedding providers.
- Hybrid search combines the best of both retrieval approaches.

Consider switching to [QMD](https://docs.openclaw.ai/concepts/memory-qmd) if you need reranking, query
expansion, or want to index directories outside the workspace.Consider [Honcho](https://docs.openclaw.ai/concepts/memory-honcho) if you want cross-session memory with
automatic user modeling.

## [​](https://docs.openclaw.ai/concepts/memory-builtin\#troubleshooting)  Troubleshooting

**Memory search disabled?** Check `openclaw memory status`. If no provider is
detected, set one explicitly or add an API key.**Local provider not detected?** Confirm the local path exists and run:

```
openclaw memory status --deep --agent main
openclaw memory index --force --agent main
```

Both standalone CLI commands and the Gateway use the same `local` provider id.
If the provider is set to `auto`, local embeddings are considered first only
when `memorySearch.local.modelPath` points to an existing local file.**Stale results?** Run `openclaw memory index --force` to rebuild. The watcher
may miss changes in rare edge cases.**sqlite-vec not loading?** OpenClaw falls back to in-process cosine similarity
automatically. Check logs for the specific load error.

## [​](https://docs.openclaw.ai/concepts/memory-builtin\#configuration)  Configuration

For embedding provider setup, hybrid search tuning (weights, MMR, temporal
decay), batch indexing, multimodal memory, sqlite-vec, extra paths, and all
other config knobs, see the
[Memory configuration reference](https://docs.openclaw.ai/reference/memory-config).

## [​](https://docs.openclaw.ai/concepts/memory-builtin\#related)  Related

- [Memory overview](https://docs.openclaw.ai/concepts/memory)
- [Memory search](https://docs.openclaw.ai/concepts/memory-search)
- [Active memory](https://docs.openclaw.ai/concepts/active-memory)

[Memory overview](https://docs.openclaw.ai/concepts/memory) [QMD memory engine](https://docs.openclaw.ai/concepts/memory-qmd)

Ctrl+I