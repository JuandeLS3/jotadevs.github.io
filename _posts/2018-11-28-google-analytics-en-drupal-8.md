---
layout: post
title: Google Analytics en Drupal 8
description: Aprovecha todas las ventajas y datos en bruto que te facilita Google Analytics con el módulo para Drupal 8
author: jotadevs
categories: code
---

![small image]({{site.baseurl}}/images/analytics.png)



> Todos los contenidos de este blog los podrás encontrar en Drupal Sapiens ([https://drupalsapiens.com/es](https://drupalsapiens.com)), la nueva plataforma de Divulgación y Cursos de Drupal, ¡con contenidos muy interesantes!

Google Analytics es una herramienta muy potente sacada de la mano de Google que permite **monitorizar y analizar datos de nuestra página web.** Gráficos, datos en tiempo real, visitas, ubicaciones, audiencia... son solo algunas de todas las utilidades que nos proporciona Google Analytics, además, desde un panel modular totalmente personalizable y amigable.

Con Drupal 8 tenemos incluido en el core el módulo "*Statistics*", que puede ser útil en blogs o páginas simples o que no requieran tantas estadísticas. Pero a la hora de trabajar en un proyecto mayor, se nos quedará corto, y para ello existe el módulo de [Google Analytics](https://www.drupal.org/project/google_analytics), el cual se encuentra actualmente con la versión 8.x.2.3 estable.

Para comenzar, necesitaremos disponer de composer en nuestro proyecto, el cual explico cómo obtenerlo desde [el post Utilidades para Drupal I](https://juandels3.github.io/utilidades-drupal/). 
Una vez instalado y listo, descargaremos el módulo con composer.

    composer require drupal/google_analytics

Una vez descargado, tocará habilitarlo en nuestro Drupal 8. Para ello tenemos dos opciones:

 - Hacerlo por interfaz desde **Extend > Marcar el módulo "Google Analytics" > Presionar botón "Install"**.
 - Hacerlo mediante la herramienta drush, la cual [explico aquí como instalar](https://juandels3.github.io/utilidades-drupal/).

El comando sería el siguiente:

    drush en -y google_analytics

Una vez habilitado, podemos acceder a su configuración desde la interfaz de administración en **Configuration > System > Google Analytics**.

La única configuración que debemos editar será el "Web Property ID" que normalmente comienza por **"UA-XXXXX"**. Para obtener este código es obligatorio registrarse en Google y crear una Propiedad. La propiedad será el sitio web el cual llevará asignado el Web Property ID, es decir, nuestro sitio web. 
Una vez hayamos registrado nuestra cuenta y la propiedad, Google nos facilitará un ID de seguimiento, que será el que debamos introducir en nuestro Drupal. Cuando guardemos la configuración, Google Analytics ya trackeará las visitas, usuarios activos, etc...

## Tracking scope

El módulo nos ofrece también poder configurar el alcance del seguimiento, es decir, hasta donde Google Analytics captará datos (clicks, visitas...) de nuestra web.

 - Domains. Podemos indicar en qué dominios Google Analytics realizará tracking.
 - Pages. Podemos añadir excepciones a páginas que no queremos que sean trackeadas.
 - Roles. Podemos asignar el seguimiento a un rol o roles en particular.
 - Users. Permite realizar tracking/seguimiento a usuarios.
 - Links and downloads. Permite realizar seguimiento de ficheros o archivos de descargas mediante URLs
 - Messages. Permite realizar seguimiento a los mensajes tipo warning, error o status.
 - Search and Advertising. Permite realizar seguimiento a búsquedas internas y anuncios de Google Adsense.
 - Privacy. Permite ocultar el último octeto de las IP trackeadas. Por defecto está habilitado.

Más abajo disponemos de 3 ajustes más:

 - Custom dimensions y Custom metrics: permite segmentar y medir información específica de nuestra web. [Google lo explica de forma más extendida aquí.](https://developers.google.com/analytics/devguides/collection/analyticsjs/custom-dims-mets)
 - Advanced settings. Son los ajustes más personalizables y avanzados del módulo. También se encuentra la opción de debug.


Así que no lo dudes, saca provecho de esta potente herramienta de Google para conseguir información en bruto de tu página web.
