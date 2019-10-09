---
layout: post
title: Trabajando con el cron de Drupal 8 
description: Toca hablar del cron de Drupal 8: qué es lo que hace y qué puede llegar a conseguirse con él
author: juandels3
categories: code
---

![small image]({{site.baseurl}}/images/cron-drupal.jpeg)

# Trabajando con el Cron de Drupal 8

¡Hola a tod@s! Pido disculpas por mi ausencia estos meses, pero he estado en una etapa de novedades y cambios y necesitaba un tiempo para asentarlo y volver más fuerte. 

Cada día nos despertamos por la mañana, desayunamos, nos vestimos, lavamos los dientes, peinamos... y bueno, hacemos todas esas tareas cotidianas para empezar el día. Si no hacemos estas cosas sentimos que nos falta algo. El cron de Drupal 8 funciona de una forma similar. 

## ¿Qué es?

El término 'cron' se refiere a las tareas automatizadas que un portal Drupal 8 ejecuta cada cierto periodo de tiempo o cuando el administrador desea, el tiempo es totalmente configurable. Se encarga, entre otras cosas, de verificar si hay actualizaciones disponibles para el núcleo de Drupal, para los módulos y temas contribuidos, indexar las búsquedas del portal, borrar cachés internas,etc. Es nada más y nada menos que una tarea automatizada que se encarga del mantenimiento del portal, pero su funcionalidad puede extenderse hasta el infinito y mas allá...

## El cron a nivel de interfaz

Por defecto, el cron se ejecuta cada tres horas, pero ésto puede configurarse e incluso lanzarse manualmente desde la propia interfaz en **Configuración > Sistema > Cron**. Siempre es recomendable echar un vistazo al 'Reporte de estado' desde **Reportes > Reporte de estado** cada vez que finalize la ejecución del cron para comprobar si ha encontrado nuevas actualizaciones o problemas. Cualquier usuario con los permisos adecuados puede ejecutar el cron desde la interfaz de administración.

Es posible que alguna que otra vez recibamos un error al lanzar el cron donde éste se bloquea y lanza una excepción indicando de que no ha llegado a efectuarse. Esto ocurre cuando por ejemplo se lanza el cron desde distintos usuarios o de forma reiterativa. Para desbloquearlo debemos ejecutar una query en la base de datos o directamente hacerlo desde drush:

```
drush sqlq "DELETE FROM semaphore WHERE name = 'cron';"
```

## El cron a nivel de código

Las posibilidades de 'jugar' con el cron son extensas. El cron no solo se encarga de las tareas de mantenimiento arriba mencionadas, también puede encargarse de procesos personalizados que se definan en el código. Para ello, la API de Drupal 8 nos proporciona el hook_cron, que nos ofrece añadir funcionalidades durante la ejecución del cron, permitiendo al desarrollador implementar múltiples tareas o mantenimientos custom al portal, independientemente de los que hace Drupal.
Es importante destacar que el cron **siempre va a suponer un costo temporal de rendimiento** en el portal, ya que el servidor web estará ocupado en el mantenimiento de éste y no podrá servir otras peticiones durante el proceso. Por lo tanto, habría que estudiar si afectan excesivamente el performance del sitio implementaciones como envío masivo de correos, tareas en ficheros o usuarios, etc.

A nivel de código también es posible ejecutar el cron:

```
\Drupal::lock()->release('cron');
```

Imaginemos que nuestro cliente nos pide que se deben encriptar los datos de los usuarios que lleven X tiempo inactivos en el portal debido a la ley de protección de datos. Para conseguir esto, no nos queda otra que comprobar periódicamente el último acceso de todos los usuarios del portal y encriptar los datos si se cumplen las condiciones de tiempo. Las condiciones de tiempo las regula el administrador del sitio desde la configuración en la interfaz, donde regulará el tiempo límite que un usuario no debe alcanzar para que se encripten sus datos.

