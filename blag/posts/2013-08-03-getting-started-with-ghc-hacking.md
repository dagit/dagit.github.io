---
title: Getting Started With GHC Hacking
tags: haskell
---

In my opinion, GHC may very well be the best documented open source project.
And despite this, lots of folks still ask "How can I get started?" or "Has
anyone written a guide on how to contribute to GHC?" and similar questions.

The goal of this article is two fold:

  * Get the message out that there is documentation for getting started with
    hacking on GHC and contributing back to GHC HQ.
  * Provide a template for boostrapping yourself into GHC hacking.

I assume that as a reader you are motivated, enthusiastic, and you know
Haskell. Recall that we learn best by doing. You have to dive in. So before we go any further:

```
git clone https://github.com/ghc/ghc.git
```

While we wait for that to finish, let's dive in :)

# Drinking from the firehose

GHC has been the subject of many academic papers. These papers are good for
understanding the intuition and motivation behind the source code you'll find
in the GHC repository. The vast majority of the papers about GHC are very
approachable and well written, *if* you have the right background. As such, you
will need to find a way to develop that background and "level up" before the
papers will be useful to you. Read the section below, "Finding your way" for more
information about this.

In addition to research publications (and perhaps, more widely approachable),
the GHC source code is very well commented and the GHC trac has a wealth of
information. Let's try to enumerate all the places you can find out about GHC:

  * [Simon Peyton-Jones' research publications](http://research.microsoft.com/en-us/um/people/simonpj/papers/papers.html)
  * GHC Trac
    * [Tickets](http://ghc.haskell.org/trac/ghc/query?status=new&status=assigned&status=reopened&group=priority&type=bug&order=id&desc=1)
    * [Wiki](http://ghc.haskell.org/trac/ghc/wiki/Commentary)
  * [The excellently commented source code itself](https://github.com/ghc/ghc)
  * [#ghc](http://irc.lc/freenode/ghc/) on freenode
  * GHC mailing lists
    * [ghc-devs](http://www.haskell.org/mailman/listinfo/ghc-devs)
    * [ghc-users](http://www.haskell.org/mailman/listinfo/glasgow-haskell-users)
  * Books
    * ["The Architecture of Open Source Applications"](http://www.aosabook.org/en/ghc.html)
    * ["Implementing functional languages: a tutorial"](http://research.microsoft.com/en-us/um/people/simonpj/papers/pj-lester-book/)

The commentary page of the GHC Trac is specifically design to answer the
question, "How do I get started hacking on GHC?". If there is only one link
that you follow from this post, follow this one:
[http://ghc.haskell.org/trac/ghc/wiki/Commentary](http://ghc.haskell.org/trac/ghc/wiki/Commentary)

# Finding your way

Now for the second half of this post. How to "level up" so that you have
sufficient skills to contribute to GHC in a meaningful way. Here is a list of
things that will help you or anyone else become a GHC contributor. Remeber, you
really can't start too small:

  * Get a local build working
  * Write some code to add a trivial feature (change the ghci banner for example)
  * Build your modified version and see it work
  * Find an expert who is willing to help you (try IRC or the mailing lists)
  * With the help of your expert friend, find an easy bug and fix it

If you get this far, you'll have made it further than most haskellers! At this
point, you'll have the skills and knowledge necessary to fix bugs. Keep in mind
that bug fix contributions are very valuable in the open source world where
core contributors would rather spend their time on new features (or similar
tasks where their deep expertise is required).

If your ambition is to develop true GHC wizardry (eg., understanding the
research publications and the source code deeply) you'll have a long road ahead
of you. A road where continuing to fix bugs and other modest contributions will
accelerate you down the path.

What you'll need to get there is time to play with the ideas in the
publications. For example, I wanted to understand how to make a new backend for
ghc. So I started reading papers about the intermediate representation that GHC
uses and also I started doing the exercises in the book, ["Implementing
functional languages: a
tutorial"](http://research.microsoft.com/en-us/um/people/simonpj/papers/pj-lester-book/).

Read everything you can and implement the ideas (even in a "toy" way) and play
with it. Doing this gives you much deep understanding that just reading. Then
when you go back to the GHC sources much more of it will make intuitive sense
to you because you now understand the principles it is derived from.

# Parting words

Fire up your editor and get to work :)
