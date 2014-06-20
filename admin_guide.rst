.. LeoFS documentation master file, created by
   sphinx-quickstart on Tue Feb 21 10:38:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. _administration-guide-label:

Administration Guide
================================

.. _operation-flow-diagram-label:

Operation Flow Diagram
-----------------------

The documentation is the LeoFS operation flow diagram as follows:

.. image:: _static/images/leofs-flow-diagram.jpg
   :width: 780px

* `The diagram only <http://www.leofs.org/docs/_images/leofs-flow-diagram.jpg>`_

System launch order
----------------------

The documentation in this section outlines core administrative tasks and practices that operators of LeoFS will want to consider.
LeoFS' system launch is very simple:

.. image:: _static/images/leofs-order-of-system-launch.png
   :width: 640px



Explanation of the Operations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

\

+-------------+------------------------------------+------------------------------------------------------------+
| Order       | Command                            | Explanation                                                |
+=============+====================================+============================================================+
| 1           | $ bin/leofs_manager start          | Starting Manager Master **on Manager Master Node**         |
+-------------+------------------------------------+------------------------------------------------------------+
| 2           | $ bin/leofs_manager start          | Starting Manager Slave  **on Manager Slave Node**          |
+-------------+------------------------------------+------------------------------------------------------------+
| 3           | $ bin/leofs_storage start          | Starting Storage(s) **on each Storage Nodes**              |
+-------------+------------------------------------+------------------------------------------------------------+
| 4           | $ telnet $manager-master 10010     | Accessing **Manager Master Console** by telnet             |
+-------------+------------------------------------+------------------------------------------------------------+
| 5           | > start                            | Starting LeoFS (manager and storage)                       |
+-------------+------------------------------------+------------------------------------------------------------+
| 6           | > status                           | Confirm status of the cluster on Manager Master Console #1 |
+-------------+------------------------------------+------------------------------------------------------------+
| 7           | $ bin/leofs_gateway start          | Starting Gateway(s) **on each Gateway Node**               |
+-------------+------------------------------------+------------------------------------------------------------+
| 8           | > status                           | Confirm status of the cluster on Manager Master Console #2 |
+-------------+------------------------------------+------------------------------------------------------------+


System launch step by step
--------------------------

.. index::
    pair: Operation; System Launch

Start manager-master on **LeoFS-Manager Master** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ cd $LEOFS_DEPLOYED_DIR
    $ manager_0/bin/leo_manager start

Start manager-slave on **LeoFS-Manager Slave** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ cd $LEOFS_DEPLOYED_DIR
    $ manager_1/bin/leo_manager start

Start storage on each **LeoFS-Storage** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ cd $LEOFS_DEPLOYED_DIR
    $ leo_storage/bin/leo_storage start

Open LeoFS Manager Console on **LeoFS-Manager Master** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 'status' command - Inspect LeoFS-cluster ::

    $ telnet 127.0.0.1 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    status
    [System config]
                    System version : 1.0.0
                        Cluster Id : leofs_1
                             DC Id : dc_1
                    Total replicas : 3
               # of successes of R : 1
               # of successes of W : 2
               # of successes of D : 2
     # of DC-awareness replicas    : 0
                         ring size : 2^128
                 Current ring hash :
                    Prev ring hash :
    [Multi DC replication settings]
             max # of joinable DCs : 2
                # of replicas a DC : 1

    [Node(s) state]
    -------+--------------------------+--------------+----------------+----------------+----------------------------
     type  |           node           |    state     |  current ring  |   prev ring    |          updated at
    -------+--------------------------+--------------+----------------+----------------+----------------------------
      S    | storage_0@127.0.0.1      | attached     |                |                | 2014-04-03 11:28:20 +0900
      S    | storage_1@127.0.0.1      | attached     |                |                | 2014-04-03 11:28:20 +0900
      S    | storage_2@127.0.0.1      | attached     |                |                | 2014-04-03 11:28:20 +0900
      S    | storage_3@127.0.0.1      | attached     |                |                | 2014-04-03 11:28:20 +0900


**'start' command** - Launch LeoFS-cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    start
    OK

