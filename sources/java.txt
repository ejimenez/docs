####
Java
####

Relación RPC, EJB y RMI
=======================

 * RPC (remote procedure call) Protocolo que permite crear aplicaciones cliente/servidor basadas en la llamada a procedimientos remotos.
 * RMI (remote method invocation) Tecnología que permite hacer llamadas remotas entre objetos de diferentes máquinas virtuales de Java.
 * EJB (enterprise Java beans) Tecnología de componentes distribuidos de la empresa Sun que forma parte de la edición empresarial de Java.

RMI es una tecnología de objetos distribuidos que es una evolución de RPC, ya que RPC realiza llamadas a funciones remotas, mientras que RMI realiza llamadas a métodos de objetos remotos.

EJB utiliza internamente el protocolo RMI para realizar las comunicaciones.

Capa de presentación
====================
Una forma es usar el framework Java Server Faces, donde el controlador será el servlet FacesServlet, las acciones estarán definidas con ManagedBean y las vistas se implementarán mediante Facelets.


Capa de negocio
===============
Definir un EJB de sesión sin estado con acceso remoto que sigue el patrón fachada

Como el acceso puede ser tanto local como remoto, el EJB implementa dos interfaces para estos dos
tipos de accesos.

Capa de información
===================

Java Persistence API (JPA), que nos permitirá gestionar la persistencia y sus relaciones, tiene todas las ventajas de un framework de mapeo y además es un estándar.

Cambiar java del sistema
========================

Te da a elegir entre los que tienes instalados:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo update-alternatives --config java

Cambiar a una versión determinada:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo update-java-alternatives -s java-7-oracle


Otros recursos:
~~~~~~~~~~~~~~~

* http://www.ubuntu-guia.com/2012/04/instalar-oracle-java-7-en-ubuntu-1204.html
* http://blog.jfexart.com/2012/02/seleccionar-que-version-de-java-usar.html
* http://es.wikihow.com/instalar-Java-en-Linux
