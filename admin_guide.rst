.. LeoFS documentation master file, created by
   sphinx-quickstart on Tue Feb 21 10:38:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Administration Guide
================================
System Operation
--------------------------------
Order of system launch
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

LeoFS's system launch is very easy and simple as follows.

.. image:: _static/images/leofs-order-of-system-launch.png
   :width: 720px



Operation
""""""""""
+-------------+------------------------------------+------------------------------------------------------------+
| Order       | Command                            | Explanation                                                |
+=============+====================================+============================================================+
| 1           | $ bin/leofs_manager start          | Starting Manager Master **on Manager Master Node**         |
+-------------+------------------------------------+------------------------------------------------------------+
| 2           | $ bin/leofs_manager start          | Starting Manager Slave  **on Manager Slave Node**          |
+-------------+------------------------------------+------------------------------------------------------------+
| 3           | $ bin/leofs_storage start          | Startting Storage(s) **on each Storage Node**              |
+-------------+------------------------------------+------------------------------------------------------------+
| 4           | $ telnet $manager-master 10010     | Accessing **Manager Master Console** by telnet             |
+-------------+------------------------------------+------------------------------------------------------------+
| 5           | > start                            | Starting LeoFS (manager and storages)                      |
+-------------+------------------------------------+------------------------------------------------------------+
| 6           | > status                           | Confirm status of the cluster on Manager Master Console #1 |
+-------------+------------------------------------+------------------------------------------------------------+
| 7           | $ bin/leofs_gateway start          | Startting Gateway(s) **on each Gateway Node**              |
+-------------+------------------------------------+------------------------------------------------------------+
| 8           | > status                           | Confirm status of the cluster on Manager Master Console #2 |
+-------------+------------------------------------+------------------------------------------------------------+


Detail of system launch
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. index::
   pair: Operation; System Launch

Operate starting manager-master on **LeoFS-Manager Master** node
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

::

    $ cd $LEOFS_DEPLOYED_DIR
    $ manager_0/bin/leo_manager start

Operate starting manager-slave on **LeoFS-Manager Slave** node
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

::

    $ cd $LEOFS_DEPLOYED_DIR
    $ manager_1/bin/leo_manager start

Operate starting storage on each **LeoFS-Storage** node
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

::

    $ cd $LEOFS_DEPLOYED_DIR
    $ storage/bin/leo_storage start

LeoFS Manager Console on **LeoFS-Manager Master** node
""""""""""""""""""""""""""""""""""""""""""""""""""""""""

* 'status' command - Inspect LeoFS-cluster ::

    $ telnet 127.0.0.1 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    status
    [system config]
                 version : 0.10.1
     # of replicas       : 3
     # of successes of R : 1
     # of successes of W : 2
     # of successes of D : 2
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


**'start' command** - Launch LeoFS-cluster.
"""""""""""""""""""""""""""""""""""""""""""""

::

    start
    OK

Confirm#1 by **LeoFS-Manager** node's console
"""""""""""""""""""""""""""""""""""""""""""""""

::

    status
    [system config]
                 version : 0.10.1
     # of replicas       : 3
     # of successes of R : 1
     # of successes of W : 2
     # of successes of D : 2
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


Launch Gateway on each **LeoFS-Gateway** node.
"""""""""""""""""""""""""""""""""""""""""""""""""""

::

    $ cd $LEOFS_DEPLOYED_DIR/
    $ gateway/bin/leo_gateway start


Confirm#2 by **LeoFS-Manager** master node's console
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""

::

    status
    [system config]
                 version : 0.10.1
     # of replicas       : 3
     # of successes of R : 1
     # of successes of W : 2
     # of successes of D : 2
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


LeoFS-cluster's operation commands
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. index::
   pair: Operation; Command

LeoFS-cluster's operation commands are executed on **LeoFS-Manager Console**.

.. image:: _static/images/leofs-life-cycle.png
   :width: 720px


Command List
"""""""""""""

\

