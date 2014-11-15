---
title: Testing Tutorial
layout: page
modified: 
excerpt: 
image:
  feature: 
  credit: 
  creditlink: 
context: spt
sideimage: 
---

<section id="table-of-contents" class="toc"> 
  <header> <h3>Overview</h3> </header>
  <div id="drawer" markdown="1">
  *  Auto generated table of contents
  {:toc}
  </div>
</section><!-- /#table-of-contents -->

This short primer shows how to use tests as a basis for language
development with Spoofax. As an example project we create a small
'calculator' language that shows many of the basics of language
definitions.

## A Calculator Language

Our example language supports arithmetic expressions, variables, and
assignments. An example:

    a = 3
    a * 4

Based on this simple language we can write test cases. Doing test-driven
development, these can even drive the development of the language, but
in this document we focus on the tests.

Tests can be written using a `.spt` file. Create one in a Spoofax
project, and press control-space to get a basic test definition. It will
have tests of this form:

    test description [[
      a = 3
      a * 4
    ]] 0 errors

As the example shows, each test can quote a program using `[[`, `[[[` or
`[[[[` brackets. It can also specify a *description* for the test, and a
condition. This test specifies that `0` semantic `errors` are expected.

## Syntax

Example of tests of the syntax:

    test Add [[
      1 + 2
    ]] parse succeeds

    test Abstract syntax (1) [[
      1
    ]]
      parse to Int("1")

    test Abstract syntax (2) [[
      1 * 2
    ]]
      parse to Mul(Int("1"), _)

    test Parentheses [[
      (1 + 2)
    ]]

These tests specify parse success, compare the abstract syntax of the
test input against a pattern, or simply specify that the test should
succeed.

Tests can also use concrete syntax to test operator precedence and
associativity:

    test Multiply and add (1) [[
      1 + 2 * 3
    ]] parse to Add(_, Mul(_, _))

    test Multiply and add (2) [[
      1 + 2 * 3
    ]] parse to [[
      1 + (2 * 3)
    ]]

    test Add and multiply [[
      1 * 2 + 3
    ]] parse to [[
      (1 * 2) + 3
    ]]

    test Add and add [[
      1 + 2 + 3
    ]] parse to [[
      (1 + 2) + 3
    ]]

## Evaluation of expressions

The following tests will `run` a transformation named `calc` to test
evaluation of expressions:

    test Constant [[
      1
    ]] run calc to "1"

    test Add [[
      1 + 1
    ]] run calc to "2"

    test Subexpression [[
      1 + [[2 + 3]]
    ]] run calc to "5"

Note that the last test in this series uses the `[[` ... `]``]` brackets
to make a **selection** in the test. The `calc` transformation is
evaluated for this selection.

## Variables

The following tests exercise the definition of variables:

    test Variable [[
      x
    ]] parse

    test Variable [[
      longname
    ]] parse

    test Assignment [[
      x = 4
    ]] parse

    test Multiple statements [[
       x = 1
       y = 2
    ]] parse to Statements([Assign(_, _), Assign(_, _)])

        Stm*       -> Start {cons("Statements")}
        ID "=" Exp -> Stm {cons("Assign")}
        Exp        -> Stm
        ID         -> Exp {cons("Var")}

    test Evaluate multiple statements [[
      1
      2
    ]] run calc to "2"

      calc:
         Statements(s*) -> last
         where
            s'*  := <map(calc)> s*;
            last := <last> s'*

    test Eval constant [[
      PI
    ]] run calc to "3.14"

      calc:
        Var("PI") -> "3.14"

    test Eval multiple variables [[
      x = 2
      y = x * 2 + x
      y
    ]] run calc to "6"

## Editor features

The following test cases test the editor facilities of the language:

    test Variable unassigned [[
      y
    ]] 1 error /unassigned/

This test succeeds if the input has `1 error` and it matches the regular
expression `/unassigned/` (as variable `y` is unassigned).

    test Multiple assignments to same variable [[
      y = 1
      y = 2
      y
    ]] /multiple/

This test succeeds if there are one or more (error) messages that match
`/multiple/` (as there are *multiple* definitions of `y`).

    test Reference resolving (1) [[
      x = 4
      [[x]]
    ]] resolve

    test Reference resolving (2) [[
      [[x]] = 4
      [[x]]
    ]] resolve #2 to #1

    test Content completion [[
      avariable = 1
      [[a]]
    ]] complete to "avariable"

These test cases test reference resolving and content completion. They
use the `[[` ... `]``]` selection mechanic to select parts of the
program. The first test case specifies that reference resolving should
work for the selection. The second specifies that the second selection
should resolve to the first selection. The last test case specifies that
the selection should provide a content completion option `avariable`.

## Execution

The `Calculang` project generates Java code. The following test case
tests the output that is returned when the program is compiled and
executed:

    test 42 [[
      42
    ]] build generate-result to 42

## Source Code & Paper

The source for our example project is collected in
[before.zip](/spt/calculang/before.zip). The
complete project, with all functionality implemented to make the test
cases succeed is collected in
[after.zip](/spt/calculang/before.zip).

A full description of the testing language can be found in the paper
[Integrated Language Definition Testing. Enabling Test-Driven Language
Development](http://researchr.org/publication/KatsVermaasVisser2011).
Presentation slides also show Spoofax testing in action:
[http://slidesha.re/tYaHQT](http://slidesha.re/tYaHQT)

<iframe src="//www.slideshare.net/slideshow/embed_code/9993006" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/lennartkats/integrated-language-definition-testing-enabling-testdriven-language-development" title="Integrated Language Definition Testing: Enabling Test-Driven Language Development (SPLASH 2012)" target="_blank">Integrated Language Definition Testing: Enabling Test-Driven Language Development (SPLASH 2012)</a> </strong> from <strong><a href="//www.slideshare.net/lennartkats" target="_blank">lennartkats</a></strong> </div>
