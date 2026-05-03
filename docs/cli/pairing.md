---
source_url: https://docs.openclaw.ai/cli/pairing.md
title: ""
---

\> ## Documentation Index
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Pairing

\# \`openclaw pairing\`

Approve or inspect DM pairing requests (for channels that support pairing).

Related:

\\* Pairing flow: \[Pairing\](/channels/pairing)

\## Commands

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw pairing list telegram
openclaw pairing list --channel telegram --account work
openclaw pairing list telegram --json

openclaw pairing approve ```
openclaw pairing approve telegram
openclaw pairing approve --channel telegram --account work  --notify
```

## `pairing list`

List pending pairing requests for one channel.

Options:

* `[channel]`: positional channel id
* `--channel `: explicit channel id
* `--account `: account id for multi-account channels
* `--json`: machine-readable output

Notes:

* If multiple pairing-capable channels are configured, you must provide a channel either positionally or with `--channel`.
* Extension channels are allowed as long as the channel id is valid.

## `pairing approve`

Approve a pending pairing code and allow that sender.

Usage:

* `openclaw pairing approve  `
* `openclaw pairing approve --channel  `
* `openclaw pairing approve ` when exactly one pairing-capable channel is configured

Options:

* `--channel `: explicit channel id
* `--account `: account id for multi-account channels
* `--notify`: send a confirmation back to the requester on the same channel

Owner bootstrap:

* If `commands.ownerAllowFrom` is empty when you approve a pairing code, OpenClaw also records the approved sender as the command owner, using a channel-scoped entry such as `telegram:123456789`.
* This only bootstraps the first owner. Later pairing approvals do not replace or expand `commands.ownerAllowFrom`.
* The command owner is the human operator account allowed to run owner-only commands and approve dangerous actions such as `/diagnostics`, `/export-trajectory`, `/config`, and exec approvals.

## Notes

* Channel input: pass it positionally (`pairing list telegram`) or with `--channel `.
* `pairing list` supports `--account ` for multi-account channels.
* `pairing approve` supports `--account ` and `--notify`.
* If only one pairing-capable channel is configured, `pairing approve ` is allowed.
* If you approved a sender before this bootstrap existed, run `openclaw doctor`; it warns when no command owner is configured and shows the `openclaw config set commands.ownerAllowFrom ...` command to fix it.

## Related

* [CLI reference](/cli)
* [Channel pairing](/channels/pairing)
```