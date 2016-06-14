========================
Using khmer on real data
========================

khmer contains a set of command-line scripts that wrap the library code.
These scripts can serve a variety of purposes; the main ones are
k-mer abundance distribution analysis, k-mer abundance trimming,
digital normalization, and partitioning.  There are also a set of
scripts for data handling (e.g. read interleaving) that are just useful
to have around.

The documentation for the command line scripts is at
https://khmer.readthedocs.io/en/v2.0/user/scripts.html

Note that we use `Semantic Versioning <http://semver.org/>`__ on the
scripts, so (unlike the Python and C++ APIs) we guarantee that
scripts will retain backwards compatibility (vice bugs) within any major
release number, and that any minor release will only add command-line
flags.

Some background philosophy
==========================

We have largely shied away from actually producing a final result, and
instead focused on building tools and utilities that manipulate data
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

  This was actually one of the motivations behind the semi-streaming
  trimming, which does very efficient k-mer error trimming using the
  diginorm paradigm without actually changing the coverage profile much.

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
``abundance-dist-single.py``, but for multiple data files you'll need
to use ``load-into-counting.py`` and concatenate the files into
``abundance-dist.py``.

There are several utilities, including read coverage distribution
analysis (see ``sandbox/calc-median-distribution.py``).

K-mer abundance trimming
========================

khmer is particularly good at removing low-abundance k-mers from data
sets, for the purpose of removing errors and decreasing memory
requirements (see `Zhang et al., 2014 <http://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0101271>`__).
Typically this is done on a
per-read basis, where each read (or pair of reads) is examined in the
context of a k-mer database, and then the read(s) are trimmed at a
given position.

We have two methods for doing this, non-streaming and streaming.

Non-streaming
-------------

The non-streaming procedure is:

1. Create a countgraph from all of the data (``load-into-counting.py``)

2. For each data set, use ``filter-abund.py`` with the created countgraph to
   do abundance trimming.

Streaming
---------

The streaming procedure is to run ``trim-low-abund.py`` on all your
data at once.  This is described in `Zhang et al., 2015
<https://peerj.com/preprints/890/>`__.

All the data vs some of the data
--------------------------------

For trimming, an important consideration is that "hard" k-mer
abundance trimming (where you eliminate any k-mer with an abundance
below a threshold) needs to be done in the context of the entire data
set.  "Soft" or variable-coverage k-mer abundance trimming (``-V``)
can be done on each read data set individually, but should then be
re-done on the whole data set.

Note, you can trim high-abundance k-mers off of reads with
``sandbox/trim-below-abund.py``; this helps reduce and remove repeats.

Note the correspondence between high-abundance k-mers and repeats.

Digital normalization
=====================

Digital normalization (`Brown et al., 2012
<https://arxiv.org/abs/1203.4802>`__) can be run with the
``normalize-by-median.py`` script; optionally, you can run it and do
read trimming at the same time, using ``trim-low-abund.py
--diginorm``.

If you're not doing k-mer abundance trimming simultaneously with diginorm,
then we recommend a three-pass approach to digital normalization:

1. Run normalize-by-median to a coverage of 20 (``-C 20``);

2. Apply error trimming (filter-abund.py);

3. Run normalize-by-median to a coverage of 5.

Otherwise you will statistically end up with a lot of errors in the 5 reads
you have covering any particular locus.

All the data vs some of the data
--------------------------------

Diginorm can be run piecemeal on large data sets, because it retains
low-coverage data; but by the end you should run it on all your data
simultaneously.

Partitioning
============

We don't really recommend using this at the moment, but here's roughly
how it works:

0. (Optionally, but recommended) Run digital normalization on all your reads.

1. Build a tagged graph from diginorm reads (``load-graph.py``).
   
2. Partition the graph (potentially piecewise) using ``partition-graph.py``.

3. Merge partitions (``merge-partitions.py``)

4. Extract the reads by partition (``extract-partitions.py``).

For small data sets, you can run ``do-partition.py`` to do all of this at
once.

Partitioning is described in `Pell et al., 2012
<http://www.pnas.org/content/109/33/13272.abstract>`__ and applied to
soil metagenomes in `Howe et al., 2014
<www.pnas.org/content/111/13/4904>`__.

Note:

* k-mer size k < k_0 issue: if you partition at a low k-mer size, all higher
  k paths remain.

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

Most of the scripts are either low memory or constant memory; in the
case of the constant memory scripts, the max memory used is specified
by the ``-M`` parameter, plus epsilon ("a small bit").  The major
exception here is ``filter-abund.py`` and ``filter-abund-single.py``
which use 1.1x the ``-M`` parameter because they allocate both a Countgraph
and a Nodegraph.

For any scripts that use ``Nodegraph`` or ``Countgraph``, the optimal
memory parameters can be computed from the number of unique k-mers present
in the data set (which is one of the reasons for the HyperLogLog
functionality).

Counting scripts may use the optional ``bigcount`` code which stores
high counts for k-mers exactly, in an STL map.

Partitioning and tagging use unconstrained amounts of memory.

The impact of low memory and false positives on graph structure is
discussed in `Pell et al., 2012
<http://www.pnas.org/content/109/33/13272.abstract>`__, and the effect
of false positives on k-mer counting is analyzed in `Zhang et al.,
2014
<http://journals.plos.org/plosone/article?id=10.1371%2Fjournal.pone.0101271>`__

K-mer sizes, and k-mer size limitations
---------------------------------------

We have two hash functions implemented in khmer currently.

The primary hash function (as implemented by ``hash``) is reversible
(with ``reverse_hash``).  Hash values are stored in 64-bit numbers and
hence can only go up to k=32.  This can be problematic, but the
reversibility is thoroughly baked into the traversal code and we need
to do a reasonable amount of refactoring to fix this (see
https://github.com/dib-lab/khmer/issues/60 and linked issues).  An
alternate solution would be to allow multiple hash functions
(see https://github.com/dib-lab/khmer/issues/27).

The "secondary" hash function (used by HyperLogLog and MinHash sketching) is
64-bit `MurmurHash <https://en.wikipedia.org/wiki/MurmurHash>`__.  It is
an irreversible hash function that can be used for arbitrary text.  There
is no k-mer size limitation on it.

Input and output formats
------------------------

khmer consumes FASTA and FASTQ, both single- and paired-end.

khmer's output format leaves the headers unchanged, with the exception of
the partitioning code, which annotates the headers with the partition
ID.  (Yes, this was a bad idea.)

Paired end sequence names
~~~~~~~~~~~~~~~~~~~~~~~~~

khmer supports most of the paired-end naming schemes.

Interleaved vs split reads
~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, khmer takes in "broken-paired" format, in which pairs of
reads are immediately adjacent to each other in the file.  This can be
easily and efficiently split into paired/single-ended files, as well
as left- and right-reads.  We have scripts to do this, too.

The major rationale for broken-paired format is that it permits reads to
be kept together for the purposes of streaming.
