password
========

* Volver a :ref:`tipos_de_campos`

.. image:: ../../databases/img/password.png

Funciona como un campo **type: text** solo ocultando los caracteres escritos.

.. code-block:: yaml
   :emphasize-lines: 3,5

    pass: 
      title: _('Pass')
      type: password
      required: true
      adapter: Blowfish

Configuración
-------------

adapter
^^^^^^^

Aquí podemos indicar el tipo de encriptación de la contraseña a guardar en nuestra base de datos, en el que podemos elegir entre:

- Blowfish (Por defecto) [#codeBlowfish]_
- Sha1 [#codeSha1]_
- Md5 [#codeMd5]_
- Sha_Salt
- Md5_Salt

--------------

.. [#codeBlowfish] Blowfish usa bloques de 64 bits y claves que van desde los 32 bits hasta 448 bits.

.. [#codeSha1] SHA1 produce una salida resumen de 160 bits (20 bytes) de un mensaje que puede tener un tamaño máximo de 2 :sup:`64` bits. SHA1("") = da39a3ee5e6b4b0d3255bfef95601890afd80709

.. [#codeMd5] La codificación del MD5 de 128 bits es representada típicamente como un número de 32 dígitos hexadecimal. El siguiente código de 28 bytes ASCII será tratado con MD5 y veremos su correspondiente hash de salida: MD5("Generando un MD5 de un texto") = 5df9f63916ebf8528697b629022993e8