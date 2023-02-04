:py:mod:`flow.processor.offline.channelio`
==========================================

.. py:module:: flow.processor.offline.channelio

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

   .. note:: Design

       I chose to create separate objects for the reader and write because
       sometimes they must offer the same function, but with different
       functionality, like `close`.

       The `ChannelIO` encapsulates a single
       `flow.processor.offline.channelqueue.ChannelQueue`. The difference is that
       ChannelQueue is a slightly modified version of a normal Python Queue, that
       just puts and gets single elements, and `ChannelIO` adds the ability to put
       and get sequences of elements efficiently. It achieves this efficiency by
       simply putting the sequences that were put, and construcing new sequences
       (using `flow.processor.slicer.Window`) when values are retrieves from the
       queue.



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.processor.offline.channelio.ReaderFront
   flow.processor.offline.channelio.WriterFront
   flow.processor.offline.channelio.ChannelIO




.. py:class:: ReaderFront

   Bases: :py:obj:`_Front`\ [\ :py:obj:`T`\ ]

   Interface for reading a sequence of items from an offline `ChannelIO`.

   .. py:method:: get(to)

      Get the top N items on the queue, where N is the capacity specified in
      `to`. The number of items retrieved from the queue in this way does not
      need to match the size of the sequence as it was put onto the queue
      using `WriterFront.put`.

      :param to: the (capacity limited) value reference in which the value is
      :param constructed:

      Returns: `to`

      :raises ValueError: if there are insufficient items in the queue.



.. py:class:: WriterFront

   Bases: :py:obj:`_Front`\ [\ :py:obj:`T`\ ]

   Interface for writing a sequence of items to an offline `ChannelIO`.

   .. py:method:: put(values)

      Append a sequence of `values` to the queue. The values can be retrieved
      using `ReaderFront.get`. Low indices of the sequence are appended first,
      such that the order of the sequence is maintained even if the values are
      taken from the queue with multiple calls.

      .. warning::

          The sequence is not copied! Make sure that the producer does not
          modify the values after the sequence has been put onto the queue.

      :param values: the values to append to the queue



.. py:class:: ChannelIO

   Bases: :py:obj:`_Base`\ [\ :py:obj:`T`\ ]

   I/O interfaces for a channel.

   .. py:method:: reader()

      The interface to get objects from the queue.


   .. py:method:: writer()

      The interface to put objects in the queue



