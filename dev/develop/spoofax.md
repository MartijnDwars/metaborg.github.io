# Creating an Eclipse installation for Spoofax development

If you are developing a project that is included in Spoofax it is recommended to set up Eclipse for Spoofax development. This section describes how to create an Eclipse installation for developing Spoofax components.

## Requirements

1. A working **Spoofax and StrategoXT build** is required before being able to develop. Follow the [Building Spoofax and StrategoXT](/dev/build/#building-spoofax-and-strategoxt) section for instructions on how to build Spoofax and StrategoXT.

## Generating an Eclipse instance

The `b` script in the `spoofax-releng` repository can generate an Eclipse installation for you. Change directory into the spoofax-releng repository and run:

{% highlight bash %}
./b gen-dev-spoofax -d ~/eclipse/spoofax-dev
{% endhighlight %}

This will download and install Eclipse into `~/eclipse/spoofax-dev` with the right plugins and eclipse.ini for Spoofax development. The latest nightly version of the Spoofax plugin will be installed into that Eclipse. If you would like to install your locally built Spoofax plugin instead, pass the `-l` flag:

{% highlight bash %}
./b gen-dev-spoofax -l -d ~/eclipse/spoofax-dev
{% endhighlight %}

Generating an Eclipse installation can take several minutes. After it's done generating, open the Eclipse installation and confirm that it works by creating a Spoofax entity project.

## Fixing Eclipse settings

Some Eclipse settings unfortunately have sub-optimal defaults. Go to the Eclipse preferences and set these options:

* General
	* Enable: Keep next/previous editor, view and perspectives dialog open
* General &rarr; Startup and Shutdown
	* Enable: Refresh workspace on startup
* General &rarr; Workspace
	* Enable: Refresh using native hooks or polling
* Maven
	* Enable: Do not automatically update dependencies from remote repositories
	* Enable: Download Artifact Sources
	* Enable: Download Artifact JavaDoc
* Maven &rarr; User Interface
  * Enable: Open XML page in the POM editor by default
* Run/Debug &rarr; Launching
	* Disable: Build (if required) before launching

## Developing

Import the projects you'd like to hack on. To import Java and language projects, use **`Import -> Maven -> Existing Maven Projects`**. Eclipse plugins are still imported with `Import -> General -> Existing Projects into Workspace`.

To test your changes in the Spoofax Eclise plugin, import the `org.metaborg.spoofax.eclipse` project from the `spoofax-eclipse` repository, which provides launch configurations for starting new Eclipse instances. Press the little down arrow next to the bug icon (next to the play icon) and choose `Spoofax with core (all plug-ins)` to start a new Eclipse instance that contains your changes.

Some gotcha's:

* When starting a new Eclipse instance using `Spoofax with core (all plug-ins)`, Eclipse might report problems about `org.eclipse.jdt.annotation`, `org.metaborg.meta.lang.spt.testrunner.cmd`, and `org.metaborg.meta.lang.spt.testrunner.core`. These problems can be ignored.
* If you change a language and want to test it in a new Eclipse instance, import that language's corresponding Eclipse plugin project. For example, `org.metaborg.meta.lang.nabl` has Eclipse plugin project `org.metaborg.meta.lang.nabl.eclipse`. Then compile both those projects from the command-line (don't forget to turn off build automatically in Eclipse), and start a new Eclipse instance.

## Troubleshooting

If there are many errors in a project, try updating the Maven project. Right click the project and choose `Maven -> Update Project...`, uncheck `Clean projects` in the new dialog and press OK. This will update the project from the POM file, update any dependencies, and trigger a build. If this does not solve the problems, try it again but this time with `Clean projects` checked. Note that if you clean a language project, it has to be rebuilt from the command-line. Restarting Eclipse and repeating these steps may also help.

Multiple projects can be updated by selecting multiple projects in the package/project explorer, or by checking projects in the update dialog.

## Advanced: developing from scratch

In some cases it can be beneficial to have full control over all projects, instead of relying on Maven artifacts and the installed Spoofax plugin.
Only follow this approach if you know what you are doing!
To develop from scratch, uninstall Spoofax from Eclipse, and import all projects from `spoofax-releng` into the workspace.

If you change a language project, build them on the command-line, because languages cannot be built inside Eclipse without the Spoofax plugin.
