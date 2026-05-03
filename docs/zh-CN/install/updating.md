---
source_url: https://docs.openclaw.ai/zh-CN/install/updating
title: "\u66f4\u65b0 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/install/updating#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

维护

更新

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [推荐：openclaw update](https://docs.openclaw.ai/zh-CN/install/updating#%E6%8E%A8%E8%8D%90%EF%BC%9Aopenclaw-update)
- [在 npm 和 git 安装之间切换](https://docs.openclaw.ai/zh-CN/install/updating#%E5%9C%A8-npm-%E5%92%8C-git-%E5%AE%89%E8%A3%85%E4%B9%8B%E9%97%B4%E5%88%87%E6%8D%A2)
- [替代方式：重新运行安装器](https://docs.openclaw.ai/zh-CN/install/updating#%E6%9B%BF%E4%BB%A3%E6%96%B9%E5%BC%8F%EF%BC%9A%E9%87%8D%E6%96%B0%E8%BF%90%E8%A1%8C%E5%AE%89%E8%A3%85%E5%99%A8)
- [替代方式：手动使用 npm、pnpm 或 bun](https://docs.openclaw.ai/zh-CN/install/updating#%E6%9B%BF%E4%BB%A3%E6%96%B9%E5%BC%8F%EF%BC%9A%E6%89%8B%E5%8A%A8%E4%BD%BF%E7%94%A8-npm%E3%80%81pnpm-%E6%88%96-bun)
- [高级 npm 安装主题](https://docs.openclaw.ai/zh-CN/install/updating#%E9%AB%98%E7%BA%A7-npm-%E5%AE%89%E8%A3%85%E4%B8%BB%E9%A2%98)
- [自动更新器](https://docs.openclaw.ai/zh-CN/install/updating#%E8%87%AA%E5%8A%A8%E6%9B%B4%E6%96%B0%E5%99%A8)
- [更新后](https://docs.openclaw.ai/zh-CN/install/updating#%E6%9B%B4%E6%96%B0%E5%90%8E)
- [运行 Doctor](https://docs.openclaw.ai/zh-CN/install/updating#%E8%BF%90%E8%A1%8C-doctor)
- [重启 Gateway 网关](https://docs.openclaw.ai/zh-CN/install/updating#%E9%87%8D%E5%90%AF-gateway-%E7%BD%91%E5%85%B3)
- [验证](https://docs.openclaw.ai/zh-CN/install/updating#%E9%AA%8C%E8%AF%81)
- [回滚](https://docs.openclaw.ai/zh-CN/install/updating#%E5%9B%9E%E6%BB%9A)
- [固定版本（npm）](https://docs.openclaw.ai/zh-CN/install/updating#%E5%9B%BA%E5%AE%9A%E7%89%88%E6%9C%AC%EF%BC%88npm%EF%BC%89)
- [固定提交（源码）](https://docs.openclaw.ai/zh-CN/install/updating#%E5%9B%BA%E5%AE%9A%E6%8F%90%E4%BA%A4%EF%BC%88%E6%BA%90%E7%A0%81%EF%BC%89)
- [如果你遇到问题](https://docs.openclaw.ai/zh-CN/install/updating#%E5%A6%82%E6%9E%9C%E4%BD%A0%E9%81%87%E5%88%B0%E9%97%AE%E9%A2%98)
- [相关内容](https://docs.openclaw.ai/zh-CN/install/updating#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

让 OpenClaw 保持最新。

## [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E6%8E%A8%E8%8D%90%EF%BC%9Aopenclaw-update)  推荐：`openclaw update`

最快的更新方式。它会检测你的安装类型（npm 或 git），获取最新版本，运行 `openclaw doctor`，并重启 Gateway 网关。

```
openclaw update
```

要切换渠道或指定特定版本：

```
openclaw update --channel beta
openclaw update --channel dev
openclaw update --tag main
openclaw update --dry-run   # preview without applying
```

`--channel beta` 优先使用 beta，但当 beta 标签缺失或早于最新稳定版时，
运行时会回退到 stable/latest。如果你想为一次性包更新使用原始 npm beta dist-tag，
请使用 `--tag beta`。查看 [开发渠道](https://docs.openclaw.ai/zh-CN/install/development-channels) 了解渠道语义。

## [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E5%9C%A8-npm-%E5%92%8C-git-%E5%AE%89%E8%A3%85%E4%B9%8B%E9%97%B4%E5%88%87%E6%8D%A2)  在 npm 和 git 安装之间切换

当你想更改安装类型时，请使用渠道。更新器会保留你的
状态、配置、凭证和 `~/.openclaw` 中的工作区；它只会更改
CLI 和 Gateway 网关使用的 OpenClaw 代码安装。

```
# npm package install -> editable git checkout
openclaw update --channel dev

# git checkout -> npm package install
openclaw update --channel stable
```

先使用 `--dry-run` 运行，以预览确切的安装模式切换：

```
openclaw update --channel dev --dry-run
openclaw update --channel stable --dry-run
```

`dev` 渠道会确保存在 git checkout，构建它，并从该 checkout 安装全局 CLI。
`stable` 和 `beta` 渠道使用包安装。如果 Gateway 网关已经安装，
`openclaw update` 会刷新服务元数据并重启它，除非你传入 `--no-restart`。

## [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E6%9B%BF%E4%BB%A3%E6%96%B9%E5%BC%8F%EF%BC%9A%E9%87%8D%E6%96%B0%E8%BF%90%E8%A1%8C%E5%AE%89%E8%A3%85%E5%99%A8)  替代方式：重新运行安装器

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

添加 `--no-onboard` 可跳过新手引导。要通过安装器强制指定安装类型，
请传入 `--install-method git --no-onboard` 或
`--install-method npm --no-onboard`。如果 `openclaw update` 在 npm 包安装阶段之后失败，请重新运行
安装器。安装器不会调用旧的更新器；它会直接运行全局
包安装，并且可以恢复部分更新的 npm 安装。

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method npm
```

要将恢复固定到特定版本或 dist-tag，请添加 `--version`：

```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method npm --version <version-or-dist-tag>
```

## [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E6%9B%BF%E4%BB%A3%E6%96%B9%E5%BC%8F%EF%BC%9A%E6%89%8B%E5%8A%A8%E4%BD%BF%E7%94%A8-npm%E3%80%81pnpm-%E6%88%96-bun)  替代方式：手动使用 npm、pnpm 或 bun

```
npm i -g openclaw@latest
```

当 `openclaw update` 管理全局 npm 安装时，它会先把目标安装到
临时 npm prefix，验证打包的 `dist` 清单，然后把干净的包树切换
到真正的全局 prefix。这样可以避免 npm 将新包覆盖到旧包遗留的过期文件上。
如果安装命令失败，OpenClaw 会使用 `--omit=optional` 重试一次。
该重试有助于原生可选依赖无法编译的主机，同时如果回退也失败，
仍会显示原始失败。

```
pnpm add -g openclaw@latest
```

```
bun add -g openclaw@latest
```

### [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E9%AB%98%E7%BA%A7-npm-%E5%AE%89%E8%A3%85%E4%B8%BB%E9%A2%98)  高级 npm 安装主题

只读包树

OpenClaw 在运行时会将打包的全局安装视为只读，即使当前用户可以写入全局包目录。插件包安装位于用户配置目录下由 OpenClaw 拥有的 npm/git 根目录中，并且 Gateway 网关启动不会修改 OpenClaw 包树。一些 Linux npm 设置会把全局包安装在 root 拥有的目录下，例如 `/usr/lib/node_modules/openclaw`。OpenClaw 支持这种布局，因为插件安装/更新命令会写入该全局包目录之外的位置。

强化的 systemd 单元

授予 OpenClaw 对其配置/状态根目录的写入权限，以便显式插件安装、插件更新和 Doctor 清理可以持久化其更改：

```
ReadWritePaths=/var/lib/openclaw /home/openclaw/.openclaw /tmp
```

磁盘空间预检

在包更新和显式插件安装之前，OpenClaw 会尽力对目标卷执行磁盘空间检查。空间不足会生成一条带有已检查路径的警告，但不会阻止更新，因为文件系统配额、快照和网络卷可能会在检查后发生变化。实际的包管理器安装和安装后验证仍然是权威结果。

## [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E8%87%AA%E5%8A%A8%E6%9B%B4%E6%96%B0%E5%99%A8)  自动更新器

自动更新器默认关闭。在 `~/.openclaw/openclaw.json` 中启用它：

```
{
  update: {
    channel: "stable",
    auto: {
      enabled: true,
      stableDelayHours: 6,
      stableJitterHours: 12,
      betaCheckIntervalHours: 1,
    },
  },
}
```

| 渠道 | 行为 |
| --- | --- |
| `stable` | 等待 `stableDelayHours`，然后在 `stableJitterHours` 范围内应用确定性抖动（分散发布）。 |
| `beta` | 每隔 `betaCheckIntervalHours` 检查一次（默认：每小时），并立即应用。 |
| `dev` | 不会自动应用。请手动使用 `openclaw update`。 |

Gateway 网关还会在启动时记录一条更新提示（可用 `update.checkOnStart: false` 禁用）。
对于降级或事故恢复，请在 Gateway 网关环境中设置 `OPENCLAW_NO_AUTO_UPDATE=1`，以便即使配置了 `update.auto.enabled` 也阻止自动应用。除非同时禁用 `update.checkOnStart`，否则启动更新提示仍可运行。通过实时 Gateway 网关控制平面处理器请求的包管理器更新
会在包切换后强制执行一次非延迟、无冷却的更新重启。这样可以
避免旧的内存中进程停留过久，从已被替换的包树中延迟加载 chunks。
对于受监管的安装，Shell `openclaw update` 仍是首选路径，因为它可以在更新前后停止并
重启服务。

## [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E6%9B%B4%E6%96%B0%E5%90%8E)  更新后

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/install/updating#%E8%BF%90%E8%A1%8C-doctor)

运行 Doctor

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/install/updating#)

```
openclaw doctor
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/install/updating#)

迁移配置、审计私信策略，并检查 Gateway 网关健康状况。详情： [Doctor](https://docs.openclaw.ai/zh-CN/gateway/doctor)

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/install/updating#%E9%87%8D%E5%90%AF-gateway-%E7%BD%91%E5%85%B3)

重启 Gateway 网关

5

[Navigate to header](https://docs.openclaw.ai/zh-CN/install/updating#)

```
openclaw gateway restart
```

6

[Navigate to header](https://docs.openclaw.ai/zh-CN/install/updating#%E9%AA%8C%E8%AF%81)

验证

7

[Navigate to header](https://docs.openclaw.ai/zh-CN/install/updating#)

```
openclaw health
```

## [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E5%9B%9E%E6%BB%9A)  回滚

### [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E5%9B%BA%E5%AE%9A%E7%89%88%E6%9C%AC%EF%BC%88npm%EF%BC%89)  固定版本（npm）

```
npm i -g openclaw@<version>
openclaw doctor
openclaw gateway restart
```

`npm view openclaw version` 会显示当前已发布版本。

### [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E5%9B%BA%E5%AE%9A%E6%8F%90%E4%BA%A4%EF%BC%88%E6%BA%90%E7%A0%81%EF%BC%89)  固定提交（源码）

```
git fetch origin
git checkout "$(git rev-list -n 1 --before=\"2026-01-01\" origin/main)"
pnpm install && pnpm build
openclaw gateway restart
```

要返回到最新版本：`git checkout main && git pull`。

## [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E5%A6%82%E6%9E%9C%E4%BD%A0%E9%81%87%E5%88%B0%E9%97%AE%E9%A2%98)  如果你遇到问题

- 再次运行 `openclaw doctor`，并仔细阅读输出。
- 对于源码 checkout 上的 `openclaw update --channel dev`，更新器会在需要时自动引导 `pnpm`。如果你看到 pnpm/corepack 引导错误，请手动安装 `pnpm`（或重新启用 `corepack`），然后重新运行更新。
- 检查： [故障排除](https://docs.openclaw.ai/zh-CN/gateway/troubleshooting)
- 在 Discord 中提问： [https://discord.gg/clawd](https://discord.gg/clawd)

## [​](https://docs.openclaw.ai/zh-CN/install/updating\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [安装概览](https://docs.openclaw.ai/zh-CN/install)：所有安装方法。
- [Doctor](https://docs.openclaw.ai/zh-CN/gateway/doctor)：更新后的健康检查。
- [迁移](https://docs.openclaw.ai/zh-CN/install/migrating)：主版本迁移指南。

[Bun（实验性）](https://docs.openclaw.ai/zh-CN/install/bun) [迁移指南](https://docs.openclaw.ai/zh-CN/install/migrating)

Ctrl+I