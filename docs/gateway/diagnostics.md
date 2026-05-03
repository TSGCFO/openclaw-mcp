---
source_url: https://docs.openclaw.ai/gateway/diagnostics
title: "Diagnostics export - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/diagnostics#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Health and diagnostics

Diagnostics export

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/gateway/diagnostics#quick-start)
- [Chat command](https://docs.openclaw.ai/gateway/diagnostics#chat-command)
- [What the export contains](https://docs.openclaw.ai/gateway/diagnostics#what-the-export-contains)
- [Privacy model](https://docs.openclaw.ai/gateway/diagnostics#privacy-model)
- [Stability recorder](https://docs.openclaw.ai/gateway/diagnostics#stability-recorder)
- [Useful options](https://docs.openclaw.ai/gateway/diagnostics#useful-options)
- [Disable diagnostics](https://docs.openclaw.ai/gateway/diagnostics#disable-diagnostics)
- [Related](https://docs.openclaw.ai/gateway/diagnostics#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw can create a local diagnostics zip for bug reports. It combines
sanitized Gateway status, health, logs, config shape, and recent payload-free
stability events.Treat diagnostics bundles like secrets until you have reviewed them. They are
designed to omit or redact payloads and credentials, but they still summarize
local Gateway logs and host-level runtime state.

## [​](https://docs.openclaw.ai/gateway/diagnostics\#quick-start)  Quick start

```
openclaw gateway diagnostics export
```

The command prints the written zip path. To choose a path:

```
openclaw gateway diagnostics export --output openclaw-diagnostics.zip
```

For automation:

```
openclaw gateway diagnostics export --json
```

## [​](https://docs.openclaw.ai/gateway/diagnostics\#chat-command)  Chat command

Owners can use `/diagnostics [note]` in chat to request a local Gateway export.
Use this when the bug happened in a real conversation and you want one
copy-pasteable report for support:

1. Send `/diagnostics` in the conversation where you noticed the problem. Add a
short note if it helps, for example `/diagnostics bad tool choice`.
2. OpenClaw sends the diagnostics preamble and asks for one explicit exec
approval. The approval runs `openclaw gateway diagnostics export --json`.
Do not approve diagnostics through an allow-all rule.
3. After approval, OpenClaw replies with a pasteable report containing the local
bundle path, manifest summary, privacy notes, and relevant session ids.

In group chats, an owner can still run `/diagnostics`, but OpenClaw does not
post the diagnostic details back into the shared chat. It sends the preamble,
approval prompts, Gateway export result, and Codex session/thread breakdown to
the owner through the private approval route. The group only gets a short notice
that the diagnostics flow was sent privately. If OpenClaw cannot find a private
owner route, the command fails closed and asks the owner to run it from a DM.When the active OpenClaw session is using the native OpenAI Codex harness,
the same exec approval also covers an OpenAI feedback upload for the Codex
runtime threads OpenClaw knows about. That upload is separate from the local
Gateway zip and appears only for Codex harness sessions. Before approval, the
prompt explains that approving diagnostics will also send Codex feedback, but it
does not list Codex session or thread ids. After approval, the chat reply lists
the channels, OpenClaw session ids, Codex thread ids, and local resume commands
for the threads that were sent to OpenAI servers. If you deny or ignore the
approval, OpenClaw does not run the export, does not send Codex feedback, and
does not print the Codex ids.That makes the common Codex debugging loop short: notice the bad behavior in
Telegram, Discord, or another channel, run `/diagnostics`, approve once, share
the report with support, then run the printed `codex resume <thread-id>` command
locally if you want to inspect the native Codex thread yourself. See
[Codex harness](https://docs.openclaw.ai/plugins/codex-harness#inspect-a-codex-thread-from-the-cli) for
that inspection workflow.

## [​](https://docs.openclaw.ai/gateway/diagnostics\#what-the-export-contains)  What the export contains

The zip includes:

- `summary.md`: human-readable overview for support.
- `diagnostics.json`: machine-readable summary of config, logs, status, health,
and stability data.
- `manifest.json`: export metadata and file list.
- Sanitized config shape and non-secret config details.
- Sanitized log summaries and recent redacted log lines.
- Best-effort Gateway status and health snapshots.
- `stability/latest.json`: newest persisted stability bundle, when available.

The export is useful even when the Gateway is unhealthy. If the Gateway cannot
answer status or health requests, the local logs, config shape, and latest
stability bundle are still collected when available.

## [​](https://docs.openclaw.ai/gateway/diagnostics\#privacy-model)  Privacy model

Diagnostics are designed to be shareable. The export keeps operational data
that helps debugging, such as:

- subsystem names, plugin ids, provider ids, channel ids, and configured modes
- status codes, durations, byte counts, queue state, and memory readings
- sanitized log metadata and redacted operational messages
- config shape and non-secret feature settings

The export omits or redacts:

- chat text, prompts, instructions, webhook bodies, and tool outputs
- credentials, API keys, tokens, cookies, and secret values
- raw request or response bodies
- account ids, message ids, raw session ids, hostnames, and local usernames

When a log message looks like user, chat, prompt, or tool payload text, the
export keeps only that a message was omitted and the byte count.

## [​](https://docs.openclaw.ai/gateway/diagnostics\#stability-recorder)  Stability recorder

The Gateway records a bounded, payload-free stability stream by default when
diagnostics are enabled. It is for operational facts, not content.The same diagnostic heartbeat records liveness samples when the Gateway keeps
running but the Node.js event loop or CPU looks saturated. These
`diagnostic.liveness.warning` events include event-loop delay, event-loop
utilization, CPU-core ratio, and active/waiting/queued session counts. Idle
samples stay in telemetry at `info` level; they are only logged as Gateway
warnings when diagnostic work is active, waiting, or queued. They do not
restart the Gateway by themselves.Inspect the live recorder:

```
openclaw gateway stability
openclaw gateway stability --type payload.large
openclaw gateway stability --json
```

Inspect the newest persisted stability bundle after a fatal exit, shutdown
timeout, or restart startup failure:

```
openclaw gateway stability --bundle latest
```

Create a diagnostics zip from the newest persisted bundle:

```
openclaw gateway stability --bundle latest --export
```

Persisted bundles live under `~/.openclaw/logs/stability/` when events exist.

## [​](https://docs.openclaw.ai/gateway/diagnostics\#useful-options)  Useful options

```
openclaw gateway diagnostics export \
  --output openclaw-diagnostics.zip \
  --log-lines 5000 \
  --log-bytes 1000000
```

- `--output <path>`: write to a specific zip path.
- `--log-lines <count>`: maximum sanitized log lines to include.
- `--log-bytes <bytes>`: maximum log bytes to inspect.
- `--url <url>`: Gateway WebSocket URL for status and health snapshots.
- `--token <token>`: Gateway token for status and health snapshots.
- `--password <password>`: Gateway password for status and health snapshots.
- `--timeout <ms>`: status and health snapshot timeout.
- `--no-stability-bundle`: skip persisted stability bundle lookup.
- `--json`: print machine-readable export metadata.

## [​](https://docs.openclaw.ai/gateway/diagnostics\#disable-diagnostics)  Disable diagnostics

Diagnostics are enabled by default. To disable the stability recorder and
diagnostic event collection:

```
{
  diagnostics: {
    enabled: false,
  },
}
```

Disabling diagnostics reduces bug-report detail. It does not affect normal
Gateway logging.

## [​](https://docs.openclaw.ai/gateway/diagnostics\#related)  Related

- [Health checks](https://docs.openclaw.ai/gateway/health)
- [Gateway CLI](https://docs.openclaw.ai/cli/gateway#gateway-diagnostics-export)
- [Gateway protocol](https://docs.openclaw.ai/gateway/protocol#system-and-identity)
- [Logging](https://docs.openclaw.ai/logging)
- [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) — separate flow for streaming diagnostics to a collector

[Gateway logging](https://docs.openclaw.ai/gateway/logging) [Troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)

Ctrl+I