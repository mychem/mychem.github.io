Installation
============

In this chapter, we will discuss the steps necessary to compile and
install Mychem.

-  :ref:`How to Obtain Mychem` - mainly concentrates on
   downloading the last stable version of Mychem.

-  :ref:`Requirements` - lists the programs and libraries
   which you need installed to successfully compile Mychem.

-  :ref:`Compilation and Installation` - leads you through
   all the steps of compilation and installation of the application.

-  :ref:`Mychem API` - tells how to build the API documentation of
   Mychem.


Requirements
------------

In order to successfully compile and install Mychem, you need the
following programs and libraries.

-  C/C++ compiler

   The compilation has been tested successfully with the GNU C Compiler
   (GCC).

-  CMake version 3.10.0 or higher.

   CMake is a multi-platform Makefile generator. It is a Free Software
   and can be downloaded on the `CMake website <https://cmake.org/>`_.

-  MySQL or MariaDB version 5.5 or higher.

   The MySQL database server is available from the `MySQL website <https://www.mysql.com/>`_.

-  Open Babel version 3.0 or higher

   The software is available from the `Open Babel website <https://openbabel.org/>`_.


How to Obtain Mychem
--------------------

The stable versions of Mychem are released as a tarball that has to be
extracted and compiled. The code can also be directly retrieven by
cloning the mychem-code GIT repository. The following sections
describe how to proceed.


Source Archive
++++++++++++++

The last stable version can be found on the `Mychem GitHub release page
<https://github.com/mychem/mychem-code/releases>`_. It is provided as
ZIP and compressed TAR archives. The last stable version is v2.0.0.


GIT Repository
++++++++++++++

The last development version of Mychem can be obtained by cloning the
`mychem-code GIT repository <http://github.com/mychem/mychem-code>`_. In case you
are using a command line interface, follow this step:

.. code-block:: bash

    $ git clone https://github.com/mychem/mychem-code.git


Compilation and Installation
----------------------------

This section describes the compilation of Mychem's source code for
GNU/Linux and Mac OS X.


Compiling and Installing Mychem on GNU/Linux
++++++++++++++++++++++++++++++++++++++++++++

The compilation and installation instructions of the Mychem software
on GNU/Linux is detailed in the two following parts. The default
is to use MariaDB server, but Mychem has been successfully tested
with MySQL.


Prerequisites
~~~~~~~~~~~~~

The following devlopment packages are required for a successful
compilation on GNU/Linux:

* with Ubuntu or Debian: libopenbabel-dev, libmariadb-dev, libmariadbclient-dev

* with RedHat and derivatives:
  sudo yum install -y epel-release
  sudo yum update
  sudo yum install -y openbabel-devel mariadb-devel

Each release is automatically tested on the following operating systems
using gitare automatically tested via
`GitHub Actions <https://github.com/features/actions>`_:

* Ubuntu 20.04

* Ubuntu 22.04

* Ubuntu 24.04


Standard Installation on GNU/Linux
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This part describes the standard way to compile and install Mychem on
GNU/Linux. You have to check that the header files for the MySQL library
and the Open Babel library are installed on your workstation. If they
are not installed, the CMake software will raise an error when
generating the Makefile. First, extract the appropriate source package.
If you are using a command line interface, follow this instructions:

-  For the tar gzipped archive:

   .. code-block:: bash

       $ tar -xfzv mychem-2.0.0.tar.gz

-  For the zip archive:

   .. code-block:: bash

       $ unzip mychem-2.0.0.zip

CMake can build the libraries and executables into any directory. If the
directory contains the source, the build is called *in source*. In other
cases, it is called *out of source*. CMake strongly recommends and
promotes building out of source.

-  In source build:

   .. code-block:: bash


       $ cd mychem-2.0.0
       $ cmake .
       $ make
       $ sudo make install

