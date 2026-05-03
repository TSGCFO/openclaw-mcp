---
source_url: https://docs.openclaw.ai/zh-CN/nodes/talk
title: "\u901a\u8bdd\u6a21\u5f0f - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/nodes/talk#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

媒体与设备

通话模式

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [行为（macOS）](https://docs.openclaw.ai/zh-CN/nodes/talk#%E8%A1%8C%E4%B8%BA%EF%BC%88macos%EF%BC%89)
- [回复中的语音指令](https://docs.openclaw.ai/zh-CN/nodes/talk#%E5%9B%9E%E5%A4%8D%E4%B8%AD%E7%9A%84%E8%AF%AD%E9%9F%B3%E6%8C%87%E4%BB%A4)
- [配置（~/.openclaw/openclaw.json）](https://docs.openclaw.ai/zh-CN/nodes/talk#%E9%85%8D%E7%BD%AE%EF%BC%88-%2F-openclaw%2Fopenclaw-json%EF%BC%89)
- [macOS UI](https://docs.openclaw.ai/zh-CN/nodes/talk#macos-ui)
- [Android UI](https://docs.openclaw.ai/zh-CN/nodes/talk#android-ui)
- [说明](https://docs.openclaw.ai/zh-CN/nodes/talk#%E8%AF%B4%E6%98%8E)
- [相关内容](https://docs.openclaw.ai/zh-CN/nodes/talk#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

通话模式是一个连续语音对话循环：

1. 监听语音
2. 将转录文本发送给模型（主会话，`chat.send`）
3. 等待响应
4. 通过已配置的通话提供商朗读响应（`talk.speak`）

## [​](https://docs.openclaw.ai/zh-CN/nodes/talk\#%E8%A1%8C%E4%B8%BA%EF%BC%88macos%EF%BC%89)  行为（macOS）

- 启用通话模式时，显示 **常驻悬浮层**。
- 在 **监听 → 思考 → 朗读** 阶段之间切换。
- 在 **短暂停顿**（静音窗口）后，发送当前转录文本。
- 回复会 **写入 WebChat**（与手动输入相同）。
- **语音打断**（默认开启）：如果智能体正在朗读时用户开始说话，我们会停止播放，并记录打断时间戳，以供下一个提示使用。

## [​](https://docs.openclaw.ai/zh-CN/nodes/talk\#%E5%9B%9E%E5%A4%8D%E4%B8%AD%E7%9A%84%E8%AF%AD%E9%9F%B3%E6%8C%87%E4%BB%A4)  回复中的语音指令

智能体可以在回复前添加 **单独一行 JSON** 来控制语音：

```
{ "voice": "<voice-id>", "once": true }
```

规则：

- 仅第一条非空行有效。
- 未知键会被忽略。
- `once: true` 仅应用于当前这次回复。
- 如果没有 `once`，该语音会成为通话模式新的默认语音。
- 在 TTS 播放前，这一行 JSON 会被移除。

支持的键：

- `voice` / `voice_id` / `voiceId`
- `model` / `model_id` / `modelId`
- `speed`, `rate`（WPM）, `stability`, `similarity`, `style`, `speakerBoost`
- `seed`, `normalize`, `lang`, `output_format`, `latency_tier`
- `once`

## [​](https://docs.openclaw.ai/zh-CN/nodes/talk\#%E9%85%8D%E7%BD%AE%EF%BC%88-/-openclaw/openclaw-json%EF%BC%89)  配置（`~/.openclaw/openclaw.json`）

```
{
  talk: {
    provider: "elevenlabs",
    providers: {
      elevenlabs: {
        voiceId: "elevenlabs_voice_id",
        modelId: "eleven_v3",
        outputFormat: "mp3_44100_128",
        apiKey: "elevenlabs_api_key",
      },
      mlx: {
        modelId: "mlx-community/Soprano-80M-bf16",
      },
      system: {},
    },
    speechLocale: "ru-RU",
    silenceTimeoutMs: 1500,
    interruptOnSpeech: true,
  },
}
```

默认值：

- `interruptOnSpeech`：true
- `silenceTimeoutMs`：未设置时，通话模式会在发送转录文本前使用平台默认的停顿窗口（`macOS` 和 `Android` 为 `700 ms`，`iOS` 为 `900 ms`）
- `provider`：选择当前启用的通话提供商。对于 `macOS` 本地播放路径，可使用 `elevenlabs`、`mlx` 或 `system`。
- `providers.<provider>.voiceId`：对于 ElevenLabs，会回退到 `ELEVENLABS_VOICE_ID` / `SAG_VOICE_ID`（或者在 API key 可用时使用第一个 ElevenLabs 语音）。
- `providers.elevenlabs.modelId`：未设置时默认为 `eleven_v3`。
- `providers.mlx.modelId`：未设置时默认为 `mlx-community/Soprano-80M-bf16`。
- `providers.elevenlabs.apiKey`：会回退到 `ELEVENLABS_API_KEY`（如果可用，也可使用 Gateway 网关 shell 配置文件中的值）。
- `speechLocale`：可选的 BCP 47 区域设置 ID，用于 `iOS` / `macOS` 上设备端通话语音识别。不设置则使用设备默认值。
- `outputFormat`：在 `macOS` / `iOS` 上默认是 `pcm_44100`，在 `Android` 上默认是 `pcm_24000`（设置 `mp3_*` 可强制使用 MP3 流式传输）

## [​](https://docs.openclaw.ai/zh-CN/nodes/talk\#macos-ui)  macOS UI

- 菜单栏开关： **Talk**
- 配置标签页： **Talk Mode** 分组（voice id + 打断开关）
- 悬浮层：
  - **Listening**：云朵随麦克风音量脉动
  - **Thinking**：下沉动画
  - **Speaking**：向外辐射的圆环
  - 点击云朵：停止朗读
  - 点击 X：退出通话模式

## [​](https://docs.openclaw.ai/zh-CN/nodes/talk\#android-ui)  Android UI

- 语音标签页开关： **Talk**
- 手动 **Mic** 和 **Talk** 是互斥的运行时采集模式。
- 当应用离开前台或用户离开语音标签页时，手动 Mic 会停止。
- Talk Mode 会持续运行，直到被关闭或 Android 节点断开连接，并且在启用期间使用 Android 麦克风前台服务类型。

## [​](https://docs.openclaw.ai/zh-CN/nodes/talk\#%E8%AF%B4%E6%98%8E)  说明

- 需要语音和麦克风权限。
- 使用会话键 `main` 对 `chat.send` 发起调用。
- Gateway 网关会使用当前启用的通话提供商，通过 `talk.speak` 解析通话播放。只有在该 RPC 不可用时，Android 才会回退到本地系统 TTS。
- `macOS` 本地 MLX 播放会在可用时使用内置的 `openclaw-mlx-tts` helper，或者使用 `PATH` 上的可执行文件。开发时可设置 `OPENCLAW_MLX_TTS_BIN`，指向自定义 helper 二进制文件。
- `eleven_v3` 的 `stability` 会校验为 `0.0`、`0.5` 或 `1.0`；其他模型接受 `0..1`。
- 设置 `latency_tier` 时，会校验其范围为 `0..4`。
- Android 支持 `pcm_16000`、`pcm_22050`、`pcm_24000` 和 `pcm_44100` 输出格式，以实现低延迟 `AudioTrack` 流式传输。

## [​](https://docs.openclaw.ai/zh-CN/nodes/talk\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [语音唤醒](https://docs.openclaw.ai/zh-CN/nodes/voicewake)
- [音频和语音笔记](https://docs.openclaw.ai/zh-CN/nodes/audio)
- [媒体理解](https://docs.openclaw.ai/zh-CN/nodes/media-understanding)

[相机采集](https://docs.openclaw.ai/zh-CN/nodes/camera) [语音唤醒](https://docs.openclaw.ai/zh-CN/nodes/voicewake)

Ctrl+I