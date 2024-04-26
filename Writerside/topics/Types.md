# Types

The terms that make up the language part of AIDDL have no link to any concrete
AI problem by themselves. These links are established by defining types within
the language. In this way we can establish different fragments of AIDDL that
express well known problems.

AIDDL types are expressed within AIDDL through the expressions detailed
below. We first go over the basic builing blocks and then discuss how
constraints can be attached to types, as well as how generic types can be
defined in AIDDL.

## Basic Types 

Basic types do not need to be defined and can always be used with the references
listed in the table below.  Shorter names can be used with the help of
namespaces. Custom types often utilize basic types.

| Name               | Reference                                | Description                                                            | Examples                                        |
|--------------------|------------------------------------------|------------------------------------------------------------------------|-------------------------------------------------|
| Term               | `org.aiddl.type.term`                    | Every AIDDL expression is a term.                                      | see below                                       |
| Symbolic           | `org.aiddl.type.term.symbolic`           | Non-numerical constants.                                               | `a` `e1` `+` `#integer`                         |
| Boolean            | `org.aiddl.type.term.symbolic.boolean`   | Boolean constants                                                      | `true`  `false`                                 |
| Variable           | `org.aiddl.type.term.variable`           | Named variables beginning with `?` or anonymous variables `_`          | `?x` `?e1` `_`                                  |
| String             | `org.aiddl.type.term.string`             | Any string of characters in quotes.                                    | `"a"` `"abc"` `"1 2 3"`                         |
| Numerical          | `org.aiddl.type.term.numerical`          | Different types of numerical values.                                   | see below                                       |
| Integer            | `org.aiddl.type.term.numerical.integer`  | Positive or negative integers.                                         | `0` `-3` `11`                                   |
| Rational           | `org.aiddl.type.term.numerical.rational` | Positive or negative rational numbers.                                 | `0/1` `-1/3` `110/13`                           |
| Real               | `org.aiddl.type.term.numerical.real`     | Positive or negative real numbers.                                     | `0.0` `-1.3` `1.1`                              |
| Infinity           | `org.aiddl.type.term.numerical.inf`      | Positive or negative infinity.                                         | `INF` `+INF` `-INF`                             |
| NaN                | `org.aiddl.type.term.numerical.nan`      | Not a Number                                                           | `NaN`                                           |
| Collection         | `org.aiddl.type.term.collection`         | Collections of terms.                                                  | see below                                       |
| Set                | `org.aiddl.type.term.collection.set`     | A set of terms. Cannot be matched to other terms.                      | `{}` `{e1 e2 e3}` `{1 1 2}`                     |
| List               | `org.aiddl.type.term.collection.list`    | A list of terms.                                                       | `[]` `[e1 e2 e3]` `[1 1 2]`                     |
| Tuple              | `org.aiddl.type.term.tuple`              | A tuple of terms. Unlike lists, we assume tuples will not be extended. | `()` `(e1 e2 e3)` `(1 1 2)`                     |
| Reference          | `org.aiddl.type.term.reference`          | A reference to an entry in a specific module.                          | `e@m` references entry named `e` in module `m`  |
| Function Reference | `org.aiddl.type.term.fun-ref`            | Reference to a function. Allows using functions as data.               | `^org.aiddl.eval.add`                           |
|                    |                                          |                                                                        | `^f@m` references function `f` in module `m`    |
| Key Value Pair     | `org.aiddl.type.term.key-value`          | A key and a value term. The key must not be a Key-value pair itself.   | `x:10` symbolic key `x` with integer value `10` |

## Type Expressions

Types can be specified in AIDDL via `#type` entries that specify a name and a
type definition. Assuming a symbolic name for the type, the new types URI will
be its module URI concatenated with the type name. Defining a type will create a
membership function with the type's URI. This function can be used to test if a
term satisfies a type.

