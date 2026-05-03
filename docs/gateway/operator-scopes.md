---
source_url: https://docs.openclaw.ai/gateway/operator-scopes
title: "Operator scopes - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/operator-scopes#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Security and sandboxing

Operator scopes

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Roles](https://docs.openclaw.ai/gateway/operator-scopes#roles)
- [Scope levels](https://docs.openclaw.ai/gateway/operator-scopes#scope-levels)
- [Method scope is only the first gate](https://docs.openclaw.ai/gateway/operator-scopes#method-scope-is-only-the-first-gate)
- [Device pairing approvals](https://docs.openclaw.ai/gateway/operator-scopes#device-pairing-approvals)
- [Node pairing approvals](https://docs.openclaw.ai/gateway/operator-scopes#node-pairing-approvals)
- [Shared-secret auth](https://docs.openclaw.ai/gateway/operator-scopes#shared-secret-auth)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Operator scopes define what a Gateway client may do after it authenticates.
They are a control-plane guardrail inside one trusted Gateway operator domain,
not hostile multi-tenant isolation. If you need strong separation between
people, teams, or machines, run separate Gateways under separate OS users or
hosts.Related: [Security](https://docs.openclaw.ai/gateway/security), [Gateway protocol](https://docs.openclaw.ai/gateway/protocol),
[Gateway pairing](https://docs.openclaw.ai/gateway/pairing), [Devices CLI](https://docs.openclaw.ai/cli/devices).

## [​](https://docs.openclaw.ai/gateway/operator-scopes\#roles)  Roles

Gateway WebSocket clients connect with one role:

- `operator`: control-plane clients such as CLI, Control UI, automation, and
trusted helper processes.
- `node`: capability hosts such as macOS, iOS, Android, or headless nodes that
expose commands through `node.invoke`.

Operator RPC methods require the `operator` role. Node-originated methods
require the `node` role.

## [​](https://docs.openclaw.ai/gateway/operator-scopes\#scope-levels)  Scope levels

| Scope | Meaning |
| --- | --- |
| `operator.read` | Read-only status, lists, catalog, logs, session reads, and other non-mutating control-plane calls. |
| `operator.write` | Normal mutating operator actions such as sending messages, invoking tools, updating talk/voice settings, and node command relay. Also satisfies `operator.read`. |
| `operator.admin` | Administrative control-plane access. Satisfies every `operator.*` scope. Required for config mutation, updates, native hooks, sensitive reserved namespaces, and high-risk approvals. |
| `operator.pairing` | Device and node pairing management, including listing, approving, rejecting, removing, rotating, and revoking pairing records or device tokens. |
| `operator.approvals` | Exec and plugin approval APIs. |
| `operator.talk.secrets` | Reading Talk configuration with secrets included. |

Unknown future `operator.*` scopes require an exact match unless the caller has
`operator.admin`.

## [​](https://docs.openclaw.ai/gateway/operator-scopes\#method-scope-is-only-the-first-gate)  Method scope is only the first gate

Each Gateway RPC has a least-privilege method scope. That method scope decides
whether the request can reach the handler. Some handlers then apply stricter
approval-time checks based on the concrete thing being approved or mutated.Examples:

- `device.pair.approve` is reachable with `operator.pairing`, but approving an
operator device can only mint or preserve scopes the caller already holds.
- `node.pair.approve` is reachable with `operator.pairing`, then derives extra
approval scopes from the pending node command list.
- `chat.send` is normally a write-scoped method, but persistent `/config set`
and `/config unset` require `operator.admin` at command level.

This lets lower-scope operators perform low-risk pairing actions without making
all pairing approval admin-only.

## [​](https://docs.openclaw.ai/gateway/operator-scopes\#device-pairing-approvals)  Device pairing approvals

Device pairing records are the durable source of approved roles and scopes.
Already paired devices do not get broader access silently: reconnects that ask
for a broader role or broader scopes create a new pending upgrade request.When approving a device request:

- A request with no operator role does not need operator token scope approval.
- A request for `operator.read`, `operator.write`, `operator.approvals`,
`operator.pairing`, or `operator.talk.secrets` requires the caller to hold
those scopes, or `operator.admin`.
- A request for `operator.admin` requires `operator.admin`.
- A repair request with no explicit scopes can inherit the existing operator
token scopes. If that existing token is admin-scoped, approval still requires
`operator.admin`.

For paired-device token sessions, management is self-scoped unless the caller
also has `operator.admin`: non-admin callers can rotate, revoke, or remove only
their own device entry.

## [​](https://docs.openclaw.ai/gateway/operator-scopes\#node-pairing-approvals)  Node pairing approvals

Legacy `node.pair.*` uses a separate Gateway-owned node pairing store. WS nodes
use device pairing with `role: node`, but the same approval-level vocabulary
applies.`node.pair.approve` uses the pending request command list to derive additional
required scopes:

- Commandless request: `operator.pairing`
- Non-exec node commands: `operator.pairing` \+ `operator.write`
- `system.run`, `system.run.prepare`, or `system.which`:
`operator.pairing` \+ `operator.admin`

Node pairing establishes identity and trust. It does not replace the node’s
own `system.run` exec approval policy.

## [​](https://docs.openclaw.ai/gateway/operator-scopes\#shared-secret-auth)  Shared-secret auth

Shared gateway token/password auth is treated as trusted operator access for
that Gateway. OpenAI-compatible HTTP surfaces and `/tools/invoke` restore the
normal full operator default scope set for shared-secret bearer auth, even if a
caller sends narrower declared scopes.Identity-bearing modes, such as trusted proxy auth or private-ingress `none`,
can still honor explicit declared scopes. Use separate Gateways for real trust
boundary separation.

[Security audit checks](https://docs.openclaw.ai/gateway/security/audit-checks) [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing)

Ctrl+I