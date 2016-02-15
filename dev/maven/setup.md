# Setting up Maven

This section describes how to set up Maven, which is a requirement for building or developing any MetaBorg components such as Spoofax.

## Requirements

Maven requires a Java compiler, so a JDK needs to be installed on your computer, since the JRE does not ship with a Java compiler. The latest JDK can be downloaded and installed from: <http://www.oracle.com/technetwork/java/javase/downloads/index.html>. We require at least JDK7.

On OSX, it can be a bit tricky to use the installed JDK, because Apple by default installs JRE 6. To check which version of Java you are running, execute the `java -version` command. If this tells you that the Java version is 1.7, everything is fine. If not, the Java version can be set with a command. After you have installed JDK 7, execute:

{% highlight bash %}
export JAVA_HOME=`/usr/libexec/java_home -v 1.7`
{% endhighlight %}

Note that setting the Java version this way is not permanent. To make it permanent, add that line to `~/.bashrc` (create the file if it does not exist) or the equivalent for your OS/shell, which will execute it whenever a new terminal is opened.

Confirm your JDK installation and version by executing `java -version` and `javac -version`.

## Installing

Maven can be downloaded and installed from <http://maven.apache.org/download.cgi>. We require Maven 3.3.9 or higher.

Maven can be easily installed on OSX with Homebrew by executing `brew install maven`. Confirm the installation and version by running `mvn --version`.

## Memory allocation

By default, Maven does not assign a lot of memory to the JVM that it runs in, which may lead to out of memory exceptions during builds. To increase the allocated memory, execute before building:

{% highlight bash %}
export MAVEN_OPTS="-Xms512m -Xmx1024m -Xss16m -XX:MaxPermSize=512m"
{% endhighlight %}

Note that this is not permanent. To make it permanent, add that line to `~/.bashrc` (create the file if it does not exist) or the equivalent for your OS/shell, which will execute it whenever a new terminal is opened.
