Molecule Formats
================

The *Molecule Formats* appendix describes the file formats supported by
the Mychem software. Further informations about chemical file formats
are available on
`Wikipedia <http://en.wikipedia.org/wiki/Chemical_file_format>`_.

Serialized OBMol
----------------

The serialized OBMol format is a bit string obtained by serializing an
OBMol object. This type of string stores most of the data contained in a
OBMol object, with exception to 2D and 3D coordinates. The binary
structure of this string is described on the `ChemiSQL
Website <http://chemdb.sourceforge.net/wiki/index.php/Binary_storage_of_OBMol_objects_for_faster_operations>`_.

CML
---

The Chemical Markup Language (CML) is an open standard for representing
molecular structures, as well as many other types of chemical data. It
is based on the XML language and can be simply processed by any XML
parser. The `CML Project
Website <http://sourceforge.net/projects/cml/>`_, hosts the XML Schema
and source codes for parsing and working with CML data.

Fingerprints
------------

This section presents fingerprint types used by the Mychem software.
Further details about fingerprints are given in an
`article <http://www.dalkescientific.com/writings/diary/archive/2008/06/26/fingerprint_background.html>`_
written by Andrew Dalke. In short, fingerprints are a bit string
representation of a molecule. Most of them can be classified in two
categories:

-  Structural fingerprint - This fingerprint type is based on
   substructure features.

-  Hash fingerprints - This fingerprint type is a hash of the
   representation of a molecule. It is used most often in similarity
   searching, with the hypothesis that two similar compounds create
   similar fingerprints, and that two similar fingerprints means the
   compounds are similar.

FP2
+++

FP2 fingerprints index small molecule fragments based on linear segments
of up to 7 atoms in length. They are hash fingerprints. The
specification of the FP2 fingerprints is available on the `Open Babel
Website <http://openbabel.org/wiki/FP2>`_.

FP3
+++

FP3 fingerprints index small molecule fragments based on a list of SMART
patterns. They are hash fingerprints. The SMART patterns are listed in
the file named ``patterns.txt``, that is distributed with the Open Babel
software. The specification of the FP3 fingerprints is available on the
`Open Babel Website <http://openbabel.org/wiki/FP3>`_.

FP4
+++

FP4 fingerprints index small molecule fragments based on a list of SMART
patterns. They are hash fingerprints. The SMART patterns are listed in
the file named ``SMARTS_InteLigand.txt``, that is distributed with the
Open Babel software. The specification of the FP4 fingerprints is
available on the `Open Babel Wiki <http://openbabel.org/wiki/FP4>`_.

InChI
-----

The IUPAC International Chemical Identifier (InChI) is an identifier for
chemical substances that can be used in printed and electronic data
sources. It was developed under IUPAC Project 2000-025-1-800. Details of
the project are available from the `IUPAC
Website <http://www.iupac.org/inchi>`_.

Sybyl Mol2
----------

The Sybyl Mol2 format is a complete, portable representation of a
molecule. It is an ASCII file that contains structural data as well as
Sybyl related data (Sybyl is a chemoinformatics software released by
Tripos). The file format is described on the `Tripos
Website <http://www.tripos.com/data/support/mol2.pdf>`_.

MDL Molfile
-----------

A MDL Molfile is a file format created by MDL for holding data about the
atoms, bonds, connectivity and coordinates of a molecule. This file
format consists of some header information, the Connection Table (CT)
containing atom info, then bond connections and types, followed by
sections for more complex information. The format is described on the
`MDLI
Website <http://www.mdli.com/downloads/public/ctfile/ctfile.jsp>`_.
There is two versions of MDL Molfile:

-  V2000

-  V3000

V2000
+++++

It is the current standard. However, it presents a limitation, as it
support up to 999 atoms or bonds.

V3000
+++++

The V3000 format has been created to support more than 999 atoms or
bonds. It can be useful for describing proteins and polymers.

PDB
---

The Protein Data Bank Format is commonly used for proteins and
biological macromolecules. It was originally designed as a
fixed-column-width format and thus officially has a built-in maximum
number of atoms; however, many tools can read files that exceed the
limit. Some PDB files contain an optional section describing atom
connectivity as well as position. Further informations about this format
can be retrieved from the `PDB Website <http://www.wwpdb.org/docs.html>`_.

SMILES
------

The Simplified Molecular Input Line Entry Specification (SMILES) format
is a linear text format which can describe the connectivity and
chirality of a molecule. It does not include 2D or 3D coordinates and
hydrogens atoms are not represented. The SMILES are described on the
`Daylight Website <http://www.daylight.com/smiles/>`_. However, a
complete SMILES standard does not exist. Craig James is leading a
campaign to develop an Open Standard for SMILES. The discussion is
taking place under the umbrella of the `Blue Obelisk
Group <http://blueobelisk.sourceforge.net/wiki/Open_Standard_for_SMILES>`_.

Canonical SMILES
++++++++++++++++

Canonical SMILES is a particular type of SMILES, that has a single
*canonical* form for any particular molecule, regardless of atom order.
