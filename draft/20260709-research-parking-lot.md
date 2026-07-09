# 研究停车场：Belief、Hidden State 与 Information

*状态：探索性笔记。这些想法目前有意不纳入稳定版研究地图。只有在完成文献梳理与实验之后，才考虑将它们升级、修改、合并或删除。*

## 相对稳定的背景

目前较宽泛的研究兴趣是：智能系统如何在部分可观测条件下运作——它们如何获得 observation、形成对 hidden state 的 belief，并随着新信息持续更新这一 belief。

目前的最小架构暂时表示为：

```text
                    Hidden State
              （不可被直接完整观测）
                          │
                          │ 产生可获得的证据
                          ▼
                    Observations
              （可能来自不同形式与来源）
                          │
                          │ belief update
                          ▼
             Belief over Hidden States
          （对 hidden state 的当前内部估计）
                          ▲
                          │
                          │ 新 observations
                          │
              Information Acquisition
                 （passive or active）
```

这张图只保留四个核心概念：

1. **Hidden state**：当前研究中假定存在、但不能被智能体直接完整观测的状态。它不再依赖额外的 hidden world 概念。
2. **Observations**：智能体实际获得的证据。图像、文字、视频、传感器、他人发言、工具输出和记忆，都可以成为 observation；它们不必被提升为独立的核心节点。
3. **Belief over hidden states**：智能体根据已有 observations，对 hidden state 形成的当前估计。它可以是显式概率分布，也可以是隐式 learned representation。
4. **Information acquisition**：新 observations 进入系统的过程。它可以是 passive 的，例如环境变化、他人主动发言或持续传感；也可以是 active 的，例如搜索、提问、检查、实验、干预或工具调用。

这张图有意不包含以下内容：

- **Objective / task**：它们可能决定什么信息是相关的，但目前尚未进入这个最小认知循环。
- **Prediction、planning、decision 或 task execution**：它们都可以使用 belief，但不是当前框架必须解释的核心节点。
- **Information channel**：它目前容易同时指模态、介质、接口和获取方式，因此暂不作为基础概念。多模态和多来源 observation 仍然可以在 Observations 节点中讨论。
- **Constraint**：它暂时不是一个独立模块，而是对 **observations 如何更新 belief** 的一种候选解释。

“Projection”与“Hidden world”目前都不进入核心架构。前者尚未显示出不可替代的作用；后者可以保留为哲学背景，但不是当前可操作模型所必需的概念。

## Constraint 视角的暂定适用范围

只有在一个可能性空间已经被定义时，**constraint** 这个词才真正有意义。

在极限情况下，当尚未引入任何任务相关证据时，该可能性空间中的所有元素都仍然是可接受的。此时，constraint 的作用，就是让一部分可能性变得不再可接受，或直接变得不可能。从这个意义上说，constraint 视角本质上是减法式的、限制性的，而不是加法式的。

但这并不意味着：世界或模型在没有观测约束时，就会真的进行均匀随机输出。这里需要区分几件事：

1. **可能性空间必须先存在。** 如果没有变量、规则、架构、语义或 hypothesis class，就没有任何东西可以被 constraint 限制。
2. **没有 observational constraint，不等于没有结构。** Structural assumptions 可能早已使某些结果不可能，或使某些结果比另一些结果更有可能。
3. **高不确定性不一定等于均匀分布。** 一个 prior 可以很分散，但仍然是非均匀的。
4. **概率更新不一定会删除可能性。** 一个 observation 也可能只是重新加权，而不是把某些可能性压到零。

因此，目前暂时区分三类 constraint：

- **Structural constraints（结构约束）**：定义可接受空间的约束，例如任务规则、物理假设、模型架构、hypothesis class、因果结构或语义定义。
- **Observational constraints（观测约束）**：来自证据，并对这个空间中的可能性进行限制或重新加权。
- **Incremental / effective constraints（增量／有效约束）**：只指某个新 observation 相对于智能体当前 belief 所带来的新增变化。重复证据，或本来就已经高度预期的证据，可能几乎不增加有效约束。

一个最简结构可以写成：

```text
结构性假设定义一个可能性空间
              ↓
当前 belief 在其中分配支持度或权重
              ↓
新 observation 限制或重新加权兼容的可能性
              ↓
更新后的 belief
```

在这个解释下，constraint 未必是一个从 observation 中被单独“抽取”出来的对象。它也可能更适合被理解为：**一个 observation 作用于当前 belief，并在既定可能性空间中产生的效果。**

