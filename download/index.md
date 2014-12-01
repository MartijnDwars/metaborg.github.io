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

To install Spoofax, all you need is [JRE 7 or higher](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 
and a copy of Eclipse. Eclipse versions 4.2 to 4.4 are supported, but we recommend [Eclipse Luna (4.4)](http://www.eclipse.org/downloads/) variant [Eclipse IDE for Java Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/lunasr1).
Make sure that Eclipse detects the correct JRE (_Window_ &rarr; _Preferences_ &rarr; _Java_ &rarr; _Installed JREs_) and that the JDK compliance level is set to 1.7 or higher (_Window_ &rarr; _Preferences_ &rarr; _Java_ &rarr; _Compiler_).

Spoofax runs on Windows, Linux, and Mac OS X. Editors built with Spoofax use 100% Java code and can be used on any platform that supports Eclipse.

## eclipse.ini

Spoofax requires *no separate binary packages or native installation*,
but for the development of plugins we strongly recommend you change the
Eclipse configuration to better accommodate meta-programming.

Please change the `eclipse.ini` file to include the following options
just after the `-vmargs` argument:

    -Xss8m
    -Xms256m
    -Xmx1024m
    -XX:MaxPermSize=256m
    -server
    -Djava.net.preferIPv4Stack=true

This sets an 8 MB stack size and 1024 MB of virtual memory (adjust
downwards for low-memory systems). The `eclipse.ini` file is located in
the installation directory of Eclipse, and typically in
`/Applications/eclipse/Eclipse.app/Contents/MacOS` for Mac OS X. An
example of a file after the changes is provided
[here](/Spoofax/ExampleIni).

Note that every option appears on a separate line, with no following
spaces, and that the memory options appear *after* the `-vmargs` option.
The `-Xmx` and `-Xms` options replace corresponding options that may
have been in the existing file. Setting these options ensures that
Eclipse has sufficient memory to dynamically load editors and is
prepared for a full meta-programming environment.

### Spoofax update sites

You can install the Spoofax language workbench using the update site
mechanism of Eclipse. Go to the _Help_ &rarr; _Install New Software..._ menu in
Eclipse. Then select _Add..._, enter the site name `Spoofax`, and the
update site URL:

       http://download.spoofax.org/update/stable/

Now select Spoofax (you may have to reselect the update site you just
entered), and follow the steps of the wizard.

#### Nightly builds

If you know what you are doing, and/or you want to help out with testing
and development, the latest build can be installed from the nightly
update site. These builds are untested!

       http://download.spoofax.org/update/nightly/

An archive of Eclipse update sites for the nightly builds is available at [http://download.spoofax.org/update/nightly-archive](http://download.spoofax.org/update/nightly-archive).

## License

Spoofax is open source software and is licensed under the [GNU Lesser General Public License (LGPL)](http://www.tldrlegal.com/l/lgpl2).

## Supporters

We use the [JProfiler](http://www.ej-technologies.com/products/jprofiler/overview.html) Java profiler to find and fix performance issues in Spoofax.

## Version history

We keep track of all resolved issues at our [YellowGrass issue
tracker](http://yellowgrass.org/project/Spoofax). Below is an overview
of released versions. We also maintain a
[roadmap](http://yellowgrass.org/roadmap/Spoofax) of past and future
versions.

### 2014

- [1.2](https://github.com/metaborg/doc/blob/master/releases/spoofax/1.2.md)

### 2013

-   [1.1](http://metaborg.org/post/2/spoofax-1.1)

### 2012

-   [1.0.2](http://strategoxt.org/Spoofax/News#1.0.2)

### 2011

-   [1.0](http://strategoxt.org/Spoofax/News#1.0)
-   [0.6.1](http://yellowgrass.org/tag/Spoofax/0.6.1)

### 2010

-   [0.6.0](http://yellowgrass.org/tag/Spoofax/0.6.0)
-   [0.5.2](http://yellowgrass.org/tag/Spoofax/0.5.2)
-   [0.5.1](http://yellowgrass.org/tag/Spoofax/0.5.1)
-   [0.5](http://yellowgrass.org/tag/Spoofax/0.5)
-   [0.4.6](http://yellowgrass.org/tag/Spoofax/0.4.6)
-   [0.4.5](http://yellowgrass.org/tag/Spoofax/0.4.5)
-   [0.4.4](http://yellowgrass.org/tag/Spoofax/0.4.4)
-   [0.4.3](http://yellowgrass.org/tag/Spoofax/0.4.3)
-   [0.4.2](http://yellowgrass.org/tag/Spoofax/0.4.2)
-   0.4.1
-   0.4

### 2006

-   [0.3.11](http://journal.boblycat.org/node/1159)
-   [0.3](http://journal.boblycat.org/node/1112)

### 2005

-   [0.2](http://journal.boblycat.org/node/1106)

