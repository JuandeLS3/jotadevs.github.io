---
layout: post
title: Instalar Drupal 10 desde Docker4Drupal
description: Descarga e instalación de Drupal 10 en su versión más reciente desde el stack Docker4Drupal en menos de 20 minutos.
categories: code
author: juandels3
---

![small image]({{site.baseurl}}/images/Drupal10_0.jpg)

**¡NOTA IMPORTANTE!**

> Todos los contenidos de este blog los podrás encontrar en Drupal Sapiens ([https://drupalsapiens.com/es](https://drupalsapiens.com)), la nueva plataforma de Divulgación y Cursos de Drupal, ¡con contenidos muy interesantes!


¡Hola! ¡Finalizamos un caluroso y seguro que divertido verano para comenzar el mes de septiembre con mucha energía!

Te doy la bienvenida a este nuevo artículo de mi blog, en el que repasaremos **cómo llevar a cabo la instalación de Drupal 10 en su versión más reciente** a través de docker4drupal en la distribución **Ubuntu** (Linux).

## ¿Por qué Docker4Drupal?

Docker4Drupal es uno (si no el que más) de los stacks de docker más utilizados a la hora de montar infraestructuras Drupal en el ámbito profesional, por varias razones muy concretas:

1. Extremadamente **fácil** de instalar.
2. Es sencillo de **mantener y escalar**, gracias el uso de contenedores de Docker.
3. Tiene un stack completo de librerías y herramientas muy útiles **enfocadas en Drupal**.
4. Tiene la **estructura recomendada** de jerarquía de ficheros y carpetas, además de **composer** y **drush** ya instalados y configurados.

Puedes acceder a la documentación de docker4drupal a través de [aquí](https://wodby.com/docs/1.0/stacks/drupal/local/).

## Requisitos antes de continuar

Debemos cumplir la [verificación de los requisitos](https://wodby.com/docs/1.0/stacks/drupal/local/#requirements) para continuar con la descarga y el uso de docker4drupal, y eso incluye:

1. **Instalar Docker**.
2. **Instalar docker-compose como plugin**.

Para instalar Docker introducimos los siguientes comandos desde nuestra shell, que servirán para actualizar nuestros repositorios locales y añadir la gpg de Docker al mismo:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Y a continuación, procedemos a **instalar Docker**:

```console
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Una vez instalado, verificamos la instalación con el siguiente comando:

```console
sudo docker run hello-world
```

Para finalizar con los requisitos, **instalamos docker compose como plugin** de Docker:

```console
sudo apt-get update
sudo apt-get install docker-compose-plugin
```

Ya podemos verificar la instalación introduciendo:

```console
docker compose version
```

## Preparando contenedores

En primer lugar, tenemos que crear una carpeta en la que se ubicará nuestro proyecto de Drupal 10.
Una vez dentro de ella, clonamos el repositorio de docker4drupal:

```console
git clone https://github.com/wodby/docker4drupal.git .
```

Ahora nos centraremos en modificar un fichero importante: **.env**.

    PROJECT_NAME=drupal10_juandels3
    PROJECT_BASE_URL=juandels3.docker.localhost
    PROJECT_PORT=8000

Es importante utilizar el puerto 8000 (u otro) para no tener conflictos con puertos utilizados por otros servicios, como Apache (80) o Tomcat (8080).
La sección de variables que establecen la conectividad a la base de datos la podemos dejar por defecto.
Es importante que la variable DRUPAL_TAG esté definida así para hacer uso de Drupal 10:

    DRUPAL_TAG=10-[tag]

## Definir codebase

Por defecto docker4drupal utiliza una ruta remota para alojar nuestro código base, de modo que no podemos acceder a él fácilmente desde nuestro editor de código o IDE favorito. Para modificar este comportamiento y tener nuestro codebase a mano, debemos modificar el fichero **docker-compose.override.yml**.
En concreto, tenemos que comentar las líneas de la key "codebase" de los servicios PHP, crond y nginx:

![small image]({{site.baseurl}}/images/drupal10_1.png)

## Levantar contenedores e instalar Drupal

Ya tenemos todo configurado. Ahora es el momento de arrancar nuestros contenedores con el siguiente comando:

```console
docker compose up -d
```

A continuación comenzará la descarga y configuración de nuestros contenedores. Una vez finalizado el proceso, podemos acceder a nuestro sitio web a través del navegador para iniciar el setup de instalación de Drupal 10:

    http://juandels3.docker.localhost:8000

![small image]({{site.baseurl}}/images/drupal10_2.png)

Docker4drupal se encarga de definir mediante el fichero .env las variables de conexión a nuestro base de datos y la ejecución del primer "composer install" que descarga el core de drupal y todas sus dependencias, por lo que seguir el setup de Drupal 10 es muy sencillo y solo tendremos que decidir el perfil de instalación, el idioma y la información básica del sitio.

## Conclusión

Docker permite llevar fácilmente la gestión de los servicios a través de los contenedores, dándonos la posibilidad de prescindir de ellos cuando lo necesitemos, o añadir nuevos si así se requiere.
Este stack en particular, docker4drupal, tiene los servicios y herramientas más importantes y solicitados a la hora de llevar a cabo proyectos con Drupal; todo desde una fácil gestión y lo más importante: una garantía a la escalabilidad y al soporte de un proyecto web con Drupal.

## Curso Completo de Drupal 10: ¡actualízate al máximo nivel!

Si quieres aprender esto y mucho más en formato vídeo, puedes echarle un vistazo al nuevo [**Curso Completo de Drupal 10**](https://www.udemy.com/course/curso-completo-drupal-10/?couponCode=DRUPAL10NOW) con:

- Más de **12h de vídeo** y más de **60** clases.
- Formación desde TODAS las perspectivas (**site building, backend y frontend**)
- Para todos los públicos, sepas o no Drupal, **partimos desde 0 hasta temas más avanzados**.
- Más de **500** alumnos inscritos en mis cursos de Drupal y valoración de **4,5** estrellas.

Puedes mirarlo con un **descuento** desde aquí: [https://www.udemy.com/course/curso-completo-drupal-10/?couponCode=DRUPAL10NOW](https://www.udemy.com/course/curso-completo-drupal-10/?couponCode=DRUPAL10NOW)

¡Muchas gracias, y hasta la próxima!