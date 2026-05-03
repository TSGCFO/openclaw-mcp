---
source_url: https://docs.openclaw.ai/zh-CN/plugins/voice-call
title: "\u8bed\u97f3\u901a\u8bdd\u63d2\u4ef6 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/plugins/voice-call#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

扩展

语音通话插件

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [快速开始](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)
- [配置](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E9%85%8D%E7%BD%AE)
- [会话范围](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E4%BC%9A%E8%AF%9D%E8%8C%83%E5%9B%B4)
- [实时语音对话](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E5%AE%9E%E6%97%B6%E8%AF%AD%E9%9F%B3%E5%AF%B9%E8%AF%9D)
- [工具策略](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E5%B7%A5%E5%85%B7%E7%AD%96%E7%95%A5)
- [实时提供商示例](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E5%AE%9E%E6%97%B6%E6%8F%90%E4%BE%9B%E5%95%86%E7%A4%BA%E4%BE%8B)
- [流式转写](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E6%B5%81%E5%BC%8F%E8%BD%AC%E5%86%99)
- [流式传输提供商示例](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E6%B5%81%E5%BC%8F%E4%BC%A0%E8%BE%93%E6%8F%90%E4%BE%9B%E5%95%86%E7%A4%BA%E4%BE%8B)
- [通话的 TTS](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E9%80%9A%E8%AF%9D%E7%9A%84-tts)
- [TTS 示例](https://docs.openclaw.ai/zh-CN/plugins/voice-call#tts-%E7%A4%BA%E4%BE%8B)
- [入站通话](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E5%85%A5%E7%AB%99%E9%80%9A%E8%AF%9D)
- [按号码路由](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E6%8C%89%E5%8F%B7%E7%A0%81%E8%B7%AF%E7%94%B1)
- [语音输出契约](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E8%AF%AD%E9%9F%B3%E8%BE%93%E5%87%BA%E5%A5%91%E7%BA%A6)
- [对话启动行为](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E5%AF%B9%E8%AF%9D%E5%90%AF%E5%8A%A8%E8%A1%8C%E4%B8%BA)
- [Twilio 流断开宽限期](https://docs.openclaw.ai/zh-CN/plugins/voice-call#twilio-%E6%B5%81%E6%96%AD%E5%BC%80%E5%AE%BD%E9%99%90%E6%9C%9F)
- [过期通话清理器](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E8%BF%87%E6%9C%9F%E9%80%9A%E8%AF%9D%E6%B8%85%E7%90%86%E5%99%A8)
- [Webhook 安全](https://docs.openclaw.ai/zh-CN/plugins/voice-call#webhook-%E5%AE%89%E5%85%A8)
- [CLI](https://docs.openclaw.ai/zh-CN/plugins/voice-call#cli)
- [智能体工具](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E6%99%BA%E8%83%BD%E4%BD%93%E5%B7%A5%E5%85%B7)
- [Gateway 网关 RPC](https://docs.openclaw.ai/zh-CN/plugins/voice-call#gateway-%E7%BD%91%E5%85%B3-rpc)
- [故障排除](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)
- [设置因 webhook 暴露失败](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E8%AE%BE%E7%BD%AE%E5%9B%A0-webhook-%E6%9A%B4%E9%9C%B2%E5%A4%B1%E8%B4%A5)
- [提供商凭证失败](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E6%8F%90%E4%BE%9B%E5%95%86%E5%87%AD%E8%AF%81%E5%A4%B1%E8%B4%A5)
- [通话启动但提供商 webhook 未到达](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E9%80%9A%E8%AF%9D%E5%90%AF%E5%8A%A8%E4%BD%86%E6%8F%90%E4%BE%9B%E5%95%86-webhook-%E6%9C%AA%E5%88%B0%E8%BE%BE)
- [签名验证失败](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E7%AD%BE%E5%90%8D%E9%AA%8C%E8%AF%81%E5%A4%B1%E8%B4%A5)
- [Google Meet Twilio 加入失败](https://docs.openclaw.ai/zh-CN/plugins/voice-call#google-meet-twilio-%E5%8A%A0%E5%85%A5%E5%A4%B1%E8%B4%A5)
- [实时通话没有语音](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E5%AE%9E%E6%97%B6%E9%80%9A%E8%AF%9D%E6%B2%A1%E6%9C%89%E8%AF%AD%E9%9F%B3)
- [相关](https://docs.openclaw.ai/zh-CN/plugins/voice-call#%E7%9B%B8%E5%85%B3)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw 的语音通话通过插件实现。支持外呼通知、多轮对话、全双工实时语音、流式转写，以及带 allowlist 策略的呼入通话。**当前提供商：**`twilio`（Programmable Voice + Media Streams）、`telnyx`（Call Control v2）、`plivo`（Voice API + XML transfer + GetInput speech）、`mock`（开发/无网络）。

Voice Call 插件运行在 **Gateway 网关进程内部**。如果你使用远程 Gateway 网关，请在运行 Gateway 网关的机器上安装并配置该插件，然后重启 Gateway 网关以加载它。

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)  快速开始

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/plugins/voice-call#)

安装插件

- 从 npm 安装

- 从本地文件夹安装（开发）


```
openclaw plugins install @openclaw/voice-call
```

```
PLUGIN_SRC=./path/to/local/voice-call-plugin
openclaw plugins install "$PLUGIN_SRC"
cd "$PLUGIN_SRC" && pnpm install
```

如果 npm 报告 OpenClaw 拥有的包已弃用，则该包版本来自较旧的外部包发布线；请使用当前打包的 OpenClaw 构建，或使用本地文件夹路径，直到更新的 npm 包发布。之后重启 Gateway 网关，以便插件加载。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/plugins/voice-call#)

配置提供商和 webhook

在 `plugins.entries.voice-call.config` 下设置配置（完整结构见下方 [配置](https://docs.openclaw.ai/zh-CN/plugins/voice-call#configuration)）。至少需要：
`provider`、提供商凭据、`fromNumber`，以及一个可公开访问的 webhook URL。

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/plugins/voice-call#)

验证设置

```
openclaw voicecall setup
```

默认输出在聊天日志和终端中可读。它会检查插件是否启用、提供商凭据、webhook 暴露情况，以及是否只有一种音频模式（`streaming` 或 `realtime`）处于活动状态。脚本请使用 `--json`。

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/plugins/voice-call#)

冒烟测试

```
openclaw voicecall smoke
openclaw voicecall smoke --to "+15555550123"
```

两者默认都是空运行。添加 `--yes` 才会实际拨出一个简短的外呼通知通话：

```
openclaw voicecall smoke --to "+15555550123" --yes
```

对于 Twilio、Telnyx 和 Plivo，设置必须解析为 **公开 webhook URL**。如果 `publicUrl`、隧道 URL、Tailscale URL 或 serve 回退解析到 loopback 或私有网络地址空间，设置会失败，而不是启动一个无法接收运营商 webhook 的提供商。

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E9%85%8D%E7%BD%AE)  配置

如果 `enabled: true` 但所选提供商缺少凭据，Gateway 网关启动时会记录 setup-incomplete 警告并列出缺失键名，然后跳过启动运行时。命令、RPC 调用和智能体工具在使用时仍会返回确切缺失的提供商配置。

语音通话凭据接受 SecretRef。`plugins.entries.voice-call.config.twilio.authToken`、`plugins.entries.voice-call.config.realtime.providers.*.apiKey`、`plugins.entries.voice-call.config.streaming.providers.*.apiKey` 和 `plugins.entries.voice-call.config.tts.providers.*.apiKey` 会通过标准 SecretRef 表面解析；请参阅 [SecretRef 凭据表面](https://docs.openclaw.ai/zh-CN/reference/secretref-credential-surface)。

```
{
  plugins: {
    entries: {
      "voice-call": {
        enabled: true,
        config: {
          provider: "twilio", // or "telnyx" | "plivo" | "mock"
          fromNumber: "+15550001234", // or TWILIO_FROM_NUMBER for Twilio
          toNumber: "+15550005678",
          sessionScope: "per-phone", // per-phone | per-call
          numbers: {
            "+15550009999": {
              inboundGreeting: "Silver Fox Cards, how can I help?",
              responseSystemPrompt: "You are a concise baseball card specialist.",
              tts: {
                providers: {
                  openai: { voice: "alloy" },
                },
              },
            },
          },

          twilio: {
            accountSid: "ACxxxxxxxx",
            authToken: "...",
          },
          telnyx: {
            apiKey: "...",
            connectionId: "...",
            // Telnyx webhook public key from the Mission Control Portal
            // (Base64; can also be set via TELNYX_PUBLIC_KEY).
            publicKey: "...",
          },
          plivo: {
            authId: "MAxxxxxxxxxxxxxxxxxxxx",
            authToken: "...",
          },

          // Webhook server
          serve: {
            port: 3334,
            path: "/voice/webhook",
          },

          // Webhook security (recommended for tunnels/proxies)
          webhookSecurity: {
            allowedHosts: ["voice.example.com"],
            trustedProxyIPs: ["100.64.0.1"],
          },

          // Public exposure (pick one)
          // publicUrl: "https://example.ngrok.app/voice/webhook",
          // tunnel: { provider: "ngrok" },
          // tailscale: { mode: "funnel", path: "/voice/webhook" },

          outbound: {
            defaultMode: "notify", // notify | conversation
          },

          streaming: { enabled: true /* see Streaming transcription */ },
          realtime: { enabled: false /* see Realtime voice */ },
        },
      },
    },
  },
}
```

提供商暴露和安全说明

- Twilio、Telnyx 和 Plivo 都需要一个 **可公开访问** 的 webhook URL。
- `mock` 是本地开发提供商（无网络调用）。
- Telnyx 需要 `telnyx.publicKey`（或 `TELNYX_PUBLIC_KEY`），除非 `skipSignatureVerification` 为 true。
- `skipSignatureVerification` 仅用于本地测试。
- 在 ngrok 免费层上，将 `publicUrl` 设置为确切的 ngrok URL；签名验证始终强制执行。
- `tunnel.allowNgrokFreeTierLoopbackBypass: true` 仅在 `tunnel.provider="ngrok"` 且 `serve.bind` 是 loopback（ngrok 本地代理）时，允许带无效签名的 Twilio webhook。仅限本地开发。
- Ngrok 免费层 URL 可能会变化或添加插页行为；如果 `publicUrl` 漂移，Twilio 签名会失败。生产环境：优先使用稳定域名或 Tailscale funnel。

流式连接上限

- `streaming.preStartTimeoutMs` 会关闭从未发送有效 `start` 帧的 socket。
- `streaming.maxPendingConnections` 限制未认证 pre-start socket 总数。
- `streaming.maxPendingConnectionsPerIp` 限制每个源 IP 的未认证 pre-start socket 数量。
- `streaming.maxConnections` 限制已打开媒体流 socket 总数（pending + active）。

旧版配置迁移

使用 `provider: "log"`、`twilio.from` 或旧版 `streaming.*` OpenAI 键的较旧配置会由 `openclaw doctor --fix` 重写。运行时回退目前仍接受旧的 voice-call 键，但重写路径是 `openclaw doctor --fix`，兼容 shim 是临时的。自动迁移的 streaming 键：

- `streaming.sttProvider` → `streaming.provider`
- `streaming.openaiApiKey` → `streaming.providers.openai.apiKey`
- `streaming.sttModel` → `streaming.providers.openai.model`
- `streaming.silenceDurationMs` → `streaming.providers.openai.silenceDurationMs`
- `streaming.vadThreshold` → `streaming.providers.openai.vadThreshold`

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E4%BC%9A%E8%AF%9D%E8%8C%83%E5%9B%B4)  会话范围

默认情况下，Voice Call 使用 `sessionScope: "per-phone"`，因此来自同一来电者的重复通话会保留对话记忆。当每个运营商通话都应从全新上下文开始时，请设置 `sessionScope: "per-call"`，例如接待、预订、IVR，或同一电话号码可能代表不同会议的 Google Meet 桥接流程。

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E5%AE%9E%E6%97%B6%E8%AF%AD%E9%9F%B3%E5%AF%B9%E8%AF%9D)  实时语音对话

`realtime` 为实时通话音频选择全双工实时语音提供商。它与 `streaming` 分开，后者只会将音频转发给实时转写提供商。

`realtime.enabled` 不能与 `streaming.enabled` 组合使用。每个通话请选择一种音频模式。

当前运行时行为：

- Twilio Media Streams 支持 `realtime.enabled`。
- `realtime.provider` 是可选的。如果未设置，Voice Call 会使用第一个注册的实时语音提供商。
- 内置实时语音提供商：Google Gemini Live（`google`）和 OpenAI（`openai`），由它们的提供商插件注册。
- 提供商拥有的原始配置位于 `realtime.providers.<providerId>` 下。
- Voice Call 默认公开共享的 `openclaw_agent_consult` 实时工具。当来电者请求更深入推理、当前信息或常规 OpenClaw 工具时，实时模型可以调用它。
- `realtime.fastContext.enabled` 默认关闭。启用后，Voice Call 会先为 consult 问题搜索已索引的记忆/会话上下文，并在 `realtime.fastContext.timeoutMs` 内将这些片段返回给实时模型；只有当 `realtime.fastContext.fallbackToConsult` 为 true 时，才会回退到完整 consult 智能体。
- 如果 `realtime.provider` 指向未注册的提供商，或根本没有注册实时语音提供商，Voice Call 会记录警告并跳过实时媒体，而不是让整个插件失败。
- Consult 会话键会在可用时复用已存储的通话会话，然后回退到配置的 `sessionScope`（默认为 `per-phone`，隔离通话则为 `per-call`）。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E5%B7%A5%E5%85%B7%E7%AD%96%E7%95%A5)  工具策略

`realtime.toolPolicy` 控制 consult 运行：

| 策略 | 行为 |
| --- | --- |
| `safe-read-only` | 公开 consult 工具，并将常规智能体限制为 `read`、`web_search`、`web_fetch`、`x_search`、`memory_search` 和 `memory_get`。 |
| `owner` | 公开 consult 工具，并允许常规智能体使用普通智能体工具策略。 |
| `none` | 不公开 consult 工具。自定义 `realtime.tools` 仍会透传给实时提供商。 |

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E5%AE%9E%E6%97%B6%E6%8F%90%E4%BE%9B%E5%95%86%E7%A4%BA%E4%BE%8B)  实时提供商示例

- Google Gemini Live

- OpenAI


默认值：API key 来自 `realtime.providers.google.apiKey`、`GEMINI_API_KEY` 或 `GOOGLE_GENERATIVE_AI_API_KEY`；模型为 `gemini-2.5-flash-native-audio-preview-12-2025`；语音为 `Kore`。

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          provider: "twilio",
          inboundPolicy: "allowlist",
          allowFrom: ["+15550005678"],
          realtime: {
            enabled: true,
            provider: "google",
            instructions: "Speak briefly. Call openclaw_agent_consult before using deeper tools.",
            toolPolicy: "safe-read-only",
            providers: {
              google: {
                apiKey: "${GEMINI_API_KEY}",
                model: "gemini-2.5-flash-native-audio-preview-12-2025",
                voice: "Kore",
              },
            },
          },
        },
      },
    },
  },
}
```

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          realtime: {
            enabled: true,
            provider: "openai",
            providers: {
              openai: { apiKey: "${OPENAI_API_KEY}" },
            },
          },
        },
      },
    },
  },
}
```

