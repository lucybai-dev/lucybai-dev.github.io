# How Worlds Die

*How do we reason when truth is unobservable?*

> You do not need to know Werewolf to follow this essay. The only rules that matter are these: every player has a hidden role; wolves secretly choose someone to kill at night; the Seer can check a player’s identity; the Guard can protect one player; and during the day, everyone argues and votes while anyone may lie about their role.

## Prologue｜I Thought I Could Find the Truth

When I first started playing Werewolf, I was drawn to the logic of the game.

Some players were telling the truth. Some players were lying.

I thought this was a game about finding logical cracks.

If you were good enough, you could cut through the fog, find the truth, and win.

Then one game made me question that.

## Chapter 1｜A Story That Seemed to Close

In that game, I was a wolf, but I had been claiming to be the Guard.

To make the situation easier to explain, let us first look at the board from an omniscient view.

![Endgame board from the omniscient view](/draft/assets/how-worlds-die-board-en.svg)

Before the game state completely locked against me, I decided to put my story into play first.

I said that the real Seer and the real Guard were both wolves pretending to be good players.

That day, the real Seer checked me and identified me as a wolf.

If anything, that made my story feel more complete: from inside my version of events, a false Seer was naturally accusing the real Guard.

That night, we did not target a special role. We targeted an ordinary villager.

The real Guard protected him, so no one died overnight.

The next day, the real Guard and I both claimed that the same villager had been attacked.

![The next day: two Guard claims name the same kill target](/draft/assets/how-worlds-die-observation-en.svg)

So the fact that no one died overnight now had a reasonable explanation.

If I were the Guard, the villager survived because I had protected him.

At that moment, the story closed.

In the end, I won that game.

But later, during review, I went through the whole game again and realized that one part of the story still could not be explained.

## Chapter 2｜The Problem Was Not One Sentence, But the Whole World

I first assumed that I really was the Guard.

Then the Seer who had identified me as a wolf was lying, and the player counterclaiming Guard was also a wolf.

I kept pushing that world forward.

If it were true, I would be the only good-side special role still alive.

The wolves would already know who I was.

So why did they not target me?

Killing me would have moved them much closer to winning by eliminating the good side’s special roles.

But they did not kill me. They targeted the villager instead.

That was where I stopped.

Everything people had said could still be explained. The fact that no one died overnight could still be explained.

Only the wolves’ night action could not be explained.

So I switched to another assumption.

If the counterclaim was the real Guard, the Seer was real, and I was a wolf, everything suddenly fit.

The wolves knew who the ordinary villager was, so they targeted him.

The real Guard protected him.

The next day, I named the same target and completed my story.

![Starting from the kill target, comparing two worlds](/draft/assets/how-worlds-die-world-tree-en.svg)

That was when I realized that what I had been comparing was not one player’s identity in isolation.

I was comparing **which world could explain everything that had already happened**.

## Chapter 3｜Then I Learned How to Let Worlds Die

After that, when I reviewed a game, I stopped rushing to decide who was who.

I would first lay out all the worlds that could still stand, then push them forward one by one.

That game was like this.

If I were the Guard, why did the wolves not target me?

That world died first.

But I soon realized that a world does not have to die from an immediate contradiction.

Sometimes, if you push it one more step forward, it collapses on its own.

For example, I could also claim that I was the Guard and had protected myself the previous night, which was why no one died.

That explains the previous night.

But the Guard cannot protect the same person on two consecutive nights.

Even if the good side votes out one supposed wolf during the day, the remaining wolf can still kill me at night.

The good side still loses.

If the good side wants to win, then that world is already dead.

It just dies in the future.

![If the good side believes I am the Guard, this world still loses one round later](/draft/assets/how-worlds-die-future-en.svg)

At that moment, I realized that a world does not have to contradict the present.

It can crack in the next round, in the choices it leaves open, in the outcome it leads to, or somewhere further in the future.

Back then, I felt like I had finally found real “reasoning.”

