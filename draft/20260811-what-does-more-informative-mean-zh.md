# 什么叫“更有信息”？

*为什么知道得更多，不一定让你做得更好*

> 这篇文章主要沿着一个四种可能世界的盒子游戏，追问一个看起来很简单的问题：当几条 observation 都可以获得时，我们到底在按什么标准说其中一条“更有信息”？

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

但这几个标准真的会给出同一个答案吗？

我们先把问题缩到一个很小的盒子游戏里。

## Chapter 1｜目标固定时，答案其实很简单

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

B 不知道颜色，但能以 **75% 的准确率**告诉你奖品在哪一边。为了让例子保持简单，我们假设它的错误是对称的：奖品在左边时，它有 75% 概率说“左”；奖品在右边时，它也有 75% 概率说“右”。

谁提供了更好的信息？

如果先看准确率，A 好像没有悬念。

它说红色，奖品就一定是红色。

B 说左边，却有四分之一的概率说错。

可一旦真正要打开盒子，结果反过来了。

没有任何信息时，左右各有一半概率，随便选一边，成功率是 50%。

听完 A，虽然颜色确定了，奖品仍然有一半概率在左边、一半概率在右边。

成功率还是 50%。

听完 B，如果直接按照它提示的位置行动，成功率却变成了 75%。

所以如果目标已经固定成：

> **尽可能找到奖品。**

答案其实很简单：选 B。

到这里甚至可以反问：既然目标已经告诉我们什么才有用，这篇文章是不是可以结束了？

如果我们只关心这一局游戏，确实可以。

但 A 又明显不是“没有信息”。

它一旦告诉我们“红色”，四个 possible worlds 就只剩两个：左 + 红，或者右 + 红。

A 排除了一半的世界；B 却更能帮助我们行动。

更有意思的是，只要把任务改成“判断奖品颜色”，排序马上又会反过来：A 直接把正确率提高到 100%，B 几乎没有帮助。

所以这里混在一起的至少有两件事：

> **一条 observation 让我们少了多少不确定性？**

和：

> **这些减少掉的不确定性，对当前行动有没有用？**

这正好把上一篇末尾留下的三个直觉词重新摆到桌上：区分力、决策价值、获取成本。

其中最麻烦的其实是第一个。

上一篇说“区分力”时，它还只是一个直觉标签。真正往下追以后，会发现这个词本身就会分裂成几种不同的问题。

## Chapter 2｜A 明明让我知道得更多，为什么却没用？

先暂时忘掉开哪个盒子，只问：看完一项 observation 之后，我们对真实世界少了多少 uncertainty？

四个世界一开始完全等概率。

一个完全不确定的二选一问题有 1 bit 的 entropy。这个盒子游戏里有两个彼此独立的二选一：位置是左 / 右，颜色是红 / 蓝。所以一开始总共有 2 bits 的 uncertainty。

A 精确告诉颜色，相当于彻底解决了其中一个二选一问题。

所以在这个例子里，A 消除了：

```text
1 bit
```

B 不一样。

假设 B 说“左边”，我们对位置的判断会从：

```text
50% / 50%
```

变成：

```text
75% / 25%
```

信息论用 **entropy** 来衡量这种概率分布还有多不确定。`50% / 50%` 的 entropy 是 1 bit，`75% / 25%` 约是 0.81 bits。

所以 B 让位置的不确定性减少了约：

```text
1 - 0.81 = 0.19 bits
```

在我们假设位置和颜色彼此独立、B 又不透露颜色的前提下，它对完整 hidden state 平均提供的 information 也约是 0.19 bits。

于是出现了文章最重要的反差：

```text
A：约 1 bit
B：约 0.19 bits
```

如果问题是“谁让完整世界少了更多 uncertainty”，A 的确更高。

如果问题是“谁更能帮我找到奖品”，B 更有价值。

两边都没有算错。它们只是在回答不同的问题。

