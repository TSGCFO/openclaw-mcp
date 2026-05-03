---
source_url: https://docs.openclaw.ai/plugins/manage-plugins
title: "Manage plugins - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/plugins/manage-plugins#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Plugins

Manage plugins

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [List plugins](https://docs.openclaw.ai/plugins/manage-plugins#list-plugins)
- [Install plugins](https://docs.openclaw.ai/plugins/manage-plugins#install-plugins)
- [Update plugins](https://docs.openclaw.ai/plugins/manage-plugins#update-plugins)
- [Uninstall plugins](https://docs.openclaw.ai/plugins/manage-plugins#uninstall-plugins)
- [Publish plugins](https://docs.openclaw.ai/plugins/manage-plugins#publish-plugins)
- [Publish to ClawHub](https://docs.openclaw.ai/plugins/manage-plugins#publish-to-clawhub)
- [Publish to npmjs.com](https://docs.openclaw.ai/plugins/manage-plugins#publish-to-npmjs-com)
- [Source choice](https://docs.openclaw.ai/plugins/manage-plugins#source-choice)
- [Related](https://docs.openclaw.ai/plugins/manage-plugins#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Most plugin workflows are a few commands: search, install, restart the Gateway,
verify, and uninstall when you no longer need the plugin.

## [​](https://docs.openclaw.ai/plugins/manage-plugins\#list-plugins)  List plugins

```
openclaw plugins list
openclaw plugins list --enabled
openclaw plugins list --verbose
openclaw plugins list --json
```

Use `--json` for scripts. It includes registry diagnostics and each plugin’s
static `dependencyStatus` when the plugin package declares `dependencies` or
`optionalDependencies`.

```
openclaw plugins list --json \
  | jq '.plugins[] | {id, enabled, format, source, dependencyStatus}'
```

`plugins list` is a cold inventory check. It shows what OpenClaw can discover
from config, manifests, and the plugin registry; it does not prove that an
already-running Gateway process imported the plugin runtime.

## [​](https://docs.openclaw.ai/plugins/manage-plugins\#install-plugins)  Install plugins

```
# Search ClawHub for plugin packages.
openclaw plugins search "calendar"

# Bare package specs try ClawHub first, then npm fallback.
openclaw plugins install <package>

# Force one source.
openclaw plugins install clawhub:<package>
openclaw plugins install npm:<package>

# Install a specific version or dist-tag.
openclaw plugins install clawhub:<package>@1.2.3
openclaw plugins install clawhub:<package>@beta
openclaw plugins install npm:@scope/openclaw-plugin@1.2.3
openclaw plugins install npm:@openclaw/codex

# Install from git or a local development checkout.
openclaw plugins install git:github.com/acme/openclaw-plugin@v1.0.0
openclaw plugins install ./my-plugin
openclaw plugins install --link ./my-plugin
```

After installing plugin code, restart the Gateway that serves your channels:

```
openclaw gateway restart
openclaw plugins inspect <plugin-id> --runtime --json
```

Use `inspect --runtime` when you need proof that the plugin registered runtime
surfaces such as tools, hooks, services, Gateway methods, or plugin-owned CLI
commands.

## [​](https://docs.openclaw.ai/plugins/manage-plugins\#update-plugins)  Update plugins

```
openclaw plugins update <plugin-id>
openclaw plugins update <npm-package-or-spec>
openclaw plugins update --all
```

If a plugin was installed from an npm dist-tag such as `@beta`, later
`update <plugin-id>` calls reuse that recorded tag. Passing an explicit npm spec
switches the tracked install to that spec for future updates.

```
openclaw plugins update @scope/openclaw-plugin@beta
openclaw plugins update @scope/openclaw-plugin
```

The second command moves a plugin back to the registry’s default release line
when it was previously pinned to an exact version or tag.When `openclaw update` runs on the beta channel, default-line npm and ClawHub
plugin records try the matching plugin `@beta` release first. If that beta
release does not exist, OpenClaw falls back to the recorded default/latest spec.
Exact versions and explicit tags such as `@rc` or `@beta` are preserved.

## [​](https://docs.openclaw.ai/plugins/manage-plugins\#uninstall-plugins)  Uninstall plugins

```
openclaw plugins uninstall <plugin-id> --dry-run
openclaw plugins uninstall <plugin-id>
openclaw plugins uninstall <plugin-id> --keep-files
openclaw gateway restart
```

Uninstall removes the plugin’s config entry, plugin index record, allow/deny list
entries, and linked load paths when applicable. Managed install directories are
removed unless you pass `--keep-files`.

## [​](https://docs.openclaw.ai/plugins/manage-plugins\#publish-plugins)  Publish plugins

You can publish external plugins to [ClawHub](https://clawhub.ai/), npmjs.com, or
both.

### [​](https://docs.openclaw.ai/plugins/manage-plugins\#publish-to-clawhub)  Publish to ClawHub

ClawHub is the primary public discovery surface for OpenClaw plugins. It gives
users searchable metadata, version history, and registry scan results before
install.

```
npm i -g clawhub
clawhub login
clawhub package publish your-org/your-plugin --dry-run
clawhub package publish your-org/your-plugin
clawhub package publish your-org/your-plugin@v1.0.0
```

Users install from ClawHub with:

```
openclaw plugins install clawhub:<package>
openclaw plugins install <package>
```

The bare form still checks ClawHub first.

### [​](https://docs.openclaw.ai/plugins/manage-plugins\#publish-to-npmjs-com)  Publish to npmjs.com

Native npm plugins must include a plugin manifest and `package.json` OpenClaw
entrypoint metadata.

package.json

```
{
  "name": "@acme/openclaw-plugin",
  "version": "1.0.0",
  "type": "module",
  "openclaw": {
    "extensions": ["./dist/index.js"]
  }
}
```

```
npm publish --access public
```

Users install npm-only with:

```
openclaw plugins install npm:@acme/openclaw-plugin
openclaw plugins install npm:@acme/openclaw-plugin@beta
openclaw plugins install npm:@acme/openclaw-plugin@1.0.0
```

If the same package is also available on ClawHub, `npm:` skips ClawHub lookup and
forces npm resolution.

## [​](https://docs.openclaw.ai/plugins/manage-plugins\#source-choice)  Source choice

- **ClawHub**: use when you want OpenClaw-native discovery, scan summaries,
versions, and install hints.
- **npmjs.com**: use when you already ship JavaScript packages or need npm
dist-tags/private registry workflows.
- **Git**: use when you want to install directly from a branch, tag, or commit.
- **Local path**: use when you are developing or testing a plugin on the same
machine.

## [​](https://docs.openclaw.ai/plugins/manage-plugins\#related)  Related

- [Plugins](https://docs.openclaw.ai/tools/plugin) \- overview and troubleshooting
- [`openclaw plugins`](https://docs.openclaw.ai/cli/plugins) \- full CLI reference
- [ClawHub](https://docs.openclaw.ai/tools/clawhub) \- publish and registry operations
- [Building plugins](https://docs.openclaw.ai/plugins/building-plugins) \- create a plugin package
- [Plugin manifest](https://docs.openclaw.ai/plugins/manifest) \- manifest and package metadata

[Install and Configure](https://docs.openclaw.ai/tools/plugin) [Community plugins](https://docs.openclaw.ai/plugins/community)

Ctrl+I