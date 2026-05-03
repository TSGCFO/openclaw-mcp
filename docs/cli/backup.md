---
source_url: https://docs.openclaw.ai/cli/backup
title: "Backup - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/backup#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Gateway and service

Backup

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw backup](https://docs.openclaw.ai/cli/backup#openclaw-backup)
- [Notes](https://docs.openclaw.ai/cli/backup#notes)
- [What gets backed up](https://docs.openclaw.ai/cli/backup#what-gets-backed-up)
- [Invalid config behavior](https://docs.openclaw.ai/cli/backup#invalid-config-behavior)
- [Size and performance](https://docs.openclaw.ai/cli/backup#size-and-performance)
- [Related](https://docs.openclaw.ai/cli/backup#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/backup\#openclaw-backup)  `openclaw backup`

Create a local backup archive for OpenClaw state, config, auth profiles, channel/provider credentials, sessions, and optionally workspaces.

```
openclaw backup create
openclaw backup create --output ~/Backups
openclaw backup create --dry-run --json
openclaw backup create --verify
openclaw backup create --no-include-workspace
openclaw backup create --only-config
openclaw backup verify ./2026-03-09T00-00-00.000Z-openclaw-backup.tar.gz
```

## [​](https://docs.openclaw.ai/cli/backup\#notes)  Notes

- The archive includes a `manifest.json` file with the resolved source paths and archive layout.
- Default output is a timestamped `.tar.gz` archive in the current working directory.
- If the current working directory is inside a backed-up source tree, OpenClaw falls back to your home directory for the default archive location.
- Existing archive files are never overwritten.
- Output paths inside the source state/workspace trees are rejected to avoid self-inclusion.
- `openclaw backup verify <archive>` validates that the archive contains exactly one root manifest, rejects traversal-style archive paths, and checks that every manifest-declared payload exists in the tarball.
- `openclaw backup create --verify` runs that validation immediately after writing the archive.
- `openclaw backup create --only-config` backs up just the active JSON config file.

## [​](https://docs.openclaw.ai/cli/backup\#what-gets-backed-up)  What gets backed up

`openclaw backup create` plans backup sources from your local OpenClaw install:

- The state directory returned by OpenClaw’s local state resolver, usually `~/.openclaw`
- The active config file path
- The resolved `credentials/` directory when it exists outside the state directory
- Workspace directories discovered from the current config, unless you pass `--no-include-workspace`

Model auth profiles are already part of the state directory under
`agents/<agentId>/agent/auth-profiles.json`, so they are normally covered by the
state backup entry.If you use `--only-config`, OpenClaw skips state, credentials-directory, and workspace discovery and archives only the active config file path.OpenClaw canonicalizes paths before building the archive. If config, the
credentials directory, or a workspace already live inside the state directory,
they are not duplicated as separate top-level backup sources. Missing paths are
skipped.The archive payload stores file contents from those source trees, and the embedded `manifest.json` records the resolved absolute source paths plus the archive layout used for each asset.Installed plugin source and manifest files under the state directory’s
`extensions/` tree are included, but their nested `node_modules/` dependency
trees are skipped. Those dependencies are rebuildable install artifacts; after
restoring an archive, use `openclaw plugins update <id>` or reinstall the plugin
with `openclaw plugins install <spec> --force` when a restored plugin reports
missing dependencies.

## [​](https://docs.openclaw.ai/cli/backup\#invalid-config-behavior)  Invalid config behavior

`openclaw backup` intentionally bypasses the normal config preflight so it can still help during recovery. Because workspace discovery depends on a valid config, `openclaw backup create` now fails fast when the config file exists but is invalid and workspace backup is still enabled.If you still want a partial backup in that situation, rerun:

```
openclaw backup create --no-include-workspace
```

That keeps state, config, and the external credentials directory in scope while
skipping workspace discovery entirely.If you only need a copy of the config file itself, `--only-config` also works when the config is malformed because it does not rely on parsing the config for workspace discovery.

## [​](https://docs.openclaw.ai/cli/backup\#size-and-performance)  Size and performance

OpenClaw does not enforce a built-in maximum backup size or per-file size limit.Practical limits come from the local machine and destination filesystem:

- Available space for the temporary archive write plus the final archive
- Time to walk large workspace trees and compress them into a `.tar.gz`
- Time to rescan the archive if you use `openclaw backup create --verify` or run `openclaw backup verify`
- Filesystem behavior at the destination path. OpenClaw prefers a no-overwrite hard-link publish step and falls back to exclusive copy when hard links are unsupported

Large workspaces are usually the main driver of archive size. If you want a smaller or faster backup, use `--no-include-workspace`.For the smallest archive, use `--only-config`.

## [​](https://docs.openclaw.ai/cli/backup\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)

[CLI reference](https://docs.openclaw.ai/cli) [Crestodian](https://docs.openclaw.ai/cli/crestodian)

Ctrl+I