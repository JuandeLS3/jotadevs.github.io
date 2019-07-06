---
layout: post
title: Crear custom field en una vista
description: Trabajaremos en crear un campo personalizado para una vista en Drupal 8.
author: juandels3
categories: code
---

![small image]({{site.baseurl}}/images/drupal8-views.png)

# Crear custom fields en las vistas

En muchos casos concretos, el proyecto en el que trabajamos puede requerirnos a cumplir un caso de uso muy concreto, que una vista de Drupal 8 no es capaz de proveernos mediante la interfaz. Pero como ya sabemos Drupal 8 es flexible y posee una API potente en las vistas que nos permite personalizarlas y adaptar éstas a nuestras necesidades. 
Hoy voy a explicar **cómo crear campos personalizados en las vistas de Drupal 8**.

## Estructura de carpetas

En primer lugar y antes que nada, debemos de tener o crear un módulo custom. Nunca he explicado ni tengo pensado cómo crear un módulo custom en Drupal 8 porque creo que hay suficiente [documentación](https://www.drupal.org/docs/8/creating-custom-modules) al respecto en internet. Aún así, herramientas como Drupal Console, la cual explico cómo descargar e instalar en mi [post](https://juandels3.github.io/utilidades-drupal/), que nos permite crear fácilmente desde la consola un módulo custom a través de varias preguntas secuenciales. También podemos hacerlo manualmente, creando la estructura de carpetas necesaria. 

Nuestra estructura de carpetas para el módulo "mycustom_module" que acabamos de crear ha quedado así:

    ├── mycustom_module
        ├── mycustom_module.info.yml
        ├── mycustom_module.views.inc
        ├── src
        │   └── Plugin
        │       ├── views
        │       │   └── field
        │       │       └── ShowRoles.php

Explicaremos brevemente qué hace cada fichero:

 - mycustom_module.info.yml: recoge información acerca del módulo custom que hemos creado.
 - mycustom_module.views.inc:  es el fichero que se usa en Drupal 8 para lanzar hooks cómo hook_views_data_alter o hook_views_data. En Drupal 7 se hacía directamente en el .module. Puedes encontrar info [aquí](https://www.drupal.org/node/1875596) acerca de este cambio.
 - ShowRoles.php: clase que contiene la lógica que queramos aplicar al campo que se mostrará en la vista. Hereda de FieldPluginBase.

## Creando el campo

Una vez tengamos nuestra estructura de carpetas correctamente, nos centraremos en **ShowRoles.php** en primer lugar y **mycustom_module.views.inc** en segundo.

Así quedaría nuestro ShowRoles.php.

    <?php
    namespace Drupal\ocai_module\Plugin\views\field;
    
    use Drupal\views\Plugin\views\field\FieldPluginBase;
    use Drupal\views\ResultRow;
    use Drupal\user\Entity\User;
    
    /**
     * Field handler to show user's roles.
     *
     * @ingroup views_field_handlers
     *
     * @ViewsField("show_roles")
     */
    class ShowRoles extends FieldPluginBase {
    
      /**
       * @{inheritdoc}
       */
      public function query() {
      }
    
      /**
       * @{inheritdoc}
       */
      public function render(ResultRow $values) {
    
        $user = User::load(\Drupal::currentUser()->id());
        $user_roles = $user->getRoles();
        $output['#markup'] = 'Hey ' . $user->getUsername() . ' your roles are: <br>'
        . implode(', ', $user_roles);
    
        return $output;
      }
    }

Nuestro campo es simple, lo único que hará será mostrar los roles del usuario actual en una ruta que configuraremos luego desde la interfaz de vistas. Lo importante de esta clase es que debe heredar de FieldPluginBase, por tanto se sobreescribirán los métodos render() y query(). Mucho ojo en la anotación @ViewsField("show_roles"), que será donde configuremos el ID de nuestro campo custom.

Así quedaría nuestro mycustom_module.views.inc.

    <?php  
      
    /**  
     * Implements hook_views_data_alter(). {  
      
      $data['node']['show_roles'] = [  
      'title' => t('Show roles to user'),  
      'field' => [  
      'title' => t('Show roles to user'),  
      'help' => t('Show roles to current user'),  
      'id' => 'show_roles',  
      ],  
      ];  
    }

Aquí es importante que coincida correctamente el ID con el que pusimos en ShowRoles.php.
Para terminar, debemos asegurarnos que los ficheros deben tener los permisos correctos y por último ejecutar el comando **drush cr** para borrar cachés. 

## Configurando la vista en la interfaz

Ahora trabajaremos en crear la vista desde la interfaz de administración de Drupal. 
Cuando busquemos en los "Fields" de la vista, si hemos hecho todo correctamente deberá de aparecer nuestro custom field.

![small image]({{site.baseurl}}/images/custom_field_sc_1.png)

Una vez lo encontremos, lo aplicamos a la vista. Configuraremos el path '/view-test' para mostrar el custom field ahí. 
Finalmente el resultado será el siguiente:

![small image]({{site.baseurl}}/images/custom_field_sc_2.png)