The following example defines a new type called `org.aiddl.my-module.new-type`
which is an AIDDL set.

    (#mod self org.aiddl.my-module)
    
    (#type new-type ^org.aiddl.type.term.collection.set)

This example can be seen as simply renaming the AIDDL set type.  The `^` signals
that the following is a function reference. This would allow us to write an
entry

    (^$new-type S {a b c})

using a function reference to the name of the type in the type slot of the
entry.

Any basic AIDDL type can be used in a type definition, but in most cases we
would like to be more precise. For this, we can employ a combination of type
specifiers. In the following examples, we assume the basic namespace to make the
definitions more compact.

    (#req eval org.aiddl.eval.namespace)
    (#nms base-nms basic@eval)

The following sections go over a variety of commonly used type constructs.

### Enumerations

*Enumeration* types define a type through a list of possible terms.

    (#type enum-type (type.enum {a b ?x (f 10)}))

### Numerical Ranges

*Range* types specify a numerical range.

    (#type range-type (type.range min:0 max:10))

### Typed Collections

*Collection* types limit collections, lists or sets to elements of a specific
type. This is the first example of a type that includes other types.

    (#type set-of-sym (type.set ^symbolic))

Whenever a type is expected, we can define it directly instead of using a named
type.

    (#type set-of-enum (type.set (type.enum {a b c})))

If we use a defined type, we can use the full URI, a self reference (if defined
in the current module) or an entry reference. The following three definitions
are identical in the current module:

    (#type set-of-enum-a (type.set ^$enum-type))
    (#type set-of-enum-b (type.set ^enum-type@self))
    (#type set-of-enum-c (type.set ^org.aiddl.my-module.enum-type))

This works because after a `^` all references are translated to the full
symbolic URI. Similar types can be defined with `type.list` for typed lists and
`type.col` for typed lists or sets.

### Unions and Intersections

We can also specify type *unions* and *intersections*. A union of two or more
types creates a type that include the members of all the given types. The
following type will consist of all terms that are variables or symbols:

    (#type sym-or-val (type.union ^variable ^symbolic))

An intersection of two or more given types creates a type whose members belong
to every individual type. The following example creates an intersection between
to enum types. The final type will consist only of the symbols `b` and `c`.

    (#type t_a (type.enum {a b c}))
    (#type t_b (type.enum {  b c d e}))
    
    (#type t (type.inter [^$t_a ^$t_b]))

### Typed Key-value Pairs

A *typed key-value pair* allow us to restrict the type of the key and value of a
key-value pair. The following example creates a type that includes all key-value
pairs whose keys are symbols and whose values are numerical:

    (#type t_kvp (type.kvp ^symbolic:^numerical))

### Signed Tuples

A *signed tuple* gives a list of types for the corresponding fields in a
tuple. The following type contains all tuples of length three containing a
numerical term in the first and third field, and a symbolic term in the last
field:

    (#type t_sig (type.sig [^numerical ^symbolic ^numerical]))

Signed tuples also allow to specify minimum length, maximum length and how the
last elements are repeated. The following type constains all tuples with minimum
length five and whose fields alternate between symbolic and numerical starting
at the fourth field.

    (#type t_sig (type.sig [^numerical ^symbolic ^numerical] min:5 max:+INF repeat:2))

### Dictionary

*Dictionaries* allow to specify types for tuples or collections that contain types
behind certain keys.  The following type includes all tuples or collections that
have a key `from` and a key `to` pointing to a symbol and a key `weight`
pointing to a numerical:

    (#type t_edge (type.dict [from:^symbolic to:^symbolic weight:^symbolic]))

### Matrix

*Matrix* types can be used to define lists/tuples of lists/tuples. We can define
types by row, column, or for each cell and restrict the size.  The following
example defines a type for a 3-by-3 matrix of symbolic terms:

    (#type t_mat (type.matrix m:3 n:3 cell-type:^symbolic))

The next example instead specifies types for the rows in the matrix.  Each row
consists of a symbolic, a variable, and a numerical term in this order:

    (#type t_mat (type.matrix m:3 n:3 row-types:[^symbolic ^variable ^numerical]))

The final example applies the same idea to the colums of the matrix:

    (#type t_mat (type.matrix m:3 n:3 col-types:[^symbolic ^variable ^numerical]))

If `m` and `row-types` are provided we assume `m` to equal the number of row
types (and the same for `n` and `col-types`). Otherwise the type is empty.

## Constraints

Any type definition can be provided with a reference to a Boolean function that
will be treated as a constraint on a term. The following type contains all
tuples of symbols and of length two whose elements are not equal.

    (#type (type.sig [^symbolic ^symbolic] constraint:^!=))

## Generic Types

If a type is determined in terms of one or more other types, we can define a
generic type by using a tuple with the name of the type constructorfollowed by
variables to represent generic types used elsewhere in the type definition.

Suppose we want to define a generic type `pair` of tuples of length two without
deciding its field types. The following accomplishes this:

    (#type (pair ?A ?B) (type.sig [?A ?B]))

Rather than a type the above should be seen as a type constructor.  Internally,
a function `org.aiddl.my-module.pair` will be generated that takes two arguments
and returns a type function. As a result, we can use the following to create a
set of all pairs of symbolic terms.

    (#type (type.set ($pair ^symbolic ^symbolic)))

When

    ($pair ^symbolic ^symbolic)

is evaluated, it will be replaced by

    `^org.aiddl.my-module.pair.1`

which is an actual type that can be used by the type checker. The name above may
change depending on the implementation or the order in which types are
constructed from a generic type.
