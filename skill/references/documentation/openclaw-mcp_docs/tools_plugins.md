# Openclaw-Mcp_Docs - Tools Plugins

**Pages:** 198

---

## Gemini search

**URL:** https://docs.openclaw.ai/tools/gemini-search

**Contents:**
- Gemini search
- Documentation Index
- ​Get an API key
- ​Config
- ​How it works
- ​Supported parameters
- ​Model selection
- ​Base URL overrides
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw configure --section web
```

Example 2 (lua):
```lua
{
  plugins: {
    entries: {
      google: {
        config: {
          webSearch: {
            apiKey: "AIza...", // optional if GEMINI_API_KEY or models.providers.google.apiKey is set
            baseUrl: "https://generativelanguage.googleapis.com/v1beta", // optional; falls back to models.providers.google.baseUrl
            model: "gemini-2.5-flash", // default
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "gemini",
      },
    },
  },
}
```

---

## Music generation

**URL:** https://docs.openclaw.ai/tools/music-generation

**Contents:**
- Music generation
- Documentation Index
- ​Quick start
- ​Supported providers
  - ​Capability matrix
- ​Tool parameters
- ​Async behavior
  - ​Task lifecycle
- ​Configuration
  - ​Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Pick a default model (optional)

Configure the workflow

Cloud auth (optional)

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      musicGenerationModel: {
        primary: "google/lyria-3-clip-preview",
      },
    },
  },
}
```

Example 2 (unknown):
```unknown
/tool music_generate prompt="Warm ambient synth loop with soft tape texture"
```

Example 3 (unknown):
```unknown
Generate a cinematic piano track with soft strings and no vocals.
```

Example 4 (unknown):
```unknown
Generate an energetic chiptune loop about launching a rocket at sunrise.
```

---

## ClawHub

**URL:** https://docs.openclaw.ai/tools/clawhub

**Contents:**
- ClawHub
- Documentation Index
- ​Quick start
- ​Native OpenClaw flows
- ​What ClawHub is
- ​Workspace and skill loading
- ​Service features
- ​Security and moderation
- ​ClawHub CLI
  - ​Global options

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Auth (login / logout / whoami)

Browse / inspect plugins

Install / update / list

Delete / undelete (owner or admin)

Sync (scan local + publish new or updated)

Local changes vs registry versions

Sync scanning and fallback roots

Telemetry (install counts)

**Examples:**

Example 1 (unknown):
```unknown
openclaw skills search "calendar"
```

Example 2 (unknown):
```unknown
openclaw skills install <skill-slug>
```

Example 3 (markdown):
```markdown
npm i -g clawhub
# or
pnpm add -g clawhub
```

Example 4 (sql):
```sql
openclaw skills search "calendar"
openclaw skills install <skill-slug>
openclaw skills update --all
```

---

## Background exec and process tool

**URL:** https://docs.openclaw.ai/gateway/background-process

**Contents:**
- Background exec and process tool
- Documentation Index
- ​Background Exec + Process Tool
- ​exec tool
- ​Child process bridging
- ​process tool
- ​Examples
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{ "tool": "exec", "command": "sleep 5 && echo done", "yieldMs": 1000 }
```

Example 2 (json):
```json
{ "tool": "process", "action": "poll", "sessionId": "<id>" }
```

Example 3 (json):
```json
{ "tool": "exec", "command": "npm run build", "background": true }
```

Example 4 (json):
```json
{ "tool": "process", "action": "write", "sessionId": "<id>", "data": "y\n" }
```

---

## Zalo personal plugin

**URL:** https://docs.openclaw.ai/plugins/zalouser

**Contents:**
- Zalo personal plugin
- Documentation Index
- ​Zalo Personal (plugin)
- ​Naming
- ​Where it runs
- ​Install
  - ​Option A: install from npm
  - ​Option B: install from a local folder (dev)
- ​Config
- ​CLI

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/zalouser
```

Example 2 (bash):
```bash
PLUGIN_SRC=./path/to/local/zalouser-plugin
openclaw plugins install "$PLUGIN_SRC"
cd "$PLUGIN_SRC" && pnpm install
```

Example 3 (json):
```json
{
  channels: {
    zalouser: {
      enabled: true,
      dmPolicy: "pairing",
    },
  },
}
```

Example 4 (sql):
```sql
openclaw channels login --channel zalouser
openclaw channels logout --channel zalouser
openclaw channels status --probe
openclaw message send --channel zalouser --target <threadId> --message "Hello from OpenClaw"
openclaw directory peers list --channel zalouser --query "name"
```

---

## Skills config

**URL:** https://docs.openclaw.ai/tools/skills-config

**Contents:**
- Skills config
- Documentation Index
- ​Agent skill allowlists
- ​Fields
- ​Notes
  - ​Sandboxed skills + env vars
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  skills: {
    allowBundled: ["gemini", "peekaboo"],
    load: {
      extraDirs: ["~/Projects/agent-scripts/skills", "~/Projects/oss/some-skill-pack/skills"],
      watch: true,
      watchDebounceMs: 250,
    },
    install: {
      preferBrew: true,
      nodeManager: "npm", // npm | pnpm | yarn | bun (Gateway runtime still Node; bun not recommended)
    },
    entries: {
      "image-lab": {
        enabled: true,
        apiKey: { source: "env", provider: "default", id: "GEMINI_API_KEY" }, // or plaintext string
        env: {
          GEMINI_API_KEY: "GEMINI_KEY_HERE",
        },
      },
      peekaboo: { enabled: true },
      sag: { enabled: false },
    },
  },
}
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      skills: ["github", "weather"],
    },
    list: [
      { id: "writer" }, // inherits defaults -> github, weather
      { id: "docs", skills: ["docs-search"] }, // replaces defaults
      { id: "locked-down", skills: [] }, // no skills
    ],
  },
}
```

---

## Plugin testing

**URL:** https://docs.openclaw.ai/plugins/sdk-testing

**Contents:**
- Plugin testing
- Documentation Index
- ​Test utilities
  - ​Available exports
  - ​Types
- ​Testing target resolution
- ​Testing patterns
  - ​Testing registration contracts
  - ​Testing runtime config access
  - ​Unit testing a channel plugin

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
import {
  shouldAckReaction,
  removeAckReactionAfterReply,
} from "openclaw/plugin-sdk/channel-feedback";
import { installCommonResolveTargetErrorCases } from "openclaw/plugin-sdk/channel-target-testing";
import { AUTH_PROFILE_RUNTIME_CONTRACT } from "openclaw/plugin-sdk/agent-runtime-test-contracts";
import { createTestPluginApi } from "openclaw/plugin-sdk/plugin-test-api";
import { expectChannelInboundContextContract } from "openclaw/plugin-sdk/channel-contract-testing";
import { createStartAccountContext } from "openclaw/plugin-sdk/channel-test-helpers";
import { describePluginRegistrationContract } from "openclaw/plugin-sdk/plugin-test-contracts";
import { registerSingleProviderPlugin } from "openclaw/plugin-sdk/plugin-test-runtime";
import { describeOpenAIProviderRuntimeContract } from "openclaw/plugin-sdk/provider-test-contracts";
import { getProviderHttpMocks } from "openclaw/plugin-sdk/provider-http-test-mocks";
import { withEnv, withFetchPreconnect, withServer } from "openclaw/plugin-sdk/test-env";
import {
  bundledPluginRoot,
  createCliRuntimeCapture,
  typedCases,
} from "openclaw/plugin-sdk/test-fixtures";
import { mockNodeBuiltinModule } from "openclaw/plugin-sdk/test-node-mocks";
```

Example 2 (sql):
```sql
import type {
  ChannelAccountSnapshot,
  ChannelGatewayContext,
} from "openclaw/plugin-sdk/channel-contract";
import type { OpenClawConfig } from "openclaw/plugin-sdk/config-types";
import type { MockFn, PluginRuntime, RuntimeEnv } from "openclaw/plugin-sdk/plugin-test-runtime";
```

Example 3 (lua):
```lua
import { describe } from "vitest";
import { installCommonResolveTargetErrorCases } from "openclaw/plugin-sdk/channel-target-testing";

describe("my-channel target resolution", () => {
  installCommonResolveTargetErrorCases({
    resolveTarget: ({ to, mode, allowFrom }) => {
      // Your channel's target resolution logic
      return myChannelResolveTarget({ to, mode, allowFrom });
    },
    implicitAllowFrom: ["user1", "user2"],
  });

  // Add channel-specific test cases
  it("should resolve @username targets", () => {
    // ...
  });
});
```

Example 4 (javascript):
```javascript
import { describe, it, expect, vi } from "vitest";

describe("my-channel plugin", () => {
  it("should resolve account from config", () => {
    const cfg = {
      channels: {
        "my-channel": {
          token: "test-token",
          allowFrom: ["user1"],
        },
      },
    };

    const account = myPlugin.setup.resolveAccount(cfg, undefined);
    expect(account.token).toBe("test-token");
  });

  it("should inspect account without materializing secrets", () => {
    const cfg = {
      channels: {
        "my-channel": { token: "test-token" },
      },
    };

    const inspection = myPlugin.setup.inspectAccount(cfg, undefined);
    expect(inspection.configured).toBe(true);
    expect(inspection.tokenStatus).toBe("available");
    // No token value exposed
    expect(inspection).not.toHaveProperty("token");
  });
});
```

---

## Codex harness

**URL:** https://docs.openclaw.ai/plugins/codex-harness

**Contents:**
- Codex harness
- Documentation Index
- ​Quick config
- ​What this plugin changes
- ​Route map
- ​Pick the right model prefix
  - ​What doctor warnings mean
- ​Requirements
- ​Workspace bootstrap files
- ​Add Codex alongside other models

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw models auth login --provider openai-codex
```

Example 2 (json):
```json
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

Example 3 (json):
```json
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

Example 4 (json):
```json
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
      agentRuntime: {
        id: "auto",
      },
    },
    list: [
      {
        id: "main",
        default: true,
        model: "anthropic/claude-opus-4-6",
      },
      {
        id: "codex",
        name: "Codex",
        model: "openai/gpt-5.5",
        agentRuntime: {
          id: "codex",
        },
      },
    ],
  },
}
```

---

## Building plugins

**URL:** https://docs.openclaw.ai/plugins/building-plugins

**Contents:**
- Building plugins
- Documentation Index
- ​Prerequisites
- ​What kind of plugin?
- Channel plugin
- Provider plugin
- Tool / hook plugin
- ​Quick start: tool plugin
- ​Plugin capabilities
- ​Registering agent tools

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create the package and manifest

Write the entry point

**Examples:**

Example 1 (json):
```json
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

Example 2 (sql):
```sql
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

Example 3 (go):
```go
clawhub package publish your-org/your-plugin --dry-run
clawhub package publish your-org/your-plugin
openclaw plugins install clawhub:@myorg/openclaw-my-plugin
```

Example 4 (unknown):
```unknown
pnpm test -- <bundled-plugin-root>/my-plugin/
```

---

## Elevated mode

**URL:** https://docs.openclaw.ai/tools/elevated

**Contents:**
- Elevated mode
- Documentation Index
- ​Directives
- ​How it works
- ​Resolution order
- ​Availability and allowlists
- ​What elevated does not control
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Commands run outside the sandbox

**Examples:**

Example 1 (sass):
```sass
{
  tools: {
    elevated: {
      enabled: true,
      allowFrom: {
        discord: ["user-id-123"],
        whatsapp: ["+15555550123"],
      },
    },
  },
}
```

Example 2 (unknown):
```unknown
/elevated full
```

Example 3 (unknown):
```unknown
/elevated on run the deployment script
```

---

## Plugin SDK migration

**URL:** https://docs.openclaw.ai/plugins/sdk-migration

**Contents:**
- Plugin SDK migration
- Documentation Index
- ​What is changing
- ​Why this changed
- ​Compatibility policy
- ​How to migrate
- ​Import path reference
- ​Active deprecations
- ​Removal timeline
- ​Suppressing the warnings temporarily

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Migrate runtime config load/write helpers

Migrate Pi tool-result extensions to middleware

Migrate approval-native handlers to capability facts

Audit Windows wrapper fallback behavior

Find deprecated imports

Replace with focused imports

Replace broad infra-runtime imports

Migrate channel route helpers

Common import path table

command-auth help builders → command-status

Mention gating helpers → resolveInboundMentionDecision

Channel runtime shim and channel actions helpers

Web search provider tool() helper → createTool() on the plugin

Plaintext channel envelopes → BodyForAgent

Provider discovery types → provider catalog types

Thinking policy hooks → resolveThinkingProfile

External OAuth provider fallback → contracts.externalAuthProviders

Provider env-var lookup → setup.providers[].envVars

Memory plugin registration → registerMemoryCapability

Subagent session messages types renamed

runtime.tasks.flow → runtime.tasks.managedFlows

Embedded extension factories → agent tool-result middleware

OpenClawSchemaType alias → OpenClawConfig

**Examples:**

Example 1 (swift):
```swift
await api.runtime.config.mutateConfigFile({
  afterWrite: { mode: "auto" },
  mutate(draft) {
    draft.plugins ??= {};
  },
});
```

Example 2 (scala):
```scala
// Pi and Codex runtime dynamic tools
api.registerAgentToolResultMiddleware(async (event) => {
  return compactToolResult(event);
}, {
  runtimes: ["pi", "codex"],
});
```

Example 3 (json):
```json
{
  "contracts": {
    "agentToolResultMiddleware": ["pi", "codex"]
  }
}
```

Example 4 (sass):
```sass
// Before
const program = applyWindowsSpawnProgramPolicy({ candidate });

// After
const program = applyWindowsSpawnProgramPolicy({
  candidate,
  // Only set this for trusted compatibility callers that intentionally
  // accept shell-mediated fallback.
  allowShellFallback: true,
});
```

---

## Reactions

**URL:** https://docs.openclaw.ai/tools/reactions

**Contents:**
- Reactions
- Documentation Index
- ​How it works
- ​Channel behavior
- ​Reaction level
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Zalo Personal (zalouser)

**Examples:**

Example 1 (json):
```json
{
  "action": "react",
  "messageId": "msg-123",
  "emoji": "thumbsup"
}
```

---

## Exa search

**URL:** https://docs.openclaw.ai/tools/exa-search

**Contents:**
- Exa search
- Documentation Index
- ​Get an API key
- ​Config
- ​Base URL override
- ​Tool parameters
  - ​Content extraction
  - ​Search modes
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw configure --section web
```

Example 2 (lua):
```lua
{
  plugins: {
    entries: {
      exa: {
        config: {
          webSearch: {
            apiKey: "exa-...", // optional if EXA_API_KEY is set
            baseUrl: "https://api.exa.ai", // optional; OpenClaw appends /search
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "exa",
      },
    },
  },
}
```

