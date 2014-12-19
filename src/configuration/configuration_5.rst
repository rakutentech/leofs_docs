.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _leofs-with-nfs-label:

.. index::
   pair: Configuration; Multi data center replication

Multi data center replication
=============================

Since
-------

LeoFS v1.0.0


.. index::
   pair: NFS; Purpose

Purpose
-------
This section is a step by step guide to setting up the multi data center replication.

.. index::
   pair: Multi data center replication; Getting Started

Getting Started
---------------

Pre-requirement
~~~~~~~~~~~~~~~

.. note:: We have checked this mechanism with CentOS 6.5 and Ubuntu Server 14.04 LTS but we're goinng to investigate other OS such as FreeBSD and SmartOS.


LeoFS Manager Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set |leo_manager_conf_1|
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you need to change DC's Id and Cluster's Id, set ``system.dc_id`` and ``cluster_id``

::

    ## DC Id
    system.dc_id = dc_N

    ## Cluster Id
    system.cluster_id = leofs_N


Set |leo_manager_conf_2|
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You need to set RPC-related configuration as follows to be able to communicate each cluster with LeoFS's RPC. Regarding ``rpc.server.listen_port``, which is used by the `join-cluster <admin_guide_9.html#join-cluster>`_ command.



::

    ## RPC-Server's acceptors
    rpc.server.acceptors = 16

    ## RPC-Server's listening port number
    rpc.server.listen_port = 13075

    ## RPC-Server's listening timeout(second)
    rpc.server.listen_timeout = 5000

    ## RPC-Client's size of connection pool
    rpc.client.connection_pool_size = 16

    ## RPC-Client's size of connection buffer
    rpc.client.connection_buffer_size = 16


LeoFS Storage Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set |leo_storage_conf|
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You need to set RPC-related configuration as follows to be able to communicate each cluster with LeoFS's RPC.

::

    ## RPC-Server's acceptors
    ## this value must be determinted by following logic
    ## rpc.server.acceptor need to be larger than
    ## rpc.client.connection_pool(buffer)_size * "# of storage nodes + # of manager nodes in your cluster"
    ## The default value is suitable for less than 16 nodes in a cluster
    rpc.server.acceptors = 128

    ## RPC-Server's listening port number
    rpc.server.listen_port = 13077

    ## RPC-Server's listening timeout(second)
    rpc.server.listen_timeout = 30000

    ## RPC-Client's size of connection pool
    rpc.client.connection_pool_size = 8

    ## RPC-Client's size of connection buffer
    rpc.client.connection_buffer_size = 8

* See Also: |multidc-replication|


.. |leo_manager_conf_1| raw:: html

   <a href="https://github.com/leo-project/leo_manager/blob/1.1.1/priv/leo_manager_0.conf#L55-L59" target="_blank">leo_manager.conf - DC Id and Cluster Id</a>

.. |leo_manager_conf_2| raw:: html

   <a href="https://github.com/leo-project/leo_manager/blob/1.1.1/priv/leo_manager_0.conf#L140-L153" target="_blank">RPC-related configuration</a>

.. |leo_storage_conf| raw:: html

   <a href="https://github.com/leo-project/leo_storage/blob/1.1.1/priv/leo_storage.conf#L169-L186" target="_blank">RPC-related configuration</a>


.. |multidc-replication| raw:: html

   <a href="http://leo-project.net/leofs/blog-entry-3.html" target="_blank">Multi Data Center Replication (1st phase)</a>
