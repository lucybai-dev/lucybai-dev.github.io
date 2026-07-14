# 研究停车场：Belief、Hidden State 与 Information

*状态：探索性笔记。本文只记录尚未进入稳定研究地图的方向。当前目标不是继续增加概念，而是用文献与实验淘汰、合并和修正它们。*

## 一、最小框架

当前只保留四个核心概念：

```text
                    Hidden State
              （不可被直接完整观测）
                          │
                          ▼
                    Observations
              （当前实际获得的证据）
                          │
                          ▼
             Belief over Hidden States
          （对 hidden state 的当前内部估计）
                          ▲
                          │
              Information Acquisition
                 （passive or active）
```

其中：

- **Hidden state**：某个已经限定的建模问题中，不可被直接完整观测的状态。
- **Observation**：文字、图像、视频、传感器、他人发言、工具输出、记忆或实验结果等实际证据。
- **Belief**：系统根据已有 observations 对 hidden state 形成的当前估计；可以是显式概率分布，也可以是隐式 learned representation。
- **Information acquisition**：新 observations 进入系统的过程，包括被动观察、查询、检索、实验和干预。

暂不进入核心图的概念：

- **Background knowledge / model**：解释 observation 所依赖的长期知识、规则、prior、likelihood 或生成机制。
- **Constraint**：对 observation 如何改变 belief 的候选解释，不是独立模块。
- **Objective / task**：可能决定什么 state 和 information 是相关的，但暂不进入最小认知循环。
- **Hidden world、projection、information channel**：目前都不是必要的操作性概念。

## 二、Constraint 的工作性解释

Constraint 只有在可能性空间、hypothesis space 或 state space 已经被定义时才有意义。

```text
Background knowledge / structural assumptions
                    ↓
      定义 hypothesis space 与生成模型
                    ↓
             Current belief
                    ↓
             New observation
                    ↓
比较不同 hypotheses 下该 observation 出现的可能性
                    ↓
限制、排除或重新加权 competing hypotheses
                    ↓
              Updated belief
```

目前的工作性判断是：

> **Effective constraint 不是 observation 的内在属性，而是 observation 在给定 current belief、hypothesis space 与 observation-generating model 后，对 competing hypotheses 产生的区分效果。**

三门问题说明了这一点：相同的“主持人打开 C 门”，在不同主持人 policy 下会产生不同 belief update。

这个视角必须与以下成熟概念比较，而不能预设自己是新理论：

- Bayesian conditioning 与 likelihood ratio；
- version space 与 hypothesis elimination；
- information gain、Bayesian surprise 与 value of information；
- constraint propagation 与 belief propagation；
- partial information decomposition；
- causal identification 与 experimental design。

## 三、四个文献专题

Parking lot 不再按大量独立 hypothesis 平铺，而是收缩为四个专题。

### 专题 A：State / Belief Representation

**目的：建立共同语言，不优先追求创新。**

需要吸收的已有问题：

- 什么是 task-relevant state？
- 什么使 latent representation 对预测或决策充分？
- Belief 必须是显式概率分布，还是可以是隐式向量？
- Invariance、sensitivity、sufficiency、calibration 与 uncertainty 如何评价？
- Bisimulation、predictive state representation、world model 与 causal representation 分别保留什么结构？

当前工作性假设：

> 一个 belief-like representation 应当整合历史 observations，对无关表面变化稳定，对任务相关 hidden-state 变化敏感，并支持未来预测、更新和信息获取。

但这基本是已有 representation-learning 问题的重新组织，暂不作为主要突破口。

### 专题 B：Hypothesis Space 与 Hypothesis Pruning

**这是当前最值得深入的专题之一。**

核心问题：

> Observation 的价值，能否被理解为它对 competing hypotheses 的限制、排除或重新加权？

需要重点查：

- version spaces；
- hypothesis elimination；
- Bayesian model selection；
- scientific hypothesis testing；
- abductive reasoning；
- belief revision；
- constraint satisfaction 与 support reduction。

关键未决问题：

1. Hypothesis space 是人工给定、由预训练获得，还是随学习共同形成？
2. 如果 representation space 中没有正确概念，新的 observation 是否还能形成有效 constraint？
3. Support reduction、probability reweighting 与 representational change 是否可以统一描述？
4. 当 observation-generating model 未知时，如何避免错误 pruning？

雪地狼例子属于这里：训练数据可能没有排除“雪地背景”这一 shortcut hypothesis，因此模型没有被迫学到动物结构。

### 专题 C：Information Value 与 Constraint Diversity

**这是当前最有可能形成可检验贡献的专题。**

核心问题：

> 在固定数据或获取预算下，什么样的 observations 最能区分 competing hypotheses？

需要区分：

- redundant information；
- unique information；
- synergistic information；
- conditional information；
- effective sample size；
- intervention diversity；
- causal coverage；
- ordinary data diversity。

工作性假设：

> 在固定样本量、token 数与计算预算下，能够排除更多错误 hypotheses，或提供更多 unique / synergistic evidence 的数据组成，可能比 raw sample count 更能预测 OOD generalization。

这个假设目前不能表述为“约束一定比样本量更重要”。更准确的版本是：

> **Raw sample count 会高估高度重复数据的有效信息量，而 hypothesis-discrimination capacity 可能是更接近泛化能力的变量。**

