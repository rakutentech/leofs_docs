.. LeoFS documentation

.. _leofs-configuration-label:

LeoFS Configuration
======================

.. index::
   pair: Configuration; Relationship of configuration files

.. Relationship of configuration files
.. -----------------------------------

.. Each configuration of node refers a set value of other name of nodes as follows:

.. .. image:: _static/images/leofs-conf-relationship.png
..    :width: 700px


.. SNMP-related configuration refers a directory name of SNMPA as follows:

.. .. image:: _static/images/leofs-conf-relationship-snmpa.png
..    :width: 700px


LeoFS Configuration
-------------------

Each LeoFS node has one configuration files, ``leo_manager.conf``, ``leo_storage.conf`` and ``leo_gateway.conf``, which are located in the following directories:
\

+---------------+---------------------------------------------------------+
| Application   | Location                                                |
+===============+=========================================================+
| Manager-Master| $LEOFS_HOME/package/leo_manager_0/etc/leo_manager.conf  |
+---------------+---------------------------------------------------------+
| Manager-Slave | $LEOFS_HOME/package/leo_manager_1/etc/leo_manager.conf  |
+---------------+---------------------------------------------------------+
| Storage       | $LEOFS_HOME/package/leo_storage/etc/leo_storage.conf    |
+---------------+---------------------------------------------------------+
| Gateway       | $LEOFS_HOME/package/leo_gateway/etc/leo_gateway.conf    |
+---------------+---------------------------------------------------------+

* The ``*.config`` file is used to set various attributes of the application. Also, it used to pass parameters to the Erlang node such as the name and cookie of the node.


.. index::
   pair: Configuration; LeoFS Manager-Master

.. _conf_manager_label:

LeoFS Manager-Master
--------------------

.. _system-configuration-label:

The Consistency Level
^^^^^^^^^^^^^^^^^^^^^

.. note::  The consistency level is configured in this file. It should not be modified while the system is running.

+-------------+---------------------------------------------------------+
| Property    | Explanation                                             |
+=============+=========================================================+
| n           | # of replicas                                           |
+-------------+---------------------------------------------------------+
| r           | # of replicas needed for a successful READ operation    |
+-------------+---------------------------------------------------------+
| w           | # of replicas needed for a successful WRITE operation   |
+-------------+---------------------------------------------------------+
| d           | # of replicas needed for a successful DELETE operation  |
+-------------+---------------------------------------------------------+
| level_1     | # of dc-aware replicas (Supported from v1.0.0 onward)   |
+-------------+---------------------------------------------------------+
| level_2     | # of rack-aware replicas                                |
+-------------+---------------------------------------------------------+
| bit_of_ring | # of bits for the hash-ring (fixed 128bit)              |
+-------------+---------------------------------------------------------+

* A reference consistency level

+-------------+--------------------------------------------------------+
| Level       | Configuration                                          |
+=============+========================================================+
| Low         | n = 3, r = 1, w = 1, d = 1                             |
+-------------+--------------------------------------------------------+
| Middle      | n = 3, [r = 1 | r = 2], w = 2, d = 2                   |
+-------------+--------------------------------------------------------+
| High        | n = 3, [r = 2 | r = 3], w = 3, d = 3                   |
+-------------+--------------------------------------------------------+

* **Example - File: ${LEOFS_SRC}/package/manager_0/etc/leo_manager.conf**:

.. code-block:: bash


    ## --------------------------------------------------------------------
    ## MANAGER - Consistency Level
    ##     * Only set its configurations to **Manager-master**
    ##     * See: http://www.leofs.org/docs/configuration.html#the-consistency-level
    ## --------------------------------------------------------------------
    ## A number of replicas
    consistency.num_of_replicas = 3

    ## A number of replicas needed for a successful WRITE operation
    consistency.write = 2

    ## A number of replicas needed for a successful READ operation
    consistency.read = 1

    ## A number of replicas needed for a successful DELETE operation
    consistency.delete = 2

    ## A number of rack-aware replicas
    consistency.rack_aware_replicas = 0

\
\


Configuration of the Manager-Master node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**[leo_manager.conf]**

* The ``leo_manager.conf`` location: **${LEOFS_HOME}/package/manager_0/etc/leo_manager.conf**
* Modification of the required items:

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|${SLAVE-IP}     | Manager-Slave node's IP-address                        |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
|                |                                                        |
|                | - ref:${LEOFS_SRC}/apps/leo_manager/snmp/              |
|                |                                                        |
|                | - [snmpa_manager_0|snmpa_manager_1|snmpa_manager_0]    |
+----------------+--------------------------------------------------------+

