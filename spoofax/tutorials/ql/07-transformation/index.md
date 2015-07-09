---
layout: page
title: "Spoofax QL | 7 | Transformation"
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
[previous](/spoofax/tutorials/ql/06-disambiguation) | 
[next](/spoofax/tutorials/ql/08-analysis)]

In case you did not finish your syntax definition, we provide you a working solution in project `org.spoofax.lang.lwc.ql.syntax`. You can open and build this project and continue the hands-on session from there. If you do, close your previous project.

## Objectives

Improve the outline view of the QL editor and add a normalizer.
This will be your first encounter with [Stratego](http://metaborg.org/stratego/), Spoofax’ language for specifying transformations and code generators.

## Outline View

### Rewrite Rules

An outline view can be specified by rewrite rules `to-outline-label` in `editor/QL-Outliner.str`. These rules should rewrite AST nodes to their label in an outline view. For example, the following rule rewrites a form to its name, which will be used as a label.

    rules

      to-outline-label: Form(f, q*) -> f

On the left-hand side, the rule matches a variable declaration. During the match, variable `f` and `q*` are bound to actual terms. On the right-hand side, the rule instantiates a label. During the instantiation, variable `f` is replaced with the term it is bound to. You can extend `to-outline-label` to provide labels for questions.
When you build the project and open a QL questionnaire, you will get an outline of this questionnaire in the outline view. In case you do not see any outline view, you can select it in *Show View* from Eclipse's *Window* menu.

### Annotations

In Stratego, terms can be annotated with additional information. The Spoofax outline view uses annotations to determine the icon of a node. You can specify the icon to use in an annotation.

    to-outline-label: Form(f, q*) -> f{"icons/file_obj.gif"}

## Normalization

To ease analyses and code generation, you should transform QL questionnaires into a normal form, where a form consists only of conditionals with a single, unconditional question inside.
For example, consider the following questionnaire and its normal form.

    form Box1HouseOwning {
      hasSoldHouse: “Did you sell a house in 2010?” boolean
      hasBoughtHouse: “Did you by a house in 2010?” boolean
      hasMaintLoan: “Did you enter a loan for maintenance/reconstruction?” boolean

      if (hasSoldHouse) {
        sellingPrice: “Price the house was sold for:” money
        privateDebt: “Private debts for the sold house:” money
        valueResidue: “Value residue:” money (sellingPrice - privateDebt)
      }
    }  

*****

    form Box1HouseOwning {
      if (true) {
        hasSoldHouse : "Did you sell a house in 2010?" boolean
      }

      if (true) {
        hasBoughtHouse : "Did you by a house in 2010?" boolean
      }

      if (true) {
        hasMaintLoan : "Did you enter a loan for maintenance/reconstruction?" boolean
      }

      if (hasSoldHouse) {
        sellingPrice : "Price the house was sold for:" money
      }

      if (hasSoldHouse) {
        privateDebt : "Private debts for the sold house:" money
      }

      if (hasSoldHouse) {
        valueResidue : "Value residue:" money ( sellingPrice - privateDebt )
      }
    }

The transformation is specified in `trans/analysis/normalize.str`.  This file contains already rules for normalizing forms and lists of questions. You should add rules for normalizing  single questions. These rules take the form

    normalize-question: pattern -> pattern’

where `pattern` matches a single question and `pattern’` specifies the normalized question. You need three rules:

1. Add a condition to unconditional questions.
2. Add a condition to unconditional computed questions.
3. Combine conditions of nested conditionals.

Next, you should add a rule for breaking a conditional with multiple questions into multiple conditionals. This rule should have the form

    normalize-group: [ pattern | qs ] -> [ pattern’, pattern’’ | qs]

where `pattern` matches a conditional with at least two questions, while `pattern’` and `pattern’’` specify the two new conditionals.

When you build your project, you can test your normalization by selecting **Show normalized** from the **Syntax** menu of the QL editor.

Next, you specify semantic analyses for QL.
