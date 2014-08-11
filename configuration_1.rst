.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _leofs-configuration-label:

Configuration Fundamentals and LeoFS Manager
============================================

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
| Manager-Master| {LEOFS_HOME}/package/leo_manager_0/etc/leo_manager.conf |
+---------------+---------------------------------------------------------+
| Manager-Slave | {LEOFS_HOME}/package/leo_manager_1/etc/leo_manager.conf |
+---------------+---------------------------------------------------------+
| Storage       | {LEOFS_HOME}/package/leo_storage/etc/leo_storage.conf   |
+---------------+---------------------------------------------------------+
| Gateway       | {LEOFS_HOME}/package/leo_gateway/etc/leo_gateway.conf   |
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

+---------------------------------+---------------------------------------------------------+
| Property                        | Description                                             |
+=================================+=========================================================+
| n - consistency.num_of_replicas | # of replicas                                           |
+---------------------------------+---------------------------------------------------------+
| r - consistency.read            | # of replicas needed for a successful READ operation    |
+---------------------------------+---------------------------------------------------------+
| w - consistency.write           | # of replicas needed for a successful WRITE operation   |
+---------------------------------+---------------------------------------------------------+
| d - consistency.delete          | # of replicas needed for a successful DELETE operation  |
+---------------------------------+---------------------------------------------------------+
| consistency.rack_aware_replicas | # of rack-aware replicas                                |
+---------------------------------+---------------------------------------------------------+

.. index::
   pair: Configuration; Consistency level

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

* **Example - File: {LEOFS_SRC}/package/manager_0/etc/leo_manager.conf**:

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

* The ``leo_manager.conf`` location: **{LEOFS_HOME}/package/manager_0/etc/leo_manager.conf**
* Modification of the required items:

+----------------+--------------------------------------------------------+
| Property       | Description                                            |
+================+========================================================+
|{SLAVE-IP}      | Manager-Slave node's IP-address                        |
+----------------+--------------------------------------------------------+
|{SNMPA-DIR}     | SNMPA configuration files directory                    |
|                |                                                        |
|                | - ref:{LEOFS_SRC}/apps/leo_manager/snmp/               |
|                |                                                        |
|                | - [snmpa_manager_0|snmpa_manager_1|snmpa_manager_0]    |
+----------------+--------------------------------------------------------+

\

.. code-block:: bash

    ## --------------------------------------------------------------------
    ## MANAGER
    ## --------------------------------------------------------------------
    ## Mode of Manager: [master, slave]
    manager.mode = master

    ## Partner of manager's alias
    manager.partner = manager_1@{SLAVE-IP}

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
    consistency.num_of_replicas = 3

    ## A number of replicas needed for a successful WRITE operation
    consistency.write = 2

    ## A number of replicas needed for a successful READ operation
    consistency.read = 1

    ## A number of replicas needed for a successful DELETE operation
    consistency.delete = 2

    ## A number of rack-aware replicas
    consistency.rack_aware_replicas = 0

    ## --------------------------------------------------------------------
    ## MANAGER - Multi DataCenter Settings
    ## --------------------------------------------------------------------
    ## A number of replication targets
    mdc_replication.max_targets = 2

    ## A number of replicas a DC
    mdc_replication.num_of_replicas_a_dc = 1

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
    snmp_agent = {SNMPA-DIR}/snmp/snmpa_manager_0/LEO-MANAGER


**[Erlang VM related properties]**

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|{MASTER-IP}     | Manager-Master IP                                      |
+----------------+--------------------------------------------------------+
|{SNMPA-DIR}     | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the leofs-gateway node
    nodename = manager_0@{MASTER-IP}

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
    snmp_conf = {SNMPA-DIR}/snmp/snmpa_manager_0/leo_maanager_snmp

.. index::
   pair: Configuration; LeoFS Manager-Slave

LeoFS Manager-Slave
-------------------

**Configuration of the Manager-Slave node**

**[leo_manager.conf]**

* The ``leo_manager.conf`` location: **{LEOFS_HOME}/package/manager_1/etc/leo_manager.conf**
* Modification of the required items:

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|{MASTER-IP}     | Manager-Master node's IP-address                       |
+----------------+--------------------------------------------------------+
|{SNMPA-DIR}     | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash


    ## --------------------------------------------------------------------
    ## MANAGER
    ## --------------------------------------------------------------------
    ## Mode of Manager: [master, slave]
    manager.mode = slave

    ## Partner of manager's alias
    manager.partner = manager_0@{MASTER-IP}

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
    snmp_agent = {SNMPA-DIR}/snmp/snmpa_manager_1/LEO-MANAGER

**[Erlang VM related properties]**

+----------------+--------------------------------------------------------+
|Property        | Description                                            |
+================+========================================================+
|{SLAVE-IP}      | Manager-Slave IP                                       |
+----------------+--------------------------------------------------------+
|{SNMPA-DIR}     | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the leofs-gateway node
    nodename = manager_1@{SLAVE-IP}

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
    snmp_conf = {SNMPA-DIR}/snmp/snmpa_manager_1/leo_maanager_snmp
