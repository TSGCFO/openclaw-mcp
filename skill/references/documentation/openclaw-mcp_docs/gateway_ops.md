# Openclaw-Mcp_Docs - Gateway Ops

**Pages:** 43

---

## Tailscale

**URL:** https://docs.openclaw.ai/gateway/tailscale

**Contents:**
- Tailscale
- Documentation Index
- ​Modes
- ​Auth
- ​Config examples
  - ​Tailnet-only (Serve)
  - ​Tailnet-only (bind to Tailnet IP)
  - ​Public internet (Funnel + shared password)
- ​CLI examples
- ​Notes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "serve" },
  },
}
```

Example 2 (json):
```json
{
  gateway: {
    bind: "tailnet",
    auth: { mode: "token", token: "your-token" },
  },
}
```

Example 3 (json):
```json
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "funnel" },
    auth: { mode: "password", password: "replace-me" },
  },
}
```

Example 4 (unknown):
```unknown
openclaw gateway --tailscale serve
openclaw gateway --tailscale funnel --auth password
```

---

## Health checks

**URL:** https://docs.openclaw.ai/gateway/health

**Contents:**
- Health checks
- Documentation Index
- ​Quick checks
- ​Deep diagnostics
- ​Health monitor config
- ​When something fails
- ​Dedicated “health” command
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Logs

**URL:** https://docs.openclaw.ai/cli/logs

**Contents:**
- Logs
- Documentation Index
- ​openclaw logs
- ​Options
- ​Shared Gateway RPC options
- ​Examples
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
openclaw logs
openclaw logs --follow
openclaw logs --follow --interval 2000
openclaw logs --limit 500 --max-bytes 500000
openclaw logs --json
openclaw logs --plain
openclaw logs --no-color
openclaw logs --limit 500
openclaw logs --local-time
openclaw logs --follow --local-time
openclaw logs --url ws://127.0.0.1:18789 --token "$OPENCLAW_GATEWAY_TOKEN"
```

---

## Trusted proxy auth

**URL:** https://docs.openclaw.ai/gateway/trusted-proxy-auth

**Contents:**
- Trusted proxy auth
- Documentation Index
- ​When to use
- ​When NOT to use
- ​How it works
- ​Control UI pairing behavior
- ​Configuration
  - ​Configuration reference
- ​TLS termination and HSTS
  - ​Rollout guidance

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Proxy authenticates the user

Proxy adds an identity header

Gateway verifies trusted source

Gateway extracts identity

Traefik with forward auth

trusted_proxy_untrusted_source

trusted_proxy_loopback_source

trusted_proxy_user_missing

trusted_proxy_missing_header_*

trusted_proxy_user_not_allowed

trusted_proxy_origin_not_allowed

WebSocket still failing

Test the proxy independently

Update OpenClaw config

**Examples:**

Example 1 (json):
```json
{
  gateway: {
    // Trusted-proxy auth expects requests from a non-loopback trusted proxy source by default
    bind: "lan",

    // CRITICAL: Only add your proxy's IP(s) here
    trustedProxies: ["10.0.0.1", "172.17.0.1"],

    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        // Header containing authenticated user identity (required)
        userHeader: "x-forwarded-user",

        // Optional: headers that MUST be present (proxy verification)
        requiredHeaders: ["x-forwarded-proto", "x-forwarded-host"],

        // Optional: restrict to specific users (empty = allow all)
        allowUsers: ["nick@example.com", "admin@company.org"],

        // Optional: allow a same-host loopback proxy after explicit opt-in
        allowLoopback: false,
      },
    },
  },
}
```

Example 2 (sass):
```sass
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

Example 3 (sass):
```sass
{
  gateway: {
    tls: { enabled: true },
    http: {
      securityHeaders: {
        strictTransportSecurity: "max-age=31536000; includeSubDomains",
      },
    },
  },
}
```

Example 4 (json):
```json
{
  gateway: {
    bind: "lan",
    trustedProxies: ["10.0.0.1"], // Pomerium's IP
    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        userHeader: "x-pomerium-claim-email",
        requiredHeaders: ["x-pomerium-jwt-assertion"],
      },
    },
  },
}
```

---

## Secrets management

**URL:** https://docs.openclaw.ai/gateway/secrets

**Contents:**
- Secrets management
- Documentation Index
- ​Goals and runtime model
- ​Active-surface filtering
- ​Gateway auth surface diagnostics
- ​Onboarding reference preflight
- ​SecretRef contract
- ​Provider config
- ​Exec integration examples
- ​MCP server environment variables

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Examples of inactive surfaces

**Examples:**

Example 1 (lua):
```lua
{ source: "env" | "file" | "exec", provider: "default", id: "..." }
```

Example 2 (json):
```json
{ source: "env", provider: "default", id: "OPENAI_API_KEY" }
```

Example 3 (json):
```json
{ source: "file", provider: "filemain", id: "/providers/openai/apiKey" }
```

Example 4 (json):
```json
{ source: "exec", provider: "vault", id: "providers/openai/apiKey" }
```

---

## Authentication

**URL:** https://docs.openclaw.ai/gateway/authentication

**Contents:**
- Authentication
- Documentation Index
- ​Recommended setup (API key, any provider)
- ​Anthropic: Claude CLI and token compatibility
- ​Anthropic note
- ​Checking model auth status
- ​API key rotation behavior (gateway)
- ​Controlling which credential is used
  - ​Per-session (chat command)
  - ​Per-agent (CLI override)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
export <PROVIDER>_API_KEY="..."
openclaw models status
```

Example 2 (lua):
```lua
cat >> ~/.openclaw/.env <<'EOF'
<PROVIDER>_API_KEY=...
EOF
```

Example 3 (unknown):
```unknown
openclaw models status
openclaw doctor
```

Example 4 (markdown):
```markdown
# Run on the gateway host
claude auth login
claude auth status --text
openclaw models auth login --provider anthropic --method cli --set-default
```

---

## Configuration reference

**URL:** https://docs.openclaw.ai/gateway/configuration-reference

**Contents:**
- Configuration reference
- Documentation Index
- ​Channels
- ​Agent defaults, multi-agent, sessions, and messages
- ​Tools and custom providers
- ​Models
- ​MCP
- ​Skills
- ​Plugins
- ​Commitments

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway field details

Legacy bridge config (historical reference)

**Examples:**

Example 1 (json):
```json
{
  models: {
    // Optional. Default: true. Requires a Gateway restart when changed.
    pricing: { enabled: false },
  },
}
```

Example 2 (json):
```json
{
  mcp: {
    // Optional. Default: 600000 ms (10 minutes). Set 0 to disable idle eviction.
    sessionIdleTtlMs: 600000,
    servers: {
      docs: {
        command: "npx",
        args: ["-y", "@modelcontextprotocol/server-fetch"],
      },
      remote: {
        url: "https://example.com/mcp",
        transport: "streamable-http", // streamable-http | sse
        headers: {
          Authorization: "Bearer ${MCP_REMOTE_TOKEN}",
        },
      },
    },
  },
}
```

