# Tools Plugins

_65 pages from docs.openclaw.ai_


---

## Browser - OpenClaw

_Source: <https://docs.openclaw.ai/cli/browser>_

# `openclaw browser`

Manage OpenClaw’s browser control surface and run browser actions (lifecycle, profiles, tabs, snapshots, screenshots, navigation, input, state emulation, and debugging).Related:

- Browser tool + API: [Browser tool](https://docs.openclaw.ai/tools/browser)

## Common flags

- `--url <gatewayWsUrl>`: Gateway WebSocket URL (defaults to config).
- `--token <token>`: Gateway token (if required).
- `--timeout <ms>`: request timeout (ms).
- `--expect-final`: wait for a final Gateway response.
- `--browser-profile <name>`: choose a browser profile (default from config).
- `--json`: machine-readable output (where supported).

## Quick start (local)

```
openclaw browser profiles
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw open https://example.com
openclaw browser --browser-profile openclaw snapshot
```

Agents can run the same readiness check with `browser({ action: "doctor" })`.

## Quick troubleshooting

If `start` fails with `not reachable after start`, troubleshoot CDP readiness first. If `start` and `tabs` succeed but `open` or `navigate` fails, the browser control plane is healthy and the failure is usually navigation SSRF policy.Minimal sequence:

```
openclaw browser --browser-profile openclaw doctor
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw tabs
openclaw browser --browser-profile openclaw open https://example.com
```

Detailed guidance: [Browser troubleshooting](https://docs.openclaw.ai/tools/browser#cdp-startup-failure-vs-navigation-ssrf-block)

## Lifecycle

```
openclaw browser status
openclaw browser doctor
openclaw browser doctor --deep
openclaw browser start
openclaw browser start --headless
openclaw browser stop
openclaw browser --browser-profile openclaw reset-profile
```

Notes:

- `doctor --deep` adds a live snapshot probe. It is useful when basic CDP
readiness is green but you want proof that the current tab can be inspected.
- For `attachOnly` and remote CDP profiles, `openclaw browser stop` closes the
active control session and clears temporary emulation overrides even when
OpenClaw did not launch the browser process itself.
- For local managed profiles, `openclaw browser stop` stops the spawned browser
process.
- `openclaw browser start --headless` applies only to that start request and
only when OpenClaw launches a local managed browser. It does not rewrite
`browser.headless` or profile config, and it is a no-op for an already-running
browser.
- On Linux hosts without `DISPLAY` or `WAYLAND_DISPLAY`, local managed profiles
run headless automatically unless `OPENCLAW_BROWSER_HEADLESS=0`,
`browser.headless=false`, or `browser.profiles.<name>.headless=false`
explicitly requests a visible browser.

## If the command is missing

If `openclaw browser` is an unknown command, check `plugins.allow` in
`~/.openclaw/openclaw.json`.When `plugins.allow` is present, list the bundled browser plugin explicitly
unless the config already has a root `browser` block:

```
{
  plugins: {
    allow: ["telegram", "browser"],
  },
}
```

An explicit root `browser` block, for example `browser.enabled=true` or
`browser.profiles.<name>`, also activates the bundled browser plugin under a
restrictive plugin allowlist.Related: [Browser tool](https://docs.openclaw.ai/tools/browser#missing-browser-command-or-tool)

## Profiles

Profiles are named browser routing configs. In practice:

- `openclaw`: launches or attaches to a dedicated OpenClaw-managed Chrome instance (isolated user data dir).
- `user`: controls your existing signed-in Chrome session via Chrome DevTools MCP.
- custom CDP profiles: point at a local or remote CDP endpoint.

```
openclaw browser profiles
openclaw browser create-profile --name work --color "#FF5A36"
openclaw browser create-profile --name chrome-live --driver existing-session
openclaw browser create-profile --name remote --cdp-url https://browser-host.example.com
openclaw browser delete-profile -

_… [truncated; see https://docs.openclaw.ai/cli/browser for full content]_


---

## Plugins - OpenClaw

_Source: <https://docs.openclaw.ai/cli/plugins>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugins and skills

Plugins

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

## Commands

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

### Install

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
on [npmjs.com/org/openclaw](http

_… [truncated; see https://docs.openclaw.ai/cli/plugins for full content]_


---

## Skills - OpenClaw

_Source: <https://docs.openclaw.ai/cli/skills>_

# `openclaw skills`

Inspect local skills and install/update skills from ClawHub.Related:

- Skills system: [Skills](https://docs.openclaw.ai/tools/skills)
- Skills config: [Skills config](https://docs.openclaw.ai/tools/skills-config)
- ClawHub installs: [ClawHub](https://docs.openclaw.ai/tools/clawhub)

## Commands

```
openclaw skills search "calendar"
openclaw skills search --limit 20 --json
openclaw skills install <slug>
openclaw skills install <slug> --version <version>
openclaw skills install <slug> --force
openclaw skills install <slug> --agent <id>
openclaw skills update <slug>
openclaw skills update --all
openclaw skills update --all --agent <id>
openclaw skills list
openclaw skills list --eligible
openclaw skills list --json
openclaw skills list --verbose
openclaw skills list --agent <id>
openclaw skills info <name>
openclaw skills info <name> --json
openclaw skills info <name> --agent <id>
openclaw skills check
openclaw skills check --agent <id>
openclaw skills check --json
```

`search`/`install`/`update` use ClawHub directly and install into the active
workspace `skills/` directory. `list`/`info`/`check` still inspect the local
skills visible to the current workspace and config. Workspace-backed commands
resolve the target workspace from `--agent <id>`, then the current working
directory when it is inside a configured agent workspace, then the default
agent.This CLI `install` command downloads skill folders from ClawHub. Gateway-backed
skill dependency installs triggered from onboarding or Skills settings use the
separate `skills.install` request path instead.Notes:

- `search [query...]` accepts an optional query; omit it to browse the default
ClawHub search feed.
- `search --limit <n>` caps returned results.
- `install --force` overwrites an existing workspace skill folder for the same
slug.
- `--agent <id>` targets one configured agent workspace and overrides current
working directory inference.
- `update --all` only updates tracked ClawHub installs in the active workspace.
- `check --agent <id>` checks the selected agent’s workspace and reports which
ready skills are actually visible to that agent’s prompt or command surface.
- `list` is the default action when no subcommand is provided.
- `list`, `info`, and `check` write their rendered output to stdout. With
`--json`, that means the machine-readable payload stays on stdout for pipes
and scripts.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Skills](https://docs.openclaw.ai/tools/skills)

[Plugins](https://docs.openclaw.ai/cli/plugins) [Dashboard](https://docs.openclaw.ai/cli/dashboard)

Ctrl+I


---

## https://docs.openclaw.ai/cli/skills.md

_Source: <https://docs.openclaw.ai/cli/skills.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Skills

\# \`openclaw skills\`

Inspect local skills and install/update skills from ClawHub.

Related:

\\* Skills system: \[Skills\](/tools/skills)
\\* Skills config: \[Skills config\](/tools/skills-config)
\\* ClawHub installs: \[ClawHub\](/tools/clawhub)

\## Commands

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw skills search "calendar"
openclaw skills search --limit 20 --json
openclaw skills install
openclaw skills install  --version
openclaw skills install  --force
openclaw skills install  --agent
openclaw skills update
openclaw skills update --all
openclaw skills update --all --agent
openclaw skills list
openclaw skills list --eligible
openclaw skills list --json
openclaw skills list --verbose
openclaw skills list --agent
openclaw skills info
openclaw skills info  --json
openclaw skills info  --agent
openclaw skills check
openclaw skills check --json
openclaw skills check --agent
\`\`\`

\`search\`/\`install\`/\`update\` use ClawHub directly and install into the active
workspace \`skills/\` directory. \`list\`/\`info\`/\`check\` still inspect the local
skills visible to the current workspace and config. Workspace-backed commands
resolve the target workspace from \`--agent \`, then the current working
directory when it is inside a configured agent workspace, then the default
agent.

This CLI \`install\` command downloads skill folders from ClawHub. Gateway-backed
skill dependency installs triggered from onboarding or Skills settings use the
separate \`skills.install\` request path instead.

Notes:

\\* \`search \[query...\]\` accepts an optional query; omit it to browse the default
 ClawHub search feed.
\\* \`search --limit \` caps returned results.
\\* \`install --force\` overwrites an existing workspace skill folder for the same
 slug.
\\* \`--agent \` targets one configured agent workspace and overrides current
 working directory inference.
\\* \`update --all\` only updates tracked ClawHub installs in the active workspace.
\\* \`list\` is the default action when no subcommand is provided.
\\* \`list\`, \`info\`, and \`check\` write their rendered output to stdout. With
 \`--json\`, that means the machine-readable payload stays on stdout for pipes
 and scripts.

\## Related

\\* \[CLI reference\](/cli)
\\* \[Skills\](/tools/skills)


---

## https://docs.openclaw.ai/gateway/tools-invoke-http-api.md

_Source: <https://docs.openclaw.ai/gateway/tools-invoke-http-api.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Tools invoke API

\# Tools Invoke (HTTP)

OpenClaw’s Gateway exposes a simple HTTP endpoint for invoking a single tool directly. It is always enabled and uses Gateway auth plus tool policy. Like the OpenAI-compatible \`/v1/\*\` surface, shared-secret bearer auth is treated as trusted operator access for the whole gateway.

\\* \`POST /tools/invoke\`
\\* Same port as the Gateway (WS + HTTP multiplex): \`http://:/tools/invoke\`

Default max payload size is 2 MB.

\## Authentication

Uses the Gateway auth configuration.

Common HTTP auth paths:

\\* shared-secret auth (\`gateway.auth.mode="token"\` or \`"password"\`):
 \`Authorization: Bearer \`
\\* trusted identity-bearing HTTP auth (\`gateway.auth.mode="trusted-proxy"\`):
 route through the configured identity-aware proxy and let it inject the
 required identity headers
\\* private-ingress open auth (\`gateway.auth.mode="none"\`):
 no auth header required

Notes:

\\* When \`gateway.auth.mode="token"\`, use \`gateway.auth.token\` (or \`OPENCLAW\_GATEWAY\_TOKEN\`).
\\* When \`gateway.auth.mode="password"\`, use \`gateway.auth.password\` (or \`OPENCLAW\_GATEWAY\_PASSWORD\`).
\\* When \`gateway.auth.mode="trusted-proxy"\`, the HTTP request must come from a
 configured trusted proxy source; same-host loopback proxies require explicit
 \`gateway.auth.trustedProxy.allowLoopback = true\`.
\\* If \`gateway.auth.rateLimit\` is configured and too many auth failures occur, the endpoint returns \`429\` with \`Retry-After\`.

\## Security boundary (important)

Treat this endpoint as a \*\*full operator-access\*\* surface for the gateway instance.

\\* HTTP bearer auth here is not a narrow per-user scope model.
\\* A valid Gateway token/password for this endpoint should be treated like an owner/operator credential.
\\* For shared-secret auth modes (\`token\` and \`password\`), the endpoint restores the normal full operator defaults even if the caller sends a narrower \`x-openclaw-scopes\` header.
\\* Shared-secret auth also treats direct tool invokes on this endpoint as owner-sender turns.
\\* Trusted identity-bearing HTTP modes (for example trusted proxy auth or \`gateway.auth.mode="none"\` on a private ingress) honor \`x-openclaw-scopes\` when present and otherwise fall back to the normal operator default scope set.
\\* Keep this endpoint on loopback/tailnet/private ingress only; do not expose it directly to the public internet.

Auth matrix:

\\* \`gateway.auth.mode="token"\` or \`"password"\` + \`Authorization: Bearer ...\`
 \\* proves possession of the shared gateway operator secret
 \\* ignores narrower \`x-openclaw-scopes\`
 \\* restores the full default operator scope set:
 \`operator.admin\`, \`operator.approvals\`, \`operator.pairing\`,
 \`operator.read\`, \`operator.talk.secrets\`, \`operator.write\`
 \\* treats direct tool invokes on this endpoint as owner-sender turns
\\* trusted identity-bearing HTTP modes (for example trusted proxy auth, or \`gateway.auth.mode="none"\` on private ingress)
 \\* authenticate some outer trusted identity or deployment boundary
 \\* honor \`x-openclaw-scopes\` when the header is present
 \\* fall back to the normal operator default scope set when the header is absent
 \\* only lose owner semantics when the caller explicitly narrows scopes and omits \`operator.admin\`

\## Request body

\`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 "tool": "sessions\_list",
 "action": "json",
 "args": {},
 "sessionKey": "main",
 "dryRun": false
}
\`\`\`

Fields:

\\* \`tool\` (string, required): tool name to invoke.
\\* \`action\` (string, optional): mapped into args if the tool schema supports \`action\` and the args payload omitted it.
\\* \`args\` (object, optional): tool-specific arguments.
\\* \`sessionKey\` (string, optional): target session key. If omitted or \`"main"\`, the Ga

_… [truncated; see https://docs.openclaw.ai/gateway/tools-invoke-http-api.md for full content]_


---

## Testing: updates and plugins - OpenClaw

_Source: <https://docs.openclaw.ai/help/testing-updates-plugins>_

[OpenClaw home page](https://docs.openclaw.ai/)

Testing

Testing: updates and plugins

This is the dedicated checklist for update and plugin validation. The goal is
simple: prove the installable package can update real user state, repair stale
legacy state through `doctor`, and still install, load, update, and uninstall
plugins from the supported sources.For the broader test runner map, see [Testing](https://docs.openclaw.ai/help/testing). For live provider
keys and network-touching suites, see [Testing live](https://docs.openclaw.ai/help/testing-live).

## What we protect

Update and plugin tests protect these contracts:

- A package tarball is complete, has a valid `dist/postinstall-inventory.json`,
and does not depend on unpacked repo files.
- A user can move from an older published package to the candidate package
without losing config, agents, sessions, workspaces, plugin allowlists, or
channel config.
- `openclaw doctor --fix --non-interactive` owns legacy cleanup and repair
paths. Startup should not grow hidden compatibility migrations for stale
plugin state.
- Plugin installs work from local directories, git repos, npm packages, and the
ClawHub registry path.
- Plugin npm dependencies are installed in the managed npm root, scanned before
trust, and removed through npm during uninstall so hoisted dependencies do not
linger.
- Plugin update is stable when nothing changed: install records, resolved
source, installed dependency layout, and enabled state stay intact.

## Local proof during development

Start narrow:

```
pnpm changed:lanes --json
pnpm check:changed
pnpm test:changed
```

For plugin install, uninstall, dependency, or package-inventory changes, also
run the focused tests that cover the edited seam:

```
pnpm test src/plugins/uninstall.test.ts src/infra/package-dist-inventory.test.ts test/scripts/package-acceptance-workflow.test.ts
```

Before any package Docker lane consumes a tarball, prove the package artifact:

```
pnpm release:check
```

`release:check` runs config/docs/API drift checks, writes the package dist
inventory, runs `npm pack --dry-run`, rejects forbidden packed files, installs
the tarball into a temp prefix, runs postinstall, and smokes bundled channel
entrypoints.

## Docker lanes

The Docker lanes are the product-level proof. They install or update a real
package inside Linux containers and assert behavior through CLI commands,
Gateway startup, HTTP probes, RPC status, and filesystem state.Use focused lanes while iterating:

```
pnpm test:docker:plugins
pnpm test:docker:plugin-lifecycle-matrix
pnpm test:docker:plugin-update
pnpm test:docker:upgrade-survivor
pnpm test:docker:published-upgrade-survivor
pnpm test:docker:update-migration
```

Important lanes:

- `test:docker:plugins` validates plugin install smoke, local folder installs,
local folder update skip behavior, local folders with preinstalled
dependencies, `file:` package installs, git installs with CLI execution, git
moving-ref updates, npm registry installs with hoisted transitive
dependencies, npm update no-ops, local ClawHub fixture installs and update
no-ops, marketplace update behavior, and Claude-bundle enable/inspect. Set
`OPENCLAW_PLUGINS_E2E_CLAWHUB=0` to keep the ClawHub block hermetic/offline.
- `test:docker:plugin-lifecycle-matrix` installs the candidate package in a bare
container, runs an npm plugin through install, inspect, disable, enable,
explicit upgrade, explicit downgrade, and uninstall after deleting the plugin
code. It logs RSS and CPU metrics for each phase.
- `test:docker:plugin-update` validates that an unchanged installed plugin does
not reinstall or lose install metadata during `openclaw plugins update`.
- `test:docker:upgrade-survivor` installs the candidate tarball over a dirty
old-user fixture, runs package update plus non-interactive doctor, then starts
a loopback Gateway and checks state preservation.
- `test:docker:published-upgrade-survivor` first installs a published baseline,
configures it through

_… [truncated; see https://docs.openclaw.ai/help/testing-updates-plugins for full content]_


---

## Building plugins - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/building-plugins>_

[OpenClaw home page](https://docs.openclaw.ai/)

Building plugins

Building plugins

Plugins extend OpenClaw with new capabilities: channels, model providers,
speech, realtime transcription, realtime voice, media understanding, image
generation, video generation, web fetch, web search, agent tools, or any
combination.You do not need to add your plugin to the OpenClaw repository. Publish to
[ClawHub](https://docs.openclaw.ai/tools/clawhub) and users install with
`openclaw plugins install clawhub:<package-name>`. Bare package specs still
install from npm during the launch cutover.

## Prerequisites

- Node >= 22 and a package manager (npm or pnpm)
- Familiarity with TypeScript (ESM)
- For in-repo plugins: repository cloned and `pnpm install` done. Source
checkout plugin development is pnpm-only because OpenClaw loads bundled
plugins from the `extensions/*` workspace packages.

## What kind of plugin?

[**Channel plugin** \\
\\
Connect OpenClaw to a messaging platform (Discord, IRC, etc.)](https://docs.openclaw.ai/plugins/sdk-channel-plugins)

[**Provider plugin** \\
\\
Add a model provider (LLM, proxy, or custom endpoint)](https://docs.openclaw.ai/plugins/sdk-provider-plugins)

[**Tool / hook plugin** \\
\\
Register agent tools, event hooks, or services — continue below](https://docs.openclaw.ai/plugins/hooks)

For a channel plugin that isn’t guaranteed to be installed when onboarding/setup
runs, use `createOptionalChannelSetupSurface(...)` from
`openclaw/plugin-sdk/channel-setup`. It produces a setup adapter + wizard pair
that advertises the install requirement and fails closed on real config writes
until the plugin is installed.

## Quick start: tool plugin

This walkthrough creates a minimal plugin that registers an agent tool. Channel
and provider plugins have dedicated guides linked above.

1

[Navigate to header](https://docs.openclaw.ai/plugins/building-plugins#)

Create the package and manifest

package.json

openclaw.plugin.json

```
{
  "name": "@myorg/openclaw-my-plugin",
  "version": "1.0.0",
  "type": "module",
  "openclaw": {
    "extensions": ["./index.ts"],
    "compat": {
      "pluginApi": ">=2026.3.24-beta.2",
      "minGatewayVersion": "2026.3.24-beta.2"
    },
    "build": {
      "openclawVersion": "2026.3.24-beta.2",
      "pluginSdkVersion": "2026.3.24-beta.2"
    }
  }
}
```

Every plugin needs a manifest, even with no config. Runtime-registered tools
must be listed in `contracts.tools` so OpenClaw can discover the owning
plugin without loading every plugin runtime. Plugins should also declare
`activation.onStartup` intentionally. This example sets it to `true`. See
[Manifest](https://docs.openclaw.ai/plugins/manifest) for the full schema. The canonical ClawHub
publish snippets live in `docs/snippets/plugin-publish/`.

2

[Navigate to header](https://docs.openclaw.ai/plugins/building-plugins#)

Write the entry point

```
// index.ts
import { definePluginEntry } from "openclaw/plugin-sdk/plugin-entry";
import { Type } from "@sinclair/typebox";

export default definePluginEntry({
  id: "my-plugin",
  name: "My Plugin",
  description: "Adds a custom tool to OpenClaw",
  register(api) {
    api.registerTool({
      name: "my_tool",
      description: "Do a thing",
      parameters: Type.Object({ input: Type.String() }),
      async execute(_id, params) {
        return { content: [{ type: "text", text: `Got: ${params.input}` }] };
      },
    });
  },
});
```

`definePluginEntry` is for non-channel plugins. For channels, use
`defineChannelPluginEntry` — see [Channel Plugins](https://docs.openclaw.ai/plugins/sdk-channel-plugins).
For full entry point options, see [Entry Points](https://docs.openclaw.ai/plugins/sdk-entrypoints).

3

[Navigate to header](https://docs.openclaw.ai/plugins/building-plugins#)

Test and publish

**External plugins:** validate and publish with ClawHub, then install:

```
clawhub package publish your-org/your-plugin --dry-run
clawhub package publish your-org/your-plugin
openclaw plug

_… [truncated; see https://docs.openclaw.ai/plugins/building-plugins for full content]_


---

## Codex Computer Use - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/codex-computer-use>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugins

Codex Computer Use

Computer Use is a Codex-native MCP plugin for local desktop control. OpenClaw
does not vendor the desktop app, execute desktop actions itself, or bypass
Codex permissions. The bundled `codex` plugin only prepares Codex app-server:
it enables Codex plugin support, finds or installs the configured Codex
Computer Use plugin, checks that the `computer-use` MCP server is available, and
then lets Codex own the native MCP tool calls during Codex-mode turns.Use this page when OpenClaw is already using the native Codex harness. For the
runtime setup itself, see [Codex harness](https://docs.openclaw.ai/plugins/codex-harness).

## OpenClaw.app and Peekaboo

OpenClaw.app’s Peekaboo integration is separate from Codex Computer Use. The
macOS app can host a PeekabooBridge socket so the `peekaboo` CLI can reuse the
app’s local Accessibility and Screen Recording grants for Peekaboo’s own
automation tools. That bridge does not install or proxy Codex Computer Use, and
Codex Computer Use does not call through the PeekabooBridge socket.Use [Peekaboo bridge](https://docs.openclaw.ai/platforms/mac/peekaboo) when you want OpenClaw.app to be
a permission-aware host for Peekaboo CLI automation. Use this page when a
Codex-mode OpenClaw agent should have Codex’s native `computer-use` MCP plugin
available before the turn starts.

## iOS app

The iOS app is separate from Codex Computer Use. It does not install or proxy
the Codex `computer-use` MCP server and it is not a desktop-control backend.
Instead, the iOS app connects as an OpenClaw node and exposes mobile
capabilities through node commands such as `canvas.*`, `camera.*`, `screen.*`,
`location.*`, and `talk.*`.Use [iOS](https://docs.openclaw.ai/platforms/ios) when you want an agent to drive an iPhone node through
the gateway. Use this page when a Codex-mode agent should control the local
macOS desktop through Codex’s native Computer Use plugin.

## Direct cua-driver MCP

Codex Computer Use is not the only way to expose desktop control. If you want
OpenClaw-managed runtimes to call TryCua’s driver directly, use the upstream
`cua-driver mcp` server through OpenClaw’s MCP registry instead of the
Codex-specific marketplace flow.After installing `cua-driver`, either ask it for the OpenClaw command:

```
cua-driver mcp-config --client openclaw
```

or register the stdio server yourself:

```
openclaw mcp set cua-driver '{"command":"cua-driver","args":["mcp"]}'
```

That path keeps the upstream MCP tool surface intact, including the driver
schemas and structured MCP responses. Use it when you want the CUA driver
available as a normal OpenClaw MCP server. Use the Codex Computer Use setup on
this page when Codex app-server should own plugin installation, MCP reloads,
and native tool calls inside Codex-mode turns.CUA’s driver is macOS-specific and still requires the local macOS permissions
that its app prompts for, such as Accessibility and Screen Recording. OpenClaw
does not install `cua-driver`, grant those permissions, or bypass the upstream
driver’s safety model.

## Quick setup

Set `plugins.entries.codex.config.computerUse` when Codex-mode turns must have
Computer Use available before a thread starts:

```
{
  plugins: {
    entries: {
      codex: {
        enabled: true,
        config: {
          computerUse: {
            autoInstall: true,
          },
        },
      },
    },
  },
  agents: {
    defaults: {
      model: "openai/gpt-5.5",
      agentRuntime: {
        id: "codex",
      },
    },
  },
}
```

With this config, OpenClaw checks Codex app-server before each Codex-mode turn.
If Computer Use is missing but Codex app-server has already discovered an
installable marketplace, OpenClaw asks Codex app-server to install or re-enable
the plugin and reload MCP servers. On macOS, when no matching marketplace is
registered and the standard Codex app bundle exists, OpenClaw also tries to
register the bundled Codex mark

_… [truncated; see https://docs.openclaw.ai/plugins/codex-computer-use for full content]_


---

## Codex harness - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/codex-harness>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugins

Codex harness

The bundled `codex` plugin lets OpenClaw run embedded agent turns through the
Codex app-server instead of the built-in PI harness.Use this when you want Codex to own the low-level agent session: model
discovery, native thread resume, native compaction, and app-server execution.
OpenClaw still owns chat channels, session files, model selection, tools,
approvals, media delivery, and the visible transcript mirror.When a source chat turn runs through the Codex harness, visible replies default
to the OpenClaw `message` tool if the deployment has not explicitly configured
`messages.visibleReplies`. The agent can still finish its Codex turn privately;
it only posts to the channel when it calls `message(action="send")`. Set
`messages.visibleReplies: "automatic"` to keep direct-chat final replies on the
legacy automatic delivery path.Codex heartbeat turns also get the `heartbeat_respond` tool by default, so the
agent can record whether the wake should stay quiet or notify without encoding
that control flow in final text.If you are trying to orient yourself, start with
[Agent runtimes](https://docs.openclaw.ai/concepts/agent-runtimes). The short version is:
`openai/gpt-5.5` is the model ref, `codex` is the runtime, and Telegram,
Discord, Slack, or another channel remains the communication surface.

## Quick config

Most users who want “Codex in OpenClaw” want this route: sign in with a
ChatGPT/Codex subscription, then run embedded agent turns through the native
Codex app-server runtime. The model ref still stays canonical as
`openai/gpt-*`; subscription auth comes from the Codex account/profile, not
from an `openai-codex/*` model prefix.First sign in with Codex OAuth if you have not already:

```
openclaw models auth login --provider openai-codex
```

Then enable the bundled `codex` plugin and force the Codex runtime:

```
{
  plugins: {
    entries: {
      codex: {
        enabled: true,
      },
    },
  },
  agents: {
    defaults: {
      model: "openai/gpt-5.5",
      agentRuntime: {
        id: "codex",
      },
    },
  },
}
```

If your config uses `plugins.allow`, include `codex` there too:

```
{
  plugins: {
    allow: ["codex"],
    entries: {
      codex: {
        enabled: true,
      },
    },
  },
}
```

Do not use `openai-codex/gpt-*` when you mean native Codex runtime. That prefix
is the explicit “Codex OAuth through PI” route. Config changes apply to new or
reset sessions; existing sessions keep their recorded runtime.

## What this plugin changes

The bundled `codex` plugin contributes several separate capabilities:

| Capability | How you use it | What it does |
| --- | --- | --- |
| Native embedded runtime | `agentRuntime.id: "codex"` | Runs OpenClaw embedded agent turns through Codex app-server. |
| Native chat-control commands | `/codex bind`, `/codex resume`, `/codex steer`, … | Binds and controls Codex app-server threads from a messaging conversation. |
| Codex app-server provider/catalog | `codex` internals, surfaced through the harness | Lets the runtime discover and validate app-server models. |
| Codex media-understanding path | `codex/*` image-model compatibility paths | Runs bounded Codex app-server turns for supported image understanding models. |
| Native hook relay | Plugin hooks around Codex-native events | Lets OpenClaw observe/block supported Codex-native tool/finalization events. |

Enabling the plugin makes those capabilities available. It does **not**:

- start using Codex for every OpenAI model
- convert `openai-codex/*` model refs into the native runtime
- make ACP/acpx the default Codex path
- hot-switch existing sessions that already recorded a PI runtime
- replace OpenClaw channel delivery, session files, auth-profile storage, or
message routing

The same plugin also owns the native `/codex` chat-control command surface. If
the plugin is enabled and the user asks to bind, resume, steer, stop, or inspect
Codex threads f

_… [truncated; see https://docs.openclaw.ai/plugins/codex-harness for full content]_


---

## Community plugins - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/community>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugins

Community plugins

Community plugins are third-party packages that extend OpenClaw with new
channels, tools, providers, or other capabilities. They are built and maintained
by the community, usually published on [ClawHub](https://docs.openclaw.ai/tools/clawhub), and installable
with a single command. Npm remains the launch default for bare package specs
while ClawHub pack installs roll out.ClawHub is the canonical discovery surface for community plugins. Do not open
docs-only PRs just to add your plugin here for discoverability; publish it on
ClawHub instead.

```
openclaw plugins install clawhub:<package-name>
```

Use `openclaw plugins install <package-name>` for npm-hosted packages.

## Listed plugins

### Apify

Scrape data from any website with 20,000+ ready-made scrapers. Let your agent
extract data from Instagram, Facebook, TikTok, YouTube, Google Maps, Google
Search, e-commerce sites, and more — just by asking.

- **npm:**`@apify/apify-openclaw-plugin`
- **repo:** [github.com/apify/apify-openclaw-plugin](https://github.com/apify/apify-openclaw-plugin)

```
openclaw plugins install @apify/apify-openclaw-plugin
```

### Codex App Server Bridge

Independent OpenClaw bridge for Codex App Server conversations. Bind a chat to
a Codex thread, talk to it with plain text, and control it with chat-native
commands for resume, planning, review, model selection, compaction, and more.

- **npm:**`openclaw-codex-app-server`
- **repo:** [github.com/pwrdrvr/openclaw-codex-app-server](https://github.com/pwrdrvr/openclaw-codex-app-server)

```
openclaw plugins install openclaw-codex-app-server
```

### DingTalk

Enterprise robot integration using Stream mode. Supports text, images, and
file messages via any DingTalk client.

- **npm:**`@largezhou/ddingtalk`
- **repo:** [github.com/largezhou/openclaw-dingtalk](https://github.com/largezhou/openclaw-dingtalk)

```
openclaw plugins install @largezhou/ddingtalk
```

### Lossless Claw (LCM)

Lossless Context Management plugin for OpenClaw. DAG-based conversation
summarization with incremental compaction — preserves full context fidelity
while reducing token usage.

- **npm:**`@martian-engineering/lossless-claw`
- **repo:** [github.com/Martian-Engineering/lossless-claw](https://github.com/Martian-Engineering/lossless-claw)

```
openclaw plugins install @martian-engineering/lossless-claw
```

### Opik

Official plugin that exports agent traces to Opik. Monitor agent behavior,
cost, tokens, errors, and more.

- **npm:**`@opik/opik-openclaw`
- **repo:** [github.com/comet-ml/opik-openclaw](https://github.com/comet-ml/opik-openclaw)

```
openclaw plugins install @opik/opik-openclaw
```

### Prometheus Avatar

Give your OpenClaw agent a Live2D avatar with real-time lip-sync, emotion
expressions, and text-to-speech. Includes creator tools for AI asset generation
and one-click deployment to the Prometheus Marketplace. Currently in alpha.

- **npm:**`@prometheusavatar/openclaw-plugin`
- **repo:** [github.com/myths-labs/prometheus-avatar](https://github.com/myths-labs/prometheus-avatar)

```
openclaw plugins install @prometheusavatar/openclaw-plugin
```

### QQbot

Connect OpenClaw to QQ via the QQ Bot API. Supports private chats, group
mentions, channel messages, and rich media including voice, images, videos,
and files.Current OpenClaw releases bundle QQ Bot. Use the bundled setup in
[QQ Bot](https://docs.openclaw.ai/channels/qqbot) for normal installs; install this external plugin only
when you intentionally want the Tencent-maintained standalone package.

- **npm:**`@tencent-connect/openclaw-qqbot`
- **repo:** [github.com/tencent-connect/openclaw-qqbot](https://github.com/tencent-connect/openclaw-qqbot)

```
openclaw plugins install @tencent-connect/openclaw-qqbot
```

### wecom

WeCom channel plugin for OpenClaw by the Tencent WeCom team. Powered by
WeCom Bot WebSocket persistent connections, it supports direct messages & group
chats, streami

_… [truncated; see https://docs.openclaw.ai/plugins/community for full content]_


---

## Plugin compatibility - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/compatibility>_

[OpenClaw home page](https://docs.openclaw.ai/)

Building plugins

Plugin compatibility

OpenClaw keeps older plugin contracts wired through named compatibility
adapters before removing them. This protects existing bundled and external
plugins while the SDK, manifest, setup, config, and agent runtime contracts
evolve.

## Compatibility registry

Plugin compatibility contracts are tracked in the core registry at
`src/plugins/compat/registry.ts`.Each record has:

- a stable compatibility code
- status: `active`, `deprecated`, `removal-pending`, or `removed`
- owner: SDK, config, setup, channel, provider, plugin execution, agent runtime,
or core
- introduction and deprecation dates when applicable
- replacement guidance
- docs, diagnostics, and tests that cover the old and new behavior

The registry is the source for maintainer planning and future plugin inspector
checks. If a plugin-facing behavior changes, add or update the compatibility
record in the same change that adds the adapter.Doctor repair and migration compatibility is tracked separately at
`src/commands/doctor/shared/deprecation-compat.ts`. Those records cover old
config shapes, install-ledger layouts, and repair shims that may need to stay
available after the runtime compatibility path is removed.Release sweeps should check both registries. Do not delete a doctor migration
just because the matching runtime or config compatibility record expired; first
verify there is no supported upgrade path that still needs the repair. Also
revalidate each replacement annotation during release planning because plugin
ownership and config footprint can change as providers and channels move out of
core.

## Plugin inspector package

The plugin inspector should live outside the core OpenClaw repo as a separate
package/repository backed by the versioned compatibility and manifest
contracts.The day-one CLI should be:

```
openclaw-plugin-inspector ./my-plugin
```

It should emit:

- manifest/schema validation
- the contract compatibility version being checked
- install/source metadata checks
- cold-path import checks
- deprecation and compatibility warnings

Use `--json` for stable machine-readable output in CI annotations. OpenClaw
core should expose contracts and fixtures the inspector can consume, but should
not publish the inspector binary from the main `openclaw` package.

### Maintainer acceptance lane

Use Blacksmith Testbox for the installable-package acceptance lane when validating
the external inspector against OpenClaw plugin packages. Run it from a clean
OpenClaw checkout after the package is built:

```
blacksmith testbox warmup ci-check-testbox.yml --ref main --idle-timeout 90
blacksmith testbox run --id <tbx_id> "pnpm install && pnpm build && npm exec --yes @openclaw/plugin-inspector@0.1.0 -- ./extensions/telegram --json"
blacksmith testbox run --id <tbx_id> "npm exec --yes @openclaw/plugin-inspector@0.1.0 -- ./extensions/discord --json"
blacksmith testbox run --id <tbx_id> "npm exec --yes @openclaw/plugin-inspector@0.1.0 -- <clawhub-plugin-dir> --json"
blacksmith testbox stop <tbx_id>
```

Keep this lane opt-in for maintainers because it installs an external npm
package and may inspect plugin packages cloned outside the repo. The local repo
guards cover the SDK export map, compatibility registry metadata, deprecated
SDK-import burn-down, and bundled extension import boundaries; Testbox inspector
proof covers the package as external plugin authors consume it.

## Deprecation policy

OpenClaw should not remove a documented plugin contract in the same release
that introduces its replacement.The migration sequence is:

1. Add the new contract.
2. Keep the old behavior wired through a named compatibility adapter.
3. Emit diagnostics or warnings when plugin authors can act.
4. Document the replacement and timeline.
5. Test both old and new paths.
6. Wait through the announced migration window.
7. Remove only with explicit breaking-release approval.

Deprecated records must in

_… [truncated; see https://docs.openclaw.ai/plugins/compatibility for full content]_


---

## Plugin dependency resolution - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/dependency-resolution>_

# Plugin dependency resolution

OpenClaw keeps plugin dependency work at install/update time. Runtime loading
does not run package managers, repair dependency trees, or mutate the OpenClaw
package directory.

## Responsibility split

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

## Install roots

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

## Local plugins

Local plugins are treated as developer-controlled directories. OpenClaw does not
run `npm install`, `pnpm install`, or dependency repair for them. If a local
plugin has dependencies, install them in that plugin before loading it.Third-party TypeScript local plugins can use the emergency Jiti path. Packaged
JavaScript plugins and bundled internal plugins load through native
import/require instead of Jiti.

## Startup and reload

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

## Bundled plugins

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
| `npm install -g openclaw` | Built runtime tree inside the package | OpenClaw package an

_… [truncated; see https://docs.openclaw.ai/plugins/dependency-resolution for full content]_


---

## Google Meet plugin - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/google-meet>_

# or
export GEMINI_API_KEY=...
```

`blackhole-2ch` installs the `BlackHole 2ch` virtual audio device. Homebrew’s
installer requires a reboot before macOS exposes the device:

```
sudo reboot
```

After reboot, verify both pieces:

```
system_profiler SPAudioDataType | grep -i BlackHole
command -v sox
```

Enable the plugin:

```
{
  plugins: {
    entries: {
      "google-meet": {
        enabled: true,
        config: {},
      },
    },
  },
}
```

Check setup:

```
openclaw googlemeet setup
```

The setup output is meant to be agent-readable and mode-aware. It reports Chrome
profile, node pinning, and, for realtime Chrome joins, the BlackHole/SoX audio
bridge and delayed realtime intro checks. For observe-only joins, check the same
transport with `--mode transcribe`; that mode skips realtime audio prerequisites
because it does not listen through or speak through the bridge:

```
openclaw googlemeet setup --transport chrome-node --mode transcribe
```

When Twilio delegation is configured, setup also reports whether the
`voice-call` plugin, Twilio credentials, and public webhook exposure are ready.
Treat any `ok: false` check as a blocker for the checked transport and mode
before asking an agent to join. Use `openclaw googlemeet setup --json` for
scripts or machine-readable output. Use `--transport chrome`,
`--transport chrome-node`, or `--transport twilio` to preflight a specific
transport before an agent tries it.For Twilio, always preflight the transport explicitly when the default transport
is Chrome:

```
openclaw googlemeet setup --transport twilio
```

That catches missing `voice-call` wiring, Twilio credentials, or unreachable
webhook exposure before the agent tries to dial the meeting.Join a meeting:

```
openclaw googlemeet join https://meet.google.com/abc-defg-hij
```

Or let an agent join through the `google_meet` tool:

```
{
  "action": "join",
  "url": "https://meet.google.com/abc-defg-hij",
  "transport": "chrome-node",
  "mode": "realtime"
}
```

The agent-facing `google_meet` tool stays available on non-macOS hosts for
artifact, calendar, setup, transcribe, Twilio, and `chrome-node` flows. Local
Chrome realtime actions are blocked there because the bundled realtime Chrome
audio path currently depends on macOS `BlackHole 2ch`. On Linux, use
`mode: "transcribe"`, Twilio dial-in, or a macOS `chrome-node` host for realtime
Chrome participation.Create a new meeting and join it:

```
openclaw googlemeet create --transport chrome-node --mode realtime
```

For API-created rooms, use Google Meet `SpaceConfig.accessType` when you want
the room’s no-knock policy to be explicit instead of inherited from the Google
account defaults:

```
openclaw googlemeet create --access-type OPEN --transport chrome-node --mode realtime
```

`OPEN` lets anyone with the Meet URL join without knocking. `TRUSTED` lets the
host organization’s trusted users, invited external users, and dial-in users
join without knocking. `RESTRICTED` limits no-knock entry to invitees. These
settings only apply to the official Google Meet API creation path, so OAuth
credentials must be configured.If you authenticated Google Meet before this option was available, rerun
`openclaw googlemeet auth login --json` after adding the
`meetings.space.settings` scope to your Google OAuth consent screen.Create only the URL without joining:

```
openclaw googlemeet create --no-join
```

`googlemeet create` has two paths:

- API create: used when Google Meet OAuth credentials are configured. This is
the most deterministic path and does not depend on browser UI state.
- Browser fallback: used when OAuth credentials are absent. OpenClaw uses the
pinned Chrome node, opens `https://meet.google.com/new`, waits for Google to
redirect to a real meeting-code URL, then returns that URL. This path requires
the OpenClaw Chrome profile on the node to already be signed in to Google.
Browser automation handles Meet’s own first-run microphone prompt; that prompt
is not treated as a Goo

_… [truncated; see https://docs.openclaw.ai/plugins/google-meet for full content]_


---

## Plugin hooks - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/hooks>_

[OpenClaw home page](https://docs.openclaw.ai/)

Building plugins

Plugin hooks

Plugin hooks are in-process extension points for OpenClaw plugins. Use them
when a plugin needs to inspect or change agent runs, tool calls, message flow,
session lifecycle, subagent routing, installs, or Gateway startup.Use [internal hooks](https://docs.openclaw.ai/automation/hooks) instead when you want a small
operator-installed `HOOK.md` script for command and Gateway events such as
`/new`, `/reset`, `/stop`, `agent:bootstrap`, or `gateway:startup`.

## Quick start

Register typed plugin hooks with `api.on(...)` from your plugin entry:

```
import { definePluginEntry } from "openclaw/plugin-sdk/plugin-entry";

export default definePluginEntry({
  id: "tool-preflight",
  name: "Tool Preflight",
  register(api) {
    api.on(
      "before_tool_call",
      async (event) => {
        if (event.toolName !== "web_search") {
          return;
        }

        return {
          requireApproval: {
            title: "Run web search",
            description: `Allow search query: ${String(event.params.query ?? "")}`,
            severity: "info",
            timeoutMs: 60_000,
            timeoutBehavior: "deny",
          },
        };
      },
      { priority: 50 },
    );
  },
});
```

Hook handlers run sequentially in descending `priority`. Same-priority hooks
keep registration order.`api.on(name, handler, opts?)` accepts:

- `priority` — handler ordering (higher runs first).
- `timeoutMs` — optional per-hook budget. When set, the hook runner aborts that
handler after the budget elapses and continues with the next one, instead of
letting slow setup or recall work consume the caller’s configured model
timeout. Omit it to use the default observation/decision timeout that the
hook runner applies generically.

Each hook receives `event.context.pluginConfig`, the resolved config for the
plugin that registered that handler. Use it for hook decisions that need
current plugin options; OpenClaw injects it per handler without mutating the
shared event object seen by other plugins.

## Hook catalog

Hooks are grouped by the surface they extend. Names in **bold** accept a
decision result (block, cancel, override, or require approval); all others are
observation-only.**Agent turn**

- `before_model_resolve` — override provider or model before session messages load
- `agent_turn_prepare` — consume queued plugin turn injections and add same-turn context before prompt hooks
- `before_prompt_build` — add dynamic context or system-prompt text before the model call
- `before_agent_start` — compatibility-only combined phase; prefer the two hooks above
- **`before_agent_reply`** — short-circuit the model turn with a synthetic reply or silence
- **`before_agent_finalize`** — inspect the natural final answer and request one more model pass
- `agent_end` — observe final messages, success state, and run duration
- `heartbeat_prompt_contribution` — add heartbeat-only context for background monitor and lifecycle plugins

**Conversation observation**

- `model_call_started` / `model_call_ended` — observe sanitized provider/model call metadata, timing, outcome, and bounded request-id hashes without prompt or response content
- `llm_input` — observe provider input (system prompt, prompt, history)
- `llm_output` — observe provider output

**Tools**

- **`before_tool_call`** — rewrite tool params, block execution, or require approval
- `after_tool_call` — observe tool results, errors, and duration
- **`tool_result_persist`** — rewrite the assistant message produced from a tool result
- **`before_message_write`** — inspect or block an in-progress message write (rare)

**Messages and delivery**

- **`inbound_claim`** — claim an inbound message before agent routing (synthetic replies)
- `message_received` — observe inbound content, sender, thread, and metadata
- **`message_sending`** — rewrite outbound content or cancel delivery
- `message_sent` — observe outbound delivery succe

_… [truncated; see https://docs.openclaw.ai/plugins/hooks for full content]_


---

## Manage plugins - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/manage-plugins>_

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

## Update plugins

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

## Uninstall plugins

```
openclaw plugins uninstall <plugin-id> --dry-run
openclaw plugins uninstall <plugin-id>
openclaw plugins uninstall <plugin-id> --keep-files
openclaw gateway restart
```

Uninstall removes the plugin’s config entry, plugin index record, allow/deny list
entries, and linked load paths when applicable. Managed install directories are
removed unless you pass `--keep-files`.

## Publish plugins

You can publish external plugins to [ClawHub](https://clawhub.ai/), npmjs.com, or
both.

### Publish to ClawHub

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

### Publish to npmjs.com

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

## Source choice

- **ClawHub**: use when you want OpenClaw-native discovery, scan summaries,
versions, and install hints.
- **npmjs.com**: use when you already ship JavaScript packages or need npm
dist-tags/private registry workflows.
- **Git**: use when you want to install directly from a branch, tag, or commit.
- **Local path**: use when you are d

_… [truncated; see https://docs.openclaw.ai/plugins/manage-plugins for full content]_


---

## Plugin manifest - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/manifest>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugin SDK reference

Plugin manifest

This page is for the **native OpenClaw plugin manifest** only.For compatible bundle layouts, see [Plugin bundles](https://docs.openclaw.ai/plugins/bundles).Compatible bundle formats use different manifest files:

- Codex bundle: `.codex-plugin/plugin.json`
- Claude bundle: `.claude-plugin/plugin.json` or the default Claude component
layout without a manifest
- Cursor bundle: `.cursor-plugin/plugin.json`

OpenClaw auto-detects those bundle layouts too, but they are not validated
against the `openclaw.plugin.json` schema described here.For compatible bundles, OpenClaw currently reads bundle metadata plus declared
skill roots, Claude command roots, Claude bundle `settings.json` defaults,
Claude bundle LSP defaults, and supported hook packs when the layout matches
OpenClaw runtime expectations.Every native OpenClaw plugin **must** ship a `openclaw.plugin.json` file in the
**plugin root**. OpenClaw uses this manifest to validate configuration
**without executing plugin code**. Missing or invalid manifests are treated as
plugin errors and block config validation.See the full plugin system guide: [Plugins](https://docs.openclaw.ai/tools/plugin).
For the native capability model and current external-compatibility guidance:
[Capability model](https://docs.openclaw.ai/plugins/architecture#public-capability-model).

## What this file does

`openclaw.plugin.json` is the metadata OpenClaw reads **before it loads your**
**plugin code**. Everything below must be cheap enough to inspect without booting
plugin runtime.**Use it for:**

- plugin identity, config validation, and config UI hints
- auth, onboarding, and setup metadata (alias, auto-enable, provider env vars, auth choices)
- activation hints for control-plane surfaces
- shorthand model-family ownership
- static capability-ownership snapshots (`contracts`)
- QA runner metadata the shared `openclaw qa` host can inspect
- channel-specific config metadata merged into catalog and validation surfaces

**Do not use it for:** registering runtime behavior, declaring code entrypoints,
or npm install metadata. Those belong in your plugin code and `package.json`.

## Minimal example

```
{
  "id": "voice-call",
  "configSchema": {
    "type": "object",
    "additionalProperties": false,
    "properties": {}
  }
}
```

## Rich example

```
{
  "id": "openrouter",
  "name": "OpenRouter",
  "description": "OpenRouter provider plugin",
  "version": "1.0.0",
  "providers": ["openrouter"],
  "modelSupport": {
    "modelPrefixes": ["router-"]
  },
  "modelIdNormalization": {
    "providers": {
      "openrouter": {
        "prefixWhenBare": "openrouter"
      }
    }
  },
  "providerEndpoints": [\
    {\
      "endpointClass": "openrouter",\
      "hostSuffixes": ["openrouter.ai"]\
    }\
  ],
  "providerRequest": {
    "providers": {
      "openrouter": {
        "family": "openrouter"
      }
    }
  },
  "cliBackends": ["openrouter-cli"],
  "syntheticAuthRefs": ["openrouter-cli"],
  "providerAuthEnvVars": {
    "openrouter": ["OPENROUTER_API_KEY"]
  },
  "providerAuthAliases": {
    "openrouter-coding": "openrouter"
  },
  "channelEnvVars": {
    "openrouter-chatops": ["OPENROUTER_CHATOPS_TOKEN"]
  },
  "providerAuthChoices": [\
    {\
      "provider": "openrouter",\
      "method": "api-key",\
      "choiceId": "openrouter-api-key",\
      "choiceLabel": "OpenRouter API key",\
      "groupId": "openrouter",\
      "groupLabel": "OpenRouter",\
      "optionKey": "openrouterApiKey",\
      "cliFlag": "--openrouter-api-key",\
      "cliOption": "--openrouter-api-key <key>",\
      "cliDescription": "OpenRouter API key",\
      "onboardingScopes": ["text-inference"]\
    }\
  ],
  "uiHints": {
    "apiKey": {
      "label": "API key",
      "placeholder": "sk-or-v1-...",
      "sensitive": true
    }
  },
  "configSchema": {
    "type": "object",
    "additionalProperties": false,
    "properties": {
      "apiKey":

_… [truncated; see https://docs.openclaw.ai/plugins/manifest for full content]_


---

## Memory LanceDB - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/memory-lancedb>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugins

Memory LanceDB

`memory-lancedb` is a bundled memory plugin that stores long-term memory in
LanceDB and uses embeddings for recall. It can automatically recall relevant
memories before a model turn and capture important facts after a response.Use it when you want a local vector database for memory, need an
OpenAI-compatible embedding endpoint, or want to keep a memory database outside
the default built-in memory store.

`memory-lancedb` is an active memory plugin. Enable it by selecting the memory
slot with `plugins.slots.memory = "memory-lancedb"`. Companion plugins such as
`memory-wiki` can run beside it, but only one plugin owns the active memory slot.

## Quick start

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

## Provider-backed embeddings

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

## Ollama embeddings

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

## OpenAI-compatible providers

Some OpenAI-compatible embedding providers reject the `encoding_format`
parameter, while others ignore it and always return `number[]` vectors.
`memory-lancedb` therefore omits `encoding_format` on embedding requests and
accepts either float

_… [truncated; see https://docs.openclaw.ai/plugins/memory-lancedb for full content]_


---

## Memory wiki - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/memory-wiki>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugins

Memory wiki

`memory-wiki` is a bundled plugin that turns durable memory into a compiled
knowledge vault.It does **not** replace the active memory plugin. The active memory plugin still
owns recall, promotion, indexing, and dreaming. `memory-wiki` sits beside it
and compiles durable knowledge into a navigable wiki with deterministic pages,
structured claims, provenance, dashboards, and machine-readable digests.Use it when you want memory to behave more like a maintained knowledge layer and
less like a pile of Markdown files.

## What it adds

- A dedicated wiki vault with deterministic page layout
- Structured claim and evidence metadata, not just prose
- Page-level provenance, confidence, contradictions, and open questions
- Compiled digests for agent/runtime consumers
- Wiki-native search/get/apply/lint tools
- Optional bridge mode that imports public artifacts from the active memory plugin
- Optional Obsidian-friendly render mode and CLI integration

## How it fits with memory

Think of the split like this:

| Layer | Owns |
| --- | --- |
| Active memory plugin (`memory-core`, QMD, Honcho, etc.) | Recall, semantic search, promotion, dreaming, memory runtime |
| `memory-wiki` | Compiled wiki pages, provenance-rich syntheses, dashboards, wiki-specific search/get/apply |

If the active memory plugin exposes shared recall artifacts, OpenClaw can search
both layers in one pass with `memory_search corpus=all`.When you need wiki-specific ranking, provenance, or direct page access, use the
wiki-native tools instead.

## Recommended hybrid pattern

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

## Vault modes

`memory-wiki` supports three vault modes:

### `isolated`

Own vault, own sources, no dependency on `memory-core`.Use this when you want the wiki to be its own curated knowledge store.

### `bridge`

Reads public memory artifacts and memory events from the active memory plugin
through public plugin SDK seams.Use this when you want the wiki to compile and organize the memory plugin’s
exported artifacts without reaching into private plugin internals.Bridge mode can index:

- exported memory artifacts
- dream reports
- daily notes
- memory root files
- memory event logs

### `unsafe-local`

Explicit same-machine escape hatch for local private paths.This mode is intentionally experimental and non-portable. Use it only when you
understand the trust boundary and specifically need local filesystem access that
bridge mode cannot provide.

## Vault layout

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

Managed content stays inside generated bloc

_… [truncated; see https://docs.openclaw.ai/plugins/memory-wiki for full content]_


---

## Plugin inventory - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/plugin-inventory>_

# Plugin inventory

This page is generated from `extensions/*/package.json`, `openclaw.plugin.json`,
and the root npm package `files` exclusions. Regenerate it with:

```
pnpm plugins:inventory:gen
```

## Definitions

- **Core npm package:** built into the `openclaw` npm package and available without a separate plugin install.
- **Official external package:** OpenClaw-maintained plugin omitted from the core npm package, kept in this official inventory, and installed on demand through ClawHub and/or npm.
- **Source checkout only:** repo-local plugin omitted from published npm artifacts and not advertised as an installable package.

Source checkouts are different from npm installs: after `pnpm install`, bundled
plugins load from `extensions/<id>` so local edits and package-local workspace
dependencies are available.

## Core npm package

| Plugin | Description | Distribution | Surface |
| --- | --- | --- | --- |
| [alibaba](https://docs.openclaw.ai/plugins/reference/alibaba) | Adds video generation provider support. | `@openclaw/alibaba-provider`<br>included in OpenClaw | contracts: videoGenerationProviders |
| [amazon-bedrock](https://docs.openclaw.ai/plugins/reference/amazon-bedrock) | Adds Amazon Bedrock model provider support to OpenClaw. | `@openclaw/amazon-bedrock-provider`<br>included in OpenClaw | providers: amazon-bedrock; contracts: memoryEmbeddingProviders |
| [amazon-bedrock-mantle](https://docs.openclaw.ai/plugins/reference/amazon-bedrock-mantle) | Adds Amazon Bedrock Mantle model provider support to OpenClaw. | `@openclaw/amazon-bedrock-mantle-provider`<br>included in OpenClaw | providers: amazon-bedrock-mantle |
| [anthropic](https://docs.openclaw.ai/plugins/reference/anthropic) | Adds Anthropic model provider support to OpenClaw. | `@openclaw/anthropic-provider`<br>included in OpenClaw | providers: anthropic; contracts: mediaUnderstandingProviders |
| [anthropic-vertex](https://docs.openclaw.ai/plugins/reference/anthropic-vertex) | Adds Anthropic Vertex model provider support to OpenClaw. | `@openclaw/anthropic-vertex-provider`<br>included in OpenClaw | providers: anthropic-vertex |
| [arcee](https://docs.openclaw.ai/plugins/reference/arcee) | Adds Arcee model provider support to OpenClaw. | `@openclaw/arcee-provider`<br>included in OpenClaw | providers: arcee |
| [azure-speech](https://docs.openclaw.ai/plugins/reference/azure-speech) | Azure AI Speech text-to-speech (MP3, native Ogg/Opus voice notes, PCM telephony). | `@openclaw/azure-speech`<br>included in OpenClaw | contracts: speechProviders |
| [bonjour](https://docs.openclaw.ai/plugins/reference/bonjour) | Advertise the local OpenClaw gateway over Bonjour/mDNS. | `@openclaw/bonjour`<br>included in OpenClaw | plugin |
| [browser](https://docs.openclaw.ai/plugins/reference/browser) | Adds agent-callable tools. | `@openclaw/browser-plugin`<br>included in OpenClaw | contracts: tools; skills |
| [byteplus](https://docs.openclaw.ai/plugins/reference/byteplus) | Adds BytePlus, BytePlus Plan model provider support to OpenClaw. | `@openclaw/byteplus-provider`<br>included in OpenClaw | providers: byteplus, byteplus-plan; contracts: videoGenerationProviders |
| [cerebras](https://docs.openclaw.ai/plugins/reference/cerebras) | Adds Cerebras model provider support to OpenClaw. | `@openclaw/cerebras-provider`<br>included in OpenClaw | providers: cerebras |
| [chutes](https://docs.openclaw.ai/plugins/reference/chutes) | Adds Chutes model provider support to OpenClaw. | `@openclaw/chutes-provider`<br>included in OpenClaw | providers: chutes |
| [cloudflare-ai-gateway](https://docs.openclaw.ai/plugins/reference/cloudflare-ai-gateway) | Adds Cloudflare AI Gateway model provider support to OpenClaw. | `@openclaw/cloudflare-ai-gateway-provider`<br>included in OpenClaw | providers: cloudflare-ai-gateway |
| [comfy](https://docs.openclaw.ai/plugins/reference/comfy) | Adds ComfyUI model provider support to OpenClaw. | `@openclaw/comfy-provider`<br>included in OpenClaw | provider

_… [truncated; see https://docs.openclaw.ai/plugins/plugin-inventory for full content]_


---

## Plugin reference - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/reference>_

# Plugin reference

This page is generated from `extensions/*/package.json` and
`openclaw.plugin.json`. Regenerate it with:

```
pnpm plugins:inventory:gen
```

| Plugin | Description | Distribution | Surface |
| --- | --- | --- | --- |
| [acpx](https://docs.openclaw.ai/plugins/reference/acpx) | Embedded ACP runtime backend with plugin-owned session and transport management. | `@openclaw/acpx`<br>npm; ClawHub | skills |
| [alibaba](https://docs.openclaw.ai/plugins/reference/alibaba) | Adds video generation provider support. | `@openclaw/alibaba-provider`<br>included in OpenClaw | contracts: videoGenerationProviders |
| [amazon-bedrock](https://docs.openclaw.ai/plugins/reference/amazon-bedrock) | Adds Amazon Bedrock model provider support to OpenClaw. | `@openclaw/amazon-bedrock-provider`<br>included in OpenClaw | providers: amazon-bedrock; contracts: memoryEmbeddingProviders |
| [amazon-bedrock-mantle](https://docs.openclaw.ai/plugins/reference/amazon-bedrock-mantle) | Adds Amazon Bedrock Mantle model provider support to OpenClaw. | `@openclaw/amazon-bedrock-mantle-provider`<br>included in OpenClaw | providers: amazon-bedrock-mantle |
| [anthropic](https://docs.openclaw.ai/plugins/reference/anthropic) | Adds Anthropic model provider support to OpenClaw. | `@openclaw/anthropic-provider`<br>included in OpenClaw | providers: anthropic; contracts: mediaUnderstandingProviders |
| [anthropic-vertex](https://docs.openclaw.ai/plugins/reference/anthropic-vertex) | Adds Anthropic Vertex model provider support to OpenClaw. | `@openclaw/anthropic-vertex-provider`<br>included in OpenClaw | providers: anthropic-vertex |
| [arcee](https://docs.openclaw.ai/plugins/reference/arcee) | Adds Arcee model provider support to OpenClaw. | `@openclaw/arcee-provider`<br>included in OpenClaw | providers: arcee |
| [azure-speech](https://docs.openclaw.ai/plugins/reference/azure-speech) | Azure AI Speech text-to-speech (MP3, native Ogg/Opus voice notes, PCM telephony). | `@openclaw/azure-speech`<br>included in OpenClaw | contracts: speechProviders |
| [bluebubbles](https://docs.openclaw.ai/plugins/reference/bluebubbles) | Adds the BlueBubbles channel surface for sending and receiving OpenClaw messages. | `@openclaw/bluebubbles`<br>npm; ClawHub | channels: bluebubbles |
| [bonjour](https://docs.openclaw.ai/plugins/reference/bonjour) | Advertise the local OpenClaw gateway over Bonjour/mDNS. | `@openclaw/bonjour`<br>included in OpenClaw | plugin |
| [brave](https://docs.openclaw.ai/plugins/reference/brave) | Adds web search provider support. | `@openclaw/brave-plugin`<br>npm; ClawHub | contracts: webSearchProviders |
| [browser](https://docs.openclaw.ai/plugins/reference/browser) | Adds agent-callable tools. | `@openclaw/browser-plugin`<br>included in OpenClaw | contracts: tools; skills |
| [byteplus](https://docs.openclaw.ai/plugins/reference/byteplus) | Adds BytePlus, BytePlus Plan model provider support to OpenClaw. | `@openclaw/byteplus-provider`<br>included in OpenClaw | providers: byteplus, byteplus-plan; contracts: videoGenerationProviders |
| [cerebras](https://docs.openclaw.ai/plugins/reference/cerebras) | Adds Cerebras model provider support to OpenClaw. | `@openclaw/cerebras-provider`<br>included in OpenClaw | providers: cerebras |
| [chutes](https://docs.openclaw.ai/plugins/reference/chutes) | Adds Chutes model provider support to OpenClaw. | `@openclaw/chutes-provider`<br>included in OpenClaw | providers: chutes |
| [cloudflare-ai-gateway](https://docs.openclaw.ai/plugins/reference/cloudflare-ai-gateway) | Adds Cloudflare AI Gateway model provider support to OpenClaw. | `@openclaw/cloudflare-ai-gateway-provider`<br>included in OpenClaw | providers: cloudflare-ai-gateway |
| [codex](https://docs.openclaw.ai/plugins/reference/codex) | Codex app-server harness and Codex-managed GPT model catalog. | `@openclaw/codex`<br>npm; ClawHub | providers: codex; contracts: mediaUnderstandingProviders, migrationProviders |
| [comfy](https://docs.openclaw.ai/pl

_… [truncated; see https://docs.openclaw.ai/plugins/reference for full content]_


---

## Building channel plugins - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/sdk-channel-plugins>_

[OpenClaw home page](https://docs.openclaw.ai/)

Building plugins

Building channel plugins

This guide walks through building a channel plugin that connects OpenClaw to a
messaging platform. By the end you will have a working channel with DM security,
pairing, reply threading, and outbound messaging.

If you have not built any OpenClaw plugin before, read
[Getting Started](https://docs.openclaw.ai/plugins/building-plugins) first for the basic package
structure and manifest setup.

## How channel plugins work

Channel plugins do not need their own send/edit/react tools. OpenClaw keeps one
shared `message` tool in core. Your plugin owns:

- **Config** — account resolution and setup wizard
- **Security** — DM policy and allowlists
- **Pairing** — DM approval flow
- **Session grammar** — how provider-specific conversation ids map to base chats, thread ids, and parent fallbacks
- **Outbound** — sending text, media, and polls to the platform
- **Threading** — how replies are threaded
- **Heartbeat typing** — optional typing/busy signals for heartbeat delivery targets

Core owns the shared message tool, prompt wiring, the outer session-key shape,
generic `:thread:` bookkeeping, and dispatch.If your channel supports typing indicators outside inbound replies, expose
`heartbeat.sendTyping(...)` on the channel plugin. Core calls it with the
resolved heartbeat delivery target before the heartbeat model run starts and
uses the shared typing keepalive/cleanup lifecycle. Add `heartbeat.clearTyping(...)`
when the platform needs an explicit stop signal.If your channel adds message-tool params that carry media sources, expose those
param names through `describeMessageTool(...).mediaSourceParams`. Core uses
that explicit list for sandbox path normalization and outbound media-access
policy, so plugins do not need shared-core special cases for provider-specific
avatar, attachment, or cover-image params.
Prefer returning an action-keyed map such as
`{ "set-profile": ["avatarUrl", "avatarPath"] }` so unrelated actions do not
inherit another action’s media args. A flat array still works for params that
are intentionally shared across every exposed action.If your platform stores extra scope inside conversation ids, keep that parsing
in the plugin with `messaging.resolveSessionConversation(...)`. That is the
canonical hook for mapping `rawId` to the base conversation id, optional thread
id, explicit `baseConversationId`, and any `parentConversationCandidates`.
When you return `parentConversationCandidates`, keep them ordered from the
narrowest parent to the broadest/base conversation.Use `openclaw/plugin-sdk/channel-route` when plugin code needs to normalize
route-like fields, compare a child thread with its parent route, or build a
stable dedupe key from `{ channel, to, accountId, threadId }`. The helper
normalizes numeric thread ids the same way core does, so plugins should prefer
it over ad hoc `String(threadId)` comparisons.
Plugins with provider-specific target grammar can inject their parser into
`resolveChannelRouteTargetWithParser(...)` and still get the same route target
shape and thread fallback semantics core uses.Bundled plugins that need the same parsing before the channel registry boots
can also expose a top-level `session-key-api.ts` file with a matching
`resolveSessionConversation(...)` export. Core uses that bootstrap-safe surface
only when the runtime plugin registry is not available yet.`messaging.resolveParentConversationCandidates(...)` remains available as a
legacy compatibility fallback when a plugin only needs parent fallbacks on top
of the generic/raw id. If both hooks exist, core uses
`resolveSessionConversation(...).parentConversationCandidates` first and only
falls back to `resolveParentConversationCandidates(...)` when the canonical hook
omits them.

## Approvals and channel capabilities

Most channel plugins do not need approval-specific code.

- Core owns same-chat `/approve`, shared approval button payloads, and generic

_… [truncated; see https://docs.openclaw.ai/plugins/sdk-channel-plugins for full content]_


---

## Plugin SDK overview - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/sdk-overview>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugin SDK reference

Plugin SDK overview

The plugin SDK is the typed contract between plugins and core. This page is the
reference for **what to import** and **what you can register**.

This page is for plugin authors using `openclaw/plugin-sdk/*` inside
OpenClaw. For external apps, scripts, dashboards, CI jobs, and IDE extensions
that want to run agents through the Gateway, use the
[OpenClaw App SDK](https://docs.openclaw.ai/concepts/openclaw-sdk) and the `@openclaw/sdk` package
instead.

Looking for a how-to guide instead? Start with [Building plugins](https://docs.openclaw.ai/plugins/building-plugins), use [Channel plugins](https://docs.openclaw.ai/plugins/sdk-channel-plugins) for channel plugins, [Provider plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins) for provider plugins, and [Plugin hooks](https://docs.openclaw.ai/plugins/hooks) for tool or lifecycle hook plugins.

## Import convention

Always import from a specific subpath:

```
import { definePluginEntry } from "openclaw/plugin-sdk/plugin-entry";
import { defineChannelPluginEntry } from "openclaw/plugin-sdk/channel-core";
```

Each subpath is a small, self-contained module. This keeps startup fast and
prevents circular dependency issues. For channel-specific entry/build helpers,
prefer `openclaw/plugin-sdk/channel-core`; keep `openclaw/plugin-sdk/core` for
the broader umbrella surface and shared helpers such as
`buildChannelConfigSchema`.For channel config, publish the channel-owned JSON Schema through
`openclaw.plugin.json#channelConfigs`. The `plugin-sdk/channel-config-schema`
subpath is for shared schema primitives and the generic builder. OpenClaw’s
bundled plugins use `plugin-sdk/bundled-channel-config-schema` for retained
bundled-channel schemas. Deprecated compatibility exports remain on
`plugin-sdk/channel-config-schema-legacy`; neither bundled schema subpath is a
pattern for new plugins.

Do not import provider- or channel-branded convenience seams (for example
`openclaw/plugin-sdk/slack`, `.../discord`, `.../signal`, `.../whatsapp`).
Bundled plugins compose generic SDK subpaths inside their own `api.ts` /
`runtime-api.ts` barrels; core consumers should either use those plugin-local
barrels or add a narrow generic SDK contract when a need is truly
cross-channel.A small set of bundled-plugin helper seams still appear in the generated export
map when they have tracked owner usage. They exist for bundled-plugin
maintenance only and are not recommended import paths for new third-party
plugins.`openclaw/plugin-sdk/discord` and `openclaw/plugin-sdk/telegram-account` are
also kept as deprecated compatibility facades for tracked owner usage. Do not
copy those import paths into new plugins; use injected runtime helpers and
generic channel SDK subpaths instead.

## Subpath reference

The plugin SDK is exposed as a set of narrow subpaths grouped by area (plugin
entry, channel, provider, auth, runtime, capability, memory, and reserved
bundled-plugin helpers). For the full catalog — grouped and linked — see
[Plugin SDK subpaths](https://docs.openclaw.ai/plugins/sdk-subpaths).The generated list of 200+ subpaths lives in `scripts/lib/plugin-sdk-entrypoints.json`.

## Registration API

The `register(api)` callback receives an `OpenClawPluginApi` object with these
methods:

### Capability registration

| Method | What it registers |
| --- | --- |
| `api.registerProvider(...)` | Text inference (LLM) |
| `api.registerAgentHarness(...)` | Experimental low-level agent executor |
| `api.registerCliBackend(...)` | Local CLI inference backend |
| `api.registerChannel(...)` | Messaging channel |
| `api.registerSpeechProvider(...)` | Text-to-speech / STT synthesis |
| `api.registerRealtimeTranscriptionProvider(...)` | Streaming realtime transcription |
| `api.registerRealtimeVoiceProvider(...)` | Duplex realtime voice sessions |
| `api.registerMediaUnderstandingProvider(...)` | Image/audio/video analysis |
| `api.regi

_… [truncated; see https://docs.openclaw.ai/plugins/sdk-overview for full content]_


---

## Plugin setup and config - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/sdk-setup>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugin SDK reference

Plugin setup and config

Reference for plugin packaging (`package.json` metadata), manifests (`openclaw.plugin.json`), setup entries, and config schemas.

**Looking for a walkthrough?** The how-to guides cover packaging in context: [Channel plugins](https://docs.openclaw.ai/plugins/sdk-channel-plugins#step-1-package-and-manifest) and [Provider plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-1-package-and-manifest).

## Package metadata

Your `package.json` needs an `openclaw` field that tells the plugin system what your plugin provides:

- Channel plugin

- Provider plugin / ClawHub baseline

```
{
  "name": "@myorg/openclaw-my-channel",
  "version": "1.0.0",
  "type": "module",
  "openclaw": {
    "extensions": ["./index.ts"],
    "setupEntry": "./setup-entry.ts",
    "channel": {
      "id": "my-channel",
      "label": "My Channel",
      "blurb": "Short description of the channel."
    }
  }
}
```

openclaw-clawhub-package.json

```
{
  "name": "@myorg/openclaw-my-plugin",
  "version": "1.0.0",
  "type": "module",
  "openclaw": {
    "extensions": ["./index.ts"],
    "compat": {
      "pluginApi": ">=2026.3.24-beta.2",
      "minGatewayVersion": "2026.3.24-beta.2"
    },
    "build": {
      "openclawVersion": "2026.3.24-beta.2",
      "pluginSdkVersion": "2026.3.24-beta.2"
    }
  }
}
```

If you publish the plugin externally on ClawHub, those `compat` and `build` fields are required. The canonical publish snippets live in `docs/snippets/plugin-publish/`.

### `openclaw` fields

[​](https://docs.openclaw.ai/plugins/sdk-setup#param-extensions)

extensions

string\[\]

Entry point files (relative to package root).

[​](https://docs.openclaw.ai/plugins/sdk-setup#param-setup-entry)

setupEntry

string

Lightweight setup-only entry (optional).

[​](https://docs.openclaw.ai/plugins/sdk-setup#param-channel)

channel

object

Channel catalog metadata for setup, picker, quickstart, and status surfaces.

[​](https://docs.openclaw.ai/plugins/sdk-setup#param-providers)

providers

string\[\]

Provider ids registered by this plugin.

[​](https://docs.openclaw.ai/plugins/sdk-setup#param-install)

install

object

Install hints: `npmSpec`, `localPath`, `defaultChoice`, `minHostVersion`, `expectedIntegrity`, `allowInvalidConfigRecovery`.

[​](https://docs.openclaw.ai/plugins/sdk-setup#param-startup)

startup

object

Startup behavior flags.

### `openclaw.channel`

`openclaw.channel` is cheap package metadata for channel discovery and setup surfaces before runtime loads.

| Field | Type | What it means |
| --- | --- | --- |
| `id` | `string` | Canonical channel id. |
| `label` | `string` | Primary channel label. |
| `selectionLabel` | `string` | Picker/setup label when it should differ from `label`. |
| `detailLabel` | `string` | Secondary detail label for richer channel catalogs and status surfaces. |
| `docsPath` | `string` | Docs path for setup and selection links. |
| `docsLabel` | `string` | Override label used for docs links when it should differ from the channel id. |
| `blurb` | `string` | Short onboarding/catalog description. |
| `order` | `number` | Sort order in channel catalogs. |
| `aliases` | `string[]` | Extra lookup aliases for channel selection. |
| `preferOver` | `string[]` | Lower-priority plugin/channel ids this channel should outrank. |
| `systemImage` | `string` | Optional icon/system-image name for channel UI catalogs. |
| `selectionDocsPrefix` | `string` | Prefix text before docs links in selection surfaces. |
| `selectionDocsOmitLabel` | `boolean` | Show the docs path directly instead of a labeled docs link in selection copy. |
| `selectionExtras` | `string[]` | Extra short strings appended in selection copy. |
| `markdownCapable` | `boolean` | Marks the channel as markdown-capable for outbound formatting decisions. |
| `exposure` | `object` | Channel visibility controls for setup, configured lists, and docs surfaces. |
| `

_… [truncated; see https://docs.openclaw.ai/plugins/sdk-setup for full content]_


---

## Skill workshop plugin - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/skill-workshop#skill-workshop-plugin>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugins

Skill workshop plugin

Skill Workshop is **experimental**. It is disabled by default, its capture
heuristics and reviewer prompts may change between releases, and automatic
writes should be used only in trusted workspaces after reviewing pending-mode
output first.Skill Workshop is procedural memory for workspace skills. It lets an agent turn
reusable workflows, user corrections, hard-won fixes, and recurring pitfalls
into `SKILL.md` files under:

```
<workspace>/skills/<skill-name>/SKILL.md
```

This is different from long-term memory:

- **Memory** stores facts, preferences, entities, and past context.
- **Skills** store reusable procedures the agent should follow on future tasks.
- **Skill Workshop** is the bridge from a useful turn to a durable workspace
skill, with safety checks and optional approval.

Skill Workshop is useful when the agent learns a procedure such as:

- how to validate externally sourced animated GIF assets
- how to replace screenshot assets and verify dimensions
- how to run a repo-specific QA scenario
- how to debug a recurring provider failure
- how to repair a stale local workflow note

It is not intended for:

- facts like “the user likes blue”
- broad autobiographical memory
- raw transcript archiving
- secrets, credentials, or hidden prompt text
- one-off instructions that will not repeat

## Default state

The bundled plugin is **experimental** and **disabled by default** unless it is
explicitly enabled in `plugins.entries.skill-workshop`.The plugin manifest does not set `enabledByDefault: true`. The `enabled: true`
default inside the plugin config schema applies only after the plugin entry has
already been selected and loaded.Experimental means:

- the plugin is supported enough for opt-in testing and dogfooding
- proposal storage, reviewer thresholds, and capture heuristics can evolve
- pending approval is the recommended starting mode
- auto apply is for trusted personal/workspace setups, not shared or hostile
input-heavy environments

## Enable

Minimal safe config:

```
{
  plugins: {
    entries: {
      "skill-workshop": {
        enabled: true,
        config: {
          autoCapture: true,
          approvalPolicy: "pending",
          reviewMode: "hybrid",
        },
      },
    },
  },
}
```

With this config:

- the `skill_workshop` tool is available
- explicit reusable corrections are queued as pending proposals
- threshold-based reviewer passes can propose skill updates
- no skill file is written until a pending proposal is applied

Use automatic writes only in trusted workspaces:

```
{
  plugins: {
    entries: {
      "skill-workshop": {
        enabled: true,
        config: {
          autoCapture: true,
          approvalPolicy: "auto",
          reviewMode: "hybrid",
        },
      },
    },
  },
}
```

`approvalPolicy: "auto"` still uses the same scanner and quarantine path. It
does not apply proposals with critical findings.

## Configuration

| Key | Default | Range / values | Meaning |
| --- | --- | --- | --- |
| `enabled` | `true` | boolean | Enables the plugin after the plugin entry is loaded. |
| `autoCapture` | `true` | boolean | Enables post-turn capture/review on successful agent turns. |
| `approvalPolicy` | `"pending"` | `"pending"`, `"auto"` | Queue proposals or write safe proposals automatically. |
| `reviewMode` | `"hybrid"` | `"off"`, `"heuristic"`, `"llm"`, `"hybrid"` | Chooses explicit correction capture, LLM reviewer, both, or neither. |
| `reviewInterval` | `15` | `1..200` | Run reviewer after this many successful turns. |
| `reviewMinToolCalls` | `8` | `1..500` | Run reviewer after this many observed tool calls. |
| `reviewTimeoutMs` | `45000` | `5000..180000` | Timeout for the embedded reviewer run. |
| `maxPending` | `50` | `1..200` | Max pending/quarantined proposals kept per workspace. |
| `maxSkillBytes` | `40000` | `1024..200000` | Max generated skill/support file size. |

Recommended prof

_… [truncated; see https://docs.openclaw.ai/plugins/skill-workshop#skill-workshop-plugin for full content]_


---

## Voice call plugin - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/voice-call>_

[OpenClaw home page](https://docs.openclaw.ai/)

Plugins

Voice call plugin

Voice calls for OpenClaw via a plugin. Supports outbound notifications,
multi-turn conversations, full-duplex realtime voice, streaming
transcription, and inbound calls with allowlist policies.**Current providers:**`twilio` (Programmable Voice + Media Streams),
`telnyx` (Call Control v2), `plivo` (Voice API + XML transfer + GetInput
speech), `mock` (dev/no network).

The Voice Call plugin runs **inside the Gateway process**. If you use a
remote Gateway, install and configure the plugin on the machine running
the Gateway, then restart the Gateway to load it.

## Quick start

1

[Navigate to header](https://docs.openclaw.ai/plugins/voice-call#)

Install the plugin

- From npm

- From a local folder (dev)

```
openclaw plugins install @openclaw/voice-call
```

```
PLUGIN_SRC=./path/to/local/voice-call-plugin
openclaw plugins install "$PLUGIN_SRC"
cd "$PLUGIN_SRC" && pnpm install
```

If npm reports the OpenClaw-owned package as deprecated, that package version
is from an older external package train; use a current packaged OpenClaw
build or the local folder path until a newer npm package is published.Restart the Gateway afterwards so the plugin loads.

2

[Navigate to header](https://docs.openclaw.ai/plugins/voice-call#)

Configure provider and webhook

Set config under `plugins.entries.voice-call.config` (see
[Configuration](https://docs.openclaw.ai/plugins/voice-call#configuration) below for the full shape). At minimum:
`provider`, provider credentials, `fromNumber`, and a publicly
reachable webhook URL.

3

[Navigate to header](https://docs.openclaw.ai/plugins/voice-call#)

Verify setup

```
openclaw voicecall setup
```

The default output is readable in chat logs and terminals. It checks
plugin enablement, provider credentials, webhook exposure, and that
only one audio mode (`streaming` or `realtime`) is active. Use
`--json` for scripts.

4

[Navigate to header](https://docs.openclaw.ai/plugins/voice-call#)

Smoke test

```
openclaw voicecall smoke
openclaw voicecall smoke --to "+15555550123"
```

Both are dry runs by default. Add `--yes` to actually place a short
outbound notify call:

```
openclaw voicecall smoke --to "+15555550123" --yes
```

For Twilio, Telnyx, and Plivo, setup must resolve to a **public webhook URL**.
If `publicUrl`, the tunnel URL, the Tailscale URL, or the serve fallback
resolves to loopback or private network space, setup fails instead of
starting a provider that cannot receive carrier webhooks.

## Configuration

If `enabled: true` but the selected provider is missing credentials,
Gateway startup logs a setup-incomplete warning with the missing keys and
skips starting the runtime. Commands, RPC calls, and agent tools still
return the exact missing provider configuration when used.

Voice-call credentials accept SecretRefs. `plugins.entries.voice-call.config.twilio.authToken`, `plugins.entries.voice-call.config.realtime.providers.*.apiKey`, `plugins.entries.voice-call.config.streaming.providers.*.apiKey`, and `plugins.entries.voice-call.config.tts.providers.*.apiKey` resolve through the standard SecretRef surface; see [SecretRef credential surface](https://docs.openclaw.ai/reference/secretref-credential-surface).

```
{
  plugins: {
    entries: {
      "voice-call": {
        enabled: true,
        config: {
          provider: "twilio", // or "telnyx" | "plivo" | "mock"
          fromNumber: "+15550001234", // or TWILIO_FROM_NUMBER for Twilio
          toNumber: "+15550005678",
          sessionScope: "per-phone", // per-phone | per-call
          numbers: {
            "+15550009999": {
              inboundGreeting: "Silver Fox Cards, how can I help?",
              responseSystemPrompt: "You are a concise baseball card specialist.",
              tts: {
                providers: {
                  openai: { voice: "alloy" },
                },
              },
            },
          },

          twilio: {

_… [truncated; see https://docs.openclaw.ai/plugins/voice-call for full content]_


---

## Webhooks plugin - OpenClaw

_Source: <https://docs.openclaw.ai/plugins/webhooks>_

# Webhooks (plugin)

The Webhooks plugin adds authenticated HTTP routes that bind external
automation to OpenClaw TaskFlows.Use it when you want a trusted system such as Zapier, n8n, a CI job, or an
internal service to create and drive managed TaskFlows without writing a custom
plugin first.

## Where it runs

The Webhooks plugin runs inside the Gateway process.If your Gateway runs on another machine, install and configure the plugin on
that Gateway host, then restart the Gateway.

## Configure routes

Set config under `plugins.entries.webhooks.config`:

```
{
  plugins: {
    entries: {
      webhooks: {
        enabled: true,
        config: {
          routes: {
            zapier: {
              path: "/plugins/webhooks/zapier",
              sessionKey: "agent:main:main",
              secret: {
                source: "env",
                provider: "default",
                id: "OPENCLAW_WEBHOOK_SECRET",
              },
              controllerId: "webhooks/zapier",
              description: "Zapier TaskFlow bridge",
            },
          },
        },
      },
    },
  },
}
```

Route fields:

- `enabled`: optional, defaults to `true`
- `path`: optional, defaults to `/plugins/webhooks/<routeId>`
- `sessionKey`: required session that owns the bound TaskFlows
- `secret`: required shared secret or SecretRef
- `controllerId`: optional controller id for created managed flows
- `description`: optional operator note

Supported `secret` inputs:

- Plain string
- SecretRef with `source: "env" | "file" | "exec"`

If a secret-backed route cannot resolve its secret at startup, the plugin skips
that route and logs a warning instead of exposing a broken endpoint.

## Security model

Each route is trusted to act with the TaskFlow authority of its configured
`sessionKey`.This means the route can inspect and mutate TaskFlows owned by that session, so
you should:

- Use a strong unique secret per route
- Prefer secret references over inline plaintext secrets
- Bind routes to the narrowest session that fits the workflow
- Expose only the specific webhook path you need

The plugin applies:

- Shared-secret authentication
- Request body size and timeout guards
- Fixed-window rate limiting
- In-flight request limiting
- Owner-bound TaskFlow access through `api.runtime.tasks.managedFlows.bindSession(...)`

## Request format

Send `POST` requests with:

- `Content-Type: application/json`
- `Authorization: Bearer <secret>` or `x-openclaw-webhook-secret: <secret>`

Example:

```
curl -X POST https://gateway.example.com/plugins/webhooks/zapier \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_SHARED_SECRET' \
  -d '{"action":"create_flow","goal":"Review inbound queue"}'
```

## Supported actions

The plugin currently accepts these JSON `action` values:

- `create_flow`
- `get_flow`
- `list_flows`
- `find_latest_flow`
- `resolve_flow`
- `get_task_summary`
- `set_waiting`
- `resume_flow`
- `finish_flow`
- `fail_flow`
- `request_cancel`
- `cancel_flow`
- `run_task`

### `create_flow`

Creates a managed TaskFlow for the route’s bound session.Example:

```
{
  "action": "create_flow",
  "goal": "Review inbound queue",
  "status": "queued",
  "notifyPolicy": "done_only"
}
```

### `run_task`

Creates a managed child task inside an existing managed TaskFlow.Allowed runtimes are:

- `subagent`
- `acp`

Example:

```
{
  "action": "run_task",
  "flowId": "flow_123",
  "runtime": "acp",
  "childSessionKey": "agent:main:acp:worker",
  "task": "Inspect the next message batch"
}
```

## Response shape

Successful responses return:

```
{
  "ok": true,
  "routeId": "zapier",
  "result": {}
}
```

Rejected requests return:

```
{
  "ok": false,
  "routeId": "zapier",
  "code": "not_found",
  "error": "TaskFlow not found.",
  "result": {}
}
```

The plugin intentionally scrubs owner/session metadata from webhook responses.

## Related docs

- [Plugin runtime SDK](https://docs.openclaw.ai/plugins/sdk-runtime)
- [Hooks and webhook

_… [truncated; see https://docs.openclaw.ai/plugins/webhooks for full content]_


---

## TOOLS.md template - OpenClaw

_Source: <https://docs.openclaw.ai/reference/templates/TOOLS>_

# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that’s unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

* * *

Add whatever helps you do your job. This is your cheat sheet.

## Related

- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)

[SOUL.md template](https://docs.openclaw.ai/reference/templates/SOUL) [USER template](https://docs.openclaw.ai/reference/templates/USER)

Ctrl+I


---

## Tools and plugins - OpenClaw

_Source: <https://docs.openclaw.ai/tools>_

[OpenClaw home page](https://docs.openclaw.ai/)

Overview

Tools and plugins

Everything the agent does beyond generating text happens through **tools**.
Tools are how the agent reads files, runs commands, browses the web, sends
messages, and interacts with devices.

## Tools, skills, and plugins

OpenClaw has three layers that work together:

1

[Navigate to header](https://docs.openclaw.ai/tools#)

Tools are what the agent calls

A tool is a typed function the agent can invoke (e.g. `exec`, `browser`,
`web_search`, `message`). OpenClaw ships a set of **built-in tools** and
plugins can register additional ones.The agent sees tools as structured function definitions sent to the model API.

2

[Navigate to header](https://docs.openclaw.ai/tools#)

Skills teach the agent when and how

A skill is a markdown file (`SKILL.md`) injected into the system prompt.
Skills give the agent context, constraints, and step-by-step guidance for
using tools effectively. Skills live in your workspace, in shared folders,
or ship inside plugins.[Skills reference](https://docs.openclaw.ai/tools/skills) \| [Creating skills](https://docs.openclaw.ai/tools/creating-skills)

3

[Navigate to header](https://docs.openclaw.ai/tools#)

Plugins package everything together

A plugin is a package that can register any combination of capabilities:
channels, model providers, tools, skills, speech, realtime transcription,
realtime voice, media understanding, image generation, video generation,
web fetch, web search, and more. Some plugins are **core** (shipped with
OpenClaw), others are **external** (published on npm by the community).[Install and configure plugins](https://docs.openclaw.ai/tools/plugin) \| [Build your own](https://docs.openclaw.ai/plugins/building-plugins)

## Built-in tools

These tools ship with OpenClaw and are available without installing any plugins:

| Tool | What it does | Page |
| --- | --- | --- |
| `exec` / `process` | Run shell commands, manage background processes | [Exec](https://docs.openclaw.ai/tools/exec), [Exec Approvals](https://docs.openclaw.ai/tools/exec-approvals) |
| `code_execution` | Run sandboxed remote Python analysis | [Code Execution](https://docs.openclaw.ai/tools/code-execution) |
| `browser` | Control a Chromium browser (navigate, click, screenshot) | [Browser](https://docs.openclaw.ai/tools/browser) |
| `web_search` / `x_search` / `web_fetch` | Search the web, search X posts, fetch page content | [Web](https://docs.openclaw.ai/tools/web), [Web Fetch](https://docs.openclaw.ai/tools/web-fetch) |
| `read` / `write` / `edit` | File I/O in the workspace |  |
| `apply_patch` | Multi-hunk file patches | [Apply Patch](https://docs.openclaw.ai/tools/apply-patch) |
| `message` | Send messages across all channels | [Agent Send](https://docs.openclaw.ai/tools/agent-send) |
| `canvas` | Drive node Canvas (present, eval, snapshot) |  |
| `nodes` | Discover and target paired devices |  |
| `cron` / `gateway` | Manage scheduled jobs; inspect, patch, restart, or update the gateway |  |
| `image` / `image_generate` | Analyze or generate images | [Image Generation](https://docs.openclaw.ai/tools/image-generation) |
| `music_generate` | Generate music tracks | [Music Generation](https://docs.openclaw.ai/tools/music-generation) |
| `video_generate` | Generate videos | [Video Generation](https://docs.openclaw.ai/tools/video-generation) |
| `tts` | One-shot text-to-speech conversion | [TTS](https://docs.openclaw.ai/tools/tts) |
| `sessions_*` / `subagents` / `agents_list` | Session management, status, and sub-agent orchestration | [Sub-agents](https://docs.openclaw.ai/tools/subagents) |
| `session_status` | Lightweight `/status`-style readback and session model override | [Session Tools](https://docs.openclaw.ai/concepts/session-tool) |

For image work, use `image` for analysis and `image_generate` for generation or editing. If you target `openai/*`, `google/*`, `fal/*`, or another non-default image provider, configure that provider’s a

_… [truncated; see https://docs.openclaw.ai/tools for full content]_


---

## ACP agents - OpenClaw

_Source: <https://docs.openclaw.ai/tools/acp-agents>_

[OpenClaw home page](https://docs.openclaw.ai/)

Agent coordination

ACP agents

[Agent Client Protocol (ACP)](https://agentclientprotocol.com/) sessions
let OpenClaw run external coding harnesses (for example Pi, Claude Code,
Cursor, Copilot, Droid, OpenClaw ACP, OpenCode, Gemini CLI, and other
supported ACPX harnesses) through an ACP backend plugin.Each ACP session spawn is tracked as a [background task](https://docs.openclaw.ai/automation/tasks).

**ACP is the external-harness path, not the default Codex path.** The
native Codex app-server plugin owns `/codex ...` controls and the
`agentRuntime.id: "codex"` embedded runtime; ACP owns
`/acp ...` controls and `sessions_spawn({ runtime: "acp" })` sessions.If you want Codex or Claude Code to connect as an external MCP client
directly to existing OpenClaw channel conversations, use
[`openclaw mcp serve`](https://docs.openclaw.ai/cli/mcp) instead of ACP.

## Which page do I want?

| You want to… | Use this | Notes |
| --- | --- | --- |
| Bind or control Codex in the current conversation | `/codex bind`, `/codex threads` | Native Codex app-server path when the `codex` plugin is enabled; includes bound chat replies, image forwarding, model/fast/permissions, stop, and steer controls. ACP is an explicit fallback |
| Run Claude Code, Gemini CLI, explicit Codex ACP, or another external harness _through_ OpenClaw | This page | Chat-bound sessions, `/acp spawn`, `sessions_spawn({ runtime: "acp" })`, background tasks, runtime controls |
| Expose an OpenClaw Gateway session _as_ an ACP server for an editor or client | [`openclaw acp`](https://docs.openclaw.ai/cli/acp) | Bridge mode. IDE/client talks ACP to OpenClaw over stdio/WebSocket |
| Reuse a local AI CLI as a text-only fallback model | [CLI Backends](https://docs.openclaw.ai/gateway/cli-backends) | Not ACP. No OpenClaw tools, no ACP controls, no harness runtime |

## Does this work out of the box?

Yes, after installing the official ACP runtime plugin:

```
openclaw plugins install @openclaw/acpx
openclaw config set plugins.entries.acpx.enabled true
```

Source checkouts can use the local `extensions/acpx` workspace plugin after
`pnpm install`. Run `/acp doctor` for a readiness check.OpenClaw only teaches agents about ACP spawning when ACP is **truly**
**usable**: ACP must be enabled, dispatch must not be disabled, the current
session must not be sandbox-blocked, and a runtime backend must be
loaded. If those conditions are not met, ACP plugin skills and
`sessions_spawn` ACP guidance stay hidden so the agent does not suggest
an unavailable backend.

First-run gotchas

- If `plugins.allow` is set, it is a restrictive plugin inventory and **must** include `acpx`; otherwise the installed ACP backend is intentionally blocked and `/acp doctor` reports the missing allowlist entry.
- The Codex ACP adapter is staged with the `acpx` plugin and launched locally when possible.
- Other target harness adapters may still be fetched on demand with `npx` the first time you use them.
- Vendor auth still has to exist on the host for that harness.
- If the host has no npm or network access, first-run adapter fetches fail until caches are pre-warmed or the adapter is installed another way.

Runtime prerequisites

ACP launches a real external harness process. OpenClaw owns routing,
background-task state, delivery, bindings, and policy; the harness
owns its provider login, model catalog, filesystem behavior, and
native tools.Before blaming OpenClaw, verify:

- `/acp doctor` reports an enabled, healthy backend.
- The target id is allowed by `acp.allowedAgents` when that allowlist is set.
- The harness command can start on the Gateway host.
- Provider auth is present for that harness (`claude`, `codex`, `gemini`, `opencode`, `droid`, etc.).
- The selected model exists for that harness — model ids are not portable across harnesses.
- The requested `cwd` exists and is accessible, or omit `cwd` and let the backend use its default.
- Permission mode matches the

_… [truncated; see https://docs.openclaw.ai/tools/acp-agents for full content]_


---

## ACP agents — setup - OpenClaw

_Source: <https://docs.openclaw.ai/tools/acp-agents-setup>_

[OpenClaw home page](https://docs.openclaw.ai/)

Agent coordination

ACP agents — setup

For the overview, operator runbook, and concepts, see [ACP agents](https://docs.openclaw.ai/tools/acp-agents).The sections below cover acpx harness config, plugin setup for the MCP bridges, and permission configuration.Use this page only when you are setting up the ACP/acpx route. For native Codex
app-server runtime config, use [Codex harness](https://docs.openclaw.ai/plugins/codex-harness). For
OpenAI API keys or Codex OAuth model-provider config, use
[OpenAI](https://docs.openclaw.ai/providers/openai).Codex has two OpenClaw routes:

| Route | Config/command | Setup page |
| --- | --- | --- |
| Native Codex app-server | `/codex ...`, `agentRuntime.id: "codex"` | [Codex harness](https://docs.openclaw.ai/plugins/codex-harness) |
| Explicit Codex ACP adapter | `/acp spawn codex`, `runtime: "acp", agentId: "codex"` | This page |

Prefer the native route unless you explicitly need ACP/acpx behavior.

## acpx harness support (current)

Current acpx built-in harness aliases:

- `claude`
- `codex`
- `copilot`
- `cursor` (Cursor CLI: `cursor-agent acp`)
- `droid`
- `gemini`
- `iflow`
- `kilocode`
- `kimi`
- `kiro`
- `openclaw`
- `opencode`
- `pi`
- `qwen`

When OpenClaw uses the acpx backend, prefer these values for `agentId` unless your acpx config defines custom agent aliases.
If your local Cursor install still exposes ACP as `agent acp`, override the `cursor` agent command in your acpx config instead of changing the built-in default.Direct acpx CLI usage can also target arbitrary adapters via `--agent <command>`, but that raw escape hatch is an acpx CLI feature (not the normal OpenClaw `agentId` path).Model control is adapter-capability dependent. Codex ACP model refs are
normalized by OpenClaw before startup. Other harnesses need ACP `models` plus
`session/set_model` support; if a harness exposes neither that ACP capability
nor its own startup model flag, OpenClaw/acpx cannot force a model selection.

## Required config

Core ACP baseline:

```
{
  acp: {
    enabled: true,
    // Optional. Default is true; set false to pause ACP dispatch while keeping /acp controls.
    dispatch: { enabled: true },
    backend: "acpx",
    defaultAgent: "codex",
    allowedAgents: [\
      "claude",\
      "codex",\
      "copilot",\
      "cursor",\
      "droid",\
      "gemini",\
      "iflow",\
      "kilocode",\
      "kimi",\
      "kiro",\
      "openclaw",\
      "opencode",\
      "pi",\
      "qwen",\
    ],
    maxConcurrentSessions: 8,
    stream: {
      coalesceIdleMs: 300,
      maxChunkChars: 1200,
    },
    runtime: {
      ttlMinutes: 120,
    },
  },
}
```

Thread binding config is channel-adapter specific. Example for Discord:

```
{
  session: {
    threadBindings: {
      enabled: true,
      idleHours: 24,
      maxAgeHours: 0,
    },
  },
  channels: {
    discord: {
      threadBindings: {
        enabled: true,
        spawnAcpSessions: true,
      },
    },
  },
}
```

If thread-bound ACP spawn does not work, verify the adapter feature flag first:

- Discord: `channels.discord.threadBindings.spawnAcpSessions=true`

Current-conversation binds do not require child-thread creation. They require an active conversation context and a channel adapter that exposes ACP conversation bindings.See [Configuration Reference](https://docs.openclaw.ai/gateway/configuration-reference).

## Plugin setup for acpx backend

Fresh installs ship the bundled `acpx` runtime plugin enabled by default, so ACP
usually works without a manual plugin install step.Start with:

```
/acp doctor
```

If you disabled `acpx`, denied it via `plugins.allow` / `plugins.deny`, or want
to switch to a local development checkout, use the explicit plugin path:

```
openclaw plugins install acpx
openclaw config set plugins.entries.acpx.enabled true
```

Local workspace install during development:

```
openclaw plugins install ./path/to/local/acpx-plugin
```

Then verify backen

_… [truncated; see https://docs.openclaw.ai/tools/acp-agents-setup for full content]_


---

## Brave search - OpenClaw

_Source: <https://docs.openclaw.ai/tools/brave-search>_

# Brave Search API

OpenClaw supports Brave Search API as a `web_search` provider.

## Get an API key

1. Create a Brave Search API account at [https://brave.com/search/api/](https://brave.com/search/api/)
2. In the dashboard, choose the **Search** plan and generate an API key.
3. Store the key in config or set `BRAVE_API_KEY` in the Gateway environment.

## Config example

```
{
  plugins: {
    entries: {
      brave: {
        config: {
          webSearch: {
            apiKey: "BRAVE_API_KEY_HERE",
            mode: "web", // or "llm-context"
            baseUrl: "https://api.search.brave.com", // optional proxy/base URL override
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "brave",
        maxResults: 5,
        timeoutSeconds: 30,
      },
    },
  },
}
```

Provider-specific Brave search settings now live under `plugins.entries.brave.config.webSearch.*`.
Legacy `tools.web.search.apiKey` still loads through the compatibility shim, but it is no longer the canonical config path.`webSearch.mode` controls the Brave transport:

- `web` (default): normal Brave web search with titles, URLs, and snippets
- `llm-context`: Brave LLM Context API with pre-extracted text chunks and sources for grounding

`webSearch.baseUrl` can point Brave requests at a trusted Brave-compatible proxy
or gateway. OpenClaw appends `/res/v1/web/search` or `/res/v1/llm/context` to
the configured base URL and keeps the base URL in the cache key. Public
endpoints must use `https://`; `http://` is accepted only for trusted loopback
or private-network proxy hosts.

## Tool parameters

[​](https://docs.openclaw.ai/tools/brave-search#param-query)

query

string

required

Search query.

[​](https://docs.openclaw.ai/tools/brave-search#param-count)

count

number

default:"5"

Number of results to return (1–10).

[​](https://docs.openclaw.ai/tools/brave-search#param-country)

country

string

2-letter ISO country code (e.g. `US`, `DE`).

[​](https://docs.openclaw.ai/tools/brave-search#param-language)

language

string

ISO 639-1 language code for search results (e.g. `en`, `de`, `fr`).

[​](https://docs.openclaw.ai/tools/brave-search#param-search-lang)

search\_lang

string

Brave search-language code (e.g. `en`, `en-gb`, `zh-hans`).

[​](https://docs.openclaw.ai/tools/brave-search#param-ui-lang)

ui\_lang

string

ISO language code for UI elements.

[​](https://docs.openclaw.ai/tools/brave-search#param-freshness)

freshness

'day' \| 'week' \| 'month' \| 'year'

Time filter — `day` is 24 hours.

[​](https://docs.openclaw.ai/tools/brave-search#param-date-after)

date\_after

string

Only results published after this date (`YYYY-MM-DD`).

[​](https://docs.openclaw.ai/tools/brave-search#param-date-before)

date\_before

string

Only results published before this date (`YYYY-MM-DD`).

**Examples:**

```
// Country and language-specific search
await web_search({
  query: "renewable energy",
  country: "DE",
  language: "de",
});

// Recent results (past week)
await web_search({
  query: "AI news",
  freshness: "week",
});

// Date range search
await web_search({
  query: "AI developments",
  date_after: "2024-01-01",
  date_before: "2024-06-30",
});
```

## Notes

- OpenClaw uses the Brave **Search** plan. If you have a legacy subscription (e.g. the original Free plan with 2,000 queries/month), it remains valid but does not include newer features like LLM Context or higher rate limits.
- Each Brave plan includes **$5/month in free credit** (renewing). The Search plan costs $5 per 1,000 requests, so the credit covers 1,000 queries/month. Set your usage limit in the Brave dashboard to avoid unexpected charges. See the [Brave API portal](https://brave.com/search/api/) for current plans.
- The Search plan includes the LLM Context endpoint and AI inference rights. Storing results to train or tune models requires a plan with explicit storage rights. See the Brave [Terms of Service](https://api-dashboard.search.brave

_… [truncated; see https://docs.openclaw.ai/tools/brave-search for full content]_


---

## Browser (OpenClaw-managed) - OpenClaw

_Source: <https://docs.openclaw.ai/tools/browser>_

[OpenClaw home page](https://docs.openclaw.ai/)

Web browser

Browser (OpenClaw-managed)

OpenClaw can run a **dedicated Chrome/Brave/Edge/Chromium profile** that the agent controls.
It is isolated from your personal browser and is managed through a small local
control service inside the Gateway (loopback only).Beginner view:

- Think of it as a **separate, agent-only browser**.
- The `openclaw` profile does **not** touch your personal browser profile.
- The agent can **open tabs, read pages, click, and type** in a safe lane.
- The built-in `user` profile attaches to your real signed-in Chrome session via Chrome MCP.

## What you get

- A separate browser profile named **openclaw** (orange accent by default).
- Deterministic tab control (list/open/focus/close).
- Agent actions (click/type/drag/select), snapshots, screenshots, PDFs.
- A bundled `browser-automation` skill that teaches agents the snapshot,
stable-tab, stale-ref, and manual-blocker recovery loop when the browser
plugin is enabled.
- Optional multi-profile support (`openclaw`, `work`, `remote`, …).

This browser is **not** your daily driver. It is a safe, isolated surface for
agent automation and verification.

## Quick start

```
openclaw browser --browser-profile openclaw doctor
openclaw browser --browser-profile openclaw doctor --deep
openclaw browser --browser-profile openclaw status
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw open https://example.com
openclaw browser --browser-profile openclaw snapshot
```

If you get “Browser disabled”, enable it in config (see below) and restart the
Gateway.If `openclaw browser` is missing entirely, or the agent says the browser tool
is unavailable, jump to [Missing browser command or tool](https://docs.openclaw.ai/tools/browser#missing-browser-command-or-tool).

## Plugin control

The default `browser` tool is a bundled plugin. Disable it to replace it with another plugin that registers the same `browser` tool name:

```
{
  plugins: {
    entries: {
      browser: {
        enabled: false,
      },
    },
  },
}
```

Defaults need both `plugins.entries.browser.enabled` **and**`browser.enabled=true`. Disabling only the plugin removes the `openclaw browser` CLI, `browser.request` gateway method, agent tool, and control service as one unit; your `browser.*` config stays intact for a replacement.Browser config changes require a Gateway restart so the plugin can re-register its service.

## Agent guidance

Tool-profile note: `tools.profile: "coding"` includes `web_search` and
`web_fetch`, but it does not include the full `browser` tool. If the agent or a
spawned sub-agent should use browser automation, add browser at the profile
stage:

```
{
  tools: {
    profile: "coding",
    alsoAllow: ["browser"],
  },
}
```

For a single agent, use `agents.list[].tools.alsoAllow: ["browser"]`.
`tools.subagents.tools.allow: ["browser"]` alone is not enough because sub-agent
policy is applied after profile filtering.The browser plugin ships two levels of agent guidance:

- The `browser` tool description carries the compact always-on contract: pick
the right profile, keep refs on the same tab, use `tabId`/labels for tab
targeting, and load the browser skill for multi-step work.
- The bundled `browser-automation` skill carries the longer operating loop:
check status/tabs first, label task tabs, snapshot before acting, resnapshot
after UI changes, recover stale refs once, and report login/2FA/captcha or
camera/microphone blockers as manual action instead of guessing.

Plugin-bundled skills are listed in the agent’s available skills when the
plugin is enabled. The full skill instructions are loaded on demand, so routine
turns do not pay the full token cost.

## Missing browser command or tool

If `openclaw browser` is unknown after an upgrade, `browser.request` is missing, or the agent reports the browser tool as unavailable, the usual cause is a `plugins.allow` list that omits `browser` and no

_… [truncated; see https://docs.openclaw.ai/tools/browser for full content]_


---

## Browser control API - OpenClaw

_Source: <https://docs.openclaw.ai/tools/browser-control>_

[OpenClaw home page](https://docs.openclaw.ai/)

Web browser

Browser control API

For setup, configuration, and troubleshooting, see [Browser](https://docs.openclaw.ai/tools/browser).
This page is the reference for the local control HTTP API, the `openclaw browser`
CLI, and scripting patterns (snapshots, refs, waits, debug flows).

## Control API (optional)

For local integrations only, the Gateway exposes a small loopback HTTP API:

- Status/start/stop: `GET /`, `POST /start`, `POST /stop`
- Tabs: `GET /tabs`, `POST /tabs/open`, `POST /tabs/focus`, `DELETE /tabs/:targetId`
- Snapshot/screenshot: `GET /snapshot`, `POST /screenshot`
- Actions: `POST /navigate`, `POST /act`
- Hooks: `POST /hooks/file-chooser`, `POST /hooks/dialog`
- Downloads: `POST /download`, `POST /wait/download`
- Permissions: `POST /permissions/grant`
- Debugging: `GET /console`, `POST /pdf`
- Debugging: `GET /errors`, `GET /requests`, `POST /trace/start`, `POST /trace/stop`, `POST /highlight`
- Network: `POST /response/body`
- State: `GET /cookies`, `POST /cookies/set`, `POST /cookies/clear`
- State: `GET /storage/:kind`, `POST /storage/:kind/set`, `POST /storage/:kind/clear`
- Settings: `POST /set/offline`, `POST /set/headers`, `POST /set/credentials`, `POST /set/geolocation`, `POST /set/media`, `POST /set/timezone`, `POST /set/locale`, `POST /set/device`

All endpoints accept `?profile=<name>`. `POST /start?headless=true` requests a
one-shot headless launch for local managed profiles without changing persisted
browser config; attach-only, remote CDP, and existing-session profiles reject
that override because OpenClaw does not launch those browser processes.If shared-secret gateway auth is configured, browser HTTP routes require auth too:

- `Authorization: Bearer <gateway token>`
- `x-openclaw-password: <gateway password>` or HTTP Basic auth with that password

Notes:

- This standalone loopback browser API does **not** consume trusted-proxy or
Tailscale Serve identity headers.
- If `gateway.auth.mode` is `none` or `trusted-proxy`, these loopback browser
routes do not inherit those identity-bearing modes; keep them loopback-only.

### `/act` error contract

`POST /act` uses a structured error response for route-level validation and
policy failures:

```
{ "error": "<message>", "code": "ACT_*" }
```

Current `code` values:

- `ACT_KIND_REQUIRED` (HTTP 400): `kind` is missing or unrecognized.
- `ACT_INVALID_REQUEST` (HTTP 400): action payload failed normalization or validation.
- `ACT_SELECTOR_UNSUPPORTED` (HTTP 400): `selector` was used with an unsupported action kind.
- `ACT_EVALUATE_DISABLED` (HTTP 403): `evaluate` (or `wait --fn`) is disabled by config.
- `ACT_TARGET_ID_MISMATCH` (HTTP 403): top-level or batched `targetId` conflicts with request target.
- `ACT_EXISTING_SESSION_UNSUPPORTED` (HTTP 501): action is not supported for existing-session profiles.

Other runtime failures may still return `{ "error": "<message>" }` without a
`code` field.

### Playwright requirement

Some features (navigate/act/AI snapshot/role snapshot, element screenshots,
PDF) require Playwright. If Playwright isn’t installed, those endpoints return
a clear 501 error.What still works without Playwright:

- ARIA snapshots
- Role-style accessibility snapshots (`--interactive`, `--compact`,
`--depth`, `--efficient`) when a per-tab CDP WebSocket is available. This is
a fallback for inspection and ref discovery; Playwright remains the primary
action engine.
- Page screenshots for the managed `openclaw` browser when a per-tab CDP
WebSocket is available
- Page screenshots for `existing-session` / Chrome MCP profiles
- `existing-session` ref-based screenshots (`--ref`) from snapshot output

What still needs Playwright:

- `navigate`
- `act`
- AI snapshots that depend on Playwright’s native AI snapshot format
- CSS-selector element screenshots (`--element`)
- full browser PDF export

Element screenshots also reject `--full-page`; the route returns `fullPage is not supported for eleme

_… [truncated; see https://docs.openclaw.ai/tools/browser-control for full content]_


---

## Browser login - OpenClaw

_Source: <https://docs.openclaw.ai/tools/browser-login>_

# Browser login + X/Twitter posting

## Manual login (recommended)

When a site requires login, **sign in manually** in the **host** browser profile (the openclaw browser).Do **not** give the model your credentials. Automated logins often trigger anti‑bot defenses and can lock the account.Back to the main browser docs: [Browser](https://docs.openclaw.ai/tools/browser).

## Which Chrome profile is used?

OpenClaw controls a **dedicated Chrome profile** (named `openclaw`, orange‑tinted UI). This is separate from your daily browser profile.For agent browser tool calls:

- Default choice: the agent should use its isolated `openclaw` browser.
- Use `profile="user"` only when existing logged-in sessions matter and the user is at the computer to click/approve any attach prompt.
- If you have multiple user-browser profiles, specify the profile explicitly instead of guessing.

Two easy ways to access it:

1. **Ask the agent to open the browser** and then log in yourself.
2. **Open it via CLI**:

```
openclaw browser start
openclaw browser open https://x.com
```

If you have multiple profiles, pass `--browser-profile <name>` (the default is `openclaw`).

## X/Twitter: recommended flow

- **Read/search/threads:** use the **host** browser (manual login).
- **Post updates:** use the **host** browser (manual login).

## Sandboxing + host browser access

Sandboxed browser sessions are **more likely** to trigger bot detection. For X/Twitter (and other strict sites), prefer the **host** browser.If the agent is sandboxed, the browser tool defaults to the sandbox. To allow host control:

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",
        browser: {
          allowHostControl: true,
        },
      },
    },
  },
}
```

Then target the host browser:

```
openclaw browser open https://x.com --browser-profile openclaw --target host
```

Or disable sandboxing for the agent that posts updates.

## Related

- [Browser](https://docs.openclaw.ai/tools/browser)
- [Browser Linux troubleshooting](https://docs.openclaw.ai/tools/browser-linux-troubleshooting)
- [Browser WSL2 troubleshooting](https://docs.openclaw.ai/tools/browser-wsl2-windows-remote-cdp-troubleshooting)

[Browser control API](https://docs.openclaw.ai/tools/browser-control) [Browser troubleshooting](https://docs.openclaw.ai/tools/browser-linux-troubleshooting)

Ctrl+I


---

## https://docs.openclaw.ai/tools/browser.md

_Source: <https://docs.openclaw.ai/tools/browser.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Browser (OpenClaw-managed)

OpenClaw can run a \*\*dedicated Chrome/Brave/Edge/Chromium profile\*\* that the agent controls.
It is isolated from your personal browser and is managed through a small local
control service inside the Gateway (loopback only).

Beginner view:

\\* Think of it as a \*\*separate, agent-only browser\*\*.
\\* The \`openclaw\` profile does \*\*not\*\* touch your personal browser profile.
\\* The agent can \*\*open tabs, read pages, click, and type\*\* in a safe lane.
\\* The built-in \`user\` profile attaches to your real signed-in Chrome session via Chrome MCP.

\## What you get

\\* A separate browser profile named \*\*openclaw\*\* (orange accent by default).
\\* Deterministic tab control (list/open/focus/close).
\\* Agent actions (click/type/drag/select), snapshots, screenshots, PDFs.
\\* A bundled \`browser-automation\` skill that teaches agents the snapshot,
 stable-tab, stale-ref, and manual-blocker recovery loop when the browser
 plugin is enabled.
\\* Optional multi-profile support (\`openclaw\`, \`work\`, \`remote\`, ...).

This browser is \*\*not\*\* your daily driver. It is a safe, isolated surface for
agent automation and verification.

\## Quick start

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw browser --browser-profile openclaw doctor
openclaw browser --browser-profile openclaw doctor --deep
openclaw browser --browser-profile openclaw status
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw open https://example.com
openclaw browser --browser-profile openclaw snapshot
\`\`\`

If you get “Browser disabled”, enable it in config (see below) and restart the
Gateway.

If \`openclaw browser\` is missing entirely, or the agent says the browser tool
is unavailable, jump to \[Missing browser command or tool\](/tools/browser#missing-browser-command-or-tool).

\## Plugin control

The default \`browser\` tool is a bundled plugin. Disable it to replace it with another plugin that registers the same \`browser\` tool name:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 plugins: {
 entries: {
 browser: {
 enabled: false,
 },
 },
 },
}
\`\`\`

Defaults need both \`plugins.entries.browser.enabled\` \*\*and\*\* \`browser.enabled=true\`. Disabling only the plugin removes the \`openclaw browser\` CLI, \`browser.request\` gateway method, agent tool, and control service as one unit; your \`browser.\*\` config stays intact for a replacement.

Browser config changes require a Gateway restart so the plugin can re-register its service.

\## Agent guidance

Tool-profile note: \`tools.profile: "coding"\` includes \`web\_search\` and
\`web\_fetch\`, but it does not include the full \`browser\` tool. If the agent or a
spawned sub-agent should use browser automation, add browser at the profile
stage:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 tools: {
 profile: "coding",
 alsoAllow: \["browser"\],
 },
}
\`\`\`

For a single agent, use \`agents.list\[\].tools.alsoAllow: \["browser"\]\`.
\`tools.subagents.tools.allow: \["browser"\]\` alone is not enough because sub-agent
policy is applied after profile filtering.

The browser plugin ships two levels of agent guidance:

\\* The \`browser\` tool description carries the compact always-on contract: pick
 the right profile, keep refs on the same tab, use \`tabId\`/labels for tab
 targeting, and load the browser skill for multi-step work.
\\* The bundled \`browser-automation\` skill carries the longer operating loop:
 check status/tabs first, label task tabs, snapshot before acting, resnapshot
 after UI changes, recover stale refs once, and report login/2FA/captcha or
 camera/microphone blockers as manual action instead of guessing.

Plugin-bundled skills are listed in the agent's available sk

_… [truncated; see https://docs.openclaw.ai/tools/browser.md for full content]_


---

## ClawHub - OpenClaw

_Source: <https://docs.openclaw.ai/tools/clawhub>_

# or
pnpm add -g clawhub
```

## Native OpenClaw flows

- Skills

- Plugins

```
openclaw skills search "calendar"
openclaw skills install <skill-slug>
openclaw skills update --all
```

Native `openclaw` commands install into your active workspace and
persist source metadata so later `update` calls can stay on ClawHub.

```
openclaw plugins search "calendar"
openclaw plugins install clawhub:<package>
openclaw plugins update --all
```

`plugins search` queries the ClawHub plugin catalog and prints install-ready
package names. Bare npm-safe plugin specs are also tried against ClawHub
before npm:

```
openclaw plugins install openclaw-codex-app-server
```

Use `npm:<package>` when you want npm-only resolution without a
ClawHub lookup:

```
openclaw plugins install npm:openclaw-codex-app-server
```

Plugin installs validate advertised `pluginApi` and
`minGatewayVersion` compatibility before archive install runs, so
incompatible hosts fail closed early instead of partially installing
the package. When a package version publishes a ClawPack artifact,
OpenClaw prefers that artifact, verifies the ClawHub digest header and
downloaded bytes, and records the ClawPack digest metadata for later
updates. Older package versions without ClawPack metadata still use the
legacy package archive verification path.

`openclaw plugins install clawhub:...` only accepts installable plugin
families. If a ClawHub package is actually a skill, OpenClaw stops and
points you at `openclaw skills install <slug>` instead.Anonymous ClawHub plugin installs also fail closed for private packages.
Community or other non-official channels can still install, but OpenClaw
warns so operators can review source and verification before enabling
them.

## What ClawHub is

- A public registry for OpenClaw skills and plugins.
- A versioned store of skill bundles and metadata.
- A discovery surface for search, tags, and usage signals.

A typical skill is a versioned bundle of files that includes:

- A `SKILL.md` file with the primary description and usage.
- Optional configs, scripts, or supporting files used by the skill.
- Metadata such as tags, summary, and install requirements.

ClawHub uses metadata to power discovery and safely expose skill
capabilities. The registry tracks usage signals (stars, downloads) to
improve ranking and visibility. Each publish creates a new semver
version, and the registry keeps version history so users can audit
changes.

## Workspace and skill loading

The separate `clawhub` CLI also installs skills into `./skills` under
your current working directory. If an OpenClaw workspace is configured,
`clawhub` falls back to that workspace unless you override `--workdir`
(or `CLAWHUB_WORKDIR`). OpenClaw loads workspace skills from
`<workspace>/skills` and picks them up in the **next** session.If you already use `~/.openclaw/skills` or bundled skills, workspace
skills take precedence. For more detail on how skills are loaded,
shared, and gated, see [Skills](https://docs.openclaw.ai/tools/skills).

## Service features

| Feature | Notes |
| --- | --- |
| Public browsing | Skills and their `SKILL.md` content are publicly viewable. |
| Search | Embedding-powered (vector search), not just keywords. |
| Versioning | Semver, changelogs, and tags (including `latest`). |
| Downloads | Zip per version. |
| Stars and comments | Community feedback. |
| Security scan summaries | Detail pages show the latest scan state before install or download. |
| Scanner detail pages | VirusTotal, ClawScan, and static-analysis results have deep links. |
| Owner recovery dashboard | Publishers can see scan-held owned content from `/dashboard`. |
| Owner-requested rescans | Owners can request limited rescans for false-positive recovery. |
| Moderation | Approvals and audits. |
| CLI-friendly API | Suitable for automation and scripting. |

## Security and moderation

ClawHub is open by default — anyone can upload skills, but a GitHub
account must be **at least one week old** to pu

_… [truncated; see https://docs.openclaw.ai/tools/clawhub for full content]_


---

## Creating skills - OpenClaw

_Source: <https://docs.openclaw.ai/tools/creating-skills>_

# Hello World Skill

When the user asks for a greeting, use the `echo` tool to say
"Hello from your custom skill!".
```

Use hyphen-case with lowercase letters, digits, and hyphens for the skill
`name`. Keep the folder name and frontmatter `name` aligned.

3

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Add tools (optional)

You can define custom tool schemas in the frontmatter or instruct the agent
to use existing system tools (like `exec` or `browser`). Skills can also
ship inside plugins alongside the tools they document.

4

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Load the skill

Start a new session so OpenClaw picks up the skill:

```
# From chat
/new

# Or restart the gateway
openclaw gateway restart
```

Verify the skill loaded:

```
openclaw skills list
```

5

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Test it

Send a message that should trigger the skill:

```
openclaw agent --message "give me a greeting"
```

Or just chat with the agent and ask for a greeting.

## Skill metadata reference

The YAML frontmatter supports these fields:

| Field | Required | Description |
| --- | --- | --- |
| `name` | Yes | Unique identifier using lowercase letters, digits, and hyphens |
| `description` | Yes | One-line description shown to the agent |
| `metadata.openclaw.os` | No | OS filter (`["darwin"]`, `["linux"]`, etc.) |
| `metadata.openclaw.requires.bins` | No | Required binaries on PATH |
| `metadata.openclaw.requires.config` | No | Required config keys |

## Best practices

- **Be concise** — instruct the model on _what_ to do, not how to be an AI
- **Safety first** — if your skill uses `exec`, ensure prompts don’t allow arbitrary command injection from untrusted input
- **Test locally** — use `openclaw agent --message "..."` to test before sharing
- **Use ClawHub** — browse and contribute skills at [ClawHub](https://clawhub.ai/)

## Where skills live

| Location | Precedence | Scope |
| --- | --- | --- |
| `\<workspace\>/skills/` | Highest | Per-agent |
| `\<workspace\>/.agents/skills/` | High | Per-workspace agent |
| `~/.agents/skills/` | Medium | Shared agent profile |
| `~/.openclaw/skills/` | Medium | Shared (all agents) |
| Bundled (shipped with OpenClaw) | Low | Global |
| `skills.load.extraDirs` | Lowest | Custom shared folders |

## Related

- [Skills reference](https://docs.openclaw.ai/tools/skills) — loading, precedence, and gating rules
- [Skills config](https://docs.openclaw.ai/tools/skills-config) — `skills.*` config schema
- [ClawHub](https://docs.openclaw.ai/tools/clawhub) — public skill registry
- [Building Plugins](https://docs.openclaw.ai/plugins/building-plugins) — plugins can ship skills

[Skills](https://docs.openclaw.ai/tools/skills) [Skills config](https://docs.openclaw.ai/tools/skills-config)

Ctrl+I


---

## Creating skills - OpenClaw

_Source: <https://docs.openclaw.ai/tools/creating-skills#creating-skills>_

# Hello World Skill

When the user asks for a greeting, use the `echo` tool to say
"Hello from your custom skill!".
```

Use hyphen-case with lowercase letters, digits, and hyphens for the skill
`name`. Keep the folder name and frontmatter `name` aligned.

3

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Add tools (optional)

You can define custom tool schemas in the frontmatter or instruct the agent
to use existing system tools (like `exec` or `browser`). Skills can also
ship inside plugins alongside the tools they document.

4

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Load the skill

Start a new session so OpenClaw picks up the skill:

```
# From chat
/new

# Or restart the gateway
openclaw gateway restart
```

Verify the skill loaded:

```
openclaw skills list
```

5

[Navigate to header](https://docs.openclaw.ai/tools/creating-skills#)

Test it

Send a message that should trigger the skill:

```
openclaw agent --message "give me a greeting"
```

Or just chat with the agent and ask for a greeting.

## Skill metadata reference

The YAML frontmatter supports these fields:

| Field | Required | Description |
| --- | --- | --- |
| `name` | Yes | Unique identifier using lowercase letters, digits, and hyphens |
| `description` | Yes | One-line description shown to the agent |
| `metadata.openclaw.os` | No | OS filter (`["darwin"]`, `["linux"]`, etc.) |
| `metadata.openclaw.requires.bins` | No | Required binaries on PATH |
| `metadata.openclaw.requires.config` | No | Required config keys |

## Best practices

- **Be concise** — instruct the model on _what_ to do, not how to be an AI
- **Safety first** — if your skill uses `exec`, ensure prompts don’t allow arbitrary command injection from untrusted input
- **Test locally** — use `openclaw agent --message "..."` to test before sharing
- **Use ClawHub** — browse and contribute skills at [ClawHub](https://clawhub.ai/)

## Where skills live

| Location | Precedence | Scope |
| --- | --- | --- |
| `\<workspace\>/skills/` | Highest | Per-agent |
| `\<workspace\>/.agents/skills/` | High | Per-workspace agent |
| `~/.agents/skills/` | Medium | Shared agent profile |
| `~/.openclaw/skills/` | Medium | Shared (all agents) |
| Bundled (shipped with OpenClaw) | Low | Global |
| `skills.load.extraDirs` | Lowest | Custom shared folders |

## Related

- [Skills reference](https://docs.openclaw.ai/tools/skills) — loading, precedence, and gating rules
- [Skills config](https://docs.openclaw.ai/tools/skills-config) — `skills.*` config schema
- [ClawHub](https://docs.openclaw.ai/tools/clawhub) — public skill registry
- [Building Plugins](https://docs.openclaw.ai/plugins/building-plugins) — plugins can ship skills

[Skills](https://docs.openclaw.ai/tools/skills) [Skills config](https://docs.openclaw.ai/tools/skills-config)

Ctrl+I


---

_27 additional pages omitted to keep this file ≤ 146 KB. See https://docs.openclaw.ai for full content._
