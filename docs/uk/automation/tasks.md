---
source_url: https://docs.openclaw.ai/uk/automation/tasks
title: "\u0424\u043e\u043d\u043e\u0432\u0456 \u0437\u0430\u0432\u0434\u0430\u043d\u043d\u044f - OpenClaw"
---

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/automation/tasks#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/uk)

![UA](https://d3gk2c5xim1je2.cloudfront.net/flags/UA.svg)

Українська

Пошук...

Ctrl K

Пошук...

Navigation

Automation and tasks

Фонові завдання

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Коротко](https://docs.openclaw.ai/uk/automation/tasks#%D0%BA%D0%BE%D1%80%D0%BE%D1%82%D0%BA%D0%BE)
- [Швидкий старт](https://docs.openclaw.ai/uk/automation/tasks#%D1%88%D0%B2%D0%B8%D0%B4%D0%BA%D0%B8%D0%B9-%D1%81%D1%82%D0%B0%D1%80%D1%82)
- [Що створює завдання](https://docs.openclaw.ai/uk/automation/tasks#%D1%89%D0%BE-%D1%81%D1%82%D0%B2%D0%BE%D1%80%D1%8E%D1%94-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F)
- [Життєвий цикл завдання](https://docs.openclaw.ai/uk/automation/tasks#%D0%B6%D0%B8%D1%82%D1%82%D1%94%D0%B2%D0%B8%D0%B9-%D1%86%D0%B8%D0%BA%D0%BB-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F)
- [Доставка та сповіщення](https://docs.openclaw.ai/uk/automation/tasks#%D0%B4%D0%BE%D1%81%D1%82%D0%B0%D0%B2%D0%BA%D0%B0-%D1%82%D0%B0-%D1%81%D0%BF%D0%BE%D0%B2%D1%96%D1%89%D0%B5%D0%BD%D0%BD%D1%8F)
- [Політики сповіщень](https://docs.openclaw.ai/uk/automation/tasks#%D0%BF%D0%BE%D0%BB%D1%96%D1%82%D0%B8%D0%BA%D0%B8-%D1%81%D0%BF%D0%BE%D0%B2%D1%96%D1%89%D0%B5%D0%BD%D1%8C)
- [Довідник CLI](https://docs.openclaw.ai/uk/automation/tasks#%D0%B4%D0%BE%D0%B2%D1%96%D0%B4%D0%BD%D0%B8%D0%BA-cli)
- [Дошка завдань чату (/tasks)](https://docs.openclaw.ai/uk/automation/tasks#%D0%B4%D0%BE%D1%88%D0%BA%D0%B0-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D1%8C-%D1%87%D0%B0%D1%82%D1%83-%2Ftasks)
- [Інтеграція статусу (навантаження завдань)](https://docs.openclaw.ai/uk/automation/tasks#%D1%96%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B0%D1%86%D1%96%D1%8F-%D1%81%D1%82%D0%B0%D1%82%D1%83%D1%81%D1%83-%D0%BD%D0%B0%D0%B2%D0%B0%D0%BD%D1%82%D0%B0%D0%B6%D0%B5%D0%BD%D0%BD%D1%8F-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D1%8C)
- [Зберігання та обслуговування](https://docs.openclaw.ai/uk/automation/tasks#%D0%B7%D0%B1%D0%B5%D1%80%D1%96%D0%B3%D0%B0%D0%BD%D0%BD%D1%8F-%D1%82%D0%B0-%D0%BE%D0%B1%D1%81%D0%BB%D1%83%D0%B3%D0%BE%D0%B2%D1%83%D0%B2%D0%B0%D0%BD%D0%BD%D1%8F)
- [Де зберігаються завдання](https://docs.openclaw.ai/uk/automation/tasks#%D0%B4%D0%B5-%D0%B7%D0%B1%D0%B5%D1%80%D1%96%D0%B3%D0%B0%D1%8E%D1%82%D1%8C%D1%81%D1%8F-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F)
- [Автоматичне обслуговування](https://docs.openclaw.ai/uk/automation/tasks#%D0%B0%D0%B2%D1%82%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D1%87%D0%BD%D0%B5-%D0%BE%D0%B1%D1%81%D0%BB%D1%83%D0%B3%D0%BE%D0%B2%D1%83%D0%B2%D0%B0%D0%BD%D0%BD%D1%8F)
- [Як завдання пов’язані з іншими системами](https://docs.openclaw.ai/uk/automation/tasks#%D1%8F%D0%BA-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F-%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D1%96-%D0%B7-%D1%96%D0%BD%D1%88%D0%B8%D0%BC%D0%B8-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%D0%BC%D0%B8)
- [Пов’язане](https://docs.openclaw.ai/uk/automation/tasks#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Шукаєте планування? Див. [Автоматизація та завдання](https://docs.openclaw.ai/uk/automation), щоб вибрати правильний механізм. Ця сторінка є журналом активності для фонової роботи, а не планувальником.

Фонові завдання відстежують роботу, що виконується **поза основним сеансом розмови**: запуски ACP, створення підagentів, ізольовані виконання cron-завдань і операції, ініційовані з CLI.Завдання **не** замінюють сеанси, cron-завдання чи heartbeats — це **журнал активності**, який записує, яка відокремлена робота відбулася, коли саме та чи завершилася вона успішно.

Не кожен запуск агента створює завдання. Heartbeat-ходи та звичайний інтерактивний чат не створюють. Усі виконання cron, створення ACP, створення підagentів і команди агента з CLI створюють.

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%BA%D0%BE%D1%80%D0%BE%D1%82%D0%BA%D0%BE)  Коротко

- Завдання — це **записи**, а не планувальники — cron і Heartbeat вирішують, _коли_ виконується робота, а завдання відстежують, _що сталося_.
- ACP, підagенти, усі cron-завдання та операції CLI створюють завдання. Heartbeat-ходи — ні.
- Кожне завдання проходить через `queued → running → terminal` (succeeded, failed, timed\_out, cancelled або lost).
- Cron-завдання залишаються активними, доки cron-середовище виконання все ще володіє завданням; якщо
стан середовища виконання в пам’яті зник, обслуговування завдань спершу перевіряє довговічну історію
запусків cron, перш ніж позначити завдання як lost.
- Завершення керується push-механізмом: відокремлена робота може сповістити напряму або пробудити
сеанс/Heartbeat запитувача після завершення, тому цикли опитування статусу
зазвичай мають неправильну форму.
- Ізольовані cron-запуски та завершення підagentів у режимі best-effort очищають відстежувані вкладки браузера/процеси для свого дочірнього сеансу перед фінальним службовим очищенням.
- Доставка ізольованого cron пригнічує застарілі проміжні відповіді батьківського сеансу, доки робота підagentів-нащадків ще завершується, і віддає перевагу фінальному виводу нащадка, якщо він надходить до доставки.
- Сповіщення про завершення доставляються напряму в канал або ставляться в чергу до наступного Heartbeat.
- `openclaw tasks list` показує всі завдання; `openclaw tasks audit` виявляє проблеми.
- Термінальні записи зберігаються 7 днів, а потім автоматично видаляються.

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D1%88%D0%B2%D0%B8%D0%B4%D0%BA%D0%B8%D0%B9-%D1%81%D1%82%D0%B0%D1%80%D1%82)  Швидкий старт

- Список і фільтрація

- Перегляд

- Скасування та сповіщення

- Аудит і обслуговування

- Потік завдань


```
# List all tasks (newest first)
openclaw tasks list

# Filter by runtime or status
openclaw tasks list --runtime acp
openclaw tasks list --status running
```

```
# Show details for a specific task (by ID, run ID, or session key)
openclaw tasks show <lookup>
```

```
# Cancel a running task (kills the child session)
openclaw tasks cancel <lookup>

# Change notification policy for a task
openclaw tasks notify <lookup> state_changes
```

```
# Run a health audit
openclaw tasks audit

# Preview or apply maintenance
openclaw tasks maintenance
openclaw tasks maintenance --apply
```

```
# Inspect TaskFlow state
openclaw tasks flow list
openclaw tasks flow show <lookup>
openclaw tasks flow cancel <lookup>
```

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D1%89%D0%BE-%D1%81%D1%82%D0%B2%D0%BE%D1%80%D1%8E%D1%94-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F)  Що створює завдання

| Джерело | Тип середовища виконання | Коли створюється запис завдання | Типова політика сповіщень |
| --- | --- | --- | --- |
| Фонові запуски ACP | `acp` | Створення дочірнього сеансу ACP | `done_only` |
| Оркестрація підagentів | `subagent` | Створення підagента через `sessions_spawn` | `done_only` |
| Cron-завдання (усі типи) | `cron` | Кожне виконання cron (основний сеанс та ізольоване) | `silent` |
| Операції CLI | `cli` | Команди `openclaw agent`, що виконуються через Gateway | `silent` |
| Медіазавдання агента | `cli` | Запуски `music_generate`/`video_generate` із підтримкою сеансу | `silent` |

Типові сповіщення для cron і медіа

Cron-завдання основного сеансу за замовчуванням використовують політику сповіщень `silent` — вони створюють записи для відстеження, але не генерують сповіщень. Ізольовані cron-завдання також за замовчуванням мають `silent`, але помітніші, бо виконуються у власному сеансі.Запуски `music_generate` і `video_generate` із підтримкою сеансу також використовують політику сповіщень `silent`. Вони все одно створюють записи завдань, але завершення повертається до початкового сеансу агента як внутрішнє пробудження, щоб агент міг сам написати подальше повідомлення та прикріпити готовий медіафайл. Якщо ви вмикаєте `tools.media.asyncCompletion.directSend`, асинхронні завершення `video_generate` можуть спершу спробувати пряму доставку в канал; асинхронні завершення `music_generate` залишаються на шляху пробудження сеансу запитувача.

Запобіжник для одночасного video\_generate

Поки завдання `video_generate` із підтримкою сеансу ще активне, інструмент також працює як запобіжник: повторні виклики `video_generate` у тому самому сеансі повертають статус активного завдання замість запуску другої паралельної генерації. Використовуйте `action: "status"`, коли потрібен явний перегляд прогресу/статусу з боку агента.

Що не створює завдань

- Heartbeat-ходи — основний сеанс; див. [Heartbeat](https://docs.openclaw.ai/uk/gateway/heartbeat)
- Звичайні інтерактивні ходи чату
- Прямі відповіді `/command`

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%B6%D0%B8%D1%82%D1%82%D1%94%D0%B2%D0%B8%D0%B9-%D1%86%D0%B8%D0%BA%D0%BB-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F)  Життєвий цикл завдання

agent starts

completes ok

error

timeout exceeded

operator cancels

session gone > 5 min

session gone > 5 min

queued

running

succeeded

failed

timed\_out

cancelled

lost

| Статус | Що це означає |
| --- | --- |
| `queued` | Створено, очікує запуску агента |
| `running` | Хід агента активно виконується |
| `succeeded` | Успішно завершено |
| `failed` | Завершено з помилкою |
| `timed_out` | Перевищено налаштований час очікування |
| `cancelled` | Зупинено оператором через `openclaw tasks cancel` |
| `lost` | Середовище виконання втратило авторитетний базовий стан після 5-хвилинного пільгового періоду |

Переходи відбуваються автоматично — коли пов’язаний запуск агента завершується, статус завдання оновлюється відповідно.Завершення запуску агента є авторитетним для активних записів завдань. Успішний відокремлений запуск фіналізується як `succeeded`, звичайні помилки запуску — як `failed`, а результати тайм-ауту або переривання — як `timed_out`. Якщо оператор уже скасував завдання або середовище виконання вже записало сильніший термінальний стан, наприклад `failed`, `timed_out` чи `lost`, пізніший сигнал успіху не понижує цей термінальний статус.`lost` враховує середовище виконання:

- ACP-завдання: зникли метадані базового дочірнього сеансу ACP.
- Завдання підagentів: базовий дочірній сеанс зник із цільового сховища агента.
- Cron-завдання: cron-середовище виконання більше не відстежує завдання як активне, а довговічна
історія запусків cron не показує термінального результату для цього запуску. Офлайн-аудит CLI
не вважає власний порожній внутрішньопроцесний стан cron-середовища виконання авторитетним.
- CLI-завдання: ізольовані завдання дочірнього сеансу використовують дочірній сеанс; CLI-завдання
з підтримкою чату натомість використовують живий контекст запуску, тож завислі
рядки сеансів каналу/групи/приватного чату не підтримують їх активними. Запуски
`openclaw agent` із підтримкою Gateway також фіналізуються за результатом свого запуску, тому завершені запуски
не залишаються активними, доки прибиральник позначить їх як `lost`.

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%B4%D0%BE%D1%81%D1%82%D0%B0%D0%B2%D0%BA%D0%B0-%D1%82%D0%B0-%D1%81%D0%BF%D0%BE%D0%B2%D1%96%D1%89%D0%B5%D0%BD%D0%BD%D1%8F)  Доставка та сповіщення

Коли завдання досягає термінального стану, OpenClaw сповіщає вас. Є два шляхи доставки:**Пряма доставка** — якщо завдання має цільовий канал (`requesterOrigin`), повідомлення про завершення надсилається прямо в цей канал (Telegram, Discord, Slack тощо). Для завершень підagentів OpenClaw також зберігає прив’язану маршрутизацію гілки/теми, коли вона доступна, і може заповнити відсутні `to` / обліковий запис із збереженого маршруту сеансу запитувача (`lastChannel` / `lastTo` / `lastAccountId`), перш ніж відмовитися від прямої доставки.**Доставка через чергу сеансу** — якщо пряма доставка не вдається або origin не задано, оновлення ставиться в чергу як системна подія в сеансі запитувача й з’являється під час наступного Heartbeat.

Завершення завдання запускає негайне пробудження Heartbeat, щоб ви швидко побачили результат — вам не потрібно чекати наступного запланованого Heartbeat-тику.

Це означає, що звичайний робочий процес базується на push-механізмі: один раз запустіть відокремлену роботу, а потім дозвольте середовищу виконання пробудити або сповістити вас після завершення. Опитуйте стан завдання лише тоді, коли потрібні налагодження, втручання або явний аудит.

### [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%BF%D0%BE%D0%BB%D1%96%D1%82%D0%B8%D0%BA%D0%B8-%D1%81%D0%BF%D0%BE%D0%B2%D1%96%D1%89%D0%B5%D0%BD%D1%8C)  Політики сповіщень

Керуйте тим, скільки повідомлень отримувати про кожне завдання:

| Політика | Що доставляється |
| --- | --- |
| `done_only` (типова) | Лише термінальний стан (succeeded, failed тощо) — **це типове значення** |
| `state_changes` | Кожен перехід стану та оновлення прогресу |
| `silent` | Нічого |

Змініть політику, поки завдання виконується:

```
openclaw tasks notify <lookup> state_changes
```

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%B4%D0%BE%D0%B2%D1%96%D0%B4%D0%BD%D0%B8%D0%BA-cli)  Довідник CLI

tasks list

```
openclaw tasks list [--runtime <acp|subagent|cron|cli>] [--status <status>] [--json]
```

Стовпці виводу: ID завдання, тип, статус, доставка, ID запуску, дочірній сеанс, підсумок.

tasks show

```
openclaw tasks show <lookup>
```

Токен пошуку приймає ID завдання, ID запуску або ключ сеансу. Показує повний запис, зокрема таймінг, стан доставки, помилку та термінальний підсумок.

tasks cancel

```
openclaw tasks cancel <lookup>
```

Для ACP-завдань і завдань підagentів це завершує дочірній сеанс. Для завдань, відстежуваних CLI, скасування записується в реєстрі завдань (окремого дескриптора дочірнього середовища виконання немає). Статус переходить у `cancelled`, а сповіщення про доставку надсилається, коли це застосовно.

tasks notify

```
openclaw tasks notify <lookup> <done_only|state_changes|silent>
```

tasks audit

```
openclaw tasks audit [--json]
```

Виявляє операційні проблеми. Знахідки також з’являються в `openclaw status`, коли виявлено проблеми.

| Виявлення | Серйозність | Тригер |
| --- | --- | --- |
| `stale_queued` | warn | У черзі понад 10 хвилин |
| `stale_running` | error | Виконується понад 30 хвилин |
| `lost` | warn/error | Власність завдання, підтримувана runtime, зникла; збережені втрачені завдання попереджають до `cleanupAfter`, потім стають помилками |
| `delivery_failed` | warn | Доставлення не вдалося, а політика сповіщення не є `silent` |
| `missing_cleanup` | warn | Термінальне завдання без часової позначки очищення |
| `inconsistent_timestamps` | warn | Порушення часової шкали (наприклад, завершено до початку) |

обслуговування завдань

```
openclaw tasks maintenance [--json]
openclaw tasks maintenance --apply [--json]
```

Використовуйте це, щоб попередньо переглянути або застосувати узгодження, проставлення міток очищення та обрізання для завдань і стану Task Flow.Узгодження враховує runtime:

- Завдання ACP/subagent перевіряють свій базовий дочірній сеанс.
- Завдання subagent, дочірній сеанс яких має tombstone відновлення після перезапуску, позначаються як втрачені, а не обробляються як відновлювані базові сеанси.
- Завдання Cron перевіряють, чи runtime cron досі володіє job, потім відновлюють термінальний статус зі збережених журналів запусків cron/стану job, перш ніж fallback до `lost`. Лише процес Gateway є авторитетним для in-memory набору активних job cron; офлайн-аудит CLI використовує довготривалу історію, але не позначає завдання cron як втрачене лише через те, що цей локальний Set порожній.
- Завдання CLI на основі чату перевіряють власний live run контекст, а не лише рядок сеансу чату.

Очищення після завершення також враховує runtime:

- Завершення subagent best-effort закриває відстежувані вкладки браузера/процеси для дочірнього сеансу, перш ніж триває очищення оголошення.
- Завершення ізольованого cron best-effort закриває відстежувані вкладки браузера/процеси для сеансу cron, перш ніж запуск повністю завершується.
- Доставлення ізольованого cron за потреби очікує подальші дії нащадкового subagent і пригнічує застарілий текст підтвердження батьківського процесу замість його оголошення.
- Доставлення завершення subagent віддає перевагу найновішому видимому тексту assistant; якщо він порожній, воно fallback до sanitized найновішого тексту tool/toolResult, а запуски викликів інструментів лише з timeout можуть згортатися до короткого підсумку часткового прогресу. Термінальні невдалі запуски оголошують статус помилки без повторного відтворення захопленого тексту відповіді.
- Помилки очищення не маскують реальний результат завдання.

tasks flow list \| show \| cancel

```
openclaw tasks flow list [--status <status>] [--json]
openclaw tasks flow show <lookup> [--json]
openclaw tasks flow cancel <lookup>
```

Використовуйте ці команди, коли вас цікавить оркеструвальний Task Flow, а не один окремий запис фонового завдання.

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%B4%D0%BE%D1%88%D0%BA%D0%B0-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D1%8C-%D1%87%D0%B0%D1%82%D1%83-/tasks)  Дошка завдань чату (`/tasks`)

Використовуйте `/tasks` у будь-якому сеансі чату, щоб побачити фонові завдання, пов’язані з цим сеансом. Дошка показує активні та нещодавно завершені завдання з runtime, статусом, часом, а також прогресом або деталями помилки.Коли поточний сеанс не має видимих пов’язаних завдань, `/tasks` fallback до локальних для агента лічильників завдань, тож ви все одно отримуєте огляд без витоку деталей інших сеансів.Для повного операторського журналу використовуйте CLI: `openclaw tasks list`.

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D1%96%D0%BD%D1%82%D0%B5%D0%B3%D1%80%D0%B0%D1%86%D1%96%D1%8F-%D1%81%D1%82%D0%B0%D1%82%D1%83%D1%81%D1%83-%D0%BD%D0%B0%D0%B2%D0%B0%D0%BD%D1%82%D0%B0%D0%B6%D0%B5%D0%BD%D0%BD%D1%8F-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D1%8C)  Інтеграція статусу (навантаження завдань)

`openclaw status` містить короткий підсумок завдань:

```
Tasks: 3 queued · 2 running · 1 issues
```

Підсумок повідомляє:

- **active** — кількість `queued` \+ `running`
- **failures** — кількість `failed` \+ `timed_out` \+ `lost`
- **byRuntime** — розподіл за `acp`, `subagent`, `cron`, `cli`

І `/status`, і інструмент `session_status` використовують task snapshot з урахуванням очищення: активні завдання мають пріоритет, застарілі завершені рядки приховуються, а нещодавні помилки показуються лише тоді, коли не залишилося активної роботи. Це зберігає картку статусу зосередженою на тому, що важливо зараз.

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%B7%D0%B1%D0%B5%D1%80%D1%96%D0%B3%D0%B0%D0%BD%D0%BD%D1%8F-%D1%82%D0%B0-%D0%BE%D0%B1%D1%81%D0%BB%D1%83%D0%B3%D0%BE%D0%B2%D1%83%D0%B2%D0%B0%D0%BD%D0%BD%D1%8F)  Зберігання та обслуговування

### [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%B4%D0%B5-%D0%B7%D0%B1%D0%B5%D1%80%D1%96%D0%B3%D0%B0%D1%8E%D1%82%D1%8C%D1%81%D1%8F-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F)  Де зберігаються завдання

Записи завдань зберігаються в SQLite за адресою:

```
$OPENCLAW_STATE_DIR/tasks/runs.sqlite
```

Registry завантажується в пам’ять під час запуску gateway і синхронізує записи в SQLite для довговічності між перезапусками.
Gateway тримає write-ahead log SQLite обмеженим, використовуючи стандартний поріг
autocheckpoint SQLite, а також періодичні та shutdown `TRUNCATE` checkpoints.

### [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%B0%D0%B2%D1%82%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D1%87%D0%BD%D0%B5-%D0%BE%D0%B1%D1%81%D0%BB%D1%83%D0%B3%D0%BE%D0%B2%D1%83%D0%B2%D0%B0%D0%BD%D0%BD%D1%8F)  Автоматичне обслуговування

Sweeper запускається кожні **60 секунд** і виконує чотири дії:

1

[Navigate to header](https://docs.openclaw.ai/uk/automation/tasks#)

Узгодження

Перевіряє, чи активні завдання досі мають авторитетну runtime-підтримку. Завдання ACP/subagent використовують стан дочірнього сеансу, завдання cron використовують володіння active-job, а завдання CLI на основі чату використовують власний run context. Якщо цей базовий стан відсутній понад 5 хвилин, завдання позначається як `lost`.

2

[Navigate to header](https://docs.openclaw.ai/uk/automation/tasks#)

Відновлення сеансу ACP

Закриває термінальні або осиротілі parent-owned одноразові сеанси ACP, а також закриває застарілі термінальні або осиротілі persistent сеанси ACP лише тоді, коли не залишається активної прив’язки розмови.

3

[Navigate to header](https://docs.openclaw.ai/uk/automation/tasks#)

Проставлення міток очищення

Установлює часову позначку `cleanupAfter` для термінальних завдань (endedAt + 7 днів). Під час retention втрачені завдання все ще з’являються в аудиті як попередження; після завершення строку `cleanupAfter` або коли метадані очищення відсутні, вони є помилками.

4

[Navigate to header](https://docs.openclaw.ai/uk/automation/tasks#)

Обрізання

Видаляє записи після їхньої дати `cleanupAfter`.

**Retention:** записи термінальних завдань зберігаються **7 днів**, потім автоматично обрізаються. Налаштування не потрібне.

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D1%8F%D0%BA-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F-%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D1%96-%D0%B7-%D1%96%D0%BD%D1%88%D0%B8%D0%BC%D0%B8-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%D0%BC%D0%B8)  Як завдання пов’язані з іншими системами

Завдання і Task Flow

[Task Flow](https://docs.openclaw.ai/uk/automation/taskflow) — це шар оркестрації потоків над фоновими завданнями. Один flow може координувати кілька завдань протягом свого життєвого циклу, використовуючи керовані або mirrored режими sync. Використовуйте `openclaw tasks`, щоб перевіряти окремі записи завдань, і `openclaw tasks flow`, щоб перевіряти оркеструвальний flow.Див. [Task Flow](https://docs.openclaw.ai/uk/automation/taskflow) для деталей.

Завдання і cron

**Визначення** cron job зберігається в `~/.openclaw/cron/jobs.json`; runtime-стан виконання зберігається поруч у `~/.openclaw/cron/jobs-state.json`. **Кожне** виконання cron створює запис завдання — і main-session, і isolated. Завдання cron main-session типово мають політику сповіщення `silent`, щоб їх можна було відстежувати без створення сповіщень.Див. [Cron Jobs](https://docs.openclaw.ai/uk/automation/cron-jobs).

Завдання і Heartbeat

Запуски Heartbeat — це ходи main-session, вони не створюють записів завдань. Коли завдання завершується, воно може ініціювати heartbeat wake, щоб ви швидко побачили результат.Див. [Heartbeat](https://docs.openclaw.ai/uk/gateway/heartbeat).

Завдання і сеанси

Завдання може посилатися на `childSessionKey` (де виконується робота) і `requesterSessionKey` (хто його запустив). Сеанси — це контекст розмови; завдання — це відстеження активності поверх нього.

Завдання і запуски агента

`runId` завдання пов’язує його із запуском агента, який виконує роботу. Події життєвого циклу агента (початок, завершення, помилка) автоматично оновлюють статус завдання — вам не потрібно керувати життєвим циклом вручну.

## [​](https://docs.openclaw.ai/uk/automation/tasks\#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)  Пов’язане

- [Автоматизація і завдання](https://docs.openclaw.ai/uk/automation) — усі механізми автоматизації з першого погляду
- [CLI: Завдання](https://docs.openclaw.ai/uk/cli/tasks) — довідник команд CLI
- [Heartbeat](https://docs.openclaw.ai/uk/gateway/heartbeat) — періодичні ходи main-session
- [Заплановані завдання](https://docs.openclaw.ai/uk/automation/cron-jobs) — планування фонової роботи
- [Task Flow](https://docs.openclaw.ai/uk/automation/taskflow) — оркестрація flow над завданнями

[Scheduled tasks](https://docs.openclaw.ai/uk/automation/cron-jobs) [Потік завдань](https://docs.openclaw.ai/uk/automation/taskflow)

Ctrl+I