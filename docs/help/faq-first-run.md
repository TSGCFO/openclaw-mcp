---
source_url: https://docs.openclaw.ai/help/faq-first-run
title: "FAQ: first-run setup - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/help/faq-first-run#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

FAQ

FAQ: first-run setup

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start and first-run setup](https://docs.openclaw.ai/help/faq-first-run#quick-start-and-first-run-setup)
- [Related](https://docs.openclaw.ai/help/faq-first-run#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Quick-start and first-run Q&A. For everyday operations, models, auth, sessions,
and troubleshooting see the main [FAQ](https://docs.openclaw.ai/help/faq).

## [​](https://docs.openclaw.ai/help/faq-first-run\#quick-start-and-first-run-setup)  Quick start and first-run setup

I am stuck, fastest way to get unstuck

Use a local AI agent that can **see your machine**. That is far more effective than asking
in Discord, because most “I’m stuck” cases are **local config or environment issues** that
remote helpers cannot inspect.

- **Claude Code**: [https://www.anthropic.com/claude-code/](https://www.anthropic.com/claude-code/)
- **OpenAI Codex**: [https://openai.com/codex/](https://openai.com/codex/)

These tools can read the repo, run commands, inspect logs, and help fix your machine-level
setup (PATH, services, permissions, auth files). Give them the **full source checkout** via
the hackable (git) install:

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git
```

This installs OpenClaw **from a git checkout**, so the agent can read the code + docs and
reason about the exact version you are running. You can always switch back to stable later
by re-running the installer without `--install-method git`.Tip: ask the agent to **plan and supervise** the fix (step-by-step), then execute only the
necessary commands. That keeps changes small and easier to audit.If you discover a real bug or fix, please file a GitHub issue or send a PR:
[https://github.com/openclaw/openclaw/issues](https://github.com/openclaw/openclaw/issues) [https://github.com/openclaw/openclaw/pulls](https://github.com/openclaw/openclaw/pulls)Start with these commands (share outputs when asking for help):

```
openclaw status
openclaw models status
openclaw doctor
```

What they do:

- `openclaw status`: quick snapshot of gateway/agent health + basic config.
- `openclaw models status`: checks provider auth + model availability.
- `openclaw doctor`: validates and repairs common config/state issues.

Other useful CLI checks: `openclaw status --all`, `openclaw logs --follow`,
`openclaw gateway status`, `openclaw health --verbose`.Quick debug loop: [First 60 seconds if something is broken](https://docs.openclaw.ai/help/faq#first-60-seconds-if-something-is-broken).
Install docs: [Install](https://docs.openclaw.ai/install), [Installer flags](https://docs.openclaw.ai/install/installer), [Updating](https://docs.openclaw.ai/install/updating).

Heartbeat keeps skipping. What do the skip reasons mean?

Common heartbeat skip reasons:

- `quiet-hours`: outside the configured active-hours window
- `empty-heartbeat-file`: `HEARTBEAT.md` exists but only contains blank/header-only scaffolding
- `no-tasks-due`: `HEARTBEAT.md` task mode is active but none of the task intervals are due yet
- `alerts-disabled`: all heartbeat visibility is disabled (`showOk`, `showAlerts`, and `useIndicator` are all off)

In task mode, due timestamps are only advanced after a real heartbeat run
completes. Skipped runs do not mark tasks as completed.Docs: [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat), [Automation & Tasks](https://docs.openclaw.ai/automation).

Recommended way to install and set up OpenClaw

The repo recommends running from source and using onboarding:

```
curl -fsSL https://openclaw.ai/install.sh | bash
openclaw onboard --install-daemon
```

The wizard can also build UI assets automatically. After onboarding, you typically run the Gateway on port **18789**.From source (contributors/dev):

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm build
pnpm ui:build
openclaw onboard
```

If you don’t have a global install yet, run it via `pnpm openclaw onboard`.

How do I open the dashboard after onboarding?

The wizard opens your browser with a clean (non-tokenized) dashboard URL right after onboarding and also prints the link in the summary. Keep that tab open; if it didn’t launch, copy/paste the printed URL on the same machine.

How do I authenticate the dashboard on localhost vs remote?

**Localhost (same machine):**

- Open `http://127.0.0.1:18789/`.
- If it asks for shared-secret auth, paste the configured token or password into Control UI settings.
- Token source: `gateway.auth.token` (or `OPENCLAW_GATEWAY_TOKEN`).
- Password source: `gateway.auth.password` (or `OPENCLAW_GATEWAY_PASSWORD`).
- If no shared secret is configured yet, generate a token with `openclaw doctor --generate-gateway-token`.

**Not on localhost:**

- **Tailscale Serve** (recommended): keep bind loopback, run `openclaw gateway --tailscale serve`, open `https://<magicdns>/`. If `gateway.auth.allowTailscale` is `true`, identity headers satisfy Control UI/WebSocket auth (no pasted shared secret, assumes trusted gateway host); HTTP APIs still require shared-secret auth unless you deliberately use private-ingress `none` or trusted-proxy HTTP auth.
Bad concurrent Serve auth attempts from the same client are serialized before the failed-auth limiter records them, so the second bad retry can already show `retry later`.
- **Tailnet bind**: run `openclaw gateway --bind tailnet --token "<token>"` (or configure password auth), open `http://<tailscale-ip>:18789/`, then paste the matching shared secret in dashboard settings.
- **Identity-aware reverse proxy**: keep the Gateway behind a trusted proxy, configure `gateway.auth.mode: "trusted-proxy"`, then open the proxy URL. Same-host loopback proxies require explicit `gateway.auth.trustedProxy.allowLoopback = true`.
- **SSH tunnel**: `ssh -N -L 18789:127.0.0.1:18789 user@host` then open `http://127.0.0.1:18789/`. Shared-secret auth still applies over the tunnel; paste the configured token or password if prompted.

See [Dashboard](https://docs.openclaw.ai/web/dashboard) and [Web surfaces](https://docs.openclaw.ai/web) for bind modes and auth details.

Why are there two exec approval configs for chat approvals?

They control different layers:

- `approvals.exec`: forwards approval prompts to chat destinations
- `channels.<channel>.execApprovals`: makes that channel act as a native approval client for exec approvals

The host exec policy is still the real approval gate. Chat config only controls where approval
prompts appear and how people can answer them.In most setups you do **not** need both:

- If the chat already supports commands and replies, same-chat `/approve` works through the shared path.
- If a supported native channel can infer approvers safely, OpenClaw now auto-enables DM-first native approvals when `channels.<channel>.execApprovals.enabled` is unset or `"auto"`.
- When native approval cards/buttons are available, that native UI is the primary path; the agent should only include a manual `/approve` command if the tool result says chat approvals are unavailable or manual approval is the only path.
- Use `approvals.exec` only when prompts must also be forwarded to other chats or explicit ops rooms.
- Use `channels.<channel>.execApprovals.target: "channel"` or `"both"` only when you explicitly want approval prompts posted back into the originating room/topic.
- Plugin approvals are separate again: they use same-chat `/approve` by default, optional `approvals.plugin` forwarding, and only some native channels keep plugin-approval-native handling on top.

Short version: forwarding is for routing, native client config is for richer channel-specific UX.
See [Exec Approvals](https://docs.openclaw.ai/tools/exec-approvals).

What runtime do I need?

Node **>= 22** is required. `pnpm` is recommended. Bun is **not recommended** for the Gateway.

Does it run on Raspberry Pi?

Yes. The Gateway is lightweight - docs list **512MB-1GB RAM**, **1 core**, and about **500MB**
disk as enough for personal use, and note that a **Raspberry Pi 4 can run it**.If you want extra headroom (logs, media, other services), **2GB is recommended**, but it’s
not a hard minimum.Tip: a small Pi/VPS can host the Gateway, and you can pair **nodes** on your laptop/phone for
local screen/camera/canvas or command execution. See [Nodes](https://docs.openclaw.ai/nodes).

Any tips for Raspberry Pi installs?

Short version: it works, but expect rough edges.

- Use a **64-bit** OS and keep Node >= 22.
- Prefer the **hackable (git) install** so you can see logs and update fast.
- Start without channels/skills, then add them one by one.
- If you hit weird binary issues, it is usually an **ARM compatibility** problem.

Docs: [Linux](https://docs.openclaw.ai/platforms/linux), [Install](https://docs.openclaw.ai/install).

It is stuck on wake up my friend / onboarding will not hatch. What now?

That screen depends on the Gateway being reachable and authenticated. The TUI also sends
“Wake up, my friend!” automatically on first hatch. If you see that line with **no reply**
and tokens stay at 0, the agent never ran.

1. Restart the Gateway:

```
openclaw gateway restart
```

2. Check status + auth:

```
openclaw status
openclaw models status
openclaw logs --follow
```

3. If it still hangs, run:

```
openclaw doctor
```

If the Gateway is remote, ensure the tunnel/Tailscale connection is up and that the UI
is pointed at the right Gateway. See [Remote access](https://docs.openclaw.ai/gateway/remote).

Can I migrate my setup to a new machine (Mac mini) without redoing onboarding?

Yes. Copy the **state directory** and **workspace**, then run Doctor once. This
keeps your bot “exactly the same” (memory, session history, auth, and channel
state) as long as you copy **both** locations:

1. Install OpenClaw on the new machine.
2. Copy `$OPENCLAW_STATE_DIR` (default: `~/.openclaw`) from the old machine.
3. Copy your workspace (default: `~/.openclaw/workspace`).
4. Run `openclaw doctor` and restart the Gateway service.

That preserves config, auth profiles, WhatsApp creds, sessions, and memory. If you’re in
remote mode, remember the gateway host owns the session store and workspace.**Important:** if you only commit/push your workspace to GitHub, you’re backing
up **memory + bootstrap files**, but **not** session history or auth. Those live
under `~/.openclaw/` (for example `~/.openclaw/agents/<agentId>/sessions/`).Related: [Migrating](https://docs.openclaw.ai/install/migrating), [Where things live on disk](https://docs.openclaw.ai/help/faq#where-things-live-on-disk),
[Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace), [Doctor](https://docs.openclaw.ai/gateway/doctor),
[Remote mode](https://docs.openclaw.ai/gateway/remote).

Where do I see what is new in the latest version?

Check the GitHub changelog:
[https://github.com/openclaw/openclaw/blob/main/CHANGELOG.md](https://github.com/openclaw/openclaw/blob/main/CHANGELOG.md)Newest entries are at the top. If the top section is marked **Unreleased**, the next dated
section is the latest shipped version. Entries are grouped by **Highlights**, **Changes**, and
**Fixes** (plus docs/other sections when needed).

Cannot access docs.openclaw.ai (SSL error)

Some Comcast/Xfinity connections incorrectly block `docs.openclaw.ai` via Xfinity
Advanced Security. Disable it or allowlist `docs.openclaw.ai`, then retry.
Please help us unblock it by reporting here: [https://spa.xfinity.com/check\_url\_status](https://spa.xfinity.com/check_url_status).If you still can’t reach the site, the docs are mirrored on GitHub:
[https://github.com/openclaw/openclaw/tree/main/docs](https://github.com/openclaw/openclaw/tree/main/docs)

Difference between stable and beta

**Stable** and **beta** are **npm dist-tags**, not separate code lines:

- `latest` = stable
- `beta` = early build for testing

Usually, a stable release lands on **beta** first, then an explicit
promotion step moves that same version to `latest`. Maintainers can also
publish straight to `latest` when needed. That’s why beta and stable can
point at the **same version** after promotion.See what changed:
[https://github.com/openclaw/openclaw/blob/main/CHANGELOG.md](https://github.com/openclaw/openclaw/blob/main/CHANGELOG.md)For install one-liners and the difference between beta and dev, see the accordion below.

How do I install the beta version and what is the difference between beta and dev?

**Beta** is the npm dist-tag `beta` (may match `latest` after promotion).
**Dev** is the moving head of `main` (git); when published, it uses the npm dist-tag `dev`.One-liners (macOS/Linux):

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --beta
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --install-method git
```

Windows installer (PowerShell):
[https://openclaw.ai/install.ps1](https://openclaw.ai/install.ps1)More detail: [Development channels](https://docs.openclaw.ai/install/development-channels) and [Installer flags](https://docs.openclaw.ai/install/installer).

How do I try the latest bits?

Two options:

1. **Dev channel (git checkout):**

```
openclaw update --channel dev
```

This switches to the `main` branch and updates from source.

2. **Hackable install (from the installer site):**

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git
```

That gives you a local repo you can edit, then update via git.If you prefer a clean clone manually, use:

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm build
```

Docs: [Update](https://docs.openclaw.ai/cli/update), [Development channels](https://docs.openclaw.ai/install/development-channels),
[Install](https://docs.openclaw.ai/install).

How long does install and onboarding usually take?

Rough guide:

- **Install:** 2-5 minutes
- **Onboarding:** 5-15 minutes depending on how many channels/models you configure

If it hangs, use [Installer stuck](https://docs.openclaw.ai/help/faq-first-run#quick-start-and-first-run-setup)
and the fast debug loop in [I am stuck](https://docs.openclaw.ai/help/faq-first-run#quick-start-and-first-run-setup).

Installer stuck? How do I get more feedback?

Re-run the installer with **verbose output**:

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --verbose
```

Beta install with verbose:

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --beta --verbose
```

For a hackable (git) install:

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git --verbose
```

Windows (PowerShell) equivalent:

```
# install.ps1 has no dedicated -Verbose flag yet.
Set-PSDebug -Trace 1
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
Set-PSDebug -Trace 0
```

More options: [Installer flags](https://docs.openclaw.ai/install/installer).

Windows install says git not found or openclaw not recognized

Two common Windows issues:**1) npm error spawn git / git not found**

- Install **Git for Windows** and make sure `git` is on your PATH.
- Close and reopen PowerShell, then re-run the installer.

**2) openclaw is not recognized after install**

- Your npm global bin folder is not on PATH.
- Check the path:














```
npm config get prefix
```

- Add that directory to your user PATH (no `\bin` suffix needed on Windows; on most systems it is `%AppData%\npm`).
- Close and reopen PowerShell after updating PATH.

If you want the smoothest Windows setup, use **WSL2** instead of native Windows.
Docs: [Windows](https://docs.openclaw.ai/platforms/windows).

Windows exec output shows garbled Chinese text - what should I do?

This is usually a console code page mismatch on native Windows shells.Symptoms:

- `system.run`/`exec` output renders Chinese as mojibake
- The same command looks fine in another terminal profile

Quick workaround in PowerShell:

```
chcp 65001
[Console]::InputEncoding = [System.Text.UTF8Encoding]::new($false)
[Console]::OutputEncoding = [System.Text.UTF8Encoding]::new($false)
$OutputEncoding = [System.Text.UTF8Encoding]::new($false)
```

Then restart the Gateway and retry your command:

```
openclaw gateway restart
```

If you still reproduce this on latest OpenClaw, track/report it in:

- [Issue #30640](https://github.com/openclaw/openclaw/issues/30640)

The docs did not answer my question - how do I get a better answer?

Use the **hackable (git) install** so you have the full source and docs locally, then ask
your bot (or Claude/Codex) _from that folder_ so it can read the repo and answer precisely.

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git
```

More detail: [Install](https://docs.openclaw.ai/install) and [Installer flags](https://docs.openclaw.ai/install/installer).

How do I install OpenClaw on Linux?

Short answer: follow the Linux guide, then run onboarding.

- Linux quick path + service install: [Linux](https://docs.openclaw.ai/platforms/linux).
- Full walkthrough: [Getting Started](https://docs.openclaw.ai/start/getting-started).
- Installer + updates: [Install & updates](https://docs.openclaw.ai/install/updating).

How do I install OpenClaw on a VPS?

Any Linux VPS works. Install on the server, then use SSH/Tailscale to reach the Gateway.Guides: [exe.dev](https://docs.openclaw.ai/install/exe-dev), [Hetzner](https://docs.openclaw.ai/install/hetzner), [Fly.io](https://docs.openclaw.ai/install/fly).
Remote access: [Gateway remote](https://docs.openclaw.ai/gateway/remote).

Where are the cloud/VPS install guides?

We keep a **hosting hub** with the common providers. Pick one and follow the guide:

- [VPS hosting](https://docs.openclaw.ai/vps) (all providers in one place)
- [Fly.io](https://docs.openclaw.ai/install/fly)
- [Hetzner](https://docs.openclaw.ai/install/hetzner)
- [exe.dev](https://docs.openclaw.ai/install/exe-dev)

How it works in the cloud: the **Gateway runs on the server**, and you access it
from your laptop/phone via the Control UI (or Tailscale/SSH). Your state + workspace
live on the server, so treat the host as the source of truth and back it up.You can pair **nodes** (Mac/iOS/Android/headless) to that cloud Gateway to access
local screen/camera/canvas or run commands on your laptop while keeping the
Gateway in the cloud.Hub: [Platforms](https://docs.openclaw.ai/platforms). Remote access: [Gateway remote](https://docs.openclaw.ai/gateway/remote).
Nodes: [Nodes](https://docs.openclaw.ai/nodes), [Nodes CLI](https://docs.openclaw.ai/cli/nodes).

Can I ask OpenClaw to update itself?

Short answer: **possible, not recommended**. The update flow can restart the
Gateway (which drops the active session), may need a clean git checkout, and
can prompt for confirmation. Safer: run updates from a shell as the operator.Use the CLI:

```
openclaw update
openclaw update status
openclaw update --channel stable|beta|dev
openclaw update --tag <dist-tag|version>
openclaw update --no-restart
```

If you must automate from an agent:

```
openclaw update --yes --no-restart
openclaw gateway restart
```

Docs: [Update](https://docs.openclaw.ai/cli/update), [Updating](https://docs.openclaw.ai/install/updating).

What does onboarding actually do?

`openclaw onboard` is the recommended setup path. In **local mode** it walks you through:

- **Model/auth setup** (provider OAuth, API keys, Anthropic setup-token, plus local model options such as LM Studio)
- **Workspace** location + bootstrap files
- **Gateway settings** (bind/port/auth/tailscale)
- **Channels** (WhatsApp, Telegram, Discord, Mattermost, Signal, iMessage, plus bundled channel plugins like QQ Bot)
- **Daemon install** (LaunchAgent on macOS; systemd user unit on Linux/WSL2)
- **Health checks** and **skills** selection

It also warns if your configured model is unknown or missing auth.

Do I need a Claude or OpenAI subscription to run this?

No. You can run OpenClaw with **API keys** (Anthropic/OpenAI/others) or with
**local-only models** so your data stays on your device. Subscriptions (Claude
Pro/Max or OpenAI Codex) are optional ways to authenticate those providers.For Anthropic in OpenClaw, the practical split is:

- **Anthropic API key**: normal Anthropic API billing
- **Claude CLI / Claude subscription auth in OpenClaw**: Anthropic staff
told us this usage is allowed again, and OpenClaw is treating `claude -p`
usage as sanctioned for this integration unless Anthropic publishes a new
policy

For long-lived gateway hosts, Anthropic API keys are still the more
predictable setup. OpenAI Codex OAuth is explicitly supported for external
tools like OpenClaw.OpenClaw also supports other hosted subscription-style options including
**Qwen Cloud Coding Plan**, **MiniMax Coding Plan**, and
**Z.AI / GLM Coding Plan**.Docs: [Anthropic](https://docs.openclaw.ai/providers/anthropic), [OpenAI](https://docs.openclaw.ai/providers/openai),
[Qwen Cloud](https://docs.openclaw.ai/providers/qwen),
[MiniMax](https://docs.openclaw.ai/providers/minimax), [GLM Models](https://docs.openclaw.ai/providers/glm),
[Local models](https://docs.openclaw.ai/gateway/local-models), [Models](https://docs.openclaw.ai/concepts/models).

Can I use Claude Max subscription without an API key?

Yes.Anthropic staff told us OpenClaw-style Claude CLI usage is allowed again, so
OpenClaw treats Claude subscription auth and `claude -p` usage as sanctioned
for this integration unless Anthropic publishes a new policy. If you want
the most predictable server-side setup, use an Anthropic API key instead.

Do you support Claude subscription auth (Claude Pro or Max)?

Yes.Anthropic staff told us this usage is allowed again, so OpenClaw treats
Claude CLI reuse and `claude -p` usage as sanctioned for this integration
unless Anthropic publishes a new policy.Anthropic setup-token is still available as a supported OpenClaw token path, but OpenClaw now prefers Claude CLI reuse and `claude -p` when available.
For production or multi-user workloads, Anthropic API key auth is still the
safer, more predictable choice. If you want other subscription-style hosted
options in OpenClaw, see [OpenAI](https://docs.openclaw.ai/providers/openai), [Qwen / Model\\
Cloud](https://docs.openclaw.ai/providers/qwen), [MiniMax](https://docs.openclaw.ai/providers/minimax), and [GLM\\
Models](https://docs.openclaw.ai/providers/glm).

Why am I seeing HTTP 429 rate\_limit\_error from Anthropic?

That means your **Anthropic quota/rate limit** is exhausted for the current window. If you
use **Claude CLI**, wait for the window to reset or upgrade your plan. If you
use an **Anthropic API key**, check the Anthropic Console
for usage/billing and raise limits as needed.If the message is specifically:
`Extra usage is required for long context requests`, the request is trying to use
Anthropic’s 1M context beta (`context1m: true`). That only works when your
credential is eligible for long-context billing (API key billing or the
OpenClaw Claude-login path with Extra Usage enabled).Tip: set a **fallback model** so OpenClaw can keep replying while a provider is rate-limited.
See [Models](https://docs.openclaw.ai/cli/models), [OAuth](https://docs.openclaw.ai/concepts/oauth), and
[/gateway/troubleshooting#anthropic-429-extra-usage-required-for-long-context](https://docs.openclaw.ai/gateway/troubleshooting#anthropic-429-extra-usage-required-for-long-context).

Is AWS Bedrock supported?

Yes. OpenClaw has a bundled **Amazon Bedrock (Converse)** provider. With AWS env markers present, OpenClaw can auto-discover the streaming/text Bedrock catalog and merge it as an implicit `amazon-bedrock` provider; otherwise you can explicitly enable `plugins.entries.amazon-bedrock.config.discovery.enabled` or add a manual provider entry. See [Amazon Bedrock](https://docs.openclaw.ai/providers/bedrock) and [Model providers](https://docs.openclaw.ai/providers/models). If you prefer a managed key flow, an OpenAI-compatible proxy in front of Bedrock is still a valid option.

How does Codex auth work?

OpenClaw supports **OpenAI Code (Codex)** via OAuth (ChatGPT sign-in). Use
`openai/gpt-5.5` with `agentRuntime.id: "codex"` for the common setup:
ChatGPT/Codex subscription auth plus native Codex app-server execution. Use
`openai-codex/gpt-5.5` only when you want Codex OAuth through the default
PI runner. Use `openai/gpt-5.5` without the Codex runtime override for
direct OpenAI API-key access.
See [Model providers](https://docs.openclaw.ai/concepts/model-providers) and [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard).

Why does OpenClaw still mention openai-codex?

`openai-codex` is the provider and auth-profile id for ChatGPT/Codex OAuth.
It is also the explicit PI model prefix for Codex OAuth:

- `openai/gpt-5.5` \+ `agentRuntime.id: "codex"` = ChatGPT/Codex subscription auth with native Codex runtime
- `openai-codex/gpt-5.5` = Codex OAuth route in PI
- `openai/gpt-5.5` without a Codex runtime override = direct OpenAI API-key route in PI
- `openai-codex:...` = auth profile id, not a model ref

If you want the direct OpenAI Platform billing/limit path, set
`OPENAI_API_KEY`. If you want ChatGPT/Codex subscription auth, sign in with
`openclaw models auth login --provider openai-codex`. For native Codex
runtime, keep the model ref as `openai/gpt-5.5` and set
`agentRuntime.id: "codex"`. Use `openai-codex/*` model refs only for PI
runs.

Why can Codex OAuth limits differ from ChatGPT web?

Codex OAuth uses OpenAI-managed, plan-dependent quota windows. In practice,
those limits can differ from the ChatGPT website/app experience, even when
both are tied to the same account.OpenClaw can show the currently visible provider usage/quota windows in
`openclaw models status`, but it does not invent or normalize ChatGPT-web
entitlements into direct API access. If you want the direct OpenAI Platform
billing/limit path, use `openai/*` with an API key.

Do you support OpenAI subscription auth (Codex OAuth)?

Yes. OpenClaw fully supports **OpenAI Code (Codex) subscription OAuth**.
OpenAI explicitly allows subscription OAuth usage in external tools/workflows
like OpenClaw. Onboarding can run the OAuth flow for you.See [OAuth](https://docs.openclaw.ai/concepts/oauth), [Model providers](https://docs.openclaw.ai/concepts/model-providers), and [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard).

How do I set up Gemini CLI OAuth?

Gemini CLI uses a **plugin auth flow**, not a client id or secret in `openclaw.json`.Steps:

1. Install Gemini CLI locally so `gemini` is on `PATH`
   - Homebrew: `brew install gemini-cli`
   - npm: `npm install -g @google/gemini-cli`
2. Enable the plugin: `openclaw plugins enable google`
3. Login: `openclaw models auth login --provider google-gemini-cli --set-default`
4. Default model after login: `google-gemini-cli/gemini-3-flash-preview`
5. If requests fail, set `GOOGLE_CLOUD_PROJECT` or `GOOGLE_CLOUD_PROJECT_ID` on the gateway host

This stores OAuth tokens in auth profiles on the gateway host. Details: [Model providers](https://docs.openclaw.ai/concepts/model-providers).

Is a local model OK for casual chats?

Usually no. OpenClaw needs large context + strong safety; small cards truncate and leak. If you must, run the **largest** model build you can locally (LM Studio) and see [/gateway/local-models](https://docs.openclaw.ai/gateway/local-models). Smaller/quantized models increase prompt-injection risk - see [Security](https://docs.openclaw.ai/gateway/security).

How do I keep hosted model traffic in a specific region?

Pick region-pinned endpoints. OpenRouter exposes US-hosted options for MiniMax, Kimi, and GLM; choose the US-hosted variant to keep data in-region. You can still list Anthropic/OpenAI alongside these by using `models.mode: "merge"` so fallbacks stay available while respecting the regioned provider you select.

Do I have to buy a Mac Mini to install this?

No. OpenClaw runs on macOS or Linux (Windows via WSL2). A Mac mini is optional - some people
buy one as an always-on host, but a small VPS, home server, or Raspberry Pi-class box works too.You only need a Mac **for macOS-only tools**. For iMessage, use [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles) (recommended) - the BlueBubbles server runs on any Mac, and the Gateway can run on Linux or elsewhere. If you want other macOS-only tools, run the Gateway on a Mac or pair a macOS node.Docs: [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles), [Nodes](https://docs.openclaw.ai/nodes), [Mac remote mode](https://docs.openclaw.ai/platforms/mac/remote).

Do I need a Mac mini for iMessage support?

You need **some macOS device** signed into Messages. It does **not** have to be a Mac mini -
any Mac works. **Use [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles)** (recommended) for iMessage - the BlueBubbles server runs on macOS, while the Gateway can run on Linux or elsewhere.Common setups:

- Run the Gateway on Linux/VPS, and run the BlueBubbles server on any Mac signed into Messages.
- Run everything on the Mac if you want the simplest single-machine setup.

Docs: [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles), [Nodes](https://docs.openclaw.ai/nodes),
[Mac remote mode](https://docs.openclaw.ai/platforms/mac/remote).

If I buy a Mac mini to run OpenClaw, can I connect it to my MacBook Pro?

Yes. The **Mac mini can run the Gateway**, and your MacBook Pro can connect as a
**node** (companion device). Nodes don’t run the Gateway - they provide extra
capabilities like screen/camera/canvas and `system.run` on that device.Common pattern:

- Gateway on the Mac mini (always-on).
- MacBook Pro runs the macOS app or a node host and pairs to the Gateway.
- Use `openclaw nodes status` / `openclaw nodes list` to see it.

Docs: [Nodes](https://docs.openclaw.ai/nodes), [Nodes CLI](https://docs.openclaw.ai/cli/nodes).

Can I use Bun?

Bun is **not recommended**. We see runtime bugs, especially with WhatsApp and Telegram.
Use **Node** for stable gateways.If you still want to experiment with Bun, do it on a non-production gateway
without WhatsApp/Telegram.

Telegram: what goes in allowFrom?

`channels.telegram.allowFrom` is **the human sender’s Telegram user ID** (numeric). It is not the bot username.Setup asks for numeric user IDs only. If you already have legacy `@username` entries in config, `openclaw doctor --fix` can try to resolve them.Safer (no third-party bot):

- DM your bot, then run `openclaw logs --follow` and read `from.id`.

Official Bot API:

- DM your bot, then call `https://api.telegram.org/bot<bot_token>/getUpdates` and read `message.from.id`.

Third-party (less private):

- DM `@userinfobot` or `@getidsbot`.

See [/channels/telegram](https://docs.openclaw.ai/channels/telegram#access-control-and-activation).

Can multiple people use one WhatsApp number with different OpenClaw instances?

Yes, via **multi-agent routing**. Bind each sender’s WhatsApp **DM** (peer `kind: "direct"`, sender E.164 like `+15551234567`) to a different `agentId`, so each person gets their own workspace and session store. Replies still come from the **same WhatsApp account**, and DM access control (`channels.whatsapp.dmPolicy` / `channels.whatsapp.allowFrom`) is global per WhatsApp account. See [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent) and [WhatsApp](https://docs.openclaw.ai/channels/whatsapp).

Can I run a "fast chat" agent and an "Opus for coding" agent?

Yes. Use multi-agent routing: give each agent its own default model, then bind inbound routes (provider account or specific peers) to each agent. Example config lives in [Multi-Agent Routing](https://docs.openclaw.ai/concepts/multi-agent). See also [Models](https://docs.openclaw.ai/concepts/models) and [Configuration](https://docs.openclaw.ai/gateway/configuration).

Does Homebrew work on Linux?

Yes. Homebrew supports Linux (Linuxbrew). Quick setup:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> ~/.profile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
brew install <formula>
```

If you run OpenClaw via systemd, ensure the service PATH includes `/home/linuxbrew/.linuxbrew/bin` (or your brew prefix) so `brew`-installed tools resolve in non-login shells.
Recent builds also prepend common user bin dirs on Linux systemd services (for example `~/.local/bin`, `~/.npm-global/bin`, `~/.local/share/pnpm`, `~/.bun/bin`) and honor `PNPM_HOME`, `NPM_CONFIG_PREFIX`, `BUN_INSTALL`, `VOLTA_HOME`, `ASDF_DATA_DIR`, `NVM_DIR`, and `FNM_DIR` when set.

Difference between the hackable git install and npm install

- **Hackable (git) install:** full source checkout, editable, best for contributors.
You run builds locally and can patch code/docs.
- **npm install:** global CLI install, no repo, best for “just run it.”
Updates come from npm dist-tags.

Docs: [Getting started](https://docs.openclaw.ai/start/getting-started), [Updating](https://docs.openclaw.ai/install/updating).

Can I switch between npm and git installs later?

Yes. Use `openclaw update --channel ...` when OpenClaw is already installed.
This **does not delete your data** \- it only changes the OpenClaw code install.
Your state (`~/.openclaw`) and workspace (`~/.openclaw/workspace`) stay untouched.From npm to git:

```
openclaw update --channel dev
```

From git to npm:

```
openclaw update --channel stable
```

Add `--dry-run` to preview the planned mode switch first. The updater runs
Doctor follow-ups, refreshes plugin sources for the target channel, and
restarts the gateway unless you pass `--no-restart`.The installer can force either mode too:

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method npm
```

Backup tips: see [Backup strategy](https://docs.openclaw.ai/help/faq#where-things-live-on-disk).

Should I run the Gateway on my laptop or a VPS?

Short answer: **if you want 24/7 reliability, use a VPS**. If you want the
lowest friction and you’re okay with sleep/restarts, run it locally.**Laptop (local Gateway)**

- **Pros:** no server cost, direct access to local files, live browser window.
- **Cons:** sleep/network drops = disconnects, OS updates/reboots interrupt, must stay awake.

**VPS / cloud**

- **Pros:** always-on, stable network, no laptop sleep issues, easier to keep running.
- **Cons:** often run headless (use screenshots), remote file access only, you must SSH for updates.

**OpenClaw-specific note:** WhatsApp/Telegram/Slack/Mattermost/Discord all work fine from a VPS. The only real trade-off is **headless browser** vs a visible window. See [Browser](https://docs.openclaw.ai/tools/browser).**Recommended default:** VPS if you had gateway disconnects before. Local is great when you’re actively using the Mac and want local file access or UI automation with a visible browser.

How important is it to run OpenClaw on a dedicated machine?

Not required, but **recommended for reliability and isolation**.

- **Dedicated host (VPS/Mac mini/Pi):** always-on, fewer sleep/reboot interruptions, cleaner permissions, easier to keep running.
- **Shared laptop/desktop:** totally fine for testing and active use, but expect pauses when the machine sleeps or updates.

If you want the best of both worlds, keep the Gateway on a dedicated host and pair your laptop as a **node** for local screen/camera/exec tools. See [Nodes](https://docs.openclaw.ai/nodes).
For security guidance, read [Security](https://docs.openclaw.ai/gateway/security).

What are the minimum VPS requirements and recommended OS?

OpenClaw is lightweight. For a basic Gateway + one chat channel:

- **Absolute minimum:** 1 vCPU, 1GB RAM, ~500MB disk.
- **Recommended:** 1-2 vCPU, 2GB RAM or more for headroom (logs, media, multiple channels). Node tools and browser automation can be resource hungry.

OS: use **Ubuntu LTS** (or any modern Debian/Ubuntu). The Linux install path is best tested there.Docs: [Linux](https://docs.openclaw.ai/platforms/linux), [VPS hosting](https://docs.openclaw.ai/vps).

Can I run OpenClaw in a VM and what are the requirements?

Yes. Treat a VM the same as a VPS: it needs to be always on, reachable, and have enough
RAM for the Gateway and any channels you enable.Baseline guidance:

- **Absolute minimum:** 1 vCPU, 1GB RAM.
- **Recommended:** 2GB RAM or more if you run multiple channels, browser automation, or media tools.
- **OS:** Ubuntu LTS or another modern Debian/Ubuntu.

If you are on Windows, **WSL2 is the easiest VM style setup** and has the best tooling
compatibility. See [Windows](https://docs.openclaw.ai/platforms/windows), [VPS hosting](https://docs.openclaw.ai/vps).
If you are running macOS in a VM, see [macOS VM](https://docs.openclaw.ai/install/macos-vm).

## [​](https://docs.openclaw.ai/help/faq-first-run\#related)  Related

- [FAQ](https://docs.openclaw.ai/help/faq) — the main FAQ (models, sessions, gateway, security, more)
- [Install overview](https://docs.openclaw.ai/install)
- [Getting started](https://docs.openclaw.ai/start/getting-started)
- [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting)

[FAQ](https://docs.openclaw.ai/help/faq) [Models FAQ](https://docs.openclaw.ai/help/faq-models)

Ctrl+I