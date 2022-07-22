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

The following is a complete list of all implemented evaluator functions that
should be supported by every implementation of the AIDDL Core Library.  Most
lower case symbols (`a,b,e,f,m,t,x`) refer to terms that may also be evaluated.
The symbol `σ` is a substitution. A substitution is a non-cyclic mapping
from terms to terms. Applying a substitution `σ = \{k_1:v_1, ... \}` to
a term `x` is written as `xσ` and results in a new term that recursively
replaces all appearances of `k_i` in `x` by `v_i`.  Another important concept is
matching. A term `a` can be matched to a term `b` iff there exists a
substitution `σ` such that `aσ = b`.  Upper case letters represent
collections, which can be either collection terms `C`, sets `S`, or lists
`L`. In some cases we use set notation on collections (which may be lists) for
simplicity.


|------------------|-----------------------------------|----------------------------------------------------------------------------|
| Name             | Arguments                         | Description                                                                |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| org.aiddl.eval   |                                   |                                                                            |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| call             | `f, x`                            | Call function referred to by `f` with argument `x`                         |
| lambda           | `a`                               | Create reference to anonymous function defined by `a`                      |
| quote            | `x`                               | `x`                                                                        |
| type             | `x`, `t`                          | Check if `x` has type `t`                                                  |
| signature        | `X=(x_1 \ldots) , S=[s_1 \ldots]` | `\mathit{true}` iff all `x_i` have type `s_{\min{i,\vert S\vert }}`        |
| equals           | `a`, `b`                          | `a = b`                                                                    |
| not-equals       | `a`, `b`                          | `a \neq b`                                                                 |
| matches          | `a`, `b`                          | `\exists_σ : aσ = b`                                             |
| first            | `L`                               | first element of `L` (`L` may be a tuple)                                  |
| last             | `L`                               | last element of `L` (`L` may be a tuple)                                   |
| size             | `C`                               | `\vert C\vert `                                                            |
| get-key          | `C,k`                             | `v` s.t. `k:v \in C` (`C` may be a tuple)                                  |
| get-idx          | `L,n`                             | `n`th element of `L` (`L` may be a tuple)                                  |
| let              | `σ,a`                        | `a σ`                                                                 |
| if               | `c,a,b`                           | if `c` then `a` else `b`                                                   |
| cond             | `L`                               | `e_i` s.t. `\min_i : (c_i:e_i) \in L \wedge c_i`                           |
| map              | `f,m,C`                           | `\{ fσ \vert  e \in C, mσ=e \}`                                  |
| filter           | `f,m,C`                           | `\{ e \vert  e \in C, mσ=e,fσ = true \}`                         |
| reduce           | `f,m,C`                           |                                                                            |
| match            | `f,m,C`                           |                                                                            |
| zip              | `f,m,C`                           |                                                                            |
| key              | `k:v`                             | `k`                                                                        |
| value            | `k:v`                             | `v`                                                                        |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| .logic           |                                   |                                                                            |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| not              | `x`                               | true if `x` is false                                                       |
| and              | `x_1 \ldots x_n`                  | true if all `x_i` are true                                                 |
| or               | `x_1 \ldots x_n`                  | true if any `x_i` is true                                                  |
| forall           | `m, C, x`                         | `\forall_{e \in C} \exists_σ : e_i = m σ \wedge x σ = true` |
| exists           | `m, C, x`                         | `\exists_{e \in C} \exists_σ : e_i = m σ \wedge x σ = true` |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| .collection      |                                   |                                                                            |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| in               | `e, C`                            | `e \in C`                                                                  |
| contains         | `C, e`                            | `e \in C`                                                                  |
| contains-all     | `C_1, C_2`                        | `C_2 \subseteq C_1`                                                        |
| contains-any     | `C_1, C_2`                        | `C_1 \cap C_2 \neq \varnothing`                                            |
| contains-match   | `C, m`                            | `\exists_{e \in C, σ} : mσ = e`                                  |
| add-element      | `e, C`                            | `C' ~ \text{s.t.} ~ e \in C'`                                              |
| add-all          | `C_1, C_2`                        | `C' = C_1 \cup C_2`                                                        |
| remove           | `e, C`                            | `C-\{e\}`                                                                  |
| remove-all       | `C_1, C_2`                        | `C_2-C_1`                                                                  |
| sum              | `C`                               | `\sum_{e \in C} e`                                                         |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| .collection.list |                                   |                                                                            |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| concat           | `L_1, L_2`                        | `L' = L_1 \dot L_2`                                                        |
| % head           | `L`                               | first element of `L`                                                       |
| % tail           | `L`                               | last element of `L`                                                        |
| % cut-head       | `L`                               | `L` without first element                                                  |
| % cut-tail       | `L`                               | `L` without last element                                                   |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| .collection.set  |                                   |                                                                            |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| union            | `\{S_1, \ldots, S_n \}`           | `S' = \bigcup_i S_i`                                                       |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| .numerical       |                                   |                                                                            |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| add              | `a,b`                             | `a+b`                                                                      |
| sub              | `a,b`                             | `a-b`                                                                      |
| mult             | `a,b`                             | `ab`                                                                       |
| div              | `a,b`                             | `a/b`                                                                      |
| modulo           | `a,b`                             | `a \mod b`                                                                 |
| greater-than     | `a,b`                             | `a > b`                                                                    |
| greater-than-eq  | `a,b`                             | `a \geq b`                                                                 |
| less-than        | `a,b`                             | `a < b`                                                                    |
| less-than-eq     | `a,b`                             | `a \leq b`                                                                 |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| .term            |                                   |                                                                            |
|------------------|-----------------------------------|----------------------------------------------------------------------------|
| term             | `a`                               | true                                                                       |
| numerical        | `a`                               | `a` is numerical                                                           |
| integer          | `a`                               | `a` is integer                                                             |
| rational         | `a`                               | `a` is rational                                                            |
| real             | `a`                               | `a` is real                                                                |
| infinity         | `a`                               | `a` is infinity                                                            |
| symbolic         | `a`                               | `a` is symbolic                                                            |
| boolean          | `a`                               | `a` is boolean                                                             |
| string           | `a`                               | `a` is string                                                              |
| variable         | `a`                               | `a` is variable                                                            |
| reference        | `a`                               | `a` is reference                                                           |
| fun-ref          | `a`                               | `a` is function reference                                                  |
| collection       | `a`                               | `a` is collection                                                          |
| list             | `a`                               | `a` is list                                                                |
| set              | `a`                               | `a` is set                                                                 |
| tuple            | `a`                               | `a` is tuple                                                               |
| key-value        | `a`                               | `a` is key-value pair                                                      |
