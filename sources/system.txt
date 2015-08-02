##############
System recipes
##############

rsync
=====

Sicronización con ancho de banda limitado, borrado en destino y reintentos automáticos en caso de fallo.

.. code-block:: bash

    while [ 1 ]
    do
        rsync -avz --delete –bwlimit=256 carpeta/* <usuario>@<maquina>:/carpeta
        if [ "$?" = "0" ] ; then
            echo "rsync completed normally"
            exit
        else
            echo "Rsync failure. Backing off and retrying..."
            sleep 180
        fi
    done