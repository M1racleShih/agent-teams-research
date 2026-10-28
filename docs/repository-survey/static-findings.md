# 运行前的静态发现

以下保留首轮调查记录，不是本次迁移重新核验外部仓库的结果，也不是运行验证。证据位置沿用调查时记法；当前本地路径映射见[工作区说明](../setup/workspace.md)。

## 7. 复现前应记录的静态发现

### 7.1 不要把历史固定依赖和当前上游混在一起

主仓库 `bb.edn` 固定：

```text
crap4clj   e068673a852a8142323ac680fa3366de65bc2227
cloverage  61e3cac426e9907a9dd01c37597f85c71a57ff90
           :deps/root "cloverage"
```

工程宪法又要求 Agent 获取工具的最新版本。这是两种不同的解析路径：

- 开发者运行主仓库验证任务：使用 `bb.edn` 的固定依赖。
- Agent 通过工具助手安装：按 catalog/source 路径获取仓库，再生成 wrapper。

研究时应记录每个仓库的 SHA 和实际执行入口；不要 clone latest 后就默认等价于作者的整个验证环境。研究 clone 不建议使用 `--depth 1`，否则不便对照固定历史提交。

### 7.2 工具 catalog 与上游 task 名存在静态不匹配

主仓库 `swarm_tool.bb` 根据 `:bb-task` 生成 `bb --config <tool>/bb.edn <task>`。本次检查到：

| 工具身份 | SwarmForge main catalog | 上游当前 bb.edn 的任务 |
|---|---|---|
| crap4clj | `crap4clj` | `crap` |
| clj-mutate | `clj-mutate` | `mutate` |
| dry4clj | `dry4clj` | `deintroverter` |

这说明“wrapper 存在”不能自动证明“工具可执行”。`require-tool!` 检查 wrapper 可执行权限，并不调用工具做健康检查。此处尚未运行复现，不把静态不匹配扩大成整个工作流必定失败的结论。

另外，`ensure-source!` 发现现有目录含 `bb.edn` 就复用，并不自动 fetch 最新提交；它的实际缓存行为需要与宪法中 latest 的要求对照研究。

证据：`swarm-forge/swarmforge/scripts/swarm_tool.bb:86`、`:98`、`:107`、`:114`、`:162`。

### 7.3 实验分支工具安装机制也不相同

`sprint-module-squad` 的 tool-table 安装命令包含 `/Users/unclebob/projects/...` 绝对路径，安装脚本接收工具根目录与 task 两个参数；`squad` 对应脚本则接收一个 task，读取 `SWARMFORGE_TOOL_SRC_DIR`。

因此研究 Linux 可移植性时需要分别核对，不能因为分支名字相近就套用同一套安装假设。

### 7.4 运行实验应使用独立目标仓库

启动器为 Codex/Copilot 添加 yolo 参数，为 Claude/Grok 配置 bypass permissions，还会为 Codex 写入 worktree 的信任配置。Git worktree 提供代码 checkout 隔离，**不等于操作系统或权限沙箱**。

首轮只做静态研究；运行时用专门的 disposable 示例项目，不要直接以研究仓库或个人工作项目作为实验目标。
