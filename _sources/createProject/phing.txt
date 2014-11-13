Phing
=====

Phing requiere de dos archivos que deberemos crear en el directorio **phing**:

 * **build.xml**, en el cual se definen las tareas a realizar
 * **build.properties** para la configuración.

.. _build_xml:

build.xml
---------

El archivo tendrá la misma estructura para todos los proyectos de Klear, ya que
las acciones son las mismas.
El link de descarga es este:
`build.xml <https://github.com/irontec/klear-tutorial/blob/gh-pages/_static/phing/build.xml>`_

.. note::

   El archivo varía dependiendo de que acciones hay que ejecutar, por lo que habrá que ir
   revisando que no haya cambiado la implementación de **Klear** y con ello el **build.xml**
   . De ser así hay que actualizar el fichero.

build.properties
----------------

Este archivo tendrá la siguiente estructura:

.. code-block:: ini

   # This dir must contain the local application
   build.dir=./

   # Credentials for the database migrations
   db.host=localhost
   db.user=root
   db.pass=pass
   db.name=nuevoproyecto

   # paths to programs
   progs.mysql=/usr/bin/mysql

   # paths needed for generators
   modelGenerator.path=/opt/klear-development/generator
   application.path=../web/application

Inicialización de Phing
-----------------------

Antes de inicializar Phing hay que crear la base de datos con la que vamos a trabajar:

.. code-block:: console

   $ mysql -uroot -ppass -e "CREATE DATABASE nuevoproyecto"

Después creamos el archivo predeploy.sql con las órdenes iniciales en el directorio phing.

.. code-block:: mysql

   CREATE TABLE changelog (
   change_number BIGINT NOT NULL,
   delta_set VARCHAR(10) NOT NULL,
   start_dt TIMESTAMP NOT NULL,
   complete_dt TIMESTAMP NULL,
   applied_by VARCHAR(100) NOT NULL,
   description VARCHAR(500) NOT NULL
   ) COMMENT '[ignore]';
   ALTER TABLE changelog ADD CONSTRAINT Pkchangelog PRIMARY KEY (change_number, delta_set);

Una vez está todo configurado sólo deberemos ejecutar los siguientes comandos en el mismo directorio:

.. code-block:: console

   $ mkdir -p deltas deploy/scripts
   $ phing init