Not exposing one lie.

But laying out all possible worlds and cutting away every impossible one.

If I could keep doing that, then what remained at the end would be the truth.

## Chapter 4｜Why There May Not Be One World Left

For a long time, I thought my reasoning just was not good enough.

I thought that if I kept practicing, then one day only the one true world would remain.

Later, I realized that the problem was not my skill.

The problem was that the goal I had been chasing — **reaching the one true world through reasoning** — might not exist in the first place.

I started noticing more and more endgames like this.

Only a few players were left.

Two worlds could both explain the game.

Neither had an obvious hole.

Neither could be falsified by the observations still available.

They could stay alive at the same time, all the way until the game ended.

I slowly realized that truth is not something we can force into view by reasoning harder.

It is revealed only when the hidden roles are finally shown.

Before then, all we can see are claims, votes, attack targets, checks, and the actions each player leaves behind.

Those observations can rule out some worlds.

But they do not necessarily leave only one behind.

Pruning makes the worlds fewer.

It does not guarantee that only one world will remain.

## Chapter 5｜Truth Is Not the End

At first, I thought that if I exposed the lie, I would get the truth.

Later, I thought that if I kept cutting away the wrong worlds, I would get the truth.

Eventually, I realized that the goal I had been chasing might not exist.

Until the roles are revealed at the end, we may never be able to uniquely determine what truly happened in the game.

What reasoning really does is not prove which world is true.

It proves, again and again, which worlds are no longer possible.

We often want the truth because we believe it will let us make the right decision.

But perhaps what the world really asks of us is not to find the truth first.

It asks us to make a choice while the full truth is still hidden.

In Werewolf, that choice is the vote.

Reasoning cannot guarantee that we find the one true world.

Nor can it guarantee that we make the right decision.

But it can keep eliminating worlds that are no longer possible.

Maybe, in the end, more than one world will still be alive.

But at least the worlds that should have died long ago will no longer guide our choices.

## Chapter 6｜From Possible Worlds to AI Systems

The language of “worlds” is not specific to Werewolf.

Different fields use different terms, but the underlying structure is familiar:

| In the game | In AI, statistics, and decision-making |
|---|---|
| The real roles and the wolves’ actual choices | **Hidden state**: what is true but not directly observed |
| Claims, votes, checks, attacks, and survival | **Observations**: partial evidence generated by that hidden state |
| The different stories still compatible with the evidence | **Hypotheses** or possible latent states |
| Which worlds remain plausible, and by how much | **Belief** or posterior support over hypotheses |
| Removing or weakening worlds after new evidence | **Belief update**, Bayesian conditioning, or hypothesis pruning |
| Voting before every role is known | **Action under uncertainty** |

The mapping is not exact, and the terminology changes across Bayesian inference, control theory, reinforcement learning, and representation learning.

But it points to the same problem: an intelligent system rarely sees the world directly. It receives partial observations, compresses their history into an internal state, and must act before uncertainty disappears.

In a classical partially observable model, that internal uncertainty can be represented explicitly as a **belief state**: a probability distribution over possible hidden states.

Modern neural systems often do not maintain a readable table of possible worlds. They compress the observation history into a latent representation.

The important question is therefore not whether a model contains a module literally named “belief.”

It is whether its internal representation can do the work a belief should do:

- preserve the possibilities that still matter;
- revise them when new evidence arrives;
- discard explanations that no longer fit;
- retain uncertainty when the evidence is insufficient;
- support decisions before the truth is fully revealed.

This also connects back to the earlier question of representation: what a system should preserve depends partly on which possible worlds it may later need to distinguish.

But one problem remains.

What happens when several worlds survive because the existing observations simply cannot distinguish them?

Thinking longer about the same evidence does not create a new observation.

At that point, reasoning may need to change the conditions under which the world is observed — by asking a question, running a test, using a tool, or taking an action that makes different worlds respond differently.

That is the question the next Note will explore.