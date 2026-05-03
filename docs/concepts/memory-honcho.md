---
source_url: https://docs.openclaw.ai/concepts/memory-honcho
title: "Honcho memory - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/memory-honcho#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Memory

Honcho memory

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What it provides](https://docs.openclaw.ai/concepts/memory-honcho#what-it-provides)
- [Available tools](https://docs.openclaw.ai/concepts/memory-honcho#available-tools)
- [Getting started](https://docs.openclaw.ai/concepts/memory-honcho#getting-started)
- [Configuration](https://docs.openclaw.ai/concepts/memory-honcho#configuration)
- [Migrating existing memory](https://docs.openclaw.ai/concepts/memory-honcho#migrating-existing-memory)
- [How it works](https://docs.openclaw.ai/concepts/memory-honcho#how-it-works)
- [Honcho vs builtin memory](https://docs.openclaw.ai/concepts/memory-honcho#honcho-vs-builtin-memory)
- [CLI commands](https://docs.openclaw.ai/concepts/memory-honcho#cli-commands)
- [Further reading](https://docs.openclaw.ai/concepts/memory-honcho#further-reading)
- [Related](https://docs.openclaw.ai/concepts/memory-honcho#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[Honcho](https://honcho.dev/) adds AI-native memory to OpenClaw. It persists
conversations to a dedicated service and builds user and agent models over time,
giving your agent cross-session context that goes beyond workspace Markdown
files.

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#what-it-provides)  What it provides

- **Cross-session memory** — conversations are persisted after every turn, so
context carries across session resets, compaction, and channel switches.
- **User modeling** — Honcho maintains a profile for each user (preferences,
facts, communication style) and for the agent (personality, learned
behaviors).
- **Semantic search** — search over observations from past conversations, not
just the current session.
- **Multi-agent awareness** — parent agents automatically track spawned
sub-agents, with parents added as observers in child sessions.

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#available-tools)  Available tools

Honcho registers tools that the agent can use during conversation:**Data retrieval (fast, no LLM call):**

| Tool | What it does |
| --- | --- |
| `honcho_context` | Full user representation across sessions |
| `honcho_search_conclusions` | Semantic search over stored conclusions |
| `honcho_search_messages` | Find messages across sessions (filter by sender, date) |
| `honcho_session` | Current session history and summary |

**Q&A (LLM-powered):**

| Tool | What it does |
| --- | --- |
| `honcho_ask` | Ask about the user. `depth='quick'` for facts, `'thorough'` for synthesis |

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#getting-started)  Getting started

Install the plugin and run setup:

```
openclaw plugins install @honcho-ai/openclaw-honcho
openclaw honcho setup
openclaw gateway --force
```

The setup command prompts for your API credentials, writes the config, and
optionally migrates existing workspace memory files.

Honcho can run entirely locally (self-hosted) or via the managed API at
`api.honcho.dev`. No external dependencies are required for the self-hosted
option.

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#configuration)  Configuration

Settings live under `plugins.entries["openclaw-honcho"].config`:

```
{
  plugins: {
    entries: {
      "openclaw-honcho": {
        config: {
          apiKey: "your-api-key", // omit for self-hosted
          workspaceId: "openclaw", // memory isolation
          baseUrl: "https://api.honcho.dev",
        },
      },
    },
  },
}
```

For self-hosted instances, point `baseUrl` to your local server (for example
`http://localhost:8000`) and omit the API key.

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#migrating-existing-memory)  Migrating existing memory

If you have existing workspace memory files (`USER.md`, `MEMORY.md`,
`IDENTITY.md`, `memory/`, `canvas/`), `openclaw honcho setup` detects and
offers to migrate them.

Migration is non-destructive — files are uploaded to Honcho. Originals are
never deleted or moved.

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#how-it-works)  How it works

After every AI turn, the conversation is persisted to Honcho. Both user and
agent messages are observed, allowing Honcho to build and refine its models over
time.During conversation, Honcho tools query the service in the `before_prompt_build`
phase, injecting relevant context before the model sees the prompt. This ensures
accurate turn boundaries and relevant recall.

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#honcho-vs-builtin-memory)  Honcho vs builtin memory

|  | Builtin / QMD | Honcho |
| --- | --- | --- |
| **Storage** | Workspace Markdown files | Dedicated service (local or hosted) |
| **Cross-session** | Via memory files | Automatic, built-in |
| **User modeling** | Manual (write to MEMORY.md) | Automatic profiles |
| **Search** | Vector + keyword (hybrid) | Semantic over observations |
| **Multi-agent** | Not tracked | Parent/child awareness |
| **Dependencies** | None (builtin) or QMD binary | Plugin install |

Honcho and the builtin memory system can work together. When QMD is configured,
additional tools become available for searching local Markdown files alongside
Honcho’s cross-session memory.

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#cli-commands)  CLI commands

```
openclaw honcho setup                        # Configure API key and migrate files
openclaw honcho status                       # Check connection status
openclaw honcho ask <question>               # Query Honcho about the user
openclaw honcho search <query> [-k N] [-d D] # Semantic search over memory
```

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#further-reading)  Further reading

- [Plugin source code](https://github.com/plastic-labs/openclaw-honcho)
- [Honcho documentation](https://docs.honcho.dev/)
- [Honcho OpenClaw integration guide](https://docs.honcho.dev/v3/guides/integrations/openclaw)
- [Memory](https://docs.openclaw.ai/concepts/memory) — OpenClaw memory overview
- [Context Engines](https://docs.openclaw.ai/concepts/context-engine) — how plugin context engines work

## [​](https://docs.openclaw.ai/concepts/memory-honcho\#related)  Related

- [Memory overview](https://docs.openclaw.ai/concepts/memory)
- [Builtin memory engine](https://docs.openclaw.ai/concepts/memory-builtin)
- [QMD memory engine](https://docs.openclaw.ai/concepts/memory-qmd)

[QMD memory engine](https://docs.openclaw.ai/concepts/memory-qmd) [Memory search](https://docs.openclaw.ai/concepts/memory-search)

Ctrl+I