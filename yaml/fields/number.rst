======
number
======

* :ref:`Volver a Tipos de Campos <tipos_de_campos>`

Campo numérico.

Configuración
=============

.. code-block:: yaml

   fieldName:
     type: number
       source:
         control: spinner
         max: 100
         min: 0
         step: 1
         preconfiguredValues:
           - 10
           - 20
           - 30
           - 40
           - 50

* **control**: si está a **spinner** se muestran flechas para subir y bajar el valor del campo.
* **max**: valor máximo del **spinner**.
* **min**: valor mínimo del **spinner**.
* **step**: cuanto avanza el **spinner** cada vez que clicamos en las flechas.
* **preconfiguredValues**: valores preconfigurados que se mostrarán en botones a la derecha del spinner