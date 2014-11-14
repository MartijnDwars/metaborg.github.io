# Spoofax Sunshine

Spoofax Sunshine is the runtime library that allows Spoofax-based languages to be used outside of Eclipse (e.g. command line interface).

# Obtaining Sunshine

## Binaries

You can download the latest prebuilt binary of Spoofax Sunshine from the [releases page](https://github.com/metaborg/spoofax-sunshine/releases). The Jar file contains all dependencies.

## Source

The Spoofax Sunshine project is hosted on GitHub in the [metaborg/spoofax-sunshine](https://github.com/metaborg/spoofax-sunshine) repository.


# Usage

Sunshine can be launched in one of two ways:

1. manually specifying the details of language used
2. automatically discovering and loading all languages in a language repository

## 1. Using Sunshine with manual language

This is an example of how Sunshine can be invoked with manual language specification:

    java -jar spoofax-sunshine.jar
      --lang WebDSL
      --ctree ../../webdsl2/include/webdsl.ctree
      --jar ../../webdsl2/include/webdsl.jar
      --jar ../../webdsl2/include/webdsl-java.jar
      --table ../../webdsl2/include/WebDSL.tbl
      --ssymb Unit
      --ext app
     
      --project ../../yellowgrass/
      --builder "Generate Java"
      --build-on yellowgrass.app

Where:

- `--lang` specifies the name of the language,
- `--ctree` and `--jar` specify path relative to the execution directory where the language's transformation code resides
- `--table` gives the parse table to use for parsing
- `--ssymb` gives that start-symbol to use for parsing
- `--ext` specifies the file extension to associate with the given language
- `--project` defines the root directory containing the source code to be processed using the language
- `--builder` gives the name of the transformation to be applied to the input file. The argument to `--builder` is the title of the menu action as specified in the `LANG-Menus.esv` file in the language definition, e.g.:

    action: "Generate Java" = generate-java ... 

- `--build-on` specifies the file that the transformation should be applied on. 




## 2. Automatic language discovery

Sunshine can automatically detect and load all languages in a given directory structure. This (a) eliminates the need for verbose invocations and (b) allows multiple languages to be loaded and used at the same time. In the latter case Sunshine selects transformations to apply by dispatching on file extensions.

The example invocation above now becomes:

    java -jar spoofax-sunshine.jar
      --auto-lang ../../webdsl2/include
      --project ../../yellowgrass/
      --builder "Compute Metrics"
      --build-on yellowgrass.app

Where:

- `--auto-lang` instructs Sunshine to recursively look for `LANG-packed.esv` files and load the corresponding languages.

## Applying the on-save handler as a transformation

Sunshine can automatically invoke the on-save handler defines in the `LANG-main.esv` file. For this use the name passed to the `--builder` argument should be *"OnSave"*.

## Getting help

Sunshine supports additional parameters and flags. A list and brief explanation can be obtained by running:

    java -jar spoofax-sunshine.jar --help


