.. LeoFS documentation master file, created by
   sphinx-quickstart on Tue Feb 21 10:38:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. index::
    pair: Benchmark; Benchmark

Benchmark
================================

.. index::
    pair: Set up basho_bench; Set up basho_bench

Set up basho_bench
--------------------------------

Install
^^^^^^^^

* `Basho basho_bench <https://github.com/basho/basho_bench/>`_
* Commands to set up basho_bench are following.

::

    git clone git://github.com/basho/basho_bench.git
    git clone https://github.com/leo-project/leofs.git
    cd basho_bench
    cp -i ../leofs/test/src/*.erl src/
    cp -i ../leofs/test/include/*.hrl include/
    make all

Configuration
^^^^^^^^^^^^^

Put a bucket for the tests
""""""""""""""""""""""""""""

.. note:: After launch LeoFS, You need to put ``bucket`` on "manager-console" before the tests. In this example, the bucket-name is ``test`` and a user, ``_test_leofs`` as "test-user" is registered already into LeoFS.

::

    $ telnet ${MANAGER_CONSOLE_IP} 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    add-bucket test 05236


Edit `"/etc/hosts"`
"""""""""""""""""""

.. note:: LeoFS's domains are ruled by :ref:`this rule <s3-path-label>`.

* You need to modify ``/etc/hosts`` before the tests because basho_bench cannot reach LeoFS-Gateway. 

::

  127.0.0.1 localhost
  127.0.0.1 test.localhost


.. index::
    pair: Configuration file for basho_bench; Configuration file for basho_bench

Configuration file for basho_bench
-------------------------------------

Samples
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Some samples are included in leofs repo where path is ${LEOFS_ROOT}/test/conf/leofs_*.config

.. code-block:: erlang

    {mode,      max}.
    {duration,   3}.
    {concurrent, 48}.
    
    {driver, basho_bench_driver_leofs}.
    {code_paths, ["deps/ibrowse"]}.
    
    {http_raw_ips, ["${HOST_NAME}"]}.
    {http_raw_port, 8080}.
    {http_raw_path, "/${BUCKET}"}.
    
    {key_generator,   {partitioned_sequential_int, 1000000}}.
    {value_generator, {fixed_bin, 16384}}. %% 16KB
    {operations, [{put,1}]}. %% PUT:100%

Description
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  +---------------+--------------------------------------------------------+
  | Key           | Value                                                  |
  +===============+========================================================+
  | http_raw_ips  | Target hosts which are equal to `Gateway Nodes`        |
  +---------------+--------------------------------------------------------+
  | http_raw_port | Target port listening on Gateway Nodes                 |
  +---------------+--------------------------------------------------------+
  | http_raw_path | URL path prefix. First level of path MUST be matched a |
  |               | BUCKET name                                            |
  +---------------+--------------------------------------------------------+

These are covered more in detail on the `Basho wiki <http://wiki.basho.com/Benchmarking-with-Basho-Bench.html>`_.

.. index::
    pair: Run basho_bench; Run basho_bench

Run basho_bench
--------------------------------

Commands to run basho_bench are following.

::

    ### Loading 1M records each size is 16KB
    cd basho_bench
    ./basho_bench ../leofs/test/conf/leofs_16K_LOAD1M.config

 
