---
source_url: https://docs.openclaw.ai/web/tui
title: "TUI - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/web/tui#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web interfaces

TUI

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/web/tui#quick-start)
- [Gateway mode](https://docs.openclaw.ai/web/tui#gateway-mode)
- [Local mode](https://docs.openclaw.ai/web/tui#local-mode)
- [What you see](https://docs.openclaw.ai/web/tui#what-you-see)
- [Mental model: agents + sessions](https://docs.openclaw.ai/web/tui#mental-model-agents-%2B-sessions)
- [Sending + delivery](https://docs.openclaw.ai/web/tui#sending-%2B-delivery)
- [Pickers + overlays](https://docs.openclaw.ai/web/tui#pickers-%2B-overlays)
- [Keyboard shortcuts](https://docs.openclaw.ai/web/tui#keyboard-shortcuts)
- [Slash commands](https://docs.openclaw.ai/web/tui#slash-commands)
- [Local shell commands](https://docs.openclaw.ai/web/tui#local-shell-commands)
- [Repair configs from the local TUI](https://docs.openclaw.ai/web/tui#repair-configs-from-the-local-tui)
- [Tool output](https://docs.openclaw.ai/web/tui#tool-output)
- [Terminal colors](https://docs.openclaw.ai/web/tui#terminal-colors)
- [History + streaming](https://docs.openclaw.ai/web/tui#history-%2B-streaming)
- [Connection details](https://docs.openclaw.ai/web/tui#connection-details)
- [Options](https://docs.openclaw.ai/web/tui#options)
- [Troubleshooting](https://docs.openclaw.ai/web/tui#troubleshooting)
- [Connection troubleshooting](https://docs.openclaw.ai/web/tui#connection-troubleshooting)
- [Related](https://docs.openclaw.ai/web/tui#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

## [​](https://docs.openclaw.ai/web/tui\#quick-start)  Quick start

### [​](https://docs.openclaw.ai/web/tui\#gateway-mode)  Gateway mode

1. Start the Gateway.

```
openclaw gateway
```

2. Open the TUI.

```
openclaw tui
```

3. Type a message and press Enter.

Remote Gateway:

```
openclaw tui --url ws://<host>:<port> --token <gateway-token>
```

Use `--password` if your Gateway uses password auth.

### [​](https://docs.openclaw.ai/web/tui\#local-mode)  Local mode

Run the TUI without a Gateway:

```
openclaw chat
# or
openclaw tui --local
```

Notes:

- `openclaw chat` and `openclaw terminal` are aliases for `openclaw tui --local`.
- `--local` cannot be combined with `--url`, `--token`, or `--password`.
- Local mode uses the embedded agent runtime directly. Most local tools work, but Gateway-only features are unavailable.
- `openclaw` and `openclaw crestodian` also use this TUI shell, with Crestodian as the local setup and repair chat backend.

## [​](https://docs.openclaw.ai/web/tui\#what-you-see)  What you see

- Header: connection URL, current agent, current session.
- Chat log: user messages, assistant replies, system notices, tool cards.
- Status line: connection/run state (connecting, running, streaming, idle, error).
- Footer: connection state + agent + session + model + think/fast/verbose/trace/reasoning + token counts + deliver.
- Input: text editor with autocomplete.

## [​](https://docs.openclaw.ai/web/tui\#mental-model-agents-+-sessions)  Mental model: agents + sessions

- Agents are unique slugs (e.g. `main`, `research`). The Gateway exposes the list.
- Sessions belong to the current agent.
- Session keys are stored as `agent:<agentId>:<sessionKey>`.

  - If you type `/session main`, the TUI expands it to `agent:<currentAgent>:main`.
  - If you type `/session agent:other:main`, you switch to that agent session explicitly.
- Session scope:
  - `per-sender` (default): each agent has many sessions.
  - `global`: the TUI always uses the `global` session (the picker may be empty).
- The current agent + session are always visible in the footer.
- When started without `--session`, gateway-mode TUI resumes the last selected session for the same gateway, agent, and session scope if that session still exists. Passing `--session`, `/session`, `/new`, or `/reset` remains explicit.

## [​](https://docs.openclaw.ai/web/tui\#sending-+-delivery)  Sending + delivery

- Messages are sent to the Gateway; delivery to providers is off by default.
- Turn delivery on:
  - `/deliver on`
  - or the Settings panel
  - or start with `openclaw tui --deliver`

## [​](https://docs.openclaw.ai/web/tui\#pickers-+-overlays)  Pickers + overlays

- Model picker: list available models and set the session override.
- Agent picker: choose a different agent.
- Session picker: shows only sessions for the current agent.
- Settings: toggle deliver, tool output expansion, and thinking visibility.

## [​](https://docs.openclaw.ai/web/tui\#keyboard-shortcuts)  Keyboard shortcuts

- Enter: send message
- Esc: abort active run
- Ctrl+C: clear input (press twice to exit)
- Ctrl+D: exit
- Ctrl+L: model picker
- Ctrl+G: agent picker
- Ctrl+P: session picker
- Ctrl+O: toggle tool output expansion
- Ctrl+T: toggle thinking visibility (reloads history)

## [​](https://docs.openclaw.ai/web/tui\#slash-commands)  Slash commands

Core:

- `/help`
- `/status`
- `/agent <id>` (or `/agents`)
- `/session <key>` (or `/sessions`)
- `/model <provider/model>` (or `/models`)

Session controls:

- `/think <off|minimal|low|medium|high>`
- `/fast <status|on|off>`
- `/verbose <on|full|off>`
- `/trace <on|off>`
- `/reasoning <on|off|stream>`
- `/usage <off|tokens|full>`
- `/elevated <on|off|ask|full>` (alias: `/elev`)
- `/activation <mention|always>`
- `/deliver <on|off>`

Session lifecycle:

- `/new` or `/reset` (reset the session)
- `/abort` (abort the active run)
- `/settings`
- `/exit`

Local mode only:

- `/auth [provider]` opens the provider auth/login flow inside the TUI.

Other Gateway slash commands (for example, `/context`) are forwarded to the Gateway and shown as system output. See [Slash commands](https://docs.openclaw.ai/tools/slash-commands).

## [​](https://docs.openclaw.ai/web/tui\#local-shell-commands)  Local shell commands

- Prefix a line with `!` to run a local shell command on the TUI host.
- The TUI prompts once per session to allow local execution; declining keeps `!` disabled for the session.
- Commands run in a fresh, non-interactive shell in the TUI working directory (no persistent `cd`/env).
- Local shell commands receive `OPENCLAW_SHELL=tui-local` in their environment.
- A lone `!` is sent as a normal message; leading spaces do not trigger local exec.

## [​](https://docs.openclaw.ai/web/tui\#repair-configs-from-the-local-tui)  Repair configs from the local TUI

Use local mode when the current config already validates and you want the
embedded agent to inspect it on the same machine, compare it against the docs,
and help repair drift without depending on a running Gateway.If `openclaw config validate` is already failing, start with `openclaw configure`
or `openclaw doctor --fix` first. `openclaw chat` does not bypass the invalid-
config guard.Typical loop:

1. Start local mode:

```
openclaw chat
```

2. Ask the agent what you want checked, for example:

```
Compare my gateway auth config with the docs and suggest the smallest fix.
```

3. Use local shell commands for exact evidence and validation:

```
!openclaw config file
!openclaw docs gateway auth token secretref
!openclaw config validate
!openclaw doctor
```

4. Apply narrow changes with `openclaw config set` or `openclaw configure`, then rerun `!openclaw config validate`.
5. If Doctor recommends an automatic migration or repair, review it and run `!openclaw doctor --fix`.

Tips:

- Prefer `openclaw config set` or `openclaw configure` over hand-editing `openclaw.json`.
- `openclaw docs "<query>"` searches the live docs index from the same machine.
- `openclaw config validate --json` is useful when you want structured schema and SecretRef/resolvability errors.

## [​](https://docs.openclaw.ai/web/tui\#tool-output)  Tool output

- Tool calls show as cards with args + results.
- Ctrl+O toggles between collapsed/expanded views.
- While tools run, partial updates stream into the same card.

## [​](https://docs.openclaw.ai/web/tui\#terminal-colors)  Terminal colors

- The TUI keeps assistant body text in your terminal’s default foreground so dark and light terminals both stay readable.
- If your terminal uses a light background and auto-detection is wrong, set `OPENCLAW_THEME=light` before launching `openclaw tui`.
- To force the original dark palette instead, set `OPENCLAW_THEME=dark`.

## [​](https://docs.openclaw.ai/web/tui\#history-+-streaming)  History + streaming

- On connect, the TUI loads the latest history (default 200 messages).
- Streaming responses update in place until finalized.
- The TUI also listens to agent tool events for richer tool cards.

## [​](https://docs.openclaw.ai/web/tui\#connection-details)  Connection details

- The TUI registers with the Gateway as `mode: "tui"`.
- Reconnects show a system message; event gaps are surfaced in the log.

## [​](https://docs.openclaw.ai/web/tui\#options)  Options

- `--local`: Run against the local embedded agent runtime
- `--url <url>`: Gateway WebSocket URL (defaults to config or `ws://127.0.0.1:<port>`)
- `--token <token>`: Gateway token (if required)
- `--password <password>`: Gateway password (if required)
- `--session <key>`: Session key (default: `main`, or `global` when scope is global)
- `--deliver`: Deliver assistant replies to the provider (default off)
- `--thinking <level>`: Override thinking level for sends
- `--message <text>`: Send an initial message after connecting
- `--timeout-ms <ms>`: Agent timeout in ms (defaults to `agents.defaults.timeoutSeconds`)
- `--history-limit <n>`: History entries to load (default `200`)

When you set `--url`, the TUI does not fall back to config or environment credentials. Pass `--token` or `--password` explicitly. Missing explicit credentials is an error. In local mode, do not pass `--url`, `--token`, or `--password`.

## [​](https://docs.openclaw.ai/web/tui\#troubleshooting)  Troubleshooting

No output after sending a message:

- Run `/status` in the TUI to confirm the Gateway is connected and idle/busy.
- Check the Gateway logs: `openclaw logs --follow`.
- Confirm the agent can run: `openclaw status` and `openclaw models status`.
- If you expect messages in a chat channel, enable delivery (`/deliver on` or `--deliver`).

## [​](https://docs.openclaw.ai/web/tui\#connection-troubleshooting)  Connection troubleshooting

- `disconnected`: ensure the Gateway is running and your `--url/--token/--password` are correct.
- No agents in picker: check `openclaw agents list` and your routing config.
- Empty session picker: you might be in global scope or have no sessions yet.

## [​](https://docs.openclaw.ai/web/tui\#related)  Related

- [Control UI](https://docs.openclaw.ai/web/control-ui) — web-based control interface
- [Config](https://docs.openclaw.ai/cli/config) — inspect, validate, and edit `openclaw.json`
- [Doctor](https://docs.openclaw.ai/cli/doctor) — guided repair and migration checks
- [CLI Reference](https://docs.openclaw.ai/cli) — full CLI command reference

[WebChat](https://docs.openclaw.ai/web/webchat)

Ctrl+I