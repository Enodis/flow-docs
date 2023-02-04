:py:mod:`flow.lang.composition`
===============================

.. py:module:: flow.lang.composition

.. autoapi-nested-parse::

   Composition of data flow graphs



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.lang.composition.Pipeline



Functions
~~~~~~~~~

.. autoapisummary::

   flow.lang.composition.graph
   flow.lang.composition.bundle



.. py:function:: graph()

   A context manager that should be used to create flow graphs. Using this
   context manager creates a new :py:mod:`flow.model.graph.Model` and makes it
   active in the `with` context.


.. py:class:: Pipeline(head, tail)

   Bases: :py:obj:`flow.lang.types.IEndPoint`

   A helper class that allows nodes to be connected with >>


.. py:function:: bundle(**kwargs)

   Create a bundle from a number of pipelines.

   A bundle is a pipelinable collection of named pipelines.


