---
layout: default
title: Modules
parent: The Language
nav_order: 2
---

Every AIDDL file represents a module and begins with a module declaration that
tells us the name of its module. This provides an alternative way to refer to
the file and allows others modules to refer to this module. This also means that
the names of entries only need to be unique within a module/file. For example,
the module entry

    (#mod self org.aiddl.module)

has type `#mod`, name `self`, and value `org.aiddl.module`.  The name refers to
the way in which the module refers to itself (usually we use the symbolic term
`self`). The value is the way in which other modules refer to this module
(i.e. the global name of the module). Module names should be unique across all
modules.
