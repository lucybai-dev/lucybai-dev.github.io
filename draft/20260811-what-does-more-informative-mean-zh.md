# 什么叫“更有信息”？

*为什么知道得更多，不一定让你做得更好*

> 这篇文章只用一个四种可能世界的盒子游戏，试着回答一个看起来很简单的问题：当几条 observation 都可以获得时，我们到底在按什么标准说其中一条“更有信息”？

## Prologue｜如果有几条信息可以选，应该选哪一条？

上一篇文章最后留下了一个问题。

当现有 evidence 已经无法继续区分仍然可能的世界，继续围绕同一份信息思考，并不会凭空产生新的 observation。

这时，一个更主动的智能体需要决定下一步去看什么、问什么，或者做什么。

可现实里，下一步往往不只有一种 observation 可以获得。

如果同时有几条信息可以选择，我们应该选哪一条？

直觉上，这似乎不难。

更准确的，应该更好。

知道更多细节的，应该更好。

能排除更多可能世界的，似乎也应该更好。

上一篇文章里，我把“区分力”“决策价值”和“获取成本”并排放在一起，用来描述一条 observation 是否值得获取。

然而再往前一步，一旦真的要在几条 observations 之间做选择，这几个标准就不一定指向同一个答案。

它们看起来像是在描述同一件事的不同方面，实际上却可能给出完全相反的排序。

为了把问题缩到足够小，我们先回到一个只有四种可能世界的盒子游戏。

## Chapter 1｜如果目标固定，答案其实很简单

桌上有左、右两个盒子。

奖品随机放在其中一个盒子里，同时奖品本身随机是红色或蓝色。

于是隐藏的真实状态一共有四种等概率可能：

| 可能世界 | 奖品位置 | 奖品颜色 |
|---|---|---|
| H₁ | 左 | 红 |
| H₂ | 左 | 蓝 |
| H₃ | 右 | 红 |
| H₄ | 右 | 蓝 |

你最后只能做一个动作：打开左边或者右边的盒子。

现在有两个信息提供者。

A 能够 **100% 准确**地告诉你奖品是什么颜色。

B 不知道颜色，但能以 **75% 的准确率**告诉你奖品在哪一边。

这里我们还假设 B 的错误是对称的，**即奖品在左边时，它有 75% 概率说“左”；奖品在右边时，它也有 75% 概率说“右”。**

谁提供了更好的信息？

如果先看准确率，A 好像没有悬念。

它说红色，奖品就一定是红色。

B 说左边，却有四分之一的概率说错。

可一旦真正要打开盒子，结果反过来了。

没有任何信息时，左右各有一半概率，随便选一边，成功率是 50%。

听完 A，虽然颜色确定了，奖品仍然有一半概率在左边、一半概率在右边。

你的成功率还是 50%。

听完 B，如果直接按照它提示的位置行动，成功率却变成了 75%。

所以如果目标已经固定成：

> **尽可能找到奖品。**

那这一局其实没什么悬念。

选 B。

它对当前任务更有价值。

甚至可以说，到这里问题已经解决了。

可这时又出现一个很自然的反问：

> **那这篇文章是不是到这里就可以结束了？既然目标不同会改变排序，直接选择最能提高目标表现的 evidence 不就行了吗？**

如果我们只关心这一局游戏，确实可以。

但“对当前任务最有用”和“本身包含更多 information”并不是同一个问题。

只要把任务改一下，这个区别就会出现。

假设明天的任务不再是找盒子，而是判断奖品是什么颜色。

这时 A 会直接把正确率提高到 100%，B 却几乎没有帮助。

同样两条 information，任务一变，排序就反过来了。

这说明：

> **task-specific value 可以告诉我们当前应该选谁，却不能自动回答一条 information source 在更一般意义上“知道了多少”。**

有时我们还没有确定未来任务是什么，却已经需要比较两个传感器、两个数据源或两种 observation mechanisms。

我们在前三篇其实一直沿着同一条线往前走：第一篇问 representation 应该保留哪些差异；第二篇问真实状态不可见时，哪些 possible worlds 仍然可能成立；第三篇再问，当现有 observation 不够时，智能体怎样主动制造新的 evidence。

到了这里，问题自然变成了：

> **如果可以选择下一条 evidence，我们究竟应该怎样比较它们？**

