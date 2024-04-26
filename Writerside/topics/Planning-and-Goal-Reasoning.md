# Planning and Goal Reasoning

The same objects can be refered to by many names.  In planning locations may
often be listed with names such as `loc-a` that do not give an indication about
the type of location.  User provided goals are likely not in terms of the
symbols used by the planner. For instance a goal may require the robot to go to
a `kitchen`. In this example, we use a reasoner to translate goals provided to a
planner to goals that the planner can understand. As an input, we use a graph
that describes sub-class relationships and our reasoner simply attemts to find a
path from symbols unknown to the planner to symbols known to the planner.

Go
[here](https://github.com/uwe-koeckemann/AIDDL/tree/master/example/planning-and-goal-inference)
for instructions to run the example.

