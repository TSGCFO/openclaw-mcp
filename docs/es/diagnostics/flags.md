---
source_url: https://docs.openclaw.ai/es/diagnostics/flags
title: "Opciones de diagn\u00f3stico - OpenClaw"
---

[Saltar al contenido principal](https://docs.openclaw.ai/es/diagnostics/flags#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/es)

![ES](https://d3gk2c5xim1je2.cloudfront.net/flags/ES.svg)

Español

Buscar...

Ctrl K

Buscar...

Navigation

Diagnostics

Opciones de diagnóstico

[Get started](https://docs.openclaw.ai/es) [Install](https://docs.openclaw.ai/es/install) [Channels](https://docs.openclaw.ai/es/channels) [Agents](https://docs.openclaw.ai/es/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/es/tools) [Models](https://docs.openclaw.ai/es/providers) [Platforms](https://docs.openclaw.ai/es/platforms) [Gateway & Ops](https://docs.openclaw.ai/es/gateway) [Reference](https://docs.openclaw.ai/es/cli) [Help](https://docs.openclaw.ai/es/help)

En esta página

- [Cómo funciona](https://docs.openclaw.ai/es/diagnostics/flags#c%C3%B3mo-funciona)
- [Activar mediante configuración](https://docs.openclaw.ai/es/diagnostics/flags#activar-mediante-configuraci%C3%B3n)
- [Anulación de entorno (puntual)](https://docs.openclaw.ai/es/diagnostics/flags#anulaci%C3%B3n-de-entorno-puntual)
- [Artefactos de línea de tiempo](https://docs.openclaw.ai/es/diagnostics/flags#artefactos-de-l%C3%ADnea-de-tiempo)
- [Dónde van los registros](https://docs.openclaw.ai/es/diagnostics/flags#d%C3%B3nde-van-los-registros)
- [Extraer registros](https://docs.openclaw.ai/es/diagnostics/flags#extraer-registros)
- [Notas](https://docs.openclaw.ai/es/diagnostics/flags#notas)
- [Relacionado](https://docs.openclaw.ai/es/diagnostics/flags#relacionado)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Los indicadores de diagnóstico te permiten activar registros de depuración específicos sin habilitar el registro detallado en todas partes. Los indicadores son opcionales y no tienen efecto a menos que un subsistema los compruebe.

## [​](https://docs.openclaw.ai/es/diagnostics/flags\#c%C3%B3mo-funciona)  Cómo funciona

- Los indicadores son cadenas (sin distinción entre mayúsculas y minúsculas).
- Puedes activar indicadores en la configuración o mediante una anulación de entorno.
- Se admiten comodines:
  - `telegram.*` coincide con `telegram.http`
  - `*` activa todos los indicadores

## [​](https://docs.openclaw.ai/es/diagnostics/flags\#activar-mediante-configuraci%C3%B3n)  Activar mediante configuración

```
{
  "diagnostics": {
    "flags": ["telegram.http"]
  }
}
```

Varios indicadores:

```
{
  "diagnostics": {
    "flags": ["telegram.http", "brave.http", "gateway.*"]
  }
}
```

Reinicia el Gateway después de cambiar los indicadores.

## [​](https://docs.openclaw.ai/es/diagnostics/flags\#anulaci%C3%B3n-de-entorno-puntual)  Anulación de entorno (puntual)

```
OPENCLAW_DIAGNOSTICS=telegram.http,telegram.payload
```

Desactivar todos los indicadores:

```
OPENCLAW_DIAGNOSTICS=0
```

## [​](https://docs.openclaw.ai/es/diagnostics/flags\#artefactos-de-l%C3%ADnea-de-tiempo)  Artefactos de línea de tiempo

El indicador `timeline` escribe eventos estructurados de arranque y temporización
en tiempo de ejecución para arneses de QA externos:

```
OPENCLAW_DIAGNOSTICS=timeline \
OPENCLAW_DIAGNOSTICS_TIMELINE_PATH=/tmp/openclaw-timeline.jsonl \
openclaw gateway run
```

También puedes activarlo en la configuración:

```
{
  "diagnostics": {
    "flags": ["timeline"]
  }
}
```

La ruta del archivo de línea de tiempo sigue viniendo de
`OPENCLAW_DIAGNOSTICS_TIMELINE_PATH`. Cuando `timeline` se activa solo desde la
configuración, los primeros intervalos de carga de configuración no se emiten
porque OpenClaw aún no ha leído la configuración; los intervalos de arranque
posteriores usan el indicador de configuración.`OPENCLAW_DIAGNOSTICS=1`, `OPENCLAW_DIAGNOSTICS=all` y
`OPENCLAW_DIAGNOSTICS=*` también activan la línea de tiempo porque activan todos
los indicadores de diagnóstico. Prefiere `timeline` cuando solo quieras el
artefacto de temporización JSONL.Los registros de línea de tiempo usan el contenedor `openclaw.diagnostics.v1`.
Los eventos pueden incluir identificadores de proceso, nombres de fase, nombres
de intervalo, duraciones, identificadores de Plugin, recuentos de dependencias,
muestras de retraso del bucle de eventos, nombres de operaciones de proveedor,
estado de salida de procesos secundarios y nombres/mensajes de errores de
arranque. Trata los archivos de línea de tiempo como artefactos locales de
diagnóstico; revísalos antes de compartirlos fuera de tu máquina.

## [​](https://docs.openclaw.ai/es/diagnostics/flags\#d%C3%B3nde-van-los-registros)  Dónde van los registros

Los indicadores emiten registros en el archivo estándar de registro de diagnóstico. De forma predeterminada:

```
/tmp/openclaw/openclaw-YYYY-MM-DD.log
```

Si defines `logging.file`, usa esa ruta en su lugar. Los registros son JSONL (un objeto JSON por línea). La censura sigue aplicándose según `logging.redactSensitive`.

## [​](https://docs.openclaw.ai/es/diagnostics/flags\#extraer-registros)  Extraer registros

Elige el archivo de registro más reciente:

```
ls -t /tmp/openclaw/openclaw-*.log | head -n 1
```

Filtrar diagnósticos HTTP de Telegram:

```
rg "telegram http error" /tmp/openclaw/openclaw-*.log
```

Filtrar diagnósticos HTTP de Brave Search:

```
rg "brave http" /tmp/openclaw/openclaw-*.log
```

O sigue el registro mientras reproduces el problema:

```
tail -f /tmp/openclaw/openclaw-$(date +%F).log | rg "telegram http error"
```

Para Gateways remotos, también puedes usar `openclaw logs --follow` (consulta [/cli/logs](https://docs.openclaw.ai/es/cli/logs)).

## [​](https://docs.openclaw.ai/es/diagnostics/flags\#notas)  Notas

- Si `logging.level` está configurado por encima de `warn`, estos registros pueden suprimirse. El valor predeterminado `info` funciona bien.
- `brave.http` registra URL/parámetros de consulta de solicitudes de Brave Search, estado/temporización de respuestas y eventos de acierto/fallo/escritura de caché. No registra claves de API ni cuerpos de respuesta, pero las consultas de búsqueda pueden ser sensibles.
- Es seguro dejar los indicadores activados; solo afectan al volumen de registro del subsistema específico.
- Usa [/logging](https://docs.openclaw.ai/es/logging) para cambiar destinos, niveles y censura de registros.

## [​](https://docs.openclaw.ai/es/diagnostics/flags\#relacionado)  Relacionado

- [Diagnósticos del Gateway](https://docs.openclaw.ai/es/gateway/diagnostics)
- [Solución de problemas del Gateway](https://docs.openclaw.ai/es/gateway/troubleshooting)

[Variables de entorno](https://docs.openclaw.ai/es/help/environment) [Fallo de Node + tsx](https://docs.openclaw.ai/es/debug/node-issue)

Ctrl+I