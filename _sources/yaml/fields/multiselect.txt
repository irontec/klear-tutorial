multiselect
===========

* Volver a :ref:`tipos_de_campos`

.. contents::
   :local:
   :depth: 3

Con este campo podemos vincular o relacionar nuestro datos o otros datos que se encuentran en nuestra base de datos. Para ello debe haber 3 Tablas:
**Tabla1**, **Tabla2** y otra tabla que las relacione como **relationTabla1Tabla2**.

.. attention::
   Si hay un parámetro mal configurado puede dar a errores con la posibilidad de que el YAML LIST donde se encuentra el campo no se abre o no se
   llegue a guardar correctamente. 

.. code-block:: yaml

    RelationEntity: 
      type: multiselect
      source:
        data: mapper
        config:
          relationMapper: \MyApp\Mapper\Sql\RelationEntity
          relatedMapperName: \MyApp\Mapper\Sql\Entity
          relatedFieldName: nameEntity
          relationProperty: Entity

*Suponemos que añadimos este campo multiselect al YAML MODEL de la Tabla1*

* **RelationEntity**: Nombre de la Relación *(relationTabla1Tabla2)*
* **relatedMapperName**: Mapper de la tabla de Relaciones (n-m) *(relationTabla1Tabla2)*
* **relationMapper**: Mapper de la tabla con quien se relaciona *(Tabla2)*
* **relatedFieldName**: Nombre de un campo de la tabla con quien se relaciona para mostrar sus valores *(Un nombre de uno de los campos de la Tabla2)*
* **relationProperty**: Este valor lo encontramos en \MyApp\Model\Raw\RelationEntity. Copia solo el texto que viene después de **"$_"**. Indicamos su ubicación en la línea resaltada:

.. code-block:: php
   :emphasize-lines: 8
   
   <?php
    /**
     * Parent relation RelationEntity_ibfk_2
     *
     * @var \MyApp\Model\Raw\RelationEntity
     */
     
    protected $_Entity;
    

Ejemplo KlearIntervals
----------------------

El siguiente ejemplo pertenece a la tabla **KlearUsers**, **KlearRoles** y su tabla que las relaciona **KlearUsersRoles**. 

.. code-block:: php
   
    KlearUsersRoles:
      title: _('Roles')
      type: multiselect
      source:
        data: mapper
        config:
          relationMapper: \KlearInterval\Mapper\Sql\KlearUsersRoles
          relatedMapperName: \KlearInterval\Mapper\Sql\KlearRoles
          relatedFieldName: name #Nombre de un campo de la tabla Roles
          relationProperty: KlearRole

.. _ejemploFilterClass:

Ejemplo filterClass
-------------------

El filterClass es una clase para filtrar los valores que nos ofrece el multiselect. Por ejemplo, si no queremos que nos aparezca
todos los valores de una tabla pues podemos colocar una condición y los valores que la cumpla pues se mostrará en el listado
del multiselect.

.. code-block:: yaml
   :emphasize-lines: 10

    RelationEntity: 
      type: multiselect
      source:
        data: mapper
        config:
          relationMapper: \MyApp\Mapper\Sql\RelationEntity
          relatedMapperName: \MyApp\Mapper\Sql\Entity
          relatedFieldName: nameEntity
          relationProperty: Entity
          filterClass: Application_Filter_Name
          
.. note::
   Por formalidad, el siguiente archivo php lo solemos crear en nuestra carpeta **library/Applicationlib/Filter**.


.. code-block:: php
   :linenos:
   :emphasize-lines: 3,5,7-23,25-31

   <?php
   
   class Application_Filter_Name implements KlearMatrix_Model_Field_Select_Filter_Interface
   {
       protected $_condition = array();
   
       public function setRouteDispatcher(KlearMatrix_Model_RouteDispatcher $routeDispatcher)
       {
           //Get Action
           $currentAction = $routeDispatcher->getActionName();
           
           //Get Controller
           $currentController = $routeDispatcher->getControllerName();
           
           //Get ModelName and your Controller
           $currentItemName = $routeDispatcher->getCurrentItemName();
           
           //NUESTRA CONDICIÓN CON CODIO WHERE MYSQL
           $this->_condition[] = "active = 1"; 
           //En este ejemplo decimos que solo muestre los valores cuyo campo Active = 1
           
           return true;
       }
   
       public function getCondition()
       {
           if (count($this->_condition) > 0) {
               return '(' . implode(" AND ", $this->_condition) . ')';
           }
           return;
       } 
       
   }

Ejemplo autocomplete
--------------------

Para incorporar el autocomplete en nuestro campo multiselect, hay que configurar los siguientes códigos:

En el **YAML MODEL** *model.yaml*

.. code-block:: yaml
   :emphasize-lines: 10-12

    RelationEntity: 
      type: multiselect
      source:
        data: mapper
        config:
          relationMapper: \MyApp\Mapper\Sql\RelationEntity
          relatedMapperName: \MyApp\Mapper\Sql\Entity
          relatedFieldName: nameEntity
          relationProperty: Entity
      decorators:
        autocomplete:
          command: autocomplete_command
          
En el **YAML LIST** que usará el campo select autocomplete, la sección **"commands"** debe contener lo siguiente:

.. code-block:: yaml
   :emphasize-lines: 2-17

   commands:
     autocomplete_command:
       <<: *Entity
       controller: field-decorator
       action: index
       autocomplete:
         filterClass: Filter_Class_Name
         condition: 'active = 1'
         mapperName: \MyApp\Mapper\Sql\Entity
         label: nameFieldShow
         fieldName:
           fields:
             - name
             - nif
           template: '%name% [%nif%]'
         limit: 8
         order: MAC

Propiedades Comandos
^^^^^^^^^^^^^^^^^^^^

* **filterClass**: Clase por la cual se filtra el resultado, **no funciona la que está en el model**, hay que definirlo dentro del command del autocomplete también. :ref:`ejemploFilterClass`

* **condition**: Condición que se mete como un where Mysql. Si **filterClass** está también definido, saltará una excepción. **Únicamente debe definirse uno de los dos**.

* **mapperName**: El mismo del relatedMapperName, la entidad donde se buscará los valores.

* **label**: El mismo de “relatedFieldName”, listará los valores de dicho campo.

* **fieldName**: Si queremos mostrar más de un campo como label, se pueden definir varios y un template, igual que en la configuración de un select.

* **limit**: Número máximo de elementos que salen en el autocompletado (sale un campito al lado diciendo cuántos elementos hay en total).

* **order**: Campo por le cual se ordena el listado. Se puede añadir mas de un campo separado por **”,”**.

.. image:: img/multiselect-autocomplete.png