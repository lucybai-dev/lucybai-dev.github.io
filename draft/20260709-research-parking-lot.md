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

1. **Hidden state**：当前建模问题中假定存在、但不能被智能体直接完整观测的状态。选择从 hidden state 开始，意味着已经限定了一个具体的 modeling problem；它不是整个世界，而是当前问题中被建模的隐藏变量。
2. **Observations**：智能体实际获得的证据。图像、文字、视频、传感器、他人发言、工具输出和记忆，都可以成为 observation；它们不必被提升为独立的核心节点。
3. **Belief over hidden states**：智能体根据已有 observations，对 hidden state 形成的当前估计。它可以是显式概率分布，也可以是隐式 learned representation。
4. **Information acquisition**：新 observations 进入系统的过程。它可以是 passive 的，例如环境变化、他人主动发言或持续传感；也可以是 active 的，例如搜索、提问、检查、实验、干预或工具调用。

这张图有意不包含以下内容：

- **Objective / task**：它们可能决定什么 hidden state 和 information 是相关的，但目前尚未进入这个最小认知循环。
- **Prediction、planning、decision 或 task execution**：它们都可以使用 belief，但不是当前框架必须解释的核心节点。
- **Information channel**：它容易同时指模态、介质、接口和获取方式，因此暂不作为基础概念。多模态和多来源 observation 仍然可以在 Observations 节点中讨论。
- **Constraint**：它暂时不是一个独立模块，而是对 **observations 如何更新 belief** 的一种候选解释。
- **Background knowledge / model**：它很可能是正确解释 observation 所必需的条件，但其边界尚未稳定，因此暂时放在核心图外讨论。

“Projection”与“Hidden world”目前都不进入核心架构。前者尚未显示出不可替代的作用；后者可以保留为哲学背景，但不是当前可操作模型所必需的概念。

## 核心图之外的必要条件：Background Knowledge / Model

最近的讨论表明，observation 通常不能被孤立解释。智能体需要某种已有知识，才能判断一个 observation 在不同 hidden state 下出现的可能性。

例如：

- 在狼人杀中，玩家必须预先知道游戏规则、角色能力和常见策略，发言与投票才具有可解释性。
- 在三门问题中，必须知道主持人的开门 policy，观察到“主持人打开 C 门”才可以正确更新 belief。
- 在雪地狼例子中，动物、背景、形态等已有 representation 或 hypothesis space，会影响模型究竟把“狼”与动物结构关联，还是错误地与雪地背景关联。

因此，一个更完整但尚未进入核心图的关系可能是：

```text
Background Knowledge / Model
              +
Current Observations
              ↓
Belief over Hidden States
```

Background knowledge 可能包含：

- structural rules；
- prior knowledge；
- observation-generating model；
- likelihood model；
- dynamics；
- causal assumptions；
- 长期预训练形成的统计结构。

它不一定对应一个独立神经网络模块。在端到端系统中，这些功能可以纠缠在同一个 representation 中。当前框架关心的是系统是否完成这些功能，而不是强制规定其实现必须模块化。

## Constraint 视角的暂定适用范围

只有在一个可能性空间已经被定义时，**constraint** 这个词才真正有意义。

在极限情况下，当尚未引入任何任务相关证据时，该可能性空间中的所有元素都仍然是可接受的。此时，constraint 的作用，就是让一部分可能性变得不再可接受，或直接变得不可能。从这个意义上说，constraint 视角本质上是减法式的、限制性的，而不是加法式的。

但这并不意味着：世界或模型在没有观测约束时，就会真的进行均匀随机输出。这里需要区分几件事：

1. **可能性空间必须先存在。** 如果没有变量、规则、架构、语义或 hypothesis class，就没有任何东西可以被 constraint 限制。
2. **没有 observational constraint，不等于没有结构。** Structural assumptions 可能早已使某些结果不可能，或使某些结果比另一些结果更有可能。
3. **高不确定性不一定等于均匀分布。** 一个 prior 可以很分散，但仍然是非均匀的。
4. **概率更新不一定会删除可能性。** 一个 observation 也可能只是重新加权，而不是把某些可能性压到零。
5. **Observation 的意义依赖生成机制。** 同一个 observation 在不同 observation model 或 policy 下，可能产生完全不同的 belief update。