.. code-block:: bash

    ## --------------------------------------------------------------------
    ## MANAGER
    ## --------------------------------------------------------------------
    ## LeoFS version
    ## system_version = 0.16.8

    ## Mode of Manager: [master, slave]
    manager.mode = master

    ## Partner of manager's alias
    manager.partner = manager_1@${SLAVE-IP}

    ## Manager-console accepatable port number
    console.port.cui  = 10010
    console.port.json = 10020

    ## Manager-console's number of acceptors
    console.acceptors.cui = 3
    console.acceptors.json = 16

    ## --------------------------------------------------------------------
    ## MANAGER - Consistency Level
    ##     * Only set its configurations to **Manager-master**
    ##     * See: http://www.leofs.org/docs/configuration.html#the-consistency-level
    ## --------------------------------------------------------------------
    ## A number of replicas
    consistency.num_of_replicas = 1

    ## A number of replicas needed for a successful WRITE operation
    consistency.write = 1

    ## A number of replicas needed for a successful READ operation
    consistency.read = 1

    ## A number of replicas needed for a successful DELETE operation
    consistency.delete = 1

    ## A number of rack-aware replicas
    consistency.rack_aware_replicas = 0

    ## --------------------------------------------------------------------
    ## MANAGER - Mnesia
    ##     * Store the info storage-cluster and the info of gateway(s)
    ##     * Store the RING and the command histories
    ## --------------------------------------------------------------------
    ## Mnesia dir
    mnesia.dir = ./work/mnesia/127.0.0.1

    ## The write threshold for transaction log dumps
    ## as the number of writes to the transaction log
    mnesia.dump_log_write_threshold = 50000

    ## Controls how often disc_copies tables are dumped from memory
    mnesia.dc_dump_limit = 40

    ## --------------------------------------------------------------------
    ## MANAGER - Log
    ## --------------------------------------------------------------------
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

    ## --------------------------------------------------------------------
    ## MANAGER - Other Directories
    ## --------------------------------------------------------------------
    ## Directory of queue for monitoring "RING"
    queue_dir = ./work/queue

    ## Directory of SNMP agent configuration
    snmp_agent = ${SNMPA-DIR}/snmp/snmpa_manager_0/LEO-MANAGER


**[Erlang VM related properties]**

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|${MASTER-IP}    | Manager-Master IP                                      |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the leofs-gateway node
    nodename = manager_0@${MASTER-IP}

    ## Cookie for distributed node communication.  All nodes in the same cluster
    ## should use the same cookie or they will not be able to communicate.
    distributed_cookie = 401321b4

    ## Enable kernel poll
    erlang.kernel_poll = true

    ## Number of async threads
    erlang.asyc_threads = 32

    ## Increase number of concurrent ports/sockets
    erlang.max_ports = 64000

    ## Set the location of crash dumps
    erlang.crash_dump = ./log/erl_crash.dump

    ## Raise the ETS table limit
    erlang.max_ets_tables = 256000

    ## Raise the default erlang process limit
    process_limit = 1048576

    ## Path of SNMP-agent configuration
    snmp_conf = ${SNMPA-DIR}/snmp/snmpa_manager_0/leo_maanager_snmp

.. index::
   pair: Configuration; LeoFS Manager-Slave

LeoFS Manager-Slave
-------------------

**Configuration of the Manager-Slave node**

**[leo_manager.conf]**

* The ``leo_manager.conf`` location: **${LEOFS_HOME}/package/manager_1/etc/leo_manager.conf**
* Modification of the required items:

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|${MASTER-IP}    | Manager-Master node's IP-address                       |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash


    ## --------------------------------------------------------------------
    ## MANAGER
    ## --------------------------------------------------------------------
    ## LeoFS version
    ## system_version = 0.16.8

    ## Mode of Manager: [master, slave]
    manager.mode = slave

    ## Partner of manager's alias
    manager.partner = manager_0@${MASTER-IP}

    ## Manager-console accepatable port number
    console.port.cui  = 10011
    console.port.json = 10021

    ## Manager-console's number of acceptors
    console.acceptors.cui = 3
    console.acceptors.json = 16

    ## --------------------------------------------------------------------
    ## MANAGER - Mnesia
    ##     * Store the info storage-cluster and the info of gateway(s)
    ##     * Store the RING and the command histories
    ## --------------------------------------------------------------------
    ## Mnesia dir
    mnesia.dir = ./work/mnesia/127.0.0.1

    ## The write threshold for transaction log dumps
    ## as the number of writes to the transaction log
    mnesia.dump_log_write_threshold = 50000

    ## Controls how often disc_copies tables are dumped from memory
    mnesia.dc_dump_limit = 40

    ## --------------------------------------------------------------------
    ## MANAGER - Log
    ## --------------------------------------------------------------------
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

    ## --------------------------------------------------------------------
    ## MANAGER - Other Directories
    ## --------------------------------------------------------------------
    ## Directory of queue for monitoring "RING"
    queue_dir = ./work/queue

    ## Directory of SNMP agent configuration
    snmp_agent = ${SNMPA-DIR}/snmp/snmpa_manager_1/LEO-MANAGER

