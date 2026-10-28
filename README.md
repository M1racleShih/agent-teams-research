# Agent Teams Research

研究 Bob 如何通过 SwarmForge 组织 Agent 团队：角色分工、任务路由、调度决策、并行隔离、成果交接、审查、人工审批和故障恢复。

**不研究单个 Agent 的模型调用、工具循环或内核实现。** Codex / Grok / Claude / Copilot 作为外部执行器；工程质量工具只看调用时的输入、输出及失败怎样影响协作。

本目录 `research/` 是后续工作的主要 Git 仓库。GitHub：<https://github.com/M1racleShih/agent-teams-research>；main 的后续修改通过主题分支和 PR 合并。

## 按需查阅

按当前问题选择入口，不必依次读完整张表，也不必展开文件中的全部引用。

| 想了解什么 | 去哪里查 |
|---|---|
| 项目目标、分支路线与固定快照 | 本页；完整 SHA 见[快照记录](docs/sources/snapshots.json)。 |
| 当前做到哪里、下一步做什么 | [HANDOFF.md](HANDOFF.md#current) 的当前进度与下一步。 |
| 当前阶段读哪些源码、准备哪些研究产物 | [研究计划](docs/research-plan.md#stages)，只读所需阶段。 |
| Agent 的必要约束与按需读取条件 | [AGENTS.md](AGENTS.md)。 |
| 文档怎样分层、何时用图、怎样写得易懂 | [文档与绘图规则](rules/documentation.md)。 |
| 仓库取舍、后端边界或静态兼容性发现 | [仓库调查索引](docs/repository-survey.md)，再选相关主题。 |
| 技能选择、固定版本与安装情况 | [技能记录](docs/skill-selection.md)。 |
| 本地目录、其他机器恢复与 Git 协作 | [工作区说明](docs/setup/workspace.md)。 |

进度只在 HANDOFF 中维护，阶段细节只在研究计划中维护。结论必须区分 prompt 要求、源码约束、测试证据和实际运行结果，不能把静态调查当作运行验证。

<a id="research-route"></a>

## 研究主线与源码入口

源码链接指向 GitHub 的固定提交；本地阅读使用相邻 checkout，路径映射见快照记录。

| 阶段 | 研究视图 | 固定快照 | 核心问题 |
|---|---|---|---|
| 1 | [main](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/README.md) + [two-pack](https://github.com/unclebob/swarm-forge/blob/b75d16062b619e6eab06e03995e344676bc755cd/README.md) | `f4f5fbcae0de` / `b75d16062b61` | 一个任务如何从 coder 交给 cleaner，再完成回传？ |
| 2 | [six-pack](https://github.com/unclebob/swarm-forge/blob/066a62ffa6cbc8c859262536dc579fecf8535ebb/README.md) | `066a62ffa6cb` | 专业分工怎样组织成固定团队？谁拥有哪些产物和验证责任？ |
| 3 | [lieutenant](https://github.com/unclebob/swarm-forge/blob/23653942488281a3c6d9a60b7e9378be0a1ca1c2/README.md) | `236539424882` | 规划、卡片派发、多项目执行怎样分离？ |
| 4 | [squad](https://github.com/unclebob/swarm-forge/blob/319801e105357ad3299ee0492afe01c82247ab9c/README.md) | `319801e10535` | 常驻协调者怎样管理临时 specialist？决策和机械执行由谁负责？ |
| 5 | [sprint-module-squad](https://github.com/unclebob/swarm-forge/blob/63693c14b65a5d10f12529cb7a3cc85088257397/README.md) | `63693c14b65a` | 模块级并行、依赖、集成和冲突修复怎样组织？ |

这是从简单到复杂的阅读路线，不是对分支历史继承关系的断言。`squad` 和 `sprint-module-squad` 属于实验工作流，复杂不代表成熟。

### 对照分支

| 分支 | 固定快照 | 对照价值 |
|---|---|---|
| [four-pack](https://github.com/unclebob/swarm-forge/blob/04662cf96ca83fb45783b4d73cd1daf0acefc0b8/README.md) | `04662cf96ca8` | 中等规模固定流水线，与 six-pack 比较职责拆分 |
| [project-manager](https://github.com/unclebob/swarm-forge/blob/2cc1795fbd1f5cedefd0c371aa1835464d1b4a15/README.md) | `2cc1795fbd1f` | 多项目、多 pack 管理，与 lieutenant 的规划派发区分 |
| [adversaries](https://github.com/unclebob/swarm-forge/blob/68d52bbc99af898405b2a5c08854f8ce37eeced5/README.md) | `68d52bbc99af` | coder/reviewer 反复审查返工，与单向流水线对照 |

<a id="workspace"></a>

## 工作区目录

```text
research-swarm-forge/
+-- research/
|   +-- README.md
|   +-- AGENTS.md
|   +-- HANDOFF.md
|   +-- docs/
|   +-- rules/
|   +-- .dynamic-skills/
|   +-- .agents/skills/
+-- swarm-forge/
+-- worktrees/
```

只有 `research/` 是此研究仓库。相邻源码不纳入 Git，也不移动；技能输出和本机状态由忽略规则排除，配置与版本锁进入 Git。共享仓库没有打包上游源码、技能内容或完整运行环境。
