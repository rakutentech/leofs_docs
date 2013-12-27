.. LeoFS documentation master file, created by
   sphinx-quickstart on Tue Feb 21 10:38:17 2012.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. index::
    pair: Benchmark; Benchmark

Benchmarking
================================

.. index::
    pair: Set up basho_bench; Set up basho_bench

Setting up basho_bench
--------------------------------

Installation
^^^^^^^^^^^^

* `Basho's basho_bench's repository <https://github.com/basho/basho_bench/>`_
* `Basho's basho_bench's documentation <http://docs.basho.com/riak/latest/cookbooks/Benchmarking>`_
* Use the following commands to set up basho_bench.

.. code-block:: bash

    $ git clone git://github.com/basho/basho_bench.git
    $ git clone https://github.com/leo-project/leofs.git
    $ cd basho_bench
    $ cp -i ../leofs/test/src/*.erl src/
    $ cp -i ../leofs/test/include/*.hrl include/
    $ make all

Preparations before testing
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Create a test bucket
""""""""""""""""""""

.. note:: After starting LeoFS, you need to create a ``bucket`` on "manager-console" that will be used for testing. In this example, the bucket name is ``test``. It is owned by the user ``_test_leofs`` that is already registered internally by LeoFS.

.. code-block:: bash

    $ telnet ${MANAGER_CONSOLE_IP} 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    add-endpoint ${GATEWAY_IP}
    OK

    add-bucket test 05236
    OK

    get-buckets
    bucket   | owner       | created at
    ---------+-------------+---------------------------
    test     | _test_leofs | 2013-02-27 14:06:54 +0900

    update-acl test 05236 public-read
    OK


.. index::
    pair: Configuration file for basho_bench; Configuration file for basho_bench

Configuration file for basho_bench
----------------------------------

Examples
^^^^^^^^

* Some examples are included in the LeoFS repository at ${LEOFS_ROOT}/test/conf/leofs_*.config
    * `Learn more about basho_bench's configuration <http://docs.basho.com/riak/latest/cookbooks/Benchmarking/#Configuration>`_

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
  | http_raw_ips      | The `Gateway nodes` we want to benchmark               |
  +-------------------+--------------------------------------------------------+
  | http_raw_port     | The port used by the Gateway nodes                     |
  +-------------------+--------------------------------------------------------+
  | http_raw_path     | URL path prefix. The first path segment MUST be a      |
  |                   | BUCKET name                                            |
  +-------------------+--------------------------------------------------------+
  | check_integrity   | Check integrity of registered object -                 |
  | `(default:false)` | compare an original MD5 with a retrieved object's MD5  |
  |                   |                                                        |
  |                   | (Only for developers)                                  |
  +-------------------+--------------------------------------------------------+

.. index::
    pair: Run basho_bench; Run basho_bench

Running basho_bench(1)
--------------------------------

.. note:: In this example, ``LeoFS`` and ``basho_bench`` are installed locally.

* The following commands can be used to run basho_bench.

.. code-block:: bash

    ### Loading 1M records of size 16KB
    cd basho_bench
    ./basho_bench ../leofs/test/conf/leofs_16K_LOAD1M.config

\

Running basho_bench(2)
--------------------------------

.. note:: In this example, ``LeoFS`` and ``basho_bench`` are installed on different hosts.


Configure the ``endpoint`` on LeoFS-Manager console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Allows basho_bench's requests to reach `${HOST_NAME_OF_LEOFS_GATEWAY}`.

.. code-block:: bash

    $ telnet ${MANAGER_CONSOLE_IP} 10010
    Trying 127.0.0.1...
    Connected to localhost.
    Escape character is '^]'.

    add-endpoint ${HOST_NAME_OF_LEOFS_GATEWAY}
    OK

    get-endpoints
    endpoint                      | created at
    ------------------------------+---------------------------
    localhost                     | 2013-03-01 00:14:04 +0000
    s3.amazonaws.com              | 2013-03-01 00:14:04 +0000
    ${HOST_NAME_OF_LEOFS_GATEWAY} | 2013-03-01 00:14:04 +0000



Edit the benchmark's configuration file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* You need to modify the values for ``http_raw_ips`` and ``http_raw_port``

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

Running basho_bench
^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    ### Loading 1M records each size is 16KB
    cd basho_bench
    ./basho_bench ../leofs/test/conf/leofs_16K_LOAD1M.config


