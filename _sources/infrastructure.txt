====================================================
Infrastructure and development for the khmer project
====================================================

Version control
===============

All of khmer development is done at http://github.com/dib-lab/khmer/ using
Git and GitHub.

Issue tracking
==============

All issue tracking is done via github issues:
https://github.com/dib-lab/khmer/issues

Note especially meta-issues:
https://github.com/dib-lab/khmer/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+meta

Automated tests
===============

All tests are under ``tests/``.

We use py.test.

All tests can be run with 'make test'.

You can run specific files with ``py.test <filename>``.

Documentation
=============

Documentation is written in reStructuredText.

Documentation is hosted at https://khmer.readthedocs.io/.

Literate resting
================

We keep our tutorials and recipes working with automated tests a la:

https://github.com/dib-lab/literate-resting

Benchmarking
============

See @jessicamizzi's latest,
https://github.com/jessicamizzi/ep-streaming/blob/master/Utilization-Figures.ipynb,
and 2014 PyCon (http://ivory.idyll.org/blog/2014-pycon.html).

Communication
=============

There are two mailing lists (`khmer
<http://lists.idyll.org/listinfo/khmer>`__ and `khmer-announce
<http://lists.idyll.org/listinfo/khmer-announce>`__); we have a
`gitter channel <http://gitter.im/dib-lab/khmer>`__; and that's about
it.

Proposing changes and code review
=================================

We use `GitHub Flow <https://guides.github.com/introduction/flow/>`__;
basically, submit pull requests.

Docs for getting started:
https://khmer.readthedocs.io/en/v2.0/dev/getting-started.html

We have coding guidelines and a code review checklist:
https://khmer.readthedocs.io/en/v2.0/dev/coding-guidelines-and-review.html

Sandbox scripts
===============

We have lots of scripts in ``sandbox/`` that are not officially
supported but may be useful - see `README
<https://github.com/dib-lab/khmer/blob/master/sandbox/README.rst>`__.

Release checklist
=================

We have a release checklist:

https://khmer.readthedocs.io/en/v2.0/dev/release.html
