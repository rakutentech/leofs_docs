.. LeoFS documentation
.. @author leo-project
.. @since July, 2012

LeoFS v1.0 Manual
=================

**LeoFS** is a highly available, distributed, eventually consistent object/blob store. If you are searching a storage system that is able to store huge amount and various kind of files such as photo, movie, log data and so on, LeoFS is suitable for that.

LeoFS is supporting the following features:

* S3-API Support
    * LeoFS is an Amazon S3 compatible storage system.
    * Switch to LeoFS to decrease your cost from more expensive public-cloud solutions.
* Large Object Support
    * LeoFS can handle files with more than GB
* Multi Data Center Replication
    * LeoFS is a highly scalable, fault-tolerant distributed file system without SPOF.
    * LeoFS's cluster can be viewed as ONE-HUGE storage. It consists of a set of loosely connected nodes.
    * We can build a global scale storage system with easy operations

We can access LeoFS server using S3 clients and S3 client libries of each programming language.

Slide
^^^^^

The presentation - `Scaling and High Performance Storage System: LeoFS <http://www.slideshare.net/rakutentech/scaling-and-high-performance-storage-system-leofs>`_. was given at Erlang User Conference 2014 in Stockholm on June 2014

Goal
^^^^

LeoFS aims to provide all of 3-HIGHs as follow:

* HIGH Reliability
    * Nine nines - Operating ratios is 99.9999999%
* High Scalability
    * Build huge-cluster at low cost
* HIGH Cost Performance
    * Fast - Over 10Gbps
    * A lower cost than other storage
    * Provide easy management and easy operation


.. toctree::
   :hidden:

   intro
   getting_started
   install
   configuration
   admin_guide
   usage
   rest_api
   s3_api
   s3_client
   benchmark
   leo_center
   faq_index
