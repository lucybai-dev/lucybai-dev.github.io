# 研究停车场：Belief Update 与 Information Acquisition

*用途：这里只保留当前仍可能导出文献问题、可证伪假设、实验或系统设计的少量想法。Git 历史负责保存被删除的旧内容，正文不承担档案功能。*

当前限制：

- 最多三个优先研究问题；
- 最多三个活跃假设；
- 最多两个近期实验入口；
- 新想法必须合并或替换旧想法，不能只追加。

## 一、长期目标

长期目标是构建比当前 LLM 更智能的系统：它能够结合人类与机器的优势，持续维护和修正 belief，主动获取信息，并发现仅靠人脑难以完成的模式与推理。

这只是 vision，不是当前论文 claim。当前阶段不设计完整的替代 LLM 架构。

## 二、最小工作模型

```text
Background Model B + Task Specification T
                    ↓
定义 hypothesis space H 与 observation model
                    ↓
          Current Observations O
                    ↓
          Belief over H / Hidden State
                    ↑
       Information Acquisition
  （observation / query / intervention）
```

几个功能必须区分：

- **Background model**：长期知识、prior、规则、生成机制与 causal assumptions；
- **Task specification**：定义目标、相关变量与输出要求；
- **Observation**：当前获得的证据；
- **Belief**：对 competing hypotheses 或 hidden states 的当前支持度；
- **Information acquisition**：选择下一条观察、查询、工具调用、实验或干预；
- **Policy / capability constraints**：限制系统可以采取的动作。

这些功能不要求对应独立神经模块，但不同类型的信息不应拥有相同的更新权限。

Prompt injection 可以被看成一个候选应用：本应只作为 observation 的外部内容，越权修改了 task specification、policy 或 action control。

## 三、当前核心命题

> **Observation 的有效价值不是其内在属性，而是它在给定 current belief、hypothesis space 与 observation-generating model 后，对 competing hypotheses 产生的区分效果。**

这种效果可能表现为：

- 排除某些 hypotheses；
- 重新分配概率或支持度；
- 暴露已有 representation 中缺少必要概念；
- 改变下一步最有价值的信息获取方式。

该命题目前只是统一视角。它必须与 Bayesian conditioning、likelihood ratio、version space、belief revision、information gain、partial information decomposition 和 causal experimental design 比较，不能预设为新机制。

## 四、三个优先研究问题

### RQ1：什么才算 belief-like representation？

> 什么行为和表示性质，能把 belief 与 observation embedding、history summary 或普通 latent state 区分开？

重点性质：sufficiency、calibration、update consistency、对反证的修正能力，以及对任务相关状态变化的敏感性。

### RQ2：如何衡量 evidence 的 hypothesis-discrimination capacity？

> 如何衡量序列或多模态 observations 对 competing hypotheses 的区分能力，并区分 redundant、unique 与 synergistic evidence？

这里要检验“constraint diversity”是否真的提供了新变量，还是已经被现有信息论、identifiability、coverage 或 effective sample size 覆盖。

### RQ3：如何围绕 belief 主动获取信息？

> 当 belief 是隐式表示、observation model 不完全已知且获取有成本时，系统如何选择下一次 observation、query 或 intervention，以最有效地区分剩余 hypotheses？

## 五、三个活跃假设

### H1：Belief quality 可以预测系统在新证据下的行为

由 sufficiency、calibration、update consistency 和 contradiction recovery 构成的 belief-quality profile，可能比普通 representation similarity 更能预测 OOD、长期推理和信息获取表现。

### H2：数据价值取决于区分能力，而不只是数量

在固定样本量、token、模型容量和计算预算下，能够排除或降权更多错误 hypotheses 的数据组成，可能比 raw sample count 更能预测 OOD generalization。

这不等于“多样性永远比数据量重要”，也不意味着所有过拟合都来自 constraint 不足。

### H3：基于 hypothesis discrimination 的 acquisition 更有效

根据 expected hypothesis discrimination 选择 query、experiment 或 intervention，可能比随机获取、只看 uncertainty 或只追求即时任务 reward 更高效。

## 六、近期实验入口

### 实验 A：可控的 belief-update benchmark

构造一个具有显式 hidden-state / hypothesis space 和 observation-generating model 的部分可观测环境。

系统操纵：

- redundant、unique 与 synergistic evidence；
- shortcut correlation；
- observation-model shift；
- passive observation、query 与 intervention。

比较：

- explicit Bayesian belief；
- recurrent latent state；
- Transformer history encoder；
- predictive latent model。

测量：belief accuracy、calibration、update consistency、contradiction recovery、OOD 与 acquisition efficiency。

### 实验 B：Typed update 与 prompt injection（候选应用，不是当前主线）

把 system policy、task instruction、retrieved content、tool result 与 model belief 标记为不同信息类型，并限制它们可以更新的状态。

检验这种设计能否在保留正常 belief revision 的同时，减少不可信 observation 越权修改 task、policy 或 action 的情况。

## 七、最小文献地图

只保留五组：

1. POMDP、Bayesian filtering 与 belief representation；
2. Version spaces、Bayesian model selection 与 belief revision；
3. Information gain、PID、data valuation 与 identifiability；
4. Active learning、Bayesian experimental design 与 active causal discovery；
5. Instruction–data separation、information-flow control 与 prompt injection。

读每篇论文只回答四件事：

1. 它维护的 state / hypothesis 是什么？
2. Evidence 如何更新它？
3. Observation value 如何衡量？
4. 它导出了什么可验证结果或系统设计？

## 八、删除规则

以下内容不再保存在正文中：

- 只有解释作用、不能产生预测或实验的概念；
- 已被成熟术语完整覆盖、但没有新增区别的重命名；
- 大量相似例子和应用领域枚举；
- 尚无证据支持的完整 AI 架构；
- Hidden world、projection、information channel 等非必要节点；
- “constraint 是新理论”“多模态必然更懂世界”“显式模块必然优于端到端”等强结论。

一个新条目只有同时满足以下至少两项才进入正文：

- 对应清楚的文献检索问题；
- 导出可证伪预测；
- 能形成一个具体实验或 benchmark；
- 会改变系统设计或评价方法；
- 与现有概念存在明确而必要的区别。
