---
title: Understanding Darcs Commute
tags: darcs, commute, imported
date: 2008-10-24
---

People often want to understand how commute on patches works. Usually we start
by saying:

Given two patches, $A$ and $B$, if $A$ and $B$ commute then: $AB
\longleftrightarrow B’A’$, for some $B’$ and $A’$.

Naturally people ask, “But what is the relationship between $A$ and $A’$ or $B$
and $B’$?” This is a very important question and I’ll provide you with some
insight.

Suppose we have a repository with 2 files, $a$ and $b$. We could then make the
following operations:

```
mv b c
mv a b
mv c a
```

You can think of each operation as a transformation on the ’state’ of your
repository.  Suppose also, that we make an edit to $a$, and an edit to $b$.

Let’s name the above, using $T$ for transformation:

$$T_{bc} = \text{mv b c}$$
$$T_{ab} = \text{mv a b}$$
$$T_{ca} = \text{mv c a}$$
$$T_a = \text{edit a}$$
$$T_b = \text{edit b}$$

You can imagine that if I gave the diff for $T_a$ and the diff for $T_b$ that
you could apply those diffs in either order to your repository and get the same
final ’state’. Meaning, $a$ and $b$ are the same whether you update $a$ first
or $b$ first.

But, suppose instead that I performed $T_{bc}$, $T_{ab}$, and then $T_{ca}$.
This has the effect of swapping $a$ and $b$ by name. Now suppose you applied the
diffs $T_a$ and $T_b$. What would you want the outcome to be?  It turns out,
that it matters which operations were created first. If you created the diffs
$T_a$ and $T_b$ *before* you did the operations of the swap, then you should
expect that after the swap the diff for $T_a$ actually modifies $b$, whereas
$T_b$ should modify $a$. On the other hand, if you created the diffs $T_a$ and
$T_b$ *after* the swap, then you expect $T_a$ to modify $a$ and $T_b$ to modify
$b$.

We have an intuitive idea of ‘context’ now. As in, what is the context that $T_a$
and $T_b$ were created in? Knowing this will tell us how they transform the
repository state.

Intuitively, it seems as though we need to remember the ‘context’ in which $T_a$
and $T_b$ were created. So let’s say that the operations performed up to the
point where $T_a$ is created is the context of $T_a$. In other words, the context
for $T_a$ is sequence of transformations that existed when $T_a$ was created.
Similarly, since $T_a$ is a transformation, creating it results in a new context,
which is the old context plus $T_a$. We could say that $T_b$ has this context.
Going a bit further, it seems like we should talk about how $T_a$ has a
pre-context and it also has a post-context.

For example, if we created $T_a$ before doing the swap, then the pre-context
might include two transformations, one that creates $a$ and another one that
creates $b$. The post-context would then include those two transformations and
$T_a$ itself. If we created $T_a$ after doing the swap, the pre-context and
post-contexts of $T_a$ would include $T_{bc}$, $T_{ab}$ and $T_{ca}$ also.

Now a side note about commutative functions. Consider the function created by
composing $T_a$ and $T_b$, let’s write $T_a \circ T_b$. Recall, that with
function composition parameters start on the right and pass through the
sequence to the left. As discussed in the intro, $T_a \circ T_b$ is equal to
$T_b \circ T_a$. This is because $T_a$ and $T_b$ are independent of each other.
Thus, we would say that the functions $T_a$ and $T_b$ are commutative
functions.  This means, that changing their order of application does not
change the result.

We are saying that:

$$T_a \circ T_b = T_b \circ T_a$$

Because $T_a$ and $T_b$ are commutative it doesn’t matter which order we compose
them. If we restrict our view to just the repository above with only the files
$a$, $b$ and no $c$, then on this restricted set of repository state how do these two
compare?

$$T_b \circ T_a$$
$$T_a \circ T_b \circ (T_{ca} \circ T_{ab} \circ T_{bc})$$

In plain English, the first one edits $a$ and then $b$, the second one swaps
$a$ and $b$, edits $b$ and finally edits $a$.  As far as the mathematics of it
is concerned, the first one will edit $a$ and $b$, while the second one will
have $T_a$ editing a different $a$ than the first one and $T_b$ editing a
different $b$ than the second one.  Going a bit further, let’s say that $T_a$
and $T_b$ were created without any of $T_{bc}$, $T_{ab}$ or $T_{ca}$ in their
context. So we could have two scenarios.

