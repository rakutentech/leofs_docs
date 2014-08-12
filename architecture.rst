.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

Architecture
============

We're really focused on is **High Availability**, **High Scalability** and **High Cost Performance Ratio** because unstructured data such as *image/photo*, *short movie*, *event-data*, *log* and so on, they have been exponentially increasing day by day, so we needed to build a cloud storage with all them.

We really succeeded in designing and implementing LeoFS as simple as possible. LeoFS consists of 3 core components - `LeoFS Gateway <leofs-gateway-detail.html>`_, `LeoFS Storage <leofs-storage-detail.html>`_ and `LeoFS Manager <leofs-manager-detail.html>`_. The role of each component is clearly defined.

.. image:: _static/images/leofs-architecture.001.jpg
   :width: 760px

Also, what we carefully desined LeoFS is 3 things:

* To keep running and No SPOF
* To keep high-performance, regardless of the kind of data and amout data
* To provide easy administration, we already provide LeoFS CUI and `GUI console <leo_center.html>`_.


.. toctree::
   :hidden:

   leofs-gateway-detail
   leofs-storage-detail
   leofs-manager-detail

