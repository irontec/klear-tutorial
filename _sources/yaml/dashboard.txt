Configuración Dashboard
=======================

Dashboard es un controlador de KlearMatrix que permite listar como pantalla principal un resumen de todos los klearmatrixList_Controller.

a configurar en klear.yaml:


.. code-block:: yaml

  Dashboard:
    default: true
    title: Panel de control
    class: ui-silk-bricks
    description: Vista general de todas las entidades

y se crea el fichero Dashboard.yaml:

.. code-block:: yaml

   production:
     main:
       module: klearMatrix
       defaultScreen: Dashboard_screen
   
     screens:
       Dashboard_screen:
         controller: dashboard
         action: index
         title: "Vista general de la plataforma"
         useExplain: false # Este valor poner solo a true cuando hay tablas con más de 10.000 registros. Si no, es que falla siempre!!
         sectionsBlackList: # sirve para eliminar del dashboard automático las secciones que no queramos que aparezcan
           - CompaniesAdminUsersListEdit
           - yseguimosquitando
           
   testing:
     _extends: production
   staging:
     _extends: production
   development:
     _extends: production

Automágicamente, se listarán agrupados por secciones, todos los klearmatrix::list