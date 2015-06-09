Controllers
-----------

Los controladores que crea el generador, extendienden de "**Iron_Controller_Rest_BaseController**" que es un controlador en "**library/Iron**" el cual implementa una serie de acciones, que facilitan las respuestas y para hacer **logs**.

Aparte de extender del "**BaseController**", traen todos los metodos con logicas funcionales.

* [**index**] Este es el metodo con más tareas, las cuales son:
   * Se limitan los resultados por petición, los cuales estan defenidos en la variable "**_limitPage**".
   * Para hacer el uso de la paginación espera el parametro **page** con el número de paginación que se quiere. por defecto lista la página **1**.
   * El order por defecto es "**primaryKey DESC**", pero tambien se espera el parametro "**order**" para cambiarlo.
   * Para hacer busquedas, se espera el parametro "**search**"
   * Y como opción extra espera el parametro **fields**, el cual permite elegir que campos se enviaran en la respuesta separados por coma "**,**"
   * En el resultado final, se envia en los **Headers** el parametro "**totalItems**" con el número total de elementos (sin tener en cuenta la limitación).
* [**get**] Envia el recurso que se a pedido. Tambien soporta el parametro **fields** 
* [**post**] Crea un recurso en base a los parametros que se envíen.
* [**put**] Actualiza un recurso, en base al **primaryKey**.
* [**delete**] Elimina un recuro, en base al **primaryKey**.

Todos estos metodos aplican los codigo de estado HTTP que correspondan. (**200, 201, 204, 404,** ...).