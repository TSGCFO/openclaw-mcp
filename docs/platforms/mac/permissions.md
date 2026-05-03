---
source_url: https://docs.openclaw.ai/platforms/mac/permissions
title: "macOS permissions - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/platforms/mac/permissions#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Setup

macOS permissions

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Requirements for stable permissions](https://docs.openclaw.ai/platforms/mac/permissions#requirements-for-stable-permissions)
- [Recovery checklist when prompts disappear](https://docs.openclaw.ai/platforms/mac/permissions#recovery-checklist-when-prompts-disappear)
- [Files and folders permissions (Desktop/Documents/Downloads)](https://docs.openclaw.ai/platforms/mac/permissions#files-and-folders-permissions-desktop%2Fdocuments%2Fdownloads)
- [Related](https://docs.openclaw.ai/platforms/mac/permissions#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

macOS permission grants are fragile. TCC associates a permission grant with the
app’s code signature, bundle identifier, and on-disk path. If any of those change,
macOS treats the app as new and may drop or hide prompts.

## [​](https://docs.openclaw.ai/platforms/mac/permissions\#requirements-for-stable-permissions)  Requirements for stable permissions

- Same path: run the app from a fixed location (for OpenClaw, `dist/OpenClaw.app`).
- Same bundle identifier: changing the bundle ID creates a new permission identity.
- Signed app: unsigned or ad-hoc signed builds do not persist permissions.
- Consistent signature: use a real Apple Development or Developer ID certificate
so the signature stays stable across rebuilds.

Ad-hoc signatures generate a new identity every build. macOS will forget previous
grants, and prompts can disappear entirely until the stale entries are cleared.

## [​](https://docs.openclaw.ai/platforms/mac/permissions\#recovery-checklist-when-prompts-disappear)  Recovery checklist when prompts disappear

1. Quit the app.
2. Remove the app entry in System Settings -> Privacy & Security.
3. Relaunch the app from the same path and re-grant permissions.
4. If the prompt still does not appear, reset TCC entries with `tccutil` and try again.
5. Some permissions only reappear after a full macOS restart.

Example resets (replace bundle ID as needed):

```
sudo tccutil reset Accessibility ai.openclaw.mac
sudo tccutil reset ScreenCapture ai.openclaw.mac
sudo tccutil reset AppleEvents
```

## [​](https://docs.openclaw.ai/platforms/mac/permissions\#files-and-folders-permissions-desktop/documents/downloads)  Files and folders permissions (Desktop/Documents/Downloads)

macOS may also gate Desktop, Documents, and Downloads for terminal/background processes. If file reads or directory listings hang, grant access to the same process context that performs file operations (for example Terminal/iTerm, LaunchAgent-launched app, or SSH process).Workaround: move files into the OpenClaw workspace (`~/.openclaw/workspace`) if you want to avoid per-folder grants.If you are testing permissions, always sign with a real certificate. Ad-hoc
builds are only acceptable for quick local runs where permissions do not matter.

## [​](https://docs.openclaw.ai/platforms/mac/permissions\#related)  Related

- [macOS app](https://docs.openclaw.ai/platforms/macos)
- [macOS signing](https://docs.openclaw.ai/platforms/mac/signing)

[Menu bar icon](https://docs.openclaw.ai/platforms/mac/icon) [macOS signing](https://docs.openclaw.ai/platforms/mac/signing)

Ctrl+I