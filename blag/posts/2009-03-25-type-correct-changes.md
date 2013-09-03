---
title: Type-Correct Changes -- A Safe Approach to Version Control Implementation
tags: darcs, haskell, imported
---

On March 20th, 2009, I successfully defended my Master’s thesis in Computer
Science.

**Abstract:**

> Ensuring correctness of real-world software applications is a
> challenging task. Testing can be used to find many bugs, but is typically not
> sufficient for proving correctness or even eliminating entire classes of bugs.
> However, formal proof and verification techniques tend to be very heavy weight
> and are simply not available for day to day use in many common programming
> environments.
> 
> We demonstrate a form of light-weight proof assistant by using the type
> checking features of the programming language Haskell with existing extensions.
> We apply this work to the Open Source version control system Darcs. The
> properties checked by our approach are derived directly from the data model
> used by Darcs. This allows us to eliminate entire classes of bugs at compile
> time. We also examine how these techniques improve the quality of the Darcs
> codebase and the challenges that arise when applying these techniques in
> practice.

You can read the [full thesis here](http://files.codersbase.com/thesis.pdf).
The slides from my [presentation are located
here](http://files.codersbase.com/thesistalk.pdf).

The bottom line is that we used Generalized Algebraic Data Types (GADTs) to
enforce proper patch manipulations. In the Darcs implementation, patches are
stored in sequences and rearranging those sequences can only be done is very
specific ways. Our use of GADTs allowed us to express those constraints using
existentially quantified types, phantom types, and witness types. If you’ve
ever wondered how to use GADTs in real-world software, this serves as a very
illustrative example.

