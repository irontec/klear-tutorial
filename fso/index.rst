===
FSO
===

Este es un modulo, que esta en **Iron** (https://github.com/irontec/klear-library) y se encarga de servir toda clase de ficheros, haciendo uso de Zend_Cache para optimizar el servicio.
Enviando las cabeceras correspondientes para esto. De la misma forma, está integrado con los modelos para servir las url finales de forma fácil con la función **$model->get{Fso}Url('profile')**. 
A la hora de configurar cada **profile** especifica como se va a servir el fichero: de una forma básica (**binary**), la cual es una descarga del mismo, o de forma especial (**image**), la cual genera la imagen con la posibilidad de tratarla antes de visualizarla (resize, crop, ...).

Para que los ficheros se puedan almacenar en la base de datos, la clave primaria de la tabla en la que se almacenan no puede ser **[uuid]**. En su lugar, se dispone del tag **[uuid:php]**.


Para activar este modulo, en el application.ini se necesitan esta 2 lineas:

.. code-block:: ini

   resources.frontController.moduleDirectory.IronModules = "/opt/klear/library/Iron/modules/"
   IronModule.fso = true


La configuración de este modulo se hace en el archivo **fso.ini** situado en **application/config/**.

.. contents::
   :local:
   :depth: 3

fso.ini
-------

En este archivo se declaran los **perfiles** que son los encargados de tener la configuración que tendra cada fso. Los perfiles tienen que ser unicos y con nombres claros, ya que tambien forman parte de la **URL** del archio.

Ejemplo de un **fso.ini** basico.

.. code-block:: ini

   [production]
   
   config.life = 9999999999
   config.routeMap = {id}-{basename}
   
   autores.model = Authors
   autores.fso = img
   autores.type = image
   
   autoresImg.extend = autores
   autoresImg.changeSize = crop
   autoresImg.width = 64
   autoresImg.height = 64
   
   autoresFile.model = Authors
   autoresFile.fso = file
   autoresFile.type = binary
   
   [development:production]
   [localdev:production]


config.life
-----------

Tiempo de vida, para el fichero en cache.


config.routeMap
---------------

Este parametro es importante, ya que con el se buscara el archivo que se va a servir.

El uso normal seria:

.. code-block:: ini

   config.routeMap = {id}-{basename}

* **id**: primaryKey del modelo
* **basename**: Nombre real del fichero

Pero tambien se puede puede forzar el uso de una extención en concreto.

.. attention::
   **Solo usar con imagenes**

.. code-block:: ini

   config.routeMap = {id}-{name}.{ext}

Para cambiar la ruta de las imágenes, hay que cambiar el config.routeMap y, en el Model de la entidad, hay que modificar el método getFileUrl(), donde File es el nombre de la columna del fichero

.. attention::
   **Solo usar si el nombre de fichero es único**

.. code-block:: php

   <?php
   $route = array(
      'profile' => $profile,
      'routeMap' => $this->getIconBaseName()
   );

.. code-block:: ini

   config.routeMap = {basename}



Perfil Binay
------------

Ejemplo de un perfil **Binary**:

.. code-block:: ini

   autoresFile.model = Authors
   autoresFile.fso = file
   autoresFile.type = binary

* **type**: Establece como se va a tratar el archivo (**binary/image**).
* **model**: Nombre del modelo o tabla donde se esta la información de este contenido.
* **fso**: El nombre original del campo antes de pasar los generadores. Si el campo ahora se llama **fileBaseName** el fso sera **file**.


Perfil Basico de Image
----------------------

Ejemplo de un perfil **Image** basico:

.. code-block:: ini
   
   autoresImage.model = Authors
   autoresImage.fso = img
   autoresImage.type = image
   autoresImage.changeSize = crop
   autoresImage.width = 100
   autoresImage.height = 100
   
Los 3 primero parametros son iguales a los del perfil **binary** (**model, fso y type**) con la diferencia que **type** cambia a **image**.

El 4 parametros "**changeSize**" es el clave y obligatorio para el tratado de las imágenes.

Los demas parametros son los requeridos por cada tipo de **changeSize**.

Tipos de **changeSize**:

* original
* scale
* crop
* resize
* resize-crop
* crop-resize
* circle

Ejemplos de uso en **Perfiles avanzados de Image**.

Comprimir calidad de imagen
---------------------------

El parametro **compressionQuality** establece la calidad de compresión para la imagen que se va a mostrar.


.. code-block:: ini

   autores.model = Authors
   autores.fso = img
   autores.type = image
   autores.compressionQuality = 80
   autores.changeSize = crop
   autores.width = 100
   autores.height = 100

Perfil Extendido
----------------

Extender pefiles es enfocado para las imagenes, ya que se puede querar una misma imagen con diferentes tamaños o formatos. Para no tener que declarar una y otra vez el **model, fso** y **type** se usa el **extend** y nombre del perfil, lo que hace que tome las propiedadel perfil mensionado.

Ejemplo de **extend**:

.. code-block:: ini

   autores.model = Authors
   autores.fso = img
   autores.type = image
   
   autoresCrop.extend = autores
   autoresCrop.changeSize = crop
   autoresCrop.compressionQuality = 80
   autoresCrop.width = 100
   autoresCrop.height = 100
   
   autoresResize.extend = autores
   autoresResize.changeSize = resize
   autoresResize.width = 64
   autoresResize.height = 64

Perfiles avanzados de Image
---------------------------

.. code-block:: ini

   ;+------+
   ;|Image |
   ;+------+
   profile.model = Model
   profile.fso = fso
   profile.type = image
   
   ;+--------------+
   ;| Image Circle |
   ;+--------------+
   circle.extend = profile
   circle.changeSize = 'circle'
   circle.size = 600
   
   ;+--------------+
   ;| Image Resize |
   ;+--------------+
   resize.extend = profile
   resize.changeSize = 'resize'
   resize.width = 500
   resize.height = 280
   
   ;+------------+
   ;| Image Crop |
   ;+------------+
   crop.extend = profile
   crop.changeSize = 'crop'
   crop.width = 658
   crop.height = 472
   
   ;+-------------------+
   ;| Image Crop-Resize |
   ;+-------------------+
   cropResize.extend = profile
   cropResize.changeSize = 'crop-resize'
   cropResize.width = 280
   cropResize.height = 150
   
   ;+-------------------+
   ;| Image Resize-Crop |
   ;+-------------------+
   resizeCrop.extend = profile
   resizeCrop.changeSize = 'resize-crop'
   resizeCrop.width = 280
   resizeCrop.height = 150
   
   ;+-------------+
   ;| Image Scale |
   ;+-------------+
   scale.extend = profile
   scale.changeSize = 'scale'
   scale.width = 350
   scale.height = 280
   
   ;+----------------------------------+
   ;| Image Otras opciones adicionales |
   ;+----------------------------------+
   original.extend = profile
   original.changeSize = 'original'
   
   original.negate = 'yes'; Pasa los colores a negativos
   
   original.flop = 'yes'; Invierte la imagen de derecha a izquierda
   
   original.vignette.blackPoint = 10
   original.vignette.whitePoint = 10
   original.vignette.x = 10
   original.vignette.y = 10
   
   
   original.border.color = 'rgba(0,0,0,.7)'; //Crea un borde al rededor de la imagen con transparencia
   ;original.border.color = '#A6F918'; //Crea un borde al rededor de la imagen
   original.border.width = 15
   original.border.height = 15
   
   original.framing.color = 'rgba(0,0,0,.7)'; //Crea un bonito enmarcado al rededor de la imagen con transparencia
   ;original.framing.color = '#d353d3'; //Crea un bonito enmarcado al rededor de la imagen
   original.framing.width = 5
   original.framing.height = 5
   original.framing.innerBevel = 3
   original.framing.outerBevel = 3
   
