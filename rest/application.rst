application.ini
---------------

En el **application.ini** se definen más parametros de los mencionados en :ref:`rest_generator`. Hay que llamar a un par de plugins y se declara el sistema de **logs**.

.. code-block:: ini

   restConfig.path = APPLICATION_PATH "/modules/rest/"
   restConfig.usersAuthTable = "KlearUsers"
   restConfig.fieldUsername = "login"
   restConfig.fieldPassword = "pass"
   
   resources.frontController.plugins.authRest = "{{namespace}}_Controller_Plugin_Auth"
   resources.frontController.plugins.paramsRest = "Iron_Plugin_RestParamsParser"
   resources.frontController.moduleDirectory.modules = APPLICATION_PATH "/modules"
   
   restLog.log.access.syslog.writerName = "Syslog"
   restLog.log.access.syslog.writerParams.application = "api-rest"
   restLog.log.access.syslog.writerParams.facility = LOG_SYSLOG
   
   restLog.log.error.syslog.writerName = "Syslog"
   restLog.log.error.syslog.writerParams.application = "api-rest"
   restLog.log.error.syslog.writerParams.facility = LOG_SYSLOG


* **path**: la ruta del modulo **REST**. **(Aunque no se use el generador este parametro es obligatorio)**.
* **usersAuthTable**: Nombre de la table de autenticacón
* **fieldUsername**: Campo de identificacón del usuario
* **fieldPassword**: Campo de la contraseña del usuario
