.. _klear:

Configuración Klear
===================

.. contents::
   :local:
   :depth: 3


En esta sección hablaremos de lo básico para que klear empiece a funcionar en nuestro ordenador. Siendo los dos primeros puntos, los más importantes
ya que su correcta configuración depende los generadores y el funcionamiento de la interfaz del Klear:

.. attention::
   Recordar que para tener acceso o identificarse en la aplicación, hay que tener creado la :ref:`tablaklearUsers`. Además, la carpeta **cache** que debe
   estar en la carpeta **application** debe contar con todos los permisos; si se piensa usar la interfaz de Klear para subir archivos, debe estar creado la
   carpeta **storage** al mismo nivel de **application** con todos los permisos como se explica en :ref:`generatorDataBaseFile`.

   .. code-block:: console

       $ cd /home/user/workspace/NuevoProyecto/web
       $ mkdir application/cache storage
       $ chmod 777 application/cache
       $ chmod 777 storage

application.ini
---------------

Configuración del archivo /home/user/workspace/NuevoProyecto/web/application/configs/**application.ini**.

Por defecto el Zend te crea el siguiente archivo, siendo como prioridad la sección "[production]"
configuración que se cogerá cuando se suba en producción y "[development : production]" la configuración cuando lo
trabajaremos en local:

.. code-block:: ini
   :linenos:
   :emphasize-lines: 1-9,18-21

   [production]
   phpSettings.display_startup_errors = 0
   phpSettings.display_errors = 0
   includePaths.library = APPLICATION_PATH "/../library"
   bootstrap.path = APPLICATION_PATH "/Bootstrap.php"
   bootstrap.class = "Bootstrap"
   appnamespace = "Application"
   resources.frontController.controllerDirectory = APPLICATION_PATH "/controllers"
   resources.frontController.params.displayExceptions = 0

   resources.layout.layoutPath = APPLICATION_PATH "/layouts/scripts/"
   [staging : production]

   [testing : production]
   phpSettings.display_startup_errors = 1
   phpSettings.display_errors = 1

   [development : production]
   phpSettings.display_startup_errors = 1
   phpSettings.display_errors = 1
   resources.frontController.params.displayExceptions = 1

Para **Klear** es necesario incorporar sus módulos y librerías que se deben haber descargado desde el GitHub de Irontec e
intentar organizarlo de esta manera.:

.. code-block:: ini
   :linenos:
   :emphasize-lines: 12,14-17,19-20,22-24,26-30,47,49-50

   [production]

   phpSettings.display_startup_errors = 0
   phpSettings.display_errors = 0
   includePaths.library = APPLICATION_PATH "/../library"
   bootstrap.path = APPLICATION_PATH "/Bootstrap.php"
   bootstrap.class = "Bootstrap"

   resources.frontController.controllerDirectory = APPLICATION_PATH "/controllers"
   resources.frontController.params.displayExceptions = 0

   appnamespace = "KlearInterval"

   resources.view[] = ""
   resources.modules[] = ""
   resources.frontController.moduleDirectory = "/opt/klear-libs/modules"
   includePaths.klearLibrary = /opt/klear-libs/library

   autoloaderNamespaces[] = "KlearInterval"
   autoloaderNamespaces[] = "Iron"

   resources.frontController.actionhelperpaths.Iron_Controller_Action_Helper = "Iron/Controller/Action/Helper"
   resources.frontController.plugins.AjaxLayout = "Iron_Controller_Plugin_AjaxLayout"
   resources.view.helperPath.Iron_View_Helper = "Iron/View/Helper"

   resources.db.adapter = "MYSQLI"
   resources.db.params.dbname = "klearIntervals"
   resources.db.params.username = "root"
   resources.db.params.password = "pass"
   resources.db.params.host = "localhost"

   resources.locale.default = "es_ES"
   resources.locale.force = true

   [staging : production]

   [testing : production]
   phpSettings.display_startup_errors = 1
   phpSettings.display_errors = 1

   [development : production]

   phpSettings.display_startup_errors = 1
   phpSettings.display_errors = 1
   resources.frontController.params.displayExceptions = 1

   resources.cachemanager.klearconfig.backend.name = Black_Hole

   resources.frontController.moduleDirectory = "/opt/klear-development/modules"
   includePaths.klearLibrary = /opt/klear-development/library

Para poder trabajar en el entorno de desarrollo debemos indicarlo en el archivo
/home/user/workspace/NuevoProyecto/web/public/.htaccess

.. code-block:: ini

    SetEnv APPLICATION_ENV development

.. _klearini:

klear.ini
---------

Configuración del archivo /home/user/workspace/NuevoProyecto/web/application/configs/**klear.ini**.

.. warning::
   Sin este archivo, los generadores "db" , "yaml" y "model-mappers" no funcionan.

Es la base para que los generadores principales sepan cómo configurar la interfaz del Klear por defecto como idiomas, zona horaria,
desarrollador responsable, etc. Es como configurar el application.ini, pero aquí solo podemos configurar los siguientes puntos:

Un ejemplo muy sencillo de cómo podría configurarse un klear.ini, sería el siguiente:

.. code-block:: ini

   [production]
   klear.languages[] = es
   klear.languages[] = en
   klear.languages[] = fr
   klear.languages[] = pt

   docs.author = "Nombre usuario"
   docs.email = "nombre@usuario.com"

   klear.countries.table = Countries
   klear.countries.name = name
   klear.countries.code = code

   klear.timezones.table = Timezones
   klear.timezones.tz = tz
   klear.timezones.comment = comment
   klear.timezones.countryLink = countryId

   [testing : production]

   [development : production]

Klear Languages
+++++++++++++++

Son los lenguages que emplearíamos en nuestra Interfaz del Klear lo significaría que nuestros generadores con cuántos y qué idiomas
serán necesarios para configurar nuestra base de datos, los yaml y los models-mapper. Luego por nuestra cuenta o por el cliente, se
encargarán de traducir de los literales de la interfaz por si estos no se encuentran en nuestra base de datos de idiomas del Klear.

.. code-block:: ini

   klear.languages[] = es
   klear.languages[] = en
   klear.languages[] = fr
   klear.languages[] = pt

Docs Author
+++++++++++

Este campo es opcional, pues no destaca en la interfaz del Klear pero sí en los archivos internos. Es una formalidad para identificar
a la persona responsable del proyecto.

.. code-block:: ini

   docs.author = "Your Name"
   docs.email = "your@name.com"

Klear Countries y Klear Timezones
+++++++++++++++++++++++++++++++++

Estos dos campos se encuentran normalmente relacionados. Timezones depende de Countries. Tiene como objetivo contener datos
estáticos de Países y Zonas horarias para hacer uso de ellos según sea necesario para nuestro proyecto.

Klear Countries
***************

.. code-block:: ini

   klear.countries.table = Countries
   klear.countries.name = name
   klear.countries.code = code

* **code** es un campo único

* **name** es un campo Multilang (se escribirá el nombre del país en cada idioma).

* Deben estar los lenguajes definidos en **klear.ini**.

* Deben estar creados los modelos para la tabla especificada.

Klear Timezones
***************

.. code-block:: ini

   klear.timezones.table = Timezones
   klear.timezones.tz = tz
   klear.timezones.comment = comment
   klear.timezones.countryLink = countryId

* **tz** es un campo único

* **comment** es un campo opcional. Es un comentario para países con múltiples timezones…

* **countryId** implica que la tabla de países tiene que existir, estar configurada en el klear.ini y estar ya importada (o hacerlo al mismo tiempo)

