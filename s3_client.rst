Amazon S3 Client Tutorials
================================

Getting Your S3 Key
---------------------

You can get s3 keys from ``LeoFS's Manager Console``.

::

  $ telnet 127.0.0.1 10010
  Trying 127.0.0.1...
  Connected to 127.0.0.1.
  Escape character is '^]'.

's3-gen-key ${USER-ID}' command generates your s3 key.

::

  s3-gen-key hoge
  access-key-id: 05dcba94333c7590a635
  secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a

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

.. note:: You need to rewrite 'Endpoint' and 'Port' as follows:


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
  s3.buckets.create("test")

  # get bucket
  bucket = s3.buckets["test"]

  # create new object - like unix's touch-command
  object = bucket.objects.create("image")

  # show objects in the bucket
  bucket.objects.each do |obj|
    p obj
  end

  # get S3Object
  object = bucket.objects["image"]

  # write object from file
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

.. Getting Started with Node: 'knox'
.. -------------------------------------


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

Edit "/ets/hosts"
^^^^^^^^^^^^^^^^^^^^^^^^^

::

    127.0.0.1 localhost {bucket_name}.localhost

Set "~/.passwd-s3fs"
^^^^^^^^^^^^^^^^^^^^^^^^^

::

    {access_key}:{secret}

Mount "LeoFS"
^^^^^^^^^^^^^^^^^^^^^^^^^

::

    s3fs {bucket_name} {mount_point} -o url='http://{endpoint}:{port}'


Connect LeoFS from DragonDisk
------------------------------------------------------

DragonDisk is a powerful file manager for Amazon S3 Compatible Storage.

URL: http://www.dragondisk.com/


Setting up LeoFS account details
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* To setup your LeoFS's account, go to the menu ``File/Accounts``.
* If the details are valid, you can see that S3 has been added on the Root list.

.. image:: _static/images/dragondisk-2.png
   :width: 320px

Operating files from  main view
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* You can ``upload files`` into the LeoFS, ``download file`` from the LeoFS and operate others.

.. image:: _static/images/dragondisk-1.png
   :width: 720px


