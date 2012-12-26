LeoTamer
==========

**LeoTamer** is an administration tool for LeoFS on your browser. You can easily operate LeoFS.

Install
---------

Install LeoTamer
^^^^^^^^^^^^^^^^

* **Ruby 1.9.3-p362** or higher is required.
* Download URL: http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p362.tar.gz

::

  gem install bundler
  git clone https://github.com/leo-project/leo_tamer.git
  cd leo_tamer
  bundle install

Create Admin User
^^^^^^^^^^^^^^^^^

.. note:: Current LeoTamer, Only LeoFS's administrator can use it. You need to create an administrator.

* You need to create a user as ``admin`` on LeoFS-Manager's console.

::

  $ telnet ${LEOFS_MANAGER_HOST} ${LEOFS_MANAGER_PORT}
  create-user new_admin password
    access-key-id: ab96d56258e0e9d3621a
    secret-access-key: 5c3d9c188d3e4c4372e414dbd325da86ecaa8068

  update-user-role new_admin 9 # set user role as admin
  OK

Config
^^^^^^^

Update ``config.yml`` for connecting LeoFS-Manager

:: 

  :managers:
    - "localhost:10020" # leofs manager - master node's host/port
    - "localhost:10021" # leofs manager - slave node's host/port
  :credential:
    :access_key_id: ${YOUR_ACCESS_KEY_ID}
    :secret_access_key: ${YOUR_SECRET_ACCESS_KEY}


Start LeoTamer
^^^^^^^^^^^^^^

::

  ruby config.ru ${LEO-TAMER-PORT}

Features
---------

Node Status View
^^^^^^^^^^^^^^^^

* You can confirm each node-status
* Administrators can change ``storage-staus`` with ``Change Status button``
    * storage-status: [resume, suspend, detach]

.. image:: _static/screenshots/tamer/node_status.png
   :width: 720px

Administration-related views
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Users
"""""""""

* You can confirm the registered list of users
    * Able to create a user and delete it
    * Able to change a user role with ``Update Role button``

.. image:: _static/screenshots/tamer/users.png
   :width: 720px

Buckets
"""""""""

.. note:: A bucket as root directory belongs to a user

* You can show list of buckets

.. image:: _static/screenshots/tamer/buckets.png
   :width: 720px

Endpoints
""""""""""

* You can confirm the registered list of endpoints

.. image:: _static/screenshots/tamer/endpoints.png
   :width: 720px
