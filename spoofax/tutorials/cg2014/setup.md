# Setup

## Spoofax Installation

We highly recommend to work with a fresh Eclipse and Spoofax installation. You can download a copy for your OS and architecture:

* [MacOSX 64 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-macosx-x64-jre.zip)
* [Linux 64 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-linux-x64-jre.zip)
* [Linux 32 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-linux-x86-jre.zip)
* [Windows 64 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-windows-x64-jre.zip)
* [Windows 32 bit](http://buildfarm.metaborg.org/job/spoofax-master/lastSuccessfulBuild/artifact/dist/spoofax-windows-x86-jre.zip)

## Initial Spoofax Project

You should start with a fresh Eclipse workspace and import the prepared Spoofax projects into the workspace. You can either [clone the git repository](https://github.com/metaborg/lwc2013) or [download the archive](http://download.spoofax.org/update/tutorial/spoofax-hands-on-ipa2015.zip).

1. Start Eclipse.
2. Select a fresh workspace.
3. Import the initial Spoofax projects into your workspace:
    1. Right-click into the Package Explorer.
    2. Select **Import...** from the context menu.
    3. Choose **Existing Projects into Workspace...** from the list.
    4. Specify the **Root directory**.
    5. Import all projects into your workspace by pressing the **Finish** button.
    6. Close the `org.spoofax.lang.lwc.ql.syntax`, `org.spoofax.lang.lwc.ql.analysis` and `org.spoofax.lang.lwc.ql.full` projects.
    6. Select the `org.spoofax.lang.lwc.ql.empty` project.
    7. Build the project by choosing **Build Project** from the **Project** menu or by pressing the corresponding keyboard shortcut (`Ctrl+Alt+B`, or `Cmd+Alt+B` for MacOSX).
    8. Restart Eclipse by going to the **File** menu and choosing **Restart**.

The session continues with the description of the QL language.