有关特定提供商的实时语音选项，请参阅 [Google 提供商](https://docs.openclaw.ai/zh-CN/providers/google) 和 [OpenAI provider](https://docs.openclaw.ai/zh-CN/providers/openai)。

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E6%B5%81%E5%BC%8F%E8%BD%AC%E5%86%99)  流式转写

`streaming` 为实时通话音频选择实时转写提供商。当前运行时行为：

- `streaming.provider` 是可选的。如果未设置，Voice Call 会使用第一个已注册的实时转录提供商。
- 内置实时转录提供商：Deepgram（`deepgram`）、ElevenLabs（`elevenlabs`）、Mistral（`mistral`）、OpenAI（`openai`）和 xAI（`xai`），由各自的提供商插件注册。
- 提供商拥有的原始配置位于 `streaming.providers.<providerId>` 下。
- Twilio 发送已接受的流 `start` 消息后，Voice Call 会立即注册该流，在提供商连接期间通过转录提供商排队处理入站媒体，并且只有在实时转录就绪后才开始初始问候语。
- 如果 `streaming.provider` 指向未注册的提供商，或者没有已注册的提供商，Voice Call 会记录警告并跳过媒体流式传输，而不是让整个插件失败。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E6%B5%81%E5%BC%8F%E4%BC%A0%E8%BE%93%E6%8F%90%E4%BE%9B%E5%95%86%E7%A4%BA%E4%BE%8B)  流式传输提供商示例

