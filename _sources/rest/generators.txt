.. _rest_generator:

Generadores
-----------

Si se quiere hacer de forma automatica la base de una **Api REST**, phing cuenta con una tarea llamada "**generate-rest-controllers**"
la cual necesita de un par de parametros en el **application.ini** y que las tablas que tendran controladores **REST** tengan el comentario "**[rest]**".


.. code-block:: ini

   restConfig.path = APPLICATION_PATH "/modules/rest/"
   restConfig.usersAuthTable = "KlearUsers"
   restConfig.fieldUsername = "login"
   restConfig.fieldPassword = "pass"
   
* **path**: la ruta del modulo **REST**. **(Aunque no se use el generador este parametro es obligatorio)**.
* **usersAuthTable**: Nombre de la table de autenticación
* **fieldUsername**: Campo de identificacón del usuario
* **fieldPassword**: Campo de la contraseña del usuario
   
Por otro lado, en la tabla que se usa para la autenticación, hay que añadir una columna tokenKey. Esta se utilizará para autenticar las peticiones de la API. Para generar el token a partir de la password, hay que declarar la password como PlainText en el yaml.

.. code-block:: yaml

   password: 
      title: ngettext('Password', 'Passwords', 1)
      type: password
      adapter: PlainText

Y en el mapper hay que calcular el token cada vez que se modifica la password, y guardar esta cifrada para que no se almacene en texto plano.

.. code-block:: php

   <?php	
   protected function _save(\Model\Raw\User $model,
            $recursive = false, $useTransaction = true, $transactionTag = null
    )
    {
        $pass = $model->hasChange('password');
        if ($pass) {
            $passPlain = $model->getPassword();
            if (!empty($passPlain)) {
                $newToken = md5(md5($passPlain));
                $model->setTokenKey($newToken);
                
                $salt = $this->_salt();
                $ret = crypt(
                        $model->getPassword(),
                        '$5$rounds=5000$' . $salt . '$'
                );
                $model->setPassword($ret);
                $this->_logger->log("Password Setted", \Zend_Log::INFO);
            }
        }
        return parent::_save($model, $recursive, $useTransaction, $transactionTag);
    
    }
    
    protected function _salt()
    {
        $ret = substr(md5(mt_rand(), false), 0, 8);
    
        return $ret;
    }

Entre las cosas que automatisa este generador, están: 
 * La generación de controladores con basicos con metodos funcionales
 * En cada metodo una serie de comentario, para una documentación con la librería "**crada/php-apidoc**"
 * Un plugin de autenticación por **HEADER**, que actualmente da soporte para **Basic** y **HMAC**
 * El archivo "**restApi.ini**" que define que metodos necesitan autenticación
