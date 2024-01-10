---
layout: post
title: Utilidades para Drupal I
description: Analizaremos algunas de las mejores y más usadas herramientas para Drupal
categories: code
author: jotadevs
---



> Todos los contenidos de este blog los podrás encontrar en Drupal Sapiens ([https://drupalsapiens.com/es](https://drupalsapiens.com)), la nueva plataforma de Divulgación y Cursos de Drupal, ¡con contenidos muy interesantes!

¡Buenas a tod@s! Hoy quería hablaros sobre algunas utilidades que he usado para Drupal y considero, a día de hoy, indispensables y muy útiles de cara al desarrollo en el CMS.

## Composer

Composer es como el queso para un ratón, pero ese ratón siendo Drupal. Es una herramienta de gestión de dependencias en PHP. Nos permite declarar librerías que necesitan nuestro proyecto para descargarlas, actualizarlas o eliminarlas de forma muy sencilla. Drupal 8 ya usa composer en el core para gestionar las dependencias, pero es posible usar composer para todo el proyecto como veremos a continuación. En resumen, composer es capaz de descargar las versiones más actuales (o las que le concretemos) de las librerías que le indiquemos, además de sus dependencias.

#### Instalación

-   Para instalar composer lo haremos ejecutando el siguiente comando.

{% highlight bash %}
curl -sS https://getcomposer.org/installer | phpsudo mv composer.phar /usr/local/bin/composer
{% endhighlight %}

-   Si hemos instalado drupal a partir de drupal-project, éste nos vendrá directamente con composer instalado y listo para usar en todo nuestro proyecto.

{% highlight bash %}
composer create-project drupal-composer/drupal-project:8.x-dev some-dir --stability dev --no-interaction
{% endhighlight %}

Drupal-project no es más que un template o plantilla realizada en composer, que realiza la descarga de las librerías y dependencias que usa la versión de Drupal concretada en el comando.

Sinceramente, con lo que llevo usando Drupal veo composer algo esencial a la hora de encararse con un proyecto. Nos proporciona un ahorro de tiempo y de quebraderos de cabeza, evitándonos incompatibilidades de versiones entre librerías y otras librerías de terceros, etc. Con composer nosotros tenemos el control, desde un solo fichero, **composer.json**. Bueno, en realidad son 2 ;)

## Drush

Junto con composer, es de las herramientas que más he usado. Drush es una herramienta de línea de comandos, que nos brinda mantener nuestro bonito sitio drupalero. Es una alternativa rápida a la navegación por la UI, evitándonos el uso lento y continuo del ratón. Tiene una laaaaarga lista de comandos, muy bien documentados para la mayoría de tareas. Además podemos crear nosotros mismos nuestros propios comandos para realizar tareas más complejas.

#### Instalación

-   Por la web oficial: http://docs.drush.org/en/master/install/
-   Mediante composer:

{% highlight bash %}
composer global require drush/drush sudo ln -s ~/.composer/vendor/drush/drush/drush /usr/bin/drush
{% endhighlight %}

-   Si hemos instalado drupal a partir de drupal-project, éste nos vendrá directamente con la herramienta instalada.

#### Ejemplos

-   drush en file_version # Instalamos un módulo
-   drush pmu file_version # Desinstalamos un módulo
-   drush cr # Vaciamos las cachés
-   drush updb # Actualizamos la base de datos
-   drush uli # Obtenemos link de login auto-generado

Drush es esa herramienta que usas tantas veces que si te la quitan, se te caería el mundo encima. Nos ahorra mucho tiempo evitándonos usar UI constantemente.

## Drupal Console

Es otra herramienta de línea de comandos, basada en la consola de [Symfony](http://symfony.es/pagina/que-es-symfony/). La drupal console nos facilita la vida a la hora de realizar ciertas tareas, ya sea generar PHP, YML, así como otros tipos de ficheros, ahorrándonos tiempo de desarrollo. Una de las facilidades que ofrece que más me llamó la atención fue la de creación automática de módulos. Obviamente, no te crea un módulo de la nada (quizás en unos milenios...), pero si te crea toda la ruta de directorios y ficheros, incluyendo el fichero xx_module_name.info.yml. Vale la pena pasarse por su [documentación](https://drupalconsole.com/docs), traducida en múltiples idiomas y echarle un vistazo.

#### Instalación

-   Instalación global con composer

{% highlight bash %}
composer global require drupal/console
{% endhighlight %}


-   Al igual que Drush, con drupal-project viene instalada esta utilidad.

#### Ejemplos

-   drupal -V # Versión de Drupal Console
-   drupal generate:module # Generamos estructura de un módulo
-   drupal database:drop # Elimina todas las tablas de una bd
-   drupal site:maintenance # Cambia a modo en mantenimiento

La drupal console es una herramienta que tiene mucho potencial, aunque si es verdad que yo aún no he sacado ni un 10% de su totalidad. Nos quita tareas pesadas y repetitivas como management en la base de datos.

¡Y con esto acabamos por hoy! Próximamente haré más review sobre herramientas que vaya probando para ir comentándolas.
