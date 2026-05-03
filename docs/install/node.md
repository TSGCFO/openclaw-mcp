---
source_url: https://docs.openclaw.ai/install/node
title: "Node.js - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/node#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Install overview

Node.js

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Check your version](https://docs.openclaw.ai/install/node#check-your-version)
- [Install Node](https://docs.openclaw.ai/install/node#install-node)
- [Troubleshooting](https://docs.openclaw.ai/install/node#troubleshooting)
- [openclaw: command not found](https://docs.openclaw.ai/install/node#openclaw-command-not-found)
- [Permission errors on npm install -g (Linux)](https://docs.openclaw.ai/install/node#permission-errors-on-npm-install-g-linux)
- [Related](https://docs.openclaw.ai/install/node#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw requires **Node 22.14 or newer**. **Node 24 is the default and recommended runtime** for installs, CI, and release workflows. Node 22 remains supported via the active LTS line. The [installer script](https://docs.openclaw.ai/install#alternative-install-methods) will detect and install Node automatically — this page is for when you want to set up Node yourself and make sure everything is wired up correctly (versions, PATH, global installs).

## [​](https://docs.openclaw.ai/install/node\#check-your-version)  Check your version

```
node -v
```

If this prints `v24.x.x` or higher, you’re on the recommended default. If it prints `v22.14.x` or higher, you’re on the supported Node 22 LTS path, but we still recommend upgrading to Node 24 when convenient. If Node isn’t installed or the version is too old, pick an install method below.

## [​](https://docs.openclaw.ai/install/node\#install-node)  Install Node

- macOS

- Linux

- Windows


**Homebrew** (recommended):

```
brew install node
```

Or download the macOS installer from [nodejs.org](https://nodejs.org/).

**Ubuntu / Debian:**

```
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**Fedora / RHEL:**

```
sudo dnf install nodejs
```

Or use a version manager (see below).

**winget** (recommended):

```
winget install OpenJS.NodeJS.LTS
```

**Chocolatey:**

```
choco install nodejs-lts
```

Or download the Windows installer from [nodejs.org](https://nodejs.org/).

Using a version manager (nvm, fnm, mise, asdf)

Version managers let you switch between Node versions easily. Popular options:

- [**fnm**](https://github.com/Schniz/fnm) — fast, cross-platform
- [**nvm**](https://github.com/nvm-sh/nvm) — widely used on macOS/Linux
- [**mise**](https://mise.jdx.dev/) — polyglot (Node, Python, Ruby, etc.)

Example with fnm:

```
fnm install 24
fnm use 24
```

Make sure your version manager is initialized in your shell startup file (`~/.zshrc` or `~/.bashrc`). If it isn’t, `openclaw` may not be found in new terminal sessions because the PATH won’t include Node’s bin directory.

## [​](https://docs.openclaw.ai/install/node\#troubleshooting)  Troubleshooting

### [​](https://docs.openclaw.ai/install/node\#openclaw-command-not-found)  `openclaw: command not found`

This almost always means npm’s global bin directory isn’t on your PATH.

1

[Navigate to header](https://docs.openclaw.ai/install/node#)

Find your global npm prefix

```
npm prefix -g
```

2

[Navigate to header](https://docs.openclaw.ai/install/node#)

Check if it's on your PATH

```
echo "$PATH"
```

Look for `<npm-prefix>/bin` (macOS/Linux) or `<npm-prefix>` (Windows) in the output.

3

[Navigate to header](https://docs.openclaw.ai/install/node#)

Add it to your shell startup file

- macOS / Linux

- Windows


Add to `~/.zshrc` or `~/.bashrc`:

```
export PATH="$(npm prefix -g)/bin:$PATH"
```

Then open a new terminal (or run `rehash` in zsh / `hash -r` in bash).

Add the output of `npm prefix -g` to your system PATH via Settings → System → Environment Variables.

### [​](https://docs.openclaw.ai/install/node\#permission-errors-on-npm-install-g-linux)  Permission errors on `npm install -g` (Linux)

If you see `EACCES` errors, switch npm’s global prefix to a user-writable directory:

```
mkdir -p "$HOME/.npm-global"
npm config set prefix "$HOME/.npm-global"
export PATH="$HOME/.npm-global/bin:$PATH"
```

Add the `export PATH=...` line to your `~/.bashrc` or `~/.zshrc` to make it permanent.

## [​](https://docs.openclaw.ai/install/node\#related)  Related

- [Install Overview](https://docs.openclaw.ai/install) — all installation methods
- [Updating](https://docs.openclaw.ai/install/updating) — keeping OpenClaw up to date
- [Getting Started](https://docs.openclaw.ai/start/getting-started) — first steps after install

[Installer internals](https://docs.openclaw.ai/install/installer) [Updating](https://docs.openclaw.ai/install/updating)

Ctrl+I