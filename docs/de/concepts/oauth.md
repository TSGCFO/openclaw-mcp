---
source_url: https://docs.openclaw.ai/de/concepts/oauth
title: "OAuth - OpenClaw"
---

[Zum Hauptinhalt springen](https://docs.openclaw.ai/de/concepts/oauth#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/de)

![DE](https://d3gk2c5xim1je2.cloudfront.net/flags/DE.svg)

Deutsch

Suchen...

Ctrl K

Suchen...

Navigation

Fundamentals

OAuth

[Get started](https://docs.openclaw.ai/de) [Install](https://docs.openclaw.ai/de/install) [Channels](https://docs.openclaw.ai/de/channels) [Agents](https://docs.openclaw.ai/de/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/de/tools) [Models](https://docs.openclaw.ai/de/providers) [Platforms](https://docs.openclaw.ai/de/platforms) [Gateway & Ops](https://docs.openclaw.ai/de/gateway) [Reference](https://docs.openclaw.ai/de/cli) [Help](https://docs.openclaw.ai/de/help)

Auf dieser Seite

- [Der Token Sink (warum er existiert)](https://docs.openclaw.ai/de/concepts/oauth#der-token-sink-warum-er-existiert)
- [Speicherung (wo Tokens liegen)](https://docs.openclaw.ai/de/concepts/oauth#speicherung-wo-tokens-liegen)
- [Kompatibilität mit Anthropic Legacy Tokens](https://docs.openclaw.ai/de/concepts/oauth#kompatibilit%C3%A4t-mit-anthropic-legacy-tokens)
- [Anthropic Claude CLI-Migration](https://docs.openclaw.ai/de/concepts/oauth#anthropic-claude-cli-migration)
- [OAuth-Austausch (wie der Login funktioniert)](https://docs.openclaw.ai/de/concepts/oauth#oauth-austausch-wie-der-login-funktioniert)
- [Anthropic Setup Token](https://docs.openclaw.ai/de/concepts/oauth#anthropic-setup-token)
- [OpenAI Codex (ChatGPT OAuth)](https://docs.openclaw.ai/de/concepts/oauth#openai-codex-chatgpt-oauth)
- [Refresh + Ablauf](https://docs.openclaw.ai/de/concepts/oauth#refresh-%2B-ablauf)
- [Mehrere Konten (Profile) + Routing](https://docs.openclaw.ai/de/concepts/oauth#mehrere-konten-profile-%2B-routing)
- [1) Bevorzugt: separate Agenten](https://docs.openclaw.ai/de/concepts/oauth#1-bevorzugt-separate-agenten)
- [2) Fortgeschritten: mehrere Profile in einem Agenten](https://docs.openclaw.ai/de/concepts/oauth#2-fortgeschritten-mehrere-profile-in-einem-agenten)
- [Verwandte Themen](https://docs.openclaw.ai/de/concepts/oauth#verwandte-themen)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw unterstützt „Subscription Auth“ per OAuth für Provider, die dies anbieten
(insbesondere **OpenAI Codex (ChatGPT OAuth)**). Für Anthropic gilt praktisch
nun folgende Aufteilung:

- **Anthropic-API-Schlüssel**: normale Anthropic-API-Abrechnung
- **Anthropic Claude CLI / Subscription Auth innerhalb von OpenClaw**: Anthropic-Mitarbeiter
haben uns mitgeteilt, dass diese Nutzung wieder erlaubt ist

OpenAI Codex OAuth wird ausdrücklich für die Verwendung in externen Tools wie
OpenClaw unterstützt. Diese Seite erklärt:Für Anthropic in der Produktion ist die Authentifizierung per API-Schlüssel der sicherere empfohlene Weg.

- wie der OAuth- **Token-Austausch** funktioniert (PKCE)
- wo Tokens **gespeichert** werden (und warum)
- wie Sie **mehrere Konten** handhaben (Profile + sitzungsbezogene Overrides)

OpenClaw unterstützt außerdem **Provider-Plugins**, die eigene OAuth- oder API-Schlüssel-
Flows mitbringen. Führen Sie sie aus mit:

```
openclaw models auth login --provider <id>
```

## [​](https://docs.openclaw.ai/de/concepts/oauth\#der-token-sink-warum-er-existiert)  Der Token Sink (warum er existiert)

OAuth-Provider stellen während Login-/Refresh-Flows häufig ein **neues Refresh Token** aus. Einige Provider (oder OAuth-Clients) können ältere Refresh Tokens ungültig machen, wenn für dieselbe Benutzer/App-Kombination ein neues ausgegeben wird.Praktisches Symptom:

- Sie melden sich über OpenClaw _und_ über Claude Code / Codex CLI an → eines davon wird später zufällig „abgemeldet“

Um das zu reduzieren, behandelt OpenClaw `auth-profiles.json` als **Token Sink**:

- die Runtime liest Zugangsdaten aus **einer Stelle**
- wir können mehrere Profile verwalten und deterministisch routen
- die Wiederverwendung externer CLIs ist Provider-spezifisch: Codex CLI kann ein leeres
`openai-codex:default`-Profil initialisieren, aber sobald OpenClaw ein lokales OAuth-Profil hat,
ist das lokale Refresh Token kanonisch; andere Integrationen können weiterhin
extern verwaltet bleiben und ihren CLI-Auth-Speicher erneut lesen
- Status- und Startpfade, die den konfigurierten Provider-Satz bereits kennen, beschränken
die Erkennung externer CLIs auf diesen Satz, sodass ein nicht zugehöriger CLI-Login-Speicher
bei einem Setup mit nur einem Provider nicht geprüft wird

## [​](https://docs.openclaw.ai/de/concepts/oauth\#speicherung-wo-tokens-liegen)  Speicherung (wo Tokens liegen)

Secrets werden in Auth-Speichern von Agenten gespeichert:

- Auth-Profile (OAuth + API-Schlüssel + optionale wertbezogene Refs): `~/.openclaw/agents/<agentId>/agent/auth-profiles.json`
- Legacy-Kompatibilitätsdatei: `~/.openclaw/agents/<agentId>/agent/auth.json`
(statische `api_key`-Einträge werden bereinigt, wenn sie entdeckt werden)

Legacy-Datei nur für den Import (weiterhin unterstützt, aber nicht der Hauptspeicher):

- `~/.openclaw/credentials/oauth.json` (wird bei der ersten Verwendung in `auth-profiles.json` importiert)

Alle oben genannten Speicher respektieren auch `$OPENCLAW_STATE_DIR` (Override für das Zustandsverzeichnis). Vollständige Referenz: [/gateway/configuration](https://docs.openclaw.ai/de/gateway/configuration-reference#auth-storage)Für statische Secret-Refs und das Aktivierungsverhalten von Runtime-Snapshots siehe [Secrets-Verwaltung](https://docs.openclaw.ai/de/gateway/secrets).Wenn ein sekundärer Agent kein lokales Auth-Profil hat, verwendet OpenClaw Read-through-
Vererbung aus dem Speicher des Standard-/Haupt-Agenten. Die `auth-profiles.json` des Haupt-
Agenten wird beim Lesen nicht geklont. OAuth Refresh Tokens sind besonders
sensibel: Normale Kopier-Flows überspringen sie standardmäßig, weil einige Provider Refresh Tokens nach der Verwendung rotieren
oder ungültig machen. Konfigurieren Sie für einen Agenten einen separaten OAuth-Login,
wenn er ein unabhängiges Konto benötigt.

## [​](https://docs.openclaw.ai/de/concepts/oauth\#kompatibilit%C3%A4t-mit-anthropic-legacy-tokens)  Kompatibilität mit Anthropic Legacy Tokens

Die öffentlichen Claude Code-Dokumente von Anthropic besagen, dass die direkte Nutzung von Claude Code innerhalb
der Claude-Abonnementlimits bleibt, und Anthropic-Mitarbeiter haben uns mitgeteilt, dass die Claude-
CLI-Nutzung im OpenClaw-Stil wieder erlaubt ist. OpenClaw behandelt daher die Wiederverwendung der Claude CLI und
die Nutzung von `claude -p` für diese Integration als genehmigt, sofern Anthropic
keine neue Richtlinie veröffentlicht.Die aktuellen Dokumente von Anthropic zu direkten Claude-Code-Tarifen finden Sie unter [Claude Code\\
mit Ihrem Pro- oder Max-\\
Tarif verwenden](https://support.claude.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan)
und [Claude Code mit Ihrem Team- oder Enterprise-\\
Tarif verwenden](https://support.anthropic.com/en/articles/11845131-using-claude-code-with-your-team-or-enterprise-plan/).Wenn Sie weitere abonnementartige Optionen in OpenClaw möchten, siehe [OpenAI\\
Codex](https://docs.openclaw.ai/de/providers/openai), [Qwen Cloud Coding\\
Plan](https://docs.openclaw.ai/de/providers/qwen), [MiniMax Coding Plan](https://docs.openclaw.ai/de/providers/minimax)
und [Z.AI / GLM Coding Plan](https://docs.openclaw.ai/de/providers/glm).

OpenClaw stellt außerdem Anthropic Setup Token als unterstützten Token-Auth-Pfad bereit, bevorzugt nun aber die Wiederverwendung der Claude CLI und `claude -p`, wenn verfügbar.

## [​](https://docs.openclaw.ai/de/concepts/oauth\#anthropic-claude-cli-migration)  Anthropic Claude CLI-Migration

OpenClaw unterstützt die Wiederverwendung der Anthropic Claude CLI wieder. Wenn Sie bereits einen lokalen
Claude-Login auf dem Host haben, kann Onboarding/Konfiguration ihn direkt wiederverwenden.

## [​](https://docs.openclaw.ai/de/concepts/oauth\#oauth-austausch-wie-der-login-funktioniert)  OAuth-Austausch (wie der Login funktioniert)

Die interaktiven Login-Flows von OpenClaw sind in `@mariozechner/pi-ai` implementiert und in die Assistenten/Befehle eingebunden.

### [​](https://docs.openclaw.ai/de/concepts/oauth\#anthropic-setup-token)  Anthropic Setup Token

Flow-Struktur:

1. Anthropic Setup Token starten oder Token aus OpenClaw einfügen
2. OpenClaw speichert die resultierenden Anthropic-Zugangsdaten in einem Auth-Profil
3. die Modellauswahl bleibt auf `anthropic/...`
4. vorhandene Anthropic-Auth-Profile bleiben für Rollback-/Reihenfolgensteuerung verfügbar

### [​](https://docs.openclaw.ai/de/concepts/oauth\#openai-codex-chatgpt-oauth)  OpenAI Codex (ChatGPT OAuth)

OpenAI Codex OAuth wird ausdrücklich für die Verwendung außerhalb der Codex CLI unterstützt, einschließlich OpenClaw-Workflows.Flow-Struktur (PKCE):

1. PKCE-Verifier/Challenge + zufälligen `state` generieren
2. `https://auth.openai.com/oauth/authorize?...` öffnen
3. versuchen, den Callback unter `http://127.0.0.1:1455/auth/callback` zu erfassen
4. wenn der Callback nicht gebunden werden kann (oder Sie remote/headless arbeiten), die Redirect-URL/den Code einfügen
5. Austausch über `https://auth.openai.com/oauth/token`
6. `accountId` aus dem Access Token extrahieren und `{ access, refresh, expires, accountId }` speichern

Der Assistentenpfad ist `openclaw onboard` → Auth-Auswahl `openai-codex`.

## [​](https://docs.openclaw.ai/de/concepts/oauth\#refresh-+-ablauf)  Refresh + Ablauf

Profile speichern einen `expires`-Zeitstempel.Zur Runtime:

- wenn `expires` in der Zukunft liegt → gespeichertes Access Token verwenden
- wenn abgelaufen → aktualisieren (unter einer Dateisperre) und die gespeicherten Zugangsdaten überschreiben
- wenn ein sekundärer Agent ein geerbtes OAuth-Profil des Haupt-Agenten liest, schreibt der Refresh
zurück in den Speicher des Haupt-Agenten, statt das Refresh Token in
den Speicher des sekundären Agenten zu kopieren
- Ausnahme: Einige externe CLI-Zugangsdaten bleiben extern verwaltet; OpenClaw
liest diese CLI-Auth-Speicher erneut, statt kopierte Refresh Tokens zu verbrauchen.
Das Codex-CLI-Bootstrapping ist absichtlich enger gefasst: Es legt ein leeres
`openai-codex:default`-Profil an, danach halten OpenClaw-eigene Refreshes das lokale
Profil kanonisch.

Der Refresh-Flow läuft automatisch; in der Regel müssen Sie Tokens nicht manuell verwalten.

## [​](https://docs.openclaw.ai/de/concepts/oauth\#mehrere-konten-profile-+-routing)  Mehrere Konten (Profile) + Routing

Zwei Muster:

### [​](https://docs.openclaw.ai/de/concepts/oauth\#1-bevorzugt-separate-agenten)  1) Bevorzugt: separate Agenten

Wenn „persönlich“ und „Arbeit“ nie interagieren sollen, verwenden Sie isolierte Agenten (separate Sitzungen + Zugangsdaten + Workspace):

```
openclaw agents add work
openclaw agents add personal
```

Konfigurieren Sie dann die Authentifizierung pro Agent (Assistent) und routen Sie Chats zum richtigen Agenten.

### [​](https://docs.openclaw.ai/de/concepts/oauth\#2-fortgeschritten-mehrere-profile-in-einem-agenten)  2) Fortgeschritten: mehrere Profile in einem Agenten

`auth-profiles.json` unterstützt mehrere Profil-IDs für denselben Provider.Auswahl des verwendeten Profils:

- global über die Konfigurationsreihenfolge (`auth.order`)
- sitzungsbezogen über `/model ...@<profileId>`

Beispiel (Sitzungs-Override):

- `/model Opus@anthropic:work`

So sehen Sie, welche Profil-IDs existieren:

- `openclaw channels list --json` (zeigt `auth[]`)

Zugehörige Dokumente:

- [Modell-Failover](https://docs.openclaw.ai/de/concepts/model-failover) (Rotations- + Cooldown-Regeln)
- [Slash-Befehle](https://docs.openclaw.ai/de/tools/slash-commands) (Befehlsoberfläche)

## [​](https://docs.openclaw.ai/de/concepts/oauth\#verwandte-themen)  Verwandte Themen

- [Authentifizierung](https://docs.openclaw.ai/de/gateway/authentication) — Überblick über Authentifizierung für Modell-Provider
- [Secrets](https://docs.openclaw.ai/de/gateway/secrets) — Speicherung von Zugangsdaten und SecretRef
- [Konfigurationsreferenz](https://docs.openclaw.ai/de/gateway/configuration-reference#auth-storage) — Auth-Konfigurationsschlüssel

[Leitfaden zur Persönlichkeit in \`SOUL.md\`](https://docs.openclaw.ai/de/concepts/soul) [Bootstrapping](https://docs.openclaw.ai/de/start/bootstrapping)

Ctrl+I