这仍然只是工作性解释。后续需要将其与既有概念比较，包括 hypothesis-space restriction、version spaces、Bayesian conditioning、likelihood update、information gain、surprise、constraint satisfaction 与 belief propagation。

## 停车场中的假设

### H1. 预训练可能学习的是某种关于潜在结构的 prior

语言模型在训练时直接建模的是文本，而不是世界。因此，它的内部状态可能更适合被理解为：对人类语言中表达出来的模式，或对人类 belief 分布的一种 prior，而不是对真实世界的直接建模。

问题：

- 在什么条件下，language modeling 会恢复文本背后的稳定结构，而不仅仅是表面 token 统计？
- 这种 learned prior 更接近 world prior、human-belief prior，还是单纯的 corpus prior？
- 这几种解释能否通过实验区分？

### H2. 推断过程可能表现为通过累积 constraint 进行 belief update

随着新的 observation 到来，系统可能并不只是追加信息。相对于一个由结构性假设定义的可能性空间和当前 belief，observation 可能会消除、降低权重，或重新组织彼此不兼容的可能性。

问题：

- 当 effective constraints 累积时，LLM 的 hidden state 是否会逐渐集中或稳定？
- 这个过程更适合由 Bayesian updating、constraint propagation、support reduction、energy minimization，还是其他形式来描述？
- hidden-state 的变化，能否与仍然和证据兼容的 hypotheses 联系起来？
- 什么情况下 observation 真的限制了可能性空间，什么情况下它只是改变预测，却没有减少不确定性？

### H3. 一个好的 hidden state 应该对 observation 的表面变化稳定，但对底层状态变化敏感

当 observation 的表面形式发生变化，但任务相关结构保持不变时，一个有用的 belief representation 应该相对稳定；而当底层任务相关状态发生变化时，它应该随之变化。

问题：

- 哪些训练目标会产生这种不对称性？
- 如何跨模型、跨架构测量这种稳定性？
- hidden-state stability 是否能够预测 OOD 泛化、规划质量或校准程度？

### H4. OOD 失败可能来自模型学到 observation pattern，而不是稳定的 latent structure

有些系统在训练环境中表现良好，是因为 hidden state 编码了 shortcut 或环境特定相关性。当 observation distribution 改变时，这些 representation 会失效，因为它们没有保留任务相关的稳定结构。

问题：

- 能否通过实验区分 shortcut-sensitive hidden state 与 structure-sensitive hidden state？
- 更强的 latent invariance 对 OOD 泛化来说是充分条件，还是仅仅是其中一个因素？
- 哪些失败来自 representation，哪些来自 objective、data coverage、optimization 或 policy learning？

### H5. 数据价值可能更多取决于独立 constraint，而不是原始数据量

两个 token 数或样本数相同的数据集，可能对 latent representation 提供数量完全不同的独立限制。重复数据可能几乎没有新增作用，而多样但相互一致的证据，可能显著减少不确定性。

问题：

- “Constraint diversity”能否被定义为不同于普通 data diversity 的对象？
- 它是否已经被 conditional information、mutual information、effective sample size、identifiability 或 Bayesian surprise 所覆盖？
- 在固定数据量下，constraint diversity 是否比原始数据量更能预测 representation stability 或 OOD performance？

### H6. 多个 information channel 可能收敛到彼此兼容的 belief state

文本、图像、工具、记忆、交互以及其他智能体提供的 observation，可能都在表达同一个任务相关 hidden state 的不同证据。一个更强的系统，也许能够把这些信息整合为彼此兼容的内部 belief。

问题：

- 当不同 channel 的 latent space 本身不对齐时，cross-channel consistency 到底意味着什么？
- 不同 channel 应该产生相同状态、等价状态，还是仅仅产生等价的预测与决策？
- 状态等价能否通过 sufficiency、bisimulation、predictive equivalence 或 causal abstraction 来形式化？
- 在什么条件下，多 channel 提供的是真正独立的 constraint，而不是重复 observation？

### H7. Information acquisition 比 action 更宽泛

Belief 的变化既可能来自被动 observation，也可能来自动作与干预。Agent action 只是影响未来 observation 的一种机制。

问题：

- 如何用一个统一框架表示被动观察、搜索、工具使用、提问、实验和环境反馈？
- 什么时候 observation channel 由 agent 控制，什么时候由环境、规则、其他智能体或资源限制共同决定？
- Information-seeking action 与普通 task action 的区别是什么？

### H8. Observation 的价值是 belief-relative 的

