---
source_url: https://docs.openclaw.ai/tools/browser-login
title: "Browser login - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/browser-login#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web browser

Browser login

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Browser login + X/Twitter posting](https://docs.openclaw.ai/tools/browser-login#browser-login-%2B-x%2Ftwitter-posting)
- [Manual login (recommended)](https://docs.openclaw.ai/tools/browser-login#manual-login-recommended)
- [Which Chrome profile is used?](https://docs.openclaw.ai/tools/browser-login#which-chrome-profile-is-used)
- [X/Twitter: recommended flow](https://docs.openclaw.ai/tools/browser-login#x%2Ftwitter-recommended-flow)
- [Sandboxing + host browser access](https://docs.openclaw.ai/tools/browser-login#sandboxing-%2B-host-browser-access)
- [Related](https://docs.openclaw.ai/tools/browser-login#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/tools/browser-login\#browser-login-+-x/twitter-posting)  Browser login + X/Twitter posting

## [​](https://docs.openclaw.ai/tools/browser-login\#manual-login-recommended)  Manual login (recommended)

When a site requires login, **sign in manually** in the **host** browser profile (the openclaw browser).Do **not** give the model your credentials. Automated logins often trigger anti‑bot defenses and can lock the account.Back to the main browser docs: [Browser](https://docs.openclaw.ai/tools/browser).

## [​](https://docs.openclaw.ai/tools/browser-login\#which-chrome-profile-is-used)  Which Chrome profile is used?

OpenClaw controls a **dedicated Chrome profile** (named `openclaw`, orange‑tinted UI). This is separate from your daily browser profile.For agent browser tool calls:

- Default choice: the agent should use its isolated `openclaw` browser.
- Use `profile="user"` only when existing logged-in sessions matter and the user is at the computer to click/approve any attach prompt.
- If you have multiple user-browser profiles, specify the profile explicitly instead of guessing.

Two easy ways to access it:

1. **Ask the agent to open the browser** and then log in yourself.
2. **Open it via CLI**:

```
openclaw browser start
openclaw browser open https://x.com
```

If you have multiple profiles, pass `--browser-profile <name>` (the default is `openclaw`).

## [​](https://docs.openclaw.ai/tools/browser-login\#x/twitter-recommended-flow)  X/Twitter: recommended flow

- **Read/search/threads:** use the **host** browser (manual login).
- **Post updates:** use the **host** browser (manual login).

## [​](https://docs.openclaw.ai/tools/browser-login\#sandboxing-+-host-browser-access)  Sandboxing + host browser access

Sandboxed browser sessions are **more likely** to trigger bot detection. For X/Twitter (and other strict sites), prefer the **host** browser.If the agent is sandboxed, the browser tool defaults to the sandbox. To allow host control:

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",
        browser: {
          allowHostControl: true,
        },
      },
    },
  },
}
```

Then target the host browser:

```
openclaw browser open https://x.com --browser-profile openclaw --target host
```

Or disable sandboxing for the agent that posts updates.

## [​](https://docs.openclaw.ai/tools/browser-login\#related)  Related

- [Browser](https://docs.openclaw.ai/tools/browser)
- [Browser Linux troubleshooting](https://docs.openclaw.ai/tools/browser-linux-troubleshooting)
- [Browser WSL2 troubleshooting](https://docs.openclaw.ai/tools/browser-wsl2-windows-remote-cdp-troubleshooting)

[Browser control API](https://docs.openclaw.ai/tools/browser-control) [Browser troubleshooting](https://docs.openclaw.ai/tools/browser-linux-troubleshooting)

Ctrl+I