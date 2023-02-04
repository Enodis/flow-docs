:orphan:

:py:mod:`flow.lang.builder`
===========================

.. py:module:: flow.lang.builder


Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.lang.builder.Builder




.. py:class:: Builder

   Bases: :py:obj:`Generic`\ [\ :py:obj:`T`\ ]

   An object that can configure and create a `Runner` objects for a
   Python implementation for a node.

   The type argument of Builder is the return type of the `function`.

   :param init: The initialization of the runner.

   .. py:method:: specialize(kwargs, needs)

      Provide configuration parameters for this actor.

      :param kwargs: a dictionary of parameters for the actor function.

      :returns: A runner object for the function. A single builder can be used to
                create multiple runners.



