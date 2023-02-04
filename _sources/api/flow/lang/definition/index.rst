:py:mod:`flow.lang.definition`
==============================

.. py:module:: flow.lang.definition

.. autoapi-nested-parse::

   Definition of data flow nodes from Python objects.



Module Contents
---------------


Functions
~~~~~~~~~

.. autoapisummary::

   flow.lang.definition.nodedef
   flow.lang.definition.node



.. py:function:: nodedef(c, *, windows = None, **kwargs)

   Create a node constructor for `c`. This is a useful when the result is
   assigned to a class variable as an alternative to the namespace assignment
   of `node`.

   :param c: The callable for which a node constructor must be created.
   :param windows: the window size of each port. Defaults to 1 for each port.

   :returns: A node constructor.


.. py:function:: node(c = None, *, namespace = Namespace, windows = None)

   Decorator to turn a function or dataclass into a flow node.

   :param c: The callable for which a node constructor must be created.
   :param windows: the window size of each port. Defaults to 1 for each port.

   :returns: The callable `c`. This means that the decorator leaves the decorated
             function in tact.