如果暂时不问“我现在要做什么”，还能怎样比较两条 information？

## Chapter 2｜不谈任务，只问：它减少了多少不确定性？

先暂时忘掉开哪个盒子。

只问一个更纯粹的问题：

> 看完一项 observation 之后，我们对真实世界少了多少不确定性？

四个世界一开始完全等概率。

如果要确定真实世界是哪一个，相当于需要解决两个独立的二选一问题：

1. 奖品在左边还是右边？
2. 奖品是红色还是蓝色？

一个完全不确定的二选一问题，可以看作 1 bit 的不确定性。

因此四个等概率世界一开始有：

```text
log₂4 = 2 bits
```

的不确定性。

A 精确告诉颜色，相当于把第二个二选一问题彻底解决。

四个世界缩成两个，所以 A 消除了 1 bit 的不确定性。

B 不一样。

假设 B 说：“左边。”

我们对位置的判断会从：

```text
50% / 50%
```

变成：

```text
75% / 25%
```

我们显然比原来确定了一些，但又没有完全确定。

信息论用 **entropy** 来衡量这种不确定程度。

但 entropy 的公式并不是必须死记的东西。它可以从两个很直观的要求一路走出来。

### 第一步：越不可能发生的结果，发生以后应该带来更多 information

假设一次公平抛硬币出现正面，事前概率是 `1/2`。

我们把它带来的 information 定成 1 bit。

如果两次独立抛硬币都出现正面，这个联合结果的概率是：

```text
1/2 × 1/2 = 1/4
```

直觉上，两次独立结果带来的 information 应该可以相加：

```text
1 bit + 1 bit = 2 bits
```

所以我们想找一个函数，能把“概率相乘”变成“information 相加”。

log 正好有这个性质：

```text
log(p₁p₂) = log(p₁) + log(p₂)
```

又因为概率越小，information 应该越大，所以加上负号，并以 2 为底：

```text
I(p) = -log₂p
```

于是：

```text
I(1/2) = 1 bit
I(1/4) = 2 bits
```

这里的 `I(p)` 可以理解成：**某个具体结果一旦发生，它有多“意外”、因而带来了多少 information。**

### 第二步：结果还没发生时，把各种可能结果的信息量取平均

在 observation 发生以前，我们不知道最后会看到哪个结果。

所以一种自然的做法，就是把每个可能结果的信息量，按照它自己发生的概率取平均：

```text
H(X) = Σ pᵢ I(pᵢ)
```

把刚才的：

```text
I(pᵢ) = -log₂pᵢ
```

代进去，就得到：

```text
H(X) = -Σ pᵢ log₂pᵢ
```

对于只有两种结果、概率分别是 `p` 和 `1-p` 的二选一问题，就是：

```text
H(p) = -p log₂p - (1-p) log₂(1-p)
```

于是几个端点都很直观。

当概率是 `50% / 50%` 时，我们最拿不准：

```text
H(0.5) = 1 bit
```

当概率是 `100% / 0%` 时，答案已经确定：

```text
H(1) = 0 bit
```

`75% / 25%` 位于两者之间：

```text
H(0.75) ≈ 0.81 bits
```

所以关于“奖品在哪边”这件事，我们原本有 1 bit 的不确定性，听完 B 以后还剩约 0.81 bits。

B 消除的约是：

```text
1 - 0.81 = 0.19 bits
```

由于奖品的位置和颜色彼此独立，而 B 的输出只依赖位置、不额外透露颜色，所以它对完整 hidden state 消除的不确定性也约是 0.19 bits。

于是一个有点反直觉的结果出现了：

```text
A：约 1 bit
B：约 0.19 bits
```

按照“减少了多少不确定性”这个标准，A 的确比 B 更有 information。

这和 Chapter 1 并不矛盾。

A 让我们对完整世界知道得更多。

B 却让我们在当前任务里做得更好。

两个答案在比较不同的东西。

### observation 还没发生时怎么办？

刚才我们是在 B 已经说出“左边”以后，计算这条 observation 减少了多少 uncertainty。

但在询问 B **之前**，我们还不知道它会说“左”还是“右”。

在这个完全对称的例子里，B 说左和说右各有一半概率；无论它说哪边，我们对位置的 entropy 都会从 1 bit 降到约 0.81 bits，也就是减少约 0.19 bits。

