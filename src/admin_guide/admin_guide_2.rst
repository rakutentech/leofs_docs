.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    General commands

General Commands
================

+-------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| **Shell**                                             | **Description**                                                                                   |
+=======================================================+===================================================================================================+
| leofs-adm :ref:`status <status-command>` [<node>]     | * Retrieve status of every node (default)                                                         |
|                                                       | * Retrieve status of the specified node                                                           |
+-------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`whereis <whereis-command>` <file-path>| Retrieve an assigned object by the file-path                                                      |
+-------------------------------------------------------+---------------------------------------------------------------------------------------------------+

.. _status-command:

.. index::
    pair: General commands; status-command

status
^^^^^^

Retrieve status of every node (default)

.. code-block:: bash

    $ leofs-adm status
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


status <node>
^^^^^^^^^^^^^

Retrieve status of the specified node

.. code-block:: bash

    $ leofs-adm status storage_0@127.0.0.1
    [config-1: basic]
    --------------------------------+------------------
                            version | 1.2.0
                   number of vnodes | 168
                      group level-1 |   
                      group level-2 | 
                object container(s) | [[{path,"./avs"},{num_of_containers,8}]]
                            log dir | ./log/erlang
    --------------------------------+------------------

    [config-2: watchdog]
    --------------------------------+------------------
     rex  - watch interval(sec)     | 5
     rex  - threshold mem capacity  | 33554432
    --------------------------------+------------------
     cpu  - watchdog enabled        | disabled
     cpu  - watch interval(sec)     | 5
     cpu  - threshold cpu load avg  | 1.5
     cpu  - threshold cpu util(%)   | 100
    --------------------------------+------------------
      io  - watchdog enabled        | disabled
      io  - watch interval(sec)     | 1
      io  - threshold input size/s  | 134217728
      io  - threshold output size/s | 134217728
    --------------------------------+------------------
     disk - watchdog enabled        | disabled
     disk - watch interval(sec)     | 1
     disk - threshold disk use(%)   | 85
     disk - threshold disk util(%)  | 95
    --------------------------------+------------------

    [status-1: ring]
    --------------------------------+------------------
                  ring state (cur)  | c8ab8e21
                  ring state (prev) | c8ab8e21
    --------------------------------+------------------

    [status-2: erlang-vm]
    --------------------------------+------------------
                         vm version | 5.10.4
                    total mem usage | 36080472
                   system mem usage | 20999568
                    procs mem usage | 15126936
                      ets mem usage | 5031968
                              procs | 410/1048576
                        kernel_poll | true
                   thread_pool_size | 32
    --------------------------------+------------------

    [status-3: # of msgs]
    --------------------------------+------------------
                   replication msgs | 0
                    vnode-sync msgs | 0
                     rebalance msgs | 0
    --------------------------------+------------------


    $ leofs-adm status gateway_0@127.0.0.1
    [config-1: basic]
    -------------------------------+------------------
     basic
    -------------------------------+------------------
                           version | 1.2.0
                    using protocol | s3
                           log dir | ./log/erlang
    -------------------------------+------------------
     http-server-related for REST/S3 API
    -------------------------------+------------------
                    listening port | 8080
                listening ssl port | 8443
                    # of_acceptors | 128
    -------------------------------+------------------
     cache-related
    -------------------------------+------------------
           http cache [true|false] | false
                # of cache_workers | 16
                      cache expire | 300
             cache max content len | 1048576
                ram cache capacity | 268435456
            disk cache capacity    | 524288000
            disk cache threshold   | 1048576
            disk cache data dir    | ./cache/data
            disk cache journal dir | ./cache/journal
    -------------------------------+------------------
     large-object-related
    -------------------------------+------------------
               max # of chunk objs | 1000
               chunk object length | 5242880
                 max object length | 5242880000
         reading  chunk obj length | 5242880
         threshold of chunk length | 5767168
    -------------------------------+------------------

    [config-2: watchdog]
    -------------------------------+------------------
     rex - watch interval(sec)     | 5
     rex - threshold mem capacity  | 33554432
    -------------------------------+------------------
     cpu - watchdog eanbled        | disabled
     cpu - watch interval(sec)     | 5
     cpu - threshold cpu load avg  | 2
     cpu - threshold cpu util(%)   | 100
    -------------------------------+------------------
      io - watchdog enabled        | disabled
      io - watch interval(sec)     | 5
      io - threshold input size/s  | 134217728
      io - threshold output size/s | 134217728
    -------------------------------+------------------

    [status-1: ring]
    -------------------------------+------------------
                 ring state (cur)  | c8ab8e21
                 ring state (prev) | c8ab8e21
    -------------------------------+------------------

    [status-2: erlang-vm]
    -------------------------------+------------------
                        vm version | 5.10.4
                   total mem usage | 60515880
                  system mem usage | 45586112
                   procs mem usage | 14956896
                     ets mem usage | 5491896
                             procs | 463/1048576
                       kernel_poll | true
                  thread_pool_size | 32
    -------------------------------+------------------

\

.. _whereis-command:

.. index::
    pair: General commands; whereis-command

\

whereis <file-path>
^^^^^^^^^^^^^^^^^^^

Retrieve an assigned object by the file-path
Paths used by `whereis` are ruled by :ref:`this rule <s3-path-label>`

.. code-block:: bash

    $ leofs-adm whereis leo/fast/storage.key
    -----------------------------------------------------------------------------------------------------------------------
     del? node                 ring address    size   # of chunks  checksum    vclock            when
    -----------------------------------------------------------------------------------------------------------------------
          storage_1@127.0.0.1  207643840133    35409  0             4116193149  1332407492290951  2012-06-29 14:23:31 +0900
          storage_0@127.0.0.1  207643840133    35409  0             4116193149  1332407492290951  2012-06-29 14:23:31 +0900

\

If you want to retrieve an object whose file-path contains spaces,
Enclose the file-path with double quotation.

.. code-block:: bash

    $ leofs-adm whereis "leo/fast/storage with space.key"
    -----------------------------------------------------------------------------------------------------------------------
     del? node                 ring address    size   # of chunks  checksum    vclock            when
    -----------------------------------------------------------------------------------------------------------------------
          storage_1@127.0.0.1  207643840133    35409  0             4116193149  1332407492290951  2012-06-29 14:23:31 +0900
          storage_0@127.0.0.1  207643840133    35409  0             4116193149  1332407492290951  2012-06-29 14:23:31 +0900

\

If you want to retrieve a chunk object which is part of a large object,
Append `\n` and the chunk number to the file-path.

.. code-block:: bash

    $ leofs-adm whereis leo/fast/storage.key\n1
    -----------------------------------------------------------------------------------------------------------------------
     del? node                 ring address    size   # of chunks  checksum    vclock            when
    -----------------------------------------------------------------------------------------------------------------------
          storage_1@127.0.0.1  207643840133    5120K  0             4116193149  1332407492290951  2012-06-29 14:23:31 +0900
          storage_0@127.0.0.1  207643840133    5120K  0             4116193149  1332407492290951  2012-06-29 14:23:31 +0900

\

