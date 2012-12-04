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

* Erlang OTP R14B04 <http://www.erlang.org/download_release/12>
* Erlang OTP R15B02 <http://www.erlang.org/download_release/15>

And the following erlang libraries:

* Basho Bitcask <https://github.com/basho/bitcask>
* Ninenines Cowboy <https://github.com/extend/cowboy>
* Boundary Folsom <https://github.com/boundary/folsom>
* Szktty Erlang LZ4 <https://github.com/szktty/erlang-lz4>

And we recommend you to use on 64bit builds because of having to handle large files.

.. index::
   pair: LeoFS; Download

-------------
Getting LeoFS
-------------
* Leo Project/leofs: <https://github.com/leo-project/leofs>


------------------------------------------------------
Quick Start -1 All in one for Application Development
------------------------------------------------------

1. Install
^^^^^^^^^^

Erlang (CentOS, Ubuntu, Other Linux OS)
"""""""""""""""""""""""""""""""""""""""""""

::

   $ wget http://www.erlang.org/download/otp_src_R14B04.tar.gz
   $ tar xzf otp_src_R14B04.tar.gz
   $ cd otp_src_R14B04
   $ ./configure --prefix=/usr/local/erlang/R14B04 \
                 --enable-smp-support \
                 --enable-m64-build \
                 --enable-halfword-emulator \
                 --enable-kernel-poll \
                 --without-javac \
                 --disable-native-libs \
                 --disable-hipe \
                 --disable-sctp \
                 --enable-threads
   $ make
   $ sudo make install

LeoFS
"""""""""

::

    $ git clone https://github.com/leo-project/leofs.git
    $ cd leofs
    $ make && make release


2. Configuration
^^^^^^^^^^^^^^^^^

Modify “/ets/hosts”
"""""""""""""""""""""""

* Add a LeoFS's domain in ``/ets/hosts``
* LeoFS's domains are governed by :ref:`this rule <s3-path-label>`

::

    $ sudo vi /ets/hosts

    ## Add a LeoFS's domain ##
    127.0.0.1 localhost ${BUCKET_NAME}.localhost


3. Launch LeoFS's managers and storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Launch master-manager, slave-manager
* Launch a storage - ``# of replicas is ONE`` - System-configuration's details are :ref:`here <system-configuration-label>`.

::

    $ cd $LEOFS_ROOT/package/leofs
    $ manager_0/bin/leo_manager start
    $ manager_1/bin/leo_manager start
    $ storage/bin/leo_storage start


4. Launch the system
^^^^^^^^^^^^^^^^^^^^^

* Using command is ``start`` on LeoFS's manager-console

::

    $ telnet 127.0.0.1 10010
    > start

5. Launch a LeoFS's gateway
^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ gateway/bin/leo_gateway start

6. Confirmation of the system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Using command is ``status`` on LeoFS's manager-console

::

    $ telnet 127.0.0.1 10010
    > status
    [system config]
                 version : 0.10.0
     # of replicas       : 1
     # of successes of R : 1
     # of successes of W : 1
     # of successes of D : 1
               ring size : 2^128
              ring state : 1428891014

    [node(s) state]
    ------------------------------------------------------------------------------------------------
     node                        state       ring (cur)    ring (prev)   when
    ------------------------------------------------------------------------------------------------
     storage_0@127.0.0.1         running     1428891014    1428891014    2012-09-07 14:23:08 +0900
     gateway@127.0.0.1           running     1428891014    1428891014    2012-09-07 14:24:37 +0900


7. Getting Your S3-API's Key from LeoFS's Manager-Console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Using command is ``s3-create-key`` on LeoFS's manager-console

::

    $ telnet 127.0.0.1 10010
    > s3-create-key ${YOUR_NAME}
    access-key-id: 05dcba94333c7590a635
    secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a



8. Clients
^^^^^^^^^^^

* LeoFS uses any S3-Clients as follows:
    * :ref:`Ruby - ‘aws-sdk’ <aws-sdk-ruby-label>`
    * :ref:`Ruby - ‘aws-s3’ <aws-s3-ruby-label>`
    * :ref:`FUSE - ‘S3FS-C’ <s3fs-c-label>`
    * :ref:`GUI  - ‘Dragon Disk’ <dragondisk-label>`


---------------------------
Quick Start -2 Cluster
---------------------------

...under construction...
