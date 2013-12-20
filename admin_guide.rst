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
    [system config]
                   system version : 0.14.2
                   total replicas : 1
              # of successes of R : 1
              # of successes of W : 1
              # of successes of D : 1
       # of DC-awareness replicas : 0
     # of Rack-awareness replicas : 0
                        ring size : 2^128
                 ring hash (cur)  : -1
                 ring hash (prev) : -1

    [node(s) state]
    -------------------------------------------------------------------------------------------------
     type node                    state       ring (cur)    ring (prev)   when
    -------------------------------------------------------------------------------------------------
     S    storage_0@127.0.0.1     attached                                2012-09-12 14:16:10 +0900
     S    storage_1@127.0.0.1     attached                                2012-09-12 14:17:08 +0900
     S    storage_2@127.0.0.1     attached                                2012-09-12 14:17:23 +0900
     S    storage_3@127.0.0.1     attached                                2012-09-12 14:18:00 +0900


**'start' command** - Launch LeoFS-cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    start
    OK

Confirm#1 by **LeoFS-Manager** node's console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    status
    [system config]
                system version : 0.14.2
                total replicas : 1
           # of successes of R : 1
           # of successes of W : 1
           # of successes of D : 1
    # of DC-awareness replicas : 0
  # of Rack-awareness replicas : 0
                     ring size : 2^128
              ring hash (cur)  : 1428891014
              ring hash (prev) : 1428891014

    [node(s) state]
    -------------------------------------------------------------------------------------------------
     type node                    state       ring (cur)    ring (prev)   when
    -------------------------------------------------------------------------------------------------
     S    storage_0@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:16:10 +0900
     S    storage_1@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:17:08 +0900
     S    storage_2@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:17:23 +0900
     S    storage_3@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:18:00 +0900


Launch Gateway on each **LeoFS-Gateway** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ cd $LEOFS_DEPLOYED_DIR/
    $ gateway/bin/leo_gateway start


Confirm#2 by **LeoFS-Manager** master node's console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    status
    [system config]
                system version : 0.14.2
                total replicas : 1
           # of successes of R : 1
           # of successes of W : 1
           # of successes of D : 1
    # of DC-awareness replicas : 0
  # of Rack-awareness replicas : 0
                     ring size : 2^128
              ring hash (cur)  : 1428891014
              ring hash (prev) : 1428891014

    [node(s) state]
    -------------------------------------------------------------------------------------------------
     type node                    state       ring (cur)    ring (prev)   when
    -------------------------------------------------------------------------------------------------
     S    storage_0@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:16:10 +0900
     S    storage_1@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:17:08 +0900
     S    storage_2@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:17:23 +0900
     S    storage_3@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:18:00 +0900
     G    gateway_0@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:23:26 +0900

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

+--------------------------------+---------------------------------------------------------------------------------------------------+
| Command                        | Explanation                                                                                       |
+================================+===================================================================================================+
| *Storage-node related commands*                                                                                                    |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| detach `${storage-node}`       | * Remove a storage node from the LeoFS storage-cluster                                            |
|                                | * Current status: ``running`` | ``stop``                                                          |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| suspend `${storage-node}`      | * Suspend a storage node for maintenance. This command does NOT change the "routing-table (RING)" |
|                                | * Current status: ``running``                                                                     |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| resume `${storage-node}`       | * Resume a storage node                                                                           |
|                                | * Current status: ``suspended`` | ``restarted``                                                   |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| *Storage-cluster related commands*                                                                                                 |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| start                          | * Launch LeoFS after distributing the "routing-table (RING)" from Manager to Storage and Gateway  |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| rebalance                      | * Move or Copy files into the LeoFS storage-cluster due to changed RING                           |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| whereis `${file-path}`         | * Retrieve status of an assigned file                                                             |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| **Recover**                                                                                                                        |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| recover file `${file-path}`    | * Synchronize an object between nodes in charge                                                   |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| recover node `${storage-node}` | * Recover belonging target node's objects                                                         |
+--------------------------------+---------------------------------------------------------------------------------------------------+
| recover ring `${storage-node}` | * Synchronize target node's RING with Manager's RING                                              |
+--------------------------------+---------------------------------------------------------------------------------------------------+

.. index::
    detach-command

.. _detach-command-label:

**'detach'** - Storage node is removed from the LeoFS-Cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``detach ${storage-node}``

::

    detach storage_0@127.0.0.1
    OK
    rebalance
    OK

.. index::
   suspend-command

**'suspend'** - Suspend a storage node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``suspend ${storage-node}``

::

    suspend storage_0@127.0.0.1
    OK

.. index::
   resume-command

