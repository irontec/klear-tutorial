==========
ghost list
==========

* :ref:`Volver a Tipos de Campos <tipos_de_campos>`

Los campos del tipo **ghost list** se utiliza en entidades que tenga una relación 1-n. De esta manera se podrá listar todos los valores que se encuentran en la otra entidad para editarlos o borrarlos.

Configuración
=============

Se necesita configurar de la siguiente manera:

.. code-block:: yaml
   :emphasize-lines: 4-17

    nombreAleatorio:
      title: _("title")
      type: ghost
      source:
        predefined: list
      data: mapper
      config:
        mapperName: \Application\Mapper\Sql\Entity
        filterField: fieldId
        fieldName: name
        order: name_${lang}
        forcedValues:
          active: 1
        options:
          dialogs: 
            entityDel_dialog: true
          screens: 
            entityEdit_screen: true
          default: entityEdit_screen


* **mapperName**: la entidad con la cuál está vinculada
* **filterField**: el campo id para listar solo los valores de la entidad padre
* **fieldName**: valor que se mostrará en el listado del campo de la entidad vinculada
* **order**: orden en el que se listará los valores
* **forcedValues**: (opcional) valor que debe cumplir para ser listado
* **options**: (opcional) se requiere que entityDel_dialog y entityEdit_screen estén declarados en el yaml list.

ghost list as table
===================

Permite la funcionalidad de un ghost list pero mostrando los datos en formato tabla.

Se puede seguir usando la forma anterior y simplemente añadiendo lo siguiente ya se muestra como una tabla.

.. code-block:: yaml
   :emphasize-lines: 10

    nombreAleatorio:
      title: _("title")
      type: ghost
      source:
        predefined: list
      data: mapper
      config:
        mapperName: \Application\Mapper\Sql\Entity
        filterField: fieldId
        showAsTable: true
        fieldName: name
        order: name_${lang}
        forcedValues:
          active: 1
        options:
          dialogs: 
            entityDel_dialog: true
          screens: 
            entityEdit_screen: true
          default: entityEdit_screen

Pero así usa los nombres de los campos como encabezados de columnas y sin poder cambiar el nombre.
Si se quiere usar como tabla se recomienda usar la siguiente configuración que permite más cosas:

.. code-block:: yaml
   :emphasize-lines: 7-23
   
    nombreAleatorio:
      title: _("title")
      type: ghost
      source:
        predefined: list
      data: mapper
      order:
        - campoOrdenar1
        - campoOrdenar2
      config:
        mapperName: \proyect\Mapper\Sql\Fields
        filterField: fieldId
        showAsTable: true
        fieldName:
          fields:
            field1:
              title: _("Field 1")
            otherTableFieldId: //Foreign key que hace referencia al campo id de la tabla otherTable
              title: _("Other Field")
              mapperName: \labayru\Mapper\Sql\OtherTable
              field:
                - otherField1
                - otherField2
              pattern: "%otherField1% - %otherField2%"
            field3:
              title: _("Field 3")

De esta manera podemos incluir el valor de el campo que elijamos de una tabla referenciada.
En el caso de campos ML, se muestra el valor del campo del idioma actual.
El resto de opciones se puede seguir usando de la misma manera. 