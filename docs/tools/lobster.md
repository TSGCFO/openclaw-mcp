---
source_url: https://docs.openclaw.ai/tools/lobster
title: "Lobster - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/lobster#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Tools

Lobster

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Hook](https://docs.openclaw.ai/tools/lobster#hook)
- [Why](https://docs.openclaw.ai/tools/lobster#why)
- [Why a DSL instead of plain programs?](https://docs.openclaw.ai/tools/lobster#why-a-dsl-instead-of-plain-programs)
- [How it works](https://docs.openclaw.ai/tools/lobster#how-it-works)
- [Pattern: small CLI + JSON pipes + approvals](https://docs.openclaw.ai/tools/lobster#pattern-small-cli-%2B-json-pipes-%2B-approvals)
- [JSON-only LLM steps (llm-task)](https://docs.openclaw.ai/tools/lobster#json-only-llm-steps-llm-task)
- [Workflow files (.lobster)](https://docs.openclaw.ai/tools/lobster#workflow-files-lobster)
- [Install Lobster](https://docs.openclaw.ai/tools/lobster#install-lobster)
- [Enable the tool](https://docs.openclaw.ai/tools/lobster#enable-the-tool)
- [Example: Email triage](https://docs.openclaw.ai/tools/lobster#example-email-triage)
- [Tool parameters](https://docs.openclaw.ai/tools/lobster#tool-parameters)
- [run](https://docs.openclaw.ai/tools/lobster#run)
- [resume](https://docs.openclaw.ai/tools/lobster#resume)
- [Optional inputs](https://docs.openclaw.ai/tools/lobster#optional-inputs)
- [Output envelope](https://docs.openclaw.ai/tools/lobster#output-envelope)
- [Approvals](https://docs.openclaw.ai/tools/lobster#approvals)
- [OpenProse](https://docs.openclaw.ai/tools/lobster#openprose)
- [Safety](https://docs.openclaw.ai/tools/lobster#safety)
- [Troubleshooting](https://docs.openclaw.ai/tools/lobster#troubleshooting)
- [Learn more](https://docs.openclaw.ai/tools/lobster#learn-more)
- [Case study: community workflows](https://docs.openclaw.ai/tools/lobster#case-study-community-workflows)
- [Related](https://docs.openclaw.ai/tools/lobster#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Lobster is a workflow shell that lets OpenClaw run multi-step tool sequences as a single, deterministic operation with explicit approval checkpoints.Lobster is one authoring layer above detached background work. For flow orchestration above individual tasks, see [Task Flow](https://docs.openclaw.ai/automation/taskflow) (`openclaw tasks flow`). For the task activity ledger, see [`openclaw tasks`](https://docs.openclaw.ai/automation/tasks).

## [​](https://docs.openclaw.ai/tools/lobster\#hook)  Hook

Your assistant can build the tools that manage itself. Ask for a workflow, and 30 minutes later you have a CLI plus pipelines that run as one call. Lobster is the missing piece: deterministic pipelines, explicit approvals, and resumable state.

## [​](https://docs.openclaw.ai/tools/lobster\#why)  Why

Today, complex workflows require many back-and-forth tool calls. Each call costs tokens, and the LLM has to orchestrate every step. Lobster moves that orchestration into a typed runtime:

- **One call instead of many**: OpenClaw runs one Lobster tool call and gets a structured result.
- **Approvals built in**: Side effects (send email, post comment) halt the workflow until explicitly approved.
- **Resumable**: Halted workflows return a token; approve and resume without re-running everything.

## [​](https://docs.openclaw.ai/tools/lobster\#why-a-dsl-instead-of-plain-programs)  Why a DSL instead of plain programs?

Lobster is intentionally small. The goal is not “a new language,” it’s a predictable, AI-friendly pipeline spec with first-class approvals and resume tokens.

- **Approve/resume is built in**: A normal program can prompt a human, but it can’t _pause and resume_ with a durable token without you inventing that runtime yourself.
- **Determinism + auditability**: Pipelines are data, so they’re easy to log, diff, replay, and review.
- **Constrained surface for AI**: A tiny grammar + JSON piping reduces “creative” code paths and makes validation realistic.
- **Safety policy baked in**: Timeouts, output caps, sandbox checks, and allowlists are enforced by the runtime, not each script.
- **Still programmable**: Each step can call any CLI or script. If you want JS/TS, generate `.lobster` files from code.

## [​](https://docs.openclaw.ai/tools/lobster\#how-it-works)  How it works

OpenClaw runs Lobster workflows **in-process** using an embedded runner. No external CLI subprocess is spawned; the workflow engine executes inside the gateway process and returns a JSON envelope directly.
If the pipeline pauses for approval, the tool returns a `resumeToken` so you can continue later.

## [​](https://docs.openclaw.ai/tools/lobster\#pattern-small-cli-+-json-pipes-+-approvals)  Pattern: small CLI + JSON pipes + approvals

Build tiny commands that speak JSON, then chain them into a single Lobster call. (Example command names below — swap in your own.)

```
inbox list --json
inbox categorize --json
inbox apply --json
```

```
{
  "action": "run",
  "pipeline": "exec --json --shell 'inbox list --json' | exec --stdin json --shell 'inbox categorize --json' | exec --stdin json --shell 'inbox apply --json' | approve --preview-from-stdin --limit 5 --prompt 'Apply changes?'",
  "timeoutMs": 30000
}
```

If the pipeline requests approval, resume with the token:

```
{
  "action": "resume",
  "token": "<resumeToken>",
  "approve": true
}
```

AI triggers the workflow; Lobster executes the steps. Approval gates keep side effects explicit and auditable.Example: map input items into tool calls:

```
gog.gmail.search --query 'newer_than:1d' \
  | openclaw.invoke --tool message --action send --each --item-key message --args-json '{"provider":"telegram","to":"..."}'
```

## [​](https://docs.openclaw.ai/tools/lobster\#json-only-llm-steps-llm-task)  JSON-only LLM steps (llm-task)

For workflows that need a **structured LLM step**, enable the optional
`llm-task` plugin tool and call it from Lobster. This keeps the workflow
deterministic while still letting you classify/summarize/draft with a model.Enable the tool:

```
{
  "plugins": {
    "entries": {
      "llm-task": { "enabled": true }
    }
  },
  "agents": {
    "list": [\
      {\
        "id": "main",\
        "tools": { "allow": ["llm-task"] }\
      }\
    ]
  }
}
```

Use it in a pipeline:

```
openclaw.invoke --tool llm-task --action json --args-json '{
  "prompt": "Given the input email, return intent and draft.",
  "thinking": "low",
  "input": { "subject": "Hello", "body": "Can you help?" },
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

See [LLM Task](https://docs.openclaw.ai/tools/llm-task) for details and configuration options.

## [​](https://docs.openclaw.ai/tools/lobster\#workflow-files-lobster)  Workflow files (.lobster)

Lobster can run YAML/JSON workflow files with `name`, `args`, `steps`, `env`, `condition`, and `approval` fields. In OpenClaw tool calls, set `pipeline` to the file path.

```
name: inbox-triage
args:
  tag:
    default: "family"
steps:
  - id: collect
    command: inbox list --json
  - id: categorize
    command: inbox categorize --json
    stdin: $collect.stdout
  - id: approve
    command: inbox apply --approve
    stdin: $categorize.stdout
    approval: required
  - id: execute
    command: inbox apply --execute
    stdin: $categorize.stdout
    condition: $approve.approved
```

Notes:

- `stdin: $step.stdout` and `stdin: $step.json` pass a prior step’s output.
- `condition` (or `when`) can gate steps on `$step.approved`.

## [​](https://docs.openclaw.ai/tools/lobster\#install-lobster)  Install Lobster

Bundled Lobster workflows run in-process; no separate `lobster` binary is required. The embedded runner ships with the Lobster plugin.If you need the standalone Lobster CLI for development or external pipelines, install it from the [Lobster repo](https://github.com/openclaw/lobster) and ensure `lobster` is on `PATH`.

## [​](https://docs.openclaw.ai/tools/lobster\#enable-the-tool)  Enable the tool

Lobster is an **optional** plugin tool (not enabled by default).Recommended (additive, safe):

```
{
  "tools": {
    "alsoAllow": ["lobster"]
  }
}
```

Or per-agent:

```
{
  "agents": {
    "list": [\
      {\
        "id": "main",\
        "tools": {\
          "alsoAllow": ["lobster"]\
        }\
      }\
    ]
  }
}
```

Avoid using `tools.allow: ["lobster"]` unless you intend to run in restrictive allowlist mode.

Allowlists are opt-in for optional plugins. If your allowlist only names plugin tools (like `lobster`), OpenClaw keeps core tools enabled. To restrict core tools, include the core tools or groups you want in the allowlist too.

## [​](https://docs.openclaw.ai/tools/lobster\#example-email-triage)  Example: Email triage

Without Lobster:

```
User: "Check my email and draft replies"
→ openclaw calls gmail.list
→ LLM summarizes
→ User: "draft replies to #2 and #5"
→ LLM drafts
→ User: "send #2"
→ openclaw calls gmail.send
(repeat daily, no memory of what was triaged)
```

With Lobster:

```
{
  "action": "run",
  "pipeline": "email.triage --limit 20",
  "timeoutMs": 30000
}
```

Returns a JSON envelope (truncated):

```
{
  "ok": true,
  "status": "needs_approval",
  "output": [{ "summary": "5 need replies, 2 need action" }],
  "requiresApproval": {
    "type": "approval_request",
    "prompt": "Send 2 draft replies?",
    "items": [],
    "resumeToken": "..."
  }
}
```

User approves → resume:

```
{
  "action": "resume",
  "token": "<resumeToken>",
  "approve": true
}
```

One workflow. Deterministic. Safe.

## [​](https://docs.openclaw.ai/tools/lobster\#tool-parameters)  Tool parameters

### [​](https://docs.openclaw.ai/tools/lobster\#run)  `run`

Run a pipeline in tool mode.

```
{
  "action": "run",
  "pipeline": "gog.gmail.search --query 'newer_than:1d' | email.triage",
  "cwd": "workspace",
  "timeoutMs": 30000,
  "maxStdoutBytes": 512000
}
```

Run a workflow file with args:

```
{
  "action": "run",
  "pipeline": "/path/to/inbox-triage.lobster",
  "argsJson": "{\"tag\":\"family\"}"
}
```

### [​](https://docs.openclaw.ai/tools/lobster\#resume)  `resume`

Continue a halted workflow after approval.

```
{
  "action": "resume",
  "token": "<resumeToken>",
  "approve": true
}
```

### [​](https://docs.openclaw.ai/tools/lobster\#optional-inputs)  Optional inputs

- `cwd`: Relative working directory for the pipeline (must stay within the gateway working directory).
- `timeoutMs`: Abort the workflow if it exceeds this duration (default: 20000).
- `maxStdoutBytes`: Abort the workflow if output exceeds this size (default: 512000).
- `argsJson`: JSON string passed to `lobster run --args-json` (workflow files only).

## [​](https://docs.openclaw.ai/tools/lobster\#output-envelope)  Output envelope

Lobster returns a JSON envelope with one of three statuses:

- `ok` → finished successfully
- `needs_approval` → paused; `requiresApproval.resumeToken` is required to resume
- `cancelled` → explicitly denied or cancelled

The tool surfaces the envelope in both `content` (pretty JSON) and `details` (raw object).

## [​](https://docs.openclaw.ai/tools/lobster\#approvals)  Approvals

If `requiresApproval` is present, inspect the prompt and decide:

- `approve: true` → resume and continue side effects
- `approve: false` → cancel and finalize the workflow

Use `approve --preview-from-stdin --limit N` to attach a JSON preview to approval requests without custom jq/heredoc glue. Resume tokens are now compact: Lobster stores workflow resume state under its state dir and hands back a small token key.

## [​](https://docs.openclaw.ai/tools/lobster\#openprose)  OpenProse

OpenProse pairs well with Lobster: use `/prose` to orchestrate multi-agent prep, then run a Lobster pipeline for deterministic approvals. If a Prose program needs Lobster, allow the `lobster` tool for sub-agents via `tools.subagents.tools`. See [OpenProse](https://docs.openclaw.ai/prose).

## [​](https://docs.openclaw.ai/tools/lobster\#safety)  Safety

- **Local in-process only** — workflows execute inside the gateway process; no network calls from the plugin itself.
- **No secrets** — Lobster doesn’t manage OAuth; it calls OpenClaw tools that do.
- **Sandbox-aware** — disabled when the tool context is sandboxed.
- **Hardened** — timeouts and output caps enforced by the embedded runner.

## [​](https://docs.openclaw.ai/tools/lobster\#troubleshooting)  Troubleshooting

- **`lobster timed out`** → increase `timeoutMs`, or split a long pipeline.
- **`lobster output exceeded maxStdoutBytes`** → raise `maxStdoutBytes` or reduce output size.
- **`lobster returned invalid JSON`** → ensure the pipeline runs in tool mode and prints only JSON.
- **`lobster failed`** → check gateway logs for the embedded runner error details.

## [​](https://docs.openclaw.ai/tools/lobster\#learn-more)  Learn more

- [Plugins](https://docs.openclaw.ai/tools/plugin)
- [Plugin tool authoring](https://docs.openclaw.ai/plugins/building-plugins#registering-agent-tools)

## [​](https://docs.openclaw.ai/tools/lobster\#case-study-community-workflows)  Case study: community workflows

One public example: a “second brain” CLI + Lobster pipelines that manage three Markdown vaults (personal, partner, shared). The CLI emits JSON for stats, inbox listings, and stale scans; Lobster chains those commands into workflows like `weekly-review`, `inbox-triage`, `memory-consolidation`, and `shared-task-sync`, each with approval gates. AI handles judgment (categorization) when available and falls back to deterministic rules when not.

- Thread: [https://x.com/plattenschieber/status/2014508656335770033](https://x.com/plattenschieber/status/2014508656335770033)
- Repo: [https://github.com/bloomedai/brain-cli](https://github.com/bloomedai/brain-cli)

## [​](https://docs.openclaw.ai/tools/lobster\#related)  Related

- [Automation & Tasks](https://docs.openclaw.ai/automation) — scheduling Lobster workflows
- [Automation Overview](https://docs.openclaw.ai/automation) — all automation mechanisms
- [Tools Overview](https://docs.openclaw.ai/tools) — all available agent tools

[LLM task](https://docs.openclaw.ai/tools/llm-task) [Media overview](https://docs.openclaw.ai/tools/media-overview)

Ctrl+I