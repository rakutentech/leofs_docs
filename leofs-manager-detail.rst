.. LeoFS documentation
.. Copyright (c) 2013-2014 Rakuten, Inc.

LeoFS Manager Detail
====================

*Manager* generates and manages a routing table, which is called **RING** and based on `consistent hashing <http://en.wikipedia.org/wiki/Consistent_hashing>`_.

*Manager* always monitors every `Storage <leofs-storage-detail.html>`_ and `Gateway <leofs-gateway-detail.html>`_ of status and RING in order to keep running LeoFS and consistency of a RING. And also, it distributes RING to `Storage <leofs-storage-detail.html>`_ and `Gateway <leofs-gateway-detail.html>`_.

In addition, *Manager* provides `LeoFS administration commands <admin_guide.html>`_ to be able to easily operate LeoFS.
*LeoFS administration commands* already cover entire LeoFS functions.

