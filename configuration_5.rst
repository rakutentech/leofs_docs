.. LeoFS documentation
.. Copyright (c) 2013-2014 Rakuten, Inc.

.. _leofs-with-nfs-label:

.. index::
   pair: Configuration; LeoFS with NFS

LeoFS with NFS
==============

.. index::
   pair: NFS; Purpose

Purpose
-------
This section is a step by step guide to setting up LeoFS with NFS. By
following this tutorial you can easily build a LeoFS system with NFS.

.. index::
   pair: NFS; Getting Started

Getting Started
---------------

Pre-requirement
~~~~~~~~~~~~~~~

.. note:: We have checked this mechanism with CentOS 6.5 and Ubuntu Server 14.04 LTS but we're goinng to investigate other OS such as FreeBSD and SmartOS.


- Install NFS client on CentOS 6.5

.. code-block:: bash

    $ sudo yum install nfs-utils

- Install NFS client on Ubuntu Server 14.04 LTS

.. code-block:: bash

    $ sudo apt-get install nfs-common

Configuration
~~~~~~~~~~~~~

- Modify |leo_gateway_conf|

 -  Set ``protocol`` to ``nfs``
 -  Set ``large_object.chunked_obj_len`` to ``1048576``

.. note:: If you also want to access LeoFS via S3 intereface, you need to start another LeoFS Gateway with setting ``protocol`` to s3.

::

    ## Gateway protocol to use: [s3 | rest | embed | nfs]
    protocol = nfs

    ## Length of a chunked object
    large_object.chunked_obj_len = 1048576

Start LeoFS as NFS Server with other dependent programs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- `Start LeoFS as usual <http://leo-project.net/leofs/docs/admin_guide_1.html>`_

- Start rpcbind 

.. code-block:: bash

    $ sudo service rpcbind start

- Create a bucket for NFS with ``leofs-adm``

.. code-block:: bash

    $ ./leofs-adm add-bucket test 05236           
    OK
    $ ./leofs-adm get-buckets          
    cluster id   | bucket   | owner       | permissions      | created at                
    -------------+----------+-------------+------------------+---------------------------
    leofs_1      | test     | _test_leofs | Me(full_control) | 2014-07-31 10:20:42 +0900


- Create a mount point and Mount

.. code-block:: bash

    $ sudo mkdir /mnt/leofs   
    $ sudo mount -t nfs -o nolock 127.0.0.1:/test /mnt/leofs

Now you can operate the bucket test in LeoFS as a filesystem via ``/mnt/leofs``.

Confirm that NFS works
~~~~~~~~~~~~~~~~~~~~~~

- Create a file

.. code-block:: bash

    $ touch /mnt/leofs/newfile 
    $ ls -al /mnt/leofs

    drwxrwxrwx. 0 root root 4096 7月 31 10:09 2014 .
    drwxr-xr-x. 6 root root 4096 7月 11 12:38 2014 ..
    -rw-rw-rw-  0 root root    0 7月 31 10:25 2014 newfile

- Modify a file 

.. code-block:: bash
   
    $ echo "hello world" > /mnt/leofs/newfile
    $ cat /mnt/leofs/newfile

    hello world

- Copy a file

.. code-block:: bash

    $ cp /mnt/leofs/newfile /mnt/leofs/newfile.copy 
    $ ls -al /mnt/leofs

    drwxrwxrwx  0 root root 4096 7月 31 10:09 2014 .
    drwxr-xr-x. 6 root root 4096 7月 11 12:38 2014 .. 
    -rw-rw-rw-  0 root root   12 7月 31 10:29 2014 newfile 
    -rw-rw-rw-  0 root root   12 7月 31 10:31 2014 newfile.copy
    
.. code-block:: bash

    $ ./leofs-adm whereis photo/newfile
    -------+--------------------------+--------------------------------------+------------+--------------+----------------+----------------+----------------------------
     del?  |           node           |             ring address             |    size    |   checksum   |  # of chunks   |     clock      |             when
    -------+--------------------------+--------------------------------------+------------+--------------+----------------+----------------+----------------------------
           | storage_0@127.0.0.1      | 22f3d93762d31abc5f5704f78edf1691     |        12B |   6f5902ac23 |              0 | 4ffe2d105f1f4  | 2014-07-31 10:29:01 +0900

    $ ./leofs-adm whereis photo/newfile.copy
    -------+--------------------------+--------------------------------------+------------+--------------+----------------+----------------+----------------------------
     del?  |           node           |             ring address             |    size    |   checksum   |  # of chunks   |     clock      |             when
    -------+--------------------------+--------------------------------------+------------+--------------+----------------+----------------+----------------------------
           | storage_0@127.0.0.1      | d02e1e52d93242d2dcdb98224421a1fb     |        12B |   6f5902ac23 |              0 | 4ffe2d20343a3  | 2014-07-31 10:31:17 +0900


- Diff files

