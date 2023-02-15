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

# Windowing

```{admonition} Prerequisites

* [You have installed Flow](installing)
* [You can specify graphs](specifying-graphs)
* Optional: [You can create nodes](writing-nodes)
```

In this guide you will learn how to specify, with each firing, the number of
input values a node must consume on an input port, and the number of output
values a node will produce on an output port. The term we use for "the number of
values" is *window size*, where the *window* is the sequence of value a node
will "see" at a firing.

The first part will cover how to specify window sizes when you specify a graph.
The second part will cover how to use window size when you write your own nodes.

## Specifying window size

The window size can be specified for input ports and output ports. For an input port it signifies the number of values that will be consumed from the channel and passed to the node with each firing. For an output port it signifies the number of values that will be produced on the connected channel with each firing.

In the example below we read a soundbite from a file and stream it to an audio playback node. Without any window specification this would be done sample-by-sample, each firing producing and consuming 1 sample, leading to a lot of overhead.

```{code-cell}
from flow.lang import graph
from flow.lib import plot
from flow.lib.audio import wave

with graph() as wp:
    wave.read_wav(
        filename="StarWars3.wav", 
        windows={"out":256}
    ) >> plot.line(
        ymin=-32768, 
        ymax=32767, 
        maxt=100, 
        windows={"values":32}
    )
```

As you can see, both node specification include a configuration for `windows`. This configuration variable is recognized and interpreted by Flow. It expects a dictionary where the keys are port names, either input or output ports, and the values are the sizes of the windows.

In this example `wave.read_wav` produces 256 samples with each firing, and `plot.line` consumes 32 samples with each firing. As a result, each firing of `wave.read_wav` must result in 8 firings of `plot.line`.

```{warning}

Depending on the node implementation, not all combinations of window sizes are allowed. Currently, a bad configuration of window size will at best lead to an exception.
```

## Using window size in a node implementation

When implementing a node it can be useful to have the window size that is expected on an output port. For this purpose the node can request the configuration argument `windows`. 

If one of the configuration keyword arguments of a node is called `windows`, flow will pass the final window size configuration mapping in this argument. 

Take the node `values` from {py:mod}`flow.lib.gen`. It streams values from a fixed sequence, configurable with the `values` argument, to its output. The window size determines how many values are passed with each firing:

```{literalinclude} ../../flow/lib/gen.py
---
linenos: yes
pyobject: _values
---
```

The output port is called `out`, the default output port name if none can be deduced from the definition. In this implementation the `windows` mapping used to produce the correct number of values on the output.