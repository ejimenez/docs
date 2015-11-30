==============================================
Instrucciones uso y despliegue del repositorio
==============================================

Para poder tener un repositorio de documentos sphinx publicado con github pages he seguido las instrucciones de `sphinxdoc-test <http://daler.github.io/sphinxdoc-test/includeme.html>`_

Motar sistema de directorios
============================

Descargar repositorio sphinx
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    git clone git@github.com:ejimenez/docs.git


Descargar repositorio sphinx (rama html)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    cd docs
    mkdir sphinx_html
    cd sphinx_html
    git clone git@github.com:ejimenez/docs.git html
    cd html
    git checkout gh-pages

Generar html
~~~~~~~~~~~~

.. code-block:: bash

    cd docs/sphinx
    make html


Publicar html
~~~~~~~~~~~~~

.. code-block:: bash

    cd docs/sphinx
    make publish

