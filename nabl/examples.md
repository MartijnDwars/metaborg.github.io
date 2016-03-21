## Examples

In textual software languages, names are used to reference elements like variables, functions, classes, etc. 
Name resolution analyses these names in order to establish references between definition and use sites of elements. 
In this paper, we identify reoccurring patterns for name bindings in programming languages and introduce a declarative metalanguage for the specification of name bindings in terms of namespaces, definition sites, use sites, and scoping rules. 
Based on such declarative name binding specifications, we provide a language-parametric algorithm for static name resolution during compile-time. 
We discuss the integration of the algorithm into the Spoofax language workbench and show how its results can be employed in semantic editor services like error checking, reference resolution, hover help, and content completion. 

### Example Projects

The following are example projects using NaBL for name binding:

* C# NaBL project: <https://github.com/MetaBorgCube/spoofax-csharp>

### Source Code

Our source code can be found on our SVN repository:

* Name Binding Language: <https://github.com/Metaborg/nabl>
* Index Stratego API: <https://svn.strategoxt.org/repos/StrategoXT/spoofax/trunk/spoofax/org.spoofax.interpreter.library.language/src/main/stratego/lib/>
* Index Java Backend: https://svn.strategoxt.org/repos/StrategoXT/spoofax/trunk/spoofax/org.spoofax.interpreter.library.language/src/main/java/org/spoofax/interpreter/library/language/

