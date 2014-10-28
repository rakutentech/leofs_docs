.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: Architecture; LeoFS Manager

LeoFS Manager
=============

*LeoFS Manager* generates and manages a routing table, which is called **RING** and based on `consistent hashing <http://en.wikipedia.org/wiki/Consistent_hashing>`_.

*LeoFS Manager* always monitors every `LeoFS Storage <leofs-storage-detail.html>`_ and `LeoFS Gateway <leofs-gateway-detail.html>`_ of status and RING in order to keep running LeoFS and consistency of a RING. And also, it distributes RING to `LeoFS Storage <leofs-storage-detail.html>`_ and `LeoFS Gateway <leofs-gateway-detail.html>`_.

.. image:: ../../_static/images/leofs-architecture.007.jpg
   :width: 760px

In addition, *LeoFS Manager* provides `LeoFS administration commands <admin_guide.html>`_ to be able to easily operate LeoFS.
*LeoFS administration commands* already cover entire LeoFS functions.

