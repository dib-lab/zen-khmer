====================================
Exploring graphs and graph structure
====================================

Graphs can be a tremendously powerful way to look at sequencing data,
but unlike k-mers, operating on graphs can be very computationally
expensive.  This is largely because of combinatorics: for example,
forks and loops in the graph make simple traversal expensive.  Some of
khmer's functionality is aimed at simplifying, or at least making possible,
some basic traversal approaches.

Building waypoints in De Bruijn graphs
======================================

khmer provides a technique called 'tagging', in which a subset of
k-mers in the graph are chosen as representatives of the whole graph -
and we choose these representatives so that *every* k-mer in the
entire graph is within distance D of at least one representative.

(khmer does this by using the reads as a guide into the graph;khmer
makes sure that each read has a tag every D k-mers, and, since the
graph is entirely constructed from reads, then the graph ends up being
tagged at distance D as well.)

Tagging has a few nice features:

* there are many fewer tags than there are k-mers (2D fold fewer,
  approximately.)
  
* the structure and connectivity of the tags serves as a good proxy
  for the connectivity of the overall graph; in this way they serve
  as a sort of sparse graph.

* tags are somewhat refractory to coverage: highly covered regions will
  get many fewer tags than the number of reads from that region.

* tags provide a good entry point into the graph for traversal; in essense,
  they provide waypoints for other work.

* tags could, in theory, be used as an index into the reads. (@CTB expand.)

Graph theory friends tell me that this is actually a D-dominating set,
and we'll revisit this concept later on (c.f. spacegraphcats).

.. note::

   Right now we don't do a very efficient job of creating the tags.
   I have a few ideas for improving this using streaming approaches.

.. note::

   Our tag data structure implementation is pretty bad - we use an STL set
   to store the tags...

To make use of tags, the tags need to be built first.  Here's an example.

First, construct a Nodegraph:

>>> K = 21
>>> ng = khmer.Nodegraph(K, 1e6, 4)

Create a sequence longer than K:

>>> seq = 'CCCTGTTAGCTACGTCCGTCTAAGGATATTAACATAGTTGCGACTGCGTCCTGTGCTCA'

Now, instead of adding it to ``ng`` with ``consume``, use ``consume_and_tag``::

>>> ng.consume_and_tag(seq)

You can now look at the tags with ``get_tags_and_positions``::

>>> ng.get_tags_and_positions(seq)
[(19, 156582046028), (39, 1984875433400)]

Here, the first element of each tuple is the position of the tag
within the sequence, and the second element is the actual tag -- you
can convert those back into DNA with ``reverse_hash``::

>>> x = ng.get_tags_and_positions(seq)
>>> ng.reverse_hash(x[0][1])
AACTATGTTAATATCCTTAGA

Points to make:

* you can use any query sequence you want, not just the one you used to
  create the tags ;).

* the tagging distance D is 40 by default, and can be adjusted with
  ``_set_tag_density``.

Partitioning
============

The motivation behind tags came from a desire to *partition* data sets:
back when, our intution was that metagenome data sets should split up
into disconnected components based on their origin species.  (You can
read more in Pell et al., 2012. @cite.)

This is challenging because you essentially want an all-by-all connectivity
map for the graph - you want to know, does read A connect (transitively)
to read Z, by any path, and you want to know this for all the reads in the
data set.  This is hard!

We initially developed tagging for the purpose of partitioning, and we
do the following:

* tag the graph systematically (as explained above);

* explore to a distance D from each tag, and identify all neighboring
  tags within distance D;

* assign partition IDs to each set of connected tags;

* split the reads up based on their partition IDs.

This does work, mostly, and it turns out to split the graph up into
bins that group reads by species. @@more here.

For an example of partitioning, let's construct an artifical sequence.

>>> longseq = ("AGGAGGAGCATTTGTGCGATGCCCTATGGGGAGACCTATCTGCCGGGGAAATGCGCACA",
...    "TAACATAATCTAATCTACCACATTATGAACCCCCAGTGGGCACGTGTTCATTGCGTACGATCGCATTC",
...    "TACTTGATTCCCGCAGTGGTACGACGCTATGTA")
>>> longseq = "".join(longseq)

If we break this 160-bp sequence up into three bits,

>>> begin = longseq[:70]
>>> middle = longseq[40:120]
>>> end = longseq[90:]

we can see that ``begin`` and ``end`` shouldn't connect without ``middle``.
Let's see if partitioning agrees!

First, let's build a graph and then add the beginning and end bits:

>>> K = 21
>>> ng = khmer.Nodegraph(K, 1e6, 4)
>>> ng.consume_and_tag(begin)
>>> ng.consume_and_tag(end)

Now, run the partitioning code, get back a partition object, and ask
for a partition count:

>>> p = ng.do_subset_partition()
>>> n_partitions, _ = p.count_partitions()
>>> print(n_partitions)
2

Yep - 2 partitions!

Now let's add the middle bit, which will connect the two isolated partitions:

>>> ng.consume_and_tag(middle)

If we redo the partitioning, we should now get 1 partition:

>>> p = ng.do_subset_partition()
>>> n_partitions, _ = p.count_partitions()
>>> print(n_partitions)
1

...and we do!

Partitioning hasn't, in the end, proven to be that useful in practice.
The downsides of the approach are:

* it turns out that any source of systematic bias in sequencing will
  result in a completely connected graph (@cite); such biases are
  common.

* it also turns out that most metagenome graphs are, in reality,
  completely connected.

However, tags are still potentially useful for other things than straight
up partitioning.

Labeling graphs
===============

Camille Scott extended tagging with *labels* - the idea is that
each tag can be labelled with 1 or more arbitrary identifiers.
This lets you do things like build colored De Bruijn graphs,
equiv. do pangenome analysis.

References:

http://ivory.idyll.org/blog/2015-wok-labelhash.html

https://github.com/dib-lab/2015-khmer-wok4-multimap/blob/master/do-counting.py

Building compact De Bruijn graphs
=================================

De Bruijn graphs themselves are often quite large, because they have
as many nodes in them as there are unique k-mers in the data set.
Many of these k-mers may be collapsable into linear paths.  The result
of doing this graph contraction is called a "compact De Bruijn graph",
and we have some functionality for building them in khmer.

This functionality consists of two functions on graphs:
``find_high_degree_nodes`` and ``traverse_and_mark_linear_paths``.

There's a script sandbox/extract-compact-dbg.py.

.. @hmm, could build with two different grahps at same time, different fp

Visualizing compact De Bruijn graphs with Gephi
===============================================

Assembling linear sequences
===========================

Next: :doc:`summarizing-khmer-basics`