所以在开口询问 B 之前，我们就可以计算：

```text
0.5 × 0.19 + 0.5 × 0.19 = 0.19 bits
```

也就是说，这次 observation **预计**会带来约 0.19 bits 的 information。

如果不同回答带来的 uncertainty reduction 不一样，就按每种回答发生的概率分别加权。

这就是 **expected information gain** 的基本直觉：在 experiment 发生之前，比较它平均预计能减少多少不确定性。

Lindley 在 1956 年把这种思路系统化地用于实验所提供信息的度量，也成为后来 Bayesian experimental design 的重要起点之一。

到这里，我们已经得到第一种比较 information 的办法：

> **不管当前任务是什么，只问 observation 平均让我们少了多少 uncertainty。**

但它仍然没有回答另一个问题。

如果两个 information sources 看的是同一件事，其中一个明显只是另一个的“劣化版”，能不能说前者在一个更强的意义上包含了后者？

## Chapter 3｜什么时候我们真的可以说：一个 channel 更强？

继续用盒子。

除了刚才的 A 和 B，现在再加入 C。

- B：以 75% 的准确率告诉你奖品在哪一边。
- C：100% 准确告诉你奖品在哪一边。

这一次我们很想说：

> C 不只是对当前任务更好，它似乎真的“包含”了 B 能提供的一切。

为什么？

因为如果你拥有 C，想模拟 B 非常容易。

C 每次都给出正确位置。

你只需要在得到 C 的答案以后，**完全随机地挑出 25% 的时候，把“左 / 右”故意说反。**

最后得到的新 channel 就会有：

```text
75% 正确
25% 错误
```

也就是和 B 一样。

这里的关键不是“75%”这个数字本身。

关键是：

> **拥有 C 的人，可以通过主动丢掉一部分 information 来模拟 B；拥有 B 的人却无法恢复 C 已经丢掉的那 25%。**

而且这种“故意说反”的规则不能偷偷查看真实 state 再决定什么时候翻转。

它必须只是对 C 的 observation 做与真实 state 无关的随机后处理。

否则就不是单纯在降质，而可能在后处理中重新加入新的 state information。

Blackwell comparison 抓住的正是这种关系。

如果一个 experiment 的 observation 可以经过 **state-independent garbling**，变成另一个 experiment 的 observation，那么前者在 Blackwell 意义下至少和后者一样 informative。

这个判断比“在当前这一个任务里谁准确率高”更强。

直觉很简单：

> 如果拥有 C 的人真的想按 B 的方式做任何事，他随时可以先把 C 降质成 B，再使用只看 B 的策略。

所以在以同一个 payoff-relevant state 为基础、按照期望效用做选择的 Bayesian 决策问题中，拥有 C 能达到的最优期望收益不会低于只拥有 B。

但这也马上暴露出 Blackwell ordering 的边界。

回到最开始的 A 和 B：

- A 完整告诉颜色，却不告诉位置；
- B 带噪声地告诉位置，却不告诉颜色。

A 无法通过随机后处理制造出 B，因为它根本没有位置信息。

B 也无法制造出 A，因为它根本没有颜色信息。

所以在这个盒子设定里，它们不是简单的一强一弱。

它们观察的是 hidden state 的不同方向。

这就是为什么 information sources 往往只能形成**偏序**，而不是全部排成一条从第一名到最后一名的直线。

现在我们有了第二种比较：

> **不固定一个具体任务，而问一个 channel 是否保留了另一个 channel 能够提供的全部决策能力。**

可是现实中的系统通常不只有一个 channel。

如果两条 information 同时出现，又会发生什么？

## Chapter 4｜两条 information 放在一起以后发生了什么？

还是盒子。

假设一个视觉 channel 只得到奖品的局部特写，因此能够准确判断它是红色还是蓝色，却看不到它位于左边还是右边。

另一个声音 channel 看不到颜色，却能够**分别晃动两个盒子制造声音**，根据声音差异，带着一定误差判断奖品在哪一边。

视觉主要约束颜色。

声音主要约束位置。

两个 channels 面对的是同一个 hidden state，却在切 hypothesis space 的不同方向。

从这个角度看，多模态可能有价值，并不是因为输入格式从一种变成了两种。

真正可能增加的是：

> **新的 channel 排除了旧 channel 无法排除的那些世界。**

但两个 channels 放在一起，也不能简单说 information 等于两边相加。

至少有三种不同情况。

### 1. 重复

两个摄像头都在判断奖品位于左边还是右边，而且位置接近、视角接近、共享同一个盲区。

第一个已经告诉你“左边”。

第二个也告诉你“左边”。

第二条 observation 可能还有一点新增价值，但显然不是又得到了一份完全不同的信息。

### 2. 各自独有

视觉告诉颜色。

声音帮助判断位置。

一个 channel 排除的是“颜色不对”的世界，另一个排除的是“位置不对”的世界。

它们提供的是彼此没有的约束。

### 3. 单独都没用，组合起来才有用

还可以把盒子游戏稍微改一下。

现在左右两个盒子上各有一盏指示灯，每盏灯都随机显示红色或蓝色。

规则是：

- 如果奖品在左边，两盏灯颜色相同；
- 如果奖品在右边，两盏灯颜色不同。

只看左边那盏灯，没有用。

它显示红色还是蓝色，单独都不能告诉你奖品在哪。

只看右边那盏也一样。

但如果同时看到两盏灯：

- 同色 → 奖品在左边；
- 异色 → 奖品在右边。

这时，任何一个 observation 单独都没有关于目标位置的信息，两个放在一起却能完全确定答案。

这就是 **synergy** 最小的直觉。

Partial Information Decomposition（PID）试图把这类问题形式化：多个 sources 关于同一个 target 的 information 中，哪些是重复的，哪些是各自独有的，哪些只有联合以后才出现。

这里不展开某一种具体 PID 定义，也不把它当作已经有唯一标准答案的理论。

对这篇文章来说，它最重要的作用只是提醒我们：

> **多个 channels 的价值不能只看每一个 channel 单独有多少 information，还要看它们彼此是什么关系。**

而且“内容重复”还不是最危险的情况。

十篇新闻都报道同一件事，可能只是重复。

如果继续追进去，发现十篇新闻全部引用同一条未经证实的原始消息，那问题就变成了另一件事：它们的**错误来源也是相关的**。

十篇报道，不等于十个独立 evidence。

三个模型给出相同答案，也不一定等于三个独立判断。

它们可能使用相似训练数据、相似检索源、相似方法，甚至共同依赖同一个上游信息。

所以当多个 channels 放在一起时，我们至少要问两层问题：

> 它们提供的是重复、独有还是协同的约束？

以及：

> 它们的错误到底有多独立？

## Chapter 5｜所以，“更有信息”到底是在问什么？

现在可以回到文章最开始的问题。

A 100% 准确告诉颜色。

B 75% 准确提示位置。

谁更有 information？

答案取决于你究竟在问哪一种问题。

### 如果你问：哪条 observation 让完整世界少了更多 uncertainty？

A 更高。

它排除了关于颜色的一半 possible worlds。

这是 Shannon information / information gain 在回答的问题。

### 如果你问：一个 channel 是否完整保留了另一个 channel 能提供的能力？

那就要看是否存在 Blackwell dominance。

C 能通过 state-independent garbling 变成 B，所以 C 在这个意义上比 B 更强。

A 和 B 在这个盒子设定下则不可比，因为它们约束的是 hidden state 的不同部分。

### 如果你问：在当前任务里哪条 information 最值得拿？

那就回到 Chapter 1。

任务是找盒子，就选 B。

任务是判断颜色，就选 A。

更一般地，一项 query 的 **value of information** 可以理解成：

```text
VoI(q)
= 获得 observation 后能够达到的预期最优决策价值
- 没有这项 observation 时的最优决策价值
```

如果获取 q 还要花成本 `c(q)`，则进一步比较：

```text
Net Value(q) = VoI(q) - c(q)
```

所以你在开头提出的那个最直接答案其实一直是对的：

> **如果任务、utility 和 cost 都已经给定，就应该选择能最大化当前决策价值的 evidence。**

第四篇之所以还要继续往下走，只是因为“哪条 information 对当前任务最有用”并不等同于“哪条 information source 在更一般意义上更 informative”。

前者是一个具体决策问题。

