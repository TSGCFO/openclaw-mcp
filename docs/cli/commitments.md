---
source_url: https://docs.openclaw.ai/cli/commitments
title: "`openclaw commitments` - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/commitments#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Agents and sessions

\`openclaw commitments\`

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Usage](https://docs.openclaw.ai/cli/commitments#usage)
- [Options](https://docs.openclaw.ai/cli/commitments#options)
- [Examples](https://docs.openclaw.ai/cli/commitments#examples)
- [Output](https://docs.openclaw.ai/cli/commitments#output)
- [Related](https://docs.openclaw.ai/cli/commitments#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

List and manage inferred follow-up commitments.Commitments are opt-in, short-lived follow-up memories created from
conversation context. See [Inferred commitments](https://docs.openclaw.ai/concepts/commitments) for the
conceptual guide.With no subcommand, `openclaw commitments` lists pending commitments.

## [​](https://docs.openclaw.ai/cli/commitments\#usage)  Usage

```
openclaw commitments [--all] [--agent <id>] [--status <status>] [--json]
openclaw commitments list [--all] [--agent <id>] [--status <status>] [--json]
openclaw commitments dismiss <id...> [--json]
```

## [​](https://docs.openclaw.ai/cli/commitments\#options)  Options

- `--all`: show all statuses instead of only pending commitments.
- `--agent <id>`: filter to one agent id.
- `--status <status>`: filter by status. Values: `pending`, `sent`,
`dismissed`, `snoozed`, or `expired`.
- `--json`: output machine-readable JSON.

## [​](https://docs.openclaw.ai/cli/commitments\#examples)  Examples

List pending commitments:

```
openclaw commitments
```

List every stored commitment:

```
openclaw commitments --all
```

Filter to one agent:

```
openclaw commitments --agent main
```

Find snoozed commitments:

```
openclaw commitments --status snoozed
```

Dismiss one or more commitments:

```
openclaw commitments dismiss cm_abc123 cm_def456
```

Export as JSON:

```
openclaw commitments --all --json
```

## [​](https://docs.openclaw.ai/cli/commitments\#output)  Output

Text output includes:

- commitment id
- status
- kind
- earliest due time
- scope
- suggested check-in text

JSON output also includes the commitment store path and full stored records.

## [​](https://docs.openclaw.ai/cli/commitments\#related)  Related

- [Inferred commitments](https://docs.openclaw.ai/concepts/commitments)
- [Memory overview](https://docs.openclaw.ai/concepts/memory)
- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat)
- [Scheduled tasks](https://docs.openclaw.ai/automation/cron-jobs)

[Memory](https://docs.openclaw.ai/cli/memory) [Message](https://docs.openclaw.ai/cli/message)

Ctrl+I