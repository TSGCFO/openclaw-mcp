---
source_url: https://docs.openclaw.ai/it/cli/doctor
title: "Diagnostica - OpenClaw"
---

[Vai al contenuto principale](https://docs.openclaw.ai/it/cli/doctor#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/it)

![IT](https://d3gk2c5xim1je2.cloudfront.net/flags/IT.svg)

Italiano

Cerca...

Ctrl K

Cerca...

Navigation

Gateway and service

Diagnostica

[Get started](https://docs.openclaw.ai/it) [Install](https://docs.openclaw.ai/it/install) [Channels](https://docs.openclaw.ai/it/channels) [Agents](https://docs.openclaw.ai/it/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/it/tools) [Models](https://docs.openclaw.ai/it/providers) [Platforms](https://docs.openclaw.ai/it/platforms) [Gateway & Ops](https://docs.openclaw.ai/it/gateway) [Reference](https://docs.openclaw.ai/it/cli) [Help](https://docs.openclaw.ai/it/help)

In questa pagina

- [openclaw doctor](https://docs.openclaw.ai/it/cli/doctor#openclaw-doctor)
- [Esempi](https://docs.openclaw.ai/it/cli/doctor#esempi)
- [Opzioni](https://docs.openclaw.ai/it/cli/doctor#opzioni)
- [macOS: override env di launchctl](https://docs.openclaw.ai/it/cli/doctor#macos-override-env-di-launchctl)
- [Correlati](https://docs.openclaw.ai/it/cli/doctor#correlati)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/it/cli/doctor\#openclaw-doctor)  `openclaw doctor`

Controlli di integrità e correzioni rapide per il Gateway e i canali.Correlati:

- Risoluzione dei problemi: [Risoluzione dei problemi](https://docs.openclaw.ai/it/gateway/troubleshooting)
- Audit di sicurezza: [Sicurezza](https://docs.openclaw.ai/it/gateway/security)

## [​](https://docs.openclaw.ai/it/cli/doctor\#esempi)  Esempi

```
openclaw doctor
openclaw doctor --repair
openclaw doctor --deep
openclaw doctor --repair --non-interactive
openclaw doctor --generate-gateway-token
```

## [​](https://docs.openclaw.ai/it/cli/doctor\#opzioni)  Opzioni

- `--no-workspace-suggestions`: disattiva i suggerimenti di memoria/ricerca dell’area di lavoro
- `--yes`: accetta i valori predefiniti senza chiedere conferma
- `--repair`: applica le riparazioni non di servizio consigliate senza chiedere conferma; le installazioni e le riscritture del servizio Gateway richiedono comunque una conferma interattiva o comandi Gateway espliciti
- `--fix`: alias di `--repair`
- `--force`: applica riparazioni aggressive, inclusa la sovrascrittura della configurazione personalizzata del servizio quando necessario
- `--non-interactive`: esegue senza prompt; solo migrazioni sicure e riparazioni non di servizio
- `--generate-gateway-token`: genera e configura un token del Gateway
- `--deep`: analizza i servizi di sistema alla ricerca di installazioni Gateway aggiuntive

Note:

- I prompt interattivi (come le correzioni di portachiavi/OAuth) vengono eseguiti solo quando stdin è un TTY e `--non-interactive` **non** è impostato. Le esecuzioni headless (cron, Telegram, nessun terminale) saltano i prompt.
- Prestazioni: le esecuzioni non interattive di `doctor` saltano il caricamento anticipato dei plugin, così i controlli di integrità headless restano veloci. Le sessioni interattive caricano comunque completamente i plugin quando un controllo richiede il loro contributo.
- `--fix` (alias di `--repair`) scrive un backup in `~/.openclaw/openclaw.json.bak` ed elimina le chiavi di configurazione sconosciute, elencando ogni rimozione.
- `doctor --fix --non-interactive` segnala definizioni del servizio Gateway mancanti o obsolete, ma non le installa né le riscrive al di fuori della modalità di riparazione aggiornamento. Esegui `openclaw gateway install` per un servizio mancante, oppure `openclaw gateway install --force` quando vuoi intenzionalmente sostituire il launcher.
- I controlli di integrità dello stato ora rilevano file di trascrizione orfani nella directory delle sessioni. Archiviarli come `.deleted.<timestamp>` richiede una conferma interattiva; `--fix`, `--yes` e le esecuzioni headless li lasciano al loro posto.
- Doctor analizza anche `~/.openclaw/cron/jobs.json` (o `cron.store`) alla ricerca di formati legacy dei job cron e può riscriverli sul posto prima che lo scheduler debba normalizzarli automaticamente in fase di esecuzione.
- Su Linux, doctor avvisa quando il crontab dell’utente esegue ancora il legacy `~/.openclaw/bin/ensure-whatsapp.sh`; quello script non è più mantenuto e può registrare falsi outage del Gateway WhatsApp quando cron non dispone dell’ambiente user-bus di systemd.
- Doctor pulisce lo stato legacy di staging delle dipendenze dei plugin creato da versioni precedenti di OpenClaw. Ripara anche i plugin scaricabili configurati mancanti quando il registro riesce a risolverli, e il passaggio doctor 2026.5.2 installa automaticamente i plugin scaricabili che una configurazione precedente usa già prima di contrassegnare la configurazione come toccata per quella release.
- Doctor ripara la configurazione obsoleta dei plugin rimuovendo gli id dei plugin mancanti da `plugins.allow`/`plugins.entries`, insieme alla configurazione di canale pendente corrispondente, ai target Heartbeat e agli override del modello di canale quando la scoperta dei plugin è integra.
- Doctor mette in quarantena la configurazione non valida dei plugin disabilitando la voce `plugins.entries.<id>` interessata e rimuovendo il relativo payload `config` non valido. L’avvio del Gateway salta già solo quel plugin difettoso, così gli altri plugin e canali possono continuare a funzionare.
- Imposta `OPENCLAW_SERVICE_REPAIR_POLICY=external` quando un altro supervisore possiede il ciclo di vita del Gateway. Doctor continua a segnalare lo stato di integrità del Gateway/servizio e applica le riparazioni non di servizio, ma salta installazione/avvio/riavvio/bootstrap del servizio e la pulizia dei servizi legacy.
- Su Linux, doctor ignora le unità systemd inattive extra simili al Gateway e non riscrive i metadati di comando/entrypoint per un servizio Gateway systemd in esecuzione durante la riparazione. Arresta prima il servizio oppure usa `openclaw gateway install --force` quando vuoi intenzionalmente sostituire il launcher attivo.
- Doctor migra automaticamente la configurazione Talk piatta legacy (`talk.voiceId`, `talk.modelId` e affini) in `talk.provider` \+ `talk.providers.<provider>`.
- Le esecuzioni ripetute di `doctor --fix` non segnalano/applicano più la normalizzazione di Talk quando l’unica differenza è l’ordine delle chiavi dell’oggetto.
- Doctor include un controllo di preparazione della ricerca in memoria e può consigliare `openclaw configure --section model` quando mancano le credenziali di embedding.
- Doctor avvisa quando non è configurato alcun proprietario dei comandi. Il proprietario dei comandi è l’account dell’operatore umano autorizzato a eseguire comandi riservati al proprietario e ad approvare azioni pericolose. L’associazione via DM consente solo a qualcuno di parlare con il bot; se hai approvato un mittente prima che esistesse il bootstrap del primo proprietario, imposta esplicitamente `commands.ownerAllowFrom`.
- Doctor avvisa quando sono configurati agenti in modalità Codex ed esistono asset personali della CLI Codex nella home Codex dell’operatore. Gli avvii locali dell’app-server Codex usano home isolate per agente, quindi usa `openclaw migrate codex --dry-run` per inventariare gli asset che dovrebbero essere promossi deliberatamente.
- Doctor avvisa quando le skills consentite per l’agente predefinito non sono disponibili nell’ambiente runtime corrente perché mancano bin, variabili d’ambiente, configurazione o requisiti del sistema operativo. `doctor --fix` può disabilitare quelle skills non disponibili con `skills.entries.<skill>.enabled=false`; installa/configura invece il requisito mancante quando vuoi mantenere attiva la skill.
- Se la modalità sandbox è abilitata ma Docker non è disponibile, doctor segnala un avviso ad alto segnale con rimedio (`install Docker` oppure `openclaw config set agents.defaults.sandbox.mode off`).
- Se `gateway.auth.token`/`gateway.auth.password` sono gestiti da SecretRef e non disponibili nel percorso del comando corrente, doctor segnala un avviso di sola lettura e non scrive credenziali di fallback in testo in chiaro.
- Se l’ispezione SecretRef del canale fallisce in un percorso di correzione, doctor continua e segnala un avviso invece di uscire in anticipo.
- Dopo le migrazioni della directory di stato, doctor avvisa quando account Telegram o Discord predefiniti abilitati dipendono dal fallback tramite variabili d’ambiente e `TELEGRAM_BOT_TOKEN` o `DISCORD_BOT_TOKEN` non è disponibile per il processo doctor.
- La risoluzione automatica dei nomi utente Telegram `allowFrom` (`doctor --fix`) richiede un token Telegram risolvibile nel percorso del comando corrente. Se l’ispezione del token non è disponibile, doctor segnala un avviso e salta la risoluzione automatica per quel passaggio.

## [​](https://docs.openclaw.ai/it/cli/doctor\#macos-override-env-di-launchctl)  macOS: override env di `launchctl`

Se in precedenza hai eseguito `launchctl setenv OPENCLAW_GATEWAY_TOKEN ...` (o `...PASSWORD`), quel valore sovrascrive il tuo file di configurazione e può causare errori persistenti di “non autorizzato”.

```
launchctl getenv OPENCLAW_GATEWAY_TOKEN
launchctl getenv OPENCLAW_GATEWAY_PASSWORD

launchctl unsetenv OPENCLAW_GATEWAY_TOKEN
launchctl unsetenv OPENCLAW_GATEWAY_PASSWORD
```

## [​](https://docs.openclaw.ai/it/cli/doctor\#correlati)  Correlati

- [Riferimento CLI](https://docs.openclaw.ai/it/cli)
- [Gateway doctor](https://docs.openclaw.ai/it/gateway/doctor)

[Demone](https://docs.openclaw.ai/it/cli/daemon) [Gateway](https://docs.openclaw.ai/it/cli/gateway)

Ctrl+I