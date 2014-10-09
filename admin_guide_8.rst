.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
        S3-API commands

S3-API Commands
===============

+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| **Shell**                                                                            | **Description**                                                                                      |
+======================================================================================+======================================================================================================+
| **S3-API Commands - User**                                                                                                                                                                  |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`create-user <create-user>` <user-id> <password>                      | * Register the new user                                                                              |
|                                                                                      | * Generate an S3 key pair (AccessKeyID and SecretAccessKey)                                          |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`delete-user <delete-user>` <user-id>                                 | * Remove the user                                                                                    |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`get-users <get-users>`                                               | * Retrieve the list of users                                                                         |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`update-user-role <update-user-role>`                                 | * Update the user's role                                                                             |
|                                                                                      | * Currently, we are supporting two kinds of roles                                                    |
|                                                                                      | * 1: General user, 9: Administrator                                                                  |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| **S3-API Commands - Endpoint**                                                                                                                                                              |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`add-endpoint <add-endpoint>` <endpoint>                              | * Register a new S3 Endpoint                                                                         |
|                                                                                      | * LeoFS' domains are ruled by :ref:`this rule <s3-path-label>`                                       |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`delete-endpoint <delete-endpoint>` <endpoint>                        | * Remove the endpoint                                                                                |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`get-endpoints <get-endpoints>`                                       | * Retrieve the list of endpoints                                                                     |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| **S3-API Commands - Bucket**                                                                                                                                                                |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`add-bucket <add-bucket>` <bucket> <access-key-id>                    | * Create the new bucket                                                                              |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`delete-bucket <delete-bucket>` <bucket> <access-key-id>              | * Remove the bucket and all files stored in the bucket                                               |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`get-buckets <get-buckets>`                                           | * Retrieve the list of the buckets registered                                                        |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`get-bucket <get-bucket>` <access-key-id>                             | * Retrieve the list of the buckets owned by the specified user                                       |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`chown-bucket <chown-bucket>` <bucket> <access-key-id>                | * ``v0.16.5-`` Change the owner of the bucket                                                        |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| leofs-adm :ref:`update-acl <update-acl>` <bucket> <access-key-id>                    | * ``v0.16.0-`` Update the ACL (Access Control List) for the bucket                                   |
| (private | public-read | public-read-write)                                          | * Available ACL list:                                                                                |
|                                                                                      |      * ``private (default)`` : No one except the owner has access rights                             |
|                                                                                      |      * ``public-read``       : All users have READ access                                            |
|                                                                                      |      * ``public-read-write`` : All users have READ and WRITE access                                  |
+--------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+


S3-API Commands - User
----------------------

.. ### CREATE USER ###
.. _create-user:

.. index::
        pair: S3-API commands; create-user-command

create-user <user-id> <password>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Register the new user
* Generate an S3 key pair (AccessKeyID and SecretAccessKey)

.. code-block:: bash

    $ leofs-adm create-user test_account password
    access-key-id: be8111173c8218aaf1c3
    secret-access-key: 929b09f9b794832142c59218f2907cd1c35ac163

\

.. ### DELETE USER ###
.. _delete-user:

.. index::
        pair: S3-API commands; delete-user-command

delete-user <user-id>
^^^^^^^^^^^^^^^^^^^^^

Remove the user

.. code-block:: bash

    $ leofs-adm delete-user test
    ok

\

.. ### GET USERS ###
.. _get-users:

.. index::
       pair: S3-API commands; get-users-command

get-users
^^^^^^^^^

Retrieve the list of users

.. code-block:: bash

    $ leofs-adm get-users
    user_id     | access_key_id          | created_at
    ------------+------------------------+---------------------------
    _test_leofs | 05236                  | 2012-12-07 10:27:39 +0900
    leo         | 39bbad4f3b837ed209fb   | 2012-12-07 10:27:39 +0900

\

.. ### UPDATE USER ROLE ###
.. _update-user-role:

.. index::
       pair: S3-API commands; update-user-role-command

update-user-role <user-id> <role-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Update the user's role
* Currently, we are supporting two kinds of roles
* role-id:
    * 1: General user
    * 9: Administrator

.. code-block:: bash

    $ leofs-adm update-user-role 05236 1
    OK

\


S3-API Commands - Endpoint
--------------------------

.. ### ADD ENDPOINT ###
.. _add-endpoint:

.. index::
       pair: S3-API commands; add-endpoint-command

add-endpoint <endpoint>
^^^^^^^^^^^^^^^^^^^^^^^

 - Register a new Endpoint

