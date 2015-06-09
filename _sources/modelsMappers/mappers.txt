Mappers
=======

Vamos a usar las funciones ya definidas por defecto por el Generador, aunque se pueden crear aún más según el objetivo deseado.

Buscar y Coger datos (find o fetch)
-----------------------------------

Klear cuenta con muchas opciones para coger datos, ya sea única, varias que tengan un valor en común o todas que tengamos
en una tabla. Esto depende de lo que queramos hacer, así que listaré las que Klear tiene disponible con un ejemplo (archivo MapperAbstract.php).

.. code-block:: php
   :linenos:
   :emphasize-lines: 21,25,29,33,37,41,45,48,51,59,66

   <?php

   class IndexController extends Zend_Controller_Action
   {

       public function init()
       {
           /* Initialize action controller here */
       }

       public function indexAction()
       {
           $mapper = new \KlearInterval\Mapper\Sql\Categories();

           // SE PUEDE ELEGIR CUALQUIER DE ESTAS OPCIONES SEGÚN SU FINALIDAD
           // FUNCIONES DEL MapperAbstract.php

           // ******************************************************************

           // $primaryKey = La ID del dato de la Tabla, en este caso la de Categorías
           $dato = $mapper->fetch($primaryKey);

           // Obtiene todos los datos de la tabla
           // Para listar, se necesita FOREACH
           $datos = $mapper->fetchAll();

           // Obtiene todos los datos de la tabla en formato array
           // Para listar, se necesita FOREACH
           $datos = $mapper->fetchAllToArray();

           // Obtiene todos los datos y las divide en paginadores
           // Para listar, se necesita FOREACH
           $datos = $mapper->fetchAllToPaginator();

           // Obtiene todos los datos en modo listado
           // Para listar, se necesita FOREACH
           $datos = $mapper->fetchList();

           // Obtiene los datos en modo listado array
           // Para listar, se necesita FOREACH
           $datos = $mapper->fetchListToArray();

           // Obtiene los datos en modo listado y las divide en paginadores
           // Para listar, se necesita FOREACH
           $datos = $mapper->fetchListToPaginator();

           // $where = "name = test6", y obtiene el primer dato que encaje con la comparativa
           $dato = $mapper->fetchOne($where);

           // $primaryKey = La ID del dato de la Tabla, en este caso la de Categories
           $dato = $mapper->find($primaryKey);

           // $field = 'name';
           // $field = array('name'); si se quiere comparar con más campos
           // $value = 'test6';
           // $value = array('test6'); tiene que tener el mismo que field
           // Listará todos los datos que cumplan con la condición
           // Para listar, se necesita FOREACH
           $datos = $mapper->findByField($field,$value);

           // $field = 'name';
           // $field = array('name'); si se quiere comparar con más campos
           // $value = 'test6';
           // $value = array('test6'); tiene que tener el mismo que field
           // Listará el primer dato que cumpla con la coindición
           $dato = $mapper->findOneByField($field,$value);

           // ******************************************************************


           // Los $dato solo requieren de un
           $dato->getName();

           // Los $datos require de un foreach
           foreach($datos as $dato) {
               $dato->getName();
           }
       }
   }


Actualizar datos (find o fetch, set, save)
------------------------------------------

.. code-block:: php
   :linenos:
   :emphasize-lines: 39-41,44-48

   <?php

   class IndexController extends Zend_Controller_Action
   {

       public function init()
       {
           /* Initialize action controller here */
       }

       public function indexAction()
       {
           $mapper = new \KlearInterval\Mapper\Sql\Categories();

           // SE PUEDE ELEGIR CUALQUIER DE ESTAS OPCIONES SEGÚN SU FINALIDAD
           // FUNCIONES DEL MapperAbstract.php

           // ******************************************************************


           // $field = 'name';
           // $field = array('name'); si se quiere comparar con más campos
           // $value = 'test6';
           // $value = array('test6'); tiene que tener el mismo que field
           // Listará todos los datos que cumplan con la condición
           // Para listar, se necesita FOREACH
           $datos = $mapper->findByField($field,$value);

           // $field = 'name';
           // $field = array('name'); si se quiere comparar con más campos
           // $value = 'test6';
           // $value = array('test6'); tiene que tener el mismo que field
           // Listará el primer dato que cumpla con la coindición
           $dato = $mapper->findOneByField($field,$value);

           // ******************************************************************


           // Después de que se haya encontrado un dato individual, se puede hacer uso del código SET y SAVE
           $dato->setName('Dato para cambiar');
           $dato->save();


           // Si son datos agrupados, hace falta el foreach
           foreach($datos as $dato) {
               $dato->setName('Dato para cambiar');
               $dato->save();
           }
       }
   }


