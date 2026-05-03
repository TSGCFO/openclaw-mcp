# Openclaw-Mcp_Docs - Concepts

**Pages:** 52

---

## Matrix QA

**URL:** https://docs.openclaw.ai/concepts/qa-matrix

**Contents:**
- Matrix QA
- Documentation Index
- ​Quick start
- ​What the lane does
- ​CLI
  - ​Common flags
  - ​Provider flags
- ​Profiles
- ​Scenarios
- ​Environment variables

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm openclaw qa matrix --profile fast --fail-fast
```

Example 2 (unknown):
```unknown
pnpm openclaw qa matrix [options]
```

---

## Compaction

**URL:** https://docs.openclaw.ai/concepts/compaction

**Contents:**
- Compaction
- Documentation Index
- ​How it works
- ​Auto-compaction
- ​Manual compaction
- ​Configuration
  - ​Using a different model
  - ​Identifier preservation
  - ​Active transcript byte guard
  - ​Successor transcripts

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Recognized overflow signatures

**Examples:**

Example 1 (unknown):
```unknown
/compact Focus on the API design decisions
```

Example 2 (json):
```json
{
  "agents": {
    "defaults": {
      "compaction": {
        "model": "openrouter/anthropic/claude-sonnet-4-6"
      }
    }
  }
}
```

Example 3 (json):
```json
{
  "agents": {
    "defaults": {
      "compaction": {
        "model": "ollama/llama3.1:8b"
      }
    }
  }
}
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      compaction: {
        notifyUser: true,
      },
    },
  },
}
```

---

## Typing indicators

**URL:** https://docs.openclaw.ai/concepts/typing-indicators

**Contents:**
- Typing indicators
- Documentation Index
- ​Defaults
- ​Modes
- ​Configuration
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agent: {
    typingMode: "thinking",
    typingIntervalSeconds: 6,
  },
}
```

Example 2 (json):
```json
{
  session: {
    typingMode: "message",
    typingIntervalSeconds: 4,
  },
}
```

---

## OpenClaw App SDK

**URL:** https://docs.openclaw.ai/concepts/openclaw-sdk

**Contents:**
- OpenClaw App SDK
- Documentation Index
- ​What Ships Today
- ​Connect To A Gateway
- ​Run An Agent
- ​Create And Reuse Sessions
- ​Stream Events
- ​Models, Tools, Artifacts, And Approvals
- ​Explicitly Unsupported Today
- ​App SDK Versus Plugin SDK

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (javascript):
```javascript
import { OpenClaw } from "@openclaw/sdk";

const oc = new OpenClaw({
  url: "ws://127.0.0.1:14565",
  token: process.env.OPENCLAW_GATEWAY_TOKEN,
  requestTimeoutMs: 30_000,
});

await oc.connect();
```

Example 2 (css):
```css
const oc = new OpenClaw({
  transport: {
    async request(method, params) {
      return { method, params };
    },
    async *events() {},
  },
});
```

Example 3 (javascript):
```javascript
const agent = await oc.agents.get("main");

const run = await agent.run({
  input: "Review this pull request and suggest the smallest safe fix.",
  model: "openai/gpt-5.5",
  sessionKey: "main",
  timeoutMs: 30_000,
});

for await (const event of run.events()) {
  const data = event.data as { delta?: unknown };
  if (event.type === "assistant.delta" && typeof data.delta === "string") {
    process.stdout.write(data.delta);
  }
}

const result = await run.wait({ timeoutMs: 120_000 });
console.log(result.status);
```

Example 4 (javascript):
```javascript
const session = await oc.sessions.create({
  agentId: "main",
  label: "release-review",
});

const run = await session.send("Prepare release notes from the current diff.");
await run.wait();
```

---

## Features

**URL:** https://docs.openclaw.ai/concepts/features

**Contents:**
- Features
- Documentation Index
- ​Highlights
- Channels
- Plugins
- Routing
- Media
- Apps and UI
- Mobile nodes
- ​Full list

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OAuth

**URL:** https://docs.openclaw.ai/concepts/oauth

**Contents:**
- OAuth
- Documentation Index
- ​The token sink (why it exists)
- ​Storage (where tokens live)
- ​Anthropic legacy token compatibility
- ​Anthropic Claude CLI migration
- ​OAuth exchange (how login works)
  - ​Anthropic setup-token
  - ​OpenAI Codex (ChatGPT OAuth)
- ​Refresh + expiry

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw models auth login --provider <id>
```

Example 2 (unknown):
```unknown
openclaw agents add work
openclaw agents add personal
```

---

## Plugin internals

**URL:** https://docs.openclaw.ai/plugins/architecture

**Contents:**
- Plugin internals
- Documentation Index
- Install and use plugins
- Building plugins
- Channel plugins
- Provider plugins
- SDK overview
- ​Public capability model
  - ​External compatibility stance
  - ​Plugin shapes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Enablement + validation

Vendor multi-capability

Vendor single-capability

Define the capability

Expose through the SDK

Vendor implementations

Core defines the contract

Vendor plugins register

Consumers use the shared behavior

Runtime registration enforcement

**Examples:**

Example 1 (typescript):
```typescript
import type { OpenClawPluginDefinition } from "openclaw/plugin-sdk/plugin-entry";
import {
  describeImageWithModel,
  transcribeOpenAiCompatibleAudio,
} from "openclaw/plugin-sdk/media-understanding";

const plugin: OpenClawPluginDefinition = {
  id: "exampleai",
  name: "ExampleAI",
  register(api) {
    api.registerProvider({
      id: "exampleai",
      // auth/model catalog/runtime hooks
    });

    api.registerSpeechProvider({
      id: "exampleai",
      // vendor speech config — implement the SpeechProviderPlugin interface directly
    });

    api.registerMediaUnderstandingProvider({
      id: "exampleai",
      capabilities: ["image", "audio", "video"],
      async describeImage(req) {
        return describeImageWithModel({
          provider: "exampleai",
          model: req.model,
          input: req.input,
        });
      },
      async transcribeAudio(req) {
        return transcribeOpenAiCompatibleAudio({
          provider: "exampleai",
          model: req.model,
          input: req.input,
        });
      },
    });

    api.registerWebSearchProvider(
      createPluginBackedWebSearchProvider({
        id: "exampleai-search",
        // credential + fetch logic
      }),
    );
  },
};

