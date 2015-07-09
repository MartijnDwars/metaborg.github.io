---
layout: page
title: "Spoofax QL | 4 | Syntax Definition"
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
[previous](/spoofax/tutorials/ql/03-language-interaction) | 
[next](/spoofax/tutorials/ql/05-syntax-expressions)]

Spoofax language definitions use the [SDF3](http://metaborg.org/sdf3/) Syntax Definition Formalism to define the syntax of a language. From a syntax definition a parser is generated, but also a syntax-aware editor with syntax highlighting, syntax checking, and code folding. 

The `org.spoofax.lang.lwc.ql.empty` project provides a basic project set-up with an incomplete syntax definition that we will complete in stages.

## Modules

An SDF3 definition can be split into modules. The (incomplete) main module for QL is defined in `syntax/QL.sdf3`:

```
module QL

imports Expressions
imports Types
imports Lexical
  
context-free start-symbols Start Expr Type

context-free syntax

  Start.Form = <form empty {}>
```

This module imports lexical syntax definitions for common language elements such as identifiers, integer constants, comments, and layout from `Lexical`. Furthermore, the module imports empty modules for expressions and types, and defines the start symbol and a dummy production for this symbol.

## Productions

An SDF3 production of the form 

    S.C = B

is a definition for the non-terminal sort `S` with constructor `C` and body `B`. The constructor is used to construct the abstract syntax tree node for this production. The right-hand side body of a production can be empty, can consist of a template with zero or more symbols or can consist of a single sort in the case of an injection.


## Syntax of Forms

You can now evolve the dummy production into a definition for an empty form:

    Start.Form = <form empty { }>

The template in the right hand side of a production is delimited by `<` and `>`. Alternatively, square brackets can be used to delimit a template. The template can contain zero or more literal strings or placeholders.

This template consists only of literal strings which are not enclosed within quotation marks. 

When you build the project, you can see that one of the positive test cases now succeeds. Note how the syntax in this test case is highlighted. 

When you open a new QL file, you can use content completion to get a valid form.

You can improve the form template by replacing `empty` with a placeholder `<ID>` for identifiers. The sort `ID` is defined in `Lexical`:

    Start.Form = <form <ID> { }>

Placeholders need to be enclosed within the same delimiters (either `<…>` or `[…]`) as the template. When you build the project, another test case with an empty form but with a different form name succeeds. The new form also influences content completion, which will now give placeholders for form names.


## Syntax of Questions

Placeholders can also have a number of options:

* `<Sort?>`: optional placeholder
* `<Sort*>`: repetition (0…n)
* `<Sort+>`: repetition (1…n)
* `<{Sort “,”}*>`: repetition with separator

You can use this to introduce a placeholder for questions in forms:

```
context-free syntax
  Start.Form        = <form <ID> { <Question*> }>
  Question.Question = <<ID> : <STRING> <Type>>
  Type.BoolTy       = <boolean>
```

Now, you can add additional productions for types `string`, `integer`, `date`, `decimal`, and `money` types.

## Exercise 1

Add productions to `syntax/QL.sdf3` (in the `empty` project) such that the following tests (in the `ql-exercises` project) succeed

* `exercise1.spt`
* `exercise1.ql` 

## Abstract Syntax

After defining the productions for forms and build the project, you get a working syntax-aware editor. Open the form in `exercise1.ql` in Eclipse, which contains:

```
form exercise1 {
  syntax : "Have you defined the syntax for forms?" boolean
  tests: "How many tests in exercise1.spt succeed?" integer
}
```

If you defined the productions correctly, it should parse without errors and the text should be syntax high-lighted. 

To inspect the abstract syntax tree assigned to the text by the parser, select the 'Show Abstract Syntax' entry in the 'Syntax' menu. That should get you a term like this 

```
Form(
  "exercise1"
, [ Question("syntax", Label("\"Have you defined the syntax for forms?\""), BoolTy())
  , Question("tests", Label("\"How many tests in exercise1.spt succeed?\""), IntegerTy())
  ]
)
```

in a new file `exercise1.aterm`. The term structure follows the syntax definition with the term constructors

## Content Completion

The generated editor for the language supports content completion. Control-space in the editor produces a pop-up menu with suggestions for completing the program. 
The templates for content completion are derived from the production templates. Content completion for the `Form` productions produces a single line form, which is not desirable for forms with questions. You can include layout in the template, to improve content completion:

```
Start.Form = <
  form <ID> {
    <{Question “\n\n”}*>
  }
>
```

## Formatting

The layout in templates is also used as input for the generated formatter. In `exercise1.ql` choose 'Format' from the 'Syntax' menu to see how the program is re-formatted. Can you observe how changing the layout in production templates changes the result of formatting?

## Next: Syntax of Expressions

[[next](/spoofax/tutorials/ql/05-syntax-expressions)]
