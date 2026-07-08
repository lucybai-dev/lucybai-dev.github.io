# Research Parking Lot: Belief, Hidden State, and Information

*Status: exploratory notes. These ideas are intentionally not part of the stable research map yet. They should be promoted, revised, merged, or discarded only after literature review and experiments.*

## Stable background

The broad research interest is how intelligent systems operate under partial observability: how they acquire information, form internal representations, update those representations, and use them for prediction or decision-making.

The current minimal vocabulary is:

- **Observation**: information available to an agent from the environment, memory, tools, retrieval, other agents, or stored data.
- **Hidden state**: an internal latent state used to summarize relevant history and support prediction, inference, or action.
- **Belief**: the interpretation of a hidden state as an agent's current uncertain estimate of what is not directly observed.
- **Information acquisition**: any process that produces further observations. It may be passive or actively influenced by the agent.
- **Constraint / pruning view**: a new observation changes belief by reducing, reweighting, or reorganizing the set of still-compatible explanations.

"Hidden world" remains useful as background intuition, but it is currently too broad to serve as the primary operational object. "Projection" has been removed from the core vocabulary because it has not yet shown independent explanatory or empirical value.

## Parking-lot hypotheses

### H1. Pretraining may learn a prior over possible latent structures

A pretrained language model is directly trained to model text, not the world. Its internal state may therefore be better interpreted as a prior over patterns expressed in human language, or over human belief distributions, rather than as a direct world model.

Questions:

- Under what conditions does language modeling recover stable structure behind text rather than surface token statistics?
- Is the learned prior closer to a world prior, a human-belief prior, or merely a corpus prior?
- Can these possibilities be empirically distinguished?

### H2. Inference may behave like belief update through accumulated constraints

As new observations arrive, a system may not merely append information. It may update an internal state by eliminating, down-weighting, or merging incompatible possibilities.

Questions:

- Do LLM hidden states exhibit progressive concentration or stabilization as constraints accumulate?
- Is this better described by Bayesian updating, constraint propagation, support reduction, energy minimization, or another formalism?
- Can hidden-state changes be linked to which hypotheses remain compatible with the evidence?

### H3. A good hidden state should be stable to observation changes but sensitive to underlying-state changes

A useful belief representation should remain relatively stable when superficial properties of observations change while task-relevant structure remains fixed. It should change when the underlying task-relevant state changes.

Questions:

- Which training objectives produce this asymmetry?
- How should stability be measured across models and architectures?
- Does hidden-state stability predict OOD generalization, planning quality, or calibration?

### H4. OOD failure may result from learning observation patterns instead of stable latent structure

Some systems perform well in training environments because their hidden states encode shortcuts or environment-specific correlations. When the observation distribution changes, these representations fail because they do not preserve the task-relevant structure.

Questions:

- Can shortcut-sensitive and structure-sensitive hidden states be separated experimentally?
- Is stronger latent invariance sufficient for OOD generalization, or only one contributing factor?
- Which failures come from representation, and which come from objectives, data coverage, optimization, or policy learning?

### H5. The value of data may depend more on independent constraints than on raw volume

Two datasets with equal token or sample counts may differ substantially in how many independent restrictions they place on a latent representation. Repetition may add little, while diverse but mutually consistent evidence may sharply reduce uncertainty.

Questions:

- Can "constraint diversity" be defined independently of ordinary data diversity?
- Is it already captured by conditional information, mutual information, effective sample size, identifiability, or Bayesian surprise?
- At fixed data volume, does constraint diversity better predict representation stability or OOD performance?

### H6. Multiple information channels may converge toward compatible belief states

Text, images, tools, memory, interaction, and observations from other agents may encode different evidence about the same task-relevant hidden state. A strong system may integrate them into compatible internal beliefs.

Questions:

- What does cross-channel consistency mean when latent spaces are not aligned?
- Should different channels produce identical states, equivalent states, or merely equivalent predictions and decisions?
- Can state equivalence be formalized through sufficiency, bisimulation, predictive equivalence, or causal abstraction?

### H7. Information acquisition is broader than action

Belief can change through passive observation as well as active intervention. Agent action is only one mechanism that influences future observations.

Questions:

- How should passive observation, search, tool use, questioning, experimentation, and environmental feedback be represented in one framework?
- When does an agent control the information channel, and when is it selected by the environment, rules, other agents, or resource constraints?
- What distinguishes information-seeking action from ordinary task action?

### H8. Observation value is belief-relative

The same observation can strongly change one agent's belief and barely affect another's because they have different priors, memories, knowledge, or current hypotheses.

Questions:

- Is the relevant quantity Bayesian surprise, information gain, KL divergence, likelihood ratio, support reduction, or something else?
- Does the "constraint" language add anything beyond existing information-theoretic quantities?
- Can qualitative world-pruning and probabilistic belief updating be connected without treating them as identical?

## System-building hypotheses

The hypotheses above are mostly explanatory: they ask what hidden states, belief updates, and information acquisition may be doing. The following hypotheses ask whether those ideas can be turned into interventions that improve a system.

### S1. Explicitly training hidden-state stability may improve transfer

If stable hidden states are useful rather than merely correlated with good performance, then directly encouraging equivalence across observations that share the same task-relevant state should improve OOD generalization, calibration, or planning.

This should be tested against simpler explanations such as ordinary augmentation, larger datasets, stronger regularization, or increased model capacity.

### S2. Training objectives determine which latent structure becomes usable

Different objectives may produce hidden states with systematically different properties. Next-token prediction, reconstruction, contrastive learning, latent-dynamics prediction, belief reconstruction, and planning objectives may differ in how well they preserve task-relevant structure and uncertainty.

A useful result would not only rank objectives, but explain which hidden-state properties each objective induces and under what conditions.