这里还有一个很容易忽略的地方：**这 1 bit 和 0.19 bits 都不是 observation 天生自带的标签。**

它们取决于我们把什么当作 hidden state，也取决于当前 belief。

比如，如果奖品事前就有 99% 概率是红色，只有 1% 概率是蓝色，那么 A 即使仍然 100% 准确地告诉颜色，它平均消除的不确定性也会远小于 1 bit。

所以 information gain 更像是在问：

> **相对于我原来不知道的东西，这次 observation 到底让我少不知道了多少？**

这也解释了为什么同一个 information source，对不同 belief state 的 agent 可能价值完全不同。

还有最后一步。

刚才我们是在 B 已经说出“左边”以后计算 information gain。但在真正去问 B **之前**，我们还不知道它会说左还是右。

在这个对称例子里，两种回答各有一半概率，而且无论得到哪一种，uncertainty 都会减少约 0.19 bits。因此在询问之前，我们就可以说：这次 observation 的 **expected information gain** 约是 0.19 bits。

如果不同回答能带来不同程度的 uncertainty reduction，就按它们出现的概率取平均。

Lindley 在 1956 年把这种 Shannon 式的信息度量带进对 experiment 的比较：在结果出现以前，用 prior 来衡量一个 experiment 预计能提供多少 information。

如果你想看 entropy 的公式为什么会出现 `log`，以及上面的 `0.81` 是怎么算出来的，文末 Technical Note 1 单独展开；主线先继续往下走。

现在我们至少有了一种“更多”的含义：

> **平均减少更多 uncertainty。**

不过，“减少了多少 uncertainty”仍然只是一种比较方式。

## Chapter 3｜脱离当前任务，还能怎么比较？

继续用盒子。

除了 A 和 B，再加入一个信息源 C：

- B：75% 准确地告诉奖品在哪一边；
- C：100% 准确地告诉奖品在哪一边。

这个例子里，光看 accuracy 就知道 C 更好。

但有一个比 `100% > 75%` 更值得抓住的结构。

如果你拥有 C，想模拟 B 很容易：C 每次都告诉你正确位置，你只要完全随机地挑出 25% 的时候，把“左 / 右”故意说反，就得到一个和 B 一样的 channel。

也就是说：

> **C 可以通过主动加入噪声来模拟 B；B 却无法从 noisy output 中恢复 C 原本那个确定的位置 observation。**

这就是 Blackwell comparison 最重要的直觉。

它关心的不是某一次任务里谁赢，而是：一个 information source 是否已经包含了另一个 source 能提供的全部能力。

这里要守住一个条件：这层随机后处理只能使用 C 已经给出的 observation，不能再去查看真实 state。否则它就不只是把现有 observation 弄得更粗糙，而可能借机重新获取新的 state information。

在简单的 B/C 例子里，这个标准看起来有点大材小用。

真正有价值的是，当一个 experiment 已经无法被单一 accuracy 概括时，我们仍然可以问：它能不能通过这种不看真实 state 的随机后处理，生成另一个 experiment？

再回到最开始的 A 和 B，情况就不同了。

A 有颜色 information，却没有位置信息；B 有带噪声的位置信息，却没有颜色 information。

A 无法随机后处理出 B，B 也无法随机后处理出 A。

所以它们并不是简单的一强一弱。

Blackwell comparison 一般也不是一条能把所有 information sources 从强到弱排好的直线。很多 channels 本来就不可比。

更严格的定义，以及它为什么意味着“在相应的 Bayesian decision problems 里，较强的 experiment 不会更差”，放在 Technical Note 2。

到这里，我们已经看到第二种“更多”：

> **不是多几个 bits，而是一个 channel 是否完整包含另一个 channel 的能力。**

可现实中的系统通常不只有一个 channel。

如果两条 information 同时出现，事情还会再变一次。

## Chapter 4｜两条 information 放在一起以后呢？

还是盒子。

