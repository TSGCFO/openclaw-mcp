---
source_url: https://docs.openclaw.ai/uk/automation/hooks
title: "\u0425\u0443\u043a\u0438 - OpenClaw"
---

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/automation/hooks#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/uk)

![UA](https://d3gk2c5xim1je2.cloudfront.net/flags/UA.svg)

Українська

Пошук...

Ctrl K

Пошук...

Navigation

Automation and tasks

Хуки

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Швидкий старт](https://docs.openclaw.ai/uk/automation/hooks#%D1%88%D0%B2%D0%B8%D0%B4%D0%BA%D0%B8%D0%B9-%D1%81%D1%82%D0%B0%D1%80%D1%82)
- [Типи подій](https://docs.openclaw.ai/uk/automation/hooks#%D1%82%D0%B8%D0%BF%D0%B8-%D0%BF%D0%BE%D0%B4%D1%96%D0%B9)
- [Написання хуків](https://docs.openclaw.ai/uk/automation/hooks#%D0%BD%D0%B0%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%BD%D1%8F-%D1%85%D1%83%D0%BA%D1%96%D0%B2)
- [Структура хука](https://docs.openclaw.ai/uk/automation/hooks#%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D0%B0-%D1%85%D1%83%D0%BA%D0%B0)
- [Формат HOOK.md](https://docs.openclaw.ai/uk/automation/hooks#%D1%84%D0%BE%D1%80%D0%BC%D0%B0%D1%82-hook-md)
- [Реалізація обробника](https://docs.openclaw.ai/uk/automation/hooks#%D1%80%D0%B5%D0%B0%D0%BB%D1%96%D0%B7%D0%B0%D1%86%D1%96%D1%8F-%D0%BE%D0%B1%D1%80%D0%BE%D0%B1%D0%BD%D0%B8%D0%BA%D0%B0)
- [Основні елементи контексту подій](https://docs.openclaw.ai/uk/automation/hooks#%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D1%96-%D0%B5%D0%BB%D0%B5%D0%BC%D0%B5%D0%BD%D1%82%D0%B8-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%BA%D1%81%D1%82%D1%83-%D0%BF%D0%BE%D0%B4%D1%96%D0%B9)
- [Виявлення хуків](https://docs.openclaw.ai/uk/automation/hooks#%D0%B2%D0%B8%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D1%85%D1%83%D0%BA%D1%96%D0%B2)
- [Паки хуків](https://docs.openclaw.ai/uk/automation/hooks#%D0%BF%D0%B0%D0%BA%D0%B8-%D1%85%D1%83%D0%BA%D1%96%D0%B2)
- [Вбудовані хуки](https://docs.openclaw.ai/uk/automation/hooks#%D0%B2%D0%B1%D1%83%D0%B4%D0%BE%D0%B2%D0%B0%D0%BD%D1%96-%D1%85%D1%83%D0%BA%D0%B8)
- [Докладно про session-memory](https://docs.openclaw.ai/uk/automation/hooks#%D0%B4%D0%BE%D0%BA%D0%BB%D0%B0%D0%B4%D0%BD%D0%BE-%D0%BF%D1%80%D0%BE-session-memory)
- [Конфігурація bootstrap-extra-files](https://docs.openclaw.ai/uk/automation/hooks#%D0%BA%D0%BE%D0%BD%D1%84%D1%96%D0%B3%D1%83%D1%80%D0%B0%D1%86%D1%96%D1%8F-bootstrap-extra-files)
- [Докладно про command-logger](https://docs.openclaw.ai/uk/automation/hooks#%D0%B4%D0%BE%D0%BA%D0%BB%D0%B0%D0%B4%D0%BD%D0%BE-%D0%BF%D1%80%D0%BE-command-logger)
- [Докладно про boot-md](https://docs.openclaw.ai/uk/automation/hooks#%D0%B4%D0%BE%D0%BA%D0%BB%D0%B0%D0%B4%D0%BD%D0%BE-%D0%BF%D1%80%D0%BE-boot-md)
- [Хуки плагінів](https://docs.openclaw.ai/uk/automation/hooks#%D1%85%D1%83%D0%BA%D0%B8-%D0%BF%D0%BB%D0%B0%D0%B3%D1%96%D0%BD%D1%96%D0%B2)
- [Конфігурація](https://docs.openclaw.ai/uk/automation/hooks#%D0%BA%D0%BE%D0%BD%D1%84%D1%96%D0%B3%D1%83%D1%80%D0%B0%D1%86%D1%96%D1%8F)
- [Довідник CLI](https://docs.openclaw.ai/uk/automation/hooks#%D0%B4%D0%BE%D0%B2%D1%96%D0%B4%D0%BD%D0%B8%D0%BA-cli)
- [Рекомендовані практики](https://docs.openclaw.ai/uk/automation/hooks#%D1%80%D0%B5%D0%BA%D0%BE%D0%BC%D0%B5%D0%BD%D0%B4%D0%BE%D0%B2%D0%B0%D0%BD%D1%96-%D0%BF%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D0%BA%D0%B8)
- [Усунення несправностей](https://docs.openclaw.ai/uk/automation/hooks#%D1%83%D1%81%D1%83%D0%BD%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BD%D0%B5%D1%81%D0%BF%D1%80%D0%B0%D0%B2%D0%BD%D0%BE%D1%81%D1%82%D0%B5%D0%B9)
- [Хук не виявлено](https://docs.openclaw.ai/uk/automation/hooks#%D1%85%D1%83%D0%BA-%D0%BD%D0%B5-%D0%B2%D0%B8%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%BE)
- [Хук не придатний](https://docs.openclaw.ai/uk/automation/hooks#%D1%85%D1%83%D0%BA-%D0%BD%D0%B5-%D0%BF%D1%80%D0%B8%D0%B4%D0%B0%D1%82%D0%BD%D0%B8%D0%B9)
- [Хук не виконується](https://docs.openclaw.ai/uk/automation/hooks#%D1%85%D1%83%D0%BA-%D0%BD%D0%B5-%D0%B2%D0%B8%D0%BA%D0%BE%D0%BD%D1%83%D1%94%D1%82%D1%8C%D1%81%D1%8F)
- [Пов’язане](https://docs.openclaw.ai/uk/automation/hooks#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Хуки — це невеликі скрипти, які запускаються, коли щось відбувається всередині Gateway. Їх можна знаходити в каталогах і переглядати за допомогою `openclaw hooks`. Gateway завантажує внутрішні хуки лише після того, як ви ввімкнете хуки або налаштуєте принаймні один запис хука, пак хука, застарілий обробник чи додатковий каталог хуків.В OpenClaw є два види хуків:

- **Внутрішні хуки** (ця сторінка): запускаються всередині Gateway, коли спрацьовують події агента, як-от `/new`, `/reset`, `/stop` або події життєвого циклу.
- **Webhooks**: зовнішні HTTP-ендпоїнти, які дають змогу іншим системам запускати роботу в OpenClaw. Див. [Webhooks](https://docs.openclaw.ai/uk/automation/cron-jobs#webhooks).

Хуки також можуть постачатися всередині плагінів. `openclaw hooks list` показує як окремі хуки, так і хуки, якими керують плагіни.

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%88%D0%B2%D0%B8%D0%B4%D0%BA%D0%B8%D0%B9-%D1%81%D1%82%D0%B0%D1%80%D1%82)  Швидкий старт

```
# Перелічити доступні хуки
openclaw hooks list

# Увімкнути хук
openclaw hooks enable session-memory

# Перевірити стан хуків
openclaw hooks check

# Отримати детальну інформацію
openclaw hooks info session-memory
```

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%82%D0%B8%D0%BF%D0%B8-%D0%BF%D0%BE%D0%B4%D1%96%D0%B9)  Типи подій

| Подія | Коли спрацьовує |
| --- | --- |
| `command:new` | Видано команду `/new` |
| `command:reset` | Видано команду `/reset` |
| `command:stop` | Видано команду `/stop` |
| `command` | Будь-яка подія команди (загальний слухач) |
| `session:compact:before` | До того, як Compaction підсумує історію |
| `session:compact:after` | Після завершення Compaction |
| `session:patch` | Коли властивості сесії змінено |
| `agent:bootstrap` | До того, як буде впроваджено файли bootstrap робочої теки |
| `gateway:startup` | Після запуску каналів і завантаження хуків |
| `gateway:shutdown` | Коли починається завершення роботи gateway |
| `gateway:pre-restart` | До очікуваного перезапуску gateway |
| `message:received` | Вхідне повідомлення з будь-якого каналу |
| `message:transcribed` | Після завершення транскрибування аудіо |
| `message:preprocessed` | Після завершення або пропуску попередньої обробки медіа та посилань |
| `message:sent` | Вихідне повідомлення доставлено |

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%BD%D0%B0%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%BD%D1%8F-%D1%85%D1%83%D0%BA%D1%96%D0%B2)  Написання хуків

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D0%B0-%D1%85%D1%83%D0%BA%D0%B0)  Структура хука

Кожен хук — це каталог, що містить два файли:

```
my-hook/
├── HOOK.md          # Метадані + документація
└── handler.ts       # Реалізація обробника
```

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%84%D0%BE%D1%80%D0%BC%D0%B0%D1%82-hook-md)  Формат HOOK.md

```
---
name: my-hook
description: "Короткий опис того, що робить цей хук"
metadata:
  { "openclaw": { "emoji": "🔗", "events": ["command:new"], "requires": { "bins": ["node"] } } }
---

# My Hook

Тут розміщується детальна документація.
```

**Поля метаданих** (`metadata.openclaw`):

| Поле | Опис |
| --- | --- |
| `emoji` | Emoji для відображення в CLI |
| `events` | Масив подій, які слід слухати |
| `export` | Іменований експорт для використання (типово `"default"`) |
| `os` | Обов’язкові платформи (наприклад, `["darwin", "linux"]`) |
| `requires` | Обов’язкові `bins`, `anyBins`, `env` або шляхи `config` |
| `always` | Обійти перевірки придатності (boolean) |
| `install` | Методи встановлення |

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%80%D0%B5%D0%B0%D0%BB%D1%96%D0%B7%D0%B0%D1%86%D1%96%D1%8F-%D0%BE%D0%B1%D1%80%D0%BE%D0%B1%D0%BD%D0%B8%D0%BA%D0%B0)  Реалізація обробника

```
const handler = async (event) => {
  if (event.type !== "command" || event.action !== "new") {
    return;
  }

  console.log(`[my-hook] Спрацювала команда new`);
  // Ваша логіка тут

  // За потреби надішліть повідомлення користувачу
  event.messages.push("Хук виконано!");
};

export default handler;
```

Кожна подія містить: `type`, `action`, `sessionKey`, `timestamp`, `messages` (додайте через push, щоб надіслати користувачу), і `context` (дані, специфічні для події). Контексти хуків плагінів агента та інструментів також можуть містити `trace` — контекст діагностичного трасування лише для читання, сумісний із W3C, який плагіни можуть передавати в структуровані журнали для кореляції OTEL.

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%BD%D1%96-%D0%B5%D0%BB%D0%B5%D0%BC%D0%B5%D0%BD%D1%82%D0%B8-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%BA%D1%81%D1%82%D1%83-%D0%BF%D0%BE%D0%B4%D1%96%D0%B9)  Основні елементи контексту подій

**Події команд** (`command:new`, `command:reset`): `context.sessionEntry`, `context.previousSessionEntry`, `context.commandSource`, `context.workspaceDir`, `context.cfg`.**Події повідомлень** (`message:received`): `context.from`, `context.content`, `context.channelId`, `context.metadata` (дані, специфічні для провайдера, зокрема `senderId`, `senderName`, `guildId`).**Події повідомлень** (`message:sent`): `context.to`, `context.content`, `context.success`, `context.channelId`.**Події повідомлень** (`message:transcribed`): `context.transcript`, `context.from`, `context.channelId`, `context.mediaPath`.**Події повідомлень** (`message:preprocessed`): `context.bodyForAgent` (остаточний збагачений вміст), `context.from`, `context.channelId`.**Події bootstrap** (`agent:bootstrap`): `context.bootstrapFiles` (змінюваний масив), `context.agentId`.**Події patch сесії** (`session:patch`): `context.sessionEntry`, `context.patch` (лише змінені поля), `context.cfg`. Події patch можуть запускати лише привілейовані клієнти.**Події Compaction**: `session:compact:before` містить `messageCount`, `tokenCount`. `session:compact:after` додатково містить `compactedCount`, `summaryLength`, `tokensBefore`, `tokensAfter`.`command:stop` спостерігає за тим, як користувач видає `/stop`; це скасування/життєвий цикл команди, а не бар’єр фіналізації агента. Плагінам, яким потрібно перевірити природну фінальну відповідь і попросити агента зробити ще один прохід, слід натомість використовувати типізований хук плагіна `before_agent_finalize`. Див. [Plugin hooks](https://docs.openclaw.ai/uk/plugins/hooks).**Події життєвого циклу Gateway**: `gateway:shutdown` містить `reason` і `restartExpectedMs` та спрацьовує, коли починається завершення роботи gateway. `gateway:pre-restart` містить той самий контекст, але спрацьовує лише тоді, коли завершення роботи є частиною очікуваного перезапуску і передано скінченне значення `restartExpectedMs`. Під час завершення роботи очікування кожного хука життєвого циклу є best-effort і обмежене за часом, тому завершення роботи триває навіть якщо обробник зависає.

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%B2%D0%B8%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D1%85%D1%83%D0%BA%D1%96%D0%B2)  Виявлення хуків

Хуки виявляються в таких каталогах у порядку зростання пріоритету перевизначення:

1. **Вбудовані хуки**: постачаються разом з OpenClaw
2. **Хуки плагінів**: хуки, вбудовані у встановлені плагіни
3. **Керовані хуки**: `~/.openclaw/hooks/` (встановлені користувачем, спільні для всіх робочих тек). Додаткові каталоги з `hooks.internal.load.extraDirs` мають той самий пріоритет.
4. **Хуки робочої теки**: `<workspace>/hooks/` (для конкретного агента, типово вимкнені, доки їх явно не ввімкнути)

Хуки робочої теки можуть додавати нові назви хуків, але не можуть перевизначати вбудовані, керовані або надані плагінами хуки з тією самою назвою.Gateway пропускає виявлення внутрішніх хуків під час запуску, доки внутрішні хуки не будуть налаштовані. Увімкніть вбудований або керований хук за допомогою `openclaw hooks enable <name>`, встановіть пак хука або задайте `hooks.internal.enabled=true`, щоб увімкнути цю можливість. Коли ви вмикаєте один іменований хук, Gateway завантажує лише обробник цього хука; `hooks.internal.enabled=true`, додаткові каталоги хуків і застарілі обробники вмикають широке виявлення.

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%BF%D0%B0%D0%BA%D0%B8-%D1%85%D1%83%D0%BA%D1%96%D0%B2)  Паки хуків

Паки хуків — це npm-пакети, які експортують хуки через `openclaw.hooks` у `package.json`. Встановлення:

```
openclaw plugins install <path-or-spec>
```

Npm-специфікації підтримують лише реєстр (назва пакета + необов’язкова точна версія або dist-tag). Git/URL/file-специфікації та діапазони semver відхиляються.

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%B2%D0%B1%D1%83%D0%B4%D0%BE%D0%B2%D0%B0%D0%BD%D1%96-%D1%85%D1%83%D0%BA%D0%B8)  Вбудовані хуки

| Хук | Події | Що робить |
| --- | --- | --- |
| session-memory | `command:new`, `command:reset` | Зберігає контекст сесії в `<workspace>/memory/` |
| bootstrap-extra-files | `agent:bootstrap` | Впроваджує додаткові bootstrap-файли за glob-шаблонами |
| command-logger | `command` | Логує всі команди в `~/.openclaw/logs/commands.log` |
| boot-md | `gateway:startup` | Запускає `BOOT.md` під час запуску gateway |

Увімкнути будь-який вбудований хук:

```
openclaw hooks enable <hook-name>
```

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%B4%D0%BE%D0%BA%D0%BB%D0%B0%D0%B4%D0%BD%D0%BE-%D0%BF%D1%80%D0%BE-session-memory)  Докладно про session-memory

Витягує останні 15 повідомлень користувача/асистента, генерує описовий slug назви файлу через LLM і зберігає в `<workspace>/memory/YYYY-MM-DD-slug.md`, використовуючи локальну дату хоста. Потрібно, щоб було налаштовано `workspace.dir`.

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%BA%D0%BE%D0%BD%D1%84%D1%96%D0%B3%D1%83%D1%80%D0%B0%D1%86%D1%96%D1%8F-bootstrap-extra-files)  Конфігурація bootstrap-extra-files

```
{
  "hooks": {
    "internal": {
      "entries": {
        "bootstrap-extra-files": {
          "enabled": true,
          "paths": ["packages/*/AGENTS.md", "packages/*/TOOLS.md"]
        }
      }
    }
  }
}
```

Шляхи обчислюються відносно робочої теки. Завантажуються лише розпізнані базові назви bootstrap-файлів (`AGENTS.md`, `SOUL.md`, `TOOLS.md`, `IDENTITY.md`, `USER.md`, `HEARTBEAT.md`, `BOOTSTRAP.md`, `MEMORY.md`).

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%B4%D0%BE%D0%BA%D0%BB%D0%B0%D0%B4%D0%BD%D0%BE-%D0%BF%D1%80%D0%BE-command-logger)  Докладно про command-logger

Логує кожну slash-команду в `~/.openclaw/logs/commands.log`.

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%B4%D0%BE%D0%BA%D0%BB%D0%B0%D0%B4%D0%BD%D0%BE-%D0%BF%D1%80%D0%BE-boot-md)  Докладно про boot-md

Запускає `BOOT.md` з активної робочої теки під час запуску gateway.

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%85%D1%83%D0%BA%D0%B8-%D0%BF%D0%BB%D0%B0%D0%B3%D1%96%D0%BD%D1%96%D0%B2)  Хуки плагінів

Плагіни можуть реєструвати типізовані хуки через Plugin SDK для глибшої інтеграції:
перехоплення викликів інструментів, зміни промптів, керування потоком повідомлень тощо.
Використовуйте хуки плагінів, коли вам потрібні `before_tool_call`, `before_agent_reply`,
`before_install` або інші внутрішньопроцесні хуки життєвого циклу.Повний довідник щодо хуків плагінів дивіться в [Plugin hooks](https://docs.openclaw.ai/uk/plugins/hooks).

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%BA%D0%BE%D0%BD%D1%84%D1%96%D0%B3%D1%83%D1%80%D0%B0%D1%86%D1%96%D1%8F)  Конфігурація

```
{
  "hooks": {
    "internal": {
      "enabled": true,
      "entries": {
        "session-memory": { "enabled": true },
        "command-logger": { "enabled": false }
      }
    }
  }
}
```

Змінні середовища для окремих хуків:

```
{
  "hooks": {
    "internal": {
      "entries": {
        "my-hook": {
          "enabled": true,
          "env": { "MY_CUSTOM_VAR": "value" }
        }
      }
    }
  }
}
```

Додаткові каталоги хуків:

```
{
  "hooks": {
    "internal": {
      "load": {
        "extraDirs": ["/path/to/more/hooks"]
      }
    }
  }
}
```

Застарілий формат конфігурації масиву `hooks.internal.handlers` усе ще підтримується для зворотної сумісності, але нові хуки мають використовувати систему на основі виявлення.

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%B4%D0%BE%D0%B2%D1%96%D0%B4%D0%BD%D0%B8%D0%BA-cli)  Довідник CLI

```
# Перелічити всі хуки (додайте --eligible, --verbose або --json)
openclaw hooks list

# Показати детальну інформацію про хук
openclaw hooks info <hook-name>

# Показати зведення придатності
openclaw hooks check

# Увімкнути/вимкнути
openclaw hooks enable <hook-name>
openclaw hooks disable <hook-name>
```

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%80%D0%B5%D0%BA%D0%BE%D0%BC%D0%B5%D0%BD%D0%B4%D0%BE%D0%B2%D0%B0%D0%BD%D1%96-%D0%BF%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D0%BA%D0%B8)  Рекомендовані практики

- **Робіть обробники швидкими.** Хуки запускаються під час обробки команд. Запускайте важку роботу у фоновому режимі без очікування через `void processInBackground(event)`.
- **Коректно обробляйте помилки.** Обгорніть ризиковані операції в try/catch; не викидайте помилки, щоб інші обробники могли виконатися.
- **Фільтруйте події рано.** Одразу повертайтеся, якщо тип/дія події не є релевантними.
- **Використовуйте конкретні ключі подій.** Віддавайте перевагу `"events": ["command:new"]` замість `"events": ["command"]`, щоб зменшити накладні витрати.

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%83%D1%81%D1%83%D0%BD%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BD%D0%B5%D1%81%D0%BF%D1%80%D0%B0%D0%B2%D0%BD%D0%BE%D1%81%D1%82%D0%B5%D0%B9)  Усунення несправностей

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%85%D1%83%D0%BA-%D0%BD%D0%B5-%D0%B2%D0%B8%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%BE)  Хук не виявлено

```
# Перевірити структуру каталогу
ls -la ~/.openclaw/hooks/my-hook/
# Має показати: HOOK.md, handler.ts

# Перелічити всі виявлені хуки
openclaw hooks list
```

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%85%D1%83%D0%BA-%D0%BD%D0%B5-%D0%BF%D1%80%D0%B8%D0%B4%D0%B0%D1%82%D0%BD%D0%B8%D0%B9)  Хук не придатний

```
openclaw hooks info my-hook
```

Перевірте, чи не бракує бінарних файлів (PATH), змінних середовища, значень конфігурації або сумісності з ОС.

### [​](https://docs.openclaw.ai/uk/automation/hooks\#%D1%85%D1%83%D0%BA-%D0%BD%D0%B5-%D0%B2%D0%B8%D0%BA%D0%BE%D0%BD%D1%83%D1%94%D1%82%D1%8C%D1%81%D1%8F)  Хук не виконується

1. Переконайтеся, що хук увімкнено: `openclaw hooks list`
2. Перезапустіть процес gateway, щоб хуки перезавантажилися.
3. Перевірте журнали gateway: `./scripts/clawlog.sh | grep hook`

## [​](https://docs.openclaw.ai/uk/automation/hooks\#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)  Пов’язане

- [Довідник CLI: hooks](https://docs.openclaw.ai/uk/cli/hooks)
- [Webhooks](https://docs.openclaw.ai/uk/automation/cron-jobs#webhooks)
- [Plugin hooks](https://docs.openclaw.ai/uk/plugins/hooks) — внутрішньопроцесні хуки життєвого циклу плагіна
- [Конфігурація](https://docs.openclaw.ai/uk/gateway/configuration-reference#hooks)

[Постійні вказівки](https://docs.openclaw.ai/uk/automation/standing-orders) [Інструмент apply\_patch](https://docs.openclaw.ai/uk/tools/apply-patch)

Ctrl+I