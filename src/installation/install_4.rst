.. =========================================================
.. LeoFS documentation
.. Copyright (c) 2012-2014 Rakuten, Inc.
.. http://leo-project.net/
.. =========================================================

.. index::
   pair: Firewall Rules; Installation

Firewall Rules
--------------

In order for LeoFS to work correctly, it is necessary to set and check the firewall rules in your environment as follows:

+----------------------+-----------+-----------------+--------------------------+
| Subsystem            | Direction | Ports           | Notes                    |
+======================+===========+=================+==========================+
| LeoFS Manager-Master | Incoming  | 10010/*         | Manager console          |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Master | Incoming  | 4369/*          | Erlang RPC from others   |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Master | Incoming  | 4020/*          | SNMP Listen Port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Master | Outgoing  | \*/4369         | Erlang RPC to others     |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Slave  | Incoming  | 10011/*         | Manager console          |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Slave  | Incoming  | 4369/*          | Erlang RPC from others   |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Slave  | Incoming  | 4021/*          | SNMP Listen Port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Manager-Slave  | Outgoing  | \*/4369         | Erlang RPC to others     |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Storage        | Incoming  | 4369/*          | Erlang RPC from others   |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Storage        | Incoming  | 4010/*          | SNMP Listen Port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Storage        | Outgoing  | \*/4369         | Erlang RPC to others     |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Incoming  | 8080/*          | HTTP listen port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Incoming  | 8443/*          | HTTPS listen port        |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Incoming  | 4369/*          | Erlang RPC from others   |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Incoming  | 4000/*          | SNMP Listen Port         |
+----------------------+-----------+-----------------+--------------------------+
| LeoFS Gateway        | Outgoing  | \*/4369         | Erlang RPC to others     |
+----------------------+-----------+-----------------+--------------------------+

.. raw:: html

    <div style="margin-bottom:80px;">&nbsp;</div>
