# Syntax

## Objectives

Make the *QL* editor syntax-aware and support the following editor features:

* syntax highlighting
* syntax errors
* completion templates
* code folding

## Interactive Language Design

Spoofax supports short development cycles. This enables you to develop your syntax definition step by step. In each step, you focus on a single aspect, for example a particular nonterminal symbol or a particular grammar rule. After each step, you can check your progress by building the project. In Eclipse, you build a project by choosing **Build Project** from the **Project** menu or by pressing the corresponding keyboard shortcut. After you built the project, you can test your language definition either by using an editor interactively or by running test cases.

### Using the Editor

We provide some example questionnaires in the `ql-js-example` project. You can test the QL editor in the same Eclipse instance by opening a QL questionnaire. When you later rebuild your project, any open QL editor is updated to the new version you just built.
You can use **Show abstract syntax** in the editor's **Syntax** menu to test your abstract syntax definition interactively. While you change a QL questionnaire in the editor, its corresponding AST is updated automatically. You might notice that the editor will give you an AST even for syntactically incorrect programs. This is because Spoofax editors support syntactic error recovery.

### Test Cases

We provide some example test cases in the `ql-tests` project. In Spoofax, a test suite consists of one or more `.spt` files with positive and negative test cases. You can find syntax test cases in `syntax-forms.spt`, `syntax-types.spt` and `syntax-expressions.spt`. Each test case consists of a name, a code fragment in double square brackets, and a condition which determines what kind of test should be performed and what the expected outcome is.

You get instant feedback while editing a test suite. Since you have not yet specified QL’s syntax, all positive test cases fail. You can also run the test cases of a test suite from the **Transform** menu. This will open the *Spoofax Test Runner View* which provides you information about failing and succeeding test cases in a test suite.

## Syntax Definition

Spoofax’ syntax definition formalism [SDF3](http://metaborg.org/sdf3/) supports modular syntax definitions. The main module is defined in `syntax/QL.sdf3`:

	module QL

	imports

	  Expressions
	  Types
	  Lexical

	context-free start-symbols

		Start Expr Type

	context-free syntax

		Start.Form =

This module imports lexical syntax definitions for common language elements such as identifiers, integer constants, comments, and layout from `Lexical`. Furthermore, the module imports empty modules for expressions and types, and defines the start symbol and a dummy production for this symbol.

Productions consist of a left-hand side and a right-hand side separated by `=`. The left-hand side can be either just a sort or a sort followed by `.` and a constructor name starting with a capital letter.  Constructor names are used in the abstract syntax. The right-hand side of a production can be empty, can consist of a template with zero or more symbols or can consist of a single sort in the case of an injection.

### Forms

You can now evolve the dummy production into a definition for an empty form:

    Start.Form = <form empty { }>

The template in the right hand side of a production is delimited by `<` and `>`. Alternatively, square brackets can be used to delimit a template. The template can contain zero or more literal strings or placeholders.

This template consists only of literal strings which are not enclosed within quotation marks. When you build the project, you can see that one of the positive test cases now succeeds. Note how the syntax in this test case is highlighted. When you open a new QL file, you can use content completion to get a valid form.

You can improve the form template by replacing `empty` with a placeholder `<ID>` for identifiers. The sort `ID` is defined in `Lexical`:

    Start.Form = <form <ID> { }>

Placeholders need to be enclosed within the same delimiters (either `<…>` or `[…]`) as the template. When you build the project, another test case with an empty form but with a different form name succeeds. The new form also influences content completion, which will now give placeholders for form names.

### Questions

Placeholders can also have a number of options:

* `<Sort?>`: optional placeholder
* `<Sort*>`: repetition (0…n)
* `<Sort+>`: repetition (1…n)
* `<{Sort “,”}*>`: repetition with separator

You can use this to introduce a placeholder for questions in forms:

    Start.Form        = <form <ID> { <Question*> }>
    Question.Question = <<ID> : <STRING> <Type>>
    Type.BoolTy       = <boolean>

When you build the project, content completion for forms gives you a single line form, which is not desirable for forms with questions. You can include layout in the template, to improve content completion:

    Start.Form = <
      form <ID> {
        <{Question “\n\n”}*>
      }
    >

Now, you can add additional productions for `string`, `integer`, `date`, `decimal`, and `money` types.

### Expressions

When you add productions for computed questions and conditional structures, you also need to provide productions for expressions. Typically, you will have an ambiguous syntax definition at some point. For example, the following two productions introduce an ambiguity:

    Exp.True = <true>
    Exp.And  = <<Exp> && <Exp>>

The ambiguity manifests in expressions such as `true && true && true`, since `&&` might be left- or right-associative. Another ambiguity arises when you add a production for alternatives:

    Exp.Or  = <<Exp> || <Exp>>

This ambiguity manifests in expressions such as `true || true && true`, since `||` might bind stronger or weaker than `&&`. Spoofax gives warnings on ambiguous constructs. The abstract syntax tree of such constructs includes all alternatives wrapped in a `amb` constructor.

To disambiguate your syntax definition, SDF3 provides  disambiguation constructs. You can specify associativity with attributes `left`, `right`, or `non-assoc` at the end of a production:

    Exp.And  = <<Exp> && <Exp>> {left}

You can specify precedence in a `context-free priorities` section. In this section, you order productions by referring to their sorts and constructors:

    context-free priorities

	    Exp.And > Exp.Or

You can also group productions at the same level and define the associativity of a group:

    context-free priorities

	    {left: Exp.Mul Exp.Div} > {left: Exp.Add Exp.Sub}

Next, you specify an outline view and a normalizer for QL.