+-----------------------------------+------------------------------------------------------------+
| Command                           | Explanation                                                |
+===================================+============================================================+
| detach ${storage-node}            | Remove a storage node from the cluster                     |
+-----------------------------------+------------------------------------------------------------+
| resume ${storage-node}            | Restarting - 'nodedown' or 'stop' - storage node           |
+-----------------------------------+------------------------------------------------------------+
| suspend ${storage-node}           | Suspend a storage node                                     |
+-----------------------------------+------------------------------------------------------------+
| start                             | Launch the cluster                                         |
+-----------------------------------+------------------------------------------------------------+
| rebalance                         | Rebalance files into the cluster                           |
+-----------------------------------+------------------------------------------------------------+
| history                           | Retrieve operation histories                               |
+-----------------------------------+------------------------------------------------------------+
| compact ${storage-node}           | Compact raw files used by the LeoFS Storage subsystem      |
+-----------------------------------+------------------------------------------------------------+
| du ${storage-node}                | Display disk usages(like xnix du command)                  |
+-----------------------------------+------------------------------------------------------------+
| status                            | Retrieve status of the cluster                             |
+-----------------------------------+------------------------------------------------------------+
| whereis ${filepath}               | Retrieve status of an assigned file                        |
+-----------------------------------+------------------------------------------------------------+
| purge ${filepath}                 | Purge a cached file if the specified file existed in cache |
+-----------------------------------+------------------------------------------------------------+
| s3-gen-key ${user-id}             | Generate a S3 key pair(AccessKeyID and SecretAccessKey)    |
+-----------------------------------+------------------------------------------------------------+
| s3-set-endpoint ${endpoint}       | Register a new S3 Endpoint                                 |
+-----------------------------------+------------------------------------------------------------+
| s3-delete-endpoint ${endpoint}    | Delete a S3 Endpoint                                       |
+-----------------------------------+------------------------------------------------------------+
| s3-get-endpoints                  | Retrieve all of S3 Endpoints registered                    |
+-----------------------------------+------------------------------------------------------------+
| s3-get-buckets                    | Retrieve all of Buckets registered                         |
+-----------------------------------+------------------------------------------------------------+


.. index::
   detach-command

**'detach'** - Storage node is removed from the LeoFS-Cluster
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

::

    detach storage_0@127.0.0.1
    OK
    rebalance
    OK

.. index::
   resume-command

**'resume'** - Resume a storage node
""""""""""""""""""""""""""""""""""""""""""

::

    resume storage_0@127.0.0.1
    OK

.. index::
   suspend-command

**'suspend'** - Suspend a storage node
""""""""""""""""""""""""""""""""""""""""""

::

    suspend storage_0@127.0.0.1
    OK


.. index::
   rebalance-command

**'rebalance'** - Rebalance files into the cluster
"""""""""""""""""""""""""""""""""""""""""""""""""""

    rebalance
    OK

.. index::
   history-command

**'history'** - Retrieve operation histories
"""""""""""""""""""""""""""""""""""""""""""""""

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

.. index:: compact-command

**'compact'** - Remove logical deleted objects and metadata from Object-Storage and Metadata-Storage, respectively
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

::

    compact storage_0@127.0.0.1
    OK

.. index:: du-command

**'du'** - Retrieve a number of objects from Object-Storage
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

a. summary
::

    du storage_0@127.0.0.1
                  file size: 3762871
     number of total object: 14

