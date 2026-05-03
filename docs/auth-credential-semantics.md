---
source_url: https://docs.openclaw.ai/auth-credential-semantics
title: "Auth credential semantics - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/auth-credential-semantics#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Authentication and secrets

Auth credential semantics

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Stable probe reason codes](https://docs.openclaw.ai/auth-credential-semantics#stable-probe-reason-codes)
- [Token credentials](https://docs.openclaw.ai/auth-credential-semantics#token-credentials)
- [Eligibility rules](https://docs.openclaw.ai/auth-credential-semantics#eligibility-rules)
- [Resolution rules](https://docs.openclaw.ai/auth-credential-semantics#resolution-rules)
- [Agent copy portability](https://docs.openclaw.ai/auth-credential-semantics#agent-copy-portability)
- [Explicit auth order filtering](https://docs.openclaw.ai/auth-credential-semantics#explicit-auth-order-filtering)
- [Probe target resolution](https://docs.openclaw.ai/auth-credential-semantics#probe-target-resolution)
- [External CLI credential discovery](https://docs.openclaw.ai/auth-credential-semantics#external-cli-credential-discovery)
- [OAuth SecretRef Policy Guard](https://docs.openclaw.ai/auth-credential-semantics#oauth-secretref-policy-guard)
- [Legacy-Compatible Messaging](https://docs.openclaw.ai/auth-credential-semantics#legacy-compatible-messaging)
- [Related](https://docs.openclaw.ai/auth-credential-semantics#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

This document defines the canonical credential eligibility and resolution semantics used across:

- `resolveAuthProfileOrder`
- `resolveApiKeyForProfile`
- `models status --probe`
- `doctor-auth`

The goal is to keep selection-time and runtime behavior aligned.

## [​](https://docs.openclaw.ai/auth-credential-semantics\#stable-probe-reason-codes)  Stable probe reason codes

- `ok`
- `excluded_by_auth_order`
- `missing_credential`
- `invalid_expires`
- `expired`
- `unresolved_ref`
- `no_model`

## [​](https://docs.openclaw.ai/auth-credential-semantics\#token-credentials)  Token credentials

Token credentials (`type: "token"`) support inline `token` and/or `tokenRef`.

### [​](https://docs.openclaw.ai/auth-credential-semantics\#eligibility-rules)  Eligibility rules

1. A token profile is ineligible when both `token` and `tokenRef` are absent.
2. `expires` is optional.
3. If `expires` is present, it must be a finite number greater than `0`.
4. If `expires` is invalid (`NaN`, `0`, negative, non-finite, or wrong type), the profile is ineligible with `invalid_expires`.
5. If `expires` is in the past, the profile is ineligible with `expired`.
6. `tokenRef` does not bypass `expires` validation.

### [​](https://docs.openclaw.ai/auth-credential-semantics\#resolution-rules)  Resolution rules

1. Resolver semantics match eligibility semantics for `expires`.
2. For eligible profiles, token material may be resolved from inline value or `tokenRef`.
3. Unresolvable refs produce `unresolved_ref` in `models status --probe` output.

## [​](https://docs.openclaw.ai/auth-credential-semantics\#agent-copy-portability)  Agent copy portability

Agent auth inheritance is read-through. When an agent has no local profile, it
can resolve profiles from the default/main agent store at runtime without
copying secret material into its own `auth-profiles.json`.Explicit copy flows, such as `openclaw agents add`, use this portability policy:

- `api_key` profiles are portable unless `copyToAgents: false`.
- `token` profiles are portable unless `copyToAgents: false`.
- `oauth` profiles are not portable by default because refresh tokens can be
single-use or rotation-sensitive.
- Provider-owned OAuth flows may opt in with `copyToAgents: true` only when
copying refresh material across agents is known safe.

Non-portable profiles remain available through read-through inheritance unless
the target agent signs in separately and creates its own local profile.

## [​](https://docs.openclaw.ai/auth-credential-semantics\#explicit-auth-order-filtering)  Explicit auth order filtering

- When `auth.order.<provider>` or the auth-store order override is set for a
provider, `models status --probe` only probes profile ids that remain in the
resolved auth order for that provider.
- A stored profile for that provider that is omitted from the explicit order is
not silently tried later. Probe output reports it with
`reasonCode: excluded_by_auth_order` and the detail
`Excluded by auth.order for this provider.`

## [​](https://docs.openclaw.ai/auth-credential-semantics\#probe-target-resolution)  Probe target resolution

- Probe targets can come from auth profiles, environment credentials, or
`models.json`.
- If a provider has credentials but OpenClaw cannot resolve a probeable model
candidate for it, `models status --probe` reports `status: no_model` with
`reasonCode: no_model`.

## [​](https://docs.openclaw.ai/auth-credential-semantics\#external-cli-credential-discovery)  External CLI credential discovery

- Runtime-only credentials owned by external CLIs are discovered only when the
provider, runtime, or auth profile is in scope for the current operation, or
when a stored local profile for that external source already exists.
- Auth-store callers should choose an explicit external-CLI discovery mode:
`none` for persisted/plugin auth only, `existing` for refreshing already
stored external CLI profiles, or `scoped` for a concrete provider/profile set.
- Read-only/status paths pass `allowKeychainPrompt: false`; they use file-backed
external CLI credentials only and do not read or reuse macOS Keychain results.

## [​](https://docs.openclaw.ai/auth-credential-semantics\#oauth-secretref-policy-guard)  OAuth SecretRef Policy Guard

- SecretRef input is for static credentials only.
- If a profile credential is `type: "oauth"`, SecretRef objects are not supported for that profile credential material.
- If `auth.profiles.<id>.mode` is `"oauth"`, SecretRef-backed `keyRef`/`tokenRef` input for that profile is rejected.
- Violations are hard failures in startup/reload auth resolution paths.

## [​](https://docs.openclaw.ai/auth-credential-semantics\#legacy-compatible-messaging)  Legacy-Compatible Messaging

For script compatibility, probe errors keep this first line unchanged:`Auth profile credentials are missing or expired.`Human-friendly detail and stable reason codes may be added on subsequent lines.

## [​](https://docs.openclaw.ai/auth-credential-semantics\#related)  Related

- [Secrets management](https://docs.openclaw.ai/gateway/secrets)
- [Auth storage](https://docs.openclaw.ai/concepts/oauth)

[Authentication](https://docs.openclaw.ai/gateway/authentication) [Secrets management](https://docs.openclaw.ai/gateway/secrets)

Ctrl+I