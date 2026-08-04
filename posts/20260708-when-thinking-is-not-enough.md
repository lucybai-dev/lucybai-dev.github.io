# When Thinking Is Not Enough

*When existing evidence can no longer distinguish between competing worlds, how does action become part of reasoning?*

> You do not need any prior knowledge of the Monty Hall problem, Bayesian inference, or causal inference. This essay uses a simple door-opening game with explicit rules to show why the same observation can carry different meanings under different generating mechanisms, and how action can become an experiment for acquiring new evidence.

## Prologue｜When Existing Information Has Said All It Can Say

In the previous essay, I tried to understand reasoning as something less mysterious:

We observe a set of phenomena, construct several worlds that may still be possible, and then use new facts, rules, and actions to eliminate the worlds that can no longer remain self-consistent.

This approach allows us to preserve multiple explanations temporarily. It also allows us to admit that, when information is insufficient, the truth may not be uniquely identifiable.

But it leaves one question unanswered.

If all available information has already been fully used, yet several worlds still explain everything in front of us, does further thought help?

Sometimes, the answer is no.

Not because the reasoning is insufficiently deep, but because the existing information has already said everything it can say.

The Monty Hall problem offers a world small enough to examine completely, yet rich enough to contain almost all of the essential structure.

## Chapter 1｜Three Doors and Three Worlds

There are three doors in front of you. A prize is behind one of them. The other two conceal nothing.

You choose Door 1.

At this point, the prize may be in:

- **World 1:** behind Door 1;
- **World 2:** behind Door 2;
- **World 3:** behind Door 3.

With no additional information, each world has probability `1/3`.

You can keep staring at the doors. You can analyze their colors, the positions of their handles, or why you chose Door 1.

But as long as those details are unrelated to the prize's location, they do not change the relative probabilities of the three worlds.

Thought can reorganize the information we already have. It cannot manufacture new evidence from nothing.

The “possible worlds” in the previous essay are usually called **hypotheses** in statistical inference. The probability assigned to those hypotheses is our current **belief**.

Let `H` be the hidden state and `O` the observed phenomenon. Bayesian updating can be written as:

```
P(H | O) ∝ P(O | H)P(H)
```

This formula contains three basic concepts.

**Prior:** how likely we consider each world before seeing the new observation.

**Likelihood:** if a particular world is true, how likely we are to see this observation.

**Posterior:** how likely we should consider each world after seeing the observation.

In simple terms:

```
Posterior ∝ Likelihood × Prior
```

Then the host walks over.

## Chapter 2｜Why the Host Makes Switching Better

The host knows where the prize is and follows three rules:

1. He never opens the Door 1 you already chose;
2. He never opens the door with the prize;
3. He always opens an empty door and then asks whether you want to switch.

If the prize is behind Door 1, he randomly opens one of the other two empty doors.

Now he opens Door 3. It is empty.

Should you stay, or switch to Door 2?

You should switch.

The reason can be stated in two sentences:

> Your original choice has only a `1/3` chance of being correct and a `2/3` chance of being wrong. If your first choice is wrong, a host who knows the answer is forced to open the other empty door and leave the prize-winning door available for you to switch to, so switching wins precisely in those `2/3` of cases.

The likelihood gives the same answer.

Let `O` mean “the host opens Door 3, and it is empty.”

- If the prize is behind Door 1, he has a one-half chance of opening Door 3;
- If the prize is behind Door 2, he must open Door 3;
- If the prize is behind Door 3, he cannot open Door 3.

This observation therefore has different probabilities in the three worlds. After updating:

```
P(H₁ | O) = 1/3    P(H₂ | O) = 2/3    P(H₃ | O) = 0
```

The host did not move the prize.

He used his knowledge to eliminate one world selectively.

## Chapter 3｜Why the Same Empty Door Can Mean Different Things

Now replace the host.

This host does not know where the prize is. He simply chooses at random between the unselected Doors 2 and 3.

He happens to open Door 3, and it is empty.

On the surface, you have observed exactly the same thing:

> Door 3 was opened, and it was empty.

But this time, Doors 1 and 2 each have probability `1/2`.

