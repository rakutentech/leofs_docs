Gateway and Other Operation
===========================


Gateway Maintenance and Misc Commands
-------------------------------------

\

+------------------------------------------------------+-----------------------------------------------------------------------------------+
| Command                                              | Explanation                                                                       |
+======================================================+===================================================================================+
| Gateway                                                                                                                                  |
+------------------------------------------------------+-----------------------------------------------------------------------------------+
| purge `{FILE_PATH}`                                  | Purge a cached file if the specified file exists in the cache                     |
+------------------------------------------------------+-----------------------------------------------------------------------------------+
| remove `{GATEWAY_NODE}`                              | * Retrieve status of the cluster                                                  |
|                                                      | * Retrieve status of the node                                                     |
+------------------------------------------------------+-----------------------------------------------------------------------------------+
| Misc                                                                                                                                     |
+------------------------------------------------------+-----------------------------------------------------------------------------------+
| dump-ring `{MANAGER}`|`{STORAGE}`|`{GATEWAY}`        | Dump ring and memeber-info [1.0.0-pre1 or higher ]                                |
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