**[Erlang VM related properties]**

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|${SLAVE-IP}     | Manager-Slave IP                                       |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the leofs-gateway node
    nodename = manager_1@${SLAVE-IP}

    ## Cookie for distributed node communication.  All nodes in the same cluster
    ## should use the same cookie or they will not be able to communicate.
    distributed_cookie = 401321b4

    ## Enable kernel poll
    erlang.kernel_poll = true

    ## Number of async threads
    erlang.asyc_threads = 32

    ## Increase number of concurrent ports/sockets
    erlang.max_ports = 64000

    ## Set the location of crash dumps
    erlang.crash_dump = ./log/erl_crash.dump

    ## Raise the ETS table limit
    erlang.max_ets_tables = 256000

    ## Raise the default erlang process limit
    process_limit = 1048576

    ## Path of SNMP-agent configuration
    snmp_conf = ${SNMPA-DIR}/snmp/snmpa_manager_1/leo_maanager_snmp

.. index::
   pair: Configuration; LeoFS Storage

.. _conf_storage_label:

LeoFS Storage
-------------

**Configuration of Storage nodes**

**[leo_storage.conf]**

* The ``leo_storage.conf`` location: **${LEOFS_HOME}/package/storage/etc/leo_storage.conf**
* Modification of the required items:

+-------------------------+--------------------------------------------------------+
|Property                 | Description                                            |
+=========================+========================================================+
|${MANAGER_MASTER_IP}     | Manager-master node's IP-address                       |
+-------------------------+--------------------------------------------------------+
|${MANAGER_SLAVE_IP}      | Manager-slave node's IP-address                        |
+-------------------------+--------------------------------------------------------+
|${OBJECT_STORAGE_DIR}    | Object Storage directory  - Default:"./avs"            |
+-------------------------+--------------------------------------------------------+
|${NUM_OF_CONTAINERS}     | # of AVS files storing objects(files).                 |
|                         |                                                        |
|                         | * AVS: ARIA(ex-LeoFS's name) Vector Storage            |
+-------------------------+--------------------------------------------------------+
|${NUM_OF_PROC_PER_OBJECT}| # of batch processes related to                        |
|                         | objects like replicating an object.                    |
+-------------------------+--------------------------------------------------------+
|${INT_PER_OBJECT_MIN}    | Minimum interval between consuming a job related to    |
|                         | objects like replicating an object.                    |
+-------------------------+--------------------------------------------------------+
|${INT_PER_OBJECT_MAX}    | Maximum interval between consuming a job related to    |
|                         | objects like replicating an object.                    |
+-------------------------+--------------------------------------------------------+
|${NUM_OF_PROC_SYNC_BY_VN}| # of batch processes related to                        |
|                         | syncing objects by vnode.                              |
+-------------------------+--------------------------------------------------------+
|${INT_SYNC_BY_VNODE_MIN} | Minimum interval between consuming a job related to    |
|                         | syncing objects by vnode.                              |
+-------------------------+--------------------------------------------------------+
|${INT_SYNC_BY_VNODE_MAX} | Maximum interval between consuming a job related to    |
|                         | syncing objects by vnode.                              |
+-------------------------+--------------------------------------------------------+
|${NUM_OF_PROC_REBALANCE} | # of batch processes related to                        |
|                         | rebalancing an object.                                 |
+-------------------------+--------------------------------------------------------+
|${INT_REBALANCE_MIN}     | Minimum interval between consuming a job related to    |
|                         | rebalancing an object.                                 |
+-------------------------+--------------------------------------------------------+
|${INT_REBALANCE_MAX}     | Maximum interval between consuming a job related to    |
|                         | rebalancing an object.                                 |
+-------------------------+--------------------------------------------------------+
|${NUM_OF_PROC_ASYNC_DEL} | # of batch processes related to                        |
|                         | deleting an object asynchronously.                     |
+-------------------------+--------------------------------------------------------+
|${INT_ASYNC_DEL_MIN}     | Minimum interval between consuming a job related to    |
|                         | deleting an object asynchronously.                     |
+-------------------------+--------------------------------------------------------+
|${INT_ASYNC_DEL_MAX}     | Maximum interval between consuming a job related to    |
|                         | deleting an object asynchronously.                     |
+-------------------------+--------------------------------------------------------+
|${NUM_OF_PROC_RECOVERY_N}| # of batch processes related to                        |
|                         | recovering an node.                                    |
+-------------------------+--------------------------------------------------------+
|${INT_RECOVERY_NODE_MIN} | Minimum interval between consuming a job related to    |
|                         | recovering an node.                                    |
+-------------------------+--------------------------------------------------------+
|${INT_RECOVERY_NODE_MAX} | Maximum interval between consuming a job related to    |
|                         | recovering an node.                                    |
+-------------------------+--------------------------------------------------------+
|${SNMPA-DIR}             | SNMPA configuration files directory                    |
|                         |                                                        |
|                         | - ref:${LEOFS_SRC}/apps/leo_storage/snmp/              |
|                         |                                                        |
|                         | - [snmpa_storage_0|snmpa_storage_1|snmpa_storage_0]    |
+-------------------------+--------------------------------------------------------+

.. code-block:: bash

    ## --------------------------------------------------------------------
    ## Manager's Node(s)
    ## --------------------------------------------------------------------
    ## Name of Manager node(s)
    ## managers = [manager_0@127.0.0.1, manager_1@127.0.0.1]
    managers = [${MANAGER_MASTER_IP}, ${MANAGER_SLAVE_IP}]

    ## --------------------------------------------------------------------
    ## STORAGE
    ## --------------------------------------------------------------------
    ## LeoFS version
    ## system_version = 0.16.8

    ## Object container
    obj_containers.path = [${OBJECT_STORAGE_DIR}]
    obj_containers.num_of_containers = [${NUM_OF_CONTAINERS}]

    ## e.g. Case of plural pathes
    ## obj_containers.path = [/var/leofs/avs/1, /var/leofs/avs/2]
    ## obj_containers.num_of_containers = [32, 64]

    ## A number of virtual-nodes for the redundant-manager
    ## num_of_vnodes = 168

    ## --------------------------------------------------------------------
    ## STORAGE - MQ
    ## --------------------------------------------------------------------
    ## A number of mq-server's processes
    mq.num_of_mq_procs = 8

    ## MQ recover per_object
    mq.recover_per_object.num_of_batch_process = ${NUM_OF_PROC_PER_OBJECT}
    mq.recover_per_object.interval_min = ${INT_PER_OBJECT_MIN}
    mq.recover_per_object.interval_max = ${INT_PER_OBJECT_MAX}

    ## MQ synchronize objects by vnode-id
    mq.sync_by_vnode_id.num_of_batch_process = ${NUM_OF_PROC_SYNC_BY_VN}
    mq.sync_by_vnode_id.interval_min = ${INT_SYNC_BY_VNODE_MIN}
    mq.sync_by_vnode_id.interval_max = ${INT_SYNC_BY_VNODE_MAX}

    ## MQ rebalance objects
    mq.rebalance.num_of_batch_process = ${NUM_OF_PROC_REBALANCE}
    mq.rebalance.interval_min = ${INT_REBALANCE_MIN}
    mq.rebalance.interval_max = ${INT_REBALANCE_MAX}

    ## MQ delete objects
    mq.delete_object.num_of_batch_process = ${NUM_OF_PROC_ASYNC_DEL}
    mq.delete_object.interval_min = ${INT_ASYNC_DEL_MIN}
    mq.delete_object.interval_max = ${INT_ASYNC_DEL_MAX}

    ## MQ recover a node's object
    mq.recovery_node.num_of_batch_process = ${NUM_OF_PROC_RECOVERY_N}
    mq.recovery_node.interval_min = ${INT_RECOVERY_NODE_MIN}
    mq.recovery_node.interval_max = ${INT_RECOVERY_NODE_MAX}


    ## --------------------------------------------------------------------
    ## STORAGE - Replication/Recovery object(s)
    ## --------------------------------------------------------------------
    ## Rack-id for the rack-awareness replica placement
    replication.rack_awareness.rack_id = []

    ## Size of stacked objects (bytes)
    replication.recovery.size_of_stacked_objs = 67108864

    ## Stacking timeout (msec)
    replication.recovery.stacking_timeout = 5000


    ## --------------------------------------------------------------------
    ## STORAGE - Log
    ## --------------------------------------------------------------------
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


    ## --------------------------------------------------------------------
    ## STORAGE - Other Directories
    ## --------------------------------------------------------------------
    ## Directory of queue for monitoring "RING"
    queue_dir  = ./work/queue

    ## Directory of SNMP agent configuration
    snmp_agent = ${SNMPA-DIR}/snmp/snmpa_storage_0/LEO-STORAGE


**[Erlang VM related properties]**

* Modification of the required items:

+-------------------------+--------------------------------------------------------+
|Property                 | Description                                            |
+=========================+========================================================+
|${STORAGE_ALIAS}         | Storage node's Alias name                              |
+-------------------------+--------------------------------------------------------+
|${STORAGE_IP}            | Storage node's IP-Address                              |
+-------------------------+--------------------------------------------------------+
|${SNMPA-DIR}             | SNMPA configuration files directory                    |
+-------------------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the leofs-storage node
    ## nodename = storage_0@127.0.0.1
    nodename = ${STORAGE_ALIAS}@${STORAGE_IP}

    ## Cookie for distributed node communication.  All nodes in the same cluster
    ## should use the same cookie or they will not be able to communicate.
    distributed_cookie = 401321b4

    ## Enable kernel poll
    erlang.kernel_poll = true

    ## Number of async threads
    erlang.asyc_threads = 32

    ## Increase number of concurrent ports/sockets
    erlang.max_ports = 64000

    ## Set the location of crash dumps
    erlang.crash_dump = ./log/erl_crash.dump

    ## Raise the ETS table limit
    erlang.max_ets_tables = 256000

    ## Raise the default erlang process limit
    process_limit = 1048576

    ## Path of SNMP-agent configuration
    snmp_conf = ${SNMPA-DIR}/snmp/snmpa_storage_0/leo_storage_snmp


.. index::
   pair: Configuration; LeoFS Gateway

.. _conf_gateway_label:

LeoFS Gateway
-------------

**Configuration of Gateway nodes**

**[leo_gateway.conf]**

* The ``leo_gateway.conf`` location: **${LEOFS_HOME}/package/gateway/etc/leo_gateway.conf**
* Modification of the required items:

+---------------------------+----------------------------------------------------------------------------------+
|Property                   | Description                                                                      |
+===========================+==================================================================================+
| **Basic items**                                                                                              |
+---------------------------+----------------------------------------------------------------------------------+
|${MANAGER_MASTER_IP}       | Manager-master node's IP-address                                                 |
+---------------------------+----------------------------------------------------------------------------------+
|${MANAGER_SLAVE_IP}        | Manager-slave node's IP-address                                                  |
+---------------------------+----------------------------------------------------------------------------------+
|${HTTP_HANDLER}            | Gateway's HTTP API to use, either ``s3`` (default) or ``rest``                   |
+---------------------------+----------------------------------------------------------------------------------+
|${LISTENING_PORT}          | Port number the Gateway uses for HTTP connections                                |
+---------------------------+----------------------------------------------------------------------------------+
|${NUM_OF_LISTENER}         | Numbers of processes listening for connections                                   |
+---------------------------+----------------------------------------------------------------------------------+
|${MAX_KEEPALIVE}           | Max number of requests allowed in a single keep-alive session. Defaults to 1024. |
+---------------------------+----------------------------------------------------------------------------------+
|${SNMPA-DIR}               | SNMPA configuration files directory                                              |
|                           |                                                                                  |
|                           | - ref:${LEOFS_SRC}/apps/leo_gateway/snmp/                                        |
|                           |                                                                                  |
|                           | - [snmpa_gateway_0|snmpa_gateway_1|snmpa_gateway_0]                              |
+---------------------------+----------------------------------------------------------------------------------+
| **Cache related items**                                                                                      |
+---------------------------+----------------------------------------------------------------------------------+
|${IS_HTTP_CACHE}           | Cache method: **http** OR **inner** *(default)*                                  |
|                           |                                                                                  |
|                           | +-----+---------------------------------------------------------------------+    |
|                           | |true |HTTP-based cache server, like *Varnish* OR *Squid*                   |    |
|                           | +-----+---------------------------------------------------------------------+    |
|                           | |false|Stores objects into the Gateway's memory. When READ, the *Etag* of   |    |
|                           | |     |the cache is compared with backend storage's *Etag*.                 |    |
|                           | |     |                                                                     |    |
|                           | |     | +----------+--------------------------------------------+           |    |
|                           | |     | |matched   | Return the cached object                   |           |    |
|                           | |     | +----------+--------------------------------------------+           |    |
|                           | |     | |unmatched | Return the object from the Storage node    |           |    |
|                           | |     | +----------+--------------------------------------------+           |    |
|                           | +-----+---------------------------------------------------------------------+    |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_RAM_CAPACITY}      | Memory cache capacity in bytes                                                   |
|                           |                                                                                  |
|                           | (ex. 4000000000 means using 4GB memory cache)                                    |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_DISC_CAPACITY}     | Disk cache capacity in bytes - default: 0 Bytes (disabled)                       |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_DISC_THRESHOLD_LEN}| When the length of the object exceeds this value, store the object on disk       |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_DISC_DIR_DATA}     | Directory for the disk cache data                                                |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_DISC_DIR_JOURNAL}  | Directory for the disk cache journal                                             |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_EXPIRE}            | Cache Expire in seconds                                                          |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_MAX_CONTENT_LEN}   | Cache Max Content Length in bytes                                                |
|                           |                                                                                  |
|                           | Note: *LeoFS-Gateway can cache up to 1MB*                                        |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_CONTENT_TYPE}      | Cache Content Type                                                               |
|                           |                                                                                  |
|                           | ex-1) [image/png, image/jpeg]                                                    |
|                           |                                                                                  |
|                           |       Caching only if its Content-Type was *"image/png"* OR *"image/jpeg"*       |
|                           |                                                                                  |
|                           | ex-2) []                                                                         |
|                           |                                                                                  |
|                           |       When value is empty, all objects are cached.                               |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_PATH_PATTERNS}     | Cache Path Pattern (regular expression)                                          |
|                           |                                                                                  |
|                           | ex-1) [/img/.+, /css/.+]                                                         |
|                           |                                                                                  |
|                           |       Caching only if its path was *"/img/\*"* or *"/css/\*"*                    |
|                           |                                                                                  |
|                           | ex-2) []                                                                         |
|                           |                                                                                  |
|                           |       When value is empty, all objects are cached.                               |
+---------------------------+----------------------------------------------------------------------------------+
| **Timeout**                                                                                                  |
+---------------------------+----------------------------------------------------------------------------------+
| Timeout value when        | +--------+------------------------------------------------------------------+    |
| requesting to a storage   | |Level   | Range                                                            |    |
|                           | +========+==================================================================+    |
|                           | |level_1 | 0 to 65,535 bytes                                                |    |
|                           | +--------+------------------------------------------------------------------+    |
|                           | |level_2 | 65,535 to 131,071 bytes                                          |    |
|                           | +--------+------------------------------------------------------------------+    |
|                           | |level_3 | 131,072 to 524,287 bytes                                         |    |
|                           | +--------+------------------------------------------------------------------+    |
|                           | |level_4 | 524,288 to 1,048,576 bytes                                       |    |
|                           | +--------+------------------------------------------------------------------+    |
|                           | |level_5 | over 1,048,576 bytes                                             |    |
|                           | +--------+------------------------------------------------------------------+    |
+---------------------------+----------------------------------------------------------------------------------+
| **Access Log**                                                                                               |
+---------------------------+----------------------------------------------------------------------------------+
| Output to a file          | Default value - *"is_enable_access_log"* is 'false' and Output destination is    |
| (v0.16.0-)                | set path, the proepery of which is set at *log_appender*.                        |
+---------------------------+----------------------------------------------------------------------------------+
| Output to Elasticsearch   | +----------------------+----------------------------------------------------+    |
| (Beta, v0.16.0-)          | |Property              | Description                                        |    |
|                           | +======================+====================================================+    |
|                           | |is_enable_esearch     | Turn on/off access-log of output to Elasticsearch  |    |
|                           | +----------------------+----------------------------------------------------+    |
|                           | |esearch_host          | Listening Elasticsearch's host                     |    |
|                           | +----------------------+----------------------------------------------------+    |
|                           | |esearch_port          | Listening Elasticsearch's port                     |    |
|                           | +----------------------+----------------------------------------------------+    |
|                           | |eearch_timeout        | Request timeout                                    |    |
|                           | +----------------------+----------------------------------------------------+    |
|                           | |esearch_bulk_duration | Bulk logs for the duration time                    |    |
|                           | +----------------------+----------------------------------------------------+    |
+---------------------------+----------------------------------------------------------------------------------+