Example 3 (dart):
```dart
await web_search({
  query: "transformer architecture explained",
  type: "neural",
  contents: {
    text: true, // full page text
    highlights: { numSentences: 3 }, // key sentences
    summary: true, // AI summary
  },
});
```

---

## Skills (macOS)

**URL:** https://docs.openclaw.ai/platforms/mac/skills

**Contents:**
- Skills (macOS)
- Documentation Index
- ​Data source
- ​Install actions
- ​Env/API keys
- ​Remote mode
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Grok search

**URL:** https://docs.openclaw.ai/tools/grok-search

**Contents:**
- Grok search
- Documentation Index
- ​Onboarding and configure
- ​Get an API key
- ​Config
- ​How it works
- ​Supported parameters
- ​Base URL overrides
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw configure --section web
```

Example 2 (lua):
```lua
{
  plugins: {
    entries: {
      xai: {
        config: {
          webSearch: {
            apiKey: "xai-...", // optional if XAI_API_KEY is set
            baseUrl: "https://api.x.ai/v1", // optional Responses API proxy/base URL override
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "grok",
      },
    },
  },
}
```

---

## BTW side questions

**URL:** https://docs.openclaw.ai/tools/btw

**Contents:**
- BTW side questions
- Documentation Index
- ​What it does
- ​What it does not do
- ​How context works
- ​Delivery model
- ​Surface behavior
  - ​TUI
  - ​External channels
  - ​Control UI / web

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
/btw what changed?
```

Example 2 (unknown):
```unknown
/btw what file are we editing?
/side what changed while the main run continued?
/btw what does this error mean?
/btw summarize the current task in one sentence
/btw what is 17 * 19?
```

---

## Google Meet plugin

**URL:** https://docs.openclaw.ai/plugins/google-meet

**Contents:**
- Google Meet plugin
- Documentation Index
- ​Quick start
  - ​Local gateway + Parallels Chrome
- ​Install notes
- ​Transports
  - ​Chrome
  - ​Twilio
- ​OAuth and preflight
  - ​Create Google credentials

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
brew install blackhole-2ch sox
export OPENAI_API_KEY=sk-...
# or
export GEMINI_API_KEY=...
```

Example 2 (unknown):
```unknown
sudo reboot
```

Example 3 (unknown):
```unknown
system_profiler SPAudioDataType | grep -i BlackHole
command -v sox
```

Example 4 (json):
```json
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

---

## MiniMax search

**URL:** https://docs.openclaw.ai/tools/minimax-search

**Contents:**
- MiniMax search
- Documentation Index
- ​Get a Token Plan credential
- ​Config
- ​Region selection
- ​Supported parameters
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw configure --section web
```

Example 2 (lua):
```lua
{
  plugins: {
    entries: {
      minimax: {
        config: {
          webSearch: {
            apiKey: "sk-cp-...", // optional if a MiniMax Token Plan env var is set
            region: "global", // or "cn"
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "minimax",
      },
    },
  },
}
```

---

## Plugins

**URL:** https://docs.openclaw.ai/cli/plugins

**Contents:**
- Plugins
- Documentation Index
- Plugin system
- Manage plugins
- Plugin bundles
- Plugin manifest
- Security
- ​Commands
  - ​Install
    - ​Marketplace shorthand

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Config includes and invalid-config repair

--force and reinstall vs update

--dangerously-force-unsafe-install

Hook packs and npm specs

Resolving plugin id vs npm spec

Version checks and integrity drift

--dangerously-force-unsafe-install on update

**Examples:**

Example 1 (sql):
```sql
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

Example 2 (typescript):
```typescript
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

Example 3 (elixir):
```elixir
openclaw plugins install clawhub:openclaw-codex-app-server
openclaw plugins install clawhub:openclaw-codex-app-server@1.2.3
```

Example 4 (unknown):
```unknown
openclaw plugins install openclaw-codex-app-server
```

---

## Control UI

**URL:** https://docs.openclaw.ai/web/control-ui

**Contents:**
- Control UI
- Documentation Index
- ​Quick open (local)
- ​Device pairing (first connection)
- ​Personal identity (browser-local)
- ​Runtime config endpoint
- ​Language support
- ​Appearance themes
- ​What it can do (today)
- ​Chat behavior

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

List pending requests

Approve by request ID

Channels, instances, sessions, dreams

Cron, skills, nodes, exec approvals

Cron jobs panel notes

Send and history semantics

Talk mode (browser realtime)

Abort partial retention

Insecure-auth toggle behavior

Start the UI dev server

**Examples:**

Example 1 (unknown):
```unknown
openclaw devices list
```

Example 2 (typescript):
```typescript
openclaw devices approve <requestId>
```

Example 3 (json):
```json
{
  gateway: {
    controlUi: {
      embedSandbox: "scripts",
    },
  },
}
```

Example 4 (json):
```json
{
  gateway: {
    controlUi: {
      chatMessageMaxWidth: "min(1280px, 82%)",
    },
  },
}
```

---

## Configuration — tools and custom providers

**URL:** https://docs.openclaw.ai/gateway/config-tools

**Contents:**
- Configuration — tools and custom providers
- Documentation Index
- ​Tools
  - ​Tool profiles
  - ​Tool groups
  - ​tools.allow / tools.deny
  - ​tools.byProvider
  - ​tools.elevated
  - ​tools.exec
  - ​tools.loopDetection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Media model entry fields

Auth and merge precedence

Provider connection and auth

Request transport overrides

Model catalog entries

Amazon Bedrock discovery

Cerebras (GLM 4.7 / GPT OSS)

Local models (LM Studio)

MiniMax M2.7 (direct)

Synthetic (Anthropic-compatible)

**Examples:**

Example 1 (json):
```json
{
  tools: { deny: ["browser", "canvas"] },
}
```

Example 2 (json):
```json
{
  tools: { deny: ["write", "edit", "apply_patch"] },
}
```

Example 3 (json):
```json
{
  tools: {
    profile: "coding",
    byProvider: {
      "google-antigravity": { profile: "minimal" },
      "openai/gpt-5.4": { allow: ["group:fs", "sessions_list"] },
    },
  },
}
```

Example 4 (sass):
```sass
{
  tools: {
    elevated: {
      enabled: true,
      allowFrom: {
        whatsapp: ["+15555550123"],
        discord: ["1234567890123", "987654321098765432"],
      },
    },
  },
}
```

---

## Browser troubleshooting

**URL:** https://docs.openclaw.ai/tools/browser-linux-troubleshooting

**Contents:**
- Browser troubleshooting
- Documentation Index
- ​Problem: “Failed to start Chrome CDP on port 18800”
  - ​Root cause
  - ​Solution 1: Install Google Chrome (Recommended)
  - ​Solution 2: Use Snap Chromium with Attach-Only Mode
  - ​Verifying the Browser Works
  - ​Config reference
  - ​Problem: “No Chrome tabs found for profile=“user""
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{"error":"Error: Failed to start Chrome CDP on port 18800 for profile \"openclaw\"."}
```

Example 2 (json):
```json
Note, selecting 'chromium-browser' instead of 'chromium'
chromium-browser is already the newest version (2:1snap1-0ubuntu2).
```

Example 3 (unknown):
```unknown
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt --fix-broken install -y  # if there are dependency errors
```

Example 4 (json):
```json
{
  "browser": {
    "enabled": true,
    "executablePath": "/usr/bin/google-chrome-stable",
    "headless": true,
    "noSandbox": true
  }
}
```

---

## Media overview

**URL:** https://docs.openclaw.ai/tools/media-overview

**Contents:**
- Media overview
- Documentation Index
- ​Capabilities
- Image generation
- Video generation
- Music generation
- Text-to-speech
- Media understanding
- Speech-to-text
- ​Provider capability matrix

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Plugin SDK overview

**URL:** https://docs.openclaw.ai/plugins/sdk-overview

**Contents:**
- Plugin SDK overview
- Documentation Index
- ​Import convention
- ​Subpath reference
- ​Registration API
  - ​Capability registration
  - ​Tools and commands
  - ​Infrastructure
  - ​Host hooks for workflow plugins
  - ​Gateway discovery registration

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

When to use tool-result middleware

**Examples:**

Example 1 (sql):
```sql
import { definePluginEntry } from "openclaw/plugin-sdk/plugin-entry";
import { defineChannelPluginEntry } from "openclaw/plugin-sdk/channel-core";
```

Example 2 (javascript):
```javascript
api.registerGatewayDiscoveryService({
  id: "my-discovery",
  async advertise(ctx) {
    const handle = await startMyAdvertiser({
      gatewayPort: ctx.gatewayPort,
      tls: ctx.gatewayTlsEnabled,
      displayName: ctx.machineDisplayName,
    });
    return { stop: () => handle.stop() };
  },
});
```

Example 3 (json):
```json
api.registerCli(
  async ({ program }) => {
    const { registerMatrixCli } = await import("./src/cli.js");
    registerMatrixCli({ program });
  },
  {
    descriptors: [
      {
        name: "matrix",
        description: "Manage Matrix accounts, verification, devices, and profile state",
        hasSubcommands: true,
      },
    ],
  },
);
```

Example 4 (unknown):
```unknown
my-plugin/
  api.ts            # Public exports for external consumers
  runtime-api.ts    # Internal-only runtime exports
  index.ts          # Plugin entry point
  setup-entry.ts    # Lightweight setup-only entry (optional)
```

---

## Browser login

**URL:** https://docs.openclaw.ai/tools/browser-login

**Contents:**
- Browser login
- Documentation Index
- ​Browser login + X/Twitter posting
- ​Manual login (recommended)
- ​Which Chrome profile is used?
- ​X/Twitter: recommended flow
- ​Sandboxing + host browser access
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw browser start
openclaw browser open https://x.com
```

Example 2 (json):
```json
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

Example 3 (unknown):
```unknown
openclaw browser open https://x.com --browser-profile openclaw --target host
```

---

## Plugin SDK subpaths

**URL:** https://docs.openclaw.ai/plugins/sdk-subpaths

**Contents:**
- Plugin SDK subpaths
- Documentation Index
- ​Plugin entry
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Auth and security subpaths

Runtime and storage subpaths

Capability and testing subpaths

Reserved bundled-helper subpaths

---

## Plugins

**URL:** https://docs.openclaw.ai/tools/plugin

**Contents:**
- Plugins
- Documentation Index
- ​Quick start
- ​Plugin types
- ​Package entrypoints
- ​Official plugins
  - ​OpenClaw-owned npm packages during migration
  - ​Core (shipped with OpenClaw)
- ​Configuration
- ​Discovery and precedence

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Chat-native management

Model providers (enabled by default)

Speech providers (enabled by default)

Plugin states: disabled vs missing vs invalid

**Examples:**

Example 1 (unknown):
```unknown
openclaw plugins list
```

Example 2 (sql):
```sql
# Search ClawHub plugins
openclaw plugins search "calendar"

# From ClawHub
openclaw plugins install clawhub:openclaw-codex-app-server

# From npm
openclaw plugins install npm:@acme/openclaw-plugin

# From git
openclaw plugins install git:github.com/acme/openclaw-plugin@v1.0.0

# From a local directory or archive
openclaw plugins install ./my-plugin
openclaw plugins install ./my-plugin.tgz
```

Example 3 (unknown):
```unknown
openclaw gateway restart
```

Example 4 (sql):
```sql
openclaw plugins inspect <plugin-id> --runtime --json

# If the plugin registered a CLI root, run one command from that root.
openclaw <plugin-command> --help
```

---

## apply_patch tool

**URL:** https://docs.openclaw.ai/tools/apply-patch

**Contents:**
- apply_patch tool
- Documentation Index
- ​Parameters
- ​Notes
- ​Example
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
*** Begin Patch
*** Add File: path/to/file.txt
+line 1
+line 2
*** Update File: src/app.ts
@@
-old line
+new line
*** Delete File: obsolete.txt
*** End Patch
```

Example 2 (json):
```json
{
  "tool": "apply_patch",
  "input": "*** Begin Patch\n*** Update File: src/index.ts\n@@\n-const foo = 1\n+const foo = 2\n*** End Patch"
}
```

---

## Tokenjuice

**URL:** https://docs.openclaw.ai/tools/tokenjuice

**Contents:**
- Tokenjuice
- Documentation Index
- ​Enable the plugin
- ​What tokenjuice changes
- ​Verify it is working
- ​Disable the plugin
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw config set plugins.entries.tokenjuice.enabled true
```

Example 2 (unknown):
```unknown
openclaw plugins enable tokenjuice
```

Example 3 (json):
```json
{
  plugins: {
    entries: {
      tokenjuice: {
        enabled: true,
      },
    },
  },
}
```

Example 4 (unknown):
```unknown
openclaw config set plugins.entries.tokenjuice.enabled false
```

---

## Channel turn kernel

**URL:** https://docs.openclaw.ai/plugins/sdk-channel-turn

**Contents:**
- Channel turn kernel
- Documentation Index
- ​Why a shared kernel
- ​Stage lifecycle
- ​Admission kinds
- ​Entry points
  - ​run
  - ​runPrepared
  - ​buildContext
- ​Fact types

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
runtime.channel.turn.run(...)             // adapter-driven full pipeline
runtime.channel.turn.runPrepared(...)     // channel owns dispatch; kernel runs record + finalize
runtime.channel.turn.buildContext(...)    // pure facts to FinalizedMsgContext mapping
```

Example 2 (lua):
```lua
runtime.channel.turn.runResolved(...)      // deprecated compatibility alias; prefer run
runtime.channel.turn.dispatchAssembled(...) // deprecated compatibility alias; prefer run or runPrepared
```

Example 3 (dart):
```dart
await runtime.channel.turn.run({
  channel: "tlon",
  accountId,
  raw: platformEvent,
  adapter: {
    ingest(raw) {
      return {
        id: raw.messageId,
        timestamp: raw.timestamp,
        rawText: raw.body,
        textForAgent: raw.body,
      };
    },
    classify(input) {
      return { kind: "message", canStartAgentTurn: input.rawText.length > 0 };
    },
    async preflight(input, eventClass) {
      if (await isDuplicate(input.id)) {
        return { admission: { kind: "drop", reason: "dedupe" } };
      }
      return {};
    },
    resolveTurn(input) {
      return buildAssembledTurn(input);
    },
    onFinalize(result) {
      clearPendingGroupHistory(result);
    },
  },
});
```

Example 4 (javascript):
```javascript
const { dispatchResult } = await runtime.channel.turn.runPrepared({
  channel: "matrix",
  accountId,
  routeSessionKey,
  storePath,
  ctxPayload,
  recordInboundSession,
  record: {
    onRecordError,
    updateLastRoute,
  },
  onPreDispatchFailure: async (err) => {
    await stopStatusReactions();
  },
  runDispatch: async () => {
    return await runMatrixOwnedDispatcher();
  },
});
```

---

## Browser

**URL:** https://docs.openclaw.ai/cli/browser

**Contents:**
- Browser
- Documentation Index
- ​openclaw browser
- ​Common flags
- ​Quick start (local)
- ​Quick troubleshooting
- ​Lifecycle
- ​If the command is missing
- ​Profiles
- ​Tabs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw browser profiles
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw open https://example.com
openclaw browser --browser-profile openclaw snapshot
```

Example 2 (unknown):
```unknown
openclaw browser --browser-profile openclaw doctor
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw tabs
openclaw browser --browser-profile openclaw open https://example.com
```

Example 3 (powershell):
```powershell
openclaw browser status
openclaw browser doctor
openclaw browser doctor --deep
openclaw browser start
openclaw browser start --headless
openclaw browser stop
openclaw browser --browser-profile openclaw reset-profile
```

Example 4 (json):
```json
{
  plugins: {
    allow: ["telegram", "browser"],
  },
}
```

---

## Plugin hooks

**URL:** https://docs.openclaw.ai/plugins/hooks

**Contents:**
- Plugin hooks
- Documentation Index
- ​Quick start
- ​Hook catalog
- ​Tool call policy
  - ​Tool result persistence
- ​Prompt and model hooks
  - ​Session extensions and next-turn injections
- ​Message hooks
- ​Install hooks

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
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

Example 2 (json):
```json
{
  "plugins": {
    "entries": {
      "my-plugin": {
        "hooks": {
          "timeoutMs": 30000,
          "timeouts": {
            "before_prompt_build": 90000,
            "agent_end": 60000
          }
        }
      }
    }
  }
}
```

Example 3 (typescript):
```typescript
type BeforeToolCallResult = {
  params?: Record<string, unknown>;
  block?: boolean;
  blockReason?: string;
  requireApproval?: {
    title: string;
    description: string;
    severity?: "info" | "warning" | "critical";
    timeoutMs?: number;
    timeoutBehavior?: "allow" | "deny";
    pluginId?: string;
    onResolution?: (
      decision: "allow-once" | "allow-always" | "deny" | "timeout" | "cancelled",
    ) => Promise<void> | void;
  };
};
```

Example 4 (json):
```json
{
  "plugins": {
    "entries": {
      "my-plugin": {
        "hooks": {
          "allowConversationAccess": true
        }
      }
    }
  }
}
```

---

## Memory wiki

**URL:** https://docs.openclaw.ai/plugins/memory-wiki

**Contents:**
- Memory wiki
- Documentation Index
- ​What it adds
- ​How it fits with memory
- ​Recommended hybrid pattern
- ​Vault modes
  - ​isolated
  - ​bridge
  - ​unsafe-local
- ​Vault layout

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
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

Example 2 (elixir):
```elixir
pageType: entity
entityType: person
id: entity.brad-groux
canonicalId: maintainer.brad-groux
aliases:
  - Brad
  - bgroux
privacyTier: local-private
bestUsedFor:
  - Microsoft Teams and Azure routing
notEnoughFor:
  - legal approval
lastRefreshedAt: "2026-04-29T00:00:00.000Z"
personCard:
  handles:
    - "@bgroux"
  socials:
    - "https://x.example/bgroux"
  emails:
    - brad@example.com
  timezone: America/Chicago
  lane: Microsoft ecosystem
  askFor:
    - Teams rollout questions
  avoidAskingFor:
    - unrelated billing decisions
  confidence: 0.8
  privacyTier: confirm-before-use
relationships:
  - targetId: entity.alice
    targetTitle: Alice
    kind: collaborates-with
    confidence: 0.7
    evidenceKind: discrawl-stat
claims:
  - id: claim.brad.teams
    text: Brad is useful for Microsoft Teams routing.
    status: supported
    confidence: 0.9
    evidence:
      - kind: maintainer-whois
        sourceId: source.maintainers
        privacyTier: local-private
```

Example 3 (json):
```json
{
  plugins: {
    entries: {
      "memory-wiki": {
        enabled: true,
        config: {
          vaultMode: "isolated",
          vault: {
            path: "~/.openclaw/wiki/main",
            renderMode: "obsidian",
          },
          obsidian: {
            enabled: true,
            useOfficialCli: true,
            vaultName: "OpenClaw Wiki",
            openAfterWrites: false,
          },
          bridge: {
            enabled: false,
            readMemoryArtifacts: true,
            indexDreamReports: true,
            indexDailyNotes: true,
            indexMemoryRoot: true,
            followMemoryEvents: true,
          },
          ingest: {
            autoCompile: true,
            maxConcurrentJobs: 1,
            allowUrlIngest: true,
          },
          search: {
            backend: "shared",
            corpus: "wiki",
          },
          context: {
            includeCompiledDigestPrompt: false,
          },
          render: {
            preserveHumanBlocks: true,
            createBacklinks: true,
            createDashboards: true,
          },
        },
      },
    },
  },
}
```

Example 4 (json):
```json
{
  memory: {
    backend: "qmd",
      "memory-wiki": {
        enabled: true,
        config: {
          vaultMode: "bridge",
          bridge: {
            enabled: true,
            readMemoryArtifacts: true,
            indexDreamReports: true,
            indexDailyNotes: true,
            indexMemoryRoot: true,
            followMemoryEvents: true,
          },
          search: {
            backend: "shared",
            corpus: "all",
          },
          context: {
            includeCompiledDigestPrompt: false,
          },
        },
      },
    },
  },
}
```

---

## Codex Computer Use

**URL:** https://docs.openclaw.ai/plugins/codex-computer-use

**Contents:**
- Codex Computer Use
- Documentation Index
- ​OpenClaw.app and Peekaboo
- ​iOS app
- ​Direct cua-driver MCP
- ​Quick setup
- ​Commands
- ​Marketplace choices
- ​Bundled macOS marketplace
- ​Remote catalog limit

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
cua-driver mcp-config --client openclaw
```

Example 2 (json):
```json
openclaw mcp set cua-driver '{"command":"cua-driver","args":["mcp"]}'
```

Example 3 (json):
```json
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

Example 4 (elixir):
```elixir
/codex computer-use status
/codex computer-use install
/codex computer-use install --source <marketplace-source>
/codex computer-use install --marketplace-path <path>
/codex computer-use install --marketplace <name>
```

---

## SearXNG search

**URL:** https://docs.openclaw.ai/tools/searxng-search

**Contents:**
- SearXNG search
- Documentation Index
- ​Setup
- ​Config
- ​Environment variable
- ​Plugin config reference
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Run a SearXNG instance

**Examples:**

Example 1 (json):
```json
docker run -d -p 8888:8080 searxng/searxng
```

Example 2 (sql):
```sql
openclaw configure --section web
# Select "searxng" as the provider
```

Example 3 (json):
```json
export SEARXNG_BASE_URL="http://localhost:8888"
```

Example 4 (json):
```json
{
  tools: {
    web: {
      search: {
        provider: "searxng",
      },
    },
  },
}
```

---

## Plugin reference

**URL:** https://docs.openclaw.ai/plugins/reference

**Contents:**
- Plugin reference
- Documentation Index
- ​Plugin reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm plugins:inventory:gen
```

---

## LLM task

**URL:** https://docs.openclaw.ai/tools/llm-task

**Contents:**
- LLM task
- Documentation Index
- ​Enable the plugin
- ​Config (optional)
- ​Tool parameters
- ​Output
- ​Example: Lobster workflow step
- ​Safety notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "plugins": {
    "entries": {
      "llm-task": { "enabled": true }
    }
  }
}
```

Example 2 (json):
```json
{
  "agents": {
    "list": [
      {
        "id": "main",
        "tools": { "allow": ["llm-task"] }
      }
    ]
  }
}
```

Example 3 (json):
```json
{
  "plugins": {
    "entries": {
      "llm-task": {
        "enabled": true,
        "config": {
          "defaultProvider": "openai-codex",
          "defaultModel": "gpt-5.5",
          "defaultAuthProfileId": "main",
          "allowedModels": ["openai/gpt-5.4"],
          "maxTokens": 800,
          "timeoutMs": 30000
        }
      }
    }
  }
}
```

Example 4 (json):
```json
openclaw.invoke --tool llm-task --action json --args-json '{
  "prompt": "Given the input email, return intent and draft.",
  "thinking": "low",
  "input": {
    "subject": "Hello",
    "body": "Can you help?"
  },
  "schema": {
    "type": "object",
    "properties": {
      "intent": { "type": "string" },
      "draft": { "type": "string" }
    },
    "required": ["intent", "draft"],
    "additionalProperties": false
  }
}'
```

---

## Lobster

**URL:** https://docs.openclaw.ai/tools/lobster

**Contents:**
- Lobster
- Documentation Index
- ​Hook
- ​Why
- ​Why a DSL instead of plain programs?
- ​How it works
- ​Pattern: small CLI + JSON pipes + approvals
- ​JSON-only LLM steps (llm-task)
- ​Workflow files (.lobster)
- ​Install Lobster

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
inbox list --json
inbox categorize --json
inbox apply --json
```

Example 2 (json):
```json
{
  "action": "run",
  "pipeline": "exec --json --shell 'inbox list --json' | exec --stdin json --shell 'inbox categorize --json' | exec --stdin json --shell 'inbox apply --json' | approve --preview-from-stdin --limit 5 --prompt 'Apply changes?'",
  "timeoutMs": 30000
}
```

Example 3 (json):
```json
{
  "action": "resume",
  "token": "<resumeToken>",
  "approve": true
}
```

Example 4 (lua):
```lua
gog.gmail.search --query 'newer_than:1d' \
  | openclaw.invoke --tool message --action send --each --item-key message --args-json '{"provider":"telegram","to":"..."}'
```

---

## Exec tool

**URL:** https://docs.openclaw.ai/tools/exec

**Contents:**
- Exec tool
- Documentation Index
- ​Parameters
- ​Config
  - ​PATH handling
- ​Session overrides (/exec)
- ​Authorization model
- ​Exec approvals (companion app / node host)
- ​Allowlist + safe bins
- ​Examples

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  tools: {
    exec: {
      pathPrepend: ["~/bin", "/opt/oss/bin"],
    },
  },
}
```

Example 2 (unknown):
```unknown
openclaw config get agents.list
openclaw config set agents.list[0].tools.exec.node "node-id-or-name"
```

Example 3 (sass):
```sass
/exec host=auto security=allowlist ask=on-miss node=mac-1
```

Example 4 (json):
```json
{ "tool": "exec", "command": "ls -la" }
```

---

## Web fetch

**URL:** https://docs.openclaw.ai/tools/web-fetch

**Contents:**
- Web fetch
- Documentation Index
- ​Quick start
- ​Tool parameters
- ​How it works
- ​Config
- ​Firecrawl fallback
- ​Trusted Env Proxy
- ​Limits and safety
- ​Tool profiles

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (dart):
```dart
await web_fetch({ url: "https://example.com/article" });
```

Example 2 (julia):
```julia
{
  tools: {
    web: {
      fetch: {
        enabled: true, // default: true
        provider: "firecrawl", // optional; omit for auto-detect
        maxChars: 50000, // max output chars
        maxCharsCap: 50000, // hard cap for maxChars param
        maxResponseBytes: 2000000, // max download size before truncation
        timeoutSeconds: 30,
        cacheTtlMinutes: 15,
        maxRedirects: 3,
        useTrustedEnvProxy: false, // let a trusted HTTP(S) env proxy resolve DNS
        readability: true, // use Readability extraction
        userAgent: "Mozilla/5.0 ...", // override User-Agent
        ssrfPolicy: {
          allowRfc2544BenchmarkRange: true, // opt-in for trusted fake-IP proxies using 198.18.0.0/15
          allowIpv6UniqueLocalRange: true, // opt-in for trusted fake-IP proxies using fc00::/7
        },
      },
    },
  },
}
```

Example 3 (lua):
```lua
{
  tools: {
    web: {
      fetch: {
        provider: "firecrawl", // optional; omit for auto-detect from available credentials
      },
    },
  },
  plugins: {
    entries: {
      firecrawl: {
        enabled: true,
        config: {
          webFetch: {
            apiKey: "fc-...", // optional if FIRECRAWL_API_KEY is set
            baseUrl: "https://api.firecrawl.dev",
            onlyMainContent: true,
            maxAgeMs: 86400000, // cache duration (1 day)
            timeoutSeconds: 60,
          },
        },
      },
    },
  },
}
```

Example 4 (json):
```json
{
  tools: {
    allow: ["web_fetch"],
    // or: allow: ["group:web"]  (includes web_fetch, web_search, and x_search)
  },
}
```

---

## Testing: updates and plugins

**URL:** https://docs.openclaw.ai/help/testing-updates-plugins

**Contents:**
- Testing: updates and plugins
- Documentation Index
- ​What we protect
- ​Local proof during development
- ​Docker lanes
- ​Package Acceptance
- ​Release default
- ​Legacy compatibility
- ​Adding coverage
- ​Failure triage

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm changed:lanes --json
pnpm check:changed
pnpm test:changed
```

Example 2 (unknown):
```unknown
pnpm test src/plugins/uninstall.test.ts src/infra/package-dist-inventory.test.ts test/scripts/package-acceptance-workflow.test.ts
```

Example 3 (unknown):
```unknown
pnpm release:check
```

Example 4 (sql):
```sql
pnpm test:docker:plugins
pnpm test:docker:plugin-lifecycle-matrix
pnpm test:docker:plugin-update
pnpm test:docker:upgrade-survivor
pnpm test:docker:published-upgrade-survivor
pnpm test:docker:update-migration
```

---

## Voice call plugin

**URL:** https://docs.openclaw.ai/plugins/voice-call

**Contents:**
- Voice call plugin
- Documentation Index
- ​Quick start
- ​Configuration
- ​Session scope
- ​Realtime voice conversations
  - ​Tool policy
  - ​Realtime provider examples
- ​Streaming transcription
  - ​Streaming provider examples

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Configure provider and webhook

Provider exposure and security notes

Streaming connection caps

Legacy config migrations

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/voice-call
```

Example 2 (bash):
```bash
PLUGIN_SRC=./path/to/local/voice-call-plugin
openclaw plugins install "$PLUGIN_SRC"
cd "$PLUGIN_SRC" && pnpm install
```

Example 3 (unknown):
```unknown
openclaw voicecall setup
```

Example 4 (sass):
```sass
openclaw voicecall smoke
openclaw voicecall smoke --to "+15555550123"
```

---

## Kimi search

**URL:** https://docs.openclaw.ai/tools/kimi-search

**Contents:**
- Kimi search
- Documentation Index
- ​Get an API key
- ​Config
- ​How it works
- ​Supported parameters
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw configure --section web
```

Example 2 (lua):
```lua
{
  plugins: {
    entries: {
      moonshot: {
        config: {
          webSearch: {
            apiKey: "sk-...", // optional if KIMI_API_KEY or MOONSHOT_API_KEY is set
            baseUrl: "https://api.moonshot.ai/v1",
            model: "kimi-k2.6",
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "kimi",
      },
    },
  },
}
```

---

## Slash commands

**URL:** https://docs.openclaw.ai/tools/slash-commands

**Contents:**
- Slash commands
- Documentation Index
- ​Config
- ​Command list
  - ​Core built-in commands
  - ​Generated dock commands
  - ​Bundled plugin commands
  - ​Dynamic skill commands
- ​/tools
- ​Usage surfaces (what shows where)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Model and run controls

Skills, allowlists, approvals

Owner-only writes and admin

Voice, TTS, channel control

Argument and parser notes

Channel-specific behavior

Verbose / trace / fast / reasoning safety

Fast path and inline shortcuts

Skill commands and native arguments

**Examples:**

Example 1 (json):
```json
{
  commands: {
    native: "auto",
    nativeSkills: "auto",
    text: true,
    bash: false,
    bashForegroundMs: 2000,
    config: false,
    mcp: false,
    plugins: false,
    debug: false,
    restart: true,
    ownerAllowFrom: ["discord:123456789012345678"],
    ownerDisplay: "raw",
    ownerDisplaySecret: "${OWNER_ID_HASH_SECRET}",
    allowFrom: {
      "*": ["user1"],
      discord: ["user:123"],
    },
    useAccessGroups: true,
  },
}
```

Example 2 (elixir):
```elixir
/model
/model list
/model 3
/model openai/gpt-5.4
/model opus@anthropic:default
/model status
```

Example 3 (sass):
```sass
/debug show
/debug set messages.responsePrefix="[openclaw]"
/debug set channels.whatsapp.allowFrom=["+1555","+4477"]
/debug unset messages.responsePrefix
/debug reset
```

Example 4 (unknown):
```unknown
/trace
/trace on
/trace off
```

---

## Diffs

**URL:** https://docs.openclaw.ai/tools/diffs

**Contents:**
- Diffs
- Documentation Index
- ​Quick start
- ​Disable built-in system guidance
- ​Typical agent workflow
- ​Input examples
- ​Tool input reference
- ​Output details contract
- ​Collapsed unchanged sections
- ​Plugin defaults

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Validation and limits

Compatibility aliases

File rendering hardening

Environment variables

Input validation errors

Unmodified-lines row has no expand button

**Examples:**

Example 1 (unknown):
```unknown
openclaw plugins install diffs
```

Example 2 (json):
```json
{
  plugins: {
    entries: {
      diffs: {
        enabled: true,
      },
    },
  },
}
```

Example 3 (json):
```json
{
  plugins: {
    entries: {
      diffs: {
        enabled: true,
        hooks: {
          allowPromptInjection: false,
        },
      },
    },
  },
}
```

Example 4 (json):
```json
{
  "before": "# Hello\n\nOne",
  "after": "# Hello\n\nTwo",
  "path": "docs/example.md",
  "mode": "view"
}
```

---

## Ollama web search

**URL:** https://docs.openclaw.ai/tools/ollama-search

**Contents:**
- Ollama web search
- Documentation Index
- ​Setup
- ​Config
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Choose Ollama Web Search

**Examples:**

Example 1 (unknown):
```unknown
ollama signin
```

Example 2 (unknown):
```unknown
openclaw configure --section web
```

Example 3 (json):
```json
{
  tools: {
    web: {
      search: {
        provider: "ollama",
      },
    },
  },
}
```

Example 4 (json):
```json
{
  plugins: {
    entries: {
      ollama: {
        config: {
          webSearch: {
            baseUrl: "http://ollama-host:11434",
          },
        },
      },
    },
  },
}
```

---

## Brave search

**URL:** https://docs.openclaw.ai/tools/brave-search

**Contents:**
- Brave search
- Documentation Index
- ​Brave Search API
- ​Get an API key
- ​Config example
- ​Tool parameters
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
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

Example 2 (dart):
```dart
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

---

## Trajectory bundles

**URL:** https://docs.openclaw.ai/tools/trajectory

**Contents:**
- Trajectory bundles
- Documentation Index
- ​Quick start
- ​Access
- ​What gets recorded
- ​Bundle files
- ​Capture location
- ​Disable capture
- ​Privacy and limits
- ​Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
/export-trajectory
```

Example 2 (unknown):
```unknown
/trajectory
```

Example 3 (typescript):
```typescript
.openclaw/trajectory-exports/openclaw-trajectory-<session>-<timestamp>/
```

Example 4 (unknown):
```unknown
/export-trajectory bug-1234
```

---

## Video generation

**URL:** https://docs.openclaw.ai/tools/video-generation

**Contents:**
- Video generation
- Documentation Index
- ​Quick start
- ​How async generation works
  - ​Task lifecycle
- ​Supported providers
  - ​Capability matrix
- ​Tool parameters
  - ​Required
  - ​Content inputs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Pick a default model (optional)

BytePlus Seedance 1.5

BytePlus Seedance 2.0

Google (Gemini / Veo)

**Examples:**

Example 1 (unknown):
```unknown
export GEMINI_API_KEY="your-key"
```

Example 2 (unknown):
```unknown
openclaw config set agents.defaults.videoGenerationModel.primary "google/veo-3.1-fast-generate-preview"
```

Example 3 (typescript):
```typescript
openclaw tasks list
openclaw tasks show <taskId>
openclaw tasks cancel <taskId>
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "google/veo-3.1-fast-generate-preview",
        fallbacks: ["runway/gen4.5", "qwen/wan2.6-t2v"],
      },
    },
  },
}
```

---

## Code execution

**URL:** https://docs.openclaw.ai/tools/code-execution

**Contents:**
- Code execution
- Documentation Index
- ​Setup
- ​How to use it
- ​Limits
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
{
  plugins: {
    entries: {
      xai: {
        config: {
          webSearch: {
            apiKey: "xai-...",
          },
          codeExecution: {
            enabled: true,
            model: "grok-4-1-fast",
            maxTurns: 2,
            timeoutSeconds: 30,
          },
        },
      },
    },
  },
}
```

Example 2 (lua):
```lua
Use code_execution to calculate the 7-day moving average for these numbers: ...
```

Example 3 (elixir):
```elixir
Use x_search to find posts mentioning OpenClaw this week, then use code_execution to count them by day.
```

Example 4 (elixir):
```elixir
Use web_search to gather the latest AI benchmark numbers, then use code_execution to compare percent changes.
```

---

## Skills

**URL:** https://docs.openclaw.ai/tools/skills

**Contents:**
- Skills
- Documentation Index
- ​Locations and precedence
- ​Per-agent vs shared skills
- ​Agent skill allowlists
- ​Plugins and skills
- ​Skill Workshop
- ​ClawHub (install and sync)
- ​Security
- ​SKILL.md format

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Installer selection rules

Per-installer details

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      skills: ["github", "weather"],
    },
    list: [
      { id: "writer" }, // inherits github, weather
      { id: "docs", skills: ["docs-search"] }, // replaces defaults
      { id: "locked-down", skills: [] }, // no skills
    ],
  },
}
```

Example 2 (yaml):
```yaml
---
name: image-lab
description: Generate or edit images via a provider-backed image workflow
---
```

Example 3 (json):
```json
---
name: image-lab
description: Generate or edit images via a provider-backed image workflow
metadata:
  {
    "openclaw":
      {
        "requires": { "bins": ["uv"], "env": ["GEMINI_API_KEY"], "config": ["browser.enabled"] },
        "primaryEnv": "GEMINI_API_KEY",
      },
  }
