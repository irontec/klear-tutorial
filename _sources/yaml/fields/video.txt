=====
video
=====

* :ref:`Volver a Tipos de Campos <tipos_de_campos>`

El campo vídeo permite cargar el **feed** de una (o más) cuenta de **youtube**
o **vimeo**, previsualizar los vídeos, y guardar la dirección del que se elija.

Configuración
=============

.. attention:: 

   El campo **vídeo** requiere del tag **[video]** en la base de datos, antes de 
   utilizar los generadores.

.. code-block:: yaml 
   
   fieldName:
     type: video
       source:
       settings:
         paste: true # Allows to set video url into a text field
         feed: # Video lists to pickup desired video
           youtube:
             - RCVIDEOdotORG
             - irontecsl
           vimeo:
             - surfingmag

* **paste**: si es **true** permite pegar una dirección de un vídeo y previsualizarlo en la
  zona habilitada para ello.
* **feed**: lista de canales que queremos cargar para elegir entre sus vídeos.
   * **youtube**: **array** de nombres de usuario de **youtube**.
   * **vimeo**: **array** de nombres de usuario de **vimeo**.

Este sería el aspecto del campo vídeo teniendo en cuenta la configuración arriba dada:

.. image:: img/video.png