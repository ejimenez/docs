########
mysql
########

Crear y recupear bases de datos
===============================

.. code-block:: bash

    mysqldump <mysqldump options> | gzip > dbdump.sql.gz
    gunzip -c dbdump.sql.gz | mysql -u root -p dbname


Crear una base de datos
=======================

.. code-block:: bash

  CREATE DATABASE <nombre_db>;
  CREATE USER '<usuario>'@'localhost' IDENTIFIED BY '<password>';
  use <nombre_db>;
  GRANT ALL PRIVILEGES ON <usuario>.* TO '<usuario>'@'localhost';


Instalar mysql 5.7 en una red hat 7
===================================

.. code-block:: bash

  wget http://repo.mysql.com/mysql57-community-release-el7-7.noarch.rpm
  yum localinstall mysql57-community-release-el7-7.noarch.rpm # añadirmos repositorio de mysql
  sudo yum install mysql-server
  sudo systemctl enable mysqld.service
  sudo service mysqld start


Recuperar clave de root de mysql 5.7 en red hat 7
=================================================

.. code-block:: bash

  systemctl set-environment MYSQLD_OPTS="--skip-grant-tables" # iniciamos mysql saltando los permisos sobre tablas
  service mysqld restart
  mysql
  UPDATE mysql.user SET authentication_string=PASSWORD('<nuevaclaveroot>') WHERE User='root'; # preguntar que clave debe tener
  quit
  systemctl unset-environment MYSQLD_OPTS # Volvemos a iniciar mysql de forma normal
  service mysqld restart
  mysql -u root -p
  ALTER USER 'root'@'localhost' IDENTIFIED BY '<nuevaclaveroot>'; # es necesario establecer la clave con alter user para poder crear bases de datos y usuarios con el usuario root

Plugin de validación de password
==================================

Instalación:

.. code-block:: bash

  INSTALL PLUGIN validate_password;


Desinstalación:

.. code-block:: bash

  UNINSTALL PLUGIN validate_password;

Mostrar valores de la política de validación de password:

.. code-block:: bash

    SHOW VARIABLES LIKE 'validate_password%';

Modificación de las políticas (ejemplo de cambiar a grado medio):

.. code-block:: bash

    SET GLOBAL validate_password_policy=MEDIUM;


Permisos
========

Recargar las tablas de permisos (grant tables) en la caché:

.. code-block:: bash
    
    FLUSH PRIVILEGES;

