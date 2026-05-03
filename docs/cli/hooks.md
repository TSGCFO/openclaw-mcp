---
source_url: https://docs.openclaw.ai/cli/hooks
title: "Hooks - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/hooks#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Agents and sessions

Hooks

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw hooks](https://docs.openclaw.ai/cli/hooks#openclaw-hooks)
- [List all hooks](https://docs.openclaw.ai/cli/hooks#list-all-hooks)
- [Get hook information](https://docs.openclaw.ai/cli/hooks#get-hook-information)
- [Check hooks eligibility](https://docs.openclaw.ai/cli/hooks#check-hooks-eligibility)
- [Enable a Hook](https://docs.openclaw.ai/cli/hooks#enable-a-hook)
- [Disable a Hook](https://docs.openclaw.ai/cli/hooks#disable-a-hook)
- [Notes](https://docs.openclaw.ai/cli/hooks#notes)
- [Install hook packs](https://docs.openclaw.ai/cli/hooks#install-hook-packs)
- [Update hook packs](https://docs.openclaw.ai/cli/hooks#update-hook-packs)
- [Bundled hooks](https://docs.openclaw.ai/cli/hooks#bundled-hooks)
- [session-memory](https://docs.openclaw.ai/cli/hooks#session-memory)
- [bootstrap-extra-files](https://docs.openclaw.ai/cli/hooks#bootstrap-extra-files)
- [command-logger](https://docs.openclaw.ai/cli/hooks#command-logger)
- [boot-md](https://docs.openclaw.ai/cli/hooks#boot-md)
- [Related](https://docs.openclaw.ai/cli/hooks#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/hooks\#openclaw-hooks)  `openclaw hooks`

Manage agent hooks (event-driven automations for commands like `/new`, `/reset`, and gateway startup).Running `openclaw hooks` with no subcommand is equivalent to `openclaw hooks list`.Related:

- Hooks: [Hooks](https://docs.openclaw.ai/automation/hooks)
- Plugin hooks: [Plugin hooks](https://docs.openclaw.ai/plugins/hooks)

## [​](https://docs.openclaw.ai/cli/hooks\#list-all-hooks)  List all hooks

```
openclaw hooks list
```

List all discovered hooks from workspace, managed, extra, and bundled directories.
Gateway startup does not load internal hook handlers until at least one internal hook is configured.**Options:**

- `--eligible`: Show only eligible hooks (requirements met)
- `--json`: Output as JSON
- `-v, --verbose`: Show detailed information including missing requirements

**Example output:**

```
Hooks (4/4 ready)

Ready:
  🚀 boot-md ✓ - Run BOOT.md on gateway startup
  📎 bootstrap-extra-files ✓ - Inject extra workspace bootstrap files during agent bootstrap
  📝 command-logger ✓ - Log all command events to a centralized audit file
  💾 session-memory ✓ - Save session context to memory when /new or /reset command is issued
```

**Example (verbose):**

```
openclaw hooks list --verbose
```

Shows missing requirements for ineligible hooks.**Example (JSON):**

```
openclaw hooks list --json
```

Returns structured JSON for programmatic use.

## [​](https://docs.openclaw.ai/cli/hooks\#get-hook-information)  Get hook information

```
openclaw hooks info <name>
```

Show detailed information about a specific hook.**Arguments:**

- `<name>`: Hook name or hook key (e.g., `session-memory`)

**Options:**

- `--json`: Output as JSON

**Example:**

```
openclaw hooks info session-memory
```

**Output:**

```
💾 session-memory ✓ Ready

Save session context to memory when /new or /reset command is issued

Details:
  Source: openclaw-bundled
  Path: /path/to/openclaw/hooks/bundled/session-memory/HOOK.md
  Handler: /path/to/openclaw/hooks/bundled/session-memory/handler.ts
  Homepage: https://docs.openclaw.ai/automation/hooks#session-memory
  Events: command:new, command:reset

Requirements:
  Config: ✓ workspace.dir
```

## [​](https://docs.openclaw.ai/cli/hooks\#check-hooks-eligibility)  Check hooks eligibility

```
openclaw hooks check
```

Show summary of hook eligibility status (how many are ready vs. not ready).**Options:**

- `--json`: Output as JSON

**Example output:**

```
Hooks Status

Total hooks: 4
Ready: 4
Not ready: 0
```

## [​](https://docs.openclaw.ai/cli/hooks\#enable-a-hook)  Enable a Hook

```
openclaw hooks enable <name>
```

Enable a specific hook by adding it to your config (`~/.openclaw/openclaw.json` by default).**Note:** Workspace hooks are disabled by default until enabled here or in config. Hooks managed by plugins show `plugin:<id>` in `openclaw hooks list` and can’t be enabled/disabled here. Enable/disable the plugin instead.**Arguments:**

- `<name>`: Hook name (e.g., `session-memory`)

**Example:**

```
openclaw hooks enable session-memory
```

**Output:**

```
✓ Enabled hook: 💾 session-memory
```

**What it does:**

- Checks if hook exists and is eligible
- Updates `hooks.internal.entries.<name>.enabled = true` in your config
- Saves config to disk

If the hook came from `<workspace>/hooks/`, this opt-in step is required before
the Gateway will load it.**After enabling:**

- Restart the gateway so hooks reload (menu bar app restart on macOS, or restart your gateway process in dev).

## [​](https://docs.openclaw.ai/cli/hooks\#disable-a-hook)  Disable a Hook

```
openclaw hooks disable <name>
```

Disable a specific hook by updating your config.**Arguments:**

- `<name>`: Hook name (e.g., `command-logger`)

**Example:**

```
openclaw hooks disable command-logger
```

**Output:**

```
⏸ Disabled hook: 📝 command-logger
```

**After disabling:**

- Restart the gateway so hooks reload

## [​](https://docs.openclaw.ai/cli/hooks\#notes)  Notes

- `openclaw hooks list --json`, `info --json`, and `check --json` write structured JSON directly to stdout.
- Plugin-managed hooks cannot be enabled or disabled here; enable or disable the owning plugin instead.

## [​](https://docs.openclaw.ai/cli/hooks\#install-hook-packs)  Install hook packs

```
openclaw plugins install <package>        # ClawHub first, then npm
openclaw plugins install npm:<package>    # npm only
openclaw plugins install <package> --pin  # pin version
openclaw plugins install <path>           # local path
```

Install hook packs through the unified plugins installer.`openclaw hooks install` still works as a compatibility alias, but it prints a
deprecation warning and forwards to `openclaw plugins install`.Npm specs are **registry-only** (package name + optional **exact version** or
**dist-tag**). Git/URL/file specs and semver ranges are rejected. Dependency
installs run project-local with `--ignore-scripts` for safety, even when your
shell has global npm install settings.Bare specs and `@latest` stay on the stable track. If npm resolves either of
those to a prerelease, OpenClaw stops and asks you to opt in explicitly with a
prerelease tag such as `@beta`/`@rc` or an exact prerelease version.**What it does:**

- Copies the hook pack into `~/.openclaw/hooks/<id>`
- Enables the installed hooks in `hooks.internal.entries.*`
- Records the install under `hooks.internal.installs`

**Options:**

- `-l, --link`: Link a local directory instead of copying (adds it to `hooks.internal.load.extraDirs`)
- `--pin`: Record npm installs as exact resolved `name@version` in `hooks.internal.installs`

**Supported archives:**`.zip`, `.tgz`, `.tar.gz`, `.tar`**Examples:**

```
# Local directory
openclaw plugins install ./my-hook-pack

# Local archive
openclaw plugins install ./my-hook-pack.zip

# NPM package
openclaw plugins install @openclaw/my-hook-pack

# Link a local directory without copying
openclaw plugins install -l ./my-hook-pack
```

Linked hook packs are treated as managed hooks from an operator-configured
directory, not as workspace hooks.

## [​](https://docs.openclaw.ai/cli/hooks\#update-hook-packs)  Update hook packs

```
openclaw plugins update <id>
openclaw plugins update --all
```

Update tracked npm-based hook packs through the unified plugins updater.`openclaw hooks update` still works as a compatibility alias, but it prints a
deprecation warning and forwards to `openclaw plugins update`.**Options:**

- `--all`: Update all tracked hook packs
- `--dry-run`: Show what would change without writing

When a stored integrity hash exists and the fetched artifact hash changes,
OpenClaw prints a warning and asks for confirmation before proceeding. Use
global `--yes` to bypass prompts in CI/non-interactive runs.

## [​](https://docs.openclaw.ai/cli/hooks\#bundled-hooks)  Bundled hooks

### [​](https://docs.openclaw.ai/cli/hooks\#session-memory)  session-memory

Saves session context to memory when you issue `/new` or `/reset`.**Enable:**

```
openclaw hooks enable session-memory
```

**Output:**`~/.openclaw/workspace/memory/YYYY-MM-DD-slug.md`**See:** [session-memory documentation](https://docs.openclaw.ai/automation/hooks#session-memory)

### [​](https://docs.openclaw.ai/cli/hooks\#bootstrap-extra-files)  bootstrap-extra-files

Injects additional bootstrap files (for example monorepo-local `AGENTS.md` / `TOOLS.md`) during `agent:bootstrap`.**Enable:**

```
openclaw hooks enable bootstrap-extra-files
```

**See:** [bootstrap-extra-files documentation](https://docs.openclaw.ai/automation/hooks#bootstrap-extra-files)

### [​](https://docs.openclaw.ai/cli/hooks\#command-logger)  command-logger

Logs all command events to a centralized audit file.**Enable:**

```
openclaw hooks enable command-logger
```

**Output:**`~/.openclaw/logs/commands.log`**View logs:**

```
# Recent commands
tail -n 20 ~/.openclaw/logs/commands.log

# Pretty-print
cat ~/.openclaw/logs/commands.log | jq .

# Filter by action
grep '"action":"new"' ~/.openclaw/logs/commands.log | jq .
```

**See:** [command-logger documentation](https://docs.openclaw.ai/automation/hooks#command-logger)

### [​](https://docs.openclaw.ai/cli/hooks\#boot-md)  boot-md

Runs `BOOT.md` when the gateway starts (after channels start).**Events**: `gateway:startup`**Enable**:

```
openclaw hooks enable boot-md
```

**See:** [boot-md documentation](https://docs.openclaw.ai/automation/hooks#boot-md)

## [​](https://docs.openclaw.ai/cli/hooks\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Automation hooks](https://docs.openclaw.ai/automation/hooks)

[Agents](https://docs.openclaw.ai/cli/agents) [Inference CLI](https://docs.openclaw.ai/cli/infer)

Ctrl+I