---
source_url: https://docs.openclaw.ai/start/onboarding
title: "Onboarding (macOS app) - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/start/onboarding#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

First steps

Onboarding (macOS app)

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Related](https://docs.openclaw.ai/start/onboarding#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

This doc describes the **current** first‑run setup flow. The goal is a
smooth “day 0” experience: pick where the Gateway runs, connect auth, run the
wizard, and let the agent bootstrap itself.
For a general overview of onboarding paths, see [Onboarding Overview](https://docs.openclaw.ai/start/onboarding-overview).

1

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Approve macOS warning

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/01-macos-warning.jpeg?fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=7ade99ff85eba6a2fe743ff1f7799087)

2

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Approve find local networks

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/02-local-networks.jpeg?fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=e9fcec535d0cdca207cff0cf2379e951)

3

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Welcome and security notice

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/03-security-notice.png?fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=8866e4aaac170614a163d990091addac)

Read the security notice displayed and decide accordingly

Security trust model:

- By default, OpenClaw is a personal agent: one trusted operator boundary.
- Shared/multi-user setups require lock-down (split trust boundaries, keep tool access minimal, and follow [Security](https://docs.openclaw.ai/gateway/security)).
- Local onboarding now defaults new configs to `tools.profile: "coding"` so fresh local setups keep filesystem/runtime tools without forcing the unrestricted `full` profile.
- If hooks/webhooks or other untrusted content feeds are enabled, use a strong modern model tier and keep strict tool policy/sandboxing.

4

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Local vs Remote

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/04-choose-gateway.png?fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=7e923f389e6d774363064140691b4fbe)

Where does the **Gateway** run?

- **This Mac (Local only):** onboarding can configure auth and write credentials
locally.
- **Remote (over SSH/Tailnet):** onboarding does **not** configure local auth;
credentials must exist on the gateway host.
- **Configure later:** skip setup and leave the app unconfigured.

**Gateway auth tip:**

- The wizard now generates a **token** even for loopback, so local WS clients must authenticate.
- If you disable auth, any local process can connect; use that only on fully trusted machines.
- Use a **token** for multi‑machine access or non‑loopback binds.

5

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Permissions

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/05-permissions.png?fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=6c45fa49282cf491a1425a714ec68f18)

Choose what permissions do you want to give OpenClaw

Onboarding requests TCC permissions needed for:

- Automation (AppleScript)
- Notifications
- Accessibility
- Screen Recording
- Microphone
- Speech Recognition
- Camera
- Location

6

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

CLI

This step is optional

The app can install the global `openclaw` CLI via npm, pnpm, or bun.
It prefers npm first, then pnpm, then bun if that is the only detected
package manager. For the Gateway runtime, Node remains the recommended path.

7

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Onboarding Chat (dedicated session)

After setup, the app opens a dedicated onboarding chat session so the agent can
introduce itself and guide next steps. This keeps first‑run guidance separate
from your normal conversation. See [Bootstrapping](https://docs.openclaw.ai/start/bootstrapping) for
what happens on the gateway host during the first agent run.

## [​](https://docs.openclaw.ai/start/onboarding\#related)  Related

- [Onboarding overview](https://docs.openclaw.ai/start/onboarding-overview)
- [Getting started](https://docs.openclaw.ai/start/getting-started)

[Onboarding: CLI](https://docs.openclaw.ai/start/wizard) [Personal assistant setup](https://docs.openclaw.ai/start/openclaw)

Ctrl+I

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/01-macos-warning.jpeg?w=1100&fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=b4fc07a373f4112e8458d8551cdc8e9f)

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/02-local-networks.jpeg?w=1100&fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=5e82fe57d9f4f30b3829f96878433d70)

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/03-security-notice.png?w=1100&fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=50f49bb539545827defba8bebb4d012c)

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/04-choose-gateway.png?w=1100&fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=e17cfde7ca863e36ef9442574280a49d)

![](https://mintcdn.com/clawdhub/zr61AlCx-k7XN8so/assets/macos-onboarding/05-permissions.png?w=1100&fit=max&auto=format&n=zr61AlCx-k7XN8so&q=85&s=0d11aff2af1b14f778e3304c904657b7)