.. _klear.yaml:

Klear.yaml
==========

Este archivo es creado por el generador yaml, así que solo nos basaremos en cómo configurar las opciones ya generadas
para nuestro proyecto. 
El archivo de configuración de Klear está situado en **application/configs/klear/klear.yaml**.

.. note::
   Recordar, en el tag oasis, se puede tirar de ${auth.*} para este fichero.


.. code-block:: yaml

   production: 
     main: 
       log: 
         writerName: "Null"
         writerParams: 
       sitename: MyApp
       logo: klear/images/klear.png
       year: 2013
       lang: es
       langs: 
         es: 
           title: Español
           language: es
           locale: es_ES
         en: 
           title: English
           language: en
           locale: en_US
         eu: 
           title: Euskera
           language: eu
           locale: eu_ES
       jqueryUI: 
         theme: redmond
         path: /ui/themes/absolution-red/jquery-ui.css #ruta para temas externos: theme se utilizará como nombre de éste.
         themeRoller:
           paths: #para temas externos
             verdecito: /ui/themes/aristo-green/jquery-ui.css
           themes: #para temas predefinidos
             - pepper-grinder
             - redmond
             - ui-darkness
             - aristo-red
       raw:
         css: 
           - "css/klear.css"
       cssExtended: 
       actionHelpers:
       defaultCustomConfiguration:
         optionCollectionPlacement: both
       rememberScroll: true
       auth: 
         adapter: Klear_Auth_Adapter_Basic
         title: _("Access denied")
         description: _("Insert your username")
       timezone: Europe/Madrid
     menu: 
       General: 
         title: _("Main management")
         description: _("Main management")
         submenus: 
           DevelopersList: 
             title: ngettext('Developer', 'Developers', 0)
             class: ui-silk-text-list-bullets
             description: _("List of %s", ngettext('Developer', 'Developers', 0))
             showOnlyIf: true|false
             meta: "literal que viajará hasta el atributo data-meta del elemento de menú."
           KlearImageGalleriesList: 
             title: ngettext('Klear image gallery', 'Klear image galleries', 0)
             class: ui-silk-text-list-bullets
             description: _("List of %s", ngettext('Klear image gallery', 'Klear image galleries', 0))
   testing: 
     _extends: production
   staging: 
     _extends: production
   development: 
     _extends: production

.. contents::
   :local:
   :depth: 3

main
----

Configuración básica del Interfaz del Klear

log
###

Nos sirve para especificar una ubicación para redireccionar los logs de nuestra aplicación.

.. code-block:: yaml
   
   log:
      writerName: stream
      writerParams:
        stream: /tmp/klear.log
        mode: a
        
Para syslog

.. code-block:: yaml
   
   log:
      writerName: syslog
      writerParams:
        application: app-admin-portal
        facility: LOG_USER

sitename
########

Define el nombre de la aplicación.

.. code-block:: yaml

   sitename: Mi aplicación de Klear

logo
####

Ruta hasta la imagen que queremos se use como logo. La ruta es relativa a la carpeta
**public**.

.. code-block:: yaml

   logo: img/logo.png

year
####

Año de la aplicación, por defecto se pone el año en el que fue generado este yaml.

lang
####

Con esta opción se define el idioma por defecto de la aplicación.

.. code-block:: yaml

   lang: Euskera

.. warning:: 
   
   El idioma elegido deberá estar configurado dentro de la etiqueta de **langs**\.

langs
#####

Lista con los idiomas soportados en la aplicación.

Cada idioma debe definir:

* **title**: Título del idioma
* **language**: Nombre corto del idioma (ejem.: **eu**)
* **locale**: El código locale del idioma (ejem.: **eu_ES**)

.. code-block:: yaml

   langs:
      Euskera:
        title: Euskera
        language: eu
        locale: eu_ES
      Espanol:
        title: Español
        language: es
        locale: es_ES

jqueryUI
########

Es el tema que adoptará la interfaz del Klear. Los css que optará podría estar en local o en la nube. Por ejemplo:

**Formato local**: los css deben ubicarse en la zona pública (carpeta **public**). Esta opción suele ser usada cuando
algunas aplicaciones no tienen acceso al exterior. El formato sería el siguiente:

.. code-block:: yaml
   
   jqueryUI:
      path: /css/south-street/jquery-ui-1.8.22.custom.css
      
**CSS en la nube**: Los estilos a elegir son limitados, pero son los más recomendados, serán seleccionados según la imagen
que tiene que dar la aplicación. El formato sería el siguiente:

.. code-block:: yaml

   jqueryUI: 
      theme: redmond

themeRoller
***********

Muestra una lista de temas a elegir.

