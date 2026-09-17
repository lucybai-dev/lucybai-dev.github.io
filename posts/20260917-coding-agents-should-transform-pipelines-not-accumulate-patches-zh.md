# Coding Agent 应该改造 Pipeline，而不是堆积 Patch

*把 bounded pipeline transformation 作为 agentic software change 的工作单位*

*初稿写于 2026 年 7 月，2026 年 9 月 17 日为发布做了轻微编辑。*

![传统工具、AI chatbot、AI agent 与 canonical convergence](assets/agent-work-topology-overview.svg)

每一次提交的代码修改，都会把 repository 从一个状态变成另一个状态。

这听起来很显然，但我们描述 coding task 时，仍然常常使用“增加”的语言：

- add 一个 model；
- add 一个 field；
- add 对新 case 的支持；
- add 一个 adapter；
- add 一个 fallback。

当执行本身很昂贵时，这种表达方式是合理的。人类工程师往往只能把一次 migration 拆开，一次改一个 file、一个 caller、一个 pull request。

Coding agent 改变了这个成本结构。

AI chatbot 把 information gathering 变成 many-to-one：documentation、code、history 和 context 可以被压缩成一个 decision。

Coding agent 则把 execution 变成 one-to-many：一个自然语言里的 decision，可以同时更新 codebase 里的 model、caller、test、fixture 和 runtime path。

```text
Traditional tools:  1 → 1 → 1
AI chatbots:        N → 1 → 1
AI agents:          N → 1 → M
```

真正有用的终态应该是：

```text
N information sources
→ 1 semantic decision
→ M coordinated changes
→ 1 coherent repository state
```

所以，真正重要的 engineering question，不只是怎样让 agent 行动得更快。

而是怎样定义一次 change，让 agent 能够在正确的 scope 里把它传播完整，并最终让 repository 留在一个 coherent state 上。

## 变化的单位应该是 bounded pipeline transformation

核心想法很简单：

> 把一次代码修改看成对 affected pipeline 的 transformation，而不是在旧路径旁边再加一条新路径。

这里的 pipeline 并不是指整个 system，也不特指 CI/CD pipeline 或 data pipeline。

它指任何一个边界明确的 responsibility flow：有 input、有 output，并且内部有一套 coherent contract。

它可以很大：

```text
request
→ normalization
→ domain decision
→ persistence
```

也可以很小：

```text
caller
→ calculate(price)
→ result
```

即使只是增加一个 variable，也仍然可能是一场 pipeline transformation。

之前：

```text
caller
→ calculate(price)
→ result
```

之后：

```text
caller
→ calculate(price, currency)
→ result
```

物理上的修改可能非常小，但 affected contract 已经变了。

如果一部分 internal caller 仍然使用 `calculate(price)`，另一部分已经使用 `calculate(price, currency)`，同时还有一个 fallback 在缺少 currency 时悄悄猜一个值，那么这场 transformation 就还没有完成。

重点不是每一次 change 都必须删除大量代码。

重点是 affected path 应该从一个 coherent state 移动到下一个 coherent state。

## “Replace”描述的是 state transition，而不是删除多少代码

这里很容易产生误解。

“Transform rather than add” 并不意味着每个 task 都是一场 refactor，也不意味着每次 change 都必须删除一个 class，或者重写一套 architecture。

一个新 feature 可以 transform 一条已有 pipeline。

一个 bug fix 可以用正确的 decision rule 替换错误的 decision rule。

一个新 parameter 可以用新的 function contract 替换旧 contract。

一次 dependency upgrade 可以让一条 implementation path 替换另一条。

一次 architecture migration 可以把 authority 从一个 component 转移到另一个 component。

这些情况的共同点都是：repository 从 state A 移动到了 state B。

```text
Current canonical sub-pipeline
→ bounded transformation
→ next canonical sub-pipeline
```

这次 change 完全可能增加代码。

问题只是：结果不应该留下一个没有被分类的 parallel reality。

