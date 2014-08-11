.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    Multi Datacenter replication commands

Multi Datacenter Replication Operation
======================================

+-------------------------------------------------------------------+--------------------------------------------------------------------------------------+
| Command                                                           | Description                                                                          |
+===================================================================+======================================================================================+
| join-cluster <remote-manager-master> <remote-manager-slave>       | ``1.0.0-`` Begin to communicate between the local cluster and the remote cluster     |
+-------------------------------------------------------------------+--------------------------------------------------------------------------------------+
| remove-cluster <remote-manager-master> <remote-manager-slave>     | ``1.0.0-`` Terminate to communicate between the local cluster and the remote cluster |
+-------------------------------------------------------------------+--------------------------------------------------------------------------------------+
| cluster-status                                                    | ``1.0.0-`` See the current state of cluster(s)                                       |
+-------------------------------------------------------------------+--------------------------------------------------------------------------------------+

\

.. ### JOIN-CLUSTER ###
.. _join_cluster:

.. index::
    pair: Multi Datacenter replication commands; join-cluster-command

join-cluster <remote-manager-master> <remote-manager-slave>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Begin to communicate between the local cluster and the remote cluster

::

    join-cluster manager_c2_0@10.1.2.1 manager_c2_1@10.1.2.2
    OK

\

.. ### REMOVE-CLUSTER ###
.. _remove_cluster:

.. index::
    pair: Multi Datacenter replication commands; remove-cluster-command


remove-cluster <remote-manager-master> <remote-manager-slave>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Terminate to communicate between the local cluster and the remote cluster

::

    remove-cluster manager_c2_0@10.1.2.1 manager_c2_1@10.1.2.2
    OK

\

.. ### CLUSTER-STATUS ###
.. _cluster_status:

.. index::
    pair: Multi Datacenter replication commands; cluster-status-command

cluster-status
^^^^^^^^^^^^^^

See the current state of cluster(s)

::

    cluster-status
    cluster id |   dc id    |    status    | # of storages  |          updated at
    -----------+------------+--------------+----------------+-----------------------------
    leofs_2    | dc_2       |   running    |              8 | 2014-03-21 19:17:45 +0900