---
```

Example 4 (json):
```json
---
name: gemini
description: Use Gemini CLI for coding assistance and Google search lookups.
metadata:
  {
    "openclaw":
      {
        "emoji": "♊️",
        "requires": { "bins": ["gemini"] },
        "install":
          [
            {
              "id": "brew",
              "kind": "brew",
              "formula": "gemini-cli",
              "bins": ["gemini"],
              "label": "Install Gemini CLI (brew)",
            },
          ],
      },
  }
---
```

---

## Text-to-speech

**URL:** https://docs.openclaw.ai/tools/tts

**Contents:**
- Text-to-speech
- Documentation Index
- ​Quick start
- ​Supported providers
- ​Configuration
  - ​Per-agent voice overrides
- ​Personas
  - ​Minimal persona
  - ​Full persona (provider-neutral prompt)
  - ​Persona resolution

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Top-level messages.tts.*

Local CLI (tts-local-cli)

Microsoft (no API key)

Volcengine (BytePlus Seed Speech)

**Examples:**

Example 1 (json):
```json
{
  messages: {
    tts: {
      auto: "always",
      provider: "elevenlabs",
    },
  },
}
```

Example 2 (json):
```json
{
  messages: {
    tts: {
      auto: "always",
      provider: "azure-speech",
      providers: {
        "azure-speech": {
          apiKey: "${AZURE_SPEECH_KEY}",
          region: "eastus",
          voice: "en-US-JennyNeural",
          lang: "en-US",
          outputFormat: "audio-24khz-48kbitrate-mono-mp3",
          voiceNoteOutputFormat: "ogg-24khz-16bit-mono-opus",
        },
      },
    },
  },
}
```

Example 3 (json):
```json
{
  messages: {
    tts: {
      auto: "always",
      provider: "elevenlabs",
      providers: {
        elevenlabs: {
          apiKey: "${ELEVENLABS_API_KEY}",
          model: "eleven_multilingual_v2",
          voiceId: "EXAVITQu4vr4xnSDxMaL",
        },
      },
    },
  },
}
```

Example 4 (json):
```json
{
  messages: {
    tts: {
      auto: "always",
      provider: "google",
      providers: {
        google: {
          apiKey: "${GEMINI_API_KEY}",
          model: "gemini-3.1-flash-tts-preview",
          voiceName: "Kore",
          // Optional natural-language style prompts:
          // audioProfile: "Speak in a calm, podcast-host tone.",
          // speakerName: "Alex",
        },
      },
    },
  },
}
```

---

## Plugin bundles

**URL:** https://docs.openclaw.ai/plugins/bundles

**Contents:**
- Plugin bundles
- Documentation Index
- ​Why bundles exist
- ​Install a bundle
- ​What OpenClaw maps from bundles
  - ​Supported now
    - ​Skill content
    - ​Hook packs
    - ​MCP for Pi
      - Transports

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Install from a directory, archive, or marketplace

Bundle is detected but capabilities do not run

Claude command files do not appear

Claude settings do not apply

Claude hooks do not execute

**Examples:**

Example 1 (markdown):
```markdown
# Local directory
openclaw plugins install ./my-bundle

