# 可选外部仓库

以下保留首轮调查记录，不是本次迁移重新核验外部仓库的结果，也不是运行验证。证据位置沿用调查时记法；当前本地路径映射见[工作区说明](../setup/workspace.md)。

## 6. 按研究目标再追加的仓库

### 语言特定工程工具

若要研究 Go 项目的质量门禁，补齐：

- <https://github.com/unclebob/crap4go>
- <https://github.com/unclebob/dry4go>
- <https://github.com/unclebob/mutate4go>

若要研究 Java 项目的质量门禁，补齐：

- <https://github.com/unclebob/crap4java>
- <https://github.com/unclebob/dry4java>
- <https://github.com/unclebob/mutate4java>

以上 6 个仓库均经 GitHub API 核实存在，但本轮没有全面审计其实现。它们是语言适配分支，不是理解 Clojure 编排核心的前置条件。

### 运行平台和库内部

| 仓库 | 何时值得 clone |
|---|---|
| [babashka/babashka](https://github.com/babashka/babashka) | 调查 `.bb` 执行、内建库、脚本加载与 JVM Clojure 的差异；通常安装 bb 并读文档已足够。 |
| [babashka/sci](https://github.com/babashka/sci) | 进一步追踪 Babashka 解释器、命名空间与求值限制时。 |
| [babashka/fs](https://github.com/babashka/fs)、[babashka/process](https://github.com/babashka/process) | 文件/子进程行为出现跨平台问题时；squad 的 deps.edn 也显式声明。 |
| [tmux/tmux](https://github.com/tmux/tmux) | 研究 send-keys、capture-pane、session/socket 和终端兼容性内部时。 |
| [http-kit/http-kit](https://github.com/http-kit/http-kit)、[dakrone/cheshire](https://github.com/dakrone/cheshire) | dashboard HTTP server 与 JSON 边界问题；不是独立 dashboard 仓库。 |
| [microsoft/playwright](https://github.com/microsoft/playwright) | 分析浏览器测试引擎本身时。运行主仓库 dashboard 测试只需 npm 安装依赖与 Chromium，不必 clone 它。 |
| [clojure/clojure](https://github.com/clojure/clojure)、[git/git](https://github.com/git/git)、[zsh-users/zsh](https://github.com/zsh-users/zsh) | 只有追到语言/版本控制/shell 实现问题再深入；不是首轮资料准备的必要仓库。 |
