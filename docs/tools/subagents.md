---
source_url: https://docs.openclaw.ai/tools/subagents.md
title: ""
---

\> ## Documentation Index
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Sub-agents

Sub-agents are background agent runs spawned from an existing agent run.
They run in their own session (\`agent::subagent:\`) and,
when finished, \*\*announce\*\* their result back to the requester chat
channel. Each sub-agent run is tracked as a
\[background task\](/automation/tasks).

Primary goals:

\\* Parallelize "research / long task / slow tool" work without blocking the main run.
\\* Keep sub-agents isolated by default (session separation + optional sandboxing).
\\* Keep the tool surface hard to misuse: sub-agents do \*\*not\*\* get session tools by default.
\\* Support configurable nesting depth for orchestrator patterns.

 \*\*Cost note:\*\* each sub-agent has its own context and token usage by
 default. For heavy or repetitive tasks, set a cheaper model for sub-agents
 and keep your main agent on a higher-quality model. Configure via
 \`agents.defaults.subagents.model\` or per-agent overrides. When a child
 genuinely needs the requester's current transcript, the agent can request
 \`context: "fork"\` on that one spawn. Thread-bound subagent sessions default
 to \`context: "fork"\` because they branch the current conversation into a
 follow-up thread.

\## Slash command

Use \`/subagents\` to inspect or control sub-agent runs for the \*\*current
session\*\*:

\`\`\`text theme={"theme":{"light":"min-light","dark":"min-dark"}}
/subagents list
/subagents kill
/subagents log  \[limit\] \[tools\]
/subagents info
/subagents send
/subagents steer
/subagents spawn  \[--model \] \[--thinking \]
\`\`\`

\`/subagents info\` shows run metadata (status, timestamps, session id,
transcript path, cleanup). Use \`sessions\_history\` for a bounded,
safety-filtered recall view; inspect the transcript path on disk when you
need the raw full transcript.

\### Thread binding controls

These commands work on channels that support persistent thread bindings.
See \[Thread supporting channels\](#thread-supporting-channels) below.

\`\`\`text theme={"theme":{"light":"min-light","dark":"min-dark"}}
/focus
/unfocus
/agents
/session idle
/session max-age
\`\`\`

\### Spawn behavior

\`/subagents spawn\` starts a background sub-agent as a user command (not an
internal relay) and sends one final completion update back to the
requester chat when the run finishes.

 \\* The spawn command is non-blocking; it returns a run id immediately.
 \\* On completion, the sub-agent announces a summary/result message back to the requester chat channel.
 \\* Completion is push-based. Once spawned, do \*\*not\*\* poll \`/subagents list\`, \`sessions\_list\`, or \`sessions\_history\` in a loop just to wait for it to finish; inspect status only on-demand for debugging or intervention.
 \\* On completion, OpenClaw best-effort closes tracked browser tabs/processes opened by that sub-agent session before the announce cleanup flow continues.

 \\* OpenClaw tries direct \`agent\` delivery first with a stable idempotency key.
 \\* If direct delivery fails, it falls back to queue routing.
 \\* If queue routing is still not available, the announce is retried with a short exponential backoff before final give-up.
 \\* Completion delivery keeps the resolved requester route: thread-bound or conversation-bound completion routes win when available; if the completion origin only provides a channel, OpenClaw fills the missing target/account from the requester session's resolved route (\`lastChannel\` / \`lastTo\` / \`lastAccountId\`) so direct delivery still works.

 The completion handoff to the requester session is runtime-generated
 internal context (not user-authored text) and includes:

 \\* \`Result\` — latest visible \`assistant\` reply text, otherwise sanitized latest tool/toolResult text. Terminal failed runs do not reuse captured reply text.
 \\* \`Status\` — \`completed successfully\` / \`failed\` / \`timed out\` / \`unknown\`.
 \\* Compact runtime/token stats.
 \\* A delivery instruction telling the requester agent to rewrite in normal assistant voice (not forward raw internal metadata).

 \\* \`--model\` and \`--thinking\` override defaults for that specific run.
 \\* Use \`info\`/\`log\` to inspect details and output after completion.
 \\* \`/subagents spawn\` is one-shot mode (\`mode: "run"\`). For persistent thread-bound sessions, use \`sessions\_spawn\` with \`thread: true\` and \`mode: "session"\`.
 \\* For ACP harness sessions (Claude Code, Gemini CLI, OpenCode, or explicit Codex ACP/acpx), use \`sessions\_spawn\` with \`runtime: "acp"\` when the tool advertises that runtime. See \[ACP delivery model\](/tools/acp-agents#delivery-model) when debugging completions or agent-to-agent loops. When the \`codex\` plugin is enabled, Codex chat/thread control should prefer \`/codex ...\` over ACP unless the user explicitly asks for ACP/acpx.
 \\* OpenClaw hides \`runtime: "acp"\` until ACP is enabled, the requester is not sandboxed, and a backend plugin such as \`acpx\` is loaded. \`runtime: "acp"\` expects an external ACP harness id, or an \`agents.list\[\]\` entry with \`runtime.type="acp"\`; use the default sub-agent runtime for normal OpenClaw config agents from \`agents\_list\`.


\## Context modes

Native sub-agents start isolated unless the caller explicitly asks to fork
the current transcript.

\| Mode \| When to use it \| Behavior \|
\| \-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`isolated\` \| Fresh research, independent implementation, slow tool work, or anything that can be briefed in the task text \| Creates a clean child transcript. This is the default and keeps token use lower. \|
\| \`fork\` \| Work that depends on the current conversation, prior tool results, or nuanced instructions already present in the requester transcript \| Branches the requester transcript into the child session before the child starts. \|

Use \`fork\` sparingly. It is for context-sensitive delegation, not a
replacement for writing a clear task prompt.

\## Tool: \`sessions\_spawn\`

Starts a sub-agent run with \`deliver: false\` on the global \`subagent\` lane,
then runs an announce step and posts the announce reply to the requester
chat channel.

Availability depends on the caller's effective tool policy. The \`coding\` and
\`full\` profiles expose \`sessions\_spawn\` by default. The \`messaging\` profile
does not; add \`tools.alsoAllow: \["sessions\_spawn", "sessions\_yield",\
"subagents"\]\` or use \`tools.profile: "coding"\` for agents that should delegate
work. Channel/group, provider, sandbox, and per-agent allow/deny policies can
still remove the tool after the profile stage. Use \`/tools\` from the same
session to confirm the effective tool list.

\*\*Defaults:\*\*

\\* \*\*Model:\*\* inherits the caller unless you set \`agents.defaults.subagents.model\` (or per-agent \`agents.list\[\].subagents.model\`); an explicit \`sessions\_spawn.model\` still wins.
\\* \*\*Thinking:\*\* inherits the caller unless you set \`agents.defaults.subagents.thinking\` (or per-agent \`agents.list\[\].subagents.thinking\`); an explicit \`sessions\_spawn.thinking\` still wins.
\\* \*\*Run timeout:\*\* if \`sessions\_spawn.runTimeoutSeconds\` is omitted, OpenClaw uses \`agents.defaults.subagents.runTimeoutSeconds\` when set; otherwise it falls back to \`0\` (no timeout).

\### Tool parameters

 The task description for the sub-agent.

 Optional human-readable label.

 Spawn under another agent id when allowed by \`subagents.allowAgents\`.

 \`acp\` is only for external ACP harnesses (\`claude\`, \`droid\`, \`gemini\`, \`opencode\`, or explicitly requested Codex ACP/acpx) and for \`agents.list\[\]\` entries whose \`runtime.type\` is \`acp\`.

 ACP-only. Resumes an existing ACP harness session when \`runtime: "acp"\`; ignored for native sub-agent spawns.

 ACP-only. Streams ACP run output to the parent session when \`runtime: "acp"\`; omit for native sub-agent spawns.

 Override the sub-agent model. Invalid values are skipped and the sub-agent runs on the default model with a warning in the tool result.

 Override thinking level for the sub-agent run.

 Defaults to \`agents.defaults.subagents.runTimeoutSeconds\` when set, otherwise \`0\`. When set, the sub-agent run is aborted after N seconds.

 When \`true\`, requests channel thread binding for this sub-agent session.

 If \`thread: true\` and \`mode\` omitted, default becomes \`session\`. \`mode: "session"\` requires \`thread: true\`.

 \`"delete"\` archives immediately after announce (still keeps the transcript via rename).

 \`require\` rejects spawn unless the target child runtime is sandboxed.

 \`fork\` branches the requester's current transcript into the child session. Native sub-agents only. Thread-bound spawns default to \`fork\`; non-thread spawns default to \`isolated\`.

 \`sessions\_spawn\` does \*\*not\*\* accept channel-delivery params (\`target\`,
 \`channel\`, \`to\`, \`threadId\`, \`replyTo\`, \`transport\`). For delivery, use
 \`message\`/\`sessions\_send\` from the spawned run.

\## Thread-bound sessions

When thread bindings are enabled for a channel, a sub-agent can stay bound
to a thread so follow-up user messages in that thread keep routing to the
same sub-agent session.

\### Thread supporting channels

\*\*Discord\*\* is currently the only supported channel. It supports
persistent thread-bound subagent sessions (\`sessions\_spawn\` with
\`thread: true\`), manual thread controls (\`/focus\`, \`/unfocus\`, \`/agents\`,
\`/session idle\`, \`/session max-age\`), and adapter keys
\`channels.discord.threadBindings.enabled\`,
\`channels.discord.threadBindings.idleHours\`,
\`channels.discord.threadBindings.maxAgeHours\`, and
\`channels.discord.threadBindings.spawnSessions\`.

\### Quick flow

 \`sessions\_spawn\` with \`thread: true\` (and optionally \`mode: "session"\`).

 OpenClaw creates or binds a thread to that session target in the active channel.

 Replies and follow-up messages in that thread route to the bound session.

 Use \`/session idle\` to inspect/update inactivity auto-unfocus and
 \`/session max-age\` to control the hard cap.

 Use \`/unfocus\` to detach manually.


\### Manual controls

\| Command \| Effect \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`/focus \` \| Bind the current thread (or create one) to a sub-agent/session target \|
\| \`/unfocus\` \| Remove the binding for the current bound thread \|
\| \`/agents\` \| List active runs and binding state (\`thread:\` or \`unbound\`) \|
\| \`/session idle\` \| Inspect/update idle auto-unfocus (focused bound threads only) \|
\| \`/session max-age\` \| Inspect/update hard cap (focused bound threads only) \|

\### Config switches

\\* \*\*Global default:\*\* \`session.threadBindings.enabled\`, \`session.threadBindings.idleHours\`, \`session.threadBindings.maxAgeHours\`.
\\* \*\*Channel override and spawn auto-bind keys\*\* are adapter-specific. See \[Thread supporting channels\](#thread-supporting-channels) above.

See \[Configuration reference\](/gateway/configuration-reference) and
\[Slash commands\](/tools/slash-commands) for current adapter details.

\### Allowlist

 List of agent ids that can be targeted via explicit \`agentId\` (\`\["\*"\]\` allows any). Default: only the requester agent. If you set a list and still want the requester to spawn itself with \`agentId\`, include the requester id in the list.

 Default target-agent allowlist used when the requester agent does not set its own \`subagents.allowAgents\`.

 Block \`sessions\_spawn\` calls that omit \`agentId\` (forces explicit profile selection). Per-agent override: \`agents.list\[\].subagents.requireAgentId\`.

If the requester session is sandboxed, \`sessions\_spawn\` rejects targets
that would run unsandboxed.

\### Discovery

Use \`agents\_list\` to see which agent ids are currently allowed for
\`sessions\_spawn\`. The response includes each listed agent's effective
model and embedded runtime metadata so callers can distinguish PI, Codex
app-server, and other configured native runtimes.

\### Auto-archive

\\* Sub-agent sessions are automatically archived after \`agents.defaults.subagents.archiveAfterMinutes\` (default \`60\`).
\\* Archive uses \`sessions.delete\` and renames the transcript to \`\*.deleted.\` (same folder).
\\* \`cleanup: "delete"\` archives immediately after announce (still keeps the transcript via rename).
\\* Auto-archive is best-effort; pending timers are lost if the gateway restarts.
\\* \`runTimeoutSeconds\` does \*\*not\*\* auto-archive; it only stops the run. The session remains until auto-archive.
\\* Auto-archive applies equally to depth-1 and depth-2 sessions.
\\* Browser cleanup is separate from archive cleanup: tracked browser tabs/processes are best-effort closed when the run finishes, even if the transcript/session record is kept.

\## Nested sub-agents

By default, sub-agents cannot spawn their own sub-agents
(\`maxSpawnDepth: 1\`). Set \`maxSpawnDepth: 2\` to enable one level of
nesting — the \*\*orchestrator pattern\*\*: main → orchestrator sub-agent →
worker sub-sub-agents.

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 subagents: {
 maxSpawnDepth: 2, // allow sub-agents to spawn children (default: 1)
 maxChildrenPerAgent: 5, // max active children per agent session (default: 5)
 maxConcurrent: 8, // global concurrency lane cap (default: 8)
 runTimeoutSeconds: 900, // default timeout for sessions\_spawn when omitted (0 = no timeout)
 },
 },
 },
}
\`\`\`

\### Depth levels

\| Depth \| Session key shape \| Role \| Can spawn? \|
\| \-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| 0 \| \`agent::main\` \| Main agent \| Always \|
\| 1 \| \`agent::subagent:\` \| Sub-agent (orchestrator when depth 2 allowed) \| Only if \`maxSpawnDepth >= 2\` \|
\| 2 \| \`agent::subagent::subagent:\` \| Sub-sub-agent (leaf worker) \| Never \|

\### Announce chain

Results flow back up the chain:

1\. Depth-2 worker finishes → announces to its parent (depth-1 orchestrator).
2\. Depth-1 orchestrator receives the announce, synthesizes results, finishes → announces to main.
3\. Main agent receives the announce and delivers to the user.

Each level only sees announces from its direct children.

 \*\*Operational guidance:\*\* start child work once and wait for completion
 events instead of building poll loops around \`sessions\_list\`,
 \`sessions\_history\`, \`/subagents list\`, or \`exec\` sleep commands.
 \`sessions\_list\` and \`/subagents list\` keep child-session relationships
 focused on live work — live children remain attached, ended children stay
 visible for a short recent window, and stale store-only child links are
 ignored after their freshness window. This prevents old \`spawnedBy\` /
 \`parentSessionKey\` metadata from resurrecting ghost children after
 restart. If a child completion event arrives after you already sent the
 final answer, the correct follow-up is the exact silent token
 \`NO\_REPLY\` / \`no\_reply\`.

\### Tool policy by depth

\\* Role and control scope are written into session metadata at spawn time. That keeps flat or restored session keys from accidentally regaining orchestrator privileges.
\\* \*\*Depth 1 (orchestrator, when \`maxSpawnDepth >= 2\`):\*\* gets \`sessions\_spawn\`, \`subagents\`, \`sessions\_list\`, \`sessions\_history\` so it can manage its children. Other session/system tools remain denied.
\\* \*\*Depth 1 (leaf, when \`maxSpawnDepth == 1\`):\*\* no session tools (current default behavior).
\\* \*\*Depth 2 (leaf worker):\*\* no session tools — \`sessions\_spawn\` is always denied at depth 2. Cannot spawn further children.

\### Per-agent spawn limit

Each agent session (at any depth) can have at most \`maxChildrenPerAgent\`
(default \`5\`) active children at a time. This prevents runaway fan-out
from a single orchestrator.

\### Cascade stop

Stopping a depth-1 orchestrator automatically stops all its depth-2
children:

\\* \`/stop\` in the main chat stops all depth-1 agents and cascades to their depth-2 children.
\\* \`/subagents kill \` stops a specific sub-agent and cascades to its children.
\\* \`/subagents kill all\` stops all sub-agents for the requester and cascades.

\## Authentication

Sub-agent auth is resolved by \*\*agent id\*\*, not by session type:

\\* The sub-agent session key is \`agent::subagent:\`.
\\* The auth store is loaded from that agent's \`agentDir\`.
\\* The main agent's auth profiles are merged in as a \*\*fallback\*\*; agent profiles override main profiles on conflicts.

The merge is additive, so main profiles are always available as
fallbacks. Fully isolated auth per agent is not supported yet.

\## Announce

Sub-agents report back via an announce step:

\\* The announce step runs inside the sub-agent session (not the requester session).
\\* If the sub-agent replies exactly \`ANNOUNCE\_SKIP\`, nothing is posted.
\\* If the latest assistant text is the exact silent token \`NO\_REPLY\` / \`no\_reply\`, announce output is suppressed even if earlier visible progress existed.

Delivery depends on requester depth:

\\* Top-level requester sessions use a follow-up \`agent\` call with external delivery (\`deliver=true\`).
\\* Nested requester subagent sessions receive an internal follow-up injection (\`deliver=false\`) so the orchestrator can synthesize child results in-session.
\\* If a nested requester subagent session is gone, OpenClaw falls back to that session's requester when available.

For top-level requester sessions, completion-mode direct delivery first
resolves any bound conversation/thread route and hook override, then fills
missing channel-target fields from the requester session's stored route.
That keeps completions on the right chat/topic even when the completion
origin only identifies the channel.

Child completion aggregation is scoped to the current requester run when
building nested completion findings, preventing stale prior-run child
outputs from leaking into the current announce. Announce replies preserve
thread/topic routing when available on channel adapters.

\### Announce context

Announce context is normalized to a stable internal event block:

\| Field \| Source \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| Source \| \`subagent\` or \`cron\` \|
\| Session ids \| Child session key/id \|
\| Type \| Announce type + task label \|
\| Status \| Derived from runtime outcome (\`success\`, \`error\`, \`timeout\`, or \`unknown\`) — \*\*not\*\* inferred from model text \|
\| Result content \| Latest visible assistant text, otherwise sanitized latest tool/toolResult text \|
\| Follow-up \| Instruction describing when to reply vs stay silent \|

Terminal failed runs report failure status without replaying captured
reply text. On timeout, if the child only got through tool calls, announce
can collapse that history into a short partial-progress summary instead
of replaying raw tool output.

\### Stats line

Announce payloads include a stats line at the end (even when wrapped):

\\* Runtime (e.g. \`runtime 5m12s\`).
\\* Token usage (input/output/total).
\\* Estimated cost when model pricing is configured (\`models.providers.\*.models\[\].cost\`).
\\* \`sessionKey\`, \`sessionId\`, and transcript path so the main agent can fetch history via \`sessions\_history\` or inspect the file on disk.

Internal metadata is meant for orchestration only; user-facing replies
should be rewritten in normal assistant voice.

\### Why prefer \`sessions\_history\`

\`sessions\_history\` is the safer orchestration path:

\\* Assistant recall is normalized first: thinking tags stripped; \`\` / \`\` scaffolding stripped; plain-text tool-call XML payload blocks (\`\`, \`\`, \`\`, \`\`) stripped, including truncated payloads that never close cleanly; downgraded tool-call/result scaffolding and historical-context markers stripped; leaked model control tokens (\`<\|assistant\|>\`, other ASCII \`<\|...\|>\`, full-width \`<｜...｜>\`) stripped; malformed MiniMax tool-call XML stripped.
\\* Credential/token-like text is redacted.
\\* Long blocks can be truncated.
\\* Very large histories can drop older rows or replace an oversized row with \`\[sessions\_history omitted: message too large\]\`.
\\* Raw on-disk transcript inspection is the fallback when you need the full byte-for-byte transcript.

\## Tool policy

Sub-agents use the same profile and tool-policy pipeline as the parent or
target agent first. After that, OpenClaw applies the sub-agent restriction
layer.

With no restrictive \`tools.profile\`, sub-agents get \*\*all tools except
session tools\*\* and system tools:

\\* \`sessions\_list\`
\\* \`sessions\_history\`
\\* \`sessions\_send\`
\\* \`sessions\_spawn\`

\`sessions\_history\` remains a bounded, sanitized recall view here too — it
is not a raw transcript dump.

When \`maxSpawnDepth >= 2\`, depth-1 orchestrator sub-agents additionally
receive \`sessions\_spawn\`, \`subagents\`, \`sessions\_list\`, and
\`sessions\_history\` so they can manage their children.

\### Override via config

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 subagents: {
 maxConcurrent: 1,
 },
 },
 },
 tools: {
 subagents: {
 tools: {
 // deny wins
 deny: \["gateway", "cron"\],
 // if allow is set, it becomes allow-only (deny still wins)
 // allow: \["read", "exec", "process"\]
 },
 },
 },
}
\`\`\`

\`tools.subagents.tools.allow\` is a final allow-only filter. It can narrow
the already-resolved tool set, but it cannot \*\*add back\*\* a tool removed
by \`tools.profile\`. For example, \`tools.profile: "coding"\` includes
\`web\_search\`/\`web\_fetch\` but not the \`browser\` tool. To let
coding-profile sub-agents use browser automation, add browser at the
profile stage:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 tools: {
 profile: "coding",
 alsoAllow: \["browser"\],
 },
}
\`\`\`

Use per-agent \`agents.list\[\].tools.alsoAllow: \["browser"\]\` when only one
agent should get browser automation.

\## Concurrency

Sub-agents use a dedicated in-process queue lane:

\\* \*\*Lane name:\*\* \`subagent\`
\\* \*\*Concurrency:\*\* \`agents.defaults.subagents.maxConcurrent\` (default \`8\`)

\## Liveness and recovery

OpenClaw does not treat \`endedAt\` absence as permanent proof that a
sub-agent is still alive. Unended runs older than the stale-run window
stop counting as active/pending in \`/subagents list\`, status summaries,
descendant completion gating, and per-session concurrency checks.

After a gateway restart, stale unended restored runs are pruned unless
their child session is marked \`abortedLastRun: true\`. Those
restart-aborted child sessions remain recoverable through the sub-agent
orphan recovery flow, which sends a synthetic resume message before
clearing the aborted marker.

Automatic restart recovery is bounded per child session. If the same
sub-agent child is accepted for orphan recovery repeatedly inside the
rapid re-wedge window, OpenClaw persists a recovery tombstone on that
session and stops auto-resuming it on later restarts. Run
\`openclaw tasks maintenance --apply\` to reconcile the task record, or
\`openclaw doctor --fix\` to clear stale aborted recovery flags on
tombstoned sessions.

 If a sub-agent spawn fails with Gateway \`PAIRING\_REQUIRED\` /
 \`scope-upgrade\`, check the RPC caller before editing pairing state.
 Internal \`sessions\_spawn\` coordination should connect as
 \`client.id: "gateway-client"\` with \`client.mode: "backend"\` over direct
 loopback shared-token/password auth; that path does not depend on the
 CLI's paired-device scope baseline. Remote callers, explicit
 \`deviceIdentity\`, explicit device-token paths, and browser/node clients
 still need normal device approval for scope upgrades.

\## Stopping

\\* Sending \`/stop\` in the requester chat aborts the requester session and stops any active sub-agent runs spawned from it, cascading to nested children.
\\* \`/subagents kill \` stops a specific sub-agent and cascades to its children.

\## Limitations

\\* Sub-agent announce is \*\*best-effort\*\*. If the gateway restarts, pending "announce back" work is lost.
\\* Sub-agents still share the same gateway process resources; treat \`maxConcurrent\` as a safety valve.
\\* \`sessions\_spawn\` is always non-blocking: it returns \`{ status: "accepted", runId, childSessionKey }\` immediately.
\\* Sub-agent context only injects \`AGENTS.md\` + \`TOOLS.md\` (no \`SOUL.md\`, \`IDENTITY.md\`, \`USER.md\`, \`HEARTBEAT.md\`, or \`BOOTSTRAP.md\`).
\\* Maximum nesting depth is 5 (\`maxSpawnDepth\` range: 1–5). Depth 2 is recommended for most use cases.
\\* \`maxChildrenPerAgent\` caps active children per session (default \`5\`, range \`1–20\`).

\## Related

\\* \[ACP agents\](/tools/acp-agents)
\\* \[Agent send\](/tools/agent-send)
\\* \[Background tasks\](/automation/tasks)
\\* \[Multi-agent sandbox tools\](/tools/multi-agent-sandbox-tools)