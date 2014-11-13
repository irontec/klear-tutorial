======
picker
======

* :ref:`Volver a Tipos de Campos <tipos_de_campos>`

Campo del tipo fecha y hora.

Configuración
=============

.. code-block:: yaml

   fieldName:
     type: picker
     source:
       control: date
       settings:
         dateFormat: dd-mm-yy
         timeFormat: hh:mm:ss
         showSecond: false

En el parámetro **control** indicamos que tipo de picker vamos a mostrar

* **date**: solo fecha.
* **time**: solo hora.
* **datetime**: fecha y hora.

.. note::
   Si tenemos un campo en base de datos de tipo **timestamp** o **datetime**, pero tenemos configurado el picker con **date**, guardará en el campo la fecha con hora **00:00:00**

El parámetro **settings** no es obligatorio, y podemos indicar la mayoría de las propiedades que puede tener un datepicker (se muestra al final el listado completo)

Por defecto, el formato de la fecha viene definido por el **Locale del idioma que tengamos seleccionado** y la hora siempre con formato **hh:mm:ss**. Podemos indicar otro formato para la fecha u hora, que se aplicará siempre independientemente del idioma seleccionado.

.. note::
   El formato de las fechas y hora, debe ser formato aplicado en datepicker http://api.jqueryui.com/datepicker/

.. note::
   Se recomienda solo usar los formatos **dd**, **mm** y **yy** para fechas, y **hh**, **mm** y **ss** para horas.

* **timeFormat**: solo aplicable cuando **control** es datetime o time. Si ponemos de formato **hh:mm**, automáticamente el parámetro **showSecond** se pondrá a false.
* **showSecond**: solo aplicable cuando **control** es datetime o time. Si no tenemos parámetro **timeFormat**, mostrará la hora como **hh:mm**

.. note::
   Si queremos mostrar la hora como **hh:mm**, se puede hacer indicando **timeFormat: hh:mm** y sin poner **showSecond**, o sin poner **timeFormat** e indicando **showSecond: false**.

.. code-block:: php

    protected $_availableSettings = array(
        'altField' ,
        'altFormat' ,
        'appendText' ,
        'autoSize' ,
        'buttonImage' ,
        'buttonImageOnly' ,
        'buttonText' ,
        'calculateWeek' ,
        'changeMonth' ,
        'changeYear' ,
        'closeText' ,
        'constrainInput' ,
        'currentText' ,
        'dateFormat' ,
        'timeFormat' ,
        'dayNames' ,
        'dayNamesMin' ,
        'dayNamesShort' ,
        'defaultDate' ,
        'duration' ,
        'firstDay' ,
        'gotoCurrent' ,
        'hideIfNoPrevNext' ,
        'isRTL' ,
        'maxDate' ,
        'minDate' ,
        'monthNames' ,
        'monthNamesShort' ,
        'navigationAsDateFormat' ,
        'nextText' ,
        'numberOfMonths' ,
        'prevText' ,
        'selectOtherMonths' ,
        'shortYearCutoff' ,
        'showAnim' ,
        'showButtonPanel' ,
        'showCurrentAtPos' ,
        'showMonthAfterYear' ,
        'showOn' ,
        'showOptions' ,
        'showOtherMonths' ,
        'showWeek' ,
        'stepMonths' ,
        'weekHeader' ,
        'yearRange' ,
        'yearSuffix'
    );