### S3. Information-acquisition policies can be optimized separately from task policies

A system may benefit from distinguishing actions chosen to complete a task from actions chosen to reduce uncertainty. Search, tool use, questioning, inspection, and experimentation could be selected according to their expected effect on belief rather than only immediate reward.

The key comparison is whether a separate information-acquisition objective improves sample efficiency, decision quality, or robustness relative to a single undifferentiated policy.

### S4. Belief update may require an explicit update mechanism rather than passive context accumulation

Appending more observations to context does not guarantee that a model consistently revises its internal state. Architectures or training procedures that explicitly preserve, revise, reactivate, or retract beliefs may outperform systems that rely only on ordinary sequence processing.

Candidate comparisons include recurrent belief states, external memory, structured latent variables, factorized hypotheses, state-space updates, and unstructured context windows.

### S5. The value of a new observation may be predicted from its expected belief change

If observation value is belief-relative, an agent may be able to estimate in advance which query, tool call, experiment, or data source is likely to produce the most useful update.

This hypothesis connects belief representation to practical acquisition: a better belief state should make it easier to choose the next observation efficiently.

## Candidate research questions

These are candidate questions, not yet a fixed agenda:

1. **What properties make a learned latent state a belief state rather than an observation embedding?**
2. **How do sequential observations reshape hidden states under partial observability?**
3. **Do accumulated independent constraints produce more stable and transferable representations?**
4. **Which objectives encourage hidden states to encode task-relevant latent structure instead of observation-specific shortcuts?**
5. **Can equivalent beliefs emerge across different models, architectures, modalities, or information channels?**
6. **How should information acquisition be modeled when control over the observation process is shared between agent and environment?**
7. **Can hidden-state stability be deliberately improved, and does doing so improve OOD performance?**
8. **Which belief-update mechanisms outperform passive context accumulation?**
9. **Can expected belief change guide more efficient information acquisition?**

## Candidate experimental directions

### Experiment A: Same latent situation, different observations

Construct multiple observation sequences generated from the same underlying task state. Compare whether internal representations converge, remain predictively equivalent, or support the same decisions.

### Experiment B: Sequential constraint accumulation

Reveal evidence one piece at a time. Measure how hidden states, predictions, calibration, and sets of compatible hypotheses change after each observation.

### Experiment C: Constraint diversity versus data volume

Hold sample count or token count fixed while varying redundancy, viewpoint diversity, causal variation, and independence of evidence. Test which factor best predicts robustness and OOD performance.

### Experiment D: Shortcut versus structure

Create environments where a shortcut is predictive during training but breaks at test time. Compare models trained with objectives emphasizing next-step prediction, contrastive invariance, world modeling, belief reconstruction, or planning.

### Experiment E: Cross-model and cross-architecture comparison

Use the same partially observable task with different model families. Test whether functionally equivalent belief states emerge despite representational differences.

### Experiment F: Passive versus active information acquisition

Compare systems receiving a fixed observation stream with systems allowed to query, search, inspect, or intervene. Measure uncertainty reduction, task success, and the efficiency of acquired evidence.

### Experiment G: Passive context versus explicit belief update

Give systems the same sequential evidence while varying how state is maintained: full-context replay, recurrent latent state, structured memory, explicit hypotheses, or state-space updates. Test consistency, correction after contradictory evidence, long-horizon retention, and computational cost.

### Experiment H: Optimize expected belief change

Allow an agent to select among possible queries or tools. Compare immediate-reward policies, random acquisition, entropy-based acquisition, and learned belief-change predictors.

## Literature map to build

The first literature review should map how existing fields define a good latent state:

- POMDP belief states
- Bayesian filtering and state estimation
- Predictive state representations
- State representation learning
- World models and latent dynamics
- Bisimulation and state abstraction
- Causal representation learning
- Invariant and multi-view representation learning
- OOD generalization and shortcut learning
- Active learning and Bayesian experimental design
- Active inference and information-seeking RL
- Mechanistic interpretability of LLM hidden states
- Memory architectures and continual belief revision
- Information-seeking and uncertainty-aware agent policies

For each paper or line of work, record:

| Field or paper | What is the latent state? | What makes it sufficient? | How is it updated? | What objective learns it? | How is uncertainty represented? | How is OOD tested? | Does action affect information? |
|---|---|---|---|---|---|---|---|

## Ideas deliberately not promoted

These remain useful intuitions but should not be treated as established concepts:

- **Projection / projector** as an independent layer between world and observation.
- **Hidden world as a complete high-dimensional universe or set of world lines.**
- **Pretraining as direct learning of the true world distribution.**
- **Constraint accumulation as a novel mechanism before comparison with Bayesian updating, information gain, identifiability, and existing constraint-based methods.**
- **OOD failure as evidence that a model lacks a correct belief state.** This is only one possible explanation among many.
- **Scaling laws as a consequence of accumulating independent constraints.** This is currently a speculative interpretation, not an established explanation.
- **Hidden-state stability as automatically beneficial.** Stability can also preserve the wrong abstraction or prevent necessary updating.
- **Explicit belief-update machinery as necessarily superior to context-based inference.** This must be established empirically and may depend on the task.

## Promotion rule

An item should move from this parking lot into the stable research map only when it satisfies most of the following:

1. Existing literature has been mapped and competing terminology has been identified.
2. The claim is distinguishable from an established concept rather than merely renamed.
3. It produces a falsifiable prediction.
4. At least one experiment or formal argument supports it.
5. Removing the concept would reduce explanatory or predictive power.
6. Counterexamples and limitations are stated explicitly.
7. For a system-building claim, the proposed intervention improves at least one meaningful outcome against strong baselines.

Until then, the parking lot is allowed to remain inconsistent, incomplete, and provisional.