---
source_url: https://docs.openclaw.ai/uk/automation
title: "\u0410\u0432\u0442\u043e\u043c\u0430\u0442\u0438\u0437\u0430\u0446\u0456\u044f \u0442\u0430 \u0437\u0430\u0432\u0434\u0430\u043d\u043d\u044f - OpenClaw"
---

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/automation#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/uk)

![UA](https://d3gk2c5xim1je2.cloudfront.net/flags/UA.svg)

Українська

Пошук...

Ctrl K

Пошук...

Navigation

Automation and tasks

Автоматизація та завдання

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Короткий посібник із вибору](https://docs.openclaw.ai/uk/automation#%D0%BA%D0%BE%D1%80%D0%BE%D1%82%D0%BA%D0%B8%D0%B9-%D0%BF%D0%BE%D1%81%D1%96%D0%B1%D0%BD%D0%B8%D0%BA-%D1%96%D0%B7-%D0%B2%D0%B8%D0%B1%D0%BE%D1%80%D1%83)
- [Заплановані завдання (Cron) і Heartbeat](https://docs.openclaw.ai/uk/automation#%D0%B7%D0%B0%D0%BF%D0%BB%D0%B0%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D1%96-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F-cron-%D1%96-heartbeat)
- [Основні поняття](https://docs.openclaw.ai/uk/automation#%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D1%96-%D0%BF%D0%BE%D0%BD%D1%8F%D1%82%D1%82%D1%8F)
- [Заплановані завдання (cron)](https://docs.openclaw.ai/uk/automation#%D0%B7%D0%B0%D0%BF%D0%BB%D0%B0%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D1%96-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F-cron)
- [Завдання](https://docs.openclaw.ai/uk/automation#%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F)
- [Визначені зобов’язання](https://docs.openclaw.ai/uk/automation#%D0%B2%D0%B8%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D1%96-%D0%B7%D0%BE%D0%B1%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%BD%D1%8F)
- [TaskFlow](https://docs.openclaw.ai/uk/automation#taskflow)
- [Постійні інструкції](https://docs.openclaw.ai/uk/automation#%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D1%96-%D1%96%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%86%D1%96%D1%97)
- [Хуки](https://docs.openclaw.ai/uk/automation#%D1%85%D1%83%D0%BA%D0%B8)
- [Heartbeat](https://docs.openclaw.ai/uk/automation#heartbeat)
- [Як вони працюють разом](https://docs.openclaw.ai/uk/automation#%D1%8F%D0%BA-%D0%B2%D0%BE%D0%BD%D0%B8-%D0%BF%D1%80%D0%B0%D1%86%D1%8E%D1%8E%D1%82%D1%8C-%D1%80%D0%B0%D0%B7%D0%BE%D0%BC)
- [Пов’язане](https://docs.openclaw.ai/uk/automation#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw виконує роботу у фоновому режимі через завдання, заплановані завдання, визначені
зобов’язання, хуки подій і постійні інструкції. Ця сторінка допомагає вибрати
правильний механізм і зрозуміти, як вони працюють разом.

## [​](https://docs.openclaw.ai/uk/automation\#%D0%BA%D0%BE%D1%80%D0%BE%D1%82%D0%BA%D0%B8%D0%B9-%D0%BF%D0%BE%D1%81%D1%96%D0%B1%D0%BD%D0%B8%D0%BA-%D1%96%D0%B7-%D0%B2%D0%B8%D0%B1%D0%BE%D1%80%D1%83)  Короткий посібник із вибору

| Варіант використання | Рекомендовано | Чому |
| --- | --- | --- |
| Надіслати щоденний звіт рівно о 9:00 | Заплановані завдання (Cron) | Точний час, ізольоване виконання |
| Нагадати мені через 20 хвилин | Заплановані завдання (Cron) | Одноразове завдання з точним часом (`--at`) |
| Запускати щотижневий глибокий аналіз | Заплановані завдання (Cron) | Самостійне завдання, може використовувати іншу модель |
| Перевіряти вхідні кожні 30 хвилин | Heartbeat | Об’єднується з іншими перевірками, враховує контекст |
| Відстежувати календар на наявність майбутніх подій | Heartbeat | Природно підходить для періодичної обізнаності |
| Перевірити стан після згаданої співбесіди | Визначені зобов’язання | Схоже на пам’ять подальше звернення, без точного запиту нагадування |
| М’яка перевірка турботи після контексту користувача | Визначені зобов’язання | Обмежено тим самим агентом і каналом |
| Перевірити стан підагента або запуску ACP | Фонові завдання | Журнал завдань відстежує всю відокремлену роботу |
| Перевірити, що запускалося і коли | Фонові завдання | `openclaw tasks list` і `openclaw tasks audit` |
| Багатоетапне дослідження з подальшим підсумком | TaskFlow | Стійка оркестрація з відстеженням ревізій |
| Запустити скрипт під час скидання сеансу | Хуки | Керовані подіями, спрацьовують на події життєвого циклу |
| Виконувати код під час кожного виклику інструмента | Хуки Plugin | Внутрішньопроцесні хуки можуть перехоплювати виклики інструментів |
| Завжди перевіряти відповідність перед відповіддю | Постійні інструкції | Автоматично додаються до кожного сеансу |

### [​](https://docs.openclaw.ai/uk/automation\#%D0%B7%D0%B0%D0%BF%D0%BB%D0%B0%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D1%96-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F-cron-%D1%96-heartbeat)  Заплановані завдання (Cron) і Heartbeat

| Вимір | Заплановані завдання (Cron) | Heartbeat |
| --- | --- | --- |
| Час | Точний (вирази cron, одноразові завдання) | Приблизний (типово кожні 30 хвилин) |
| Контекст сеансу | Новий (ізольований) або спільний | Повний контекст основного сеансу |
| Записи завдань | Завжди створюються | Ніколи не створюються |
| Доставка | Канал, webhook або без повідомлення | Вбудовано в основний сеанс |
| Найкраще для | Звітів, нагадувань, фонових завдань | Перевірок вхідних, календаря, сповіщень |

Використовуйте заплановані завдання (Cron), коли потрібен точний час або ізольоване виконання. Використовуйте Heartbeat, коли робота виграє від повного контексту сеансу, а приблизний час є прийнятним.

## [​](https://docs.openclaw.ai/uk/automation\#%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D1%96-%D0%BF%D0%BE%D0%BD%D1%8F%D1%82%D1%82%D1%8F)  Основні поняття

### [​](https://docs.openclaw.ai/uk/automation\#%D0%B7%D0%B0%D0%BF%D0%BB%D0%B0%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D1%96-%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F-cron)  Заплановані завдання (cron)

Cron — це вбудований планувальник Gateway для точного часу. Він зберігає завдання, пробуджує агента у потрібний момент і може доставляти результат у чат-канал або endpoint webhook. Підтримує одноразові нагадування, повторювані вирази й вхідні тригери webhook.Див. [Заплановані завдання](https://docs.openclaw.ai/uk/automation/cron-jobs).

### [​](https://docs.openclaw.ai/uk/automation\#%D0%B7%D0%B0%D0%B2%D0%B4%D0%B0%D0%BD%D0%BD%D1%8F)  Завдання

Журнал фонових завдань відстежує всю відокремлену роботу: запуски ACP, створення підагентів, ізольовані виконання cron і операції CLI. Завдання — це записи, а не планувальники. Використовуйте `openclaw tasks list` і `openclaw tasks audit`, щоб їх переглядати.Див. [Фонові завдання](https://docs.openclaw.ai/uk/automation/tasks).

### [​](https://docs.openclaw.ai/uk/automation\#%D0%B2%D0%B8%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D1%96-%D0%B7%D0%BE%D0%B1%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%BD%D1%8F)  Визначені зобов’язання

Зобов’язання — це добровільні, короткочасні спогади для подальших звернень. OpenClaw визначає їх
зі звичайних розмов, обмежує тим самим агентом і каналом та
доставляє належні перевірки через Heartbeat. Точні нагадування, які прямо просить користувач, усе ще
належать до cron.Див. [Визначені зобов’язання](https://docs.openclaw.ai/uk/concepts/commitments).

### [​](https://docs.openclaw.ai/uk/automation\#taskflow)  TaskFlow

TaskFlow — це основа оркестрації потоків поверх фонових завдань. Він керує стійкими багатоетапними потоками з керованими й дзеркальними режимами синхронізації, відстеженням ревізій і `openclaw tasks flow list|show|cancel` для перегляду.Див. [TaskFlow](https://docs.openclaw.ai/uk/automation/taskflow).

### [​](https://docs.openclaw.ai/uk/automation\#%D0%BF%D0%BE%D1%81%D1%82%D1%96%D0%B9%D0%BD%D1%96-%D1%96%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%86%D1%96%D1%97)  Постійні інструкції

Постійні інструкції надають агенту постійні операційні повноваження для визначених програм. Вони зберігаються у файлах робочого простору (зазвичай `AGENTS.md`) і додаються до кожного сеансу. Поєднуйте з cron для застосування на основі часу.Див. [Постійні інструкції](https://docs.openclaw.ai/uk/automation/standing-orders).

### [​](https://docs.openclaw.ai/uk/automation\#%D1%85%D1%83%D0%BA%D0%B8)  Хуки

Внутрішні хуки — це керовані подіями скрипти, що запускаються подіями життєвого циклу агента
(`/new`, `/reset`, `/stop`), Compaction сеансу, запуском Gateway і потоком
повідомлень. Вони автоматично виявляються в каталогах і можуть керуватися
через `openclaw hooks`. Для внутрішньопроцесного перехоплення викликів інструментів використовуйте
[хуки Plugin](https://docs.openclaw.ai/uk/plugins/hooks).Див. [Хуки](https://docs.openclaw.ai/uk/automation/hooks).

### [​](https://docs.openclaw.ai/uk/automation\#heartbeat)  Heartbeat

Heartbeat — це періодичний хід основного сеансу (типово кожні 30 хвилин). Він об’єднує кілька перевірок (вхідні, календар, сповіщення) в один хід агента з повним контекстом сеансу. Ходи Heartbeat не створюють записів завдань і не подовжують свіжість щоденного/неактивного скидання сеансу. Використовуйте `HEARTBEAT.md` для невеликого контрольного списку або блок `tasks:`, коли потрібні лише належні періодичні перевірки всередині самого Heartbeat. Порожні файли Heartbeat пропускаються як `empty-heartbeat-file`; режим завдань лише за строком пропускається як `no-tasks-due`. Heartbeat відкладаються, поки робота cron активна або в черзі, а `heartbeat.skipWhenBusy` також може відкладати їх, коли зайняті підагенти або вкладені лінії.Див. [Heartbeat](https://docs.openclaw.ai/uk/gateway/heartbeat).

## [​](https://docs.openclaw.ai/uk/automation\#%D1%8F%D0%BA-%D0%B2%D0%BE%D0%BD%D0%B8-%D0%BF%D1%80%D0%B0%D1%86%D1%8E%D1%8E%D1%82%D1%8C-%D1%80%D0%B0%D0%B7%D0%BE%D0%BC)  Як вони працюють разом

- **Cron** обробляє точні розклади (щоденні звіти, щотижневі огляди) та одноразові нагадування. Усі виконання cron створюють записи завдань.
- **Heartbeat** обробляє регулярний моніторинг (вхідні, календар, сповіщення) одним об’єднаним ходом кожні 30 хвилин.
- **Хуки** реагують на конкретні події (скидання сеансу, Compaction, потік повідомлень) за допомогою користувацьких скриптів. Хуки Plugin охоплюють виклики інструментів.
- **Постійні інструкції** дають агенту сталий контекст і межі повноважень.
- **TaskFlow** координує багатоетапні потоки поверх окремих завдань.
- **Завдання** автоматично відстежують всю відокремлену роботу, щоб ви могли її переглядати й аудіювати.

## [​](https://docs.openclaw.ai/uk/automation\#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)  Пов’язане

- [Заплановані завдання](https://docs.openclaw.ai/uk/automation/cron-jobs) — точне планування та одноразові нагадування
- [Визначені зобов’язання](https://docs.openclaw.ai/uk/concepts/commitments) — схожі на пам’ять подальші перевірки
- [Фонові завдання](https://docs.openclaw.ai/uk/automation/tasks) — журнал завдань для всієї відокремленої роботи
- [TaskFlow](https://docs.openclaw.ai/uk/automation/taskflow) — стійка оркестрація багатоетапних потоків
- [Хуки](https://docs.openclaw.ai/uk/automation/hooks) — керовані подіями скрипти життєвого циклу
- [Хуки Plugin](https://docs.openclaw.ai/uk/plugins/hooks) — внутрішньопроцесні хуки інструментів, запитів, повідомлень і життєвого циклу
- [Постійні інструкції](https://docs.openclaw.ai/uk/automation/standing-orders) — сталі інструкції агента
- [Heartbeat](https://docs.openclaw.ai/uk/gateway/heartbeat) — періодичні ходи основного сеансу
- [Довідник конфігурації](https://docs.openclaw.ai/uk/gateway/configuration-reference) — усі ключі конфігурації

[OpenProse](https://docs.openclaw.ai/uk/prose) [Scheduled tasks](https://docs.openclaw.ai/uk/automation/cron-jobs)

Ctrl+I