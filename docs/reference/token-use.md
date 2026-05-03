---
source_url: https://docs.openclaw.ai/reference/token-use
title: "Token use and costs - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/reference/token-use#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Technical reference

Token use and costs

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Token use & costs](https://docs.openclaw.ai/reference/token-use#token-use-%26-costs)
- [How the system prompt is built](https://docs.openclaw.ai/reference/token-use#how-the-system-prompt-is-built)
- [What counts in the context window](https://docs.openclaw.ai/reference/token-use#what-counts-in-the-context-window)
- [How to see current token usage](https://docs.openclaw.ai/reference/token-use#how-to-see-current-token-usage)
- [Cost estimation (when shown)](https://docs.openclaw.ai/reference/token-use#cost-estimation-when-shown)
- [Cache TTL and pruning impact](https://docs.openclaw.ai/reference/token-use#cache-ttl-and-pruning-impact)
- [Example: keep 1h cache warm with heartbeat](https://docs.openclaw.ai/reference/token-use#example-keep-1h-cache-warm-with-heartbeat)
- [Example: mixed traffic with per-agent cache strategy](https://docs.openclaw.ai/reference/token-use#example-mixed-traffic-with-per-agent-cache-strategy)
- [Example: enable Anthropic 1M context beta header](https://docs.openclaw.ai/reference/token-use#example-enable-anthropic-1m-context-beta-header)
- [Tips for reducing token pressure](https://docs.openclaw.ai/reference/token-use#tips-for-reducing-token-pressure)
- [Related](https://docs.openclaw.ai/reference/token-use#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/reference/token-use\#token-use-&-costs)  Token use & costs

OpenClaw tracks **tokens**, not characters. Tokens are model-specific, but most
OpenAI-style models average ~4 characters per token for English text.

## [​](https://docs.openclaw.ai/reference/token-use\#how-the-system-prompt-is-built)  How the system prompt is built

OpenClaw assembles its own system prompt on every run. It includes:

- Tool list + short descriptions
- Skills list (only metadata; instructions are loaded on demand with `read`).
The compact skills block is bounded by `skills.limits.maxSkillsPromptChars`,
with optional per-agent override at
`agents.list[].skillsLimits.maxSkillsPromptChars`.
- Self-update instructions
- Workspace + bootstrap files (`AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`, `BOOTSTRAP.md` when new, plus `MEMORY.md` when present). Lowercase root `memory.md` is not injected; it is legacy repair input for `openclaw doctor --fix` when paired with `MEMORY.md`. Large files are truncated by `agents.defaults.bootstrapMaxChars` (default: 12000), and total bootstrap injection is capped by `agents.defaults.bootstrapTotalMaxChars` (default: 60000). `memory/*.md` daily files are not part of the normal bootstrap prompt; they remain on-demand via memory tools on ordinary turns, but reset/startup model runs can prepend a one-shot startup-context block with recent daily memory for that first turn. Bare chat `/new` and `/reset` commands are acknowledged without invoking the model. The startup prelude is controlled by `agents.defaults.startupContext`.
- Time (UTC + user timezone)
- Reply tags + heartbeat behavior
- Runtime metadata (host/OS/model/thinking)

See the full breakdown in [System Prompt](https://docs.openclaw.ai/concepts/system-prompt).

## [​](https://docs.openclaw.ai/reference/token-use\#what-counts-in-the-context-window)  What counts in the context window

Everything the model receives counts toward the context limit:

- System prompt (all sections listed above)
- Conversation history (user + assistant messages)
- Tool calls and tool results
- Attachments/transcripts (images, audio, files)
- Compaction summaries and pruning artifacts
- Provider wrappers or safety headers (not visible, but still counted)

Some runtime-heavy surfaces have their own explicit caps:

- `agents.defaults.contextLimits.memoryGetMaxChars`
- `agents.defaults.contextLimits.memoryGetDefaultLines`
- `agents.defaults.contextLimits.toolResultMaxChars`
- `agents.defaults.contextLimits.postCompactionMaxChars`

Per-agent overrides live under `agents.list[].contextLimits`. These knobs are
for bounded runtime excerpts and injected runtime-owned blocks. They are
separate from bootstrap limits, startup-context limits, and skills prompt
limits.For images, OpenClaw downscales transcript/tool image payloads before provider calls.
Use `agents.defaults.imageMaxDimensionPx` (default: `1200`) to tune this:

- Lower values usually reduce vision-token usage and payload size.
- Higher values preserve more visual detail for OCR/UI-heavy screenshots.

For a practical breakdown (per injected file, tools, skills, and system prompt size), use `/context list` or `/context detail`. See [Context](https://docs.openclaw.ai/concepts/context).

## [​](https://docs.openclaw.ai/reference/token-use\#how-to-see-current-token-usage)  How to see current token usage

Use these in chat:

- `/status` → **emoji‑rich status card** with the session model, context usage,
last response input/output tokens, and **estimated cost** (API key only).
- `/usage off|tokens|full` → appends a **per-response usage footer** to every reply.

  - Persists per session (stored as `responseUsage`).
  - OAuth auth **hides cost** (tokens only).
- `/usage cost` → shows a local cost summary from OpenClaw session logs.

Other surfaces:

- **TUI/Web TUI:**`/status` \+ `/usage` are supported.
- **CLI:**`openclaw status --usage` and `openclaw channels list` show
normalized provider quota windows (`X% left`, not per-response costs).
Current usage-window providers: Anthropic, GitHub Copilot, Gemini CLI,
OpenAI Codex, MiniMax, Xiaomi, and z.ai.

Usage surfaces normalize common provider-native field aliases before display.
For OpenAI-family Responses traffic, that includes both `input_tokens` /
`output_tokens` and `prompt_tokens` / `completion_tokens`, so transport-specific
field names do not change `/status`, `/usage`, or session summaries.
Gemini CLI JSON usage is normalized too: reply text comes from `response`, and
`stats.cached` maps to `cacheRead` with `stats.input_tokens - stats.cached`
used when the CLI omits an explicit `stats.input` field.
For native OpenAI-family Responses traffic, WebSocket/SSE usage aliases are
normalized the same way, and totals fall back to normalized input + output when
`total_tokens` is missing or `0`.
When the current session snapshot is sparse, `/status` and `session_status` can
also recover token/cache counters and the active runtime model label from the
most recent transcript usage log. Existing nonzero live values still take
precedence over transcript fallback values, and larger prompt-oriented
transcript totals can win when stored totals are missing or smaller.
Usage auth for provider quota windows comes from provider-specific hooks when
available; otherwise OpenClaw falls back to matching OAuth/API-key credentials
from auth profiles, env, or config.
Assistant transcript entries persist the same normalized usage shape, including
`usage.cost` when the active model has pricing configured and the provider
returns usage metadata. This gives `/usage cost` and transcript-backed session
status a stable source even after the live runtime state is gone.OpenClaw keeps provider usage accounting separate from the current context
snapshot. Provider `usage.total` can include cached input, output, and multiple
tool-loop model calls, so it is useful for cost and telemetry but can overstate
the live context window. Context displays and diagnostics use the latest prompt
snapshot (`promptTokens`, or the last model call when no prompt snapshot is
available) for `context.used`.

## [​](https://docs.openclaw.ai/reference/token-use\#cost-estimation-when-shown)  Cost estimation (when shown)

Costs are estimated from your model pricing config:

```
models.providers.<provider>.models[].cost
```

These are **USD per 1M tokens** for `input`, `output`, `cacheRead`, and
`cacheWrite`. If pricing is missing, OpenClaw shows tokens only. OAuth tokens
never show dollar cost.After sidecars and channels reach the Gateway ready path, OpenClaw starts an
optional background pricing bootstrap for configured model refs that do not
already have local pricing. That bootstrap fetches remote OpenRouter and LiteLLM
pricing catalogs. Set `models.pricing.enabled: false` to skip those catalog
fetches on offline or restricted networks; explicit
`models.providers.*.models[].cost` entries continue to drive local cost
estimates.

## [​](https://docs.openclaw.ai/reference/token-use\#cache-ttl-and-pruning-impact)  Cache TTL and pruning impact

Provider prompt caching only applies within the cache TTL window. OpenClaw can
optionally run **cache-ttl pruning**: it prunes the session once the cache TTL
has expired, then resets the cache window so subsequent requests can re-use the
freshly cached context instead of re-caching the full history. This keeps cache
write costs lower when a session goes idle past the TTL.Configure it in [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) and see the
behavior details in [Session pruning](https://docs.openclaw.ai/concepts/session-pruning).Heartbeat can keep the cache **warm** across idle gaps. If your model cache TTL
is `1h`, setting the heartbeat interval just under that (e.g., `55m`) can avoid
re-caching the full prompt, reducing cache write costs.In multi-agent setups, you can keep one shared model config and tune cache behavior
per agent with `agents.list[].params.cacheRetention`.For a full knob-by-knob guide, see [Prompt Caching](https://docs.openclaw.ai/reference/prompt-caching).For Anthropic API pricing, cache reads are significantly cheaper than input
tokens, while cache writes are billed at a higher multiplier. See Anthropic’s
prompt caching pricing for the latest rates and TTL multipliers:
[https://docs.anthropic.com/docs/build-with-claude/prompt-caching](https://docs.anthropic.com/docs/build-with-claude/prompt-caching)

### [​](https://docs.openclaw.ai/reference/token-use\#example-keep-1h-cache-warm-with-heartbeat)  Example: keep 1h cache warm with heartbeat

```
agents:
  defaults:
    model:
      primary: "anthropic/claude-opus-4-6"
    models:
      "anthropic/claude-opus-4-6":
        params:
          cacheRetention: "long"
    heartbeat:
      every: "55m"
```

### [​](https://docs.openclaw.ai/reference/token-use\#example-mixed-traffic-with-per-agent-cache-strategy)  Example: mixed traffic with per-agent cache strategy

```
agents:
  defaults:
    model:
      primary: "anthropic/claude-opus-4-6"
    models:
      "anthropic/claude-opus-4-6":
        params:
          cacheRetention: "long" # default baseline for most agents
  list:
    - id: "research"
      default: true
      heartbeat:
        every: "55m" # keep long cache warm for deep sessions
    - id: "alerts"
      params:
        cacheRetention: "none" # avoid cache writes for bursty notifications
```

`agents.list[].params` merges on top of the selected model’s `params`, so you can
override only `cacheRetention` and inherit other model defaults unchanged.

### [​](https://docs.openclaw.ai/reference/token-use\#example-enable-anthropic-1m-context-beta-header)  Example: enable Anthropic 1M context beta header

Anthropic’s 1M context window is currently beta-gated. OpenClaw can inject the
required `anthropic-beta` value when you enable `context1m` on supported Opus
or Sonnet models.

```
agents:
  defaults:
    models:
      "anthropic/claude-opus-4-6":
        params:
          context1m: true
```

This maps to Anthropic’s `context-1m-2025-08-07` beta header.This only applies when `context1m: true` is set on that model entry.Requirement: the credential must be eligible for long-context usage. If not,
Anthropic responds with a provider-side rate limit error for that request.If you authenticate Anthropic with OAuth/subscription tokens (`sk-ant-oat-*`),
OpenClaw skips the `context-1m-*` beta header because Anthropic currently
rejects that combination with HTTP 401.

## [​](https://docs.openclaw.ai/reference/token-use\#tips-for-reducing-token-pressure)  Tips for reducing token pressure

- Use `/compact` to summarize long sessions.
- Trim large tool outputs in your workflows.
- Lower `agents.defaults.imageMaxDimensionPx` for screenshot-heavy sessions.
- Keep skill descriptions short (skill list is injected into the prompt).
- Prefer smaller models for verbose, exploratory work.

See [Skills](https://docs.openclaw.ai/tools/skills) for the exact skill list overhead formula.

## [​](https://docs.openclaw.ai/reference/token-use\#related)  Related

- [API usage and costs](https://docs.openclaw.ai/reference/api-usage-costs)
- [Prompt caching](https://docs.openclaw.ai/reference/prompt-caching)
- [Usage tracking](https://docs.openclaw.ai/concepts/usage-tracking)

[Onboarding Reference](https://docs.openclaw.ai/reference/wizard) [SecretRef credential surface](https://docs.openclaw.ai/reference/secretref-credential-surface)

Ctrl+I