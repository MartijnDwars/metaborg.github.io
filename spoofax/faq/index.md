---
title: Spoofax FAQ
layout: page
modified: 
excerpt: "Spoofax Frequently Asked Questions"
image:
  feature: 
  credit: 
  creditlink: 
context: spoofax
---

## Where can I find answers to some of my other questions?

* [Spoofax Q&A](http://yellowgrass.org/questions/Spoofax)

* Maybe our [documentation](/Spoofax/Documentation) can answer your
question. 

* Otherwise, the [YellowGrass](http://yellowgrass.org/project/Spoofax) issue tracker
lists issue reports and questions from other users. You can add your own
question there,

* [[wiki(spoofax/support)|Contact]] us directly.

## Updates of Spoofax cause Eclipse to time out; why?

Due to a known problem with the combination of Eclipse 3.7, Java 1.7,
and Windows, the update manager can be very slow or time out when
downloading plugins. As a workaround, you can add the option
`-Djava.net.preferIPv4Stack=true` to your `eclipse.ini` file, as
described on the [Download](/Spoofax/Download) page.

## Sometimes Spoofax says "see error log." Where can I find it?

Go to *Window \> Show view \> Error log*.

## Which files of a language project should I check into my SVN/CVS/GIT/etc. version management system?

You should check in all files, except for the `bin` and `include`
distribution directories. Some Spoofax projects also have a `utils`
directory that does not have to be checked in. Be sure to check in the
hidden files/directories that Eclipse maintains (`.project`,
`.classpath`, and `.externalToolBuilders`). If you use an Eclipse plugin
such as [Subclipse](http://subclipse.tigris.org), it'll check these
files in automatically. If you want, you can skip some of the
`.generated` files, but at this point the `build.generated.xml` and
`lib/editor-common.generated.str` files are required to build the
project.

## How can I compile my Stratego specification to a JAR file instead of an interpreted ctree?

In your `build.main.xml` file, change this line:

    <target name="all" depends="spoofaximp.default.ctree"/>

into

    <target name="all" depends="spoofaximp.default.jar"/>

Then in your `editor/YourLanguage-Builders.esv`, change the `provider`
from `provider: include/yourlang.ctree` to
`provider: include/yourlang.jar`. (There may already be a
`provider: include/yourlang-java.jar` entry there.) When you distribute
your plugin, be sure you set the "unpack" option to "true" for your
plugin.

## How can i build my project outside of Eclipse?

See [[wiki(spoofax/command-line-builds)|these instructions]].

## How can i use my language outside of Eclipse?

Support for running Spoofax projects from the command line is currently provided by the [Sunshine](https://github.com/metaborg/spoofax-sunshine) runtime library. [Usage instructions](https://github.com/metaborg/spoofax-sunshine/blob/master/README.md) can get you started. You can download the latest release from the [Sunshine release page](https://github.com/metaborg/spoofax-sunshine/releases/latest).

## How do I deploy my language plugin to other users?

Either distribute it in source form to other Spoofax users, or follow
the steps of our [Tour](/Spoofax/Tour#Plugin_deployment) to distribute
it as an Eclipse plugin that does not require an existing installation
of Spoofax.

## How can I use Spoofax with C-based Stratego projects?

Stratego projects written with the C version of Stratego typically have
a very different project structure from the one employed by Spoofax
projects. To migrate these projects to Spoofax, we advise users to
create a new Spoofax project with a wizard and adapt the existing
project to this structure. The \`build.main.xml\` file can be used to
set any Stratego options and file locations for use with the Ant build
system. Also see the next question.

## How can I suppress errors in the Stratego editor in legacy Stratego projects?

Add a file called \`.disable-global-analysis\` to disable all non-local
errors. Any local errors such as missing local variables will still be
reported, but non-local errors such as missing strategies or
constructors will then be ignored. Use \`.warn-global-analysis\` to
print warnings instead for non-local errors.

## If I have two versions of the same language in my workspace, which one will be used?

If you have two versions of a language (e.g., one version installed as a
plugin and another loaded from source), the last version that you loaded
"wins." Any Java components loaded using the default Eclipse plugin
system can only be loaded from installed.

## How can I call Java methods from my editor?

See [The Tour: Adding Java
components](http://strategoxt.org/Spoofax/Tour#Adding_Java_components).

## When does Eclipse show the Spoofax console?

The Spoofax console can be used to print debugging messages. It is only
shown for editors that have been dynamically loaded from source; not for
editors that are deployed to end users.

## How do I debug my Stratego code?

Stratego does not have a stepping debugger at this point. Instead, you
can try `<debug(!"some message)> x` to print a message in the console
with the value of term `x`, or just `debug(!"some message")` to print
the message and the current term. You can also use the `with` keyword in
rewrite rules as follows:

    rewrite: x -> x'
    with
      x' := <s> ....

which ensures that a stack trace is printed when the body of the `with`
fails (e.g., because `s` cannot be applied).

## How can I output multiple files from a Stratego rule?

You can write directly to a file as follows:

    handle := <fopen> ("somefilename.ext", "w");
    <fputs> ("file contents string", handle);
    fclose

If you use this API from a `builder` or `on save` rule, you can change
it to the following form to ensure it only writes files statements like
the above:

    generate-java:
      (selected, position, ast, path, project-path) -> None()

## How can I use concrete syntax with Spoofax?

See YellowGrass [issue 55](http://yellowgrass.org/issue/Spoofax/55).

## When I hit "build project" nothing seems to happen? What's wrong?

Eclipse must have decided that you already ran the Ant file that
controls the build. Just edit and save one of the files in the project
and try again.

## Eclipse gets very slow after using it for some time or when opening many editors

The most common cause of this is a lack of heap memory. The default
Eclipse setting of 256M (or 512M for 64-bit systems) is very small, even
for casual Eclipse users. Please [configure](/Spoofax/Download) your
Eclipse installation to use more memory.

## How can I refresh a file when Eclipse won't respond to F5?

This seems to be a common issue in Eclipse. Instead of pressing F5,
right-click on the file in the package explorer and select "Refresh".

## When implementing content proposals, what to do when I get a
NOCONTEXT term?

The `NOCONTEXT` term indicates that the parser is unable to construct a
valid abstract syntax tree that corresponds to the grammar, for the part
of the tree where content completion is triggered. You can add
additional rules to the grammar to help it make a (partial) tree for
these cases, as documented [on the mailing
list](https://mailman.st.ewi.tudelft.nl/pipermail/users/2010-December/000311.html).
This is an area for future improvement. Don't hesitate to ask further
questions about this topic!

## Where can I find the latest bleeding edge versions of Spoofax?

The very latest builds of Spoofax can be downloaded by configuring
Eclipse to use the `http://download.spoofax.org/update/nightly` update
site. These versions are frequently updated, haven't undergone extensive
testing, and may break existing features. If you run into any problems
with the nightly releases, make sure you run the latest version and
report the bug with the version number used in the [issue
tracker](http://yellowgrass.org/project/Spoofax). Current resolved and
unresolved issues for the nightly builds are listed in the
[roadmap](http://yellowgrass.org/roadmap/Spoofax).

## "Artifact not found" during installation?

Eclipse can report this error when it tries to download an older version
of Spoofax that is no longer available. (This can especially occur with
the nightly builds.) Restarting Eclipse or going to the "available
update sites" dialog and refreshing or deleting and re-adding the update
site should solve this problem.

## How do I report a deadlock?

See [how to report a
deadlock](http://wiki.eclipse.org/How_to_report_a_deadlock) in the
Eclipse wiki.

## How do I call Java functions from Stratego?

You have to wrap them as Stratego strategies. These [[wiki(stratego/external-strategies)|detailed instructions]] explain how to do this.


