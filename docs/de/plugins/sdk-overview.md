---
source_url: https://docs.openclaw.ai/de/plugins/sdk-overview
title: "Plugin-SDK-\u00dcbersicht - OpenClaw"
---

[Zum Hauptinhalt springen](https://docs.openclaw.ai/de/plugins/sdk-overview#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/de)

![DE](https://d3gk2c5xim1je2.cloudfront.net/flags/DE.svg)

Deutsch

Suchen...

Ctrl K

Suchen...

Navigation

Plugin SDK reference

Plugin-SDK-Übersicht

[Get started](https://docs.openclaw.ai/de) [Install](https://docs.openclaw.ai/de/install) [Channels](https://docs.openclaw.ai/de/channels) [Agents](https://docs.openclaw.ai/de/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/de/tools) [Models](https://docs.openclaw.ai/de/providers) [Platforms](https://docs.openclaw.ai/de/platforms) [Gateway & Ops](https://docs.openclaw.ai/de/gateway) [Reference](https://docs.openclaw.ai/de/cli) [Help](https://docs.openclaw.ai/de/help)

Auf dieser Seite

- [Importkonvention](https://docs.openclaw.ai/de/plugins/sdk-overview#importkonvention)
- [Unterpfadreferenz](https://docs.openclaw.ai/de/plugins/sdk-overview#unterpfadreferenz)
- [Registrierungs-API](https://docs.openclaw.ai/de/plugins/sdk-overview#registrierungs-api)
- [Capability-Registrierung](https://docs.openclaw.ai/de/plugins/sdk-overview#capability-registrierung)
- [Tools und Befehle](https://docs.openclaw.ai/de/plugins/sdk-overview#tools-und-befehle)
- [Infrastruktur](https://docs.openclaw.ai/de/plugins/sdk-overview#infrastruktur)
- [Host-Hooks für Workflow-Plugins](https://docs.openclaw.ai/de/plugins/sdk-overview#host-hooks-f%C3%BCr-workflow-plugins)
- [Gateway-Discovery-Registrierung](https://docs.openclaw.ai/de/plugins/sdk-overview#gateway-discovery-registrierung)
- [CLI-Registrierungsmetadaten](https://docs.openclaw.ai/de/plugins/sdk-overview#cli-registrierungsmetadaten)
- [CLI-Backend-Registrierung](https://docs.openclaw.ai/de/plugins/sdk-overview#cli-backend-registrierung)
- [Exklusive Slots](https://docs.openclaw.ai/de/plugins/sdk-overview#exklusive-slots)
- [Speicher-Embedding-Adapter](https://docs.openclaw.ai/de/plugins/sdk-overview#speicher-embedding-adapter)
- [Ereignisse und Lebenszyklus](https://docs.openclaw.ai/de/plugins/sdk-overview#ereignisse-und-lebenszyklus)
- [Hook-Entscheidungssemantik](https://docs.openclaw.ai/de/plugins/sdk-overview#hook-entscheidungssemantik)
- [API-Objektfelder](https://docs.openclaw.ai/de/plugins/sdk-overview#api-objektfelder)
- [Interne Modulkonvention](https://docs.openclaw.ai/de/plugins/sdk-overview#interne-modulkonvention)
- [Verwandt](https://docs.openclaw.ai/de/plugins/sdk-overview#verwandt)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Das Plugin-SDK ist der typisierte Vertrag zwischen Plugins und Core. Diese Seite ist die
Referenz dafür, **was importiert werden soll** und **was Sie registrieren können**.

Diese Seite richtet sich an Plugin-Autoren, die `openclaw/plugin-sdk/*` innerhalb von
OpenClaw verwenden. Für externe Apps, Skripte, Dashboards, CI-Jobs und IDE-Erweiterungen,
die Agents über das Gateway ausführen möchten, verwenden Sie stattdessen das
[OpenClaw App SDK](https://docs.openclaw.ai/de/concepts/openclaw-sdk) und das Paket `@openclaw/sdk`.

Suchen Sie stattdessen eine Anleitung? Beginnen Sie mit [Plugins erstellen](https://docs.openclaw.ai/de/plugins/building-plugins), verwenden Sie [Channel-Plugins](https://docs.openclaw.ai/de/plugins/sdk-channel-plugins) für Channel-Plugins, [Provider-Plugins](https://docs.openclaw.ai/de/plugins/sdk-provider-plugins) für Provider-Plugins und [Plugin-Hooks](https://docs.openclaw.ai/de/plugins/hooks) für Tool- oder Lifecycle-Hook-Plugins.

## [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#importkonvention)  Importkonvention

Importieren Sie immer aus einem spezifischen Unterpfad:

```
import { definePluginEntry } from "openclaw/plugin-sdk/plugin-entry";
import { defineChannelPluginEntry } from "openclaw/plugin-sdk/channel-core";
```

Jeder Unterpfad ist ein kleines, eigenständiges Modul. Dadurch bleibt der Start schnell und
Probleme mit zirkulären Abhängigkeiten werden vermieden. Für channelspezifische Entry-/Build-Helfer
bevorzugen Sie `openclaw/plugin-sdk/channel-core`; behalten Sie `openclaw/plugin-sdk/core` für
die breitere Dachoberfläche und gemeinsam genutzte Helfer wie
`buildChannelConfigSchema`.Für die Channel-Konfiguration veröffentlichen Sie das vom Channel verwaltete JSON Schema über
`openclaw.plugin.json#channelConfigs`. Der Unterpfad `plugin-sdk/channel-config-schema`
ist für gemeinsam genutzte Schema-Primitiven und den generischen Builder vorgesehen. Die
gebündelten Plugins von OpenClaw verwenden `plugin-sdk/bundled-channel-config-schema` für beibehaltene
Schemas gebündelter Channels. Veraltete Kompatibilitätsexporte bleiben auf
`plugin-sdk/channel-config-schema-legacy`; keiner der beiden Unterpfade für gebündelte Schemas ist ein
Muster für neue Plugins.

Importieren Sie keine Provider- oder Channel-gebrandeten Convenience-Seams (zum Beispiel
`openclaw/plugin-sdk/slack`, `.../discord`, `.../signal`, `.../whatsapp`).
Gebündelte Plugins setzen generische SDK-Unterpfade innerhalb ihrer eigenen `api.ts`\- /
`runtime-api.ts`-Barrels zusammen; Core-Verbraucher sollten entweder diese Plugin-lokalen
Barrels verwenden oder einen schmalen generischen SDK-Vertrag hinzufügen, wenn ein Bedarf wirklich
channelübergreifend ist.Eine kleine Gruppe von Hilfs-Seams für gebündelte Plugins erscheint weiterhin in der generierten Export-Map,
wenn sie nachverfolgte Owner-Nutzung haben. Sie existieren nur für die Wartung gebündelter Plugins
und werden nicht als Importpfade für neue Drittanbieter-Plugins empfohlen.`openclaw/plugin-sdk/discord` und `openclaw/plugin-sdk/telegram-account` werden
ebenfalls als veraltete Kompatibilitäts-Fassaden für nachverfolgte Owner-Nutzung beibehalten. Kopieren Sie
diese Importpfade nicht in neue Plugins; verwenden Sie stattdessen injizierte Runtime-Helfer und
generische Channel-SDK-Unterpfade.

## [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#unterpfadreferenz)  Unterpfadreferenz

Das Plugin-SDK wird als Satz schmaler Unterpfade bereitgestellt, gruppiert nach Bereich (Plugin-
Entry, Channel, Provider, Auth, Runtime, Capability, Memory und reservierte
Helfer für gebündelte Plugins). Den vollständigen Katalog, gruppiert und verlinkt, finden Sie unter
[Plugin-SDK-Unterpfade](https://docs.openclaw.ai/de/plugins/sdk-subpaths).Die generierte Liste von über 200 Unterpfaden befindet sich in `scripts/lib/plugin-sdk-entrypoints.json`.

## [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#registrierungs-api)  Registrierungs-API

Der `register(api)`-Callback erhält ein `OpenClawPluginApi`-Objekt mit diesen
Methoden:

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#capability-registrierung)  Capability-Registrierung

| Methode | Was sie registriert |
| --- | --- |
| `api.registerProvider(...)` | Textinferenz (LLM) |
| `api.registerAgentHarness(...)` | Experimenteller Low-Level-Agent-Executor |
| `api.registerCliBackend(...)` | Lokales CLI-Inferenz-Backend |
| `api.registerChannel(...)` | Messaging-Channel |
| `api.registerSpeechProvider(...)` | Text-to-Speech- / STT-Synthese |
| `api.registerRealtimeTranscriptionProvider(...)` | Streaming-Echtzeittranskription |
| `api.registerRealtimeVoiceProvider(...)` | Duplex-Echtzeit-Sprachsessions |
| `api.registerMediaUnderstandingProvider(...)` | Bild-/Audio-/Videoanalyse |
| `api.registerImageGenerationProvider(...)` | Bilderzeugung |
| `api.registerMusicGenerationProvider(...)` | Musikerzeugung |
| `api.registerVideoGenerationProvider(...)` | Videoerzeugung |
| `api.registerWebFetchProvider(...)` | Web-Fetch-/Scrape-Provider |
| `api.registerWebSearchProvider(...)` | Websuche |

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#tools-und-befehle)  Tools und Befehle

| Methode | Was sie registriert |
| --- | --- |
| `api.registerTool(tool, opts?)` | Agent-Tool (erforderlich oder `{ optional: true }`) |
| `api.registerCommand(def)` | Benutzerdefinierter Befehl (umgeht das LLM) |

Plugin-Befehle können `agentPromptGuidance` setzen, wenn der Agent einen kurzen,
befehlseigenen Routing-Hinweis benötigt. Beschränken Sie diesen Text auf den Befehl selbst; fügen Sie keine
Provider- oder Plugin-spezifische Policy zu Core-Prompt-Buildern hinzu.

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#infrastruktur)  Infrastruktur

| Methode | Was sie registriert |
| --- | --- |
| `api.registerHook(events, handler, opts?)` | Event-Hook |
| `api.registerHttpRoute(params)` | Gateway-HTTP-Endpunkt |
| `api.registerGatewayMethod(name, handler)` | Gateway-RPC-Methode |
| `api.registerGatewayDiscoveryService(service)` | Lokaler Gateway-Discovery-Advertiser |
| `api.registerCli(registrar, opts?)` | CLI-Unterbefehl |
| `api.registerService(service)` | Hintergrunddienst |
| `api.registerInteractiveHandler(registration)` | Interaktiver Handler |
| `api.registerAgentToolResultMiddleware(...)` | Runtime-Tool-Result-Middleware |
| `api.registerMemoryPromptSupplement(builder)` | Additiver promptnaher Memory-Abschnitt |
| `api.registerMemoryCorpusSupplement(adapter)` | Additiver Memory-Such-/Lesekorpus |

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#host-hooks-f%C3%BCr-workflow-plugins)  Host-Hooks für Workflow-Plugins

Host-Hooks sind die SDK-Seams für Plugins, die am Host-
Lifecycle teilnehmen müssen, statt nur einen Provider, Channel oder ein Tool hinzuzufügen. Sie sind
generische Verträge; der Plan-Modus kann sie verwenden, aber ebenso Genehmigungs-Workflows,
Workspace-Policy-Gates, Hintergrundmonitore, Einrichtungsassistenten und UI-Begleit-
Plugins.

| Methode | Vertrag, den sie besitzt |
| --- | --- |
| `api.registerSessionExtension(...)` | Plugin-eigener, JSON-kompatibler Session-State, der über Gateway-Sessions projiziert wird |
| `api.enqueueNextTurnInjection(...)` | Dauerhafter genau-einmal-Kontext, der für eine Session in den nächsten Agent-Turn injiziert wird |
| `api.registerTrustedToolPolicy(...)` | Gebündelte/vertrauenswürdige Pre-Plugin-Tool-Policy, die Tool-Parameter blockieren oder umschreiben kann |
| `api.registerToolMetadata(...)` | Tool-Katalog-Anzeigemetadaten ohne Änderung der Tool-Implementierung |
| `api.registerCommand(...)` | Bereichsgebundene Plugin-Befehle; Befehlsergebnisse können `continueAgent: true` setzen; native Discord-Befehle unterstützen `descriptionLocalizations` |
| `api.registerControlUiDescriptor(...)` | Control-UI-Contribution-Deskriptoren für Session-, Tool-, Run- oder Settings-Oberflächen |
| `api.registerRuntimeLifecycle(...)` | Cleanup-Callbacks für Plugin-eigene Runtime-Ressourcen auf Reset-/Delete-/Reload-Pfaden |
| `api.registerAgentEventSubscription(...)` | Bereinigte Event-Abonnements für Workflow-State und Monitore |
| `api.setRunContext(...)` / `getRunContext(...)` / `clearRunContext(...)` | Pro-Run-Plugin-Scratch-State, der beim terminalen Run-Lifecycle gelöscht wird |
| `api.registerSessionSchedulerJob(...)` | Plugin-eigene Session-Scheduler-Job-Records mit deterministischem Cleanup |

Die Verträge trennen die Autorität bewusst:

- Externe Plugins können Session-Erweiterungen, UI-Deskriptoren, Befehle, Tool-
Metadaten, Next-Turn-Injections und normale Hooks besitzen.
- Vertrauenswürdige Tool-Policies laufen vor gewöhnlichen `before_tool_call`-Hooks und sind
nur gebündelten Plugins vorbehalten, weil sie an der Host-Sicherheits-Policy teilnehmen.
- Reservierter Befehlsbesitz ist nur gebündelten Plugins vorbehalten. Externe Plugins sollten ihre
eigenen Befehlsnamen oder Aliase verwenden.
- `allowPromptInjection=false` deaktiviert promptverändernde Hooks einschließlich
`agent_turn_prepare`, `before_prompt_build`, `heartbeat_prompt_contribution`,
Prompt-Felder aus dem Legacy-`before_agent_start` und
`enqueueNextTurnInjection`.

Beispiele für Nicht-Plan-Verbraucher:

| Plugin-Archetyp | Verwendete Hooks |
| --- | --- |
| Genehmigungs-Workflow | Session-Erweiterung, Befehlsfortsetzung, Next-Turn-Injection, UI-Deskriptor |
| Budget-/Workspace-Policy-Gate | Vertrauenswürdige Tool-Policy, Tool-Metadaten, Session-Projektion |
| Hintergrund-Lifecycle-Monitor | Runtime-Lifecycle-Cleanup, Agent-Event-Abonnement, Besitz/Cleanup des Session-Schedulers, Heartbeat-Prompt-Beitrag, UI-Deskriptor |
| Einrichtungs- oder Onboarding-Assistent | Session-Erweiterung, bereichsgebundene Befehle, Control-UI-Deskriptor |

Reservierte Core-Admin-Namespaces (`config.*`, `exec.approvals.*`, `wizard.*`,
`update.*`) bleiben immer `operator.admin`, selbst wenn ein Plugin versucht, einen
schmaleren Gateway-Method-Scope zuzuweisen. Bevorzugen Sie Plugin-spezifische Präfixe für
Plugin-eigene Methoden.

When to use tool-result middleware

Gebündelte Plugins können `api.registerAgentToolResultMiddleware(...)` verwenden, wenn
sie ein Tool-Ergebnis nach der Ausführung und bevor die Runtime
dieses Ergebnis zurück in das Modell einspeist, umschreiben müssen. Dies ist der vertrauenswürdige, runtime-neutrale
Seam für asynchrone Output-Reducer wie tokenjuice.Gebündelte Plugins müssen `contracts.agentToolResultMiddleware` für jede
zielgerichtete Runtime deklarieren, zum Beispiel `["pi", "codex"]`. Externe Plugins
können diese Middleware nicht registrieren; verwenden Sie normale OpenClaw-Plugin-Hooks für Arbeit,
die kein Pre-Model-Tool-Result-Timing benötigt. Der alte, nur für Pi geltende eingebettete
Registrierungspfad der Erweiterungs-Factory wurde entfernt.

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#gateway-discovery-registrierung)  Gateway-Discovery-Registrierung

`api.registerGatewayDiscoveryService(...)` ermöglicht es einem Plugin, den aktiven
Gateway über einen lokalen Discovery-Transport wie mDNS/Bonjour bekanntzugeben. OpenClaw ruft den
Dienst beim Start des Gateway auf, wenn lokale Discovery aktiviert ist, übergibt die
aktuellen Gateway-Ports und nicht geheime TXT-Hinweisdaten und ruft beim Herunterfahren des Gateway den zurückgegebenen
`stop`-Handler auf.

```
api.registerGatewayDiscoveryService({
  id: "my-discovery",
  async advertise(ctx) {
    const handle = await startMyAdvertiser({
      gatewayPort: ctx.gatewayPort,
      tls: ctx.gatewayTlsEnabled,
      displayName: ctx.machineDisplayName,
    });
    return { stop: () => handle.stop() };
  },
});
```

Gateway-Discovery-Plugins dürfen beworbene TXT-Werte nicht als Geheimnisse oder
Authentifizierung behandeln. Discovery ist ein Routing-Hinweis; Gateway-Authentifizierung und TLS-Pinning
bleiben für Vertrauen zuständig.

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#cli-registrierungsmetadaten)  CLI-Registrierungsmetadaten

`api.registerCli(registrar, opts?)` akzeptiert zwei Arten von Metadaten auf oberster Ebene:

- `commands`: explizite Befehlswurzeln, die dem Registrar gehören
- `descriptors`: Befehlsdeskriptoren zur Parse-Zeit, die für Root-CLI-Hilfe,
Routing und verzögerte Plugin-CLI-Registrierung verwendet werden

Wenn ein Plugin-Befehl im normalen Root-CLI-Pfad verzögert geladen bleiben soll,
stellen Sie `descriptors` bereit, die jede von diesem Registrar offengelegte
Befehlswurzel auf oberster Ebene abdecken.

```
api.registerCli(
  async ({ program }) => {
    const { registerMatrixCli } = await import("./src/cli.js");
    registerMatrixCli({ program });
  },
  {
    descriptors: [\
      {\
        name: "matrix",\
        description: "Manage Matrix accounts, verification, devices, and profile state",\
        hasSubcommands: true,\
      },\
    ],
  },
);
```

Verwenden Sie `commands` allein nur, wenn Sie keine verzögerte Root-CLI-Registrierung benötigen.
Dieser eifrige Kompatibilitätspfad wird weiterhin unterstützt, installiert aber keine
deskriptorbasierten Platzhalter für verzögertes Laden zur Parse-Zeit.

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#cli-backend-registrierung)  CLI-Backend-Registrierung

`api.registerCliBackend(...)` ermöglicht es einem Plugin, die Standardkonfiguration für ein lokales
KI-CLI-Backend wie `codex-cli` zu besitzen.

- Die Backend-`id` wird zum Provider-Präfix in Modellreferenzen wie `codex-cli/gpt-5`.
- Die Backend-`config` verwendet dieselbe Form wie `agents.defaults.cliBackends.<id>`.
- Die Benutzerkonfiguration hat weiterhin Vorrang. OpenClaw führt `agents.defaults.cliBackends.<id>` über der
Plugin-Standardkonfiguration zusammen, bevor die CLI ausgeführt wird.
- Verwenden Sie `normalizeConfig`, wenn ein Backend nach dem Zusammenführen Kompatibilitätsumschreibungen benötigt
(zum Beispiel die Normalisierung alter Flag-Formen).

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#exklusive-slots)  Exklusive Slots

| Methode | Was sie registriert |
| --- | --- |
| `api.registerContextEngine(id, factory)` | Kontext-Engine (jeweils eine aktiv). Der `assemble()`-Callback erhält `availableTools` und `citationsMode`, damit die Engine Prompt-Ergänzungen anpassen kann. |
| `api.registerMemoryCapability(capability)` | Einheitliche Speicherfähigkeit |
| `api.registerMemoryPromptSection(builder)` | Builder für Speicher-Prompt-Abschnitte |
| `api.registerMemoryFlushPlan(resolver)` | Resolver für Speicher-Flush-Pläne |
| `api.registerMemoryRuntime(runtime)` | Speicher-Runtime-Adapter |

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#speicher-embedding-adapter)  Speicher-Embedding-Adapter

| Methode | Was sie registriert |
| --- | --- |
| `api.registerMemoryEmbeddingProvider(adapter)` | Speicher-Embedding-Adapter für das aktive Plugin |

- `registerMemoryCapability` ist die bevorzugte exklusive Speicher-Plugin-API.
- `registerMemoryCapability` kann auch `publicArtifacts.listArtifacts(...)`
offenlegen, damit Begleit-Plugins exportierte Speicherartefakte über
`openclaw/plugin-sdk/memory-host-core` konsumieren können, statt in das private Layout eines bestimmten
Speicher-Plugins zu greifen.
- `registerMemoryPromptSection`, `registerMemoryFlushPlan` und
`registerMemoryRuntime` sind legacy-kompatible exklusive Speicher-Plugin-APIs.
- `MemoryFlushPlan.model` kann den Flush-Turn an eine exakte `provider/model`-Referenz
wie `ollama/qwen3:8b` binden, ohne die aktive Fallback-Kette zu erben.
- `registerMemoryEmbeddingProvider` ermöglicht es dem aktiven Speicher-Plugin, eine
oder mehrere Embedding-Adapter-IDs zu registrieren (zum Beispiel `openai`, `gemini` oder eine benutzerdefinierte
vom Plugin definierte ID).
- Benutzerkonfigurationen wie `agents.defaults.memorySearch.provider` und
`agents.defaults.memorySearch.fallback` werden gegen diese registrierten
Adapter-IDs aufgelöst.

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#ereignisse-und-lebenszyklus)  Ereignisse und Lebenszyklus

| Methode | Was sie bewirkt |
| --- | --- |
| `api.on(hookName, handler, opts?)` | Typisierter Lebenszyklus-Hook |
| `api.onConversationBindingResolved(handler)` | Conversation-Binding-Callback |

Siehe [Plugin-Hooks](https://docs.openclaw.ai/de/plugins/hooks) für Beispiele, gängige Hook-Namen und Guard-Semantik.

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#hook-entscheidungssemantik)  Hook-Entscheidungssemantik

- `before_tool_call`: Die Rückgabe von `{ block: true }` ist terminal. Sobald ein Handler sie setzt, werden Handler mit niedrigerer Priorität übersprungen.
- `before_tool_call`: Die Rückgabe von `{ block: false }` wird als keine Entscheidung behandelt (genauso wie das Weglassen von `block`), nicht als Override.
- `before_install`: Die Rückgabe von `{ block: true }` ist terminal. Sobald ein Handler sie setzt, werden Handler mit niedrigerer Priorität übersprungen.
- `before_install`: Die Rückgabe von `{ block: false }` wird als keine Entscheidung behandelt (genauso wie das Weglassen von `block`), nicht als Override.
- `reply_dispatch`: Die Rückgabe von `{ handled: true, ... }` ist terminal. Sobald ein Handler den Dispatch beansprucht, werden Handler mit niedrigerer Priorität und der standardmäßige Modell-Dispatch-Pfad übersprungen.
- `message_sending`: Die Rückgabe von `{ cancel: true }` ist terminal. Sobald ein Handler sie setzt, werden Handler mit niedrigerer Priorität übersprungen.
- `message_sending`: Die Rückgabe von `{ cancel: false }` wird als keine Entscheidung behandelt (genauso wie das Weglassen von `cancel`), nicht als Override.
- `message_received`: Verwenden Sie das typisierte Feld `threadId`, wenn Sie eingehendes Thread-/Themen-Routing benötigen. Behalten Sie `metadata` für kanalspezifische Extras bei.
- `message_sending`: Verwenden Sie die typisierten Routing-Felder `replyToId` / `threadId`, bevor Sie auf kanalspezifische `metadata` zurückfallen.
- `gateway_start`: Verwenden Sie `ctx.config`, `ctx.workspaceDir` und `ctx.getCron?.()` für den Gateway-eigenen Startzustand, statt sich auf interne `gateway:startup`-Hooks zu verlassen.
- `cron_changed`: Beobachten Sie Gateway-eigene Änderungen am Cron-Lebenszyklus. Verwenden Sie `event.job?.state?.nextRunAtMs` und `ctx.getCron?.()`, wenn Sie externe Wake-Scheduler synchronisieren, und behalten Sie OpenClaw als Quelle der Wahrheit für Fälligkeitsprüfungen und Ausführung bei.

### [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#api-objektfelder)  API-Objektfelder

| Feld | Typ | Beschreibung |
| --- | --- | --- |
| `api.id` | `string` | Plugin-ID |
| `api.name` | `string` | Anzeigename |
| `api.version` | `string?` | Plugin-Version (optional) |
| `api.description` | `string?` | Plugin-Beschreibung (optional) |
| `api.source` | `string` | Plugin-Quellpfad |
| `api.rootDir` | `string?` | Plugin-Stammverzeichnis (optional) |
| `api.config` | `OpenClawConfig` | Aktueller Konfigurations-Snapshot (aktiver In-Memory-Runtime-Snapshot, wenn verfügbar) |
| `api.pluginConfig` | `Record<string, unknown>` | Plugin-spezifische Konfiguration aus `plugins.entries.<id>.config` |
| `api.runtime` | `PluginRuntime` | [Runtime-Helfer](https://docs.openclaw.ai/de/plugins/sdk-runtime) |
| `api.logger` | `PluginLogger` | Bereichsbezogener Logger (`debug`, `info`, `warn`, `error`) |
| `api.registrationMode` | `PluginRegistrationMode` | Aktueller Lademodus; `"setup-runtime"` ist das leichtgewichtige Start-/Setup-Fenster vor dem vollständigen Eintrag |
| `api.resolvePath(input)` | `(string) => string` | Pfad relativ zum Plugin-Stamm auflösen |

## [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#interne-modulkonvention)  Interne Modulkonvention

Verwenden Sie innerhalb Ihres Plugins lokale Barrel-Dateien für interne Importe:

```
my-plugin/
  api.ts            # Public exports for external consumers
  runtime-api.ts    # Internal-only runtime exports
  index.ts          # Plugin entry point
  setup-entry.ts    # Lightweight setup-only entry (optional)
```

Importieren Sie Ihr eigenes Plugin niemals über `openclaw/plugin-sdk/<your-plugin>`
aus Produktionscode. Leiten Sie interne Importe über `./api.ts` oder
`./runtime-api.ts`. Der SDK-Pfad ist ausschließlich der externe Vertrag.

Öffentliche Oberflächen gebündelter Plugins, die über Facades geladen werden (`api.ts`, `runtime-api.ts`,
`index.ts`, `setup-entry.ts` und ähnliche öffentliche Einstiegdateien), bevorzugen den
aktiven Runtime-Konfigurations-Snapshot, wenn OpenClaw bereits ausgeführt wird. Wenn noch kein Runtime-
Snapshot existiert, fallen sie auf die aufgelöste Konfigurationsdatei auf dem Datenträger zurück.
Paketierte gebündelte Plugin-Facades sollten über die Plugin-
Facade-Loader von OpenClaw geladen werden; direkte Importe aus `dist/extensions/...` umgehen die Manifest-
und Runtime-Sidecar-Prüfungen, die paketierte Installationen für Plugin-eigenen Code verwenden.Provider-Plugins können ein schmales Plugin-lokales Contract-Barrel offenlegen, wenn ein
Helfer absichtlich Provider-spezifisch ist und noch nicht in einen generischen SDK-
Unterpfad gehört. Gebündelte Beispiele:

- **Anthropic**: öffentliche `api.ts`\- / `contract-api.ts`-Schnittstelle für Claude-
Beta-Header- und `service_tier`-Stream-Helfer.
- **`@openclaw/openai-provider`**: `api.ts` exportiert Provider-Builder,
Standardmodell-Helfer und Realtime-Provider-Builder.
- **`@openclaw/openrouter-provider`**: `api.ts` exportiert den Provider-Builder
plus Onboarding-/Konfigurationshelfer.

Produktionscode von Erweiterungen sollte auch Importe von `openclaw/plugin-sdk/<other-plugin>`
vermeiden. Wenn ein Helfer wirklich geteilt wird, heben Sie ihn in einen neutralen SDK-Unterpfad
wie `openclaw/plugin-sdk/speech`, `.../provider-model-shared` oder eine andere
fähigkeitsorientierte Oberfläche, statt zwei Plugins miteinander zu koppeln.

## [​](https://docs.openclaw.ai/de/plugins/sdk-overview\#verwandt)  Verwandt

[**Einstiegspunkte** \\
\\
Optionen für `definePluginEntry` und `defineChannelPluginEntry`.](https://docs.openclaw.ai/de/plugins/sdk-entrypoints)

[**Runtime-Hilfsfunktionen** \\
\\
Vollständige Referenz des Namespace `api.runtime`.](https://docs.openclaw.ai/de/plugins/sdk-runtime)

[**Einrichtung und Konfiguration** \\
\\
Paketierung, Manifeste und Konfigurationsschemas.](https://docs.openclaw.ai/de/plugins/sdk-setup)

[**Testen** \\
\\
Test-Hilfsprogramme und Lint-Regeln.](https://docs.openclaw.ai/de/plugins/sdk-testing)

[**SDK-Migration** \\
\\
Migration von veralteten Oberflächen.](https://docs.openclaw.ai/de/plugins/sdk-migration)

[**Plugin-Interna** \\
\\
Detaillierte Architektur und Fähigkeitsmodell.](https://docs.openclaw.ai/de/plugins/architecture)

[Migrate to SDK](https://docs.openclaw.ai/de/plugins/sdk-migration) [Plugin-SDK-Unterpfade](https://docs.openclaw.ai/de/plugins/sdk-subpaths)

Ctrl+I