When the prize is behind Door 1 or Door 2, this host has the same probability of opening Door 3. The observation eliminates World 3, but it does not further favor World 1 over World 2.

The same empty door means `1/3` versus `2/3` under the first mechanism, but `1/2` versus `1/2` under the second.

The difference is not the door. It is how the door came to be opened.

A more complete update is therefore not simply:

```
P(H | O)
```

but:

```
P(H | O, M)
```

where `M` is the **observation-generating mechanism**: how the observation was selected, produced, and presented to us.

Evidence includes not only what we saw, but also why this was the particular thing we were allowed to see.

## Chapter 4｜What If You Requested the Door to Be Opened?

Change the rules once more.

This time, no host chooses a safe door for you. You directly instruct a staff member to open Door 3, regardless of whether the prize is behind it.

Fortunately, Door 3 is empty.

Doors 1 and 2 still each have probability `1/2`.

That number is the same as in the random-host case, but the generating process is different.

Which door a random host opens is a behavior within the system. It may depend on his knowledge, rules, or preferences.

By contrast, “force Door 3 open” is an action you impose from outside the system. You are no longer merely observing the value taken by a variable; you are actively setting that value.

Causal inference often expresses the distinction this way:

```
P(Y | X = x)
```

describes how to reason about `Y` after observing `X = x`;

```
P(Y | do(X = x))
```

describes what happens to `Y` after actively setting `X` to `x`.

In the Monty Hall problem, the two mechanisms may happen to produce the same posterior probability. In more complex worlds, however, observing a choice and forcing a choice are often not the same thing.

The question has now moved one step beyond “How should we interpret an observation?”

> If we can decide how the next observation is produced, what should we choose?

## Chapter 5｜Choosing the Next Observation

Expand the game to one hundred doors.

Only one door hides a prize. You choose Door 1. The chance that you chose correctly is `1/100`; the chance that you chose incorrectly is `99/100`.

You can ask a host who knows the answer to open 98 empty doors among the other 99, leaving only your Door 1 and one other door closed.

If the host follows this rule, the remaining door inherits the `99/100` probability that your original choice was wrong. Switching wins with probability `99/100`.

The hundred-door version does not change the principle of the three-door problem. It simply makes one fact more visible:

> The host's value is not that he says more. It is that he eliminates a large number of worlds that were previously possible.

Now add one more condition: inspecting and opening each door costs time or money.

The question is no longer merely whether more information is useful. It becomes:

- How many doors should be inspected?
- Will eliminating more worlds change the final decision?
- Is that change worth its cost?

At this point, action begins to function as an experiment.

A good informational action `a` should make different hypotheses predict different next observations:

```
P(O_next | H₁, a) ≠ P(O_next | H₂, a)
```

If an action produces the same result in every possible world, it has no discriminating power.

If it makes different worlds expose different responses, it is an experiment.

The original linear process:

```
Observation → Reasoning → Conclusion
```

becomes a loop:

```
Observation → Possible Worlds → Belief → Action → New Observation → Update
```

Action is no longer an auxiliary step that comes after reasoning.

It enters reasoning itself.

## Chapter 6｜More Information Is Not Necessarily More Valuable

An observation can change belief without being worth acquiring.

An intelligent agent must distinguish at least three things:

1. **Discriminating power:** how much can it distinguish among the hypotheses that remain alive?
2. **Decision value:** will that distinction improve the next decision?
3. **Acquisition cost:** how much time, resources, and risk are required to obtain it?

Some information eliminates many worlds but does not change the action. Some information can change the action, but costs more than the improvement is worth. Other information comes from an unknown generating mechanism or from one with objectives of its own.

Active intelligence is therefore not simply a matter of “asking more questions.”

It must decide:

> What is most worth knowing next, and through what mechanism should it be learned?

This is the common structure studied by fields such as active learning, Bayesian experimental design, and active sensing.

## Chapter 7｜What Was Still Missing from the Map

The previous essay already connected possible worlds to hidden state, observations, hypotheses, belief, and action under uncertainty.

That map answered a static question:

> Given a set of observations, what must the system represent internally?

But it assumed one thing: the observations had already arrived.

