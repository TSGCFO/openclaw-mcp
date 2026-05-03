---
source_url: https://docs.openclaw.ai/de/cli/update
title: "Aktualisieren - OpenClaw"
---

[Zum Hauptinhalt springen](https://docs.openclaw.ai/de/cli/update#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/de)

![DE](https://d3gk2c5xim1je2.cloudfront.net/flags/DE.svg)

Deutsch

Suchen...

Ctrl K

Suchen...

Navigation

Gateway and service

Aktualisieren

[Get started](https://docs.openclaw.ai/de) [Install](https://docs.openclaw.ai/de/install) [Channels](https://docs.openclaw.ai/de/channels) [Agents](https://docs.openclaw.ai/de/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/de/tools) [Models](https://docs.openclaw.ai/de/providers) [Platforms](https://docs.openclaw.ai/de/platforms) [Gateway & Ops](https://docs.openclaw.ai/de/gateway) [Reference](https://docs.openclaw.ai/de/cli) [Help](https://docs.openclaw.ai/de/help)

Auf dieser Seite

- [openclaw update](https://docs.openclaw.ai/de/cli/update#openclaw-update)
- [Verwendung](https://docs.openclaw.ai/de/cli/update#verwendung)
- [Optionen](https://docs.openclaw.ai/de/cli/update#optionen)
- [update status](https://docs.openclaw.ai/de/cli/update#update-status)
- [update wizard](https://docs.openclaw.ai/de/cli/update#update-wizard)
- [Was es macht](https://docs.openclaw.ai/de/cli/update#was-es-macht)
- [Git-Checkout-Ablauf](https://docs.openclaw.ai/de/cli/update#git-checkout-ablauf)
- [Kanalauswahl](https://docs.openclaw.ai/de/cli/update#kanalauswahl)
- [Update-Schritte](https://docs.openclaw.ai/de/cli/update#update-schritte)
- [--update-Kurzform](https://docs.openclaw.ai/de/cli/update#update-kurzform)
- [Verwandt](https://docs.openclaw.ai/de/cli/update#verwandt)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/de/cli/update\#openclaw-update)  `openclaw update`

Aktualisieren Sie OpenClaw sicher und wechseln Sie zwischen Stable-/Beta-/Dev-Kanälen.Wenn Sie über **npm/pnpm/bun** installiert haben (globale Installation, keine Git-Metadaten),
erfolgen Updates über den Paketmanager-Ablauf in [Aktualisieren](https://docs.openclaw.ai/de/install/updating).

## [​](https://docs.openclaw.ai/de/cli/update\#verwendung)  Verwendung

```
openclaw update
openclaw update status
openclaw update wizard
openclaw update --channel beta
openclaw update --channel dev
openclaw update --tag beta
openclaw update --tag main
openclaw update --dry-run
openclaw update --no-restart
openclaw update --yes
openclaw update --json
openclaw --update
```

## [​](https://docs.openclaw.ai/de/cli/update\#optionen)  Optionen

- `--no-restart`: Überspringt den Neustart des Gateway-Dienstes nach einem erfolgreichen Update. Paketmanager-Updates, die den Gateway neu starten, prüfen vor dem erfolgreichen Abschluss des Befehls, ob der neu gestartete Dienst die erwartete aktualisierte Version meldet.
- `--channel <stable|beta|dev>`: Legt den Update-Kanal fest (Git + npm; wird in der Konfiguration gespeichert).
- `--tag <dist-tag|version|spec>`: Überschreibt das Paketziel nur für dieses Update. Bei Paketinstallationen wird `main` auf `github:openclaw/openclaw#main` abgebildet.
- `--dry-run`: Zeigt geplante Update-Aktionen (Kanal/Tag/Ziel/Neustart-Ablauf) an, ohne Konfiguration zu schreiben, zu installieren, Plugins zu synchronisieren oder neu zu starten.
- `--json`: Gibt maschinenlesbares `UpdateRunResult`-JSON aus, einschließlich
`postUpdate.plugins.integrityDrifts`, wenn beim Plugin-Sync nach dem Update
Abweichungen bei npm-Plugin-Artefakten erkannt werden.
- `--timeout <seconds>`: Zeitlimit pro Schritt (Standard ist 1800 s).
- `--yes`: Überspringt Bestätigungsabfragen (zum Beispiel eine Downgrade-Bestätigung).

Downgrades erfordern eine Bestätigung, da ältere Versionen die Konfiguration beschädigen können.

## [​](https://docs.openclaw.ai/de/cli/update\#update-status)  `update status`

Zeigt den aktiven Update-Kanal + Git-Tag/Branch/SHA (bei Source-Checkouts) sowie die Update-Verfügbarkeit an.

```
openclaw update status
openclaw update status --json
openclaw update status --timeout 10
```

Optionen:

- `--json`: Gibt maschinenlesbares Status-JSON aus.
- `--timeout <seconds>`: Zeitlimit für Prüfungen (Standard ist 3 s).

## [​](https://docs.openclaw.ai/de/cli/update\#update-wizard)  `update wizard`

Interaktiver Ablauf zur Auswahl eines Update-Kanals und zur Bestätigung, ob der Gateway
nach dem Update neu gestartet werden soll (Standard ist Neustart). Wenn Sie `dev` ohne Git-Checkout auswählen,
wird angeboten, einen zu erstellen.Optionen:

- `--timeout <seconds>`: Zeitlimit für jeden Update-Schritt (Standard `1800`)

## [​](https://docs.openclaw.ai/de/cli/update\#was-es-macht)  Was es macht

Wenn Sie Kanäle explizit wechseln (`--channel ...`), hält OpenClaw auch die
Installationsmethode passend:

- `dev` → stellt einen Git-Checkout sicher (Standard: `~/openclaw`, überschreibbar mit `OPENCLAW_GIT_DIR`),
aktualisiert ihn und installiert die globale CLI aus diesem Checkout.
- `stable` → installiert aus npm mit `latest`.
- `beta` → bevorzugt den npm-dist-tag `beta`, fällt aber auf `latest` zurück, wenn Beta
fehlt oder älter als die aktuelle Stable-Version ist.

Der automatische Updater des Gateway-Kerns (wenn per Konfiguration aktiviert) startet den CLI-Update-Pfad
außerhalb des laufenden Gateway-Request-Handlers. Control-Plane-`update.run`-Paketmanager-Updates
erzwingen nach dem Pakettausch einen nicht verzögerten Update-Neustart ohne Abklingzeit,
da der alte Gateway-Prozess noch speicherinterne Chunks enthalten kann, die auf
Dateien zeigen, die durch das neue Paket entfernt wurden.Bei Paketmanager-Installationen löst `openclaw update` die Zielpaketversion auf,
bevor der Paketmanager aufgerufen wird. Globale npm-Installationen verwenden eine gestufte
Installation: OpenClaw installiert das neue Paket in ein temporäres npm-Präfix, prüft
dort das paketierte `dist`-Inventar und tauscht diesen sauberen Paketbaum anschließend in das
tatsächliche globale Präfix ein. Wenn die Prüfung fehlschlägt, werden Doctor nach dem Update,
Plugin-Sync und Neustartarbeiten nicht aus dem verdächtigen Baum ausgeführt. Selbst wenn die installierte Version
bereits dem Ziel entspricht, aktualisiert der Befehl die globale Paketinstallation,
führt dann Plugin-Sync, eine Aktualisierung der Core-Command-Vervollständigung und Neustartarbeiten aus. Dadurch
bleiben paketierte Sidecars und kanaleigene Plugin-Datensätze mit dem
installierten OpenClaw-Build synchron, während vollständige Neuaufbauten der Plugin-Command-Vervollständigung
expliziten `openclaw completion --write-state`-Ausführungen vorbehalten bleiben.Wenn ein lokaler verwalteter Gateway-Dienst installiert und der Neustart aktiviert ist,
stoppen Paketmanager-Updates den laufenden Dienst, bevor der Paketbaum ersetzt wird,
aktualisieren anschließend die Dienstmetadaten aus der aktualisierten Installation, starten den
Dienst neu und prüfen, ob der neu gestartete Gateway die erwartete Version meldet. Mit
`--no-restart` wird der Paketersatz weiterhin ausgeführt, aber der verwaltete Dienst wird nicht
gestoppt oder neu gestartet, sodass der laufende Gateway alten Code behalten kann, bis Sie ihn
manuell neu starten.

## [​](https://docs.openclaw.ai/de/cli/update\#git-checkout-ablauf)  Git-Checkout-Ablauf

### [​](https://docs.openclaw.ai/de/cli/update\#kanalauswahl)  Kanalauswahl

- `stable`: Checkt das neueste Nicht-Beta-Tag aus, dann Build und Doctor.
- `beta`: Bevorzugt das neueste `-beta`-Tag, fällt aber auf das neueste Stable-Tag zurück, wenn Beta fehlt oder älter ist.
- `dev`: Checkt `main` aus, dann Fetch und Rebase.

### [​](https://docs.openclaw.ai/de/cli/update\#update-schritte)  Update-Schritte

1

[Navigate to header](https://docs.openclaw.ai/de/cli/update#)

Verify clean worktree

Erfordert, dass keine uncommitted Änderungen vorhanden sind.

2

[Navigate to header](https://docs.openclaw.ai/de/cli/update#)

Switch channel

Wechselt zum ausgewählten Kanal (Tag oder Branch).

3

[Navigate to header](https://docs.openclaw.ai/de/cli/update#)

Fetch upstream

Nur Dev.

4

[Navigate to header](https://docs.openclaw.ai/de/cli/update#)

Preflight build (dev only)

Führt Lint und TypeScript-Build in einem temporären Worktree aus. Wenn die Spitze fehlschlägt, geht es bis zu 10 Commits zurück, um den neuesten sauberen Build zu finden.

5

[Navigate to header](https://docs.openclaw.ai/de/cli/update#)

Rebase

Führt ein Rebase auf den ausgewählten Commit aus (nur Dev).

6

[Navigate to header](https://docs.openclaw.ai/de/cli/update#)

Install dependencies

Verwendet den Paketmanager des Repos. Bei pnpm-Checkouts bootstrapped der Updater `pnpm` bei Bedarf (zuerst über `corepack`, dann als Fallback mit einem temporären `npm install pnpm@10`), statt `npm run build` innerhalb eines pnpm-Workspaces auszuführen.

7

[Navigate to header](https://docs.openclaw.ai/de/cli/update#)

Build Control UI

Baut den Gateway und die Control UI.

8

[Navigate to header](https://docs.openclaw.ai/de/cli/update#)

Run doctor

`openclaw doctor` wird als abschließende Safe-Update-Prüfung ausgeführt.

9

[Navigate to header](https://docs.openclaw.ai/de/cli/update#)

Sync plugins

Synchronisiert Plugins mit dem aktiven Kanal. Dev verwendet gebündelte Plugins; stable und beta verwenden npm. Aktualisiert über npm installierte Plugins.

Wenn ein exakt gepinntes npm-Plugin-Update auf ein Artefakt auflöst, dessen Integrität vom gespeicherten Installationsdatensatz abweicht, bricht `openclaw update` dieses Plugin-Artefakt-Update ab, statt es zu installieren. Installieren oder aktualisieren Sie das Plugin explizit erst, nachdem Sie geprüft haben, dass Sie dem neuen Artefakt vertrauen.

Fehler beim Plugin-Sync nach dem Update lassen das Update-Ergebnis fehlschlagen und stoppen nachgelagerte Neustartarbeiten. Beheben Sie den Installations- oder Update-Fehler des Plugins und führen Sie dann `openclaw update` erneut aus.Wenn der aktualisierte Gateway startet, ist das Plugin-Laden nur prüfend: Beim Start werden keine Paketmanager ausgeführt und keine Abhängigkeitsbäume verändert. Paketmanager-`update.run`-Neustarts umgehen die normale Leerlaufverzögerung und Neustart-Abklingzeit, nachdem der Paketbaum ausgetauscht wurde, sodass der alte Prozess entfernte Chunks nicht weiter lazy-loaden kann.Wenn der pnpm-Bootstrap weiterhin fehlschlägt, stoppt der Updater frühzeitig mit einem paketmanager-spezifischen Fehler, statt `npm run build` innerhalb des Checkouts zu versuchen.

## [​](https://docs.openclaw.ai/de/cli/update\#update-kurzform)  `--update`-Kurzform

`openclaw --update` wird zu `openclaw update` umgeschrieben (nützlich für Shells und Launcher-Skripte).

## [​](https://docs.openclaw.ai/de/cli/update\#verwandt)  Verwandt

- `openclaw doctor` (bietet bei Git-Checkouts an, zuerst ein Update auszuführen)
- [Entwicklungskanäle](https://docs.openclaw.ai/de/install/development-channels)
- [Aktualisieren](https://docs.openclaw.ai/de/install/updating)
- [CLI-Referenz](https://docs.openclaw.ai/de/cli)

[Deinstallation](https://docs.openclaw.ai/de/cli/uninstall) [Agent](https://docs.openclaw.ai/de/cli/agent)

Ctrl+I