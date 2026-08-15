# 什么叫“更多信息”？

*从 possible worlds 看信息：为什么多模态不只是更多输入*

> 我们总说某个模型“看到了更多信息”，或者增加一个 modality 就能让系统知道得更多。但如果真实世界只能通过不同的 observation channels 被间接看见，“更多”到底多在哪里？

## Prologue｜我们总是隔着一个 channel 观察世界

上一篇文章最后留下了一个很实际的问题：当现有 evidence 已经不足以继续区分几种仍然可能的情况，一个更主动的智能体必须决定下一步去看什么、问什么，或者做什么。

但在讨论“下一步看什么”之前，还有一个更基础的问题。

我们从来不是直接把真实世界本身拿到手里。

视觉给我们图像，听觉给我们声音，温度计给我们一个读数，搜索引擎给我们一组经过检索和排序的结果。它们都只是世界经过某个 **observation channel** 以后留下的 observation。

可以把这个关系先写得很简单：

```text
hidden state H  →  observation O
```

如果有多个 channels：

```text
                 → O_vision
hidden state H  → O_audio
                 → O_text
```

这篇文章想追问的，就是这些箭头到底意味着什么。

先继续用一个很小的盒子游戏。

桌上有左、右两个盒子。奖品随机放在其中一个盒子里，同时奖品本身随机是红色或蓝色。

于是隐藏的真实状态可以写成：

```text
H = (位置, 颜色)
```

一共有四种等概率的 possible worlds：

| 可能世界 | 位置 | 颜色 |
|---|---|---|
| H₁ | 左 | 红 |
| H₂ | 左 | 蓝 |
| H₃ | 右 | 红 |
| H₄ | 右 | 蓝 |

现在有三个 observation channels：

- **A**：100% 准确地告诉颜色；
- **B**：75% 准确地告诉位置；
- **C**：100% 准确地告诉位置。

先不要问谁“最好”。

先问一个更具体的问题：

> **经过这些不同的 channels，我们分别能够区分什么？**

## Chapter 1｜不同的 channel，让我们区分了什么？

假设 A 告诉我们：“奖品是红色的。”

原来的四个世界：

```text
{H₁, H₂, H₃, H₄}
```

会变成：

```text
{H₁, H₃}
```

蓝色的两个世界被排除了。

如果 C 告诉我们：“奖品在左边。”

四个世界则变成：

```text
{H₁, H₂}
```

右边的两个世界被排除了。

A 和 C 都把四种可能缩小成两种。

但它们留下来的并不是同样的两个世界。

A 让“红 / 蓝”变得可以区分；C 让“左 / 右”变得可以区分。

这已经暴露出一个很容易被“信息量”三个字遮住的东西：

> **信息不只是让可能性变少，还让一些原本混在一起的 possible worlds 变得可以区分。**

在这个盒子例子里，我们甚至可以把它画成一个 2 × 2 的格子：

| | 红 | 蓝 |
|---|---|---|
| 左 | H₁ | H₂ |
| 右 | H₃ | H₄ |

A 像是按颜色把格子切开；C 像是按位置把格子切开。

所以两条 information 即使排除了同样数量的 worlds，也完全可能提供不同的 **distinctions**。

B 又更有意思一点。

如果 B 说“左边”，右边的两个世界并不会真的消失。因为 B 有 25% 的概率说错。

变化只是：左边的 worlds 变得更可能，右边的 worlds 变得没那么可能。

所以现实中的 observation 不一定总是一个“硬切分”。有些 observation 会直接排除 worlds；有些 noisy observation 只是重新分配不同 worlds 的权重。

更一般地说，一条 observation 改变的是：

> **我们怎样区分、排除和权衡仍然可能的 hidden states。**

在这个简单例子里，“颜色”和“位置”刚好像 hidden state 的两个方向。但真实世界未必总能拆成这么整齐的坐标轴。

因此后面我会更多使用 `distinction` 这个词：不是假设世界天然分成几个字段，而是问一个 channel 到底让哪些原本难以区分的状态变得可区分。

这也是为什么，一条 information 首先不必被想成一个数字。

它先改变了 possible worlds 的结构。

## Chapter 2｜同样是 1 bit，为什么知道的不是同一件事？

当然，我们还是会想把“知道得更多”变成一个可以比较的数字。

最自然的问题是：

> **看完一条 observation 以后，我少了多少不确定性？**

这正是 Shannon information 很擅长回答的问题。