Example 3 (json):
```json
{
  skills: {
    allowBundled: ["gemini", "peekaboo"],
    load: {
      extraDirs: ["~/Projects/agent-scripts/skills"],
    },
    install: {
      preferBrew: true,
      nodeManager: "npm", // npm | pnpm | yarn | bun
    },
    entries: {
      "image-lab": {
        apiKey: { source: "env", provider: "default", id: "GEMINI_API_KEY" }, // or plaintext string
        env: { GEMINI_API_KEY: "GEMINI_KEY_HERE" },
      },
      peekaboo: { enabled: true },
      sag: { enabled: false },
    },
  },
}
```

Example 4 (json):
```json
{
  plugins: {
    enabled: true,
    allow: ["voice-call"],
    deny: [],
    load: {
      paths: ["~/Projects/oss/voice-call-plugin"],
    },
    entries: {
      "voice-call": {
        enabled: true,
        hooks: {
          allowPromptInjection: false,
        },
        config: { provider: "twilio" },
      },
    },
  },
}
```

---

## Sandboxing

**URL:** https://docs.openclaw.ai/gateway/sandboxing

**Contents:**
- Sandboxing
- Documentation Index
- ​What gets sandboxed
- ​Modes
- ​Scope
- ​Backend
  - ​Choosing a backend
  - ​Docker backend
  - ​SSH backend
  - ​OpenShell backend

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Sandboxed browser details

Authentication material

Remote-canonical consequences

Remote transport details

Current OpenShell limitations

Build the default image

Optional: build the common image

Optional: build the sandbox browser image

Sandbox browser Chromium defaults

Network security defaults

**Examples:**

Example 1 (elixir):
```elixir
{
  agents: {
    defaults: {
      sandbox: {
        mode: "all",
        backend: "ssh",
        scope: "session",
        workspaceAccess: "rw",
        ssh: {
          target: "user@gateway-host:22",
          workspaceRoot: "/tmp/openclaw-sandboxes",
          strictHostKeyChecking: true,
          updateHostKeys: true,
          identityFile: "~/.ssh/id_ed25519",
          certificateFile: "~/.ssh/id_ed25519-cert.pub",
          knownHostsFile: "~/.ssh/known_hosts",
          // Or use SecretRefs / inline contents instead of local files:
          // identityData: { source: "env", provider: "default", id: "SSH_IDENTITY" },
          // certificateData: { source: "env", provider: "default", id: "SSH_CERTIFICATE" },
          // knownHostsData: { source: "env", provider: "default", id: "SSH_KNOWN_HOSTS" },
        },
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
      sandbox: {
        mode: "all",
        backend: "openshell",
        scope: "session",
        workspaceAccess: "rw",
      },
    },
  },
  plugins: {
    entries: {
      openshell: {
        enabled: true,
        config: {
          from: "openclaw",
          mode: "remote", // mirror | remote
          remoteWorkspaceDir: "/sandbox",
          remoteAgentWorkspaceDir: "/agent",
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
      sandbox: {
        docker: {
          binds: ["/home/user/source:/source:ro", "/var/data/myapp:/data:ro"],
        },
      },
    },
    list: [
      {
        id: "build",
        sandbox: {
          docker: {
            binds: ["/mnt/cache:/cache:rw"],
          },
        },
      },
    ],
  },
}
```

Example 4 (unknown):
```unknown
scripts/sandbox-setup.sh
```

---

## Configuration

**URL:** https://docs.openclaw.ai/gateway/configuration

**Contents:**
- Configuration
- Documentation Index
- ​Minimal config
- ​Editing config
- ​Strict validation
- ​Common tasks
- ​Config hot reload
  - ​Reload modes
  - ​What hot-applies vs what needs a restart
  - ​Reload planning

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set up a channel (WhatsApp, Telegram, Discord, etc.)

Choose and configure models

Control who can message the bot

Set up group chat mention gating

Restrict skills per agent

Tune gateway channel health monitoring

Tune gateway WebSocket handshake timeout

Configure sessions and resets

Enable relay-backed push for official iOS builds

Set up heartbeat (periodic check-ins)

Set up webhooks (hooks)

Configure multi-agent routing

Split config into multiple files ($include)

Shell env import (optional)

Env var substitution in config values

Secret refs (env, file, exec)

**Examples:**

Example 1 (sass):
```sass
// ~/.openclaw/openclaw.json
{
  agents: { defaults: { workspace: "~/.openclaw/workspace" } },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

Example 2 (unknown):
```unknown
openclaw onboard       # full onboarding flow
openclaw configure     # config wizard
```

Example 3 (unknown):
```unknown
openclaw config get agents.defaults.workspace
openclaw config set agents.defaults.heartbeat.every "2h"
openclaw config unset plugins.entries.brave.config.webSearch.apiKey
```

Example 4 (json):
```json
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",   // pairing | allowlist | open | disabled
      allowFrom: ["tg:123"], // only for allowlist/open
    },
  },
}
```

---

## Discovery and transports

**URL:** https://docs.openclaw.ai/gateway/discovery

**Contents:**
- Discovery and transports
- Documentation Index
- ​Discovery & transports
- ​Terms
- ​Why we keep both “direct” and SSH
- ​Discovery inputs (how clients learn where the gateway is)
  - ​1) Bonjour / DNS-SD discovery
    - ​Service beacon details
  - ​2) Tailnet (cross-network)
  - ​3) Manual / SSH target

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OpenShell

**URL:** https://docs.openclaw.ai/gateway/openshell

**Contents:**
- OpenShell
- Documentation Index
- ​Prerequisites
- ​Quick start
- ​Workspace modes
  - ​mirror
  - ​remote
  - ​Choosing a mode
- ​Configuration reference
- ​Examples

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      sandbox: {
        mode: "all",
        backend: "openshell",
        scope: "session",
        workspaceAccess: "rw",
      },
    },
  },
  plugins: {
    entries: {
      openshell: {
        enabled: true,
        config: {
          from: "openclaw",
          mode: "remote",
        },
      },
    },
  },
}
```

Example 2 (unknown):
```unknown
openclaw sandbox list
openclaw sandbox explain
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      sandbox: {
        mode: "all",
        backend: "openshell",
      },
    },
  },
  plugins: {
    entries: {
      openshell: {
        enabled: true,
        config: {
          from: "openclaw",
          mode: "remote",
        },
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
      sandbox: {
        mode: "all",
        backend: "openshell",
        scope: "agent",
        workspaceAccess: "rw",
      },
    },
  },
  plugins: {
    entries: {
      openshell: {
        enabled: true,
        config: {
          from: "openclaw",
          mode: "mirror",
          gpu: true,
          providers: ["openai"],
          timeoutSeconds: 180,
        },
      },
    },
  },
}
```

---

## Network model

**URL:** https://docs.openclaw.ai/gateway/network-model

**Contents:**
- Network model
- Documentation Index
- ​Core rules
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Remote gateway setup

**URL:** https://docs.openclaw.ai/gateway/remote-gateway-readme

**Contents:**
- Remote gateway setup
- Documentation Index
- ​Running OpenClaw.app with a Remote Gateway
- ​Overview
- ​Quick setup
  - ​Step 1: Add SSH Config
  - ​Step 2: Copy SSH Key
  - ​Step 3: Configure Remote Gateway Auth
  - ​Step 4: Start SSH Tunnel
  - ​Step 5: Restart OpenClaw.app

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
Host remote-gateway
    HostName <REMOTE_IP>          # e.g., 172.27.187.184
    User <REMOTE_USER>            # e.g., jefferson
    LocalForward 18789 127.0.0.1:18789
    IdentityFile ~/.ssh/id_rsa
