
Getting Your S3 Key
---------------------

You can get S3 API Keys from ``LeoFS' Manager Console``.

::

  $ telnet 127.0.0.1 10010
  Trying 127.0.0.1...
  Connected to 127.0.0.1.
  Escape character is '^]'.

The 'create-user ${USER-ID}' command generates your s3 key.

::

  create-user ${USER-ID}
  access-key-id: 05dcba94333c7590a635
  secret-access-key: c776574f3661579ceb91aa8788dfcac733b21b3a

