########
Sistemas
########

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

Procesos
========

Dejar proceso en una shell remota con disown
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Una vez hayamos ejecutado el comando realizar los siguientes pasos:

.. code-block:: bash

    control + z
    bg 1 # suponiendo que tenemos el trabajo en %1
    disown -h %1

Certificados
============

Obtener información (entre ellos el alias) de un certificado:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    keytool --list -v -keystore almacendeclaves.p12 -storetype PKCS12

Comando find
============

Listado de fichero modificados hace menos de un minuto terminados en .log
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    find . -type f -cmin 1 -name "*.log"

Comando ssh
===========

Acceso ssh sin contraseña
~~~~~~~~~~~~~~~~~~~~~~~~~

Generar clave pública (sin contraseña):

.. code-block:: bash
    
    ssh-keygen -t rsa

Copias la clave pública al servidor donde queremos conectar:

.. code-block:: bash
    
    ssh-copy-id -i ~/.ssh/id_rsa.pub usuario@servidor

Para Entrar en el portal beta de uma al servidor

.. code-block:: bash
    
    ssh usuario@servidor

Usuarios
========

Crear usuario
~~~~~~~~~~~~~

.. code-block:: bash

    useradd -m -s /bin/bash -d /home/<username> <username>

Añadir a sudo:
sudo usermod -aG sudo <username>

Añadir usuario a grupo sudo
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo usermod -aG sudo <username>

Seguridad
=========

Generar clave
~~~~~~~~~~~~~

.. code-block:: bash

    date | md5sum

Encriptar un fichero
~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    gpg -c filename.txt 

Desencriptar un fichero
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    gpg filename.txt.gpg

Comando sshfs
=============

Montar carpeta remota
~~~~~~~~~~~~~~~~~~~~~

Recursos:

* https://www.digitalocean.com/community/tutorials/how-to-use-sshfs-to-mount-remote-file-systems-over-ssh
* http://albertomoralescruz.blogspot.com.es/2011/05/montando-unidades-mediante-sshfs-en-el.html

Ficheros
========

Aplicar parches
~~~~~~~~~~~~~~~
Dentro del directorio donde estan contenidos los ficheros a parchear:

.. code-block:: bash

    patch -p1 < parche.patch






