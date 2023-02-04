:py:mod:`flow.processor.asynch.channelio`
=========================================

.. py:module:: flow.processor.asynch.channelio

.. autoapi-nested-parse::

   This module implements the communication between 2 nodes: the channel. A channel
   is a queue with a single producer and a single consumer. The flow channel
   supports efficiently *putting* and *getting* sequences of values of different
   sizes. These can be lists, numpy arrays, pandas dataframes. In order to achieve
   this, the sequences put on the queue must be immutable, they must not be
   modified after they are added.

   The interfaces to the channel are provided by `ChannelIO`. It provides one
   interface for the producer, the `WriterFront`, and one interface for the
   consumer, the `ReaderFront`.



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.processor.asynch.channelio.ReaderFront
   flow.processor.asynch.channelio.WriterFront
   flow.processor.asynch.channelio.ChannelIO




.. py:class:: ReaderFront

   Bases: :py:obj:`_Front`\ [\ :py:obj:`T`\ ]

   Interface for reading a sequence of items from an async `ChannelIO`.

   .. py:method:: get(to)
      :async:

      Get the top N items on the queue, where N is the capacity specified in
      `to`. The number of items retrieved from the queue in this way does not
      need to match the size of the sequence as it was put onto the queue
      using `WriterFront.put`.

      When there are insufficient items in the queue, this method will wait or
      raise the `flow.processor.asynch.channelqueue.QueueClosedError`
      exception if the other end has closed the connection.

      :param to: the (capacity limited) value reference in which the value is
      :param constructed:

      Returns: `to`

      :raises flow.processor.asynch.channelqueue.QueueClosedError: when the
      :raises other end has closed the connection, but more items are requested.:


   .. py:method:: close()
      :async:

      Close the channel, also for the producer.



.. py:class:: WriterFront

   Bases: :py:obj:`_Front`\ [\ :py:obj:`T`\ ]

   Interface for writing a sequence of items to an async `ChannelIO`.

   .. py:method:: put(values)
      :async:

      Append a sequence of `values` to the queue. The values can be retrieved
      using `ReaderFront.get`. Low indices of the sequence are appended first,
      such that the order of the sequence is maintained even if the values are
      taken from the queue with multiple calls.

      When the queue has insufficient capacity for the value, this method will
      wait until room is available, or until the consumer closes the
      connection, in which case
      `flow.processor.asynch.channelqueue.QueueClosedError` will be raised.

      .. warning::

          The sequence is not copied! Make sure that the producer does not
          modify the values after the sequence has been put onto the queue.

      :param values: the values to append to the queue

      :raises flow.processor.asynch.channelqueue.QueueClosedError: when the
      :raises consumer has closed the connection.:


   .. py:method:: close()
      :async:

      Close the channel, also for the consumer.



.. py:class:: ChannelIO(*, maxsize = 0)

   Bases: :py:obj:`_Base`\ [\ :py:obj:`T`\ ]

   I/O interfaces to access a channel

   .. py:method:: reader()

      The interface to get objects from the queue.


   .. py:method:: writer()

      The interface to put objects in the queue



