# Agent Teams 研究：dskills 项目技能安装记录

状态：**已获用户确认，首批五个技能及追加的 `plain-docs` 均已安装到研究根目录的 `.agents/skills/`，共六个。**

## 目标与筛选依据

本项目研究 Bob 的团队分工、调度、交接、并行、审查与恢复机制，不研究单个 Agent 内核，也不改造被研究的上游项目。

筛选阶段读取了本地 dskills pool 的 121 个技能简介，核查了 16 个相关技能的完整 SKILL.md（含 manager），并进一步检查推荐项的关键资源和版本来源。用户确认后，在研究根目录执行了 `init` 和固定版本的 `plug`；未重新导入或更新 pool，也未执行任何技能自带脚本。

技能只能提供工作方法，不能代替实际源码阅读，也不会增加工具权限。特别是：

- 解释当前代码与替 Bob 设计新系统是两件事。
- prompt 要求、代码约束、测试覆盖和实际运行结果必须分开记录。
- 不因安装了研究技能而启动后台 Agent、付费实验或 swarm。

## 已安装组合：4 个研究技能 + 1 个文档整理技能 + 1 个 pool 管理入口

| dskills ID | 作用 | 在本研究中的具体用途 | 使用边界 |
|---|---|---|---|
| `mp-domain-modeling` | 统一领域术语 | 澄清 pack、forge、role、worker、assignment、card、story、handoff 等概念，记录分支间语义差异 | 只整理研究项目自己的术语表；行为和实现证据另存研究笔记。不改 Bob 的代码、命名或角色定义，也不把推测写成 Bob 的 ADR。 |
| `web-research` | 一手资料与来源核验 | 在需要时查作者说明、官方文档、仓库历史等背景，记录版本、来源和不确定性 | 这是补充来源调查，不是把逐行源码追踪替换成网页搜索；源码是实现事实的主要依据。 |
| `diagram-design` | 用合适的图解释团队机制 | 团队结构、任务交接时序、角色泳道、worker 生命周期、等待/重试/取消/完成状态 | 每张图必须来自已核实的行为，不用模板补造机制；复杂流程拆图。此技能首次使用有样式确认步骤，默认输出为 HTML + 内嵌 SVG。 |
| `verify-before-complete` | 限制结论强度 | 完成一章前核对关键结论是否有对应分支、提交和源码证据，明确哪些尚未运行验证 | 不要求为了声称完成而编造测试；静态阅读不能证明运行可靠性。 |
| `dynamic-skills` | 按需使用本地 pool | 后续某个阶段需要其他能力时，先查阅而不是预装整套 package | 管理入口，不是研究方法本身；不据此自动扩装、更新 pool 或激活其他技能。 |
| `plain-docs` | 整理已有文档，使其清晰易读 | 根据用户追加要求安装，用于研究报告、说明文字及图注的后期整理 | 保留事实、引用、代码标识符和不确定性；不代替事实核验，不改写原始证据。 |

不直接应用 `mp-discovery`、`mp-architecture`、`agent-platform` 或 `specs-and-docs` 整包：其中混有设计改造、原型、issue tracker、GSD 文件布局等本研究暂不需要的流程。

## 为什么暂不选其他看起来相关的技能

| 候选 | 暂不选择的理由 |
|---|---|
| `mp-research` | 一手资料与引用要求很贴合，但当前版本开头要求启动 background agent；当前会话未提供对应委派工具，也没有必要为此引入额外 Agent。不能只看名字就把它当作无条件适用的源码研究流程。 |
| `harness-creator` | 包含有用的协调分析视角，但主要目标是创建、审计并改善 harness，带生成脚本与固定结构评估；容易把描述 Bob 的机制转成改造项目。 |
| `write-docs` | 面向读者的流程有价值，但当前版本明确排斥 trunk docs 中的文件路径和行号，并含 GSD 衔接要求；不适合作为本项目源码证据笔记的主方法。 |
| `codebase-design` / `mp-codebase-design` | 主要用于设计或修改模块边界；本阶段先理解现有团队，不重构被研究系统。 |
| `handoff` | 强依赖 GSD slice、STATE 和汇总产物；本项目没有这套运行环境。 |
| `mp-handoff` | 可用于显式的跨会话临时交接，但默认将文档放在 OS 临时目录，不适合直接作为项目长期研究记忆；先依靠本项目 docs。 |
| `drawio` | 是可编辑图的合理替代，支持离线流程，但本轮与 diagram-design 二选一，避免两套绘图工作流重叠；需要可手工编辑的 .drawio 时再考虑。 |
| `fireworks-tech-graph` | SVG 技术图很有价值，但本次更重视时序、状态与泳道；diagram-design 的类型和语义模式与当前研究产物更直接匹配。 |
| `agent-workflow-eval` | 适合今后对比工作流改变的实际效果；目前是静态研究，不是授权运行 A/B Agent 实验。 |
| `spike-wrap-up` | 等阶段研究结束、确认有可复用操作知识时再考虑；不应在研究尚未完成时提前生成结论型 skill。 |

