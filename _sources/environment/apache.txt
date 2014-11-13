Apache
======

.. contents::
   :local:
   :depth: 3

Instalación
-----------

Abrimos un terminal y lanzamos el siguiente comando:

.. code-block:: console

   # apt-get install apache2

Con esto se ha instalado **Apache2** en la maquina.

Una vez instalado el servidor se inicia automaticamente, pero si se necesita
arrancarlo/pararlo habría que usar los siguientes comandos:

.. code-block:: console

   //Para arrancarlo
   # service apache2 start

   //Para pararlo
   # service apache2 stop

   //Para reiniciarlo
   # service apache2 restart

.. attention::

   El directorio donde se almacenan los documentos web por defecto es:

   .. code-block:: console

      $ /var/www

Si todos los pasos anteriores han salido satisfactoriamente, debe verse una página ordinaria
accediendo a `http://localhost <http://localhost>`_ en el navegador

Configuración básica
--------------------

Para que Klear funcione necesitamos activar el módulo **rewrite**, para ello
abrimos la terminal y escribimos:

.. code-block:: console

   # a2enmod rewrite

Esto activa el módulo mencionado anteriormente. Aparte de activarlo hay que configurar
**Apache2** para que pueda usarlo. Para ello desde terminal:

.. code-block:: console

   # vim /etc/apache2/sites-available/miSitio.conf

Dentro del fichero hay que añadir el siguiente contenido (cambiando los nombres de archivos, email y demas):

.. code-block:: apacheconf

   <VirtualHost *:80>
       ServerAdmin usuario@empresa.com
       DocumentRoot /var/www/

       <Directory "/var/www/">
           Options +Indexes -MultiViews +FollowSymLinks
           AllowOverride All
       </Directory>

       <DirectoryMatch .*\.svn/.*>
           Deny From All
       </DirectoryMatch>

       ServerName usuarioX.empresa.com
       ServerAlias usuarioX
       ServerAlias usuarioX.empresa.com
       ErrorLog /var/log/apache2/error.usuarioX.log
       CustomLog /var/log/apache2/access.usuarioX.log combined env=!client-ip-request
       CustomLog /var/log/apache2/access.usuarioX.log proxy env=client-ip-request
   </VirtualHost>


Una vez añadido el fichero, hay que activarlo, para ello desde terminal:

.. code-block:: console

   # a2ensite miSitio.conf

.. note::

   El nombre del sitio a activar viene definido por el nombre de archivo **.conf**.

   Por lo tanto si se cambia el nombre del fichero a la hora de hacer **a2ensite**, este será el que deberemos activar.

.. seealso::

   Para una configuración mas personalizada accede a la siguiente dirección: `Apache2 <https://httpd.apache.org/docs/2.0/es/>`_
