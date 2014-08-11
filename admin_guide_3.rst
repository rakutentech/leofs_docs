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

+---------------------------------+---------------------------------------------------------------------------------------------------+
| Command                         | Description                                                                                       |
+=================================+===================================================================================================+
| **Storage Operation**                                                                                                               |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| detach `<storage-node>`         | * Remove the storage node in the LeoFS storage cluster                                            |
|                                 | * Current status: ``running`` | ``stop``                                                          |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| suspend `<storage-node>`        | * Suspend a storage node for maintenance                                                          |
|                                 | * This command does NOT detach the node from the storage cluster                                  |
|                                 | * While suspending, it rejects any requests                                                       |
|                                 | * Current status: ``running``                                                                     |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| resume `<storage-node>`         | * Resume a storage node for finished maintenance                                                  |
|                                 | * Current status: ``suspended`` | ``restarted``                                                   |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| start                           | * Start LeoFS after distributing the RING from LeoFS Manager to LeoFS Storage and LeoFS Gateway   |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| rebalance                       | * Commit detached and attached nodes to join the cluster                                          |
|                                 | * Rebalance objects in the cluster based on the updated cluster topology                          |
+---------------------------------+---------------------------------------------------------------------------------------------------+


.. index::
    pair: Storage operation; detach-command

.. _detach-command:

detach <storage-node>
^^^^^^^^^^^^^^^^^^^^^

Remove the storage node in the storage cluster

::

    detach storage_0@127.0.0.1
    OK
    rebalance
    OK


.. index::
    pair: Storage operation; suspend-command

.. _suspend-command:

suspend <storage-node>
^^^^^^^^^^^^^^^^^^^^^^

* Suspend the storage node
* While suspending, it rejects any requests
* This command does NOT detach the node from the storage cluster

::

    suspend storage_0@127.0.0.1
    OK


.. index::
    pair: Storage operation; resume-command

.. _resume-command:

resume <storage-node>
^^^^^^^^^^^^^^^^^^^^^

Resume the storage node

::

    resume storage_0@127.0.0.1
    OK

\


.. index::
    pair: Storage operation; start-command

.. _start-command:

start
^^^^^

Start LeoFS after distributing the RING from LeoFS Manager to LeoFS Storage and LeoFS Gateway

::

    start
    OK

\


.. index::
    pair: Storage operation; rebalance-command

.. _rebalance-command:

rebalance
^^^^^^^^^

Commit detached and attached nodes to join the cluster AND Rebalance objects in the cluster based on the updated cluster topology

::

    rebalance
    OK

