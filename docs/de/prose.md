---
source_url: https://docs.openclaw.ai/de/prose
title: "OpenProse - OpenClaw"
---

[Zum Hauptinhalt springen](https://docs.openclaw.ai/de/prose#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/de)

![DE](https://d3gk2c5xim1je2.cloudfront.net/flags/DE.svg)

Deutsch

Suchen...

Ctrl K

Suchen...

Navigation

Skills

OpenProse

[Get started](https://docs.openclaw.ai/de) [Install](https://docs.openclaw.ai/de/install) [Channels](https://docs.openclaw.ai/de/channels) [Agents](https://docs.openclaw.ai/de/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/de/tools) [Models](https://docs.openclaw.ai/de/providers) [Platforms](https://docs.openclaw.ai/de/platforms) [Gateway & Ops](https://docs.openclaw.ai/de/gateway) [Reference](https://docs.openclaw.ai/de/cli) [Help](https://docs.openclaw.ai/de/help)

Auf dieser Seite

- [Was es kann](https://docs.openclaw.ai/de/prose#was-es-kann)
- [Installieren + aktivieren](https://docs.openclaw.ai/de/prose#installieren-%2B-aktivieren)
- [Slash-Befehl](https://docs.openclaw.ai/de/prose#slash-befehl)
- [Beispiel: eine einfache .prose-Datei](https://docs.openclaw.ai/de/prose#beispiel-eine-einfache-prose-datei)
- [Speicherorte von Dateien](https://docs.openclaw.ai/de/prose#speicherorte-von-dateien)
- [Statusmodi](https://docs.openclaw.ai/de/prose#statusmodi)
- [Remote-Programme](https://docs.openclaw.ai/de/prose#remote-programme)
- [Abbildung auf die OpenClaw-Laufzeit](https://docs.openclaw.ai/de/prose#abbildung-auf-die-openclaw-laufzeit)
- [Sicherheit + Freigaben](https://docs.openclaw.ai/de/prose#sicherheit-%2B-freigaben)
- [Verwandt](https://docs.openclaw.ai/de/prose#verwandt)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenProse ist ein portables, markdown-first Workflow-Format zur Orchestrierung von AI-Sitzungen. In OpenClaw wird es als Plugin mitgeliefert, das ein OpenProse-Skill-Pack plus einen Slash-Befehl `/prose` installiert. Programme leben in `.prose`-Dateien und können mehrere Subagents mit explizitem Kontrollfluss starten.Offizielle Website: [https://www.prose.md](https://www.prose.md/)

## [​](https://docs.openclaw.ai/de/prose\#was-es-kann)  Was es kann

- Multi-Agent-Recherche + Synthese mit explizitem Parallelismus.
- Wiederholbare, freigabesichere Workflows (Code Review, Incident-Triage, Content-Pipelines).
- Wiederverwendbare `.prose`-Programme, die Sie über unterstützte Agent-Laufzeitumgebungen hinweg ausführen können.

## [​](https://docs.openclaw.ai/de/prose\#installieren-+-aktivieren)  Installieren + aktivieren

Gebündelte Plugins sind standardmäßig deaktiviert. Aktivieren Sie OpenProse:

```
openclaw plugins enable open-prose
```

Starten Sie das Gateway nach dem Aktivieren des Plugins neu.Dev/lokaler Checkout: `openclaw plugins install ./path/to/local/open-prose-plugin`Verwandte Dokumentation: [Plugins](https://docs.openclaw.ai/de/tools/plugin), [Plugin manifest](https://docs.openclaw.ai/de/plugins/manifest), [Skills](https://docs.openclaw.ai/de/tools/skills).

## [​](https://docs.openclaw.ai/de/prose\#slash-befehl)  Slash-Befehl

OpenProse registriert `/prose` als vom Benutzer aufrufbaren Skill-Befehl. Er leitet an die VM-Anweisungen von OpenProse weiter und verwendet unter der Haube OpenClaw-Tools.Häufige Befehle:

```
/prose help
/prose run <file.prose>
/prose run <handle/slug>
/prose run <https://example.com/file.prose>
/prose compile <file.prose>
/prose examples
/prose update
```

## [​](https://docs.openclaw.ai/de/prose\#beispiel-eine-einfache-prose-datei)  Beispiel: eine einfache `.prose`-Datei

```
# Recherche + Synthese mit zwei Agenten, die parallel laufen.

input topic: "What should we research?"

agent researcher:
  model: sonnet
  prompt: "You research thoroughly and cite sources."

agent writer:
  model: opus
  prompt: "You write a concise summary."

parallel:
  findings = session: researcher
    prompt: "Research {topic}."
  draft = session: writer
    prompt: "Summarize {topic}."

session "Merge the findings + draft into a final answer."
context: { findings, draft }
```

## [​](https://docs.openclaw.ai/de/prose\#speicherorte-von-dateien)  Speicherorte von Dateien

OpenProse hält den Status unter `.prose/` in Ihrem Workspace:

```
.prose/
├── .env
├── runs/
│   └── {YYYYMMDD}-{HHMMSS}-{random}/
│       ├── program.prose
│       ├── state.md
│       ├── bindings/
│       └── agents/
└── agents/
```

Persistente Agenten auf Benutzerebene liegen unter:

```
~/.prose/agents/
```

## [​](https://docs.openclaw.ai/de/prose\#statusmodi)  Statusmodi

OpenProse unterstützt mehrere Status-Backends:

- **filesystem** (Standard): `.prose/runs/...`
- **in-context**: transient, für kleine Programme
- **sqlite** (experimentell): erfordert `sqlite3`-Binärdatei
- **postgres** (experimentell): erfordert `psql` und einen Connection String

Hinweise:

- sqlite/postgres sind Opt-in und experimentell.
- postgres-Zugangsdaten fließen in Subagent-Logs ein; verwenden Sie eine dedizierte Datenbank mit minimalen Rechten.

## [​](https://docs.openclaw.ai/de/prose\#remote-programme)  Remote-Programme

`/prose run <handle/slug>` wird auf `https://p.prose.md/<handle>/<slug>` aufgelöst.
Direkte URLs werden unverändert abgerufen. Dies verwendet das Tool `web_fetch` (oder `exec` für POST).

## [​](https://docs.openclaw.ai/de/prose\#abbildung-auf-die-openclaw-laufzeit)  Abbildung auf die OpenClaw-Laufzeit

OpenProse-Programme werden auf OpenClaw-Primitive abgebildet:

| OpenProse-Konzept | OpenClaw-Tool |
| --- | --- |
| Sitzung starten / Task-Tool | `sessions_spawn` |
| Datei lesen/schreiben | `read` / `write` |
| Web-Fetch | `web_fetch` |

Wenn Ihre Tool-Allowlist diese Tools blockiert, schlagen OpenProse-Programme fehl. Siehe [Skills config](https://docs.openclaw.ai/de/tools/skills-config).

## [​](https://docs.openclaw.ai/de/prose\#sicherheit-+-freigaben)  Sicherheit + Freigaben

Behandeln Sie `.prose`-Dateien wie Code. Prüfen Sie sie vor der Ausführung. Verwenden Sie Tool-Allowlists und Freigabegates von OpenClaw, um Nebenwirkungen zu kontrollieren.Für deterministische, freigabegesteuerte Workflows vergleichen Sie mit [Lobster](https://docs.openclaw.ai/de/tools/lobster).

## [​](https://docs.openclaw.ai/de/prose\#verwandt)  Verwandt

- [Text-to-speech](https://docs.openclaw.ai/de/tools/tts)
- [Markdown formatting](https://docs.openclaw.ai/de/concepts/markdown-formatting)

[ClawHub](https://docs.openclaw.ai/de/tools/clawhub) [Automatisierung & Aufgaben](https://docs.openclaw.ai/de/automation)

Ctrl+I