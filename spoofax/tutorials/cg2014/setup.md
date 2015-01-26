# Setup

## Spoofax Installation

We highly recommend to work with a fresh Eclipse and Spoofax installation. You can get a copy of Eclipse with Spoofax pre-installed from the USB sticks we distributed in the room. Alternatively, you can download a copy for your OS and architecture:

| OS      | Arch   | Link                                                                        |
| ------- | ------ | --------------------------------------------------------------------------- |
| MacOSX  | 64 bit | http://download.metaborg.org/update/installations/spoofax-macosx-x86_64.zip |
| Linux   | 64 bit | http://download.metaborg.org/update/installations/spoofax-linux-x86_64.zip  |
| Linux   | 32 bit | http://download.metaborg.org/update/installations/spoofax-linux-x86.zip     |
| Windows | 64 bit | http://download.metaborg.org/update/installations/spoofax-win32-x86_64.zip  |
| Windows | 32 bit | http://download.metaborg.org/update/installations/spoofax-win32-x86.zip     |


## Initial Spoofax Project

You should start with a fresh Eclipse workspace and import the prepared Spoofax projects into the workspace. You can either [download the archive](http://download.metaborg.org/update/tutorial/spoofax-hands-on-ipa2015.zip) or get it from the USB sticks (file name `spoofax-hands-on.zip`) we distributed in the room.

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