因此，目前暂时区分三类 constraint：

- **Structural constraints（结构约束）**：定义可接受空间的约束，例如任务规则、物理假设、模型架构、hypothesis class、因果结构或语义定义。
- **Observational constraints（观测约束）**：来自证据，并对这个空间中的可能性进行限制或重新加权。
- **Incremental / effective constraints（增量／有效约束）**：只指某个新 observation 相对于智能体当前 belief 所带来的新增变化。重复证据，或本来就已经高度预期的证据，可能几乎不增加有效约束。

一个更完整的工作性结构可以写成：

```text
结构性假设与 background knowledge 定义可能性空间和生成模型
                         ↓
当前 belief 在可能性空间中分配支持度或权重
                         ↓
观察到新的 observation
                         ↓
比较不同 hidden states 下该 observation 出现的可能性
                         ↓
限制或重新加权兼容的可能性
                         ↓
更新后的 belief
```

在这个解释下，constraint 未必是从 observation 中被单独抽取出来的对象。它更可能是：**一个 observation 在给定 current belief、hypothesis space 与 observation-generating model 后，对 competing hidden states 产生的区分效果。**

这仍然只是工作性解释。后续需要将其与 hypothesis-space restriction、version spaces、Bayesian conditioning、likelihood update、likelihood ratio、information gain、Bayesian surprise、constraint satisfaction、belief propagation 与 causal identification 比较。

## 停车场中的假设

### H1. 预训练可能形成长期 background knowledge，而非当前 belief

语言模型在训练时直接建模的是文本，而不是世界。其参数可能提供当前推断所依赖的长期统计结构、语言知识和背景知识，但不应被直接等同于当前任务中的 belief。

问题：

- 这种长期结构更接近 world prior、human-belief prior，还是 corpus prior？
- 当前 context 中动态变化的 belief，能否与参数中的长期知识区分？
- 多模态、交互式或 embodied pretraining 是否会形成比纯语言模型更接近 world-relevant background knowledge 的 representation？

### H2. 推断过程可能表现为通过累积 effective constraints 进行 belief update

随着新的 observation 到来，系统可能并不只是追加信息。相对于结构性假设、background knowledge、当前 belief 与 observation model，observation 可能会消除、降低权重，或重新组织彼此不兼容的可能性。

问题：

- 当 effective constraints 累积时，learned belief representation 是否会逐渐集中、稳定或变得更充分？
- 这个过程更适合由 Bayesian updating、constraint propagation、support reduction、energy minimization，还是其他形式来描述？
- hidden-state representation 的变化，能否与仍然和证据兼容的 hypotheses 联系起来？
- 什么情况下 observation 真的提供区分能力，什么情况下它只是重复已有信息？

### H3. 一个好的 belief representation 应对 observation 的表面变化稳定，但对底层状态变化敏感

当 observation 的表面形式发生变化，但任务相关 hidden state 保持不变时，一个有用的 belief representation 应该相对稳定；而当底层任务相关状态发生变化时，它应该随之变化。

问题：

- 哪些训练目标会产生这种不对称性？
- 如何同时测量 invariance、sensitivity、sufficiency 与 uncertainty？
- belief-representation quality 是否能够预测 OOD 泛化、规划质量或校准程度？

### H4. OOD 与过拟合可能部分来自训练数据提供的有效约束不足

训练数据可能允许大量 observation-specific shortcuts 与任务相关结构同时解释训练样本。模型即使完全拟合训练数据，也未必被迫选择能够迁移的 representation。

这个假设不是说所有过拟合都来自 constraint 不足；optimization bias、model capacity、objective、noise 与 distribution shift 也可能起作用。

问题：

- 在固定数据量、模型容量与计算预算下，提高独立有效约束是否会减少过拟合并改善 OOD？
- 如何区分“约束不足”与“已有足够证据但 optimization 仍选择 shortcut”？
- 雪地狼一类问题中，哪些变化真正排除了背景 shortcut，而不是简单增加图片数量？

