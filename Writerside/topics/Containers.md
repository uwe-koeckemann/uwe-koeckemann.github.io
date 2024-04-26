# The Container

A *container* is the main data structure of the AIDDL core and serves two purposes.

1. Store and access modules and their entries
2. Register and lookup functions

When an AIDDL module file is parsed, its contents will be added to a container.
If any functions for types are defined in the module (via `#def` or `#type`),
they will be added to the container's function registry under a name composed of
the definitions entry name and the module name.

For example in module `org.aiddl.test` the type entry

    (#type my-type (set-of int))

will register a type check function under the name `org.aiddl.test.my-type`.

## Entries

Entries can be accessed based on their name or by matching against type, name,
and value patterns.

## Modules

Modules can be created by providing a symbolic name.

Aliases used by modules for other modules can be added (usually just used by the
parser to manage requirement names).

## Functions

Functions can be accessed based on their symbolic name or added by providing a
symbolic name and a function object.
