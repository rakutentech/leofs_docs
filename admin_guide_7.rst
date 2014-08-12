.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    Manager maintenance

Manager Maintenance
===================

+--------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
| **Shell**                                                                            | **Description**                                                                   |
+======================================================================================+===================================================================================+
| leofs-adm :ref:`backup-mnesia <backup-mnesia>` <backup-filepath>                     | * Copy LeoFS's Manager data to the filepath                                       |
+--------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
| leofs-adm :ref:`restore-mnesia <restore-mnesia>` <backup-filepath>                   | * Restore LeoFS's Manager data from the backup file                               |
+--------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
| leofs-adm :ref:`update-managers <update-managers>` <manager-master> <manager-slave>  | * Update LeoFS Manager nodes                                                      |
|                                                                                      | * Destribute the new LeoFS Manager nodes to LeoFS Storage and Gateway             |
+--------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
| leofs-adm :ref:`dump-ring <dump-ring>` (<manager-node>|<storage-node>|<gateway-node>)| * Dump the ring data to the local disk                                            |
+--------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+


.. _backup-mnesia:

.. index::
    pair: Manager maintenance; backup-mnesia-command

backup-mnesia <backup-filepath>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Copy LeoFS's Manager data to the file-path

.. code-block:: bash

    $ leofs-adm backup-mnesia /path/to/file
    OK

\

.. _restore-mnesia:

.. index::
    pair: Manager maintenance; restore-mnesia-command

restore-mnesia <backup-filepath>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Restore LeoFS's Manager data from the backup file

.. code-block:: bash

    $ leofs-adm restore-mnesia /path/to/file
    OK

\

.. _update-managers:

.. index::
    pair: Manager maintenance; update-managers-command

update-managers <manager-master> <manager-slave>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Update LeoFS Manager nodes
* Destribute the new LeoFS Manager nodes to LeoFS Storage and Gateway

.. code-block:: bash

    $ leofs-adm update-managers manager_0@10.1.0.1 manager_1@10.1.0.2
    OK

\

.. _dump-ring:

.. index::
    pair: Manager maintenance; dump-ring-command

dump-ring (<manager-node>|<storage-node>|<gateway-node>)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Dump the ring data to the local disk

.. code-block:: bash

    $ leofs-adm dump-ring storage_0@10.1.0.11
    OK

\

