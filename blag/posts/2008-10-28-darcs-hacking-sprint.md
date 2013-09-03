---
title: Darcs Hacking Sprint -- Summary from Portland Team
tags: darcs, hackingsprint, haskell, imported, pdx
---

<span class="Apple-style-span" style="border-collapse: collapse; font-family: arial, sans-serif; font-size: 13px;"></span>
The weekend of 24-25 October 2008 was an&nbsp;<strong>International</strong>&nbsp;darcs hacking sprint! The sprint was a lot of fun and we’ll be having more. The sprint provides a very productive atmosphere for hacking.

We had a team in Brighton with posts from&nbsp;<a href="http://koweycode.blogspot.com/2008/10/darcs-hacking-sprints-some-pictures.html" style="color: #2244bb;" target="_blank">Day 1</a>&nbsp;and&nbsp;<a href="http://koweycode.blogspot.com/2008/10/darcs-hacking-sprint-team-brighton-day.html" style="color: #2244bb;" target="_blank">Day 2</a>. We also had team in Paris but I don’t have a link for them.

Here are just some of the highlights from the Portland Team:
<ul>
<li>Adding language pragmas in all files:<ul>
<li>makes the code cleaner when it’s time to drop ghc6.6 support</li>
<li>all required language extensions are now known</li>
<li>makes it easier to check for Haskell’ compatibility</li>
</ul>
</li>
<li>Removed OldFastPackedStrings</li>
<li>Replaced FastPackedStrings api in favor of Data.ByteString api<ul>
<li>lots of small optimizations, less pack/unpack, more standard
ByteString code</li>
<li>removed a fair bit of C code, new code compiles to same or
faster assembly (Don checked)</li>
</ul>
</li>
<li>cabalization:<ul>
<li>no autoconf or make needed</li>
<li>cabal install tested and working on linux / osx, windows testing
soon to follow</li>
<li>builds out of the box w/ 6.8 and 6.10</li>
<li>configure is much faster</li>
</ul>
</li>
<li><a href="http://galois.com/~dons/images/darcs.svg" style="color: #2244bb;" target="_blank">module graph</a>&nbsp;(depends on cabalization)</li>
<li>Duncan improved zlib<ul>
<li>soon to be available on hackage</li>
<li>allows us to replace our own implementation of zlib bindings
with the main stream one</li>
<li>will make building on windows easier</li>
<li>can use lazy bytestrings</li>
</ul>
</li>
</ul>
Here are some random pictures from the Portland Sprint.
Packages we should consider:
<img alt="Packages we should consider" src="http://galois.com/~dons/images/darcs-oct-08/Image003.jpg" />
The TODO list we made on the first day:
<img alt="The TODO list we made on the first day" src="http://galois.com/~dons/images/darcs-oct-08/Image004.jpg" />
Checking on Team Brighton:
<img alt="Checking on Team Brighton" src="http://galois.com/~dons/images/darcs-oct-08/Image006.jpg" />
Duncan and Jason looking at the projector:
<img alt="Duncan and Jason looking at the projector" src="http://galois.com/~dons/images/darcs-oct-08/Image007.jpg" />
Ah, beautiful Portland in fall:
<img alt="Ah, beautiful Portland in fall" src="http://galois.com/~dons/images/darcs-oct-08/Image002.jpg" />
