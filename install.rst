.. LeoFS documentation master file, created by
   sphinx-quickstart on Tue Feb 21 10:38:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

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


Configuration of Each Server - Manager, Storage and Gateway
---------------------------------------------------------------------

.. image:: _static/images/leofs-conf-relationship.png
   :width: 700px

.. image:: _static/images/leofs-conf-relationship-snmpa.png
   :width: 700px



LeoFS Manager-Master
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Manager-Master's Properties for launch**

* **File-1: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_0/etc/app.config**

+----------------+--------------------------------------------------------+
|Property        | Configuration                                          |
+================+========================================================+
|${SLAVE-IP}     | Manager-Slave node's IP-address                        |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
|                |                                                        |
|                | - ref:${LEOFS_SRC}/apps/leo_manager/snmp/              |
|                |                                                        |
|                | - [snmpa_manager_0|snmpa_manager_1|snmpa_manager_0]    |
+----------------+--------------------------------------------------------+

.. code-block:: erlang

    [
        {sasl, [
                {sasl_error_logger, {file, "./log/sasl-error.log"}},
                {errlog_type, error},
                {error_logger_mf_dir, "./log/sasl"},
                {error_logger_mf_maxbytes, 10485760}, % 10 MB max file size
                {error_logger_mf_maxfiles, 5}         % 5 files max
               ]},
        {mnesia, [
                  {dir, "./work/mnesia"},
                  {dump_log_write_threshold, 50000},
                  {dc_dump_limit,            40}
                 ]},
        {leo_manager, [
                   %% == System Ver ==
                   {system_version, "0.14.0-RC1" },

                   %% == System Configuration ==
                   %%
                   %% n: # of replicated files
                   %% w: # of successes of write-operation
                   %% r: # of successes of read-operation
                   %% d: # of successes of delete-operation
                   %% bit_of_ring: Ring size - 128 = 2^128
                   {system, [{n, 1 },
                             {w, 1 },
                             {r, 1 },
                             {d, 1 },
                             {bit_of_ring, 128}
                            ]},

                   %% == Available Commands ==
                   {available_commands, all },

                   %% == Manager Properties ==
                   %% Mode of server - [master|slave]
                   {manager_mode,     master },
                   %% Partner of manager's alias
                   {manager_partners, ["manager_1@127.0.0.1"] },
                   %% Manager accepatable port number
                   {port_cui,         10010 },
                   {port_json,        10020 },

                   %% # of acceptors
                   {num_of_acceptors_cui,   3},
                   {num_of_acceptors_json, 16},

                   %% Compaction: # of execution of conncurrent
                   {num_of_compact_proc, 3 },

                   %% == Log-specific properties ==
                   %%
                   %% Log output level
                   %%   0: debug
                   %%   1: info
                   %%   2: warning
                   %%   3: error
                   {log_level,    1 },
                   %% Log appender - [file]
                   {log_appender, [
                                   {file, [{path, "./log/app"}]}
                                  ]},

                   %% == Directories ==
                   %%
                   %% Directory of log output
                   {log_dir,          "./log"},
                   %% Directory of mq's db-file
                   {queue_dir,        "./work/queue"},
                   %% Directory of snmp-agent
                   {snmp_agent,       "./snmp/snmpa_manager_0/LEO-MANAGER"}
                  ]},
    ].


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_0/etc/vm.args**

+----------------+--------------------------------------------------------+
|Property        | Configuration                                          |
+================+========================================================+
|${MASTER-IP  }  | Manager-Master IP                                      |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the node
    -name manager_0@${MASTER-IP}

    ## Cookie for distributed erlang
    -setcookie 401321b4

    ## Heartbeat management; auto-restarts VM if it dies or becomes unresponsive
    ## (Disabled by default..use with caution!)
    ##-heart

    ## Enable kernel poll and a few async threads
    +K true
    +A 32

    ## Increase number of concurrent ports/sockets
    ##-env ERL_MAX_PORTS 4096

    ## Tweak GC to run more often
    ##-env ERL_FULLSWEEP_AFTER 10

    ## SNMP Config file
    -config ./snmp/${SNMPA-DIR}/leo_manager_snmp

    ## set up the node with the -hidden flag
    -hidden


LeoFS Manager-Slave
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Manager-Slave's Properties for launch

* **File-1: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_0/etc/app.config**

+----------------+--------------------------------------------------------+
|Property        | Configuration                                          |
+================+========================================================+
|${MASTER-IP}    | Manager-Master node's IP-address                       |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: erlang

    [

        {leo_manager,
                 [

                  %% Manager Configuration
                  {manager_mode,     slave },
                  {manager_partners, ["manager_0@${MASTER-IP}"] },
                  {port,             10011 },
                  {num_of_acceptors, 3},

                  %% Directories
                  {log_dir,          "./log"},
                  {queue_dir,        "./work/queue"},
                  {snmp_agent,       "./snmp/${SNMPA-DIR}/LEO-MANAGER"}
                 ]}
    ].


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_1/etc/vm.args**

