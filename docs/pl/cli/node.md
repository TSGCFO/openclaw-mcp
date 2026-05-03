---
source_url: https://docs.openclaw.ai/pl/cli/node
title: "Node - OpenClaw"
---

[Przejdź do głównej treści](https://docs.openclaw.ai/pl/cli/node#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/pl)

![PL](https://d3gk2c5xim1je2.cloudfront.net/flags/PL.svg)

Polski

Szukaj...

Ctrl K

Szukaj...

Navigation

Tools and execution

Node

[Get started](https://docs.openclaw.ai/pl) [Install](https://docs.openclaw.ai/pl/install) [Channels](https://docs.openclaw.ai/pl/channels) [Agents](https://docs.openclaw.ai/pl/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pl/tools) [Models](https://docs.openclaw.ai/pl/providers) [Platforms](https://docs.openclaw.ai/pl/platforms) [Gateway & Ops](https://docs.openclaw.ai/pl/gateway) [Reference](https://docs.openclaw.ai/pl/cli) [Help](https://docs.openclaw.ai/pl/help)

Na tej stronie

- [openclaw node](https://docs.openclaw.ai/pl/cli/node#openclaw-node)
- [Dlaczego warto używać hosta Node?](https://docs.openclaw.ai/pl/cli/node#dlaczego-warto-u%C5%BCywa%C4%87-hosta-node)
- [Proxy przeglądarki (zero-config)](https://docs.openclaw.ai/pl/cli/node#proxy-przegl%C4%85darki-zero-config)
- [Uruchomienie (na pierwszym planie)](https://docs.openclaw.ai/pl/cli/node#uruchomienie-na-pierwszym-planie)
- [Uwierzytelnianie Gateway dla hosta Node](https://docs.openclaw.ai/pl/cli/node#uwierzytelnianie-gateway-dla-hosta-node)
- [Usługa (w tle)](https://docs.openclaw.ai/pl/cli/node#us%C5%82uga-w-tle)
- [Pairing](https://docs.openclaw.ai/pl/cli/node#pairing)
- [Zatwierdzenia exec](https://docs.openclaw.ai/pl/cli/node#zatwierdzenia-exec)
- [Powiązane](https://docs.openclaw.ai/pl/cli/node#powi%C4%85zane)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/pl/cli/node\#openclaw-node)  `openclaw node`

Uruchamia **bezgłowego hosta Node**, który łączy się z Gateway WebSocket i udostępnia
na tej maszynie `system.run` / `system.which`.

## [​](https://docs.openclaw.ai/pl/cli/node\#dlaczego-warto-u%C5%BCywa%C4%87-hosta-node)  Dlaczego warto używać hosta Node?

Użyj hosta Node, gdy chcesz, aby agenci **uruchamiali polecenia na innych maszynach**
w Twojej sieci bez instalowania na nich pełnej aplikacji towarzyszącej dla macOS.Typowe przypadki użycia:

- Uruchamianie poleceń na zdalnych maszynach Linux/Windows (serwery buildów, maszyny laboratoryjne, NAS).
- Utrzymanie **sandboxingu** exec na Gateway, ale delegowanie zatwierdzonych uruchomień do innych hostów.
- Zapewnienie lekkiego, bezgłowego celu wykonawczego dla automatyzacji lub węzłów CI.

Wykonanie nadal jest chronione przez **zatwierdzenia exec** i listy dozwolonych per agent
na hoście Node, dzięki czemu możesz zachować zakres dostępu do poleceń jako ograniczony i jawny.

## [​](https://docs.openclaw.ai/pl/cli/node\#proxy-przegl%C4%85darki-zero-config)  Proxy przeglądarki (zero-config)

Hosty Node automatycznie anonsują proxy przeglądarki, jeśli `browser.enabled` nie jest
wyłączone na węźle. Pozwala to agentowi używać automatyzacji przeglądarki na tym węźle
bez dodatkowej konfiguracji.Domyślnie proxy udostępnia zwykłą powierzchnię profilu przeglądarki węzła. Jeśli
ustawisz `nodeHost.browserProxy.allowProfiles`, proxy staje się restrykcyjne:
adresowanie profili spoza listy dozwolonych jest odrzucane, a trasy tworzenia/usuwania
trwałych profili są blokowane przez proxy.W razie potrzeby wyłącz je na węźle:

```
{
  nodeHost: {
    browserProxy: {
      enabled: false,
    },
  },
}
```

## [​](https://docs.openclaw.ai/pl/cli/node\#uruchomienie-na-pierwszym-planie)  Uruchomienie (na pierwszym planie)

```
openclaw node run --host <gateway-host> --port 18789
```

Opcje:

- `--host <host>`: host Gateway WebSocket (domyślnie: `127.0.0.1`)
- `--port <port>`: port Gateway WebSocket (domyślnie: `18789`)
- `--tls`: używa TLS dla połączenia z Gateway
- `--tls-fingerprint <sha256>`: oczekiwany fingerprint certyfikatu TLS (sha256)
- `--node-id <id>`: nadpisuje identyfikator Node (czyści token Pairing)
- `--display-name <name>`: nadpisuje wyświetlaną nazwę węzła

## [​](https://docs.openclaw.ai/pl/cli/node\#uwierzytelnianie-gateway-dla-hosta-node)  Uwierzytelnianie Gateway dla hosta Node

`openclaw node run` i `openclaw node install` rozwiązują uwierzytelnianie Gateway z config/env (polecenia node nie mają flag `--token`/`--password`):

- Najpierw sprawdzane są `OPENCLAW_GATEWAY_TOKEN` / `OPENCLAW_GATEWAY_PASSWORD`.
- Następnie fallback do lokalnej konfiguracji: `gateway.auth.token` / `gateway.auth.password`.
- W trybie lokalnym host Node celowo nie dziedziczy `gateway.remote.token` / `gateway.remote.password`.
- Jeśli `gateway.auth.token` / `gateway.auth.password` są jawnie skonfigurowane przez SecretRef i nierozwiązane, rozwiązywanie uwierzytelniania Node kończy się w trybie fail-closed (bez maskującego fallbacku z trybu zdalnego).
- W `gateway.mode=remote` pola klienta zdalnego (`gateway.remote.token` / `gateway.remote.password`) także kwalifikują się zgodnie z regułami priorytetu trybu zdalnego.
- Rozwiązywanie uwierzytelniania hosta Node honoruje tylko zmienne środowiskowe `OPENCLAW_GATEWAY_*`.

Dla węzła łączącego się z Gateway `ws://` innym niż local loopback w zaufanej sieci
prywatnej ustaw `OPENCLAW_ALLOW_INSECURE_PRIVATE_WS=1`. Bez tego uruchomienie węzła
kończy się w trybie fail-closed i prosi o użycie `wss://`, tunelu SSH lub Tailscale.
Jest to opt-in na poziomie środowiska procesu, a nie klucz konfiguracji `openclaw.json`.
`openclaw node install` utrwala tę wartość w nadzorowanej usłudze węzła, gdy jest ona
obecna w środowisku polecenia instalacji.

## [​](https://docs.openclaw.ai/pl/cli/node\#us%C5%82uga-w-tle)  Usługa (w tle)

Zainstaluj bezgłowego hosta Node jako usługę użytkownika.

```
openclaw node install --host <gateway-host> --port 18789
```

Opcje:

- `--host <host>`: host Gateway WebSocket (domyślnie: `127.0.0.1`)
- `--port <port>`: port Gateway WebSocket (domyślnie: `18789`)
- `--tls`: używa TLS dla połączenia z Gateway
- `--tls-fingerprint <sha256>`: oczekiwany fingerprint certyfikatu TLS (sha256)
- `--node-id <id>`: nadpisuje identyfikator Node (czyści token Pairing)
- `--display-name <name>`: nadpisuje wyświetlaną nazwę węzła
- `--runtime <runtime>`: runtime usługi (`node` lub `bun`)
- `--force`: reinstaluje/nadpisuje, jeśli już zainstalowane

Zarządzanie usługą:

```
openclaw node status
openclaw node start
openclaw node stop
openclaw node restart
openclaw node uninstall
```

Użyj `openclaw node run` dla hosta Node na pierwszym planie (bez usługi).Polecenia usługi akceptują `--json` dla danych wyjściowych czytelnych maszynowo.Host Node ponawia restart Gateway i zamknięcia sieci w obrębie procesu. Jeśli
Gateway zgłosi terminalną pauzę uwierzytelniania tokenu/hasła/bootstrap, host Node
loguje szczegóły zamknięcia i kończy się niezerowym kodem, aby launchd/systemd mogły
uruchomić go ponownie ze świeżą konfiguracją i poświadczeniami. Pauzy wymagające Pairing
pozostają w przepływie na pierwszym planie, aby oczekujące żądanie mogło zostać zatwierdzone.

## [​](https://docs.openclaw.ai/pl/cli/node\#pairing)  Pairing

Pierwsze połączenie tworzy w Gateway oczekujące żądanie Pairing urządzenia (`role: node`).
Zatwierdź je przez:

```
openclaw devices list
openclaw devices approve <requestId>
```

W ściśle kontrolowanych sieciach węzłów operator Gateway może jawnie włączyć
automatyczne zatwierdzanie pierwszego Pairing węzła z zaufanych zakresów CIDR:

```
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24"],
      },
    },
  },
}
```

To ustawienie jest domyślnie wyłączone. Dotyczy tylko świeżego Pairing `role: node` bez
żądanych zakresów. Klienci operator/browser, Control UI, WebChat oraz aktualizacje roli,
zakresów, metadanych lub klucza publicznego nadal wymagają ręcznego zatwierdzenia.Jeśli węzeł ponowi próbę Pairing ze zmienionymi szczegółami uwierzytelniania
(rola/zakresy/klucz publiczny), poprzednie oczekujące żądanie zostaje zastąpione,
a tworzony jest nowy `requestId`.
Uruchom ponownie `openclaw devices list` przed zatwierdzeniem.Host Node przechowuje identyfikator Node, token, wyświetlaną nazwę i informacje o połączeniu z Gateway w
`~/.openclaw/node.json`.

## [​](https://docs.openclaw.ai/pl/cli/node\#zatwierdzenia-exec)  Zatwierdzenia exec

`system.run` jest chronione przez lokalne zatwierdzenia exec:

- `~/.openclaw/exec-approvals.json`
- [Exec approvals](https://docs.openclaw.ai/pl/tools/exec-approvals)
- `openclaw approvals --node <id|name|ip>` (edycja z Gateway)

Dla zatwierdzonego asynchronicznego exec na Node OpenClaw przygotowuje kanoniczny
`systemRunPlan` przed wyświetleniem promptu. Późniejsze zatwierdzone przekazanie
`system.run` ponownie używa tego zapisanego planu, więc edycje pól command/cwd/session
po utworzeniu żądania zatwierdzenia są odrzucane zamiast zmieniać to, co wykona węzeł.

## [​](https://docs.openclaw.ai/pl/cli/node\#powi%C4%85zane)  Powiązane

- [Dokumentacja CLI](https://docs.openclaw.ai/pl/cli)
- [Nodes](https://docs.openclaw.ai/pl/nodes)

[Flows (przekierowanie)](https://docs.openclaw.ai/pl/cli/flows) [Węzły](https://docs.openclaw.ai/pl/cli/nodes)

Ctrl+I