.. note:: LeoFS domains are ruled by :ref:`this rule <s3-path-label>`


.. code-block:: bash

    $ leofs-adm add-endpoint leo-project.net
    OK

\

.. ### DELETE ENDPOINTS ###
.. _delete-endpoint:

.. index::
       pair: S3-API commands; delete-endpoint-command

delete-endpoint <endpoint>
^^^^^^^^^^^^^^^^^^^^^^^^^^

Remove the endpoint

.. code-block:: bash

    $ leofs-adm delete-endpoint leo-project.net
    OK

\

.. ### GET ENDPOINTS ###
.. _get-endpoints:

.. index::
       pair: S3-API commands; delete-endpoint-command

get-endpoints
^^^^^^^^^^^^^

Retrieve the list of endpoints

.. code-block:: bash

    $ leofs-adm get-endpoints
    endpoint         | created at
    -----------------+---------------------------
    s3.amazonaws.com | 2012-09-12 14:09:52 +0900
    localhost        | 2012-09-12 14:09:52 +0900
    leofs.org        | 2012-09-12 14:09:52 +0900

\


S3-API Commands - Bucket
------------------------

.. ### ADD BUCKET ###
.. _add-bucket:

.. index::
       pair: S3-API commands; add-bucket-command


add-bucket <bcuket> <access-key-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 Create the bucket

.. code-block:: bash

    $ leofs-adm add-bucket backup 05236
    OK

\

.. ### DELETE BUCKET ###
.. _delete-bucket:

.. index::
       pair: S3-API commands; delete-bucket-command

delete-bucket <bucket> <access-key-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Remove the bucket and all files stored in the bucket

.. code-block:: bash

    $ leofs-adm delete-bucket backup 05236
    OK

\

.. ### GET BUCKETS ###
.. _get-buckets:

.. index::
       pair: S3-API commands; get-buckets-command

get-buckets
^^^^^^^^^^^

Retrieve the list of the buckets registered

.. code-block:: bash

    $ leofs-adm get-buckets
    cluster id   | bucket   | owner       | permissions                            | created at
    -------------+----------+-------------+----------------------------------------+---------------------------
    leofs_1      | backup   | _test_leofs | Me(full_control), Everyone(read)       | 2014-04-03 11:39:01 +0900
    leofs_1      | docs     | _test_leofs | Me(full_control), Everyone(read)       | 2014-04-03 11:39:25 +0900
    leofs_1      | logs     | _test_leofs | Me(full_control), Everyone(read,write) | 2014-04-03 11:39:38 +0900
    leofs_1      | movie    | _test_leofs | Me(full_control)                       | 2014-04-03 11:39:45 +0900

\

.. ### GET BUCKET ###
.. _get-bucket:

.. index::
       pair: S3-API commands; get-bucket-command

get-bucket <access-key-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^

Retrieve the list of the buckets owned by the specified user

.. code-block:: bash

    $ leofs-adm get-bucket 05236
    bucket   | permissions                            | created at
    ---------+----------------------------------------+---------------------------
    backup   | Me(full_control), Everyone(read)       | 2014-04-03 11:39:01 +0900
    docs     | Me(full_control), Everyone(read)       | 2014-04-03 11:39:25 +0900
    logs     | Me(full_control), Everyone(read,write) | 2014-04-03 11:39:38 +0900
    movie    | Me(full_control)                       | 2014-04-03 11:39:45 +0900

\

.. ### CHANGE BUCKET OWNER ###
.. _chown-bucket:

.. index::
       pair: S3-API commands; chown-bucket-command

chown-bucket <bucket> <access-key-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``v0.16.5-`` Change the owner of the bucket

.. code-block:: bash

    $ leofs-adm chown-bucket backup 47ad5ca9
    OK

\

.. ### UPDATE ACL ###
.. _update-acl:

.. index::
        pair: S3-API commands; update-acl-command

update-acl <bucket> <access-key-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* ``v0.16.0-`` Update the ACL (Access Control List) for the bucket
* Available ACL list:
    * ``private (default)`` : No one except the owner has access rights
    * ``public-read``       : All users have READ access
    * ``public-read-write`` : All users have READ and WRITE access

.. code-block:: bash

    $ leofs-adm update-acl photo 05236 private
    ok

    $ leofs-adm update-acl photo 05236 public-read
    ok

    $ leofs-adm update-acl photo 05236 public-read-write
    ok

\


Canned ACL
^^^^^^^^^^^

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
