Klear Dinámico
==============

.. note::

   Por ahora, su uso está recomendado para que un klear cambie su interfaz según el dominio que lo llama.

Para crear un Klear Dinámico o una interfaz que cambie según el dominio en el que es abierto, podemos
hacer uso del **dynamicConfigClass**.
Por ejemplo, si tenemos un Klear, este se muestra según los parámetros configurados en el archivo klear.yaml
pero si llega el caso que queramos usar un klear para
diferentes dominios como para klear-one.irontec.com, klear-two.irontec.com, klear-three.irontec.com, etc...
Todos esos dominios apuntan sólo a un Klear y en función del dominio que se conecte, pues que el
interfaz del Klear cambie como si modificaras el archivo klear.yaml. Así que para hacer uso de este código, necesitamos
incorporar los siguientes códigos:

**klear.yaml**

.. code-block:: yaml
   :emphasize-lines: 3

   production:
     main:
       dynamicConfigClass: Applicationlib_Dynamic_Config

Y la clase al que se llama **Application/web/library/Applicationlib/Dynamic/Config.php**.

.. note::

   Se puede saber el respectivo nombre del campo en cada función. Y ya en la función podemos retornar el valor que está
   por defecto en el klear.yaml o el valor que queramos ya haciendo uso de nuestros códigos, base de datos, etc.

.. code-block:: php
   :linenos:

   <?php

   class Irritruklib_Dynamic_Config extends Klear_Model_Settings_Dynamic_Abstract
   {
       public function init($siteConfig)
       {
          return $siteConfig;
       }

       public function processSiteName($sitename)
       {
           return $sitename;
       }

       public function processSiteSubName($sitesubname)
       {
           return $sitesubname;
       }

       public function processLogo($logo)
       {
           return $logo;
       }

       public function processYear($year)
       {
           return $year;
       }

       public function processLang($lang)
       {
           return $lang;
       }

       public function processLangs($langs)
       {
           return $langs;
       }

       public function processjQueryUI($jQueryUIconf)
       {
           return $jQueryUIconf;
       }

       public function processCssExtended($cssExtended)
       {
           return $cssExtended;
       }

       public function processAuthConfig($auth)
       {
           return $auth;
       }

       public function processRawCss($rawCss)
       {
           return $rawCss;
       }

       public function processRawJavascripts($rawJavascripts)
       {
           return $rawJavascripts;
       }

       public function processTimezone($timezone)
       {
           return $timezone;
       }


       public function processSignature($signature)
       {
           return $signature;
       }
   }

Ejemplo klearIntervals
----------------------

Para hacer una ejemplo sencillo, tenemos el dynamicConfigClass en el **klear.yaml**
y la clase se encuentra en **library/KlearIntervallib/Dynamic/Config.php**.

Si tienes el proyecto en tu host y puedes ingresar con la dirección **user.company.com/klearIntervals/klear**
y cambias dicho dominio por **localhost/klearIntervals/klear**, cambiará la apariencia y textos que normalmente
se configurarían en el **klear.yaml**.