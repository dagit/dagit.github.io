---
title: Picking a GUI library to use with OpenGL
tags: gui, haskell, libraries, opengl, imported
---

OpenGL provides nice real-time graphics primitives, but to get started with OpenGL you have to first get an OpenGL rendering context on the screen.

For years the simplest cross-platform way to do this was by using the GLUT library. &nbsp;GLUT is unattractive for reasons I'll explain towards the end, but it's easy to get started using it.  This tends to make it a nice choice for beginners. &nbsp;These days there are many alternatives to GLUT for those of us who want simplicity and cross-platform compatibility. &nbsp;Here I will focus on Haskell options.

<iframe width='500' height='300' frameborder='0' src='https://spreadsheets.google.com/pub?hl=en&hl=en&key=0AhzGR3A_VvepdHBLTHg1UTdSYnpFOXY5MF9pYXdBbWc&single=true&gid=0&output=html&widget=true'></iframe>

Full spreadsheet view <a href="https://spreadsheets.google.com/pub?hl=en&hl=en&key=0AhzGR3A_VvepdHBLTHg1UTdSYnpFOXY5MF9pYXdBbWc&single=true&gid=0&output=html">here</a>.

The significance of the license columns is that pure Haskell code which has a standard LGPL license forces your binaries to also abide by the LGPL restrictions.  This is due to GHC's cross-module inlining.  On the other hand, if a foreign library has an LGPL license but BSD3 Haskell bindings then this does not apply.  See for example, the SDL bindings.  License entries that read "LGPL with linking exception" mean that using it with Haskell code does not cause your binaries to fall under LGPL restrictions due to the license explicitly granting that exception.

The use of <tt>atexit()</tt> can lead to subtle problems so I've tried to highlight which libraries use it.  Please let me know if you see a mistake in my categorizing the use of <tt>atexit()</tt>.

The requirement of extra libraries is more of a hassle for people who will use cabal-install to get and build your program.  If your project is a popular one with a large audience you will want to create an installer for end-users, in which case you could offer to install the extra libraries when they install your program.

For simple uses, such as learning or demos programs, my current recommendation is <a href="http://hackage.haskell.org/package/GLFW-b">GLFW-b</a>.  <a href="http://hackage.haskell.org/package/SDL">SDL</a> also makes a nice choice, but it requires more from your windows users as they will have to install the libraries separately.  Linux and OSX users will have to install the libraries but they have the option of using a package manager to handle the install.

Gtk2hs and wxHaskell are harder for novices as they come with restrictions to what you can do.  For example, at one point in time gtk2hs programs had to be very careful when using threading.  I don't know if this is still an issue though.

Surprisingly, all of the libraries in my table have good documentation.  Although, in some cases the Haskell bindings themselves do not have documentation. Typically it's not an issue to go by the original library docs.

<a href="http://hackage.haskell.org/package/GLUT">GLUT</a> is attractive, in part because it's well known, it's included in the Haskell Platform, and lots of documentation and examples exist.  Keep in mind, it's technically not free software (although the source is available), it uses <tt>atexit()</tt>, and even though it's included in the Haskell Platform windows users will still need to download glut32.dll and install it in their application build directory.  If you have the urge to use GLUT, try to use freeglut as your implementation.

Based on the above list of offers, you can see that it would be very nice if someone created a library based on the APIs of GLFW, SDL, or GLUT, but did a direct binding to the native APIs of windows, OSX, and linux so that we'd have a "pure" Haskell option that doesn't require extra C libraries.  For this to become a nice solution it would be nice if the Haskell Platform bundled a binding to native OSX APIs.

I hope the above comparisons help you to pick the right GUI library for your next project that uses OpenGL!