-  Build out-of-source (recommended):

   .. code-block:: bash

       $ cd mychem-2.0.0
       $ mkdir build
       $ cd build
       $ cmake ..
       $ make
       $ sudo make install
       $ cd ..

   **Note**

   The default installation directory can be retrieven with the
   following command:

   .. code-block:: bash

       $ mysql_config --plugindir
       /usr/lib/x86_64-linux-gnu/mariadb18/plugin

Once the library is installed, the SQL functions are created with the
following command:

.. code-block:: bash

    $ mysql -u user -p < src/mychemdb.sql


**Note**

On Mac OS X and Windows, another SQL file is used instead of the
``src/mychemdb.sql`` file. The name of this file is detailed in the
corresponding OS section.


Customized Installation
~~~~~~~~~~~~~~~~~~~~~~~

You can customized the build and installation process by modifying CMake
arguments. For example, if you want to change the path of the
installation directory:

.. code-block:: bash

    $ cd /path/to/mychem/build
    $ cmake -DCMAKE_INSTALL_PREFIX=/convenient/path ..

If you want to have more details about the compilation process, use the
following option for the ``make`` command:

.. code-block:: bash

    $ make VERBOSE=1


Ubuntu specifities
~~~~~~~~~~~~~~~~~~

AppArmor is a Linux Security Module and is installed by default on
Ubuntu. It permits to confine individual programs to a set of listed
files. To configure correctly AppArmor for Mychem, please follow the
instructions detailed in the :ref:`AppArmor` section.


Testing the installation
~~~~~~~~~~~~~~~~~~~~~~~~

Since v0.5, Mychem includes a test suite. To build and use these
programs, you have to set the MySQL connection settings and run the
tests. The following example builds Mychem out-of-source and enables
testing.

.. code-block:: bash

    $ cd mychem-2.0.0
    $ mkdir build
    $ cd build
    $ cmake -DMY_HOST=localhost -DMY_USER=user -DMY_PASSWD=passwd ..
    $ make
    $ make install
    $ cd ..

When running the command:

.. code-block:: bash

   $ cmake -DMY_HOST=localhost -DMY_USER=user -DMY_PASSWD=passwd ..

You will see the following line in the status message:

::

    -- Test module enabled

The program use the *mysql* database for testing. If the *user* do not
have access to this database, it is possible to set the name of the
database by using the -DMY_DB option.

    **Note**

    If the user can access MySQL without a password, then you do not
    need to set the ``MY_PASSWD`` parameter. The two other parameters (
    ``MY_HOST`` and ``MY_USER``) are mandatory.

    **Note**

    The password is not stored in a safe location. If you are doing
    tests on a critical server, please use directly the test executables
    and do not use the CMake facility to perform the tests ! You can
    find the test executables in the ``/path/to/mychem/build/tests``
    directory.

To run the tests, use the command ``make test``. You should see the
following results:

::

    Running tests...
    Test project mychem-code/build
        Start 1: ConversionTest
    1/5 Test #1: ConversionTest ...................   Passed    0.05 sec
        Start 2: HelperTest
    2/5 Test #2: HelperTest .......................   Passed    0.00 sec
        Start 3: ModificationTest
    3/5 Test #3: ModificationTest .................   Passed    0.01 sec
        Start 4: MolmatchTest
    4/5 Test #4: MolmatchTest .....................   Passed    0.04 sec
        Start 5: PropertyTest
    5/5 Test #5: PropertyTest .....................   Passed    0.03 sec

    100% tests passed, 0 tests failed out of 5

    Total Test time (real) =   0.14 sec

The ``LastTest.log`` file contains more details about the test results.
It can be found in the ``/path/to/mychem/build/Testing/Temporary``
directory.


Installation Troubleshooting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Building your application can raise some errors:

-  If CMake returns the following error:

   ::

       CMake Error: This project requires some variables
       to be set, and cmake can not find them. Please set the following
       variables: OPENBABEL2_INCLUDE_DIR (ADVANCED) OPENBABEL2_LIBRARIES
       (ADVANCED)

   It means that CMake did not find Open Babel. If you know where Open
   Babel is installed on your system, you can tell it to CMake with:

   .. code-block:: bash

       $ cd /path/to/mychem-2.0.0/build
       $ cmake -DOPENBABEL2_INCLUDE_DIR=/path/to/openbabel/include \
       -DOPENBABEL2_LIBRARIES=/path/to/library ..


Installing Mychem on Mac OS X
+++++++++++++++++++++++++++++

The installation on Mac OS X differs slightly from an installation on
GNU/linux. First, you have to set some parameters for CMake. The
following list shows common values used when compiling Mychem on Mac OS
X. Note that these values may differ on your system.

+------------------------+---------------------------------------------------------------+
| CMake Variable         | Value                                                         |
+========================+===============================================================+
| OPENBABEL2_INCLUDE_DIR | ``/usr/local/include/openbabel-2.0``                          |
+------------------------+---------------------------------------------------------------+
| OPENBABEL2_LIBRARIES   | ``/usr/local/lib/libopenbabel.dylib``                         |
+------------------------+---------------------------------------------------------------+
| MYSQL_INCLUDE_DIR      | ``/Library/MySQL/include/mysql``                              |
+                        +---------------------------------------------------------------+
|                        | ``/Applications/MAMP/Library/include/mysql``                  |
+------------------------+---------------------------------------------------------------+
| MYSQL_LIBRARIES        | ``/Library/MySQL/lib/mysql/libmysqlclient.dylib``             |
+                        +---------------------------------------------------------------+
|                        | ``/Applications/MAMP/Library/lib/mysql/libmysqlclient.dylib`` |
+------------------------+---------------------------------------------------------------+

Once the library is installed, the SQL functions are created with the
following command:

.. code-block:: bash

    $ mysql -u user -p < src/mychemdb_macosx.sql

When executing the previous command, you can have the following problem:

.. code-block:: bash

    $ mysql -u root -p < src/mychemdb_macosx.sql
    Enter password:
      ERROR 1126 (HY000) at line 10: Cannot open shared library 'libmychem.dylib'
      (errno: 2 dlopen(/usr/local/mysql/lib/plugin/libmychem.dylib, 2): Library
      not loaded: libmysqlclient.18.dylib
      Referenced from: /usr/local/m)

This problem can be fixed by modifying the libmychem.dylib shared
library so that all dependent libraries contain the correct path
information:

.. code-block:: bash

    $ otool -L libmychem.dylib
    libmychem.dylib:
            /usr/local/lib/libmychem.0.dylib (compatibility version 0.0.0, current
            version 2.0.0)
            /usr/local/lib/libopenbabel.4.dylib (compatibility version 4.0.0,
            current version 4.0.1)
            libmysqlclient.18.dylib (compatibility version 18.0.0, current version
            18.0.0)
            /usr/lib/libSystem.B.dylib (compatibility version 2.0.0, current
            version 159.1.0)
            /usr/lib/libstdc++.6.dylib (compatibility version 7.0.0, current
            version 52.0.0)
    $ sudo find / -name 'libmysqlclient.18.dylib' -print
    Password:
    /usr/local/mysql-5.5.24-osx10.6-x86_64/lib/libmysqlclient.18.dylib
    $ sudo install_name_tool -change libmysqlclient.18.dylib \
     /usr/local/mysql-5.5.24-osx10.6-x86_64/lib/libmysqlclient.18.dylib \
     libmychem.dylib


Mychem API
----------

API documentation is available on the `Mychem
website <http://mychem.github.io/api/index.html>`_.

You can also build the documentation yourself, by using the `Doxygen
software <http://www.doxygen.org/>`_. To generate this documentation,
use the following commands:

.. code-block:: bash

    $ cd /path/to/mychem-2.0.0
    $ mkdir api
    $ doxygen Doxyfile

The API documentation can be read using a web browser at the following
url: ``file:///path/to/mychem-2.0.0/api/html/index.html``

