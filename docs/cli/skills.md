---
source_url: https://docs.openclaw.ai/cli/skills.md
title: ""
---

\> ## Documentation Index
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Skills

\# \`openclaw skills\`

Inspect local skills and install/update skills from ClawHub.

Related:

\\* Skills system: \[Skills\](/tools/skills)
\\* Skills config: \[Skills config\](/tools/skills-config)
\\* ClawHub installs: \[ClawHub\](/tools/clawhub)

\## Commands

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw skills search "calendar"
openclaw skills search --limit 20 --json
openclaw skills install
openclaw skills install  --version
openclaw skills install  --force
openclaw skills install  --agent
openclaw skills update
openclaw skills update --all
openclaw skills update --all --agent
openclaw skills list
openclaw skills list --eligible
openclaw skills list --json
openclaw skills list --verbose
openclaw skills list --agent
openclaw skills info
openclaw skills info  --json
openclaw skills info  --agent
openclaw skills check
openclaw skills check --json
openclaw skills check --agent
\`\`\`

\`search\`/\`install\`/\`update\` use ClawHub directly and install into the active
workspace \`skills/\` directory. \`list\`/\`info\`/\`check\` still inspect the local
skills visible to the current workspace and config. Workspace-backed commands
resolve the target workspace from \`--agent \`, then the current working
directory when it is inside a configured agent workspace, then the default
agent.

This CLI \`install\` command downloads skill folders from ClawHub. Gateway-backed
skill dependency installs triggered from onboarding or Skills settings use the
separate \`skills.install\` request path instead.

Notes:

\\* \`search \[query...\]\` accepts an optional query; omit it to browse the default
 ClawHub search feed.
\\* \`search --limit \` caps returned results.
\\* \`install --force\` overwrites an existing workspace skill folder for the same
 slug.
\\* \`--agent \` targets one configured agent workspace and overrides current
 working directory inference.
\\* \`update --all\` only updates tracked ClawHub installs in the active workspace.
\\* \`list\` is the default action when no subcommand is provided.
\\* \`list\`, \`info\`, and \`check\` write their rendered output to stdout. With
 \`--json\`, that means the machine-readable payload stays on stdout for pipes
 and scripts.

\## Related

\\* \[CLI reference\](/cli)
\\* \[Skills\](/tools/skills)