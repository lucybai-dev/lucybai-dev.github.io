# What Does “More Information” Mean?

*A Possible-Worlds View of Information: Why Multimodality Is More Than Just More Input*

> We often say that a model has “seen more information,” or that adding another modality should let a system know more. But if we can only observe the world indirectly through different observation channels, what exactly is the “more” in more information?

## Prologue｜We Always Observe the World Through a Channel

The previous essay ended with a practical question: when the evidence we already have can no longer distinguish among several worlds that remain possible, an active agent has to decide what to observe, what to ask, or what to do next.

But before asking what to observe next, there is a more basic question.

We never receive the hidden state of the world directly.

Vision gives us images. Hearing gives us sounds. A thermometer gives us a reading. A search engine returns a set of retrieved and ranked results. Each is an observation produced through a particular **observation channel**, not the hidden state itself.

We can write the relationship in the simplest possible way:

```text
hidden state H  →  observation O
```

With multiple channels, the same hidden state can generate different observations:

```text
H → O_vision
H → O_audio
H → O_text
```

This essay asks what those arrows actually mean.

Consider a very small box game.

There are two boxes, left and right. A prize is placed randomly in one of them, and the prize itself is randomly red or blue.

We can write the hidden state as:

```text
H = (location, color)
```

There are four equally likely possible worlds:

| Possible World | Location | Color |
|---|---|---|
| H₁ | Left | Red |
| H₂ | Left | Blue |
| H₃ | Right | Red |
| H₄ | Right | Blue |

Now suppose we have three observation channels:

- **A** tells us the color with 100% accuracy;
- **B** tells us the location with 75% accuracy;
- **C** tells us the location with 100% accuracy.

Do not ask yet which one is “best.”

Ask something more specific first:

> **What do these different channels allow us to distinguish?**

## Chapter 1｜What Do Different Channels Let Us Distinguish?

Suppose A tells us, “The prize is red.”

The original four worlds:

```text
{H₁, H₂, H₃, H₄}
```

become:

```text
{H₁, H₃}
```

The two blue worlds are eliminated.

If C tells us, “The prize is on the left,” the four worlds instead become:

```text
{H₁, H₂}
```

The two worlds in which the prize is on the right are eliminated.

A and C both reduce four possibilities to two.

But they do not leave the same two worlds possible.

A distinguishes the red worlds from the blue worlds. C distinguishes the left worlds from the right worlds.

This already exposes something that the phrase “amount of information” can easily hide:

> **Information does not merely reduce the number of possibilities. It changes which possible worlds we can distinguish from one another.**

In this box game, we can even draw the hidden state as a 2 × 2 grid:

| | Red | Blue |
|---|---|---|
| Left | H₁ | H₂ |
| Right | H₃ | H₄ |

A cuts the grid by color. C cuts it by location.

So two observations can eliminate the same number of worlds and still provide completely different **distinctions**.

B is more interesting.

If B says “left,” the two right-side worlds do not disappear, because B is wrong 25% of the time.

Instead, the left-side worlds become more likely and the right-side worlds become less likely.

So an observation does not always create a hard partition. Some observations eliminate worlds outright; noisy observations may only redistribute probability across the worlds that remain possible.

More generally, an observation changes:

> **how we distinguish, eliminate, and weigh the hidden states that may still be true.**

In this toy example, color and location happen to look like two clean “directions” in the hidden state. Real hidden states need not decompose into such tidy coordinates.

That is why I will use the word `distinction` more often than “direction.” The point is not to assume that reality naturally comes with labeled axes. The point is to ask which states a channel makes distinguishable that were previously hard to tell apart.

This is also why information need not be thought of first as a number.

It first changes the structure of the possible worlds.

## Chapter 2｜If Both Give Us 1 Bit, Why Don’t They Tell Us the Same Thing?

Of course, we still want a number for “knowing more.”

The most natural question is:

> **After seeing an observation, how much uncertainty has been removed?**

This is exactly the kind of question Shannon's framework is good at answering.

In the box game, location is a completely uncertain binary choice: left or right.

Color is another completely uncertain binary choice: red or blue.

A fully uncertain binary choice carries 1 bit of uncertainty, so the four equally likely worlds begin with 2 bits in total.

A completely resolves the color, so it removes:

```text
1 bit
```

C completely resolves the location, so it also removes:

```text
1 bit
```

Therefore:

```text
IG(A) = IG(C) = 1 bit
```

But Chapter 1 already showed that A and C do not tell us the same thing at all.

One distinguishes color. The other distinguishes location.

That gives us an important separation:

> **A single number can tell us how much uncertainty disappeared, but not which distinctions we gained.**

Or more simply:

> **“How much do I know?” and “What do I know?” are not the same question.**

Now consider B.

If B says “left,” our belief about location moves from:

```text
50% / 50%
```

to:

