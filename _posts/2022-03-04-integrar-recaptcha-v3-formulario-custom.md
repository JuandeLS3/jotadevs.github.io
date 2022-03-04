---
layout: post
title: Integrar reCAPTCHA v3 en un formulario custom
description: Adiós a los bots y agencias publicitarias engañosas.
categories: code
author: juandels3
---

![small image]({{site.baseurl}}/captcha/captcha.png)

# Integrar reCAPTCHA v3 en un formulario custom

¡Hola querid@ drupaler@!

Pues resulta que entre tanta mudanza, bricolaje y [juegos hechos en Unity](https://juandels3.github.io/publico-mi-videojuego/) he recordado que tenía un blog Drupal. ¿Será que mis venas chorrean azul por el anuncio de la DrupalCamp 2022 en Zaragoza? La respuesta es... tal vez.
La cosa es que hoy quería escribir un poco acerca de la integración en un formulario custom Drupal de Captcha v3, que es la siguiente versión a la 2; ¡qué ingeniosos son en Silicon Valley!

## ¿Qué es reCAPTCHA v3?

Captcha es un "validador" o un "anti-spammer" que diferencia a los humanos de las máquinas, en los procesos en los que se requiere la entrada de datos; normalmente formularios.

La versión v3 de reCAPTCHA valida la entrada de datos según una puntuación obtenida, dependiendo de cómo se hayan introducido los datos. Si esa puntuación supera un umbral (que se configura), el usuario puede seguir adelante, sino, será sometido a un "Challenge" o prueba para certificar que es humano.

## Instalar e integrar

Para disponer de la API de Captcha v3, necesitamos:

- Conexión a internet.
- Una cuenta de Google.
- Vender nuestra alma (datos) a Google.

Si cumplimos los 3 requisitos, podemos conseguir una API Key de Captcha en el siguiente enlace: [https://www.google.com/recaptcha/admin/create](https://www.google.com/recaptcha/admin/create)

El formulario es muy sencillo, y lo más importante es seleccionar reCAPTCHA v3 e indicar el/los dominio/s en los que desees usar Captcha.

![small image]({{site.baseurl}}/captcha/captcha_1.png)

Una vez hecho, Google nos dará la CLAVE DEL SITIO WEB y la CLAVE SECRETA, como puedes ver en la imagen.

![small image]({{site.baseurl}}/captcha/captcha_2.png)

FYI: no es la pantalla de tu PC ni tu navegador, he subrayado en rojo para que no me roben mis keys.
Bueno y ahora... ¡volvamos a Drupal...! Para este caso en particular me he tomado el placer de instalar un Drupal 9.3.6 con docker4drupal -en este blog no divulgamos por divulgar, PROBAMOS y CERTIFICAMOS lo que se escribe, faltaría más-

![small image]({{site.baseurl}}/captcha/captcha_3.png)

Para la integración de reCAPTCHA v3 (ya lo escribo bien, ¿vale?) en Drupal, usaremos el módulo contribuido [reCAPTCHA v3](https://www.drupal.org/project/recaptcha_v3), que a su vez tiene dependencia de [captcha](https://www.drupal.org/project/captcha), y que a su vez éste tiene dependencia de [recaptcha](https://www.drupal.org/project/recaptcha). En fin... solo tómate un minuto para imaginar un mundo sombrío sin [composer](https://getcomposer.org/).

Ahora nos dirigimos a la configuración del módulo, y pegamos las credenciales que Google nos ha facilitado para reCAPTCHA v3.

![small image]({{site.baseurl}}/captcha/captcha_5.png)

El siguiente paso es crear una "Acción", que será la medida que el anti-spam de Google tomará en caso de que la puntuación obtenida por la persona/bot que rellene nuestro formulario, no supere la media que nosotros digamos (Theshold).

![small image]({{site.baseurl}}/captcha/captcha_9.png)

Y ahora finalizamos con el plato fuerte: ¿cómo integrar reCAPTCHA v3 en un formulario custom? Vamos allá...

Nos movemos al tab "Form settings", y posiblemente, muy posiblemente de hecho, NO aparezca nuestro formulario custom en el listado, porque sí, porque Drupal es así de majo.
Por suerte para nosotros tenemos un botón azul que nos deja añadir formularios custom mediante su Form ID, y es aquí donde surge una rotura espacio-temporal de dudas, problemas y preguntas, al no escribir el FORM ID que se requiere para localizar el formulario.
Localizar este form id es muy sencillo, sólo tenemos que abrir nuestra consola o inspector de elementos desde nuestro navegador favorito, y buscar entre los elementos del formulario un campo hidden llamado "form_id".

![small image]({{site.baseurl}}/captcha/captcha_8.png)

El resto ya sabes cómo va, copiamos y pegamos en nuestro nuevo "CAPTCHA point". Aquí es importante seleccionar como "Challenge type" la acción que previamente hemos creado.

![small image]({{site.baseurl}}/captcha/captcha_10.png)

Y así de fácil y rápido, tenemos el anti-spammer más odiado por los bots y agencias de publicidad engañosas configurado correctamente en nuestro formulario custom. Podemos darnos cuenta de esto al ver en la esquina inferior derecha el logo de reCAPTCHA, o simplemente buscando en la consola del navegador el nombre de la librería.

![small image]({{site.baseurl}}/captcha/captcha_11.png)

Si estás implementando reCAPTCHA v3 en un dominio que no está en la whitelist que hemos configurado en los primeros pasos, te saldrá el logo pero con un mensaje en rojo indicándolo.

Antes de acabar, quiero dar todas mis fuerzas y admiración al pueblo ucraniano, y expresar mi mayor desagrado y repulsión a la invasión rusa. Es muy triste ver cómo volvemos a recaer en nuestros errores del pasado, pero mientras siga habiendo dictadores y criminales como Putin (que lamentablemente los hay, y muchos) peligrará la paz y por ende, nuestra superviviencia. NO A LA GUERRA - STOP WAR.
