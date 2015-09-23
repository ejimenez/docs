######
Screen
######

Dejar un comando ejecutándose en una shell remota
=================================================

Al ejecutar screen por primera vez, se nos abre una shell. Ejecutamos el comando que queramos dejar andando hasta que termine. Y pulsamos Ctrl + A + D. Con eso, nos "desconectamos" de esa shell, ya podemos cerrar la ventana con toda la tranquilidad del mundo. Nuestro comando sigue ejecutandose en su propia shell. Posteriormente, en cualquier momento, podemos "conectarnos" a esa shell para ver como va nuestro comando, para ello ejecutamos screen -r

Más utilidades:
===============

Algunas cosas más sobre screen:

* Ayuda: Ctrl + a + ?
* Abre una ventana nueva: Ctrl + a + c
* Cambia el título de una ventana: Ctrl + a + shift + a
* Salir de una ventana: Ctrl + d
* Lista las ventanas en la sesión: Ctrl + a + "
* Cambiar de una ventana a otra: Ctrl + a + a 
* Salir sin destruir CTRL + a d.
* Salir destruyendo la sesión: CTRL + a k
* Listar sesiones: screen -ls


Compatir una sesíón de screen con otro usuario:
===============================================

Arracar la sesión
~~~~~~~~~~~~~~~~~

.. code-block:: bash

    screen -d -m -S mySharedSession

Otro usuario puede interactuar en la sesión abierta:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    screen -x mySharedSession



