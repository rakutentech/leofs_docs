.. LeoFS documentation

.. _leofs-configuration-label:

LeoFS Configuration
======================

.. index::
   pair: Configuration; Relationship of configuration files

Relationship of configuration files
-----------------------------------

.. image:: _static/images/leofs-conf-relationship.png
   :width: 700px

.. image:: _static/images/leofs-conf-relationship-snmpa.png
   :width: 700px


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

* **Example - File: ${LEOFS_SRC}/package/leofs/manager_0/etc/app.config**:

.. code-block:: erlang

        {leo_manager,
                 [
                  %% System Configuration
                  {system, [{n, 3 },  %% # of replicas
                            {w, 2 },  %% # of replicas needed for a successful WRITE  operation
                            {r, 1 },  %% # of replicas needed for a successful READ   operation
                            {d, 2 },  %% # of replicas needed for a successful DELETE operation
                            {level_1, 0}, %% # of DC-awareness replicas (Plan to support with v1.0.0)
                            {level_2, 0}, %% # of Rack-awareness replicas
                            {bit_of_ring, 128}
                           ]},


\
\


Configuration of the Manager-Master node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* **File-1: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_0/etc/app.config**

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

.. code-block:: erlang

    [
        {sasl, [
                {sasl_error_logger, {file, "./log/sasl-error.log"}},
                {errlog_type, error},
                {error_logger_mf_dir, "./log/sasl"},
                {error_logger_mf_maxbytes, 10485760}, % 10 MB max file size
                {error_logger_mf_maxfiles, 5}         % 5 files max
               ]},
        {mnesia, [
                  {dir, "./work/mnesia/${IP}"},
                  {dump_log_write_threshold, 50000},
                  {dc_dump_limit,            40}
                 ]},
        {leo_manager, [
                   %% == System Ver ==
                   {system_version, "0.14.6" },

                   %% == System Configuration ==
                   %%
                   %% n: # of replicated files
                   %% w: # of successes of write-operation
                   %% r: # of successes of read-operation
                   %% d: # of successes of delete-operation
                   %% bit_of_ring: Ring size - 128 = 2^128
                   {system, [{n, 1 },
                             {w, 1 },
                             {r, 1 },
                             {d, 1 },
                             {bit_of_ring, 128},
                             {level_1, 0 },
                             {level_2, 0 }
                            ]},

                   %% == Available Commands ==
                   {available_commands, all },

                   %% == Manager Properties ==
                   %% Mode of server - [master|slave]
                   {manager_mode,     master },
                   %% Partner of manager's alias
                   {manager_partners, ["manager_1@127.0.0.1"] },
                   %% Manager accepatable port number
                   {port_cui,         10010 },
                   {port_json,        10020 },

                   %% # of acceptors
                   {num_of_acceptors_cui,   3},
                   {num_of_acceptors_json, 16},

                   %% Compaction: # of execution of conncurrent
                   {num_of_compact_proc, 3 },

                   %% == Log-specific properties ==
                   %%
                   %% Log output level
                   %%   0: debug
                   %%   1: info
                   %%   2: warning
                   %%   3: error
                   {log_level,    1 },
                   %% Log appender - [file]
                   {log_appender, [
                                   {file, [{path, "./log/app"}]}
                                  ]},

                   %% == Directories ==
                   %%
                   %% Directory of log output
                   {log_dir,          "./log"},
                   %% Directory of mq's db-file
                   {queue_dir,        "./work/queue"},
                   %% Directory of snmp-agent
                   {snmp_agent,       "./snmp/snmpa_manager_0/LEO-MANAGER"}
                  ]},
    ].


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_0/etc/vm.args**

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|${MASTER-IP  }  | Manager-Master IP                                      |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the node
    -name manager_0@${MASTER-IP}

    ## Cookie for distributed erlang
    -setcookie 401321b4

    ## Heartbeat management; auto-restarts VM if it dies or becomes unresponsive
    ## (Disabled by default..use with caution!)
    ##-heart

    ## Enable kernel poll and a few async threads
    +K true
    +A 32

    ## Increase number of concurrent ports/sockets
    ##-env ERL_MAX_PORTS 4096

    ## Tweak GC to run more often
    ##-env ERL_FULLSWEEP_AFTER 10

    ## SNMP Config file
    -config ./snmp/${SNMPA-DIR}/leo_manager_snmp

    ## set up the node with the -hidden flag
    -hidden

.. index::
   pair: Configuration; LeoFS Manager-Slave

LeoFS Manager-Slave
-------------------

**Configuration of the Manager-Slave node**

* **File-1: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_0/etc/app.config**

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|${MASTER-IP}    | Manager-Master node's IP-address                       |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: erlang

    [

        {leo_manager,
                 [

                  %% Manager Configuration
                  {manager_mode,     slave },
                  {manager_partners, ["manager_0@${MASTER-IP}"] },
                  {port,             10011 },
                  {num_of_acceptors, 3},

                  %% Directories
                  {log_dir,          "./log"},
                  {queue_dir,        "./work/queue"},
                  {snmp_agent,       "./snmp/${SNMPA-DIR}/LEO-MANAGER"}
                 ]}
    ].


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_1/etc/vm.args**

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|${SLAVE-IP}     | Manager-Slave IP                                       |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the node
    -name manager_0@${SLAVE-IP}

    ## Cookie for distributed erlang
    -setcookie 401321b4

    ## Heartbeat management; auto-restarts VM if it dies or becomes unresponsive
    ## (Disabled by default..use with caution!)
    ##-heart

    ## Enable kernel poll and a few async threads
    +K true
    +A 32

    ## Increase number of concurrent ports/sockets
    ##-env ERL_MAX_PORTS 4096

    ## Tweak GC to run more often
    ##-env ERL_FULLSWEEP_AFTER 10

    ## SNMP Config file
    -config ./snmp/${SNMPA-DIR}/leo_manager_snmp

    ## set up the node with the -hidden flag
    -hidden


.. index::
   pair: Configuration; LeoFS Storage

.. _conf_storage_label:

LeoFS Storage
-------------

**Configuration of Storage nodes**

* **File-1: ${LEOFS_DEPLOYED_DIR}/package/leofs/storage/etc/app.config**

+-------------------------+--------------------------------------------------------+
|Property                 | Description                                            |
+=========================+========================================================+
|${OBJECT_STORAGE_DIR}    | Object Storage directory  - Default:"./avs"            |
+-------------------------+--------------------------------------------------------+
|${MANAGER_MASTER_IP}     | Manager-master node's IP-address                       |
+-------------------------+--------------------------------------------------------+
|${MANAGER_SLAVE_IP}      | Manager-slave node's IP-address                        |
+-------------------------+--------------------------------------------------------+
|${SNMPA-DIR}             | SNMPA configuration files directory                    |
|                         |                                                        |
|                         | - ref:${LEOFS_SRC}/apps/leo_storage/snmp/              |
|                         |                                                        |
|                         | - [snmpa_storage_0|snmpa_storage_1|snmpa_storage_0]    |
+-------------------------+--------------------------------------------------------+

.. code-block:: erlang

    {leo_storage, [
                   %% == System Ver ==
                   {system_version, "0.14.3" },

                   %% == Storage Configuration ==
                   %%
                   %% Object containers properties:
                   %% @param path              - Directory of object-containers
                   %% @param num_of_containers - # of object-containers
                   %%
                   %% Notes:
                   %%   If you set up LeoFS on 'development', default value - "./avs" - is OK.
                   %%   If you set up LeoFS on 'production' or 'staging', You should need to change "volume",
                   %%       And We recommend volume's partition is XFS.
                   %%
                   {obj_containers,     [[{path, ${OBJECT_STORAGE_DIR}}, {num_of_containers, 64}]] },

                   %% leo-manager's nodes
                   {managers,           [${MANAGER_MASTER_IP}, ${MANAGER_SLAVE_IP}] },

                   %% # of virtual-nodes
                   {num_of_vnodes,      168 },

                   %% # of mq-server's processes
                   {num_of_mq_procs,    8 },

                   %% mq - queues consumption's intervals
                   %% - per_object
                   {cns_interval_per_object_min, 0  },
                   {cns_interval_per_object_max, 16 },
                   %% - sync_by_vnode_id
                   {cns_interval_sync_by_vnode_id_min, 0  },
                   {cns_interval_sync_by_vnode_id_max, 16 },
                   %% - for rebalance
                   {cns_interval_rebalance_min, 0  },
                   {cns_interval_rebalance_max, 16 },
                   %% - async deletion objects (after remove a bucket)
                   {cns_interval_async_deletion_min, 0  },
                   {cns_interval_async_deletion_max, 16 },
                   %% - recovery node
                   {cns_interval_recovery_node_min,  0  },
                   {cns_interval_recovery_node_max,  16 },

                   %% == For Ordning-Reda ==
                   %% Size of stacked objects (bytes)
                   {size_of_stacked_objs,    67108864 },
                   %% Stacking timeout (msec)
                   {stacking_timeout,        5000 },

                   %% == Log-specific properties ==
                   %%
                   {log_level,    1 },
                   {log_appender, [
                                   {file, [{path, "./log/app"}]}
                                  ]},

                   %% == Directories ==
                   %%
                   %% Directory of log output
                   {log_dir,     "./log"},
                   %% Directory of mq's db-files
                   {queue_dir,   "./work/queue"},
                   %% Directory of SNMP-Agent
                   {snmp_agent,  ${SNMPA-DIR}}
                  ]},

    {leo_object_storage, [{profile, false},
                          {metadata_storage, 'bitcask'},

                          %% Strict comparison of object's checksum with metadata's it
                          %% (default:false)
                          {is_strict_check, false }
                         ]},


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/storage/etc/vm.args**

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

    ## Name of the node
    -name ${STORAGE_ALIAS}@${STORAGE_IP}

    ## Cookie for distributed erlang
    -setcookie 401321b4

    ## Heartbeat management; auto-restarts VM if it dies or becomes unresponsive
    ## (Disabled by default..use with caution!)
    ##-heart

    ## Enable kernel poll and a few async threads
    +K true
    +A 32

    ## Increase number of concurrent ports/sockets
    ##-env ERL_MAX_PORTS 4096

    ## Tweak GC to run more often
    ##-env ERL_FULLSWEEP_AFTER 10

    ## SNMP Config file
    -config ./snmp/${SNMPA-DIR}/leo_storage_snmp

    ## Sets the maximum number of concurrent processes for this system
    +P 1048576

    ## set up the node with the -hidden flag
    -hidden


.. index::
   pair: Configuration; LeoFS Gateway

.. _conf_gateway_label:

LeoFS Gateway
-------------

**Configuration of Gateway nodes**

* **File-1: ${LEOFS_DEPLOYED_DIR}/package/leofs/gateway/etc/app.config**

+---------------------------+----------------------------------------------------------------------------------+
|Property                   | Description                                                                      |
+===========================+==================================================================================+
|${LISTENING_PORT}          | Port number the Gateway uses for HTTP connections                                |
+---------------------------+----------------------------------------------------------------------------------+
|${NUM_OF_LISTENER}         | Numbers of processes listening for connections                                   |
+---------------------------+----------------------------------------------------------------------------------+
|${MANAGER_MASTER_IP}       | Manager-master node's IP-address                                                 |
+---------------------------+----------------------------------------------------------------------------------+
|${MANAGER_SLAVE_IP}        | Manager-slave node's IP-address                                                  |
+---------------------------+----------------------------------------------------------------------------------+
|${SNMPA-DIR}               | SNMPA configuration files directory                                              |
|                           |                                                                                  |
|                           | - ref:${LEOFS_SRC}/apps/leo_gateway/snmp/                                        |
|                           |                                                                                  |
|                           | - [snmpa_gateway_0|snmpa_gateway_1|snmpa_gateway_0]                              |
+---------------------------+----------------------------------------------------------------------------------+
|${HTTP_HANDLER}            | Gateway's HTTP API to use, either ``s3`` (default) or ``rest``                   |
+---------------------------+----------------------------------------------------------------------------------+
|${MAX_KEEPALIVE}           | Max number of requests allowed in a single keep-alive session. Defaults to 1024. |
+---------------------------+----------------------------------------------------------------------------------+
| *Cache related items*                                                                                        |
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
|${CACHE_EXPIRE}            | [**cache-mode:http**] HTTP Cache Expire in seconds                               |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_MAX_C_LEN}         | [**cache-mode:http**] HTTP Cache Max Content Length in bytes                     |
|                           |                                                                                  |
|                           | Note: *LeoFS-Gateway can cache up to 1MB*                                        |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_C_TYPE}            | [**cache-mode:http**] HTTP Cache Content Type                                    |
|                           |                                                                                  |
|                           | ex-1) ["image/png", "image/jpeg"]                                                |
|                           |                                                                                  |
|                           |       Caching only if its Content-Type was *"image/png"* OR *"image/jpeg"*       |
|                           |                                                                                  |
|                           | ex-2) []                                                                         |
|                           |                                                                                  |
|                           |       When value is empty, all objects are cached.                               |
+---------------------------+----------------------------------------------------------------------------------+
|${CACHE_PATH_PATTERNS}     | [**cache-mode:http**] HTTP Cache Path Pattern (regular expression)               |
|                           |                                                                                  |
|                           | ex-1) ["/img/.+", "/css/.+"]                                                     |
|                           |                                                                                  |
|                           |       Caching only if its path was *"/img/\*"* or *"/css/\*"*                    |
|                           |                                                                                  |
|                           | ex-2) []                                                                         |
|                           |                                                                                  |
|                           |       When value is empty, all objects are cached.                               |
+---------------------------+----------------------------------------------------------------------------------+


