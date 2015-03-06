# Building Spoofax languages on the command-line

In Eclipse, with the Spoofax plugin installed, languages can simply be built by building the project. On the command-line, Maven is used to build the language, refer to [Setting up Maven](setting-up-maven.md) for instructions on how to install Maven.

## Building the language

Language projects should contain a `pom.xml` file, which tells Maven how to build the language. If this file does not exist, build the project inside Eclipse once to generate it. Build the project by `cd`-ing into the project and executing `mvn clean verify`. 

If your language uses the SDF3, NaBL, or TS meta-languages, you must run the compilers for these languages, because the compilers are not available in a Maven build yet. First build your language inside Spoofax, this will run the compilers for these languages. If your language is under source control, check the generated files in. You must also add the following workaround to the language's POM file if you use SDF3:

```xml
<!-- Hack to prevent the src-gen folder from being cleaned. Since the build cannot run meta-languages such as SDF3, the
  src-gen folder is committed to the git repository. The clean goal will clean the src-gen folder, temporarily renaming
  it will prevent that. This hack overrides the spoofax-clean antrun execution from the parent POM. -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-antrun-plugin</artifactId>
  <executions>
    <execution>
      <id>spoofax-clean</id>
      <goals>
        <goal>run</goal>
      </goals>
      <phase>clean</phase>
      <configuration>
        <skip>${skip-language-build}</skip>
        <target>
          <move file="src-gen" tofile="src-gen-keep" />
          <ant antfile="build.main.xml" inheritRefs="true">
            <target name="clean" />
          </ant>
          <move file="src-gen-keep" tofile="src-gen" />
        </target>
      </configuration>
    </execution>
  </executions>
</plugin>
```

The build outputs of a language build are:

* Parse table: `include/language.tbl`
* Stratego binary: `include/language.jar` or `include/language.ctree`
* Stratego Java primitives binary: `include/language-java.jar`
* Packed ESV: `include/language.packed.esv`

These build outputs can be used by [Sunshine](https://github.com/metaborg/spoofax-sunshine/blob/master/README.md), the command-line implementation of Spoofax.

## Building an Eclipse updates site

TODO
