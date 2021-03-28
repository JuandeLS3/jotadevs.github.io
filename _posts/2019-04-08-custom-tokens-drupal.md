---
layout: post
title: Custom tokens en Drupal 8
description: Implementaremos tokens personalizados para renderizar cadenas de texto según nuestras necesidades
author: jotadevs
categories: code
---

![small image]({{site.baseurl}}/images/misildrupalero.png)

¡Hola a tod@s!  
  
Hoy vamos a hablar de los **tokens** de Drupal, qué utilidad tienen y cómo implementarlos correctamente de una forma sencilla en nuestro proyecto Drupal 8.

## ¿Qué son los tokens?

Los tokens son una lista de 'comodines' o fragmentos de texto que nos provee el módulo contribuido [Token](https://www.drupal.org/project/token) que nos solucionan la vida a la hora de mostrar mensajes o rutas dinámicas, aunque también tiene otros casos de uso. Algunos ejemplos de tokens pueden ser:

Bienvenido Jaime, eres el usuario número 134.211 ==> Bienvenido [user-name], eres el usuario número [node.count]

En el caso anterior, podemos encontrar **dos tokens**. Éstos pueden ser facilitados por el mismo [Drupal](https://www.drupal.org/node/390482) o pueden ser personalizados (custom). 

## Tokens personalizados

Por desgracia, Drupal no tendrá accesible siempre todos los tokens, ya que dependen mucho del contexto (la sesión/página actual, el tipo de contenido, vista, etc...)
Es enconces cuando podemos usar los tokens personalizados de forma programática. 

En primer lugar, debemos crearnos un módulo custom (si es que no tenemos ya ninguno) para posteriormente trabajar en el **mymodule.module** de nuestro proyecto.

En este ejemplo, crearemos un token que nos dará el tipo de moneda que tiene seleccionada el usuario actual logueado.
En primer lugar, implementamos el hook token info, que será el que de un nombre, descripción y nombre máquina al token que vamos a crear.

    /**
     * Implements hook_token_info().
     */
    function mymodule_token_info() {
      $info = [];
      $info['types']['custom_token'] = ['name' => t('Custom tokens'), 'description' => t('My module c
      ustom tokens')];
      $info['tokens']['custom_token']['current_user_currency'][] = 'Provides the user's currency';
      return $info;
    }

A continuación, usaremos el hook_tokens para aplicar la lógica deseada. Lo que haremos será hacer una consulta simple a la base de datos para obtener la moneda del usuario actual.

    /**
     * Implements hook_tokens().
     */
    function mymodule_tokens($type, $tokens, array $data, array $options, \Drupal\Core\Render\BubbleableMetadata $bubbleable_metadata) {
      $replacements = [];
    
      if ($type == 'custom_token') {
        foreach ($tokens as $name => $original) {
          // Find the desired token by name.
          switch ($name) {
            case 'current_user_currency':
              $current_user = User::load(\Drupal::currentUser()->id());
              $connection = \Drupal::database();
              $query = $connection->select('users', u);
              $query->condition('u.uid', $current_user->id(), '=');
              $query->fields('u', ['currency']);
              $result = $query->execute() == 'EUR' ? 'EUR' : 'USD';
              $replacements[$original] = $currency;
              break;
          }
        }
      }
      return $replacements;
    }


Una vez borremos cachés (drush cr) ya podemos utilizar este custom token a nuestro gusto.