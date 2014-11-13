===
map
===

* :ref:`Volver a Tipos de Campos <tipos_de_campos>`

El campo **mapa** se utiliza para guardar las direcciones de una forma
mas visual.

.. contents::
   :local:
   :depth: 3

Requisitos
==========

El campo de la base de datos deberá tener el tag **[map]**.

Con el tag seteado, el **generador de base** de datos creará los campos necesarios
(*campoLat* y *campoLng*) ,y el **generador de yaml** generará el código necesario para
mostrar el mapa.

Configuración
=============

.. code-block:: yaml

   fieldName:
     title: título
     type: map
       source: #optional
         settings:
           width: 550
           height: 400 
           zoom: 10 
           previewZoom: 9 
           previewWidth: 80
           previewHeight: 80
           defaultLat: 41.40918
           defaultLng: 2.225461

* **width**: ancho del mapa en las pantallas **Edit** y **New**.
* **height**: alto del mapa en las pantallas **Edit** y **New**.
* **zoom**: zoom del mapa en las pantallas **Edit** y **New**.
* **previewZoom**: zoom por defecto en la pantalla **List**.
* **previewWidth**: ancho por defecto en la pantalla **List**.
* **previewHeight**: alto por defecto en la pantalla **List**.
* **defaultLat**: latitud por defecto.
* **defaultLng**: longitud por defecto.

.. attention:: 
   
   El **preview** muestra una imagen en lugar del mapa real en el listado.
   
   Si el campo está a **readOnly**, en el **Edit** también se muestra una imágen.