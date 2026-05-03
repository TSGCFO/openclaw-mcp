---
source_url: https://docs.openclaw.ai/concepts/usage-tracking
title: "Usage tracking - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/usage-tracking#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Concept internals

Usage tracking

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What it is](https://docs.openclaw.ai/concepts/usage-tracking#what-it-is)
- [Where it shows up](https://docs.openclaw.ai/concepts/usage-tracking#where-it-shows-up)
- [Providers + credentials](https://docs.openclaw.ai/concepts/usage-tracking#providers-%2B-credentials)
- [Related](https://docs.openclaw.ai/concepts/usage-tracking#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

## [​](https://docs.openclaw.ai/concepts/usage-tracking\#what-it-is)  What it is

- Pulls provider usage/quota directly from their usage endpoints.
- No estimated costs; only the provider-reported windows.
- Human-readable status output is normalized to `X% left`, even when an
upstream API reports consumed quota, remaining quota, or only raw counts.
- Session-level `/status` and `session_status` can fall back to the latest
transcript usage entry when the live session snapshot is sparse. That
fallback fills missing token/cache counters, can recover the active runtime
model label, and prefers the larger prompt-oriented total when session
metadata is missing or smaller. Existing nonzero live values still win.

## [​](https://docs.openclaw.ai/concepts/usage-tracking\#where-it-shows-up)  Where it shows up

- `/status` in chats: emoji‑rich status card with session tokens + estimated cost (API key only). Provider usage shows for the **current model provider** when available as a normalized `X% left` window.
- `/usage off|tokens|full` in chats: per-response usage footer (OAuth shows tokens only).
- `/usage cost` in chats: local cost summary aggregated from OpenClaw session logs.
- CLI: `openclaw status --usage` prints a full per-provider breakdown.
- CLI: `openclaw channels list` prints the same usage snapshot alongside provider config (use `--no-usage` to skip).
- macOS menu bar: “Usage” section under Context (only if available).

## [​](https://docs.openclaw.ai/concepts/usage-tracking\#providers-+-credentials)  Providers + credentials

- **Anthropic (Claude)**: OAuth tokens in auth profiles.
- **GitHub Copilot**: OAuth tokens in auth profiles.
- **Gemini CLI**: OAuth tokens in auth profiles.

  - JSON usage falls back to `stats`; `stats.cached` is normalized into
    `cacheRead`.
- **OpenAI Codex**: OAuth tokens in auth profiles (accountId used when present).
- **MiniMax**: API key or MiniMax OAuth auth profile. OpenClaw treats
`minimax`, `minimax-cn`, and `minimax-portal` as the same MiniMax quota
surface, prefers stored MiniMax OAuth when present, and otherwise falls back
to `MINIMAX_CODE_PLAN_KEY`, `MINIMAX_CODING_API_KEY`, or `MINIMAX_API_KEY`.
Usage polling derives the Coding Plan host from `models.providers.minimax-portal.baseUrl`
or `models.providers.minimax.baseUrl` when configured, and otherwise uses the
MiniMax CN host.
MiniMax’s raw `usage_percent` / `usagePercent` fields mean **remaining**
quota, so OpenClaw inverts them before display; count-based fields win when
present.

  - Coding-plan window labels come from provider hours/minutes fields when
    present, then fall back to the `start_time` / `end_time` span.
  - If the coding-plan endpoint returns `model_remains`, OpenClaw prefers the
    chat-model entry, derives the window label from timestamps when explicit
    `window_hours` / `window_minutes` fields are absent, and includes the model
    name in the plan label.
- **Xiaomi MiMo**: API key via env/config/auth store (`XIAOMI_API_KEY`).
- **z.ai**: API key via env/config/auth store.

Usage is hidden when no usable provider usage auth can be resolved. Providers
can supply plugin-specific usage auth logic; otherwise OpenClaw falls back to
matching OAuth/API-key credentials from auth profiles, environment variables,
or config.

## [​](https://docs.openclaw.ai/concepts/usage-tracking\#related)  Related

- [Token use and costs](https://docs.openclaw.ai/reference/token-use)
- [API usage and costs](https://docs.openclaw.ai/reference/api-usage-costs)
- [Prompt caching](https://docs.openclaw.ai/reference/prompt-caching)

[Typing indicators](https://docs.openclaw.ai/concepts/typing-indicators) [Timezones](https://docs.openclaw.ai/concepts/timezone)

Ctrl+I