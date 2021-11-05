---
layout: default
title: First Steps (Java Core)
parent: First Steps
grand_parent: Getting Started
nav_order: 1
---

The following program is a simple hello world. It imports two of the representation classes,
creates a `StringTerm` and prints it to the console.


    package org.aiddl.hello_world;

    import org.aiddl.core.representation.Term;
    import org.aiddl.core.representation.StringTerm;
    
    public class HelloAIDDL {
        public static void main( String[] args ) {
            StringTerm helloAiddl = Term.string("Hello AIDDL World!");
            System.out.println(helloAiddl);
        }
    }


To run this program, create a Java application gradle project and run it with:

- Linux:

        ./gradlew run
        
- Windows:

        gradlew run