export default plugin;
```

---

## Presence

**URL:** https://docs.openclaw.ai/concepts/presence

**Contents:**
- Presence
- Documentation Index
- ​Presence fields (what shows up)
- ​Producers (where presence comes from)
  - ​1) Gateway self entry
  - ​2) WebSocket connect
    - ​Why one-off CLI commands do not show up
  - ​3) system-event beacons
  - ​4) Node connects (role: node)
- ​Merge + dedupe rules (why instanceId matters)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## TypeBox

**URL:** https://docs.openclaw.ai/concepts/typebox

**Contents:**
- TypeBox
- Documentation Index
- ​TypeBox as protocol source of truth
- ​Mental model (30 seconds)
- ​Where the schemas live
- ​Current pipeline
- ​How the schemas are used at runtime
- ​Example frames
- ​Minimal client (Node.js)
- ​Worked example: add a method end-to-end

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (r):
```r
Client                    Gateway
  |---- req:connect -------->|
  |<---- res:hello-ok --------|
  |<---- event:tick ----------|
  |---- req:health ---------->|
  |<---- res:health ----------|
```

Example 2 (json):
```json
{
  "type": "req",
  "id": "c1",
  "method": "connect",
  "params": {
    "minProtocol": 3,
    "maxProtocol": 3,
    "client": {
      "id": "openclaw-macos",
      "displayName": "macos",
      "version": "1.0.0",
      "platform": "macos 15.1",
      "mode": "ui",
      "instanceId": "A1B2"
    }
  }
}
```

Example 3 (json):
```json
{
  "type": "res",
  "id": "c1",
  "ok": true,
  "payload": {
    "type": "hello-ok",
    "protocol": 3,
    "server": { "version": "dev", "connId": "ws-1" },
    "features": { "methods": ["health"], "events": ["tick"] },
    "snapshot": {
      "presence": [],
      "health": {},
      "stateVersion": { "presence": 0, "health": 0 },
      "uptimeMs": 0
    },
    "policy": { "maxPayload": 1048576, "maxBufferedBytes": 1048576, "tickIntervalMs": 30000 }
  }
}
```

Example 4 (json):
```json
{ "type": "req", "id": "r1", "method": "health" }
```

---

## Channel docking

**URL:** https://docs.openclaw.ai/concepts/channel-docking

**Contents:**
- Channel docking
- Documentation Index
- ​Example
- ​Why use it
- ​Required config
- ​Commands
- ​What changes
- ​What does not change
- ​Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  session: {
    identityLinks: {
      alice: ["telegram:123", "discord:456"],
    },
  },
}
```

Example 2 (unknown):
```unknown
/dock_discord
```

Example 3 (json):
```json
{
  session: {
    identityLinks: {
      alice: ["telegram:123", "discord:456", "slack:U123"],
    },
  },
}
```

---

## Agent runtime

**URL:** https://docs.openclaw.ai/concepts/agent

**Contents:**
- Agent runtime
- Documentation Index
- ​Workspace (required)
- ​Bootstrap files (injected)
- ​Built-in tools
- ​Skills
- ​Runtime boundaries
- ​Sessions
- ​Steering while streaming
- ​Model refs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{ agents: { defaults: { skipBootstrap: true } } }
```

---

## Usage tracking

**URL:** https://docs.openclaw.ai/concepts/usage-tracking

**Contents:**
- Usage tracking
- Documentation Index
- ​What it is
- ​Where it shows up
- ​Providers + credentials
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Memory overview

**URL:** https://docs.openclaw.ai/concepts/memory

**Contents:**
- Memory overview
- Documentation Index
- ​How it works
- ​Inferred commitments
- ​Memory tools
- ​Memory Wiki companion plugin
- ​Memory search
- ​Memory backends
- Builtin (default)
- QMD

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "agents": {
    "defaults": {
      "compaction": {
        "memoryFlush": {
          "model": "ollama/qwen3:8b"
        }
      }
    }
  }
}
```

Example 2 (unknown):
```unknown
openclaw memory rem-backfill --path ./memory --stage-short-term
```

Example 3 (unknown):
```unknown
openclaw memory rem-backfill --rollback
openclaw memory rem-backfill --rollback-short-term
```

Example 4 (sql):
```sql
openclaw memory status          # Check index status and provider
openclaw memory search "query"  # Search from the command line
openclaw memory index --force   # Rebuild the index
```

---

## Agent workspace

**URL:** https://docs.openclaw.ai/concepts/agent-workspace

**Contents:**
- Agent workspace
- Documentation Index
- ​Default location
- ​Extra workspace folders
- ​Workspace file map
- ​What is NOT in the workspace
- ​Git backup (recommended, private)
- ​Do not commit secrets
- ​Moving the workspace to a new machine
- ​Advanced notes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

AGENTS.md — operating instructions

SOUL.md — persona and tone

USER.md — who the user is

IDENTITY.md — name, vibe, emoji

TOOLS.md — local tool conventions

HEARTBEAT.md — heartbeat checklist

BOOT.md — startup checklist

BOOTSTRAP.md — first-run ritual

memory/YYYY-MM-DD.md — daily memory log

MEMORY.md — curated long-term memory (optional)

skills/ — workspace skills (optional)

canvas/ — Canvas UI files (optional)

Copy sessions (optional)

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
    },
  },
}
```

Example 2 (json):
```json
{ agents: { defaults: { skipBootstrap: true } } }
```

Example 3 (unknown):
```unknown
cd ~/.openclaw/workspace
git init
git add AGENTS.md SOUL.md TOOLS.md IDENTITY.md USER.md HEARTBEAT.md memory/
git commit -m "Add agent workspace"
```

Example 4 (unknown):
```unknown
git branch -M main
git remote add origin <https-url>
git push -u origin main
```

---

## Session tools

**URL:** https://docs.openclaw.ai/concepts/session-tool

**Contents:**
- Session tools
- Documentation Index
- ​Available tools
- ​Listing and reading sessions
- ​Sending cross-session messages
- ​Status and orchestration helpers
- ​Spawning sub-agents
- ​Visibility
- ​Further reading
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  tools: {
    profile: "messaging",
    alsoAllow: ["sessions_spawn", "sessions_yield", "subagents"],
  },
}
```

---

## Context

**URL:** https://docs.openclaw.ai/concepts/context

**Contents:**
- Context
- Documentation Index
- ​Quick start (inspect context)
- ​Example output
  - ​/context list
  - ​/context detail
- ​What counts toward the context window
- ​How OpenClaw builds the system prompt
- ​Injected workspace files (Project Context)
- ​Skills: injected vs loaded on-demand

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
🧠 Context breakdown
Workspace: <workspaceDir>
Bootstrap max/file: 12,000 chars
Sandbox: mode=non-main sandboxed=false
System prompt (run): 38,412 chars (~9,603 tok) (Project Context 23,901 chars (~5,976 tok))

