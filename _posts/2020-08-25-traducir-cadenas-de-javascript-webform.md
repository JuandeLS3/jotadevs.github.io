---
layout: post
title: Traducir cadenas en JS desde Webform
description: Soluciones frente a la incapacidad de Webform para traducir cadenas directamente desde javascript.
categories: code
author: juandels3
---

![small image]({{site.baseurl}}/images/javascript.jpg)

# Traducir cadenas en JS desde Webform

¡Hola a tod@s! Después de tomarme un periodo de vacaciones blogueras, ¡es hora de volver a la acción!
En este artículo quiero contaros la problemática que encontramos con Webform durante el desarrollo de un proyecto en Drupal 8, y por supuesto, la solución "temporal".

Todo esto sucede, como ya os he contado, en el módulo contrib **Webform**, y más específicamente en el apartado **CSS / JS**, dentro de la configuración de un formulario.
Resulta que a la hora de configurar el Javascript de un Webform dentro de este apartado, si incluyes cadenas para luego traducirlas no bastará con un simple *"Drupal->t()"*, ya que el motor de traducción de Drupal no encontrará tus cadenas traducibles, porque están almacenadas en el fichero de configuración de Webform.

Pero entonces, **¿cómo traducimos las cadenas dentro de este javascript?**

En primer lugar tenemos que analizar las posibles soluciones y cual se adapta mejor a nuestro caso.

1. Obviar las traducciones: no se si esto es una solución del todo, pero si no es realmente necesario traducir las cadenas, puedes obviar la traducción de éstas y fuera problemas.
2. No usar webform: quizás esta sea la solución más interesante si necesitas traducir cadenas a través de Javascript. Para ello deberías replantearte el uso del módulo Webform y si es más sencillo crear un módulo custom, con su formulario y su javascript, que se cargará en el libraries.yml y, donde en este caso, no tendrás ningún problema en traducir cadenas desde la interfaz de traducciones.
3. Algo más... "temporal": Si necesitas sí o sí el uso de Webform porque tu proyecto gestiona muchos formularios a través de este módulo, puedes solucionarlo como te mostraré a continuación.

Resulta que en el javascript de Webform disponemos de la variable drupalSettings, que será nuestra salvadora. Esta variable contiene objetos y propiedades que son default de Drupal, otras que suministran módulos contribuidos, y por supuesto los que pasemos nosotros en los módulos custom. La solución está en el objeto **path** de la variable drupalSettings, que siempre viene por defecto.

    var Currentlanguage = drupalSettings.path.currentLanguage;

A través de la propiedad "currentLanguage" podemos obtener el idioma a través de la url, de modo que podemos saber el idioma en el que el usuario visualiza el site en el momento de renderizar el webform.
Con esto en nuestras manos, simplemente podemos elegir (siempre dependiendo de nuestro caso concreto) un abanico de posibilidades y condicionales para traducir la cadena.

(I) Si tenemos solo dos idiomas y el de por defecto es inglés, podemos aplicar un condicional simple.

    var message;
    if ( Currentlanguage == 'pt' ){
     message = 'Obrigado por contactar';
    } else {
     message = 'Thank you for contacting';
    }

(II) Si tenemos varios idiomas, podemos optar por una estructura switch.

    switch ( Currentlanguage ) {
      case 'pt':
        message = 'Obrigado por contactar';
        break;
      case 'fr':
        message = 'Gracias par contactar';
        break;
      case 'es':
        message = 'Gracias por contactar';
        break;
      default:
        // 'en' is default language.
        message = 'Thank you for contacting';  
        break;
    }

No hay que olvidar que el javascript tiene que recogerse usando los Drupal.behaviours y no los ready, para poder acceder a las variables drupalSettings, Drupal... (tal como se muestra en la imagen siguiente)

![small image]({{site.baseurl}}/images/js_webform_example.png)

Quizás no sea la mejor solución, o la más elegante, pero es una válida hasta que Webform solucione este error, en la traducción de cadenas.
Te invito a seguir esta issue por si salen soluciones mejores a las que te propongo: https://www.drupal.org/project/webform/issues/3167153

¡Un abrazo!