+----------------+--------------------------------------------------------+
|Property        | Configuration                                          |
+================+========================================================+
|${SLAVE-IP}     | Manager-Slave IP                                       |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the node
    -name manager_0@${SLAVE-IP}

    ## Cookie for distributed erlang
    -setcookie 401321b4

    ## Heartbeat management; auto-restarts VM if it dies or becomes unresponsive
    ## (Disabled by default..use with caution!)
    ##-heart

    ## Enable kernel poll and a few async threads
    +K true
    +A 32

    ## Increase number of concurrent ports/sockets
    ##-env ERL_MAX_PORTS 4096

    ## Tweak GC to run more often
    ##-env ERL_FULLSWEEP_AFTER 10

    ## SNMP Config file
    -config ./snmp/${SNMPA-DIR}/leo_manager_snmp

    ## set up the node with the -hidden flag
    -hidden


LeoFS Storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Storage's Properties for launch

* **File-1: ${LEOFS_DEPLOYED_DIR}/package/leofs/storage/etc/app.config**

+-------------------------+--------------------------------------------------------+
|Property                 | Configuration                                          |
+=========================+========================================================+
|${OBJECT_STORAGE_DIR}    | Object Storage directory  - Default:"./avs"            |
+-------------------------+--------------------------------------------------------+
|${MANAGER_MASTER_IP}     | Manager-master node's IP-address                       |
+-------------------------+--------------------------------------------------------+
|${MANAGER_SLAVE_IP}      | Manager-slave node's IP-address                        |
+-------------------------+--------------------------------------------------------+
|${SNMPA-DIR}             | SNMPA configuration files directory                    |
|                         |                                                        |
|                         | - ref:${LEOFS_SRC}/apps/leo_storage/snmp/              |
|                         |                                                        |
|                         | - [snmpa_storage_0|snmpa_storage_1|snmpa_storage_0]    |
+-------------------------+--------------------------------------------------------+

.. code-block:: erlang

    {leo_storage, [
                   %% == System Ver ==
                   {system_version, "0.14.0-RC1" },

                   %% == Storage Configuration ==
                   %%
                   %% Object containers properties:
                   %% @param path              - Directory of object-containers
                   %% @param num_of_containers - # of object-containers
                   %%
                   %% Notes:
                   %%   If you set up LeoFS on 'development', default value - "./avs" - is OK.
                   %%   If you set up LeoFS on 'production' or 'staging', You should need to change "volume",
                   %%       And We recommend volume's partition is XFS.
                   %%
                   {obj_containers,     [[{path, ${OBJECT_STORAGE_DIR}}, {num_of_containers, 64}]] },

                   %% leo-manager's nodes
                   {managers,           [${MANAGER_MASTER_IP}, ${MANAGER_SLAVE_IP}] },

                   %% # of virtual-nodes
                   {num_of_vnodes,      168 },

                   %% # of mq-server's processes
                   {num_of_mq_procs,    8 },

                   %% == For Ordning-Reda ==
                   %% Size of stacked objects (bytes)
                   {size_of_stacked_objs,    67108864 },
                   %% Stacking timeout (msec)
                   {stacking_timeout,        5000 },

                   %% == Log-specific properties ==
                   %%
                   {log_level,    1 },
                   {log_appender, [
                                   {file, [{path, "./log/app"}]}
                                  ]},

                   %% == Directories ==
                   %%
                   %% Directory of log output
                   {log_dir,     "./log"},
                   %% Directory of mq's db-files
                   {queue_dir,   "./work/queue"},
                   %% Directory of SNMP-Agent
                   {snmp_agent,  ${SNMPA-DIR}}
                  ]},


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/storage/etc/vm.args**

+-------------------------+--------------------------------------------------------+
|Property                 | Configuration                                          |
+=========================+========================================================+
|${STORAGE_ALIAS}         | Storage node's Alias name                              |
+-------------------------+--------------------------------------------------------+
|${STORAGE_IP}            | Storage node's IP-Address                              |
+-------------------------+--------------------------------------------------------+
|${SNMPA-DIR}             | SNMPA configuration files directory                    |
+-------------------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the node
    -name ${STORAGE_ALIAS}@${STORAGE_IP}

    ## Cookie for distributed erlang
    -setcookie 401321b4

    ## Heartbeat management; auto-restarts VM if it dies or becomes unresponsive
    ## (Disabled by default..use with caution!)
    ##-heart

    ## Enable kernel poll and a few async threads
    +K true
    +A 32

    ## Increase number of concurrent ports/sockets
    ##-env ERL_MAX_PORTS 4096

    ## Tweak GC to run more often
    ##-env ERL_FULLSWEEP_AFTER 10

    ## SNMP Config file
    -config ./snmp/${SNMPA-DIR}/leo_storage_snmp

    ## Sets the maximum number of concurrent processes for this system
    +P 1048576

    ## set up the node with the -hidden flag
    -hidden