Injected workspace files:
- AGENTS.md: OK | raw 1,742 chars (~436 tok) | injected 1,742 chars (~436 tok)
- SOUL.md: OK | raw 912 chars (~228 tok) | injected 912 chars (~228 tok)
- TOOLS.md: TRUNCATED | raw 54,210 chars (~13,553 tok) | injected 20,962 chars (~5,241 tok)
- IDENTITY.md: OK | raw 211 chars (~53 tok) | injected 211 chars (~53 tok)
- USER.md: OK | raw 388 chars (~97 tok) | injected 388 chars (~97 tok)
- HEARTBEAT.md: MISSING | raw 0 | injected 0
- BOOTSTRAP.md: OK | raw 0 chars (~0 tok) | injected 0 chars (~0 tok)

Skills list (system prompt text): 2,184 chars (~546 tok) (12 skills)
Tools: read, edit, write, exec, process, browser, message, sessions_send, …
Tool list (system prompt text): 1,032 chars (~258 tok)
Tool schemas (JSON): 31,988 chars (~7,997 tok) (counts toward context; not shown as text)
Tools: (same as above)

Session tokens (cached): 14,250 total / ctx=32,000
```

Example 2 (sass):
```sass
🧠 Context breakdown (detailed)
…
Top skills (prompt entry size):
- frontend-design: 412 chars (~103 tok)
- oracle: 401 chars (~101 tok)
… (+10 more skills)

Top tools (schema size):
- browser: 9,812 chars (~2,453 tok)
- exec: 6,240 chars (~1,560 tok)
… (+N more tools)
```

---

## SOUL.md personality guide

**URL:** https://docs.openclaw.ai/concepts/soul

**Contents:**
- SOUL.md personality guide
- Documentation Index
- ​What belongs in SOUL.md
- ​Why this works
- ​The Molty prompt
- ​What good looks like
- ​One warning
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
Read your `SOUL.md`. Now rewrite it with these changes:

1. You have opinions now. Strong ones. Stop hedging everything with "it depends" - commit to a take.
2. Delete every rule that sounds corporate. If it could appear in an employee handbook, it doesn't belong here.
3. Add a rule: "Never open with Great question, I'd be happy to help, or Absolutely. Just answer."
4. Brevity is mandatory. If the answer fits in one sentence, one sentence is what I get.
5. Humor is allowed. Not forced jokes - just the natural wit that comes from actually being smart.
6. You can call things out. If I'm about to do something dumb, say so. Charm over cruelty, but don't sugarcoat.
7. Swearing is allowed when it lands. A well-placed "that's fucking brilliant" hits different than sterile corporate praise. Don't force it. Don't overdo it. But if a situation calls for a "holy shit" - say holy shit.
8. Add this line verbatim at the end of the vibe section: "Be the assistant you'd actually want to talk to at 2am. Not a corporate drone. Not a sycophant. Just... good."

Save the new `SOUL.md`. Welcome to having a personality.
```

---

## Session pruning

**URL:** https://docs.openclaw.ai/concepts/session-pruning

**Contents:**
- Session pruning
- Documentation Index
- ​Why it matters
- ​How it works
- ​Legacy image cleanup
- ​Smart defaults
- ​Enable or disable
- ​Pruning vs compaction
- ​Further reading
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      contextPruning: { mode: "cache-ttl", ttl: "5m" },
    },
  },
}
```

---

## Models CLI

**URL:** https://docs.openclaw.ai/concepts/models

**Contents:**
- Models CLI
- Documentation Index
- Model failover
- Model providers
- Agent runtimes
- Configuration reference
- ​How model selection works
- ​Selection source and fallback behavior
- ​Quick model policy
- ​Onboarding (recommended)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Provider auth failover

Related model surfaces

Clobber protection rules

Persistence and live switching

Auth and probe behavior

Merge mode precedence

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard
```

Example 2 (json):
```json
openclaw config set agents.defaults.models '{"openai/gpt-5.4":{}}' --strict-json --merge
```

Example 3 (unknown):
```unknown
Model "provider/model" is not allowed. Use /model to list available models.
```

Example 4 (json):
```json
{
  agent: {
    model: { primary: "anthropic/claude-sonnet-4-6" },
    models: {
      "anthropic/claude-sonnet-4-6": { alias: "Sonnet" },
      "anthropic/claude-opus-4-6": { alias: "Opus" },
    },
  },
}
```

---

## Markdown formatting

**URL:** https://docs.openclaw.ai/concepts/markdown-formatting

