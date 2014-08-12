.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: Configuration; LeoFS Gateway

.. _conf_gateway_label:

LeoFS Gateway
-------------

**Configuration of LeoFS Gateway nodes**

**[leo_gateway.conf]**

* The ``leo_gateway.conf`` location: **{LEOFS_HOME}/package/gateway/etc/leo_gateway.conf**
* Modification of the required items:

+---------------------------+--------------------------------------------------------------------------------------+
|Property                   | Description                                                                          |
+===========================+======================================================================================+
| **Basic items**                                                                                                  |
+---------------------------+--------------------------------------------------------------------------------------+
|{MANAGER_MASTER_IP}        | Manager-master node's IP-address                                                     |
+---------------------------+--------------------------------------------------------------------------------------+
|{MANAGER_SLAVE_IP}         | Manager-slave node's IP-address                                                      |
+---------------------------+--------------------------------------------------------------------------------------+
|{HTTP_HANDLER}             | LeoFS Gateway's HTTP API to use, either ``s3`` (default) or ``rest``                 |
+---------------------------+--------------------------------------------------------------------------------------+
|{LISTENING_PORT}           | Port number the LeoFS Gateway uses for HTTP connections                              |
+---------------------------+--------------------------------------------------------------------------------------+
|{NUM_OF_LISTENER}          | Numbers of processes listening for connections                                       |
+---------------------------+--------------------------------------------------------------------------------------+
|{MAX_KEEPALIVE}            | Max number of requests allowed in a single keep-alive session. Defaults to 1024.     |
+---------------------------+--------------------------------------------------------------------------------------+
|{SNMPA-DIR}                | SNMPA configuration files directory                                                  |
|                           |                                                                                      |
|                           | - ref:{LEOFS_SRC}/apps/leo_gateway/snmp/                                             |
|                           |                                                                                      |
|                           | - [snmpa_gateway_0|snmpa_gateway_1|snmpa_gateway_0]                                  |
+---------------------------+--------------------------------------------------------------------------------------+
| **Cache related items**                                                                                          |
+---------------------------+--------------------------------------------------------------------------------------+
|{IS_HTTP_CACHE}            | Cache method: **http** OR **inner** *(default)*                                      |
|                           |                                                                                      |
|                           | +-----+-------------------------------------------------------------------------+    |
|                           | |true |HTTP-based cache server, like *Varnish* OR *Squid*                       |    |
|                           | +-----+-------------------------------------------------------------------------+    |
|                           | |false|Stores objects into the LeoFS Gateway's memory. When READ, the *Etag* of |    |
|                           | |     |the cache is compared with backend storage's *Etag*.                     |    |
|                           | |     |                                                                         |    |
|                           | |     | +----------+----------------------------------------------+             |    |
|                           | |     | |matched   | Return the cached object                     |             |    |
|                           | |     | +----------+----------------------------------------------+             |    |
|                           | |     | |unmatched | Return the object from the LeoFS Storage node|             |    |
|                           | |     | +----------+----------------------------------------------+             |    |
|                           | +-----+-------------------------------------------------------------------------+    |
+---------------------------+--------------------------------------------------------------------------------------+
|{CACHE_RAM_CAPACITY}       | Memory cache capacity in bytes                                                       |
|                           |                                                                                      |
|                           | (ex. 4000000000 means using 4GB memory cache)                                        |
+---------------------------+--------------------------------------------------------------------------------------+
|{CACHE_DISC_CAPACITY}      | Disk cache capacity in bytes - default: 0 Bytes (disabled)                           |
+---------------------------+--------------------------------------------------------------------------------------+
|{CACHE_DISC_THRESHOLD_LEN} | When the length of the object exceeds this value, store the object on disk           |
+---------------------------+--------------------------------------------------------------------------------------+
|{CACHE_DISC_DIR_DATA}      | Directory for the disk cache data                                                    |
+---------------------------+--------------------------------------------------------------------------------------+
|{CACHE_DISC_DIR_JOURNAL}   | Directory for the disk cache journal                                                 |
+---------------------------+--------------------------------------------------------------------------------------+
|{CACHE_EXPIRE}             | Cache Expire in seconds                                                              |
+---------------------------+--------------------------------------------------------------------------------------+
|{CACHE_MAX_CONTENT_LEN}    | Cache Max Content Length in bytes                                                    |
|                           |                                                                                      |
|                           | Note: *LeoFS-Gateway can cache up to 1MB*                                            |
+---------------------------+--------------------------------------------------------------------------------------+
|{CACHE_CONTENT_TYPE}       | Cache Content Type                                                                   |
|                           |                                                                                      |
|                           | ex-1) [image/png, image/jpeg]                                                        |
|                           |                                                                                      |
|                           |       Caching only if its Content-Type was *"image/png"* OR *"image/jpeg"*           |
|                           |                                                                                      |
|                           | ex-2) []                                                                             |
|                           |                                                                                      |
|                           |       When value is empty, all objects are cached.                                   |
+---------------------------+--------------------------------------------------------------------------------------+
|{CACHE_PATH_PATTERNS}      | Cache Path Pattern (regular expression)                                              |
|                           |                                                                                      |
|                           | ex-1) [/img/.+, /css/.+]                                                             |
|                           |                                                                                      |
|                           |       Caching only if its path was *"/img/\*"* or *"/css/\*"*                        |
|                           |                                                                                      |
|                           | ex-2) []                                                                             |
|                           |                                                                                      |
|                           |       When value is empty, all objects are cached.                                   |
+---------------------------+--------------------------------------------------------------------------------------+
| **Timeout**                                                                                                      |
+---------------------------+--------------------------------------------------------------------------------------+
| Timeout value when        | +--------+------------------------------------------------------------------+        |
| requesting to a storage   | |Level   | Range                                                            |        |
|                           | +========+==================================================================+        |
|                           | |level_1 | 0 to 65,535 bytes                                                |        |
|                           | +--------+------------------------------------------------------------------+        |
|                           | |level_2 | 65,535 to 131,071 bytes                                          |        |
|                           | +--------+------------------------------------------------------------------+        |
|                           | |level_3 | 131,072 to 524,287 bytes                                         |        |
|                           | +--------+------------------------------------------------------------------+        |
|                           | |level_4 | 524,288 to 1,048,576 bytes                                       |        |
|                           | +--------+------------------------------------------------------------------+        |
|                           | |level_5 | over 1,048,576 bytes                                             |        |
|                           | +--------+------------------------------------------------------------------+        |
+---------------------------+--------------------------------------------------------------------------------------+
| **Access Log**                                                                                                   |
+---------------------------+--------------------------------------------------------------------------------------+
| Output to a file          | Default value - *"is_enable_access_log"* is 'false' and Output destination is        |
| (v0.16.0-)                | set path, the proepery of which is set at *log_appender*.                            |
+---------------------------+--------------------------------------------------------------------------------------+
| Output to Elasticsearch   | +----------------------+----------------------------------------------------+        |
| (Beta, v0.16.0-)          | |Property              | Description                                        |        |
|                           | +======================+====================================================+        |
|                           | |is_enable_esearch     | Turn on/off access-log of output to Elasticsearch  |        |
|                           | +----------------------+----------------------------------------------------+        |
|                           | |esearch_host          | Listening Elasticsearch's host                     |        |
|                           | +----------------------+----------------------------------------------------+        |
|                           | |esearch_port          | Listening Elasticsearch's port                     |        |
|                           | +----------------------+----------------------------------------------------+        |
|                           | |eearch_timeout        | Request timeout                                    |        |
|                           | +----------------------+----------------------------------------------------+        |
|                           | |esearch_bulk_duration | Bulk logs for the duration time                    |        |
|                           | +----------------------+----------------------------------------------------+        |
+---------------------------+--------------------------------------------------------------------------------------+


