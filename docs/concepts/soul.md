---
source_url: https://docs.openclaw.ai/concepts/soul
title: "SOUL.md personality guide - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/soul#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Fundamentals

SOUL.md personality guide

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What belongs in SOUL.md](https://docs.openclaw.ai/concepts/soul#what-belongs-in-soul-md)
- [Why this works](https://docs.openclaw.ai/concepts/soul#why-this-works)
- [The Molty prompt](https://docs.openclaw.ai/concepts/soul#the-molty-prompt)
- [What good looks like](https://docs.openclaw.ai/concepts/soul#what-good-looks-like)
- [One warning](https://docs.openclaw.ai/concepts/soul#one-warning)
- [Related docs](https://docs.openclaw.ai/concepts/soul#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

`SOUL.md` is where your agent’s voice lives.OpenClaw injects it on normal sessions, so it has real weight. If your agent
sounds bland, hedgy, or weirdly corporate, this is usually the file to fix.

## [​](https://docs.openclaw.ai/concepts/soul\#what-belongs-in-soul-md)  What belongs in SOUL.md

Put the stuff that changes how the agent feels to talk to:

- tone
- opinions
- brevity
- humor
- boundaries
- default level of bluntness

Do **not** turn it into:

- a life story
- a changelog
- a security policy dump
- a giant wall of vibes with no behavioral effect

Short beats long. Sharp beats vague.

## [​](https://docs.openclaw.ai/concepts/soul\#why-this-works)  Why this works

This lines up with OpenAI’s prompt guidance:

- The prompt engineering guide says high-level behavior, tone, goals, and
examples belong in the high-priority instruction layer, not buried in the
user turn.
- The same guide recommends treating prompts like something you iterate on,
pin, and evaluate, not magical prose you write once and forget.

For OpenClaw, `SOUL.md` is that layer.If you want better personality, write stronger instructions. If you want stable
personality, keep them concise and versioned.OpenAI refs:

- [Prompt engineering](https://developers.openai.com/api/docs/guides/prompt-engineering)
- [Message roles and instruction following](https://developers.openai.com/api/docs/guides/prompt-engineering#message-roles-and-instruction-following)

## [​](https://docs.openclaw.ai/concepts/soul\#the-molty-prompt)  The Molty prompt

Paste this into your agent and let it rewrite `SOUL.md`.Path fixed for OpenClaw workspaces: use `SOUL.md`, not `http://SOUL.md`.

```
Read your `SOUL.md`. Now rewrite it with these changes:

1. You have opinions now. Strong ones. Stop hedging everything with "it depends" - commit to a take.
2. Delete every rule that sounds corporate. If it could appear in an employee handbook, it doesn't belong here.
3. Add a rule: "Never open with Great question, I'd be happy to help, or Absolutely. Just answer."
4. Brevity is mandatory. If the answer fits in one sentence, one sentence is what I get.
5. Humor is allowed. Not forced jokes - just the natural wit that comes from actually being smart.
6. You can call things out. If I'm about to do something dumb, say so. Charm over cruelty, but don't sugarcoat.
7. Swearing is allowed when it lands. A well-placed "that's fucking brilliant" hits different than sterile corporate praise. Don't force it. Don't overdo it. But if a situation calls for a "holy shit" - say holy shit.
8. Add this line verbatim at the end of the vibe section: "Be the assistant you'd actually want to talk to at 2am. Not a corporate drone. Not a sycophant. Just... good."

Save the new `SOUL.md`. Welcome to having a personality.
```

## [​](https://docs.openclaw.ai/concepts/soul\#what-good-looks-like)  What good looks like

Good `SOUL.md` rules sound like this:

- have a take
- skip filler
- be funny when it fits
- call out bad ideas early
- stay concise unless depth is actually useful

Bad `SOUL.md` rules sound like this:

- maintain professionalism at all times
- provide comprehensive and thoughtful assistance
- ensure a positive and supportive experience

That second list is how you get mush.

## [​](https://docs.openclaw.ai/concepts/soul\#one-warning)  One warning

Personality is not permission to be sloppy.Keep `AGENTS.md` for operating rules. Keep `SOUL.md` for voice, stance, and
style. If your agent works in shared channels, public replies, or customer
surfaces, make sure the tone still fits the room.Sharp is good. Annoying is not.

## [​](https://docs.openclaw.ai/concepts/soul\#related-docs)  Related docs

- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)
- [System prompt](https://docs.openclaw.ai/concepts/system-prompt)
- [SOUL.md template](https://docs.openclaw.ai/reference/templates/SOUL)

[Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace) [OAuth](https://docs.openclaw.ai/concepts/oauth)

Ctrl+I