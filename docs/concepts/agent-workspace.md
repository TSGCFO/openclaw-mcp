---
source_url: https://docs.openclaw.ai/concepts/agent-workspace.md
title: ""
---

\> ## Documentation Index
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Agent workspace

The workspace is the agent's home. It is the only working directory used for file tools and for workspace context. Keep it private and treat it as memory.

This is separate from \`~/.openclaw/\`, which stores config, credentials, and sessions.

 The workspace is the \*\*default cwd\*\*, not a hard sandbox. Tools resolve relative paths against the workspace, but absolute paths can still reach elsewhere on the host unless sandboxing is enabled. If you need isolation, use \[\`agents.defaults.sandbox\`\](/gateway/sandboxing) (and/or per-agent sandbox config).

 When sandboxing is enabled and \`workspaceAccess\` is not \`"rw"\`, tools operate inside a sandbox workspace under \`~/.openclaw/sandboxes\`, not your host workspace.

\## Default location

\\* Default: \`~/.openclaw/workspace\`
\\* If \`OPENCLAW\_PROFILE\` is set and not \`"default"\`, the default becomes \`~/.openclaw/workspace-\`.
\\* Override in \`~/.openclaw/openclaw.json\`:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{
 agents: {
 defaults: {
 workspace: "~/.openclaw/workspace",
 },
 },
}
\`\`\`

\`openclaw onboard\`, \`openclaw configure\`, or \`openclaw setup\` will create the workspace and seed the bootstrap files if they are missing.

 Sandbox seed copies only accept regular in-workspace files; symlink/hardlink aliases that resolve outside the source workspace are ignored.

If you already manage the workspace files yourself, you can disable bootstrap file creation:

\`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
{ agents: { defaults: { skipBootstrap: true } } }
\`\`\`

\## Extra workspace folders

Older installs may have created \`~/openclaw\`. Keeping multiple workspace directories around can cause confusing auth or state drift, because only one workspace is active at a time.

 \*\*Recommendation:\*\* keep a single active workspace. If you no longer use the extra folders, archive or move them to Trash (for example \`trash ~/openclaw\`). If you intentionally keep multiple workspaces, make sure \`agents.defaults.workspace\` points to the active one.

 \`openclaw doctor\` warns when it detects extra workspace directories.

\## Workspace file map

These are the standard files OpenClaw expects inside the workspace:

 Operating instructions for the agent and how it should use memory. Loaded at the start of every session. Good place for rules, priorities, and "how to behave" details.

 Persona, tone, and boundaries. Loaded every session. Guide: \[SOUL.md personality guide\](/concepts/soul).

 Who the user is and how to address them. Loaded every session.

 The agent's name, vibe, and emoji. Created/updated during the bootstrap ritual.

 Notes about your local tools and conventions. Does not control tool availability; it is only guidance.

 Optional tiny checklist for heartbeat runs. Keep it short to avoid token burn.

 Optional startup checklist run automatically on gateway restart (when \[internal hooks\](/automation/hooks) are enabled). Keep it short; use the message tool for outbound sends.

 One-time first-run ritual. Only created for a brand-new workspace. Delete it after the ritual is complete.

 Daily memory log (one file per day). Recommended to read today + yesterday on session start.

 Curated long-term memory. Only load in the main, private session (not shared/group contexts). See \[Memory\](/concepts/memory) for the workflow and automatic memory flush.

 Workspace-specific skills. Highest-precedence skill location for that workspace. Overrides project agent skills, personal agent skills, managed skills, bundled skills, and \`skills.load.extraDirs\` when names collide.

 Canvas UI files for node displays (for example \`canvas/index.html\`).

 If any bootstrap file is missing, OpenClaw injects a "missing file" marker into the session and continues. Large bootstrap files are truncated when injected; adjust limits with \`agents.defaults.bootstrapMaxChars\` (default: 12000) and \`agents.defaults.bootstrapTotalMaxChars\` (default: 60000). \`openclaw setup\` can recreate missing defaults without overwriting existing files.

\## What is NOT in the workspace

These live under \`~/.openclaw/\` and should NOT be committed to the workspace repo:

\\* \`~/.openclaw/openclaw.json\` (config)
\\* \`~/.openclaw/agents//agent/auth-profiles.json\` (model auth profiles: OAuth + API keys)
\\* \`~/.openclaw/agents//agent/codex-home/\` (per-agent Codex runtime account, config, skills, plugins, and native thread state)
\\* \`~/.openclaw/credentials/\` (channel/provider state plus legacy OAuth import data)
\\* \`~/.openclaw/agents//sessions/\` (session transcripts + metadata)
\\* \`~/.openclaw/skills/\` (managed skills)

If you need to migrate sessions or config, copy them separately and keep them out of version control.

\## Git backup (recommended, private)

Treat the workspace as private memory. Put it in a \*\*private\*\* git repo so it is backed up and recoverable.

Run these steps on the machine where the Gateway runs (that is where the workspace lives).

 If git is installed, brand-new workspaces are initialized automatically. If this workspace is not already a repo, run:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 cd ~/.openclaw/workspace
 git init
 git add AGENTS.md SOUL.md TOOLS.md IDENTITY.md USER.md HEARTBEAT.md memory/
 git commit -m "Add agent workspace"
 \`\`\`

 1\. Create a new \*\*private\*\* repository on GitHub.
 2\. Do not initialize with a README (avoids merge conflicts).
 3\. Copy the HTTPS remote URL.
 4\. Add the remote and push:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 git branch -M main
 git remote add origin
 git push -u origin main
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 gh auth login
 gh repo create openclaw-workspace --private --source . --remote origin --push
 \`\`\`

 1\. Create a new \*\*private\*\* repository on GitLab.
 2\. Do not initialize with a README (avoids merge conflicts).
 3\. Copy the HTTPS remote URL.
 4\. Add the remote and push:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 git branch -M main
 git remote add origin
 git push -u origin main
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 git status
 git add .
 git commit -m "Update memory"
 git push
 \`\`\`


\## Do not commit secrets

 Even in a private repo, avoid storing secrets in the workspace:

 \\* API keys, OAuth tokens, passwords, or private credentials.
 \\* Anything under \`~/.openclaw/\`.
 \\* Raw dumps of chats or sensitive attachments.

 If you must store sensitive references, use placeholders and keep the real secret elsewhere (password manager, environment variables, or \`~/.openclaw/\`).

Suggested \`.gitignore\` starter:

\`\`\`gitignore theme={"theme":{"light":"min-light","dark":"min-dark"}}
.DS\_Store
.env
\*\*/\*.key
\*\*/\*.pem
\*\*/secrets\*
\`\`\`

\## Moving the workspace to a new machine

 Clone the repo to the desired path (default \`~/.openclaw/workspace\`).

 Set \`agents.defaults.workspace\` to that path in \`~/.openclaw/openclaw.json\`.

 Run \`openclaw setup --workspace \` to seed any missing files.

 If you need sessions, copy \`~/.openclaw/agents//sessions/\` from the old machine separately.


\## Advanced notes

\\* Multi-agent routing can use different workspaces per agent. See \[Channel routing\](/channels/channel-routing) for routing configuration.
\\* If \`agents.defaults.sandbox\` is enabled, non-main sessions can use per-session sandbox workspaces under \`agents.defaults.sandbox.workspaceRoot\`.

\## Related

\\* \[Heartbeat\](/gateway/heartbeat) — HEARTBEAT.md workspace file
\\* \[Sandboxing\](/gateway/sandboxing) — workspace access in sandboxed environments
\\* \[Session\](/concepts/session) — session storage paths
\\* \[Standing orders\](/automation/standing-orders) — persistent instructions in workspace files