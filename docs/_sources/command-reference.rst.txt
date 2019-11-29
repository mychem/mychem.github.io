Command Reference
=================

This chapter describes all SQL commands provided by Mychem. These
commands are classified in five sections:

-  :ref:`Conversion Commands` - this section details functions that
   convert chemical files.

-  :ref:`Helper Commands` - this section details functions that return
   informations about the Mychem environment.

-  :ref:`Modification Commands` - this section details functions that
   modify chemical structures.

-  :ref:`Molmatch Commands` - this section details functions that
   compare chemical structures.

-  :ref:`Property Commands` - this section details functions that
   compute molecular properties.

The Default Molecule Type
-------------------------

Many functions are using or returning a molecule in *DEFAULT_TYPE*
format. Since version 0.6.0 of Mychem, the DEFAULT_TYPE format is *MDL
Molfile (V2000)*. In earlier versions, the DEFAULT_TYPE format was
*InChI*.

The speed of several functions has been measured when using InChI and
MDL Molfile formats. It has been shown that using MDL Molfile is a bit
faster than InChI. Using the MDL Molfile format is also interesting, as
it permits to store 2D or 3D coordinates. Moreover, the MDL Molfile
format is commonly used by many softwares.

Conversion Commands
-------------------

Conversion commmands permit to convert chemical data from a format to
another format. Mychem uses the Open Babel library for the conversion,
but does not implement all the 80 chemical file formats supported by
Open Babel. Mychem supports only few of them. They are detailed in
the :ref:`Molecule Formats` appendix.

To avoid having too many functions, a set of two functions is available
for each format: FORMAT_TO_MOLECULE and MOLECULE_TO_FORMAT. If you
need other formats, please open a ticket on the `feature
tracker <https://github.com/mychem/mychem-code/issues>`_.

   **Note**

   If a function of this section fails, it returns an empty string.

