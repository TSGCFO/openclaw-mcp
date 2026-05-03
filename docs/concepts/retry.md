---
source_url: https://docs.openclaw.ai/concepts/retry
title: "Retry policy - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/retry#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Messages and delivery

Retry policy

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Goals](https://docs.openclaw.ai/concepts/retry#goals)
- [Defaults](https://docs.openclaw.ai/concepts/retry#defaults)
- [Behavior](https://docs.openclaw.ai/concepts/retry#behavior)
- [Model providers](https://docs.openclaw.ai/concepts/retry#model-providers)
- [Discord](https://docs.openclaw.ai/concepts/retry#discord)
- [Telegram](https://docs.openclaw.ai/concepts/retry#telegram)
- [Configuration](https://docs.openclaw.ai/concepts/retry#configuration)
- [Notes](https://docs.openclaw.ai/concepts/retry#notes)
- [Related](https://docs.openclaw.ai/concepts/retry#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

## [​](https://docs.openclaw.ai/concepts/retry\#goals)  Goals

- Retry per HTTP request, not per multi-step flow.
- Preserve ordering by retrying only the current step.
- Avoid duplicating non-idempotent operations.

## [​](https://docs.openclaw.ai/concepts/retry\#defaults)  Defaults

- Attempts: 3
- Max delay cap: 30000 ms
- Jitter: 0.1 (10 percent)
- Provider defaults:
  - Telegram min delay: 400 ms
  - Discord min delay: 500 ms

## [​](https://docs.openclaw.ai/concepts/retry\#behavior)  Behavior

### [​](https://docs.openclaw.ai/concepts/retry\#model-providers)  Model providers

- OpenClaw lets provider SDKs handle normal short retries.
- For Stainless-based SDKs such as Anthropic and OpenAI, retryable responses
(`408`, `409`, `429`, and `5xx`) can include `retry-after-ms` or
`retry-after`. When that wait is longer than 60 seconds, OpenClaw injects
`x-should-retry: false` so the SDK surfaces the error immediately and model
failover can rotate to another auth profile or fallback model.
- Override the cap with `OPENCLAW_SDK_RETRY_MAX_WAIT_SECONDS=<seconds>`.
Set it to `0`, `false`, `off`, `none`, or `disabled` to let SDKs honor long
`Retry-After` sleeps internally.

### [​](https://docs.openclaw.ai/concepts/retry\#discord)  Discord

- Retries on rate-limit errors (HTTP 429), request timeouts, HTTP 5xx responses,
and transient transport failures such as DNS lookup failures, connection
resets, socket closes, and fetch failures.
- Uses Discord `retry_after` when available, otherwise exponential backoff.

### [​](https://docs.openclaw.ai/concepts/retry\#telegram)  Telegram

- Retries on transient errors (429, timeout, connect/reset/closed, temporarily unavailable).
- Uses `retry_after` when available, otherwise exponential backoff.
- Markdown parse errors are not retried; they fall back to plain text.

## [​](https://docs.openclaw.ai/concepts/retry\#configuration)  Configuration

Set retry policy per provider in `~/.openclaw/openclaw.json`:

```
{
  channels: {
    telegram: {
      retry: {
        attempts: 3,
        minDelayMs: 400,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
    },
    discord: {
      retry: {
        attempts: 3,
        minDelayMs: 500,
        maxDelayMs: 30000,
        jitter: 0.1,
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/concepts/retry\#notes)  Notes

- Retries apply per request (message send, media upload, reaction, poll, sticker).
- Composite flows do not retry completed steps.

## [​](https://docs.openclaw.ai/concepts/retry\#related)  Related

- [Model failover](https://docs.openclaw.ai/concepts/model-failover)
- [Command queue](https://docs.openclaw.ai/concepts/queue)

[Streaming and chunking](https://docs.openclaw.ai/concepts/streaming) [Command queue](https://docs.openclaw.ai/concepts/queue)

Ctrl+I