# Learning for Planning

This example uses a planner in a simple domain. Initially there are no operators
but a set of actions with unknown preconditions and effects. Over time the
system will learn operators from observerd state transitions which will allow it
to hopefully reach the goal.

- Loop:
    1. Try to find a plan
        - No plan found: try random action
        - Plan found: execute plan
    2. Record state transition data
    3. Learn a decision tree to predict action effects given action and state
    4. Convert decision tree to operators

Go
[here](https://github.com/uwe-koeckemann/AIDDL/tree/master/example/learning-agent)
for instructions to run this example.

