---
title: "The SPT Testing Language"
layout: page
modified: 
excerpt: ""
image:
  feature: 
  credit: 
  creditlink: 
context: spt
---

<section id="table-of-contents" class="toc"> 
  <header> <h3>Overview</h3> </header>
  <div id="drawer" markdown="1">
  *  Auto generated table of contents
  {:toc}
  </div>
</section><!-- /#table-of-contents -->

The _Spoofax Testing Language_ (SPT) can be used to write language-agnostic tests for a language
implemented in Spoofax. Parsing rules, operator precedence and associativity, the abstract syntax
tree, errors and warnings, and transformations can be tested using SPT.

## Test module

An SPT module is a single file that specifies the tests, the language of the tests, the start symbol of the test, and any setup that the tests require. This is an example of an SPT module:

	module Expression-tests
	
	start symbol Exp
	
	test Add [[
		1 + 2
	]] parse succeeds
	
	test Multiply has correct AST [[
		1 * 2
	]] parse to Mul(Int("1"), _)
	
	test Parentheses [[
		(1 + 2)
	]]
	
	test Multiply and add precedence [[
		1 * 2 + 3
	]] parse to [[
		(1 * 2) + 3
	]]
	
## Module header
An SPT module's header specifies a module name and a language name. Additionally it can specify a
start symbol.

### Module name
The module name is a name used to distinguish test modules. The name can consist of letters,
numbers, underscores and dashes, but must start with a letter.

	module Expression-tests
	
### Language name
The language name is the name of the language that is tested. It must match the name of the
language as specified in the language's _General properties_ (`.main.esv`).

	language Entity-language

### Start symbol
By default all the context-free start-symbols specified in the syntax are possible start symbols for a
test. To limit the tests to a particular start symbol, use the `start symbol` directive:

	start symbol Exp

Make sure the start symbol specified for the test is also a context-free start-symbol for the
syntax.

## Setup blocks
Sometimes there is some boilerplate code that all tests share. For example, syntax that must
surround the fragment being tested, or variables that need to be defined. This kind of code
can be put into `setup` blocks.

A setup block has a description and some fragment that's inserted into each test. The place where
the setup block is defined also dictates where the fragment is put into each test. Most of the time
when using setup blocks, one block at the start and one block at the end of the test module are
used. Their descriptions can be chosen arbitrarily.

For example, this is a complete test module including setup blocks:

	module Statements
	language Some-Test-Language

	setup Before [[
		var x : Int := 10;
		var y : Int := 42;
		
		function foo() {
	]]
	
	test Global field reference [[
		var test := x;
	]] resolve

	setup Footer [[
		}
	]]

## Tests
A test specification starts with the `test` keyword and has a description, code fragment, and
condition.

	test Add 1 to 2 [[
		1 + 2
	]] parse to Add(Int("1"), Int("2"))

The description must be on a single line, and can contain almost all characters, except the opening
square bracket (`[`) and the double quote (`"`).

The fragment of a test is the exact code that must be parsed by the language. It is written between
double (`[[` `]]`), triple (`[[[` `]]]`) or quadruple (`[[[[` `]]]]`) square brackets. Usually
double brackets are sufficient, but if some part of the fragment also contains a sequence of square
brackets then use triple or quadruple brackets instead to avoid conflict.

	test Nested arrays [[[
		abc[def[0]]
	]]]

### Parsing condition
To have a test succeed when parsing succeeds, use the `parse succeeds` condition. Similarly, to have
a test succeed when parsing fails (e.g. a negative test case for illegal code), use the
`parse fails` condition.

	test Valid identifier [[
		abcd
	]] parse succeeds
	
	test Invalid identifier [[
		ab$cd
	]] parse fails

When no condition is specified, `parse succeeds` is implicitly assumed.

### Abstract Syntax Tree condition
A test can verify whether a particular Abstract Syntax Tree is produced by parsing the fragment. Use
the `parse to` condition followed by the expected AST. These kinds of test can be used to verify
operator precedence and associativity:

	test Add-Multiply precedence [[
		1 + 2 * 3
	]] parse to Add(Int("1"), Mul(Int("2"), Int("3")))
	
Parts of the AST may be replaced by a wildcard underscore (`_`). Wherever a wildcard appears,
the AST contains something in its place, but it doesn't matter what it exactly is.

	test Add-Multiply precedence [[
		1 + 2 * 3
	]] parse to Add(_, Mul(_, _))

If the AST is very big, you might want to write the AST in a file and refer to that file using
the `parse to file` syntax, where the file contains the expected ATerm.

For example, if the file `addmul.aterm` in the same directory has this contents:

	Add(Int("1"), Mul(Int("2"), Int("3")))

Then this test should succeed:

	test Add-Multiply precedence [[
		1 + 2 * 3
	]] parse to file addmul.aterm

