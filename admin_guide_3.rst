.. LeoFS documentation
.. Copyright (c) 2013-2014 Rakuten, Inc.

Multi Datacenter Replication Operation
======================================

.. index::
    MultiDC-related commands

MultiDC-related Commands
------------------------

\

+-------------------------------------------------------------------+-------------------------------------------------------------------------------+
| Command                                                           | Explanation                                                                   |
+===================================================================+===============================================================================+
| join-cluster `{REMOTE_MANAGER_MASTER}` `{REMOTE_MANAGER_SLAVE}`   | [1.0.0-] Communicate between the local-cluster and a remote cluster           |
+-------------------------------------------------------------------+-------------------------------------------------------------------------------+
| remove-cluster `{REMOTE_MANAGER_MASTER}` `{REMOTE_MANAGER_SLAVE}` | [1.0.0-] Remove communication between clusters                                |
+-------------------------------------------------------------------+-------------------------------------------------------------------------------+
| cluster-status                                                    | [1.0.0-] Retrieve current status of clusters                                  |
+-------------------------------------------------------------------+-------------------------------------------------------------------------------+

\

.. ### JOIN-CLUSTER ###

.. _join_cluster:

.. index::
    pair: MultiDC-related commands; join-cluster-command

**'join-cluster'** - Communicate between the local-cluster and a remote cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``join-cluster {REMOTE_MANAGER_MASTER} {REMOTE_MANAGER_SLAVE}``

::

    join-cluster manager_c2_0@10.1.2.1 manager_c2_1@10.1.2.2
    OK

\

.. ### REMOVE-CLUSTER ###

.. _remove_cluster:

.. index::
    pair: MultiDC-related commands; remove-cluster-command

**'remove-cluster'** - Remove communication between clusters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``remove-cluster {REMOTE_MANAGER_MASTER} {REMOTE_MANAGER_SLAVE}``

::

    remove-cluster manager_c2_0@10.1.2.1 manager_c2_1@10.1.2.2
    OK

\

.. ### CLUSTER-STATUS ###

.. _cluster_status:

.. index::
    pair: MultiDC-related commands; cluster-status-command

**'cluster-status'** - Retrieve current status of clusters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``cluster-status``

::

    cluster-status
    cluster id |   dc id    |    status    | # of storages  |          updated at
    -----------+------------+--------------+----------------+-----------------------------
    leofs_2    | dc_2       |   running    |              8 | 2014-03-21 19:17:45 +0900

