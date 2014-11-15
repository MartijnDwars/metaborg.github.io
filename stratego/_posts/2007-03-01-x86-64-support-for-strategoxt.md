---
title: x86-64 support for Stratego/XT!
layout: post
context: stratego
modified: 2007-03-01
---

Stratego/XT supports x86-64 processors (in 64-bit mode) from release <a href="http://buildfarm.st.ewi.tudelft.nl/releases/strategoxt/strategoxt-0.17M3pre16744/">0.17M3pre16744</a> (<a href="http://buildfarm.st.ewi.tudelft.nl/releases/strategoxt/strategoxt-unstable-latest/">or later</a>), the sdf2-bundle from release <a href="http://buildfarm.st.ewi.tudelft.nl/releases/meta-environment/sdf2-bundle-2.4pre212034-sqzzbkp3/">2.4pre212034</a> (<a href="http://buildfarm.st.ewi.tudelft.nl/releases/meta-environment/sdf2-bundle-unstable-latest/">or later</a>). The preliminary releases are available from our new <a href="http://buildfarm.st.ewi.tudelft.nl/releases">Nix buildfarm at the TU Delft</a>. The x86-64 support is based on a branch of the ATerm library developed by Eelco Dolstra and Erik Scheffers, and some new 64-bit patches for the sdf2-bundle. The x86-64 bit requirements are completely hidden in the ATerm library and the Auto/XT build system, thus packages based on Stratego/XT should support x86-64 machines out of the box if they do not contain custom native C code. The preliminary releases with 64-bit support will soon be used in the Stratego/XT packages integration build, which currently still refers to the <a href="http://nix.cs.uu.nl/dist/">Nix Buildfarm at Utrecht University</a>. More information about the issues and the specific patches required for x86-64 support is available in a <a href="http://mbravenboer.blogspot.com/2007/03/x86-64-support-for-strategoxt.html">post</a> at <a href="http://mbravenboer.blogspot.com">Subject to Meta Programming</a>.