Confirm#1 by **LeoFS-Manager** node's console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    status
    [System config]
                    System version : 1.0.0
                        Cluster Id : leofs_1
                             DC Id : dc_1
                    Total replicas : 3
               # of successes of R : 1
               # of successes of W : 2
               # of successes of D : 2
     # of DC-awareness replicas    : 0
                         ring size : 2^128
                 Current ring hash : 8cd79c31
                    Prev ring hash : 8cd79c31
    [Multi DC replication settings]
             max # of joinable DCs : 2
                # of replicas a DC : 1

    [Node(s) state]
    -------+--------------------------+--------------+----------------+----------------+----------------------------
     type  |           node           |    state     |  current ring  |   prev ring    |          updated at
    -------+--------------------------+--------------+----------------+----------------+----------------------------
      S    | storage_0@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      S    | storage_1@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      S    | storage_2@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      S    | storage_3@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900


Launch Gateway on each **LeoFS-Gateway** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ cd $LEOFS_DEPLOYED_DIR/
    $ gateway/bin/leo_gateway start


Confirm#2 by **LeoFS-Manager** master node's console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    status
    [System config]
                    System version : 1.0.0
                        Cluster Id : leofs_1
                             DC Id : dc_1
                    Total replicas : 3
               # of successes of R : 1
               # of successes of W : 2
               # of successes of D : 2
     # of DC-awareness replicas    : 0
                         ring size : 2^128
                 Current ring hash : 8cd79c31
                    Prev ring hash : 8cd79c31
    [Multi DC replication settings]
             max # of joinable DCs : 2
                # of replicas a DC : 1

    [Node(s) state]
    -------+--------------------------+--------------+----------------+----------------+----------------------------
     type  |           node           |    state     |  current ring  |   prev ring    |          updated at
    -------+--------------------------+--------------+----------------+----------------+----------------------------
      S    | storage_0@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      S    | storage_1@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      S    | storage_2@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      S    | storage_3@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      G    | gateway_0@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:21 +0900
      G    | gateway_1@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:21 +0900

\

Storage Cluster Operation Commands
----------------------------------

.. index::
    pair: Operation; Command

* LeoFS-cluster's operation commands are executed on **LeoFS-Manager Console**.
* LeoFS operation flow diagram is :ref:`here <operation-flow-diagram-label>`.


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
| start                           | * Launch LeoFS after distributing the "routing-table (RING)" from Manager to Storage and Gateway  |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| rebalance                       | * Move or Copy files into the LeoFS storage-cluster due to changed RING                           |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| whereis `{FILE_PATH}`           | * Retrieve status of an assigned file                                                             |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| **Recover**                                                                                                                         |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover file `{FILE_PATH}`      | * Synchronize an object between nodes in charge                                                   |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover node `{STORAGE_NODE}`   | * Recover belonging target node's objects                                                         |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover ring `{STORAGE_NODE}`   | * Synchronize target node's RING with Manager's RING                                              |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover cluster `{CLUSTER_ID}`  | * [v1.0.0-] Synchronize objects between local-cluster with a remote-cluster                       |
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
| du `{STORAGE_NODE}`                                       | * Display disk usages (like Unix du command)                   |
+-----------------------------------------------------------+----------------------------------------------------------------+
| du detail `{STORAGE_NODE}`                                | * Display disk usages in details (like Unix du command)        |
+-----------------------------------------------------------+----------------------------------------------------------------+
| **Compaction**                                                                                                             |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact start `{STORAGE_NODE}` `all` | `{NUM_OF_TARGETS}` | * Compact raw files used by the LeoFS Storage subsystem        |
| `[{NUM_OF_COMPACT_PROC}]`                                 | * Default {NUM_OF_COMPACT_PROC} is '3'                         |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact suspend `{STORAGE_NODE}`                          | * Suspend a compaction job in progress                         |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact resume  `{STORAGE_NODE}`                          | * Resume a suspended compaction job                            |
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

**'compact start'** - Start doing compaction raw-files with targets and a number of compaction-processes

Command: ``compact start {STORAGE_NODE} all | {NUM_OF_TARGETS} [{NUM_OF_COMPACT_PROC}]``

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


MultiDC-related Commands
------------------------

\

+-------------------------------------------------------------------+-------------------------------------------------------------------------------+
| Command                                                           | Explanation                                                                   |
+===================================================================+===============================================================================+
| join-cluster `{REMOTE_MANAGER_MASTER}` `{REMOTE_MANAGER_SLAVE}`   | * [1.0.0-] Communicate between the local-cluster and a remote cluster         |
+-------------------------------------------------------------------+-------------------------------------------------------------------------------+
| remove-cluster `{REMOTE_MANAGER_MASTER}` `{REMOTE_MANAGER_SLAVE}` | * [1.0.0-] Remove communication between clusters                              |
+-------------------------------------------------------------------+-------------------------------------------------------------------------------+
| cluster-status                                                    | * [1.0.0-] Retrieve current status of clusters                                |
+-------------------------------------------------------------------+-------------------------------------------------------------------------------+

