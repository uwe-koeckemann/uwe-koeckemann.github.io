# Overview

- AIDDL is both a language and a framework for integrative AI. That means its
  main purpose is to facilitate the combination of different AI techniques into
  a larger (integrated) system.
- As a language, AIDDL allows to write models for different AI problems in a
  common language.
- As a framework, AIDDL allows to manipulate terms written in the language,
  check types, use a rich library of default implementations of AI algorithms.
- Together language and framework allow to describe and execute integrated AI
  systems in the same language as their inputs.
  
## Typical Workflow

1. Write one or more AIDDL files containing the required models.
2. Write a program that:
  - loads your AIDDL files and accesses required terms
  - loads existing AI functions from the AIDDL framework
  - add missing functions (e.g., application specific, glue, wrappers around
    existing AI libraries)
  - combine functions into an integrated AI system

## Language

The language can be divided into three types of terms.

- Basic terms: symbols, variables, numbers, strings
- Composite terms: lists, sets, tuples, key-value pairs
- Reference terms: references to entries of functions

## Types

Types specify subsets of the language.

They are used to:

1. Describe models used by AI algorithms (e.g., a planning domain, machine
   learning data or models, a logical knowledge base).
2. Describe applications specific input/output models. This allows to write
   application specific models without choosing an AI approach.

Consider the following example: the language contains any set mixing symbols,
numbers, variables, etc.

    { 1 2 3 a b c }

We can define a type to express all sets that contain positive integers.

    { 1 2 3 }

The core library (briefly covered in the next section) contains functionality to
load types and perform type checks on terms. In the previous example,
the type ~sets containing only positive integers~ would test negative for the first 
set and positive for the second.

## Core Library

The core library makes AIDDL accessibly in a specific programming language.

- Load AIDDL files (also called modules) into containers
- Default operations on terms (e.g., numerical, set or list operations)
- Evaluate terms 
- Type check terms (given a type definition)
- Run unit tests written in AIDDL format

## Common Library

The common library is a collection of types and algorithm implementations of
common AI algorithms. Its main purpose is to provide default components for
rapidly crafting integrated AI systems.

## External Libraries

External libraries provide wrappers around other libraries to allow them to use
AIDDL models as input and output. These libraries are usually dedicated to
specific AI problems and their implementations are expected to be more mature,
stable, and closer to the state-of-the-art than the ones found in the common
library.

External libraries are kept separate from the common library to avoid dependency
overload.

## Examples

A collection of projects that show how AIDDL can be used to create integrated AI
systems. 
