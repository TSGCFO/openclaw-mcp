---
source_url: https://docs.openclaw.ai/tools/skills#per-agent-vs-shared-skills
title: "Skills - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/skills#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Skills

Skills

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Locations and precedence](https://docs.openclaw.ai/tools/skills#locations-and-precedence)
- [Per-agent vs shared skills](https://docs.openclaw.ai/tools/skills#per-agent-vs-shared-skills)
- [Agent skill allowlists](https://docs.openclaw.ai/tools/skills#agent-skill-allowlists)
- [Plugins and skills](https://docs.openclaw.ai/tools/skills#plugins-and-skills)
- [Skill Workshop](https://docs.openclaw.ai/tools/skills#skill-workshop)
- [ClawHub (install and sync)](https://docs.openclaw.ai/tools/skills#clawhub-install-and-sync)
- [Security](https://docs.openclaw.ai/tools/skills#security)
- [SKILL.md format](https://docs.openclaw.ai/tools/skills#skill-md-format)
- [Optional frontmatter keys](https://docs.openclaw.ai/tools/skills#optional-frontmatter-keys)
- [Gating (load-time filters)](https://docs.openclaw.ai/tools/skills#gating-load-time-filters)
- [Sandboxing notes](https://docs.openclaw.ai/tools/skills#sandboxing-notes)
- [Installer specs](https://docs.openclaw.ai/tools/skills#installer-specs)
- [Config overrides](https://docs.openclaw.ai/tools/skills#config-overrides)
- [Environment injection](https://docs.openclaw.ai/tools/skills#environment-injection)
- [Snapshots and refresh](https://docs.openclaw.ai/tools/skills#snapshots-and-refresh)
- [Skills watcher](https://docs.openclaw.ai/tools/skills#skills-watcher)
- [Remote macOS nodes (Linux gateway)](https://docs.openclaw.ai/tools/skills#remote-macos-nodes-linux-gateway)
- [Token impact](https://docs.openclaw.ai/tools/skills#token-impact)
- [Managed skills lifecycle](https://docs.openclaw.ai/tools/skills#managed-skills-lifecycle)
- [Looking for more skills?](https://docs.openclaw.ai/tools/skills#looking-for-more-skills)
- [Related](https://docs.openclaw.ai/tools/skills#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw uses **[AgentSkills](https://agentskills.io/)-compatible** skill
folders to teach the agent how to use tools. Each skill is a directory
containing a `SKILL.md` with YAML frontmatter and instructions. OpenClaw
loads bundled skills plus optional local overrides, and filters them at
load time based on environment, config, and binary presence.

## [​](https://docs.openclaw.ai/tools/skills\#locations-and-precedence)  Locations and precedence

OpenClaw loads skills from these sources, **highest precedence first**:

| # | Source | Path |
| --- | --- | --- |
| 1 | Workspace skills | `<workspace>/skills` |
| 2 | Project agent skills | `<workspace>/.agents/skills` |
| 3 | Personal agent skills | `~/.agents/skills` |
| 4 | Managed/local skills | `~/.openclaw/skills` |
| 5 | Bundled skills | shipped with the install |
| 6 | Extra skill folders | `skills.load.extraDirs` (config) |

If a skill name conflicts, the highest source wins.Codex CLI’s native `$CODEX_HOME/skills` directory is not one of these OpenClaw
skill roots. In Codex harness mode, local app-server launches use isolated
per-agent Codex homes, so personal Codex CLI skills are not loaded implicitly.
Use `openclaw migrate codex --dry-run` to inventory them and
`openclaw migrate codex` to choose skill directories with an interactive
checkbox prompt before copying them into the current OpenClaw agent workspace.
For non-interactive runs, repeat `--skill <name>` for the exact skills to copy.

## [​](https://docs.openclaw.ai/tools/skills\#per-agent-vs-shared-skills)  Per-agent vs shared skills

In **multi-agent** setups each agent has its own workspace:

| Scope | Path | Visible to |
| --- | --- | --- |
| Per-agent | `<workspace>/skills` | Only that agent |
| Project-agent | `<workspace>/.agents/skills` | Only that workspace’s agent |
| Personal-agent | `~/.agents/skills` | All agents on that machine |
| Shared managed/local | `~/.openclaw/skills` | All agents on that machine |
| Shared extra dirs | `skills.load.extraDirs` (lowest precedence) | All agents on that machine |

Same name in multiple places → highest source wins. Workspace beats
project-agent, beats personal-agent, beats managed/local, beats bundled,
beats extra dirs.

## [​](https://docs.openclaw.ai/tools/skills\#agent-skill-allowlists)  Agent skill allowlists

Skill **location** and skill **visibility** are separate controls.
Location/precedence decides which copy of a same-named skill wins; agent
allowlists decide which skills an agent can actually use.

```
{
  agents: {
    defaults: {
      skills: ["github", "weather"],
    },
    list: [\
      { id: "writer" }, // inherits github, weather\
      { id: "docs", skills: ["docs-search"] }, // replaces defaults\
      { id: "locked-down", skills: [] }, // no skills\
    ],
  },
}
```

Allowlist rules

- Omit `agents.defaults.skills` for unrestricted skills by default.
- Omit `agents.list[].skills` to inherit `agents.defaults.skills`.
- Set `agents.list[].skills: []` for no skills.
- A non-empty `agents.list[].skills` list is the **final** set for that
agent — it does not merge with defaults.
- The effective allowlist applies across prompt building, skill
slash-command discovery, sandbox sync, and skill snapshots.

## [​](https://docs.openclaw.ai/tools/skills\#plugins-and-skills)  Plugins and skills

Plugins can ship their own skills by listing `skills` directories in
`openclaw.plugin.json` (paths relative to the plugin root). Plugin skills
load when the plugin is enabled. This is the right place for tool-specific
operating guides that are too long for the tool description but should be
available whenever the plugin is installed — for example, the browser
plugin ships a `browser-automation` skill for multi-step browser control.Plugin skill directories are merged into the same low-precedence path as
`skills.load.extraDirs`, so a same-named bundled, managed, agent, or
workspace skill overrides them. You can gate them via
`metadata.openclaw.requires.config` on the plugin’s config entry.See [Plugins](https://docs.openclaw.ai/tools/plugin) for discovery/config and [Tools](https://docs.openclaw.ai/tools) for
the tool surface those skills teach.

## [​](https://docs.openclaw.ai/tools/skills\#skill-workshop)  Skill Workshop

The optional, experimental **Skill Workshop** plugin can create or update
workspace skills from reusable procedures observed during agent work. It
is disabled by default and must be explicitly enabled via
`plugins.entries.skill-workshop`.Skill Workshop writes only to `<workspace>/skills`, scans generated
content, supports pending approval or automatic safe writes, quarantines
unsafe proposals, and refreshes the skill snapshot after successful
writes so new skills become available without a Gateway restart.Use it for corrections such as _“next time, verify GIF attribution”_ or
hard-won workflows such as media QA checklists. Start with pending
approval; use automatic writes only in trusted workspaces after reviewing
its proposals. Full guide: [Skill Workshop plugin](https://docs.openclaw.ai/plugins/skill-workshop).

## [​](https://docs.openclaw.ai/tools/skills\#clawhub-install-and-sync)  ClawHub (install and sync)

[ClawHub](https://clawhub.ai/) is the public skills registry for OpenClaw.
Use native `openclaw skills` commands for discover/install/update, or the
separate `clawhub` CLI for publish/sync workflows. Full guide:
[ClawHub](https://docs.openclaw.ai/tools/clawhub).

| Action | Command |
| --- | --- |
| Install a skill into the workspace | `openclaw skills install <skill-slug>` |
| Update all installed skills | `openclaw skills update --all` |
| Sync (scan + publish updates) | `clawhub sync --all` |

Native `openclaw skills install` installs into the active workspace
`skills/` directory. The separate `clawhub` CLI also installs into
`./skills` under your current working directory (or falls back to the
configured OpenClaw workspace). OpenClaw picks that up as
`<workspace>/skills` on the next session.
Configured skill roots also support one grouping level, such as
`skills/<group>/<skill>/SKILL.md`, so related third-party skills can be
kept under a shared folder without broad recursive scanning.ClawHub skill pages expose the latest security scan state before install,
with scanner detail pages for VirusTotal, ClawScan, and static analysis.
`openclaw skills install <slug>` remains only the install path; publishers
recover false positives through the ClawHub dashboard or
`clawhub skill rescan <slug>`.

## [​](https://docs.openclaw.ai/tools/skills\#security)  Security

Treat third-party skills as **untrusted code**. Read them before enabling.
Prefer sandboxed runs for untrusted inputs and risky tools. See
[Sandboxing](https://docs.openclaw.ai/gateway/sandboxing) for the agent-side controls.

- Workspace and extra-dir skill discovery only accepts skill roots and `SKILL.md` files whose resolved realpath stays inside the configured root.
- Gateway-backed skill dependency installs (`skills.install`, onboarding, and the Skills settings UI) run the built-in dangerous-code scanner before executing installer metadata. `critical` findings block by default unless the caller explicitly sets the dangerous override; suspicious findings still warn only.
- `openclaw skills install <slug>` is different — it downloads a ClawHub skill folder into the workspace and does not use the installer-metadata path above.
- `skills.entries.*.env` and `skills.entries.*.apiKey` inject secrets into the **host** process for that agent turn (not the sandbox). Keep secrets out of prompts and logs.

For a broader threat model and checklists, see [Security](https://docs.openclaw.ai/gateway/security).

## [​](https://docs.openclaw.ai/tools/skills\#skill-md-format)  SKILL.md format

`SKILL.md` must include at least:

```
---
name: image-lab
description: Generate or edit images via a provider-backed image workflow
---
```

OpenClaw follows the AgentSkills spec for layout/intent. The parser used
by the embedded agent supports **single-line** frontmatter keys only;
`metadata` should be a **single-line JSON object**. Use `{baseDir}` in
instructions to reference the skill folder path.

### [​](https://docs.openclaw.ai/tools/skills\#optional-frontmatter-keys)  Optional frontmatter keys

[​](https://docs.openclaw.ai/tools/skills#param-homepage)

homepage

string

URL surfaced as “Website” in the macOS Skills UI. Also supported via `metadata.openclaw.homepage`.

[​](https://docs.openclaw.ai/tools/skills#param-user-invocable)

user-invocable

boolean

default:"true"

When `true`, the skill is exposed as a user slash command.

[​](https://docs.openclaw.ai/tools/skills#param-disable-model-invocation)

disable-model-invocation

boolean

default:"false"

When `true`, OpenClaw keeps the skill’s instructions out of the agent’s normal
prompt. The skill is still installed and can still be run explicitly as a
slash command when `user-invocable` is also `true`.

[​](https://docs.openclaw.ai/tools/skills#param-command-dispatch)

command-dispatch

"tool"

When set to `tool`, the slash command bypasses the model and dispatches directly to a tool.

[​](https://docs.openclaw.ai/tools/skills#param-command-tool)

command-tool

string

Tool name to invoke when `command-dispatch: tool` is set.

[​](https://docs.openclaw.ai/tools/skills#param-command-arg-mode)

command-arg-mode

"raw"

default:"raw"

For tool dispatch, forwards the raw args string to the tool (no core parsing). The tool is invoked with `{ command: "<raw args>", commandName: "<slash command>", skillName: "<skill name>" }`.

## [​](https://docs.openclaw.ai/tools/skills\#gating-load-time-filters)  Gating (load-time filters)

OpenClaw filters skills at load time using `metadata` (single-line JSON):

```
---
name: image-lab
description: Generate or edit images via a provider-backed image workflow
metadata:
  {
    "openclaw":
      {
        "requires": { "bins": ["uv"], "env": ["GEMINI_API_KEY"], "config": ["browser.enabled"] },
        "primaryEnv": "GEMINI_API_KEY",
      },
  }
---
```

Fields under `metadata.openclaw`:

[​](https://docs.openclaw.ai/tools/skills#param-always)

always

boolean

When `true`, always include the skill (skip other gates).

[​](https://docs.openclaw.ai/tools/skills#param-emoji)

emoji

string

Optional emoji used by the macOS Skills UI.

[​](https://docs.openclaw.ai/tools/skills#param-homepage-1)

homepage

string

Optional URL shown as “Website” in the macOS Skills UI.

[​](https://docs.openclaw.ai/tools/skills#param-os)

os

"darwin" \| "linux" \| "win32"

Optional list of platforms. If set, the skill is only eligible on those OSes.

[​](https://docs.openclaw.ai/tools/skills#param-requires-bins)

requires.bins

string\[\]

Each must exist on `PATH`.

[​](https://docs.openclaw.ai/tools/skills#param-requires-any-bins)

requires.anyBins

string\[\]

At least one must exist on `PATH`.

[​](https://docs.openclaw.ai/tools/skills#param-requires-env)

requires.env

string\[\]

Env var must exist or be provided in config.

[​](https://docs.openclaw.ai/tools/skills#param-requires-config)

requires.config

string\[\]

List of `openclaw.json` paths that must be truthy.

[​](https://docs.openclaw.ai/tools/skills#param-primary-env)

primaryEnv

string

Env var name associated with `skills.entries.<name>.apiKey`.

[​](https://docs.openclaw.ai/tools/skills#param-install)

install

object\[\]

Optional installer specs used by the macOS Skills UI (brew/node/go/uv/download).

If no `metadata.openclaw` is present, the skill is always eligible (unless
disabled in config or blocked by `skills.allowBundled` for bundled skills).

Legacy `metadata.clawdbot` blocks are still accepted when
`metadata.openclaw` is absent, so older installed skills keep their
dependency gates and installer hints. New and updated skills should use
`metadata.openclaw`.

### [​](https://docs.openclaw.ai/tools/skills\#sandboxing-notes)  Sandboxing notes

- `requires.bins` is checked on the **host** at skill load time.
- If an agent is sandboxed, the binary must also exist **inside the container**. Install it via `agents.defaults.sandbox.docker.setupCommand` (or a custom image). `setupCommand` runs once after the container is created. Package installs also require network egress, a writable root FS, and a root user in the sandbox.
- Example: the `summarize` skill (`skills/summarize/SKILL.md`) needs the `summarize` CLI in the sandbox container to run there.

### [​](https://docs.openclaw.ai/tools/skills\#installer-specs)  Installer specs

```
---
name: gemini
description: Use Gemini CLI for coding assistance and Google search lookups.
metadata:
  {
    "openclaw":
      {
        "emoji": "♊️",
        "requires": { "bins": ["gemini"] },
        "install":
          [\
            {\
              "id": "brew",\
              "kind": "brew",\
              "formula": "gemini-cli",\
              "bins": ["gemini"],\
              "label": "Install Gemini CLI (brew)",\
            },\
          ],
      },
  }
---
```

Installer selection rules

- If multiple installers are listed, the gateway picks a single preferred option (brew when available, otherwise node).
- If all installers are `download`, OpenClaw lists each entry so you can see the available artifacts.
- Installer specs can include `os: ["darwin"|"linux"|"win32"]` to filter options by platform.
- Node installs honor `skills.install.nodeManager` in `openclaw.json` (default: npm; options: npm/pnpm/yarn/bun). This only affects skill installs; the Gateway runtime should still be Node — Bun is not recommended for WhatsApp/Telegram.
- Gateway-backed installer selection is preference-driven: when install specs mix kinds, OpenClaw prefers Homebrew when `skills.install.preferBrew` is enabled and `brew` exists, then `uv`, then the configured node manager, then other fallbacks like `go` or `download`.
- If every install spec is `download`, OpenClaw surfaces all download options instead of collapsing to one preferred installer.

Per-installer details

- **Go installs:** if `go` is missing and `brew` is available, the gateway installs Go via Homebrew first and sets `GOBIN` to Homebrew’s `bin` when possible.
- **Download installs:**`url` (required), `archive` (`tar.gz` \| `tar.bz2` \| `zip`), `extract` (default: auto when archive detected), `stripComponents`, `targetDir` (default: `~/.openclaw/tools/<skillKey>`).

## [​](https://docs.openclaw.ai/tools/skills\#config-overrides)  Config overrides

Bundled and managed skills can be toggled and supplied with env values
under `skills.entries` in `~/.openclaw/openclaw.json`:

```
{
  skills: {
    entries: {
      "image-lab": {
        enabled: true,
        apiKey: { source: "env", provider: "default", id: "GEMINI_API_KEY" }, // or plaintext string
        env: {
          GEMINI_API_KEY: "GEMINI_KEY_HERE",
        },
        config: {
          endpoint: "https://example.invalid",
          model: "nano-pro",
        },
      },
      peekaboo: { enabled: true },
      sag: { enabled: false },
    },
  },
}
```

[​](https://docs.openclaw.ai/tools/skills#param-enabled)

enabled

boolean

`false` disables the skill even if it is bundled or installed.
The bundled `coding-agent` skill is opt-in: set
`skills.entries.coding-agent.enabled: true` before exposing it to agents,
then make sure one of `claude`, `codex`, `opencode`, or `pi` is installed and
authenticated for its own CLI.

[​](https://docs.openclaw.ai/tools/skills#param-api-key)

apiKey

string \| { source, provider, id }

Convenience for skills that declare `metadata.openclaw.primaryEnv`. Supports plaintext or SecretRef.

[​](https://docs.openclaw.ai/tools/skills#param-env)

env

Record<string, string>

Injected only if the variable is not already set in the process.

[​](https://docs.openclaw.ai/tools/skills#param-config)

config

object

Optional bag for custom per-skill fields. Custom keys must live here.

[​](https://docs.openclaw.ai/tools/skills#param-allow-bundled)

allowBundled

string\[\]

Optional allowlist for **bundled** skills only. If set, only bundled skills in the list are eligible (managed/workspace skills unaffected).

If the skill name contains hyphens, quote the key (JSON5 allows quoted
keys). Config keys match the **skill name** by default — if a skill
defines `metadata.openclaw.skillKey`, use that key under `skills.entries`.

For stock image generation/editing inside OpenClaw, use the core
`image_generate` tool with `agents.defaults.imageGenerationModel` instead
of a bundled skill. Skill examples here are for custom or third-party
workflows. For native image analysis use the `image` tool with
`agents.defaults.imageModel`. If you pick `openai/*`, `google/*`,
`fal/*`, or another provider-specific image model, add that provider’s
auth/API key too.

## [​](https://docs.openclaw.ai/tools/skills\#environment-injection)  Environment injection

When an agent run starts, OpenClaw:

1. Reads skill metadata.
2. Applies `skills.entries.<key>.env` and `skills.entries.<key>.apiKey` to `process.env`.
3. Builds the system prompt with **eligible** skills.
4. Restores the original environment after the run ends.

Environment injection is **scoped to the agent run**, not a global shell
environment.For the bundled `claude-cli` backend, OpenClaw also materializes the same
eligible snapshot as a temporary Claude Code plugin and passes it with
`--plugin-dir`. Claude Code can then use its native skill resolver while
OpenClaw still owns precedence, per-agent allowlists, gating, and
`skills.entries.*` env/API key injection. Other CLI backends use the
prompt catalog only.

## [​](https://docs.openclaw.ai/tools/skills\#snapshots-and-refresh)  Snapshots and refresh

OpenClaw snapshots the eligible skills **when a session starts** and
reuses that list for subsequent turns in the same session. Changes to
skills or config take effect on the next new session.Skills can refresh mid-session in two cases:

- The skills watcher is enabled.
- A new eligible remote node appears.

Think of this as a **hot reload**: the refreshed list is picked up on the
next agent turn. If the effective agent skill allowlist changes for that
session, OpenClaw refreshes the snapshot so visible skills stay aligned
with the current agent.

### [​](https://docs.openclaw.ai/tools/skills\#skills-watcher)  Skills watcher

By default, OpenClaw watches skill folders and bumps the skills snapshot
when `SKILL.md` files change. Configure under `skills.load`:

```
{
  skills: {
    load: {
      watch: true,
      watchDebounceMs: 250,
    },
  },
}
```

### [​](https://docs.openclaw.ai/tools/skills\#remote-macos-nodes-linux-gateway)  Remote macOS nodes (Linux gateway)

If the Gateway runs on Linux but a **macOS node** is connected with
`system.run` allowed (Exec approvals security not set to `deny`),
OpenClaw can treat macOS-only skills as eligible when the required
binaries are present on that node. The agent should execute those skills
via the `exec` tool with `host=node`.This relies on the node reporting its command support and on a bin probe
via `system.which` or `system.run`. Offline nodes do **not** make
remote-only skills visible. If a connected node stops answering bin
probes, OpenClaw clears its cached bin matches so agents no longer see
skills that cannot currently run there.

## [​](https://docs.openclaw.ai/tools/skills\#token-impact)  Token impact

When skills are eligible, OpenClaw injects a compact XML list of available
skills into the system prompt (via `formatSkillsForPrompt` in
`pi-coding-agent`). The cost is deterministic:

- **Base overhead** (only when ≥1 skill): 195 characters.
- **Per skill:** 97 characters + the length of the XML-escaped `<name>`, `<description>`, and `<location>` values.

Formula (characters):

```
total = 195 + Σ (97 + len(name_escaped) + len(description_escaped) + len(location_escaped))
```

XML escaping expands `& < > " '` into entities (`&amp;`, `&lt;`, etc.),
increasing length. Token counts vary by model tokenizer. A rough
OpenAI-style estimate is ~4 chars/token, so **97 chars ≈ 24 tokens** per
skill plus your actual field lengths.

## [​](https://docs.openclaw.ai/tools/skills\#managed-skills-lifecycle)  Managed skills lifecycle

OpenClaw ships a baseline set of skills as **bundled skills** with the
install (npm package or OpenClaw.app). `~/.openclaw/skills` exists for
local overrides — for example, pinning or patching a skill without
changing the bundled copy. Workspace skills are user-owned and override
both on name conflicts.

## [​](https://docs.openclaw.ai/tools/skills\#looking-for-more-skills)  Looking for more skills?

Browse [https://clawhub.ai](https://clawhub.ai/). Full configuration
schema: [Skills config](https://docs.openclaw.ai/tools/skills-config).

## [​](https://docs.openclaw.ai/tools/skills\#related)  Related

- [ClawHub](https://docs.openclaw.ai/tools/clawhub) — public skills registry
- [Creating skills](https://docs.openclaw.ai/tools/creating-skills) — building custom skills
- [Plugins](https://docs.openclaw.ai/tools/plugin) — plugin system overview
- [Skill Workshop plugin](https://docs.openclaw.ai/plugins/skill-workshop) — generate skills from agent work
- [Skills config](https://docs.openclaw.ai/tools/skills-config) — skill configuration reference
- [Slash commands](https://docs.openclaw.ai/tools/slash-commands) — all available slash commands

[Plugin architecture internals](https://docs.openclaw.ai/plugins/architecture-internals) [Creating skills](https://docs.openclaw.ai/tools/creating-skills)

Ctrl+I