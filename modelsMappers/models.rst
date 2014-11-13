Models
======

Vamos a usar las funciones ya definidas por defecto por el Generador, aunque se pueden crear aún más según el objetivo deseado.

Guardar datos (set, save, get)
------------------------------

Para guardar datos usaremos las funciones **set** y **save**. Ejemplo usando un models de KlearIntervals:

.. code-block:: php
   :linenos:
   :emphasize-lines: 11-25

   <?php

   class IndexController extends Zend_Controller_Action
   {

       public function init()
       {
           /* Initialize action controller here */
       }

       public function indexAction()
       {
           $modelCategory = new KlearInterval\Model\Categories(); // Usamos el model de la tabla que necesitamos, en este caso la table es Categories

           $modelCategory->setName('test6'); // Llamos la función set + 'campo' y ponemos el valor que se guardará en dicho campo.
           // .
           // .
           // .
           // Repetir tantas veces, según la cantidad de campos de la tabla.

           $modelCategory->save(); // Esta función es la que hacer el guardado en sus respectivos campos


           $modelCategory->getId(); // OPCIONAL, se puede obtener la ID del dato u otros campos con la función 'get' + Campo
       }
   }

.. _modelsCreate:

Crear nuestros propios models
-----------------------------

Klear tiene un límite de funciones que puede usar, practicamente las más básicas o útiles que se pueden usar en los proyectos
en general. Aunque si quisieramos crear nuestros propios modelos o agregar nuestros propios parámetros para que haga funciones
específivas o repetitivas para el proyecto, tendremos que saber donde incorporarlas.

Como bien saben, existe el :ref:`generatorModelsMappers` por lo que sabemos, las genera o crea dichos ficheros en nuestra carpeta **library**,
pero dicho generador se puede usar tantas veces como haga falta cada vez que modifiquemos nuestra base de datos, en otras palabras,
sobreescribe los ficheros tantas veces como es solicitado. Así que, debido a ello, debemos saber donde guardar a salvo nuestros
promios models para que no se sobreescriban y se mantengan siempre a disposición de la aplicación.

La carpeta Models se encuentra en la carpeta **library**: library/Application/Model. Si se llegara volver a usar el :ref:`generatorModelsMappers`
se volvería sobreescribir **TODO** el contenido **SOLO** de la carpeta **RAW** y dejándonos los demás archivos a nuestra imaginación
para aplicarle todo tipo de programación. Por ejemplo, el Model de Klear Intervals, se encuentra así definida:

.. attention:: Solo podemos editar las que tenemos resaltadas en el siguiente código, el resto se podrá sobreescribir.


.. code-block:: console
   :emphasize-lines: 3-17,39-42

   $ tree library/KlearInterval/Model/
   library/KlearInterval/Model/
   ├── Categories.php
   ├── Developers.php
   ├── KlearImageGalleries.php
   ├── KlearImageGalleriesPictures.php
   ├── KlearImageGalleriesSizes.php
   ├── KlearRoles.php
   ├── KlearRolesSections.php
   ├── KlearSections.php
   ├── KlearUsers.php
   ├── KlearUsersRoles.php
   ├── Marcas.php
   ├── Multifields.php
   ├── News.php
   ├── Paginator.php
   ├── Projects.php
   ├── Raw
   │   ├── Categories.php
   │   ├── Developers.php
   │   ├── generator.log
   │   ├── KlearImageGalleries.php
   │   ├── KlearImageGalleriesPictures.php
   │   ├── KlearImageGalleriesSizes.php
   │   ├── KlearRoles.php
   │   ├── KlearRolesSections.php
   │   ├── KlearSections.php
   │   ├── KlearUsers.php
   │   ├── KlearUsersRoles.php
   │   ├── Marcas.php
   │   ├── ModelAbstract.php
   │   ├── Multifields.php
   │   ├── News.php
   │   ├── Projects.php
   │   ├── RelNewsCategories.php
   │   ├── Sections.php
   │   ├── SubSections.php
   │   └── Tablacosas.php
   ├── RelNewsCategories.php
   ├── Sections.php
   ├── SubSections.php
   └── Tablacosas.php

Ejemplo simple
##############

Agregaremos una nueva función llamado **countLength** a nuestro modelo de Categorías *(Categories.php)* ya que aparte
de nos devuelva la Id, necesitamos que nos devuelva la cantidad de caracteres del nombre que se guardó.

.. code-block:: php
   :linenos:
   :emphasize-lines: 31-42

   <?php

   /**
    * Application Model
    *
    * @package KlearInterval\Model
    * @subpackage Model
    * @author User name
    * @copyright ZF model generator
    * @license http://framework.zend.com/license/new-bsd     New BSD License
    */

   /**
    *
    *
    * @package KlearInterval\Model
    * @subpackage Model
    * @author Lander Ontoria Gardeazabal
    */

   namespace KlearInterval\Model;
   class Categories extends Raw\Categories
   {
       /**
        * This method is called just after parent's constructor
        */
       public function init()
       {
       }

       public function countLength($value)
       {
           $model = new \KlearInterval\Model\Categories();

           $model->setName($value);

           $model->save();

           $dato = array('id' => $model->getId(), 'length' => strlen($cadena));

           return $dato;
       }
   }

Así que ahora para hacer uso de esa nueva función de nuestro model, lo podemos llamar de esta manera.

.. code-block:: php
   :linenos:
   :emphasize-lines: 11-22

   <?php

   class IndexController extends Zend_Controller_Action
   {

       public function init()
       {
           /* Initialize action controller here */
       }

       public function indexAction()
       {
           $model = new \KlearInterval\Model\Categories();

           $value = 'Texto de prueba';

           $data = $model->countLength($value);

           var_dump($value); //Devolvería array('id' => 8, 'length' => 15);

           exit;
       }
   }
