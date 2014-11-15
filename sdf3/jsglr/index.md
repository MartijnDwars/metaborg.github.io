---
title: "JSGLR"
layout: page
modified: 
excerpt: ""
image:
  feature: 
  credit: 
  creditlink: 
context: sdf3
---

<section id="table-of-contents" class="toc"> 
  <header> <h3>Overview</h3> </header>
  <div id="drawer" markdown="1">
  *  Auto generated table of contents
  {:toc}
  </div>
</section><!-- /#table-of-contents -->

## Invoking JSGLR from command line

Assume you have a parse table file (.tbl) and you want to obtain an AST and/or parse messages for a file. Begin by downloading a build of Spoofax Sunshine from [here](https://github.com/metaborg/spoofax-sunshine/releases/latest). This Jar must be on the classpath of your Java application.

The class below expects a path to a parse table, a start symbol and a file to parse as CLI arguments. It will attempt to call the parser, emit any errors discovered and print the AST produced if any:

    import org.spoofax.interpreter.terms.IStrategoTerm;
    import org.spoofax.sunshine.model.messages.IMessage;
    import org.spoofax.sunshine.parser.model.IParseTableProvider;
    import org.spoofax.sunshine.parser.model.ParserConfig;
    import org.spoofax.sunshine.services.analyzer.AnalysisResult;
    import org.spoofax.sunshine.services.parser.FileBasedParseTableProvider;
    import org.spoofax.sunshine.services.parser.JSGLRI;
    
    public class ParseTest {
        public static void main(String[] args) {
            int parserTimeoutMillis = 3000;
    
            File parseTableFile = new File(args[0]);
            String startSymbol = args[1];
            File fileToParse = new File(args[2]);
    
            ServiceRegistry.INSTANCE().registerService(LaunchConfiguration.class,
                    new LaunchConfiguration(null, null));
    
            IParseTableProvider tableProvier = new PathBasedParseTableProvider(
                    parseTableFile.toPath());
    
            ParserConfig config = new ParserConfig(startSymbol, tableProvier,
                    parserTimeoutMillis);
    
            JSGLRI parser = new JSGLRI(config, fileToParse);
    
            AnalysisFileResult result = parser.parse();
    
            for (IMessage message : result.messages()) {
                System.err.println(message);
            }
    
            System.out.println(result.ast());
        }
    }

## Note on performance

If you are using the method above to time measurements, please be aware that the following factors will skew the measured time:

1. JVM startup & warmup
2. Class loading times
3. Indirections introduced by Spoofax Sunshine.
4. JSGLR has to read and load the parse table for every parsed file. 
