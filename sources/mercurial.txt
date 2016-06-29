#########
Mercurial
#########

 
Documentación Mercurial
=======================

* `Guía Rápida <http://mercurial.selenic.com/wiki/QuickStart>`_
* `Filosofía <http://mercurial.selenic.com/wiki/UnderstandingMercurial>`_
* `Tutorial <http://mercurial.selenic.com/wiki/Tutorial>`_
* `Guía definitiva <http://hgbook.red-bean.com/read/>`_


Recetas Mercurial
=================

Configuración
~~~~~~~~~~~~~

Configurar ignores de ficheros
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

'''A nivel de sistema''':

Creamos /etc/mercurial/.hgignore, con:

.. code-block:: bash

    # use glob syntax.
    syntax: glob 
    *.pyc
    *~
    *egg-info
    build/*
    buildout/downloads/*
    buildout/parts/*
    buildout/eggs/*
    buildout/var/*
    buildout/etc/*
    buildout/bin/*



Creamos archivo de configuración, /etc/mercurial/hgrc con:

.. code-block:: bash

    [ui]
    ignore= /etc/mercurial/.hgignore


Como hacer el equivalente a blame de svn (en el fichero anterior añadir lo siguiente):

.. code-block:: bash

    [alias]
    blame = annotate --user --number


'''A nivel de usuario:'''

Igual que el anterior pero creando el fichero .hgrc en el home del usuario.


'''A nivel de repositorio:'''

Creamos el fichero .hgignore con las reglas dentro del directorio root del repositorio.

Configurar nombre de usuario
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

En el fichero de configuración .hgrc:

.. code-block:: bash

    [ui]
    username = John Doe <john@example.com>

Extensiones útiles
^^^^^^^^^^^^^^^^^^

Incluir las siguientes extensiones en el .hgrc:

.. code-block:: bash

    [extensions]
    fetch =
    rebase =
    color =
    hgext.mq =
    hgk =
    pager =

    [pager]
    pager = LESS='FSRX' less

Configurar hooks PEP8, Pyflakes y PDB locales
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Instalar `hghooks <http://pypi.python.org/pypi/hghooks/0.3.1 hghooks>`_:

.. code-block:: bash

    $ sudo easy_install hghooks


 En Arch Linux se puede instalar desde AUR directamente:

.. code-block:: bash

    $ yaourt -Sy python-hghooks


* `Configurar hghooks <http://pypi.python.org/pypi/hghooks/0.3.1#usage>`_, añadiendo las siguientes líneas al fichero `$HOME/.hgrc`:

.. code-block:: bash

    [hooks]
    pretxncommit.pyflakes = python:hghooks.code.pyflakeshook
    pretxncommit.pep8 = python:hghooks.code.pep8hook
    pretxncommit.pdb = python:hghooks.code.pdbhook

    [pep8]
    ignore = E501


Crear un repositorio externo hg
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    echo nested = https://example.com/nested/repo/path > .hgsub
    hg add .hgsub
    hg clone https://example.com/nested/repo/path nested

Gestión de Ramas
================

Crear una nueva rama
~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    hg branch <branch_name>


Nota: todavía tienes que comitearla, ver más adelante.

Trabajar en una rama
~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    hg update <branch_name>


Comitear la rama
~~~~~~~~~~~~~~~~

Me sitúo en la rama:

.. code-block:: bash

    hg update <branch_name>

Comiteo la rama:

.. code-block:: bash

    hg push --new-branch

Nota: comitear una nueva rama solo se hace una vez.

Actualizar una rama
~~~~~~~~~~~~~~~~~~~

Para traerte los nuevos cambios a tu repositorio:

.. code-block:: bash

    hg pull
    hg update <branch_name>

Enviar cambios al servidor:

.. code-block:: bash

    hg push

Cerrar una rama
~~~~~~~~~~~~~~~

.. code-block:: bash

    hg commit -m "Cerramos la rama <branch_name>" --close-branch

Mezcla de una rama con otra
~~~~~~~~~~~~~~~~~~~~~~~~~~~

En el siguiente ejemplo mezclamos los cambios de <branch_name> a la rama default:

.. code-block:: bash

    hg update default
    hg merge <branch_name>  # aqui puede haber conflictos
    hg commit -m "Mezclamos <branch_name> en la rama de desarrollo"
    hg push

Trasplantar un commit de una rama a otra
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Necesitas tener activa la extensión transplant:

.. code-block:: bash

    [extensions]
    transplant=


Un ejemplo de uso sería:

.. code-block:: bash

    $ hg transplant 40:43 56

Acciones comunes
================

Recuperar fichero versionado borrado localmente
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    hg revert <nombre-fichero>


Resolver conflictos manualmente 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Después de resolver los conflictos manualmente hay que marcarlos como resueltos:

.. code-block:: bash

    hg resolve -m <nombre_fichero>


Revertir commits locales, sin revertir los cambios en los ficheros
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Imagina que has hecho un commit local, pero se te ha olvidado pasar el pep8. Los pasos a seguir son:

.. code-block:: bash

    hg log | less # recupera el mensaje del último commit
    hg rollback # revierte el commit
    # pasale el pep8 al fichero
    hg commmit # vuelve a comitearlo, pegando el mensaje copiado en el primer punto
    hg push # envía los cambios al servidor


Revertir cambios locales, OJO revierte los cambios en los ficheros locales (como si no hubieras hecho nada)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Última opción para cuando se quiere revertir algo y no sabes muy bien como hacerlo (guarda los diff antes)

.. code-block:: bash

    hg diff  "ruta cambios a guardar" > cambios.diff
    hg update -C


Deshacer un revert de cambios locales no comiteados
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Supongamos que tenemos cambios y hacemos `hg revert hello.c`, para recuperar los cambios hacemos lo siguiente:

.. code-block:: bash

    rm hello.c
    mv hello.c.orig hello.c

Alteración de commits locales porque el push al servidor falla
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Supongamos que hacemos dos commits, y después, al hacer el push, falla en el servidor porque el primero de los dos commits es erróneo (no menciona ticket abierto, falla el pyflakes, etc.).

La solución pasa por hacer uso de las colas de la extensión mq, que hay que instalarse en local, añadiendo la siguiente línea en el `$HOME/.hgrc`:

.. code-block:: bash

    [extensions]
    ...
    hgext.mq =



Veámos el log para hacernos una idea de nuestro fallo de prueba:

.. code-block:: bash

    $ hg log -l 2

    changeset:   140:8ff7e5eac70f
    tag:         tip
    user:        msaelices
    date:        Thu Nov 04 18:13:20 2010 +0100
    summary:     Otro commit de prueba normal. Ver #1.

    changeset:   139:9ce891ce41fe
    user:        msaelices
    date:        Thu Nov 04 18:12:34 2010 +0100
    summary:     Commit de prueba para que deberia hacer saltar pyflakes. Ver #1.


Lo que queremos hacer es lo siguiente:
* Crear una cola de parches, incluyendo tanto los cambios 140 y 139 en ella.
* Desencolar el parche del cambio 139.
* Cambiar a mano el mensaje de commit, añadiendo un `no-pyflakes` detrás para permitir el push.
* Re-encolar el parche y finalizar el tratamiento de las colas.
* Hacer push.

Esto se hace con la siguiente secuencia:

.. code-block:: bash

    $ hg qinit    <--- si nunca has usado colas de parches
    $ hg qimport -r140 -r139    <--- encolamos los dos changesets, no se puede encolar el 139 sin encolar el siguiente
    $ hg log -l 2

    changeset:   140:8ff7e5eac70f
    tag:         140.diff
    tag:         qtip         <--- inicio de la cola
    tag:         tip
    user:        msaelices
    date:        Thu Nov 04 18:13:20 2010 +0100
    summary:     Otro commit de prueba normal. Ver #1.

    changeset:   139:9ce891ce41fe
    tag:         139.diff
    tag:         qbase        <--- fin de la cola
    user:        msaelices
    date:        Thu Nov 04 18:12:34 2010 +0100
    summary:     Commit de prueba para que deberia hacer saltar pyflakes. Ver #1.

    $ hg qpop
    popping 140.diff
    now at: 139.diff
    $ hg qpop
    popping 139.diff
    patch queue now empty
    $ vim .hg/patches/139.diff  <--- editamos el mensaje de commit y le añadimos "no-pyflakes"
    $ hg qpush
    applying 139.diff
    now at: 139.diff
    $ hg qpush
    applying 140.diff
    now at: 140.diff
    $ hg qfinish -a
    $ hg log -l 2

...
    changeset:   139:dfd682600978
    user:        msaelices
    date:        Thu Nov 04 18:12:34 2010 +0100
    summary:     Commit de prueba para que deberia hacer saltar pyflakes. Ver #1. no-pyflakes.  <--- mensaje de commit cambiado :-)

    $ hg push

Extensión opcional: histedit para alterar los mensajes en commits locales
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 * Nombre extensión: histedit
 * Repositorio: https://bitbucket.org/durin42/histedit

Descarga y configuración
^^^^^^^^^^^^^^^^^^^^^^^^
* En la shell ejecutamos:

.. code-block:: bash

    hg clone https://bitbucket.org/durin42/histedit

* Abrir nuestro `.hgrc` y en la sección `[extensions]` añadimos lo siguiente:

.. code-block:: bash

    histedit = /path/to/histedit/hg_histedit.py

Ejemplo de uso
^^^^^^^^^^^^^^
* Suponiendo que el mensaje de commit que queremos cambiar es '''139''', ejecutamos:

.. code-block:: bash

    $ hg histedit -r139


* Aparecerá una pantalla con los commits realizados. Solo tenemos que saber qué mensaje queremos cambiar y editar la linea cambiando 'pick' por 'edit' y salvar los cambios.
* Tras esto ejecutamos:

.. code-block:: bash

    $ hg histedit --continue

* Aparecerá el mensaje de nuestro commit listo para modificar.
* Mergear cambios con `hg merge`.

'''Enlaces relacionados:'''
 * http://mercurial.selenic.com/wiki/HisteditExtension
 * http://knowledgestockpile.blogspot.com/2010/12/changing-commit-message-of-revision-in.html

Mezcla de los catálogos de traducción .po
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Para resolver los conflictos de forma eficiente en los archivos de traducción (catálogos .po) se puede instalar y configurar la herramienta `pomerge`. Que se instala con el paquete debian `translate-toolkit`

.. code-block:: bash

    [merge-tools]
    pomerge.priority = 60
    pomerge.premerge = False
    pomerge.args = $local $other $output
    pomerge.executable = pomerge

    [merge-patterns]
    **.po = pomerge
    **.mo = internal:fail

Otros
=====

*  `Equivalencia de comandos entre git y mercurial <http://mercurial.selenic.com/wiki/GitConcepts#Command_equivalence_table>`_