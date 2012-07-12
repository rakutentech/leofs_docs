Amazon S3 Client Tutorials
================================

Getting Started with Ruby: 'aws-sdk' (made by Amazon)
------------------------------------------------------

A part of the Ruby's library, ``aws-sdk``, is available against the LeoFS as a client. The official document of ``aws-sdk`` is here: http://aws.amazon.com/sdkforruby/.


Install AWS-SDK for Ruby
^^^^^^^^^^^^^^^^^^^^^^^^^

::

  $ gem install aws-sdk


Sample Code
^^^^^^^^^^^^^^^^^^^^^^

Connect to LeoFS
"""""""""""""""""

.. code-block:: ruby

  require "aws-sdk"

  Endpoint = "localhost"
  Port = 8080

  class AWS::S3::Client
    # magic to force subdirectory access
    def self.path_style_bucket_name? bucket_name
      true
    end
  end

  class MyHandler < AWS::Core::Http::NetHttpHandler
    # magic to reconfigure port
    def handle(request, response)
      request.port = ::Port
      super
    end
  end

  AWS.config(
    access_key_id: "YOUR_ACCESS_KEY_ID", # For now, a string is required.
    secret_access_key: "YOUR_SECRET_ACCESS_KEY",
    s3_endpoint: Endpoint
  )

  s3 = AWS::S3.new


PUT an object into the LeoFS
"""""""""""""""""""""""""""""

.. code-block:: ruby

  # This is a Dummy Bucket (LeoFS doesn't support S3 bucket currently).
  bucket = s3.buckets["bucket"]

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

  Image = object.read


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

   S3Object.store('/path/to/remotefile',
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

Getting Started with Node: 'knox'
-------------------------------------
