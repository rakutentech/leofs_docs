.. LeoFS documentation master file, created by
   sphinx-quickstart on Tue Feb 21 10:38:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Getting Started
================================

-------------------
System Requirements
-------------------
LeoFS development currently targets Debian 6, Ubuntu-Server 12.04 LTS|13.04 and CentOS 6.x, but should work on
most Linux platforms with the following software:

* `Erlang OTP R15B03-1 <http://www.erlang.org/download_release/16>`_
* `Erlang OTP R16B01 <http://www.erlang.org/download_release/19>`_


LeoFS includes the following Erlang libraries:

* `Basho Bitcask <https://github.com/basho/bitcask>`_
* `Ninenines Cowboy <https://github.com/extend/cowboy>`_
* `Boundary Folsom <https://github.com/boundary/folsom>`_
* `Szktty Erlang LZ4 <https://github.com/szktty/erlang-lz4>`_

We recommend that you use a 64bit system to be able to handle large files.

.. index::
   pair: LeoFS; Download

-------------
Getting LeoFS
-------------
* Leo Project/leofs: <https://github.com/leo-project/leofs>


------------------------------------------------------
Quick Start -1 All in one for Application Development
------------------------------------------------------

Pupose
^^^^^^

This document is intended to give you a step by step guide to setting up LeoFS for the first time. For this tutorial, you can easily build "LeoFS stand-alone system".

.. note:: In this section, ``LeoFS-Storage``, ``LeoFS-Gateway`` and ``LeoFS-Manager`` are all installed on a single system, with no clustering.

1. Install
^^^^^^^^^^

.. _erlang-install-label:

Erlang (CentOS, Ubuntu, Other Linux OS)
"""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

   ##
   ## 1. Install libatomic
   ##
   $ wget http://www.hpl.hp.com/research/linux/atomic_ops/download/libatomic_ops-7.2d.tar.gz
   $ tar xzvf libatomic_ops-7.2d.tar.gz
   $ cd libatomic_ops-7.2d
   $ ./configure --prefix=/usr/local
   $ make
   $ sudo make install

   ##
   ## 2. Install Erlang (R15B03-1)
   ##
   $ wget http://www.erlang.org/download/otp_src_R15B03-1.tar.gz
   $ tar xzf otp_src_R15B03-1.tar.gz
   $ cd otp_src_R15B03-1
   $ ./configure --prefix=/usr/local/erlang/R15B03 \
                 --enable-smp-support \
                 --enable-m64-build \
                 --enable-halfword-emulator \
                 --enable-kernel-poll \
                 --without-javac \
                 --disable-native-libs \
                 --disable-hipe \
                 --disable-sctp \
                 --enable-threads \
                 --with-libatomic_ops=/usr/local
   $ make
   $ sudo make install

   ##
   ## 3. Set PATH
   ##
   $ vi ~/.profile
       ## append the follows:
       export ERL_HOME=/usr/local/erlang/R15B03
       export PATH=$PATH:$ERL_HOME/bin

   $ source ~/.profile


.. _leofs-install-label:

LeoFS
"""""""""

::

    $ git clone https://github.com/leo-project/leofs.git
    $ cd leofs
    $ make && make release


2. Configuration
^^^^^^^^^^^^^^^^^

