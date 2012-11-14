Amazon S3 Client Tutorials
================================

Getting Your S3 Key
---------------------

You can get S3-API's Keys from ``LeoFS's Manager Console``.

::

  $ telnet 127.0.0.1 10010
  Trying 127.0.0.1...
  Connected to 127.0.0.1.
  Escape character is '^]'.

's3-gen-key ${USER-ID}' command generates your s3 key.

::

  s3-gen-key ${USER-ID}
  access-key-id: 05dcba94333c7590a635
  secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a


.. _aws-sdk-ruby-label:

Getting Started with Ruby: 'aws-sdk'
------------------------------------------------------

A part of the Ruby's library, ``aws-sdk``, is available against the LeoFS as a client. It's made by Amazon. The official document of ``aws-sdk`` is here: http://aws.amazon.com/sdkforruby/.

Install AWS-SDK for Ruby
^^^^^^^^^^^^^^^^^^^^^^^^^

::

  $ gem install aws-sdk

Sample Code
^^^^^^^^^^^^^^^^^^^^^^

Connect to LeoFS
"""""""""""""""""

.. note:: LeoFS's domains are governed by :ref:`this rule <s3-path-label>`. You need to rewrite 'Endpoint' and 'Port' as follows:


.. code-block:: ruby

  require "aws-sdk"

  Endpoint = "localhost"
  Port = 8080

  class LeoFSHandler < AWS::Core::Http::NetHttpHandler
    # magic to reconfigure port
    def handle(request, response)
      request.port = ::Port
      super
    end
  end

  AWS.config(
    access_key_id: "YOUR_ACCESS_KEY_ID", # set your s3 key
    secret_access_key: "YOUR_SECRET_ACCESS_KEY",
    s3_endpoint: Endpoint,
    http_handler: LeoFSHandler.new,
    s3_force_path_style: true,
    use_ssl: false
  )

  s3 = AWS::S3.new


PUT an object into the LeoFS
"""""""""""""""""""""""""""""

.. code-block:: ruby

  # create bucket
  s3.buckets.create("photo")

  # get bucket
  bucket = s3.buckets["photo"]

  # create a new object
  object = bucket.objects.create("image", "value")

  # show objects in the bucket
  bucket.objects.with_prefix("").each do |obj|
    p obj
  end

  # retrieve an object
  object = bucket.objects["image"]

  # insert an object
  object.write(
    file: "/path/to/image.png",
    content_type: "png/image"
  )


