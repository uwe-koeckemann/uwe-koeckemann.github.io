# Java

We use `<AIDDL>` as the root of the [aiddl.org](http://www.aiddl.org) git
repository. To install the AIDDL libraries, open a console in the root of the
library you would like to install:

1. Core library: `<AIDDL>/core/java/`
2. Common library: `<AIDDL>/common/java/`

For each of these (in the listed order) run the following on the command line:

<tabs>
<tab title="Linux">

        ./gradlew publishToMavenLocal
</tab>
<tab title="Windows">

        gradlew publishToMavenLocal
</tab>
</tabs>


## Project Setup with Gradle

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

### Application Gradle File Example {collapsible="true" default-state="collapsed"}

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
     
### Library Gradle File Example  {collapsible="true" default-state="collapsed"}

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

       
## Troubleshooting

- Gradle error: Class file compiled with a different version
  - Cause: Your Java version is probably too new for the Gradle version used by
    the wrapper.
  - Solution: Update the Gradle wrapper to the most recent version `<VER>` of
    gradle, or use an older version of Java (in case gradle does not support
    your java version yet). Please contact us, so we can update the wrapper in
    the repository as well. Meanwhile the following command should do the trick:
    
        ./gradlew wrapper --gradle-version=<VER>
