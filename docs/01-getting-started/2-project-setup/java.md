---
layout: default
title: Java Projects with Gradle
parent: Setting up AIDDL Projects
grand_parent: Getting Started
nav_order: 2
---

If you followed the installation instructions on the previous page, AIDDL
libraries have been installed in a local maven repository (i.e. a folder on your
computer). To make sure Gradle finds the library, add the following under
`repositories {...}` in your `build.gradle` file:

    mavenLocal()
    
To use AIDDL libraries, add the following under `dependencies {...}`.

    implementation("org.aiddl.core:aiddl-core") {
        version {
            strictly("[2.0.0, 3.0.0[")
        }
    }
    implementation("org.aiddl.util:aiddl-util") {
        version {
            strictly("[1.0.0, 2.0.0[")
        }
    }
    implementation("org.aiddl.common:aiddl-common") {
        version {
            strictly("[2.0.0, 3.0.0[")
        }
    }

The following is the `build.gradle` file of one of the runnable AIDDL example
applications. It also includes a `run` task to execute your application.


    apply plugin: 'java'
    apply plugin: 'application'
    
    sourceCompatibility = 1.8
    targetCompatibility = 1.8
    
    group = 'org.aiddl.example.learning-and-planning'
    version = '2.0'
    
    mainClassName = "org.aiddl.example.learning_agent.Run"
    
    repositories {
        mavenCentral()
        mavenLocal()
    }
    
    dependencies {
        testImplementation 'junit:junit:4.12'
    
        implementation("org.aiddl.core:aiddl-core") {
            version {
                strictly("[2.0.0, 3.0.0[")
            }
        }
        implementation("org.aiddl.util:aiddl-util") {
            version {
                strictly("[1.0.0, 2.0.0[")
            }
        }
        implementation("org.aiddl.common:aiddl-common") {
            version {
                strictly("[2.0.0, 3.0.0[")
            }
        }    
    }
    
    run {
        main = "org.aiddl.example.learning_agent.Run"
        classpath = sourceSets.main.runtimeClasspath 
    
        systemProperties System.getProperties()
    
        if(System.getProperty("exec.args") != null) {
          args System.getProperty("exec.args").split()
        }
    }
    
Finally, here is a full example for the `build.gradle` file of the AIDDL common
Java library. It has no run task, but includes everything required to create a
library that can be installed in the same way as AIDDL common. 

    apply plugin: 'java-library'
    apply plugin: 'maven-publish'
    
    sourceCompatibility = 1.8
    targetCompatibility = 1.8
    
    group = 'org.aiddl.common'
    version = '2.0.0'
    
    repositories {
        mavenCentral()
        mavenLocal()
    }
    
    dependencies {
        testImplementation 'junit:junit:4.12'
        implementation("org.aiddl.core:aiddl-core") {
            version {
                strictly("[2.0.0, 3.0.0[")
            }
        }
        implementation("org.aiddl.util:aiddl-util") {
            version {
                strictly("[1.0.0, 2.0.0[")
            }
        }
    }
    
    test {
          	testLogging {
    		showStandardStreams = true
    	}
    }
    
    publishing {
        publications {
            maven(MavenPublication) {
                groupId = 'org.aiddl.common'
                artifactId = 'aiddl-common'
                version = '2.0.0'
    
                from components.java
            }
        }
    }
    
    task sourcesJar(type: Jar, dependsOn: classes) {
        classifier = 'sources'
        from sourceSets.main.allSource
    }
    
    task javadocJar(type: Jar, dependsOn: javadoc) {
        classifier = 'javadoc'
        from javadoc.destinationDir
    }
    
    artifacts {
        archives sourcesJar
        archives javadocJar
    }
    
    
    defaultTasks 'clean', 'build'apply plugin: 'java-library'
    apply plugin: 'maven-publish'
    
    sourceCompatibility = 1.8
    targetCompatibility = 1.8
    
    group = 'org.aiddl.common'
    version = '2.0.0'
    
    repositories {
        mavenCentral()
        mavenLocal()
    }
    
    dependencies {
        testImplementation 'junit:junit:4.12'
        implementation("org.aiddl.core:aiddl-core") {
            version {
                strictly("[2.0.0, 3.0.0[")
            }
        }
        implementation("org.aiddl.util:aiddl-util") {
            version {
                strictly("[1.0.0, 2.0.0[")
            }
        }
    }
    
    test {
          	testLogging {
    		showStandardStreams = true
    	}
    }
    
    publishing {
        publications {
            maven(MavenPublication) {
                groupId = 'org.aiddl.common'
                artifactId = 'aiddl-common'
                version = '2.0.0'
    
                from components.java
            }
        }
    }
    
    task sourcesJar(type: Jar, dependsOn: classes) {
        classifier = 'sources'
        from sourceSets.main.allSource
    }
    
    task javadocJar(type: Jar, dependsOn: javadoc) {
        classifier = 'javadoc'
        from javadoc.destinationDir
    }
    
    artifacts {
        archives sourcesJar
        archives javadocJar
    }
    
    defaultTasks 'clean', 'build'
