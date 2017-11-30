---
layout: post
title: Control de Versiones Interno
subtitle: 
date: 09/07/2017 20:15:00

gh-repo: aguito/qlik4dev
gh-badge: [star, follow]
---

Con independencia de los sistemas profesionales de control de versiones tipo git, svn, etc. algunos autores recomiendan registrar en una pestaña o seccion de script información comentada sobre la finalidad del proyecto y los cambios ya sean evolutivos o correctivos que se han realizado.

Desde mi punto de vista esta es una gran idea, pero orientada a desarrolladores. En este sentido, es interesante agregar esa información a modo de tabla INLINE para que no sólo los desarrolladores puedan disponer de ella, sino que los usuarios también conozcan las modificaciones y en que momento se realizan en el documento.

Como la finalidad es crear una tabla independiente se crea una pequeña isla en el modelo de datos, pero que tiene un mínimo impacto a nivel de rendimiento ya que normalmente no se aplica ningún tipo de filtro sobre ella. Si se trabaja en QlikView es recomendable asociar a esta tabla un estado alterno propio.

Por conevenio, y para no utilizar nombres muy largos, la pestaña/seccion se llama SVC (System Version Control) y suele encontrarse la primera.

Normalmente la informacion que se suele referenciar es el número de la versión, la fecha, el autor y una descripción de las modificaciones. Aunque se podrian agregar los campos que se consideren necesarios.

Para evitar, la asociación con futuros campos del informe, la tabla se cualifica.

~~~
 //*************************************
 //system Version Control
 //*************************************

 Qualify *;

 SVC:
 LOAD * INLINE [Version|Date|Author|Description
 0.1.0|09/09/2017|aguito|go live
 0.1.1|15/09/2017|aguito|first changes done ...

 ] (Delimiter is '|');

 Unqualify *;

~~~

Y se utiliza un delimitador distinto a la coma para que si en campo “Description” se detalla bastante información es muy posible que se utilicen comas en la redacción para hacer más sencilla la lectura.

Para referenciar en todo el docuento la vesión del informe, se puede crear en el pie de página un objeto de texto con la información de la última versión, y convertirlo en una vinculo a la página SVC con todo el detalle del control de versiones.

Para obtener la última versión se puede utilizar la función FirstSortedValue de la siguiente forma:

~~~
 ='Last Version: ' & FirstSortedValue([SVC.Version],-[SVC.Date])
~~~

Esta función devuelve la última versión documentada, tomando como referencia la última fecha en la que se ha realizado el cambio.


Finalmente, los usuarios pueden ver en la hoja SVC que se encuentra al final del documento el contenido de control de versiones en una tabla.

![SVC]({{ "/images/svc.png" | absolute_url }})