在这个盒子游戏里，位置是一个完全不确定的二选一：左 / 右。

颜色也是一个完全不确定的二选一：红 / 蓝。

一个完全拿不准的二选一，可以看作 1 bit 的 uncertainty，所以四个等概率世界一开始共有 2 bits。

A 完全解决颜色，因此减少：

```text
1 bit
```

C 完全解决位置，也减少：

```text
1 bit
```

于是：

```text
IG(A) = IG(C) = 1 bit
```

但我们刚刚已经看到，A 和 C 根本没有告诉我们同一件事。

一个让颜色可区分，一个让位置可区分。

所以这里出现了一个很重要的区别：

> **一个数字可以告诉我们少了多少 uncertainty，却不能告诉我们究竟获得了哪些 distinctions。**

或者更简单地说：

> **“知道多少”和“知道了什么”，不是同一个问题。**

再看 B。

如果 B 说“左边”，我们对位置的 belief 会从：

```text
50% / 50%
```

变成：

```text
75% / 25%
```

我们更确定了一些，但没有完全确定。

信息论用 **entropy** 衡量这种概率分布还剩多少 uncertainty。`50% / 50%` 的 entropy 是 1 bit，而 `75% / 25%` 约是 0.81 bits。

所以 B 平均减少的位置 uncertainty 约为：

```text
1 - 0.81 = 0.19 bits
```

而 B 不告诉颜色，所以它对完整 hidden state 的 information gain 仍然约是 0.19 bits。

于是：

```text
A：约 1 bit
B：约 0.19 bits
```

如果问题只是“谁平均减少了更多 uncertainty”，A 的确更多。

但这里还有一个容易忽略的地方：**这 1 bit 也不是 A 身上固定装着的某种信息含量。**

它取决于我们原来不知道什么。

如果事前已经有 99% 的把握奖品是红色，那么 A 即使仍然 100% 准确地告诉颜色，也不会再带来 1 bit 的新 information。

所以 information gain 更准确地是在问：

> **相对于我原来的 belief，这次 observation 平均让我少不知道了多少？**

具体为什么 `75% / 25%` 的 entropy 是约 0.81，以及公式里的 `log` 从哪里来，放在文末附注 1。主线到这里先只保留一件事：

> **information quantity 很重要，但 quantity 会把“到底区分了什么”压缩掉。**

而这件事一旦放到多个 channels 上，就变得更明显。

## Chapter 3｜多个 channels 放在一起，究竟多出了什么？

假设现在不是只拿一条 observation，而是同时拥有多个 channels。

直觉上很容易说：

> 图像 + 声音当然比只有图像“信息更多”。

但如果 Chapter 1 和 Chapter 2 的讨论成立，这句话其实说得太快了。

因为真正的问题不是有几个输入接口，而是：

> **新的 channel 到底带来了哪些原来没有的 distinctions？**

### 3.1 两份输入，可能还是同一份信息

先看最极端的情况。

一个摄像头拍到一帧画面，系统把同一帧复制两次，送进两个不同接口。

形式上，它现在收到了两个 inputs。

但第二份并没有让任何新的 hidden states 变得可区分。

```text
O₂ = O₁
```

所以：

> **输入数量增加，不等于新的 information 增加。**

现实里的两个 sensors 很少完全相同。即使看同一个目标，只要视角、噪声或误差来源不同，第二个 sensor 就可能继续提供新 evidence。

这里的重点只是：不能按 input 数量直接数 information。

### 3.2 不同 channel，可能补上不同的区别

回到盒子。

假设视觉只能看到奖品的局部特写，因此很容易判断红 / 蓝，却看不到它究竟在左盒还是右盒。

声音 channel 则不看颜色。我们分别晃动两个盒子，通过里面传出的声音差异，带着一定误差判断奖品在哪一边。

视觉主要帮助区分颜色。

声音主要帮助区分位置。

它们不是简单地把同一条 evidence 重复两遍，而是在同一个 hidden state 上提供不同的 distinctions。

在盒子这个刻意简化的例子里，可以很直观地说：两个 modalities 像是在从不同“方向”约束：

```text
H = (位置, 颜色)
```

但这里的“方向”只是一个方便理解的比喻。

真实 hidden state 未必真的有一组干净、可命名的坐标轴。更一般地说，不同 modalities 可能以不同方式切分 hypothesis space，让不同的 possible worlds 变得可区分。

所以多模态真正有意思的地方，开始从“输入格式更多”变成了：

