.. LeoFS documentation

Install LeoFS
================================
.. index::
   pair: Erlang; Installation


Erlang
--------------------------------
Prepare
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Install OS-related libraries (CentOS 6.2)
"""""""""""""""""""""""""""""""""""""""""
.. index::
   pair: CentOS-6.2; Installation

* Install when using yum

::

   # yum install libuuid-devel

Install OS-related libraries (Ubuntu Server 12.04 LTS)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""
.. index::
   pair: Ubuntu-12.04; Installation

* Install when using apt (Ubuntu)

::

   # sudo apt-get install libtool libncurses5-dev libssl-dev

Install "libatomic_ops" for R15B03-1  *(both CentOS and Ubuntu)*
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

   $ wget http://www.hpl.hp.com/research/linux/atomic_ops/download/libatomic_ops-7.2d.tar.gz
   $ tar xzvf libatomic_ops-7.2d.tar.gz
   $ cd libatomic_ops-7.2d
   $ ./configure --prefix=/usr/local
   $ make
   $ sudo make install

Download "Erlang R15B03-1"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   ## [R15B03]
   $ cd $WORK_DIR
   $ wget http://www.erlang.org/download/otp_src_R15B03-1.tar.gz

Build for Linux (CentOS, Debian and Others)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   ## [R15B03-1]
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

Confirm
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    ## [R15B03-1]
    $ erl
    Erlang R15B03 (erts-5.9.3) [source] [64-bit halfword] [smp:2:2] [async-threads:0] [kernel-poll:false]

    Eshell V5.9.3  (abort with ^G)
    1>


XFS-related
------------

.. note:: If You deploy LeoFS on your **DEV environments**, You does NOT need this operaion, but if you deploy LeoFS on your **PRODUCTION environments**, You need to install XFS-libs and create an XFS's partition.

Install OS-related libraries (CentOS 6.2) for XFS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. index::
   pair: CentOS-6.2; Installation

* Install when using yum (CentOS)

::

   # yum --enablerepo=centosplus install kmod-xfs xfsprogs xfsprogs-devel


Create XFS Partition (Volume)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. index::
   pair: XFS; Installation

Only **LeoFS-Storage node** use XFS (unix local file system). Because XFS is high I/O efficiency for Large-File. **LeoFS-Storage** is implemented on top of files stored in a single filesystem created on top of the a few TB volume.

Create partition
"""""""""""""""""

::

   # fdisk /dev/sda

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

Execute
""""""""

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

   # fdisk /dev/sda

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

   # reboot

Execute 'Format partition'
"""""""""""""""""""""""""""

* `Reference(EN) <http://www.ibm.com/developerworks/linux/library/l-fs10/index.html>`_
* `Reference(JP) <http://www.ibm.com/developerworks/jp/linux/library/l-fs10/index.html>`_

::

   # mkfs.xfs -d agcount=4 -l size=32m ${TARGET_PARTITION}

Modify "/etc/fstab" file
"""""""""""""""""""""""""

::

   # vi /etc/fstab
   /dev/sda3   /mnt/xfs   xfs   noatime,nodiratime,osyncisdsync 0 0

Create mount point and Execute "mount" command
"""""""""""""""""""""""""""""""""""""""""""""""

::

   # mkdir /mnt/xfs
   # mount -a

