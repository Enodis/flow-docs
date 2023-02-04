:py:mod:`flow.processor.offline.channelqueue`
=============================================

.. py:module:: flow.processor.offline.channelqueue

.. autoapi-nested-parse::

   The low-level implementation of a channel.



Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.processor.offline.channelqueue.ChannelQueue




.. py:class:: ChannelQueue

   Bases: :py:obj:`Generic`\ [\ :py:obj:`T`\ ]

   A single producer, single consumer queue (FIFO). All values on the queue
   should have the same type. The difference with normal Python Queues is in
   the way items are retrieved from the queue.

   Rather than a *get* method, this queue provides the top item using the
   `fetch` method, but the item is not removed from the queue yet.  This means
   that subsequent calls to `fetch` will return the same value, until a call to
   `drop` is made, which will remove the top item from the queue.

   This functionality is useful when the items are sequences that can be
   partially consumed.

   .. py:method:: put(item)

      Append a single item to the queue.

      :param item: The item to add to the queue.


   .. py:method:: fetch()

      Fetch the top item on the queue, but do not remove it.

      :returns: The top item on the queue

      :raises ValueError: if there are insufficient values.


   .. py:method:: drop()

      Remove the top item from the queue.



