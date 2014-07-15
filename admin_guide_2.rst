Storage Operation
=================

Storage Cluster Operation Commands
----------------------------------

.. index::
    pair: Operation; Command

* LeoFS-cluster's operation commands are executed on **LeoFS-Manager Console**.
* Please refer :ref:`LeoFS operation flow diagram <operation-flow-diagram-label>`, too.


.. index::
   Storage-cluster-related-commands


Table of Storage Cluster's Commands
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

\

+---------------------------------+---------------------------------------------------------------------------------------------------+
| Command                         | Explanation                                                                                       |
+=================================+===================================================================================================+
| *Storage-node related commands*                                                                                                     |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| detach `{STORAGE_NODE}`         | * Remove a storage node from the LeoFS storage-cluster                                            |
|                                 | * Current status: ``running`` | ``stop``                                                          |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| suspend `{STORAGE_NODE}`        | * Suspend a storage node for maintenance. This command does NOT change the "routing-table (RING)" |
|                                 | * Current status: ``running``                                                                     |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| resume `{STORAGE_NODE}`         | * Resume a storage node                                                                           |
|                                 | * Current status: ``suspended`` | ``restarted``                                                   |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| *Storage-cluster related commands*                                                                                                  |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| start                           | Launch LeoFS after distributing the "routing-table (RING)" from Manager to Storage and Gateway    |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| rebalance                       | Move or Copy files into the LeoFS storage-cluster due to changed RING                             |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| whereis `{FILE_PATH}`           | Retrieve status of an assigned file                                                               |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| **Recover**                                                                                                                         |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover file `{FILE_PATH}`      | Synchronize an object between nodes in charge                                                     |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover node `{STORAGE_NODE}`   | Recover belonging target node's objects                                                           |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover ring `{STORAGE_NODE}`   | Synchronize target node's RING with Manager's RING                                                |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover cluster `{CLUSTER_ID}`  | [v1.0.0-] Synchronize objects between local-cluster with a remote-cluster                         |
+---------------------------------+---------------------------------------------------------------------------------------------------+

.. index::
    detach-command

.. _detach-command-label:

**'detach'** - Storage node is removed from the LeoFS-Cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``detach {STORAGE_NODE}``

::

    detach storage_0@127.0.0.1
    OK
    rebalance
    OK

.. index::
   suspend-command

**'suspend'** - Suspend a storage node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``suspend {STORAGE_NODE}``

::

    suspend storage_0@127.0.0.1
    OK

.. index::
   resume-command

**'resume'** - Resume a storage node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``resume {STORAGE_NODE}``

::

    resume storage_0@127.0.0.1
    OK

.. index::
   rebalance-command

.. _rebalance-command-label:

**'rebalance'** - Rebalance files into the cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``rebalance``

::

    rebalance
    OK

.. _whereis:

.. index::
   whereis-command

**'whereis'**
^^^^^^^^^^^^^

Paths used by `whereis` are ruled by :ref:`this rule <s3-path-label>`

Command: ``whereis {FILE_PATH}``

::

    whereis leo/fast/storage.key
    -----------------------------------------------------------------------------------------------------------------------
     del? node                 ring address    size   # of chunks  checksum    vclock            when
    -----------------------------------------------------------------------------------------------------------------------
          storage_1@127.0.0.1  207643840133    35409  0             4116193149  1332407492290951  2012-06-29 14:23:31 +0900
          storage_0@127.0.0.1  207643840133    35409  0             4116193149  1332407492290951  2012-06-29 14:23:31 +0900

\

\

**recover** - Recover target node's objects and RING synchronization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. index:: recover-file-command

**'recover file'** - Synchronize an object between nodes

::

  recover file leo/fast/storage.key
  OK

\

.. index:: recover-node-command

**'recover node'** - Recover target node's objects

::

  recover node storage_0@127.0.0.1
  OK

\

.. index:: recover-ring-command

**'recover ring'** - Synchronize target node's RING with Manager's RING

::

  recover ring storage_0@127.0.0.1
  OK

\
\

Storage Maintenance Commands
----------------------------

\

+-----------------------------------------------------------+----------------------------------------------------------------+
| Command                                                   | Explanation                                                    |
+===========================================================+================================================================+
| **Disk Usage**                                                                                                             |
+-----------------------------------------------------------+----------------------------------------------------------------+
| du `{STORAGE_NODE}`                                       | Display disk usages (like Unix du command)                     |
+-----------------------------------------------------------+----------------------------------------------------------------+
| du detail `{STORAGE_NODE}`                                | Display disk usages in details (like Unix du command)          |
+-----------------------------------------------------------+----------------------------------------------------------------+
| **Compaction**                                                                                                             |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact start `{STORAGE_NODE}` `all` | `{NUM_OF_TARGETS}` | * Compact raw files used by the LeoFS Storage subsystem        |
| `[{NUM_OF_COMPACT_PROC}]`                                 | * Default {NUM_OF_COMPACT_PROC} is '3'                         |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact suspend `{STORAGE_NODE}`                          | Suspend a compaction job in progress                           |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact resume  `{STORAGE_NODE}`                          | Resume a suspended compaction job                              |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact status  `{STORAGE_NODE}`                          | * Display compaction statuses                                  |
|                                                           | * Compaction's status: ``idle``, ``running``, ``suspend``      |
+-----------------------------------------------------------+----------------------------------------------------------------+

\

**du** - Disk Usage
^^^^^^^^^^^^^^^^^^^

.. index:: du-command

**'du'** - Display disk usage (summary)

Command: ``du {STORAGE_NODE}``

::

    du storage_0@127.0.0.1
     active number of objects: 19968
      total number of objects: 39936
       active size of objects: 198256974.0
        total size of objects: 254725020.0
         ratio of active size: 77.83%
        last compaction start: 2013-03-04 12:39:47 +0900
          last compaction end: 2013-03-04 12:39:55 +0900

.. index:: du-detail-command

**'du detail'** - Display disk usage in details (per raw file)

Command: ``du detail {STORAGE_NODE}``

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

**compact** - Remove logical deleted objects and meta data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

\

.. image:: _static/images/leofs-compaction-state-transition.png
   :width: 640px

\
\

.. index:: compact-start-command

**'compact start'** - Start compaction

Command: ``compact start {STORAGE_NODE} (all | {NUM_OF_TARGETS}) [{NUM_OF_COMPACT_PROC}]``

.. note:: Default ``{NUM_OF_COMPACT_PROC}`` is '3' - You can control the number of processes to execute compaction in parallel. It enables you to get maximum performance by setting an appropriate number corresponding to the number of cores.

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

.. index:: compact-suspend-command

**'compact suspend'** - Suspend a compaction job in progress

Command: ``compact suspend {STORAGE_NODE}``

::

    compact suspend storage_0@127.0.0.1
    OK

\

.. index:: compact-resume-command

**'compact resume'** - Resume a suspended compaction job

Command: ``compact resume {STORAGE_NODE}``

::

    compact resume storage_0@127.0.0.1
    OK

\

.. index:: compact-status-command

**'compact status'** - Retrieve compaction statuses

Command: ``compact status {STORAGE_NODE}``

* Compaction's status: ``idle``, ``running``, ``suspend``

::

  compact status storage_0@127.0.0.1
          current status: running
   last compaction start: 2013-03-04 12:39:47 +0900
           total targets: 64
    # of pending targets: 5
    # of ongoing targets: 3
    # of out of targets : 56

