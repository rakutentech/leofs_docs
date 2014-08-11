S3-API Commands
===============

\

+------------------------------------------------------+-------------------------------------------------------------------+
| Command                                              | Explanation                                                       |
+======================================================+===================================================================+
| create-user `${user-id}`                             | * Generate an S3 key pair (AccessKeyID and SecretAccessKey)       |
+------------------------------------------------------+-------------------------------------------------------------------+
| delete-user `${user-id}`                             | * Remove a user                                                   |
+------------------------------------------------------+-------------------------------------------------------------------+
| get-users                                            | * Retrieve all the registered users                               |
+------------------------------------------------------+-------------------------------------------------------------------+
| add-endpoint `${endpoint}`                           | * Register a new S3 Endpoint                                      |
|                                                      | * LeoFS' domains are ruled by :ref:`this rule <s3-path-label>`    |
+------------------------------------------------------+-------------------------------------------------------------------+
| delete-endpoint `${endpoint}`                        | * Delete an S3 Endpoint                                           |
+------------------------------------------------------+-------------------------------------------------------------------+
| get-endpoints                                        | * Retrieve all the registered S3 Endpoints                        |
+------------------------------------------------------+-------------------------------------------------------------------+
| add-bucket `${bucket}` `${access_key_id}`            | * Create a bucket from Manager(s) and Gateway(s)                  |
+------------------------------------------------------+-------------------------------------------------------------------+
| delete-bucket `${bucket}` `${access_key_id}`         | * Remove a bucket from Manager(s), Gateway(s) and Storage-cluster |
+------------------------------------------------------+-------------------------------------------------------------------+
| get-buckets                                          | * Retrieve all of registered buckets                              |
+------------------------------------------------------+-------------------------------------------------------------------+
| get-bucket `${access_key_id}`                        | * Retrieve buckets of a user                                      |
+------------------------------------------------------+-------------------------------------------------------------------+
| chown-bucket `${bucket}` `${access_key_id}`          | * Change owner of a bucket (v0.16.5-)                             |
+------------------------------------------------------+-------------------------------------------------------------------+
| update-acl `${bucket}` `${access_key_id}`            | * Update a ACL for a bucket (v0.16.0-)                            |
| `private | public-read | public-read-write`          |                                                                   |
+------------------------------------------------------+-------------------------------------------------------------------+
| get-acl `${bucket}`                                  | * Retrieve a ACL for a bucket (v0.16.0-)                          |
+------------------------------------------------------+-------------------------------------------------------------------+


.. ### CREATE USER ###

.. _s3-create-user:

.. index::
    create-user-command

**'create-user'** - Create a user and generate an S3 key pair (AccessKeyID and SecretAccessKey)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``create-user ${user-id}``

::

    create-user test_account
    access-key-id: be8111173c8218aaf1c3
    secret-access-key: 929b09f9b794832142c59218f2907cd1c35ac163


.. ### DELETE USER ###

.. _s3-delete-user:

.. index::
    delete-user-command

**'delete-user'** - Remove a user from LeoFS manager's DB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``delete-user ${user-id}``

::

    delete-user test
    ok


.. ### GET USERS ###

.. _s3-get-users:

.. index::
    get-users-command

**'get-users'** - Retrieve users from LeoFS manager's DB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``get-users``

::

    get-users
    user_id     | access_key_id          | created_at
    ------------+------------------------+---------------------------
    _test_leofs | 05236                  | 2012-12-07 10:27:39 +0900
    leo         | 39bbad4f3b837ed209fb   | 2012-12-07 10:27:39 +0900


.. ### SET ENDPOINT ###

.. _s3-add-endpoint:

.. index::
    add-endpoint-command

**'add-endpoint'** - Register a new Endpoint
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: LeoFS domains are ruled by :ref:`this rule <s3-path-label>`

Command: ``add-endpoint ${endpoint}``

::

    add-endpoint test_account
    OK


.. ### DELETE ENDPOINTS ###

.. _s3-delete-endpoint:

.. index::
    delete-endpoint-command

**'delete-endpoint'** - Remove an Endpoint
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``delete-endpoint ${endpoint}``

::

    delete-endpoint test
    OK


.. ### GET ENDPOINTS ###

.. _s3-get-endpoints:

.. index::
    get-endpoints-command

**'get-endpoints'** - Retrieve all the registered Endpoints
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``get-endpoints``