Borrar datos (find o fetch, delete)
-----------------------------------

.. code-block:: php
   :linenos:
   :emphasize-lines: 39-41,43-46

   <?php

   class IndexController extends Zend_Controller_Action
   {

       public function init()
       {
           /* Initialize action controller here */
       }

       public function indexAction()
       {
           $mapper = new \KlearInterval\Mapper\Sql\Categories();

           // SE PUEDE ELEGIR CUALQUIER DE ESTAS OPCIONES SEGÚN SU FINALIDAD
           // FUNCIONES DEL MapperAbstract.php

           // ******************************************************************


           // $field = 'name';
           // $field = array('name'); si se quiere comparar con más campos
           // $value = 'test6';
           // $value = array('test6'); tiene que tener el mismo que field
           // Listará todos los datos que cumplan con la condición
           // Para listar, se necesita FOREACH
           $datos = $mapper->findByField($field,$value);

           // $field = 'name';
           // $field = array('name'); si se quiere comparar con más campos
           // $value = 'test6';
           // $value = array('test6'); tiene que tener el mismo que field
           // Listará el primer dato que cumpla con la coindición
           $dato = $mapper->findOneByField($field,$value);

           // ******************************************************************


           // Después de que se haya encontrado un dato individual, se puede hacer uso del delete()
           $dato->delete();


           // Si son datos agrupados, hace falta el foreach
           foreach($datos as $dato) {
               $dato->delete();
           }
       }
   }

Crear nuestros propios Mappers
------------------------------

Este tutorial es parecido al :ref:`modelsCreate`, pero por formalidad hay que saber diferenciarlos para poder
organizarnos con las nuevas funciones. Los Mappers normalmente son usadas para recoger o modificar la información
ya guardada, así que lo que nos interesa es saber cuánta información necesitamos y cómo la queremos.

La carpeta **Mapper**, que se encuentran en nuestra carpeta **library**, tiene la carpeta **Sql** que contiene
la carpeta Raw que se regenera cada vez que se usa el :ref:`generatorModelsMappers` pero aparte de ella, esta a su
vez contiene la carpeta **DbTable** que también se vuelve a generar por lo que que solo podrémos editar cualquier
archivo que se encuentren fuera de ellas. Un ejemplo del proyecto Klear Interval:

.. attention:: Solo podemos editar las que tenemos resaltadas en el siguiente código, el resto se podrá sobreescribir.

.. code-block:: console
   :emphasize-lines: 4,27-39,61-64

   $ tree library/KlearInterval/Mapper/
   library/KlearInterval/Mapper/
   └── Sql
       ├── Categories.php
       ├── DbTable
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
       │   ├── Multifields.php
       │   ├── News.php
       │   ├── Projects.php
       │   ├── RelNewsCategories.php
       │   ├── Rowset.php
       │   ├── Sections.php
       │   ├── SubSections.php
       │   ├── Tablacosas.php
       │   └── TableAbstract.php
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
       │   ├── MapperAbstract.php
       │   ├── Marcas.php
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

Modificaremos el archivo **Categories.php** de la carpeta **Mapper/Sql**. Agregando la función **getDateData()**, es una
sencilla función para coger el campo **cleanName** de la tabla **Categories** con solo dar la **ID** y a parte quiero que se me
devuelva la **fecha y hora** de la consulta.

.. code-block:: php
   :linenos:
   :emphasize-lines: 23-30

   <?php

   /**
    * Application Model Mapper
    *
    * @package Mapper
    * @subpackage Sql
    * @author User Name
    * @copyright ZF model generator
    * @license http://framework.zend.com/license/new-bsd     New BSD License
    */

   /**
    * Data Mapper implementation for KlearInterval\Model\Categories
    *
    * @package Mapper
    * @subpackage Sql
    * @author Lander Ontoria Gardeazabal
    */
   namespace KlearInterval\Mapper\Sql;
   class Categories extends Raw\Categories
   {
       public function getDateData($id) {

           $mapper = new \KlearInterval\Mapper\Sql\Categories();

           $data = $mapper->find($id);

           return array($data->getCleanName(), date('d-M-Y H:i:s'));
       }
   }

La manera de acceder a tal controlador sería de la siguiente manera:

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
           $mapper = new \KlearInterval\Mapper\Sql\Categories();

           $id = 4;

           $data = $mapper->getDateData($id);

           var_dump($data); // Devolvería array('test4', '26-02-2014 09:56:55');

           exit;
       }
   }

