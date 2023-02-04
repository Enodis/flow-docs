---
jupytext:
  cell_metadata_filter: -all
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Specifying graphs

```{admonition} Prerequisites

* [You have installed Flow](installing)
```

In this guide you will learn how to create a flow graph from a number of flow
node definitions. In the guide ["Writing nodes"](writing-nodes) you can learn
how to define your own nodes, in this guide we are going to use a number of
built-in flow nodes to construct a flow graph. We will also explore a few things
that can be done with the flow graph once it is constructed.

Specifying flow graphs is done with the `lang` module. The module responsible for the
language that describes flow nodes and flow graphs. We need one components from
this module:

* {py:func}`graph <flow.lang.composition.graph>`: a context-manager that creates
  a new {py:mod}`Model <flow.model.graph.Model>` and activates it such that it
  will be used in the `with` statement when nodes are instantiated. nodes for
  Python objects.

We import this component as follows:

```{code-cell}
from flow.lang import graph
```

## Node instantiation

A node is defined by the functionality it provides and the inputs and outputs it
has, much like a normal function. And where a function needs to be *called* for
it to be of any use, a *node* needs to be *instantiated*.

Node *instantiation* does two to three things:

1. It *creates a unique instance* for that node in the graph. The *instance*
   manages the live behavior of the node. It is what will be configured,
   connected, and updated during execution. The same node may be instantiated
   again for another part of the same graph. 
2. It *configures the node*, if necessary. Some node require an initial
   configuration, that may or may not be static throughout the execution.
3. Instantiation may partially *connect the inputs* of a node, but this is not
   mandatory.

## The first example

We are going to create a small data flow graph with a single *source* node that
streams values from a fixed sequence to a *sink* node that stores the sequence in
a list.

````{margin}
```{note}
The packages {py:mod}`flow.lib.gen` and {py:mod}`flow.lib.out` come with flow 
and offer a number of simple nodes to stream values into  and out of a data flow 
graph.
```
````

For the source node we will use {py:mod}`flow.lib.gen.values`, and for the sink
node we will use {py:mod}`flow.lib.out.sequence`.

To instantiate these we must call them, much like a constructor. The *positional
arguments* of such node constructors *may* be used to connect input channels.
This is optional, though. You can also choose to connect a node later.

Configuration of a node is always done with *keyword arguments*.

Let's examine the example:

```{code-cell}
from flow.lib import gen, out

output = []

with graph() as g:
    vs = gen.values(values=range(10))
    out.sequence(vs, out=output)
```

There are three things to note here. 

First, the node is constructed in a `with graph()` context. This creates a new
data flow graph, and makes sure that any newly created node is added there. The
data flow graph is made available in `g` and can be accessed later for
inspection or execution.

Second, the result of the call to `gen.values` is captured in a variable, `vs`.
Later on we can use this variable to connect the output of the node to other
nodes.

Third, the call to `out.sequence` mixes connecting an input with configuration.
The variable `vs` is passed without a keyword and is thus treated as a streaming
input, and the variable `output` is passed *with* keyword and is treated as a
configuration.

## The streaming operator `>>`

It happens very often that one node is only connected to the next which is again
connected to the next, and so on, making a kind of chain of nodes. Writing such
chains with nested calls does not help readability. Luckily, you can use the
streaming operator `>>` of C++ fame to do the same thing. Our example becomes:

```{code-cell}
from flow.lib import gen, out

output = []

with graph() as g:
    gen.values(values=range(10)) >> out.sequence(out=output)
```

But the strength of this operator is demonstrated better when we take the
mean over a number of values:

```{code-cell}
from flow.lib import gen, arith, out

output = []

with graph() as g:
    gen.values(values=range(10)) >> arith.mean().windows(5) >> out.sequence(out=output)
```

## Bundling multiple streams

Multiple channels can be grouped into a single object, called a
*bundle*. Each channel in a bundle is named, something that is
used to connect bundles to actors.

When you connect a bundle to an actor the port names of the actor will be used
to look-up channel names in the bundle, and only those will be connected. In
fact, each node has a bundle for the input ports, and a bundle for the output
ports.

````{margin}
```{note}
At the time of writing this guide, the nodes in this example do not actually exist, they are merely used to demonstrated the use of *bundles*.
```
````

Let's see how this work with an example. Suppose we have a simple graph that
rebalances the left and right channel of an audio file and plays it on a
speaker. The graph could look like this:

```py
@node(namespace=audio)
@dataclass
class rebalance:
  left: Sequence[float]
  right: Sequence[float]

  def __init__(self, left: Sequence[float], right: Sequence[float], 
               *, left_right: float=0) -> None:
      sf = 1-left_right if left_right>0 else 1
      xf = 0-left_right if left_right<0 else 0
      self.left  = [l*sf + r*xf for l, r in zip(left, right)]
      self.right = [l*xf + r*sf for l, r in zip(left, right)]

with graph() as g:
    wav.read("file.wav") >> audio.rebalance(left_right=0.8) >> audio.play()
```

<!-- TODO: rebalance actually doesn't work: we cannot have the same name for input and output right now! -->

Assuming the `wav.read` outputs a channel "left" and "right", this would
automatically connect those outputs to proper ports of the `rebalance` node,
and the same holds for the channel to `audio.play`.

## Node views

When a node has many input or output channels, it is sometimes useful to select
the ones you want to connect. This can be achieved by indexing the node with the
names of the ports of interest. For example, `node["input1", "output2"]`, would
return a view on `node` with only the ports `input1` and `output2`.

At other times it is useful to create a view on a node but with different names for the ports. This is especially useful when combining generic nodes, with specialized ones. This can be achieved with the `view` method of a node. 

Suppose you want to plot the left channel of a WAV file. You could do this in the following ways:

1. Create a renamed view on `plot.line`:  
   ```py
   wav.read("file.wav") >> plot.line().view(values="left")
   ``` 
   
   This works because the input of `plot.line` is called `values`, and the
   channel is connected by name when there are multiple to choose from.

2. Create a renamed view on `wav.read`: 
   ```py
   wav.read("file.wav").view(left="values") >> plot.line()
   ```

   Same reason as (1).
   
3. Create a single output view on `wav.read`: 
   ```py
   wav.read("file.wav")["left"] >> plot.line()
   ```

   This works because connecting a single pipeline to a port always works,
   regardless of the name.