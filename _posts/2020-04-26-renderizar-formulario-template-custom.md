---
layout: post
title: Formulario en un template custom
description: Cargaremos un form en un bloque a través de un template, para ajustar una estructura html y una funcionalidad por js
categories: code
author: juandels3
---

![small image]({{site.baseurl}}/images/drupal-twig.png)

¡Hola a tod@s! En primer lugar, quiero mandar muchas fuerzas y ánimos a las familias afectadas por el covid-19 y mi deseo de que esta situación pase lo más pronto posible. Por el momento, no queda otra que #QuedarseEnCasa y, para algunas personas, incluso seguir la actividad profesional mediante el teletrabajo, como es mi caso. Hoy me apetecía escribir algo drupalero, por lo tanto os traigo una breve explicación de cómo construir un formulario y mostrarlo en un template twig personalizado en Drupal 8.

En este ejemplo, construiremos un formulario muy básico; se trata de un listado (select) de ciudades, que queremos renderizar en un template para ajustar unos estilos muy particulares y una funcionalidad por js.

Debemos comenzar creando el formulario. Para ello y cómo siempre en Drupal 8, tenemos que partir de una estructura modular, es decir, tenemos que hacer todo esto a partir de la estructura básica de un módulo custom; puedes ver cómo es la estructura de un módulo personalizado [aquí](https://www.drupal.org/docs/8/creating-custom-modules/basic-structure).
El formulario lo ubicaremos en **my_module/src/Form/MyForm.php**

En la clase MyForm.php destacaremos el método buildForm(), donde construiremos el form.

    public function buildForm(array $form, FormStateInterface $form_state) {  
      $form['cities'] = [  
        '#type' => 'select',  
        '#options' => cities[...],  
      ];   
      $form['#attached']['library'][] = 'my_module/cityvalues';  
      
      return parent::buildForm($form, $form_state);  
    }

El fichero js también tenemos que declararlo en nuestro **my_module.libraries.yml** de la siguiente forma:

    cityvalues:
      js:
        js/cityvalues.js: {}
      dependencies:
        - core/jquery
        
El siguiente paso es crear el bloque custom, que se encargará de cargar el formulario previamente creado a través del template.

    public function build() {  
      $form = \Drupal::formBuilder()->getForm('Drupal\my_module\Form\MyForm');
        
      return [  
        '#theme' => 'block_cities',  
        '#city_form' => $form,  
      ];
    }

Cómo podéis observar, primero instanciamos el formulario en la variable *$form*. Después en el array que devolvemos, llamamos al id de la plantilla twig (que será 'cities_template') y le pasaremos en una variable el formulario.
Ahora haremos uso de nuestro my_module.module para usar el hook_theme.

    function my_module_theme() {  
      return [  
        'block_cities' => [  
          'template' => 'block--cities',  
          'variables' => [  
            'city_form' => NULL,  
          ],
        ],
      ];
    }

Este hook se encargará de identificar el id 'block_cities' con el nombre de la plantilla que tiene asignada (block--cities.html.twig), además de setear la variable 'city_form' que va a recibir posteriormente, que contendrá el formulario instanciado.
El último paso es crear la plantilla twig, respetando el directorio **my_module/templates/block--cities.html.twig**

`<div class="cities-form">  
   {{ city_form }}  
</div>`


Y no debemos olvidarnos de que también adjuntamos un js a este form, que ubicaremos en **my_module/js/cityvalues.js**


De esta forma, cada vez que coloquemos el bloque custom en algún sitio de nuestro portal, estaremos llamando un formulario que pasará por un template custom y que carga un js con una funcionalidad concreta. 
Espero que os haya servido y que hayáis aprendido algo nuevo, ¡que eso es lo más importante!

*Rev 0.3*