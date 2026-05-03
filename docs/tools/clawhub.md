---
source_url: https://docs.openclaw.ai/tools/clawhub
title: "ClawHub - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/clawhub#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Skills

ClawHub

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/tools/clawhub#quick-start)
- [Native OpenClaw flows](https://docs.openclaw.ai/tools/clawhub#native-openclaw-flows)
- [What ClawHub is](https://docs.openclaw.ai/tools/clawhub#what-clawhub-is)
- [Workspace and skill loading](https://docs.openclaw.ai/tools/clawhub#workspace-and-skill-loading)
- [Service features](https://docs.openclaw.ai/tools/clawhub#service-features)
- [Security and moderation](https://docs.openclaw.ai/tools/clawhub#security-and-moderation)
- [ClawHub CLI](https://docs.openclaw.ai/tools/clawhub#clawhub-cli)
- [Global options](https://docs.openclaw.ai/tools/clawhub#global-options)
- [Commands](https://docs.openclaw.ai/tools/clawhub#commands)
- [Common workflows](https://docs.openclaw.ai/tools/clawhub#common-workflows)
- [Plugin package metadata](https://docs.openclaw.ai/tools/clawhub#plugin-package-metadata)
- [Versioning, lockfile, and telemetry](https://docs.openclaw.ai/tools/clawhub#versioning-lockfile-and-telemetry)
- [Environment variables](https://docs.openclaw.ai/tools/clawhub#environment-variables)
- [Related](https://docs.openclaw.ai/tools/clawhub#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

ClawHub is the public registry for **OpenClaw skills and plugins**.

- Use native `openclaw` commands to search, install, and update skills, and to install plugins from ClawHub.
- Use the separate `clawhub` CLI for registry auth, publish, delete/undelete, and sync workflows.

Site: [clawhub.ai](https://clawhub.ai/)

## [​](https://docs.openclaw.ai/tools/clawhub\#quick-start)  Quick start

1

[Navigate to header](https://docs.openclaw.ai/tools/clawhub#)

Search

```
openclaw skills search "calendar"
```

2

[Navigate to header](https://docs.openclaw.ai/tools/clawhub#)

Install

```
openclaw skills install <skill-slug>
```

3

[Navigate to header](https://docs.openclaw.ai/tools/clawhub#)

Use

Start a new OpenClaw session — it picks up the new skill.

4

[Navigate to header](https://docs.openclaw.ai/tools/clawhub#)

Publish (optional)

For registry-authenticated workflows (publish, sync, manage), install
the separate `clawhub` CLI:

```
npm i -g clawhub
# or
pnpm add -g clawhub
```

## [​](https://docs.openclaw.ai/tools/clawhub\#native-openclaw-flows)  Native OpenClaw flows

- Skills

- Plugins


```
openclaw skills search "calendar"
openclaw skills install <skill-slug>
openclaw skills update --all
```

Native `openclaw` commands install into your active workspace and
persist source metadata so later `update` calls can stay on ClawHub.

```
openclaw plugins search "calendar"
openclaw plugins install clawhub:<package>
openclaw plugins update --all
```

`plugins search` queries the ClawHub plugin catalog and prints install-ready
package names. Bare npm-safe plugin specs are also tried against ClawHub
before npm:

```
openclaw plugins install openclaw-codex-app-server
```

Use `npm:<package>` when you want npm-only resolution without a
ClawHub lookup:

```
openclaw plugins install npm:openclaw-codex-app-server
```

Plugin installs validate advertised `pluginApi` and
`minGatewayVersion` compatibility before archive install runs, so
incompatible hosts fail closed early instead of partially installing
the package. When a package version publishes a ClawPack artifact,
OpenClaw prefers that artifact, verifies the ClawHub digest header and
downloaded bytes, and records the ClawPack digest metadata for later
updates. Older package versions without ClawPack metadata still use the
legacy package archive verification path.

`openclaw plugins install clawhub:...` only accepts installable plugin
families. If a ClawHub package is actually a skill, OpenClaw stops and
points you at `openclaw skills install <slug>` instead.Anonymous ClawHub plugin installs also fail closed for private packages.
Community or other non-official channels can still install, but OpenClaw
warns so operators can review source and verification before enabling
them.

## [​](https://docs.openclaw.ai/tools/clawhub\#what-clawhub-is)  What ClawHub is

- A public registry for OpenClaw skills and plugins.
- A versioned store of skill bundles and metadata.
- A discovery surface for search, tags, and usage signals.

A typical skill is a versioned bundle of files that includes:

- A `SKILL.md` file with the primary description and usage.
- Optional configs, scripts, or supporting files used by the skill.
- Metadata such as tags, summary, and install requirements.

ClawHub uses metadata to power discovery and safely expose skill
capabilities. The registry tracks usage signals (stars, downloads) to
improve ranking and visibility. Each publish creates a new semver
version, and the registry keeps version history so users can audit
changes.

## [​](https://docs.openclaw.ai/tools/clawhub\#workspace-and-skill-loading)  Workspace and skill loading

The separate `clawhub` CLI also installs skills into `./skills` under
your current working directory. If an OpenClaw workspace is configured,
`clawhub` falls back to that workspace unless you override `--workdir`
(or `CLAWHUB_WORKDIR`). OpenClaw loads workspace skills from
`<workspace>/skills` and picks them up in the **next** session.If you already use `~/.openclaw/skills` or bundled skills, workspace
skills take precedence. For more detail on how skills are loaded,
shared, and gated, see [Skills](https://docs.openclaw.ai/tools/skills).

## [​](https://docs.openclaw.ai/tools/clawhub\#service-features)  Service features

| Feature | Notes |
| --- | --- |
| Public browsing | Skills and their `SKILL.md` content are publicly viewable. |
| Search | Embedding-powered (vector search), not just keywords. |
| Versioning | Semver, changelogs, and tags (including `latest`). |
| Downloads | Zip per version. |
| Stars and comments | Community feedback. |
| Security scan summaries | Detail pages show the latest scan state before install or download. |
| Scanner detail pages | VirusTotal, ClawScan, and static-analysis results have deep links. |
| Owner recovery dashboard | Publishers can see scan-held owned content from `/dashboard`. |
| Owner-requested rescans | Owners can request limited rescans for false-positive recovery. |
| Moderation | Approvals and audits. |
| CLI-friendly API | Suitable for automation and scripting. |

## [​](https://docs.openclaw.ai/tools/clawhub\#security-and-moderation)  Security and moderation

ClawHub is open by default — anyone can upload skills, but a GitHub
account must be **at least one week old** to publish. This slows down
abuse without blocking legitimate contributors.

Security scans

ClawHub runs automated security checks on published skills and plugin
releases. Public detail pages summarize the current result, and scanner
rows link to dedicated detail pages for VirusTotal, ClawScan, and static
analysis.Scan-held or blocked releases may be unavailable on public catalog and
install surfaces while still visible to their owner in `/dashboard`.

Reporting

- Any signed-in user can report a skill.
- Report reasons are required and recorded.
- Each user can have up to 20 active reports at a time.
- Skills with more than 3 unique reports are auto-hidden by default.

Moderation

- Moderators can view hidden skills, unhide them, delete them, or ban users.
- Abusing the report feature can result in account bans.
- Interested in becoming a moderator? Ask in the OpenClaw Discord and contact a moderator or maintainer.

## [​](https://docs.openclaw.ai/tools/clawhub\#clawhub-cli)  ClawHub CLI

You only need this for registry-authenticated workflows such as
publish/sync.

### [​](https://docs.openclaw.ai/tools/clawhub\#global-options)  Global options

[​](https://docs.openclaw.ai/tools/clawhub#param-workdir-dir)

--workdir <dir>

string

Working directory. Default: current dir; falls back to OpenClaw workspace.

[​](https://docs.openclaw.ai/tools/clawhub#param-dir-dir)

--dir <dir>

string

default:"skills"

Skills directory, relative to workdir.

[​](https://docs.openclaw.ai/tools/clawhub#param-site-url)

--site <url>

string

Site base URL (browser login).

[​](https://docs.openclaw.ai/tools/clawhub#param-registry-url)

--registry <url>

string

Registry API base URL.

[​](https://docs.openclaw.ai/tools/clawhub#param-no-input)

--no-input

boolean

Disable prompts (non-interactive).

[​](https://docs.openclaw.ai/tools/clawhub#param-v-cli-version)

-V, --cli-version

boolean

Print CLI version.

### [​](https://docs.openclaw.ai/tools/clawhub\#commands)  Commands

Auth (login / logout / whoami)

```
clawhub login              # browser flow
clawhub login --token <token>
clawhub logout
clawhub whoami
```

Login options:

- `--token <token>` — paste an API token.
- `--label <label>` — label stored for browser login tokens (default: `CLI token`).
- `--no-browser` — do not open a browser (requires `--token`).

Search

```
clawhub search "query"
```

Searches skills. For plugin/package discovery, use `clawhub package explore`.

- `--limit <n>` — max results.

Browse / inspect plugins

```
clawhub package explore --family code-plugin
clawhub package explore "episodic-claw" --family code-plugin
clawhub package inspect episodic-claw
```

`package explore` and `package inspect` are the ClawHub CLI surfaces for plugin/package discovery and metadata inspection. Native OpenClaw installs still use `openclaw plugins install clawhub:<package>`.Options:

- `--family skill|code-plugin|bundle-plugin` — filter package family.
- `--official` — show only official packages.
- `--executes-code` — show only packages that execute code.
- `--version <version>` / `--tag <tag>` — inspect a specific package version.
- `--versions`, `--files`, `--file <path>` — inspect package history and files.
- `--json` — machine-readable output.

Install / update / list

```
clawhub install <slug>
clawhub update <slug>
clawhub update --all
clawhub list
```

Options:

- `--version <version>` — install or update to a specific version (single slug only on `update`).
- `--force` — overwrite if the folder already exists, or when local files do not match any published version.
- `clawhub list` reads `.clawhub/lock.json`.

Publish skills

```
clawhub skill publish <path>
```

Options:

- `--slug <slug>` — skill slug.
- `--name <name>` — display name.
- `--version <version>` — semver version.
- `--changelog <text>` — changelog text (can be empty).
- `--tags <tags>` — comma-separated tags (default: `latest`).

Publish plugins

```
clawhub package publish <source>
```

`<source>` can be a local folder, `owner/repo`, `owner/repo@ref`, or a
GitHub URL.Options:

- `--dry-run` — build the exact publish plan without uploading anything.
- `--json` — emit machine-readable output for CI.
- `--source-repo`, `--source-commit`, `--source-ref` — optional overrides when auto-detection is not enough.

Request rescans

```
clawhub skill rescan <slug>
clawhub skill rescan <slug> --yes --json

clawhub package rescan <name>
clawhub package rescan <name> --yes --json
```

Rescan commands require a logged-in owner token and target the latest
published skill version or plugin release. In non-interactive runs, pass
`--yes`.JSON responses include the target kind, name, version, rescan status, and
remaining/max request counts for that version or release.

Delete / undelete (owner or admin)

```
clawhub delete <slug> --yes
clawhub undelete <slug> --yes
```

Sync (scan local + publish new or updated)

```
clawhub sync
```

Options:

- `--root <dir...>` — extra scan roots.
- `--all` — upload everything without prompts.
- `--dry-run` — show what would be uploaded.
- `--bump <type>` — `patch|minor|major` for updates (default: `patch`).
- `--changelog <text>` — changelog for non-interactive updates.
- `--tags <tags>` — comma-separated tags (default: `latest`).
- `--concurrency <n>` — registry checks (default: `4`).

## [​](https://docs.openclaw.ai/tools/clawhub\#common-workflows)  Common workflows

- Search

- Find a plugin

- Install

- Update all

- Publish a single skill

- Sync many skills

- Publish a plugin from GitHub


```
clawhub search "postgres backups"
```

```
clawhub package explore --family code-plugin
clawhub package explore "memory" --family code-plugin
clawhub package inspect episodic-claw
```

```
clawhub install my-skill-pack
```

```
clawhub update --all
```

```
clawhub skill publish ./my-skill --slug my-skill --name "My Skill" --version 1.0.0 --tags latest
```

```
clawhub sync --all
```

```
clawhub package publish your-org/your-plugin --dry-run
clawhub package publish your-org/your-plugin
clawhub package publish your-org/your-plugin@v1.0.0
clawhub package publish https://github.com/your-org/your-plugin
```

### [​](https://docs.openclaw.ai/tools/clawhub\#plugin-package-metadata)  Plugin package metadata

Code plugins must include the required OpenClaw metadata in
`package.json`:

```
{
  "name": "@myorg/openclaw-my-plugin",
  "version": "1.0.0",
  "type": "module",
  "openclaw": {
    "extensions": ["./src/index.ts"],
    "runtimeExtensions": ["./dist/index.js"],
    "compat": {
      "pluginApi": ">=2026.3.24-beta.2",
      "minGatewayVersion": "2026.3.24-beta.2"
    },
    "build": {
      "openclawVersion": "2026.3.24-beta.2",
      "pluginSdkVersion": "2026.3.24-beta.2"
    }
  }
}
```

Published packages should ship **built JavaScript** and point
`runtimeExtensions` at that output. Git checkout installs can still fall
back to TypeScript source when no built files exist, but built runtime
entries avoid runtime TypeScript compilation in startup, doctor, and
plugin loading paths.

## [​](https://docs.openclaw.ai/tools/clawhub\#versioning-lockfile-and-telemetry)  Versioning, lockfile, and telemetry

Versioning and tags

- Each publish creates a new **semver**`SkillVersion`.
- Tags (like `latest`) point to a version; moving tags lets you roll back.
- Changelogs are attached per version and can be empty when syncing or publishing updates.

Local changes vs registry versions

Updates compare the local skill contents to registry versions using a
content hash. If local files do not match any published version, the
CLI asks before overwriting (or requires `--force` in
non-interactive runs).

Sync scanning and fallback roots

`clawhub sync` scans your current workdir first. If no skills are
found, it falls back to known legacy locations (for example
`~/openclaw/skills` and `~/.openclaw/skills`). This is designed to
find older skill installs without extra flags.

Storage and lockfile

- Installed skills are recorded in `.clawhub/lock.json` under your workdir.
- Auth tokens are stored in the ClawHub CLI config file (override via `CLAWHUB_CONFIG_PATH`).

Telemetry (install counts)

When you run `clawhub sync` while logged in, the CLI sends a minimal
snapshot to compute install counts. You can disable this entirely:

```
export CLAWHUB_DISABLE_TELEMETRY=1
```

## [​](https://docs.openclaw.ai/tools/clawhub\#environment-variables)  Environment variables

| Variable | Effect |
| --- | --- |
| `CLAWHUB_SITE` | Override the site URL. |
| `CLAWHUB_REGISTRY` | Override the registry API URL. |
| `CLAWHUB_CONFIG_PATH` | Override where the CLI stores the token/config. |
| `CLAWHUB_WORKDIR` | Override the default workdir. |
| `CLAWHUB_DISABLE_TELEMETRY=1` | Disable telemetry on `sync`. |

## [​](https://docs.openclaw.ai/tools/clawhub\#related)  Related

- [Community plugins](https://docs.openclaw.ai/plugins/community)
- [Plugins](https://docs.openclaw.ai/tools/plugin)
- [Skills](https://docs.openclaw.ai/tools/skills)

[Slash commands](https://docs.openclaw.ai/tools/slash-commands) [OpenProse](https://docs.openclaw.ai/prose)

Ctrl+I