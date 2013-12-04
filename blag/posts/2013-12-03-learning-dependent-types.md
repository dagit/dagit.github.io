---
title: Learning Dependent Types
tags: math, types, agda, advice
---

*Note: I originally wrote this explanation to help someone in our local
Function Programming group get started. Later, I realized that it may be
interested to a wider audience.*

Learning dependent types is not easy! In my experience it requires a lot
self-directed study, googling, feeling dumb, and figuring out what questions to
ask. Despite all that, it is quite rewarding and worthwhile.

I started down that road some time back and I pick it up and put it down
sporadically, so I haven't made as much progress as I would like. Regular,
diligent study is the only way to make it stick.

**A bit of caution:** Something to watch out for is what Bob Harper refers to as
playing a game with the type checker, mentions it in the first lecture of his
HoTT class that I linked below. He's referring to situations where you try
everything you can until it finally type checks and then you move on. I think
the solution here is to not simply move on once it finally type checks. Instead
you have to pick it apart and understand the solution. This is what I meant
when I said you have to figure out what questions to ask.

Another aside, Type theory and lambda calculus go together very well. It really
pays off to study both. Rewriting is another related topic that is useful to
understand, although you don't have to become an expert. Of course, having a
basic understanding of the curry-howard correspondence and logic is beneficial
as well.

The path I would suggest to people is roughly as follows, based on the
philosophy that we learn best by doing and that it's better to start with
concrete and move towards abstract in due time:

  1. Learn (or review) the untyped and simply typed lambda calculi. For
     example, make an interpreter in your favorite language. There are many
     great books and websites on the topic. Pick a few and get coding.

  1. Read (and implement) [Typing Haskell in
     Haskell](http://web.cecs.pdx.edu/~mpj/thih/). This will give you a sense
     of how type schema (or parametric polymorphism) change type checking and
     type inference (compared to simply typed). Make an effort to understand
     unification best you can. It will be very important when you get to
     dependently typed languages. If you can, think about the type equations
     that would be generated and how to solve them.

  1. Implement the toy dependently typed language, [LambdaPi](http://www.andres-loeh.de/LambdaPi/). You may find
     these resources handy as well:

     * [http://math.andrej.com/2012/11/08/how-to-implement-dependent-type-theory-i/](http://math.andrej.com/2012/11/08/how-to-implement-dependent-type-theory-i/)
     * [http://www.cis.upenn.edu/~bcpierce/attapl/](http://www.cis.upenn.edu/~bcpierce/attapl/)
     * [http://www.cs.kent.ac.uk/people/staff/sjt/TTFP/](http://www.cs.kent.ac.uk/people/staff/sjt/TTFP/)

  1. Start to tackle the [Agda
     tutorial](http://www.cse.chalmers.se/~ulfn/papers/afp08/tutorial.pdf).
     Chances are you will get stuck.  When that happens, put it down for
     awhile, and try this [tutorial](http://oxij.org/note/BrutalDepTypes/).
     Eventually, pick it up again and review from the beginning and see if you
     can make it further. 

If you make it that far, you'll have a very solid understanding of the basics.
I also recommend reading a bit beyond your comfort zone. On reddit you can find
both [/r/types](http://reddit.com/r/types) and
[/r/dependent_types](http://reddit.com/r/dependent_types) where you can easily
find advanced things outside of your comfort zone :)

I've also found a few classes online where the lectures and problem sets are available:

  * [Computer Aided Formal Reasoning
    (G53CFR,G54CFR)](http://www.cs.nott.ac.uk/~txa/g53cfr/)
  * [Interactive Theorem Proving for Agda
    users](http://www.cs.swan.ac.uk/~csetzer/lectures/intertheo/07/interactiveTheoremProvingForAgdaUsers.html)
  * [Connor McBride's Agda
    course](http://www.cl.cam.ac.uk/~ok259/agda-course-13/)
  * Bob Harper's [HoTT course](http://www.cs.cmu.edu/~rwh/courses/hott/) (maybe
    not all that useful for Agda)

Please let me know if you think I left something out!
