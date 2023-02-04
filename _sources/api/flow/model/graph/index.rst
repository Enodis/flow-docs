:orphan:

:py:mod:`flow.model.graph`
==========================

.. py:module:: flow.model.graph


Module Contents
---------------

Classes
~~~~~~~

.. autoapisummary::

   flow.model.graph.Model




.. py:class:: Model(incoming_graph_data=None, multigraph_input=None, **attr)

   Bases: :py:obj:`networkx.MultiDiGraph`

   Container for a data flow graph.

   .. py:property:: name

      String identifier of the graph.

      This graph attribute appears in the attribute dict G.graph
      keyed by the string `"name"`. as well as an attribute (technically
      a property) `G.name`. This is entirely user controlled.

   .. py:method:: add_actor(name, runner)

      Add an actor to the graph.

      :param name: The name of the actor
      :param runner: The implementation of the actor


   .. py:method:: add_channel(srcname, srcspec, dstname, dstspec)

      Add an channel/edge to the graph.

      :param srcname: The name of the source actor
      :param srcspec: The specification of the output port of the source actor
      :param dstname: The name of the destination actor
      :param dstspec: The specification of the input port of the destination actor


   .. py:method:: adj()

      Graph adjacency object holding the neighbors of each node.

      This object is a read-only dict-like structure with node keys
      and neighbor-dict values.  The neighbor-dict is keyed by neighbor
      to the edgekey-dict.  So `G.adj[3][2][0]['color'] = 'blue'` sets
      the color of the edge `(3, 2, 0)` to `"blue"`.

      Iterating over G.adj behaves like a dict. Useful idioms include
      `for nbr, datadict in G.adj[n].items():`.

      The neighbor information is also provided by subscripting the graph.
      So `for nbr, foovalue in G[node].data('foo', default=1):` works.

      For directed graphs, `G.adj` holds outgoing (successor) info.


   .. py:method:: succ()

      Graph adjacency object holding the successors of each node.

      This object is a read-only dict-like structure with node keys
      and neighbor-dict values.  The neighbor-dict is keyed by neighbor
      to the edgekey-dict.  So `G.adj[3][2][0]['color'] = 'blue'` sets
      the color of the edge `(3, 2, 0)` to `"blue"`.

      Iterating over G.adj behaves like a dict. Useful idioms include
      `for nbr, datadict in G.adj[n].items():`.

      The neighbor information is also provided by subscripting the graph.
      So `for nbr, foovalue in G[node].data('foo', default=1):` works.

      For directed graphs, `G.succ` is identical to `G.adj`.


   .. py:method:: pred()

      Graph adjacency object holding the predecessors of each node.

      This object is a read-only dict-like structure with node keys
      and neighbor-dict values.  The neighbor-dict is keyed by neighbor
      to the edgekey-dict.  So `G.adj[3][2][0]['color'] = 'blue'` sets
      the color of the edge `(3, 2, 0)` to `"blue"`.

      Iterating over G.adj behaves like a dict. Useful idioms include
      `for nbr, datadict in G.adj[n].items():`.


   .. py:method:: add_edge(u_for_edge, v_for_edge, key=None, **attr)

      Add an edge between u and v.

      The nodes u and v will be automatically added if they are
      not already in the graph.

      Edge attributes can be specified with keywords or by directly
      accessing the edge's attribute dictionary. See examples below.

      :param u_for_edge: Nodes can be, for example, strings or numbers.
                         Nodes must be hashable (and not None) Python objects.
      :type u_for_edge: nodes
      :param v_for_edge: Nodes can be, for example, strings or numbers.
                         Nodes must be hashable (and not None) Python objects.
      :type v_for_edge: nodes
      :param key: Used to distinguish multiedges between a pair of nodes.
      :type key: hashable identifier, optional (default=lowest unused integer)
      :param attr: Edge data (or labels or objects) can be assigned using
                   keyword arguments.
      :type attr: keyword arguments, optional

      :rtype: The edge key assigned to the edge.

      .. seealso::

         :obj:`add_edges_from`
             add a collection of edges

      .. rubric:: Notes

      To replace/update edge data, use the optional key argument
      to identify a unique edge.  Otherwise a new edge will be created.

      NetworkX algorithms designed for weighted graphs cannot use
      multigraphs directly because it is not clear how to handle
      multiedge weights.  Convert to Graph using edge attribute
      'weight' to enable weighted graph algorithms.

      Default keys are generated using the method `new_edge_key()`.
      This method can be overridden by subclassing the base class and
      providing a custom `new_edge_key()` method.

      .. rubric:: Examples

      The following all add the edge e=(1, 2) to graph G:

      >>> G = nx.MultiDiGraph()
      >>> e = (1, 2)
      >>> key = G.add_edge(1, 2)  # explicit two-node form
      >>> G.add_edge(*e)  # single edge as tuple of two nodes
      1
      >>> G.add_edges_from([(1, 2)])  # add edges from iterable container
      [2]

      Associate data to edges using keywords:

      >>> key = G.add_edge(1, 2, weight=3)
      >>> key = G.add_edge(1, 2, key=0, weight=4)  # update data for key=0
      >>> key = G.add_edge(1, 3, weight=7, capacity=15, length=342.7)

      For non-string attribute keys, use subscript notation.

      >>> ekey = G.add_edge(1, 2)
      >>> G[1][2][0].update({0: 5})
      >>> G.edges[1, 2, 0].update({0: 5})


   .. py:method:: remove_edge(u, v, key=None)

      Remove an edge between u and v.

      :param u: Remove an edge between nodes u and v.
      :type u: nodes
      :param v: Remove an edge between nodes u and v.
      :type v: nodes
      :param key: Used to distinguish multiple edges between a pair of nodes.
                  If None, remove a single edge between u and v. If there are
                  multiple edges, removes the last edge added in terms of
                  insertion order.
      :type key: hashable identifier, optional (default=None)

      :raises NetworkXError: If there is not an edge between u and v, or
          if there is no edge with the specified key.

      .. seealso::

         :obj:`remove_edges_from`
             remove a collection of edges

      .. rubric:: Examples

      >>> G = nx.MultiDiGraph()
      >>> nx.add_path(G, [0, 1, 2, 3])
      >>> G.remove_edge(0, 1)
      >>> e = (1, 2)
      >>> G.remove_edge(*e)  # unpacks e from an edge tuple

      For multiple edges

      >>> G = nx.MultiDiGraph()
      >>> G.add_edges_from([(1, 2), (1, 2), (1, 2)])  # key_list returned
      [0, 1, 2]

      When ``key=None`` (the default), edges are removed in the opposite
      order that they were added:

      >>> G.remove_edge(1, 2)
      >>> G.edges(keys=True)
      OutMultiEdgeView([(1, 2, 0), (1, 2, 1)])

      For edges with keys

      >>> G = nx.MultiDiGraph()
      >>> G.add_edge(1, 2, key="first")
      'first'
      >>> G.add_edge(1, 2, key="second")
      'second'
      >>> G.remove_edge(1, 2, key="first")
      >>> G.edges(keys=True)
      OutMultiEdgeView([(1, 2, 'second')])


   .. py:method:: edges()

      An OutMultiEdgeView of the Graph as G.edges or G.edges().

      edges(self, nbunch=None, data=False, keys=False, default=None)

      The OutMultiEdgeView provides set-like operations on the edge-tuples
      as well as edge attribute lookup. When called, it also provides
      an EdgeDataView object which allows control of access to edge
      attributes (but does not provide set-like operations).
      Hence, ``G.edges[u, v, k]['color']`` provides the value of the color
      attribute for the edge from ``u`` to ``v`` with key ``k`` while
      ``for (u, v, k, c) in G.edges(data='color', default='red', keys=True):``
      iterates through all the edges yielding the color attribute with
      default `'red'` if no color attribute exists.

      Edges are returned as tuples with optional data and keys
      in the order (node, neighbor, key, data). If ``keys=True`` is not
      provided, the tuples will just be (node, neighbor, data), but
      multiple tuples with the same node and neighbor will be
      generated when multiple edges between two nodes exist.

      :param nbunch: The view will only report edges from these nodes.
      :type nbunch: single node, container, or all nodes (default= all nodes)
      :param data: The edge attribute returned in 3-tuple (u, v, ddict[data]).
                   If True, return edge attribute dict in 3-tuple (u, v, ddict).
                   If False, return 2-tuple (u, v).
      :type data: string or bool, optional (default=False)
      :param keys: If True, return edge keys with each edge, creating (u, v, k,
                   d) tuples when data is also requested (the default) and (u,
                   v, k) tuples when data is not requested.
      :type keys: bool, optional (default=False)
      :param default: Value used for edges that don't have the requested attribute.
                      Only relevant if data is not True or False.
      :type default: value, optional (default=None)

      :returns: **edges** -- A view of edge attributes, usually it iterates over (u, v)
                (u, v, k) or (u, v, k, d) tuples of edges, but can also be
                used for attribute lookup as ``edges[u, v, k]['foo']``.
      :rtype: OutMultiEdgeView

      .. rubric:: Notes

      Nodes in nbunch that are not in the graph will be (quietly) ignored.
      For directed graphs this returns the out-edges.

      .. rubric:: Examples

      >>> G = nx.MultiDiGraph()
      >>> nx.add_path(G, [0, 1, 2])
      >>> key = G.add_edge(2, 3, weight=5)
      >>> key2 = G.add_edge(1, 2) # second edge between these nodes
      >>> [e for e in G.edges()]
      [(0, 1), (1, 2), (1, 2), (2, 3)]
      >>> list(G.edges(data=True))  # default data is {} (empty dict)
      [(0, 1, {}), (1, 2, {}), (1, 2, {}), (2, 3, {'weight': 5})]
      >>> list(G.edges(data="weight", default=1))
      [(0, 1, 1), (1, 2, 1), (1, 2, 1), (2, 3, 5)]
      >>> list(G.edges(keys=True))  # default keys are integers
      [(0, 1, 0), (1, 2, 0), (1, 2, 1), (2, 3, 0)]
      >>> list(G.edges(data=True, keys=True))
      [(0, 1, 0, {}), (1, 2, 0, {}), (1, 2, 1, {}), (2, 3, 0, {'weight': 5})]
      >>> list(G.edges(data="weight", default=1, keys=True))
      [(0, 1, 0, 1), (1, 2, 0, 1), (1, 2, 1, 1), (2, 3, 0, 5)]
      >>> list(G.edges([0, 2]))
      [(0, 1), (2, 3)]
      >>> list(G.edges(0))
      [(0, 1)]
      >>> list(G.edges(1))
      [(1, 2), (1, 2)]

      .. seealso:: :obj:`in_edges`, :obj:`out_edges`


   .. py:method:: in_edges()

      An InMultiEdgeView of the Graph as G.in_edges or G.in_edges().

      in_edges(self, nbunch=None, data=False, keys=False, default=None)

      :param nbunch: The view will only report edges incident to these nodes.
      :type nbunch: single node, container, or all nodes (default= all nodes)
      :param data: The edge attribute returned in 3-tuple (u, v, ddict[data]).
                   If True, return edge attribute dict in 3-tuple (u, v, ddict).
                   If False, return 2-tuple (u, v).
      :type data: string or bool, optional (default=False)
      :param keys: If True, return edge keys with each edge, creating 3-tuples
                   (u, v, k) or with data, 4-tuples (u, v, k, d).
      :type keys: bool, optional (default=False)
      :param default: Value used for edges that don't have the requested attribute.
                      Only relevant if data is not True or False.
      :type default: value, optional (default=None)

      :returns: **in_edges** -- A view of edge attributes, usually it iterates over (u, v)
                or (u, v, k) or (u, v, k, d) tuples of edges, but can also be
                used for attribute lookup as `edges[u, v, k]['foo']`.
      :rtype: InMultiEdgeView

      .. seealso:: :obj:`edges`


   .. py:method:: degree()

      A DegreeView for the Graph as G.degree or G.degree().

      The node degree is the number of edges adjacent to the node.
      The weighted node degree is the sum of the edge weights for
      edges incident to that node.

      This object provides an iterator for (node, degree) as well as
      lookup for the degree for a single node.

      :param nbunch: The view will only report edges incident to these nodes.
      :type nbunch: single node, container, or all nodes (default= all nodes)
      :param weight: The name of an edge attribute that holds the numerical value used
                     as a weight.  If None, then each edge has weight 1.
                     The degree is the sum of the edge weights adjacent to the node.
      :type weight: string or None, optional (default=None)

      :returns: If multiple nodes are requested (the default), returns a `DiMultiDegreeView`
                mapping nodes to their degree.
                If a single node is requested, returns the degree of the node as an integer.
      :rtype: DiMultiDegreeView or int

      .. seealso:: :obj:`out_degree`, :obj:`in_degree`

      .. rubric:: Examples

      >>> G = nx.MultiDiGraph()
      >>> nx.add_path(G, [0, 1, 2, 3])
      >>> G.degree(0)  # node 0 with degree 1
      1
      >>> list(G.degree([0, 1, 2]))
      [(0, 1), (1, 2), (2, 2)]
      >>> G.add_edge(0, 1) # parallel edge
      1
      >>> list(G.degree([0, 1, 2])) # parallel edges are counted
      [(0, 2), (1, 3), (2, 2)]


   .. py:method:: in_degree()

      A DegreeView for (node, in_degree) or in_degree for single node.

      The node in-degree is the number of edges pointing in to the node.
      The weighted node degree is the sum of the edge weights for
      edges incident to that node.

      This object provides an iterator for (node, degree) as well as
      lookup for the degree for a single node.

      :param nbunch: The view will only report edges incident to these nodes.
      :type nbunch: single node, container, or all nodes (default= all nodes)
      :param weight: The edge attribute that holds the numerical value used
                     as a weight.  If None, then each edge has weight 1.
                     The degree is the sum of the edge weights adjacent to the node.
      :type weight: string or None, optional (default=None)

      :returns: * *If a single node is requested*
                * **deg** (*int*) -- Degree of the node
                * *OR if multiple nodes are requested*
                * **nd_iter** (*iterator*) -- The iterator returns two-tuples of (node, in-degree).

      .. seealso:: :obj:`degree`, :obj:`out_degree`

      .. rubric:: Examples

      >>> G = nx.MultiDiGraph()
      >>> nx.add_path(G, [0, 1, 2, 3])
      >>> G.in_degree(0)  # node 0 with degree 0
      0
      >>> list(G.in_degree([0, 1, 2]))
      [(0, 0), (1, 1), (2, 1)]
      >>> G.add_edge(0, 1) # parallel edge
      1
      >>> list(G.in_degree([0, 1, 2])) # parallel edges counted
      [(0, 0), (1, 2), (2, 1)]


   .. py:method:: out_degree()

      Returns an iterator for (node, out-degree) or out-degree for single node.

      out_degree(self, nbunch=None, weight=None)

      The node out-degree is the number of edges pointing out of the node.
      This function returns the out-degree for a single node or an iterator
      for a bunch of nodes or if nothing is passed as argument.

      :param nbunch: The view will only report edges incident to these nodes.
      :type nbunch: single node, container, or all nodes (default= all nodes)
      :param weight: The edge attribute that holds the numerical value used
                     as a weight.  If None, then each edge has weight 1.
                     The degree is the sum of the edge weights.
      :type weight: string or None, optional (default=None)

      :returns: * *If a single node is requested*
                * **deg** (*int*) -- Degree of the node
                * *OR if multiple nodes are requested*
                * **nd_iter** (*iterator*) -- The iterator returns two-tuples of (node, out-degree).

      .. seealso:: :obj:`degree`, :obj:`in_degree`

      .. rubric:: Examples

      >>> G = nx.MultiDiGraph()
      >>> nx.add_path(G, [0, 1, 2, 3])
      >>> G.out_degree(0)  # node 0 with degree 1
      1
      >>> list(G.out_degree([0, 1, 2]))
      [(0, 1), (1, 1), (2, 1)]
      >>> G.add_edge(0, 1) # parallel edge
      1
      >>> list(G.out_degree([0, 1, 2])) # counts parallel edges
      [(0, 2), (1, 1), (2, 1)]


   .. py:method:: is_multigraph()

      Returns True if graph is a multigraph, False otherwise.


   .. py:method:: is_directed()

      Returns True if graph is directed, False otherwise.


   .. py:method:: to_undirected(reciprocal=False, as_view=False)

      Returns an undirected representation of the digraph.

      :param reciprocal: If True only keep edges that appear in both directions
                         in the original digraph.
      :type reciprocal: bool (optional)
      :param as_view: If True return an undirected view of the original directed graph.
      :type as_view: bool (optional, default=False)

      :returns: **G** -- An undirected graph with the same name and nodes and
                with edge (u, v, data) if either (u, v, data) or (v, u, data)
                is in the digraph.  If both edges exist in digraph and
                their edge data is different, only one edge is created
                with an arbitrary choice of which edge data to use.
                You must check and correct for this manually if desired.
      :rtype: MultiGraph

      .. seealso:: :obj:`MultiGraph`, :obj:`copy`, :obj:`add_edge`, :obj:`add_edges_from`

      .. rubric:: Notes

      This returns a "deepcopy" of the edge, node, and
      graph attributes which attempts to completely copy
      all of the data and references.

      This is in contrast to the similar D=MultiDiGraph(G) which
      returns a shallow copy of the data.

      See the Python copy module for more information on shallow
      and deep copies, https://docs.python.org/3/library/copy.html.

      Warning: If you have subclassed MultiDiGraph to use dict-like
      objects in the data structure, those changes do not transfer
      to the MultiGraph created by this method.

      .. rubric:: Examples

      >>> G = nx.path_graph(2)  # or MultiGraph, etc
      >>> H = G.to_directed()
      >>> list(H.edges)
      [(0, 1), (1, 0)]
      >>> G2 = H.to_undirected()
      >>> list(G2.edges)
      [(0, 1)]


   .. py:method:: reverse(copy=True)

      Returns the reverse of the graph.

      The reverse is a graph with the same nodes and edges
      but with the directions of the edges reversed.

      :param copy: If True, return a new DiGraph holding the reversed edges.
                   If False, the reverse graph is created using a view of
                   the original graph.
      :type copy: bool optional (default=True)


   .. py:method:: to_directed_class()

      Returns the class to use for empty directed copies.

      If you subclass the base classes, use this to designate
      what directed class to use for `to_directed()` copies.


   .. py:method:: to_undirected_class()

      Returns the class to use for empty undirected copies.

      If you subclass the base classes, use this to designate
      what directed class to use for `to_directed()` copies.


   .. py:method:: new_edge_key(u, v)

      Returns an unused key for edges between nodes `u` and `v`.

      The nodes `u` and `v` do not need to be already in the graph.

      .. rubric:: Notes

      In the standard MultiGraph class the new key is the number of existing
      edges between `u` and `v` (increased if necessary to ensure unused).
      The first edge will have key 0, then 1, etc. If an edge is removed
      further new_edge_keys may not be in this order.

      :param u:
      :type u: nodes
      :param v:
      :type v: nodes

      :returns: **key**
      :rtype: int


   .. py:method:: add_edges_from(ebunch_to_add, **attr)

      Add all the edges in ebunch_to_add.

      :param ebunch_to_add: Each edge given in the container will be added to the
                            graph. The edges can be:

                                - 2-tuples (u, v) or
                                - 3-tuples (u, v, d) for an edge data dict d, or
                                - 3-tuples (u, v, k) for not iterable key k, or
                                - 4-tuples (u, v, k, d) for an edge with data and key k
      :type ebunch_to_add: container of edges
      :param attr: Edge data (or labels or objects) can be assigned using
                   keyword arguments.
      :type attr: keyword arguments, optional

      :rtype: A list of edge keys assigned to the edges in `ebunch`.

      .. seealso::

         :obj:`add_edge`
             add a single edge

         :obj:`add_weighted_edges_from`
             convenient way to add weighted edges

      .. rubric:: Notes

      Adding the same edge twice has no effect but any edge data
      will be updated when each duplicate edge is added.

      Edge attributes specified in an ebunch take precedence over
      attributes specified via keyword arguments.

      Default keys are generated using the method ``new_edge_key()``.
      This method can be overridden by subclassing the base class and
      providing a custom ``new_edge_key()`` method.

      .. rubric:: Examples

      >>> G = nx.Graph()  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.add_edges_from([(0, 1), (1, 2)])  # using a list of edge tuples
      >>> e = zip(range(0, 3), range(1, 4))
      >>> G.add_edges_from(e)  # Add the path graph 0-1-2-3

      Associate data to edges

      >>> G.add_edges_from([(1, 2), (2, 3)], weight=3)
      >>> G.add_edges_from([(3, 4), (1, 4)], label="WN2898")


   .. py:method:: remove_edges_from(ebunch)

      Remove all edges specified in ebunch.

      :param ebunch: Each edge given in the list or container will be removed
                     from the graph. The edges can be:

                         - 2-tuples (u, v) A single edge between u and v is removed.
                         - 3-tuples (u, v, key) The edge identified by key is removed.
                         - 4-tuples (u, v, key, data) where data is ignored.
      :type ebunch: list or container of edge tuples

      .. seealso::

         :obj:`remove_edge`
             remove a single edge

      .. rubric:: Notes

      Will fail silently if an edge in ebunch is not in the graph.

      .. rubric:: Examples

      >>> G = nx.path_graph(4)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> ebunch = [(1, 2), (2, 3)]
      >>> G.remove_edges_from(ebunch)

      Removing multiple copies of edges

      >>> G = nx.MultiGraph()
      >>> keys = G.add_edges_from([(1, 2), (1, 2), (1, 2)])
      >>> G.remove_edges_from([(1, 2), (2, 1)])  # edges aren't directed
      >>> list(G.edges())
      [(1, 2)]
      >>> G.remove_edges_from([(1, 2), (1, 2)])  # silently ignore extra copy
      >>> list(G.edges)  # now empty graph
      []

      When the edge is a 2-tuple ``(u, v)`` but there are multiple edges between
      u and v in the graph, the most recent edge (in terms of insertion
      order) is removed.

      >>> G = nx.MultiGraph()
      >>> for key in ("x", "y", "a"):
      ...     k = G.add_edge(0, 1, key=key)
      >>> G.edges(keys=True)
      MultiEdgeView([(0, 1, 'x'), (0, 1, 'y'), (0, 1, 'a')])
      >>> G.remove_edges_from([(0, 1)])
      >>> G.edges(keys=True)
      MultiEdgeView([(0, 1, 'x'), (0, 1, 'y')])


   .. py:method:: has_edge(u, v, key=None)

      Returns True if the graph has an edge between nodes u and v.

      This is the same as `v in G[u] or key in G[u][v]`
      without KeyError exceptions.

      :param u: Nodes can be, for example, strings or numbers.
      :type u: nodes
      :param v: Nodes can be, for example, strings or numbers.
      :type v: nodes
      :param key: If specified return True only if the edge with
                  key is found.
      :type key: hashable identifier, optional (default=None)

      :returns: **edge_ind** -- True if edge is in the graph, False otherwise.
      :rtype: bool

      .. rubric:: Examples

      Can be called either using two nodes u, v, an edge tuple (u, v),
      or an edge tuple (u, v, key).

      >>> G = nx.MultiGraph()  # or MultiDiGraph
      >>> nx.add_path(G, [0, 1, 2, 3])
      >>> G.has_edge(0, 1)  # using two nodes
      True
      >>> e = (0, 1)
      >>> G.has_edge(*e)  #  e is a 2-tuple (u, v)
      True
      >>> G.add_edge(0, 1, key="a")
      'a'
      >>> G.has_edge(0, 1, key="a")  # specify key
      True
      >>> G.has_edge(1, 0, key="a")  # edges aren't directed
      True
      >>> e = (0, 1, "a")
      >>> G.has_edge(*e)  # e is a 3-tuple (u, v, 'a')
      True

      The following syntax are equivalent:

      >>> G.has_edge(0, 1)
      True
      >>> 1 in G[0]  # though this gives :exc:`KeyError` if 0 not in G
      True
      >>> 0 in G[1]  # other order; also gives :exc:`KeyError` if 0 not in G
      True


   .. py:method:: get_edge_data(u, v, key=None, default=None)

      Returns the attribute dictionary associated with edge (u, v,
      key).

      If a key is not provided, returns a dictionary mapping edge keys
      to attribute dictionaries for each edge between u and v.

      This is identical to `G[u][v][key]` except the default is returned
      instead of an exception is the edge doesn't exist.

      :param u:
      :type u: nodes
      :param v:
      :type v: nodes
      :param default: Value to return if the specific edge (u, v, key) is not
                      found, OR if there are no edges between u and v and no key
                      is specified.
      :type default: any Python object (default=None)
      :param key: Return data only for the edge with specified key, as an
                  attribute dictionary (rather than a dictionary mapping keys
                  to attribute dictionaries).
      :type key: hashable identifier, optional (default=None)

      :returns: **edge_dict** -- The edge attribute dictionary, OR a dictionary mapping edge
                keys to attribute dictionaries for each of those edges if no
                specific key is provided (even if there's only one edge
                between u and v).
      :rtype: dictionary

      .. rubric:: Examples

      >>> G = nx.MultiGraph()  # or MultiDiGraph
      >>> key = G.add_edge(0, 1, key="a", weight=7)
      >>> G[0][1]["a"]  # key='a'
      {'weight': 7}
      >>> G.edges[0, 1, "a"]  # key='a'
      {'weight': 7}

      Warning: we protect the graph data structure by making
      `G.edges` and `G[1][2]` read-only dict-like structures.
      However, you can assign values to attributes in e.g.
      `G.edges[1, 2, 'a']` or `G[1][2]['a']` using an additional
      bracket as shown next. You need to specify all edge info
      to assign to the edge data associated with an edge.

      >>> G[0][1]["a"]["weight"] = 10
      >>> G.edges[0, 1, "a"]["weight"] = 10
      >>> G[0][1]["a"]["weight"]
      10
      >>> G.edges[1, 0, "a"]["weight"]
      10

      >>> G = nx.MultiGraph()  # or MultiDiGraph
      >>> nx.add_path(G, [0, 1, 2, 3])
      >>> G.edges[0, 1, 0]["weight"] = 5
      >>> G.get_edge_data(0, 1)
      {0: {'weight': 5}}
      >>> e = (0, 1)
      >>> G.get_edge_data(*e)  # tuple form
      {0: {'weight': 5}}
      >>> G.get_edge_data(3, 0)  # edge not in graph, returns None
      >>> G.get_edge_data(3, 0, default=0)  # edge not in graph, return default
      0
      >>> G.get_edge_data(1, 0, 0)  # specific key gives back
      {'weight': 5}


   .. py:method:: copy(as_view=False)

      Returns a copy of the graph.

      The copy method by default returns an independent shallow copy
      of the graph and attributes. That is, if an attribute is a
      container, that container is shared by the original an the copy.
      Use Python's `copy.deepcopy` for new containers.

      If `as_view` is True then a view is returned instead of a copy.

      .. rubric:: Notes

      All copies reproduce the graph structure, but data attributes
      may be handled in different ways. There are four types of copies
      of a graph that people might want.

      Deepcopy -- A "deepcopy" copies the graph structure as well as
      all data attributes and any objects they might contain.
      The entire graph object is new so that changes in the copy
      do not affect the original object. (see Python's copy.deepcopy)

      Data Reference (Shallow) -- For a shallow copy the graph structure
      is copied but the edge, node and graph attribute dicts are
      references to those in the original graph. This saves
      time and memory but could cause confusion if you change an attribute
      in one graph and it changes the attribute in the other.
      NetworkX does not provide this level of shallow copy.

      Independent Shallow -- This copy creates new independent attribute
      dicts and then does a shallow copy of the attributes. That is, any
      attributes that are containers are shared between the new graph
      and the original. This is exactly what `dict.copy()` provides.
      You can obtain this style copy using:

          >>> G = nx.path_graph(5)
          >>> H = G.copy()
          >>> H = G.copy(as_view=False)
          >>> H = nx.Graph(G)
          >>> H = G.__class__(G)

      Fresh Data -- For fresh data, the graph structure is copied while
      new empty data attribute dicts are created. The resulting graph
      is independent of the original and it has no edge, node or graph
      attributes. Fresh copies are not enabled. Instead use:

          >>> H = G.__class__()
          >>> H.add_nodes_from(G)
          >>> H.add_edges_from(G.edges)

      View -- Inspired by dict-views, graph-views act like read-only
      versions of the original graph, providing a copy of the original
      structure without requiring any memory for copying the information.

      See the Python copy module for more information on shallow
      and deep copies, https://docs.python.org/3/library/copy.html.

      :param as_view: If True, the returned graph-view provides a read-only view
                      of the original graph without actually copying any data.
      :type as_view: bool, optional (default=False)

      :returns: **G** -- A copy of the graph.
      :rtype: Graph

      .. seealso::

         :obj:`to_directed`
             return a directed copy of the graph.

      .. rubric:: Examples

      >>> G = nx.path_graph(4)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> H = G.copy()


   .. py:method:: to_directed(as_view=False)

      Returns a directed representation of the graph.

      :returns: **G** -- A directed graph with the same name, same nodes, and with
                each edge (u, v, k, data) replaced by two directed edges
                (u, v, k, data) and (v, u, k, data).
      :rtype: MultiDiGraph

      .. rubric:: Notes

      This returns a "deepcopy" of the edge, node, and
      graph attributes which attempts to completely copy
      all of the data and references.

      This is in contrast to the similar D=MultiDiGraph(G) which
      returns a shallow copy of the data.

      See the Python copy module for more information on shallow
      and deep copies, https://docs.python.org/3/library/copy.html.

      Warning: If you have subclassed MultiGraph to use dict-like objects
      in the data structure, those changes do not transfer to the
      MultiDiGraph created by this method.

      .. rubric:: Examples

      >>> G = nx.MultiGraph()
      >>> G.add_edge(0, 1)
      0
      >>> G.add_edge(0, 1)
      1
      >>> H = G.to_directed()
      >>> list(H.edges)
      [(0, 1, 0), (0, 1, 1), (1, 0, 0), (1, 0, 1)]

      If already directed, return a (deep) copy

      >>> G = nx.MultiDiGraph()
      >>> G.add_edge(0, 1)
      0
      >>> H = G.to_directed()
      >>> list(H.edges)
      [(0, 1, 0)]


   .. py:method:: number_of_edges(u=None, v=None)

      Returns the number of edges between two nodes.

      :param u: If u and v are specified, return the number of edges between
                u and v. Otherwise return the total number of all edges.
      :type u: nodes, optional (Gefault=all edges)
      :param v: If u and v are specified, return the number of edges between
                u and v. Otherwise return the total number of all edges.
      :type v: nodes, optional (Gefault=all edges)

      :returns: **nedges** -- The number of edges in the graph.  If nodes `u` and `v` are
                specified return the number of edges between those nodes. If
                the graph is directed, this only returns the number of edges
                from `u` to `v`.
      :rtype: int

      .. seealso:: :obj:`size`

      .. rubric:: Examples

      For undirected multigraphs, this method counts the total number
      of edges in the graph::

          >>> G = nx.MultiGraph()
          >>> G.add_edges_from([(0, 1), (0, 1), (1, 2)])
          [0, 1, 0]
          >>> G.number_of_edges()
          3

      If you specify two nodes, this counts the total number of edges
      joining the two nodes::

          >>> G.number_of_edges(0, 1)
          2

      For directed multigraphs, this method can count the total number
      of directed edges from `u` to `v`::

          >>> G = nx.MultiDiGraph()
          >>> G.add_edges_from([(0, 1), (0, 1), (1, 0)])
          [0, 1, 0]
          >>> G.number_of_edges(0, 1)
          2
          >>> G.number_of_edges(1, 0)
          1


   .. py:method:: add_node(node_for_adding, **attr)

      Add a single node `node_for_adding` and update node attributes.

      :param node_for_adding: A node can be any hashable Python object except None.
      :type node_for_adding: node
      :param attr: Set or change node attributes using key=value.
      :type attr: keyword arguments, optional

      .. seealso:: :obj:`add_nodes_from`

      .. rubric:: Examples

      >>> G = nx.Graph()  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.add_node(1)
      >>> G.add_node("Hello")
      >>> K3 = nx.Graph([(0, 1), (1, 2), (2, 0)])
      >>> G.add_node(K3)
      >>> G.number_of_nodes()
      3

      Use keywords set/change node attributes:

      >>> G.add_node(1, size=10)
      >>> G.add_node(3, weight=0.4, UTM=("13S", 382871, 3972649))

      .. rubric:: Notes

      A hashable object is one that can be used as a key in a Python
      dictionary. This includes strings, numbers, tuples of strings
      and numbers, etc.

      On many platforms hashable items also include mutables such as
      NetworkX Graphs, though one should be careful that the hash
      doesn't change on mutables.


   .. py:method:: add_nodes_from(nodes_for_adding, **attr)

      Add multiple nodes.

      :param nodes_for_adding: A container of nodes (list, dict, set, etc.).
                               OR
                               A container of (node, attribute dict) tuples.
                               Node attributes are updated using the attribute dict.
      :type nodes_for_adding: iterable container
      :param attr: Update attributes for all nodes in nodes.
                   Node attributes specified in nodes as a tuple take
                   precedence over attributes specified via keyword arguments.
      :type attr: keyword arguments, optional (default= no attributes)

      .. seealso:: :obj:`add_node`

      .. rubric:: Examples

      >>> G = nx.Graph()  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.add_nodes_from("Hello")
      >>> K3 = nx.Graph([(0, 1), (1, 2), (2, 0)])
      >>> G.add_nodes_from(K3)
      >>> sorted(G.nodes(), key=str)
      [0, 1, 2, 'H', 'e', 'l', 'o']

      Use keywords to update specific node attributes for every node.

      >>> G.add_nodes_from([1, 2], size=10)
      >>> G.add_nodes_from([3, 4], weight=0.4)

      Use (node, attrdict) tuples to update attributes for specific nodes.

      >>> G.add_nodes_from([(1, dict(size=11)), (2, {"color": "blue"})])
      >>> G.nodes[1]["size"]
      11
      >>> H = nx.Graph()
      >>> H.add_nodes_from(G.nodes(data=True))
      >>> H.nodes[1]["size"]
      11


   .. py:method:: remove_node(n)

      Remove node n.

      Removes the node n and all adjacent edges.
      Attempting to remove a non-existent node will raise an exception.

      :param n: A node in the graph
      :type n: node

      :raises NetworkXError: If n is not in the graph.

      .. seealso:: :obj:`remove_nodes_from`

      .. rubric:: Examples

      >>> G = nx.path_graph(3)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> list(G.edges)
      [(0, 1), (1, 2)]
      >>> G.remove_node(1)
      >>> list(G.edges)
      []


   .. py:method:: remove_nodes_from(nodes)

      Remove multiple nodes.

      :param nodes: A container of nodes (list, dict, set, etc.).  If a node
                    in the container is not in the graph it is silently
                    ignored.
      :type nodes: iterable container

      .. seealso:: :obj:`remove_node`

      .. rubric:: Examples

      >>> G = nx.path_graph(3)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> e = list(G.nodes)
      >>> e
      [0, 1, 2]
      >>> G.remove_nodes_from(e)
      >>> list(G.nodes)
      []


   .. py:method:: nodes()

      A NodeView of the Graph as G.nodes or G.nodes().

      Can be used as `G.nodes` for data lookup and for set-like operations.
      Can also be used as `G.nodes(data='color', default=None)` to return a
      NodeDataView which reports specific node data but no set operations.
      It presents a dict-like interface as well with `G.nodes.items()`
      iterating over `(node, nodedata)` 2-tuples and `G.nodes[3]['foo']`
      providing the value of the `foo` attribute for node `3`. In addition,
      a view `G.nodes.data('foo')` provides a dict-like interface to the
      `foo` attribute of each node. `G.nodes.data('foo', default=1)`
      provides a default for nodes that do not have attribute `foo`.

      :param data: The node attribute returned in 2-tuple (n, ddict[data]).
                   If True, return entire node attribute dict as (n, ddict).
                   If False, return just the nodes n.
      :type data: string or bool, optional (default=False)
      :param default: Value used for nodes that don't have the requested attribute.
                      Only relevant if data is not True or False.
      :type default: value, optional (default=None)

      :returns: Allows set-like operations over the nodes as well as node
                attribute dict lookup and calling to get a NodeDataView.
                A NodeDataView iterates over `(n, data)` and has no set operations.
                A NodeView iterates over `n` and includes set operations.

                When called, if data is False, an iterator over nodes.
                Otherwise an iterator of 2-tuples (node, attribute value)
                where the attribute is specified in `data`.
                If data is True then the attribute becomes the
                entire data dictionary.
      :rtype: NodeView

      .. rubric:: Notes

      If your node data is not needed, it is simpler and equivalent
      to use the expression ``for n in G``, or ``list(G)``.

      .. rubric:: Examples

      There are two simple ways of getting a list of all nodes in the graph:

      >>> G = nx.path_graph(3)
      >>> list(G.nodes)
      [0, 1, 2]
      >>> list(G)
      [0, 1, 2]

      To get the node data along with the nodes:

      >>> G.add_node(1, time="5pm")
      >>> G.nodes[0]["foo"] = "bar"
      >>> list(G.nodes(data=True))
      [(0, {'foo': 'bar'}), (1, {'time': '5pm'}), (2, {})]
      >>> list(G.nodes.data())
      [(0, {'foo': 'bar'}), (1, {'time': '5pm'}), (2, {})]

      >>> list(G.nodes(data="foo"))
      [(0, 'bar'), (1, None), (2, None)]
      >>> list(G.nodes.data("foo"))
      [(0, 'bar'), (1, None), (2, None)]

      >>> list(G.nodes(data="time"))
      [(0, None), (1, '5pm'), (2, None)]
      >>> list(G.nodes.data("time"))
      [(0, None), (1, '5pm'), (2, None)]

      >>> list(G.nodes(data="time", default="Not Available"))
      [(0, 'Not Available'), (1, '5pm'), (2, 'Not Available')]
      >>> list(G.nodes.data("time", default="Not Available"))
      [(0, 'Not Available'), (1, '5pm'), (2, 'Not Available')]

      If some of your nodes have an attribute and the rest are assumed
      to have a default attribute value you can create a dictionary
      from node/attribute pairs using the `default` keyword argument
      to guarantee the value is never None::

          >>> G = nx.Graph()
          >>> G.add_node(0)
          >>> G.add_node(1, weight=2)
          >>> G.add_node(2, weight=3)
          >>> dict(G.nodes(data="weight", default=1))
          {0: 1, 1: 2, 2: 3}


   .. py:method:: number_of_nodes()

      Returns the number of nodes in the graph.

      :returns: **nnodes** -- The number of nodes in the graph.
      :rtype: int

      .. seealso::

         :obj:`order`
             identical method

         :obj:`__len__`
             identical method

      .. rubric:: Examples

      >>> G = nx.path_graph(3)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.number_of_nodes()
      3


   .. py:method:: order()

      Returns the number of nodes in the graph.

      :returns: **nnodes** -- The number of nodes in the graph.
      :rtype: int

      .. seealso::

         :obj:`number_of_nodes`
             identical method

         :obj:`__len__`
             identical method

      .. rubric:: Examples

      >>> G = nx.path_graph(3)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.order()
      3


   .. py:method:: has_node(n)

      Returns True if the graph contains the node n.

      Identical to `n in G`

      :param n:
      :type n: node

      .. rubric:: Examples

      >>> G = nx.path_graph(3)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.has_node(0)
      True

      It is more readable and simpler to use

      >>> 0 in G
      True


   .. py:method:: add_weighted_edges_from(ebunch_to_add, weight='weight', **attr)

      Add weighted edges in `ebunch_to_add` with specified weight attr

      :param ebunch_to_add: Each edge given in the list or container will be added
                            to the graph. The edges must be given as 3-tuples (u, v, w)
                            where w is a number.
      :type ebunch_to_add: container of edges
      :param weight: The attribute name for the edge weights to be added.
      :type weight: string, optional (default= 'weight')
      :param attr: Edge attributes to add/update for all edges.
      :type attr: keyword arguments, optional (default= no attributes)

      .. seealso::

         :obj:`add_edge`
             add a single edge

         :obj:`add_edges_from`
             add multiple edges

      .. rubric:: Notes

      Adding the same edge twice for Graph/DiGraph simply updates
      the edge data. For MultiGraph/MultiDiGraph, duplicate edges
      are stored.

      .. rubric:: Examples

      >>> G = nx.Graph()  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.add_weighted_edges_from([(0, 1, 3.0), (1, 2, 7.5)])


   .. py:method:: update(edges=None, nodes=None)

      Update the graph using nodes/edges/graphs as input.

      Like dict.update, this method takes a graph as input, adding the
      graph's nodes and edges to this graph. It can also take two inputs:
      edges and nodes. Finally it can take either edges or nodes.
      To specify only nodes the keyword `nodes` must be used.

      The collections of edges and nodes are treated similarly to
      the add_edges_from/add_nodes_from methods. When iterated, they
      should yield 2-tuples (u, v) or 3-tuples (u, v, datadict).

      :param edges: The first parameter can be a graph or some edges. If it has
                    attributes `nodes` and `edges`, then it is taken to be a
                    Graph-like object and those attributes are used as collections
                    of nodes and edges to be added to the graph.
                    If the first parameter does not have those attributes, it is
                    treated as a collection of edges and added to the graph.
                    If the first argument is None, no edges are added.
      :type edges: Graph object, collection of edges, or None
      :param nodes: The second parameter is treated as a collection of nodes
                    to be added to the graph unless it is None.
                    If `edges is None` and `nodes is None` an exception is raised.
                    If the first parameter is a Graph, then `nodes` is ignored.
      :type nodes: collection of nodes, or None

      .. rubric:: Examples

      >>> G = nx.path_graph(5)
      >>> G.update(nx.complete_graph(range(4, 10)))
      >>> from itertools import combinations
      >>> edges = (
      ...     (u, v, {"power": u * v})
      ...     for u, v in combinations(range(10, 20), 2)
      ...     if u * v < 225
      ... )
      >>> nodes = [1000]  # for singleton, use a container
      >>> G.update(edges, nodes)

      .. rubric:: Notes

      It you want to update the graph using an adjacency structure
      it is straightforward to obtain the edges/nodes from adjacency.
      The following examples provide common cases, your adjacency may
      be slightly different and require tweaks of these examples::

      >>> # dict-of-set/list/tuple
      >>> adj = {1: {2, 3}, 2: {1, 3}, 3: {1, 2}}
      >>> e = [(u, v) for u, nbrs in adj.items() for v in nbrs]
      >>> G.update(edges=e, nodes=adj)

      >>> DG = nx.DiGraph()
      >>> # dict-of-dict-of-attribute
      >>> adj = {1: {2: 1.3, 3: 0.7}, 2: {1: 1.4}, 3: {1: 0.7}}
      >>> e = [
      ...     (u, v, {"weight": d})
      ...     for u, nbrs in adj.items()
      ...     for v, d in nbrs.items()
      ... ]
      >>> DG.update(edges=e, nodes=adj)

      >>> # dict-of-dict-of-dict
      >>> adj = {1: {2: {"weight": 1.3}, 3: {"color": 0.7, "weight": 1.2}}}
      >>> e = [
      ...     (u, v, {"weight": d})
      ...     for u, nbrs in adj.items()
      ...     for v, d in nbrs.items()
      ... ]
      >>> DG.update(edges=e, nodes=adj)

      >>> # predecessor adjacency (dict-of-set)
      >>> pred = {1: {2, 3}, 2: {3}, 3: {3}}
      >>> e = [(v, u) for u, nbrs in pred.items() for v in nbrs]

      >>> # MultiGraph dict-of-dict-of-dict-of-attribute
      >>> MDG = nx.MultiDiGraph()
      >>> adj = {
      ...     1: {2: {0: {"weight": 1.3}, 1: {"weight": 1.2}}},
      ...     3: {2: {0: {"weight": 0.7}}},
      ... }
      >>> e = [
      ...     (u, v, ekey, d)
      ...     for u, nbrs in adj.items()
      ...     for v, keydict in nbrs.items()
      ...     for ekey, d in keydict.items()
      ... ]
      >>> MDG.update(edges=e)

      .. seealso::

         :obj:`add_edges_from`
             add multiple edges to a graph

         :obj:`add_nodes_from`
             add multiple nodes to a graph


   .. py:method:: neighbors(n)

      Returns an iterator over all neighbors of node n.

      This is identical to `iter(G[n])`

      :param n: A node in the graph
      :type n: node

      :returns: **neighbors** -- An iterator over all neighbors of node n
      :rtype: iterator

      :raises NetworkXError: If the node n is not in the graph.

      .. rubric:: Examples

      >>> G = nx.path_graph(4)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> [n for n in G.neighbors(0)]
      [1]

      .. rubric:: Notes

      Alternate ways to access the neighbors are ``G.adj[n]`` or ``G[n]``:

      >>> G = nx.Graph()  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.add_edge("a", "b", weight=7)
      >>> G["a"]
      AtlasView({'b': {'weight': 7}})
      >>> G = nx.path_graph(4)
      >>> [n for n in G[0]]
      [1]


   .. py:method:: adjacency()

      Returns an iterator over (node, adjacency dict) tuples for all nodes.

      For directed graphs, only outgoing neighbors/adjacencies are included.

      :returns: **adj_iter** -- An iterator over (node, adjacency dictionary) for all nodes in
                the graph.
      :rtype: iterator

      .. rubric:: Examples

      >>> G = nx.path_graph(4)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> [(n, nbrdict) for n, nbrdict in G.adjacency()]
      [(0, {1: {}}), (1, {0: {}, 2: {}}), (2, {1: {}, 3: {}}), (3, {2: {}})]


   .. py:method:: clear()

      Remove all nodes and edges from the graph.

      This also removes the name, and all graph, node, and edge attributes.

      .. rubric:: Examples

      >>> G = nx.path_graph(4)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.clear()
      >>> list(G.nodes)
      []
      >>> list(G.edges)
      []


   .. py:method:: clear_edges()

      Remove all edges from the graph without altering nodes.

      .. rubric:: Examples

      >>> G = nx.path_graph(4)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.clear_edges()
      >>> list(G.nodes)
      [0, 1, 2, 3]
      >>> list(G.edges)
      []


   .. py:method:: subgraph(nodes)

      Returns a SubGraph view of the subgraph induced on `nodes`.

      The induced subgraph of the graph contains the nodes in `nodes`
      and the edges between those nodes.

      :param nodes: A container of nodes which will be iterated through once.
      :type nodes: list, iterable

      :returns: **G** -- A subgraph view of the graph. The graph structure cannot be
                changed but node/edge attributes can and are shared with the
                original graph.
      :rtype: SubGraph View

      .. rubric:: Notes

      The graph, edge and node attributes are shared with the original graph.
      Changes to the graph structure is ruled out by the view, but changes
      to attributes are reflected in the original graph.

      To create a subgraph with its own copy of the edge/node attributes use:
      G.subgraph(nodes).copy()

      For an inplace reduction of a graph to a subgraph you can remove nodes:
      G.remove_nodes_from([n for n in G if n not in set(nodes)])

      Subgraph views are sometimes NOT what you want. In most cases where
      you want to do more than simply look at the induced edges, it makes
      more sense to just create the subgraph as its own graph with code like:

      ::

          # Create a subgraph SG based on a (possibly multigraph) G
          SG = G.__class__()
          SG.add_nodes_from((n, G.nodes[n]) for n in largest_wcc)
          if SG.is_multigraph():
              SG.add_edges_from((n, nbr, key, d)
                  for n, nbrs in G.adj.items() if n in largest_wcc
                  for nbr, keydict in nbrs.items() if nbr in largest_wcc
                  for key, d in keydict.items())
          else:
              SG.add_edges_from((n, nbr, d)
                  for n, nbrs in G.adj.items() if n in largest_wcc
                  for nbr, d in nbrs.items() if nbr in largest_wcc)
          SG.graph.update(G.graph)

      .. rubric:: Examples

      >>> G = nx.path_graph(4)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> H = G.subgraph([0, 1, 2])
      >>> list(H.edges)
      [(0, 1), (1, 2)]


   .. py:method:: edge_subgraph(edges)

      Returns the subgraph induced by the specified edges.

      The induced subgraph contains each edge in `edges` and each
      node incident to any one of those edges.

      :param edges: An iterable of edges in this graph.
      :type edges: iterable

      :returns: **G** -- An edge-induced subgraph of this graph with the same edge
                attributes.
      :rtype: Graph

      .. rubric:: Notes

      The graph, edge, and node attributes in the returned subgraph
      view are references to the corresponding attributes in the original
      graph. The view is read-only.

      To create a full graph version of the subgraph with its own copy
      of the edge or node attributes, use::

          G.edge_subgraph(edges).copy()

      .. rubric:: Examples

      >>> G = nx.path_graph(5)
      >>> H = G.edge_subgraph([(0, 1), (3, 4)])
      >>> list(H.nodes)
      [0, 1, 3, 4]
      >>> list(H.edges)
      [(0, 1), (3, 4)]


   .. py:method:: size(weight=None)

      Returns the number of edges or total of all edge weights.

      :param weight: The edge attribute that holds the numerical value used
                     as a weight. If None, then each edge has weight 1.
      :type weight: string or None, optional (default=None)

      :returns: **size** -- The number of edges or
                (if weight keyword is provided) the total weight sum.

                If weight is None, returns an int. Otherwise a float
                (or more general numeric if the weights are more general).
      :rtype: numeric

      .. seealso:: :obj:`number_of_edges`

      .. rubric:: Examples

      >>> G = nx.path_graph(4)  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.size()
      3

      >>> G = nx.Graph()  # or DiGraph, MultiGraph, MultiDiGraph, etc
      >>> G.add_edge("a", "b", weight=2)
      >>> G.add_edge("b", "c", weight=4)
      >>> G.size()
      2
      >>> G.size(weight="weight")
      6.0


   .. py:method:: nbunch_iter(nbunch=None)

      Returns an iterator over nodes contained in nbunch that are
      also in the graph.

      The nodes in nbunch are checked for membership in the graph
      and if not are silently ignored.

      :param nbunch: The view will only report edges incident to these nodes.
      :type nbunch: single node, container, or all nodes (default= all nodes)

      :returns: **niter** -- An iterator over nodes in nbunch that are also in the graph.
                If nbunch is None, iterate over all nodes in the graph.
      :rtype: iterator

      :raises NetworkXError: If nbunch is not a node or sequence of nodes.
          If a node in nbunch is not hashable.

      .. seealso:: :obj:`Graph.__iter__`

      .. rubric:: Notes

      When nbunch is an iterator, the returned iterator yields values
      directly from nbunch, becoming exhausted when nbunch is exhausted.

      To test whether nbunch is a single node, one can use
      "if nbunch in self:", even after processing with this routine.

      If nbunch is not a node or a (possibly empty) sequence/iterator
      or None, a :exc:`NetworkXError` is raised.  Also, if any object in
      nbunch is not hashable, a :exc:`NetworkXError` is raised.


   .. py:method:: has_successor(u, v)

      Returns True if node u has successor v.

      This is true if graph has the edge u->v.


   .. py:method:: has_predecessor(u, v)

      Returns True if node u has predecessor v.

      This is true if graph has the edge u<-v.


   .. py:method:: successors(n)

      Returns an iterator over successor nodes of n.

      A successor of n is a node m such that there exists a directed
      edge from n to m.

      :param n: A node in the graph
      :type n: node

      :raises NetworkXError: If n is not in the graph.

      .. seealso:: :obj:`predecessors`

      .. rubric:: Notes

      neighbors() and successors() are the same.


   .. py:method:: predecessors(n)

      Returns an iterator over predecessor nodes of n.

      A predecessor of n is a node m such that there exists a directed
      edge from m to n.

      :param n: A node in the graph
      :type n: node

      :raises NetworkXError: If n is not in the graph.

      .. seealso:: :obj:`successors`



