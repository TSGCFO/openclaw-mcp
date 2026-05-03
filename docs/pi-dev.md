---
source_url: https://docs.openclaw.ai/pi-dev
title: "Pi development workflow - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/pi-dev#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Advanced setup

Pi development workflow

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Type checking and linting](https://docs.openclaw.ai/pi-dev#type-checking-and-linting)
- [Running Pi tests](https://docs.openclaw.ai/pi-dev#running-pi-tests)
- [Manual testing](https://docs.openclaw.ai/pi-dev#manual-testing)
- [Clean slate reset](https://docs.openclaw.ai/pi-dev#clean-slate-reset)
- [References](https://docs.openclaw.ai/pi-dev#references)
- [Related](https://docs.openclaw.ai/pi-dev#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

A sane workflow for working on the Pi integration in OpenClaw.

## [​](https://docs.openclaw.ai/pi-dev\#type-checking-and-linting)  Type checking and linting

- Default local gate: `pnpm check`
- Build gate: `pnpm build` when the change can affect build output, packaging, or lazy-loading/module boundaries
- Full landing gate for Pi-heavy changes: `pnpm check && pnpm test`

## [​](https://docs.openclaw.ai/pi-dev\#running-pi-tests)  Running Pi tests

Run the Pi-focused test set directly with Vitest:

```
pnpm test \
  "src/agents/pi-*.test.ts" \
  "src/agents/pi-embedded-*.test.ts" \
  "src/agents/pi-tools*.test.ts" \
  "src/agents/pi-settings.test.ts" \
  "src/agents/pi-tool-definition-adapter*.test.ts" \
  "src/agents/pi-hooks/**/*.test.ts"
```

To include the live provider exercise:

```
OPENCLAW_LIVE_TEST=1 pnpm test src/agents/pi-embedded-runner-extraparams.live.test.ts
```

This covers the main Pi unit suites:

- `src/agents/pi-*.test.ts`
- `src/agents/pi-embedded-*.test.ts`
- `src/agents/pi-tools*.test.ts`
- `src/agents/pi-settings.test.ts`
- `src/agents/pi-tool-definition-adapter.test.ts`
- `src/agents/pi-hooks/*.test.ts`

## [​](https://docs.openclaw.ai/pi-dev\#manual-testing)  Manual testing

Recommended flow:

- Run the gateway in dev mode:
  - `pnpm gateway:dev`
- Trigger the agent directly:
  - `pnpm openclaw agent --message "Hello" --thinking low`
- Use the TUI for interactive debugging:
  - `pnpm tui`

For tool call behavior, prompt for a `read` or `exec` action so you can see tool streaming and payload handling.

## [​](https://docs.openclaw.ai/pi-dev\#clean-slate-reset)  Clean slate reset

State lives under the OpenClaw state directory. Default is `~/.openclaw`. If `OPENCLAW_STATE_DIR` is set, use that directory instead.To reset everything:

- `openclaw.json` for config
- `agents/<agentId>/agent/auth-profiles.json` for model auth profiles (API keys + OAuth)
- `credentials/` for provider/channel state that still lives outside the auth profile store
- `agents/<agentId>/sessions/` for agent session history
- `agents/<agentId>/sessions/sessions.json` for the session index
- `sessions/` if legacy paths exist
- `workspace/` if you want a blank workspace

If you only want to reset sessions, delete `agents/<agentId>/sessions/` for that agent. If you want to keep auth, leave `agents/<agentId>/agent/auth-profiles.json` and any provider state under `credentials/` in place.

## [​](https://docs.openclaw.ai/pi-dev\#references)  References

- [Testing](https://docs.openclaw.ai/help/testing)
- [Getting Started](https://docs.openclaw.ai/start/getting-started)

## [​](https://docs.openclaw.ai/pi-dev\#related)  Related

- [Pi integration architecture](https://docs.openclaw.ai/pi)

[Setup](https://docs.openclaw.ai/start/setup)

Ctrl+I