- OpenAI

- xAI


默认值：API key `streaming.providers.openai.apiKey` 或
`OPENAI_API_KEY`；模型 `gpt-4o-transcribe`；`silenceDurationMs: 800`；
`vadThreshold: 0.5`。

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "openai",
            streamPath: "/voice/stream",
            providers: {
              openai: {
                apiKey: "sk-...", // optional if OPENAI_API_KEY is set
                model: "gpt-4o-transcribe",
                silenceDurationMs: 800,
                vadThreshold: 0.5,
              },
            },
          },
        },
      },
    },
  },
}
```

默认值：API key `streaming.providers.xai.apiKey` 或 `XAI_API_KEY`；
端点 `wss://api.x.ai/v1/stt`；编码 `mulaw`；采样率 `8000`；
`endpointingMs: 800`；`interimResults: true`。

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "xai",
            streamPath: "/voice/stream",
            providers: {
              xai: {
                apiKey: "${XAI_API_KEY}", // optional if XAI_API_KEY is set
                endpointingMs: 800,
                language: "en",
              },
            },
          },
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E9%80%9A%E8%AF%9D%E7%9A%84-tts)  通话的 TTS

Voice Call 使用核心 `messages.tts` 配置在通话中进行流式语音。你可以在插件配置下用 **相同结构** 覆盖它，它会与 `messages.tts` 深度合并。

