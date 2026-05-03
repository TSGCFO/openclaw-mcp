---
source_url: https://docs.openclaw.ai/concepts/model-failover
title: "Model failover - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/model-failover#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Concepts and configuration

Model failover

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Runtime flow](https://docs.openclaw.ai/concepts/model-failover#runtime-flow)
- [Selection source policy](https://docs.openclaw.ai/concepts/model-failover#selection-source-policy)
- [Auth storage (keys + OAuth)](https://docs.openclaw.ai/concepts/model-failover#auth-storage-keys-%2B-oauth)
- [Profile IDs](https://docs.openclaw.ai/concepts/model-failover#profile-ids)
- [Rotation order](https://docs.openclaw.ai/concepts/model-failover#rotation-order)
- [Session stickiness (cache-friendly)](https://docs.openclaw.ai/concepts/model-failover#session-stickiness-cache-friendly)
- [Why OAuth can “look lost”](https://docs.openclaw.ai/concepts/model-failover#why-oauth-can-%E2%80%9Clook-lost%E2%80%9D)
- [Cooldowns](https://docs.openclaw.ai/concepts/model-failover#cooldowns)
- [Billing disables](https://docs.openclaw.ai/concepts/model-failover#billing-disables)
- [Model fallback](https://docs.openclaw.ai/concepts/model-failover#model-fallback)
- [Candidate chain rules](https://docs.openclaw.ai/concepts/model-failover#candidate-chain-rules)
- [Which errors advance fallback](https://docs.openclaw.ai/concepts/model-failover#which-errors-advance-fallback)
- [Cooldown skip vs probe behavior](https://docs.openclaw.ai/concepts/model-failover#cooldown-skip-vs-probe-behavior)
- [Session overrides and live model switching](https://docs.openclaw.ai/concepts/model-failover#session-overrides-and-live-model-switching)
- [Observability and failure summaries](https://docs.openclaw.ai/concepts/model-failover#observability-and-failure-summaries)
- [Related config](https://docs.openclaw.ai/concepts/model-failover#related-config)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw handles failures in two stages:

1. **Auth profile rotation** within the current provider.
2. **Model fallback** to the next model in `agents.defaults.model.fallbacks`.

This doc explains the runtime rules and the data that backs them.

## [​](https://docs.openclaw.ai/concepts/model-failover\#runtime-flow)  Runtime flow

For a normal text run, OpenClaw evaluates candidates in this order:

1

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Resolve session state

Resolve the active session model and auth-profile preference.

2

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Build candidate chain

Build the model candidate chain from the current model selection and the fallback policy for that selection source. Configured defaults, cron job primaries, and auto-selected fallback models can use configured fallbacks; explicit user session selections are strict.

3

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Try the current provider

Try the current provider with auth-profile rotation/cooldown rules.

4

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Advance on failover-worthy errors

If that provider is exhausted with a failover-worthy error, move to the next model candidate.

5

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Persist fallback override

Persist the selected fallback override before the retry starts so other session readers see the same provider/model the runner is about to use. The persisted model override is marked `modelOverrideSource: "auto"`.

6

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Roll back narrowly on failure

If the fallback candidate fails, roll back only the fallback-owned session override fields when they still match that failed candidate.

7

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Throw FallbackSummaryError if exhausted

If every candidate fails, throw a `FallbackSummaryError` with per-attempt detail and the soonest cooldown expiry when one is known.

This is intentionally narrower than “save and restore the whole session”. The reply runner only persists the model-selection fields it owns for fallback:

- `providerOverride`
- `modelOverride`
- `modelOverrideSource`
- `authProfileOverride`
- `authProfileOverrideSource`
- `authProfileOverrideCompactionCount`

That prevents a failed fallback retry from overwriting newer unrelated session mutations such as manual `/model` changes or session rotation updates that happened while the attempt was running.

## [​](https://docs.openclaw.ai/concepts/model-failover\#selection-source-policy)  Selection source policy

OpenClaw separates the selected provider/model from why it was selected. That source controls whether the fallback chain is allowed:

- **Configured default**: `agents.defaults.model.primary` uses `agents.defaults.model.fallbacks`.
- **Agent primary**: `agents.list[].model` is strict unless that agent model object includes its own `fallbacks`. Use `fallbacks: []` to make the strict behavior explicit, or provide a non-empty list to opt that agent into model fallback.
- **Auto fallback override**: a runtime fallback writes `providerOverride`, `modelOverride`, and `modelOverrideSource: "auto"` before retrying. That auto override can keep walking the configured fallback chain and is cleared by `/new`, `/reset`, and `sessions.reset`.
- **User session override**: `/model`, the model picker, `session_status(model=...)`, and `sessions.patch` write `modelOverrideSource: "user"`. That is an exact session selection. If the selected provider/model fails before producing a reply, OpenClaw reports the failure instead of answering from an unrelated configured fallback.
- **Legacy session override**: older session entries may have `modelOverride` without `modelOverrideSource`. OpenClaw treats those as user overrides so an explicit old selection is not silently converted into fallback behavior.
- **Cron payload model**: a cron job `payload.model` / `--model` is a job primary, not a user session override. It uses configured fallbacks unless the job provides `payload.fallbacks`; `payload.fallbacks: []` makes the cron run strict.

## [​](https://docs.openclaw.ai/concepts/model-failover\#auth-storage-keys-+-oauth)  Auth storage (keys + OAuth)

OpenClaw uses **auth profiles** for both API keys and OAuth tokens.

- Secrets live in `~/.openclaw/agents/<agentId>/agent/auth-profiles.json` (legacy: `~/.openclaw/agent/auth-profiles.json`).
- Runtime auth-routing state lives in `~/.openclaw/agents/<agentId>/agent/auth-state.json`.
- Config `auth.profiles` / `auth.order` are **metadata + routing only** (no secrets).
- Legacy import-only OAuth file: `~/.openclaw/credentials/oauth.json` (imported into `auth-profiles.json` on first use).

More detail: [OAuth](https://docs.openclaw.ai/concepts/oauth)Credential types:

- `type: "api_key"` → `{ provider, key }`
- `type: "oauth"` → `{ provider, access, refresh, expires, email? }` (\+ `projectId`/`enterpriseUrl` for some providers)

## [​](https://docs.openclaw.ai/concepts/model-failover\#profile-ids)  Profile IDs

OAuth logins create distinct profiles so multiple accounts can coexist.

- Default: `provider:default` when no email is available.
- OAuth with email: `provider:<email>` (for example `google-antigravity:user@gmail.com`).

Profiles live in `~/.openclaw/agents/<agentId>/agent/auth-profiles.json` under `profiles`.

## [​](https://docs.openclaw.ai/concepts/model-failover\#rotation-order)  Rotation order

When a provider has multiple profiles, OpenClaw chooses an order like this:

1

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Explicit config

`auth.order[provider]` (if set).

2

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Configured profiles

`auth.profiles` filtered by provider.

3

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Stored profiles

Entries in `auth-profiles.json` for the provider.

If no explicit order is configured, OpenClaw uses a round‑robin order:

- **Primary key:** profile type ( **OAuth before API keys**).
- **Secondary key:**`usageStats.lastUsed` (oldest first, within each type).
- **Cooldown/disabled profiles** are moved to the end, ordered by soonest expiry.

### [​](https://docs.openclaw.ai/concepts/model-failover\#session-stickiness-cache-friendly)  Session stickiness (cache-friendly)

OpenClaw **pins the chosen auth profile per session** to keep provider caches warm. It does **not** rotate on every request. The pinned profile is reused until:

- the session is reset (`/new` / `/reset`)
- a compaction completes (compaction count increments)
- the profile is in cooldown/disabled

Manual selection via `/model …@<profileId>` sets a **user override** for that session and is not auto-rotated until a new session starts.

Auto-pinned profiles (selected by the session router) are treated as a **preference**: they are tried first, but OpenClaw may rotate to another profile on rate limits/timeouts. User-pinned profiles stay locked to that profile; if it fails and model fallbacks are configured, OpenClaw moves to the next model instead of switching profiles.

### [​](https://docs.openclaw.ai/concepts/model-failover\#why-oauth-can-%E2%80%9Clook-lost%E2%80%9D)  Why OAuth can “look lost”

If you have both an OAuth profile and an API key profile for the same provider, round‑robin can switch between them across messages unless pinned. To force a single profile:

- Pin with `auth.order[provider] = ["provider:profileId"]`, or
- Use a per-session override via `/model …` with a profile override (when supported by your UI/chat surface).

## [​](https://docs.openclaw.ai/concepts/model-failover\#cooldowns)  Cooldowns

When a profile fails due to auth/rate-limit errors (or a timeout that looks like rate limiting), OpenClaw marks it in cooldown and moves to the next profile.

What lands in the rate-limit / timeout bucket

That rate-limit bucket is broader than plain `429`: it also includes provider messages such as `Too many concurrent requests`, `ThrottlingException`, `concurrency limit reached`, `workers_ai ... quota limit exceeded`, `throttled`, `resource exhausted`, and periodic usage-window limits such as `weekly/monthly limit reached`.Format/invalid-request errors (for example Cloud Code Assist tool call ID validation failures) are treated as failover-worthy and use the same cooldowns. OpenAI-compatible stop-reason errors such as `Unhandled stop reason: error`, `stop reason: error`, and `reason: error` are classified as timeout/failover signals.Generic server text can also land in that timeout bucket when the source matches a known transient pattern. For example, the bare pi-ai stream-wrapper message `An unknown error occurred` is treated as failover-worthy for every provider because pi-ai emits it when provider streams end with `stopReason: "aborted"` or `stopReason: "error"` without specific details. JSON `api_error` payloads with transient server text such as `internal server error`, `unknown error, 520`, `upstream error`, or `backend error` are also treated as failover-worthy timeouts.OpenRouter-specific generic upstream text such as bare `Provider returned error` is treated as timeout only when the provider context is actually OpenRouter. Generic internal fallback text such as `LLM request failed with an unknown error.` stays conservative and does not trigger failover by itself.

SDK retry-after caps

Some provider SDKs may otherwise sleep for a long `Retry-After` window before returning control to OpenClaw. For Stainless-based SDKs such as Anthropic and OpenAI, OpenClaw caps SDK-internal `retry-after-ms` / `retry-after` waits at 60 seconds by default and surfaces longer retryable responses immediately so this failover path can run. Tune or disable the cap with `OPENCLAW_SDK_RETRY_MAX_WAIT_SECONDS`; see [Retry behavior](https://docs.openclaw.ai/concepts/retry).

Model-scoped cooldowns

Rate-limit cooldowns can also be model-scoped:

- OpenClaw records `cooldownModel` for rate-limit failures when the failing model id is known.
- A sibling model on the same provider can still be tried when the cooldown is scoped to a different model.
- Billing/disabled windows still block the whole profile across models.

Cooldowns use exponential backoff:

- 1 minute
- 5 minutes
- 25 minutes
- 1 hour (cap)

State is stored in `auth-state.json` under `usageStats`:

```
{
  "usageStats": {
    "provider:profile": {
      "lastUsed": 1736160000000,
      "cooldownUntil": 1736160600000,
      "errorCount": 2
    }
  }
}
```

## [​](https://docs.openclaw.ai/concepts/model-failover\#billing-disables)  Billing disables

Billing/credit failures (for example “insufficient credits” / “credit balance too low”) are treated as failover-worthy, but they’re usually not transient. Instead of a short cooldown, OpenClaw marks the profile as **disabled** (with a longer backoff) and rotates to the next profile/provider.

Not every billing-shaped response is `402`, and not every HTTP `402` lands here. OpenClaw keeps explicit billing text in the billing lane even when a provider returns `401` or `403` instead, but provider-specific matchers stay scoped to the provider that owns them (for example OpenRouter `403 Key limit exceeded`).Meanwhile temporary `402` usage-window and organization/workspace spend-limit errors are classified as `rate_limit` when the message looks retryable (for example `weekly usage limit exhausted`, `daily limit reached, resets tomorrow`, or `organization spending limit exceeded`). Those stay on the short cooldown/failover path instead of the long billing-disable path.

State is stored in `auth-state.json`:

```
{
  "usageStats": {
    "provider:profile": {
      "disabledUntil": 1736178000000,
      "disabledReason": "billing"
    }
  }
}
```

Defaults:

- Billing backoff starts at **5 hours**, doubles per billing failure, and caps at **24 hours**.
- Backoff counters reset if the profile hasn’t failed for **24 hours** (configurable).
- Overloaded retries allow **1 same-provider profile rotation** before model fallback.
- Overloaded retries use **0 ms backoff** by default.

## [​](https://docs.openclaw.ai/concepts/model-failover\#model-fallback)  Model fallback

If all profiles for a provider fail, OpenClaw moves to the next model in `agents.defaults.model.fallbacks`. This applies to auth failures, rate limits, and timeouts that exhausted profile rotation (other errors do not advance fallback). Provider errors that do not expose enough detail are still labeled precisely in fallback state: `empty_response` means the provider returned no usable message or status, `no_error_details` means the provider explicitly returned `Unknown error (no error details in response)`, and `unclassified` means OpenClaw preserved the raw preview but no classifier matched it yet.Overloaded and rate-limit errors are handled more aggressively than billing cooldowns. By default, OpenClaw allows one same-provider auth-profile retry, then switches to the next configured model fallback without waiting. Provider-busy signals such as `ModelNotReadyException` land in that overloaded bucket. Tune this with `auth.cooldowns.overloadedProfileRotations`, `auth.cooldowns.overloadedBackoffMs`, and `auth.cooldowns.rateLimitedProfileRotations`.When a run starts from the configured default primary, a cron job primary, an agent primary with explicit fallbacks, or an auto-selected fallback override, OpenClaw can walk the matching configured fallback chain. Agent primaries without explicit fallbacks and explicit user selections (for example `/model ollama/qwen3.5:27b`, the model picker, `sessions.patch`, or one-off CLI provider/model overrides) are strict: if that provider/model is unreachable or fails before producing a reply, OpenClaw reports the failure instead of answering from an unrelated fallback.

### [​](https://docs.openclaw.ai/concepts/model-failover\#candidate-chain-rules)  Candidate chain rules

OpenClaw builds the candidate list from the currently requested `provider/model` plus configured fallbacks.

Rules

- The requested model is always first.
- Explicit configured fallbacks are deduplicated but not filtered by the model allowlist. They are treated as explicit operator intent.
- If the current run is already on a configured fallback in the same provider family, OpenClaw keeps using the full configured chain.
- If the current run is on a different provider than config and that current model is not already part of the configured fallback chain, OpenClaw does not append unrelated configured fallbacks from another provider.
- When no explicit fallback override is supplied to the fallback runner, the configured primary is appended at the end so the chain can settle back onto the normal default once earlier candidates are exhausted.
- When a caller supplies `fallbacksOverride`, the runner uses exactly the requested model plus that override list. An empty list disables model fallback and prevents the configured primary from being appended as a hidden retry target.

### [​](https://docs.openclaw.ai/concepts/model-failover\#which-errors-advance-fallback)  Which errors advance fallback

- Continues on

- Does not continue on


- auth failures
- rate limits and cooldown exhaustion
- overloaded/provider-busy errors
- timeout-shaped failover errors
- billing disables
- `LiveSessionModelSwitchError`, which is normalized into a failover path so a stale persisted model does not create an outer retry loop
- other unrecognized errors when there are still remaining candidates

- explicit aborts that are not timeout/failover-shaped
- context overflow errors that should stay inside compaction/retry logic (for example `request_too_large`, `INVALID_ARGUMENT: input exceeds the maximum number of tokens`, `input token count exceeds the maximum number of input tokens`, `The input is too long for the model`, or `ollama error: context length exceeded`)
- a final unknown error when there are no candidates left

### [​](https://docs.openclaw.ai/concepts/model-failover\#cooldown-skip-vs-probe-behavior)  Cooldown skip vs probe behavior

When every auth profile for a provider is already in cooldown, OpenClaw does not automatically skip that provider forever. It makes a per-candidate decision:

Per-candidate decisions

- Persistent auth failures skip the whole provider immediately.
- Billing disables usually skip, but the primary candidate can still be probed on a throttle so recovery is possible without restarting.
- The primary candidate may be probed near cooldown expiry, with a per-provider throttle.
- Same-provider fallback siblings can be attempted despite cooldown when the failure looks transient (`rate_limit`, `overloaded`, or unknown). This is especially relevant when a rate limit is model-scoped and a sibling model may still recover immediately.
- Transient cooldown probes are limited to one per provider per fallback run so a single provider does not stall cross-provider fallback.

## [​](https://docs.openclaw.ai/concepts/model-failover\#session-overrides-and-live-model-switching)  Session overrides and live model switching

Session model changes are shared state. The active runner, `/model` command, compaction/session updates, and live-session reconciliation all read or write parts of the same session entry.That means fallback retries have to coordinate with live model switching:

- Only explicit user-driven model changes mark a pending live switch. That includes `/model`, `session_status(model=...)`, and `sessions.patch`.
- System-driven model changes such as fallback rotation, heartbeat overrides, or compaction never mark a pending live switch on their own.
- User-driven model overrides are treated as exact selections for fallback policy, so an unreachable selected provider surfaces as a failure instead of being masked by `agents.defaults.model.fallbacks`.
- Before a fallback retry starts, the reply runner persists the selected fallback override fields to the session entry.
- Auto fallback overrides remain selected on subsequent turns so OpenClaw does not probe a known-bad primary on every message. `/new`, `/reset`, and `sessions.reset` clear auto-sourced overrides and return the session to the configured default.
- `/status` shows the selected model and, when fallback state differs, the active fallback model and reason.
- Live-session reconciliation prefers persisted session overrides over stale runtime model fields.
- If a live-switch error points at a later candidate in the active fallback chain, OpenClaw jumps directly to that selected model instead of walking unrelated candidates first.
- If the fallback attempt fails, the runner rolls back only the override fields it wrote, and only if they still match that failed candidate.

This prevents the classic race:

1

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Primary fails

The selected primary model fails.

2

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Fallback chosen in memory

Fallback candidate is chosen in memory.

3

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Session store still says old primary

Session store still reflects the old primary.

4

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Live reconciliation reads stale state

Live-session reconciliation reads the stale session state.

5

[Navigate to header](https://docs.openclaw.ai/concepts/model-failover#)

Retry snapped back

The retry gets snapped back to the old model before the fallback attempt starts.

The persisted fallback override closes that window, and the narrow rollback keeps newer manual or runtime session changes intact.

## [​](https://docs.openclaw.ai/concepts/model-failover\#observability-and-failure-summaries)  Observability and failure summaries

`runWithModelFallback(...)` records per-attempt details that feed logs and user-facing cooldown messaging:

- provider/model attempted
- reason (`rate_limit`, `overloaded`, `billing`, `auth`, `model_not_found`, and similar failover reasons)
- optional status/code
- human-readable error summary

Structured `model_fallback_decision` logs also include flat `fallbackStep*` fields when a candidate fails, is skipped, or a later fallback succeeds. These fields make the attempted transition explicit (`fallbackStepFromModel`, `fallbackStepToModel`, `fallbackStepFromFailureReason`, `fallbackStepFromFailureDetail`, `fallbackStepFinalOutcome`) so log and diagnostic exporters can reconstruct the primary failure even when the terminal fallback also fails.When every candidate fails, OpenClaw throws `FallbackSummaryError`. The outer reply runner can use that to build a more specific message such as “all models are temporarily rate-limited” and include the soonest cooldown expiry when one is known.That cooldown summary is model-aware:

- unrelated model-scoped rate limits are ignored for the attempted provider/model chain
- if the remaining block is a matching model-scoped rate limit, OpenClaw reports the last matching expiry that still blocks that model

## [​](https://docs.openclaw.ai/concepts/model-failover\#related-config)  Related config

See [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) for:

- `auth.profiles` / `auth.order`
- `auth.cooldowns.billingBackoffHours` / `auth.cooldowns.billingBackoffHoursByProvider`
- `auth.cooldowns.billingMaxHours` / `auth.cooldowns.failureWindowHours`
- `auth.cooldowns.overloadedProfileRotations` / `auth.cooldowns.overloadedBackoffMs`
- `auth.cooldowns.rateLimitedProfileRotations`
- `agents.defaults.model.primary` / `agents.defaults.model.fallbacks`
- `agents.defaults.imageModel` routing

See [Models](https://docs.openclaw.ai/concepts/models) for the broader model selection and fallback overview.

[Model providers](https://docs.openclaw.ai/concepts/model-providers) [Alibaba Model Studio](https://docs.openclaw.ai/providers/alibaba)

Ctrl+I