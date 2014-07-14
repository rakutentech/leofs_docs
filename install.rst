.. LeoFS documentation

.. _leofs-installation-label:

LeoFS Installation
================================
.. index::
   pair: Erlang; Installation


System Requirements
-------------------
LeoFS development currently targets Debian 6, Ubuntu-Server 12.04 LTS or Higher and CentOS 6.5, but should work on
most Linux platforms with the following software installed:

* `Erlang/OTP R16B03-1 <http://www.erlang.org/download_release/23>`_


Installing Erlang
-----------------

.. note:: We recommend this installation method. Please follow the relevant instructions for your environment.


Preparation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Install required libraries using yum (CentOS 6.5)
"""""""""""""""""""""""""""""""""""""""""""""""""
.. index::
   pair: CentOS-6.5; Installation

::

   $ sudo yum install gcc gcc-c++ glibc-devel make ncurses-devel openssl-devel autoconf \
                      libuuid-devel cmake check check-devel

Install required libraries using apt-get (Ubuntu Server 12.04 LTS or Higher)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
.. index::
   pair: Ubuntu-12.04; Installation

::

   $ sudo apt-get install build-essential libtool libncurses5-dev libssl-dev cmake check

Install "libatomic_ops" for R16B03-1  *(both CentOS and Ubuntu)*
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

   $ wget http://www.hpl.hp.com/research/linux/atomic_ops/download/libatomic_ops-7.2d.tar.gz
   $ tar xzvf libatomic_ops-7.2d.tar.gz
   $ cd libatomic_ops-7.2d
   $ ./configure --prefix=/usr/local
   $ make
   $ sudo make install

Download "Erlang R16B03-1"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   ## [R16B03-1]
   $ cd $WORK_DIR
   $ wget http://www.erlang.org/download/otp_src_R16B03-1.tar.gz


Build Erlang on CentOS 6.5
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   $ tar xzf otp_src_R16B03-1.tar.gz
   $ cd otp_src_R16B03-1
   $ CFLAGS="-DOPENSSL_NO_EC=1" \
     ./configure --prefix=/usr/local/erlang/R16B03-1 \
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

Build Erlang on Debian and others
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   $ tar xzf otp_src_R16B03-1.tar.gz
   $ cd otp_src_R16B03-1
   $ ./configure --prefix=/usr/local/erlang/R16B03-1 \
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


Confirm
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ erl
    Erlang R16B03-1 (erts-5.10.4) [source] [64-bit halfword] [smp:8:8] [async-threads:10] [kernel-poll:false]

    Eshell V5.10.4  (abort with ^G)
    1>


XFS-related
------------

.. note:: We highly recommend using an XFS partition, as it is the file system that shows the better results with LeoFS. This section describes the installation instructions related to XFS. If you are deploying LeoFS on a **DEV environment**, you do NOT need to perform this operation.

Install required libraries for XFS with yum (CentOS 6.5)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. index::
   pair: CentOS-6.5; Installation

::

   $ sudo yum --enablerepo=centosplus install kmod-xfs xfsprogs xfsprogs-devel


Create an XFS Partition (Volume)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. index::
   pair: XFS; Installation

Only the servers running **LeoFS storage nodes** will benefit from using XFS (unix local file system). XFS provides particularly efficient I/O for large files. **LeoFS-Storage** is implemented on top of files stored in a single file system created on top of the a few TB volume.

Start fdisk
"""""""""""""""""

::

   $ sudo fdisk /dev/sda

   The number of cylinders for this disk is set to 8908.
   There is nothing wrong with that, but this is larger than 1024,
   and could in certain setups cause problems with:
   1) software that runs at boot time (e.g., old versions of LILO)
   2) booting and partitioning software from other OSs
   (e.g., DOS FDISK, OS/2 FDISK)

   Command (m for help): p

   Disk /dev/sda: 73.2 GB, 73272393728 bytes
   255 heads, 63 sectors/track, 8908 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes

      Device Boot      Start         End      Blocks   Id  System
   /dev/sda1   *           1        1951    15671376   83  Linux
   /dev/sda2            1952        2472     4184932+  82  Linux swap / Solaris

Create partition
""""""""""""""""

