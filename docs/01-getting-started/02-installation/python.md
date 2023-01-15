---
layout: default
title: Python
parent: Installing AIDDL
nav_order: 2
grand_parent: Getting Started
---

We use `<AIDDL>` as the base of the [aiddl.org](http://www.aiddl.org) git
repository. To install the AIDDL libraries, open a console in the root of the
library you would like to install:

1. Core library: `<AIDDL>/core/java/`
2. Util library: `<AIDDL>/util/java/`

And type 

    pip install -e .
    

# Using Conda

We recommended creating a virtual environment for installing AIDDL libraries.
This avoids conflicts and allows to test things without messing with your
existing environments. It also allows picking the exact Python version we wish
to use. With, e.g., [miniconda](https://docs.conda.io/en/latest/miniconda.html),
you can create a Python 3.7 environment named `aiddl` with

    conda create -n aiddl python=3.7

then switch to it with

    conda activate aiddl

before installing the libraries above. The Python version should be at least
3.x, but 3.7 is known to work.
