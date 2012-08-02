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

   # sudo apt-get install libtool libncurses5-dev


Download "Erlang R14B04"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
::

   $ cd $WORK_DIR
   $ wget http://www.erlang.org/download/otp_src_R14B04.tar.gz

Build for Linux (CentOS, Debian and Others)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
::

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

Confirm
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
::

    $ erl
    Erlang R14B04 (erts-5.8.4) [source] [64-bit] [smp:2:2] [rq:2] [async-threads:0] [kernel-poll:false]

    Eshell V5.8.5  (abort with ^G)
    1>


XFS-related
------------

.. note:: If You deploy LeoFS on your **DEV environments**, You does NOT need this operaion. But If You deploy LeoFS on your **PRODUCTION environments**, You need to install XFS-libs and create an XFS's partition.

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

::

   # mkfs.xfs -d agcount=13 -l size=32m /dev/sda3

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
      |      |--- ecache_app/
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
      |      |--- leo_redundant_manager/
      |      |--- leo_statistics/
      |      |--- leo_storage/
      |      |--- meck/
      |      |--- mochiweb/
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
      |      |        |--- ets/
      |      |        |--- lib/
      |      |        |--- log/
      |      |        |--- releases/
      |      |        |--- snmp/
      |      |        `--- work/
      |      |--- manager_0/
      |      |        |--- bin/
      |      |        |--- erts-5.8.5/
      |      |        |--- ets/
      |      |        |--- lib/
      |      |        |--- log/
      |      |        |--- releases/
      |      |        |--- snmp/
      |      |        `--- work/
      |      |--- manager_1/
      |      |        |--- bin/
      |      |        |--- erts-5.8.5/
      |      |        |--- ets/
      |      |        |--- lib/
      |      |        |--- log/
      |      |        |--- releases/
      |      |        |--- snmp/
      |      |        `--- work/
      |      `--- storage/
      |               |--- bin/
      |               |--- erts-5.8.5/
      |               |--- ets/
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
| log/ring    | For RING's Dump files                                  |
+-------------+--------------------------------------------------------+
| log/sasl    | For Erlang-SASL Logs                                   |
+-------------+--------------------------------------------------------+
| **work/**                                                            |
+-------------+--------------------------------------------------------+
| work/mnesia/| For System internal info which is stored into 'Mnesia' |
+-------------+--------------------------------------------------------+
| work/queue  | For Message Queue's data which is stored into 'bitcask'|
+-------------+--------------------------------------------------------+

- ref: Basho bitcask <https://github.com/basho/bitcask>


::

   ${LEOFS_DEPLOYED_DIR}
     |      `--- storage/
     |               |--- bin/
     |               |--- erts-5.8.5/
     |               |--- ets/
     |               |--- lib/
     |               |--- log/
     |               |     |--- app/
     |               |     |--- ring/
     |               |     `--- sasl/
     |               |--- releases/
     |               |--- snmp/
     |               `--- work/
                           |--- mnesia
                           `--- queue


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

::

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
                 ]}
    ].

Firewall Rules
--------------

+----------------+-----------+-----------------+--------------------------+
| Subsystem      | Direction | Ports           | Notes                    |
+================+===========+=================+==========================+
| Manager-Master | Incoming  | 10010/*         | admin console            |
+----------------+-----------+-----------------+--------------------------+
| Manager-Master | Incoming  | 4369/*          | erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Manager-Master | Outgoing  | \*/4369         | erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Incoming  | 10011/*         | admin console            |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Incoming  | 4369/*          | erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Manager-Slave  | Outgoing  | \*/4369         | erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+
| Storage        | Incoming  | 4369/*          | erlang RPC from others   |
+----------------+-----------+-----------------+--------------------------+
| Storage        | Outgoing  | \*/4369         | erlang RPC to others     |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 8080/*          | HTTP listen port         |
+----------------+-----------+-----------------+--------------------------+
| Gateway        | Incoming  | 4369/*          | erlang RPC from others   |
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

::

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
                  {manager_partners, ["manager_1@${SLAVE-IP}"] },
                  {port,             10010 },
                  {num_of_acceptors, 3},
                  %% Directories
                  {log_dir,          "./log"},
                  {queue_dir,        "./work/queue"},
                  {snmp_agent,       "./snmp/${SNMPA-DIR}/LEO-MANAGER"}
                 ]}
    ].


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_0/etc/vm.config**

+----------------+--------------------------------------------------------+
|Property        | Configuration                                          |
+================+========================================================+
|${MASTER-IP  }  | Manager-Master IP                                      |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

::

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

::

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
                  {manager_partners, ["manager_0@${MASTER-IP}"] },
                  {port,             10010 },
                  {num_of_acceptors, 3},
                  %% Directories
                  {log_dir,          "./log"},
                  {queue_dir,        "./work/queue"},
                  {snmp_agent,       "./snmp/${SNMPA-DIR}/LEO-MANAGER"}
                 ]}
    ].


* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/manager_1/etc/vm.config**

+----------------+--------------------------------------------------------+
|Property        | Configuration                                          |
+================+========================================================+
|${SLAVE-IP}     | Manager-Slave IP                                       |
+----------------+--------------------------------------------------------+
|${SNMPA-DIR}    | SNMPA configuration files directory                    |
+----------------+--------------------------------------------------------+

::

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

::

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
        {leo_storage,
                 [
                  %% Storage Configuration
                  {obj_containers,     [{{volume, "${OBJECT_STORAGE_DIR}"}, {num_of_containers, 64}}] },
                  {managers,           ["manager_0@${MANAGER_MASTER_IP}", "manager_1@${MANAGER_SLAVE_IP}"] },
                  {num_of_replicators, 32 },
                  {num_of_repairers,   32 },
                  {num_of_mq_procs,    8 },

                  %% Log-specific properties.
                  {log_level,    1 },
                  {log_appender, [file]},

                  %% Directories
                  {log_dir,     "./log"},
                  {queue_dir,   "./work/queue"},
                  {snmp_agent,  "./snmp/${SNMPA-DIR}/LEO-STORAGE"}
                 ]},
                 .
                 .

* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/storage/etc/vm.config**

+-------------------------+--------------------------------------------------------+
|Property                 | Configuration                                          |
+=========================+========================================================+
|${STORAGE_ALIAS}         | Storage node's Alias name                              |
+-------------------------+--------------------------------------------------------+
|${STORAGE_IP}            | Storage node's IP-Address                              |
+-------------------------+--------------------------------------------------------+
|${SNMPA-DIR}             | SNMPA configuration files directory                    |
+-------------------------+--------------------------------------------------------+

::

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



LeoFS Gateway
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gateway's Properties for launch

* **File-1: ${LEOFS_DEPLOYED_DIR}/package/leofs/gateway/etc/app.config**

+--------------------+--------------------------------------------------------+
|Property            | Configuration                                          |
+====================+========================================================+
|${LISTENING_PORT}   | Gateway's listening port number                        |
+--------------------+--------------------------------------------------------+
|${NUM_OF_LISTENNER} | Numbers of Gateway's listening processes               |
+--------------------+--------------------------------------------------------+
|${MANAGER_MASTER_IP}| Manager-master node's IP-address                       |
+--------------------+--------------------------------------------------------+
|${MANAGER_SLAVE_IP} | Manager-slave node's IP-address                        |
+--------------------+--------------------------------------------------------+
|${CACHE_PLUGIN}     | (Optional) Http Cache Module(mochiweb_mod_cache)       |
+--------------------+--------------------------------------------------------+
|${CACHE_EXPIRE}     | (Optional) Http Cache Expire in second                 |
|                    | (ex. 60 means one minute)                              |
+--------------------+--------------------------------------------------------+
|${CACHE_MAX_C_LEN}  | (Optional) Http Cache Max Content Length in byte       |
|                    |                                                        |
|                    | (ex. 8192 means caching if contents was less than 8KB) |
+--------------------+--------------------------------------------------------+
|${CACHE_C_TYPE}     | (Optional) Http Cache Content Type                     |
|                    |                                                        |
|                    | (ex. ["image/png", "image/jpeg"] means caching only if |
|                    |                                                        |
|                    | its Content-Type was "image/png" or "image/jpeg"       |
+--------------------+--------------------------------------------------------+
|${CACHE_PATH_PAT}   | (Optional) Http Cache Path Pattern(regular expression) |
|                    |                                                        |
|                    | (ex. ["/img/.+", "/css/.+" means caching only if       |
|                    |                                                        |
|                    | its path was "/img/*" or "/css/*"                      |
+--------------------+--------------------------------------------------------+
|${SNMPA-DIR}        | SNMPA configuration files directory                    |
|                    |                                                        |
|                    | - ref:${LEOFS_SRC}/apps/leo_gateway/snmp/              |
|                    |                                                        |
|                    | - [snmpa_gateway_0|snmpa_gateway_1|snmpa_gateway_0]    |
+--------------------+--------------------------------------------------------+
|${CACHE_TOTAL_SIZE} | Total Memory Cache Size in byte                        |
|                    | (ex. 4000000000 means using 4GB memory cache)          |
+--------------------+--------------------------------------------------------+


::

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
        {leo_gateway,
                 [
                  %% Gateway Configuration
                  {port, ${LISTENING_PORT} },
                  {num_of_acceptors, ${NUM_OF_LISTENNER} },
                  {managers, ["manager_0@${MANAGER_MASTER_IP}", "manager_1@${MANAGER_SLAVE_IP}"] },

                  %% Cache Configuration(Optional)
                  {cache_plugin, ${CACHE_PLUGIN} },
                  {cache_expire, ${CACHE_EXPIRE} },
                  {cache_max_content_len, ${CACHE_MAX_C_LEN} },
                  {cachable_path_pattern, ${CACHE_PATH_PAT} },
                  {cachable_content_type, ${CACHE_C_TYPE} },

                  %% Log-specific properties.
                  {log_level,    0 },
                  {log_appender, [file]},

                  %% Directories
                  {log_dir,     "./log"},
                  {queue_dir,   "./work/queue"},
                  {snmp_agent,  "./snmp/${SNMPA-DIR}/LEO-GATEWAY"}
                 ]},
        {ecache_app,
                [
                   {rec_max_size, ${CACHE_TOTAL_SIZE} },
                   {proc_num, 32}
                ]},
                 .
                 .

* **File-2: ${LEOFS_DEPLOYED_DIR}/package/leofs/gateway/etc/vm.config**

+--------------------+--------------------------------------------------------+
|Property            | Configuration                                          |
+====================+========================================================+
|${GATEWAY_ALIAS}    | Gateway node's Alias name                              |
+--------------------+--------------------------------------------------------+
|${GATEWAY_IP}       | Gateway node's IP-Address                              |
+--------------------+--------------------------------------------------------+
|${SNMPA-DIR}        | SNMPA configuration files directory                    |
+--------------------+--------------------------------------------------------+

::

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