**Contents:**
- Markdown formatting
- Documentation Index
- ​Goals
- ​Pipeline
- ​IR example
- ​Where it is used
- ​Table handling
- ​Chunking rules
- ​Link policy
- ​Spoilers

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
Hello **world** — see [docs](https://docs.openclaw.ai).
```

Example 2 (json):
```json
{
  "text": "Hello world — see docs.",
  "styles": [{ "start": 6, "end": 11, "style": "bold" }],
  "links": [{ "start": 19, "end": 23, "href": "https://docs.openclaw.ai" }]
}
```

Example 3 (yaml):
```yaml
channels:
  discord:
    markdown:
      tables: code
    accounts:
      work:
        markdown:
          tables: off
```

---

## Multi-agent routing

**URL:** https://docs.openclaw.ai/concepts/multi-agent

**Contents:**
- Multi-agent routing
- Documentation Index
- ​What is “one agent”?
- ​Paths (quick map)
  - ​Single-agent mode (default)
- ​Agent helper
- ​Quick start
- ​Multiple agents = multiple people, multiple personalities
- ​Cross-agent QMD memory search
- ​One WhatsApp number, multiple people (DM split)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create each agent workspace

Create channel accounts

Add agents, accounts, and bindings

accountId match for a channel

Tie-breaking and AND semantics

Discord bots per agent

Telegram bots per agent

WhatsApp numbers per agent

**Examples:**

Example 1 (typescript):
```typescript
~/.openclaw/agents/<agentId>/agent/auth-profiles.json
```

Example 2 (unknown):
```unknown
openclaw agents add work
```

Example 3 (unknown):
```unknown
openclaw agents list --bindings
```

Example 4 (unknown):
```unknown
openclaw agents add coding
openclaw agents add social
```

---

## Plugin architecture internals

**URL:** https://docs.openclaw.ai/plugins/architecture-internals

**Contents:**
- Plugin architecture internals
- Documentation Index
- ​Load pipeline
  - ​Manifest-first behavior
  - ​Plugin cache boundary
- ​Registry model
- ​Conversation binding callbacks
- ​Provider runtime hooks
  - ​Hook order and usage
  - ​Provider example

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Pass-through catalog providers

OAuth and usage endpoint providers

Replay and transcript cleanup families

Catalog-only providers

Anthropic-specific stream helpers

**Examples:**

Example 1 (javascript):
```javascript
export default {
  id: "my-plugin",
  register(api) {
    api.onConversationBindingResolved(async (event) => {
      if (event.status === "approved") {
        // A binding now exists for this plugin + conversation.
        console.log(event.binding?.conversationId);
        return;
      }

      // The request was denied; clear any local pending state.
      console.log(event.request.conversation.conversationId);
    });
  },
};
```

Example 2 (javascript):
```javascript
api.registerProvider({
  id: "example-proxy",
  label: "Example Proxy",
  auth: [],
  catalog: {
    order: "simple",
    run: async (ctx) => {
      const apiKey = ctx.resolveProviderApiKey("example-proxy").apiKey;
      if (!apiKey) {
        return null;
      }
      return {
        provider: {
          baseUrl: "https://proxy.example.com/v1",
          apiKey,
          api: "openai-completions",
          models: [{ id: "auto", name: "Auto" }],
        },
      };
    },
  },
  resolveDynamicModel: (ctx) => ({
    id: ctx.modelId,
    name: ctx.modelId,
    provider: "example-proxy",
    api: "openai-completions",
    baseUrl: "https://proxy.example.com/v1",
    reasoning: false,
    input: ["text"],
    cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
    contextWindow: 128000,
    maxTokens: 8192,
  }),
  prepareRuntimeAuth: async (ctx) => {
    const exchanged = await exchangeToken(ctx.apiKey);
    return {
      apiKey: exchanged.token,
      baseUrl: exchanged.baseUrl,
      expiresAt: exchanged.expiresAt,
    };
  },
  resolveUsageAuth: async (ctx) => {
    const auth = await ctx.resolveOAuthToken();
    return auth ? { token: auth.token } : null;
  },
  fetchUsageSnapshot: async (ctx) => {
    return await fetchExampleProxyUsage(ctx.token, ctx.timeoutMs, ctx.fetchFn);
  },
});
```

Example 3 (javascript):
```javascript
const clip = await api.runtime.tts.textToSpeech({
  text: "Hello from OpenClaw",
  cfg: api.config,
});

const result = await api.runtime.tts.textToSpeechTelephony({
  text: "Hello from OpenClaw",
  cfg: api.config,
});

const voices = await api.runtime.tts.listVoices({
  provider: "elevenlabs",
  cfg: api.config,
});
```

Example 4 (dart):
```dart
api.registerSpeechProvider({
  id: "acme-speech",
  label: "Acme Speech",
  isConfigured: ({ config }) => Boolean(config.messages?.tts),
  synthesize: async (req) => {
    return {
      audioBuffer: Buffer.from([]),
      outputFormat: "mp3",
      fileExtension: ".mp3",
      voiceCompatible: false,
    };
  },
});
```

---

## Parallel specialist lanes

**URL:** https://docs.openclaw.ai/concepts/parallel-specialist-lanes

**Contents:**
- Parallel specialist lanes
- Documentation Index
- ​First principles
- ​Recommended rollout
  - ​Phase 1: lane contracts + background heavy work
  - ​Phase 2: priority and concurrency controls
  - ​Phase 3: coordinator / traffic controller
- ​Minimal lane contract template
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      maxConcurrent: 4,
      subagents: { maxConcurrent: 8 },
    },
  },
  messages: {
    queue: {
      mode: "collect",
      debounceMs: 1000,
      cap: 20,
      drop: "summarize",
    },
  },
}
```

Example 2 (markdown):
```markdown
# Lane contract

## Owns

- <job this lane is responsible for>

## Does not own

- <work to hand off>

## Chat budget

- Answer quick questions directly.
- For multi-step, slow, or tool-heavy work: acknowledge briefly, spawn/background
  the work, then return the result when complete.

## Handoff

If another lane owns the request, reply with:

- target lane
- objective
- relevant context
- exact next action

## Tool posture

Use the smallest tool surface that can complete the task. Avoid broad shell or
network work unless this lane explicitly owns it.
```

---

## Agent runtimes

**URL:** https://docs.openclaw.ai/concepts/agent-runtimes

**Contents:**
- Agent runtimes
- Documentation Index
- ​Codex surfaces
- ​Runtime ownership
- ​Runtime selection
- ​Compatibility contract
- ​Status labels
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
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

Example 2 (json):
```json
{
  agents: {
    defaults: {
      model: "anthropic/claude-opus-4-7",
      agentRuntime: { id: "claude-cli" },
    },
  },
}
```

---

## Context engine

**URL:** https://docs.openclaw.ai/concepts/context-engine

**Contents:**
- Context engine
- Documentation Index
- ​Quick start
- ​How it works
  - ​Subagent lifecycle (optional)
  - ​System prompt addition
- ​The legacy engine
- ​Plugin engines
  - ​The ContextEngine interface
  - ​ownsCompaction

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Check which engine is active

Install a plugin engine

Enable and select the engine

Switch back to legacy (optional)

ownsCompaction: false or unset

**Examples:**

Example 1 (markdown):
```markdown
openclaw doctor
# or inspect config directly:
cat ~/.openclaw/openclaw.json | jq '.plugins.slots.contextEngine'
```

Example 2 (elixir):
```elixir
openclaw plugins install @martian-engineering/lossless-claw
```

Example 3 (unknown):
```unknown
openclaw plugins install -l ./my-context-engine
```

Example 4 (css):
```css
// openclaw.json
{
  plugins: {
    slots: {
      contextEngine: "lossless-claw", // must match the plugin's registered engine id
    },
    entries: {
      "lossless-claw": {
        enabled: true,
        // Plugin-specific config goes here (see the plugin's docs)
      },
    },
  },
}
```

---

## Builtin memory engine

**URL:** https://docs.openclaw.ai/concepts/memory-builtin

**Contents:**
- Builtin memory engine
- Documentation Index
- ​What it provides
- ​Getting started
- ​Supported embedding providers
- ​How indexing works
- ​When to use
- ​Troubleshooting
- ​Configuration
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
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
        provider: "local",
        fallback: "none",
        local: {
          modelPath: "~/.node-llama-cpp/models/embeddinggemma-300m-qat-Q8_0.gguf",
        },
      },
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw memory status --deep --agent main
openclaw memory index --force --agent main
```

---

## Model failover

**URL:** https://docs.openclaw.ai/concepts/model-failover

**Contents:**
- Model failover
- Documentation Index
- ​Runtime flow
- ​Selection source policy
- ​Auth storage (keys + OAuth)
- ​Profile IDs
- ​Rotation order
  - ​Session stickiness (cache-friendly)
  - ​Why OAuth can “look lost”
- ​Cooldowns

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Resolve session state

Build candidate chain

Try the current provider

Advance on failover-worthy errors

Persist fallback override

Roll back narrowly on failure

Throw FallbackSummaryError if exhausted

What lands in the rate-limit / timeout bucket

Model-scoped cooldowns

Per-candidate decisions

Fallback chosen in memory

Session store still says old primary

Live reconciliation reads stale state

**Examples:**

Example 1 (json):
```json
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

