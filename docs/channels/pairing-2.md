---
source_url: https://docs.openclaw.ai/channels/pairing
title: "Pairing - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels/pairing#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Configuration

Pairing

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [1) DM pairing (inbound chat access)](https://docs.openclaw.ai/channels/pairing#1-dm-pairing-inbound-chat-access)
- [Approve a sender](https://docs.openclaw.ai/channels/pairing#approve-a-sender)
- [Reusable sender groups](https://docs.openclaw.ai/channels/pairing#reusable-sender-groups)
- [Where the state lives](https://docs.openclaw.ai/channels/pairing#where-the-state-lives)
- [2) Node device pairing (iOS/Android/macOS/headless nodes)](https://docs.openclaw.ai/channels/pairing#2-node-device-pairing-ios%2Fandroid%2Fmacos%2Fheadless-nodes)
- [Pair via Telegram (recommended for iOS)](https://docs.openclaw.ai/channels/pairing#pair-via-telegram-recommended-for-ios)
- [Approve a node device](https://docs.openclaw.ai/channels/pairing#approve-a-node-device)
- [Optional trusted-CIDR node auto-approve](https://docs.openclaw.ai/channels/pairing#optional-trusted-cidr-node-auto-approve)
- [Node pairing state storage](https://docs.openclaw.ai/channels/pairing#node-pairing-state-storage)
- [Notes](https://docs.openclaw.ai/channels/pairing#notes)
- [Related docs](https://docs.openclaw.ai/channels/pairing#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

“Pairing” is OpenClaw’s explicit access approval step.
It is used in two places:

1. **DM pairing** (who is allowed to talk to the bot)
2. **Node pairing** (which devices/nodes are allowed to join the gateway network)

Security context: [Security](https://docs.openclaw.ai/gateway/security)

## [​](https://docs.openclaw.ai/channels/pairing\#1-dm-pairing-inbound-chat-access)  1) DM pairing (inbound chat access)

When a channel is configured with DM policy `pairing`, unknown senders get a short code and their message is **not processed** until you approve.Default DM policies are documented in: [Security](https://docs.openclaw.ai/gateway/security)`dmPolicy: "open"` is public only when the effective DM allowlist includes `"*"`.
Setup and validation require that wildcard for public-open configs. If existing
state contains `open` with concrete `allowFrom` entries, runtime still admits
only those senders, and pairing-store approvals do not widen `open` access.Pairing codes:

- 8 characters, uppercase, no ambiguous chars (`0O1I`).
- **Expire after 1 hour**. The bot only sends the pairing message when a new request is created (roughly once per hour per sender).
- Pending DM pairing requests are capped at **3 per channel** by default; additional requests are ignored until one expires or is approved.

### [​](https://docs.openclaw.ai/channels/pairing\#approve-a-sender)  Approve a sender

```
openclaw pairing list telegram
openclaw pairing approve telegram <CODE>
```

If no command owner is configured yet, approving a DM pairing code also bootstraps
`commands.ownerAllowFrom` to the approved sender, such as `telegram:123456789`.
That gives first-time setups an explicit owner for privileged commands and exec
approval prompts. After an owner exists, later pairing approvals only grant DM
access; they do not add more owners.Supported channels: `bluebubbles`, `discord`, `feishu`, `googlechat`, `imessage`, `irc`, `line`, `matrix`, `mattermost`, `msteams`, `nextcloud-talk`, `nostr`, `openclaw-weixin`, `signal`, `slack`, `synology-chat`, `telegram`, `twitch`, `whatsapp`, `zalo`, `zalouser`.

### [​](https://docs.openclaw.ai/channels/pairing\#reusable-sender-groups)  Reusable sender groups

Use top-level `accessGroups` when the same trusted sender set should apply to
multiple message channels or to both DM and group allowlists.Static groups use `type: "message.senders"` and are referenced with
`accessGroup:<name>` from channel allowlists:

```
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        discord: ["discord:123456789012345678"],
        telegram: ["987654321"],
        whatsapp: ["+15551234567"],
      },
    },
  },
  channels: {
    telegram: { dmPolicy: "allowlist", allowFrom: ["accessGroup:operators"] },
    whatsapp: { groupPolicy: "allowlist", groupAllowFrom: ["accessGroup:operators"] },
  },
}
```

Access groups are documented in detail here: [Access groups](https://docs.openclaw.ai/channels/access-groups)

### [​](https://docs.openclaw.ai/channels/pairing\#where-the-state-lives)  Where the state lives

Stored under `~/.openclaw/credentials/`:

- Pending requests: `<channel>-pairing.json`
- Approved allowlist store:
  - Default account: `<channel>-allowFrom.json`
  - Non-default account: `<channel>-<accountId>-allowFrom.json`

Account scoping behavior:

- Non-default accounts read/write only their scoped allowlist file.
- Default account uses the channel-scoped unscoped allowlist file.

Treat these as sensitive (they gate access to your assistant).

The pairing allowlist store is for DM access. Group authorization is separate.
Approving a DM pairing code does not automatically allow that sender to run group
commands or control the bot in groups. First-owner bootstrap is separate config
state in `commands.ownerAllowFrom`, and group chat delivery still follows the
channel’s group allowlists (for example `groupAllowFrom`, `groups`, or per-group
or per-topic overrides depending on the channel).

## [​](https://docs.openclaw.ai/channels/pairing\#2-node-device-pairing-ios/android/macos/headless-nodes)  2) Node device pairing (iOS/Android/macOS/headless nodes)

Nodes connect to the Gateway as **devices** with `role: node`. The Gateway
creates a device pairing request that must be approved.

### [​](https://docs.openclaw.ai/channels/pairing\#pair-via-telegram-recommended-for-ios)  Pair via Telegram (recommended for iOS)

If you use the `device-pair` plugin, you can do first-time device pairing entirely from Telegram:

1. In Telegram, message your bot: `/pair`
2. The bot replies with two messages: an instruction message and a separate **setup code** message (easy to copy/paste in Telegram).
3. On your phone, open the OpenClaw iOS app → Settings → Gateway.
4. Paste the setup code and connect.
5. Back in Telegram: `/pair pending` (review request IDs, role, and scopes), then approve.

The setup code is a base64-encoded JSON payload that contains:

- `url`: the Gateway WebSocket URL (`ws://...` or `wss://...`)
- `bootstrapToken`: a short-lived single-device bootstrap token used for the initial pairing handshake

That bootstrap token carries the built-in pairing bootstrap profile:

- primary handed-off `node` token stays `scopes: []`
- any handed-off `operator` token stays bounded to the bootstrap allowlist:
`operator.approvals`, `operator.read`, `operator.talk.secrets`, `operator.write`
- bootstrap scope checks are role-prefixed, not one flat scope pool:
operator scope entries only satisfy operator requests, and non-operator roles
must still request scopes under their own role prefix
- later token rotation/revocation remains bounded by both the device’s approved
role contract and the caller session’s operator scopes

Treat the setup code like a password while it is valid.

### [​](https://docs.openclaw.ai/channels/pairing\#approve-a-node-device)  Approve a node device

```
openclaw devices list
openclaw devices approve <requestId>
openclaw devices reject <requestId>
```

If the same device retries with different auth details (for example different
role/scopes/public key), the previous pending request is superseded and a new
`requestId` is created.

An already paired device does not get broader access silently. If it reconnects asking for more scopes or a broader role, OpenClaw keeps the existing approval as-is and creates a fresh pending upgrade request. Use `openclaw devices list` to compare the currently approved access with the newly requested access before you approve.

### [​](https://docs.openclaw.ai/channels/pairing\#optional-trusted-cidr-node-auto-approve)  Optional trusted-CIDR node auto-approve

Device pairing remains manual by default. For tightly controlled node networks,
you can opt in to first-time node auto-approval with explicit CIDRs or exact IPs:

```
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24"],
      },
    },
  },
}
```

This only applies to fresh `role: node` pairing requests with no requested
scopes. Operator, browser, Control UI, and WebChat clients still require manual
approval. Role, scope, metadata, and public-key changes still require manual
approval.

### [​](https://docs.openclaw.ai/channels/pairing\#node-pairing-state-storage)  Node pairing state storage

Stored under `~/.openclaw/devices/`:

- `pending.json` (short-lived; pending requests expire)
- `paired.json` (paired devices + tokens)

### [​](https://docs.openclaw.ai/channels/pairing\#notes)  Notes

- The legacy `node.pair.*` API (CLI: `openclaw nodes pending|approve|reject|remove|rename`) is a
separate gateway-owned pairing store. WS nodes still require device pairing.
- The pairing record is the durable source of truth for approved roles. Active
device tokens stay bounded to that approved role set; a stray token entry
outside the approved roles does not create new access.

## [​](https://docs.openclaw.ai/channels/pairing\#related-docs)  Related docs

- Security model + prompt injection: [Security](https://docs.openclaw.ai/gateway/security)
- Updating safely (run doctor): [Updating](https://docs.openclaw.ai/install/updating)
- Channel configs:
  - Telegram: [Telegram](https://docs.openclaw.ai/channels/telegram)
  - WhatsApp: [WhatsApp](https://docs.openclaw.ai/channels/whatsapp)
  - Signal: [Signal](https://docs.openclaw.ai/channels/signal)
  - BlueBubbles (iMessage): [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles)
  - iMessage (legacy): [iMessage](https://docs.openclaw.ai/channels/imessage)
  - Discord: [Discord](https://docs.openclaw.ai/channels/discord)
  - Slack: [Slack](https://docs.openclaw.ai/channels/slack)

[Zalo personal](https://docs.openclaw.ai/channels/zalouser) [Access groups](https://docs.openclaw.ai/channels/access-groups)

Ctrl+I