.. code-block:: bash

    ## --------------------------------------------------------------------
    ## Manager's Node(s)
    ## --------------------------------------------------------------------
    ## Name of Manager node(s)
    ## managers = [manager_0@127.0.0.1, manager_1@127.0.0.1]
    managers = [${MANAGER_MASTER_IP}, ${MANAGER_SLAVE_IP}]

    ## --------------------------------------------------------------------
    ## GATEWAY
    ## --------------------------------------------------------------------
    ## LeoFS version
    ## system_version = 0.16.8

    ## Gateway’s HTTP API to use: [s3 | rest | embed]
    http.handler = ${HTTP_HANDLER}

    ## Port number the Gateway uses for HTTP connections
    http.port = ${LISTENING_PORT}

    ## Numbers of processes listening for connections
    http.num_of_acceptors = ${NUM_OF_LISTENER}

    ## Maximum number of requests allowed in a single keep-alive session
    http.max_keepalive = ${MAX_KEEPALIVE}

    ## Total number of virtual directories
    ## http.layer_of_dirs = 12

    ## Port number the Gateway uses for HTTPS connections
    ## http.ssl_port     = 8443

    ## SSL Certificate file
    ## http.ssl_certfile = ./etc/server_cert.pem

    ## SSL key
    ## http.ssl_keyfile  = ./etc/server_key.pem

    ## Synchronized time of a bucket property (second)
    bucket_prop_sync_interval = 300

    ## --------------------------------------------------------------------
    ## GATEWAY - Large Object
    ## --------------------------------------------------------------------
    ## Total number of chunked objects
    large_object.max_chunked_objs = 1000

    ## Maximum length of an object
    large_object.max_len_of_obj = 524288000

    ## Length of a chunked object
    large_object.chunked_obj_len = 5242880

    ## Threshold of length of a chunked object
    large_object.threshold_of_chunk_len = 5767168

    ## Reading length of a chuncked object [v0.16.8-]
    ##   * If happening timeout when copying a large object,
    ##     you will solve to set this value as less than 5MB.
    ##   * default: "large_object.chunked_obj_len" (5242880 - 5MB)
    large_object.reading_chunked_obj_len = 5242880

    ## --------------------------------------------------------------------
    ## GATEWAY - Cache
    ## --------------------------------------------------------------------
    ## If this parameter is 'true', Gateway turns on HTTP-based cache server, like Varnish OR Squid.
    ## If this parameter is 'false', Stores objects into the Gateway’s memory.
    ## When operating READ, the Etag of the cache is compared with a backend storage’s Etag.
    cache.http_cache = ${IS_HTTP_CACHE}

    ## A number of cache workers
    ## cache.cache_workers = 16

    ## Memory cache capacity in bytes
    cache.cache_ram_capacity  = ${CACHE_RAM_CAPACITY}

    ## Disk cache capacity in bytes
    cache.cache_disc_capacity = ${CACHE_DISC_CAPACITY}

    ## When the length of the object exceeds this value, store the object on disk
    cache.cache_disc_threshold_len = ${CACHE_DISC_THRESHOLD_LEN}

    ## Directory for the disk cache data
    cache.cache_disc_dir_data    = ${CACHE_DISC_DIR_DATA}

    ## Directory for the disk cache journal
    cache.cache_disc_dir_journal = ${CACHE_DISC_DIR_JOURNAL}

    ## Cache Expire in seconds
    cache.cache_expire = ${CACHE_EXPIRE}

    ## Cache Max Content Length in bytes
    cache.cache_max_content_len = ${CACHE_MAX_CONTENT_LEN}

    ## Cache Content Type(s)
    ## In case of "empty", all objects are cached.
    cache.cachable_content_type = ${CACHE_CONTENT_TYPE}

    ## Cache Path Pattern(s) (regular expression)
    ## In case of "empty", all objects are cached.
    cache.cachable_path_pattern = ${CACHE_PATH_PATTERNS}

    ## --------------------------------------------------------------------
    ## GATEWAY - Timeout
    ## --------------------------------------------------------------------
    ## Timeout value when requesting to a storage
    ## 0 to 65,535 bytes
    timeout.level_1 =  5000

    ## 65,535 to 131,071 bytes
    timeout.level_2 =  7000

    ## 131,072 to 524,287 bytes
    timeout.level_3 = 10000

    ## 524,288 to 1,048,576 bytes
    timeout.level_4 = 20000

    ## 1,048,576 bytes and over
    timeout.level_5 = 30000

    ## --------------------------------------------------------------------
    ## GATEWAY - Log
    ## --------------------------------------------------------------------
    ##
    ## Log level: [0:debug, 1:info, 2:warn, 3:error]
    log.log_level = 1

    ## Is enable access-log [true, false]
    log.is_enable_access_log = true

    ## Output log file(s) - Erlang's log
    log.erlang = ./log/erlang

    ## Output log file(s) - app
    log.app = ./log/app

    ## Output log file(s) - members of storage-cluster
    log.member_dir = ./log/ring

    ## Output log file(s) - ring
    log.ring_dir = ./log/ring

    ## Is enable Elasticsearch for access-log
    ## log.is_enable_esearch = false

    ## Node of Elasticsearch
    ## log.esearch.host = 127.0.0.1

    ## Elasticsearch listening port
    ## log.esearch.port = 9200

    ## Elasticsearch receive timeout
    ## log.esearch.timeout = 5000

    ## Duration of stack objects
    ## log.esearch.esearch_bulk_duration = 3000


    ## --------------------------------------------------------------------
    ## GATEWAY - Other Directories
    ## --------------------------------------------------------------------
    ## Directory of queue for monitoring "RING"
    queue_dir  = ./work/queue

    ## Directory of SNMP agent configuration
    snmp_agent = ${SNMPA-DIR}/snmp/snmpa_gateway_0/LEO-GATEWAY