LeoFS Gateway
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gateway's Properties for launch

* **File-1: ${LEOFS_DEPLOYED_DIR}/package/leofs/gateway/etc/app.config**

+--------------------+----------------------------------------------------------------------------------+
|Property            | Configuration                                                                    |
+====================+==================================================================================+
|${LISTENING_PORT}   | Gateway's listening port number                                                  |
+--------------------+----------------------------------------------------------------------------------+
|${NUM_OF_LISTENNER} | Numbers of Gateway's listening processes                                         |
+--------------------+----------------------------------------------------------------------------------+
|${MANAGER_MASTER_IP}| Manager-master node's IP-address                                                 |
+--------------------+----------------------------------------------------------------------------------+
|${MANAGER_SLAVE_IP} | Manager-slave node's IP-address                                                  |
+--------------------+----------------------------------------------------------------------------------+
|${SNMPA-DIR}        | SNMPA configuration files directory                                              |
|                    |                                                                                  |
|                    | - ref:${LEOFS_SRC}/apps/leo_gateway/snmp/                                        |
|                    |                                                                                  |
|                    | - [snmpa_gateway_0|snmpa_gateway_1|snmpa_gateway_0]                              |
+--------------------+----------------------------------------------------------------------------------+
|${USE_S3_AUTH}      | Whether using S3 Authentication or not.                                          |
|                    | *Default value is 'true'*                                                        |
+--------------------+----------------------------------------------------------------------------------+
| *Cache related items*                                                                                 |
+--------------------+----------------------------------------------------------------------------------+
|${IS_HTTP_CACHE}    | Method of chache - **http** OR **inner** *(default)*                             |
|                    |                                                                                  |
|                    | +-----+---------------------------------------------------------------------+    |
|                    | |true |HTTP-base cache server - Like a *Varnish* OR *Squid*                 |    |
|                    | +-----+---------------------------------------------------------------------+    |
|                    | |false|Stored objects into the gateway's memory. When READ, the *Etag* of   |    |
|                    | |     |a cache is comapared with backend-storage's *Etag*.                  |    |
|                    | |     | +----------+--------------------------------------------+           |    |
|                    | |     | |matched   | Return a cache                             |           |    |
|                    | |     | +----------+--------------------------------------------+           |    |
|                    | |     | |unmatched | Return an original-object from the storage |           |    |
|                    | |     | +----------+--------------------------------------------+           |    |
|                    | +-----+---------------------------------------------------------------------+    |
+--------------------+----------------------------------------------------------------------------------+
|${CACHE_TOTAL_SIZE} | Total Memory Cache Size in byte                                                  |
|                    | (ex. 4000000000 means using 4GB memory cache)                                    |
+--------------------+----------------------------------------------------------------------------------+
|${CACHE_EXPIRE}     | [**cache-mode:http**] Http Cache Expire in second                                |
+--------------------+----------------------------------------------------------------------------------+
|${CACHE_MAX_C_LEN}  | [**cache-mode:http**] Http Cache Max Content Length in byte                      |
|                    |                                                                                  |
|                    | Note: *LeoFS-gateway can cache up to 1MB*                                        |
+--------------------+----------------------------------------------------------------------------------+
|${CACHE_C_TYPE}     | [**cache-mode:http**] Http Cache Content Type                                    |
|                    |                                                                                  |
|                    | ex-1) ["image/png", "image/jpeg"]                                                |
|                    |                                                                                  |
|                    |       Caching only if its Content-Type was *"image/png"* OR *"image/jpeg"*       |
|                    |                                                                                  |
|                    | ex-2) []                                                                         |
|                    |                                                                                  |
|                    |       When rule is empty, all objects are cached.                                |
+--------------------+----------------------------------------------------------------------------------+
|${CACHE_PATH_PAT}   | [**cache-mode:http**] Http Cache Path Pattern(regular expression)                |
|                    |                                                                                  |
|                    | ex-1) ["/img/.+", "/css/.+"]                                                     |
|                    |                                                                                  |
|                    |       Caching only if its path was *"/img/\*"* or *"/css/\*"*                    |
|                    |                                                                                  |
|                    | ex-2) []                                                                         |
|                    |                                                                                  |
|                    |       When rule is empty, all objects are cached.                                |
+--------------------+----------------------------------------------------------------------------------+


