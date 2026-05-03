---
source_url: https://docs.openclaw.ai/zh-CN/help/environment
title: "\u73af\u5883\u53d8\u91cf - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/help/environment#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

环境与调试

环境变量

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [优先级（最高 → 最低）](https://docs.openclaw.ai/zh-CN/help/environment#%E4%BC%98%E5%85%88%E7%BA%A7%EF%BC%88%E6%9C%80%E9%AB%98-%E2%86%92-%E6%9C%80%E4%BD%8E%EF%BC%89)
- [配置 env 块](https://docs.openclaw.ai/zh-CN/help/environment#%E9%85%8D%E7%BD%AE-env-%E5%9D%97)
- [命令行环境导入](https://docs.openclaw.ai/zh-CN/help/environment#%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%8E%AF%E5%A2%83%E5%AF%BC%E5%85%A5)
- [运行时注入的环境变量](https://docs.openclaw.ai/zh-CN/help/environment#%E8%BF%90%E8%A1%8C%E6%97%B6%E6%B3%A8%E5%85%A5%E7%9A%84%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
- [UI 环境变量](https://docs.openclaw.ai/zh-CN/help/environment#ui-%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
- [配置中的环境变量替换](https://docs.openclaw.ai/zh-CN/help/environment#%E9%85%8D%E7%BD%AE%E4%B8%AD%E7%9A%84%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E6%9B%BF%E6%8D%A2)
- [密钥引用与 ${ENV} 字符串](https://docs.openclaw.ai/zh-CN/help/environment#%E5%AF%86%E9%92%A5%E5%BC%95%E7%94%A8%E4%B8%8E-%24env-%E5%AD%97%E7%AC%A6%E4%B8%B2)
- [路径相关环境变量](https://docs.openclaw.ai/zh-CN/help/environment#%E8%B7%AF%E5%BE%84%E7%9B%B8%E5%85%B3%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
- [日志记录](https://docs.openclaw.ai/zh-CN/help/environment#%E6%97%A5%E5%BF%97%E8%AE%B0%E5%BD%95)
- [OPENCLAW\_HOME](https://docs.openclaw.ai/zh-CN/help/environment#openclaw_home)
- [nvm 用户：web\_fetch TLS 失败](https://docs.openclaw.ai/zh-CN/help/environment#nvm-%E7%94%A8%E6%88%B7%EF%BC%9Aweb_fetch-tls-%E5%A4%B1%E8%B4%A5)
- [旧版环境变量](https://docs.openclaw.ai/zh-CN/help/environment#%E6%97%A7%E7%89%88%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
- [相关内容](https://docs.openclaw.ai/zh-CN/help/environment#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw 会从多个来源读取环境变量。规则是 **绝不覆盖已有值**。

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E4%BC%98%E5%85%88%E7%BA%A7%EF%BC%88%E6%9C%80%E9%AB%98-%E2%86%92-%E6%9C%80%E4%BD%8E%EF%BC%89)  优先级（最高 → 最低）

1. **进程环境**（Gateway 网关进程已从父级命令行解释器/守护进程继承的内容）。
2. **当前工作目录中的 `.env`**（dotenv 默认行为；不会覆盖）。
3. **全局 `.env`**，位于 `~/.openclaw/.env`（也就是 `$OPENCLAW_STATE_DIR/.env`；不会覆盖）。
4. **配置 `env` 块**，位于 `~/.openclaw/openclaw.json`（仅在缺失时应用）。
5. **可选登录命令行环境导入**（`env.shellEnv.enabled` 或 `OPENCLAW_LOAD_SHELL_ENV=1`），仅对缺失的预期键名应用。

在使用默认状态目录的 Ubuntu 全新安装中，OpenClaw 还会在全局 `.env` 之后将 `~/.config/openclaw/gateway.env` 视为兼容性回退。如果两个文件都存在且内容不一致，OpenClaw 会保留 `~/.openclaw/.env` 并打印警告。如果配置文件完全缺失，则跳过第 4 步；如果已启用，命令行环境导入仍会运行。

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E9%85%8D%E7%BD%AE-env-%E5%9D%97)  配置 `env` 块

设置内联环境变量有两种等效方式（两者都不会覆盖已有值）：

```
{
  env: {
    OPENROUTER_API_KEY: "sk-or-...",
    vars: {
      GROQ_API_KEY: "gsk-...",
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%8E%AF%E5%A2%83%E5%AF%BC%E5%85%A5)  命令行环境导入

`env.shellEnv` 会运行你的登录命令行解释器，并仅导入 **缺失的** 预期键名：

```
{
  env: {
    shellEnv: {
      enabled: true,
      timeoutMs: 15000,
    },
  },
}
```

环境变量等价项：

- `OPENCLAW_LOAD_SHELL_ENV=1`
- `OPENCLAW_SHELL_ENV_TIMEOUT_MS=15000`

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E8%BF%90%E8%A1%8C%E6%97%B6%E6%B3%A8%E5%85%A5%E7%9A%84%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)  运行时注入的环境变量

OpenClaw 还会向派生的子进程注入上下文标记：

- `OPENCLAW_SHELL=exec`：为通过 `exec` 工具运行的命令设置。
- `OPENCLAW_SHELL=acp`：为 ACP 运行时后端进程启动设置（例如 `acpx`）。
- `OPENCLAW_SHELL=acp-client`：在 `openclaw acp client` 启动 ACP 桥接进程时设置。
- `OPENCLAW_SHELL=tui-local`：为本地 TUI `!` 命令行命令设置。

这些是运行时标记（不是必需的用户配置）。它们可用于命令行环境/配置文件逻辑
以应用特定于上下文的规则。

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#ui-%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)  UI 环境变量

- `OPENCLAW_THEME=light`：当你的终端使用浅色背景时，强制使用浅色 TUI 调色板。
- `OPENCLAW_THEME=dark`：强制使用深色 TUI 调色板。
- `COLORFGBG`：如果你的终端导出它，OpenClaw 会使用背景颜色提示来自动选择 TUI 调色板。

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E9%85%8D%E7%BD%AE%E4%B8%AD%E7%9A%84%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E6%9B%BF%E6%8D%A2)  配置中的环境变量替换

你可以使用 `${VAR_NAME}` 语法在配置字符串值中直接引用环境变量：

```
{
  models: {
    providers: {
      "vercel-gateway": {
        apiKey: "${VERCEL_GATEWAY_API_KEY}",
      },
    },
  },
}
```

了解详情，请参阅 [配置：环境变量替换](https://docs.openclaw.ai/zh-CN/gateway/configuration-reference#env-var-substitution)。

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E5%AF%86%E9%92%A5%E5%BC%95%E7%94%A8%E4%B8%8E-$env-%E5%AD%97%E7%AC%A6%E4%B8%B2)  密钥引用与 `${ENV}` 字符串

OpenClaw 支持两种由环境变量驱动的模式：

- 配置值中的 `${VAR}` 字符串替换。
- SecretRef 对象（`{ source: "env", provider: "default", id: "VAR" }`），用于支持密钥引用的字段。

两者都会在激活时从进程环境变量解析。SecretRef 的详细信息见 [密钥管理](https://docs.openclaw.ai/zh-CN/gateway/secrets)。

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E8%B7%AF%E5%BE%84%E7%9B%B8%E5%85%B3%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)  路径相关环境变量

| 变量 | 用途 |
| --- | --- |
| `OPENCLAW_HOME` | 覆盖用于所有内部路径解析的主目录（`~/.openclaw/`、智能体目录、会话、凭证）。当以专用服务用户身份运行 OpenClaw 时很有用。 |
| `OPENCLAW_STATE_DIR` | 覆盖状态目录（默认 `~/.openclaw`）。 |
| `OPENCLAW_CONFIG_PATH` | 覆盖配置文件路径（默认 `~/.openclaw/openclaw.json`）。 |
| `OPENCLAW_INCLUDE_ROOTS` | 目录的路径列表；`$include` 指令可通过这些目录解析配置目录之外的文件（默认：无 — `$include` 被限制在配置目录内）。会展开波浪号。 |

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E6%97%A5%E5%BF%97%E8%AE%B0%E5%BD%95)  日志记录

| 变量 | 用途 |
| --- | --- |
| `OPENCLAW_LOG_LEVEL` | 同时覆盖文件和控制台的日志级别（例如 `debug`、`trace`）。优先于配置中的 `logging.level` 和 `logging.consoleLevel`。无效值会被忽略并发出警告。 |

### [​](https://docs.openclaw.ai/zh-CN/help/environment\#openclaw_home)  `OPENCLAW_HOME`

设置后，`OPENCLAW_HOME` 会在所有内部路径解析中替代系统主目录（`$HOME` / `os.homedir()`）。这可为无头服务账户实现完整的文件系统隔离。**优先级：**`OPENCLAW_HOME` \> `$HOME` \> `USERPROFILE` \> `os.homedir()`**示例**（macOS LaunchDaemon）：

```
<key>EnvironmentVariables</key>
<dict>
  <key>OPENCLAW_HOME</key>
  <string>/Users/user</string>
</dict>
```

也可以将 `OPENCLAW_HOME` 设置为波浪号路径（例如 `~/svc`），使用前会通过 `$HOME` 展开。

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#nvm-%E7%94%A8%E6%88%B7%EF%BC%9Aweb_fetch-tls-%E5%A4%B1%E8%B4%A5)  nvm 用户：web\_fetch TLS 失败

如果 Node.js 是通过 **nvm**（而不是系统包管理器）安装的，内置 `fetch()` 会使用
nvm 捆绑的 CA 存储，其中可能缺少较新的根 CA（用于 Let’s Encrypt 的 ISRG Root X1/X2、
DigiCert Global Root G2 等）。这会导致 `web_fetch` 在大多数 HTTPS 站点上以 `"fetch failed"` 失败。在 Linux 上，OpenClaw 会自动检测 nvm，并在实际启动环境中应用修复：

- `openclaw gateway install` 会将 `NODE_EXTRA_CA_CERTS` 写入 systemd 服务环境
- `openclaw` CLI 入口点会在 Node 启动前设置 `NODE_EXTRA_CA_CERTS` 并重新执行自身

**手动修复（用于旧版本或直接 `node ...` 启动）：**在启动 OpenClaw 前导出该变量：

```
export NODE_EXTRA_CA_CERTS=/etc/ssl/certs/ca-certificates.crt
openclaw gateway run
```

不要只依赖将此变量写入 `~/.openclaw/.env`；Node 会在进程启动时读取
`NODE_EXTRA_CA_CERTS`。

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E6%97%A7%E7%89%88%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)  旧版环境变量

OpenClaw 只读取 `OPENCLAW_*` 环境变量。早期版本中的旧版
`CLAWDBOT_*` 和 `MOLTBOT_*` 前缀会被静默忽略。如果启动时 Gateway 网关进程上仍设置了其中任何变量，OpenClaw 会发出一条
Node 弃用警告（`OPENCLAW_LEGACY_ENV_VARS`），列出检测到的前缀和总数。请将每个值重命名，方法是用
`OPENCLAW_` 替换旧版前缀（例如 `CLAWDBOT_GATEWAY_TOKEN` →
`OPENCLAW_GATEWAY_TOKEN`）；旧名称不会生效。

## [​](https://docs.openclaw.ai/zh-CN/help/environment\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [Gateway 网关配置](https://docs.openclaw.ai/zh-CN/gateway/configuration)
- [常见问题：环境变量和 .env 加载](https://docs.openclaw.ai/zh-CN/help/faq#env-vars-and-env-loading)
- [Models 概览](https://docs.openclaw.ai/zh-CN/concepts/models)

[OpenClaw 世界观](https://docs.openclaw.ai/zh-CN/start/lore) [调试](https://docs.openclaw.ai/zh-CN/help/debugging)

Ctrl+I