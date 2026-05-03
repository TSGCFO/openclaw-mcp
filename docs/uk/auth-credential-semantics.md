---
source_url: https://docs.openclaw.ai/uk/auth-credential-semantics
title: "\u0421\u0435\u043c\u0430\u043d\u0442\u0438\u043a\u0430 \u043e\u0431\u043b\u0456\u043a\u043e\u0432\u0438\u0445 \u0434\u0430\u043d\u0438\u0445 \u0430\u0432\u0442\u0435\u043d\u0442\u0438\u0444\u0456\u043a\u0430\u0446\u0456\u0457 - OpenClaw"
---

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/auth-credential-semantics#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/uk)

![UA](https://d3gk2c5xim1je2.cloudfront.net/flags/UA.svg)

Українська

Пошук...

Ctrl K

Пошук...

Navigation

Authentication and secrets

Семантика облікових даних автентифікації

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Стабільні коди причин перевірки](https://docs.openclaw.ai/uk/auth-credential-semantics#%D1%81%D1%82%D0%B0%D0%B1%D1%96%D0%BB%D1%8C%D0%BD%D1%96-%D0%BA%D0%BE%D0%B4%D0%B8-%D0%BF%D1%80%D0%B8%D1%87%D0%B8%D0%BD-%D0%BF%D0%B5%D1%80%D0%B5%D0%B2%D1%96%D1%80%D0%BA%D0%B8)
- [Токенові облікові дані](https://docs.openclaw.ai/uk/auth-credential-semantics#%D1%82%D0%BE%D0%BA%D0%B5%D0%BD%D0%BE%D0%B2%D1%96-%D0%BE%D0%B1%D0%BB%D1%96%D0%BA%D0%BE%D0%B2%D1%96-%D0%B4%D0%B0%D0%BD%D1%96)
- [Правила придатності](https://docs.openclaw.ai/uk/auth-credential-semantics#%D0%BF%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0-%D0%BF%D1%80%D0%B8%D0%B4%D0%B0%D1%82%D0%BD%D0%BE%D1%81%D1%82%D1%96)
- [Правила розв’язання](https://docs.openclaw.ai/uk/auth-credential-semantics#%D0%BF%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0-%D1%80%D0%BE%D0%B7%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%BD%D1%8F)
- [Портативність копії агента](https://docs.openclaw.ai/uk/auth-credential-semantics#%D0%BF%D0%BE%D1%80%D1%82%D0%B0%D1%82%D0%B8%D0%B2%D0%BD%D1%96%D1%81%D1%82%D1%8C-%D0%BA%D0%BE%D0%BF%D1%96%D1%97-%D0%B0%D0%B3%D0%B5%D0%BD%D1%82%D0%B0)
- [Фільтрування явного порядку автентифікації](https://docs.openclaw.ai/uk/auth-credential-semantics#%D1%84%D1%96%D0%BB%D1%8C%D1%82%D1%80%D1%83%D0%B2%D0%B0%D0%BD%D0%BD%D1%8F-%D1%8F%D0%B2%D0%BD%D0%BE%D0%B3%D0%BE-%D0%BF%D0%BE%D1%80%D1%8F%D0%B4%D0%BA%D1%83-%D0%B0%D0%B2%D1%82%D0%B5%D0%BD%D1%82%D0%B8%D1%84%D1%96%D0%BA%D0%B0%D1%86%D1%96%D1%97)
- [Розв’язання цілі перевірки](https://docs.openclaw.ai/uk/auth-credential-semantics#%D1%80%D0%BE%D0%B7%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%BD%D1%8F-%D1%86%D1%96%D0%BB%D1%96-%D0%BF%D0%B5%D1%80%D0%B5%D0%B2%D1%96%D1%80%D0%BA%D0%B8)
- [Виявлення облікових даних зовнішнього CLI](https://docs.openclaw.ai/uk/auth-credential-semantics#%D0%B2%D0%B8%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BE%D0%B1%D0%BB%D1%96%D0%BA%D0%BE%D0%B2%D0%B8%D1%85-%D0%B4%D0%B0%D0%BD%D0%B8%D1%85-%D0%B7%D0%BE%D0%B2%D0%BD%D1%96%D1%88%D0%BD%D1%8C%D0%BE%D0%B3%D0%BE-cli)
- [Захист політики OAuth SecretRef](https://docs.openclaw.ai/uk/auth-credential-semantics#%D0%B7%D0%B0%D1%85%D0%B8%D1%81%D1%82-%D0%BF%D0%BE%D0%BB%D1%96%D1%82%D0%B8%D0%BA%D0%B8-oauth-secretref)
- [Повідомлення, сумісні зі спадщиною](https://docs.openclaw.ai/uk/auth-credential-semantics#%D0%BF%D0%BE%D0%B2%D1%96%D0%B4%D0%BE%D0%BC%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D1%81%D1%83%D0%BC%D1%96%D1%81%D0%BD%D1%96-%D0%B7%D1%96-%D1%81%D0%BF%D0%B0%D0%B4%D1%89%D0%B8%D0%BD%D0%BE%D1%8E)
- [Пов’язане](https://docs.openclaw.ai/uk/auth-credential-semantics#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Цей документ визначає канонічну семантику придатності й розв’язання облікових даних, що використовується в:

- `resolveAuthProfileOrder`
- `resolveApiKeyForProfile`
- `models status --probe`
- `doctor-auth`

Мета — узгодити поведінку під час вибору та під час виконання.

## [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D1%81%D1%82%D0%B0%D0%B1%D1%96%D0%BB%D1%8C%D0%BD%D1%96-%D0%BA%D0%BE%D0%B4%D0%B8-%D0%BF%D1%80%D0%B8%D1%87%D0%B8%D0%BD-%D0%BF%D0%B5%D1%80%D0%B5%D0%B2%D1%96%D1%80%D0%BA%D0%B8)  Стабільні коди причин перевірки

- `ok`
- `excluded_by_auth_order`
- `missing_credential`
- `invalid_expires`
- `expired`
- `unresolved_ref`
- `no_model`

## [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D1%82%D0%BE%D0%BA%D0%B5%D0%BD%D0%BE%D0%B2%D1%96-%D0%BE%D0%B1%D0%BB%D1%96%D0%BA%D0%BE%D0%B2%D1%96-%D0%B4%D0%B0%D0%BD%D1%96)  Токенові облікові дані

Токенові облікові дані (`type: "token"`) підтримують вбудований `token` та/або `tokenRef`.

### [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D0%BF%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0-%D0%BF%D1%80%D0%B8%D0%B4%D0%B0%D1%82%D0%BD%D0%BE%D1%81%D1%82%D1%96)  Правила придатності

1. Профіль токена непридатний, коли відсутні і `token`, і `tokenRef`.
2. `expires` є необов’язковим.
3. Якщо `expires` присутній, це має бути скінченне число, більше за `0`.
4. Якщо `expires` недійсний (`NaN`, `0`, від’ємне значення, нескінченне значення або неправильний тип), профіль непридатний із `invalid_expires`.
5. Якщо `expires` у минулому, профіль непридатний із `expired`.
6. `tokenRef` не обходить перевірку `expires`.

### [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D0%BF%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%B0-%D1%80%D0%BE%D0%B7%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%BD%D1%8F)  Правила розв’язання

1. Семантика розв’язувача збігається із семантикою придатності для `expires`.
2. Для придатних профілів матеріал токена може бути розв’язаний із вбудованого значення або `tokenRef`.
3. Нерозв’язні посилання створюють `unresolved_ref` у виводі `models status --probe`.

## [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D0%BF%D0%BE%D1%80%D1%82%D0%B0%D1%82%D0%B8%D0%B2%D0%BD%D1%96%D1%81%D1%82%D1%8C-%D0%BA%D0%BE%D0%BF%D1%96%D1%97-%D0%B0%D0%B3%D0%B5%D0%BD%D1%82%D0%B0)  Портативність копії агента

Успадкування автентифікації агента працює з наскрізним читанням. Коли агент не має локального профілю, він може під час виконання розв’язувати профілі зі сховища типового/основного агента без копіювання секретного матеріалу у власний `auth-profiles.json`.Явні потоки копіювання, як-от `openclaw agents add`, використовують таку політику портативності:

- Профілі `api_key` портативні, якщо не вказано `copyToAgents: false`.
- Профілі `token` портативні, якщо не вказано `copyToAgents: false`.
- Профілі `oauth` за замовчуванням не портативні, оскільки токени оновлення можуть бути одноразовими або чутливими до ротації.
- OAuth-потоки, що належать провайдеру, можуть увімкнути це через `copyToAgents: true` лише коли відомо, що копіювання матеріалу оновлення між агентами є безпечним.

Непортативні профілі лишаються доступними через успадкування з наскрізним читанням, якщо цільовий агент не ввійде окремо та не створить власний локальний профіль.

## [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D1%84%D1%96%D0%BB%D1%8C%D1%82%D1%80%D1%83%D0%B2%D0%B0%D0%BD%D0%BD%D1%8F-%D1%8F%D0%B2%D0%BD%D0%BE%D0%B3%D0%BE-%D0%BF%D0%BE%D1%80%D1%8F%D0%B4%D0%BA%D1%83-%D0%B0%D0%B2%D1%82%D0%B5%D0%BD%D1%82%D0%B8%D1%84%D1%96%D0%BA%D0%B0%D1%86%D1%96%D1%97)  Фільтрування явного порядку автентифікації

- Коли для провайдера задано `auth.order.<provider>` або перевизначення порядку сховища автентифікації, `models status --probe` перевіряє лише ідентифікатори профілів, що лишаються в розв’язаному порядку автентифікації для цього провайдера.
- Збережений профіль для цього провайдера, пропущений у явному порядку, не буде непомітно випробуваний пізніше. Вивід перевірки повідомляє про нього з `reasonCode: excluded_by_auth_order` і деталлю `Excluded by auth.order for this provider.`

## [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D1%80%D0%BE%D0%B7%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%BD%D1%8F-%D1%86%D1%96%D0%BB%D1%96-%D0%BF%D0%B5%D1%80%D0%B5%D0%B2%D1%96%D1%80%D0%BA%D0%B8)  Розв’язання цілі перевірки

- Цілі перевірки можуть походити з профілів автентифікації, облікових даних середовища або `models.json`.
- Якщо провайдер має облікові дані, але OpenClaw не може розв’язати для нього кандидата моделі, придатного для перевірки, `models status --probe` повідомляє `status: no_model` із `reasonCode: no_model`.

## [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D0%B2%D0%B8%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BE%D0%B1%D0%BB%D1%96%D0%BA%D0%BE%D0%B2%D0%B8%D1%85-%D0%B4%D0%B0%D0%BD%D0%B8%D1%85-%D0%B7%D0%BE%D0%B2%D0%BD%D1%96%D1%88%D0%BD%D1%8C%D0%BE%D0%B3%D0%BE-cli)  Виявлення облікових даних зовнішнього CLI

- Облікові дані лише для виконання, що належать зовнішнім CLI, виявляються тільки коли провайдер, середовище виконання або профіль автентифікації перебуває в області дії поточної операції, або коли збережений локальний профіль для цього зовнішнього джерела вже існує.
- Викликачі сховища автентифікації мають вибирати явний режим виявлення зовнішнього CLI: `none` для лише збереженої/Plugin автентифікації, `existing` для оновлення вже збережених профілів зовнішнього CLI або `scoped` для конкретного набору провайдерів/профілів.
- Шляхи лише для читання/статусу передають `allowKeychainPrompt: false`; вони використовують лише файлові облікові дані зовнішнього CLI і не читають та не перевикористовують результати macOS Keychain.

## [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D0%B7%D0%B0%D1%85%D0%B8%D1%81%D1%82-%D0%BF%D0%BE%D0%BB%D1%96%D1%82%D0%B8%D0%BA%D0%B8-oauth-secretref)  Захист політики OAuth SecretRef

- Ввід SecretRef призначений лише для статичних облікових даних.
- Якщо облікові дані профілю мають `type: "oauth"`, об’єкти SecretRef не підтримуються для матеріалу облікових даних цього профілю.
- Якщо `auth.profiles.<id>.mode` має значення `"oauth"`, ввід `keyRef`/`tokenRef` на основі SecretRef для цього профілю відхиляється.
- Порушення є жорсткими помилками в шляхах розв’язання автентифікації під час запуску/перезавантаження.

## [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D0%BF%D0%BE%D0%B2%D1%96%D0%B4%D0%BE%D0%BC%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D1%81%D1%83%D0%BC%D1%96%D1%81%D0%BD%D1%96-%D0%B7%D1%96-%D1%81%D0%BF%D0%B0%D0%B4%D1%89%D0%B8%D0%BD%D0%BE%D1%8E)  Повідомлення, сумісні зі спадщиною

Для сумісності зі скриптами перший рядок помилок перевірки лишається незмінним:`Auth profile credentials are missing or expired.`Зручні для людини деталі та стабільні коди причин можуть додаватися в наступних рядках.

## [​](https://docs.openclaw.ai/uk/auth-credential-semantics\#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)  Пов’язане

- [Керування секретами](https://docs.openclaw.ai/uk/gateway/secrets)
- [Сховище автентифікації](https://docs.openclaw.ai/uk/concepts/oauth)

[Автентифікація](https://docs.openclaw.ai/uk/gateway/authentication) [Secrets management](https://docs.openclaw.ai/uk/gateway/secrets)

Ctrl+I