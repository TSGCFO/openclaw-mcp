---
source_url: https://docs.openclaw.ai/cli/tui
title: "TUI - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/tui#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Interfaces

TUI

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw tui](https://docs.openclaw.ai/cli/tui#openclaw-tui)
- [Examples](https://docs.openclaw.ai/cli/tui#examples)
- [Config repair loop](https://docs.openclaw.ai/cli/tui#config-repair-loop)
- [Related](https://docs.openclaw.ai/cli/tui#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/tui\#openclaw-tui)  `openclaw tui`

Open the terminal UI connected to the Gateway, or run it in local embedded
mode.Related:

- TUI guide: [TUI](https://docs.openclaw.ai/web/tui)

Notes:

- `chat` and `terminal` are aliases for `openclaw tui --local`.
- `--local` cannot be combined with `--url`, `--token`, or `--password`.
- `tui` resolves configured gateway auth SecretRefs for token/password auth when possible (`env`/`file`/`exec` providers).
- When launched from inside a configured agent workspace directory, TUI auto-selects that agent for the session key default (unless `--session` is explicitly `agent:<id>:...`).
- Local mode uses the embedded agent runtime directly. Most local tools work, but Gateway-only features are unavailable.
- Local mode adds `/auth [provider]` inside the TUI command surface.
- Plugin approval gates still apply in local mode. Tools that require approval prompt for a decision in the terminal; nothing is silently auto-approved because the Gateway is not involved.

## [​](https://docs.openclaw.ai/cli/tui\#examples)  Examples

```
openclaw chat
openclaw tui --local
openclaw tui
openclaw tui --url ws://127.0.0.1:18789 --token <token>
openclaw tui --session main --deliver
openclaw chat --message "Compare my config to the docs and tell me what to fix"
# when run inside an agent workspace, infers that agent automatically
openclaw tui --session bugfix
```

## [​](https://docs.openclaw.ai/cli/tui\#config-repair-loop)  Config repair loop

Use local mode when the current config already validates and you want the
embedded agent to inspect it, compare it against the docs, and help repair it
from the same terminal:If `openclaw config validate` is already failing, use `openclaw configure` or
`openclaw doctor --fix` first. `openclaw chat` does not bypass the invalid-
config guard.

```
openclaw chat
```

Then inside the TUI:

```
!openclaw config file
!openclaw docs gateway auth token secretref
!openclaw config validate
!openclaw doctor
```

Apply targeted fixes with `openclaw config set` or `openclaw configure`, then
rerun `openclaw config validate`. See [TUI](https://docs.openclaw.ai/web/tui) and [Config](https://docs.openclaw.ai/cli/config).

## [​](https://docs.openclaw.ai/cli/tui\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [TUI](https://docs.openclaw.ai/web/tui)

[Dashboard](https://docs.openclaw.ai/cli/dashboard) [ACP](https://docs.openclaw.ai/cli/acp)

Ctrl+I