# Archive
openclaw plugins install ./my-bundle.tgz

# Claude marketplace
openclaw plugins marketplace list <marketplace-name>
openclaw plugins install <plugin-name>@<marketplace-name>
```

Example 2 (typescript):
```typescript
openclaw plugins list
openclaw plugins inspect <id>
```

Example 3 (unknown):
```unknown
openclaw gateway restart
```

Example 4 (json):
```json
{
  "mcp": {
    "servers": {
      "my-server": {
        "command": "node",
        "args": ["server.js"],
        "env": { "PORT": "3000" }
      }
    }
  }
}
```

---

## Thinking levels

**URL:** https://docs.openclaw.ai/tools/thinking

**Contents:**
- Thinking levels
- Documentation Index
- ​What it does
- ​Resolution order
- ​Setting a session default
- ​Application by agent
- ​Fast mode (/fast)
- ​Verbose directives (/verbose or /v)
- ​Plugin trace directives (/trace)
- ​Reasoning visibility (/reasoning)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Memory LanceDB

**URL:** https://docs.openclaw.ai/plugins/memory-lancedb

**Contents:**
- Memory LanceDB
- Documentation Index
- ​Quick start
- ​Provider-backed embeddings
- ​Ollama embeddings
- ​OpenAI-compatible providers
- ​Recall and capture limits
- ​Commands
- ​Storage
- ​Runtime dependencies

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
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

Example 2 (unknown):
```unknown
openclaw gateway restart
```

Example 3 (unknown):
```unknown
openclaw plugins list
```

Example 4 (json):
```json
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

---

## Plugin compatibility

**URL:** https://docs.openclaw.ai/plugins/compatibility

**Contents:**
- Plugin compatibility
- Documentation Index
- ​Compatibility registry
- ​Plugin inspector package
  - ​Maintainer acceptance lane
- ​Deprecation policy
- ​Current compatibility areas
- ​Release notes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw-plugin-inspector ./my-plugin
```

Example 2 (elixir):
```elixir
blacksmith testbox warmup ci-check-testbox.yml --ref main --idle-timeout 90
blacksmith testbox run --id <tbx_id> "pnpm install && pnpm build && npm exec --yes @openclaw/plugin-inspector@0.1.0 -- ./extensions/telegram --json"
blacksmith testbox run --id <tbx_id> "npm exec --yes @openclaw/plugin-inspector@0.1.0 -- ./extensions/discord --json"
blacksmith testbox run --id <tbx_id> "npm exec --yes @openclaw/plugin-inspector@0.1.0 -- <clawhub-plugin-dir> --json"
blacksmith testbox stop <tbx_id>
```

---

## Plugin setup and config

**URL:** https://docs.openclaw.ai/plugins/sdk-setup

**Contents:**
- Plugin setup and config
- Documentation Index
- ​Package metadata
  - ​openclaw fields
  - ​openclaw.channel
  - ​openclaw.install
  - ​Deferred full load
- ​Plugin manifest
- ​ClawHub publishing
- ​Setup entry

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

minHostVersion enforcement

allowInvalidConfigRecovery scope

When OpenClaw uses setupEntry instead of the full entry

What setupEntry must register

What setupEntry should NOT include

Shared allowFrom prompts

Standard channel setup status

Optional channel setup surface

Binary-backed setup helpers

**Examples:**

Example 1 (json):
```json
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

Example 2 (json):
```json
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

Example 3 (json):
```json
{
  "openclaw": {
    "channel": {
      "id": "my-channel",
      "label": "My Channel",
      "selectionLabel": "My Channel (self-hosted)",
      "detailLabel": "My Channel Bot",
      "docsPath": "/channels/my-channel",
      "docsLabel": "my-channel",
      "blurb": "Webhook-based self-hosted chat integration.",
      "order": 80,
      "aliases": ["mc"],
      "preferOver": ["my-channel-legacy"],
      "selectionDocsPrefix": "Guide:",
      "selectionExtras": ["Markdown"],
      "markdownCapable": true,
      "exposure": {
        "configured": true,
        "setup": true,
        "docs": true
      },
      "quickstartAllowFrom": true
    }
  }
}
```

Example 4 (json):
```json
{
  "openclaw": {
    "install": {
      "npmSpec": "@wecom/wecom-openclaw-plugin@1.2.3",
      "expectedIntegrity": "sha512-REPLACE_WITH_NPM_DIST_INTEGRITY",
      "defaultChoice": "npm"
    }
  }
}
```

---

## Skill workshop plugin

**URL:** https://docs.openclaw.ai/plugins/skill-workshop

**Contents:**
- Skill workshop plugin
- Documentation Index
- ​Default state
- ​Enable
- ​Configuration
- ​Capture paths
  - ​Tool suggestions
  - ​Heuristic capture
  - ​LLM reviewer
- ​Proposal lifecycle

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Force a safe write (apply: true)

Force pending under auto policy (apply: false)

Append to a named section

**Examples:**

Example 1 (typescript):
```typescript
<workspace>/skills/<skill-name>/SKILL.md
```

Example 2 (json):
```json
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

