# Going Further: Writing AI Algorithms

The common library makes heavy use of three interfaces that cover a wide range of algorithms. 

The following questions cover these three interfaces:

- Can the algorithm be implemented as a pure function?
- Can the algorithm be called multiple times to return alternative solutions to the same problem?
- Does the algorithm have parameters?

## Function Interface

The core library provides a simple interface for AI algorithm which takes a term as input and 
returns a term as a result.

To implement a learning algorithm, your input may be a (labeled) data set and your output would a model 
(such as a decision tree). 

For a reasoning algorithm, the input could contain a knowledge base and a query and the output could 
be one or more answers to the query.

In case of planning, the input would contain an initial state, a goal and a set of operators and the output is 
a plan or a symbol indicating failure in case no plan exists.

## Initializable

Some algorithms behave like iterators in the sense that they can be asked for further solutions. 
In case of planning, the first plan may be rejected by a user. In such cases we may want to continue search
(possibly after adding some additional constraints).

For this purpose, the intilizable interface exists to allow functions to be set to a clear initial state
and then support multiple calls of the actual function (e.g., to get the next solution).

Example: search algorithms are initialized with a set of possible start nodes that should be used as 
origins of the search.

## Configuration

Many algorithms include parameters that may influence performance on specific problems. 
For this purpose the configurable interface exists which allows a common way to configure any AIDDL common algorithm
through a single AIDDL term (which can contain an arbitrary amount of parameters if needed).

This covers learning rates, neural network layouts, thresholds, or maximum iterations for stochastic search algorithms.

## Other Interfaces / Traits



### Graph Search



### Tree Search

Generic trait that allows searching in a tree

### Learner

Extension of the function interface which adds methods for fitting and prediction.


