.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _auto-compaction-label:

.. index::
   pair: Configuration; Auto-compaction

Auto-compaction [1.2.2-]
========================

.. index::
   pair: Auto-compaction; Purpose

Purpose
-------

The compaction mechanism supports to remove unnecessary objects in a node of LeoFS Storage. And also, this mechanism does not block retrieving and puting object during data-compaction as much as possible.


Since LeoFS 1.2.2 it is possible to execute automatic data-compaction in order to reduce administration-costs and Keep running a storage cluster stably. You need to only modify the auto-compaction configuration.



.. index::
   pair: Auto-compaction; Getting Started

Getting Started
---------------

Pre-requirement
~~~~~~~~~~~~~~~

.. note:: The auto-compaction mechanism communicates with `the watchdog mechanism <configuration_7.html>`_  in order to restrict load of the processing and keep running the node. If you would like to use that, you need to set up `the watchdog configuration <configuration_7.html>`_.

\

.. index::
   pair: Auto-compaction; LeoFS Storage

Configuration of LeoFS Storage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Modify |leo_storage_conf| at the section of auto-compaction.

LeoFS Storage's auto-compaction properties:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+-----------------------------------------------------+-------------------+---------------------------------------------------------------------------------------------------------------+
| Property                                            | Default value     | Description                                                                                                   |
+=====================================================+===================+===============================================================================================================+
| autonomic_op.compaction.is_enabled                  | false             | * Is enabled auto-compaction?  *[true|false]*                                                                 |
+-----------------------------------------------------+-------------------+---------------------------------------------------------------------------------------------------------------+
| autonomic_op.compaction.parallel_procs              | 1                 | * Number of parallel procs of data-compaction                                                                 |
+-----------------------------------------------------+-------------------+---------------------------------------------------------------------------------------------------------------+
| autonomic_op.compaction.threshold_active_size_ratio | 70                | * Warning ratio of active size *(%)*                                                                          |
|                                                     |                   | * NOT affects the auto-compaction by this configuration yet. We plan to support this configuration with v1.4. |
+-----------------------------------------------------+-------------------+---------------------------------------------------------------------------------------------------------------+
| autonomic_op.compaction.threshold_active_size_ratio | 60                | * Threshold ratio of active size *(%)*                                                                        |
|                                                     |                   | * (Size of active objects / Size of total objects) * 100 *(%)*                                                |
|                                                     |                   | * When it was less than the ratio, LeoFS Storage executes data-compaction automatically                       |
+-----------------------------------------------------+-------------------+---------------------------------------------------------------------------------------------------------------+


See Also
^^^^^^^^

* `LeoFS Storage configuration  <configuration_2.html>`_
* `LeoFS Watchdog configuration <configuration_7.html>`_


.. |leo_storage_conf| raw:: html

   <a href="https://github.com/leo-project/leo_storage/blob/master/priv/leo_storage.conf" tarrget="_blank">leo_storage.conf</a>