Example 3 (json):
```json
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

Example 4 (json):
```json
// Conservative: explicit tool use only, no automatic capture.
{
  autoCapture: false,
  approvalPolicy: "pending",
  reviewMode: "off",
}
```

---

## Perplexity search

**URL:** https://docs.openclaw.ai/tools/perplexity-search

**Contents:**
- Perplexity search
- Documentation Index
- ​Perplexity Search API
- ​Getting a Perplexity API key
- ​OpenRouter compatibility
- ​Config examples
  - ​Native Perplexity Search API
  - ​OpenRouter / Sonar compatibility
- ​Where to set the key
- ​Tool parameters

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
{
  plugins: {
    entries: {
      perplexity: {
        config: {
          webSearch: {
            apiKey: "pplx-...",
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "perplexity",
      },
    },
  },
}
```

Example 2 (json):
```json
{
  plugins: {
    entries: {
      perplexity: {
        config: {
          webSearch: {
            apiKey: "<openrouter-api-key>",
            baseUrl: "https://openrouter.ai/api/v1",
            model: "perplexity/sonar-pro",
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "perplexity",
      },
    },
  },
}
```

Example 3 (dart):
```dart
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

// Domain filtering (allowlist)
await web_search({
  query: "climate research",
  domain_filter: ["nature.com", "science.org", ".edu"],
});

// Domain filtering (denylist - prefix with -)
await web_search({
  query: "product reviews",
  domain_filter: ["-reddit.com", "-pinterest.com"],
});

// More content extraction
await web_search({
  query: "detailed AI research",
  max_tokens: 50000,
  max_tokens_per_page: 4096,
});
```

---

## Skills

**URL:** https://docs.openclaw.ai/cli/skills

**Contents:**
- Skills
- Documentation Index
- ​openclaw skills
- ​Commands
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
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

---

## Browser (OpenClaw-managed)

**URL:** https://docs.openclaw.ai/tools/browser

**Contents:**
- Browser (OpenClaw-managed)
- Documentation Index
- ​What you get
- ​Quick start
- ​Plugin control
- ​Agent guidance
- ​Missing browser command or tool
- ​Profiles: openclaw vs user
- ​Configuration
- ​Use Brave or another Chromium-based browser

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Ports and reachability

Existing-session feature limitations

**Examples:**

Example 1 (unknown):
```unknown
openclaw browser --browser-profile openclaw doctor
openclaw browser --browser-profile openclaw doctor --deep
openclaw browser --browser-profile openclaw status
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw open https://example.com
openclaw browser --browser-profile openclaw snapshot
```

Example 2 (json):
```json
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

Example 3 (json):
```json
{
  tools: {
    profile: "coding",
    alsoAllow: ["browser"],
  },
}
```

Example 4 (json):
```json
{
  plugins: {
    allow: ["telegram", "browser"],
  },
}
```

---

## Building provider plugins

**URL:** https://docs.openclaw.ai/plugins/sdk-provider-plugins

**Contents:**
- Building provider plugins
- Documentation Index
- ​Walkthrough
  - ​Step 1: Package and manifest
  - ​Step 5: Add extra capabilities
  - ​Step 6: Test
- ​Publish to ClawHub
- ​File structure
- ​Catalog order reference
- ​Next steps

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Register the provider

Add dynamic model resolution

Add runtime hooks (as needed)

SDK seams powering the family builders

All available provider hooks

Add extra capabilities (optional)

**Examples:**

Example 1 (json):
```json
{
  "name": "@myorg/openclaw-acme-ai",
  "version": "1.0.0",
  "type": "module",
  "openclaw": {
    "extensions": ["./index.ts"],
    "providers": ["acme-ai"],
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

Example 2 (javascript):
```javascript
import { definePluginEntry } from "openclaw/plugin-sdk/plugin-entry";
import { createProviderApiKeyAuthMethod } from "openclaw/plugin-sdk/provider-auth";

export default definePluginEntry({
  id: "acme-ai",
  name: "Acme AI",
  description: "Acme AI model provider",
  register(api) {
    api.registerProvider({
      id: "acme-ai",
      label: "Acme AI",
      docsPath: "/providers/acme-ai",
      envVars: ["ACME_AI_API_KEY"],

      auth: [
        createProviderApiKeyAuthMethod({
          providerId: "acme-ai",
          methodId: "api-key",
          label: "Acme AI API key",
          hint: "API key from your Acme AI dashboard",
          optionKey: "acmeAiApiKey",
          flagName: "--acme-ai-api-key",
          envVar: "ACME_AI_API_KEY",
          promptMessage: "Enter your Acme AI API key",
          defaultModel: "acme-ai/acme-large",
        }),
      ],

      catalog: {
        order: "simple",
        run: async (ctx) => {
          const apiKey =
            ctx.resolveProviderApiKey("acme-ai").apiKey;
          if (!apiKey) return null;
          return {
            provider: {
              baseUrl: "https://api.acme-ai.com/v1",
              apiKey,
              api: "openai-completions",
              models: [
                {
                  id: "acme-large",
                  name: "Acme Large",
                  reasoning: true,
                  input: ["text", "image"],
                  cost: { input: 3, output: 15, cacheRead: 0.3, cacheWrite: 3.75 },
                  contextWindow: 200000,
                  maxTokens: 32768,
                },
                {
                  id: "acme-small",
                  name: "Acme Small",
                  reasoning: false,
                  input: ["text"],
                  cost: { input: 1, output: 5, cacheRead: 0.1, cacheWrite: 1.25 },
                  contextWindow: 128000,
                  maxTokens: 8192,
                },
              ],
            },
          };
        },
      },
    });
  },
});
```

Example 3 (json):
```json
api.registerTextTransforms({
  input: [
    { from: /red basket/g, to: "blue basket" },
    { from: /paper ticket/g, to: "digital ticket" },
    { from: /left shelf/g, to: "right shelf" },
  ],
  output: [
    { from: /blue basket/g, to: "red basket" },
    { from: /digital ticket/g, to: "paper ticket" },
    { from: /right shelf/g, to: "left shelf" },
  ],
});
```

Example 4 (json):
```json
import { defineSingleProviderPluginEntry } from "openclaw/plugin-sdk/provider-entry";

export default defineSingleProviderPluginEntry({
  id: "acme-ai",
  name: "Acme AI",
  description: "Acme AI model provider",
  provider: {
    label: "Acme AI",
    docsPath: "/providers/acme-ai",
    auth: [
      {
        methodId: "api-key",
        label: "Acme AI API key",
        hint: "API key from your Acme AI dashboard",
        optionKey: "acmeAiApiKey",
        flagName: "--acme-ai-api-key",
        envVar: "ACME_AI_API_KEY",
        promptMessage: "Enter your Acme AI API key",
        defaultModel: "acme-ai/acme-large",
      },
    ],
    catalog: {
      buildProvider: () => ({
        api: "openai-completions",
        baseUrl: "https://api.acme-ai.com/v1",
        models: [{ id: "acme-large", name: "Acme Large" }],
      }),
      buildStaticProvider: () => ({
        api: "openai-completions",
        baseUrl: "https://api.acme-ai.com/v1",
        models: [{ id: "acme-large", name: "Acme Large" }],
      }),
    },
  },
});
```

---

## Community plugins

**URL:** https://docs.openclaw.ai/plugins/community

**Contents:**
- Community plugins
- Documentation Index
- ​Listed plugins
  - ​Apify
  - ​Codex App Server Bridge
  - ​DingTalk
  - ​Lossless Claw (LCM)
  - ​Opik
  - ​Prometheus Avatar
  - ​QQbot

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Publish to ClawHub or npm

Use docs PRs only for source-doc changes

**Examples:**

Example 1 (unknown):
```unknown
openclaw plugins install clawhub:<package-name>
```

Example 2 (elixir):
```elixir
openclaw plugins install @apify/apify-openclaw-plugin
```

Example 3 (unknown):
```unknown
openclaw plugins install openclaw-codex-app-server
```

Example 4 (elixir):
```elixir
openclaw plugins install @largezhou/ddingtalk
```

---

## Webhooks plugin

**URL:** https://docs.openclaw.ai/plugins/webhooks

**Contents:**
- Webhooks plugin
- Documentation Index
- ​Webhooks (plugin)
- ​Where it runs
- ​Configure routes
- ​Security model
- ​Request format
- ​Supported actions
  - ​create_flow
  - ​run_task

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
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

Example 2 (json):
```json
curl -X POST https://gateway.example.com/plugins/webhooks/zapier \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_SHARED_SECRET' \
  -d '{"action":"create_flow","goal":"Review inbound queue"}'
```

Example 3 (json):
```json
{
  "action": "create_flow",
  "goal": "Review inbound queue",
  "status": "queued",
  "notifyPolicy": "done_only"
}
```

Example 4 (json):
```json
{
  "action": "run_task",
  "flowId": "flow_123",
  "runtime": "acp",
  "childSessionKey": "agent:main:acp:worker",
  "task": "Inspect the next message batch"
}
```

---

## Exec approvals — advanced

**URL:** https://docs.openclaw.ai/tools/exec-approvals-advanced

**Contents:**
- Exec approvals — advanced
- Documentation Index
- ​Safe bins (stdin-only)
  - ​Argv validation and denied flags
  - ​Trusted binary directories
  - ​Shell chaining, wrappers, and multiplexers
  - ​Safe bins versus allowlist
- ​Interpreter/runtime commands
  - ​Followup delivery behavior
- ​Approval forwarding to chat channels

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  tools: {
    exec: {
      safeBins: ["jq", "myfilter"],
      safeBinProfiles: {
        myfilter: {
          minPositional: 0,
          maxPositional: 0,
          allowedValueFlags: ["-n", "--limit"],
          deniedFlags: ["-f", "--file", "-c", "--command"],
        },
      },
    },
  },
}
```

Example 2 (json):
```json
{
  approvals: {
    exec: {
      enabled: true,
      mode: "session", // "session" | "targets" | "both"
      agentFilter: ["main"],
      sessionFilter: ["discord"], // substring or regex
      targets: [
        { channel: "slack", to: "U12345678" },
        { channel: "telegram", to: "123456789" },
      ],
    },
  },
}
```

Example 3 (typescript):
```typescript
/approve <id> allow-once
/approve <id> allow-always
/approve <id> deny
```

Example 4 (json):
```json
{
  approvals: {
    plugin: {
      enabled: true,
      mode: "targets",
      agentFilter: ["main"],
      targets: [
        { channel: "slack", to: "U12345678" },
        { channel: "telegram", to: "123456789" },
      ],
    },
  },
}
```

---

## Tool-loop detection

**URL:** https://docs.openclaw.ai/tools/loop-detection

**Contents:**
- Tool-loop detection
- Documentation Index
- ​Why this exists
- ​Configuration block
  - ​Field behavior
- ​Recommended setup
- ​Logs and expected behavior
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  tools: {
    loopDetection: {
      enabled: false,
      historySize: 30,
      warningThreshold: 10,
      criticalThreshold: 20,
      globalCircuitBreakerThreshold: 30,
      detectors: {
        genericRepeat: true,
        knownPollNoProgress: true,
        pingPong: true,
      },
    },
  },
}
```

Example 2 (json):
```json
{
  agents: {
    list: [
      {
        id: "safe-runner",
        tools: {
          loopDetection: {
            enabled: true,
            warningThreshold: 8,
            criticalThreshold: 16,
          },
        },
      },
    ],
  },
}
```

---

## Firecrawl

**URL:** https://docs.openclaw.ai/tools/firecrawl

**Contents:**
- Firecrawl
- Documentation Index
- ​Get an API key
- ​Configure Firecrawl search
- ​Configure Firecrawl scrape + web_fetch fallback
  - ​Self-hosted Firecrawl
- ​Firecrawl plugin tools
  - ​firecrawl_search
  - ​firecrawl_scrape
- ​Stealth / bot circumvention

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  tools: {
    web: {
      search: {
        provider: "firecrawl",
      },
    },
  },
  plugins: {
    entries: {
      firecrawl: {
        enabled: true,
        config: {
          webSearch: {
            apiKey: "FIRECRAWL_API_KEY_HERE",
            baseUrl: "https://api.firecrawl.dev",
          },
        },
      },
    },
  },
}
```

Example 2 (json):
```json
{
  plugins: {
    entries: {
      firecrawl: {
        enabled: true,
        config: {
          webFetch: {
            apiKey: "FIRECRAWL_API_KEY_HERE",
            baseUrl: "https://api.firecrawl.dev",
            onlyMainContent: true,
            maxAgeMs: 172800000,
            timeoutSeconds: 60,
          },
        },
      },
    },
  },
}
```

---

## Plugin inventory

**URL:** https://docs.openclaw.ai/plugins/plugin-inventory

**Contents:**
- Plugin inventory
- Documentation Index
- ​Plugin inventory
- ​Definitions
- ​Core npm package
- ​Official external packages
- ​Source checkout only

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm plugins:inventory:gen
```

---

## xAI

**URL:** https://docs.openclaw.ai/providers/xai

**Contents:**
- xAI
- Documentation Index
- ​Getting started
- ​Built-in catalog
- ​OpenClaw feature coverage
  - ​Fast-mode mappings
  - ​Legacy compatibility aliases
- ​Features
- ​Live testing
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Streaming speech-to-text

x_search configuration

Code execution configuration

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice xai-api-key
```

Example 2 (json):
```json
{
  agents: { defaults: { model: { primary: "xai/grok-4.3" } } },
}
```

Example 3 (unknown):
```unknown
openclaw config set tools.web.search.provider grok
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "xai/grok-imagine-video",
      },
    },
  },
}
```

---

## Plugin manifest

**URL:** https://docs.openclaw.ai/plugins/manifest

**Contents:**
- Plugin manifest
- Documentation Index
- ​What this file does
- ​Minimal example
- ​Rich example
- ​Top-level field reference
- ​Generation provider metadata reference
- ​Tool metadata reference
- ​providerAuthChoices reference
- ​commandAliases reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "id": "voice-call",
  "configSchema": {
    "type": "object",
    "additionalProperties": false,
    "properties": {}
  }
}
```

