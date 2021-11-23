---
layout: default
title: A Full Example: Placing 4 Queens
nav_order: 2
has_children: true
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
parts together to assemble a CSP.

    (Problem@C
      csp
      (
        {?X1 ?X2 ?X3 ?X4}
        {
          ?X1:$D
          ?X2:$D
          ?X3:$D
          ?X4:$D
        }
        {
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
        {?X1 ?X2 ?X3 ?X4}
        {
          ?X1:$D
          ?X2:$D
          ?X3:$D
          ?X4:$D
        }
        {
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


# References 


[1] Dechter, Rina (2003). "Constraint processing". Elsevier Morgan Kaufmann.
