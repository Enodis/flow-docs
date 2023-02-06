:py:mod:`flow.lang.types`
=========================

.. py:module:: flow.lang.types

.. autoapi-nested-parse::

   Abstract types for the data flow language.



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.lang.types.IPipelinable
   flow.lang.types.IEndPoint
   flow.lang.types.IConnectable
   flow.lang.types.IChannel
   flow.lang.types.IPort
   flow.lang.types.IOutput
   flow.lang.types.INode




.. py:exception:: CompositionError

   Bases: :py:obj:`Exception`

   Common base class for all non-exit exceptions.

   .. py:method:: with_traceback()

      Exception.with_traceback(tb) --
      set self.__traceback__ to tb and return self.



.. py:class:: IPipelinable

   Bases: :py:obj:`abc.ABC`

   Helper class that provides a standard way to create an ABC using
   inheritance.


.. py:class:: IEndPoint

   Bases: :py:obj:`IPipelinable`

   Helper class that provides a standard way to create an ABC using
   inheritance.


.. py:class:: IConnectable

   Bases: :py:obj:`abc.ABC`

   Helper class that provides a standard way to create an ABC using
   inheritance.


.. py:class:: IChannel

   Bases: :py:obj:`IConnectable`

   Helper class that provides a standard way to create an ABC using
   inheritance.


.. py:class:: IPort(node, spec)

   Bases: :py:obj:`flow.model.PortSpec`, :py:obj:`IPipelinable`

   Helper class that provides a standard way to create an ABC using
   inheritance.


.. py:class:: IOutput(node, spec)

   Bases: :py:obj:`IPort`

   Helper class that provides a standard way to create an ABC using
   inheritance.


.. py:class:: INode

   Bases: :py:obj:`IPipelinable`

   Helper class that provides a standard way to create an ABC using
   inheritance.