```

Example 2 (typescript):
```typescript
ssh-copy-id -i ~/.ssh/id_rsa <REMOTE_USER>@<REMOTE_IP>
```

Example 3 (unknown):
```unknown
openclaw config set gateway.remote.token "<your-token>"
```

Example 4 (unknown):
```unknown
ssh -N remote-gateway &
```

---

## Threat model (MITRE ATLAS)

**URL:** https://docs.openclaw.ai/security/THREAT-MODEL-ATLAS

**Contents:**
- Threat model (MITRE ATLAS)
- Documentation Index
- ​OpenClaw Threat Model v1.0
- ​MITRE ATLAS Framework
  - ​Framework attribution
  - ​Contributing to This Threat Model
- ​1. Introduction
  - ​1.1 Purpose
  - ​1.2 Scope
  - ​1.3 Out of Scope

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
┌─────────────────────────────────────────────────────────────────┐
│                    UNTRUSTED ZONE                                │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │  WhatsApp   │  │  Telegram   │  │   Discord   │  ...         │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘              │
│         │                │                │                      │
└─────────┼────────────────┼────────────────┼──────────────────────┘
          │                │                │
          ▼                ▼                ▼
┌─────────────────────────────────────────────────────────────────┐
│                 TRUST BOUNDARY 1: Channel Access                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                      GATEWAY                              │   │
│  │  • Device Pairing (1h DM / 5m node grace period)           │   │
│  │  • AllowFrom / AllowList validation                       │   │
│  │  • Token/Password/Tailscale auth                          │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                 TRUST BOUNDARY 2: Session Isolation              │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                   AGENT SESSIONS                          │   │
│  │  • Session key = agent:channel:peer                       │   │
│  │  • Tool policies per agent                                │   │
│  │  • Transcript logging                                     │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                 TRUST BOUNDARY 3: Tool Execution                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                  EXECUTION SANDBOX                        │   │
│  │  • Docker sandbox OR Host (exec-approvals)                │   │
│  │  • Node remote execution                                  │   │
│  │  • SSRF protection (DNS pinning + IP blocking)            │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                 TRUST BOUNDARY 4: External Content               │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              FETCHED URLs / EMAILS / WEBHOOKS             │   │
│  │  • External content wrapping (XML tags)                   │   │
│  │  • Security notice injection                              │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                 TRUST BOUNDARY 5: Supply Chain                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                      CLAWHUB                              │   │
│  │  • Skill publishing (semver, SKILL.md required)           │   │
│  │  • Pattern-based moderation flags                         │   │
│  │  • VirusTotal scanning (coming soon)                      │   │
│  │  • GitHub account age verification                        │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

Example 2 (unknown):
```unknown
// Known-bad identifiers
/(keepcold131\/ClawdAuthenticatorTool|ClawdAuthenticatorTool)/i

// Suspicious keywords
/(malware|stealer|phish|phishing|keylogger)/i
/(api[-_ ]?key|token|password|private key|secret)/i
/(wallet|seed phrase|mnemonic|crypto)/i
/(discord\.gg|webhook|hooks\.slack)/i
/(curl[^\n]+\|\s*(sh|bash))/i
/(bit\.ly|tinyurl\.com|t\.co|goo\.gl|is\.gd)/i
```

Example 3 (unknown):
```unknown
T-PERSIST-001 → T-EVADE-001 → T-EXFIL-003
(Publish malicious skill) → (Evade moderation) → (Harvest credentials)
```

Example 4 (unknown):
```unknown
T-EXEC-001 → T-EXEC-004 → T-IMPACT-001
(Inject prompt) → (Bypass exec approval) → (Execute commands)
```

---

## Operator scopes

**URL:** https://docs.openclaw.ai/gateway/operator-scopes

**Contents:**
- Operator scopes
- Documentation Index
- ​Roles
- ​Scope levels
- ​Method scope is only the first gate
- ​Device pairing approvals
- ​Node pairing approvals
- ​Shared-secret auth

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Contributing to the threat model

**URL:** https://docs.openclaw.ai/security/CONTRIBUTING-THREAT-MODEL

**Contents:**
- Contributing to the threat model
- Documentation Index
- ​Contributing to the OpenClaw Threat Model
- ​Ways to Contribute
  - ​Add a Threat
  - ​Suggest a Mitigation
  - ​Propose an Attack Chain
  - ​Fix or Improve Existing Content
- ​What we use
  - ​MITRE ATLAS

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Gateway runbook

**URL:** https://docs.openclaw.ai/gateway

**Contents:**
- Gateway runbook
- Documentation Index
- Deep troubleshooting
- Configuration
- Secrets management
- Secrets plan contract
- ​5-minute local startup
- ​Runtime model
- ​OpenAI-compatible endpoints
  - ​Port and bind precedence

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify service health

Validate channel readiness

**Examples:**

Example 1 (lua):
```lua
openclaw gateway --port 18789
# debug/trace mirrored to stdio
openclaw gateway --port 18789 --verbose
# force-kill listener on selected port, then start
openclaw gateway --force
```

Example 2 (unknown):
```unknown
openclaw gateway status
openclaw status
openclaw logs --follow
```

Example 3 (unknown):
```unknown
openclaw channels status --probe
```

Example 4 (unknown):
```unknown
openclaw gateway status
openclaw gateway status --deep   # adds a system-level service scan
openclaw gateway status --json
openclaw gateway install
openclaw gateway restart
openclaw gateway stop
openclaw secrets reload
openclaw logs --follow
openclaw doctor
```

---

## Multiple gateways

**URL:** https://docs.openclaw.ai/gateway/multiple-gateways

**Contents:**
- Multiple gateways
- Documentation Index
- ​Best recommended setup
- ​Rescue-Bot Quickstart
- ​Why this works
- ​What --profile rescue onboard Changes
- ​General multi-gateway setup
- ​Isolation checklist
- ​Port mapping (derived)
- ​Browser/CDP notes (common footgun)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
# Rescue bot (separate Telegram bot, separate profile, port 19789)
openclaw --profile rescue onboard
openclaw --profile rescue gateway install --port 19789
```

Example 2 (markdown):
```markdown
# main (default profile)
openclaw setup
openclaw gateway --port 18789

# extra gateway
openclaw --profile ops setup
openclaw --profile ops gateway --port 19789
```

Example 3 (unknown):
```unknown
openclaw --profile main setup
openclaw --profile main gateway --port 18789

openclaw --profile ops setup
openclaw --profile ops gateway --port 19789
```

Example 4 (unknown):
```unknown
openclaw gateway install
openclaw --profile ops gateway install --port 19789
```

---

## Security

**URL:** https://docs.openclaw.ai/cli/security