\

.. ### JOIN-CLUSTER ###

.. _join_cluster:

.. index::
    join-cluster-command

**'join-cluster'** - Communicate between the local-cluster and a remote cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``join-cluster {REMOTE_MANAGER_MASTER} {REMOTE_MANAGER_SLAVE}``

::

    join-cluster manager_c2_0@10.1.2.1 manager_c2_1@10.1.2.2
    OK

\

.. ### REMOVE-CLUSTER ###

.. _remove_cluster:

.. index::
    remove-cluster-command

**'remove-cluster'** - Remove communication between clusters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``remove-cluster {REMOTE_MANAGER_MASTER} {REMOTE_MANAGER_SLAVE}``

::

    remove-cluster manager_c2_0@10.1.2.1 manager_c2_1@10.1.2.2
    OK

\

.. ### CLUSTER-STATUS ###

.. _cluster_status:

.. index::
    cluster-status-command

**'cluster-status'** - Retrieve current status of clusters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``cluster-status``

::

    cluster-status
    cluster id |   dc id    |    status    | # of storages  |          updated at
    -----------+------------+--------------+----------------+-----------------------------
    leofs_2    | dc_2       |   running    |              8 | 2014-03-21 19:17:45 +0900





Gateway Maintenance Commands
----------------------------

\

+------------------------------------------------------+-----------------------------------------------------------------------------------+
| Command                                              | Explanation                                                                       |
+======================================================+===================================================================================+
| purge `{FILE_PATH}`                                  | * Purge a cached file if the specified file exists in the cache                   |
+------------------------------------------------------+-----------------------------------------------------------------------------------+
| remove `{GATEWAY_NODE}`                              | * Retrieve status of the cluster                                                  |
|                                                      | * Retrieve status of the node                                                     |
+------------------------------------------------------+-----------------------------------------------------------------------------------+
| dump-ring `{MANAGER}`|`{STORAGE}`|`{GATEWAY}`        | * Dump ring and memeber-info [1.0.0-pre1 or higher ]                              |
+------------------------------------------------------+-----------------------------------------------------------------------------------+
| history                                              | Retrieve history of operations                                                    |
+------------------------------------------------------+-----------------------------------------------------------------------------------+


.. index::
    status-command

**'status'** - Retrieve status of the cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command-1: ``status``

::

    status
    [System config]
                    System version : 1.0.0
                        Cluster Id : leofs_1
                             DC Id : dc_1
                    Total replicas : 3
               # of successes of R : 1
               # of successes of W : 2
               # of successes of D : 2
     # of DC-awareness replicas    : 0
                         ring size : 2^128
                 Current ring hash : 8cd79c31
                    Prev ring hash : 8cd79c31
    [Multi DC replication settings]
             max # of joinable DCs : 2
                # of replicas a DC : 1

    [Node(s) state]
    -------+--------------------------+--------------+----------------+----------------+----------------------------
     type  |           node           |    state     |  current ring  |   prev ring    |          updated at
    -------+--------------------------+--------------+----------------+----------------+----------------------------
      S    | storage_0@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      S    | storage_1@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      S    | storage_2@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      S    | storage_3@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:20 +0900
      G    | gateway_0@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:21 +0900
      G    | gateway_1@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:21 +0900

Command-2: ``status {STORAGE_NODE}`` OR ``status {GATEWAY_NODE}``

