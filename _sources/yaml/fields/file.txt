file
====

* Volver a :ref:`tipos_de_campos`

.. image:: ../../databases/img/file.png

Este sección es originado por el tag :ref:`generatorDataBaseFile` que se encuentra en el comentario del campo de la tabla.

.. contents::
   :local:
   :depth: 3

Ejemplo de un campo **type: file**:


.. code-block:: yaml
   :emphasize-lines: 3,5-45
   
    archivo: 
      title: _('Archivo')
      type: file
      required: true
      source: 
        data: fso
        size_limit: 20M
        extensions:
          - jpg
          - png
        options: 
          download: 
            external: true
            type: command
            target: ArchivoDownload_command
            icon: ui-silk-bullet-disk
            title: _("Download file")
            onNull: hide
          delete:
            type: dialog
            target: ArchivoDelete_command
            icon: ui-silk-bin
            title: _("Delete file")
            onNull: hide
          upload: 
            type: command
            target: ArchivoUpload_command
            title: _("Upload file")
            class: qq-uploader
            onNull: show
          preview: 
            target: ArchivoPreview_command
            type: command
            class: filePreview
            external: 1
            props: 
              width: 150
              height: 150
            crop: 1
            onNull: hide
          previewList: 
            target: ArchivoPreview_command
            type: command
            class: filePreview
            listController: 1
            external: 1
            props: 
              width: 30
              height: 30
            crop: 1
            onNull: hide
            
Configuración
-------------

Los parámetros que pueden ser configurados en este campo.

size_limit
^^^^^^^^^^

Límite máximo del archivo subido, en el ejemplo anterior es 20M [#unidaMedida]_ el peso máximo del archivo.

extensions
^^^^^^^^^^

Listar los tipos de archivos que están permitidos para subir en nuestro servidor. En nuestro ejemplo, tenemos las exteniones de imágenes [#extensionImage]_ *jpg* y *png*.

options
^^^^^^^

download
########

Permite la descarga del archivo en la interfaz.

.. attention::
   Si se quiere emplear esta opción, asegurarse que el siguiente código también esté incorporado en el **YAML LIST** que llamará dicho campo.
   
.. code-block:: yaml
   :emphasize-lines: 2-6
   
     commands: 
       ArchivoDownload_command: 
         <<: *Model
         controller: File
         action: force-download
         mainColumn: archivo
   
delete
######

Permite la eliminación del archivo en la interfaz.

.. attention::
   Si se quiere emplear esta opción, asegurarse que el siguiente código también esté incorporado en el **YAML LIST** que llamará dicho campo.
   
.. code-block:: yaml
   :emphasize-lines: 2-6
   
     dialogs: 
       ArchivoDelete_command: 
         <<: *Model
         controller: File
         action: delete
         mainColumn: archivo

upload
######

Permite la subida del archivo en la interfaz.

.. attention::
   Si se quiere emplear esta opción, asegurarse que el siguiente código también esté incorporado en el **YAML LIST** que llamará dicho campo.
   
.. code-block:: yaml
   :emphasize-lines: 2-6
   
     commands: 
       ArchivoUpload_command: 
         <<: *Importadores
         controller: File
         action: upload
         mainColumn: archivo

preview
#######

Solo para los archivos del tipo imagen [#extensionImage]_ para ver una visualización de dicha imagen en las ventanas **NEW** o **EDIT**.

previewList
###########

Solo para los archivos del tipo imagen [#extensionImage]_ para ver una visualización de dicha imagen en el **LIST**.

.. attention::
   Si se quiere emplear esta opción, asegurarse que el siguiente código también esté incorporado en el **YAML LIST** que llamará dicho campo.
   
.. seealso:: Esta opción puede ser usado tanto para el **preview** y **previewList**

.. code-block:: yaml
   :emphasize-lines: 2-6
   
     commands: 
       ArchivoPreview_command: 
         <<: *Importadores
         controller: File
         action: preview
         mainColumn: archivo

.. [#unidaMedida] "M" es la unida de medida en Megabyte de un archivo.
.. [#extensionImage] Las extensiones más usadas y conocidas son jpg, jpeg, png y gif.
