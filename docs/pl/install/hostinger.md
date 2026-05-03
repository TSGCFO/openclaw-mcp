---
source_url: https://docs.openclaw.ai/pl/install/hostinger
title: "Hostinger - OpenClaw"
---

[Przejdź do głównej treści](https://docs.openclaw.ai/pl/install/hostinger#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/pl)

![PL](https://d3gk2c5xim1je2.cloudfront.net/flags/PL.svg)

Polski

Szukaj...

Ctrl K

Szukaj...

Navigation

Hosting

Hostinger

[Get started](https://docs.openclaw.ai/pl) [Install](https://docs.openclaw.ai/pl/install) [Channels](https://docs.openclaw.ai/pl/channels) [Agents](https://docs.openclaw.ai/pl/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pl/tools) [Models](https://docs.openclaw.ai/pl/providers) [Platforms](https://docs.openclaw.ai/pl/platforms) [Gateway & Ops](https://docs.openclaw.ai/pl/gateway) [Reference](https://docs.openclaw.ai/pl/cli) [Help](https://docs.openclaw.ai/pl/help)

Na tej stronie

- [Wymagania wstępne](https://docs.openclaw.ai/pl/install/hostinger#wymagania-wst%C4%99pne)
- [Opcja A: OpenClaw 1-Click](https://docs.openclaw.ai/pl/install/hostinger#opcja-a-openclaw-1-click)
- [Opcja B: OpenClaw na VPS](https://docs.openclaw.ai/pl/install/hostinger#opcja-b-openclaw-na-vps)
- [Zweryfikuj konfigurację](https://docs.openclaw.ai/pl/install/hostinger#zweryfikuj-konfiguracj%C4%99)
- [Rozwiązywanie problemów](https://docs.openclaw.ai/pl/install/hostinger#rozwi%C4%85zywanie-problem%C3%B3w)
- [Kolejne kroki](https://docs.openclaw.ai/pl/install/hostinger#kolejne-kroki)
- [Powiązane](https://docs.openclaw.ai/pl/install/hostinger#powi%C4%85zane)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Uruchom trwały Gateway OpenClaw na [Hostinger](https://www.hostinger.com/openclaw) przez zarządzone wdrożenie **1-Click** albo instalację na **VPS**.

## [​](https://docs.openclaw.ai/pl/install/hostinger\#wymagania-wst%C4%99pne)  Wymagania wstępne

- konto Hostinger ( [rejestracja](https://www.hostinger.com/openclaw))
- około 5-10 minut

## [​](https://docs.openclaw.ai/pl/install/hostinger\#opcja-a-openclaw-1-click)  Opcja A: OpenClaw 1-Click

Najszybszy sposób na start. Hostinger obsługuje infrastrukturę, Docker i automatyczne aktualizacje.

1

[Navigate to header](https://docs.openclaw.ai/pl/install/hostinger#)

Kup i uruchom

1. Na [stronie Hostinger OpenClaw](https://www.hostinger.com/openclaw) wybierz plan Managed OpenClaw i dokończ zakup.

Podczas zakupu możesz wybrać kredyty **Ready-to-Use AI**, które są opłacane z góry i od razu integrowane w OpenClaw — bez potrzeby zakładania zewnętrznych kont ani używania kluczy API od innych dostawców. Możesz od razu zacząć czatować. Alternatywnie podczas konfiguracji możesz podać własny klucz od Anthropic, OpenAI, Google Gemini albo xAI.

2

[Navigate to header](https://docs.openclaw.ai/pl/install/hostinger#)

Wybierz kanał wiadomości

Wybierz jeden lub więcej kanałów do połączenia:

- **WhatsApp** — zeskanuj kod QR pokazany w kreatorze konfiguracji.
- **Telegram** — wklej token bota z [BotFather](https://t.me/BotFather).

3

[Navigate to header](https://docs.openclaw.ai/pl/install/hostinger#)

Zakończ instalację

Kliknij **Finish**, aby wdrożyć instancję. Gdy będzie gotowa, uzyskaj dostęp do dashboardu OpenClaw z poziomu **OpenClaw Overview** w hPanel.

## [​](https://docs.openclaw.ai/pl/install/hostinger\#opcja-b-openclaw-na-vps)  Opcja B: OpenClaw na VPS

Większa kontrola nad serwerem. Hostinger wdraża OpenClaw przez Docker na twoim VPS, a ty zarządzasz nim przez **Docker Manager** w hPanel.

1

[Navigate to header](https://docs.openclaw.ai/pl/install/hostinger#)

Kup VPS

1. Na [stronie Hostinger OpenClaw](https://www.hostinger.com/openclaw) wybierz plan OpenClaw on VPS i dokończ zakup.

Podczas zakupu możesz wybrać kredyty **Ready-to-Use AI** — są one opłacane z góry i od razu integrowane w OpenClaw, dzięki czemu możesz zacząć czatować bez żadnych zewnętrznych kont ani kluczy API od innych dostawców.

2

[Navigate to header](https://docs.openclaw.ai/pl/install/hostinger#)

Skonfiguruj OpenClaw

Gdy VPS zostanie przygotowany, wypełnij pola konfiguracji:

- **Gateway token** — generowany automatycznie; zachowaj go na później.
- **WhatsApp number** — twój numer z kodem kraju (opcjonalnie).
- **Telegram bot token** — z [BotFather](https://t.me/BotFather) (opcjonalnie).
- **API keys** — potrzebne tylko wtedy, gdy podczas zakupu nie wybrałeś kredytów Ready-to-Use AI.

3

[Navigate to header](https://docs.openclaw.ai/pl/install/hostinger#)

Uruchom OpenClaw

Kliknij **Deploy**. Gdy wszystko będzie działać, otwórz dashboard OpenClaw z hPanel, klikając **Open**.

Logami, restartami i aktualizacjami zarządza się bezpośrednio z interfejsu Docker Manager w hPanel. Aby zaktualizować, kliknij **Update** w Docker Manager, co pobierze najnowszy obraz.

## [​](https://docs.openclaw.ai/pl/install/hostinger\#zweryfikuj-konfiguracj%C4%99)  Zweryfikuj konfigurację

Wyślij „Hi” do swojego asystenta na podłączonym kanale. OpenClaw odpowie i przeprowadzi cię przez początkowe preferencje.

## [​](https://docs.openclaw.ai/pl/install/hostinger\#rozwi%C4%85zywanie-problem%C3%B3w)  Rozwiązywanie problemów

**Dashboard się nie ładuje** — poczekaj kilka minut, aż kontener zakończy provisioning. Sprawdź logi Docker Manager w hPanel.**Kontener Docker ciągle się restartuje** — otwórz logi Docker Manager i sprawdź błędy konfiguracji (brakujące tokeny, nieprawidłowe klucze API).**Bot Telegram nie odpowiada** — wyślij wiadomość z kodem parowania bezpośrednio z Telegram jako wiadomość w swoim czacie OpenClaw, aby zakończyć połączenie.

## [​](https://docs.openclaw.ai/pl/install/hostinger\#kolejne-kroki)  Kolejne kroki

- [Kanały](https://docs.openclaw.ai/pl/channels) — połącz Telegram, WhatsApp, Discord i inne
- [Konfiguracja Gateway](https://docs.openclaw.ai/pl/gateway/configuration) — wszystkie opcje konfiguracji

## [​](https://docs.openclaw.ai/pl/install/hostinger\#powi%C4%85zane)  Powiązane

- [Przegląd instalacji](https://docs.openclaw.ai/pl/install)
- [Hosting VPS](https://docs.openclaw.ai/pl/vps)
- [DigitalOcean](https://docs.openclaw.ai/pl/install/digitalocean)

[Hetzner](https://docs.openclaw.ai/pl/install/hetzner) [Kubernetes](https://docs.openclaw.ai/pl/install/kubernetes)

Ctrl+I