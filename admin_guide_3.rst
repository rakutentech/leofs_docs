.. LeoFS documentation
.. Copyright (c) 2013-2014 Rakuten, Inc.

Storage Operation
=================

.. index::
    Storage operation

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
| start                           | Start LeoFS after distributing the RING from LeoFS Manager to LeoFS Storage and LeoFS Gateway     |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| rebalance                       | Commit detached and attached nodes to join the cluster AND Rebalance objects in the cluster       |
|                                 | based on the updated cluster topology                                                             |
+---------------------------------+---------------------------------------------------------------------------------------------------+


.. index::
    pair: Storage operation; detach-command

.. _detach-command-label:

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

.. _suspend-command-label:

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

.. _resume-command-label:

resume <storage-node>
^^^^^^^^^^^^^^^^^^^^^

Resume the storage node

::

    resume storage_0@127.0.0.1
    OK

\


.. index::
    pair: Storage operation; start-command

.. _start-command-label:

start
^^^^^

Start LeoFS after distributing the RING from LeoFS Manager to LeoFS Storage and LeoFS Gateway

::

    rebalance
    OK

\


.. index::
    pair: Storage operation; rebalance-command

.. _rebalance-command-label:

rebalance
^^^^^^^^^

Commit detached and attached nodes to join the cluster AND Rebalance objects in the cluster based on the updated cluster topology

::

    rebalance
    OK

\


Recover Commands
=================

+---------------------------------+---------------------------------------------------------------------------------------------------+
| Command                         | Description                                                                                       |
+=================================+===================================================================================================+
| **Recover Commands**                                                                                                                |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover file `<file-path>`      | Recover an inconsistent object specified by the file-path                                         |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover node `<storage-node>`   | Recover all inconsistent objects in the specified node                                            |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover ring `<storage-node>`   | Recover rings of the specified node                                                               |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover cluster `<cluster-id>`  | [v1.0.0-] Recover all inconsistent objects in the specified cluster                               |
+---------------------------------+---------------------------------------------------------------------------------------------------+


.. index::
    pair: Recover commands; recover-file-command

recover file <file-path>
^^^^^^^^^^^^^^^^^^^^^^^^^

Recover an inconsistent object specified by the file-path

::

  recover file leo/fast/storage.key
  OK

\


.. index::
    pair: Recover commands; recover-node-command

recover node <node>
^^^^^^^^^^^^^^^^^^^

Recover all inconsistent objects in the specified node

::

  recover node storage_0@127.0.0.1
  OK

\


.. index::
    pair: Recover commands; recover-ring-command

recover ring <node>
^^^^^^^^^^^^^^^^^^^

Recover rings of the specified node

::

  recover ring storage_0@127.0.0.1
  OK

\


.. index::
    pair: Recover commands; recover-cluster-command

recover cluster <cluster-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Recover all inconsistent objects in the specified cluster

::

  recover cluster cluster-1
  OK

\


Compaction Commands
===================

.. index::
    Compaction commands

Remove logical deleted objects and meta data and check the current disk usage

+-------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
| Command                                                     | Description                                                                                                     |
+=============================================================+=================================================================================================================+
| **Compaction**                                                                                                                                                                |
+-------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
| compact start `<storage-node>` (`all` | `<num-of-targets>`) | * Execute to remove unnecessary objects from the node                                                           |
| `[<num-of-compaction-proc>]`                                | * num-of-targets: It controls the number of containers in parallel                                              |
|                                                             | * num-of-compaction-procs: It controls the number of procs to execute the compaction in parallel                |
+-------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
| compact suspend `<storage-node>`                            | Suspend to execute the compaction                                                                               |
+-------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
| compact resume  `<storage-node>`                            | Resume to execute the compaction                                                                                |
+-------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
| compact status  `<storage-node>`                            | * See the current compaction status                                                                             |
|                                                             | * Compaction's status: ``idle``, ``running``, ``suspend``                                                       |
+-------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
| **Disk Usage**                                                                                                                                                                |
+-------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
| du `<storage-node>`                                         | See the current disk usages                                                                                     |
+-------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
| du detail `<storage-node>`                                  | See the current disk usages in the details                                                                      |
+-------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+

\


.. image:: _static/images/leofs-compaction-state-transition.png
   :width: 640px

\


.. index::
    pair: Compaction commands; compact-start-command

compact start <storage-node> (all | <num-of-targets>) [<num-of-compaction-procs>]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Execute to remove unnecessary objects from the node
* num-of-targets: It controls the number of containers in parallel
* num-of-compaction-procs: It controls the number of procs to execute the compaction in parallel


.. note:: Default ``<num-of-compation-procs>`` is '3' - You can control the number of processes to execute compaction in parallel. It enables you to get maximum performance by setting an appropriate number corresponding to the number of cores.

::

    ## All compaction-targets will be executed with 3 concurrent processes
    ## (default concurrency is 3)
    compact start storage_0@127.0.0.1 all
    OK

::

    ## Number of compaction-targets will be executed with 2 concurrent processes
    compact start storage_0@127.0.0.1 5 2
    OK

\


.. index::
    pair: Compaction commands; compact-suspend-command

compact suspend <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    compact suspend storage_0@127.0.0.1
    OK

\


.. index::
    pair: Compaction commands; compact-resume-command

compact resume <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    compact resume storage_0@127.0.0.1
    OK

\


.. index::
    pair: Compaction commands; compact-status-command


compact status <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Compaction's status: ``idle``, ``running`` and ``suspend``

::

  compact status storage_0@127.0.0.1
          current status: running
   last compaction start: 2013-03-04 12:39:47 +0900
           total targets: 64
    # of pending targets: 5
    # of ongoing targets: 3
    # of out of targets : 56

\

.. index::
    pair: Compaction commands; du-command

du <storage-node>
^^^^^^^^^^^^^^^^^

See the current disk usages

::

    du storage_0@127.0.0.1
     active number of objects: 19968
      total number of objects: 39936
       active size of objects: 198256974.0
        total size of objects: 254725020.0
         ratio of active size: 77.83%
        last compaction start: 2013-03-04 12:39:47 +0900
          last compaction end: 2013-03-04 12:39:55 +0900


.. index::
.. index::
    pair: Compaction commands; du-detail-command

du detail <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^^

See the current disk usages in the details


::

    du detail storage_0@127.0.0.1
    [du(storage stats)]
                    file path: /home/leofs/dev/leofs/package/leofs/storage/avs/object/0.avs
     active number of objects: 320
      total number of objects: 640
       active size of objects: 3206378.0
        total size of objects: 4082036.0
         ratio of active size: 78.55%
        last compaction start: 2013-03-04 12:39:47 +0900
          last compaction end: 2013-03-04 12:39:55 +0900
    .
    .
    .
                    file path: /home/leofs/dev/leofs/package/leofs/storage/avs/object/63.avs
     active number of objects: 293
      total number of objects: 586
       active size of objects: 2968909.0
        total size of objects: 3737690.0
         ratio of active size: 79.43%
        last compaction start: ____-__-__ __:__:__
          last compaction end: ____-__-__ __:__:__

\
