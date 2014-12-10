Configuración lists
===================

.. contents::
   :local:
   :depth: 3


A parte de las configuraciones descritas en :ref:`configuracion_general`, el controller
del tipo **list** tiene otras configuraciones propias que se definen a continuación:

------

Configuración
-------------

.. code-block:: yaml

   screens:
    Contacts:
      controller: list
      <<: *Contactos
      title: _("Listado de contactos")
      filterClass: Filter_Class_Name
      rawSelect:
        "FROM Table t INNER JOIN
        (SELECT column1, column2, column3, column4
        FROM Table GROUP BY column1, column2, column3, column4 HAVING count(id) > 1) dup
        ON t.column1 = dup.column1 and t.column2 = dup.column2 and t.column3 = dup.column3 and t.column4 = dup.column4"
      searchAlias: t
      rawCondition: "category in ('category1', 'category2')"
      order:
        field:
          - firstName
          - t.column1
        type: desc
      pagination:
        items: 40
      preconfiguredFilters:
        ana:
          title: _("Ana")
          field: firstName
          value: "Ana"
          description: _("Utiliza este pre filtrado para buscar a Ana")
      presettedFilters:
        Etiqueta: 
          field: status 
          value: 
            - 'hidden'
            - 'waiting'
          op: 'eq' 
      fields:
        options:
          title: _("Opciones")
          screens:
            editContact: true
          dialogs:
            deleteContact: true
            customDialogContact: true
          default: editContact

rawSelect
---------

Permite que un ListScreen muestre los resultados de una SELECT escrita a mano.

  .. code-block:: yaml

     screens:
      Contacts:
        controller: list
        <<: *Contactos
        title: _("Listado de contactos")
        filterClass: Filter_Class_Name
        rawSelect:
          "FROM Table t INNER JOIN
          (SELECT column1, column2, column3, column4
          FROM Table GROUP BY column1, column2, column3, column4 HAVING count(id) > 1) dup
          ON t.column1 = dup.column1 and t.column2 = dup.column2 and t.column3 = dup.column3 and t.column4 = dup.column4"
        searchAlias: t
        order:
          field:
            - t.column1
            - t.column2
            - t.column3
          type: desc
          
En este ejemplo se muestra una SELECT que busca registros duplicados en la tabla Table comparando los campos column1, column2, column3 y column4.

La select se debe introducir desde la palabra FROM, es decir, no se debe usar el SELECT columName ni SELECT *.
Tampoco se debe usar WHERE ni LIMIT ni OFFSET, ya que LIMIT Y OFFSET los controla Klear en el ListScreen y el WHERE se puede meter como siempre en un 'rawCondition'.

La opción searchAlias es opcional y se usa solo en casos como este ejemplo en los que a la tabla se le da un alias (t). 
En este caso si no se pone searchAlias: t no funcionarán los filtros ni las búsquedas de klear.

rawCondition
------------

Permite escribir una condición directamente.

order
-----

Indica el campo o campos por los que se ordena el listado, también el orden.
Hay dos formas posibles de ordenación.

1. Orden por un único campo:

  .. code-block:: yaml

     order: firstName
     orderType: desc

* **order**: campo por el que se ordena.
* **orderType**: orden a seguir. Puede ser *ASC* o *DESC*, por defecto **ASC**.

2. Orden por mas de un campo:

  .. code-block:: yaml

     order:
       field:
         firstName: true
         lastName: true
       type: desc

* **field**: indica el campo o campos por los que ordenar. Puede ser uno solo de forma *field:
  firstName* o varios como está en el ejemplo.
* **type**: indica el orden en que se ordena, igual para todos los campos que vayan en field.
  Puede ser *asc* o *desc*. No es obligatorio y por defecto es lo que considere mysql.

pagination
----------

Se puede definir el límite de items por página.

* **items**: número de elementos que se muestran por página.

csv
---

Activar la opción de **CSV** genera un botón, el cual nos devuelve un *csv* para descarga
con el listado actual.

.. code-block:: yaml
   
   csv: true

La opción de **CSV** tiene la posibilidad de añadir propiedades avanzadas. En el siguiente
ejemplo se muestran todas:

.. code-block:: yaml

   csv:
     active: true
     filename: "ArchivoCSV"
     headers: true
     enclosure: '"'
     separator: ";"
     nameklear: true
     ignoreBlackList: true
     newLine: "\r\n" windows | "\r" mac | "\n" unix
     encoding: "utf-8" | "iso-8859-1"
     executionSeconds: "n"



* **executionSeconds**: Setea dinámicamente el max_execution time cuando alquien descarga csv.

autoRefresh
----------- 

La opción **autoRefresh** nos permite refrescar el listado cada 30 segundos. Aunque 15 segundos antes de refrescar, nos mostrará un botón para posponer el refrescado del listado por si estamos trabajando en el listado.

.. code-block:: yaml
   
   autoRefresh: true

También podemos optar por configurar dicho intervalo, siempre y cuando el valor numérico sea mayor que 30 (sino se usará la que está configurada por defecto). Por ejemplo:

.. code-block:: yaml
   
   autoRefresh: 90

preconfigured filters
---------------------

Podemos definir filtros pre-configurados, los cuales generan botones en la parte superior
de los listados. Se define en el primer nivel del **screen**.

En el ejemplo superior, se ha pre-configurado un filtro para aquellas personas que se llamen *Ana*.
Se pueden configurar tantos como se requieran.

.. code-block:: yaml

   preconfiguredFilters:
     ana: # Etiqueta para definir el filtro
       title: _("Ana")
       field: firstName
       value: "Ana"
       description: _("Utiliza este pre filtrado para buscar a Ana")

