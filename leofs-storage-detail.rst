.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: Architecture; LeoFS Storage

LeoFS Storage
=============

Fundamentals
--------------

*Storage* consists of object and the metadata storage. In addition, it includes replicator and repairer in order to realise |eventualy_consistency|.

In case of a write operation, Storage accepts a request from Gateway then automatically replicate an object into the storage cluster. Finally, Storage confirms whether a stored object satisfy the consistency rule or NOT.

On the other hand, in case of a read operation, *Gateway* requests a storage node. Then the storage retrieves an object from the local object-storage or the remote storage-node. Finaly, the storage respond an object to the gateway. Also, the storage checks consistency of the object with the asynchronous processing.

If the storage finds inconsistency of an object, it will be recovered with the backend process. The object eventually keep consistensy with their functions.


.. image:: _static/images/leofs-architecture.003.jpg
   :width: 760px


Data Structure
--------------

LeoFS's object consists of 3 layers which are **metadata**, **needle** and **object container**.

* The object storage manages and stores both an object and a metadata, which merges as a needle.
* The metadata storage manages and stores attributes of an object which includes *filename*, *size*, *checksum*, and so on. And it depends of |bitcask_2| or |leveldb|.
* The object container is a log structured file format.
    * This format is robust and high performance because effect of local file system is just a little part.
    * The storage is necessary to GC - the **compaction** mechanism in order to remove unnecessary objects from the object container.

.. image:: _static/images/leofs-architecture.005.jpg
   :width: 760px


Large object support
--------------------

LeoFS supports to handle a large size object since v0.12. The purpose of this function is 2 things:
    * 1st one is to equalize disk usage of every storage node.
    * 2nd one is to realize high I/O efficiency and high availability.

In case of a write operation, a large size object is divided to plural objects at *Gateway* then they're replicated into *the storage cluster* similarly to a small size object. And also, the default chunk size is *5 mega bytes*, value of which is able to change a custom chunked object size.

On the other hand, In case of READ of a large object, first, *Gateway* retrieves a metadata of a requested object from a client. Then if it is a large size object, *Gateway* retrieves the chunked objects in order of the chunk object number from the storage cluster. Finally, *Gateway* responds the objects to the client.


.. image:: _static/images/leofs-architecture.006.jpg
   :width: 760px

.. |eventualy_consistency| raw:: html

   <a href="http://en.wikipedia.org/wiki/Eventual_consistency" target="_blank">Eventual consistency</a>

.. |bitcask| raw:: html

   <a href="https://github.com/basho/bitcask" target="_blank">bitcask</a>

.. |bitcask_2| raw:: html

   <a href="https://github.com/basho/bitcask" target="_blank">bitcask</a>

.. |leveldb| raw:: html

   <a href="https://github.com/basho/eleveldb" target="_blank">leveldb</a>

