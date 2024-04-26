# Installation

## Setting up the AIDDL_PATH Variable

Before installing the AIDDL libraries, we need to set the `AIDDL_PATH`
environment variable.  This will allow to import required modules (via `#req`)
by using their name. Below, we use `<AIDDL>` as the root of the [aiddl.org](http://www.aiddl.org) git
repository.

<tabs>
<tab title="Linux">
Add the following lines to the end of the `~/.profile` file:


    AIDDL_PATH="<AIDDL>/core/aiddl/:<AIDDL>/common/aiddl/"

</tab>
<tab title="Windows">

1. Navigate to: Computer -> Settings -> Advanced Settings -> Environment Variables
2. Add a new variable named `AIDDL_PATH`
3. Change the value of AIDDL_PATH to include the following folders:


        <AIDDL>/core/aiddl
        <AIDDL>/common/aiddl

</tab>
</tabs>
