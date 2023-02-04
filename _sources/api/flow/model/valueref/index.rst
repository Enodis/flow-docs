:orphan:

:py:mod:`flow.model.valueref`
=============================

.. py:module:: flow.model.valueref


Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.model.valueref.IValueRef
   flow.model.valueref.SingletonRef
   flow.model.valueref.ListRef




.. py:class:: IValueRef

   Bases: :py:obj:`abc.ABC`, :py:obj:`Generic`\ [\ :py:obj:`T`\ ]

   Interface for objects that are constructed at an input port for the
   arguments of the actor.

   .. py:method:: box(values)
      :abstractmethod:

      Set the value.


   .. py:method:: extend(values)
      :abstractmethod:

      Add a sequence of values to the actor argument. For most value types
      this will add `values` to the end of the sequence, and potentially raise
      an exception when the capacity has run out.


   .. py:method:: unbox()
      :abstractmethod:

      Create the value to be passed as the actor argument.



.. py:class:: SingletonRef(capacity)

   Bases: :py:obj:`IValueRef`\ [\ :py:obj:`T`\ ]

   A single value. It requires exactly one value.

   .. py:method:: box(value)

      Set the value.


   .. py:method:: extend(values)

      Add a sequence of values to the actor argument. For most value types
      this will add `values` to the end of the sequence, and potentially raise
      an exception when the capacity has run out.


   .. py:method:: unbox()

      Create the value to be passed as the actor argument.



.. py:class:: ListRef(capacity)

   Bases: :py:obj:`IValueRef`\ [\ :py:obj:`T`\ ]

   A list of values.

   .. py:method:: box(values)

      Set the value.


   .. py:method:: extend(values)

      Add a sequence of values to the actor argument. For most value types
      this will add `values` to the end of the sequence, and potentially raise
      an exception when the capacity has run out.


   .. py:method:: unbox()

      Create the value to be passed as the actor argument.



