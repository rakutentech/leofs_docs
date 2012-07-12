Tutorial for Ruby
=================
.. index::
   pair: Ruby; Amazon S3 API

Install AWS-SDK for Ruby
------------------------

::
  
  gem install aws-sdk

Sample Code
-----------

.. code-block:: ruby

  require "aws-sdk"
  
  Endpoint = "example.com"
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
  
  # This is a Dummy Bucket (LeoFS doesn't support S3 bucket currently).
  bucket = s3.buckets["hoge"]
  
  # create new object
  object = bucket.objects.create("hoge")
  
  # show objects in the bucket
  bucket.objects.each do |obj|
    p obj
  end
  
  # get S3Object
  object = bucket.objects["hoge"]
  
  File.open("foo", "w") {|f| f.puts "Hello, LeoFS!" }
  
  # write object from file
  object.write(
    file: "foo",
    content_type: "text/plain"
  )
  
  File.delete("foo")
  
  # PUT
  object.write("This is a text.")
  
  # HEAD
  p object.head
  
  # GET
  p object.read
  
  # DELETE
  object.delete
