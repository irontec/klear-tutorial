Ficheros INI
============

En esta sección profundizaremos en los ficheros ini de la aplicación:

.. toctree::
   :maxdepth: 3

   application-ini
   klear-ini

.. attention::
   Recordar que para tener acceso o identificarse en la aplicación, hay que tener creada la :ref:`tablaklearUsers`. Además, la carpeta **cache** que debe
   estar en la carpeta **application** debe contar con todos los permisos; si se piensa usar la interfaz de Klear para subir archivos, debe estar creada la
   carpeta **storage** al mismo nivel de **application** con todos los permisos como se explica en :ref:`generatorDataBaseFile`.

   .. code-block:: console

       $ cd /home/user/workspace/NuevoProyecto/web
       $ mkdir application/cache storage
       $ chmod 777 application/cache
       $ chmod 777 storage