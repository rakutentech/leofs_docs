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

* 0.10 (Aug 2012)
    * Enhance S3-API
        * Authentication
        * Bucket-related
* 0.12 (Oct 2012)
    * Large Object Support
    * Support Cowboy on "leo_gateway"
    * Web-Console (Leo Tamer)
        * Log Analysis/Search
* 0.14 (Dec 2012)
    * Multi-layer Cache (Using SSD)
    * Multi-tenant
    * Streaming
    * Web-Console (Leo Tamer)
        * Cluster manager/monitor
* 0.16 (2013)
    * HBase integration
        * Distributed Lock Mechanism

