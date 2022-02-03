---
layout: default
title: Requirements
parent: The Language
nav_order: 5
---

Requirements are used to make local references to other modules.  A requirement
is an entry of type `#req`:

    (#req A "required.aiddl")

The name `A` of a requirement creates a local alias for the module found in the
required file. This alias can then be used in references instead of the full
name. Given the requirement above, the entry reference term `e@A` refers to an
entry named `e` in the module `required.aiddl`.

The above example uses a relative filename. Alternatively, any symbolic module
name of files recursivly found in any folder listed in the `AIDDL_PATH`
environment variable can be used regardless of their folder. If the module 
of the file `required.aiddl` is defined via

    (#mod self org.aiddl.required)

and the file is placed in `AIDDL_PATH`, we could replace the above `#req`
statement with

    (#req A org.aiddl.required)