b. detail
::

    du detail storage_0@127.0.0.1
               file path: /usr/local/leofs/avs/storage_0/vol1/object/leofs_object_storage_0_7.avs
               file size: 48234060240
  number of total object: 9873

               file path: /usr/local/leofs/avs/storage_0/vol1/object/leofs_object_storage_0_6.avs
               file size: 48234060240
  number of total object: 9873

               file path: /usr/local/leofs/avs/storage_0/vol1/object/leofs_object_storage_0_5.avs
               file size: 48234060240
  number of total object: 8013

               file path: /usr/local/leofs/avs/storage_0/vol1/object/leofs_object_storage_0_4.avs
               file size: 48234060240
  number of total object: 8973

               file path: /usr/local/leofs/avs/storage_0/vol1/object/leofs_object_storage_0_3.avs
               file size: 48234060240
  number of total object: 8099

               file path: /usr/local/leofs/avs/storage_0/vol1/object/leofs_object_storage_0_2.avs
               file size: 48234060240
  number of total object: 9673

               file path: /usr/local/leofs/avs/storage_0/vol1/object/leofs_object_storage_0_1.avs
               file size: 48234060240
  number of total object: 9973

               file path: /usr/local/leofs/avs/storage_0/vol1/object/leofs_object_storage_0_0.avs
               file size: 48234060240
  number of total object: 10240


.. index::
   status-command

**'status'** - Retrieve status of the cluster
"""""""""""""""""""""""""""""""""""""""""""""""

::

    [system config]
                 version : 0.10.1
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

.. _whereis:

.. index::
   whereis-command

**'whereis'**
"""""""""""""""

Paths used by `whereis` are governed by :ref:`this rule <s3-path-label>`

::

    whereis leofs.org/is/s3/comaptible/storage.key
    ------------------------------------------------------------------------------------------------------------
     del? node                 ring address    size      checksum    vclock            when
    ------------------------------------------------------------------------------------------------------------
          storage_1@127.0.0.1  207643840133    35409     4116193149  1332407492290951  2012-06-29 14:23:31 +0900
          storage_0@127.0.0.1  207643840133    35409     4116193149  1332407492290951  2012-06-29 14:23:31 +0900

.. _purge:

.. index::
   purge-command

