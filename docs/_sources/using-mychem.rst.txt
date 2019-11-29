Using Mychem
============

This chapter provides a short tutorial about the usage of Mychem. It
will present you a simple way to create a chemical database with Mychem
and how to use some functions. More details about each function used in
this tutorial are available in the :ref:`Command Reference`.

The Database
------------

A chemical database is composed of one or several tables. The examples
presented in this chapter are using a set of four tables, with the
following structure:

-  *compounds* - a table containing an unique id for each molecule and
   its name.

   .. code-block:: console

      +---------+------------------+------+-----+---------------------+----------------+
      | Field   | Type             | Null | Key | Default             | Extra          |
      +---------+------------------+------+-----+---------------------+----------------+
      | id      | int(11) unsigned | NO   | PRI | NULL                | auto_increment |
      | name    | varchar(255)     | NO   | MUL | NULL                |                |
      | created | timestamp        | NO   |     | 0000-00-00 00:00:00 |                |
      +---------+------------------+------+-----+---------------------+----------------+

-  *1D_structures* - a table containing an unique reference to the
   *compounds* table and several types of 1D molecular descriptors
   (InChI code and SMILES).

   .. code-block:: console

      +-------------+------------------+------+-----+---------+-------+
      | Field       | Type             | Null | Key | Default | Extra |
      +-------------+------------------+------+-----+---------+-------+
      | compound_id | int(11) unsigned | NO   | PRI | NULL    |       |
      | inchi       | text             | NO   |     | NULL    |       |
      | smiles      | text             | NO   |     | NULL    |       |
      +-------------+------------------+------+-----+---------+-------+

-  *3D_structures* - a table containing an unique reference to the
   *compounds* table and the 3D structure in MDL Molfile format.

   .. code-block:: console

      +-------------+------------------+------+-----+---------+-------+
      | Field       | Type             | Null | Key | Default | Extra |
      +-------------+------------------+------+-----+---------+-------+
      | compound_id | int(11) unsigned | NO   | PRI | NULL    |       |
      | molfile     | text             | NO   |     | NULL    |       |
      +-------------+------------------+------+-----+---------+-------+

-  *bin_structures* - a table containing an unique reference to the
   *compounds* table and several types of binary descriptors
   (fingerprints and serialized OBMol object).

   .. code-block:: console

      +--------------+------------------+------+-----+---------+-------+
      | Field        | Type             | Null | Key | Default | Extra |
      +--------------+------------------+------+-----+---------+-------+
      | compound_id  | int(11) unsigned | NO   | PRI | NULL    |       |
      | fp2          | blob             | YES  |     | NULL    |       |
      | obserialized | blob             | YES  |     | NULL    |       |
      +--------------+------------------+------+-----+---------+-------+


Such tables can be easily created and populated using
mychemdb_manager, a Python script released with Mychem. It can be found
in the ``scripts`` directory. Its usage is detailed in the :ref:`Chemical Database Manager`
chapter.

Examples
--------

Once the database and the tables are created, you can use many chemical
functions. Here is a (very) short overview of the possibilities. The
dataset used for this example can be freely obtained from the `Chemical Structures Project <http://chemfiles.sourceforge.net>`_.

Calculate the Molecular Weight
++++++++++++++++++++++++++++++

The computation of the molecular weight of a molecule is performed by
the ``MOLWEIGHT`` function. In the following example, the molecular
weight of the amino acid *glycine* is calculated.

::

    mysql> SELECT MOLWEIGHT(`3D_structures`.`molfile`)
        -> FROM `compounds`,`3D_structures`
        -> WHERE `compounds`.`name`='glycine'
        -> AND `compounds`.`id`=`3D_structures`.`compound_id`;
            -> 75.066600

Search a Substructure
+++++++++++++++++++++

The ``MATCH_SUBSTRUCTURE`` function permits to find a substructure and
belong to the :ref:`Molmatch Commands` function group. In this example,
we are looking for compounds containing a phenol group:

::

    mysql> SELECT `compounds`.`name` 
        -> FROM `compounds`,`bin_structures`
        -> WHERE `compounds`.`id`=`bin_structures`.`compound_id`
        -> AND MATCH_SUBSTRUCT('[OH]c1ccccc1',`obserialized`);
        -> LIMIT 10;
    +-----------------------------+
    | name                        |
    +-----------------------------+
    | 2,3,4,5,6-Pentachlorophenol |
    | 2,3-Dimethylphenol          |
    | 2,4,6-Trichlorophenol       |
    | 2,4-Dimethylphenol          |
    | 2,5-Dimethylphenol          |
    | 2,6-Dimethylphenol          |
    | 2-Bromophenol               |
    | 2-Chloro-5-methylphenol     |
    | 2-Chlorophenol              |
    | 2-Hydroxybenzaldehyde       |
    +-----------------------------+
    10 rows in set (0.02 sec)
