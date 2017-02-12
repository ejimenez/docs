#######
Arduino
#######

Proyectos de arduino starter kit
================================

Información completa de los proyectos en `la página oficial <https://www.arduino.cc/en/Main/ArduinoStarterKit>`_

Proyecto 2: Nave espacial
~~~~~~~~~~~~~~~~~~~~~~~~~

Este proyecto consiste en configurar las clavijas 3,4 y 5 como salidas que encenderán tres leds y la clavija 2 como entrada que recogerá la señal del pulsador.

Por defecto estará encendido el led verde. Cuando se accione el pulsador, se apagará el led verde, se encenderán y apagaran los leds rojos y se volverá a encender el led verde.

.. raw:: html

    <div style="position:relative;height:0;padding-bottom:56.25%"><iframe src="https://www.youtube.com/embed/PBki-vbwMAI?ecver=2" width="640" height="360" frameborder="0" style="position:absolute;width:100%;height:100%;left:0" allowfullscreen></iframe></div>

El código:

.. literalinclude:: media/arduino/02_spaceship_interface.ino

Proyecto 3: Termómetro
~~~~~~~~~~~~~~~~~~~~~~

Este proyecto consiste en configurar como entrada la clavija analógica A0 que recogerá la señal de un sensor de temperatura y como salidas las clavijas digitales 2, 3 y 4 que encenderán tres leds.

Por defecto se configurará una temperatura base de 20. Cuando frotemos el sensor, la temperatura subirá encendiendo los leds.

El video:

.. raw:: html

    <div style="position:relative;height:0;padding-bottom:56.25%"><iframe src="https://www.youtube.com/embed/ISOyui-lcA8?ecver=2" width="640" height="360" frameborder="0" style="position:absolute;width:100%;height:100%;left:0" allowfullscreen></iframe></div>

El código:

.. literalinclude:: media/arduino/03_termometro.ino