GET an object from the LeoFS
"""""""""""""""""""""""""""""

.. code-block:: ruby

  image = object.read


DELETE an object from the LeoFS
""""""""""""""""""""""""""""""""

.. code-block:: ruby

  object.delete


HEAD an object from the LeoFS
""""""""""""""""""""""""""""""""

.. code-block:: ruby

  metadata = object.head
  p metadata.to_hash


.. _aws-s3-ruby-label:

Getting Started with Ruby: 'aws-s3'
-------------------------------------

A part of the Ruby's library, ``aws-s3``, is available against the LeoFS as a client. The official document of ``aws-s3`` is here: http://amazon.rubyforge.org/.

Install AWS-S3
^^^^^^^^^^^^^^^^^^^^^^

..

   $ gem install aws-s3


Sample Code
^^^^^^^^^^^^^^^^^^^^^^

The following sample script should upload a text file (located in ``/path/to/localfile``) to LeoFS server at ``localhost:8080/somewhere/path/to/remotefile``.

.. code-block:: ruby

   require 'aws/s3'

   AWS::S3::Base.establish_connection!(
     :access_key_id     => "dummy",
     :secret_access_key => "dummy",
     :server            => "localhost",
     :port              => 8080)

   file = '/path/to/localfile'

   AWS::S3::S3Object.store('/path/to/remotefile',
                  open(file).read,
                  'somewhere',
                  :content_type => 'text/plain')

You can confirm the uploaded file by using ``curl`` command with ``'Host: s3.amazonaws.com'`` header.

.. code-block:: bash

   $ curl --header "Host: s3.amazonaws.com" http://localhost:8080/somewhere/path/to/remotefile

.. note:: ``aws-s3`` client force to use ``'s3.amazonaws.com'`` for the value of Host header. This matches the pattern #1 of :ref:`s3-path-label`

You can also fetch the uploaded file by ``S3Object.value(path, bucket)`` method as follows.

.. code-block:: ruby

   S3Object.value('/path/to/remotefile', 'somewhere')

As for deleting an object, you can use ``S3Object.delete(key, bucket)``

.. code-block:: ruby

   S3Object.value('/path/to/remotefile', 'somewhere')

.. note:: ``S3Object.find(path, backet)`` does not work because the current LeoFS does not support Bucket API on which the ``find`` method depends.




.. _aws-sdk-java-label:

Getting Started with Java: 'aws-sdk'
------------------------------------------------------

Getting AWS SDK for Java
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

"SDK for Java" is here: http://aws.amazon.com/sdkforjava/

.. note:: LeoFS's domains are governed by :ref:`this rule <s3-path-label>`. You need to set 'Proxy Host' and 'Proxy Port' with ClientConfiguration class.


Sample Code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: java

  import java.io.BufferedReader;
  import java.io.File;
  import java.io.FileOutputStream;
  import java.io.IOException;
  import java.io.InputStream;
  import java.io.InputStreamReader;
  import java.io.OutputStreamWriter;
  import java.io.Writer;
  import java.util.UUID;

  import com.amazonaws.AmazonClientException;
  import com.amazonaws.AmazonServiceException;
  import com.amazonaws.auth.AWSCredentials;
  import com.amazonaws.auth.BasicAWSCredentials;
  import com.amazonaws.services.s3.AmazonS3;
  import com.amazonaws.services.s3.AmazonS3Client;
  import com.amazonaws.services.s3.model.GetObjectRequest;
  import com.amazonaws.services.s3.model.ListObjectsRequest;
  import com.amazonaws.services.s3.model.PutObjectRequest;
  import com.amazonaws.services.s3.model.Bucket;
  import com.amazonaws.services.s3.model.S3Object;
  import com.amazonaws.services.s3.model.ObjectListing;
  import com.amazonaws.services.s3.model.S3ObjectSummary;
  import com.amazonaws.ClientConfiguration;
  import com.amazonaws.Protocol;

  public class LeoFSSample {
      public static void main(String[] args) throws IOException {
          /* ---------------------------------------------------------
           * You need to set 'Proxy host', 'Proxy port' and 'Protocol'
           * --------------------------------------------------------- */
          ClientConfiguration config = new ClientConfiguration();
          config.setProxyHost("localhost"); // LeoFS Gateway's Host
          config.setProxyPort(8080);        // LeoFS Gateway's Port
          config.withProtocol(Protocol.HTTP);

          final String accessKeyId = "YOUR_ACCESS_KEY_ID";
          final String secretAccessKey = "YOUR_SECRET_ACCESS_KEY";

          AWSCredentials credentials = new BasicAWSCredentials(accessKeyId, secretAccessKey);
          AmazonS3 s3 = new AmazonS3Client(credentials, config);

          final String bucketName = "test-bucket-" + UUID.randomUUID();
          final String key = "test-key";

          try {
              // Create a bucket
              s3.createBucket(bucketName);

              // Retrieve list of buckets
              for (Bucket bucket : s3.listBuckets()) {
                  System.out.println("Bucket:" + bucket.getName());
              }

              // PUT an object into the LeoFS
              s3.putObject(new PutObjectRequest(bucketName, key, createFile()));

              // GET an object from the LeoFS
              S3Object object = s3.getObject(new GetObjectRequest(bucketName, key));
              dumpInputStream(object.getObjectContent());

              // Retrieve list of objects from the LeoFS
              ObjectListing objectListing =
                  s3.listObjects(new ListObjectsRequest().withBucketName(bucketName));

              for (S3ObjectSummary objectSummary : objectListing.getObjectSummaries()) {
                  System.out.println(objectSummary.getKey() +
                                     "Size:" + objectSummary.getSize());
              }

              // DELETE an object from the LeoFS
              s3.deleteObject(bucketName, key);

              // DELETE a bucket from the LeoFS
              s3.deleteBucket(bucketName);

          } catch (AmazonServiceException ase) {
              System.out.println(ase.getMessage());
              System.out.println(ase.getStatusCode());
          } catch (AmazonClientException ace) {
              System.out.println(ace.getMessage());
          }
      }

      private static File createFile() throws IOException {
          File file = File.createTempFile("leofs_test", ".txt");
          file.deleteOnExit();

          Writer writer = new OutputStreamWriter(new FileOutputStream(file));
          writer.write("Hello, world!\n");
          writer.close();

          return file;
      }

      private static void dumpInputStream(InputStream input) throws IOException {
          BufferedReader reader = new BufferedReader(new InputStreamReader(input));
          while (true) {
              String line = reader.readLine();
              if (line == null) break;
              System.out.println(line);
          }
      }
  }

.. _aws-sdk-php-label:

Getting Started with PHP: 'aws-sdk'
------------------------------------------------------

.. note:: LeoFS's domains are governed by :ref:`this rule <s3-path-label>`.

Install aws-sdk for PHP
^^^^^^^^^^^^^^^^^^^^^^^

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

::

  127.0.0.1 s3.amazonaws.com
  127.0.0.1 ${bucket_name}.s3.amazonaws.com # if you use create_bucket

Sample Code
^^^^^^^^^^^

.. code-block:: php

  <?php
  require_once 'AWSSDKforPHP/sdk.class.php';

  $s3 = new AmazonS3(array(
    "key" => "YOUR ACCESS KEY ID",
    "secret" => "YOUR SECRET ACCESS KEY",
  ));
  $s3->use_ssl = false;
  $s3->enable_path_style();

  $bucket_name = "bucket";
  $object_name = "key";

  # create bucket (region is a dummy)
  $bucket = $s3->create_bucket($bucket_name, AmazonS3::REGION_US_E1);

  # create object
  $object = $s3->create_object($bucket_name, $object_name, array("body" => "This is a new object."));

  # get object
  $object = $s3->get_object($bucket_name, $object_name);
  print_r($object);

  # get list of buckets
  $buckets = $s3->get_bucket_list();
  print_r($buckets);

  # head
  $head = $s3->get_object_headers($bucket_name, $object_name);
  print_r($head);

  # delete
  $result = $s3->delete_object($bucket_name, $object_name);
  print_r($result);
  ?>

.. _aws-sdk-php2-label:

Getting Started with PHP: 'aws-sdk version 2'
------------------------------------------------------

.. note:: LeoFS's domains are governed by :ref:`this rule <s3-path-label>`.

Install aws-sdk for PHP 2
^^^^^^^^^^^^^^^^^^^^^^^

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

::

  127.0.0.1 s3.amazonaws.com
  127.0.0.1 ${bucket_name}.s3.amazonaws.com # if you use create_bucket

Sample Code
^^^^^^^^^^^^

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

.. Getting Started with Python: 'boto'
.. -------------------------------------

.. Boto is a Python interface to Amazon Web Services. You can use it against LeoFS too.
.. Repository: https://github.com/boto/boto
.. Documentation: http://docs.pythonboto.org/en/latest/index.html

.. Install boto
.. ^^^^^^^^^^^^^^^^^^^^^^

.. setup.py
.. """"""""
.. ::

..   git clone https://github.com/boto/boto.git; cd boto; sudo python setup.py install

.. easy_install
.. """"""""""""
.. ::

..   sudo easy_install boto

.. Sample Code
.. """""""""""