## 已审阅的推荐版本

以下为 pool 内容哈希，不是 SwarmForge 提交。安装时应沿用这些已审阅版本；若改用 pool 后续版本，需要重新核对有无流程变化。

```text
mp-domain-modeling
  dc71783b0c0ab062f4e248ef84820e02b115f033b75883de6194e60087948c7a
web-research
  c663d9da1163b66fcd598457834814e9d5ea84db76ccdf67657f0e02981e3fa9
diagram-design
  c48bc9cf600db54148d9f55710bbecdab56598aa43bbbe0a7ed59c4c3735e6c0
verify-before-complete
  de7d6e977b17adbff0029fed4798f16ec9a2d7e03aeb58261137f3cf36b2f08c
dynamic-skills
  0fa76a60621773144f092787884a89f45a9c3fd592b73d638cba2886d129423f
plain-docs
  00a637d1eb0bcc14edc3c9bcfb4d97400eefea5141eaa1320463a7bbe9dbe0e2
```

推荐项的 SKILL.md、术语/ADR 模板、图解类型参考、语义模式与基本自检脚本均在对应 pool object 中存在。这里仅做存在性与文本核查，没有执行绘图或验证脚本，不声称其全部依赖或导出能力已运行验证。

## 安装记录与验证

- 当前根目录：工作区内的 `research/` 独立 Git 仓库。首批安装曾在父工作区中完成，现随资料迁入；未写入相邻 `../swarm-forge/` 或 `../worktrees/*`。
- 原生输出：`.agents/skills/<skill-id>/`，使用 copy 模式，不是指向 pool 的 symlink。
- dskills 选择了 `codex` 路径适配器，仅用于生成用户指定的 `.agents/skills`；没有创建 `.pi/`、`.claude/` 或 `.codex/` 目录。Pi 也支持读取项目 `.agents/skills`，无需再复制一份。
- 配置：`.dynamic-skills/config.json`；版本锁：`.dynamic-skills/lock.json`。六个版本均与上面的完整哈希一致；迁移后用 `dskills config --track` 改为 `track: true`，只提交共享配置、版本锁与忽略文件，不提交生成副本、状态或历史缓存。
- 用户追加授权后，仅新增 `plain-docs`；首批五项的版本保持不变。
- `dskills --json status` 返回 `healthy: true`、`issues: []`，已选择技能恰好为上述六项。
- `LC_ALL=C dskills --json sync --dry-run` 返回空计划，表明没有待补建或待同步的输出。
- 已逐文件比较六个安装目录与对应 pool object 的 SHA-256，所有文件一致；全部 9 个上游 checkout 仍干净。
- Pi 应从受信任的 `research/` 启动；已在该目录启动的会话可用 `/reload` 刷新发现。若旧会话仍以父目录为工作目录，仅 `/reload` 不会自动切入子仓库，需要在正确目录重新启动。技能实际调用名取自各自 frontmatter，例如 `mp-domain-modeling` 的声明名是 `domain-modeling`。

### 迁移前的本机兼容性问题

首次安装时，父工作区不是 Git 仓库。`plug` 因 dskills 仅识别英文的非 Git 错误、而本机 Git 输出中文而中止；当时技能尚未写入，项目状态保持健康。随后仅为安装命令临时设置 `LC_ALL=C`，首批五项安装成功，没有修改系统语言设置或 dskills 源码。现在 `research/` 已建立 Git 仓库，不再是原来的非 Git 目录场景。

锁文件保留技能的原始来源信息，部分为本机绝对路径，并非凭据或跨机器可用的下载地址。换机恢复限制见[工作区说明](setup/workspace.md)。

安装技能不同时授权创建术语表、生成图、修改上游 instructions 或运行团队实验；这些产物按后续具体研究任务产生。
