Dialogs
=======

Los dialogs son las ventanas emergentes que aparece en nuestra interfaz del Klear. Estas ventanas las puede originar los iconos
de la columna de opciones o botones en la misma pantalla. Klear traer trae tres tipos de dialogs que pueden ser configuradas
desde los yaml para distintas funciones: **Delete** y **Clone**.

.. contents::
   :local:
   :depth: 2
   
Delete
------
Este dialog con el **controller DELETE** [#controller]_ sirve para borrar un dato desde la interfaz del Klear. Esta opción 
viene por defecto en los yaml **LIST** cuando son creadas por el :ref:`generatorYaml`. Por defecto, se presenta de esta manera:

.. code-block:: yaml
   :emphasize-lines: 19
   
   production:
     main:
       module: klearMatrix
       defaultScreen: sectionsList_screen
     screens:
       sectionsList_screen:
         controller: list
         pagination:
           items: 25
         <<: *Sections
         title: _("List of %s", ngettext('Section', 'Sections', 0))
         fields:
           options:
             title: ngettext("Option", "Options", 0)
             screens:
               sectionsEdit_screen: true
               subSectionsList_screen: true
             dialogs:
               sectionsDel_dialog: true
             default: sectionsEdit_screen
             

.. code-block:: yaml
   :emphasize-lines: 2-8
   
     dialogs:
       sectionsDel_dialog:
         <<: *Sections
         controller: delete
         class: ui-silk-bin
         label: false
         title: _("Delete %s", ngettext('Section', 'Sections', 1))
         description: _("You want to delete this %s?", ngettext('Section', 'Sections', 1))
         labelOnList: true // Indicará que si se debe mostrar un label, cuando se comporta como una opción de pantalla global List
         multiItem: true // Hace que el dialogo sea compatible con selección múltiple de filas (cuando delete es configurada como opcion global de listado)


Clone
-----

Este dialog con el **controller CLONE** [#controller]_ sirve para clonar o copiar un dato desde la interfaz del Klear.
Esta opción no viene por defecto en los yaml **LIST** por lo que se tendrá que incorporarse manualmente con el siguiente
sencillo procedimiento:

.. attention::
   Los siguientes códigos no hacen falta añadir todo el código en nuestro **YAML LIST**, sino es una forma de ubicar el
   siguiente código dentro de nuestro campo **dialogs**. El procedimiento es parecido como el **dialog DELETE**.

.. code-block:: yaml
   :emphasize-lines: 6
   
   nuestraPantalla_screen:
     fields: 
       options: 
         title: _("Options")
           dialogs: 
             itemClone_dialog: true
             
Parámetros de configuración
###########################

* **controller**: Hay que definir el controller CLONE

* **class**: Para definir el icono que queremos que aparezca

* **title**: Se mostrará como encabezado del dialog

* **description**: Se muestra dentro del dialog en la primera pantalla

* **message**: Se muestra cuando todo ha salido bien

*  **cloneDependents**: Se usa para indicar que se quiere clonar las relaciones.

*  **postCloneMethods**: Se usa para indicar un método del modelo a ejecutar.
 
   * **clonned**: Se ejecuta el método del modelo clonado (éste recibe el modelo original como parámetro).
   * **original**: Se ejecuta el método del modelo original (éste recibe el modelo clonado como parámetro).
   
.. code-block:: yaml
   :emphasize-lines: 2-
   
   dialogs:
      itemClone_dialog: 
        <<: *Item
        controller: clone
        class: ui-silk-clone
        title: _("Clone %s", ngettext('Item', 'Items', 1))
        description: _("You want to clone this %s?", ngettext('Item', 'Item', 1))
        message: _("%s successfully cloned", ngettext('Item', 'Items', 1))
        cloneDependents: true
        postCloneMethods:
          clonned: isAClone
          original: setAsClonned


MassUpdate
----------

Este dialog con el **controller MassUpdate** [#controller]_ sirve para actualizar un campo de tipo select, multiselect o checkbox desde la interfaz de listado de Klear.
Esta opción no viene por defecto en los yaml **LIST** por lo que se tendrá que incorporarse manualmente con el siguiente
sencillo (a la par que entretenido) procedimiento:

.. attention::
   Los siguientes códigos no hacen falta añadir todo el código en nuestro **YAML LIST**, sino es una forma de ubicar el
   siguiente código dentro de nuestro campo **dialogs**. El procedimiento es parecido como el **dialog DELETE**.

.. code-block:: yaml
   :emphasize-lines: 6
   
   nuestraPantalla_screen:
     fields: 
       options: 
         title: _("Options")
           dialogs: 
             itemMassUpdate_dialog: true
             
Parámetros de configuración
###########################

* **controller**: Hay que definir el controller MassUpdate

* **class**: Para definir el icono que queremos que aparezca

* **title**: Se mostrará como encabezado del dialog

* **description**: Se muestra dentro del dialog en la primera pantalla

* **message**: Se muestra cuando todo ha salido bien
         
.. code-block:: yaml
   :emphasize-lines: 2-8
   
   dialogs:
      itemMassUpdate_dialog: 
        <<: *Item
        controller: mass-update
        class: ui-silk-pencil-go
        title: _("Actualizar campo en %s", ngettext('Item', 'Items', 1))
        description: _("Do you want to update "X" on this %s?", ngettext('Item', 'Item', 1))
        message: _("(%total%) %s successfully updated", ngettext('Item', 'Items', 1))
        labelOnList: true # Opcional, si se quiere que salga el label, cuando es una opción general de pantalla(con multiItem)
        multiItem: true # Opcional, si se quiere que sea una opción multi-campo

        
Custom dialog
-------------

Por el momento, existe un determinado de dialogs configurados en nuestro Klear para uso estándar para los proyectos, pero también podemos
crear nuestros propios dialogs para que hagan funciones distintas a lo que está programado con solo ubicar correctamente los siguientes códigos 
en estas rutas.

.. attention:: 
   Tiene que incorporarse todos estos códigos, empezar y quedarse a medio camino, solo provocará un error en el Listado donde se incorporó el código.

En cualquiera de nuestros **YAML LIST**. Debemos incorporar los siguientes códigos, siendo lo marcado lo que se tiene que incorporar:

.. code-block:: yaml
   :emphasize-lines: 6
   
   nuestraPantalla_screen:
     fields: 
       options: 
         title: _("Options")
           dialogs: 
             nameaction_dialog: true

.. code-block:: yaml
   :emphasize-lines: 2-8
   
   dialogs:
       nameaction_dialog:
         title: _("My Dialog")
         module: default
         label: true
         controller: klear-custom-nameaction
         action: index
         class: ui-silk-page-white-width
         labelOnEdit: true|false|string // La opción se dibujará con "label" cuando es una opción general de Edit
         labelOnList: true|false|string // La opción se dibujará con "label" cuando es una opción general de List
         labelOnEntityPostSave: true|false|string // La opción se dibujará con "label" cuando es una opción general de EntityPostSave
         multiItem: true // Al invocarase la opción como general option desde List, el controlador será invocado con pk como un array de Ids.
         alwaysEnabled: true //Sólo se usa cuando multiItem = true. En este caso se usa para permitir que el botón del diálogo esté habilitado aunque no se seleccione ningún registro.
         
Después de indicar el **controller** [#zendFramework]_ hay que crear dicho controlador en nuestra carpeta **controllers** *(application/controllers)*, en
este caso sería el archivo **KlearCustomNameActionController.php** con el siguiente contenido, teniendo en cuenta que lo marcado es obligatorio:
         
.. code-block:: php
   :linenos:
   :emphasize-lines: 9-20,38-53,56-61

   <?php
    
   class KlearCustomNameActionController extends Zend_Controller_Action
   {
       protected $_mainRouter;
    
       public function init()
       {
           // Nos aseguramos que este controlador se ejecuta sólamente desde klear!
           if ((!$this->_mainRouter = $this->getRequest()->getUserParam("mainRouter")) ||
               (!is_object($this->_mainRouter)) ) {
                   throw New Zend_Exception('',Zend_Controller_Plugin_ErrorHandler::EXCEPTION_NO_ACTION);
           }
    
           //Inicia el contenido en Json
           $this->_helper->ContextSwitch()
                ->addActionContext('NameAction', 'json')
                ->initContext('json');
    
           $this->_helper->layout->disableLayout();
       }
    
       public function indexAction()
       {
    
           if ($this->getRequest()->getParam("Parametro de verificación")) {
              $this->_helper->viewRenderer->setNoRender(true);
              //Las acciones que generara este Parametro
           }
           

           // Si queremos coger la Id del dato donde es llamado nuestro dialog.
           // Detectamos si hemos sido invocados desde un listado
           if (is_array($this->_mainRouter->getParam('pk')) {

               $aIds = $this->_mainRouter->getParam('pk');

           } else {

               $id = $this->_mainRouter->getParam('pk');
               // sería buena idea (compatible con multiItems: true)
               // $aIds = array($id);

           }
           
    
           //Por ultimo generamos las opciones del Custom Dialog
           //y las acciones de cada opción
    
           $data = array(
             'title' => _("Mi Accion"),
                       'message'=>_("Estas seguro de ejecutar esta ación"),
                       'buttons'=>array(
                                  // Este botón volverá a llamar el dialog enviando un parámetro y será reconocido por la línea 26
                                  _('Aceptar') => array(
                                          'recall'=>true,
                                          'params'=>array(
                                                  "Parametro de verificación" => true 
                                      ) 
                                    ),
                                  _('Cancelar') => array(
                                                'recall' => false,
                                                )
                                         )
                );
    
           //Inicia los plugins de KlearMatrix
           $jsonResponse = new Klear_Model_DispatchResponse();
           $jsonResponse->setModule('klearMatrix');
           $jsonResponse->setPlugin('klearMatrixGenericDialog');
           $jsonResponse->addJsFile("/js/plugins/jquery.klearmatrix.genericdialog.js");
           $jsonResponse->setData($data);
           $jsonResponse->attachView($this->view);
       }
   }
   
El resto ya está a la imaginación del programador de lo que quiere crear o lograr con este sistema.

.. [#controller] Clases ya definidas en el módulo del Klear para que cumplan una función determinada.
.. [#zendFramework] En Zend Framework, el controller es una clase que tiene que ser llamada {Controller name}Controller. 
   Tengan en cuenta que {Controller name} debe comenzar con la primer letra en mayúscula. 
   Esta clase debe estar dentro de un archvo llamado {Controller name}Controller.php dentro de la carpeta application/controllers. 
   Cada acción es una función pública dentro de la clase del controlador que debe llamarse {action name}Action. 
   En este caso {action name} tiene que ser escrito en letra en minúscula. 
   Los nombres con mayúsculas y minúsculas son permitidas en los controladores y las acciones, pero tiene algunas reglas especiales que tienes que comprender antes de que las utilices.
