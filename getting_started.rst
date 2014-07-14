:tocdepth: 3

Getting Started
===============

.. index::
   pair: LeoFS; Download

-------------
Getting LeoFS
-------------
* Leo Project/leofs: <https://github.com/leo-project/leofs>

------------------------------------------------------
Quick Start -1 All in one for Application Development
------------------------------------------------------

Purpose
^^^^^^^

This section is a step by step guide to setting up LeoFS for the first time. By following this tutorial you can easily build a stand-alone LeoFS system.

.. note:: In this section, ``LeoFS-Storage``, ``LeoFS-Gateway`` and ``LeoFS-Manager`` are all installed on a single system, with no clustering.

1. Install
^^^^^^^^^^

* Download a LeoFS package from `LeoFS website <http://leo-project.net/leofs/download.html>`_

.. _install_leofs_label:

Install LeoFS on CentOS-6.x
"""""""""""""""""""""""""""""""""""""""""""""""""
.. index::
   pair: CentOS-6; Installation

::

    $ wget http://leo-project.net/leofs/packages/rpm/x86_64/leofs-{VERSION}.x86_64.rpm
    $ sudo rpm -ivh leofs-{VERSION}.x86_64.rpm
    $ ls -l /usr/local/leofs/
    total 4
    drwxr-xr-x 6 root   root   4096 Jun 20 15:37 {VERSION}
    $ chown -R {USER}:{GROUP} /usr/local/leofs/{VERSION}

Install LeoFS on Ubuntu Server 12.04 LTS or Higher
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
.. index::
   pair: Ubuntu; Installation

.. code-block:: bash

    $ wget http://leo-project.net/leofs/packages/ubuntu/x86_64/leofs_{VERSION}_amd64.deb
    $ sudo dpkg -i leofs_{VERSION}_amd64.deb
    $ ls -l /usr/local/leofs/
    total 4
    drwxr-xr-x 6 root   root   4096 Jun 20 15:37 {VERSION}
    $ chown -R {USER}:{GROUP} /usr/local/leofs/{VERSION}


2. Configuration
^^^^^^^^^^^^^^^^^

