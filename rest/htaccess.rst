.htaccess
---------

En necesario que **apache** tenga activo el modulo de **Headers**, para especificar que metodos y que parametros se van a usar. 

.. code-block:: console

   Header set Access-Control-Allow-Methods "GET, PUT, POST, OPTIONS, DELETE"
   Header set Access-Control-Allow-Credentials "true"
   Header set Access-Control-Allow-Headers "Authorization, Origin, Content-Type, X-CSRF-Token, page, Request-Date"
   Header set Access-Control-Expose-Headers "totalItem"

* **Access-Control-Allow-Methods**: Se declaran los metodos que se van a usar.
* **Access-Control-Allow-Credentials**: Se activa el uso de credenciales de autenticación
* **Access-Control-Allow-Headers**: Se declaran los parametros que se van a recibir en los **Headers**
* **Access-Control-Expose-Headers**: Se declaran los parametros que se van a enviar en los **Headers**