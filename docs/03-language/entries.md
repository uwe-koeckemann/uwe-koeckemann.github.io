---
layout: default
title: Entries
parent: The Language
nav_order: 1
---

Every AIDDL file is a list of entries.  Each entry contains a *type*, a *name*
and a *value* written as a tuple of three elements (hence each entry is itself
also a term).  In the previous secion we saw the special entry that delcares the
module of the file. The module entry is followed by any number of regular
entries that may define types or functions (see following sections), or contain
data/models. The following entry

    (^org.aiddl.type.collection.set S {a b c})
    
 declares an entry named `S` as the set `{a b c}` with type
 `^org.aiddl.type.collection.set`. Recall that the `^` marks the following
 symbol as a function reference term, which allows us to pass functions as
 arguments without evaluating them by accident.
 
 The type used above is one of the default types in AIDDL.  There are a variety
 of special types (like `#mod` and all starting with `#`) that we will go
 through next. We can also use references to any custom type we define (as
 explained in the section about types).
