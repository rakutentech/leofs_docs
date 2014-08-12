.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: Getting Started; Quick Start: All in one in a local node

---------------------------------------
Quick Start: All in one in a local node
---------------------------------------

Purpose
^^^^^^^

This section is a step by step guide to setting up LeoFS for the first time. By following this tutorial you can easily build a stand-alone LeoFS system.

.. note:: In this section, ``LeoFS Storage``, ``LeoFS Gateway`` and ``LeoFS Manager`` are all installed on a single system, with no clustering.

Install
^^^^^^^

* Download a LeoFS package from `LeoFS website <http://leo-project.net/leofs/download.html>`_

.. _install_leofs_label:

Install LeoFS on CentOS-6.x
"""""""""""""""""""""""""""

.. code-block:: bash

    $ wget http://leo-project.net/leofs/packages/rpm/x86_64/leofs-{VERSION}.x86_64.rpm
    $ sudo rpm -ivh leofs-{VERSION}.x86_64.rpm
    $ ls -l /usr/local/leofs/
    total 4
    drwxr-xr-x 6 root   root   4096 Jun 20 15:37 {VERSION}
    $ chown -R {USER}:{GROUP} /usr/local/leofs/{VERSION}

Install LeoFS on Ubuntu Server 12.04 LTS or Higher
""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

    $ wget http://leo-project.net/leofs/packages/ubuntu/x86_64/leofs_{VERSION}_amd64.deb
    $ sudo dpkg -i leofs_{VERSION}_amd64.deb
    $ ls -l /usr/local/leofs/
    total 4
    drwxr-xr-x 6 root   root   4096 Jun 20 15:37 {VERSION}
    $ chown -R {USER}:{GROUP} /usr/local/leofs/{VERSION}


Configuration
^^^^^^^^^^^^^

Modify “/etc/hosts”
"""""""""""""""""""""""

* Add a domain for the LeoFS bucket in ``/etc/hosts``
* Bucket names must follow :ref:`these rules <s3-path-label>`

.. code-block:: bash

    $ sudo vi /etc/hosts

    ## Replace {BUCKET_NAME} with the name of the bucket ##
    127.0.0.1 localhost {BUCKET_NAME}.localhost


Launch LeoFS' managers and storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* By default there is only one replica, you may want to :ref:`configure the system <system-configuration-label>`.
* Start master-manager, slave-manager
* Start a storage node

.. code-block:: bash

    $ cd /usr/local/leofs/{VERSION}
    $ leo_manager_0/bin/leo_manager start
    $ leo_manager_1/bin/leo_manager start
    $ leo_storage/bin/leo_storage start


Start the system
^^^^^^^^^^^^^^^^

* Use the command ``start`` in the LeoFS manager console

.. code-block:: bash

    $ leofs-adm start

Start a LeoFS Gateway node
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ leo_gateway/bin/leo_gateway start

Confirm that the system is running
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``status`` in the LeoFS manager console

.. code-block:: bash

    $ leofs-adm status
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


Get your S3 API Key from the LeoFS manager console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command :ref:`create-user <create-user>` in the LeoFS manager console
* It takes the user name as its only argument

.. code-block:: bash

    $ leofs-adm create-user <your_name>
    access-key-id: 05dcba94333c7590a635
    secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a

Clients
^^^^^^^

* You can now connect to LeoFS using any S3 client, including:
    * :ref:`Ruby - ‘aws-sdk’ <aws-sdk-ruby-label>`
    * :ref:`FUSE - ‘S3FS-C’ <s3fs-c-label>`
    * :ref:`GUI  - ‘Dragon Disk’ <dragondisk-label>`

Using LeoFS
^^^^^^^^^^^

* Use the command :ref:`add-bucket <add-bucket>` in the LeoFS manager console
* It takes the bucket name and access-key-id got in the previous section as its arguments

.. code-block:: bash

    $ leofs-adm add-bucket <bcuket> <access-key-id>
    ok

* Insert some data into LeoFS by using any S3 client as mentioned above
* You can now get the data stored in LeoFS

.. code-block:: bash

    $ curl http://localhost:8080/your_bucket_name/path/to/file
    > {CONTENTS}

.. note:: From version 0.16.0, you need to set ACL settings of your bucket to ``public-read`` by using the command :ref:`update-acl<update-acl>` if you want to get the data stored in LeoFS via web browser.

Wrap up
^^^^^^^

You now know how to setup a *stand-alone LeoFS system*. Make sure to have a look at :ref:`Quick Start -2 Cluster <quick-start2-label>` to learn how to setup a LeoFS cluster.

