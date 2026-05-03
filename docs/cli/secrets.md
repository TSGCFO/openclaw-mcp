---
source_url: https://docs.openclaw.ai/cli/secrets
title: "Secrets - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/secrets#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Gateway and service

Secrets

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw secrets](https://docs.openclaw.ai/cli/secrets#openclaw-secrets)
- [Reload runtime snapshot](https://docs.openclaw.ai/cli/secrets#reload-runtime-snapshot)
- [Audit](https://docs.openclaw.ai/cli/secrets#audit)
- [Configure (interactive helper)](https://docs.openclaw.ai/cli/secrets#configure-interactive-helper)
- [Apply a saved plan](https://docs.openclaw.ai/cli/secrets#apply-a-saved-plan)
- [Why no rollback backups](https://docs.openclaw.ai/cli/secrets#why-no-rollback-backups)
- [Example](https://docs.openclaw.ai/cli/secrets#example)
- [Related](https://docs.openclaw.ai/cli/secrets#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/secrets\#openclaw-secrets)  `openclaw secrets`

Use `openclaw secrets` to manage SecretRefs and keep the active runtime snapshot healthy.Command roles:

- `reload`: gateway RPC (`secrets.reload`) that re-resolves refs and swaps runtime snapshot only on full success (no config writes).
- `audit`: read-only scan of configuration/auth/generated-model stores and legacy residues for plaintext, unresolved refs, and precedence drift (exec refs are skipped unless `--allow-exec` is set).
- `configure`: interactive planner for provider setup, target mapping, and preflight (TTY required).
- `apply`: execute a saved plan (`--dry-run` for validation only; dry-run skips exec checks by default, and write mode rejects exec-containing plans unless `--allow-exec` is set), then scrub targeted plaintext residues.

Recommended operator loop:

```
openclaw secrets audit --check
openclaw secrets configure
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json
openclaw secrets audit --check
openclaw secrets reload
```

If your plan includes `exec` SecretRefs/providers, pass `--allow-exec` on both dry-run and write apply commands.Exit code note for CI/gates:

- `audit --check` returns `1` on findings.
- unresolved refs return `2`.

Related:

- Secrets guide: [Secrets Management](https://docs.openclaw.ai/gateway/secrets)
- Credential surface: [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface)
- Security guide: [Security](https://docs.openclaw.ai/gateway/security)

## [​](https://docs.openclaw.ai/cli/secrets\#reload-runtime-snapshot)  Reload runtime snapshot

Re-resolve secret refs and atomically swap runtime snapshot.

```
openclaw secrets reload
openclaw secrets reload --json
openclaw secrets reload --url ws://127.0.0.1:18789 --token <token>
```

Notes:

- Uses gateway RPC method `secrets.reload`.
- If resolution fails, gateway keeps last-known-good snapshot and returns an error (no partial activation).
- JSON response includes `warningCount`.

Options:

- `--url <url>`
- `--token <token>`
- `--timeout <ms>`
- `--json`

## [​](https://docs.openclaw.ai/cli/secrets\#audit)  Audit

Scan OpenClaw state for:

- plaintext secret storage
- unresolved refs
- precedence drift (`auth-profiles.json` credentials shadowing `openclaw.json` refs)
- generated `agents/*/agent/models.json` residues (provider `apiKey` values and sensitive provider headers)
- legacy residues (legacy auth store entries, OAuth reminders)

Header residue note:

- Sensitive provider header detection is name-heuristic based (common auth/credential header names and fragments such as `authorization`, `x-api-key`, `token`, `secret`, `password`, and `credential`).

```
openclaw secrets audit
openclaw secrets audit --check
openclaw secrets audit --json
openclaw secrets audit --allow-exec
```

Exit behavior:

- `--check` exits non-zero on findings.
- unresolved refs exit with higher-priority non-zero code.

Report shape highlights:

- `status`: `clean | findings | unresolved`
- `resolution`: `refsChecked`, `skippedExecRefs`, `resolvabilityComplete`
- `summary`: `plaintextCount`, `unresolvedRefCount`, `shadowedRefCount`, `legacyResidueCount`
- finding codes:
  - `PLAINTEXT_FOUND`
  - `REF_UNRESOLVED`
  - `REF_SHADOWED`
  - `LEGACY_RESIDUE`

## [​](https://docs.openclaw.ai/cli/secrets\#configure-interactive-helper)  Configure (interactive helper)

Build provider and SecretRef changes interactively, run preflight, and optionally apply:

```
openclaw secrets configure
openclaw secrets configure --plan-out /tmp/openclaw-secrets-plan.json
openclaw secrets configure --apply --yes
openclaw secrets configure --providers-only
openclaw secrets configure --skip-provider-setup
openclaw secrets configure --agent ops
openclaw secrets configure --json
```

Flow:

- Provider setup first (`add/edit/remove` for `secrets.providers` aliases).
- Credential mapping second (select fields and assign `{source, provider, id}` refs).
- Preflight and optional apply last.

Flags:

- `--providers-only`: configure `secrets.providers` only, skip credential mapping.
- `--skip-provider-setup`: skip provider setup and map credentials to existing providers.
- `--agent <id>`: scope `auth-profiles.json` target discovery and writes to one agent store.
- `--allow-exec`: allow exec SecretRef checks during preflight/apply (may execute provider commands).

Notes:

- Requires an interactive TTY.
- You cannot combine `--providers-only` with `--skip-provider-setup`.
- `configure` targets secret-bearing fields in `openclaw.json` plus `auth-profiles.json` for the selected agent scope.
- `configure` supports creating new `auth-profiles.json` mappings directly in the picker flow.
- Canonical supported surface: [SecretRef Credential Surface](https://docs.openclaw.ai/reference/secretref-credential-surface).
- It performs preflight resolution before apply.
- If preflight/apply includes exec refs, keep `--allow-exec` set for both steps.
- Generated plans default to scrub options (`scrubEnv`, `scrubAuthProfilesForProviderTargets`, `scrubLegacyAuthJson` all enabled).
- Apply path is one-way for scrubbed plaintext values.
- Without `--apply`, CLI still prompts `Apply this plan now?` after preflight.
- With `--apply` (and no `--yes`), CLI prompts an extra irreversible confirmation.
- `--json` prints the plan + preflight report, but the command still requires an interactive TTY.

Exec provider safety note:

- Homebrew installs often expose symlinked binaries under `/opt/homebrew/bin/*`.
- Set `allowSymlinkCommand: true` only when needed for trusted package-manager paths, and pair it with `trustedDirs` (for example `["/opt/homebrew"]`).
- On Windows, if ACL verification is unavailable for a provider path, OpenClaw fails closed. For trusted paths only, set `allowInsecurePath: true` on that provider to bypass path security checks.

## [​](https://docs.openclaw.ai/cli/secrets\#apply-a-saved-plan)  Apply a saved plan

Apply or preflight a plan generated previously:

```
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --allow-exec
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --dry-run --allow-exec
openclaw secrets apply --from /tmp/openclaw-secrets-plan.json --json
```

Exec behavior:

- `--dry-run` validates preflight without writing files.
- exec SecretRef checks are skipped by default in dry-run.
- write mode rejects plans that contain exec SecretRefs/providers unless `--allow-exec` is set.
- Use `--allow-exec` to opt in to exec provider checks/execution in either mode.

Plan contract details (allowed target paths, validation rules, and failure semantics):

- [Secrets Apply Plan Contract](https://docs.openclaw.ai/gateway/secrets-plan-contract)

What `apply` may update:

- `openclaw.json` (SecretRef targets + provider upserts/deletes)
- `auth-profiles.json` (provider-target scrubbing)
- legacy `auth.json` residues
- `~/.openclaw/.env` known secret keys whose values were migrated

## [​](https://docs.openclaw.ai/cli/secrets\#why-no-rollback-backups)  Why no rollback backups

`secrets apply` intentionally does not write rollback backups containing old plaintext values.Safety comes from strict preflight + atomic-ish apply with best-effort in-memory restore on failure.

## [​](https://docs.openclaw.ai/cli/secrets\#example)  Example

```
openclaw secrets audit --check
openclaw secrets configure
openclaw secrets audit --check
```

If `audit --check` still reports plaintext findings, update the remaining reported target paths and rerun audit.

## [​](https://docs.openclaw.ai/cli/secrets\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Secrets management](https://docs.openclaw.ai/gateway/secrets)

[Reset](https://docs.openclaw.ai/cli/reset) [Security](https://docs.openclaw.ai/cli/security)

Ctrl+I