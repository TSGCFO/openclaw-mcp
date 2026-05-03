---
source_url: https://docs.openclaw.ai/start/getting-started.md
title: ""
---

\> ## Documentation Index
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Getting started

Install OpenClaw, run onboarding, and chat with your AI assistant — all in
about 5 minutes. By the end you will have a running Gateway, configured auth,
and a working chat session.

\## What you need

\\* \*\*Node.js\*\* — Node 24 recommended (Node 22.14+ also supported)
\\* \*\*An API key\*\* from a model provider (Anthropic, OpenAI, Google, etc.) — onboarding will prompt you

 Check your Node version with \`node --version\`.
 \*\*Windows users:\*\* both native Windows and WSL2 are supported. WSL2 is more
 stable and recommended for the full experience. See \[Windows\](/platforms/windows).
 Need to install Node? See \[Node setup\](/install/node).

\## Quick setup

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 curl -fsSL https://openclaw.ai/install.sh \| bash
 \`\`\`

 ![Install Script Process](https://mintcdn.com/clawdhub/U8jr7qEbUc9OU9YR/assets/install-script.svg?fit=max&auto=format&n=U8jr7qEbUc9OU9YR&q=85&s=50706f81e3210a610262f14facb11f65)
 \`\`\`powershell theme={"theme":{"light":"min-light","dark":"min-dark"}}
 iwr -useb https://openclaw.ai/install.ps1 \| iex
 \`\`\`

 Other install methods (Docker, Nix, npm): \[Install\](/install).

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --install-daemon
 \`\`\`

 The wizard walks you through choosing a model provider, setting an API key,
 and configuring the Gateway. It takes about 2 minutes.

 See \[Onboarding (CLI)\](/start/wizard) for the full reference.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway status
 \`\`\`

 You should see the Gateway listening on port 18789.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw dashboard
 \`\`\`

 This opens the Control UI in your browser. If it loads, everything is working.

 Type a message in the Control UI chat and you should get an AI reply.

 Want to chat from your phone instead? The fastest channel to set up is
 \[Telegram\](/channels/telegram) (just a bot token). See \[Channels\](/channels)
 for all options.

 If you maintain a localized or customized dashboard build, point
 \`gateway.controlUi.root\` to a directory that contains your built static
 assets and \`index.html\`.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 mkdir -p "$HOME/.openclaw/control-ui-custom"
 # Copy your built static files into that directory.
 \`\`\`

 Then set:

 \`\`\`json theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 "gateway": {
 "controlUi": {
 "enabled": true,
 "root": "$HOME/.openclaw/control-ui-custom"
 }
 }
 }
 \`\`\`

 Restart the gateway and reopen the dashboard:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway restart
 openclaw dashboard
 \`\`\`

\## What to do next

 Discord, Feishu, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, and more.

 Control who can message your agent.

 Models, tools, sandbox, and advanced settings.

 Browser, exec, web search, skills, and plugins.

 If you run OpenClaw as a service account or want custom paths:

 \\* \`OPENCLAW\_HOME\` — home directory for internal path resolution
 \\* \`OPENCLAW\_STATE\_DIR\` — override the state directory
 \\* \`OPENCLAW\_CONFIG\_PATH\` — override the config file path

 Full reference: \[Environment variables\](/help/environment).

\## Related

\\* \[Install overview\](/install)
\\* \[Channels overview\](/channels)
\\* \[Setup\](/start/setup)