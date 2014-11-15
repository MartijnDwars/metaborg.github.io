---
title: Global Variables
layout: post
context: stratego
modified: 2007-04-26
---

Stratego now supports *scoped* global variables. In the context of a dynamic rules section one
can now write
<pre>
    rules( Foo := &lt;compute&gt; )
</pre>
which abbreviates the following commonly used programming pattern:
<pre>
    x := &lt;compute&gt;
    ; rules( Foo : _ -> x )
</pre>
The value bound in the assignment can be retrieved by the application &lt;Foo&gt;. 
The usual scoping features of dynamic rules apply to global variable as well.
For more information see this [blog](http://eelcovisser.org/post/56/global-variables).