**'purge'**
""""""""""""""""

Paths used by `purge` are governed by :ref:`this rule <s3-path-label>`

::

    purge leofs.org/is/s3/comaptible/storage.key
    OK

.. _s3-gen-key:

.. index::
   s3-gen-key-command

**'s3-gen-key'** - Generate a S3 key pair(AccessKeyID and SecretAccessKey)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Specify a user-id which must be unique across the whole system

::

   s3-gen-key test
   access-key-id: be8111173c8218aaf1c3
   secret-access-key: 929b09f9b794832142c59218f2907cd1c35ac163

.. _s3-set-endpoint:

.. index::
   s3-set-endpoint-command

**'s3-set-endpoint'** - Register a new S3 Endpoint
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Specify a new endpoint to be registered

::

   s3-set-endpoint test
   OK

.. _s3-delete-endpoint:

.. index::
   s3-delete-endpoint-command

**'s3-delete-endpoint'** - Delete a S3 Endpoint
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Specify a endpoint to be deleted

::

   s3-delete-endpoint test
   OK

.. _s3-get-endpoints:

.. index::
   s3-get-endpoints-command

**'s3-get-endpoints'** - Retrieve all of S3 Endpoints registered
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

::

    s3-get-endpoints
    endpoint         | created at
    -----------------+---------------------------
    s3.amazonaws.com | 2012-09-12 14:09:52 +0900
    localhost        | 2012-09-12 14:09:52 +0900
    leofs.org        | 2012-09-12 14:09:52 +0900

.. _s3-get-buckets:

.. index::
   s3-get-buckets-command

**'s3-get-buckets'** - Retrieve all of Buckets registered
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

::

    s3-get-buckets
    bucket | owner     | created at
    -------+-----------+---------------------------
    backup | __leofs__ | 2012-09-12 14:30:07 +0900
    docs   | __leofs__ | 2012-09-12 14:29:30 +0900
    logs   | __leofs__ | 2012-09-12 14:29:34 +0900
    photo  | __leofs__ | 2012-09-12 14:29:26 +0900

.. index::
   attach-new-storage

\
\

**Attach/Detach node from the cluster during in operation**
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
\

.. image:: _static/images/leofs-order-of-attach.png
   :width: 720px

.. index::
   detach-storage

.. image:: _static/images/leofs-order-of-detach.png
   :width: 720px


Setup SNMPA
--------------------------------

.. index::
    SNMP

Manager
^^^^^^^^^^^

a. SNMPA-Properties
""""""""""""""""""""

\

+------------------+------------------------------------+
| Property         | Value / Range                      |
+==================+====================================+
| Port             | 4020 .. 4022                       |
+------------------+------------------------------------+
| Branch           | 1.3.6.1.4.1.35450.11               |
+------------------+------------------------------------+
| snmpa_manager_0  | Port: 4020                         |
+------------------+------------------------------------+
| snmpa_manager_1  | Port: 4021                         |
+------------------+------------------------------------+
| snmpa_manager_2  | Port: 4022                         |
+------------------+------------------------------------+

b. SNMPA-Items
""""""""""""""

\

+------------------+------------------------------------+
| Branch Number    | Explanation                        |
+==================+====================================+
| 1                | Node name                          |
+------------------+------------------------------------+
| **1-min Averages**                                    |
+------------------+------------------------------------+
| 2                | # of processes                     |
+------------------+------------------------------------+
| 3                | Total memory usage                 |
+------------------+------------------------------------+
| 4                | System memory usage                |
+------------------+------------------------------------+
| 5                | Processes memory usage             |
+------------------+------------------------------------+
| 6                | ETS memory usage                   |
+------------------+------------------------------------+
| **5-min Averages**                                    |
+------------------+------------------------------------+
| 7                | # of processes                     |
+------------------+------------------------------------+
| 8                | Total memory usage                 |
+------------------+------------------------------------+
| 9                | Sysem memory usage                 |
+------------------+------------------------------------+
| 10               | Processes memory usage             |
+------------------+------------------------------------+
| 11               | ETS memory usage                   |
+------------------+------------------------------------+

c. Method of confirmation
"""""""""""""""""""""""""

::

    $ snmpwalk -v 2c -c public 127.0.0.1:4020 .1.3.6.1.4.1.35450.11
    SNMPv2-SMI::enterprises.35450.11.1.0 = STRING: "manager_0@127.0.0.1"
    SNMPv2-SMI::enterprises.35450.11.2.0 = Gauge32: 123
    SNMPv2-SMI::enterprises.35450.11.3.0 = Gauge32: 30289989
    SNMPv2-SMI::enterprises.35450.11.4.0 = Gauge32: 24256857
    SNMPv2-SMI::enterprises.35450.11.5.0 = Gauge32: 6033132
    SNMPv2-SMI::enterprises.35450.11.6.0 = Gauge32: 1914017
    SNMPv2-SMI::enterprises.35450.11.7.0 = Gauge32: 123
    SNMPv2-SMI::enterprises.35450.11.8.0 = Gauge32: 30309552
    SNMPv2-SMI::enterprises.35450.11.9.0 = Gauge32: 24278377
    SNMPv2-SMI::enterprises.35450.11.10.0 = Gauge32: 6031175
    SNMPv2-SMI::enterprises.35450.11.11.0 = Gauge32: 1935758


Storage
^^^^^^^^^^^

a. SNMPA-Properties
"""""""""""""""""""

\

+------------------+------------------------------------+
| Property         | Value / Range                      |
+==================+====================================+
| Port             | 4010 .. 4013                       |
+------------------+------------------------------------+
| Branch           | 1.3.6.1.4.1.35450.25               |
+------------------+------------------------------------+
| snmpa_storage_0  | Port: 4010                         |
+------------------+------------------------------------+
| snmpa_storage_1  | Port: 4011                         |
+------------------+------------------------------------+
| snmpa_storage_2  | Port: 4012                         |
+------------------+------------------------------------+
| snmpa_storage_3  | Port: 4013                         |
+------------------+------------------------------------+

b. SNMPA-Items
""""""""""""""

\

+------------------+------------------------------------+
| Branch Number    | Explanation                        |
+==================+====================================+
| 1                | Node name                          |
+------------------+------------------------------------+
| **VM-related values (1-min Averages)**                |
+------------------+------------------------------------+
| 2                | # of processes                     |
+------------------+------------------------------------+
| 3                | Total memory usage                 |
+------------------+------------------------------------+
| 4                | System memory usage                |
+------------------+------------------------------------+
| 5                | Processes memory usage             |
+------------------+------------------------------------+
| 6                | ETS memory usage                   |
+------------------+------------------------------------+
| **VM-related values (5-min Averages)**                |
+------------------+------------------------------------+
| 7                | # of processes                     |
+------------------+------------------------------------+
| 8                | Total memory usage                 |
+------------------+------------------------------------+
| 9                | Sysem memory usage                 |
+------------------+------------------------------------+
| 10               | Processes memory usage             |
+------------------+------------------------------------+
| 11               | ETS memory usage                   |
+------------------+------------------------------------+
| **Request-Counter (1-min Averages)**                  |
+------------------+------------------------------------+
| 12               | # of WRITEs                        |
+------------------+------------------------------------+
| 13               | # of READs                         |
+------------------+------------------------------------+
| 14               | # of DELETEs                       |
+------------------+------------------------------------+
| **Request-Counter (5-min Averages)**                  |
+------------------+------------------------------------+
| 15               | # of WRITEs                        |
+------------------+------------------------------------+
| 16               | # of READs                         |
+------------------+------------------------------------+
| 17               | # of DELETEs                       |
+------------------+------------------------------------+
| **# of objects**  (plan to support with 0.12.0)       |
+------------------+------------------------------------+
| 18               | # of active objects                |
+------------------+------------------------------------+
| 19               | Total objects                      |
+------------------+------------------------------------+
| 20               | Total size of active objects       |
+------------------+------------------------------------+
| 21               | Total size                         |
+------------------+------------------------------------+
| **MQ-related**                                        |
+------------------+------------------------------------+
| 22               | # of replication messages          |
+------------------+------------------------------------+
| 23               | # of sync-vnode messages           |
+------------------+------------------------------------+
| 24               | # of rebalance messages            |
+------------------+------------------------------------+


c. Method of confirmation
""""""""""""""""""""""""""

::

    $ snmpwalk -v 2c -c public 127.0.0.1:4010 .1.3.6.1.4.1.35450.24
    SNMPv2-SMI::enterprises.35450.24.1.0 = STRING: "storage_0@127.0.0.1"
    SNMPv2-SMI::enterprises.35450.24.2.0 = Gauge32: 227
    SNMPv2-SMI::enterprises.35450.24.3.0 = Gauge32: 33165164
    SNMPv2-SMI::enterprises.35450.24.4.0 = Gauge32: 24504020
    SNMPv2-SMI::enterprises.35450.24.5.0 = Gauge32: 8661144
    SNMPv2-SMI::enterprises.35450.24.6.0 = Gauge32: 1952903
    SNMPv2-SMI::enterprises.35450.24.7.0 = Gauge32: 227
    SNMPv2-SMI::enterprises.35450.24.8.0 = Gauge32: 33379629
    SNMPv2-SMI::enterprises.35450.24.9.0 = Gauge32: 24493694
    SNMPv2-SMI::enterprises.35450.24.10.0 = Gauge32: 8885935
    SNMPv2-SMI::enterprises.35450.24.11.0 = Gauge32: 1941680
    SNMPv2-SMI::enterprises.35450.24.12.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.13.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.14.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.15.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.16.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.17.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.18.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.19.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.20.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.21.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.22.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.23.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.24.24.0 = Gauge32: 0

Gateway
^^^^^^^^^^^

a. SNMPA-Properties
""""""""""""""""""""

\

+------------------+------------------------------------+
| Item             | Value / Range                      |
+==================+====================================+
| Port             | 4000 .. 4001                       |
+------------------+------------------------------------+
| Branch           | 1.3.6.1.4.1.35450.27               |
+------------------+------------------------------------+
| snmpa_gateway_0  | Port: 4000                         |
+------------------+------------------------------------+
| snmpa_gateway_1  | Port: 4001                         |
+------------------+------------------------------------+

b. SNMPA-Items
""""""""""""""

\

+------------------+------------------------------------+
| Branch Number    | Explanation                        |
+==================+====================================+
| 1                | Node name                          |
+------------------+------------------------------------+
| **VM-related values (1-min Averages)**                |
+------------------+------------------------------------+
| 2                | # of processes                     |
+------------------+------------------------------------+
| 3                | Total memory usage                 |
+------------------+------------------------------------+
| 4                | System memory usage                |
+------------------+------------------------------------+
| 5                | Processes memory usage             |
+------------------+------------------------------------+
| 6                | ETS memory usage                   |
+------------------+------------------------------------+
| **VM-related values (5-min Averages)**                |
+------------------+------------------------------------+
| 7                | # of processes                     |
+------------------+------------------------------------+
| 8                | Total memory usage                 |
+------------------+------------------------------------+
| 9                | Sysem memory usage                 |
+------------------+------------------------------------+
| 10               | Processes memory usage             |
+------------------+------------------------------------+
| 11               | ETS memory usage                   |
+------------------+------------------------------------+
| **Request-Counter (1-min Averages)**                  |
+------------------+------------------------------------+
| 12               | # of WRITEs                        |
+------------------+------------------------------------+
| 13               | # of READs                         |
+------------------+------------------------------------+
| 14               | # of DELETEs                       |
+------------------+------------------------------------+
| **Request-Counter (5-min Averages)**                  |
+------------------+------------------------------------+
| 15               | # of WRITEs                        |
+------------------+------------------------------------+
| 16               | # of READs                         |
+------------------+------------------------------------+
| 17               | # of DELETEs                       |
+------------------+------------------------------------+
| **Cache-related**                                     |
+------------------+------------------------------------+
| 18               | Count of cache-hit                 |
+------------------+------------------------------------+
| 19               | Count of cache-miss                |
+------------------+------------------------------------+
| 20               | Total of files (objects)           |
+------------------+------------------------------------+
| 21               | Total cached size                  |
+------------------+------------------------------------+

c. Method of confirmation
""""""""""""""""""""""""""

::

    $ snmpwalk -v 2c -c public 127.0.0.1:4000 .1.3.6.1.4.1.35450.21
    SNMPv2-SMI::enterprises.35450.21.1.0 = STRING: "gateway_0@127.0.0.1"
    SNMPv2-SMI::enterprises.35450.21.2.0 = Gauge32: 279
    SNMPv2-SMI::enterprises.35450.21.3.0 = Gauge32: 45266128
    SNMPv2-SMI::enterprises.35450.21.4.0 = Gauge32: 36653905
    SNMPv2-SMI::enterprises.35450.21.5.0 = Gauge32: 8612223
    SNMPv2-SMI::enterprises.35450.21.6.0 = Gauge32: 2276519
    SNMPv2-SMI::enterprises.35450.21.7.0 = Gauge32: 279
    SNMPv2-SMI::enterprises.35450.21.8.0 = Gauge32: 45157433
    SNMPv2-SMI::enterprises.35450.21.9.0 = Gauge32: 36385227
    SNMPv2-SMI::enterprises.35450.21.10.0 = Gauge32: 8772210
    SNMPv2-SMI::enterprises.35450.21.11.0 = Gauge32: 2261105
    SNMPv2-SMI::enterprises.35450.21.12.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.21.13.0 = Gauge32: 13
    SNMPv2-SMI::enterprises.35450.21.14.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.21.15.0 = Gauge32: 3
    SNMPv2-SMI::enterprises.35450.21.16.0 = Gauge32: 24
    SNMPv2-SMI::enterprises.35450.21.17.0 = Gauge32: 0
    SNMPv2-SMI::enterprises.35450.21.18.0 = Gauge32: 21
    SNMPv2-SMI::enterprises.35450.21.19.0 = Gauge32: 39
    SNMPv2-SMI::enterprises.35450.21.20.0 = Gauge32: 3
    SNMPv2-SMI::enterprises.35450.21.21.0 = Gauge32: 565700
