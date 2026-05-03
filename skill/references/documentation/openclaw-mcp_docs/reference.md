# Openclaw-Mcp_Docs - Reference

**Pages:** 19

---

## Credits

**URL:** https://docs.openclaw.ai/reference/credits

**Contents:**
- Credits
- Documentation Index
- ​Credits and Acknowledgments
- ​The name
- ​Credits
- ​Core contributors
- ​License
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Transcript hygiene

**URL:** https://docs.openclaw.ai/reference/transcript-hygiene

**Contents:**
- Transcript hygiene
- Documentation Index
- ​Global rule: runtime context is not user transcript
- ​Where this runs
- ​Global rule: image sanitization
- ​Global rule: malformed tool calls
- ​Global rule: inter-session input provenance
- ​Provider matrix (current behavior)
- ​Historical behavior (pre-2026.1.22)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## SecretRef credential surface

**URL:** https://docs.openclaw.ai/reference/secretref-credential-surface

**Contents:**
- SecretRef credential surface
- Documentation Index
- ​Supported credentials
  - ​openclaw.json targets (secrets configure + secrets apply + secrets audit)
  - ​auth-profiles.json targets (secrets configure + secrets apply + secrets audit)
- ​Unsupported credentials
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## RPC adapters

**URL:** https://docs.openclaw.ai/reference/rpc

**Contents:**
- RPC adapters
- Documentation Index
- ​Pattern A: HTTP daemon (signal-cli)
- ​Pattern B: stdio child process (legacy: imsg)
- ​Adapter guidelines
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Memory configuration reference

**URL:** https://docs.openclaw.ai/reference/memory-config

**Contents:**
- Memory configuration reference
- Documentation Index
- Memory overview
- Builtin engine
- QMD engine
- Memory search
- Active memory
- ​Provider selection
  - ​Auto-detection order
  - ​Custom provider ids

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

OpenAI-compatible input types

Local (GGUF + node-llama-cpp)

**Examples:**

Example 1 (json):
```json
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

Example 2 (json):
```json
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
        model: "text-embedding-3-small",
        remote: {
          baseUrl: "https://api.example.com/v1/",
          apiKey: "YOUR_KEY",
        },
      },
    },
  },
}
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
        remote: {
          baseUrl: "https://embeddings.example/v1",
          apiKey: "env:EMBEDDINGS_API_KEY",
        },
        model: "asymmetric-embedder",
        queryInputType: "query",
        documentInputType: "passage",
      },
    },
  },
}
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "bedrock",
        model: "amazon.titan-embed-text-v2:0",
      },
    },
  },
}
```

---

## Prompt caching

**URL:** https://docs.openclaw.ai/reference/prompt-caching

**Contents:**
- Prompt caching
- Documentation Index
- ​Primary knobs
  - ​cacheRetention (global default, model, and per-agent)
  - ​contextPruning.mode: "cache-ttl"
  - ​Heartbeat keep-warm
- ​Provider behavior
  - ​Anthropic (direct API)
  - ​OpenAI (direct API)
  - ​Anthropic Vertex

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (yaml):
```yaml
agents:
  defaults:
    params:
      cacheRetention: "long" # none | short | long
```

Example 2 (yaml):
```yaml
agents:
  defaults:
    models:
      "anthropic/claude-opus-4-6":
        params:
          cacheRetention: "short" # none | short | long
```

Example 3 (yaml):
```yaml
agents:
  list:
    - id: "alerts"
      params:
        cacheRetention: "none"
```

Example 4 (yaml):
```yaml
agents:
  defaults:
    contextPruning:
      mode: "cache-ttl"
      ttl: "1h"
```

---

## BOOT.md template

**URL:** https://docs.openclaw.ai/reference/templates/BOOT

**Contents:**
- BOOT.md template
- Documentation Index
- ​BOOT.md
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Tests

**URL:** https://docs.openclaw.ai/reference/test

**Contents:**
- Tests
- Documentation Index
- ​Local PR gate
- ​Model latency bench (local keys)
- ​CLI startup bench
- ​Onboarding E2E (Docker)
- ​QR import smoke (Docker)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
scripts/e2e/onboard-docker.sh
```

Example 2 (unknown):
```unknown
pnpm test:docker:qr
```

---

## USER template

**URL:** https://docs.openclaw.ai/reference/templates/USER

**Contents:**
- USER template
- Documentation Index
- ​USER.md - About Your Human
- ​Context
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## BOOTSTRAP.md template

**URL:** https://docs.openclaw.ai/reference/templates/BOOTSTRAP

**Contents:**
- BOOTSTRAP.md template
- Documentation Index
- ​BOOTSTRAP.md - Hello, World
- ​The Conversation
- ​After You Know Who You Are
- ​Connect (Optional)
- ​When you are done
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Application modernization plan

**URL:** https://docs.openclaw.ai/reference/application-modernization-plan