```text
75% / 25%
```

We are more certain than before, but not completely certain.

Information theory uses **entropy** to measure how much uncertainty remains in a probability distribution. The entropy of `50% / 50%` is 1 bit, while the entropy of `75% / 25%` is about 0.81 bits.

So B reduces uncertainty about location by roughly:

```text
1 - 0.81 = 0.19 bits
```

B says nothing about color, so its information gain about the full hidden state is still about 0.19 bits.

Thus:

```text
A: about 1 bit
B: about 0.19 bits
```

If the question is only “Which observation reduces more uncertainty on average?”, then by this measure A provides more information.

But there is another subtle point: **that 1 bit is not a fixed amount of information stored inside A itself.**

It depends on what we did not already know.

If we were already 99% certain that the prize was red, then even a perfectly accurate color observation would no longer give us 1 new bit. We were already almost certain about color.

Information gain is therefore better understood as asking:

> **Relative to my prior belief, how much uncertainty does this observation remove on average?**

Why the entropy of `75% / 25%` is about 0.81, and why a `log` appears in the formula, are explained in Technical Note 1. For the main argument, we only need one point:

> **The amount of information matters, but a scalar summary compresses away the structure of what was actually distinguished.**

And that becomes even more important once multiple channels are involved.

## Chapter 3｜When Multiple Channels Come Together, What Actually Gets Added?

Suppose we now have multiple channels at once rather than a single observation.

It is easy to say:

> Image + audio obviously contains “more information” than image alone.

But if Chapters 1 and 2 are right, that conclusion comes too quickly.

The real question is not how many input interfaces we have. It is:

> **Which distinctions does the new channel add that were not already available?**

### 3.1 Two Inputs Can Still Be the Same Information

Start with the most extreme case.

A camera captures one frame, and the system feeds that exact same frame into two different input slots.

Formally, the system now receives two inputs.

But the second copy makes no additional hidden states distinguishable.

```text
O₂ = O₁
```

So:

> **More inputs do not automatically mean more information.**

Real sensors are rarely exact duplicates. Even when two sensors observe the same target, a different viewpoint, noise process, or error source may make the second sensor informative.

The point here is narrower: we cannot count information simply by counting inputs.

### 3.2 Different Channels Can Add Different Distinctions

Return to the boxes.

Suppose vision only sees a close-up of the prize, so it can easily tell red from blue but cannot tell whether the prize is in the left or right box.

The audio channel does not reveal color. Instead, we shake the two boxes and use differences in the resulting sounds to infer—imperfectly—which box contains the prize.

Vision mainly helps distinguish color.

Audio mainly helps distinguish location.

The two channels are not simply repeating the same evidence. They provide different distinctions over the same hidden state.

In this deliberately simple example, it is natural to say that the two modalities constrain the hidden state along different “directions”:

```text
H = (location, color)
```

But “direction” is only an explanatory metaphor here.

A real hidden state may not have a clean set of named coordinates. More generally, different modalities may partition the hypothesis space in different ways, making different possible worlds distinguishable.

So the interesting question about multimodality begins to shift from “How many input formats are there?” to:

> **Does a new modality provide distinctions that the existing channels did not already provide?**

### 3.3 Some Distinctions Appear Only When Channels Are Combined

There is an even more counterintuitive case.

Imagine that the boxes contain a mechanism that emits one visual signal and one audio signal.

The visual signal can be only red or blue.

The audio signal can be only high-pitched or low-pitched.

The mechanism follows this rule:

| Visual | Audio | Prize Location |
|---|---|---|
| Red | High | Left |
| Blue | Low | Left |
| Red | Low | Right |
| Blue | High | Right |

For each location, the two allowed combinations are equally likely.

Now look only at vision.

Red can occur when the prize is left or right. Blue can also occur when the prize is left or right.

So vision alone tells us nothing about location.

The same is true of audio: a high tone can occur when the prize is on either side, and so can a low tone.

But once color and pitch are known together, the location becomes perfectly determined.

The interesting point is not merely that “two useless signals become useful when added together.”

It is that:

> **Neither vision nor audio alone carries information about the location. The location becomes identifiable only from their joint pattern.**

That is the essential intuition behind **synergy**.

If I had to compress this chapter into one sentence, I would say:

> **Multimodal information is not only information accumulation; it can also be constraint composition.**

In other words, the value of multimodality may lie not only in what each channel provides on its own, but also in the new constraints created by their combination.

### 3.4 Where Does This Intuition Show Up in Research?

Up to this point, I had intended the box game only as a way to explain an intuition: different modalities do not merely bring in more information separately; their relationship can expose structure that no single channel reveals on its own.

But when I looked through the literature, I found that closely related questions have already been formalized.

Earlier work on **Partial Information Decomposition (PID)** tried to decompose the information that multiple sources provide about the same target into **redundant, unique, and synergistic** components. The framework introduced by Williams and Beer in 2010 is an important starting point for that line of work.

