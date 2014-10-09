.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    Storage operation

Storage Operation
=================

* LeoFS Storage operation commands are executed on **LeoFS-Manager console** OR the ``leofs-adm`` script.
* Refer :ref:`LeoFS operation flow diagram <operation-flow-diagram-label>`

+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| **Shell**                                                 | **Description**                                                                                   |
+===========================================================+===================================================================================================+
| leofs-adm :ref:`detach <detach-command>` <storage-node>   | * Remove the storage node in the LeoFS storage cluster                                            |
|                                                           | * Current status: ``running`` | ``stop``                                                          |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`suspend <suspend-command>` <storage-node> | * Suspend a storage node for maintenance                                                          |
|                                                           | * This command does NOT detach the node from the storage cluster                                  |
|                                                           | * While suspending, it rejects any requests                                                       |
|                                                           | * Current status: ``running``                                                                     |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`resume <resume-command>` <storage-node>   | * Resume a storage node for finished maintenance                                                  |
|                                                           | * Current status: ``suspended`` | ``restarted``                                                   |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`start <start-command>`                    | * Start LeoFS after distributing the RING from LeoFS Manager to LeoFS Storage and LeoFS Gateway   |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`rebalance <rebalance-command>`            | * Commit detached and attached nodes to join the cluster                                          |
|                                                           | * Rebalance objects in the cluster based on the updated cluster topology                          |
+-----------------------------------------------------------+---------------------------------------------------------------------------------------------------+


.. index::
    pair: Storage operation; detach-command

.. _detach-command:

detach <storage-node>
^^^^^^^^^^^^^^^^^^^^^

Remove the storage node in the storage cluster

.. code-block:: bash

    $ leofs-adm detach storage_0@127.0.0.1
    OK
    $ leofs-adm rebalance
    OK


.. index::
    pair: Storage operation; suspend-command

.. _suspend-command:

suspend <storage-node>
^^^^^^^^^^^^^^^^^^^^^^

* Suspend the storage node
* While suspending, it rejects any requests
* This command does NOT detach the node from the storage cluster

.. code-block:: bash

    $ leofs-adm suspend storage_0@127.0.0.1
    OK


.. index::
    pair: Storage operation; resume-command

.. _resume-command:

resume <storage-node>
^^^^^^^^^^^^^^^^^^^^^

Resume the storage node

.. code-block:: bash

    $ leofs-adm resume storage_0@127.0.0.1
    OK

\


.. index::
    pair: Storage operation; start-command

.. _start-command:

start
^^^^^

Start LeoFS after distributing the RING from LeoFS Manager to LeoFS Storage and LeoFS Gateway

.. code-block:: bash

    $ leofs-adm start
    OK

\


.. index::
    pair: Storage operation; rebalance-command

.. _rebalance-command:

rebalance
^^^^^^^^^

Commit detached and attached nodes to join the cluster AND Rebalance objects in the cluster based on the updated cluster topology

.. code-block:: bash

    $ leofs-adm rebalance
    OK

