---
layout: post
title: Subresource Integrity (SRI) en Drupal 7
description: Implementación de Subresource Integrity en Drupal 7 para mejorar la seguridad de los recursos externos.
categories: code
author: juandels3
---

![small image]({{site.baseurl}}/images/drupal-security.jpg)

¡Hola! Hoy quiero compartir una problemática que me he encontrado en un proyecto Drupal 7, además de cómo se ha solucionado.
Pero antes de empezar, ¡un poco de contexto!

# Contexto

La problemática en particular se da cuando el cliente nos solicita la mejora de la seguridad en uno de los portales que corren en un Drupal 7. El cliente nos adjunta un informe de seguridad
donde, a través de ciertas herramientas como [Security Headers](https://securityheaders.com) o el [Observatorio de Mozilla](https://observatory.mozilla.org/)  vienen indicadas las vulnerabilidades
mediante un análisis a las cabeceras y elementos de la página web.

El objetivo a partir de este punto es corregir estas vulnerabilidades y obtener una mejor puntuación. Hasta aquí todo bien, ¿no?

Uno de los puntos más críticos y que más puntos restan en la revisión del Observatorio de Mozilla es precisamente la cabecera SRI o Subresource Integrity (Integridad de los recursos, en su traducción al español).
Esta característica permite al navegador identificar si un recurso externo no ha sido manipulado o vulnerado, ya que éste tiene dos atributos que identifican exactamente cómo son; algo así con el "ADN de un recurso externo".

- **Integrity**: es el primer atributo, y contiene una cadena o hash codificado en base64 y que certifica que el recurso externo no ha sido manipulado desde que se creó.
- **Crossorigin**: es el segundo atributo, y gestiona cómo tratará el navegador este recurso. Normalmente se describe como "anonymous" para recursos externos con un dominio distinto al origen y que no requiere credenciales durante la solicitud a éste.

Ya tenemos el contexto y la problemática sobre la mesa. Ahora, ¿cómo implementamos la Subresource Integrity en todos los recursos externos de nuestro site Drupal 7?

## Solución con AdvAgg Subresource Integrity

Desde Drupal 8 en adelante existen muchas facilidades con respecto a la implementación de SRI. En Drupal 7 la cosa es distinta, y dependiendo de los recursos externos que tengamos en la web la cosa se puede complicar.

El primer paso es instalar el módulo AdvAgg y el submódulo AdvAgg Subresource Integrity. Este módulo nos automatizará la adición de SRI a casi todos los recursos externos del portal, pudiendo configurar el nivel de integridad del recurso (el hash que se genera).

![small image]({{site.baseurl}}/images/sri-1.png)

Una vez borres cachés, muchos de tus recursos externos contarán con SRI de forma inmediata, simplemente maravilloso.

En este punto solo nos queda comprobar desde el Observatorio de Mozilla si todos nuestros recursos tienen SRI habilitado y celebrarlo con una cerveza.

![small image]({{site.baseurl}}/images/sri-2.png)

Oh wait... esa cerveza tendrá que esperar.

## Solución manual

A veces no todo es tan sencillo y bonito (ejem, Drupal 7 codazo, codazo) y requiere que nos remanguemos y pensemos. En este punto, solo nos queda una forma de arreglar esto: localizar y agregar SRI a los recursos externos necesarios de forma manual. ¡Empecemos! Te prometo que no será para tanto...

### Localización de recursos


El Observatorio de Mozilla nos dice "qué falla pero no quiénes lo provocan". Para localizar los recursos externos que no tienen habilitado SRI tenemos dos opciones:
- Opción A: buscar en el inspector de elementos del navegador y localizarlos uno a uno.
- Opción B: utilizar la herramienta que te voy a indicar y terminar antes de la hora de comer.

La herramienta que me ayudó mucho es [SRI-CHECK]([https://github.com/4armed/sri-check]). Esta sencilla y potente herramienta requiere de python pip y se encarga de analizar una URL en busca de recursos que no tienen SRI implementado. Una vez instalado, nos muestra lo siguiente:

![small image]({{site.baseurl}}/images/sri-3.png)

Tengo 5 recursos externos que se están cargando sin SRI. Además, dos de ellos se están cargando con HTTP, lo cual resta muchos más puntos desde el Observatorio de Mozilla por ser más inseguro.


### Implementación de SRI

Para implementar SRI en los recursos que hemos localizado,  seguiremos los siguientes pasos:

1. Crear un módulo custom para dicha funcionalidad; por ejemplo, "mymodule_sri" o "sri".
2. Crearemos el fichero "sri.module".
3. Implementamos el hook_js_alter para sobreescribir los atributos de todos estos recursos. Te dejo un fragmento de código con un ejemplo:

```
/**
 * Implements HOOK_js_alter().
 */
function sri_js_alter(&$javascript) {
  // Implements SRI.
  $javascript['//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js']['attributes']['crossorigin'] = 'anonymous';
  $javascript['//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js']['attributes']['integrity'] =
    'sha384-aBL3Lzi6c9LNDGvpHkZrrm3ZVsIwohDD7CDozL0pk8FwCrfmV7H9w8j3L7ikEv6h';
}
```

Para introducir el parámetro "**Integrity**" puedes obtener un hash para tu recurso de forma sencilla aquí: [https://www.srihash.org/](https://www.srihash.org/). Introduce el sha (la firma) de tu recurso proporcionado por esta web en el atributo "integrity" y... ¡listo!
Para solventar el problema de HTTP, puedes utilizar este mismo hook y añadir lo siguiente para sobreescribir el src:

```
$javascript['//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js']['data'] =
  'https://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js';
```

El segundo recurso externo marcado en rojo en la imagen anterior no pude cambiarlo desde aquí, ya que no es un javascript. Para solventar esto busqué en el core de dónde procedía esta url y encontré que es una especificación W3C de GRDDL (un tipo de marcado).  Utilicé un template_preprocess_html para sobreescribir la variable que contenía dicha url, de la siguiente manera:

```
/**
 * Implements template_preprocess_html().
 */
function mymodule_preprocess_html(&$variables) {
  // Change http provided by the core to https.
  $variables['grddl_profile'] = 'https://www.w3.org/1999/xhtml/vocab/';
}
```

¡Recuerda que esto va en el fichero **template.php de tu tema custom**!

# Conclusión

La conclusión de este post es que no todo es lo que parece y que es necesario prestar más atención a la seguridad de nuestro sitio web.
¡Un abrazo y hasta la próxima!