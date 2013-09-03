---
title: Installing Agda on Windows
tags: agda, windows, imported
---

I was able to get Agda installed and working on windows. The versions involved:

  * Agda (from darcs, it's roughly a 2.3.3 prelease)
  * ghc-7.6.1 32-bit
  * gnu emacs 24.2
  * Windows 7 64-bit
  * Deja Vu font

  1. First, I installed ghc from
     [here](http://www.haskell.org/ghc/download_ghc_7_6_1).
  
  1. The next step is the hardest one. You need a copy of darcs. I already had
     a copy of darcs from a previous ghc install. If you can't find a windows
     binary of darcs that works for you, try these patches:
     [http://lists.osuosl.org/pipermail/darcs-users/2012-December/026733.html](http://lists.osuosl.org/pipermail/darcs-users/2012-December/026733.html)
  
  
  1. Fetching Agda and the standard library is quite slow so I recommend starting that now. The commands are:

         darcs get --lazy http://code.haskell.org/Agda
         darcs get --lazy http://www.cse.chalmers.se/~nad/repos/lib/

  1. Next you'll want to get a recent copy of emacs. I installed emacs 24.2
     because it has a nice built-in package manager to make it easier to install
     extensions. If you use emacs 24.2 you'll need to patch your Agda installation.
     You can get emacs from here:
     [http://ftp.gnu.org/gnu/emacs/windows/](http://ftp.gnu.org/gnu/emacs/windows/)
  
     Just expand that archive somewhere and add runemacs to your start menu. We'll
     continue with the configuration of emacs in a later step.

  1. Finding a good monospace font with unicode glyphs is not easy and I
     recommend Deja Vu. It's probably missing some glyphs but I haven't run into any
     yet. You can get it here: [http://dejavu-fonts.org/wiki/Download](http://dejavu-fonts.org/wiki/Download)

     I expanded the tarball using 7-zip and then copied the font files into the
     font folder in Windows. You can get there by going to Start --\> Control
     Panel --\> Fonts.

  1. **Note: This step is no longer needed with the darcs version of Agda.**
     Once the download of Agda finishes, go into that directory for the build.
     I created a patch on this ticket that fixes unicode support. If that hasn't
     been applied yet, you'll need to download the patch and apply it yourself. You
     can try to apply it in either case and darcs will simply ignore it if it's
     there.

         darcs apply <downloaded patch>

  1. To start the build I recommend using cabal-dev, and the command would be:

         cabal-dev install --prefix=$HOME/AppData/Roaming/cabal/
     
     If you use plain cabal, it would simply be this command:

         cabal install
     
     Once the build finishes, it's time to configure emacs. Use the agda-mode
     command to start the configuration:

         agda-mode setup

  1. Fire up emacs and open ~/.emacs. Mine looks something like this:

     <script src="https://gist.github.com/4574857.js"></script>

     You can see that I've configured my default font to be Deja Vu. You can set
     this via the menus, or just copy what I have. I've also got a bit in there that
     makes sure everything is setup to use utf8. I think that is optional. You'll
     want to make sure you edit line 8 so that agda can find the standard library
     code that you downloaded.

  1. Reload your ~/.emacs config (I find it easiest to just restart emacs). Put
     this sample code into Foo.agda and try to load it with C-c C-l. If everything
     is working it should produce an error message in the AgdaInfo buffer with
     correct looking unicode symbols:
     <script src="https://gist.github.com/4574913.js"></script>

If you get stuck try looking around the Agda wiki for pointers. I've found
that most of the documentation is hanging off this page:
[http://wiki.portal.chalmers.se/agda/agda.php?n=Main.Documentation](http://wiki.portal.chalmers.se/agda/agda.php?n=Main.Documentation)
