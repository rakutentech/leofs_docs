LeoTamer
==========

LeoTamer is a admin tool for LeoFS on browsers.

Install
---------

Install LeoTamer
^^^^^^^^^^^^^^^^

ruby 1.9.3-p327 or higher is required.

::

  gem install bundler
  git clone https://github.com/leo-project/leo_tamer.git
  cd leo_tamer
  bundle install

Create Admin User
^^^^^^^^^^^^^^^^^

Only admin user of LeoFS can use LeoTamer now.
Therefore, you have to create admin user before use LeoTamer.

::

  $ telnet ${LEOFS_MANAGER_HOST} ${LEOFS_MANAGER_PORT}
  create-user new_admin password
    access-key-id: ab96d56258e0e9d3621a
    secret-access-key: 5c3d9c188d3e4c4372e414dbd325da86ecaa8068
  update-user-role new_admin 9 # set user role as admin
  OK

Config
^^^^^^^

Edit config.yml.

:: 

  :managers:
    - "localhost:10020" # leofs master manager host/port
    - "localhost:10021" # slave
  :credential:
    :access_key_id: ${YOUR_ACCESS_KEY_ID}
    :secret_access_key: ${YOUR_SECRET_ACCESS_KEY}

Start LeoTamer on Console
^^^^^^^^^^^^^^^^^^^^^^^^^^

::

  ruby config.ru ${PORT}

Features
---------

Node Status
^^^^^^^^^^^

.. image:: _static/screenshots/tamer/node_status.png

In 'Node Status' tab, you can see node status and execute operations resume/suspend/detach on storage nodes.
To execute operations, click 'Change Status' button.

Admin
^^^^^^^

Buckets
"""""""""

.. image:: _static/screenshots/tamer/buckets.png

Endpoints
""""""""""

.. image:: _static/screenshots/tamer/endpoints.png

Users
"""""""""

.. image:: _static/screenshots/tamer/users.png