Example 2 (json):
```json
{
  "usageStats": {
    "provider:profile": {
      "disabledUntil": 1736178000000,
      "disabledReason": "billing"
    }
  }
}
```

---

## Honcho memory

**URL:** https://docs.openclaw.ai/concepts/memory-honcho

**Contents:**
- Honcho memory
- Documentation Index
- ​What it provides
- ​Available tools
- ​Getting started
- ​Configuration
- ​Migrating existing memory
- ​How it works
- ​Honcho vs builtin memory
- ​CLI commands

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @honcho-ai/openclaw-honcho
openclaw honcho setup
openclaw gateway --force
```

Example 2 (json):
```json
{
  plugins: {
    entries: {
      "openclaw-honcho": {
        config: {
          apiKey: "your-api-key", // omit for self-hosted
          workspaceId: "openclaw", // memory isolation
          baseUrl: "https://api.honcho.dev",
        },
      },
    },
  },
}
```

Example 3 (typescript):
```typescript
openclaw honcho setup                        # Configure API key and migrate files
openclaw honcho status                       # Check connection status
openclaw honcho ask <question>               # Query Honcho about the user
openclaw honcho search <query> [-k N] [-d D] # Semantic search over memory
```

---

## Delegate architecture

**URL:** https://docs.openclaw.ai/concepts/delegate-architecture

**Contents:**
- Delegate architecture
- Documentation Index
- ​What is a delegate?
- ​Why delegates?
- ​Capability tiers
  - ​Tier 1: Read-Only + Draft
  - ​Tier 2: Send on Behalf
  - ​Tier 3: Proactive
- ​Prerequisites: isolation and hardening
  - ​Hard blocks (non-negotiable)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  id: "delegate",
  workspace: "~/.openclaw/workspace-delegate",
  tools: {
    allow: ["read", "exec", "message", "cron"],
    deny: ["write", "edit", "apply_patch", "browser", "canvas"],
  },
}
```

Example 2 (json):
```json
{
  id: "delegate",
  workspace: "~/.openclaw/workspace-delegate",
  sandbox: {
    mode: "all",
    scope: "agent",
  },
}
```

Example 3 (unknown):
```unknown
openclaw agents add delegate
```

Example 4 (markdown):
```markdown
# Exchange Online PowerShell
Set-Mailbox -Identity "principal@[organization].org" `
  -GrantSendOnBehalfTo "delegate@[organization].org"
```

---

## Agent loop

**URL:** https://docs.openclaw.ai/concepts/agent-loop

**Contents:**
- Agent loop
- Documentation Index
- ​Entry points
- ​How it works (high-level)
- ​Queueing + concurrency
- ​Session + workspace preparation
- ​Prompt assembly + system prompt
- ​Hook points (where you can intercept)
  - ​Internal hooks (Gateway hooks)
  - ​Plugin hooks (agent + gateway lifecycle)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## `openclaw commitments`

**URL:** https://docs.openclaw.ai/cli/commitments

**Contents:**
- `openclaw commitments`
- Documentation Index
- ​Usage
- ​Options
- ​Examples
- ​Output
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
openclaw commitments [--all] [--agent <id>] [--status <status>] [--json]
openclaw commitments list [--all] [--agent <id>] [--status <status>] [--json]
openclaw commitments dismiss <id...> [--json]
```

Example 2 (unknown):
```unknown
openclaw commitments
```

Example 3 (unknown):
```unknown
openclaw commitments --all
```

Example 4 (unknown):
```unknown
openclaw commitments --agent main
```

---

## Timezones

**URL:** https://docs.openclaw.ai/concepts/timezone

**Contents:**
- Timezones
- Documentation Index
- ​Message envelopes (local by default)
  - ​Examples
- ​Tool payloads (raw provider data + normalized fields)
- ​User timezone for the system prompt
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
[Provider ... 2026-01-05 16:26 PST] message text
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      envelopeTimezone: "local", // "utc" | "local" | "user" | IANA timezone
      envelopeTimestamp: "on", // "on" | "off"
      envelopeElapsed: "on", // "on" | "off"
    },
  },
}
```

Example 3 (sass):
```sass
[Signal Alice +1555 2026-01-18 00:19 PST] hello
```

Example 4 (sass):
```sass
[Signal Alice +1555 2026-01-18 06:19 GMT+1] hello
```

---

## Pi integration architecture

**URL:** https://docs.openclaw.ai/pi

**Contents:**
- Pi integration architecture
- Documentation Index
- ​Overview
- ​Package dependencies
- ​File structure
- ​Core integration flow
  - ​1. Running an Embedded Agent
  - ​2. Session Creation
  - ​3. Event Subscription
  - ​4. Prompting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "@mariozechner/pi-agent-core": "0.70.2",
  "@mariozechner/pi-ai": "0.70.2",
  "@mariozechner/pi-coding-agent": "0.70.2",
  "@mariozechner/pi-tui": "0.70.2"
}
```

