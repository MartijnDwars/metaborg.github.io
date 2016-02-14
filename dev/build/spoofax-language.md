# Building Spoofax languages on the command-line

In Eclipse, with the Spoofax plugin installed, languages can simply be built by building the project. On the command-line, Maven is used to build the language. This section describes how to build a Spoofax language on the command-line using Maven.

## Requirements

1. **Maven 3.2.5 or 3.3.9** is required to build Spoofax languages, refer to the [Setting up Maven](/dev/maven/#setting-up-maven) section for instructions on how to install Maven.
2. **MetaBorg Maven artifacts** are required since languages depend on several MetaBorg artifacts. Follow [Using MetaBorg Maven artifacts](/dev/maven/#using-metaborg-maven-artifacts) to make these artifacts available to Maven.

## Building the language

Language projects should contain a `pom.xml` file, which tells Maven how to build the language. Build the project by `cd`-ing into the project and executing `mvn clean verify`. 