**Contents:**
- Security
- Documentation Index
- ​openclaw security
- ​Audit
- ​JSON output
- ​What --fix changes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw security audit
openclaw security audit --deep
openclaw security audit --deep --password <password>
openclaw security audit --deep --token <token>
openclaw security audit --fix
openclaw security audit --json
```

Example 2 (unknown):
```unknown
openclaw security audit --json | jq '.summary'
openclaw security audit --deep --json | jq '.findings[] | select(.severity=="critical") | .checkId'
```

Example 3 (css):
```css
openclaw security audit --fix --json | jq '{fix: .fix.ok, summary: .report.summary}'
```

---

## Logging

**URL:** https://docs.openclaw.ai/logging

**Contents:**
- Logging
- Documentation Index
- ​Where logs live
- ​How to read logs
  - ​CLI: live tail (recommended)
  - ​Control UI (web)
  - ​Channel-only logs
- ​Log formats
  - ​File logs (JSONL)
  - ​Console output

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "logging": {
    "file": "/path/to/openclaw.log"
  }
}
```

Example 2 (unknown):
```unknown
openclaw logs --follow
```

Example 3 (unknown):
```unknown
openclaw doctor
```

Example 4 (unknown):
```unknown
openclaw channels logs --channel whatsapp
```

---

## Configuration examples

**URL:** https://docs.openclaw.ai/gateway/configuration-examples

**Contents:**
- Configuration examples
- Documentation Index
- ​Quick start
  - ​Absolute minimum
  - ​Recommended starter
