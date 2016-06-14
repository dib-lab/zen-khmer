================================
Off label and emerging use cases
================================

Underpublicized tools
=====================

HyperLogLog
-----------

Count numer of unique k-mers very efficiently.  Can be used in
streaming/observation mode.

`Preprint
<http://biorxiv.org/content/early/2016/06/07/056846>`__

Xander
------

Assemble directly from the graph using protein Hidden Markov Models.
Note, Xander is implemented in Java, and is not built into khmer.

`Paper
<https://microbiomejournal.biomedcentral.com/articles/10.1186/s40168-015-0093-6>`__

sourmash
--------

`Documentation/paper
<http://sourmash.readthedocs.io/en/latest/>`__. MinHash
sketches. Note, the library is directly integrated into khmer; see
spacegraphcats, below.

Blog posts: http://ivory.idyll.org/blog/2016-sourmash.html and
http://ivory.idyll.org/blog/2016-sourmash-signatures.html

screed
------

`Documentation <https://screed.readthedocs.io>`__.

Pretty useful, but unpublished. Perhaps a `JOSS pub? <http://joss.theoj.org>`__

Applying existing tools to new problems
=======================================

Diginorm for genomes
--------------------

Digital normalzation can be used on really messy genome sequencing data:
https://www.ncbi.nlm.nih.gov/pubmed/23985341

diginorm for reference-based work
---------------------------------

(Tamer's work with cufflinks and variant calling.)

(Erich's work with RNA scaffolding.)

k-mer baiting
-------------

(Jaron's work on extracting 16s.)

Unpreprinted fodder
===================

Graph alignment, error correction, and variant calling
------------------------------------------------------

Read-to-graph alignment: http://ivory.idyll.org/blog/2015-wok-error-correction.html

Variant calling: http://ivory.idyll.org/blog/2015-wok-variant-calling.html

Note that this functionality can be used on any kind of graph
(normalized, abundance-filtered, etc.)

More generally, *read graphs* seem useful...

IGS
---

Reads are windows into the graphs and can be used across data sets.
(IGS presentation.)

Developing new algorithms, data structures, and tools
=====================================================

MinHash sketches on graphs
--------------------------

* MinHash sketches can be applied piecewise to graphs.

spacegraphcats, domination hierarchies, and the catlas
------------------------------------------------------

* species separation & better species partitioning

(presentation)

Future directions
=================

In no particular order,

* Streaming assembly
* Graph indexing of short- (and long?)-read data sets
* Structural variation analysis
* Graph searching more generally
* RNAseq exploration and downsampling
