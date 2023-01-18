---
layout: default
title: AIDDL Files (aka Modules)
nav_order: 4
parent: Getting Started
has_children: true
---

In the previous section, we assembled AIDDL terms directly in the core
libraries. While this is often useful to assemble return values or intermediate
structures, we most often want to write models in their own files that can be
used by multiple programs. 

AIDDL files (ending in .aiddl) consist of a series of entries. Each entry has a
type, a name, and a value.  The type tells us which subset of the AIDDL is
allowed as a value.  The name can be used to retrieve the entry in a program or
to refer to it in other entries. The value is the actual content of the entry.

    (org.aiddl.type.collection.set S {1 2 3})
    
In fact, each AIDDL file is a module and its first entry has the form

    (#mod self uri)
    
where `#mod` is a special type only used in the first entry of a module, `self`
is the self-alias of the module (how it refers to itself) and `uri` is a
globally unique name for the module. No two modules should every use the same
name. This allows us to refer to the entries in other modules via reference
terms.
