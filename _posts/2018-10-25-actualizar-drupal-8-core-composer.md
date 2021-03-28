---
layout: post
title: Actualizar core de Drupal 8 con composer
description: Actualizaremos el core de Drupal 8 con composer paso a paso de forma sencilla
author: jotadevs
categories: code
---

![small image]({{site.baseurl}}/images/drupal_logo.png)

Buenas tardes a tod@s. Hoy vamos a llevar a cabo el procedimiento de una actualización del **core de Drupal 8**. 
Se han puesto muchos esfuerzos para que este proceso hoy en día sea fácil y no cause demasiados problemas.

Antes de empezar a actualizar tu Drupal, es importante saber el ciclo de actualizaciones que se lanzan.

 - **Las versiones de parches o patch versions** (8.0.1, 8.0.2, 8.0.3, etc.) se lanzarán cada mes, solucionando errores reportados y brechas de seguridad. Se ha dado casos en los que estas actualizaciones salen antes de tiempo por ser errores graves.
 - **Las versiones menores o minor versions** (8.1.0, 8.2.0, etc.) se lanzarán aproximadamente cada seis meses e incorporarán nuevas características y hotfixes.
 - **Las versiones a largo plazo o LTS** (8.0.0, 9.0.0) son versiones que son lanzadas como bien dice el nombre, para dar un soporte a largo plazo. Drupal 8 se lanzó el 19 de Noviembre de 2015, mientras que la próxima versión LTS será Drupal 9 y se lanzará en el 2020.

Una vez aprendido el calendario drupalero, podemos pasar a la acción para actualizar nuestro sitio web. Recordar que es necesario tener instalado **composer** para gestionar las versiones de la paquetería y **drush** para realizar las tareas de administración de una forma más rápida directamente desde consola. Desde [mi post te explico como instalar](https://juandels3.github.io/utilidades-drupal/) ambas valiosas utilidades. Existen otros métodos de actualizar Drupal, pero este es el más recomendable a día de hoy.

## Actualizando nuestro Drupal 8

En primer lugar, debemos comprobar si hay actualizaciones disponibles, esto se puede hacer desde la interfaz de administración de nuestro portal (status report) o desde la consola con el siguiente comando:

    composer outdated drupal/*

Una vez realizado y tras unos instantes de espera, nos imprimirá una tabla con la paquetería que requiere actualizarse, indicando la versión actual y la más reciente disponible. Si nos aparece el core, es necesario actualizarlo. También es importante echarle un ojo a nuestro composer.json, para verificar nuestra versión actual de Drupal y saber si nuestra actualización será a una patch version o a una minor version. Si por ejemplo queremos actualizar de la 8.5.0 a la 8.6.0 es necesario reemplazar en el composer.json la versión del core de "~8.5.x" a "^8.6"

Para comenzar, haremos un dump de la base de datos como medida de seguridad por si algo va mal (más vale prevenir...)

    drush sql-dump backup.sql

El siguiente paso será leer las notas de la versión lanzada, ya que siempre puede darse casos en los que algunos módulos hayan sufrido cambios o se recomiende actuar de alguna manera en la actualización. Nunca viene mal estar informados de por qué vamos a actualizar nuestro Drupal.

Según las recomendaciones de Drupal, en el siguiente paso se recomienda poner nuestro sitio web en mantenimiento. Esto puede hacerse directamente desde la interfaz de administración o con los siguientes comandos:

    drush sset system.maintenance_mode 1
    drush cr

Y ahora vamos al **comando más importante**, donde llevaremos a cabo la actualización del core de Drupal mediante composer, incluyendo sus dependencias (normalmente componentes symfony, entre otros)

    composer update drupal/core --with-dependencies

Si el comando no nos funciona, puede deberse a que nuestro Drupal fue montado con composer/drupal-project, donde la orden cambiará un poco:

    composer update drupal/core webflo/drupal-core-require-dev --with-dependencies

Por último, actualizaremos la base de datos (si hubiera actualizaciones) para que esté acorde a los cambios implementados en la paquetería.

    drush updatedb
    drush cr

Para acabar solo tendremos que poner nuestro sitio activo desactivando el modo mantenimiento desde la interfaz de administración o con los comandos:

    drush sset system.maintenance_mode 0
    drush cr

Con esto se terminaría el proceso de actualización de nuestro Drupal. Una vez mecanicemos este proceso, se nos hará mucho más ameno y rápido. Antes de terminar, es recomendable comprobar el status report desde nuestra interfaz de administración para comprobar que todo ha ido correctamente.