**'resume'** - Resume a storage node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``resume ${storage-node}``

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

Command: ``whereis ${file-path}``

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
| du `${storage-node}`                                      | * Display disk usages (like Unix du command)                   |
+-----------------------------------------------------------+----------------------------------------------------------------+
| du detail `${storage-node}`                               | * Display disk usages in details (like Unix du command)        |
+-----------------------------------------------------------+----------------------------------------------------------------+
| **Compaction**                                                                                                             |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact start `${storage-node}` `all | ${num_of_targets}` | * Compact raw files used by the LeoFS Storage subsystem        |
| `[${num_of_compact_proc}]`                                | * Default ${num_of_compact_proc} is '3'                        |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact suspend `${storage-node}`                         | * Suspend a compaction job in progress                         |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact resume  `${storage-node}`                         | * Resume a suspended compaction job                            |
+-----------------------------------------------------------+----------------------------------------------------------------+
| compact status  `${storage-node}`                         | * Display compaction statuses                                  |
|                                                           | * Compaction's status: ``idle``, ``running``, ``suspend``      |
+-----------------------------------------------------------+----------------------------------------------------------------+

\

**du** - Disk Usage
^^^^^^^^^^^^^^^^^^^

.. index:: du-command

**'du'** - Display disk usage (summary)

Command: ``du ${storage-node}``

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

Command: ``du detail ${storage-node}``

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

Command: ``compact start ${storage-node} all | ${num_of_targets} [${num_of_compact_proc}]``

.. note:: Default ``${num_of_compact_proc}`` is '3' - You can control the number of processes to execute compaction in parallel. It enables you to get maximum performance by setting an appropriate number corresponding to the number of cores.

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

Command: ``compact suspend ${storage-node}``

::

    compact suspend storage_0@127.0.0.1
    OK

\

.. index:: compact-resume-command

**'compact resume'** - Resume a suspended compaction job

Command: ``compact resume ${storage-node}``

::

    compact resume storage_0@127.0.0.1
    OK

\

.. index:: compact-status-command

**'compact status'** - Retrieve compaction statuses

Command: ``compact status ${storage-node}``

* Compaction's status: ``idle``, ``running``, ``suspend``

::

  compact status storage_0@127.0.0.1
          current status: running
   last compaction start: 2013-03-04 12:39:47 +0900
           total targets: 64
    # of pending targets: 5
    # of ongoing targets: 3
    # of out of targets : 56



Gateway Maintenance Commands
----------------------------

\

+------------------------------------------------------+-----------------------------------------------------------------------------------+
| Command                                              | Explanation                                                                       |
+======================================================+===================================================================================+
| purge ${file-path}                                   | * Purge a cached file if the specified file exists in the cache                   |
+------------------------------------------------------+-----------------------------------------------------------------------------------+
| remove ${gateway-node}                               | * Remove the gateway node from manager when the state of the node is 'stop'       |
+------------------------------------------------------+-----------------------------------------------------------------------------------+

.. _purge:

.. index::
   purge-command

**'purge'**
^^^^^^^^^^^

Paths used by `purge` are ruled by :ref:`this rule <s3-path-label>`

Command: ``purge ${file-path}``

::

    purge leofs.org/is/s3/comaptible/storage.key
    OK

\
\

Manager Maintenance Commands
----------------------------

+------------------------------------------------------+----------------------------------------------------------------+
| Command                                              | Explanation                                                    |
+======================================================+================================================================+
| update-managers ${manager-master} ${manager-slave}   | * Update manager's nodes to specified master/slave nodes       |
+------------------------------------------------------+----------------------------------------------------------------+
| backup-mnesia ${backup-filepath}                     | * Backup mnesia-data to specified filepath                     |
+------------------------------------------------------+----------------------------------------------------------------+
| restore-mnesia ${backup-filepath}                    | * Restore mnesia-data from specified filepath                  |
+------------------------------------------------------+----------------------------------------------------------------+



S3-API Commands
---------------

\