Using that language to look back at the three toy cases: the duplicated observation in 3.1 is close to the purest form of **redundancy**; the visual and audio channels in 3.2 can be understood intuitively as carrying **unique information** that the other does not provide; and 3.3, where neither vision nor audio reveals location alone but their combination does, has the canonical structure of **synergy**.

This is only an intuitive correspondence. The toy examples are not themselves a rigorous PID decomposition, and the literature does not have one universally accepted definition for every PID component.

Closely related questions have also appeared directly in multimodal learning.

For example, Liang et al. argue in their NeurIPS 2023 paper **Factorized Contrastive Learning** that learning only the redundant information shared across modalities can be insufficient, because task-relevant information may also be unique to a particular modality. Their approach is designed to capture both shared and unique task-relevant information.

More recently, Yang, Wang, and Hu's ICML 2025 paper **Efficient Quantification of Multimodal Interaction at Sample Level** explicitly describes multimodal interaction in terms of redundancy, uniqueness, and synergy, and attempts to quantify those components at the sample level.

I do not mean that these papers somehow “prove” the box-game intuition. Their definitions, assumptions, and technical questions are much more precise than this toy example.

But they do show something interesting:

> **The question we just reached by reasoning from possible worlds—whether multiple modalities repeat the same information, contribute different information, or produce new information only when combined—is a real research question.**

That makes me more comfortable thinking about the value of multimodality in terms of **information structure**, rather than input count alone.

## Chapter 4｜Does Knowing This Change What I Do?

So far, we have asked what an observation lets us know, and what multiple channels add when they are combined.

But an agent that has to act must eventually ask one more question:

> **Will knowing this make me act differently?**

Now return to A and B.

Suppose your only goal is to find the prize, and your only action is to open either the left or the right box.

With no information, your success rate is 50%.

A tells you the color perfectly.

It removes 1 bit of uncertainty and really does eliminate two possible worlds.

But after learning red or blue, the prize is still equally likely to be on the left or the right.

So the optimal action does not change. The success rate remains 50%.

B provides only about 0.19 bits of information gain, but it shifts the location belief from 50 / 50 to 75 / 25.

If you open the box B recommends, the success rate becomes 75%.

That reveals another important distinction:

> **Being able to distinguish more possible worlds does not mean that all of those distinctions matter for the current action.**

If opening the correct box is worth 1 and opening the wrong box is worth 0, then in this simplified one-observation-then-act setting:

```text
VoI(B) = 0.75 - 0.50 = 0.25
```

A has decision value 0 for this particular task.

This is what **value of information** is meant to capture: how much the expected value of the optimal decision improves after receiving an observation.

Once acquisition cost is included, the question becomes even more practical.

If B costs 0.10:

```text
Net Value(B) = 0.25 - 0.10 = 0.15
```

It is worth asking.

If B costs 0.30:

```text
Net Value(B) = 0.25 - 0.30 = -0.05
```

The observation still provides information and still improves the decision, but it is no longer worth acquiring.

So when action is the final goal, the question becomes:

> **Will this new distinction change my decision? If so, is it worth the cost of acquiring it?**

This does not contradict Shannon information.

Shannon information tells us how much uncertainty is reduced. Possible-world distinctions tell us what was separated. When multiple sources are involved, we can ask whether those distinctions are redundant, unique, or available only jointly. Decision value then asks whether those distinctions matter for the action in front of us.

They answer different layers of the problem.

## Ending｜The Real Question Is Not Just “Is There More Information?”

Looking back, the phrase “more information” compresses several different questions into one.

A and C both remove 1 bit of uncertainty, yet make different possible worlds distinguishable.

Vision and audio may provide different distinctions. Sometimes they are highly redundant, sometimes complementary, and sometimes a distinction appears only after the observations are combined.

And even when an observation truly makes more worlds distinguishable, those distinctions may still fail to change the current action.

So I now find it more useful to think of information as a relationship rather than as a fixed quantity stored inside data.

It depends on a chain of relationships:

```text
hidden state
    ↓
observation channel
    ↓
which states become distinguishable
    ↓
how those distinctions combine with other observations
    ↓
whether they ultimately change belief and action
```

The meaning of a piece of information is not only how much uncertainty it removes. It also depends on which possible worlds it allows us to distinguish from one another.

And when multiple modalities are involved, perhaps the better question is not “How much more input did we add?” but:

> **What distinctions can the system now make that no single channel could support on its own?**

More important than asking “How much more do I know?” may be asking:

> **What can I distinguish now that I could not distinguish before?**

---

## Technical Notes｜Optional for the Main Argument

### Note 1｜Why Does Entropy Contain a Log?

This is not a full axiomatic derivation. It is only meant to explain why the formula has this shape.

