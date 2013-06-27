.. LeoFS documentation master file, created by
   sphinx-quickstart on Tue Feb 21 10:38:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Amazon S3 API and Interface
---------------------------

S3 API Implementation Status
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Objects API
"""""""""""

 +----+----------------------------------+--------------------------------------+
 |    | Method                           | Status                               |
 +====+==================================+======================================+
 | 1  | **DELETE Object**                | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 2  | Delete Multiple Objects          | No                                   |
 +----+----------------------------------+--------------------------------------+
 | 3  | **GET Object**                   | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 4  | GET Object ACL                   | Plans to support with 0.14           |
 +----+----------------------------------+--------------------------------------+
 | 5  | GET Object torrent               | No                                   |
 +----+----------------------------------+--------------------------------------+
 | 6  | **HEAD Object**                  | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 7  | **POST Object**                  | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 8  | **PUT Object**                   | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 9  | PUT Object ACL                   | Plans to support with 0.14           |
 +----+----------------------------------+--------------------------------------+
 | 10 | **PUT Object - Copy**            | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 11 | **Initiate Multipart Upload**    | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 12 | **Upload Part**                  | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 13 | **Upload Part - Copy**           | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 14 | **Complete Multipart Upload**    | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 15 | **Abort Multipart Upload**       | **Yes**                              |
 +----+----------------------------------+--------------------------------------+
 | 16 | List Parts                       | No                                   |
 +----+----------------------------------+--------------------------------------+

Buckets API
"""""""""""

 +----+--------------------------------+--------------------------------------+
 |    | Method                         | Status                               |
 +====+================================+======================================+
 | 1  | **DELETE Bucket**              | **Yes**                              |
 +----+--------------------------------+--------------------------------------+
 | 2  | DELETE Bucket lifecycle        | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 3  | DELETE Bucket policy           | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 4  | DELETE Bucket website          | Plans to support with 0.14           |
 +----+--------------------------------+--------------------------------------+
 | 5  | **GET Bucket (List Objects)**  | **Yes**                              |
 +----+--------------------------------+--------------------------------------+
 | 6  | GET Bucket acl                 | Plans to support with 0.14           |
 +----+--------------------------------+--------------------------------------+
 | 7  | GET Bucket lifecycle           | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 8  | GET Bucket policy              | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 9  | GET Bucket location            | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 10 | GET Bucket logging             | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 11 | GET Bucket notification        | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 12 | GET Bucket Object versions     | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 13 | GET Bucket requestPayment      | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 14 | GET Bucket versioning          | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 15 | GET Bucket website             | Plans to support with 0.14           |
 +----+--------------------------------+--------------------------------------+
 | 16 | **HEAD Bucket**                | **Yes**                              |
 +----+--------------------------------+--------------------------------------+
 | 17 | List Multipart Uploads         | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 18 | **PUT Bucket**                 | **Yes**                              |
 +----+--------------------------------+--------------------------------------+
 | 19 | PUT Bucket acl                 | Plans to support with 0.14           |
 +----+--------------------------------+--------------------------------------+
 | 20 | PUT Bucket lifecycle           | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 21 | PUT Bucket policy              | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 22 | PUT Bucket logging             | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 23 | PUT Bucket notification        | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 24 | PUT Bucket requestPayment      | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 24 | PUT Bucket versioning          | No                                   |
 +----+--------------------------------+--------------------------------------+
 | 25 | PUT Bucket website             | Plans to support with 0.14           |
 +----+--------------------------------+--------------------------------------+


Amazon S3 Interface
^^^^^^^^^^^^^^^^^^^

.. index::
   pair: Official Documents; Amazon S3 Interface

Amazon S3 Official Documentation
""""""""""""""""""""""""""""""""

* `The official documentation <http://docs.amazonwebservices.com/AmazonS3/2006-03-01/dev/Welcome.html?r=7602>`_

.. _s3-path-label:

How to determine the name of Bucket
"""""""""""""""""""""""""""""""""""

.. index::
   pair: How to determine the name of Bucket; Installation

*  `Virtual Hosting of Buckets <http://docs.amazonwebservices.com/AmazonS3/2006-03-01/dev/VirtualHosting.html>`_
*   Values used for determining the name:
    * S3 uses the **HTTP Host header** or a **Path segment in the HTTP request line**.
    * How S3 determines what to use depends on the **Domain name**.
*   Patterns

1. **http://s3.amazonaws.com/bucket/path_to_file**

  In this case, the name of the bucket is the first segment of the path.

  `bucket`

  +--------------+--------------------------------------------------------+
  | Name         | Value                                                  |
  +==============+========================================================+
  | Request line | GET /bucket/path_to_file HTTP/1.1                      |
  +--------------+--------------------------------------------------------+
  | Host header  | Host: s3.amazonaws.com                                 |
  +--------------+--------------------------------------------------------+

  The argument of LeoFS' :ref:`whereis <whereis>` | :ref:`purge <purge>` commands should be `bucket/path_to_file`.

2. **http://www.example.com.s3.amazonaws.com/path_to_file**

  In this case, the name of the bucket is the part of the domain name before `.s3.amazonaws.com`.

  `www.example.com`

  +--------------+--------------------------------------------------------+
  | Name         | Value                                                  |
  +==============+========================================================+
  | Request line | GET /path_to_file HTTP/1.1                             |
  +--------------+--------------------------------------------------------+
  | Host header  | Host: www.example.com.s3.amazonaws.com                 |
  +--------------+--------------------------------------------------------+

  The argument of LeoFS' :ref:`whereis <whereis>` | :ref:`purge <purge>` commands should be `www.example.com/path_to_file`.

3. **http://www.example.com/path_to_file**

  In this case, the name of bucket is equal to the FQDN.

    `www.example.com`

  +--------------+--------------------------------------------------------+
  | Name         | Value                                                  |
  +==============+========================================================+
  | Request line | GET /path_to_file HTTP/1.1                             |
  +--------------+--------------------------------------------------------+
  | Host header  | Host: www.example.com                                  |
  +--------------+--------------------------------------------------------+

  The argument of LeoFS' :ref:`whereis <whereis>` | :ref:`purge <purge>` commands should be `www.example.com/path_to_file`.

