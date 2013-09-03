---
title: Darcs 2 Real-World Push Performance Evaluation
date: 2008-08-21
tags: darcs, performance, imported
---

Thanks to Eric Kow, Duncan Coutts and Ian Lynagh we have some great timing data
for using darcs2 and darcs1 to push patches over ssh.

Eric wrote a script to test three different scenarios of using darcs to push patches:

 * Scenario l1r1: This is a local darcs1 client talking to a remote darcs1 executable.
 * Scenario l1r2: This is a local darcs1 client talking to a remote darcs2 executable.
 * Scenario l2r2: This is a local darcs2 client talking to a remote darcs2 executable.

Next, Duncan and Ian provided us with access to 131 real-world repositories
hosted at [http://code.haskell.org](http://code.haskell.org). We ran the script
to push patches to each repository, this gave us a ton of times. Then in Excel
we crunched these numbers to see that not only is scenario l2r2 no worse than
the other two, it’s actually faster on the time consuming cases!

The one caveat we found is that the minimum start-up time for the first two
scenarios is 1 second and in the last scenario it’s 2 seconds. I’m confident we
can shave off this 1 second difference in the future.

<img height="219" src="http://spreadsheets.google.com/pub?key=pCrZlx9LBLA2GSXzICOaULw&amp;oid=2&amp;output=image" width="600" />

This is a histogram that shows you how the push times distribute, click on it
for a large image. Along the bottom we have how many seconds the push took, and
along the vertical axis we have the number of data points in that range. At a
glance you can see that most repositories take just a few seconds to push. We
can also see that darcs2 is slower on small pushes by about one second. Darcs2
in this chart corresponds to l2r2 and darcs1 corresponds to l1r1.

On a side note, we also tested converting all the repositories to darcs2
repository format and that worked great as well. Converting all the
repositories at once takes less than 20 minutes on my laptop without a single
error. There were a few warnings, but that’s to be expected as potentially
exponential merges are fixed in the new darcs2 format, but darcs emits a
warning when fixing them.

For anyone that wants to see the raw numbers click
[here.](http://files.codersbase.com/darcs-times.htm) The link does work, but
not all web browsers are showing the numbers. Opera and FF3 work on some
platforms and not others.

