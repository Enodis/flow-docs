:py:mod:`flow.processor.asynch.channelqueue`
============================================

.. py:module:: flow.processor.asynch.channelqueue

.. autoapi-nested-parse::

   Safe queue for asyncio.

   The Queue implementation in this module provides:

   1. Separation of reader and writer interface
   2. Queue hang-up from both sides that trigger an exception at the other end.
   3. Propagation of exceptions by reraising them when they are sent over a queue.



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.processor.asynch.channelqueue.ReaderFront
   flow.processor.asynch.channelqueue.WriterFront
   flow.processor.asynch.channelqueue.ChannelQueue




.. py:exception:: QueueClosedError

   Bases: :py:obj:`RuntimeError`

   Exception that will be raised for a waiting `get` or `put` action when the
   other end hangs-up.

   .. py:method:: with_traceback()

      Exception.with_traceback(tb) --
      set self.__traceback__ to tb and return self.



.. py:class:: ReaderFront(base)

   Bases: :py:obj:`_Front`\ [\ :py:obj:`T`\ ]

   Reader/receiver access to a `Queue`

   .. py:property:: is_closed
      :type: bool

      The queue has been closed.

   .. py:method:: fetch()
      :async:

      Fetch the top item on the queue, but do not remove it.

      :returns: The top item on the queue

      :raises QueueClosedError: when a new item is required but the
      :raises WriterFront:


   .. py:method:: drop()

      Remove the top item from the queue.


   .. py:method:: close()
      :async:

      Close the queue. This will raise an exception with a waiting
      `WriterFront`, and it will remove all existing entries from the queue.

      The queue cannot be used by either end after the reader has been closed.



.. py:class:: WriterFront(base)

   Bases: :py:obj:`_Front`\ [\ :py:obj:`T`\ ]

   Writer/sender access to a `Queue`

   .. py:property:: is_closed
      :type: bool

      The queue has been closed.

   .. py:method:: put(value)
      :async:

      Put a single value in the queue.

      This may block if the capacity of the queue is limited. When the queue
      is closed, from either `ReaderFront` or `WriterFront`, this will raise
      `QueueClosedError`.

      :param value: the value to put in the queue.


   .. py:method:: close()
      :async:

      Close the queue. This will raise an exception with a waiting
      `ReaderFront`. The queue is

      The queue cannot be used by this writer afterwards, but it can be used
      by the reader to get the remaining values. Only when all remaining
      values are retrieved will the reader get a `QueueClosedError`.



.. py:class:: ChannelQueue(*, maxsize = 0)

   Bases: :py:obj:`_Base`\ [\ :py:obj:`T`\ ]

   Construct a "safe" queue for a single producer and a single consumer. An
   optional `maxsize` can be passed to limit the capacity of the queue. When
   the maximum capacity has been reached the `writer` will wait until room is
   available in the queue. Without `maxsize` the capacity is "infinite".

   .. py:method:: reader()

      The interface to get objects from the queue.


   .. py:method:: writer()

      The interface to put objects in the queue



