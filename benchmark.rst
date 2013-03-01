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

* `Basho basho_bench's repository <https://github.com/basho/basho_bench/>`_
* `Basho basho_bench's docs <http://docs.basho.com/riak/latest/cookbooks/Benchmarking>`_
* Commands to set up basho_bench are following.

.. code-block:: bash

    $ git clone git://github.com/basho/basho_bench.git
    $ git clone https://github.com/leo-project/leofs.git
    $ cd basho_bench
    $ cp -i ../leofs/test/src/*.erl src/
    $ cp -i ../leofs/test/include/*.hrl include/
    $ make all

Prepare before the tests
^^^^^^^^^^^^^^^^^^^^^^^^

Put a bucket for the tests
""""""""""""""""""""""""""

.. note:: After launch LeoFS, You need to put ``bucket`` on "manager-console" before the tests. In this example, the bucket-name is ``test`` and a user, ``_test_leofs`` as "test-user" is registered already into LeoFS.

.. code-block:: bash

    $ telnet ${MANAGER_CONSOLE_IP} 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    add-bucket test 05236
    OK

    get-buckets
    bucket   | owner       | created at
    ---------+-------------+---------------------------
    test     | _test_leofs | 2013-02-27 14:06:54 +0900


.. index::
    pair: Configuration file for basho_bench; Configuration file for basho_bench

Configuration file for basho_bench
-------------------------------------

Samples
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Some samples are included in leofs repo where path is ${LEOFS_ROOT}/test/conf/leofs_*.config
    * You can see more detail - `basho_bench's configuration <http://docs.basho.com/riak/latest/cookbooks/Benchmarking/#Configuration>`_

.. code-block:: erlang

    {mode,      max}.
    {duration,   10}.
    {concurrent, 50}.

    {driver, basho_bench_driver_leofs}.
    {code_paths, ["deps/ibrowse"]}.

    {http_raw_ips, ["${HOST_NAME_OF_LEOFS_GATEWAY}"]}.
    {http_raw_port, 8080}.
    {http_raw_path, "/test"}.
    %% {http_raw_path, "/${BUCKET}"}.

    {key_generator,   {partitioned_sequential_int, 1000000}}.
    {value_generator, {fixed_bin, 16384}}. %% 16KB
    {operations, [{put,1}]}.               %% PUT:100%
    %%{operations, [{put,1}, {get, 4}]}.   %% PUT:20%, GET:80%

    {check_integrity, false}.


Description
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  +-------------------+--------------------------------------------------------+
  | Key               | Value                                                  |
  +===================+========================================================+
  | http_raw_ips      | Target hosts which are equal to `Gateway Nodes`        |
  +-------------------+--------------------------------------------------------+
  | http_raw_port     | Target port listening on Gateway Nodes                 |
  +-------------------+--------------------------------------------------------+
  | http_raw_path     | URL path prefix. First level of path MUST be matched a |
  |                   | BUCKET name                                            |
  +-------------------+--------------------------------------------------------+
  | check_integrity   | Check integrity of registered object -                 |
  | `(default:false)` | compare an original-MD5 with a retrieving object's MD5 |
  |                   |                                                        |
  |                   | (Only for developers)                                  |
  +-------------------+--------------------------------------------------------+

.. index::
    pair: Run basho_bench; Run basho_bench

Run basho_bench(1)
--------------------------------

.. note:: In this example, ``LeoFS`` and ``basho_bench`` are installed into "local".

* Commands to run basho_bench are following.

.. code-block:: bash

    ### Loading 1M records each size is 16KB
    cd basho_bench
    ./basho_bench ../leofs/test/conf/leofs_16K_LOAD1M.config

\

Run basho_bench(2)
--------------------------------

.. note:: In this example, ``LeoFS`` and ``basho_bench`` are installed into respective hosts.


Set ``endpoint`` on LeoFS-Manager console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* From basho_bench's requests needs to reach `${HOST_NAME_OF_LEOFS_GATEWAY}`

.. code-block:: bash

    $ telnet ${MANAGER_CONSOLE_IP} 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    set-endpoint ${HOST_NAME_OF_LEOFS_GATEWAY}
    OK

    get-endpoints
    endpoint                      | created at
    ------------------------------+---------------------------
    localhost                     | 2013-03-01 00:14:04 +0000
    s3.amazonaws.com              | 2013-03-01 00:14:04 +0000
    ${HOST_NAME_OF_LEOFS_GATEWAY} | 2013-03-01 00:14:04 +0000



Edit LeoFS-benchmark's configutation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* You need to modify configuration's items of ``http_raw_ips`` and ``http_raw_port``

.. code-block:: erlang

    {mode,      max}.
    {duration,   10}.
    {concurrent, 50}.

    {driver, basho_bench_driver_leofs}.
    {code_paths, ["deps/ibrowse"]}.

    {http_raw_ips, ["${HOST_NAME_OF_LEOFS_GATEWAY}"]}. %% able to set plural nodes
    {http_raw_port, ${PORT}}. %% default: 8080
    {http_raw_path, "/test"}.
    %% {http_raw_path, "/${BUCKET}"}.

    {key_generator,   {partitioned_sequential_int, 1000000}}.
    {value_generator, {fixed_bin, 16384}}. %% 16KB
    {operations, [{put,1}]}.               %% PUT:100%
    %%{operations, [{put,1}, {get, 4}]}.   %% PUT:20%, GET:80%

    {check_integrity, false}.

Run
^^^

.. code-block:: bash

    ### Loading 1M records each size is 16KB
    cd basho_bench
    ./basho_bench ../leofs/test/conf/leofs_16K_LOAD1M.config


