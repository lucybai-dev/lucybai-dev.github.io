# From DeepSeek’s Cache Hit Rate to Representation Learning

*Some notes on representation, structure, and how new structures emerge.*

While looking into DeepSeek recently, one question kept bothering me.

Why is the cache hit rate so high?

Tasks change. Code changes. The topic of conversation changes. And yet, in many cases, the cache hit rate remains surprisingly high.

That question leads naturally to KV cache.

From an engineering perspective, KV cache is about avoiding repeated computation. But if we keep pulling on this thread, a more interesting question comes into view:

> What exactly is KV cache storing?

Or to phrase it differently:

> What information is worth keeping?

## From Cache to Representation

If KV cache stores some kind of intermediate representation, the next question is natural.

Once something has already been turned into vectors and matrices, why not compress it further?

Matrix transformations, dimensionality reduction, and sparse representations are all familiar ideas in machine learning.

The more interesting question is not:

> Can it be compressed?

But rather:

> What should be preserved during compression?

For ordinary file compression, the goal is to reconstruct the original data as faithfully as possible.

But models seem to care less about the original input itself, and more about the parts that remain useful for future reasoning.

In other words, the focus of compression may not be information itself, but structure.

This question is not unique to KV cache.

Many research directions are essentially dealing with similar questions:

What information is redundant?

What information is useful?

What structure is worth preserving?

## From Compression to Abstraction

Is abstraction a special kind of compression?

At first glance, this feels natural.

But if we look closer, things become less straightforward.

ZIP is compression too.

But ZIP does not form concepts.

If we compress an entire book into an archive, the file becomes smaller, but it does not understand the book any better.

So perhaps there are two different kinds of compression.

One is about reconstruction.

The other is about understanding.

From this angle, abstraction looks more like compression that preserves structure.

A large amount of detail is discarded.

But the structures that support understanding, prediction, and reasoning are preserved.

This often shows up when building models.

Information in the real world is almost always highly entangled.

We cannot put everything into a model exactly as it appears in reality.

We have to decide which factors are worth keeping and which can be ignored.

At first, this may look like discarding information. But what is really happening is often not simple deletion. It is reorganization.

Signals that were mixed together are separated.

Some relationships are preserved.

Some details are deliberately ignored.

Other structures are recombined in the process.

This may look like reducing information, but in many cases it is closer to continuous information transformation.

Signals are decomposed.

Signals are recombined.

Relationships between signals are re-encoded.

What eventually enters the model is no longer the raw world itself, but a new representation.

In this sense, abstraction is not merely about compressing information.

It is also about forming representations.

This leads to the next question:

> Is abstraction a process of forming representations?

## From Abstraction to Innovation

The airplane is a classic example.

An airplane is not a bird.

If we tried to copy feathers, bones, and muscles directly, we probably would never have built an airplane.

What was preserved was lift.

The engine did not preserve biological muscle. It preserved propulsion.

Control theory did not preserve anatomy. It preserved stability.

What happened was not imitation.

It was:

Decomposition.

Abstraction.

Recombination.

Lift, propulsion, structure, and control were extracted from the original system and recombined in a new one.

That new system became the airplane.

From this angle, innovation starts to look less mysterious.

Many engineering innovations seem to follow a similar pattern:

First, decompose an existing structure.

Then abstract the important parts.

Finally, recombine them in a new context.

A line appears here that will keep coming back:

> Decompose → Abstract → Recombine

The airplane example leaves behind another question.

Did we discover lift, or did we create the airplane?

In some sense, both happened at the same time.

## From Innovation to Representation Learning

If we bring this line of thought back to machine learning, the question changes again.

What exactly is a neural network doing?

It is easy to first describe it as compression.

But that explanation quickly becomes incomplete.

Many layers are not compressing at all.

Attention is not simply compression.

FFN layers often expand dimensionality.

MoE does the same.

If we understand neural networks as systems that continuously compress information, we quickly run into contradictions.

So the question becomes:

> Are neural networks really compressing?
>
> Or is a better description representation transformation?

Once input enters a network, it is not simply compressed.

It is continuously turned into new representations.

Those representations are transformed and combined into further representations.

This process continues layer by layer.

This reminds me of a problem I often encountered when building models.

Prediction is often not the hardest part.

The hard part is the middle layer.

Signals in the real world are rarely naturally structured.

Markets, user behavior, organizational collaboration, biological systems — most of the time, these are highly entangled.

The real difficulty in modeling is often not choosing the algorithm.

It is deciding:

What should become a variable?

What should become a feature?

What should be preserved?

What should be ignored?

Many prediction problems eventually collapse into representation problems.

