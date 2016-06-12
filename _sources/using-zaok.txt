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
run it there, by clicking::

  ...
