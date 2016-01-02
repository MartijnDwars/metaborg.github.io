---
title: Download
layout: page
modified:
excerpt: "Downloading Spoofax"
image:
  feature:
  credit:
  creditlink:
context: download
sideimage:
---

<section id="table-of-contents" class="toc">
  <header> <h3>Overview</h3> </header>
  <div id="drawer" markdown="1">
  *  Auto generated table of contents
  {:toc}
  </div>
</section><!-- /#table-of-contents -->

Spoofax is a platform for developing textual domain-specific languages with full-featured Eclipse editor plugins.

## Complete Eclipse Distribution

The easiest way to get started with Spoofax is to download an Eclipse Mars installation with Spoofax preinstalled.
The Eclipse installations come in two variants; one requires a [Java Runtime Environment (JRE) version 7 or higher](http://www.oracle.com/technetwork/java/javase/downloads/index.html) to be installed your system, and one that comes with an embedded JRE.
To use an installation, just extract it and run Eclipse.

Download a premade Eclipse Mars installation for your platform, *without* embedded JRE:

* [Windows 32-bits](http://download.spoofax.org/update/release/1.5.0/spoofax-win32-x86.zip)
* [Windows 64-bits](http://download.spoofax.org/update/release/1.5.0/spoofax-win32-x86_64.zip)
* [Linux 32-bits](http://download.spoofax.org/update/release/1.5.0/spoofax-linux-x86.tar.gz)
* [Linux 64-bits](http://download.spoofax.org/update/release/1.5.0/spoofax-linux-x86_64.tar.gz)
* [Mac OS X (Intel only)](http://download.spoofax.org/update/release/1.5.0/spoofax-macosx-x86_64.tar.gz)

Or with embedded JRE:

* [Windows 32-bits, embedded JRE](http://download.spoofax.org/update/release/1.5.0/spoofax-win32-x86-jre.zip)
* [Windows 64-bits, embedded JRE](http://download.spoofax.org/update/release/1.5.0/spoofax-win32-x86_64-jre.zip)
* [Linux 32-bits, embedded JRE](http://download.spoofax.org/update/release/1.5.0/spoofax-linux-x86-jre.tar.gz)
* [Linux 64-bits, embedded JRE](http://download.spoofax.org/update/release/1.5.0/spoofax-linux-x86_64-jre.tar.gz)
* [Mac OS X (Intel only), embedded JRE](http://download.spoofax.org/update/release/1.5.0/spoofax-macosx-x86_64-jre.tar.gz)


## Eclipse Plugin

To install Spoofax as an Eclipse plugin, all you need is a [Java Runtime Environment (JRE) version 7 or higher](http://www.oracle.com/technetwork/java/javase/downloads/index.html) and a copy of Eclipse.
Eclipse versions 4.3 to 4.5 are supported, but we recommend [Eclipse Mars (4.5)](http://www.eclipse.org/downloads/) variant [Eclipse IDE for Java Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/mars1).
Make sure that Eclipse detects the correct JRE (_Window_ &rarr; _Preferences_ &rarr; _Java_ &rarr; _Installed JREs_) and that the JDK compliance level is set to 1.7 or higher (_Window_ &rarr; _Preferences_ &rarr; _Java_ &rarr; _Compiler_).

Spoofax runs on Windows, Linux, and Mac OS X.

### eclipse.ini

Spoofax requires *no separate binary packages or native installation*, but for the development of plugins we strongly recommend you change the Eclipse configuration to better accommodate meta-programming.

Please change the `eclipse.ini` file to include the following options just after the `-vmargs` argument:

```
-Xss16m
-Xms1024m
-Xmx1024m
-XX:MaxPermSize=256m
-server
-Djava.net.preferIPv4Stack=true
```

This sets an 16 MB stack size and 1024 MB of virtual memory (adjust downwards for low-memory systems).
The `eclipse.ini` file is located in the installation directory of Eclipse for Windows and Linux, and in `Eclipse.app/Contents/MacOS` for Mac OS X.

Note that every option appears on a separate line, with no following spaces, and that the memory options appear *after* the `-vmargs` option.
The `-Xmx` and `-Xms` options replace corresponding options that may have been in the existing file.
Setting these options ensures that Eclipse has sufficient memory to dynamically load editors and is prepared for a full meta-programming environment.

### Spoofax update site

You can install the Spoofax language workbench using the update site mechanism of Eclipse.
Go to the _Help_ &rarr; _Install New Software..._ menu in Eclipse.
Then select _Add..._, enter the site name `Spoofax`, and the update site URL:

```
http://download.spoofax.org/update/stable/
```

Now select Spoofax (you may have to reselect the update site you just entered), and follow the steps of the wizard.

## Nightly builds

If you know what you're doing, and/or want to help out with testing and development, the latest build farm builds can be installed.
These builds are untested!

Use the following update site to install or update to the latest nightly version:

```
http://download.spoofax.org/update/nightly/
```

Or [download an Eclipse installation from our build farm](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/eclipse/) with the nightly version of Spoofax preinstalled.

## License

Spoofax is open source software and is licensed under the [GNU Lesser General Public License (LGPL)](http://www.tldrlegal.com/l/lgpl2).

## Supporters

We use the [JProfiler](http://www.ej-technologies.com/products/jprofiler/overview.html) Java profiler to find and fix performance issues in Spoofax.

## Version history

We keep track of all resolved issues at our [YellowGrass issue tracker](http://yellowgrass.org/project/Spoofax).
Below is an overview of released versions.
We also maintain a [roadmap](http://yellowgrass.org/roadmap/Spoofax) of past and future versions.

### 2015

* [1.5.0](/spoofax/spoofax-1-5-0/)
* [1.4.0](/spoofax/spoofax-1-4-0/)

### 2014

* [1.3.1](/spoofax/spoofax-1-3-1/)
* [1.3.0](/spoofax/spoofax-1-3-0/)
* [1.2.0](/spoofax/spoofax-1-2-0/)

### 2013

* [1.1.0](/spoofax/spoofax-1-1-0/)

### 2012

* [1.0.2](/spoofax/spoofax-1-0-2/)

### 2011

* [1.0.0](/spoofax/spoofax-1-0-0/)
* 0.6.1

### 2010

* [0.6.0](http://yellowgrass.org/tag/Spoofax/0.6.0)
* [0.5.2](http://yellowgrass.org/tag/Spoofax/0.5.2)
* [0.5.1](http://yellowgrass.org/tag/Spoofax/0.5.1)
* [0.5.0](http://yellowgrass.org/tag/Spoofax/0.5)
* [0.4.6](http://yellowgrass.org/tag/Spoofax/0.4.6)
* [0.4.5](http://yellowgrass.org/tag/Spoofax/0.4.5)
* [0.4.4](http://yellowgrass.org/tag/Spoofax/0.4.4)
* [0.4.3](http://yellowgrass.org/tag/Spoofax/0.4.3)
* [0.4.2](http://yellowgrass.org/tag/Spoofax/0.4.2)
* 0.4.1
* 0.4.0

### 2006

* 0.3.11
* 0.3.0

### 2005

* 0.2.0
