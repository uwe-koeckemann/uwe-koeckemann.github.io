# Modules

Every AIDDL file represents a module and begins with a module declaration of the
following form:

    (#mod self module.uri).

In this declaration `#mod` shows that this is a module term, `self` refers to
the internal name of the module (its *self-alias*), and `module.uri` is the way
in which other modules refer to this module (its global name). Module names
should be unique across all modules.