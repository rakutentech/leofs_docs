.. LeoFS documentation
.. Copyright (c) 2013-2014 Rakuten, Inc.

LeoFS Storage
=============

*Storage* consists of object and the metadata storage. In addition, it includes replicator and repairer in order to realise |eventualy_consistency|.

    * The object storage* manages and stores both an object and a metadata, which merges as a needle.
    * The metadata storage* manages and stores attributes of an object which includes filename, size, checksum, and so on. And it depends of |bitcask| or |leveldb|.


LeoFS supports to handle a large size object since v0.12. The purpose of this function is 2 things:
    * 1st one is to equalize disk usage of every storage node.
    * 2nd one is to realize high I/O efficiency and high availability.


.. |eventualy_consistency| raw:: html

   <a href="http://en.wikipedia.org/wiki/Eventual_consistency" target="_blank">Eventual consistency</a>

.. |bitcask| raw:: html

   <a href="https://github.com/basho/bitcask" target="_blank">bitcask</a>

.. |bitcask| raw:: html

   <a href="https://github.com/basho/bitcask" target="_blank">bitcask</a>

.. |leveldb| raw:: html

   <a href="https://github.com/basho/eleveldb" target="_blank">leveldb</a>