同一个 observation 可能强烈改变一个智能体的 belief，却几乎不改变另一个，因为它们拥有不同的 prior、memory、knowledge 或当前 hypotheses。

问题：

- 相关量究竟是 Bayesian surprise、information gain、KL divergence、likelihood ratio、support reduction，还是其他对象？
- “Constraint”这套语言，是否真的超出了已有 information-theoretic quantities？
- 能否连接定性的 world pruning 与概率式 belief update，同时又不把它们视为完全相同？
- Effective constraint 应该由 support reduction、probability reweighting、decision change，还是 representational change 来定义？

## 系统构建类假设

上面的假设主要是解释性的：它们在问 hidden state、belief update 与 information acquisition 可能在做什么。下面的假设则关注：这些想法能否转化成干预，从而真正改善系统。

### S1. 显式训练 hidden-state stability 可能提升迁移能力

如果稳定的 hidden state 不只是与好性能相关，而是真的具有因果作用，那么，显式鼓励那些共享同一任务相关状态的 observation 映射到等价 representation，应该能够改善 OOD 泛化、校准或规划。

这个假设必须与更简单的解释比较，例如普通 augmentation、更大数据集、更强 regularization 或更大模型容量。

### S2. Training objective 决定哪种 latent structure 会变得可用

不同 objective 可能系统性地产生不同性质的 hidden state。Next-token prediction、reconstruction、contrastive learning、latent-dynamics prediction、belief reconstruction 与 planning objective，可能在保留任务相关结构和不确定性方面表现不同。

一个有价值的结果，不应该只是给 objective 排名，而应解释：每类 objective 在什么条件下诱导出哪些 hidden-state properties。

### S3. Information-acquisition policy 可以与 task policy 分开优化

系统可能受益于区分两类 action：一类用于直接完成任务，一类用于减少不确定性。搜索、工具使用、提问、检查与实验，可以根据它们对 belief 的预期影响来选择，而不只是根据即时 reward。

关键比较是：单独的信息获取目标，是否比一个没有区分用途的统一 policy，在 sample efficiency、decision quality 或 robustness 上更好。

### S4. Belief update 可能需要显式更新机制，而不是仅依赖 context accumulation

把更多 observation 追加到 context 中，并不能保证模型会一致地修正内部状态。显式保存、修订、重新激活或撤回 belief 的架构与训练方法，可能优于仅依靠普通 sequence processing 的系统。

候选比较包括 recurrent belief state、external memory、structured latent variable、factorized hypotheses、state-space update 与 unstructured context window。

### S5. 新 observation 的价值可能可以通过其预期 belief change 来预测

如果 observation value 是 belief-relative 的，那么智能体也许可以提前估计：哪一个 query、tool call、experiment 或 data source 最可能带来有用更新。

这个假设把 belief representation 与实际 acquisition 连接起来：更好的 belief state，应该使系统更容易高效选择下一个 observation。

## 候选研究问题

这些是候选问题，还不是固定研究议程：

1. **什么性质能让一个 learned latent state 成为 belief state，而不仅仅是 observation embedding？**
2. **在部分可观测条件下，序列 observation 如何重塑 hidden state？**
3. **累积的独立 constraints 是否会产生更稳定、更可迁移的 representation？**
4. **哪些 objective 会鼓励 hidden state 编码任务相关 latent structure，而不是 observation-specific shortcut？**
5. **不同模型、架构、模态或 information channel，能否形成等价 belief？**
6. **当 agent 与环境共同控制 observation process 时，information acquisition 应如何建模？**
7. **Hidden-state stability 能否被主动改善，并进一步提升 OOD performance？**
8. **哪些 belief-update mechanism 能够优于被动 context accumulation？**
9. **Expected belief change 能否指导更高效的信息获取？**
10. **在什么假设下，constraint view 是一个真正有用的描述，而不仅仅是对 Bayesian 或 information-theoretic update 的重命名？**

## 候选实验方向

### 实验 A：同一 latent situation，不同 observation

构造由同一底层任务状态生成的多种 observation sequence。比较内部 representation 是否收敛、是否保持 predictive equivalence，或是否支持相同决策。

### 实验 B：Sequential constraint accumulation

先显式定义一个 hypothesis space 或 state space，再逐步揭示 evidence。每一步测量 support reduction、probability reweighting、hidden-state change、prediction、calibration 与仍然兼容的 hypotheses。

### 实验 C：Constraint diversity 与 data volume

