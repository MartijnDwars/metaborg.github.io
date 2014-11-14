# Building Spoofax languages on the command-line

In Eclipse, with the Spoofax plugin installed, languages can simply be built by building the project. On the command-line, Maven is used to build the language, refer to [Setting up Maven](setting-up-maven.md) for instructions on how to install Maven.

## Building the language

Language projects should contain a `pom.xml` file, which tells Maven how to build the language. If this file does not exist, build the project inside Eclipse once to generate it. Build the project by `cd`-ing into the project and executing `mvn clean verify`. The build outputs of a language build are:

* Parse table: `include/language.tbl`
* Stratego binary: `include/language.jar` or `include/language.ctree`
* Stratego Java primitives binary: `include/language-java.jar`
* Packed ESV: `include/language.packed.esv`

These build outputs can be used by [Sunshine](https://github.com/metaborg/spoofax-sunshine/blob/master/README.md), the command-line implementation of Spoofax.

## Building an Eclipse updates site

TODO
