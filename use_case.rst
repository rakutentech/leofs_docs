Use Case
========

.. index::
   pair: Configuration; LeoFS's Use Case - Without load-balancer

Without load-balancer for TEST
------------------------------

.. image:: _static/images/leofs-usecase-without-lb.png
   :width: 700px

* Example:
    * Endpoint: ``yourhost.com``
    * bucket: ``test``
* Client(s)
    * You need to edit ``/etc/hosts`` in your client machine(s)
    * You can use any clients as the follows:
        * :ref:`Program Lang's clients <aws-sdk-ruby-label>`
        * :ref:`s3fs-c <s3fs-c-label>`
        * :ref:`s3cmd <s3cmd-label>`
        * :ref:`DragonDisk <dragondisk-label>`
* Each server's configuration (only `ip` and `name`)
    * :ref:`Manager <conf_manager_label>`
        * IP: 192.168.1.7, 192.168.1.8
        * Name: manager_0@192.168.1.7, manager_1@192.168.1.8
    * :ref:`Gateway <conf_gateway_label>`
        * IP: 192.168.1.3
        * Name: gateway_0@192.168.1.3
    * :ref:`Storage <conf_storage_label>`
        * IP: 192.168.1.4 .. 192.168.1.6
        * Name: storage_0@192.168.1.4 .. storage_2@192.168.1.6
* Execute commands on LeoFS-Manager's console for the authentication
    * Set an endpoint - ``yourhost.com``
    * Create a user   - ``yourname``

::

    $ telnet 127.0.0.1 10010
    > set-endpoint yourhost.com
    OK
    > create-user yourname
    access-key-id: 05dcba94333c7590a635
    secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a

\

With load-balancer for Production/Staging
-----------------------------------------

(under construnction)