.. code-block:: erlang

    [
        {sasl, [
                {sasl_error_logger, {file, "./log/sasl-error.log"}},
                {errlog_type, error},
                {error_logger_mf_dir, "./log/sasl"},
                {error_logger_mf_maxbytes, 10485760}, % 10 MB max file size
                {error_logger_mf_maxfiles, 5}         % 5 files max
               ]},

        {leo_gateway, [
                %% System Ver
                {system_version, "0.14.0-RC1" },

                %% Gateway Properties:
                {listener, leo_s3_http},
                {http, [
                        %% Use S3-API ? (OR Rest-API)
                        {s3_api, ${USE_S3_AUTH}},
                        %% Gateway port number:
                        {port, ${LISTENING_PORT} },
                        %% # of acceptors:
                        {num_of_acceptors, ${NUM_OF_LISTENNER} },
                        %% max # of layer of directories:
                        {layer_of_dirs, {1, 12} },
                        %% ssl related:
                        {ssl_port,     8443 },
                        {ssl_certfile, "./etc/server_cert.pem" },
                        {ssl_keyfile,  "./etc/server_key.pem" }
                       ]},

                %% Large-object-related properties:
                {large_object, [
                                {max_chunked_objs,  1000  },
                                {max_len_for_obj,   524288000 },
                                {chunked_obj_len,   5242880 },
                                {threshold_obj_len, 5767168 }
                               ]},

                %% Cache-related properties:
                {cache, [
                         %% Use HTTP-cache ?
                         {http_cache, ${IS_HTTP_CACHE}},
                         %% # of Cache workers
                         {cache_workers, 128 },
                         %% Total of Cache capacity (MB)
                         {cache_capacity, ${CACHE_TOTAL_SIZE} },
                         %% Cache expire time. (sec)
                         {cache_expire, ${CACHE_EXPIRE} },
                         %% Acceptable maximum content length (MB)
                         {cache_max_content_len, ${CACHE_MAX_C_LEN} },
                         %% Acceptable content-type(s)
                         {cachable_content_type, ${CACHE_C_TYPE} },
                         %% Acceptable URL-Pattern(s)
                         {cachable_path_pattern, ${CACHE_PATH_PAT} }
                        ]},

                %% Timeout when request from gateway to storage ==
                {timeout, [
                           {level_1,  5000},
                           {level_2,  7000},
                           {level_3, 10000},
                           {level_4, 20000},
                           {level_5, 30000}
                          ]},

                %% Manager - leo-manager's nodes
                {managers, [${MANAGER_MASTER_IP}, ${MANAGER_SLAVE_IP}] },

                %% Log-specific properties
                %%   - Log output level
                %%         0: debug
                %%         1: info
                %%         2: warning
                %%         3: error
                {log_level,    1 },
                %% Log appender - [file]
                {log_appender, [
                                {file, [{path, "./log/app"}]}
                               ]},

                %% Directory of log output
                {log_dir,     "./log"},
                %% Directory of mq's db-files
                {queue_dir,   "./work/queue"},
                %% Directory of snmp-agent
                {snmp_agent,  "./snmp/snmpa_gateway_0/LEO-GATEWAY"}
               ]},


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/gateway/etc/vm.args**

+--------------------+--------------------------------------------------------+
|Property            | Configuration                                          |
+====================+========================================================+
|${GATEWAY_ALIAS}    | Gateway node's Alias name                              |
+--------------------+--------------------------------------------------------+
|${GATEWAY_IP}       | Gateway node's IP-Address                              |
+--------------------+--------------------------------------------------------+
|${SNMPA-DIR}        | SNMPA configuration files directory                    |
+--------------------+--------------------------------------------------------+

.. code-block:: bash

    ## Name of the node
    -name ${GATEWAY_ALIAS}@${GATEWAY_IP}

    ## Cookie for distributed erlang
    -setcookie 401321b4

    ## Heartbeat management; auto-restarts VM if it dies or becomes unresponsive
    ## (Disabled by default..use with caution!)
    ##-heart

    ## Enable kernel poll and a few async threads
    +K true
    +A 32

    ## Increase number of concurrent ports/sockets
    ##-env ERL_MAX_PORTS 4096

    ## Tweak GC to run more often
    ##-env ERL_FULLSWEEP_AFTER 10

    ## SNMP Config file
    -config ./snmp/${SNMPA-DIR}/leo_gateway_snmp

    ## Sets the maximum number of concurrent processes for this system
    +P 1048576

    ## set up the node with the -hidden flag
    -hidden