Confirm
"""""""""

::

   # df
   Filesystem           1K-blocks      Used Available Use% Mounted on
   /dev/sda1             15180256   2153492  12243196  15% /
   tmpfs                  2025732         0   2025732   0% /dev/shm
   /dev/sda3             51664400      4


Install "LeoFS"
--------------------------------
.. index::
   pair: LeoFS; Installation

LeoFS's file structure (After decompress an LeoFS-archive)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Before executed make-command
""""""""""""""""""""""""""""""""

::

    $ git clone https://github.com/leo-project/leofs.git

    ${LEOFS_SRC_DIR}
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

After executed make-command
"""""""""""""""""""""""""""""""

::

    $ cd ${LEOFS_SRC}/
    $ make
    $ make release

    ${LEOFS_SRC_DIR}
      |
      |--- LICENSE
      |--- Makefile
      |---- deps/
      |      |--- bear/
      |      |--- bitcask/
      |      |--- cherly/
      |      |--- cowboy/
      |      |--- ecache/
      |      |--- eleveldb/
      |      |--- folsom/
      |      |--- jiffy/
      |      |--- leo_backend_db/
      |      |--- leo_commons/
      |      |--- leo_gateway/
      |      |--- leo_logger/
      |      |--- leo_manager/
      |      |--- leo_mq/
      |      |--- leo_object_storage/
      |      |--- leo_ordning_reda/
      |      |--- leo_redundant_manager/
      |      |--- leo_s3_libs/
      |      |--- leo_statistics/
      |      |--- leo_storage/
      |      |--- lz4/
      |      |--- meck/
      |      `--- proper/
      |---- doc/
      |---- rebar
      |---- rebar.config
      `---- rel/
             |--- leo_gateway/
             |--- leo_manager/
             `--- leo_storage/

Build "LeoFS"
^^^^^^^^^^^^^^^^^

::

    $ cd leofs/
    $ make
    $ make release
    $ cp -r package/leofs ${LEOFS_DEPLOYED_DIR}
    $ cd ${LEOFS_DEPLOYED_DIR}/

    [LeoFS deployed files layout]
    ${LEOFS_DEPLOYED_DIR}
      |--- leofs
      |      |--- gateway/
      |      |        |--- bin/
      |      |        |--- erts-5.8.5/
      |      |        |--- etc/
      |      |        |--- lib/
      |      |        |--- log/
      |      |        |--- releases/
      |      |        |--- snmp/
      |      |        `--- work/
      |      |--- manager_0/
      |      |        |--- bin/
      |      |        |--- erts-5.8.5/
      |      |        |--- etc/
      |      |        |--- lib/
      |      |        |--- log/
      |      |        |--- releases/
      |      |        |--- snmp/
      |      |        `--- work/
      |      |--- manager_1/
      |      |        |--- bin/
      |      |        |--- erts-5.8.5/
      |      |        |--- etc/
      |      |        |--- lib/
      |      |        |--- log/
      |      |        |--- releases/
      |      |        |--- snmp/
      |      |        `--- work/
      |      `--- storage/
      |               |--- bin/
      |               |--- erts-5.8.5/
      |               |--- etc/
      |               |--- lib/
      |               |--- log/
      |               |--- releases/
      |               |--- snmp/
      |               `--- work/

Log Dir and Working Dir
^^^^^^^^^^^^^^^^^^^^^^^^^^^

\

+-------------+--------------------------------------------------------+
| Directory   | Explanation                                            |
+=============+========================================================+
| **log/**                                                             |
+-------------+--------------------------------------------------------+
| log/app/    | For Application logs                                   |
+-------------+--------------------------------------------------------+
| log/ring    | For RING (routing-table for replication) Dump files    |
+-------------+--------------------------------------------------------+
| log/sasl    | For Erlang-SASL Logs                                   |
+-------------+--------------------------------------------------------+
| **work/**                                                            |
+-------------+--------------------------------------------------------+
| work/mnesia/| For System internal info which is stored into 'Mnesia' |
+-------------+--------------------------------------------------------+
| work/queue  | For Message Queue's data which is stored into 'bitcask'|
+-------------+--------------------------------------------------------+

- ref: `Basho bitcask <https://github.com/basho/bitcask>`_


::

   ${LEOFS_DEPLOYED_DIR}
     |      `--- storage/
     |               |--- bin/
     |               |--- erts-5.8.5/
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

.. _system-configuration-label:

Set up LeoFS's system-configuration (Only LeoFS-Manager)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* File: ${LEOFS_SRC}/package/leofs/manager_0/etc/app.config

.. note::  **Consistency Level** is decided by this configuration file. Also, It should not modify in operation.

+-------------+--------------------------------------------------------+
| Property    | Explanation                                            |
+=============+========================================================+
| n           | # of replicas                                          |
+-------------+--------------------------------------------------------+
| r           | # of replicas needed for a successful READ operation   |
+-------------+--------------------------------------------------------+
| w           | # of replicas needed for a successful WRITE operation  |
+-------------+--------------------------------------------------------+
| d           | # of replicas needed for a successful DELETE operation |
+-------------+--------------------------------------------------------+
| bit_of_ring | # of bits of hash-ring (fixed 128bit)                  |
+-------------+--------------------------------------------------------+

* A reference consistency level

+-------------+--------------------------------------------------------+
| Level       | Configuration                                          |
+=============+========================================================+
| Low         | n = 3, r = 1, w = 1, d = 1                             |
+-------------+--------------------------------------------------------+
| Middle      | n = 3, [r = 1 | r = 2], w = 2, d = 2                   |
+-------------+--------------------------------------------------------+
| High        | n = 3, [r = 2 | r = 3], w = 3, d = 3                   |
+-------------+--------------------------------------------------------+

* **Example - File: ${LEOFS_SRC}/package/leofs/manager_0/etc/app.config**:

.. code-block:: erlang

    %% Example (Part of manager-configurations):
    [

        {leo_manager,
                 [
                  %% System Configuration
                  {system, [{n, 3 },  %% # of replicas
                            {w, 2 },  %% # of replicas needed for a successful WRITE  operation
                            {r, 1 },  %% # of replicas needed for a successful READ   operation
                            {d, 2 },  %% # of replicas needed for a successful DELETE operation
                            {bit_of_ring, 128}
                           ]},
                  %% Manager Configuration
                  {manager_mode,     master },
                  {manager_partners, ["manager_1@127.0.0.1"] },
                  {port,             10010 },
                  {num_of_acceptors, 3},
                  %% Directories
                  {log_dir,          "./log"},
                  {queue_dir,        "./work/queue"},
                  {snmp_agent,       "./snmp/manager_0/LEO-MANAGER"}
                 ]},

    ].

Firewall Rules
--------------

+----------------+-----------+-----------------+--------------------------+
| Subsystem      | Direction | Ports           | Notes                    |
+================+===========+=================+==========================+
| Manager-Master | Incoming  | 10010/*         | Manager console          |
+----------------+-----------+-----------------+--------------------------+
| Manager-Master | Incoming  | 4369/*          | erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Manager-Master | Incoming  | 4020/*          | SNMP Listen Port         |
+----------------+-----------+-----------------+--------------------------+
| Manager-Master | Outgoing  | \*/4369         | erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Incoming  | 10011/*         | Manager console          |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Incoming  | 4369/*          | erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Incoming  | 4021/*          | SNMP Listen Port         |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Outgoing  | \*/4369         | erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+
| Storage        | Incoming  | 4369/*          | erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Storage        | Incoming  | 4010/*          | SNMP Listen Port         |
+----------------+-----------+-----------------+--------------------------+
| Storage        | Outgoing  | \*/4369         | erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 8080/*          | HTTP listen port         |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 8443/*          | HTTPS listen port        |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 4369/*          | erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 4000/*          | SNMP Listen Port         |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Outgoing  | \*/4369         | erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+

