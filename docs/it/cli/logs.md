---
source_url: https://docs.openclaw.ai/it/cli/logs
title: "Log - OpenClaw"
---

[Vai al contenuto principale](https://docs.openclaw.ai/it/cli/logs#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/it)

![IT](https://d3gk2c5xim1je2.cloudfront.net/flags/IT.svg)

Italiano

Cerca...

Ctrl K

Cerca...

Navigation

Gateway and service

Log

[Get started](https://docs.openclaw.ai/it) [Install](https://docs.openclaw.ai/it/install) [Channels](https://docs.openclaw.ai/it/channels) [Agents](https://docs.openclaw.ai/it/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/it/tools) [Models](https://docs.openclaw.ai/it/providers) [Platforms](https://docs.openclaw.ai/it/platforms) [Gateway & Ops](https://docs.openclaw.ai/it/gateway) [Reference](https://docs.openclaw.ai/it/cli) [Help](https://docs.openclaw.ai/it/help)

In questa pagina

- [openclaw logs](https://docs.openclaw.ai/it/cli/logs#openclaw-logs)
- [Opzioni](https://docs.openclaw.ai/it/cli/logs#opzioni)
- [Opzioni RPC condivise del Gateway](https://docs.openclaw.ai/it/cli/logs#opzioni-rpc-condivise-del-gateway)
- [Esempi](https://docs.openclaw.ai/it/cli/logs#esempi)
- [Note](https://docs.openclaw.ai/it/cli/logs#note)
- [Correlati](https://docs.openclaw.ai/it/cli/logs#correlati)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/it/cli/logs\#openclaw-logs)  `openclaw logs`

Esegue il tail dei log su file del Gateway tramite RPC (funziona in modalità remota).Correlati:

- Panoramica dei log: [Log](https://docs.openclaw.ai/it/logging)
- CLI del Gateway: [gateway](https://docs.openclaw.ai/it/cli/gateway)

## [​](https://docs.openclaw.ai/it/cli/logs\#opzioni)  Opzioni

- `--limit <n>`: numero massimo di righe di log da restituire (predefinito `200`)
- `--max-bytes <n>`: numero massimo di byte da leggere dal file di log (predefinito `250000`)
- `--follow`: segue lo stream dei log
- `--interval <ms>`: intervallo di polling durante il follow (predefinito `1000`)
- `--json`: emette eventi JSON delimitati da righe
- `--plain`: output in testo normale senza formattazione stilizzata
- `--no-color`: disabilita i colori ANSI
- `--local-time`: mostra i timestamp nel tuo fuso orario locale

## [​](https://docs.openclaw.ai/it/cli/logs\#opzioni-rpc-condivise-del-gateway)  Opzioni RPC condivise del Gateway

`openclaw logs` accetta anche i flag client standard del Gateway:

- `--url <url>`: URL WebSocket del Gateway
- `--token <token>`: token del Gateway
- `--timeout <ms>`: timeout in ms (predefinito `30000`)
- `--expect-final`: attende una risposta finale quando la chiamata al Gateway è supportata da un agente

Quando passi `--url`, la CLI non applica automaticamente la configurazione o le credenziali dell’ambiente. Includi `--token` esplicitamente se il Gateway di destinazione richiede l’autenticazione.

## [​](https://docs.openclaw.ai/it/cli/logs\#esempi)  Esempi

```
openclaw logs
openclaw logs --follow
openclaw logs --follow --interval 2000
openclaw logs --limit 500 --max-bytes 500000
openclaw logs --json
openclaw logs --plain
openclaw logs --no-color
openclaw logs --limit 500
openclaw logs --local-time
openclaw logs --follow --local-time
openclaw logs --url ws://127.0.0.1:18789 --token "$OPENCLAW_GATEWAY_TOKEN"
```

## [​](https://docs.openclaw.ai/it/cli/logs\#note)  Note

- Usa `--local-time` per mostrare i timestamp nel tuo fuso orario locale.
- Se il Gateway local loopback implicito richiede l’abbinamento, si chiude durante la connessione o va in timeout prima che `logs.tail` risponda, `openclaw logs` ripiega automaticamente sul log su file del Gateway configurato. Le destinazioni `--url` esplicite non usano questo fallback.

## [​](https://docs.openclaw.ai/it/cli/logs\#correlati)  Correlati

- [Riferimento CLI](https://docs.openclaw.ai/it/cli)
- [Log del Gateway](https://docs.openclaw.ai/it/gateway/logging)

[Salute](https://docs.openclaw.ai/it/cli/health) [Migrare](https://docs.openclaw.ai/it/cli/migrate)

Ctrl+I