.. code-block:: yaml

   themeRoller:
      paths: #para temas externos
         verdecito: /ui/themes/aristo-green/jquery-ui.css
      themes: #para temas predefinidos
         - pepper-grinder
         - redmond
         - ui-darkness
         - aristo-red

Themes oficiales
****************
    - base
    - black-tie
    - blitzer
    - cupertino
    - dark-hive
    - dot-luv
    - eggplant
    - excite-bike
    - flick
    - hot-sneaks
    - humanity
    - le-frog
    - mint-choc
    - overcast
    - pepper-grinder
    - redmond
    - smoothness
    - south-street
    - start
    - sunny
    - swanky-purse
    - trontastic
    - ui-darkness
    - ui-lightness
    - vader

Themes irontec
**************
    - absolution
    - absolution-green
    - absolution-red
    - aristo
    - aristo-dark
    - aristo-green
    - aristo-red
    - delta
    - twitter
    - redmond-red

cssExtended
###########
Esta configuración es opcional, normalmente es usada para que podamos elegir un favicon o ampliar nuestro listado de iconos en la interfaz. Ya sea porque
los iconos por defecto no se adecuan al tema de la aplicación o porque se requiere nuevas imágenes para representar nuestras secciones.

Seleccionar favicon
*******************

Solo se necesita poner la imagen en la carpeta **public** de nuestro proyecto y a partir de allí señalar su ubicación.

.. code-block:: yaml

   cssExtended:
      silkExtendedIconPath: /images/favicon.ico
      
.. note::
   Los navegadores solo reconocen como un favicon, las imágenes con la extensión **.ico** que suele tener un tamaño de **16x16** o **32x32** (px).

Generar nuevos iconos
*********************