后者还可能在问 uncertainty reduction、Blackwell ordering，或者多个 sources 之间的 redundancy、synergy 和 dependence。

所以我现在更愿意把“哪条信息更有信息”理解成一个没有完全说清楚的问题。

它不是一个天然只有单一数字答案的问题。

> **information 不是事实数量的简单累加。**

一条 observation 的意义，来自它如何改变我们对仍然可能的世界的区分；它和已经拥有的 evidence 是什么关系；以及这些改变最后会不会真正抵达行动。

## 这一篇装进系统以后

为了不让这些理论停在“几种不同的信息概念”上，可以把这一篇压回三个问题。

### 人类已经解决了什么？

我们已经有几种相当成熟、但回答不同问题的工具。

- information gain：一项 observation 平均减少多少 uncertainty；
- Blackwell ordering：一个 observation mechanism 是否可以看作另一个的随机降质；
- value of information：在具体 belief、action 和 utility 下，它是否真的改善决策；
- multi-source information：多个 observations 之间是否存在 redundancy、synergy 或相关错误。

### 这些答案依赖什么假设？

这些比较都不是凭空成立的。

information gain 需要先定义 hidden state 和概率分布。

Blackwell comparison 假设 observation mechanisms 本身是已知的。

value of information 需要当前 belief、可选 actions、utility 和 cost。

多个 channels 的联合更新还依赖我们是否正确建模了它们之间的 dependence。

换句话说，第四篇并没有解决“information 从哪里来”。

它只是告诉我们：

> **如果 hidden state、observation mechanism、utility 等结构已经给定，information 可以怎样被比较。**

### 这个理论在系统中放在哪里？

如果把它装进一个更完整的智能体，它对应的可以是一个 **Information Evaluator**。

输入包括：

- 当前 belief；
- 候选 observation channels 或 queries；
- observation model；
- 当前任务的 utility；
- acquisition cost。

输出则不是一个单一的“信息分数”，而是一组不同判断：

- expected information gain；
- 是否存在 Blackwell dominance，还是两个 channels 根本不可比；
- task-specific value of information；
- net value after cost；
- 多个 sources 之间是否存在明显的重复、协同或相关错误。

这一步还不替 agent 决定“下一步问什么”。

它只是先把一个更基础的问题说清楚：

> **当几个 observations 摆在面前时，我们究竟是在按什么标准比较它们？**

## Epilogue｜为什么偏偏让我看见它？

到这里，我们已经把“更有信息”拆成了几种不同问题。

但整篇文章其实一直默认了一件事。

我们默认自己知道 observation 是怎样产生的。

B 为什么有 75% 准确率，我们知道。

C 怎样通过随机降质变成 B，我们也知道。

不同 channels 是否共享错误来源，至少在理论上也被当作一个可以建模的问题。

现实中，这些东西往往恰恰是未知的。

搜索结果经过排序。

新闻经过选择和转载。

工具接口决定向我们暴露完整状态，还是只返回一个摘要。

传感器有自己的失真机制。

另一个 agent 甚至可能有自己的目标，并主动选择什么告诉我们、什么不告诉我们。

所以 observation 不仅有内容。

它还有自己的生成过程、选择过程和呈现过程。

到了这里，问题就不再只是：

> 哪一条 information 更有信息？

而会继续往下一层走：

> **为什么偏偏让我看见它？**

## Further Reading

- Claude E. Shannon (1948), “A Mathematical Theory of Communication,” *Bell System Technical Journal*, 27, 379–423, 623–656.
- David Blackwell (1953), “Equivalent Comparisons of Experiments,” *The Annals of Mathematical Statistics*, 24(2), 265–272.
- D. V. Lindley (1956), “On a Measure of the Information Provided by an Experiment,” *The Annals of Mathematical Statistics*, 27(4), 986–1005.
- Ronald A. Howard (1966), “Information Value Theory,” *IEEE Transactions on Systems Science and Cybernetics*, 2(1), 22–26.
- Paul L. Williams & Randall D. Beer (2010), “Nonnegative Decomposition of Multivariate Information,” arXiv:1004.2515.
- Nils Bertschinger, Johannes Rauh, Eckehard Olbrich, Jürgen Jost & Nihat Ay (2014), “Quantifying Unique Information,” *Entropy*, 16(4), 2161–2183.