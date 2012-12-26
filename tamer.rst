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

Node Status
^^^^^^^^^^^

Node status view shows each node's status and
it enables admin users to execute resume/suspend/detach operations on a strage node (click 'Change Status' button)

.. image:: _static/screenshots/tamer/node_status.png
   :width: 720px

Admin
^^^^^^^

This tab provides tools for administrators to manage users, buckets and endpoints.

Users
"""""""""

Users view shows each user's id, role, access_key_id and when the user was created.
You can create new users and delete them.
'Update Role' button enables admin users to update user's role from general to admin or from admin to general.

.. image:: _static/screenshots/tamer/users.png
   :width: 720px

Buckets
"""""""""

Buckets view shows which buckets each users have and
you can add a bucket on this view.

.. image:: _static/screenshots/tamer/buckets.png
   :width: 720px

Endpoints
""""""""""

Endpoints view shows the list of endpoints.
You can also add and delete endpoints.

.. image:: _static/screenshots/tamer/endpoints.png
   :width: 720px
