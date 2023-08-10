---
layout: default
title: Scala
parent: Installing AIDDL
nav_order: 3
grand_parent: Getting Started
---

# Using AIDDL in Your Project

To use AIDDL in a Scala project, add the following dependencies to your
`build.sbt`:

    libraryDependencies += "org.aiddl" % "aiddl-core-scala" % "1.0.0"
    libraryDependencies += "org.aiddl" % "aiddl-common-scala" % "0.1.0"
    
Also make sure Maven Central is used as a resolver:

    resolvers += Resolver.mavenCentral

# Compiling AIDDL Libraries (optional)

Install the [Scala Build Tool (SBT)](https://www.scala-sbt.org/download.html).

We use `<AIDDL>` as the root of the [aiddl.org](http://www.aiddl.org) git
repository. To install the AIDDL libraries, open a console in the root of the
library you would like to install:

1. Core library: `<AIDDL>/core/scala/`
2. Common library: `<AIDDL>/common/scala/`

For each of these (in the listed order) run sbt with:

        sbt compile test publishM2
                
This will compile the project, run all test cases, and finally publish the
project in your local Maven repository.
    
To depend on the version we just compiled, we need to add Maven local as a
resolver:

    resolvers += Resolver.mavenLocal,
    
In this case we also need to adjust the version number to the compiled version.