> **一个新的 modality，有没有提供其他 channels 原本没有的 distinctions？**

### 3.3 有些信息，只存在于 channels 之间

还有一种情况更反直觉。

想象盒子的机关会同时给出一个视觉信号和一个声音信号。

视觉信号只有两种：红、蓝。

声音信号也只有两种：高音、低音。

机关的规则是：

| 视觉 | 声音 | 奖品位置 |
|---|---|---|
| 红 | 高 | 左 |
| 蓝 | 低 | 左 |
| 红 | 低 | 右 |
| 蓝 | 高 | 右 |

并且对每个位置来说，两种允许的组合都同样可能。

现在只看视觉。

红色既可能对应左，也可能对应右；蓝色也一样。

所以视觉单独对位置没有帮助。

只听声音也是如此：高音可能来自左，也可能来自右；低音也一样。

但如果同时知道颜色和音调，位置立刻确定。

这里最有意思的不是“两个没用的信息加起来突然变有用”。

而是：

> **关于位置的这条 distinction，并不属于视觉单独，也不属于声音单独；它存在于两个 modalities 的关系里。**

这就是 **synergy** 最干净的直觉之一。

如果一定要把这一章压成一句话，我会说：

> **Multimodal information 不只是 information accumulation，也可能是 constraint composition。**

也就是：多模态的价值，有时不在于每个 channel 单独又知道了一点什么，而在于它们组合以后，对同一个 hidden state 形成了什么新的约束。

### 写到这里，我回头查了一下现在的研究

写到这里时，我原本只是想用这个盒子游戏说明一个直觉：不同 modalities 不只是分别带来更多 information，它们之间的关系本身，也可能暴露任何单一 channel 都看不到的结构。

但查资料时我发现，这个问题并没有只停留在这样的直觉里。

更早的 Partial Information Decomposition（PID）工作已经尝试把多个 sources 关于同一个 target 的 information 拆成 **redundant、unique 和 synergistic** 的部分。Williams 和 Beer 在 2010 年提出的框架，就是这条研究线的重要起点之一。

而到了 multimodal learning，本身也出现了非常接近的问题。

例如 Liang 等人在 NeurIPS 2023 的 **Factorized Contrastive Learning** 中明确指出，只学习 modalities 之间共享的 redundant information 可能不够，因为与下游任务相关的信息也可能只存在于某一个 modality 的 unique 部分。他们试图同时捕捉 shared 和 unique task-relevant information。

更近一些，Yang、Wang 和 Hu 在 ICML 2025 的工作 **Efficient Quantification of Multimodal Interaction at Sample Level** 中，直接把 modalities 之间的 interaction 写成 redundancy、uniqueness 和 synergy，并尝试在 sample level 对这些成分进行量化。

我不想把这些论文写成“它们证明了盒子游戏是对的”。它们研究的问题、定义和假设都比这个小例子严格得多。

但它们至少说明了一件很有意思的事：

> **我们刚刚从 possible worlds 一步步推到的问题——多个 modalities 到底是在重复、各自补充，还是只有联合起来才产生新的 information——确实已经是一个真实的研究问题。**

这也让我更愿意把多模态的价值理解成 information structure，而不只是 input count。

## Chapter 4｜一个 channel 能不能替代另一个？

到这里，我们讨论的都是多个 sources 放在一起以后会发生什么。

但还有另一种比较方式。

有些 channels 并不是互补关系，而是真的可以说：有了其中一个，另一个就变得可以被模拟。

回到 B 和 C：

- B：75% 准确地告诉位置；
- C：100% 准确地告诉位置。

如果拥有 C，要制造一个和 B 一样的 channel 很容易。

C 每次都给出正确位置，我们只需要在它输出以后，独立地以 25% 的概率把“左 / 右”随机翻转一次。

于是一个完美的位置 observation 就被人为降质成了 75% 准确的 observation。

```text
C  →  加入随机噪声  →  B
```

反过来却做不到。

一旦只有 B，已经丢掉的那 25% information 不能凭空恢复。

因此这个例子里的关键不是“C 的 accuracy 比 B 高”，而是：

> **C 可以只靠加工自己的 observation 来模拟 B；B 却不能反过来模拟 C。**

这就是 **Blackwell comparison** 背后的核心直觉。

更严格地说，那个后处理必须不再偷偷访问真实 state。否则我们不是在加工已有 information，而是在过程中重新获得了 evidence。

