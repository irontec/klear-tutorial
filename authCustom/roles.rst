Roles
=====

.. contents::
   :local:
   :depth: 4

Crear Adaptador
###############

Para crear el adaptador para los roles, usaremos como ejemplo la carpeta Auth que se encuentra en el proyecto Klear Interval (web/library/KlearIntervallib/Auth).
En la carpeta **Auth** intentaremos tener la siguiente estructura:

.. code-block:: console

   KlearIntervallib/
   └── Auth/
       ├── Adapter.php
       ├── Model/
       │   └── User.php
       └── Users.php
       
.. attention:: 
   Lo siguiente que viene son los contenidos de cada archivo: Adapter.php, Users.php y User.php. Después de copiarlo, asegurarse que los nombres KlearIntervarlib o KlearInterval se
   adapte a su proyecto.

Adapter.php
***********

.. code-block:: php
   :linenos:

   <?php
   
   class KlearIntervallib_Auth_Adapter implements \Klear_Auth_Adapter_KlearAuthInterface
   {
       protected $_username;
       protected $_password;
       protected $_userId;
   
       protected $_userMapper;
   
       /**
        *
        * @var Klear_Auth_Adapter_Interfaces_BasicUserModel
        */
       protected $_user;
   
       public function __construct(Zend_Controller_Request_Abstract $request, Klear_Model_ConfigParser $authConfig = null)
       {
           $this->_username = $request->getPost('username', '');
           $this->_password = $request->getPost('password', '');
           $this->_initUserMapper($authConfig);
       }
   
       protected function _initUserMapper(Klear_Model_ConfigParser $authConfig = null)
       {
           if ($authConfig->exists('userMapper')) {
   
               $userMapperName = $authConfig->getProperty('userMapper');
           } else {
   
               // TODO: Log auth fallback info message;
               $userMapperName = '\Klear_Model_Mapper_Users';
           }
   
           $this->_userMapper = new $userMapperName;
   
           if (!$this->_userMapper instanceof Klear_Auth_Adapter_Interfaces_BasicUserMapper) {
   
               throw new \Exception('Auth userMapper must implement Klear_Auth_Adapter_BasicUserInterface');
           }
       }
   
       public function authenticate()
       {
           try {
   
               $user = $this->_userMapper->findByLogin($this->_username);
   
               if ($this->_userHasValidCredentials($user) ) {
   
                   $this->_user = $user;
                   $authResult = Zend_Auth_Result::SUCCESS;
                   $authMessage = array("message"=>"Welcome!");
   
               } else {
   
                   $authResult = Zend_Auth_Result::FAILURE_CREDENTIAL_INVALID;
                   $authMessage = array("message"=>"Usuario o contraseña incorrectos.");
               }
               
               return new Zend_Auth_Result($authResult, $this->_username, $authMessage);
   
           } catch (Exception $e) {
   
               $authResult = Zend_Auth_Result::FAILURE_UNCATEGORIZED;
               $authMessage['message'] = $e->getMessage();
               return new Zend_Auth_Result($authResult, $this->_username, $authMessage);
           }
       }
   
       protected function _userHasValidCredentials(Klear_Auth_Adapter_Interfaces_BasicUserModel $user = null)
       {
           if (!is_null($user)) {
   
               $hash = $user->getPassword();
   
               if ($user->isActive() && $this->_checkPassword($this->_password, $hash)) {
   
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

Users.php
*********

.. code-block:: php
   :linenos:

   <?php
   use \KlearInterval\Mapper\Sql\KlearUsers as KlearUsersMapper;
   
   class KlearIntervallib_Auth_Users implements Klear_Auth_Adapter_Interfaces_BasicUserMapper
   {
       protected $_dbTable;
   
       /**
        *
        * @var \Domoalert\Model\KlearUsers
        */
       protected $_atezateUser;
   
       public function __construct()
       {
           $this->_dbTable = new Klear_Model_DbTable_Users();
       }
   
       public function findByLogin($login)
       {
           $select = $this->_dbTable->select()->where('login = ?', $login);
           $row = $this->_dbTable->fetchRow($select);
   
           if ($row) {
               $user = new KlearIntervallib_Auth_Model_User();
               return $this->_poblateUser($user, $row);
           }
           
           return null;
       }
   
       protected function _poblateUser(Klear_Model_User $user, Zend_Db_Table_Row $row)
       {
           $user->setId($row->userId);
           $user->setLogin($row->login);
           $user->setEmail($row->email);
           $user->setPassword($row->pass);
           $user->setActive($row->active);
           
           $userMapper = new KlearUsersMapper();
           $this->_atezateUser = $userMapper->findOneByField('userId', $row->userId);
           $roles = $this->_atezateUser->getKlearUsersRoles();
           $aSections = array();
           
   
           foreach ($roles as $rol) {
   
               $secs = $rol->getKlearRole()->getKlearRolesSections();
   
               foreach ($secs as $sec) {
   
                   $aSections[] = $sec->getKlearSection()->getIdentifier();
               }
           }
           array_unique($aSections);
   
           $user->setUserAccessSections($aSections);
   
           if ($user->getId() == '1') {
   
               $user->setAdministrator(true);
           }
   
           return $user;
       }
   }
   
User.php (De la carpeta Model)
******************************

.. code-block:: php
   :linenos:
   
   <?php
   class KlearIntervallib_Auth_Model_User extends Klear_Model_User
   {
       protected $_administrator = false;
       protected $_accessSections;
   
       public function setUserAccessSections($data)
       {
           $this->access = $data;
           $this->_accessSections = $data;
           return $this;
       }
   
       public function getUserAccessSections()
       {
           return $this->_accessSections;
       }
   
       public function setAdministrator($data)
       {
           $this->_administrator = $data;
           return $this;
       }
   
       public function getAdministrator()
       {
           return $this->_administrator;
       }
   }
   
klear.yaml
**********

Añadir en el klear.yaml en la sección **Auth**. El siguiente código:

.. code-block:: yaml 
   :emphasize-lines: 2-3
   
    auth:
      userMapper: KlearIntervallib_Auth_Users
      adapter: KlearIntervallib_Auth_Adapter
      title: _("Access denied")
      description: _("Insert your username")
      session:
        name: klear_interval
        disableChangeName: true
