---
layout: default
title: A Full Example: Planning to Place N-Queens
nav_order: 6
parent: Getting Started
has_children: false
---

In this section, we will look at a toy example of integrative AI that integrates
toy examples from constraint processing and planning.

Our problem is the following:

Given a chessboard of size `n` by `n` with `n` queens in arbitrary positions an
no other pieces, find a plan of pick and place actions that arranges the `n`
queens in a way that they do not threaten each other (i.e. they are not on the
same row, column, or diagonal).

This problem can be broken down into two steps:

1. Find a valid arrangements of `n` queens on the board.
2. Find a plan that achieves the valid arrangement from the current one.

# Find a Valid Placement (CSP)

The first step can be modelled as a *Constraint Satisfaction Problem (CSP)* [1].
A CSP consists of a set of variables with discrete and finite domains, and
constraints that must be satisfied in a solution. A solution is an assignment
of variables that satisfies all constraints.

For the n-queens problem, variables represent ways of placing the queens on the
board and constraints make sure they are not placed on the same row, column or
diagonal. This can be slightly simplified, if we use one variable for each
column of the board with one possible value for each row.

With these ideas in mind, let's create an AIDDL representation for a 4-queens
problem.

## AIDDL Module Header

First, we create an aiddl file `n-queens.aiddl` and start by specifying the
module.

    (#mod self org.aiddl.example.planning-n-queens.csp)

This is an entry with the special type `#mod` in its type field. The second
element `self` is a symbol that this module uses to refer to itself locally. The
third element `org.aiddl.example.planning-n-queens.csp` is the global name of
the module. Other modules can use this name to refer to this module.

Next, we bring a namespace into scope to be able to use the short hand of some
default functions.

    (#req eval org.aiddl.eval.namespace)
    (#nms eval-nms  basic@eval)
    
The first line above is a require entry with special type `#req`. Much like
`#mod`, the second element, `eval`, tell us how we call the required module
locally (in this module). The third element, `org.aiddl.eval.namespace`, is the
global name of the required module. This means that the `#mod` entry of the
module loaded above will look like `(#mod self
org.aiddl.example.planning-n-queens.csp)`.

The second line above is a namespace entry of special type `#nms`. It loads the
name space `basic` from the module `eval` loaded in the previous line. As with
`#mod`, the second element `eval-nms` of the entry tells us how we refer to this
namespace locally.

The `#nms` entry will perform a substitution to every entry in our file so we
can use short names or symbols for otherwise long default function names. We 
will see how this is used a bit later.

Finally, we load another module that contains the types used by our CSP.

    (#req C org.aiddl.common.reasoning.constraint)

## n-queens in AIDDL

Now we have all we need to start modelling our 4-queens problem.
So we need variables, domains, and constraints.

### Variables

We use one variable per column and put them together in a set:

    {?X1 ?X2 ?X3 ?X4}
    
### Domains

Each variable can have a value between 1 and 4:

    {1 2 3 4}
    
We associate variables to their domains in a set of key-value pairs:

    {
      ?X1:{1 2 3 4}
      ?X2:{1 2 3 4}
      ?X3:{1 2 3 4}
      ?X4:{1 2 3 4}
    }

Now, this expression repeats the same value set four times. To simplify this, we
create an entry `D` to store our value set.

    (Domain@CP D {1 2 3 4})

The type of the entry originates from the module `C`. We can then use the entry
to achieve a slightly more compact version of our domain set:

    {
      ?X1:$D
      ?X2:$D
      ?X3:$D
      ?X4:$D
    }

Here, `$D` is a reference to the local entry called `D`.
Now all we need is our set of constraints.

### Constraints

Each constraint consists of two parts: a scope and a Boolean function. The scope
tells us which variables our constraints applies to. The scope `(?X ?Y)` applies
to two variables `?X` and `?Y`. To express our constraints, we will use some
built-in functions.
The Boolean function should check if `?X` and `?Y` have different values:

    (!= ?X ?Y)
    
and if they are not on the same diagonal:

    (!= ($abs (- ?X ?Y)) ?D)
    
The idea of this expression is to take the absolute difference between the
values assigned to `?X` and `?Y` and make sure that it does not equal a value
`?D`. `?D` is the distance between the colums of the variables. So, for
instance, variables in column `1` and `3` cannot be assigned values whose
absolute difference is `2`. So `?D` for `?X1` and `?X3` should be `2`.  The
functions `!=` and `-` are brought in with our namespace above and will be
replaced internally by rather long symbols with unique names (e.g.,
`org.aiddl.eval.not-equals`). The motivation behind this is to not "block" any
of the short operator symbols and allow them to be used in types without
unintentionally evaluated terms.

Due to the way CSPs are solved, we may not always have a value assigned to each
variable. If this is the case, we cannot tell yet if a constraint is satisfied. 
To cover this case, we use the expression

    (has-type ?X ^variable)

which will return `true` if `?X` has type `variable`. If we put it all together
with boolean operators `or` and `and` for `?X1` and `?X2` we get the following
expression:

    (or
      (has-type ?X1 ^variable)
      (has-type ?X3 ^variable)
      (and
        (!= ?X1 ?X3)
        (!= ($abs (- ?X1 ?X3)) 3)))

We can wrap this in a function definition entry with special type `#def`.

    (#def (f-con-1-3 ?X1 ?X3)
      (or
        (has-type ?X1 ^variable)
        (has-type ?X3 ^variable)
        (and
          (!= ?X1 ?X3)
          (!= ($abs (- ?X1 ?X3)) 3)))
          
       
Constraints are assembled by putting a the scope and a reference to 
the function in a tuple:

    ((?X1 ?X3) ^$f-con-1-3)
    
### Putting it all Together
 
After creating constraints for each variable pair, we can finally put all these
parts together to assemble a CSP as a tuple of three key-value pairs for
variables, domains, and constraints. Keys and values are separated by a `:`.

    (Problem@C
      csp
      (
        variables:{?X1 ?X2 ?X3 ?X4}
        domains:{
          ?X1:$D
          ?X2:$D
          ?X3:$D
          ?X4:$D
        }
        constraints:{
          ((?X1 ?X2) ^$f-con-1-2)
          ((?X1 ?X3) ^$f-con-1-3)
          ((?X1 ?X4) ^$f-con-1-4)
          ((?X2 ?X3) ^$f-con-2-3)
          ((?X2 ?X4) ^$f-con-2-4)
          ((?X3 ?X4) ^$f-con-3-4)
        }))

This should do the trick, but we have a lot of redundancy if we write a function
for each pair of variables. What we really want is a generator for constraints
between variables. We can achieve this by using a lambda function:

    (#def (diagonal ?X ?Y ?D)
      (lambda (?X ?Y)
        (or
          (has-type ?X ^variable)
          (has-type ?Y ^variable)
          (and
            (!= ?X ?Y)
            (!= ($abs (- ?X ?Y)) ?D)))))

Instead of a Boolean value, the function `diagonal` returns an anonymous
function binding `?X` and `?Y` to two variables and `?D` to the distance. This
anonymour function takes the role of our constraint test, so the `diagonal`
function works as a constraint generator. This allows us to rewrite our CSP as
follows:

    (Problem@C
      csp
      (
        variables:{?X1 ?X2 ?X3 ?X4}
        domains:{
          ?X1:$D
          ?X2:$D
          ?X3:$D
          ?X4:$D
        }
        constraints:{
          ((?X1 ?X2) ($diagonal ?X1 ?X2 1))
          ((?X1 ?X3) ($diagonal ?X1 ?X3 2))
          ((?X1 ?X4) ($diagonal ?X1 ?X4 3))
          ((?X2 ?X3) ($diagonal ?X2 ?X3 1))
          ((?X2 ?X4) ($diagonal ?X2 ?X4 2))
          ((?X3 ?X4) ($diagonal ?X3 ?X4 1))
        }))

To get the full CSP, the value of the above term has to be evaluated.  Once this
is done, all appearances of the domain `$D` will be replaced by `{1 2 3 4}` and
all calls to `$diagonal` will be replaced by references to Boolean functions
that will evaluate our constraints.

## Solving the CSP with AIDDL Common

First, we create an empty container and parse the file containing our problem
into it. We remember the name of the parsed module, so we can access its entries
later.

    val db = new Container
    val modCsp = Parser.parseInto("../aiddl/n-queens.aiddl", db)
    
Then, we access the entry named `csp` from the loaded module and ask for its
processed value. Any functions or references in our CSP will be evaluated in the
process. This means that in the resulting value, all six calls to diagonal will
be replaced with references to to functions that can then be used as
constraints.
    
    val csp = db.getProcessedValueOrPanic(modCsp, Sym("csp"))
    
Finally, we create the CSP solver and use it to find a solution. 

    val cspSolver = new CspSolver
    cspSolver.init(csp)
    val a = cspSolver.search
 
The content of `a` after is an `Option` that should contain an assignment of all
variables that does not violate any constraint:

    Some(List(?X1:2, ?X2:4, ?X3:1, ?X4:3))
    
The solver keeps its state. So running `search` again will backtrack from the
previous solution. When there are no more solutions, it will return `None`.
If we run the following two lines after the first search above

    println(cspSolver.search)
    println(cspSolver.search)
    
we get the following output:
    
    Some(List(?X1:3, ?X2:1, ?X3:4, ?X4:2))
    None
    
    
## Planning to Place n-queens

The second step is to define our planning problem.  For this we need to define
an initial state (a board with some queens in random places and the state of the
gripper), some idea of what the system can do (pick and place queens with the
gripper), and a goal (a target configuration of queens).

State and goals are expressed as sets of state-variable assignments (represented
with key-value pairs). The board is represented with a state-variable `(board ?i
?j)` for each space on the board. These variables have two possible values
`free` and `queen`.  The only other state-variable is `gripper-free` which can
take the values `true` or `false`.

With this, we can write our initial state for four queens as:

    (State@SVP S0
      {
        (board 1 1) : queen
        (board 1 2) : queen
        (board 1 3) : queen
        (board 1 4) : queen
        (board 2 1) : free
        (board 2 2) : free
        (board 2 3) : free
        (board 2 4) : free
        (board 3 1) : free
        (board 3 2) : free
        (board 3 3) : free
        (board 3 4) : free
        (board 4 1) : free
        (board 4 2) : free
        (board 4 3) : free
        (board 4 4) : free
        
        gripper-free : true
      })

Operators allow us to model what a system can do to change a state. In the
simple case, they come with a name, a set of preconditions, and a set of
effects. Preconditions tell us in which state an operator can be used, while
effects tell us how the state changes. Both take the same format as a state
itself: a set of state-variable assignments.
To avoid having to write two operators for each row and column of the board, we use variables that usually also appear in the name of the operator.

The operator `pick` can be used on any field `?r` `?c` that contains a queen if
the gripper is free. Its effect is that the field becomes free and the gripper
is no longer free.

    (Operator@SVP pick
      (
        name:(pick ?r ?c)
        preconditions:{
          (board ?r ?c) : queen
          gripper-free : true
        }
        effects:{
          (board ?r ?c) : free
          gripper-free : false 
        }
      )
    )

The operator `place` can be used on any field `?r` `?c` that is free if the
gripper is not free. Its effect is that the space contains a queen and the
gripper is freed.

    (Operator@SVP place
      (
        name:(place ?r ?c)
        preconditions:{
          (board ?r ?c) : free
          gripper-free : false
        }
        effects:{
          (board ?r ?c) : queen
          gripper-free : true
        }
      )
    )

Finally, the goal can be any board configuration. So if we take the output from above, we have the following goal:

    (Goal@SVP G
      {
        (board 3 1) : queen
        (board 1 2) : queen
        (board 4 3) : queen
        (board 2 4) : queen
      }
    )
    
## Solving the Planning Problem

This step is very similar to solving the CSP.

    val modPlan = Parser.parseInto("../aiddl/planning.aiddl", db)
    val planningProblem = db.getProcessedValueOrPanic(modPlan, Sym("problem"))
  
    val forwardPlanner = new ForwardSearchPlanIterator
    forwardPlanner.init(planningProblemSubstituted)
  
    val answer = forwardPlanner.search
    
    answer match {
      case Some(plan) => plan.foreach(println)
      case None => println("No plan found.")
    }

This should produce the following output:

    (pick 1 1)
    (place 4 4)
    (pick 1 3)
    (place 4 2)
    (pick 1 2)
    (place 1 3)
    (pick 1 4)
    (place 3 4)
    (pick 4 4)
    (place 2 1)
    
    
## Integrating Constraint Processing and Planning
    
Now that we have both ends of the problem in the same language, 
actually integrating them becomes fairly straight forward.
If we change our goal to

    (Goal@SVP G
      {
        (board ?X1 1) : queen
        (board ?X2 2) : queen
        (board ?X3 3) : queen
        (board ?X4 4) : queen
      }
    )
    
we can substitute the assignments provided by the CSP solver to decide where the
queens should be placed. For this, we can create a `Substitution` object from
our assignment (here, the `get` unpacks the `Option` and would crash the program
if no solution exists):

    val substitution = Substitution.from(ListTerm(a.get))
    
Finally, we apply the substitution to our planning problem.

    val planningProblemSubstituted = planningProblem \ substitution

This connects both ends and leads to the same planning problem we solved
previously, only that the solution is provided by the CSP solver.

## Discussion

While the example we use here is a toy example it can easily transfered to more
useful cases. Consider, for instance, a truck that needs to be loaded with
objects of certain shapes. In this case the CSP allows us to decide where
objects should go, while a planner can be used to decide how the objects are
placed in their chosen spots. For this purpose it may be possible to further
extend the scenario to allow reasoning about the motion of our robotic arms, or
the order in which items should arrive on a conveyor belt.


# References 


[1] Dechter, Rina (2003). "Constraint processing". Elsevier Morgan Kaufmann.
