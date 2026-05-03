---
source_url: https://docs.openclaw.ai/cli/agent
title: "Agent - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/agent#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Agents and sessions

Agent

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw agent](https://docs.openclaw.ai/cli/agent#openclaw-agent)
- [Options](https://docs.openclaw.ai/cli/agent#options)
- [Examples](https://docs.openclaw.ai/cli/agent#examples)
- [Notes](https://docs.openclaw.ai/cli/agent#notes)
- [Related](https://docs.openclaw.ai/cli/agent#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/agent\#openclaw-agent)  `openclaw agent`

Run an agent turn via the Gateway (use `--local` for embedded).
Use `--agent <id>` to target a configured agent directly.Pass at least one session selector:

- `--to <dest>`
- `--session-id <id>`
- `--agent <id>`

Related:

- Agent send tool: [Agent send](https://docs.openclaw.ai/tools/agent-send)

## [​](https://docs.openclaw.ai/cli/agent\#options)  Options

- `-m, --message <text>`: required message body
- `-t, --to <dest>`: recipient used to derive the session key
- `--session-id <id>`: explicit session id
- `--agent <id>`: agent id; overrides routing bindings
- `--model <id>`: model override for this run (`provider/model` or model id)
- `--thinking <level>`: agent thinking level (`off`, `minimal`, `low`, `medium`, `high`, plus provider-supported custom levels such as `xhigh`, `adaptive`, or `max`)
- `--verbose <on|off>`: persist verbose level for the session
- `--channel <channel>`: delivery channel; omit to use the main session channel
- `--reply-to <target>`: delivery target override
- `--reply-channel <channel>`: delivery channel override
- `--reply-account <id>`: delivery account override
- `--local`: run the embedded agent directly (after plugin registry preload)
- `--deliver`: send the reply back to the selected channel/target
- `--timeout <seconds>`: override agent timeout (default 600 or config value)
- `--json`: output JSON

## [​](https://docs.openclaw.ai/cli/agent\#examples)  Examples

```
openclaw agent --to +15555550123 --message "status update" --deliver
openclaw agent --agent ops --message "Summarize logs"
openclaw agent --agent ops --model openai/gpt-5.4 --message "Summarize logs"
openclaw agent --session-id 1234 --message "Summarize inbox" --thinking medium
openclaw agent --to +15555550123 --message "Trace logs" --verbose on --json
openclaw agent --agent ops --message "Generate report" --deliver --reply-channel slack --reply-to "#reports"
openclaw agent --agent ops --message "Run locally" --local
```

## [​](https://docs.openclaw.ai/cli/agent\#notes)  Notes

- Gateway mode falls back to the embedded agent when the Gateway request fails. Use `--local` to force embedded execution up front.
- `--local` still preloads the plugin registry first, so plugin-provided providers, tools, and channels stay available during embedded runs.
- `--local` and embedded fallback runs are treated as one-shot runs. Bundled MCP loopback resources and warm Claude stdio sessions opened for that local process are retired after the reply, so scripted invocations do not keep local child processes alive.
- Gateway-backed runs leave Gateway-owned MCP loopback resources under the running Gateway process; older clients may still send the historical cleanup flag, but the Gateway accepts it as a compatibility no-op.
- `--channel`, `--reply-channel`, and `--reply-account` affect reply delivery, not session routing.
- `--json` keeps stdout reserved for the JSON response. Gateway, plugin, and embedded-fallback diagnostics are routed to stderr so scripts can parse stdout directly.
- Embedded fallback JSON includes `meta.transport: "embedded"` and `meta.fallbackFrom: "gateway"` so scripts can distinguish fallback runs from Gateway runs.
- If the Gateway accepts an agent run but the CLI times out waiting for the final reply, embedded fallback uses a fresh explicit `gateway-fallback-*` session/run id and reports `meta.fallbackReason: "gateway_timeout"` plus the fallback session fields. This avoids racing the Gateway-owned transcript lock or silently replacing the original routed conversation session.
- When this command triggers `models.json` regeneration, SecretRef-managed provider credentials are persisted as non-secret markers (for example env var names, `secretref-env:ENV_VAR_NAME`, or `secretref-managed`), not resolved secret plaintext.
- Marker writes are source-authoritative: OpenClaw persists markers from the active source config snapshot, not from resolved runtime secret values.

## [​](https://docs.openclaw.ai/cli/agent\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Agent runtime](https://docs.openclaw.ai/concepts/agent)

[Update](https://docs.openclaw.ai/cli/update) [Agents](https://docs.openclaw.ai/cli/agents)

Ctrl+I