::

    status storage_0@127.0.0.1
    [config]
                version : 0.14.1
          obj-container : [[{path,"./avs"},{num_of_containers,64}]]
                log dir : ./log
    [status-1: ring]
      ring state (cur)  : 64212f2d
      ring state (prev) : 64212f2d
    [status-2: erlang-vm]
             vm version : 5.9.3.1
        total mem usage : 30886632
       system mem usage : 12774309
        procs mem usage : 18178027
          ets mem usage : 1154464
                  procs : 326/1048576
            kernel_poll : true
       thread_pool_size : 32
    [status-3: # of msgs]
       replication msgs : 0
        vnode-sync msgs : 0
         rebalance msgs : 0

::

    status gateway_0@127.0.0.1
    [config-1]
                          version : 0.14.1
                          log dir : ./log
    [config-2]
      -- http-server-related --
              using api [s3|rest] : s3
                   listening port : 8080
               listening ssl port : 8443
                   # of_acceptors : 0
      -- cache-related --
          http cache [true|false] : false
               # of cache_workers : 128
                     cache expire : 300
            cache max content len : 1048576
               ram cache capacity : 1073741824
           disc cache capacity    : 0
           disc cache threshold   : 1048576
           disc cache data dir    : ./cache/data
           disc cache journal dir : ./cache/journal
      -- large-object-related --
            max # of chunked objs : 1000
                max object length : 524288000
            chunked object length : 5242880
     threshold chunked obj length : 5767168

     [status-1: ring]
                ring state (cur)  : 64212f2d
                ring state (prev) : 64212f2d

    [status-2: erlang-vm]
                       vm version : 5.9.3.1
                  total mem usage : 48095776
                 system mem usage : 34839664
                  procs mem usage : 13261128
                    ets mem usage : 1195144
                            procs : 504/1048576
                      kernel_poll : true
                 thread_pool_size : 32

.. index::
    history-command

**'history'** - Retrieve history of operations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``history``

::

    history
    [Histories]
    1    | 2012-06-29 14:23:01 +0900 | status
    2    | 2012-06-29 14:23:02 +0900 | status
    3    | 2012-06-29 14:23:03 +0900 | attach storage_0@127.0.0.1
    4    | 2012-06-29 14:23:04 +0900 | attach storage_1@127.0.0.1
    5    | 2012-06-29 14:23:05 +0900 | attach storage_2@127.0.0.1
    6    | 2012-06-29 14:23:06 +0900 | attach storage_3@127.0.0.1
    7    | 2012-06-29 14:23:07 +0900 | start
    8    | 2012-06-29 14:23:15 +0900 | status


.. index::
    attach-new-storage

\

Upgrade your old version LeoFS to v1.0.2
----------------------------------------

This section describes the way of replacement of old LeoFS to v1.0.2

Upgrade flow diagram
^^^^^^^^^^^^^^^^^^^^

\

.. image:: _static/images/leofs-upgrade-flow-diagram.png
   :width: 780px

* `The diagram only <http://www.leofs.org/docs/_images/leofs-upgrade-flow-diagram.png>`_

\

.. note:: If you're using LeoFS v1.0.0-pre1, v0.16 or v0.14, you need to take over the configuration of ``metadata-storage`` as follows because from v1.0.0-pre2, the default configuration is ``leveldb``. So we're planning to provide the ``db-converter`` tool - from ``bitcask`` to ``leveldb`` with v1.1.0.

Takeover a part of confugurations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

\

+-------------------------------------+---------------+
| Item                                | Default value |
+=====================================+===============+
| leo_object_storage.metadata_storage | bitcask       |
+-------------------------------------+---------------+

\

Adjust Every Path
^^^^^^^^^^^^^^^^^

* Manager: [mnesia, log-dir and queue-dir]

.. code-block:: bash

    .
    .
    .
    ## Mnesia dir
    mnesia.dir = ./work/mnesia/{IP}
    .
    .
    .
    ## Log level: [0:debug, 1:info, 2:warn, 3:error]
    log.log_level = 1
    ## Output log file(s) - Erlang's log
    log.erlang = ./log/erlang
    ## Output log file(s) - app
    log.app = ./log/app
    ## Output log file(s) - members of storage-cluster
    log.member_dir = ./log/ring
    ## Output log file(s) - ring
    log.ring_dir = ./log/ring

    ## Directory of queue for monitoring "RING"
    queue_dir = ./work/queue
    ## Directory of SNMP agent configuration
    snmp_agent = ./snmp/snmpa_manager_0/LEO-MANAGER


* Storage: [obj_containers, log-dir and queue-dir]

.. code-block:: bash

    ## Object container
    obj_containers.path = [./avs]
    obj_containers.num_of_containers = [8]

    ## e.g. Case of plural pathes
    ## obj_containers.path = [/var/leofs/avs/1, /var/leofs/avs/2]
    ## obj_containers.num_of_containers = [32, 64]
    .
    .
    .
    ## Log level: [0:debug, 1:info, 2:warn, 3:error]
    log.log_level = 1
    ## Output log file(s) - Erlang's log
    log.erlang = ./log/erlang
    ## Output log file(s) - app
    log.app = ./log/app
    ## Output log file(s) - members of storage-cluster
    log.member_dir = ./log/ring
    ## Output log file(s) - ring
    log.ring_dir = ./log/ring

    ## Directory of queue for monitoring "RING"
    queue_dir = ./work/queue
    ## Directory of SNMP agent configuration
    snmp_agent = ./snmp/snmpa_storage_0/LEO-STORAGE


* Gateway: [SSL-related files, cache-related pathes, log-dir and queue-dir]

.. code-block:: bash

    ## SSL Certificate file
    http.ssl_certfile = ./etc/server_cert.pem
    ## SSL key
    http.ssl_keyfile  = ./etc/server_key.pem

    ## Directory for the disk cache data
    cache.cache_disc_dir_data    = ./cache/data
    ## Directory for the disk cache journal
    cache.cache_disc_dir_journal = ./cache/journal
    .
    .
    .
    ## Log level: [0:debug, 1:info, 2:warn, 3:error]
    log.log_level = 1
    ## Output log file(s) - Erlang's log
    log.erlang = ./log/erlang
    ## Output log file(s) - app
    log.app = ./log/app
    ## Output log file(s) - members of storage-cluster
    log.member_dir = ./log/ring
    ## Output log file(s) - ring
    log.ring_dir = ./log/ring

    ## Directory of queue for monitoring "RING"
    queue_dir = ./work/queue
    ## Directory of SNMP agent configuration
    snmp_agent = ./snmp/snmpa_gateway_0/LEO-GATEWAY


Attach/Detach node into a Storage-cluster in operation
------------------------------------------------------

This section describes the process of adding and removing nodes in a LeoFS Storage cluster.

* Adding a storage node:
    * The node can be added to the cluster once it is running. You can use the :ref:`rebalance <rebalance-command-label>` command to request a join from the Manager.
* Removing a storage node:
    * The node can be removed from the cluster when it is either running or stopped. You can use the :ref:`detach <detach-command-label>` command to remove the node.
    * After that, you need to execute the :ref:`rebalance <rebalance-command-label>` command in the Manager to actually remove the node from the storage cluster.


.. image:: _static/images/leofs-order-of-attach.png
   :width: 640px

.. index::
   detach-storage

.. image:: _static/images/leofs-order-of-detach.png
   :width: 640px




Gateway Access-log Format (v1.0.0-pre3)
---------------------------------------

LeoFS-Gateway is able to output access-log. If you would like to use this option, you can check and set :ref:`the configuration <conf_gateway_label>`.

Sample
^^^^^^

::

    --------+-------+--------------------+----------+-------+---------------------------------------+-----------------------+----------
    Method  | Bucket| Path               |Child Num |  Size | Timestamp                             | Unix Time             | Response
    --------+-------+--------------------+----------|-------+---------------------------------------+-----------------------+----------
    [HEAD]   photo   photo/1              0          0       2013-10-18 13:28:56.148269 +0900        1381206536148320        500
    [HEAD]   photo   photo/1              0          0       2013-10-18 13:28:56.465670 +0900        1381206536465735        404
    [HEAD]   photo   photo/city/tokyo.png 0          0       2013-10-18 13:28:56.489234 +0900        1381206536489289        200
    [GET]    photo   photo/1              0          1024    2013-10-18 13:28:56.518631 +0900        1381206536518693        500
    [GET]    photo   photo/city/paris.png 0          2048    2013-10-18 13:28:56.550376 +0900        1381206536550444        404
    [PUT]    logs    logs/leofs           1          5242880 2013-10-18 13:28:56.518631 +0900        1381206536518693        500
    [PUT]    logs    logs/leofs           2          5242880 2013-10-18 13:28:56.518631 +0900        1381206536518693        500
    [PUT]    logs    logs/leofs           3          5120    2013-10-18 13:28:56.518631 +0900        1381206536518693        500

Format
^^^^^^

.. note:: The format of the access log is **Tab Separated Values**.

+---------------+------------------------------------------------------------+
| Column Number | Explanation                                                |
+===============+============================================================+
| 1             | Method: [HEAD|PUT|GET|DELETE]                              |
+---------------+------------------------------------------------------------+
| 2             | Bucket                                                     |
+---------------+------------------------------------------------------------+
| 3             | Filename (including path)                                  |
+---------------+------------------------------------------------------------+
| 4             | Child number of a file                                     |
+---------------+------------------------------------------------------------+
| 5             | File Size (byte)                                           |
+---------------+------------------------------------------------------------+
| 6             | Timestamp with timezone                                    |
+---------------+------------------------------------------------------------+
| 7             | Unixtime (including micro-second)                          |
+---------------+------------------------------------------------------------+
| 8             | Response (HTTP Status Code)                                |
+---------------+------------------------------------------------------------+
