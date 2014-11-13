application.ini
===============

Este archivo de configuración controla el comportamiento de la aplicación Zend.

.. attention:: 

   En el archivo se hace referencia a los ficheros de Klear, estos deberán ser previamente 
   instalados en el sistema.

.. note::

   Si queremos configurar la aplication.ini. Tenemos un ejemplo bastante útil en la sección :ref:`Creación del Proyecto <klear>`.

Códigos exclusivos solo para Klear
----------------------------------

.. contents::
   :local:
   :depth: 2

Klear Cache
###########

Klear Memcached
***************

Usa Memcached en vez de la caché normal.

.. code-block:: ini

   resources.cachemanager.klearconfig.backend.name = Memcached


Klear Black Hole
****************

Desactiva el uso de la cache

.. code-block:: ini

   resources.cachemanager.klearconfig.backend.name = Black_Hole
   
klearmatrixDashboard cache
**************************

Por defecto el Dashboard es cacheado cada 5 minutos. Sin embargo haciendo uso de los siguiente códigos podemos cambiar el tiempo
y la forma de guardar la caché:


Usar el Memcached en el Dashboard:

.. code-block:: ini

   resources.cachemanager.klearmatrixDashboard.backend.name = Memcached

Cambiar el tiempo de cacheado a 10 minutos:

.. code-block:: ini

   resources.cachemanager.klearmatrixDashboard.frontend.options.lifetime = 600
   
Plugin Kleardress
#################

Este plugin cambia la dirección **/klear** por solo **/**, tendremos a Klear en el dominio inicial (index), aunque también
significaría **no hacer uso de la zona pública del proyecto**.

.. code-block:: ini
   
   resources.frontController.plugins.KlearDress = "Iron_Controller_Plugin_KlearDress"

.. note:: 
   
   Ideal para **[intra|extra]**\nets.
   
Translator Zona Pública
#######################

Este código nos ayuda hacer uso de los archivos .po desde la zona pública del proyecto (hacer una web Multi-idioma):

.. code-block:: ini

   resources.frontController.plugins[] = 'Iron_Controller_Plugin_PublicTranslator'
   translate.language.es.title = 'Español'
   translate.language.es.language = 'es'
   translate.language.es.locale = 'es_ES'
   translate.language.en.title = 'English'
   translate.language.en.language = 'en'
   translate.language.en.locale = 'en_US'
   translate.language.fr.title = 'Francés'
   translate.language.fr.language = 'fr'
   translate.language.fr.locale = 'fr_FR'
   translate.language.pt.title = 'Portugués'
   translate.language.pt.language = 'pt'
   translate.language.pt.locale = 'pt_PT'
   
   translate.requestParam = 'lang'
   translate.defaultLanguage = 'es'
   translate.cookies.enabled = true
   translate.cookies.lifetime = 2592000 ;1 month
   
   defaultLanguageZendRegistryKey = 'defaultLang'