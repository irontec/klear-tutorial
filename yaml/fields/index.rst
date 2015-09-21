.. _tipos_de_campos:

Tipos de Campos
===============

Klear cuenta con variedades de campos y estás se deben incorporar según a las necesidades del proyecto para que el usuario sepa guardar los datos de una
manera rápida y sin complicaciones. Estos tipos de campos, solo pueden ser incorporadas o modificadas en los **YAML MODEL** *(klear/model)*.
Por formalidad, se tiene que indicar siempre el tipo en cada campo, sino será considerado como un campo del **type: text** por defecto.
En orden alfabético tenemos:

.. toctree::
   :maxdepth: 1

   checkbox
   file
   ghost
   ghostconcat
   ghostcount
   ghostlist
   ghostimage
   html5
   map
   multiselect
   number
   password
   picker
   select
   text
   textarea
   video

Propiedades comunes
^^^^^^^^^^^^^^^^^^^

Todos estos campos tiene un conjunto de propiedades que son comunes para todos:

* **title**: Nombre que se visualizará para el campo. Si se utiliza la nomenclatura de gettext "_('Nombre_campo')", se intentará traducir usando los archivos de configuración de idiomas
* **required**: Indica si el campo es obligatorio o no.
* **type**: Indica el tipo de campo que se trata. Puede seleccionarse cualquiera de los listados arriba.
* **showSize**: Mostrar el tamaño que ocupa el *valor* actual del campo en BBDD.
