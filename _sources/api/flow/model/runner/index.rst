:py:mod:`flow.model.runner`
===========================

.. py:module:: flow.model.runner

.. autoapi-nested-parse::

   Runners for various types of Python objects.



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.model.runner.RunnerInit
   flow.model.runner.Runner
   flow.model.runner.StatelessRunner
   flow.model.runner.IteratorRunner
   flow.model.runner.StatefulRunnerInit
   flow.model.runner.StatefulRunner




.. py:class:: RunnerInit

   Bases: :py:obj:`Generic`\ [\ :py:obj:`T`\ ]

   Data members for a `Runner`.

   The type argument of Builder is the return type of the `function`.

   :param function: The callable that implements the actor with return type `T`


.. py:class:: Runner

   Bases: :py:obj:`RunnerInit`\ [\ :py:obj:`T`\ ]

   Encapsulation for the implementation of a node.

   .. py:method:: initialize()

      initialize before the execution of the whole graph starts.


   .. py:method:: execute(inputs = None)

      Execute the actor.

      :param inputs: The input values for a single execution of
                     the actor.

      :returns: An object that represents the values for all output
                ports of the actors. The actual type depends on the type
                of runner, but it must boil down to a dict with an object
                for each output port.



.. py:class:: StatelessRunner

   Bases: :py:obj:`Runner`\ [\ :py:obj:`RunnerOutputs`\ ]

   A runner for stateless actors. These can be implemented by pure functions.

   .. py:method:: initialize()

      initialize before the execution of the whole graph starts.


   .. py:method:: execute(inputs = None)

      Execute the actor.

      :param inputs: The input values for a single execution of
                     the actor.

      :returns: An object that represents the values for all output
                ports of the actors. The actual type depends on the type
                of runner, but it must boil down to a dict with an object
                for each output port.



.. py:class:: IteratorRunner

   Bases: :py:obj:`Runner`\ [\ :py:obj:`Iterator`\ [\ :py:obj:`RunnerOutputs`\ ]\ ]

   A runner for iterator actors. Iterator actors have no input ports, and a
   **single** output port. Each iteration sends new values to the output port.

   .. py:method:: initialize()

      initialize before the execution of the whole graph starts.


   .. py:method:: execute(inputs = None)

      Execute the actor.

      :param inputs: The input values for a single execution of
                     the actor.

      :returns: An object that represents the values for all output
                ports of the actors. The actual type depends on the type
                of runner, but it must boil down to a dict with an object
                for each output port.



.. py:class:: StatefulRunnerInit

   Bases: :py:obj:`RunnerInit`\ [\ :py:obj:`T`\ ]

   The data members for `StatefulRunner`.
   :param function: The function or method that implements a single execution of the actor.
   :param constructor: The callable that will create and initialize the actor state.
   :param destructor: The callable that will clean-up the actor state.


.. py:class:: StatefulRunner

   Bases: :py:obj:`Runner`\ [\ :py:obj:`RunnerOutputs`\ ], :py:obj:`StatefulRunnerInit`\ [\ :py:obj:`RunnerOutputs`\ ]

   A runner for stateful actors.

   Stateful actors are objects constructed and configured during
   `Runner.initialize`. An execution returns a dict with objects for each
   output port.

   :param builder: the `StatefulBuilder` that created this runner.
   :param kwargs: the configuration dict of this runner.

   .. py:method:: initialize()

      initialize before the execution of the whole graph starts.


   .. py:method:: execute(inputs = None)

      Execute the actor.

      :param inputs: The input values for a single execution of
                     the actor.

      :returns: An object that represents the values for all output
                ports of the actors. The actual type depends on the type
                of runner, but it must boil down to a dict with an object
                for each output port.



