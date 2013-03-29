.. LeoFS documentation master file

REST API
========

.. index::
   pair: REST API; Interface

Configuration
-------------

* You need to update ``${USE_S3_AUTH}``, which value is ``false`` in :ref:`Gateway configuration <conf_gateway_label>`

Interface
---------

Relationship betwwen HTTP-verb and LeoFS's Behaivior
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

\

+----------------+--------------------------------------------------------+
| HTTP Verb      | LeoFS's Behaiviour                                     |
+================+========================================================+
| PUT/POST       | Insert an object into the storage-cluster              |
+----------------+--------------------------------------------------------+
| GET            | Retrieve an object from the storage-cluster            |
+----------------+--------------------------------------------------------+
| DELETE         | Remove an object from the storage-cluster              |
+----------------+--------------------------------------------------------+

URL format
^^^^^^^^^^

* URL format: http://${HOST}:8080/**${KEY}**
    * LeoFS uses NOT URL but ``${KEY}`` into the system
    * You can check that an object is assigned storage-node as the follows:

::

    $ telnet 127.0.0.1 10010
    whereis ${KEY}


.. index::
   pair: REST API; Examples

Examples
--------

POST/PUT

::

    $ curl -X POST -H "Content-Type:image/png" \
              --data-binary @test.jpg http://hostname:8080/_test/_image/file.png

    $ curl -X PUT -H "Content-Type:image/png" \
              --data-binary @test.jpg http://hostname:8080/_test/_image/file.png

GET

::

    $ curl -X GET http://hostname:8080/_test/_image/file.png
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 6019k    0 6019k    0     0   210M      0 --:--:-- --:--:-- --:--:--  217M

DELETE

::

    $ curl -X DELETE http://hostname:8080/_test/_image/file.png

