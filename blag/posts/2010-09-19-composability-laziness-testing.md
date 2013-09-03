---
title: Composability, laziness, testing, and proofs (oh my!)
tags: haskell, testing, types, imported
---

**UPDATED:** I forgot to mention types as a reason for composability!

By now you've probably all seen the excellent talk that <a href="http://video2010.scottishrubyconference.com/show_video/11/1">Larry Diehl gave at the Scottish Ruby convention this year</a>.  The talk encourages the audience to think of proofs in languages such as <a href="http://en.wikipedia.org/wiki/Agda_(theorem_prover)">Agda</a> as composable unit tests.

If you're an experienced Haskeller then I won't have to explain to you why composability is easier in Haskell than other languages, but let me try anyway as it's easy to forget why composability is so important.

<ol>
<li>Haskell is pure so we get a lot of composability just from knowing statically the precise set of inputs a function takes.  This means we can easily take a function from one context and use it in another one.  We've removed any hidden or implicit state.  It's all lexically known.</li>
<li>Laziness is another reason why Haskell code is highly composable.  The Haskell prelude is full of composable list functions.  By operating on (potentially) infinite data structures and only computing as much as is needed you can define combinators that glue together rather nicely.  For example, the functions <tt>take</tt> and <tt>iterate</tt> allow you to define loops as infinite sequences while allowing you to only process some finite prefix of the sequence.</li>
<li>Another major reason Haskell is highly composable is higher-order functions.  Functions like <tt>foldr</tt>, <tt>foldl</tt>, and <tt>bracket</tt> demonstrate that you can write the structure once and reuse it with whatever logic you need in the body.</li>
<li>Haskell has rich types that correspond accurately to what a function takes and what it computes, although they are not as precise as we get with dependently typed languages.  These types give us hints about when it's safe to compose things by giving us a bit of machine checkable documentation.  Conversely, when things cannot fit together as written we will get type errors.</li>
</ol>

As you can see, each of these features enhances and enables opportunities for composability which makes us as programmers more efficient, allows us to program at a level closer to our specification, and means we can get things right once and reuse it later.

How does this relate to testing?

First let's review the <a href="http://en.wikipedia.org/wiki/Curry%E2%80%93Howard_correspondence">Curry-Howard correspondence</a>, which says that our types are logical propositions and our programs represent proofs of those propositions.

Unit tests are evidence that our code does what we expect for some finite subset of the inputs.  While they are not proofs that our code works in general they do provide some assurance.  Larry's point is that in some languages, such as Agda, you can write lemmas that are full blown proofs of correctness using the Curry-Howard correspondence.  Furthermore, you can compose these lemmas into theorems.

My question is, can we compose unit tests in Haskell?

It occurs to me that typically with unit tests we don't know what logical proposition the unit test corresponds to.  Often the type is very simple, such as <tt>Int -> Int -> Bool</tt>, or <tt>IO ()</tt>.  I suspect that to make unit tests composable in the same way as lemmas we need to know what logical statement corresponds to each unit test.  We could compose the body of the unit tests, which corresponds to composing their proofs, but that's not really scaleable and essentially amounts to copy&amp;paste programming.

This is where <a href="http://www.haskell.org/haskellwiki/Introduction_to_QuickCheck">QuickCheck</a> comes in.  In QuickCheck we keep the proposition at the value level, but we take a step forward in at least stating the proposition.  We also try to ensure the property for more than manually picked inputs by using a random value generator or clever value enumeration.

Now my question turns into, have you ever considered composing your QuickCheck properties to build bigger properties?  Can we build QuickCheck theorems from our QuickCheck lemmas?

Pushing this a bit more, can we redesign QuickCheck so that the properties are reflected in the types of the properties?  Would this help make QuickCheck properties more composable or would it lead to Haskell type hackery madness?

Does anyone have pointers to work on this?
