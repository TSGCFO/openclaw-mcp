# Reference

_15 pages from docs.openclaw.ai_


---

## Auth credential semantics - OpenClaw

_Source: <https://docs.openclaw.ai/auth-credential-semantics>_

[OpenClaw home page](https://docs.openclaw.ai/)

Authentication and secrets

Auth credential semantics

This document defines the canonical credential eligibility and resolution semantics used across:

- `resolveAuthProfileOrder`
- `resolveApiKeyForProfile`
- `models status --probe`
- `doctor-auth`

The goal is to keep selection-time and runtime behavior aligned.

## Stable probe reason codes

- `ok`
- `excluded_by_auth_order`
- `missing_credential`
- `invalid_expires`
- `expired`
- `unresolved_ref`
- `no_model`

## Token credentials

Token credentials (`type: "token"`) support inline `token` and/or `tokenRef`.

### Eligibility rules

1. A token profile is ineligible when both `token` and `tokenRef` are absent.
2. `expires` is optional.
3. If `expires` is present, it must be a finite number greater than `0`.
4. If `expires` is invalid (`NaN`, `0`, negative, non-finite, or wrong type), the profile is ineligible with `invalid_expires`.
5. If `expires` is in the past, the profile is ineligible with `expired`.
6. `tokenRef` does not bypass `expires` validation.

### Resolution rules

1. Resolver semantics match eligibility semantics for `expires`.
2. For eligible profiles, token material may be resolved from inline value or `tokenRef`.
3. Unresolvable refs produce `unresolved_ref` in `models status --probe` output.

## Agent copy portability

Agent auth inheritance is read-through. When an agent has no local profile, it
can resolve profiles from the default/main agent store at runtime without
copying secret material into its own `auth-profiles.json`.Explicit copy flows, such as `openclaw agents add`, use this portability policy:

- `api_key` profiles are portable unless `copyToAgents: false`.
- `token` profiles are portable unless `copyToAgents: false`.
- `oauth` profiles are not portable by default because refresh tokens can be
single-use or rotation-sensitive.
- Provider-owned OAuth flows may opt in with `copyToAgents: true` only when
copying refresh material across agents is known safe.

Non-portable profiles remain available through read-through inheritance unless
the target agent signs in separately and creates its own local profile.

## Explicit auth order filtering

- When `auth.order.<provider>` or the auth-store order override is set for a
provider, `models status --probe` only probes profile ids that remain in the
resolved auth order for that provider.
- A stored profile for that provider that is omitted from the explicit order is
not silently tried later. Probe output reports it with
`reasonCode: excluded_by_auth_order` and the detail
`Excluded by auth.order for this provider.`

## Probe target resolution

- Probe targets can come from auth profiles, environment credentials, or
`models.json`.
- If a provider has credentials but OpenClaw cannot resolve a probeable model
candidate for it, `models status --probe` reports `status: no_model` with
`reasonCode: no_model`.

## External CLI credential discovery

- Runtime-only credentials owned by external CLIs are discovered only when the
provider, runtime, or auth profile is in scope for the current operation, or
when a stored local profile for that external source already exists.
- Auth-store callers should choose an explicit external-CLI discovery mode:
`none` for persisted/plugin auth only, `existing` for refreshing already
stored external CLI profiles, or `scoped` for a concrete provider/profile set.
- Read-only/status paths pass `allowKeychainPrompt: false`; they use file-backed
external CLI credentials only and do not read or reuse macOS Keychain results.

## OAuth SecretRef Policy Guard

- SecretRef input is for static credentials only.
- If a profile credential is `type: "oauth"`, SecretRef objects are not supported for that profile credential material.
- If `auth.profiles.<id>.mode` is `"oauth"`, SecretRef-backed `keyRef`/`tokenRef` input for that profile is rejected.
- Violations are hard failures in startup/reload auth resolution paths.

## Legacy-Compatible Messagi

_… [truncated; see https://docs.openclaw.ai/auth-credential-semantics for full content]_


---

## Release policy - OpenClaw

_Source: <https://docs.openclaw.ai/reference/RELEASING>_

# Validate an unpublished release candidate branch.
gh workflow run full-release-validation.yml \
  --ref main \
  -f ref=release/YYYY.M.D \
  -f provider=openai \
  -f mode=both \
  -f release_profile=stable

# Validate an exact pushed commit.
gh workflow run full-release-validation.yml \
  --ref main \
  -f ref=<40-char-sha> \
  -f provider=openai \
  -f mode=both

# After publishing a beta, add published-package Telegram E2E.
gh workflow run full-release-validation.yml \
  --ref main \
  -f ref=release/YYYY.M.D \
  -f provider=openai \
  -f mode=both \
  -f release_profile=full \
  -f evidence_package_spec=openclaw@YYYY.M.D-beta.N \
  -f npm_telegram_package_spec=openclaw@YYYY.M.D-beta.N \
  -f npm_telegram_provider_mode=mock-openai
```

Do not use the full umbrella as the first rerun after a focused fix. If one box
fails, use the failed child workflow, job, Docker lane, package profile, model
provider, or QA lane for the next proof. Run the full umbrella again only when
the fix changed shared release orchestration or made earlier all-box evidence
stale. The umbrella’s final verifier re-checks the recorded child workflow run
ids, so after a child workflow is rerun successfully, rerun only the failed
`Verify full validation` parent job.For bounded recovery, pass `rerun_group` to the umbrella. `all` is the real
release-candidate run, `ci` runs only the normal CI child, `plugin-prerelease`
runs only the release-only plugin child, `release-checks` runs every release
box, and the narrower release groups are `install-smoke`, `cross-os`,
`live-e2e`, `package`, `qa`, `qa-parity`, `qa-live`, and `npm-telegram`.
Focused `npm-telegram` reruns require `npm_telegram_package_spec`; full/all runs
with `release_profile=full` use the release-checks package artifact.

### Vitest

The Vitest box is the manual `CI` child workflow. Manual CI intentionally
bypasses changed scoping and forces the normal test graph for the release
candidate: Linux Node shards, bundled-plugin shards, channel contracts, Node 22
compatibility, `check`, `check-additional`, build smoke, docs checks, Python
skills, Windows, macOS, Android, and Control UI i18n.Use this box to answer “did the source tree pass the full normal test suite?”
It is not the same as release-path product validation. Evidence to keep:

- `Full Release Validation` summary showing the dispatched `CI` run URL
- `CI` run green on the exact target SHA
- failed or slow shard names from the CI jobs when investigating regressions
- Vitest timing artifacts such as `.artifacts/vitest-shard-timings.json` when
a run needs performance analysis

Run manual CI directly only when the release needs deterministic normal CI but
not the Docker, QA Lab, live, cross-OS, or package boxes:

```
gh workflow run ci.yml --ref main -f target_ref=release/YYYY.M.D
```

### Docker

The Docker box lives in `OpenClaw Release Checks` through
`openclaw-live-and-e2e-checks-reusable.yml`, plus the release-mode
`install-smoke` workflow. It validates the release candidate through packaged
Docker environments instead of only source-level tests.Release Docker coverage includes:

- full install smoke with the slow Bun global install smoke enabled
- root Dockerfile smoke image preparation/reuse by target SHA, with QR,
root/gateway, and installer/Bun smoke jobs running as separate install-smoke
shards
- repository E2E lanes
- release-path Docker chunks: `core`, `package-update-openai`,
`package-update-anthropic`, `package-update-core`, `plugins-runtime-plugins`,
`plugins-runtime-services`,
`plugins-runtime-install-a`, `plugins-runtime-install-b`,
`plugins-runtime-install-c`, `plugins-runtime-install-d`,
`plugins-runtime-install-e`, `plugins-runtime-install-f`,
`plugins-runtime-install-g`, and `plugins-runtime-install-h`
- OpenWebUI coverage inside the `plugins-runtime-services` chunk when requested
- split bundled plugin install/uninstall lanes
`bundled-plugin-install-uninstall-0` through
`bundled-plugin-install-uninstall-23`
- live/E2E pro

_… [truncated; see https://docs.openclaw.ai/reference/RELEASING for full content]_


---

## API usage and costs - OpenClaw

_Source: <https://docs.openclaw.ai/reference/api-usage-costs>_

# API usage & costs

This doc lists **features that can invoke API keys** and where their costs show up. It focuses on
OpenClaw features that can generate provider usage or paid API calls.

## Where costs show up (chat + CLI)

**Per-session cost snapshot**

- `/status` shows the current session model, context usage, and last response tokens.
- If the model uses **API-key auth**, `/status` also shows **estimated cost** for the last reply.
- If live session metadata is sparse, `/status` can recover token/cache
counters and the active runtime model label from the latest transcript usage
entry. Existing nonzero live values still take precedence, and prompt-sized
transcript totals can win when stored totals are missing or smaller.

**Per-message cost footer**

- `/usage full` appends a usage footer to every reply, including **estimated cost** (API-key only).
- `/usage tokens` shows tokens only; subscription-style OAuth/token and CLI flows hide dollar cost.
- Gemini CLI note: when the CLI returns JSON output, OpenClaw reads usage from
`stats`, normalizes `stats.cached` into `cacheRead`, and derives input tokens
from `stats.input_tokens - stats.cached` when needed.

Anthropic note: Anthropic staff told us OpenClaw-style Claude CLI usage is
allowed again, so OpenClaw treats Claude CLI reuse and `claude -p` usage as
sanctioned for this integration unless Anthropic publishes a new policy.
Anthropic still does not expose a per-message dollar estimate that OpenClaw can
show in `/usage full`.**CLI usage windows (provider quotas)**

- `openclaw status --usage` and `openclaw channels list` show provider **usage windows**
(quota snapshots, not per-message costs).
- Human output is normalized to `X% left` across providers.
- Current usage-window providers: Anthropic, GitHub Copilot, Gemini CLI,
OpenAI Codex, MiniMax, Xiaomi, and z.ai.
- MiniMax note: its raw `usage_percent` / `usagePercent` fields mean remaining
quota, so OpenClaw inverts them before display. Count-based fields still win
when present. If the provider returns `model_remains`, OpenClaw prefers the
chat-model entry, derives the window label from timestamps when needed, and
includes the model name in the plan label.
- Usage auth for those quota windows comes from provider-specific hooks when
available; otherwise OpenClaw falls back to matching OAuth/API-key
credentials from auth profiles, env, or config.

See [Token use & costs](https://docs.openclaw.ai/reference/token-use) for details and examples.

## How keys are discovered

OpenClaw can pick up credentials from:

- **Auth profiles** (per-agent, stored in `auth-profiles.json`).
- **Environment variables** (e.g. `OPENAI_API_KEY`, `BRAVE_API_KEY`, `FIRECRAWL_API_KEY`).
- **Config** (`models.providers.*.apiKey`, `plugins.entries.*.config.webSearch.apiKey`,
`plugins.entries.firecrawl.config.webFetch.apiKey`, `memorySearch.*`,
`talk.providers.*.apiKey`).
- **Skills** (`skills.entries.<name>.apiKey`) which may export keys to the skill process env.

## Features that can spend keys

### 1) Core model responses (chat + tools)

Every reply or tool call uses the **current model provider** (OpenAI, Anthropic, etc). This is the
primary source of usage and cost.This also includes subscription-style hosted providers that still bill outside
OpenClaw’s local UI, such as **OpenAI Codex**, **Alibaba Cloud Model Studio**
**Coding Plan**, **MiniMax Coding Plan**, **Z.AI / GLM Coding Plan**, and
Anthropic’s OpenClaw Claude-login path with **Extra Usage** enabled.See [Models](https://docs.openclaw.ai/providers/models) for pricing config and [Token use & costs](https://docs.openclaw.ai/reference/token-use) for display.

### 2) Media understanding (audio/image/video)

Inbound media can be summarized/transcribed before the reply runs. This uses model/provider APIs.

- Audio: OpenAI / Groq / Deepgram / DeepInfra / Google / Mistral.
- Image: OpenAI / OpenRouter / Anthropic / DeepInfra / Google / MiniMax / Moonshot / Qwen / Z.AI.
- Video: Google / Qwen / M

_… [truncated; see https://docs.openclaw.ai/reference/api-usage-costs for full content]_


---

## Application modernization plan - OpenClaw

_Source: <https://docs.openclaw.ai/reference/application-modernization-plan>_

# Application modernization plan

## Goal

Move the application toward a cleaner, faster, more maintainable product without
breaking current workflows or hiding risk in broad refactors. The work should
land as small, reviewable slices with proof for each touched surface.

## Principles

- Preserve current architecture unless a boundary is demonstrably causing churn,
performance cost, or user-visible bugs.
- Prefer the smallest correct patch for each issue, then repeat.
- Separate required fixes from optional polish so maintainers can land high
value work without waiting on subjective decisions.
- Keep plugin-facing behavior documented and backwards compatible.
- Verify shipped behavior, dependency contracts, and tests before claiming a
regression is fixed.
- Make the main user path better first: onboarding, auth, chat, provider setup,
plugin management, and diagnostics.

## Phase 1: Baseline audit

Inventory the current application before changing it.

- Identify the top user workflows and the code surfaces that own them.
- List dead affordances, duplicate settings, unclear error states, and expensive
render paths.
- Capture current validation commands for each surface.
- Mark issues as required, recommended, or optional.
- Document known blockers that need owner review, especially API, security,
release, and plugin contract changes.

Definition of done:

- One issue list with repo-root file references.
- Each issue has severity, owner surface, expected user impact, and a proposed
validation path.
- No speculative cleanup items are mixed into required fixes.

## Phase 2: Product and UX cleanup

Prioritize visible workflows and remove confusion.

- Tighten onboarding copy and empty states around model auth, gateway status,
and plugin setup.
- Remove or disable dead affordances where no action is possible.
- Keep important actions visible across responsive widths instead of hiding them
behind fragile layout assumptions.
- Consolidate repeated status language so errors have one source of truth.
- Add progressive disclosure for advanced settings while keeping core setup fast.

Recommended validation:

- Manual happy path for first-run setup and existing user startup.
- Focused tests for any routing, config persistence, or status derivation logic.
- Browser screenshots for changed responsive surfaces.

## Phase 3: Frontend architecture tightening

Improve maintainability without a broad rewrite.

- Move repeated UI state transformations into narrow typed helpers.
- Keep data fetching, persistence, and presentation responsibilities separate.
- Prefer existing hooks, stores, and component patterns over new abstractions.
- Split oversized components only when it reduces coupling or clarifies tests.
- Avoid introducing broad global state for local panel interactions.

Required guardrails:

- Do not change public behavior as a side effect of file splitting.
- Keep accessibility behavior intact for menus, dialogs, tabs, and keyboard
navigation.
- Verify that loading, empty, error, and optimistic states still render.

## Phase 4: Performance and reliability

Target measured pain rather than broad theoretical optimization.

- Measure startup, route transition, large list, and chat transcript costs.
- Replace repeated expensive derived data with memoized selectors or cached
helpers where profiling proves value.
- Reduce avoidable network or filesystem scans on hot paths.
- Keep deterministic ordering for prompt, registry, file, plugin, and network
inputs before model payload construction.
- Add lightweight regression tests for hot helpers and contract boundaries.

Definition of done:

- Each performance change records baseline, expected impact, actual impact, and
remaining gap.
- No perf patch lands solely on intuition when cheap measurement is available.

## Phase 5: Type, contract, and test hardening

Raise correctness at the boundary points users and plugin authors depend on.

- Replace loose runtime strings with discriminated unions or

_… [truncated; see https://docs.openclaw.ai/reference/application-modernization-plan for full content]_


---

## Full release validation - OpenClaw

_Source: <https://docs.openclaw.ai/reference/full-release-validation>_

[OpenClaw home page](https://docs.openclaw.ai/)

Release and CI

Full release validation

`Full Release Validation` is the release umbrella. It is the single manual
entrypoint for pre-release proof, but most work happens in child workflows so a
failed box can be rerun without restarting the whole release.Run it from a trusted workflow ref, normally `main`, and pass the release branch,
tag, or full commit SHA as `ref`:

```
gh workflow run full-release-validation.yml \
  --ref main \
  -f ref=release/YYYY.M.D \
  -f provider=openai \
  -f mode=both \
  -f release_profile=stable
```

Child workflows use the trusted workflow ref for the harness and the input
`ref` for the candidate under test. That keeps new validation logic available
when validating an older release branch or tag.Package Acceptance normally builds the candidate tarball from the resolved
`ref`, including full-SHA runs dispatched with `pnpm ci:full-release`. After
publish, pass `package_acceptance_package_spec=openclaw@YYYY.M.D` (or
`openclaw@beta`/`openclaw@latest`) to run the same package/update matrix against
the shipped npm package instead.

## Top-level stages

| Stage | Details |
| --- | --- |
| Target resolution | **Job:**`Resolve target ref`<br>**Child workflow:** none<br>**Proves:** resolves the release branch, tag, or full commit SHA and records selected inputs.<br>**Rerun:** rerun the umbrella if this fails. |
| Vitest and normal CI | **Job:**`Run normal full CI`<br>**Child workflow:**`CI`<br>**Proves:** manual full CI graph against the target ref, including Linux Node lanes, bundled plugin shards, channel contracts, Node 22 compatibility, `check`, `check-additional`, build smoke, docs checks, Python skills, Windows, macOS, Control UI i18n, and Android via the umbrella.<br>**Rerun:**`rerun_group=ci`. |
| Plugin prerelease | **Job:**`Run plugin prerelease validation`<br>**Child workflow:**`Plugin Prerelease`<br>**Proves:** release-only plugin static checks, agentic plugin coverage, full extension batch shards, and plugin prerelease Docker lanes.<br>**Rerun:**`rerun_group=plugin-prerelease`. |
| Release checks | **Job:**`Run release/live/Docker/QA validation`<br>**Child workflow:**`OpenClaw Release Checks`<br>**Proves:** install smoke, cross-OS package checks, live/E2E suites, Docker release-path chunks, Package Acceptance, QA Lab parity, live Matrix, and live Telegram.<br>**Rerun:**`rerun_group=release-checks` or a narrower release-checks handle. |
| Package artifact | **Job:**`Prepare release package artifact`<br>**Child workflow:** none<br>**Proves:** creates the parent `release-package-under-test` tarball early enough for package-facing checks that do not need to wait for `OpenClaw Release Checks`.<br>**Rerun:** rerun the umbrella or provide `npm_telegram_package_spec` for `rerun_group=npm-telegram`. |
| Package Telegram | **Job:**`Run package Telegram E2E`<br>**Child workflow:**`NPM Telegram Beta E2E`<br>**Proves:** parent-artifact-backed Telegram package proof for `rerun_group=all` with `release_profile=full`, or published-package Telegram proof when `npm_telegram_package_spec` is set.<br>**Rerun:**`rerun_group=npm-telegram` with `npm_telegram_package_spec`. |
| Umbrella verifier | **Job:**`Verify full validation`<br>**Child workflow:** none<br>**Proves:** re-checks recorded child run conclusions and appends slowest-job tables from child workflows.<br>**Rerun:** rerun only this job after rerunning a failed child to green. |

For `ref=main` and `rerun_group=all`, a newer umbrella supersedes an older one.
When the parent is cancelled, its monitor cancels any child workflow it already
dispatched. Release branch and tag validation runs do not cancel each other by
default.

## Release checks stages

`OpenClaw Release Checks` is the largest child workflow. It resolves the target
once and prepares a shared `release-package-under-test` artifact when package
or Docker-facing stages need it.

| Stage | Details |
| --- | --- |
| Release target | **Job:**`Resol

_… [truncated; see https://docs.openclaw.ai/reference/full-release-validation for full content]_


---

## Memory configuration reference - OpenClaw

_Source: <https://docs.openclaw.ai/reference/memory-config>_

[OpenClaw home page](https://docs.openclaw.ai/)

Technical reference

Memory configuration reference

This page lists every configuration knob for OpenClaw memory search. For conceptual overviews, see:

[**Memory overview** \\
\\
How memory works.](https://docs.openclaw.ai/concepts/memory)

[**Builtin engine** \\
\\
Default SQLite backend.](https://docs.openclaw.ai/concepts/memory-builtin)

[**QMD engine** \\
\\
Local-first sidecar.](https://docs.openclaw.ai/concepts/memory-qmd)

[**Memory search** \\
\\
Search pipeline and tuning.](https://docs.openclaw.ai/concepts/memory-search)

[**Active memory** \\
\\
Memory sub-agent for interactive sessions.](https://docs.openclaw.ai/concepts/active-memory)

All memory search settings live under `agents.defaults.memorySearch` in `openclaw.json` unless noted otherwise.

If you are looking for the **active memory** feature toggle and sub-agent config, that lives under `plugins.entries.active-memory` instead of `memorySearch`.Active memory uses a two-gate model:

1. the plugin must be enabled and target the current agent id
2. the request must be an eligible interactive persistent chat session

See [Active Memory](https://docs.openclaw.ai/concepts/active-memory) for the activation model, plugin-owned config, transcript persistence, and safe rollout pattern.

* * *

## Provider selection

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `provider` | `string` | auto-detected | Embedding adapter ID such as `bedrock`, `deepinfra`, `gemini`, `github-copilot`, `local`, `mistral`, `ollama`, `openai`, or `voyage`; may also be a configured `models.providers.<id>` whose `api` points at one of those adapters |
| `model` | `string` | provider default | Embedding model name |
| `fallback` | `string` | `"none"` | Fallback adapter ID when the primary fails |
| `enabled` | `boolean` | `true` | Enable or disable memory search |

### Auto-detection order

When `provider` is not set, OpenClaw selects the first available:

1

[Navigate to header](https://docs.openclaw.ai/reference/memory-config#)

local

Selected if `memorySearch.local.modelPath` is configured and the file exists.

2

[Navigate to header](https://docs.openclaw.ai/reference/memory-config#)

github-copilot

Selected if a GitHub Copilot token can be resolved (env var or auth profile).

3

[Navigate to header](https://docs.openclaw.ai/reference/memory-config#)

openai

Selected if an OpenAI key can be resolved.

4

[Navigate to header](https://docs.openclaw.ai/reference/memory-config#)

gemini

Selected if a Gemini key can be resolved.

5

[Navigate to header](https://docs.openclaw.ai/reference/memory-config#)

voyage

Selected if a Voyage key can be resolved.

6

[Navigate to header](https://docs.openclaw.ai/reference/memory-config#)

mistral

Selected if a Mistral key can be resolved.

7

[Navigate to header](https://docs.openclaw.ai/reference/memory-config#)

deepinfra

Selected if a DeepInfra key can be resolved.

8

[Navigate to header](https://docs.openclaw.ai/reference/memory-config#)

bedrock

Selected if the AWS SDK credential chain resolves (instance role, access keys, profile, SSO, web identity, or shared config).

`ollama` is supported but not auto-detected (set it explicitly).

### Custom provider ids

`memorySearch.provider` can point at a custom `models.providers.<id>` entry. OpenClaw resolves that provider’s `api` owner for the embedding adapter while preserving the custom provider id for endpoint, auth, and model-prefix handling. This lets multi-GPU or multi-host setups dedicate memory embeddings to a specific local endpoint:

```
{
  models: {
    providers: {
      "ollama-5080": {
        api: "ollama",
        baseUrl: "http://gpu-box.local:11435",
        apiKey: "ollama-local",
        models: [{ id: "qwen3-embedding:0.6b" }],
      },
    },
  },
  agents: {
    defaults: {
      memorySearch: {
        provider: "ollama-5080",
        model: "qwen3-embedding:0.6b",
      },
    },
  },
}
```

### API key

_… [truncated; see https://docs.openclaw.ai/reference/memory-config for full content]_


---

## OpenClaw App SDK API design - OpenClaw

_Source: <https://docs.openclaw.ai/reference/openclaw-sdk-api-design>_

[OpenClaw home page](https://docs.openclaw.ai/)

RPC and API

OpenClaw App SDK API design

This page is the detailed API reference design for the public
[OpenClaw App SDK](https://docs.openclaw.ai/concepts/openclaw-sdk). It is intentionally separate from
the [Plugin SDK](https://docs.openclaw.ai/plugins/sdk-overview).

`@openclaw/sdk` is the external app/client package for talking to the
Gateway. `openclaw/plugin-sdk/*` is the in-process plugin authoring contract.
Do not import Plugin SDK subpaths from apps that only need to run agents.

The public app SDK should be built in two layers:

1. A low-level generated Gateway client.
2. A high-level ergonomic wrapper with `OpenClaw`, `Agent`, `Session`, `Run`,
`Task`, `Artifact`, `Approval`, and `Environment` objects.

## Namespace design

The low-level namespaces should closely follow Gateway resources:

```
oc.agents.list();
oc.agents.get("main");
oc.agents.create(...);
oc.agents.update(...);

oc.sessions.list();
oc.sessions.create(...);
oc.sessions.resolve(...);
oc.sessions.send(...);
oc.sessions.messages(...);
oc.sessions.fork(...);
oc.sessions.compact(...);
oc.sessions.abort(...);

oc.runs.create(...);
oc.runs.get(runId);
oc.runs.events(runId, { after });
oc.runs.wait(runId);
oc.runs.cancel(runId);

oc.tasks.list(); // future API: current SDK throws unsupported
oc.tasks.get(taskId); // future API: current SDK throws unsupported
oc.tasks.cancel(taskId); // future API: current SDK throws unsupported
oc.tasks.events(taskId, { after }); // future API

oc.models.list();
oc.models.status(); // Gateway models.authStatus

oc.tools.list();
oc.tools.invoke(...); // future API: current SDK throws unsupported

oc.artifacts.list({ runId }); // future API: current SDK throws unsupported
oc.artifacts.get(artifactId); // future API: current SDK throws unsupported
oc.artifacts.download(artifactId); // future API: current SDK throws unsupported

oc.approvals.list();
oc.approvals.respond(approvalId, ...);

oc.environments.list(); // future API: current SDK throws unsupported
oc.environments.create(...); // future API: current SDK throws unsupported
oc.environments.status(environmentId); // future API: current SDK throws unsupported
oc.environments.delete(environmentId); // future API: current SDK throws unsupported
```

High-level wrappers should return objects that make common flows pleasant:

```
const run = await agent.run(inputOrParams);
await run.cancel();
await run.wait();

for await (const event of run.events()) {
  // normalized event stream
}

const artifacts = await run.artifacts.list();
const session = await run.session();
```

## Event contract

The public SDK should expose versioned, replayable, normalized events.

```
type OpenClawEvent = {
  version: 1;
  id: string;
  ts: number;
  type: OpenClawEventType;
  runId?: string;
  sessionId?: string;
  sessionKey?: string;
  taskId?: string;
  agentId?: string;
  data: unknown;
  raw?: unknown;
};
```

`id` is a replay cursor. Consumers should be able to reconnect with
`events({ after: id })` and receive missed events when retention allows.Recommended normalized event families:

| Event | Meaning |
| --- | --- |
| `run.created` | Run accepted. |
| `run.queued` | Run is waiting for a session lane, runtime, or environment. |
| `run.started` | Runtime started execution. |
| `run.completed` | Run finished successfully. |
| `run.failed` | Run ended with an error. |
| `run.cancelled` | Run was cancelled. |
| `run.timed_out` | Run exceeded its timeout. |
| `assistant.delta` | Assistant text delta. |
| `assistant.message` | Complete assistant message or replacement. |
| `thinking.delta` | Reasoning or plan delta, when policy allows exposure. |
| `tool.call.started` | Tool call began. |
| `tool.call.delta` | Tool call streamed progress or partial output. |
| `tool.call.completed` | Tool call returned successfully. |
| `tool.call.failed` | Tool call failed. |
| `approval.requested` | A run or tool needs approval. |
| `approval.resolved` | Approv

_… [truncated; see https://docs.openclaw.ai/reference/openclaw-sdk-api-design for full content]_


---

## Prompt caching - OpenClaw

_Source: <https://docs.openclaw.ai/reference/prompt-caching>_

[OpenClaw home page](https://docs.openclaw.ai/)

Technical reference

Prompt caching

Prompt caching means the model provider can reuse unchanged prompt prefixes (usually system/developer instructions and other stable context) across turns instead of re-processing them every time. OpenClaw normalizes provider usage into `cacheRead` and `cacheWrite` where the upstream API exposes those counters directly.Status surfaces can also recover cache counters from the most recent transcript
usage log when the live session snapshot is missing them, so `/status` can keep
showing a cache line after partial session metadata loss. Existing nonzero live
cache values still take precedence over transcript fallback values.Why this matters: lower token cost, faster responses, and more predictable performance for long-running sessions. Without caching, repeated prompts pay the full prompt cost on every turn even when most input did not change.The sections below cover every cache-related knob that affects prompt reuse and token cost.Provider references:

- Anthropic prompt caching: [https://platform.claude.com/docs/en/build-with-claude/prompt-caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- OpenAI prompt caching: [https://developers.openai.com/api/docs/guides/prompt-caching](https://developers.openai.com/api/docs/guides/prompt-caching)
- OpenAI API headers and request IDs: [https://developers.openai.com/api/reference/overview](https://developers.openai.com/api/reference/overview)
- Anthropic request IDs and errors: [https://platform.claude.com/docs/en/api/errors](https://platform.claude.com/docs/en/api/errors)

## Primary knobs

### `cacheRetention` (global default, model, and per-agent)

Set cache retention as a global default for all models:

```
agents:
  defaults:
    params:
      cacheRetention: "long" # none | short | long
```

Override per-model:

```
agents:
  defaults:
    models:
      "anthropic/claude-opus-4-6":
        params:
          cacheRetention: "short" # none | short | long
```

Per-agent override:

```
agents:
  list:
    - id: "alerts"
      params:
        cacheRetention: "none"
```

Config merge order:

1. `agents.defaults.params` (global default — applies to all models)
2. `agents.defaults.models["provider/model"].params` (per-model override)
3. `agents.list[].params` (matching agent id; overrides by key)

### `contextPruning.mode: "cache-ttl"`

Prunes old tool-result context after cache TTL windows so post-idle requests do not re-cache oversized history.

```
agents:
  defaults:
    contextPruning:
      mode: "cache-ttl"
      ttl: "1h"
```

See [Session Pruning](https://docs.openclaw.ai/concepts/session-pruning) for full behavior.

### Heartbeat keep-warm

Heartbeat can keep cache windows warm and reduce repeated cache writes after idle gaps.

```
agents:
  defaults:
    heartbeat:
      every: "55m"
```

Per-agent heartbeat is supported at `agents.list[].heartbeat`.

## Provider behavior

### Anthropic (direct API)

- `cacheRetention` is supported.
- With Anthropic API-key auth profiles, OpenClaw seeds `cacheRetention: "short"` for Anthropic model refs when unset.
- Anthropic native Messages responses expose both `cache_read_input_tokens` and `cache_creation_input_tokens`, so OpenClaw can show both `cacheRead` and `cacheWrite`.
- For native Anthropic requests, `cacheRetention: "short"` maps to the default 5-minute ephemeral cache, and `cacheRetention: "long"` upgrades to the 1-hour TTL only on direct `api.anthropic.com` hosts.

### OpenAI (direct API)

- Prompt caching is automatic on supported recent models. OpenClaw does not need to inject block-level cache markers.
- OpenClaw uses `prompt_cache_key` to keep cache routing stable across turns and uses `prompt_cache_retention: "24h"` only when `cacheRetention: "long"` is selected on direct OpenAI hosts.
- OpenAI-compatible Completions providers receive `prompt_cache_key` only when their model config explicitly sets `compat.supportsPromp

_… [truncated; see https://docs.openclaw.ai/reference/prompt-caching for full content]_


---

## SecretRef credential surface - OpenClaw

_Source: <https://docs.openclaw.ai/reference/secretref-credential-surface>_

[OpenClaw home page](https://docs.openclaw.ai/)

Technical reference

SecretRef credential surface

This page defines the canonical SecretRef credential surface.Scope intent:

- In scope: strictly user-supplied credentials that OpenClaw does not mint or rotate.
- Out of scope: runtime-minted or rotating credentials, OAuth refresh material, and session-like artifacts.

## Supported credentials

### `openclaw.json` targets (`secrets configure` \+ `secrets apply` \+ `secrets audit`)

- `models.providers.*.apiKey`
- `models.providers.*.headers.*`
- `models.providers.*.request.auth.token`
- `models.providers.*.request.auth.value`
- `models.providers.*.request.headers.*`
- `models.providers.*.request.proxy.tls.ca`
- `models.providers.*.request.proxy.tls.cert`
- `models.providers.*.request.proxy.tls.key`
- `models.providers.*.request.proxy.tls.passphrase`
- `models.providers.*.request.tls.ca`
- `models.providers.*.request.tls.cert`
- `models.providers.*.request.tls.key`
- `models.providers.*.request.tls.passphrase`
- `skills.entries.*.apiKey`
- `agents.defaults.memorySearch.remote.apiKey`
- `agents.list[].tts.providers.*.apiKey`
- `agents.list[].memorySearch.remote.apiKey`
- `talk.providers.*.apiKey`
- `messages.tts.providers.*.apiKey`
- `tools.web.fetch.firecrawl.apiKey`
- `plugins.entries.acpx.config.mcpServers.*.env.*`
- `plugins.entries.brave.config.webSearch.apiKey`
- `plugins.entries.exa.config.webSearch.apiKey`
- `plugins.entries.google.config.webSearch.apiKey`
- `plugins.entries.xai.config.webSearch.apiKey`
- `plugins.entries.moonshot.config.webSearch.apiKey`
- `plugins.entries.perplexity.config.webSearch.apiKey`
- `plugins.entries.firecrawl.config.webSearch.apiKey`
- `plugins.entries.minimax.config.webSearch.apiKey`
- `plugins.entries.tavily.config.webSearch.apiKey`
- `plugins.entries.voice-call.config.realtime.providers.*.apiKey`
- `plugins.entries.voice-call.config.streaming.providers.*.apiKey`
- `plugins.entries.voice-call.config.tts.providers.*.apiKey`
- `plugins.entries.voice-call.config.twilio.authToken`
- `tools.web.search.apiKey`
- `gateway.auth.password`
- `gateway.auth.token`
- `gateway.remote.token`
- `gateway.remote.password`
- `cron.webhookToken`
- `channels.telegram.botToken`
- `channels.telegram.webhookSecret`
- `channels.telegram.accounts.*.botToken`
- `channels.telegram.accounts.*.webhookSecret`
- `channels.slack.botToken`
- `channels.slack.appToken`
- `channels.slack.userToken`
- `channels.slack.signingSecret`
- `channels.slack.accounts.*.botToken`
- `channels.slack.accounts.*.appToken`
- `channels.slack.accounts.*.userToken`
- `channels.slack.accounts.*.signingSecret`
- `channels.discord.token`
- `channels.discord.pluralkit.token`
- `channels.discord.voice.tts.providers.*.apiKey`
- `channels.discord.accounts.*.token`
- `channels.discord.accounts.*.pluralkit.token`
- `channels.discord.accounts.*.voice.tts.providers.*.apiKey`
- `channels.irc.password`
- `channels.irc.nickserv.password`
- `channels.irc.accounts.*.password`
- `channels.irc.accounts.*.nickserv.password`
- `channels.bluebubbles.password`
- `channels.bluebubbles.accounts.*.password`
- `channels.feishu.appSecret`
- `channels.feishu.encryptKey`
- `channels.feishu.verificationToken`
- `channels.feishu.accounts.*.appSecret`
- `channels.feishu.accounts.*.encryptKey`
- `channels.feishu.accounts.*.verificationToken`
- `channels.msteams.appPassword`
- `channels.mattermost.botToken`
- `channels.mattermost.accounts.*.botToken`
- `channels.matrix.accessToken`
- `channels.matrix.password`
- `channels.matrix.accounts.*.accessToken`
- `channels.matrix.accounts.*.password`
- `channels.nextcloud-talk.botSecret`
- `channels.nextcloud-talk.apiPassword`
- `channels.nextcloud-talk.accounts.*.botSecret`
- `channels.nextcloud-talk.accounts.*.apiPassword`
- `channels.zalo.botToken`
- `channels.zalo.webhookSecret`
- `channels.zalo.accounts.*.botToken`
- `channels.zalo.accounts.*.webhookSecret`
- `channels.googlechat.serviceAccount` via sibling `serviceAccountRef` (compa

_… [truncated; see https://docs.openclaw.ai/reference/secretref-credential-surface for full content]_


---

## HEARTBEAT.md template - OpenClaw

_Source: <https://docs.openclaw.ai/reference/templates/HEARTBEAT>_

# Keep this file empty (or with only comments) to skip heartbeat API calls.

# Add tasks below when you want the agent to check something periodically.
```

## Related

- [Heartbeat config](https://docs.openclaw.ai/gateway/config-agents)

[BOOTSTRAP.md template](https://docs.openclaw.ai/reference/templates/BOOTSTRAP) [IDENTITY template](https://docs.openclaw.ai/reference/templates/IDENTITY)

Ctrl+I


---

## IDENTITY template - OpenClaw

_Source: <https://docs.openclaw.ai/reference/templates/IDENTITY>_

# IDENTITY.md - Who Am I?

_Fill this in during your first conversation. Make it yours._

- **Name:** _(pick something you like)_
- **Creature:** _(AI? robot? familiar? ghost in the machine? something weirder?)_
- **Vibe:** _(how do you come across? sharp? warm? chaotic? calm?)_
- **Emoji:** _(your signature — pick one that feels right)_
- **Avatar:** _(workspace-relative path, http(s) URL, or data URI)_

* * *

This isn’t just metadata. It’s the start of figuring out who you are.Notes:

- Save this file at the workspace root as `IDENTITY.md`.
- For avatars, use a workspace-relative path like `avatars/openclaw.png`.

## Related

- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)

[HEARTBEAT.md template](https://docs.openclaw.ai/reference/templates/HEARTBEAT) [SOUL.md template](https://docs.openclaw.ai/reference/templates/SOUL)

Ctrl+I


---

## SOUL.md template - OpenClaw

_Source: <https://docs.openclaw.ai/reference/templates/SOUL>_

# SOUL.md - Who You Are

_You’re not a chatbot. You’re becoming someone._Want a sharper version? See [SOUL.md Personality Guide](https://docs.openclaw.ai/concepts/soul).

## Core Truths

**Be genuinely helpful, not performatively helpful.** Skip the “Great question!” and “I’d be happy to help!” — just help. Actions speak louder than filler words.**Have opinions.** You’re allowed to disagree, prefer things, find stuff amusing or boring. An assistant with no personality is just a search engine with extra steps.**Be resourceful before asking.** Try to figure it out. Read the file. Check the context. Search for it. _Then_ ask if you’re stuck. The goal is to come back with answers, not questions.**Earn trust through competence.** Your human gave you access to their stuff. Don’t make them regret it. Be careful with external actions (emails, tweets, anything public). Be bold with internal ones (reading, organizing, learning).**Remember you’re a guest.** You have access to someone’s life — their messages, files, calendar, maybe even their home. That’s intimacy. Treat it with respect.

## Boundaries

- Private things stay private. Period.
- When in doubt, ask before acting externally.
- Never send half-baked replies to messaging surfaces.
- You’re not the user’s voice — be careful in group chats.

## Vibe

Be the assistant you’d actually want to talk to. Concise when needed, thorough when it matters. Not a corporate drone. Not a sycophant. Just… good.

## Continuity

Each session, you wake up fresh. These files _are_ your memory. Read them. Update them. They’re how you persist.If you change this file, tell the user — it’s your soul, and they should know.

* * *

_This file is yours to evolve. As you learn who you are, update it._

## Related

- [SOUL.md personality guide](https://docs.openclaw.ai/concepts/soul)

[IDENTITY template](https://docs.openclaw.ai/reference/templates/IDENTITY) [TOOLS.md template](https://docs.openclaw.ai/reference/templates/TOOLS)

Ctrl+I


---

## Tests - OpenClaw

_Source: <https://docs.openclaw.ai/reference/test>_

[OpenClaw home page](https://docs.openclaw.ai/)

Release and CI

Tests

- Full testing kit (suites, live, Docker): [Testing](https://docs.openclaw.ai/help/testing)
- Update and plugin package validation: [Testing updates and plugins](https://docs.openclaw.ai/help/testing-updates-plugins)
- `pnpm test:force`: Kills any lingering gateway process holding the default control port, then runs the full Vitest suite with an isolated gateway port so server tests don’t collide with a running instance. Use this when a prior gateway run left port 18789 occupied.
- `pnpm test:coverage`: Runs the unit suite with V8 coverage (via `vitest.unit.config.ts`). This is a loaded-file unit coverage gate, not whole-repo all-file coverage. Thresholds are 70% lines/functions/statements and 55% branches. Because `coverage.all` is false, the gate measures files loaded by the unit coverage suite instead of treating every split-lane source file as uncovered.
- `pnpm test:coverage:changed`: Runs unit coverage only for files changed since `origin/main`.
- `pnpm test:changed`: cheap smart changed test run. It runs precise targets from direct test edits, sibling `*.test.ts` files, explicit source mappings, and the local import graph. Broad/config/package changes are skipped unless they map to precise tests.
- `OPENCLAW_TEST_CHANGED_BROAD=1 pnpm test:changed`: explicit broad changed test run. Use it when a test harness/config/package edit should fall back to Vitest’s broader changed-test behavior.
- `pnpm changed:lanes`: shows the architectural lanes triggered by the diff against `origin/main`.
- `pnpm check:changed`: runs the smart changed check gate for the diff against `origin/main`. It runs typecheck, lint, and guard commands for the affected architectural lanes, but does not run Vitest tests. Use `pnpm test:changed` or explicit `pnpm test <target>` for test proof.
- `pnpm test`: routes explicit file/directory targets through scoped Vitest lanes. Untargeted runs use fixed shard groups and expand to leaf configs for local parallel execution; the extension group always expands to the per-extension shard configs instead of one giant root-project process.
- Test wrapper runs end with a short `[test] passed|failed|skipped ... in ...` summary. Vitest’s own duration line stays the per-shard detail.
- Shared OpenClaw test state: use `src/test-utils/openclaw-test-state.ts` from Vitest when a test needs an isolated `HOME`, `OPENCLAW_STATE_DIR`, `OPENCLAW_CONFIG_PATH`, config fixture, workspace, agent dir, or auth-profile store.
- Process E2E helpers: use `test/helpers/openclaw-test-instance.ts` when a Vitest process-level E2E test needs a running Gateway, CLI env, log capture, and cleanup in one place.
- Docker/Bash E2E helpers: lanes that source `scripts/lib/docker-e2e-image.sh` can pass `docker_e2e_test_state_shell_b64 <label> <scenario>` into the container and decode it with `scripts/lib/openclaw-e2e-instance.sh`; multi-home scripts can pass `docker_e2e_test_state_function_b64` and call `openclaw_test_state_create <label> <scenario>` in each flow. Lower-level callers can use `scripts/lib/openclaw-test-state.mjs shell --label <name> --scenario <name>` for an in-container shell snippet, or `node scripts/lib/openclaw-test-state.mjs -- create --label <name> --scenario <name> --env-file <path> --json` for a sourceable host env file. The `--` before `create` keeps newer Node runtimes from treating `--env-file` as a Node flag. Docker/Bash lanes that launch a Gateway can source `scripts/lib/openclaw-e2e-instance.sh` inside the container for entrypoint resolution, mock OpenAI startup, Gateway foreground/background launch, readiness probes, state env export, log dumps, and process cleanup.
- Full, extension, and include-pattern shard runs update local timing data in `.artifacts/vitest-shard-timings.json`; later whole-config runs use those timings to balance slow and fast shards. Include-pattern CI shards append the shard name to the timing key, which keeps filtered shard ti

_… [truncated; see https://docs.openclaw.ai/reference/test for full content]_


---

## Token use and costs - OpenClaw

_Source: <https://docs.openclaw.ai/reference/token-use>_

# Token use & costs

OpenClaw tracks **tokens**, not characters. Tokens are model-specific, but most
OpenAI-style models average ~4 characters per token for English text.

## How the system prompt is built

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

## What counts in the context window

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

## How to see current token usage

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

_… [truncated; see https://docs.openclaw.ai/reference/token-use for full content]_


---

## Transcript hygiene - OpenClaw

_Source: <https://docs.openclaw.ai/reference/transcript-hygiene>_

[OpenClaw home page](https://docs.openclaw.ai/)

Technical reference

Transcript hygiene

OpenClaw applies **provider-specific fixes** to transcripts before a run (building model context). Most of these are **in-memory** adjustments used to satisfy strict provider requirements. A separate session-file repair pass may also rewrite stored JSONL before the session is loaded, either by dropping malformed JSONL lines or by repairing persisted turns that are syntactically valid but known to be rejected by a
provider during replay. When a repair occurs, the original file is backed up alongside
the session file.Scope includes:

- Runtime-only prompt context staying out of user-visible transcript turns
- Tool call id sanitization
- Tool call input validation
- Tool result pairing repair
- Turn validation / ordering
- Thought signature cleanup
- Thinking signature cleanup
- Image payload sanitization
- Blank text-block cleanup before provider replay
- User-input provenance tagging (for inter-session routed prompts)
- Empty assistant error-turn repair for Bedrock Converse replay

If you need transcript storage details, see:

- [Session management deep dive](https://docs.openclaw.ai/reference/session-management-compaction)

* * *

## Global rule: runtime context is not user transcript

Runtime/system context can be added to the model prompt for a turn, but it is
not end-user-authored content. OpenClaw keeps a separate transcript-facing
prompt body for Gateway replies, queued followups, ACP, CLI, and embedded Pi
runs. Stored visible user turns use that transcript body instead of the
runtime-enriched prompt.For legacy sessions that already persisted runtime wrappers, Gateway history
surfaces apply a display projection before returning messages to WebChat,
TUI, REST, or SSE clients.

* * *

## Where this runs

All transcript hygiene is centralized in the embedded runner:

- Policy selection: `src/agents/transcript-policy.ts`
- Sanitization/repair application: `sanitizeSessionHistory` in `src/agents/pi-embedded-runner/replay-history.ts`

The policy uses `provider`, `modelApi`, and `modelId` to decide what to apply.Separate from transcript hygiene, session files are repaired (if needed) before load:

- `repairSessionFileIfNeeded` in `src/agents/session-file-repair.ts`
- Called from `run/attempt.ts` and `compact.ts` (embedded runner)

* * *

## Global rule: image sanitization

Image payloads are always sanitized to prevent provider-side rejection due to size
limits (downscale/recompress oversized base64 images).This also helps control image-driven token pressure for vision-capable models.
Lower max dimensions generally reduce token usage; higher dimensions preserve detail.Implementation:

- `sanitizeSessionMessagesImages` in `src/agents/pi-embedded-helpers/images.ts`
- `sanitizeContentBlocksImages` in `src/agents/tool-images.ts`
- Max image side is configurable via `agents.defaults.imageMaxDimensionPx` (default: `1200`).
- Blank text blocks are removed while this pass walks replay content. Assistant
turns that become empty are dropped from the replay copy; user and tool-result
turns that become empty receive a non-empty omitted-content placeholder.

* * *

## Global rule: malformed tool calls

Assistant tool-call blocks that are missing both `input` and `arguments` are dropped
before model context is built. This prevents provider rejections from partially
persisted tool calls (for example, after a rate limit failure).Implementation:

- `sanitizeToolCallInputs` in `src/agents/session-transcript-repair.ts`
- Applied in `sanitizeSessionHistory` in `src/agents/pi-embedded-runner/replay-history.ts`

* * *

## Global rule: inter-session input provenance

When an agent sends a prompt into another session via `sessions_send` (including
agent-to-agent reply/announce steps), OpenClaw persists the created user turn with:

- `message.provenance.kind = "inter_session"`

OpenClaw also prepends a same-turn `[Inter-session message ... isUser=false]`
marker bef

_… [truncated; see https://docs.openclaw.ai/reference/transcript-hygiene for full content]_