.. .. code-block:: python

..   #!/usr/bin/python
..   # coding: utf8

..   from boto.s3.connection import S3Connection, OrdinaryCallingFormat
..   from boto.s3.bucket import Bucket
..   from boto.s3.key import Key

..   AWS_ACCESS_KEY = "YOUR_ACCESS_KEY_ID"
..   AWS_SECRET_ACCESS_KEY = "YOUR_SECRET_ACCESS_KEY"

..   conn = S3Connection(AWS_ACCESS_KEY,
..                       AWS_SECRET_ACCESS_KEY,
..                       host = "example.com",
..                       port = 8080,
..                       calling_format = OrdinaryCallingFormat(),
..                       is_secure = False
..          )

..   # create bucket
..   bucket = conn.create_bucket("leofs-bucket")

..   # create object
..   s3_object = bucket.new_key("image_file")

..   # write
..   s3_object.set_contents_from_string("This is a text.")

..   # show buckets
..   for bucket in conn.get_all_buckets():
..     print bucket

..     # show S3Objects
..     for obj in bucket.get_all_keys():
..       print obj

..     print

..   # get bucket
..   bucket = conn.get_bucket("leofs-bucket")
..   print bucket

..   # get S3Object
..   s3_object = bucket.get_key("image_file")
..   print s3_object

