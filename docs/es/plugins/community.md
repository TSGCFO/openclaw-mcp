---
source_url: https://docs.openclaw.ai/es/plugins/community
title: "Plugins de la comunidad - OpenClaw"
---

[Saltar al contenido principal](https://docs.openclaw.ai/es/plugins/community#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/es)

![ES](https://d3gk2c5xim1je2.cloudfront.net/flags/ES.svg)

Español

Buscar...

Ctrl K

Buscar...

Navigation

Plugins

Plugins de la comunidad

[Get started](https://docs.openclaw.ai/es) [Install](https://docs.openclaw.ai/es/install) [Channels](https://docs.openclaw.ai/es/channels) [Agents](https://docs.openclaw.ai/es/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/es/tools) [Models](https://docs.openclaw.ai/es/providers) [Platforms](https://docs.openclaw.ai/es/platforms) [Gateway & Ops](https://docs.openclaw.ai/es/gateway) [Reference](https://docs.openclaw.ai/es/cli) [Help](https://docs.openclaw.ai/es/help)

En esta página

- [Plugins listados](https://docs.openclaw.ai/es/plugins/community#plugins-listados)
- [Apify](https://docs.openclaw.ai/es/plugins/community#apify)
- [Codex App Server Bridge](https://docs.openclaw.ai/es/plugins/community#codex-app-server-bridge)
- [DingTalk](https://docs.openclaw.ai/es/plugins/community#dingtalk)
- [Lossless Claw (LCM)](https://docs.openclaw.ai/es/plugins/community#lossless-claw-lcm)
- [Opik](https://docs.openclaw.ai/es/plugins/community#opik)
- [Prometheus Avatar](https://docs.openclaw.ai/es/plugins/community#prometheus-avatar)
- [QQbot](https://docs.openclaw.ai/es/plugins/community#qqbot)
- [wecom](https://docs.openclaw.ai/es/plugins/community#wecom)
- [Yuanbao](https://docs.openclaw.ai/es/plugins/community#yuanbao)
- [Envía tu plugin](https://docs.openclaw.ai/es/plugins/community#env%C3%ADa-tu-plugin)
- [Estándar de calidad](https://docs.openclaw.ai/es/plugins/community#est%C3%A1ndar-de-calidad)
- [Relacionado](https://docs.openclaw.ai/es/plugins/community#relacionado)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Los plugins de la comunidad son paquetes de terceros que extienden OpenClaw con nuevos
canales, herramientas, proveedores u otras capacidades. La comunidad los crea y mantiene,
normalmente se publican en [ClawHub](https://docs.openclaw.ai/es/tools/clawhub), y se pueden instalar
con un solo comando. Npm sigue siendo el valor predeterminado de lanzamiento para especificaciones de paquetes simples
mientras se despliegan las instalaciones de paquetes de ClawHub.ClawHub es la superficie canónica de descubrimiento para plugins de la comunidad. No abras
PRs solo de documentación únicamente para agregar tu plugin aquí por visibilidad; publícalo en
ClawHub en su lugar.

```
openclaw plugins install clawhub:<package-name>
```

Usa `openclaw plugins install <package-name>` para paquetes alojados en npm.

## [​](https://docs.openclaw.ai/es/plugins/community\#plugins-listados)  Plugins listados

### [​](https://docs.openclaw.ai/es/plugins/community\#apify)  Apify

Extrae datos de cualquier sitio web con más de 20 000 extractores listos para usar. Permite que tu agente
extraiga datos de Instagram, Facebook, TikTok, YouTube, Google Maps, Google
Search, sitios de comercio electrónico y más, solo con pedírselo.

- **npm:**`@apify/apify-openclaw-plugin`
- **repositorio:** [github.com/apify/apify-openclaw-plugin](https://github.com/apify/apify-openclaw-plugin)

```
openclaw plugins install @apify/apify-openclaw-plugin
```

### [​](https://docs.openclaw.ai/es/plugins/community\#codex-app-server-bridge)  Codex App Server Bridge

Puente independiente de OpenClaw para conversaciones de Codex App Server. Vincula un chat a
un hilo de Codex, habla con él con texto sin formato y contrólalo con comandos
nativos de chat para reanudar, planificar, revisar, seleccionar modelo, compaction y más.

- **npm:**`openclaw-codex-app-server`
- **repositorio:** [github.com/pwrdrvr/openclaw-codex-app-server](https://github.com/pwrdrvr/openclaw-codex-app-server)

```
openclaw plugins install openclaw-codex-app-server
```

### [​](https://docs.openclaw.ai/es/plugins/community\#dingtalk)  DingTalk

Integración de robot empresarial usando el modo Stream. Admite texto, imágenes y
mensajes de archivo mediante cualquier cliente de DingTalk.

- **npm:**`@largezhou/ddingtalk`
- **repositorio:** [github.com/largezhou/openclaw-dingtalk](https://github.com/largezhou/openclaw-dingtalk)

```
openclaw plugins install @largezhou/ddingtalk
```

### [​](https://docs.openclaw.ai/es/plugins/community\#lossless-claw-lcm)  Lossless Claw (LCM)

Plugin de gestión de contexto sin pérdida para OpenClaw. Resumen de conversaciones basado en DAG
con Compaction incremental: conserva la fidelidad completa del contexto
mientras reduce el uso de tokens.

- **npm:**`@martian-engineering/lossless-claw`
- **repositorio:** [github.com/Martian-Engineering/lossless-claw](https://github.com/Martian-Engineering/lossless-claw)

```
openclaw plugins install @martian-engineering/lossless-claw
```

### [​](https://docs.openclaw.ai/es/plugins/community\#opik)  Opik

Plugin oficial que exporta trazas de agentes a Opik. Supervisa el comportamiento del agente,
el costo, los tokens, los errores y más.

- **npm:**`@opik/opik-openclaw`
- **repositorio:** [github.com/comet-ml/opik-openclaw](https://github.com/comet-ml/opik-openclaw)

```
openclaw plugins install @opik/opik-openclaw
```

### [​](https://docs.openclaw.ai/es/plugins/community\#prometheus-avatar)  Prometheus Avatar

Dale a tu agente de OpenClaw un avatar Live2D con sincronización labial en tiempo real,
expresiones emocionales y texto a voz. Incluye herramientas para creadores para la generación
de recursos de IA y despliegue con un clic en Prometheus Marketplace. Actualmente en alfa.

- **npm:**`@prometheusavatar/openclaw-plugin`
- **repositorio:** [github.com/myths-labs/prometheus-avatar](https://github.com/myths-labs/prometheus-avatar)

```
openclaw plugins install @prometheusavatar/openclaw-plugin
```

### [​](https://docs.openclaw.ai/es/plugins/community\#qqbot)  QQbot

Conecta OpenClaw a QQ mediante la API de QQ Bot. Admite chats privados, menciones
de grupo, mensajes de canal y medios enriquecidos, incluidos voz, imágenes, videos
y archivos.Las versiones actuales de OpenClaw incluyen QQ Bot. Usa la configuración incluida en
[QQ Bot](https://docs.openclaw.ai/es/channels/qqbot) para instalaciones normales; instala este plugin externo solo
cuando quieras intencionalmente el paquete independiente mantenido por Tencent.

- **npm:**`@tencent-connect/openclaw-qqbot`
- **repositorio:** [github.com/tencent-connect/openclaw-qqbot](https://github.com/tencent-connect/openclaw-qqbot)

```
openclaw plugins install @tencent-connect/openclaw-qqbot
```

### [​](https://docs.openclaw.ai/es/plugins/community\#wecom)  wecom

Plugin de canal WeCom para OpenClaw del equipo de Tencent WeCom. Impulsado por
conexiones persistentes de WeCom Bot WebSocket, admite mensajes directos y chats
grupales, respuestas en streaming, mensajería proactiva, procesamiento de imágenes/archivos, formato
Markdown, control de acceso integrado y skills de documentos/reuniones/mensajería.

- **npm:**`@wecom/wecom-openclaw-plugin`
- **repositorio:** [github.com/WecomTeam/wecom-openclaw-plugin](https://github.com/WecomTeam/wecom-openclaw-plugin)

```
openclaw plugins install @wecom/wecom-openclaw-plugin
```

### [​](https://docs.openclaw.ai/es/plugins/community\#yuanbao)  Yuanbao

Plugin de canal Yuanbao para OpenClaw del equipo de Tencent Yuanbao. Impulsado por
conexiones persistentes WebSocket, admite mensajes directos y chats grupales,
respuestas en streaming, mensajería proactiva, procesamiento de imágenes/archivos/audio/video,
formato Markdown, control de acceso integrado y menús de comandos de barra.

- **npm:**`openclaw-plugin-yuanbao`
- **repositorio:** [github.com/YuanbaoTeam/yuanbao-openclaw-plugin](https://github.com/YuanbaoTeam/yuanbao-openclaw-plugin)

```
openclaw plugins install openclaw-plugin-yuanbao
```

## [​](https://docs.openclaw.ai/es/plugins/community\#env%C3%ADa-tu-plugin)  Envía tu plugin

Damos la bienvenida a plugins de la comunidad que sean útiles, documentados y seguros de operar.

1

[Navigate to header](https://docs.openclaw.ai/es/plugins/community#)

Publica en ClawHub o npm

Tu plugin debe poder instalarse mediante `openclaw plugins install \<package-name\>`.
Publica en [ClawHub](https://docs.openclaw.ai/es/tools/clawhub), a menos que necesites específicamente una
distribución solo por npm.
Consulta [Crear Plugins](https://docs.openclaw.ai/es/plugins/building-plugins) para ver la guía completa.

2

[Navigate to header](https://docs.openclaw.ai/es/plugins/community#)

Alójalo en GitHub

El código fuente debe estar en un repositorio público con documentación de configuración y un rastreador
de incidencias.

3

[Navigate to header](https://docs.openclaw.ai/es/plugins/community#)

Usa PRs de documentación solo para cambios en la documentación fuente

No necesitas un PR de documentación solo para que tu plugin sea detectable. Publícalo
en ClawHub en su lugar.Abre un PR de documentación solo cuando la documentación fuente de OpenClaw necesite un cambio real
de contenido, como corregir instrucciones de instalación o agregar documentación
entre repositorios que pertenezca al conjunto principal de documentación.

## [​](https://docs.openclaw.ai/es/plugins/community\#est%C3%A1ndar-de-calidad)  Estándar de calidad

| Requisito | Por qué |
| --- | --- |
| Publicado en ClawHub o npm | Los usuarios necesitan que `openclaw plugins install` funcione |
| Repositorio público de GitHub | Revisión de código fuente, seguimiento de incidencias, transparencia |
| Documentación de configuración y uso | Los usuarios necesitan saber cómo configurarlo |
| Mantenimiento activo | Actualizaciones recientes o gestión receptiva de incidencias |

Los envoltorios de bajo esfuerzo, la propiedad poco clara o los paquetes sin mantenimiento pueden ser rechazados.

## [​](https://docs.openclaw.ai/es/plugins/community\#relacionado)  Relacionado

- [Instalar y configurar Plugins](https://docs.openclaw.ai/es/tools/plugin) — cómo instalar cualquier plugin
- [Crear Plugins](https://docs.openclaw.ai/es/plugins/building-plugins) — crea el tuyo
- [Manifiesto de Plugin](https://docs.openclaw.ai/es/plugins/manifest) — esquema del manifiesto

[Manage plugins](https://docs.openclaw.ai/es/plugins/manage-plugins) [Inventario de Plugin](https://docs.openclaw.ai/es/plugins/plugin-inventory)

Ctrl+I