##########
Subversion
##########


Lo básico
=========

La documentación es muy extensa pero normalmente usas **sólo 6 cosas**:

* svn co <path_proyecto> # te bajas el proyecto
* svn up # dentro del directorio del proyecto, lo actualice con los cambios de tus compañeros
* svn add <fichero> añade un nuevo fichero al repositorio
* svn mv # para mover archivos o directorio
* svn rm # para borrar archivos del repositorio 
* svn ci # dentro del directorio del proyecto, sube todos los cambios que has realizado en el proyecto al servidor central de código. Aplica tambien los archivos añadidos, borrados y movidos.


Referencias:

* **Referencia de los comandos completa:** http://svnbook.red-bean.com/nightly/es/svn-ch-9.html 
* **Libro subversión español**: http://svnbook.red-bean.com/nightly/es/svn-book.html


Acciones comunes
================

¿Cómo comparar un fichero local (con sus modificaciones) con la última versión en el repositorio?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    svn diff --revision HEAD


¿Como deshacer cambios hechos en el servidor?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    svn merge -r<numero_version_mala_actual>:<numero_version_buena_antigua> <archivo o directorio>
    svn ci <archivo o directorio>


Cómo crear una tag
~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ svn copy http://svn.example.com/repos/calc/trunk \
           http://svn.example.com/repos/calc/tags/release-1.0 \
      -m "Tagging the 1.0 release of the 'calc' project."

    Committed revision 351.


Ramas
=====

Como crear una rama
-------------------

Supongamos que queremos hacer una rama del proyecto calc.

.. code-block:: bash

    $ svn copy http://svn.example.com/repos/calc/trunk \
           http://svn.example.com/repos/calc/branches/my-calc-branch \
      -m "Creating a private branch of /calc/trunk."



Gestión de ramas
----------------

Tenemos dos alternativas:

* Usando sólo subversion.
* Subversion + Herramienta de gestión de ramas `svnmerge <http://www.orcaware.com/svn/wiki/Svnmerge.py/>`_. **Muy recomendado**. 

Ventajas svnmerge:

* Mantiene un registro de cambios fusionados.
* Evita que se aplique dos veces el mismo cambio.
* No tienes que buscar el último cambio fusionado para saber a partir de cual seguir.
* Permite definir que conjunto de cambios que no quieres fusionar y lo recueda en futuros merges.
* Genera mesajes de commits con un log de todas las cosas que se van a fusionar.

Ejemplos de fusionado
---------------------

Datos Supuestos:

* trunk: http://svn.example.com/repos/calc/trunk/
* rama: http://svn.example.com/repos/calc/branches/my-calc-branch
* Las copias locales donde se va a aplicar los cambios están actualizadas.

Recomendaciones:

* Si tienes externals que después de una fusión va a dejar de serlo porque se han congelado, mejor borralas en donde son externals para evitar conflictos
* Cuidado con el archivo svnmerge-commit-message.txt que genera svnmerge, ya que contiene texto "Ver #..", "Cierra #.." y si no lo cambias te mete comentarios en los tickets.

Fusionar cambios de la rama principal (trunk) a tu rama 
-------------------------------------------------------

Supongamos que estamos dentro del directorio de la rama "my-calc-branch"

Con ayuda de svnmerge:
~~~~~~~~~~~~~~~~~~~~~~

1. Inicializamos registro de cambios en la rama. Sólo se hace una vez después de crearla.

.. code-block:: bash

    $ svnmerge.py init


2. Podemos revisar que cambios serán fusionados

.. code-block:: bash

    $ svnmerge.py avail               # show only the revision numbers
    $ svnmerge.py avail --log         # show logs of the revisions on the trunk available for merging
    $ svnmerge.py avail --diff        # show diffs of the revisions on the trunk available for merging



3. Fusionamos los cambios de trunk en la rama

Todos los cambios:

.. code-block:: bash

    $ svnmerge.py merge


o un subconjunto:

.. code-block:: bash

    $ svnmerge.py merge -r4500,4671,4812


4. Subimos los cambios usando un archivo generado por svnmerge con el log de los mismos (borrando las últimas líneas, que podrían dar ruido en el trac):

.. code-block:: bash

    svn ci -F svnmerge-commit-message.txt



Si se quiere se puede bloquear algunos cambios para que no se fusionen:

.. code-block:: bash

    $ svnmerge.py block -r6456-6460,6881



A mano sólo con subversion:
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sintaxis:

.. code-block:: bash

    $ svn merge -r <rango_de_cambios> <url_trunk> <directorio_donde_aplicar>


1. Simulación del merge para ver que ficheros cambiarían:

.. code-block:: bash

    $ svn merge --dry-run -r 343:344 http://svn.example.com/repos/calc/trunk my-calc-branch


2. Haciendo merge real:

.. code-block:: bash

    $ svn merge -r 343:344 http://svn.example.com/repos/calc/trunk my-calc-branch


3. Subiendo los cambios:

.. code-block:: bash

    svn commit -m "portado cambio r344  desde trunk."


**Nota:** como subversion no tiene histórico de fusiones se recomienda indicarlo en el mensaje.

Fusionar la rama con el trunk
-----------------------------

Supongamos que estamos dentro del directorio del trunk

Con ayuda de svnmerge:
~~~~~~~~~~~~~~~~~~~~~~

1. Inicializamos registro de cambios en trunk relacionados con la rama BRANCH_URL. Sólo se ejecuta una vez.

.. code-block:: bash

    $ svnmerge.py init BRANCH_URL 


2. Aplicamos los cambios de la rama al trunk.

.. code-block:: bash

    $ svnmerge.py merge --bidirectional

3. Para cerrar la rama si no se va a seguir desarrollando en ella:

.. code-block:: bash

    $ svnmerge.py uninit -S BRANCH_URL


A mano sólo con subversion:
~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Encontrar la versión en la que se creo la rama con --stop-on-copy

.. code-block:: bash

    svn log --verbose --stop-on-copy  http://svn.example.com/repos/calc/branches/my-calc-branch


Ejemplo de salida:

.. code-block:: bash

    …
    ------------------------------------------------------------------------
    r341 | usuario | 2002-11-03 15:27:56 -0600 (Thu, 07 Nov 2002) | 2 lines
    Changed paths:
       A /calc/branches/my-calc-branch (from /calc/trunk:340)


Vemos que se creó en la revisón 341

2. Nos dirigimos al trunk y actualizamos:

.. code-block:: bash

    $ cd calc/trunk
    $ svn update
    At revision 405.


3. Aplicamos cambios de nuestra rama al trunk

.. code-block:: bash

    $ svn merge -r 341:HEAD http://svn.example.com/repos/calc/branches/my-calc-branch


4. Comprobamos los cambios

.. code-block:: bash

    $ svn status


5. Examinamos diffs, ejecutamos test, etc ...

6. Comiteamos los cambios con un mensaje indicando que es una fusión de una rama:

.. code-block:: bash

    $ svn commit -m "Merged my-calc-branch changes r341:405 into the trunk."


**Nota:** es muy importante especificar en el mensaje del cambio que es una fusión, para que en futuras fusiones sepamos desde que changeset debemos comenzar.

