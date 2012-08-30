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

* Basho basho_bench <https://github.com/basho/basho_bench/>
* Commands to set up basho_bench are following.

::

    git clone git://github.com/basho/basho_bench.git
    git clone https://github.com/leo-project/leofs.git
    cd basho_bench
    cp -i ../leofs/test/src/*.erl src/
    cp -i ../leofs/test/include/*.hrl include/
    make all

.. index::
    pair: Configuration file for basho_bench; Configuration file for basho_bench

Configuration file for basho_bench
-------------------------------------

Samples
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Some samples are included in leofs repo where path is ${LEOFS_ROOT}/test/conf/leofs_*.config

::

    {mode,      max}.
    {duration,   3}.
    {concurrent, 48}.
    
    {driver, basho_bench_driver_leofs}.
    {code_paths, ["deps/ibrowse"]}.
    
    {http_raw_ips, ["localhost"]}.
    {http_raw_port, 8080}.
    {http_raw_path, "/bbb"}.
    
    {key_generator,   {partitioned_sequential_int, 1000000}}.
    {value_generator, {fixed_bin, 16384}}.
    {operations, [{put,1}]}.

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

 
