.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _knox-label:

Getting Started with Node.js
----------------------------

Install "Knox"
^^^^^^^^^^^^^^

::

  npm install knox

Edit /etc/hosts
^^^^^^^^^^^^^^^

.. note:: LeoFS domains are ruled by :ref:`this rule <s3-path-label>`.

::

  127.0.0.1 ${bucket_name}.localhost

Example usage
^^^^^^^^^^^^^

.. code-block:: javascript

  Var knox = require("knox")

  var client = knox.createClient({
    key: "YOUR ACCESS KEY ID",
    secret: "YOUR SECRET ACCESS KEY",
    bucket: "bucket",
    endpoint: "bucket.localhost", // ${bucket_name}.localhost
    port: 8080
  });

  // PUT object
  var string = "Hello, world!";
  client.put("key", {
    "Content-Length": string.length,
    "Content-Type": "application/json"
  }).end(string);

  // HEAD object
  client.headFile("key", function(err, res) {
    console.log("Headers:\n", res.headers);
  });

  // GET object
  client.getFile("key", function(err, res) {
    res.on('data', function(chunk){
      console.log(chunk.toString());
    });
  });

  // DELETE object
  client.deleteFile("key", function(err, res) {
    console.log(res.statusCode);
  });

