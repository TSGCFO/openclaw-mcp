---
source_url: https://docs.openclaw.ai/tools/loop-detection
title: "Tool-loop detection - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/loop-detection#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Tools

Tool-loop detection

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Why this exists](https://docs.openclaw.ai/tools/loop-detection#why-this-exists)
- [Configuration block](https://docs.openclaw.ai/tools/loop-detection#configuration-block)
- [Field behavior](https://docs.openclaw.ai/tools/loop-detection#field-behavior)
- [Recommended setup](https://docs.openclaw.ai/tools/loop-detection#recommended-setup)
- [Logs and expected behavior](https://docs.openclaw.ai/tools/loop-detection#logs-and-expected-behavior)
- [Notes](https://docs.openclaw.ai/tools/loop-detection#notes)
- [Related](https://docs.openclaw.ai/tools/loop-detection#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw can keep agents from getting stuck in repeated tool-call patterns.
The guard is **disabled by default**.Enable it only where needed, because it can block legitimate repeated calls with strict settings.

## [​](https://docs.openclaw.ai/tools/loop-detection\#why-this-exists)  Why this exists

- Detect repetitive sequences that do not make progress.
- Detect high-frequency no-result loops (same tool, same inputs, repeated errors).
- Detect specific repeated-call patterns for known polling tools.

## [​](https://docs.openclaw.ai/tools/loop-detection\#configuration-block)  Configuration block

Global defaults:

```
{
  tools: {
    loopDetection: {
      enabled: false,
      historySize: 30,
      warningThreshold: 10,
      criticalThreshold: 20,
      globalCircuitBreakerThreshold: 30,
      detectors: {
        genericRepeat: true,
        knownPollNoProgress: true,
        pingPong: true,
      },
    },
  },
}
```

Per-agent override (optional):

```
{
  agents: {
    list: [\
      {\
        id: "safe-runner",\
        tools: {\
          loopDetection: {\
            enabled: true,\
            warningThreshold: 8,\
            criticalThreshold: 16,\
          },\
        },\
      },\
    ],
  },
}
```

### [​](https://docs.openclaw.ai/tools/loop-detection\#field-behavior)  Field behavior

- `enabled`: Master switch. `false` means no loop detection is performed.
- `historySize`: number of recent tool calls kept for analysis.
- `warningThreshold`: threshold before classifying a pattern as warning-only.
- `criticalThreshold`: threshold for blocking repetitive loop patterns.
- `globalCircuitBreakerThreshold`: global no-progress breaker threshold.
- `detectors.genericRepeat`: detects repeated same-tool + same-params patterns.
- `detectors.knownPollNoProgress`: detects known polling-like patterns with no state change.
- `detectors.pingPong`: detects alternating ping-pong patterns.

For `exec`, no-progress checks compare stable command outcomes and ignore volatile runtime metadata such as duration, PID, session ID, and working directory.
When a run id is available, recent tool-call history is evaluated only within that run so scheduled heartbeat cycles and fresh runs do not inherit stale loop counts from earlier runs.

## [​](https://docs.openclaw.ai/tools/loop-detection\#recommended-setup)  Recommended setup

- Start with `enabled: true`, defaults unchanged.
- Keep thresholds ordered as `warningThreshold < criticalThreshold < globalCircuitBreakerThreshold`.
- If false positives occur:
  - raise `warningThreshold` and/or `criticalThreshold`
  - (optionally) raise `globalCircuitBreakerThreshold`
  - disable only the detector causing issues
  - reduce `historySize` for less strict historical context

## [​](https://docs.openclaw.ai/tools/loop-detection\#logs-and-expected-behavior)  Logs and expected behavior

When a loop is detected, OpenClaw reports a loop event and blocks or dampens the next tool-cycle depending on severity.
This protects users from runaway token spend and lockups while preserving normal tool access.

- Prefer warning and temporary suppression first.
- Escalate only when repeated evidence accumulates.

## [​](https://docs.openclaw.ai/tools/loop-detection\#notes)  Notes

- `tools.loopDetection` is merged with agent-level overrides.
- Per-agent config fully overrides or extends global values.
- If no config exists, guardrails stay off.

## [​](https://docs.openclaw.ai/tools/loop-detection\#related)  Related

- [Exec approvals](https://docs.openclaw.ai/tools/exec-approvals)
- [Thinking levels](https://docs.openclaw.ai/tools/thinking)
- [Sub-agents](https://docs.openclaw.ai/tools/subagents)

[Tokenjuice](https://docs.openclaw.ai/tools/tokenjuice) [Trajectory bundles](https://docs.openclaw.ai/tools/trajectory)

Ctrl+I