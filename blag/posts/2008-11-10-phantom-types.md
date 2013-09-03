---
title: Phantom Types, Existentials and Controlling Unification -- Part 1
tags: haskell, imported, types
---

A phantom type is a type that has no value associated with it, such as the
following:

~~~haskell
data P phantom = P Int
~~~

Above, the type “phantom” has no value associated with it on the right-hand
side of the equal sign. This means that whenever we construct a value of type P
we also need to give a type for phantom, but because it has no value associated
with it to constrain its type the type system can make it unify with anything.
For example these are all valid:

~~~haskell
P 5 :: P String
P 5 :: P [Int]
P 5 :: P (IO ())
~~~

The reason we care about phantom types is that they allow us to embed extra
bits of information in our types. In this regard, you can think of phantom
types as a tagging system for types. This allows you to, for example, encode a
simpler type system within Haskell’s types. You could use this when making an
evaluator for a language in Haskell.

Now, there is a well known problem with the unification above. The problem is
that P can be made to unify with all kinds of things. So people often use smart
constructors to control the unification. For example, P would be declared in a
module and the data constructor would not be exported from that module and
instead you’d export functions like this:

~~~haskell
mkIntP :: Int -> P Int
mkIntP n = P n

mkStringP :: String -> P String
mkStringP s = P (length s)
~~~

Now you’ve controlled how the unification of the phantom type by not allowing
users of your data type to choose how it unifies.

Suppose we don’t know the full extent of the types that the user wants the
phantom to unify with. Which is to say, there are an unbounded number of types
for which the phantom can unify but you want to give the user of your code a
way to control the unification.

Let’s talk about existentials for a moment. We can give existential types by
using a language extension that allows us to explicitly give a “forall” in the
type. Now the oddity of giving an existential with a universal quantification
is well documented but I won’t discuss it here. You might create a Seal type
like this:

~~~haskell
data Seal a = forall x. Seal (a x)
~~~

Now when we put a value inside a Seal we forget everything we know about the
type x. The only thing we remember about x is that it exists. This means that
when we open up the seal:

~~~haskell
f :: Seal a -> ()
f (Seal a) = ()
~~~

We have to invent a new type for x. Here the type system is smart and creates a
new fresh name, let’s call it an eigenvariable, for x inside the pattern match
of f.

This eigenvariable for x is distinct. The only type it is equal to is itself.
This is because when we put x in the Seal we agreed to forget everything we
ever knew about it--except that it exists.  We could try this:

~~~haskell
g :: Seal a -> a x
g (Seal a) = a
~~~

Here the type system is very smart and complains. The error message is a bit
confusing, but what it is trying to tell us is that we cannot safely let the
eigenvariable for x escape to a higher scope. Letting this happen has
implications I won’t go into.

Now, remember what I was saying about letting phantom types unify and wanting
to control the unification? Well, Seal gives us a way to let the user put
whatever types they want in the phantom type&nbsp;<b>and</b>&nbsp;it gives the
user a way to control how that type unifies!

~~~haskell
mkSealP :: a -> Seal P
mkSealP a = Seal (P a)
~~~

Now the user of our code can make a P with an arbitrary phantom type that we
didn’t have to anticipate with a smart constructor and the user gains back
control over how the phantom type of P will unify. With the current bit of code
it won’t unify with much :)

Now we’ve moved the problem from unifying with too much to not unifying with
anything. Next time I’ll discuss some strategies for recovering information
about x so that you can do something meaningful with that type.

