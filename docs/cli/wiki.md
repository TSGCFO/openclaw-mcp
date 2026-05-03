---
source_url: https://docs.openclaw.ai/cli/wiki
title: "Wiki - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/wiki#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Utility

Wiki

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw wiki](https://docs.openclaw.ai/cli/wiki#openclaw-wiki)
- [What it is for](https://docs.openclaw.ai/cli/wiki#what-it-is-for)
- [Common commands](https://docs.openclaw.ai/cli/wiki#common-commands)
- [Commands](https://docs.openclaw.ai/cli/wiki#commands)
- [wiki status](https://docs.openclaw.ai/cli/wiki#wiki-status)
- [wiki doctor](https://docs.openclaw.ai/cli/wiki#wiki-doctor)
- [wiki init](https://docs.openclaw.ai/cli/wiki#wiki-init)
- [wiki ingest <path-or-url>](https://docs.openclaw.ai/cli/wiki#wiki-ingest-%3Cpath-or-url%3E)
- [wiki compile](https://docs.openclaw.ai/cli/wiki#wiki-compile)
- [wiki lint](https://docs.openclaw.ai/cli/wiki#wiki-lint)
- [wiki search <query>](https://docs.openclaw.ai/cli/wiki#wiki-search-%3Cquery%3E)
- [wiki get <lookup>](https://docs.openclaw.ai/cli/wiki#wiki-get-%3Clookup%3E)
- [wiki apply](https://docs.openclaw.ai/cli/wiki#wiki-apply)
- [wiki bridge import](https://docs.openclaw.ai/cli/wiki#wiki-bridge-import)
- [wiki unsafe-local import](https://docs.openclaw.ai/cli/wiki#wiki-unsafe-local-import)
- [wiki obsidian ...](https://docs.openclaw.ai/cli/wiki#wiki-obsidian)
- [Practical usage guidance](https://docs.openclaw.ai/cli/wiki#practical-usage-guidance)
- [Configuration tie-ins](https://docs.openclaw.ai/cli/wiki#configuration-tie-ins)
- [Related](https://docs.openclaw.ai/cli/wiki#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/wiki\#openclaw-wiki)  `openclaw wiki`

Inspect and maintain the `memory-wiki` vault.Provided by the bundled `memory-wiki` plugin.Related:

- [Memory Wiki plugin](https://docs.openclaw.ai/plugins/memory-wiki)
- [Memory Overview](https://docs.openclaw.ai/concepts/memory)
- [CLI: memory](https://docs.openclaw.ai/cli/memory)

## [​](https://docs.openclaw.ai/cli/wiki\#what-it-is-for)  What it is for

Use `openclaw wiki` when you want a compiled knowledge vault with:

- wiki-native search and page reads
- provenance-rich syntheses
- contradiction and freshness reports
- bridge imports from the active memory plugin
- optional Obsidian CLI helpers

## [​](https://docs.openclaw.ai/cli/wiki\#common-commands)  Common commands

```
openclaw wiki status
openclaw wiki doctor
openclaw wiki init
openclaw wiki ingest ./notes/alpha.md
openclaw wiki compile
openclaw wiki lint
openclaw wiki search "alpha"
openclaw wiki search "who should I ask about Teams?" --mode route-question
openclaw wiki get entity.alpha --from 1 --lines 80

openclaw wiki apply synthesis "Alpha Summary" \
  --body "Short synthesis body" \
  --source-id source.alpha

openclaw wiki apply metadata entity.alpha \
  --source-id source.alpha \
  --status review \
  --question "Still active?"

openclaw wiki bridge import
openclaw wiki unsafe-local import

openclaw wiki obsidian status
openclaw wiki obsidian search "alpha"
openclaw wiki obsidian open syntheses/alpha-summary.md
openclaw wiki obsidian command workspace:quick-switcher
openclaw wiki obsidian daily
```

## [​](https://docs.openclaw.ai/cli/wiki\#commands)  Commands

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-status)  `wiki status`

Inspect current vault mode, health, and Obsidian CLI availability.Use this first when you are unsure whether the vault is initialized, bridge mode
is healthy, or Obsidian integration is available.When bridge mode is active and configured to read memory artifacts, this command
queries the running Gateway so it sees the same active memory plugin context as
agent/runtime memory.

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-doctor)  `wiki doctor`

Run wiki health checks and surface configuration or vault problems.When bridge mode is active and configured to read memory artifacts, this command
queries the running Gateway before building the report. Disabled bridge imports
and bridge configs that do not read memory artifacts remain local/offline.Typical issues include:

- bridge mode enabled without public memory artifacts
- invalid or missing vault layout
- missing external Obsidian CLI when Obsidian mode is expected

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-init)  `wiki init`

Create the wiki vault layout and starter pages.This initializes the root structure, including top-level indexes and cache
directories.

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-ingest-%3Cpath-or-url%3E)  `wiki ingest <path-or-url>`

Import content into the wiki source layer.Notes:

- URL ingest is controlled by `ingest.allowUrlIngest`
- imported source pages keep provenance in frontmatter
- auto-compile can run after ingest when enabled

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-compile)  `wiki compile`

Rebuild indexes, related blocks, dashboards, and compiled digests.This writes stable machine-facing artifacts under:

- `.openclaw-wiki/cache/agent-digest.json`
- `.openclaw-wiki/cache/claims.jsonl`

If `render.createDashboards` is enabled, compile also refreshes report pages.

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-lint)  `wiki lint`

Lint the vault and report:

- structural issues
- provenance gaps
- contradictions
- open questions
- low-confidence pages/claims
- stale pages/claims

Run this after meaningful wiki updates.

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-search-%3Cquery%3E)  `wiki search <query>`

Search wiki content.Behavior depends on config:

- `search.backend`: `shared` or `local`
- `search.corpus`: `wiki`, `memory`, or `all`
- `--mode`: `auto`, `find-person`, `route-question`, `source-evidence`, or
`raw-claim`

Use `wiki search` when you want wiki-specific ranking or provenance details.
For one broad shared recall pass, prefer `openclaw memory search` when the
active memory plugin exposes shared search.Search modes help the agent choose the right surface:

- `find-person`: aliases, handles, socials, canonical IDs, and person pages
- `route-question`: ask-for/best-used-for hints and relationship context
- `source-evidence`: source pages and structured evidence fields
- `raw-claim`: structured claim text with claim/evidence metadata

Examples:

```
openclaw wiki search "bgroux" --mode find-person
openclaw wiki search "who knows Teams rollout?" --mode route-question
openclaw wiki search "maintainer-whois" --mode source-evidence
openclaw wiki search "strong route Teams" --mode raw-claim --json
```

Text output includes `Claim:` and `Evidence:` lines when a result matches a
structured claim. JSON output additionally exposes `matchedClaimId`,
`matchedClaimStatus`, `matchedClaimConfidence`, `evidenceKinds`, and
`evidenceSourceIds` for agent-side drilldown.

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-get-%3Clookup%3E)  `wiki get <lookup>`

Read a wiki page by id or relative path.Examples:

```
openclaw wiki get entity.alpha
openclaw wiki get syntheses/alpha-summary.md --from 1 --lines 80
```

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-apply)  `wiki apply`

Apply narrow mutations without freeform page surgery.Supported flows include:

- create/update a synthesis page
- update page metadata
- attach source ids
- add questions
- add contradictions
- update confidence/status
- write structured claims

This command exists so the wiki can evolve safely without manually editing
managed blocks.

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-bridge-import)  `wiki bridge import`

Import public memory artifacts from the active memory plugin into bridge-backed
source pages.Use this in `bridge` mode when you want the latest exported memory artifacts
pulled into the wiki vault.For active bridge artifact reads, the CLI routes the import through Gateway RPC
so the import uses the runtime memory plugin context. If bridge imports are
disabled or artifact reads are turned off, the command keeps the local/offline
zero-import behavior.

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-unsafe-local-import)  `wiki unsafe-local import`

Import from explicitly configured local paths in `unsafe-local` mode.This is intentionally experimental and same-machine only.

### [​](https://docs.openclaw.ai/cli/wiki\#wiki-obsidian)  `wiki obsidian ...`

Obsidian helper commands for vaults running in Obsidian-friendly mode.Subcommands:

- `status`
- `search`
- `open`
- `command`
- `daily`

These require the official `obsidian` CLI on `PATH` when
`obsidian.useOfficialCli` is enabled.

## [​](https://docs.openclaw.ai/cli/wiki\#practical-usage-guidance)  Practical usage guidance

- Use `wiki search` \+ `wiki get` when provenance and page identity matter.
- Use `wiki apply` instead of hand-editing managed generated sections.
- Use `wiki lint` before trusting contradictory or low-confidence content.
- Use `wiki compile` after bulk imports or source changes when you want fresh
dashboards and compiled digests immediately.
- Use `wiki bridge import` when bridge mode depends on newly exported memory
artifacts.

## [​](https://docs.openclaw.ai/cli/wiki\#configuration-tie-ins)  Configuration tie-ins

`openclaw wiki` behavior is shaped by:

- `plugins.entries.memory-wiki.config.vaultMode`
- `plugins.entries.memory-wiki.config.search.backend`
- `plugins.entries.memory-wiki.config.search.corpus`
- `plugins.entries.memory-wiki.config.bridge.*`
- `plugins.entries.memory-wiki.config.obsidian.*`
- `plugins.entries.memory-wiki.config.render.*`
- `plugins.entries.memory-wiki.config.context.includeCompiledDigestPrompt`

See [Memory Wiki plugin](https://docs.openclaw.ai/plugins/memory-wiki) for the full config model.

## [​](https://docs.openclaw.ai/cli/wiki\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Memory wiki](https://docs.openclaw.ai/plugins/memory-wiki)

[Proxy](https://docs.openclaw.ai/cli/proxy) [RPC adapters](https://docs.openclaw.ai/reference/rpc)

Ctrl+I