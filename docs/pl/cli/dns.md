---
source_url: https://docs.openclaw.ai/pl/cli/dns
title: "DNS - OpenClaw"
---

[Przejdź do głównej treści](https://docs.openclaw.ai/pl/cli/dns#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/pl)

![PL](https://d3gk2c5xim1je2.cloudfront.net/flags/PL.svg)

Polski

Szukaj...

Ctrl K

Szukaj...

Navigation

Utility

DNS

[Get started](https://docs.openclaw.ai/pl) [Install](https://docs.openclaw.ai/pl/install) [Channels](https://docs.openclaw.ai/pl/channels) [Agents](https://docs.openclaw.ai/pl/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pl/tools) [Models](https://docs.openclaw.ai/pl/providers) [Platforms](https://docs.openclaw.ai/pl/platforms) [Gateway & Ops](https://docs.openclaw.ai/pl/gateway) [Reference](https://docs.openclaw.ai/pl/cli) [Help](https://docs.openclaw.ai/pl/help)

Na tej stronie

- [openclaw dns](https://docs.openclaw.ai/pl/cli/dns#openclaw-dns)
- [Konfiguracja](https://docs.openclaw.ai/pl/cli/dns#konfiguracja)
- [dns setup](https://docs.openclaw.ai/pl/cli/dns#dns-setup)
- [Powiązane](https://docs.openclaw.ai/pl/cli/dns#powi%C4%85zane)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/pl/cli/dns\#openclaw-dns)  `openclaw dns`

Narzędzia pomocnicze DNS do wykrywania w sieci rozległej (Tailscale + CoreDNS). Obecnie skupiają się na macOS + CoreDNS z Homebrew.Powiązane:

- Wykrywanie Gateway: [Discovery](https://docs.openclaw.ai/pl/gateway/discovery)
- Konfiguracja wykrywania w sieci rozległej: [Configuration](https://docs.openclaw.ai/pl/gateway/configuration)

## [​](https://docs.openclaw.ai/pl/cli/dns\#konfiguracja)  Konfiguracja

```
openclaw dns setup
openclaw dns setup --domain openclaw.internal
openclaw dns setup --apply
```

## [​](https://docs.openclaw.ai/pl/cli/dns\#dns-setup)  `dns setup`

Zaplanuj lub zastosuj konfigurację CoreDNS dla wykrywania unicast DNS-SD.Opcje:

- `--domain <domain>`: domena wykrywania w sieci rozległej (na przykład `openclaw.internal`)
- `--apply`: zainstaluj lub zaktualizuj konfigurację CoreDNS i uruchom ponownie usługę (wymaga sudo; tylko macOS)

Co pokazuje:

- rozwiązaną domenę wykrywania
- ścieżkę pliku strefy
- bieżące adresy IP tailnet
- zalecaną konfigurację wykrywania `openclaw.json`
- wartości serwera nazw/domeny Tailscale Split DNS do ustawienia

Uwagi:

- Bez `--apply` polecenie jest tylko narzędziem pomocniczym do planowania i wypisuje zalecaną konfigurację.
- Jeśli pominięto `--domain`, OpenClaw używa `discovery.wideArea.domain` z konfiguracji.
- `--apply` obecnie obsługuje tylko macOS i zakłada CoreDNS z Homebrew.
- `--apply` inicjalizuje plik strefy, jeśli to konieczne, zapewnia istnienie sekcji importu CoreDNS i uruchamia ponownie usługę `coredns` z brew.

## [​](https://docs.openclaw.ai/pl/cli/dns\#powi%C4%85zane)  Powiązane

- [Dokumentacja referencyjna CLI](https://docs.openclaw.ai/pl/cli)
- [Discovery](https://docs.openclaw.ai/pl/gateway/discovery)

[Autouzupełnianie](https://docs.openclaw.ai/pl/cli/completion) [Dokumentacja](https://docs.openclaw.ai/pl/cli/docs)

Ctrl+I