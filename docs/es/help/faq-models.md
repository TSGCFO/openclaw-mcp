---
source_url: https://docs.openclaw.ai/es/help/faq-models
title: "Preguntas frecuentes: modelos y autenticaci\u00f3n - OpenClaw"
---

[Saltar al contenido principal](https://docs.openclaw.ai/es/help/faq-models#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/es)

![ES](https://d3gk2c5xim1je2.cloudfront.net/flags/ES.svg)

Español

Buscar...

Ctrl K

Buscar...

Navigation

FAQ

Preguntas frecuentes: modelos y autenticación

[Get started](https://docs.openclaw.ai/es) [Install](https://docs.openclaw.ai/es/install) [Channels](https://docs.openclaw.ai/es/channels) [Agents](https://docs.openclaw.ai/es/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/es/tools) [Models](https://docs.openclaw.ai/es/providers) [Platforms](https://docs.openclaw.ai/es/platforms) [Gateway & Ops](https://docs.openclaw.ai/es/gateway) [Reference](https://docs.openclaw.ai/es/cli) [Help](https://docs.openclaw.ai/es/help)

En esta página

- [Modelos: valores predeterminados, selección, alias, cambio](https://docs.openclaw.ai/es/help/faq-models#modelos-valores-predeterminados-selecci%C3%B3n-alias-cambio)
- [Conmutación por error de modelos y “All models failed”](https://docs.openclaw.ai/es/help/faq-models#conmutaci%C3%B3n-por-error-de-modelos-y-%E2%80%9Call-models-failed%E2%80%9D)
- [Perfiles de autenticación: qué son y cómo gestionarlos](https://docs.openclaw.ai/es/help/faq-models#perfiles-de-autenticaci%C3%B3n-qu%C3%A9-son-y-c%C3%B3mo-gestionarlos)
- [Relacionado](https://docs.openclaw.ai/es/help/faq-models#relacionado)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Modelos y perfiles de autenticación: preguntas y respuestas. Para configuración, sesiones, Gateway, canales y
solución de problemas, consulta la [FAQ](https://docs.openclaw.ai/es/help/faq) principal.

## [​](https://docs.openclaw.ai/es/help/faq-models\#modelos-valores-predeterminados-selecci%C3%B3n-alias-cambio)  Modelos: valores predeterminados, selección, alias, cambio

¿Qué es el "modelo predeterminado"?

El modelo predeterminado de OpenClaw es lo que configures como:

```
agents.defaults.model.primary
```

Los modelos se referencian como `provider/model` (ejemplo: `openai/gpt-5.5` u `openai-codex/gpt-5.5`). Si omites el proveedor, OpenClaw primero intenta un alias, luego una coincidencia única de proveedor configurado para ese ID de modelo exacto, y solo después recurre al proveedor predeterminado configurado como ruta de compatibilidad obsoleta. Si ese proveedor ya no expone el modelo predeterminado configurado, OpenClaw recurre al primer proveedor/modelo configurado en lugar de mostrar un valor predeterminado obsoleto de un proveedor eliminado. Aun así, deberías configurar `provider/model` **explícitamente**.

¿Qué modelo recomiendas?

**Predeterminado recomendado:** usa el modelo más potente de última generación disponible en tu pila de proveedores.
**Para agentes con herramientas habilitadas o entradas no confiables:** prioriza la potencia del modelo sobre el costo.
**Para chat rutinario/de bajo riesgo:** usa modelos alternativos más baratos y enruta según el rol del agente.MiniMax tiene su propia documentación: [MiniMax](https://docs.openclaw.ai/es/providers/minimax) y
[Modelos locales](https://docs.openclaw.ai/es/gateway/local-models).Regla general: usa el **mejor modelo que puedas costear** para trabajo de alto riesgo, y un modelo más barato
para chat rutinario o resúmenes. Puedes enrutar modelos por agente y usar subagentes para
paralelizar tareas largas (cada subagente consume tokens). Consulta [Modelos](https://docs.openclaw.ai/es/concepts/models) y
[Subagentes](https://docs.openclaw.ai/es/tools/subagents).Advertencia importante: los modelos más débiles o sobrecuantizados son más vulnerables a la inyección
de prompts y al comportamiento inseguro. Consulta [Seguridad](https://docs.openclaw.ai/es/gateway/security).Más contexto: [Modelos](https://docs.openclaw.ai/es/concepts/models).

¿Cómo cambio de modelo sin borrar mi configuración?

Usa **comandos de modelo** o edita solo los campos de **modelo**. Evita reemplazar toda la configuración.Opciones seguras:

- `/model` en el chat (rápido, por sesión)
- `openclaw models set ...` (actualiza solo la configuración del modelo)
- `openclaw configure --section model` (interactivo)
- edita `agents.defaults.model` en `~/.openclaw/openclaw.json`

Evita `config.apply` con un objeto parcial a menos que quieras reemplazar toda la configuración.
Para ediciones RPC, inspecciona primero con `config.schema.lookup` y prefiere `config.patch`. La carga útil de lookup te da la ruta normalizada, documentación/restricciones superficiales del esquema y resúmenes inmediatos de los elementos secundarios.
para actualizaciones parciales.
Si sobrescribiste la configuración, restaura desde una copia de seguridad o vuelve a ejecutar `openclaw doctor` para reparar.Documentación: [Modelos](https://docs.openclaw.ai/es/concepts/models), [Configurar](https://docs.openclaw.ai/es/cli/configure), [Configuración](https://docs.openclaw.ai/es/cli/config), [Doctor](https://docs.openclaw.ai/es/gateway/doctor).

¿Puedo usar modelos autoalojados (llama.cpp, vLLM, Ollama)?

Sí. Ollama es la ruta más sencilla para modelos locales.Configuración más rápida:

1. Instala Ollama desde `https://ollama.com/download`
2. Descarga un modelo local como `ollama pull gemma4`
3. Si también quieres modelos en la nube, ejecuta `ollama signin`
4. Ejecuta `openclaw onboard` y elige `Ollama`
5. Elige `Local` o `Cloud + Local`

Notas:

- `Cloud + Local` te da modelos en la nube además de tus modelos locales de Ollama
- los modelos en la nube como `kimi-k2.5:cloud` no necesitan una descarga local
- para cambio manual, usa `openclaw models list` y `openclaw models set ollama/<model>`

Nota de seguridad: los modelos más pequeños o muy cuantizados son más vulnerables a la inyección
de prompts. Recomendamos firmemente **modelos grandes** para cualquier bot que pueda usar herramientas.
Si aun así quieres modelos pequeños, habilita el aislamiento y listas de herramientas permitidas estrictas.Documentación: [Ollama](https://docs.openclaw.ai/es/providers/ollama), [Modelos locales](https://docs.openclaw.ai/es/gateway/local-models),
[Proveedores de modelos](https://docs.openclaw.ai/es/concepts/model-providers), [Seguridad](https://docs.openclaw.ai/es/gateway/security),
[Aislamiento](https://docs.openclaw.ai/es/gateway/sandboxing).

¿Qué usan OpenClaw, Flawd y Krill como modelos?

- Estas implementaciones pueden diferir y cambiar con el tiempo; no hay una recomendación fija de proveedor.
- Comprueba la configuración de runtime actual en cada gateway con `openclaw models status`.
- Para agentes sensibles a la seguridad o con herramientas habilitadas, usa el modelo más potente de última generación disponible.

¿Cómo cambio de modelo sobre la marcha (sin reiniciar)?

Usa el comando `/model` como mensaje independiente:

```
/model sonnet
/model opus
/model gpt
/model gpt-mini
/model gemini
/model gemini-flash
/model gemini-flash-lite
```

Estos son los alias integrados. Los alias personalizados pueden añadirse mediante `agents.defaults.models`.Puedes listar los modelos disponibles con `/model`, `/model list` o `/model status`.`/model` (y `/model list`) muestra un selector compacto y numerado. Selecciona por número:

```
/model 3
```

También puedes forzar un perfil de autenticación específico para el proveedor (por sesión):

```
/model opus@anthropic:default
/model opus@anthropic:work
```

Consejo: `/model status` muestra qué agente está activo, qué archivo `auth-profiles.json` se está usando y qué perfil de autenticación se intentará después.
También muestra el endpoint del proveedor configurado (`baseUrl`) y el modo de API (`api`) cuando están disponibles.**¿Cómo quito la fijación de un perfil que configuré con @profile?**Vuelve a ejecutar `/model` **sin** el sufijo `@profile`:

```
/model anthropic/claude-opus-4-6
```

Si quieres volver al valor predeterminado, selecciónalo desde `/model` (o envía `/model <default provider/model>`).
Usa `/model status` para confirmar qué perfil de autenticación está activo.

¿Puedo usar GPT 5.5 para tareas diarias y Codex 5.5 para programar?

Sí. Trata la elección del modelo y la elección del runtime por separado:

- **Agente de programación Codex nativo:** configura `agents.defaults.model.primary` en `openai/gpt-5.5` y `agents.defaults.agentRuntime.id` en `"codex"`. Inicia sesión con `openclaw models auth login --provider openai-codex` cuando quieras autenticación de suscripción de ChatGPT/Codex.
- **Tareas directas de la API de OpenAI a través de PI:** usa `/model openai/gpt-5.5` sin una anulación de runtime de Codex y configura `OPENAI_API_KEY`.
- **OAuth de Codex a través de PI:** usa `/model openai-codex/gpt-5.5` solo cuando quieras intencionalmente el ejecutor PI normal con OAuth de Codex.
- **Subagentes:** enruta tareas de programación a un agente solo de Codex con su propio modelo y valor predeterminado de `agentRuntime`.

Consulta [Modelos](https://docs.openclaw.ai/es/concepts/models) y [Comandos slash](https://docs.openclaw.ai/es/tools/slash-commands).

¿Cómo configuro el modo rápido para GPT 5.5?

Usa un cambio de sesión o un valor predeterminado de configuración:

- **Por sesión:** envía `/fast on` mientras la sesión usa `openai/gpt-5.5` u `openai-codex/gpt-5.5`.
- **Valor predeterminado por modelo:** configura `agents.defaults.models["openai/gpt-5.5"].params.fastMode` o `agents.defaults.models["openai-codex/gpt-5.5"].params.fastMode` en `true`.

Ejemplo:

```
{
  agents: {
    defaults: {
      models: {
        "openai/gpt-5.5": {
          params: {
            fastMode: true,
          },
        },
      },
    },
  },
}
```

Para OpenAI, el modo rápido se asigna a `service_tier = "priority"` en solicitudes nativas compatibles de Responses. Las anulaciones de sesión con `/fast` prevalecen sobre los valores predeterminados de configuración.Consulta [Razonamiento y modo rápido](https://docs.openclaw.ai/es/tools/thinking) y [Modo rápido de OpenAI](https://docs.openclaw.ai/es/providers/openai#fast-mode).

¿Por qué veo "Model ... is not allowed" y luego no hay respuesta?

Si `agents.defaults.models` está configurado, se convierte en la **lista de permitidos** para `/model` y cualquier
anulación de sesión. Elegir un modelo que no esté en esa lista devuelve:

```
Model "provider/model" is not allowed. Use /model to list available models.
```

Ese error se devuelve **en lugar de** una respuesta normal. Solución: añade el modelo a
`agents.defaults.models`, elimina la lista de permitidos o elige un modelo desde `/model list`.

¿Por qué veo "Unknown model: minimax/MiniMax-M2.7"?

Esto significa que el **proveedor no está configurado** (no se encontró configuración de proveedor MiniMax ni perfil de autenticación),
por lo que el modelo no puede resolverse.Lista de comprobación para corregirlo:

1. Actualiza a una versión actual de OpenClaw (o ejecuta desde `main` de código fuente) y luego reinicia el gateway.
2. Asegúrate de que MiniMax esté configurado (asistente o JSON), o de que la autenticación de MiniMax
exista en perfiles de entorno/autenticación para que el proveedor coincidente pueda inyectarse
(`MINIMAX_API_KEY` para `minimax`, `MINIMAX_OAUTH_TOKEN` u OAuth de MiniMax almacenado
para `minimax-portal`).
3. Usa el ID de modelo exacto (distingue mayúsculas y minúsculas) para tu ruta de autenticación:
`minimax/MiniMax-M2.7` o `minimax/MiniMax-M2.7-highspeed` para configuración
con clave de API, o `minimax-portal/MiniMax-M2.7` /
`minimax-portal/MiniMax-M2.7-highspeed` para configuración OAuth.
4. Ejecuta:














```
openclaw models list
```










y elige de la lista (o `/model list` en el chat).

Consulta [MiniMax](https://docs.openclaw.ai/es/providers/minimax) y [Modelos](https://docs.openclaw.ai/es/concepts/models).

¿Puedo usar MiniMax como predeterminado y OpenAI para tareas complejas?

Sí. Usa **MiniMax como predeterminado** y cambia de modelo **por sesión** cuando sea necesario.
Los fallbacks son para **errores**, no para “tareas difíciles”, así que usa `/model` o un agente separado.**Opción A: cambiar por sesión**

```
{
  env: { MINIMAX_API_KEY: "sk-...", OPENAI_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "minimax/MiniMax-M2.7" },
      models: {
        "minimax/MiniMax-M2.7": { alias: "minimax" },
        "openai/gpt-5.5": { alias: "gpt" },
      },
    },
  },
}
```

Luego:

```
/model gpt
```

**Opción B: agentes separados**

- Agente A predeterminado: MiniMax
- Agente B predeterminado: OpenAI
- Enruta por agente o usa `/agent` para cambiar

Documentación: [Modelos](https://docs.openclaw.ai/es/concepts/models), [Enrutamiento multiagente](https://docs.openclaw.ai/es/concepts/multi-agent), [MiniMax](https://docs.openclaw.ai/es/providers/minimax), [OpenAI](https://docs.openclaw.ai/es/providers/openai).

¿opus / sonnet / gpt son accesos directos integrados?

Sí. OpenClaw incluye algunos atajos predeterminados (solo se aplican cuando el modelo existe en `agents.defaults.models`):

- `opus` → `anthropic/claude-opus-4-6`
- `sonnet` → `anthropic/claude-sonnet-4-6`
- `gpt` → `openai/gpt-5.5` para configuraciones con clave de API, u `openai-codex/gpt-5.5` cuando está configurado para OAuth de Codex
- `gpt-mini` → `openai/gpt-5.4-mini`
- `gpt-nano` → `openai/gpt-5.4-nano`
- `gemini` → `google/gemini-3.1-pro-preview`
- `gemini-flash` → `google/gemini-3-flash-preview`
- `gemini-flash-lite` → `google/gemini-3.1-flash-lite-preview`

Si configuras tu propio alias con el mismo nombre, tu valor prevalece.

¿Cómo defino/anulo accesos directos de modelos (alias)?

Los alias vienen de `agents.defaults.models.<modelId>.alias`. Ejemplo:

```
{
  agents: {
    defaults: {
      model: { primary: "anthropic/claude-opus-4-6" },
      models: {
        "anthropic/claude-opus-4-6": { alias: "opus" },
        "anthropic/claude-sonnet-4-6": { alias: "sonnet" },
        "anthropic/claude-haiku-4-5": { alias: "haiku" },
      },
    },
  },
}
```

Luego `/model sonnet` (o `/<alias>` cuando sea compatible) se resuelve a ese ID de modelo.

¿Cómo añado modelos de otros proveedores como OpenRouter o Z.AI?

OpenRouter (pago por token; muchos modelos):

```
{
  agents: {
    defaults: {
      model: { primary: "openrouter/anthropic/claude-sonnet-4-6" },
      models: { "openrouter/anthropic/claude-sonnet-4-6": {} },
    },
  },
  env: { OPENROUTER_API_KEY: "sk-or-..." },
}
```

Z.AI (modelos GLM):

```
{
  agents: {
    defaults: {
      model: { primary: "zai/glm-5" },
      models: { "zai/glm-5": {} },
    },
  },
  env: { ZAI_API_KEY: "..." },
}
```

Si haces referencia a un proveedor/modelo pero falta la clave del proveedor requerida, obtendrás un error de autenticación en tiempo de ejecución (por ejemplo, `No API key found for provider "zai"`).**No se encontró ninguna clave de API para el proveedor después de agregar un agente nuevo**Esto suele significar que el **agente nuevo** tiene un almacén de autenticación vacío. La autenticación es por agente y
se almacena en:

```
~/.openclaw/agents/<agentId>/agent/auth-profiles.json
```

Opciones de corrección:

- Ejecuta `openclaw agents add <id>` y configura la autenticación durante el asistente.
- O copia solo los perfiles estáticos portables `api_key` / `token` del almacén de autenticación del agente principal al almacén de autenticación del agente nuevo.
- Para perfiles OAuth, inicia sesión desde el agente nuevo cuando necesite su propia cuenta; de lo contrario, OpenClaw puede leer desde el agente predeterminado/principal sin clonar tokens de actualización.

No reutilices `agentDir` entre agentes; provoca colisiones de autenticación/sesión.

## [​](https://docs.openclaw.ai/es/help/faq-models\#conmutaci%C3%B3n-por-error-de-modelos-y-%E2%80%9Call-models-failed%E2%80%9D)  Conmutación por error de modelos y “All models failed”

¿Cómo funciona la conmutación por error?

La conmutación por error ocurre en dos etapas:

1. **Rotación de perfiles de autenticación** dentro del mismo proveedor.
2. **Modelo de respaldo** al siguiente modelo en `agents.defaults.model.fallbacks`.

Los tiempos de espera se aplican a los perfiles que fallan (retroceso exponencial), por lo que OpenClaw puede seguir respondiendo incluso cuando un proveedor tiene limitación de tasa o falla temporalmente.El grupo de limitación de tasa incluye más que simples respuestas `429`. OpenClaw
también trata mensajes como `Too many concurrent requests`,
`ThrottlingException`, `concurrency limit reached`,
`workers_ai ... quota limit exceeded`, `resource exhausted` y límites periódicos
de ventanas de uso (`weekly/monthly limit reached`) como límites de tasa
que justifican la conmutación por error.Algunas respuestas que parecen de facturación no son `402`, y algunas respuestas HTTP `402`
también permanecen en ese grupo transitorio. Si un proveedor devuelve
texto explícito de facturación en `401` o `403`, OpenClaw aún puede mantenerlo en
la vía de facturación, pero los comparadores de texto específicos del proveedor permanecen limitados al
proveedor que los posee (por ejemplo, OpenRouter `Key limit exceeded`). Si un mensaje `402`
en cambio parece una ventana de uso reintentable o un
límite de gasto de organización/espacio de trabajo (`daily limit reached, resets tomorrow`,
`organization spending limit exceeded`), OpenClaw lo trata como
`rate_limit`, no como una deshabilitación prolongada por facturación.Los errores de desbordamiento de contexto son distintos: firmas como
`request_too_large`, `input exceeds the maximum number of tokens`,
`input token count exceeds the maximum number of input tokens`,
`input is too long for the model` u `ollama error: context length         exceeded` permanecen en la ruta de Compaction/reintento en lugar de avanzar al modelo
de respaldo.El texto genérico de error del servidor es intencionalmente más limitado que “cualquier cosa con
unknown/error dentro”. OpenClaw sí trata formas transitorias con alcance de proveedor
como el `An unknown error occurred` directo de Anthropic, el
`Provider returned error` directo de OpenRouter, errores de motivo de detención como `Unhandled stop reason:         error`, cargas JSON `api_error` con texto transitorio del servidor
(`internal server error`, `unknown error, 520`, `upstream error`, `backend         error`) y errores de proveedor ocupado como `ModelNotReadyException` como
señales de tiempo de espera/sobrecarga que justifican la conmutación por error cuando el contexto del proveedor
coincide.
El texto genérico de respaldo interno como `LLM request failed with an unknown         error.` se mantiene conservador y no activa por sí solo el modelo de respaldo.

¿Qué significa "No credentials found for profile anthropic:default"?

Significa que el sistema intentó usar el ID de perfil de autenticación `anthropic:default`, pero no pudo encontrar credenciales para él en el almacén de autenticación esperado.**Lista de comprobación para corregirlo:**

- **Confirma dónde viven los perfiles de autenticación**(rutas nuevas frente a heredadas)

  - Actual: `~/.openclaw/agents/<agentId>/agent/auth-profiles.json`
  - Heredada: `~/.openclaw/agent/*` (migrada por `openclaw doctor`)
- **Confirma que tu variable de entorno la carga el Gateway**
  - Si configuraste `ANTHROPIC_API_KEY` en tu shell pero ejecutas el Gateway mediante systemd/launchd, es posible que no la herede. Ponla en `~/.openclaw/.env` o habilita `env.shellEnv`.
- **Asegúrate de estar editando el agente correcto**
  - Las configuraciones multiagente significan que puede haber varios archivos `auth-profiles.json`.
- **Comprueba el estado del modelo/autenticación**
  - Usa `openclaw models status` para ver los modelos configurados y si los proveedores están autenticados.

**Lista de comprobación para corregir “No credentials found for profile anthropic”**Esto significa que la ejecución está fijada a un perfil de autenticación de Anthropic, pero el Gateway
no puede encontrarlo en su almacén de autenticación.

- **Usa Claude CLI**  - Ejecuta `openclaw models auth login --provider anthropic --method cli --set-default` en el host del gateway.
- **Si quieres usar una clave de API en su lugar**  - Pon `ANTHROPIC_API_KEY` en `~/.openclaw/.env` en el **host del gateway**.
  - Borra cualquier orden fijado que fuerce un perfil faltante:














    ```
    openclaw models auth order clear --provider anthropic
    ```
- **Confirma que estás ejecutando comandos en el host del gateway**  - En modo remoto, los perfiles de autenticación viven en la máquina del gateway, no en tu portátil.

¿Por qué también intentó usar Google Gemini y falló?

Si tu configuración de modelo incluye Google Gemini como respaldo (o cambiaste a una forma abreviada de Gemini), OpenClaw lo intentará durante la conmutación por error del modelo. Si no has configurado credenciales de Google, verás `No API key found for provider "google"`.Corrección: proporciona autenticación de Google o elimina/evita los modelos de Google en `agents.defaults.model.fallbacks` / alias para que el respaldo no se dirija allí.**Solicitud LLM rechazada: se requiere firma de razonamiento (Google Antigravity)**Causa: el historial de la sesión contiene **bloques de razonamiento sin firmas** (a menudo de
un flujo interrumpido/parcial). Google Antigravity requiere firmas para los bloques de razonamiento.Solución: OpenClaw ahora elimina los bloques de razonamiento sin firma para Google Antigravity Claude. Si sigue apareciendo, inicia una **sesión nueva** o establece `/thinking off` para ese agente.

## [​](https://docs.openclaw.ai/es/help/faq-models\#perfiles-de-autenticaci%C3%B3n-qu%C3%A9-son-y-c%C3%B3mo-gestionarlos)  Perfiles de autenticación: qué son y cómo gestionarlos

Relacionado: [/concepts/oauth](https://docs.openclaw.ai/es/concepts/oauth) (flujos OAuth, almacenamiento de tokens, patrones multicuenta)

¿Qué es un perfil de autenticación?

Un perfil de autenticación es un registro de credenciales con nombre (OAuth o clave de API) vinculado a un proveedor. Los perfiles viven en:

```
~/.openclaw/agents/<agentId>/agent/auth-profiles.json
```

¿Cuáles son los ID de perfil típicos?

OpenClaw usa ID con prefijo de proveedor como:

- `anthropic:default` (común cuando no existe identidad de correo electrónico)
- `anthropic:<email>` para identidades OAuth
- ID personalizados que elijas (por ejemplo, `anthropic:work`)

¿Puedo controlar qué perfil de autenticación se prueba primero?

Sí. La configuración admite metadatos opcionales para perfiles y un orden por proveedor (`auth.order.<provider>`). Esto **no** almacena secretos; asigna ID a proveedor/modo y establece el orden de rotación.OpenClaw puede omitir temporalmente un perfil si está en un **enfriamiento** breve (límites de tasa/tiempos de espera/fallos de autenticación) o en un estado **deshabilitado** más largo (facturación/créditos insuficientes). Para inspeccionarlo, ejecuta `openclaw models status --json` y revisa `auth.unusableProfiles`. Ajuste: `auth.cooldowns.billingBackoffHours*`.Los enfriamientos por límite de tasa pueden tener alcance por modelo. Un perfil que está en enfriamiento
para un modelo aún puede ser utilizable para un modelo hermano en el mismo proveedor,
mientras que las ventanas de facturación/deshabilitado siguen bloqueando todo el perfil.También puedes establecer una anulación de orden **por agente** (almacenada en el `auth-state.json` de ese agente) mediante la CLI:

```
# Usa de forma predeterminada el agente predeterminado configurado (omite --agent)
openclaw models auth order get --provider anthropic

# Bloquea la rotación a un solo perfil (probar solo este)
openclaw models auth order set --provider anthropic anthropic:default

# O establece un orden explícito (respaldo dentro del proveedor)
openclaw models auth order set --provider anthropic anthropic:work anthropic:default

# Borra la anulación (volver a config auth.order / round-robin)
openclaw models auth order clear --provider anthropic
```

Para apuntar a un agente específico:

```
openclaw models auth order set --provider anthropic --agent main anthropic:default
```

Para verificar qué se intentará realmente, usa:

```
openclaw models status --probe
```

Si un perfil almacenado se omite del orden explícito, la sonda informa
`excluded_by_auth_order` para ese perfil en lugar de probarlo silenciosamente.

OAuth frente a clave de API: ¿cuál es la diferencia?

OpenClaw admite ambos:

- **OAuth** a menudo aprovecha el acceso por suscripción (cuando corresponde).
- Las **claves de API** usan facturación de pago por token.

El asistente admite explícitamente Anthropic Claude CLI, OpenAI Codex OAuth y claves de API.

## [​](https://docs.openclaw.ai/es/help/faq-models\#relacionado)  Relacionado

- [FAQ](https://docs.openclaw.ai/es/help/faq) — la FAQ principal
- [FAQ — inicio rápido y configuración de primera ejecución](https://docs.openclaw.ai/es/help/faq-first-run)
- [Selección de modelo](https://docs.openclaw.ai/es/concepts/model-providers)
- [Conmutación por error de modelo](https://docs.openclaw.ai/es/concepts/model-failover)

[First-run FAQ](https://docs.openclaw.ai/es/help/faq-first-run) [Pruebas](https://docs.openclaw.ai/es/help/testing)

Ctrl+I