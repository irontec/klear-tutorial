===========
ghost image
===========

* :ref:`Volver a Tipos de Campos <tipos_de_campos>`

Los campos del tipo **ghost image** se utilizan para mostrar una imagen desde una URL externa, construida a partir de la configuración del campo y de un campo de la base de datos.

Configuración
=============

En el model.yaml:

.. code-block:: yaml

      production:
        class: \SPRI\Model\Users
        fields:
          gravatar:
            title: _('Gravatar')
            type: ghost
            source:
              predefined: image
              url: "http://www.gravatar.com/avatar/"
              idField: email
              md5IdField: true
              params:
                s: 200

* **url**: URL base a partir de la cual se construye el resto de la URL.
* **idField**: Campo que se usa como id. Se pone inmediatamente despues de la URL base.
* **md5IdField**: Opcional. Se usa para especificar si se quiere transformar el id mediante md5. Esto es específico para Gravatar ya que este usa el md5 del e-mail. Por defecto es false.
* **params**: Opcional. Son los parámetros a pasar a la URL una vez formada. Se pueden poner los que se quiera y se transforman al formato URL (http://www.gravatar.com/avatar/md5deid?param1=value1&param2=value2)
