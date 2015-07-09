---
layout: page
title: "Spoofax QL | 5 | Syntax of Expressions"
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
[previous](/spoofax/tutorials/ql/04-syntax-definition) | 
[next](/spoofax/tutorials/ql/06-disambiguation)]

In the previous chapter we saw how to assign structure to sentences using SDF3 productions. 

## Conditional Questions and Expressions

The questions in QL can have conditions that determine whether a question should be posed with expressions accessing the values of previous questions. Expressions are also used to display values
based on previous answers. 

```
form Box1HouseOwning {
    hasSoldHouse: "Did you sell a house in 2010?" boolean
    hasBoughtHouse: "Did you buy a house in 2010?" boolean
    hasMaintLoan: "Did you enter a loan for maintenance/reconstruction?" boolean

    if (hasSoldHouse) {
        sellingPrice: "Price the house was sold for:" money
        privateDebt: "Private debts for the sold house:" money
        valueResidue: "Value residue:" money (sellingPrice - privateDebt)
    }
} 
```

Expressions are defined using the same form as productions as we saw before:

```
Expr.Not = <!<Expr>>
Expr.Add = <<Expr> + <Expr>>
```

## Exercise 2: Syntax of Expressions

Add productions to module `syntax/Expressions.sdf3` such that the following tests succeed:

* `exercise2.spt`
* `exercise2.ql` 

[[next](/spoofax/tutorials/ql/06-disambiguation)]