---
source_url: https://docs.openclaw.ai/es/plugins/message-presentation
title: "Presentaci\u00f3n de mensajes - OpenClaw"
---

[Saltar al contenido principal](https://docs.openclaw.ai/es/plugins/message-presentation#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/es)

![ES](https://d3gk2c5xim1je2.cloudfront.net/flags/ES.svg)

Español

Buscar...

Ctrl K

Buscar...

Navigation

Plugins

Presentación de mensajes

[Get started](https://docs.openclaw.ai/es) [Install](https://docs.openclaw.ai/es/install) [Channels](https://docs.openclaw.ai/es/channels) [Agents](https://docs.openclaw.ai/es/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/es/tools) [Models](https://docs.openclaw.ai/es/providers) [Platforms](https://docs.openclaw.ai/es/platforms) [Gateway & Ops](https://docs.openclaw.ai/es/gateway) [Reference](https://docs.openclaw.ai/es/cli) [Help](https://docs.openclaw.ai/es/help)

En esta página

- [Contrato](https://docs.openclaw.ai/es/plugins/message-presentation#contrato)
- [Ejemplos de productores](https://docs.openclaw.ai/es/plugins/message-presentation#ejemplos-de-productores)
- [Contrato del renderizador](https://docs.openclaw.ai/es/plugins/message-presentation#contrato-del-renderizador)
- [Flujo de renderizado del núcleo](https://docs.openclaw.ai/es/plugins/message-presentation#flujo-de-renderizado-del-n%C3%BAcleo)
- [Reglas de degradación](https://docs.openclaw.ai/es/plugins/message-presentation#reglas-de-degradaci%C3%B3n)
- [Mapeo de proveedores](https://docs.openclaw.ai/es/plugins/message-presentation#mapeo-de-proveedores)
- [Presentación frente a InteractiveReply](https://docs.openclaw.ai/es/plugins/message-presentation#presentaci%C3%B3n-frente-a-interactivereply)
- [Fijación de entrega](https://docs.openclaw.ai/es/plugins/message-presentation#fijaci%C3%B3n-de-entrega)
- [Lista de verificación para autores de Plugin](https://docs.openclaw.ai/es/plugins/message-presentation#lista-de-verificaci%C3%B3n-para-autores-de-plugin)
- [Documentos relacionados](https://docs.openclaw.ai/es/plugins/message-presentation#documentos-relacionados)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

La presentación de mensajes es el contrato compartido de OpenClaw para una interfaz de chat saliente enriquecida.
Permite que agentes, comandos de CLI, flujos de aprobación y plugins describan la intención
del mensaje una vez, mientras cada Plugin de canal representa la mejor forma nativa que pueda.Usa la presentación para una interfaz de mensajes portable:

- secciones de texto
- texto breve de contexto/pie
- divisores
- botones
- menús de selección
- título y tono de tarjeta

No agregues nuevos campos nativos de proveedor como `components` de Discord, `blocks` de Slack,
`buttons` de Telegram, `card` de Teams o `card` de Feishu a la herramienta
compartida de mensajes. Esas son salidas de renderizador propiedad del Plugin de canal.

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#contrato)  Contrato

Los autores de Plugin importan el contrato público desde:

```
import type {
  MessagePresentation,
  ReplyPayloadDelivery,
} from "openclaw/plugin-sdk/interactive-runtime";
```

Forma:

```
type MessagePresentation = {
  title?: string;
  tone?: "neutral" | "info" | "success" | "warning" | "danger";
  blocks: MessagePresentationBlock[];
};

type MessagePresentationBlock =
  | { type: "text"; text: string }
  | { type: "context"; text: string }
  | { type: "divider" }
  | { type: "buttons"; buttons: MessagePresentationButton[] }
  | { type: "select"; placeholder?: string; options: MessagePresentationOption[] };

type MessagePresentationButton = {
  label: string;
  value?: string;
  url?: string;
  style?: "primary" | "secondary" | "success" | "danger";
};

type MessagePresentationOption = {
  label: string;
  value: string;
};

type ReplyPayloadDelivery = {
  pin?:
    | boolean
    | {
        enabled: boolean;
        notify?: boolean;
        required?: boolean;
      };
};
```

Semántica de botones:

- `value` es un valor de acción de la aplicación que se enruta de vuelta por la
ruta de interacción existente del canal cuando el canal admite controles clicables.
- `url` es un botón de enlace. Puede existir sin `value`.
- `label` es obligatorio y también se usa en el texto de reserva.
- `style` es orientativo. Los renderizadores deben asignar los estilos no admitidos a un valor
predeterminado seguro, no hacer que el envío falle.

Semántica de selección:

- `options[].value` es el valor de aplicación seleccionado.
- `placeholder` es orientativo y puede ser ignorado por canales sin compatibilidad nativa
con selecciones.
- Si un canal no admite selecciones, el texto de reserva enumera las etiquetas.

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#ejemplos-de-productores)  Ejemplos de productores

Tarjeta simple:

```
{
  "title": "Deploy approval",
  "tone": "warning",
  "blocks": [\
    { "type": "text", "text": "Canary is ready to promote." },\
    { "type": "context", "text": "Build 1234, staging passed." },\
    {\
      "type": "buttons",\
      "buttons": [\
        { "label": "Approve", "value": "deploy:approve", "style": "success" },\
        { "label": "Decline", "value": "deploy:decline", "style": "danger" }\
      ]\
    }\
  ]
}
```

Botón de enlace solo con URL:

```
{
  "blocks": [\
    { "type": "text", "text": "Release notes are ready." },\
    {\
      "type": "buttons",\
      "buttons": [{ "label": "Open notes", "url": "https://example.com/release" }]\
    }\
  ]
}
```

Menú de selección:

```
{
  "title": "Choose environment",
  "blocks": [\
    {\
      "type": "select",\
      "placeholder": "Environment",\
      "options": [\
        { "label": "Canary", "value": "env:canary" },\
        { "label": "Production", "value": "env:prod" }\
      ]\
    }\
  ]
}
```

Envío con CLI:

```
openclaw message send --channel slack \
  --target channel:C123 \
  --message "Deploy approval" \
  --presentation '{"title":"Deploy approval","tone":"warning","blocks":[{"type":"text","text":"Canary is ready."},{"type":"buttons","buttons":[{"label":"Approve","value":"deploy:approve","style":"success"},{"label":"Decline","value":"deploy:decline","style":"danger"}]}]}'
```

Entrega fijada:

```
openclaw message send --channel telegram \
  --target -1001234567890 \
  --message "Topic opened" \
  --pin
```

Entrega fijada con JSON explícito:

```
{
  "pin": {
    "enabled": true,
    "notify": true,
    "required": false
  }
}
```

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#contrato-del-renderizador)  Contrato del renderizador

Los plugins de canal declaran la compatibilidad de renderizado en su adaptador saliente:

```
const adapter: ChannelOutboundAdapter = {
  deliveryMode: "direct",
  presentationCapabilities: {
    supported: true,
    buttons: true,
    selects: true,
    context: true,
    divider: true,
  },
  deliveryCapabilities: {
    pin: true,
  },
  renderPresentation({ payload, presentation, ctx }) {
    return renderNativePayload(payload, presentation, ctx);
  },
  async pinDeliveredMessage({ target, messageId, pin }) {
    await pinNativeMessage(target, messageId, { notify: pin.notify === true });
  },
};
```

Los campos de capacidades son booleanos intencionalmente simples. Describen lo que el
renderizador puede hacer interactivo, no todos los límites de la plataforma nativa. Los renderizadores siguen
siendo responsables de límites específicos de plataforma como el número máximo de botones, el número de bloques y
el tamaño de tarjeta.

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#flujo-de-renderizado-del-n%C3%BAcleo)  Flujo de renderizado del núcleo

Cuando un `ReplyPayload` o una acción de mensaje incluye `presentation`, el núcleo:

1. Normaliza la carga útil de presentación.
2. Resuelve el adaptador saliente del canal de destino.
3. Lee `presentationCapabilities`.
4. Llama a `renderPresentation` cuando el adaptador puede renderizar la carga útil.
5. Recurre a texto conservador cuando el adaptador está ausente o no puede renderizar.
6. Envía la carga útil resultante por la ruta normal de entrega del canal.
7. Aplica metadatos de entrega como `delivery.pin` después del primer mensaje
enviado correctamente.

El núcleo es responsable del comportamiento de reserva para que los productores puedan permanecer independientes del canal. Los plugins
de canal son responsables del renderizado nativo y la gestión de interacciones.

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#reglas-de-degradaci%C3%B3n)  Reglas de degradación

La presentación debe poder enviarse con seguridad en canales limitados.El texto de reserva incluye:

- `title` como la primera línea
- bloques `text` como párrafos normales
- bloques `context` como líneas de contexto compactas
- bloques `divider` como separador visual
- etiquetas de botones, incluidas las URL para botones de enlace
- etiquetas de opciones de selección

Los controles nativos no admitidos deben degradarse en lugar de hacer fallar todo el envío.
Ejemplos:

- Telegram con botones en línea desactivados envía texto de reserva.
- Un canal sin compatibilidad con selección enumera las opciones de selección como texto.
- Un botón solo con URL se convierte en un botón de enlace nativo o en una línea de URL de reserva.
- Los fallos opcionales al fijar no hacen fallar el mensaje entregado.

La excepción principal es `delivery.pin.required: true`; si se solicita fijar como
obligatorio y el canal no puede fijar el mensaje enviado, la entrega informa un fallo.

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#mapeo-de-proveedores)  Mapeo de proveedores

Renderizadores incluidos actuales:

| Canal | Destino de renderizado nativo | Notas |
| --- | --- | --- |
| Discord | Componentes y contenedores de componentes | Conserva `channelData.discord.components` heredado para productores existentes de cargas útiles nativas del proveedor, pero los nuevos envíos compartidos deben usar `presentation`. |
| Slack | Block Kit | Conserva `channelData.slack.blocks` heredado para productores existentes de cargas útiles nativas del proveedor, pero los nuevos envíos compartidos deben usar `presentation`. |
| Telegram | Texto más teclados en línea | Los botones/selecciones requieren capacidad de botón en línea para la superficie de destino; de lo contrario se usa texto de reserva. |
| Mattermost | Texto más propiedades interactivas | Otros bloques se degradan a texto. |
| Microsoft Teams | Adaptive Cards | El texto sin formato `message` se incluye con la tarjeta cuando ambos se proporcionan. |
| Feishu | Tarjetas interactivas | El encabezado de tarjeta puede usar `title`; el cuerpo evita duplicar ese título. |
| Canales simples | Texto de reserva | Los canales sin renderizador siguen recibiendo una salida legible. |

La compatibilidad con cargas útiles nativas del proveedor es una facilidad de transición para productores
de respuestas existentes. No es una razón para agregar nuevos campos nativos compartidos.

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#presentaci%C3%B3n-frente-a-interactivereply)  Presentación frente a InteractiveReply

`InteractiveReply` es el subconjunto interno anterior usado por ayudantes de aprobación e interacción.
Admite:

- texto
- botones
- selecciones

`MessagePresentation` es el contrato canónico compartido de envío. Agrega:

- título
- tono
- contexto
- divisor
- botones solo con URL
- metadatos de entrega genéricos mediante `ReplyPayload.delivery`

Usa ayudantes de `openclaw/plugin-sdk/interactive-runtime` al conectar código
anterior:

```
import {
  interactiveReplyToPresentation,
  normalizeMessagePresentation,
  presentationToInteractiveReply,
  renderMessagePresentationFallbackText,
} from "openclaw/plugin-sdk/interactive-runtime";
```

El código nuevo debe aceptar o producir `MessagePresentation` directamente.

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#fijaci%C3%B3n-de-entrega)  Fijación de entrega

Fijar es comportamiento de entrega, no presentación. Usa `delivery.pin` en lugar de
campos nativos del proveedor como `channelData.telegram.pin`.Semántica:

- `pin: true` fija el primer mensaje entregado correctamente.
- `pin.notify` tiene `false` como valor predeterminado.
- `pin.required` tiene `false` como valor predeterminado.
- Los fallos opcionales al fijar se degradan y dejan intacto el mensaje enviado.
- Los fallos obligatorios al fijar hacen fallar la entrega.
- Los mensajes fragmentados fijan el primer fragmento entregado, no el fragmento final.

Las acciones manuales de mensaje `pin`, `unpin` y `pins` siguen existiendo para mensajes
existentes cuando el proveedor admite esas operaciones.

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#lista-de-verificaci%C3%B3n-para-autores-de-plugin)  Lista de verificación para autores de Plugin

- Declara `presentation` desde `describeMessageTool(...)` cuando el canal puede
renderizar o degradar de forma segura la presentación semántica.
- Agrega `presentationCapabilities` al adaptador saliente en tiempo de ejecución.
- Implementa `renderPresentation` en código de tiempo de ejecución, no en código de configuración de Plugin
del plano de control.
- Mantén las bibliotecas de interfaz nativa fuera de rutas activas de configuración/catálogo.
- Conserva los límites de plataforma en el renderizador y las pruebas.
- Agrega pruebas de reserva para botones no admitidos, selecciones, botones URL, duplicación de título/texto
y envíos mixtos de `message` más `presentation`.
- Agrega compatibilidad con fijación de entrega mediante `deliveryCapabilities.pin` y
`pinDeliveredMessage` solo cuando el proveedor puede fijar el id del mensaje enviado.
- No expongas nuevos campos nativos de proveedor de tarjeta/bloque/componente/botón mediante
el esquema de acción de mensaje compartido.

## [​](https://docs.openclaw.ai/es/plugins/message-presentation\#documentos-relacionados)  Documentos relacionados

- [CLI de mensajes](https://docs.openclaw.ai/es/cli/message)
- [Descripción general del SDK de Plugin](https://docs.openclaw.ai/es/plugins/sdk-overview)
- [Arquitectura de Plugin](https://docs.openclaw.ai/es/plugins/architecture-internals#message-tool-schemas)
- [Plan de refactorización de la presentación de canales](https://docs.openclaw.ai/es/plan/ui-channels)

[Memory LanceDB](https://docs.openclaw.ai/es/plugins/memory-lancedb) [Plugin de taller de Skills](https://docs.openclaw.ai/es/plugins/skill-workshop)

Ctrl+I