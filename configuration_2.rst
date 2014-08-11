.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: Configuration; LeoFS Storage

.. _conf_storage_label:

LeoFS Storage
-------------

**Configuration of Storage nodes**

**[leo_storage.conf]**

* The ``leo_storage.conf`` location: **{LEOFS_HOME}/package/storage/etc/leo_storage.conf**
* Modification of the required items:

+-------------------------+--------------------------------------------------------+
|Property                 | Description                                            |
+=========================+========================================================+
|{MANAGER_MASTER_IP}      | Manager-master node's IP-address                       |
+-------------------------+--------------------------------------------------------+
|{MANAGER_SLAVE_IP}       | Manager-slave node's IP-address                        |
+-------------------------+--------------------------------------------------------+
|{OBJECT_STORAGE_DIR}     | Object Storage directory  - Default:"./avs"            |
+-------------------------+--------------------------------------------------------+
|{NUM_OF_CONTAINERS}      | # of AVS files storing objects(files).                 |
|                         |                                                        |
|                         | * AVS: ARIA(ex-LeoFS's name) Vector Storage            |
+-------------------------+--------------------------------------------------------+
|{NUM_OF_PROC_PER_OBJECT} | # of batch processes related to                        |
|                         | objects like replicating an object.                    |
+-------------------------+--------------------------------------------------------+
|{INT_PER_OBJECT_MIN}     | Minimum interval between consuming a job related to    |
|                         | objects like replicating an object.                    |
+-------------------------+--------------------------------------------------------+
|{INT_PER_OBJECT_MAX}     | Maximum interval between consuming a job related to    |
|                         | objects like replicating an object.                    |
+-------------------------+--------------------------------------------------------+
|{NUM_OF_PROC_SYNC_BY_VN} | # of batch processes related to                        |
|                         | syncing objects by vnode.                              |
+-------------------------+--------------------------------------------------------+
|{INT_SYNC_BY_VNODE_MIN}  | Minimum interval between consuming a job related to    |
|                         | syncing objects by vnode.                              |
+-------------------------+--------------------------------------------------------+
|{INT_SYNC_BY_VNODE_MAX}  | Maximum interval between consuming a job related to    |
|                         | syncing objects by vnode.                              |
+-------------------------+--------------------------------------------------------+
|{NUM_OF_PROC_REBALANCE}  | # of batch processes related to                        |
|                         | rebalancing an object.                                 |
+-------------------------+--------------------------------------------------------+
|{INT_REBALANCE_MIN}      | Minimum interval between consuming a job related to    |
|                         | rebalancing an object.                                 |
+-------------------------+--------------------------------------------------------+
|{INT_REBALANCE_MAX}      | Maximum interval between consuming a job related to    |
|                         | rebalancing an object.                                 |
+-------------------------+--------------------------------------------------------+
|{NUM_OF_PROC_ASYNC_DEL}  | # of batch processes related to                        |
|                         | deleting an object asynchronously.                     |
+-------------------------+--------------------------------------------------------+
|{INT_ASYNC_DEL_MIN}      | Minimum interval between consuming a job related to    |
|                         | deleting an object asynchronously.                     |
+-------------------------+--------------------------------------------------------+
|{INT_ASYNC_DEL_MAX}      | Maximum interval between consuming a job related to    |
|                         | deleting an object asynchronously.                     |
+-------------------------+--------------------------------------------------------+
|{NUM_OF_PROC_RECOVERY_N} | # of batch processes related to                        |
|                         | recovering an node.                                    |
+-------------------------+--------------------------------------------------------+
|{INT_RECOVERY_NODE_MIN}  | Minimum interval between consuming a job related to    |
|                         | recovering an node.                                    |
+-------------------------+--------------------------------------------------------+
|{INT_RECOVERY_NODE_MAX}  | Maximum interval between consuming a job related to    |
|                         | recovering an node.                                    |
+-------------------------+--------------------------------------------------------+
|{SNMPA-DIR}              | SNMPA configuration files directory                    |
|                         |                                                        |
|                         | - ref:{LEOFS_SRC}/apps/leo_storage/snmp/               |
|                         |                                                        |
|                         | - [snmpa_storage_0|snmpa_storage_1|snmpa_storage_0]    |
+-------------------------+--------------------------------------------------------+

.. code-block:: bash

    ## --------------------------------------------------------------------
    ## Manager's Node(s)
    ## --------------------------------------------------------------------
    ## Name of Manager node(s)
    ## managers = [manager_0@127.0.0.1, manager_1@127.0.0.1]
    managers = [{MANAGER_MASTER_IP}, {MANAGER_SLAVE_IP}]

    ## --------------------------------------------------------------------
    ## STORAGE
    ## --------------------------------------------------------------------
    ## Object container
    obj_containers.path = [{OBJECT_STORAGE_DIR}]
    obj_containers.num_of_containers = [{NUM_OF_CONTAINERS}]

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
    mq.recover_per_object.num_of_batch_process = {NUM_OF_PROC_PER_OBJECT}
    mq.recover_per_object.interval_min = {INT_PER_OBJECT_MIN}
    mq.recover_per_object.interval_max = {INT_PER_OBJECT_MAX}

    ## MQ synchronize objects by vnode-id
    mq.sync_by_vnode_id.num_of_batch_process = {NUM_OF_PROC_SYNC_BY_VN}
    mq.sync_by_vnode_id.interval_min = {INT_SYNC_BY_VNODE_MIN}
    mq.sync_by_vnode_id.interval_max = {INT_SYNC_BY_VNODE_MAX}

    ## MQ rebalance objects
    mq.rebalance.num_of_batch_process = {NUM_OF_PROC_REBALANCE}
    mq.rebalance.interval_min = {INT_REBALANCE_MIN}
    mq.rebalance.interval_max = {INT_REBALANCE_MAX}

    ## MQ delete objects
    mq.delete_object.num_of_batch_process = {NUM_OF_PROC_ASYNC_DEL}
    mq.delete_object.interval_min = {INT_ASYNC_DEL_MIN}
    mq.delete_object.interval_max = {INT_ASYNC_DEL_MAX}

    ## MQ recover a node's object
    mq.recovery_node.num_of_batch_process = {NUM_OF_PROC_RECOVERY_N}
    mq.recovery_node.interval_min = {INT_RECOVERY_NODE_MIN}
    mq.recovery_node.interval_max = {INT_RECOVERY_NODE_MAX}


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
    snmp_agent = {SNMPA-DIR}/snmp/snmpa_storage_0/LEO-STORAGE


**[Erlang VM related properties]**

* Modification of the required items:

+-------------------------+--------------------------------------------------------+
|Property                 | Description                                            |
+=========================+========================================================+
|{STORAGE_ALIAS}          | Storage node's Alias name                              |
+-------------------------+--------------------------------------------------------+
|{STORAGE_IP}             | Storage node's IP-Address                              |
+-------------------------+--------------------------------------------------------+
|{SNMPA-DIR}              | SNMPA configuration files directory                    |
+-------------------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the leofs-storage node
    ## nodename = storage_0@127.0.0.1
    nodename = {STORAGE_ALIAS}@{STORAGE_IP}

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
    snmp_conf = {SNMPA-DIR}/snmp/snmpa_storage_0/leo_storage_snmp

