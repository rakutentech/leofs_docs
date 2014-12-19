.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   FAQ LeoFS administration

===========================
FAQ: LeoFS Administration
===========================

.. index::
   pair: FAQ LeoFS administration; Features


Where can I get the packages for LeoFS?
---------------------------------------

You can get the packages by platform from |leofs-website|.
LeoFS package for FreeBSD has been published at |fresh-ports| and for SmartOS has been published at |smartos-package|, which is made by |project-fifo|.

\

How can I run my LeoFS cluster automatically?
---------------------------------------------

Currently after restarting a storage node,  you need to issue the resume command with leofs-adm script manually like this.

.. code-block:: bash

    leofs-adm resume storage_0@127.0.0.1


We're planing to provide LeoFS's autonomic-operation like `auto-compaction`, `auto-rebalance` and others with LeoFS v1.2.

\

How do multiple users login into LeoFS Manager's console at the same time?
--------------------------------------------------------------------------

Answer 1:
^^^^^^^^^^

Actually, there is no login status in LeoFS, but the number of listening tcp connections is limited by leo_manger.conf (default: console.acceptors.cui = 3).
So while using default settings, 3 connections can be connected to a manager in parallel.

Answer 2:
^^^^^^^^^^

Since LeoFS v1.1.0, LeoFS have the more powerful alternative option |leofs-adm| command.
This command have not only same functionalities with the existing telnet way but also do NOT keep an tcp connection established for long time(connect only when issueing a command).
So you do not need to worry about the number of tcp connections.

\

The result of the du command can be different with the actual disk-usage
-------------------------------------------------------------------------

In order to reduce system resource usage for calculating the result of the du command, LeoFS keep that information on memory and when stopping itself, LeoFS save those data into a local file.
Then when restarting, LeoFS load those on memory.

So if LeoFS is stopped unintentionally like killing by OOM killer, those data can become inconsistent with actual usage.

If you get into this situation, you can recover those data by issueing the compact-start command to the node. after finishing the compaction, the result of the du command will be consitent with actual usage.

See Also:
    * `du-command <../admin_guide/admin_guide_5.html#du>`_

\

When issueing the recover node command the LeoFS can get into high load
------------------------------------------------------------------------

Since the `recover-node command <../admin_guide/admin_guide_4.html#recover-node-command>`_ can lead to issue lots of disk I/O and consume network bandwidth, if you face to issue the recover-node to multiple nodes at once, LeoFS can get into high load and become unresponsive. So we recommend you execute the recover-node command to target nodes one by one.

If this solution could not work for you, you're able to control how much recover-node consume system resources by changing the MQ-related parameters in `leo_storage.conf <../configuration/configuration_2.html>`_

See Also:
    * `recover-node command <../admin_guide/admin_guide_4.html#recover-node-command>`_
    * `leo_storage.conf <../configuration/configuration_2.html>`_

\


What should I do when Too many processes errors happen?
-------------------------------------------------------

LeoFS usually try to keep the number of Erlang processes as minimum as possible, but there are some exceptions when doing something asynchronously.

* Replicating an object to the non-primary assigned nodes
* Retrying to replicate an object when the previous attempt failed

Given that LeoFS suffered from very high load AND there are some nodes downed for some reason, The number of Erlang processes gradually have increased and might have reached the sysmte limit.

We recommend users to set an appropriate value which depends on your workload to the ``+P option``. Also if the ``+P option`` does NOT work for you, there are some possibilities that some external system resources like disk, network equipments have broken, Please check out the dmesg/syslog on your sysmtem.

See Also:
    * |erlang-p|

\


Why does starting a leo_storage using bitcask as metadata take too much time?
-----------------------------------------------------------------------------

When starting a leo_storage with bitcask, since leo_storage always call the ``bitcask:merge`` operation, starting process may take too much time if leo_storage stored lots of objects. We recommend users to replace ``bitcask`` with ``leveldb`` by using |b2l|.


.. |leofs-adm| raw:: html

   <a href="https://github.com/leo-project/leofs/blob/master/leofs-adm" target="_blank">leofs-adm</a>

.. |leofs-website| raw:: html

   <a href="http://leo-project.net/leofs/download.html" target="_blank">LeoFS download page</a>

.. |fresh-ports| raw:: html

   <a href="http://www.freshports.org/databases/leofs" target="_blank">Fresh ports/database</a>

.. |smartos-package| raw:: html

   <a href="http://release.project-fifo.net/pkg/rel/" target="_blank">LeoFS packages for SmartOS</a>

.. |project-fifo| raw:: html

   <a href="http://project-fifo.net" target="_blank">Project FiFo</a>

.. |erlang-p| raw:: html

   <a href="http://erlang.org/doc/man/erl.html#+P" target="_blank">Erlang - +P</a>

.. |b2l| raw:: html

   <a href="https://github.com/leo-project/leofs_utils/tree/develop/tools/b2l" target="_blank">Tool:Converting metadata from bitcask to leveldb</a>
