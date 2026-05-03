---
source_url: https://docs.openclaw.ai/tools/index#tools-and-plugins
title: "Tools and plugins - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/index#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Overview

Tools and plugins

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Tools, skills, and plugins](https://docs.openclaw.ai/tools/index#tools-skills-and-plugins)
- [Built-in tools](https://docs.openclaw.ai/tools/index#built-in-tools)
- [Plugin-provided tools](https://docs.openclaw.ai/tools/index#plugin-provided-tools)
- [Tool configuration](https://docs.openclaw.ai/tools/index#tool-configuration)
- [Allow and deny lists](https://docs.openclaw.ai/tools/index#allow-and-deny-lists)
- [Tool profiles](https://docs.openclaw.ai/tools/index#tool-profiles)
- [Tool groups](https://docs.openclaw.ai/tools/index#tool-groups)
- [Provider-specific restrictions](https://docs.openclaw.ai/tools/index#provider-specific-restrictions)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Everything the agent does beyond generating text happens through **tools**.
Tools are how the agent reads files, runs commands, browses the web, sends
messages, and interacts with devices.

## [​](https://docs.openclaw.ai/tools/index\#tools-skills-and-plugins)  Tools, skills, and plugins

OpenClaw has three layers that work together:

1

[Navigate to header](https://docs.openclaw.ai/tools/index#)

Tools are what the agent calls

A tool is a typed function the agent can invoke (e.g. `exec`, `browser`,
`web_search`, `message`). OpenClaw ships a set of **built-in tools** and
plugins can register additional ones.The agent sees tools as structured function definitions sent to the model API.

2

[Navigate to header](https://docs.openclaw.ai/tools/index#)

Skills teach the agent when and how

A skill is a markdown file (`SKILL.md`) injected into the system prompt.
Skills give the agent context, constraints, and step-by-step guidance for
using tools effectively. Skills live in your workspace, in shared folders,
or ship inside plugins.[Skills reference](https://docs.openclaw.ai/tools/skills) \| [Creating skills](https://docs.openclaw.ai/tools/creating-skills)

3

[Navigate to header](https://docs.openclaw.ai/tools/index#)

Plugins package everything together

A plugin is a package that can register any combination of capabilities:
channels, model providers, tools, skills, speech, realtime transcription,
realtime voice, media understanding, image generation, video generation,
web fetch, web search, and more. Some plugins are **core** (shipped with
OpenClaw), others are **external** (published on npm by the community).[Install and configure plugins](https://docs.openclaw.ai/tools/plugin) \| [Build your own](https://docs.openclaw.ai/plugins/building-plugins)

## [​](https://docs.openclaw.ai/tools/index\#built-in-tools)  Built-in tools

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

For image work, use `image` for analysis and `image_generate` for generation or editing. If you target `openai/*`, `google/*`, `fal/*`, or another non-default image provider, configure that provider’s auth/API key first.For music work, use `music_generate`. If you target `google/*`, `minimax/*`, or another non-default music provider, configure that provider’s auth/API key first.For video work, use `video_generate`. If you target `qwen/*` or another non-default video provider, configure that provider’s auth/API key first.For workflow-driven audio generation, use `music_generate` when a plugin such as
ComfyUI registers it. This is separate from `tts`, which is text-to-speech.`session_status` is the lightweight status/readback tool in the sessions group.
It answers `/status`-style questions about the current session and can
optionally set a per-session model override; `model=default` clears that
override. Like `/status`, it can backfill sparse token/cache counters and the
active runtime model label from the latest transcript usage entry.`gateway` is the owner-only runtime tool for gateway operations:

- `config.schema.lookup` for one path-scoped config subtree before edits
- `config.get` for the current config snapshot + hash
- `config.patch` for partial config updates with restart
- `config.apply` only for full-config replacement
- `update.run` for explicit self-update + restart

For partial changes, prefer `config.schema.lookup` then `config.patch`. Use
`config.apply` only when you intentionally replace the entire config.
For broader config docs, read [Configuration](https://docs.openclaw.ai/gateway/configuration) and
[Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference).
The tool also refuses to change `tools.exec.ask` or `tools.exec.security`;
legacy `tools.bash.*` aliases normalize to the same protected exec paths.

### [​](https://docs.openclaw.ai/tools/index\#plugin-provided-tools)  Plugin-provided tools

Plugins can register additional tools. Some examples:

- [Diffs](https://docs.openclaw.ai/tools/diffs) — diff viewer and renderer
- [LLM Task](https://docs.openclaw.ai/tools/llm-task) — JSON-only LLM step for structured output
- [Lobster](https://docs.openclaw.ai/tools/lobster) — typed workflow runtime with resumable approvals
- [Music Generation](https://docs.openclaw.ai/tools/music-generation) — shared `music_generate` tool with workflow-backed providers
- [OpenProse](https://docs.openclaw.ai/prose) — markdown-first workflow orchestration
- [Tokenjuice](https://docs.openclaw.ai/tools/tokenjuice) — compact noisy `exec` and `bash` tool results

Plugin tools are still authored with `api.registerTool(...)` and declared in
the plugin manifest’s `contracts.tools` list. OpenClaw captures the validated
tool descriptor during discovery and caches it by plugin source and contract, so
later tool planning can skip plugin runtime loading. Tool execution still loads
the owning plugin and calls the live registered implementation.

## [​](https://docs.openclaw.ai/tools/index\#tool-configuration)  Tool configuration

### [​](https://docs.openclaw.ai/tools/index\#allow-and-deny-lists)  Allow and deny lists

Control which tools the agent can call via `tools.allow` / `tools.deny` in
config. Deny always wins over allow.

```
{
  tools: {
    allow: ["group:fs", "browser", "web_search"],
    deny: ["exec"],
  },
}
```

OpenClaw fails closed when an explicit allowlist resolves to no callable tools.
For example, `tools.allow: ["query_db"]` only works if a loaded plugin actually
registers `query_db`. If no built-in, plugin, or bundled MCP tool matches the
allowlist, the run stops before the model call instead of continuing as a
text-only run that could hallucinate tool results.

### [​](https://docs.openclaw.ai/tools/index\#tool-profiles)  Tool profiles

`tools.profile` sets a base allowlist before `allow`/`deny` is applied.
Per-agent override: `agents.list[].tools.profile`.

| Profile | What it includes |
| --- | --- |
| `full` | Unrestricted baseline for broader command/control access; same as leaving `tools.profile` unset |
| `coding` | `group:fs`, `group:runtime`, `group:web`, `group:sessions`, `group:memory`, `cron`, `image`, `image_generate`, `music_generate`, `video_generate` |
| `messaging` | `group:messaging`, `sessions_list`, `sessions_history`, `sessions_send`, `session_status` |
| `minimal` | `session_status` only |

`tools.profile: "messaging"` is intentionally narrow for channel-focused
agents. It leaves out broader command/control tools such as filesystem, runtime,
browser, canvas, nodes, cron, and gateway control. Use `tools.profile: "full"`
as the unrestricted baseline for broader command/control access, then trim
access with `tools.allow` / `tools.deny` when needed.

`coding` includes lightweight web tools (`web_search`, `web_fetch`, `x_search`)
but not the full browser-control tool. Browser automation can drive real
sessions and logged-in profiles, so add it explicitly with
`tools.alsoAllow: ["browser"]` or a per-agent
`agents.list[].tools.alsoAllow: ["browser"]`.

Configuring `tools.exec` or `tools.fs` under a restrictive profile (`messaging`, `minimal`) does not implicitly widen the profile’s allowlist. Add explicit `tools.alsoAllow` entries (for example `["exec", "process"]` for exec, or `["read", "write", "edit"]` for fs) when you want a restrictive profile to use those configured sections. OpenClaw logs a startup warning when a config section is present without a matching `alsoAllow` grant.

The `coding` and `messaging` profiles also allow configured bundle MCP tools
under the plugin key `bundle-mcp`. Add `tools.deny: ["bundle-mcp"]` when you
want a profile to keep its normal built-ins but hide all configured MCP tools.
The `minimal` profile does not include bundle MCP tools.Example (broadest tool surface by default):

```
{
  tools: {
    profile: "full",
  },
}
```

### [​](https://docs.openclaw.ai/tools/index\#tool-groups)  Tool groups

Use `group:*` shorthands in allow/deny lists:

| Group | Tools |
| --- | --- |
| `group:runtime` | exec, process, code\_execution (`bash` is accepted as an alias for `exec`) |
| `group:fs` | read, write, edit, apply\_patch |
| `group:sessions` | sessions\_list, sessions\_history, sessions\_send, sessions\_spawn, sessions\_yield, subagents, session\_status |
| `group:memory` | memory\_search, memory\_get |
| `group:web` | web\_search, x\_search, web\_fetch |
| `group:ui` | browser, canvas |
| `group:automation` | cron, gateway |
| `group:messaging` | message |
| `group:nodes` | nodes |
| `group:agents` | agents\_list |
| `group:media` | image, image\_generate, music\_generate, video\_generate, tts |
| `group:openclaw` | All built-in OpenClaw tools (excludes plugin tools) |

`sessions_history` returns a bounded, safety-filtered recall view. It strips
thinking tags, `<relevant-memories>` scaffolding, plain-text tool-call XML
payloads (including `<tool_call>...</tool_call>`,
`<function_call>...</function_call>`, `<tool_calls>...</tool_calls>`,
`<function_calls>...</function_calls>`, and truncated tool-call blocks),
downgraded tool-call scaffolding, leaked ASCII/full-width model control
tokens, and malformed MiniMax tool-call XML from assistant text, then applies
redaction/truncation and possible oversized-row placeholders instead of acting
as a raw transcript dump.

### [​](https://docs.openclaw.ai/tools/index\#provider-specific-restrictions)  Provider-specific restrictions

Use `tools.byProvider` to restrict tools for specific providers without
changing global defaults:

```
{
  tools: {
    profile: "coding",
    byProvider: {
      "google-antigravity": { profile: "minimal" },
    },
  },
}
```

[Install and Configure](https://docs.openclaw.ai/tools/plugin)

Ctrl+I