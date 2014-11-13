Configuración Googlecharts
==========================

Googlecharts es un controlador de KlearMatrix que permite crear gráficos de
Google Charts para usar como pantalla principal o para poner como opción en una
list screen.

Como pantalla principal
-----------------------

A configurar en klear.yaml:


.. code-block:: yaml

    Statistics: #se puede llamar como se quiera
      default: true
      title: _("Statistics")
      class: ui-silk-chart-bar
      description: "Resumen de llamadas"

y se crea el fichero Statistics.yaml (o el nombre que hemos puesto en el klear.yaml):

.. code-block:: yaml

    production:
      main:
        module: klearMatrix
        defaultScreen: charts_screen
      screens:
        charts_screen: &statistics_Link
          class: ui-silk-chart-bar-link
          controller: googlecharts
          title: _("Statistics") #opcional
          chartGroups: #Lista de grupos de gráficos
            userInfo: # Primer grupo. No tiene gráficos definidos dentro. Se puede utilizar como cabecera
              title: _("User info") #opcional
              comment: "<h1>Esto es un comentario</h1>" #opcional. Aquí se puede usar HTML, variables de auth y gettext
              show: true #Opcional. Default true. Se pueden usar variables de auth ${auth.showChart}
            annualHistorical: #Segundo grupo
              title: _("Historical annual") #opcional
              comment: <h1>Hola que tal</h1> #opcional. Aquí se puede usar HTML, variables de auth y gettext
              show: true #opcional. Default true. Se pueden usar variables de auth ${auth.showChart}
              charts: #Opcional. Lista de gráficos
                perPatterns: #Primer gráfico. Con las opciones mínimas
                  sql: #Obligatorio. Sentencia SQL. Se pueden utilizar variables de auth.
                    "SELECT column1 as col1,
                    count(column2) as col2,
                    sum(column3) as col3
                    FROM tablename
                    WHERE ${auth.chartFilterCondition}
                    AND tablename.column4 = {parent}
                    AND column5=1
                    AND columnDate1 > DATE_SUB(now(), INTERVAL 11 MONTH)
                    GROUP BY column1"
                Columnchart: #Gráfico que no usa una sentencia SQL si no una tabla metida en el yaml.
                  title: ColumChart
                  type: ColumnChart
                  table: #Tabla de datos. Si está la opción sql se mostrarán los datos de la sentencia SQL en vez de los de la tabla.
                    - Year | Sales | Expenses
                    - 2004 | 1000 | 400
                    - 2005 | 1170 | 460
                    - 2006 | 660 | 1120
                    - 2007 | 1030 | 540
                  options:
                    title: Company Performance
                    width: 400
                    hAxis:
                      title: Year
                      titleTextStyle:
                        color: red
                totalCalls: #Otro gráfico
                  title: _("Calls per month") #Opcional
                  comment: _("Historical annual %s", "${auth.chartCompanyName}") #Opcional. Aquí se puede usar HTML, variables de auth y gettext
                  legend: _("metered") #Aquí se puede usar HTML, variables de auth y gettext #Opcional
                  type: ColumnChart #Opcional. Tipo de gráfico de Google Charts. Default ColumnChart
                  show: true #se pueden usar variables de auth ${auth.showChart} #Opcional. Default true
                  sql: #Obligatorio. Sentencia SQL a utilizar. La primera columna es el eje X. El resto son las series de valores del eje Y.
                    "SELECT MONTHNAME(callStart) as columna1,
                    count(*) as columna2,
                    sum(price) as columna3
                    FROM calls
                    WHERE ${auth.chartFilterCondition}
                    AND calls.user = {parent}
                    AND answered=1
                    AND callStart > DATE_SUB(now(), INTERVAL 11 MONTH)
                    GROUP BY MONTH(callStart)"
                  cols: #Opcional. mapeo de las columnas para poder traducirlas
                    columna1: _("Date") #La primera columna es el eje X
                    columna2: _("Number of calls") #El resto de columnas son las series de valores del eje Y.
                    columna3: _("Cost")
                  options: #Opcional Son opciones de Google Charts. Se pasan a la api tal cual.
                    legend: top
                    width: 300
                    height: 150
                    vAxis:
                      logScale: true #Muy útil cuando comparamos dos series don rangos de datos muy distintos.
    testing:
      _extends: production
    development:
      _extends: production

Debido a que la mayoría de opciones se  pueden omitir, podríamos definir un par de gráficos simplemente con el siguiente código:

.. code-block:: yaml

    production:
      main:
        module: klearMatrix
        defaultScreen: googleCharts_screen
      screens:
        googleCharts_screen: &statistics_Link
          class: ui-silk-chart-bar-link
          controller: googlecharts
          chartGroups:
            annualHistorical:
              charts:
                perPatterns:
                  sql:
                    "SELECT dest as col1,
                    count(*) as col2,
                    sum(price) as col3
                    FROM Calls
                    WHERE Calls.user = {parent}
                    AND callStart > DATE_SUB(now(), INTERVAL 11 MONTH)
                    GROUP BY dest"
                totalCalls:
                  sql:
                    "SELECT MONTHNAME(callStart) as columna1,
                    count(*) as columna2,
                    sum(price) as columna3
                    FROM Calls
                    WHERE Calls.user = {parent}
                    AND callStart > DATE_SUB(now(), INTERVAL 11 MONTH)
                    GROUP BY MONTH(callStart)"
    testing:
      _extends: production
    development:
      _extends: production


Como opción en un List Screen
-----------------------------

Se hace igual que en una pantalla princial, pero hay que añadir el **mapper** del padre y podemos utilizar el
**[format| (%parent%)]** para mostrar en el título el padre.

Para poder filtrar por el el pk del padre hay que añadir en la sentencia SQL **%parent%** que se sustituirá
por el pk del padre.


.. code-block:: yaml
   :emphasize-lines: 17,28,31,41,50

    production:
      main:
        module: klearMatrix
        defaultScreen: companiesList_screen
      screens:
        companiesList_screen: &companiesList_screenLink
              controller: list
              pagination:
                items: 25
              <<: *Companies
              title: _("List of %s %2s", ngettext('Company', 'Companies', 0), "[format| (%parent%)]")
              fields:
                options:
                  title: _("Options")
                  screens:
                    companiesEdit_screen: true
                    statistics_screen: true
                  dialogs:
                    companiesDel_dialog: true
                  default: companiesEdit_screen
              options:
                title: _("Options")
                screens:
                  companiesNew_screen: true
                dialogs:
                  companiesDel_dialog: true
        statistics_screen: &statistics_Link
          <<: *Companies
          class: ui-silk-chart-bar-link
          controller: googlecharts
          title: _("Statistics %s", "[format| (%parent%)]")
          chartGroups:
            annualHistorical:
              charts:
                perPatterns:
                  sql:
                    "SELECT dest as col1,
                    count(*) as col2,
                    sum(price) as col3
                    FROM Calls
                    WHERE Calls.user = %parent%
                    AND callStart > DATE_SUB(now(), INTERVAL 11 MONTH)
                    GROUP BY dest"
                totalCalls:
                  sql:
                    "SELECT MONTHNAME(callStart) as columna1,
                    count(*) as columna2,
                    sum(price) as columna3
                    FROM Calls
                    WHERE Calls.user = %parent%
                    AND callStart > DATE_SUB(now(), INTERVAL 11 MONTH)
                    GROUP BY MONTH(callStart)"
    testing:
      _extends: production
    development:
      _extends: production

