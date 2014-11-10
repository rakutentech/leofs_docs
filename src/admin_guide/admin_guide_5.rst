.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    Compaction commands

Compaction Commands
===================

Remove logical deleted objects and meta data and check the current disk usage

+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| **Shell**                                                                            | **Description**                                                                                      |
+======================================================================================+======================================================================================================+
| **Compaction Commands**                                                                                                                                                                     |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`compact-start <compact-start>` <storage-node> (all|<num-of-targets>) | * Remove unnecessary objects from the node                                                           |
| [<num-of-compaction-proc>]                                                           | * ``num-of-targets``: It controls the number of containers in parallel                               |
|                                                                                      | * ``num-of-compaction-procs``: It controls the number of procs to execute the compaction in parallel |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`compact-suspend <compact-suspend>` <storage-node>                    | * Suspend the compaction                                                                             |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`compact-resume <compact-resume>` <storage-node>                      | * Resume the compaction                                                                              |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`compact-status <compact-status>` <storage-node>                      | * See the current compaction status                                                                  |
|                                                                                      | * Compaction's status: ``idle``, ``running``, ``suspend``                                            |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`diagnose-start <diagnose-start>` <storage-node>                      | * ``v1.1.5-`` Diagnose data of a target storage node                                                 |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| **Disk Usage**                                                                                                                                                                              |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`du <du>` <storage-node>                                              | * See the current disk usages                                                                        |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`du detail <du-detail>` <storage-node>                                | * See the current disk usages in the details                                                         |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+

\


.. image:: ../../_static/images/leofs-compaction-state-transition.png
   :width: 640px

\


.. _compact-start:

.. index::
    pair: Compaction commands; compact-start-command

compact-start <storage-node> (all | <num-of-targets>) [<num-of-compaction-procs>]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Remove unnecessary objects from the node
* num-of-targets: It controls the number of containers in parallel
* num-of-compaction-procs: It controls the number of procs to execute the compaction in parallel


.. note:: Default ``<num-of-compation-procs>`` is '3' - You can control the number of processes to execute compaction in parallel. It enables you to get maximum performance by setting an appropriate number corresponding to the number of cores.

.. code-block:: bash

    ## All compaction-targets will be executed with 3 concurrent processes
    ## (default concurrency is 3)
    $ leofs-adm compact-start storage_0@127.0.0.1 all
    OK

    ## Number of compaction-targets will be executed with 2 concurrent processes
    $ leofs-adm compact-start storage_0@127.0.0.1 5 2
    OK

\

.. _compact-suspend:

.. index::
    pair: Compaction commands; compact-suspend-command

compact-suspend <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Suspend the compaction

.. code-block:: bash

    $ leofs-adm compact-suspend storage_0@127.0.0.1
    OK

\


.. _compact-resume:

.. index::
    pair: Compaction commands; compact-resume-command

compact-resume <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Resume the compaction

.. code-block:: bash

    $ leofs-adm compact-resume storage_0@127.0.0.1
    OK

\

.. _compact-status:

.. index::
    pair: Compaction commands; compact-status-command


compact-status <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* See the current compaction status
* Compaction's status: ``idle``, ``running`` and ``suspend``

.. code-block:: bash

  $ leofs-adm compact-status storage_0@127.0.0.1
          current status: running
   last compaction start: 2013-03-04 12:39:47 +0900
           total targets: 64
    # of pending targets: 5
    # of ongoing targets: 3
    # of out of targets : 56

\

.. _diagnose-start:

.. index::
    pair: Compaction commands; diagnose-start-command

diagnose-start <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* ``v1.1.5-`` Diagnose data of a target storage node
* See also: :ref:`LeoFS Storage data-diagnosis-log format <data_diagnosis_log>`

.. code-block:: bash

    $ leofs-adm diagnose-start storage_0@127.0.0.1
    OK

\

.. _du:

.. index::
    pair: Compaction commands; du-command

du <storage-node>
^^^^^^^^^^^^^^^^^

See the current disk usages

.. code-block:: bash

    $ leofs-adm du storage_0@127.0.0.1
     active number of objects: 19968
      total number of objects: 39936
       active size of objects: 198256974.0
        total size of objects: 254725020.0
         ratio of active size: 77.83%
        last compaction start: 2013-03-04 12:39:47 +0900
          last compaction end: 2013-03-04 12:39:55 +0900


.. _du-detail:

.. index::
    pair: Compaction commands; du-detail-command

du detail <storage-node>
^^^^^^^^^^^^^^^^^^^^^^^^

See the current disk usages in the details


.. code-block:: bash

    $ leofs-adm du detail storage_0@127.0.0.1
    [du(storage stats)]
                    file path: /home/leofs/dev/leofs/package/leofs/storage/avs/object/0.avs
     active number of objects: 320
      total number of objects: 640
       active size of objects: 3206378.0
        total size of objects: 4082036.0
         ratio of active size: 78.55%
        last compaction start: 2013-03-04 12:39:47 +0900
          last compaction end: 2013-03-04 12:39:55 +0900
    .
    .
    .
                    file path: /home/leofs/dev/leofs/package/leofs/storage/avs/object/63.avs
     active number of objects: 293
      total number of objects: 586
       active size of objects: 2968909.0
        total size of objects: 3737690.0
         ratio of active size: 79.43%
        last compaction start: ____-__-__ __:__:__
          last compaction end: ____-__-__ __:__:__