### H5. 数据价值可能更多取决于 unique 与 synergistic constraints，而不是原始数据量

两个 token 数或样本数相同的数据集，可能对 latent representation 提供数量完全不同的有效限制。重复数据可能几乎没有新增作用；不同 observation 既可能提供 unique information，也可能只有组合后才产生 synergistic information。

问题：

- “Constraint diversity”能否被定义为不同于普通 data diversity 的对象？
- 它是否已经被 conditional information、partial information decomposition、mutual information、effective sample size、identifiability 或 Bayesian surprise 所覆盖？
- 在固定数据量下，unique、redundant 与 synergistic evidence 中，哪些最能预测 representation quality 与 OOD performance？

### H6. 多来源、多模态 observations 可能共同形成兼容的 belief

文本、图像、视频、工具、记忆、交互以及其他智能体提供的 observation，可能都在表达同一个 hidden state 的不同证据。系统应当能够整合这些 evidence，而不是仅做表面 feature fusion。

问题：

- 不同来源应该产生相同 latent state、等价 state，还是仅仅产生等价预测与决策？
- 状态等价能否通过 sufficiency、bisimulation、predictive equivalence 或 causal abstraction 来形式化？
- 多来源 information 何时是 redundant、unique、synergistic 或彼此冲突的？
- 多模态 foundation model 能否承担 background knowledge 的功能，并成为当前 LLM 的更一般替代物？

### H7. Information acquisition 比 action 更宽泛

Belief 的变化既可能来自被动 observation，也可能来自主动搜索、提问、实验或干预。Agent action 只是影响未来 observation 的一种机制。

暂定分类：

- **Passive acquisition**：环境变化、持续传感、他人主动发言、系统推送。
- **Query / retrieval**：提问、搜索、读取记忆、调用数据库或工具。
- **Intervention**：主动改变系统，再观察结果，例如实验、操控或 Pearl 式 `do()`。

问题：

- 如何用一个统一框架表示这些 acquisition mechanism？
- Information-seeking action 与普通 task action 的区别是什么？
- 当 acquisition 有成本、风险和时间限制时，应如何决定继续获取信息还是使用当前 belief？

### H8. Observation 的价值是 belief-relative 且 model-relative 的

同一个 observation 可能强烈改变一个智能体的 belief，却几乎不改变另一个，因为它们拥有不同的 prior、background knowledge、observation model、memory 或 current hypotheses。

三门问题提供了一个最小例子：观察到主持人打开 C 门的含义，取决于主持人的开门 policy。正确更新不是只看 observation，而是比较不同 hidden state 下该 observation 出现的 likelihood。

问题：

- 相关量究竟是 Bayesian surprise、information gain、KL divergence、likelihood ratio、support reduction，还是其他对象？
- Effective constraint 应由 support reduction、probability reweighting、decision change，还是 representational change 定义？
- 当 observation-generating model 未知或错误时，belief update 如何保持鲁棒？

### H9. Causal intervention 是 active information acquisition 的特殊形式

因果推断中的 `do()` 不只是获取更多 observation，而是主动改变生成过程，使 competing causal hypotheses 产生不同结果。其价值在于提高不同 causal structures 的可区分性。

问题：

- Observation、query 与 intervention 在 constraint strength 上有何系统差异？
- 能否把 causal experiment design 解释为选择能够最大化 hypothesis discrimination 的 information acquisition？
- 学习系统如何同时更新对 hidden state、observation model 与 causal structure 的 belief？

### H10. 功能可以分析性分解，但实现不必模块化

Transition、observation model、belief update、background knowledge 与 acquisition policy 可以作为功能进行分析，但端到端神经网络未必需要为它们设置独立模块。它们可以在同一 representation 或 computation 中纠缠实现。

问题：

- 什么行为证据足以说明一个 end-to-end system 实现了 belief-like update？
- 功能分解能否导出可操作的 probe、benchmark 或 intervention？
- 在什么条件下，显式模块化优于端到端混合表示？

## 系统构建类假设

### S1. 显式训练 belief-representation quality 可能提升迁移能力

