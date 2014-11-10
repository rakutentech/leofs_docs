.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

Launch and Operation Flow
=========================

.. _operation-flow-diagram-label:

.. index::
    pair: Administration; Operation flow diagram

Operation Flow Diagram
-----------------------

The LeoFS operation flow is as follows:

.. image:: ../../_static/images/leofs-flow-diagram.jpg
   :width: 780px

* `The diagram only <http://www.leofs.org/docs/_images/leofs-flow-diagram.jpg>`_


.. index::
    pair: Administration; LeoFS launch order

LeoFS launch order
----------------------

LeoFS's system launch process is very simple:

.. image:: ../../_static/images/leofs-order-of-system-launch.png
   :width: 640px



Description of the Operations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

\

+-------------+------------------------------------+--------------------------------------------------------------+
| **Order**   | **Command/Script**                 | **Description**                                              |
+=============+====================================+==============================================================+
| 1           | $ bin/leofs_manager start          | Start LeoFS Manager’s master                                 |
+-------------+------------------------------------+--------------------------------------------------------------+
| 2           | $ bin/leofs_manager start          | Start LeoFS Manager’s slave                                  |
+-------------+------------------------------------+--------------------------------------------------------------+
| 3           | $ bin/leofs_storage start          | Start LeoFS Storage                                          |
+-------------+------------------------------------+--------------------------------------------------------------+
| 4           | $ leofs-adm start                  | Start LeoFS storage cluster                                  |
+-------------+------------------------------------+--------------------------------------------------------------+
| 5           | $ leofs-adm status                 | Confirm state of the every LeoFS Storage                     |
+-------------+------------------------------------+--------------------------------------------------------------+
| 6           | $ bin/leofs_gateway start          | Start LeoFS Gateway                                          |
+-------------+------------------------------------+--------------------------------------------------------------+
| 7           | $ leofs-adm status                 | Confirm state of the every node - LeoFS Storaage and Gateway |
+-------------+------------------------------------+--------------------------------------------------------------+


.. index::
    pair: Administration; LeoFS launch step by step

LeoFS launch step by step
--------------------------

Start LeoFS Manager's master
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ cd $LEOFS_DEPLOYED_DIR
    $ manager_0/bin/leo_manager start


Start LeoFS Manager's slave
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ cd $LEOFS_DEPLOYED_DIR
    $ manager_1/bin/leo_manager start


Start LeoFS Storage
^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ cd $LEOFS_DEPLOYED_DIR
    $ leo_storage/bin/leo_storage start


Connect LeoFS Manager with leofs-adm
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 'status' command - Inspect LeoFS-cluster

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
                 Current ring hash :
                    Prev ring hash :
    [Multi DC replication settings]
             max # of joinable DCs : 2
                # of replicas a DC : 1

    [Node(s) state]
    -------+--------------------------+--------------+----------------+----------------+----------------------------
     type  |           node           |    state     |  current ring  |   prev ring    |          updated at
    -------+--------------------------+--------------+----------------+----------------+----------------------------
      S    | storage_0@127.0.0.1      | attached     |                |                | 2014-04-03 11:28:20 +0900
      S    | storage_1@127.0.0.1      | attached     |                |                | 2014-04-03 11:28:20 +0900
      S    | storage_2@127.0.0.1      | attached     |                |                | 2014-04-03 11:28:20 +0900
      S    | storage_3@127.0.0.1      | attached     |                |                | 2014-04-03 11:28:20 +0900


The "start" command - Start LeoFS storage cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ leofs-adm start
    OK


Confirm state of the every LeoFS storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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


Start LeoFS Gateway
^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ cd $LEOFS_DEPLOYED_DIR/
    $ gateway/bin/leo_gateway start


Confirm state of the every node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

