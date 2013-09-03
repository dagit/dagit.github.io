---
title: Delimited Continuations and version control: an update
tags: continuations, darcs, imported
---

Last time I presented the idea that version control and delimited continuations
are related. &nbsp;I left off with a question how how to make Darcs fit this
model. &nbsp;I think I understand now what I was missing.

I forgot to think about Darcs operations in terms of the intermediate operations that get performed. &nbsp;In Darcs, everything is based on commuting patches, even merging. &nbsp;Therefore, to see how Darcs fits into this model it's important to think about commuting patches in terms of delimited continuations.

Specifically, I now believe that commuting two patches introduces marks that can be <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">shift</span>ed to later.

I have several ideas for the next steps of this. &nbsp;One is to start modeling toy versions of svn and darcs in Haskell via delimited continuations. &nbsp;After that, I would like to figure out the correspondence between the delimited continuations that I've identified and their data structure reification as zippers. &nbsp;I hope to have more about that later.

Judging by a paper written by Oleg, there should be a natural way to convert the delimited continuation representation into a zipper. &nbsp;Investigating this model might shed light on the Darcs patch model, or even lead to a more concise formalism.