假设一个视觉 channel 只能看到奖品的局部特写，因此知道它是红色还是蓝色，却看不到它在哪个盒子里。

另一个声音 channel 看不到颜色，却可以分别晃动两个盒子，根据声音差异，带着一定误差判断奖品在哪一边。

视觉主要约束颜色，声音主要约束位置。

这时第二个 channel 的价值，不在于系统突然有了“另一种输入格式”，而在于它排除了旧 channel 无法排除的世界。

但两个 channels 放在一起，也不能简单把各自的 information 相加。

有时它们在重复同一种约束。两个位置接近、共享盲区的摄像头，即使都说“左边”，第二个 observation 也不等于又得到了一份完全独立的新 evidence。

有时它们提供的是彼此没有的约束。视觉告诉颜色，声音帮助判断位置，这两条 observation 在 hypothesis space 里切的是不同方向。

还有一种更有意思的情况：单独看都没用，放在一起才有用。

想象左右两个盒子上各有一盏灯。规则是：奖品在左边时两盏灯同色，奖品在右边时两盏灯异色。

单独看任何一盏灯，都不知道奖品在哪；同时看到两盏，却能立刻判断位置。

这就是 **synergy** 最小的直觉。

Partial Information Decomposition（PID）试图把这类关系形式化：多个 sources 关于同一个 target 的 information 中，哪些是重复的，哪些是各自独有的，哪些只有联合以后才出现。这里不展开具体分解，因为 PID 本身也没有唯一公认的定义。

对这篇文章来说，只需要记住一个更简单的结论：

> **多一个 channel，不等于简单多一份 information；关键在于它新增了什么约束，以及这些约束和已有 evidence 是什么关系。**

甚至“看到十次相同结论”也不自动等于十个独立 evidence，因为这些 observations 可能共享同一个错误来源。

## Chapter 5｜最后到底该拿哪一条？

现在可以回到最开始那个最实际的问题。

如果目标已经固定，真正决定“该不该拿”一条 information 的，还是它会不会改善行动。

这就是 **value of information** 的问题。

在找盒子的任务里，没有任何信息时，最优成功率是 0.50；获得 B 以后，按它提示的位置行动，成功率是 0.75。

所以在这个把收益直接定义成“找对盒子的概率”的简化设定里：

```text
VoI(B) = 0.75 - 0.50 = 0.25
```

但有价值，还不等于值得获取。

如果询问 B 的成本是 0.10：

```text
Net Value(B) = 0.25 - 0.10 = 0.15
```

值得问。

如果成本是 0.30：

```text
Net Value(B) = 0.25 - 0.30 = -0.05
```

它仍然能改善判断，却已经不值得买。

所以，在本文这种“先获取一次 information，再立即做决策”的设定里，如果任务、utility 和 cost 都已经给定，最开始那个简单答案其实一直是对的：

> **选择能让决策变得更好，而且扣除获取成本以后仍然最值得的 evidence。**

第四篇之所以没有在 Chapter 1 就结束，是因为“更有信息”这句话往往没有把自己问的到底是哪一个问题说清楚。

现在可以把这些问题放在同一张表里：

| 你真正想问什么？ | 对应的比较 |
|---|---|
| observation 平均减少多少 uncertainty？ | information gain |
| 一个 channel 是否完整包含另一个 channel 的能力？ | Blackwell comparison |
| 多个 channels 放在一起新增了什么？ | redundancy / unique / synergy |
| 这条 information 会不会改善当前行动？ | value of information |
| 改善是否值得它的成本？ | net value after cost |

它们不是五个互相竞争的“信息定义”。

它们只是回答不同层次的问题。

所以我现在更愿意把上一篇留下的三个直觉词这样理解：

**区分力**需要继续拆开：减少多少 uncertainty、是否存在结构性的 dominance、多个 sources 如何组合。

**决策价值**必须相对于具体任务来定义。

