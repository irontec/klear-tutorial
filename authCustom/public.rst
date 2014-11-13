Zona Pública
============

.. contents::
   :local:
   :depth: 4

Podemos también incluir la autentificación de Klear para dar acceso a la zona pública. Se podría usar la tabla de
KlearUsers o crear una nueva pero con la ventaja de usar la encriptación de las contraseñas del Klear y hacer uso de los datos
del usuario durante una sessión activa.

.. attention::
   Tener en cuenta el uso de la opción **"disableChangeName"** para evitar conflictos de sesiones entre la zona Klear y la zona pública.
   Si queremos que Klear comparta su session, la opción debería ser **"disableChangeName = true"**.

.. note::
   El **PublicAdapter.php** es parecido al Adapter.php

.. code-block:: php
   :linenos:

   <?php

   use \MyApp\Mapper\Sql\Entity as UsersMapper;

   class MyApp_Auth_PublicAdapter implements Zend_Auth_Adapter_Interface
   {
       protected $_username;
       protected $_password;

       protected $_userMapper;
       protected $_user;


       public function __construct($user, $password)
       {
           $this->_usersMapper = new UsersMapper();
           $this->_username = $user;
           $this->_password = $password;
       }

       public function authenticate()
       {
           try {
               $user = $this->_usersMapper->findOneByField('user', $this->_username);

               if ($this->_userHasValidCredentials($user)) {

                   $this->_user = $user;
                   $authResult = Zend_Auth_Result::SUCCESS;

               } else {

                   $authResult = Zend_Auth_Result::FAILURE_CREDENTIAL_INVALID;

               }

               return new Zend_Auth_Result($authResult, $this->_username);
           } catch (Exception $e) {

               $authResult = Zend_Auth_Result::FAILURE_UNCATEGORIZED;
               $authMessage['message'] = $e->getMessage();
               return new Zend_Auth_Result($authResult, $this->_username, $authMessage);
           }


       }

       protected function _userHasValidCredentials($user = null)
       {
           if (!is_null($user)) {
               $hash = $user->getPassword();
               if ($user->getActive()=='1' && $this->_checkPassword($this->_password, $hash)) {
                   return true;
               }
           }
           return false;
       }

       protected function _checkPassword($clearPass, $hash)
       {
           $hashParts = explode('$', trim($hash, '$'), 2);

           switch ($hashParts[0]) {
            case '1': //md5
                list(,,$salt,) = explode("$", $hash);
                $salt = '$1$' . $salt . '$';
                break;

            case '5': //sha
                list(,,$rounds,$salt,) = explode("$", $hash);
                $salt = '$5$' . $rounds . '$' . $salt . '$';
                break;

            case '2a': //blowfish
                $salt = substr($hash, 0, 29);
                break;
           }

           $res = crypt($clearPass, $salt . '$');
           return $res == $hash;
       }

       public function saveStorage()
       {
           $auth = Zend_Auth::getInstance();
           $authStorage = $auth->getStorage();
           $this->_user->readWrite  = false;
           $this->_user->readOnly = false;
           $authStorage->write($this->_user);
       }
   }

Así que la manera recomendado de usarlo, sería con el siguiente código en nuestro Controller que nos ayuda a autenficarnos.

.. code-block:: php

   <?php
       $username = 'admin';
       $password = 'pass';

       $auth = Zend_Auth::getInstance();
       $authAdapter = new MyApp_Auth_PublicAdapter($username,$password);

       if ($authAdapter->authenticate() && $authAdapter->authenticate()->getCode()==1) {
           $authAdapter->saveStorage();
       }

       $result = $auth->authenticate($authAdapter);

       if($result->isValid()) {
           echo 'Usted se identifico correctamente.';
           exit;
       }