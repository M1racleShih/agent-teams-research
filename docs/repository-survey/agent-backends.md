# Agent 后端研究边界

以下保留首轮调查记录，不是本次迁移重新核验外部仓库的结果，也不是运行验证。证据位置沿用调查时记法；当前本地路径映射见[工作区说明](../setup/workspace.md)。

## 5. Agent 后端（明确排除出本次源码研究范围）

SwarmForge 的这一层主要是通过外部 CLI 进程启动 Agent，再用 tmux 输入与终端捕获交互；它不是把多个 Agent SDK 嵌入进程的框架。模型 API、工具循环、上下文和权限机制主要属于后端。

本次只研究 SwarmForge 如何启动、唤醒、观察和停止这些进程，不 clone 或追踪后端内核。以下为先前调查得到的边界说明。

| 仓库 | 源码研究价值 | 建议 |
|---|---|---|
| [openai/codex](https://github.com/openai/codex) | 公开 Rust CLI/Agent 实现；对应 `codex -C`、`--no-alt-screen`、`--yolo`、工作目录信任配置 | 本次不 clone；SwarmForge 的适配代码和 CLI 文档足够界定团队运行边界。 |
| [xai-org/grok-build](https://github.com/xai-org/grok-build) | 官方公开 `grok` CLI/TUI 和 agent runtime 源码；README 明确说明可构建，官方发布命令名为 grok | 本次不 clone。复现时仅需核对具体 CLI 版本对 `--minimal`、`--rules`、`--verbatim`、权限参数的支持，不声称当前源码就是作者当时安装的二进制。 |
| [anthropics/claude-code](https://github.com/anthropics/claude-code) | 公开仓库主要提供文档、插件、示例、脚本及反馈资料，不能视作完整核心 Agent 实现 | 按需浏览/clone 插件及集成资料即可，不列首批核心源码。 |
| [github/copilot-cli](https://github.com/github/copilot-cli) | 本次根目录核验包含 README、changelog、install.sh、license、GitHub 配置，并非完整 Agent 核心源码 | 查 CLI 文档和发布行为即可。其公开 license 也不是一般的开放源码许可。 |

没有证据表明 SwarmForge 依赖 OpenAI 的同名 `swarm` 库、LangGraph、AutoGen、CrewAI、MCP server 仓库或 Claude Agent SDK；不要因为领域相近就当成必克隆依赖。

Grok Build README 提到自身含 Codex / OpenCode 的源码移植，这是 **Grok 后端自己的实现关联**，不能据此推断 SwarmForge 直接依赖 OpenCode。

证据：`swarm-forge/swarmforge/scripts/swarmforge.bb:468`、`:490`、`:548`；[Grok Build README](https://github.com/xai-org/grok-build#repository-layout)；[Copilot license](https://github.com/github/copilot-cli/blob/main/LICENSE.md)。
