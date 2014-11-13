=====
ghost
=====

* :ref:`Volver a Tipos de Campos <tipos_de_campos>`

Los campos del tipo **ghost** son válidos, para mostrar algo más que el simple
dato que ofrece el campo. Se pueden concatenar diversos campos, o diréctamente
definir el **HTML** que se verá en el lugar del campo.

.. contents::
   :local:
   :depth: 3

Configuración
=============

.. code-block:: yaml

   fieldName:
     type: ghost
     dirty: true
     source:
       class: Application_klear_Ghost_Item
       method: getData
       orderMethod: getOrderBy
       searchMethod: getSearchConditionsForItem
       field: modelId
       cache:
         campo: true
       conditions:
         campo: valor

* **dirty**: para que interprete el código **HTML** devuelto. Por defecto el **false**.
* **class**: clase encargada de servir los datos.
* **method**: método (función) que se encarga de servir el dato que deseamos mostrar.
   * Se recibe el **$model** en la función y se puede devolver tanto texto plano como **html**.
* **orderMethod**: función que se encarga de generar el **Order by**.
   * Se recibe el **$model** en la función.
   * Para ordenar por un campo de una tabla relacionada, tendremos que calcular el orden deseado
     y devolver los valores del campo de la tabla principal de la siguiente manera:

     .. code-block:: guess

        return "FIELD(campo,".implode(',',valoresOrdenados).")";

* **searchMethod**: Función que genera el **WHERE** para filtrar los resultados de la búsqueda.
   * Parametros recibidos:
      * **$values**: Palabras claves del buscador del klear dentro de un **array**.
      * **$model**: El modelo vacío del **YAML** que lo origina.
      * **$searchOps**: Opciones para buscar que se implementará más adelante en **Klear**.
   * La función tiene que devolver un código mysql  para buscar; por ejemplo:
      * *return "(id = 3 AND name='admin')";* si encuentra el registro
      * *return "false";* si no se encuentra nada.

Clase de ejemplo
================

.. code-block:: php

   <?php
   
   class Application_Klear_Ghost_Item extends KlearMatrix_Model_Field_Ghost_Abstract
   {
   
       public function getData($model)
       {
         # Tenemos el $model para trabajar

         # Nuestro código para generar el resultado,

         # Se puede devolver HTML si se ha definido dirty:true
         return $resultado;
       }

       public function getSearchConditionsForItem($values, $searchOps, $model)
       {
           # Tenemos un array de valores de búsqueda
           # También tenemos el $model
           # y las $searchOps
           
           # Nuestro código para generar el WHERE que se requiera
   
           # Devolvemos el WHERE que hemos generado
           return $where;
       }
   
       public function getOrderBy($model)
       {
           # En esta función también se recibe el $model
           
           # Devolvemos el valor generado para el ORDER BY
           return "FIELD(campo,".implode(',',valoresOrdenados).")"; 
       }
   
   }

.. note:: 

   Todas las clases que generemos del tipo **Ghost**, deberán implementar 
   **KlearMatrix_Model_Field_Ghost_Abstract**. 
   
   Las funciones dentro de la clase no tienen porque llamarse igual, ya que se definen
   en el **model.yaml**.