* **title**: título del botón del filtro.
* **field**: campo por el que se filtrará.
* **value**: valor que se usará para el filtrado. Si se quiere filtrar por un campo NULL, en *value* debe ir en comillas 'NULL'. Ejemplo: value: 'NULL'
* **description**: valor que se usará para mostrar en el tooltip.

presetted filters
-----------------

Básicamente tiene la misma funcionalidad que la de los filtros preconfigurados, con la diferencia de que estos
se aplican por defecto. Se define en el primer nivel del **screen**.

.. code-block:: yaml

   presettedFilters:
      Etiqueta: # Etiqueta para definir el filtro
        field: status 
        value: 
          - 'hidden'
          - 'waiting'
        op: 'eq' 

* **field**: campo sobre el que se aplica el presetted filter
* **value**: puede ser *array* o *string*, es el valor por el que se filtrará.
* **op**: por defecto será **eq**.
   * **lt** : menor que *(less than)*
   * **eq**: igual *(equal)*
   * **gt**: mayor que *(greater than)*

filterClass
-----------

Un ejemplo de como se estructura un **filterClass** personalizado:

.. code-block:: php

   <?php
      class Filter_Class_Name implements KlearMatrix_Model_Interfaces_FilterList
      {
          protected $_condition = array();
      
          public function setRouteDispatcher(KlearMatrix_Model_RouteDispatcher $routeDispatcher)
          {
              //La acción actual
              $currentAction = $routeDispatcher->getActionName();
              
              //El controller actual (edit, list...)
              $currentController = $routeDispatcher->getControllerName();
              
              //Para recoger el PK de la pantalla anterior
              $pk = $routeDispatcher->getParam('pk'); 
              
              //Añadir la condición
              $this->_condition[] = "Our Condition";
              
              return true;
          }
      
          //Ésta función no debe tocarse
          public function getCondition()
          {
              if (count($this->_condition) > 0) {
                  return '(' . implode(" AND ", $this->_condition) . ')';
              }
              return;
          } 
      }

parentOptionCustomizer
----------------------

Esta propiedad permite aplicar modificaciones sobre las opciones de los listados.
Se define en el primer nivel del **screen**.

.. code-block:: yaml

   tiendasList_screen:
     ...
     options:
       screens:
         tiendasHorariosList_screen: true

   tiendasHorariosList_screen:
     filterField: idTienda
     parentOptionCustomizer:
       - recordCount
       - App_Model_MyCustomizer 

En el ejemplo se muestra la opción de de **recordCount** sobre la opción que se ha añadido
a la pantalla *tiendasList_screen*. Esto lo que hará es mostrar el número de registros que hay
dentro de la opción filtrada.

La segunda opción dentro de **parentOptionCustomizer**, es el nombre de una clase propia.
Se puede definir una clase e implantar lo que se desee siempre y cuando extienda de
**KlearMatrix_Model_Interfaces_ParentOptionCustomizer**.

Mostrar/ocultar opciones
------------------------

Dentro del **parentOptionCustomizer**, podemos definir cualquier clase que queramos.
Dentro de esta clase podemos jugar a mostrar/ocultar los botones de opciones de
la vista de lista.

.. code-block:: php

   <?php
   class App_Model_MyCustomizer implements \KlearMatrix_Model_Interfaces_ParentOptionCustomizer
   {
       /**
        * @var KlearMatrix_Model_RouteDispatcher
        */
       protected $_mainRouter = null;
   
       /**
        * @var array
        */
       protected $_mainRouterOriginalParams = null;
   
       /**
        * @var KlearMatrix_Model_AbstractOption
        */
       protected $_option = null;
   
       protected $_resultWrapper = 'div';
       protected $_cssClass = 'hidden';
   
       public function __construct(\Zend_Config $configuration)
       {
           $front = \Zend_Controller_Front::getInstance();
           $this->_mainRouter = $front->getRequest()->getUserParam("mainRouter");
           $this->_mainRouterOriginalParams = $this->_mainRouter->getParams();
       }
   
       public function setOption (\KlearMatrix_Model_Option_Abstract $option)
       {
           $this->_option = $option;
       }
   
       /**
        * @return KlearMatrix_Model_ParentOptionCustomizer_Response
        */
       public function customize($parentModel)
       {
           // Si queremos recoger el Mapper/Model del screen a cargar
           $item = $this->_mainRouter->loadScreen($this->_option->getName());
           $model = $item->getObjectInstance();
           $mapper = $model->getMapper();

           // Al tratarse de un filtered screen necesita la pk del padre
           // se puede envíar cualquier parametro de ésta misma forma
           $this->_mainRouter->setParams(array("pk" => $parentModel->getPrimaryKey()));

           /* **NUESTRO CÓDIGO A IMPLEMENTAR** */

           // Hay que tener en cuenta que todas las options pasan por aquí
           // nos cercioramos de que es sobre la que queremos actuar, de lo 
           // contrario se devuelve la respuesta que espera el sistema
           if ($this->_option->getName() == "nuestro_screen"
               && $queremosMostrar) {
               /* Para mostrar el botón habría que devolver null */
               return null;
           } else {
               /* Para no mostrarlo iniciamos una respuesta vacía */
               $response = new \KlearMatrix_Model_ParentOptionCustomizer_Response();
               $response->setParentWrapper($this->_resultWrapper)
                   ->setParentCssClass($this->_cssClass);

               return $response;
           } 
       }
   }

Como se puede ver, a esta función nos llega el **$parentModel**, que sería el modelo de la línea de lista sobre la que se mostrará el botón de opción o no.

Para saber exactamente sobre que botón estamos trabajando debemos utilizar:

.. code-block:: guess

   $this->_option->getName()
