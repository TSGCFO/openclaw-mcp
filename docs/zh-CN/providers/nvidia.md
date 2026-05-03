---
source_url: https://docs.openclaw.ai/zh-CN/providers/nvidia
title: "NVIDIA - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/zh-CN/providers/nvidia#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

NVIDIA

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [入门指南](https://docs.openclaw.ai/zh-CN/providers/nvidia#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)
- [配置示例](https://docs.openclaw.ai/zh-CN/providers/nvidia#%E9%85%8D%E7%BD%AE%E7%A4%BA%E4%BE%8B)
- [内置目录](https://docs.openclaw.ai/zh-CN/providers/nvidia#%E5%86%85%E7%BD%AE%E7%9B%AE%E5%BD%95)
- [高级配置](https://docs.openclaw.ai/zh-CN/providers/nvidia#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)
- [相关内容](https://docs.openclaw.ai/zh-CN/providers/nvidia#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

NVIDIA 在 `https://integrate.api.nvidia.com/v1` 提供 OpenAI 兼容 API，可免费使用开放模型。使用来自 [build.nvidia.com](https://build.nvidia.com/settings/api-keys) 的 API 密钥进行身份验证。

## [​](https://docs.openclaw.ai/zh-CN/providers/nvidia\#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)  入门指南

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/nvidia#)

获取你的 API 密钥

在 [build.nvidia.com](https://build.nvidia.com/settings/api-keys) 创建一个 API 密钥。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/nvidia#)

导出密钥并运行新手引导

```
export NVIDIA_API_KEY="nvapi-..."
openclaw onboard --auth-choice nvidia-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/nvidia#)

设置 NVIDIA 模型

```
openclaw models set nvidia/nvidia/nemotron-3-super-120b-a12b
```

如果你传入 `--nvidia-api-key` 而不是环境变量，该值会进入 shell 历史记录和 `ps` 输出。尽可能优先使用 `NVIDIA_API_KEY` 环境变量。

对于非交互式设置，你也可以直接传入密钥：

```
openclaw onboard --auth-choice nvidia-api-key --nvidia-api-key "nvapi-..."
```

## [​](https://docs.openclaw.ai/zh-CN/providers/nvidia\#%E9%85%8D%E7%BD%AE%E7%A4%BA%E4%BE%8B)  配置示例

```
{
  env: { NVIDIA_API_KEY: "nvapi-..." },
  models: {
    providers: {
      nvidia: {
        baseUrl: "https://integrate.api.nvidia.com/v1",
        api: "openai-completions",
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "nvidia/nvidia/nemotron-3-super-120b-a12b" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/providers/nvidia\#%E5%86%85%E7%BD%AE%E7%9B%AE%E5%BD%95)  内置目录

| 模型 ref | 名称 | 上下文 | 最大输出 |
| --- | --- | --- | --- |
| `nvidia/nvidia/nemotron-3-super-120b-a12b` | NVIDIA Nemotron 3 Super 120B | 262,144 | 8,192 |
| `nvidia/moonshotai/kimi-k2.5` | Kimi K2.5 | 262,144 | 8,192 |
| `nvidia/minimaxai/minimax-m2.5` | Minimax M2.5 | 196,608 | 8,192 |
| `nvidia/z-ai/glm5` | GLM 5 | 202,752 | 8,192 |

## [​](https://docs.openclaw.ai/zh-CN/providers/nvidia\#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)  高级配置

自动启用行为

当设置了 `NVIDIA_API_KEY` 环境变量时，提供商会自动启用。除了密钥以外，不需要显式的提供商配置。

目录和定价

内置目录是静态的。由于 NVIDIA 目前为列出的模型提供免费 API 访问权限，源代码中的成本默认值为 `0`。

OpenAI 兼容端点

NVIDIA 使用标准的 `/v1` 补全端点。任何 OpenAI 兼容工具都应可直接配合 NVIDIA base URL 使用。

NVIDIA 模型目前可免费使用。请查看 [build.nvidia.com](https://build.nvidia.com/) 获取最新的可用性和速率限制详情。

## [​](https://docs.openclaw.ai/zh-CN/providers/nvidia\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

[**模型选择** \\
\\
选择提供商、模型 ref 和故障转移行为。](https://docs.openclaw.ai/zh-CN/concepts/model-providers)

[**配置参考** \\
\\
智能体、模型和提供商的完整配置参考。](https://docs.openclaw.ai/zh-CN/gateway/configuration-reference)

Ctrl+I