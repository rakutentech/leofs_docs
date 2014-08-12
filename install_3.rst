.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: LeoFS; Installation

Install LeoFS
-------------

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
| Directory   | Description                                            |
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

