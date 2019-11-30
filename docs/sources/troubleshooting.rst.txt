Troubleshooting
===============

When running Mychem, you may encounter certain errors that prevent the
Mychem software to run perfectly. The purpose of this chapter is to help
you to diagnose and correct some of these errors.

MySQL-Related Errors
--------------------

This section describes the errors encounter with MySQL when running
Mychem.

ERROR 2013 (HY000): Lost connection to MySQL server during query
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The following examples show an error messages you may encounter when
using the SMILES_TO_MOLECULE function.

::

    mysql> SELECT SMILES_TO_MOLECULE('CCOCC');
    ERROR 2013 (HY000): Lost connection to MySQL server during query

This problem has been observed with version of Mychem earlier to 0.6.0.
To avoid this error, set the thread_stack parameter of MySQL to 192K.
The thread_stack parameter is defined in the MySQL server configuration
file.

AppArmor
--------

AppArmor is a Linux Security Module implementation of name-based access
controls. AppArmor confines individual programs to a set of listed
files. The default configuration of AppArmor does not permit the use of
Mychem. In fact, MySQL is not allowed to access the Open Babel library.
To fix this problem, the following lines must be added to the
``/etc/apparmor.d/local/usr.sbin.mysqld`` file:

::

    /usr/lib/openbabel/* m,
    /usr/lib/openbabel/2.3.2/* m,
    /usr/share/openbabel/* r,
    /usr/share/openbabel/2.3.2/* r,

    **Note**

    Replace 2.3.2 by the version of OpenBabel on your system (2.3.0,
    2.3.1, ...).

Other Errors
------------

If you encounter an error not listed here, please report it on our `bug
tracking system <https://github.com/mychem/mychem-code/issues>`_.