```
{
  tts: {
    provider: "elevenlabs",
    providers: {
      elevenlabs: {
        voiceId: "pMsXgVXv3BLzUgSXRplE",
        modelId: "eleven_multilingual_v2",
      },
    },
  },
}
```

**Microsoft speech 会在语音通话中被忽略。** 电话音频需要 PCM；当前 Microsoft 传输不暴露电话 PCM 输出。

行为说明：

- 插件配置中的旧版 `tts.<provider>` 键（`openai`、`elevenlabs`、`microsoft`、`edge`）会由 `openclaw doctor --fix` 修复；已提交的配置应使用 `tts.providers.<provider>`。
- 启用 Twilio 媒体流式传输时会使用核心 TTS；否则通话会回退到提供商原生语音。
- 如果 Twilio 媒体流已处于活动状态，Voice Call 不会回退到 TwiML `<Say>`。如果在该状态下电话 TTS 不可用，播放请求会失败，而不是混合两条播放路径。
- 当电话 TTS 回退到次级提供商时，Voice Call 会记录包含提供商链（`from`、`to`、`attempts`）的警告，便于调试。
- 当 Twilio 插话或流拆除清空待处理 TTS 队列时，已排队的播放请求会完成结算，而不是让呼叫者在等待播放完成时挂起。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#tts-%E7%A4%BA%E4%BE%8B)  TTS 示例

