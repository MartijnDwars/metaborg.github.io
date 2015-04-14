# Setting up Eclipse for Spoofax development

If you are developing a project that is included in Spoofax it is recommended to set up Eclipse for Spoofax development. This section describes how to create an Eclipse installation for developing Spoofax components.

## Requirements

1. A working **Spoofax and StrategoXT build** is required before being able to develop. Follow the [Building Spoofax and StrategoXT](/dev/build/) section for instructions on how to build Spoofax and StrategoXT.

## Generating an Eclipse instance

The `releng` script in the spoofax-releng repository can generate an Eclipse installation for you. Change directory into the spoofax-releng repository and run:

```
./releng gen-dev-spoofax -d ~/eclipse/spoofax-dev
```

This will download and install Eclipse into `~/eclipse/spoofax-dev` with the right plugins and eclipse.ini for Spoofax development. The latest nightly version of the Spoofax plugin will be installed into that Eclipse. If you would like to install your locally built Spoofax plugin instead, pass the `-l` flag:

```
./releng gen-dev-spoofax -l -d ~/eclipse/spoofax-dev
```

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

Now check out any projects you want to work on, import them into Eclipse, and start developing!

To test changes, a new Eclipse instance needs to be started.
Clone the `https://github.com/metaborg/spoofax.git` repository and import the `org.strategoxt.imp.runtime` project into Eclipse, which provides launch configurations for starting new Eclipse instances.
Press the little down arrow next to the bug icon (next to the play icon) and choose `Spoofax (no assertions)` to start a new Eclipse instance that contains your changes. To run with assertions enabled, use the `Spoofax (with assertions)` item, this will also provide more debugging output in the console.

## Advanced: developing from scratch

In some cases it can be beneficial to have full control over all plugin projects, instead of relying on an installed Spoofax plugin. Only follow this approach if you know what you are doing!

To develop from scratch, uninstall Spoofax from Eclipse, and import the following projects into Eclipse:

* imp-patched
	* org.eclipse.imp.cheatsheets
	* org.eclipse.imp.java.hosted
	* org.eclipse.imp.lpg
	* org.eclipse.imp.lpg.ide
	* org.eclipse.imp.lpg.metatooling
	* org.eclipse.imp.metatooling
	* org.eclipse.imp.preferences
	* org.eclipse.imp.prefspecs
	* org.eclipse.imp.presentation
	* org.eclipse.imp.runtime
	* org.eclipse.imp.smapi
	* org.eclipse.imp.smapifier
	* org.eclipse.imp.xform
* mb-rep
	* org.spoofax.aterm
	* org.spoofax.interpreter.library.index
	* org.spoofax.terms
	* org.strategoxt.imp.editors.aterm
* mb-exec
	* org.spoofax.interpreter
	* org.spoofax.interpreter.adapter.ecj
	* org.spoofax.interpreter.core
	* org.spoofax.interpreter.library.eclipse
	* org.spoofax.interpreter.library.interpreter
	* org.spoofax.interpreter.library.java
	* org.spoofax.interpreter.library.jline
	* org.spoofax.interpreter.library.xml
	* org.spoofax.interpreter.ui
* mb-exec-deps
	* org.spoofax.interpreter.externaldeps
* runtime-libraries
	* org.metaborg.runtime.task
	* org.metaborg.meta.lang.analysis
	* org.spoofax.meta.runtime.libraries
* spoofax
	* org.strategoxt.imp.generator
	* org.strategoxt.imp.metatooling
	* org.strategoxt.imp.nativebundle
	* org.strategoxt.imp.runtime
	* org.strategoxt.imp.runtime.sidebyside.latest
	* org.strategoxt.imp.runtime.sidebyside.main
	* make-permissive-jar-installer (inside ```org.strategoxt.imp.generator/morepoms```)
* spoofax-sunshine
	* org.metaborg.sunshine
* meta languages
	* stratego
		* org.strategoxt.imp.editors.stratego
	* spt
		* org.strategoxt.imp.testing
		* org.strategoxt.imp.testing.ui
	* esv
		* org.strategoxt.imp.editors.editorservice
	* sdf
		* org.strategoxt.imp.editors.sdf
		* org.strategoxt.imp.editors.template
	* aster
		* org.strategoxt.imp.editors.aster
	* box
		* org.strategoxt.imp.editors.pp
	* nabl
		* org.metaborg.meta.lang.nabl
	* rtg
		* org.strategoxt.imp.editors.rtg
	* ts
		* org.metaborg.meta.lang.ts
	* dynsem
		* org.metaborg.meta.interpreter.framework
* jsglr
	* org.spoofax.interpreter.library.jsglr
	* org.spoofax.jsglr
* spoofax-debug
	* org.strategoxt.imp.debug.core
	* org.strategoxt.imp.debug.stratego.core
	* org.strategoxt.imp.debug.stratego.runtime
	* org.strategoxt.imp.debug.stratego.test
	* org.strategoxt.imp.debug.stratego.transformer
	* org.strategoxt.imp.debug.ui
* modelware
	* org.spoofax.modelware.emf
	* org.spoofax.modelware.gmf
* strategoxt
	* org.strategoxt.jarprovider
	* org.strategoxt.strj
* deployment
	* org.metaborg.maven.build.java
	* org.metaborg.maven.build.spoofax.eclipse
	* org.metaborg.maven.build.spoofax.libs
	* org.metaborg.maven.build.spoofax.sunshine
	* org.metaborg.maven.build.strategoxt
	* org.metaborg.maven.parent
	* org.metaborg.maven.parent.java
	* org.metaborg.maven.parent.language
	* org.metaborg.maven.parent	.plugin
	* org.spoofax.modelware.emf.feature
	* org.spoofax.modelware.gmf.feature
	* org.strategoxt.imp.feature
	* org.strategoxt.imp.updatesite
* other dependencies
	* shrike
		* com.ibm.wala.shrike
	* lpg-runtime
		* lpg.runtime.java

If you change a language project, run the Spoofax build on the command-line, because those languages cannot be built inside Eclipse without the Spoofax plugin.
You **must turn off 'Project &rarr; Build Automatically'** in Eclipse, otherwise the Maven and Eclipse compilers will interfere and possibly fail the build. After the Maven build is finished, enable 'Build Automatically' again.
