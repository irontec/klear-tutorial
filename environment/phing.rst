Phing
=====

**PH**\ing  **I**\s **N**\ot **G**\NU make: PHP5 project build system based on `Apache Ant <http://ant.apache.org/>`_

En terminos normales, se puede decir que es un software que nos permite automatizar tareas comunes.

Instalación
-----------

Primero hay que registrar el canal que necesitamos para 
instalar **PHING** a través de **PEAR**\. Desde la terminal:

.. code-block:: console

   $ pear channel-discover pear.phing.info

Con el comando anterior el canal queda registrado. Ahora hay que buscar el paquete que
queremos instalar, para ello pediremos la lista de coincidencias para la palabra "phing".
prev
Desde la terminal:

.. code-block:: console

   $ pear remote-list -c phing
   //Output del comando
   Channel phing Available packages:
   =================================
   Package   Version
   phing     2.7.0
   phingdocs 2.7.0

Con esto ya sabemos el nombre del paquete y la versión que debemos instalar. 
Para terminar solo falta instalar el paquete.

Desde la terminal:

.. code-block:: console

   $ pear install phing/phing-2.7.0

.. attention::

   Si no funciona nos bajaremos el paquete ejecutable de la página oficial de Phing:

   .. code-block:: console

      # wget http://www.phing.info/get/phing-latest.phar -P /usr/local/bin/
      # chmod +x /usr/local/bin/phing-latest.phar
      # ln -s /usr/local/bin/phing-latest.phar /usr/local/bin/phing

