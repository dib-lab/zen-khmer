==================================
Preface - using this documentation
==================================

There are three ways to use this document.

Reading it
==========

First, you can just read it.  Booooring.

---

Running it locally
==================

Second, you can clone it to your local computer with::

  git clone http://github.com/dib-lab/zaok.git

and run it there.  You'll also need to have Python 3.5 and specific
versions of khmer and some other software installed -- we suggest you
create a virtualenv and install those software packages like so::

  cd ./zaok/
  python3.5 -m virtualenv zaok_env
  source zaok_env/bin/activate
  pip install -r requirements.txt

Then, you can run::

  jupyter notebook &

----

Running it in a binder on mybinder
==================================

Third, you can take advantage of `mybinder <http://mybinder.org>`__ and
run it there, by clicking:

.. image:: http://mybinder.org/badge.svg
   :target: http://mybinder.org/repo/dib-lab/zaok

Building the full documentation yourself
========================================

The documentation source (reStructuredText + Jupyter Notebooks) is in
the GitHub repository is http://github.com/dib-lab/zaok.git

To build it all, you'll need to install sphinx, as well as the stuff
in requirements.txt.

Then run::

  make html

and look in ``_build/html/`` for the output.

The GitHub pages site hosted at https://dib-lab.github.io/zen-khmer/
is in the gh-pages/ branch at https://github.com/dib-lab/zen-khmer.
(We do it this way to avoid having to switch branches to update the
docs.)

Jupyter Notebook quickstart
===========================

``Shift-ENTER`` will execute a cell.

You can go in and edit a cell and re-run it with ``Shift-ENTER``.

The Kernel menu has an entry 'Restart & Run all', too.
