---
layout: default
title: Load a file and get a processed value
parent: Recipes & Snippets
nav_order: 1
---

Aim:

- Parse an AIDDL file
- Access and fully resolve and evaluate the value of an entry

Suppose we want to parse a file called `problem-01.aiddl` from the current directory and
get a processed version of the entry with the symbolic name `problem`. The
processed value will be fully evaluated and all references recursivly resolved.
If anything goes wrong and exception will be thrown.

# Scala

    val c = new Container()
    val modName = Parser.parseInto("./problem-01.aiddl", c)
    val problem = c.getProcessedValueOrPanic(modName, Sym("problem"))

# Java

    Container c = new Container();
	SymbolicTerm modName = Parser.parseFile( "./problem-01.aiddl", c);
    Term problem = c.getProcessedValueOrPanic(modName, Term.sym("problem"));
    
# Python

    c = Container()
    mod_name = parse("./problem-01.aiddl", c)
    problem = c.get_processed_value_or_panic(Sym("problem"), module=mod_name)

    
