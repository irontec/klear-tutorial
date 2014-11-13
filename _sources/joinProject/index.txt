.. _joinProject:

Unirse a un proyecto ya creado
==============================


.. contents::
   :local:
   :depth: 3

Crear un proyecto en Eclipse
----------------------------

Para crear un proyecto nuevo en Ecplise sigue  :ref:`estos pasos. <createEclipseProjectLink>`.

Obtener el proyecto
-------------------

El proyecto se obtendrá de diferente manera en función del sistema de control de versiones que se utilice.


Bases de Datos
--------------

Crear la Base de Datos
++++++++++++++++++++++

.. code-block:: console

   $ mysql -uroot -ppass -e "CREATE DATABASE tuBD"

.. attention::

   Configura los datos de tu base de datos en los archivos:
   /home/irontec/workspace/NuevoProyecto/web/application/configs/application.ini:

   .. code-block:: ini

      [development : production]
      ...
      resources.db.adapter = "MYSQLI"
      resources.db.params.dbname = "tuBD"
      resources.db.params.username = "root"
      resources.db.params.password = "pass"
      resources.db.params.host = "localhost"

   /home/irontec/workspace/NuevoProyecto/phing/build.properties

   .. code-block:: ini

      # Credentials for the database migrations
      db.host=localhost
      db.user=root
      db.pass=pass
      db.name=tuBD

Phing
+++++

Ejecutamos los siguientes comandos:

.. code-block:: console

   $ cd /home/irontec/workspace/NuevoProyecto/phing/
   $ phing init
   $ phing migrate

Generadores
+++++++++++

Ejecutamos los generadores según :ref:`estas instrucciones <generadoresDBLink>`.

