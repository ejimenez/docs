########
postgres
########

Create and restore a compresed backup
=====================================

.. code-block:: bash

    pg_dump dbname | gzip > filename.gz
    gunzip -c filename.gz | psql dbname


