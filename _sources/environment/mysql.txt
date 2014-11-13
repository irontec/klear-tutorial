MySQL
=====

Para instalar **MySQL** en la terminal:

.. code-block:: console

   # apt-get install mysql-server-5.5 mysql-client-5.5 libmysqlclient-dev

.. note:: 

   Durante la instalación el sistema pide que se configure la password de root de **MySQL**.
   Será necesario acordarse de la misma para poder acceder mas adelante.

Para probar la instalación, desde la terminal:

.. code-block:: console

   //XXXX será la contraseña especificada anteriormente
   $ mysql -uroot -pXXXX