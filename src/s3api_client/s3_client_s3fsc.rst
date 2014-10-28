.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _s3fs-c-label:

Getting Started with S3FS-C
---------------------------

S3FS-C is a FUSE (File System in User Space) based file system backed by Amazon S3 storage buckets. Once mounted, S3 can be used just like it was a local file system.

Install libs for S3FS-C into Ubuntu-12.04
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ sudo apt-get install libfuse-dev libcurl4-openssl-dev fuse-utils

Install "S3FS-C"
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ git clone https://github.com/leo-project/s3fs-c.git
    $ cd s3fs-c
    $ ./configure
    $ make
    $ sudo make install

Modify "/etc/hosts"
^^^^^^^^^^^^^^^^^^^^^^^^^

* Add a LeoFS domain in ``/etc/hosts``
* LeoFS' domains are ruled by :ref:`this rule <s3-path-label>`

.. code-block:: bash

    $ sudo vi /etc/hosts

    ## Add a LeoFS domain ##
    127.0.0.1 localhost <bucket>.localhost

Create a credential file for S3FS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ vi ~/.passwd-s3fs

    ## Set access-key and secret-key ##
    <access-key-id>:<secret-key>

    $ chmod 600 ~/.passwd-s3fs


Mount "LeoFS"
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ s3fs <bucket> <mount-point> -o url='http://<endpoint>:<port>'

