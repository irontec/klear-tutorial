===========
ghost count
===========

* :ref:`Volver a Tipos de Campos <tipos_de_campos>`

Los campos del tipo **ghost count** se utilizan para mostrar el número de coincidencias de un item de un List Screen
en otro List Screen con el que está relacionado.

Configuración
=============

En el model.yaml:

.. code-block:: yaml

    #include ../conf.d/mapperList.yaml
    production:
      class: \GoogleChartsSample\Model\Companies
      fields:
        name:
          title: ngettext('Name', 'Names', 1)
          type: text
          required: true
          default: true
        contador:
          title: _("Number of calls in last month relative to last year")
          type: ghost
       #   prefix: _("Coincidences ")
       #   sufix: _(" times")
          source:
            predefined: count
            mapper: \GoogleChartsSample\Mapper\Sql\Calls
       #     <<: *Calls #Se pueden usar enlaces
            relatedField: idCompany
            countCondition: "idCompany = %id% and call_start_time > DATE_SUB(now(), INTERVAL 1 MONTH)" #si se usa esta opción se ignora relatedField
            totalCondition: "idCompany = %id% and call_start_time > DATE_SUB(now(), INTERVAL 1 YEAR)" #Si no se usa esta opción se ignora la siguiente (template).
            template: '%count% de %total% (%percent%%)' #Se ignora si no se usa totalCondition

- **mapper**: Hay que indicar el mapper del List Screen con el que está relacionado. Se pueden usar enlaces.

- **relatedField**: Opcional. Nombre del campo del List Screen por el cual se relacionan.

- **countCondition**: Opcional. Condicion Where por la que filtrar la cuenta. Si se usa esta opción se ignora el relatedField.
  Se puede hacer referencia al contenido de los campos del modelo actual mediante %nombreCampo%

- **totalCondition**: Opcional. Condición where para calcular el total de registros.

- **template**: Template a usar para formatear el resultado. Si no se pone el template por defecto es '%count% / %total% (%percent%%)'.
  Si no se pone la opción totalCondition se ignora esta opción.

.. attention::

   Siempre hay que poner una de las dos siguientes opciones:

   -  **relatedField**

   -  **countCondition**
