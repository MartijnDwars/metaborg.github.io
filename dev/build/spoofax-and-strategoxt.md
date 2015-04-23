# Building Spoofax and StrategoXT

This section describes how to build Spoofax and StrategoXT from scratch, on the command-line.

## Supported platforms

OSX, Linux, and Windows are supported.

## Requirements

1. **Git 1.8.2 or higher** is required to check out the source code from our GitHub repositories. Instructions on how to install Git for your platform can be found here: <http://git-scm.com/downloads>. If you run OSX and have [Homebrew](http://brew.sh/) installed, you can install Git by executing `brew install git`. Confirm your Git installation by executing `git version`.
2. **Maven 3.2 or higher** is required to build Spoofax and StrategoXT, refer to the [Setting up Maven](/dev/maven/#setting-up-maven) section for instructions on how to install Maven.
3. **MetaBorg Maven artifacts** are required since the build depends on artifacts from previous builds for bootstrapping purposes. Follow [Using MetaBorg Maven artifacts](/dev/maven/#using-metaborg-maven-artifacts) to make these artifacts available to Maven.
4. **Python 3.4 or higher** is used to orchestrate the build. Instructions on how to install Python for your platform can be found here: <https://www.python.org/downloads/>. If you run OSX and have [Homebrew](http://brew.sh/) installed, you can install Python by executing `brew install python3`. Confirm your Python installation by executing `python3 --version`.

## Cloning the source code

Clone the source code from the [spoofax-releng](https://github.com/metaborg/spoofax-releng) repository with the following commands:

{% highlight bash %}
git clone https://github.com/metaborg/spoofax-releng.git
cd spoofax-releng
git submodule update --init --remote --recursive
{% endhighlight %}

Cloning and updating submodules can take a while, since we have many submodules and some have a large history.

## Building

To build Spoofax, simply execute:

{% highlight bash %}
./releng build all
{% endhighlight %}

This downloads the latest StrategoXT, and builds Spoofax. If you also want to build StrategoXT from scratch, execute:

{% highlight bash %}
./releng build -s -t all
{% endhighlight %}

The `-s` flag build StrategoXT instead of downloading it, and `-t` skips the StrategoXT tests since they are very lengthy.
The `all` part of the command indicates that we want to build all components. If you would only like to build the Java components of Spoofax, and skip the Eclipse plugins, execute:

{% highlight bash %}
./releng build java
{% endhighlight %}

Use `./releng build` to get a list of components available for building, and `./releng build --help` for help on all the command-line flags and switches.

If you have opened a project in the repository in Eclipse, you **must turn off 'Project &rarr; Build Automatically'** in Eclipse, otherwise the Maven and Eclipse compilers will interfere and possibly fail the build. After the Maven build is finished, enable 'Build Automatically' again.
{: .notice}

## Updating the source code

If you want to update the repository and submodules, execute:

{% highlight bash %}
git pull --rebase
./releng update
{% endhighlight %}

The `git pull` command will update any changes in the main repository, and the `releng update` command will update all submodules.

## Switching to a different branch

Switching to a different branch, for example the `spoofax-release` branch, is done with the following commands:

{% highlight bash %}
git checkout spoofax-release
git pull --rebase
./releng checkout
./releng update
{% endhighlight %}

The `releng checkout` command will check out the correct branches in all submodules, because Git does not do this automatically.

## Troubleshooting

### Resetting and cleaning

If updating or checking out a branch of submodule fails (because of unstaged or conflicting changes), you can try to resolve it yourself, or you can reset and clean everything.

**Resetting and cleaning DELETES UNCOMMITTED AND UNPUSHED CHANGES, which can cause PERMANENT DATA LOSS. Make sure all your changes are committed and pushed!**
{: .notice}

Reset and clean all submodules using:

{% highlight bash %}
./releng reset
./releng clean
{% endhighlight %}

### Weird compilation errors

If you get any weird compilation errors during the build, make sure that 'Project &rarr; Build Automatically' is turned off in Eclipse.

TODO: example of such a weird compilation error.
