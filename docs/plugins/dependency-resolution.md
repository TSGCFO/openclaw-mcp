---
source_url: https://docs.openclaw.ai/plugins/dependency-resolution
title: "Plugin dependency resolution - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/plugins/dependency-resolution#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Plugins

Plugin dependency resolution

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Plugin dependency resolution](https://docs.openclaw.ai/plugins/dependency-resolution#plugin-dependency-resolution)
- [Responsibility split](https://docs.openclaw.ai/plugins/dependency-resolution#responsibility-split)
- [Install roots](https://docs.openclaw.ai/plugins/dependency-resolution#install-roots)
- [Local plugins](https://docs.openclaw.ai/plugins/dependency-resolution#local-plugins)
- [Startup and reload](https://docs.openclaw.ai/plugins/dependency-resolution#startup-and-reload)
- [Bundled plugins](https://docs.openclaw.ai/plugins/dependency-resolution#bundled-plugins)
- [Legacy cleanup](https://docs.openclaw.ai/plugins/dependency-resolution#legacy-cleanup)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/plugins/dependency-resolution\#plugin-dependency-resolution)  Plugin dependency resolution

OpenClaw keeps plugin dependency work at install/update time. Runtime loading
does not run package managers, repair dependency trees, or mutate the OpenClaw
package directory.

## [​](https://docs.openclaw.ai/plugins/dependency-resolution\#responsibility-split)  Responsibility split

Plugin packages own their dependency graph:

- runtime dependencies live in the plugin package `dependencies` or
`optionalDependencies`
- SDK/core imports are peer or supplied OpenClaw imports
- local development plugins bring their own already-installed dependencies
- npm and git plugins are installed into OpenClaw-owned package roots

OpenClaw owns only the plugin lifecycle:

- discover the plugin source
- install or update the package when explicitly requested
- record the install metadata
- load the plugin entrypoint
- fail with an actionable error when dependencies are missing

## [​](https://docs.openclaw.ai/plugins/dependency-resolution\#install-roots)  Install roots

OpenClaw uses stable per-source roots:

- npm packages install under `~/.openclaw/npm`
- git packages clone under `~/.openclaw/git`
- local/path/archive installs are copied or referenced without dependency repair

npm installs run in the npm root with:

```
npm install --prefix ~/.openclaw/npm <spec> --omit=dev --ignore-scripts --no-audit --no-fund
```

npm may hoist transitive dependencies to `~/.openclaw/npm/node_modules` beside
the plugin package. OpenClaw scans the managed npm root before trusting the
install and uses npm to remove npm-managed packages during uninstall, so hoisted
runtime dependencies stay inside the managed cleanup boundary.git installs clone or refresh the repository, then run:

```
npm install --omit=dev --ignore-scripts --no-audit --no-fund
```

The installed plugin then loads from that package directory, so package-local
and parent `node_modules` resolution works the same way it does for a normal
Node package.

## [​](https://docs.openclaw.ai/plugins/dependency-resolution\#local-plugins)  Local plugins

Local plugins are treated as developer-controlled directories. OpenClaw does not
run `npm install`, `pnpm install`, or dependency repair for them. If a local
plugin has dependencies, install them in that plugin before loading it.Third-party TypeScript local plugins can use the emergency Jiti path. Packaged
JavaScript plugins and bundled internal plugins load through native
import/require instead of Jiti.

## [​](https://docs.openclaw.ai/plugins/dependency-resolution\#startup-and-reload)  Startup and reload

Gateway startup and config reload never install plugin dependencies. They read
the plugin install records, compute the entrypoint, and load it.If a dependency is missing at runtime, the plugin fails to load and the error
should point the operator to an explicit fix:

```
openclaw plugins update <id>
openclaw plugins install <source>
openclaw doctor --fix
```

`doctor --fix` can clean legacy OpenClaw-generated dependency state and install
configured downloadable plugins that are missing from the local install records.
It does not repair dependencies for an already-installed local plugin.

## [​](https://docs.openclaw.ai/plugins/dependency-resolution\#bundled-plugins)  Bundled plugins

Lightweight and core-critical bundled plugins are shipped as part of OpenClaw.
They should either have no heavy runtime dependency tree or be moved out to a
downloadable package on ClawHub/npm.For the current generated list of plugins that ship in the core package, install
externally, or stay source-only, see [Plugin inventory](https://docs.openclaw.ai/plugins/plugin-inventory).Bundled plugin manifests must not request dependency staging. Large or optional
plugin functionality should be packaged as a normal plugin and installed through
the same npm/git/ClawHub path as third-party plugins.In source checkouts, OpenClaw treats the repository as a pnpm monorepo. After
`pnpm install`, bundled plugins load from `extensions/<id>` so package-local
workspace dependencies are available and edits are picked up directly. Source
checkout development is pnpm-only; plain `npm install` at the repository root is
not a supported way to prepare bundled plugin dependencies.

| Install shape | Bundled plugin location | Dependency owner |
| --- | --- | --- |
| `npm install -g openclaw` | Built runtime tree inside the package | OpenClaw package and explicit plugin install/update/doctor flows |
| Git checkout plus `pnpm install` | `extensions/<id>` workspace packages | The pnpm workspace, including each plugin package’s own dependencies |
| `openclaw plugins install ...` | Managed npm/git/ClawHub plugin root | The plugin install/update flow |

## [​](https://docs.openclaw.ai/plugins/dependency-resolution\#legacy-cleanup)  Legacy cleanup

Older OpenClaw versions generated bundled-plugin dependency roots at startup or
during doctor repair. Current doctor cleanup removes those stale directories and
symlinks when `--fix` is used, including old `plugin-runtime-deps` roots,
`.openclaw-runtime-deps*` manifests, generated plugin `node_modules`, install
stage directories, and package-local pnpm stores.These paths are legacy debris only. New installs should not create them.

[Plugin bundles](https://docs.openclaw.ai/plugins/bundles) [Codex harness](https://docs.openclaw.ai/plugins/codex-harness)

Ctrl+I