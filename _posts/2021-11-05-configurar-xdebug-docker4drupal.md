---
layout: post
title: Configurar xDebug en docker4drupal
description: Cómo habilitar y configurar xDebug en docker4drupal
categories: code
author: juandels3
---

![small image]({{site.baseurl}}/images/xdebug_header.png)

# Configurar xDebug en docker4drupal

¡Hola a tod@s!

Hacía ya un tiempo que no escribía nada por aquí, y hoy he pensado que era un buen momento para hacerlo. Porque sí, las cosas a veces no se deben pensar demasiado, ya que entramos en un flujo de procrastinación que no deseamos.

Después de varios eventos personales que han sucedido desde aquel 7 de Junio que escribí mi último artículo, es el momento de hablar de **Drupal**. Porque si estás aquí leyendo no es por amor al arte, ¿verdad?

Estos meses he estado trabajando "a full" con **docker4drupal**, un maravilloso stack de utilidades montadas en docker específicamente para Drupal; muy parecido a **ddev**, otro aclamado stack el cuál he tenido también que usar en algunos proyectos.
Docker4drupal es maravilloso, ya que te permite usar, casi a golpe de clic, una gran variedad de contenedores ya preparados únicamente para ser habilitados, a través del docker-compose.yml.

En este artículo no vamos a extendernos demasiado en qué es y qué ofrece docker4drupal; vamos a ser mucho más específicos. Hablaremos de **cómo habilitar y configurar xDebug en docker4drupal**.

Como ya sabes, xDebug es la extensión PHP que permite al desarrollador depurar código. Es algo indispensable si estás desarrollando en Drupal, por lo que es casi obligatorio tenerlo habilitado y funcionando en nuestro IDE favorito, PHPStorm.

## Configurar el docker-compose.yml

Habilitar xDebug en docker4drupal no tiene ciencia ni muchos más misterios que los descritos en alguna que otra obra de Jesús Relinque, de hecho el equipo de Wodby nos lo ha dejado muy [documentado](https://wodby.com/docs/1.0/stacks/drupal/local/). También hay que aclarar que, desde la versión 3 de xDebug, la cosa en cuanto a configuración ha mejorado muchísimo, y ahora es más fácil que nunca habilitar xDebug, ¡así que no esperes más para actualizar!

El primer paso será configurar nuestro fichero **docker-compose.yml**, que será el **docker-compose.override.yml** si ya tenemos un proyecto Drupal montado y funcionando.
Añadiremos las siguientes directivas de configuración:

    PHP_XDEBUG: 1
    PHP_XDEBUG_MODE: debug
    PHP_XDEBUG_REMOTE_HOST: 127.0.0.1
    PHP_XDEBUG_DEFAULT_ENABLE: 1
    PHP_XDEBUG_START_WITH_REQUEST: "yes"

Después de esto, vamos a recrear los contenedores para que los cambios realizados surjan efecto.

    docker-compose up -d

Ahora comprobaremos desde el php.ini si ya tenemos la extensión de xDebug habilitada. Esto podemos comprobarlo directamente desde nuestro Drupal, en el siguiente path:

    /admin/reports/status/php

![small image]({{site.baseurl}}/images/xdebug_1.png)

## Configurar PhpStorm

Falta un último paso para poder depurar nuestro elegante y organizado código; configurar nuestro IDE, PhpStorm.

Para ello, haremos clic en el botón "Add Configuration", que aparece en la parte superior derecha del IDE.
Después añadiremos una configuración de tipo PHP Web Page.
Ahora procedemos a crear un servidor que escuche las peticiones web; para ello hacemos clic en los 3 puntos "..." y después al "+".

![small image]({{site.baseurl}}/images/xdebug_3.png)

Rellenamos los campos, entre ellos el host (el dns que nos provee docker4drupal para acceder a nuestro site) y el puerto de escucha del site, que normalmente suele ser el 8000 o el 80.
El último paso para tener nuestro server listo, es mapear las rutas, por lo que marcamos tanto el check "User path mappings..." como el de "Shared", y vamos mapeando algunas rutas (no es necesario todas) teniendo en cuenta que la ruta local de nuestro proyecto debe coincidir con la del servidor web que nos ha virtualizado docker con el servicio/container php. Por ejemplo, la ruta **raíz** de nuestro proyecto, sería mapeada normalmente a /var/www/html.

![small image]({{site.baseurl}}/images/xdebug_2.png)

## Últimos pasos y troubleshooting

Sólo nos queda probar que todo funciona (a veces, el paso más duro...)

Desde PhpStorm, colocaremos un punto de ruptura en algún bloque de código el cuál tengamos la certeza que será solicitado al acceder vía web en nuestro site.
Si al arrancar observamos que NO funciona, presta atención a los errores o advertencias de log que nos salen desde el IDE y prueba secuencialmente las soluciones que te propongo:

1. El error más tonto: ¿seguro que el bloque de código el cuál has puesto el/los punto/s de ruptura están siendo solicitados? Te propongo que pongas más puntos de ruptura, en algún hook que sepas que se va a lanzar de un módulo contrib o del mismo core de Drupal.
2. El error más informático: reinicia y tal vez funcione. Ahora en serio, parece una tontería, pero prueba a reiniciar PhpStorm y a parar (make stop) y volver a levantar los contenedores (make up). The caché is real.
3. Lanza comandos drush desde la shell (por ejemplo, un drush cr) y comprueba si te sale algún log de xDebug; si es así, es buena señal.
4. Si ninguna de las anteriores ha funcionado, no es tu día de suerte. Puedes probar a descargar la extensión de xDebug disponible para el navegador, que facilita las cosas.
5. Si has llegado hasta aquí es que posiblemente te has confundido en algún paso, tienes una versión distinta a la que se está explicando en este post, o directamente no eres un verdadero Drupal Jedi. Sigue leyendo la conclusión.

## Conclusión

Sabes que me gusta terminar siempre con una conclusión. Y es que xDebug es curarte en salud y paz mental. Las autoridades sanitarias drupaleras no recomiendan el uso excesivo de print_r() y echo en la depuración de código; se ruega visite a su xDebug para solucionar su vida.

Gracias por llegar hasta aquí y espero haberte ayudado. Si no ha sido así, tengo algo para ti...
Para celebrar que BlackFriday está a la vuelta de la esquina, te dejaré dos códigos promocionales para que obtengas un gran descuento en el Curso Completo de Drupal en español. En este curso, además de mucho Drupal, encontrarás en relación a este post cómo habilitar xDebug en docker4drupal paso a paso **y en formato vídeo**. Y no solo eso, también cómo usar xDebug en un proyecto real junto a otros ejemplos.

- Promo locura de BlackFriday: el curso al menor precio posible: 9,99€, sólo disponible 5 DÍAS, disponible hasta el **11-11-21**:


    JOTADEVSBL4FR121_1

- Promo de BlackFriday para rezagad@s: el curso rebajado al 90%, disponible hasta el **07-12-21**:


    JOTADEVSBL4FR121_2

Sígueme en [Twitter](https://twitter.com/jotadevs), ya que de vez en cuando suelto alguna que otra promo :)
