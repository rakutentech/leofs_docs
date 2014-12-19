.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _watchdog-label:

.. index::
   pair: Configuration; Watchdog

Watchdog
========

.. index::
   pair: Watchdog; Purpose

Since
-------

LeoFS v1.2.2


Purpose
-------

The watchdog mechanism monitors CPU, Erlang-IO and Disk status in order to keep running a node stably. Also, this mechanism communicates ``the MQ mechanism`` and `the auto-compaction <configuration_8.html>`_/compaction mechanism.

.. index::
   pair: Watchdog; Getting Started

Getting Started
---------------

Pre-requirement
~~~~~~~~~~~~~~~

.. note:: We have supported this mechanism for CentOS 6.5/7.0, Ubuntu Server 14.04 LTS, FreeBSD and SmartOS. And also, ``disk monitoring`` is using the |iostat-command| and |df-command|, you need to install it before getting started the watchdog.


.. index::
   pair: Watchdog; LeoFS Gateway

Configuration of LeoFS Gateway
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note:: All default setting of the watchdogs are disabled. Before getting use this mechanism, you need to turn on them.

Modify |leo_gateway_conf| at the section of wathdog.


LeoFS Gateway's watchdog properties:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+--------------------------------------+-------------------+----------------------------------------------+
| Property                             | Default value     | Description                                  |
+======================================+===================+==============================================+
| **rex** *(rpc-server)*                                                                                  |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.rex.interval                | 5                 | Watch interval *(sec)*                       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.rex.threshold_mem_capacity  | 33554432          | Threshold memory capacity for binary *(byte)*|
+--------------------------------------+-------------------+----------------------------------------------+
| **CPU**                                                                                                 |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.is_enabled              | false             | Is cpu-watchdog enabled? *[true|false]*      |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.interval                | 5                 | Watch interval(sec)                          |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.raised_error_times      | 3                 | Raised error times                           |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.threshold_cpu_load_avg  | 5.0               | Threshold CPU load avg for 1min/5min         |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.threshold_cpu_util      | 100               | Threshold CPU load util *(%)*                |
+--------------------------------------+-------------------+----------------------------------------------+
| **Erlang IO**                                                                                           |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.is_enabled               | false             | Is io-watchdog enabled? *[true|false]*       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.interval                 | 1                 | Watch interval *(sec)*                       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.threshold_input_per_sec  | 134217728         | Threshold input size/sec *(byte)*            |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.threshold_output_per_sec | 134217728         | Threshold output size/se *(byte)*            |
+--------------------------------------+-------------------+----------------------------------------------+

\

.. index::
   pair: Watchdog; LeoFS Storage

Configuration of LeoFS Storage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Modify |leo_storage_conf| at the section of watchdog.

LeoFS Storage's watchdog properties:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+--------------------------------------+-------------------+----------------------------------------------+
| Property                             | Default value     | Description                                  |
+======================================+===================+==============================================+
| **rex** *(rpc-server)*                                                                                  |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.rex.interval                | 5                 | Watch interval *(sec)*                       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.rex.threshold_mem_capacity  | 33554432          | Threshold memory capacity for binary *(byte)*|
+--------------------------------------+-------------------+----------------------------------------------+
| **CPU**                                                                                                 |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.is_enabled              | false             | Is cpu-watchdog enabled? *[true|false]*      |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.interval                | 5                 | Watch interval *(sec)*                       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.raised_error_times      | 3                 | Raised error times                           |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.threshold_cpu_load_avg  | 5.0               | Threshold CPU load avg for 1min/5min         |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.threshold_cpu_util      | 100               | Threshold CPU load util *(%)*                |
+--------------------------------------+-------------------+----------------------------------------------+
| **Erlang IO**                                                                                           |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.is_enabled               | false             | Is io-watchdog enabled? *[true|false]*       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.interval                 | 1                 | Watch interval *(sec)*                       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.threshold_input_per_sec  | 134217728         | Threshold input size/sec *(byte)*            |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.threshold_output_per_sec | 134217728         | Threshold output size/se *(byte)*            |
+--------------------------------------+-------------------+----------------------------------------------+
| **DISK**                                                                                                |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.is_enabled             | false             | Is disk-watchdog enabled? *[true|false]*     |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.interval               | 1                 | Watch interval *(sec)*                       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.raised_error_times     | 3                 | Raised error times                           |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.threshold_disk_use     | 85                | Threshold disk use *(%)*                     |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.threshold_disk_util    | 100               | Threshold disk util *(%)*                    |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.threshold_disk_rkb     | 262144            | Threshold disk read KB/sec                   |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.threshold_disk_wkb     | 262144            | Threshold disk write KB/sec                  |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.target_devices         | []                | Target devices for checking disk utilization |
+--------------------------------------+-------------------+----------------------------------------------+

See Also
^^^^^^^^

* `LeoFS Storage configuration  <configuration_2.html>`_
* `LeoFS Auto-compaction configuration <configuration_8.html>`_


.. |iostat-command| raw:: html

   <a href="http://en.wikipedia.org/wiki/Iostat" target="_blank">iostat command</a>

.. |df-command| raw:: html

   <a href="http://en.wikipedia.org/wiki/Df_%28Unix%29" target="_blank">df command</a>

.. |leo_gateway_conf| raw:: html

   <a href="https://github.com/leo-project/leo_gateway/blob/master/priv/leo_gateway.conf" target="_blank">leo_gateway.conf</a>

.. |leo_storage_conf| raw:: html

   <a href="https://github.com/leo-project/leo_storage/blob/master/priv/leo_storage.conf" tarrget="_blank">leo_storage.conf</a>
