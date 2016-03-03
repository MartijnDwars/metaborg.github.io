Stratego is a language and toolset for [program transformation](http://strategoxt.org/Transform/ProgramTransformation). The Stratego language provides rewrite rules for expressing basic transformations, programmable rewriting strategies for controlling the application of rules, concrete syntax for expressing the patterns of rules in the syntax of the object language, and dynamic rewrite rules for expressing context-sensitive transformations, thus supporting the development of transformation components at a high level of abstraction.

## Documentation

We are working to migrate and update the old Stratego wiki from the old site <http://strategoxt.org>. Perhaps one of these quick links will help you now:

* [Stratego Language](language/)
* [Stratego API](http://releases.strategoxt.org/docs/api/)
* Instructions for [calling into Java code from Stratego](/stratego/external-strategies/)
* The [Stratego issue tracker](http://yellowgrass.org/project/StrategoXT). Please note that Spoofax issues are reported separately.

## Download

The Stratego language and tools are included in [Spoofax](/download/) installations. You can obtain a stand-alone distribution of Stratego for Java from [Stratego's GitHub repository](https://github.com/metaborg/strategoxt/releases/tag/baseline). You only need to download *strategoxt.jar*.

## Contact

* Join the [users@strategoxt.org mailing list for questions](https://mailman.st.ewi.tudelft.nl/listinfo/users) for help in using Stratego.
* Join the [developers@strategoxt.org](https://mailman.st.ewi.tudelft.nl/listinfo/developers) if you are or are interesting in contributing to Stratego.
* Join the IRC channel at [#stratego on freenode.net](irc://irc.freenode.net/#stratego) ([web version](http://webchat.freenode.net/?channels=stratego)). Note that most Stratego developers are in the [CET](http://www.timeanddate.com/worldclock/custom.html?cities=16) timezone.

## Obtaining the source, building from source and contributing

[Stratego's GitHub repository](https://github.com/metaborg/strategoxt) manages all of Stratego's source code. The Stratego for C version (not actively maintained but working) is in the [*master*](https://github.com/metaborg/strategoxt/tree/master) branch. The Stratego for Java version is kept in the [*java-bootstrap*](https://github.com/metaborg/strategoxt/tree/java-bootstrap) branch.

### Building from source

The instructions for the Java version on Linux and Apple OS X accompany the source code, and can be accessed directly [here](https://github.com/metaborg/strategoxt/blob/java-bootstrap/strategoxt/BUILDING.md). Internet connectivity is required to initialise the build environment.

### Contributing

We welcome contributions and discussions for improvements, please see above for contact information. For external contributions we follow the GitHub fork, develop, submit pull request model. By submitting a pull request you acknowledge that you transfer copyright of the code you contribute and that your merged contribution will inherit the Stratego license.

## License

Stratego is licensed under the [GNU Lesser General Public License version 2.1](http://www.tldrlegal.com/l/lgpl2).


