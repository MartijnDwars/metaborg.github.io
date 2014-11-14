# TS: Type System Specification

Types "categorize objects according to their usage and behaviour [Cardelli & Wegner 1985](http://researchr.org/publication/CardelliW85 "Cardelli & Wegner 1985").” A _type system_ formalizes this categorization, in order to ensure that only the intended operations are used on the representation of values of a type, avoiding run-time errors [Pierce 2002](http://researchr.org/publication/Pierce2002 "Pierce 2002"). 
Typical formalizations of type systems tangle the name resolution with type analysis, the computation of types of expressions. 
_TS_ is a high-level language for the declarative specification of type analysis that is complementary to the name analysis expressed in [NaBL](https://github.com/metaborg/doc/blob/master/meta-languages/nabl/NaBL.md "NaBL").

## Example

The following TS module defines the type rules for PCF:

	module types
	
	type rules // binding
	
	  Var(x) : t
	  where definition of x : t
	  
	  Param(x, t) : t
	  
	  Fun(p, e) : FunType(tp, te)
	  where p : tp and e : te 
	   
	  App(e1, e2) : tr
	  where e1 : FunType(tf, tr) and e2 : ta
	    and tf == ta
	        else error "type mismatch" on e2
	
	  Fix(p, e) : tp
	  where p : tp and e : te 
	    and tp == te
	        else error "type mismatch" on p
	 
	  Let(x, tx, e1, e2) : t2
	  where e2 : t2 and e1 : t1 
	    and t1 == tx
	        else error "type mismatch" on e1
	
	type rules // arithmetic 
	
	  Num(i) : IntType()
	
	  Ifz(e1, e2, e3) : t2
	  where e1 : IntType() and e2 : t2 and e3 : t3
	    and t2 == t3 
	        else error "types not compatible" on e3
	  
	  e@Add(e1, e2) 
	  +  e@Sub(e1, e2) 
	  + e@Mul(e1, e2) : IntType()
	  where e1 : IntType() 
	        else error "Int type expected" on e
	    and e2 : IntType()
	        else error "Int type expected" on e


Type rules define judgments of the form `p : t` stating that term `p` has type `t`. Terms and types are abstract syntax trees based on the syntax definition. The `where` clause of a rule defines the conditions under which the judgment holds. For example, consider the rule for function application (`App`) above.
The rule defines that `App(e1, e2)` has type `tr`, provided that 
the first argument `e1` has function type `FunType(tf, tr)`, the second argument `e2` has type `ta`, and that the formal argument and the actual argument have the same type (`tf == ta`). The `else` branch on that equation generates a type error for use in the IDE on target term `e2`.

Type rules are complementary to name binding. This means that type rules do not have to propagate typing environments. 
Assuming that name binding has been performed, the type rule for variables (`Var`) can obtain the type of a variable by simply referring to the `definition of x`. 
Note that in the name binding rule for `Param` above we associated the type from the parameter declaration to the definition of a variable.

The rules for PCF suggest that name resolution and type analysis can be staged sequentially. In general, however, name binding may depend on the results of type analysis. For example, in a class-based object-oriented language, resolving a method call expression `e.f(e1,..,en)` requires knowing the type of the receiver `e` to determine the target class, and may require knowing the types of the argument expressions to resolve overloading.

## Type Functions

## Type Relations

