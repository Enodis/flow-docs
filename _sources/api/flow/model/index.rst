:py:mod:`flow.model`
====================

.. py:module:: flow.model

.. autoapi-nested-parse::

   Data flow model.

   Time series/stream: named iterable of values `[a, b, c, ...]`

   Runner options (++ means concatenate sequences):

   1. pure function: *F([a, b, c, ...]) === [F(a), F(b), F(c), ...]*
   2. generator: *F(\*\*kwargs) === [a, b, c, ...]*
      or: *F(\*\*kwargs) === a ++ b ++ c ++ ...*
   3. windowed function: *F([a, b, c,...]) ~== F([a,b]) ++ F([b,c]) ++ F([c,...]) ++ ...*
      Depending on the window size and step size
   4. narrowing function:  *F([a, b, c, ...]) ~== [F([a,b]), F([b,c]), F([c,...]),   ...]*
      The difference with a windowed function is that the output of an input
      window is a single value. This may not matter, and can possibly be combined.
   5. bulk function: F needs complete input

   There are a variety of windowing alternatives. Runners work on a view on the
   input time series, and produce a single value or a sequence of values for a new
   time series.

   A runner may have multiple input streams and multiple output streams. Input
   stream values are passed as named arguments, output streams are returned as dict
   of iterables.

   Any of the functions may have state that needs to be initialized before
   execution.



Submodules
----------
.. toctree::
   :titlesonly:
   :maxdepth: 1

   runner/index.rst


Package Contents
----------------

Classes
~~~~~~~

.. autoapisummary::

   flow.model.Model
   flow.model.PortSpec
   flow.model.Runner
   flow.model.RunnerInit
   flow.model.IteratorRunner
   flow.model.StatefulRunner
   flow.model.StatefulRunnerInit
   flow.model.StatelessRunner
   flow.model.ListRef
   flow.model.SingletonRef
   flow.model.IValueRef




Attributes
~~~~~~~~~~

.. autoapisummary::

   flow.model.PortWindowSize
   flow.model.RunnerConfig















