Bootstrap
---------

En el **Bootstrap** hay que incluir este **_initRouter** que instancia la ruta del modulo.

.. code-block:: php

   <?php
   class Bootstrap extends Zend_Application_Bootstrap_Bootstrap
   {

      public function _initRouter()
      {
         $front = Zend_Controller_Front::getInstance();
         $restRoute = new Zend_Rest_Route($front, array(), array('rest'));
         $front->getRouter()->addRoute('rest', $restRoute);
      }

   }

Para hacer nuevas rutas personalizadas, hay que crear un Bootstrap nuevo en APPLICATION_PATH/modules/rest. Este Bootstrap debe gestionar que, si se instancia desde consola, no debe generar las rutas personalizadas. En caso de no realizar correctamente esta gestión, la generación de la documentación dará un error.

.. code-block:: php

   <?php
    protected function _initRouting()
    {
        $frontController = Zend_Controller_Front::getInstance();
        $router = $frontController->getRouter();

        if($router instanceof \Iron_Controller_Router_Cli) {
            return;
        }

        $router->addRoutes(
                array(
                    'poi' => new Zend_Controller_Router_Route(
                                    'rest/user/:username',
                                    array(
                                        'controller' => 'user',
                                        'action' => 'get',
                                        'module' => 'rest'
                                    )
                             ),
                    ),
        );
    
    }
