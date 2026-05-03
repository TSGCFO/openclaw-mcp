---
source_url: https://docs.openclaw.ai/cli/browser
title: "Browser - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/browser#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Tools and execution

Browser

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw browser](https://docs.openclaw.ai/cli/browser#openclaw-browser)
- [Common flags](https://docs.openclaw.ai/cli/browser#common-flags)
- [Quick start (local)](https://docs.openclaw.ai/cli/browser#quick-start-local)
- [Quick troubleshooting](https://docs.openclaw.ai/cli/browser#quick-troubleshooting)
- [Lifecycle](https://docs.openclaw.ai/cli/browser#lifecycle)
- [If the command is missing](https://docs.openclaw.ai/cli/browser#if-the-command-is-missing)
- [Profiles](https://docs.openclaw.ai/cli/browser#profiles)
- [Tabs](https://docs.openclaw.ai/cli/browser#tabs)
- [Snapshot / screenshot / actions](https://docs.openclaw.ai/cli/browser#snapshot-%2F-screenshot-%2F-actions)
- [State and storage](https://docs.openclaw.ai/cli/browser#state-and-storage)
- [Debugging](https://docs.openclaw.ai/cli/browser#debugging)
- [Existing Chrome via MCP](https://docs.openclaw.ai/cli/browser#existing-chrome-via-mcp)
- [Remote browser control (node host proxy)](https://docs.openclaw.ai/cli/browser#remote-browser-control-node-host-proxy)
- [Related](https://docs.openclaw.ai/cli/browser#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/browser\#openclaw-browser)  `openclaw browser`

Manage OpenClaw’s browser control surface and run browser actions (lifecycle, profiles, tabs, snapshots, screenshots, navigation, input, state emulation, and debugging).Related:

- Browser tool + API: [Browser tool](https://docs.openclaw.ai/tools/browser)

## [​](https://docs.openclaw.ai/cli/browser\#common-flags)  Common flags

- `--url <gatewayWsUrl>`: Gateway WebSocket URL (defaults to config).
- `--token <token>`: Gateway token (if required).
- `--timeout <ms>`: request timeout (ms).
- `--expect-final`: wait for a final Gateway response.
- `--browser-profile <name>`: choose a browser profile (default from config).
- `--json`: machine-readable output (where supported).

## [​](https://docs.openclaw.ai/cli/browser\#quick-start-local)  Quick start (local)

```
openclaw browser profiles
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw open https://example.com
openclaw browser --browser-profile openclaw snapshot
```

Agents can run the same readiness check with `browser({ action: "doctor" })`.

## [​](https://docs.openclaw.ai/cli/browser\#quick-troubleshooting)  Quick troubleshooting

If `start` fails with `not reachable after start`, troubleshoot CDP readiness first. If `start` and `tabs` succeed but `open` or `navigate` fails, the browser control plane is healthy and the failure is usually navigation SSRF policy.Minimal sequence:

```
openclaw browser --browser-profile openclaw doctor
openclaw browser --browser-profile openclaw start
openclaw browser --browser-profile openclaw tabs
openclaw browser --browser-profile openclaw open https://example.com
```

Detailed guidance: [Browser troubleshooting](https://docs.openclaw.ai/tools/browser#cdp-startup-failure-vs-navigation-ssrf-block)

## [​](https://docs.openclaw.ai/cli/browser\#lifecycle)  Lifecycle

```
openclaw browser status
openclaw browser doctor
openclaw browser doctor --deep
openclaw browser start
openclaw browser start --headless
openclaw browser stop
openclaw browser --browser-profile openclaw reset-profile
```

Notes:

- `doctor --deep` adds a live snapshot probe. It is useful when basic CDP
readiness is green but you want proof that the current tab can be inspected.
- For `attachOnly` and remote CDP profiles, `openclaw browser stop` closes the
active control session and clears temporary emulation overrides even when
OpenClaw did not launch the browser process itself.
- For local managed profiles, `openclaw browser stop` stops the spawned browser
process.
- `openclaw browser start --headless` applies only to that start request and
only when OpenClaw launches a local managed browser. It does not rewrite
`browser.headless` or profile config, and it is a no-op for an already-running
browser.
- On Linux hosts without `DISPLAY` or `WAYLAND_DISPLAY`, local managed profiles
run headless automatically unless `OPENCLAW_BROWSER_HEADLESS=0`,
`browser.headless=false`, or `browser.profiles.<name>.headless=false`
explicitly requests a visible browser.

## [​](https://docs.openclaw.ai/cli/browser\#if-the-command-is-missing)  If the command is missing

If `openclaw browser` is an unknown command, check `plugins.allow` in
`~/.openclaw/openclaw.json`.When `plugins.allow` is present, list the bundled browser plugin explicitly
unless the config already has a root `browser` block:

```
{
  plugins: {
    allow: ["telegram", "browser"],
  },
}
```

An explicit root `browser` block, for example `browser.enabled=true` or
`browser.profiles.<name>`, also activates the bundled browser plugin under a
restrictive plugin allowlist.Related: [Browser tool](https://docs.openclaw.ai/tools/browser#missing-browser-command-or-tool)

## [​](https://docs.openclaw.ai/cli/browser\#profiles)  Profiles

Profiles are named browser routing configs. In practice:

- `openclaw`: launches or attaches to a dedicated OpenClaw-managed Chrome instance (isolated user data dir).
- `user`: controls your existing signed-in Chrome session via Chrome DevTools MCP.
- custom CDP profiles: point at a local or remote CDP endpoint.

```
openclaw browser profiles
openclaw browser create-profile --name work --color "#FF5A36"
openclaw browser create-profile --name chrome-live --driver existing-session
openclaw browser create-profile --name remote --cdp-url https://browser-host.example.com
openclaw browser delete-profile --name work
```

Use a specific profile:

```
openclaw browser --browser-profile work tabs
```

## [​](https://docs.openclaw.ai/cli/browser\#tabs)  Tabs

```
openclaw browser tabs
openclaw browser tab new --label docs
openclaw browser tab label t1 docs
openclaw browser tab select 2
openclaw browser tab close 2
openclaw browser open https://docs.openclaw.ai --label docs
openclaw browser focus docs
openclaw browser close t1
```

`tabs` returns `suggestedTargetId` first, then the stable `tabId` such as `t1`,
the optional label, and the raw `targetId`. Agents should pass
`suggestedTargetId` back into `focus`, `close`, snapshots, and actions. You can
assign a label with `open --label`, `tab new --label`, or `tab label`; labels,
tab ids, raw target ids, and unique target-id prefixes are all accepted.
When Chromium replaces the underlying raw target during a navigation or form
submit, OpenClaw keeps the stable `tabId`/label attached to the replacement tab
when it can prove the match. Raw target ids remain volatile; prefer
`suggestedTargetId`.

## [​](https://docs.openclaw.ai/cli/browser\#snapshot-/-screenshot-/-actions)  Snapshot / screenshot / actions

Snapshot:

```
openclaw browser snapshot
openclaw browser snapshot --urls
```

Screenshot:

```
openclaw browser screenshot
openclaw browser screenshot --full-page
openclaw browser screenshot --ref e12
openclaw browser screenshot --labels
```

Notes:

- `--full-page` is for page captures only; it cannot be combined with `--ref`
or `--element`.
- `existing-session` / `user` profiles support page screenshots and `--ref`
screenshots from snapshot output, but not CSS `--element` screenshots.
- `--labels` overlays current snapshot refs on the screenshot.
- `snapshot --urls` appends discovered link destinations to AI snapshots so
agents can choose direct navigation targets instead of guessing from link
text alone.

Navigate/click/type (ref-based UI automation):

```
openclaw browser navigate https://example.com
openclaw browser click <ref>
openclaw browser click-coords 120 340
openclaw browser type <ref> "hello"
openclaw browser press Enter
openclaw browser hover <ref>
openclaw browser scrollintoview <ref>
openclaw browser drag <startRef> <endRef>
openclaw browser select <ref> OptionA OptionB
openclaw browser fill --fields '[{"ref":"1","value":"Ada"}]'
openclaw browser wait --text "Done"
openclaw browser evaluate --fn '(el) => el.textContent' --ref <ref>
```

Action responses return the current raw `targetId` after action-triggered page
replacement when OpenClaw can prove the replacement tab. Scripts should still
store and pass `suggestedTargetId`/labels for long-lived workflows.File + dialog helpers:

```
openclaw browser upload /tmp/openclaw/uploads/file.pdf --ref <ref>
openclaw browser waitfordownload
openclaw browser download <ref> report.pdf
openclaw browser dialog --accept
```

Managed Chrome profiles save ordinary click-triggered downloads into the OpenClaw
downloads directory (`/tmp/openclaw/downloads` by default, or the configured temp
root). Use `waitfordownload` or `download` when the agent needs to wait for a
specific file and return its path; those explicit waiters own the next download.

## [​](https://docs.openclaw.ai/cli/browser\#state-and-storage)  State and storage

Viewport + emulation:

```
openclaw browser resize 1280 720
openclaw browser set viewport 1280 720
openclaw browser set offline on
openclaw browser set media dark
openclaw browser set timezone Europe/London
openclaw browser set locale en-GB
openclaw browser set geo 51.5074 -0.1278 --accuracy 25
openclaw browser set device "iPhone 14"
openclaw browser set headers '{"x-test":"1"}'
openclaw browser set credentials myuser mypass
```

Cookies + storage:

```
openclaw browser cookies
openclaw browser cookies set session abc123 --url https://example.com
openclaw browser cookies clear
openclaw browser storage local get
openclaw browser storage local set token abc123
openclaw browser storage session clear
```

## [​](https://docs.openclaw.ai/cli/browser\#debugging)  Debugging

```
openclaw browser console --level error
openclaw browser pdf
openclaw browser responsebody "**/api"
openclaw browser highlight <ref>
openclaw browser errors --clear
openclaw browser requests --filter api
openclaw browser trace start
openclaw browser trace stop --out trace.zip
```

## [​](https://docs.openclaw.ai/cli/browser\#existing-chrome-via-mcp)  Existing Chrome via MCP

Use the built-in `user` profile, or create your own `existing-session` profile:

```
openclaw browser --browser-profile user tabs
openclaw browser create-profile --name chrome-live --driver existing-session
openclaw browser create-profile --name brave-live --driver existing-session --user-data-dir "~/Library/Application Support/BraveSoftware/Brave-Browser"
openclaw browser --browser-profile chrome-live tabs
```

This path is host-only. For Docker, headless servers, Browserless, or other remote setups, use a CDP profile instead.Current existing-session limits:

- snapshot-driven actions use refs, not CSS selectors
- `browser.actionTimeoutMs` defaults supported `act` requests to 60000 ms when
callers omit `timeoutMs`; per-call `timeoutMs` still wins.
- `click` is left-click only
- `type` does not support `slowly=true`
- `press` does not support `delayMs`
- `hover`, `scrollintoview`, `drag`, `select`, `fill`, and `evaluate` reject
per-call timeout overrides
- `select` supports one value only
- `wait --load networkidle` is not supported
- file uploads require `--ref` / `--input-ref`, do not support CSS
`--element`, and currently support one file at a time
- dialog hooks do not support `--timeout`
- screenshots support page captures and `--ref`, but not CSS `--element`
- `responsebody`, download interception, PDF export, and batch actions still
require a managed browser or raw CDP profile

## [​](https://docs.openclaw.ai/cli/browser\#remote-browser-control-node-host-proxy)  Remote browser control (node host proxy)

If the Gateway runs on a different machine than the browser, run a **node host** on the machine that has Chrome/Brave/Edge/Chromium. The Gateway will proxy browser actions to that node (no separate browser control server required).Use `gateway.nodes.browser.mode` to control auto-routing and `gateway.nodes.browser.node` to pin a specific node if multiple are connected.Security + remote setup: [Browser tool](https://docs.openclaw.ai/tools/browser), [Remote access](https://docs.openclaw.ai/gateway/remote), [Tailscale](https://docs.openclaw.ai/gateway/tailscale), [Security](https://docs.openclaw.ai/gateway/security)

## [​](https://docs.openclaw.ai/cli/browser\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Browser](https://docs.openclaw.ai/tools/browser)

[Approvals](https://docs.openclaw.ai/cli/approvals) [Cron](https://docs.openclaw.ai/cli/cron)

Ctrl+I