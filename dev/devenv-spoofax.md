# Setting up Eclipse for Spoofax development

If you are developing a project that is included in Spoofax it is recommended to set up Eclipse for Spoofax development to test changes to the project.

## Install Eclipse

First, [download and install Eclipse](http://www.eclipse.org/downloads/packages/eclipse-standard-44/lunar). Make sure that you are running Java 7 or higher. There are many preferences in Eclipse that should be on by default, but are not for some reason. Go to the Eclipse preferences and enable these options:

* General
	* Keep next/previous editor, view and perspectives dialog open
* General &rarr; Startup and Shutdown
	* Refresh workspace on startup 
* General &rarr; Workspace
	* Refresh using native hooks or polling

And disable:

* Run/Debug &rarr; Launching
	* Build (if required) before launching

## Install m2eclipse plugin

The m2eclipse plugin runs Maven inside Eclipse. It is required when working on Java components of Spoofax, because Maven manages the dependencies between projects and on third party libraries. If you will never work on Java components of Spoofax, you can skip installing this plugin. Install the m2eclipse plugin into Eclipse from the following update site: `http://download.eclipse.org/technology/m2e/milestones/1.6`.

After installing the plugin into Eclipse and restarting, go into the Eclipse preferences and click on the `Maven` item. Disable the following checkboxes:

* Do not automatically update dependencies from remote repositories

Enable the following checkboxes:

* Download Artifact Sources
* Download Artifact JavaDoc

After that you need to install the Tycho Configurator from the update site `http://repo1.maven.org/maven2/.m2e/connectors/m2eclipse-tycho/0.7.0/N/LATEST/`. Be careful with the version, as 0.8 and up is known to break the current Spoofax builds. Install and restart eclipse.

Now click on the `Discovery` item and click on `Open Catalog`, a new window will appear where connectors can be installed. Check the following connectors and press Finish:

* buildhelper
* m2e-jdt-compiler

Two Eclipse plugins will be installed which finish hooking Maven plugins into Eclipse, together with the Tycho Configurator. Finish the installation and restart Eclipse.
Maven will now run inside Eclipse on projects with the Maven nature. It will automatically resolve dependencies to projects in the workspace and download third party dependencies.

## Setting up for Spoofax development

There are two ways to set up Eclipse for Spoofax development:

1. Install Spoofax into Eclipse, check out projects that you want to work on, and import them into Eclipse.
2. Check out all projects, build them with Maven, and import them into Eclipse.

The **first approach is recommended** because it is not possible to build languages inside Eclipse in the second approach, and it also requires less projects to be checked out. If you are not working on languages and want full control over all of Spoofax's components, use the second approach.

### First approach

Install the nightly Spoofax into Eclipse from update site: <http://download.spoofax.org/update/nightly/>.

Change the `eclipse.ini` file to include the following options just after the `-vmargs` argument
and restart Eclipse.

```
-Xss8m
-Xms256m
-Xmx1024m
-XX:MaxPermSize=256m
-server
-Djava.net.preferIPv4Stack=true
```

Clone the <https://github.com/metaborg/spoofax.git> repository and import the `org.strategoxt.imp.runtime` project into Eclipse, which provides launch configurations for Spoofax.

To make Maven resolve dependencies of required projects, Maven artifact repositories have to be added to your Maven settings file. Please read [these instructions](artifacts.md) on how to add our repositories.

Now check out any projects you want to work, import them into Eclipse, and start developing!

### Second approach

Check out the spoofax-releng repository: 

```bash
git clone https://github.com/metaborg/spoofax-releng.git
cd spoofax-releng
git submodule update --init --remote
```

Alternatively, you can check out all repositories yourself for more control. 

Since it is not possible to build languages inside Eclipse (because Spoofax is not installed), the languages have to be built using Maven. The Spoofax generator, debugging, and strategoxt also require special build steps which cannot be executed in Eclipse. Follow [the description for building spoofax](building-spoofax.md) to build everything with Maven. 

Import the following projects into Eclipse:

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
	
Eclipse plugins for EMF and GMF must be installed to be able to run the modelware component. Either close the modelware projects, or install the following plugins from the Eclipse releases update site:

* Graphical Modeling Framework (GMF) Runtime SDK
* EMF - Eclipse Modeling Framework SDK
* EMF Compare Core SDK

Now you can work on Java components such as the terms or Stratego interpreter projects. If a language was changed, for example by pulling in changes from git, just run the Maven builds again. __You must disable `Build Automatically` in the `Project` menu in Eclipse before running a Maven build, otherwise the Eclipse and Maven builds will interfere and cause errors. After the Maven build is finished, enable `Build Automatically` again.__

The advantage of this approach is that Spoofax is completely built from your Eclipse workspace, without any dependencies on an installed Spoofax version.

## Testing changes

To test changes to the Spoofax Eclipse plugin, a new Eclipse instance needs to be started. Press the little down arrow next to the bug icon (next to the play icon) and choose `Spoofax (no assertions)` to start a new Eclipse instance that contains your changes. To run with assertions enabled, use the `Spoofax (with assertions)` item, this will also provide more debugging output in the console.
