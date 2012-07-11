Amazon S3 Client Samples
================================

Getting Started with Ruby: 'aws-s3'
-------------------------------------

A part of the Ruby's library, ``aws-s3``, is available against the LeoFS gateways as a client. The official document of ``aws-s3`` is here: http://amazon.rubyforge.org/.

Installation
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
