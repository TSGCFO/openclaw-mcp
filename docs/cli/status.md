---
source_url: https://docs.openclaw.ai/cli/status
title: "Status - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/status#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Gateway and service

Status

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw status](https://docs.openclaw.ai/cli/status#openclaw-status)
- [Related](https://docs.openclaw.ai/cli/status#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/status\#openclaw-status)  `openclaw status`

Diagnostics for channels + sessions.

```
openclaw status
openclaw status --all
openclaw status --deep
openclaw status --usage
```

Notes:

- `--deep` runs live probes (WhatsApp Web + Telegram + Discord + Slack + Signal).
- Plain `openclaw status` stays on the fast read-only path and marks memory as `not checked` instead of unavailable when it skips memory inspection. Heavy security audit, plugin compatibility, and memory-vector probes are left to `openclaw status --all`, `openclaw status --deep`, `openclaw security audit`, and `openclaw memory status --deep`.
- `status --json --all` reports memory details from the active memory plugin runtime selected by `plugins.slots.memory`. Custom memory plugins can leave built-in `agents.defaults.memorySearch.enabled` disabled and still report their own files, chunks, vector, and FTS state.
- `--usage` prints normalized provider usage windows as `X% left`.
- Session status output separates `Execution:` from `Runtime:`. `Execution` is the sandbox path (`direct`, `docker/*`), while `Runtime` tells you whether the session is using `OpenClaw Pi Default`, `OpenAI Codex`, a CLI backend, or an ACP backend such as `codex (acp/acpx)`. See [Agent runtimes](https://docs.openclaw.ai/concepts/agent-runtimes) for the provider/model/runtime distinction.
- MiniMax’s raw `usage_percent` / `usagePercent` fields are remaining quota, so OpenClaw inverts them before display; count-based fields win when present. `model_remains` responses prefer the chat-model entry, derive the window label from timestamps when needed, and include the model name in the plan label.
- When the current session snapshot is sparse, `/status` can backfill token and cache counters from the most recent transcript usage log. Existing nonzero live values still win over transcript fallback values.
- Transcript fallback can also recover the active runtime model label when the live session entry is missing it. If that transcript model differs from the selected model, status resolves the context window against the recovered runtime model instead of the selected one.
- For prompt-size accounting, transcript fallback prefers the larger prompt-oriented total when session metadata is missing or smaller, so custom-provider sessions do not collapse to `0` token displays.
- Output includes per-agent session stores when multiple agents are configured.
- Overview includes Gateway + node host service install/runtime status when available.
- Overview includes update channel + git SHA (for source checkouts).
- Update info surfaces in the Overview; if an update is available, status prints a hint to run `openclaw update` (see [Updating](https://docs.openclaw.ai/install/updating)).
- Read-only status surfaces (`status`, `status --json`, `status --all`) resolve supported SecretRefs for their targeted config paths when possible.
- If a supported channel SecretRef is configured but unavailable in the current command path, status stays read-only and reports degraded output instead of crashing. Human output shows warnings such as “configured token unavailable in this command path”, and JSON output includes `secretDiagnostics`.
- When command-local SecretRef resolution succeeds, status prefers the resolved snapshot and clears transient “secret unavailable” channel markers from the final output.
- `status --all` includes a Secrets overview row and a diagnosis section that summarizes secret diagnostics (truncated for readability) without stopping report generation.

## [​](https://docs.openclaw.ai/cli/status\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Doctor](https://docs.openclaw.ai/gateway/doctor)

[Setup](https://docs.openclaw.ai/cli/setup) [Uninstall](https://docs.openclaw.ai/cli/uninstall)

Ctrl+I