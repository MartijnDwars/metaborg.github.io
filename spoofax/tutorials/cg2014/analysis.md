# Semantic Analyses

## Objectives

Extend the QL editor with semantic analyses to support the following editor features:

* reference resolution
* name-based content completion
* type errors
* cyclic dependency errors

## Name Analysis

In Spoofax, name bindings are specified in [NaBL](http://metaborg.org/nabl/). NaBL stands for *Name Binding Language* and the acronym is pronounced 'enable'. Name binding is specified in terms of namespaces, binding instances (name declarations), bound instances (name references), scopes and imports.

To start a new name binding specification, open `trans/analysis/names.nab`.

    module analysis/name

    imports include/QL

### Namespaces

You start your name binding specification with a `namespaces` section. In NaBL, a namespace is a collection of names and is not necessarily connected to a specific language concept. Different concepts can contribute names to a single namespace.
QL has two different namespaces for forms and questions:

    namespaces

      Form
      Question

### Names

Once you have defined the namespaces, you can define name bindings rules in a `binding rules` section:

    binding rules

      Form(f, _): defines unique Form f

Each binding rule is of the form `pattern : clause*`, where pattern is a term pattern with variables (e.g., `f`) and wildcards (`_`) and `clause*` is a list of name binding specifications about the language construct that is matched by pattern. For example, the shown rule specifies binding instances of form names. Its pattern matches forms and its `defines` clause states that these forms are definition sites for form names (i.e. they bind form names). These names need to be `unique`, otherwise an error is reported.

You can now define similar binding rules for questions. To specify bound instances of question names, you need a `refers` clause:

    Ref(q): refers to Question q

This clause states that references in expressions are use sites for question names (i.e. they refer to bound question names).

When you save the file and build your project, you will get reference resolution for question names and error messages for duplicate form and question names in your QL editor.

### Scopes

You might recognise that reference resolution works across files in the same Eclipse project. Though this is a nice feature, this is incorrect for QL, where forms scope their questions. You can specify this scoping behaviour in the binding rule for forms:

    Form(f, _):
      defines unique Form f
      scopes Question

## Type Analysis

In Spoofax, type systems are specified in the new metalanguage [TS](http://metaborg.org/ts/). The type system of a language is specified in terms of typing rules and constraints.

To start a new type system specification, open `trans/analysis/types.ts`.

    module analysis/types

    imports include/QL

### Constants

Typing rules specify the types of expressions. In the simplest case, the type of an expression is directly known. An typical example for such expressions are literals. For example, boolean literals are of type `BoolTy`.

    type rules

      True() : BoolTy()
      False(): BoolTy()

### Names and Types

For references to questions, you need to lookup the type of the corresponding question. Types of questions can be specified in the corresponding NaBL rules:

    Question(q, _, t): defines Question q of type t

In TS, you can lookup the type of questions as follows:

    Ref(q): ty
    where definition of q: ty

When you build your project, your typing binding rules become effective. Some of the test cases succeed now and in the QL editor you get type errors and can inspect types by hovering over expressions.

### Expressions

In more complicated expressions, you need to check the types of subexpressions. The general idea is that only well-typed expressions have a type. For example, a logical negation is of type `BoolTy`, when its subexpression is of type `BoolTy`.

    Not(e) : BoolTy()
    where e : BoolTy()
     else error "Expected boolean subexpression” on e

This typing rule checks the type of the subexpression and reports an error on this subexpression, if the check fails. You can now define similar rules for the boolean operators `&&` and `||`. Here you need to combine multiple checks with `and`.

For arithmetic expressions, the checks become slightly more complicated. For example, we can add either two integers or two floats or two amounts of money, but not an integer and an amount of money:

    t@Plus(x, y): ty
    where x : x-ty
      and y : y-ty
      and (x-ty == IntTy()   and y-ty == IntTy()   and IntTy() => ty)
       or (x-ty == FloatTy() and y-ty == FloatTy() and FloatTy() => ty)
       or (x-ty == MoneyTy() and y-ty == MoneyTy() and MoneyTy() => ty)
     else error $[Cannot add [x-ty] and [y-ty]] on t

### Constraints

Some language constructs do not have a type on their own, but might expect a certain type of a subexpression. For example, a conditional group requires a boolean condition. You can specify such additional constraints in TS as follows:

    Conditional(c, _):-
    where c: BoolTy()
     else error "Expected boolean condition” on c

## Dependency Analysis

Not all constraints are based on types. For example, we can have cyclic dependencies of questions. Therefore, NaBL and TS provide means to specify custom properties and constraints over them.

### Custom Properties

Currently, custom properties need to be specified in NaBL, even if these properties are not connected with names. You can define a custom property `dependency` for questions in a `properties` section:

    properties

      dependency of Question: ID

The idea is, to use this property to model the dependencies of expressions. This can be specified similar to the type of expressions in typing rules. You can do this `trans/analysis/dependencies.ts`:

    module dependencies

    imports include/QL

    type rules

      True()  has dependency ()
      False() has dependency ()
      Ref(q)  has dependency q

The boolean literals do not have any dependency and a reference to a question `q` has a dependency on `q`. Expressions with subexpressions need to propagate dependencies from the subexpressions:

    Not(e) has dependency d
    where e has dependency d

    Add(e1, e2) has dependency d
    where e1 has dependency d1
      and e2 has dependency d2
      and d1 union d2 => d

When you build your project, your dependency rules become effective in your QL editor. You can inspect dependencies by hovering over expressions.

### Relations

So far, you have specified the dependencies of an expression. You can model the dependencies between questions as a transitive relation `<depends-on:` in TS:

    relations

      define transitive <depends-on:

This relation is initially empty. You can store entries `<depends-on:` based on patterns:

    type rules

      Conditional(e, [Question(q, _, _)]):-
      where e has dependency d
        and store q <depends-on: d

Finally, you can define a constraint for cyclic dependencies:

    Conditional(e, [Question(q, _, _)]):-
    where e has dependency d
      and not( d <depends-on: q )
     else error "cyclic dependency" on q

Next, you specify an extension to QL in the final part of this session.