需要避免把 constraint diversity 当成新名词后再寻找定义。首先要确认它是否已经被 PID、mutual information、effective sample size、coverage 或 identifiability 覆盖。

### 专题 D：Scientific Discovery、Causal Discovery 与 Active Information Acquisition

**这是另一个最值得深入的专题。**

核心问题：

> 如何主动选择 observation，使 competing hypotheses 产生最可区分的结果？

Information acquisition 暂分为：

- **Passive observation**：环境变化、持续传感、他人主动提供信息；
- **Query / retrieval**：提问、搜索、读取记忆、调用工具或数据库；
- **Intervention**：主动改变生成过程再观察结果，例如实验、操控和 Pearl 式 `do()`。

Causal intervention 只是 active information acquisition 的特殊形式。它的价值在于改变生成机制，使被动 observational data 无法区分的 causal hypotheses 产生不同结果。

需要重点查：

- Bayesian experimental design；
- active learning；
- value of information；
- active sensing；
- information-seeking RL；
- active causal discovery；
- optimal experiment design；
- automated scientific discovery。

开放问题：

1. Belief 是隐式神经表示时，如何估计 expected information gain？
2. Observation model 或 causal model 未知时，如何同时学习模型并选择下一次 acquisition？
3. 如何平衡 hypothesis discrimination、任务价值、时间、成本与风险？
4. Query、search 与 intervention 是否需要统一策略，还是应分别建模？

## 四、当前只保留三个优先研究问题

### RQ1

> **什么性质能让一个 learned representation 成为 belief，而不仅仅是 observation embedding 或 history summary？**

这一问题主要用于建立评价标准，不优先声称理论创新。

### RQ2

> **如何衡量一组 observations 对 competing hypotheses 的区分能力，包括 redundancy、unique information 与 synergy？**

这是 constraint diversity 最需要解决的形式化问题。

### RQ3

> **Belief quality 与 hypothesis-discrimination capacity，能否共同预测 OOD generalization 和 information-acquisition efficiency？**

这是最接近可实验研究的问题。

## 五、当前最小实验入口

暂时不设计“大一统 AI 架构”。先建立一个合成、可控的部分可观测环境：

```text
显式 hidden-state / hypothesis space
              ↓
可控 observation-generating model
              ↓
操纵 redundant / unique / synergistic evidence
              ↓
训练不同 belief encoders
              ↓
测量 belief quality、OOD 与 acquisition efficiency
```

优先比较：

- observation-only encoder；
- RNN / Transformer history encoder；
- explicit Bayesian belief；
- predictive latent model；
- invariant / bisimulation-style representation。

优先测量：

- sufficiency；
- invariance；
- sensitivity；
- calibration；
- update consistency；
- contradiction recovery；
- hypothesis elimination / reweighting accuracy；
- information-acquisition efficiency。

第一阶段只回答三个问题：

1. 哪些 representation 真正在追踪 hidden state，而不是 observation pattern？
2. 哪些数据组成真正排除了 shortcut hypotheses？
3. 更好的 belief 是否更会选择下一条有价值的 observation？

## 六、文献阅读顺序

### 第一组：共同语言

- State representation learning
- POMDP belief states
- Predictive state representations
- World models
- Bisimulation 与 state abstraction
- Causal representation learning

### 第二组：当前核心

- Version spaces
- Hypothesis elimination
- Belief revision
- Bayesian model selection
- Abductive reasoning
- Scientific hypothesis testing

### 第三组：信息价值

- Bayesian experimental design
- Active learning
- Value of information
- Partial information decomposition
- Effective sample size
- Data valuation 与 data diversity

### 第四组：主动发现

- Active sensing
- Information-seeking RL
- Causal discovery
- Active causal learning
- Intervention design
- Automated scientific discovery

对每篇论文统一记录：

| 论文 / 方向 | Hypothesis space 是什么？ | Observation 如何生成？ | Belief 如何表示？ | 什么算有效 update？ | 如何衡量 observation value？ | 是否支持主动 acquisition？ | 如何测试 OOD？ |
|---|---|---|---|---|---|---|---|

## 七、暂不升级的想法

以下内容继续保留在停车场，但不作为当前研究主线：

- Hidden world 与 projection；
- 把 pretraining 直接解释成 world prior；
- 把 LLM 参数直接等同于 current belief；
- 默认多模态模型一定拥有更好的 background knowledge；
- 默认 constraint accumulation 是独立于 Bayesian 或 information-theoretic update 的新机制；
- 把所有过拟合归因于 effective constraints 不足；
- 把 hidden-state stability 直接等同于正确 representation；
- 默认显式 belief module 一定优于 end-to-end model；
- 把 causal intervention 等同于所有 active information acquisition；
- 现在就设计一个替代 LLM 的完整新架构。

## 八、升级规则

一个想法只有满足以下大部分条件，才进入稳定研究地图：

1. 已完成相关文献梳理，并识别已有术语和形式化；
2. 能与已有概念区分，而不只是换名；
3. 能导出可证伪预测；
4. 有实验或形式化论证支持；
5. 删除它会降低解释力或预测力；
6. 已明确适用条件、反例和限制；
7. 对 system-building claim，必须优于强 baseline；
8. 对统一性 claim，必须说明统一后新增了什么预测或方法。

在此之前，parking lot 只能用于收集和淘汰想法，不代表任何结论。