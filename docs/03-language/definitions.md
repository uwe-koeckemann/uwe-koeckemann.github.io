---
layout: default
title: Defining Functions
parent: The Language
nav_order: 7
---

A definition is an entry of type `#def` The name of a definition entry is the
name of the type it defines and its value is a function that can be evaluated to
determine if a term is an instance of the type. Functions may refer to their
argument by using the special terms `#self` or `#arg`.

The following definition returns `true` for all integers between 0 and 10 (for
readability we assume namespaces `basic-ops`, `logic-ops`, and `numerical-ops`
from `eval-nms.aiddl`.

    (#def SmallInteger 
      (and
        (has-type #self ^integer)
        (>= #self 0)
        (<= #self 10)))
    
Within the same module, this function can be evaluated by refering to its name
at the beginning of a tuple. For instance

    ($SmallInteger 5)
    
would evaluate to `true`. 

Note that in most cases, functions are better defined in a programming language
with an AIDDL core. However, it can be convenient to use `#def` do define
constraints for types or assemble parts of a model from parameters.
