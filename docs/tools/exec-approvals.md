---
source_url: https://docs.openclaw.ai/tools/exec-approvals
title: "Exec approvals - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/exec-approvals#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Tools

Exec approvals

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Inspecting the effective policy](https://docs.openclaw.ai/tools/exec-approvals#inspecting-the-effective-policy)
- [Where it applies](https://docs.openclaw.ai/tools/exec-approvals#where-it-applies)
- [Trust model](https://docs.openclaw.ai/tools/exec-approvals#trust-model)
- [macOS split](https://docs.openclaw.ai/tools/exec-approvals#macos-split)
- [Settings and storage](https://docs.openclaw.ai/tools/exec-approvals#settings-and-storage)
- [Policy knobs](https://docs.openclaw.ai/tools/exec-approvals#policy-knobs)
- [exec.security](https://docs.openclaw.ai/tools/exec-approvals#exec-security)
- [exec.ask](https://docs.openclaw.ai/tools/exec-approvals#exec-ask)
- [askFallback](https://docs.openclaw.ai/tools/exec-approvals#askfallback)
- [tools.exec.strictInlineEval](https://docs.openclaw.ai/tools/exec-approvals#tools-exec-strictinlineeval)
- [YOLO mode (no-approval)](https://docs.openclaw.ai/tools/exec-approvals#yolo-mode-no-approval)
- [Persistent gateway-host “never prompt” setup](https://docs.openclaw.ai/tools/exec-approvals#persistent-gateway-host-%E2%80%9Cnever-prompt%E2%80%9D-setup)
- [Local shortcut](https://docs.openclaw.ai/tools/exec-approvals#local-shortcut)
- [Node host](https://docs.openclaw.ai/tools/exec-approvals#node-host)
- [Session-only shortcut](https://docs.openclaw.ai/tools/exec-approvals#session-only-shortcut)
- [Allowlist (per agent)](https://docs.openclaw.ai/tools/exec-approvals#allowlist-per-agent)
- [Auto-allow skill CLIs](https://docs.openclaw.ai/tools/exec-approvals#auto-allow-skill-clis)
- [Safe bins and approval forwarding](https://docs.openclaw.ai/tools/exec-approvals#safe-bins-and-approval-forwarding)
- [Control UI editing](https://docs.openclaw.ai/tools/exec-approvals#control-ui-editing)
- [Approval flow](https://docs.openclaw.ai/tools/exec-approvals#approval-flow)
- [System events](https://docs.openclaw.ai/tools/exec-approvals#system-events)
- [Denied approval behavior](https://docs.openclaw.ai/tools/exec-approvals#denied-approval-behavior)
- [Implications](https://docs.openclaw.ai/tools/exec-approvals#implications)
- [Related](https://docs.openclaw.ai/tools/exec-approvals#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Exec approvals are the **companion app / node host guardrail** for letting
a sandboxed agent run commands on a real host (`gateway` or `node`). A
safety interlock: commands are allowed only when policy + allowlist +
(optional) user approval all agree. Exec approvals stack **on top of**
tool policy and elevated gating (unless elevated is set to `full`, which
skips approvals).

Effective policy is the **stricter** of `tools.exec.*` and approvals
defaults; if an approvals field is omitted, the `tools.exec` value is
used. Host exec also uses local approvals state on that machine — a
host-local `ask: "always"` in `~/.openclaw/exec-approvals.json` keeps
prompting even if session or config defaults request `ask: "on-miss"`.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#inspecting-the-effective-policy)  Inspecting the effective policy

| Command | What it shows |
| --- | --- |
| `openclaw approvals get` / `--gateway` / `--node <id|name|ip>` | Requested policy, host policy sources, and the effective result. |
| `openclaw exec-policy show` | Local-machine merged view. |
| `openclaw exec-policy set` / `preset` | Synchronize the local requested policy with the local host approvals file in one step. |

When a local scope requests `host=node`, `exec-policy show` reports that
scope as node-managed at runtime instead of pretending the local
approvals file is the source of truth.If the companion app UI is **not available**, any request that would
normally prompt is resolved by the **ask fallback** (default: `deny`).

Native chat approval clients can seed channel-specific affordances on the
pending approval message. For example, Matrix seeds reaction shortcuts
(`✅` allow once, `❌` deny, `♾️` allow always) while still leaving
`/approve ...` commands in the message as a fallback.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#where-it-applies)  Where it applies

Exec approvals are enforced locally on the execution host:

- **Gateway host** → `openclaw` process on the gateway machine.
- **Node host** → node runner (macOS companion app or headless node host).

### [​](https://docs.openclaw.ai/tools/exec-approvals\#trust-model)  Trust model

- Gateway-authenticated callers are trusted operators for that Gateway.
- Paired nodes extend that trusted operator capability onto the node host.
- Exec approvals reduce accidental execution risk, but are **not** a per-user auth boundary.
- Approved node-host runs bind canonical execution context: canonical cwd, exact argv, env binding when present, and pinned executable path when applicable.
- For shell scripts and direct interpreter/runtime file invocations, OpenClaw also tries to bind one concrete local file operand. If that bound file changes after approval but before execution, the run is denied instead of executing drifted content.
- File binding is intentionally best-effort, **not** a complete semantic model of every interpreter/runtime loader path. If approval mode cannot identify exactly one concrete local file to bind, it refuses to mint an approval-backed run instead of pretending full coverage.

### [​](https://docs.openclaw.ai/tools/exec-approvals\#macos-split)  macOS split

- The **node host service** forwards `system.run` to the **macOS app** over local IPC.
- The **macOS app** enforces approvals and executes the command in UI context.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#settings-and-storage)  Settings and storage

Approvals live in a local JSON file on the execution host:

```
~/.openclaw/exec-approvals.json
```

Example schema:

```
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
      "allowlist": [\
        {\
          "id": "B0C8C0B3-2C2D-4F8A-9A3C-5A4B3C2D1E0F",\
          "pattern": "~/Projects/**/bin/rg",\
          "source": "allow-always",\
          "commandText": "rg -n TODO",\
          "lastUsedAt": 1737150000000,\
          "lastUsedCommand": "rg -n TODO",\
          "lastResolvedPath": "/Users/user/Projects/.../bin/rg"\
        }\
      ]
    }
  }
}
```

## [​](https://docs.openclaw.ai/tools/exec-approvals\#policy-knobs)  Policy knobs

### [​](https://docs.openclaw.ai/tools/exec-approvals\#exec-security)  `exec.security`

[​](https://docs.openclaw.ai/tools/exec-approvals#param-security)

security

"deny" \| "allowlist" \| "full"

- `deny` — block all host exec requests.
- `allowlist` — allow only allowlisted commands.
- `full` — allow everything (equivalent to elevated).

### [​](https://docs.openclaw.ai/tools/exec-approvals\#exec-ask)  `exec.ask`

[​](https://docs.openclaw.ai/tools/exec-approvals#param-ask)

ask

"off" \| "on-miss" \| "always"

- `off` — never prompt.
- `on-miss` — prompt only when the allowlist does not match.
- `always` — prompt on every command. `allow-always` durable trust does **not** suppress prompts when effective ask mode is `always`.

### [​](https://docs.openclaw.ai/tools/exec-approvals\#askfallback)  `askFallback`

[​](https://docs.openclaw.ai/tools/exec-approvals#param-ask-fallback)

askFallback

"deny" \| "allowlist" \| "full"

Resolution when a prompt is required but no UI is reachable.

- `deny` — block.
- `allowlist` — allow only if allowlist matches.
- `full` — allow.

### [​](https://docs.openclaw.ai/tools/exec-approvals\#tools-exec-strictinlineeval)  `tools.exec.strictInlineEval`

[​](https://docs.openclaw.ai/tools/exec-approvals#param-strict-inline-eval)

strictInlineEval

boolean

When `true`, OpenClaw treats inline code-eval forms as approval-only
even if the interpreter binary itself is allowlisted. Defense-in-depth
for interpreter loaders that do not map cleanly to one stable file
operand.

Examples that strict mode catches:

- `python -c`
- `node -e`, `node --eval`, `node -p`
- `ruby -e`
- `perl -e`, `perl -E`
- `php -r`
- `lua -e`
- `osascript -e`

In strict mode these commands still need explicit approval, and
`allow-always` does not persist new allowlist entries for them
automatically.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#yolo-mode-no-approval)  YOLO mode (no-approval)

If you want host exec to run without approval prompts, you must open
**both** policy layers — requested exec policy in OpenClaw config
(`tools.exec.*`) **and** host-local approvals policy in
`~/.openclaw/exec-approvals.json`.YOLO is the default host behavior unless you tighten it explicitly:

| Layer | YOLO setting |
| --- | --- |
| `tools.exec.security` | `full` on `gateway`/`node` |
| `tools.exec.ask` | `off` |
| Host `askFallback` | `full` |

**Important distinctions:**

- `tools.exec.host=auto` chooses **where** exec runs: sandbox when available, otherwise gateway.
- YOLO chooses **how** host exec is approved: `security=full` plus `ask=off`.
- In YOLO mode, OpenClaw does **not** add a separate heuristic command-obfuscation approval gate or script-preflight rejection layer on top of the configured host exec policy.
- `auto` does not make gateway routing a free override from a sandboxed session. A per-call `host=node` request is allowed from `auto`; `host=gateway` is only allowed from `auto` when no sandbox runtime is active. For a stable non-auto default, set `tools.exec.host` or use `/exec host=...` explicitly.

CLI-backed providers that expose their own noninteractive permission mode
can follow this policy. Claude CLI adds
`--permission-mode bypassPermissions` when OpenClaw’s requested exec
policy is YOLO. Override that backend behavior with explicit Claude args
under `agents.defaults.cliBackends.claude-cli.args` / `resumeArgs` —
for example `--permission-mode default`, `acceptEdits`, or
`bypassPermissions`.If you want a more conservative setup, tighten either layer back to
`allowlist` / `on-miss` or `deny`.

### [​](https://docs.openclaw.ai/tools/exec-approvals\#persistent-gateway-host-%E2%80%9Cnever-prompt%E2%80%9D-setup)  Persistent gateway-host “never prompt” setup

1

[Navigate to header](https://docs.openclaw.ai/tools/exec-approvals#)

Set the requested config policy

```
openclaw config set tools.exec.host gateway
openclaw config set tools.exec.security full
openclaw config set tools.exec.ask off
openclaw gateway restart
```

2

[Navigate to header](https://docs.openclaw.ai/tools/exec-approvals#)

Match the host approvals file

```
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

### [​](https://docs.openclaw.ai/tools/exec-approvals\#local-shortcut)  Local shortcut

```
openclaw exec-policy preset yolo
```

That local shortcut updates both:

- Local `tools.exec.host/security/ask`.
- Local `~/.openclaw/exec-approvals.json` defaults.

It is intentionally local-only. To change gateway-host or node-host
approvals remotely, use `openclaw approvals set --gateway` or
`openclaw approvals set --node <id|name|ip>`.

### [​](https://docs.openclaw.ai/tools/exec-approvals\#node-host)  Node host

For a node host, apply the same approvals file on that node instead:

```
openclaw approvals set --node <id|name|ip> --stdin <<'EOF'
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

**Local-only limitations:**

- `openclaw exec-policy` does not synchronize node approvals.
- `openclaw exec-policy set --host node` is rejected.
- Node exec approvals are fetched from the node at runtime, so node-targeted updates must use `openclaw approvals --node ...`.

### [​](https://docs.openclaw.ai/tools/exec-approvals\#session-only-shortcut)  Session-only shortcut

- `/exec security=full ask=off` changes only the current session.
- `/elevated full` is a break-glass shortcut that also skips exec approvals for that session.

If the host approvals file stays stricter than config, the stricter host
policy still wins.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#allowlist-per-agent)  Allowlist (per agent)

Allowlists are **per agent**. If multiple agents exist, switch which agent
you are editing in the macOS app. Patterns are glob matches.Patterns can be resolved binary path globs or bare command-name globs.
Bare names match only commands invoked through `PATH`, so `rg` can match
`/opt/homebrew/bin/rg` when the command is `rg`, but **not**`./rg` or
`/tmp/rg`. Use a path glob when you want to trust one specific binary
location.Legacy `agents.default` entries are migrated to `agents.main` on load.
Shell chains such as `echo ok && pwd` still need every top-level segment
to satisfy allowlist rules.Examples:

- `rg`
- `~/Projects/**/bin/peekaboo`
- `~/.local/bin/*`
- `/opt/homebrew/bin/rg`

Each allowlist entry tracks:

| Field | Meaning |
| --- | --- |
| `id` | Stable UUID used for UI identity |
| `lastUsedAt` | Last-used timestamp |
| `lastUsedCommand` | Last command that matched |
| `lastResolvedPath` | Last resolved binary path |

## [​](https://docs.openclaw.ai/tools/exec-approvals\#auto-allow-skill-clis)  Auto-allow skill CLIs

When **Auto-allow skill CLIs** is enabled, executables referenced by
known skills are treated as allowlisted on nodes (macOS node or headless
node host). This uses `skills.bins` over the Gateway RPC to fetch the
skill bin list. Disable this if you want strict manual allowlists.

- This is an **implicit convenience allowlist**, separate from manual path allowlist entries.
- It is intended for trusted operator environments where Gateway and node are in the same trust boundary.
- If you require strict explicit trust, keep `autoAllowSkills: false` and use manual path allowlist entries only.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#safe-bins-and-approval-forwarding)  Safe bins and approval forwarding

For safe bins (the stdin-only fast-path), interpreter binding details, and
how to forward approval prompts to Slack/Discord/Telegram (or run them as
native approval clients), see
[Exec approvals — advanced](https://docs.openclaw.ai/tools/exec-approvals-advanced).

## [​](https://docs.openclaw.ai/tools/exec-approvals\#control-ui-editing)  Control UI editing

Use the **Control UI → Nodes → Exec approvals** card to edit defaults,
per-agent overrides, and allowlists. Pick a scope (Defaults or an agent),
tweak the policy, add/remove allowlist patterns, then **Save**. The UI
shows last-used metadata per pattern so you can keep the list tidy.The target selector chooses **Gateway** (local approvals) or a **Node**.
Nodes must advertise `system.execApprovals.get/set` (macOS app or
headless node host). If a node does not advertise exec approvals yet,
edit its local `~/.openclaw/exec-approvals.json` directly.CLI: `openclaw approvals` supports gateway or node editing — see
[Approvals CLI](https://docs.openclaw.ai/cli/approvals).

## [​](https://docs.openclaw.ai/tools/exec-approvals\#approval-flow)  Approval flow

When a prompt is required, the gateway broadcasts
`exec.approval.requested` to operator clients. The Control UI and macOS
app resolve it via `exec.approval.resolve`, then the gateway forwards the
approved request to the node host.For `host=node`, approval requests include a canonical `systemRunPlan`
payload. The gateway uses that plan as the authoritative
command/cwd/session context when forwarding approved `system.run`
requests.That matters for async approval latency:

- The node exec path prepares one canonical plan up front.
- The approval record stores that plan and its binding metadata.
- Once approved, the final forwarded `system.run` call reuses the stored plan instead of trusting later caller edits.
- If the caller changes `command`, `rawCommand`, `cwd`, `agentId`, or `sessionKey` after the approval request was created, the gateway rejects the forwarded run as an approval mismatch.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#system-events)  System events

Exec lifecycle is surfaced as system messages:

- `Exec running` (only if the command exceeds the running notice threshold).
- `Exec finished`.
- `Exec denied`.

These are posted to the agent’s session after the node reports the event.
Gateway-host exec approvals emit the same lifecycle events when the
command finishes (and optionally when running longer than the threshold).
Approval-gated execs reuse the approval id as the `runId` in these
messages for easy correlation.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#denied-approval-behavior)  Denied approval behavior

When an async exec approval is denied, OpenClaw prevents the agent from
reusing output from any earlier run of the same command in the session.
The denial reason is passed with explicit guidance that no command output
is available, which stops the agent from claiming there is new output or
repeating the denied command with stale results from a prior successful
run.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#implications)  Implications

- **`full`** is powerful; prefer allowlists when possible.
- **`ask`** keeps you in the loop while still allowing fast approvals.
- Per-agent allowlists prevent one agent’s approvals from leaking into others.
- Approvals only apply to host exec requests from **authorized senders**. Unauthorized senders cannot issue `/exec`.
- `/exec security=full` is a session-level convenience for authorized operators and skips approvals by design. To hard-block host exec, set approvals security to `deny` or deny the `exec` tool via tool policy.

## [​](https://docs.openclaw.ai/tools/exec-approvals\#related)  Related

[**Exec approvals — advanced** \\
\\
Safe bins, interpreter binding, and approval forwarding to chat.](https://docs.openclaw.ai/tools/exec-approvals-advanced)

[**Exec tool** \\
\\
Shell command execution tool.](https://docs.openclaw.ai/tools/exec)

[**Elevated mode** \\
\\
Break-glass path that also skips approvals.](https://docs.openclaw.ai/tools/elevated)

[**Sandboxing** \\
\\
Sandbox modes and workspace access.](https://docs.openclaw.ai/gateway/sandboxing)

[**Security** \\
\\
Security model and hardening.](https://docs.openclaw.ai/gateway/security)

[**Sandbox vs tool policy vs elevated** \\
\\
When to reach for each control.](https://docs.openclaw.ai/gateway/sandbox-vs-tool-policy-vs-elevated)

[**Skills** \\
\\
Skill-backed auto-allow behavior.](https://docs.openclaw.ai/tools/skills)

[Elevated mode](https://docs.openclaw.ai/tools/elevated) [Exec approvals — advanced](https://docs.openclaw.ai/tools/exec-approvals-advanced)

Ctrl+I