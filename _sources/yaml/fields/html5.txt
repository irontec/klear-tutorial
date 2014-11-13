html5
=====

* Volver a :ref:`tipos_de_campos`

Html5 [#html5]_ es una versión que incorpora nuevas mejoras en los tipos de campos, tanto en validaciones como en forma. Aunque es una gran novedad, no todos los navegadores
reconocen o soportan dichos parámetros, por lo que Klear solo ha incorporado las que son reconocidas por la mayoría. Por ejemplo:

.. contents::
   :local:
   :depth: 2

url
---

Este campo verifica que su valor sea una Url valida, de lo contrario el mensaje de error se ejecutara mediante el “h5Validate”.

.. code-block:: yaml
   :emphasize-lines: 3,5

    campoText: 
      title: _('Campo Text')
      type: html5
      source:
         control: url


email
-----

Este campo verifica que su valor sea un email valido, de lo contrario el mensaje de error se ejecutara mediante el “h5Validate”.

.. code-block:: yaml
   :emphasize-lines: 4,6

    email: 
      title: ngettext('Email', 'Emails', 1)
      required: true
      type: html5
      source:
        control: email

También se puede hacer una validación de dominio con el “checkEmail” y la personalización del mensaje que saldrá en el pop-up de error.

.. code-block:: yaml
   :emphasize-lines: 4,6-11

    email: 
      title: ngettext('Email', 'Emails', 1)
      required: true
      type: html5
      source:
        control: email
        checkEmail: true
        invalidMailError:
          i18n:
            es: Email inválido
            en: Invalid email

range
-----

Este campo es numérico entero, consiste en crear un intervalo (*mixRange* y *maxRange*) de números para luego añadir dicha cantidad en la base de datos.

.. code-block:: yaml
   :emphasize-lines: 3,5-7

    campoText: 
      title: _('Campo Text')
      type: html5
      source:
         control: range
         minRange: 25
         maxRange: 55

---------

.. [#html5] HTML5 (HyperText Markup Language, versión 5) es la quinta revisión importante del lenguaje básico de la World Wide Web, HTML. Todavía se encuentra en modo experimental.