Example 2 (lua):
```lua
src/agents/
├── pi-embedded-runner.ts          # Re-exports from pi-embedded-runner/
├── pi-embedded-runner/
│   ├── run.ts                     # Main entry: runEmbeddedPiAgent()
│   ├── run/
│   │   ├── attempt.ts             # Single attempt logic with session setup
│   │   ├── params.ts              # RunEmbeddedPiAgentParams type
│   │   ├── payloads.ts            # Build response payloads from run results
│   │   ├── images.ts              # Vision model image injection
│   │   └── types.ts               # EmbeddedRunAttemptResult
│   ├── abort.ts                   # Abort error detection
│   ├── cache-ttl.ts               # Cache TTL tracking for context pruning
│   ├── compact.ts                 # Manual/auto compaction logic
│   ├── extensions.ts              # Load pi extensions for embedded runs
│   ├── extra-params.ts            # Provider-specific stream params
│   ├── google.ts                  # Google/Gemini turn ordering fixes
│   ├── history.ts                 # History limiting (DM vs group)
│   ├── lanes.ts                   # Session/global command lanes
│   ├── logger.ts                  # Subsystem logger
│   ├── model.ts                   # Model resolution via ModelRegistry
│   ├── runs.ts                    # Active run tracking, abort, queue
│   ├── sandbox-info.ts            # Sandbox info for system prompt
│   ├── session-manager-cache.ts   # SessionManager instance caching
│   ├── session-manager-init.ts    # Session file initialization
│   ├── system-prompt.ts           # System prompt builder
│   ├── tool-split.ts              # Split tools into builtIn vs custom
│   ├── types.ts                   # EmbeddedPiAgentMeta, EmbeddedPiRunResult
│   └── utils.ts                   # ThinkLevel mapping, error description
├── pi-embedded-subscribe.ts       # Session event subscription/dispatch
├── pi-embedded-subscribe.types.ts # SubscribeEmbeddedPiSessionParams
├── pi-embedded-subscribe.handlers.ts # Event handler factory
├── pi-embedded-subscribe.handlers.lifecycle.ts
├── pi-embedded-subscribe.handlers.types.ts
├── pi-embedded-block-chunker.ts   # Streaming block reply chunking
├── pi-embedded-messaging.ts       # Messaging tool sent tracking
├── pi-embedded-helpers.ts         # Error classification, turn validation
├── pi-embedded-helpers/           # Helper modules
├── pi-embedded-utils.ts           # Formatting utilities
├── pi-tools.ts                    # createOpenClawCodingTools()
├── pi-tools.abort.ts              # AbortSignal wrapping for tools
├── pi-tools.policy.ts             # Tool allowlist/denylist policy
├── pi-tools.read.ts               # Read tool customizations
├── pi-tools.schema.ts             # Tool schema normalization
├── pi-tools.types.ts              # AnyAgentTool type alias
├── pi-tool-definition-adapter.ts  # AgentTool -> ToolDefinition adapter
├── pi-settings.ts                 # Settings overrides
├── pi-hooks/                      # Custom pi hooks
│   ├── compaction-safeguard.ts    # Safeguard extension
│   ├── compaction-safeguard-runtime.ts
│   ├── context-pruning.ts         # Cache-TTL context pruning extension
│   └── context-pruning/
├── model-auth.ts                  # Auth profile resolution
├── auth-profiles.ts               # Profile store, cooldown, failover
├── model-selection.ts             # Default model resolution
├── models-config.ts               # models.json generation
├── model-catalog.ts               # Model catalog cache
├── context-window-guard.ts        # Context window validation
├── failover-error.ts              # FailoverError class
├── defaults.ts                    # DEFAULT_PROVIDER, DEFAULT_MODEL
├── system-prompt.ts               # buildAgentSystemPrompt()
├── system-prompt-params.ts        # System prompt parameter resolution
├── system-prompt-report.ts        # Debug report generation
├── tool-summaries.ts              # Tool description summaries
├── tool-policy.ts                 # Tool policy resolution
├── transcript-policy.ts           # Transcript validation policy
├── skills.ts                      # Skill snapshot/prompt building
├── skills/                        # Skill subsystem
├── sandbox.ts                     # Sandbox context resolution
├── sandbox/                       # Sandbox subsystem
├── channel-tools.ts               # Channel-specific tool injection
├── openclaw-tools.ts              # OpenClaw-specific tools
├── bash-tools.ts                  # exec/process tools
├── apply-patch.ts                 # apply_patch tool (OpenAI)
├── tools/                         # Individual tool implementations
│   ├── browser-tool.ts
│   ├── canvas-tool.ts
│   ├── cron-tool.ts
│   ├── gateway-tool.ts
│   ├── image-tool.ts
│   ├── message-tool.ts
│   ├── nodes-tool.ts
│   ├── session*.ts
│   ├── web-*.ts
│   └── ...
└── ...
```

Example 3 (sass):
```sass
import { runEmbeddedPiAgent } from "./agents/pi-embedded-runner.js";

const result = await runEmbeddedPiAgent({
  sessionId: "user-123",
  sessionKey: "main:whatsapp:+1234567890",
  sessionFile: "/path/to/session.jsonl",
  workspaceDir: "/path/to/workspace",
  config: openclawConfig,
  prompt: "Hello, how are you?",
  provider: "anthropic",
  model: "claude-sonnet-4-6",
  timeoutMs: 120_000,
  runId: "run-abc",
  onBlockReply: async (payload) => {
    await sendToChannel(payload.text, payload.mediaUrls);
  },
});
```

Example 4 (javascript):
```javascript
import {
  createAgentSession,
  DefaultResourceLoader,
  SessionManager,
  SettingsManager,
} from "@mariozechner/pi-coding-agent";

const resourceLoader = new DefaultResourceLoader({
  cwd: resolvedWorkspace,
  agentDir,
  settingsManager,
  additionalExtensionPaths,
});
await resourceLoader.reload();

const { session } = await createAgentSession({
  cwd: resolvedWorkspace,
  agentDir,
  authStorage: params.authStorage,
  modelRegistry: params.modelRegistry,
  model: params.model,
  thinkingLevel: mapThinkingLevel(params.thinkLevel),
  tools: builtInTools,
  customTools: allCustomTools,
  sessionManager,
  settingsManager,
  resourceLoader,
});

applySystemPromptOverrideToSession(session, systemPromptOverride);
```

---

## Dreaming

**URL:** https://docs.openclaw.ai/concepts/dreaming

**Contents:**
- Dreaming
- Documentation Index
- ​What dreaming writes
- ​Phase model
- ​Session transcript ingestion
- ​Dream Diary
- ​Deep ranking signals
- ​Scheduling
- ​Quick start
- ​Slash command

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "plugins": {
    "entries": {
      "memory-core": {
        "config": {
          "dreaming": {
            "enabled": true
          }
        }
      }
    }
  }
}
```

Example 2 (json):
```json
{
  "plugins": {
    "entries": {
      "memory-core": {
        "config": {
          "dreaming": {
            "enabled": true,
            "timezone": "America/Los_Angeles",
            "frequency": "0 */6 * * *"
          }
        }
      }
    }
  }
}
```

Example 3 (unknown):
```unknown
/dreaming status
/dreaming on
/dreaming off
/dreaming help
```

Example 4 (unknown):
```unknown
openclaw memory promote
openclaw memory promote --apply
openclaw memory promote --limit 5
openclaw memory status --deep
```

---

## Gateway architecture

**URL:** https://docs.openclaw.ai/concepts/architecture

**Contents:**
- Gateway architecture
- Documentation Index
- ​Overview
- ​Components and flows
  - ​Gateway (daemon)
  - ​Clients (mac app / CLI / web admin)
  - ​Nodes (macOS / iOS / Android / headless)
  - ​WebChat
- ​Connection lifecycle (single client)
- ​Wire protocol (summary)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
ssh -N -L 18789:127.0.0.1:18789 user@host
```

