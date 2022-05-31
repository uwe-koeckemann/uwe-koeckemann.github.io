---
layout: default
title: Namespaces
parent: The Language
nav_order: 4
---

Namespaces allow to substitute symbolic names when a module is loaded.  This
allows to introduce shortcuts for overly long names (such as the default
function names in AIDDL).

A namespace is introduced with an entry of type `#nms`.  The value of this entry
is a (reference to a) collection of key-value pairs.  Applying a namespace to a
module will substitute every occurrence of a key with its value *when the module
is loaded*.

The following example creates a simple namespace that will replace every
occurance of the symbolic term `+` with the symbolic term
`org.aiddl.eval.numerical.add`.

    (#nms A {+:org.aiddl.eval.numerical.add})
    
Applying this namespace to the following term

    (+ 1 2)
    
will replace it with

    (org.aiddl.eval.numerical.add 1 2)
    
To extend this namespace, simply add more key-value pairs:

    (#nms A {
              +:org.aiddl.eval.numerical.add
              -:org.aiddl.eval.numerical.sub
            })
    

Various standard namespaces are defined in `org.aiddl.eval-nms` (see [here](https://github.com/uwe-koeckemann/AIDDL/blob/master/core/aiddl/eval-nms.aiddl)) and listed
below. These can be loaded by loading the module and the namespace.

    (#req nms org.aiddl.eval-nms)
    (#nms nms-basic basic-ops@nms)

*Note: Using a namespace may have unwanted side-effects since it
overwrites any of the symbolic keys in its collection. Therefore it is
recommended to use small namespaces where possible.*
    
# Basic Operations

Coveres miscelaneous basic functions.
    
    (^$namespace basic-ops {
      eval            : org.aiddl.eval
      eval-ref        : org.aiddl.eval.eval-ref
      eval-all-refs   : org.aiddl.eval.eval-all-refs
      call-request    : org.aiddl.util.call-request
      init-function   : org.aiddl.eval.init-function
      config-function : org.aiddl.eval.config-function
      call            : org.aiddl.eval.call
      load-fun        : org.aiddl.eval.load-function
      load-factory    : org.aiddl.eval.load-function-factory
      core-lang       : org.aiddl.eval.core-lang
      lambda          : org.aiddl.eval.lambda
      quote           : org.aiddl.eval.quote
      has-type        : org.aiddl.eval.type
      signature       : org.aiddl.eval.signature
      domain          : org.aiddl.eval.domain
      matches         : org.aiddl.eval.matches
      =               : org.aiddl.eval.equals
      equals          : org.aiddl.eval.equals
      !=              : org.aiddl.eval.not-equals
      not-equals      : org.aiddl.eval.not-equals
      substitute      : org.aiddl.eval.substitute
      let             : org.aiddl.eval.let
      if              : org.aiddl.eval.if
      cond            : org.aiddl.eval.cond
      map             : org.aiddl.eval.map
      filter          : org.aiddl.eval.filter
      reduce          : org.aiddl.eval.reduce
      match           : org.aiddl.eval.match
      zip             : org.aiddl.eval.zip
      substitute      : org.aiddl.eval.substitute
    })
    
# Sym Ops

Covers operations on symbols.
    
    (^$namespace sym-ops {
    ;; Symbolic
      sym-concat      : org.aiddl.eval.symbolic.concat
      sym-split       : org.aiddl.eval.symbolic.split
    })
    
# String Ops

Covers operations on strings.
    
    (^$namespace string-ops {
    ;; String
      str-concat      : org.aiddl.eval.string.concat
      mod-folder      : org.aiddl.eval.string.module-folder 
    })
    
# Collection Ops

Covers operations on collections and tuples.

    (^$namespace collection-ops {
    ;; Collection & Tuple
      size            : org.aiddl.eval.size
      get-key         : org.aiddl.eval.get-key
      contains-key    : org.aiddl.eval.contains-key
      is-unique-map   : org.aiddl.eval.is-unique-map
    ;; List & Tuple
      first           : org.aiddl.eval.first
      last            : org.aiddl.eval.last
      get-idx         : org.aiddl.eval.get-idx
      in              : org.aiddl.eval.collection.in
      contains        : org.aiddl.eval.collection.contains
      contains-all    : org.aiddl.eval.collection.contains-all
      contains-any    : org.aiddl.eval.collection.contains-any
      add-element     : org.aiddl.eval.collection.add-element
      add-all         : org.aiddl.eval.collection.add-all
      remove          : org.aiddl.eval.collection.remove
      remove-all      : org.aiddl.eval.collection.remove-all
      contains-match  : org.aiddl.eval.collection.contains-match
      put-all         : org.aiddl.eval.collection.put-all
      sum             : org.aiddl.eval.collection.sum
      col-union       : org.aiddl.eval.set.union
      concat          : org.aiddl.eval.list.concat
      head            : org.aiddl.eval.list.head
      tail            : org.aiddl.eval.list.tail
      cut-head        : org.aiddl.eval.list.cut-head
      cut-tail        : org.aiddl.eval.list.cut-tail
    })
    
# Key Value Ops

Covers operations on key-value pairs.

    (^$namespace key-value-ops {
      key             : org.aiddl.eval.key
      value           : org.aiddl.eval.value
    })
    
# Logic Ops

Covers logical operators and quantification.
    
    (^$namespace logic-ops {
      not             : org.aiddl.eval.logic.not
      and             : org.aiddl.eval.logic.and
      or              : org.aiddl.eval.logic.or
      exists          : org.aiddl.eval.logic.exists
      forall          : org.aiddl.eval.logic.forall
    })
    
# Numerical Ops

Covers operations involving numerical terms.
    
    (^$namespace numerical-ops {
      =               : org.aiddl.eval.equals
      !=              : org.aiddl.eval.not-equals
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
    
# Type Ops

Covers type names and definitions.
    
    (^$namespace type-ops {
      term            : org.aiddl.type.term
      numerical       : org.aiddl.type.term.numerical
      integer         : org.aiddl.type.term.numerical.integer
      rational        : org.aiddl.type.term.numerical.rational
      real            : org.aiddl.type.term.numerical.real
      infinity        : org.aiddl.type.term.numerical.infinity
      symbolic        : org.aiddl.type.term.symbolic
      boolean         : org.aiddl.type.term.symbolic.boolean
      string          : org.aiddl.type.term.string
      variable        : org.aiddl.type.term.variable
      ent-ref         : org.aiddl.type.term.ref.entry
      fun-ref         : org.aiddl.type.term.ref.function
      collection      : org.aiddl.type.term.collection
      list            : org.aiddl.type.term.collection.list
      set             : org.aiddl.type.term.collection.set
      tuple           : org.aiddl.type.term.tuple
      key-value       : org.aiddl.type.term.key-value
    
      enum            : org.aiddl.type.enum
      range           : org.aiddl.type.range
      col-of          : org.aiddl.type.collection-of
      set-of          : org.aiddl.type.set-of
      list-of         : org.aiddl.type.list-of
                                             
      union           : org.aiddl.type.union
      inter           : org.aiddl.type.intersection
      sig             : org.aiddl.type.tuple.signed
      typed-kvp       : org.aiddl.type.typed-key-value
      dict            : org.aiddl.type.dictionary
      matrix          : org.aiddl.type.matrix
    })

# Basic

The basic namespace offers all functions but uses longer names in some cases to
avoid conflicts. 

    (^$namespace basic {
      eval            : org.aiddl.eval
      eval-ref        : org.aiddl.eval.eval-ref
      eval-all-refs   : org.aiddl.eval.eval-all-refs
      call-request    : org.aiddl.util.call-request
      init-function   : org.aiddl.eval.init-function
      config-function : org.aiddl.eval.config-function
      call            : org.aiddl.eval.call
      load-fun        : org.aiddl.eval.load-function
      load-factory    : org.aiddl.eval.load-function-factory
      core-lang       : org.aiddl.eval.core-lang
      lambda          : org.aiddl.eval.lambda
      quote           : org.aiddl.eval.quote
      has-type        : org.aiddl.eval.type
      domain          : org.aiddl.eval.domain
    ;; Term
      matches         : org.aiddl.eval.matches
      =               : org.aiddl.eval.equals
      equals          : org.aiddl.eval.equals
      !=              : org.aiddl.eval.not-equals
      not-equals      : org.aiddl.eval.not-equals
      substitute      : org.aiddl.eval.substitute
    ;; Symbolic
      sym-concat      : org.aiddl.eval.symbolic.concat
      sym-split       : org.aiddl.eval.symbolic.split
    ;; String
      str-concat      : org.aiddl.eval.string.concat
      mod-folder      : org.aiddl.eval.string.module-folder 
    ;; Collection & Tuple
      size            : org.aiddl.eval.size
      get-key         : org.aiddl.eval.get-key
      contains-key    : org.aiddl.eval.contains-key
      is-unique-map   : org.aiddl.eval.is-unique-map
    ;; List & Tuple
      first           : org.aiddl.eval.first
      last            : org.aiddl.eval.last
      get-idx         : org.aiddl.eval.get-idx
    ;; Key-Value Term
      key             : org.aiddl.eval.key
      value           : org.aiddl.eval.value
      let             : org.aiddl.eval.let
      if              : org.aiddl.eval.if
      cond            : org.aiddl.eval.cond
      map             : org.aiddl.eval.map
      filter          : org.aiddl.eval.filter
      reduce          : org.aiddl.eval.reduce
      match           : org.aiddl.eval.match
      zip             : org.aiddl.eval.zip
    ;; Logical
      not             : org.aiddl.eval.logic.not
      and             : org.aiddl.eval.logic.and
      or              : org.aiddl.eval.logic.or
      exists          : org.aiddl.eval.logic.exists
      forall          : org.aiddl.eval.logic.forall
    ;; Collection
      in              : org.aiddl.eval.collection.in
      contains        : org.aiddl.eval.collection.contains
      contains-all    : org.aiddl.eval.collection.contains-all
      contains-any    : org.aiddl.eval.collection.contains-any
      add-element     : org.aiddl.eval.collection.add-element
      add-all         : org.aiddl.eval.collection.add-all
      remove          : org.aiddl.eval.collection.remove
      remove-all      : org.aiddl.eval.collection.remove-all
      contains-match  : org.aiddl.eval.collection.contains-match
      put-all         : org.aiddl.eval.collection.put-all
      sum             : org.aiddl.eval.collection.sum
      union           : org.aiddl.eval.set.union
      concat          : org.aiddl.eval.list.concat
      head            : org.aiddl.eval.list.head
      tail            : org.aiddl.eval.list.tail
      cut-head        : org.aiddl.eval.list.cut-head
      cut-tail        : org.aiddl.eval.list.cut-tail
    ;; Numerical
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
    ;; Types
      term            : org.aiddl.type.term
      numerical       : org.aiddl.type.term.numerical
      integer         : org.aiddl.type.term.numerical.integer
      rational        : org.aiddl.type.term.numerical.rational
      real            : org.aiddl.type.term.numerical.real
      infinity        : org.aiddl.type.term.numerical.infinity
      symbolic        : org.aiddl.type.term.symbolic
      boolean         : org.aiddl.type.term.symbolic.boolean
      string          : org.aiddl.type.term.string
      variable        : org.aiddl.type.term.variable
      reference       : org.aiddl.type.term.ref.entry
      fun-ref         : org.aiddl.type.term.ref.function
      collection      : org.aiddl.type.term.collection
      list            : org.aiddl.type.term.collection.list
      set             : org.aiddl.type.term.collection.set
      tuple           : org.aiddl.type.term.tuple
      key-value       : org.aiddl.type.term.key-value
      type.enum       : org.aiddl.type.enum
      type.range      : org.aiddl.type.range
      type.col        : org.aiddl.type.collection-of
      type.set        : org.aiddl.type.set-of
      type.list       : org.aiddl.type.list-of
      type.union      : org.aiddl.type.union
      type.inter      : org.aiddl.type.intersection
      type.sig        : org.aiddl.type.tuple.signed
      type.kvp        : org.aiddl.type.typed-key-value
      type.dict       : org.aiddl.type.dictionary
      type.map        : org.aiddl.type.dictionary
      type.kv-tuple   : org.aiddl.type.dictionary
      type.matrix     : org.aiddl.type.matrix
    })
