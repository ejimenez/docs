########
postgres
########

Create and restore a compresed backup
=====================================

.. code-block:: bash

    pg_dump dbname | gzip > filename.gz
    gunzip -c filename.gz | psql dbname


Change table owner
==================

.. code-block:: bash

    ALTER TABLE tablax OWNER TO usuario_nuevo;

Delete cascade
==============

.. code-block:: bash
    
    TRUNCATE table CASCADE;


