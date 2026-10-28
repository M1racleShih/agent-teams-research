# 调查范围与分支视图

以下保留首轮调查记录，不是本次迁移重新核验外部仓库的结果，也不是运行验证。证据位置沿用调查时记法；当前本地路径映射见[工作区说明](../setup/workspace.md)。

本页的建目录命令已在父工作区执行过，仅供历史记录，不要在 `research/` 中重复运行。

## 1. 按实际研究目标修订的结论

研究对象是 <https://github.com/unclebob/swarm-forge>，目标是 **Bob 如何组织 Agent 团队**：角色职责、任务分解、调度、成果交接、并行隔离、审查、人工审批与故障恢复；不是单个 Agent 的模型调用、工具循环或底层实现。

- **当前无需新增 clone。** 团队编排实现、配置、角色提示和协作协议都在已有仓库的不同分支中。没有发现研究这一主题必须补齐的独立团队编排仓库。
- **第一优先级是展开已有仓库的分支。** `main` 不是完整产品，只看它会遗漏固定团队定义、动态 worker 和 sprint 控制平面。
- **撤回“首批补齐 9 个工具仓库”的建议。** 那是工程工具链研究的资料清单，不是 Agent Teams 研究的前置条件。CRAP、mutation、DRY 等先当作角色调用的外部工具，关注其输入、输出和证据如何进入交接即可。
- **Codex / Grok / Claude / Copilot 按黑盒后端处理，不研究其内核，也不建议为此 clone。** 只读 SwarmForge 自己的启动适配及交互契约。
- **最多可选 APS 一个外部仓库。** 只有需要细看 specifier → coder 之间的验收规格契约时才补；初期读本仓库角色提示已经足够。

下文保留已核实的工具与后端清单作为备查，不再作为当前行动清单。本次没有执行 clone，没有启动 swarm 或 Agent，也没有修改上游 checkout。

## 2. 调查范围与证据基线

本地目录：`swarm-forge/`。

- origin：`https://github.com/unclebob/swarm-forge`
- HEAD：`f4f5fbcae0de6f7dcc26e82400334227647cfdb2`
- 非 shallow clone；已配置抓取全部远程分支。
- 已存在 9 个分支的远程跟踪引用；本次 `git ls-remote --heads origin` 返回的分支 SHA 与所调查的本地引用一致。
- HEAD 没有 Git submodule；本地工作区调查前后保持干净。

调查包含：主分支 README、安装器、`bb.edn`、工程宪法、工具目录及安装实现、Agent 启动实现、dashboard 依赖，以及全部产品/实验分支的 README、配置和实验工具表。通过 GitHub API 核实相关仓库存在性、默认分支，并读取关键上游 README、`bb.edn` / `deps.edn`。

这不是运行验证：当前 PATH 未发现 `bb`，未安装依赖、执行测试或调用模型。因此下文的兼容性问题均标明为静态发现，不声称已经端到端复现。

## 3. 最先补齐的是分支视图，不是重复 clone

| 分支 | 调查时短 SHA | 研究意义 |
|---|---|---|
| `main` | `f4f5fbc` | 共享运行时、安装器、共享工程规则、handoff、dashboard |
| `two-pack` | `b75d160` | coder → cleaner；最小固定流水线，局部规则禁止 Gherkin / acceptance |
| `four-pack` | `04662cf` | specifier → coder → refactorer → architect；规格审批与验收测试 |
| `six-pack` | `066a62f` | 规格、实现、清理、架构、hardender、QA 六个角色 |
| `project-manager` | `2cc1795` | 多项目、多 pack forge；host lieutenant 与项目生命周期 |
| `lieutenant` | `2365394` | 单项目模板、多项目调度；utility/component/QA/review 卡片路由 |
| `squad` | `319801e` | 常驻 leader/troubleshooter + 短生命周期 specialist；确定性 advisor、状态机与契约 |
| `sprint-module-squad` | `63693c1` | Sprint 0、模块任务、依赖顺序、集成、daemon 主导合并 |
| `adversaries` | `68d52bb` | coder/reviewer 对抗审查循环；recommendations 与 approval 终止条件 |

重要区别：

1. `two-pack` / `four-pack` / `six-pack` 是同一仓库的分支，不是三个独立仓库。安装器把 `main` 的共享脚本和文章与 pack 分支的配置及角色组合。
2. `project-manager` / `lieutenant` 携带独立安装所需的共享运行时副本，不能假设与 `main` 永远逐字一致。
3. `squad` / `sprint-module-squad` / `adversaries` 是实验工作流，不是 `get-swarm-forge` 支持的产品参数。
4. `simple-windows` 是历史 tag，不是当前产品。
5. 只研究 `main` 会漏掉动态 worker、角色契约、sprint 控制平面和 typed card routing。

建议主线：`main + two-pack` → `six-pack` → `lieutenant` → `squad` → `sprint-module-squad`。`four-pack`、`project-manager` 和 `adversaries` 用于对照。这个顺序是从简单到复杂的阅读顺序，不代表已经核实的线性演化史。

现已用 detached worktree 展开全部 8 个分支，源码入口与研究顺序见研究根目录的 [`README.md`](../../README.md)。各 worktree 已核对 SHA、detached 状态和工作区干净状态；未更新远程引用，也未启动 Agent。detached 并不自动令文件只读。

以下为已完成的建目录步骤，仅供记录，不要直接重复执行：

```bash
# 在 research-swarm-forge/ 下执行；不会启动任何 Agent
# 下面的 worktree add 已执行，重复执行会因目录已存在而失败
# 本地目前已有全部分支，只有需要更新时才执行下一行
# git -C swarm-forge fetch origin
mkdir -p worktrees
for branch in two-pack four-pack six-pack project-manager lieutenant squad sprint-module-squad adversaries; do
  git -C swarm-forge worktree add --detach "../worktrees/$branch" "origin/$branch"
done
```

这些是源码研究 checkout，和运行时自动创建的角色 `.worktrees/` 不是同一种用途。不要直接在研究 checkout 中运行安装器：安装器会向当前目标目录复制/替换文件。

证据：`swarm-forge/README.md:14`、`:18`、`:61`、`:163`；`swarm-forge/get-swarm-forge:87`、`:126`、`:205`。