-  ``CML_TO_MOLECULE(molecule)``

   ``CML_TO_MOLECULE`` converts a *molecule* in CML format to a
   molecule in DEFAULT_TYPE format.

   ::

       mysql> SELECT CML_TO_MOLECULE(cml_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 2-Aminoacetic acid
        OpenBabel11190809032D

        10  9  0  0  0  0  0  0  0  0999 V2000
          -0.1068   -1.0521    1.1509 H   0  0  0  0  0  0  0  0  0  0  0  0
           0.0877   -0.0798    0.6477 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.2870    0.7082    1.3331 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.5185    0.1919    0.3951 N   0  0  0  0  0  0  0  0  0  0  0  0
           2.0168    0.1266    1.2568 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.8890   -0.4693   -0.2544 H   0  0  0  0  0  0  0  0  0  0  0  0
          -0.6775   -0.0767   -0.6578 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.4455   -0.6968   -1.6798 O   0  0  0  0  0  0  0  0  0  0  0  0
          -1.7851    0.6991   -0.6705 O   0  0  0  0  0  0  0  0  0  0  0  0
          -2.2101    0.6489   -1.5212 H   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         2  3  1  0  0  0  0
         2  4  1  0  0  0  0
         2  7  1  0  0  0  0
         4  5  1  0  0  0  0
         4  6  1  0  0  0  0
         7  8  2  0  0  0  0
         7  9  1  0  0  0  0
         9 10  1  0  0  0  0
       M  END

-  ``MOLECULE_TO_CML(molecule)``

   ``MOLECULE_TO_CML`` converts a *molecule* in DEFAULT_TYPE format
   to a molecule in CML format.

   ::

       mysql> SELECT MOLECULE_TO_CML(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> <molecule id="id2-Aminoacetic acid">
        <name>2-Aminoacetic acid</name>
        <atomArray>
         <atom id="a1" elementType="H" x3="-0.106800" y3="-1.052100" z3="1.150900"/>
         <atom id="a2" elementType="C" x3="0.087700" y3="-0.079800" z3="0.647700"/>
         <atom id="a3" elementType="H" x3="-0.287000" y3="0.708200" z3="1.333100"/>
         <atom id="a4" elementType="N" x3="1.518500" y3="0.191900" z3="0.395100"/>
         <atom id="a5" elementType="H" x3="2.016800" y3="0.126600" z3="1.256800"/>
         <atom id="a6" elementType="H" x3="1.889000" y3="-0.469300" z3="-0.254400"/>
         <atom id="a7" elementType="C" x3="-0.677500" y3="-0.076700" z3="-0.657800"/>
         <atom id="a8" elementType="O" x3="-0.445500" y3="-0.696800" z3="-1.679800"/>
         <atom id="a9" elementType="O" x3="-1.785100" y3="0.699100" z3="-0.670500"/>
         <atom id="a10" elementType="H" x3="-2.210100" y3="0.648900" z3="-1.521200"/>
        </atomArray>
        <bondArray>
         <bond atomRefs2="a1 a2" order="1"/>
         <bond atomRefs2="a2 a3" order="1"/>
         <bond atomRefs2="a2 a4" order="1"/>
         <bond atomRefs2="a2 a7" order="1"/>
         <bond atomRefs2="a4 a5" order="1"/>
         <bond atomRefs2="a4 a6" order="1"/>
         <bond atomRefs2="a7 a8" order="2"/>
         <bond atomRefs2="a7 a9" order="1"/>
         <bond atomRefs2="a9 a10" order="1"/>
        </bondArray>
       </molecule>

-  ``FINGERPRINT(molecule, type)``

   ``FINGERPRINT`` converts a *molecule* in DEFAULT_TYPE format to a
   fingerprint. The fingerprint type is specified by the second argument
   (FP2, FP3 or FP4). The SQL type of the converted molecule is a binary
   string for all kinds of fingerprint.

   ::

       mysql> SELECT FINGERPRINT(molecule_col, "FP2") FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> binary fingerprint (type FP2)

   **Note**

   Tanimoto scoring can be improved by using a concatenation of
   different fingerprint types. The concatenation of fingerprints
   can be performed with the following query:

   ::

       mysql> SELECT CONCAT(FINGERPRINT(molecule_col,"FP2"),
           -> FINGERPRINT(molecule_col,"FP3")) FROM tbl_name
           -> WHERE id=9;
              -> binary fingerprint (type FP2 + type FP3)

-  ``FINGERPRINT2(molecule)``

   ``FINGERPRINT2`` converts a *molecule* in DEFAULT_TYPE format to a
   FP2 fingerprint. The SQL type of FP2 fingerprints is a binary string.

   ::

       mysql> SELECT FINGERPRINT2(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> binary fingerprint (type FP2)

-  ``FINGERPRINT3(molecule)``

   ``FINGERPRINT3`` converts a *molecule* in DEFAULT_TYPE format to a
   FP3 fingerprint. The SQL type of FP3 fingerprints is a binary string.

   ::

       mysql> SELECT FINGERPRINT3(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> binary fingerprint (type FP3)

-  ``FINGERPRINT4(molecule)``

   ``FINGERPRINT4`` converts a *molecule* in DEFAULT_TYPE format to a
   FP4 fingerprint. The SQL type of FP4 fingerprints is a binary string.

   ::

       mysql> SELECT FINGERPRINT4(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> binary fingerprint (type FP4)

-  ``INCHI_TO_MOLECULE(molecule)``

   ``INCHI_TO_MOLECULE`` converts a *molecule* in InChI format to a
   molecule in DEFAULT_TYPE format.

   ::

       mysql> SELECT INCHI_TO_MOLECULE(inchi_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 
        OpenBabel11190809142D

         5  4  0  0  0  0  0  0  0  0999 V2000
           0.0000    0.0000    0.0000 C   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 C   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 N   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 O   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 O   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         1  3  1  0  0  0  0
         2  4  2  0  0  0  0
         2  5  1  0  0  0  0
       M  END

-  ``MOLECULE_TO_INCHI(molecule)``

   ``MOLECULE_TO_INCHI`` converts a *molecule* in DEFAULT_TYPE format
   to a molecule in InChI format.

   ::

       mysql> SELECT MOLECULE_TO_INCHI(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> InChI=1S/C2H5NO2/c3-1-2(4)5/h1,3H2,(H,4,5)

   **Note**

   The ``INCHI_VERSION`` function permits to know
   which version of the InChI library is used.

-  ``MOL2_TO_MOLECULE(molecule)``

   ``MOL2_TO_MOLECULE`` converts a *molecule* in Sybyl Mol2 format to
   a molecule in DEFAULT_TYPE format.

   ::

       mysql> SELECT MOL2_TO_MOLECULE(mol2_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 2-Aminoacetic acid
        OpenBabel11190809032D

        10  9  0  0  0  0  0  0  0  0999 V2000
          -0.1068   -1.0521    1.1509 H   0  0  0  0  0  0  0  0  0  0  0  0
           0.0877   -0.0798    0.6477 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.2870    0.7082    1.3331 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.5185    0.1919    0.3951 N   0  0  0  0  0  0  0  0  0  0  0  0
           2.0168    0.1266    1.2568 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.8890   -0.4693   -0.2544 H   0  0  0  0  0  0  0  0  0  0  0  0
          -0.6775   -0.0767   -0.6578 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.4455   -0.6968   -1.6798 O   0  0  0  0  0  0  0  0  0  0  0  0
          -1.7851    0.6991   -0.6705 O   0  0  0  0  0  0  0  0  0  0  0  0
          -2.2101    0.6489   -1.5212 H   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         2  3  1  0  0  0  0
         2  4  1  0  0  0  0
         2  7  1  0  0  0  0
         4  5  1  0  0  0  0
         4  6  1  0  0  0  0
         7  8  2  0  0  0  0
         7  9  1  0  0  0  0
         9 10  1  0  0  0  0
       M  END

-  ``MOLECULE_TO_MOL2(molecule)``

   ``MOLECULE_TO_MOL2`` converts a *molecule* in DEFAULT_TYPE format
   to a molecule in Sybyl Mol2 format.

   ::

       mysql> SELECT MOLECULE_TO_MOL2(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               ->@<TRIPOS>MOLECULE
       2-Aminoacetic acid
        10 9 0 0 0
       SMALL
       GASTEIGER

       @<TRIPOS>ATOM
             1 HA1        -0.1068   -1.0521    1.1509 H       1  GLY1        0.0537
             2 CA          0.0877   -0.0798    0.6477 C.3     1  GLY1        0.0918
             3 HA2        -0.2870    0.7082    1.3331 H       1  GLY1        0.0537
             4 N           1.5185    0.1919    0.3951 N.3     1  GLY1       -0.3209
             5 H1          2.0168    0.1266    1.2568 H       1  GLY1        0.1187
             6 H2          1.8890   -0.4693   -0.2544 H       1  GLY1        0.1187
             7 C          -0.6775   -0.0767   -0.6578 C.2     1  GLY1        0.3185
             8 O          -0.4455   -0.6968   -1.6798 O.2     1  GLY1       -0.2496
             9 OXT        -1.7851    0.6991   -0.6705 O.3     1  GLY1       -0.4797
            10 HXT        -2.2101    0.6489   -1.5212 H       1  GLY1        0.2951
       @<TRIPOS>BOND
            1     1     2    1
            2     2     3    1
            3     2     4    1
            4     2     7    1
            5     4     5    1
            6     4     6    1
            7     7     8    2
            8     7     9    1
            9     9    10    1

-  ``MOLECULE_TO_MOLECULE(molecule)``

   ``MOLECULE_TO_MOLECULE`` converts a *molecule* in the old
   DEFAULT_TYPE format (InChI) to a molecule in the current
   DEFAULT_TYPE format (MDL Molfile). This function is useful when
   updating a database with a new version of Mychem.

   ::

       mysql> SELECT MOLECULE_TO_MOLECULE(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 
        OpenBabel11190809092D

         5  4  0  0  0  0  0  0  0  0999 V2000
           0.0000    0.0000    0.0000 C   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 C   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 N   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 O   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 O   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         1  3  1  0  0  0  0
         2  4  2  0  0  0  0
         2  5  1  0  0  0  0
       M  END

-  ``MOLECULE_TO_SERIALIZEDOBMOL(molecule)``

   ``MOLECULE_TO_SERIALIZEDOBMOL`` converts a *molecule* in
   DEFAULT_TYPE format to a serialized OBMol object. The SQL type of
   the serialized object is a binary string.

   ::

       mysql> SELECT MOLECULE_TO_SERIALIZEDOBMOL(molecule_col)
           -> FROM tbl_name WHERE name='2-Aminoacetic acid';
               -> binary string

-  ``MOLFILE_TO_MOLECULE(molecule)``

   ``MOLFILE_TO_MOLECULE`` converts a *molecule* in MDL Molfile
   (V2000) format to a molecule in DEFAULT_TYPE format.

   ::

       mysql> SELECT MOLFILE_TO_MOLECULE(molfile_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 2-Aminoacetic acid
        OpenBabel12091111263D

        10  9  0  0  0  0  0  0  0  0999 V2000
          -0.1068   -1.0521    1.1509 H   0  0  0  0  0  0  0  0  0  0  0  0
           0.0877   -0.0798    0.6477 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.2870    0.7082    1.3331 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.5185    0.1919    0.3951 N   0  0  0  0  0  0  0  0  0  0  0  0
           2.0168    0.1266    1.2568 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.8890   -0.4693   -0.2544 H   0  0  0  0  0  0  0  0  0  0  0  0
          -0.6775   -0.0767   -0.6578 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.4455   -0.6968   -1.6798 O   0  0  0  0  0  0  0  0  0  0  0  0
          -1.7851    0.6991   -0.6705 O   0  0  0  0  0  0  0  0  0  0  0  0
          -2.2101    0.6489   -1.5212 H   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         2  3  1  0  0  0  0
         2  4  1  0  0  0  0
         2  7  1  0  0  0  0
         4  5  1  0  0  0  0
         4  6  1  0  0  0  0
         7  8  2  0  0  0  0
         7  9  1  0  0  0  0
         9 10  1  0  0  0  0
       M  END

-  ``MOLECULE_TO_MOLFILE(molecule)``

   ``MOLECULE_TO_MOLFILE`` converts a *molecule* in DEFAULT_TYPE
   format to a molecule in MDL Molfile (V2000) format.

   ::

       mysql> SELECT MOLECULE_TO_MOLFILE(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 2-Aminoacetic acid
        OpenBabel11190809062D

        10  9  0  0  0  0  0  0  0  0999 V2000
          -0.1068   -1.0521    1.1509 H   0  0  0  0  0  0  0  0  0  0  0  0
           0.0877   -0.0798    0.6477 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.2870    0.7082    1.3331 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.5185    0.1919    0.3951 N   0  0  0  0  0  0  0  0  0  0  0  0
           2.0168    0.1266    1.2568 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.8890   -0.4693   -0.2544 H   0  0  0  0  0  0  0  0  0  0  0  0
          -0.6775   -0.0767   -0.6578 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.4455   -0.6968   -1.6798 O   0  0  0  0  0  0  0  0  0  0  0  0
          -1.7851    0.6991   -0.6705 O   0  0  0  0  0  0  0  0  0  0  0  0
          -2.2101    0.6489   -1.5212 H   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         2  3  1  0  0  0  0
         2  4  1  0  0  0  0
         2  7  1  0  0  0  0
         4  5  1  0  0  0  0
         4  6  1  0  0  0  0
         7  8  2  0  0  0  0
         7  9  1  0  0  0  0
         9 10  1  0  0  0  0
         5  4  0  0  0  0  0
       M  END

-  ``PDB_TO_MOLECULE(molecule)``

   ``PDB_TO_MOLECULE`` converts a *molecule* in PDB format to a
   molecule in DEFAULT_TYPE format.

   ::

       mysql> SELECT PDB_TO_MOLECULE(pdb_col) FROM tbl_name
           -> WHERE name='1CRN';
               ->
        OpenBabel08210907243D

       327337  0  0  0  0  0  0  0  0999 V2000
          17.0470   14.0990    3.6250 N   0  0  0  0  0
          16.9670   12.7840    4.3380 C   0  0  0  0  0
          15.6850   12.7550    5.1330 C   0  0  0  0  0
          15.2680   13.8250    5.5940 O   0  0  0  0  0
          18.1700   12.7030    5.3370 C   0  0  0  0  0
          19.3340   12.8290    4.4630 O   0  0  0  0  0
          18.1500   11.5460    6.3040 C   0  0  0  0  0
          15.1150   11.5550    5.2650 N   0  0  0  0  0
          13.8560   11.4690    6.0660 C   0  0  0  0  0
          14.1640   10.7850    7.3790 C   0  0  0  0  0
          14.9930    9.8620    7.4430 O   0  0  0  0  0
          12.7320   10.7110    5.2610 C   0  0  0  0  0
          13.3080    9.4390    4.9260 O   0  0  0  0  0
          12.4840   11.4420    3.8950 C   0  0  0  0  0
          13.4880   11.2410    8.4170 N   0  0  0  0  0
          13.6600   10.7070    9.7870 C   0  0  0  0  0
          12.2690   10.4310   10.3230 C   0  0  0  0  0
          11.3930   11.3080   10.1850 O   0  0  0  0  0
          14.3680   11.7480   10.6910 C   0  0  0  0  0
          15.8850   12.4260   10.0160 S   0  0  0  0  0
          12.0190    9.2720   10.9280 N   0  0  0  0  0
          10.6460    8.9910   11.4080 C   0  0  0  0  0
          10.6540    8.7930   12.9190 C   0  0  0  0  0
          11.6590    8.2960   13.4910 O   0  0  0  0  0
          10.0570    7.7520   10.6820 C   0  0  0  0  0
           9.8370    8.0180    8.9040 S   0  0  0  0  0
           9.5610    9.1080   13.5630 N   0  0  0  0  0
           9.4480    9.0340   15.0120 C   0  0  0  0  0
           9.2880    7.6700   15.6060 C   0  0  0  0  0
       ...

-  ``SMILES_TO_MOLECULE(molecule)``

   ``SMILES_TO_MOLECULE`` converts a *molecule* in SMILES format to a
   molecule in DEFAULT_TYPE format.

   ::

       mysql> SELECT SMILES_TO_MOLECULE(smiles_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 
        OpenBabel11190809142D

         5  4  0  0  0  0  0  0  0  0999 V2000
           0.0000    0.0000    0.0000 C   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 N   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 C   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 O   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 O   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         1  3  1  0  0  0  0
         3  4  2  0  0  0  0
         3  5  1  0  0  0  0
       M  END

-  ``MOLECULE_TO_SMILES(molecule)``

   ``MOLECULE_TO_SMILES`` converts a *molecule* in DEFAULT_TYPE format
   to molecule in SMILES format.

   ::

       mysql> SELECT MOLECULE_TO_SMILES(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> C(C(=O)O)N

   **Note**

   If you need a single canonical form for any particular molecule,
   regardless of atom order, the use of the
   ``MOLECULE_TO_CANONICAL_SMILES`` function should be preferred.

-  ``MOLECULE_TO_CANONICAL_SMILES(molecule)``

   ``MOLECULE_TO_CANONICAL_SMILES`` converts a *molecule* in
   DEFAULT_TYPE format to a molecule in Canonical SMILES format.

   ::

       mysql> SELECT MOLECULE_TO_CANONICAL_SMILES(molecule_col)
           -> FROM tbl_name WHERE name='2-Aminoacetic acid';
               -> NCC(=O)O

-  ``V3000_TO_MOLECULE(molecule)``

   ``V3000_TO_MOLECULE`` converts a *molecule* in MDL Molfile (V3000)
   format to a molecule in DEFAULT_TYPE format.

   ::

       mysql> SELECT V3000_TO_MOLECULE(V3000_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 2-Aminoacetic acid
        OpenBabel11190809082D

        10  9  0  0  0  0  0  0  0  0999 V2000
          -0.1068   -1.0521    1.1509 H   0  0  0  0  0  0  0  0  0  0  0  0
           0.0877   -0.0798    0.6477 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.2870    0.7082    1.3331 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.5185    0.1919    0.3951 N   0  0  0  0  0  0  0  0  0  0  0  0
           2.0168    0.1266    1.2568 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.8890   -0.4693   -0.2544 H   0  0  0  0  0  0  0  0  0  0  0  0
          -0.6775   -0.0767   -0.6578 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.4455   -0.6968   -1.6798 O   0  0  0  0  0  0  0  0  0  0  0  0
          -1.7851    0.6991   -0.6705 O   0  0  0  0  0  0  0  0  0  0  0  0
          -2.2101    0.6489   -1.5212 H   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         2  3  1  0  0  0  0
         2  4  1  0  0  0  0
         2  7  1  0  0  0  0
         4  5  1  0  0  0  0
         4  6  1  0  0  0  0
         7  8  2  0  0  0  0
         7  9  1  0  0  0  0
         9 10  1  0  0  0  0
       M  END

-  ``MOLECULE_TO_V3000(molecule)``

   ``MOLECULE_TO_V3000`` converts a *molecule* in DEFAULT_TYPE format
   to a molecule in MDL Molfile (V3000) format.

   ::

       mysql> SELECT MOLECULE_TO_V3000(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 2-Aminoacetic acid
        OpenBabel11190809132D
         0  0  0     0  0            999 V3000

       M  V30 BEGIN CTAB
       M  V30 COUNTS 10 9 0 0 0
       M  V30 BEGIN ATOM
       M  V30 1 H -0.1068 -1.0521 1.1509 0
       M  V30 2 C 0.0877 -0.0798 0.6477 0
       M  V30 3 H -0.287 0.7082 1.3331 0
       M  V30 4 N 1.5185 0.1919 0.3951 0
       M  V30 5 H 2.0168 0.1266 1.2568 0
       M  V30 6 H 1.889 -0.4693 -0.2544 0
       M  V30 7 C -0.6775 -0.0767 -0.6578 0
       M  V30 8 O -0.4455 -0.6968 -1.6798 0
       M  V30 9 O -1.7851 0.6991 -0.6705 0
       M  V30 10 H -2.2101 0.6489 -1.5212 0
       M  V30 END ATOM
       M  V30 BEGIN BOND
       M  V30 1 1 1 2
       M  V30 2 1 2 3
       M  V30 3 1 2 4
       M  V30 4 1 2 7
       M  V30 5 1 4 5
       M  V30 6 1 4 6
       M  V30 7 2 7 8
       M  V30 8 1 7 9
       M  V30 9 1 9 10
       M  V30 END BOND
       M  V30 END CTAB
       M  END

Helper Commands
---------------

This section details *helper* functions. They permit to get informations
about the Mychem environment.

-  ``INCHI_VERSION()``

   ``INCHI_VERSION`` returns the version of the InChI library.

   ::

       mysql> SELECT INCHI_VERSION();
               -> 1.02

-  ``MYCHEM_VERSION()``

   ``MYCHEM_VERSION`` returns the Mychem version.

   ::

       mysql> SELECT MYCHEM_VERSION();
               -> 1.0.0

-  ``OPENBABEL_VERSION()``

   ``OPENBABEL_VERSION`` returns the Open Babel version.

   ::

       mysql> SELECT OPENBABEL_VERSION();
               -> 2.3.2

Modification Commands
---------------------

This section describes *modification* functions that modify chemical
structures. The following functions are fully working starting on
version 0.6.0 of Mychem.

**Note**

If a function of this section fails, it returns an empty string.

-  ``ADD_HYDROGENS(molecule)``

   ``ADD_HYDROGENS`` adds hydrogens to a *molecule* (makes explicit
   the hydrogen atoms).

   ::

       mysql> SELECT ADD_HYDROGENS(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 2-Aminoacetic acid
        OpenBabel11190809242D

        10  9  0  0  0  0  0  0  0  0999 V2000
          -0.1068   -1.0521    1.1509 H   0  0  0  0  0  0  0  0  0  0  0  0
           0.0877   -0.0798    0.6477 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.2870    0.7082    1.3331 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.5185    0.1919    0.3951 N   0  0  0  0  0  0  0  0  0  0  0  0
           2.0168    0.1266    1.2568 H   0  0  0  0  0  0  0  0  0  0  0  0
           1.8890   -0.4693   -0.2544 H   0  0  0  0  0  0  0  0  0  0  0  0
          -0.6775   -0.0767   -0.6578 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.4455   -0.6968   -1.6798 O   0  0  0  0  0  0  0  0  0  0  0  0
          -1.7851    0.6991   -0.6705 O   0  0  0  0  0  0  0  0  0  0  0  0
          -2.2101    0.6489   -1.5212 H   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         2  3  1  0  0  0  0
         2  4  1  0  0  0  0
         2  7  1  0  0  0  0
         4  5  1  0  0  0  0
         4  6  1  0  0  0  0
         7  8  2  0  0  0  0
         7  9  1  0  0  0  0
         9 10  1  0  0  0  0
       M  END

-  ``REMOVE_HYDROGENS(molecule)``

   ``REMOVE_HYDROGENS`` removes the hydrogens from a *molecule* (makes
   implicit the hydrogen atoms).

   ::

       mysql> SELECT REMOVE_HYDROGENS(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               ->  2-Aminoacetic acid
        OpenBabel11190809252D

         5  4  0  0  0  0  0  0  0  0999 V2000
           0.0877   -0.0798    0.6477 C   0  0  0  0  0  0  0  0  0  0  0  0
           1.5185    0.1919    0.3951 N   0  0  0  0  0  0  0  0  0  0  0  0
          -0.6775   -0.0767   -0.6578 C   0  0  0  0  0  0  0  0  0  0  0  0
          -0.4455   -0.6968   -1.6798 O   0  0  0  0  0  0  0  0  0  0  0  0
          -1.7851    0.6991   -0.6705 O   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         1  3  1  0  0  0  0
         3  4  2  0  0  0  0
         3  5  1  0  0  0  0
       M  END

-  ``STRIP_SALTS(molecule)``

   ``STRIP_SALTS`` removes all atoms except for the larger contiguous
   fragment.

   ::

       mysql> SELECT STRIP_SALTS(molecule_col) FROM tbl_name
           -> WHERE name='sodium 2-aminoacetate';
               ->
        OpenBabel11190812342D

         5  4  0  0  0  0  0  0  0  0999 V2000
           0.0000    0.0000    0.0000 N   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 C   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 C   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 O   0  0  0  0  0  0  0  0  0  0  0  0
           0.0000    0.0000    0.0000 O   0  0  0  0  0  0  0  0  0  0  0  0
         1  2  1  0  0  0  0
         2  3  1  0  0  0  0
         3  4  2  0  0  0  0
         3  5  1  0  0  0  0
       M  CHG  1   5  -1
       M  END

Molmatch Commands
-----------------

The *molmatch* functions permit to compare chemical structures.

-  ``BIT_FP_AND(fingerprint, fingerprint)``

   ``BIT_FP_AND`` operates on two fingerprints (bit patterns) of equal
   length and performs the logical AND operation on each pair of
   corresponding bits. In each pair, the result is 1 if the both bits
   are 1. Otherwise, the result is 0. If the two fingerprints do not
   have the same length, the function returns NULL.

   ::

       mysql> SELECT BIT_FP_AND(fingerprint1, fingerprint2);
               -> binary fingerprint

   **Note**

   The ``BIT_FP_AND`` function is very useful when
   working with structure fingerprints. For example, if a molecule
   (with a fingerprint fp1) is a substructure of an other molecule
   (with a fingerprint fp2), the following property is observed:

   ::

       mysql> SELECT TANIMOTO(BIT_FP_AND(fp1, fp2), fp1);
               -> 1

-  ``BIT_FP_COUNT(fingerprint)``

   ``BIT_FP_COUNT`` returns the number of bits that are set in the
   *fingerprint* binary representation.

   ::

       mysql> SELECT BIT_FP_COUNT(fp_col) FROM tbl_name
           -> WHERE name='1H-indole';
               -> 23

-  ``BIT_FP_OR(fingerprint, fingerprint)``

   ``BIT_FP_OR`` operates on two fingerprints (bit patterns) of equal
   length and performs the logical OR operation on each pair of
   corresponding bits. In each pair, if the first bit is 1 or the second
   bit is 1 (or both), the result is 1. Otherwise, the result is 0. If
   the two fingerprints do not have same length, the function returns
   NULL.

   ::

       mysql> SELECT BIT_FP_OR(fingerprint1, fingerprint2);
               -> binary fingerprint

-  ``MATCH_SUBSTRUCT(query_smarts, reference_obmol)``

   ``MATCH_SUBSTRUCT`` checks if a *query_smarts* fragment is a
   substructure of a *reference_obmol* molecule. The first argument is
   a SMARTS string, whereas the second argument is a serialized OBMol
   object. The second argument type is generated by the
   ``MOLECULE_TO_SERIALIZEDOBMOL`` function. If the *query_smarts* is
   a substructure of *reference_obmol*, the function returns 1,
   otherwise, it returns 0.

   ::

       mysql> SELECT MATCH_SUBSTRUCT('C=O', serializedobmol_col)
           -> FROM tbl_name WHERE name='2-Aminoacetic acid';
               -> 1

   **Note**

   If the function encounters an error, it returns NULL.

-  ``SUBSTRUCT_ATOM_IDS(query_smarts, reference_obmol)``

   ``SUBSTRUCT_ATOM_IDS`` returns the atom ids of a *reference_obmol*
   molecule that are contained in substructures matching a
   *query_smarts* fragment. The first argument is a SMARTS string,
   whereas the second argument is a serialized OBMol object. The second
   argument type is generated by the ``MOLECULE_TO_SERIALIZEDOBMOL``
   function. If a *reference_obmol* molecule contains several fragments
   matching a *query_smarts* fragment, a list of items is returned.
   Each item contains a fragment's atom ids and is separated from the
   next item by a semicolon character.

   ::

       mysql> SELECT SUBSTRUCT_ATOM_IDS('C(=O)', serializedobmol_col)
           -> FROM tbl_name WHERE name='2-Aminoacetic acid';
               -> 2 3 ;

   **Note**

   If the function encounter an error, it returns NULL.

-  ``SUBSTRUCT_COUNT(query_smarts, reference_obmol)``

   ``SUBSTRUCT_COUNT`` returns the number of *query_smarts* fragments
   founded in a *reference_obmol* molecule. The first argument is a
   SMARTS string, whereas the second argument is a serialized OBMol
   object. The second argument type is generated by the
   ``MOLECULE_TO_SERIALIZEDOBMOL`` function.

   ::

       mysql> SELECT SUBSTRUCT_COUNT('C(=O)', serializedobmol_col)
           -> FROM tbl_name WHERE name='2-Aminoacetic acid';
               -> 2

   **Note**

   If the function encounter an error, it returns NULL.

-  ``TANIMOTO(first_fingerprint, second_fingerprint)``

   ``TANIMOTO`` returns the tanimoto coefficient between two
   fingerprints. Fingerprints are bit patterns and can be generated with
   the ``FINGERPRINT`` function. The returned value is
   comprised between 0 and 1. The higher the tanimoto coefficient is,
   the more the molecules are similar.

   ::

       mysql> SELECT TANIMOTO(molecule_fp, fp_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 0.8934

   **Note**

   The use of another Mychem functions (like ``FINGERPRINT`` or
   ``FINGERPRINT2``) within the ``TANIMOTO`` function makes the
   query slower. In  order to get the best performance, you should
   use the ``SET`` function of MySQL:

   ::

      mysql> SET @fp = (SELECT FINGERPRINT2(
          -> SMILES_TO_MOLECULE('C(C(=O)O)N')));
      mysql> SELECT id FROM tbl_name WHERE TANIMOTO(@fp, fp_col)
          -> FROM tbl_name > 0.7;
              -> list of id

Property Commands
-----------------

This section describes several functions that calculate molecular
properties.

-  ``EXACTMASS(molecule)``

   ``EXACTMASS`` returns the monoisotopic molecular weight of a
   *molecule*. The monoisotopic molecular weight is defined as the
   molecular weight calculated using the mass of the most abundant
   isotope for each element of a molecule. The unit of the returned
   value is g.mol\ :sup:`-1`.

   ::

       mysql> SELECT EXACTMASS(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 75.032028

-  ``IS_2D(molecule)``

   ``IS_2D`` returns 1 if a molecule has 2D coordinates.

   ::

       mysql> SELECT IS_2D(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 1

-  ``IS_3D(molecule)``

   ``IS_3D`` returns 1 if a molecule has 3D coordinates.

   ::

       mysql> SELECT IS_3D(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 1

-  ``IS_CHIRAL(molecule)``

   ``IS_CHIRAL`` returns 1 if a molecule is chiral.

   ::

       mysql> SELECT IS_CHIRAL(molecule_col) FROM tbl_name
           -> WHERE name='2S-Butan-2-ol';
               -> 1

-  ``MOLFORMULA(molecule)``

   ``MOLFORMULA`` returns the molecular formula of a *molecule*.

   ::

       mysql> SELECT MOLFORMULA(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> C2H5NO2

-  ``MOLLOGP(molecule)``

   ``MOLLOGP`` returns the LogP of a *molecule*.

   ::

       mysql> SELECT MOLLOGP(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> -0.27

   **Note**

   Note that the result of this function is depending on the hydrogen
   atoms. If there is any doubt on the presence of hydrogen atoms in
   the molecule, it is recommended to use the ``ADD_HYDROGENS``
   function.

-  ``MOLMR(molecule)``

   ``MOLMR`` returns the molar refractivity of a *molecule*. The unit
   of the returned value is J.mol\ :sup:`-1`.K\ :sup:`-1`.

   ::

       mysql> SELECT MOLMR(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 16.2072

   **Note**

   Note that the result of this function is depending on the
   hydrogen atoms. If there is any doubt on the presence of hydrogen
   atoms in the molecule, it is recommended to use the
   ``ADD_HYDROGENS`` function.

-  ``MOLPSA(molecule)``

   ``MOLPSA`` returns the topological polar surface area of a
   molecule. The unit of the returned value is Å\ :sup:`2`.

   ::

       mysql> SELECT MOLPSA(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 63.32

   **Note**

   Note that the result of this function is depending on the
   hydrogen atoms. If there is any doubt on the presence of hydrogen
   atoms in the molecule, it is recommended to use the
   ``ADD_HYDROGENS`` function.

-  ``MOLWEIGHT(molecule)``

   ``MOLWEIGHT`` returns the molecular weight of a *molecule*. The
   unit of the returned value is g.mol\ :sup:`-1`.

   ::

       mysql> SELECT MOLWEIGHT(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 75.066600

-  ``NUMBER_OF_ACCEPTORS(molecule)``

   ``NUMBER_OF_ACCEPTORS`` returns the number of hydrogen-bond
   acceptors in a *molecule*.

   ::

       mysql> SELECT NUMBER_OF_ACCEPTORS(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 3

   **Note**

   Note that the result of this function is depending on the
   hydrogen atoms. If there is any doubt on the presence of hydrogen
   atoms in the molecule, it is recommended to use the
   ``ADD_HYDROGENS`` function.

-  ``NUMBER_OF_ATOMS(molecule)``

   ``NUMBER_OF_ATOMS`` returns the number of atoms in a *molecule*.

   ::

       mysql> SELECT NUMBER_OF_ATOMS(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 10

-  ``NUMBER_OF_BONDS(molecule)``

   ``NUMBER_OF_BONDS`` returns the number of bonds in a *molecule*.

   ::

       mysql> SELECT NUMBER_OF_BONDS(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 9

-  ``NUMBER_OF_DONORS(molecule)``

   ``NUMBER_OF_DONORS`` returns the numbers of hydrogen-bond donors in
   a *molecule*.

   ::

       mysql> SELECT NUMBER_OF_DONORS(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 2

   **Note**

   Note that the result of this function is depending on the
   presence of hydrogen atoms in the molecule. If the hydrogen atoms
   are not described by the molecule, the ``ADD_HYDROGENS`` function
   must be used. The two following examples described the effect of
   the ADD_HYDROGENS function:

      ::

         mysql> SELECT NUMBER_OF_DONORS(
             -> SMILES_TO_MOLECULE('O=C(O)CCN'));
                 -> 0

      ::

         mysql> SELECT NUMBER_OF_DONORS(ADD_HYDROGENS(
             -> SMILES_TO_MOLECULE('O=C(O)CCN')));
                 -> 2

-  ``NUMBER_OF_HEAVY_ATOMS(molecule)``

   ``NUMBER_OF_HEAVY_ATOMS`` returns the number of heavy atoms in a
   *molecule* (all atoms except hydrogen).

   ::

       mysql> SELECT NUMBER_OF_HEAVY_ATOMS(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 5

-  ``NUMBER_OF_RINGS(molecule)``

   ``NUMBER_OF_RINGS`` returns the number of rings in a *molecule*.

   ::

       mysql> SELECT NUMBER_OF_RINGS(molecule_col) FROM tbl_name
           -> WHERE name='adenine';
               -> 2

-  ``NUMBER_OF_ROTABLE_BONDS(molecule)``

   ``NUMBER_OF_ROTABLE_BONDS`` returns the number of rotable bonds in
   a *molecule*.

   ::

       mysql> SELECT NUMBER_OF_ROTABLE_BONDS(molecule_col) FROM tbl_name
           -> WHERE name='2-Aminoacetic acid';
               -> 1

-  ``TOTAL_CHARGE(molecule)``

   ``TOTAL_CHARGE`` returns the total charge of a *molecule*.

   ::

       mysql> SELECT TOTAL_CHARGE(SMILES_TO_MOLECULE('NCC(=O)[O-]'));
               -> -1
