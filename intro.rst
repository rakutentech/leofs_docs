.. LeoFS documentation master file, created by
   sphinx-quickstart on Tue Feb 21 10:38:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Introduction
================================

LeoFS Overview
--------------------------------

**"LeoFS"** is highly scalable, fault-tolerant distributed file system (DFS) for the Web. Different than traditional distributed file system and other DFS — **"LeoFS"** offers a number of unique benefits to users:

* High Cost Performance
* High Reliability
* High Scalability

.. image:: _static/images/leofs-architecture.011.png
   :width: 760px

Goals
--------------------------------

* LeoFS aims to provide the following advantages:
    * HIGH Cost Performance
        * Fast - Over 200MB/sec into 10GE
        * A lower cost than other storage
        * Provide easy management and easy operation
    * HIGH Reliability
        * Nine nines - Operating ratio is 99.9999999%
    * High Scalability
        * Build "Huge Cluster" at low cost

Milestones
--------------------------------

* 0.9.1
    * Large Object Support (over 64MB)
    * Support Cowboy on "leo_gateway"
    * Enhance S3-API (1)
        * Bucket-related
* 0.9.2
    * Enhance S3-API (2)
        * Authentication
    * Web-Console (Leo Tamer)
        * Log Analysis/Search
        * File Manager

