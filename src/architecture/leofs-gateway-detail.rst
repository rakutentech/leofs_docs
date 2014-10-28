.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: Architecture; LeoFS Gateway

LeoFS Gateway
=============

*LeoFS Gateway* consists of the fast HTTP-Server - |Cowboy|, the API handler and the cache mecanism. It provides `the REST-API <rest_api.html>`_ and |S3API|. You're able to easily access LeoFS with S3-Clients such as |s3cmd|, |DragonDisk|, program languages - |Erlang|, |Java|, |Ruby|, |Python|, |Go| and so on.

.. image:: ../../_static/images/leofs-architecture.002.jpg
   :width: 760px

A client requests an object or a bucket operation to *LeoFS Gateway* then *LeoFS Gateway* requests the message of operation to a storage-node.

A destination storage node is decided by the routing-table. It is called **RING** which is generated and provided at `LeoFS Manager <leofs-manager-detail.html>`_ and which is based on consistent-hashing.

Also, LeoFS Gateway provides built-in support for the object-cache mechanism in order to realize Keeping high performance and reduction of traffic between *LeoFS Gateway* and `LeoFS Storage <leofs-storage-detail.html>`_.



.. |Cowboy| raw:: html

   <a href="https://github.com/extend/cowboy" target="_blank">Cowboy</a>

.. |S3API| raw:: html

   <a href="http://docs.aws.amazon.com/AmazonS3/latest/API/APIRest.html" target="_blank">Amazon S3-API</a>

.. |s3cmd| raw:: html

   <a href="http://s3tools.org/s3cmd" target="_blank">s3cmd</a>

.. |DragonDisk| raw:: html

   <a href="http://www.dragondisk.com/" target="_blank">DragonDisk</a>

.. |Erlang| raw:: html

   <a href="https://github.com/gleber/erlcloud" target="_blank">Erlang</a>

.. |Java| raw:: html

   <a href="https://github.com/aws/aws-sdk-java" target="_blank">Java</a>

.. |Ruby| raw:: html

   <a href="https://github.com/aws/aws-sdk-ruby" target="_blank">Ruby</a>

.. |Python| raw:: html

   <a href="https://github.com/boto/boto" target="_blank">Python</a>


.. |Go| raw:: html

   <a href="https://github.com/rlmcpherson/s3gof3r" target="_blank">Go</a>
