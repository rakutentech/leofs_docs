
.. _erlcloud-label:

Getting Started with Erlang
---------------------------

Getting "erlcloud"
^^^^^^^^^^^^^^^^^^

* ``erlcloud`` is a Erlang interface to Amazon Web Services. You can use it for LeoFS too.
    * `Repository <https://github.com/gleber/erlcloud>`_

Edit /etc/hosts
^^^^^^^^^^^^^^^

.. note:: LeoFS domains are ruled by :ref:`this rule <s3-path-label>`.

::

  127.0.0.1 s3.amazonaws.com
  127.0.0.1 ${bucket_name}.s3.amazonaws.com # if you use create_bucket

Example usage
^^^^^^^^^^^^^

.. code-block:: erlang

  %% Launch "erlcloud" and configuration
  erlcloud:start(),
  Conf_1 = erlcloud_s3:new("?YOUR_ACCESS_KEY_ID",
                         "?YOUR_SECRET_ACCESS_KEY",
                         "localhost",
                         8080),
  Conf_2 = Conf_1#aws_config{s3_scheme = "http://"},

  Bucket = "erlang",
  Key = "test-key",
  Val = "value",

  try
      %% Create an bucket
      erlcloud_s3:create_bucket(Bucket, Conf_2),

      %% Retrieve list of buckets
      List = erlcloud_s3:list_buckets(Conf_2),
      io:format("[debug]buckets:~p~n", [List]),

      %% Put an object
      erlcloud_s3:put_object(Bucket, Key, Val, [], Conf_2),

      %% Retrieve list of objects
      Objs = erlcloud_s3:list_objects(Bucket, Conf_2),
      io:format("[debug]objects:~p~n", [Objs]),

      %% Retrieve an object
      Obj = erlcloud_s3:get_object(Bucket, Key, Conf_2),
      io:format("[debug]inserted object:~p~n", [Obj]),

      %% Retrieve metadata of an object
      Meta = erlcloud_s3:get_object_metadata(Bucket, Key, Conf_2),
      io:format("[debug]metadata:~p~n", [Meta]),

      %% Remove an object
      DeletedObj = erlcloud_s3:delete_object(Bucket, Key, Conf_2),
      io:format("[debug]deleted object:~p~n", [DeletedObj]),

      try
          %% Retrieve an object, again
          NotFoundObj = erlcloud_s3:get_object(Bucket, Key, Conf_2),
          io:format("[debug]not found object:~p~n", [NotFoundObj])
      catch
          error:{aws_error,{http_error,404,_,_}} ->
              io:format("[debug]404 not found object~n")
      end
  after
      ok = erlcloud_s3:delete_bucket(Bucket, Conf_2)
  end.

