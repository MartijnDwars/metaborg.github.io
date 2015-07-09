---
layout: page
title: "Spoofax QL | 6 | Disambiguation"
modified:
excerpt:
tags: []
image:
  feature: 
  credit:  
  creditlink: 
toc: true
share: true
context: spoofax
---

<section id="table-of-contents" class="toc">
  <header> <h3>Overview</h3> </header>
  <div id="drawer" markdown="1">
  *  Auto generated table of contents
  {:toc}
  </div>
</section><!-- /#table-of-contents -->

[[start](/spoofax/tutorials/ql) | 
[previous](/spoofax/tutorials/ql/05-syntax-expressions) | 
[next](/spoofax/tutorials/ql/07-transformation)]


## Exercise 3

Open `exercise3.spt` and `exercise3.ql` in the `ql-exercises` project. Observe that the tests fail. 

## Ambiguity

Why do these tests fail? If you followed the lead from the previous chapter, your expression productions look something like this:

```
context-free syntax
  Expr.Add  = <<Expr> + <Expr>>
  Expr.Sub  = <<Expr> - <Expr>>   
  Expr.Mul  = <<Expr> * <Expr>>
  Expr.Div  = <<Expr> / <Expr>>
```

These define a fine _abstract_ syntax of expressions, which pass the tests in `exercise2.spt`. However, combining these operators without parentheses leads to ambiguity. 

With conventional parser generators this could be a problem at parser generation time, for example, leading to parse table conflicts. SDF3 has no problem with ambiguous grammars. The parser it generates will produce parse trees (technically parse forests) that represent the ambiguity using an ambiguity node.

For example, the expression `x - y * z` will be parsed using the grammar above as

```
amb([ Mul(Sub(Ref("x"), Ref("y")), Ref("z"))
    , Sub(Ref("x"), Mul(Ref("y"), Ref("z")))])
```

In a test you can inspect these ambiguities by hovering your cursor over the text of the test. In a generated editor, you can inspect the abstract syntax term through the `Syntax > Show Abstract Syntax` menu entry.


## Explicit Disambiguation

One approach to disambiguate such ambiguities is by encoding precedence in the grammar productions by using additional non-terminals. For example, the arithmetic expression productions above can be disambiguated as follows:

```
  Expr = Expr2
  
  Expr0.Paren = <(<Expr>)>   
  Expr0.Ref = <<ID>> 
   
  Expr1     = Expr0
  Expr1.Mul = <<Expr1> * <Expr0>>
  Expr1.Div = <<Expr1> / <Expr0>>
  
  Expr2     = Expr1
  Expr2.Add = <<Expr2> + <Expr1>>
  Expr2.Sub = <<Expr2> - <Expr1>>
```

The integer suffix represents the precedence level (lower number is higher precedence). Note how the left associativity of the operators is modeled by making the productions left recursive, but let the right argument refer to the lower level. 

The disadvantage of this approach is that adding an operator requires updating the precedence chain, which requires updating the precedence level at the right places.

Furthermore, the `bracket` annotation that omits a constructor for parentheses in abstract syntax terms only works on productions that use the same non-terminal sort as they define. As a result, an expression such as `(x - y) * z` is parsed as

```
Mul(Paren(Sub(Ref("x"), Ref("y"))), Ref("z"))
```

Of course it is possible to remove these `Paren` constructors using a transformation, but that is tedious.

Finally, pretty-printing terms to text, requires inserting parentheses at the right places in order to guarantee that the resulting textual expression has the same structure. Writing the grammar in this style also requires constructing a parenthesis insertion algorithm manually.

## Declarative Disambiguation

To avoid these issues, SDF3 provides several mechanisms for declarative disambiguation. 

### Associativity

The associativity of binary operators can be specified using the `left`, `right`, or `non-assoc` production annotations. For example, in arithmetic, addition is left-associative, but power is right-associative:

```
context-free syntax
  Expr.Add = <<Expr> + <Expr>> {left}
  Expr.Exp = <<Expr> ^ <Expr>> {right}
```

### Priority

The relative precedence of productions is specified using the `>` relation in a `context-free priorities` section referring to productions using their non-terminal sort and constructor. For example, to define that multiplication has higher priority than addition, you write:

```
context-free priorities
  Expr.Mul > Expr.Add
```

Productions at the same priority level can be defined in a group, and can be assigned a mutual associativity declaration:

```
context-free priorities
  {left: Expr.Mul Expr.Div} > {left: Expr.Add Expr.Sub}
```

### Parenthesize

A side effect of using declarative associativity and priority rules, is that these facts are stated explicitly and can be used for other purposes than 


## Exercise 3 (continued)

Use associativity and priority declarations to disambiguate your expression grammar so that the tests in `exercise3.spt` and `exercise3.ql` succeed.

## Reserved Words

You may still have the tests for `true literal` and `false literal` in `exercise3.spt` failing, since `true` and `false` are parsed as references (identifiers) using the `Expr.Ref` production.

Adding the following productions declares `true` and `false` as keywords representing the `True()` and `False()` terms:

```
context-free syntax
  Expr.True  = <true>
  Expr.False = <false>
```

However, this causes another ambiguity, since `true` can now be parsed as _both_ a keyword _and_ an identifier.

To make these keywords into _reserved_ words, we define _reject_ productions that exclude these words from the valid identifiers:

```
context-free syntax
  ID = "true"  {reject}
  ID = "false" {reject}
```

## Lexical Syntax and Restrictions

We have not considered the _lexical_ syntax of QL, i.e. the syntax of tokens. And that is not necessary when first starting to use Spoofax, since the `Common.sdf3` module that is added to new projects contains the basic syntax for tokens to get started. 

To get an impression, here is the lexical syntax for identifiers from module `Lexical.sdf3`

```
lexical syntax

  ID = [a-zA-Z][a-zA-Z0-9]*
  
lexical restrictions

  ID -/- [a-zA-Z0-9\_]
```

Productions in `lexical syntax` sections are similar to productions in context-free syntax section with some differences:

* The symbols in context-free syntax productions are separated by optional `LAYOUT` (comments and whitespace), the symbols in lexical productions are not.

* Lexical productions are typically not defined using templates, since they need to be specific about whitespace usage.

* Lexical productions use character classes such as `[a-zA-Z0-9]` to define a choice of characters from a set

In addition, lexical disambiguation is often needed and is done using follow restrictions, which realize greedy matching.

See module `syntax/Lexical.sdf3` for more examples


## Next: Transformation

[Next](/spoofax/tutorials/ql/07-transformation), you specify an outline view and a normalizer for QL.



