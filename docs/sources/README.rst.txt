====================
Mychem Documentation
====================

The Mychem documentation uses reStructuredText format as its markup
language. For generating PDF and HTML outputs, we are using 
`Sphinx <https://github.com/sphinx-doc/sphinx>`_.

Sphinx is a tool that makes it easy to create documentation from
reStructuredText-formatted text.

To generate PDF or HTML from the files, Sphinx has to be installed.
If Sphinx is not provided by our distribution, you can install it
using **pip**:

::

   $ pip install -U sphinx

Once Sphinx is installed, the PDF version can be generated from the
Mychem documentation repository with:

::

  $ make latexpdf

or HTML with:

::

  $ make html
