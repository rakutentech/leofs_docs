.. LeoFS documentation
.. Copyright (c) 2013-2014 Rakuten, Inc.

.. _aws-sdk-ruby-label:

Getting Started with Ruby
-------------------------

aws-sdk's official documentation is `here. <http://aws.amazon.com/sdkforruby/>`_

Install AWS-SDK for Ruby
^^^^^^^^^^^^^^^^^^^^^^^^

::

  $ gem install aws-sdk

Example usage
^^^^^^^^^^^^^

Connecting to LeoFS
"""""""""""""""""""

.. note:: LeoFS' domains are ruled by :ref:`this rule <s3-path-label>`. You need to configure the *Endpoint*, *Port*, *AccessKeyId* and *SecretAccessKey* like in this example:


.. code-block:: ruby

  ## This code supports "aws-sdk v1.9.5"
  require "aws-sdk"

  Endpoint = "localhost"
  Port = 8080
  # set your s3 key
  AccessKeyId = "YOUR_ACCESS_KEY_ID"
  SecretAccessKey = "YOUR_SECRET_ACCESS_KEY"

  class LeoFSHandler < AWS::Core::Http::NetHttpHandler
    def handle(request, response)
      request.port = ::Port
      super
    end
  end

  SP = AWS::Core::CredentialProviders::StaticProvider.new(
  {
      :access_key_id     => AccessKeyId,
      :secret_access_key => SecretAccessKey
  })

  AWS.config(
    access_key_id: AccessKeyId,
    secret_access_key: SecretAccessKey,
    s3_endpoint: Endpoint,
    http_handler: LeoFSHandler.new,
    credential_provider: SP,
    s3_force_path_style: true,
    use_ssl: false
  )

  s3 = AWS::S3.new


PUT an object into LeoFS
""""""""""""""""""""""""

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


GET an object from LeoFS
""""""""""""""""""""""""

.. code-block:: ruby

  image = object.read


DELETE an object from LeoFS
"""""""""""""""""""""""""""

.. code-block:: ruby

  object.delete


Get an object HEAD from LeoFS
"""""""""""""""""""""""""""""

.. code-block:: ruby

  metadata = object.head
  p metadata.to_hash


Uploading to LeoFS using multipart
"""""""""""""""""""""""""""""""""""

.. code-block:: ruby

  ## This code supports "aws-sdk v1.9.5"
  require 'aws-sdk'

  Endpoint = "leofs.org"
  Port = 8080

  ## set your s3 key
  AccessKeyId = "YOUR_ACCESS_KEY_ID"
  SecretAccessKey = "YOUR_SECRET_ACCESS_KEY"

  class LeoFSHandler < AWS::Core::Http::NetHttpHandler
    def handle(request, response)
      request.port = ::Port
      super
    end
  end

  SP = AWS::Core::CredentialProviders::StaticProvider.new(
  {
      :access_key_id     => AccessKeyId,
      :secret_access_key => SecretAccessKey
  })

  AWS.config(
    :access_key_id => 'access-key-id',
    :secret_access_key => 'secret-access-key',
    s3_endpoint: Endpoint,
    http_handler: LeoFSHandler.new,
    credential_provider: SP,
    s3_force_path_style: true,
    use_ssl: false
  )

  file_path_for_multipart_upload = '/path/to/file'
  bucket = AWS::S3.new.buckets['bucket-name']

  open(file_path_for_multipart_upload) do |file|
    uploading_object = bucket.objects[File.basename(file.path)]
    uploading_object.multipart_upload do |upload|
      while !file.eof?
        upload.add_part(file.read 5242880) ## 5MB ##
        p('Aborted') if upload.aborted?
      end
    end
  end