固定样本量或 token 数，改变 redundancy、viewpoint diversity、causal variation 与 evidence independence。测试哪些因素最能预测 robustness 与 OOD performance。

### 实验 D：Shortcut 与 structure

构造一个训练时 shortcut 有效、测试时 shortcut 失效的环境。比较强调 next-step prediction、contrastive invariance、world modeling、belief reconstruction 或 planning 的不同模型。

### 实验 E：跨模型与跨架构比较

在同一个 partially observable task 上使用不同模型家族。测试即便 representation 形式不同，它们是否仍会形成 functionally equivalent belief state。

### 实验 F：Passive 与 active information acquisition

比较两类系统：一类接收固定 observation stream，另一类可以 query、search、inspect 或 intervene。测量 uncertainty reduction、task success 与 evidence acquisition efficiency。

### 实验 G：Passive context 与 explicit belief update

给不同系统相同的 sequential evidence，但改变状态维护方式：full-context replay、recurrent latent state、structured memory、explicit hypotheses 或 state-space update。测试一致性、面对矛盾证据时的修正能力、long-horizon retention 与计算成本。

### 实验 H：优化 expected belief change

允许 agent 在多个 query 或 tool 中进行选择。比较 immediate-reward policy、random acquisition、entropy-based acquisition 与 learned belief-change predictor。

## 待建立的文献地图

第一轮 literature review 应该重点梳理：已有领域如何定义一个好的 latent state，以及它们如何形式化“限制可能性”这一过程。

- POMDP belief states
- Bayesian filtering 与 state estimation
- Predictive state representations
- State representation learning
- World models 与 latent dynamics
- Bisimulation 与 state abstraction
- Causal representation learning
- Invariant representation 与 multi-view learning
- OOD generalization 与 shortcut learning
- Active learning 与 Bayesian experimental design
- Active inference 与 information-seeking RL
- LLM hidden state 的 mechanistic interpretability
- Memory architecture 与 continual belief revision
- Information-seeking 与 uncertainty-aware agent policy
- Version spaces 与 hypothesis elimination
- Constraint satisfaction 与 factor graphs
- Bayesian conditioning、likelihood update 与 support reduction

对每篇 paper 或每条研究线，记录：

| 领域或论文 | 可能性空间是什么？ | Latent state 是什么？ | 什么算 constraint 或 update？ | 什么使 state 充分？ | 如何学习或更新？ | 如何表示 uncertainty？ | 如何测试 OOD？ | Action 是否影响信息？ |
|---|---|---|---|---|---|---|---|---|

## 暂不升级的想法

下面这些直觉仍然可能有用，但目前不应该被当作已经成立的概念：

- **Projection / projector** 作为 world 与 observation 之间的独立层。
- **Hidden world** 被定义成一个完整的高维宇宙或 world-line 集合。
- **Pretraining** 被直接解释为学习真实 world distribution。
- **Constraint accumulation** 在尚未与 Bayesian updating、information gain、identifiability、version spaces 及既有 constraint-based method 比较前，就被当作新机制。
- **没有 observational constraint 就等于字面意义上的均匀随机。** 可能性空间本身可能早已包含 structural constraint 与非均匀 prior。
- **每个 informative observation 都必然消除某些可能性。** 有些 evidence 只会重新加权。
- **Constraint 是 observation 的内在属性。** 它的有效贡献可能依赖 current belief 与 structural assumptions。
- **OOD failure** 被直接视为模型缺少正确 belief state 的证据。这只是许多可能解释之一。
- **Scaling law** 被解释为独立 constraints 累积的结果。目前这只是一种高度推测性的解释。
- **Hidden-state stability** 被默认视为有益。稳定性也可能保存错误 abstraction，或阻止必要更新。
- **Explicit belief-update machinery** 被默认视为一定优于 context-based inference。这必须由实验建立，并且可能依赖任务。

## 升级规则

一个想法只有在满足以下大部分条件后，才应该从 parking lot 进入稳定研究地图：

1. 已经完成相关文献梳理，并识别出竞争性术语与已有形式化。
2. 这个 claim 能与既有概念区分，而不只是换了名字。
3. 它能够导出可证伪预测。
4. 至少有一个实验结果或形式化论证支持它。
5. 删除这个概念后，解释力或预测力会下降。
6. 已经明确写出反例与限制。
7. 如果它是 system-building claim，那么提出的干预必须在强 baseline 下改善至少一个有意义的结果。

在满足这些条件之前，parking lot 可以保持不一致、不完整，也可以随时修改。