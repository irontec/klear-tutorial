.. _generadoresDBLink:

Generadores
===========

.. contents::
   :local:
   :depth: 1


.. _dbGenerator:

Generador de BD
***************

El generador de la base de datos nos sirve para adaptar nuestras tablas de la Base de datos a la interfaz del Klear,
según la función de los campos de la tabla.

Aplicar el siguiente comando para aplicar los cambios necesarios a la base de datos.

.. code-block:: console

   $ php /opt/klear-development/generator/klear-db-generator.php -a /home/user/workspace/NuevoProyecto/web/application/



Generador de Models y Mappers
*****************************

Genera las clases para todas las tablas existentes de la base de datos, a menos que sean ignoradas con el tag [ignore].

Todas las clases son guardadas en la carpeta **library** (APPLICATION_PATH/../library).

Requisitos
----------

1. Haber configurado el application.ini con los parámetros necesarios para acceder a la **base de datos** y el **appnamespace** (normalmente el nombre del proyecto).

.. note::
   Más detalles en :ref:`klear`

2. Tener las tablas creadas, como mínimo la tabla KlearUsers.

.. note::
   Más detalles en :ref:`tablaklearUsers`


Generar los models y mappers
----------------------------

.. code-block:: console

   $ php /opt/klear-development/generator/klear-models-mappers-generator.php -a /home/user/workspace/NuevoProyecto/web/application/ -e development

.. _generadorAssets:

Generador de Assets
*******************

.. attention::
   | El Generador de Assets (klear-assets-generator) se encarga de autorrellenar las tablas Countries y Timezones según el parámetro (-c/-t) que le enviemos.
   | Ejecutar sólo si se usan Timezones y Countries.

.. _generadorPaises:

Países
------

Primero registramos los países debido a las dependencias de la tabla Timezones:

.. code-block:: console

   $ php /opt/klear-development/generator/klear-assets-generator.php -a /home/user/workspace/NuevoProyecto/web/application/ -e development -c -v
   Registering es.
   Registering eu.
   Registering en.
   Registering pt.
   .......................................................................................
   259 new countries added/updated.

.. note::
   La fuente de los países que se coge para la tabla se encuentra en: https://raw.github.com/umpirsky/country-list/master/country/icu/%lang%/country.json

.. _generadorTimeZones:

TimeZones
---------

Repetimos el proceso con los Timezones:

.. code-block:: console

   $ php /opt/klear-development/generator/klear-assets-generator.php -a /home/user/workspace/NuevoProyecto/web/application/ -e development -t -v
   .......................................................................................
   836 timezones added/updated.

.. note::
   La fuente de los timezones está en: http://www.iana.org/time-zones/repository/data/zone.tab

Generador de Yaml
*****************

Descripción
-----------

El generador de **Yaml** al igual que los de **Models**\/**Mappers**
se encuentra en la carpeta *generator* de la carpeta principal de **Klear**.

El generador crea por defecto el archivo **klear.yaml** y cada uno de los **models**.

Por otra parte, para aquellas tablas que en la **BBDD** tengan como comentario la etiqueta
**[entity]** creará el *ModelList.yaml* correspondiente.

Requisitos
----------

Los únicos requisitos que tiene el generador:

* En el archivo **application.ini**:
   * Configuración de base de datos.
   * Configuración del **namespace**.


Generar los yaml
----------------

.. code-block:: console

   $ php /opt/klear-development/generator/klear-yaml-generator.php -a /home/user/workspace/NuevoProyecto/web/application/ -e development