Modify “/etc/hosts”
"""""""""""""""""""""""

* Add a domain for the LeoFS bucket in ``/etc/hosts``
* Bucket names must follow :ref:`these rules <s3-path-label>`

::

    $ sudo vi /etc/hosts

    ## Replace ${BUCKET_NAME} with the name of the bucket ##
    127.0.0.1 localhost ${BUCKET_NAME}.localhost


3. Launch LeoFS' managers and storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* By default there is only one replica, you may want to :ref:`configure the system <system-configuration-label>`.
* Start master-manager, slave-manager
* Start a storage node

::

    $ cd $LEOFS_ROOT/package/leofs
    $ manager_0/bin/leo_manager start
    $ manager_1/bin/leo_manager start
    $ storage/bin/leo_storage start


4. Start the system
^^^^^^^^^^^^^^^^^^^^^

* Use the command ``start`` in the LeoFS manager console

::

    $ telnet 127.0.0.1 10010
    > start

5. Start a LeoFS gateway node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ gateway/bin/leo_gateway start

6. Confirm that the system is running
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``status`` in the LeoFS manager console

::

    $ telnet 127.0.0.1 10010
    > status
    status
    [system config]
                 version : 0.14.4
     # of replicas       : 1
     # of successes of R : 1
     # of successes of W : 1
     # of successes of D : 1
     # of awareness replicas [DC]  : 0
     # of awareness replicas [Rack]: 0
               ring size : 2^128
        ring hash (cur)  : 1428891014
        ring hash (prev) : 1428891014

    [node(s) state]
    ------------------------------------------------------------------------------------------------
     node                        state       ring (cur)    ring (prev)   when
    ------------------------------------------------------------------------------------------------
     storage_0@127.0.0.1         running     1428891014    1428891014    2013-07-04 11:23:08 +0900
     gateway@127.0.0.1           running     1428891014    1428891014    2013-07-04 11:24:37 +0900


7. Get your S3 API Key from the LeoFS manager console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``create-user`` in the LeoFS manager console
* It takes the user name as its only argument

::

    $ telnet 127.0.0.1 10010
    > create-user ${YOUR_NAME}
    access-key-id: 05dcba94333c7590a635
    secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a



8. Clients
^^^^^^^^^^^

* You can now connect to LeoFS using any S3 client, including:
    * :ref:`Ruby - ‘aws-sdk’ <aws-sdk-ruby-label>`
    * :ref:`FUSE - ‘S3FS-C’ <s3fs-c-label>`
    * :ref:`GUI  - ‘Dragon Disk’ <dragondisk-label>`

Wrap up
^^^^^^^

You were able to learn building *LeoFS stand-alone system*. Make sure to have a look :ref:`Quick Start -2 Cluster <quick-start2-label>` to help you get a more understanding.


.. _quick-start2-label:

---------------------------
Quick Start -2 Cluster
---------------------------

Purpose
^^^^^^^

For this tutorial, you can easily build "LeoFS cluster". We does not explain each step in detail here, this guide is just to help you get it up and running immediately. We also recommend that you should read the LeoFS Installation, Configuration and Administration Guide to learn how to administer LeoFS. We would like you to be successful in as short a time as possible.

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


1. Install Erlang and LeoFS each server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* :ref:`Install Erlang <erlang-install-label>`
* :ref:`Install LeoFS <leofs-install-label>`


2. Configuration - Edit a part of *"vm.args"* each server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Filepath: "$LEOFS_ROOL/package/leo_*/etc/vm.args"
* Precondition
    * ``-name`` is a unique into the LeoFS
* Edit *Manager-master's vm.args*

.. code-block:: bash

    ## Name of the node
    -name manager_0@10.0.1.101
    ... omitted below

* Edit *Manager-slave's vm.args*

.. code-block:: bash

    ## Name of the node
    -name manager_0@10.0.1.102
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

* Reference: :ref:`Configuring your new LeoFS system using LeoFS-Manager <system-configuration-label>`
* Edit *Manager's app.config*
    * "$LEOFS_ROOT/package/leo_manager_0/etc/app.config"

.. code-block:: erlang

    [
        {leo_manager, [
                   %% == System Ver ==
                   {system_version, "0.14.4" },

                   %% == System Configuration ==
                   %% - Consistency Level
                   {system, [{n, 2 },  %% number of replicated files is 2
                             {w, 1 },  %% number of of successes of write-operation is 1
                             {r, 1 },  %% number of of successes of read-operation is 1
                             {d, 1 },  %% number of of successes of delete-operation is 1
                             {bit_of_ring, 128} %% size of routing-table (RING)
                            ]},


4. Order of server launch
^^^^^^^^^^^^^^^^^^^^^^^^^

* Manager-master
* Manager-slave
* Storages
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


* Launch each Storage

.. code-block:: bash

    $ $LEOFS_ROOT/package/leo_storage/bin/leo_storage start

* Launch each Gateway

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
    > status
    status
    [system config]
                 version : 0.14.4
     # of replicas       : 2
     # of successes of R : 1
     # of successes of W : 1
     # of successes of D : 1
     # of awareness replicas [DC]  : 0
     # of awareness replicas [Rack]: 0
               ring size : 2^128
        ring hash (cur)  : 1428891014
        ring hash (prev) : 1428891014

    [node(s) state]
    ------------------------------------------------------------------------------------------------
     node                        state       ring (cur)    ring (prev)   when
    ------------------------------------------------------------------------------------------------
     storage_0@10.0.1.104        running     1428891014    1428891014    2013-07-04 11:23:08 +0900
     storage_1@10.0.1.105        running     1428891014    1428891014    2013-07-04 11:23:08 +0900
     storage_2@10.0.1.106        running     1428891014    1428891014    2013-07-04 11:23:08 +0900
     gateway_0@10.0.1.103        running     1428891014    1428891014    2013-07-04 11:24:37 +0900


8. Get your S3 API Key from the LeoFS manager console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Use the command ``create-user`` in the LeoFS manager console
* It takes the user name as its only argument

::

    $ telnet 127.0.0.1 10010
    > create-user ${YOUR_NAME}
    access-key-id: 05dcba94333c7590a635
    secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a


Wrap up
^^^^^^^

So you were able to build *LeoFS cluster*. Make sure to have a look :ref:`LeoFS installation <leofs-installation-label>`, :ref:`LeoFS Configuration <leofs-configuration-label>` and :ref:`Administration Guide <administration-guide-label>` to help you get a deeper understanding.