.. code-block:: bash

    ## --------------------------------------------------------------------
    ## Manager's Node(s)
    ## --------------------------------------------------------------------
    ## Name of Manager node(s)
    ## managers = [manager_0@127.0.0.1, manager_1@127.0.0.1]
    managers = [{MANAGER_MASTER_IP}, {MANAGER_SLAVE_IP}]

    ## --------------------------------------------------------------------
    ## GATEWAY
    ## --------------------------------------------------------------------
    ## Gateway's protocol to use: [s3 | rest | embed]
    protocol = {HTTP_HANDLER}

    ## Port number the Gateway uses for HTTP connections
    http.port = {LISTENING_PORT}

    ## Numbers of processes listening for connections
    http.num_of_acceptors = {NUM_OF_LISTENER}

    ## Maximum number of requests allowed in a single keep-alive session
    http.max_keepalive = {MAX_KEEPALIVE}

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
    cache.http_cache = {IS_HTTP_CACHE}

    ## A number of cache workers
    ## cache.cache_workers = 16

    ## Memory cache capacity in bytes
    cache.cache_ram_capacity  = {CACHE_RAM_CAPACITY}

    ## Disk cache capacity in bytes
    cache.cache_disc_capacity = {CACHE_DISC_CAPACITY}

    ## When the length of the object exceeds this value, store the object on disk
    cache.cache_disc_threshold_len = {CACHE_DISC_THRESHOLD_LEN}

    ## Directory for the disk cache data
    cache.cache_disc_dir_data    = {CACHE_DISC_DIR_DATA}

    ## Directory for the disk cache journal
    cache.cache_disc_dir_journal = {CACHE_DISC_DIR_JOURNAL}

    ## Cache Expire in seconds
    cache.cache_expire = {CACHE_EXPIRE}

    ## Cache Max Content Length in bytes
    cache.cache_max_content_len = {CACHE_MAX_CONTENT_LEN}

    ## Cache Content Type(s)
    ## In case of "empty", all objects are cached.
    cache.cachable_content_type = {CACHE_CONTENT_TYPE}

    ## Cache Path Pattern(s) (regular expression)
    ## In case of "empty", all objects are cached.
    cache.cachable_path_pattern = {CACHE_PATH_PATTERNS}

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
    snmp_agent = {SNMPA-DIR}/snmp/snmpa_gateway_0/LEO-GATEWAY



**[Erlang VM related properties]**

* Modification of the required items:

+--------------------+--------------------------------------------------------+
|Property            | Description                                            |
+====================+========================================================+
|{GATEWAY_ALIAS}     | Gateway node's Alias name                              |
+--------------------+--------------------------------------------------------+
|{GATEWAY_IP}        | Gateway node's IP-Address                              |
+--------------------+--------------------------------------------------------+
|{SNMPA-DIR}         | SNMPA configuration files directory                    |
+--------------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the leofs-gateway node
    ## nodename = gateway_0@127.0.0.1
    nodename = {GATEWAY_ALIAS}@{GATEWAY_IP}

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
    snmp_conf = {SNMPA-DIR}/snmp/snmpa_gateway_0/leo_gateway_snmp

