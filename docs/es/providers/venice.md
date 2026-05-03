---
source_url: https://docs.openclaw.ai/es/providers/venice
title: "Venice AI - OpenClaw"
---

[Saltar al contenido principal](https://docs.openclaw.ai/es/providers/venice#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/es)

![ES](https://d3gk2c5xim1je2.cloudfront.net/flags/ES.svg)

Español

Buscar...

Ctrl K

Buscar...

Navigation

Providers

Venice AI

[Get started](https://docs.openclaw.ai/es) [Install](https://docs.openclaw.ai/es/install) [Channels](https://docs.openclaw.ai/es/channels) [Agents](https://docs.openclaw.ai/es/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/es/tools) [Models](https://docs.openclaw.ai/es/providers) [Platforms](https://docs.openclaw.ai/es/platforms) [Gateway & Ops](https://docs.openclaw.ai/es/gateway) [Reference](https://docs.openclaw.ai/es/cli) [Help](https://docs.openclaw.ai/es/help)

En esta página

- [Por qué Venice en OpenClaw](https://docs.openclaw.ai/es/providers/venice#por-qu%C3%A9-venice-en-openclaw)
- [Modos de privacidad](https://docs.openclaw.ai/es/providers/venice#modos-de-privacidad)
- [Funciones](https://docs.openclaw.ai/es/providers/venice#funciones)
- [Primeros pasos](https://docs.openclaw.ai/es/providers/venice#primeros-pasos)
- [Selección de modelos](https://docs.openclaw.ai/es/providers/venice#selecci%C3%B3n-de-modelos)
- [Comportamiento de reproducción de DeepSeek V4](https://docs.openclaw.ai/es/providers/venice#comportamiento-de-reproducci%C3%B3n-de-deepseek-v4)
- [Catálogo integrado (41 en total)](https://docs.openclaw.ai/es/providers/venice#cat%C3%A1logo-integrado-41-en-total)
- [Descubrimiento de modelos](https://docs.openclaw.ai/es/providers/venice#descubrimiento-de-modelos)
- [Compatibilidad con transmisión y herramientas](https://docs.openclaw.ai/es/providers/venice#compatibilidad-con-transmisi%C3%B3n-y-herramientas)
- [Precios](https://docs.openclaw.ai/es/providers/venice#precios)
- [Venice (anonimizado) frente a API directa](https://docs.openclaw.ai/es/providers/venice#venice-anonimizado-frente-a-api-directa)
- [Ejemplos de uso](https://docs.openclaw.ai/es/providers/venice#ejemplos-de-uso)
- [Solución de problemas](https://docs.openclaw.ai/es/providers/venice#soluci%C3%B3n-de-problemas)
- [Configuración avanzada](https://docs.openclaw.ai/es/providers/venice#configuraci%C3%B3n-avanzada)
- [Relacionado](https://docs.openclaw.ai/es/providers/venice#relacionado)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Venice AI proporciona **inferencia de IA centrada en la privacidad** con compatibilidad con modelos sin censura y acceso a modelos propietarios principales a través de su proxy anonimizado. Toda la inferencia es privada de forma predeterminada: sin entrenamiento con tus datos, sin registro.

## [​](https://docs.openclaw.ai/es/providers/venice\#por-qu%C3%A9-venice-en-openclaw)  Por qué Venice en OpenClaw

- **Inferencia privada** para modelos de código abierto (sin registro).
- **Modelos sin censura** cuando los necesites.
- **Acceso anonimizado** a modelos propietarios (Opus/GPT/Gemini) cuando la calidad importa.
- Endpoints `/v1` compatibles con OpenAI.

## [​](https://docs.openclaw.ai/es/providers/venice\#modos-de-privacidad)  Modos de privacidad

Venice ofrece dos niveles de privacidad; entender esto es clave para elegir tu modelo:

| Modo | Descripción | Modelos |
| --- | --- | --- |
| **Privado** | Totalmente privado. Los prompts/respuestas **nunca se almacenan ni se registran**. Efímero. | Llama, Qwen, DeepSeek, Kimi, MiniMax, Venice Uncensored, etc. |
| **Anonimizado** | Enrutado a través de Venice con los metadatos eliminados. El proveedor subyacente (OpenAI, Anthropic, Google, xAI) ve solicitudes anonimizadas. | Claude, GPT, Gemini, Grok |

Los modelos anonimizados **no** son totalmente privados. Venice elimina los metadatos antes de reenviar, pero el proveedor subyacente (OpenAI, Anthropic, Google, xAI) sigue procesando la solicitud. Elige modelos **Privados** cuando se requiera privacidad total.

## [​](https://docs.openclaw.ai/es/providers/venice\#funciones)  Funciones

- **Centrado en la privacidad**: elige entre modos “privado” (totalmente privado) y “anonimizado” (enrutado por proxy)
- **Modelos sin censura**: acceso a modelos sin restricciones de contenido
- **Acceso a modelos principales**: usa Claude, GPT, Gemini y Grok mediante el proxy anonimizado de Venice
- **API compatible con OpenAI**: endpoints `/v1` estándar para una integración sencilla
- **Transmisión**: compatible con todos los modelos
- **Llamada a funciones**: compatible con modelos seleccionados (comprueba las capacidades del modelo)
- **Visión**: compatible con modelos con capacidad de visión
- **Sin límites de tasa estrictos**: puede aplicarse limitación por uso justo en casos de uso extremo

## [​](https://docs.openclaw.ai/es/providers/venice\#primeros-pasos)  Primeros pasos

1

[Navigate to header](https://docs.openclaw.ai/es/providers/venice#)

Obtén tu clave de API

1. Regístrate en [venice.ai](https://venice.ai/)
2. Ve a **Settings > API Keys > Create new key**
3. Copia tu clave de API (formato: `vapi_xxxxxxxxxxxx`)

2

[Navigate to header](https://docs.openclaw.ai/es/providers/venice#)

Configura OpenClaw

Elige tu método de configuración preferido:

- Interactivo (recomendado)

- Variable de entorno

- No interactivo


```
openclaw onboard --auth-choice venice-api-key
```

Esto hará lo siguiente:

1. Solicitar tu clave de API (o usar `VENICE_API_KEY` existente)
2. Mostrar todos los modelos Venice disponibles
3. Permitirte elegir tu modelo predeterminado
4. Configurar el proveedor automáticamente

```
export VENICE_API_KEY="vapi_xxxxxxxxxxxx"
```

```
openclaw onboard --non-interactive \
  --auth-choice venice-api-key \
  --venice-api-key "vapi_xxxxxxxxxxxx"
```

3

[Navigate to header](https://docs.openclaw.ai/es/providers/venice#)

Verifica la configuración

```
openclaw agent --model venice/kimi-k2-5 --message "Hello, are you working?"
```

## [​](https://docs.openclaw.ai/es/providers/venice\#selecci%C3%B3n-de-modelos)  Selección de modelos

Después de la configuración, OpenClaw muestra todos los modelos Venice disponibles. Elige según tus necesidades:

- **Modelo predeterminado**: `venice/kimi-k2-5` para razonamiento privado sólido con visión.
- **Opción de alta capacidad**: `venice/claude-opus-4-6` para la ruta anonimizada de Venice más potente.
- **Privacidad**: elige modelos “privados” para inferencia totalmente privada.
- **Capacidad**: elige modelos “anonimizados” para acceder a Claude, GPT, Gemini mediante el proxy de Venice.

Cambia tu modelo predeterminado en cualquier momento:

```
openclaw models set venice/kimi-k2-5
openclaw models set venice/claude-opus-4-6
```

Enumera todos los modelos disponibles:

```
openclaw models list --all --provider venice
```

También puedes ejecutar `openclaw configure`, seleccionar **Model/auth** y elegir **Venice AI**.

Usa la tabla siguiente para elegir el modelo adecuado para tu caso de uso.

| Caso de uso | Modelo recomendado | Por qué |
| --- | --- | --- |
| **Chat general (predeterminado)** | `kimi-k2-5` | Razonamiento privado sólido con visión |
| **Mejor calidad general** | `claude-opus-4-6` | Opción anonimizada de Venice más potente |
| **Privacidad + programación** | `qwen3-coder-480b-a35b-instruct` | Modelo privado de programación con gran contexto |
| **Visión privada** | `kimi-k2-5` | Compatibilidad con visión sin salir del modo privado |
| **Rápido + barato** | `qwen3-4b` | Modelo ligero de razonamiento |
| **Tareas privadas complejas** | `deepseek-v3.2` | Razonamiento sólido, pero sin compatibilidad con herramientas de Venice |
| **Sin censura** | `venice-uncensored` | Sin restricciones de contenido |

## [​](https://docs.openclaw.ai/es/providers/venice\#comportamiento-de-reproducci%C3%B3n-de-deepseek-v4)  Comportamiento de reproducción de DeepSeek V4

Si Venice expone modelos DeepSeek V4 como `venice/deepseek-v4-pro` o
`venice/deepseek-v4-flash`, OpenClaw completa el marcador de posición de reproducción
`reasoning_content` requerido por DeepSeek V4 en los mensajes del asistente cuando el proxy
lo omite. Venice rechaza el control `thinking` nativo de nivel superior de DeepSeek, por lo que
OpenClaw mantiene esa corrección de reproducción específica del proveedor separada de los controles
de pensamiento del proveedor DeepSeek nativo.

## [​](https://docs.openclaw.ai/es/providers/venice\#cat%C3%A1logo-integrado-41-en-total)  Catálogo integrado (41 en total)

Modelos privados (26): totalmente privados, sin registro

| ID de modelo | Nombre | Contexto | Funciones |
| --- | --- | --- | --- |
| `kimi-k2-5` | Kimi K2.5 | 256k | Predeterminado, razonamiento, visión |
| `kimi-k2-thinking` | Kimi K2 Thinking | 256k | Razonamiento |
| `llama-3.3-70b` | Llama 3.3 70B | 128k | General |
| `llama-3.2-3b` | Llama 3.2 3B | 128k | General |
| `hermes-3-llama-3.1-405b` | Hermes 3 Llama 3.1 405B | 128k | General, herramientas desactivadas |
| `qwen3-235b-a22b-thinking-2507` | Qwen3 235B Thinking | 128k | Razonamiento |
| `qwen3-235b-a22b-instruct-2507` | Qwen3 235B Instruct | 128k | General |
| `qwen3-coder-480b-a35b-instruct` | Qwen3 Coder 480B | 256k | Programación |
| `qwen3-coder-480b-a35b-instruct-turbo` | Qwen3 Coder 480B Turbo | 256k | Programación |
| `qwen3-5-35b-a3b` | Qwen3.5 35B A3B | 256k | Razonamiento, visión |
| `qwen3-next-80b` | Qwen3 Next 80B | 256k | General |
| `qwen3-vl-235b-a22b` | Qwen3 VL 235B (Vision) | 256k | Visión |
| `qwen3-4b` | Venice Small (Qwen3 4B) | 32k | Rápido, razonamiento |
| `deepseek-v3.2` | DeepSeek V3.2 | 160k | Razonamiento, herramientas desactivadas |
| `venice-uncensored` | Venice Uncensored (Dolphin-Mistral) | 32k | Sin censura, herramientas desactivadas |
| `mistral-31-24b` | Venice Medium (Mistral) | 128k | Visión |
| `google-gemma-3-27b-it` | Google Gemma 3 27B Instruct | 198k | Visión |
| `openai-gpt-oss-120b` | OpenAI GPT OSS 120B | 128k | General |
| `nvidia-nemotron-3-nano-30b-a3b` | NVIDIA Nemotron 3 Nano 30B | 128k | General |
| `olafangensan-glm-4.7-flash-heretic` | GLM 4.7 Flash Heretic | 128k | Razonamiento |
| `zai-org-glm-4.6` | GLM 4.6 | 198k | General |
| `zai-org-glm-4.7` | GLM 4.7 | 198k | Razonamiento |
| `zai-org-glm-4.7-flash` | GLM 4.7 Flash | 128k | Razonamiento |
| `zai-org-glm-5` | GLM 5 | 198k | Razonamiento |
| `minimax-m21` | MiniMax M2.1 | 198k | Razonamiento |
| `minimax-m25` | MiniMax M2.5 | 198k | Razonamiento |

Modelos anonimizados (15): mediante el proxy de Venice

| ID de modelo | Nombre | Contexto | Funciones |
| --- | --- | --- | --- |
| `claude-opus-4-6` | Claude Opus 4.6 (a través de Venice) | 1M | Razonamiento, visión |
| `claude-opus-4-5` | Claude Opus 4.5 (a través de Venice) | 198k | Razonamiento, visión |
| `claude-sonnet-4-6` | Claude Sonnet 4.6 (a través de Venice) | 1M | Razonamiento, visión |
| `claude-sonnet-4-5` | Claude Sonnet 4.5 (a través de Venice) | 198k | Razonamiento, visión |
| `openai-gpt-54` | GPT-5.4 (a través de Venice) | 1M | Razonamiento, visión |
| `openai-gpt-53-codex` | GPT-5.3 Codex (a través de Venice) | 400k | Razonamiento, visión, programación |
| `openai-gpt-52` | GPT-5.2 (a través de Venice) | 256k | Razonamiento |
| `openai-gpt-52-codex` | GPT-5.2 Codex (a través de Venice) | 256k | Razonamiento, visión, programación |
| `openai-gpt-4o-2024-11-20` | GPT-4o (a través de Venice) | 128k | Visión |
| `openai-gpt-4o-mini-2024-07-18` | GPT-4o Mini (a través de Venice) | 128k | Visión |
| `gemini-3-1-pro-preview` | Gemini 3.1 Pro (a través de Venice) | 1M | Razonamiento, visión |
| `gemini-3-pro-preview` | Gemini 3 Pro (a través de Venice) | 198k | Razonamiento, visión |
| `gemini-3-flash-preview` | Gemini 3 Flash (a través de Venice) | 256k | Razonamiento, visión |
| `grok-41-fast` | Grok 4.1 Fast (a través de Venice) | 1M | Razonamiento, visión |
| `grok-code-fast-1` | Grok Code Fast 1 (a través de Venice) | 256k | Razonamiento, programación |

## [​](https://docs.openclaw.ai/es/providers/venice\#descubrimiento-de-modelos)  Descubrimiento de modelos

OpenClaw incluye un catálogo semilla de Venice respaldado por manifiesto para el listado de modelos de solo lectura. La actualización en tiempo de ejecución aún puede descubrir modelos desde la API de Venice y recurre al catálogo del manifiesto si no se puede acceder a la API.El endpoint `/models` es público (no se necesita autenticación para listar), pero la inferencia requiere una clave de API válida.

## [​](https://docs.openclaw.ai/es/providers/venice\#compatibilidad-con-transmisi%C3%B3n-y-herramientas)  Compatibilidad con transmisión y herramientas

| Función | Compatibilidad |
| --- | --- |
| **Streaming** | Todos los modelos |
| **Llamadas a funciones** | La mayoría de los modelos (consulta `supportsFunctionCalling` en la API) |
| **Visión/Imágenes** | Modelos marcados con la función “Visión” |
| **Modo JSON** | Compatible mediante `response_format` |

## [​](https://docs.openclaw.ai/es/providers/venice\#precios)  Precios

Venice usa un sistema basado en créditos. Consulta [venice.ai/pricing](https://venice.ai/pricing) para ver las tarifas actuales:

- **Modelos privados**: Generalmente de menor costo
- **Modelos anonimizados**: Similar al precio directo de la API + una pequeña tarifa de Venice

### [​](https://docs.openclaw.ai/es/providers/venice\#venice-anonimizado-frente-a-api-directa)  Venice (anonimizado) frente a API directa

| Aspecto | Venice (anonimizado) | API directa |
| --- | --- | --- |
| **Privacidad** | Metadatos eliminados, anonimizado | Tu cuenta vinculada |
| **Latencia** | +10-50 ms (proxy) | Directa |
| **Funciones** | La mayoría de las funciones compatibles | Funciones completas |
| **Facturación** | Créditos de Venice | Facturación del proveedor |

## [​](https://docs.openclaw.ai/es/providers/venice\#ejemplos-de-uso)  Ejemplos de uso

```
# Use the default private model
openclaw agent --model venice/kimi-k2-5 --message "Quick health check"

# Use Claude Opus via Venice (anonymized)
openclaw agent --model venice/claude-opus-4-6 --message "Summarize this task"

# Use uncensored model
openclaw agent --model venice/venice-uncensored --message "Draft options"

# Use vision model with image
openclaw agent --model venice/qwen3-vl-235b-a22b --message "Review attached image"

# Use coding model
openclaw agent --model venice/qwen3-coder-480b-a35b-instruct --message "Refactor this function"
```

## [​](https://docs.openclaw.ai/es/providers/venice\#soluci%C3%B3n-de-problemas)  Solución de problemas

API key not recognized

```
echo $VENICE_API_KEY
openclaw models list | grep venice
```

Asegúrate de que la clave comience con `vapi_`.

Model not available

El catálogo de modelos de Venice se actualiza dinámicamente. Ejecuta `openclaw models list` para ver los modelos disponibles actualmente. Algunos modelos pueden estar temporalmente sin conexión.

Connection issues

La API de Venice está en `https://api.venice.ai/api/v1`. Asegúrate de que tu red permita conexiones HTTPS.

Más ayuda: [Solución de problemas](https://docs.openclaw.ai/es/help/troubleshooting) y [Preguntas frecuentes](https://docs.openclaw.ai/es/help/faq).

## [​](https://docs.openclaw.ai/es/providers/venice\#configuraci%C3%B3n-avanzada)  Configuración avanzada

Config file example

```
{
  env: { VENICE_API_KEY: "vapi_..." },
  agents: { defaults: { model: { primary: "venice/kimi-k2-5" } } },
  models: {
    mode: "merge",
    providers: {
      venice: {
        baseUrl: "https://api.venice.ai/api/v1",
        apiKey: "${VENICE_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "kimi-k2-5",\
            name: "Kimi K2.5",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 256000,\
            maxTokens: 65536,\
          },\
        ],
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/es/providers/venice\#relacionado)  Relacionado

[**Model selection** \\
\\
Elegir proveedores, referencias de modelo y comportamiento de conmutación por error.](https://docs.openclaw.ai/es/concepts/model-providers)

[**Venice AI** \\
\\
Página principal de Venice AI y registro de cuenta.](https://venice.ai/)

[**API documentation** \\
\\
Referencia de la API de Venice y documentación para desarrolladores.](https://docs.venice.ai/)

[**Pricing** \\
\\
Tarifas y planes actuales de créditos de Venice.](https://venice.ai/pricing)

[Together AI](https://docs.openclaw.ai/es/providers/together) [Gateway de IA de Vercel](https://docs.openclaw.ai/es/providers/vercel-ai-gateway)

Ctrl+I