**[Erlang VM related properties]**

* Modification of the required items:

+--------------------+--------------------------------------------------------+
|Property            | Description                                            |
+====================+========================================================+
|${GATEWAY_ALIAS}    | Gateway node's Alias name                              |
+--------------------+--------------------------------------------------------+
|${GATEWAY_IP}       | Gateway node's IP-Address                              |
+--------------------+--------------------------------------------------------+
|${SNMPA-DIR}        | SNMPA configuration files directory                    |
+--------------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the leofs-gateway node
    ## nodename = gateway_0@127.0.0.1
    nodename = ${GATEWAY_ALIAS}@${GATEWAY_IP}

    ## Cookie for distributed node communication.  All nodes in the same cluster
    ## should use the same cookie or they will not be able to communicate.
    distributed_cookie = 401321b4

    ## Enable kernel poll
    erlang.kernel_poll = true

    ## Number of async threads
    erlang.asyc_threads = 32

    ## Increase number of concurrent ports/sockets
    erlang.max_ports = 64000

    ## Set the location of crash dumps
    erlang.crash_dump = ./log/erl_crash.dump

    ## Raise the ETS table limit
    erlang.max_ets_tables = 256000

    ## Raise the default erlang process limit
    process_limit = 1048576

    ## Path of SNMP-agent configuration
    snmp_conf = ${SNMPA-DIR}/snmp/snmpa_gateway_0/leo_gateway_snmp

