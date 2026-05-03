---
source_url: https://docs.openclaw.ai/concepts/commitments
title: "Inferred commitments - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/commitments#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Memory

Inferred commitments

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Enable commitments](https://docs.openclaw.ai/concepts/commitments#enable-commitments)
- [How it works](https://docs.openclaw.ai/concepts/commitments#how-it-works)
- [Scope](https://docs.openclaw.ai/concepts/commitments#scope)
- [Commitments vs reminders](https://docs.openclaw.ai/concepts/commitments#commitments-vs-reminders)
- [Manage commitments](https://docs.openclaw.ai/concepts/commitments#manage-commitments)
- [Privacy and cost](https://docs.openclaw.ai/concepts/commitments#privacy-and-cost)
- [Troubleshooting](https://docs.openclaw.ai/concepts/commitments#troubleshooting)
- [Related](https://docs.openclaw.ai/concepts/commitments#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Commitments are short-lived follow-up memories. When enabled, OpenClaw can
notice that a conversation created a future check-in opportunity and remember
to bring it back later.Examples:

- You mention an interview tomorrow. OpenClaw may check in afterward.
- You say you are exhausted. OpenClaw may ask later whether you slept.
- The agent says it will follow up after something changes. OpenClaw may track
that open loop.

Commitments are not durable facts like `MEMORY.md`, and they are not exact
reminders. They sit between memory and automation: OpenClaw remembers a
conversation-bound obligation, then heartbeat delivers it when it is due.

## [​](https://docs.openclaw.ai/concepts/commitments\#enable-commitments)  Enable commitments

Commitments are off by default. Enable them in config:

```
openclaw config set commitments.enabled true
openclaw config set commitments.maxPerDay 3
```

Equivalent `openclaw.json`:

```
{
  "commitments": {
    "enabled": true,
    "maxPerDay": 3
  }
}
```

`commitments.maxPerDay` limits how many inferred follow-ups can be delivered
per agent session in a rolling day. The default is `3`.

## [​](https://docs.openclaw.ai/concepts/commitments\#how-it-works)  How it works

After an agent reply, OpenClaw may run a hidden background extraction pass in a
separate context. That pass looks only for inferred follow-up commitments. It
does not write into the visible conversation and it does not ask the main agent
to reason about the extraction.When it finds a high-confidence candidate, OpenClaw stores a commitment with:

- the agent id
- the session key
- the original channel and delivery target
- a due window
- a short suggested check-in
- non-instructional metadata for heartbeat to decide whether to send it

Delivery happens through heartbeat. When a commitment becomes due, heartbeat
adds the commitment to the heartbeat turn for the same agent and channel scope.
The model can send one natural check-in or reply `HEARTBEAT_OK` to dismiss it.
If heartbeat is configured with `target: "none"`, due commitments remain
internal and do not send external check-ins. Commitment delivery prompts do not
replay the original conversation text, and due commitment heartbeat turns run
without OpenClaw tools.OpenClaw never delivers an inferred commitment immediately after writing it.
The due time is clamped to at least one heartbeat interval after the commitment
is created, so the follow-up cannot echo back in the same moment it was
inferred.

## [​](https://docs.openclaw.ai/concepts/commitments\#scope)  Scope

Commitments are scoped to the exact agent and channel context where they were
created. A follow-up inferred while talking to one agent in Discord is not
delivered by another agent, another channel, or an unrelated session.This scope is part of the feature. Natural check-ins should feel like the same
conversation continuing, not like a global reminder system.

## [​](https://docs.openclaw.ai/concepts/commitments\#commitments-vs-reminders)  Commitments vs reminders

| Need | Use |
| --- | --- |
| ”Remind me at 3 PM” | [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs) |
| ”Ping me in 20 minutes” | [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs) |
| ”Run this report every weekday” | [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs) |
| ”I have an interview tomorrow” | Commitments |
| ”I was up all night” | Commitments |
| ”Follow up if I do not answer this open thread” | Commitments |

Exact user requests already belong to the scheduler path. Commitments are only
for inferred follow-ups: the moments where the user did not ask for a reminder,
but the conversation clearly created a useful future check-in.

## [​](https://docs.openclaw.ai/concepts/commitments\#manage-commitments)  Manage commitments

Use the CLI to inspect and clear stored commitments:

```
openclaw commitments
openclaw commitments --all
openclaw commitments --agent main
openclaw commitments --status snoozed
openclaw commitments dismiss cm_abc123
```

See [`openclaw commitments`](https://docs.openclaw.ai/cli/commitments) for the command reference.

## [​](https://docs.openclaw.ai/concepts/commitments\#privacy-and-cost)  Privacy and cost

Commitment extraction uses an LLM pass, so enabling it adds background model
usage after eligible turns. The pass is hidden from the user-visible
conversation, but it can read the recent exchange needed to decide whether a
follow-up exists.Stored commitments are local OpenClaw state. They are operational memory, not
long-term memory. Disable the feature with:

```
openclaw config set commitments.enabled false
```

## [​](https://docs.openclaw.ai/concepts/commitments\#troubleshooting)  Troubleshooting

If expected follow-ups are not appearing:

- Confirm `commitments.enabled` is `true`.
- Check `openclaw commitments --all` for pending, dismissed, snoozed, or expired
records.
- Make sure heartbeat is running for the agent.
- Check whether `commitments.maxPerDay` has already been reached for that
agent session.
- Remember that exact reminders are skipped by commitment extraction and should
appear under [scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs) instead.

## [​](https://docs.openclaw.ai/concepts/commitments\#related)  Related

- [Memory overview](https://docs.openclaw.ai/concepts/memory)
- [Active memory](https://docs.openclaw.ai/concepts/active-memory)
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat)
- [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs)
- [`openclaw commitments`](https://docs.openclaw.ai/cli/commitments)
- [Configuration reference](https://docs.openclaw.ai/gateway/configuration-reference#commitments)

[Active memory](https://docs.openclaw.ai/concepts/active-memory) [Dreaming](https://docs.openclaw.ai/concepts/dreaming)

Ctrl+I