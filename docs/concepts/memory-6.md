---
source_url: https://docs.openclaw.ai/concepts/memory
title: "Memory overview - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/memory#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

⌘K

Search...

Navigation

Memory

Memory overview

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [How it works](https://docs.openclaw.ai/concepts/memory#how-it-works)
- [Inferred commitments](https://docs.openclaw.ai/concepts/memory#inferred-commitments)
- [Memory tools](https://docs.openclaw.ai/concepts/memory#memory-tools)
- [Memory Wiki companion plugin](https://docs.openclaw.ai/concepts/memory#memory-wiki-companion-plugin)
- [Memory search](https://docs.openclaw.ai/concepts/memory#memory-search)
- [Memory backends](https://docs.openclaw.ai/concepts/memory#memory-backends)
- [Knowledge wiki layer](https://docs.openclaw.ai/concepts/memory#knowledge-wiki-layer)
- [Automatic memory flush](https://docs.openclaw.ai/concepts/memory#automatic-memory-flush)
- [Dreaming](https://docs.openclaw.ai/concepts/memory#dreaming)
- [Grounded backfill and live promotion](https://docs.openclaw.ai/concepts/memory#grounded-backfill-and-live-promotion)
- [CLI](https://docs.openclaw.ai/concepts/memory#cli)
- [Further reading](https://docs.openclaw.ai/concepts/memory#further-reading)
- [Related](https://docs.openclaw.ai/concepts/memory#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw remembers things by writing **plain Markdown files** in your agent’s
workspace. The model only “remembers” what gets saved to disk — there is no
hidden state.

## [​](https://docs.openclaw.ai/concepts/memory\#how-it-works)  How it works

Your agent has three memory-related files:

- **`MEMORY.md`** — long-term memory. Durable facts, preferences, and
decisions. Loaded at the start of every DM session.
- **`memory/YYYY-MM-DD.md`** — daily notes. Running context and observations.
Today and yesterday’s notes are loaded automatically.
- **`DREAMS.md`** (optional) — Dream Diary and dreaming sweep
summaries for human review, including grounded historical backfill entries.

These files live in the agent workspace (default `~/.openclaw/workspace`).

If you want your agent to remember something, just ask it: “Remember that I
prefer TypeScript.” It will write it to the appropriate file.

## [​](https://docs.openclaw.ai/concepts/memory\#inferred-commitments)  Inferred commitments

Some future follow-ups are not durable facts. If you mention an interview
tomorrow, the useful memory may be “check in after the interview,” not “store
this forever in `MEMORY.md`.”[Commitments](https://docs.openclaw.ai/concepts/commitments) are opt-in, short-lived follow-up memories
for that case. OpenClaw infers them in a hidden background pass, scopes them to
the same agent and channel, and delivers due check-ins through heartbeat.
Explicit reminders still use [scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs).

## [​](https://docs.openclaw.ai/concepts/memory\#memory-tools)  Memory tools

The agent has two tools for working with memory:

- **`memory_search`** — finds relevant notes using semantic search, even when
the wording differs from the original.
- **`memory_get`** — reads a specific memory file or line range.

Both tools are provided by the active memory plugin (default: `memory-core`).

## [​](https://docs.openclaw.ai/concepts/memory\#memory-wiki-companion-plugin)  Memory Wiki companion plugin

If you want durable memory to behave more like a maintained knowledge base than
just raw notes, use the bundled `memory-wiki` plugin.`memory-wiki` compiles durable knowledge into a wiki vault with:

- deterministic page structure
- structured claims and evidence
- contradiction and freshness tracking
- generated dashboards
- compiled digests for agent/runtime consumers
- wiki-native tools like `wiki_search`, `wiki_get`, `wiki_apply`, and `wiki_lint`

It does not replace the active memory plugin. The active memory plugin still
owns recall, promotion, and dreaming. `memory-wiki` adds a provenance-rich
knowledge layer beside it.See [Memory Wiki](https://docs.openclaw.ai/plugins/memory-wiki).

## [​](https://docs.openclaw.ai/concepts/memory\#memory-search)  Memory search

When an embedding provider is configured, `memory_search` uses **hybrid**
**search** — combining vector similarity (semantic meaning) with keyword matching
(exact terms like IDs and code symbols). This works out of the box once you have
an API key for any supported provider.

OpenClaw auto-detects your embedding provider from available API keys. If you
have an OpenAI, Gemini, Voyage, or Mistral key configured, memory search is
enabled automatically.

For details on how search works, tuning options, and provider setup, see
[Memory Search](https://docs.openclaw.ai/concepts/memory-search).

## [​](https://docs.openclaw.ai/concepts/memory\#memory-backends)  Memory backends

## Builtin (default)

SQLite-based. Works out of the box with keyword search, vector similarity, and
hybrid search. No extra dependencies.

## QMD

Local-first sidecar with reranking, query expansion, and the ability to index
directories outside the workspace.

## Honcho

AI-native cross-session memory with user modeling, semantic search, and
multi-agent awareness. Plugin install.

## LanceDB

Bundled LanceDB-backed memory with OpenAI-compatible embeddings, auto-recall,
auto-capture, and local Ollama embedding support.

## [​](https://docs.openclaw.ai/concepts/memory\#knowledge-wiki-layer)  Knowledge wiki layer

## Memory Wiki

Compiles durable memory into a provenance-rich wiki vault with claims,
dashboards, bridge mode, and Obsidian-friendly workflows.

## [​](https://docs.openclaw.ai/concepts/memory\#automatic-memory-flush)  Automatic memory flush

Before [compaction](https://docs.openclaw.ai/concepts/compaction) summarizes your conversation, OpenClaw
runs a silent turn that reminds the agent to save important context to memory
files. This is on by default — you do not need to configure anything.To keep that housekeeping turn on a local model, set an exact memory-flush model
override:

```
{
  "agents": {
    "defaults": {
      "compaction": {
        "memoryFlush": {
          "model": "ollama/qwen3:8b"
        }
      }
    }
  }
}
```

The override applies only to the memory-flush turn and does not inherit the
active session fallback chain.

The memory flush prevents context loss during compaction. If your agent has
important facts in the conversation that are not yet written to a file, they
will be saved automatically before the summary happens.

## [​](https://docs.openclaw.ai/concepts/memory\#dreaming)  Dreaming

Dreaming is an optional background consolidation pass for memory. It collects
short-term signals, scores candidates, and promotes only qualified items into
long-term memory (`MEMORY.md`).It is designed to keep long-term memory high signal:

- **Opt-in**: disabled by default.
- **Scheduled**: when enabled, `memory-core` auto-manages one recurring cron job
for a full dreaming sweep.
- **Thresholded**: promotions must pass score, recall frequency, and query
diversity gates.
- **Reviewable**: phase summaries and diary entries are written to `DREAMS.md`
for human review.

For phase behavior, scoring signals, and Dream Diary details, see
[Dreaming](https://docs.openclaw.ai/concepts/dreaming).

## [​](https://docs.openclaw.ai/concepts/memory\#grounded-backfill-and-live-promotion)  Grounded backfill and live promotion

The dreaming system now has two closely related review lanes:

- **Live dreaming** works from the short-term dreaming store under
`memory/.dreams/` and is what the normal deep phase uses when deciding what
can graduate into `MEMORY.md`.
- **Grounded backfill** reads historical `memory/YYYY-MM-DD.md` notes as
standalone day files and writes structured review output into `DREAMS.md`.

Grounded backfill is useful when you want to replay older notes and inspect what
the system thinks is durable without manually editing `MEMORY.md`.When you use:

```
openclaw memory rem-backfill --path ./memory --stage-short-term
```

the grounded durable candidates are not promoted directly. They are staged into
the same short-term dreaming store the normal deep phase already uses. That
means:

- `DREAMS.md` stays the human review surface.
- the short-term store stays the machine-facing ranking surface.
- `MEMORY.md` is still only written by deep promotion.

If you decide the replay was not useful, you can remove the staged artifacts
without touching ordinary diary entries or normal recall state:

```
openclaw memory rem-backfill --rollback
openclaw memory rem-backfill --rollback-short-term
```

## [​](https://docs.openclaw.ai/concepts/memory\#cli)  CLI

```
openclaw memory status          # Check index status and provider
openclaw memory search "query"  # Search from the command line
openclaw memory index --force   # Rebuild the index
```

## [​](https://docs.openclaw.ai/concepts/memory\#further-reading)  Further reading

- [Builtin memory engine](https://docs.openclaw.ai/concepts/memory-builtin): default SQLite backend.
- [QMD memory engine](https://docs.openclaw.ai/concepts/memory-qmd): advanced local-first sidecar.
- [Honcho memory](https://docs.openclaw.ai/concepts/memory-honcho): AI-native cross-session memory.
- [Memory LanceDB](https://docs.openclaw.ai/plugins/memory-lancedb): LanceDB-backed plugin with OpenAI-compatible embeddings.
- [Memory Wiki](https://docs.openclaw.ai/plugins/memory-wiki): compiled knowledge vault and wiki-native tools.
- [Memory search](https://docs.openclaw.ai/concepts/memory-search): search pipeline, providers, and tuning.
- [Dreaming](https://docs.openclaw.ai/concepts/dreaming): background promotion from short-term recall to long-term memory.
- [Memory configuration reference](https://docs.openclaw.ai/reference/memory-config): all config knobs.
- [Compaction](https://docs.openclaw.ai/concepts/compaction): how compaction interacts with memory.

## [​](https://docs.openclaw.ai/concepts/memory\#related)  Related

- [Active memory](https://docs.openclaw.ai/concepts/active-memory)
- [Memory search](https://docs.openclaw.ai/concepts/memory-search)
- [Builtin memory engine](https://docs.openclaw.ai/concepts/memory-builtin)
- [Honcho memory](https://docs.openclaw.ai/concepts/memory-honcho)
- [Memory LanceDB](https://docs.openclaw.ai/plugins/memory-lancedb)
- [Commitments](https://docs.openclaw.ai/concepts/commitments)

[Session tools](https://docs.openclaw.ai/concepts/session-tool) [Builtin memory engine](https://docs.openclaw.ai/concepts/memory-builtin)

⌘I