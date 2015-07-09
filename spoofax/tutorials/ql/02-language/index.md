---
layout: page
title: "Spoofax QL | 2 | The QL Language"
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
[previous](/spoofax/tutorials/ql/01-setup) | 
[next](/spoofax/tutorials/ql/03-language-interaction)]

In this tutorial you will develop a language definition for *QL*, a simple DSL for describing form-based questionnaires. Such questionnaires are characterized by conditional entry fields and (spreadsheet-like) dependency-directed computation.
The language and this description originates from the Language Workbench Challenge 2013.

## Example Questionnaire

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

## Syntax

QL consists of questions grouped in a top-level form construct. First, each question identified by a name that at the same time represents the result of the question. In other words the name of a question is also the variable that holds the answer. Second, a question has a label that contains the actual question text presented to the user. Third, every question has a type. Finally, a question can optionally be associated to an expression. This makes the question computed.

A questionnaire consists of a number of questions arranged in sequential and conditional structures. Sequential composition prescribes the order of presentation. Conditional structures associate an enabling condition to a group of questions, in which case these questions should only be presented to the user if and when the condition becomes true. The expression language used in conditions is the same as the expressions used in computed questions.

For expressions we restrict ourselves to booleans (e.g., `&&`, `||` and `!`), comparisons (`<`, `>`, `>=`, `<=`, `!=` and `==`) and basic arithmetic (`+`, `-`, `*` and `/`). The required types are `boolean`, `string`, `integer`, `date` and `decimal` and `money`.

Next, you define the syntax of QL in Spoofax.

## Objectives

Make the *QL* editor syntax-aware and support the following editor features:

* syntax highlighting
* syntax errors
* completion templates
* code folding
