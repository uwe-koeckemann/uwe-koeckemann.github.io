---
layout: default
title: Terms
parent: The Language
nav_order: 3
---

Everything written in AIDDL proper (i.e. the language) is a *Term*.

The following table provides an overview of all basic types of terms with a
short description and example.

| Name               | Reference                           | Description                                                            | Examples                                        |
|--------------------|-------------------------------------|------------------------------------------------------------------------|-------------------------------------------------|
| Term               | `org.aiddl.term`                    | Every AIDDL expression is a term.                                      | see below                                       |
| Symbolic           | `org.aiddl.term.symbolic`           | Non-numerical constants.                                               | `a` `e1` `+` `#integer`                         |
| Boolean            | `org.aiddl.term.symbolic.boolean`   | Boolean constants                                                      | `true`  `false`                                 |
| Variable           | `org.aiddl.term.variable`           | Named variables beginning with `?` or anonymous variables `_`          | `?x` `?e1` `_`                                  |
| String             | `org.aiddl.term.string`             | Any string of characters in quotes.                                    | `"a"` `"abc"` `"1 2 3"`                         |
| Numerical          | `org.aiddl.term.numerical`          | Different types of numerical values.                                   | see below                                       |
| Integer            | `org.aiddl.term.numerical.integer`  | Positive or negative integers.                                         | `0` `-3` `11`                                   |
| Rational           | `org.aiddl.term.numerical.rational` | Positive or negative rational numbers.                                 | `0/1` `-1/3` `110/13`                           |
| Real               | `org.aiddl.term.numerical.real`     | Positive or negative real numbers.                                     | `0.0` `-1.3` `1.1`                              |
| Infinity           | `org.aiddl.term.numerical.inf`      | Positive or negative infinity.                                         | `INF` `+INF` `-INF`                             |
| NaN                | `org.aiddl.term.numerical.nan`      | Not a Number                                                           | `NaN`                                           |
| Collection         | `org.aiddl.term.collection`         | Collections of terms.                                                  | see below                                       |
| Set                | `org.aiddl.term.collection.set`     | A set of terms. Cannot be matched to other terms.                      | `{}` `{e1 e2 e3}` `{1 1 2}`                     |
| List               | `org.aiddl.term.collection.list`    | A list of terms.                                                       | `[]` `[e1 e2 e3]` `[1 1 2]`                     |
| Tuple              | `org.aiddl.term.tuple`              | A tuple of terms. Unlike lists, we assume tuples will not be extended. | `()` `(e1 e2 e3)` `(1 1 2)`                     |
| Reference          | `org.aiddl.term.reference`          | A reference to an entry in a specific module.                          | `e@m` references entry named `e` in module `m`  |
| Function Reference | `org.aiddl.term.fun-ref`            | Reference to a function. Allows using functions as data.               | `^org.aiddl.eval.add`                           |
|                    |                                     |                                                                        | `^f@m` references function `f` in module `m`    |
| Key Value Pair     | `org.aiddl.term.key-value`          | A key and a value term. The key must not be a Key-value pair itself.   | `x:10` symbolic key `x` with integer value `10` |

# Grammar

Every AIDDL file represents a module and consists of a module specification and
an arbitrary number of entries. Each entry consists of a type, a name and a
value (all terms).

    <AiddlFile>  :: <Module> (<Entry>)*
    <Module>     :: "(#mod" <Symbolic>
                            <Symbolic> ")"
    <Entry>      :: "("<Term> <Term> <Term>")"
    <Term>       ::  <Basic> | <Composite>
                  |  <Reference>
    <Basic>      :: <Symbol> | <Numerical>
                  | <Variable> | <String>
    <Numerical>  :: <Integer> | <Rational>
                  | <Real> | <Infinity> | <NaN>
    <Symbolic>   :: (("a"-"z"|"A"-"Z"|"#")
                     ("a"-"z"|"A"-"Z"|"0"-"9"
                     |"_"|"."|"-"|"'")*)
                     |"+"|"-"|"/"|"*"|"&"|"|"
                     |"!"|"="|"<"|">"|"=>"
                     |"<=>"|"^"|"!="|"<="|">="
    <String>        :: "\"" [~\"]* "\""
    <Variable>      :: <NamedVar> | "_"
    <NamedVar> :: ?(("a"-"z"|"A"-"Z")
                    ("a"-"z"|"A"-"Z"
                    |"0"-"9"|"_"|"."|"-"|"'")*)
    <Integer> :: ["-"]("0"|"1"-"9")("0"-"9"]*
    <Rational> :: ["-"]("0"|"1"-"9")("0"-"9")*
                   "/" ("1"-"9"("0"-"9")*)
    <Real> :: ["-"] ("0"|"1"-"9")("0"-"9")*
               "." ("0"-"9")+
    <Infinity> :: ["+"|"-"]"INF"
    <NaN> :: "NaN"
    <Composite> :: <List> | <Set>
                 | <Tuple> | <KeyValue>
    <List>       ::  "[" <Term>* "]"
    <Set>        ::  "{" <Term>* "}"
    <Tuple>      ::  "(" <Term>* ")"
    <KeyValue>   :: [<Symbolic>|<String>|<Variable>
                    |<Numerical>|<Collection>|<Tuple>
                    |<Reference>]":"<Term>
    <Reference> :: <EntRef> | <FunRef>
    <EntRef>  :: [<Symbolic>|<Tuple>]"@"<Symbolic>
               | "$"<Term>
    <FunRef> :: "^"[<Symbolic>|"$"<Symbolic>
                   |<Symbolic>"@"<Symbolic>]

# Reference Terms

There are two type of reference terms in AIDDL: entry references and function
references.

## Entry Reference Terms

*Entry reference terms* point to entries in the same or in other modules.  This
is useful when the referenced entry may change or when trying to break down
large models into smaller pieces that can then be assembled.  In general,
references can be written as

    name@alias
    
where `name` is again an entry name and `alias` is the local name of the module
in which the entry can be found.  References to the same module can be written
more compactly as

    $name

where `name` is the name of an entry in the current module. 
This is equivalent to writing 

    name@self

if `self` is the self alias of the current module established by the `#mod`
entry of a module:

    (#mod self uri)

Other aliases are established through `#req` entries that load other modules as
requirements. For example, after loading the module `req-uri` with

    (#req mod req-uri)
    
we can refer to an entry named `name` with
    
    name@mod

## Resolving References

In any AIDDL core there exists a function that allows to resolve any references
in a term recursively given a container. 

Note that the result of this may change if the referenced entries change. So any
term that includes a reference term may represent a different term depending on
when it is resolved.

Further, entry references may refer to their own entry. In such cases some care
is required when resolving references in terms. For instance

    (^org.aiddl.type.collection.set S {$S})
    
Defines `S` as a set that contains `S`. Attempting to fully resolve this term is
not possible because the result is an infinetly nested set.

# Function Reference Terms
 
*Function reference terms* signal that a symbol references a function.  This
allows to pass functions as arguments (i.e. higher-order functions) and to use
these functions as building blocks (e.g., a heuristic function can be provided
to a search algorithm).  Function references are preceded by the `^` symbol and
consist of a symbolic term, or a reference term.

In the latter case, the alias will be resolved to the URI of the
referenced module. The following two entries, for instance, are equivalent if
`M` is an alias for `org.aiddl.my-module`:


    ^org.aiddl.my-module.f
    ^f@M

Function references assume that their functions are registered (in the container
or function registry) when they are first used. The actual functions may be
implemented in any programming langage with an AIDDL core library or via `#def`
or `#type` entries.
