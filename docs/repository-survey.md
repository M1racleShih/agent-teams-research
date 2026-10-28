# SwarmForge 仓库调查索引

调查对象是 <https://github.com/unclebob/swarm-forge>。研究目标是 Bob 怎样组织 Agent 团队，不是单个 Agent 内核或工程工具内部实现。

## 按问题查阅

| 当前问题 | 对应记录 |
|---|---|
| 为什么现有仓库足够、调查了什么、各分支怎样区分 | [调查范围与分支视图](repository-survey/scope-and-branches.md) |
| 工程工具有哪些、何时才需要深入 | [工程工具备查](repository-survey/external-tools.md) |
| Agent 后端哪些内容不属于研究范围 | [后端边界](repository-survey/agent-backends.md) |
| 哪些外部仓库只在特定问题下考虑 | [可选外部仓库](repository-survey/optional-repositories.md) |
| 固定依赖、工具任务名、平台路径和运行隔离有哪些静态问题 | [静态发现](repository-survey/static-findings.md) |
| 首轮调查提出过哪些研究问题与准备建议 | [原研究问题](repository-survey/research-questions.md) |

只读当前问题对应的记录，不顺序预读所有子文档。原长文按主题拆分，调查结论、源码位置和未运行验证的限制保持不变。

## 记录的适用范围

- 这些是首轮静态调查记录，不代表已完成各工作流源码深读或运行实验；外部链接并非迁移时再次核验的最新结果。
- 工具和后端清单用于备查，不是新增 clone、安装依赖或启动 Agent 的授权。现有上游 clone 与八个分支视图足够开始团队协作研究。
- 历史证据中的 `swarm-forge/<file>` 表示上游 clone 内的文件；从当前研究仓读取时使用 `../swarm-forge/<file>`，分支源码使用 `../worktrees/<branch>/`。
- 完整固定提交见[快照记录](sources/snapshots.json)；当前计划见[研究计划](research-plan.md#stages)，当前进度仅在 [HANDOFF](../HANDOFF.md#current) 维护。
