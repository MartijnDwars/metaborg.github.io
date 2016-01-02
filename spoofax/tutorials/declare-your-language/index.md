---
layout: page
title: "Spoofax Tutorial: Questionnaire Language"
modified:
excerpt:
tags: []
image:
  feature:
  credit:  
  creditlink:
toc: true
share: true
context: spoofax
---

# Declare Your Language in Spoofax / Exercises

## Installing Spoofax

We highly recommend to work with a fresh Eclipse and Spoofax installation. You can download a copy for your OS and architecture:

* [MacOSX 64 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-macosx-x64-jre.zip)
* [Linux 64 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-linux-x64-jre.zip)
* [Linux 32 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-linux-x86-jre.zip)
* [Windows 64 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-windows-x64-jre.zip)
* [Windows 32 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-windows-x86-jre.zip)

## Initial Spoofax Project

You should start with a fresh Eclipse workspace and import the prepared Spoofax projects into the workspace. You can either [clone the git repository](https://github.com/metaborgcube/metaborg-ql) (select the dsldi-2015 branch) or [download the archive](http://download.spoofax.org/update/tutorial/spoofax-hands-on-ipa2015.zip).

1. Start Eclipse.
2. Select a fresh workspace.
3. Configure preferences (Eclipse > Preferences)
    1. Change appearance to Classic look (optional)
    1. Change font (General > Appearance > Colors and Fonts) then (Basic > Text Font) to Monaco 14pt (or something you like better)
    3. Turn off ‘Build Automatically’ in the Project menu
4. Import the initial Spoofax projects into your workspace:
    1. Right-click into the Package Explorer.
    2. Select **Import…** from the context menu.
    3. Choose **Existing Projects into Workspace…** from the list.
    4. Specify the **Root directory** of the metaborg-ql directory.
    5. Import all projects into your workspace by pressing the **Finish** button.
    6. Close the `org.spoofax.lang.lwc.ql.syntax`, `org.spoofax.lang.lwc.ql.analysis` and `org.spoofax.lang.lwc.ql.full` projects.
    6. Select the `org.spoofax.lang.lwc.ql.empty` project.
    7. Build the project by choosing **Build Project** from the **Project** menu or by pressing the corresponding keyboard shortcut (`Ctrl+Alt+B`, or `Cmd+Alt+B` for MacOSX).
    8. Restart Eclipse by going to the **File** menu and choosing **Restart**.

Spoofax

The PAPLJ project


building the project


## 1. Syntax

* Open projects paplj.syntax, paplj.syntax.exercises

* Build the paplj.syntax project

* Open a test-suite in the exercises project
  * Run the `Run testsuites` entry in the `Transform` menu

* Add productions to make the exercise tests succeed
  * syntax/Expressions.sdf3 
  * syntax/Classes.sdf3

* Use the doodle.pj file to try out different programs, or make your own

* Try out the `Syntax` menu
  * Format
  * Show Abstract Syntax
  * How do ‘Builders’ work?

* Close project paplj.syntax

## 2. Transformation

* Open paplj.transformation, paplj.transformation.exercises

* Inspect the solution for the syntax exercises

* Add rewrite rules to make the exercise tests succeed
  * trans/desugar/desugar-rules.str
  * trans/desugar/resugar-rules.str

* Use the doodle.pj file to try out different programs, or make your own

* Try out the `Transform` menu
  * Desugar
  * Desugar AST

* Close project paplj.transformation

## 3. Name and Type Analysis

* Open projects paplj.analysis, paplj.analysis.exercises

* Inspect the solution for the transformation exercises

* Add name binding rules to make the exercise tests succeed
  * trans/check/names.nab

* Add type rules to make the exercise tests succeed 
  * trans/check/types.ts

* Close project paplj.analysis

## 4. Dynamic Semantics

* Open projects paplj.semantics, paplj.semantics.exercises

* Inspect the solution for the analysis exercises

* Add reduction rules to make the exercise tests succeed
  * trans/semantics/interp.ds

* To build, first generate Java code from DynSem
  * Use `All to Java` entry in `Semantics` menu for `trans/semantics/semantics.ds`

* Close project paplj.semantics

## 5. Full Definition

The project paplj.full contains the full definition of the dynamic semantics
