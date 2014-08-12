.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    LeoFS Gateway operation

LeoFS Gateway Operation
=======================

+--------------------------------------------------------------+-----------------------------------------------------------------------------------+
| **Shell**                                                    | **Description**                                                                   |
+==============================================================+===================================================================================+
| leofs-adm :ref:`purge-cache <purge>` <file-path>             | * Remove the cache from each LeoFS Gateway node                                   |
+--------------------------------------------------------------+-----------------------------------------------------------------------------------+
| leofs-adm :ref:`remove-gateway <remove>` <gateway-node>      | * Remove the LeoFS Gateway node, which is already stopped                         |
+--------------------------------------------------------------+-----------------------------------------------------------------------------------+

\

.. _purge:

.. index::
    pair: LeoFS Gateway operation; purge-command

purge-cache <file-path>
^^^^^^^^^^^^^^^^^^^^^^^

Remove the cache from each gateway

.. code-block:: bash

    $ leofs-adm purge-cache leo/fast/storage.key
    OK

\

.. _remove:

.. index::
    pair: LeoFS Gateway operation; remove-command

remove-gateway <gateway-node>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    $ leofs-adm remove-gateway gateway_0@127.0.0.1
    OK