::

    get-endpoints
    endpoint         | created at
    -----------------+---------------------------
    s3.amazonaws.com | 2012-09-12 14:09:52 +0900
    localhost        | 2012-09-12 14:09:52 +0900
    leofs.org        | 2012-09-12 14:09:52 +0900

.. ### ADD BUCKET ###
.. _s3-add-bucket:

.. index::
    add-bucket-command

**'add-bucket'** - Create a bucket from Manager(s) and Gateway(s)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``add-bucket ${bucket} ${access_key_id}``

::

    add-bucket backup 05236
    OK


.. ### DELETE BUCKET ###
.. _s3-delete-bucket:

.. index::
    delete-bucket-command

**'delete-bucket'** - Remove a bucket from Manager(s), Gateway(s) and Storage-cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``delete-bucket ${bucket} ${access_key_id}``

::

    delete-bucket backup 05236
    OK


.. ### GET BUCKETS ###
.. _s3-get-buckets:

.. index::
    get-buckets-command

**'get-buckets'** - Retrieve list of buckets registered
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``get-buckets``

::

    get-buckets
    cluster id   | bucket   | owner       | permissions                            | created at
    -------------+----------+-------------+----------------------------------------+---------------------------
    leofs_1      | backup   | _test_leofs | Me(full_control), Everyone(read)       | 2014-04-03 11:39:01 +0900
    leofs_1      | docs     | _test_leofs | Me(full_control), Everyone(read)       | 2014-04-03 11:39:25 +0900
    leofs_1      | logs     | _test_leofs | Me(full_control), Everyone(read,write) | 2014-04-03 11:39:38 +0900
    leofs_1      | movie    | _test_leofs | Me(full_control)                       | 2014-04-03 11:39:45 +0900

.. ### GET BUCKET ###
.. _s3-get-bucket:

.. index::
    get-bucket-command

**'get-bucket'** - Retrieve buckets of a user
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``get-bucket  ${access_key_id}``

::

    get-bucket 05236
    bucket   | permissions                            | created at
    ---------+----------------------------------------+---------------------------
    backup   | Me(full_control), Everyone(read)       | 2014-04-03 11:39:01 +0900
    docs     | Me(full_control), Everyone(read)       | 2014-04-03 11:39:25 +0900
    logs     | Me(full_control), Everyone(read,write) | 2014-04-03 11:39:38 +0900
    movie    | Me(full_control)                       | 2014-04-03 11:39:45 +0900


.. ### CHANGE BUCKET OWNER ###
.. _s3-chown-bucket:

.. index::
    chown-bucket-command

**'chown-bucket'** - Change owner of a bucket (v0.16.5-)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``chown-bucket ${bucket} ${new_access_key_id}``

::

    chown-bucket backup 47ad5ca9
    OK


.. ### UPDATE ACL ###
.. _s3-update-acl:

.. index::
    update-acl-command

**'update-acl'** - Update a ACL for a bucket (v0.16.0-)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Command: ``update-acl ${bucket} ${access_key_id}``

::

    update-acl photo 05236 private
    ok

    update-acl photo 05236 public-read
    ok

    update-acl photo 05236 public-read-write
    ok



**Canned ACL**
^^^^^^^^^^^^^^

.. note:: When using S3-API, LeoFS supports a set of predefined grants, known as canned ACLs. Each canned ACL has a predefined a set of grantees and permissions. The following table lists the set of canned ACLs and the associated predefined grants.

+------------------+-----------------------+------------------------------------------------------------------------+
| Canned ACL       | Applies to            | Permissions added to ACL                                               |
+==================+=======================+========================================================================+
| private          | Bucket and object     | Owner gets FULL_CONTROL. No one else has access rights (default).      |
+------------------+-----------------------+------------------------------------------------------------------------+
| public-read      | Bucket and object     | Owner gets FULL_CONTROL. The AllUsers group gets READ access.          |
+------------------+-----------------------+------------------------------------------------------------------------+
| public-read-write| Bucket and object     | Owner gets FULL_CONTROL. The AllUsers group gets READ and WRITE access.|
|                  |                       | Granting this on a bucket is generally not recommended.                |
+------------------+-----------------------+------------------------------------------------------------------------+

* Reference:`Access Control List (ACL) Overview <http://docs.aws.amazon.com/AmazonS3/latest/dev/ACLOverview.html>`_
