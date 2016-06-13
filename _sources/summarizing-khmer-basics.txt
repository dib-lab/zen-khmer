======================
Summarizing the basics
======================

khmer is built around the idea of storing k-mer presence/absence and k-mer
counts efficiently.

It has a number of efficient algorithms for removing errors from sequencing
data.

It also has some functionality for exploring, tagging, and labeling graphs.

We're just adding some newer functionality for looking at compact De
Bruijn graphs and assembling directly off of the graph.

What is unique, special, or otherwise different about khmer?
------------------------------------------------------------

None of these are unique, but we like them.

* The library approach is sadly somewhat rare in bioinformatics.

* We have a Python API that we "dogfood" - we use it ourselves for everything.

* We are concerned about sustainability of software development, so we test
  the heck out of everything.

* Our focus on online and streaming algorithms is rather rare.

* Our explicit focus on low memory is also somewhat rare.

* Our overall approach (prefiltering and diagnostics) is rather different.
