PHP
===

.. contents::
   :local:
   :depth: 3

Instalación
-----------

En la terminal escribimos el siguiente comando para instalar **PHP** y los paquetes
que necesitamos para arrancar Klear:

.. code-block:: console

   # apt-get install php5 libapache2-mod-php5 php5-cli php5-mysql php5-mcrypt php5-curl php-pear php5-dev libyaml-dev php5-imagick imagemagick

Modificamos el archivo /etc/php5/apache2/php.ini para permitir subir archivos desde Klear:

.. code-block:: ini

   ...
   post_max_size = 20M
   ...
   upload_max_filesize = 20M
   ...

PHP ya está instalado en el sistema, ahora tendremos que recargar **Apache2** para que se
haga eco del cambio. Para ello:

.. code-block:: console

   # service apache2 restart

Para probar la instalación reciente, creamos un script de PHP.
Desde la terminal:

.. code-block:: console

   # vim /var/www/info.php

Y añadimos el siguiente contenido al fichero:

.. code-block:: php
   :linenos:
   
   <?php
      phpinfo();
   ?>

Para ver el fichero que hemos generado nos dirigimos a `http://localhost/info.php <http://localhost/info.php>`_,
si todo ha ido bien se verá la información de la configuración del PHP.

.. caution:: 

   Si se ha generado el archivo con otro nombre al especificado en el ejemplo, se debe cambiar la dirección de la URL
   para ver el resultado. 

YAML
----

Para poder leer los archivos de configuración del tipo **YAML** hace falta instalar la extensión para **PHP**.

Para ello es mejor hacerlo vía **PECL**, desde la terminal:

.. code-block:: console

   # pecl install yaml

Usar opción por defecto [autodetect]

Para asegurarnos de que los modulos que necesitamos están ya instalados, en la terminal:

.. code-block:: console

   $ php -m
   [PHP Modules]
   bcmath
   bz2
   calendar
   Core
   ctype
   ...

Este es listado de los modulos de **PHP** instalados en el sistema.

.. attention:: 

   Si en el listado anterior no se muestra el recién instalado módulo **yaml**
   habrá que crear un archivo de configuración .ini para que cargue el módulo.
   
   .. code-block:: console
       
      # echo "extension=yaml.so" | tee /etc/php5/conf.d/yaml.ini /etc/php5/cli/conf.d/yaml.ini /etc/php5/apache2/conf.d/yaml.ini >/dev/null