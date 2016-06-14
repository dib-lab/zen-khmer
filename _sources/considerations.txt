===============================
Perspectives and considerations
===============================

Avoid assembling as long as possible
====================================

Heuristics remove real information.

* strain variation is collapsed
* repeats are ignored.
* isoforms removed.

Key point is that true path(s) are in the graph, within limits of
coverage.  Keep graph around as long as possible.

Be cloud compatible
===================

Low memory, low I/O. Heavy compute is fine.

Think streaming
===============

We think streaming is the future.  We should do more of it.

Be practically efficient, but not necessarily optimized
=======================================================

Workflows should complete, and not fail (due to lack of memory or
other concerns); but at the same time, optimizing the heck out of code
tends to lead to incomprehensible code.  These considerations should
be balanced... somehow.

Correctness is more important than speed
========================================

Hopefully this is obvious.

Intermediate goal
=================

Our intermediate goal is to make asking questions of data quick and easy,
while simultaneously not killing our own career(s).

This typically involves finding a set of questions that can't be easily
addressed (or addressed at all), and then trying to find a common
solution to those problems that involves algorithmic or data structure
novelty.

There are obvious problems with this approach but it's worked so far.

Our end goal is biological data analysis - probably.
====================================================

In the past, we've focused on method development that is critical for
advancing biological data analysis - mostly our own.  This means that
many of our tools have fairly narrow applicability.

This is open to change, since it's becoming clear(er) that the kinds
of methods we are building have broad(er) applicability across
biology.  However, it is hard to make such arguments to funding agencies.

We don't expect other people to use our software...
===================================================

There are lots of reasons for people not to invest in using khmer:
wrong language, spotty support, etc.  And this is completely OK by me:
khmer is about exploring useful research ideas and boiling them down to
simple concepts that are as simple as possible to reimplement.

For example, digital normalization is a three line algorithm.  Because
of this simplicity, there are at least four approximate
re-implementations of it roaming about:

* Trinity's `in silico normalization <http://ivory.idyll.org/blog/trinity-in-silico-normalize.html>`__
* NeatFreq from JCVI - `paper <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4245761/>`__
* BBNorm from JGI - `link <http://seqanswers.com/forums/showthread.php?t=49763>`__
* Mira - `link <http://mira-assembler.sourceforge.net/docs/DefinitiveGuideToMIRA.html#sect_ref_data_reduction>`__

...but stability of our software is still a core value
======================================================

We and others use our software to do things, not just to explore concepts
in bioinformatics and software development.  Therefore we should minimize
churn.