::

   Command (m for help): n
   Command action
      e   extended
      p   primary partition (1-4)
   p
   Partition number (1-4): 3
   First cylinder (2473-8908, default 2473):[Enter]
   Using default value 2473
   Last cylinder or +size or +sizeM or +sizeK (2473-8908, default 8908):[Enter]
   Using default value 8908

   Command (m for help): w
   The partition table has been altered!

   Calling ioctl() to re-read partition table.

   WARNING: Re-reading the partition table failed with error 16: Device or  resource busy.
   The kernel still uses the old table.
   The new table will be used at the next reboot.
   Syncing disks.

Confirm
""""""""

::

   $ sudo fdisk /dev/sda

   The number of cylinders for this disk is set to 8908.
   There is nothing wrong with that, but this is larger than 1024,
   and could in certain setups cause problems with:
   1) software that runs at boot time (e.g., old versions of LILO)
   2) booting and partitioning software from other OSs
   (e.g., DOS FDISK, OS/2 FDISK)

   Command (m for help): p

   Disk /dev/sda: 73.2 GB, 73272393728 bytes
   255 heads, 63 sectors/track, 8908 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
   /dev/sda1   *           1        1951    15671376   83  Linux
   /dev/sda2            1952        2472     4184932+  82  Linux swap / Solaris
   /dev/sda3            2473        8908    51697170   83  Linux

Reboot
"""""""

::

   $ sudo reboot

Format the partition
"""""""""""""""""""""""""""

* `Reference(EN) <http://www.ibm.com/developerworks/linux/library/l-fs10/index.html>`_
* `Reference(JP) <http://www.ibm.com/developerworks/jp/linux/library/l-fs10/index.html>`_

::

   $ sudo mkfs.xfs -d agcount=4 -l size=32m {TARGET_PARTITION}

Modify the "/etc/fstab" file
""""""""""""""""""""""""""""

::

   $ sudo vi /etc/fstab
   /dev/sda3   /mnt/xfs   xfs   noatime,nodiratime,osyncisdsync 0 0

Mount the partition
"""""""""""""""""""""""""""""""""""""""""""""""

::

   $ sudo mkdir /mnt/xfs
   $ sudo mount -a

Confirm
"""""""""

::

   $ df
   Filesystem           1K-blocks      Used Available Use% Mounted on
   /dev/sda1             15180256   2153492  12243196  15% /
   tmpfs                  2025732         0   2025732   0% /dev/shm
   /dev/sda3             51664400      4


LeoFS
--------------------------------
.. index::
   pair: LeoFS; Installation

This installation method is based on a source build, so if you do not have Erlang already installed, you need to first install Erlang. Also, building LeoFS from source requires Erlang R15B03-1 or R16B03-1.


File structure
^^^^^^^^^^^^^^

Before running make
"""""""""""""""""""

::

    $ git clone https://github.com/leo-project/leofs.git

    {LEOFS_SRC_DIR}
      |
      |--- LICENSE
      |--- Makefile
      |--- apps/
      |--- deps/
      |--- doc/
      |--- rebar
      |--- rebar.config
      `--- rel/
             |--- leo_gateway/
             |--- leo_manager/
             `--- leo_storage/

After running make
""""""""""""""""""

