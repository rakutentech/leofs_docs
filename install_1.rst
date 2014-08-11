.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: Erlang; Installation

Install Erlang
---------------
LeoFS development currently targets Debian 6, Ubuntu-Server 12.04 LTS or Higher and CentOS 6.5, but should work on
most Linux platforms with the following software installed:

* `Erlang/OTP R16B03-1 <http://www.erlang.org/download_release/23>`_

.. note:: We recommend this installation method. Please follow the relevant instructions for your environment.


Preparation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Install required libraries using yum (CentOS 6.5)
"""""""""""""""""""""""""""""""""""""""""""""""""
.. index::
   pair: CentOS-6.5; Installation

::

   $ sudo yum install gcc gcc-c++ glibc-devel make ncurses-devel openssl-devel autoconf \
                      libuuid-devel cmake check check-devel

Install required libraries using apt-get (Ubuntu Server 12.04 LTS or Higher)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
.. index::
   pair: Ubuntu-12.04; Installation

::

   $ sudo apt-get install build-essential libtool libncurses5-dev libssl-dev cmake check

Install "libatomic_ops" for R16B03-1  *(both CentOS and Ubuntu)*
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash

   $ wget http://www.ivmaisoft.com/_bin/atomic_ops/libatomic_ops-7.4.2.tar.gz
   $ tar xzvf libatomic_ops-7.4.2.tar.gz
   $ cd libatomic_ops-7.4.2
   $ ./configure --prefix=/usr/local
   $ make
   $ sudo make install

Download "Erlang R16B03-1"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   ## [R16B03-1]
   $ cd $WORK_DIR
   $ wget http://www.erlang.org/download/otp_src_R16B03-1.tar.gz


Build Erlang on CentOS 6.5
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   $ tar xzf otp_src_R16B03-1.tar.gz
   $ cd otp_src_R16B03-1
   $ CFLAGS="-DOPENSSL_NO_EC=1" \
     ./configure --prefix=/usr/local/erlang/R16B03-1 \
                 --enable-smp-support \
                 --enable-m64-build \
                 --enable-halfword-emulator \
                 --enable-kernel-poll \
                 --without-javac \
                 --disable-native-libs \
                 --disable-hipe \
                 --disable-sctp \
                 --enable-threads \
                 --with-libatomic_ops=/usr/local
   $ make
   $ sudo make install

Build Erlang on Debian and others
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

   $ tar xzf otp_src_R16B03-1.tar.gz
   $ cd otp_src_R16B03-1
   $ ./configure --prefix=/usr/local/erlang/R16B03-1 \
                 --enable-smp-support \
                 --enable-m64-build \
                 --enable-halfword-emulator \
                 --enable-kernel-poll \
                 --without-javac \
                 --disable-native-libs \
                 --disable-hipe \
                 --disable-sctp \
                 --enable-threads \
                 --with-libatomic_ops=/usr/local
   $ make
   $ sudo make install

Confirm
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ erl
    Erlang R16B03-1 (erts-5.10.4) [source] [64-bit halfword] [smp:8:8] [async-threads:10] [kernel-poll:false]

    Eshell V5.10.4  (abort with ^G)
    1>

