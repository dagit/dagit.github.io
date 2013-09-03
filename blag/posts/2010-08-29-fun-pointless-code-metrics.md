---
title: Fun, but pointless, code metrics
tags: c, darcs, git, haskell, metrics, imported
---

I'm not really sure what motivated this, but I just used <a href="http://cloc.sourceforge.net/">cloc</a>&nbsp;to count the lines of code in both the darcs source and the git source. &nbsp;Here are the numbers.  The git source tree: <pre>1951 text files.
    1836 unique files.                                          
     848 files ignored.

http://cloc.sourceforge.net v 1.51  T=15.0 s (72.3 files/s, 20377.3 lines/s)
--------------------------------------------------------------------------------
Language                      files          blank        comment           code
--------------------------------------------------------------------------------
C                               267          15517          13469         100133
Bourne Shell                    589          15127           5508          84826
Perl                             40           3798           3441          23825
Tcl/Tk                           39           1453            375           9762
C/C++ Header                     99           1977           3557           8301
make                             12            413            434           2673
Bourne Again Shell                1            144            110           2165
Lisp                              2            231            170           1779
Python                           13            465            442           1384
ASP.Net                           8            141              0            931
m4                                2             87             21            858
CSS                               2            154             24            710
Javascript                        2            113            319            477
Assembly                          1             26            100             98
XSLT                              7             15             29             77
DOS Batch                         1              0              0              1
--------------------------------------------------------------------------------
SUM:                           1085          39661          27999         238000
--------------------------------------------------------------------------------
</pre>
The darcs source tree: <pre >561 text files.
     549 unique files.                                          
      57 files ignored.

http://cloc.sourceforge.net v 1.51  T=189.0 s (2.7 files/s, 298.0 lines/s)
--------------------------------------------------------------------------------
Language                      files          blank        comment           code
--------------------------------------------------------------------------------
Haskell                         169           4361           7374          27760
Bourne Shell                    300           2071           2869           8333
C                                 7            325            153           1494
HTML                              5             41              4            316
C/C++ Header                     12             92             83            308
Bourne Again Shell                3             51             95            180
Perl                              2             43             36            130
CSS                               1             21              3             79
make                              1             12              6             53
Lisp                              1              5              6             23
--------------------------------------------------------------------------------
SUM:                            501           7022          10629          38676
--------------------------------------------------------------------------------
</pre>
Take those categories with a grain of salt.  For example, the darcs source does not have any lisp files.  It is interesting that git has 200k more lines than darcs.  I'm not sure what that says.  C is far more verbose than Haskell?  Although, that's not really fair because they also have an order of magnitude more shell code.  If you're just comparing C to Haskell it's a factor of about 4.
