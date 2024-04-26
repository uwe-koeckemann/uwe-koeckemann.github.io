# Writing a Simple Program

## Java

The following program is a simple hello world. It imports two of the representation classes,
creates a `StringTerm` and prints it to the console.

<tabs>
<tab title="Scala">

``` Scala
// TODO
```
</tab>
<tab title="Python">

``` Python
# TODO
```
</tab>
<tab title="Java">

``` Java
package org.aiddl.hello_world;

import org.aiddl.core.representation.Term;
import org.aiddl.core.representation.StringTerm;

public class HelloAIDDL {
    public static void main( String[] args ) {
        StringTerm helloAiddl = Term.string("Hello AIDDL World!");
        System.out.println(helloAiddl);
    }
}
```
</tab>
</tabs>




To run this program, create a Java application gradle project and run it with:

<tabs>
<tab title="Scala">

```
sbt run
```
</tab>
<tab title="Python">

```
python main.py
```
</tab>
<tab title="Java">
<tabs>
<tab title="Windows">

```
gradlew run
```
</tab>
<tab title="Linux">

```
./gradlew run
```
</tab>
</tabs>
</tab>
</tabs>


        