# SwarmForge 团队协作研究计划

本文件保存各阶段的目标、源码入口和预期产物，不记录当前执行进度。续接工作看 [HANDOFF.md](../HANDOFF.md)，稳定约束看 [AGENTS.md](../AGENTS.md)。这些链接按需要查阅，不要求先读完。

只读当前阶段中与眼前问题有关的部分，再沿实际调用或数据流查看源码；下列入口不是一次性通读清单。先追踪一个任务从接收到完成的过程，再比较团队规模和控制方式，避免把研究写成文件摘要集合。

<a id="stages"></a>

## 阶段索引

| 阶段 | 研究视图 | 核心问题 |
|---|---|---|
| [1](#phase-1) | main + two-pack | 一个任务怎样从 coder 交给 cleaner，再回传并完成？ |
| [2](#phase-2) | six-pack | 固定团队怎样拆分职责与验证责任？ |
| [3](#phase-3) | lieutenant | 规划、卡片路由和多项目派发怎样分离？ |
| [4](#phase-4) | squad | 常驻协调者怎样管理临时 specialist？ |
| [5](#phase-5) | sprint-module-squad | 模块并行、依赖、集成和冲突修复怎样组织？ |

固定提交与对照分支见 [README 的快照表](../README.md#research-route)。源码链接固定到 GitHub 提交；本地 main 位于 `../swarm-forge/`，各分支位于 `../worktrees/<branch>/`，均相对于研究仓库根目录。离线阅读直接使用对应本地文件。这是从简单到复杂的阅读路线，不是已验证的分支演化史；`squad` 和 `sprint-module-squad` 属于实验工作流，复杂不代表成熟。

<a id="phase-1"></a>

## 第一阶段：main + two-pack

目标：先重建一个任务的完整协作时序，不深入外部工程工具。Codex / Grok / Claude / Copilot 作为黑盒执行器，只看 SwarmForge 怎样调用它们。

### 阅读入口

按当前追踪的问题选择入口，不用先读完这六项。

1. **组合团队**：[`get-swarm-forge`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/get-swarm-forge)。确认共享运行时与 pack 定义怎样组合；two-pack 自身不是共享运行时的完整副本。这里只读源码，不执行安装器。
2. **团队拓扑**：[`swarmforge.conf`](https://github.com/unclebob/swarm-forge/blob/b75d16062b619e6eab06e03995e344676bc755cd/swarmforge/swarmforge.conf)。初查配置为 coder/Grok 在项目主 checkout，cleaner/Codex 在独立 worktree，cleaner 使用 `batch back-one`。继续核对这些配置怎样影响启动和交接。
3. **职责与共同规则**：[`constitution.prompt`](https://github.com/unclebob/swarm-forge/blob/b75d16062b619e6eab06e03995e344676bc755cd/swarmforge/constitution.prompt)、[`project.prompt`](https://github.com/unclebob/swarm-forge/blob/b75d16062b619e6eab06e03995e344676bc755cd/swarmforge/constitution/articles/project.prompt)、[`coder.prompt`](https://github.com/unclebob/swarm-forge/blob/b75d16062b619e6eab06e03995e344676bc755cd/swarmforge/roles/coder.prompt)、[`cleaner.prompt`](https://github.com/unclebob/swarm-forge/blob/b75d16062b619e6eab06e03995e344676bc755cd/swarmforge/roles/cleaner.prompt)，以及 main 的[共享文章](https://github.com/unclebob/swarm-forge/tree/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/constitution/articles/)。区分共同规则、角色要求和程序实际检查。
4. **启动与隔离**：[`swarmforge.bb`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/scripts/swarmforge.bb)。追踪配置解析、worktree/session 建立、指令组装和 Agent 启动；后端内部不展开。
5. **协作协议**：[`handoff-protocol.md`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/handoff-protocol.md)。再按所追踪的步骤对照 [`swarm_handoff.bb`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/scripts/swarm_handoff.bb)、[`handoffd.bb`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/scripts/handoffd.bb)、[`ready_for_next.bb`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/scripts/ready_for_next.bb)、[`merge_and_process.bb`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/scripts/merge_and_process.bb)、[`done_with_current.bb`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/scripts/done_with_current.bb)。
6. **操作者入口与结束条件**：[`pack_board.bb`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/scripts/pack_board.bb)、[`pack_web.bb`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/swarmforge/scripts/pack_web.bb)。确认 New Task 怎样进入角色、交付怎样影响看板、何时进入 Done。

### 预期研究笔记

以下是待完成的产物，不是已经验证的行为描述。笔记统一保存在本研究根目录的 `docs/`。

- **团队结构图**：说明操作者、dashboard、Agent、worktree、handoff daemon 与 Git 的关系。
- **任务时序图**：从 New Task → coder → commit/handoff → cleaner → terminal handback → Done 开始核对，标出各步执行者与状态落盘位置；若源码与此路径不同，以证据修正。
- **角色职责表**：记录输入、允许修改的产物、验证责任、输出、接收者和禁止行为。
- **约束落实表**：分别记录 prompt 要求、helper 校验、状态机条件、Git 约束和人工判断；单列已有测试证据及是否实际运行。
- **失败路径表**：分析交付失败、合并冲突、批处理不兼容、进程退出和重启时的处理方式，明确未覆盖或尚未查清的情况。

<a id="phase-2"></a>

## 第二阶段：固定专业团队 six-pack

从 [`roles/`](https://github.com/unclebob/swarm-forge/tree/066a62ffa6cbc8c859262536dc579fecf8535ebb/swarmforge/roles/) 与 [`swarmforge.conf`](https://github.com/unclebob/swarm-forge/blob/066a62ffa6cbc8c859262536dc579fecf8535ebb/swarmforge/swarmforge.conf) 开始，制作六角色责任矩阵。重点比较职责独立性、审查是否能修改产品、批处理、反向传播及最终收敛，而不是逐个研究角色调用的分析工具。需要中等规模对照时，再看 four-pack。

<a id="phase-3"></a>

## 第三阶段：规划与派发 lieutenant

从 [host lieutenant](https://github.com/unclebob/swarm-forge/blob/23653942488281a3c6d9a60b7e9378be0a1ca1c2/swarmforge/roles/lieutenant.prompt)、[项目模板](https://github.com/unclebob/swarm-forge/blob/23653942488281a3c6d9a60b7e9378be0a1ca1c2/.swarmforge/project-pack/swarmforge/swarmforge.conf) 和 [`card_type.bb`](https://github.com/unclebob/swarm-forge/blob/23653942488281a3c6d9a60b7e9378be0a1ca1c2/swarmforge/scripts/card_type.bb) 开始。区分团队之外的规划者与团队内部的执行角色，追踪等待卡片、审批、路由和派发条件。需要多项目、多 pack 管理对照时，再看 project-manager。

<a id="phase-4"></a>

## 第四阶段：动态团队 squad

从 [`squad.conf`](https://github.com/unclebob/swarm-forge/blob/319801e105357ad3299ee0492afe01c82247ab9c/swarmforge/squad.conf)、[常驻角色](https://github.com/unclebob/swarm-forge/tree/319801e105357ad3299ee0492afe01c82247ab9c/swarmforge/roles/)、[临时角色模板及输入输出要求](https://github.com/unclebob/swarm-forge/tree/319801e105357ad3299ee0492afe01c82247ab9c/swarmforge/role-templates/)、[`squad_next.clj`](https://github.com/unclebob/swarm-forge/blob/319801e105357ad3299ee0492afe01c82247ab9c/swarmforge/scripts/squad_next.clj) 开始，再按具体问题追踪 daemon、assignment、spawn 和 retire。重点查清哪些决策是确定性的，哪些判断留给 Agent，角色的输入输出要求在哪些位置被读取和检查。

<a id="phase-5"></a>

## 第五阶段：模块并行 sprint-module-squad

从 [`sprints.md`](https://github.com/unclebob/swarm-forge/blob/63693c14b65a5d10f12529cb7a3cc85088257397/sprints.md)、[`squad_sprint.clj`](https://github.com/unclebob/swarm-forge/blob/63693c14b65a5d10f12529cb7a3cc85088257397/swarmforge/scripts/squad_sprint.clj) 和 [`squad_sprint_next.clj`](https://github.com/unclebob/swarm-forge/blob/63693c14b65a5d10f12529cb7a3cc85088257397/swarmforge/scripts/squad_sprint_next.clj) 开始。重点分析 Sprint 0、模块接口、依赖就绪、质量汇合点、daemon 合并职责与 merger 修复。

需要比较反复审查与返工时，再看 adversaries；它不是推进到本阶段的前置阅读任务。
