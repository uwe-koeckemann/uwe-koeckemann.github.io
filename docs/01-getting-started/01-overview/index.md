---
layout: default
title: Overview
nav_order: 2
parent: Getting Started
has_children: true
---

# What is AIDDL?

- AIDDL is both a language and a framework for integrative AI.
- As a language, AIDDL allows to write models for different AI problems in a
  common language.
- As a framework, AIDDL allows to manipulate terms written in the language,
  check types, use a rich library of default imlementations of AI algorithms.
- Together language and framework allow to describe and execute integrated AI
  systems in the same language as their inputs.
  
# Overview



## Language

The language can be divided into three types of terms.

- Basic terms: symbols, variables, numbers, strings
- Composite terms: lists, sets, tuples, key-value pairs
- Reference terms: references to entries of functions

## Types

Types specify subsets of the language. 

Consider the following exampl: the language contains any set mixing symbols,
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

- Load AIDDL files (modules) into containers
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
