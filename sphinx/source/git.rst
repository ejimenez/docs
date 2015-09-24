###
Git
###

Todas estas recetas están sacadas de `progit <http://git-scm.com/book/es>`_


Conceptos básicos
=================

* Git no guarda solo cambios, hace instantáneas de los ficheros en un momento dado (cuando se hace un commit)
* Las ramas en git son punteros a un commit determinado.
* HEAD es un puntero a la rama en la que actualmente te encuentras.
* origin: repositorio remoto original desde el que si hizo clone.
* master: es la rama local por defecto en los repositorios git.

Operaciones básicas
===================

* Cuando añades un fichero y luego quieres revertirlo sin borrarlo de tu directorio:

.. code-block:: bash

    git add fichero
    git rm --cached fichero

* Cuando has comiteado algo y se te ha olvidado comitear un archivo o quieres cambiar el mensaje de confirmación:

.. code-block:: bash

    $ git commit -m 'initial commit'
    $ git add forgotten_file
    $ git commit --amend

La última linea sobrescribe el commit anterior.


* Deshacer los cambios preparados de un fichero:

.. code-block:: bash

    $ git reset HEAD file

* Deshacer los cambios no preparados de un fichero:

.. code-block:: bash

    git checkout -- file

Repositorios Remotos
====================

* Añadir un repositorio remoto:

.. code-block:: bash

    git remote add pb git://github.com/paulboone/ticgit.git


* Recibir datos de repositorios remotos (actualiza las referencias remotas):

.. code-block:: bash
    
    $ git fetch [remote-name]


* Recibir y unir de un repositorio remoto. Hace fetch y además merge de la rama remota a la rama local, esto se hace auntomáticamente cuando la rama local se ha trackeado a la remota:

.. code-block:: bash

    git pull


* Enviar tus cambios a un servidor remoto:

.. code-block:: bash

    git push [nombre-remoto][nombre-rama]
    $ git push origin master


Etiquetas
=========

* Crear una etiqueta:

.. code-block:: bash
    
    $ git tag -a v1.4 -m 'my version 1.4'

* Crear etiqueta de una versión anterior:

.. code-block:: bash

    $ git tag -a v1.2 <suma de comprobación>

* Compartir etiquetas en otro repositorio:

.. code-block:: bash
    
    git push origin [tagname]

* si quieres compartirlas todas:

.. code-block:: bash
    
    git push origin [tagname]

Ramas
=====

* Crear una nueva rama

.. code-block:: bash
    
    $ git branch <branch_name>

* Cambiar a una rama:

.. code-block:: bash

    $ git checkout <branch_name>

* Crear una rama y cambiar automaticamente:

.. code-block:: bash
    
    $ git checkout -b <branch_name>

* Mezclar dos ramas (master y hotfix):

.. code-block:: bash

    git checkout master
    git merge hotfix

* Borrar una rama

.. code-block:: bash
    
    $ git branch -d hotfix

* Listar todas las ramas:

.. code-block:: bash
    
    git branch

* Ver los últimos cambios de todas las ramas:

.. code-block:: bash
    
    git branch -v

* Ramas que hay mergeadas

.. code-block:: bash
    
    git branch --merged

* Ramas que contienen trabajo pendiente de mergear:

.. code-block:: bash

    git branch --no-merged

* Borrar una rama que contiene commits no mergeados:

.. code-block:: bash

    git branch -D <branch_name>

* Sincronizar tu trabajo con la rama remota master del servidor original:

.. code-block:: bash
    
    git fetch origin

* Publicar una rama local propia (ej serverfix), para que otros puedan trabajar en ella:

.. code-block:: bash
    
    git push origin serverfix

* Pushear una rama local (serverfix) a una rama remota con nombre distinto (awesomebranch):

.. code-block:: bash
    
    git push origin serverfix:awesomebranch

* Hacer traking branch para que los push y los pull vayan automáticamente a una rama remota determinada

.. code-block:: bash

    git checkout -b [branch] [remotename]/[branch]

Version git 1.6.2 o superior tiene un alias:

.. code-block:: bash
    
    git checkout --track origin/serverfix

Hacer que tu rama local "sf" haga push y pull automáticamente a origin/serverfix

.. code-block:: bash

    git checkout -b sf origin/serverfix

Borrar una rama remota "serverfix":

.. code-block:: bash
    
    git push origin :serverfix

Ver información sobre ramas locales trackeadas a ramas remotas:

.. code-block:: bash

    git remote show origin

Pendiente de completar
======================

A partir de http://progit.org/book/es/ch3-6.html