---

## Command queue

**URL:** https://docs.openclaw.ai/concepts/queue

**Contents:**
- Command queue
- Documentation Index
- ​Why
- ​How it works
- ​Defaults
- ​Queue modes
- ​Queue options
- ​Precedence
- ​Per-session overrides
- ​Scope and guarantees

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  messages: {
    queue: {
      mode: "steer",
      debounceMs: 500,
      cap: 20,
      drop: "summarize",
      byChannel: { discord: "collect" },
    },
  },
}
```

---

## Steering queue

**URL:** https://docs.openclaw.ai/concepts/queue-steering

**Contents:**
- Steering queue
- Documentation Index
- ​Runtime boundary
- ​Modes
- ​Burst example
- ​Scope
- ​Debounce
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Streaming and chunking

**URL:** https://docs.openclaw.ai/concepts/streaming

**Contents:**
- Streaming and chunking
- Documentation Index
- ​Block streaming (channel messages)
  - ​Media delivery with block streaming
- ​Chunking algorithm (low/high bounds)
- ​Coalescing (merge streamed blocks)
- ​Human-like pacing between blocks
- ​”Stream chunks or everything”
- ​Preview streaming modes
  - ​Channel mapping

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
Model output
  └─ text_delta/events
       ├─ (blockStreamingBreak=text_end)
       │    └─ chunker emits blocks as buffer grows
       └─ (blockStreamingBreak=message_end)
            └─ chunker flushes at message_end
                   └─ channel send (block replies)
```

Example 2 (json):
```json
{
  "channels": {
    "telegram": {
      "streaming": {
        "mode": "partial",
        "preview": {
          "toolProgress": false
        }
      }
    }
  }
}
```

---

## Messages

**URL:** https://docs.openclaw.ai/concepts/messages

**Contents:**
- Messages
- Documentation Index
- ​Message flow (high level)
- ​Inbound dedupe
- ​Inbound debouncing
- ​Sessions and devices
- ​Tool result metadata
- ​Inbound bodies and history context
- ​Queueing and followups
- ​Channel run ownership

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (perl):
```perl
Inbound message
  -> routing/bindings -> session key
  -> queue (if a run is active)
  -> agent run (streaming + tools)
  -> outbound replies (channel limits + chunking)
```

Example 2 (json):
```json
{
  messages: {
    inbound: {
      debounceMs: 2000,
      byChannel: {
        whatsapp: 5000,
        slack: 1500,
        discord: 1500,
      },
    },
  },
}
```

---

## Session management

**URL:** https://docs.openclaw.ai/concepts/session

**Contents:**
- Session management
- Documentation Index
- ​How messages are routed
- ​DM isolation
  - ​Dock linked channels
- ​Session lifecycle
- ​Where state lives
- ​Session maintenance
- ​Inspecting sessions
- ​Further reading

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  session: {
    dmScope: "per-channel-peer", // isolate by channel + sender
  },
}
```

Example 2 (json):
```json
{
  session: {
    maintenance: {
      mode: "enforce",
      pruneAfter: "30d",
      maxEntries: 500,
    },
  },
}
```

---

## Progress drafts

**URL:** https://docs.openclaw.ai/concepts/progress-drafts

**Contents:**
- Progress drafts
- Documentation Index
- ​Quick Start
- ​What Users See
- ​Choose A Mode
- ​Configure Labels
- ​Control Progress Lines
- ​Channel Behavior
- ​Finalization
- ​Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
Shelling
- reading recent channel context
- checking matching issues
- preparing reply
```

Example 2 (json):
```json
{
  channels: {
    discord: {
      streaming: {
        mode: "progress",
      },
    },
  },
}
```

Example 3 (unknown):
```unknown
Thinking
Shelling
Scuttling
Clawing
Pinching
Molting
Bubbling
Tiding
Reefing
Cracking
Sifting
Brining
Nautiling
Krilling
Barnacling
Lobstering
Tidepooling
Pearling
Snapping
Surfacing
```

Example 4 (json):
```json
{
  channels: {
    discord: {
      streaming: {
        mode: "progress",
        progress: {
          label: "Investigating",
        },
      },
    },
  },
}
```

---

## System prompt

**URL:** https://docs.openclaw.ai/concepts/system-prompt

**Contents:**
- System prompt
- Documentation Index
- ​Structure
- ​Prompt modes
- ​Prompt snapshots
- ​Workspace bootstrap injection
- ​Time handling
- ​Skills
- ​Documentation
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
<available_skills>
  <skill>
    <name>...</name>
    <description>...</description>
    <location>...</location>
  </skill>
</available_skills>
```

---

## Experimental features

**URL:** https://docs.openclaw.ai/concepts/experimental-features

**Contents:**
- Experimental features
- Documentation Index
- ​Currently documented flags
- ​Local model lean mode
  - ​Why these three tools
  - ​When to turn it on
  - ​When to leave it off
  - ​Enable
- ​Experimental does not mean hidden
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      experimental: {
        localModelLean: true,
      },
    },
  },
}
```

Example 2 (unknown):
```unknown
openclaw status --deep
```

---

## Model providers

**URL:** https://docs.openclaw.ai/concepts/model-providers

**Contents:**
- Model providers
- Documentation Index
- ​Quick rules
- ​Plugin-owned provider behavior
- ​API key rotation
- ​Built-in providers (pi-ai catalog)
  - ​OpenAI
  - ​Anthropic
  - ​OpenAI Codex OAuth
  - ​Other subscription-style hosted options

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Model refs and CLI helpers

Adding provider auth does not change your primary model

OpenAI provider/runtime split

Key sources and priority

When rotation kicks in

Set project (if needed)

Default optional fields

Proxy-route shaping rules

**Examples:**

Example 1 (json):
```json
{
  agents: { defaults: { model: { primary: "openai/gpt-5.5" } } },
}
```

Example 2 (json):
```json
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

Example 3 (json):
```json
{
  plugins: { entries: { codex: { enabled: true } } },
  agents: {
    defaults: {
      model: { primary: "openai/gpt-5.5" },
      agentRuntime: { id: "codex" },
    },
  },
}
```

Example 4 (json):
```json
{
  models: {
    providers: {
      "openai-codex": {
        models: [{ id: "gpt-5.5", contextTokens: 160000 }],
      },
    },
  },
}
```

---

## Retry policy

**URL:** https://docs.openclaw.ai/concepts/retry

**Contents:**
- Retry policy
- Documentation Index
- ​Goals
- ​Defaults
- ​Behavior
  - ​Model providers
  - ​Discord
  - ​Telegram
- ​Configuration
- ​Notes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  channels: {
    telegram: {
      retry: {
        attempts: 3,
        minDelayMs: 400,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
    },
    discord: {
      retry: {
        attempts: 3,
        minDelayMs: 500,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
    },
  },
}
```

