---
source_url: https://docs.openclaw.ai/es/platforms/mac/logging
title: "Registro en macOS - OpenClaw"
---

[Saltar al contenido principal](https://docs.openclaw.ai/es/platforms/mac/logging#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/es)

![ES](https://d3gk2c5xim1je2.cloudfront.net/flags/ES.svg)

Español

Buscar...

Ctrl K

Buscar...

Navigation

Runtime

Registro en macOS

[Get started](https://docs.openclaw.ai/es) [Install](https://docs.openclaw.ai/es/install) [Channels](https://docs.openclaw.ai/es/channels) [Agents](https://docs.openclaw.ai/es/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/es/tools) [Models](https://docs.openclaw.ai/es/providers) [Platforms](https://docs.openclaw.ai/es/platforms) [Gateway & Ops](https://docs.openclaw.ai/es/gateway) [Reference](https://docs.openclaw.ai/es/cli) [Help](https://docs.openclaw.ai/es/help)

En esta página

- [Registro (macOS)](https://docs.openclaw.ai/es/platforms/mac/logging#registro-macos)
- [Registro rotativo de diagnóstico en archivo (panel Debug)](https://docs.openclaw.ai/es/platforms/mac/logging#registro-rotativo-de-diagn%C3%B3stico-en-archivo-panel-debug)
- [Datos privados del registro unificado en macOS](https://docs.openclaw.ai/es/platforms/mac/logging#datos-privados-del-registro-unificado-en-macos)
- [Habilitar para OpenClaw (ai.openclaw)](https://docs.openclaw.ai/es/platforms/mac/logging#habilitar-para-openclaw-ai-openclaw)
- [Deshabilitar después de depurar](https://docs.openclaw.ai/es/platforms/mac/logging#deshabilitar-despu%C3%A9s-de-depurar)
- [Relacionado](https://docs.openclaw.ai/es/platforms/mac/logging#relacionado)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/es/platforms/mac/logging\#registro-macos)  Registro (macOS)

## [​](https://docs.openclaw.ai/es/platforms/mac/logging\#registro-rotativo-de-diagn%C3%B3stico-en-archivo-panel-debug)  Registro rotativo de diagnóstico en archivo (panel Debug)

OpenClaw enruta los registros de la app de macOS mediante swift-log (registro unificado de forma predeterminada) y puede escribir un registro local rotativo en archivo cuando necesitas una captura duradera.

- Nivel de detalle: **panel Debug → Logs → App logging → Verbosity**
- Habilitar: **panel Debug → Logs → App logging → “Write rolling diagnostics log (JSONL)”**
- Ubicación: `~/Library/Logs/OpenClaw/diagnostics.jsonl` (rota automáticamente; los archivos antiguos se sufijan con `.1`, `.2`, …)
- Borrar: **panel Debug → Logs → App logging → “Clear”**

Notas:

- Esto está **desactivado por defecto**. Habilítalo solo mientras estés depurando activamente.
- Trata el archivo como sensible; no lo compartas sin revisarlo.

## [​](https://docs.openclaw.ai/es/platforms/mac/logging\#datos-privados-del-registro-unificado-en-macos)  Datos privados del registro unificado en macOS

El registro unificado redacta la mayoría de las cargas útiles salvo que un subsistema active `privacy -off`. Según el artículo de Peter sobre macOS [logging privacy shenanigans](https://steipete.me/posts/2025/logging-privacy-shenanigans) (2025), esto se controla mediante un plist en `/Library/Preferences/Logging/Subsystems/` indexado por el nombre del subsistema. Solo las nuevas entradas del registro recogen la flag, así que actívala antes de reproducir un problema.

## [​](https://docs.openclaw.ai/es/platforms/mac/logging\#habilitar-para-openclaw-ai-openclaw)  Habilitar para OpenClaw (`ai.openclaw`)

- Escribe primero el plist en un archivo temporal y luego instálalo atómicamente como root:

```
cat <<'EOF' >/tmp/ai.openclaw.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>DEFAULT-OPTIONS</key>
    <dict>
        <key>Enable-Private-Data</key>
        <true/>
    </dict>
</dict>
</plist>
EOF
sudo install -m 644 -o root -g wheel /tmp/ai.openclaw.plist /Library/Preferences/Logging/Subsystems/ai.openclaw.plist
```

- No hace falta reiniciar; logd detecta el archivo rápidamente, pero solo las nuevas líneas del registro incluirán cargas útiles privadas.
- Visualiza la salida enriquecida con el helper existente, por ejemplo `./scripts/clawlog.sh --category WebChat --last 5m`.

## [​](https://docs.openclaw.ai/es/platforms/mac/logging\#deshabilitar-despu%C3%A9s-de-depurar)  Deshabilitar después de depurar

- Elimina la sobrescritura: `sudo rm /Library/Preferences/Logging/Subsystems/ai.openclaw.plist`.
- Opcionalmente ejecuta `sudo log config --reload` para forzar a logd a descartar inmediatamente la sobrescritura.
- Recuerda que esta superficie puede incluir números de teléfono y cuerpos de mensajes; mantén el plist activo solo mientras necesites realmente ese nivel adicional de detalle.

## [​](https://docs.openclaw.ai/es/platforms/mac/logging\#relacionado)  Relacionado

- [App de macOS](https://docs.openclaw.ai/es/platforms/macos)
- [Registro del Gateway](https://docs.openclaw.ai/es/gateway/logging)

[Comprobaciones de estado (macOS)](https://docs.openclaw.ai/es/platforms/mac/health) [Control remoto](https://docs.openclaw.ai/es/platforms/mac/remote)

Ctrl+I