---
source_url: https://docs.openclaw.ai/it/help/environment
title: "Variabili d'ambiente - OpenClaw"
---

[Vai al contenuto principale](https://docs.openclaw.ai/it/help/environment#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/it)

![IT](https://d3gk2c5xim1je2.cloudfront.net/flags/IT.svg)

Italiano

Cerca...

Ctrl K

Cerca...

Navigation

Diagnostics

Variabili d'ambiente

[Get started](https://docs.openclaw.ai/it) [Install](https://docs.openclaw.ai/it/install) [Channels](https://docs.openclaw.ai/it/channels) [Agents](https://docs.openclaw.ai/it/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/it/tools) [Models](https://docs.openclaw.ai/it/providers) [Platforms](https://docs.openclaw.ai/it/platforms) [Gateway & Ops](https://docs.openclaw.ai/it/gateway) [Reference](https://docs.openclaw.ai/it/cli) [Help](https://docs.openclaw.ai/it/help)

In questa pagina

- [Precedenza (dalla più alta alla più bassa)](https://docs.openclaw.ai/it/help/environment#precedenza-dalla-pi%C3%B9-alta-alla-pi%C3%B9-bassa)
- [Blocco env della configurazione](https://docs.openclaw.ai/it/help/environment#blocco-env-della-configurazione)
- [Importazione dell’ambiente della shell](https://docs.openclaw.ai/it/help/environment#importazione-dell%E2%80%99ambiente-della-shell)
- [Variabili d’ambiente iniettate a runtime](https://docs.openclaw.ai/it/help/environment#variabili-d%E2%80%99ambiente-iniettate-a-runtime)
- [Variabili d’ambiente della UI](https://docs.openclaw.ai/it/help/environment#variabili-d%E2%80%99ambiente-della-ui)
- [Sostituzione delle variabili d’ambiente nella configurazione](https://docs.openclaw.ai/it/help/environment#sostituzione-delle-variabili-d%E2%80%99ambiente-nella-configurazione)
- [Riferimenti ai segreti rispetto alle stringhe ${ENV}](https://docs.openclaw.ai/it/help/environment#riferimenti-ai-segreti-rispetto-alle-stringhe-%24env)
- [Variabili d’ambiente relative ai percorsi](https://docs.openclaw.ai/it/help/environment#variabili-d%E2%80%99ambiente-relative-ai-percorsi)
- [Logging](https://docs.openclaw.ai/it/help/environment#logging)
- [OPENCLAW\_HOME](https://docs.openclaw.ai/it/help/environment#openclaw_home)
- [Utenti nvm: errori TLS di web\_fetch](https://docs.openclaw.ai/it/help/environment#utenti-nvm-errori-tls-di-web_fetch)
- [Variabili d’ambiente legacy](https://docs.openclaw.ai/it/help/environment#variabili-d%E2%80%99ambiente-legacy)
- [Correlati](https://docs.openclaw.ai/it/help/environment#correlati)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw carica le variabili d’ambiente da più fonti. La regola è **non sovrascrivere mai i valori esistenti**.

## [​](https://docs.openclaw.ai/it/help/environment\#precedenza-dalla-pi%C3%B9-alta-alla-pi%C3%B9-bassa)  Precedenza (dalla più alta alla più bassa)

1. **Ambiente del processo** (ciò che il processo Gateway ha già dalla shell/daemon padre).
2. **`.env` nella directory di lavoro corrente** (predefinito di dotenv; non sovrascrive).
3. **`.env` globale** in `~/.openclaw/.env` (alias `$OPENCLAW_STATE_DIR/.env`; non sovrascrive).
4. **Blocco `env` della configurazione** in `~/.openclaw/openclaw.json` (applicato solo se mancante).
5. **Importazione opzionale della shell di login** (`env.shellEnv.enabled` o `OPENCLAW_LOAD_SHELL_ENV=1`), applicata solo per le chiavi attese mancanti.

Nelle nuove installazioni su Ubuntu che usano la directory di stato predefinita, OpenClaw tratta anche `~/.config/openclaw/gateway.env` come fallback di compatibilità dopo il `.env` globale. Se entrambi i file esistono e sono in disaccordo, OpenClaw mantiene `~/.openclaw/.env` e stampa un avviso.Se il file di configurazione manca del tutto, il passaggio 4 viene saltato; l’importazione dalla shell viene comunque eseguita se abilitata.

## [​](https://docs.openclaw.ai/it/help/environment\#blocco-env-della-configurazione)  Blocco `env` della configurazione

Due modi equivalenti per impostare variabili d’ambiente inline (entrambi non sovrascrivono):

```
{
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: {
      GROQ_API_KEY: "gsk-...",
    },
  },
}
```

## [​](https://docs.openclaw.ai/it/help/environment\#importazione-dell%E2%80%99ambiente-della-shell)  Importazione dell’ambiente della shell

`env.shellEnv` esegue la tua shell di login e importa solo le chiavi attese **mancanti**:

```
{
  env: {
    shellEnv: {
      enabled: true,
      timeoutMs: 15000,
    },
  },
}
```

Equivalenti come variabili d’ambiente:

- `OPENCLAW_LOAD_SHELL_ENV=1`
- `OPENCLAW_SHELL_ENV_TIMEOUT_MS=15000`

## [​](https://docs.openclaw.ai/it/help/environment\#variabili-d%E2%80%99ambiente-iniettate-a-runtime)  Variabili d’ambiente iniettate a runtime

OpenClaw inietta anche marcatori di contesto nei processi figli generati:

- `OPENCLAW_SHELL=exec`: impostata per i comandi eseguiti tramite lo strumento `exec`.
- `OPENCLAW_SHELL=acp`: impostata per la generazione di processi del backend runtime ACP (per esempio `acpx`).
- `OPENCLAW_SHELL=acp-client`: impostata per `openclaw acp client` quando genera il processo bridge ACP.
- `OPENCLAW_SHELL=tui-local`: impostata per i comandi shell `!` della TUI locale.

Questi sono marcatori di runtime (non configurazione utente richiesta). Possono essere usati nella logica di shell/profilo
per applicare regole specifiche del contesto.

## [​](https://docs.openclaw.ai/it/help/environment\#variabili-d%E2%80%99ambiente-della-ui)  Variabili d’ambiente della UI

- `OPENCLAW_THEME=light`: forza la palette TUI chiara quando il terminale ha uno sfondo chiaro.
- `OPENCLAW_THEME=dark`: forza la palette TUI scura.
- `COLORFGBG`: se il terminale la esporta, OpenClaw usa l’indizio sul colore di sfondo per scegliere automaticamente la palette TUI.

## [​](https://docs.openclaw.ai/it/help/environment\#sostituzione-delle-variabili-d%E2%80%99ambiente-nella-configurazione)  Sostituzione delle variabili d’ambiente nella configurazione

Puoi fare riferimento direttamente alle variabili d’ambiente nei valori stringa della configurazione usando la sintassi `${VAR_NAME}`:

```
{
  models: {
    providers: {
      "vercel-gateway": {
        apiKey: "${VERCEL_GATEWAY_API_KEY}",
      },
    },
  },
}
```

Consulta [Configurazione: sostituzione delle variabili d’ambiente](https://docs.openclaw.ai/it/gateway/configuration-reference#env-var-substitution) per i dettagli completi.

## [​](https://docs.openclaw.ai/it/help/environment\#riferimenti-ai-segreti-rispetto-alle-stringhe-$env)  Riferimenti ai segreti rispetto alle stringhe `${ENV}`

OpenClaw supporta due pattern basati sull’ambiente:

- Sostituzione di stringhe `${VAR}` nei valori di configurazione.
- Oggetti SecretRef (`{ source: "env", provider: "default", id: "VAR" }`) per i campi che supportano i riferimenti ai segreti.

Entrambi vengono risolti dall’ambiente del processo al momento dell’attivazione. I dettagli di SecretRef sono documentati in [Gestione dei segreti](https://docs.openclaw.ai/it/gateway/secrets).

## [​](https://docs.openclaw.ai/it/help/environment\#variabili-d%E2%80%99ambiente-relative-ai-percorsi)  Variabili d’ambiente relative ai percorsi

| Variabile | Scopo |
| --- | --- |
| `OPENCLAW_HOME` | Sovrascrive la directory home usata per tutta la risoluzione dei percorsi interni (`~/.openclaw/`, directory degli agenti, sessioni, credenziali). Utile quando si esegue OpenClaw come utente di servizio dedicato. |
| `OPENCLAW_STATE_DIR` | Sovrascrive la directory di stato (predefinita `~/.openclaw`). |
| `OPENCLAW_CONFIG_PATH` | Sovrascrive il percorso del file di configurazione (predefinito `~/.openclaw/openclaw.json`). |
| `OPENCLAW_INCLUDE_ROOTS` | Elenco di percorsi di directory in cui le direttive `$include` possono risolvere file fuori dalla directory di configurazione (predefinito: nessuno — `$include` è confinato alla directory di configurazione). Espande la tilde. |

## [​](https://docs.openclaw.ai/it/help/environment\#logging)  Logging

| Variabile | Scopo |
| --- | --- |
| `OPENCLAW_LOG_LEVEL` | Sovrascrive il livello di log sia per file sia per console (ad es. `debug`, `trace`). Ha precedenza su `logging.level` e `logging.consoleLevel` nella configurazione. I valori non validi vengono ignorati con un avviso. |

### [​](https://docs.openclaw.ai/it/help/environment\#openclaw_home)  `OPENCLAW_HOME`

Quando impostata, `OPENCLAW_HOME` sostituisce la directory home di sistema (`$HOME` / `os.homedir()`) per tutta la risoluzione dei percorsi interni. Questo abilita l’isolamento completo del filesystem per gli account di servizio headless.**Precedenza:**`OPENCLAW_HOME` \> `$HOME` \> `USERPROFILE` \> `os.homedir()`**Esempio** (macOS LaunchDaemon):

```
<key>EnvironmentVariables</key>
<dict>
  <key>OPENCLAW_HOME</key>
  <string>/Users/user</string>
</dict>
```

`OPENCLAW_HOME` può anche essere impostata su un percorso con tilde (ad es. `~/svc`), che viene espanso usando `$HOME` prima dell’uso.

## [​](https://docs.openclaw.ai/it/help/environment\#utenti-nvm-errori-tls-di-web_fetch)  Utenti nvm: errori TLS di web\_fetch

Se Node.js è stato installato tramite **nvm** (non tramite il gestore di pacchetti del sistema), il `fetch()` integrato usa
lo store CA incluso in nvm, che potrebbe non includere CA radice moderne (ISRG Root X1/X2 per Let’s Encrypt,
DigiCert Global Root G2, ecc.). Questo fa sì che `web_fetch` fallisca con `"fetch failed"` sulla maggior parte dei siti HTTPS.Su Linux, OpenClaw rileva automaticamente nvm e applica la correzione nell’ambiente di avvio effettivo:

- `openclaw gateway install` scrive `NODE_EXTRA_CA_CERTS` nell’ambiente del servizio systemd
- l’entrypoint CLI `openclaw` riesegue se stesso con `NODE_EXTRA_CA_CERTS` impostata prima dell’avvio di Node

**Correzione manuale (per versioni precedenti o avvii diretti `node ...`):**Esporta la variabile prima di avviare OpenClaw:

```
export NODE_EXTRA_CA_CERTS=/etc/ssl/certs/ca-certificates.crt
openclaw gateway run
```

Non fare affidamento sulla scrittura solo in `~/.openclaw/.env` per questa variabile; Node legge
`NODE_EXTRA_CA_CERTS` all’avvio del processo.

## [​](https://docs.openclaw.ai/it/help/environment\#variabili-d%E2%80%99ambiente-legacy)  Variabili d’ambiente legacy

OpenClaw legge solo variabili d’ambiente `OPENCLAW_*`. I prefissi legacy
`CLAWDBOT_*` e `MOLTBOT_*` delle versioni precedenti vengono ignorati silenziosamente.Se qualcuno è ancora impostato sul processo Gateway all’avvio, OpenClaw emette un
singolo avviso di deprecazione Node (`OPENCLAW_LEGACY_ENV_VARS`) che elenca i
prefissi rilevati e il conteggio totale. Rinomina ogni valore sostituendo il
prefisso legacy con `OPENCLAW_` (per esempio `CLAWDBOT_GATEWAY_TOKEN` →
`OPENCLAW_GATEWAY_TOKEN`); i vecchi nomi non hanno alcun effetto.

## [​](https://docs.openclaw.ai/it/help/environment\#correlati)  Correlati

- [Configurazione del Gateway](https://docs.openclaw.ai/it/gateway/configuration)
- [FAQ: variabili d’ambiente e caricamento di .env](https://docs.openclaw.ai/it/help/faq#env-vars-and-env-loading)
- [Panoramica dei modelli](https://docs.openclaw.ai/it/concepts/models)

[Live tests](https://docs.openclaw.ai/it/help/testing-live) [Flag di diagnostica](https://docs.openclaw.ai/it/diagnostics/flags)

Ctrl+I