## Transformation contract 是控制点

Intent 和 execution 之间真正的 control point，不应该是一份逐文件展开的详细计划。

那反而浪费了 agent 自己发现 reference、完成 mechanical work 的能力。

Transformation contract 应该定义的是 semantic change：

```text
Pipeline 的哪一部分正在变化？
它当前的 contract 是什么？
目标 contract 是什么？
完成后必须保持哪些 invariant？
Transformation 到哪里停止？
Verified boundary 之外还剩下哪些 uncertainty？
```

Contract 不需要提前列出每一个 file 或 affected symbol。Agent 可以从 repository 里自行发现具体 implementation surface；真正应该保持稳定的是 target state、propagation boundary 和 completion criteria。

例如，下面这个要求太弱：

> 给 price calculation 增加 currency support。

一个 transformation-oriented decision 应该更接近：

> 在 pricing sub-pipeline 内，把 currency 变成 price calculation 的显式必需 input。迁移 scope 内所有 internal producer 和 consumer。不要保留一个会自行推断 currency 的 internal fallback。到 public API boundary 为止。

第二种写法没有规定要改哪些文件。

它定义了 old state、new state、propagation surface 和 stopping boundary。

这些信息已经足够让 agent fan out 成一组 coordinated actions。

## 为什么 capability specification 本身还不够

普通的 feature specification 通常描述的是 capability：

> System 应该能够做什么？

例如：

- support 多种 currency；
- accept 一个新 field；
- expose 一个新 endpoint；
- preserve 现有 behavior；
- pass test suite。

Agent 完全可能满足这些 requirement，同时让多套 internal contract 继续并存。

于是这里出现了一个重要区别：

```text
Capability specification
= 什么必须变得可能

Transformation contract
= affected path 必须怎样改变
  以及哪些 internal state 可以继续存在
```

这并不是反对 capability specification。System 最终应该具备什么能力，仍然应该由它来定义。

问题在于，capability intent 本身天然容易变成 additive。它告诉我们应该加入什么，却不一定说明哪一套 existing contract 正在被替换、哪些 caller 必须迁移、哪里允许 compatibility、以及哪些东西完成后不应该继续处于 active state。

所以，对于由 agent 执行的工作，在理解当前 repository state 之后，capability intent 还应该配上一份针对 affected path 的 transformation contract：

```text
current sub-pipeline
→ target sub-pipeline
```

两者合起来，应该同时定义：

```text
what becomes possible
+
what becomes canonical
```

## 为什么它应该位于 loop engineering 之前

Loop engineering 关注的是 agent 怎样继续工作：

```text
observe
→ plan
→ act
→ test
→ inspect
→ retry
```

这是 execution mechanism。

但 loop 本身并不会决定哪一个 repository state 应该最终留下来。

如果 target 只有：

```text
the feature exists
and the tests pass
```

那么一个能力很强的 loop 完全可能不断加入 adapter、optional parameter、converter、fallback 和 compatibility branch，直到 test 变绿。

Loop 在 execution 层面成功了，但 pipeline 却可能积累出越来越多相互竞争的 path。

更合理的顺序应该是：

```text
bounded transformation
→ target state and invariants
→ agent loop
→ coordinated execution
→ verified resulting state
```

Loop engineering 回答的是：

> Agent 怎样持续取得进展？

Transformation contract 回答的是：

> 这个 loop 到底在执行什么 change？什么样的 resulting state 才算完成？

Loop 位于 transformation contract 的下游。

## Subagent 划分的是 execution，不是 semantic authority

这个模型并不排斥 subagent。

多个 agent 完全可以在同一场 transformation 内协作：

```text
Shared transformation contract
├── dependency discovery
├── implementation
├── test migration
└── residual-path verification
```

但它们不应该各自独立决定哪一个 contract、writer 或 path 才是 canonical。

否则，一个 agent 添加新 contract，另一个为了 test 保留旧 contract，第三个加 converter，第四个再加 fallback。