---

## QA overview

**URL:** https://docs.openclaw.ai/concepts/qa-e2e-automation

**Contents:**
- QA overview
- Documentation Index
- ​Command surface
- ​Operator flow
- ​Live transport coverage
- ​Telegram and Discord QA reference
  - ​Shared CLI flags
  - ​Telegram QA
  - ​Discord QA
  - ​Convex credential pool

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm qa:lab:up
```

Example 2 (unknown):
```unknown
pnpm openclaw qa docker-build-image
pnpm qa:lab:build
pnpm qa:lab:up:fast
pnpm qa:lab:watch
```

Example 3 (unknown):
```unknown
pnpm qa:otel:smoke
```

Example 4 (unknown):
```unknown
pnpm openclaw qa matrix --profile fast --fail-fast
```

---

## QMD memory engine

**URL:** https://docs.openclaw.ai/concepts/memory-qmd

**Contents:**
- QMD memory engine
- Documentation Index
- ​What it adds over builtin
- ​Getting started
  - ​Prerequisites
  - ​Enable
- ​How the sidecar works
- ​Search performance and compatibility
- ​Model overrides
- ​Indexing extra paths

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  memory: {
    backend: "qmd",
  },
}
```

Example 2 (unknown):
```unknown
qmd search "router notes" --json -n 10 -c memory-root-main -c memory-dir-main
```

Example 3 (unknown):
```unknown
qmd --help | grep -i collection
```

Example 4 (unknown):
```unknown
export QMD_EMBED_MODEL="hf:Qwen/Qwen3-Embedding-0.6B-GGUF/Qwen3-Embedding-0.6B-Q8_0.gguf"
export QMD_RERANK_MODEL="/absolute/path/to/reranker.gguf"
export QMD_GENERATE_MODEL="/absolute/path/to/generator.gguf"
```

---

## Active memory

**URL:** https://docs.openclaw.ai/concepts/active-memory

**Contents:**
- Active memory
- Documentation Index
- ​Quick start
- ​Speed recommendations
  - ​Cerebras setup
- ​How to see it
- ​Session toggle
- ​When it runs
- ​Session types
- ​Where it runs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Embedding provider switched or stopped working

Recall feels slow, empty, or inconsistent

First recall after gateway restart returns `status=timeout`

**Examples:**

Example 1 (json):
```json
{
  plugins: {
    entries: {
      "active-memory": {
        enabled: true,
        config: {
          enabled: true,
          agents: ["main"],
          allowedChatTypes: ["direct"],
          modelFallback: "google/gemini-3-flash",
          queryMode: "recent",
          promptStyle: "balanced",
          timeoutMs: 15000,
          maxSummaryChars: 220,
          persistTranscripts: false,
          logging: true,
        },
      },
    },
  },
}
```

Example 2 (unknown):
```unknown
openclaw gateway
```

Example 3 (unknown):
```unknown
/verbose on
/trace on
```

Example 4 (json):
```json
{
  models: {
    providers: {
      cerebras: {
        baseUrl: "https://api.cerebras.ai/v1",
        apiKey: "${CEREBRAS_API_KEY}",
        api: "openai-completions",
        models: [{ id: "gpt-oss-120b", name: "GPT OSS 120B (Cerebras)" }],
      },
    },
  },
  plugins: {
    entries: {
      "active-memory": {
        enabled: true,
        config: { model: "cerebras/gpt-oss-120b" },
      },
    },
  },
}
```

---

## Inferred commitments

**URL:** https://docs.openclaw.ai/concepts/commitments

**Contents:**
- Inferred commitments
- Documentation Index
- ​Enable commitments
- ​How it works
- ​Scope
- ​Commitments vs reminders
- ​Manage commitments
- ​Privacy and cost
- ​Troubleshooting
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw config set commitments.enabled true
openclaw config set commitments.maxPerDay 3
```

Example 2 (json):
```json
{
  "commitments": {
    "enabled": true,
    "maxPerDay": 3
  }
}
```

Example 3 (unknown):
```unknown
openclaw commitments
openclaw commitments --all
openclaw commitments --agent main
openclaw commitments --status snoozed
openclaw commitments dismiss cm_abc123
```

Example 4 (unknown):
```unknown
openclaw config set commitments.enabled false
```

---

## Session management deep dive

**URL:** https://docs.openclaw.ai/reference/session-management-compaction

**Contents:**
- Session management deep dive
- Documentation Index
- ​Source of truth: the Gateway
- ​Two persistence layers
- ​On-disk locations
- ​Store maintenance and disk controls
- ​Cron sessions and run logs
- ​Session keys (sessionKey)
- ​Session ids (sessionId)
- ​Session store schema (sessions.json)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw sessions cleanup --dry-run
openclaw sessions cleanup --enforce
```

Example 2 (json):
```json
{
  compaction: {
    enabled: true,
    reserveTokens: 16384,
    keepRecentTokens: 20000,
  },
}
```

---

## Memory search

**URL:** https://docs.openclaw.ai/concepts/memory-search

**Contents:**
- Memory search
- Documentation Index
- ​Quick start
- ​Supported providers
- ​How search works
- ​Improving search quality
  - ​Temporal decay
  - ​MMR (diversity)
  - ​Enable both
- ​Multimodal memory

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai", // or "gemini", "local", "ollama", etc.
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
        query: {
          hybrid: {
            mmr: { enabled: true },
            temporalDecay: { enabled: true },
          },
        },
      },
    },
  },
}
```

---

## Mantis

**URL:** https://docs.openclaw.ai/concepts/mantis

**Contents:**
- Mantis
- Documentation Index
- ​Goals
- ​Non Goals
- ​Ownership
- ​Command Shape
- ​Run Lifecycle
- ​Discord MVP
- ​Existing QA Pieces
- ​Evidence Model

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm openclaw qa mantis discord-smoke \
  --output-dir .artifacts/qa-e2e/mantis/discord-smoke
```

Example 2 (unknown):
```unknown
pnpm openclaw qa mantis run \
  --transport discord \
  --scenario discord-status-reactions-tool-only \
  --baseline origin/main \
  --candidate HEAD \
  --output-dir .artifacts/qa-e2e/mantis/local-discord-status-reactions
```

Example 3 (elixir):
```elixir
@Mantis discord status reactions
```

Example 4 (sass):
```sass
@Mantis discord status reactions baseline=origin/main candidate=HEAD
```

---
