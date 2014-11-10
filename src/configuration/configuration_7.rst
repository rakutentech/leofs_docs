.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _leofs-with-nfs-label:

.. index::
   pair: Configuration; LeoFS watchdog

LeoFS watchdog
==============

.. index::
   pair: Watchdog; Purpose

Purpose
-------


.. index::
   pair: Watchdog; Getting Started

Getting Started
---------------

Pre-requirement
~~~~~~~~~~~~~~~

.. note:: We have checked this mechanism with CentOS 6.5 and Ubuntu Server 13.10/14.04 LTS but we're goinng to investigate other OS such as FreeBSD and SmartOS. And also, disk monitoring is using the |iostat-command|, you need to install it before getting started the watchdog.


.. index::
   pair: Watchdog; LeoFS Gateway

Configuration of LeoFS Gateway
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Modify |leo_gateway_conf|

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
| watchdog.cpu.is_enabled              | false             | Is cpu-watchdog enabled *[true|false]*       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.interval                | 5                 | Watch interval(sec)                          |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.threshold_cpu_load_avg  | 2                 | Threshold CPU load avg for 1min/5min         |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.threshold_cpu_util      | 100               | Threshold CPU load util(%)                   |
+--------------------------------------+-------------------+----------------------------------------------+
| **IO**                                                                                                  |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.is_enabled               | false             | Is io-watchdog enabled *[true|false]*        |
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

- Modify |leo_storage_conf|

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
| watchdog.cpu.is_enabled              | false             | Is cpu-watchdog enabled *[true|false]*       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.interval                | 5                 | Watch interval(sec)                          |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.threshold_cpu_load_avg  | 2                 | Threshold CPU load avg for 1min/5min         |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.cpu.threshold_cpu_util      | 100               | Threshold CPU load util(%)                   |
+--------------------------------------+-------------------+----------------------------------------------+
| **IO**                                                                                                  |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.is_enabled               | false             | Is io-watchdog enabled *[true|false]*        |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.interval                 | 1                 | Watch interval *(sec)*                       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.threshold_input_per_sec  | 134217728         | Threshold input size/sec *(byte)*            |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.io.threshold_output_per_sec | 134217728         | Threshold output size/se *(byte)*            |
+--------------------------------------+-------------------+----------------------------------------------+
| **DISK**                                                                                                |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.is_enabled             | false             | Is disk-watchdog enabled *[true|false]*      |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.interval               | 1                 | Watch interval *(sec)*                       |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.threshold_disk_use     | 85                | Threshold disk use *(%)*                     |
+--------------------------------------+-------------------+----------------------------------------------+
| watchdog.disk.threshold_disk_util    | 95                | Threshold disk util *(%)*                    |
+--------------------------------------+-------------------+----------------------------------------------+


.. |iostat-command| raw:: html

   <a href="http://en.wikipedia.org/wiki/Iostat" target="_blank">iostat command</a>

.. |leo_gateway_conf| raw:: html

   <a href="https://github.com/leo-project/leo_gateway/blob/master/priv/leo_gateway.conf" target="_blank">leo_gateway.conf</a>

.. |leo_storage_conf| raw:: html

   <a href="https://github.com/leo-project/leo_storage/blob/master/priv/leo_storage.conf" tarrget="_blank">leo_storage.conf</a>
