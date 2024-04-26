# Writing a Simple Program

The following program is a simple hello world. It imports two of the representation classes,
creates a string term and prints it to the console.

<tabs>
<tab title="Scala">

``` Scala
import org.aiddl.core.scala.representation.Str

object Main extends App {
  val message = Str("Hello AIDDL World!")
  println(message)
}
```
</tab>
<tab title="Python">

``` Python
from aiddl_core.representation import Str

message = Str("Hello AIDDL World!")
print(message)
```
</tab>
<tab title="Java">

``` Java
package org.aiddl.example.hello_world;

import org.aiddl.core.representation.Term;

public class HelloAIDDL {
    public static void main( String[] args ) {
        StringTerm helloAiddl = Term.string("Hello AIDDL World!");
        System.out.println(helloAiddl);
    }
}
```
</tab>
</tabs>

## How to run

All three examples can be found in the [AIDDL Git repository](https://github.com/uwe-koeckemann/AIDDL/tree/master/example/hello-world).

<tabs>
<tab title="Scala">
In the  <code>hello-world/scala</code> folder:

```
sbt run
```
</tab>
<tab title="Python">
Make sure <code>aiddl_core</code> is available in the current environment.
In the <code>hello-world/python</code> folder run the following command:

```
python main.py
```
</tab>
<tab title="Java">
In the <code>hello-world/java</code> folder run the following command:

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


        