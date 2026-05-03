---
source_url: https://docs.openclaw.ai/cli/channels
title: "Channels - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/channels#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Channels and messaging

Channels

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw channels](https://docs.openclaw.ai/cli/channels#openclaw-channels)
- [Common commands](https://docs.openclaw.ai/cli/channels#common-commands)
- [Status / capabilities / resolve / logs](https://docs.openclaw.ai/cli/channels#status-%2F-capabilities-%2F-resolve-%2F-logs)
- [Add / remove accounts](https://docs.openclaw.ai/cli/channels#add-%2F-remove-accounts)
- [Login and logout (interactive)](https://docs.openclaw.ai/cli/channels#login-and-logout-interactive)
- [Troubleshooting](https://docs.openclaw.ai/cli/channels#troubleshooting)
- [Capabilities probe](https://docs.openclaw.ai/cli/channels#capabilities-probe)
- [Resolve names to IDs](https://docs.openclaw.ai/cli/channels#resolve-names-to-ids)
- [Related](https://docs.openclaw.ai/cli/channels#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/channels\#openclaw-channels)  `openclaw channels`

Manage chat channel accounts and their runtime status on the Gateway.Related docs:

- Channel guides: [Channels](https://docs.openclaw.ai/channels)
- Gateway configuration: [Configuration](https://docs.openclaw.ai/gateway/configuration)

## [​](https://docs.openclaw.ai/cli/channels\#common-commands)  Common commands

```
openclaw channels list
openclaw channels status
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
openclaw channels resolve --channel slack "#general" "@jane"
openclaw channels logs --channel all
```

## [​](https://docs.openclaw.ai/cli/channels\#status-/-capabilities-/-resolve-/-logs)  Status / capabilities / resolve / logs

- `channels status`: `--probe`, `--timeout <ms>`, `--json`
- `channels capabilities`: `--channel <name>`, `--account <id>` (only with `--channel`), `--target <dest>`, `--timeout <ms>`, `--json`
- `channels resolve`: `<entries...>`, `--channel <name>`, `--account <id>`, `--kind <auto|user|group>`, `--json`
- `channels logs`: `--channel <name|all>`, `--lines <n>`, `--json`

`channels status --probe` is the live path: on a reachable gateway it runs per-account
`probeAccount` and optional `auditAccount` checks, so output can include transport
state plus probe results such as `works`, `probe failed`, `audit ok`, or `audit failed`.
If the gateway is unreachable, `channels status` falls back to config-only summaries
instead of live probe output.Do not use `openclaw sessions`, Gateway `sessions.list`, or the agent
`sessions_list` tool as a channel socket-health signal. Those surfaces report
stored conversation rows, not provider runtime state. After a Discord provider
restart, a connected but quiet account may be healthy while no Discord session
row appears until the next inbound or outbound conversation event.

## [​](https://docs.openclaw.ai/cli/channels\#add-/-remove-accounts)  Add / remove accounts

```
openclaw channels add --channel telegram --token <bot-token>
openclaw channels add --channel nostr --private-key "$NOSTR_PRIVATE_KEY"
openclaw channels remove --channel telegram --delete
```

`openclaw channels add --help` shows per-channel flags (token, private key, app token, signal-cli paths, etc).

`channels remove` only operates on installed/configured channel plugins. Use `channels add` first for installable catalog channels.
For runtime-backed channel plugins, `channels remove` also asks the running Gateway to stop the selected account before it updates config, so disabling or deleting an account does not leave the old listener active until restart.Common non-interactive add surfaces include:

- bot-token channels: `--token`, `--bot-token`, `--app-token`, `--token-file`
- Signal/iMessage transport fields: `--signal-number`, `--cli-path`, `--http-url`, `--http-host`, `--http-port`, `--db-path`, `--service`, `--region`
- Google Chat fields: `--webhook-path`, `--webhook-url`, `--audience-type`, `--audience`
- Matrix fields: `--homeserver`, `--user-id`, `--access-token`, `--password`, `--device-name`, `--initial-sync-limit`
- Nostr fields: `--private-key`, `--relay-urls`
- Tlon fields: `--ship`, `--url`, `--code`, `--group-channels`, `--dm-allowlist`, `--auto-discover-channels`
- `--use-env` for default-account env-backed auth where supported

If a channel plugin needs to be installed during a flag-driven add command, OpenClaw uses the channel’s default install source without opening the interactive plugin install prompt.When you run `openclaw channels add` without flags, the interactive wizard can prompt:

- account ids per selected channel
- optional display names for those accounts
- `Bind configured channel accounts to agents now?`

If you confirm bind now, the wizard asks which agent should own each configured channel account and writes account-scoped routing bindings.You can also manage the same routing rules later with `openclaw agents bindings`, `openclaw agents bind`, and `openclaw agents unbind` (see [agents](https://docs.openclaw.ai/cli/agents)).When you add a non-default account to a channel that is still using single-account top-level settings, OpenClaw promotes account-scoped top-level values into the channel’s account map before writing the new account. Most channels land those values in `channels.<channel>.accounts.default`, but bundled channels can preserve an existing matching promoted account instead. Matrix is the current example: if one named account already exists, or `defaultAccount` points at an existing named account, promotion preserves that account instead of creating a new `accounts.default`.Routing behavior stays consistent:

- Existing channel-only bindings (no `accountId`) continue to match the default account.
- `channels add` does not auto-create or rewrite bindings in non-interactive mode.
- Interactive setup can optionally add account-scoped bindings.

If your config was already in a mixed state (named accounts present and top-level single-account values still set), run `openclaw doctor --fix` to move account-scoped values into the promoted account chosen for that channel. Most channels promote into `accounts.default`; Matrix can preserve an existing named/default target instead.

## [​](https://docs.openclaw.ai/cli/channels\#login-and-logout-interactive)  Login and logout (interactive)

```
openclaw channels login --channel whatsapp
openclaw channels logout --channel whatsapp
```

- `channels login` supports `--verbose`.
- `channels login` and `logout` can infer the channel when only one supported login target is configured.
- `channels logout` prefers the live Gateway path when reachable, so logout stops any active listener before clearing channel auth state. If a local Gateway is not reachable, it falls back to local auth cleanup.
- Run `channels login` from a terminal on the gateway host. Agent `exec` blocks this interactive login flow; channel-native agent login tools, such as `whatsapp_login`, should be used from chat when available.

## [​](https://docs.openclaw.ai/cli/channels\#troubleshooting)  Troubleshooting

- Run `openclaw status --deep` for a broad probe.
- Use `openclaw doctor` for guided fixes.
- `openclaw channels list` prints `Claude: HTTP 403 ... user:profile` → usage snapshot needs the `user:profile` scope. Use `--no-usage`, or provide a claude.ai session key (`CLAUDE_WEB_SESSION_KEY` / `CLAUDE_WEB_COOKIE`), or re-auth via Claude CLI.
- `openclaw channels status` falls back to config-only summaries when the gateway is unreachable. If a supported channel credential is configured via SecretRef but unavailable in the current command path, it reports that account as configured with degraded notes instead of showing it as not configured.

## [​](https://docs.openclaw.ai/cli/channels\#capabilities-probe)  Capabilities probe

Fetch provider capability hints (intents/scopes where available) plus static feature support:

```
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
```

Notes:

- `--channel` is optional; omit it to list every channel (including extensions).
- `--account` is only valid with `--channel`.
- `--target` accepts `channel:<id>` or a raw numeric channel id and only applies to Discord.
- Probes are provider-specific: Discord intents + optional channel permissions; Slack bot + user scopes; Telegram bot flags + webhook; Signal daemon version; Microsoft Teams app token + Graph roles/scopes (annotated where known). Channels without probes report `Probe: unavailable`.

## [​](https://docs.openclaw.ai/cli/channels\#resolve-names-to-ids)  Resolve names to IDs

Resolve channel/user names to IDs using the provider directory:

```
openclaw channels resolve --channel slack "#general" "@jane"
openclaw channels resolve --channel discord "My Server/#support" "@someone"
openclaw channels resolve --channel matrix "Project Room"
```

Notes:

- Use `--kind user|group|auto` to force the target type.
- Resolution prefers active matches when multiple entries share the same name.
- `channels resolve` is read-only. If a selected account is configured via SecretRef but that credential is unavailable in the current command path, the command returns degraded unresolved results with notes instead of aborting the entire run.
- `channels resolve` does not install channel plugins. Use `channels add --channel <name>` before resolving names for an installable catalog channel.

## [​](https://docs.openclaw.ai/cli/channels\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Channels overview](https://docs.openclaw.ai/channels)

[\`openclaw tasks\`](https://docs.openclaw.ai/cli/tasks) [Devices](https://docs.openclaw.ai/cli/devices)

Ctrl+I