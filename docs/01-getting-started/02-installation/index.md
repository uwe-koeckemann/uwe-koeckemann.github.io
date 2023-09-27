---
layout: default
title: Installing AIDDL
nav_order: 2
parent: Getting Started
has_children: true
---

Before installing the AIDDL libraries, we need to set the `AIDDL_PATH`
environment variable.  This will allow to import required modules (via `#req`)
by using their name.

# Linux

Add the following lines to the end of the `~/.profile` file:

    AIDDL_PATH="<AIDDL-ROOT>/core/aiddl/:<AIDDL-ROOT>/common/aiddl/"

# Windows

1. Navigate to: Computer -> Settings -> Advanced Settings -> Environment Variables
2. Add a new variable named `AIDDL_PATH`
3. Change the value of AIDDL_PATH to include the following folders:

        <AIDDL-ROOT>/core/aiddl
        <AIDDL-ROOT>/common/aiddl
