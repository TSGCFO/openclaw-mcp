---
source_url: https://docs.openclaw.ai/cli/plugins
title: "Plugins - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/plugins#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Plugins and skills

Plugins

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Commands](https://docs.openclaw.ai/cli/plugins#commands)
- [Install](https://docs.openclaw.ai/cli/plugins#install)
- [Marketplace shorthand](https://docs.openclaw.ai/cli/plugins#marketplace-shorthand)
- [List](https://docs.openclaw.ai/cli/plugins#list)
- [Plugin index](https://docs.openclaw.ai/cli/plugins#plugin-index)
- [Uninstall](https://docs.openclaw.ai/cli/plugins#uninstall)
- [Update](https://docs.openclaw.ai/cli/plugins#update)
- [Inspect](https://docs.openclaw.ai/cli/plugins#inspect)
- [Doctor](https://docs.openclaw.ai/cli/plugins#doctor)
- [Registry](https://docs.openclaw.ai/cli/plugins#registry)
- [Marketplace](https://docs.openclaw.ai/cli/plugins#marketplace)
- [Related](https://docs.openclaw.ai/cli/plugins#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Manage Gateway plugins, hook packs, and compatible bundles.

[**Plugin system** \\
\\
End-user guide for installing, enabling, and troubleshooting plugins.](https://docs.openclaw.ai/tools/plugin)

[**Manage plugins** \\
\\
Quick examples for install, list, update, uninstall, and publishing.](https://docs.openclaw.ai/plugins/manage-plugins)

[**Plugin bundles** \\
\\
Bundle compatibility model.](https://docs.openclaw.ai/plugins/bundles)

[**Plugin manifest** \\
\\
Manifest fields and config schema.](https://docs.openclaw.ai/plugins/manifest)

[**Security** \\
\\
Security hardening for plugin installs.](https://docs.openclaw.ai/gateway/security)

## [​](https://docs.openclaw.ai/cli/plugins\#commands)  Commands

```
openclaw plugins list
openclaw plugins list --enabled
openclaw plugins list --verbose
openclaw plugins list --json
openclaw plugins search <query>
openclaw plugins search <query> --limit 20
openclaw plugins search <query> --json
openclaw plugins install <path-or-spec>
openclaw plugins inspect <id>
openclaw plugins inspect <id> --runtime
openclaw plugins inspect <id> --json
openclaw plugins inspect --all
openclaw plugins info <id>
openclaw plugins enable <id>
openclaw plugins disable <id>
openclaw plugins registry
openclaw plugins registry --refresh
openclaw plugins uninstall <id>
openclaw plugins doctor
openclaw plugins update <id-or-npm-spec>
openclaw plugins update --all
openclaw plugins marketplace list <marketplace>
openclaw plugins marketplace list <marketplace> --json
```

For slow install, inspect, uninstall, or registry-refresh investigation, run the
command with `OPENCLAW_PLUGIN_LIFECYCLE_TRACE=1`. The trace writes phase timings
to stderr and keeps JSON output parseable. See [Debugging](https://docs.openclaw.ai/help/debugging#plugin-lifecycle-trace).

Bundled plugins ship with OpenClaw. Some are enabled by default (for example bundled model providers, bundled speech providers, and the bundled browser plugin); others require `plugins enable`.Native OpenClaw plugins must ship `openclaw.plugin.json` with an inline JSON Schema (`configSchema`, even if empty). Compatible bundles use their own bundle manifests instead.`plugins list` shows `Format: openclaw` or `Format: bundle`. Verbose list/info output also shows the bundle subtype (`codex`, `claude`, or `cursor`) plus detected bundle capabilities.

### [​](https://docs.openclaw.ai/cli/plugins\#install)  Install

```
openclaw plugins search "calendar"                   # search ClawHub plugins
openclaw plugins install <package>                      # npm by default
openclaw plugins install clawhub:<package>              # ClawHub only
openclaw plugins install npm:<package>                  # npm only
openclaw plugins install git:github.com/<owner>/<repo>  # git repo
openclaw plugins install git:github.com/<owner>/<repo>@<ref>
openclaw plugins install <package> --force              # overwrite existing install
openclaw plugins install <package> --pin                # pin version
openclaw plugins install <package> --dangerously-force-unsafe-install
openclaw plugins install <path>                         # local path
openclaw plugins install <plugin>@<marketplace>         # marketplace
openclaw plugins install <plugin> --marketplace <name>  # marketplace (explicit)
openclaw plugins install <plugin> --marketplace https://github.com/<owner>/<repo>
```

Bare package names install from npm by default during the launch cutover. Use `clawhub:<package>` for ClawHub. Treat plugin installs like running code. Prefer pinned versions.

`plugins search` queries ClawHub for installable plugin packages and prints
install-ready package names. It searches code-plugin and bundle-plugin packages,
not skills. Use `openclaw skills search` for ClawHub skills.

ClawHub is the primary distribution and discovery surface for most plugins. Npm
remains a supported fallback and direct-install path. OpenClaw-owned
`@openclaw/*` plugin packages are published on npm again; see the current list
on [npmjs.com/org/openclaw](https://www.npmjs.com/org/openclaw) or the
[plugin inventory](https://docs.openclaw.ai/plugins/plugin-inventory). Stable installs use `latest`.
Beta-channel installs and updates prefer the npm `beta` dist-tag when that tag
is available, then fall back to `latest`.

Config includes and invalid-config recovery

If your `plugins` section is backed by a single-file `$include`, `plugins install/update/enable/disable/uninstall` write through to that included file and leave `openclaw.json` untouched. Root includes, include arrays, and includes with sibling overrides fail closed instead of flattening. See [Config includes](https://docs.openclaw.ai/gateway/configuration) for the supported shapes.If config is invalid during install, `plugins install` normally fails closed and tells you to run `openclaw doctor --fix` first. During Gateway startup, invalid config for one plugin is isolated to that plugin so other channels and plugins can keep running; `openclaw doctor --fix` can quarantine the invalid plugin entry. The only documented install-time exception is a narrow bundled-plugin recovery path for plugins that explicitly opt into `openclaw.install.allowInvalidConfigRecovery`.

--force and reinstall vs update

`--force` reuses the existing install target and overwrites an already-installed plugin or hook pack in place. Use it when you are intentionally reinstalling the same id from a new local path, archive, ClawHub package, or npm artifact. For routine upgrades of an already tracked npm plugin, prefer `openclaw plugins update <id-or-npm-spec>`.If you run `plugins install` for a plugin id that is already installed, OpenClaw stops and points you at `plugins update <id-or-npm-spec>` for a normal upgrade, or at `plugins install <package> --force` when you genuinely want to overwrite the current install from a different source.

--pin scope

`--pin` applies to npm installs only. It is not supported with `git:` installs; use an explicit git ref such as `git:github.com/acme/plugin@v1.2.3` when you want a pinned source. It is not supported with `--marketplace`, because marketplace installs persist marketplace source metadata instead of an npm spec.

--dangerously-force-unsafe-install

`--dangerously-force-unsafe-install` is a break-glass option for false positives in the built-in dangerous-code scanner. It allows the install to continue even when the built-in scanner reports `critical` findings, but it does **not** bypass plugin `before_install` hook policy blocks and does **not** bypass scan failures.This CLI flag applies to plugin install/update flows. Gateway-backed skill dependency installs use the matching `dangerouslyForceUnsafeInstall` request override, while `openclaw skills install` remains a separate ClawHub skill download/install flow.If a plugin you published on ClawHub is blocked by a registry scan, use the publisher steps in [ClawHub](https://docs.openclaw.ai/tools/clawhub).

Hook packs and npm specs

`plugins install` is also the install surface for hook packs that expose `openclaw.hooks` in `package.json`. Use `openclaw hooks` for filtered hook visibility and per-hook enablement, not package installation.Npm specs are **registry-only** (package name + optional **exact version** or **dist-tag**). Git/URL/file specs and semver ranges are rejected. Dependency installs run project-local with `--ignore-scripts` for safety, even when your shell has global npm install settings.Use `npm:<package>` when you want to make npm resolution explicit. Bare package specs also install directly from npm during the launch cutover.Bare specs and `@latest` stay on the stable track. If npm resolves either of those to a prerelease, OpenClaw stops and asks you to opt in explicitly with a prerelease tag such as `@beta`/`@rc` or an exact prerelease version such as `@1.2.3-beta.4`.If a bare install spec matches an official plugin id (for example `diffs`), OpenClaw installs the catalog entry directly. To install an npm package with the same name, use an explicit scoped spec (for example `@scope/diffs`).

Git repositories

Use `git:<repo>` to install directly from a git repository. Supported forms include `git:github.com/owner/repo`, `git:owner/repo`, full `https://`, `ssh://`, `git://`, `file://`, and `git@host:owner/repo.git` clone URLs. Add `@<ref>` or `#<ref>` to check out a branch, tag, or commit before install.Git installs clone into a temporary directory, check out the requested ref when present, then use the normal plugin directory installer. That means manifest validation, dangerous-code scanning, package-manager install work, and install records behave like npm installs. Recorded git installs include the source URL/ref plus the resolved commit so `openclaw plugins update` can re-resolve the source later.After installing from git, use `openclaw plugins inspect <id> --runtime --json` to verify runtime registrations such as gateway methods and CLI commands. If the plugin registered a CLI root with `api.registerCli`, execute that command directly through the OpenClaw root CLI, for example `openclaw demo-plugin ping`.

Archives

Supported archives: `.zip`, `.tgz`, `.tar.gz`, `.tar`. Native OpenClaw plugin archives must contain a valid `openclaw.plugin.json` at the extracted plugin root; archives that only contain `package.json` are rejected before OpenClaw writes install records.Claude marketplace installs are also supported.

ClawHub installs use an explicit `clawhub:<package>` locator:

```
openclaw plugins install clawhub:openclaw-codex-app-server
openclaw plugins install clawhub:openclaw-codex-app-server@1.2.3
```

Bare npm-safe plugin specs install from npm by default during the launch cutover:

```
openclaw plugins install openclaw-codex-app-server
```

Use `npm:` to make npm-only resolution explicit:

```
openclaw plugins install npm:openclaw-codex-app-server
openclaw plugins install npm:@scope/plugin-name@1.0.1
```

OpenClaw checks the advertised plugin API / minimum gateway compatibility before install. When the selected ClawHub version publishes a ClawPack artifact, OpenClaw downloads the versioned npm-pack `.tgz`, verifies the ClawHub digest header and the artifact digest, then installs it through the normal archive path. Older ClawHub versions without ClawPack metadata still install through the legacy package archive verification path. Recorded installs keep their ClawHub source metadata, artifact kind, npm integrity, npm shasum, tarball name, and ClawPack digest facts for later updates.
Unversioned ClawHub installs keep an unversioned recorded spec so `openclaw plugins update` can follow newer ClawHub releases; explicit version or tag selectors such as `clawhub:pkg@1.2.3` and `clawhub:pkg@beta` remain pinned to that selector.

#### [​](https://docs.openclaw.ai/cli/plugins\#marketplace-shorthand)  Marketplace shorthand

Use `plugin@marketplace` shorthand when the marketplace name exists in Claude’s local registry cache at `~/.claude/plugins/known_marketplaces.json`:

```
openclaw plugins marketplace list <marketplace-name>
openclaw plugins install <plugin-name>@<marketplace-name>
```

Use `--marketplace` when you want to pass the marketplace source explicitly:

```
openclaw plugins install <plugin-name> --marketplace <marketplace-name>
openclaw plugins install <plugin-name> --marketplace <owner/repo>
openclaw plugins install <plugin-name> --marketplace https://github.com/<owner>/<repo>
openclaw plugins install <plugin-name> --marketplace ./my-marketplace
```

- Marketplace sources

- Remote marketplace rules


- a Claude known-marketplace name from `~/.claude/plugins/known_marketplaces.json`
- a local marketplace root or `marketplace.json` path
- a GitHub repo shorthand such as `owner/repo`
- a GitHub repo URL such as `https://github.com/owner/repo`
- a git URL

For remote marketplaces loaded from GitHub or git, plugin entries must stay inside the cloned marketplace repo. OpenClaw accepts relative path sources from that repo and rejects HTTP(S), absolute-path, git, GitHub, and other non-path plugin sources from remote manifests.

For local paths and archives, OpenClaw auto-detects:

- native OpenClaw plugins (`openclaw.plugin.json`)
- Codex-compatible bundles (`.codex-plugin/plugin.json`)
- Claude-compatible bundles (`.claude-plugin/plugin.json` or the default Claude component layout)
- Cursor-compatible bundles (`.cursor-plugin/plugin.json`)

Compatible bundles install into the normal plugin root and participate in the same list/info/enable/disable flow. Today, bundle skills, Claude command-skills, Claude `settings.json` defaults, Claude `.lsp.json` / manifest-declared `lspServers` defaults, Cursor command-skills, and compatible Codex hook directories are supported; other detected bundle capabilities are shown in diagnostics/info but are not yet wired into runtime execution.

### [​](https://docs.openclaw.ai/cli/plugins\#list)  List

```
openclaw plugins list
openclaw plugins list --enabled
openclaw plugins list --verbose
openclaw plugins list --json
openclaw plugins search <query>
openclaw plugins search <query> --limit 20
openclaw plugins search <query> --json
```

[​](https://docs.openclaw.ai/cli/plugins#param-enabled)

--enabled

boolean

Show only enabled plugins.

[​](https://docs.openclaw.ai/cli/plugins#param-verbose)

--verbose

boolean

Switch from the table view to per-plugin detail lines with source/origin/version/activation metadata.

[​](https://docs.openclaw.ai/cli/plugins#param-json)

--json

boolean

Machine-readable inventory plus registry diagnostics and package dependency install state.

`plugins list` reads the persisted local plugin registry first, with a manifest-only derived fallback when the registry is missing or invalid. It is useful for checking whether a plugin is installed, enabled, and visible to cold startup planning, but it is not a live runtime probe of an already-running Gateway process. After changing plugin code, enablement, hook policy, or `plugins.load.paths`, restart the Gateway that serves the channel before expecting new `register(api)` code or hooks to run. For remote/container deployments, verify you are restarting the actual `openclaw gateway run` child, not only a wrapper process.`plugins list --json` includes each plugin’s `dependencyStatus` from `package.json``dependencies` and `optionalDependencies`. OpenClaw checks whether those package
names are present along the plugin’s normal Node `node_modules` lookup path; it
does not import plugin runtime code, run a package manager, or repair missing
dependencies.

`plugins search` is a remote ClawHub catalog lookup. It does not inspect local
state, mutate config, install packages, or load plugin runtime code. Search
results include the ClawHub package name, family, channel, version, summary, and
an install hint such as `openclaw plugins install clawhub:<package>`.For bundled plugin work inside a packaged Docker image, bind-mount the plugin
source directory over the matching packaged source path, such as
`/app/extensions/synology-chat`. OpenClaw will discover that mounted source
overlay before `/app/dist/extensions/synology-chat`; a plain copied source
directory remains inert so normal packaged installs still use compiled dist.For runtime hook debugging:

- `openclaw plugins inspect <id> --runtime --json` shows registered hooks and diagnostics from a module-loaded inspection pass. Runtime inspection never installs dependencies; use `openclaw doctor --fix` to clean legacy dependency state or install missing configured downloadable plugins.
- `openclaw gateway status --deep --require-rpc` confirms the reachable Gateway, service/process hints, config path, and RPC health.
- Non-bundled conversation hooks (`llm_input`, `llm_output`, `before_agent_finalize`, `agent_end`) require `plugins.entries.<id>.hooks.allowConversationAccess=true`.

Use `--link` to avoid copying a local directory (adds to `plugins.load.paths`):

```
openclaw plugins install -l ./my-plugin
```

`--force` is not supported with `--link` because linked installs reuse the source path instead of copying over a managed install target.Use `--pin` on npm installs to save the resolved exact spec (`name@version`) in the managed plugin index while keeping the default behavior unpinned.

### [​](https://docs.openclaw.ai/cli/plugins\#plugin-index)  Plugin index

Plugin install metadata is machine-managed state, not user config. Installs and updates write it to `plugins/installs.json` under the active OpenClaw state directory. Its top-level `installRecords` map is the durable source of install metadata, including records for broken or missing plugin manifests. The `plugins` array is the manifest-derived cold registry cache. The file includes a do-not-edit warning and is used by `openclaw plugins update`, uninstall, diagnostics, and the cold plugin registry.When OpenClaw sees shipped legacy `plugins.installs` records in config, it moves them into the plugin index and removes the config key; if either write fails, the config records are kept so the install metadata is not lost.

### [​](https://docs.openclaw.ai/cli/plugins\#uninstall)  Uninstall

```
openclaw plugins uninstall <id>
openclaw plugins uninstall <id> --dry-run
openclaw plugins uninstall <id> --keep-files
```

`uninstall` removes plugin records from `plugins.entries`, the persisted plugin index, plugin allow/deny list entries, and linked `plugins.load.paths` entries when applicable. Unless `--keep-files` is set, uninstall also removes the tracked managed install directory when it is inside OpenClaw’s plugin extensions root. For active memory plugins, the memory slot resets to `memory-core`.

`--keep-config` is supported as a deprecated alias for `--keep-files`.

### [​](https://docs.openclaw.ai/cli/plugins\#update)  Update

```
openclaw plugins update <id-or-npm-spec>
openclaw plugins update --all
openclaw plugins update <id-or-npm-spec> --dry-run
openclaw plugins update @openclaw/voice-call
openclaw plugins update openclaw-codex-app-server --dangerously-force-unsafe-install
```

Updates apply to tracked plugin installs in the managed plugin index and tracked hook-pack installs in `hooks.internal.installs`.

Resolving plugin id vs npm spec

When you pass a plugin id, OpenClaw reuses the recorded install spec for that plugin. That means previously stored dist-tags such as `@beta` and exact pinned versions continue to be used on later `update <id>` runs.For npm installs, you can also pass an explicit npm package spec with a dist-tag or exact version. OpenClaw resolves that package name back to the tracked plugin record, updates that installed plugin, and records the new npm spec for future id-based updates.Passing the npm package name without a version or tag also resolves back to the tracked plugin record. Use this when a plugin was pinned to an exact version and you want to move it back to the registry’s default release line.

Beta channel updates

`openclaw plugins update` reuses the tracked plugin spec unless you pass a new spec. `openclaw update` additionally knows the active OpenClaw update channel: on the beta channel, default-line npm and ClawHub plugin records try `@beta` first, then fall back to the recorded default/latest spec if no plugin beta release exists. Exact versions and explicit tags stay pinned to that selector.

Version checks and integrity drift

Before a live npm update, OpenClaw checks the installed package version against the npm registry metadata. If the installed version and recorded artifact identity already match the resolved target, the update is skipped without downloading, reinstalling, or rewriting `openclaw.json`.When a stored integrity hash exists and the fetched artifact hash changes, OpenClaw treats that as npm artifact drift. The interactive `openclaw plugins update` command prints the expected and actual hashes and asks for confirmation before proceeding. Non-interactive update helpers fail closed unless the caller supplies an explicit continuation policy.

--dangerously-force-unsafe-install on update

`--dangerously-force-unsafe-install` is also available on `plugins update` as a break-glass override for built-in dangerous-code scan false positives during plugin updates. It still does not bypass plugin `before_install` policy blocks or scan-failure blocking, and it only applies to plugin updates, not hook-pack updates.

### [​](https://docs.openclaw.ai/cli/plugins\#inspect)  Inspect

```
openclaw plugins inspect <id>
openclaw plugins inspect <id> --runtime
openclaw plugins inspect <id> --json
```

Inspect shows identity, load status, source, manifest capabilities, policy flags, diagnostics, install metadata, bundle capabilities, and any detected MCP or LSP server support without importing plugin runtime by default. Add `--runtime` to load the plugin module and include registered hooks, tools, commands, services, gateway methods, and HTTP routes. Runtime inspection reports missing plugin dependencies directly; installs and repairs stay in `openclaw plugins install`, `openclaw plugins update`, and `openclaw doctor --fix`.Plugin-owned CLI commands are installed as root `openclaw` command groups. After `inspect --runtime` shows a command under `cliCommands`, run it as `openclaw <command> ...`; for example a plugin that registers `demo-git` can be verified with `openclaw demo-git ping`.Each plugin is classified by what it actually registers at runtime:

- **plain-capability** — one capability type (e.g. a provider-only plugin)
- **hybrid-capability** — multiple capability types (e.g. text + speech + images)
- **hook-only** — only hooks, no capabilities or surfaces
- **non-capability** — tools/commands/services but no capabilities

See [Plugin shapes](https://docs.openclaw.ai/plugins/architecture#plugin-shapes) for more on the capability model.

The `--json` flag outputs a machine-readable report suitable for scripting and auditing. `inspect --all` renders a fleet-wide table with shape, capability kinds, compatibility notices, bundle capabilities, and hook summary columns. `info` is an alias for `inspect`.

### [​](https://docs.openclaw.ai/cli/plugins\#doctor)  Doctor

```
openclaw plugins doctor
```

`doctor` reports plugin load errors, manifest/discovery diagnostics, and compatibility notices. When everything is clean it prints `No plugin issues detected.`For module-shape failures such as missing `register`/`activate` exports, rerun with `OPENCLAW_PLUGIN_LOAD_DEBUG=1` to include a compact export-shape summary in the diagnostic output.

### [​](https://docs.openclaw.ai/cli/plugins\#registry)  Registry

```
openclaw plugins registry
openclaw plugins registry --refresh
openclaw plugins registry --json
```

The local plugin registry is OpenClaw’s persisted cold read model for installed plugin identity, enablement, source metadata, and contribution ownership. Normal startup, provider owner lookup, channel setup classification, and plugin inventory can read it without importing plugin runtime modules.Use `plugins registry` to inspect whether the persisted registry is present, current, or stale. Use `--refresh` to rebuild it from the persisted plugin index, config policy, and manifest/package metadata. This is a repair path, not a runtime activation path.

`OPENCLAW_DISABLE_PERSISTED_PLUGIN_REGISTRY=1` is a deprecated break-glass compatibility switch for registry read failures. Prefer `plugins registry --refresh` or `openclaw doctor --fix`; the env fallback is only for emergency startup recovery while the migration rolls out.

### [​](https://docs.openclaw.ai/cli/plugins\#marketplace)  Marketplace

```
openclaw plugins marketplace list <source>
openclaw plugins marketplace list <source> --json
```

Marketplace list accepts a local marketplace path, a `marketplace.json` path, a GitHub shorthand like `owner/repo`, a GitHub repo URL, or a git URL. `--json` prints the resolved source label plus the parsed marketplace manifest and plugin entries.

## [​](https://docs.openclaw.ai/cli/plugins\#related)  Related

- [Building plugins](https://docs.openclaw.ai/plugins/building-plugins)
- [CLI reference](https://docs.openclaw.ai/cli)
- [Community plugins](https://docs.openclaw.ai/plugins/community)

[Webhooks](https://docs.openclaw.ai/cli/webhooks) [Skills](https://docs.openclaw.ai/cli/skills)

Ctrl+I