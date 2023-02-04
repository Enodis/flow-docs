# Design of flow.processor

All forms of execution have the following components:

* **Processor**: an object responsible for the actual execution of the data flow
  graph
* **Program**: the specification for a processor on the actions it should take
  to execute a particular data flow graph.
* **compilation**: the process of turning a data flow graph `flow.model.Model` into a
  **Program**.
* **Operation**: the means to execute a `flow.model.runner.Runner` for the given
  *processor*.
