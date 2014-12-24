.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. _aws-sdk-golang-label:

Getting Started with Golang
---------------------------

`crowdmob/goamz <https://github.com/crowdmob/goamz>`_ is one of golang packages to interact with the `Amazon Web Service <http://aws.amazon.com/>`_ . 

Install goamz
^^^^^^^^^^^^^

::

  $ go get github.com/crowdmob/goamz/aws
  $ go get github.com/crowdmob/goamz/s3


Example usage
^^^^^^^^^^^^^

PUT an object into LeoFS
""""""""""""""""""""""""

This example tries to connect LeoFS and put an object. 

.. code-block:: golang

   package main

   import (
       "fmt"

       "github.com/crowdmob/goamz/aws"
       "github.com/crowdmob/goamz/s3"
   )

   func main() {

       auth := aws.Auth{
           AccessKey: "YOUR_ACCESS_KEY",
           SecretKey: "YOUR_SECRET_KEY",
       }

       region := aws.Region{
           S3Endpoint: fmt.Sprintf("%s:%s", "YOUR_END_POINT_HOST", "YOUR_END_POINT_PORT"),
       }

       client := s3.New(auth, region)
       bucket := client.Bucket("YOUR_BUCKET_NAME")

       err := bucket.Put("YOUR_PATH", []byte("YOUR_CONTENTS"), "text/plain", "", s3.Options{})
       if err != nil {
           panic(err)
       }
   }

.. note:: LeoFS' domains are ruled by :ref:`this rule <s3-path-label>`. You need to configure the *Endpoint*, *Port*, *AccessKeyId* and *SecretAccessKey*

GET an object from LeoFS
"""""""""""""""""""

This example tries to connect LeoFS and get an object. 

.. code-block:: golang

   package main

   import (
       "fmt"
       "io/ioutil"

       "github.com/crowdmob/goamz/aws"
       "github.com/crowdmob/goamz/s3"
   )

   func main() {

       auth := aws.Auth{
           AccessKey: "YOUR_ACCESS_KEY",
           SecretKey: "YOUR_SECRET_KEY",
       }

       region := aws.Region{
           S3Endpoint: fmt.Sprintf("%s:%s", "YOUR_END_POINT_HOST", "YOUR_END_POINT_PORT"),
       }

       client := s3.New(auth, region)
       bucket := client.Bucket("YOUR_BUCKET_NAME")

       r, err := bucket.GetReader("YOUR_PATH")
       if err != nil {
           panic(err)
       }

       content, _ := ioutil.ReadAll(r)
       r.Close()
   }


DELETE an object from LeoFS
"""""""""""""""""""""""""""

This example tries to connect LeoFS and delete an object. 

.. code-block:: golang

   package main

   import (
       "fmt"

       "github.com/crowdmob/goamz/aws"
       "github.com/crowdmob/goamz/s3"
   )

   func main() {

       auth := aws.Auth{
           AccessKey: "YOUR_ACCESS_KEY",
           SecretKey: "YOUR_SECRET_KEY",
       }

       region := aws.Region{
           S3Endpoint: fmt.Sprintf("%s:%s", "YOUR_END_POINT_HOST", "YOUR_END_POINT_PORT"),
       }

       client := s3.New(auth, region)
       bucket := client.Bucket("YOUR_BUCKET_NAME")
       
       err := bucket.Del("YOUR_PATH")
       if err != nil {
           panic(err)
       }
   }