.. code-block:: erlang

    [
        {sasl, [
                {sasl_error_logger, {file, "./log/sasl-error.log"}},
                {errlog_type, error},
                {error_logger_mf_dir, "./log/sasl"},
                {error_logger_mf_maxbytes, 10485760}, % 10 MB max file size
                {error_logger_mf_maxfiles, 5}         % 5 files max
               ]},

        {leo_gateway, [
                %% System Ver
                {system_version, "0.14.1" },

                %% Gateway Properties:
                {http, [
                        %% http-handler (API) - [s3 or rest]:
                        {handler, ${HTTP_HANDLER}},
                        %% Gateway port number:
                        {port, ${LISTENING_PORT} },
                        %% # of acceptors:
                        {num_of_acceptors, ${NUM_OF_LISTENER} },
                        %% max keep-alive:
                        {max_keepalive, ${MAX_KEEPALIVE} },
                        %% max # of layer of directories:
                        {layer_of_dirs, {1, 12} },
                        %% ssl related:
                        {ssl_port,     8443 },
                        {ssl_certfile, "./etc/server_cert.pem" },
                        {ssl_keyfile,  "./etc/server_key.pem" }
                       ]},

                %% Large-object-related properties:
                {large_object, [
                                {max_chunked_objs,  1000  },
                                {max_len_for_obj,   524288000 }, %% 500.0MB
                                {chunked_obj_len,   5242880 },   %% 5.0MB
                                {threshold_obj_len, 5767168 }    %% 5.5MB
                               ]},

                %% Cache-related properties:
                {cache, [
                         %% Use HTTP-cache ?
                         {http_cache, ${IS_HTTP_CACHE}},
                         %% # of Cache workers
                         {cache_workers, 128 },

                         %% Total of Cache capacity into the RAM (MB)
                         {cache_ram_capacity,  ${CACHE_RAM_CAPACITY} },
                         %% Total of Cache capacity into the Disc (MB)
                         {cache_disc_capacity, ${CACHE_DISC_CAPACITY} },

                         %% Disc-cache's threshold length which value is exceeded
                         %% when an object is stored into the disc
                         {cache_disc_threshold_len, ${CACHE_DISC_THRESHOLD_LEN} },
                         %% Disc-cache's directory
                         {cache_disc_dir_data,    ${CACHE_DISC_DIR_DATA} },
                         {cache_disc_dir_journal, ${CACHE_DISC_DIR_JOURNAL} },

                         %% Cache expire time. (sec)
                         {cache_expire, ${CACHE_EXPIRE} },
                         %% Acceptable maximum content length (MB)
                         {cache_max_content_len, ${CACHE_MAX_C_LEN} },
                         %% Acceptable content-type(s)
                         {cachable_content_type, ${CACHE_C_TYPE} },
                         %% Acceptable URL-Pattern(s)
                         {cachable_path_pattern, ${CACHE_PATH_PATTERNS} }
                        ]},

                %% Timeout when request from gateway to storage ==
                {timeout, [
                           {level_1,  5000},
                           {level_2,  7000},
                           {level_3, 10000},
                           {level_4, 20000},
                           {level_5, 30000}
                          ]},

                %% Manager - leo-manager's nodes
                {managers, [${MANAGER_MASTER_IP}, ${MANAGER_SLAVE_IP}] },

                %% Log-specific properties
                %%   - Log output level
                %%         0: debug
                %%         1: info
                %%         2: warning
                %%         3: error
                {log_level,    1 },
                %% Log appender - [file]
                {log_appender, [
                                {file, [{path, "./log/app"}]}
                               ]},

                %% Directory of log output
                {log_dir,     "./log"},
                %% Directory of mq's db-files
                {queue_dir,   "./work/queue"},
                %% Directory of snmp-agent
                {snmp_agent,  "./snmp/snmpa_gateway_0/LEO-GATEWAY"}
               ]},


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/gateway/etc/vm.args**

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

    ## Name of the node
    -name ${GATEWAY_ALIAS}@${GATEWAY_IP}

    ## Cookie for distributed erlang
    -setcookie 401321b4

    ## Heartbeat management; auto-restarts VM if it dies or becomes unresponsive
    ## (Disabled by default..use with caution!)
    ##-heart

    ## Enable kernel poll and a few async threads
    +K true
    +A 32

    ## Increase number of concurrent ports/sockets
    ##-env ERL_MAX_PORTS 4096

    ## Tweak GC to run more often
    ##-env ERL_FULLSWEEP_AFTER 10

    ## SNMP Config file
    -config ./snmp/${SNMPA-DIR}/leo_gateway_snmp

    ## Sets the maximum number of concurrent processes for this system
    +P 1048576

    ## set up the node with the -hidden flag
    -hidden

\


.. index::
    SNMP

SNMPA Setup
-----------

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
| 9                | Sysem memory usage                 |
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
| 9                | Sysem memory usage                 |
+------------------+------------------------------------+
| 10               | Processes memory usage             |
+------------------+------------------------------------+
| 11               | ETS memory usage                   |
+------------------+------------------------------------+
| **Request Counter (1-min Averages)**                  |
+------------------+------------------------------------+
| 12               | # of WRITEs                        |
+------------------+------------------------------------+
| 13               | # of READs                         |
+------------------+------------------------------------+
| 14               | # of DELETEs                       |
+------------------+------------------------------------+
| **Request Counter (5-min Averages)**                  |
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
| 9                | Sysem memory usage                 |
+------------------+------------------------------------+
| 10               | Processes memory usage             |
+------------------+------------------------------------+
| 11               | ETS memory usage                   |
+------------------+------------------------------------+
| **Request Counter (1-min Averages)**                  |
+------------------+------------------------------------+
| 12               | # of WRITEs                        |
+------------------+------------------------------------+
| 13               | # of READs                         |
+------------------+------------------------------------+
| 14               | # of DELETEs                       |
+------------------+------------------------------------+
| **Request Counter (5-min Averages)**                  |
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