The variations of the Monty Hall problem supply what was missing from that map.

| New structure in the Monty Hall problem | Corresponding concept | Question it adds |
|---|---|---|
| The rule the host uses to open a door | observation model / selection mechanism | Why can the same observation have different meanings? |
| Deciding what information to request from the host | active information acquisition | How should an agent choose its next piece of evidence? |
| Choosing the operation that best distinguishes the remaining worlds | experimental design | Which action best exposes differences between hypotheses? |
| Forcing a specified door to open | intervention / causal inference | Why is observing something different from actively causing it? |
| Paying a cost to open a door | value of information | Is the improvement in decision quality worth the cost of the information? |
| Acting, receiving a new observation, and updating belief | agent–environment loop / POMDP | Why is reasoning a loop rather than a one-way process? |

The structure in the second essay was roughly:

```
O → H → B → A
```

Existing observations constrain possible worlds, produce a belief, and support an action under uncertainty.

This third essay adds:

```
Bₜ → Aₜ → Oₜ₊₁ → Bₜ₊₁
```

Action does not merely consume the existing belief. It also changes how the next observation is produced, thereby changing the belief at the next moment.

Of course, real AI systems do not face three doors. But the information they receive does not appear from nowhere either.

Search and retrieval systems decide which results to return. Users decide which facts to provide. Tool interfaces decide whether to expose a complete state or a simple summary. Another agent may also select information strategically, hoping to induce a particular belief in the recipient.

Therefore:

- Failing to retrieve a counterexample does not mean that no counterexample exists;
- A tool returning “success” does not mean the real objective has been achieved;
- A piece of text appearing in a prompt does not mean it has the authority to change the task's objective.

In the Monty Hall problem, the host's rule determines the meaning of the empty door.

In AI systems, retrieval, ranking, tool policy, user selection, and the strategies of other agents likewise determine the meaning of an observation.

## Chapter 8｜What Else Does an Agent Need Beyond Reasoning?

Longer and more complex reasoning over a fixed context can use the existing information more thoroughly.

But it does not mean that the system can:

- recognize that the existing evidence is no longer sufficient;
- understand the mechanism that produced an observation;
- choose a new question or action with discriminating power;
- distinguish passive observation from active intervention;
- recalibrate belief after the environment's mechanism changes.

Long reasoning improves how the system processes the information it already has.

Active intelligence changes the system's relationship with information.

## Chapter 9｜Where This Line Leads

The first essay asked:

> What information and structure should a system preserve?

That is a question of representation.

The second essay asked:

> When the true state is hidden, which worlds can still be possible?

That is a question of hypotheses and belief.

This essay continues:

> When existing evidence cannot distinguish those worlds, how should the system acquire new evidence?

That is a question of active information acquisition.

The three essays therefore form a continuous path:

```
Representation → Possible Worlds → Belief
→ Information Acquisition → Intervention → New Observation
```

The first essay discusses what a system should preserve. The second asks which worlds the existing evidence allows to remain alive. The third connects these components into a loop that continues to run.

This path branches into many further questions:

- What kind of internal representation deserves to be called a belief?
- How should the value of two observations be compared?
- When the observation model itself is unknown, how should a system infer both the world and the information source?
- When an information provider has objectives of its own, is ordinary Bayesian updating still reliable?
- When actions change the environment and future data, how can a system avoid forming self-confirming beliefs?

These questions lead respectively toward Bayesian decision theory, the Blackwell information order, experimental design, causal inference, dual control, strategic information, and learned belief representation.

But the most important transition has already occurred:

> Thinking is not the endpoint of intelligence.

When existing evidence has said everything it can say, real intelligence begins to decide what the world should be made to answer next.

## Further Reading

- Thomas Bayes, *An Essay towards solving a Problem in the Doctrine of Chances*
- Richard Bellman, *A Markovian Decision Process*
- Leslie Pack Kaelbling, Michael Littman and Anthony Cassandra, *Planning and Acting in Partially Observable Stochastic Domains*
- Dennis Lindley, *On a Measure of the Information Provided by an Experiment*
- Judea Pearl, *Causality*
