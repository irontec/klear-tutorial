Documentación
=============

Los controllers creados por el generador, traen los comentarios necesario para hacer una documentación con la libreria "**crada/php-apidoc**" (https://github.com/calinrada/php-apidoc).

Los generadores tambien crean una serie de archivos y carpetas para automatizar la generación de la documentación.

En la raiz del proyecto, a la altura de "**application/**" y "**public/**" el generador crea el directorio "**cli/**" con los archivos **cliDocs.php** y **runCliDocs.sh**. aparte en **application/controllers/** tambien crea el archivo **ApidocController.php** y en el directio **library/** crea el directorio **Composer** con un **composer.json** preparado para instalar la libreria "**crada/php-apidoc**".

.. contents::
   :local:
   :depth: 3


ApidocController.php
--------------------

.. code-block:: php

   <?php
   
   use Crada\Apidoc\Builder;
   use Crada\Apidoc\Exception;
   
   class ApidocController extends Zend_Controller_Action
   {
   
       public function init()
       {
   
           /**
            * Initialize action controller here
            */
           if (isset($_SERVER['REMOTE_ADDR'])) {
               print('Command Line Only!');
               exit(1);
           }
   
           $this->_helper->viewRenderer->setNoRender();
   
       }
   
       public function indexAction()
       {
   
           $moduleControllerPath = APPLICATION_PATH . '/modules/rest/controllers/';
   
           $classes = array();
   
           foreach (glob($moduleControllerPath . "*Controller.php") as $file) {
               include_once  $file;
               $classes[] = "Rest_" . basename($file, ".php");
           }
   
           $outputDir  = APPLICATION_PATH . '/../public/apidocs';
           $outputFile = 'index.html';
   
           try {
   
               $builder = new Builder($classes, $outputDir, $outputFile);
               $builder->generate();
   
           } catch (Exception $e) {
   
               echo 'There was an error generating the documentation: ', $e->getMessage();
   
           }
   
           echo "Done\n";
           exit(0);
   
       }
   
   }

composer.json
-------------

.. code-block:: json

   {
       "name":"Testing",
       "description":"",
       "require":{
           "crada/php-apidoc":"@dev"
       }
   }

cliDocs.php
-----------

.. code-block:: php

   #!/usr/bin/php
   <?php

    defined('__DIR__') || define('__DIR__', dirname(__FILE__));

    defined('APPLICATION_PATH')
    || define('APPLICATION_PATH', realpath(dirname(__FILE__) . '/../application'));
    define('APPLICATION_CLI', true);

    set_include_path(
        implode(
            PATH_SEPARATOR,
            array(
                realpath(APPLICATION_PATH . '/../library'),
                get_include_path(),
            )
        )
    );

    /** Composer **/
    require_once __DIR__ . '/../library/Composer/vendor/autoload.php';

    require_once 'Zend/Loader/Autoloader.php';
    $loader = Zend_Loader_Autoloader::getInstance();

    // we need this custom namespace to load our custom class
    $loader->registerNamespace('Testing_');

    $getopt = new Zend_Console_Getopt(
        array(
        'action|a=s' => 'action to perform in format of "module/controller/action/param1/param2/param3/.."',
        'env|e-s'    => 'defines application environment (defaults to "production")',
        'help|h'     => 'displays usage information',
        )
    );

    try {
        $getopt->parse();
    } catch (Zend_Console_Getopt_Exception $e) {
        // Bad options passed: report usage
        echo $e->getUsageMessage();
        return false;
    }

    /**
     * show help message in case it was requested or params were incorrect (module, controller and action)
     */
    if ($getopt->getOption('h') || !$getopt->getOption('a')) {
        echo $getopt->getUsageMessage();
        return true;
    }

    /**
     * initialize values based on presence or absence of CLI options
     */
    $env      = $getopt->getOption('e');
    defined('APPLICATION_ENV')
     || define('APPLICATION_ENV', (null === $env) ? 'cli' : $env);


    /**
     * initialize Zend_Application
     */
    $application = new Zend_Application(
        APPLICATION_ENV,
        APPLICATION_PATH . '/configs/application.ini'
    );

    /**
     * bootstrap and retrive the frontController resource
     */
    $front = $application->getBootstrap()
          ->bootstrap('frontController')
          ->getResource('frontController');

    $params = array_reverse(explode('/', $getopt->getOption('a')));
    $module = array_pop($params);
    $controller = array_pop($params);
    $action = array_pop($params);
    $request = new Zend_Controller_Request_Simple($action, $controller, $module);

    /**
     * set front controller options to make everything operational from CLI
     */
    $front->setRequest($request)
       ->setResponse(new Zend_Controller_Response_Cli())
       ->setRouter(new Iron_Controller_Router_Cli())
       ->throwExceptions(true);

    /**
     * lets bootstrap our application and enjoy!
     */
    $application->bootstrap()->run();


runCliDocs.sh
-------------

.. code-block:: bash

   #!/bin/bash
   BASEDIR=$(dirname $0)
   /usr/bin/php ${BASEDIR}/cliDocs.php -a default/apidoc/index -e development


Crear documentación
-------------------

Con todo esto es su sitio y correctamente configurado, se ejecuta el **runCliDocs.sh** que creara en **public/** el directorio **apidocs** donde esta la documentación en html, lista para ver desde un navegador.