+------------------------------------------------------+-------------------------------------------------------------------+
| Command                                              | Explanation                                                       |
+======================================================+===================================================================+
| create-user `${user-id}`                             | * Generate an S3 key pair (AccessKeyID and SecretAccessKey)       |
+------------------------------------------------------+-------------------------------------------------------------------+
| delete-user `${user-id}`                             | * Remove a user                                                   |
+------------------------------------------------------+-------------------------------------------------------------------+
| get-users                                            | * Retrieve all the registered users                               |
+------------------------------------------------------+-------------------------------------------------------------------+
| add-endpoint `${endpoint}`                           | * Register a new S3 Endpoint                                      |
|                                                      | * LeoFS' domains are ruled by :ref:`this rule <s3-path-label>`    |
+------------------------------------------------------+-------------------------------------------------------------------+
| delete-endpoint `${endpoint}`                        | * Delete an S3 Endpoint                                           |
+------------------------------------------------------+-------------------------------------------------------------------+
| get-endpoints                                        | * Retrieve all the registered S3 Endpoints                        |
+------------------------------------------------------+-------------------------------------------------------------------+
| add-bucket `${bucket}` `${access_key_id}`            | * Create a bucket from Manager(s) and Gateway(s)                  |
+------------------------------------------------------+-------------------------------------------------------------------+
| delete-bucket `${bucket}` `${access_key_id}`         | * Remove a bucket from Manager(s), Gateway(s) and Storage-cluster |
+------------------------------------------------------+-------------------------------------------------------------------+
| get-buckets                                          | * Retrieve all of registered buckets                              |
+------------------------------------------------------+-------------------------------------------------------------------+
| chown-bucket `${bucket}` `${access_key_id}`          | * Change owner of a bucket (v0.16.5-)                             |
+------------------------------------------------------+-------------------------------------------------------------------+
| update-acl `${bucket}` `${access_key_id}`            | * Update a ACL for a bucket (v0.16.0-)                            |
| `private | public-read | public-read-write`          |                                                                   |
+------------------------------------------------------+-------------------------------------------------------------------+
| get-acl `${bucket}`                                  | * Retrieve a ACL for a bucket (v0.16.0-)                          |
+------------------------------------------------------+-------------------------------------------------------------------+


.. ### CREATE USER ###

.. _s3-create-user:

.. index::
    create-user-command

**'create-user'** - Create a user and generate an S3 key pair (AccessKeyID and SecretAccessKey)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``create-user ${user-id}``

::

    create-user test_account
    access-key-id: be8111173c8218aaf1c3
    secret-access-key: 929b09f9b794832142c59218f2907cd1c35ac163


.. ### DELETE USER ###

.. _s3-delete-user:

.. index::
    delete-user-command

**'delete-user'** - Remove a user from LeoFS manager's DB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``delete-user ${user-id}``

::

    delete-user test
    ok


.. ### GET USERS ###

.. _s3-get-users:

.. index::
    get-users-command

**'get-users'** - Retrieve users from LeoFS manager's DB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``get-users``

::

    get-users
    user_id     | access_key_id          | created_at
    ------------+------------------------+---------------------------
    _test_leofs | 05236                  | 2012-12-07 10:27:39 +0900
    leo         | 39bbad4f3b837ed209fb   | 2012-12-07 10:27:39 +0900


.. ### SET ENDPOINT ###

.. _s3-add-endpoint:

.. index::
    add-endpoint-command

**'add-endpoint'** - Register a new Endpoint
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: LeoFS domains are ruled by :ref:`this rule <s3-path-label>`

Command: ``add-endpoint ${endpoint}``

::

    add-endpoint test_account
    OK


.. ### DELETE ENDPOINTS ###

.. _s3-delete-endpoint:

.. index::
    delete-endpoint-command

**'delete-endpoint'** - Remove an Endpoint
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``delete-endpoint ${endpoint}``

::

    delete-endpoint test
    OK


.. ### GET ENDPOINTS ###

.. _s3-get-endpoints:

.. index::
    get-endpoints-command

**'get-endpoints'** - Retrieve all the registered Endpoints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``get-endpoints``

::

    get-endpoints
    endpoint         | created at
    -----------------+---------------------------
    s3.amazonaws.com | 2012-09-12 14:09:52 +0900
    localhost        | 2012-09-12 14:09:52 +0900
    leofs.org        | 2012-09-12 14:09:52 +0900

.. ### ADD BUCKET ###
.. _s3-add-bucket:

.. index::
    add-bucket-command

**'add-bucket'** - Create a bucket from Manager(s) and Gateway(s)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``add-bucket ${bucket} ${access_key_id}``

::

    add-bucket backup 05236
    OK


.. ### DELETE BUCKET ###
.. _s3-delete-bucket:

.. index::
    delete-bucket-command

**'delete-bucket'** - Remove a bucket from Manager(s), Gateway(s) and Storage-cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``delete-bucket ${bucket} ${access_key_id}``

::

    delete-bucket backup 05236
    OK


.. ### GET BUCKETS ###
.. _s3-get-buckets:

.. index::
    get-buckets-command

**'get-buckets'** - Retrieve list of Buckets registered
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``get-buckets``