- ​Expanded example (major options)
- ​Common patterns
  - ​Shared skill baseline with one override
  - ​Multi-platform setup
  - ​Trusted node network auto-approval

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
{
  agent: { workspace: "~/.openclaw/workspace" },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

Example 2 (sass):
```sass
{
  identity: {
    name: "Clawd",
    theme: "helpful assistant",
    emoji: "🦞",
  },
  agent: {
    workspace: "~/.openclaw/workspace",
    model: { primary: "anthropic/claude-sonnet-4-6" },
  },
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: {
    visibleReplies: "automatic",
    groupChat: {
      visibleReplies: "message_tool", // default; use "automatic" for legacy room replies
    },
  },
}
```

Example 3 (sass):
```sass
{
  // Environment + shell
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: {
      GROQ_API_KEY: "gsk-...",
    },
    shellEnv: {
      enabled: true,
      timeoutMs: 15000,
    },
  },

  // Auth profile metadata (secrets live in auth-profiles.json)
  auth: {
    profiles: {
      "anthropic:default": { provider: "anthropic", mode: "api_key" },
      "anthropic:work": { provider: "anthropic", mode: "api_key" },
      "openai:default": { provider: "openai", mode: "api_key" },
      "openai-codex:personal": { provider: "openai-codex", mode: "oauth" },
    },
    order: {
      anthropic: ["anthropic:default", "anthropic:work"],
      openai: ["openai:default"],
      "openai-codex": ["openai-codex:personal"],
    },
  },

  // Identity
  identity: {
    name: "Samantha",
    theme: "helpful sloth",
    emoji: "🦥",
  },

  // Logging
  logging: {
    level: "info",
    file: "/tmp/openclaw/openclaw.log",
    consoleLevel: "info",
    consoleStyle: "pretty",
    redactSensitive: "tools",
  },

  // Message formatting
  messages: {
    messagePrefix: "[openclaw]",
    visibleReplies: "automatic",
    responsePrefix: ">",
    ackReaction: "👀",
    ackReactionScope: "group-mentions",
    groupChat: {
      historyLimit: 50,
      visibleReplies: "message_tool", // normal final replies stay private in groups/channels
    },
    queue: {
      mode: "steer",
      debounceMs: 500,
      cap: 20,
      drop: "summarize",
      byChannel: {
        whatsapp: "steer",
        telegram: "steer",
        discord: "steer",
        slack: "steer",
        signal: "steer",
        imessage: "steer",
        webchat: "steer",
      },
    },
  },

  // Tooling
  tools: {
    media: {
      audio: {
        enabled: true,
        maxBytes: 20971520,
        models: [
          { provider: "openai", model: "gpt-4o-mini-transcribe" },
          // Optional CLI fallback (Whisper binary):
          // { type: "cli", command: "whisper", args: ["--model", "base", "{{MediaPath}}"] }
        ],
        timeoutSeconds: 120,
      },
      video: {
        enabled: true,
        maxBytes: 52428800,
        models: [{ provider: "google", model: "gemini-3-flash-preview" }],
      },
    },
  },

  // Session behavior
  session: {
    scope: "per-sender",
    dmScope: "per-channel-peer", // recommended for multi-user inboxes
    reset: {
      mode: "daily",
      atHour: 4,
      idleMinutes: 60,
    },
    resetByChannel: {
      discord: { mode: "idle", idleMinutes: 10080 },
    },
    resetTriggers: ["/new", "/reset"],
    store: "~/.openclaw/agents/default/sessions/sessions.json",
    maintenance: {
      mode: "warn",
      pruneAfter: "30d",
      maxEntries: 500,
      resetArchiveRetention: "30d", // duration or false
      maxDiskBytes: "500mb", // optional
      highWaterBytes: "400mb", // optional (defaults to 80% of maxDiskBytes)
    },
    typingIntervalSeconds: 5,
    sendPolicy: {
      default: "allow",
      rules: [{ action: "deny", match: { channel: "discord", chatType: "group" } }],
    },
  },

  // Channels
  channels: {
    whatsapp: {
      dmPolicy: "pairing",
      allowFrom: ["+15555550123"],
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },

    telegram: {
      enabled: true,
      botToken: "YOUR_TELEGRAM_BOT_TOKEN",
      allowFrom: ["123456789"],
      groupPolicy: "allowlist",
      groupAllowFrom: ["123456789"],
      groups: { "*": { requireMention: true } },
    },

    discord: {
      enabled: true,
      token: "YOUR_DISCORD_BOT_TOKEN",
      dm: { enabled: true, allowFrom: ["123456789012345678"] },
      guilds: {
        "123456789012345678": {
          slug: "friends-of-openclaw",
          requireMention: false,
          channels: {
            general: { allow: true },
            help: { allow: true, requireMention: true },
          },
        },
      },
    },

    slack: {
      enabled: true,
      botToken: "xoxb-REPLACE_ME",
      appToken: "xapp-REPLACE_ME",
      channels: {
        "#general": { allow: true, requireMention: true },
      },
      dm: { enabled: true, allowFrom: ["U123"] },
      slashCommand: {
        enabled: true,
        name: "openclaw",
        sessionPrefix: "slack:slash",
        ephemeral: true,
      },
    },
  },

  // Agent runtime
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      userTimezone: "America/Chicago",
      model: {
        primary: "anthropic/claude-sonnet-4-6",
        fallbacks: ["anthropic/claude-opus-4-6", "openai/gpt-5.4"],
      },
      imageModel: {
        primary: "openrouter/anthropic/claude-sonnet-4-6",
      },
      models: {
        "anthropic/claude-opus-4-6": { alias: "opus" },
        "anthropic/claude-sonnet-4-6": { alias: "sonnet" },
        "openai/gpt-5.4": { alias: "gpt" },
      },
      skills: ["github", "weather"], // inherited by agents that omit list[].skills
      thinkingDefault: "low",
      verboseDefault: "off",
      reasoningDefault: "off",
      elevatedDefault: "on",
      blockStreamingDefault: "off",
      blockStreamingBreak: "text_end",
      blockStreamingChunk: {
        minChars: 800,
        maxChars: 1200,
        breakPreference: "paragraph",
      },
      blockStreamingCoalesce: {
        idleMs: 1000,
      },
      humanDelay: {
        mode: "natural",
      },
      timeoutSeconds: 600,
      mediaMaxMb: 5,
      typingIntervalSeconds: 5,
      maxConcurrent: 3,
      heartbeat: {
        every: "30m",
        model: "anthropic/claude-sonnet-4-6",
        target: "last",
        directPolicy: "allow", // allow (default) | block
        to: "+15555550123",
        prompt: "HEARTBEAT",
        ackMaxChars: 300,
      },
      memorySearch: {
        provider: "gemini",
        model: "gemini-embedding-001",
        remote: {
          apiKey: "${GEMINI_API_KEY}",
        },
        extraPaths: ["../team-docs", "/srv/shared-notes"],
      },
      sandbox: {
        mode: "non-main",
        scope: "session", // preferred over legacy perSession: true
        workspaceRoot: "~/.openclaw/sandboxes",
        docker: {
          image: "openclaw-sandbox:bookworm-slim",
          workdir: "/workspace",
          readOnlyRoot: true,
          tmpfs: ["/tmp", "/var/tmp", "/run"],
          network: "none",
          user: "1000:1000",
        },
        browser: {
          enabled: false,
        },
      },
    },
    list: [
      {
        id: "main",
        default: true,
        // inherits defaults.skills -> github, weather
        groupChat: {
          mentionPatterns: ["@openclaw", "openclaw"],
        },
        thinkingDefault: "high", // per-agent thinking override
        reasoningDefault: "on", // per-agent reasoning visibility
        fastModeDefault: false, // per-agent fast mode
      },
      {
        id: "quick",
        skills: [], // no skills for this agent
        fastModeDefault: true, // this agent always runs fast
        thinkingDefault: "off",
      },
    ],
  },

  tools: {
    allow: ["exec", "process", "read", "write", "edit", "apply_patch"],
    deny: ["browser", "canvas"],
    exec: {
      backgroundMs: 10000,
      timeoutSec: 1800,
      cleanupMs: 1800000,
    },
    elevated: {
      enabled: true,
      allowFrom: {
        whatsapp: ["+15555550123"],
        telegram: ["123456789"],
        discord: ["123456789012345678"],
        slack: ["U123"],
        signal: ["+15555550123"],
        imessage: ["user@example.com"],
        webchat: ["session:demo"],
      },
    },
  },

  // Custom model providers
  models: {
    mode: "merge",
    providers: {
      "custom-proxy": {
        baseUrl: "http://localhost:4000/v1",
        apiKey: "LITELLM_KEY",
        api: "openai-responses",
        authHeader: true,
        headers: { "X-Proxy-Region": "us-west" },
        models: [
          {
            id: "llama-3.1-8b",
            name: "Llama 3.1 8B",
            api: "openai-responses",
            reasoning: false,
            input: ["text"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 128000,
            maxTokens: 32000,
          },
        ],
      },
    },
  },

  // Cron jobs
  cron: {
    enabled: true,
    store: "~/.openclaw/cron/cron.json",
    maxConcurrentRuns: 2, // cron dispatch + isolated cron agent-turn execution
    sessionRetention: "24h",
    runLog: {
      maxBytes: "2mb",
      keepLines: 2000,
    },
  },

  // Webhooks
  hooks: {
    enabled: true,
    path: "/hooks",
    token: "shared-secret",
    presets: ["gmail"],
    transformsDir: "~/.openclaw/hooks/transforms",
    mappings: [
      {
        id: "gmail-hook",
        match: { path: "gmail" },
        action: "agent",
        wakeMode: "now",
        name: "Gmail",
        sessionKey: "hook:gmail:{{messages[0].id}}",
        messageTemplate: "From: {{messages[0].from}}\nSubject: {{messages[0].subject}}",
        textTemplate: "{{messages[0].snippet}}",
        deliver: true,
        channel: "last",
        to: "+15555550123",
        thinking: "low",
        timeoutSeconds: 300,
        transform: {
          module: "gmail.js",
          export: "transformGmail",
        },
      },
    ],
    gmail: {
      account: "openclaw@gmail.com",
      label: "INBOX",
      topic: "projects/<project-id>/topics/gog-gmail-watch",
      subscription: "gog-gmail-watch-push",
      pushToken: "shared-push-token",
      hookUrl: "http://127.0.0.1:18789/hooks/gmail",
      includeBody: true,
      maxBytes: 20000,
      renewEveryMinutes: 720,
      serve: { bind: "127.0.0.1", port: 8788, path: "/" },
      tailscale: { mode: "funnel", path: "/gmail-pubsub" },
    },
  },

  // Gateway + networking
  gateway: {
    mode: "local",
    port: 18789,
    bind: "loopback",
    controlUi: { enabled: true, basePath: "/openclaw" },
    auth: {
      mode: "token",
      token: "gateway-token",
      allowTailscale: true,
    },
    tailscale: { mode: "serve", resetOnExit: false },
    remote: { url: "ws://gateway.tailnet:18789", token: "remote-token" },
    reload: { mode: "hybrid", debounceMs: 300 },
  },

  skills: {
    allowBundled: ["gemini", "peekaboo"],
    load: {
      extraDirs: ["~/Projects/agent-scripts/skills"],
    },
    install: {
      preferBrew: true,
      nodeManager: "npm", // npm | pnpm | yarn | bun
    },
    entries: {
      "image-lab": {
        enabled: true,
        apiKey: "GEMINI_KEY_HERE",
        env: { GEMINI_API_KEY: "GEMINI_KEY_HERE" },
      },
      peekaboo: { enabled: true },
    },
  },
}
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
      skills: ["github", "weather"],
    },
    list: [
      { id: "main", default: true },
      { id: "docs", workspace: "~/.openclaw/workspace-docs", skills: ["docs-search"] },
    ],
  },
}
```

---

## Heartbeat

**URL:** https://docs.openclaw.ai/gateway/heartbeat

**Contents:**
- Heartbeat
- Documentation Index
- ​Quick start (beginner)
- ​Defaults
- ​What the heartbeat prompt is for
- ​Response contract
- ​Config
  - ​Scope and precedence
  - ​Per-agent heartbeats
  - ​Active hours example

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Add HEARTBEAT.md (optional)

Decide where heartbeat messages should go

Session and target routing

Visibility and skip behavior

Session lifecycle and audit

**Examples:**

Example 1 (json):
```json
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last", // explicit delivery to last contact (default is "none")
        directPolicy: "allow", // default: allow direct/DM targets; set "block" to suppress
        lightContext: true, // optional: only inject HEARTBEAT.md from bootstrap files
        isolatedSession: true, // optional: fresh session each run (no conversation history)
        skipWhenBusy: true, // optional: also defer when subagent or nested lanes are busy
        // activeHours: { start: "08:00", end: "24:00" },
        // includeReasoning: true, // optional: send separate `Reasoning:` message too
      },
    },
  },
}
```

Example 2 (sass):
```sass
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m", // default: 30m (0m disables)
        model: "anthropic/claude-opus-4-6",
        includeReasoning: false, // default: false (deliver separate Reasoning: message when available)
        lightContext: false, // default: false; true keeps only HEARTBEAT.md from workspace bootstrap files
        isolatedSession: false, // default: false; true runs each heartbeat in a fresh session (no conversation history)
        skipWhenBusy: false, // default: false; true also waits for subagent/nested lanes
        target: "last", // default: none | options: last | none | <channel id> (core or plugin, e.g. "bluebubbles")
        to: "+15551234567", // optional channel-specific override
        accountId: "ops-bot", // optional multi-account channel id
        prompt: "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.",
        ackMaxChars: 300, // max chars allowed after HEARTBEAT_OK
      },
    },
  },
}
```

Example 3 (sass):
```sass
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last", // explicit delivery to last contact (default is "none")
      },
    },
    list: [
      { id: "main", default: true },
      {
        id: "ops",
        heartbeat: {
          every: "1h",
          target: "whatsapp",
          to: "+15551234567",
          timeoutSeconds: 45,
          prompt: "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.",
        },
      },
    ],
  },
}
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last", // explicit delivery to last contact (default is "none")
        activeHours: {
          start: "09:00",
          end: "22:00",
          timezone: "America/New_York", // optional; uses your userTimezone if set, otherwise host tz
        },
      },
    },
  },
}
```

---

## OpenTelemetry export

**URL:** https://docs.openclaw.ai/gateway/opentelemetry

**Contents:**
- OpenTelemetry export
- Documentation Index
- ​How it fits together
- ​Quick start
- ​Signals exported
- ​Configuration reference
  - ​Environment variables
- ​Privacy and content capture
- ​Sampling and flushing
- ​Exported metrics

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install clawhub:@openclaw/diagnostics-otel
```

Example 2 (json):
```json
{
  plugins: {
    allow: ["diagnostics-otel"],
    entries: {
      "diagnostics-otel": { enabled: true },
    },
  },
  diagnostics: {
    enabled: true,
    otel: {
      enabled: true,
      endpoint: "http://otel-collector:4318",
      protocol: "http/protobuf",
      serviceName: "openclaw-gateway",
      traces: true,
      metrics: true,
      logs: true,
      sampleRate: 0.2,
      flushIntervalMs: 60000,
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw plugins enable diagnostics-otel
```

Example 4 (lua):
```lua
{
  diagnostics: {
    enabled: true,
    otel: {
      enabled: true,
      endpoint: "http://otel-collector:4318",
      tracesEndpoint: "http://otel-collector:4318/v1/traces",
      metricsEndpoint: "http://otel-collector:4318/v1/metrics",
      logsEndpoint: "http://otel-collector:4318/v1/logs",
      protocol: "http/protobuf", // grpc is ignored
      serviceName: "openclaw-gateway",
      headers: { "x-collector-token": "..." },
      traces: true,
      metrics: true,
      logs: true,
      sampleRate: 0.2, // root-span sampler, 0.0..1.0
      flushIntervalMs: 60000, // metric export interval (min 1000ms)
      captureContent: {
        enabled: false,
        inputMessages: false,
        outputMessages: false,
        toolInputs: false,
        toolOutputs: false,
        systemPrompt: false,
      },
    },
  },
}
```

---

## Diagnostics export

**URL:** https://docs.openclaw.ai/gateway/diagnostics

**Contents:**
- Diagnostics export
- Documentation Index
- ​Quick start
- ​Chat command
- ​What the export contains
- ​Privacy model
- ​Stability recorder
- ​Useful options
- ​Disable diagnostics
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw gateway diagnostics export
```

Example 2 (unknown):
```unknown
openclaw gateway diagnostics export --output openclaw-diagnostics.zip
```

Example 3 (unknown):
```unknown
openclaw gateway diagnostics export --json
```

Example 4 (unknown):
```unknown
openclaw gateway stability
openclaw gateway stability --type payload.large
openclaw gateway stability --json
```

---

## Network proxy

**URL:** https://docs.openclaw.ai/security/network-proxy

**Contents:**
- Network proxy
- Documentation Index
- ​Network Proxy
- ​Why Use a Proxy?
- ​How OpenClaw Routes Traffic
- ​Related Proxy Terms
- ​Configuration
- ​Proxy Requirements
- ​Recommended Blocked Destinations
- ​Validation

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (perl):
```perl
OpenClaw process
  fetch                  -> operator-managed filtering proxy -> public internet
  node:http and https    -> operator-managed filtering proxy -> public internet
  WebSocket clients      -> operator-managed filtering proxy -> public internet
```

Example 2 (yaml):
```yaml
proxy:
  enabled: true
  proxyUrl: http://127.0.0.1:3128
```

Example 3 (sass):
```sass
OPENCLAW_PROXY_URL=http://127.0.0.1:3128 openclaw gateway run
```

Example 4 (json):
```json
openclaw config set proxy.enabled true
openclaw config set proxy.proxyUrl http://127.0.0.1:3128
openclaw gateway install --force
openclaw gateway start
```

---

## CLI backends

**URL:** https://docs.openclaw.ai/gateway/cli-backends

**Contents:**
- CLI backends
- Documentation Index
- ​Beginner-friendly quick start
- ​Using it as a fallback
- ​Configuration overview
  - ​Example configuration
- ​How it works
- ​Sessions
- ​Fallback prelude from claude-cli sessions
- ​Images (pass-through)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw agent --message "hi" --model codex-cli/gpt-5.5
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      cliBackends: {
        "codex-cli": {
          command: "/opt/homebrew/bin/codex",
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
      model: {
        primary: "anthropic/claude-opus-4-6",
        fallbacks: ["codex-cli/gpt-5.5"],
      },
      models: {
        "anthropic/claude-opus-4-6": { alias: "Opus" },
        "codex-cli/gpt-5.5": {},
      },
    },
  },
}
```

Example 4 (unknown):
```unknown
agents.defaults.cliBackends
```

---

## Status

**URL:** https://docs.openclaw.ai/cli/status

**Contents:**
- Status
- Documentation Index
- ​openclaw status
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw status
openclaw status --all
openclaw status --deep
openclaw status --usage
```

---

## Diagnostics flags

**URL:** https://docs.openclaw.ai/diagnostics/flags

**Contents:**
- Diagnostics flags
- Documentation Index
- ​How it works
- ​Enable via config
- ​Env override (one-off)
- ​Timeline artifacts
- ​Where logs go
- ​Extract logs
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "diagnostics": {
    "flags": ["telegram.http"]
  }
}
```

Example 2 (json):
```json
{
  "diagnostics": {
    "flags": ["telegram.http", "brave.http", "gateway.*"]
  }
}
```

Example 3 (sass):
```sass
OPENCLAW_DIAGNOSTICS=telegram.http,telegram.payload
```

Example 4 (sass):
```sass
OPENCLAW_DIAGNOSTICS=0
```

---

## Gateway lock

**URL:** https://docs.openclaw.ai/gateway/gateway-lock

**Contents:**
- Gateway lock
- Documentation Index
- ​Why
- ​Mechanism
- ​Error surface
- ​Operational notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Bridge protocol

**URL:** https://docs.openclaw.ai/gateway/bridge-protocol

**Contents:**
- Bridge protocol
- Documentation Index
- ​Why it existed
- ​Transport
- ​Handshake + pairing
- ​Frames
- ​Exec lifecycle events
- ​Historical tailnet usage
- ​Versioning
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Gateway

**URL:** https://docs.openclaw.ai/cli/gateway

**Contents:**
- Gateway
- Documentation Index
- Bonjour discovery
- Discovery overview
- Configuration
- ​Run the Gateway
  - ​Options
  - ​Startup profiling
- ​Query a running Gateway
  - ​gateway health

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Privacy and bundle behavior

Linux systemd auth-drift checks

Auth and SecretRefs at install time

**Examples:**

Example 1 (unknown):
```unknown
openclaw gateway
```

Example 2 (unknown):
```unknown
openclaw gateway run
```

Example 3 (json):
```json
openclaw gateway health --url ws://127.0.0.1:18789
```

Example 4 (unknown):
```unknown
openclaw gateway usage-cost
openclaw gateway usage-cost --days 7
openclaw gateway usage-cost --json
```

---

## Security audit checks

**URL:** https://docs.openclaw.ai/gateway/security/audit-checks

**Contents:**
- Security audit checks
- Documentation Index
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Secrets apply plan contract

**URL:** https://docs.openclaw.ai/gateway/secrets-plan-contract

**Contents:**
- Secrets apply plan contract
- Documentation Index
- ​Plan file shape
- ​Supported target scope
- ​Target type behavior
- ​Path validation rules
- ​Failure behavior
- ​Exec provider consent behavior
- ​Runtime and audit scope notes
- ​Operator checks

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  version: 1,
  protocolVersion: 1,
  targets: [
    {
      type: "models.providers.apiKey",
      path: "models.providers.openai.apiKey",
      pathSegments: ["models", "providers", "openai", "apiKey"],
      providerId: "openai",
      ref: { source: "env", provider: "default", id: "OPENAI_API_KEY" },
    },
    {
      type: "auth-profiles.api_key.key",
      path: "profiles.openai:default.key",
      pathSegments: ["profiles", "openai:default", "key"],
      agentId: "main",
      ref: { source: "env", provider: "default", id: "OPENAI_API_KEY" },
    },
  ],
}
```

Example 2 (unknown):
```unknown
Invalid plan target path for models.providers.apiKey: models.providers.openai.baseUrl
```

Example 3 (sql):
```sql
# Validate plan without writes
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run

# Then apply for real
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json

# For exec-containing plans, opt in explicitly in both modes
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run --allow-exec
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --allow-exec
```

---

## Prometheus metrics

**URL:** https://docs.openclaw.ai/gateway/prometheus

**Contents:**
- Prometheus metrics
- Documentation Index
- ​Quick start
- ​Metrics exported
- ​Label policy
- ​PromQL recipes
- ​Choosing between Prometheus and OpenTelemetry export
- ​Troubleshooting
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Scrape the protected route

Bounded, low-cardinality labels

Series cap and overflow accounting

What never appears in Prometheus output

`openclaw_prometheus_series_dropped_total` is climbing

Prometheus shows stale series after a restart

**Examples:**

Example 1 (unknown):
```unknown
GET /api/diagnostics/prometheus
```

Example 2 (elixir):
```elixir
openclaw plugins install clawhub:@openclaw/diagnostics-prometheus
```

Example 3 (json):
```json
{
  plugins: {
    allow: ["diagnostics-prometheus"],
    entries: {
      "diagnostics-prometheus": { enabled: true },
    },
  },
  diagnostics: {
    enabled: true,
  },
}
```

Example 4 (unknown):
```unknown
openclaw plugins enable diagnostics-prometheus
```

---

## Remote access

**URL:** https://docs.openclaw.ai/gateway/remote

**Contents:**
- Remote access
- Documentation Index
- ​The core idea
- ​Common VPN and tailnet setups
  - ​Always-on Gateway in your tailnet
  - ​Home desktop runs the Gateway
  - ​Laptop runs the Gateway
- ​Command flow (what runs where)
- ​SSH tunnel (CLI + tools)
- ​CLI remote defaults

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
ssh -N -L 18789:127.0.0.1:18789 user@host
```

Example 2 (json):
```json
{
  gateway: {
    mode: "remote",
    remote: {
      url: "ws://127.0.0.1:18789",
      token: "your-token",
    },
  },
}
```

Example 3 (typescript):
```typescript
Host remote-gateway
    HostName <REMOTE_IP>
    User <REMOTE_USER>
    LocalForward 18789 127.0.0.1:18789
    IdentityFile ~/.ssh/id_rsa
```

Example 4 (typescript):
```typescript
ssh-copy-id -i ~/.ssh/id_rsa <REMOTE_USER>@<REMOTE_IP>
```

---

## Security

**URL:** https://docs.openclaw.ai/gateway/security

**Contents:**
- Security
- Documentation Index
- ​Scope first: personal assistant security model
- ​Quick check: openclaw security audit
  - ​Deployment and host trust
  - ​Shared Slack workspace: real risk
  - ​Company-shared agent: acceptable pattern
- ​Gateway and node trust concept
- ​Trust boundary matrix
- ​Not vulnerabilities by design

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Common findings that are out of scope

Flags tracked by the audit today

All `dangerous*` / `dangerously*` keys in the config schema

**Examples:**

Example 1 (unknown):
```unknown
openclaw security audit
openclaw security audit --deep
openclaw security audit --fix
openclaw security audit --json
```

Example 2 (json):
```json
{
  gateway: {
    mode: "local",
    bind: "loopback",
    auth: { mode: "token", token: "replace-with-long-random-token" },
  },
  session: {
    dmScope: "per-channel-peer",
  },
  tools: {
    profile: "messaging",
    deny: ["group:automation", "group:runtime", "group:fs", "sessions_spawn", "sessions_send"],
    fs: { workspaceOnly: true },
    exec: { security: "deny", ask: "always" },
    elevated: { enabled: false },
  },
  channels: {
    whatsapp: { dmPolicy: "pairing", groups: { "*": { requireMention: true } } },
  },
}
```

Example 3 (yaml):
```yaml
gateway:
  trustedProxies:
    - "10.0.0.1" # reverse proxy IP
  # Optional. Default false.
  # Only enable if your proxy cannot provide X-Forwarded-For.
  allowRealIpFallback: false
  auth:
    mode: password
    password: ${OPENCLAW_GATEWAY_PASSWORD}
```

Example 4 (bash):
```bash
proxy_set_header X-Forwarded-For $remote_addr;
proxy_set_header X-Real-IP $remote_addr;
```

---

## Gateway logging

**URL:** https://docs.openclaw.ai/gateway/logging

**Contents:**
- Gateway logging
- Documentation Index
- ​Logging
- ​File-based logger
- ​Console capture
- ​Redaction
- ​Gateway WebSocket logs
  - ​WS log style
- ​Console formatting (subsystem logging)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw logs --follow
```

Example 2 (markdown):
```markdown
# optimized (only errors/slow)
openclaw gateway

# show all WS traffic (paired)
openclaw gateway --verbose --ws-log compact

# show all WS traffic (full meta)
openclaw gateway --verbose --ws-log full
```

---

## Sandbox vs tool policy vs elevated

**URL:** https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated

**Contents:**
- Sandbox vs tool policy vs elevated
- Documentation Index
- ​Quick debug
- ​Sandbox: where tools run
  - ​Bind mounts (security quick check)
- ​Tool policy: which tools exist/are callable
  - ​Tool groups (shorthands)
- ​Elevated: exec-only “run on host”
- ​Common “sandbox jail” fixes
  - ​”Tool X blocked by sandbox tool policy”

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw sandbox explain
openclaw sandbox explain --session agent:main:main
openclaw sandbox explain --agent work
openclaw sandbox explain --json
```

Example 2 (json):
```json
{
  tools: {
    sandbox: {
      tools: {
        allow: ["group:runtime", "group:fs", "group:sessions", "group:memory"],
      },
    },
  },
}
```

---

## Troubleshooting

**URL:** https://docs.openclaw.ai/gateway/troubleshooting

**Contents:**
- Troubleshooting
- Documentation Index
- ​Command ladder
- ​Split brain installs and newer config guard
- ​Anthropic 429 extra usage required for long context
- ​Local OpenAI-compatible backend passes direct probes but agent runs fail
- ​No replies
- ​Dashboard control UI connectivity
  - ​Auth detail codes quick map
- ​Gateway service not running

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Reinstall the gateway service

Remove stale wrappers

Use an eligible credential

Configure fallback models

Connect / auth signatures

Wait for connect.challenge

Send the device nonce

Plugin / executable signatures

Chrome MCP / existing-session signatures

Element / screenshot / upload signatures

1. Auth and URL override behavior changed

2. Bind and auth guardrails are stricter

3. Pairing and device identity state changed

**Examples:**

Example 1 (unknown):
```unknown
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

Example 2 (unknown):
```unknown
which openclaw
openclaw --version
openclaw gateway status --deep
openclaw config get meta.lastTouchedVersion
```

Example 3 (unknown):
```unknown
openclaw gateway install --force
openclaw gateway restart
```

Example 4 (unknown):
```unknown
openclaw logs --follow
openclaw models status
openclaw config get agents.defaults.models
```

---

## Bonjour discovery

**URL:** https://docs.openclaw.ai/gateway/bonjour

**Contents:**
- Bonjour discovery
- Documentation Index
- ​Bonjour / mDNS discovery
- ​Wide-area Bonjour (Unicast DNS-SD) over Tailscale
  - ​Gateway config (recommended)
  - ​One-time DNS server setup (gateway host)
  - ​Tailscale DNS settings
  - ​Gateway listener security (recommended)
- ​What advertises
- ​Service types

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  gateway: { bind: "tailnet" }, // tailnet-only (recommended)
  discovery: { wideArea: { enabled: true } }, // enables wide-area DNS-SD publishing
}
```

Example 2 (unknown):
```unknown
openclaw dns setup --apply
```

Example 3 (sass):
```sass
dns-sd -B _openclaw-gw._tcp openclaw.internal.
dig @<TAILNET_IPV4> -p 53 _openclaw-gw._tcp.openclaw.internal PTR +short
```

Example 4 (unknown):
```unknown
dns-sd -B _openclaw-gw._tcp local.
```

---

## Doctor

**URL:** https://docs.openclaw.ai/gateway/doctor

**Contents:**
- Doctor
- Documentation Index
- ​Quick start
  - ​Headless and automation modes
- ​What it does (summary)
- ​Dreams UI backfill and reset
- ​Detailed behavior and rationale
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Health, UI, and updates

Config and migrations

Gateway, services, and supervisors

Auth, security, and pairing

0. Optional update (git installs)

1. Config normalization

2. Legacy config key migrations

2b. OpenCode provider overrides

2c. Browser migration and Chrome MCP readiness

2d. OAuth TLS prerequisites

2e. Codex OAuth provider overrides

2f. Codex plugin route warnings

3. Legacy state migrations (disk layout)

3a. Legacy plugin manifest migrations

3b. Legacy cron store migrations

3c. Session lock cleanup

3d. Session transcript branch repair

4. State integrity checks (session persistence, routing, and safety)

5. Model auth health (OAuth expiry)

6. Hooks model validation

7. Sandbox image repair

7b. Plugin install cleanup

8. Gateway service migrations and cleanup hints

8b. Startup Matrix migration

8c. Device pairing and auth drift

10. systemd linger (Linux)

11. Workspace status (skills, plugins, and legacy dirs)

11b. Bootstrap file size

11d. Stale channel plugin cleanup

11c. Shell completion

12. Gateway auth checks (local token)

12b. Read-only SecretRef-aware repairs

13. Gateway health check + restart

13b. Memory search readiness

14. Channel status warnings

15. Supervisor config audit + repair

16. Gateway runtime + port diagnostics

17. Gateway runtime best practices

18. Config write + wizard metadata

19. Workspace tips (backup + memory system)

**Examples:**

Example 1 (unknown):
```unknown
openclaw doctor
```

Example 2 (unknown):
```unknown
openclaw doctor --yes
```

Example 3 (unknown):
```unknown
openclaw doctor --repair
```

Example 4 (unknown):
```unknown
openclaw doctor --repair --force
```

---

## Doctor

**URL:** https://docs.openclaw.ai/cli/doctor

**Contents:**
- Doctor
- Documentation Index
- ​openclaw doctor
- ​Examples
- ​Options
- ​macOS: launchctl env overrides
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw doctor
openclaw doctor --repair
openclaw doctor --deep
openclaw doctor --repair --non-interactive
openclaw doctor --generate-gateway-token
```

Example 2 (unknown):
```unknown
launchctl getenv OPENCLAW_GATEWAY_TOKEN
launchctl getenv OPENCLAW_GATEWAY_PASSWORD

launchctl unsetenv OPENCLAW_GATEWAY_TOKEN
launchctl unsetenv OPENCLAW_GATEWAY_PASSWORD
```

---

## Security

**URL:** https://docs.openclaw.ai/gateway/security/index

**Contents:**
- Security
- Documentation Index
- ​Scope first: personal assistant security model
- ​Quick check: openclaw security audit
  - ​Deployment and host trust
  - ​Shared Slack workspace: real risk
  - ​Company-shared agent: acceptable pattern
- ​Gateway and node trust concept
- ​Trust boundary matrix
- ​Not vulnerabilities by design

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Common findings that are out of scope

Flags tracked by the audit today

All `dangerous*` / `dangerously*` keys in the config schema

**Examples:**

Example 1 (unknown):
```unknown
openclaw security audit
openclaw security audit --deep
openclaw security audit --fix
openclaw security audit --json
```

Example 2 (json):
```json
{
  gateway: {
    mode: "local",
    bind: "loopback",
    auth: { mode: "token", token: "replace-with-long-random-token" },
  },
  session: {
    dmScope: "per-channel-peer",
  },
  tools: {
    profile: "messaging",
    deny: ["group:automation", "group:runtime", "group:fs", "sessions_spawn", "sessions_send"],
    fs: { workspaceOnly: true },
    exec: { security: "deny", ask: "always" },
    elevated: { enabled: false },
  },
  channels: {
    whatsapp: { dmPolicy: "pairing", groups: { "*": { requireMention: true } } },
  },
}
```

Example 3 (yaml):
```yaml
gateway:
  trustedProxies:
    - "10.0.0.1" # reverse proxy IP
  # Optional. Default false.
  # Only enable if your proxy cannot provide X-Forwarded-For.
  allowRealIpFallback: false
  auth:
    mode: password
    password: ${OPENCLAW_GATEWAY_PASSWORD}
```

Example 4 (bash):
```bash
proxy_set_header X-Forwarded-For $remote_addr;
proxy_set_header X-Real-IP $remote_addr;
```

---