Para empezar necesitamos crear la carpeta **"css"** o **"style"** en nuestra carpeta **public**. Luego en nuestra carpeta
**/public/css/** crearemos la siguiente estructura de carpetas:

.. code-block:: console

   .
   └── ui-klear
       ├── cache
       └── icons

.. caution::
   Asegurarse que todas las carpetas tengan todos los permisos: **chmod 777** 
   
Especificar la ruta de la carpeta icons:

.. code-block:: yaml

   cssExtended:
      silkExtendedIconPath: /css/ui-klear/icons
   
Por último, agregar **"/css-extended/silk-extended/silk-extended-sprite.css?generate"** en nuestro ruta después de **"/klear"**,
por ejemplo:

.. code-block:: text
   
   http:// ~ /klear/css-extended/silk-extended/silk-extended-sprite.css?generate

.. note::
   Klear tiene disponible los iconos de la serie Silk Icon Sprite: https://klear-wiki.irontec.com/silk/
   
raw
###

Si no estamos satisfechos con los estilos que ya están definidos, podemos también incorporar algunos propios haciendo uso de esta opción.

Para ello, debemos crear una carpeta con el nombre **"assets"** en nuestra carpeta **"application"**, dentro de ella ya decidimos donde
ubicaremos nuestro estilo; por ejemplo, yo crearía la carpeta **"css"** y dentro de ella mi archivo de estilo (.css), quedando algo como esto:

.. code-block:: console

   application
   └── assets
       └── css
           └── style.css

En el yaml, debemos especificar la ruta de la siguiente manera:

.. code-block:: yaml

    raw:
      css:
        - "./default/css/style.css"

optionCollectionPlacement
#########################

Configurar las posiciones de los botones dentro de los módulos de Klear.

Los valores permitidos son: **top** | **both** | **bottom**

La configuración puede ser definida como **string** o como **colección**.

- **Configuración de string**:

.. code-block:: yaml

   optionCollectionPlacement: both

- **Configuración de colección**:

.. code-block:: yaml

   optionCollectionPlacement:
     default: 'both'
     list: 'top'
     edit: 'bottom'

rememberScroll
##############

Permite recordar la posición de las pestañas.

Esto es útil en listados largos en los que al editar uno de los registros
la página se mueve hacia arriba para facilitar la edición. En estos casos
al cerrar la pestaña de edición o al volver a la pestaña original del listado
el listado está al comienzo, teniendo que volver a buscar por que registro
estábamos antes de editar.

Si se activa esta opción al cerrar la pestaña de edición o al volver a la pestaña del listado,
el listado se moverá automáticamente a la posición en la que estábamos antes de
editar.

.. code-block:: yaml

    rememberScroll: true

auth
####

Configuración del adaptador de autenticación.

.. attention:: 

   El adaptador por defecto 
   es **Klear_Auth_Adapter_Basic**\, pero podemos modificarlo siempre y cuando
   el que utilicemos implemente **Klear_Auth_Adapter_KlearAuthInterface**\.

Párametros configurables para el adaptador:

* **adapter**: Nombre del adaptador.
* **title**: Título de la ventana emergente de login.
* **description**: Descripción que se muestra en la ventana de login.
* **userMapper**: Clase Mapper que implementa Klear_Auth_Adapter_Interfaces_BasicUserMapper. El valor predeterminado es Klear_Model_Mapper_Users.
* **session**: Configuración de almacenamiento de sesión.
   * **name**: Nombre de sesión para la autenticación (por defecto **klear_auth**\) , si queremos tener mas de una instancia en el mismo host, esto debe modificarse.
   * **disableChangeName**: Si queremos que Klear comparta el namespace con la zona pública pondremos a **true**.

.. code-block:: yaml

   auth:
     adapter: Klear_Auth_Adapter_Basic
     title: _("Access denied")
     description: _("Insert your username")
     userMapper: \MyApp\Mapper\Sql\KlearUsers
     session:
       name: my_auth_session_name
       disableChangeName: false

El siguiente código genera la tabla necesaria, con el usuario **admin** y contraseña *admin*:

.. code-block:: mysql

    CREATE TABLE `KlearUsers` (
      `userId` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
      `login` varchar(40) NOT NULL,
      `email` varchar(255) NOT NULL,
      `pass` varchar(80) NOT NULL COMMENT '[password]',
      `active` tinyint(1) DEFAULT '0',
      `createdOn` datetime NOT NULL DEFAULT '0000-00-00 00:00:00',
      PRIMARY KEY (`userId`),
      UNIQUE KEY `login` (`login`),
      UNIQUE KEY `email` (`email`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
    INSERT INTO `KlearUsers`(login, email, pass, active) VALUES('admin','admin@example.com','$2a$08$OBG.QrnbL0iJh3O0LlKxBOFWhZKc77NeHqn8vutpg5velSt0dwkeK',1);

disableMinifier
###############

Para activar o desactivar los minificadores dinámicos. Útil en entornos de desarrollo.

.. code-block:: yaml

   disableMinifier: true

disableAssetsCache
##################

Para activar o desactivar la cache de **css** y **js**. Útil para entornos de desarrollo.

.. code-block:: yaml

   disableAssetsCache: true

defaultCustomConfiguration
##########################

Posibilita la configuración de propiedades fácilmente accesibles desde cualquier parte de la aplicación.

.. code-block:: yaml

   defaultCustomConfiguration:
        optionCollectionPlacement: both
        autoClose: true
        disableSave: false
        disableAddAnother: false
        autoShowSizeOnExpandableFields: true

Después en el código:

.. code-block:: guess

   $bootstrap = Zend_Controller_Front::getInstance()->getParam('bootstrap');
   $siteConfig = $bootstrap->getResource('modules')->offsetGet('klear')->getOption('siteConfig');
        
   $property = $siteConfig->getDefaultCustomConfiguration('optionCollectionPlacement');

timezone
########

La base de datos, por defecto cuenta con la zona horaria UTC 0, pero para mostrar la fecha y hora, según a nuestra zona horaria, hay que
modificar este apartado.

.. code-block:: yaml
   
   timezone: Europe/Madrid

menu
----

General
#######

El klear por defecto lista todas las tablas con el tag **[entity]**, pero si llega a generarse por segunda vez los yaml, las nuevas tablas
no se verán listadas en el **klear/klear.yaml** sino en **klearRaw/klear.yaml**.

.. code-block:: yaml

     menu: 
       General: 
         title: _("Main management")
         description: _("Main management")
         showOnlyIf: true|false
         meta: "literal que viajará hasta el atributo data-meta del elemento de menú."
         submenus: 
           DevelopersList: 
             title: ngettext('Developer', 'Developers', 0)
             class: ui-silk-text-list-bullets
             description: _("List of %s", ngettext('Developer', 'Developers', 0))
             showOnlyIf: true|false
             meta: "literal que viajará hasta el atributo data-meta del elemento de menú."

Sus opciones correspondientes son las siguientes:

title
*****
Título de la sección

description
***********
Descripción de la sección

showOnlyIf
**********
Valor booleano que deterinará si la sección/subsección se muestra o no
(A combinar con ${auth.*} para un disfrute máximo.

meta
****
LLevará hasta el marcado "data-meta", un literal concreto.
(a combinar con un script custom)


submenus
********
Aquí listaremos todos los list que necesitamos para hacer nuestra interfaz (opciones **title** y **description**).

footerMenu
----------

Este menu esta en la parte inferior centrada de la pantalla. Solo lista el icono y el título de la sección.
El primer tab de configuración es el nombre del menu, el siguiente es el del "title" y el "submenus" en el cual se configura las vistas que se listaran.

.. code-block:: yaml

  footerMenu:
    footer:
      title: ''
      submenus:



