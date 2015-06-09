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