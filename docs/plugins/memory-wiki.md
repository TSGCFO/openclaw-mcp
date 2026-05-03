---
source_url: https://docs.openclaw.ai/plugins/memory-wiki
title: "Memory wiki - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/plugins/memory-wiki#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Plugins

Memory wiki

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What it adds](https://docs.openclaw.ai/plugins/memory-wiki#what-it-adds)
- [How it fits with memory](https://docs.openclaw.ai/plugins/memory-wiki#how-it-fits-with-memory)
- [Recommended hybrid pattern](https://docs.openclaw.ai/plugins/memory-wiki#recommended-hybrid-pattern)
- [Vault modes](https://docs.openclaw.ai/plugins/memory-wiki#vault-modes)
- [isolated](https://docs.openclaw.ai/plugins/memory-wiki#isolated)
- [bridge](https://docs.openclaw.ai/plugins/memory-wiki#bridge)
- [unsafe-local](https://docs.openclaw.ai/plugins/memory-wiki#unsafe-local)
- [Vault layout](https://docs.openclaw.ai/plugins/memory-wiki#vault-layout)
- [Structured claims and evidence](https://docs.openclaw.ai/plugins/memory-wiki#structured-claims-and-evidence)
- [Agent-facing entity metadata](https://docs.openclaw.ai/plugins/memory-wiki#agent-facing-entity-metadata)
- [Compile pipeline](https://docs.openclaw.ai/plugins/memory-wiki#compile-pipeline)
- [Dashboards and health reports](https://docs.openclaw.ai/plugins/memory-wiki#dashboards-and-health-reports)
- [Search and retrieval](https://docs.openclaw.ai/plugins/memory-wiki#search-and-retrieval)
- [Agent tools](https://docs.openclaw.ai/plugins/memory-wiki#agent-tools)
- [Prompt and context behavior](https://docs.openclaw.ai/plugins/memory-wiki#prompt-and-context-behavior)
- [Configuration](https://docs.openclaw.ai/plugins/memory-wiki#configuration)
- [Example: QMD + bridge mode](https://docs.openclaw.ai/plugins/memory-wiki#example-qmd-%2B-bridge-mode)
- [CLI](https://docs.openclaw.ai/plugins/memory-wiki#cli)
- [Obsidian support](https://docs.openclaw.ai/plugins/memory-wiki#obsidian-support)
- [Recommended workflow](https://docs.openclaw.ai/plugins/memory-wiki#recommended-workflow)
- [Related docs](https://docs.openclaw.ai/plugins/memory-wiki#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

`memory-wiki` is a bundled plugin that turns durable memory into a compiled
knowledge vault.It does **not** replace the active memory plugin. The active memory plugin still
owns recall, promotion, indexing, and dreaming. `memory-wiki` sits beside it
and compiles durable knowledge into a navigable wiki with deterministic pages,
structured claims, provenance, dashboards, and machine-readable digests.Use it when you want memory to behave more like a maintained knowledge layer and
less like a pile of Markdown files.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#what-it-adds)  What it adds

- A dedicated wiki vault with deterministic page layout
- Structured claim and evidence metadata, not just prose
- Page-level provenance, confidence, contradictions, and open questions
- Compiled digests for agent/runtime consumers
- Wiki-native search/get/apply/lint tools
- Optional bridge mode that imports public artifacts from the active memory plugin
- Optional Obsidian-friendly render mode and CLI integration

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#how-it-fits-with-memory)  How it fits with memory

Think of the split like this:

| Layer | Owns |
| --- | --- |
| Active memory plugin (`memory-core`, QMD, Honcho, etc.) | Recall, semantic search, promotion, dreaming, memory runtime |
| `memory-wiki` | Compiled wiki pages, provenance-rich syntheses, dashboards, wiki-specific search/get/apply |

If the active memory plugin exposes shared recall artifacts, OpenClaw can search
both layers in one pass with `memory_search corpus=all`.When you need wiki-specific ranking, provenance, or direct page access, use the
wiki-native tools instead.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#recommended-hybrid-pattern)  Recommended hybrid pattern

A strong default for local-first setups is:

- QMD as the active memory backend for recall and broad semantic search
- `memory-wiki` in `bridge` mode for durable synthesized knowledge pages

That split works well because each layer stays focused:

- QMD keeps raw notes, session exports, and extra collections searchable
- `memory-wiki` compiles stable entities, claims, dashboards, and source pages

Practical rule:

- use `memory_search` when you want one broad recall pass across memory
- use `wiki_search` and `wiki_get` when you want provenance-aware wiki results
- use `memory_search corpus=all` when you want shared search to span both layers

If bridge mode reports zero exported artifacts, the active memory plugin is not
currently exposing public bridge inputs yet. Run `openclaw wiki doctor` first,
then confirm the active memory plugin supports public artifacts.When bridge mode is active and `bridge.readMemoryArtifacts` is enabled,
`openclaw wiki status`, `openclaw wiki doctor`, and `openclaw wiki bridge import` read through the running Gateway. That keeps CLI bridge checks aligned
with the runtime memory plugin context. If bridge is disabled or artifact reads
are turned off, those commands keep their local/offline behavior.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#vault-modes)  Vault modes

`memory-wiki` supports three vault modes:

### [​](https://docs.openclaw.ai/plugins/memory-wiki\#isolated)  `isolated`

Own vault, own sources, no dependency on `memory-core`.Use this when you want the wiki to be its own curated knowledge store.

### [​](https://docs.openclaw.ai/plugins/memory-wiki\#bridge)  `bridge`

Reads public memory artifacts and memory events from the active memory plugin
through public plugin SDK seams.Use this when you want the wiki to compile and organize the memory plugin’s
exported artifacts without reaching into private plugin internals.Bridge mode can index:

- exported memory artifacts
- dream reports
- daily notes
- memory root files
- memory event logs

### [​](https://docs.openclaw.ai/plugins/memory-wiki\#unsafe-local)  `unsafe-local`

Explicit same-machine escape hatch for local private paths.This mode is intentionally experimental and non-portable. Use it only when you
understand the trust boundary and specifically need local filesystem access that
bridge mode cannot provide.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#vault-layout)  Vault layout

The plugin initializes a vault like this:

```
<vault>/
  AGENTS.md
  WIKI.md
  index.md
  inbox.md
  entities/
  concepts/
  syntheses/
  sources/
  reports/
  _attachments/
  _views/
  .openclaw-wiki/
```

Managed content stays inside generated blocks. Human note blocks are preserved.The main page groups are:

- `sources/` for imported raw material and bridge-backed pages
- `entities/` for durable things, people, systems, projects, and objects
- `concepts/` for ideas, abstractions, patterns, and policies
- `syntheses/` for compiled summaries and maintained rollups
- `reports/` for generated dashboards

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#structured-claims-and-evidence)  Structured claims and evidence

Pages can carry structured `claims` frontmatter, not just freeform text.Each claim can include:

- `id`
- `text`
- `status`
- `confidence`
- `evidence[]`
- `updatedAt`

Evidence entries can include:

- `kind`
- `sourceId`
- `path`
- `lines`
- `weight`
- `confidence`
- `privacyTier`
- `note`
- `updatedAt`

This is what makes the wiki act more like a belief layer than a passive note
dump. Claims can be tracked, scored, contested, and resolved back to sources.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#agent-facing-entity-metadata)  Agent-facing entity metadata

Entity pages can also carry routing metadata for agent use. This is generic
frontmatter, so it works for people, teams, systems, projects, or any other
entity type.Common fields include:

- `entityType`: for example `person`, `team`, `system`, or `project`
- `canonicalId`: stable identity key used across aliases and imports
- `aliases`: names, handles, or labels that should resolve to the same page
- `privacyTier`: `public`, `local-private`, `sensitive`, or `confirm-before-use`
- `bestUsedFor` / `notEnoughFor`: compact routing hints
- `lastRefreshedAt`: source-refresh timestamp separate from page edit time
- `personCard`: optional person-specific routing card with handles, socials,
emails, timezone, lane, ask-for, avoid-asking-for, confidence, and privacy
- `relationships`: typed edges to related pages with target, kind, weight,
confidence, evidence kind, privacy tier, and note

For a people wiki, the agent should usually start with
`reports/person-agent-directory.md`, then open the person page with `wiki_get`
before using contact details or inferred facts.Example:

```
pageType: entity
entityType: person
id: entity.brad-groux
canonicalId: maintainer.brad-groux
aliases:
  - Brad
  - bgroux
privacyTier: local-private
bestUsedFor:
  - Microsoft Teams and Azure routing
notEnoughFor:
  - legal approval
lastRefreshedAt: "2026-04-29T00:00:00.000Z"
personCard:
  handles:
    - "@bgroux"
  socials:
    - "https://x.example/bgroux"
  emails:
    - brad@example.com
  timezone: America/Chicago
  lane: Microsoft ecosystem
  askFor:
    - Teams rollout questions
  avoidAskingFor:
    - unrelated billing decisions
  confidence: 0.8
  privacyTier: confirm-before-use
relationships:
  - targetId: entity.alice
    targetTitle: Alice
    kind: collaborates-with
    confidence: 0.7
    evidenceKind: discrawl-stat
claims:
  - id: claim.brad.teams
    text: Brad is useful for Microsoft Teams routing.
    status: supported
    confidence: 0.9
    evidence:
      - kind: maintainer-whois
        sourceId: source.maintainers
        privacyTier: local-private
```

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#compile-pipeline)  Compile pipeline

The compile step reads wiki pages, normalizes summaries, and emits stable
machine-facing artifacts under:

- `.openclaw-wiki/cache/agent-digest.json`
- `.openclaw-wiki/cache/claims.jsonl`

These digests exist so agents and runtime code do not have to scrape Markdown
pages.Compiled output also powers:

- first-pass wiki indexing for search/get flows
- claim-id lookup back to owning pages
- compact prompt supplements
- report/dashboard generation

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#dashboards-and-health-reports)  Dashboards and health reports

When `render.createDashboards` is enabled, compile maintains dashboards under
`reports/`.Built-in reports include:

- `reports/open-questions.md`
- `reports/contradictions.md`
- `reports/low-confidence.md`
- `reports/claim-health.md`
- `reports/stale-pages.md`
- `reports/person-agent-directory.md`
- `reports/relationship-graph.md`
- `reports/provenance-coverage.md`
- `reports/privacy-review.md`

These reports track things like:

- contradiction note clusters
- competing claim clusters
- claims missing structured evidence
- low-confidence pages and claims
- stale or unknown freshness
- pages with unresolved questions
- person/entity routing cards
- structured relationship edges
- evidence class coverage
- non-public privacy tiers that need review before use

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#search-and-retrieval)  Search and retrieval

`memory-wiki` supports two search backends:

- `shared`: use the shared memory search flow when available
- `local`: search the wiki locally

It also supports three corpora:

- `wiki`
- `memory`
- `all`

Important behavior:

- `wiki_search` and `wiki_get` use compiled digests as a first pass when possible
- claim ids can resolve back to the owning page
- contested/stale/fresh claims influence ranking
- provenance labels can survive into results
- search mode can bias ranking for person lookup, question routing, source
evidence, or raw claims

Practical rule:

- use `memory_search corpus=all` for one broad recall pass
- use `wiki_search` \+ `wiki_get` when you care about wiki-specific ranking,
provenance, or page-level belief structure

Search modes:

- `auto`: balanced default
- `find-person`: boost person-like entities, aliases, handles, socials, and
canonical IDs
- `route-question`: boost agent cards, ask-for hints, best-used-for hints, and
relationship context
- `source-evidence`: boost source pages and structured evidence metadata
- `raw-claim`: boost matching structured claims and return claim/evidence
metadata in results

When a result matches a structured claim, `wiki_search` can return
`matchedClaimId`, `matchedClaimStatus`, `matchedClaimConfidence`,
`evidenceKinds`, and `evidenceSourceIds` in its details payload. Text output
also includes compact `Claim:` and `Evidence:` lines when available.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#agent-tools)  Agent tools

The plugin registers these tools:

- `wiki_status`
- `wiki_search`
- `wiki_get`
- `wiki_apply`
- `wiki_lint`

What they do:

- `wiki_status`: current vault mode, health, Obsidian CLI availability
- `wiki_search`: search wiki pages and, when configured, shared memory corpora;
accepts `mode` for person lookup, question routing, source evidence, or raw
claim drilldown
- `wiki_get`: read a wiki page by id/path or fall back to shared memory corpus
- `wiki_apply`: narrow synthesis/metadata mutations without freeform page surgery
- `wiki_lint`: structural checks, provenance gaps, contradictions, open questions

The plugin also registers a non-exclusive memory corpus supplement, so shared
`memory_search` and `memory_get` can reach the wiki when the active memory
plugin supports corpus selection.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#prompt-and-context-behavior)  Prompt and context behavior

When `context.includeCompiledDigestPrompt` is enabled, memory prompt sections
append a compact compiled snapshot from `agent-digest.json`.That snapshot is intentionally small and high-signal:

- top pages only
- top claims only
- contradiction count
- question count
- confidence/freshness qualifiers

This is opt-in because it changes prompt shape and is mainly useful for context
engines or legacy prompt assembly that explicitly consume memory supplements.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#configuration)  Configuration

Put config under `plugins.entries.memory-wiki.config`:

```
{
  plugins: {
    entries: {
      "memory-wiki": {
        enabled: true,
        config: {
          vaultMode: "isolated",
          vault: {
            path: "~/.openclaw/wiki/main",
            renderMode: "obsidian",
          },
          obsidian: {
            enabled: true,
            useOfficialCli: true,
            vaultName: "OpenClaw Wiki",
            openAfterWrites: false,
          },
          bridge: {
            enabled: false,
            readMemoryArtifacts: true,
            indexDreamReports: true,
            indexDailyNotes: true,
            indexMemoryRoot: true,
            followMemoryEvents: true,
          },
          ingest: {
            autoCompile: true,
            maxConcurrentJobs: 1,
            allowUrlIngest: true,
          },
          search: {
            backend: "shared",
            corpus: "wiki",
          },
          context: {
            includeCompiledDigestPrompt: false,
          },
          render: {
            preserveHumanBlocks: true,
            createBacklinks: true,
            createDashboards: true,
          },
        },
      },
    },
  },
}
```

Key toggles:

- `vaultMode`: `isolated`, `bridge`, `unsafe-local`
- `vault.renderMode`: `native` or `obsidian`
- `bridge.readMemoryArtifacts`: import active memory plugin public artifacts
- `bridge.followMemoryEvents`: include event logs in bridge mode
- `search.backend`: `shared` or `local`
- `search.corpus`: `wiki`, `memory`, or `all`
- `context.includeCompiledDigestPrompt`: append compact digest snapshot to memory prompt sections
- `render.createBacklinks`: generate deterministic related blocks
- `render.createDashboards`: generate dashboard pages

### [​](https://docs.openclaw.ai/plugins/memory-wiki\#example-qmd-+-bridge-mode)  Example: QMD + bridge mode

Use this when you want QMD for recall and `memory-wiki` for a maintained
knowledge layer:

```
{
  memory: {
    backend: "qmd",
      "memory-wiki": {
        enabled: true,
        config: {
          vaultMode: "bridge",
          bridge: {
            enabled: true,
            readMemoryArtifacts: true,
            indexDreamReports: true,
            indexDailyNotes: true,
            indexMemoryRoot: true,
            followMemoryEvents: true,
          },
          search: {
            backend: "shared",
            corpus: "all",
          },
          context: {
            includeCompiledDigestPrompt: false,
          },
        },
      },
    },
  },
}
```

This keeps:

- QMD in charge of active memory recall
- `memory-wiki` focused on compiled pages and dashboards
- prompt shape unchanged until you intentionally enable compiled digest prompts

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#cli)  CLI

`memory-wiki` also exposes a top-level CLI surface:

```
openclaw wiki status
openclaw wiki doctor
openclaw wiki init
openclaw wiki ingest ./notes/alpha.md
openclaw wiki compile
openclaw wiki lint
openclaw wiki search "alpha"
openclaw wiki get entity.alpha
openclaw wiki apply synthesis "Alpha Summary" --body "..." --source-id source.alpha
openclaw wiki bridge import
openclaw wiki obsidian status
```

See [CLI: wiki](https://docs.openclaw.ai/cli/wiki) for the full command reference.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#obsidian-support)  Obsidian support

When `vault.renderMode` is `obsidian`, the plugin writes Obsidian-friendly
Markdown and can optionally use the official `obsidian` CLI.Supported workflows include:

- status probing
- vault search
- opening a page
- invoking an Obsidian command
- jumping to the daily note

This is optional. The wiki still works in native mode without Obsidian.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#recommended-workflow)  Recommended workflow

1. Keep your active memory plugin for recall/promotion/dreaming.
2. Enable `memory-wiki`.
3. Start with `isolated` mode unless you explicitly want bridge mode.
4. Use `wiki_search` / `wiki_get` when provenance matters.
5. Use `wiki_apply` for narrow syntheses or metadata updates.
6. Run `wiki_lint` after meaningful changes.
7. Turn on dashboards if you want stale/contradiction visibility.

## [​](https://docs.openclaw.ai/plugins/memory-wiki\#related-docs)  Related docs

- [Memory Overview](https://docs.openclaw.ai/concepts/memory)
- [CLI: memory](https://docs.openclaw.ai/cli/memory)
- [CLI: wiki](https://docs.openclaw.ai/cli/wiki)
- [Plugin SDK overview](https://docs.openclaw.ai/plugins/sdk-overview)

[Voice call](https://docs.openclaw.ai/plugins/voice-call) [Memory LanceDB](https://docs.openclaw.ai/plugins/memory-lancedb)

Ctrl+I