如果 invariance、sensitivity、sufficiency、calibration 与 contradiction recovery 具有因果作用，那么直接优化这些性质可能改善 OOD、规划或长期推断。

### S2. Training objective 决定哪种 latent structure 变得可用

Next-token prediction、reconstruction、contrastive learning、latent-dynamics prediction、belief reconstruction、causal prediction 与 planning objective，可能诱导出不同的 belief-like properties。

### S3. Information-acquisition policy 可以与 task policy 分开优化

搜索、提问、检查、实验与干预，可以根据它们对 competing hypotheses 的预期区分效果选择，而不只是根据即时 reward。

### S4. Belief update 可能需要显式更新机制，而不是仅依赖 context accumulation

显式保存、修订、撤回与重新激活 belief 的机制，可能在矛盾证据、长时序与有限 context 条件下优于普通序列处理；但该优势必须相对强 baseline 实证建立。

### S5. 新 observation 的价值可能由 expected belief change 或 hypothesis discrimination 预测

Agent 也许可以提前估计哪个 query、tool、experiment 或 intervention 最可能带来有用更新，同时考虑任务相关性、获取成本与风险。

### S6. 多模态 background model 与动态 belief engine 可能是不同功能

未来系统可能把长期、多模态、跨任务的 background knowledge，与当前任务中持续变化的 belief maintenance 区分开来。它们可以是不同模块，也可以是端到端系统中的不同功能。

## 候选研究问题

1. **什么性质能让一个 learned latent representation 成为 belief，而不仅仅是 observation embedding 或 history summary？**
2. **在部分可观测条件下，序列 observations 如何重塑 belief representation？**
3. **Background knowledge、observation model 与 current belief 在神经系统中能否被功能性区分？**
4. **哪些 observations 提供 unique、redundant 或 synergistic constraints？**
5. **在固定数据预算下，effective-constraint diversity 是否比 raw sample count 更能预测 OOD？**
6. **Belief quality 能否预测 OOD、calibration 与 information-acquisition efficiency？**
7. **不同模型、架构与模态能否形成 functionally equivalent beliefs？**
8. **当 observation-generating model 未知时，系统应如何进行鲁棒 belief update？**
9. **Query、search 与 causal intervention 应如何在统一 information-acquisition framework 中表示？**
10. **Expected belief change 或 hypothesis discrimination 能否指导更高效的信息获取？**
11. **功能分解能否在端到端模型中导出可靠的 probes、benchmarks 与训练目标？**
12. **多模态 foundation model 能否提供比纯 LLM 更完整的 background knowledge，并与动态 belief engine 协同？**
13. **在什么假设下，constraint view 是真正有用的统一描述，而不仅是 Bayesian、causal 或 information-theoretic update 的重命名？**

## 候选实验方向

### 实验 A：同一 hidden state，不同 observations

构造由同一底层状态生成的多种 observation sequence，比较内部 representation 是否收敛、保持 predictive equivalence 或支持相同决策。

### 实验 B：Sequential constraint accumulation

显式定义 hypothesis/state space 和 observation-generating model，再逐步揭示 evidence。每一步测量 support reduction、probability reweighting、belief-state change、calibration 与 compatible hypotheses。

### 实验 C：Constraint diversity 与 data volume

固定样本量、label distribution、模型容量与 compute，改变 redundancy、viewpoint、causal variation、background correlation、unique information 与 synergy。

### 实验 D：Shortcut 与 structure

构造训练时 shortcut 有效、测试时失效的环境，比较哪些数据变化真正排除 shortcut，哪些只增加重复样本。

### 实验 E：Background knowledge 与 current belief

给模型相同当前 observation，但改变预训练知识、规则说明或 observation policy description，测试其 belief update 是否系统变化。

### 实验 F：三门问题式 observation-model shift

保持 observation 相同，改变主持人 policy 或生成机制，测试模型是否依据 likelihood 正确更新，而不是只匹配表面 observation。

### 实验 G：Passive、query 与 intervention

比较被动接收证据、主动查询和主动干预在 uncertainty reduction、causal identification、task success 与 acquisition cost 上的差异。

### 实验 H：Explicit belief update 与 end-to-end context

