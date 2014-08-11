.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    Manager maintenance

Manager Maintenance
===================

+------------------------------------------------------------+-----------------------------------------------------------------------------------+
| **Command**                                                | **Description**                                                                   |
+============================================================+===================================================================================+
| **Manager Maintenance**                                                                                                                        |
+------------------------------------------------------------+-----------------------------------------------------------------------------------+
| backup-mnesia <backup-filepath>                            | * Copy LeoFS's Manager data to the file-path                                      |
+------------------------------------------------------------+-----------------------------------------------------------------------------------+
| restore-mnesia <backup-filepath>                           | * Restore LeoFS's Manager data from the backup file                               |
+------------------------------------------------------------+-----------------------------------------------------------------------------------+
| update-managers <manager-master> <manager-slave>           | * Update LeoFS Manager nodes                                                      |
|                                                            | * Destribute the new LeoFS Manager nodes to LeoFS Storage and Gateway             |
+------------------------------------------------------------+-----------------------------------------------------------------------------------+
| dump-ring (<manager-node>|<storage-node>|<gateway-node>)   | * Dump the ring data to the local disk                                            |
+------------------------------------------------------------+-----------------------------------------------------------------------------------+


.. index::
    pair: Manager maintenance; backup-mnesia-command

backup-mnesia <backup-filepath>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Copy LeoFS's Manager data to the file-path

::

    backup-mnesia /path/to/file
    OK

\

.. index::
    pair: Manager maintenance; restore-mnesia-command

restore-mnesia <backup-filepath>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Restore LeoFS's Manager data from the backup file

::

    restore-mnesia /path/to/file
    OK

\

.. index::
    pair: Manager maintenance; update-managers-command

update-managers <manager-master> <manager-slave>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Update LeoFS Manager nodes
* Destribute the new LeoFS Manager nodes to LeoFS Storage and Gateway

::

    update-managers manager_0@10.1.0.1 manager_1@10.1.0.2
    OK

\

.. index::
    pair: Manager maintenance; dump-ring-command

dump-ring (<manager-node>|<storage-node>|<gateway-node>)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Dump the ring data to the local disk

::

    dump-ring storage_0@10.1.0.11
