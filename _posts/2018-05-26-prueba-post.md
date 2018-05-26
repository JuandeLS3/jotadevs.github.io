---
layout: post
title: Hablemos sobre Big Data
description: Seguro que has oído hablar alguna vez sobre Big Data. Pero, ¿sabes todo lo que se encierra detrás realmente?
categories: code
author: juandels3
---

![small image](http://static.america-retail.com/2018/04/what-is-big-data.png)
Hola a tod@s y bienvenidos al primer post oficial del [nuevo site](https://juandels3.github.io/). En primer lugar darte las gracias por seguir al tanto de mi blog, incluso después de la migración de web, de Wordpress a Github/**Jekyll**, donde muy pronto hablaremos sobre dicha tecnología.

Hoy quería dedicar el post a un tema un tanto popular, pero complejo de definir: el **Big Data** o "Datos a gran escala" en su traducción al español. En 2013 Dan Ariely, profesor de psicología y economía conductual de la universidad Duke publicó en su muro de Facebook lo siguiente:

![small image](https://pbs.twimg.com/media/BpHiz_wCUAA_scg.png)

> El Big Data es como el sexo entre adolescentes: todo el mundo habla sobre ello, nadie sabe realmente cómo hacerlo, todo el mundo piensa que lo está haciendo, por lo que todo el mundo afirma que lo está haciendo...

La Wikipedia define el Big Data tal que así:

> Es un concepto que hace referencia a un conjuntos de datos tan grandes que aplicaciones informáticas tradicionales de procesamiento de datos no son suficientes para tratar con ellos

Entonces podemos decir que Big Data hace referencia a un conjunto de datos **tan grande** que sobrepasa la capacidad de software tradicional para poder ser **capturado, almacenado, gestionado y analizado** con el fin de obtener **valor** para crear ideas innovadoras.

## Las 4 V's del Big Data

Para que una gran cantidad de datos o Big Data puedan sernos útiles, hemos de tener muy en cuenta lo que se denominan "Las 4 V's del Big Data":

 - Volumen
	 - Necesitamos grandes cantidades de datos a tratar.
 - Variedad
	 - Deben de haber datos estructurados, no estructurados, textos, imágenes, etc...
 - Velocidad
	 - El tiempo que invertimos en tratar y procesar los datos no debe ser muy alto.
 - Valor
	 - Convertir los datos en bruto en información útil.

## ¿Tantos datos?

Uno se pone a pensar y... ¿realmente generamos tantos datos? Analicemos esto.

![small image](https://dc722jrlp2zu8.cloudfront.net/media/django-summernote/2018-01-25/40743221-d605-4813-b088-76eded6f9b7c.png)

Según muesta la imagen, en 2013 habían 4.4 Zettabytes datos digitales en internet; sin embargo, sobre 2020 serán 44 Zetabytes.

La siguiente imagen muestra la increíble cantidad de datos que procesan compañías como Facebook, Google o Twitter en sus apps en 2016 y 2017 durante **1 minuto**.

![small image](https://dc722jrlp2zu8.cloudfront.net/media/django-summernote/2018-01-25/884dc210-bbcb-473c-ab51-29b1baa4341f.png)

## Cómo se procesan tantos datos

En primer lugar, es muy necesario un procesamiento distribuido, donde todos los datos se procesen en distintas máquinas o nodos, consiguiendo así una mayor **velocidad**. 
También se debe disponer de **escalabilidad horizontal**, que es la capacidad de soportar el crecimiento de procesamiento tan solo añadiendo nuevo hardware, cuando sea necesario, obteniendo así una infraestructura escalable y tolerante a fallos, ya que siempre que caiga un nodo habrá otro sustituyéndole, siguiendo el proceso su ejecución normal.

Lo bueno de disponer de una buena infraestructura de escalabilidad horizontal, es que no se tiene por qué invertir demasiados fondos en hardware caro.

## Las 4 fases principales

Para procesar una gran cantidad de datos, se desarrollan 4 fases principales.

 - Ingesta
	 - Los datos que proceden de una fuente externa entran en nuestro nodo.
 - Persistencia
	 - Almacenamos o escribimos los datos que se acaban de ingestar.
 - Procesamiento
	 - Se procesan y analizan los datos, realizando operaciones con el fin de conseguir valor.
 - Visualización
	 - Se muestran gráficamente los resultados obtenidos.

## Un ecosistema inmenso

Para conseguir grandes datos son necesarias gran variedad de herramientas. Por esta razón, el ecosistema de Big Data es inmenso y ofrece muchas posibilidades y servicios. Muchas herramientas son de pago y otras son open source.

![small image](http://mattturck.com/wp-content/uploads/2017/05/Matt-Turck-FirstMark-2017-Big-Data-Landscape.png)

Si quereis introduciros en el mundo del Big Data es recomendable conocer todas estas herramientas y normalmente especializarse en algunas. 

## Datos y más datos

Cuesta creer que en una app o una web sean registrados todos nuestros datos, clicks, búsquedas, etc. Por eso es importante saber qué estamos compartiendo y con quiénes, aunque cada vez tengamos menos control sobre la cantidad de datos que compartimos en la red de redes. 

> Según IDC **en el año 2020, cada ser humano creará** 1,7 MB **de información cada segundo**