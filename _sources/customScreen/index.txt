=============
Custom screen
=============

.. contents::
   :local:
   :depth: 3

Si tenemos la necesidad de implementar algo que los tipos de pantalla de
**klear** no nos permiten, siempre podemos crear un **screen** personalizado,
el cual puede hacer lo que deseemos.

Configuración
=============

List yaml
---------

Lo primero será definir la configuración de la nueva **screen**, para ello hay que editar el
archivo **yaml** del **list** en el cual queremos que aparezca.

.. code-block:: yaml

   personalized_screen:
      <<: *Modelo
      controller: klear-custom-screen
      action: my-screen
      class: ui-silk-pencil
      title: _("Mi pantalla personalizada")

* **controller**: será el campo que haga referencia al nuevo screen creado.

  El controller definido como **klear-custom-screen**, hace referencia
  a **KlearCustomScreenController**.

* **action**: la acción a la que llama el **screen**, en este caso sería
  **MyScreenAction**.

Controller
----------

El archivo del **controller** mas simple posible quedaría así:

.. code-block:: php

   <?php

   class KlearCustomScreenController extends Zend_Controller_Action
   {
       public function init()
       {
           /* Initialize action controller here */
           $this->_helper->layout->disableLayout();
   
           $this->_helper->ContextSwitch()
               ->addActionContext('my-screen', 'json')
               ->initContext('json');
   
           if ((!$this->_mainRouter = $this->getRequest()->getParam("mainRouter")) || (!is_object($this->_mainRouter)) ) {
               throw New Zend_Exception('',Zend_Controller_Plugin_ErrorHandler::EXCEPTION_NO_ACTION);
           }
       }
   
       public function myScreenAction()
       {
           // Tenemos el $this->_mainRouter para explotar
           // los datos necesarios
           
           // Do stuff

           // Creamos un array con los datos que queremos devolver
           $data = array();
           
           // Hay que devolver un objeto Klear_Model_DispatchResponse()
           $jsonResponse = new Klear_Model_DispatchResponse();
           $jsonResponse->setModule('default');
           $jsonResponse->addJsFile("../js/jquery.myScreen.js");
           $jsonResponse->setData($data);
           $jsonResponse->attachView($this->view);
       }
   }

Este controlador estará en el mismo directorio que están los demás que componen la aplicación.

.. attention:: 

   Un error muy común es denominar mal los archivos, ya que al final hay que añadir el
   **Controller**, quedando este fichero así: **klearCustomScreenController.php**.

Hay que tener en cuenta que en la acción **init** debemos añadir nuestra acción personalizada
al **contentSwitch** para que pueda funcionar como **Json**.

En ésta misma acción, recogemos el **_mainRouter** para poder explotar los datos de la aplicación
( de que pantalla venimos, la configuración del list, etc...).

Por último, en nuestra acción, depués de hacer lo que haga falta siempre debemos devolver un
objeto **Klear_Model_DispatchResponse()**. A este objeto se le pueden añadir muchas cosas,
lo mejor es entrar dentro de la
`clase <http://dev2.irontec.com/websvn/filedetails.php?repname=klear&path=%2Ftrunk%2Fmodules%2Fklear%2Fmodels%2FDispatchResponse.php>`_
y ver que posibilidades hay.

Fichero JS
----------

En nuestro caso, hemos añadido los datos generados en la acción (que se recibirán en la parte
del **klear**) y también el archivo **jquery.myScreen.js** para controlar el comportamiento
personalizado de nuestro **screen**.

Éste archivo hay que guardarlo en la ruta:

.. code-block:: console

   APPLICATION_PATH/assets/js/custom/jquery.myScreen.js

Y el archivo con lo básico quedaría así:

.. code-block:: js

   ;(function load($) {

       if (!$.klear.checkDeps(['$.klearmatrix.module','$.ui.form'],load)) {
           return;
       }
   
       var __namespace__ = "custom.myScreen";
   
       $.widget("custom.myScreen", $.klearmatrix.module,  {
           options: {
               data : null,
               moduleName: 'myScreen'
           },
           _super: $.klearmatrix.module.prototype,
           _create : function() {
               this._super._create.apply(this);
           },
           _init: function() {
   
               if (this.options.data.templateName) {
                   var $appliedTemplate = this._loadTemplate(this.options.data.templateName);
                   $(this.element.klearModule("getPanel")).append($appliedTemplate);
   
                   this
                      ._applyDecorators()
                      ._registerBaseEvents()
                      ._registerFieldsEvents()
                      ._registerEvents()
                      ._doAction();
               }
           },
           _doAction: function() {
               //Do stuff
           }
       });
   
       $.widget.bridge("myScreen", $.custom.myScreen);
   
   })(jQuery);

Como se vé, este **Script** extiende de **$.klearmatrix.module** y **$.ui.form**.
En la función **_init** se lanzan las funciones básicas y la última sería nuestra función
personalizada, en la cual podremos meter el kung-fu necesario.

.. attention:: 

   Revisar bien el script internamente, los nombres de ficheros y demás. Deberá estar todo bien
   denominado para poder funcionar correctamente.
