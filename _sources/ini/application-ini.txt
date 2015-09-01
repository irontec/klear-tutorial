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
   
Logs
####

Con esto configuramos los logs tanto para klear como para cualquier parte del código de nuestra aplicación.

.. code-block:: ini

    ;;;;;;;;;;
    ;; LOGS ;;
    ;;;;;;;;;;
    ;;;;;;;;;;;;;;; Priorities ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    ;; EMERG   = 0; // Emergency: system is unusable             ;;
    ;; ALERT   = 1; // Alert: action must be taken immediately   ;;
    ;; CRIT   = 2; // Critical: critical conditions              ;;
    ;; ERR    = 3; // Error: error conditions                    ;;
    ;; WARN   = 4; // Warning: warning conditions                ;;
    ;; NOTICE = 5; // Notice: normal but significant condition   ;;
    ;; INFO   = 6; // Informational: informational messages      ;;
    ;; DEBUG  = 7; // Debug: debug messages                      ;;
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    ;;;;;;;;;;;;;;; Available filters ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    ;;
    ;;
    ;; Zend_Log_Filter_Message
    ;;
    ;; resources.log.<LOGGER>.filterName = 'Message'
    ;;
    ;; resources.log.<LOGGER>.filterParams.regexp = <REGEX>
    ;;
    ;;
    ;;
    ;; Zend_Log_Filter_Priority
    ;;
    ;; resources.log.<LOGGER>.filterName = 'Priority'
    ;;
    ;; resources.log.<LOGGER>.filterParams.priority = <PRIORITY> ;;
    ;; resources.log.<LOGGER>.filterParams.operator = "<="
    ;;
    ;;
    ;;
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    resources.log.timestampFormat = "Y-m-d H:s:i"
    ;;;;;;;;;;;;;;;;;;;;;; MAIL ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    resources.log.mail.writerName = "Mail"
    resources.log.mail.writerParams.mail = "Zend_Mail"
    Resources.log.mail.writerParams.charset = 'utf-8'
    resources.log.mail.writerParams.subject = 'miApp log'
    resources.log.mail.writerParams.from = "no-reply@irontec.com"
    resources.log.mail.writerParams.to[] = 'mail-destino@irontec.com'
    resources.log.mail.filterName = "Priority"
    resources.log.mail.filterParams.priority = 2
    resources.log.mail.filterParams.operator = "<="
    resources.log.mail.formatterName = "Simple"
    resources.log.mail.formatterParams.format = '%timestamp% %priorityName% (%priority%) [Mail 1]: %message%'
    resources.log.mail2.writerName = "Mail"
    resources.log.mail2.writerParams.mail = "Zend_Mail"
    resources.log.mail2.writerParams.charset = 'utf-8'
    resources.log.mail2.writerParams.subject = 'appName log'
    resources.log.mail2.writerParams.from = "no-reply@origen.com"
    resources.log.mail2.writerParams.to[] = 'destino@destino.com'
    resources.log.mail2.filterName = 'Message'
    resources.log.mail2.filterParams.regexp = '#^\[Texto\]#'
    resources.log.mail2.formatterName = "Simple"
    resources.log.mail2.formatterParams.format = '%timestamp% %priorityName% (%priority%) [Mail 2]: %message%'
    ;;;;;;;;; SYSLOG ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    resources.log.syslog.writerName = "Syslog"
    resources.log.syslog.writerParams.application = "appName"
    resources.log.syslog.writerParams.facility = LOG_SYSLOG
    resources.log.syslog.filterName = "Priority"
    resources.log.syslog.filterParams.priority = 6
    resources.log.syslog.filterParams.operator = "<="
    resources.log.syslog.formatterName = "Simple"
    resources.log.syslog.formatterParams.format = '%timestamp% %priorityName% (%priority%): %message%'
    ;;;;;;;;;;;;; FIREBUG ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    resources.log.firebug.writerName = "Firebug"
    resources.log.firebug.filterName = "Priority"
    resources.log.firebug.filterParams.priority = 7
    resources.log.firebug.filterParams.operator = "<="
    resources.log.firebug.formatterName = "Simple"
    resources.log.firebug.formatterParams.format = '%timestamp% %priorityName% (%priority%): %message%'
    ;;;;;;;;;;;;; Zend_Log_Writer_Stream ;;;;;;;;;;;;;;;;;;;;;;;;;;;
    resources.log.stream.writerName = "Stream"
    resources.log.stream.writerParams.stream = APPLICATION_PATH "/logs/appNamelog.log"
    resources.log.stream.writerParams.mode = "a"
    resources.log.stream.formatterName = "Simple"
    resources.log.stream.formatterParams.format = '%timestamp% %priorityName% (%priority%) FILE: %message%' PHP_EOL
    ;;;;;;;;;; Zend_Log_Writer_Null ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
    resources.log.null.writerName = "Null"
    
