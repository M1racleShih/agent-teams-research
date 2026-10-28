# 当前研究交接

本文件只记录进度与接续动作，不保存完整研究计划。只有续接或推进源码研究时才需要读取；文档整理或技能管理不因此转成研究任务。

<a id="current"></a>

## 当前进度

- 主要工作仓库已迁至 `research/`，是独立 Git 仓库；GitHub 目标为 `M1racleShih/agent-teams-research`，后续 main 修改走主题分支和 PR。父目录只留工作区指引。
- 当前阶段：**第一阶段 main + two-pack，调查和资料准备已完成，完整任务时序尚未核实。**
- 已有[完整上游 clone](https://github.com/unclebob/swarm-forge/tree/f4f5fbcae0de6f7dcc26e82400334227647cfdb2)，本地位于 `../swarm-forge/`，以及 `../worktrees/` 下的 8 个 detached worktree；沿用首轮调查的固定提交，没有 fetch 更新。提交与目录见 [README](README.md#research-route)，核对快照时再查。
- 六个已确认技能已安装到项目 `.agents/skills/`，固定了版本；没有更新 pool 或扩装其他技能。只有处理技能问题时才查[安装记录](docs/skill-selection.md)。
- README 已改为项目导航；详细阅读入口在研究计划中，AGENTS.md 保存必要约束和按需读取条件。
- 尚未完成各工作流的源码深读，也没有团队结构图、完整任务时序、角色职责表、约束落实表或失败路径笔记。此前的入口调查不能当作这些产物已经完成。

<a id="next"></a>

## 下一步

在用户要求推进源码研究时：

1. 从 [`../swarm-forge/get-swarm-forge`](https://github.com/unclebob/swarm-forge/blob/f4f5fbcae0de6f7dcc26e82400334227647cfdb2/get-swarm-forge) 只读追踪 main 的共享运行时与 two-pack 配置、角色怎样组合，记录对应提交、文件位置与尚未查明的问题；不执行安装器。
2. 然后核对 [`../worktrees/two-pack/swarmforge/swarmforge.conf`](https://github.com/unclebob/swarm-forge/blob/b75d16062b619e6eab06e03995e344676bc755cd/swarmforge/swarmforge.conf)，区分配置声明和程序实际处理；再沿当前问题进入启动或交接实现。

需要阶段目标、后续入口或产物清单时，只读[研究计划的第一阶段](docs/research-plan.md#phase-1)。本次接续不要求预读 six-pack、lieutenant、squad 或 sprint-module-squad。

<a id="blockers"></a>

## 阻塞与运行状态

- 当前没有阻止静态研究的已知阻塞。
- 尚未启动 swarm / 后台 Agent、安装运行依赖或执行上游项目测试，不能据此声称运行可靠。此前调查时 PATH 未发现 `bb`；只有准备运行实验时才重新检查，不是阅读源码的前置条件。
- 运行实验仍需用户另行授权，并使用独立 disposable 目标项目，不能直接在研究 checkout 中运行安装器或 swarm。需要排查兼容性时，再查[静态发现](docs/repository-survey/static-findings.md)。

## 何时更新

实际研究有进展、出现阻塞、切换阶段或需要交接时，更新当前位置、已完成产物链接、验证范围和下一步。不要追加逐轮聊天记录，也不要把完整研究计划、规则或源码清单复制到这里。
