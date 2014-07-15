.. LeoFS documentation
.. Copyright (c) 2013-2014 Rakuten, Inc.

.. _operation-flow-diagram-label:

Operation Flow and Launch
=========================

Operation Flow Diagram
-----------------------

The LeoFS operation flow is as follows:

.. image:: _static/images/leofs-flow-diagram.jpg
   :width: 780px

* `The diagram only <http://www.leofs.org/docs/_images/leofs-flow-diagram.jpg>`_

System launch order
----------------------

LeoFS's system launch process is very simple:

.. image:: _static/images/leofs-order-of-system-launch.png
   :width: 640px



Explanation of the Operations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

\

+-------------+------------------------------------+------------------------------------------------------------+
| Order       | Command                            | Explanation                                                |
+=============+====================================+============================================================+
| 1           | $ bin/leofs_manager start          | Starting Manager Master **on Manager Master Node**         |
+-------------+------------------------------------+------------------------------------------------------------+
| 2           | $ bin/leofs_manager start          | Starting Manager Slave  **on Manager Slave Node**          |
+-------------+------------------------------------+------------------------------------------------------------+
| 3           | $ bin/leofs_storage start          | Starting Storage(s) **on each Storage Nodes**              |
+-------------+------------------------------------+------------------------------------------------------------+
| 4           | $ telnet $manager-master 10010     | Accessing **Manager Master Console** by telnet             |
+-------------+------------------------------------+------------------------------------------------------------+
| 5           | > start                            | Starting LeoFS (manager and storage)                       |
+-------------+------------------------------------+------------------------------------------------------------+
| 6           | > status                           | Confirm status of the cluster on Manager Master Console #1 |
+-------------+------------------------------------+------------------------------------------------------------+
| 7           | $ bin/leofs_gateway start          | Starting Gateway(s) **on each Gateway Node**               |
+-------------+------------------------------------+------------------------------------------------------------+
| 8           | > status                           | Confirm status of the cluster on Manager Master Console #2 |
+-------------+------------------------------------+------------------------------------------------------------+


System launch step by step
--------------------------

.. index::
    pair: Operation; System Launch

Start manager-master on **LeoFS-Manager Master** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ cd $LEOFS_DEPLOYED_DIR
    $ manager_0/bin/leo_manager start

Start manager-slave on **LeoFS-Manager Slave** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ cd $LEOFS_DEPLOYED_DIR
    $ manager_1/bin/leo_manager start

Start storage on each **LeoFS-Storage** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ cd $LEOFS_DEPLOYED_DIR
    $ leo_storage/bin/leo_storage start

Open LeoFS Manager Console on **LeoFS-Manager Master** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 'status' command - Inspect LeoFS-cluster ::

    $ telnet 127.0.0.1 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

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


**'start' command** - Launch LeoFS-cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    start
    OK

Confirm#1 by **LeoFS-Manager** node's console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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


Launch Gateway on each **LeoFS-Gateway** node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ cd $LEOFS_DEPLOYED_DIR/
    $ gateway/bin/leo_gateway start


Confirm#2 by **LeoFS-Manager** master node's console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

