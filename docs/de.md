---
source_url: https://docs.openclaw.ai/de
title: "OpenClaw - OpenClaw"
---

[Zum Hauptinhalt springen](https://docs.openclaw.ai/de#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/de)

![DE](https://d3gk2c5xim1je2.cloudfront.net/flags/DE.svg)

Deutsch

Suchen...

Ctrl K

Suchen...

Navigation

Overview

OpenClaw

[Get started](https://docs.openclaw.ai/de) [Install](https://docs.openclaw.ai/de/install) [Channels](https://docs.openclaw.ai/de/channels) [Agents](https://docs.openclaw.ai/de/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/de/tools) [Models](https://docs.openclaw.ai/de/providers) [Platforms](https://docs.openclaw.ai/de/platforms) [Gateway & Ops](https://docs.openclaw.ai/de/gateway) [Reference](https://docs.openclaw.ai/de/cli) [Help](https://docs.openclaw.ai/de/help)

Auf dieser Seite

- [OpenClaw 🦞](https://docs.openclaw.ai/de#openclaw-)
- [Was ist OpenClaw?](https://docs.openclaw.ai/de#was-ist-openclaw)
- [So funktioniert es](https://docs.openclaw.ai/de#so-funktioniert-es)
- [Hauptfunktionen](https://docs.openclaw.ai/de#hauptfunktionen)
- [Schnellstart](https://docs.openclaw.ai/de#schnellstart)
- [Dashboard](https://docs.openclaw.ai/de#dashboard)
- [Konfiguration (optional)](https://docs.openclaw.ai/de#konfiguration-optional)
- [Hier starten](https://docs.openclaw.ai/de#hier-starten)
- [Mehr erfahren](https://docs.openclaw.ai/de#mehr-erfahren)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/de\#openclaw-)  OpenClaw 🦞

![OpenClaw](https://mintcdn.com/clawdhub/-t5HSeZ3Y_0_wH4i/assets/openclaw-logo-text-dark.png?fit=max&auto=format&n=-t5HSeZ3Y_0_wH4i&q=85&s=61797dcb0c37d6e9279b8c5ad2e850e4)![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/assets/openclaw-logo-text.png?fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=d799bea41acb92d4c9fd1075c575879f)

> _“EXFOLIATE! EXFOLIATE!”_ — Wahrscheinlich ein Weltraumhummer

**Gateway für KI-Agenten auf jedem Betriebssystem über Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo und mehr.**

Senden Sie eine Nachricht und erhalten Sie eine Agentenantwort direkt in Ihre Tasche. Betreiben Sie ein einziges Gateway über integrierte Channels, gebündelte Channel-Plugins, WebChat und mobile Nodes.

[**Erste Schritte** \\
\\
Installieren Sie OpenClaw und starten Sie das Gateway in wenigen Minuten.](https://docs.openclaw.ai/de/start/getting-started)

[**Onboarding ausführen** \\
\\
Geführte Einrichtung mit `openclaw onboard` und Pairing-Abläufen.](https://docs.openclaw.ai/de/start/wizard)

[**Die Control UI öffnen** \\
\\
Starten Sie das Browser-Dashboard für Chat, Konfiguration und Sitzungen.](https://docs.openclaw.ai/web/control-ui)

## [​](https://docs.openclaw.ai/de\#was-ist-openclaw)  Was ist OpenClaw?

OpenClaw ist ein **selbst gehostetes Gateway**, das Ihre bevorzugten Chat-Apps und Channel-Oberflächen — integrierte Channels plus gebündelte oder externe Channel-Plugins wie Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo und mehr — mit KI-Coding-Agenten wie Pi verbindet. Sie betreiben einen einzigen Gateway-Prozess auf Ihrer eigenen Maschine (oder einem Server), und er wird zur Brücke zwischen Ihren Messaging-Apps und einem jederzeit verfügbaren KI-Assistenten.**Für wen ist es gedacht?** Entwickler und Power-User, die einen persönlichen KI-Assistenten möchten, dem sie von überall aus Nachrichten senden können — ohne die Kontrolle über ihre Daten aufzugeben oder sich auf einen gehosteten Dienst zu verlassen.**Was macht es anders?**

- **Selbst gehostet**: läuft auf Ihrer Hardware, nach Ihren Regeln
- **Multi-Channel**: ein Gateway bedient gleichzeitig integrierte Channels plus gebündelte oder externe Channel-Plugins
- **Agent-native**: entwickelt für Coding-Agenten mit Tool-Nutzung, Sitzungen, Speicher und Multi-Agent-Routing
- **Open Source**: MIT-lizenziert, von der Community getragen

**Was benötigen Sie?** Node 24 (empfohlen) oder Node 22 LTS (`22.14+`) zur Kompatibilität, einen API-Key Ihres gewählten Providers und 5 Minuten. Für beste Qualität und Sicherheit verwenden Sie das stärkste Modell der neuesten Generation, das Ihnen zur Verfügung steht.

## [​](https://docs.openclaw.ai/de\#so-funktioniert-es)  So funktioniert es

Das Gateway ist die einzige Quelle der Wahrheit für Sitzungen, Routing und Channel-Verbindungen.

## [​](https://docs.openclaw.ai/de\#hauptfunktionen)  Hauptfunktionen

[**Multi-Channel-Gateway** \\
\\
Discord, iMessage, Signal, Slack, Telegram, WhatsApp, WebChat und mehr mit einem einzigen Gateway-Prozess.](https://docs.openclaw.ai/de/channels)

[**Plugin-Channels** \\
\\
Gebündelte Plugins fügen in normalen aktuellen Releases Matrix, Nostr, Twitch, Zalo und mehr hinzu.](https://docs.openclaw.ai/de/tools/plugin)

[**Multi-Agent-Routing** \\
\\
Isolierte Sitzungen pro Agent, Workspace oder Absender.](https://docs.openclaw.ai/de/concepts/multi-agent)

[**Medienunterstützung** \\
\\
Senden und empfangen Sie Bilder, Audio und Dokumente.](https://docs.openclaw.ai/de/nodes/images)

[**Web-Control-UI** \\
\\
Browser-Dashboard für Chat, Konfiguration, Sitzungen und Nodes.](https://docs.openclaw.ai/web/control-ui)

[**Mobile Nodes** \\
\\
Koppeln Sie iOS- und Android-Nodes für Canvas-, Kamera- und sprachaktivierte Workflows.](https://docs.openclaw.ai/de/nodes)

## [​](https://docs.openclaw.ai/de\#schnellstart)  Schnellstart

1

[Navigate to header](https://docs.openclaw.ai/de#)

OpenClaw installieren

```
npm install -g openclaw@latest
```

2

[Navigate to header](https://docs.openclaw.ai/de#)

Onboarding durchführen und den Dienst installieren

```
openclaw onboard --install-daemon
```

3

[Navigate to header](https://docs.openclaw.ai/de#)

Chatten

Öffnen Sie die Control UI in Ihrem Browser und senden Sie eine Nachricht:

```
openclaw dashboard
```

Oder verbinden Sie einen Channel ( [Telegram](https://docs.openclaw.ai/de/channels/telegram) ist am schnellsten) und chatten Sie von Ihrem Telefon aus.

Benötigen Sie die vollständige Installations- und Entwicklungsumgebung? Siehe [Getting Started](https://docs.openclaw.ai/de/start/getting-started).

## [​](https://docs.openclaw.ai/de\#dashboard)  Dashboard

Öffnen Sie die browserbasierte Control UI, nachdem das Gateway gestartet wurde.

- Lokaler Standard: [http://127.0.0.1:18789/](http://127.0.0.1:18789/)
- Remote-Zugriff: [Web surfaces](https://docs.openclaw.ai/web) und [Tailscale](https://docs.openclaw.ai/de/gateway/tailscale)

![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/whatsapp-openclaw.jpg?fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=b74a3630b0e971f466eff15fbdc642cb)

## [​](https://docs.openclaw.ai/de\#konfiguration-optional)  Konfiguration (optional)

Die Konfiguration befindet sich unter `~/.openclaw/openclaw.json`.

- Wenn Sie **nichts tun**, verwendet OpenClaw die gebündelte Pi-Binärdatei im RPC-Modus mit Sitzungen pro Absender.
- Wenn Sie es stärker absichern möchten, beginnen Sie mit `channels.whatsapp.allowFrom` und (für Gruppen) Erwähnungsregeln.

Beispiel:

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

## [​](https://docs.openclaw.ai/de\#hier-starten)  Hier starten

[**Docs-Hubs** \\
\\
Alle Docs und Leitfäden, nach Anwendungsfall organisiert.](https://docs.openclaw.ai/de/start/hubs)

[**Konfiguration** \\
\\
Zentrale Gateway-Einstellungen, Tokens und Provider-Konfiguration.](https://docs.openclaw.ai/de/gateway/configuration)

[**Remote-Zugriff** \\
\\
SSH- und Tailnet-Zugriffsmuster.](https://docs.openclaw.ai/de/gateway/remote)

[**Channels** \\
\\
Channel-spezifische Einrichtung für Feishu, Microsoft Teams, WhatsApp, Telegram, Discord und mehr.](https://docs.openclaw.ai/de/channels/telegram)

[**Nodes** \\
\\
iOS- und Android-Nodes mit Pairing, Canvas, Kamera und Geräteaktionen.](https://docs.openclaw.ai/de/nodes)

[**Hilfe** \\
\\
Einstiegspunkt für häufige Lösungen und Fehlerbehebung.](https://docs.openclaw.ai/de/help)

## [​](https://docs.openclaw.ai/de\#mehr-erfahren)  Mehr erfahren

[**Vollständige Funktionsliste** \\
\\
Vollständige Channel-, Routing- und Medienfunktionen.](https://docs.openclaw.ai/de/concepts/features)

[**Multi-Agent-Routing** \\
\\
Workspace-Isolation und Sitzungen pro Agent.](https://docs.openclaw.ai/de/concepts/multi-agent)

[**Sicherheit** \\
\\
Tokens, Allowlists und Sicherheitskontrollen.](https://docs.openclaw.ai/de/gateway/security)

[**Fehlerbehebung** \\
\\
Gateway-Diagnose und häufige Fehler.](https://docs.openclaw.ai/de/gateway/troubleshooting)

[**Über das Projekt und Danksagungen** \\
\\
Projektursprung, Mitwirkende und Lizenz.](https://docs.openclaw.ai/de/reference/credits)

[Showcase](https://docs.openclaw.ai/de/start/showcase)

Ctrl+I

![OpenClaw](https://mintcdn.com/clawdhub/-t5HSeZ3Y_0_wH4i/assets/openclaw-logo-text-dark.png?w=1100&fit=max&auto=format&n=-t5HSeZ3Y_0_wH4i&q=85&s=ed926636a9752c9ce39acccf51c3b271)

![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/assets/openclaw-logo-text.png?w=1100&fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=88255cdd2554a6b341c89ae709743441)

![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/whatsapp-openclaw.jpg?w=1100&fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=72f5064ba581433011975bde37c74964)