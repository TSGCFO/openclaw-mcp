---
source_url: https://docs.openclaw.ai/channels/troubleshooting
title: "Channel troubleshooting - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels/troubleshooting#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Configuration

Channel troubleshooting

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Command ladder](https://docs.openclaw.ai/channels/troubleshooting#command-ladder)
- [WhatsApp](https://docs.openclaw.ai/channels/troubleshooting#whatsapp)
- [WhatsApp failure signatures](https://docs.openclaw.ai/channels/troubleshooting#whatsapp-failure-signatures)
- [Telegram](https://docs.openclaw.ai/channels/troubleshooting#telegram)
- [Telegram failure signatures](https://docs.openclaw.ai/channels/troubleshooting#telegram-failure-signatures)
- [Discord](https://docs.openclaw.ai/channels/troubleshooting#discord)
- [Discord failure signatures](https://docs.openclaw.ai/channels/troubleshooting#discord-failure-signatures)
- [Slack](https://docs.openclaw.ai/channels/troubleshooting#slack)
- [Slack failure signatures](https://docs.openclaw.ai/channels/troubleshooting#slack-failure-signatures)
- [iMessage and BlueBubbles](https://docs.openclaw.ai/channels/troubleshooting#imessage-and-bluebubbles)
- [iMessage and BlueBubbles failure signatures](https://docs.openclaw.ai/channels/troubleshooting#imessage-and-bluebubbles-failure-signatures)
- [Signal](https://docs.openclaw.ai/channels/troubleshooting#signal)
- [Signal failure signatures](https://docs.openclaw.ai/channels/troubleshooting#signal-failure-signatures)
- [QQ Bot](https://docs.openclaw.ai/channels/troubleshooting#qq-bot)
- [QQ Bot failure signatures](https://docs.openclaw.ai/channels/troubleshooting#qq-bot-failure-signatures)
- [Matrix](https://docs.openclaw.ai/channels/troubleshooting#matrix)
- [Matrix failure signatures](https://docs.openclaw.ai/channels/troubleshooting#matrix-failure-signatures)
- [Related](https://docs.openclaw.ai/channels/troubleshooting#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Use this page when a channel connects but behavior is wrong.

## [​](https://docs.openclaw.ai/channels/troubleshooting\#command-ladder)  Command ladder

Run these in order first:

```
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

Healthy baseline:

- `Runtime: running`
- `Connectivity probe: ok`
- `Capability: read-only`, `write-capable`, or `admin-capable`
- Channel probe shows transport connected and, where supported, `works` or `audit ok`

## [​](https://docs.openclaw.ai/channels/troubleshooting\#whatsapp)  WhatsApp

### [​](https://docs.openclaw.ai/channels/troubleshooting\#whatsapp-failure-signatures)  WhatsApp failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| Connected but no DM replies | `openclaw pairing list whatsapp` | Approve sender or switch DM policy/allowlist. |
| Group messages ignored | Check `requireMention` \+ mention patterns in config | Mention the bot or relax mention policy for that group. |
| QR login times out with 408 | Check gateway `HTTPS_PROXY` / `HTTP_PROXY` env | Set a reachable proxy; use `NO_PROXY` only for bypasses. |
| Random disconnect/relogin loops | `openclaw channels status --probe` \+ logs | Recent reconnects are flagged even when currently connected; watch logs, restart the gateway, then relink if flapping continues. |

Full troubleshooting: [WhatsApp troubleshooting](https://docs.openclaw.ai/channels/whatsapp#troubleshooting)

## [​](https://docs.openclaw.ai/channels/troubleshooting\#telegram)  Telegram

### [​](https://docs.openclaw.ai/channels/troubleshooting\#telegram-failure-signatures)  Telegram failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| `/start` but no usable reply flow | `openclaw pairing list telegram` | Approve pairing or change DM policy. |
| Bot online but group stays silent | Verify mention requirement and bot privacy mode | Disable privacy mode for group visibility or mention bot. |
| Send failures with network errors | Inspect logs for Telegram API call failures | Fix DNS/IPv6/proxy routing to `api.telegram.org`. |
| Startup reports `getMe returned 401` | Check configured token source | Re-copy or regenerate the BotFather token and update `botToken`, `tokenFile`, or default-account `TELEGRAM_BOT_TOKEN`. |
| Polling stalls or reconnects slowly | `openclaw logs --follow` for polling diagnostics | Upgrade; if restarts are false positives, tune `pollingStallThresholdMs`. Persistent stalls still point to proxy/DNS/IPv6. |
| `setMyCommands` rejected at startup | Inspect logs for `BOT_COMMANDS_TOO_MUCH` | Reduce plugin/skill/custom Telegram commands or disable native menus. |
| Upgraded and allowlist blocks you | `openclaw security audit` and config allowlists | Run `openclaw doctor --fix` or replace `@username` with numeric sender IDs. |

Full troubleshooting: [Telegram troubleshooting](https://docs.openclaw.ai/channels/telegram#troubleshooting)

## [​](https://docs.openclaw.ai/channels/troubleshooting\#discord)  Discord

### [​](https://docs.openclaw.ai/channels/troubleshooting\#discord-failure-signatures)  Discord failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| Bot online but no guild replies | `openclaw channels status --probe` | Allow guild/channel and verify message content intent. |
| Group messages ignored | Check logs for mention gating drops | Mention bot or set guild/channel `requireMention: false`. |
| DM replies missing | `openclaw pairing list discord` | Approve DM pairing or adjust DM policy. |

Full troubleshooting: [Discord troubleshooting](https://docs.openclaw.ai/channels/discord#troubleshooting)

## [​](https://docs.openclaw.ai/channels/troubleshooting\#slack)  Slack

### [​](https://docs.openclaw.ai/channels/troubleshooting\#slack-failure-signatures)  Slack failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| Socket mode connected but no responses | `openclaw channels status --probe` | Verify app token + bot token and required scopes; watch for `botTokenStatus` / `appTokenStatus = configured_unavailable` on SecretRef-backed setups. |
| DMs blocked | `openclaw pairing list slack` | Approve pairing or relax DM policy. |
| Channel message ignored | Check `groupPolicy` and channel allowlist | Allow the channel or switch policy to `open`. |

Full troubleshooting: [Slack troubleshooting](https://docs.openclaw.ai/channels/slack#troubleshooting)

## [​](https://docs.openclaw.ai/channels/troubleshooting\#imessage-and-bluebubbles)  iMessage and BlueBubbles

### [​](https://docs.openclaw.ai/channels/troubleshooting\#imessage-and-bluebubbles-failure-signatures)  iMessage and BlueBubbles failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| No inbound events | Verify webhook/server reachability and app permissions | Fix webhook URL or BlueBubbles server state. |
| Can send but no receive on macOS | Check macOS privacy permissions for Messages automation | Re-grant TCC permissions and restart channel process. |
| DM sender blocked | `openclaw pairing list imessage` or `openclaw pairing list bluebubbles` | Approve pairing or update allowlist. |

Full troubleshooting:

- [iMessage troubleshooting](https://docs.openclaw.ai/channels/imessage#troubleshooting)
- [BlueBubbles troubleshooting](https://docs.openclaw.ai/channels/bluebubbles#troubleshooting)

## [​](https://docs.openclaw.ai/channels/troubleshooting\#signal)  Signal

### [​](https://docs.openclaw.ai/channels/troubleshooting\#signal-failure-signatures)  Signal failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| Daemon reachable but bot silent | `openclaw channels status --probe` | Verify `signal-cli` daemon URL/account and receive mode. |
| DM blocked | `openclaw pairing list signal` | Approve sender or adjust DM policy. |
| Group replies do not trigger | Check group allowlist and mention patterns | Add sender/group or loosen gating. |

Full troubleshooting: [Signal troubleshooting](https://docs.openclaw.ai/channels/signal#troubleshooting)

## [​](https://docs.openclaw.ai/channels/troubleshooting\#qq-bot)  QQ Bot

### [​](https://docs.openclaw.ai/channels/troubleshooting\#qq-bot-failure-signatures)  QQ Bot failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| Bot replies “gone to Mars” | Verify `appId` and `clientSecret` in config | Set credentials or restart the gateway. |
| No inbound messages | `openclaw channels status --probe` | Verify credentials on the QQ Open Platform. |
| Voice not transcribed | Check STT provider config | Configure `channels.qqbot.stt` or `tools.media.audio`. |
| Proactive messages not arriving | Check QQ platform interaction requirements | QQ may block bot-initiated messages without recent interaction. |

Full troubleshooting: [QQ Bot troubleshooting](https://docs.openclaw.ai/channels/qqbot#troubleshooting)

## [​](https://docs.openclaw.ai/channels/troubleshooting\#matrix)  Matrix

### [​](https://docs.openclaw.ai/channels/troubleshooting\#matrix-failure-signatures)  Matrix failure signatures

| Symptom | Fastest check | Fix |
| --- | --- | --- |
| Logged in but ignores room messages | `openclaw channels status --probe` | Check `groupPolicy`, room allowlist, and mention gating. |
| DMs do not process | `openclaw pairing list matrix` | Approve sender or adjust DM policy. |
| Encrypted rooms fail | `openclaw matrix verify status` | Re-verify the device, then check `openclaw matrix verify backup status`. |
| Backup restore is pending/broken | `openclaw matrix verify backup status` | Run `openclaw matrix verify backup restore` or rerun with a recovery key. |
| Cross-signing/bootstrap looks wrong | `openclaw matrix verify bootstrap` | Repair secret storage, cross-signing, and backup state in one pass. |

Full setup and config: [Matrix](https://docs.openclaw.ai/channels/matrix)

## [​](https://docs.openclaw.ai/channels/troubleshooting\#related)  Related

- [Pairing](https://docs.openclaw.ai/channels/pairing)
- [Channel routing](https://docs.openclaw.ai/channels/channel-routing)
- [Gateway troubleshooting](https://docs.openclaw.ai/gateway/troubleshooting)

[Channel location parsing](https://docs.openclaw.ai/channels/location) [QA channel](https://docs.openclaw.ai/channels/qa-channel)

Ctrl+I