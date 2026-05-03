---
source_url: https://docs.openclaw.ai/cli/skills
title: "Skills - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/skills#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Plugins and skills

Skills

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw skills](https://docs.openclaw.ai/cli/skills#openclaw-skills)
- [Commands](https://docs.openclaw.ai/cli/skills#commands)
- [Related](https://docs.openclaw.ai/cli/skills#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/skills\#openclaw-skills)  `openclaw skills`

Inspect local skills and install/update skills from ClawHub.Related:

- Skills system: [Skills](https://docs.openclaw.ai/tools/skills)
- Skills config: [Skills config](https://docs.openclaw.ai/tools/skills-config)
- ClawHub installs: [ClawHub](https://docs.openclaw.ai/tools/clawhub)

## [​](https://docs.openclaw.ai/cli/skills\#commands)  Commands

```
openclaw skills search "calendar"
openclaw skills search --limit 20 --json
openclaw skills install <slug>
openclaw skills install <slug> --version <version>
openclaw skills install <slug> --force
openclaw skills install <slug> --agent <id>
openclaw skills update <slug>
openclaw skills update --all
openclaw skills update --all --agent <id>
openclaw skills list
openclaw skills list --eligible
openclaw skills list --json
openclaw skills list --verbose
openclaw skills list --agent <id>
openclaw skills info <name>
openclaw skills info <name> --json
openclaw skills info <name> --agent <id>
openclaw skills check
openclaw skills check --agent <id>
openclaw skills check --json
```

`search`/`install`/`update` use ClawHub directly and install into the active
workspace `skills/` directory. `list`/`info`/`check` still inspect the local
skills visible to the current workspace and config. Workspace-backed commands
resolve the target workspace from `--agent <id>`, then the current working
directory when it is inside a configured agent workspace, then the default
agent.This CLI `install` command downloads skill folders from ClawHub. Gateway-backed
skill dependency installs triggered from onboarding or Skills settings use the
separate `skills.install` request path instead.Notes:

- `search [query...]` accepts an optional query; omit it to browse the default
ClawHub search feed.
- `search --limit <n>` caps returned results.
- `install --force` overwrites an existing workspace skill folder for the same
slug.
- `--agent <id>` targets one configured agent workspace and overrides current
working directory inference.
- `update --all` only updates tracked ClawHub installs in the active workspace.
- `check --agent <id>` checks the selected agent’s workspace and reports which
ready skills are actually visible to that agent’s prompt or command surface.
- `list` is the default action when no subcommand is provided.
- `list`, `info`, and `check` write their rendered output to stdout. With
`--json`, that means the machine-readable payload stays on stdout for pipes
and scripts.

## [​](https://docs.openclaw.ai/cli/skills\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Skills](https://docs.openclaw.ai/tools/skills)

[Plugins](https://docs.openclaw.ai/cli/plugins) [Dashboard](https://docs.openclaw.ai/cli/dashboard)

Ctrl+I