比较 full-context Transformer、recurrent latent state、structured memory、explicit Bayesian belief 与 learned state-space update。

### 实验 I：Belief-quality benchmark

同时测量 sufficiency、invariance、sensitivity、calibration、update consistency、contradiction recovery 与 acquisition usefulness。

### 实验 J：多模态 background model + belief engine

比较纯语言 background model、多模态 foundation model，以及是否使用独立动态 belief state 的系统，在新任务、跨模态证据与主动获取信息条件下的表现。

## 待建立的文献地图

第一轮 literature review 应重点梳理：已有领域如何定义 latent state、background model、belief update、constraint 与 information acquisition。

- POMDP belief states
- Bayesian filtering 与 state estimation
- Predictive state representations
- State representation learning
- World models 与 latent dynamics
- Bisimulation 与 state abstraction
- Causal representation learning
- Causal discovery 与 active causal learning
- Intervention design 与 causal experimental design
- Invariant representation 与 multi-view learning
- Multimodal fusion、alignment 与 partial information decomposition
- OOD generalization、shortcut learning 与 spurious correlation
- Data diversity、effective sample size 与 data valuation
- Active learning 与 Bayesian experimental design
- Active sensing、active inference 与 information-seeking RL
- Value of information 与 expected information gain
- LLM hidden state 的 mechanistic interpretability
- Memory architecture 与 continual belief revision
- Version spaces 与 hypothesis elimination
- Constraint satisfaction、factor graphs 与 belief propagation
- Bayesian conditioning、likelihood ratio 与 support reduction
- Foundation models、world models 与 multimodal background knowledge

对每篇 paper 或研究线记录：

| 领域或论文 | Hidden state / hypothesis space 是什么？ | Background knowledge / model 是什么？ | Observation 如何生成？ | 什么算 constraint 或 update？ | Belief 如何表示？ | Acquisition 能否主动控制？ | 如何测 OOD / calibration？ | 是否要求模块化实现？ |
|---|---|---|---|---|---|---|---|---|

## 暂不升级的想法

- **Projection / projector** 作为 world 与 observation 之间的独立层。
- **Hidden world** 被定义成完整高维宇宙或 world-line 集合。
- **Pretraining** 被直接解释为学习真实 world distribution。
- **LLM 参数** 被直接等同于 current belief。
- **多模态模型** 被默认视为一定拥有更好的 background knowledge。
- **Constraint accumulation** 在尚未与 Bayesian、causal、information-theoretic 与 constraint-based methods 比较前，就被当作新机制。
- **没有 observational constraint 就等于均匀随机。** 可能性空间可能已有 structural constraints 与非均匀 prior。
- **每个 informative observation 都必然删除可能性。** 有些 evidence 只重新加权。
- **Constraint 是 observation 的内在属性。** 它依赖 current belief、background model 与 hypothesis space。
- **过拟合全部由 effective constraints 不足导致。** 这最多是候选机制之一。
- **OOD failure** 被直接视为模型缺少正确 belief 的证据。
- **Hidden-state stability** 被默认视为有益。稳定性也可能保存错误 abstraction。
- **Explicit belief-update machinery** 被默认视为一定优于 end-to-end context inference。
- **功能可分析分解** 被误解为网络必须采用模块化架构。
- **Causal intervention** 被等同于所有 active information acquisition。它只是其中一种特殊形式。

## 升级规则

一个想法只有在满足以下大部分条件后，才应该从 parking lot 进入稳定研究地图：

1. 已完成相关文献梳理，并识别竞争性术语与已有形式化。
2. Claim 能与既有概念区分，而不只是换名。
3. 它能够导出可证伪预测。
4. 至少有一个实验结果或形式化论证支持它。
5. 删除该概念后，解释力或预测力会下降。
6. 已明确写出反例、适用条件与限制。
7. 对 system-building claim，所提干预必须在强 baseline 下改善至少一个有意义结果。
8. 对统一性 claim，必须说明它统一了哪些已有对象，以及统一后新增了什么可操作预测。

在满足这些条件之前，parking lot 可以保持不一致、不完整，也可以随时修改。