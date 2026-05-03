---
source_url: https://docs.openclaw.ai/pl/cli/tasks
title: "`openclaw tasks` - OpenClaw"
---

[Przejdź do głównej treści](https://docs.openclaw.ai/pl/cli/tasks#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/pl)

![PL](https://d3gk2c5xim1je2.cloudfront.net/flags/PL.svg)

Polski

Szukaj...

Ctrl K

Szukaj...

Navigation

Agents and sessions

\`openclaw tasks\`

[Get started](https://docs.openclaw.ai/pl) [Install](https://docs.openclaw.ai/pl/install) [Channels](https://docs.openclaw.ai/pl/channels) [Agents](https://docs.openclaw.ai/pl/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pl/tools) [Models](https://docs.openclaw.ai/pl/providers) [Platforms](https://docs.openclaw.ai/pl/platforms) [Gateway & Ops](https://docs.openclaw.ai/pl/gateway) [Reference](https://docs.openclaw.ai/pl/cli) [Help](https://docs.openclaw.ai/pl/help)

Na tej stronie

- [Użycie](https://docs.openclaw.ai/pl/cli/tasks#u%C5%BCycie)
- [Opcje główne](https://docs.openclaw.ai/pl/cli/tasks#opcje-g%C5%82%C3%B3wne)
- [Podpolecenia](https://docs.openclaw.ai/pl/cli/tasks#podpolecenia)
- [list](https://docs.openclaw.ai/pl/cli/tasks#list)
- [show](https://docs.openclaw.ai/pl/cli/tasks#show)
- [notify](https://docs.openclaw.ai/pl/cli/tasks#notify)
- [cancel](https://docs.openclaw.ai/pl/cli/tasks#cancel)
- [audit](https://docs.openclaw.ai/pl/cli/tasks#audit)
- [maintenance](https://docs.openclaw.ai/pl/cli/tasks#maintenance)
- [flow](https://docs.openclaw.ai/pl/cli/tasks#flow)
- [Powiązane](https://docs.openclaw.ai/pl/cli/tasks#powi%C4%85zane)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Sprawdzaj trwałe zadania w tle i stan TaskFlow. Bez podpolecenia
`openclaw tasks` jest równoważne z `openclaw tasks list`.Zobacz [Zadania w tle](https://docs.openclaw.ai/pl/automation/tasks), aby poznać model cyklu życia i dostarczania.

## [​](https://docs.openclaw.ai/pl/cli/tasks\#u%C5%BCycie)  Użycie

```
openclaw tasks
openclaw tasks list
openclaw tasks list --runtime acp
openclaw tasks list --status running
openclaw tasks show <lookup>
openclaw tasks notify <lookup> state_changes
openclaw tasks cancel <lookup>
openclaw tasks audit
openclaw tasks maintenance
openclaw tasks maintenance --apply
openclaw tasks flow list
openclaw tasks flow show <lookup>
openclaw tasks flow cancel <lookup>
```

## [​](https://docs.openclaw.ai/pl/cli/tasks\#opcje-g%C5%82%C3%B3wne)  Opcje główne

- `--json`: wyjście JSON.
- `--runtime <name>`: filtruj według rodzaju: `subagent`, `acp`, `cron` lub `cli`.
- `--status <name>`: filtruj według statusu: `queued`, `running`, `succeeded`, `failed`, `timed_out`, `cancelled` lub `lost`.

## [​](https://docs.openclaw.ai/pl/cli/tasks\#podpolecenia)  Podpolecenia

### [​](https://docs.openclaw.ai/pl/cli/tasks\#list)  `list`

```
openclaw tasks list [--runtime <name>] [--status <name>] [--json]
```

Wyświetla śledzone zadania w tle od najnowszych.

### [​](https://docs.openclaw.ai/pl/cli/tasks\#show)  `show`

```
openclaw tasks show <lookup> [--json]
```

Pokazuje jedno zadanie według identyfikatora zadania, identyfikatora uruchomienia lub klucza sesji.

### [​](https://docs.openclaw.ai/pl/cli/tasks\#notify)  `notify`

```
openclaw tasks notify <lookup> <done_only|state_changes|silent>
```

Zmienia politykę powiadomień dla uruchomionego zadania.

### [​](https://docs.openclaw.ai/pl/cli/tasks\#cancel)  `cancel`

```
openclaw tasks cancel <lookup>
```

Anuluje uruchomione zadanie w tle.

### [​](https://docs.openclaw.ai/pl/cli/tasks\#audit)  `audit`

```
openclaw tasks audit [--severity <warn|error>] [--code <name>] [--limit <n>] [--json]
```

Ujawnia nieaktualne, utracone, z błędami dostarczania lub w inny sposób niespójne rekordy zadań i TaskFlow. Utracone zadania zachowane do czasu `cleanupAfter` są ostrzeżeniami; wygasłe lub nieoznaczone utracone zadania są błędami.

### [​](https://docs.openclaw.ai/pl/cli/tasks\#maintenance)  `maintenance`

```
openclaw tasks maintenance [--apply] [--json]
```

Wyświetla podgląd lub stosuje uzgadnianie zadań i TaskFlow, oznaczanie czyszczenia oraz przycinanie.
W przypadku zadań Cron uzgadnianie używa utrwalonych logów uruchomień/stanu zadań przed oznaczeniem
starego aktywnego zadania jako `lost`, dzięki czemu ukończone uruchomienia Cron nie stają się fałszywymi błędami audytu
tylko dlatego, że zniknął stan działania Gateway przechowywany w pamięci. Audyt CLI offline
nie jest autorytatywny dla lokalnego w procesie zestawu aktywnych zadań Cron Gateway.

### [​](https://docs.openclaw.ai/pl/cli/tasks\#flow)  `flow`

```
openclaw tasks flow list [--status <name>] [--json]
openclaw tasks flow show <lookup> [--json]
openclaw tasks flow cancel <lookup>
```

Sprawdza lub anuluje trwały stan TaskFlow w rejestrze zadań.

## [​](https://docs.openclaw.ai/pl/cli/tasks\#powi%C4%85zane)  Powiązane

- [Dokumentacja CLI](https://docs.openclaw.ai/pl/cli)
- [Zadania w tle](https://docs.openclaw.ai/pl/automation/tasks)

[System](https://docs.openclaw.ai/pl/cli/system) [Kanały](https://docs.openclaw.ai/pl/cli/channels)

Ctrl+I