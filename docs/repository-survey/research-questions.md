# 首轮调查的研究问题与准备建议

以下保留首轮调查记录，不是本次迁移重新核验外部仓库的结果，也不是运行验证。证据位置沿用调查时记法；当前本地路径映射见[工作区说明](../setup/workspace.md)。

下文目录树是迁移前布局，准备动作与可选 clone 不是当前任务；现有布局见 [README](../../README.md#workspace)，当前进度见 [HANDOFF](../../HANDOFF.md#current)。

## 8. 当前行动清单：只展开团队分支

```text
research-swarm-forge/
  README.md                    # 团队研究索引、阶段与源码入口
  swarm-forge/                 # 已有：主仓库
  worktrees/                   # 已展开 8 个分支的研究视图
  docs/repository-survey.md     # 本调查
```

当前不必创建 `upstreams/` 或批量 clone 工程工具、Agent 后端。只有后续确认需要深入验收规格契约，才可选执行：

```bash
# 可选，不是研究团队编排的前置条件；本次未执行
mkdir -p upstreams
git clone https://github.com/unclebob/Acceptance-Pipeline-Specification.git \
  upstreams/Acceptance-Pipeline-Specification
```

## 9. Agent Teams 的研究主线与问题

### 从简单团队到动态团队

1. **团队的声明方式**：`get-swarm-forge`、`swarmforge.conf`、`constitution.prompt`、articles、roles。区分团队拓扑、共同规则和角色专属职责；不要把自然语言要求直接当作代码已强制执行的约束。
2. **最小协作闭环（main + two-pack）**：coder → commit → handoff → daemon → cleaner → terminal handback → board Done。先建立完整任务时序，不引入验收工具实现。
3. **成果交接与并行隔离（main）**：`handoff-protocol.md` → `handoff_lib.bb` / `swarm_handoff.bb` → `ready_for_next*` → `merge_and_process*` → `done_with_current*` → `handoffd.bb`。追踪 worktree/branch、队列、审计、审批、批处理、反向传播和冲突处理。
4. **固定团队分工（six-pack，对照 four-pack）**：逐一阅读角色提示和局部规则，制作“输入 → 允许修改的产物 → 验证责任 → 输出 → 下一角色”表；关注审查是否独立、职责是否重叠、返工和结束如何定义。
5. **计划与调度（lieutenant，对照 project-manager）**：host lieutenant 提示、项目模板、typed card routes、`forge*`、`pack_board*`、`pack_web*`。区分面向操作者的规划建议、多项目管理和实际卡片派发。
6. **动态 specialist（squad）**：persistent roles、role-templates、`.contract.edn`、`worker-common.prompt`、`squad_next*`、`squadd*`、`squad_assign*`、`squad_spawn*`、`squad_retire*`。追踪 advisor 决策、daemon 机械执行及 leader 判断之间的边界。
7. **模块并行与集成（sprint-module-squad）**：`sprints.md`、`squad_sprint*`、`squad_sprint_next*`、模块图/顺序模板及 merger 角色。关注 Sprint 0、模块接口、依赖就绪、集成归属、取消和冲突恢复。
8. **对抗评审对照（adversaries）**：coder/reviewer 角色、recommendations 与 approval artifacts；比较反复质疑的双角色循环和固定流水线质量角色的差别。

### 每条工作流都要回答的六个问题

- **谁决定下一步？** LLM 角色、确定性 advisor、daemon 还是操作者？
- **交接的是什么？** 聊天文本、任务引用、提交 SHA、文件产物还是一组 evidence？
- **什么保证可以并行？** worktree 隔离、模块接口、依赖顺序与资源容量如何配合？
- **规则在哪里落实？** 哪些只是 prompt，哪些被 contract/helper/state machine 校验，哪些只有人工审批？
- **失败怎么处理？** 合并冲突、交付重试、审批拒绝、worker 退出、取消与重启各由谁接手？
- **什么才算完成？** Agent 自述、交接提交、验证证据、审批记录和看板 Done 的关系是什么？

**当前推荐准备规模：现有主仓库 + 各分支研究视图；无需新增外部仓库。** 工具实现和 Agent 内核不是本次主线。
