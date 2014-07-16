.. LeoFS documentation
.. Copyright (c) 2013-2014 Rakuten, Inc.

Architecture
============

We're really focused on is **High Availability**, **High Scalability** and **High Cost Performance Ratio** because unstructured data such as image, movie, event-data, log and so on, they have been exponentially increasing in our services day by day, so we needed to build the cloud storage with all them.

I really succeeded in designing and implementing LeoFS as simple as possible. As you can see on this slide, LeoFS consists of 3 core functions - Gateway, Storage and Manager. The role of each function is clearly defined. I'm going to introduce them later.

Also, what I carefully desined LeoFS is 3 things:

* To keep running and No SPOF
* To keep high-performance, regardless of the kind of data and amout data
* To provide easy administration, we already provide LeoFS CUI and `GUI console <leo_center.html>`_.


.. toctree::
   :hidden:

   leofs-gateway-detail
   leofs-storage-detail
   leofs-manager-detail

