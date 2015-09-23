######
Python
######

Fechas
======

Formatos de fechas en distintos idiomas:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo locale-gen es_ES.UTF-8

.. code-block:: python

    >>> import time
    >>> print time.strftime("%a, %d %b %Y %H:%M:%S")
    Sun, 23 Oct 2005 20:38:56
    >>> import locale
    >>> locale.setlocale(locale.LC_TIME, "sv_SE") # swedish
    'sv_SE'
    >>> print time.strftime("%a, %d %b %Y %H:%M:%S")
    sön, 23 okt 2005 20:39:15

Fuente: http://stackoverflow.com/questions/985505/locale-date-formatting-in-python


