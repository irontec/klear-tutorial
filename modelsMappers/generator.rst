.. _generatorModelsMappers:

Generador
=========

Genera las clases para todas las tablas existentes de la base de datos, a menos que sean ignoradas con el tag [ignore].

Todas las clases son guardadas en la carpeta **librery** (APPLICATION_PATH/../library).

Requisitos
----------

1. Haber configurado el application.ini con los parámetros necesarios para acceder a la **base de datos** y el **appnamespace** (normalmente el nombre del proyecto).
   
.. note::
   Más detalles en :ref:`klear`
   
2. Tener las tablas creadas, como mínimo la tabla KlearUsers.

.. note::
   Con más detalles en :ref:`tablaklearUsers`

   
Generar los models y mappers
----------------------------

Usar: klear-models-mappers-generator.php [ options ]
--application|-a <string> Zend Framework APPLICATION_PATH

Ejemplo para generar las carpetas del proyecto KlearInterval, estando en la carpeta **generator**:

.. code-block:: console

   $ php klear-models-mappers-generator.php -a ../../klearInterval/web/application/
