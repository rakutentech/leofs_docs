.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    LeoFS Storage operation

Storage Operation
=================

.. index::
   pair: LeoFS Storage cluster; LeoFS Storage operation

Storage Cluster Operation
-------------------------

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

\

.. index::
   pair: LeoFS Storage MQ; LeoFS Storage operation

Storage MQ Operation [1.2.0-]
-----------------------------

* LeoFS Storage MQ is controllable mechanism manually. We've published ``mq-suspend`` and ``mq-resume`` command in ``leofs-adm`` script.

+--------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| **Shell**                                                                | **Description**                                                                                   |
+==========================================================================+===================================================================================================+
| leofs-adm :ref:`mq-stats <mq-stats-command>` <storage-node>              | * See the statuses of message queues used in LeoFS Storage                                        |
+--------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`mq-suspend <mq-suspend-command>` <storage-node> <mq-id>  | * Suspend a process consuming a message queue                                                     |
|                                                                          | * Active message queues only can be suspended                                                     |
|                                                                          | * While suspending, no messages are consumed                                                      |
+--------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`mq-resume <mq-resume-command>` <storage-node> <mq-id>    | * Resume a process consuming a message queue                                                      |
+--------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+

\

.. _mq-stats-command:

mq-stats <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ ./leofs-adm mq-stats storage_0@127.0.0.1
                 id                |    state    | num of msgs |            description
   --------------------------------+-------------+-------------|-----------------------------------
    leo_delete_dir_queue           |   idling    | 0           | delete directories
    leo_comp_meta_with_dc_queue    |   idling    | 0           | compare metadata w/remote-node
    leo_sync_obj_with_dc_queue     |   idling    | 0           | sync objs w/remote-node
    leo_recovery_node_queue        |   idling    | 0           | recovery objs of node
    leo_async_deletion_queue       |   idling    | 0           | async deletion of objs
    leo_rebalance_queue            |   idling    | 0           | rebalance objs
    leo_sync_by_vnode_id_queue     |   idling    | 0           | sync objs by vnode-id
    leo_per_object_queue           |   idling    | 0           | recover inconsistent objs


.. _mq-suspend-command:

mq-suspend <storage-node> <mq-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ ./leofs-adm mq-suspend storage_0@127.0.0.1 leo_delete_dir_queue
    OK

.. _mq-resume-command:

mq-resume <storage-node> <mq-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ ./leofs-adm mq-resume storage_0@127.0.0.1 leo_delete_dir_queue
    OK
