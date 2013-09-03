---
title: Simple Unit Testing in Haskell
date: 2006-09-01
tags: cabal, haskell, testing, imported
---

**Note: This post is now old and the ideas described here are NOT recommended.
There are much better libraries available now for unit testing and
QuickChecking. This post exists simply for the sake of documenting how I did it
way back when.**

Recently I started using QuickCheck but things were a bit hard to get working
so I’ll help write down what I’ve learned now that it’s working nicely.  I
wanted to store all my tests in one file, say, Tests.hs and only mention them
once in the entire code base. So, once a test is defined I want everything else
to be automated, I don’t want to have to list it for inclusion in a harness or
any of that junk.  Prerequisites

My setup requires Template Haskell (TH) and a Haskell parser which means you’ll
need GHC. You’ll need quickcheck and a desire to test :-) I don’t assume any
knowledge of TH, quickcheck, cabal or parsing haskell but I don’t really
explain them either. If you get lost by my lack of details shoot me an email or
post a comment and I’ll be happy to clarify.  Setup

Template Haskell has a restriction on top level splices that says you cannot
use a function in a splice if the function was defined in the same module as
the splice. I already have a file in my project called “Utility.hs” where I
store my general purpose and misc. functions so this is where I place things to
be used in top level splices. This will make more sense when we get to mkCheck.

When you define a test for QuickCheck the name always begins with `prop_`. Here
is an example test:

``` haskell
-- from Tests.hs
prop_lrotate1 xs = lrotate (length xs) xs == xs
  where types = xs :: [Int]
```

This test says, whenver you rotate a list by its length you better get the
original list back (obviously we are assuming a finite list). This test, also
known as a property in quickcheck terminology, goes into Tests.hs.

In Utility.hs I’ve defined some functions to read over Tests.hs and pull out
the names of any properties. I decided to use a Haskell98 parser just to be
safe, but you could use regular expressions here.

``` haskell
-- From Utility.hs
{- | looks in Tests.hs for functions like prop_foo and returns
  the list.  Requires that Tests.hs be valid Haskell98. -}
tests :: [String]
tests = unsafePerformIO $
  do h <- openFile "src/Tests.hs" ReadMode
     s <- hGetContents h
     case parseModule s of
       (ParseOk (HsModule _ _ _ _ ds)) -> return (map declName (filter isProp ds))
       (ParseFailed loc s')            -> error (s' ++ " " ++ show loc)

{- | checks if function binding name starts with @prop_@ indicating
 that it is a quickcheck property -}
isProp :: HsDecl -> Bool
isProp d@(HsFunBind _) = "prop_" `isPrefixOf` (declName d)
isProp _ = False

{- | takes an HsDecl and returns the name of the declaration -}
declName :: HsDecl -> String
declName (HsFunBind (HsMatch _ (HsIdent name) _ _ _:_)) = name
declName _                                              = undefined
```

Why do I need the `unsafePerformIO`? Well, that’s to get around a little problem
I was having with top level splices. Perhaps if I were a little bit better with
Template Haskell I wouldn’t need it. In this case it’s perfectly fine because
we won’t be changing Tests.hs while we’re compiling the testsuite so the list
of tests will not change while we’re using this function. Now that we have a
list of test names we can build an AST to execute the tests. This is where
Template Haskell comes in.

``` haskell
-- From Utility.hs
mkCheck name = [| putStr (name ++ ": ")
               >> quickCheck $(varE (mkName name)) |]

mkChecks []        = undefined -- if we don't have any tests, then the test suite is undefined right?
mkChecks [name]    = mkCheck name
mkChecks (name:ns) = [| $(mkCheck name) >> $(mkChecks ns) |]
```

You can get fancier if you like, but I simply output the name of the test right
before the status. That way when a test fails it’s easy to see which one.

Next we create a very simple module, Unit.hs, to run the tests for us.

``` haskell
{-# OPTIONS_GHC -fno-warn-unused-imports -no-recomp -fth #-}
module Unit where

import Utility -- our TH functions
import Tests -- our test cases

runTests :: IO ()
runTests = $(mkChecks tests)
```

The GHC options will take a bit of explaining. I’ll get back to why `-no-recomp`
is there when I talk about cabal. The `-fth` is for template haskell, you’ll need
that in Utility.hs also. If you compile with `-Wall -Werror` then
`-fno-warn-unused-imports` keeps GHC from complaining about importing Tests. You
need the import because we splice the test cases in but the unused imports
check doesn’t know about what we’re doing with TH.

Alright, at this point all you need to do build and run your tests is:

    ghc --make Unit.hs -main-is Unit.runTests -o unit

Followed by

    unit

(or use unit.exe if you’re on windows)

Cabal
-----

I went a step further and made things work with Cabal.
To do this, go into Setup.hs and make these changes:

``` haskell
import Distribution.Simple
import System.Cmd
import System.Exit

main = defaultMainWithHooks (defaultUserHooks { runTests = quickCheck } )
  where
  quickCheck _ _ _ _ = do ec <- system $ "ghc --make -odir dist/build -hidir dist/build -idist/build:src src/Unit.hs -main-is Unit.runTests -o unit"
                          case ec of
                            ExitSuccess -> System "unit"
                            _           -> return ec
```

Here I’m assuming you keep your source code in the “src” directory relative to
the .cabal file. Now after you build, you should be able to test with, `runghc
Setup.hs test`. I mentioned I’d talk more about that `-no-recomp` option in
Unit.hs. I noticed that whever I compiled my program then compiled Unit.hs
everything went smoothly but when I compiled Unit.hs in the normal flow of
compiling my program that I would get errors about undefined symbols when I
typed `runghc Setup.hs test`. To force ghc to always rebuild Unit.hs you just
need to add `-no-recomp`. Another option would be system `touch src/Unit.hs`
right before the ghc line in quickCheck above.

Note: The setup described here matches mine as close as possible without some
extra details specific to my project (I have a rule in my cabal file for
building a dll. I didn’t show it here, but I’d be happy to send it to you if
you need such a thing :). So it’s possible I’ve left out something simple like
an import somewhere. So if you try these steps and get stuck let me know.

