---
source_url: https://docs.openclaw.ai/concepts/parallel-specialist-lanes
title: "Parallel specialist lanes - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/parallel-specialist-lanes#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Multi-agent

Parallel specialist lanes

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [First principles](https://docs.openclaw.ai/concepts/parallel-specialist-lanes#first-principles)
- [Recommended rollout](https://docs.openclaw.ai/concepts/parallel-specialist-lanes#recommended-rollout)
- [Phase 1: lane contracts + background heavy work](https://docs.openclaw.ai/concepts/parallel-specialist-lanes#phase-1-lane-contracts-%2B-background-heavy-work)
- [Phase 2: priority and concurrency controls](https://docs.openclaw.ai/concepts/parallel-specialist-lanes#phase-2-priority-and-concurrency-controls)
- [Phase 3: coordinator / traffic controller](https://docs.openclaw.ai/concepts/parallel-specialist-lanes#phase-3-coordinator-%2F-traffic-controller)
- [Minimal lane contract template](https://docs.openclaw.ai/concepts/parallel-specialist-lanes#minimal-lane-contract-template)
- [Related](https://docs.openclaw.ai/concepts/parallel-specialist-lanes#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Parallel specialist lanes let one Gateway route different chats or rooms to
different agents, while keeping the user experience fast. The trick is to treat
parallelism as a scarce-resource design problem, not just as “more agents”.

## [​](https://docs.openclaw.ai/concepts/parallel-specialist-lanes\#first-principles)  First principles

A specialist lane only improves throughput when it reduces contention for the
real bottlenecks:

- **Session locks**: only one run should mutate a given session at a time.
- **Global model capacity**: all visible chat runs still share provider limits.
- **Tool capacity**: shell, browser, network, and repository work can be slower
than the model turn itself.
- **Context budget**: long transcripts make every future turn slower and less
focused.
- **Ownership ambiguity**: duplicate agents doing the same job waste capacity.

OpenClaw already serializes runs per session and caps global parallelism through
the [command queue](https://docs.openclaw.ai/concepts/queue). Specialist lanes add policy on top:
which agent owns which work, what stays in chat, and what becomes background
work.

## [​](https://docs.openclaw.ai/concepts/parallel-specialist-lanes\#recommended-rollout)  Recommended rollout

### [​](https://docs.openclaw.ai/concepts/parallel-specialist-lanes\#phase-1-lane-contracts-+-background-heavy-work)  Phase 1: lane contracts + background heavy work

Give every lane a written contract in its workspace and system prompt:

- **Purpose**: the work this lane owns.
- **Non-goals**: work it should hand off instead of attempting.
- **Chat budget**: quick answers stay in chat; long tasks should acknowledge
briefly, then run in a background sub-agent or task.
- **Handoff rule**: when another lane owns the work, say where it should go and
provide a compact handoff summary.
- **Tool-risk rule**: prefer the smallest tool surface that can do the job.

This is the cheapest phase and fixes most clogging: one coding job no longer
turns the research lane into molasses, and each chat keeps its own context clean.

### [​](https://docs.openclaw.ai/concepts/parallel-specialist-lanes\#phase-2-priority-and-concurrency-controls)  Phase 2: priority and concurrency controls

Tune queue and model capacity around the business value of each lane:

```
{
  agents: {
    defaults: {
      maxConcurrent: 4,
      subagents: { maxConcurrent: 8 },
    },
  },
  messages: {
    queue: {
      mode: "collect",
      debounceMs: 1000,
      cap: 20,
      drop: "summarize",
    },
  },
}
```

Use direct/personal chats and production-ops agents for high-priority work. Let
research, drafting, and batch coding move to background tasks when the system is
busy.

### [​](https://docs.openclaw.ai/concepts/parallel-specialist-lanes\#phase-3-coordinator-/-traffic-controller)  Phase 3: coordinator / traffic controller

Add a small coordinator pattern once multiple lanes are active:

- Track active lane tasks and owners.
- Detect duplicate requests across groups.
- Route handoff summaries between lanes.
- Surface only blockers, completed results, and decisions the human must make.

Do not start here. A coordinator without lane contracts just coordinates chaos.

## [​](https://docs.openclaw.ai/concepts/parallel-specialist-lanes\#minimal-lane-contract-template)  Minimal lane contract template

```
# Lane contract

## Owns

- <job this lane is responsible for>

## Does not own

- <work to hand off>

## Chat budget

- Answer quick questions directly.
- For multi-step, slow, or tool-heavy work: acknowledge briefly, spawn/background
  the work, then return the result when complete.

## Handoff

If another lane owns the request, reply with:

- target lane
- objective
- relevant context
- exact next action

## Tool posture

Use the smallest tool surface that can complete the task. Avoid broad shell or
network work unless this lane explicitly owns it.
```

## [​](https://docs.openclaw.ai/concepts/parallel-specialist-lanes\#related)  Related

- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)
- [Command queue](https://docs.openclaw.ai/concepts/queue)
- [Sub-agents](https://docs.openclaw.ai/tools/subagents)

[Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent) [Presence](https://docs.openclaw.ai/concepts/presence)

Ctrl+I