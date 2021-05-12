Introduction
============

About this document
-------------------

This document describes the use of the `iRODS <https://irods.org>`_
client with the `iRODS-based service
<http://www.france-grilles.fr/catalogue-de-services/fg-irods/>`_
(FG-iRODS) provided by France Grilles. It is based on the online
documentation written by Catherine Biscarat, Pierre Gay and Jérôme
Pansanel.

The last version of this document is made available on:
https://github.com/FranceGrilles/user-docs/tree/main/irods-en.


| Copyright (c) 2021 CNRS, University of Bordeaux and University of Strasbourg.
| This document is licensed under a `Creative Commons Attribution 4.0 International license <https://creativecommons.org/licenses/by/4.0/>`_.


The FG-iRODS service
--------------------

The FG-iRODS service, provided by the `France Grilles <http://france-grilles.fr>`_
French Research Infrastructure, aims to provide a convenient way to manage
research data. It is composed of the following elements:

* a highly available storage infrastructure which permit to distribute the
  data geographically ;

* the iRODS software ;

* a dedicated user support ;

* a set of tools to make the solution simple to use

The access to the service is nominative and realised through a simple
application sent to `info@france-frilles.fr <mailto:info@france-grilles.fr>`_.
Once the application is processed, and before the account creation, the
following additionnal information are requested:

* to sign the policies of the service: http://www.france-grilles.fr/IMG/pdf/iRODS_Service_Policy.pdf;

* to provide a light data management plan (DMP). Several examples of DMP
  are made available by DMP OPIDoR `<https://dmp.opidor.fr/>`_.

At the end of the registration procedure, the access to the service is open
to the requester and the documentation of the service is sent to the users.


Client Installation
===================

Prerequisite
------------

The user needs an access to a computer with a GNU/Linux operating system,
like CentOS 7 or Ubuntu (16.04, 18.04). He needs also to have the right
to install new softwares on this system.


Environment
-----------

The iRODS CLI client use a file for storing most of the configuration
details. This file is ``~/irods/irods_environment.json`` and need to
be created with the following content:

.. code-block:: console

   {
       "irods_host": "ccirodsfg.in2p3.fr",
       "irods_port": 5555,
       "irods_zone_name": "FranceGrillesZone",
       "irods_user_name": "<username>",
       "irods_default_resource": "<default_resource>",
       "irods_client_server_negotiation": "request_server_negotiation",
       "irods_client_server_policy": "CS_NEG_REQUIRE",
       "irods_default_hash_scheme": "SHA256",
       "irods_default_number_of_transfer_threads": 4,
       "irods_encryption_algorithm": "AES-256-CBC",
       "irods_encryption_key_size": 32,
       "irods_encryption_num_hash_rounds": 16,
       "irods_encryption_salt_size": 8,
       "irods_match_hash_policy": "compatible",
       "irods_maximum_size_for_single_buffer_in_megabytes": 32,
       "irods_ssl_verify_server": "cert"
   }

The *<username>* and *<default_resource>* should be replaced by the values
given to you during the registration procedure.


Package Installation
--------------------

The client packages are made available for CentOS 7, Ubuntu 16.04 and Ubuntu 18.04.

Instructions for configuring your package manager to include the iRODS *APT* or
*YUM*-based repository can be found at https://packages.irods.org.

Once the repository has been configured, install the *irods-icommands* package.

Check that your client installation is working with:

.. code-block:: shell-session

   ~$ iinit

The **iinit** permits to open a session to the iRODS instance. This
command will ask you for the connection password. Once you have
finished working with FG-iRODS, you can close the session with
**iexit**.

The **ienv** command display your iRODS environment:

.. code-block:: console

   irods_version - 4.2.8
   irods_client_server_negotiation - request_server_negotiation
   irods_encryption_key_size - 32
   irods_environment_file - /home/<user>/.irods/irods_environment.json
   irods_default_hash_scheme - SHA256
   irods_default_number_of_transfer_threads - 4
   irods_host - ccirodsfg.in2p3.fr
   irods_client_server_policy - CS_NEG_REQUIRE
   irods_session_environment_file - /home/<user>/.irods/irods_environment.json.15934
   irods_default_resource - <default_resource>
   irods_encryption_algorithm - AES-256-CBC
   irods_encryption_num_hash_rounds - 16
   irods_encryption_salt_size - 8
   irods_match_hash_policy - compatible
   irods_ssl_verify_server - cert
   irods_maximum_size_for_single_buffer_in_megabytes - 32
   irods_port - 5555
   irods_user_name - <username>
   irods_zone_name - FranceGrillesZone


Using the FG-iRODS service
==========================

Interactive Help
----------------

**ihelp** permits to display the list of iRODS commands, as well as the
help on a specific command:

.. code-block:: shell-session

   $ ihelp ils
   Usage: ils [-ArlLv] dataObj|collection ...
   Usage: ils --bundle [-r] dataObj|collection ...
   Display data Objects and collections stored in irods.
   Options are:
    -A  ACL (access control list) and inheritance format
    -l  long format
    -L  very long format
    -r  recursive - show subcollections
    -t  ticket - use a read (or write) ticket to access collection information
    -v  verbose
    -V  Very verbose
    -h  this help
    --bundle - list the subfiles in the bundle file (usually stored in the
        /myZone/bundle collection) created by iphybun command.

   iRODS Version 4.2.8                ils

A full description of the icommands is available in the
`iRODS documentation <https://docs.irods.org/4.2.8/icommands/user/>`_.


The Working Directory
---------------------

The **ils** command permits you to display the content of the current
working directory. By default, the working directory is your user
directory :

.. code-block:: shell-session

   ~$ ils
   /FranceGrillesZone/home/<username>:

* *FranceGrillesZone*: the name of the iRODS zone

* */home/<username>*: your default working directory

It is possible to modify the default directory used when you connect
by adding the following lines to your iRODS configuration file :

.. code-block:: console

   "irods_cwd": "<repository_path>",
   "irods_home": "<repository_path>",

The value of *<repository_path>* needs to be replaced by the desired path.


Uploading Data
--------------

In this section, some files will be uploaded to FG-iRODS. The file used
in the following examples is ``foo.bin``. It can be replaced by a file
of your choice. If you want to work with the ``foo.bin`` file, you can
create it with the following command:

.. code-block:: shell-session

   $ dd if=/dev/urandom of=foo.bin count=65536

The file is uploaded to the iRODS infrastructure with:

.. code-block:: shell-session

   $ iput -K foo.bin

The *-K* option permits to verify the checksum and to store it in the
iRODS database. It is recommanded to always use this option. The file is
now available on FG-iRODS:

.. code-block:: shell-session

   $ ils
   /FranceGrillesZone/home/<username>:
     foo.bin

**Note:** the commands to steer iRODS are very similar to bash commands
and can easily be confused!

The file can be deleted with this command:

.. code-block:: shell-session

   $ irm foo.bin


Logical and Physical Namespace
------------------------------

iRODS provides an abstraction from the physical location of the files,
e.g. ``/FranceGrillesZone/home/<username>/foo.bin`` is the logical path
which only iRODS knows. To get more details about the physical namespace,
use the **-L** option with the **ils** command:

.. code-block:: shell-session

   $ ils -L
   /FranceGrillesZone/home/<username>:
     <username>         0 mcia;mcia-fgirods1     33554432 2020-11-20.09:30 & foo.bin
       sha2:veVzp+ApMzyVRzZN0BZIkDyFuqUp/4tM4sLVACp00B8=    generic    /vault1/resc/home/<username>/foo.bin

The result of this command indicate us that:

  * file ``foo.bin``  is registered by FG-iRODS as
    ``/FranceGrillesZone/home/<username>/foo.bin``;

  * its owner is *<username>*;

  * it lies on the storage resource *mcia*;

  * there is only a single replica, with the id *0*;

  * the file has a size of 33554432 bytes;

  * its checksum has been registered
    (*sha:veVzp+ApMzyVRzZN0BZIkDyFuqUp/4tM4sLVACp00B8=*);


Downloading Data
----------------

The file stored in FG-iRODS can be downloaded with:

.. code-block:: shell-session

   $ iget -K foo.bin foo-restore.txt


The ``foo.bin`` file has been downloaded and renamed to ``foo-restore.txt``.
With the *-K* option, the checksum of the local file is compared with
the checksum of the file on the iRODS server.


Structuring Data
----------------

Creating Collections
++++++++++++++++++++

On your computer, data are organised in folders. In iRODS, you will
organising them the same way. However, folders are called *collections*.

To create an iRODS collection:

.. code-block:: shell-session

   $ imkdir mycollection

The ``foo.bin`` file can be moved to that collection with:

.. code-block:: shell-session

   $ imv foo.bin mycollection
   $ ils -L mycollection
   /FranceGrillesZone/home/<username>/mycollection:
     <username>         0 mcia;mcia-fgirods1     33554432 2020-11-20.10:18 & foo.bin
       sha2:veVzp+ApMzyVRzZN0BZIkDyFuqUp/4tM4sLVACp00B8=    generic    /vault1/resc/home/<username>/mycollection/foo.bin

You see that the logical iRODS collection
``/FranceGrillesZone/home/<username>/mycollection`` has the physical
counterpart ``/vault1/resc/home/<username>/mycollection``.
So data does not end up on the iRODS server randomly but follows the
structure.

Data can also be put directly into an iRODS collection:

.. code-block:: shell-session

   $ iput -K -r bar.txt mycollection
   $ ils  /FranceGrillesZone/home/<username>/mycollection
   /FranceGrillesZone/home/<username>/mycollection:
     bar.txt
     foo.bin


The *-r* flag can be used for recursive upload.


Navigating through Collections
++++++++++++++++++++++++++++++

The current working directory corresponds to the place where you are
working in the folder hierarchy of iRODS. To display your current
iRODS working directory, use:

.. code-block:: shell-session

   $ ipwd
   /FranceGrillesZone/home/<username>

If you do not specify a full path, but only a partial path like
``mycollection/<file>``, iRODS automatically uses the current working
directory as a prefix. You can move along the folder hierarchy and
modify your current working directory with the **icd** command:

.. code-block:: shell-session

   $ icd mycollection


Managing Metadata
-----------------

To access the full potential of iRODS, it is required to use metadata.

Creating Metadata
+++++++++++++++++

Each file can be annoted with *Attribute*, *Value*, *Unit* triples (AVU).
These triples are added to the iRODS database (iCAT) and are searchable.
Metadata can be added to a file with:

.. code-block:: shell-session

   $ imeta add -d foo.bin 'length' '20' 'words'


The *Unit* field can be empty:

.. code-block:: shell-session

   $ imeta add -d foo.bin 'project' 'example'

Metadata can also be added to a collection:

.. code-block:: shell-session

   $ imeta add -C mycollection 'author' 'John Smith'


Listing Metadata
++++++++++++++++

To list metadata on data objects (files), do:

.. code-block:: shell-session

   $ imeta ls -d foo.bin
   AVUs defined for dataObj /FranceGrillesZone/home/<username>/mycollection/foo.bin:
   attribute: length
   value: 20
   units: words

and the following on collections:

.. code-block:: shell-session

   ~$ imeta ls -C mycollection
   AVUs defined for collection /FranceGrillesZone/home/<username>/mycollection:
   attribute: author
   value: John Smith
   units:


Querying Metadata
+++++++++++++++++

To query the iCAT metadata catalogue, use the following command:

.. code-block:: shell-session

   $ imeta qu -d 'length' = '20'
   collection: /FranceGrillesZone/home/<username>/mycollection
   dataObj: foo.bin

Advanced search
+++++++++++++++

In order to perform a more accurate selection of files or collections,
it is possible to directly interact with the iCAT catalogue with the
**iquest** command:

.. code-block:: shell-session

   $ iquest "select COLL_NAME, META_COLL_ATTR_VALUE where META_COLL_ATTR_NAME like 'author'"
   COLL_NAME = /FranceGrillesZone/home/<username>/mycollection
   META_COLL_ATTR_VALUE = John Smith
   ------------------------------------------------------------

The results can be filtered with one or more attributes:

.. code-block:: shell-session

   $ iquest "select COLL_NAME, META_COLL_ATTR_VALUE where META_COLL_ATTR_NAME like 'author' \
   and META_COLL_ATTR_VALUE like 'John%'"
   COLL_NAME = /FranceGrillesZone/home/<username>/mycollection
   META_COLL_ATTR_VALUE = John Smith
   ------------------------------------------------------------

**NOTE**: the '%' symbol is a wildcard.

If you are looking for a data object rather than a collection, replace
the *META_COLL_ATTR_NAME* attribute with *META_DATA_ATTR_NAME*. There
are a lot of predefined attributes that can be used in your searches.
To display the list of these attributes, use:

.. code-block:: shell-session

   $ iquest attrs


Access Control
--------------

iRODS has similar Access Control Lists (ACL) as a unix file system,
with read, write and own rights. The current access rights of your data
can be checked with:

  .. code-block:: shell-session

   $ ils -r -A
   /FranceGrillesZone/home/<username>/mycollection:
           ACL - jpansanel#FranceGrillesZone:own
           Inheritance - Disabled
     bar.txt
           ACL - <username>#FranceGrillesZone:own
     foo.bin
           ACL - <username>#FranceGrillesZone:own


The access rights are specified after the *ACL* keyword. In this example,
*<username>* owns all files listed. No one else can access the files.

Collections have a *Inheritance* attribut. If this value of this attribut
is set to *Enabled*, all content of the collection will inherit the access
rights from the collection. The inheritance applies only to newly uploaded files.

To give read access to a file to a colleague, use the following command:

.. code-block:: shell-session


   $ ichmod read <colleague> foo.bin

The user *<colleague>* can now access the ``foo.bin`` file, but it will not
be able to modify or delete it.

