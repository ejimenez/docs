######
docker
######

Recetas sacadas de `rm-rf.es <http://rm-rf.es/como-instalar-configurar-usar-docker-linux-containers/>`_

Gestión de imágenes y contenedores
==================================

* Buscar imágenes:

.. code-block:: bash

    docker search NOMBRE_IMAGEN | more

* Descargar imagen:

.. code-block:: bash

    docker pull NOMBRE_IMAGEN

* Para visualizar todas las imágenes:

.. code-block:: bash

    docker images

* Crear y acceder al contenedor:

docker run IMAGEN COMANDO_INICIAL

.. code-block:: bash

    docker run -i -t --name NOMBRE_CONTENEDOR NOMBRE_IMAGEN /usr/bin/bash

* Parar el contenedor:

.. code-block:: bash

    docker stop ID_CONTENEDOR

* Iniciar contenedor parado:

.. code-block:: bash

    docker start -a -i ID_CONTENEDOR

* Borrar un contenedor:

.. code-block:: bash

    docker rm ID_CONTENEDOR

* Para saber que procesos se están ejecutando en el contenedor:

.. code-block:: bash
    
    docker top ID_CONTENEDOR

* Listado de contenedores:

.. code-block:: bash

    docker ps -l

* Comitear cambios de un contenedor:

.. code-block:: bash

    docker commit ID_CONTENEDOR NOMBRE_IMAGEN

* Si queremos aderirnos a la shell ya activa dentro del contenedor:

.. code-block:: bash

    docker attach ID_CONTENEDOR

* Ejecutar proceso en docker en nuevo terminal:

.. code-block:: bash

    docker exec -i -t ID_CONTENEDOR /bin/bash

* Obtener un registro de lo que se ha hecho en el contenedor:

.. code-block:: bash

    docker logs -f ID_CONTENEDOR


Compose
=======

* Comezar un servicio, ejecutar un comando en ese servicio y obtener el control de ese comando en la terminal actual:

.. code-block:: bash

    docker-compose run --service-ports <service> <comando>
    

* Ejemplo de ejecutar runserver de django (muy útil cuando necesitas usar un debugeador como ipdb):

.. code-block:: bash

    docker-compose run --service-ports webdjango bin/django runserver 0.0.0.0:8080

