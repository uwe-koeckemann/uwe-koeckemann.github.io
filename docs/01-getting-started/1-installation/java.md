---
layout: default
title: Java
parent: Installing AIDDL
grand_parent: Getting Started
nav_order: 1
---

We use `<AIDDL>` as the root of the [aiddl.org](http://www.aiddl.org) git
repository. To install the AIDDL libraries, open a console in the root of the
library you would like to install:

1. Core library: `<AIDDL>/core/java/`
2. Util library: `<AIDDL>/util/java/`
3. Common library: `<AIDDL>/common/java/`

For each of these (in the listed order) run the following on the command line:

- Linux:

        ./gradlew publishToMavenLocal
    
- Windows:

        gradlew publishToMavenLocal
       
       
# Troubleshooting

- Gradle error: Class file compiled with a different version
  - Cause: Your Java version is probably too new for the Gradle version used by
    the wrapper.
  - Solution: Update the Gradle wrapper to the most recent version `<VER>` of
    gradle, or use an older version of Java (in case gradle does not support
    your java version yet). Please contact us, so we can update the wrapper in
    the repository as well. Meanwhile the following command should do the trick:
    
        ./gradlew wrapper --gradle-version=<VER>
