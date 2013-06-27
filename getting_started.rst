.. LeoFS documentation master file, created by
   sphinx-quickstart on Tue Feb 21 10:38:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Getting Started
================================

-------------------
System Requirements
-------------------
LeoFS development currently targets Debian 6, Ubuntu-Server 12.04 LTS and CentOS 6.x, but should work on
most Linux platforms with the following software:

* `Erlang OTP R15B03-1 <http://www.erlang.org/download_release/16>`_

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

.. note:: In this section, ``LeoFS-Storage``, ``LeoFS-Gateway`` and ``LeoFS-Manager`` are all installed on a single system, with no clustering.

1. Install
^^^^^^^^^^

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
                 version : 0.12.7
     # of replicas       : 1
     # of successes of R : 1
     # of successes of W : 1
     # of successes of D : 1
               ring size : 2^128
        ring hash (cur)  : 1428891014
        ring hash (prev) : 1428891014

    [node(s) state]
    ------------------------------------------------------------------------------------------------
     node                        state       ring (cur)    ring (prev)   when
    ------------------------------------------------------------------------------------------------
     storage_0@127.0.0.1         running     1428891014    1428891014    2012-09-07 14:23:08 +0900
     gateway@127.0.0.1           running     1428891014    1428891014    2012-09-07 14:24:37 +0900


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


---------------------------
Quick Start -2 Cluster
---------------------------

(under construction)
