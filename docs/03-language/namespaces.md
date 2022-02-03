---
layout: default
title: Namespaces
parent: The Language
nav_order: 4
---

Namespaces allow to avoid extensive usage of references. A namespace is of type
`#nms` and its value is a (reference to a) collection of key-value pairs.
Applying a name space to a module will substitute every occurrence of a key with
its value for each entry in the collection.

The following example will replace every occurance of the symbolic term `+` with the symbolic term `org.aiddl.eval.numerical.add`.

    (#nms A {+:org.aiddl.eval.numerical.add})

Applying this namespace to the following term

    (+ 1 2)
    
will replace it with

    (org.aiddl.eval.numerical.add 1 2)

Various standard namespaces are defined in `org.aiddl.eval-nms`. The following
example covers commonly used numerical operations (equality is covered in
`basic-ops` in the same module).


    (^$namespace numerical-ops {
      +               : org.aiddl.eval.numerical.add
      -               : org.aiddl.eval.numerical.sub
      *               : org.aiddl.eval.numerical.mult
      div             : org.aiddl.eval.numerical.div
      modulo          : org.aiddl.eval.numerical.modulo
      >               : org.aiddl.eval.numerical.greater-than
      >=              : org.aiddl.eval.numerical.greater-than-eq
      <               : org.aiddl.eval.numerical.less-than
      <=              : org.aiddl.eval.numerical.less-than-eq
      is-pos          : org.aiddl.eval.numerical.is-positive
      is-neg          : org.aiddl.eval.numerical.is-negative
      is-zero         : org.aiddl.eval.numerical.is-zero
      is-inf          : org.aiddl.eval.numerical.is-infinite
      is-inf-pos      : org.aiddl.eval.numerical.is-infinite-positive
      is-inf-neg      : org.aiddl.eval.numerical.is-infinite-negative
      is-nan          : org.aiddl.eval.numerical.is-nan
    })


