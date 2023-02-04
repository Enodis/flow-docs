:py:mod:`flow.processor.asynch.processor`
=========================================

.. py:module:: flow.processor.asynch.processor

.. autoapi-nested-parse::

   Execute the nodes of a data flow graph with dynamic event-driven system.

   Async execution of a data flow graph is useful when data is received, live,
   from external sources. In streaming execution the node cannot get the entire
   input data as the data may be continuous, so also the output data must be
   written piecemeal.



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.processor.asynch.processor.IAsyncOperation
   flow.processor.asynch.processor.AsyncOperation
   flow.processor.asynch.processor.AsyncTask
   flow.processor.asynch.processor.AsyncGenerator
   flow.processor.asynch.processor.AsyncProgram
   flow.processor.asynch.processor.AsyncProcessor




.. py:class:: IAsyncOperation

   Bases: :py:obj:`abc.ABC`

   An asynchronous operation has a name. It has an `initialize` method that is
   called once during the initialization phase, and a `execute` method that is
   called once during the execution phase of the program.

   .. py:method:: initialize(state)
      :abstractmethod:

      Connect to the relevant input and output queues from the processor state.

      :param state: the collection of queues of the `AsyncProcessor`.


   .. py:method:: execute()
      :abstractmethod:
      :async:

      Start the execution of this operation. Termination of the operation is
      dependent on the implementation of this method.



.. py:class:: AsyncOperation(name, inmap, outmap)

   Bases: :py:obj:`IAsyncOperation`

   Common base for streaming operations with the sequence: read inputs,
   execute, write outputs.

   .. py:method:: initialize(state)

      Connect to the relevant input and output queues from the processor state.

      :param state: the collection of queues of the `AsyncProcessor`.


   .. py:method:: terminate()
      :async:

      Close all input and output queues.

      This method should not be called directly, but it may be overriden in
      subclasses. Termination of a node should be done by returning from
      `AsyncOperation.main`, normally or with an exception.


   .. py:method:: execute()
      :async:

      Start the execution of this operation. Termination of the operation is
      dependent on the implementation of this method.


   .. py:method:: read_inputs()
      :async:

      Read values from the input queues, waiting when necessary.

      This method may raise `QueueClosedError`, the default behavior is to
      terminate the node, closing all connected queues.

      :returns: A dict with values for each input port.


   .. py:method:: write_outputs(values)
      :async:

      Write values to the output queues.

      :param values: a dict with a value to put to each output port.


   .. py:method:: main()
      :abstractmethod:
      :async:

      Implementation of the operation.  Queue closure, and proper termination
      of the operation is handled outside this function. The default response
      to queue closure on any input or output is to terminate execution and
      close all other queues.



.. py:class:: AsyncTask(name, runner, inmap, outmap)

   Bases: :py:obj:`AsyncOperation`

   An operation that performs a function on inputs producing outputs.

   .. py:method:: main()
      :async:

      Implementation of the operation.  Queue closure, and proper termination
      of the operation is handled outside this function. The default response
      to queue closure on any input or output is to terminate execution and
      close all other queues.


   .. py:method:: terminate()
      :async:

      Close all input and output queues.

      This method should not be called directly, but it may be overriden in
      subclasses. Termination of a node should be done by returning from
      `AsyncOperation.main`, normally or with an exception.


   .. py:method:: initialize(state)

      Connect to the relevant input and output queues from the processor state.

      :param state: the collection of queues of the `AsyncProcessor`.


   .. py:method:: execute()
      :async:

      Start the execution of this operation. Termination of the operation is
      dependent on the implementation of this method.


   .. py:method:: read_inputs()
      :async:

      Read values from the input queues, waiting when necessary.

      This method may raise `QueueClosedError`, the default behavior is to
      terminate the node, closing all connected queues.

      :returns: A dict with values for each input port.


   .. py:method:: write_outputs(values)
      :async:

      Write values to the output queues.

      :param values: a dict with a value to put to each output port.



.. py:class:: AsyncGenerator(name, runner, outmap)

   Bases: :py:obj:`AsyncOperation`

   An operation for iterators without inputs

   .. py:method:: main()
      :async:

      Implementation of the operation.  Queue closure, and proper termination
      of the operation is handled outside this function. The default response
      to queue closure on any input or output is to terminate execution and
      close all other queues.


   .. py:method:: initialize(state)

      Connect to the relevant input and output queues from the processor state.

      :param state: the collection of queues of the `AsyncProcessor`.


   .. py:method:: terminate()
      :async:

      Close all input and output queues.

      This method should not be called directly, but it may be overriden in
      subclasses. Termination of a node should be done by returning from
      `AsyncOperation.main`, normally or with an exception.


   .. py:method:: execute()
      :async:

      Start the execution of this operation. Termination of the operation is
      dependent on the implementation of this method.


   .. py:method:: read_inputs()
      :async:

      Read values from the input queues, waiting when necessary.

      This method may raise `QueueClosedError`, the default behavior is to
      terminate the node, closing all connected queues.

      :returns: A dict with values for each input port.


   .. py:method:: write_outputs(values)
      :async:

      Write values to the output queues.

      :param values: a dict with a value to put to each output port.



.. py:class:: AsyncProgram(tasks, queues)

   A program is the representation of a data flow graph that can be executed by
   a `AsyncProcessor`. No run-time entities are created yet, that will be
   done during execution of the program.

   The normal way of creating a AsyncProgram is by calling the
   `AsyncProgram.compile` class method.

   A streaming program specifies a collection of tasks to be executed
   simultaneously, and a collection of queues that represent the communication
   between the tasks.

   :param tasks: the tasks that represent the actor nodes in the data flow graph.
   :param queues: the communication between the actor nodes in the data flow
   :param graph.:

   .. py:method:: compile(g)
      :classmethod:

      Compile a data flow graph into a streaming program.

      :param g: the data flow graph to compile.

      :returns: A program that contains the specification for the execution of the
                data flow graph for a `AsyncProcessor`.



.. py:class:: AsyncProcessor

   A streaming processor can execute a `AsyncProgram`.

   .. py:method:: execute(program)
      :async:

      Execute a data flow graph.

      :param program: a `Model` which will be compiled and executed, or a
      :param `AsyncProgram` which will be executed.:



