Introduction
============

À propos de ce document
-----------------------

Ce document présente l'utilisation du client `iRODS <https://irods.org>`_
avec le `service iRODS
<http://www.france-grilles.fr/catalogue-de-services/fg-irods/>`_
(FG-iRODS) proposé par France Grilles. Il est basé sur le contenu
de la documentation en ligne du service FG-iRODS rédigée par Catherine
Biscarat, Pierre Gay et Jérôme Pansanel et complété par de la documentation rédigé par Patrick Moreau et Vincent Nègre pour le projet PHENOME. 

La dernière version de ce document est disponible sur :
https://github.com/FranceGrilles/user-docs/tree/main/irods-fr.

| Copyright (c) 2021 CNRS, Université de Bordeaux et Université de Strasbourg.
| Ce document est distribué sous la licence `Creative Commons Attribution 4.0 International license <https://creativecommons.org/licenses/by/4.0/>`_.


Le service FG-iRODS
-------------------

Le service FG-iRODS, proposé par l'infrastructure de recherche
`France Grilles <http://france-grilles.fr>`_ a pour objectif
de faciliter la gestion des données de recherche. Il repose sur :

* une infrastructure de stockage géographiquement distribuée
  de niveau *production* hautement disponible ;

* l'utilisation du logiciel iRODS ;

* un accompagnement personnalisé des utilisateurs ;

* un ensemble d'outils permettant d'en simplifier l'usage.

L'accès à ce service est nominatif et réalisé par une simple demande
adressée  à `info@france-frilles.fr <mailto:info@france-grilles.fr>`_.
Après étude de la demande et avant création des comptes, il est demandé
aux utilisateurs :

* de signer les conditions d'accès au service FG-iRODS : http://www.france-grilles.fr/IMG/pdf/iRODS_Service_Policy.pdf ;

* de fournir un plan de gestion de données (PGD). Plusieurs exemples de PGD
  sont disponibles sur le site de DMP OPIDoR `<https://dmp.opidor.fr/>`_ .

Une fois les documents reçus, l'accès au service est ouvert, les
identifiants et la documentation du service transmis aux
utilisateurs.


Installation du client en ligne de commandes (iCommands)
======================

C'est le client à privilégier car il offre le plus de de fonctionnalités. C'est le plus stable et le plus rapide. 

Prérequis
---------

L'installation du client iRODS nécessite de disposer d'un poste client
fonctionnant avec le système CentOS 7 ou Ubuntu (16.04, 18.04), sur
lequel il est possible d'installer de nouveaux logiciels.

Les utilisateurs sous Windows 10 peuvent installer WSL (Windows Subsystem for Linux) qui permet d’exécuter un environnement Linux directement sur Windows 10 (https://docs.microsoft.com/fr-fr/windows/wsl/install-win10).


Environnement
-------------

Le client en ligne de commande iRODS utilise un fichier pour stocker
la configuration pour se connecter à l'infrastructure iRODS. Ce fichier
est *~/irods/irods_environment.json* et doit être créé avec le contenu
suivant :

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

Les valeurs *<username>* et *<default_resource>* doivent être remplacées par
les valeurs qui vous ont été transmises lors de l'enregistrement auprès du service.


Installation des paquets
------------------------

Les paquets pour le client iRODS sont disponibles pour les systèmes
CentOS 7, Ubuntu 16.04 et Ubuntu 18.04.

Les instructions d'installation pour les systèmes de gestion de paquets
*APT* et *YUM* sont détaillées sur la page https://packages.irods.org.

Une fois que le dépôt est correctement configuré, il faut installer le
paquet ``irods-icommands``.

La vérification de l'installation du client est réalisée avec :

.. code-block:: shell-session

   $ iinit

La commande **iinit** permet d'ouvrir une session vers l'instance iRODS.
Elle vous demande le mot de passe pour vous connecter. Une fois que vous
avez terminé votre transfert de données, vous pouvez terminer la session
avec la commande **iexit**.

La commande **ienv** affiche l'environnement iRODS :

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


Utilisation du service FG-iRODS
===============================

Aide en ligne
-------------

**ihelp** permet d'afficher la liste des commandes iRODS, ainsi que l'aide
sur une commande spécifique :

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

La liste complète des commandes disponibles est également disponible dans
la `documentation officielle iRODS <https://docs.irods.org/4.2.8/icommands/user/>`_.


Répertoire de travail
---------------------

La commande **ils** affiche le contenu du répertoire courant avec lequel
vous travaillez sur le système FG-iRODS (par défaut, il s'agit de votre
répertoire utilisateur) :

.. code-block:: shell-session

   $ ils
   /FranceGrillesZone/home/<username>:

* *FranceGrillesZone* : le nom de la zone iRODS

* */home/<username>* : votre répertoire personnel

Il est possible de modifier le répertoire sur lequel le client
iRODS se connecte en ajoutant les lignes suivantes au fichier
de configuration iRODS :

.. code-block:: console

   "irods_cwd": "<chemin_repertoire>",
   "irods_home": "<chemin_repertoire>",

Il faut remplacer *<chemin_repertoire>* par le chemin souhaité par
défaut.


Chargement des données
----------------------

Dans cette section, des fichiers vont être chargés vers FG-iRODS. Le
fichier utilisé pour ces exemples est ``foo.bin``, et peut être
remplacé par un autre fichier de votre choix. Si vous souhaitez travailler
avec le fichier ``foo.bin``, vous pouvez le créer avec la commande
suivante :

.. code-block:: shell-session

   $ dd if=/dev/urandom of=foo.bin count=65536

Le fichier est copié vers l'infrastructure iRODS avec la commande :

.. code-block:: shell-session

   $ iput -K foo.bin

L'option *-K* permet de vérifier le *checksum* et de le stocker dans la base
de données. Il est recommandé de l'utiliser systématiquement. Le fichier
est maintenant disponible sur FG-iRODS :

.. code-block:: shell-session

   $ ils
   /FranceGrillesZone/home/<username>:
     foo.bin

Le fichier peut être supprimé avec la commande suivante :

.. code-block:: shell-session

   $ irm foo.bin


Espace de nom et chemin physique
--------------------------------

iRODS fournit une abstraction de l'emplacement physique des fichiers.
Par exemple, ``/FranceGrillesZone/home/<username>/foo.bin`` est le chemin
logique utilisé par iRODS. Pour savoir où sont réellement stockées
les données, il faut utiliser l'option **-L** avec la commande **ils** :

.. code-block:: shell-session

   $ ils -L
   /FranceGrillesZone/home/<username>:
     <username>         0 mcia;mcia-fgirods1     33554432 2020-11-20.09:30 & foo.bin
       sha2:veVzp+ApMzyVRzZN0BZIkDyFuqUp/4tM4sLVACp00B8=    generic    /vault1/resc/home/<username>/foo.bin


Cette commmande nous indique que :

  * le fichier ``foo.bin`` est enregistré par FG-iRODS comme :
    ``/FranceGrillesZone/home/<username>/foo.bin`` ;

  * son propriétaire est *<username>* ;

  * il a été chargé sur la ressource de stockage *mcia* ;

  * il n'y a qu'un seul réplica, dont l'identifiant est *0* ;

  * sa taille est de 33554432 octets ;

  * son *checksum* a été enregistré (*sha:veVzp+ApMzyVRzZN0BZIkDyFuqUp/4tM4sLVACp00B8=*).


Téléchargement de données
-------------------------

Le fichier stocké dans FG-iRODS peut être téléchargé avec :

.. code-block:: shell-session

   $ iget -K foo.bin foo-restore.txt

Le fichier ``foo.bin`` a été téléchargé et nommé ``foo-restore.txt``.
Avec l'option **-K** option, le *checksum* du fichier local est comparé
avec le *checksum* du fichier sur FG-iRODS.


Structuration des données
-------------------------

Bonnes pratiques
++++++++++++++++

- Afn de conserver une compatibilité maximale entre les environnements Windows et Linux, il est fortement déconseillé d’utiliser des caractères accentués ainsi que des espaces pour nommer des fichiers ou des collections.

- La longueur des chemins d’accès aux fichiers ne doit pas excéder 256 caractères incluant le nom du fichier lui-même.

- Il est préférable de transférer des fichiers volumineux en nombre restreint par rapport au transfert d’une multitude de ‘petits’ fichiers. Il convient de créer une archive en local (.tar) avant de procéder au transfert : la durée de copie en sera grandement diminuée (mécanisme de threads) et le catalogue iRODs sera moins sollicité.

- Il existe différents clients compatibles avec iRODS (https://irods.org/clients/) mais le client en ligne de commande (iCommands) est à privilégier.

Création d'une collection
+++++++++++++++++++++++++

Sur votre ordinateur, les données sont organisées dans des répertoires.
Avec iRODS, elles sont organisées de la même manière, sauf que ces dossiers
sont appelés des *collections*.

Pour créer une collection iRODS :

.. code-block:: shell-session

   $ imkdir mycollection

Le fichier ``foo.bin`` peut être déplacé dans la collection
*mycollection* avec :

.. code-block:: shell-session

   $ imv foo.bin mycollection
   $ ils -L mycollection
   /FranceGrillesZone/home/<username>/mycollection:
     <username>         0 mcia;mcia-fgirods1     33554432 2020-11-20.10:18 & foo.bin
       sha2:veVzp+ApMzyVRzZN0BZIkDyFuqUp/4tM4sLVACp00B8=    generic    /vault1/resc/home/<username>/mycollection/foo.bin

Vous pouver voir que le chemin logique de la collection
``/FranceGrillesZone/home/<username>/mycollection`` a un
répertoire physique : ``/vault1/resc/home/<username>/mycollection``.
Ainsi, les données n'arrivent pas n'importe où sur un serveur iRODS,
mais se placent dans cette structure.

Les données peuvent être chargées directement dans une collection :

.. code-block:: shell-session

   $ iput -K -r bar.txt mycollection
   $ ils  /FranceGrillesZone/home/<username>/mycollection
   /FranceGrillesZone/home/<username>/mycollection:
     bar.txt
     foo.bin

L'option **-r** permet un chargement récursif. Elle est utile si vous souhaitez transférer le contenu d'un dossier.


Naviguer à travers les collections
++++++++++++++++++++++++++++++++++

Le répertoire courant de travail correspond à l'emplacement sur lequel
vous vous trouvez dans l'arborescence iRODS. Pour afficher votre répertoire
courant sur iRODS, utilisez :

.. code-block:: shell-session

   $ ipwd
   /FranceGrillesZone/home/<username>

Si vous ne spécifiez pas le chemin complet, mais uniquemenet un chemin
relatif tel que ``mycollection/<file>``, iRDS utilise automatiquement
le répertoire courant de travail comme préfixe. Vous pouvez vous déplacer
dans l'arborescence et modifier ce répertoire courant de travail avec la
commande **icd** :

.. code-block:: shell-session

   $ icd mycollection


Gestion des métadonnées
-----------------------

iRODS est un logiciel disposant de nombreuses fonctionnalités reposant
sur l'utilisation des métadonnées.

Création de métadonnées
+++++++++++++++++++++++

Il est possible d'ajouter à chaque fichier une ou plusieurs métadonnées
représentées sous forme de triplet *Attribute*, *Value*, *Unit* (AVU).
Ces triplets sont ajoutés dans la base iCAT d'iRODS et peuvent être
recherchés. Les métadonnées sont ajoutées avec la commande :

.. code-block:: shell-session

   $ imeta add -d foo.bin 'length' '20' 'words'


Le champs *Unit* peut être vide :

.. code-block:: shell-session

   $ imeta add -d foo.bin 'project' 'example'

Les métadonnnées peuvent également être ajoutées à une collection :

.. code-block:: shell-session

   $ imeta add -C mycollection 'author' 'John Smith'


Affichage des métadonnées
+++++++++++++++++++++++++

Pour afficher les métadonnées d'un objet de données (fichier), il
faut entrer :

.. code-block:: shell-session

   $ imeta ls -d foo.bin
   AVUs defined for dataObj /FranceGrillesZone/home/<username>/mycollection/foo.bin:
   attribute: length
   value: 20
   units: words

et pour une collection, la commande suivante :

.. code-block:: shell-session

   $ imeta ls -C mycollection
   AVUs defined for collection /FranceGrillesZone/home/<username>/mycollection:
   attribute: author
   value: John Smith
   units:


Recherche avec les métadonnées
------------------------------

La recherche de fichiers ou de collections à l'aide des métadonnées
est effectuée avec la commande suivante :

.. code-block:: shell-session

   $ imeta qu -d 'length' = '20'
   collection: /FranceGrillesZone/home/<username>/mycollection
   dataObj: foo.bin



Recherche avancée
-----------------

Afin d'effectuer une recherche plus fine de fichiers ou de collections,
il est possible d'interroger directement le catalogue iCAT avec la
commande **iquest** :

.. code-block:: shell-session

   $ iquest "select COLL_NAME, META_COLL_ATTR_VALUE where META_COLL_ATTR_NAME like 'author'"
   COLL_NAME = /FranceGrillesZone/home/<username>/mycollection
   META_COLL_ATTR_VALUE = John Smith
   ------------------------------------------------------------

Les résultats peuvent être filtrés en fonction d'une valeur spécifique :

.. code-block:: shell-session

   $ iquest "select COLL_NAME, META_COLL_ATTR_VALUE where META_COLL_ATTR_NAME like 'author' \
   and META_COLL_ATTR_VALUE like 'John%'"
   COLL_NAME = /FranceGrillesZone/home/<username>/mycollection
   META_COLL_ATTR_VALUE = John Smith
   ------------------------------------------------------------


**NOTE**: le caractère '%' est un caractère générique (*wildcard*).

Si vous recherchez un objet de données plutôt qu'une collection, il
faut remplacer *META_COLL_ATTR_NAME* par *META_DATA_ATTR_NAME*. De
nombreux attributs peuvent être utilisés pour les recherches. Pour
les afficher, utilisez :

.. code-block:: shell-session

   $ iquest attrs


Contrôle d'accès
----------------

iRODS propose un mécanisme de droits d'accès similaire au système
disponible sur les systèmes UNIX (ACL). Il permet de contrôler les
droits de lecture, d'écriture et de propriété. Pour afficher les droits
d'accès à la collection actuelle :

.. code-block:: shell-session

   $ ils -r -A
   /FranceGrillesZone/home/<username>/mycollection:
           ACL - <username>#FranceGrillesZone:own
           Inheritance - Disabled
     bar.txt
           ACL - <username>#FranceGrillesZone:own
     foo.bin
           ACL - <username>#FranceGrillesZone:own


Les droits d'accès à un fichier sont spécifiés après le mot-clé *ACL*.
Dans cet exemple, *<username>* est propriétaire de tous les fichiers
affichés. Aucune autre personne ne peut y accéder.

Les collections ont un attribut *Inheritance*. Lorsque la valeur de cet
attribut est égale à *Enabled*, l'ensemble du contenu de la collection
hérite des droits d'accès de la collection. Cet héritage ne s'applique
qu'aux nouveaux fichiers copiés dans la collection.

Les commandes permettant d'activer ou de désactiver l'héritage sont les suivantes:

.. code-block:: shell-session

   ichmod inherit mycollection
   ichmod noinherit mycollection 

La modification des droits d'accès pour autoriser un collègue à accéder
à ses données se fait avec :

.. code-block:: shell-session

   $ ichmod read <colleague> foo.bin

L'utilisateur *<colleague>* peut maintenant accéder en lecture au
fichier ``foo.bin``.

Pour une gestion avancée des droits d'accés la commande *igroupadmin* permet de créer des utilisateurs, gérer des groupes, ... Elle est disponible pour les utilisateurs de type groupeadmin.

Installation du client Brocoli
==============================

Ce client a été développé par Pierre Gay pour le mesocentre de Calcul Intensif Aquitain. Il offre une interface graphique permettant de se connecter à iRODS. 
Son installation est détaillée ici : https://github.com/mesocentre-mcia/brocoli. 
Bien qu'expérimentale l'installation sous Windows fonctionne. Elle nécessite l'installation de miniconda afin de disposer de l'environnement pip.

Installation du client FUSE
==============================

Il permet de monter son home irods comme un système de fichier local. Le client fuse est livré avec le package des icommandes. 

Une fois le package installé, on trouve la commande irodsFs avec les autres icommandes sous /usr. Pour l'utiliser les commandes sont les suivantes:

- On se logue sur la zone irods :

.. code-block:: shell-session
   iinit

- On crée un point de montage sur son ordinateur:

.. code-block:: shell-session
   sudo mkdir -p /tmp/fmount

- On monte son home irods:

.. code-block:: shell-session
   irodsFs /tmp/fmount
   
- On peut maintenant accéder aux fichiers en lecture:

.. code-block:: shell-session
   ls -l /tmp/fmount/
   total 12300504
   -rw-rw---- 1 pmoreau pmoreau 10737418240 févr. 15 09:51 test_data_10G

- On peut aussi écrire (si les droits l'autorisent):

.. code-block:: shell-session
   cp  foo.bin /tmp/fmount
   
- Aprés utilisation il faut démonter le système de fichiers :

.. code-block:: shell-session
   umount -u /tmp/fmount

Attention cependant les performances sont en-dessous des icommandes.