The real world does not enter a model directly.

There is always some kind of representation in the middle.

And representation itself is often the hardest part to define.

From this perspective, one of the most successful aspects of deep learning may be that it hands this problem over to optimization.

Instead of defining representations first and learning afterward, representations gradually form under the constraints of the objective.

## From Representation Learning to Interpretability

If neural networks are constantly forming new representations, the next question is:

> What exactly did they learn?

Many research directions live around this question, including representation learning, interpretability, and recent work on internal model representations.

But there is an even more interesting question underneath:

What makes a representation meaningful?

In many cases, we instinctively assume:

> Meaningful = understandable to humans

But models optimize loss.

They do not optimize for human intuition.

A model may discover a representation that solves the task, while that representation does not match the way humans would decompose the problem.

This is part of why interpretability is difficult.

The question is not only:

> What did the model learn?

It is also:

> What is the relationship between the model’s representation and the representations humans are used to?

I remember an example from a Machine Learning class in graduate school.

The professor talked about training a model to recognize horses.

Later, they discovered that the model was not really using the horse itself. It was using a watermark that consistently appeared in the corner of the image.

For the original task, this was clearly a failure.

That was also how I understood it at the time.

But later, someone offered another perspective. I no longer remember the exact details, but I still remember the impact of that moment.

The idea was roughly this:

A representation that is useless for recognizing horses is not necessarily useless for every task.

If the goal were to identify the data source, the setting, or even which stable the image came from, that watermark might become a very useful feature.

At that moment, I realized something.

When we say “the model learned the wrong thing,” there is another question hidden underneath:

> What do we actually want the model to learn?

Does an effective representation necessarily correspond to a real structure?

This is an easy question to overlook.

In many tasks, the model only needs to complete the objective.

It does not need to explain how it did so.

Shortcut learning, spurious correlations, and local optima all point toward the same thing:

A representation can be very effective.

But it may not correspond to the structure we thought the model had learned.

From this perspective, local optima are not only optimization problems.

They can also be representation problems.

The model finds a path that is good enough to complete the task.

But that path may not correspond to the most stable or essential structure in the real world.

If we rely entirely on human-defined representations, the model may never discover new ways of representing things.

If we remove all constraints, it may learn many shortcuts that only work for the current task.

Human representations matter.

But human representations may not be the only possible representations.

Finding a balance between these two sides is one of the long-running problems in representation learning and interpretability.

## From Representation to the World

If representation is how a model makes sense of the world, what happens after representation?

Representation itself is not the destination.

Decision-making is.

From this perspective, many seemingly different research directions begin to appear on the same map.

Bayesian methods focus on updating representations.

Causal inference focuses on relationships between representations.

World models focus on organizing representations.

Reinforcement learning focuses on using representations to make decisions.

These directions look different.

But behind them is the same question:

> What is the relationship between the model’s representation and the structure of the real world?

Prediction asks:

> What will happen next?

Causality asks:

> Why does it happen?

World models ask:

> What is the world itself like?

They are not necessarily opposed to one another.

They describe the same problem at different levels.

## From Discovery to Creation

Looking back, the airplane example never really left.

An airplane is not a bird.

It does not copy the structure of a bird.

But it also does not escape aerodynamics.

It is built on preserving certain structures, and then forming a new structure through recombination.

Representation learning seems to run into a similar problem.

If a model forms a representation that humans never explicitly designed, but that works reliably for a task, what exactly is happening?

Is it discovering a structure that was already there?

Or is it creating a new way of representing things?

This question sounds abstract.

But it appears in many research discussions.

When AlphaFold predicts protein structures, is it merely predicting them, or discovering a structure that was already latent in the system?

When a generative model produces a design that has never existed before, is it creating, or recombining existing structures?

When a neural network forms an internal representation that humans have never named, but that works reliably, we face the same question.

Perhaps discovery and creation are not completely opposed.

Often, creation builds on discovery.

And discovery itself depends on new ways of representing things.

The airplane did not create lift.

But it created a new structure for using lift.

Similarly, a model may not create the world itself.

But it may create a new way of representing the world.

## Back to the Original Question

At the beginning, I only wanted to understand one concrete question:

Why is DeepSeek’s cache hit rate so high?

But following that question led to a different set of questions:

What information is worth preserving?

What structure is worth abstracting?

How are representations formed?

Does an effective representation necessarily correspond to a real structure?

How do new structures emerge?

Looking back, the original question never really disappeared.

It simply kept appearing in different forms.

Perhaps these questions do not have one unified answer.

But they all seem to point in the same direction:

> How should we represent the world?