### Concrete Syntax condition
Instead of literally writing the AST, concrete syntax (between double, triple or quadruple
square brackets) can be used that must parse to the same AST or the test fails.

	test Add-Multiply precedence [[
		1 + 2 * 3
	]] parse to [[
		1 + (2 * 3)
	]]

### Semantic error/warning markers condition
To test whether a specific number of semantic errors and warnings is generated, the `errors` and
`warnings` marker conditions can be used. These are preceded by the expected number of markers.

	test Variable declaration [[
		var s : String = "a";
	]] 0 errors
	
	test Bad variable declaration [[
		var s : String = 25;
	]] 1 error

Note that the singular `error` and plural `errors` are equivalent. Same for `warnings`.

A marker condition can be succeeded by a string to which the messages are matched. This string is
written between a pair of forward slashes (`/`). The test succeeds if at least one of the messages
contains the specified string.

	test Variable unassigned [[
		y
	]] 1 error /unassigned/

It is also possible to not specify the number of errors and warnings, and instead specify only the
string to match in one of the marker messages. For example, the following test could give multiple
errors and warnings. If one of those contains the string `multiple`, the test succeeds.

	test Multiple definitions of the same variable [[
		var y = 1
		var y := String = 2
		y
	]] /multiple/

### Reference resolving condition
Another possible condition is that a particular reference must be resolved for the test to succeed.
This is denoted by the `resolve` condition. Square brackets can be used to select the parts of the
fragment that must be resolved.

	test Reference resolving (1) [[
		x = 4
		[[x]]
	]] resolve
	
Additionally the reference and definitions in the condition can be made explicit. In this example
the `#1` refers to the first selected part of the fragment, and `#2` to the second selected part.

	test Reference resolving (1) [[
		[[x]] = 4
		[[x]]
	]] resolve #2 to #1

### Transformation condition
The fragment can be transformed using the `run`, `build` and `refactor` conditions, which must not
fail for the test to succeed. Optionally the expected result can be specified using the `to`
keyword.

By default the whole test fragment is parsed to an AST and fed to the transformation, but it is
possible to instead select a portion of the fragment using square brackets:

	test Adds up to 3 [[
		[[1 + 2]] * 3
	]] run calc to "3"

#### Run transformation
A `run` transformation must accept the AST of the fragment. For example, the following Stratego
rules:

	calc: Add(x, y) -> <addS> (<calc> x, <calc> y)
	calc: Int(i) -> i
	
can be applied to the input like this:

	test Adds up to 3 [[
		1 + 2
	]] run calc

Optionally, the expected result `3` can be specified like this:

	test Adds up to 3 [[
		1 + 2
	]] run calc to "3"

#### Build transformation
A `build` transformation is similar to a `run` transformation, except that the transformation must
accept the tuple `(node, position, ast, path, project-path)`. These are builders that are usually
defined in the `Menus.esv` files of the language.

	test 42 [[
		42
	]] build generate-result to 42

#### Refactor transformation
	
	test Refactor entity [[
		entity [[User]] {
			name : String
		}
	]] refactor rename-entity("Customer") to [[
		entity Customer {
			name : String
		}
	]]

## More information

- [Kats, Lennart CL, Rob Vermaas, and Eelco Visser. "Integrated language definition testing: enabling test-driven language development." _ACM SIGPLAN Notices._ Vol. 46. No. 10. ACM, 2011.](http://researchr.org/publication/KatsVV11)
- [Integrated Language Definition Testing: Enabling Test-Driven Language Development (SPLASH 2012)](http://www.slideshare.net/lennartkats/integrated-language-definition-testing-enabling-testdriven-language-development)
- [Test-Driven Language Development with Spoofax � A Calculator Language](https://strategoxt.org/Spoofax/Testing#Evaluation_of_expressions)

## Troubleshooting
Here are some possible issues and their solutions or workarounds.

### I just started writing a grammar but even my most basic tests fail
Ensure you have defined rules for `LAYOUT` in your grammar.

For example, in SDF3:

	lexical syntax
		LAYOUT = [\ \t\n\r]

	context-free restrictions
		LAYOUT? -/- [\ \t\n\r]

### I use a start symbol but all my tests fail
In addition to specifying a start symbol for the test module, make sure that you've specified that
symbol as a start symbol for the grammar too.

For example, if you have this SPT start symbol:

	start symbol
		Exp

Then also add it to your SDF3 start-symbols:

	context-free start-symbol
		Start Exp

### My test has square brackets and doesn't work
Put a space or newline after the test fragment before the closing square brackets of the test.

For example, instead of this:

	test Array [[abc[0]]]

Write this:

	test Array [[abc[0] ]]
		
### The last line of my test module is not syntax highlighted correctly
Put some more newlines at the end of your test module.

## Not supported
The following features are not supported:

- Full regular expression support in marker conditions.
- Note marker conditions.
- Freeform expression as condition.
- Combinations of test conditions.
- The `[[...]]` placeholder in setup blocks.
- Double quotes around fragments.
- [Content completion](http://yellowgrass.org/issue/Spoofax/760).
