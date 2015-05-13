Phing
=====

Phing requiere de **un único** archivo que deberemos crear en el directorio **phing**:

 * **build.xml**, en el cual se definen las tareas a realizar

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



La última versión de build.xml, tiene soporte para parámetros

* -De >> Setea el entorno de ejecución [default "production"]
.. code-block:: console

  $ phing db-change -De=development


* -Dk >> Setea la ruta a la carpeta base de klear [default "/opt/klear"]
.. code-block:: console
  
  $ phing db-change -Dk=/remoteKlear

* -Da >> Setea la ruta a la carpeta de la aplicación ZF1 [default "../web/application"]
.. code-block:: console
  
  $ phing db-change -Da=../admin/application



Es buena idea editar la copia local de build.xml en cada proyecto con los valores en producción:

.. code-block:: xml

    <!-- command line switch >> -Dk=/remoteKlear -->
    <property name="klear.base" refid="k" fallback="/opt/klear" />
    <!-- command line switch >> -Da=../web/application -->
    <property name="application.path" refid="a" fallback="../web/application" />
    <!-- command line switch >> -De=development -->
    <property name="application.env" refid="e" fallback="production" />


Así ejecutaremos en producción tranquilamente:

.. code-block:: console

  $ phing migrate



Para no tener que acordarse se los argumentos de consola cada vez que ejecutamos phing en desarrollo, se puede crear un pequeño script:


.. code-block:: console

  $ echo "phing \$1 -De=development -Dk=/remoteKlear -Da=../admin/application" > dev-phing; chmod a+x dev-phing


Luego, en desarrollo simplement ejecutaríamos:


.. code-block:: console

  $ ./dev-phing db-change


Inicialización de Phing
-----------------------

Antes de inicializar Phing hay que crear la base de datos con la que vamos a trabajar:

.. code-block:: console

   $ mysql -uroot -ppass -e "CREATE DATABASE nuevoproyecto"
   
Luego en /web/application/configs/application.ini añadimos los datos de la base de datos para development.
.. code-block:: console
   resources.db.adapter = "MYSQLI"
   resources.db.params.dbname = "nuevoproyecto"
   resources.db.params.username = "root"
   resources.db.params.password = "farsa"
   resources.db.params.host = "localhost" 

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