**Contents:**
- Application modernization plan
- Documentation Index
- ​Application modernization plan
- ​Goal
- ​Principles
- ​Phase 1: Baseline audit
- ​Phase 2: Product and UX cleanup
- ​Phase 3: Frontend architecture tightening
- ​Phase 4: Performance and reliability
- ​Phase 5: Type, contract, and test hardening

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (yaml):
```yaml
# Frontend Delivery Standards

Use this skill when implementing or reviewing user-facing React, Next.js,
desktop webview, or app UI work.

## Operating rules

- Start from the existing product workflow and code conventions.
- Prefer the smallest correct patch that improves the current user path.
- Separate required fixes from optional polish in the handoff.
- Do not build marketing pages when the request is for an application surface.
- Keep actions visible and usable across supported viewport sizes.
- Remove dead affordances instead of leaving controls that cannot act.
- Preserve loading, empty, error, success, and permission states.
- Use existing design-system components, hooks, stores, and icons before adding
  new primitives.

## Implementation checklist

1. Identify the primary user task and the component or route that owns it.
2. Read the local component patterns before editing.
3. Patch the narrowest surface that solves the issue.
4. Add responsive constraints for fixed-format controls, toolbars, grids, and
   counters so text and hover states cannot resize the layout unexpectedly.
5. Keep data loading, state derivation, and rendering responsibilities clear.
6. Add tests when logic, persistence, routing, permissions, or shared helpers
   change.
7. Verify the main happy path and the most relevant edge case.

## Visual quality gates

- Text must fit inside its container on mobile and desktop.
- Toolbars may wrap, but controls must remain reachable.
- Buttons should use familiar icons when the icon is clearer than text.
- Cards should be used for repeated items, modals, and framed tools, not for
  every page section.
- Avoid one-note color palettes and decorative backgrounds that compete with
  operational content.
- Dense product surfaces should optimize for scanning, comparison, and repeated
  use.

## Handoff format

Report:

- What changed.
- What user behavior changed.
- Required validation that passed.
- Any validation skipped and the concrete reason.
- Optional follow-up work, clearly separated from required fixes.
```

---

## Auth credential semantics

**URL:** https://docs.openclaw.ai/auth-credential-semantics

**Contents:**
- Auth credential semantics
- Documentation Index
- ​Stable probe reason codes
- ​Token credentials
  - ​Eligibility rules
  - ​Resolution rules
- ​Agent copy portability
- ​Explicit auth order filtering
- ​Probe target resolution
- ​External CLI credential discovery

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## IDENTITY template

**URL:** https://docs.openclaw.ai/reference/templates/IDENTITY

**Contents:**
- IDENTITY template
- Documentation Index
- ​IDENTITY.md - Who Am I?
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## HEARTBEAT.md template

**URL:** https://docs.openclaw.ai/reference/templates/HEARTBEAT

**Contents:**
- HEARTBEAT.md template
- Documentation Index
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
# Keep this file empty (or with only comments) to skip heartbeat API calls.

# Add tasks below when you want the agent to check something periodically.
```

---

## Full release validation

**URL:** https://docs.openclaw.ai/reference/full-release-validation

**Contents:**
- Full release validation
- Documentation Index
- ​Top-level stages
- ​Release checks stages
- ​Docker release-path chunks
- ​Release profiles
- ​Full-only additions
- ​Focused reruns
- ​Evidence to keep
- ​Workflow files

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
gh workflow run full-release-validation.yml \
  --ref main \
  -f ref=release/YYYY.M.D \
  -f provider=openai \
  -f mode=both \
  -f release_profile=stable
```

---

## Rich output protocol

**URL:** https://docs.openclaw.ai/reference/rich-output-protocol

**Contents:**
- Rich output protocol
- Documentation Index
- ​[embed ...]
- ​Stored rendering shape
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
[embed ref="cv_123" title="Status" /]
```

Example 2 (json):
```json
{
  "type": "canvas",
  "preview": {
    "kind": "canvas",
    "surface": "assistant_message",
    "render": "url",
    "viewId": "cv_123",
    "url": "/__openclaw__/canvas/documents/cv_123/index.html",
    "title": "Status",
    "preferredHeight": 320
  }
}
```

---

## Release policy

**URL:** https://docs.openclaw.ai/reference/RELEASING

**Contents:**
- Release policy
- Documentation Index
- ​Version naming
- ​Release cadence
- ​Release operator checklist
- ​Release preflight
- ​Release test boxes
  - ​Vitest
  - ​Docker
  - ​QA Lab

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm ci:full-release --sha <full-sha>
```

Example 2 (sass):
```sass
gh workflow run full-release-validation.yml \
  --ref main \
  -f ref=release/YYYY.M.D \
  -f provider=openai \
  -f mode=both \
  -f release_profile=stable \
  -f evidence_package_spec=openclaw@YYYY.M.D-beta.N
```

Example 3 (sass):
```sass
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

Example 4 (sass):
```sass
gh workflow run ci.yml --ref main -f target_ref=release/YYYY.M.D
```

---

## SOUL.md template

**URL:** https://docs.openclaw.ai/reference/templates/SOUL

**Contents:**
- SOUL.md template
- Documentation Index
- ​SOUL.md - Who You Are
- ​Core Truths
- ​Boundaries
- ​Vibe
- ​Continuity
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Token use and costs

**URL:** https://docs.openclaw.ai/reference/token-use

**Contents:**
- Token use and costs
- Documentation Index
- ​Token use & costs
- ​How the system prompt is built
- ​What counts in the context window
- ​How to see current token usage
- ​Cost estimation (when shown)
- ​Cache TTL and pruning impact
  - ​Example: keep 1h cache warm with heartbeat
  - ​Example: mixed traffic with per-agent cache strategy

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
models.providers.<provider>.models[].cost
```

Example 2 (yaml):
```yaml
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

Example 3 (yaml):
```yaml
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

Example 4 (yaml):
```yaml
agents:
  defaults:
    models:
      "anthropic/claude-opus-4-6":
        params:
          context1m: true
```

---
