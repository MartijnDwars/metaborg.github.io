---
title: "Syntax Definition in SDF"
layout: page
modified: 
excerpt: ""
image:
  feature: 
  credit: 
  creditlink: 
context: sdf3
---

<section id="table-of-contents" class="toc"> 
  <header> <h3>Overview</h3> </header>
  <div id="drawer" markdown="1">
  *  Auto generated table of contents
  {:toc}
  </div>
</section><!-- /#table-of-contents -->


First, basic structure of SDF. Second, how syntax is defined SDF. Third,
examples of lexical and context-free syntax. Fourth, more detailed
coverage of disambigation.


## SDF: The Basics

In this section, we give an overview of the basic constructs of SDF.
After this section, you will now the basic idea of SDF. The next
sections will discuss these constructs more detail.

### Modules 

Before defining some actual syntax, we have to explain the basic
structure of a module. For this, let's take a closer look at the
language constructs that are used in the modules we showed earlier in
[Figure 5.4](tutorial-parsing.html#Fig-Modules "Figure 5.4.  SDF modules for a small language of arithmetic expressions.").

### Example: Basic constructs of SDF

<pre><code>
module Expression 
imports
  Lexical Operators 

exports  
  context-free start-symbol Exp 
  context-free syntax 
    Id          -> Exp {cons("Var")} 
    IntConst    -> Exp {cons("Int")} 
    "(" Exp ")" -> Exp {bracket}

module Operators
exports
  sorts Exp  
  context-free syntax
    Exp "*" Exp -> Exp {left, cons("Times")} 
    Exp "/" Exp -> Exp {left, cons("Div")}
    Exp "%" Exp -> Exp {left, cons("Mod")}
  
    Exp "+" Exp -> Exp {left, cons("Plus")} 
    Exp "-" Exp -> Exp {left, cons("Minus")}

  context-free priorities 
    {left:
      Exp "*" Exp -> Exp
      Exp "/" Exp -> Exp
      Exp "%" Exp -> Exp
    } 
  > {left:
      Exp "+" Exp -> Exp
      Exp "-" Exp -> Exp
    }

module Lexical
exports
  sorts Id IntConst 
  lexical syntax 
    [a-zA-Z]+ -> Id       
    [0-9]+    -> IntConst 
    [\ \t\n]  -> LAYOUT

  lexical restrictions 
    Id -/- [a-zA-Z]
</code></pre>

[Example
6.1](tutorial-sdf.html#ex-basic-module "Example 6.1. Basic constructs of SDF")
shows these modules, highlighting some of the constructs that are
important to know before we dive into the details of defining syntax.

+--------------------------------------+--------------------------------------+
| [![1](images/callouts/1.png)](#ex-ba | Modules have a <span                 |
| sic-module-name)                     | class="emphasis">*name*</span>,      |
|                                      | which can be a plain identifier,     |
|                                      | such as `Expression` in   |
|                                      | this example. The module must be in  |
|                                      | a file with same name and the        |
|                                      | `.sdf` extension. The     |
|                                      | module name can also be a path, for  |
|                                      | example                              |
|                                      | `java/expressions/Assignment`{.filen |
|                                      | ame}.                                |
|                                      | In this case, the module has to be   |
|                                      | in a file with name                  |
|                                      | `Assignment.sdf`, which   |
|                                      | must be in a directory               |
|                                      | `java/expressions`.       |
+--------------------------------------+--------------------------------------+
| [![2](images/callouts/2.png)](#ex-ba | A module can optionally <span        |
| sic-module-imports)                  | class="emphasis">*import*</span> a   |
|                                      | number of other modules. Multiple    |
|                                      | modules can be imported with a       |
|                                      | single import, as we have done in    |
|                                      | this example, or multiple imports    |
|                                      | can be used. This is not very        |
|                                      | common: usually, modules have just a |
|                                      | single imports declaration.          |
|                                      |                                      |
|                                      | Modules are always imported by their |
|                                      | full name. So, if the name of a      |
|                                      | module is a path, such as            |
|                                      | `java/expressions/Assignment`{.filen |
|                                      | ame},                                |
|                                      | then the module must be imported     |
|                                      | with that full name, even if it is   |
|                                      | in the directory                     |
|                                      | `java/expressions`. If    |
|                                      | the name of the modules are long,    |
|                                      | which is typically the case if you   |
|                                      | use full paths to organize a large   |
|                                      | syntax definition in different       |
|                                      | directories, then the names are      |
|                                      | usually mentioned on separate lines, |
|                                      | but still in a single import.        |
+--------------------------------------+--------------------------------------+
| [![3](images/callouts/3.png)](#ex-ba | Modules contain a number of <span    |
| sic-module-exports)                  | class="emphasis">*sections*</span>,  |
|                                      | of which we now only consider the    |
|                                      | `exports` section. An         |
|                                      | `exports` section defines a   |
|                                      | number of syntactic aspects, called  |
|                                      | a <span                              |
|                                      | class="emphasis">*grammar*</span>,   |
|                                      | that will be available to modules    |
|                                      | that import this module. This        |
|                                      | includes syntax, but also            |
|                                      | declarations of sorts, start         |
|                                      | symbols, priorities, restrictions,   |
|                                      | etc.                                 |
|                                      |                                      |
|                                      | A module can also just import other  |
|                                      | modules and not actually define any  |
|                                      | syntactical aspects itself. This is  |
|                                      | typically the case in main modules   |
|                                      | of large syntax definitions, which   |
|                                      | only import modules for names,       |
|                                      | expressions, statements, etc.        |
+--------------------------------------+--------------------------------------+


### Start Symbols


+--------------------------------------+--------------------------------------+
| [![4](images/callouts/4.png)](#ex-ba | Every syntax definition needs to     |
| sic-module-start-symbols)            | define one or more start symbols,    |
|                                      | otherwise not a single input will    |
|                                      | accepted. Start symbols are the      |
|                                      | language constructs that are allowed |
|                                      | at the top-level of a source file.   |
|                                      | For our simple expression language,  |
|                                      | the start symbol is `Exp`.    |
|                                      | For a java syntax definition, the    |
|                                      | start symbol could be                |
|                                      | `CompilationUnit`, which      |
|                                      | consists of a package, import, and   |
|                                      | type declarations.                   |
+--------------------------------------+--------------------------------------+



### 6.1.3. Sorts 


+--------------------------------------+--------------------------------------+
| [![7](images/callouts/7.png)](#ex-ba | Every syntax definition introduces   |
| sic-module-sorts1)                   | names for the syntactical sorts of a |
| [![10](images/callouts/10.png)](#ex- | language, such as `Exp` and   |
| basic-module-sorts2)                 | `Id`. These names can be      |
|                                      | declared in a `sorts`         |
|                                      | declaration. Declaring sorts is      |
|                                      | optional, but the SDF parser         |
|                                      | generator will give a warning if a   |
|                                      | sort that is used somewhere in the   |
|                                      | syntax definition is not declared.   |
|                                      | It is a good habit to declare all    |
|                                      | the sorts, since this makes it       |
|                                      | easier to find possible              |
|                                      | miss-spellings of these names.       |
|                                      |                                      |
|                                      | Note that in SDF syntax definitions  |
|                                      | we do not directly use the           |
|                                      | terminology of terminal and          |
|                                      | non-terminal, since actually only    |
|                                      | single characters are terminals in   |
|                                      | SDF, and almost everything else is a |
|                                      | non-terminal. Lexical and            |
|                                      | context-free sorts are both declared |
|                                      | as sorts.                            |
+--------------------------------------+--------------------------------------+


### 6.1.4. Syntax 


+--------------------------------------+--------------------------------------+
| [![5](images/callouts/5.png)](#ex-ba | The actual syntax is defined in      |
| sic-module-syntax1)                  | `lexical` and                 |
| [![11](images/callouts/11.png)](#ex- | `context-free` syntax. The    |
| basic-module-syntax2)                | lexical syntax defines the syntax of |
|                                      | language constructs like literals,   |
|                                      | comments, whitespace, and            |
|                                      | identifiers, or what is usally       |
|                                      | referred to as terminals. The        |
|                                      | context-free syntax defines the      |
|                                      | syntax of constructs like operators, |
|                                      | statements, or what is usually       |
|                                      | referred to as non-terminals.        |
|                                      |                                      |
|                                      | In other parser generators the       |
|                                      | lexical syntax is often specified in |
|                                      | a separate scanner specification,    |
|                                      | but in SDF these lexical aspects are |
|                                      | integrated in the definition of the  |
|                                      | context-free syntax. We will come    |
|                                      | back to that later when we discuss   |
|                                      | the definition of lexical syntax.    |
+--------------------------------------+--------------------------------------+



+--------------------------------------+--------------------------------------+
| [![6](images/callouts/6.png)](#ex-ba | Productions can have <span           |
| sic-module-attr1)                    | class="emphasis">*attributes*</span> |
| [![8](images/callouts/8.png)](#ex-ba | can have attributes, specified       |
| sic-module-attr2)                    | between curly braces after the       |
|                                      | production. Some of these            |
|                                      | attributes, such as `left`    |
|                                      | have a special meaning for SDF       |
|                                      | itself. Other attributes can specify |
|                                      | information about a production that  |
|                                      | target a different tool, such as     |
|                                      | `bracket` and `cons`,  |
|                                      | which target the tool that implodes  |
|                                      | parse trees to abstract syntax       |
|                                      | trees.                               |
+--------------------------------------+--------------------------------------+


### 6.1.5. Disambiguation 


+--------------------------------------+--------------------------------------+
| [![9](images/callouts/9.png)](#ex-ba | SDF support constructs to define in  |
| sic-module-priorities)               | a declarative way that certain kinds |
| [![12](images/callouts/12.png)](#ex- | of derivations are not allowed, also |
| basic-module-restriction)            | known as disambiguation filters. In  |
|                                      | our example, there two examples of   |
|                                      | this: we define <span                |
|                                      | class="emphasis">*priorities*</span> |
|                                      | of the arithmetic expressions, and   |
|                                      | there is a <span                     |
|                                      | class="emphasis">*lexical            |
|                                      | restriction*</span> that specifies   |
|                                      | that an identifier can never be      |
|                                      | followed by a character that is      |
|                                      | allowed in an identifier. We will    |
|                                      | explain these mechanisms later.      |
+--------------------------------------+--------------------------------------+


6.2. Syntax {.title style="clear: both"}
-----------


### 6.2.1. Lexical and Context-free Syntax 

Usually, parsing is performed in two phases. First, a lexical analysis
phase splits the input in tokens, based on a grammar for the lexical
syntax of the language. This lexical grammar is usually specified by a
set of regular expressions that specifiy the tokens of the language.
Second, a parser based on a grammar for the context-free syntax of the
language performs the syntactic analysis. This approach has several
disadvantages for certain applications, which won't discuss in detail
for now. One of the most important disadvantages is that the combination
of the two grammars is not a complete, declarative definition of the
syntax of the language.

SDF integrates the definition of lexical and context-free syntax in a
single formalism, thus supporting the <span
class="emphasis">*complete*</span> description of the syntax of a
language in a single specification. All syntax, both lexical and
context-free, is defined by <span class="emphasis">*productions*</span>,
respectively in `lexical syntax` and `context-free syntax`
sections. Parsing of languages defined in SDF is implemented by
scannerless generalized-LR parsing, which operates on individual
characters instead of tokens.

**Expressive Power.** Since lexical and context-free syntax are both
defined by productions, there is actually no difference in the
expressive power of the lexical and context-free grammar. Hence, lexical
syntax can be a context-free language, instead of being restricted to a
regular grammar, which is the case when using conventional lexical
analysis tools based on regular expression. For example, this means that
you can define the syntax of nested comments in SDF, which we will
illustrate later. In practice, more important is that it is easier to
define lexical syntax using productions than using regular expressions.

**Layout.** Then, why are there two different sections for defining
syntax? The difference between these two kinds of syntax sections is
that in lexical syntax no layout (typically whitespace and comments) is
allowed between symbols. In contrast, in context-free syntax sections
layout is allowed between the symbols of a production. We will explain
later how layout is defined. The allowance of layout is the only
difference between the two kinds of syntax sections.


### 6.2.2. Productions, Sorts, and Symbols 

In the [Section
5.1](tutorial-parsing.html#section-parsing-concepts "5.1. Concepts: Grammars, Parse Trees, and Abstract Syntax Trees")
we recapped context-free grammars and productions, which have the form
A~0~ -\> A~1~ ... A~n~, where A~0~ is non-terminal and A~1~ ... A~n~ is
a string of terminals and non-terminals. Also, we mentioned earlier that
the distinction between terminals and non-terminals is less useful in
SDF, since only single characters are terminals if the lexical and
context-free syntax are defined in a single formalism. For this reason,
every element of a production, i.e. A~0~ ... A~n~ is called a <span
class="emphasis">*symbol*</span>. So, productions take a list of symbols
and produce another symbol.


### 6.2.3. Symbols and Regular Expressions 

There are two primary symbols:

<div class="variablelist">

<span class="term">Sorts</span>

:   Sorts are names for language specific constructs, such as
    `Exp`, `Id`, and `IntConst`. These names are
    declared using the previously introduced `sorts` declaration
    and defined by productions.

<span class="term">Character Classes</span>

:   A character class is set of characters. Character classes are
    specified by single characters, character ranges, and can be
    combined using set operators, such as complement, difference, union,
    intersection. Examples: `[abc]`, `[a-z]`,
    `[a-zA-Z0-9]`, `~[\n]`. We will discuss character
    classes in more detail in [Section
    6.2.4](tutorial-sdf.html#section-sdf-character-classes "6.2.4. Character Classes").

</div>

Of course, defining an entire language using productions that can
contain only sorts and character classes would be a lot of work. For
example, programming languages usually contain all kinds of list
constructs. Specification of lists with plain context-free grammars
requires several productions for each list construct. SDF provides a
bunch of regular expression operators abbreviating these common
patterns. In the following list, `A` represents a symbol and
`c` a character.

<div class="variablelist">

<span class="term"> `"c0 ... cn"` </span>

:   Literals are strings that must literally occur in the input, such as
    keywords (`if`, `while`, `class`), literals
    (`null`, `true`) and operators (`+`,
    `*`). Literals can be written naturally as, for example,
    `"while"`. Escaping of special characters will be discussed
    in [???]().

<span class="term">`A*`</span>

:   Zero or more symbols `A`. Examples: `Stm*`,
    `[a-zA-Z]*`

<span class="term">`A+`</span>

:   One or more symbols `A`. Examples: `TypeDec+`,
    `[a-zA-Z]+`

<span class="term">`{A0 A1}*`</span>

:   Zero or more symbols `A0` separated by `A1`. Examples:
    `{Exp ","}*`, `{FormalParam ","}*`

<span class="term">`{A0 A1}+`</span>

:   One or more symbols `A0` separated by `A1`. Examples:
    `{Id "."}+`, `{InterfaceType ","}+`

<span class="term">`A?`</span>

:   Optional symbol `A`. Examples: `Expr?`,
    `[fFdD]?`

<span class="term">`A0 | A1`</span>

:   Alternative of symbol `A0` or `A1`. Example:
    `{Expr ","}* | LocalVarDec`

<span class="term">`(A0 ... An)`</span>

:   Sequence of symbols `A0         ... An`.


### 6.2.4. Character Classes 

In order to define the syntax at the level of characters, SDF provides
character classes, which represent a set of characters from which one
character can be recognized during parsing. The content of a character
classes is a specification of single characters or character ranges
(`c0-``c1`). Letters and digits can be written as
themselves, all other characters should be escaped using a slash, e.g.
`\_`. Characters can also be indicated by their decimal ASCII
code, e.g. `\13` for linefeed. Some often used non-printable
characters have more mnemonic names, e.g., `\n` for newline,
`\ ` for space and `\t` for tab.

Character classes can be combined using set operations. The most common
one is the unary complement operator `~`, e.g `~[\n]`.
Binary operators are the set difference `/`, union `\/`
and intersection `/\`.

<div class="example">

**Example 6.2. Examples of Character Classes**

<div class="example-contents">

<div class="variablelist">

<span class="term"> `[0-9]` </span>

:   Character class for digits: 0, 1, 2, 3, 4, 5, 6, 8, 9.

<span class="term"> `[0-9a-fA-F]` </span>

:   Characters typically used in hexi-decimal literals.

<span class="term"> `[fFdD]` </span>

:   Characters used as a floating point type suffix, typically in C-like
    languages.

<span class="term"> `[\ \t\12\r\n]` </span>

:   Typical character class for defining whitespace. Note that SDF does
    not yet support \\f as an escape for form feed (ASCII code 12).

<span class="term"> `[btnfr\"\'\\]` </span>

:   Character class for the set of characters that are usually allowed
    as escape sequences in C-like programming languages.

<span class="term"> `~[\"\\\n\r]` </span>

:   The set of characters that is typically allowed in string literals.

6.3. Examples: Defining Lexical Syntax {.title style="clear: both"}
--------------------------------------

Until now, we have mostly discussed the design of SDF. Now, it's about
time to see how all these fancy ideas for syntax definition work out in
practice. In this and the next section, we will present a series of
examples that explain how typical language constructs are defined in
SDF. This first section covers examples of lexical syntax constructs.
The next section will be about context-free syntax.

### 6.3.1. Simple Whitespace 

Before we can start with the examples of lexical constructs like
identifiers and literals, you need to know the basics of defining
whitespace. In SDF, layout is a special sort, called `LAYOUT`. To
define layout, you have to define productions that produce this
`LAYOUT` sort. Thus, to allow whitespace we can define a
production that takes all whitespace characters and produces layout.
Layout is lexical syntax, so we define this in a lexical syntax section.

``` {.programlisting}
lexical syntax
  [\ \t\r\n] -> LAYOUT
```

We can now also reveal how `context-free syntax` exactly works.
In `context-free syntax`, layout is allowed between symbols in
the left-hand side of the productions, by automatically inserting
optional layout (e.g. `LAYOUT?`) between them.

In the following examples, we will assume that whitespace is always
defined in this way. So, we will not repeat this production in the
examples. We will come back to the details of whitespace and comments
later.


### 6.3.2. Identifiers 

Almost every language has identifiers, so we will start with that.
Defining identifiers themselves is easy, but there is some more
definition of syntax required, as we will see next. First, the actual
definition of identifiers. As in most languages, we want to disallow
digits as the first character of an identifier, so we take a little bit
more restrictive character class for that first character.

``` {.programlisting}
lexical syntax
  [A-Za-z][A-Za-z0-9]* -> Id
```

#### 6.3.2.1. Reserving Keywords 

If a language would only consists of identifiers, then this production
does the job. Unfortunately, life is not that easy. In practice,
identifiers interact with other language constructs. The best known
interaction is that most languages do not allow keywords (such as
`if`, `while`, `class`) and special literals (such
as `null`, `true`). In SDF, keywords and special literals
are not automatically preferred over identifiers. For example, consider
the following, very simple expression language (for the context-free
syntax we appeal to your intuition for now).

``` {.programlisting}
lexical syntax
  [A-Za-z][A-Za-z0-9]* -> Id
  "true"  -> Bool
  "false" -> Bool

context-free start-symbols Exp
context-free syntax
  Id   -> Exp {cons("Id")}
  Bool -> Exp {cons("Bool")}
```

The input `true` can now be parsed as an identifier as well as a
boolean literal. Since the generalized-LR parser actually supports
ambiguities, we can even try this out:

``` {.screen}
$ echo "true" | sglri -p Test.tbl
amb([Bool("true"), Id("true")])
```

The `amb` term is a representation of the ambiguity. The argument
of the ambiguity is a list of alternatives. In this case, the first is
the boolean literal and the second is the identifier true. So, we have
to define explicitly that we do not want to allow these boolean literals
as identifiers. For this purpose, we can use SDF <span
class="emphasis">*reject productions*</span>. The intuition of reject
productions is that <span class="emphasis">*all*</span> derivations of a
symbol for which there is a reject production are forbidden. In this
example, we need to create productions for the boolean literals to
identifiers.

``` {.programlisting}
lexical syntax
  "true"  -> Id {reject}
  "false" -> Id {reject}
```

For `true`, there will now be two derivations for an `Id`:
one using the reject production and one using the real production for
identifiers. Because of that reject production, all derivations will be
rejected, so `true` is not an identifier anymore. Indeed, if we
add these productions to our syntax definition, then the true literal is
no longer ambiguous:

``` {.screen}
$ echo "true" | sglri -p Test.tbl
Bool("true")
```

We can make the definition of these reject productions a bit more
concise by just reusing the `Bool` sort. In the same way, we can
define keywords using separate production rules and have a single reject
production from keywords to identifiers.

``` {.programlisting}
lexical syntax
  Bool    -> Id {reject}
  Keyword -> Id {reject}

  "class" -> Keyword
  "if"    -> Keyword
  "while" -> Keyword
```

#### 6.3.2.2. Longest Match 


Scanners usually apply a longest match policy for scanning tokens. Thus,
if the next character can be included in the current token, then this
will always be done, regardless of the consequences after this token. In
most languages, this is indeed the required behaviour, but in some
languages longest match scanning actually doesn't work. Similar to not
automatically reserving keywords, SDF doesn't choose the longest match
by default. Instead, you need to specify explicitly that you want to
recognize the longest match.

For example, suppose that we introduce two language constructs based on
the previously defined `Id`. The following productions define two
statements: a simple goto and a construct for variable declarations,
where the first `Id` is the type and the second the variable
name.

``` {.programlisting}
context-free syntax
  Id    -> Stm {cons("Goto")}
  Id Id -> Stm {cons("VarDec")}
```

For the input `foo`, which is of course intended to be a goto,
the parser will now happily split up the identifier `foo`, which
results in variable declarations. Hence, this input is ambiguous.

``` {.screen}
$ echo "foo" | sglri -p Test.tbl
amb([Goto("foo"), VarDec("f","oo"), VarDec("fo","o")])
```

To specify that we want the longest match of an identifier, we define a
<span class="emphasis">*follow restriction*</span>. Such a follow
restriction indicates that a string of a certain symbol cannot be
followed by a character from the given character class. In this way,
follow restrictions can be used to encode longest match disambiguation.
In this case, we need to specify that an `Id` cannot be followed
by one of the identifier characters:

``` {.programlisting}
lexical restrictions
  Id -/- [A-Za-z0-9]
```

Indeed, the input `foo` is no longer ambiguous and is parsed as a
goto:

``` {.screen}
$ echo "foo" | sglri -p Test.tbl
Goto("foo")
```

</div>

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.3.3. Keywords 

</div>

</div>

</div>

In [Section
6.3.2](tutorial-sdf.html#section-sdf-lexical-identifiers "6.3.2. Identifiers")
we explained how to reject keywords as identifiers, so we will not
repeat that here. Also, we discussed how to avoid that identifiers get
split. A similar split issue arises with keywords. Usually, we want to
forbid a letter immediately after a keyword, but the scannerless parser
will happily start a new identifier token immediately after the keyword.
To illustrate this, we need to introduce a keyword, so let's make our
previous `goto` statement a bit more clear:

``` {.programlisting}
context-free syntax
  "goto" Id -> Stm {cons("Goto")}
```

To illustrate the problem, let's take the input `gotox`. Of
course, we don't want to allow this string to be a goto, but without a
follow restriction, it will actually be parsed by starting an identifier
after the `goto`:

``` {.screen}
$ echo "gotox" | sglri -p Test.tbl
Goto("x")
```

The solution is to specify a follow restriction on the `"goto"`
literal symbol.

``` {.programlisting}
lexical restrictions
  "goto" -/- [A-Za-z0-9]
```

It is not possible to define the follow restrictions on the
`Keyword` sort that we introduced earlier in the `reject`
example. The follow restriction must be defined on the symbol that <span
class="emphasis">*literally*</span> occurs in the production, which is
not the case with the `Keyword` symbol. However, you can specify
all the symbols in a single follow restriction, seperated by spaces:

``` {.programlisting}
lexical restrictions
  "goto" "if" -/- [A-Za-z0-9]
```

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.3.4. Integer Literals 

</div>

</div>

</div>

Compared to identifiers, integer literals are usually very easy to
define, since they do not really interact with other language
constructs. Just to be sure, we still define a lexical restriction. The
need for this restriction depends on the language in which the integer
literal is used.

``` {.programlisting}
lexical syntax
  [0-9]+ -> IntConst

lexical restrictions
  IntConst -/- [0-9]
```

In mainstream languages, there are often several notations for integer
literal, for example decimal, hexadecimal, or octal. The alternatives
are then usually prefixed with one or more character that indicates the
kind of integer literal. In Java, hexadecimal numerals start with
`0x` and octal with a `0` (zero). For this, we have to
make the definition of decimal numerals a bit more precise, since
`01234` is now an <span class="emphasis">*octal*</span> numeral.

``` {.programlisting}
lexical syntax
  "0"         -> DecimalNumeral
  [1-9][0-9]* -> DecimalNumeral

  [0][xX] [0-9a-fA-F]+ -> HexaDecimalNumeral
  [0]     [0-7]+       -> OctalNumeral
```

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.3.5. Floating-Point Literals 

</div>

</div>

</div>

Until now, the productions for lexical syntax have not been very
complex. In some cases, the definition of lexical syntax might even seem
to be more complex in SDF, since you explicitly have to define behaviour
that is implicit in existing lexical anlalysis tools. Fortunately, the
expressiveness of lexical syntax in SDF also has important advantages,
even if it is applied to language that are designed to be processed with
a separate scanner. As a first example, let's take a look at the
definition of floating-point literals.

Floating-point literals consists of three elements: digits, which may
include a dot, an exponent, and a float suffix (e.g. `f`,
`d` etc). There are three optional elements in float literals:
the dot, the exponent, and the float suffix. But, if you leave them all
out, then the floating-point literal no longer distinguishes itself from
an integer literal. So, one of the floating-point specific elements is
required. For example, valid floating-point literals are: `1.0`,
`1.`, `.1`, `1f`, and `1e5`, but invalid
are: `1`, and `.e5`. These rules are encoded in the usual
definition of floating-point literals by duplicating the production rule
and making different elements optional and required in each production.
For example:

``` {.programlisting}
lexical syntax
  [0-9]+ "." [0-9]* ExponentPart? [fFdD]? -> FloatLiteral
  [0-9]* "." [0-9]+ ExponentPart? [fFdD]? -> FloatLiteral
  [0-9]+            ExponentPart  [fFdD]? -> FloatLiteral
  [0-9]+            ExponentPart? [fFdD]  -> FloatLiteral

  [eE] SignedInteger -> ExponentPart
  [\+\-]? [0-9]+ -> SignedInteger
```

However, in SDF we can use <span class="emphasis">*reject
production*</span> to reject these special cases. So, the definition of
floating-point literals itself can be more naturally defined in a single
production
[![1](images/callouts/1.png)](tutorial-sdf.html#prod-float-literal). The
reject production
[![2](images/callouts/2.png)](tutorial-sdf.html#prod-reject-integer-literal)
defines that there should at least be one element of a floating-point
literal: it rejects plain integer literals. The reject production
[![3](images/callouts/3.png)](tutorial-sdf.html#prod-reject-dot) defines
that the digits part of the floating-point literals is not allowed to be
a single dot.

``` {.programlisting}
lexical syntax
  FloatDigits ExponentPart? [fFdD]? -> FloatLiteral 
  [0-9]* "." [0-9]* -> FloatDigits
  [0-9]+            -> FloatDigits

  [0-9]+ -> FloatLiteral {reject} 
  "."    -> FloatDigits  {reject} 
```

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.3.6. Comments 

</div>

</div>

</div>

Similar to defining whitespace, comments can be allowed everywhere by
defining additional `LAYOUT` productions. In this section, we
give examples of how to define several kinds of common comments.

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

#### 6.3.6.1. End-of-line Comment 

</div>

</div>

</div>

Most languages support end-of-line comments, which start with special
characters, such as `//`, `#`, or `%`. After that,
all characters on that line are part of the comment. Defining
end-of-line comments is quite easy: after the initial characters, every
character except for the line-terminating characters is allowed until a
line terminator.

``` {.programlisting}
lexical syntax
  "//" ~[\n]* [\n] -> LAYOUT
```

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

#### 6.3.6.2. Traditional Block Comment 

</div>

</div>

</div>

Block comments (i.e. `/* ... */`) are a bit more tricky to
define, since the content of a block comment may include an asterisk
(`*`). Let's first take a look at a definition of block comments
that does not allow an asterisk in its content:

``` {.programlisting}
  "/*" ~[\*]* "*/" -> LAYOUT
```

If we allow an asterisk and a slash, the sequence `*/` will be
allowed as well. So, the parser will accept the string `/* */ */`
as a comment, which is not valid in C-like languages. In general,
allowing this in a language would be very inefficient, since the parser
can never decide where to stop a block comment. So, we need to disallow
just the specific sequence of characters `*/` inside a comment.
We can specify this using a <span class="emphasis">*follow
restriction*</span>: an asterisk in a block comments is allowed, but it
cannot be followed by a slash (`/`).

But, on what symbol do we specify this follow restriction? As explained
earlier, we need to specify this follow restriction on a symbol that
<span class="emphasis">*literally*</span> occurs in the production. So,
we could try to allow a `"*"`, and introduce a follow restriction
on that:

``` {.programlisting}
lexical syntax
  "/*" (~[\*] | "*")* "*/" -> LAYOUT

lexical restrictions
  "*" -/- [\/]
```

But, the symbol `"*"` also occurs in other productions, for
example in multiplication expressions and we do not explicitly say here
that we intend to refer to the `"*"` in the block comment
production. To distinguish the block comment asterisk from the
multiplication operator, we introduce a new sort, creatively named
`Asterisk`, for which we can specify a follow restriction that
only applies to the asterisk in a block comment.

``` {.programlisting}
lexical syntax
  "/*" (~[\*] | Asterisk)* "*/" -> LAYOUT
  [\*] -> Asterisk

lexical restrictions
  Asterisk -/- [\/]
```

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

#### 6.3.6.3. Balanced Block Comments 

</div>

</div>

</div>

To illustrate that lexical syntax in SDF can actually be context-free,
we now show an example of how to implement balanced, nested block
comments, i.e. a block comment that supports block comments in its
content: `/* /* */     */`. Defining the syntax for nested block
comments is quite easy, since we can just define a production that
allows a block comment inside itself
[![1](images/callouts/1.png)](tutorial-sdf.html#sdf-nested-comment). For
performance and predictability, it is important to require that the
comments are balanced correctly. So, in addition to disallowing
`*/` inside in block comments, we now also have to disallow
`/*`. For this, we introduce a `Slash` sort, for which we
define a follow restriction
[![2](images/callouts/2.png)](tutorial-sdf.html#sdf-slash-follow-restriction),
similar to the `Asterisk` sort that we discussed in the previous
section.

``` {.programlisting}
lexical syntax
  BlockComment -> LAYOUT

  "/*" CommentPart* "*/" -> BlockComment
  ~[\/\*]      -> CommentPart
  Asterisk     -> CommentPart
  Slash        -> CommentPart
  BlockComment -> CommentPart 
  [\/] -> Slash
  [\*] -> Asterisk

lexical restrictions
  Asterisk -/- [\/]
  Slash    -/- [\*]  
```

</div>

</div>

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

6.4. Examples: Defining Context-Free Syntax {.title style="clear: both"}
-------------------------------------------

</div>

</div>

</div>

Context-free syntax in SDF is syntax where layout is allowed between the
symbols of the productions. Context-free syntax can be defined in a
natural way, thanks to the use of generalized-LR parsing, declarative
disambiguation mechanism, and an extensive set of regular expression
operators. To illustrate the definition of context-free syntax, we give
examples of defining expressions and statements. Most of the time will
be spend on explaining the disambiguation mechanisms.

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.4.1. Expressions 

</div>

</div>

</div>

In the following sections, we will explain the details of a slightly
extended version of the SDF modules in [Example
6.1](tutorial-sdf.html#ex-basic-module "Example 6.1. Basic constructs of SDF"),
shown in [Example
6.3](tutorial-sdf.html#xmpl-sdf-expression-module "Example 6.3. Syntax of Small Expression Language in SDF").

<div class="example">

**Example 6.3. Syntax of Small Expression Language in SDF**

<div class="example-contents">

``` {.programlisting}
module Expression
imports
  Lexical

exports
  context-free start-symbols Exp
  context-free syntax
    Id          -> Var 
    Var         -> Exp {cons("Var") }
    IntConst    -> Exp {cons("Int") }}
    "(" Exp ")" -> Exp {bracket }

    "-" Exp     -> Exp {cons("UnaryMinus")}
    Exp "*" Exp -> Exp {cons("Times"), left }
    Exp "/" Exp -> Exp {cons("Div"), left}
    Exp "%" Exp -> Exp {cons("Mod"), left}
    Exp "+" Exp -> Exp {cons("Plus") , left}
    Exp "-" Exp -> Exp {cons("Minus"), left}
    Exp "=" Exp -> Exp {cons("Eq"), non-assoc }
    Exp ">" Exp -> Exp {cons("Gt"), non-assoc}

  context-free priorities 
       "-" Exp -> Exp 
    > {left: 
        Exp "*" Exp -> Exp
        Exp "/" Exp -> Exp
        Exp "%" Exp -> Exp
      } 
    > {left: 
        Exp "+" Exp -> Exp
        Exp "-" Exp -> Exp
      }
    > {non-assoc: 
        Exp "=" Exp -> Exp
        Exp ">" Exp -> Exp
      }
```

</div>

</div>

\

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.4.2. Constructor Attributes and Abstract Syntax Trees 

</div>

</div>

</div>

First, it is about time to explain the constructor attribute,
`cons`, which was already briefly mentioned in [Section
5.1.2](tutorial-parsing.html#section-concepts-parse-trees "5.1.2. Parse Trees").
In the example expression language, most productions have a constructor
attribute, for example
[![2](images/callouts/2.png)](tutorial-sdf.html#xmpl-cf-cons1),
[![3](images/callouts/3.png)](tutorial-sdf.html#xmpl-cf-cons2) and
[![6](images/callouts/6.png)](tutorial-sdf.html#xmpl-cf-cons3), but some
have not, for example
[![1](images/callouts/1.png)](tutorial-sdf.html#xmpl-cf-injection) and
[![4](images/callouts/4.png)](tutorial-sdf.html#xmpl-cf-bracket).

The `cons` attribute does not have any actual meaning in the
definition of the syntax, i.e the presence or absence of a constructor
does not affect the syntax that is defined in any way. The constructor
only serves to specify the name of the abstract syntax tree node that is
to be constructed if that production is applied. In this way, the
`cons` attribute
[![3](images/callouts/3.png)](tutorial-sdf.html#xmpl-cf-cons2) of the
production for integer literals, defines that an `Int` node
should be produced for that production:

``` {.screen}
$ echo "1" | sglri -p Test.tbl
Int("1")
```

Note that this `Int` constructor takes a single argument, a
string, which is the name of the variable. This argument of `Int`
is a string because the production for `IntConst` is defined in
<span class="emphasis">*lexical syntax*</span> and all derivations from
lexical syntax productions are represented as strings, i.e. without
structure. As another example, the production for addition has a
`Plus` constructor attribute
[![3](images/callouts/3.png)](tutorial-sdf.html#xmpl-cf-cons2). This
production has three symbols on the left-hand side, but the constructor
takes only two arguments, since literals are not included in the
abstract syntax tree.

``` {.screen}
$ echo "1+2" | sglri -p Test.tbl
Plus(Int("1"),Int("2"))
```

However, there are also productions that have no `cons`
attribute, i.e.
[![1](images/callouts/1.png)](tutorial-sdf.html#xmpl-cf-injection) and
[![4](images/callouts/4.png)](tutorial-sdf.html#xmpl-cf-bracket). The
production
[![1](images/callouts/1.png)](tutorial-sdf.html#xmpl-cf-injection) from
`Id` to `Var` is called an <span
class="emphasis">*injection*</span>, since it does not involve any
additional syntax. Injections don't need to have a constructor
attribute. If it is left out, then the application of the production
will not produce a node in the abstract syntax tree. Example:

``` {.screen}
$ echo "x" | sglri -p Test.tbl
Var("x")
```

Nevertheless, the production
[![4](images/callouts/4.png)](tutorial-sdf.html#xmpl-cf-bracket) does
involve additional syntax, but does not have a constructor. In this
case, the `bracket` attribute should be used to indicate that
this is a symbol between brackets, which should be literals. The
`bracket` attribute does not affect the syntax of the language,
similar to the constructor attribute. Hence, the parenthesis in the
following example do not introduce a node, and the `Plus` is a
direct subterm of `Times`.

``` {.screen}
$ echo "(1 + 2) * 3" | sglri -p Test.tbl
Times(Plus(Int("1"),Int("2")),Int("3"))
```

**Conventions.** In Stratego/XT, constructors are by covention <span
class="emphasis">*CamelCase*</span>. Constructors may be overloaded,
i.e. the same name can be used for several productions, but be careful
with this feature: it might be more difficult to distinguish the several
cases for some tools. Usually, constructors are not overloaded for
productions with same number of arguments (arity).

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.4.3. Ambiguities in Expressions 

</div>

</div>

</div>

<div class="example">

**Example 6.4. Ambiguous Syntax Definition for Expressions**

<div class="example-contents">

``` {.programlisting}
  Exp "+" Exp -> Exp {cons("Plus")}
  Exp "-" Exp -> Exp {cons("Minus")}
  Exp "*" Exp -> Exp {cons("Mul")} 
  Exp "/" Exp -> Exp {cons("Div")}
```

</div>

</div>

\
Syntax definitions that use only a single non-terminal for expressions
are highly ambiguous. [Example
6.4](tutorial-sdf.html#xmpl-sdf-amb-expression "Example 6.4. Ambiguous Syntax Definition for Expressions")
shows the basic arithmetic operators defined in this way. For every
combination of operators, there are now multiple possible derivations.
For example, the string `a+b*c` has two possible derivations,
which we can actually see because of the use of a generalized-LR parser:

``` {.programlisting}
$ echo "a + b * c" | sglri -p Test3.tbl  | pp-aterm
amb(
  [ Times(Plus(Var("a"), Var("b")), Var("c"))
  , Plus(Var("a"), Times(Var("b"), Var("c")))
  ]
)
```

These ambiguities can be solved by using the associativities and
priorities of the various operators to disallow undesirable derivations.
For example, from the derivations of `a + b *       c` we usually
want to disallow the second one, where the multiplications binds weaker
than the addition operator. In plain context-free grammars the
associativity and priority rules of a language can be encoded in the
syntax definition by introducing separate non-terminals for all the
priority levels and let every argument of productions refer to such a
specific priority level. [Example
6.5](tutorial-sdf.html#xmpl-sdf-disamb-expression "Example 6.5. Non-Ambiguous Syntax Definition for Expressions")
shows how the usual priorities and associativity of the operators of the
example can be encoded in this way. For example, this definition will
never allow an `AddExp` as an argument of a `MulExp`,
which implies that `*` binds stronger than `+`. Also,
`AddExp` can only occur at the left-hand side of an
`AddExp`, which makes the operator left associative.

This way of dealing with associativity and priorities has several
disadvantages. First, the disambiguation is not natural, since it is
difficult to derive the more high-level rules of priorities and
associativity from this definition. Second, it is difficult to define
expressions in a modular way, since the levels need to be known and new
operators might affect the carefully crafted productions for the
existing ones. Third, due to all the priority levels and the productions
that connect these levels, the parse trees are more complex and parsing
is less efficient. For these reasons SDF features a more declarative way
of defining associativity and priorities, which we discuss in the next
section.

<div class="example">

**Example 6.5. Non-Ambiguous Syntax Definition for Expressions**

<div class="example-contents">

``` {.programlisting}
  AddExp -> Exp

  MulExp -> AddExp
  AddExp "+" MulExp -> AddExp {cons("Plus")}
  AddExp "-" MulExp -> AddExp {cons("Minus")}

  PrimExp -> MulExp
  MulExp "*" PrimExp -> MulExp {cons("Mul")} 
  MulExp "/" PrimExp -> MulExp {cons("Div")}

  IntConst -> PrimExp {cons("Int")}
  Id       -> PrimExp {cons("Var")}
```

</div>

</div>

\

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.4.4. Associativity and Priorities 

</div>

</div>

</div>

<div class="warning" style="margin-left: 0.5in; margin-right: 0.5in;">

### Work in Progress 

This chapter is work in progress. Not all parts have been finished yet.
The latest revision of this manual may contain more material. Refer to
the [online
version](http://releases.strategoxt.org/strategoxt-manual/strategoxt-manual-unstable/).

</div>

In order to support natural syntax definitions, SDF provides several
declarative disambiguation mechanisms. Associativity declarations
(`left`, `right`, `non-assoc`), disambiguate
combinations of a binary operator with itself and with other operators.
Thus, the left associativity of `+` entails that `a+b+c`
is parsed as `(a+b)+c`. Priority declarations (`>`)
declare the relative priority of productions. A production with lower
priority cannot be a direct subtree of a production with higher
priority. Thus `a+b*c` is parsed as `a+(b*c)` since the
other parse `(a+b)*c` has a conflict between the `*` and
`+` productions.

``` {.programlisting}
  ...
> Exp "&"  Exp -> Exp
> Exp "^"  Exp -> Exp
> Exp "|"  Exp -> Exp
> Exp "&&" Exp -> Exp
> Exp "||" Exp -> Exp
> ... 
```

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

#### 6.4.4.1. Group Associativity 

</div>

</div>

</div>

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

#### 6.4.4.2. Priorities and Hedged Symbols 

</div>

</div>

</div>

Solution: introduce an auxiliary non-terminal

</div>

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.4.5. Array Creation and Access Ambiguity 

</div>

</div>

</div>

This is usually handled with introducing a new non-terminal.

``` {.programlisting}
context-free syntax
  "new" ArrayBaseType DimExp+ Dim*  -> ArrayCreationExp {cons("NewArray")}

  Exp "[" Exp "]" -> Exp {cons("ArrayAccess")}
  ArrayCreationExp "[" Exp "]" -> Exp {reject}
```

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.4.6. Statements 

</div>

</div>

</div>

[Figure
6.1](tutorial-sdf.html#Fig-RegularExpressions "Figure 6.1.  Syntax definition with regular expressions.")
illustrates the use of these operators in the extension of the
expression language with statements and function declarations. Lists are
used in numerous places, such as for the sequential composition of
statements (`Seq`), the declarations in a let binding, and the
formal and actual arguments of a function (`FunDec` and
`Call`). An example function definition in this language is:

<div class="figure">

**Figure 6.1. Syntax definition with regular expressions.**

<div class="figure-contents">

``` {.screen}
module Statements
imports Expressions 
exports
  sorts Dec FunDec
  context-free syntax
    Var ":=" Exp                     -> Exp {cons("Assign")}
    "(" {Exp ";"}* ")"           -> Exp {cons("Seq")}
    "if" Exp "then" Exp "else" Exp   -> Exp {cons("If")}
    "while" Exp "do" Exp             -> Exp {cons("While")}
    "let" Dec* "in" {Exp ";"}* "end" -> Exp {cons("Let")}
    "var" Id ":=" Exp                -> Dec {cons("VarDec")}
    FunDec+                  -> Dec {cons("FunDecs")}
    "function" Id "(" {Id ","}* ")"  
      "=" Exp                        -> FunDec {cons("FunDec")}
    Var "(" {Exp ","}* ")"       -> Exp {cons("Call")}
  context-free priorities
    {non-assoc:
     Exp "=" Exp             -> Exp
     Exp ">" Exp              -> Exp}
  >  Var ":=" Exp                    -> Exp
  > {right:
     "if" Exp "then" Exp "else" Exp  -> Exp
     "while" Exp "do" Exp            -> Exp}
  >  {Exp ";"}+ ";" {Exp ";"}+       -> {Exp ";"}+

context-free start-symbols Exp
```

</div>

</div>

\
``` {.screen}
function fact(n, x) =
  if n > 0 then fact(n - 1, n * x) else x
```

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

#### 6.4.6.1. Dangling Else 

</div>

</div>

</div>

</div>

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.4.7. Whitespace and Comments 

</div>

</div>

</div>

``` {.programlisting}
context-free restrictions
  LAYOUT? -/- [\ \t\12\n\r]
```

Why follow restrictions on whitespace is necessary

``` {.programlisting}
context-free restrictions
  LAYOUT?  -/- [\/].[\*]
  LAYOUT?  -/- [\/].[\/]
```

</div>

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

6.5. Unit Testing of Syntax Definitions {.title style="clear: both"}
---------------------------------------

</div>

</div>

</div>

<span>**Parse-unit**</span> is a tool, part of Stratego/XT, for testing
SDF syntax definitions. The spirit of unit testing is implemented in
parse-unit by allowing you to check that small code fragments are parsed
correctly with your syntax definition.

In a parse testsuite you can define tests with an input and an expected
result. You can specify that a test should succeed (`succeeds`,
for lazy people), fail (`fails`) or that the abstract syntax tree
should have a specific format. The input can be an inline string or the
contents of a file for larger tests.

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.5.1. Usage example 

</div>

</div>

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

#### 6.5.1.1. Syntax Definition 

</div>

</div>

</div>

Assuming the following grammar for a simple arithmetic expressions:

``` {.programlisting}
module Exp
exports
  context-free start-symbols Exp
  sorts Id IntConst Exp
  
  lexical syntax
    [\ \t\n]  -> LAYOUT
    [a-zA-Z]+ -> Id
    [0-9]+    -> IntConst
  
  context-free syntax
    Id        -> Exp {cons("Var")}
    IntConst  -> Exp {cons("Int")}
  
    Exp "*"  Exp -> Exp  {left, cons("Mul")}
    Exp "/"  Exp -> Exp  {left, cons("Div")}
    Exp "%"  Exp -> Exp  {left, cons("Mod")}
  
    Exp "+"  Exp -> Exp  {left, cons("Plus")}
    Exp "-"  Exp -> Exp  {left, cons("Minus")}
  
  context-free priorities
    {left:
      Exp "*"  Exp -> Exp
      Exp "/"  Exp -> Exp
      Exp "%"  Exp -> Exp
    } 
  > {left:
      Exp "+"  Exp -> Exp
      Exp "-"  Exp -> Exp
    }
```

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

#### 6.5.1.2. Parse Testsuite 

</div>

</div>

</div>

You could define the following parse testsuite in a file
`expression.testsuite` :

``` {.programlisting}
testsuite Expressions
topsort Exp

test simple int literal
  "5" -> Int("5")

test simple addition
  "2 + 3" -> Plus(Int("2"), Int("3"))

test addition is left associative
  "1 + 2 + 3" -> Plus(Plus(Int("1"), Int("2")), Int("3"))

test
  "1 + 2 + 3" succeeds

test multiplication has higher priority than addition
  "1 + 2 * 3" -> Plus(Int("1"), Mul(Int("2"), Int("3")))

test
  "x" -> Var("x")

test
  "x1" -> Var("x1")

test
  "x1" fails

test
  "1 * 2 * 3" -> Mul(Int("1"), Mul(Int("2"), Int("3")))
```

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

#### 6.5.1.3. Running the Parse Testsuite 

</div>

</div>

</div>

Running this parse testsuite with:

``` {.screen}
  $ parse-unit -i expression.testsuite -p Exp.tbl
```

will output:

``` {.screen}
-----------------------------------------------------------------------
executing testsuite Expressions with 9 tests
-----------------------------------------------------------------------
* OK   : test 1 (simple int literal)
* OK   : test 2 (simple addition)
* OK   : test 3 (addition is left associative)
* OK   : test 4 (1 + 2 + 3)
* OK   : test 5 (multiplication has higher priority than addition)
* OK   : test 6 (x)
sglr: error in g_0.tmp, line 1, col 2: character `1' (\x31) unexpected
* ERROR: test 7 (x1)
  - parsing failed
  - expected:  Var("x1")
sglr: error in h_0.tmp, line 1, col 2: character `1' (\x31) unexpected
* OK   : test 8 (x1)
* ERROR: test 9 (1 * 2 * 3)
  - succeeded: Mul(Mul(Int("1"),Int("2")),Int("3"))
  - expected:  Mul(Int("1"),Mul(Int("2"),Int("3")))
-----------------------------------------------------------------------
results testsuite Expressions
successes : 7
failures  : 2
-----------------------------------------------------------------------
```

</div>

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.5.2. Parse Testsuite Syntax 

</div>

</div>

</div>

You cannot escape special characters because there is no need to escape
them. The idea of the testsuite syntax is that test input typically
contains a lot of special characters, which therefore they should no be
special and should not need escaping.

Anyhow, you still need some mechanism make it clear where the test input
stops. Therefore the testsuite syntax supports several quotation
symbols. Currently you can choose from: `"`, `""`,
`"""`, and `[`, `[[`, `[[[`. Typically, if
you need a double quote in your test input, then you use the `[`.

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.5.3. Debugging Support 

</div>

</div>

</div>

Parse-unit has an option to parse a single test and write the result to
the output. In this mode ambiguities are accepted, which is useful for
debugging. The option for the 'single test mode' is
`--single nr`{.option} where *`nr`* is the number in the testsuite
(printed when the testsuite is executed). The `--asfix2`{.option} flag
can be used to produce an asfix2 parse tree instead of an abstract
syntax tree.

</div>

<div class="section" lang="en">

<div class="titlepage">

<div>

<div>

### 6.5.4. Invocation in Automake 

</div>

</div>

</div>

The following make rule can be used to invoke parse-unit from your build
system.

``` {.programlisting}
%.runtestsuite : %.testsuite
      $(SDF_TOOLS)/bin/parse-unit -i $< -p $(PARSE_UNIT_PTABLE) --verbose 1 -o /dev/null
    
```

A typical `Makefile.am` fo testing your syntax definitions
looks like:

``` {.programlisting}
EXTRA_DIST = $(wildcard *.testsuite)

TESTSUITES = \
  expressions.testsuite \
  identifiers.testsuite

PARSE_UNIT_PTABLE = $(top_srcdir)/syn/Foo.tbl

installcheck-local: $(TESTSUITES:.testsuite=.runtestsuite)
    
```

