---
layout: default
title: Evaluating Terms
parent: Core Library
nav_order: 1
---

It is often convenient to automatically create parts of a model from parameters
or based on other parts and known functions. The evaluator is a function that
interprets AIDDL terms similar to the Lisp programming language for this
purpose.

The evaluator takes any AIDDL term as an argument and recursively evaluates it
according to the following cases:


- List: evaluate all arguments and return new list with result
- Set: evaluate all arguments and return new set with result
- Key-value pair: evaluate key and value and return new key-value pair
- Entry Reference: resolve reference (default) resolve reference and recursively
  evaluate result (must be enabled manually)
- Tuple (x1, x2, ..., xn):
  - x1 is not a function URI: evaluate all arguments and return new tuple with result
  - x1 is function f: evaluate `x2` to `y1` and so on and create tuple `(y1,
    ..., yn)` of evaluated arguments
    - n == 1: return f(())
    - n == 2: return f(y2)
    - else: return \texttt{f((y2, ..., yn))}
- _: return argument unchanged

Any time it comes across a tuple with a known function as first element the
function will be called on the remainder of the arguments evaluated. Under the
hood there are a few more cases considered. Functions flagged for *lazy
evaluation* are assumed to evaluate their own arguments. This is important to
allow functions like a Boolean *and* or *or* to avoid evaluating all arguments
in case a `false` or `true` result is found early.
