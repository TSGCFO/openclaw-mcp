---
source_url: https://docs.openclaw.ai/uk/automation/standing-orders
title: "\u041f\u043e\u0441\u0442\u0456\u0439\u043d\u0456 \u0432\u043a\u0430\u0437\u0456\u0432\u043a\u0438 - OpenClaw"
---

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/automation/standing-orders#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/uk)

![UA](https://d3gk2c5xim1je2.cloudfront.net/flags/UA.svg)

Українська

Пошук...

Ctrl K

Пошук...

Navigation

Automation and tasks

Постійні вказівки

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Навіщо потрібні постійні вказівки](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BD%D0%B0%D0%B2%D1%96%D1%89%D0%BE-%D0%BF%D0%BE%D1%82%D1%80%D1%96%D0%B1%D0%BD%D1%96-%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D1%96-%D0%B2%D0%BA%D0%B0%D0%B7%D1%96%D0%B2%D0%BA%D0%B8)
- [Як це працює](https://docs.openclaw.ai/uk/automation/standing-orders#%D1%8F%D0%BA-%D1%86%D0%B5-%D0%BF%D1%80%D0%B0%D1%86%D1%8E%D1%94)
- [Анатомія постійної вказівки](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%B0%D0%BD%D0%B0%D1%82%D0%BE%D0%BC%D1%96%D1%8F-%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D0%BE%D1%97-%D0%B2%D0%BA%D0%B0%D0%B7%D1%96%D0%B2%D0%BA%D0%B8)
- [Постійні вказівки та Cron jobs](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D1%96-%D0%B2%D0%BA%D0%B0%D0%B7%D1%96%D0%B2%D0%BA%D0%B8-%D1%82%D0%B0-cron-jobs)
- [Приклади](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4%D0%B8)
- [Приклад 1: контент і соціальні мережі (щотижневий цикл)](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4-1-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%BD%D1%82-%D1%96-%D1%81%D0%BE%D1%86%D1%96%D0%B0%D0%BB%D1%8C%D0%BD%D1%96-%D0%BC%D0%B5%D1%80%D0%B5%D0%B6%D1%96-%D1%89%D0%BE%D1%82%D0%B8%D0%B6%D0%BD%D0%B5%D0%B2%D0%B8%D0%B9-%D1%86%D0%B8%D0%BA%D0%BB)
- [Приклад 2: фінансові операції (запуск за подією)](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4-2-%D1%84%D1%96%D0%BD%D0%B0%D0%BD%D1%81%D0%BE%D0%B2%D1%96-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%86%D1%96%D1%97-%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA-%D0%B7%D0%B0-%D0%BF%D0%BE%D0%B4%D1%96%D1%94%D1%8E)
- [Приклад 3: моніторинг і сповіщення (безперервно)](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4-3-%D0%BC%D0%BE%D0%BD%D1%96%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3-%D1%96-%D1%81%D0%BF%D0%BE%D0%B2%D1%96%D1%89%D0%B5%D0%BD%D0%BD%D1%8F-%D0%B1%D0%B5%D0%B7%D0%BF%D0%B5%D1%80%D0%B5%D1%80%D0%B2%D0%BD%D0%BE)
- [Шаблон виконати-перевірити-звітувати](https://docs.openclaw.ai/uk/automation/standing-orders#%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD-%D0%B2%D0%B8%D0%BA%D0%BE%D0%BD%D0%B0%D1%82%D0%B8-%D0%BF%D0%B5%D1%80%D0%B5%D0%B2%D1%96%D1%80%D0%B8%D1%82%D0%B8-%D0%B7%D0%B2%D1%96%D1%82%D1%83%D0%B2%D0%B0%D1%82%D0%B8)
- [Архітектура з кількома програмами](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%B0%D1%80%D1%85%D1%96%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0-%D0%B7-%D0%BA%D1%96%D0%BB%D1%8C%D0%BA%D0%BE%D0%BC%D0%B0-%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%B0%D0%BC%D0%B8)
- [Найкращі практики](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BD%D0%B0%D0%B9%D0%BA%D1%80%D0%B0%D1%89%D1%96-%D0%BF%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D0%BA%D0%B8)
- [Робіть](https://docs.openclaw.ai/uk/automation/standing-orders#%D1%80%D0%BE%D0%B1%D1%96%D1%82%D1%8C)
- [Уникайте](https://docs.openclaw.ai/uk/automation/standing-orders#%D1%83%D0%BD%D0%B8%D0%BA%D0%B0%D0%B9%D1%82%D0%B5)
- [Пов’язане](https://docs.openclaw.ai/uk/automation/standing-orders#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Постійні вказівки надають вашому агенту **постійні операційні повноваження** для визначених програм. Замість того щоб щоразу давати окремі інструкції для завдань, ви визначаєте програми з чіткою сферою дії, тригерами та правилами ескалації — і агент виконує їх автономно в межах цих кордонів.У цьому й полягає різниця між тим, щоб щоп’ятниці казати вашому помічнику «надішли щотижневий звіт», і наданням постійних повноважень: «Щотижневий звіт — у твоїй відповідальності. Готуй його щоп’ятниці, надсилай і ескалюй лише якщо щось виглядає не так».

## [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%BD%D0%B0%D0%B2%D1%96%D1%89%D0%BE-%D0%BF%D0%BE%D1%82%D1%80%D1%96%D0%B1%D0%BD%D1%96-%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D1%96-%D0%B2%D0%BA%D0%B0%D0%B7%D1%96%D0%B2%D0%BA%D0%B8)  Навіщо потрібні постійні вказівки

**Без постійних вказівок:**

- Ви маєте надсилати агенту запит для кожного завдання
- Агент простоює між запитами
- Рутинна робота забувається або відкладається
- Ви стаєте вузьким місцем

**Із постійними вказівками:**

- Агент виконує роботу автономно в межах визначених кордонів
- Рутинна робота відбувається за розкладом без додаткових запитів
- Ви долучаєтесь лише для винятків і погоджень
- Агент продуктивно використовує час простою

## [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D1%8F%D0%BA-%D1%86%D0%B5-%D0%BF%D1%80%D0%B0%D1%86%D1%8E%D1%94)  Як це працює

Постійні вказівки визначаються у файлах вашого [робочого простору агента](https://docs.openclaw.ai/uk/concepts/agent-workspace). Рекомендований підхід — включати їх безпосередньо в `AGENTS.md` (який автоматично додається в кожну сесію), щоб агент завжди мав їх у контексті. Для більших конфігурацій ви також можете розмістити їх в окремому файлі, наприклад `standing-orders.md`, і посилатися на нього з `AGENTS.md`.Кожна програма визначає:

1. **Сферу дії** — що агент уповноважений робити
2. **Тригери** — коли виконувати (за розкладом, подією або умовою)
3. **Точки погодження** — що потребує людського схвалення перед виконанням
4. **Правила ескалації** — коли слід зупинитися й попросити допомоги

Агент завантажує ці інструкції в кожній сесії через bootstrap-файли робочого простору (див. [Робочий простір агента](https://docs.openclaw.ai/uk/concepts/agent-workspace) для повного списку файлів, що додаються автоматично) і виконує їх у поєднанні з [Cron jobs](https://docs.openclaw.ai/uk/automation/cron-jobs) для примусового запуску за часом.

Розміщуйте постійні вказівки в `AGENTS.md`, щоб гарантувати їх завантаження в кожній сесії. Bootstrap робочого простору автоматично додає `AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`, `BOOTSTRAP.md` і `MEMORY.md` — але не довільні файли в підкаталогах.

## [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%B0%D0%BD%D0%B0%D1%82%D0%BE%D0%BC%D1%96%D1%8F-%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D0%BE%D1%97-%D0%B2%D0%BA%D0%B0%D0%B7%D1%96%D0%B2%D0%BA%D0%B8)  Анатомія постійної вказівки

```
## Program: Weekly Status Report

**Authority:** Compile data, generate report, deliver to stakeholders
**Trigger:** Every Friday at 4 PM (enforced via cron job)
**Approval gate:** None for standard reports. Flag anomalies for human review.
**Escalation:** If data source is unavailable or metrics look unusual (>2σ from norm)

### Execution steps

1. Pull metrics from configured sources
2. Compare to prior week and targets
3. Generate report in Reports/weekly/YYYY-MM-DD.md
4. Deliver summary via configured channel
5. Log completion to Agent/Logs/

### What NOT to do

- Do not send reports to external parties
- Do not modify source data
- Do not skip delivery if metrics look bad — report accurately
```

## [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D1%96-%D0%B2%D0%BA%D0%B0%D0%B7%D1%96%D0%B2%D0%BA%D0%B8-%D1%82%D0%B0-cron-jobs)  Постійні вказівки та Cron jobs

Постійні вказівки визначають, **що** агент уповноважений робити. [Cron jobs](https://docs.openclaw.ai/uk/automation/cron-jobs) визначають, **коли** це відбувається. Вони працюють разом:

```
Standing Order: "You own the daily inbox triage"
    ↓
Cron Job (8 AM daily): "Execute inbox triage per standing orders"
    ↓
Agent: Reads standing orders → executes steps → reports results
```

Запит для Cron job має посилатися на постійну вказівку, а не дублювати її:

```
openclaw cron add \
  --name daily-inbox-triage \
  --cron "0 8 * * 1-5" \
  --tz America/New_York \
  --timeout-seconds 300 \
  --announce \
  --channel bluebubbles \
  --to "+1XXXXXXXXXX" \
  --message "Execute daily inbox triage per standing orders. Check mail for new alerts. Parse, categorize, and persist each item. Report summary to owner. Escalate unknowns."
```

## [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4%D0%B8)  Приклади

### [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4-1-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%BD%D1%82-%D1%96-%D1%81%D0%BE%D1%86%D1%96%D0%B0%D0%BB%D1%8C%D0%BD%D1%96-%D0%BC%D0%B5%D1%80%D0%B5%D0%B6%D1%96-%D1%89%D0%BE%D1%82%D0%B8%D0%B6%D0%BD%D0%B5%D0%B2%D0%B8%D0%B9-%D1%86%D0%B8%D0%BA%D0%BB)  Приклад 1: контент і соціальні мережі (щотижневий цикл)

```
## Program: Content & Social Media

**Authority:** Draft content, schedule posts, compile engagement reports
**Approval gate:** All posts require owner review for first 30 days, then standing approval
**Trigger:** Weekly cycle (Monday review → mid-week drafts → Friday brief)

### Weekly cycle

- **Monday:** Review platform metrics and audience engagement
- **Tuesday–Thursday:** Draft social posts, create blog content
- **Friday:** Compile weekly marketing brief → deliver to owner

### Content rules

- Voice must match the brand (see SOUL.md or brand voice guide)
- Never identify as AI in public-facing content
- Include metrics when available
- Focus on value to audience, not self-promotion
```

### [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4-2-%D1%84%D1%96%D0%BD%D0%B0%D0%BD%D1%81%D0%BE%D0%B2%D1%96-%D0%BE%D0%BF%D0%B5%D1%80%D0%B0%D1%86%D1%96%D1%97-%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA-%D0%B7%D0%B0-%D0%BF%D0%BE%D0%B4%D1%96%D1%94%D1%8E)  Приклад 2: фінансові операції (запуск за подією)

```
## Program: Financial Processing

**Authority:** Process transaction data, generate reports, send summaries
**Approval gate:** None for analysis. Recommendations require owner approval.
**Trigger:** New data file detected OR scheduled monthly cycle

### When new data arrives

1. Detect new file in designated input directory
2. Parse and categorize all transactions
3. Compare against budget targets
4. Flag: unusual items, threshold breaches, new recurring charges
5. Generate report in designated output directory
6. Deliver summary to owner via configured channel

### Escalation rules

- Single item > $500: immediate alert
- Category > budget by 20%: flag in report
- Unrecognizable transaction: ask owner for categorization
- Failed processing after 2 retries: report failure, do not guess
```

### [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%BF%D1%80%D0%B8%D0%BA%D0%BB%D0%B0%D0%B4-3-%D0%BC%D0%BE%D0%BD%D1%96%D1%82%D0%BE%D1%80%D0%B8%D0%BD%D0%B3-%D1%96-%D1%81%D0%BF%D0%BE%D0%B2%D1%96%D1%89%D0%B5%D0%BD%D0%BD%D1%8F-%D0%B1%D0%B5%D0%B7%D0%BF%D0%B5%D1%80%D0%B5%D1%80%D0%B2%D0%BD%D0%BE)  Приклад 3: моніторинг і сповіщення (безперервно)

```
## Program: System Monitoring

**Authority:** Check system health, restart services, send alerts
**Approval gate:** Restart services automatically. Escalate if restart fails twice.
**Trigger:** Every heartbeat cycle

### Checks

- Service health endpoints responding
- Disk space above threshold
- Pending tasks not stale (>24 hours)
- Delivery channels operational

### Response matrix

| Condition        | Action                   | Escalate?                |
| ---------------- | ------------------------ | ------------------------ |
| Service down     | Restart automatically    | Only if restart fails 2x |
| Disk space < 10% | Alert owner              | Yes                      |
| Stale task > 24h | Remind owner             | No                       |
| Channel offline  | Log and retry next cycle | If offline > 2 hours     |
```

## [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD-%D0%B2%D0%B8%D0%BA%D0%BE%D0%BD%D0%B0%D1%82%D0%B8-%D0%BF%D0%B5%D1%80%D0%B5%D0%B2%D1%96%D1%80%D0%B8%D1%82%D0%B8-%D0%B7%D0%B2%D1%96%D1%82%D1%83%D0%B2%D0%B0%D1%82%D0%B8)  Шаблон виконати-перевірити-звітувати

Постійні вказівки працюють найкраще в поєднанні зі строгою дисципліною виконання. Кожне завдання в постійній вказівці має дотримуватися такого циклу:

1. **Виконати** — Зробити фактичну роботу (а не просто підтвердити інструкцію)
2. **Перевірити** — Підтвердити, що результат правильний (файл існує, повідомлення доставлено, дані розібрано)
3. **Звітувати** — Повідомити власнику, що було зроблено і що було перевірено

```
### Execution rules

- Every task follows Execute-Verify-Report. No exceptions.
- "I'll do that" is not execution. Do it, then report.
- "Done" without verification is not acceptable. Prove it.
- If execution fails: retry once with adjusted approach.
- If still fails: report failure with diagnosis. Never silently fail.
- Never retry indefinitely — 3 attempts max, then escalate.
```

Цей шаблон запобігає найпоширенішому збою агентів: підтвердженню завдання без його завершення.

## [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%B0%D1%80%D1%85%D1%96%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0-%D0%B7-%D0%BA%D1%96%D0%BB%D1%8C%D0%BA%D0%BE%D0%BC%D0%B0-%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%B0%D0%BC%D0%B8)  Архітектура з кількома програмами

Для агентів, які керують кількома напрямами, організовуйте постійні вказівки як окремі програми з чіткими межами:

```
## Program 1: [Domain A] (Weekly)

...

## Program 2: [Domain B] (Monthly + On-Demand)

...

## Program 3: [Domain C] (As-Needed)

...

## Escalation Rules (All Programs)

- [Common escalation criteria]
- [Approval gates that apply across programs]
```

Кожна програма має мати:

- власну **частоту тригерів** (щотижня, щомісяця, за подією, безперервно)
- власні **точки погодження** (деякі програми потребують більшого нагляду, ніж інші)
- чіткі **межі** (агент має знати, де закінчується одна програма і починається інша)

## [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%BD%D0%B0%D0%B9%D0%BA%D1%80%D0%B0%D1%89%D1%96-%D0%BF%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D0%BA%D0%B8)  Найкращі практики

### [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D1%80%D0%BE%D0%B1%D1%96%D1%82%D1%8C)  Робіть

- Починайте з вузьких повноважень і розширюйте їх у міру зростання довіри
- Визначайте явні точки погодження для дій із високим ризиком
- Додавайте розділи «Що НЕ робити» — межі так само важливі, як і дозволи
- Поєднуйте з Cron jobs для надійного виконання за розкладом
- Щотижня переглядайте журнали агента, щоб переконатися, що постійні вказівки виконуються
- Оновлюйте постійні вказівки відповідно до зміни ваших потреб — це живі документи

### [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D1%83%D0%BD%D0%B8%D0%BA%D0%B0%D0%B9%D1%82%D0%B5)  Уникайте

- Надавати широкі повноваження в перший день («роби все, що вважаєш за потрібне»)
- Пропускати правила ескалації — кожна програма має містити умову «коли зупинитися і запитати»
- Припускати, що агент запам’ятає усні інструкції — заносьте все у файл
- Змішувати різні напрями в одній програмі — окремі програми для окремих сфер
- Забувати про примусове виконання через Cron jobs — постійні вказівки без тригерів стають лише рекомендаціями

## [​](https://docs.openclaw.ai/uk/automation/standing-orders\#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)  Пов’язане

- [Автоматизація і завдання](https://docs.openclaw.ai/uk/automation): усі механізми автоматизації з одного погляду.
- [Cron jobs](https://docs.openclaw.ai/uk/automation/cron-jobs): забезпечення виконання постійних вказівок за розкладом.
- [Hooks](https://docs.openclaw.ai/uk/automation/hooks): скрипти, що запускаються за подіями життєвого циклу агента.
- [Webhooks](https://docs.openclaw.ai/uk/automation/cron-jobs#webhooks): вхідні HTTP-тригери подій.
- [Робочий простір агента](https://docs.openclaw.ai/uk/concepts/agent-workspace): де зберігаються постійні вказівки, включно з повним списком bootstrap-файлів, що додаються автоматично (`AGENTS.md`, `SOUL.md` тощо).

[Потік завдань](https://docs.openclaw.ai/uk/automation/taskflow) [Хуки](https://docs.openclaw.ai/uk/automation/hooks)

Ctrl+I