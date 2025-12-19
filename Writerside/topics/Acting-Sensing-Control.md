# Acting, Sensing &amp; Control

This section covers how to interface with external processes. For this purpose the Scala common library
offers several abstractions. 

1. Actor: execute actions and monitor their status
2. Sensor: read information from external sources
3. Controller: given a goal, actor, and sensor, choose actions to achieve the goal
4. Dispatcher: manage executions of plans (e.g., sequential, partial-order) and deal with execution errors

All of these abstractions extend the `Tickable` trait whose purpose is to allow 
to control the frequency with which they update their internal states.

## Actor

The actor's responsibility is to execute some external process and keep track of
the status of the execution. Examples include running a program binary, sending a robot
to a location, or even sending a task to another agent. 

Typical usage of the Actor is:

1. Dispatch an action
2. Tick the actor regularly until it's done
3. Dispatch next action

The interface supports checking if action terms are supported, dispatching them, canceling and checking on the status.

- `supported(action: Term) : Boolean` checks if action is supported
- `dispatch(action: Term) : Option[ActionInstanceId]` attempts to dispatch an action and returns an ID if successful or `None` otherwise.
- `dispatchBlock(action: Term) : Option[ActionInstanceId]` same as dispatch, but will block until action is completed
- `cancel(id: ActionInstanceId)` attempt to cancel action given its ID (may or may not be supported)
- `status(id: ActionInstanceId)` read the current execution status of an action
- `idle: Boolean` check if the actor is currently executing any action
- `registerCallback(f: (ActionInstanceId, Term, Status) => Unit ): Unit` register callback function that takes an ID, the action term, and a status as arguments and will be called when the status changes.
- `tick: Unit` checks on the external process (needs to be overwritten when Actor is implemented) 

The status of a dispatched action is one of the following:

- `Pending` dispatched but not started
- `Active` currently running
- `Succeeded` successfully completed
- `Error(code: Term, msg: String)` something went wrong
- `Recalling` cancelling action that has not started yet
- `Recalled` cancelled action that never started
- `Preempting` cancelling action while its running
- `Preempted` cancelled action while it was running

This set of states is based on the Robot Operating System 1 (ROS1) and might be 
simplified in the future to match the more recent ROS2 actions which merge recall and preempt into cancel. 

## Sensor

A sensor value consists of three elements:

- a value term: what was sensed
- a sequence ID
- a nanosecond timestamp (taken when `performSenseAndUpdate` is called)

Sensors have three modes: 

- `OnDemand` mode performs a sense operation when `sense` is called
- `Frequency` mode performs a sense operation when `tick` is called and returns the most recent read on `sense`
- `Mixed` performs sense in both cases

To implement a sensor, extend the `Sensor` class and overwrite the `performSense` method to return the sensed vale. 
Make sure to set the sensor mode when creating concrete instances of your new sensor. 

Example:

```Scala
    object TestSensor extends Sensor {
      override val sensorMode: SensorMode = Mixed
      override protected def performSense: Term = Num(42)
    }
```

## Controller

A controller uses a sensor to read the current state, then calls a `decide` 
method to generate a list of instructions which are then forwarded to its
actor. Callbacks can be registered to read signals.

- `setGoal(goal: Term): Unit`
- `currentGoal: Term`
- `decide(state: Term): List[Instruction]` 
- `enable: Unit`
- `disable: Unit`
- `registerCallback(cb: Signal => Unit): Unit` 
- `removeCallback(cb: Signal => Unit): Unit`
- `tick: Unit`

To create your own controller, override the `decide` method and instantiate with a sensor and actor.
The following controller senses a number and returns a `+` action when it is too low and a `-` action
when it is too large. When the goal is reached it emits the corresponding signal and returns an empty list. 

```Scala
    object NumberController extends Controller {
      override val actor: Actor = NumberSimulator
      override val sensor: Sensor = NumberSimulator

      this.setGoal(sensor.sense.value)

      override def decide(state: Term): List[Instruction] = {
        if ( state.asNum < currentGoal.asNum ) {
          List(Instruction(Sym("+")))
        } else if ( state.asNum > currentGoal.asNum ) {
          List(Instruction(Sym("-")))
        } else {
          this.callback(GoalReached(this.currentGoal))
          Nil
        }
      }
    }
```

See `/test/scala/org/aiddl/common/scala/execution/ControllerSuite.scala` for the full example including the implementation 
of `NumberSimulator`. 


### Controller Instructions

Instructions consist of an optional ID term and the action term. The latter will be dispatched while the former can be 
used to get a unique handle on an action. This is useful in cases where actions may be repeated, but the ordering needs to
be tracked (e.g., an action `load-robot` may happen many times, so a partial-order planner may assign some form of ID 
to keep track of partial-orders) 

### Controller Signals

- Enabled: Controller has been enabled
- Disabled: Controller has been disabled
- Skipped: Controller skipped tick because it is disabled
- GoalReached(goal: Term):  Controller has reached a goal
- GoalUpdated(goal: Term): Controller goal has been updated
- Dispatched(instanceId: ActionInstanceId, actionId: Term, action: Term): Controller has dispatched an action
- UnexpectedActionResult(actionId: Term, action: Term): Action succeeded, but sensed state was not expected
- ActorFailure(instanceId: ActionInstanceId, actionId: Term, action: Term, code: Term, message: String): Actor failed to execute an action
- DispatchFailure(id: Option[Term], action: Term): An action could not be dispatched
- GoalUnreachable(goal: Term): Controller cannot reach goal from sensed state


## Dispatcher

Plans come in a variety of forms with different expectations of when the next action is to 
be executed. 

- A Sequential plan is a list of actions executed in total order.
- A partially-ordered is a set of actions with a partial order between them. Each
  action can be executed once all its predecessors in the partial order have been executed.

The purpose of the dispatcher trait is to have a common interface regardless of the 
type of plan we want to execute. The trait should be implemented for each concrete type of plan.

Currently, we have two implementations for sequential plans and partially-ordered plans.

## See Also

External Libraries -> Services via Protobuf & gRPC 