---
source_url: https://docs.openclaw.ai/pl/start/wizard
title: "Wprowadzenie (CLI) - OpenClaw"
---

[Przejdź do głównej treści](https://docs.openclaw.ai/pl/start/wizard#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/pl)

![PL](https://d3gk2c5xim1je2.cloudfront.net/flags/PL.svg)

Polski

Szukaj...

Ctrl K

Szukaj...

Navigation

First steps

Wprowadzenie (CLI)

[Get started](https://docs.openclaw.ai/pl) [Install](https://docs.openclaw.ai/pl/install) [Channels](https://docs.openclaw.ai/pl/channels) [Agents](https://docs.openclaw.ai/pl/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pl/tools) [Models](https://docs.openclaw.ai/pl/providers) [Platforms](https://docs.openclaw.ai/pl/platforms) [Gateway & Ops](https://docs.openclaw.ai/pl/gateway) [Reference](https://docs.openclaw.ai/pl/cli) [Help](https://docs.openclaw.ai/pl/help)

Na tej stronie

- [QuickStart kontra Zaawansowane](https://docs.openclaw.ai/pl/start/wizard#quickstart-kontra-zaawansowane)
- [Co konfiguruje onboarding](https://docs.openclaw.ai/pl/start/wizard#co-konfiguruje-onboarding)
- [Dodaj kolejnego agenta](https://docs.openclaw.ai/pl/start/wizard#dodaj-kolejnego-agenta)
- [Pełna dokumentacja](https://docs.openclaw.ai/pl/start/wizard#pe%C5%82na-dokumentacja)
- [Powiązana dokumentacja](https://docs.openclaw.ai/pl/start/wizard#powi%C4%85zana-dokumentacja)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Onboarding przez CLI to **zalecany** sposób konfiguracji OpenClaw na macOS,
Linuksie lub Windows (przez WSL2; zdecydowanie zalecane).
Konfiguruje lokalny Gateway albo połączenie ze zdalnym Gateway, a także kanały, Skills
i domyślne ustawienia obszaru roboczego w jednym prowadzonym procesie.

```
openclaw onboard
```

Najszybszy pierwszy czat: otwórz Control UI (konfiguracja kanału nie jest potrzebna). Uruchom
`openclaw dashboard` i rozmawiaj w przeglądarce. Dokumentacja: [Dashboard](https://docs.openclaw.ai/pl/web/dashboard).

Aby później ponownie skonfigurować:

```
openclaw configure
openclaw agents add <name>
```

`--json` nie oznacza trybu nieinteraktywnego. W skryptach używaj `--non-interactive`.

Onboarding przez CLI obejmuje krok wyszukiwania w sieci, w którym możesz wybrać dostawcę,
takiego jak Brave, DuckDuckGo, Exa, Firecrawl, Gemini, Grok, Kimi, MiniMax Search,
Ollama Web Search, Perplexity, SearXNG albo Tavily. Niektórzy dostawcy wymagają
klucza API, a inni go nie wymagają. Możesz też skonfigurować to później za pomocą
`openclaw configure --section web`. Dokumentacja: [Narzędzia webowe](https://docs.openclaw.ai/pl/tools/web).

## [​](https://docs.openclaw.ai/pl/start/wizard\#quickstart-kontra-zaawansowane)  QuickStart kontra Zaawansowane

Onboarding zaczyna się od wyboru **QuickStart** (ustawienia domyślne) albo **Zaawansowane** (pełna kontrola).

- QuickStart (ustawienia domyślne)

- Zaawansowane (pełna kontrola)


- Lokalny Gateway (loopback)
- Domyślny obszar roboczy (albo istniejący obszar roboczy)
- Port Gateway **18789**
- Uwierzytelnianie Gateway **Token** (generowane automatycznie, nawet na loopback)
- Domyślna polityka narzędzi dla nowych konfiguracji lokalnych: `tools.profile: "coding"` (istniejący jawny profil zostaje zachowany)
- Domyślna izolacja DM: lokalny onboarding zapisuje `session.dmScope: "per-channel-peer"`, gdy wartość nie jest ustawiona. Szczegóły: [Dokumentacja konfiguracji CLI](https://docs.openclaw.ai/pl/start/wizard-cli-reference#outputs-and-internals)
- Ekspozycja Tailscale **Wyłączona**
- DM w Telegram + WhatsApp domyślnie używają **listy dozwolonych** (pojawi się prośba o podanie numeru telefonu)

- Udostępnia każdy krok (tryb, obszar roboczy, Gateway, kanały, demon, Skills).

## [​](https://docs.openclaw.ai/pl/start/wizard\#co-konfiguruje-onboarding)  Co konfiguruje onboarding

**Tryb lokalny (domyślny)** przeprowadza Cię przez te kroki:

1. **Model/Uwierzytelnianie** — wybierz dowolnego obsługiwanego dostawcę albo przepływ uwierzytelniania (klucz API, OAuth albo ręczne uwierzytelnianie specyficzne dla dostawcy), w tym Custom Provider
(zgodny z OpenAI, zgodny z Anthropic albo Unknown z automatycznym wykrywaniem). Wybierz model domyślny.
Uwaga dotycząca bezpieczeństwa: jeśli ten agent będzie uruchamiać narzędzia albo przetwarzać treści z webhooków/hooków, wybierz najsilniejszy dostępny model najnowszej generacji i utrzymuj rygorystyczną politykę narzędzi. Słabsze/starsze poziomy są bardziej podatne na prompt injection.
W uruchomieniach nieinteraktywnych `--secret-input-mode ref` zapisuje odwołania oparte na env w profilach uwierzytelniania zamiast jawnych wartości kluczy API.
W nieinteraktywnym trybie `ref` zmienna env dostawcy musi być ustawiona; przekazanie flag z kluczem inline bez tej zmiennej env kończy się szybkim błędem.
W uruchomieniach interaktywnych wybór trybu odwołania do sekretu pozwala wskazać zmienną środowiskową albo skonfigurowane odwołanie dostawcy (`file` lub `exec`), z szybką walidacją preflight przed zapisaniem.
Dla Anthropic interaktywny onboarding/configure oferuje **Anthropic Claude CLI** jako preferowaną lokalną ścieżkę oraz **klucz API Anthropic** jako zalecaną ścieżkę produkcyjną. Anthropic setup-token pozostaje też dostępny jako obsługiwana ścieżka uwierzytelniania tokenem.
2. **Obszar roboczy** — lokalizacja plików agenta (domyślnie `~/.openclaw/workspace`). Dodaje początkowe pliki bootstrap.
3. **Gateway** — port, adres powiązania, tryb uwierzytelniania, ekspozycja Tailscale.
W interaktywnym trybie tokena wybierz domyślne przechowywanie tokena w postaci jawnego tekstu albo włącz SecretRef.
Nieinteraktywna ścieżka SecretRef dla tokena: `--gateway-token-ref-env <ENV_VAR>`.
4. **Kanały** — wbudowane i dołączone kanały czatu, takie jak BlueBubbles, Discord, Feishu, Google Chat, Mattermost, Microsoft Teams, QQ Bot, Signal, Slack, Telegram, WhatsApp i inne.
5. **Demon** — instaluje LaunchAgent (macOS), jednostkę użytkownika systemd (Linux/WSL2) albo natywne Zaplanowane zadanie Windows z awaryjną obsługą folderu Autostart dla użytkownika.
Jeśli uwierzytelnianie tokenem wymaga tokena, a `gateway.auth.token` jest zarządzane przez SecretRef, instalacja demona je waliduje, ale nie utrwala rozwiązanego tokena w metadanych środowiska usługi nadzorującej.
Jeśli uwierzytelnianie tokenem wymaga tokena, a skonfigurowany SecretRef tokena nie jest rozwiązany, instalacja demona zostaje zablokowana z praktycznymi wskazówkami.
Jeśli skonfigurowane są zarówno `gateway.auth.token`, jak i `gateway.auth.password`, a `gateway.auth.mode` nie jest ustawione, instalacja demona zostaje zablokowana do czasu jawnego ustawienia trybu.
6. **Kontrola stanu** — uruchamia Gateway i sprawdza, czy działa.
7. **Skills** — instaluje zalecane Skills i opcjonalne zależności.

Ponowne uruchomienie onboardingu **nie** usuwa niczego, chyba że jawnie wybierzesz **Resetuj** (albo przekażesz `--reset`).
CLI `--reset` domyślnie obejmuje konfigurację, poświadczenia i sesje; użyj `--reset-scope full`, aby uwzględnić obszar roboczy.
Jeśli konfiguracja jest nieprawidłowa albo zawiera przestarzałe klucze, onboarding poprosi najpierw o uruchomienie `openclaw doctor`.

**Tryb zdalny** konfiguruje tylko lokalnego klienta do łączenia się z Gateway w innym miejscu.
Nie instaluje ani nie zmienia niczego na zdalnym hoście.

## [​](https://docs.openclaw.ai/pl/start/wizard\#dodaj-kolejnego-agenta)  Dodaj kolejnego agenta

Użyj `openclaw agents add <name>`, aby utworzyć osobnego agenta z własnym obszarem roboczym,
sesjami i profilami uwierzytelniania. Uruchomienie bez `--workspace` rozpoczyna onboarding.Co ustawia:

- `agents.list[].name`
- `agents.list[].workspace`
- `agents.list[].agentDir`

Uwagi:

- Domyślne obszary robocze mają postać `~/.openclaw/workspace-<agentId>`.
- Dodaj `bindings`, aby trasować wiadomości przychodzące (onboarding może to zrobić).
- Flagi nieinteraktywne: `--model`, `--agent-dir`, `--bind`, `--non-interactive`.

## [​](https://docs.openclaw.ai/pl/start/wizard\#pe%C5%82na-dokumentacja)  Pełna dokumentacja

Szczegółowe omówienia krok po kroku i wyniki konfiguracji znajdziesz w
[Dokumentacji konfiguracji CLI](https://docs.openclaw.ai/pl/start/wizard-cli-reference).
Przykłady nieinteraktywne znajdziesz w [Automatyzacji CLI](https://docs.openclaw.ai/pl/start/wizard-cli-automation).
Głębszą dokumentację techniczną, w tym szczegóły RPC, znajdziesz w
[Dokumentacji onboardingu](https://docs.openclaw.ai/pl/reference/wizard).

## [​](https://docs.openclaw.ai/pl/start/wizard\#powi%C4%85zana-dokumentacja)  Powiązana dokumentacja

- Dokumentacja poleceń CLI: [`openclaw onboard`](https://docs.openclaw.ai/pl/cli/onboard)
- Omówienie onboardingu: [Omówienie onboardingu](https://docs.openclaw.ai/pl/start/onboarding-overview)
- Onboarding aplikacji macOS: [Onboarding](https://docs.openclaw.ai/pl/start/onboarding)
- Rytuał pierwszego uruchomienia agenta: [Bootstrap agenta](https://docs.openclaw.ai/pl/start/bootstrapping)

[Onboarding Overview](https://docs.openclaw.ai/pl/start/onboarding-overview) [Onboarding: macOS App](https://docs.openclaw.ai/pl/start/onboarding)

Ctrl+I