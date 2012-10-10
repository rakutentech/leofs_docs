LeoFS Manager Client
=====================

LeoFS Manager Client is a Ruby library to communicate with LeoFS Manager.

Install
---------

::

  $ gem install leofs_manager_client

Sample Code
------------

.. code-block:: ruby

  require "leofs_manager_client"

  manager = LeoFSManager::Client.new("localhost:10020")

  # getting status of LeoFS Cluster
  status = manager.status #=> LeoFSManager::Status
  
  status.node_list.each do |node|
    node.node # node name
    node.state # "running", "stop", "suspended", "downed"
  end
