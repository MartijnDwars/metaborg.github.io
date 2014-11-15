---
title: Stack traces on "rewriting failed"
layout: post
context: stratego
modified: 2008-05-24
---

Since late March, the Stratego compiler and auxiliary libraries have 
supported stack traces upon rewriting failed. The following trace is
taken from a typical XTC component that uses =io-wrap=: 

<pre>
./prog: rewriting failed, trace:
        main_0_0
        io_wrap_1_0
        option_wrap_5_0
        lifted144
        input_1_0
        lifted145
        output_1_0
        lifted0
        my_wrap_1_0
        foo_0_0
        bar_0_0
        zap_0_0
</pre>

More details may be found in 
[two](http://journal.boblycat.org/node/2937)
[posts](http://journal.boblycat.org/node/2934) to our planet.