De esta manera podemos añadir todos los writters que queramos, aunque sean del mismo tipo, siempre que le demos un nombre único ( **resources.log.writter1**... **resources.log.writter2**... ) Hay que tener en cuenta que cada writter tiene sus propios parámetros.
La configuración **timestampFormat = "Y-m-d H:s:i"** es común a todos los writters.

Klear está preparado para utilizar esta configuración por lo que si sólo queremos usar logs por defecto de Klear con esto sería suficiente.

Para hacer logs en cualquier otra parte del código hay que hacer lo siguiente:

.. code-block:: php

    $bootstrap = \Zend_Controller_Front::getInstance()->getParam('bootstrap');
    $logger = $bootstrap->getResource('log');
    $logger->log("Mensaje", \Zend_Log::WARN);
    

.. attention:: 

    Hay que tener en cuenta que **si $logger es nulo** (no se ha especificado ningún writter en el application.ini) **e intentamos hacer:**
    
    .. code-block:: ini
    
         $logger->log("Mensaje", \Zend_Log::WARN);
    
    se lanzará una excepción, por lo que si en algún entorno no queremos loggear hay que añadir la línea:

    .. code-block:: php
    
        resources.log.null.writerName = "Null"
        
Con esto vamos a loggear en todos los writters que lo permitan. Para saber que writters van aloggear nos tenemos que fijar en los filtros ( **resources.log.writter1.filterName**, **resources.log.writter1.filterParams**. ).
Básicamente hay dos tipos de filtros:

    • Los que filtran por prioridad.
    • Los que filtran por expresión regular en el mensaje.

Prioridad
*********

.. code-block:: ini

    resources.log.writter1.filterName = 'Priority'
    resources.log.writter1.filterParams.priority = 4
    resources.log.writter1.filterParams.operator = "<="
    
Con el parámetro **priority** especificamos la prioridad, es decir, el código de error que le pasamos cuando hacemos **$logger->log("Mensaje", \Zend_Log:: WARN);**. En este caso el 4 se corresponde con un warning.

Con el parámetro **operator** le indicamos la condición. En este caso que sea menor o igual a 4. Si no se indica el operator por defecto se usa el ' <= '. Es decir, el writter1 solamente va a loggear los mensajes cuyo código de error sea menor o igual que 4.

Por ejemplo, en el caso en el que queremos que se guarde el log en una carpeta concreta, pero queremos que cada tipo de log esté en un archivo distinto (**miapp.error.log**, **miapp.warning.log**, **miapp.info.log**...) se podría usar un writter tipo 'Stream' distinto por cada tipo de log usando el filtro de prioridad y usando el operador '=='.

Sería algo así:

.. code-block:: ini

    resources.log.stream.writerName = "Stream"
    resources.log.stream.writerParams.stream = APPLICATION_PATH "/logs/miapp.error.log"
    resources.log.stream.writerParams.mode = "a"
    resources.log.stream.filterName = "Priority"
    resources.log.stream.filterParams.priority = 3
    resources.log.stream.filterParams.operator = "<="
    resources.log.stream.formatterName = "Simple"
    resources.log.stream.formatterParams.format = '%timestamp% %priorityName% (%priority%): %message%' PHP_EOL
    resources.log.stream1.writerName = "Stream"
    resources.log.stream1.writerParams.stream = APPLICATION_PATH "/logs/miapp.warning.log"
    resources.log.stream1.writerParams.mode = "a"
    resources.log.stream1.filterName = "Priority"
    resources.log.stream1.filterParams.priority = 4
    resources.log.stream1.filterParams.operator = "=="
    resources.log.stream1.formatterName = "Simple"
    resources.log.stream1.formatterParams.format = '%timestamp% %priorityName% (%priority%): %message%' PHP_EOL
    resources.log.stream2.writerName = "Stream"
    resources.log.stream.writerParams.stream = APPLICATION_PATH "/logs/miapp.info.log"
    resources.log.stream2.writerParams.mode = "a"
    resources.log.stream2.filterName = "Priority"
    resources.log.stream2.filterParams.priority = 5
    resources.log.stream2.filterParams.operator = ">="
    resources.log.stream2.formatterName = "Simple"
    resources.log.stream2.formatterParams.format = '%timestamp% %priorityName% (%priority%): %message%' PHP_EOL

Expresión regular
*****************

.. code-block:: ini

    resources.log.writter2.filterName = 'Message'
    resources.log.writter2.filterParams.regexp = '#^\[Texto\]#'
    
Este filtro solo tiene un parámetro, **regexp**, que es en el que le indicamos la expresión regular a matchear contra el mensaje del log. Para que el writter loggee se tiene que cumplir la expresión regular, que en este caso loggeará todos los mensajes que empiecen por “[Texto]”. De esta manera podemos tener dos writters del mismo tipo que matcheen diferentes expresiones regulares.

Por ejemplo, queremos que los logs se mande por e-mail, pero en función de la parte del código en el que estemos queremos que se manden a una dirección u otra.
Para esto hacemos dos writters con un to distinto para cada uno y con una expresión regular distinta para cada uno. Así en función del mensaje del log se usará un writter u otro y por tanto se mandará a un e-mail o a otro.