为什么这个区别值得单独讲？

因为它和上一章的问题不一样。

Chapter 3 问的是：

> 多个 sources 放在一起，到底新增了什么？

Blackwell 在这里更像是在问：

> **一个 source 能不能替代另一个？**

再看 A 和 C，就会发现这种替代关系根本不存在。

A 完美告诉颜色，却没有位置；C 完美告诉位置，却没有颜色。

A 无法只加工“红 / 蓝”制造出“左 / 右”；C 也无法只加工“左 / 右”恢复“红 / 蓝”。

所以两个都很准确的 channels，也可能根本没有必要硬排成一条统一的强弱顺序。

它们提供的是不同 distinctions。

更严格的 Blackwell 条件放在附注 2。

## Chapter 5｜知道了这些，会改变我的行动吗？

到这里，我们一直在问 observation 让我们知道了什么。

但一个会行动的 agent 最后还要问另一件事：

> **知道以后，我会做得不一样吗？**

现在终于回到最开始的 A 和 B。

假设你的唯一任务是找到奖品，只能打开左边或右边其中一个盒子。

没有 information 时，成功率是 50%。

A 完美告诉颜色。

它减少了 1 bit uncertainty，也确实让两个 possible worlds 被排除掉了。

但知道红 / 蓝以后，奖品仍然一半在左、一半在右。

所以最优行动没有改变，成功率仍然是 50%。

B 只提供约 0.19 bits 的 information gain，却把位置 belief 从 50 / 50 推到 75 / 25。

按照它提示的方向开盒子，成功率变成 75%。

于是出现了另一个重要区别：

> **能够区分更多 possible worlds，不代表这些 distinctions 都与当前行动有关。**

如果把“找对盒子”记作 1，“找错”记作 0，那么在这个一次 observation 后立刻决策的简化游戏里：

```text
VoI(B) = 0.75 - 0.50 = 0.25
```

而 A 对这个任务的 decision value 是 0。

这就是 **value of information** 想捕捉的东西：得到 observation 以后，最优决策的期望价值提高了多少。

再加上查询成本，问题就更实际。

如果 B 要花 0.10：

```text
Net Value(B) = 0.25 - 0.10 = 0.15
```

值得问。

如果它要花 0.30：

```text
Net Value(B) = 0.25 - 0.30 = -0.05
```

它仍然提供 information，也仍然改善判断，但已经不值得现在获得。

所以最终行动时，问题会变成：

> **这个新的 distinction 会不会改变我的决定？如果会，它值不值得获取？**

这不是在否定 Shannon information。

恰恰相反：Shannon information 告诉我们 uncertainty 减少了多少；possible-world distinctions 告诉我们到底分开了什么；Blackwell 告诉我们一种 observation structure 能不能模拟另一种；decision value 再问这些区别对当前行动有没有用。

它们回答的是不同层次的问题。

## Ending｜真正的问题不是“信息更多了吗？”

现在再回头看“更多 information”这句话，会发现它把几件不同的事压在了一起。

A 和 C 都减少 1 bit uncertainty，却让不同的 possible worlds 变得可区分。

视觉和声音各自可能提供不同的 distinctions；有时它们高度重复，有时彼此补充，还有些 distinction 只有联合观察以后才出现。

一个 channel 也可能只是另一个 channel 的降质版本；或者两者根本无法互相替代。

最后，即使某条 observation 真的让我们区分了更多世界，这些区别也不一定会改变当前行动。

所以我现在更愿意把 information 想成一种关系，而不是装在数据里的固定数量。

它发生在：

```text
hidden state
    ↓
observation channel
    ↓
哪些状态因此变得可区分
    ↓
这些区别如何与其他 observations 组合
    ↓
它们最终是否改变 belief 和 action
```

一条 information 的意义，不只是它让我们少了多少 uncertainty，还在于它让哪些原本混在一起的 possible worlds 变得可以区分。

而当多个 modalities 同时存在时，更值得问的也许不是“又多了多少输入”，而是：

> **它们让系统看见了哪些单一 channel 原本看不见的区别？**

比“我又知道了多少”更值得追问的是：

> **我现在能够区分什么，是以前区分不了的？**

---

## Technical Notes｜不影响主线，可以跳过

### Note 1｜为什么 entropy 里会出现 log？

这里不做完整的公理化推导，只解释公式为什么长成这样。

如果一个结果发生的概率是 `p`，我们希望“越罕见的结果带来的 information 越大”。

