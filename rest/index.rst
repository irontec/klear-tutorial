====
REST
====

Para automatizar la creación de aplicaciones **REST**, se creo un nuevo generador "**klear-rest-generator.php**. El cual busca las tablas que tengan el comentario **[rest]** para crear un controlador **REST** con metodos preestablecidos, con el fin de agilizar la construnccióin de esta.
Aparte de generar los controladores **REST** tambien crea un plugin de autenticación por **Headers** y un sistema para la documentación de aplicaciones **rest**.

.. toctree::
   :maxdepth: 3
   
   generators
   application
   bootstrap
   auth
   restApi
   htaccess
   controllers
   docs