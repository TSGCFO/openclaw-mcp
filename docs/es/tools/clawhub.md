---
source_url: https://docs.openclaw.ai/es/tools/clawhub
title: "ClawHub - OpenClaw"
---

[Saltar al contenido principal](https://docs.openclaw.ai/es/tools/clawhub#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/es)

![ES](https://d3gk2c5xim1je2.cloudfront.net/flags/ES.svg)

Español

Buscar...

Ctrl K

Buscar...

Navigation

Skills

ClawHub

[Get started](https://docs.openclaw.ai/es) [Install](https://docs.openclaw.ai/es/install) [Channels](https://docs.openclaw.ai/es/channels) [Agents](https://docs.openclaw.ai/es/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/es/tools) [Models](https://docs.openclaw.ai/es/providers) [Platforms](https://docs.openclaw.ai/es/platforms) [Gateway & Ops](https://docs.openclaw.ai/es/gateway) [Reference](https://docs.openclaw.ai/es/cli) [Help](https://docs.openclaw.ai/es/help)

En esta página

- [Inicio rápido](https://docs.openclaw.ai/es/tools/clawhub#inicio-r%C3%A1pido)
- [Flujos nativos de OpenClaw](https://docs.openclaw.ai/es/tools/clawhub#flujos-nativos-de-openclaw)
- [Qué es ClawHub](https://docs.openclaw.ai/es/tools/clawhub#qu%C3%A9-es-clawhub)
- [Espacio de trabajo y carga de skills](https://docs.openclaw.ai/es/tools/clawhub#espacio-de-trabajo-y-carga-de-skills)
- [Funciones del servicio](https://docs.openclaw.ai/es/tools/clawhub#funciones-del-servicio)
- [Seguridad y moderación](https://docs.openclaw.ai/es/tools/clawhub#seguridad-y-moderaci%C3%B3n)
- [CLI de ClawHub](https://docs.openclaw.ai/es/tools/clawhub#cli-de-clawhub)
- [Opciones globales](https://docs.openclaw.ai/es/tools/clawhub#opciones-globales)
- [Comandos](https://docs.openclaw.ai/es/tools/clawhub#comandos)
- [Flujos de trabajo comunes](https://docs.openclaw.ai/es/tools/clawhub#flujos-de-trabajo-comunes)
- [Metadatos del paquete de Plugin](https://docs.openclaw.ai/es/tools/clawhub#metadatos-del-paquete-de-plugin)
- [Versionado, archivo de bloqueo y telemetría](https://docs.openclaw.ai/es/tools/clawhub#versionado-archivo-de-bloqueo-y-telemetr%C3%ADa)
- [Variables de entorno](https://docs.openclaw.ai/es/tools/clawhub#variables-de-entorno)
- [Relacionado](https://docs.openclaw.ai/es/tools/clawhub#relacionado)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

ClawHub es el registro público de **skills y plugins de OpenClaw**.

- Usa comandos nativos de `openclaw` para buscar, instalar y actualizar skills, y para instalar plugins desde ClawHub.
- Usa la CLI separada `clawhub` para flujos de autenticación del registro, publicación, eliminación/restauración y sincronización.

Sitio: [clawhub.ai](https://clawhub.ai/)

## [​](https://docs.openclaw.ai/es/tools/clawhub\#inicio-r%C3%A1pido)  Inicio rápido

1

[Navigate to header](https://docs.openclaw.ai/es/tools/clawhub#)

Buscar

```
openclaw skills search "calendar"
```

2

[Navigate to header](https://docs.openclaw.ai/es/tools/clawhub#)

Instalar

```
openclaw skills install <skill-slug>
```

3

[Navigate to header](https://docs.openclaw.ai/es/tools/clawhub#)

Usar

Inicia una nueva sesión de OpenClaw: detectará la nueva skill.

4

[Navigate to header](https://docs.openclaw.ai/es/tools/clawhub#)

Publicar (opcional)

Para flujos autenticados con el registro (publicar, sincronizar, gestionar), instala
la CLI separada `clawhub`:

```
npm i -g clawhub
# or
pnpm add -g clawhub
```

## [​](https://docs.openclaw.ai/es/tools/clawhub\#flujos-nativos-de-openclaw)  Flujos nativos de OpenClaw

- Skills

- Plugins


```
openclaw skills search "calendar"
openclaw skills install <skill-slug>
openclaw skills update --all
```

Los comandos nativos de `openclaw` instalan en tu espacio de trabajo activo y
conservan los metadatos de origen para que las llamadas posteriores a `update` puedan seguir usando ClawHub.

```
openclaw plugins search "calendar"
openclaw plugins install clawhub:<package>
openclaw plugins update --all
```

`plugins search` consulta el catálogo de plugins de ClawHub e imprime nombres
de paquetes listos para instalar. Usa `clawhub:<package>` cuando quieras resolución de ClawHub.
Las especificaciones de plugins compatibles con npm sin prefijo se instalan desde npm durante la transición de lanzamiento:

```
openclaw plugins install openclaw-codex-app-server
```

`npm:<package>` también es solo para npm y es útil cuando una especificación podría
ser ambigua:

```
openclaw plugins install npm:openclaw-codex-app-server
```

Las instalaciones de plugins validan la compatibilidad anunciada de `pluginApi` y
`minGatewayVersion` antes de que se ejecute la instalación del archivo, de modo que
los hosts incompatibles fallan de forma cerrada y temprana en lugar de instalar
parcialmente el paquete. Cuando una versión de paquete publica un artefacto ClawPack,
OpenClaw prefiere el `.tgz` exacto de npm-pack subido, verifica el encabezado de resumen de ClawHub
y los bytes descargados, y registra el tipo de artefacto, la integridad de npm,
el shasum de npm, el nombre del tarball y los metadatos del resumen de ClawPack para actualizaciones
posteriores. Las versiones de paquetes más antiguas sin metadatos de ClawPack siguen usando la
ruta heredada de verificación del archivo de paquete.

`openclaw plugins install clawhub:...` solo acepta familias de plugins
instalables. Si un paquete de ClawHub es en realidad una skill, OpenClaw se detiene y
te indica usar `openclaw skills install <slug>` en su lugar.Las instalaciones anónimas de plugins de ClawHub también fallan de forma cerrada para paquetes privados.
Los canales comunitarios u otros no oficiales aún pueden instalarse, pero OpenClaw
advierte para que los operadores puedan revisar el origen y la verificación antes de habilitarlos.

## [​](https://docs.openclaw.ai/es/tools/clawhub\#qu%C3%A9-es-clawhub)  Qué es ClawHub

- Un registro público de skills y plugins de OpenClaw.
- Un almacén versionado de paquetes de skills y metadatos.
- Una superficie de descubrimiento para búsqueda, etiquetas y señales de uso.

Una skill típica es un paquete versionado de archivos que incluye:

- Un archivo `SKILL.md` con la descripción y el uso principales.
- Configuraciones, scripts o archivos auxiliares opcionales usados por la skill.
- Metadatos como etiquetas, resumen y requisitos de instalación.

ClawHub usa metadatos para impulsar el descubrimiento y exponer de forma segura las
capacidades de las skills. El registro rastrea señales de uso (estrellas, descargas) para
mejorar la clasificación y la visibilidad. Cada publicación crea una nueva versión
semver, y el registro conserva el historial de versiones para que los usuarios puedan auditar
los cambios.

## [​](https://docs.openclaw.ai/es/tools/clawhub\#espacio-de-trabajo-y-carga-de-skills)  Espacio de trabajo y carga de skills

La CLI separada `clawhub` también instala skills en `./skills` dentro de
tu directorio de trabajo actual. Si hay un espacio de trabajo de OpenClaw configurado,
`clawhub` recurre a ese espacio de trabajo salvo que sobrescribas `--workdir`
(o `CLAWHUB_WORKDIR`). OpenClaw carga las skills del espacio de trabajo desde
`<workspace>/skills` y las detecta en la **siguiente** sesión.Si ya usas `~/.openclaw/skills` o skills incluidas, las skills del espacio de trabajo
tienen prioridad. Para más detalles sobre cómo se cargan, comparten y controlan
las skills, consulta [Skills](https://docs.openclaw.ai/es/tools/skills).

## [​](https://docs.openclaw.ai/es/tools/clawhub\#funciones-del-servicio)  Funciones del servicio

| Función | Notas |
| --- | --- |
| Navegación pública | Las Skills y su contenido de `SKILL.md` se pueden ver públicamente. |
| Búsqueda | Impulsada por embeddings (búsqueda vectorial), no solo palabras clave. |
| Versionado | Semver, registros de cambios y etiquetas (incluida `latest`). |
| Descargas | Zip por versión. |
| Estrellas y comentarios | Comentarios de la comunidad. |
| Resúmenes de análisis de seguridad | Las páginas de detalle muestran el estado del análisis más reciente antes de instalar o descargar. |
| Páginas de detalle de analizadores | Los resultados de VirusTotal, ClawScan y análisis estático tienen enlaces profundos. |
| Panel de recuperación del propietario | Los publicadores pueden ver contenido propio retenido por análisis desde `/dashboard`. |
| Reanálisis solicitados por el propietario | Los propietarios pueden solicitar reanálisis limitados para recuperación de falsos positivos. |
| Moderación | Aprobaciones y auditorías. |
| API apta para CLI | Adecuada para automatización y scripting. |

## [​](https://docs.openclaw.ai/es/tools/clawhub\#seguridad-y-moderaci%C3%B3n)  Seguridad y moderación

ClawHub es abierto por defecto: cualquiera puede subir skills, pero una cuenta de GitHub
debe tener **al menos una semana de antigüedad** para publicar. Esto ralentiza
los abusos sin bloquear a colaboradores legítimos.

Análisis de seguridad

ClawHub ejecuta comprobaciones de seguridad automatizadas en skills publicadas y lanzamientos
de plugins. Las páginas públicas de detalle resumen el resultado actual, y las filas de analizadores
enlazan a páginas de detalle dedicadas para VirusTotal, ClawScan y análisis
estático.Los lanzamientos retenidos por análisis o bloqueados pueden no estar disponibles en el catálogo público y
las superficies de instalación mientras siguen visibles para su propietario en `/dashboard`.

Reportes

- Cualquier usuario con sesión iniciada puede reportar una skill.
- Los motivos del reporte son obligatorios y quedan registrados.
- Cada usuario puede tener hasta 20 reportes activos a la vez.
- Las Skills con más de 3 reportes únicos se ocultan automáticamente por defecto.

Moderación

- Los moderadores pueden ver skills ocultas, volver a mostrarlas, eliminarlas o prohibir usuarios.
- Abusar de la función de reportes puede derivar en prohibiciones de cuenta.
- ¿Te interesa convertirte en moderador? Pregunta en el Discord de OpenClaw y contacta con un moderador o mantenedor.

## [​](https://docs.openclaw.ai/es/tools/clawhub\#cli-de-clawhub)  CLI de ClawHub

Solo necesitas esto para flujos autenticados con el registro, como
publicar/sincronizar.

### [​](https://docs.openclaw.ai/es/tools/clawhub\#opciones-globales)  Opciones globales

[​](https://docs.openclaw.ai/es/tools/clawhub#param-workdir-dir)

--workdir <dir>

string

Directorio de trabajo. Predeterminado: directorio actual; recurre al espacio de trabajo de OpenClaw.

[​](https://docs.openclaw.ai/es/tools/clawhub#param-dir-dir)

--dir <dir>

string

predeterminado:"skills"

Directorio de skills, relativo a workdir.

[​](https://docs.openclaw.ai/es/tools/clawhub#param-site-url)

--site <url>

string

URL base del sitio (inicio de sesión en navegador).

[​](https://docs.openclaw.ai/es/tools/clawhub#param-registry-url)

--registry <url>

string

URL base de la API del registro.

[​](https://docs.openclaw.ai/es/tools/clawhub#param-no-input)

--no-input

boolean

Deshabilita los prompts (no interactivo).

[​](https://docs.openclaw.ai/es/tools/clawhub#param-v-cli-version)

-V, --cli-version

boolean

Imprime la versión de la CLI.

### [​](https://docs.openclaw.ai/es/tools/clawhub\#comandos)  Comandos

Auth (login / logout / whoami)

```
clawhub login              # browser flow
clawhub login --token <token>
clawhub logout
clawhub whoami
```

Opciones de inicio de sesión:

- `--token <token>` — pega un token de API.
- `--label <label>` — etiqueta almacenada para tokens de inicio de sesión en navegador (predeterminado: `CLI token`).
- `--no-browser` — no abrir un navegador (requiere `--token`).

Buscar

```
clawhub search "query"
```

Busca skills. Para descubrir plugins/paquetes, usa `clawhub package explore`.

- `--limit <n>` — resultados máximos.

Explorar / inspeccionar plugins

```
clawhub package explore --family code-plugin
clawhub package explore "episodic-claw" --family code-plugin
clawhub package inspect episodic-claw
```

`package explore` y `package inspect` son las superficies de la CLI de ClawHub para descubrir plugins/paquetes e inspeccionar metadatos. Las instalaciones nativas de OpenClaw siguen usando `openclaw plugins install clawhub:<package>`.Opciones:

- `--family skill|code-plugin|bundle-plugin` — filtrar familia de paquetes.
- `--official` — mostrar solo paquetes oficiales.
- `--executes-code` — mostrar solo paquetes que ejecutan código.
- `--version <version>` / `--tag <tag>` — inspeccionar una versión de paquete específica.
- `--versions`, `--files`, `--file <path>` — inspeccionar el historial y los archivos del paquete.
- `--json` — salida legible por máquina.

Instalar / actualizar / listar

```
clawhub install <slug>
clawhub update <slug>
clawhub update --all
clawhub list
```

Opciones:

- `--version <version>` — instalar o actualizar a una versión específica (solo un slug en `update`).
- `--force` — sobrescribir si la carpeta ya existe, o cuando los archivos locales no coinciden con ninguna versión publicada.
- `clawhub list` lee `.clawhub/lock.json`.

Publicar skills

```
clawhub skill publish <path>
```

Opciones:

- `--slug <slug>` — slug de la skill.
- `--name <name>` — nombre para mostrar.
- `--version <version>` — versión semver.
- `--changelog <text>` — texto del registro de cambios (puede estar vacío).
- `--tags <tags>` — etiquetas separadas por comas (predeterminado: `latest`).

Publicar plugins

```
clawhub package publish <source>
```

`<source>` puede ser una carpeta local, `owner/repo`, `owner/repo@ref` o una
URL de GitHub.Opciones:

- `--dry-run` — construir el plan exacto de publicación sin subir nada.
- `--json` — emitir salida legible por máquina para CI.
- `--source-repo`, `--source-commit`, `--source-ref` — sobrescrituras opcionales cuando la detección automática no basta.

Solicitar reanálisis

```
clawhub skill rescan <slug>
clawhub skill rescan <slug> --yes --json

clawhub package rescan <name>
clawhub package rescan <name> --yes --json
```

Los comandos de reanálisis requieren un token de propietario con sesión iniciada y apuntan a la versión
publicada más reciente de la skill o al lanzamiento del plugin. En ejecuciones no interactivas, pasa
`--yes`.Las respuestas JSON incluyen el tipo de destino, nombre, versión, estado del reanálisis y
recuentos de solicitudes restantes/máximas para esa versión o lanzamiento.

Eliminar / restaurar (propietario o administrador)

```
clawhub delete <slug> --yes
clawhub undelete <slug> --yes
```

Sincronizar (analizar local + publicar nuevo o actualizado)

```
clawhub sync
```

Opciones:

- `--root <dir...>` — raíces de análisis adicionales.
- `--all` — subir todo sin prompts.
- `--dry-run` — mostrar qué se subiría.
- `--bump <type>` — `patch|minor|major` para actualizaciones (predeterminado: `patch`).
- `--changelog <text>` — registro de cambios para actualizaciones no interactivas.
- `--tags <tags>` — etiquetas separadas por comas (predeterminado: `latest`).
- `--concurrency <n>` — comprobaciones del registro (predeterminado: `4`).

## [​](https://docs.openclaw.ai/es/tools/clawhub\#flujos-de-trabajo-comunes)  Flujos de trabajo comunes

- Search

- Find a plugin

- Install

- Update all

- Publish a single skill

- Sync many skills

- Publish a plugin from GitHub


```
clawhub search "postgres backups"
```

```
clawhub package explore --family code-plugin
clawhub package explore "memory" --family code-plugin
clawhub package inspect episodic-claw
```

```
clawhub install my-skill-pack
```

```
clawhub update --all
```

```
clawhub skill publish ./my-skill --slug my-skill --name "My Skill" --version 1.0.0 --tags latest
```

```
clawhub sync --all
```

```
clawhub package publish your-org/your-plugin --dry-run
clawhub package publish your-org/your-plugin
clawhub package publish your-org/your-plugin@v1.0.0
clawhub package publish https://github.com/your-org/your-plugin
```

### [​](https://docs.openclaw.ai/es/tools/clawhub\#metadatos-del-paquete-de-plugin)  Metadatos del paquete de Plugin

Los plugins de código deben incluir los metadatos requeridos de OpenClaw en
`package.json`:

```
{
  "name": "@myorg/openclaw-my-plugin",
  "version": "1.0.0",
  "type": "module",
  "openclaw": {
    "extensions": ["./src/index.ts"],
    "runtimeExtensions": ["./dist/index.js"],
    "compat": {
      "pluginApi": ">=2026.3.24-beta.2",
      "minGatewayVersion": "2026.3.24-beta.2"
    },
    "build": {
      "openclawVersion": "2026.3.24-beta.2",
      "pluginSdkVersion": "2026.3.24-beta.2"
    }
  }
}
```

Los paquetes publicados deberían incluir **JavaScript compilado** y apuntar
`runtimeExtensions` a esa salida. Las instalaciones desde un checkout de Git aún pueden
recurrir al código fuente de TypeScript cuando no existan archivos compilados, pero las entradas de runtime compiladas
evitan la compilación de TypeScript en runtime durante el inicio, doctor y
las rutas de carga de plugins.

## [​](https://docs.openclaw.ai/es/tools/clawhub\#versionado-archivo-de-bloqueo-y-telemetr%C3%ADa)  Versionado, archivo de bloqueo y telemetría

Versioning and tags

- Cada publicación crea una nueva `SkillVersion` **semver**.
- Las etiquetas (como `latest`) apuntan a una versión; mover etiquetas te permite revertir.
- Los registros de cambios se adjuntan por versión y pueden estar vacíos al sincronizar o publicar actualizaciones.

Local changes vs registry versions

Las actualizaciones comparan el contenido local de la skill con las versiones del registro usando un
hash de contenido. Si los archivos locales no coinciden con ninguna versión publicada, la
CLI pregunta antes de sobrescribir (o requiere `--force` en
ejecuciones no interactivas).

Sync scanning and fallback roots

`clawhub sync` escanea primero tu directorio de trabajo actual. Si no se encuentran skills,
recurre a ubicaciones heredadas conocidas (por ejemplo,
`~/openclaw/skills` y `~/.openclaw/skills`). Esto está diseñado para
encontrar instalaciones de skills antiguas sin flags adicionales.

Storage and lockfile

- Las skills instaladas se registran en `.clawhub/lock.json` dentro de tu directorio de trabajo.
- Los tokens de autenticación se almacenan en el archivo de configuración de la CLI de ClawHub (anula mediante `CLAWHUB_CONFIG_PATH`).

Telemetry (install counts)

Cuando ejecutas `clawhub sync` con sesión iniciada, la CLI envía una instantánea
mínima para calcular los conteos de instalaciones. Puedes desactivar esto por completo:

```
export CLAWHUB_DISABLE_TELEMETRY=1
```

## [​](https://docs.openclaw.ai/es/tools/clawhub\#variables-de-entorno)  Variables de entorno

| Variable | Efecto |
| --- | --- |
| `CLAWHUB_SITE` | Anula la URL del sitio. |
| `CLAWHUB_REGISTRY` | Anula la URL de la API del registro. |
| `CLAWHUB_CONFIG_PATH` | Anula dónde la CLI almacena el token/configuración. |
| `CLAWHUB_WORKDIR` | Anula el directorio de trabajo predeterminado. |
| `CLAWHUB_DISABLE_TELEMETRY=1` | Desactiva la telemetría en `sync`. |

## [​](https://docs.openclaw.ai/es/tools/clawhub\#relacionado)  Relacionado

- [Plugins de la comunidad](https://docs.openclaw.ai/es/plugins/community)
- [Plugins](https://docs.openclaw.ai/es/tools/plugin)
- [Skills](https://docs.openclaw.ai/es/tools/skills)

[Slash commands](https://docs.openclaw.ai/es/tools/slash-commands) [OpenProse](https://docs.openclaw.ai/es/prose)

Ctrl+I