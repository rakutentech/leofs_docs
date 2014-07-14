
.. _boto-label:

Getting Started with Python
---------------------------

* Boto is a Python interface to Amazon Web Services. You can use it for LeoFS too.
    * `Repository <https://github.com/boto/boto>`_
    * `Documentation <http://docs.pythonboto.org/en/latest/index.html>`_

Install "boto"
^^^^^^^^^^^^^^

setup.py
""""""""
::

  git clone https://github.com/boto/boto.git; cd boto; sudo python setup.py install

easy_install
""""""""""""
::

  sudo easy_install boto

Example usage
"""""""""""""

.. note:: LeoFS domains are ruled by :ref:`this rule <s3-path-label>`.

.. code-block:: python

  #!/usr/bin/python
  # coding: utf8

  from boto.s3.connection import S3Connection, OrdinaryCallingFormat
  from boto.s3.bucket import Bucket
  from boto.s3.key import Key

  AWS_ACCESS_KEY = "YOUR_ACCESS_KEY_ID"
  AWS_SECRET_ACCESS_KEY = "YOUR_SECRET_ACCESS_KEY"

  conn = S3Connection(AWS_ACCESS_KEY,
                      AWS_SECRET_ACCESS_KEY,
                      host = "example.com",
                      port = 8080,
                      calling_format = OrdinaryCallingFormat(),
                      is_secure = False
         )

  # create bucket
  bucket = conn.create_bucket("leofs-bucket")

  # create object
  s3_object = bucket.new_key("image_file")

  # write
  s3_object.set_contents_from_string("This is a text.")

  # show buckets
  for bucket in conn.get_all_buckets():
    print bucket

    # show S3Objects
    for obj in bucket.get_all_keys():
      print obj

    print

  # get bucket
  bucket = conn.get_bucket("leofs-bucket")
  print bucket

  # get S3Object
  s3_object = bucket.get_key("image_file")
  print s3_object

  # read
  print s3_object.read()

  # write from file
  #s3_object.set_contents_from_filename("filename")

  # delete S3Object
  s3_object.delete()

