# Mirror Maven Central Repository artifacts

Artifacts of most open source projects are hosted on the [Central Repository](https://search.maven.org/) server. If you are building any project using Maven, many artifacts will be downloaded from that server. While it is a fast server, it can still take a while to download all required artifacts for big projects.

If you are on the TUDelft network, you can use our local mirror of the Central Repository to speed things up. Using the mirroring requires a change in your local settings.xml file located at `~/.m2/settings.xml`. If this file does not exist, create it with the following content:

__Our artifact server is currently down, mirroring will fail.__
{: .notice}

{% highlight xml %}
<?xml version="1.0" ?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <mirrors>
    <mirror>
      <id>metaborg-nexus-central-mirror</id>
      <url>http://artifacts.metaborg.org/content/repositories/central/</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
  </mirrors>
</settings>
{% endhighlight %}

If you've already created a settings file before and want to add the mirror configuration, just add the `mirror` element (and the `mirrors` element if it does not exist yet) to the settings file.