\

.. index::
    SNMP

SNMPA Setup
-----------

Each LeoFS node provides a built in SNMP server which allows to connect external systems, such as `Nagios <http://www.nagios.org/>`_ and `Zabbix <http://www.zabbix.com/>`_. You can retrieve various statistics as follows:

Manager
^^^^^^^

a. SNMPA Properties

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

b. SNMPA Items

\

+------------------+------------------------------------+
| Branch Number    | Description                        |
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
| 9                | System memory usage                |
+------------------+------------------------------------+
| 10               | Processes memory usage             |
+------------------+------------------------------------+
| 11               | ETS memory usage                   |
+------------------+------------------------------------+

c. Method of confirmation

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
^^^^^^^

a. SNMPA Properties

\

+------------------+------------------------------------+
| Property         | Value / Range                      |
+==================+====================================+
| Port             | 4010 .. 4013                       |
+------------------+------------------------------------+
| Branch           | 1.3.6.1.4.1.35450.24               |
+------------------+------------------------------------+
| snmpa_storage_0  | Port: 4010                         |
+------------------+------------------------------------+
| snmpa_storage_1  | Port: 4011                         |
+------------------+------------------------------------+
| snmpa_storage_2  | Port: 4012                         |
+------------------+------------------------------------+
| snmpa_storage_3  | Port: 4013                         |
+------------------+------------------------------------+

