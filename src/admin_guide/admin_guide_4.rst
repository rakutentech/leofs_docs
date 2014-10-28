.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    Recover commands

Recover Commands
=================

+-------------------------------------------------------------------------+--------------------------------------------------------------------+
| **Shell**                                                               | **Description**                                                    |
+=========================================================================+====================================================================+
| leofs-adm :ref:`recover-file <recover-file-command>` <file-path>        | Recover an inconsistent object specified by the file-path          |
+-------------------------------------------------------------------------+--------------------------------------------------------------------+
| leofs-adm :ref:`recover-node <recover-node-command>` <storage-node>     | Recover all inconsistent objects in the specified node             |
+-------------------------------------------------------------------------+--------------------------------------------------------------------+
| leofs-adm :ref:`recover-ring <recover-ring-command>` <storage-node>     | Recover rings of the specified node                                |
+-------------------------------------------------------------------------+--------------------------------------------------------------------+
| leofs-adm :ref:`recover-cluster <recover-cluster-command>` <cluster-id> | [v1.0.0-] Recover all inconsistent objects in the specified cluster|
+-------------------------------------------------------------------------+--------------------------------------------------------------------+


.. _recover-file-command:

.. index::
    pair: Recover commands; recover-file-command

recover-file <file-path>
^^^^^^^^^^^^^^^^^^^^^^^^^

Recover an inconsistent object specified by the file-path

.. code-block:: bash

  $ leofs-adm recover-file leo/fast/storage.key
  OK

\


.. _recover-node-command:

.. index::
    pair: Recover commands; recover-node-command

recover-node <node>
^^^^^^^^^^^^^^^^^^^

Recover all inconsistent objects in the specified node

.. code-block:: bash

  $ leofs-adm recover-node storage_0@127.0.0.1
  OK

\

.. _recover-ring-command:

.. index::
    pair: Recover commands; recover-ring-command

recover-ring <node>
^^^^^^^^^^^^^^^^^^^

Recover rings of the specified node

.. code-block:: bash

  $ leofs-adm recover-ring storage_0@127.0.0.1
  OK

\

.. _recover-cluster-command:

.. index::
    pair: Recover commands; recover-cluster-command

recover-cluster <cluster-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Recover all inconsistent objects in the specified cluster

.. code-block:: bash

  $ leofs-adm recover-cluster cluster-1
  OK
