---
source_url: https://docs.openclaw.ai/uk/automation/taskflow
title: "\u041f\u043e\u0442\u0456\u043a \u0437\u0430\u0432\u0434\u0430\u043d\u044c - OpenClaw"
---

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/automation/taskflow#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/uk)

![UA](https://d3gk2c5xim1je2.cloudfront.net/flags/UA.svg)

Українська

Пошук...

Ctrl K

Пошук...

Navigation

Automation and tasks

Потік завдань

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Коли використовувати потік завдань](https://docs.openclaw.ai/uk/automation/taskflow#%D0%BA%D0%BE%D0%BB%D0%B8-%D0%B2%D0%B8%D0%BA%D0%BE%D1%80%D0%B8%D1%81%D1%82%D0%BE%D0%B2%D1%83%D0%B2%D0%B0%D1%82%D0%B8-%D0%BF%D0%BE%D1%82%D1%96%D0%BA-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D1%8C)
- [Надійний шаблон запланованого робочого процесу](https://docs.openclaw.ai/uk/automation/taskflow#%D0%BD%D0%B0%D0%B4%D1%96%D0%B9%D0%BD%D0%B8%D0%B9-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD-%D0%B7%D0%B0%D0%BF%D0%BB%D0%B0%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D0%BE%D0%B3%D0%BE-%D1%80%D0%BE%D0%B1%D0%BE%D1%87%D0%BE%D0%B3%D0%BE-%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D1%83)
- [Режими синхронізації](https://docs.openclaw.ai/uk/automation/taskflow#%D1%80%D0%B5%D0%B6%D0%B8%D0%BC%D0%B8-%D1%81%D0%B8%D0%BD%D1%85%D1%80%D0%BE%D0%BD%D1%96%D0%B7%D0%B0%D1%86%D1%96%D1%97)
- [Керований режим](https://docs.openclaw.ai/uk/automation/taskflow#%D0%BA%D0%B5%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B9-%D1%80%D0%B5%D0%B6%D0%B8%D0%BC)
- [Дзеркальний режим](https://docs.openclaw.ai/uk/automation/taskflow#%D0%B4%D0%B7%D0%B5%D1%80%D0%BA%D0%B0%D0%BB%D1%8C%D0%BD%D0%B8%D0%B9-%D1%80%D0%B5%D0%B6%D0%B8%D0%BC)
- [Стійкий стан і відстеження ревізій](https://docs.openclaw.ai/uk/automation/taskflow#%D1%81%D1%82%D1%96%D0%B9%D0%BA%D0%B8%D0%B9-%D1%81%D1%82%D0%B0%D0%BD-%D1%96-%D0%B2%D1%96%D0%B4%D1%81%D1%82%D0%B5%D0%B6%D0%B5%D0%BD%D0%BD%D1%8F-%D1%80%D0%B5%D0%B2%D1%96%D0%B7%D1%96%D0%B9)
- [Поведінка скасування](https://docs.openclaw.ai/uk/automation/taskflow#%D0%BF%D0%BE%D0%B2%D0%B5%D0%B4%D1%96%D0%BD%D0%BA%D0%B0-%D1%81%D0%BA%D0%B0%D1%81%D1%83%D0%B2%D0%B0%D0%BD%D0%BD%D1%8F)
- [Команди CLI](https://docs.openclaw.ai/uk/automation/taskflow#%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D0%B8-cli)
- [Як потоки пов’язані із завданнями](https://docs.openclaw.ai/uk/automation/taskflow#%D1%8F%D0%BA-%D0%BF%D0%BE%D1%82%D0%BE%D0%BA%D0%B8-%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D1%96-%D1%96%D0%B7-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F%D0%BC%D0%B8)
- [Пов’язане](https://docs.openclaw.ai/uk/automation/taskflow#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Потік завдань — це підкладка оркестрації потоків, що розташована над [фоновими завданнями](https://docs.openclaw.ai/uk/automation/tasks). Він керує стійкими багатокроковими потоками з власним станом, відстеженням ревізій і семантикою синхронізації, тоді як окремі завдання залишаються одиницею відокремленої роботи.

## [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D0%BA%D0%BE%D0%BB%D0%B8-%D0%B2%D0%B8%D0%BA%D0%BE%D1%80%D0%B8%D1%81%D1%82%D0%BE%D0%B2%D1%83%D0%B2%D0%B0%D1%82%D0%B8-%D0%BF%D0%BE%D1%82%D1%96%D0%BA-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D1%8C)  Коли використовувати потік завдань

Використовуйте потік завдань, коли робота охоплює кілька послідовних або розгалужених кроків і вам потрібне стійке відстеження прогресу після перезапусків Gateway. Для окремих фонових операцій достатньо звичайного [завдання](https://docs.openclaw.ai/uk/automation/tasks).

| Сценарій | Використання |
| --- | --- |
| Окреме фонове завдання | Звичайне завдання |
| Багатокроковий конвеєр (A, потім B, потім C) | Потік завдань (керований) |
| Спостереження за зовнішньо створеними завданнями | Потік завдань (дзеркальний) |
| Одноразове нагадування | Завдання Cron |

## [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D0%BD%D0%B0%D0%B4%D1%96%D0%B9%D0%BD%D0%B8%D0%B9-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD-%D0%B7%D0%B0%D0%BF%D0%BB%D0%B0%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D0%BE%D0%B3%D0%BE-%D1%80%D0%BE%D0%B1%D0%BE%D1%87%D0%BE%D0%B3%D0%BE-%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D1%81%D1%83)  Надійний шаблон запланованого робочого процесу

Для повторюваних робочих процесів, таких як брифінги з ринкової аналітики, розглядайте розклад, оркестрацію та перевірки надійності як окремі рівні:

1. Використовуйте [Scheduled Tasks](https://docs.openclaw.ai/uk/automation/cron-jobs) для часу запуску.
2. Використовуйте постійну сесію Cron, коли робочий процес має спиратися на попередній контекст.
3. Використовуйте [Lobster](https://docs.openclaw.ai/uk/tools/lobster) для детермінованих кроків, етапів схвалення та токенів відновлення.
4. Використовуйте потік завдань, щоб відстежувати багатокроковий запуск через дочірні завдання, очікування, повторні спроби та перезапуски Gateway.

Приклад форми Cron:

```
openclaw cron add \
  --name "Market intelligence brief" \
  --cron "0 7 * * 1-5" \
  --tz "America/New_York" \
  --session session:market-intel \
  --message "Run the market-intel Lobster workflow. Verify source freshness before summarizing." \
  --announce \
  --channel slack \
  --to "channel:C1234567890"
```

Використовуйте `session:<id>` замість `isolated`, коли повторюваному робочому процесу потрібна навмисно збережена історія, підсумки попередніх запусків або сталий контекст. Використовуйте `isolated`, коли кожен запуск має починатися з чистого аркуша, а весь потрібний стан явно визначений у робочому процесі.Усередині робочого процесу розміщуйте перевірки надійності перед кроком підсумовування LLM:

```
name: market-intel-brief
steps:
  - id: preflight
    command: market-intel check --json
  - id: collect
    command: market-intel collect --json
    stdin: $preflight.json
  - id: summarize
    command: market-intel summarize --json
    stdin: $collect.json
  - id: approve
    command: market-intel deliver --preview
    stdin: $summarize.json
    approval: required
  - id: deliver
    command: market-intel deliver --execute
    stdin: $summarize.json
    condition: $approve.approved
```

Рекомендовані перевірки перед запуском:

- Доступність браузера та вибір профілю, наприклад `openclaw` для керованого стану або `user`, коли потрібна сесія Chrome з виконаним входом. Див. [Browser](https://docs.openclaw.ai/uk/tools/browser).
- Облікові дані API та квота для кожного джерела.
- Доступність мережі для потрібних кінцевих точок.
- Увімкнені для агента потрібні інструменти, такі як `lobster`, `browser` і `llm-task`.
- Налаштоване місце призначення для збоїв Cron, щоб помилки перевірки перед запуском були видимими. Див. [Scheduled Tasks](https://docs.openclaw.ai/uk/automation/cron-jobs#delivery-and-output).

Рекомендовані поля походження даних для кожного зібраного елемента:

```
{
  "sourceUrl": "https://example.com/report",
  "retrievedAt": "2026-04-24T12:00:00Z",
  "asOf": "2026-04-24",
  "title": "Example report",
  "content": "..."
}
```

Налаштуйте робочий процес так, щоб він відхиляв або позначав застарілі елементи до етапу підсумовування. Крок LLM має отримувати лише структурований JSON, і його слід просити зберігати `sourceUrl`, `retrievedAt` і `asOf` у своєму вихідному результаті. Використовуйте [LLM Task](https://docs.openclaw.ai/uk/tools/llm-task), коли вам потрібен перевірений за схемою крок моделі всередині робочого процесу.Для багаторазово використовуваних командних або спільнотних робочих процесів пакуйте CLI, файли `.lobster` і будь-які примітки з налаштування як skill або Plugin і публікуйте їх через [ClawHub](https://docs.openclaw.ai/uk/tools/clawhub). Зберігайте специфічні для робочого процесу запобіжники в цьому пакеті, якщо тільки в API Plugin не бракує потрібної загальної можливості.

## [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D1%80%D0%B5%D0%B6%D0%B8%D0%BC%D0%B8-%D1%81%D0%B8%D0%BD%D1%85%D1%80%D0%BE%D0%BD%D1%96%D0%B7%D0%B0%D1%86%D1%96%D1%97)  Режими синхронізації

### [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D0%BA%D0%B5%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B9-%D1%80%D0%B5%D0%B6%D0%B8%D0%BC)  Керований режим

Потік завдань повністю володіє життєвим циклом. Він створює завдання як кроки потоку, доводить їх до завершення та автоматично просуває стан потоку.Приклад: потік щотижневого звіту, який (1) збирає дані, (2) генерує звіт і (3) доставляє його. Потік завдань створює кожен крок як фонове завдання, чекає завершення, а потім переходить до наступного кроку.

```
Flow: weekly-report
  Step 1: gather-data     → task created → succeeded
  Step 2: generate-report → task created → succeeded
  Step 3: deliver         → task created → running
```

### [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D0%B4%D0%B7%D0%B5%D1%80%D0%BA%D0%B0%D0%BB%D1%8C%D0%BD%D0%B8%D0%B9-%D1%80%D0%B5%D0%B6%D0%B8%D0%BC)  Дзеркальний режим

Потік завдань спостерігає за зовнішньо створеними завданнями та підтримує стан потоку синхронізованим, не беручи на себе відповідальність за створення завдань. Це корисно, коли завдання походять із Cron, команд CLI або інших джерел і вам потрібне уніфіковане подання їхнього прогресу як потоку.Приклад: три незалежні завдання Cron, які разом утворюють рутину “ранкових операцій”. Дзеркальний потік відстежує їхній сукупний прогрес, не керуючи тим, коли і як вони виконуються.

## [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D1%81%D1%82%D1%96%D0%B9%D0%BA%D0%B8%D0%B9-%D1%81%D1%82%D0%B0%D0%BD-%D1%96-%D0%B2%D1%96%D0%B4%D1%81%D1%82%D0%B5%D0%B6%D0%B5%D0%BD%D0%BD%D1%8F-%D1%80%D0%B5%D0%B2%D1%96%D0%B7%D1%96%D0%B9)  Стійкий стан і відстеження ревізій

Кожен потік зберігає власний стан і відстежує ревізії, тому прогрес зберігається після перезапусків Gateway. Відстеження ревізій дає змогу виявляти конфлікти, коли кілька джерел одночасно намагаються просунути той самий потік.
Реєстр потоків використовує SQLite з обмеженим обслуговуванням журналу випереджального запису, включно з періодичними контрольними точками та контрольними точками під час завершення роботи, щоб довготривалі Gateway не накопичували необмежено великі побічні файли `registry.sqlite-wal`.

## [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D0%BF%D0%BE%D0%B2%D0%B5%D0%B4%D1%96%D0%BD%D0%BA%D0%B0-%D1%81%D0%BA%D0%B0%D1%81%D1%83%D0%B2%D0%B0%D0%BD%D0%BD%D1%8F)  Поведінка скасування

`openclaw tasks flow cancel` встановлює для потоку стійкий намір скасування. Активні завдання в межах потоку скасовуються, і нові кроки більше не запускаються. Намір скасування зберігається після перезапусків, тому скасований потік залишається скасованим, навіть якщо Gateway перезапуститься до того, як усі дочірні завдання завершаться.

## [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D0%B8-cli)  Команди CLI

```
# List active and recent flows
openclaw tasks flow list

# Show details for a specific flow
openclaw tasks flow show <lookup>

# Cancel a running flow and its active tasks
openclaw tasks flow cancel <lookup>
```

| Команда | Опис |
| --- | --- |
| `openclaw tasks flow list` | Показує відстежувані потоки зі статусом і режимом синхронізації |
| `openclaw tasks flow show <id>` | Переглянути один потік за ідентифікатором потоку або ключем пошуку |
| `openclaw tasks flow cancel <id>` | Скасувати запущений потік і його активні завдання |

## [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D1%8F%D0%BA-%D0%BF%D0%BE%D1%82%D0%BE%D0%BA%D0%B8-%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D1%96-%D1%96%D0%B7-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F%D0%BC%D0%B8)  Як потоки пов’язані із завданнями

Потоки координують завдання, а не замінюють їх. Один потік за час свого існування може керувати кількома фоновими завданнями. Використовуйте `openclaw tasks`, щоб переглядати окремі записи завдань, і `openclaw tasks flow`, щоб переглядати потік-оркестратор.

## [​](https://docs.openclaw.ai/uk/automation/taskflow\#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)  Пов’язане

- [Background Tasks](https://docs.openclaw.ai/uk/automation/tasks) — реєстр відокремленої роботи, яку координують потоки
- [CLI: tasks](https://docs.openclaw.ai/uk/cli/tasks) — довідник команд CLI для `openclaw tasks flow`
- [Automation Overview](https://docs.openclaw.ai/uk/automation) — усі механізми автоматизації в одному огляді
- [Cron Jobs](https://docs.openclaw.ai/uk/automation/cron-jobs) — заплановані завдання, які можуть передавати дані в потоки

[Background tasks](https://docs.openclaw.ai/uk/automation/tasks) [Постійні вказівки](https://docs.openclaw.ai/uk/automation/standing-orders)

Ctrl+I