Example 2 (json):
```json
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
  "providerEndpoints": [
    {
      "endpointClass": "openrouter",
      "hostSuffixes": ["openrouter.ai"]
    }
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
  "providerAuthChoices": [
    {
      "provider": "openrouter",
      "method": "api-key",
      "choiceId": "openrouter-api-key",
      "choiceLabel": "OpenRouter API key",
      "groupId": "openrouter",
      "groupLabel": "OpenRouter",
      "optionKey": "openrouterApiKey",
      "cliFlag": "--openrouter-api-key",
      "cliOption": "--openrouter-api-key <key>",
      "cliDescription": "OpenRouter API key",
      "onboardingScopes": ["text-inference"]
    }
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
      "apiKey": {
        "type": "string"
      }
    }
  }
}
```

Example 3 (json):
```json
{
  "contracts": {
    "imageGenerationProviders": ["example-image"]
  },
  "imageGenerationProviderMetadata": {
    "example-image": {
      "aliases": ["example-image-oauth"],
      "authProviders": ["example-image"],
      "configSignals": [
        {
          "rootPath": "plugins.entries.example-image.config",
          "overlayPath": "image",
          "mode": {
            "path": "mode",
            "default": "local",
            "allowed": ["local"]
          },
          "requiredAny": ["workflow", "workflowPath"],
          "required": ["promptNodeId"]
        }
      ],
      "authSignals": [
        {
          "provider": "example-image"
        },
        {
          "provider": "example-image-oauth",
          "providerBaseUrl": {
            "provider": "example-image",
            "defaultBaseUrl": "https://api.example.com/v1",
            "allowedBaseUrls": ["https://api.example.com/v1"]
          }
        }
      ]
    }
  }
}
```

Example 4 (json):
```json
{
  "providerAuthEnvVars": {
    "example": ["EXAMPLE_API_KEY"]
  },
  "contracts": {
    "tools": ["example_search"]
  },
  "toolMetadata": {
    "example_search": {
      "authSignals": [
        {
          "provider": "example"
        }
      ],
      "configSignals": [
        {
          "rootPath": "plugins.entries.example.config",
          "overlayPath": "search",
          "required": ["apiKey"]
        }
      ]
    }
  }
}
```

---

## Image generation

**URL:** https://docs.openclaw.ai/tools/image-generation

**Contents:**
- Image generation
- Documentation Index
- ​Quick start
- ​Common routes
- ​Supported providers
- ​Provider capabilities
- ​Tool parameters
- ​Configuration
  - ​Model selection
  - ​Provider selection order

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Pick a default model (optional)

Per-call model overrides are exact

Auto-detection is auth-aware

OpenAI gpt-image-2 (and gpt-image-1.5)

OpenRouter image models

xAI grok-imagine-image

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "openai/gpt-image-2",
        timeoutMs: 180_000,
      },
    },
  },
}
```

Example 2 (sass):
```sass
/tool image_generate action=list
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "openai/gpt-image-2",
        timeoutMs: 180_000,
        fallbacks: [
          "openrouter/google/gemini-3.1-flash-image-preview",
          "google/gemini-3.1-flash-image-preview",
          "fal/fal-ai/flux/dev",
        ],
      },
    },
  },
}
```

Example 4 (json):
```json
"Generate a watercolor version of this photo" + image: "/path/to/photo.jpg"
```

---

## Plugin entry points

**URL:** https://docs.openclaw.ai/plugins/sdk-entrypoints

**Contents:**
- Plugin entry points
- Documentation Index
- ​definePluginEntry
- ​defineChannelPluginEntry
- ​defineSetupPluginEntry
- ​Registration mode
- ​Plugin shapes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "openclaw": {
    "extensions": ["./src/index.ts"],
    "runtimeExtensions": ["./dist/index.js"],
    "setupEntry": "./src/setup-entry.ts",
    "runtimeSetupEntry": "./dist/setup-entry.js"
  }
}
```

Example 2 (lua):
```lua
import { definePluginEntry } from "openclaw/plugin-sdk/plugin-entry";

export default definePluginEntry({
  id: "my-plugin",
  name: "My Plugin",
  description: "Short summary",
  register(api) {
    api.registerProvider({
      /* ... */
    });
    api.registerTool({
      /* ... */
    });
  },
});
```

Example 3 (lua):
```lua
import { defineChannelPluginEntry } from "openclaw/plugin-sdk/channel-core";

export default defineChannelPluginEntry({
  id: "my-channel",
  name: "My Channel",
  description: "Short summary",
  plugin: myChannelPlugin,
  setRuntime: setMyRuntime,
  registerCliMetadata(api) {
    api.registerCli(/* ... */);
  },
  registerFull(api) {
    api.registerGatewayMethod(/* ... */);
  },
});
```

Example 4 (sql):
```sql
import { defineSetupPluginEntry } from "openclaw/plugin-sdk/channel-core";

export default defineSetupPluginEntry(myChannelPlugin);
```

---

## ACP

**URL:** https://docs.openclaw.ai/cli/acp

**Contents:**
- ACP
- Documentation Index
- ​What this is not
- ​Compatibility Matrix
- ​Known Limitations
- ​Usage
- ​ACP client (debug)
- ​How to use this
- ​Selecting agents
- ​Use from acpx (Codex, Claude, other ACP clients)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (swift):
```swift
openclaw acp

# Remote Gateway
openclaw acp --url wss://gateway-host:18789 --token <token>

# Remote Gateway (token from file)
openclaw acp --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token

# Attach to an existing session key
openclaw acp --session agent:main:main

# Attach by label (must already exist)
openclaw acp --session-label "support inbox"

# Reset the session key before the first prompt
openclaw acp --session agent:main:main --reset-session
```

Example 2 (markdown):
```markdown
openclaw acp client

# Point the spawned bridge at a remote Gateway
openclaw acp client --server-args --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token

# Override the server command (default: openclaw)
openclaw acp client --server "node" --server-args openclaw.mjs acp --url ws://127.0.0.1:19001
```

Example 3 (typescript):
```typescript
openclaw config set gateway.remote.url wss://gateway-host:18789
openclaw config set gateway.remote.token <token>
```

Example 4 (markdown):
```markdown
openclaw acp --url wss://gateway-host:18789 --token <token>
# preferred for local process safety
openclaw acp --url wss://gateway-host:18789 --token-file ~/.openclaw/gateway.token
```

---

## PDF tool

**URL:** https://docs.openclaw.ai/tools/pdf

**Contents:**
- PDF tool
- Documentation Index
- ​Availability
- ​Input reference
- ​Supported PDF references
- ​Execution modes
  - ​Native provider mode
  - ​Extraction fallback mode
- ​Config
- ​Output details

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      pdfModel: {
        primary: "anthropic/claude-opus-4-6",
        fallbacks: ["openai/gpt-5.4-mini"],
      },
      pdfMaxBytesMb: 10,
      pdfMaxPages: 20,
    },
  },
}
```

Example 2 (json):
```json
{
  "pdf": "/tmp/report.pdf",
  "prompt": "Summarize this report in 5 bullets"
}
```

Example 3 (json):
```json
{
  "pdfs": ["/tmp/q1.pdf", "/tmp/q2.pdf"],
  "prompt": "Compare risks and timeline changes across both documents"
}
```

Example 4 (json):
```json
{
  "pdf": "https://example.com/report.pdf",
  "pages": "1-3,7",
  "model": "openai/gpt-5.4-mini",
  "prompt": "Extract only customer-impacting incidents"
}
```

---

## Web search

**URL:** https://docs.openclaw.ai/tools/web

**Contents:**
- Web search
- Documentation Index
- ​Quick start
- ​Choosing a provider
- Brave Search
- DuckDuckGo
- Exa
- Firecrawl
- Gemini
- Grok

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw configure --section web
```

Example 2 (dart):
```dart
await web_search({ query: "OpenClaw plugin SDK" });
```

Example 3 (dart):
```dart
await x_search({ query: "dinner recipes" });
```

Example 4 (json):
```json
{
  tools: {
    web: {
      search: {
        enabled: true,
        openaiCodex: {
          enabled: true,
          mode: "cached",
          allowedDomains: ["example.com"],
          contextSize: "high",
          userLocation: {
            country: "US",
            city: "New York",
            timezone: "America/New_York",
          },
        },
      },
    },
  },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/pt-BR

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Começar
- Executar onboarding
- Abrir a Control UI
- ​O que é o OpenClaw?
- ​Como funciona
- ​Principais recursos
- Gateway multicanal

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway para qualquer sistema operacional para agentes de IA em Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo e mais. Envie uma mensagem e receba a resposta de um agente no seu bolso. Execute um Gateway em canais integrados, plugins de canal incluídos, WebChat e Nodes móveis.

Fazer o onboarding e instalar o serviço

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## Message presentation

**URL:** https://docs.openclaw.ai/plugins/message-presentation

**Contents:**
- Message presentation
- Documentation Index
- ​Contract
- ​Producer examples
- ​Renderer contract
- ​Core render flow
- ​Degradation rules
- ​Provider mapping
- ​Presentation vs InteractiveReply
- ​Delivery pin

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
import type {
  MessagePresentation,
  ReplyPayloadDelivery,
} from "openclaw/plugin-sdk/interactive-runtime";
```

Example 2 (typescript):
```typescript
type MessagePresentation = {
  title?: string;
  tone?: "neutral" | "info" | "success" | "warning" | "danger";
  blocks: MessagePresentationBlock[];
};

type MessagePresentationBlock =
  | { type: "text"; text: string }
  | { type: "context"; text: string }
  | { type: "divider" }
  | { type: "buttons"; buttons: MessagePresentationButton[] }
  | { type: "select"; placeholder?: string; options: MessagePresentationOption[] };

type MessagePresentationButton = {
  label: string;
  value?: string;
  url?: string;
  style?: "primary" | "secondary" | "success" | "danger";
};

type MessagePresentationOption = {
  label: string;
  value: string;
};

type ReplyPayloadDelivery = {
  pin?:
    | boolean
    | {
        enabled: boolean;
        notify?: boolean;
        required?: boolean;
      };
};
```

Example 3 (json):
```json
{
  "title": "Deploy approval",
  "tone": "warning",
  "blocks": [
    { "type": "text", "text": "Canary is ready to promote." },
    { "type": "context", "text": "Build 1234, staging passed." },
    {
      "type": "buttons",
      "buttons": [
        { "label": "Approve", "value": "deploy:approve", "style": "success" },
        { "label": "Decline", "value": "deploy:decline", "style": "danger" }
      ]
    }
  ]
}
```

Example 4 (json):
```json
{
  "blocks": [
    { "type": "text", "text": "Release notes are ready." },
    {
      "type": "buttons",
      "buttons": [{ "label": "Open notes", "url": "https://example.com/release" }]
    }
  ]
}
```

---

## Building channel plugins

**URL:** https://docs.openclaw.ai/plugins/sdk-channel-plugins

**Contents:**
- Building channel plugins
- Documentation Index
- ​How channel plugins work
- ​Approvals and channel capabilities
- ​Inbound mention policy
- ​Walkthrough
- ​File structure
- ​Advanced topics
- Threading options
- Message tool integration

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Build the channel plugin object

What createChatChannelPlugin does for you

Handle inbound messages

**Examples:**

Example 1 (lua):
```lua
import {
  implicitMentionKindWhen,
  matchesMentionWithExplicit,
  resolveInboundMentionDecision,
} from "openclaw/plugin-sdk/channel-inbound";

const mentionMatch = matchesMentionWithExplicit(text, {
  mentionRegexes,
  mentionPatterns,
});

const facts = {
  canDetectMention: true,
  wasMentioned: mentionMatch.matched,
  hasAnyMention: mentionMatch.hasExplicitMention,
  implicitMentionKinds: [
    ...implicitMentionKindWhen("reply_to_bot", isReplyToBot),
    ...implicitMentionKindWhen("quoted_bot", isQuoteOfBot),
  ],
};

const decision = resolveInboundMentionDecision({
  facts,
  policy: {
    isGroup,
    requireMention,
    allowedImplicitMentionKinds: requireExplicitMention ? [] : ["reply_to_bot", "quoted_bot"],
    allowTextCommands,
    hasControlCommand,
    commandAuthorized,
  },
});

if (decision.shouldSkip) return;
```

Example 2 (json):
```json
{
  "name": "@myorg/openclaw-acme-chat",
  "version": "1.0.0",
  "type": "module",
  "openclaw": {
    "extensions": ["./index.ts"],
    "setupEntry": "./setup-entry.ts",
    "channel": {
      "id": "acme-chat",
      "label": "Acme Chat",
      "blurb": "Connect OpenClaw to Acme Chat."
    }
  }
}
```

Example 3 (typescript):
```typescript
import {
  createChatChannelPlugin,
  createChannelPluginBase,
} from "openclaw/plugin-sdk/channel-core";
import type { OpenClawConfig } from "openclaw/plugin-sdk/channel-core";
import { acmeChatApi } from "./client.js"; // your platform API client

type ResolvedAccount = {
  accountId: string | null;
  token: string;
  allowFrom: string[];
  dmPolicy: string | undefined;
};

function resolveAccount(
  cfg: OpenClawConfig,
  accountId?: string | null,
): ResolvedAccount {
  const section = (cfg.channels as Record<string, any>)?.["acme-chat"];
  const token = section?.token;
  if (!token) throw new Error("acme-chat: token is required");
  return {
    accountId: accountId ?? null,
    token,
    allowFrom: section?.allowFrom ?? [],
    dmPolicy: section?.dmSecurity,
  };
}

export const acmeChatPlugin = createChatChannelPlugin<ResolvedAccount>({
  base: createChannelPluginBase({
    id: "acme-chat",
    setup: {
      resolveAccount,
      inspectAccount(cfg, accountId) {
        const section =
          (cfg.channels as Record<string, any>)?.["acme-chat"];
        return {
          enabled: Boolean(section?.token),
          configured: Boolean(section?.token),
          tokenStatus: section?.token ? "available" : "missing",
        };
      },
    },
  }),

  // DM security: who can message the bot
  security: {
    dm: {
      channelKey: "acme-chat",
      resolvePolicy: (account) => account.dmPolicy,
      resolveAllowFrom: (account) => account.allowFrom,
      defaultPolicy: "allowlist",
    },
  },

  // Pairing: approval flow for new DM contacts
  pairing: {
    text: {
      idLabel: "Acme Chat username",
      message: "Send this code to verify your identity:",
      notify: async ({ target, code }) => {
        await acmeChatApi.sendDm(target, `Pairing code: ${code}`);
      },
    },
  },

  // Threading: how replies are delivered
  threading: { topLevelReplyToMode: "reply" },

  // Outbound: send messages to the platform
  outbound: {
    attachedResults: {
      sendText: async (params) => {
        const result = await acmeChatApi.sendMessage(
          params.to,
          params.text,
        );
        return { messageId: result.id };
      },
    },
    base: {
      sendMedia: async (params) => {
        await acmeChatApi.sendFile(params.to, params.filePath);
      },
    },
  },
});
```

