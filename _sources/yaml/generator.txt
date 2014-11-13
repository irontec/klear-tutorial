Generador: Yaml
===============

.. contents:: 
   :local:
   :depth: 3

El generador de **Yaml** al igual que los de **Models**\/**Mappers**
se encuentra en la carpeta *generator* de la carpeta principal de **Klear**.

.. attention::

   Es recomendable hacer un **svn up** antes de lanzar los generadores, para asegurarnos
   de estar siempre en la última versión.

Descripción
-----------

El generador crea por defecto el archivo **klear.yaml** y cada uno de los **models**.

Por otra parte, para aquellas tablas que en la **BBDD** tengan como comentario la etiqueta
**[entity]** creará el *ModelList.yaml* correspondiente.

Requisitos
----------

Los únicos requisitos que tiene el generador:

* En el archivo **application.ini**:
   * Configuración de base de datos.
   * Configuración del **namespace**.

.. note::

   El archivo **application.ini** se encuentra en:

   .. code-block:: guess

      APPLICATION_PATH/configs/application.ini

Funcionamiento
--------------

Dentro de la carpeta *generator* en la carpeta principal de **Klear**:

.. code-block:: guess

   > php klear-yaml-generator.php --application|-a <application> --enviroment|-e <enviroment> --namespace|-n <namespace> --do-not-generate-links|-L --do-not-generate-klear|-K

* **<application>** es la ruta hasta el **APPLICATION_PATH** de **Zend**.
* **<enviroment>** es el enviroment del application.ini de donde coger la configuración.
* **<namespace>** es el namespace de la aplicación. Si no se pasa, se usará el appnamespace del application.ini
* **--do-not-generate-links|-L** no genera los links a los screen/dialogs
* **--do-not-generate-klear|-K** no genera los yaml de configuración en el directorio klear

Ficheros generados
------------------

Después de lanzar el generador correctamente, los archivos resultantes serán los siguientes:

* **APPLICATION_PATH**/configs/klear/klear.yaml
* **APPLICATION_PATH**/configs/klear/\*List.yaml
* **APPLICATION_PATH**/configs/klear/models/\*.yaml
* **APPLICATION_PATH**/configs/klear/conf.d/mappers.yaml

Estos archivos, los genera si **NO** existen ya en el sistema, si existen no se sobreescriben. Para ello
el generador escribe en la carpeta **APPLICATION_PATH**/configs/klearRaw todos los archivos nuevos
cada vez que lanzamos el generador.

Por lo tanto se podrá copiar y pegar el código nuevo generado
sin pisar lo que se haya modificado en los archivos existentes.

mappers.yaml
************

Este archivo es vital que esté siempre actualizado, ya que es el que define
el archivo que le toca a cada **Entidad**.

.. attention::

   Este archivo no se sobre-escribe desde el generador, por lo que habrá que añadir las
   líneas nuevas comparando el archivo actual con el generado en la carpeta **klearRaw**

En los archivos de configuración se utilizan los **alias** de cada **entidad**, y este archivo
es el que define que archivo debe cargar el sistema para cada **alias**.

.. code-block:: yaml

   mappers:
     - &KlearUsers {mapper: \KlearInterval\Mapper\Sql\KlearUsers, modelFile: KlearUsers}
     - &News {mapper: \KlearInterval\Mapper\Sql\News, modelFile: News}
     - &Sections {mapper: \KlearInterval\Mapper\Sql\Sections, modelFile: Sections}
     - &SubSections {mapper: \KlearInterval\Mapper\Sql\SubSections, modelFile: SubSections}
     - &Developers {mapper: \KlearInterval\Mapper\Sql\Developers, modelFile: Developers}
     - &Multifields {mapper: \KlearInterval\Mapper\Sql\Multifields, modelFile: Multifields}
     - &Projects {mapper: \KlearInterval\Mapper\Sql\Projects, modelFile: Projects}