---
source_url: https://docs.openclaw.ai/zh-CN/providers/cerebras
title: "Cerebras - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/zh-CN/providers/cerebras#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Cerebras

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [入门指南](https://docs.openclaw.ai/zh-CN/providers/cerebras#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)
- [非交互式设置](https://docs.openclaw.ai/zh-CN/providers/cerebras#%E9%9D%9E%E4%BA%A4%E4%BA%92%E5%BC%8F%E8%AE%BE%E7%BD%AE)
- [内置目录](https://docs.openclaw.ai/zh-CN/providers/cerebras#%E5%86%85%E7%BD%AE%E7%9B%AE%E5%BD%95)
- [手动配置](https://docs.openclaw.ai/zh-CN/providers/cerebras#%E6%89%8B%E5%8A%A8%E9%85%8D%E7%BD%AE)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[Cerebras](https://www.cerebras.ai/) 提供高速、兼容 OpenAI 的推理服务。

| 属性 | 值 |
| --- | --- |
| 提供商 | `cerebras` |
| 认证 | `CEREBRAS_API_KEY` |
| API | 兼容 OpenAI |
| Base URL | `https://api.cerebras.ai/v1` |

## [​](https://docs.openclaw.ai/zh-CN/providers/cerebras\#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)  入门指南

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/cerebras#)

获取 API 密钥

在 [Cerebras Cloud Console](https://cloud.cerebras.ai/) 中创建一个 API 密钥。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/cerebras#)

运行新手引导

```
openclaw onboard --auth-choice cerebras-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/cerebras#)

验证模型可用

```
openclaw models list --provider cerebras
```

### [​](https://docs.openclaw.ai/zh-CN/providers/cerebras\#%E9%9D%9E%E4%BA%A4%E4%BA%92%E5%BC%8F%E8%AE%BE%E7%BD%AE)  非交互式设置

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cerebras-api-key \
  --cerebras-api-key "$CEREBRAS_API_KEY"
```

## [​](https://docs.openclaw.ai/zh-CN/providers/cerebras\#%E5%86%85%E7%BD%AE%E7%9B%AE%E5%BD%95)  内置目录

OpenClaw 为公共的兼容 OpenAI 端点内置了一个静态 Cerebras 目录：

| Model ref | 名称 | 备注 |
| --- | --- | --- |
| `cerebras/zai-glm-4.7` | Z.ai GLM 4.7 | 默认模型；预览版 reasoning 模型 |
| `cerebras/gpt-oss-120b` | GPT OSS 120B | 生产级 reasoning 模型 |
| `cerebras/qwen-3-235b-a22b-instruct-2507` | Qwen 3 235B Instruct | 预览版非 reasoning 模型 |
| `cerebras/llama3.1-8b` | Llama 3.1 8B | 生产级速度优先模型 |

Cerebras 将 `zai-glm-4.7` 和 `qwen-3-235b-a22b-instruct-2507` 标记为预览模型，并且文档说明 `llama3.1-8b` / `qwen-3-235b-a22b-instruct-2507` 将于 2026 年 5 月 27 日弃用。在将它们用于生产环境之前，请查看 Cerebras 的 supported-models 页面。

## [​](https://docs.openclaw.ai/zh-CN/providers/cerebras\#%E6%89%8B%E5%8A%A8%E9%85%8D%E7%BD%AE)  手动配置

内置插件通常意味着你只需要 API 密钥。若你想覆盖模型元数据，请使用显式的
`models.providers.cerebras` 配置：

```
{
  env: { CEREBRAS_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "cerebras/zai-glm-4.7" },
    },
  },
  models: {
    mode: "merge",
    providers: {
      cerebras: {
        baseUrl: "https://api.cerebras.ai/v1",
        apiKey: "${CEREBRAS_API_KEY}",
        api: "openai-completions",
        models: [\
          { id: "zai-glm-4.7", name: "Z.ai GLM 4.7" },\
          { id: "gpt-oss-120b", name: "GPT OSS 120B" },\
        ],
      },
    },
  },
}
```

如果 Gateway 网关以守护进程方式运行（launchd/systemd），请确保 `CEREBRAS_API_KEY`
对该进程可用，例如放在 `~/.openclaw/.env` 中，或通过
`env.shellEnv` 提供。

Ctrl+I