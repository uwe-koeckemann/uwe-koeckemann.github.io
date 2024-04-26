# Using AIDDL Common

AIDDL common provides implementations and types for common AI and related
algorithms.

## Example 1: Solving a Traveling Salesperson Problem

Consider the following entry which describes a Traveling Salesperson Problem
(TSP) in which we have to find a shortest path that visits each node exactly
once and returns to the starting node.

    (^Problem@TSP problem
      ( 
       V:{n1 n2 n3 n4 n5}
         E  :    {
           {n1 n2}
           {n1 n3}
           {n1 n4}
           {n1 n5}
           {n2 n3}
           {n2 n4}
           {n2 n5}
           {n3 n4}
           {n3 n5}
           {n4 n5}
         }
      
         weights  :    {
           {n1 n2}:105
           {n1 n3}:280
           {n1 n4}:219
           {n1 n5}:207
           {n2 n3}:385
           {n2 n4}:114
           {n2 n5}:102
           {n3 n4}:499
           {n3 n5}:487
           {n4 n5}:12
         }
      ) 
    )
    
Assuming this entry is stored in a file called `tsp-n05-01.aiddl`, we can load
it and solve the problem with AIDDL common with the following short Scala
program (taken from one of the AIDDL common test cases).

The first block creates a container and parser, loads the file, typechecks the
module and finally retrieves the entry named `problem` from it.

``` scala
val c = new Container()
val parser = new Parser(c)
val m = parser.parseFile("tsp-n05-01.aiddl")
assert(c.typeCheckModule(m))
val p = c.getProcessedValueOrPanic(m, Sym("problem"))
``` 
 
The second block creates the solver, initializes it with the poblem and
retrieves the optimal solution.

``` scala
    val tspSolver = new TspSolver
    tspSolver.init(p)
    val sol = tspSolver.optimal
    assert( sol.get != Common.NIL )
    assert( tspSolver.best == Num(998) )
``` 

## Example 2: Solving a Knapsack Problem

Say we have the following entry with name `problem` whose value is a term
defining a Knapsack problem with 10 items of different values and weights. We
can take at most three of a single item and have a capacity of 100 wight units
to fill.

    (^Problem-Bounded@KS problem
      {
        capacity:100
        per-item-limit:3
        items:
          [
            (name:i1 value:26 weight:4)
            (name:i2 value:22 weight:5)
            (name:i3 value:13 weight:5)
            (name:i4 value:2 weight:22)
            (name:i5 value:16 weight:6)
            (name:i6 value:9 weight:20)
            (name:i7 value:14 weight:18)
            (name:i8 value:6 weight:9)
            (name:i9 value:2 weight:22)
            (name:i10 value:6 weight:15)
          ]
      })

Notice that below, we actually create our solver from the `BranchAndBound` trait
by providing a variable ordering, value ordering, an estimator for remaining
costs, and turning off propagation. 

``` scala
    val module = parser.parseFile("knapsack-04.aiddl")
    val problem = c.getProcessedValueOrPanic(module, Sym("problem"))
    
    object coSolver extends BranchAndBound {
      setRemainingCostFunction(Knapsack.remainingCostEstGeneratorFillGreedy(problem))
      staticVariableOrdering = Knapsack.variableOrderingGenerator(problem)
      staticValueOrdering = Knapsack.valueOrdering
      usePropagation = false
    }

    coSolver.init(converter(problem))
    val aco = coSolver.optimal
    assert(coSolver.best == Num(259))
```