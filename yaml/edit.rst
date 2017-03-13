Configuración edit/new
======================

.. contents::
   :local:
   :depth: 3

A parte de las configuraciones descritas en :ref:`configuracion_general`, los
controllers **edit** y **new** tienen configuraciones propias que se definen a continuación.

.. note::

   Todas las siguientes configuraciones son válidas para **Edit**, pero no para **New**.

forcedPk y calculatedPk
-----------------------

Si queremos abrir una pantalla de Edit directamente desde el menú lateral, no disponemos
del Pk necesario para cargar los datos del registro. Tenemos dos opciones de indicar el Pk:

Indicando directamente el PK:

.. code-block:: yaml

   forcedPk: 1

Creando una clase y un método, donde haremos el cálculo necesario para devolver el Pk:

.. code-block:: yaml

   calculatedPk:
      class: App_Model_Calculated
      method: calculatedPk

.. code-block:: php

   <?php
   class App_Model_Calculated
   {
      public function calculatedPk(KlearMatrix_Model_RouteDispatcher $router)
      {
         /* nuestro código */
         return $pk;
      }
   }

disableSave
-----------

Para ocultar el botón de guardado.

.. code-block:: yaml

   disableSaveEdit_screen:
     <<: *ModelName
     disableSave: true

fullWidth
---------

Hace que el ancho del **fieldset** contenedor del formulario tenga el **width** a *"auto"*,
lo que hace que se expanda rellenando todo el espacio.

.. code-block:: yaml

   fullWidthEdit_screen:
     <<: *Model
     fullWidth: true


fixedPositions
--------------

Permite agrupar campos en una única fila.

La idea es crear grupos de campos.

.. code-block:: yaml

   fixedPositions:
     group0: # identificador del grupo, no se muestra nunca
       label: _('Metadatos') # El título no es obligatorio
       fields:
         metaDescription: 6
         metaKeywords: true # (ó 1)
     group1:
       label: _('Modo de publicación')
       fields:
         status: true
         publishedDate: true

* **label**: Título del grupo de campos, se muestra como *legend*.
* colsPerRow: Especifica el número de espacios por fila (para conseguir agrupaciones de más de una fila)
* fields: array de campos
   * Cada campo tendrá asociado un valor boolean ( **true|false** ), o numérico.
   * Si todos los campos son booleanos, el espacio se reparte equitativamente.
   * Si uno de los campos tiene valor numérico, el ancho se reparte tomando
     ese valor como guía.

conditionalBlacklist
--------------------

Campos que se añaden al **Blacklist** en función del valor.

.. code-block:: yaml

   fields:
     conditionalBlacklist:
       hasMobile:
         condition: '0'
         toHideFields:
           cellPhoneDDIId: true
           cellPhoneShortNumber: true

En este caso, cuando **hasMobile** sea igual a **0**, los campos **cellPhoneDDIId**
y **cellPhoneShortNumber** no se mostrarán.

.. attention::

   Ésta opción es válida solamente para las pantallas del tipo **Edit**.

postActionOptions
-----------------

Con esta opción podemos incluir opciones de un ListScreen (screens, dialogs y commands) en el diálogo de confirmación que sale al crear un registro nuevo.

De esta manera podemos acceder a esas opciones desde el propio diálogo sin tener que cerrarlo e ir a la opción correspondiente.

.. attention::

   Esta opción es válida solamente para las pantallas del tipo **New**.

.. code-block:: yaml

   postActionOptions:
     screens:
       sampleEdit_screen: true
       sampleList_screen: true
     dialogs:
       sampleDel_dialog: true
     commands:
       sample_command: true

actionMessages
--------------

Permite introducir diálogos antes de guardar.

.. code-block:: yaml

   actionMessages:
     before:
       title: _("Guardando elemento")
       message: _("Va a guardar un elemento.<br />¿Desea continuar?")
       actions:
         ok:
           label: _("Si")
           return: true
         cancelar:
           label: _("No")
           return: false

* **title**: Título del diálodo.
* **message**: Texto del diálogo, soporta HTML.
* **actions**: Botones del diálogo.
   * **label**: Texto del botón.
   * **return**: Si es true, se continuará con el guardado. Si es false, se aborta.


Se pueden introducir tantos diálogos como se quiera.