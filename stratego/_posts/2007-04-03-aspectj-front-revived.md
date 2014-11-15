---
title: AspectJ-front revived, support for Microsoft Windows
layout: post
context: stratego
modified: 2007-04-03
---

The [AspectJ-front](/sdf/aspectj-front/) package has been updated to be easier to install and be more portable. The AspectJ syntax definition of AspectJ-front heavily exercises the SDF parser generator, which used to make it rather difficult to install the package on machines with a limited amount of memory. The new packages includes the compiled parse tables and also the package is more portable, including support for native Microsoft Windows! The package now also provides a library (DLL on Microsoft Windows) for parsing and pretty-printing AspectJ source files.