If an event occurs with probability `p`, we want rarer events to carry more information.

At the same time, if two independent events occur together, their probabilities multiply:

```text
p₁p₂
```

while we want independent information to add.

A logarithm turns multiplication into addition:

```text
log(p₁p₂) = log(p₁) + log(p₂)
```

Because smaller probabilities should correspond to larger information, we add a minus sign. Using base 2 gives units of bits:

```text
I(p) = -log₂p
```

This quantity is commonly called **surprisal**.

Before the observation occurs, we do not know which outcome we will see, so we average the surprisal of each possible outcome, weighted by its own probability:

```text
H(X) = -Σ pᵢ log₂pᵢ
```

This is entropy.

For a binary variable with probabilities `p` and `1-p`:

```text
H(p) = -p log₂p - (1-p) log₂(1-p)
```

Therefore:

```text
H(0.5) = 1 bit
H(0.75) ≈ 0.81 bits
H(1) = 0 bit
```

So when B changes the location belief from `50% / 50%` to `75% / 25%`, the reduction in uncertainty is:

```text
1 - 0.81 ≈ 0.19 bits
```

### Note 2｜What If We Really Want to Ask Whether One Channel Contains Another?

The main text does not pursue this question because it is not necessary for the multimodal argument. But if we want a stronger notion of one observation structure containing another, **Blackwell comparison** gives a classic answer.

Return to B and C:

- B tells the location with 75% accuracy;
- C tells the location with 100% accuracy.

With C, we can take its correct output and independently flip “left / right” 25% of the time:

```text
C  →  add random noise  →  B
```

That produces an observation channel identical to B.

The important condition is that this random post-processing cannot look at the true state again. It may only transform the observation that C has already produced. Otherwise, we are not degrading existing information; we are acquiring new evidence during the transformation.

More generally, suppose two experiments observe the same payoff-relevant state. If the observation from experiment C can be transformed into the observation from experiment B through randomized post-processing that is **independent of the true state**, then B can be viewed as a garbling of C.

A decision-maker with access to C can simulate having B by first garbling C and then applying any policy defined for B.

Therefore, in the corresponding Bayesian decision problems, the optimal expected payoff available with C cannot be lower than the optimal expected payoff available with B.

This is more general than saying that one channel has higher accuracy on one task. It compares observation structures rather than performance under one particular payoff function.

### Note 3｜Redundancy, Unique Information, and Synergy

Williams and Beer introduced **Partial Information Decomposition (PID)** in 2010 as a way to describe how the information that multiple sources provide about the same target can be decomposed into different components, including redundant and synergistic information. Later work developed alternative definitions of unique information and different decomposition schemes.

An important caveat is that there is no single PID decomposition that is universally accepted across the literature.

The main text therefore does not treat any one PID definition as the final answer. It only borrows the structure of the questions PID makes explicit:

- Which information is redundant across sources?
- Which information is available only from a particular source?
- Which information appears only when sources are observed jointly?

Likewise, the language in the main text about constraining the hidden state from different “directions” is explanatory language for understanding these structures, not a new formal information-theoretic quantity.

## Further Reading

- Claude E. Shannon (1948), “A Mathematical Theory of Communication,” *Bell System Technical Journal*, 27(3), 379–423; 27(4), 623–656.
- David Blackwell (1953), “Equivalent Comparisons of Experiments,” *The Annals of Mathematical Statistics*, 24(2), 265–272. doi:10.1214/aoms/1177729032.
- D. V. Lindley (1956), “On a Measure of the Information Provided by an Experiment,” *The Annals of Mathematical Statistics*, 27(4), 986–1005. doi:10.1214/aoms/1177728069.
- Ronald A. Howard (1966), “Information Value Theory,” *IEEE Transactions on Systems Science and Cybernetics*, 2(1), 22–26. doi:10.1109/TSSC.1966.300074.
- Paul L. Williams & Randall D. Beer (2010), “Nonnegative Decomposition of Multivariate Information,” arXiv:1004.2515.
- Nils Bertschinger, Johannes Rauh, Eckehard Olbrich, Jürgen Jost & Nihat Ay (2014), “Quantifying Unique Information,” *Entropy*, 16(4), 2161–2183. doi:10.3390/e16042161.
- Paul Pu Liang, Zihao Deng, Martin Q. Ma, James Y. Zou, Louis-Philippe Morency & Ruslan Salakhutdinov (2023), “Factorized Contrastive Learning: Going Beyond Multi-view Redundancy,” *Advances in Neural Information Processing Systems 36 (NeurIPS 2023)*.
- Zequn Yang, Hongfa Wang & Di Hu (2025), “Efficient Quantification of Multimodal Interaction at Sample Level,” *Proceedings of the 42nd International Conference on Machine Learning (ICML 2025)*, PMLR 267, 71302–71317.