::

    get-buckets
    bucket | owner     | created at
    -------+-----------+---------------------------
    backup | __leofs__ | 2012-09-12 14:30:07 +0900
    docs   | __leofs__ | 2012-09-12 14:29:30 +0900
    logs   | __leofs__ | 2012-09-12 14:29:34 +0900
    photo  | __leofs__ | 2012-09-12 14:29:26 +0900


.. ### CHANGE BUCKET OWNER ###
.. _s3-chown-bucket:

.. index::
    chown-bucket-command

**'chown-bucket'** - Change owner of a bucket (v0.16.5-)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``chown-bucket ${bucket} ${new_access_key_id}``

::

    chown-bucket backup 47ad5ca9
    OK


.. ### UPDATE ACL ###
.. _s3-update-acl:

.. index::
    update-acl-command

**'update-acl'** - Update a ACL for a bucket (v0.16.0-)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``update-acl ${bucket} ${access_key_id}``

::

    update-acl photo 05236 private
    ok

    update-acl photo 05236 public-read
    ok

    update-acl photo 05236 public-read-write
    ok


.. ### RETRIVE ACL ###
.. _s3-get-acl:

.. index::
    get-acl-command

**'get-acl'** - Retrieve a ACL for a bucket (v0.16.0-)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``get-acl ${bucket}``

::

    ## Updated acl as 'private'
    get-acl photo
    access_key_id| acls
    -------------+--------------------------
    05236        | full_contorol


    ## Updated acl as 'public-read'
    get-acl photo
    access_key_id                                   | permissions
    ------------------------------------------------+-------------------------
    http://acs.amazonaws.com/groups/global/AllUsers | read


    ## Updated acl as 'public-read-write'
    get-acl photo
    access_key_id                                   | permissions
    ------------------------------------------------+-------------------------
    http://acs.amazonaws.com/groups/global/AllUsers | read, write


Canned ACL:

.. note:: When using S3-API, LeoFS supports a set of predefined grants, known as canned ACLs. Each canned ACL has a predefined a set of grantees and permissions. The following table lists the set of canned ACLs and the associated predefined grants.

+------------------+-----------------------+------------------------------------------------------------------------+
| Canned ACL       | Applies to            | Permissions added to ACL                                               |
+==================+=======================+========================================================================+
| private          | Bucket and object     | Owner gets FULL_CONTROL. No one else has access rights (default).      |
+------------------+-----------------------+------------------------------------------------------------------------+
| public-read      | Bucket and object     | Owner gets FULL_CONTROL. The AllUsers group gets READ access.          |
+------------------+-----------------------+------------------------------------------------------------------------+
| public-read-write| Bucket and object     | Owner gets FULL_CONTROL. The AllUsers group gets READ and WRITE access.|
|                  |                       | Granting this on a bucket is generally not recommended.                |
+------------------+-----------------------+------------------------------------------------------------------------+

    * Reference:
        * `Access Control List (ACL) Overview <http://docs.aws.amazon.com/AmazonS3/latest/dev/ACLOverview.html>`_


\
\

Miscellaneous Commands
----------------------

\

+------------------------------------------------------+----------------------------------------------------------------+
| Command                                              | Explanation                                                    |
+======================================================+================================================================+
| status [${NODE}]                                     | * Retrieve status of the cluster                               |
|                                                      | * Retrieve status of the node                                  |
+------------------------------------------------------+----------------------------------------------------------------+
| history                                              | Retrieve history of operations                                 |
+------------------------------------------------------+----------------------------------------------------------------+


.. index::
    status-command

**'status'** - Retrieve status of the cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command-1: ``status``

::

    status
    [system config]
                 version : 0.12.7
     # of replicas       : 1
     # of successes of R : 1
     # of successes of W : 1
     # of successes of D : 1
               ring size : 2^128
        ring hash (cur)  : 1428891014
        ring hash (prev) : 1428891014

    [node(s) state]
    -------------------------------------------------------------------------------------------------
     type node                    state       ring (cur)    ring (prev)   when
    -------------------------------------------------------------------------------------------------
     S    storage_0@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:16:10 +0900
     S    storage_1@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:17:08 +0900
     S    storage_2@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:17:23 +0900
     S    storage_3@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:18:00 +0900
     G    gateway_0@127.0.0.1     running     1428891014    1428891014    2012-09-12 14:23:26 +0900

Command-2: ``status ${storage-node}`` OR ``status ${gateway-node}``

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
\

Upgrade LeoFS v0.14.9/v0.16.0/v0.16.5 to v0.16.8
------------------------------------------------

This section describes the way of replacement of LeoFS from v0.14.9 or v0.16.0 or v0.16.5 to v0.16.8.

