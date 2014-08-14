.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _s3cmd-label:

Connect to LeoFS using s3cmd
----------------------------

Getting `s3cmd <http://sourceforge.net/projects/s3tools/files/>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration
^^^^^^^^^^^^^

.. note:: LeoFS' domains are ruled by :ref:`this rule <s3-path-label>`. You need to set 'Endpoint' and 'Port'.

.. code-block:: bash

  $ s3cmd --configure

  Enter new values or accept defaults in brackets with Enter.
  Refer to user manual for detailed description of all options.

  Access key and Secret key are your identifiers for Amazon S3
  Access Key: ${ACCESS_KEY}
  Secret Key: ${SECRET_ACCESS_KEY}

  Encryption password is used to protect your files from reading
  by unauthorized persons while in transfer to S3
  Encryption password:
  Path to GPG program [/usr/bin/gpg]:

  When using secure HTTPS protocol all communication with Amazon S3
  servers is protected from 3rd party eavesdropping. This method is
  slower than plain HTTP and can't be used if you're behind a proxy
  Use HTTPS protocol [No]:

  On some networks all internet access must go through a HTTP proxy.
  Try setting it here if you can't connect to S3 directly
  HTTP Proxy server name: localhost
  HTTP Proxy server port [3128]: 8080

  New settings:
    Access Key: ${ACCESS_KEY}
    Secret Key: ${SECRET_ACCESS_KEY}
    Encryption password:
    Path to GPG program: /usr/bin/gpg
    Use HTTPS protocol: False
    HTTP Proxy server name: ${ENDPOINT}
    HTTP Proxy server port: ${PORT}

  Test access with supplied credentials? [Y/n]

.. note:: LeoFS's Gateway respond a large size object with "chunked transfer encording - NOT put Content-Length on the HTTP header". Unfortunately, "s3cmd" does not support that, yet.


Commands
^^^^^^^^^^^^

 +----+-----------------------------------------------------------------------------------------------------+----------------+
 |    | Command                                                                                             | Support Status |
 +====+===============================================+=====================================================+================+
 | 1  | Make bucket                                   | s3cmd mb s3://BUCKET                                | **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 2  | Remove bucket                                 | s3cmd rb s3://BUCKET                                | **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 3  | List objects or buckets                       | s3cmd ls [s3://BUCKET[/PREFIX]]                     | **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 4  | List all object in all buckets                | s3cmd la                                            | No             |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 5  | Put file into bucket                          | s3cmd put FILE [FILE...] s3://BUCKET[/PREFIX]       | **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 6  | Get file from bucket                          | s3cmd get s3://BUCKET/OBJECT LOCAL_FILE             | **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 7  | Delete file from bucket                       | s3cmd del s3://BUCKET/OBJECT                        | **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 8  | Synchronize a directory tree to S3            | s3cmd sync LOCAL_DIR s3://BUCKET[/PREFIX]           | **Yes**        |
 |    |                                               |                                                     |                |
 |    |                                               | s3://BUCKET[/PREFIX] LOCAL_DIR                      |                |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 9  | Disk usage by buckets                         | s3cmd du [s3://BUCKET[/PREFIX]]                     | No             |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 10 | Get various info about buckets or files       | s3cmd info s3://BUCKET[/OBJECT]                     | No             |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 11 | Copy object                                   | s3cmd cp s3://BUCKET1/OBJECT1 s3://BUCKET2[/OBJECT2]| **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 12 | Move object                                   | s3cmd mv s3://BUCKET1/OBJECT1 s3://BUCKET2[/OBJECT2]| **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+

