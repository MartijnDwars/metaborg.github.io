# Using MetaBorg Maven artifacts

MetaBorg's Maven artifacts are hosted on our artifact server: <http://artifacts.metaborg.org>. To use these artifacts, repositories have to be added to your Maven configuration. Repositories can be added to your local Maven settings file (which is recommended), or to a project's POM file.

## Local settings file

The recommended approach is to add repositories to your local Maven settings file, located at `~/.m2/settings.xml`. If you have not created this file yet, or want to completely replace it, simply create it with the following content:

{% highlight xml %}
<?xml version="1.0" ?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">  
  <profiles>
    <profile>
      <id>add-metaborg-release-repos</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <repositories>
        <repository>
          <id>metaborg-release-repo</id>
          <url>http://artifacts.metaborg.org/content/repositories/releases/</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>false</enabled>
          </snapshots>
        </repository>
      </repositories>
      <pluginRepositories>
        <pluginRepository>
          <id>metaborg-release-repo</id>
          <url>http://artifacts.metaborg.org/content/repositories/releases/</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>false</enabled>
          </snapshots>
        </pluginRepository>
      </pluginRepositories>
    </profile>

    <profile>
      <id>add-metaborg-snapshot-repos</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <repositories>
        <repository>
          <id>metaborg-snapshot-repo</id>
          <url>http://artifacts.metaborg.org/content/repositories/snapshots/</url>
          <releases>
            <enabled>false</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
        </repository>
      </repositories>
      <pluginRepositories>
        <pluginRepository>
          <id>metaborg-snapshot-repo</id>
          <url>http://artifacts.metaborg.org/content/repositories/snapshots/</url>
          <releases>
            <enabled>false</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
        </pluginRepository>
      </pluginRepositories>
    </profile>

    <profile>
      <id>add-spoofax-eclipse-repos</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <repositories>
        <repository>
          <id>spoofax-eclipse-repo</id>
          <url>http://download.spoofax.org/update/nightly/</url>
          <layout>p2</layout>
          <releases>
            <enabled>false</enabled>
          </releases>
          <snapshots>
            <enabled>false</enabled>
          </snapshots>
        </repository>
      </repositories>
    </profile>
  </profiles>
</settings>
{% endhighlight %}

If you've already created a settings file before and want to add the repositories, just add the `profile` element (and the `profiles` element if it does not exist yet) to the settings file.

## Advanced: project POM file

Repositories can also be added directly to a project's POM file, which only set the repositories for that particular project. This is not recommended, because it makes repositories harder to change by users, and duplicates the configuration. But it can be convenient, because it does not require an external settings file.

To do this, just add the the following content to the POM file:

{% highlight xml %}
<repositories>
	<repository>
		<id>metaborg-release-repo</id>
		<url>http://artifacts.metaborg.org/content/repositories/releases/</url>
		<releases>
			<enabled>true</enabled>
		</releases>
		<snapshots>
			<enabled>false</enabled>
		</snapshots>
	</repository>
	<repository>
		<id>metaborg-snapshot-repo</id>
		<url>http://artifacts.metaborg.org/content/repositories/snapshots/</url>
		<releases>
			<enabled>false</enabled>
		</releases>
		<snapshots>
			<enabled>true</enabled>
		</snapshots>
	</repository>
	<repository>
		<id>spoofax-eclipse-repo</id>
		<url>http://download.spoofax.org/update/nightly/</url>
		<layout>p2</layout>
		<releases>
			<enabled>false</enabled>
		</releases>
		<snapshots>
			<enabled>false</enabled>
		</snapshots>
	</repository>
</repositories>

<pluginRepositories>
	<pluginRepository>
		<id>metaborg-release-repo</id>
		<url>http://artifacts.metaborg.org/content/repositories/releases/</url>
		<releases>
			<enabled>true</enabled>
		</releases>
		<snapshots>
			<enabled>false</enabled>
		</snapshots>
	</pluginRepository>
	<pluginRepository>
		<id>metaborg-snapshot-repo</id>
		<url>http://artifacts.metaborg.org/content/repositories/snapshots/</url>
		<releases>
			<enabled>false</enabled>
		</releases>
		<snapshots>
			<enabled>true</enabled>
		</snapshots>
	</pluginRepository>
</pluginRepositories>
{% endhighlight %}
