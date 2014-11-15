---
title: Introducing the Spoofax Language Workbench
layout: post
context: spoofax
modified: 2010-05-28
---

We are pleased to announce the 0.5 release of the [Spoofax language workbench](/spoofax/), an Eclipse plugin that seamlessly integrates Java versions of Stratego and SDF into Eclipse. Spoofax can be used to develop new languages and transformations based on SDF and Stratego in the Eclipse environment. Read on below and be sure to follow our [tour with screenshots](/spoofax/tour/) for more information.

### Stratego and SDF for Java

Stratego and SDF have traditonally been implemented using C, but to increase portability we have developed Java versions of the Stratego compiler and the JSGLR parser for SDF. These new implementations are seamlessly integrated into the Spoofax environment, but can also be used as stand-alone tools.

### Building programming languages with IDE support

IDE support has become essential for developers to be productive with programming languages. Spoofax provides IDE support for Stratego and !SDF for developers of languages and transformations. It also aids in the development of IDE support for new languages: from the first version of an SDF grammar, an editor can be created for the language and used [side-by-side](/spoofax/features/) with the definition in Eclipse. Using Stratego, the editor can be enhanced with transformations and semantic editor services such as reference resolving and content completion.

The screenshot below illustrates some of the IDE features supported by editors created with Spoofax (click to enlarge):

<a href="/spoofax/images/screenshot-annotated-small.png"><img src="/spoofax/images/screenshot-annotated-small.png" alt="Spoofax editor features"  width="460" height="249"  border="0" /></a>

### More information

Spoofax can be downloaded from </download/>. When installed in Eclipse, the plugin provides a "New project" wizard that creates a new skeleton project illustrating some of the Spoofax features. The website also includes a [tour](/spoofax/tour) further showcasing the features of the workbench. For migrating C-based Stratego projects to Spoofax, please read our [FAQ](/spoofax/faq/) or contact us in case of other questions.

An overview of the architecture of Spoofax and how Spoofax can be used in the development of new languages and IDE services is given in the paper [The Spoofax Language Workbench. Rules for Declarative Specification of Languages and IDEs](http://researchr.org/publication/KatsVisser2010) by Lennart Kats and Eelco Visser, accepted for publication at [SPLASH/OOPSLA 2010](http://www.splashcon.org/). Further documentation can be found on the Spoofax website.

