---
source_url: https://docs.openclaw.ai/es/channels/zalouser
title: "Zalo personal - OpenClaw"
---

[Saltar al contenido principal](https://docs.openclaw.ai/es/channels/zalouser#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/es)

![ES](https://d3gk2c5xim1je2.cloudfront.net/flags/ES.svg)

Español

Buscar...

Ctrl K

Buscar...

Navigation

Regional platforms

Zalo personal

[Get started](https://docs.openclaw.ai/es) [Install](https://docs.openclaw.ai/es/install) [Channels](https://docs.openclaw.ai/es/channels) [Agents](https://docs.openclaw.ai/es/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/es/tools) [Models](https://docs.openclaw.ai/es/providers) [Platforms](https://docs.openclaw.ai/es/platforms) [Gateway & Ops](https://docs.openclaw.ai/es/gateway) [Reference](https://docs.openclaw.ai/es/cli) [Help](https://docs.openclaw.ai/es/help)

En esta página

- [Plugin incluido](https://docs.openclaw.ai/es/channels/zalouser#plugin-incluido)
- [Configuración rápida (principiante)](https://docs.openclaw.ai/es/channels/zalouser#configuraci%C3%B3n-r%C3%A1pida-principiante)
- [Qué es](https://docs.openclaw.ai/es/channels/zalouser#qu%C3%A9-es)
- [Nomenclatura](https://docs.openclaw.ai/es/channels/zalouser#nomenclatura)
- [Encontrar IDs (directorio)](https://docs.openclaw.ai/es/channels/zalouser#encontrar-ids-directorio)
- [Límites](https://docs.openclaw.ai/es/channels/zalouser#l%C3%ADmites)
- [Control de acceso (DM)](https://docs.openclaw.ai/es/channels/zalouser#control-de-acceso-dm)
- [Acceso a grupos (opcional)](https://docs.openclaw.ai/es/channels/zalouser#acceso-a-grupos-opcional)
- [Activación por mención en grupo](https://docs.openclaw.ai/es/channels/zalouser#activaci%C3%B3n-por-menci%C3%B3n-en-grupo)
- [Varias cuentas](https://docs.openclaw.ai/es/channels/zalouser#varias-cuentas)
- [Escritura, reacciones y acuses de entrega](https://docs.openclaw.ai/es/channels/zalouser#escritura-reacciones-y-acuses-de-entrega)
- [Solución de problemas](https://docs.openclaw.ai/es/channels/zalouser#soluci%C3%B3n-de-problemas)
- [Relacionado](https://docs.openclaw.ai/es/channels/zalouser#relacionado)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Estado: experimental. Esta integración automatiza una **cuenta personal de Zalo** mediante `zca-js` nativo dentro de OpenClaw.

Esta es una integración no oficial y puede provocar la suspensión o prohibición de la cuenta. Úsala bajo tu propio riesgo.

## [​](https://docs.openclaw.ai/es/channels/zalouser\#plugin-incluido)  Plugin incluido

Zalo Personal se distribuye como un Plugin incluido en las versiones actuales de OpenClaw, por lo que las compilaciones
empaquetadas normales no necesitan una instalación separada.Si usas una compilación antigua o una instalación personalizada que excluye Zalo Personal,
instala un paquete npm actual cuando se publique uno:

- Instalar mediante la CLI: `openclaw plugins install @openclaw/zalouser`
- O desde un checkout de código fuente: `openclaw plugins install ./path/to/local/zalouser-plugin`
- Detalles: [Plugins](https://docs.openclaw.ai/es/tools/plugin)

Si npm informa que el paquete propiedad de OpenClaw está obsoleto, usa una compilación
empaquetada actual de OpenClaw o la ruta de checkout local hasta que se publique
un paquete npm más nuevo.No se requiere ningún binario de CLI externo `zca`/`openzca`.

## [​](https://docs.openclaw.ai/es/channels/zalouser\#configuraci%C3%B3n-r%C3%A1pida-principiante)  Configuración rápida (principiante)

1. Asegúrate de que el Plugin Zalo Personal esté disponible.
   - Las versiones empaquetadas actuales de OpenClaw ya lo incluyen.
   - Las instalaciones antiguas/personalizadas pueden añadirlo manualmente con los comandos anteriores.
2. Inicia sesión (QR, en la máquina del Gateway):
   - `openclaw channels login --channel zalouser`
   - Escanea el código QR con la aplicación móvil de Zalo.
3. Habilita el canal:

```
{
  channels: {
    zalouser: {
      enabled: true,
      dmPolicy: "pairing",
    },
  },
}
```

4. Reinicia el Gateway (o finaliza la configuración).
5. El acceso por DM usa emparejamiento de forma predeterminada; aprueba el código de emparejamiento en el primer contacto.

## [​](https://docs.openclaw.ai/es/channels/zalouser\#qu%C3%A9-es)  Qué es

- Se ejecuta completamente dentro del proceso mediante `zca-js`.
- Usa escuchadores de eventos nativos para recibir mensajes entrantes.
- Envía respuestas directamente mediante la API de JS (texto/medios/enlace).
- Está diseñado para casos de uso de “cuenta personal” donde la API de Zalo Bot no está disponible.

## [​](https://docs.openclaw.ai/es/channels/zalouser\#nomenclatura)  Nomenclatura

El id del canal es `zalouser` para dejar explícito que esto automatiza una **cuenta personal de usuario de Zalo** (no oficial). Mantenemos `zalo` reservado para una posible integración oficial futura con la API de Zalo.

## [​](https://docs.openclaw.ai/es/channels/zalouser\#encontrar-ids-directorio)  Encontrar IDs (directorio)

Usa la CLI de directorio para descubrir pares/grupos y sus IDs:

```
openclaw directory self --channel zalouser
openclaw directory peers list --channel zalouser --query "name"
openclaw directory groups list --channel zalouser --query "work"
```

## [​](https://docs.openclaw.ai/es/channels/zalouser\#l%C3%ADmites)  Límites

- El texto saliente se divide en fragmentos de ~2000 caracteres (límites del cliente de Zalo).
- El streaming está bloqueado de forma predeterminada.

## [​](https://docs.openclaw.ai/es/channels/zalouser\#control-de-acceso-dm)  Control de acceso (DM)

`channels.zalouser.dmPolicy` admite: `pairing | allowlist | open | disabled` (predeterminado: `pairing`).`channels.zalouser.allowFrom` acepta IDs de usuario o nombres. Durante la configuración, los nombres se resuelven a IDs usando la búsqueda de contactos dentro del proceso del Plugin.Aprueba mediante:

- `openclaw pairing list zalouser`
- `openclaw pairing approve zalouser <code>`

## [​](https://docs.openclaw.ai/es/channels/zalouser\#acceso-a-grupos-opcional)  Acceso a grupos (opcional)

- Predeterminado: `channels.zalouser.groupPolicy = "open"` (grupos permitidos). Usa `channels.defaults.groupPolicy` para sobrescribir el valor predeterminado cuando no esté definido.
- Restringe a una allowlist con:
  - `channels.zalouser.groupPolicy = "allowlist"`
  - `channels.zalouser.groups` (las claves deben ser IDs de grupo estables; los nombres se resuelven a IDs al iniciar cuando es posible)
  - `channels.zalouser.groupAllowFrom` (controla qué remitentes en los grupos permitidos pueden activar el bot)
- Bloquea todos los grupos: `channels.zalouser.groupPolicy = "disabled"`.
- El asistente de configuración puede solicitar allowlists de grupos.
- Al iniciar, OpenClaw resuelve los nombres de grupos/usuarios en las allowlists a IDs y registra el mapeo.
- La coincidencia de allowlist de grupos se basa solo en ID de forma predeterminada. Los nombres sin resolver se ignoran para autenticación a menos que `channels.zalouser.dangerouslyAllowNameMatching: true` esté habilitado.
- `channels.zalouser.dangerouslyAllowNameMatching: true` es un modo de compatibilidad de emergencia que vuelve a habilitar la coincidencia mutable por nombre de grupo.
- Si `groupAllowFrom` no está definido, en tiempo de ejecución se recurre a `allowFrom` para las comprobaciones de remitentes de grupo.
- Las comprobaciones de remitente se aplican tanto a mensajes de grupo normales como a comandos de control (por ejemplo `/new`, `/reset`).

Ejemplo:

```
{
  channels: {
    zalouser: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["1471383327500481391"],
      groups: {
        "123456789": { allow: true },
        "Work Chat": { allow: true },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/es/channels/zalouser\#activaci%C3%B3n-por-menci%C3%B3n-en-grupo)  Activación por mención en grupo

- `channels.zalouser.groups.<group>.requireMention` controla si las respuestas en grupo requieren una mención.
- Orden de resolución: id/nombre de grupo exacto -> slug de grupo normalizado -> `*` -\> predeterminado (`true`).
- Esto se aplica tanto a grupos en allowlist como al modo de grupo abierto.
- Citar un mensaje del bot cuenta como una mención implícita para la activación en grupo.
- Los comandos de control autorizados (por ejemplo `/new`) pueden omitir la activación por mención.
- Cuando se omite un mensaje de grupo porque se requiere una mención, OpenClaw lo almacena como historial de grupo pendiente y lo incluye en el siguiente mensaje de grupo procesado.
- El límite de historial de grupo usa `messages.groupChat.historyLimit` de forma predeterminada (respaldo `50`). Puedes sobrescribirlo por cuenta con `channels.zalouser.historyLimit`.

Ejemplo:

```
{
  channels: {
    zalouser: {
      groupPolicy: "allowlist",
      groups: {
        "*": { allow: true, requireMention: true },
        "Work Chat": { allow: true, requireMention: false },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/es/channels/zalouser\#varias-cuentas)  Varias cuentas

Las cuentas se asignan a perfiles `zalouser` en el estado de OpenClaw. Ejemplo:

```
{
  channels: {
    zalouser: {
      enabled: true,
      defaultAccount: "default",
      accounts: {
        work: { enabled: true, profile: "work" },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/es/channels/zalouser\#escritura-reacciones-y-acuses-de-entrega)  Escritura, reacciones y acuses de entrega

- OpenClaw envía un evento de escritura antes de despachar una respuesta (mejor esfuerzo).
- La acción de reacción a mensaje `react` es compatible con `zalouser` en las acciones de canal.

  - Usa `remove: true` para eliminar un emoji de reacción específico de un mensaje.
  - Semántica de reacciones: [Reacciones](https://docs.openclaw.ai/es/tools/reactions)
- Para mensajes entrantes que incluyen metadatos de evento, OpenClaw envía acuses de entregado + visto (mejor esfuerzo).

## [​](https://docs.openclaw.ai/es/channels/zalouser\#soluci%C3%B3n-de-problemas)  Solución de problemas

**El inicio de sesión no persiste:**

- `openclaw channels status --probe`
- Vuelve a iniciar sesión: `openclaw channels logout --channel zalouser && openclaw channels login --channel zalouser`

**La allowlist/el nombre de grupo no se resolvió:**

- Usa IDs numéricos en `allowFrom`/`groupAllowFrom`/`groups`, o nombres exactos de amigos/grupos.

**Actualizado desde una configuración antigua basada en CLI:**

- Elimina cualquier suposición antigua sobre procesos externos `zca`.
- El canal ahora se ejecuta completamente en OpenClaw sin binarios de CLI externos.

## [​](https://docs.openclaw.ai/es/channels/zalouser\#relacionado)  Relacionado

- [Resumen de canales](https://docs.openclaw.ai/es/channels) — todos los canales compatibles
- [Emparejamiento](https://docs.openclaw.ai/es/channels/pairing) — autenticación por DM y flujo de emparejamiento
- [Grupos](https://docs.openclaw.ai/es/channels/groups) — comportamiento del chat de grupo y activación por mención
- [Enrutamiento de canales](https://docs.openclaw.ai/es/channels/channel-routing) — enrutamiento de sesiones para mensajes
- [Seguridad](https://docs.openclaw.ai/es/gateway/security) — modelo de acceso y endurecimiento

[Zalo](https://docs.openclaw.ai/es/channels/zalo) [Emparejamiento](https://docs.openclaw.ai/es/channels/pairing)

Ctrl+I