---
source_url: https://docs.openclaw.ai/gateway/background-process
title: "Background exec and process tool - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/background-process#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Scaling and operations

Background exec and process tool

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Background Exec + Process Tool](https://docs.openclaw.ai/gateway/background-process#background-exec-%2B-process-tool)
- [exec tool](https://docs.openclaw.ai/gateway/background-process#exec-tool)
- [Child process bridging](https://docs.openclaw.ai/gateway/background-process#child-process-bridging)
- [process tool](https://docs.openclaw.ai/gateway/background-process#process-tool)
- [Examples](https://docs.openclaw.ai/gateway/background-process#examples)
- [Related](https://docs.openclaw.ai/gateway/background-process#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/gateway/background-process\#background-exec-+-process-tool)  Background Exec + Process Tool

OpenClaw runs shell commands through the `exec` tool and keeps long‑running tasks in memory. The `process` tool manages those background sessions.

## [​](https://docs.openclaw.ai/gateway/background-process\#exec-tool)  exec tool

Key parameters:

- `command` (required)
- `yieldMs` (default 10000): auto‑background after this delay
- `background` (bool): background immediately
- `timeout` (seconds, default `tools.exec.timeoutSec`): kill the process after this timeout; set `timeout: 0` only to disable the exec process timeout for that call
- `elevated` (bool): run outside the sandbox if elevated mode is enabled/allowed (`gateway` by default, or `node` when the exec target is `node`)
- Need a real TTY? Set `pty: true`.
- `workdir`, `env`

Behavior:

- Foreground runs return output directly.
- When backgrounded (explicit or timeout), the tool returns `status: "running"` \+ `sessionId` and a short tail.
- Background and `yieldMs` runs inherit `tools.exec.timeoutSec` unless the call provides an explicit `timeout`.
- Output is kept in memory until the session is polled or cleared.
- If the `process` tool is disallowed, `exec` runs synchronously and ignores `yieldMs`/`background`.
- Spawned exec commands receive `OPENCLAW_SHELL=exec` for context-aware shell/profile rules.
- For long-running work that starts now, start it once and rely on automatic
completion wake when it is enabled and the command emits output or fails.
- If automatic completion wake is unavailable, or you need quiet-success
confirmation for a command that exited cleanly without output, use `process`
to confirm completion.
- Do not emulate reminders or delayed follow-ups with `sleep` loops or repeated
polling; use cron for future work.

## [​](https://docs.openclaw.ai/gateway/background-process\#child-process-bridging)  Child process bridging

When spawning long-running child processes outside the exec/process tools (for example, CLI respawns or gateway helpers), attach the child-process bridge helper so termination signals are forwarded and listeners are detached on exit/error. This avoids orphaned processes on systemd and keeps shutdown behavior consistent across platforms.Environment overrides:

- `PI_BASH_YIELD_MS`: default yield (ms)
- `PI_BASH_MAX_OUTPUT_CHARS`: in‑memory output cap (chars)
- `OPENCLAW_BASH_PENDING_MAX_OUTPUT_CHARS`: pending stdout/stderr cap per stream (chars)
- `PI_BASH_JOB_TTL_MS`: TTL for finished sessions (ms, bounded to 1m–3h)

Config (preferred):

- `tools.exec.backgroundMs` (default 10000)
- `tools.exec.timeoutSec` (default 1800)
- `tools.exec.cleanupMs` (default 1800000)
- `tools.exec.notifyOnExit` (default true): enqueue a system event + request heartbeat when a backgrounded exec exits.
- `tools.exec.notifyOnExitEmptySuccess` (default false): when true, also enqueue completion events for successful backgrounded runs that produced no output.

## [​](https://docs.openclaw.ai/gateway/background-process\#process-tool)  process tool

Actions:

- `list`: running + finished sessions
- `poll`: drain new output for a session (also reports exit status)
- `log`: read the aggregated output (supports `offset` \+ `limit`)
- `write`: send stdin (`data`, optional `eof`)
- `send-keys`: send explicit key tokens or bytes to a PTY-backed session
- `submit`: send Enter / carriage return to a PTY-backed session
- `paste`: send literal text, optionally wrapped in bracketed paste mode
- `kill`: terminate a background session
- `clear`: remove a finished session from memory
- `remove`: kill if running, otherwise clear if finished

Notes:

- Only backgrounded sessions are listed/persisted in memory.
- Sessions are lost on process restart (no disk persistence).
- Session logs are only saved to chat history if you run `process poll/log` and the tool result is recorded.
- `process` is scoped per agent; it only sees sessions started by that agent.
- Use `poll` / `log` for status, logs, quiet-success confirmation, or
completion confirmation when automatic completion wake is unavailable.
- Use `write` / `send-keys` / `submit` / `paste` / `kill` when you need input
or intervention.
- `process list` includes a derived `name` (command verb + target) for quick scans.
- `process log` uses line-based `offset`/`limit`.
- When both `offset` and `limit` are omitted, it returns the last 200 lines and includes a paging hint.
- When `offset` is provided and `limit` is omitted, it returns from `offset` to the end (not capped to 200).
- Polling is for on-demand status, not wait-loop scheduling. If the work should
happen later, use cron instead.

## [​](https://docs.openclaw.ai/gateway/background-process\#examples)  Examples

Run a long task and poll later:

```
{ "tool": "exec", "command": "sleep 5 && echo done", "yieldMs": 1000 }
```

```
{ "tool": "process", "action": "poll", "sessionId": "<id>" }
```

Start immediately in background:

```
{ "tool": "exec", "command": "npm run build", "background": true }
```

Send stdin:

```
{ "tool": "process", "action": "write", "sessionId": "<id>", "data": "y\n" }
```

Send PTY keys:

```
{ "tool": "process", "action": "send-keys", "sessionId": "<id>", "keys": ["C-c"] }
```

Submit current line:

```
{ "tool": "process", "action": "submit", "sessionId": "<id>" }
```

Paste literal text:

```
{ "tool": "process", "action": "paste", "sessionId": "<id>", "text": "line1\nline2\n" }
```

## [​](https://docs.openclaw.ai/gateway/background-process\#related)  Related

- [Exec tool](https://docs.openclaw.ai/tools/exec)
- [Exec approvals](https://docs.openclaw.ai/tools/exec-approvals)

[Gateway lock](https://docs.openclaw.ai/gateway/gateway-lock) [Multiple gateways](https://docs.openclaw.ai/gateway/multiple-gateways)

Ctrl+I