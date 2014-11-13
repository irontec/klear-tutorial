Gestión de las Bases de Datos
=============================

Como ya hemos explicado en la sección de :ref:`createProject`, Klear utiliza Phing y Klear-db-generator para trabajar con las bases de datos.
El funcionamiento se resume en dos procesos:

1. Phing ejecuta las SQL en la base de datos.
2. El Generador de Bases de Datos de Klear adapta las tablas creadas que contengan parámetros especiales (comentarios) para poder trabajar con ellas de una forma óptima.

.. toctree::
   :maxdepth: 1

   phing
   generator