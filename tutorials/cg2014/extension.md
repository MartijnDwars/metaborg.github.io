# Extending QL

In case you did not finish your analysis, we provide you a working solution in project `org.spoofax.lang.lwc.ql.analysis`. You can build this project and continue the hands-on session from there.

## Objectives

Add support for integer literals, float literals and money literals to QL. Extend QL’s syntax definition, typing rules, and  code generation to handle these literals.

## Syntax

The `Lexical` module provides a definition for integer literals. You can integrate these directly into expressions. Next, you need to specify the lexical syntax of float literals and integrate this as well into expressions. Your specification or money literals can reuse the definition of float literals.

## Analysis

You need to provide typing rules for the new language constructs. You should also provide a constraint for division by zero. Finally, you need to provide dependency rules for the new language constructs.

## Code Generation

Finally, you need to add rules for translating QL literals into JavaScript literals to `trans/generation/generate-js.str`.
