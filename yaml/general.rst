.. _configuracion_general:

Configuración general
=====================

.. contents:: 
   :local:
   :depth: 3
   

Los screen donde controller es de tipo **list|new|edit** tienen varias
configuraciones compartidas, que se explican en esta sección. 
Estos archivos están en **APPLICATION_PATH**/configs/klear. 

Configuración
-------------

El siguiente fichero se muestra como ejemplo para explicar seguidamente
cada uno de los componentes del mismo:

.. code-block:: yaml

   screens:
    Contacts:
      controller: list
      mapper: \Mappers\Sql\Contacts
      modelFile: Contactos
      title: _("Listado de contactos")
      class: ui-silk-add
      label: false
      shortcutOption: "L"
      filterField: companyId
      forcedValues:
        companyId: 2
        active: 1
      rawCondition: 'estado != "incompleto"'
      fields:
        order:
          firstName: true
          lastName: true
          company: true
        blacklist:
          active: true
          phone: true
        whitelist:
          contactId: true
        readOnly:
          phone: true
          dateFrom:
            conditions:
              defaultLanguageId: 1
              state:
                - ongoing
                - done
                - closed
                - rejected
      options:
        screens:
          NewContact: true
          EditContact: true
        dialogs:
          DeleteContact: true
        default: EditContact
      info:
        type: box
        position: left
        icon: help
        text: _("Esto es el texto mostrado al desplegar la ayuda.")
        label: _("¿Necesitas ayuda?")

general
-------

Opciones que se repiten para todo tipo de **screen**\'s (list, edit y new) .

* **controller**: tipo de controlador a usar, **list**, **edit** o **new**.
* **mapper**: mapper que va a cargar los datos. [#mapper]_
* **modelFile**: archivo de modelo que se encuentra en **APPLICATION_PATH**/configs/klear/model/. [#mapper]_
* **title**: título de la pantalla.
* **class**: icono a mostrar, se pueden ver en el siguiente `listado <https://klear-wiki.irontec.com/silk/>`_.
* **label**: mostrar u ocultar el texto al lado del icono. Por defecto es **true**.
* **labelOnEdit**: habilitar la etiqueta siempre que se invoque desde un edit o sustituirla con un string.
* **labelOnList**: habilitar la etiqueta siempre que se invoque desde un list o sustituirla con un string.
* **labelOnEntityPostSave**: habilitar la etiqueta siempre que se invoque desde un EntityPostSave o sustituirla con un string. 
* **filterField**: campo por el cual se filtraría el **screen** actual, si tuvieramos **pk** proveniente de una pantalla padre.
* **forcedValues**: valores forzados manualmente, al listar se filtra y al guardar se guarda el valor seteado. [#null]_
* **rawCondition**: se filtran los datos por la **SQL** definida.
* **shortcutOption**: Siempre que el item sea invocado como option, se bindeará el shortcut Ctrl+Alt+[LETRA] como lanzador de la opción. (siempre que no sea uno de los shortcuts "oficiales").

fields
------

Opciones que afectan a la visualización de los campos del modelo de datos.

* **order**: indica el orden en que se van a mostrar los campos.
  Si no se pone **order**, los campos se muestran en el orden que estén generados en el modelo de datos.
  Si solo se ponen algunos campos en order, muestra esos campos en ese orden y
  el resto como esté ordenado en el modelo de datos. No se tiene en cuenta si el valor es **true** o **false**.
* **readOnly**: marca los campos listados como **de solo lectúra**.
  Útil para los casos en los que queramos crear un campo en el **New** pero no
  queramos permitir editarlo en el **Edit**.
* **blacklist**: campos que no queremos mostrar en la pantalla. Con valor **true** no se muestra, con valor **false** se muestra.
* **whitelist**: esta configuración es únicamente para mostrar la clave primaria, ya que es el único campo oculto por defecto.
  No se tiene en cuenta si el valor es **true** o **false**.

options
-------

Opciones que se muestran para cada línea de registro en el caso de las **Listas**,
y en la parte inferior en el caso de las pantallas de **Edit** y **New**.

* **screen**: pantallas a las que se podrá acceder desde la actual. Con valor **true** se muestra y a la inversa.
* **dialogs**: dialogos que serán accesibles desde la pantalla actual. Con valor **true** se muestra y a la inversa.
* **default**: pantalla que se abre por defecto al pinchar sobre un registro. [#soloLista]_

info
----

* **type**: el tipo de ayuda que queremos mostrar. Los valores posibles son **tooltip** (por defecto), **box** y **boxinfo**.
* **position**: la posición donde queremos mostrar la ayuda. **Left** o **right**.
* **icon**: icono a mostrar, por defecto *help*. Icono del tipo *ui-silk-* + icon, de esta `lista <https://klear-wiki.irontec.com/silk/>`_.
* **text**: el texto que se muestra en la ayuda. Permite introducir **HTML**.
* **label**: el título de la ayuda.

------------

.. [#mapper] Estos dos atributos normalmente se setean con el alias del fichero mappers.yaml.
.. [#null] Para añadir *null*, utilizar mayúsculas (**NULL**) .
.. [#soloLista] Esta opción es válida únicamente en aquellas pantallas del tipo **lista**.

