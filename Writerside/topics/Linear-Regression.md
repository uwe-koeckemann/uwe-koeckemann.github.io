# Linear Regression

Linear Regression [1] fits a linear function to a numerical data set.  This
problem can be solved by the Least Mean Squares method. One interesting feature
of this approach is that it can handle more complex functions by applying them
as expansions to the data set before regression.

## Type Definition

A regression problem is a key-value tuple. The key `attributes` contains its
attributes, the key `label` contains the name of the target, and the key `data`
contains a matrix with numerical cells. The constraint imposes that the label is
one of the attributes, the matrix has the correct size, and that all data points
are of the type specified in the corresponding attributes.

```
```
{
collapsible="true" 
default-state="expanded"
collapsed-title-line-number="2" 
include-lines="26-29"
src="https://raw.githubusercontent.com/uwe-koeckemann/AIDDL/master/common/aiddl/org/aiddl/common/learning/supervised.aiddl"}


The full AIDDL type definition can be found [here](https://github.com/uwe-koeckemann/AIDDL/blob/master/common/aiddl/org/aiddl/common/learning/supervised.aiddl).

## Example Input

The following problem consists of two real values attributes `x` and `y` where
`y` is the label. The `data` matrix consists of tuples of size two. The first element is the value of `y` and the second element is the value of `x`.

```
```
{
collapsible="true"
default-state="collapsed"
collapsed-title-line-number="2"
src="https://github.com/uwe-koeckemann/AIDDL/blob/ac3641ac43fd2f2bd109e7b2f20e1a829a684202/common/test/learning/regression/problem-01.aiddl?raw=true"}

The aiddl file containing this example can be found
[here](https://github.com/uwe-koeckemann/AIDDL/blob/master/common/test/learning/regression/problem-01.aiddl).

## Implementation Overview

We solve this problem with the Ordinary Least Squares [1] method.  The trait
[Learner](https://github.com/uwe-koeckemann/AIDDL/blob/master/common/scala/src/main/scala/org/aiddl/common/scala/learning/supervised/Learner.scala)
captures some common methods of most machine learning algorithms, and wraps them
up in the `apply` method. We rely on some matrix algorithms available in AIDDL
common to solve the regression problem.

``` scala
```
{
collapsible="true"
default-state="expanded"
collapsed-title-line-number="10"
include-lines="10-"
src="https://github.com/uwe-koeckemann/AIDDL/blob/master/common/scala/src/main/scala/org/aiddl/common/scala/learning/supervised/least_squares/OrdinaryLeastSquaresRegression.scala?raw=true"}


As a result, writing the `fit` comes pretty close to how it would be described
in a textbook. The `predict` method simply multiplies a data set with the weight
vector `w` and packs the result into an AIDDL list.  The full implementation can
be found
[here](https://github.com/uwe-koeckemann/AIDDL/blob/develop/common/scala/src/main/scala/org/aiddl/common/scala/learning/supervised/least_squares/OrdinaryLeastSquaresRegression.scala).

## Try It Yourself

- Link to test case
- Commented test case code

## References

[1] Murphy, Kevin P. (2012). "Machine Learning: A Probabilistic Perspective". The MIT Press.

## TODO

- Try it Yourself section
- Link to Linear Algebra article for matrix methods
