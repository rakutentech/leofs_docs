.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   FAQ LeoFS limits

=======================
FAQ: LeoFS Limits
=======================

.. index::
   pair: FAQ LeoFS limits; Features

Features
--------

* LeoFS have covered almost major |AmazonS3API| but not all.
* If you use |MultiPartUploadAPI|, the size of a part of an object must be less than the size of a chunked object in LeoFS.

See Also:
    * `Amazon S3 API and Interface <s3_api.html>`_
    * `Configuration of LeoFS Gateway nodes <configuration_3.html>`_
    * |ISSUE_177|
    * |ISSUE_190|


----

.. index::
   pair: FAQ LeoFS limits; Operations

Operations
----------

* When you upgrade your LeoFS, you can NOT change the metadata storage as |KVS| - ``bitcask`` or ``leveldb`` can be used in LeoFS - used by LeoFS Storage. In the future, we will provide the data converting tool which enables to take over the metadata.
* Compaction is needed to execute manually for now. We're planning to provide the feature Auto Compaction with LeoFS v1.2.

See Also:
    * `Configuration of Storage nodes <configuration_2.html>`_
    * `Upgrade your old version LeoFS to v1.1.0 <admin_guide_10.html>`_
    * `The roadmap of LeoFS <faq_1.html#is-there-the-roadmap-of-leofs>`_

----

.. index::
   pair: FAQ LeoFS limits; NFS support

NFS Support
-----------

* NFS implemantation with LeoFS v1.1 is a subset of |NFSv3|. Lock manager protocol, ``Authentication``, and ``Owner/Permission`` management are NOT covered.
* The `ls` command may take too much time when the target directory have lots of child. We're planning to provide better performance with LeoFS v.1.2.
* If you use LeoFS with NFS, you should set the size of a chunked object in LeoFS to 1MB (1048576Bytes), otherwise the efficiency of disk utilization can be decreased.

See Also:
    * `Configuration of LeoFS Gateway nodes <configuration_3.html>`_
    * |ISSUE_198|


----


.. |AmazonS3API| raw:: html

   <a href="http://docs.aws.amazon.com/AmazonS3/latest/API/APIRest.html" target="_blank">Amazon S3 REST API</a>

.. |MultiPartUploadAPI| raw:: html

   <a href="http://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html" target="_blank">Amazon S3 multipart upload API</a>

.. |KVS| raw:: html

   <a href="http://en.wikipedia.org/wiki/Key/value_store#Key.E2.80.93Value_or_KV_stores" target="_blank">KVS</a>

.. |NFSv3| raw:: html

   <a href="http://www.ietf.org/rfc/rfc1813.txt" target="_blank">NFS v3</a>

.. |ISSUE_198| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/198" target="_blank">NFS R/W transfer block size is limited up to 1MB</a>

.. |ISSUE_177| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/177" target="_blank">Respond an incorrect MD5 of an large object</a>

.. |ISSUE_190| raw:: html

   <a href="https://github.com/leo-project/leofs/issues/190" target="_blank">Multipart uploads of large files produces partially corrupted data when upload chunk size</a>