b. SNMPA Items

\

+------------------+------------------------------------+
| Branch Number    | Description                        |
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
| 9                | System memory usage                |
+------------------+------------------------------------+
| 10               | Processes memory usage             |
+------------------+------------------------------------+
| 11               | ETS memory usage                   |
+------------------+------------------------------------+
| **Request Counter**                                   |
+------------------+------------------------------------+
| 12               | # of WRITEs                        |
+------------------+------------------------------------+
| 13               | # of READs                         |
+------------------+------------------------------------+
| 14               | # of DELETEs                       |
+------------------+------------------------------------+
| **Request Counter**                                   |
+------------------+------------------------------------+
| 15               | # of WRITEs                        |
+------------------+------------------------------------+
| 16               | # of READs                         |
+------------------+------------------------------------+
| 17               | # of DELETEs                       |
+------------------+------------------------------------+
| **# of objects**                                      |
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
^^^^^^^

a. SNMPA Properties

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

b. SNMPA Items

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
| 9                | System memory usage                |
+------------------+------------------------------------+
| 10               | Processes memory usage             |
+------------------+------------------------------------+
| 11               | ETS memory usage                   |
+------------------+------------------------------------+
| **Request Counter**                                   |
+------------------+------------------------------------+
| 12               | # of WRITEs                        |
+------------------+------------------------------------+
| 13               | # of READs                         |
+------------------+------------------------------------+
| 14               | # of DELETEs                       |
+------------------+------------------------------------+
| **Request Counter**                                   |
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
