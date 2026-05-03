---
source_url: https://docs.openclaw.ai/diagnostics/flags
title: "Diagnostics flags - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/diagnostics/flags#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Diagnostics

Diagnostics flags

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [How it works](https://docs.openclaw.ai/diagnostics/flags#how-it-works)
- [Enable via config](https://docs.openclaw.ai/diagnostics/flags#enable-via-config)
- [Env override (one-off)](https://docs.openclaw.ai/diagnostics/flags#env-override-one-off)
- [Timeline artifacts](https://docs.openclaw.ai/diagnostics/flags#timeline-artifacts)
- [Where logs go](https://docs.openclaw.ai/diagnostics/flags#where-logs-go)
- [Extract logs](https://docs.openclaw.ai/diagnostics/flags#extract-logs)
- [Notes](https://docs.openclaw.ai/diagnostics/flags#notes)
- [Related](https://docs.openclaw.ai/diagnostics/flags#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Diagnostics flags let you enable targeted debug logs without turning on verbose logging everywhere. Flags are opt-in and have no effect unless a subsystem checks them.

## [​](https://docs.openclaw.ai/diagnostics/flags\#how-it-works)  How it works

- Flags are strings (case-insensitive).
- You can enable flags in config or via an env override.
- Wildcards are supported:
  - `telegram.*` matches `telegram.http`
  - `*` enables all flags

## [​](https://docs.openclaw.ai/diagnostics/flags\#enable-via-config)  Enable via config

```
{
  "diagnostics": {
    "flags": ["telegram.http"]
  }
}
```

Multiple flags:

```
{
  "diagnostics": {
    "flags": ["telegram.http", "brave.http", "gateway.*"]
  }
}
```

Restart the gateway after changing flags.

## [​](https://docs.openclaw.ai/diagnostics/flags\#env-override-one-off)  Env override (one-off)

```
OPENCLAW_DIAGNOSTICS=telegram.http,telegram.payload
```

Disable all flags:

```
OPENCLAW_DIAGNOSTICS=0
```

## [​](https://docs.openclaw.ai/diagnostics/flags\#timeline-artifacts)  Timeline artifacts

The `timeline` flag writes structured startup and runtime timing events for
external QA harnesses:

```
OPENCLAW_DIAGNOSTICS=timeline \
OPENCLAW_DIAGNOSTICS_TIMELINE_PATH=/tmp/openclaw-timeline.jsonl \
openclaw gateway run
```

You can also enable it in config:

```
{
  "diagnostics": {
    "flags": ["timeline"]
  }
}
```

The timeline file path still comes from
`OPENCLAW_DIAGNOSTICS_TIMELINE_PATH`. When `timeline` is enabled only from
config, the earliest config-loading spans are not emitted because OpenClaw has
not read config yet; subsequent startup spans use the config flag.`OPENCLAW_DIAGNOSTICS=1`, `OPENCLAW_DIAGNOSTICS=all`, and
`OPENCLAW_DIAGNOSTICS=*` also enable the timeline because they enable every
diagnostics flag. Prefer `timeline` when you only want the JSONL timing
artifact.Timeline records use the `openclaw.diagnostics.v1` envelope. Events can include
process ids, phase names, span names, durations, plugin ids, dependency counts,
event-loop delay samples, provider operation names, child-process exit state,
and startup error names/messages. Treat timeline files as local diagnostics
artifacts; review them before sharing outside your machine.

## [​](https://docs.openclaw.ai/diagnostics/flags\#where-logs-go)  Where logs go

Flags emit logs into the standard diagnostics log file. By default:

```
/tmp/openclaw/openclaw-YYYY-MM-DD.log
```

If you set `logging.file`, use that path instead. Logs are JSONL (one JSON object per line). Redaction still applies based on `logging.redactSensitive`.

## [​](https://docs.openclaw.ai/diagnostics/flags\#extract-logs)  Extract logs

Pick the latest log file:

```
ls -t /tmp/openclaw/openclaw-*.log | head -n 1
```

Filter for Telegram HTTP diagnostics:

```
rg "telegram http error" /tmp/openclaw/openclaw-*.log
```

Filter for Brave Search HTTP diagnostics:

```
rg "brave http" /tmp/openclaw/openclaw-*.log
```

Or tail while reproducing:

```
tail -f /tmp/openclaw/openclaw-$(date +%F).log | rg "telegram http error"
```

For remote gateways, you can also use `openclaw logs --follow` (see [/cli/logs](https://docs.openclaw.ai/cli/logs)).

## [​](https://docs.openclaw.ai/diagnostics/flags\#notes)  Notes

- If `logging.level` is set higher than `warn`, these logs may be suppressed. Default `info` is fine.
- `brave.http` logs Brave Search request URLs/query params, response status/timing, and cache hit/miss/write events. It does not log API keys or response bodies, but search queries can be sensitive.
- Flags are safe to leave enabled; they only affect log volume for the specific subsystem.
- Use [/logging](https://docs.openclaw.ai/logging) to change log destinations, levels, and redaction.

## [​](https://docs.openclaw.ai/diagnostics/flags\#related)  Related

- [Gateway diagnostics](https://docs.openclaw.ai/gateway/diagnostics)
- [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)

[Environment variables](https://docs.openclaw.ai/help/environment) [Node + tsx crash](https://docs.openclaw.ai/debug/node-issue)

Ctrl+I