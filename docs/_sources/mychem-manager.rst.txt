Chemical Database Manager
=========================

This chapter presents the **mychemdb_manager** tool, a chemical database manager
for handling chemical databases with MySQL and Mychem.

mychemdb_manager
----------------

The Mychem software is useful when working with chemical databases. In
order to facilitate the creation and the management of such databases,
``mychemdb_manager``, a Python program, is distributed with the Mychem
code. This script is a command line interface that permits to create
or update a chemical database.

The ``mychemdb_manager`` script can be found in the ``scripts`` directory from
Mychem. It is a Python program released under the new BSD license. It
requires the **pymysql** Python module. This module is provided by most
GNU/Linux distributions. It can also be installed using ``pip``.

The usage of the script is simple. A help can be displayed by using the
``-h`` option:

.. code-block:: console

   usage: mychem_manager [-h] [-v] -H HOST -U USER -D DATABASE [-P] [-n NAME_TAG]
                         [-l LOG_FILE] [-p PREFIX] [-a | -r] [-V]
                         sdfile
   
   mychem_manager load a file in MDL SDF format into a MySQL database and creates
   a chemical cartridge with Mychem.
   
   positional arguments:
     sdfile                Name of the MDL SDF file containing the chemical-data
                           to load into the MySQL database.
   
   optional arguments:
     -h, --help            show this help message and exit
     -v, --version         show program's version number and exit
     -H HOST, --host HOST  Name of the MySQL host.
     -U USER, --user USER  User for login to the MySQL server.
     -D DATABASE, --db DATABASE
                           Name of the MySQL database to use
     -P, --password        Specify if a password is required to connect to MySQL.
     -n NAME_TAG, --nametag NAME_TAG
                           Name of the tag used in the MDL SDF file to define the
                           name of the chemical compound.
     -l LOG_FILE, --logfile LOG_FILE
                           Name of the log file to send logging output to.
     -p PREFIX, --prefix PREFIX
                           Prefix added to the default table names.
     -a, --append          Specify if the data should be added to the existing
                           mychem tables.
     -r, --replace         Specify if the new data should replace existing data.
     -V, --verbose         Enable verbose debug messages.


The first step is to create a database for storing the chemical tables.
In this documentation, the database will be named *mychem*, but any other name
can be used (the ``-D`` option permit to set the database name).

::

    --
    -- Database creation
    --

    CREATE DATABASE `mychem`;

Once the database is created, it is possible to load the MDL SDF file
with the ``mychemdb_manager`` script:

.. code-block:: bash

    $ python mychemdb_manager -D mychem -H localhost -U user -P database.sdf
    Enter MySQL password:
    The MDL SDFFile has been successfully loaded.

The previous command will create the following tables:

-  *mychem_compounds* - It contains the compound's name and two timestamps
   (when the entry is created and when the entry is updated).

-  *mychem_1D_structures* - It contains the 1D representation of the
   compounds (SMILES and InChI code).

-  *mychem_3D_structures* - It contains the 3D structure of the compounds in
   MDL Molfile format.

-  *mychem_bin_structures* - It contains the binary representation (fp2 and
   obserialized object) of the compounds.

    **Note**

    It is possible to use another prefix than *mychem* by using
    the ``-t`` option.

If all these tables are already existing, you have to choose either to
append data (``-a`` option) or to replace existing data (``-r`` option).