Covert the configuration files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _conf-file-converter-usage:

Using the Converter
""""""""""""""""""""

.. note:: "LeoFS Configuration File Converter" is able to convert configuration files of the previous version to the format of LeoFS v0.16.8. It will be helpful to your operation.

* LeoFS's configuration: http://www.leofs.org/docs/configuration.html
* Download: https://github.com/leo-project/leofs_utils/releases/tag/0.2.0
* Repository: https://github.com/leo-project/leofs_utils/tree/develop/converter
* Usage:

.. code-block:: bash

    $ ./leofs_conf_converter -h
    Usage: leofs_conf_converter [-h] [-a <app_config>] [-e <vm_args>]
                                [-d <dest>] [-v]

      -h, --help        Show the program options
      -a, --app_config  Individual 'app config'
      -e, --vm_args     Individual 'vm.args'
      -d, --dest_file   The file name to write
      -v, --version     Show version information

    $ ./leofs_conf_converter -a test/leo_gateway.config -e test/vm.args -d leo_gateway.conf

* Result:

.. code-block:: bash

    ## LeoFS Configuration
    ##
    ## Converted the configuration
    ##     from "test/leo_gateway.config" and "test/vm.args"
    ##     to "leo_gateway.conf"
    ##
    ## ------------------------
    ## For Applications
    ## ------------------------
    sasl.sasl_error_log = ./log/sasl/sasl-error.log
    sasl.errlog_type = error
    sasl.error_logger_mf_dir = ./log/sasl
    sasl.error_logger_mf_maxbytes = 10485760
    sasl.error_logger_mf_maxfiles = 5
    http.handler = s3
    http.port = 8080
    http.num_of_acceptors = 128
    http.max_keepalive = 4096
    http.layer_of_dirs = 12
    http.ssl_port = 8443
    bucket_prop_sync_interval = 300
    large_object.max_chunked_objs  = 1000
    large_object.max_len_of_obj = 524288000
    large_object.chunked_obj_len = 5242880
    large_object.threshold_of_chunk_len = 5767168
    large_object.reading_chunked_obj_len = 5242880
    managers = ['manager_0@127.0.0.1','manager_1@127.0.0.1']

    ## The rest is omitted

    ## ------------------------
    ## For Erlang-VM
    ## ------------------------
    nodename = manager_0@127.0.0.1
    distributed_cookie = 401321b4
    erlang.kernel_poll = true
    erlang.asyc_threads = 32
    snmp_conf = ./snmp/snmpa_manager_0/leo_manager_snmp


Upgrade flow diagram
^^^^^^^^^^^^^^^^^^^^

\

.. image:: _static/images/leofs-upgrade-flow-diagram.png
   :width: 780px

* `The diagram only <http://www.leofs.org/docs/_images/leofs-upgrade-flow-diagram.png>`_


Adjust Every Path
^^^^^^^^^^^^^^^^^

* Manager: [mnesia, log-dir and queue-dir]

.. code-block:: bash

    .
    .
    .
    ## Mnesia dir
    mnesia.dir = ./work/mnesia/${IP}
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




Gateway Access-log Format (v0.16.0-)
------------------------------------

LeoFS-Gateway is able to output access-log. If you would like to use this option, you can check and set :ref:`the configuration <conf_gateway_label>`.

Sample
^^^^^^

::

    --------+-------+--------------------+-------+---------------------------------------+-----------------------+----------
    Method  | Bucket| Path               |Size   | Timestamp                             | Unix Time             | Response
    --------+-------+--------------------+-------+---------------------------------------+-----------------------+----------
    [HEAD]   photo   photo/1              0       2013-10-18 13:28:56.148269 +0900        1381206536148320        500
    [HEAD]   photo   photo/1              0       2013-10-18 13:28:56.465670 +0900        1381206536465735        404
    [HEAD]   photo   photo/city/tokyo.png 0       2013-10-18 13:28:56.489234 +0900        1381206536489289        200
    [GET]    photo   photo/1              0       2013-10-18 13:28:56.518631 +0900        1381206536518693        500
    [GET]    photo   photo/city/paris.png 0       2013-10-18 13:28:56.550376 +0900        1381206536550444        404

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
| 2             | Filename (including path)                                  |
+---------------+------------------------------------------------------------+
| 3             | File Size (byte)                                           |
+---------------+------------------------------------------------------------+
| 4             | Timestamp with timezone                                    |
+---------------+------------------------------------------------------------+
| 5             | Unixtime (including micro-second)                          |
+---------------+------------------------------------------------------------+
| 6             | Response (HTTP Status Code)                                |
+---------------+------------------------------------------------------------+

