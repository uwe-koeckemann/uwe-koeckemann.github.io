# Requirements

Requirements are used to load other modules and allow us to make references to
these modules.  A requirement is an entry of type `#req`:

    (#req name module)

The name `name` of a requirement is the local alias for the module. The value
`module` points to the required module.The `name` alias can be used in
references instead of the full name. Given the requirement above, the entry
reference term `e@name` refers to an entry named `e` in the module `module`.

There are multiple options to specify the `module` part above.

1. The URI of a module whose file can be recursively found in the `AIDDL_PATH` environment variable.
2. A string representing a file name relative to the current module's file

Consider the following module:

    (#mod self example)
    (#req A "domain.aiddl")
    (#req B org.aiddl.common.planning.state-variable)

The file `domain.aiddl` needs to be placed in the same folder as the example
module. The second requirement refers to the module
`org.aiddl.common.planning.state-variable` by using its symbolic URI.  The file
of this module is part of the AIDDL Common library. In order to use this name,
the `AIDDL_PATH` must contain the path to the aiddl common definitions:

    "<AIDDL>/common/aiddl/"

where `<AIDDL>` is replaced by the path to the AIDDL repository.

