Zend
====

.. contents::
   :local:
   :depth: 3

Instalación
-----------

Para empezar a instalar el Zend Framework, abrimos un terminal y empezamos:

1. Instalamos Zend Framework

.. code-block:: console

   # apt-get install zend-framework zend-framework-bin
   
.. attention::

   Comprobar que la versión que se va a instalar no sea la 2.xx.x ya que no es compatible con klear.

   .. code-block:: console

      # apt-cache policy zend-framework

   A la hora de crear este manual la versión que instala el repositorio de ubuntu es la 1.11.1

2. Ahora nos aseguramos que nuestro PHP tenga en cuenta las librerías de PHP y Zend quitando el **';'** del archivo zend-framework.ini
que se encuentra en las siguientes direcciones:

   * /etc/php5/conf.d/zend-framework.ini
   * /etc/php5/apache2/conf.d/zend-framework.ini
   * /etc/php5/cli/conf.d/zend-framework.ini

.. note::

   Según la versión de Ubuntu instalada es posible que los directorios /etc/php5/apache2/conf.d/ y /etc/php5/cli/conf.d/ sean enlaces al directorio /etc/php5/conf.d/. Si este es el caso basta con modificar el archivo /etc/php5/conf.d/zend-framework.ini ya que los otros dos apuntan a este.

.. code-block:: console

   # vim /etc/php5/conf.d/zend-framework.ini
   
.. code-block:: ini

   ; include_path=${include_path} “:/usr/share/php/libzend-framework-php”
   
3. Reinciamos apache

.. code-block:: console

   # service apache2 restart
   
4. Podemos ver la versión de nuestro Zend con el siguiente comando en nuestro terminal.

.. code-block:: console

   $ zf show version

.. note:: 
   Si la versión no corresponde como mínimo al 1.12 tienes que actualizarlo manualmente desde la web oficial de Zend Framework
   para evitar cualquier problema con el Klear o proyectos ya creados por Irontec.


Actualización del Zend Framework
--------------------------------

1. Backup de nuestro Zend para volver a empezar si surge algún problema.
   
.. code-block:: console

   # mv /usr/share/php/libzend-framework-php /usr/share/php/libzend-framework-php.backup

2. Descargamos la versión a instalar:
   
.. code-block:: console
   
   # wget https://packages.zendframework.com/releases/ZendFramework-1.12.5/ZendFramework-1.12.5-minimal.tar.gz

3. Extraemos el tar gz:

.. code-block:: console

   # tar -xzf ZendFramework-1.12.5-minimal.tar.gz -C /tmp

4. Copiamos las librerías al directorio

.. code-block:: console
   
   # cp -r /tmp/ZendFramework-1.12.5-minimal/library/Zend /usr/share/php/libzend-framework-php/

5. Borramos los archivos que no necesitamos ya

..  code-block:: console

   # rm -R /tmp/ZendFramework-1.12.5-minimal
   
6. Ahora comprobamos nuestra versión del Zend.

.. code-block:: console

   $ zf show version
   
.. note::
   Si llegara a surgir algún error, comprobamos los permisos de las carpetas y la ponemos como la copia de seguridad.