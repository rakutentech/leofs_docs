.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   FAQ fundamentals

=======================
FAQ: LeoFS Fundamentals
=======================

.. index::
   pair: FAQ fundamentals; What kind of storage is leofs?

What kind of storage is leofs?
------------------------------

|LeoFS| is a highly scalable, fault-tolerant |object_storage| for the Web. Significantly, LeoFS supports huge amount and various kind unstructured data such as photo, movie, document, log data and so on.

Operationally, LeoFS features multi-master replication with automated failover and built-in horizontal scaling via |ConsistentHashing|.

See Also:
    * `LeoFS Overview <intro.html>`_
    * |LeoFS_Slide|

----

.. index::
   pair: FAQ fundamentals; What are typical uses for LeoFS?

What are typical uses for LeoFS?
--------------------------------

If you are searching a storage system that is able to store huge amount and various kind of files such as photo, movie, log data and so on, LeoFS is suitable for that.

This is because LeoFS is a highly available, distributed storage system. Also, LeoFS can be used to store a lot of data efficiently, safely, and inexpensively.

See Also:
    * `LeoFS Overview <intro.html>`_

----

.. index::
   pair: FAQ fundamentals; What is benefit for LeoFS users?

What is benefit for LeoFS users?
--------------------------------

Advantage
^^^^^^^^^

LeoFS is supporting the following features:

* S3-API Support
    * LeoFS is an Amazon S3 compatible storage system.
    * Switch to LeoFS to decrease your cost from more expensive public-cloud solutions.
* Large Object Support
    * LeoFS can handle files with more than GB
* Multi Data Center Replication
    * LeoFS is a highly scalable, fault-tolerant distributed file system without |SPOF|.
    * LeoFS's cluster can be viewed as ONE-HUGE storage. It consists of a set of loosely connected nodes.
    * We can build a global scale storage system with easy operations
* High Performance without |SPOF|
    * According to the original cache mechanism and sophisticated innternal architecture, LeoFS keeps high performance regardless of amount and kind of data without |SPOF|.

In near future, LeoFS is going to provide the powerful features with LeoFS v1.2.

* NFS support
* LeoFS insight
* OpenStack integration


For business managers
^^^^^^^^^^^^^^^^^^^^^

* Storing confidential/sensitive data internally
* Saving cost to use commodity servers
* Increasing service level with speedy response
* Expanding business globally


For administrators
^^^^^^^^^^^^^^^^^^

* Easy to install with the packages
* Easy to operate with `LeoCenter <leo_center.html>`_

----

.. index::
   pair: FAQ fundamentals; What is architecture of LeoFS?

What is architecture of LeoFS?
------------------------------

We've been mainly focusing on **High Availability**, **High Scalability** and **High Cost Performance Ratio** since unstructured data such as images, movies and logs have been exponentially increasing day by day, and we needed to build a cloud storage that can handle all them.

LeoFS consists of 3 core components - `LeoFS Gateway <leofs-gateway-detail.html>`_, `Storage <leofs-storage-detail.html>`_ and `Manager <leofs-manager-detail.html>`_. The role of each component is clearly defined.


.. image:: _static/images/leofs-architecture.001.jpg
   :width: 780px

`LeoFS Gateway <leofs-gateway-detail.html>`_ handles http-requests and http-responses from clients when using REST-API OR S3-API. Also, it has the built-in object-cache system.

`LeoFS Storage <leofs-storage-detail.html>`_ handles *GET*, *PUT* and *DELETE*, Also it has replicator and recoverer in order to keep running and consistency.

`LeoFS Manager <leofs-manager-detail.html>`_ always monitors Gateway(s) and Storage(s). Manger monitors node-status and RING(logical routing-table) checksum to keep running and consistency.


Also, what we payed attention when we desined LeoFS are the following 3 things:
    * To keep always running and No |SPOF|
    * To keep high-performance, regardless of the kind and amount of data
    * To provide easy administration, we already provide LeoFS CUI and GUI console.

----

.. index::
   pair: FAQ fundamentals; Is there the roadmap of LeoFS?

Is there the roadmap of LeoFS?
------------------------------

We've published LeoFS milestones on both of |GitHub| and `LeoFS website <milestone.html>`_. We may revise the milestones occasionally because there is a possibility to add new features or change priority of implementation. We'll keep them always updated.


.. image:: _static/images/leofs-milestone-toward-v2.0.png
   :width: 780px

.. raw:: html

    <br/>

----

.. index::
   pair: FAQ fundamentals; What language is LeoFS written in?

What language is LeoFS written in?
----------------------------------

LeoFS is implemented in |Erlang|. Also, `LeoCenter <leo_center.html>`_ as Web GUI Console is written in Ruby and JavaScript.

See Also:
    * `LeoFS Overview <intro.html>`_
    * |GitHub|
    * |LeoCenter|

----

.. index::
   pair: FAQ fundamentals; What language can I use to work with LeoFS?

What language can I use to work with LeoFS?
-------------------------------------------

LeoFS clients exist for all of the most popular programming languages as S3-API client. See the latest list of clients for `details <s3_client.html>`_.

See Also:
    * `Amazon S3 Client Tutorials <s3_client.html>`_

----



.. |LeoFS| raw:: html

   <a href="http://leo-project.net/leofs/" target="_blank">LeoFS</a>

.. |object_storage| raw:: html

   <a href="http://en.wikipedia.org/wiki/Object_storage" target="_blank">object storage</a>

.. |ConsistentHashing| raw:: html

   <a href="http://en.wikipedia.org/wiki/Consistent_hashing" target="_blank">Consistent hashing</a>

.. |LeoFS_Slide| raw:: html

   <a href="http://www.slideshare.net/rakutentech/scaling-and-high-performance-storage-system-leofs" target="_blank">Slide - Scaling and High Performance Storage System: LeoFS</a>

.. |SPOF| raw:: html

   <a href="http://en.wikipedia.org/wiki/Single_point_of_failure" target="_blank">SPOF - Single Point Of Failure</a>

.. |GitHub| raw:: html

   <a href="https://github.com/leo-project/leofs" target="_blank">LeoFS on GitHub</a>

.. |LeoCenter| raw:: html

   <a href="https://github.com/leo-project/leo_center" target="_blank">LeoCenter on GitHub</a>

.. |Erlang| raw:: html

   <a href="http://www.erlang.org/" target="_blank">Erlang</a>

