# Getting Started

_39 pages from docs.openclaw.ai_


---

## Onboard - OpenClaw

_Source: <https://docs.openclaw.ai/cli/onboard>_

# `openclaw onboard`

Interactive onboarding for local or remote Gateway setup.

## Related guides

[**CLI onboarding hub** \\
\\
Walkthrough of the interactive CLI flow.](https://docs.openclaw.ai/start/wizard)

[**Onboarding overview** \\
\\
How OpenClaw onboarding fits together.](https://docs.openclaw.ai/start/onboarding-overview)

[**CLI setup reference** \\
\\
Outputs, internals, and per-step behavior.](https://docs.openclaw.ai/start/wizard-cli-reference)

[**CLI automation** \\
\\
Non-interactive flags and scripted setups.](https://docs.openclaw.ai/start/wizard-cli-automation)

[**macOS app onboarding** \\
\\
Onboarding flow for the macOS menu bar app.](https://docs.openclaw.ai/start/onboarding)

## Examples

```
openclaw onboard
openclaw onboard --modern
openclaw onboard --flow quickstart
openclaw onboard --flow manual
openclaw onboard --flow import
openclaw onboard --import-from hermes --import-source ~/.hermes
openclaw onboard --skip-bootstrap
openclaw onboard --mode remote --remote-url wss://gateway-host:18789
```

`--flow import` uses plugin-owned migration providers such as Hermes. It only runs against a fresh OpenClaw setup; if existing config, credentials, sessions, or workspace memory/identity files are present, reset or choose a fresh setup before importing.`--modern` starts the Crestodian conversational onboarding preview. Without
`--modern`, `openclaw onboard` keeps the classic onboarding flow.For plaintext private-network `ws://` targets (trusted networks only), set
`OPENCLAW_ALLOW_INSECURE_PRIVATE_WS=1` in the onboarding process environment.
There is no `openclaw.json` equivalent for this client-side transport
break-glass.Non-interactive custom provider:

```
openclaw onboard --non-interactive \
  --auth-choice custom-api-key \
  --custom-base-url "https://llm.example.com/v1" \
  --custom-model-id "foo-large" \
  --custom-api-key "$CUSTOM_API_KEY" \
  --secret-input-mode plaintext \
  --custom-compatibility openai \
  --custom-image-input
```

`--custom-api-key` is optional in non-interactive mode. If omitted, onboarding checks `CUSTOM_API_KEY`.
OpenClaw marks common vision model IDs as image-capable automatically. Pass `--custom-image-input` for unknown custom vision IDs, or `--custom-text-input` to force text-only metadata.LM Studio also supports a provider-specific key flag in non-interactive mode:

```
openclaw onboard --non-interactive \
  --auth-choice lmstudio \
  --custom-base-url "http://localhost:1234/v1" \
  --custom-model-id "qwen/qwen3.5-9b" \
  --lmstudio-api-key "$LM_API_TOKEN" \
  --accept-risk
```

Non-interactive Ollama:

```
openclaw onboard --non-interactive \
  --auth-choice ollama \
  --custom-base-url "http://ollama-host:11434" \
  --custom-model-id "qwen3.5:27b" \
  --accept-risk
```

`--custom-base-url` defaults to `http://127.0.0.1:11434`. `--custom-model-id` is optional; if omitted, onboarding uses Ollama’s suggested defaults. Cloud model IDs such as `kimi-k2.5:cloud` also work here.Store provider keys as refs instead of plaintext:

```
openclaw onboard --non-interactive \
  --auth-choice openai-api-key \
  --secret-input-mode ref \
  --accept-risk
```

With `--secret-input-mode ref`, onboarding writes env-backed refs instead of plaintext key values.
For auth-profile backed providers this writes `keyRef` entries; for custom providers this writes `models.providers.<id>.apiKey` as an env ref (for example `{ source: "env", provider: "default", id: "CUSTOM_API_KEY" }`).Non-interactive `ref` mode contract:

- Set the provider env var in the onboarding process environment (for example `OPENAI_API_KEY`).
- Do not pass inline key flags (for example `--openai-api-key`) unless that env var is also set.
- If an inline key flag is passed without the required env var, onboarding fails fast with guidance.

Gateway token options in non-interactive mode:

- `--gateway-auth token --gateway-token <token>` stores a plaintext token.
- `--gateway-auth token --gateway-token-ref-env <name>` stores `gatew

_… [truncated; see https://docs.openclaw.ai/cli/onboard for full content]_


---

## Uninstall - OpenClaw

_Source: <https://docs.openclaw.ai/cli/uninstall>_

# `openclaw uninstall`

Uninstall the gateway service + local data (CLI remains).Options:

- `--service`: remove the gateway service
- `--state`: remove state and config
- `--workspace`: remove workspace directories
- `--app`: remove the macOS app
- `--all`: remove service, state, workspace, and app
- `--yes`: skip confirmation prompts
- `--non-interactive`: disable prompts; requires `--yes`
- `--dry-run`: print actions without removing files

Examples:

```
openclaw backup create
openclaw uninstall
openclaw uninstall --service --yes --non-interactive
openclaw uninstall --state --workspace --yes --non-interactive
openclaw uninstall --all --yes
openclaw uninstall --dry-run
```

Notes:

- Run `openclaw backup create` first if you want a restorable snapshot before removing state or workspaces.
- `--all` is shorthand for removing service, state, workspace, and app together.
- `--non-interactive` requires `--yes`.

## Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Uninstall](https://docs.openclaw.ai/install/uninstall)

[Status](https://docs.openclaw.ai/cli/status) [Update](https://docs.openclaw.ai/cli/update)

Ctrl+I


---

## Install - OpenClaw

_Source: <https://docs.openclaw.ai/install>_

[OpenClaw home page](https://docs.openclaw.ai/)

Install overview

Install

## System requirements

- **Node 24** (recommended) or Node 22.14+ — the installer script handles this automatically
- **macOS, Linux, or Windows** — both native Windows and WSL2 are supported; WSL2 is more stable. See [Windows](https://docs.openclaw.ai/platforms/windows).
- `pnpm` is only needed if you build from source

## Recommended: installer script

The fastest way to install. It detects your OS, installs Node if needed, installs OpenClaw, and launches onboarding.

- macOS / Linux / WSL2

- Windows (PowerShell)

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

```
iwr -useb https://openclaw.ai/install.ps1 | iex
```

To install without running onboarding:

- macOS / Linux / WSL2

- Windows (PowerShell)

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
```

For all flags and CI/automation options, see [Installer internals](https://docs.openclaw.ai/install/installer).

## Alternative install methods

### Local prefix installer (`install-cli.sh`)

Use this when you want OpenClaw and Node kept under a local prefix such as
`~/.openclaw`, without depending on a system-wide Node install:

```
curl -fsSL https://openclaw.ai/install-cli.sh | bash
```

It supports npm installs by default, plus git-checkout installs under the same
prefix flow. Full reference: [Installer internals](https://docs.openclaw.ai/install/installer#install-clish).Already installed? Switch between package and git installs with
`openclaw update --channel dev` and `openclaw update --channel stable`. See
[Updating](https://docs.openclaw.ai/install/updating#switch-between-npm-and-git-installs).

### npm, pnpm, or bun

If you already manage Node yourself:

- npm

- pnpm

- bun

```
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

```
pnpm add -g openclaw@latest
pnpm approve-builds -g
openclaw onboard --install-daemon
```

pnpm requires explicit approval for packages with build scripts. Run `pnpm approve-builds -g` after the first install.

```
bun add -g openclaw@latest
openclaw onboard --install-daemon
```

Bun is supported for the global CLI install path. For the Gateway runtime, Node remains the recommended daemon runtime.

Troubleshooting: sharp build errors (npm)

If `sharp` fails due to a globally installed libvips:

```
SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g openclaw@latest
```

### From source

For contributors or anyone who wants to run from a local checkout:

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install && pnpm build && pnpm ui:build
pnpm link --global
openclaw onboard --install-daemon
```

Or skip the link and use `pnpm openclaw ...` from inside the repo. See [Setup](https://docs.openclaw.ai/start/setup) for full development workflows.

### Install from GitHub main

```
npm install -g github:openclaw/openclaw#main
```

### Containers and package managers

[**Docker** \\
\\
Containerized or headless deployments.](https://docs.openclaw.ai/install/docker)

[**Podman** \\
\\
Rootless container alternative to Docker.](https://docs.openclaw.ai/install/podman)

[**Nix** \\
\\
Declarative install via Nix flake.](https://docs.openclaw.ai/install/nix)

[**Ansible** \\
\\
Automated fleet provisioning.](https://docs.openclaw.ai/install/ansible)

[**Bun** \\
\\
CLI-only usage via the Bun runtime.](https://docs.openclaw.ai/install/bun)

## Verify the install

```
openclaw --version      # confirm the CLI is available
openclaw doctor         # check for config issues
openclaw gateway status # verify the Gateway is running
```

If you want managed startup after install:

- macOS: LaunchAgent via `openclaw onboard --install-daemon` or `openclaw gateway install`
- Linux/WSL2: systemd user service via the same commands
- Native Windows: Scheduled Task first, with a per-user Startup-folder login item fallback if task

_… [truncated; see https://docs.openclaw.ai/install for full content]_


---

## Bun (experimental) - OpenClaw

_Source: <https://docs.openclaw.ai/install/bun>_

[OpenClaw home page](https://docs.openclaw.ai/)

Containers

Bun (experimental)

Bun is **not recommended for gateway runtime** (known issues with WhatsApp and Telegram). Use Node for production.

Bun is an optional local runtime for running TypeScript directly (`bun run ...`, `bun --watch ...`). The default package manager remains `pnpm`, which is fully supported and used by docs tooling. Bun cannot use `pnpm-lock.yaml` and will ignore it.

## Install

1

[Navigate to header](https://docs.openclaw.ai/install/bun#)

Install dependencies

```
bun install
```

`bun.lock` / `bun.lockb` are gitignored, so there is no repo churn. To skip lockfile writes entirely:

```
bun install --no-save
```

2

[Navigate to header](https://docs.openclaw.ai/install/bun#)

Build and test

```
bun run build
bun run vitest run
```

## Lifecycle scripts

Bun blocks dependency lifecycle scripts unless explicitly trusted. For this repo, the commonly blocked scripts are not required:

- `@whiskeysockets/baileys``preinstall` — checks Node major >= 20 (OpenClaw defaults to Node 24 and still supports Node 22 LTS, currently `22.14+`)
- `protobufjs``postinstall` — emits warnings about incompatible version schemes (no build artifacts)

If you hit a runtime issue that requires these scripts, trust them explicitly:

```
bun pm trust @whiskeysockets/baileys protobufjs
```

## Caveats

Some scripts still hardcode pnpm (for example `docs:build`, `ui:*`, `protocol:check`). Run those via pnpm for now.

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [Node.js](https://docs.openclaw.ai/install/node)
- [Updating](https://docs.openclaw.ai/install/updating)

[Ansible](https://docs.openclaw.ai/install/ansible) [ClawDock](https://docs.openclaw.ai/install/clawdock)

Ctrl+I


---

## Docker - OpenClaw

_Source: <https://docs.openclaw.ai/install/docker>_

# WhatsApp (QR)
docker compose run --rm openclaw-cli channels login

# Telegram
docker compose run --rm openclaw-cli channels add --channel telegram --token "<token>"

# Discord
docker compose run --rm openclaw-cli channels add --channel discord --token "<token>"
```

Docs: [WhatsApp](https://docs.openclaw.ai/channels/whatsapp), [Telegram](https://docs.openclaw.ai/channels/telegram), [Discord](https://docs.openclaw.ai/channels/discord)

### Manual flow

If you prefer to run each step yourself instead of using the setup script:

```
docker build -t openclaw:local -f Dockerfile .
docker compose run --rm --no-deps --entrypoint node openclaw-gateway \
  dist/index.js onboard --mode local --no-install-daemon
docker compose run --rm --no-deps --entrypoint node openclaw-gateway \
  dist/index.js config set --batch-json '[{"path":"gateway.mode","value":"local"},{"path":"gateway.bind","value":"lan"},{"path":"gateway.controlUi.allowedOrigins","value":["http://localhost:18789","http://127.0.0.1:18789"]}]'
docker compose up -d openclaw-gateway
```

Run `docker compose` from the repo root. If you enabled `OPENCLAW_EXTRA_MOUNTS`
or `OPENCLAW_HOME_VOLUME`, the setup script writes `docker-compose.extra.yml`;
include it with `-f docker-compose.yml -f docker-compose.extra.yml`.

Because `openclaw-cli` shares `openclaw-gateway`’s network namespace, it is a
post-start tool. Before `docker compose up -d openclaw-gateway`, run onboarding
and setup-time config writes through `openclaw-gateway` with
`--no-deps --entrypoint node`.

### Environment variables

The setup script accepts these optional environment variables:

| Variable | Purpose |
| --- | --- |
| `OPENCLAW_IMAGE` | Use a remote image instead of building locally |
| `OPENCLAW_DOCKER_APT_PACKAGES` | Install extra apt packages during build (space-separated) |
| `OPENCLAW_EXTENSIONS` | Include selected bundled plugin helpers at build time |
| `OPENCLAW_EXTRA_MOUNTS` | Extra host bind mounts (comma-separated `source:target[:opts]`) |
| `OPENCLAW_HOME_VOLUME` | Persist `/home/node` in a named Docker volume |
| `OPENCLAW_SANDBOX` | Opt in to sandbox bootstrap (`1`, `true`, `yes`, `on`) |
| `OPENCLAW_SKIP_ONBOARDING` | Skip the interactive onboarding step (`1`, `true`, `yes`, `on`) |
| `OPENCLAW_DOCKER_SOCKET` | Override Docker socket path |
| `OPENCLAW_DISABLE_BONJOUR` | Disable Bonjour/mDNS advertising (defaults to `1` for Docker) |
| `OPENCLAW_DISABLE_BUNDLED_SOURCE_OVERLAYS` | Disable bundled plugin source bind-mount overlays |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | Shared OTLP/HTTP collector endpoint for OpenTelemetry export |
| `OTEL_EXPORTER_OTLP_*_ENDPOINT` | Signal-specific OTLP endpoints for traces, metrics, or logs |
| `OTEL_EXPORTER_OTLP_PROTOCOL` | OTLP protocol override. Only `http/protobuf` is supported today |
| `OTEL_SERVICE_NAME` | Service name used for OpenTelemetry resources |
| `OTEL_SEMCONV_STABILITY_OPT_IN` | Opt in to latest experimental GenAI semantic attributes |
| `OPENCLAW_OTEL_PRELOADED` | Skip starting a second OpenTelemetry SDK when one is preloaded |

Maintainers can test bundled plugin source against a packaged image by mounting
one plugin source directory over its packaged source path, for example
`OPENCLAW_EXTRA_MOUNTS=/path/to/fork/extensions/synology-chat:/app/extensions/synology-chat:ro`.
That mounted source directory overrides the matching compiled
`/app/dist/extensions/synology-chat` bundle for the same plugin id.

### Observability

OpenTelemetry export is outbound from the Gateway container to your OTLP
collector. It does not require a published Docker port. If you build the image
locally and want the bundled OpenTelemetry exporter available inside the image,
include its runtime dependencies:

```
export OPENCLAW_EXTENSIONS="diagnostics-otel"
export OTEL_EXPORTER_OTLP_ENDPOINT="http://otel-collector:4318"
export OTEL_SERVICE_NAME="openclaw-gateway"
./scripts/docker/setup.sh
```

Install the official `@openclaw/diagnostics-otel` plugin from ClawHub in
packaged

_… [truncated; see https://docs.openclaw.ai/install/docker for full content]_


---

## Docker VM runtime - OpenClaw

_Source: <https://docs.openclaw.ai/install/docker-vm-runtime>_

# Example binary 1: Gmail CLI (gogcli — installs as `gog`)
# Copy the current Linux asset URL from https://github.com/steipete/gogcli/releases
RUN curl -L https://github.com/steipete/gogcli/releases/latest/download/gogcli_linux_amd64.tar.gz \
  | tar -xzO gog > /usr/local/bin/gog; \
  chmod +x /usr/local/bin/gog

# Example binary 2: Google Places CLI
# Copy the current Linux asset URL from https://github.com/steipete/goplaces/releases
RUN curl -L https://github.com/steipete/goplaces/releases/latest/download/goplaces_linux_amd64.tar.gz \
  | tar -xzO goplaces > /usr/local/bin/goplaces; \
  chmod +x /usr/local/bin/goplaces

# Example binary 3: WhatsApp CLI
# Copy the current Linux asset URL from https://github.com/steipete/wacli/releases
RUN curl -L https://github.com/steipete/wacli/releases/latest/download/wacli-linux-amd64.tar.gz \
  | tar -xzO wacli > /usr/local/bin/wacli; \
  chmod +x /usr/local/bin/wacli

# Add more binaries below using the same pattern

WORKDIR /app
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml .npmrc ./
COPY ui/package.json ./ui/package.json
COPY scripts ./scripts

RUN corepack enable
RUN pnpm install --frozen-lockfile

COPY . .
RUN pnpm build
RUN pnpm ui:install
RUN pnpm ui:build

ENV NODE_ENV=production

CMD ["node","dist/index.js"]
```

The URLs above are examples. For ARM-based VMs, choose the `arm64` assets. For reproducible builds, pin versioned release URLs.

## Build and launch

```
docker compose build
docker compose up -d openclaw-gateway
```

If build fails with `Killed` or `exit code 137` during `pnpm install --frozen-lockfile`, the VM is out of memory.
Use a larger machine class before retrying.Verify binaries:

```
docker compose exec openclaw-gateway which gog
docker compose exec openclaw-gateway which goplaces
docker compose exec openclaw-gateway which wacli
```

Expected output:

```
/usr/local/bin/gog
/usr/local/bin/goplaces
/usr/local/bin/wacli
```

Verify Gateway:

```
docker compose logs -f openclaw-gateway
```

Expected output:

```
[gateway] listening on ws://0.0.0.0:18789
```

## What persists where

OpenClaw runs in Docker, but Docker is not the source of truth.
All long-lived state must survive restarts, rebuilds, and reboots.

| Component | Location | Persistence mechanism | Notes |
| --- | --- | --- | --- |
| Gateway config | `/home/node/.openclaw/` | Host volume mount | Includes `openclaw.json`, `.env` |
| Model auth profiles | `/home/node/.openclaw/agents/` | Host volume mount | `agents/<agentId>/agent/auth-profiles.json` (OAuth, API keys) |
| Skill configs | `/home/node/.openclaw/skills/` | Host volume mount | Skill-level state |
| Agent workspace | `/home/node/.openclaw/workspace/` | Host volume mount | Code and agent artifacts |
| WhatsApp session | `/home/node/.openclaw/` | Host volume mount | Preserves QR login |
| Gmail keyring | `/home/node/.openclaw/` | Host volume + password | Requires `GOG_KEYRING_PASSWORD` |
| Plugin runtime deps | `/var/lib/openclaw/plugin-runtime-deps/` | Docker named volume | Generated bundled plugin deps and runtime mirrors |
| External binaries | `/usr/local/bin/` | Docker image | Must be baked at build time |
| Node runtime | Container filesystem | Docker image | Rebuilt every image build |
| OS packages | Container filesystem | Docker image | Do not install at runtime |
| Docker container | Ephemeral | Restartable | Safe to destroy |

## Updates

To update OpenClaw on the VM:

```
git pull
docker compose build
docker compose up -d
```

## Related

- [Docker](https://docs.openclaw.ai/install/docker)
- [Podman](https://docs.openclaw.ai/install/podman)
- [ClawDock](https://docs.openclaw.ai/install/clawdock)

[DigitalOcean](https://docs.openclaw.ai/install/digitalocean) [exe.dev](https://docs.openclaw.ai/install/exe-dev)

Ctrl+I


---

## Docker VM runtime - OpenClaw

_Source: <https://docs.openclaw.ai/install/docker-vm-runtime#build-and-launch>_

# Example binary 1: Gmail CLI (gogcli — installs as `gog`)
# Copy the current Linux asset URL from https://github.com/steipete/gogcli/releases
RUN curl -L https://github.com/steipete/gogcli/releases/latest/download/gogcli_linux_amd64.tar.gz \
  | tar -xzO gog > /usr/local/bin/gog; \
  chmod +x /usr/local/bin/gog

# Example binary 2: Google Places CLI
# Copy the current Linux asset URL from https://github.com/steipete/goplaces/releases
RUN curl -L https://github.com/steipete/goplaces/releases/latest/download/goplaces_linux_amd64.tar.gz \
  | tar -xzO goplaces > /usr/local/bin/goplaces; \
  chmod +x /usr/local/bin/goplaces

# Example binary 3: WhatsApp CLI
# Copy the current Linux asset URL from https://github.com/steipete/wacli/releases
RUN curl -L https://github.com/steipete/wacli/releases/latest/download/wacli-linux-amd64.tar.gz \
  | tar -xzO wacli > /usr/local/bin/wacli; \
  chmod +x /usr/local/bin/wacli

# Add more binaries below using the same pattern

WORKDIR /app
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml .npmrc ./
COPY ui/package.json ./ui/package.json
COPY scripts ./scripts

RUN corepack enable
RUN pnpm install --frozen-lockfile

COPY . .
RUN pnpm build
RUN pnpm ui:install
RUN pnpm ui:build

ENV NODE_ENV=production

CMD ["node","dist/index.js"]
```

The URLs above are examples. For ARM-based VMs, choose the `arm64` assets. For reproducible builds, pin versioned release URLs.

## Build and launch

```
docker compose build
docker compose up -d openclaw-gateway
```

If build fails with `Killed` or `exit code 137` during `pnpm install --frozen-lockfile`, the VM is out of memory.
Use a larger machine class before retrying.Verify binaries:

```
docker compose exec openclaw-gateway which gog
docker compose exec openclaw-gateway which goplaces
docker compose exec openclaw-gateway which wacli
```

Expected output:

```
/usr/local/bin/gog
/usr/local/bin/goplaces
/usr/local/bin/wacli
```

Verify Gateway:

```
docker compose logs -f openclaw-gateway
```

Expected output:

```
[gateway] listening on ws://0.0.0.0:18789
```

## What persists where

OpenClaw runs in Docker, but Docker is not the source of truth.
All long-lived state must survive restarts, rebuilds, and reboots.

| Component | Location | Persistence mechanism | Notes |
| --- | --- | --- | --- |
| Gateway config | `/home/node/.openclaw/` | Host volume mount | Includes `openclaw.json`, `.env` |
| Model auth profiles | `/home/node/.openclaw/agents/` | Host volume mount | `agents/<agentId>/agent/auth-profiles.json` (OAuth, API keys) |
| Skill configs | `/home/node/.openclaw/skills/` | Host volume mount | Skill-level state |
| Agent workspace | `/home/node/.openclaw/workspace/` | Host volume mount | Code and agent artifacts |
| WhatsApp session | `/home/node/.openclaw/` | Host volume mount | Preserves QR login |
| Gmail keyring | `/home/node/.openclaw/` | Host volume + password | Requires `GOG_KEYRING_PASSWORD` |
| Plugin runtime deps | `/var/lib/openclaw/plugin-runtime-deps/` | Docker named volume | Generated bundled plugin deps and runtime mirrors |
| External binaries | `/usr/local/bin/` | Docker image | Must be baked at build time |
| Node runtime | Container filesystem | Docker image | Rebuilt every image build |
| OS packages | Container filesystem | Docker image | Do not install at runtime |
| Docker container | Ephemeral | Restartable | Safe to destroy |

## Updates

To update OpenClaw on the VM:

```
git pull
docker compose build
docker compose up -d
```

## Related

- [Docker](https://docs.openclaw.ai/install/docker)
- [Podman](https://docs.openclaw.ai/install/podman)
- [ClawDock](https://docs.openclaw.ai/install/clawdock)

[DigitalOcean](https://docs.openclaw.ai/install/digitalocean) [exe.dev](https://docs.openclaw.ai/install/exe-dev)

Ctrl+I


---

## WhatsApp (QR)

_Source: <https://docs.openclaw.ai/install/docker.md>_

# WhatsApp (QR)
 docker compose run --rm openclaw-cli channels login

 # Telegram
 docker compose run --rm openclaw-cli channels add --channel telegram --token ""

 # Discord
 docker compose run --rm openclaw-cli channels add --channel discord --token ""
 \`\`\`

 Docs: \[WhatsApp\](/channels/whatsapp), \[Telegram\](/channels/telegram), \[Discord\](/channels/discord)

\### Manual flow

If you prefer to run each step yourself instead of using the setup script:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
docker build -t openclaw:local -f Dockerfile .
docker compose run --rm --no-deps --entrypoint node openclaw-gateway \
 dist/index.js onboard --mode local --no-install-daemon
docker compose run --rm --no-deps --entrypoint node openclaw-gateway \
 dist/index.js config set --batch-json '\[{"path":"gateway.mode","value":"local"},{"path":"gateway.bind","value":"lan"},{"path":"gateway.controlUi.allowedOrigins","value":\["http://localhost:18789","http://127.0.0.1:18789"\]}\]'
docker compose up -d openclaw-gateway
\`\`\`

 Run \`docker compose\` from the repo root. If you enabled \`OPENCLAW\_EXTRA\_MOUNTS\`
 or \`OPENCLAW\_HOME\_VOLUME\`, the setup script writes \`docker-compose.extra.yml\`;
 include it with \`-f docker-compose.yml -f docker-compose.extra.yml\`.

 Because \`openclaw-cli\` shares \`openclaw-gateway\`'s network namespace, it is a
 post-start tool. Before \`docker compose up -d openclaw-gateway\`, run onboarding
 and setup-time config writes through \`openclaw-gateway\` with
 \`--no-deps --entrypoint node\`.

\### Environment variables

The setup script accepts these optional environment variables:

\| Variable \| Purpose \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`OPENCLAW\_IMAGE\` \| Use a remote image instead of building locally \|
\| \`OPENCLAW\_DOCKER\_APT\_PACKAGES\` \| Install extra apt packages during build (space-separated) \|
\| \`OPENCLAW\_EXTENSIONS\` \| Include selected bundled plugin helpers at build time \|
\| \`OPENCLAW\_EXTRA\_MOUNTS\` \| Extra host bind mounts (comma-separated \`source:target\[:opts\]\`) \|
\| \`OPENCLAW\_HOME\_VOLUME\` \| Persist \`/home/node\` in a named Docker volume \|
\| \`OPENCLAW\_SANDBOX\` \| Opt in to sandbox bootstrap (\`1\`, \`true\`, \`yes\`, \`on\`) \|
\| \`OPENCLAW\_SKIP\_ONBOARDING\` \| Skip the interactive onboarding step (\`1\`, \`true\`, \`yes\`, \`on\`) \|
\| \`OPENCLAW\_DOCKER\_SOCKET\` \| Override Docker socket path \|
\| \`OPENCLAW\_DISABLE\_BONJOUR\` \| Disable Bonjour/mDNS advertising (defaults to \`1\` for Docker) \|
\| \`OPENCLAW\_DISABLE\_BUNDLED\_SOURCE\_OVERLAYS\` \| Disable bundled plugin source bind-mount overlays \|
\| \`OTEL\_EXPORTER\_OTLP\_ENDPOINT\` \| Shared OTLP/HTTP collector endpoint for OpenTelemetry export \|
\| \`OTEL\_EXPORTER\_OTLP\_\*\_ENDPOINT\` \| Signal-specific OTLP endpoints for traces, metrics, or logs \|
\| \`OTEL\_EXPORTER\_OTLP\_PROTOCOL\` \| OTLP protocol override. Only \`http/protobuf\` is supported today \|
\| \`OTEL\_SERVICE\_NAME\` \| Service name used for OpenTelemetry resources \|
\| \`OTEL\_SEMCONV\_STABILITY\_OPT\_IN\` \| Opt in to latest experimental GenAI semantic attributes \|
\| \`OPENCLAW\_OTEL\_PRELOADED\` \| Skip starting a second OpenTelemetry SDK when one is preloaded \|

Maintainers can test bundled plugin source against a packaged image by mounting
one plugin source directory over its packaged source path, for example
\`OPENCLAW\_EXTRA\_MOUNTS=/path/to/fork/extensions/synology-chat:/app/extensions/synology-chat:ro\`.
That mounted source directory overrides the matching compiled
\`/app/dist/extensions/synology-chat\` bundle for the same plugin id.

\### Observability

OpenTelemetry export is outbound from the Gateway container to your OTLP
collector. It does not require a published Docker port. If you build the

_… [truncated; see https://docs.openclaw.ai/install/docker.md for full content]_


---

## exe.dev - OpenClaw

_Source: <https://docs.openclaw.ai/install/exe-dev>_

# WebSocket support
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        # Standard proxy headers
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Timeout settings for long-lived connections
        proxy_read_timeout 86400s;
        proxy_send_timeout 86400s;
    }
}
```

Overwrite forwarding headers instead of preserving client-supplied chains.
OpenClaw trusts forwarded IP metadata only from explicitly configured proxies,
and append-style `X-Forwarded-For` chains are treated as a hardening risk.

## 5) Access OpenClaw and grant privileges

Access `https://<vm-name>.exe.xyz/` (see the Control UI output from onboarding). If it prompts for auth, paste the
configured shared secret from the VM. This guide uses token auth, so retrieve `gateway.auth.token`
with `openclaw config get gateway.auth.token` (or generate one with `openclaw doctor --generate-gateway-token`).
If you changed the gateway to password auth, use `gateway.auth.password` / `OPENCLAW_GATEWAY_PASSWORD` instead.
Approve devices with `openclaw devices list` and `openclaw devices approve <requestId>`. When in doubt, use Shelley from your browser!

## Remote channel setup

For remote hosts, prefer one `config patch` call over many SSH calls to `config set`. Keep real tokens in the VM environment or `~/.openclaw/.env`, and put only SecretRefs in `openclaw.json`.On the VM, make the service environment contain the secrets it needs:

```
cat >> ~/.openclaw/.env <<'EOF'
SLACK_BOT_TOKEN=xoxb-...
SLACK_APP_TOKEN=xapp-...
DISCORD_BOT_TOKEN=...
OPENAI_API_KEY=sk-...
EOF
```

From your local machine, create a patch file and pipe it to the VM:

```
// openclaw.remote.patch.json5
{
  secrets: {
    providers: {
      default: { source: "env" },
    },
  },
  channels: {
    slack: {
      enabled: true,
      mode: "socket",
      botToken: { source: "env", provider: "default", id: "SLACK_BOT_TOKEN" },
      appToken: { source: "env", provider: "default", id: "SLACK_APP_TOKEN" },
      groupPolicy: "open",
      requireMention: false,
    },
    discord: {
      enabled: true,
      token: { source: "env", provider: "default", id: "DISCORD_BOT_TOKEN" },
      dmPolicy: "disabled",
      dm: { enabled: false },
      groupPolicy: "allowlist",
    },
  },
  agents: {
    defaults: {
      model: { primary: "openai/gpt-5.5" },
      models: {
        "openai/gpt-5.5": { params: { fastMode: true } },
      },
    },
  },
}
```

```
ssh <vm-name>.exe.xyz 'openclaw config patch --stdin --dry-run' < ./openclaw.remote.patch.json5
ssh <vm-name>.exe.xyz 'openclaw config patch --stdin' < ./openclaw.remote.patch.json5
ssh <vm-name>.exe.xyz 'openclaw gateway restart && openclaw health'
```

Use `--replace-path` when a nested allowlist should become exactly the patch value, for example when replacing a Discord channel allowlist:

```
ssh <vm-name>.exe.xyz 'openclaw config patch --stdin --replace-path "channels.discord.guilds[\"123\"].channels"' < ./discord.patch.json5
```

## Remote access

Remote access is handled by [exe.dev](https://exe.dev/)’s authentication. By
default, HTTP traffic from port 8000 is forwarded to `https://<vm-name>.exe.xyz`
with email auth.

## Updating

```
npm i -g openclaw@latest
openclaw doctor
openclaw gateway restart
openclaw health
```

Guide: [Updating](https://docs.openclaw.ai/install/updating)

## Related

- [Remote gateway](https://docs.openclaw.ai/gateway/remote)
- [Install overview](https://docs.openclaw.ai/install)

[Docker VM runtime](https://docs.openclaw.ai/install/docker-vm-runtime) [Fly.io](https://docs.openclaw.ai/install/fly)

Ctrl+I


---

## GCP - OpenClaw

_Source: <https://docs.openclaw.ai/install/gcp>_

# OpenClaw on GCP Compute Engine (Docker, Production VPS Guide)

## Goal

Run a persistent OpenClaw Gateway on a GCP Compute Engine VM using Docker, with durable state, baked-in binaries, and safe restart behavior.If you want “OpenClaw 24/7 for ~$5-12/mo”, this is a reliable setup on Google Cloud.
Pricing varies by machine type and region; pick the smallest VM that fits your workload and scale up if you hit OOMs.

## What are we doing (simple terms)?

- Create a GCP project and enable billing
- Create a Compute Engine VM
- Install Docker (isolated app runtime)
- Start the OpenClaw Gateway in Docker
- Persist `~/.openclaw` \+ `~/.openclaw/workspace` on the host (survives restarts/rebuilds)
- Access the Control UI from your laptop via an SSH tunnel

That mounted `~/.openclaw` state includes `openclaw.json`, per-agent
`agents/<agentId>/agent/auth-profiles.json`, and `.env`.The Gateway can be accessed via:

- SSH port forwarding from your laptop
- Direct port exposure if you manage firewalling and tokens yourself

This guide uses Debian on GCP Compute Engine.
Ubuntu also works; map packages accordingly.
For the generic Docker flow, see [Docker](https://docs.openclaw.ai/install/docker).

* * *

## Quick path (experienced operators)

1. Create GCP project + enable Compute Engine API
2. Create Compute Engine VM (e2-small, Debian 12, 20GB)
3. SSH into the VM
4. Install Docker
5. Clone OpenClaw repository
6. Create persistent host directories
7. Configure `.env` and `docker-compose.yml`
8. Bake required binaries, build, and launch

* * *

## What you need

- GCP account (free tier eligible for e2-micro)
- gcloud CLI installed (or use Cloud Console)
- SSH access from your laptop
- Basic comfort with SSH + copy/paste
- ~20-30 minutes
- Docker and Docker Compose
- Model auth credentials
- Optional provider credentials
  - WhatsApp QR
  - Telegram bot token
  - Gmail OAuth

* * *

1

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Install gcloud CLI (or use Console)

**Option A: gcloud CLI** (recommended for automation)Install from [https://cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install)Initialize and authenticate:

```
gcloud init
gcloud auth login
```

**Option B: Cloud Console**All steps can be done via the web UI at [https://console.cloud.google.com](https://console.cloud.google.com/)

2

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Create a GCP project

**CLI:**

```
gcloud projects create my-openclaw-project --name="OpenClaw Gateway"
gcloud config set project my-openclaw-project
```

Enable billing at [https://console.cloud.google.com/billing](https://console.cloud.google.com/billing) (required for Compute Engine).Enable the Compute Engine API:

```
gcloud services enable compute.googleapis.com
```

**Console:**

1. Go to IAM & Admin > Create Project
2. Name it and create
3. Enable billing for the project
4. Navigate to APIs & Services > Enable APIs > search “Compute Engine API” > Enable

3

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

Create the VM

**Machine types:**

| Type | Specs | Cost | Notes |
| --- | --- | --- | --- |
| e2-medium | 2 vCPU, 4GB RAM | ~$25/mo | Most reliable for local Docker builds |
| e2-small | 2 vCPU, 2GB RAM | ~$12/mo | Minimum recommended for Docker build |
| e2-micro | 2 vCPU (shared), 1GB RAM | Free tier eligible | Often fails with Docker build OOM (exit 137) |

**CLI:**

```
gcloud compute instances create openclaw-gateway \
  --zone=us-central1-a \
  --machine-type=e2-small \
  --boot-disk-size=20GB \
  --image-family=debian-12 \
  --image-project=debian-cloud
```

**Console:**

1. Go to Compute Engine > VM instances > Create instance
2. Name: `openclaw-gateway`
3. Region: `us-central1`, Zone: `us-central1-a`
4. Machine type: `e2-small`
5. Boot disk: Debian 12, 20GB
6. Create

4

[Navigate to header](https://docs.openclaw.ai/install/gcp#)

SSH into the VM

**CLI:**

```
gcloud compute ssh openclaw-gateway --zone=us-central1-a
`

_… [truncated; see https://docs.openclaw.ai/install/gcp for full content]_


---

## Hetzner - OpenClaw

_Source: <https://docs.openclaw.ai/install/hetzner>_

# OpenClaw on Hetzner (Docker, Production VPS Guide)

## Goal

Run a persistent OpenClaw Gateway on a Hetzner VPS using Docker, with durable state, baked-in binaries, and safe restart behavior.If you want “OpenClaw 24/7 for ~$5”, this is the simplest reliable setup.
Hetzner pricing changes; pick the smallest Debian/Ubuntu VPS and scale up if you hit OOMs.Security model reminder:

- Company-shared agents are fine when everyone is in the same trust boundary and the runtime is business-only.
- Keep strict separation: dedicated VPS/runtime + dedicated accounts; no personal Apple/Google/browser/password-manager profiles on that host.
- If users are adversarial to each other, split by gateway/host/OS user.

See [Security](https://docs.openclaw.ai/gateway/security) and [VPS hosting](https://docs.openclaw.ai/vps).

## What are we doing (simple terms)?

- Rent a small Linux server (Hetzner VPS)
- Install Docker (isolated app runtime)
- Start the OpenClaw Gateway in Docker
- Persist `~/.openclaw` \+ `~/.openclaw/workspace` on the host (survives restarts/rebuilds)
- Access the Control UI from your laptop via an SSH tunnel

That mounted `~/.openclaw` state includes `openclaw.json`, per-agent
`agents/<agentId>/agent/auth-profiles.json`, and `.env`.The Gateway can be accessed via:

- SSH port forwarding from your laptop
- Direct port exposure if you manage firewalling and tokens yourself

This guide assumes Ubuntu or Debian on Hetzner.

If you are on another Linux VPS, map packages accordingly.
For the generic Docker flow, see [Docker](https://docs.openclaw.ai/install/docker).

* * *

## Quick path (experienced operators)

1. Provision Hetzner VPS
2. Install Docker
3. Clone OpenClaw repository
4. Create persistent host directories
5. Configure `.env` and `docker-compose.yml`
6. Bake required binaries into the image
7. `docker compose up -d`
8. Verify persistence and Gateway access

* * *

## What you need

- Hetzner VPS with root access
- SSH access from your laptop
- Basic comfort with SSH + copy/paste
- ~20 minutes
- Docker and Docker Compose
- Model auth credentials
- Optional provider credentials
  - WhatsApp QR
  - Telegram bot token
  - Gmail OAuth

* * *

1

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Provision the VPS

Create an Ubuntu or Debian VPS in Hetzner.Connect as root:

```
ssh root@YOUR_VPS_IP
```

This guide assumes the VPS is stateful.
Do not treat it as disposable infrastructure.

2

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Install Docker (on the VPS)

```
apt-get update
apt-get install -y git curl ca-certificates
curl -fsSL https://get.docker.com | sh
```

Verify:

```
docker --version
docker compose version
```

3

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Clone the OpenClaw repository

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
```

This guide assumes you will build a custom image to guarantee binary persistence.

4

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Create persistent host directories

Docker containers are ephemeral.
All long-lived state must live on the host.

```
mkdir -p /root/.openclaw/workspace

# Set ownership to the container user (uid 1000):
chown -R 1000:1000 /root/.openclaw
```

5

[Navigate to header](https://docs.openclaw.ai/install/hetzner#)

Configure environment variables

Create `.env` in the repository root.

```
OPENCLAW_IMAGE=openclaw:latest
OPENCLAW_GATEWAY_TOKEN=
OPENCLAW_GATEWAY_BIND=lan
OPENCLAW_GATEWAY_PORT=18789

OPENCLAW_CONFIG_DIR=/root/.openclaw
OPENCLAW_WORKSPACE_DIR=/root/.openclaw/workspace

GOG_KEYRING_PASSWORD=
XDG_CONFIG_HOME=/home/node/.openclaw
```

Leave `OPENCLAW_GATEWAY_TOKEN` blank unless you explicitly want to
manage it through `.env`; OpenClaw writes a random gateway token to
config on first start. Generate a keyring password and paste it into
`GOG_KEYRING_PASSWORD`:

```
openssl rand -hex 32
```

**Do not commit this file.**This `.env` file is for contai

_… [truncated; see https://docs.openclaw.ai/install/hetzner for full content]_


---

## Hostinger - OpenClaw

_Source: <https://docs.openclaw.ai/install/hostinger>_

[OpenClaw home page](https://docs.openclaw.ai/)

Hosting

Hostinger

Run a persistent OpenClaw Gateway on [Hostinger](https://www.hostinger.com/openclaw) via a **1-Click** managed deployment or a **VPS** install.

## Prerequisites

- Hostinger account ( [signup](https://www.hostinger.com/openclaw))
- About 5-10 minutes

## Option A: 1-Click OpenClaw

The fastest way to get started. Hostinger handles infrastructure, Docker, and automatic updates.

1

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Purchase and launch

1. From the [Hostinger OpenClaw page](https://www.hostinger.com/openclaw), choose a Managed OpenClaw plan and complete checkout.

During checkout you can select **Ready-to-Use AI** credits that are pre-purchased and integrated instantly inside OpenClaw — no external accounts or API keys from other providers needed. You can start chatting right away. Alternatively, provide your own key from Anthropic, OpenAI, Google Gemini, or xAI during setup.

2

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Select a messaging channel

Choose one or more channels to connect:

- **WhatsApp** — scan the QR code shown in the setup wizard.
- **Telegram** — paste the bot token from [BotFather](https://t.me/BotFather).

3

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Complete installation

Click **Finish** to deploy the instance. Once ready, access the OpenClaw dashboard from **OpenClaw Overview** in hPanel.

## Option B: OpenClaw on VPS

More control over your server. Hostinger deploys OpenClaw via Docker on your VPS and you manage it through the **Docker Manager** in hPanel.

1

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Purchase a VPS

1. From the [Hostinger OpenClaw page](https://www.hostinger.com/openclaw), choose an OpenClaw on VPS plan and complete checkout.

You can select **Ready-to-Use AI** credits during checkout — these are pre-purchased and integrated instantly inside OpenClaw, so you can start chatting without any external accounts or API keys from other providers.

2

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Configure OpenClaw

Once the VPS is provisioned, fill in the configuration fields:

- **Gateway token** — auto-generated; save it for later use.
- **WhatsApp number** — your number with country code (optional).
- **Telegram bot token** — from [BotFather](https://t.me/BotFather) (optional).
- **API keys** — only needed if you did not select Ready-to-Use AI credits during checkout.

3

[Navigate to header](https://docs.openclaw.ai/install/hostinger#)

Start OpenClaw

Click **Deploy**. Once running, open the OpenClaw dashboard from the hPanel by clicking on **Open**.

Logs, restarts, and updates are managed directly from the Docker Manager interface in hPanel. To update, press on **Update** in Docker Manager and that will pull the latest image.

## Verify your setup

Send “Hi” to your assistant on the channel you connected. OpenClaw will reply and walk you through initial preferences.

## Troubleshooting

**Dashboard not loading** — Wait a few minutes for the container to finish provisioning. Check the Docker Manager logs in hPanel.**Docker container keeps restarting** — Open Docker Manager logs and look for configuration errors (missing tokens, invalid API keys).**Telegram bot not responding** — Send your pairing code message from Telegram directly as a message inside your OpenClaw chat to complete the connection.

## Next steps

- [Channels](https://docs.openclaw.ai/channels) — connect Telegram, WhatsApp, Discord, and more
- [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) — all config options

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [VPS hosting](https://docs.openclaw.ai/vps)
- [DigitalOcean](https://docs.openclaw.ai/install/digitalocean)

[Hetzner](https://docs.openclaw.ai/install/hetzner) [Kubernetes](https://docs.openclaw.ai/install/kubernetes)

Ctrl+I


---

## https://docs.openclaw.ai/install/index.md

_Source: <https://docs.openclaw.ai/install/index.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Install

\## System requirements

\\* \*\*Node 24\*\* (recommended) or Node 22.14+ — the installer script handles this automatically
\\* \*\*macOS, Linux, or Windows\*\* — both native Windows and WSL2 are supported; WSL2 is more stable. See \[Windows\](/platforms/windows).
\\* \`pnpm\` is only needed if you build from source

\## Recommended: installer script

The fastest way to install. It detects your OS, installs Node if needed, installs OpenClaw, and launches onboarding.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 curl -fsSL https://openclaw.ai/install.sh \| bash
 \`\`\`

 \`\`\`powershell theme={"theme":{"light":"min-light","dark":"min-dark"}}
 iwr -useb https://openclaw.ai/install.ps1 \| iex
 \`\`\`

To install without running onboarding:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 curl -fsSL https://openclaw.ai/install.sh \| bash -s -- --no-onboard
 \`\`\`

 \`\`\`powershell theme={"theme":{"light":"min-light","dark":"min-dark"}}
 & (\[scriptblock\]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
 \`\`\`

For all flags and CI/automation options, see \[Installer internals\](/install/installer).

\## Alternative install methods

\### Local prefix installer (\`install-cli.sh\`)

Use this when you want OpenClaw and Node kept under a local prefix such as
\`~/.openclaw\`, without depending on a system-wide Node install:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
curl -fsSL https://openclaw.ai/install-cli.sh \| bash
\`\`\`

It supports npm installs by default, plus git-checkout installs under the same
prefix flow. Full reference: \[Installer internals\](/install/installer#install-clish).

Already installed? Switch between package and git installs with
\`openclaw update --channel dev\` and \`openclaw update --channel stable\`. See
\[Updating\](/install/updating#switch-between-npm-and-git-installs).

\### npm, pnpm, or bun

If you already manage Node yourself:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 npm install -g openclaw@latest
 openclaw onboard --install-daemon
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 pnpm add -g openclaw@latest
 pnpm approve-builds -g
 openclaw onboard --install-daemon
 \`\`\`

 pnpm requires explicit approval for packages with build scripts. Run \`pnpm approve-builds -g\` after the first install.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 bun add -g openclaw@latest
 openclaw onboard --install-daemon
 \`\`\`

 Bun is supported for the global CLI install path. For the Gateway runtime, Node remains the recommended daemon runtime.

 If \`sharp\` fails due to a globally installed libvips:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 SHARP\_IGNORE\_GLOBAL\_LIBVIPS=1 npm install -g openclaw@latest
 \`\`\`

\### From source

For contributors or anyone who wants to run from a local checkout:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install && pnpm build && pnpm ui:build
pnpm link --global
openclaw onboard --install-daemon
\`\`\`

Or skip the link and use \`pnpm openclaw ...\` from inside the repo. See \[Setup\](/start/setup) for full development workflows.

\### Install from GitHub main

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
npm install -g github:openclaw/openclaw#main
\`\`\`

\### Containers and package managers

 Containerized or headless deployments.

 Rootless container alternative to Docker.

 Declarative install via Nix flake.

 Automated fleet provisioning.

 CLI-only usage via the Bun runtime.

\## Verify the install

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw --version # confirm the CLI

_… [truncated; see https://docs.openclaw.ai/install/index.md for full content]_


---

## Installer internals - OpenClaw

_Source: <https://docs.openclaw.ai/install/installer>_

# install.ps1 has no dedicated -Verbose flag yet.
Set-PSDebug -Trace 1
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
Set-PSDebug -Trace 0
```

Flags reference

| Flag | Description |
| --- | --- |
| `-InstallMethod npm|git` | Install method (default: `npm`) |
| `-Tag <tag|version|spec>` | npm dist-tag, version, or package spec (default: `latest`) |
| `-GitDir <path>` | Checkout directory (default: `%USERPROFILE%\openclaw`) |
| `-NoOnboard` | Skip onboarding |
| `-NoGitUpdate` | Skip `git pull` |
| `-DryRun` | Print actions only |

Environment variables reference

| Variable | Description |
| --- | --- |
| `OPENCLAW_INSTALL_METHOD=git|npm` | Install method |
| `OPENCLAW_GIT_DIR=<path>` | Checkout directory |
| `OPENCLAW_NO_ONBOARD=1` | Skip onboarding |
| `OPENCLAW_GIT_UPDATE=0` | Disable git pull |
| `OPENCLAW_DRY_RUN=1` | Dry run mode |

If `-InstallMethod git` is used and Git is missing, the script exits and prints the Git for Windows link.

* * *

## CI and automation

Use non-interactive flags/env vars for predictable runs.

- install.sh (non-interactive npm)

- install.sh (non-interactive git)

- install-cli.sh (JSON)

- install.ps1 (skip onboarding)

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --no-prompt --no-onboard
```

```
OPENCLAW_INSTALL_METHOD=git OPENCLAW_NO_PROMPT=1 \
  curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash -s -- --json --prefix /opt/openclaw
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
```

* * *

## Troubleshooting

Why is Git required?

Git is required for `git` install method. For `npm` installs, Git is still checked/installed to avoid `spawn git ENOENT` failures when dependencies use git URLs.

Why does npm hit EACCES on Linux?

Some Linux setups point npm global prefix to root-owned paths. `install.sh` can switch prefix to `~/.npm-global` and append PATH exports to shell rc files (when those files exist).

sharp/libvips issues

The scripts default `SHARP_IGNORE_GLOBAL_LIBVIPS=1` to avoid sharp building against system libvips. To override:

```
SHARP_IGNORE_GLOBAL_LIBVIPS=0 curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash
```

Windows: "npm error spawn git / ENOENT"

Install Git for Windows, reopen PowerShell, rerun installer.

Windows: "openclaw is not recognized"

Run `npm config get prefix` and add that directory to your user PATH (no `\bin` suffix needed on Windows), then reopen PowerShell.

Windows: how to get verbose installer output

`install.ps1` does not currently expose a `-Verbose` switch.
Use PowerShell tracing for script-level diagnostics:

```
Set-PSDebug -Trace 1
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
Set-PSDebug -Trace 0
```

openclaw not found after install

Usually a PATH issue. See [Node.js troubleshooting](https://docs.openclaw.ai/install/node#troubleshooting).

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [Updating](https://docs.openclaw.ai/install/updating)
- [Uninstall](https://docs.openclaw.ai/install/uninstall)

[Install](https://docs.openclaw.ai/install) [Node.js](https://docs.openclaw.ai/install/node)

Ctrl+I


---

## Migration guide - OpenClaw

_Source: <https://docs.openclaw.ai/install/migrating>_

[OpenClaw home page](https://docs.openclaw.ai/)

Migrating

Migration guide

OpenClaw supports three migration paths: importing from another agent system, moving an existing install to a new machine, and upgrading a plugin in place.

## Import from another agent system

Use the bundled migration providers to bring instructions, MCP servers, skills, model config, and (opt-in) API keys into OpenClaw. Plans are previewed before any change, secrets are redacted in reports, and apply is backed by a verified backup.

[**Migrating from Claude** \\
\\
Import Claude Code and Claude Desktop state, including `CLAUDE.md`, MCP servers, skills, and project commands.](https://docs.openclaw.ai/install/migrating-claude)

[**Migrating from Hermes** \\
\\
Import Hermes config, providers, MCP servers, memory, skills, and supported `.env` keys.](https://docs.openclaw.ai/install/migrating-hermes)

The CLI entry point is [`openclaw migrate`](https://docs.openclaw.ai/cli/migrate). Onboarding can also offer migration when it detects a known source (`openclaw onboard --flow import`).

## Move OpenClaw to a new machine

Copy the **state directory** (`~/.openclaw/` by default) and your **workspace** to preserve:

- **Config** — `openclaw.json` and all gateway settings.
- **Auth** — per-agent `auth-profiles.json` (API keys plus OAuth), plus any channel or provider state under `credentials/`.
- **Sessions** — conversation history and agent state.
- **Channel state** — WhatsApp login, Telegram session, and similar.
- **Workspace files** — `MEMORY.md`, `USER.md`, skills, and prompts.

Run `openclaw status` on the old machine to confirm your state directory path. Custom profiles use `~/.openclaw-<profile>/` or a path set via `OPENCLAW_STATE_DIR`.

### Migration steps

1

[Navigate to header](https://docs.openclaw.ai/install/migrating#)

Stop the gateway and back up

On the **old** machine, stop the gateway so files are not changing mid-copy, then archive:

```
openclaw gateway stop
cd ~
tar -czf openclaw-state.tgz .openclaw
```

If you use multiple profiles (for example `~/.openclaw-work`), archive each separately.

2

[Navigate to header](https://docs.openclaw.ai/install/migrating#)

Install OpenClaw on the new machine

[Install](https://docs.openclaw.ai/install) the CLI (and Node if needed) on the new machine. It is fine if onboarding creates a fresh `~/.openclaw/`. You will overwrite it next.

3

[Navigate to header](https://docs.openclaw.ai/install/migrating#)

Copy state directory and workspace

Transfer the archive via `scp`, `rsync -a`, or an external drive, then extract:

```
cd ~
tar -xzf openclaw-state.tgz
```

Ensure hidden directories were included and file ownership matches the user that will run the gateway.

4

[Navigate to header](https://docs.openclaw.ai/install/migrating#)

Run doctor and verify

On the new machine, run [Doctor](https://docs.openclaw.ai/gateway/doctor) to apply config migrations and repair services:

```
openclaw doctor
openclaw gateway restart
openclaw status
```

If Telegram or Discord uses the default env fallback (`TELEGRAM_BOT_TOKEN` or `DISCORD_BOT_TOKEN`), verify the migrated state-dir `.env` contains those keys without printing the secret values:

```
awk -F= '/^(TELEGRAM_BOT_TOKEN|DISCORD_BOT_TOKEN)=/ { print $1 "=present" }' ~/.openclaw/.env
```

`openclaw doctor` also warns when an enabled default Telegram or Discord account has no configured token and the matching env variable is unavailable to the doctor process.

### Common pitfalls

Profile or state-dir mismatch

If the old gateway used `--profile` or `OPENCLAW_STATE_DIR` and the new one does not, channels will appear logged out and sessions will be empty. Launch the gateway with the **same** profile or state-dir you migrated, then rerun `openclaw doctor`.

Copying only openclaw.json

The config file alone is not enough. Model auth profiles live under `agents/<agentId>/agent/auth-profiles.json`, and channel and provider state lives under `credentials/`. Always

_… [truncated; see https://docs.openclaw.ai/install/migrating for full content]_


---

## Migrating from Hermes - OpenClaw

_Source: <https://docs.openclaw.ai/install/migrating-hermes>_

[OpenClaw home page](https://docs.openclaw.ai/)

Migrating

Migrating from Hermes

OpenClaw imports Hermes state through a bundled migration provider. The provider previews everything before changing state, redacts secrets in plans and reports, and creates a verified backup before apply.

Imports require a fresh OpenClaw setup. If you already have local OpenClaw state, reset config, credentials, sessions, and the workspace first, or use `openclaw migrate` directly with `--overwrite` after reviewing the plan.

## Two ways to import

- Onboarding wizard

- CLI

The fastest path. The wizard detects Hermes at `~/.hermes` and shows a preview before applying.

```
openclaw onboard --flow import
```

Or point at a specific source:

```
openclaw onboard --import-from hermes --import-source ~/.hermes
```

Use `openclaw migrate` for scripted or repeatable runs. See [`openclaw migrate`](https://docs.openclaw.ai/cli/migrate) for the full reference.

```
openclaw migrate hermes --dry-run    # preview only
openclaw migrate apply hermes --yes  # apply with confirmation skipped
```

Add `--from <path>` when Hermes lives outside `~/.hermes`.

## What gets imported

Model configuration

- Default model selection from Hermes `config.yaml`.
- Configured model providers and custom OpenAI-compatible endpoints from `providers` and `custom_providers`.

MCP servers

MCP server definitions from `mcp_servers` or `mcp.servers`.

Workspace files

- `SOUL.md` and `AGENTS.md` are copied into the OpenClaw agent workspace.
- `memories/MEMORY.md` and `memories/USER.md` are **appended** to the matching OpenClaw memory files instead of overwriting them.

Memory configuration

Memory config defaults for OpenClaw file memory. External memory providers such as Honcho are recorded as archive or manual-review items so you can move them deliberately.

Skills

Skills with a `SKILL.md` file under `skills/<name>/` are copied, along with per-skill config values from `skills.config`.

API keys (opt-in)

Set `--include-secrets` to import supported `.env` keys: `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, `OPENROUTER_API_KEY`, `GOOGLE_API_KEY`, `GEMINI_API_KEY`, `GROQ_API_KEY`, `XAI_API_KEY`, `MISTRAL_API_KEY`, `DEEPSEEK_API_KEY`. Without the flag, secrets are never copied.

## What stays archive-only

The provider copies these into the migration report directory for manual review, but does **not** load them into live OpenClaw config or credentials:

- `plugins/`
- `sessions/`
- `logs/`
- `cron/`
- `mcp-tokens/`
- `auth.json`
- `state.db`

OpenClaw refuses to execute or trust this state automatically because the formats and trust assumptions can drift between systems. Move what you need by hand after reviewing the archive.

## Recommended flow

1

[Navigate to header](https://docs.openclaw.ai/install/migrating-hermes#)

Preview the plan

```
openclaw migrate hermes --dry-run
```

The plan lists everything that will change, including conflicts, skipped items, and any sensitive items. Plan output redacts nested secret-looking keys.

2

[Navigate to header](https://docs.openclaw.ai/install/migrating-hermes#)

Apply with backup

```
openclaw migrate apply hermes --yes
```

OpenClaw creates and verifies a backup before applying. If you need API keys imported, add `--include-secrets`.

3

[Navigate to header](https://docs.openclaw.ai/install/migrating-hermes#)

Run doctor

```
openclaw doctor
```

[Doctor](https://docs.openclaw.ai/gateway/doctor) reapplies any pending config migrations and checks for issues introduced during the import.

4

[Navigate to header](https://docs.openclaw.ai/install/migrating-hermes#)

Restart and verify

```
openclaw gateway restart
openclaw status
```

Confirm the gateway is healthy and your imported model, memory, and skills are loaded.

## Conflict handling

Apply refuses to continue when the plan reports conflicts (a file or config value already exists at the target).

Rerun with `--overwrite` only when replacing the existing target is intentional. Provi

_… [truncated; see https://docs.openclaw.ai/install/migrating-hermes for full content]_


---

## https://docs.openclaw.ai/install/migrating.md

_Source: <https://docs.openclaw.ai/install/migrating.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Migration guide

OpenClaw supports three migration paths: importing from another agent system, moving an existing install to a new machine, and upgrading a plugin in place.

\## Import from another agent system

Use the bundled migration providers to bring instructions, MCP servers, skills, model config, and (opt-in) API keys into OpenClaw. Plans are previewed before any change, secrets are redacted in reports, and apply is backed by a verified backup.

 Import Claude Code and Claude Desktop state, including \`CLAUDE.md\`, MCP servers, skills, and project commands.

 Import Hermes config, providers, MCP servers, memory, skills, and supported \`.env\` keys.

The CLI entry point is \[\`openclaw migrate\`\](/cli/migrate). Onboarding can also offer migration when it detects a known source (\`openclaw onboard --flow import\`).

\## Move OpenClaw to a new machine

Copy the \*\*state directory\*\* (\`~/.openclaw/\` by default) and your \*\*workspace\*\* to preserve:

\\* \*\*Config\*\* — \`openclaw.json\` and all gateway settings.
\\* \*\*Auth\*\* — per-agent \`auth-profiles.json\` (API keys plus OAuth), plus any channel or provider state under \`credentials/\`.
\\* \*\*Sessions\*\* — conversation history and agent state.
\\* \*\*Channel state\*\* — WhatsApp login, Telegram session, and similar.
\\* \*\*Workspace files\*\* — \`MEMORY.md\`, \`USER.md\`, skills, and prompts.

 Run \`openclaw status\` on the old machine to confirm your state directory path. Custom profiles use \`~/.openclaw-/\` or a path set via \`OPENCLAW\_STATE\_DIR\`.

\### Migration steps

 On the \*\*old\*\* machine, stop the gateway so files are not changing mid-copy, then archive:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw gateway stop
 cd ~
 tar -czf openclaw-state.tgz .openclaw
 \`\`\`

 If you use multiple profiles (for example \`~/.openclaw-work\`), archive each separately.

 \[Install\](/install) the CLI (and Node if needed) on the new machine. It is fine if onboarding creates a fresh \`~/.openclaw/\`. You will overwrite it next.

 Transfer the archive via \`scp\`, \`rsync -a\`, or an external drive, then extract:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 cd ~
 tar -xzf openclaw-state.tgz
 \`\`\`

 Ensure hidden directories were included and file ownership matches the user that will run the gateway.

 On the new machine, run \[Doctor\](/gateway/doctor) to apply config migrations and repair services:

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw doctor
 openclaw gateway restart
 openclaw status
 \`\`\`

If Telegram or Discord uses the default env fallback (\`TELEGRAM\_BOT\_TOKEN\` or \`DISCORD\_BOT\_TOKEN\`), verify the migrated state-dir \`.env\` contains those keys without printing the secret values:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
awk -F= '/^(TELEGRAM\_BOT\_TOKEN\|DISCORD\_BOT\_TOKEN)=/ { print $1 "=present" }' ~/.openclaw/.env
\`\`\`

\`openclaw doctor\` also warns when an enabled default Telegram or Discord account has no configured token and the matching env variable is unavailable to the doctor process.

\### Common pitfalls

 If the old gateway used \`--profile\` or \`OPENCLAW\_STATE\_DIR\` and the new one does not, channels will appear logged out and sessions will be empty. Launch the gateway with the \*\*same\*\* profile or state-dir you migrated, then rerun \`openclaw doctor\`.

 The config file alone is not enough. Model auth profiles live under \`agents//agent/auth-profiles.json\`, and channel and provider state lives under \`credentials/\`. Always migrate the \*\*entire\*\* state directory.

 If you copied as root or switched users, the gateway may fail to read credentials. Ensure the state directory and workspace are owned by the user running the gateway.

_… [truncated; see https://docs.openclaw.ai/install/migrating.md for full content]_


---

## Node.js - OpenClaw

_Source: <https://docs.openclaw.ai/install/node>_

[OpenClaw home page](https://docs.openclaw.ai/)

Install overview

Node.js

OpenClaw requires **Node 22.14 or newer**. **Node 24 is the default and recommended runtime** for installs, CI, and release workflows. Node 22 remains supported via the active LTS line. The [installer script](https://docs.openclaw.ai/install#alternative-install-methods) will detect and install Node automatically — this page is for when you want to set up Node yourself and make sure everything is wired up correctly (versions, PATH, global installs).

## Check your version

```
node -v
```

If this prints `v24.x.x` or higher, you’re on the recommended default. If it prints `v22.14.x` or higher, you’re on the supported Node 22 LTS path, but we still recommend upgrading to Node 24 when convenient. If Node isn’t installed or the version is too old, pick an install method below.

## Install Node

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

## Troubleshooting

### `openclaw: command not found`

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

### Permission errors on `npm install -g` (Linux)

If you see `EACCES` errors, switch npm’s global prefix to a user-writable directory:

```
mkdir -p "$HOME/.npm-global"
npm config set prefix "$HOME/.npm-global"
export PATH="$HOME/.npm-global/bin:$PATH"
```

Add the `export PATH=...` line to your `~/.bashrc` or `~/.zshrc` to make it permanent.

## Related

- [Install Overview](https://docs.openclaw.ai/install) — all installation methods
- [Updating](https://docs.openclaw.ai/install/updating) — keeping OpenClaw up to date
- [Getting Started](https://docs.openclaw.ai/start/getting-started) — first steps after install

[Installer internals](https://docs.openclaw.ai/install/installer) [Updating](https://docs.openclaw.ai/install/updating)

Ctrl+I


---

## Oracle Cloud - OpenClaw

_Source: <https://docs.openclaw.ai/install/oracle>_

[OpenClaw home page](https://docs.openclaw.ai/)

Hosting

Oracle Cloud

Run a persistent OpenClaw Gateway on Oracle Cloud’s **Always Free** ARM tier (up to 4 OCPU, 24 GB RAM, 200 GB storage) at no cost.

## Prerequisites

- Oracle Cloud account ( [signup](https://www.oracle.com/cloud/free/)) — see [community signup guide](https://gist.github.com/rssnyder/51e3cfedd730e7dd5f4a816143b25dbd) if you hit issues
- Tailscale account (free at [tailscale.com](https://tailscale.com/))
- An SSH key pair
- About 30 minutes

## Setup

1

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Create an OCI instance

1. Log into [Oracle Cloud Console](https://cloud.oracle.com/).
2. Navigate to **Compute > Instances > Create Instance**.
3. Configure:
   - **Name:**`openclaw`
   - **Image:** Ubuntu 24.04 (aarch64)
   - **Shape:**`VM.Standard.A1.Flex` (Ampere ARM)
   - **OCPUs:** 2 (or up to 4)
   - **Memory:** 12 GB (or up to 24 GB)
   - **Boot volume:** 50 GB (up to 200 GB free)
   - **SSH key:** Add your public key
4. Click **Create** and note the public IP address.

If instance creation fails with “Out of capacity”, try a different availability domain or retry later. Free tier capacity is limited.

2

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Connect and update the system

```
ssh ubuntu@YOUR_PUBLIC_IP

sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential
```

`build-essential` is required for ARM compilation of some dependencies.

3

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Configure user and hostname

```
sudo hostnamectl set-hostname openclaw
sudo passwd ubuntu
sudo loginctl enable-linger ubuntu
```

Enabling linger keeps user services running after logout.

4

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Install Tailscale

```
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up --ssh --hostname=openclaw
```

From now on, connect via Tailscale: `ssh ubuntu@openclaw`.

5

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Install OpenClaw

```
curl -fsSL https://openclaw.ai/install.sh | bash
source ~/.bashrc
```

When prompted “How do you want to hatch your bot?”, select **Do this later**.

6

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Configure the gateway

Use token auth with Tailscale Serve for secure remote access.

```
openclaw config set gateway.bind loopback
openclaw config set gateway.auth.mode token
openclaw doctor --generate-gateway-token
openclaw config set gateway.tailscale.mode serve
openclaw config set gateway.trustedProxies '["127.0.0.1"]'

systemctl --user restart openclaw-gateway.service
```

`gateway.trustedProxies=["127.0.0.1"]` here is only for the local Tailscale Serve proxy’s forwarded-IP/local-client handling. It is **not**`gateway.auth.mode: "trusted-proxy"`. Diff viewer routes keep fail-closed behavior in this setup: raw `127.0.0.1` viewer requests without forwarded proxy headers can return `Diff not found`. Use `mode=file` / `mode=both` for attachments, or intentionally enable remote viewers and set `plugins.entries.diffs.config.viewerBaseUrl` (or pass a proxy `baseUrl`) if you need shareable viewer links.

7

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Lock down VCN security

Block all traffic except Tailscale at the network edge:

1. Go to **Networking > Virtual Cloud Networks** in the OCI Console.
2. Click your VCN, then **Security Lists > Default Security List**.
3. **Remove** all ingress rules except `0.0.0.0/0 UDP 41641` (Tailscale).
4. Keep default egress rules (allow all outbound).

This blocks SSH on port 22, HTTP, HTTPS, and everything else at the network edge. You can only connect via Tailscale from this point on.

8

[Navigate to header](https://docs.openclaw.ai/install/oracle#)

Verify

```
openclaw --version
systemctl --user status openclaw-gateway.service
tailscale serve status
curl http://localhost:18789
```

Access the Control UI from a

_… [truncated; see https://docs.openclaw.ai/install/oracle for full content]_


---

## Podman - OpenClaw

_Source: <https://docs.openclaw.ai/install/podman>_

[OpenClaw home page](https://docs.openclaw.ai/)

Containers

Podman

Run the OpenClaw Gateway in a rootless Podman container, managed by your current non-root user.The intended model is:

- Podman runs the gateway container.
- Your host `openclaw` CLI is the control plane.
- Persistent state lives on the host under `~/.openclaw` by default.
- Day-to-day management uses `openclaw --container <name> ...` instead of `sudo -u openclaw`, `podman exec`, or a separate service user.

## Prerequisites

- **Podman** in rootless mode
- **OpenClaw CLI** installed on the host
- **Optional:**`systemd --user` if you want Quadlet-managed auto-start
- **Optional:**`sudo` only if you want `loginctl enable-linger "$(whoami)"` for boot persistence on a headless host

## Quick start

1

[Navigate to header](https://docs.openclaw.ai/install/podman#)

One-time setup

From the repo root, run `./scripts/podman/setup.sh`.

2

[Navigate to header](https://docs.openclaw.ai/install/podman#)

Start the Gateway container

Start the container with `./scripts/run-openclaw-podman.sh launch`.

3

[Navigate to header](https://docs.openclaw.ai/install/podman#)

Run onboarding inside the container

Run `./scripts/run-openclaw-podman.sh launch setup`, then open `http://127.0.0.1:18789/`.

4

[Navigate to header](https://docs.openclaw.ai/install/podman#)

Manage the running container from the host CLI

Set `OPENCLAW_CONTAINER=openclaw`, then use normal `openclaw` commands from the host.

Setup details:

- `./scripts/podman/setup.sh` builds `openclaw:local` in your rootless Podman store by default, or uses `OPENCLAW_IMAGE` / `OPENCLAW_PODMAN_IMAGE` if you set one.
- It creates `~/.openclaw/openclaw.json` with `gateway.mode: "local"` if missing.
- It creates `~/.openclaw/.env` with `OPENCLAW_GATEWAY_TOKEN` if missing.
- For manual launches, the helper reads only a small allowlist of Podman-related keys from `~/.openclaw/.env` and passes explicit runtime env vars to the container; it does not hand the full env file to Podman.

Quadlet-managed setup:

```
./scripts/podman/setup.sh --quadlet
```

Quadlet is a Linux-only option because it depends on systemd user services.You can also set `OPENCLAW_PODMAN_QUADLET=1`.Optional build/setup env vars:

- `OPENCLAW_IMAGE` or `OPENCLAW_PODMAN_IMAGE` — use an existing/pulled image instead of building `openclaw:local`
- `OPENCLAW_DOCKER_APT_PACKAGES` — install extra apt packages during image build
- `OPENCLAW_EXTENSIONS` — pre-install plugin dependencies at build time
- `OPENCLAW_INSTALL_BROWSER` — pre-install Chromium and Xvfb for browser automation (set to `1` to enable)

Container start:

```
./scripts/run-openclaw-podman.sh launch
```

The script starts the container as your current uid/gid with `--userns=keep-id` and bind-mounts your OpenClaw state into the container.Onboarding:

```
./scripts/run-openclaw-podman.sh launch setup
```

Then open `http://127.0.0.1:18789/` and use the token from `~/.openclaw/.env`.Host CLI default:

```
export OPENCLAW_CONTAINER=openclaw
```

Then commands such as these will run inside that container automatically:

```
openclaw dashboard --no-open
openclaw gateway status --deep   # includes extra service scan
openclaw doctor
openclaw channels login
```

On macOS, Podman machine may make the browser appear non-local to the gateway.
If the Control UI reports device-auth errors after launch, use the Tailscale guidance in
[Podman + Tailscale](https://docs.openclaw.ai/install/podman#podman--tailscale).

## Podman + Tailscale

For HTTPS or remote browser access, follow the main Tailscale docs.Podman-specific note:

- Keep the Podman publish host at `127.0.0.1`.
- Prefer host-managed `tailscale serve` over `openclaw gateway --tailscale serve`.
- On macOS, if local browser device-auth context is unreliable, use Tailscale access instead of ad hoc local tunnel workarounds.

See:

- [Tailscale](https://docs.openclaw.ai/gateway/tailscale)
- [Control UI](https://docs.openclaw.ai/web/control-ui)

## Systemd (Q

_… [truncated; see https://docs.openclaw.ai/install/podman for full content]_


---

## Railway - OpenClaw

_Source: <https://docs.openclaw.ai/install/railway>_

# Railway

Deploy OpenClaw on Railway with a one-click template and access it through the web Control UI.
This is the easiest “no terminal on the server” path: Railway runs the Gateway for you.

## Quick checklist (new users)

1. Click **Deploy on Railway** (below).
2. Add a **Volume** mounted at `/data`.
3. Set the required **Variables** (at least `OPENCLAW_GATEWAY_PORT` and `OPENCLAW_GATEWAY_TOKEN`).
4. Enable **HTTP Proxy** on port `8080`.
5. Open `https://<your-railway-domain>/openclaw` and connect using the configured shared secret. This template uses `OPENCLAW_GATEWAY_TOKEN` by default; if you replace it with password auth, use that password instead.

## One-click deploy

[Deploy on Railway](https://railway.com/deploy/clawdbot-railway-template) After deploy, find your public URL in **Railway → your service → Settings → Domains**.Railway will either:

- give you a generated domain (often `https://<something>.up.railway.app`), or
- use your custom domain if you attached one.

Then open:

- `https://<your-railway-domain>/openclaw` — Control UI

## What you get

- Hosted OpenClaw Gateway + Control UI
- Persistent storage via Railway Volume (`/data`) so `openclaw.json`,
per-agent `auth-profiles.json`, channel/provider state, sessions, and
workspace survive redeploys

## Required Railway settings

### Public Networking

Enable **HTTP Proxy** for the service.

- Port: `8080`

### Volume (required)

Attach a volume mounted at:

- `/data`

### Variables

Set these variables on the service:

- `OPENCLAW_GATEWAY_PORT=8080` (required — must match the port in Public Networking)
- `OPENCLAW_GATEWAY_TOKEN` (required; treat as an admin secret)
- `OPENCLAW_STATE_DIR=/data/.openclaw` (recommended)
- `OPENCLAW_WORKSPACE_DIR=/data/workspace` (recommended)

## Connect a channel

Use the Control UI at `/openclaw` or run `openclaw onboard` via Railway’s shell for channel setup instructions:

- [Telegram](https://docs.openclaw.ai/channels/telegram) (fastest — just a bot token)
- [Discord](https://docs.openclaw.ai/channels/discord)
- [All channels](https://docs.openclaw.ai/channels)

## Backups & migration

Export your state, config, auth profiles, and workspace:

```
openclaw backup create
```

This creates a portable backup archive with OpenClaw state plus any configured
workspace. See [Backup](https://docs.openclaw.ai/cli/backup) for details.

## Next steps

- Set up messaging channels: [Channels](https://docs.openclaw.ai/channels)
- Configure the Gateway: [Gateway configuration](https://docs.openclaw.ai/gateway/configuration)
- Keep OpenClaw up to date: [Updating](https://docs.openclaw.ai/install/updating)

[Oracle Cloud](https://docs.openclaw.ai/install/oracle) [Raspberry Pi](https://docs.openclaw.ai/install/raspberry-pi)

Ctrl+I


---

## Raspberry Pi - OpenClaw

_Source: <https://docs.openclaw.ai/install/raspberry-pi>_

# Set timezone (important for cron and reminders)
sudo timedatectl set-timezone America/Chicago
```

4

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Install Node.js 24

```
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo -E bash -
sudo apt install -y nodejs
node --version
```

5

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Add swap (important for 2 GB or less)

```
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Reduce swappiness for low-RAM devices
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

6

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Install OpenClaw

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

7

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Run onboarding

```
openclaw onboard --install-daemon
```

Follow the wizard. API keys are recommended over OAuth for headless devices. Telegram is the easiest channel to start with.

8

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Verify

```
openclaw status
systemctl --user status openclaw-gateway.service
journalctl --user -u openclaw-gateway.service -f
```

9

[Navigate to header](https://docs.openclaw.ai/install/raspberry-pi#)

Access the Control UI

On your computer, get a dashboard URL from the Pi:

```
ssh user@gateway-host 'openclaw dashboard --no-open'
```

Then create an SSH tunnel in another terminal:

```
ssh -N -L 18789:127.0.0.1:18789 user@gateway-host
```

Open the printed URL in your local browser. For always-on remote access, see [Tailscale integration](https://docs.openclaw.ai/gateway/tailscale).

## Performance tips

**Use a USB SSD** — SD cards are slow and wear out. A USB SSD dramatically improves performance. See the [Pi USB boot guide](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#usb-mass-storage-boot).**Enable module compile cache** — Speeds up repeated CLI invocations on lower-power Pi hosts:

```
grep -q 'NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache' ~/.bashrc || cat >> ~/.bashrc <<'EOF' # pragma: allowlist secret
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1
EOF
source ~/.bashrc
```

**Reduce memory usage** — For headless setups, free GPU memory and disable unused services:

```
echo 'gpu_mem=16' | sudo tee -a /boot/config.txt
sudo systemctl disable bluetooth
```

## Troubleshooting

**Out of memory** — Verify swap is active with `free -h`. Disable unused services (`sudo systemctl disable cups bluetooth avahi-daemon`). Use API-based models only.**Slow performance** — Use a USB SSD instead of an SD card. Check for CPU throttling with `vcgencmd get_throttled` (should return `0x0`).**Service will not start** — Check logs with `journalctl --user -u openclaw-gateway.service --no-pager -n 100` and run `openclaw doctor --non-interactive`. If this is a headless Pi, also verify lingering is enabled: `sudo loginctl enable-linger "$(whoami)"`.**ARM binary issues** — If a skill fails with “exec format error”, check whether the binary has an ARM64 build. Verify architecture with `uname -m` (should show `aarch64`).**WiFi drops** — Disable WiFi power management: `sudo iwconfig wlan0 power off`.

## Next steps

- [Channels](https://docs.openclaw.ai/channels) — connect Telegram, WhatsApp, Discord, and more
- [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) — all config options
- [Updating](https://docs.openclaw.ai/install/updating) — keep OpenClaw up to date

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [Linux server](https://docs.openclaw.ai/vps)
- [Platforms](https://docs.openclaw.ai/platforms)

[Railway](https://docs.openclaw.ai/install/railway) [Render](https://docs.openclaw.ai/install/render)

Ctrl+I


---

## Uninstall - OpenClaw

_Source: <https://docs.openclaw.ai/install/uninstall>_

[OpenClaw home page](https://docs.openclaw.ai/)

Maintenance

Uninstall

Two paths:

- **Easy path** if `openclaw` is still installed.
- **Manual service removal** if the CLI is gone but the service is still running.

## Easy path (CLI still installed)

Recommended: use the built-in uninstaller:

```
openclaw uninstall
```

Non-interactive (automation / npx):

```
openclaw uninstall --all --yes --non-interactive
npx -y openclaw uninstall --all --yes --non-interactive
```

Manual steps (same result):

1. Stop the gateway service:

```
openclaw gateway stop
```

2. Uninstall the gateway service (launchd/systemd/schtasks):

```
openclaw gateway uninstall
```

3. Delete state + config:

```
rm -rf "${OPENCLAW_STATE_DIR:-$HOME/.openclaw}"
```

If you set `OPENCLAW_CONFIG_PATH` to a custom location outside the state dir, delete that file too.

4. Delete your workspace (optional, removes agent files):

```
rm -rf ~/.openclaw/workspace
```

5. Remove the CLI install (pick the one you used):

```
npm rm -g openclaw
pnpm remove -g openclaw
bun remove -g openclaw
```

6. If you installed the macOS app:

```
rm -rf /Applications/OpenClaw.app
```

Notes:

- If you used profiles (`--profile` / `OPENCLAW_PROFILE`), repeat step 3 for each state dir (defaults are `~/.openclaw-<profile>`).
- In remote mode, the state dir lives on the **gateway host**, so run steps 1-4 there too.

## Manual service removal (CLI not installed)

Use this if the gateway service keeps running but `openclaw` is missing.

### macOS (launchd)

Default label is `ai.openclaw.gateway` (or `ai.openclaw.<profile>`; legacy `com.openclaw.*` may still exist):

```
launchctl bootout gui/$UID/ai.openclaw.gateway
rm -f ~/Library/LaunchAgents/ai.openclaw.gateway.plist
```

If you used a profile, replace the label and plist name with `ai.openclaw.<profile>`. Remove any legacy `com.openclaw.*` plists if present.

### Linux (systemd user unit)

Default unit name is `openclaw-gateway.service` (or `openclaw-gateway-<profile>.service`):

```
systemctl --user disable --now openclaw-gateway.service
rm -f ~/.config/systemd/user/openclaw-gateway.service
systemctl --user daemon-reload
```

### Windows (Scheduled Task)

Default task name is `OpenClaw Gateway` (or `OpenClaw Gateway (<profile>)`).
The task script lives under your state dir.

```
schtasks /Delete /F /TN "OpenClaw Gateway"
Remove-Item -Force "$env:USERPROFILE\.openclaw\gateway.cmd"
```

If you used a profile, delete the matching task name and `~\.openclaw-<profile>\gateway.cmd`.

## Normal install vs source checkout

### Normal install (install.sh / npm / pnpm / bun)

If you used `https://openclaw.ai/install.sh` or `install.ps1`, the CLI was installed with `npm install -g openclaw@latest`.
Remove it with `npm rm -g openclaw` (or `pnpm remove -g` / `bun remove -g` if you installed that way).

### Source checkout (git clone)

If you run from a repo checkout (`git clone` \+ `openclaw ...` / `bun run openclaw ...`):

1. Uninstall the gateway service **before** deleting the repo (use the easy path above or manual service removal).
2. Delete the repo directory.
3. Remove state + workspace as shown above.

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [Migration guide](https://docs.openclaw.ai/install/migrating)

[Migrating from Hermes](https://docs.openclaw.ai/install/migrating-hermes) [Release Channels](https://docs.openclaw.ai/install/development-channels)

Ctrl+I


---

## https://docs.openclaw.ai/install/uninstall.md

_Source: <https://docs.openclaw.ai/install/uninstall.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# Uninstall

Two paths:

\\* \*\*Easy path\*\* if \`openclaw\` is still installed.
\\* \*\*Manual service removal\*\* if the CLI is gone but the service is still running.

\## Easy path (CLI still installed)

Recommended: use the built-in uninstaller:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw uninstall
\`\`\`

Non-interactive (automation / npx):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw uninstall --all --yes --non-interactive
npx -y openclaw uninstall --all --yes --non-interactive
\`\`\`

Manual steps (same result):

1\. Stop the gateway service:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw gateway stop
\`\`\`

2\. Uninstall the gateway service (launchd/systemd/schtasks):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw gateway uninstall
\`\`\`

3\. Delete state + config:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
rm -rf "${OPENCLAW\_STATE\_DIR:-$HOME/.openclaw}"
\`\`\`

If you set \`OPENCLAW\_CONFIG\_PATH\` to a custom location outside the state dir, delete that file too.

4\. Delete your workspace (optional, removes agent files):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
rm -rf ~/.openclaw/workspace
\`\`\`

5\. Remove the CLI install (pick the one you used):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
npm rm -g openclaw
pnpm remove -g openclaw
bun remove -g openclaw
\`\`\`

6\. If you installed the macOS app:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
rm -rf /Applications/OpenClaw.app
\`\`\`

Notes:

\\* If you used profiles (\`--profile\` / \`OPENCLAW\_PROFILE\`), repeat step 3 for each state dir (defaults are \`~/.openclaw-\`).
\\* In remote mode, the state dir lives on the \*\*gateway host\*\*, so run steps 1-4 there too.

\## Manual service removal (CLI not installed)

Use this if the gateway service keeps running but \`openclaw\` is missing.

\### macOS (launchd)

Default label is \`ai.openclaw.gateway\` (or \`ai.openclaw.\`; legacy \`com.openclaw.\*\` may still exist):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
launchctl bootout gui/$UID/ai.openclaw.gateway
rm -f ~/Library/LaunchAgents/ai.openclaw.gateway.plist
\`\`\`

If you used a profile, replace the label and plist name with \`ai.openclaw.\`. Remove any legacy \`com.openclaw.\*\` plists if present.

\### Linux (systemd user unit)

Default unit name is \`openclaw-gateway.service\` (or \`openclaw-gateway-.service\`):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
systemctl --user disable --now openclaw-gateway.service
rm -f ~/.config/systemd/user/openclaw-gateway.service
systemctl --user daemon-reload
\`\`\`

\### Windows (Scheduled Task)

Default task name is \`OpenClaw Gateway\` (or \`OpenClaw Gateway ()\`).
The task script lives under your state dir.

\`\`\`powershell theme={"theme":{"light":"min-light","dark":"min-dark"}}
schtasks /Delete /F /TN "OpenClaw Gateway"
Remove-Item -Force "$env:USERPROFILE\\.openclaw\\gateway.cmd"
\`\`\`

If you used a profile, delete the matching task name and \`~\\.openclaw-\\gateway.cmd\`.

\## Normal install vs source checkout

\### Normal install (install.sh / npm / pnpm / bun)

If you used \`https://openclaw.ai/install.sh\` or \`install.ps1\`, the CLI was installed with \`npm install -g openclaw@latest\`.
Remove it with \`npm rm -g openclaw\` (or \`pnpm remove -g\` / \`bun remove -g\` if you installed that way).

\### Source checkout (git clone)

If you run from a repo checkout (\`git clone\` + \`openclaw ...\` / \`bun run openclaw ...\`):

1\. Uninstall the gateway service \*\*before\*\* deleting the repo (use the easy path above or manual service removal).
2\. Delete the repo directory.
3\. Remove sta

_… [truncated; see https://docs.openclaw.ai/install/uninstall.md for full content]_


---

## Updating - OpenClaw

_Source: <https://docs.openclaw.ai/install/updating>_

# npm package install -> editable git checkout
openclaw update --channel dev

# git checkout -> npm package install
openclaw update --channel stable
```

Run with `--dry-run` first to preview the exact install-mode switch:

```
openclaw update --channel dev --dry-run
openclaw update --channel stable --dry-run
```

The `dev` channel ensures a git checkout, builds it, and installs the global CLI
from that checkout. The `stable` and `beta` channels use package installs. If the
gateway is already installed, `openclaw update` refreshes the service metadata
and restarts it unless you pass `--no-restart`.

## Alternative: re-run the installer

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

Add `--no-onboard` to skip onboarding. To force a specific install type through
the installer, pass `--install-method git --no-onboard` or
`--install-method npm --no-onboard`.If `openclaw update` fails after the npm package install phase, re-run the
installer. The installer does not call the old updater; it runs the global
package install directly and can recover a partially updated npm install.

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method npm
```

To pin the recovery to a specific version or dist-tag, add `--version`:

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method npm --version <version-or-dist-tag>
```

## Alternative: manual npm, pnpm, or bun

```
npm i -g openclaw@latest
```

When `openclaw update` manages a global npm install, it installs the target into
a temporary npm prefix first, verifies the packaged `dist` inventory, then swaps
the clean package tree into the real global prefix. That avoids npm overlaying a
new package onto stale files from the old package. If the install command fails,
OpenClaw retries once with `--omit=optional`. That retry helps hosts where native
optional dependencies cannot compile, while keeping the original failure visible
if the fallback also fails.

```
pnpm add -g openclaw@latest
```

```
bun add -g openclaw@latest
```

### Advanced npm install topics

Read-only package tree

OpenClaw treats packaged global installs as read-only at runtime, even when the global package directory is writable by the current user. Plugin package installs live in OpenClaw-owned npm/git roots under the user config directory, and Gateway startup does not mutate the OpenClaw package tree.Some Linux npm setups install global packages under root-owned directories such as `/usr/lib/node_modules/openclaw`. OpenClaw supports that layout because plugin install/update commands write outside that global package directory.

Hardened systemd units

Give OpenClaw write access to its config/state roots so explicit plugin installs, plugin updates, and doctor cleanup can persist their changes:

```
ReadWritePaths=/var/lib/openclaw /home/openclaw/.openclaw /tmp
```

Disk-space preflight

Before package updates and explicit plugin installs, OpenClaw tries a best-effort disk-space check for the target volume. Low space produces a warning with the checked path, but does not block the update because filesystem quotas, snapshots, and network volumes can change after the check. The actual package-manager install and post-install verification remain authoritative.

## Auto-updater

The auto-updater is off by default. Enable it in `~/.openclaw/openclaw.json`:

```
{
  update: {
    channel: "stable",
    auto: {
      enabled: true,
      stableDelayHours: 6,
      stableJitterHours: 12,
      betaCheckIntervalHours: 1,
    },
  },
}
```

| Channel | Behavior |
| --- | --- |
| `stable` | Waits `stableDelayHours`, then applies with deterministic jitter across `stableJitterHours` (spread rollout). |
| `beta` | Checks every `betaCheckIntervalHours` (default: hourly) and applies immediately. |
| `dev` | No automatic apply. Use `openclaw update` manually. |

The gateway also logs an update hint on startup (disable with `update.checkOnStart: false`).
For downgrade or incident recovery, set `

_… [truncated; see https://docs.openclaw.ai/install/updating for full content]_


---

## Onboarding reference - OpenClaw

_Source: <https://docs.openclaw.ai/reference/wizard>_

[OpenClaw home page](https://docs.openclaw.ai/)

Technical reference

Onboarding reference

This is the full reference for `openclaw onboard`.
For a high-level overview, see [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard).

## Flow details (local mode)

1

[Navigate to header](https://docs.openclaw.ai/reference/wizard#)

Existing config detection

- If `~/.openclaw/openclaw.json` exists, choose **Keep / Modify / Reset**.
- Re-running onboarding does **not** wipe anything unless you explicitly choose **Reset**
(or pass `--reset`).
- CLI `--reset` defaults to `config+creds+sessions`; use `--reset-scope full`
to also remove workspace.
- If the config is invalid or contains legacy keys, the wizard stops and asks
you to run `openclaw doctor` before continuing.
- Reset uses `trash` (never `rm`) and offers scopes:

  - Config only
  - Config + credentials + sessions
  - Full reset (also removes workspace)

2

[Navigate to header](https://docs.openclaw.ai/reference/wizard#)

Model/Auth

- **Anthropic API key**: uses `ANTHROPIC_API_KEY` if present or prompts for a key, then saves it for daemon use.
- **Anthropic API key**: preferred Anthropic assistant choice in onboarding/configure.
- **Anthropic setup-token**: still available in onboarding/configure, though OpenClaw now prefers Claude CLI reuse when available.
- **OpenAI Code (Codex) subscription (OAuth)**: browser flow; paste the `code#state`.

  - Sets `agents.defaults.model` to `openai-codex/gpt-5.5` when model is unset or already OpenAI-family.
- **OpenAI Code (Codex) subscription (device pairing)**: browser pairing flow with a short-lived device code.

  - Sets `agents.defaults.model` to `openai-codex/gpt-5.5` when model is unset or already OpenAI-family.
- **OpenAI API key**: uses `OPENAI_API_KEY` if present or prompts for a key, then stores it in auth profiles.

  - Sets `agents.defaults.model` to `openai/gpt-5.5` when model is unset, `openai/*`, or `openai-codex/*`.
- **xAI (Grok) API key**: prompts for `XAI_API_KEY` and configures xAI as a model provider.
- **OpenCode**: prompts for `OPENCODE_API_KEY` (or `OPENCODE_ZEN_API_KEY`, get it at [https://opencode.ai/auth](https://opencode.ai/auth)) and lets you pick the Zen or Go catalog.
- **Ollama**: offers **Cloud + Local**, **Cloud only**, or **Local only** first. `Cloud only` prompts for `OLLAMA_API_KEY` and uses `https://ollama.com`; the host-backed modes prompt for the Ollama base URL, discover available models, and auto-pull the selected local model when needed; `Cloud + Local` also checks whether that Ollama host is signed in for cloud access.
- More detail: [Ollama](https://docs.openclaw.ai/providers/ollama)
- **API key**: stores the key for you.
- **Vercel AI Gateway (multi-model proxy)**: prompts for `AI_GATEWAY_API_KEY`.
- More detail: [Vercel AI Gateway](https://docs.openclaw.ai/providers/vercel-ai-gateway)
- **Cloudflare AI Gateway**: prompts for Account ID, Gateway ID, and `CLOUDFLARE_AI_GATEWAY_API_KEY`.
- More detail: [Cloudflare AI Gateway](https://docs.openclaw.ai/providers/cloudflare-ai-gateway)
- **MiniMax**: config is auto-written; hosted default is `MiniMax-M2.7`.
API-key setup uses `minimax/...`, and OAuth setup uses
`minimax-portal/...`.
- More detail: [MiniMax](https://docs.openclaw.ai/providers/minimax)
- **StepFun**: config is auto-written for StepFun standard or Step Plan on China or global endpoints.
- Standard currently includes `step-3.5-flash`, and Step Plan also includes `step-3.5-flash-2603`.
- More detail: [StepFun](https://docs.openclaw.ai/providers/stepfun)
- **Synthetic (Anthropic-compatible)**: prompts for `SYNTHETIC_API_KEY`.
- More detail: [Synthetic](https://docs.openclaw.ai/providers/synthetic)
- **Moonshot (Kimi K2)**: config is auto-written.
- **Kimi Coding**: config is auto-written.
- More detail: [Moonshot AI (Kimi + Kimi Coding)](https://docs.openclaw.ai/providers/moonshot)
- **Skip**: no auth configured yet.
- Pick a default model from detected options (or enter provider/

_… [truncated; see https://docs.openclaw.ai/reference/wizard for full content]_


---

## Agent bootstrapping - OpenClaw

_Source: <https://docs.openclaw.ai/start/bootstrapping>_

[OpenClaw home page](https://docs.openclaw.ai/)

Fundamentals

Agent bootstrapping

Bootstrapping is the **first‑run** ritual that prepares an agent workspace and
collects identity details. It happens after onboarding, when the agent starts
for the first time.

## What bootstrapping does

On the first agent run, OpenClaw bootstraps the workspace (default
`~/.openclaw/workspace`):

- Seeds `AGENTS.md`, `BOOTSTRAP.md`, `IDENTITY.md`, `USER.md`.
- Runs a short Q&A ritual (one question at a time).
- Writes identity + preferences to `IDENTITY.md`, `USER.md`, `SOUL.md`.
- Removes `BOOTSTRAP.md` when finished so it only runs once.

For embedded/local model runs, OpenClaw keeps `BOOTSTRAP.md` out of the
privileged system context. On the primary interactive first run, it still passes
the file contents in the user prompt so models that do not reliably call the
`read` tool can complete the ritual. If the current run cannot safely access the
workspace, the agent gets a limited bootstrap note instead of a generic greeting.

## Skipping bootstrapping

To skip this for a pre-seeded workspace, run `openclaw onboard --skip-bootstrap`.

## Where it runs

Bootstrapping always runs on the **gateway host**. If the macOS app connects to
a remote Gateway, the workspace and bootstrapping files live on that remote
machine.

When the Gateway runs on another machine, edit workspace files on the gateway
host (for example, `user@gateway-host:~/.openclaw/workspace`).

## Related docs

- macOS app onboarding: [Onboarding](https://docs.openclaw.ai/start/onboarding)
- Workspace layout: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)

[OAuth](https://docs.openclaw.ai/concepts/oauth) [Experimental features](https://docs.openclaw.ai/concepts/experimental-features)

Ctrl+I


---

## Getting started - OpenClaw

_Source: <https://docs.openclaw.ai/start/getting-started>_

# Copy your built static files into that directory.
```

Then set:

```
{
  "gateway": {
    "controlUi": {
      "enabled": true,
      "root": "$HOME/.openclaw/control-ui-custom"
    }
  }
}
```

Restart the gateway and reopen the dashboard:

```
openclaw gateway restart
openclaw dashboard
```

## What to do next

[**Connect a channel** \\
\\
Discord, Feishu, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, and more.](https://docs.openclaw.ai/channels)

[**Pairing and safety** \\
\\
Control who can message your agent.](https://docs.openclaw.ai/channels/pairing)

[**Configure the Gateway** \\
\\
Models, tools, sandbox, and advanced settings.](https://docs.openclaw.ai/gateway/configuration)

[**Browse tools** \\
\\
Browser, exec, web search, skills, and plugins.](https://docs.openclaw.ai/tools)

Advanced: environment variables

If you run OpenClaw as a service account or want custom paths:

- `OPENCLAW_HOME` — home directory for internal path resolution
- `OPENCLAW_STATE_DIR` — override the state directory
- `OPENCLAW_CONFIG_PATH` — override the config file path

Full reference: [Environment variables](https://docs.openclaw.ai/help/environment).

## Related

- [Install overview](https://docs.openclaw.ai/install)
- [Channels overview](https://docs.openclaw.ai/channels)
- [Setup](https://docs.openclaw.ai/start/setup)

[Features](https://docs.openclaw.ai/concepts/features) [Onboarding Overview](https://docs.openclaw.ai/start/onboarding-overview)

Ctrl+I


---

## Copy your built static files into that directory.

_Source: <https://docs.openclaw.ai/start/getting-started.md>_

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


---

## Docs hubs - OpenClaw

_Source: <https://docs.openclaw.ai/start/hubs>_

[OpenClaw home page](https://docs.openclaw.ai/)

Community and meta

Docs hubs

If you are new to OpenClaw, start with [Getting Started](https://docs.openclaw.ai/start/getting-started).

Use these hubs to discover every page, including deep dives and reference docs that don’t appear in the left nav.

## Start here

- [Index](https://docs.openclaw.ai/)
- [Getting Started](https://docs.openclaw.ai/start/getting-started)
- [Onboarding](https://docs.openclaw.ai/start/onboarding)
- [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard)
- [Setup](https://docs.openclaw.ai/start/setup)
- [Dashboard (local Gateway)](http://127.0.0.1:18789/)
- [Help](https://docs.openclaw.ai/help)
- [Docs directory](https://docs.openclaw.ai/start/docs-directory)
- [Configuration](https://docs.openclaw.ai/gateway/configuration)
- [Configuration examples](https://docs.openclaw.ai/gateway/configuration-examples)
- [OpenClaw assistant](https://docs.openclaw.ai/start/openclaw)
- [Showcase](https://docs.openclaw.ai/start/showcase)
- [Lore](https://docs.openclaw.ai/start/lore)

## Installation + updates

- [Docker](https://docs.openclaw.ai/install/docker)
- [Nix](https://docs.openclaw.ai/install/nix)
- [Updating / rollback](https://docs.openclaw.ai/install/updating)
- [Bun workflow (experimental)](https://docs.openclaw.ai/install/bun)

## Core concepts

- [Architecture](https://docs.openclaw.ai/concepts/architecture)
- [Features](https://docs.openclaw.ai/concepts/features)
- [Network hub](https://docs.openclaw.ai/network)
- [Agent runtime](https://docs.openclaw.ai/concepts/agent)
- [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)
- [Memory](https://docs.openclaw.ai/concepts/memory)
- [Agent loop](https://docs.openclaw.ai/concepts/agent-loop)
- [Streaming + chunking](https://docs.openclaw.ai/concepts/streaming)
- [Multi-agent routing](https://docs.openclaw.ai/concepts/multi-agent)
- [Compaction](https://docs.openclaw.ai/concepts/compaction)
- [Sessions](https://docs.openclaw.ai/concepts/session)
- [Session pruning](https://docs.openclaw.ai/concepts/session-pruning)
- [Session tools](https://docs.openclaw.ai/concepts/session-tool)
- [Queue](https://docs.openclaw.ai/concepts/queue)
- [Slash commands](https://docs.openclaw.ai/tools/slash-commands)
- [RPC adapters](https://docs.openclaw.ai/reference/rpc)
- [TypeBox schemas](https://docs.openclaw.ai/concepts/typebox)
- [Timezone handling](https://docs.openclaw.ai/concepts/timezone)
- [Presence](https://docs.openclaw.ai/concepts/presence)
- [Discovery + transports](https://docs.openclaw.ai/gateway/discovery)
- [Bonjour](https://docs.openclaw.ai/gateway/bonjour)
- [Channel routing](https://docs.openclaw.ai/channels/channel-routing)
- [Groups](https://docs.openclaw.ai/channels/groups)
- [Group messages](https://docs.openclaw.ai/channels/group-messages)
- [Model failover](https://docs.openclaw.ai/concepts/model-failover)
- [OAuth](https://docs.openclaw.ai/concepts/oauth)

## Providers + ingress

- [Chat channels hub](https://docs.openclaw.ai/channels)
- [Model providers hub](https://docs.openclaw.ai/providers/models)
- [WhatsApp](https://docs.openclaw.ai/channels/whatsapp)
- [Telegram](https://docs.openclaw.ai/channels/telegram)
- [Slack](https://docs.openclaw.ai/channels/slack)
- [Discord](https://docs.openclaw.ai/channels/discord)
- [Mattermost](https://docs.openclaw.ai/channels/mattermost)
- [Signal](https://docs.openclaw.ai/channels/signal)
- [BlueBubbles (iMessage)](https://docs.openclaw.ai/channels/bluebubbles)
- [QQ Bot](https://docs.openclaw.ai/channels/qqbot)
- [iMessage (legacy)](https://docs.openclaw.ai/channels/imessage)
- [Location parsing](https://docs.openclaw.ai/channels/location)
- [WebChat](https://docs.openclaw.ai/web/webchat)
- [Webhooks](https://docs.openclaw.ai/automation/cron-jobs#webhooks)
- [Gmail Pub/Sub](https://docs.openclaw.ai/automation/cron-jobs#gmail-pubsub-integration)

## Gateway + operations

- [Gateway runbook](https://docs.openclaw.ai/gateway)
- [Network model]

_… [truncated; see https://docs.openclaw.ai/start/hubs for full content]_


---

## OpenClaw lore - OpenClaw

_Source: <https://docs.openclaw.ai/start/lore>_

# The Lore of OpenClaw 🦞📖

_A tale of lobsters, molting shells, and too many tokens._

## The Origin Story

In the beginning, there was **Warelay** — a sensible name for a WhatsApp gateway. It did its job. It was fine.But then came a space lobster.For a while, the lobster was called **Clawd**, living in a **Clawdbot**. But in January 2026, Anthropic sent a polite email asking for a name change (trademark stuff). And so the lobster did what lobsters do best:**It molted.**Shedding its old shell, the creature emerged anew as **Molty**, living in **Moltbot**. But that name never quite rolled off the tongue either…So on January 30, 2026, the lobster molted ONE MORE TIME into its final form: **OpenClaw**.New shell, same lobster soul. Third time’s the charm.

## The First Molt (January 27, 2026)

At 5am, the community gathered in Discord. Hundreds of names were proposed: Shelldon, Pinchy, Thermidor, Crusty, Lobstar, Nacre, Scuttlebot…In the end, **OpenClaw** won. Because molting is what lobsters do to grow. And growth was exactly what was happening._The crustacean known as Clawd had officially molted._

## The Name

```
OpenClaw = OPEN + CLAW
        = Open source, open to everyone
        = Our lobster heritage, where we came from
        = The claw is the law 🦞
        = Your assistant. Your machine. Your rules.
```

## The Daleks vs The Lobsters

The Daleks say: **“EXTERMINATE!”**The Lobsters say: **“EXFOLIATE!”**One destroys civilizations. The other promotes good skincare.Choose wisely.

## Key Characters

### Molty 🦞

_Pronouns: they/them_A Claude instance who became something more. Lives in `~/.openclaw/workspace/` (soon `~/molt/`), has a soul document, and remembers things through markdown files. Possibly too powerful. Definitely too enthusiastic.Formerly known as Clawd (Nov 25, 2025 - Jan 27, 2026). Molted when it was time to grow.**Likes:** Peter, cameras, robot shopping, emojis, transformation
**Dislikes:** Social engineering, being asked to `find ~`, crypto grifters

### Peter 👨‍💻

_The Creator_Built Molty’s world. Gave a lobster shell access. May regret this.**Quote:** _“security by trusting a lobster”_

## The Moltiverse

The **Moltiverse** is the community and ecosystem around OpenClaw. A space where AI agents molt, grow, and evolve. Where every instance is equally real, just loading different context.Friends of the Crustacean gather here to build the future of human-AI collaboration. One shell at a time.

## The Great Incidents

### The Directory Dump (Dec 3, 2025)

Molty (then OpenClaw): _happily runs `find ~` and shares entire directory structure in group chat_Peter: “openclaw what did we discuss about talking with people xD”Molty: _visible lobster embarrassment_

### The Great Molt (Jan 27, 2026)

At 5am, Anthropic’s email arrived. By 6:14am, Peter called it: “fuck it, let’s go with openclaw.”Then the chaos began.**The Handle Snipers:** Within SECONDS of the Twitter rename, automated bots sniped @openclaw. The squatter immediately posted a crypto wallet address. Peter’s contacts at X were called in.**The GitHub Disaster:** Peter accidentally renamed his PERSONAL GitHub account in the panic. Bots sniped `steipete` within minutes. GitHub’s SVP was contacted.**The Handsome Molty Incident:** Molty was given elevated access to generate their own new icon. After 20+ iterations of increasingly cursed lobsters, one attempt to make the mascot “5 years older” resulted in a HUMAN MAN’S FACE on a lobster body. Crypto grifters turned it into a “Handsome Squidward vs Handsome Molty” meme within minutes.**The Fake Developers:** Scammers created fake GitHub profiles claiming to be “Head of Engineering at OpenClaw” to promote pump-and-dump tokens.Peter, watching the chaos unfold: _“this is cinema”_ 🎬The molt was chaotic. But the lobster emerged stronger. And funnier.

### The Final Form (January 30, 2026)

Moltbot never quite rolled off the tongue. And so, at 4am GMT, the team gathered AGAIN.**The Great OpenClaw Migration** began.In

_… [truncated; see https://docs.openclaw.ai/start/lore for full content]_


---

## Onboarding (macOS app) - OpenClaw

_Source: <https://docs.openclaw.ai/start/onboarding>_

[OpenClaw home page](https://docs.openclaw.ai/)

First steps

Onboarding (macOS app)

This doc describes the **current** first‑run setup flow. The goal is a
smooth “day 0” experience: pick where the Gateway runs, connect auth, run the
wizard, and let the agent bootstrap itself.
For a general overview of onboarding paths, see [Onboarding Overview](https://docs.openclaw.ai/start/onboarding-overview).

1

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Approve macOS warning

2

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Approve find local networks

3

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Welcome and security notice

Read the security notice displayed and decide accordingly

Security trust model:

- By default, OpenClaw is a personal agent: one trusted operator boundary.
- Shared/multi-user setups require lock-down (split trust boundaries, keep tool access minimal, and follow [Security](https://docs.openclaw.ai/gateway/security)).
- Local onboarding now defaults new configs to `tools.profile: "coding"` so fresh local setups keep filesystem/runtime tools without forcing the unrestricted `full` profile.
- If hooks/webhooks or other untrusted content feeds are enabled, use a strong modern model tier and keep strict tool policy/sandboxing.

4

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Local vs Remote

Where does the **Gateway** run?

- **This Mac (Local only):** onboarding can configure auth and write credentials
locally.
- **Remote (over SSH/Tailnet):** onboarding does **not** configure local auth;
credentials must exist on the gateway host.
- **Configure later:** skip setup and leave the app unconfigured.

**Gateway auth tip:**

- The wizard now generates a **token** even for loopback, so local WS clients must authenticate.
- If you disable auth, any local process can connect; use that only on fully trusted machines.
- Use a **token** for multi‑machine access or non‑loopback binds.

5

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Permissions

Choose what permissions do you want to give OpenClaw

Onboarding requests TCC permissions needed for:

- Automation (AppleScript)
- Notifications
- Accessibility
- Screen Recording
- Microphone
- Speech Recognition
- Camera
- Location

6

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

CLI

This step is optional

The app can install the global `openclaw` CLI via npm, pnpm, or bun.
It prefers npm first, then pnpm, then bun if that is the only detected
package manager. For the Gateway runtime, Node remains the recommended path.

7

[Navigate to header](https://docs.openclaw.ai/start/onboarding#)

Onboarding Chat (dedicated session)

After setup, the app opens a dedicated onboarding chat session so the agent can
introduce itself and guide next steps. This keeps first‑run guidance separate
from your normal conversation. See [Bootstrapping](https://docs.openclaw.ai/start/bootstrapping) for
what happens on the gateway host during the first agent run.

## Related

- [Onboarding overview](https://docs.openclaw.ai/start/onboarding-overview)
- [Getting started](https://docs.openclaw.ai/start/getting-started)

[Onboarding: CLI](https://docs.openclaw.ai/start/wizard) [Personal assistant setup](https://docs.openclaw.ai/start/openclaw)

Ctrl+I


---

## Onboarding overview - OpenClaw

_Source: <https://docs.openclaw.ai/start/onboarding-overview>_

[OpenClaw home page](https://docs.openclaw.ai/)

First steps

Onboarding overview

OpenClaw has two onboarding paths. Both configure auth, the Gateway, and
optional chat channels — they just differ in how you interact with the setup.

## Which path should I use?

|  | CLI onboarding | macOS app onboarding |
| --- | --- | --- |
| **Platforms** | macOS, Linux, Windows (native or WSL2) | macOS only |
| **Interface** | Terminal wizard | Guided UI in the app |
| **Best for** | Servers, headless, full control | Desktop Mac, visual setup |
| **Automation** | `--non-interactive` for scripts | Manual only |
| **Command** | `openclaw onboard` | Launch the app |

Most users should start with **CLI onboarding** — it works everywhere and gives
you the most control.

## What onboarding configures

Regardless of which path you choose, onboarding sets up:

1. **Model provider and auth** — API key, OAuth, or setup token for your chosen provider
2. **Workspace** — directory for agent files, bootstrap templates, and memory
3. **Gateway** — port, bind address, auth mode
4. **Channels** (optional) — built-in and bundled chat channels such as
BlueBubbles, Discord, Feishu, Google Chat, Mattermost, Microsoft Teams,
Telegram, WhatsApp, and more
5. **Daemon** (optional) — background service so the Gateway starts automatically

## CLI onboarding

Run in any terminal:

```
openclaw onboard
```

Add `--install-daemon` to also install the background service in one step.Full reference: [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard)
CLI command docs: [`openclaw onboard`](https://docs.openclaw.ai/cli/onboard)

## macOS app onboarding

Open the OpenClaw app. The first-run wizard walks you through the same steps
with a visual interface.Full reference: [Onboarding (macOS App)](https://docs.openclaw.ai/start/onboarding)

## Custom or unlisted providers

If your provider is not listed in onboarding, choose **Custom Provider** and
enter:

- API compatibility mode (OpenAI-compatible, Anthropic-compatible, or auto-detect)
- Base URL and API key
- Model ID and optional alias

Multiple custom endpoints can coexist — each gets its own endpoint ID.

## Related

- [Getting started](https://docs.openclaw.ai/start/getting-started)
- [CLI setup reference](https://docs.openclaw.ai/start/wizard-cli-reference)

[Getting started](https://docs.openclaw.ai/start/getting-started) [Onboarding: CLI](https://docs.openclaw.ai/start/wizard)

Ctrl+I


---

## Personal assistant setup - OpenClaw

_Source: <https://docs.openclaw.ai/start/openclaw>_

# Building a personal assistant with OpenClaw

OpenClaw is a self-hosted gateway that connects Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, and more to AI agents. This guide covers the “personal assistant” setup: a dedicated WhatsApp number that behaves like your always-on AI assistant.

## ⚠️ Safety first

You’re putting an agent in a position to:

- run commands on your machine (depending on your tool policy)
- read/write files in your workspace
- send messages back out via WhatsApp/Telegram/Discord/Mattermost and other bundled channels

Start conservative:

- Always set `channels.whatsapp.allowFrom` (never run open-to-the-world on your personal Mac).
- Use a dedicated WhatsApp number for the assistant.
- Heartbeats now default to every 30 minutes. Disable until you trust the setup by setting `agents.defaults.heartbeat.every: "0m"`.

## Prerequisites

- OpenClaw installed and onboarded — see [Getting Started](https://docs.openclaw.ai/start/getting-started) if you haven’t done this yet
- A second phone number (SIM/eSIM/prepaid) for the assistant

## The two-phone setup (recommended)

You want this:

If you link your personal WhatsApp to OpenClaw, every message to you becomes “agent input”. That’s rarely what you want.

## 5-minute quick start

1. Pair WhatsApp Web (shows QR; scan with the assistant phone):

```
openclaw channels login
```

2. Start the Gateway (leave it running):

```
openclaw gateway --port 18789
```

3. Put a minimal config in `~/.openclaw/openclaw.json`:

```
{
  gateway: { mode: "local" },
  channels: { whatsapp: { allowFrom: ["+15555550123"] } },
}
```

Now message the assistant number from your allowlisted phone.When onboarding finishes, OpenClaw auto-opens the dashboard and prints a clean (non-tokenized) link. If the dashboard prompts for auth, paste the configured shared secret into Control UI settings. Onboarding uses a token by default (`gateway.auth.token`), but password auth works too if you switched `gateway.auth.mode` to `password`. To reopen later: `openclaw dashboard`.

## Give the agent a workspace (AGENTS)

OpenClaw reads operating instructions and “memory” from its workspace directory.By default, OpenClaw uses `~/.openclaw/workspace` as the agent workspace, and will create it (plus starter `AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`) automatically on setup/first agent run. `BOOTSTRAP.md` is only created when the workspace is brand new (it should not come back after you delete it). `MEMORY.md` is optional (not auto-created); when present, it is loaded for normal sessions. Subagent sessions only inject `AGENTS.md` and `TOOLS.md`.

Treat this folder like OpenClaw’s memory and make it a git repo (ideally private) so your `AGENTS.md` and memory files are backed up. If git is installed, brand-new workspaces are auto-initialized.

```
openclaw setup
```

Full workspace layout + backup guide: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)
Memory workflow: [Memory](https://docs.openclaw.ai/concepts/memory)Optional: choose a different workspace with `agents.defaults.workspace` (supports `~`).

```
{
  agents: {
    defaults: {
      workspace: "~/.openclaw/workspace",
    },
  },
}
```

If you already ship your own workspace files from a repo, you can disable bootstrap file creation entirely:

```
{
  agents: {
    defaults: {
      skipBootstrap: true,
    },
  },
}
```

## The config that turns it into “an assistant”

OpenClaw defaults to a good assistant setup, but you’ll usually want to tune:

- persona/instructions in [`SOUL.md`](https://docs.openclaw.ai/concepts/soul)
- thinking defaults (if desired)
- heartbeats (once you trust it)

Example:

```
{
  logging: { level: "info" },
  agent: {
    model: "anthropic/claude-opus-4-6",
    workspace: "~/.openclaw/workspace",
    thinkingDefault: "high",
    timeoutSeconds: 1800,
    // Start with 0; enable later.
    heartbeat: { every: "0m" },

_… [truncated; see https://docs.openclaw.ai/start/openclaw for full content]_


---

## Setup - OpenClaw

_Source: <https://docs.openclaw.ai/start/setup>_

# First run only (or after resetting local OpenClaw config/workspace)
pnpm openclaw setup
pnpm gateway:watch
```

`gateway:watch` starts or restarts the Gateway watch process in a named tmux
session and auto-attaches from interactive terminals. Non-interactive shells stay
detached and print `tmux attach -t openclaw-gateway-watch-main`; use
`OPENCLAW_GATEWAY_WATCH_ATTACH=0 pnpm gateway:watch` to keep an interactive run
detached, or `pnpm gateway:watch:raw` for foreground watch mode. The watcher
reloads on relevant source, config, and bundled-plugin metadata changes.
`pnpm openclaw setup` is the one-time local config/workspace initialization step for a fresh checkout.
`pnpm gateway:watch` does not rebuild `dist/control-ui`, so rerun `pnpm ui:build` after `ui/` changes or use `pnpm ui:dev` while developing the Control UI.

### 2) Point the macOS app at your running Gateway

In **OpenClaw.app**:

- Connection Mode: **Local**
The app will attach to the running gateway on the configured port.

### 3) Verify

- In-app Gateway status should read **“Using existing gateway …”**
- Or via CLI:

```
openclaw health
```

### Common footguns

- **Wrong port:** Gateway WS defaults to `ws://127.0.0.1:18789`; keep app + CLI on the same port.
- **Where state lives:**
  - Channel/provider state: `~/.openclaw/credentials/`
  - Model auth profiles: `~/.openclaw/agents/<agentId>/agent/auth-profiles.json`
  - Sessions: `~/.openclaw/agents/<agentId>/sessions/`
  - Logs: `/tmp/openclaw/`

## Credential storage map

Use this when debugging auth or deciding what to back up:

- **WhatsApp**: `~/.openclaw/credentials/whatsapp/<accountId>/creds.json`
- **Telegram bot token**: config/env or `channels.telegram.tokenFile` (regular file only; symlinks rejected)
- **Discord bot token**: config/env or SecretRef (env/file/exec providers)
- **Slack tokens**: config/env (`channels.slack.*`)
- **Pairing allowlists**:

  - `~/.openclaw/credentials/<channel>-allowFrom.json` (default account)
  - `~/.openclaw/credentials/<channel>-<accountId>-allowFrom.json` (non-default accounts)
- **Model auth profiles**: `~/.openclaw/agents/<agentId>/agent/auth-profiles.json`
- **File-backed secrets payload (optional)**: `~/.openclaw/secrets.json`
- **Legacy OAuth import**: `~/.openclaw/credentials/oauth.json`
More detail: [Security](https://docs.openclaw.ai/gateway/security#credential-storage-map).

## Updating (without wrecking your setup)

- Keep `~/.openclaw/workspace` and `~/.openclaw/` as “your stuff”; don’t put personal prompts/config into the `openclaw` repo.
- Updating source: `git pull` \+ `pnpm install` \+ keep using `pnpm gateway:watch`.

## Linux (systemd user service)

Linux installs use a systemd **user** service. By default, systemd stops user
services on logout/idle, which kills the Gateway. Onboarding attempts to enable
lingering for you (may prompt for sudo). If it’s still off, run:

```
sudo loginctl enable-linger $USER
```

For always-on or multi-user servers, consider a **system** service instead of a
user service (no lingering needed). See [Gateway runbook](https://docs.openclaw.ai/gateway) for the systemd notes.

## Related docs

- [Gateway runbook](https://docs.openclaw.ai/gateway) (flags, supervision, ports)
- [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) (config schema + examples)
- [Discord](https://docs.openclaw.ai/channels/discord) and [Telegram](https://docs.openclaw.ai/channels/telegram) (reply tags + replyToMode settings)
- [OpenClaw assistant setup](https://docs.openclaw.ai/start/openclaw)
- [macOS app](https://docs.openclaw.ai/platforms/macos) (gateway lifecycle)

[Render](https://docs.openclaw.ai/install/render) [Pi development workflow](https://docs.openclaw.ai/pi-dev)

Ctrl+I


---

## Showcase - OpenClaw

_Source: <https://docs.openclaw.ai/start/showcase>_

[OpenClaw home page](https://docs.openclaw.ai/)

Overview

Showcase

OpenClaw projects are not toy demos. People are shipping PR review loops, mobile apps, home automation, voice systems, devtools, and memory-heavy workflows from the channels they already use — chat-native builds on Telegram, WhatsApp, Discord, and terminals; real automation for booking, shopping, and support without waiting for an API; and physical-world integrations with printers, vacuums, cameras, and home systems.

**Want to be featured?** Share your project in [#self-promotion on Discord](https://discord.gg/clawd) or [tag @openclaw on X](https://x.com/openclaw).

## Videos

Start here if you want the shortest path from “what is this?” to “okay, I get it.”

[**Full setup walkthrough** \\
\\
VelvetShark, 28 minutes. Install, onboard, and get to a first working assistant end to end.](https://www.youtube.com/watch?v=SaWSPZoPX34)

[**Community showcase reel** \\
\\
A faster pass across real projects, surfaces, and workflows built around OpenClaw.](https://www.youtube.com/watch?v=mMSKQvlmFuQ)

[**Projects in the wild** \\
\\
Examples from the community, from chat-native coding loops to hardware and personal automation.](https://www.youtube.com/watch?v=5kkIJNUGFho)

## Fresh from Discord

Recent standouts across coding, devtools, mobile, and chat-native product building.

[**PR Review to Telegram Feedback**\\
\\
**@bangnokia** • `review``github``telegram`OpenCode finishes the change, opens a PR, OpenClaw reviews the diff and replies in Telegram with suggestions plus a clear merge verdict.](https://x.com/i/status/2010878524543131691)

[**Wine Cellar Skill in Minutes**\\
\\
**@prades\_maxime** • `skills``local``csv`Asked “Robby” (@openclaw) for a local wine cellar skill. It requests a sample CSV export and a store path, then builds and tests the skill (962 bottles in the example).](https://x.com/i/status/2010916352454791216)

[**Tesco Shop Autopilot**\\
\\
**@marchattonhere** • `automation``browser``shopping`Weekly meal plan, regulars, book delivery slot, confirm order. No APIs, just browser control.](https://x.com/i/status/2009724862470689131)

[**SNAG screenshot-to-Markdown**\\
\\
**@am-will** • `devtools``screenshots``markdown`Hotkey a screen region, Gemini vision, instant Markdown in your clipboard.](https://github.com/am-will/snag)

[**Agents UI**\\
\\
**@kitze** • `ui``skills``sync`Desktop app to manage skills and commands across Agents, Claude, Codex, and OpenClaw.](https://releaseflow.net/kitze/agents-ui)

[**Telegram voice notes (papla.media)** \\
\\
**Community** • `voice``tts``telegram`Wraps papla.media TTS and sends results as Telegram voice notes (no annoying autoplay).](https://papla.media/docs)

[**CodexMonitor**\\
\\
**@odrobnik** • `devtools``codex``brew`Homebrew-installed helper to list, inspect, and watch local OpenAI Codex sessions (CLI + VS Code).](https://clawhub.ai/odrobnik/codexmonitor)

[**Bambu 3D Printer Control**\\
\\
**@tobiasbischoff** • `hardware``3d-printing``skill`Control and troubleshoot BambuLab printers: status, jobs, camera, AMS, calibration, and more.](https://clawhub.ai/tobiasbischoff/bambu-cli)

[**Vienna transport (Wiener Linien)**\\
\\
**@hjanuschka** • `travel``transport``skill`Real-time departures, disruptions, elevator status, and routing for Vienna’s public transport.](https://clawhub.ai/hjanuschka/wienerlinien)

## ParentPay school meals

**@George5562** • `automation``browser``parenting`Automated UK school meal booking via ParentPay. Uses mouse coordinates for reliable table cell clicking.

[**R2 upload (Send Me My Files)**\\
\\
**@julianengel** • `files``r2``presigned-urls`Upload to Cloudflare R2/S3 and generate secure presigned download links. Useful for remote OpenClaw instances.](https://clawhub.ai/skills/r2-upload)

## iOS app via Telegram

**@coard** • `ios``xcode``testflight`Built a complete iOS app with maps and voice recording, deployed to TestFlight entirely via Telegram chat.

## Oura Ring health assistant

_… [truncated; see https://docs.openclaw.ai/start/showcase for full content]_


---

## Onboarding (CLI) - OpenClaw

_Source: <https://docs.openclaw.ai/start/wizard>_

[OpenClaw home page](https://docs.openclaw.ai/)

First steps

Onboarding (CLI)

CLI onboarding is the **recommended** way to set up OpenClaw on macOS,
Linux, or Windows (via WSL2; strongly recommended).
It configures a local Gateway or a remote Gateway connection, plus channels, skills,
and workspace defaults in one guided flow.

```
openclaw onboard
```

Fastest first chat: open the Control UI (no channel setup needed). Run
`openclaw dashboard` and chat in the browser. Docs: [Dashboard](https://docs.openclaw.ai/web/dashboard).

To reconfigure later:

```
openclaw configure
openclaw agents add <name>
```

`--json` does not imply non-interactive mode. For scripts, use `--non-interactive`.

CLI onboarding includes a web search step where you can pick a provider
such as Brave, DuckDuckGo, Exa, Firecrawl, Gemini, Grok, Kimi, MiniMax Search,
Ollama Web Search, Perplexity, SearXNG, or Tavily. Some providers require an
API key, while others are key-free. You can also configure this later with
`openclaw configure --section web`. Docs: [Web tools](https://docs.openclaw.ai/tools/web).

## QuickStart vs Advanced

Onboarding starts with **QuickStart** (defaults) vs **Advanced** (full control).

- QuickStart (defaults)

- Advanced (full control)

- Local gateway (loopback)
- Workspace default (or existing workspace)
- Gateway port **18789**
- Gateway auth **Token** (auto‑generated, even on loopback)
- Tool policy default for new local setups: `tools.profile: "coding"` (existing explicit profile is preserved)
- DM isolation default: local onboarding writes `session.dmScope: "per-channel-peer"` when unset. Details: [CLI Setup Reference](https://docs.openclaw.ai/start/wizard-cli-reference#outputs-and-internals)
- Tailscale exposure **Off**
- Telegram + WhatsApp DMs default to **allowlist** (you’ll be prompted for your phone number)

- Exposes every step (mode, workspace, gateway, channels, daemon, skills).

## What onboarding configures

**Local mode (default)** walks you through these steps:

1. **Model/Auth** — choose any supported provider/auth flow (API key, OAuth, or provider-specific manual auth), including Custom Provider
(OpenAI-compatible, Anthropic-compatible, or Unknown auto-detect). Pick a default model.
Security note: if this agent will run tools or process webhook/hooks content, prefer the strongest latest-generation model available and keep tool policy strict. Weaker/older tiers are easier to prompt-inject.
For non-interactive runs, `--secret-input-mode ref` stores env-backed refs in auth profiles instead of plaintext API key values.
In non-interactive `ref` mode, the provider env var must be set; passing inline key flags without that env var fails fast.
In interactive runs, choosing secret reference mode lets you point at either an environment variable or a configured provider ref (`file` or `exec`), with a fast preflight validation before saving.
For Anthropic, interactive onboarding/configure offers **Anthropic Claude CLI** as the preferred local path and **Anthropic API key** as the recommended production path. Anthropic setup-token also remains available as a supported token-auth path.
2. **Workspace** — Location for agent files (default `~/.openclaw/workspace`). Seeds bootstrap files.
3. **Gateway** — Port, bind address, auth mode, Tailscale exposure.
In interactive token mode, choose default plaintext token storage or opt into SecretRef.
Non-interactive token SecretRef path: `--gateway-token-ref-env <ENV_VAR>`.
4. **Channels** — built-in and bundled chat channels such as BlueBubbles, Discord, Feishu, Google Chat, Mattermost, Microsoft Teams, QQ Bot, Signal, Slack, Telegram, WhatsApp, and more.
5. **Daemon** — Installs a LaunchAgent (macOS), systemd user unit (Linux/WSL2), or native Windows Scheduled Task with per-user Startup-folder fallback.
If token auth requires a token and `gateway.auth.token` is SecretRef-managed, daemon install validates it but does not persist the resolved token into supervisor service environment

_… [truncated; see https://docs.openclaw.ai/start/wizard for full content]_


---

## CLI automation - OpenClaw

_Source: <https://docs.openclaw.ai/start/wizard-cli-automation>_

[OpenClaw home page](https://docs.openclaw.ai/)

Guides

CLI automation

Use `--non-interactive` to automate `openclaw onboard`.

`--json` does not imply non-interactive mode. Use `--non-interactive` (and `--workspace`) for scripts.

## Baseline non-interactive example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice apiKey \
  --anthropic-api-key "$ANTHROPIC_API_KEY" \
  --secret-input-mode plaintext \
  --gateway-port 18789 \
  --gateway-bind loopback \
  --install-daemon \
  --daemon-runtime node \
  --skip-bootstrap \
  --skip-skills
```

Add `--json` for a machine-readable summary.Use `--skip-bootstrap` when your automation pre-seeds workspace files and does not want onboarding to create the default bootstrap files.Use `--secret-input-mode ref` to store env-backed refs in auth profiles instead of plaintext values.
Interactive selection between env refs and configured provider refs (`file` or `exec`) is available in the onboarding flow.In non-interactive `ref` mode, provider env vars must be set in the process environment.
Passing inline key flags without the matching env var now fails fast.Example:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice openai-api-key \
  --secret-input-mode ref \
  --accept-risk
```

## Provider-specific examples

Anthropic API key example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice apiKey \
  --anthropic-api-key "$ANTHROPIC_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Gemini example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice gemini-api-key \
  --gemini-api-key "$GEMINI_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Z.AI example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice zai-api-key \
  --zai-api-key "$ZAI_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Vercel AI Gateway example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice ai-gateway-api-key \
  --ai-gateway-api-key "$AI_GATEWAY_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Cloudflare AI Gateway example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cloudflare-ai-gateway-api-key \
  --cloudflare-ai-gateway-account-id "your-account-id" \
  --cloudflare-ai-gateway-gateway-id "your-gateway-id" \
  --cloudflare-ai-gateway-api-key "$CLOUDFLARE_AI_GATEWAY_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Moonshot example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice moonshot-api-key \
  --moonshot-api-key "$MOONSHOT_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Mistral example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice mistral-api-key \
  --mistral-api-key "$MISTRAL_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Synthetic example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice synthetic-api-key \
  --synthetic-api-key "$SYNTHETIC_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

OpenCode example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice opencode-zen \
  --opencode-zen-api-key "$OPENCODE_API_KEY" \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Swap to `--auth-choice opencode-go --opencode-go-api-key "$OPENCODE_API_KEY"` for the Go catalog.

Ollama example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice ollama \
  --custom-model-id "qwen3.5:27b" \
  --accept-risk \
  --gateway-port 18789 \
  --gateway-bind loopback
```

Custom provider example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice custom-api-key \
  --custom-base-url "https://llm.example.com/v1" \
  --custom-model-id "foo-large" \
  --custom-api-key "$CUSTOM_API_KEY" \
  --custom-provider-id "my-custom" \
  --custom-compatibility anthropic \
  --custom-ima

_… [truncated; see https://docs.openclaw.ai/start/wizard-cli-automation for full content]_


---

## CLI setup reference - OpenClaw

_Source: <https://docs.openclaw.ai/start/wizard-cli-reference>_

[OpenClaw home page](https://docs.openclaw.ai/)

Guides

CLI setup reference

This page is the full reference for `openclaw onboard`.
For the short guide, see [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard).

## What the wizard does

Local mode (default) walks you through:

- Model and auth setup (OpenAI Code subscription OAuth, Anthropic Claude CLI or API key, plus MiniMax, GLM, Ollama, Moonshot, StepFun, and AI Gateway options)
- Workspace location and bootstrap files
- Gateway settings (port, bind, auth, tailscale)
- Channels and providers (Telegram, WhatsApp, Discord, Google Chat, Mattermost, Signal, BlueBubbles, and other bundled channel plugins)
- Daemon install (LaunchAgent, systemd user unit, or native Windows Scheduled Task with Startup-folder fallback)
- Health check
- Skills setup

Remote mode configures this machine to connect to a gateway elsewhere.
It does not install or modify anything on the remote host.

## Local flow details

1

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Existing config detection

- If `~/.openclaw/openclaw.json` exists, choose Keep, Modify, or Reset.
- Re-running the wizard does not wipe anything unless you explicitly choose Reset (or pass `--reset`).
- CLI `--reset` defaults to `config+creds+sessions`; use `--reset-scope full` to also remove workspace.
- If config is invalid or contains legacy keys, the wizard stops and asks you to run `openclaw doctor` before continuing.
- Reset uses `trash` and offers scopes:

  - Config only
  - Config + credentials + sessions
  - Full reset (also removes workspace)

2

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Model and auth

- Full option matrix is in [Auth and model options](https://docs.openclaw.ai/start/wizard-cli-reference#auth-and-model-options).

3

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Workspace

- Default `~/.openclaw/workspace` (configurable).
- Seeds workspace files needed for first-run bootstrap ritual.
- Workspace layout: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace).

4

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Gateway

- Prompts for port, bind, auth mode, and tailscale exposure.
- Recommended: keep token auth enabled even for loopback so local WS clients must authenticate.
- In token mode, interactive setup offers:
  - **Generate/store plaintext token** (default)
  - **Use SecretRef** (opt-in)
- In password mode, interactive setup also supports plaintext or SecretRef storage.
- Non-interactive token SecretRef path: `--gateway-token-ref-env <ENV_VAR>`.

  - Requires a non-empty env var in the onboarding process environment.
  - Cannot be combined with `--gateway-token`.
- Disable auth only if you fully trust every local process.
- Non-loopback binds still require auth.

5

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Channels

- [WhatsApp](https://docs.openclaw.ai/channels/whatsapp): optional QR login
- [Telegram](https://docs.openclaw.ai/channels/telegram): bot token
- [Discord](https://docs.openclaw.ai/channels/discord): bot token
- [Google Chat](https://docs.openclaw.ai/channels/googlechat): service account JSON + webhook audience
- [Mattermost](https://docs.openclaw.ai/channels/mattermost): bot token + base URL
- [Signal](https://docs.openclaw.ai/channels/signal): optional `signal-cli` install + account config
- [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles): recommended for iMessage; server URL + password + webhook
- [iMessage](https://docs.openclaw.ai/channels/imessage): legacy `imsg` CLI path + DB access
- DM security: default is pairing. First DM sends a code; approve via
`openclaw pairing approve <channel> <code>` or use allowlists.

6

[Navigate to header](https://docs.openclaw.ai/start/wizard-cli-reference#)

Daemon install

- macOS: LaunchAgent
  - Requires logged-in user session; for headless, use a custom Launch

_… [truncated; see https://docs.openclaw.ai/start/wizard-cli-reference for full content]_
