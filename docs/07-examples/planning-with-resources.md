---
layout: default
title: Planning with Resources
parent: Examples
nav_order: 4
---

This example provides a simple integration of resource management with a task
planner.  We use a forward search planner and integrate resource propagation
into state transition functions and resource constraints into pruning functions
to prune state that over- oder under-use a resource.

This is a nice example of a hybrid AI system on a finer granularity: instead of
combining a blackbox planner with a blackbox resource reasoner, we consider the
planner as a whitebox and add propagation and pruning into its regular search
operations.

Go
[here](https://github.com/uwe-koeckemann/AIDDL/tree/master/example/planning-with-resources)
for instructions to run this example.

.
