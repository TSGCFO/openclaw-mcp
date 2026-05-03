---
source_url: https://docs.openclaw.ai/es/providers/openrouter
title: "OpenRouter - OpenClaw"
---

[Saltar al contenido principal](https://docs.openclaw.ai/es/providers/openrouter#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/es)

![ES](https://d3gk2c5xim1je2.cloudfront.net/flags/ES.svg)

Español

Buscar...

Ctrl K

Buscar...

Navigation

Providers

OpenRouter

[Get started](https://docs.openclaw.ai/es) [Install](https://docs.openclaw.ai/es/install) [Channels](https://docs.openclaw.ai/es/channels) [Agents](https://docs.openclaw.ai/es/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/es/tools) [Models](https://docs.openclaw.ai/es/providers) [Platforms](https://docs.openclaw.ai/es/platforms) [Gateway & Ops](https://docs.openclaw.ai/es/gateway) [Reference](https://docs.openclaw.ai/es/cli) [Help](https://docs.openclaw.ai/es/help)

En esta página

- [Primeros pasos](https://docs.openclaw.ai/es/providers/openrouter#primeros-pasos)
- [Ejemplo de configuración](https://docs.openclaw.ai/es/providers/openrouter#ejemplo-de-configuraci%C3%B3n)
- [Referencias de modelos](https://docs.openclaw.ai/es/providers/openrouter#referencias-de-modelos)
- [Generación de imágenes](https://docs.openclaw.ai/es/providers/openrouter#generaci%C3%B3n-de-im%C3%A1genes)
- [Generación de video](https://docs.openclaw.ai/es/providers/openrouter#generaci%C3%B3n-de-video)
- [Texto a voz](https://docs.openclaw.ai/es/providers/openrouter#texto-a-voz)
- [Autenticación y encabezados](https://docs.openclaw.ai/es/providers/openrouter#autenticaci%C3%B3n-y-encabezados)
- [Configuración avanzada](https://docs.openclaw.ai/es/providers/openrouter#configuraci%C3%B3n-avanzada)
- [Relacionado](https://docs.openclaw.ai/es/providers/openrouter#relacionado)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenRouter proporciona una **API unificada** que enruta solicitudes a muchos modelos detrás de un único
punto de conexión y clave de API. Es compatible con OpenAI, por lo que la mayoría de los SDK de OpenAI funcionan cambiando la URL base.

## [​](https://docs.openclaw.ai/es/providers/openrouter\#primeros-pasos)  Primeros pasos

1

[Navigate to header](https://docs.openclaw.ai/es/providers/openrouter#)

Obtén tu clave de API

Crea una clave de API en [openrouter.ai/keys](https://openrouter.ai/keys).

2

[Navigate to header](https://docs.openclaw.ai/es/providers/openrouter#)

Ejecuta la incorporación

```
openclaw onboard --auth-choice openrouter-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/es/providers/openrouter#)

(Opcional) Cambia a un modelo específico

La incorporación usa `openrouter/auto` de forma predeterminada. Elige un modelo concreto más adelante:

```
openclaw models set openrouter/<provider>/<model>
```

## [​](https://docs.openclaw.ai/es/providers/openrouter\#ejemplo-de-configuraci%C3%B3n)  Ejemplo de configuración

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      model: { primary: "openrouter/auto" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/es/providers/openrouter\#referencias-de-modelos)  Referencias de modelos

Las referencias de modelo siguen el patrón `openrouter/<provider>/<model>`. Para ver la lista completa de
proveedores y modelos disponibles, consulta [/concepts/model-providers](https://docs.openclaw.ai/es/concepts/model-providers).

Ejemplos de respaldo incluidos:

| Referencia de modelo | Notas |
| --- | --- |
| `openrouter/auto` | Enrutamiento automático de OpenRouter |
| `openrouter/moonshotai/kimi-k2.6` | Kimi K2.6 mediante MoonshotAI |

## [​](https://docs.openclaw.ai/es/providers/openrouter\#generaci%C3%B3n-de-im%C3%A1genes)  Generación de imágenes

OpenRouter también puede respaldar la herramienta `image_generate`. Usa un modelo de imagen de OpenRouter en `agents.defaults.imageGenerationModel`:

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "openrouter/google/gemini-3.1-flash-image-preview",
        timeoutMs: 180_000,
      },
    },
  },
}
```

OpenClaw envía solicitudes de imagen a la API de imágenes de finalizaciones de chat de OpenRouter con `modalities: ["image", "text"]`. Los modelos de imagen de Gemini reciben indicaciones compatibles de `aspectRatio` y `resolution` mediante `image_config` de OpenRouter. Usa `agents.defaults.imageGenerationModel.timeoutMs` para los modelos de imagen de OpenRouter más lentos; el parámetro `timeoutMs` por llamada de la herramienta `image_generate` sigue teniendo prioridad.

## [​](https://docs.openclaw.ai/es/providers/openrouter\#generaci%C3%B3n-de-video)  Generación de video

OpenRouter también puede respaldar la herramienta `video_generate` mediante su API asíncrona `/videos`. Usa un modelo de video de OpenRouter en `agents.defaults.videoGenerationModel`:

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "openrouter/google/veo-3.1-fast",
      },
    },
  },
}
```

OpenClaw envía trabajos de texto a video y de imagen a video a OpenRouter, sondea
el `polling_url` devuelto y descarga el video completado desde
los `unsigned_urls` de OpenRouter o desde el punto de conexión documentado de contenido del trabajo.
Las imágenes de referencia se envían como imágenes de primer/último fotograma de forma predeterminada; las imágenes
etiquetadas con `reference_image` se envían como referencias de entrada de OpenRouter. El valor predeterminado
incluido `google/veo-3.1-fast` anuncia las duraciones de 4/6/8
segundos actualmente compatibles, las resoluciones `720P`/`1080P` y las relaciones de aspecto
`16:9`/`9:16`. Video a video no está registrado para OpenRouter porque la API
ascendente de generación de video actualmente acepta texto y referencias de imagen.

## [​](https://docs.openclaw.ai/es/providers/openrouter\#texto-a-voz)  Texto a voz

OpenRouter también se puede usar como proveedor de TTS mediante su punto de conexión
`/audio/speech` compatible con OpenAI.

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "openrouter",
      providers: {
        openrouter: {
          model: "hexgrad/kokoro-82m",
          voice: "af_alloy",
          responseFormat: "mp3",
        },
      },
    },
  },
}
```

Si se omite `messages.tts.providers.openrouter.apiKey`, TTS reutiliza
`models.providers.openrouter.apiKey` y luego `OPENROUTER_API_KEY`.

## [​](https://docs.openclaw.ai/es/providers/openrouter\#autenticaci%C3%B3n-y-encabezados)  Autenticación y encabezados

OpenRouter usa internamente un token Bearer con tu clave de API.En solicitudes reales de OpenRouter (`https://openrouter.ai/api/v1`), OpenClaw también añade
los encabezados documentados de atribución de aplicación de OpenRouter:

| Encabezado | Valor |
| --- | --- |
| `HTTP-Referer` | `https://openclaw.ai` |
| `X-OpenRouter-Title` | `OpenClaw` |
| `X-OpenRouter-Categories` | `cli-agent` |

Si rediriges el proveedor OpenRouter a otro proxy o URL base, OpenClaw
**no** inyecta esos encabezados específicos de OpenRouter ni marcadores de caché de Anthropic.

## [​](https://docs.openclaw.ai/es/providers/openrouter\#configuraci%C3%B3n-avanzada)  Configuración avanzada

Marcadores de caché de Anthropic

En rutas verificadas de OpenRouter, las referencias de modelos de Anthropic conservan los
marcadores `cache_control` de Anthropic específicos de OpenRouter que OpenClaw usa para
reutilizar mejor la caché de prompts en bloques de prompt del sistema/desarrollador.

Inyección de pensamiento / razonamiento

En rutas compatibles que no son `auto`, OpenClaw asigna el nivel de pensamiento seleccionado a
cargas útiles de razonamiento del proxy de OpenRouter. Las indicaciones de modelos no compatibles y
`openrouter/auto` omiten esa inyección de razonamiento. Hunter Alpha también omite
el razonamiento del proxy para referencias de modelos configuradas obsoletas porque OpenRouter podría
devolver texto de respuesta final en campos de razonamiento para esa ruta retirada.

Moldeado de solicitudes solo para OpenAI

OpenRouter sigue pasando por la ruta compatible con OpenAI de estilo proxy, por lo que
no se reenvía el moldeado de solicitudes nativo solo de OpenAI, como `serviceTier`, `store` de Responses,
cargas útiles de compatibilidad de razonamiento de OpenAI e indicaciones de caché de prompts.

Rutas respaldadas por Gemini

Las referencias de OpenRouter respaldadas por Gemini permanecen en la ruta proxy-Gemini: OpenClaw mantiene
allí el saneamiento de firmas de pensamiento de Gemini, pero no habilita la validación nativa de reproducción de Gemini
ni reescrituras de arranque.

Metadatos de enrutamiento del proveedor

Si pasas enrutamiento de proveedor de OpenRouter en parámetros de modelo, OpenClaw lo reenvía
como metadatos de enrutamiento de OpenRouter antes de que se ejecuten los envoltorios de flujo compartidos.

## [​](https://docs.openclaw.ai/es/providers/openrouter\#relacionado)  Relacionado

[**Selección de modelos** \\
\\
Elegir proveedores, referencias de modelos y comportamiento de conmutación por error.](https://docs.openclaw.ai/es/concepts/model-providers)

[**Referencia de configuración** \\
\\
Referencia completa de configuración para agentes, modelos y proveedores.](https://docs.openclaw.ai/es/gateway/configuration-reference)

[OpenCode Go](https://docs.openclaw.ai/es/providers/opencode-go) [Perplexity](https://docs.openclaw.ai/es/providers/perplexity-provider)

Ctrl+I