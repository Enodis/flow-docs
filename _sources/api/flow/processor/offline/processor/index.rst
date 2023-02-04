:py:mod:`flow.processor.offline.processor`
==========================================

.. py:module:: flow.processor.offline.processor

.. autoapi-nested-parse::

   Execute the nodes of a data flow graph with a pre-compiled sequence of operations.

   Offline execution of a data flow graph is possible when all input data for the
   data flow graph is present before execution. In offline execution the node may
   accept the entire input data, and produce all output data at once.



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.processor.offline.processor.IOfflineOperation
   flow.processor.offline.processor.OfflineOperation
   flow.processor.offline.processor.OfflineFunction
   flow.processor.offline.processor.OfflineIterator
   flow.processor.offline.processor.OfflineProgram
   flow.processor.offline.processor.OfflineProcessor




.. py:class:: IOfflineOperation

   Bases: :py:obj:`abc.ABC`

   Interface used by an `OfflineProcessor` to execute the operations of a
   program.

   .. py:method:: execute(state)
      :abstractmethod:

      Excute this operation on the given `OfflineProcessor` state.

      :param state: The state from which the operation should get and put values.



.. py:class:: OfflineOperation(name, inmap, outmap)

   Bases: :py:obj:`IOfflineOperation`

   Common structure for an offline operation with separate steps for reading
   inputs, execution, and writing outputs.

   This class should not be used directly.

   .. py:method:: ready_to_read(state)

      Check whether there is sufficient input to read all required input
      values.

      :param state: the processor state from which to retrieve the input values.


   .. py:method:: read_inputs(state)

      Read all inputs from processor state.

      :param state: the processor state from which to retrieve the input values.

      :returns: A dict with the value for each input name.


   .. py:method:: write_outputs(state, values)

      Write output values to processor state.

      :param state: the processor state to write the output values to
      :param values: a dict with a value for each output name


   .. py:method:: execute(state)
      :abstractmethod:

      Excute this operation on the given `OfflineProcessor` state.

      :param state: The state from which the operation should get and put values.



.. py:class:: OfflineFunction(name, runner, inmap, outmap)

   Bases: :py:obj:`OfflineOperation`

   An operation that performs a function on inputs producing outputs.

   .. py:method:: execute(state)

      Excute this operation on the given `OfflineProcessor` state.

      :param state: The state from which the operation should get and put values.


   .. py:method:: ready_to_read(state)

      Check whether there is sufficient input to read all required input
      values.

      :param state: the processor state from which to retrieve the input values.


   .. py:method:: read_inputs(state)

      Read all inputs from processor state.

      :param state: the processor state from which to retrieve the input values.

      :returns: A dict with the value for each input name.


   .. py:method:: write_outputs(state, values)

      Write output values to processor state.

      :param state: the processor state to write the output values to
      :param values: a dict with a value for each output name



.. py:class:: OfflineIterator(name, runner, outmap)

   Bases: :py:obj:`OfflineOperation`

   An operation for iterators without inputs

   .. py:method:: execute(state)

      Excute this operation on the given `OfflineProcessor` state.

      :param state: The state from which the operation should get and put values.


   .. py:method:: ready_to_read(state)

      Check whether there is sufficient input to read all required input
      values.

      :param state: the processor state from which to retrieve the input values.


   .. py:method:: read_inputs(state)

      Read all inputs from processor state.

      :param state: the processor state from which to retrieve the input values.

      :returns: A dict with the value for each input name.


   .. py:method:: write_outputs(state, values)

      Write output values to processor state.

      :param state: the processor state to write the output values to
      :param values: a dict with a value for each output name



.. py:class:: OfflineProgram(operations)

   Bases: :py:obj:`Iterable`\ [\ :py:obj:`IOfflineOperation`\ ]

   A program is the representation of a data flow graph that can be executed by
   an `OfflineProcessor`. No run-time entities are created yet, that will be
   done during execution of the program.

   The normal way of creating an OfflineProgram is by calling the
   `OfflineProgram.compile` class method.

   An OfflineProgram is simply a sequence of operations that adhere to
   `IOfflineOperation`.

   :param operations: the sequence of operations that comprise this program.

   .. py:method:: compile(g)
      :classmethod:

      Compile a data flow graph into an offline program.

      :param g: the data flow graph to compile.

      :returns: A program that contains the specification for the execution of the
                data flow graph for an `OfflineProcessor`.



.. py:class:: OfflineProcessor

   An offline processor can execute an `OfflineProgram`.

   .. py:method:: execute(program)

      Execute a data flow graph.

      :param program: a `Model` which will be compiled and executed, or an
                      `OfflineProgram` which will be executed.