We could, for example, start with $T_b$ and $T_a$, swap their order and then do the
swap of $a$ and $b$ afterwards. That would give us:

$$ T_b \circ T_a $$

and

$$(T_{ca} \circ T_{ab} \circ T_{bc}) \circ T_a \circ T_b$$

Intuitively, it seems like $T_a$ and $T_{bc}$ are commutative functions, eg., $T_{bc} \circ
T_a = T_a . T_{bc}$. So we could rewrite the second one as this:

$$T_{ca} \circ T_{ab} \circ T_a \circ T_{bc} \circ T_b$$

Now, suppose when we commute the function $T_a$ with $T_{ab}$, that we replace $T_a$
with $T_a’$. $T_a’$ is like $T_a$ except that $T_a’$ makes the edits of $T_a$ to $b$
instead of $a$. After all, this results in $T_a’$ editing the correct file after
the rename. Similarly, when we commute $T_b$ with $T_{bc}$, $T_b$ is replaced with $T_b’$
that edits $c$ instead of $b$. When we commute $T_b’$ with $T_{ca}$ we replace $T_b’$ with
$T_b”$ that edits $a$ instead of $c$.

So, the above goes through these steps:

$$T_{ca} \circ T_a’ \circ T_{ab} \circ T_{bc} \circ T_b  \text{(commute $T_a$ to the left) }$$
$$T_a’ \circ T_{ca} \circ T_{ab} \circ T_{bc} \circ T_b  \text{(commute $T_a’$ to the left)}$$
$$T_a’ \circ T_{ca} \circ T_{ab} \circ T_b’ \circ T_{bc} \text{(commute $T_b$ to the left) }$$
$$T_a’ \circ T_{ca} \circ T_b’ \circ T_{ab} \circ T_{bc} \text{(commute $T_b’$ to the left)}$$
$$T_a’ \circ T_b” \circ T_{ca} \circ T_{ab} \circ T_{bc}$$

The last one will then have $T_a’$ and $T_b”$ making edits the same file contents
as $T_a$ and $T_b$ respectively, even though the names of the files were changed by
the swap.

So, if you’ve followed me to this point, then you now have the intuition for
what we mean when two patches $A$ and $B$, commute to $B’$ and $A’$, as $AB
\longleftrightarrow B’A’$.  You can think of a patch as being one of the above
transformations along with the context of the transformation. You might also
notice that commute of patches must be doing something to the context of the
patches.

Patch commute has the potential to update the context and transformation the
patches it swaps OR it could update the context and leave the state
transformations equal to what they were in the input. Patch commute can also
fail, but we’re ignoring that case for the moment.

Thinking back to how we arrived at the need for context, you might notice that
for each context, that is each sequence of operations, we get one unique
repository state. This is a very important property of context. Without it,
context wouldn’t really be useful. Also, notice that the opposite is not true,
repository state does not determine the context. Which makes sense, because
there are lots of operations you can do that get the repository to a particular
state, so given a state how do you know what was done?

The next important property we want for commuting patches is that once two
patches have been commuted, you can commute them again to undo the commutation.
In fact, it turns out the examples above are saying we want contexts to
determine the same state if you commute the patches inside the context (again,
context is a sequence of patches!).

For $R$ to be an equivalence relation, we need three things:

  1. $x$ $R$ $x$, is true for all $x$
  1. if $x$ $R$ $y$ then $y$ $R$ $x$
  1. if $x$ $R$ $y$ and $y$ $R$ $z$ then $x$ $R$ $z$

Here, we replace $x$ $R$ $y$ with “the sequencing, or order, of $x$ can be obtained by
commuting adjacent elements of $y$”. Roughly how to prove each:

either claim that 0 commutes satisfies definition of $R$ or check that commute is
self-inverting relies on self-inverting nature, I think messier but should
still be provable

I’m pretty sure both (2) and (3) could be done with a brute force proof that
considered all the pairings of patch types in their general cases. Start with
all sequences of length 2, then 3 and I think at that point you could make an
inductive argument to hit sequences of length $n$. This would be a lot of work,
and I’m not convinced it could be fully automated.

Why would we want to show the above? Showing that $R$ is a relation would tell us
that sequences of patches are equivalent under commute. Now, combine this with
the idea that context determines the state uniquely and now we know sets of
patches uniquely determine your repository.