同时，如果两个独立事件同时发生，它们的概率相乘：

```text
p₁p₂
```

而我们希望两份独立 information 可以相加。

`log` 正好把乘法变成加法：

```text
log(p₁p₂) = log(p₁) + log(p₂)
```

因为概率越小，information 应该越大，所以加一个负号；以 2 为底时，单位就是 bit：

```text
I(p) = -log₂p
```

今天通常把这个量叫作 **surprisal**。

在 observation 发生以前，我们还不知道会看到哪一种结果，所以把每种结果的 surprisal 按它自己的概率取平均：

```text
H(X) = -Σ pᵢ log₂pᵢ
```

这就是 entropy。

对一个概率分别为 `p` 和 `1-p` 的二选一：

```text
H(p) = -p log₂p - (1-p) log₂(1-p)
```

因此：

```text
H(0.5) = 1 bit
H(0.75) ≈ 0.81 bits
H(1) = 0 bit
```

所以 B 把位置从 `50% / 50%` 变成 `75% / 25%` 时，减少的 uncertainty 是：

```text
1 - 0.81 ≈ 0.19 bits
```

### Note 2｜Blackwell comparison 更严格地在说什么？

假设两个 experiments 都在观察同一个 payoff-relevant state。

如果 experiment C 的 observation 可以经过一个**不依赖真实 state**的随机后处理，变成 experiment B 的 observation，那么 B 可以看作 C 的 garbling。

直观上，拥有 C 的决策者如果真的想按照 B 的方式行动，可以先自己把 C 降质成 B，再使用任何依赖 B 的策略。

因此，在相应的 Bayesian decision problems 中，拥有 C 所能达到的最优期望收益不会低于只拥有 B。

这比“某一个任务里 accuracy 更高”更一般，因为它比较的是 observation structure，而不是某一个特定 payoff 下的一次胜负。

### Note 3｜redundancy、unique information 和 synergy

Williams 和 Beer 在 2010 年提出 Partial Information Decomposition（PID），试图描述多个 sources 关于同一个 target 的 information 如何由不同部分组成，其中包括 redundant 和 synergistic components；后续工作又发展出不同的 unique-information 定义与分解方式。

这里需要注意：PID 并不存在一个所有研究者都统一采用、没有争议的唯一分解。

因此正文并没有把某一种 PID definition 当成最终答案，而只借用了它提出的问题结构：

- 哪些 information 是 sources 之间重复的？
- 哪些只有某个 source 单独提供？
- 哪些只有联合观察以后才出现？

正文里“从不同方向约束 hidden state”的说法，也是为了帮助理解这些信息结构使用的解释性语言，而不是一个新的正式 information-theoretic quantity。

## Further Reading

- Claude E. Shannon (1948), “A Mathematical Theory of Communication,” *Bell System Technical Journal*, 27(3), 379–423; 27(4), 623–656.
- David Blackwell (1953), “Equivalent Comparisons of Experiments,” *The Annals of Mathematical Statistics*, 24(2), 265–272. doi:10.1214/aoms/1177729032.
- D. V. Lindley (1956), “On a Measure of the Information Provided by an Experiment,” *The Annals of Mathematical Statistics*, 27(4), 986–1005. doi:10.1214/aoms/1177728069.
- Ronald A. Howard (1966), “Information Value Theory,” *IEEE Transactions on Systems Science and Cybernetics*, 2(1), 22–26. doi:10.1109/TSSC.1966.300074.
- Paul L. Williams & Randall D. Beer (2010), “Nonnegative Decomposition of Multivariate Information,” arXiv:1004.2515.
- Nils Bertschinger, Johannes Rauh, Eckehard Olbrich, Jürgen Jost & Nihat Ay (2014), “Quantifying Unique Information,” *Entropy*, 16(4), 2161–2183. doi:10.3390/e16042161.
- Paul Pu Liang, Zihao Deng, Martin Q. Ma, James Y. Zou, Louis-Philippe Morency & Ruslan Salakhutdinov (2023), “Factorized Contrastive Learning: Going Beyond Multi-view Redundancy,” *Advances in Neural Information Processing Systems 36 (NeurIPS 2023)*.
- Zequn Yang, Hongfa Wang & Di Hu (2025), “Efficient Quantification of Multimodal Interaction at Sample Level,” *Proceedings of the 42nd International Conference on Machine Learning (ICML 2025)*, PMLR 267, 71302–71317.
