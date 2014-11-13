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