.. code-block:: bash

    $ diff /mnt/leofs/newfile /mnt/leofs/newfile.copy

- Remove a file 

.. code-block:: bash

    $ rm /mnt/leofs/newfile 
    $ ls -al /mnt/leofs

    drwxrwxrwx  0 root root 4096 7月 31 10:09 2014 . 
    drwxr-xr-x. 6 root root 4096 7月 11 12:38 2014 .. 
    -rw-rw-rw-  0 root root   12 7月 31 10:31 2014 newfile.copy

.. code-block:: bash

    $ ./leofs-adm whereis photo/newfile
    -------+--------------------------+--------------------------------------+------------+--------------+----------------+----------------+----------------------------
     del?  |           node           |             ring address             |    size    |   checksum   |  # of chunks   |     clock      |             when
    -------+--------------------------+--------------------------------------+------------+--------------+----------------+----------------+----------------------------
      *    | storage_0@127.0.0.1      | 22f3d93762d31abc5f5704f78edf1691     |         0B |   d41d8cd98f |              0 | 4ffe2e5d9cffe  | 2014-07-31 10:34:50 +0900


- Create a directory 

.. code-block:: bash

    $ mkdir -p /mnt/leofs/1/2/3 
    $ ls -alR /mnt/leofs/1

    /mnt/leofs/1: 
    drwxrwxrwx 0 root root 4096 7月 31 19:37 2014 .
    drwxrwxrwx 0 root root 4096 7月 31 10:09 2014 ..
    drwxrwxrwx 0 root root 4096 7月 31 10:37 2014 2

    /mnt/leofs/1/2: 
    drwxrwxrwx 0 root root 4096 7月 31 19:37 2014 .
    drwxrwxrwx 0 root root 4096 7月 31 19:37 2014 .. 
    drwxrwxrwx 0 root root 4096 7月 31 10:37 2014 3

    /mnt/leofs/1/2/3:
    drwxrwxrwx 0 root root 4096 7月 31 19:37 2014 .
    drwxrwxrwx 0 root root 4096 7月 31 19:37 2014 ..

- Create a very large file 

.. code-block:: bash

    # Create a 50M file 
    $ dd if=/dev/urandom of=/mnt/leofs/1/2/3/largefile bs=1048576 count=50 
    $ ls -alR /mnt/leofs/1

    drwxrwxrwx 0 root root     4096 7月 31 19:42 2014 .
    drwxrwxrwx 0 root root     4096 7月 31 19:42 2014 ..
    -rw-rw-rw- 0 root root 52428800 7月 31 10:42 2014 largefile

.. code-block:: bash

    $ ./leofs-adm whereis photo/1/2/3/largefile
    -------+--------------------------+--------------------------------------+------------+--------------+----------------+----------------+----------------------------
     del?  |           node           |             ring address             |    size    |   checksum   |  # of chunks   |     clock      |             when
    -------+--------------------------+--------------------------------------+------------+--------------+----------------+----------------+----------------------------
           | storage_0@127.0.0.1      | b7992d2fac981fbd98230a124ac78506     |     51200K |   d41d8cd98f |             10 | 4ffe2f44badd2  | 2014-07-31 10:42:53 +0900


- Remove files recursively 

.. code-block:: bash

    $ rm -rf /mnt/leofs/1/
    $ ls -al /mnt/leofs

    drwxrwxrwx  0 root root 4096 7月 31 10:09 2014 .
    drwxr-xr-x. 6 root root 4096 7月 11 12:38 2014 ..
    -rw-rw-rw-  0 root root   12 7月 31 10:31 2014 leofs.copy

And other basic file/directory operations also should work except
controlling owners/permissions/symbolic links/special files.


.. index::
   pair: NFS; Configuration

Configuration
-------------

You can change the port number the NFS/Mount server use and the number
of acceptor processes at ``leo_gateway.conf``.

+------------------------+------------------------------------------------------------------------+
| Property               | Description                                                            |
+========================+========================================================================+
| nfs.port               | Port number the NFS server use                                         |
+------------------------+------------------------------------------------------------------------+
| nfs.num_of_acceptors   | The number of acceptor processes listening for NFS server connection   |
+------------------------+------------------------------------------------------------------------+
| mount.port             | Port number the Mount server use                                       |
+------------------------+------------------------------------------------------------------------+
| mount.num_of_acceptors | The number of acceptor processes listening for Mount server connection |
+------------------------+------------------------------------------------------------------------+

.. index::
   pair: NFS; Limits

Limits
------

Since LeoFS NFS implementation is still the alpha version, there are some limitations. The details are described at `LeoFS
Limits <http://leo-project.net/leofs/docs/faq_2.html#nfs-support>`_



.. |leo_gateway_conf| raw:: html

   <a href="https://github.com/leo-project/leo_gateway/blob/develop/priv/leo_gateway.conf#L46" target="_blank">leo_gateway.conf</a>
