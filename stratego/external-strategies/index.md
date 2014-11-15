---
title: Using Java Methods in Stratego
layout: page
modified: 
excerpt: ""
image:
  feature: 
  credit: 
  creditlink: 
context: stratego
---

<section id="table-of-contents" class="toc"> 
  <header> <h3>Overview</h3> </header>
  <div id="drawer" markdown="1">
  *  Auto generated table of contents
  {:toc}
  </div>
</section><!-- /#table-of-contents -->



To use Java methods from Stratego you have to wrap the methods as Stratego strategies. The four required steps are detailed below. An example of a Java strategy is automatically generated with every new project. This strategy is located at `editor/java/YOURLANGUAGE/strategies/java_strategy_0_0.java`, and is registered in `editor/java/YOURLANGUAGE/strategies/InteropRegisterer.java`.

## Implement a strategy in Java

Every Java strategy is a singleton, extends the `org.strategoxt.lang.Strategy` class, and overrides its `invoke` method:

	public class java_strategy_0_0 extends Strategy {

	  public static java_strategy_0_0 instance = new java_strategy_0_0();
	
	  @Override
	  public IStrategoTerm invoke(Context context, IStrategoTerm current) {
	    context.getIOAgent().printError("Input for java-strategy: " + current);
	    ITermFactory factory = context.getFactory();
	    return factory.makeString("Regards from java-strategy");
	  }
	}

The name of the strategy must indicate the number of strategy and term parameters required by the strategy. For example, `java_strategy_0_0` implies that the strategy takes no arguments. This is also reflected in the signature of the `invoke` method which only takes two parameters:

1. the current execution context
2. the current term

As with native Stratego strategies, each strategy defined in Java receives as input the current term and must return the same or another term. For example, the `java_strategy_0_0` prints the current term and returns a string as the new current term.

## Register the strategy in Java

All strategies manually defined in Java have to be registered. This is done by adding an *instance* of the strategy to the `InteropRegisterer` class. For example:

	public class InteropRegisterer extends JavaInteropRegisterer {
	
	  public InteropRegisterer() {
	    super(new Strategy[] { java_strategy_0_0.instance });
	  }
	}

## Declare the strategy in Stratego

Before the external strategy can be used in Stratego, it needs to be declared as an *external* strategy. This can be done anywhere in a Stratego file by using the `external` keyword. For example:

	external java-strategy(|)

Note that dashes (-) in the Stratego strategy name correspond to underscores (\_) in the name of the Java class implementing the strategy, i.e. `java-strategy` in Stratego corresponds to `java_strategy` in Java.

## Use the strategy in Stratego

After being implemented and registered in Java and declared in Stratego, the external strategy can be used as any other Stratego strategy.
