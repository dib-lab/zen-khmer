========================
Using khmer on real data
========================

khmer contains a set of command-line scripts that wrap the library code.
These scripts can serve a variety of purposes; the main ones are
k-mer abundance distribution analysis, k-mer abundance trimming,
digital normalization, and partitioning.  There are also a set of
scripts for data handling (e.g. read interleaving) that are just useful
to have around.

Some background philosophy
==========================

We have largely shied away from actually producing a final result, and
instead focused on building tools and utilites that manipulate data
sets between generation and analysis.  In essence, we try to act as a
filtering and forensics tool.  There are several reasons for this:

* flexibility: providing tools that can be mixed and matched is more
  flexible than providing a single tool.

* format: consuming and producing common read formats is much easier
  to get right than producing fast-evolving "final" output formats.

* simplicity: many simple tools can still yield a complex result, while
  being simple to run and easy to understand (at least individually).

* correctness: simplicity and correctness are linked ;).

* scalability: simple tools can often be made to run quite fast.

* streaming: many khmer tools support streaming input & output.

So, most of the tools and approaches described below act to slice and
dice data, with the goal of feeding the data into other programs.

The downsides to this approach are:

* bioinformatics formats still change frequently enough that keeping
  khmer working with them is a chore.

* it adds to the complexity of the process.  Many biologists would like
  a tool that simply *does the job*, and khmer is most emphatically not
  that tool.

* people may (rightly or wrongly) blame khmer when their results are
  not good.  For example, people who apply digital normalization
  sometimes expect that they can continue to use their downstream
  tools with unchanged parameters, even though diginorm alters the
  coverage distribution; this is sometimes true, and sometimes not.



K-mer abundance distribution analysis
=====================================

With khmer, it is straightforward to compute k-mer abundance
histograms (equiv.  k-mer spectra) and related values from very large
data sets.  The procedure is as follows:

1. Create a countgraph from all of the data (``load-into-counting.py``).

2. Count the k-mers in a query data set using the counts in the countgraph;

3. Summarize the counts (e.g. ``abundance-dist.py``).

The most straightforward use for this is just computing the k-mer
abundance spectra; this can be done in one step for single files with
``abundance-dist-single.py``, but for multiple data files you'll need to
use ``load-into-counting.py`` and concatenate the files into ``abundance-dist.py``.

There are several utilities, including read coverage distribution
analysis (@@).

K-mer abundance trimming
========================

khmer is particularly good at removing low-abundance k-mers from data
sets, for the purpose of removing errors and decreasing memory
requirements (see XXX).  Typically this is done on a per-read basis,
where each read (or pair of reads) is examined in the context of
a k-mer database, and then the read(s) are trimmed at a given position.

We have two methods for doing this, non-streaming and streaming.

The non-streaming procedure is:

1. Create a countgraph from all of the data.

2. For each data set

Here an important consideration is that "hard" k-mer abundance
trimming (where you eliminate any k-mer with an abundance below a
threshold) needs to be done in the context of the entire data set.
"Soft" or variable-coverage k-mer abundance trimming can be done
on each read data set individually, but should then be re-done on
the whole data set.

Note, you can trim high-abundance k-mers off of reads with
``sandbox/trim-below-abund.py``; this helps reduce and remove repeats.

Digital normalization
=====================

@@can be done successively

Partitioning
============

We don't really recommend using this at the moment, but here's roughly
how it works:

...

Recipes
=======

We have a few recipes showing how to use various khmer scripts to do
analyses that might be of interest -- documents are `here
<https://khmer-recipes.readthedocs.io/en/latest/>`__, source is `here
<https://github.com/dib-lab/khmer-recipes>`__.

Protocols
=========

We have a pair of protocols that explain how to go from raw data to an
assembly, for RNAseq and metagenome data.  The latest version is in
the ``ctb`` branch; the documents are `here
<http://khmer-protocols.readthedocs.io/en/ctb/>`__, and the source is
`here <https://github.com/dib-lab/khmer-protocols>`__.

Note: the protocols serve as an important part of the release checklist,
because they verify that khmer works as part of a larger set of tools.

Choosing parameters
===================

Memory parameters, and where they matter
----------------------------------------

K-mer sizes, and k-mer size limitations
---------------------------------------

Input and output formats
------------------------

