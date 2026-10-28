# 研究工作区与 Git 协作

本项目的主要仓库是 `research/`，GitHub 名称为 `M1racleShih/agent-teams-research`。研究资料与被研究源码分开管理，工作区布局见 [README](../../README.md#workspace)。

## 从研究仓库工作

- 启动 Agent 前先进入 `research/`；本仓库的 `AGENTS.md`、交接记录和技能在这里生效。
- 工具若仍从父目录启动，文件操作使用明确的 `research/` 前缀，Git 操作使用 `git -C research`；不要在父目录继续写研究资料。
- main 的本地源码是 `../swarm-forge/`，分支源码是 `../worktrees/<branch>/`。这些目录保持原位，没有加入本仓库或改成 submodule。
- 文档中的 SwarmForge 源码链接指向 GitHub 上的固定提交，网页阅读不依赖本机目录。离线研究使用相同提交的本地文件，不需要逐一联网打开链接。

## 在其他机器恢复资料

1. 将此仓库 clone 为工作区下的 `research/`。
2. 仅在需要本地源码研究时，按[快照记录](../sources/snapshots.json)中的仓库地址、完整 SHA 和相对路径准备相邻 checkout。不能用最新分支代替固定提交；未经授权不自动 clone 或 fetch。
3. 仅在需要技能时安装并检查 dskills。若本地 pool 保留了锁文件中的六个版本，在研究仓运行 `dskills sync --dry-run`，检查后再运行 `dskills sync`。

版本锁不是技能内容备份：其中部分技能来自本机路径，新机器没有对应 pool object 时，必须另外取得准确版本并核对哈希，不能保证仅凭本仓库自动恢复。不要为此自动更新 pool 或用 current 版本替换。

## Git 与 main 分支

- 本仓库公开；提交使用 GitHub no-reply 邮箱，不把凭据、运行缓存或本机状态提交到远程。
- main 的维护策略是：修改通过 PR 合并，禁止强推与删除，讨论须解决，管理员同样受约束。
- 当前没有 CI，不要求不存在的检查；单人维护不强制另一位审查者批准。需要新的审批或 CI 策略时另行调整。
- 后续从主题分支提交并创建 PR，不直接 push main，不为了方便绕过保护。

技能输出 `.agents/skills/` 和 dskills 状态、历史缓存不提交；共享配置、版本锁和忽略文件进入 Git。具体版本与来源见[技能记录](../skill-selection.md)。
