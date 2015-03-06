# Building Spoofax

This document describes how to build Spoofax from scratch, on the command-line.

## Brief description

This description skips over details and just provides the commands to build Spoofax. See the next section for a detailed description of the build.

Execute the following commands:

    git clone https://github.com/metaborg/spoofax-releng.git
    cd spoofax-releng
    ./update.sh
    ./build-all.sh

This will build Spoofax and its components, creating the following build products.

* **Eclipse update site**
```
spoofax-deploy/org.strategoxt.imp.updatesite/target/site
```

* **Sunshine JAR **
```
spoofax-sunshine/org.spoofax.sunshine/target/org.metaborg.sunshine-<VERSION>.jar
```

* **Benchmarker JAR**
```
spoofax-benchmark/org.metaborg.spoofax.benchmark.cmd/target/org.metaborg.spoofax.benchmark.cmd-<VERSION>.jar
```


* **Test runner JAR**
```
spt/org.metaborg.spoofax.testrunner.cmd/target/org.metaborg.spoofax.testrunner.cmd-<VERSION>.jar
```

* **Libraries JAR**
```
spoofax-deploy/org.metaborg.maven.build.spoofax.libs/target/org.metaborg.maven.build.spoofax.libs-<VERSION>.jar
```
    
## Detailed description

This is the detailed description, going over requirements and explaining each step in more detail.

### Supported platforms

Linux and OSX are supported, Windows is currently not supported.

### Requirements

Maven is the build system used to build Spoofax, refer to [Setting up Maven](setting-up-maven.md) for instructions on how to install Maven. Besides Maven, the build also requires:

**Git** is required to check out the source code from our GitHub repositories. Instructions on how to install Git for your platform can be found here: <http://git-scm.com/downloads>. If you run OSX and have [Homebrew](http://brew.sh/) installed, you can install Git by executing `brew install git`. Be sure that your Git version is 1.8.2 or higher, which you can confirm by executing `git version` on the command line.

**wget** is required to download the StrategoXT distribution. On OSX it can be installed from homebrew with `brew install wget`.

### Preparation

As preparation for building, the sources need to be checked out from GitHub. Clone the `spoofax-releng` repository and sub-repositories by executing:

    git clone https://github.com/metaborg/spoofax-releng.git
    cd spoofax-releng
    ./update.sh
    
This can take a while because some repositories have a large history, and GitHub cloning is fairly slow.

We also need to download and install the StrategoXT distribution, execute the following commands:

    cd spoofax-deploy/org.metaborg.maven.build.strategoxt
    ./build.sh
    
This will download the latest distribution from our build farm and install it in your local Maven repository. To update the installed StrategoXT distribution at a later time, run:

    ./build.sh -u
    
After the build is finished, `cd` back into the root of the repository.

### Building the Java components

There are several Java projects that Spoofax depends on, such as the abstract terms library and the Stratego interpreter. To build and install them, execute the following commands:

    cd spoofax-deploy/org.metaborg.maven.build.java
    ./build.sh
    
Maven will build and install all projects into your local Maven repository as artefacts. Other builds can then depend on these installed artefacts. After the build is finished, `cd ..` back into the root of the repository.

### Building the Spoofax Eclipse runtime

The Spoofax build uses the StrategoXT distribution and Java components which were installed in previous steps. If you are building Spoofax again at a later time, be sure to update your installed StrategoXT distribution and Java components before building Spoofax.

To start the build, just execute the build script:
    
    cd spoofax-deploy/org.metaborg.maven.build.spoofax.eclipse
    ./build.sh

The main artefact of a Spoofax build is the Eclipse update site which can be used to install or update Spoofax. After a successful build, the update site is located at:

    spoofax-deploy/org.strategoxt.imp.updatesite/target/site

You can add this local update site to Eclipse by navigating to _Help_ &rarr; _Install New Software..._ &rarr; _Add_ &rarr; _Local_.
    
Then it can be used to install Spoofax, like a regular update site. After a restart of Eclipse, Spoofax can be tested.

Alternatively, all projects can be imported into an Eclipse workspace, and a new Eclipse instance can be started to test Spoofax.

### Building Spoofax Sunshine

Sunshine depends on the StrategoXT distribution and Java components as well, be sure to keep them up to date. To start the build, just execute the build script:
    
    cd spoofax-deploy/org.metaborg.maven.build.spoofax.sunshine
    ./build.sh
    
The result is an executable JAR which includes all dependencies, which can be found at:

    spoofax-sunshine/org.spoofax.sunshine/target/org.metaborg.sunshine-<VERSION>-shaded.jar

## Additional arguments

All build scripts accept two arguments to change the behaviour of Maven. The `MAVEN_OPTS` environment variable can be set with the `-e opts`, which is used to set the JVM arguments that Maven uses. The `-a args` sets extra arguments that are passed to Maven.

For example, to increase memory and enable parallel builds with 4 threads, pass the following arguments: `-e "-server -Xmx512m -Xms512m -Xss16m" -a "-T 4"`.


## Build qualifier

Eclipse and Maven use the `major.minor.patch-qualifier` scheme for versioning. The major, minor, and patch parts of the version are set in each project, but the -qualifier part is not. In Maven, `-SNAPSHOT` is used as qualifier for replaceable (nightly) builds, but it is not required to replace `SNAPSHOT` with an actual number. However, in Eclipse, a plugin can only be upgraded if its version is higher than the installed version, so each build needs an increasing qualifier to be able to update installed plugins.

By default, the Java and Spoofax build scripts use the current date and time as qualifier. The `build-all.sh` script uses the `latest-timestamp.sh` script to get the timestamp of the latest commit in any of the submodules, which is reproducible because it only produces an increased timestamp if an actual change has been made. The qualifier for the Java and Spoofax build scripts can also be set manually by passing the `-q number` parameter as follows:

```
QUALIFIER=12345

cd org.metaborg.maven.build.java
./build.sh -q $QUALIFIER
cd ..

cd org.metaborg.maven.build.spoofax.eclipse
./build.sh -q $QUALIFIER
cd ..
```
