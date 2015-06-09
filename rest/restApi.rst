restApi.ini
-----------

Este fichero de configuración, se encarga de definir que metodos requieren de autenticación, por defecto todo estan a **true**

.. code-block:: ini

   defaultPolicy.index.authorization = true
   defaultPolicy.get.authorization = true
   defaultPolicy.put.authorization = true
   defaultPolicy.post.authorization = true
   defaultPolicy.delete.authorization = true
   
Si por alguna razón, hay un metodo de una instancia en concreto que se quiera dejar publica (**Sin autenticación**), con poner el nombre de controlador y el motodo a **false** quedaria publico.

Ejemplo:

.. code-block:: ini

   autores.index.authorization = false
   autores.get.authorization = false
   autores.put.authorization = true

Con esta configuración, la instancia de permetiria lista todos los autores (**index**) o ver uno (**get**) desde cualquier petición, pero al intentar actualizar (**put**) pediria una autenticación.