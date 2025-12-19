# Actor Service

Actors represent the capability to execute actions.

## Using a gRPC Actor (Client)

Assuming that an Actor service is offered on IP `0.0.0.0` and port `8065`, you can connect to it by using

``` Scala
import org.aiddl.core.scala.container.Container
import org.aiddl.external.grpc.scala.actor.ActorClient

@main def actorClientExample = {
  val c = new Container()
  val a: ActorClient = new ActorClient("0.0.0.0", 8065, c)
  val action = Sym("drive")
  a.dispatch(action)
}
```

After this, `a` can be used as a regular actor (see Acting, Sensing & Control)

## Creating a gRPC Actor (Server)