::

    $ cd {LEOFS_SRC}/
    $ make
    $ make release

    {LEOFS_SRC_DIR}
      |
      |--- LICENSE
      |--- Makefile
      |---- deps/
      |      |--- bear/
      |      |--- bitcask/
      |      |--- cowboy/
      |      |--- eleveldb/
      |      |--- folsom/
      |      |--- jiffy/
      |      |--- leo_backend_db/
      |      |--- leo_cache/
      |      |--- leo_commons/
      |      |--- leo_dcerl
      |      |--- leo_gateway/
      |      |--- leo_logger/
      |      |--- leo_manager/
      |      |--- leo_mcerl/
      |      |--- leo_mq/
      |      |--- leo_object_storage/
      |      |--- leo_ordning_reda/
      |      |--- leo_pod/
      |      |--- leo_redundant_manager/
      |      |--- leo_rpc/
      |      |--- leo_s3_libs/
      |      |--- leo_statistics/
      |      |--- leo_storage/
      |      |--- lz4/
      |      |--- meck/
      |      |--- proper/
      |      |--- ranch/
      |      |--- savanna_agent/
      |      `--- savanna_commons/
      |---- rebar
      |---- rebar.config
      `---- rel/
             |--- leo_gateway/
             |--- leo_manager/
             `--- leo_storage/

Building
^^^^^^^^^^^^^^^^^

::

    $ cd leofs/
    $ make
    $ make release
    $ cp -r package {LEOFS_DEPLOYED_DIR}
    $ cd {LEOFS_DEPLOYED_DIR}/

    [LeoFS deployed files layout]
    {LEOFS_DEPLOYED_DIR}
            |--- leo_gateway/
            |        |--- bin/
            |        |--- erts-{VERSION}/
            |        |--- etc/
            |        |--- lib/
            |        |--- log/
            |        |--- releases/
            |        |--- snmp/
            |        `--- work/
            |--- leo_manager_0/
            |        |--- bin/
            |        |--- erts-{VERSION}/
            |        |--- etc/
            |        |--- lib/
            |        |--- log/
            |        |--- releases/
            |        |--- snmp/
            |        `--- work/
            |--- leo_manager_1/
            |        |--- bin/
            |        |--- erts-{VERSION}/
            |        |--- etc/
            |        |--- lib/
            |        |--- log/
            |        |--- releases/
            |        |--- snmp/
            |        `--- work/
            `--- leo_storage/
                     |--- bin/
                     |--- erts-{VERSION}/
                     |--- etc/
                     |--- lib/
                     |--- log/
                     |--- releases/
                     |--- snmp/
                     `--- work/

Log Dir and Working Dir
^^^^^^^^^^^^^^^^^^^^^^^

\

+-------------+--------------------------------------------------------+
| Directory   | Explanation                                            |
+=============+========================================================+
| **log/**                                                             |
+-------------+--------------------------------------------------------+
| log/app/    | Application logs                                       |
+-------------+--------------------------------------------------------+
| log/ring/   | RING (routing-table for replication) dump files        |
+-------------+--------------------------------------------------------+
| log/sasl/   | SASL (Erlang system) Logs                              |
+-------------+--------------------------------------------------------+
| **work/**                                                            |
+-------------+--------------------------------------------------------+
| work/mnesia/| System internal data stored into 'Mnesia'              |
+-------------+--------------------------------------------------------+
| work/queue/ | Message queue data stored into 'bitcask'               |
+-------------+--------------------------------------------------------+

- ref: `Basho bitcask <https://github.com/basho/bitcask>`_


::

   {LEOFS_DEPLOYED_DIR}
     |      `--- leo_storage/
     |               |--- bin/
     |               |--- erts-{VERSION}/
     |               |--- etc/
     |               |--- lib/
     |               |--- log/
     |               |     |--- app/
     |               |     |--- ring/
     |               |     `--- sasl/
     |               |--- releases/
     |               |--- snmp/
     |               `--- work/
     .                     |--- mnesia
     .                     `--- queue

Firewall Rules
--------------

In order for LeoFS to work correctly, it is necessary to set and check the firewall rules in your environment as follows:

+----------------+-----------+-----------------+--------------------------+
| Subsystem      | Direction | Ports           | Notes                    |
+================+===========+=================+==========================+
| Manager-Master | Incoming  | 10010/*         | Manager console          |
+----------------+-----------+-----------------+--------------------------+
| Manager-Master | Incoming  | 4369/*          | Erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Manager-Master | Incoming  | 4020/*          | SNMP Listen Port         |
+----------------+-----------+-----------------+--------------------------+
| Manager-Master | Outgoing  | \*/4369         | Erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Incoming  | 10011/*         | Manager console          |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Incoming  | 4369/*          | Erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Incoming  | 4021/*          | SNMP Listen Port         |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Outgoing  | \*/4369         | Erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+
| Storage        | Incoming  | 4369/*          | Erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Storage        | Incoming  | 4010/*          | SNMP Listen Port         |
+----------------+-----------+-----------------+--------------------------+
| Storage        | Outgoing  | \*/4369         | Erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 8080/*          | HTTP listen port         |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 8443/*          | HTTPS listen port        |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 4369/*          | Erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 4000/*          | SNMP Listen Port         |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Outgoing  | \*/4369         | Erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+

