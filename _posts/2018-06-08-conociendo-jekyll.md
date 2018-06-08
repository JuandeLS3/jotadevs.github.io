---
layout: post
title: Battlefield V no tendrá Premium
description: EA y DICE han decidido dar gratuitamente todos los DLC a los que compren su juego
categories: code
author: juandels3
---

![small image]({{site.baseurl}}/images/jekyll.png)
Buenas a tod@s.

Como ya expliqué [anteriormente](https://juandels3.github.io/hablemos-sobre-big-data/) desarrollé una migración del blog de Wordpress a Github pages con Jekyll. Os debía una explicación de a qué se debió este cambio tan drástico... ¡vamos allá!

## ¿Qué es Jekyll?

Jekyll no es más que un generador de sitios webs escrito en Ruby, creado por Tom Preston-Werner, Co-fundador de Github. Sus características más destacables son la simplicidad y rapidez que tiene, debido a que sus contenidos son estáticos y por tanto no conectan en ningún momento con base de datos. Por esta razón, usar Jekyll en sitios como por ejemplo blogs, es una decisión acertada ya que en este caso, un usuario solo busca publicar contenido estático a la web, y en este caso, Jekyll lo hace genial.

## Por qué usar Jekyll

Cada proyecto es un mundo y por supuesto, tienes tecnologías a mansalva donde elegir para desarrollar tu página web. Jekyll es perfecto para usuarios **con algo de experiencia en desarrollo web** y que busquen ofrecer contenido estático sin uso de base de datos, **consiguiendo así que la carga de páginas sea mucho más rapida**. Al ser desarrollado en ruby, tiene una **gran comunidad detrás y es fácil implantar temas y plugins** para añadir funcionalidades extra que nos ayuden de cara al mantenimiento de nuestro sitio. 

También hay que destacar que Jekyll cuenta con **soporte de Github**, donde nos "incitan" a usar los dominios gratuitos del repositorio de forma gratuita. Esto puede ser una gran ventaja, ya que nos facilita el trabajo a la hora de mantener el sitio y crear las builds, y por supuesto gratis.
![small image]({{site.baseurl}}/images/080618.png)

## Flujo de trabajo

El workflow en Jekyll es simplemente fantástico, ya que separa estructuralmente muy bien configuración y contenido. 
El contenido es fácil crearlo, ya que Jekyll te permite escribirlo en formato Markdown (en mi caso) para después renderizarlo en contenido estático. Por otra parte tienes la configuración, que normalmente es un fichero llamado _config.yml; aquí se configura la información de nuestro sitio web como el nombre, titulo, autor, plantillas usadas, etc... 
Después, dependiendo del proyecto podemos elaborar el diseño con SCSS, Sass o directamente en CSS. Yo me decanté por probar con el primero en mi blog. Por último, solo nos quedaría subir los ficheros a github pages (si esa ha sido tu elección) o por ende en nuestro servidor web, apache, nginx...

## Mi opinión

La migración de mi blog la hice por experimentar Jekyll, y la verdad es que me ha gustado bastante. Al principio tiene una curva media-alta de aprendizaje, pero hay muchas cosas que se dan hechas y para un usuario sin experiencia en coding a lo mejor le puede resultar más fácil publicar sus contenidos en un CMS como Drupal o Wordpress.
Lo cierto es que el cambio en mi blog me ha encantado y de momento y durante bastante tiempo, me quedaré con esa **simplicidad**, **facilidad en el mantenimiento** con Github pages y, sobretodo, la **velocidad** y buen rendimiento que ofrece Jekyll con el contenido estático.