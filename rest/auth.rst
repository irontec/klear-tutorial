Plugin de Autenticación
-----------------------

Este es el plugin por defecto de autenticación, que tiene implementado los metodos "**Basic**" y "**Hmac**". Los cuales estan el "**library/Iron**".

.. code-block:: php

   <?php
   use Proyect\Model as Models;
   use Proyect\Mapper\Sql as Mappers;

   class Proyect_Controller_Plugin_Auth extends Zend_Controller_Plugin_Abstract
   {

      protected $_bootstrap;

      public function routeShutdown(Zend_Controller_Request_Abstract $request)
      {

         if ('rest' !== $request->getModuleName()) {
            return;
         }

         if ('OPTIONS' === $request->getMethod()) {
            return;
         }
         
         if (!$this->_checkConfigRequiredAuth($request)) {
            return;
         }
         
         ini_set('xdebug.overload_var_dump', 0);
         $this->_initAuth($request);

      }

      protected function _checkConfigRequiredAuth($request)
      {

         $methodsOptions = APPLICATION_PATH . '/configs/restApi.ini';

         if (!file_exists($methodsOptions)) {
            throw new Exception(
               'No existe el archivos de configuración restApi.ini'
            );
         }

         $methodsConfig = new \Zend_Config_Ini(
            $methodsOptions,
            APPLICATION_ENV
         );
         $controller = $request->getControllerName();
         $method = strtolower($request->getMethod());

         if (empty($methodsConfig->$controller)) {
            $check = $methodsConfig->defaultPolicy;
         } else {
            $check = $methodsConfig->$controller;
         }

         if (empty($check->$method)) {
            $auth = $methodsConfig->defaultPolicy->$method->authorization;
         } else {
            $auth = $check->$method->authorization;
         }

         return (boolval($auth));

      }

      protected function _initAuth(Zend_Controller_Request_Abstract $request)
      {

         $token = $this->getRequest()->getHeader('Authorization');

         $tokenParts = explode(' ', $token);

         if (sizeof($tokenParts) !== 2) {
            $this->_errorAuth();
         }

         $authType = $tokenParts[0];

         $mapper = new Mappers\KlearUsers();

         $getData = array(
            'user' => 'login',
            'pass' => 'pass'
         );

         if ($authType === 'Basic') {

            $auth = new \Iron_Auth_RestBasic();
            $auth->authenticate(
               $tokenParts[1],
               $mapper,
               $getData
            );

         } elseif ($authType = 'Hmac') {

            $reqDate = $this->getRequest()->getHeader('Request-Date', false);

            $auth = new \Iron_Auth_RestHmac();
            $auth->authenticate(
               $tokenParts[1],
               $reqDate,
               $mapper,
               $getData
            );

         } else {
            $this->_errorAuth();
         }

      }

      protected function _errorAuth()
      {

         $resutl = array(
            'success' => false,
            'message' => 'Authorization incorrecta'
         );

         $response = $this->getResponse();
         $response->setHttpResponseCode(401);
         $response->setBody(json_encode($resutl));
         $response->sendResponse();
         exit();

      }
   }