.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    System maintenance

System Maintenance
==================

.. index::
    pair: System maintenance; Upgrade old version to 1.1.0

\

Upgrade your old version LeoFS to v1.1.0
----------------------------------------

This section describes the way of replacement of old LeoFS to v1.1.0

Upgrade flow diagram
^^^^^^^^^^^^^^^^^^^^

\

.. image:: _static/images/leofs-upgrade-flow-diagram.jpg
   :width: 780px

* `The diagram only <http://www.leofs.org/docs/_images/leofs-upgrade-flow-diagram.jpg>`_

\

.. note:: If you're using LeoFS v1.0.0-pre1, v0.16 or v0.14, you need to take over the configuration of ``metadata-storage`` as follows because the default configuration is ``leveldb`` from v1.0.0-pre2. We're planning to provide the ``db-converter`` tool - from ``bitcask`` to ``leveldb`` with v1.2.0.

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

* LeoFS Manager: [mnesia, log-dir and queue-dir]

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


* LeoFS Storage: [obj_containers, log-dir and queue-dir]

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


* LeoFS Gateway: [SSL-related files, cache-related pathes, log-dir and queue-dir]

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


.. index::
    pair: System maintenance; Attach/Detach node operation

Attach/Detach node into a Storage-cluster in operation
------------------------------------------------------

This section describes the process of adding and removing nodes in a LeoFS Storage cluster.

* Adding a storage node:
    * The node can be added to the cluster once it is running. You can use the :ref:`rebalance <rebalance-command>` command to request a join from the Manager.
* Removing a storage node:
    * The node can be removed from the cluster when it is either running or stopped. You can use the :ref:`detach <detach-command>` command to remove the node.
    * After that, you need to execute the :ref:`rebalance <rebalance-command>` command in the Manager to actually remove the node from the storage cluster.


.. image:: _static/images/leofs-order-of-attach.png
   :width: 640px

.. index::
   detach-storage

.. image:: _static/images/leofs-order-of-detach.png
   :width: 640px

\

.. index::
    pair: System maintenance; LeoFS Gateway access-log format

LeoFS Gateway Access-log Format (v1.0.0-pre3 later)
---------------------------------------------------

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
| Column Number | Description                                                |
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

