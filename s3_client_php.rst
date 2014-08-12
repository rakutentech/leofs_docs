.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _aws-sdk-php2-label:

Getting Started with PHP
------------------------

Install AWS-SDK version 2 for PHP
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

php5-curl (Debian)
""""""""""""""""""

::

  sudo apt-get install php5-curl

PEAR (Debian)
"""""""""""""

::

  sudo apt-get install php-pear

aws-sdk for PHP
^^^^^^^^^^^^^^^^

::

  sudo pear channel-discover pear.amazonwebservices.com
  sudo pear install aws/sdk

Edit /etc/hosts
^^^^^^^^^^^^^^^

.. note:: LeoFS domains are ruled by :ref:`this rule <s3-path-label>`.

::

  127.0.0.1 s3.amazonaws.com
  127.0.0.1 ${bucket_name}.s3.amazonaws.com # if you use create_bucket

Example usage
^^^^^^^^^^^^^

.. code-block:: php

  <?php
  require "vendor/autoload.php";

  use Aws\Common\Enum\Region;
  use Aws\S3\S3Client;

  $client = S3Client::factory(array(
    "key" => "YOUR ACCESS KEY ID",
    "secret" => "YOUR SECRET ACCESS KEY",
    "region" => Region::US_EAST_1,
    "scheme" => "http",
  ));

  // list buckets
  $buckets = $client->listBuckets()->toArray();

  foreach($buckets as $bucket){
    print_r($bucket);
  }
  print("\n\n");

  // create bucket
  $result = $client->createBucket(array(
    "Bucket" => "test"
  ));

  // PUT object
  $client->putObject(array(
    "Bucket" => "test",
    "Key" => "key-test",
    "Body" => "Hello, world!"
  ));

  // GET object
  $object = $client->getObject(array(
    "Bucket" => "test",
    "Key" => "key-test"
  ));
  print($object->get("Body"));
  print("\n\n");

  // HEAD object
  $headers = $client->headObject(array(
    "Bucket" => "test",
    "Key" => "key-test"
  ));
  print_r($headers->toArray());

  // DELETE object
  $client->deleteObject(array(
    "Bucket" => "test",
    "Key" => "key-test"
  ));
  ?>

