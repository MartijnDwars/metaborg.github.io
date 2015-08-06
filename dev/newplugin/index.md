---
title: New Spoofax plugin build and development instructions
layout: page
modified:
excerpt: ""
image:
  feature:
  credit:
  creditlink:
context: dev
---

# Building

The build instructions are mostly unchanged. The only difference is that you should check out the `new-spoofax-plugin` branch of `spoofax-releng`. Please follow [Building Spoofax and StrategoXT](/dev/build/#building-spoofax-and-strategoxt) for build instructions.

# Development

Make sure a full build, as described above, works before continuing. Follow the [Generating an Eclipse instance](/dev/develop/#generating-an-eclipse-instance) and [Fixing Eclipse settings](/dev/develop/#fixing-eclipse-settings) sections of the existing guide. Make sure you pass the `-l` flag to `gen-dev-spoofax`, to install the locally built Spoofax into the new Eclipse instance, instead of the nightly Spoofax.

Once the Eclipse instance is generated, start it up and import the projects you'd like to hack on. All Java and language projects have been converted into pure Maven projects, use **`Import -> Maven -> Existing Maven Projects`** to import these projects. Eclipse plugins are still imported with `Import -> General -> Existing Projects into Workspace`.

To test your changes in the Spoofax Eclise plugin, import the `org.metaborg.spoofax.eclipse` project from the `spoofax-eclipse` repository, which provides launch configurations for starting new Eclipse instances. Press the little down arrow next to the bug icon (next to the play icon) and choose `Spoofax with core (all plug-ins)` to start a new Eclipse instance that contains your changes.

Some gotcha's:

* When starting a new Eclipse instance using `Spoofax with core (all plug-ins)`, Eclipse might report problems about `org.eclipse.jdt.annotation`, `org.metaborg.meta.lang.spt.testrunner.cmd`, `org.metaborg.meta.lang.spt.testrunner.core`, `org.metaborg.spoofax.benchmark.core`, and `org.metaborg.spoofax.benchmark.cmd`. These problems can be ignored. You can also use the `Spoofax with core (selected plug-ins)` launcher to get rid of these errors. If you do this, you need to make sure that you add your Eclipse plugins to the launch configuration if you add any new plugins.
* If you change a language and want to test it in the Eclipse plugin, import that language's corresponding Eclipse plugin project. For example, `org.metaborg.meta.lang.nabl` has Eclipse plugin project `org.metaborg.meta.lang.nabl.eclipse`. Then compile both those projects from the command-line (don't forget to turn off build automatically in Eclipse), and start a new Eclipse instance.
* If you import the `org.metaborg.spoofax.eclipse.externaldeps` project (you probably don't need to), you also have to import the `make-permissive-jar-installer` Maven project, located at `spoofax/org.strategoxt.imp.generator/morepoms/make-permissive-jar`.

## Troubleshooting

If there are many errors in a project, try updating the Maven project. Right click the project and choose `Maven -> Update Project...`, uncheck `Clean projects` in the new dialog and press OK. This will update the project from the POM file, update any dependencies, and trigger a build. If this does not solve the problems, try it again but this time with `Clean projects` checked. Note that if you clean a language project, it has to be rebuilt from the command-line.

Multiple projects can be updated by selecting multiple projects in the package/project explorer, or by checking projects in the update dialog.