![small image]({{site.baseurl}}/images/cron-drupal-img1.png)


    /**
     * Implements hook_cron().
     */
    function mymodule_module_cron() {
      $connection = \Drupal::database();
      $current_day = date('d', \Drupal::time()->getCurrentTime());
      $type = $current_day % 2 ? 1 : 0;
    
      $config = \Drupal::service('config.factory')->get('encryption_settings_form.settings');
      if ($type == 0) {
	    // Encriptar datos de usuarios con roles 'Pros' y 'Experts'.
      } else {
        // Encriptar datos de usuarios con roles 'Noobies'.
      }
    }


Tal y como vemos en el código anterior, vamos a dividir la encriptación de usuarios dependiendo de si el día actual es par o impar. Esto hará que la comprobación de todos los usuarios sobrecargue lo menos posible al servidor. Dejaremos los días pares para encriptar los usuarios con roles 'Pros y 'Experts' y los días impares para los usuarios 'Noobies'. Esto se hace porque los usuarios con roles 'Pros' y 'Experts' serán usuarios que previamente han pagado o contratado algún servicio, mientras que los usuarios 'Noobies' serán los que no han pagado y están usando algún servicio gratuito de nuestro portal, por lo que habrá más usuarios de éste último rol.
Estamos cargando y guardando la configuración de la interfaz en $config. Finalmente quedaría algo así:


    /**
     * Implements hook_cron().
     */
    function ocai_module_cron() {
      $connection = \Drupal::database();
      $current_day = date('d', \Drupal::time()->getCurrentTime());
      $type = $current_day % 2 ? 1 : 0;
    
      $config = \Drupal::service('config.factory')->get('encryption_settings_form.settings');
      if ($type == 0) {
      	// Encriptar datos de usuarios con roles 'Pros' y 'Experts'.
      	// Capturamos la configuración, si no hay nada, seteamos por defecto a 365 días (1 año)
        $days = empty($config->get('pros_experts_time_encrypt')) ? '365' : $config->get('pros_experts_time_encrypt');
        $current_date = date('m/d/Y', time());
        $final_date_timestamp = strtotime(date('Y-m-d', strtotime(
          '-' . $days . ' days', strtotime($current_date))));
        $users = \Drupal::entityQuery('user')
          ->condition('roles', ['pros', 'experts'], 'IN')
          ->condition('mail', '@', 'CONTAINS')
          ->condition('access', $final_date_timestamp, '<')
          ->execute();
    
        foreach ($users as $uid) {
          $account = User::load($uid);
          encryptUserData($connection, $account);
        }
      } else {
        // Encriptar datos de usuarios con roles 'Noobies'.
        // Capturamos la configuración, si no hay nada, seteamos por defecto a 365 días (1 año)
        $days = empty($config->get('noobies_time_encrypt')) ? '365' : $config->get('noobies_time_encrypt');
        $current_date = date('m/d/Y', time());
        $final_date_timestamp = strtotime(date('Y-m-d', strtotime(
          '-' . $days . ' days', strtotime($current_date))));
        $users = \Drupal::entityQuery('user')
          ->condition('roles', 'noobies', '=')
          ->condition('mail', '@', 'CONTAINS')
          ->condition('access', $final_date_timestamp, '<')
          ->execute();
        foreach ($users as $uid) {
          $account = User::load($uid);
          encryptUserData($connection, $account);
        }
      }
    }

En resumen, estamos lanzando el cron cada día y se encarga de comprobar periódicamente qué usuarios tienen un último acceso mayor al indicado en configuración (véase imagen anterior). En caso de que se supere el tiempo límite, se encriptarán los datos del usuario. Esto es una de las muchas cosas que puede hacerse con el cron de Drupal...

## Conclusión

El cron de Drupal es esencial para realizar ciertas tareas periódicas en nuestro sitio web. Además gracias a la API de Drupal 8 podemos crear implementaciones muy interesantes y extendidas, eso sí, sin olvidar el coste del rendimiento al servidor mientras se ejecuta.
