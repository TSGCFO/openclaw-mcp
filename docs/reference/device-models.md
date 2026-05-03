---
source_url: https://docs.openclaw.ai/reference/device-models
title: "Device model database - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/reference/device-models#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

RPC and API

Device model database

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Data source](https://docs.openclaw.ai/reference/device-models#data-source)
- [Updating the database](https://docs.openclaw.ai/reference/device-models#updating-the-database)
- [Related](https://docs.openclaw.ai/reference/device-models#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The macOS companion app shows friendly Apple device model names in the **Instances** UI by mapping Apple model identifiers (e.g. `iPad16,6`, `Mac16,6`) to human-readable names.The mapping is vendored as JSON under:

- `apps/macos/Sources/OpenClaw/Resources/DeviceModels/`

## [​](https://docs.openclaw.ai/reference/device-models\#data-source)  Data source

We currently vendor the mapping from the MIT-licensed repository:

- `kyle-seongwoo-jun/apple-device-identifiers`

To keep builds deterministic, the JSON files are pinned to specific upstream commits (recorded in `apps/macos/Sources/OpenClaw/Resources/DeviceModels/NOTICE.md`).

## [​](https://docs.openclaw.ai/reference/device-models\#updating-the-database)  Updating the database

1. Pick the upstream commits you want to pin to (one for iOS, one for macOS).
2. Update the commit hashes in `apps/macos/Sources/OpenClaw/Resources/DeviceModels/NOTICE.md`.
3. Re-download the JSON files, pinned to those commits:

```
IOS_COMMIT="<commit sha for ios-device-identifiers.json>"
MAC_COMMIT="<commit sha for mac-device-identifiers.json>"

curl -fsSL "https://raw.githubusercontent.com/kyle-seongwoo-jun/apple-device-identifiers/${IOS_COMMIT}/ios-device-identifiers.json" \
  -o apps/macos/Sources/OpenClaw/Resources/DeviceModels/ios-device-identifiers.json

curl -fsSL "https://raw.githubusercontent.com/kyle-seongwoo-jun/apple-device-identifiers/${MAC_COMMIT}/mac-device-identifiers.json" \
  -o apps/macos/Sources/OpenClaw/Resources/DeviceModels/mac-device-identifiers.json
```

4. Ensure `apps/macos/Sources/OpenClaw/Resources/DeviceModels/LICENSE.apple-device-identifiers.txt` still matches upstream (replace it if the upstream license changes).
5. Verify the macOS app builds cleanly (no warnings):

```
swift build --package-path apps/macos
```

## [​](https://docs.openclaw.ai/reference/device-models\#related)  Related

- [Nodes](https://docs.openclaw.ai/nodes)
- [Node troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting)

[App SDK API design](https://docs.openclaw.ai/reference/openclaw-sdk-api-design) [Default AGENTS.md](https://docs.openclaw.ai/reference/AGENTS.default)

Ctrl+I