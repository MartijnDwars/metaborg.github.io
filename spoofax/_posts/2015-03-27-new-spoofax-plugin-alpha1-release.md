---
title: New Spoofax plugin alpha1 release
layout: post
context: spoofax
---

We're pleased to release our first alpha version of the rewritten Spoofax Eclipse plugin. The plugin is based on Spoofax core, which is a platform independent library with Spoofax's core functionality. We've chosen for a rewrite of the Eclipse plugin because it was infeasible to convert the current plugin to use Spoofax core. This first alpha version has the basic features of Spoofax, with more features being added in the coming months.

This document describes installation, porting language projects, differences in features, and how to report issues. Be sure to read the section on differences in features, because some operations are very different compared to the existing Spoofax Eclipse plugin.

**This is alpha software that has had very limited testing. Back up your data before using the new plugin!**

## Installing

The new Spoofax plugin cannot be installed alongside the existing plugin, you need to install it into a new Eclipse instance. **This alpha release is only compatible with the Eclipse IDE for Eclipse Committers**, [Download Eclipse IDE for Eclipse Committers 4.4.2](http://www.eclipse.org/downloads/packages/eclipse-ide-eclipse-committers-442/lunasr2) and install the new Spoofax plugin from this update site:

```
http://download.spoofax.org/update/release/newplugin-alpha1/
```

There is also a nightly update site for the new plugin, where new builds will be pushed whenever changes to the code are made:

```
http://download.spoofax.org/update/newplugin-nightly/
```

## Porting a language project

Language projects can be automatically ported to work with the new plugin by right clicking on the project and choosing `Spoofax (meta) -> Upgrade language project`. A dialog will appear where the name and id of the language must be filled in. If the language project contains a `packed.esv` file, these will be filled in for you. If not, you need to open the `main.esv` file and copy the name and id from there.

## Features

### Retained

The following features were retained from the existing Spoofax Eclipse plugin:

* Dynamic language development: build a language, see the effects without having to start a new Eclipse instance
* Syntax highlighting
* Info/warning/error markers
* Builders
* On-save builder

### Differences

#### Project builds

Eclipse has the concept of incremental project builders, which incrementally parse, analyze, and compile files inside a project. An example of such a project builder is the Eclipse JDT builder which incrementally builds Java files. Spoofax currently does not use this functionality, but the new plugin does!

The project builder for Spoofax parses, analyses, runs the on-save handler, and shows all error markers, for all language files (Stratego files, SDF3 files, instances of your DSL files, etc.) in the project. If the project is opened for the first time, a full build will occur, building all language files. When changes occur in the project, an incremental build occurs, building only changed files. This makes the `call-onsave` step in language builds unnecessary, because the project builder will always keep the SDF3, NaBL, and TS files up to date.

The commands under the `Project` menu in Eclipse now also affect Spoofax language files. If you execute `Project -> Clean...`, the `.cache` folder will be deleted, the index and task engine will be reset, all error markers will be deleted, and a full build will occur. This makes the `Reset and Reanalyze` builder  unnecessary, since this is now properly integrated with Eclipse. Automatic building can also be turned off by disabling `Project -> Build Automatically`. Builds will then only occur if `Project -> Build Project` or `Cmd+Alt+B` is invoked.

The Spoofax project builder is not enabled by default, to enable it a 'Spoofax nature' must be added to the project. A nature in Eclipse is a project tag which enables functionality for that project. To add the Spoofax nature to a project, right click the project, and choose `Spoofax -> Add Spoofax nature`.

Editors will parse and analyze files regardless of there being a Spoofax nature, but the on-save handler will not be called.

#### Language project builds

Language projects, e.g. your DSL project, can be automatically ported (described in a previous section), but I will explain the differences for reference purposes. 

Language projects require a 'Spoofax meta nature'. Choose `Spoofax (meta) -> Add Spoofax meta nature` to add it. This will also add a Spoofax nature if it is absent. Language projects are always built on-demand by invoking `Project -> Build Project` or `Cmd+Alt+B`, which will run the Ant build and reload the language afterwards. The `.externalToolBuilders` directory which contains some Eclipse files to invoke Ant builds are not necessary any more.

#### Builders

Builders are now located in the `Spoofax` main menu instead of buttons on the tool bar. Builders wait for the analyzed AST if needed, so the issue where builders are sometimes not executed on the analyzed AST should be solved now.

#### Cancellation

Editor updates can now be cancelled by clicking the red stop button in the progress view. If the progress view is not visible, you can open it by choosing `Window -> Show View -> Progress`. If the editor update is not responsive (it is looping for example), the thread running the editor update will be killed after a while. Killing a thread during analysis may leave the index and task engine in an inconsistent state. If this happens, clean the project using `Project -> Clean...` to force a full build, which makes the state consistent again.

Project builds and transformations cannot be cancelled in this release, but this will be supported in the future.

#### Console logging

Console logging in the new plugin is more prominent so that we can diagnose problems more easily. If the console is not visible, you can open it by choosing `Window -> Show View -> Console`. Spoofax adds 3 consoles to Eclipse:

* **Spoofax developers console**: produces a lot of debug information to diagnose problems with Spoofax
* **Spoofax console**: shows informational, warning, and error messages, mainly to diagnose problems with your language, such as failing Stratego transformations.
* **Spoofax language build console**: shows the output of language Ant builds.

By default, this alpha release activates the developers console. You can switch to different consoles by clicking the arrow next to the blue monitor icon in the console view. The language build console is automatically activated when building a language.

All informational, warning, and error messages are also sent to Eclipse's error log. The error log can sometimes contain more information about exceptions and stack traces in errors. If the error log is not visible, you can open it by choosing `Window -> Show View -> Error Log`.

#### Manually loading/unloading a language

A language can be manually loaded or reloaded by right clicking a project and choosing `Spoofax (meta) -> Load language`, and unloaded with `Spoofax (meta) -> Unload language`.

#### No IMP dependency

The new plugin does not depend on IMP, immensely simplifying and cleaning up the codebase.

### Missing

The following features that are in the existing plugin are missing from this alpha release, but may be added back in the future:

* Meta-languages
	* SPT
	* RTG
	* Box/PP
	* Aster
* Editor services
	* Reference resolution
	* Content completion
	* Outline
	* Hover tooltips
* New project wizard
* Concrete syntax extensions and .meta files
* Some Stratego primitives (please report missing primitives)
* Modelware
* eclipse.ini check
* Ant build log coloring

## Reporting issues

Use the main menu `Spoofax (meta) -> Report issue` to report issues, it provides a link to [this Yellowgrass project](http://yellowgrass.org/project/SpoofaxWithCore) and the Eclipse and Spoofax versions which you can copy paste. If Eclipse does not open the link in your default browser, but in Safari instead, follow these steps to fix that:

1. Open Safari.
2. Go to menu Safari > Preferences > General.
3. Change 'Default web browser' to Safari.
4. Close the Preferences dialog.
5. Re-open the Preferences dialog.
6. Change 'Default web browser' to Chrome (or your default browser of choice).
7. Close the Preferences dialog.

To be able to quickly fix bugs, please provide the following information when reporting bugs:

* **Eclipse version and package**
* **Spoofax version**
* **High-level description of the bug**
* **How to reproduce the bug**
* **What the expected behaviour is**
* **Any logs, exceptions, and stack traces related to the bug**

For example:

Eclipse version: org.eclipse.epp.package.standard.feature.feature.group 4.4.2.20150219-0708
Spoofax version: org.metaborg.spoofax.eclipse 1.5.0.20150327-133337

Syntax highlighting for Stratego strategies and rules are wrong, they are colored black. To reproduce, use the following Stratego file:

```
module a

strategies

  strat = fail
```

`strat` is colored back, but it should be turquoise according to [line 37 in Stratego-Sugar-Colorer.esv](https://github.com/metaborg/stratego/blob/master/org.strategoxt.imp.editors.stratego/editor/Stratego-Sugar-Colorer.esv#L37): `StrategyDef : 0 64 128 bold`.

## Known issues

See [this Yellowgrass project](http://yellowgrass.org/tag/SpoofaxWithCore/error) for all known issues.

## Getting help

Gabriel Konat has developed Spoofax Core and the new plugin, you can contact him at:

* IRC: [#spoofax on irc.freenode.net](irc://irc.freenode.net:6666/spoofax) (nickname: Gohla)
* E-mail: <g.d.p.konat@tudelft.nl>