**获取成本**最后决定，一条有价值的 information 是否值得现在去拿。

> **information 不是事实数量的简单累加，而是 observation、hidden state、当前 belief、行动目标和获取条件之间的关系。**

如果一次 query 还会改变未来状态、影响之后能看到什么，或者决定下一步还能问什么，那么这里的一次性比较就不够了。那已经是序贯的信息获取问题，不再是本文讨论的这类一次性比较。

## Technical Notes｜不影响主线，可跳过

### Note 1｜为什么 entropy 里会出现 log？

这不是完整的公理化推导，只是解释公式为什么有这样的形状。

假设一次公平抛硬币出现正面，事前概率是 `1/2`。我们把它带来的 information 定成 1 bit。

如果两次独立抛硬币都出现正面，联合概率是：

```text
1/2 × 1/2 = 1/4
```

而两次独立结果带来的 information 应该可以相加：

```text
1 bit + 1 bit = 2 bits
```

所以我们希望找到一个函数，能把“概率相乘”变成“information 相加”。`log` 正好满足：

```text
log(p₁p₂) = log(p₁) + log(p₂)
```

又因为概率越小，information 应该越大，所以加上负号，并以 2 为底：

```text
I(p) = -log₂p
```

今天通常把这样的 `I(p)` 叫作 **surprisal**。

在结果发生以前，我们不知道最后会看到哪一种结果，于是把每种结果的 surprisal 按它自己的概率取平均：

```text
H(X) = Σ pᵢ I(pᵢ)
     = -Σ pᵢ log₂pᵢ
```

对于概率分别是 `p` 和 `1-p` 的二选一问题：

```text
H(p) = -p log₂p - (1-p) log₂(1-p)
```

因此：

```text
H(0.5) = 1 bit
H(0.75) ≈ 0.81 bits
H(1) = 0 bit
```

某个具体结果发生后的 `-log p` 是 surprisal；结果发生前的平均 surprisal 是 entropy；一项 observation 让前后的 uncertainty 减少多少，才是正文真正关心的 information gain。

### Note 2｜Blackwell comparison 更严格地在说什么？

假设两个 experiments 都在观察同一个 payoff-relevant state。

如果 experiment C 的 observation 可以经过一个**不依赖真实 state**的随机后处理，变成 experiment B 的 observation，那么 B 可以看作 C 的 garbling。

这时，拥有 C 的决策者如果真的想按 B 的方式行动，可以先把 C 经过随机后处理变成 B，再使用任何只依赖 B 的策略。

因此，在相应的 Bayesian decision problems 中，拥有 C 所能达到的最优期望收益不会低于只拥有 B。

Blackwell comparison 比“某一个任务里 accuracy 更高”更一般：它比较的是 observation structure，而不是某一个特定 payoff 下的一次胜负。

## Further Reading

- Claude E. Shannon (1948), “A Mathematical Theory of Communication,” *Bell System Technical Journal*, 27(3), 379–423; 27(4), 623–656.
- David Blackwell (1953), “Equivalent Comparisons of Experiments,” *The Annals of Mathematical Statistics*, 24(2), 265–272. doi:10.1214/aoms/1177729032.
- D. V. Lindley (1956), “On a Measure of the Information Provided by an Experiment,” *The Annals of Mathematical Statistics*, 27(4), 986–1005. doi:10.1214/aoms/1177728069.
- Ronald A. Howard (1966), “Information Value Theory,” *IEEE Transactions on Systems Science and Cybernetics*, 2(1), 22–26. doi:10.1109/TSSC.1966.300074.
- Paul L. Williams & Randall D. Beer (2010), “Nonnegative Decomposition of Multivariate Information,” arXiv:1004.2515.
- Nils Bertschinger, Johannes Rauh, Eckehard Olbrich, Jürgen Jost & Nihat Ay (2014), “Quantifying Unique Information,” *Entropy*, 16(4), 2161–2183. doi:10.3390/e16042161.