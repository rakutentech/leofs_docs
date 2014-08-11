.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
    Recover commands

Recover Commands
=================

+---------------------------------+---------------------------------------------------------------------------------------------------+
| Command                         | Description                                                                                       |
+=================================+===================================================================================================+
| **Recover Commands**                                                                                                                |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover file `<file-path>`      | Recover an inconsistent object specified by the file-path                                         |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover node `<storage-node>`   | Recover all inconsistent objects in the specified node                                            |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover ring `<storage-node>`   | Recover rings of the specified node                                                               |
+---------------------------------+---------------------------------------------------------------------------------------------------+
| recover cluster `<cluster-id>`  | [v1.0.0-] Recover all inconsistent objects in the specified cluster                               |
+---------------------------------+---------------------------------------------------------------------------------------------------+


.. index::
    pair: Recover commands; recover-file-command

recover file <file-path>
^^^^^^^^^^^^^^^^^^^^^^^^^

Recover an inconsistent object specified by the file-path

::

  recover file leo/fast/storage.key
  OK

\


.. index::
    pair: Recover commands; recover-node-command

recover node <node>
^^^^^^^^^^^^^^^^^^^

Recover all inconsistent objects in the specified node

::

  recover node storage_0@127.0.0.1
  OK

\


.. index::
    pair: Recover commands; recover-ring-command

recover ring <node>
^^^^^^^^^^^^^^^^^^^

Recover rings of the specified node

::

  recover ring storage_0@127.0.0.1
  OK

\


.. index::
    pair: Recover commands; recover-cluster-command

recover cluster <cluster-id>
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Recover all inconsistent objects in the specified cluster

::

  recover cluster cluster-1
  OK
