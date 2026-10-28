# 工程工具备查

以下保留首轮调查记录，不是本次迁移重新核验外部仓库的结果，也不是运行验证。证据位置沿用调查时记法；当前本地路径映射见[工作区说明](../setup/workspace.md)。

## 4. 工程工具仓库清单（备查，不是团队研究前置）

以下仅在进一步追踪工程质量工具实现时使用。当前研究不要求 clone；先关注哪个角色负责调用、何时调用、失败如何阻塞推进，以及结果如何进入 required evidence。运行时的 `ensure` 可以自动获取部分工具。

| 仓库 | 关联层次 | 何时需要进一步看源码 |
|---|---|---|
| [unclebob/Acceptance-Pipeline-Specification](https://github.com/unclebob/Acceptance-Pipeline-Specification) | 直接工具 + 规格契约 | 提供 Gherkin parser、IR DRY checker、Gherkin mutator；定义 JSON IR、生成器和 persistent runner-worker 协议。只有深入验收规格契约时才需要；研究角色分工先读本仓库提示即可。two-pack 是不使用此链路的明确例外。 |
| [unclebob/crap4clj](https://github.com/unclebob/crap4clj) | 直接工具 + 开发验证依赖 | 复杂度与覆盖率结合的 CRAP 指标；主仓库 `bb crap` 与 `coverage:bb` 直接依赖，含 Babashka coverage adapter。 |
| [unclebob/clj-mutate](https://github.com/unclebob/clj-mutate) | 直接工具 | 语言级 mutation、差分 manifest、覆盖率过滤、worker 与退出状态；和 Gherkin example mutation 是两层不同验证。 |
| [unclebob/dry4clj](https://github.com/unclebob/dry4clj) | 直接工具 | 主仓库 catalog 指定的 DRY 工具身份；需同时检查实际入口，而不能只读介绍。 |
| [unclebob/deintroverter4clj](https://github.com/unclebob/deintroverter4clj) | 间接依赖 | 当前 dry4clj 的 Babashka 配置依赖并委托给此仓库；crap4clj 配置也引用。其当前 README 描述的是测试断言是否触达 SUT 的启发式分析，不能与传统重复检测简单画等号。 |
| [unclebob/speclj-structure-check](https://github.com/unclebob/speclj-structure-check) | 直接工具 | 工程宪法与 catalog 引用；静态检查 Speclj 嵌套及括号结构，解释为什么运行 specs 前还要先做结构检查。 |
| [unclebob/dependency-checker](https://github.com/unclebob/dependency-checker) | 实验分支直接工具 | squad 两分支的 tool-table 引用；检查组件边界、依赖环、抽象度与稳定性。只扫 main 会遗漏。 |

核实的默认分支：APS 为 `main`，上表其余 6 个为 `master`。普通 clone 自动选默认分支即可。

### 验证基础库（当前也不需 clone）

| 仓库 | 建议 |
|---|---|
| [cloverage/cloverage](https://github.com/cloverage/cloverage) | 追踪覆盖率实现时再看。主仓库固定了 Git commit，并设置 `:deps/root "cloverage"`；团队研究只需先理解覆盖率证据的归属和交接。 |
| [slagyr/speclj](https://github.com/slagyr/speclj) | 追踪测试框架实现时再看。工程宪法及工具包装器选择此 BDD/TDD 框架；当前 main 自身测试仍主要使用 `clojure.test`，不能据宪法推断整个仓库已经迁移。 |

### 额外的上游演进观察项

[unclebob/scrap](https://github.com/unclebob/scrap) 是工具演进备查项，不列入当前团队研究计划。当前 clj-mutate README 推荐使用 SCRAP 检查 Speclj 测试结构与质量，`deps.edn` 的 `:scrap` alias 指向它。这属于工具上游演进，不是 SwarmForge 当前 catalog 直接声明的工具；不要无依据地用它替换工程宪法里的 structure-check。

证据：

- `swarm-forge/swarmforge/scripts/swarm_tool.bb:8`：直接工具 catalog。
- `swarm-forge/swarmforge/constitution/articles/engineering.prompt:3`、`:29`：工程工具与 APS。
- `swarm-forge/bb.edn:38`、`:55`：coverage / CRAP 的实际依赖。
- `origin/squad:swarmforge/tool-table.edn` 与 `origin/sprint-module-squad:swarmforge/tool-table.edn`：dependency-checker。
- [dry4clj/bb.edn](https://github.com/unclebob/dry4clj/blob/master/bb.edn)、[crap4clj/bb.edn](https://github.com/unclebob/crap4clj/blob/master/bb.edn)：deintroverter 间接依赖。
- [clj-mutate/deps.edn](https://github.com/unclebob/clj-mutate/blob/master/deps.edn)：SCRAP alias。
