---
source_url: https://docs.openclaw.ai/plugins/building-plugins
title: "Building plugins - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/plugins/building-plugins#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Building plugins

Building plugins

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Prerequisites](https://docs.openclaw.ai/plugins/building-plugins#prerequisites)
- [What kind of plugin?](https://docs.openclaw.ai/plugins/building-plugins#what-kind-of-plugin)
- [Quick start: tool plugin](https://docs.openclaw.ai/plugins/building-plugins#quick-start-tool-plugin)
- [Plugin capabilities](https://docs.openclaw.ai/plugins/building-plugins#plugin-capabilities)
- [Registering agent tools](https://docs.openclaw.ai/plugins/building-plugins#registering-agent-tools)
- [Registering CLI commands](https://docs.openclaw.ai/plugins/building-plugins#registering-cli-commands)
- [Import conventions](https://docs.openclaw.ai/plugins/building-plugins#import-conventions)
- [Pre-submission checklist](https://docs.openclaw.ai/plugins/building-plugins#pre-submission-checklist)
- [Beta release testing](https://docs.openclaw.ai/plugins/building-plugins#beta-release-testing)
- [Next steps](https://docs.openclaw.ai/plugins/building-plugins#next-steps)
- [Related](https://docs.openclaw.ai/plugins/building-plugins#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Plugins extend OpenClaw with new capabilities: channels, model providers,
speech, realtime transcription, realtime voice, media understanding, image
generation, video generation, web fetch, web search, agent tools, or any
combination.You do not need to add your plugin to the OpenClaw repository. Publish to
[ClawHub](https://docs.openclaw.ai/tools/clawhub) and users install with
`openclaw plugins install clawhub:<package-name>`. Bare package specs still
install from npm during the launch cutover.

## [​](https://docs.openclaw.ai/plugins/building-plugins\#prerequisites)  Prerequisites

- Node >= 22 and a package manager (npm or pnpm)
- Familiarity with TypeScript (ESM)
- For in-repo plugins: repository cloned and `pnpm install` done. Source
checkout plugin development is pnpm-only because OpenClaw loads bundled
plugins from the `extensions/*` workspace packages.

## [​](https://docs.openclaw.ai/plugins/building-plugins\#what-kind-of-plugin)  What kind of plugin?

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

## [​](https://docs.openclaw.ai/plugins/building-plugins\#quick-start-tool-plugin)  Quick start: tool plugin

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
openclaw plugins install clawhub:@myorg/openclaw-my-plugin
```

Bare package specs like `@myorg/openclaw-my-plugin` install from npm during
the launch cutover. Use `clawhub:` when you want ClawHub resolution.**In-repo plugins:** place under the bundled plugin workspace tree — automatically discovered.

```
pnpm test -- <bundled-plugin-root>/my-plugin/
```

## [​](https://docs.openclaw.ai/plugins/building-plugins\#plugin-capabilities)  Plugin capabilities

A single plugin can register any number of capabilities via the `api` object:

| Capability | Registration method | Detailed guide |
| --- | --- | --- |
| Text inference (LLM) | `api.registerProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins) |
| CLI inference backend | `api.registerCliBackend(...)` | [CLI Backends](https://docs.openclaw.ai/gateway/cli-backends) |
| Channel / messaging | `api.registerChannel(...)` | [Channel Plugins](https://docs.openclaw.ai/plugins/sdk-channel-plugins) |
| Speech (TTS/STT) | `api.registerSpeechProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-5-add-extra-capabilities) |
| Realtime transcription | `api.registerRealtimeTranscriptionProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-5-add-extra-capabilities) |
| Realtime voice | `api.registerRealtimeVoiceProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-5-add-extra-capabilities) |
| Media understanding | `api.registerMediaUnderstandingProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-5-add-extra-capabilities) |
| Image generation | `api.registerImageGenerationProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-5-add-extra-capabilities) |
| Music generation | `api.registerMusicGenerationProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-5-add-extra-capabilities) |
| Video generation | `api.registerVideoGenerationProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-5-add-extra-capabilities) |
| Web fetch | `api.registerWebFetchProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-5-add-extra-capabilities) |
| Web search | `api.registerWebSearchProvider(...)` | [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins#step-5-add-extra-capabilities) |
| Tool-result middleware | `api.registerAgentToolResultMiddleware(...)` | [SDK Overview](https://docs.openclaw.ai/plugins/sdk-overview#registration-api) |
| Agent tools | `api.registerTool(...)` | Below |
| Custom commands | `api.registerCommand(...)` | [Entry Points](https://docs.openclaw.ai/plugins/sdk-entrypoints) |
| Plugin hooks | `api.on(...)` | [Plugin hooks](https://docs.openclaw.ai/plugins/hooks) |
| Internal event hooks | `api.registerHook(...)` | [Entry Points](https://docs.openclaw.ai/plugins/sdk-entrypoints) |
| HTTP routes | `api.registerHttpRoute(...)` | [Internals](https://docs.openclaw.ai/plugins/architecture-internals#gateway-http-routes) |
| CLI subcommands | `api.registerCli(...)` | [Entry Points](https://docs.openclaw.ai/plugins/sdk-entrypoints) |

For the full registration API, see [SDK Overview](https://docs.openclaw.ai/plugins/sdk-overview#registration-api).Bundled plugins can use `api.registerAgentToolResultMiddleware(...)` when they
need async tool-result rewriting before the model sees the output. Declare the
targeted runtimes in `contracts.agentToolResultMiddleware`, for example
`["pi", "codex"]`. This is a trusted bundled-plugin seam; external
plugins should prefer regular OpenClaw plugin hooks unless OpenClaw grows an
explicit trust policy for this capability.If your plugin registers custom gateway RPC methods, keep them on a
plugin-specific prefix. Core admin namespaces (`config.*`,
`exec.approvals.*`, `wizard.*`, `update.*`) stay reserved and always resolve to
`operator.admin`, even if a plugin asks for a narrower scope.Hook guard semantics to keep in mind:

- `before_tool_call`: `{ block: true }` is terminal and stops lower-priority handlers.
- `before_tool_call`: `{ block: false }` is treated as no decision.
- `before_tool_call`: `{ requireApproval: true }` pauses agent execution and prompts the user for approval via the exec approval overlay, Telegram buttons, Discord interactions, or the `/approve` command on any channel.
- `before_install`: `{ block: true }` is terminal and stops lower-priority handlers.
- `before_install`: `{ block: false }` is treated as no decision.
- `message_sending`: `{ cancel: true }` is terminal and stops lower-priority handlers.
- `message_sending`: `{ cancel: false }` is treated as no decision.
- `message_received`: prefer the typed `threadId` field when you need inbound thread/topic routing. Keep `metadata` for channel-specific extras.
- `message_sending`: prefer typed `replyToId` / `threadId` routing fields over channel-specific metadata keys.

The `/approve` command handles both exec and plugin approvals with bounded fallback: when an exec approval id is not found, OpenClaw retries the same id through plugin approvals. Plugin approval forwarding can be configured independently via `approvals.plugin` in config.If custom approval plumbing needs to detect that same bounded fallback case,
prefer `isApprovalNotFoundError` from `openclaw/plugin-sdk/error-runtime`
instead of matching approval-expiry strings manually.See [Plugin hooks](https://docs.openclaw.ai/plugins/hooks) for examples and the hook reference.

## [​](https://docs.openclaw.ai/plugins/building-plugins\#registering-agent-tools)  Registering agent tools

Tools are typed functions the LLM can call. They can be required (always
available) or optional (user opt-in):

```
register(api) {
  // Required tool — always available
  api.registerTool({
    name: "my_tool",
    description: "Do a thing",
    parameters: Type.Object({ input: Type.String() }),
    async execute(_id, params) {
      return { content: [{ type: "text", text: params.input }] };
    },
  });

  // Optional tool — user must add to allowlist
  api.registerTool(
    {
      name: "workflow_tool",
      description: "Run a workflow",
      parameters: Type.Object({ pipeline: Type.String() }),
      async execute(_id, params) {
        return { content: [{ type: "text", text: params.pipeline }] };
      },
    },
    { optional: true },
  );
}
```

Every tool registered with `api.registerTool(...)` must also be declared in the
plugin manifest:

```
{
  "contracts": {
    "tools": ["my_tool", "workflow_tool"]
  }
}
```

OpenClaw captures and caches the validated descriptor from the registered tool,
so plugins do not duplicate `description` or schema data in the manifest. The
manifest contract only declares ownership and discovery; execution still calls
the live registered tool implementation.Users enable optional tools in config:

```
{
  tools: { allow: ["workflow_tool"] },
}
```

- Tool names must not clash with core tools (conflicts are skipped)
- Tools with malformed registration objects, including missing `parameters`, are skipped and reported in plugin diagnostics instead of breaking agent runs
- Use `optional: true` for tools with side effects or extra binary requirements
- Users can enable all tools from a plugin by adding the plugin id to `tools.allow`

## [​](https://docs.openclaw.ai/plugins/building-plugins\#registering-cli-commands)  Registering CLI commands

Plugins can add root `openclaw` command groups with `api.registerCli`. Provide
`descriptors` for every top-level command root so OpenClaw can show and route
the command without eagerly loading every plugin runtime.

```
register(api) {
  api.registerCli(
    ({ program }) => {
      const demo = program
        .command("demo-plugin")
        .description("Run demo plugin commands");

      demo
        .command("ping")
        .description("Check that the plugin CLI is executable")
        .action(() => {
          console.log("demo-plugin:pong");
        });
    },
    {
      descriptors: [\
        {\
          name: "demo-plugin",\
          description: "Run demo plugin commands",\
          hasSubcommands: true,\
        },\
      ],
    },
  );
}
```

After install, verify the runtime registration and execute the command:

```
openclaw plugins inspect demo-plugin --runtime --json
openclaw demo-plugin ping
```

## [​](https://docs.openclaw.ai/plugins/building-plugins\#import-conventions)  Import conventions

Always import from focused `openclaw/plugin-sdk/<subpath>` paths:

```
import { definePluginEntry } from "openclaw/plugin-sdk/plugin-entry";
import { createPluginRuntimeStore } from "openclaw/plugin-sdk/runtime-store";

// Wrong: monolithic root (deprecated, will be removed)
import { ... } from "openclaw/plugin-sdk";
```

For the full subpath reference, see [SDK Overview](https://docs.openclaw.ai/plugins/sdk-overview).Within your plugin, use local barrel files (`api.ts`, `runtime-api.ts`) for
internal imports — never import your own plugin through its SDK path.For provider plugins, keep provider-specific helpers in those package-root
barrels unless the seam is truly generic. Current bundled examples:

- Anthropic: Claude stream wrappers and `service_tier` / beta helpers
- OpenAI: provider builders, default-model helpers, realtime providers
- OpenRouter: provider builder plus onboarding/config helpers

If a helper is only useful inside one bundled provider package, keep it on that
package-root seam instead of promoting it into `openclaw/plugin-sdk/*`.Some generated `openclaw/plugin-sdk/<bundled-id>` helper seams still exist for
bundled-plugin maintenance when they have tracked owner usage. Treat those as
reserved surfaces, not as the default pattern for new third-party plugins.

## [​](https://docs.openclaw.ai/plugins/building-plugins\#pre-submission-checklist)  Pre-submission checklist

**package.json** has correct `openclaw` metadata

**openclaw.plugin.json** manifest is present and valid

Entry point uses `defineChannelPluginEntry` or `definePluginEntry`

All imports use focused `plugin-sdk/<subpath>` paths

Internal imports use local modules, not SDK self-imports

Tests pass (`pnpm test -- <bundled-plugin-root>/my-plugin/`)

`pnpm check` passes (in-repo plugins)

## [​](https://docs.openclaw.ai/plugins/building-plugins\#beta-release-testing)  Beta release testing

1. Watch for GitHub release tags on [openclaw/openclaw](https://github.com/openclaw/openclaw/releases) and subscribe via `Watch` \> `Releases`. Beta tags look like `v2026.3.N-beta.1`. You can also turn on notifications for the official OpenClaw X account [@openclaw](https://x.com/openclaw) for release announcements.
2. Test your plugin against the beta tag as soon as it appears. The window before stable is typically only a few hours.
3. Post in your plugin’s thread in the `plugin-forum` Discord channel after testing with either `all good` or what broke. If you do not have a thread yet, create one.
4. If something breaks, open or update an issue titled `Beta blocker: <plugin-name> - <summary>` and apply the `beta-blocker` label. Put the issue link in your thread.
5. Open a PR to `main` titled `fix(<plugin-id>): beta blocker - <summary>` and link the issue in both the PR and your Discord thread. Contributors cannot label PRs, so the title is the PR-side signal for maintainers and automation. Blockers with a PR get merged; blockers without one might ship anyway. Maintainers watch these threads during beta testing.
6. Silence means green. If you miss the window, your fix likely lands in the next cycle.

## [​](https://docs.openclaw.ai/plugins/building-plugins\#next-steps)  Next steps

[**Channel Plugins** \\
\\
Build a messaging channel plugin](https://docs.openclaw.ai/plugins/sdk-channel-plugins)

[**Provider Plugins** \\
\\
Build a model provider plugin](https://docs.openclaw.ai/plugins/sdk-provider-plugins)

[**SDK Overview** \\
\\
Import map and registration API reference](https://docs.openclaw.ai/plugins/sdk-overview)

[**Runtime Helpers** \\
\\
TTS, search, subagent via api.runtime](https://docs.openclaw.ai/plugins/sdk-runtime)

[**Testing** \\
\\
Test utilities and patterns](https://docs.openclaw.ai/plugins/sdk-testing)

[**Plugin Manifest** \\
\\
Full manifest schema reference](https://docs.openclaw.ai/plugins/manifest)

## [​](https://docs.openclaw.ai/plugins/building-plugins\#related)  Related

- [Plugin Architecture](https://docs.openclaw.ai/plugins/architecture) — internal architecture deep dive
- [SDK Overview](https://docs.openclaw.ai/plugins/sdk-overview) — Plugin SDK reference
- [Manifest](https://docs.openclaw.ai/plugins/manifest) — plugin manifest format
- [Channel Plugins](https://docs.openclaw.ai/plugins/sdk-channel-plugins) — building channel plugins
- [Provider Plugins](https://docs.openclaw.ai/plugins/sdk-provider-plugins) — building provider plugins

[Zalo personal plugin](https://docs.openclaw.ai/plugins/zalouser) [Plugin hooks](https://docs.openclaw.ai/plugins/hooks)

Ctrl+I