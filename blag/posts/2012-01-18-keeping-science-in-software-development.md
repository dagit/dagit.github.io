---
title: Keeping the "science" in Software Development
tags: correctness, evidence, opinion, proof, imported
---

If you already feel that math is a science you can safely skip the first
section. &nbsp;What follows are my musings on what it means to be "correct",
especially in software. &nbsp;I pose a challenge at the end for practitioners
and researchers.

# Math is science too


According to Wikipedia the <a
href="http://en.wikipedia.org/wiki/Scientific_method">scientific method</a> is:

> The scientific method is a body of techniques for investigating phenomena,
> acquiring new knowledge, or correcting and integrating previous knowledge.
> To be termed scientific, a method of inquiry must be based on empirical and
> measurable evidence subject to specific principles of reasoning.

At first glance it may appear that mathematics doesn't follow the scientific method above. The mathematics that you're most likely familiar with is a collection of definitions and the facts, lemmas and theorems, about those definitions that result from applying logical deduction. &nbsp;You might notice that if you reason that way, then textbook applications of newton's three laws do not follow the scientific method either.

You might argue that in this case the applications of newton's laws is now mathematics and no longer science. &nbsp;I would agree that it is mathematics and science. &nbsp;Let's see if I can convince you.

By the time you see the definitions and corresponding theorems in a math text they have been reduced to facts. &nbsp;The trial and error, as well as correction and integration of previous knowledge, have already happened. &nbsp;The same is true of a textbook that shows you how to apply newton's three laws. &nbsp;I recommend taking a moment to familiarize yourself with <a href="http://www.maa.org/devlin/LockhartsLament.pdf">Lockhart's lament</a>.

Math only seems to not be a science because of our relationship to learning it. &nbsp;Most of us get taught math as facts. &nbsp;If you've taken a proof based course you may have had the experience of searching for proofs. &nbsp;There is a lot of trial and error involved. &nbsp;Mathematics as a field may have already integrated previous knowledge, but if you are taking a course chances are you have not. &nbsp;So you employ trial and error as well as hypothesis refinement to build logical deductions to establish what your intuition, or pen and paper examples, says in true.

The set of assumptions that you choose can make or break a result. &nbsp;Even though mathematical results are timeless, they still bend to the sway of assumptions. &nbsp;This is exactly what happens to empirical scientists too. &nbsp;They think they know something, or have the right model, but later it turns out they reasoned from a flawed assumption and suddenly new data transform their field.

# Evidence vs Proof

If you've studied proofs in different logical settings you may have noticed that what constitutes a proof depends on what you seek to gain from the proof. &nbsp;See for example&nbsp;<a href="http://www.andrew.cmu.edu/user/avigad/Teaching/classical.pdf">classical logic versus constructive logic</a>.

In a court case or a forensics study, people look for evidence or witnesses to give data points in support of a particular position within a larger case (or claim).

Think about that for a minute. &nbsp;A claim is made, then people try to establish it or refute it. &nbsp;Often times, the assertion needs only be accepted or rejected by a "reasonable" skeptic that is willing to accept partial proof. &nbsp;Such a skeptic might be a jury or peers reviewing a publication for acceptance in a journal. &nbsp;Because we used partial proof new information could turn up later that reverses our understanding.

At the other extreme we have mathematical proofs, which are composed of logical statements following from assumptions. &nbsp;Proofs are <i>timeless</i>. &nbsp;If we get our logic right and we're clear about our assumptions then the result will stand forever. &nbsp;I say "forever", but in reality, proofs can be found to be incomplete, insufficiently rigorous, or using the wrong logic at a later date. &nbsp;Sometimes years after the arguments are accepted.

It seems that the difference here isn't in whether the resulting "facts" are immutable but rather the strength with which we intend to make the claim. &nbsp;In the forensics case, we are making a claim to the best of our incomplete knowledge about what happened. &nbsp;In the case of mathematical proof, we are making a claim that should be true forever.

In my opinion, the difference between "evidence" and "proof" exists to quantify the strength of our assertion that the claim or conjecture is true. &nbsp;Claims can roughly be placed on a continuum from weak to strong. &nbsp;Some claims, such as "a human authored this blog post", are weak in nature and need little evidence. &nbsp;Other claims, such as "the author of this blog post can see through walls", is a hard to believe as there is good evidence that humans can't see through walls. To prove the second claim I would hope that you demand really strong "evidence". &nbsp;Perhaps in the form of a demonstration, a scientific model for how I can do it, and validation from peer review.

Due to the continuum for strength of evidence described above, I consider proofs to be a form of evidence. &nbsp;Proofs, in the mathematical sense, being the strongest form of evidence. &nbsp;Claims made by politicians would be on the extreme other end of my continuum.

The strength of the evidence for the claim has practical applications too. &nbsp;I wouldn't engineer a shuttle to the moon based on arguments built by politicians. &nbsp;I would base my engineering on mathematical proofs as getting to the moon requires a high degree of correctness.

So then, what level of evidence is right if I need to be correct?

# Evidence Based Correctness

Now we're really at the heart of the matter. &nbsp;Sometimes it just doesn't matter if we're correct or not, for example when arguing with a spouse. &nbsp;Other times it matters deeply, like when we want to engineer a shuttle to the moon.

While mathematics is obsessed with being unequivocally true, other branches of science have been trying to deepen their understanding while accepting a greater risk of being wrong. &nbsp;This results in a different level of evidence required for progress.