每个 subtask 单独看都可能成功，但最终的 pipeline 却变得更不 coherent。

> Execution 可以被分布出去，但 state transition 必须保持统一。

## Git 已经提供了 isolation boundary

Git branch 和 pull request 天然就很适合这个模型：

```text
Branch
= isolated candidate repository state

Pull request
= proposed bounded transformation

Review
= verify target path、migration surface 与 remaining uncertainty

Merge
= 接受新的 repository state 成为 canonical
```

一个 pull request 不需要完成整个 system pipeline 的 transformation。

它完全可以只 transform 一个 sub-pipeline。

但在它声明的 boundary 内，应该留下一个 coherent resulting path。

这也改变了我们理解 PR atomicity 的方式。

一个 PR 并不是因为改的 file 少就一定 atomic。

它在完成一个 bounded semantic transformation 时才是 atomic。

```text
Small semantic surface
Complete affected surface
```

一个二十个文件的 PR 可以是 semantically atomic。

一个只改两个文件的 PR，也可能制造两套 competing truth。

## Compatibility 应该是一种被分类的 boundary，而不是意外出现的第二套 architecture

有些 change 确实需要暂时或永久保留 alternate path：

- database migration；
- public API compatibility；
- external plugin；
- rolling deployment；
- experiment；
- operational rollback。

目标不是禁止这些 path。

目标是对它们进行分类。

任何继续存在的 alternate path，都应该有一个明确角色：

```text
canonical
compatibility
migration
experiment
rollback
projection
```

同时还应该明确：哪一条 path 拥有 authority，以及 compatibility boundary 到哪里结束。

“存在两条 path”本身不一定是失败。

“存在两条 path，而且两条看起来都同样 canonical”才是失败。

一个有用的规则是：

> Explore broadly. Commit one coherent state.

## 一个可以试的 prompt

对于任何 target state 已经足够清楚的 code change，可以尝试在 coding-agent instruction 中加入：

```text
Treat this task as a bounded transformation of the affected pipeline, not an addition beside it.

Within the declared scope, propagate the change through every affected producer and consumer and leave one coherent internal path. Any remaining alternate path must be explicitly classified as compatibility, migration, experiment, rollback, or projection.

Do not claim completion beyond the verified boundary.
```

更短的版本是：

```text
Transform the affected path; do not create a parallel one. Migrate all affected producers and consumers, and leave one coherent internal path within the declared scope.
```

无论这个 task 是替换一套 architecture、修一个 algorithm、增加一个 feature，还是只引入一个 variable，这条原则都适用。

## 一个 research hypothesis

这仍然只是一个 hypothesis。

这里的主张不是“diff 越大越好”，也不是“所有 migration 都应该一次 PR 完成”。

真正的 claim 是：

> Coding agent 让我们第一次有可能把 software work 围绕 bounded state transition 来组织，而不是围绕 additive local patch 来组织。

一个有用的实验可以比较：

1. 标准的 feature-oriented prompt；
2. task 或 subagent decomposition；
3. bounded pipeline-transformation prompt。

可以测量的指标包括：

- functional correctness；
- active internal path 的数量；
- undocumented fallback；
- duplicate authority；
- legacy-contract residue；
- review time；
- follow-up repair cost；
- token 和 execution cost。

最重要的问题，不是 agent 到底改了多少文件。

而是 repository 最终是否进入了一个更 coherent 的 state。

Coding agent 给了我们一种新的 execution shape：

```text
many inputs
→ one semantic decision
→ many coordinated changes
```

Engineering discipline 应该把这个 shape 补完整：

```text
many inputs
→ one bounded transformation
→ many coordinated changes
→ one coherent repository state
```

这也许比 patch、file 或 task 更适合作为 agentic programming 的工作单位。

---

*我会继续在 [RepoDelta](https://github.com/repodelta/repodelta) 里探索这篇文章背后的更大问题：当 coding agent 开始成为 repository 的 primary operator，repository 和 engineering workflow 应该怎样变化。*
