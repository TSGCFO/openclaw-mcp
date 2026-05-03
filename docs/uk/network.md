---
source_url: https://docs.openclaw.ai/uk/network
title: "\u041c\u0435\u0440\u0435\u0436\u0430 - OpenClaw"
---

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/network#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/uk)

![UA](https://d3gk2c5xim1je2.cloudfront.net/flags/UA.svg)

Українська

Пошук...

Ctrl K

Пошук...

Navigation

Networking and discovery

Мережа

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Мережевий хаб](https://docs.openclaw.ai/uk/network#%D0%BC%D0%B5%D1%80%D0%B5%D0%B6%D0%B5%D0%B2%D0%B8%D0%B9-%D1%85%D0%B0%D0%B1)
- [Основна модель](https://docs.openclaw.ai/uk/network#%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D0%B0-%D0%BC%D0%BE%D0%B4%D0%B5%D0%BB%D1%8C)
- [Сполучення та ідентичність](https://docs.openclaw.ai/uk/network#%D1%81%D0%BF%D0%BE%D0%BB%D1%83%D1%87%D0%B5%D0%BD%D0%BD%D1%8F-%D1%82%D0%B0-%D1%96%D0%B4%D0%B5%D0%BD%D1%82%D0%B8%D1%87%D0%BD%D1%96%D1%81%D1%82%D1%8C)
- [Виявлення та транспорти](https://docs.openclaw.ai/uk/network#%D0%B2%D0%B8%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D1%82%D0%B0-%D1%82%D1%80%D0%B0%D0%BD%D1%81%D0%BF%D0%BE%D1%80%D1%82%D0%B8)
- [Nodes і транспорти](https://docs.openclaw.ai/uk/network#nodes-%D1%96-%D1%82%D1%80%D0%B0%D0%BD%D1%81%D0%BF%D0%BE%D1%80%D1%82%D0%B8)
- [Безпека](https://docs.openclaw.ai/uk/network#%D0%B1%D0%B5%D0%B7%D0%BF%D0%B5%D0%BA%D0%B0)
- [Пов’язане](https://docs.openclaw.ai/uk/network#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/uk/network\#%D0%BC%D0%B5%D1%80%D0%B5%D0%B6%D0%B5%D0%B2%D0%B8%D0%B9-%D1%85%D0%B0%D0%B1)  Мережевий хаб

Цей хаб містить посилання на основну документацію про те, як OpenClaw підключає, сполучає та захищає
пристрої через localhost, LAN і tailnet.

## [​](https://docs.openclaw.ai/uk/network\#%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D0%B0-%D0%BC%D0%BE%D0%B4%D0%B5%D0%BB%D1%8C)  Основна модель

Більшість операцій проходить через Gateway (`openclaw gateway`) — єдиний довготривалий процес, який керує підключеннями каналів і площиною керування WebSocket.

- **Спочатку loopback**: WS Gateway типово використовує `ws://127.0.0.1:18789`.
Прив’язки не до loopback вимагають дійсного шляху автентифікації gateway: автентифікація
токеном/паролем зі спільним секретом або правильно налаштоване розгортання
`trusted-proxy` не на loopback.
- **Один Gateway на хост** — рекомендований варіант. Для ізоляції запускайте кілька gateway з ізольованими профілями та портами ( [Кілька Gateway](https://docs.openclaw.ai/uk/gateway/multiple-gateways)).
- **Canvas host** обслуговується на тому самому порту, що й Gateway (`/__openclaw__/canvas/`, `/__openclaw__/a2ui/`), і захищений автентифікацією Gateway, коли прив’язаний не лише до loopback.
- **Віддалений доступ** зазвичай здійснюється через SSH tunnel або VPN Tailscale ( [Віддалений доступ](https://docs.openclaw.ai/uk/gateway/remote)).

Ключові посилання:

- [Архітектура Gateway](https://docs.openclaw.ai/uk/concepts/architecture)
- [Протокол Gateway](https://docs.openclaw.ai/uk/gateway/protocol)
- [Runbook Gateway](https://docs.openclaw.ai/uk/gateway)
- [Вебповерхні та режими прив’язки](https://docs.openclaw.ai/uk/web)

## [​](https://docs.openclaw.ai/uk/network\#%D1%81%D0%BF%D0%BE%D0%BB%D1%83%D1%87%D0%B5%D0%BD%D0%BD%D1%8F-%D1%82%D0%B0-%D1%96%D0%B4%D0%B5%D0%BD%D1%82%D0%B8%D1%87%D0%BD%D1%96%D1%81%D1%82%D1%8C)  Сполучення та ідентичність

- [Огляд сполучення (DM + nodes)](https://docs.openclaw.ai/uk/channels/pairing)
- [Сполучення node під керуванням Gateway](https://docs.openclaw.ai/uk/gateway/pairing)
- [CLI Devices (сполучення + ротація токенів)](https://docs.openclaw.ai/uk/cli/devices)
- [CLI Pairing (схвалення DM)](https://docs.openclaw.ai/uk/cli/pairing)

Локальна довіра:

- Прямі локальні підключення loopback можуть автоматично схвалюватися для сполучення, щоб
забезпечити зручний UX на тому самому хості.
- OpenClaw також має вузький шлях самопідключення backend/container-local для
довірених допоміжних потоків зі спільним секретом.
- Клієнти tailnet і LAN, включно з прив’язками tailnet на тому самому хості, все одно потребують
явного схвалення сполучення.

## [​](https://docs.openclaw.ai/uk/network\#%D0%B2%D0%B8%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D1%82%D0%B0-%D1%82%D1%80%D0%B0%D0%BD%D1%81%D0%BF%D0%BE%D1%80%D1%82%D0%B8)  Виявлення та транспорти

- [Виявлення та транспорти](https://docs.openclaw.ai/uk/gateway/discovery)
- [Bonjour / mDNS](https://docs.openclaw.ai/uk/gateway/bonjour)
- [Віддалений доступ (SSH)](https://docs.openclaw.ai/uk/gateway/remote)
- [Tailscale](https://docs.openclaw.ai/uk/gateway/tailscale)

## [​](https://docs.openclaw.ai/uk/network\#nodes-%D1%96-%D1%82%D1%80%D0%B0%D0%BD%D1%81%D0%BF%D0%BE%D1%80%D1%82%D0%B8)  Nodes і транспорти

- [Огляд Nodes](https://docs.openclaw.ai/uk/nodes)
- [Протокол Bridge (застарілі nodes, історично)](https://docs.openclaw.ai/uk/gateway/bridge-protocol)
- [Runbook node: iOS](https://docs.openclaw.ai/uk/platforms/ios)
- [Runbook node: Android](https://docs.openclaw.ai/uk/platforms/android)

## [​](https://docs.openclaw.ai/uk/network\#%D0%B1%D0%B5%D0%B7%D0%BF%D0%B5%D0%BA%D0%B0)  Безпека

- [Огляд безпеки](https://docs.openclaw.ai/uk/gateway/security)
- [Довідка з конфігурації Gateway](https://docs.openclaw.ai/uk/gateway/configuration)
- [Усунення несправностей](https://docs.openclaw.ai/uk/gateway/troubleshooting)
- [Doctor](https://docs.openclaw.ai/uk/gateway/doctor)

## [​](https://docs.openclaw.ai/uk/network\#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)  Пов’язане

- [Мережева модель Gateway](https://docs.openclaw.ai/uk/gateway/network-model)
- [Віддалений доступ](https://docs.openclaw.ai/uk/gateway/remote)

[Локальні моделі](https://docs.openclaw.ai/uk/gateway/local-models) [Мережева модель](https://docs.openclaw.ai/uk/gateway/network-model)

Ctrl+I