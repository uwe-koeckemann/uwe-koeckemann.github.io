---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
nav_order: 0
---

# What is AIDDL?

AIDDL (short for *Artificial Intelligence Domain Definition Language*) is a language and framework for Integrative AI.

Our aim is to provide a simple way to express many different AI problems (such
as automated planning, reasoning, learning, constraint processing) in the same
language and allow to integrate problems without imposing any assumptions on how
integration is performed.

The *AIDDL Framework* refers to core and common libraries. An *AIDDL Core*
offers parsing and various functionality for manipulating AIDDL terms in a
concrete programming language. *AIDDL Common* refers to libraries of common
algorithms that can be used to solve basic problems written in AIDDL or composed
into integrated AI systems.

# Example: Planning with Constraints

Suppose we have a simple planning problem with three robots in three different
locations and a single operators that allows to move robots between locations.
An AIDDL version of this description is shown below.

    initial-state:{
     (at r1):l1
     (at r2):l2
     (at r3):l3
    }
    
    goal:{
     (at r1):l2
     (at r2):l3
     (at r3):l1
    }
    
    operators:{
    (name:(move ?r ?a ?b)
     preconditions:{
      (at ?r):?a
     }
     effects:{
      (at ?r):?b
     })


Now, let's say we have a requirement that only two robots can be in a single
location at once. This can be expressed as a Boolean satisfiability problem.
This could be written in AIDDL as follows.

    (and
      (or (not (at r1):l1) (not (at r2):l1) (not (at r3):l1))
      (or (not (at r1):l2) (not (at r2):l2) (not (at r3):l2))
      (or (not (at r1):l3) (not (at r2):l3) (not (at r3):l3))
    )

How do we combine these two? We could use the constraints to prune states from
the search space of a planner. We could compile the planning problem into a
satisfiability problem and solve it together with our constraints. We could
compile the constraints into preconditions of operators. There are numerous ways
in which this could be handled.  The idea behind AIDDL is to facilitate
integration without imposing any preference on how this is done. Many solutions
to this simple problem may share components but assemble them in different ways.
The main aim of AIDDL is to make this as easy as possible.


