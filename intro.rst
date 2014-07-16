.. LeoFS documentation
.. Copyright (c) 2013-2014 Rakuten, Inc.

Introduction
============

**LeoFS** is a highly scalable, fault-tolerant unstructured object storage system for the Web.

LeoFS provides **High Cost Performance Ratio**. It allows you to build LeoFS clusters using commodity hardware on top of the Linux operating system. LeoFS will provide very good performance even on commodity hardware. LeoFS will require a smaller cluster than other storage to achieve the same performance. LeoFS is also very easy to setup and to operate.

LeoFS provides **High Reliability** thanks to its great design on top of the Erlang/OTP capabilities. Erlang/OTP is known for being used in production systems for years with a solid nine nines (99.9999999%) availability, and LeoFS is no exception. A LeoFS system will stay up regardless of software errors or hardware failures happening inside the cluster.

LeoFS provides **High Scalability**. Adding and removing nodes is simple and quick, allowing you to react swiftly when your needs change. A LeoFS cluster can be thought as elastic storage that you can stretch as much and as often as you need.

.. image:: _static/images/leofs-architecture.001.jpg
   :width: 760px


LeoFS consists of 3 applications - `Storage <leofs-storage-detail.html>`_, `Gateway <leofs-gateway-detail.html>`_ and `Manager <leofs-manager-detail.html>`_ which depend on Erlang.

`Gateway <leofs-gateway-detail.html>`_ handles http-request and http-response from any clients when using REST-API OR S3-API. Also, it is already built in the object-cache mechanism *(memory and disk cache)*.

`Storage <leofs-storage-detail.html>`_ handles GET, PUT and DELETE objects as well as metadata. Also, it has *replicator*, *recoverer* and *queueing mechanism* in order to keep running a storage node and realise eventualy consistency.

`Manager <leofs-manager-detail.html>`_ always monitors *Gateway* and *Storage* nodes. The main monitoring status are *Node status* and *RING's checksum* in order to realise to keep high availability and keep data consistency.


.. toctree::
   :hidden:

   goal
   milestone
