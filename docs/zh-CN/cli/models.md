---
source_url: https://docs.openclaw.ai/zh-CN/cli/models
title: "Models - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/cli/models#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

CLI 命令

Models

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [openclaw models](https://docs.openclaw.ai/zh-CN/cli/models#openclaw-models)
- [常用命令](https://docs.openclaw.ai/zh-CN/cli/models#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
- [Models 扫描](https://docs.openclaw.ai/zh-CN/cli/models#models-%E6%89%AB%E6%8F%8F)
- [Models 状态](https://docs.openclaw.ai/zh-CN/cli/models#models-%E7%8A%B6%E6%80%81)
- [别名 \+ 回退](https://docs.openclaw.ai/zh-CN/cli/models#%E5%88%AB%E5%90%8D-%2B-%E5%9B%9E%E9%80%80)
- [认证配置档案](https://docs.openclaw.ai/zh-CN/cli/models#%E8%AE%A4%E8%AF%81%E9%85%8D%E7%BD%AE%E6%A1%A3%E6%A1%88)
- [相关](https://docs.openclaw.ai/zh-CN/cli/models#%E7%9B%B8%E5%85%B3)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN/cli/models\#openclaw-models)  `openclaw models`

模型发现、扫描和配置（默认模型、回退、认证配置档案）。相关：

- 提供商 \+ 模型： [Models](https://docs.openclaw.ai/zh-CN/providers/models)
- 模型选择概念 \+ `/models` 斜杠命令： [Models 概念](https://docs.openclaw.ai/zh-CN/concepts/models)
- 提供商认证设置： [入门指南](https://docs.openclaw.ai/zh-CN/start/getting-started)

## [​](https://docs.openclaw.ai/zh-CN/cli/models\#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)  常用命令

```
openclaw models status
openclaw models list
openclaw models set <model-or-alias>
openclaw models scan
```

`openclaw models status` 会显示解析后的默认值/回退以及认证概览。
当提供商使用量快照可用时，OAuth/API key 状态部分会包含
提供商使用窗口和配额快照。
当前的使用窗口提供商：Anthropic、GitHub Copilot、Gemini CLI、OpenAI
Codex、MiniMax、Xiaomi 和 z.ai。使用量认证会尽可能来自提供商专属钩子；
否则 OpenClaw 会回退到从认证配置档案、环境变量或配置中匹配 OAuth/API key
凭据。
在 `--json` 输出中，`auth.providers` 是感知环境变量/配置/存储的提供商
概览，而 `auth.oauth` 仅表示认证存储中的配置档案健康状况。
添加 `--probe` 可对每个已配置的提供商配置档案运行实时认证探测。
探测是真实请求（可能消耗 token 并触发速率限制）。
使用 `--agent <id>` 可检查已配置智能体的模型/认证状态。省略时，
该命令会使用 `OPENCLAW_AGENT_DIR`/`PI_CODING_AGENT_DIR`（如果已设置），否则使用
已配置的默认智能体。
探测行可以来自认证配置档案、环境变量凭据或 `models.json`。注意：

- `models set <model-or-alias>` 接受 `provider/model` 或别名。
- `models list` 是只读的：它会读取配置、认证配置档案、现有目录
状态和提供商拥有的目录行，但不会重写
`models.json`。
- `Auth` 列是提供商级别且只读的。它根据本地
认证配置档案元数据、环境变量标记、已配置的提供商 key、本地提供商
标记、AWS Bedrock 环境变量/配置档案标记，以及插件合成认证元数据计算；
它不会加载提供商运行时、读取 keychain 密钥、调用提供商
API，也不会证明精确的单模型执行就绪状态。
- `models list --all --provider <id>` 可以包含来自插件清单或内置提供商目录元数据的提供商自有静态目录
行，即使你尚未对该提供商完成认证。这些行仍会显示为
不可用，直到配置了匹配的认证。
- 当提供商目录发现较慢时，`models list` 会保持控制平面响应。默认视图和已配置视图会在短暂等待后回退到已配置或
合成模型行，并让发现过程在
后台完成。当你需要精确完整的已发现目录并且
愿意等待提供商发现时，使用 `--all`。
- 宽泛的 `models list --all` 会将清单目录行合并到注册表行之上，
而不加载提供商运行时补充钩子。提供商过滤的清单
快速路径只使用标记为 `static` 的提供商；标记为 `refreshable` 的提供商
保持由注册表/缓存支持，并将清单行作为补充追加，而
标记为 `runtime` 的提供商保持在注册表/运行时发现路径上。
- `models list` 会区分原生模型元数据和运行时上限。在表格
输出中，当有效运行时
上限不同于原生上下文窗口时，`Ctx` 会显示 `contextTokens/contextWindow`；当提供商暴露该上限时，JSON 行会包含 `contextTokens`。
- `models list --provider <id>` 按提供商 ID 过滤，例如 `moonshot` 或
`openai-codex`。它不接受交互式提供商
选择器中的显示标签，例如 `Moonshot AI`。
- 模型引用会按 **第一个**`/` 拆分。如果模型 ID 包含 `/`（OpenRouter 风格），请包含提供商前缀（示例：`openrouter/moonshotai/kimi-k2`）。
- 如果省略提供商，OpenClaw 会先将输入解析为别名，然后
解析为该精确模型 ID 的唯一已配置提供商匹配项，最后才
回退到已配置的默认提供商，并显示弃用警告。
如果该提供商不再暴露已配置的默认模型，OpenClaw
会回退到第一个已配置的提供商/模型，而不是暴露
过时的已移除提供商默认值。
- `models status` 可能会在认证输出中为非密钥占位符显示 `marker(<value>)`（例如 `OPENAI_API_KEY`、`secretref-managed`、`minimax-oauth`、`oauth:chutes`、`ollama-local`），而不是将它们当作密钥遮蔽。

### [​](https://docs.openclaw.ai/zh-CN/cli/models\#models-%E6%89%AB%E6%8F%8F)  Models 扫描

`models scan` 会读取 OpenRouter 的公开 `:free` 目录，并对用于
回退的候选项进行排名。目录本身是公开的，因此仅元数据扫描不需要
OpenRouter key。默认情况下，OpenClaw 会尝试通过实时模型调用探测工具和图像支持。
如果未配置 OpenRouter key，该命令会回退到仅元数据
输出，并说明 `:free` 模型仍需要 `OPENROUTER_API_KEY` 才能进行
探测和推理。选项：

- `--no-probe`（仅元数据；不查找配置/密钥）
- `--min-params <b>`
- `--max-age-days <days>`
- `--provider <name>`
- `--max-candidates <n>`
- `--timeout <ms>`（目录请求和单次探测超时）
- `--concurrency <n>`
- `--yes`
- `--no-input`
- `--set-default`
- `--set-image`
- `--json`

`--set-default` 和 `--set-image` 需要实时探测；仅元数据扫描
结果仅供参考，不会应用到配置。

### [​](https://docs.openclaw.ai/zh-CN/cli/models\#models-%E7%8A%B6%E6%80%81)  Models 状态

选项：

- `--json`
- `--plain`
- `--check`（退出码 1=已过期/缺失，2=即将过期）
- `--probe`（对已配置认证配置档案进行实时探测）
- `--probe-provider <name>`（探测一个提供商）
- `--probe-profile <id>`（重复指定或使用逗号分隔的配置档案 ID）
- `--probe-timeout <ms>`
- `--probe-concurrency <n>`
- `--probe-max-tokens <n>`
- `--agent <id>`（已配置的智能体 ID；覆盖 `OPENCLAW_AGENT_DIR`/`PI_CODING_AGENT_DIR`）

`--json` 会将 stdout 保留给 JSON payload。认证配置档案、提供商
和启动诊断会路由到 stderr，因此脚本可以将 stdout 直接通过管道传给
`jq` 等工具。探测状态桶：

- `ok`
- `auth`
- `rate_limit`
- `billing`
- `timeout`
- `format`
- `unknown`
- `no_model`

可预期的探测详情/原因代码情况：

- `excluded_by_auth_order`：存在已存储的配置档案，但显式
`auth.order.<provider>` 省略了它，因此探测会报告该排除，而不是
尝试它。
- `missing_credential`、`invalid_expires`、`expired`、`unresolved_ref`：
配置档案存在，但不符合条件/无法解析。
- `no_model`：提供商认证存在，但 OpenClaw 无法为该提供商解析出可探测的
模型候选项。

## [​](https://docs.openclaw.ai/zh-CN/cli/models\#%E5%88%AB%E5%90%8D-+-%E5%9B%9E%E9%80%80)  别名 \+ 回退

```
openclaw models aliases list
openclaw models fallbacks list
```

## [​](https://docs.openclaw.ai/zh-CN/cli/models\#%E8%AE%A4%E8%AF%81%E9%85%8D%E7%BD%AE%E6%A1%A3%E6%A1%88)  认证配置档案

```
openclaw models auth add
openclaw models auth login --provider <id>
openclaw models auth setup-token --provider <id>
openclaw models auth paste-token
```

`models auth add` 是交互式认证辅助工具。它可以启动提供商认证
流程（OAuth/API key），也可以指导你手动粘贴 token，具体取决于你选择的
提供商。`models auth login` 会运行提供商插件的认证流程（OAuth/API key）。使用
`openclaw plugins list` 查看已安装的提供商。
使用 `openclaw models auth --agent <id> <subcommand>` 可将认证结果写入
特定已配置智能体存储。父级 `--agent` 标志会被
`add`、`login`、`setup-token`、`paste-token` 和 `login-github-copilot` 遵循。示例：

```
openclaw models auth login --provider openai-codex --set-default
```

注意：

- `setup-token` 和 `paste-token` 仍是面向暴露 token 认证方法的提供商的通用 token 命令。
- `setup-token` 需要交互式 TTY，并运行提供商的 token 认证
方法（当该提供商暴露
`setup-token` 方法时，默认使用该方法）。
- `paste-token` 接受在其他地方生成或来自自动化的 token 字符串。
- `paste-token` 需要 `--provider`，会提示输入 token 值，并将
它写入默认配置档案 ID `<provider>:manual`，除非你传入
`--profile-id`。
- `paste-token --expires-in <duration>` 会根据相对时长（如 `365d` 或 `12h`）存储绝对 token 到期时间。
- Anthropic 注意：Anthropic 员工告诉我们，OpenClaw 风格的 Claude CLI 使用已再次被允许，因此 OpenClaw 会将 Claude CLI 复用和 `claude -p` 使用视为此集成的受认可方式，除非 Anthropic 发布新策略。
- Anthropic `setup-token` / `paste-token` 仍作为受支持的 OpenClaw token 路径可用，但 OpenClaw 现在会在可用时优先使用 Claude CLI 复用和 `claude -p`。

## [​](https://docs.openclaw.ai/zh-CN/cli/models\#%E7%9B%B8%E5%85%B3)  相关

- [CLI 参考](https://docs.openclaw.ai/zh-CN/cli)
- [模型选择](https://docs.openclaw.ai/zh-CN/concepts/model-providers)
- [模型故障转移](https://docs.openclaw.ai/zh-CN/concepts/model-failover)

[消息](https://docs.openclaw.ai/zh-CN/cli/message) [节点](https://docs.openclaw.ai/zh-CN/cli/node)

Ctrl+I