---
source_url: https://docs.openclaw.ai/concepts/agent-workspace
title: "Agent workspace - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/agent-workspace#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Fundamentals

Agent workspace

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Default location](https://docs.openclaw.ai/concepts/agent-workspace#default-location)
- [Extra workspace folders](https://docs.openclaw.ai/concepts/agent-workspace#extra-workspace-folders)
- [Workspace file map](https://docs.openclaw.ai/concepts/agent-workspace#workspace-file-map)
- [What is NOT in the workspace](https://docs.openclaw.ai/concepts/agent-workspace#what-is-not-in-the-workspace)
- [Git backup (recommended, private)](https://docs.openclaw.ai/concepts/agent-workspace#git-backup-recommended-private)
- [Do not commit secrets](https://docs.openclaw.ai/concepts/agent-workspace#do-not-commit-secrets)
- [Moving the workspace to a new machine](https://docs.openclaw.ai/concepts/agent-workspace#moving-the-workspace-to-a-new-machine)
- [Advanced notes](https://docs.openclaw.ai/concepts/agent-workspace#advanced-notes)
- [Related](https://docs.openclaw.ai/concepts/agent-workspace#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The workspace is the agent’s home. It is the only working directory used for file tools and for workspace context. Keep it private and treat it as memory.This is separate from `~/.openclaw/`, which stores config, credentials, and sessions.

The workspace is the **default cwd**, not a hard sandbox. Tools resolve relative paths against the workspace, but absolute paths can still reach elsewhere on the host unless sandboxing is enabled. If you need isolation, use [`agents.defaults.sandbox`](https://docs.openclaw.ai/gateway/sandboxing) (and/or per-agent sandbox config).When sandboxing is enabled and `workspaceAccess` is not `"rw"`, tools operate inside a sandbox workspace under `~/.openclaw/sandboxes`, not your host workspace.

## [​](https://docs.openclaw.ai/concepts/agent-workspace\#default-location)  Default location

- Default: `~/.openclaw/workspace`
- If `OPENCLAW_PROFILE` is set and not `"default"`, the default becomes `~/.openclaw/workspace-<profile>`.
- Override in `~/.openclaw/openclaw.json`:

```
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
    },
  },
}
```

`openclaw onboard`, `openclaw configure`, or `openclaw setup` will create the workspace and seed the bootstrap files if they are missing.

Sandbox seed copies only accept regular in-workspace files; symlink/hardlink aliases that resolve outside the source workspace are ignored.

If you already manage the workspace files yourself, you can disable bootstrap file creation:

```
{ agents: { defaults: { skipBootstrap: true } } }
```

## [​](https://docs.openclaw.ai/concepts/agent-workspace\#extra-workspace-folders)  Extra workspace folders

Older installs may have created `~/openclaw`. Keeping multiple workspace directories around can cause confusing auth or state drift, because only one workspace is active at a time.

**Recommendation:** keep a single active workspace. If you no longer use the extra folders, archive or move them to Trash (for example `trash ~/openclaw`). If you intentionally keep multiple workspaces, make sure `agents.defaults.workspace` points to the active one.`openclaw doctor` warns when it detects extra workspace directories.

## [​](https://docs.openclaw.ai/concepts/agent-workspace\#workspace-file-map)  Workspace file map

These are the standard files OpenClaw expects inside the workspace:

AGENTS.md — operating instructions

Operating instructions for the agent and how it should use memory. Loaded at the start of every session. Good place for rules, priorities, and “how to behave” details.

SOUL.md — persona and tone

Persona, tone, and boundaries. Loaded every session. Guide: [SOUL.md personality guide](https://docs.openclaw.ai/concepts/soul).

USER.md — who the user is

Who the user is and how to address them. Loaded every session.

IDENTITY.md — name, vibe, emoji

The agent’s name, vibe, and emoji. Created/updated during the bootstrap ritual.

TOOLS.md — local tool conventions

Notes about your local tools and conventions. Does not control tool availability; it is only guidance.

HEARTBEAT.md — heartbeat checklist

Optional tiny checklist for heartbeat runs. Keep it short to avoid token burn.

BOOT.md — startup checklist

Optional startup checklist run automatically on gateway restart (when [internal hooks](https://docs.openclaw.ai/automation/hooks) are enabled). Keep it short; use the message tool for outbound sends.

BOOTSTRAP.md — first-run ritual

One-time first-run ritual. Only created for a brand-new workspace. Delete it after the ritual is complete.

memory/YYYY-MM-DD.md — daily memory log

Daily memory log (one file per day). Recommended to read today + yesterday on session start.

MEMORY.md — curated long-term memory (optional)

Curated long-term memory. Only load in the main, private session (not shared/group contexts). See [Memory](https://docs.openclaw.ai/concepts/memory) for the workflow and automatic memory flush.

skills/ — workspace skills (optional)

Workspace-specific skills. Highest-precedence skill location for that workspace. Overrides project agent skills, personal agent skills, managed skills, bundled skills, and `skills.load.extraDirs` when names collide.

canvas/ — Canvas UI files (optional)

Canvas UI files for node displays (for example `canvas/index.html`).

If any bootstrap file is missing, OpenClaw injects a “missing file” marker into the session and continues. Large bootstrap files are truncated when injected; adjust limits with `agents.defaults.bootstrapMaxChars` (default: 12000) and `agents.defaults.bootstrapTotalMaxChars` (default: 60000). `openclaw setup` can recreate missing defaults without overwriting existing files.

## [​](https://docs.openclaw.ai/concepts/agent-workspace\#what-is-not-in-the-workspace)  What is NOT in the workspace

These live under `~/.openclaw/` and should NOT be committed to the workspace repo:

- `~/.openclaw/openclaw.json` (config)
- `~/.openclaw/agents/<agentId>/agent/auth-profiles.json` (model auth profiles: OAuth + API keys)
- `~/.openclaw/agents/<agentId>/agent/codex-home/` (per-agent Codex runtime account, config, skills, plugins, and native thread state)
- `~/.openclaw/credentials/` (channel/provider state plus legacy OAuth import data)
- `~/.openclaw/agents/<agentId>/sessions/` (session transcripts + metadata)
- `~/.openclaw/skills/` (managed skills)

If you need to migrate sessions or config, copy them separately and keep them out of version control.

## [​](https://docs.openclaw.ai/concepts/agent-workspace\#git-backup-recommended-private)  Git backup (recommended, private)

Treat the workspace as private memory. Put it in a **private** git repo so it is backed up and recoverable.Run these steps on the machine where the Gateway runs (that is where the workspace lives).

1

[Navigate to header](https://docs.openclaw.ai/concepts/agent-workspace#)

Initialize the repo

If git is installed, brand-new workspaces are initialized automatically. If this workspace is not already a repo, run:

```
cd ~/.openclaw/workspace
git init
git add AGENTS.md SOUL.md TOOLS.md IDENTITY.md USER.md HEARTBEAT.md memory/
git commit -m "Add agent workspace"
```

2

[Navigate to header](https://docs.openclaw.ai/concepts/agent-workspace#)

Add a private remote

- GitHub web UI

- GitHub CLI (gh)

- GitLab web UI


1. Create a new **private** repository on GitHub.
2. Do not initialize with a README (avoids merge conflicts).
3. Copy the HTTPS remote URL.
4. Add the remote and push:

```
git branch -M main
git remote add origin <https-url>
git push -u origin main
```

```
gh auth login
gh repo create openclaw-workspace --private --source . --remote origin --push
```

1. Create a new **private** repository on GitLab.
2. Do not initialize with a README (avoids merge conflicts).
3. Copy the HTTPS remote URL.
4. Add the remote and push:

```
git branch -M main
git remote add origin <https-url>
git push -u origin main
```

3

[Navigate to header](https://docs.openclaw.ai/concepts/agent-workspace#)

Ongoing updates

```
git status
git add .
git commit -m "Update memory"
git push
```

## [​](https://docs.openclaw.ai/concepts/agent-workspace\#do-not-commit-secrets)  Do not commit secrets

Even in a private repo, avoid storing secrets in the workspace:

- API keys, OAuth tokens, passwords, or private credentials.
- Anything under `~/.openclaw/`.
- Raw dumps of chats or sensitive attachments.

If you must store sensitive references, use placeholders and keep the real secret elsewhere (password manager, environment variables, or `~/.openclaw/`).

Suggested `.gitignore` starter:

```
.DS_Store
.env
**/*.key
**/*.pem
**/secrets*
```

## [​](https://docs.openclaw.ai/concepts/agent-workspace\#moving-the-workspace-to-a-new-machine)  Moving the workspace to a new machine

1

[Navigate to header](https://docs.openclaw.ai/concepts/agent-workspace#)

Clone the repo

Clone the repo to the desired path (default `~/.openclaw/workspace`).

2

[Navigate to header](https://docs.openclaw.ai/concepts/agent-workspace#)

Update config

Set `agents.defaults.workspace` to that path in `~/.openclaw/openclaw.json`.

3

[Navigate to header](https://docs.openclaw.ai/concepts/agent-workspace#)

Seed missing files

Run `openclaw setup --workspace <path>` to seed any missing files.

4

[Navigate to header](https://docs.openclaw.ai/concepts/agent-workspace#)

Copy sessions (optional)

If you need sessions, copy `~/.openclaw/agents/<agentId>/sessions/` from the old machine separately.

## [​](https://docs.openclaw.ai/concepts/agent-workspace\#advanced-notes)  Advanced notes

- Multi-agent routing can use different workspaces per agent. See [Channel routing](https://docs.openclaw.ai/channels/channel-routing) for routing configuration.
- If `agents.defaults.sandbox` is enabled, non-main sessions can use per-session sandbox workspaces under `agents.defaults.sandbox.workspaceRoot`.

## [​](https://docs.openclaw.ai/concepts/agent-workspace\#related)  Related

- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) — HEARTBEAT.md workspace file
- [Sandboxing](https://docs.openclaw.ai/gateway/sandboxing) — workspace access in sandboxed environments
- [Session](https://docs.openclaw.ai/concepts/session) — session storage paths
- [Standing orders](https://docs.openclaw.ai/automation/standing-orders) — persistent instructions in workspace files

[Context engine](https://docs.openclaw.ai/concepts/context-engine) [SOUL.md personality guide](https://docs.openclaw.ai/concepts/soul)

Ctrl+I