..   # read
..   print s3_object.read()

..   # write from file
..   #s3_object.set_contents_from_filename("filename")

..   # delete S3Object
..   s3_object.delete()

.. Getting Started with Node: 'knox'
.. -------------------------------------

.. _s3fs-c-label:

Getting Started with S3FS-C (Ubuntu-12.04 LTS)
------------------------------------------------------

S3FS-C is a FUSE (File System in User Space) based file system backed by Amazon S3 storage buckets. Once mounted, S3 can be used just like it was a local file system.

Install libs for S3FS-C into Ubuntu-12.04
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    sudo apt-get install libfuse-dev libcurl4-openssl-dev fuse-utils

Install "S3FS-C"
^^^^^^^^^^^^^^^^^^^^^^^^^

::

    git clone https://github.com/tongwang/s3fs-c.git
    cd s3fs-c
    ./configure
    make
    sudo make install

Modify "/ets/hosts"
^^^^^^^^^^^^^^^^^^^^^^^^^

* Add a LeoFS's domain in ``/ets/hosts``
* LeoFS's domains are governed by :ref:`this rule <s3-path-label>`

::

    $ sudo vi /ets/hosts

    ## Add a LeoFS's domain ##
    127.0.0.1 localhost ${BUCKET_NAME}.localhost

Create a credential file for S3FS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ vi ~/.passwd-s3fs

    ## Set access-key and secret-key ##
    ${ACCESS_KEY}:${SECRET_KEY}

Mount "LeoFS"
^^^^^^^^^^^^^^^^^^^^^^^^^

::

    $ s3fs ${BUCKET_NAME} ${MOUNT_POINT} -o url='http://${END_POINT}:${PORT}'


.. _dragondisk-label:

Connect LeoFS from DragonDisk
------------------------------------------------------

.. note:: LeoFS's domains are governed by :ref:`this rule <s3-path-label>`.

DragonDisk is a powerful file manager for Amazon S3 Compatible Storage.

URL: http://www.dragondisk.com/


Setting up LeoFS account details
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* To setup your LeoFS's account, go to the menu ``File/Accounts``.
* If the details are valid, you can see that S3 has been added on the Root list.

.. image:: _static/images/dragondisk-2.png
   :width: 320px

Create a bucket
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* You need to create a bucket. Because each object is stored in a bucket.
* A bucket retrieved via a unique, developer-assigned key.

.. image:: _static/images/dragondisk-3.png
   :width: 720px


Operating files from  main view
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* You can ``upload files`` into the LeoFS, ``download file`` from the LeoFS and operate others.

.. image:: _static/images/dragondisk-1.png
   :width: 720px

.. _s3cmd-label:

Connect LeoFS from s3cmd
------------------------------------------------------

Getting "s3cmd"
^^^^^^^^^^^^^^^

s3cmd is here: http://sourceforge.net/projects/s3tools/files/

Configure
^^^^^^^^^^^^

.. note:: LeoFS's domains are governed by :ref:`this rule <s3-path-label>`. You need to set 'Endpoint' and 'Port'.

::

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
  Try setting it here if you can't conect to S3 directly
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


Commands
^^^^^^^^^^^^

 +----+-----------------------------------------------------------------------------------------------------+----------------+
 |    | Command                                                                                             | Support Status |
 +====+===============================================+=====================================================+================+
 | 1  | Make bucket                                   | s3cmd mb s3://BUCKET                                | **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 2  | Remove bucket                                 | s3cmd rb s3://BUCKET                                | **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 3  | List objects or bucket                        | s3cmd ls [s3://BUCKET[/PREFIX]]                     | **Yes**        |
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
 | 10 | Get various information about Buckets or Files| s3cmd info s3://BUCKET[/OBJECT]                     | No             |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 11 | Copy object                                   | s3cmd cp s3://BUCKET1/OBJECT1 s3://BUCKET2[/OBJECT2]| **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
 | 12 | Move object                                   | s3cmd mv s3://BUCKET1/OBJECT1 s3://BUCKET2[/OBJECT2]| **Yes**        |
 +----+-----------------------------------------------+-----------------------------------------------------+----------------+
