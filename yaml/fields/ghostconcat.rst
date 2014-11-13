============
ghost concat
============

* :ref:`Volver a Tipos de Campos <tipos_de_campos>`

Los campos del tipo **ghost concat** se utilizan para mostrar un grupo
de campos juntos, como si solo fueran uno.

Configuración
=============

El campo final se crea uniendo todos los campos deseados.
Cada campo puede tener su propio template definido.

Los campos se pintan en el orden que están definidos.

.. code-block:: yaml

   nombreAleatorio:
     title: _('Nombre Aleatorio')
     type: ghost
     source:
       predefined: concat
       template: 
         who: '%who%' 
         email:
           literal: '<br />email: <a href="mailto:%email%">%email%</a>'
           checkEmpty: true
         url:
           literal: '<br />url: <a href="%url%">%url%</a>'
           checkEmpty: true
         ip:
           literal: '<br /><em>IP: %ip%</em>'
           noSearch: true

* **noSearch**: a **true** para omitir el campo en la búsqueda.
* **checkEmpty**: si un campo está vacío, se puede omitir poniendo a **true**

.. attention:: 

   **checkEmpty** también ocultara aquellos campos que sean **== 0**