Example 4 (lua):
```lua
import { defineChannelPluginEntry } from "openclaw/plugin-sdk/channel-core";
import { acmeChatPlugin } from "./src/channel.js";

export default defineChannelPluginEntry({
  id: "acme-chat",
  name: "Acme Chat",
  description: "Acme Chat channel plugin",
  plugin: acmeChatPlugin,
  registerCliMetadata(api) {
    api.registerCli(
      ({ program }) => {
        program
          .command("acme-chat")
          .description("Acme Chat management");
      },
      {
        descriptors: [
          {
            name: "acme-chat",
            description: "Acme Chat management",
            hasSubcommands: false,
          },
        ],
      },
    );
  },
  registerFull(api) {
    api.registerGatewayMethod(/* ... */);
  },
});
```

---

## Tavily

**URL:** https://docs.openclaw.ai/tools/tavily

**Contents:**
- Tavily
- Documentation Index
- ​Get an API key
- ​Configure Tavily search
- ​Tavily plugin tools
  - ​tavily_search
  - ​tavily_extract
- ​Choosing the right tool
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
{
  plugins: {
    entries: {
      tavily: {
        enabled: true,
        config: {
          webSearch: {
            apiKey: "tvly-...", // optional if TAVILY_API_KEY is set
            baseUrl: "https://api.tavily.com",
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "tavily",
      },
    },
  },
}
```

---

## DuckDuckGo search

**URL:** https://docs.openclaw.ai/tools/duckduckgo-search

**Contents:**
- DuckDuckGo search
- Documentation Index
- ​Setup
- ​Config
- ​Tool parameters
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sql):
```sql
openclaw configure --section web
# Select "duckduckgo" as the provider
```

Example 2 (json):
```json
{
  tools: {
    web: {
      search: {
        provider: "duckduckgo",
      },
    },
  },
}
```

Example 3 (json):
```json
{
  plugins: {
    entries: {
      duckduckgo: {
        config: {
          webSearch: {
            region: "us-en", // DuckDuckGo region code
            safeSearch: "moderate", // "strict", "moderate", or "off"
          },
        },
      },
    },
  },
}
```

---

## Exec approvals

**URL:** https://docs.openclaw.ai/tools/exec-approvals

**Contents:**
- Exec approvals
- Documentation Index
- ​Inspecting the effective policy
- ​Where it applies
  - ​Trust model
  - ​macOS split
- ​Settings and storage
- ​Policy knobs
  - ​exec.security
  - ​exec.ask

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set the requested config policy

Match the host approvals file

**Examples:**

Example 1 (unknown):
```unknown
~/.openclaw/exec-approvals.json
```

Example 2 (json):
```json
{
  "version": 1,
  "socket": {
    "path": "~/.openclaw/exec-approvals.sock",
    "token": "base64url-token"
  },
  "defaults": {
    "security": "deny",
    "ask": "on-miss",
    "askFallback": "deny",
    "autoAllowSkills": false
  },
  "agents": {
    "main": {
      "security": "allowlist",
      "ask": "on-miss",
      "askFallback": "deny",
      "autoAllowSkills": true,
      "allowlist": [
        {
          "id": "B0C8C0B3-2C2D-4F8A-9A3C-5A4B3C2D1E0F",
          "pattern": "~/Projects/**/bin/rg",
          "source": "allow-always",
          "commandText": "rg -n TODO",
          "lastUsedAt": 1737150000000,
          "lastUsedCommand": "rg -n TODO",
          "lastResolvedPath": "/Users/user/Projects/.../bin/rg"
        }
      ]
    }
  }
}
```

Example 3 (unknown):
```unknown
openclaw config set tools.exec.host gateway
openclaw config set tools.exec.security full
openclaw config set tools.exec.ask off
openclaw gateway restart
```

Example 4 (json):
```json
openclaw approvals set --stdin <<'EOF'
{
  version: 1,
  defaults: {
    security: "full",
    ask: "off",
    askFallback: "full"
  }
}
EOF
```

---

## WSL2 + Windows + remote Chrome CDP troubleshooting

**URL:** https://docs.openclaw.ai/tools/browser-wsl2-windows-remote-cdp-troubleshooting

**Contents:**
- WSL2 + Windows + remote Chrome CDP troubleshooting
- Documentation Index
- ​Choose the right browser mode first
  - ​Option 1: Raw remote CDP from WSL2 to Windows
  - ​Option 2: Host-local Chrome MCP
- ​Working architecture
- ​Why this setup is confusing
- ​Critical rule for the Control UI
- ​Validate in layers
  - ​Layer 1: Verify Chrome is serving CDP on Windows

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
chrome.exe --remote-debugging-port=9222
```

Example 2 (json):
```json
curl http://127.0.0.1:9222/json/version
curl http://127.0.0.1:9222/json/list
```

Example 3 (json):
```json
curl http://WINDOWS_HOST_OR_IP:9222/json/version
curl http://WINDOWS_HOST_OR_IP:9222/json/list
```

Example 4 (json):
```json
{
  browser: {
    enabled: true,
    defaultProfile: "remote",
    profiles: {
      remote: {
        cdpUrl: "http://WINDOWS_HOST_OR_IP:9222",
        attachOnly: true,
        color: "#00AA00",
      },
    },
  },
}
```

---

## Creating skills

**URL:** https://docs.openclaw.ai/tools/creating-skills

**Contents:**
- Creating skills
- Documentation Index
- ​Create your first skill
- ​Skill metadata reference
- ​Best practices
- ​Where skills live
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create the skill directory

**Examples:**

Example 1 (unknown):
```unknown
mkdir -p ~/.openclaw/workspace/skills/hello-world
```

Example 2 (yaml):
```yaml
---
name: hello-world
description: A simple skill that says hello.
---

# Hello World Skill

When the user asks for a greeting, use the `echo` tool to say
"Hello from your custom skill!".
```

Example 3 (sql):
```sql
# From chat
/new

# Or restart the gateway
openclaw gateway restart
```

Example 4 (unknown):
```unknown
openclaw skills list
```

---

## TOOLS.md template

**URL:** https://docs.openclaw.ai/reference/templates/TOOLS

**Contents:**
- TOOLS.md template
- Documentation Index
- ​TOOLS.md - Local Notes
- ​What Goes Here
- ​Examples
- ​Why Separate?
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

---

## Tools and plugins

**URL:** https://docs.openclaw.ai/tools

**Contents:**
- Tools and plugins
- Documentation Index
- ​Tools, skills, and plugins
- ​Built-in tools
  - ​Plugin-provided tools
- ​Tool configuration
  - ​Allow and deny lists
  - ​Tool profiles
  - ​Tool groups
  - ​Provider-specific restrictions

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Tools are what the agent calls

Skills teach the agent when and how

Plugins package everything together

**Examples:**

Example 1 (json):
```json
{
  tools: {
    allow: ["group:fs", "browser", "web_search"],
    deny: ["exec"],
  },
}
```

Example 2 (json):
```json
{
  tools: {
    profile: "full",
  },
}
```

Example 3 (json):
```json
{
  tools: {
    profile: "coding",
    byProvider: {
      "google-antigravity": { profile: "minimal" },
    },
  },
}
```

---

## Plugin dependency resolution

**URL:** https://docs.openclaw.ai/plugins/dependency-resolution

**Contents:**
- Plugin dependency resolution
- Documentation Index
- ​Plugin dependency resolution
- ​Responsibility split
- ​Install roots
- ​Local plugins
- ​Startup and reload
- ​Bundled plugins
- ​Legacy cleanup

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
npm install --prefix ~/.openclaw/npm <spec> --omit=dev --ignore-scripts --no-audit --no-fund
```

Example 2 (sass):
```sass
npm install --omit=dev --ignore-scripts --no-audit --no-fund
```

Example 3 (sql):
```sql
openclaw plugins update <id>
openclaw plugins install <source>
openclaw doctor --fix
```

---

## Manage plugins

**URL:** https://docs.openclaw.ai/plugins/manage-plugins

**Contents:**
- Manage plugins
- Documentation Index
- ​List plugins
- ​Install plugins
- ​Update plugins
- ​Uninstall plugins
- ​Publish plugins
  - ​Publish to ClawHub
  - ​Publish to npmjs.com
- ​Source choice

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw plugins list
openclaw plugins list --enabled
openclaw plugins list --verbose
openclaw plugins list --json
```

Example 2 (unknown):
```unknown
openclaw plugins list --json \
  | jq '.plugins[] | {id, enabled, format, source, dependencyStatus}'
```

Example 3 (go):
```go
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

Example 4 (unknown):
```unknown
openclaw gateway restart
openclaw plugins inspect <plugin-id> --runtime --json
```

---

## Plugin internals

**URL:** https://docs.openclaw.ai/tools/capability-cookbook

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

## ACPx plugin

**URL:** https://docs.openclaw.ai/plugins/reference/acpx

**Contents:**
- ACPx plugin
- Documentation Index
- ​ACPx plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Alibaba plugin

**URL:** https://docs.openclaw.ai/plugins/reference/alibaba

**Contents:**
- Alibaba plugin
- Documentation Index
- ​Alibaba plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Amazon Bedrock plugin

**URL:** https://docs.openclaw.ai/plugins/reference/amazon-bedrock

**Contents:**
- Amazon Bedrock plugin
- Documentation Index
- ​Amazon Bedrock plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Amazon Bedrock Mantle plugin

**URL:** https://docs.openclaw.ai/plugins/reference/amazon-bedrock-mantle

**Contents:**
- Amazon Bedrock Mantle plugin
- Documentation Index
- ​Amazon Bedrock Mantle plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Arcee plugin

**URL:** https://docs.openclaw.ai/plugins/reference/arcee

**Contents:**
- Arcee plugin
- Documentation Index
- ​Arcee plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Azure Speech plugin

**URL:** https://docs.openclaw.ai/plugins/reference/azure-speech

**Contents:**
- Azure Speech plugin
- Documentation Index
- ​Azure Speech plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## BlueBubbles plugin

**URL:** https://docs.openclaw.ai/plugins/reference/bluebubbles

**Contents:**
- BlueBubbles plugin
- Documentation Index
- ​BlueBubbles plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Bonjour plugin

**URL:** https://docs.openclaw.ai/plugins/reference/bonjour

**Contents:**
- Bonjour plugin
- Documentation Index
- ​Bonjour plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Brave plugin

**URL:** https://docs.openclaw.ai/plugins/reference/brave

**Contents:**
- Brave plugin
- Documentation Index
- ​Brave plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Browser plugin

**URL:** https://docs.openclaw.ai/plugins/reference/browser

**Contents:**
- Browser plugin
- Documentation Index
- ​Browser plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## BytePlus plugin

**URL:** https://docs.openclaw.ai/plugins/reference/byteplus

**Contents:**
- BytePlus plugin
- Documentation Index
- ​BytePlus plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Cerebras plugin

**URL:** https://docs.openclaw.ai/plugins/reference/cerebras

**Contents:**
- Cerebras plugin
- Documentation Index
- ​Cerebras plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Chutes plugin

**URL:** https://docs.openclaw.ai/plugins/reference/chutes

**Contents:**
- Chutes plugin
- Documentation Index
- ​Chutes plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Cloudflare AI Gateway plugin

**URL:** https://docs.openclaw.ai/plugins/reference/cloudflare-ai-gateway

**Contents:**
- Cloudflare AI Gateway plugin
- Documentation Index
- ​Cloudflare AI Gateway plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Codex plugin

**URL:** https://docs.openclaw.ai/plugins/reference/codex

**Contents:**
- Codex plugin
- Documentation Index
- ​Codex plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## ComfyUI plugin

**URL:** https://docs.openclaw.ai/plugins/reference/comfy

**Contents:**
- ComfyUI plugin
- Documentation Index
- ​ComfyUI plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Deepgram plugin

**URL:** https://docs.openclaw.ai/plugins/reference/deepgram

**Contents:**
- Deepgram plugin
- Documentation Index
- ​Deepgram plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## DeepSeek plugin

**URL:** https://docs.openclaw.ai/plugins/reference/deepseek

**Contents:**
- DeepSeek plugin
- Documentation Index
- ​DeepSeek plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Diagnostics OpenTelemetry plugin

**URL:** https://docs.openclaw.ai/plugins/reference/diagnostics-otel

**Contents:**
- Diagnostics OpenTelemetry plugin
- Documentation Index
- ​Diagnostics OpenTelemetry plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Diagnostics Prometheus plugin

**URL:** https://docs.openclaw.ai/plugins/reference/diagnostics-prometheus

**Contents:**
- Diagnostics Prometheus plugin
- Documentation Index
- ​Diagnostics Prometheus plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Diffs plugin

**URL:** https://docs.openclaw.ai/plugins/reference/diffs

**Contents:**
- Diffs plugin
- Documentation Index
- ​Diffs plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Discord plugin

**URL:** https://docs.openclaw.ai/plugins/reference/discord

**Contents:**
- Discord plugin
- Documentation Index
- ​Discord plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Document Extract plugin

**URL:** https://docs.openclaw.ai/plugins/reference/document-extract

**Contents:**
- Document Extract plugin
- Documentation Index
- ​Document Extract plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## DuckDuckGo plugin

**URL:** https://docs.openclaw.ai/plugins/reference/duckduckgo

**Contents:**
- DuckDuckGo plugin
- Documentation Index
- ​DuckDuckGo plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Elevenlabs plugin

**URL:** https://docs.openclaw.ai/plugins/reference/elevenlabs

**Contents:**
- Elevenlabs plugin
- Documentation Index
- ​Elevenlabs plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Exa plugin

**URL:** https://docs.openclaw.ai/plugins/reference/exa

**Contents:**
- Exa plugin
- Documentation Index
- ​Exa plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## fal plugin

**URL:** https://docs.openclaw.ai/plugins/reference/fal

**Contents:**
- fal plugin
- Documentation Index
- ​fal plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Feishu plugin

**URL:** https://docs.openclaw.ai/plugins/reference/feishu

**Contents:**
- Feishu plugin
- Documentation Index
- ​Feishu plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## File Transfer plugin

**URL:** https://docs.openclaw.ai/plugins/reference/file-transfer

**Contents:**
- File Transfer plugin
- Documentation Index
- ​File Transfer plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Firecrawl plugin

**URL:** https://docs.openclaw.ai/plugins/reference/firecrawl

**Contents:**
- Firecrawl plugin
- Documentation Index
- ​Firecrawl plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Fireworks plugin

**URL:** https://docs.openclaw.ai/plugins/reference/fireworks

**Contents:**
- Fireworks plugin
- Documentation Index
- ​Fireworks plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Google plugin

**URL:** https://docs.openclaw.ai/plugins/reference/google

**Contents:**
- Google plugin
- Documentation Index
- ​Google plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Google Meet plugin

**URL:** https://docs.openclaw.ai/plugins/reference/google-meet

**Contents:**
- Google Meet plugin
- Documentation Index
- ​Google Meet plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Google Chat plugin

**URL:** https://docs.openclaw.ai/plugins/reference/googlechat

**Contents:**
- Google Chat plugin
- Documentation Index
- ​Google Chat plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Gradium plugin

**URL:** https://docs.openclaw.ai/plugins/reference/gradium

**Contents:**
- Gradium plugin
- Documentation Index
- ​Gradium plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Groq plugin

**URL:** https://docs.openclaw.ai/plugins/reference/groq

**Contents:**
- Groq plugin
- Documentation Index
- ​Groq plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Hugging Face plugin

**URL:** https://docs.openclaw.ai/plugins/reference/huggingface

**Contents:**
- Hugging Face plugin
- Documentation Index
- ​Hugging Face plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## iMessage plugin

**URL:** https://docs.openclaw.ai/plugins/reference/imessage

**Contents:**
- iMessage plugin
- Documentation Index
- ​iMessage plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Inworld plugin

**URL:** https://docs.openclaw.ai/plugins/reference/inworld

**Contents:**
- Inworld plugin
- Documentation Index
- ​Inworld plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## IRC plugin

**URL:** https://docs.openclaw.ai/plugins/reference/irc

**Contents:**
- IRC plugin
- Documentation Index
- ​IRC plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Kilocode plugin

**URL:** https://docs.openclaw.ai/plugins/reference/kilocode

**Contents:**
- Kilocode plugin
- Documentation Index
- ​Kilocode plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Kimi plugin

**URL:** https://docs.openclaw.ai/plugins/reference/kimi

**Contents:**
- Kimi plugin
- Documentation Index
- ​Kimi plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## LINE plugin

**URL:** https://docs.openclaw.ai/plugins/reference/line

**Contents:**
- LINE plugin
- Documentation Index
- ​LINE plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## LiteLLM plugin

**URL:** https://docs.openclaw.ai/plugins/reference/litellm

**Contents:**
- LiteLLM plugin
- Documentation Index
- ​LiteLLM plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## LLM Task plugin

**URL:** https://docs.openclaw.ai/plugins/reference/llm-task

**Contents:**
- LLM Task plugin
- Documentation Index
- ​LLM Task plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## LM Studio plugin

**URL:** https://docs.openclaw.ai/plugins/reference/lmstudio

**Contents:**
- LM Studio plugin
- Documentation Index
- ​LM Studio plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Lobster plugin

**URL:** https://docs.openclaw.ai/plugins/reference/lobster

**Contents:**
- Lobster plugin
- Documentation Index
- ​Lobster plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Matrix plugin

**URL:** https://docs.openclaw.ai/plugins/reference/matrix

**Contents:**
- Matrix plugin
- Documentation Index
- ​Matrix plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Mattermost plugin

**URL:** https://docs.openclaw.ai/plugins/reference/mattermost

**Contents:**
- Mattermost plugin
- Documentation Index
- ​Mattermost plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Memory Core plugin

**URL:** https://docs.openclaw.ai/plugins/reference/memory-core

**Contents:**
- Memory Core plugin
- Documentation Index
- ​Memory Core plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Memory Lancedb plugin

**URL:** https://docs.openclaw.ai/plugins/reference/memory-lancedb

**Contents:**
- Memory Lancedb plugin
- Documentation Index
- ​Memory Lancedb plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Memory Wiki plugin

**URL:** https://docs.openclaw.ai/plugins/reference/memory-wiki

**Contents:**
- Memory Wiki plugin
- Documentation Index
- ​Memory Wiki plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Microsoft plugin

**URL:** https://docs.openclaw.ai/plugins/reference/microsoft

**Contents:**
- Microsoft plugin
- Documentation Index
- ​Microsoft plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Microsoft Foundry plugin

**URL:** https://docs.openclaw.ai/plugins/reference/microsoft-foundry

**Contents:**
- Microsoft Foundry plugin
- Documentation Index
- ​Microsoft Foundry plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Migrate Claude plugin

**URL:** https://docs.openclaw.ai/plugins/reference/migrate-claude

**Contents:**
- Migrate Claude plugin
- Documentation Index
- ​Migrate Claude plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Migrate Hermes plugin

**URL:** https://docs.openclaw.ai/plugins/reference/migrate-hermes

**Contents:**
- Migrate Hermes plugin
- Documentation Index
- ​Migrate Hermes plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## MiniMax plugin

**URL:** https://docs.openclaw.ai/plugins/reference/minimax

**Contents:**
- MiniMax plugin
- Documentation Index
- ​MiniMax plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Mistral plugin

**URL:** https://docs.openclaw.ai/plugins/reference/mistral

**Contents:**
- Mistral plugin
- Documentation Index
- ​Mistral plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Moonshot plugin

**URL:** https://docs.openclaw.ai/plugins/reference/moonshot

**Contents:**
- Moonshot plugin
- Documentation Index
- ​Moonshot plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Microsoft Teams plugin

**URL:** https://docs.openclaw.ai/plugins/reference/msteams

**Contents:**
- Microsoft Teams plugin
- Documentation Index
- ​Microsoft Teams plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Nextcloud Talk plugin

**URL:** https://docs.openclaw.ai/plugins/reference/nextcloud-talk

**Contents:**
- Nextcloud Talk plugin
- Documentation Index
- ​Nextcloud Talk plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Nostr plugin

**URL:** https://docs.openclaw.ai/plugins/reference/nostr

**Contents:**
- Nostr plugin
- Documentation Index
- ​Nostr plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## NVIDIA plugin

**URL:** https://docs.openclaw.ai/plugins/reference/nvidia

**Contents:**
- NVIDIA plugin
- Documentation Index
- ​NVIDIA plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Ollama plugin

**URL:** https://docs.openclaw.ai/plugins/reference/ollama

**Contents:**
- Ollama plugin
- Documentation Index
- ​Ollama plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Open Prose plugin

**URL:** https://docs.openclaw.ai/plugins/reference/open-prose

**Contents:**
- Open Prose plugin
- Documentation Index
- ​Open Prose plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OpenAI plugin

**URL:** https://docs.openclaw.ai/plugins/reference/openai

**Contents:**
- OpenAI plugin
- Documentation Index
- ​OpenAI plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OpenCode plugin

**URL:** https://docs.openclaw.ai/plugins/reference/opencode

**Contents:**
- OpenCode plugin
- Documentation Index
- ​OpenCode plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OpenCode Go plugin

**URL:** https://docs.openclaw.ai/plugins/reference/opencode-go

**Contents:**
- OpenCode Go plugin
- Documentation Index
- ​OpenCode Go plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OpenRouter plugin

**URL:** https://docs.openclaw.ai/plugins/reference/openrouter

**Contents:**
- OpenRouter plugin
- Documentation Index
- ​OpenRouter plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Openshell plugin

**URL:** https://docs.openclaw.ai/plugins/reference/openshell

**Contents:**
- Openshell plugin
- Documentation Index
- ​Openshell plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Perplexity plugin

**URL:** https://docs.openclaw.ai/plugins/reference/perplexity

**Contents:**
- Perplexity plugin
- Documentation Index
- ​Perplexity plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## QA Channel plugin

**URL:** https://docs.openclaw.ai/plugins/reference/qa-channel

**Contents:**
- QA Channel plugin
- Documentation Index
- ​QA Channel plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## QA Lab plugin

**URL:** https://docs.openclaw.ai/plugins/reference/qa-lab

**Contents:**
- QA Lab plugin
- Documentation Index
- ​QA Lab plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## QA Matrix plugin

**URL:** https://docs.openclaw.ai/plugins/reference/qa-matrix

**Contents:**
- QA Matrix plugin
- Documentation Index
- ​QA Matrix plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Qianfan plugin

**URL:** https://docs.openclaw.ai/plugins/reference/qianfan

**Contents:**
- Qianfan plugin
- Documentation Index
- ​Qianfan plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## QQ Bot plugin

**URL:** https://docs.openclaw.ai/plugins/reference/qqbot

**Contents:**
- QQ Bot plugin
- Documentation Index
- ​QQ Bot plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Qwen plugin

**URL:** https://docs.openclaw.ai/plugins/reference/qwen

**Contents:**
- Qwen plugin
- Documentation Index
- ​Qwen plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Runway plugin

**URL:** https://docs.openclaw.ai/plugins/reference/runway

**Contents:**
- Runway plugin
- Documentation Index
- ​Runway plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## SearXNG plugin

**URL:** https://docs.openclaw.ai/plugins/reference/searxng

**Contents:**
- SearXNG plugin
- Documentation Index
- ​SearXNG plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Senseaudio plugin

**URL:** https://docs.openclaw.ai/plugins/reference/senseaudio

**Contents:**
- Senseaudio plugin
- Documentation Index
- ​Senseaudio plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## SGLang plugin

**URL:** https://docs.openclaw.ai/plugins/reference/sglang

**Contents:**
- SGLang plugin
- Documentation Index
- ​SGLang plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Signal plugin

**URL:** https://docs.openclaw.ai/plugins/reference/signal

**Contents:**
- Signal plugin
- Documentation Index
- ​Signal plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Skill Workshop plugin

**URL:** https://docs.openclaw.ai/plugins/reference/skill-workshop

**Contents:**
- Skill Workshop plugin
- Documentation Index
- ​Skill Workshop plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Slack plugin

**URL:** https://docs.openclaw.ai/plugins/reference/slack

**Contents:**
- Slack plugin
- Documentation Index
- ​Slack plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## StepFun plugin

**URL:** https://docs.openclaw.ai/plugins/reference/stepfun

**Contents:**
- StepFun plugin
- Documentation Index
- ​StepFun plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Synology Chat plugin

**URL:** https://docs.openclaw.ai/plugins/reference/synology-chat

**Contents:**
- Synology Chat plugin
- Documentation Index
- ​Synology Chat plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Synthetic plugin

**URL:** https://docs.openclaw.ai/plugins/reference/synthetic

**Contents:**
- Synthetic plugin
- Documentation Index
- ​Synthetic plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Tavily plugin

**URL:** https://docs.openclaw.ai/plugins/reference/tavily

**Contents:**
- Tavily plugin
- Documentation Index
- ​Tavily plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Telegram plugin

**URL:** https://docs.openclaw.ai/plugins/reference/telegram

**Contents:**
- Telegram plugin
- Documentation Index
- ​Telegram plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Tencent plugin

**URL:** https://docs.openclaw.ai/plugins/reference/tencent

**Contents:**
- Tencent plugin
- Documentation Index
- ​Tencent plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Tlon plugin

**URL:** https://docs.openclaw.ai/plugins/reference/tlon

**Contents:**
- Tlon plugin
- Documentation Index
- ​Tlon plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Together plugin

**URL:** https://docs.openclaw.ai/plugins/reference/together

**Contents:**
- Together plugin
- Documentation Index
- ​Together plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Tokenjuice plugin

**URL:** https://docs.openclaw.ai/plugins/reference/tokenjuice

**Contents:**
- Tokenjuice plugin
- Documentation Index
- ​Tokenjuice plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## TTS Local CLI plugin

**URL:** https://docs.openclaw.ai/plugins/reference/tts-local-cli

**Contents:**
- TTS Local CLI plugin
- Documentation Index
- ​TTS Local CLI plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Twitch plugin

**URL:** https://docs.openclaw.ai/plugins/reference/twitch

**Contents:**
- Twitch plugin
- Documentation Index
- ​Twitch plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Venice plugin

**URL:** https://docs.openclaw.ai/plugins/reference/venice

**Contents:**
- Venice plugin
- Documentation Index
- ​Venice plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Vercel AI Gateway plugin

**URL:** https://docs.openclaw.ai/plugins/reference/vercel-ai-gateway

**Contents:**
- Vercel AI Gateway plugin
- Documentation Index
- ​Vercel AI Gateway plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## vLLM plugin

**URL:** https://docs.openclaw.ai/plugins/reference/vllm

**Contents:**
- vLLM plugin
- Documentation Index
- ​vLLM plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Voice Call plugin

**URL:** https://docs.openclaw.ai/plugins/reference/voice-call

**Contents:**
- Voice Call plugin
- Documentation Index
- ​Voice Call plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Volcengine plugin

**URL:** https://docs.openclaw.ai/plugins/reference/volcengine

**Contents:**
- Volcengine plugin
- Documentation Index
- ​Volcengine plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Voyage plugin

**URL:** https://docs.openclaw.ai/plugins/reference/voyage

**Contents:**
- Voyage plugin
- Documentation Index
- ​Voyage plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Vydra plugin

**URL:** https://docs.openclaw.ai/plugins/reference/vydra

**Contents:**
- Vydra plugin
- Documentation Index
- ​Vydra plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Web Readability plugin

**URL:** https://docs.openclaw.ai/plugins/reference/web-readability

**Contents:**
- Web Readability plugin
- Documentation Index
- ​Web Readability plugin
- ​Distribution
- ​Surface

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Webhooks plugin

**URL:** https://docs.openclaw.ai/plugins/reference/webhooks

**Contents:**
- Webhooks plugin
- Documentation Index
- ​Webhooks plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## WhatsApp plugin

**URL:** https://docs.openclaw.ai/plugins/reference/whatsapp

**Contents:**
- WhatsApp plugin
- Documentation Index
- ​WhatsApp plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## xAI plugin

**URL:** https://docs.openclaw.ai/plugins/reference/xai

**Contents:**
- xAI plugin
- Documentation Index
- ​xAI plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Xiaomi plugin

**URL:** https://docs.openclaw.ai/plugins/reference/xiaomi

**Contents:**
- Xiaomi plugin
- Documentation Index
- ​Xiaomi plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Z.AI plugin

**URL:** https://docs.openclaw.ai/plugins/reference/zai

**Contents:**
- Z.AI plugin
- Documentation Index
- ​Z.AI plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Zalo plugin

**URL:** https://docs.openclaw.ai/plugins/reference/zalo

**Contents:**
- Zalo plugin
- Documentation Index
- ​Zalo plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Zalo Personal plugin

**URL:** https://docs.openclaw.ai/plugins/reference/zalouser

**Contents:**
- Zalo Personal plugin
- Documentation Index
- ​Zalo Personal plugin
- ​Distribution
- ​Surface
- ​Related docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---
