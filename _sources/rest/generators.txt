.. _rest_generator:

Generadores
-----------

Si se quiere hacer de forma automatica la base de una **Api REST**, phing cuenta con una tarea llamada "**generate-rest-controllers**"
la cual necesita de un par de parametros en el **application.ini** y que las tablas que tendran controladores **REST** tengan el comentario "**[rest]**".

.. code-block:: ini

   restConfig.path = APPLICATION_PATH "/modules/rest/"
   restConfig.usersAuthTable = "KlearUsers"
   restConfig.fieldUsername = "login"
   restConfig.fieldPassword = "pass"
   
* **path**: la ruta del modulo **REST**. **(Aunque no se use el generador este parametro es obligatorio)**.
* **usersAuthTable**: Nombre de la table de autenticacón
* **fieldUsername**: Campo de identificacón del usuario
* **fieldPassword**: Campo de la contraseña del usuario
   
Entre las cosas que automatisa este generador, están: 
 * La generación de controladores con basicos con metodos funcionales
 * En cada metodo una serie de comentario, para una documentación con la librería "**crada/php-apidoc**"
 * Un plugin de autenticación por **HEADER**, que actualmente da soporte para **Basic** y **HMAC**
 * El archivo "**restApi.ini**" que define que metodos necesitan autenticación