---
layout: page
title: "Spoofax QL | 3 | Language Interaction"
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
[previous](/spoofax/tutorials/ql/02-language) | 
[next](/spoofax/tutorials/ql/04-syntax-definition)]

Spoofax supports short development cycles. This enables you to develop your syntax definition step by step. In each step, you focus on a single aspect, for example a particular nonterminal symbol or a particular grammar rule. After each step, you can check your progress by building the project. In Eclipse, you build a project by choosing **Build Project** from the **Project** menu or by pressing the corresponding keyboard shortcut. After you built the project, you can test your language definition either by using an editor interactively or by running test cases.

## Using the Editor

We provide some example questionnaires in the `ql-js-example` project. You can test the QL editor in the same Eclipse instance by opening a QL questionnaire. When you later rebuild your project, any open QL editor is updated to the new version you just built.
You can use **Show abstract syntax** in the editor's **Syntax** menu to test your abstract syntax definition interactively. While you change a QL questionnaire in the editor, its corresponding AST is updated automatically. You might notice that the editor will give you an AST even for syntactically incorrect programs. This is because Spoofax editors support syntactic error recovery.

## Test Cases

The Spoofax [SPT](/spt/) testing language supports the definition of 'language unit tests' in `.spt`. For example, the test  

```
test nonempty form [[
  form nonempty {
      question : "Label" boolean        
  }
]] parse to 
   Form("nonempty", [
     Question("question", Label("\"Label\""), BoolTy())
   ])
```

tests the parsing of a QL form. Each test case consists of a name, a code fragment in double square brackets, and a condition which determines what kind of test should be performed and what the expected outcome is. Tests can also test other language aspects, such as reference resolution.

We have defined a series of tests in the `ql-exercises` project (and some more in the `ql-tests` project)

You get instant feedback while editing a test suite. Since you have not yet specified QL’s syntax, all positive test cases fail. You can also run the test cases of a test suite from the **Transform** menu. This will open the *Spoofax Test Runner View* which provides you information about failing and succeeding test cases in a test suite.

## Exercise 0: Getting Ready

* Close the projects `org.spoofax.lang.lwc.ql.analysis`,  `org.spoofax.lang.lwc.ql.full`, and  `org.spoofax.lang.lwc.ql.syntax`. (Via right click on project folder in explorer.)
* Open the project  `org.spoofax.lang.lwc.ql.empty`
* Open `exercise1.spt` and `exercise1.ql` in the `ql-exercises` project.
* Open `syntax/QL.sdf3` (in the `empty` project)
* Build the project

## Next: Syntax Definition

In the [[next](/spoofax/tutorials/ql/04-syntax-definition)] chapter we look at syntax definition.



