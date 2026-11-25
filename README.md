# OpenCode Harness

> 将本机安装的 `opencode` 接入 Cindy，作为 Cindy 任务执行引擎（Harness / ACP）。

## 项目说明

OpenCode Harness 是一个 Cindy 插件。安装后，Cindy 可以把任务交给本机的 `opencode` 处理，并通过 ACP（Agent Client Protocol）驱动会话。

- 插件 ID：`opencode-harness`
- 当前版本：`1.0.0`
- 最低 Cindy 版本：`0.1.145`
- 执行协议：ACP / JSON-RPC stdio
- 工作模式：按需启动，空闲 120 秒后退出

通过 `node/acp.cjs` 将 stdin/stdout 透传给 `opencode acp`，由 Cindy 统一驱动 ACP 会话。

## 主要文件

- `ghost.json`：插件清单、版本与执行引擎声明。
- `main.js`：插件入口。
- `settings.html`：引擎设置面板，可检测本机命令是否可用。
- `node/worker.cjs`：Cindy 侧的 JSON-RPC stdio worker。
- `node/acp.cjs`：ACP 进程桥接。
- `*.cindy`：可安装的插件包。

## 发布包

- `opencode-harness-1.0.0.cindy`

通常安装与当前 Cindy 环境匹配的最新版本即可；旧版本仅用于兼容或回溯。

## 前置条件

- 已安装 Cindy `0.1.145` 或更高版本。
- 本机已安装 `opencode`，并确保 Cindy 进程可以通过 `PATH` 找到它。
- 本插件仅负责把本机命令桥接到 Cindy，不负责安装或升级 `opencode`。

请在终端确认命令可用：

```sh
command -v opencode
```

## 安装

1. 从本目录的 `发布包` 中选择一个 `.cindy` 文件。
2. 在 Cindy 中安装该插件包。
3. 打开插件设置页面，检查本机 `opencode` 是否可用；如未检测到，请检查 `PATH` 后点击“重新检测”。
4. 在 Cindy 的任务执行引擎中选择 `OpenCode Harness`。

## 工作原理

Cindy 启动插件 worker，worker 按需拉起 ACP 桥接进程。桥接进程与本机 `opencode acp` 之间通过标准输入输出交换 JSON-RPC 消息，因此不需要额外网络端口。

## 图标来源

插件图标 `assets/icon.png` 取自对应项目的官方网站或官方 GitHub 仓库；`ghost.json` 已声明 `icon` 字段。
## 项目链接

- Cindy 官网：https://cindy.app/
- Cindy 源码：https://github.com/makecindy/cindy
- Cindy Harness 文档：https://cindy.cn/
- Cindy 插件开发指南：https://github.com/makecindy/cindy/tree/main/apps/desktop/resources/system-skills/cindy-plugin-creator

### OpenCode 官方链接

- OpenCode 官网：https://opencode.ai/
- OpenCode 源码仓库：https://github.com/anomalyco/opencode

本仓库只维护 Cindy 侧插件与发布包；`opencode` 的安装、登录和模型配置请以其官方渠道为准。
## 目录状态

本目录是独立 Git 仓库，用于单独维护 OpenCode Harness 插件的源码与发布包。
