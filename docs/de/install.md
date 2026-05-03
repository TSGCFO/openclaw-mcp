---
source_url: https://docs.openclaw.ai/de/install
title: "Installation - OpenClaw"
---

[Zum Hauptinhalt springen](https://docs.openclaw.ai/de/install#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/de)

![DE](https://d3gk2c5xim1je2.cloudfront.net/flags/DE.svg)

Deutsch

Suchen...

Ctrl K

Suchen...

Navigation

Install overview

Installation

[Get started](https://docs.openclaw.ai/de) [Install](https://docs.openclaw.ai/de/install) [Channels](https://docs.openclaw.ai/de/channels) [Agents](https://docs.openclaw.ai/de/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/de/tools) [Models](https://docs.openclaw.ai/de/providers) [Platforms](https://docs.openclaw.ai/de/platforms) [Gateway & Ops](https://docs.openclaw.ai/de/gateway) [Reference](https://docs.openclaw.ai/de/cli) [Help](https://docs.openclaw.ai/de/help)

Auf dieser Seite

- [Systemanforderungen](https://docs.openclaw.ai/de/install#systemanforderungen)
- [Empfohlen: Installer-Skript](https://docs.openclaw.ai/de/install#empfohlen-installer-skript)
- [Alternative Installationsmethoden](https://docs.openclaw.ai/de/install#alternative-installationsmethoden)
- [Installer mit lokalem Präfix (install-cli.sh)](https://docs.openclaw.ai/de/install#installer-mit-lokalem-pr%C3%A4fix-install-cli-sh)
- [npm, pnpm oder bun](https://docs.openclaw.ai/de/install#npm-pnpm-oder-bun)
- [Aus dem Quellcode](https://docs.openclaw.ai/de/install#aus-dem-quellcode)
- [Von GitHub main installieren](https://docs.openclaw.ai/de/install#von-github-main-installieren)
- [Container und Paketmanager](https://docs.openclaw.ai/de/install#container-und-paketmanager)
- [Installation überprüfen](https://docs.openclaw.ai/de/install#installation-%C3%BCberpr%C3%BCfen)
- [Hosting und Bereitstellung](https://docs.openclaw.ai/de/install#hosting-und-bereitstellung)
- [Aktualisieren, migrieren oder deinstallieren](https://docs.openclaw.ai/de/install#aktualisieren-migrieren-oder-deinstallieren)
- [Fehlerbehebung: openclaw nicht gefunden](https://docs.openclaw.ai/de/install#fehlerbehebung-openclaw-nicht-gefunden)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

## [​](https://docs.openclaw.ai/de/install\#systemanforderungen)  Systemanforderungen

- **Node 24** (empfohlen) oder Node 22.14+ — das Installer-Skript übernimmt dies automatisch
- **macOS, Linux oder Windows** — sowohl natives Windows als auch WSL2 werden unterstützt; WSL2 ist stabiler. Siehe [Windows](https://docs.openclaw.ai/de/platforms/windows).
- `pnpm` wird nur benötigt, wenn Sie aus dem Quellcode bauen

## [​](https://docs.openclaw.ai/de/install\#empfohlen-installer-skript)  Empfohlen: Installer-Skript

Der schnellste Weg zur Installation. Es erkennt Ihr Betriebssystem, installiert bei Bedarf Node, installiert OpenClaw und startet das Onboarding.

- macOS / Linux / WSL2

- Windows (PowerShell)


```
curl -fsSL https://openclaw.ai/install.sh | bash
```

```
iwr -useb https://openclaw.ai/install.ps1 | iex
```

Installation ohne Onboarding:

- macOS / Linux / WSL2

- Windows (PowerShell)


```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
```

Alle Flags sowie Optionen für CI/Automatisierung finden Sie unter [Installer internals](https://docs.openclaw.ai/de/install/installer).

## [​](https://docs.openclaw.ai/de/install\#alternative-installationsmethoden)  Alternative Installationsmethoden

### [​](https://docs.openclaw.ai/de/install\#installer-mit-lokalem-pr%C3%A4fix-install-cli-sh)  Installer mit lokalem Präfix (`install-cli.sh`)

Verwenden Sie dies, wenn Sie OpenClaw und Node unter einem lokalen Präfix wie
`~/.openclaw` halten möchten, ohne von einer systemweiten Node-Installation abhängig zu sein:

```
curl -fsSL https://openclaw.ai/install-cli.sh | bash
```

Standardmäßig unterstützt es npm-Installationen sowie Installationen aus einem Git-Checkout innerhalb desselben
Präfix-Flows. Vollständige Referenz: [Installer internals](https://docs.openclaw.ai/de/install/installer#install-clish).Bereits installiert? Wechseln Sie zwischen Paket- und Git-Installationen mit
`openclaw update --channel dev` und `openclaw update --channel stable`. Siehe
[Aktualisieren](https://docs.openclaw.ai/de/install/updating#switch-between-npm-and-git-installs).

### [​](https://docs.openclaw.ai/de/install\#npm-pnpm-oder-bun)  npm, pnpm oder bun

Wenn Sie Node bereits selbst verwalten:

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

pnpm erfordert eine explizite Genehmigung für Pakete mit Build-Skripten. Führen Sie nach der ersten Installation `pnpm approve-builds -g` aus.

```
bun add -g openclaw@latest
openclaw onboard --install-daemon
```

Bun wird für den globalen CLI-Installationspfad unterstützt. Für die Gateway-Laufzeit bleibt Node die empfohlene Daemon-Laufzeit.

Fehlerbehebung: sharp-Build-Fehler (npm)

Wenn `sharp` aufgrund eines global installierten libvips fehlschlägt:

```
SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g openclaw@latest
```

### [​](https://docs.openclaw.ai/de/install\#aus-dem-quellcode)  Aus dem Quellcode

Für Mitwirkende oder alle, die aus einem lokalen Checkout heraus arbeiten möchten:

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install && pnpm build && pnpm ui:build
pnpm link --global
openclaw onboard --install-daemon
```

Oder überspringen Sie das Linken und verwenden Sie `pnpm openclaw ...` innerhalb des Repositorys. Vollständige Entwicklungs-Workflows finden Sie unter [Setup](https://docs.openclaw.ai/de/start/setup).

### [​](https://docs.openclaw.ai/de/install\#von-github-main-installieren)  Von GitHub main installieren

```
npm install -g github:openclaw/openclaw#main
```

### [​](https://docs.openclaw.ai/de/install\#container-und-paketmanager)  Container und Paketmanager

[**Docker** \\
\\
Containerisierte oder headless Deployments.](https://docs.openclaw.ai/de/install/docker)

[**Podman** \\
\\
Rootless-Container-Alternative zu Docker.](https://docs.openclaw.ai/de/install/podman)

[**Nix** \\
\\
Deklarative Installation über Nix Flake.](https://docs.openclaw.ai/de/install/nix)

[**Ansible** \\
\\
Automatisierte Bereitstellung für Flotten.](https://docs.openclaw.ai/de/install/ansible)

[**Bun** \\
\\
Nur-CLI-Nutzung über die Bun-Laufzeit.](https://docs.openclaw.ai/de/install/bun)

## [​](https://docs.openclaw.ai/de/install\#installation-%C3%BCberpr%C3%BCfen)  Installation überprüfen

```
openclaw --version      # prüfen, ob die CLI verfügbar ist
openclaw doctor         # auf Konfigurationsprobleme prüfen
openclaw gateway status # prüfen, ob das Gateway läuft
```

Wenn Sie nach der Installation einen verwalteten Start möchten:

- macOS: LaunchAgent über `openclaw onboard --install-daemon` oder `openclaw gateway install`
- Linux/WSL2: systemd-Benutzerdienst über dieselben Befehle
- Natives Windows: zuerst Scheduled Task, mit Fallback auf ein Login-Element im benutzerspezifischen Startup-Ordner, falls die Task-Erstellung verweigert wird

## [​](https://docs.openclaw.ai/de/install\#hosting-und-bereitstellung)  Hosting und Bereitstellung

Stellen Sie OpenClaw auf einem Cloud-Server oder VPS bereit:

[**VPS** \\
\\
Beliebiger Linux-VPS](https://docs.openclaw.ai/de/vps)

[**Docker VM** \\
\\
Gemeinsame Docker-Schritte](https://docs.openclaw.ai/de/install/docker-vm-runtime)

[**Kubernetes** \\
\\
K8s](https://docs.openclaw.ai/de/install/kubernetes)

[**Fly.io** \\
\\
Fly.io](https://docs.openclaw.ai/de/install/fly)

[**Hetzner** \\
\\
Hetzner](https://docs.openclaw.ai/de/install/hetzner)

[**GCP** \\
\\
Google Cloud](https://docs.openclaw.ai/de/install/gcp)

[**Azure** \\
\\
Azure](https://docs.openclaw.ai/de/install/azure)

[**Railway** \\
\\
Railway](https://docs.openclaw.ai/de/install/railway)

[**Render** \\
\\
Render](https://docs.openclaw.ai/de/install/render)

[**Northflank** \\
\\
Northflank](https://docs.openclaw.ai/de/install/northflank)

## [​](https://docs.openclaw.ai/de/install\#aktualisieren-migrieren-oder-deinstallieren)  Aktualisieren, migrieren oder deinstallieren

[**Aktualisieren** \\
\\
OpenClaw aktuell halten.](https://docs.openclaw.ai/de/install/updating)

[**Migrieren** \\
\\
Auf einen neuen Rechner umziehen.](https://docs.openclaw.ai/de/install/migrating)

[**Deinstallieren** \\
\\
OpenClaw vollständig entfernen.](https://docs.openclaw.ai/de/install/uninstall)

## [​](https://docs.openclaw.ai/de/install\#fehlerbehebung-openclaw-nicht-gefunden)  Fehlerbehebung: `openclaw` nicht gefunden

Wenn die Installation erfolgreich war, `openclaw` aber in Ihrem Terminal nicht gefunden wird:

```
node -v           # Ist Node installiert?
npm prefix -g     # Wo sind globale Pakete?
echo "$PATH"      # Ist das globale bin-Verzeichnis in PATH?
```

Wenn `$(npm prefix -g)/bin` nicht in Ihrem `$PATH` ist, fügen Sie es Ihrer Shell-Startdatei (`~/.zshrc` oder `~/.bashrc`) hinzu:

```
export PATH="$(npm prefix -g)/bin:$PATH"
```

Öffnen Sie dann ein neues Terminal. Weitere Details finden Sie unter [Node setup](https://docs.openclaw.ai/de/install/node).

[Installer-Interna](https://docs.openclaw.ai/de/install/installer)

Ctrl+I