- Core TTS only

- Override to ElevenLabs (calls only)

- OpenAI model override (deep-merge)


```
{
  messages: {
    tts: {
      provider: "openai",
      providers: {
        openai: { voice: "alloy" },
      },
    },
  },
}
```

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          tts: {
            provider: "elevenlabs",
            providers: {
              elevenlabs: {
                apiKey: "elevenlabs_key",
                voiceId: "pMsXgVXv3BLzUgSXRplE",
                modelId: "eleven_multilingual_v2",
              },
            },
          },
        },
      },
    },
  },
}
```

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          tts: {
            providers: {
              openai: {
                model: "gpt-4o-mini-tts",
                voice: "marin",
              },
            },
          },
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E5%85%A5%E7%AB%99%E9%80%9A%E8%AF%9D)  入站通话

入站策略默认为 `disabled`。要启用入站通话，请设置：

```
{
  inboundPolicy: "allowlist",
  allowFrom: ["+15550001234"],
  inboundGreeting: "Hello! How can I help?",
}
```

`inboundPolicy: "allowlist"` 是低保证级别的主叫号码筛选。该插件会规范化提供商提供的 `From` 值并将其与 `allowFrom` 比较。Webhook 验证会认证提供商投递和载荷完整性，但它 **不能** 证明 PSTN/VoIP 主叫号码所有权。请将 `allowFrom` 视为主叫号码过滤，而不是强主叫身份。

自动响应使用智能体系统。可通过 `responseModel`、`responseSystemPrompt` 和 `responseTimeoutMs` 调整。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E6%8C%89%E5%8F%B7%E7%A0%81%E8%B7%AF%E7%94%B1)  按号码路由

当一个 Voice Call 插件接收多个电话号码的来电，并且每个号码都应表现得像不同线路时，请使用 `numbers`。例如，一个号码可以使用随意的个人助理，而另一个号码可以使用商务人设、不同的响应智能体以及不同的 TTS 语音。路由会根据提供商提供的被叫 `To` 号码选择。键必须是 E.164 号码。来电到达时，Voice Call 会解析一次匹配路由，将匹配的路由存储在通话记录上，并复用该生效配置用于问候语、经典自动响应路径、实时咨询路径和 TTS 播放。如果没有匹配的路由，则使用全局 Voice Call 配置。
出站通话不使用 `numbers`；发起通话时请显式传入出站目标、消息和会话。路由覆盖目前支持：

- `inboundGreeting`
- `tts`
- `agentId`
- `responseModel`
- `responseSystemPrompt`
- `responseTimeoutMs`

`tts` 路由值会深度合并到全局 Voice Call `tts` 配置之上，因此通常只需要覆盖提供商语音：

```
{
  inboundGreeting: "Hello from the main line.",
  responseSystemPrompt: "You are the default voice assistant.",
  tts: {
    provider: "openai",
    providers: {
      openai: { voice: "coral" },
    },
  },
  numbers: {
    "+15550001111": {
      inboundGreeting: "Silver Fox Cards, how can I help?",
      responseSystemPrompt: "You are a concise baseball card specialist.",
      tts: {
        providers: {
          openai: { voice: "alloy" },
        },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E8%AF%AD%E9%9F%B3%E8%BE%93%E5%87%BA%E5%A5%91%E7%BA%A6)  语音输出契约

对于自动响应，Voice Call 会向系统提示词追加严格的语音输出契约：

```
{"spoken":"..."}
```

Voice Call 会防御性地提取语音文本：

- 忽略标记为推理/错误内容的载荷。
- 解析直接 JSON、围栏 JSON 或内联 `"spoken"` 键。
- 回退到纯文本，并移除可能的规划/元信息开头段落。

这会让语音播放聚焦于面向呼叫者的文本，并避免将规划文本泄漏到音频中。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E5%AF%B9%E8%AF%9D%E5%90%AF%E5%8A%A8%E8%A1%8C%E4%B8%BA)  对话启动行为

对于出站 `conversation` 通话，首条消息处理与实时播放状态绑定：

- 仅当初始问候语正在主动播报时，才会抑制插话队列清空和自动响应。
- 如果初始播放失败，通话会返回 `listening`，并且初始消息仍保持排队以供重试。
- Twilio 流式传输的初始播放会在流连接时开始，不会额外延迟。
- 插话会中止活动播放，并清空已排队但尚未播放的 Twilio TTS 条目。已清空的条目会以跳过状态解析，因此后续响应逻辑可以继续，而无需等待永远不会播放的音频。
- 实时语音对话使用实时流自己的开场轮次。Voice Call **不会** 为该初始消息发布旧版 `<Say>` TwiML 更新，因此出站 `<Connect><Stream>` 会话会保持附着。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#twilio-%E6%B5%81%E6%96%AD%E5%BC%80%E5%AE%BD%E9%99%90%E6%9C%9F)  Twilio 流断开宽限期

当 Twilio 媒体流断开时，Voice Call 会等待 **2000 ms** 后再自动结束通话：

- 如果流在该窗口内重新连接，自动结束会被取消。
- 如果宽限期后没有流重新注册，通话会被结束，以防止活动通话卡住。

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E8%BF%87%E6%9C%9F%E9%80%9A%E8%AF%9D%E6%B8%85%E7%90%86%E5%99%A8)  过期通话清理器

使用 `staleCallReaperSeconds` 结束从未收到终止 Webhook 的通话（例如从未完成的通知模式通话）。默认值为 `0`（禁用）。推荐范围：

- **生产环境：** 通知类流程使用 `120`–`300` 秒。
- 保持此值 **高于 `maxDurationSeconds`**，以便正常通话能够完成。一个不错的起点是 `maxDurationSeconds + 30–60` 秒。

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          maxDurationSeconds: 300,
          staleCallReaperSeconds: 360,
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#webhook-%E5%AE%89%E5%85%A8)  Webhook 安全

当代理或隧道位于 Gateway 网关前方时，该插件会重建用于签名验证的公网 URL。这些选项控制信任哪些转发标头：

[​](https://docs.openclaw.ai/zh-CN/plugins/voice-call#param-webhook-security-allowed-hosts)

webhookSecurity.allowedHosts

string\[\]

允许来自转发标头的主机列表。

[​](https://docs.openclaw.ai/zh-CN/plugins/voice-call#param-webhook-security-trust-forwarding-headers)

webhookSecurity.trustForwardingHeaders

boolean

信任转发标头，无需允许列表。

[​](https://docs.openclaw.ai/zh-CN/plugins/voice-call#param-webhook-security-trusted-proxy-ips)

webhookSecurity.trustedProxyIPs

string\[\]

仅当请求远程 IP 与列表匹配时才信任转发标头。

附加保护：

- Twilio 和 Plivo 已启用 Webhook **重放保护**。重放的有效 Webhook 请求会被确认，但会跳过副作用。
- Twilio 对话轮次会在 `<Gather>` 回调中包含每轮 token，因此过期/重放的语音回调不能满足较新的待处理转录轮次。
- 当缺少提供商要求的签名标头时，未认证的 Webhook 请求会在读取正文前被拒绝。
- voice-call Webhook 使用共享的预认证正文配置文件（64 KB / 5 秒），并在签名验证前附加按 IP 限制的在途请求上限。

使用稳定公网主机的示例：

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          publicUrl: "https://voice.example.com/voice/webhook",
          webhookSecurity: {
            allowedHosts: ["voice.example.com"],
          },
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#cli)  CLI

```
openclaw voicecall call --to "+15555550123" --message "Hello from OpenClaw"
openclaw voicecall start --to "+15555550123"   # alias for call
openclaw voicecall continue --call-id <id> --message "Any questions?"
openclaw voicecall speak --call-id <id> --message "One moment"
openclaw voicecall dtmf --call-id <id> --digits "ww123456#"
openclaw voicecall end --call-id <id>
openclaw voicecall status --call-id <id>
openclaw voicecall tail
openclaw voicecall latency                      # summarize turn latency from logs
openclaw voicecall expose --mode funnel
```

当 Gateway 网关已在运行时，运维 `voicecall` 命令会委托给 Gateway 网关拥有的 voice-call 运行时，因此 CLI 不会绑定第二个 Webhook 服务器。如果无法访问 Gateway 网关，这些命令会回退到独立 CLI 运行时。`latency` 会从默认的 voice-call 存储路径读取 `calls.jsonl`。
使用 `--file <path>` 指向不同的日志，并使用 `--last <n>` 将分析限制为最后 N 条记录（默认 200）。输出包含轮次延迟和监听等待时间的 p50/p90/p99。

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E6%99%BA%E8%83%BD%E4%BD%93%E5%B7%A5%E5%85%B7)  智能体工具

工具名称：`voice_call`。

| 操作 | 参数 |
| --- | --- |
| `initiate_call` | `message`, `to?`, `mode?`, `dtmfSequence?` |
| `continue_call` | `callId`, `message` |
| `speak_to_user` | `callId`, `message` |
| `send_dtmf` | `callId`, `digits` |
| `end_call` | `callId` |
| `get_status` | `callId` |

此仓库在 `skills/voice-call/SKILL.md` 提供了匹配的 Skill 文档。

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#gateway-%E7%BD%91%E5%85%B3-rpc)  Gateway 网关 RPC

| 方法 | 参数 |
| --- | --- |
| `voicecall.initiate` | `to?`, `message`, `mode?`, `dtmfSequence?` |
| `voicecall.continue` | `callId`, `message` |
| `voicecall.speak` | `callId`, `message` |
| `voicecall.dtmf` | `callId`, `digits` |
| `voicecall.end` | `callId` |
| `voicecall.status` | `callId` |

`dtmfSequence` 仅在 `mode: "conversation"` 时有效。通知模式的通话如果需要接通后的按键数字，应在通话创建后使用 `voicecall.dtmf`。

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)  故障排除

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E8%AE%BE%E7%BD%AE%E5%9B%A0-webhook-%E6%9A%B4%E9%9C%B2%E5%A4%B1%E8%B4%A5)  设置因 webhook 暴露失败

从运行 Gateway 网关的同一环境运行设置：

```
openclaw voicecall setup
openclaw voicecall setup --json
```

对于 `twilio`、`telnyx` 和 `plivo`，`webhook-exposure` 必须为绿色。配置好的 `publicUrl` 如果指向本地或私有网络空间，仍会失败，因为运营商无法回调到这些地址。不要将 `localhost`、`127.0.0.1`、`0.0.0.0`、`10.x`、`172.16.x`-`172.31.x`、`192.168.x`、`169.254.x`、`fc00::/7` 或 `fd00::/8` 用作 `publicUrl`。Twilio 通知模式的外呼会在创建通话请求中直接发送初始 `<Say>` TwiML，因此第一条语音消息不依赖 Twilio 拉取 webhook TwiML。状态回调、会话通话、接通前 DTMF、实时流和接通后的通话控制仍然需要公共 webhook。使用一种公共暴露路径：

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          publicUrl: "https://voice.example.com/voice/webhook",
          // or
          tunnel: { provider: "ngrok" },
          // or
          tailscale: { mode: "funnel", path: "/voice/webhook" },
        },
      },
    },
  },
}
```

更改配置后，重启或重新加载 Gateway 网关，然后运行：

```
openclaw voicecall setup
openclaw voicecall smoke
```

除非传入 `--yes`，否则 `voicecall smoke` 是一次空运行。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E6%8F%90%E4%BE%9B%E5%95%86%E5%87%AD%E8%AF%81%E5%A4%B1%E8%B4%A5)  提供商凭证失败

检查选定的提供商和必需的凭证字段：

- Twilio：`twilio.accountSid`、`twilio.authToken` 和 `fromNumber`，或 `TWILIO_ACCOUNT_SID`、`TWILIO_AUTH_TOKEN` 和 `TWILIO_FROM_NUMBER`。
- Telnyx：`telnyx.apiKey`、`telnyx.connectionId`、`telnyx.publicKey` 和 `fromNumber`。
- Plivo：`plivo.authId`、`plivo.authToken` 和 `fromNumber`。

凭证必须存在于 Gateway 网关主机上。编辑本地 shell 配置文件不会影响已经运行的 Gateway 网关，直到它重启或重新加载其环境。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E9%80%9A%E8%AF%9D%E5%90%AF%E5%8A%A8%E4%BD%86%E6%8F%90%E4%BE%9B%E5%95%86-webhook-%E6%9C%AA%E5%88%B0%E8%BE%BE)  通话启动但提供商 webhook 未到达

确认提供商控制台指向确切的公共 webhook URL：

```
https://voice.example.com/voice/webhook
```

然后检查运行时状态：

```
openclaw voicecall status --call-id <id>
openclaw voicecall tail
openclaw logs --follow
```

常见原因：

- `publicUrl` 指向的路径与 `serve.path` 不同。
- Gateway 网关启动后，隧道 URL 发生了变化。
- 代理转发了请求，但剥离或重写了 host/proto 标头。
- 防火墙或 DNS 将公共主机名路由到了 Gateway 网关以外的位置。
- Gateway 网关重启时未启用 Voice Call 插件。

当反向代理或隧道位于 Gateway 网关前面时，将 `webhookSecurity.allowedHosts` 设置为公共主机名，或为已知代理地址使用 `webhookSecurity.trustedProxyIPs`。仅在代理边界由你控制时使用 `webhookSecurity.trustForwardingHeaders`。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E7%AD%BE%E5%90%8D%E9%AA%8C%E8%AF%81%E5%A4%B1%E8%B4%A5)  签名验证失败

提供商签名会根据 OpenClaw 从传入请求重建的公共 URL 进行检查。如果签名失败：

- 确认提供商 webhook URL 与 `publicUrl` 完全匹配，包括 scheme、host 和 path。
- 对于 ngrok 免费层 URL，当隧道主机名变化时更新 `publicUrl`。
- 确保代理保留原始 host 和 proto 标头，或配置 `webhookSecurity.allowedHosts`。
- 不要在本地测试以外启用 `skipSignatureVerification`。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#google-meet-twilio-%E5%8A%A0%E5%85%A5%E5%A4%B1%E8%B4%A5)  Google Meet Twilio 加入失败

Google Meet 使用此插件进行 Twilio 拨入加入。首先验证 Voice Call：

```
openclaw voicecall setup
openclaw voicecall smoke --to "+15555550123"
```

然后显式验证 Google Meet 传输协议：

```
openclaw googlemeet setup --transport twilio
```

如果 Voice Call 状态正常但 Meet 参与者从未加入，请检查 Meet 拨入号码、PIN 和 `--dtmf-sequence`。电话通话可能是健康的，但会议会拒绝或忽略不正确的 DTMF 序列。Google Meet 会将 Meet DTMF 序列和介绍文本传递给 `voicecall.start`。对于 Twilio 通话，Voice Call 会先提供 DTMF TwiML，重定向回 webhook，然后打开实时媒体流，以便保存的介绍在电话参与者加入会议后生成。使用 `openclaw logs --follow` 查看实时阶段跟踪。健康的 Twilio Meet 加入会按以下顺序记录日志：

- Google Meet 将 Twilio 加入委托给 Voice Call。
- Voice Call 存储接通前 DTMF TwiML。
- Twilio 初始 TwiML 会在实时处理之前被消费并提供。
- Voice Call 为 Twilio 通话提供实时 TwiML。
- 实时桥接启动，初始问候已排队。

`openclaw voicecall tail` 仍会显示持久化的通话记录；它对查看通话状态和转录很有用，但并非每个 webhook/实时转换都会出现在那里。

### [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E5%AE%9E%E6%97%B6%E9%80%9A%E8%AF%9D%E6%B2%A1%E6%9C%89%E8%AF%AD%E9%9F%B3)  实时通话没有语音

确认只启用一种音频模式。`realtime.enabled` 和 `streaming.enabled` 不能同时为 true。对于实时 Twilio 通话，还要验证：

- 已加载并注册实时提供商插件。
- `realtime.provider` 未设置，或命名了一个已注册的提供商。
- 提供商 API key 对 Gateway 网关进程可用。
- `openclaw logs --follow` 显示已提供实时 TwiML、实时桥接已启动，并且初始问候已排队。

## [​](https://docs.openclaw.ai/zh-CN/plugins/voice-call\#%E7%9B%B8%E5%85%B3)  相关

- [通话模式](https://docs.openclaw.ai/zh-CN/nodes/talk)
- [文本转语音](https://docs.openclaw.ai/zh-CN/tools/tts)
- [语音唤醒](https://docs.openclaw.ai/zh-CN/nodes/voicewake)

[Install and Configure](https://docs.openclaw.ai/zh-CN/tools/plugin) [Zalo 个人插件](https://docs.openclaw.ai/zh-CN/plugins/zalouser)

Ctrl+I