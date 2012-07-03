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
    cp -i ../leofs/test/basho_bench_driver_leofs.erl src/
    make all

.. index::
    pair: Configuration file for basho_bench; Configuration file for basho_bench

Configuration file for basho_bench
-------------------------------------

Samples
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Some samples are included in leofs repo where path is ${LEOFS_ROOT}/test/leofs_*.config

::

    {mode,      max}.
    {duration,   3}.
    {concurrent, 48}.
    
    {driver, basho_bench_driver_leofs}.
    {code_paths, ["deps/ibrowse"]}.
    
    {http_raw_ips, ["localhost"]}.
    {http_raw_port, 8080}.
    {http_raw_path, "/air/_test/16KB"}.
    
    {key_generator,   {uniform_int, 1000000}}.
    {value_generator, {fixed_bin, 16000}}.
    {operations, [{get, 4},{put,1}]}.

Description
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  +---------------+--------------------------------------------------------+
  | Key           | Value                                                  |
  +===============+========================================================+
  | http_raw_ips  | Target hosts which are equal to `Gateway Nodes`        |
  +---------------+--------------------------------------------------------+
  | http_raw_port | Target port listening on Gateway Nodes                 |
  +---------------+--------------------------------------------------------+
  | http_raw_path | URL path prefix                                        |
  +---------------+--------------------------------------------------------+

These are covered more in detail on the `Basho wiki <http://wiki.basho.com/Benchmarking-with-Basho-Bench.html>`_.

.. index::
    pair: Run basho_bench; Run basho_bench

Run basho_bench
--------------------------------

Commands to run basho_bench are following.

::

    cd basho_bench
    ./basho_bench ../leofs/test/leofs_16K_W.config

 
