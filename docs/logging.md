---
source_url: https://docs.openclaw.ai/logging
title: "Logging - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/logging#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Health and diagnostics

Logging

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Where logs live](https://docs.openclaw.ai/logging#where-logs-live)
- [How to read logs](https://docs.openclaw.ai/logging#how-to-read-logs)
- [CLI: live tail (recommended)](https://docs.openclaw.ai/logging#cli-live-tail-recommended)
- [Control UI (web)](https://docs.openclaw.ai/logging#control-ui-web)
- [Channel-only logs](https://docs.openclaw.ai/logging#channel-only-logs)
- [Log formats](https://docs.openclaw.ai/logging#log-formats)
- [File logs (JSONL)](https://docs.openclaw.ai/logging#file-logs-jsonl)
- [Console output](https://docs.openclaw.ai/logging#console-output)
- [Gateway WebSocket logs](https://docs.openclaw.ai/logging#gateway-websocket-logs)
- [Configuring logging](https://docs.openclaw.ai/logging#configuring-logging)
- [Log levels](https://docs.openclaw.ai/logging#log-levels)
- [Trace correlation](https://docs.openclaw.ai/logging#trace-correlation)
- [Model call size and timing](https://docs.openclaw.ai/logging#model-call-size-and-timing)
- [Console styles](https://docs.openclaw.ai/logging#console-styles)
- [Redaction](https://docs.openclaw.ai/logging#redaction)
- [Diagnostics and OpenTelemetry](https://docs.openclaw.ai/logging#diagnostics-and-opentelemetry)
- [Troubleshooting tips](https://docs.openclaw.ai/logging#troubleshooting-tips)
- [Related](https://docs.openclaw.ai/logging#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw has two main log surfaces:

- **File logs** (JSON lines) written by the Gateway.
- **Console output** shown in terminals and the Gateway Debug UI.

The Control UI **Logs** tab tails the gateway file log. This page explains where
logs live, how to read them, and how to configure log levels and formats.

## [​](https://docs.openclaw.ai/logging\#where-logs-live)  Where logs live

By default, the Gateway writes a rolling log file under:`/tmp/openclaw/openclaw-YYYY-MM-DD.log`The date uses the gateway host’s local timezone.Each file rotates when it reaches `logging.maxFileBytes` (default: 100 MB).
OpenClaw keeps up to five numbered archives beside the active file, such as
`openclaw-YYYY-MM-DD.1.log`, and keeps writing to a fresh active log instead of
suppressing diagnostics.You can override this in `~/.openclaw/openclaw.json`:

```
{
  "logging": {
    "file": "/path/to/openclaw.log"
  }
}
```

## [​](https://docs.openclaw.ai/logging\#how-to-read-logs)  How to read logs

### [​](https://docs.openclaw.ai/logging\#cli-live-tail-recommended)  CLI: live tail (recommended)

Use the CLI to tail the gateway log file via RPC:

```
openclaw logs --follow
```

Useful current options:

- `--local-time`: render timestamps in your local timezone
- `--url <url>` / `--token <token>` / `--timeout <ms>`: standard Gateway RPC flags
- `--expect-final`: agent-backed RPC final-response wait flag (accepted here via the shared client layer)

Output modes:

- **TTY sessions**: pretty, colorized, structured log lines.
- **Non-TTY sessions**: plain text.
- `--json`: line-delimited JSON (one log event per line).
- `--plain`: force plain text in TTY sessions.
- `--no-color`: disable ANSI colors.

When you pass an explicit `--url`, the CLI does not auto-apply config or
environment credentials; include `--token` yourself if the target Gateway
requires auth.In JSON mode, the CLI emits `type`-tagged objects:

- `meta`: stream metadata (file, cursor, size)
- `log`: parsed log entry
- `notice`: truncation / rotation hints
- `raw`: unparsed log line

If the implicit local loopback Gateway asks for pairing, closes during connect,
or times out before `logs.tail` answers, `openclaw logs` falls back to the
configured Gateway file log automatically. Explicit `--url` targets do not use
this fallback.If the Gateway is unreachable, the CLI prints a short hint to run:

```
openclaw doctor
```

### [​](https://docs.openclaw.ai/logging\#control-ui-web)  Control UI (web)

The Control UI’s **Logs** tab tails the same file using `logs.tail`.
See [/web/control-ui](https://docs.openclaw.ai/web/control-ui) for how to open it.

### [​](https://docs.openclaw.ai/logging\#channel-only-logs)  Channel-only logs

To filter channel activity (WhatsApp/Telegram/etc), use:

```
openclaw channels logs --channel whatsapp
```

## [​](https://docs.openclaw.ai/logging\#log-formats)  Log formats

### [​](https://docs.openclaw.ai/logging\#file-logs-jsonl)  File logs (JSONL)

Each line in the log file is a JSON object. The CLI and Control UI parse these
entries to render structured output (time, level, subsystem, message).File-log JSONL records also include machine-filterable top-level fields when
available:

- `hostname`: gateway host name.
- `message`: flattened log message text for full-text search.
- `agent_id`: active agent id when the log call carries agent context.
- `session_id`: active session id/key when the log call carries session context.
- `channel`: active channel when the log call carries channel context.

OpenClaw preserves the original structured log arguments alongside these fields
so existing parsers that read numbered tslog argument keys keep working.

### [​](https://docs.openclaw.ai/logging\#console-output)  Console output

Console logs are **TTY-aware** and formatted for readability:

- Subsystem prefixes (e.g. `gateway/channels/whatsapp`)
- Level coloring (info/warn/error)
- Optional compact or JSON mode

Console formatting is controlled by `logging.consoleStyle`.

### [​](https://docs.openclaw.ai/logging\#gateway-websocket-logs)  Gateway WebSocket logs

`openclaw gateway` also has WebSocket protocol logging for RPC traffic:

- normal mode: only interesting results (errors, parse errors, slow calls)
- `--verbose`: all request/response traffic
- `--ws-log auto|compact|full`: pick the verbose rendering style
- `--compact`: alias for `--ws-log compact`

Examples:

```
openclaw gateway
openclaw gateway --verbose --ws-log compact
openclaw gateway --verbose --ws-log full
```

## [​](https://docs.openclaw.ai/logging\#configuring-logging)  Configuring logging

All logging configuration lives under `logging` in `~/.openclaw/openclaw.json`.

```
{
  "logging": {
    "level": "info",
    "file": "/tmp/openclaw/openclaw-YYYY-MM-DD.log",
    "consoleLevel": "info",
    "consoleStyle": "pretty",
    "redactSensitive": "tools",
    "redactPatterns": ["sk-.*"]
  }
}
```

### [​](https://docs.openclaw.ai/logging\#log-levels)  Log levels

- `logging.level`: **file logs** (JSONL) level.
- `logging.consoleLevel`: **console** verbosity level.

You can override both via the **`OPENCLAW_LOG_LEVEL`** environment variable (e.g. `OPENCLAW_LOG_LEVEL=debug`). The env var takes precedence over the config file, so you can raise verbosity for a single run without editing `openclaw.json`. You can also pass the global CLI option **`--log-level <level>`** (for example, `openclaw --log-level debug gateway run`), which overrides the environment variable for that command.`--verbose` only affects console output and WS log verbosity; it does not change
file log levels.

### [​](https://docs.openclaw.ai/logging\#trace-correlation)  Trace correlation

File logs are JSONL. When a log call carries a valid diagnostic trace context,
OpenClaw writes the trace fields as top-level JSON keys (`traceId`, `spanId`,
`parentSpanId`, `traceFlags`) so external log processors can correlate the line
with OTEL spans and provider `traceparent` propagation.Gateway HTTP requests and Gateway WebSocket frames establish an internal request
trace scope. Logs and diagnostic events emitted inside that async scope inherit
the request trace when they do not pass an explicit trace context. Agent run and
model-call traces become children of the active request trace, so local logs,
diagnostic snapshots, OTEL spans, and trusted provider `traceparent` headers can
be joined by `traceId` without logging raw request or model content.

### [​](https://docs.openclaw.ai/logging\#model-call-size-and-timing)  Model call size and timing

Model-call diagnostics record bounded request/response measurements without
capturing raw prompt or response content:

- `requestPayloadBytes`: UTF-8 byte size of the final model request payload
- `responseStreamBytes`: UTF-8 byte size of streamed model response events
- `timeToFirstByteMs`: elapsed time before the first streamed response event
- `durationMs`: total model-call duration

These fields are available to diagnostic snapshots, model-call plugin hooks, and
OTEL model-call spans/metrics when diagnostics export is enabled.

### [​](https://docs.openclaw.ai/logging\#console-styles)  Console styles

`logging.consoleStyle`:

- `pretty`: human-friendly, colored, with timestamps.
- `compact`: tighter output (best for long sessions).
- `json`: JSON per line (for log processors).

### [​](https://docs.openclaw.ai/logging\#redaction)  Redaction

OpenClaw can redact sensitive tokens before they hit console output, file logs,
OTLP log records, persisted session transcript text, or Control UI tool
event payloads (tool start args, partial/final result payloads, derived
exec output, and patch summaries):

- `logging.redactSensitive`: `off` \| `tools` (default: `tools`)
- `logging.redactPatterns`: list of regex strings to override the default set. Custom patterns apply on top of the built-in defaults for Control UI tool payloads, so adding a pattern never weakens redaction of values already caught by the defaults.

File logs and session transcripts stay JSONL, but matching secret values are
masked before the line or message is written to disk. Redaction is best-effort:
it applies to text-bearing message content and log strings, not every
identifier or binary payload field.The built-in defaults cover common API credentials and payment-credential field
names such as card number, CVC/CVV, shared payment token, and payment credential
when they appear as JSON fields, URL parameters, CLI flags, or assignments.`logging.redactSensitive: "off"` only disables this general log/transcript
policy. OpenClaw still redacts safety-boundary payloads that can be shown to UI
clients, support bundles, diagnostics observers, approval prompts, or agent
tools. Examples include Control UI tool-call events, `sessions_history` output,
diagnostics support exports, provider error observations, exec approval command
display, and Gateway WebSocket protocol logs. Custom `logging.redactPatterns`
can still add project-specific patterns on those surfaces.

## [​](https://docs.openclaw.ai/logging\#diagnostics-and-opentelemetry)  Diagnostics and OpenTelemetry

Diagnostics are structured, machine-readable events for model runs and
message-flow telemetry (webhooks, queueing, session state). They do **not**
replace logs — they feed metrics, traces, and exporters. Events are emitted
in-process whether or not you export them.Two adjacent surfaces:

- **OpenTelemetry export** — send metrics, traces, and logs over OTLP/HTTP to
any OpenTelemetry-compatible collector or backend (Grafana, Datadog,
Honeycomb, New Relic, Tempo, etc.). Full configuration, signal catalog,
metric/span names, env vars, and privacy model live on a dedicated page:
[OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry).
- **Diagnostics flags** — targeted debug-log flags that route extra logs to
`logging.file` without raising `logging.level`. Flags are case-insensitive
and support wildcards (`telegram.*`, `*`). Configure under `diagnostics.flags`
or via the `OPENCLAW_DIAGNOSTICS=...` env override. Full guide:
[Diagnostics flags](https://docs.openclaw.ai/diagnostics/flags).

To enable diagnostics events for plugins or custom sinks without OTLP export:

```
{
  diagnostics: { enabled: true },
}
```

For OTLP export to a collector, see [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry).

## [​](https://docs.openclaw.ai/logging\#troubleshooting-tips)  Troubleshooting tips

- **Gateway not reachable?** Run `openclaw doctor` first.
- **Logs empty?** Check that the Gateway is running and writing to the file path
in `logging.file`.
- **Need more detail?** Set `logging.level` to `debug` or `trace` and retry.

## [​](https://docs.openclaw.ai/logging\#related)  Related

- [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry) — OTLP/HTTP export, metric/span catalog, privacy model
- [Diagnostics flags](https://docs.openclaw.ai/diagnostics/flags) — targeted debug-log flags
- [Gateway logging internals](https://docs.openclaw.ai/gateway/logging) — WS log styles, subsystem prefixes, and console capture
- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference#diagnostics) — full `diagnostics.*` field reference

[Doctor](https://docs.openclaw.ai/gateway/doctor) [OpenTelemetry export](https://docs.openclaw.ai/gateway/opentelemetry)

Ctrl+I