---
layout: default
title: Functions
parent: Core Library
nav_order: 3
---

The core library provides interfaces/traits for functions to target different
needs.

- Function: uses term as input and produces term as output
  - Example: use a planning problem to produce a plan
  - Example: use a machine learning problem to produce a decision tree
- Initializable: inizalize a function with a term
  - Example: initial state for a search
  - Example: set a state machine to an initial state
- Configurable: set parameters of a function
  - Example: set parameters of a search or of a learning algorithm

# Default Functions

Each core implementation has a set of default functions that should behave the
same across different core libraries.


