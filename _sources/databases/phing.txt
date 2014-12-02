Phing
=====

Phing permite definir un archivo XML con una serie de tareas que se ejecutarán (targets) para, por ejemplo, ejecutar comandos svn, creación-destrucción-cambio de directorios y archivos, permisos, o ejecución de las pruebas unitarias, y un largo etc. En este caso, aprovechamos las funcionalidades de Phing para gestionar los cambios en la Base de Datos.

Tareas definidas
****************

En la siguiente lista se muestra el listado de tareas ya definidas y se explica su funcionalidad:

* **init**: Genera la tabla necesaria para almacenar las versiones de la **BBDD**
* **migrate**: Ejecuta todos los deltas necesarios para llegar a la versión actual del proyecto
* **generate-db**: Ejecuta el generador de la Base de Datos
* **generate-models**: Ejecuta el generador de modelos
* **generate-yaml**: Ejecuta el generator de Yaml's
* **run-generators**: Ejecuta todos los generators anteriores
* **db-change**: Ejecuta el migrate y el **run-generators**

Argumentos soportados
*********************

El fichero build.xml tiene predefinidas 3 valores variables necesarios para su correcto funcionamiento:

* **-De**: Setea el entorno de ejecución [default "production"]
* **-Dk**: Setea la ruta a la carpeta base de klear [default "/opt/klear"]
* **-Da**: Setea la ruta a la carpeta de la aplicación ZF1 [default "../web/application"]

.. code-block:: console

   $ phing db-change -De=development -Dk=/remoteKlear -Da=../admin/application


DbDeploy
********

Es lo que **Phing** define como una *"Tarea Opcional"*.

Nos permite tener una serie de deltas de la base de datos (en ficheros numerados) que **dbDeploy**
se encargará de ejecutar en orden. **DbDeploy** guarda la versión de la BBDD (último delta ejecutado)
en una tabla para saber cuales son los cambios que debe ejecutar.

Los cambios realizados los va guardando en un fichero dentro del directorio **deploy/scripts/** (definido en el :ref:`build_xml`)

Es lo que usan nuestras tareas **"init"** y **"migrate"**

Estructura del directorio
*************************

.. code-block:: bash

   phing
   |-- build.xml
   |-- deltas                                   <- Directorio con ficheros delta
   |   |-- 001-Initial-KlearInterval-Tables.sql
   |   `-- 002-Add-Body-To-Subsections.sql
   |-- deploy
   |   `-- scripts
   |       |-- deploy-201303141301.sql
   |       |-- undo-201303121235.sql
   |-- predeploy.sql                            <- SQL con la estructura de la tabla del Changelog
