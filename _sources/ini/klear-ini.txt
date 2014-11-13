klear.ini
=========
.. warning:: 
   Sin este archivo, los generadores "db" , "yaml" y "model-mappers" no funcionan.

Es la base para que los generadores principales sepan cómo configurar la interfaz del Klear por defecto como idiomas, zona horaria,
desarrollador responsable, etc. Es como configurar el application.ini, pero aquí solo podemos configurar los siguientes puntos:

.. contents::
   :local:
   :depth: 2

.. note::

   Si queremos configurar klear.ini. Tenemos un ejemplo bastante útil en la sección :ref:`Creación del Proyecto <klearini>`.

   
Klear Languages
---------------

Son los lenguages que emplearíamos en nuestra Interfaz del Klear lo significaría que nuestros generadores con cuántos y qué idiomas
serán necesarios para configurar nuestra base de datos, los yaml y los models-mapper. Luego por nuestra cuenta o por el cliente, se
encargarán de traducir de los literales de la interfaz por si estos no se encuentran en nuestra base de datos de idiomas del Klear.

.. code-block:: ini

   klear.languages[] = es
   klear.languages[] = en
   klear.languages[] = fr
   klear.languages[] = pt

Docs Author
-----------

Este campo es opcional, pues no destaca en la interfaz del Klear pero sí en los archivos internos. Es una formalidad para identificar
a la persona responsable del proyecto.

.. code-block:: ini

   docs.author = "Your Name"
   
.. _klearIniCountriesTimezones:

Klear Countries y Klear Timezones
---------------------------------

Estas dos campos se encuentran normalmente relacionados o al menos Timezones depende de Countries. Tiene como objetivo contener datos
estáticos de Países y Zonas horarias para hacer uso de ella según sea necesario para nuestro proyecto. 

Klear Countries
###############

.. note::
   
   La estructura de la tabla la encontramos  :ref:`aquí <tablaklearUsersPais>`.

.. caution:: 
   Si el proyecto es multi-idioma, necesita usar el generador de la base de datos y el generador de models-mapper después de crear la tabla
   antes de seguir con la configuración.



En el klear.ini necesitaríamos indicar los campos de la tabla anterior:

.. code-block:: ini

   klear.countries.table = Countries
   klear.countries.name = name
   klear.countries.code = code
   
* **code** es un campo único

* **name** es un campo Multilang (se escribirá el nombre del país en cada idioma).

* Deben estar los lenguajes definidos normalmente en **klear.ini** - **Klear Language**

* Deben estar creados los modelos para la tabla especificada.

Por último, usamos :ref:`el generador de países <generadorPaises>`.


Klear Timezones
###############

.. note::

   La estructura de la tabla la encontramos  :ref:`aquí <tablaklearUsersPais>`.

.. caution::
   Después de crear la tabla, generar nuevamente los models-mapper por el vínculo que hay con la tabla "Country" por medio del campo "CountryId".

   
En klear.ini:

.. code-block:: ini

   klear.timezones.table = Timezones
   klear.timezones.tz = tz
   klear.timezones.comment = comment
   klear.timezones.countryLink = countryId
   
* **tz** es un campo único

* **comment** es un campo opcional. Segñun el iana, es un comentario para países con múltiples timezones…

* **countryId** implica que la tabla de países tiene que existir, estar configurada en el klear.ini y estar ya importada (o hacerlo al mismo tiempo)

* Deben estar creados los modelos (de timezone, y de countries si se especifica coutnryId) para la tabla especificada.

Por último, usamos :ref:`el generador de las zonas horarias <generadorTimeZones>`.
   
