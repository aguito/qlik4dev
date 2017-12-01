---
layout: post
title: Data Profiling
subtitle: 
date: 16/11/2017 20:15:00
tags: [qlikview, qliksense, data profiling, best practice]
gh-repo: aguito/qlik4dev
gh-badge: [star, fork, follow]
---

Una buena practica en el desarrollo de aplicaciones es garantizar la calidad de los datos, ya que son el pilar fundamental para realizar un buen analisis.

Normalmente, esta se da por hecho y a veces es motivo de grandes disgustos y requiere de un gran esfuerzo pocas veces valorado.

La inspeccion del contenido de los datos, mas conocida como __data profiling__ es un proceso continuo, que no sólo es propio del proceso de desarrollio, sino que la carga incremental de datos esta sujeta a las mismas restricciones, por lo que la correccion y monitorizacion de datos siempre estará presente.

En qlik se han desarrollado herramientas que permiten llevar a cabo procesos de profiling. Algunos lo hacen conectando la aplicacion mediante una carga binaria y mostrando un analisis muy completo en un documento independiente.

A mi personalmente me gusta incluir una hoja que me permita llevar a cabo este proceso desde el propio documento ya que asi tengo total autonomia y es muy comodo de usar durante el proceso de desarrollo, y me permite ir inspeccionando cada vez que modifico el modelo de datos, y asi ahorrarme disgustos. Yo lo he visto especialmente util cuando usamos llaves combinadas y valores de fecha, hora y como no en el tratamiento de nulos.
Obviamente es una hoja que solo es accesible a los usuarios desarrolladores.

Esta hoja la llamo __workbook__, y con esta filosofia, dejamos espacio para poder crear objetos prototipo y al mismo tiempo poder inspeccionar los datos.

El profiling aborda distintos tipos de analisis, pero de forma sencilla se suele tratar la:

+ __Exactitud__: Dentro del ámbito de exactitud se analiza el rango de los datos para identificar los estadísticos.
    + Numero de registros
    + Tipo de Dato: Texto, Numerico, Otro
    + Valor Max
    + Valor Min
    + Longitud de Cadena Max
    + Longitud de Cadena Min
+ __Unicidad__: analizar los datos identificar valores únicos
+ __Integridad__: analizar los datos para identificar valores nulos

La forma de realizar este tipo de inspección es mediante:

+ _Tablas de resultados_, a parte de los resúmenes de estadísticos permite analizar los datos desnormalizados
+ _Histogramas_ o gráficos de _ocurrencia_
+ _Análisis de Benford_ para análisis de valores numéricos


Tomando como referencia el excelente trabajo realizado por steve dark [qlikintelligence.co.uk] (
https://www.quickintelligence.co.uk/qlikview-data-profiler/) he adaptado las funcionalidades de profiling a mis documentos.

##Qlikview

En Qlikview el profiling forma parte del template que usamos normalmente. Se encuentra como he comentado en la hoja workbook. La ventaja es que en caso de necesidad se pueden copiar todos los objetos y trasladarlos a un documento que no los contenga. La estructura de los objetos es la siguiente:

[![qlikview-profiler]({{site.url}}/img/blog/qlikview_profiler.PNG )]({{site.url}}/img/blog/qlikview_profiler.PNG )

Básicamente hemos agregado algun detalle más que nos parece interesante como el _número de registros_ y la _densidad_, para no tener que consultarlo en el modelo de datos.
En cuanto a los valores y distribución para aprovechar más el espacio hemos unido en un contenedor una tabla con los valores, su distribución y en caso de ser valores numericos un análisis de Benford para detectar posibles anomalías en los valores.

Angel Monjarás hizo una presentación muy intersante en el Qlik Dev Group en Mexico. En su blog [qlikmasternlog](http://qlikmasterblog.wordpress.com/) se puede descargar.

##QlikSense

El autor recientemente ha creado la version QlikSense que también esta disponible en su página [qlikintelligence.co.uk] (
https://www.quickintelligence.co.uk/qlik-sense-data-profiler/)
Aqui la cosa se complica, como bien indica el autor del profiler en Sense no se pueden copiar objetos de un documento a otro y mrbos n distintos entorno (desktop, cloud, ...), asi que la solucion es importat un documento con todas las funcionalidades a modo de plantilla e iniciar el desarrollo. En si es la filosofia que noaotros sefuimos en view pero a dia de hoy aun no hemos creado un template de Sense que integre todas las funcionalidades que consideramos necesarias para el desarrollo, asi que habra que ponerse a ello. En Sense como  no se pueden usar contenedores, hemos decidido usar toda la paleta para realizar el profiling tal cual la propuesta.

[![qliksense-profiler]({{site.url}}/img/blog/qliksense_profiler.PNG )]({{site.url}}/img/blog/qliksense_profiler.PNG )
