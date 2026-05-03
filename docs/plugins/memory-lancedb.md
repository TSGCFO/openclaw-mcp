---
source_url: https://docs.openclaw.ai/plugins/memory-lancedb
title: "Memory LanceDB - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/plugins/memory-lancedb#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Plugins

Memory LanceDB

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/plugins/memory-lancedb#quick-start)
- [Provider-backed embeddings](https://docs.openclaw.ai/plugins/memory-lancedb#provider-backed-embeddings)
- [Ollama embeddings](https://docs.openclaw.ai/plugins/memory-lancedb#ollama-embeddings)
- [OpenAI-compatible providers](https://docs.openclaw.ai/plugins/memory-lancedb#openai-compatible-providers)
- [Recall and capture limits](https://docs.openclaw.ai/plugins/memory-lancedb#recall-and-capture-limits)
- [Commands](https://docs.openclaw.ai/plugins/memory-lancedb#commands)
- [Storage](https://docs.openclaw.ai/plugins/memory-lancedb#storage)
- [Runtime dependencies](https://docs.openclaw.ai/plugins/memory-lancedb#runtime-dependencies)
- [Troubleshooting](https://docs.openclaw.ai/plugins/memory-lancedb#troubleshooting)
- [Input length exceeds the context length](https://docs.openclaw.ai/plugins/memory-lancedb#input-length-exceeds-the-context-length)
- [Unsupported embedding model](https://docs.openclaw.ai/plugins/memory-lancedb#unsupported-embedding-model)
- [Plugin loads but no memories appear](https://docs.openclaw.ai/plugins/memory-lancedb#plugin-loads-but-no-memories-appear)
- [Related](https://docs.openclaw.ai/plugins/memory-lancedb#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

`memory-lancedb` is a bundled memory plugin that stores long-term memory in
LanceDB and uses embeddings for recall. It can automatically recall relevant
memories before a model turn and capture important facts after a response.Use it when you want a local vector database for memory, need an
OpenAI-compatible embedding endpoint, or want to keep a memory database outside
the default built-in memory store.

`memory-lancedb` is an active memory plugin. Enable it by selecting the memory
slot with `plugins.slots.memory = "memory-lancedb"`. Companion plugins such as
`memory-wiki` can run beside it, but only one plugin owns the active memory slot.

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#quick-start)  Quick start

```
{
  plugins: {
    slots: {
      memory: "memory-lancedb",
    },
    entries: {
      "memory-lancedb": {
        enabled: true,
        config: {
          embedding: {
            provider: "openai",
            model: "text-embedding-3-small",
          },
          autoRecall: true,
          autoCapture: false,
        },
      },
    },
  },
}
```

Restart the Gateway after changing plugin config:

```
openclaw gateway restart
```

Then verify the plugin is loaded:

```
openclaw plugins list
```

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#provider-backed-embeddings)  Provider-backed embeddings

`memory-lancedb` can use the same memory embedding provider adapters as
`memory-core`. Set `embedding.provider` and omit `embedding.apiKey` to use the
provider’s configured auth profile, environment variable, or
`models.providers.<provider>.apiKey`.

```
{
  plugins: {
    slots: {
      memory: "memory-lancedb",
    },
    entries: {
      "memory-lancedb": {
        enabled: true,
        config: {
          embedding: {
            provider: "openai",
            model: "text-embedding-3-small",
          },
          autoRecall: true,
        },
      },
    },
  },
}
```

This path works with provider auth profiles that expose embedding credentials.
For example, GitHub Copilot can be used when the Copilot profile/plan supports
embeddings:

```
{
  plugins: {
    slots: {
      memory: "memory-lancedb",
    },
    entries: {
      "memory-lancedb": {
        enabled: true,
        config: {
          embedding: {
            provider: "github-copilot",
            model: "text-embedding-3-small",
          },
        },
      },
    },
  },
}
```

OpenAI Codex / ChatGPT OAuth (`openai-codex`) is not an OpenAI Platform
embeddings credential. For OpenAI embeddings, use an OpenAI API key auth profile,
`OPENAI_API_KEY`, or `models.providers.openai.apiKey`. OAuth-only users can use
another embedding-capable provider such as GitHub Copilot or Ollama.

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#ollama-embeddings)  Ollama embeddings

For Ollama embeddings, prefer the bundled Ollama embedding provider. It uses the
native Ollama `/api/embed` endpoint and follows the same auth/base URL rules as
the Ollama provider documented in [Ollama](https://docs.openclaw.ai/providers/ollama).

```
{
  plugins: {
    slots: {
      memory: "memory-lancedb",
    },
    entries: {
      "memory-lancedb": {
        enabled: true,
        config: {
          embedding: {
            provider: "ollama",
            baseUrl: "http://127.0.0.1:11434",
            model: "mxbai-embed-large",
            dimensions: 1024,
          },
          recallMaxChars: 400,
          autoRecall: true,
          autoCapture: false,
        },
      },
    },
  },
}
```

Set `dimensions` for non-standard embedding models. OpenClaw knows the
dimensions for `text-embedding-3-small` and `text-embedding-3-large`; custom
models need the value in config so LanceDB can create the vector column.For small local embedding models, lower `recallMaxChars` if you see context
length errors from the local server.

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#openai-compatible-providers)  OpenAI-compatible providers

Some OpenAI-compatible embedding providers reject the `encoding_format`
parameter, while others ignore it and always return `number[]` vectors.
`memory-lancedb` therefore omits `encoding_format` on embedding requests and
accepts either float-array responses or base64-encoded float32 responses.If you have a raw OpenAI-compatible embeddings endpoint that does not have a
bundled provider adapter, omit `embedding.provider` (or leave it as `openai`) and
set `embedding.apiKey` plus `embedding.baseUrl`. This preserves the direct
OpenAI-compatible client path.Set `embedding.dimensions` for providers whose model dimensions are not built
in. For example, ZhiPu `embedding-3` uses `2048` dimensions:

```
{
  plugins: {
    entries: {
      "memory-lancedb": {
        enabled: true,
        config: {
          embedding: {
            apiKey: "${ZHIPU_API_KEY}",
            baseUrl: "https://open.bigmodel.cn/api/paas/v4",
            model: "embedding-3",
            dimensions: 2048,
          },
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#recall-and-capture-limits)  Recall and capture limits

`memory-lancedb` has two separate text limits:

| Setting | Default | Range | Applies to |
| --- | --- | --- | --- |
| `recallMaxChars` | `1000` | 100-10000 | text sent to the embedding API for recall |
| `captureMaxChars` | `500` | 100-10000 | assistant message length eligible for capture |

`recallMaxChars` controls auto-recall, the `memory_recall` tool, the
`memory_forget` query path, and `openclaw ltm search`. Auto-recall prefers the
latest user message from the turn and falls back to the full prompt only when no
user message is available. This keeps channel metadata and large prompt blocks
out of the embedding request.`captureMaxChars` controls whether a response is short enough to be considered
for automatic capture. It does not cap recall query embeddings.

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#commands)  Commands

When `memory-lancedb` is the active memory plugin, it registers the `ltm` CLI
namespace:

```
openclaw ltm list
openclaw ltm search "project preferences"
openclaw ltm stats
```

The plugin also extends `openclaw memory` with a non-vector `query` subcommand
that runs against the LanceDB table directly:

```
openclaw memory query --cols id,text,createdAt --limit 20
openclaw memory query --filter "category = 'preference'" --order-by createdAt:desc
```

- `--cols <columns>`: comma-separated column allowlist (defaults to `id`, `text`, `importance`, `category`, `createdAt`).
- `--filter <condition>`: SQL-style WHERE clause; capped at 200 characters and restricted to alphanumerics, comparison operators, quotes, parentheses, and a small set of safe punctuation.
- `--limit <n>`: positive integer; default `10`.
- `--order-by <column>:<asc|desc>`: in-memory sort applied after the filter; the sort column is auto-included in the projection.

Agents also get LanceDB memory tools from the active memory plugin:

- `memory_recall` for LanceDB-backed recall
- `memory_store` for saving important facts, preferences, decisions, and entities
- `memory_forget` for removing matching memories

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#storage)  Storage

By default, LanceDB data lives under `~/.openclaw/memory/lancedb`. Override the
path with `dbPath`:

```
{
  plugins: {
    entries: {
      "memory-lancedb": {
        enabled: true,
        config: {
          dbPath: "~/.openclaw/memory/lancedb",
          embedding: {
            apiKey: "${OPENAI_API_KEY}",
            model: "text-embedding-3-small",
          },
        },
      },
    },
  },
}
```

`storageOptions` accepts string key/value pairs for LanceDB storage backends and
supports `${ENV_VAR}` expansion:

```
{
  plugins: {
    entries: {
      "memory-lancedb": {
        enabled: true,
        config: {
          dbPath: "s3://memory-bucket/openclaw",
          storageOptions: {
            access_key: "${AWS_ACCESS_KEY_ID}",
            secret_key: "${AWS_SECRET_ACCESS_KEY}",
            endpoint: "${AWS_ENDPOINT_URL}",
          },
          embedding: {
            apiKey: "${OPENAI_API_KEY}",
            model: "text-embedding-3-small",
          },
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#runtime-dependencies)  Runtime dependencies

`memory-lancedb` depends on the native `@lancedb/lancedb` package. Packaged
OpenClaw treats that package as part of the plugin package. Gateway startup
does not repair plugin dependencies; if the dependency is missing, reinstall or
update the plugin package and restart the Gateway.If an older install logs a missing `dist/package.json` or missing
`@lancedb/lancedb` error during plugin load, upgrade OpenClaw and restart the
Gateway.If the plugin logs that LanceDB is unavailable on `darwin-x64`, use the default
memory backend on that machine, move the Gateway to a supported platform, or
disable `memory-lancedb`.

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#troubleshooting)  Troubleshooting

### [​](https://docs.openclaw.ai/plugins/memory-lancedb\#input-length-exceeds-the-context-length)  Input length exceeds the context length

This usually means the embedding model rejected the recall query:

```
memory-lancedb: recall failed: Error: 400 the input length exceeds the context length
```

Set a lower `recallMaxChars`, then restart the Gateway:

```
{
  plugins: {
    entries: {
      "memory-lancedb": {
        config: {
          recallMaxChars: 400,
        },
      },
    },
  },
}
```

For Ollama, also verify the embedding server is reachable from the Gateway host:

```
curl http://127.0.0.1:11434/v1/embeddings \
  -H "Content-Type: application/json" \
  -d '{"model":"mxbai-embed-large","input":"hello"}'
```

### [​](https://docs.openclaw.ai/plugins/memory-lancedb\#unsupported-embedding-model)  Unsupported embedding model

Without `dimensions`, only the built-in OpenAI embedding dimensions are known.
For local or custom embedding models, set `embedding.dimensions` to the vector
size reported by that model.

### [​](https://docs.openclaw.ai/plugins/memory-lancedb\#plugin-loads-but-no-memories-appear)  Plugin loads but no memories appear

Check that `plugins.slots.memory` points at `memory-lancedb`, then run:

```
openclaw ltm stats
openclaw ltm search "recent preference"
```

If `autoCapture` is disabled, the plugin will recall existing memories but will
not automatically store new ones. Use the `memory_store` tool or enable
`autoCapture` if you want automatic capture.

## [​](https://docs.openclaw.ai/plugins/memory-lancedb\#related)  Related

- [Memory overview](https://docs.openclaw.ai/concepts/memory)
- [Active memory](https://docs.openclaw.ai/concepts/active-memory)
- [Memory search](https://docs.openclaw.ai/concepts/memory-search)
- [Memory Wiki](https://docs.openclaw.ai/plugins/memory-wiki)
- [Ollama](https://docs.openclaw.ai/providers/ollama)

[Memory wiki](https://docs.openclaw.ai/plugins/memory-wiki) [Message presentation](https://docs.openclaw.ai/plugins/message-presentation)

Ctrl+I