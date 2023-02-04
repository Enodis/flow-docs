:py:mod:`flow.processor.tools.slicer`
=====================================

.. py:module:: flow.processor.tools.slicer

.. autoapi-nested-parse::

   The Slicer provides an easy way to get successive slices from a sequence.
   This is useful in the construction of value sequences from the (sequence)
   items in a queue, like `flow.processor.offline.channelqueue.ChannelQueue`.



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.processor.tools.slicer.Slicer




.. py:class:: Slicer(values)

   Bases: :py:obj:`Generic`\ [\ :py:obj:`T`\ ]

   Create a new slicer from an existing sequence.

   :param values: a sequence to be sliced.

   .. py:method:: take(count = None)

      Take the next `count` elements from this slicer. If `count` is *None*,
      take all remaining elements.

      :param count: number of elements to return

      :returns: a next slice, of length `count`, of the original sequence