Modify “/etc/hosts”
"""""""""""""""""""""""

* Add a domain for the LeoFS bucket in ``/etc/hosts``
* Bucket names must follow :ref:`these rules <s3-path-label>`

.. code-block:: bash

    $ sudo vi /etc/hosts

    ## Replace {BUCKET_NAME} with the name of the bucket ##
    127.0.0.1 localhost {BUCKET_NAME}.localhost


3. Launch LeoFS' managers and storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* By default there is only one replica, you may want to :ref:`configure the system <system-configuration-label>`.
* Start master-manager, slave-manager
* Start a storage node

.. code-block:: bash

    $ cd /usr/local/leofs/{VERSION}
    $ leo_manager_0/bin/leo_manager start
    $ leo_manager_1/bin/leo_manager start
    $ leo_storage/bin/leo_storage start


4. Start the system
^^^^^^^^^^^^^^^^^^^^^

* Use the command ``start`` in the LeoFS manager console

.. code-block:: bash

    $ telnet 127.0.0.1 10010
    > start

5. Start a LeoFS gateway node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ leo_gateway/bin/leo_gateway start

6. Confirm that the system is running
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``status`` in the LeoFS manager console

::

    $ telnet 127.0.0.1 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    status
    [System config]
                    System version : 1.0.0
                        Cluster Id : leofs_1
                             DC Id : dc_1
                    Total replicas : 1
               # of successes of R : 1
               # of successes of W : 1
               # of successes of D : 1
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
      G    | gateway_0@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:21 +0900


7. Get your S3 API Key from the LeoFS manager console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``create-user`` in the LeoFS manager console
* It takes the user name as its only argument

.. code-block:: bash

    $ telnet 127.0.0.1 10010
    > create-user {YOUR_NAME}
    access-key-id: 05dcba94333c7590a635
    secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a



8. Clients
^^^^^^^^^^^

* You can now connect to LeoFS using any S3 client, including:
    * :ref:`Ruby - ‘aws-sdk’ <aws-sdk-ruby-label>`
    * :ref:`FUSE - ‘S3FS-C’ <s3fs-c-label>`
    * :ref:`GUI  - ‘Dragon Disk’ <dragondisk-label>`

9. Using LeoFS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``add-bucket`` in the LeoFS manager console
* It takes the bucket name and access-key-id got in the previous section as its arguments

.. code-block:: bash

    $ telnet 127.0.0.1 10010
    > add-bucket {BUCKET_NAME} {YOUR_ACCESS_KEY_ID}
    ok

* Insert some data into LeoFS by using any S3 client as mentioned above
* You can now get the data stored in LeoFS

.. code-block:: bash

    $ curl http://localhost:8080/your_bucket_name/path/to/file
    > {CONTENTS}

.. note:: From version 0.16.0, you need to set ACL settings of your bucket to ``public-read`` by using the command :ref:`update-acl<s3-update-acl>` if you want to get the data stored in LeoFS via web browser.

Wrap up
^^^^^^^

You now know how to setup a *stand-alone LeoFS system*. Make sure to have a look at :ref:`Quick Start -2 Cluster <quick-start2-label>` to learn how to setup a LeoFS cluster.


.. _quick-start2-label:

---------------------------
Quick Start -2 Cluster
---------------------------

Purpose
^^^^^^^

This tutorial teaches you how to easily build a LeoFS cluster. All steps will not be explained in detail, it is assumed you already know how to setup a stand-alone LeoFS system. This guide exists to help you get a cluster up and running quickly. We recommend that you read the LeoFS Installation, Configuration and Administration Guide to learn how to administer your LeoFS cluster. We hope that by reading this tutorial you will be able to get a cluster started as quickly as possible.

Case example
^^^^^^^^^^^^

* :ref:`Manager <conf_manager_label>`
    * IP: 10.0.1.101, 10.0.1.102
    * Name: manager_0@10.0.1.101, manager_1@10.0.1.102
* :ref:`Gateway <conf_gateway_label>`
    * IP: 10.0.1.103
    * Name: gateway_0@10.0.1.103
* :ref:`Storage <conf_storage_label>`
    * IP: 10.0.1.104 .. 10.0.1.106
    * Name: storage_0@10.0.1.104 .. storage_2@10.0.1.106


1. Install Erlang and LeoFS on each server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* :ref:`Install LeoFS <install_leofs_label>`


2. Configuration - Edit *"vm.args"* on each server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* File path: "$LEOFS_ROOT/package/leo_*/etc/vm.args"
* Precondition
    * ``-name`` must be unique for each node in the LeoFS cluster

* Edit *Manager-master's vm.args*

.. code-block:: bash

    ## Name of the node
    -name manager_0@10.0.1.101
    ... omitted below

* Edit *Manager-slave's vm.args*

.. code-block:: bash

    ## Name of the node
    -name manager_1@10.0.1.102
    ... omitted below

* Edit *Gateway's vm.args*

.. code-block:: bash

    ## Name of the node
    -name gateway_0@10.0.1.103
    ... omitted below

* Edit *Storage's vm.args*

.. code-block:: bash

    ## Name of the node
    -name storage_0@10.0.1.104
    ... omitted below

3. Configuration - Consistency level
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Reference: :ref:`The consistency level <system-configuration-label>`
* Edit *Manager's leo_manager.conf*
    * You only need to modify *Manager-master* for the consistency level.
    * "$LEOFS_ROOT/package/leo_manager_0/etc/app.config"

.. code-block:: bash

    ## --------------------------------------------------------------------
    ## MANAGER - Consistency Level
    ##     * Only set its configurations to **Manager-master**
    ##     * See: http://www.leofs.org/docs/configuration.html#the-consistency-level
    ## --------------------------------------------------------------------
    ## A number of replicas
    consistency.num_of_replicas = 2

    ## A number of replicas needed for a successful WRITE operation
    consistency.write = 1

    ## A number of replicas needed for a successful READ operation
    consistency.read = 1

    ## A number of replicas needed for a successful DELETE operation
    consistency.delete = 1


4. Order of server launch
^^^^^^^^^^^^^^^^^^^^^^^^^

* Manager-master
* Manager-slave
* Storage nodes
* Gateway(s)


5. Method of server launch
^^^^^^^^^^^^^^^^^^^^^^^^^^

* Shell script: "$LEOFS_ROOT/package/leo_*/bin/leo_*"
* Launch Manager-master

.. code-block:: bash

    $ $LEOFS_ROOT/package/leo_manager_0/bin/leo_manager start

* Launch Manager-slave

.. code-block:: bash

    $ $LEOFS_ROOT/package/leo_manager_1/bin/leo_manager start


* Launch each Storage nodes

.. code-block:: bash

    $ $LEOFS_ROOT/package/leo_storage/bin/leo_storage start

* Launch each Gateway nodes

.. code-block:: bash

    $ $LEOFS_ROOT/package/leo_gateway/bin/leo_gateway start


6. Start the system
^^^^^^^^^^^^^^^^^^^

* Use the command ``start`` in the LeoFS manager console

::

    $ telnet 127.0.0.1 10010
    > start

7. Confirm that the system is running
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``status`` in the LeoFS manager console

::

    $ telnet 127.0.0.1 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    status
    [System config]
                    System version : 1.0.0
                        Cluster Id : leofs_1
                             DC Id : dc_1
                    Total replicas : 2
               # of successes of R : 1
               # of successes of W : 1
               # of successes of D : 1
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
      G    | gateway_0@127.0.0.1      | running      | 8cd79c31       | 8cd79c31       | 2014-04-03 11:28:21 +0900


8. Get your S3 API Key from the LeoFS manager console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``create-user`` in the LeoFS manager console
* It takes the user name as its only argument

.. code-block:: bash

    $ telnet 127.0.0.1 10010
    > create-user {YOUR_NAME}
    access-key-id: 05dcba94333c7590a635
    secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a

9. Using LeoFS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``add-bucket`` in the LeoFS manager console
* It takes the bucket name and access-key-id got in the previous section as its arguments

.. code-block:: bash

    $ telnet 127.0.0.1 10010
    > add-bucket {BUCKET_NAME} {YOUR_ACCESS_KEY_ID}
    ok

* Insert some data into LeoFS by using any S3 client as mentioned above
* You can now get the data stored in LeoFS

.. code-block:: bash

    $ curl http://localhost:8080/your_bucket_name/path/to/file
    > {CONTENTS}

.. note:: From version 0.16.0, you need to set ACL settings of your bucket to ``public-read`` by using the command :ref:`update-acl<s3-update-acl>` if you want to get the data stored in LeoFS via web browser.

Wrap up
^^^^^^^

You now have a working *LeoFS cluster*. Make sure to have a look at :ref:`LeoFS Installation <leofs-installation-label>`, :ref:`LeoFS Configuration <leofs-configuration-label>` and :ref:`Administration Guide <administration-guide-label>` to learn more about setting up and managing your LeoFS cluster.
